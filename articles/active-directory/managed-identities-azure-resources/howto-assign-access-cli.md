---
title: 使用 Azure CLI Azure AD 將受控識別存取權指派給資源
description: 使用 Azure CLI 在一個資源上指派受控識別、存取另一個資源的逐步指示。
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2017
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.custom: devx-track-azurecli
ms.openlocfilehash: 13c69dda1e300bcff95b6a017fdeb308a6bbf3a4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "90969244"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-azure-cli"></a>使用 Azure CLI 為受控識別指派對資源的存取權

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

以受控識別設定 Azure 資源後，您可以為此受控識別提供對另一項資源的存取權，就像任何安全性主體一樣。 此範例說明如何使用 Azure CLI 為 Azure 虛擬機器或虛擬機器擴展集的受控識別提供對 Azure 儲存體帳戶的存取權。

## <a name="prerequisites"></a>先決條件

- 如果您不熟悉 Azure 資源的受控識別，請參閱[概觀一節](overview.md)。 **請務必檢閱[系統指派和使用者指派受控識別之間的差異](overview.md#managed-identity-types)**。
- 如果您還沒有 Azure 帳戶，請先[註冊免費帳戶](https://azure.microsoft.com/free/)，再繼續進行。
- 若要執行範例指令碼，您有兩個選項：
    - 使用 [Azure Cloud Shell](../../cloud-shell/overview.md)，您可以使用程式碼區塊右上角的 [試用] 按鈕來開啟。
    - 藉由安裝最新版本的 [Azure CLI](/cli/azure/install-azure-cli)，在本機執行指令碼，然後使用 [az login](/cli/azure/reference-index#az-login) 來登入 Azure。 請使用您想用來建立資源，且已與 Azure 訂用帳戶建立關聯的帳戶。

## <a name="use-azure-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>使用 Azure RBAC 將受控識別存取權指派給另一個資源

在 Azure 資源 (例如 [Azure 虛擬機器](qs-configure-cli-windows-vm.md)或 [Azure 虛擬機器擴展集](qs-configure-cli-windows-vmss.md)) 上啟用受控識別之後： 

1. 在此範例中，我們會將 Azure 虛擬機器存取權給予儲存體帳戶。 首先，我們會使用 [az resource list](/cli/azure/resource/#az-resource-list) 來取得名為 myVM 之虛擬機器的服務主體：

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```
   用於 Azure 虛擬機器擴展集的命令是相同的，不同的是您會取得名為 "DevTestVMSS" 的虛擬機器擴展集服務主體：
   
   ```azurecli-interactive
   spID=$(az resource list -n DevTestVMSS --query [*].identity.principalId --out tsv)
   ```

1. 一旦您有服務主體 ID 後，請使用 [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create) 將虛擬機器或虛擬機器擴展集「讀取」存取權提供給稱為 "myStorageAcct" 的儲存體帳戶：

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="next-steps"></a>後續步驟

- [Azure 資源受控識別概觀](overview.md)
- 若要在 Azure 虛擬機器上啟用受控識別，請參閱[使用 Azure CLI 在 Azure VM 上設定 Azure 資源的受控識別](qs-configure-cli-windows-vm.md)。
- 若要在 Azure 虛擬機器擴展集上啟用受控識別，請參閱[使用 Azure CLI 在虛擬機器擴展集上設定 Azure 資源的受控識別](qs-configure-cli-windows-vmss.md)。
