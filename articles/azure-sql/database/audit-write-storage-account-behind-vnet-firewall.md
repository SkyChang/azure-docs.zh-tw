---
title: 對 VNet 和防火牆後方的儲存體帳戶進行審核
description: 設定在虛擬網路和防火牆後方的儲存體帳戶上寫入資料庫事件的審核
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: how-to
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 06/17/2020
ms.custom: azure-synapse
ms.openlocfilehash: 74926411b659cf5973b03b2caca58d7666803f9c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91444531"
---
# <a name="write-audit-to-a-storage-account-behind-vnet-and-firewall"></a>將 audit 寫入至 VNet 和防火牆後方的儲存體帳戶
[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-asa.md)]


[Azure SQL Database](sql-database-paas-overview.md)和[Azure Synapse Analytics](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)的審核支援將資料庫事件寫入虛擬網路和防火牆後方的[Azure 儲存體帳戶](../../storage/common/storage-account-overview.md)。

本文說明兩種設定此選項 Azure SQL Database 和 Azure 儲存體帳戶的方式。 第一個使用 Azure 入口網站，第二個使用 REST。

## <a name="background"></a>背景

[Azure 虛擬網路 (VNet) ](../../virtual-network/virtual-networks-overview.md) 是 azure 中私人網路的基本組建區塊。 VNet 可讓多種類型的 Azure 資源 (例如 Azure 虛擬機器 (VM)) 安全地彼此通訊，以及與網際網路和內部部署網路通訊。 VNet 與您自己的資料中心中的傳統網路類似，但帶來了 Azure 基礎結構的額外優點，例如調整規模、可用性和隔離。

若要深入瞭解 VNet 概念、最佳做法和其他更多，請參閱 [什麼是 Azure 虛擬網路](../../virtual-network/virtual-networks-overview.md)。

若要深入瞭解如何建立虛擬網路，請參閱 [快速入門：使用 Azure 入口網站建立虛擬網路](../../virtual-network/quick-create-portal.md)。

## <a name="prerequisites"></a>必要條件

若要讓 audit 寫入 VNet 或防火牆後方的儲存體帳戶，需要下列必要條件：

