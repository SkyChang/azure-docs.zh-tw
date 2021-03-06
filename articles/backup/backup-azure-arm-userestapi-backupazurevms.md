---
title: 使用 REST API 備份 Azure Vm
description: 在本文中，您將瞭解如何使用 REST API 來設定、啟動及管理 Azure VM 備份的備份作業。
ms.topic: conceptual
ms.date: 08/03/2018
ms.assetid: b80b3a41-87bf-49ca-8ef2-68e43c04c1a3
ms.openlocfilehash: 42af6ae69699be7eefac0aca2bcd22b1e25720b2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "89506622"
---
# <a name="back-up-an-azure-vm-using-azure-backup-via-rest-api"></a>透過 REST API 使用 Azure 備份來備份 Azure VM

本文將說明如何透過 REST API，使用 Azure 備份來管理 Azure VM 的備份。 第一次為先前未受保護的 Azure VM 設定保護、針對受保護的 Azure VM 觸發隨選備份，然後透過 REST API 修改備份 VM 的備份屬性，如下所述。

請參閱[建立保存庫](backup-azure-arm-userestapi-createorupdatevault.md)和[建立原則](backup-azure-arm-userestapi-createorupdatepolicy.md) REST API 教學課程來建立新的保存庫和原則。

假設您想要使用預設原則 (名為 "DefaultPolicy")，將 "testRG" 資源群組下方的 VM "testVM" 保護到復原服務保存庫 "testVault" (存在於資源群組 "testVaultRG" 內)。

## <a name="configure-backup-for-an-unprotected-azure-vm-using-rest-api"></a>使用 REST API 為未受保護的 Azure VM 設定備份

### <a name="discover-unprotected-azure-vms"></a>探索未受保護的 Azure VM

首先，保存庫應該能夠識別 Azure VM。 這會使用[重新整理作業](/rest/api/backup/protectioncontainers/refresh) \(英文\) 來觸發。 這是非同步 *POST*  作業，可確保保存庫取得目前訂用帳戶中所有未受保護 VM 的最新清單，並「快取」。 一旦「快取」VM 之後，復原服務將能存取該 VM 並保護它。

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupname}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/refreshContainers?api-version=2016-12-01
```

POST URI 具有 `{subscriptionId}`、`{vaultName}`、`{vaultresourceGroupName}`、`{fabricName}` 參數。 `{fabricName}` 為 "Azure"。 根據我們的範例， `{vaultName}` 是 "testVault"，而且 `{vaultresourceGroupName}` 是 "testVaultRG"。 因為 URI 中提供所有必要的參數，所以不需要個別的要求主體。

```http
POST https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/refreshContainers?api-version=2016-12-01
```

#### <a name="responses-to-refresh-operation"></a>重新整理作業的回應

「重新整理」作業為[非同步作業](../azure-resource-manager/management/async-operations.md)。 這表示此作業會建立另一項需要個別追蹤的作業。

它會傳回兩個回應：在建立另一項作業時傳回 202 (已接受)，然後在該作業完成時傳回 200 (確定)。

|名稱  |類型  |描述  |
|---------|---------|---------|
|204 沒有內容     |         |  確定，但不會傳回任何內容      |
|202 已接受     |         |     已接受    |

##### <a name="example-responses-to-refresh-operation"></a>重新整理作業的範例回應

一旦提交 *POST* 要求之後，就會傳回 202 (已接受) 回應。

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
X-Content-Type-Options: nosniff
x-ms-request-id: 43cf550d-e463-421c-8922-37e4766db27d
x-ms-client-request-id: 4910609f-bb9b-4c23-8527-eb6fa2d3253f; 4910609f-bb9b-4c23-8527-eb6fa2d3253f
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 43cf550d-e463-421c-8922-37e4766db27d
x-ms-routing-request-id: SOUTHINDIA:20180521T105701Z:43cf550d-e463-421c-8922-37e4766db27d
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:57:00 GMT
Location: https://management.azure.com/subscriptions//00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/operationResults/aad204aa-a5cf-4be2-a7db-a224819e5890?api-version=2019-05-13
X-Powered-By: ASP.NET
```

