---
title: 教學課程：使用 Azure Active Directory 設定 iProva 來自動布建使用者 |Microsoft Docs
description: 瞭解如何設定 Azure Active Directory，以將使用者帳戶自動布建和取消布建至 iProva。
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 10/29/2019
ms.author: Zhchia
ms.openlocfilehash: 1dd89198fd1c494ef3002fdc222683f56f2d7e3a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91304680"
---
# <a name="tutorial-configure-iprova-for-automatic-user-provisioning"></a>教學課程：設定 iProva 來自動布建使用者

本教學課程的目的是要示範在 iProva 中執行的步驟，以及 Azure Active Directory (Azure AD) 將 Azure AD 設定為自動布建和解除布建使用者和/或群組至 [iProva](https://www.iProva.com/)。 如需此服務的用途、運作方式和常見問題等重要詳細資訊，請參閱[使用 Azure Active Directory 對 SaaS 應用程式自動佈建和取消佈建使用者](../manage-apps/user-provisioning.md)。 

> [!NOTE]
> 此連接器目前為公開預覽版。 如需有關預覽功能的一般 Microsoft Azure 使用規定詳細資訊，請參閱 [Microsoft Azure 預覽版增補使用規定](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。


## <a name="capabilities-supported"></a>支援的功能
> [!div class="checklist"]
> * 在 iProva 中建立使用者
> * 當使用者不再需要存取權時，請移除 iProva 中的使用者
> * Azure AD 與 iProva 之間保持使用者屬性同步
> * 在 iProva 中布建群組和群組成員資格
> * IProva (建議的[單一登入](https://docs.microsoft.com/azure/active-directory/saas-apps/iprova-tutorial)) 

## <a name="prerequisites"></a>必要條件

本教學課程中概述的案例假設您已經具有下列必要條件：

* [Azure AD 租用戶](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)。
* Azure AD 中具有設定佈建[權限](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)的使用者帳戶 (例如，應用程式管理員、雲端應用程式管理員、應用程式擁有者或全域管理員)。
* [IProva 租](https://www.iProva.com/)使用者。
* IProva 中具有系統管理員許可權的使用者帳戶。

## <a name="step-1-plan-your-provisioning-deployment"></a>步驟 1： 規劃佈建部署
1. 了解[佈建服務的運作方式](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) \(部分機器翻譯\)。
2. 判斷誰會在[佈建範圍](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)內。
3. 判斷要 [在 Azure AD 與 iProva 之間對應](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)的資料。 

## <a name="step-2-configure-iprova-to-support-provisioning-with-azure-ad"></a>步驟 2： 設定 iProva 以支援 Azure AD 的布建

1. 登入您的 [IProva 管理主控台](https://www.iProva.com/)。 流覽至 [ **移至 > 應用程式管理**]。

    ![iProva 管理主控台](media/iprova-provisioning-tutorial/admin.png)

2.  按一下 [ **外部使用者管理**]。

    ![iProva 新增 SCIM](media/iprova-provisioning-tutorial/external.png)

3. 若要加入新的提供者，請按一下 **加號** 圖示。 在 [新增 **加入提供者** ] 對話方塊中，提供 **標題**。 您可以選擇新增以 **IP 為基礎的存取限制**。 按一下 **[確定]** 按鈕。

    ![iProva 新增](media/iprova-provisioning-tutorial/add.png)

    ![iProva 加入提供者](media/iprova-provisioning-tutorial/addprovider.png)

4.  按一下 [ **永久權杖** ] 按鈕。 複製 **永久權杖** 並加以儲存，因為這會是您唯一可以看到的權杖。 在 Azure 入口網站的 iProva 應用程式的 [布建] 索引標籤中，將會在 [秘密權杖] 欄位中輸入此值。

    ![iProva 建立權杖](media/iprova-provisioning-tutorial/token.png)

## <a name="step-3-add-iprova-from-the-azure-ad-application-gallery"></a>步驟 3： 從 Azure AD 應用程式資源庫新增 iProva

從 Azure AD 應用程式資源庫新增 iProva，以開始管理布建至 iProva。 如果您先前已設定 SSO 的 iProva，您可以使用相同的應用程式。 不過，建議您在一開始測試整合時，建立個別的應用程式。 [在此](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)深入了解從資源庫新增應用程式。 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>步驟 4： 定義將在佈建範圍內的人員 

Azure AD 佈建服務可供根據對應用程式的指派，或根據使用者/群組的屬性，界定將要佈建的人員。 如果您選擇根據指派來界定將佈建至應用程式的人員，您可以使用下列[步驟](../manage-apps/assign-user-or-group-access-portal.md)將使用者和群組指派給應用程式。 如果您選擇僅根據使用者或群組的屬性來界定將要佈建的人員，可以使用如[這裡](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)所述的範圍篩選條件。 

* 將使用者和群組指派給 iProva 時，您必須選取 **預設存取**以外的角色。 具有預設存取角色的使用者會從佈建中排除，而且會在佈建記錄中被標示為沒有效率。 如果應用程式上唯一可用的角色是 [預設存取] 角色，您可以[更新應用程式資訊清單](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) \(部分機器翻譯\) 以新增其他角色。 

* 從小規模開始。 在推出給所有人之前，先使用一小部分的使用者和群組進行測試。 當佈建範圍設為已指派的使用者和群組時，您可將一或兩個使用者或群組指派給應用程式來控制這點。 當範圍設為所有使用者和群組時，您可指定[以屬性為基礎的範圍篩選條件](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)。 

## <a name="step-5-configure-automatic-user-provisioning-to-iprova"></a>步驟 5。 設定自動使用者布建至 iProva 

本節將引導您逐步設定 Azure AD 布建服務，以根據 Azure AD 中的使用者和/或群組指派，在 iProva 中建立、更新及停用使用者和/或群組。

### <a name="to-configure-automatic-user-provisioning-for-iprova-in-azure-ad"></a>若要在 Azure AD 中為 iProva 設定自動使用者布建：

1. 登入 [Azure 入口網站](https://portal.azure.com)。 選取 [企業應用程式]，然後選取 [所有應用程式]。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取[iProva]****。

    ![應用程式清單中的 iProva 連結](common/all-applications.png)

3. 選取 [佈建] 索引標籤。

    ![已呼叫 [布建] 選項的 [管理選項] 螢幕擷取畫面。](common/provisioning.png)

4. 將 [佈建模式] 設定為 [自動]。

    ![[布建模式] 下拉式清單的螢幕擷取畫面，其中已呼叫 [自動] 選項。](common/provisioning-automatic.png)

5. 在 [系統**管理員認證**] 區段底下，分別輸入 [**租使用者 URL** ] 和 [**秘密權杖**] 欄位中先前抓取的**SCIM 2.0 基底 url 和永久權杖**值。 按一下 [ **測試連接** ] 以確保 Azure AD 可以連線至 iProva。 如果連接失敗，請確定您的 iProva 帳戶具有系統管理員許可權，然後再試一次。

    ![租用戶 URL + 權杖](common/provisioning-testconnection-tenanturltoken.png)

6. 在 [通知電子郵件]**** 欄位中，輸入應該收到佈建錯誤通知的個人或群組電子郵件地址，然後選取 [發生失敗時傳送電子郵件通知]**** 核取方塊。

    ![通知電子郵件](common/provisioning-notification-email.png)

7. 按一下 **[儲存]** 。

8. **在 [對應**] 區段下，選取 [**同步處理 Azure Active Directory 使用者至 iProva**]。

9. 在 [ **屬性對應** ] 區段中，檢查從 Azure AD 同步處理到 iProva 的使用者屬性。 選取為 [比對 **] 屬性的屬性會** 用來比對 iProva 中的使用者帳戶以進行更新作業。 選取 [儲存] 按鈕以認可所有變更。

   |屬性|類型|
   |---|---|
   |作用中|Boolean|
   |displayName|String|
   |title|String|
   |emails[type eq "work"].value|String|
   |preferredLanguage|String|
   |userName|String|
   |addresses[type eq "work"].country|String|
   |addresses[type eq "work"].locality|String|
   |addresses[type eq "work"].postalCode|String|
   |addresses[type eq "work"].formatted|String|
   |addresses[type eq "work"].region|String|
   |addresses[type eq "work"].streetAddress|String|
   |位址 [type eq "other"]。已格式化|String|
   |name.givenName|String|
   |name.familyName|String|
   |name.formatted|String|
   |phoneNumbers[type eq "fax"].value|String|
   |phoneNumbers[type eq "mobile"].value|String|
   |phoneNumbers[type eq "work"].value|String|
   |externalId|String|
   |角色 [主要 eq "True"]。顯示|字串|
   |角色 [主要 eq "True"]。類型|String|
   |roles[primary eq "True"].value|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:division|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:organization|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:employeeNumber|String|


10. **在 [對應**] 區段下，選取 [**同步處理 Azure Active Directory 群組至 iProva**]。

11. 在 [ **屬性對應** ] 區段中，檢查從 Azure AD 同步處理到 iProva 的群組屬性。 選取為 [比對 **] 屬性的屬性會** 用來比對 iProva 中的群組以進行更新作業。 選取 [儲存] 按鈕以認可所有變更。

      |屬性|類型|
      |---|---|
      |displayName|String|
      |members|參考|

12. 若要設定範圍篩選，請參閱[範圍篩選教學課程](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)中提供的下列指示。

13. 若要啟用 iProva Azure AD 的布建服務，請在 [**設定**] 區段中，將 [布建**狀態**] 變更為 [**開啟**]。

    ![佈建狀態已切換為開啟](common/provisioning-toggle-on.png)

14. 在 [**設定**] 區段的 [**範圍**] 中選擇所需的值，以定義您想要布建到 iProva 的使用者和/或群組。

    ![佈建範圍](common/provisioning-scope.png)

15. 當您準備好要佈建時，按一下 [儲存]。

    ![儲存雲端佈建設定](common/provisioning-configuration-save.png)

此作業會對在 [設定]**** 區段的 [範圍]**** 中定義的所有使用者和/或群組，啟動首次同步處理。 初始同步處理會比後續同步處理花費更多時間執行，只要 Azure AD 佈建服務正在執行，這大約每 40 分鐘便會發生一次。 


## <a name="step-6-monitor-your-deployment"></a>步驟 6. 監視您的部署
設定佈建後，請使用下列資源來監視您的部署：

1. 使用[佈建記錄](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) \(部分機器翻譯\) 來判斷哪些使用者已佈建成功或失敗
2. 檢查[進度列](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) \(部分機器翻譯\) 來查看佈建週期的狀態，以及其接近完成的程度
3. 如果佈建設定似乎處於狀況不良的狀態，應用程式將會進入隔離狀態。 [在此](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)深入了解隔離狀態。  

## <a name="change-log"></a>變更記錄

* 06/17/2020-已移除企業延伸模組屬性 "Manager"。

## <a name="additional-resources"></a>其他資源

* [管理企業應用程式的使用者帳戶佈建](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>後續步驟

* [瞭解如何針對佈建活動檢閱記錄和取得報告](../app-provisioning/check-status-user-account-provisioning.md)