> [!div class="checklist"]
>
> * 一般用途 v2 儲存體帳戶。 如果您有一般用途 v1 或 blob 儲存體帳戶，請 [升級至一般用途 v2 儲存體帳戶](../../storage/common/storage-account-upgrade.md)。 如需詳細資訊，請參閱[儲存體帳戶類型](../../storage/common/storage-account-overview.md#types-of-storage-accounts)。
> * 儲存體帳戶必須位於相同的訂用帳戶和 [邏輯 SQL 伺服器](logical-servers.md)的相同位置。
> * Azure 儲存體帳戶需要 `Allow trusted Microsoft services to access this storage account` 。 在儲存體帳戶 **防火牆和虛擬網路**上設定此設定。
> * 您必須具有 `Microsoft.Authorization/roleAssignments/write` 所選儲存體帳戶的許可權。 如需詳細資訊，請參閱 [Azure 內建角色](../../role-based-access-control/built-in-roles.md)。

## <a name="configure-in-azure-portal"></a>在 Azure 入口網站中設定

使用您的訂用帳戶連接到 [Azure 入口網站](https://portal.azure.com) 。 流覽至資源群組和伺服器。

1. 按一下 [安全性] 標題下的 [ **審核** ]。 選取 [ **開啟**]。

2. 選取 [儲存體] 。 選取將儲存記錄的儲存體帳戶。 儲存體帳戶必須符合 [必要條件](#prerequisites)中所列的需求。

3. 開啟 **儲存體詳細資料**

  > [!NOTE]
  > 如果選取的儲存體帳戶位於 VNet 後方，您將會看到下列訊息：
  >
  >`You have selected a storage account that is behind a firewall or in a virtual network. Using this storage requires to enable 'Allow trusted Microsoft services to access this storage account' on the storage account and creates a server managed identity with 'storage blob data contributor' RBAC.`
  >
  >如果您看不到此訊息，表示儲存體帳戶不在 VNet 後方。

4. 選取保留期限的天數。 然後按一下 [確定] 。 比保留期間舊的記錄會遭到刪除。

5. 在您的 [審核設定] 上選取 [ **儲存** ]。

您已成功設定 audit，以寫入 VNet 或防火牆後方的儲存體帳戶。

## <a name="configure-with-rest-commands"></a>使用 REST 命令設定

除了使用 Azure 入口網站之外，您還可以使用 REST 命令來設定 audit，以在 VNet 和防火牆後方的儲存體帳戶上寫入資料庫事件。

本節中的範例腳本需要您先更新腳本，再執行腳本。 取代指令碼中的下列值：

|範例值|範例描述|
|:-----|:-----|
|`<subscriptionId>`| Azure 訂用帳戶識別碼|
|`<resource group>`| 資源群組|
|`<logical SQL server>`| 伺服器名稱|
|`<administrator login>`| 系統管理員帳戶 |
|`<complex password>`| 系統管理員帳戶的複雜密碼|

若要設定 SQL Audit 以將事件寫入 VNet 或防火牆後方的儲存體帳戶：

1. 向 Azure Active Directory (Azure AD) 註冊您的伺服器。 請使用 PowerShell 或 REST API。

   **PowerShell**

   ```powershell
   Connect-AzAccount
   Select-AzSubscription -SubscriptionId <subscriptionId>
   Set-AzSqlServer -ResourceGroupName <your resource group> -ServerName <azure server name> -AssignIdentity
   ```

   [**REST API**](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)：

   範例要求

   ```html
   PUT https://management.azure.com/subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Sql/servers/<azure server name>?api-version=2015-05-01-preview
   ```

   Request body

   ```json
   {
   "identity": {
              "type": "SystemAssigned",
              },
   "properties": {
     "fullyQualifiedDomainName": "<azure server name>.database.windows.net",
     "administratorLogin": "<administrator login>",
     "administratorLoginPassword": "<complex password>",
     "version": "12.0",
     "state": "Ready"
   }
   ```

2. 開啟 [Azure 入口網站](https://portal.azure.com)。 瀏覽至儲存體帳戶。 找出 ** (IAM) 的存取控制 **，然後按一下 [ **新增角色指派**]。 將 **儲存體 Blob 資料參與者** Azure 角色指派給裝載您向 Azure Active Directory (Azure AD) 註冊之資料庫的伺服器，如同上一個步驟中所述。

   > [!NOTE]
   > 僅有具備「擁有者」權限的成員才能執行此步驟。 針對各種 Azure 內建角色，請參閱 [azure 內建角色](../../role-based-access-control/built-in-roles.md)。

3. 設定 [伺服器的 blob 稽核原則](/rest/api/sql/server%20auditing%20settings/createorupdate)，而不指定 *storageAccountAccessKey*：

   範例要求

   ```html
     PUT https://management.azure.com/subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Sql/servers/<azure server name>/auditingSettings/default?api-version=2017-03-01-preview
   ```

   Request body

   ```json
   {
     "properties": {
      "state": "Enabled",
      "storageEndpoint": "https://<storage account>.blob.core.windows.net"
     }
   }
   ```

## <a name="using-azure-powershell"></a>使用 Azure PowerShell

- [建立或更新資料庫稽核原則 (設定 AzSqlDatabaseAudit) ](/powershell/module/az.sql/set-azsqldatabaseaudit)
- [建立或補救伺服器稽核原則 (設定 AzSqlServerAudit) ](/powershell/module/az.sql/set-azsqlserveraudit)

## <a name="using-azure-resource-manager-template"></a>使用 Azure Resource Manager 範本

您可以設定使用 [Azure Resource Manager](../../azure-resource-manager/management/overview.md) 範本將審核功能寫入虛擬網路和防火牆後方的儲存體帳戶，如下列範例所示：

> [!IMPORTANT]
> 若要使用虛擬網路和防火牆後方的儲存體帳戶，您必須將 **isStorageBehindVnet** 參數設定為 true

- [部署啟用審核的 Azure SQL server，以將 audit 記錄寫入至 blob 儲存體](https://azure.microsoft.com/resources/templates/201-sql-auditing-server-policy-to-blob-storage)

> [!NOTE]
> 連結的範例位於外部公用存放庫，並提供「原樣」且沒有擔保，且不受任何 Microsoft 支援方案/服務的支援。

## <a name="next-steps"></a>後續步驟

* [使用 PowerShell 來建立虛擬網路服務端點，然後使用 Azure SQL Database 的虛擬網路規則。](scripts/vnet-service-endpoint-rule-powershell-create.md)
* [虛擬網路規則：使用 REST Api 的作業](/rest/api/sql/virtualnetworkrules)
* [針對伺服器使用虛擬網路服務端點和規則](vnet-service-endpoint-rule-overview.md)