使用 "Location" 標頭搭配簡單的 *GET* 命令，來追蹤所產生的作業。

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/operationResults/aad204aa-a5cf-4be2-a7db-a224819e5890?api-version=2019-05-13
```

探索過所有 Azure VM 之後，GET 命令就會傳回 204 (沒有內容) 回應。 保存庫現在能夠探索訂用帳戶內的任何 VM。

```http
HTTP/1.1 204 NoContent
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: cf6cd73b-9189-4942-a61d-878fcf76b1c1
x-ms-client-request-id: 25bb6345-f9fc-4406-be1a-dc6db0eefafe; 25bb6345-f9fc-4406-be1a-dc6db0eefafe
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14997
x-ms-correlation-request-id: cf6cd73b-9189-4942-a61d-878fcf76b1c1
x-ms-routing-request-id: SOUTHINDIA:20180521T105825Z:cf6cd73b-9189-4942-a61d-878fcf76b1c1
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:58:25 GMT
X-Powered-By: ASP.NET
```

### <a name="selecting-the-relevant-azure-vm"></a>選取相關的 Azure VM

 您可以藉由在訂用帳戶下方[列出所有可保護的項目](/rest/api/backup/backupprotectableitems/list) \(英文\) 來確認「快取」已完成，並在回應中找出所需的 VM。 [這項作業的回應](#example-responses-to-get-operation) 也會提供復原服務如何識別 VM 的資訊。  一旦您熟悉此模式之後，就可略過此步驟，並直接前往[啟用保護](#enabling-protection-for-the-azure-vm)。

這項作業為 *GET* 作業。

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupProtectableItems?api-version=2016-12-01&$filter=backupManagementType eq 'AzureIaasVM'
```

*GET* URI 具備所有必要參數。 不需任何額外的要求本文。

#### <a name="responses-to-get-operation"></a>取得作業的回應

|名稱  |類型  |描述  |
|---------|---------|---------|
|200 確定     | [WorkloadProtectableItemResourceList](/rest/api/backup/backupprotectableitems/list#workloadprotectableitemresourcelist)        |       確定 |

#### <a name="example-responses-to-get-operation"></a>取得作業的範例回應

一旦提交 *GET* 要求之後，就會傳回 200 (確定) 回應。

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: 7c2cf56a-e6be-4345-96df-c27ed849fe36
x-ms-client-request-id: 40c8601a-c217-4c68-87da-01db8dac93dd; 40c8601a-c217-4c68-87da-01db8dac93dd
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14979
x-ms-correlation-request-id: 7c2cf56a-e6be-4345-96df-c27ed849fe36
x-ms-routing-request-id: SOUTHINDIA:20180521T071408Z:7c2cf56a-e6be-4345-96df-c27ed849fe36
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:14:08 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainerv2;testRG;testVM/protectableItems/vm;iaasvmcontainerv2;testRG;testVM",
      "name": "iaasvmcontainerv2;testRG;testVM",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectableItems",
      "properties": {
        "virtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
        "virtualMachineVersion": "Compute",
        "resourceGroup": "testRG",
        "backupManagementType": "AzureIaasVM",
        "protectableItemType": "Microsoft.Compute/virtualMachines",
        "friendlyName": "testVM",
        "protectionState": "NotProtected"
      }
    },……………..

