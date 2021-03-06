---
title: 教學課程︰以 Azure Active Directory 設定 G Suite 來自動佈建使用者 | Microsoft Docs
description: 了解如何將使用者帳戶從 Azure AD 針對 G Suite 進行自動佈建和取消佈建。
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 01/06/2020
ms.author: Zhchia
ms.openlocfilehash: 3f2f62fe158b946e00c7f81d0cb7eeb0d8f09437
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91331122"
---
# <a name="tutorial-configure-g-suite-for-automatic-user-provisioning"></a>教學課程︰設定 G Suite 來自動佈建使用者

本教學課程說明您需要在 G Suite 和 Azure Active Directory (Azure AD) 中執行的步驟，以設定自動使用者布建。 當設定時，Azure AD 會使用 Azure AD 布建服務，自動將使用者和群組布建並取消布建至 [G Suite](https://gsuite.google.com/) 。 如需此服務的用途、運作方式和常見問題等重要詳細資訊，請參閱[使用 Azure Active Directory 對 SaaS 應用程式自動佈建和取消佈建使用者](../manage-apps/user-provisioning.md)。 

> [!NOTE]
> 本教學課程會說明建置在 Azure AD 使用者佈建服務之上的連接器。 如需此服務的用途、運作方式和常見問題等重要詳細資訊，請參閱[使用 Azure Active Directory 對 SaaS 應用程式自動佈建和取消佈建使用者](../app-provisioning/user-provisioning.md)。

> [!NOTE]
> G Suite 連接器最近已于2019年10月更新。 對 G Suite 連接器所做的變更包括：
>
> * 已新增其他 G Suite 使用者和群組屬性的支援。
> * 已更新 G Suite 目標屬性名稱，以符合 [此處](https://developers.google.com/admin-sdk/directory)所定義的名稱。
> * 已更新預設屬性對應。

## <a name="capabilities-supported"></a>支援的功能
> [!div class="checklist"]
> * 在 G Suite 中建立使用者
> * 不再需要存取時，請移除 G Suite 中的使用者
> * 在 Azure AD 與 G Suite 之間保持使用者屬性同步
> * 在 G Suite 中布建群組和群組成員資格
> *  (建議) [單一登入](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-tutorial)G Suite

## <a name="prerequisites"></a>必要條件

本教學課程中概述的案例假設您已經具有下列必要條件：

* [Azure AD 租用戶](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 中具有設定佈建[權限](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)的使用者帳戶 (例如，應用程式管理員、雲端應用程式管理員、應用程式擁有者或全域管理員)。 
* [G Suite 租使用者](https://gsuite.google.com/pricing.html)
* G Suite 上具有系統管理員許可權的使用者帳戶。

## <a name="step-1-plan-your-provisioning-deployment"></a>步驟 1： 規劃佈建部署
1. 了解[佈建服務的運作方式](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) \(部分機器翻譯\)。
2. 判斷誰會在[佈建範圍](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)內。
3. 判斷要 [在 Azure AD 與 G Suite 之間對應](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)的資料。 

## <a name="step-2-configure-g-suite-to-support-provisioning-with-azure-ad"></a>步驟 2： 設定 G Suite 以支援使用 Azure AD 布建

使用 Azure AD 設定 G Suite 來自動布建使用者之前，您必須啟用 G Suite 上的 SCIM 布建。

1. 使用您的系統管理員帳戶登入 [G Suite 管理主控台](https://admin.google.com/) ，然後選取 [ **安全性**]。 如果您沒有看到連結，它可能隱藏在畫面底部的 [其他控制項]**** 功能表之下。

    ![G Suite 安全性](./media/google-apps-provisioning-tutorial/gapps-security.png)

2. 在 [安全性]**** 頁面上，選取 [API 參考]****。

    ![G Suite API](./media/google-apps-provisioning-tutorial/gapps-api.png)

3. 選取 [啟用 API 存取]****。

    ![G Suite API 已啟用](./media/google-apps-provisioning-tutorial/gapps-api-enabled.png)

    > [!IMPORTANT]
   > 針對您想要布建至 G Suite 的每個使用者，Azure AD 中的使用者名稱 **必須** 系結至自訂網域。 例如，G Suite 不會接受類似 bob@contoso.onmicrosoft.com 的使用者名稱。 另一方面，則接受 bob@contoso.com。 您可以遵循 [此處](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain)的指示來變更現有使用者的網域。

4. 當您使用 Azure AD 新增並驗證所需的自訂網域之後，您必須使用 G Suite 再次驗證。 若要確認 G Suite 中的網域，請參閱下列步驟：

    a. 在 [ [G Suite 管理主控台](https://admin.google.com/)] 中，選取 [ **網域**]。

    ![G Suite 網域](./media/google-apps-provisioning-tutorial/gapps-domains.png)

    b. 選取 [新增網域或網域別名]****。

    ![G Suite 新增網域](./media/google-apps-provisioning-tutorial/gapps-add-domain.png)

    c. 選取 [新增另一個網域]****，然後輸入您想要新增的網域名稱。

    ![G Suite 新增另一個](./media/google-apps-provisioning-tutorial/gapps-add-another.png)

    d. 選取 [繼續並驗證網域擁有權]****。 然後依照步驟以驗證您擁有網域名稱。 如需有關如何使用 Google 驗證網域的完整指示，請參閱 [驗證網站擁有權](https://support.google.com/webmasters/answer/35179)。

    e. 針對您想要新增至 G Suite 的任何其他網域，重複上述步驟。

5. 接下來，請決定您要在 G Suite 中用來管理使用者布建的系統管理員帳戶。 流覽至 [ **管理員角色**]。

    ![G Suite 管理員](./media/google-apps-provisioning-tutorial/gapps-admin.png)

6. 若為該帳戶的系統 **管理員角色** ，請編輯該角色的 **許可權** 。 務必啟用所有 [管理 API 權限]****，以便讓此帳戶可以用來佈建。

    ![G Suite 系統管理員許可權](./media/google-apps-provisioning-tutorial/gapps-admin-privileges.png)

## <a name="step-3-add-g-suite-from-the-azure-ad-application-gallery"></a>步驟 3： 從 Azure AD 應用程式資源庫新增 G Suite

從 Azure AD 應用程式資源庫新增 G Suite，以開始管理對 G Suite 的布建。 如果您先前已設定適用于 SSO 的 G Suite，您可以使用相同的應用程式。 不過，建議您在一開始測試整合時，建立個別的應用程式。 [在此](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)深入了解從資源庫新增應用程式。 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>步驟 4： 定義將在佈建範圍內的人員 

Azure AD 佈建服務可供根據對應用程式的指派，或根據使用者/群組的屬性，界定將要佈建的人員。 如果您選擇根據指派來界定將佈建至應用程式的人員，您可以使用下列[步驟](../manage-apps/assign-user-or-group-access-portal.md)將使用者和群組指派給應用程式。 如果您選擇僅根據使用者或群組的屬性來界定將要佈建的人員，可以使用如[這裡](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)所述的範圍篩選條件。 

* 將使用者和群組指派給 G Suite 時，您必須選取 **預設存取**以外的角色。 具有預設存取角色的使用者會從佈建中排除，而且會在佈建記錄中被標示為沒有效率。 如果應用程式上唯一可用的角色是 [預設存取] 角色，您可以[更新應用程式資訊清單](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) \(部分機器翻譯\) 以新增其他角色。 

* 從小規模開始。 在推出給所有人之前，先使用一小部分的使用者和群組進行測試。 當佈建範圍設為已指派的使用者和群組時，您可將一或兩個使用者或群組指派給應用程式來控制這點。 當範圍設為所有使用者和群組時，您可指定[以屬性為基礎的範圍篩選條件](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)。 


## <a name="step-5-configure-automatic-user-provisioning-to-g-suite"></a>步驟 5。 設定 G Suite 的自動使用者布建 

此節將引導您逐步設定 Azure AD 佈建服務，以根據 Azure AD 中的使用者和/或群組指派，在 TestApp 中建立、更新和停用使用者和/或群組。

> [!NOTE]
> 若要深入瞭解 G Suite 的目錄 API 端點，請參閱 [目錄 api](https://developers.google.com/admin-sdk/directory)。

### <a name="to-configure-automatic-user-provisioning-for-g-suite-in-azure-ad"></a>若要在 Azure AD 中設定 G Suite 的自動使用者布建：

1. 登入 [Azure 入口網站](https://portal.azure.com)。 選取 [企業應用程式]，然後選取 [所有應用程式]。 使用者將需要登入 portal.azure.com，而且將無法使用 aad.portal.azure.com

    ![企業應用程式刀鋒視窗](./media/google-apps-provisioning-tutorial/enterprise-applications.png)

    ![所有應用程式刀鋒視窗](./media/google-apps-provisioning-tutorial/all-applications.png)

2. 在應用程式清單中，選取 [G Suite]****。

    ![應用程式清單中的 G Suite 連結](common/all-applications.png)

3. 選取 [佈建] 索引標籤。按一下 [開始使用]。

    ![已呼叫 [布建] 選項的 [管理選項] 螢幕擷取畫面。](common/provisioning.png)

      ![[開始使用] 刀鋒視窗](./media/google-apps-provisioning-tutorial/get-started.png)

4. 將 [佈建模式] 設定為 [自動]。

    ![[布建模式] 下拉式清單的螢幕擷取畫面，其中已呼叫 [自動] 選項。](common/provisioning-automatic.png)

5. 在 [ **管理員認證** ] 區段下，按一下 [ **授權**]。 系統會在新的瀏覽器視窗中，將您重新導向至 Google 授權對話方塊。

      ![G Suite 授權](./media/google-apps-provisioning-tutorial/authorize-1.png)

6. 確認您想要授與對 G Suite 租使用者進行變更的 Azure AD 許可權。 選取 [接受]。

     ![G Suite 租使用者驗證](./media/google-apps-provisioning-tutorial/gapps-auth.png)

7. 在 Azure 入口網站中，按一下 [ **測試連接** ]，以確保 Azure AD 可以連接到 G Suite。 如果連接失敗，請確定您的 G Suite 帳戶具有系統管理員許可權，然後再試一次。 然後再試一次**授權**步驟。

6. 在 [通知電子郵件] 欄位中，輸入應該收到佈建錯誤通知的個人或群組電子郵件地址，然後選取 [發生失敗時傳送電子郵件通知] 核取方塊。

    ![通知電子郵件](common/provisioning-notification-email.png)

7. 選取 [儲存]。

8. 在 [對應] 區段底下，選取 [佈建 Azure Active Directory 使用者]。

9. 在 [ **屬性對應** ] 區段中，檢查從 Azure AD 同步處理至 G Suite 的使用者屬性。 選取為 [比對] 屬性 **的屬性會** 用來比對 G Suite 中的使用者帳戶以進行更新作業。 如果您選擇變更相符的 [目標屬性](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)，您將需要確定 G Suite API 支援根據該屬性篩選使用者。 選取 [儲存] 按鈕以認可所有變更。

   |屬性|類型|
   |---|---|
   |primaryEmail|字串|
   |關係。[type eq "manager"]。值|String|
   |name.familyName|String|
   |name.givenName|String|
   |暫止|字串|
   |externalid.[type eq "custom"]. 值|字串|
   |externalid.[type eq "organization"]。值|字串|
   |位址。[type eq "work"]。國家/地區|字串|
   |位址。[type eq "work"]. streetAddress|字串|
   |位址。[type eq "work"]. region|字串|
   |位址。[type eq "work"]。位置|字串|
   |位址。[type eq "work"]. 郵遞區號|字串|
   |電子郵件。[type eq "work"]。位址|字串|
   |組織。[type eq "work"]. 部門|字串|
   |組織。[type eq "work"]. 標題|字串|
   |phoneNumbers.[type eq "work"]。值|字串|
   |phoneNumbers.[type eq "mobile"]。值|字串|
   |phoneNumbers.[type eq "work_fax"]. 值|字串|
   |電子郵件。[type eq "work"]。位址|字串|
   |組織。[type eq "work"]. 部門|字串|
   |組織。[type eq "work"]. 標題|字串|
   |phoneNumbers.[type eq "work"]。值|字串|
   |phoneNumbers.[type eq "mobile"]。值|字串|
   |phoneNumbers.[type eq "work_fax"]. 值|字串|
   |位址。[type eq "home"]. country|字串|
   |位址。[type eq "home"]。格式化|字串|
   |位址。[type eq "home"]。位置|字串|
   |位址。[type eq "home"]. 郵遞區號|字串|
   |位址。[type eq "home"]. region|字串|
   |位址。[type eq "home"]. streetAddress|字串|
   |位址。[type eq "other"]。國家/地區|字串|
   |位址。[type eq "other"]。格式化|字串|
   |位址。[type eq "other"]。位置|字串|
   |位址。[type eq "other"]. 郵遞區號|字串|
   |位址。[type eq "other"]. region|字串|
   |位址。[type eq "other"]. streetAddress|字串|
   |位址。[type eq "work"]。已格式化|字串|
   |changePasswordAtNextLogin|字串|
   |電子郵件。[type eq "home"]. 位址|字串|
   |電子郵件。[type eq "other"]. 位址|字串|
   |externalid.[type eq "account"]。值|字串|
   |externalid.[type eq "custom"]. customType|字串|
   |externalid.[type eq "customer"]。值|字串|
   |externalid.[type eq "login_id"]. 值|字串|
   |externalid.[type eq "network"]。值|字串|
   |性別. 類型|字串|
   |GeneratedImmutableId|字串|
   |識別碼|字串|
   |Ims。[type eq "home"]. protocol|字串|
   |Ims。[type eq "other"]. protocol|字串|
   |Ims。[type eq "work"]. protocol|字串|
   |includeInGlobalAddressList|字串|
   |ipWhitelisted|字串|
   |組織。[type eq "school"]. costCenter|字串|
   |組織。[type eq "school"]. 部門|字串|
   |組織。[type eq "school"]. 網域|字串|
   |組織。[type eq "school"]. fullTimeEquivalent|字串|
   |組織。[type eq "school"]。位置|字串|
   |組織。[type eq "school"]。名稱|字串|
   |組織。[type eq "school"]. 符號|字串|
   |組織。[type eq "school"]. 標題|字串|
   |組織。[type eq "work"]. costCenter|字串|
   |組織。[type eq "work"]。網域|字串|
   |組織。[type eq "work"]. fullTimeEquivalent|字串|
   |組織。[type eq "work"]。位置|字串|
   |組織。[type eq "work"]。名稱|字串|
   |組織。[type eq "work"]. 符號|字串|
   |OrgUnitPath|字串|
   |phoneNumbers.[type eq "home"]. 值|字串|
   |phoneNumbers.[type eq "other"]. 值|字串|
   |網站。[type eq "home"]. 值|字串|
   |網站。[type eq "other"]. 值|字串|
   |網站。[type eq "work"]。值|字串|
   

10. **在 [對應**] 區段下，選取 [布建**Azure Active Directory 群組**]。

11. 在 [ **屬性對應** ] 區段中，檢查從 Azure AD 同步處理至 G Suite 的群組屬性。 選取為 [比對] 屬性 **的屬性會** 用來比對 G Suite 中的群組以進行更新作業。 選取 [儲存] 按鈕以認可所有變更。

      |屬性|類型|
      |---|---|
      |電子郵件|字串|
      |成員|字串|
      |NAME|String|
      |description|String|

12. 若要設定範圍篩選，請參閱[範圍篩選教學課程](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)中提供的下列指示。

13. 若要啟用 G Suite 的 Azure AD 布建服務，請在 [**設定**] 區段中，將 [布建**狀態**] 變更為 [**開啟**]。

    ![佈建狀態已切換為開啟](common/provisioning-toggle-on.png)

14. 在 [**設定**] 區段的 [**範圍**] 中選擇所需的值，以定義您想要布建至 G Suite 的使用者和/或群組。

    ![佈建範圍](common/provisioning-scope.png)

15. 當您準備好要佈建時，按一下 [儲存]。

    ![儲存雲端佈建設定](common/provisioning-configuration-save.png)

此作業會對在 [設定] 區段的 [範圍] 中所定義所有使用者和群組啟動首次同步處理週期。 初始週期會比後續週期花費更多時間執行，只要 Azure AD 佈建服務正在執行，這大約每 40 分鐘便會發生一次。

> [!NOTE]
> 如果使用者已經有使用 Azure AD 使用者電子郵件地址的現有個人/取用者帳戶，則可能會在執行目錄同步作業之前，先使用 Google Transfer Tool 解決一些問題。

## <a name="step-6-monitor-your-deployment"></a>步驟 6. 監視您的部署
設定佈建後，請使用下列資源來監視您的部署：

1. 使用[佈建記錄](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) \(部分機器翻譯\) 來判斷哪些使用者已佈建成功或失敗
2. 檢查[進度列](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) \(部分機器翻譯\) 來查看佈建週期的狀態，以及其接近完成的程度
3. 如果佈建設定似乎處於狀況不良的狀態，應用程式將會進入隔離狀態。 [在此](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)深入了解隔離狀態。

## <a name="additional-resources"></a>其他資源

* [管理企業應用程式的使用者帳戶佈建](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>後續步驟

* [瞭解如何針對佈建活動檢閱記錄和取得報告](../manage-apps/check-status-user-account-provisioning.md)
