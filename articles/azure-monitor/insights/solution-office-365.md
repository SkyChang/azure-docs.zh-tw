---
title: Azure 中的 Office 365 管理解決方案
description: 本文提供在 Azure 中設定和使用 Office 365 解決方案的相關詳細資料。  它包含在 Azure 監視器中所建立之 Office 365 記錄的詳細描述。
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/30/2020
ms.openlocfilehash: eb20bf4164cb2153f6786dbec04f79453554fa25
ms.sourcegitcommit: 090ea6e8811663941827d1104b4593e29774fa19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2020
ms.locfileid: "91999750"
---
# <a name="office-365-management-solution-in-azure-preview"></a>Azure 中的 Office 365 管理解決方案 (預覽)

![Office 365 標誌](media/solution-office-365/icon.png)

> [!IMPORTANT]
> ## <a name="solution-update"></a>解決方案更新
> 此解決方案已取代為[Azure Sentinel](../../sentinel/overview.md)中的[Office 365](../../sentinel/connect-office-365.md)一般可用性解決方案，以及[Azure AD 的報告與監視解決方案](../../active-directory/reports-monitoring/plan-monitoring-and-reporting.md)。 它們一起提供舊版 Azure 監視器 Office 365 解決方案的更新版本，並提供改良的設定體驗。 您可以繼續使用現有的解決方案，直到2020年10月31日為止。
> 
> Azure Sentinel 是雲端原生安全性資訊和事件管理解決方案，可內嵌記錄並提供額外的 SIEM 功能，包括偵測、調查、搜尋和機器學習導向的見解。 使用 Azure Sentinel 現在可讓您內嵌 Office 365 SharePoint 活動和 Exchange 記錄管理。
> 
> Azure AD 報告可讓您更全面地查看環境中 Azure AD 活動的記錄，包括登入事件、審核事件，以及對目錄所做的變更。 若要連接 Azure AD 記錄檔，您可以使用 [Azure Sentinel Azure AD 連接器](../../sentinel/connect-azure-active-directory.md) ，或設定 [Azure AD 記錄與 Azure 監視器的整合](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)。 
>
> Azure AD 記錄檔的集合 Azure 監視器定價。  如需詳細資訊，請參閱 [Azure 監視器定價](https://azure.microsoft.com/pricing/details/monitor/) 。
>
> 若要使用 Azure Sentinel Office 365 方案：
> 1. 在 Azure Sentinel 中使用 Office 365 連接器會影響您工作區的定價。 如需詳細資訊，請參閱 [Azure Sentinel 定價](https://azure.microsoft.com/pricing/details/azure-sentinel/)。
> 2. 如果您已經在使用 Azure 監視器 Office 365 解決方案，則必須先使用 [下一節](#uninstall)的 [卸載] 區段中的腳本將其卸載。
> 3. 在您的工作區上[啟用 Azure Sentinel 方案](../../sentinel/quickstart-onboard.md)。
> 4. 移至 Azure Sentinel 中的 [ **資料連線器** ] 頁面，並啟用 **Office 365** 連接器。
>
> ## <a name="frequently-asked-questions"></a>常見問題集
> 
> ### <a name="q-is-it-possible-to-on-board-the-office-365-azure-monitor-solution-between-now-and-october-31"></a>問：是否可以在現在和10月31日之間，將 Office 365 Azure 監視器方案上線？
> 否，無法再使用 Azure 監視器 Office 365 解決方案上架腳本。 解決方案將在10月31日移除。
> 
> ### <a name="q-will-the-tables-and-schemas-be-changed"></a>問：是否要變更資料表和架構？
> **OfficeActivity**資料表名稱和架構會維持與目前解決方案中的相同。 您可以繼續使用新方案中的相同查詢，但不包括參考 Azure AD 資料的查詢。
> 
> 新的 [Azure AD 報告和監視方案](../../active-directory/reports-monitoring/plan-monitoring-and-reporting.md) 記錄會內嵌到 [SigninLogs](../../active-directory/reports-monitoring/concept-sign-ins.md) 和 [AuditLogs](../../active-directory/reports-monitoring/concept-audit-logs.md) 資料表，而不是 **OfficeActivity**。 如需詳細資訊，請參閱 [如何分析 Azure AD 記錄](../../active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics.md)，這也是 Azure Sentinel 和 Azure 監視器使用者的相關資訊。
> 
> 以下是將查詢從 **OfficeActivity** 轉換成 **SigninLogs**的範例：
> 
> **依使用者查詢失敗的登入：**
> 
> ```Kusto
> OfficeActivity
> | where TimeGenerated >= ago(1d) 
> | where OfficeWorkload == "AzureActiveDirectory"                      
> | where Operation == 'UserLoginFailed'
> | summarize count() by UserId    
> ```
> 
> ```Kusto
> SigninLogs
> | where ConditionalAccessStatus == "failure" or ConditionalAccessStatus == "notApplied"
> | summarize count() by UserDisplayName
> ```
> 
> **查看 Azure AD 作業：**
> 
> ```Kusto
> OfficeActivity
> | where OfficeWorkload =~ "AzureActiveDirectory"
> | sort by TimeGenerated desc
> | summarize AggregatedValue = count() by Operation
> ```
> 
> ```Kusto
> AuditLogs
> | summarize count() by OperationName
> ```
> 
> ### <a name="q-how-can-i-on-board-azure-sentinel"></a>問：我可以如何 Azure Sentinel？
> Azure Sentinel 是可在新的或現有的 Log Analytics 工作區上啟用的解決方案。 若要深入瞭解，請參閱 [Azure Sentinel 的隨附檔](../../sentinel/quickstart-onboard.md)。
>
> ### <a name="q-do-i-need-azure-sentinel-to-connect-the-azure-ad-logs"></a>問：我是否需要 Azure Sentinel 才能連接 Azure AD 記錄？
> 您可以設定與 Azure 監視器（與 Azure Sentinel 解決方案無關）的 [Azure AD 記錄整合](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)。 Azure Sentinel 提供原生連接器，以及 Azure AD 記錄檔的現成內容。 如需詳細資訊，請參閱下列有關現成安全性導向內容的問題。
>
> ###    <a name="q-what-are-the-differences-when-connecting-azure-ad-logs-from-azure-sentinel-and-azure-monitor"></a>問：從 Azure Sentinel 和 Azure 監視器連接 Azure AD 記錄時，有何差異？
> Azure Sentinel 和 Azure 監視器會根據相同的 [Azure AD 報告和監視解決方案](../../active-directory/reports-monitoring/plan-monitoring-and-reporting.md)來連接 Azure AD 記錄。 Azure Sentinel 提供單鍵的原生連接器，可連接相同的資料並提供監視資訊。
>
> ###    <a name="q-what-do-i-need-to-change-when-moving-to-the-new-azure-ad-reporting-and-monitoring-tables"></a>問：移至新的 Azure AD 報告和監視資料表時，需要變更的專案為何？
> 使用 Azure AD 資料的所有查詢（包括警示、儀表板中的查詢，以及您使用 Office 365 Azure AD 資料建立的任何內容）都必須使用新的資料表來重新建立。
>
> Azure Sentinel 和 Azure AD 提供內建內容，可讓您在移至 Azure AD 的報告和監視解決方案時使用。 如需詳細資訊，請參閱下一個有關現成安全性導向內容的問題，以及 [如何使用 Azure 監視器活頁簿來 Azure Active Directory 報表](../../active-directory/reports-monitoring/howto-use-azure-monitor-workbooks.md)。 
>
> ### <a name="q-how-i-can-use-the-azure-sentinel-out-of-the-box-security-oriented-content"></a>問：我可以如何使用 Azure Sentinel 的現成安全性導向內容？
> Azure Sentinel 根據 Office 365 和 Azure AD 記錄，提供現成可用的安全性導向儀表板、自訂警示查詢、搜尋查詢、調查和自動回應功能。 若要深入瞭解，請流覽 Azure Sentinel GitHub 和教學課程：
>
> - [偵測現成的威脅](../../sentinel/tutorial-detect-threats-built-in.md)
> - [建立自訂分析規則來偵測可疑的威脅](../../sentinel/tutorial-detect-threats-custom.md)
> - [監視您的資料](../../sentinel/tutorial-monitor-your-data.md)
> - [使用 Azure Sentinel 調查事件](../../sentinel/tutorial-investigate-cases.md)
> - [在 Azure Sentinel 中設定自動化威脅回應](../../sentinel/tutorial-respond-threats-playbook.md)
> - [Azure Sentinel GitHub 社區](https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks)
> 
> ### <a name="q-does-azure-sentinel-provide-additional-connectors-as-part-of-the-solution"></a>問： Azure Sentinel 是否提供其他連接器作為解決方案的一部分？
> 是，請參閱 [Azure Sentinel 連接資料來源](../../sentinel/connect-data-sources.md)。
> 
> ###    <a name="q-what-will-happen-on-october-31-do-i-need-to-offboard-beforehand"></a>問：10月31日會發生什麼事？ 我需要預先下架嗎？
> 
> - 您將無法從 **Office365** 解決方案接收資料。 解決方案將會從您的工作區中移除，且將無法再于 Marketplace 中使用。
> - 針對 Azure Sentinel 客戶，Log Analytics 工作區解決方案 **Office365** 將包含在 Azure Sentinel **SecurityInsights** 解決方案中。
> - 如果您未在10月31日手動下架方案，您的資料將會自動中斷連線，並移除 **OfficeActivity** 資料表。 就算如此，當您在 Azure Sentinel 中啟用 Office 365 連接器時，仍然可以還原資料表，如下所述。
> 
> ### <a name="q-will-my-data-transfer-to-the-new-solution"></a>問：我的資料會傳送到新的解決方案嗎？
> 是。 當您從工作區中移除 **Office 365** 解決方案時，其資料將會暫時無法使用，因為已移除架構。 當您在 Sentinel 中啟用新的 **Office 365** 連接器時，架構會還原至工作區，而且任何已收集的資料都會變成可用。 
 

Office 365 管理解決方案可讓您監視 Azure 監視器中的 Office 365 環境。

- 監視 Office 365 帳戶上的使用者活動，以分析使用模式並識別行為趨勢。 例如，您可以擷取特定使用方式情節，例如在貴組織外部共用的檔案或最熱門的 SharePoint 網站。
- 監視系統管理員活動以追蹤設定變更或高權限作業。
- 偵測並調查的不必要的使用者行為，並可以針對貴組織的需求進行自訂。
- 示範稽核與合規性。 例如，您可以監視機密檔案的檔案存取作業，以協助進行稽核與合規性流程。
- 針對組織的 Office 365 活動資料使用[記錄查詢](../log-query/log-query-overview.md)，執行作業疑難排解。


## <a name="uninstall"></a>解除安裝

您可以使用[移除管理解決方案](solutions.md#remove-a-monitoring-solution)中的程序移除 Office 365 管理解決方案。 但這不會停止從 Office 365 收集資料到 Azure 監視器。 請遵循底下程序，從 Office 365 取消訂閱並停止收集資料。

1. 將下列指令碼儲存為 office365_unsubscribe.ps1**。

    ```powershell
    param (
        [Parameter(Mandatory=$True)][string]$WorkspaceName,
        [Parameter(Mandatory=$True)][string]$ResourceGroupName,
        [Parameter(Mandatory=$True)][string]$SubscriptionId,
        [Parameter(Mandatory=$True)][string]$OfficeTennantId
    )
    $line='#-------------------------------------------------------------------------------------------------------------------------------------------------------------------------'
    
    $line
    IF ($Subscription -eq $null)
        {Login-AzAccount -ErrorAction Stop}
    $Subscription = (Select-AzSubscription -SubscriptionId $($SubscriptionId) -ErrorAction Stop)
    $Subscription
    $option = [System.StringSplitOptions]::RemoveEmptyEntries 
    $Workspace = (Set-AzOperationalInsightsWorkspace -Name $($WorkspaceName) -ResourceGroupName $($ResourceGroupName) -ErrorAction Stop)
    $Workspace
    $WorkspaceLocation= $Workspace.Location
    
    # Client ID for Azure PowerShell
    $clientId = "1950a258-227b-4e31-a9cf-717495945fc2"
    # Set redirect URI for Azure PowerShell
    $redirectUri = "urn:ietf:wg:oauth:2.0:oob"
    $domain='login.microsoftonline.com'
    $adTenant =  $Subscription[0].Tenant.Id
    $authority = "https://login.windows.net/$adTenant";
    $ARMResource ="https://management.azure.com/";
    $xms_client_tenant_Id ='55b65fb5-b825-43b5-8972-c8b6875867c1'
    
    switch ($WorkspaceLocation) {
           "USGov Virginia" { 
                             $domain='login.microsoftonline.us';
                              $authority = "https://login.microsoftonline.us/$adTenant";
                              $ARMResource ="https://management.usgovcloudapi.net/"; break} # US Gov Virginia
           default {
                    $domain='login.microsoftonline.com'; 
                    $authority = "https://login.windows.net/$adTenant";
                    $ARMResource ="https://management.azure.com/";break} 
                    }
    
    Function RESTAPI-Auth { 
    
    $global:SubscriptionID = $Subscription.SubscriptionId
    # Set Resource URI to Azure Service Management API
    $resourceAppIdURIARM=$ARMResource;
    # Authenticate and Acquire Token 
    # Create Authentication Context tied to Azure AD Tenant
    $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
    # Acquire token
    $platformParameters = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.PlatformParameters" -ArgumentList "Auto"
    $global:authResultARM = $authContext.AcquireTokenAsync($resourceAppIdURIARM, $clientId, $redirectUri, $platformParameters)
    $global:authResultARM.Wait()
    $authHeader = $global:authResultARM.Result.CreateAuthorizationHeader()
    $authHeader
    }
    
    Function Office-UnSubscribe-Call{
    
    #----------------------------------------------------------------------------------------------------------------------------------------------
    $authHeader = $global:authResultARM.Result.CreateAuthorizationHeader()
    $ResourceName = "https://manage.office.com"
    $SubscriptionId   = $Subscription[0].Subscription.Id
    $OfficeAPIUrl = $ARMResource + 'subscriptions/' + $SubscriptionId + '/resourceGroups/' + $ResourceGroupName + '/providers/Microsoft.OperationalInsights/workspaces/' + $WorkspaceName + '/datasources/office365datasources_'  + $SubscriptionId + $OfficeTennantId + '?api-version=2015-11-01-preview'
    
    $Officeparams = @{
        ContentType = 'application/json'
        Headers = @{
        'Authorization'="$($authHeader)"
        'x-ms-client-tenant-id'=$xms_client_tenant_Id
        'Content-Type' = 'application/json'
        }
        Method = 'Delete'
        URI = $OfficeAPIUrl
      }
    
    $officeresponse = Invoke-WebRequest @Officeparams 
    $officeresponse
    
    }
    
    #GetDetails 
    RESTAPI-Auth -ErrorAction Stop
    Office-UnSubscribe-Call -ErrorAction Stop
    ```

2. 使用下列命令來執行指令碼：

    ```
    .\office365_unsubscribe.ps1 -WorkspaceName <Log Analytics workspace name> -ResourceGroupName <Resource Group name> -SubscriptionId <Subscription ID> -OfficeTennantID <Tenant ID> 
    ```

    範例：

    ```powershell
    .\office365_unsubscribe.ps1 -WorkspaceName MyWorkspace -ResourceGroupName MyResourceGroup -SubscriptionId '60b79d74-f4e4-4867-b631-yyyyyyyyyyyy' -OfficeTennantID 'ce4464f8-a172-4dcf-b675-xxxxxxxxxxxx'
    ```

系統會提示您輸入認證。 提供 Log Analytics 工作區的認證。

## <a name="data-collection"></a>資料收集

一開始收集資料可能會需要幾個小時。 一旦開始收集資料，每次建立一筆記錄時，Office 365 都會將 [Webhook 通知](/office/office-365-management-api/office-365-management-activity-api-reference#receiving-notifications) \(英文\) 連同詳細資料傳送至 Azure 監視器。 收到此記錄的幾分鐘內，即可將其用於 Azure 監視器。

## <a name="using-the-solution"></a>使用解決方案

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

當您將 Office 365 解決方案新增至 Log Analytics 工作區時，[Office 365]**** 圖格會新增至儀表板。 此圖格會顯示計數並以圖形表示環境中的電腦數目及其更新合規性。<br><br>
![Office 365 摘要圖格](media/solution-office-365/tile.png)  

按一下 [Office 365]**** 圖格以開啟 [Office 365]**** 儀表板。

![Office 365 儀表板](media/solution-office-365/dashboard.png)  

此儀表板包含下表中的資料行。 每個資料行依計數列出前十個警示，這幾個警示符合該資料行中指定範圍和時間範圍的準則。 您可以按一下資料行底部的 [查看全部]，或按一下資料行標頭，以執行記錄搜尋來提供完整清單。

| 資料行 | 描述 |
|:--|:--|
| Operations | 提供所有受監視 Office 365 訂閱中之作用中使用者的相關資訊。 您也可以查看一段時間內發生的活動數。
| Exchange | 顯示 Exchange Server 活動細目，例如 Add-Mailbox 權限或 Set-Mailbox。 |
| SharePoint | 顯示使用者最常對 SharePoint 文件執行的活動。 當您從此圖格向下鑽研時，搜尋頁面會顯示這些活動的詳細資料，例如此活動的目標文件與位置。 例如，針對存取檔案的事件，您將能夠看到正在存取的檔、其相關聯的帳戶名稱和 IP 位址。 |
| Azure Active Directory | 包含熱門使用者活動，例如「重設使用者密碼」和「登入嘗試」。 向下鑽研時，您可以像「結果狀態」一般地查看這些活動的詳細資料。 如果您想要監視 Azure Active Directory 上的可疑活動，這會很有幫助。 |




## <a name="azure-monitor-log-records"></a>Azure 監視器記錄

由 Office 365 解決方案在 Azure 監視器 Log Analytics 工作區中建立的所有記錄，都具有 **OfficeActivity** 的「類型」****。  **OfficeWorkload** 屬性可決定該記錄所指的 Office 365 服務：Exchange、AzureActiveDirectory、SharePoint 或 OneDrive。  **RecordType** 屬性指定作業的類型。  每種作業類型會有不同的屬性，如下表所示。

### <a name="common-properties"></a>通用屬性

以下是所有 Office 365 記錄通用的屬性。

| 屬性 | 描述 |
|:--- |:--- |
| 類型 | *OfficeActivity* |
| ClientIP | 記錄活動時所使用之裝置的 IP 位址。 IP 位址會以 IPv4 或 IPv6 位址格式顯示。 |
| OfficeWorkload | 記錄所指的 Office 365 服務。<br><br>AzureActiveDirectory<br>Exchange<br>SharePoint|
| 作業 | 使用者或管理員活動的名稱。  |
| OrganizationId | 貴組織的 Office 365 租用戶 GUID。 無論此值是在哪一個 Office 365 服務中發生，對於貴組織而言它將會一律相同。 |
| RecordType | 執行的作業類型。 |
| ResultStatus | 指出 (Operation 屬性中指定的) 動作是否成功。 可能的值為 Succeeded、PartiallySucceeded 或 Failed。 對於 Exchange 管理員活動，這個值將會是 True 或 False。 |
| UserId | 執行動作導致記下該記錄之使用者的 UPN (使用者主體名稱)，例如 my_name@my_domain_name。 請注意，由系統帳戶 (例如 SHAREPOINT\system 或 NTAUTHORITY\SYSTEM) 所執行之活動的記錄也會包含在內。 | 
| UserKey | UserId 屬性所識別之使用者的替代識別碼。  例如，針對由使用者在 SharePoint、商務用 OneDrive 及 Exchange 中所執行的事件，此屬性都會填入 Passport 唯一識別碼 (PUID)。 針對在其他服務中所發生的事件，以及由系統帳戶所執行的事件，此屬性也可能會將相同的值指定為 UserID 屬性|
| UserType | 執行作業的使用者類型。<br><br>管理<br>應用程式<br>DcAdmin<br>標準<br>保留<br>ServicePrincipal<br>系統 |


### <a name="azure-active-directory-base"></a>Azure Active Directory 基底

以下是所有 Azure Active Directory 記錄通用的屬性。

| 屬性 | 描述 |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AzureActiveDirectory_EventType | Azure AD 事件類型。 |
| ExtendedProperties | Azure AD 事件的擴充屬性。 |


### <a name="azure-active-directory-account-logon"></a>Azure Active Directory 帳戶登入

這些記錄會在 Active Directory 使用者嘗試登入時建立。

| 屬性 | 描述 |
|:--- |:--- |
| `OfficeWorkload` | AzureActiveDirectory |
| `RecordType`     | AzureActiveDirectoryAccountLogon |
| `Application` | 觸發帳戶登入事件的應用程式，例如 Office 15。 |
| `Client` | 帳戶登入事件所使用的用戶端裝置、裝置作業系統及裝置瀏覽器的相關詳細資料。 |
| `LoginStatus` | 此屬性直接來自 OrgIdLogon.LoginStatus。 各種感興趣之登入失敗的對應可由警示演算法完成。 |
| `UserDomain` | 租用戶身分識別資訊 (TII)。 | 


### <a name="azure-active-directory"></a>Azure Active Directory

這些記錄會在針對 Azure Active Directory 物件進行變更或新增時建立。

| 屬性 | 描述 |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AADTarget | 對之執行 (透過 Operation 屬性所識別的) 動作的使用者。 |
| Actor | 執行動作的使用者或服務主體。 |
| ActorContextId | 執行者所屬組織的 GUID。 |
| ActorIpAddress | 以 IPV4 或 IPV6 位址格式顯示的執行者 IP 位址。 |
| InterSystemsId | 在 Office 365 服務內跨元件追蹤動作的 GUID。 |
| IntraSystemId |     由 Azure Active Directory 產生來追蹤動作的 GUID。 |
| SupportTicketId | 在「代表採取動作」狀況下進行之動作的客戶支援票證識別碼。 |
| TargetContextId | 目標使用者所屬組織的 GUID。 |


### <a name="data-center-security"></a>資料中心安全性

這些記錄是從資料中心安全性稽核資料所建立。  

| 屬性 | 描述 |
|:--- |:--- |
| EffectiveOrganization | 提高權限/Cmdlet 的目標租用戶名稱。 |
| ElevationApprovedTime | 核准提高權限時的時間戳記。 |
| ElevationApprover | Microsoft 管理員的名稱。 |
| ElevationDuration | 提高權限作用中的持續時間。 |
| ElevationRequestId |     提高權限要求的唯一識別碼。 |
| ElevationRole | 要求提高權限的角色。 |
| ElevationTime | 提高權限的開始時間。 |
| Start_Time | Cmdlet 開始執行的時間。 |


### <a name="exchange-admin"></a>Exchange 管理員

這些記錄會在對 Exchange 設定做出變更時建立。

| 屬性 | 描述 |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeAdmin |
| ExternalAccess |     指定 Cmdlet 是由貴組織的使用者、由 Microsoft 資料中心人員或資料中心服務帳戶，還是由委派的系統管理員執行。 False 值表示 Cmdlet 是由貴組織的人員執行。 True 值表示 Cmdlet 是由資料中心人員、資料中心服務帳戶或是委派的系統管理員執行。 |
| ModifiedObjectResolvedName |     這是 Cmdlet 所修改之物件的使用者易記名稱。 這只有在 Cmdlet 修改物件時才會記錄。 |
| OrganizationName | 租用戶的名稱。 |
| OriginatingServer | 執行 Cmdlet 之伺服器的名稱。 |
| 參數 | 搭配在 Operations 屬性中所識別的 Cmdlet 所使用之所有參數的名稱與值。 |


### <a name="exchange-mailbox"></a>Exchange 信箱

這些記錄會在針對 Exchange 信箱做出變更或新增時建立。

| 屬性 | 描述 |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| ClientInfoString | 用以執行作業的電子郵件用戶端相關資訊，例如瀏覽器版本、Outlook 版本及行動裝置資訊。 |
| Client_IPAddress | 記錄作業時所使用之裝置的 IP 位址。 IP 位址會以 IPv4 或 IPv6 位址格式顯示。 |
| ClientMachineName | 裝載 Outlook 用戶端的機器名稱。 |
| ClientProcessName | 用來存取信箱的電子郵件用戶端。 |
| ClientVersion | 電子郵件用戶端的版本。 |
| InternalLogonType | 保留供內部使用。 |
| Logon_Type | 指出存取信箱並執行所記錄之作業的使用者類型。 |
| LogonUserDisplayName |     執行作業之使用者的使用者易記名稱。 |
| LogonUserSid | 執行作業之使用者的 SID。 |
| MailboxGuid | 被存取之信箱的 Exchange GUID。 |
| MailboxOwnerMasterAccountSid | 信箱擁有者帳戶的主要帳戶 SID。 |
| MailboxOwnerSid | 信箱擁有者的 SID。 |
| MailboxOwnerUPN | 擁有被存取信箱之人員的電子郵件地址。 |


### <a name="exchange-mailbox-audit"></a>Exchange 信箱稽核

這些記錄會在建立信箱稽核項目時建立。

| 屬性 | 描述 |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| 項目 | 表示對之執行作業的項目 | 
| SendAsUserMailboxGuid | 存取以傳送電子郵件之信箱的 Exchange GUID。 |
| SendAsUserSmtp | 被模擬之使用者的 SMTP 位址。 |
| SendonBehalfOfUserMailboxGuid | 存取以代為傳送電子郵件之信箱的 Exchange GUID。 |
| SendOnBehalfOfUserSmtp | 代表其傳送電子郵件之使用者的 SMTP 位址。 |


### <a name="exchange-mailbox-audit-group"></a>Exchange 信箱稽核群組

這些記錄會在針對 Exchange 群組做出變更或新增時建立。

| 屬性 | 描述 |
|:--- |:--- |
| OfficeWorkload | Exchange |
| OfficeWorkload | ExchangeItemGroup |
| AffectedItems | 群組中每個項目的相關資訊。 |
| CrossMailboxOperations | 指出該作業是否涉及多個信箱。 |
| DestMailboxId | 只有在 CrossMailboxOperations 參數為 True 時才會設定。 指定目標信箱 GUID。 |
| DestMailboxOwnerMasterAccountSid | 只有在 CrossMailboxOperations 參數為 True 時才會設定。 針對目標信箱擁有者的主要帳戶 SID 指定 SID。 |
| DestMailboxOwnerSid | 只有在 CrossMailboxOperations 參數為 True 時才會設定。 指定目標信箱的 SID。 |
| DestMailboxOwnerUPN | 只有在 CrossMailboxOperations 參數為 True 時才會設定。 指定目標信箱之擁有者的 UPN。 |
| DestFolder | 針對移動等作業的目的地資料夾。 |
| 資料夾 | 項目群組所在的資料夾。 |
| 資料夾 |     涉及作業之來源資料夾的相關資訊，例如，在選取資料夾後將它加以刪除。 |


### <a name="sharepoint-base"></a>SharePoint 基底

這些是所有 SharePoint 記錄通用的屬性。

| 屬性 | 描述 |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| EventSource | 識別事件在 SharePoint 中發生。 可能的值為 SharePoint 或 ObjectModel。 |
| ItemType | 被存取或修改之物件的類型。 如需物件類型的詳細資料，請參閱 ItemType 表格。 |
| MachineDomainInfo | 裝置同步作業的相關資訊。 此資訊只會在存在於要求中時才會報告。 |
| MachineId |     裝置同步作業的相關資訊。 此資訊只會在存在於要求中時才會報告。 |
| Site_ | 使用者存取的檔案或資料夾所在之網站的 GUID。 |
| Source_Name | 觸發已稽核作業的實體。 可能的值為 SharePoint 或 ObjectModel。 |
| UserAgent | 使用者的用戶端或瀏覽器的相關資訊。 此資訊是由用戶端或瀏覽器所提供。 |


### <a name="sharepoint-schema"></a>SharePoint 結構描述

這些記錄會在針對 SharePoint 做出設定變更時建立。

| 屬性 | 描述 |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| CustomEvent | 自訂事件的選擇性字串。 |
| Event_Data |     自訂事件的選擇性承載。 |
| ModifiedProperties | 系統會針對管理員事件 (例如，將使用者新增為網站或網站集合管理員群組的成員) 包含此屬性。 此屬性包含被修改之屬性的名稱 (例如「網站管理員」群組)、已修改屬性的新值 (例如新增為網站管理員的使用者)，以及已修改物件先前的值。 |


### <a name="sharepoint-file-operations"></a>SharePoint 檔案作業

這些記錄是為了回應 SharePoint 中的檔案作業而建立。

| 屬性 | 描述 |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePointFileOperation |
| DestinationFileExtension | 被複製或移動之檔案的副檔名。 此屬性僅針對 FileCopied 和 FileMoved 事件顯示。 |
| DestinationFileName | 被複製或移動之檔案的名稱。 此屬性僅針對 FileCopied 和 FileMoved 事件顯示。 |
| DestinationRelativeUrl | 複製或移動檔案之目的地資料夾的 URL。 SiteURL、DestinationRelativeURL 及 DestinationFileName 參數之值的組合，和 ObjectID 屬性的值相同，也就是被複製之檔案的完整路徑名稱。 此屬性僅針對 FileCopied 和 FileMoved 事件顯示。 |
| SharingType | 指派給與之共用資源之使用者的共用權限類型。 此使用者是由 UserSharedWith 參數識別。 |
| Site_Url | 使用者所存取之檔案或資料夾所在的網站 URL。 |
| SourceFileExtension | 使用者所存取之檔案的副檔名。 如果存取的物件是資料夾，此屬性將會是空白。 |
| SourceFileName |     使用者所存取之檔案或資料夾的名稱。 |
| SourceRelativeUrl | 包含使用者所存取之檔案的資料夾 URL。 SiteURL、SourceRelativeURL 及 SourceFileName 參數之值的組合，和 ObjectID 屬性的值相同，也就是使用者所存取之檔案的完整路徑名稱。 |
| UserSharedWith |     與之共用資源的使用者。 |




## <a name="sample-log-queries"></a>範例記錄查詢

下表提供此解決方案所收集之更新記錄的範例記錄查詢。

| 查詢 | 描述 |
| --- | --- |
|Office 365 訂閱上所有作業的計數 |OfficeActivity &#124; summarize count() by Operation |
|SharePoint 網站的使用情況|OfficeActivity &#124; 其中 OfficeWorkload = ~ "sharepoint" &#124; 摘要計數 ( # A2 by 依 \| 計數 asc 的 SiteUrl 排序|
|依使用者類型分類的檔案存取作業 | OfficeActivity &#124; 摘要計數 ( # A1 by UserType |
|監視 Exchange 上的外部動作|OfficeActivity &#124; where OfficeWorkload =~ "exchange" and ExternalAccess == true|



## <a name="next-steps"></a>後續步驟

* 使用 [Azure 監視器中的記錄查詢](../log-query/log-query-overview.md)來檢視詳細的更新資料。
* [建立自己的儀表板](../learn/tutorial-logs-dashboards.md)來顯示您最愛的 Office 365 搜尋查詢。
* [建立警示](../platform/alerts-overview.md)來讓系統主動通知您重要的 Office 365 活動。  