```

> [!TIP]
> *GET* 回應中針對「頁面」的值數字僅限於 200。 使用 'nextLink' 欄位來取得下一組回應的 URL。

回應包含所有未受保護的 Azure VM 清單，每個 `{value}` 均包含 Azure 復原服務設定備份所需的所有資訊。 若要設定備份，請注意 `{properties}` 區段中的 `{name}` 欄位與 `{virtualMachineId}` 欄位。 從這些欄位值建構兩個變數，如下所述。

- containerName = "iaasvmcontainer;"+`{name}`
- protectedItemName = "vm;"+ `{name}`
- `{virtualMachineId}` 稍後會在[要求本文](#example-request-body)中使用

在此範例中，上述值會轉譯為：

- containerName = "iaasvmcontainer;iaasvmcontainerv2;testRG;testVM"
- protectedItemName = "vm;iaasvmcontainerv2;testRG;testVM"

### <a name="enabling-protection-for-the-azure-vm"></a>啟用 Azure VM 的保護

在「快取」並「識別出」相關的 VM 之後，選取要保護的原則。 若要深入了解保存庫中的現有原則，請參閱[清單原則 API](/rest/api/backup/backuppolicies/list) \(英文\)。 然後藉由參考原則名稱來選取[相關的原則](/rest/api/backup/protectionpolicies/get) \(英文\)。 若要建立原則，請參閱[建立原則教學課程](backup-azure-arm-userestapi-createorupdatepolicy.md)。 下列範例中選取了 "DefaultPolicy"。

啟用保護是建立「受保護的項目」的非同步 *PUT* 作業。

```http
https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2019-05-13
```

`{containerName}` 和 `{protectedItemName}` 如上述所建構的。 `{fabricName}` 為 "Azure"。 針對我們的範例，這會轉譯為：

```http
PUT https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM?api-version=2019-05-13
```

#### <a name="create-the-request-body"></a>建立要求本文

若要建立受保護的項目，以下是要求本文的元件。

|名稱  |類型  |描述  |
|---------|---------|---------|
|properties     | AzureIaaSVMProtectedItem        |ProtectedItem 資源屬性         |

如需要求本文的完整定義清單及其他詳細資訊，請參閱[建立受保護的項目 REST API 文件](/rest/api/backup/protecteditems/createorupdate#request-body) \(英文\)。

##### <a name="example-request-body"></a>要求本文範例

下列要求本文會定義建立受保護的項目所需的屬性。

```json
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/DefaultPolicy"
  }
}
```

`{sourceResourceId}` 是上述來自[清單可保護項目之回應](#example-responses-to-get-operation)的 `{virtualMachineId}`。

#### <a name="responses-to-create-protected-item-operation"></a>建立受保護專案作業的回應

受保護項目的建立是[非同步作業](../azure-resource-manager/management/async-operations.md)。 這表示此作業會建立另一項需要個別追蹤的作業。

它會傳回兩個回應：在建立另一項作業時傳回 202 (已接受)，然後在該作業完成時傳回 200 (確定)。

|名稱  |類型  |描述  |
|---------|---------|---------|
|200 確定     |    [ProtectedItemResource](/rest/api/backup/protecteditemoperationresults/get#protecteditemresource)     |  確定       |
|202 已接受     |         |     已接受    |

##### <a name="example-responses-to-create-protected-item-operation"></a>建立受保護專案作業的範例回應

一旦提交 *PUT* 要求以建立或更新受保護的項目之後，初始回應會是 202 (已接受) 並具備位置標頭或 Azure-async-header。

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2019-05-13
X-Content-Type-Options: nosniff
x-ms-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-client-request-id: e1f94eef-9b2d-45c4-85b8-151e12b07d03; e1f94eef-9b2d-45c4-85b8-151e12b07d03
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-routing-request-id: SOUTHINDIA:20180521T073907Z:db785be0-bb20-4598-bc9f-70c9428b170b
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:39:06 GMT
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2019-05-13
X-Powered-By: ASP.NET
```

然後，使用位置標頭或 Azure-AsyncOperation 標頭搭配簡單的 *GET* 命令，來追蹤所產生的作業。

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2019-05-13
```

當作業完成時，它會在回應本文中傳回 200 (確定) 以及受保護的項目內容。

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM",
  "name": "VM;testRG;testVM",
  "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems",
  "properties": {
    "friendlyName": "testVM",
    "virtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "protectionStatus": "Healthy",
    "protectionState": "IRPending",
    "healthStatus": "Passed",
    "lastBackupStatus": "",
    "lastBackupTime": "2001-01-01T00:00:00Z",
    "protectedItemDataId": "17592691116891",
    "extendedInfo": {
      "recoveryPointCount": 0,
      "policyInconsistent": false
    },
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "backupManagementType": "AzureIaasVM",
    "workloadType": "VM",
    "containerName": "iaasvmcontainerv2;testRG;testVM",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/DefaultPolicy",
    "policyName": "DefaultPolicy"
  }
}
```

