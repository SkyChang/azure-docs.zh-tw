---
author: erhopf
ms.author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 05/11/2020
ms.openlocfilehash: 235b7946fbcfc2322878428cce72e77ecceb9cfc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "88010936"
---
## <a name="authenticate-with-azure-active-directory"></a>向 Azure Active Directory 進行驗證

> [!IMPORTANT]
> 1. 目前， **只有** 電腦視覺 API、臉部 API、文字分析 API、沈浸式閱讀程式、表單辨識器、異常偵測器和所有 Bing 服務，但 Bing 自訂搜尋支援使用 AZURE ACTIVE DIRECTORY (AAD) 進行驗證。
> 2. AAD 驗證必須一律與 Azure 資源的自訂子功能變數名稱稱一起使用。 [區域端點](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains#is-there-a-list-of-regional-endpoints) 不支援 AAD 驗證。

在前面幾節中，我們示範了如何使用單一服務或多服務訂用帳戶金鑰組 Azure 認知服務進行驗證。 雖然這些金鑰提供快速且簡單的途徑來開始開發，但它們會在需要 Azure 角色型存取控制 (Azure RBAC) 的更複雜案例中變得很簡單。 讓我們看看使用 Azure Active Directory (AAD) 進行驗證所需的內容。

在下列各節中，您將使用 Azure Cloud Shell 環境或 Azure CLI 來建立子域、指派角色，以及取得持有人權杖以呼叫 Azure 認知服務。 如果您停滯，每一節會提供連結，其中包含 Azure Cloud Shell/Azure CLI 中每個命令的所有可用選項。

### <a name="create-a-resource-with-a-custom-subdomain"></a>建立具有自訂子域的資源

第一個步驟是建立自訂子域。 如果您想要使用沒有自訂子功能變數名稱稱的現有認知服務資源，請遵循 [認知服務自訂子域](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains#how-does-this-impact-existing-resources) 中的指示，為您的資源啟用自訂子域。

1. 從開啟 Azure Cloud Shell 開始。 然後 [選取訂用](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext?view=azps-3.3.0)帳戶：

   ```powershell-interactive
   Set-AzContext -SubscriptionName <SubscriptionName>
   ```

2. 接下來，使用自訂子域 [建立認知服務資源](https://docs.microsoft.com/powershell/module/az.cognitiveservices/new-azcognitiveservicesaccount?view=azps-1.8.0) 。 子功能變數名稱稱必須是全域唯一的，且不能包含特殊字元，例如： "."、"！"、"，"。

   ```powershell-interactive
   $account = New-AzCognitiveServicesAccount -ResourceGroupName <RESOURCE_GROUP_NAME> -name <ACCOUNT_NAME> -Type <ACCOUNT_TYPE> -SkuName <SUBSCRIPTION_TYPE> -Location <REGION> -CustomSubdomainName <UNIQUE_SUBDOMAIN>
   ```

3. 如果成功， **端點** 應該會顯示您資源的唯一子功能變數名稱稱。


### <a name="assign-a-role-to-a-service-principal"></a>將角色指派給服務主體

現在您已有與資源相關聯的自訂子域，您必須將角色指派給服務主體。

> [!NOTE]
> 請記住，Azure 角色指派最多可能需要五分鐘的時間來傳播。

1. 首先，讓我們來註冊 [AAD 應用程式](https://docs.microsoft.com/powershell/module/Az.Resources/New-AzADApplication?view=azps-1.8.0)。

   ```powershell-interactive
   $SecureStringPassword = ConvertTo-SecureString -String <YOUR_PASSWORD> -AsPlainText -Force

   $app = New-AzADApplication -DisplayName <APP_DISPLAY_NAME> -IdentifierUris <APP_URIS> -Password $SecureStringPassword
   ```

   在下一個步驟中，您將需要 **ApplicationId** 。

2. 接下來，您必須為 AAD 應用程式 [建立服務主體](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal?view=azps-1.8.0) 。

   ```powershell-interactive
   New-AzADServicePrincipal -ApplicationId <APPLICATION_ID>
   ```

   >[!NOTE]
   > 如果您在 Azure 入口網站中註冊應用程式，就會為您完成此步驟。

3. 最後一個步驟是 [將「認知服務使用者」角色指派](https://docs.microsoft.com/powershell/module/az.Resources/New-azRoleAssignment?view=azps-1.8.0) 給服務主體， (範圍設定為資源) 。 藉由指派角色，您會將此資源的存取權授與服務主體。 您可以對訂用帳戶中的多個資源授與相同的服務主體存取權。
   >[!NOTE]
   > 使用服務主體的 ObjectId，而不是應用程式的 ObjectId。
   > ACCOUNT_ID 將會是您所建立之認知服務帳戶的 Azure 資源識別碼。 您可以從 Azure 入口網站中資源的「屬性」尋找 Azure 資源識別碼。

   ```azurecli-interactive
   New-AzRoleAssignment -ObjectId <SERVICE_PRINCIPAL_OBJECTID> -Scope <ACCOUNT_ID> -RoleDefinitionName "Cognitive Services User"
   ```

### <a name="sample-request"></a>範例要求

在此範例中，會使用密碼來驗證服務主體。 接著會使用提供的權杖來呼叫電腦視覺 API。

1. 取得您的 **TenantId**：
   ```powershell-interactive
   $context=Get-AzContext
   $context.Tenant.Id
   ```

2. 取得權杖：
   > [!NOTE]
   > 如果您使用 Azure Cloud Shell，就無法使用此 `SecureClientSecret` 類別。 

   #### <a name="powershell"></a>[PowerShell](#tab/powershell)
   ```powershell-interactive
   $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList "https://login.windows.net/<TENANT_ID>"
   $secureSecretObject = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.SecureClientSecret" -ArgumentList $SecureStringPassword   
   $clientCredential = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential" -ArgumentList $app.ApplicationId, $secureSecretObject
   $token=$authContext.AcquireTokenAsync("https://cognitiveservices.azure.com/", $clientCredential).Result
   $token
   ```
   
   #### <a name="azure-cloud-shell"></a>[Azure Cloud Shell](#tab/azure-cloud-shell)
   ```Azure Cloud Shell
   $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList "https://login.windows.net/<TENANT_ID>"
   $clientCredential = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential" -ArgumentList $app.ApplicationId, <YOUR_PASSWORD>
   $token=$authContext.AcquireTokenAsync("https://cognitiveservices.azure.com/", $clientCredential).Result
   $token
   ``` 

   ---

3. 呼叫電腦視覺 API：
   ```powershell-interactive
   $url = $account.Endpoint+"vision/v1.0/models"
   $result = Invoke-RestMethod -Uri $url  -Method Get -Headers @{"Authorization"=$token.CreateAuthorizationHeader()} -Verbose
   $result | ConvertTo-Json
   ```

或者，您也可以使用憑證來驗證服務主體。 除了服務主體之外，也支援透過另一個 AAD 應用程式委派許可權的使用者主體。 在此情況下，在取得權杖時，系統會提示使用者進行雙因素驗證，而不是密碼或憑證。

## <a name="authorize-access-to-managed-identities"></a>授權存取受控識別
 
認知服務支援使用 [Azure 資源的受控識別來](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)Azure Active Directory (Azure AD) 驗證。 適用于 Azure 資源的受控識別可以從 Azure 虛擬機器中執行的應用程式（ (Vm) 、函式應用程式、虛擬機器擴展集和其他服務），使用 Azure AD 認證來授權存取認知服務資源。 藉由使用適用于 Azure 資源的受控識別搭配 Azure AD authentication，您可以避免將認證儲存在雲端中執行的應用程式。  

### <a name="enable-managed-identities-on-a-vm"></a>在 VM 上啟用受控識別

您必須在 VM 上啟用 Azure 資源的受控識別，才能使用適用于 Azure 資源的受控識別來授權從您的 VM 存取認知服務資源。 若要瞭解如何啟用 Azure 資源的受控識別，請參閱：

- [Azure 入口網站](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm)
- [Azure CLI](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm)
- [Azure Resource Manager 範本](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm)
- [Azure Resource Manager 用戶端程式庫](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm)

如需受控識別的詳細資訊，請參閱 [適用于 Azure 資源的受控](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)識別。