這會確認已為 VM 啟用保護，並根據原則排程觸發第一個備份。

### <a name="excluding-disks-in-azure-vm-backup"></a>排除 Azure VM 備份中的磁片

Azure 備份也提供在 Azure VM 中選擇性備份磁片子集的方法。 [這裡](selective-disk-backup-restore.md)提供更多詳細資料。 如果您想要在啟用保護期間選擇性地備份幾個磁片，則在 [啟用保護期間](#example-request-body)，下列程式碼片段應為要求主體。

```json
{
"properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/DefaultPolicy",
    "extendedProperties":  {
      "diskExclusionProperties":{
          "diskLunList":[0,1],
          "isInclusionList":true
        }
    }
}
}
```

在上述要求主體中，[擴充屬性] 區段中會提供要備份的磁片清單。

|屬性  |值  |
|---------|---------|
|diskLunList     | 磁片 LUN 清單是 *資料磁片的 lun*清單。 **作業系統磁片一律會進行備份，而且不需要提及**。        |
|IsInclusionList     | 應為 **true** ，才會在備份期間包含 lun。 如果為 **false**，則會排除上述 lun。         |

因此，如果需要只備份 OS 磁片，則應該排除 _所有_ 資料磁片。 更簡單的方法是說，不應包含任何資料磁片。 因此，磁片 LUN 清單會是空的，而且 **IsInclusionList** 會是 **true**。 同樣地，您也可以考慮選取子集的更簡單方法：應該一律排除幾個磁片，或一律包含一些磁片。 依序選擇 LUN 清單和布林值變數值。

## <a name="trigger-an-on-demand-backup-for-a-protected-azure-vm"></a>針對受保護的 Azure VM 觸發隨選備份

一旦 Azure VM 設定為備份，就會根據原則排程進行備份。 您可以等候第一個排定的備份，或隨時觸發隨選備份。 隨選備份的保留期與備份原則的保留期不同，且可指定為特定的日期時間。 如果未指定，即會假設為從觸發隨選備份當日起算的 30 天。

觸發隨選備份為 *POST* 作業。

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/backup?api-version=2016-12-01
```

`{containerName}` 和 `{protectedItemName}` 如[上述](#responses-to-get-operation)所建構的。 `{fabricName}` 為 "Azure"。 針對我們的範例，這會轉譯為：

```http
POST https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM/backup?api-version=2016-12-01
```

### <a name="create-the-request-body-for-on-demand-backup"></a>建立隨選備份的要求主體

若要觸發隨選備份，以下是要求本文的元件。

|名稱  |類型  |描述  |
|---------|---------|---------|
|properties     | [IaaSVMBackupRequest](/rest/api/backup/backups/trigger#iaasvmbackuprequest)        |BackupRequestResource 屬性         |

如需要求本文的完整定義清單及其他詳細資訊，請參閱[觸發受保護項目的備份 REST API 文件](/rest/api/backup/backups/trigger#request-body) \(英文\)。

#### <a name="example-request-body-for-on-demand-backup"></a>隨選備份的要求主體範例

下列要求本文會定義觸發受保護項目之備份所需的屬性。 如果未指定保留期，則會在備份作業觸發程式的時間內保留30天。

```json
{
   "properties": {
    "objectType": "IaasVMBackupRequest",
    "recoveryPointExpiryTimeInUTC": "2018-12-01T02:16:20.3156909Z"
  }
}
```

### <a name="responses-for-on-demand-backup"></a>隨選備份的回應

觸發隨選備份為[非同步作業](../azure-resource-manager/management/async-operations.md)。 這表示此作業會建立另一項需要個別追蹤的作業。

它會傳回兩個回應：在建立另一項作業時傳回 202 (已接受)，然後在該作業完成時傳回 200 (確定)。

|名稱  |類型  |描述  |
|---------|---------|---------|
|202 已接受     |         |     已接受    |

#### <a name="example-responses-for-on-demand-backup"></a>隨選備份的範例回應

一旦提交 *POST* 要求以進行隨選備份之後，初始回應會是 202 (已接受) 並具備位置標頭或 Azure-async-header。

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testVaultRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/b8daecaa-f8f5-44ed-9f18-491a9e9ba01f?api-version=2019-05-13
X-Content-Type-Options: nosniff
x-ms-request-id: 7885ca75-c7c6-43fb-a38c-c0cc437d8810
x-ms-client-request-id: 7df8e874-1d66-4f81-8e91-da2fe054811d; 7df8e874-1d66-4f81-8e91-da2fe054811d
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 7885ca75-c7c6-43fb-a38c-c0cc437d8810
x-ms-routing-request-id: SOUTHINDIA:20180521T083541Z:7885ca75-c7c6-43fb-a38c-c0cc437d8810
Cache-Control: no-cache
Date: Mon, 21 May 2018 08:35:41 GMT
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testVaultRG;testVM/protectedItems/vm;testRG;testVM/operationResults/b8daecaa-f8f5-44ed-9f18-491a9e9ba01f?api-version=2019-05-13
X-Powered-By: ASP.NET
```

然後，使用位置標頭或 Azure-AsyncOperation 標頭搭配簡單的 *GET* 命令，來追蹤所產生的作業。

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2019-05-13
```

當作業完成之後，它會在回應本文中傳回 200 (確定) 以及結果備份作業的識別碼。

```json
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: a8b13524-2c95-445f-b107-920806f385c1
x-ms-client-request-id: 5a63209d-3708-4e69-a75f-9499f4c8977c; 5a63209d-3708-4e69-a75f-9499f4c8977c
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14995
x-ms-correlation-request-id: a8b13524-2c95-445f-b107-920806f385c1
x-ms-routing-request-id: SOUTHINDIA:20180521T083723Z:a8b13524-2c95-445f-b107-920806f385c1
Cache-Control: no-cache
Date: Mon, 21 May 2018 08:37:22 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "00000000-0000-0000-0000-000000000000",
  "status": "Succeeded",
  "startTime": "2018-05-21T08:35:40.9488967Z",
  "endTime": "2018-05-21T08:35:40.9488967Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "7ddead57-bcb9-4269-ac31-6a1b57588700"
  }
}
```

由於備份作業是長時間執行的作業，因此需要加以追蹤，如[使用 REST API 監視作業文件](backup-azure-arm-userestapi-managejobs.md#tracking-the-job)所述。

## <a name="modify-the-backup-configuration-for-a-protected-azure-vm"></a>修改受保護 Azure VM 的備份設定

### <a name="changing-the-policy-of-protection"></a>變更保護原則

若要變更用來保護 VM 的原則，您可以使用與[啟用保護](#enabling-protection-for-the-azure-vm)相同的格式。 只在[要求本文](#example-request-body)中提供新的原則識別碼並提交要求。 例如：若要將 testVM 的原則從 ' DefaultPolicy ' 變更為 ' ProdPolicy '，請在要求主體中提供 ' ProdPolicy ' 識別碼。

```json
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/ProdPolicy"
  }
}
```

回應將遵循與[針對啟用保護](#responses-to-create-protected-item-operation)所述相同的格式。

#### <a name="excluding-disks-during-azure-vm-protection"></a>在 Azure VM 保護期間排除磁片

如果已備份 Azure VM，您可以藉由變更保護原則來指定要備份或排除的磁片清單。 請在[啟用保護期間](#excluding-disks-in-azure-vm-backup)，以與排除磁片相同的格式來準備要求

> [!IMPORTANT]
> 上述要求主體一律是要排除或納入的資料磁片的最終複本。 這不會 *新增* 至先前的設定。 例如：如果您第一次將保護更新為「排除資料磁片1」，然後重複「排除資料磁片2」，則只會在後續的備份中 *排除資料磁片 2* ，並包含資料磁片1。 這一律是將在後續備份中包含/排除的最終清單。

若要取得已排除或包含的目前磁片清單，請取得受保護的專案資訊，[如下所述。](https://docs.microsoft.com/rest/api/backup/protecteditems/get) 回應會提供資料磁片 Lun 的清單，並指出是否要包含或排除它們。

### <a name="stop-protection-but-retain-existing-data"></a>停止保護，但保留現有資料

若要在受保護的 VM 上移除保護，但保留已經備份的資料，請在要求本文中移除原則並提交要求。 移除與原則的關聯之後，就不會再觸發備份，而且不會建立任何新的復原點。

```http
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": ""
  }
}
```

回應將遵循與[針對觸發隨選備份](#example-responses-for-on-demand-backup)所述相同的格式。 您應該追蹤結果作業，如[使用 REST API 監視作業文件](backup-azure-arm-userestapi-managejobs.md#tracking-the-job)所述。

### <a name="stop-protection-and-delete-data"></a>停止保護並刪除資料

若要在受保護的 VM 上移除保護，同時也要刪除備份資料，請執行刪除作業，如[此處](/rest/api/backup/protecteditems/delete)所詳述。

停止保護並刪除資料為 *DELETE* 作業。

```http
DELETE https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2019-05-13
```

`{containerName}` 和 `{protectedItemName}` 如[上述](#responses-to-get-operation)所建構的。 `{fabricName}` 是 "Azure"。 針對我們的範例，這會轉譯為：

```http
DELETE https://management.azure.com//Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM?api-version=2019-05-13
```

#### <a name="responses-for-delete-protection"></a>刪除保護的回應

*DELETE* 作業為[非同步作業](../azure-resource-manager/management/async-operations.md)。 這表示此作業會建立另一項需要個別追蹤的作業。

它會傳回兩個回應：在建立另一項作業時傳回 202 (已接受)，然後在該作業完成時傳回 204 (NoContent)。

|名稱  |類型  |描述  |
|---------|---------|---------|
|204 NoContent     |         |  NoContent       |
|202 已接受     |         |     已接受    |

> [!IMPORTANT]
> 為了防止意外刪除的情況，復原服務保存庫有 [可供使用的虛刪除功能](use-restapi-update-vault-properties.md#soft-delete-state) 。 如果保存庫的虛刪除狀態設定為 [已啟用]，則刪除作業將不會立即刪除資料。 它會保留14天，然後永久清除。 您不需支付此14天期間的儲存體費用。 若要復原刪除操作，請參閱 [復原-刪除一節](#undo-the-deletion)。

### <a name="undo-the-deletion"></a>復原刪除

復原意外刪除類似于建立備份專案。 復原刪除之後，會保留專案，但不會觸發未來的備份。

復原刪除是 *PUT* 作業，非常類似于 [變更原則](#changing-the-policy-of-protection) 及/或 [啟用保護](#enabling-protection-for-the-azure-vm)。 只需提供在[要求主體](#example-request-body)中使用變數*isRehydrate*復原刪除的意圖，然後提交要求。 例如：若要復原 testVM 的刪除，則應該使用下列要求主體。

```http
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "protectionState": "ProtectionStopped",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "isRehydrate": true
  }
}
```

回應將遵循與[針對觸發隨選備份](#example-responses-for-on-demand-backup)所述相同的格式。 您應該追蹤結果作業，如[使用 REST API 監視作業文件](backup-azure-arm-userestapi-managejobs.md#tracking-the-job)所述。

## <a name="next-steps"></a>後續步驟

[從 Azure 虛擬機器備份還原資料](backup-azure-arm-userestapi-restoreazurevms.md)。

如需 Azure 備份 REST API 的詳細資訊，請參閱下列文件：

- [Azure 復原服務提供者 REST API](/rest/api/recoveryservices/)
- [Get started with Azure REST API](/rest/api/azure/) (開始使用 Azure REST API)
