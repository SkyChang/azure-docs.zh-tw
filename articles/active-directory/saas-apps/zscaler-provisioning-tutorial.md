---
title: 教學課程：使用 Azure Active Directory 設定 Zscaler 來自動布建使用者 |Microsoft Docs
description: 瞭解如何設定 Azure Active Directory，以將使用者帳戶自動布建和取消布建至 Zscaler。
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 52c18f8d51f18b9bc167a99fbafda2365824dfc9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91312068"
---
# <a name="tutorial-configure-zscaler-for-automatic-user-provisioning"></a>教學課程：設定 Zscaler 來自動布建使用者

本教學課程的目的是要示範在 Zscaler 中執行的步驟，以及 Azure Active Directory (Azure AD) 將 Azure AD 設定為自動布建和解除布建使用者和/或群組至 Zscaler。

> [!NOTE]
> 本教學課程會說明建置在 Azure AD 使用者佈建服務之上的連接器。 如需此服務的用途、運作方式和常見問題等重要詳細資訊，請參閱[使用 Azure Active Directory 對 SaaS 應用程式自動佈建和取消佈建使用者](../active-directory-saas-app-provisioning.md)。
>

## <a name="prerequisites"></a>必要條件

本教學課程中說明的案例假設您已經具有下列項目：

* Azure AD 租用戶。
* Zscaler 租使用者。
* Zscaler 中具有系統管理員許可權的使用者帳戶。

> [!NOTE]
> Azure AD 布建整合依賴 Zscaler SCIM API，可供 Zscaler 開發人員使用企業套件的帳戶使用。

## <a name="adding-zscaler-from-the-gallery"></a>從資源庫新增 Zscaler

使用 Azure AD 設定 Zscaler 來自動布建使用者之前，您必須從 Azure AD 應用程式資源庫將 Zscaler 新增至受控 SaaS 應用程式清單。

**若要從 Azure AD 應用程式庫新增 Zscaler，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory]  圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]  ，然後選取 [所有應用程式]  選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式]  按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中輸入 **Zscaler**，並從結果面板中選取 [Zscaler]****，然後按一下 [新增]**** 按鈕以新增應用程式。

    ![結果清單中的 Zscaler](common/search-new-app.png)

## <a name="assigning-users-to-zscaler"></a>將使用者指派給 Zscaler

Azure Active Directory 會使用稱為「指派」的概念，來判斷哪些使用者應接收對指定應用程式的存取權。 在自動使用者佈建的內容中，只有「已指派」至 Azure AD 中的應用程式之使用者和/或群組會進行同步處理。

在設定並啟用自動使用者布建之前，您應該決定 Azure AD 中的哪些使用者和/或群組需要存取 Zscaler。 一旦決定後，您可以遵循此處的指示，將這些使用者和/或群組指派給 Zscaler：

* [將使用者或群組指派給企業應用程式](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-zscaler"></a>將使用者指派給 Zscaler 的重要秘訣

* 建議將單一 Azure AD 使用者指派給 Zscaler，以測試自動使用者布建設定。 其他使用者及/或群組可能會稍後再指派。

* 將使用者指派給 Zscaler 時，您必須在 [指派] 對話方塊中選取任何有效的應用程式特定角色 (如果有的話) 。 具有**預設存取**角色的使用者會從佈建中排除。

## <a name="configuring-automatic-user-provisioning-to-zscaler"></a>設定自動使用者布建至 Zscaler

本節將引導您逐步設定 Azure AD 布建服務，以根據 Azure AD 中的使用者和/或群組指派，在 Zscaler 中建立、更新及停用使用者和/或群組。

> [!TIP]
> 您也可以選擇啟用 Zscaler 的 SAML 型單一登入，請遵循 [Zscaler 單一登入教學](zscaler-tutorial.md)課程中提供的指示。 雖然自動使用者佈建和單一登入這兩個功能互相補充，您還是可以將它們分開設定。

### <a name="to-configure-automatic-user-provisioning-for-zscaler-in-azure-ad"></a>若要在 Azure AD 中為 Zscaler 設定自動使用者布建：

1. 登入 [Azure 入口網站](https://portal.azure.com) 並選取 [ **企業應用程式**]，選取 [ **所有應用程式**]，然後選取 [ **Zscaler**]。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [Zscaler]****。

    ![應用程式清單中的 Zscaler 連結](common/all-applications.png)

3. 選取 [佈建] 索引標籤。

    ![已反白顯示布建選項的 [Zscaler-布建企業應用程式] 提要欄位螢幕擷取畫面。](./media/zscaler-provisioning-tutorial/provisioning-tab.png)

4. 將 [佈建模式] 設定為 [自動]。

    ![布建模式設定為 [自動] 的 [布建] 頁面螢幕擷取畫面。](./media/zscaler-provisioning-tutorial/provisioning-credentials.png)

5. 在 [系統 **管理員認證** ] 區段下，輸入 Zscaler 帳戶的 **租使用者 URL** 和 **秘密權杖** ，如步驟6所述。

6. 若要取得**租使用者 URL**和**秘密權杖**，請在 Zscaler 入口網站使用者介面中流覽至 [**管理 > 驗證設定**]，然後按一下 [**驗證類型**] 下的 [ **SAML** ]。

    ![[驗證設定] 頁面的螢幕擷取畫面。](./media/zscaler-provisioning-tutorial/secret-token-1.png)

    按一下 [ **設定 saml** ] 以開啟 [設定 **saml** 選項]。

    ![[設定 S] 對話方塊的螢幕擷取畫面，其中已將基底 U R L 和持有人權杖的文字方塊稱為 out。](./media/zscaler-provisioning-tutorial/secret-token-2.png)

    選取 [ **啟用 SCIM-Based** 布建] 以取出 **基底 URL** 和 **持有人權杖**，然後儲存設定。 將 **基底 url** 複製 **到租使用者 url**，並將持有人 **權杖**  複製到 Azure 入口網站中的 **秘密權杖** 。

7. 填入步驟5所示的欄位後，按一下 [ **測試連接** ] 以確保 Azure AD 可以連線至 Zscaler。 如果連接失敗，請確定您的 Zscaler 帳戶具有系統管理員許可權，然後再試一次。

    ![[系統管理員認證] 區段的螢幕擷取畫面，其中已呼叫 [測試連接] 選項。](./media/zscaler-provisioning-tutorial/test-connection.png)

8. 在 [通知電子郵件]**** 欄位中，輸入應收到佈建錯誤通知的個人或群組之電子郵件地址，然後勾選 [發生失敗時傳送電子郵件通知]**** 核取方塊。

    ![通知電子郵件文字方塊的螢幕擷取畫面。](./media/zscaler-provisioning-tutorial/notification.png)

9. 按一下 **[儲存]** 。

10. **在 [對應**] 區段下，選取 [**同步處理 Azure Active Directory 使用者至 Zscaler**]。

    ![反白顯示 [同步處理 Azure Active Directory 使用者至 Zscaler] 選項的 [對應] 區段螢幕擷取畫面。](./media/zscaler-provisioning-tutorial/user-mappings.png)

11. 在 [ **屬性對應** ] 區段中，檢查從 Azure AD 同步處理到 Zscaler 的使用者屬性。 選取為 [比對 **] 屬性的屬性會** 用來比對 Zscaler 中的使用者帳戶以進行更新作業。 選取 [儲存] 按鈕以認可所有變更。

    ![[屬性對應] 區段的螢幕擷取畫面，其中顯示七個對應。](./media/zscaler-provisioning-tutorial/user-attribute-mappings.png)

12. **在 [對應**] 區段下，選取 [**同步處理 Azure Active Directory 群組至 Zscaler**]。

    ![醒目提示 [同步處理 Azure Active Directory 群組至 Zscaler] 選項的 [對應] 區段螢幕擷取畫面。](./media/zscaler-provisioning-tutorial/group-mappings.png)

13. 在 [ **屬性對應** ] 區段中，檢查從 Azure AD 同步處理到 Zscaler 的群組屬性。 選取為 [比對 **] 屬性的屬性會** 用來比對 Zscaler 中的群組以進行更新作業。 選取 [儲存] 按鈕以認可所有變更。

    ![[屬性對應] 區段的螢幕擷取畫面，其中顯示三個對應。](./media/zscaler-provisioning-tutorial/group-attribute-mappings.png)

14. 若要設定範圍篩選，請參閱[範圍篩選教學課程](./../active-directory-saas-scoping-filters.md)中提供的下列指示。

15. 若要啟用 Zscaler Azure AD 的布建服務，請在 [**設定**] 區段中，將 [布建**狀態**] 變更為 [**開啟**]。

    ![將 [布建狀態] 選項設為 [開啟] 的螢幕擷取畫面。](./media/zscaler-provisioning-tutorial/provisioning-status.png)

16. 在 [**設定**] 區段的 [**範圍**] 中選擇所需的值，以定義您想要布建到 Zscaler 的使用者和/或群組。

    ![醒目提示 [僅同步已指派的使用者和群組] 選項之 [領域] 設定的螢幕擷取畫面。](./media/zscaler-provisioning-tutorial/scoping.png)

17. 當您準備好要佈建時，按一下 [儲存]。

    ![[Zscaler-布建企業應用程式] 提要欄位的螢幕擷取畫面，其中已呼叫 [儲存] 選項。](./media/zscaler-provisioning-tutorial/save-provisioning.png)

此作業會對在 [設定]**** 區段的 [範圍]**** 中定義的所有使用者和/或群組，啟動首次同步處理。 初始同步處理會比後續同步處理花費更多時間執行，只要 Azure AD 佈建服務正在執行，這大約每 40 分鐘便會發生一次。 您可以使用 [ **同步處理詳細資料** ] 區段來監視進度，並遵循連結來布建活動報告，該報告描述 Zscaler 上的 Azure AD 布建服務所執行的所有動作。

如需如何讀取 Azure AD 佈建記錄的詳細資訊，請參閱[關於使用者帳戶自動佈建的報告](../active-directory-saas-provisioning-reporting.md)。

## <a name="additional-resources"></a>其他資源

* [管理企業應用程式的使用者帳戶佈建](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>後續步驟

* [瞭解如何針對佈建活動檢閱記錄和取得報告](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-provisioning-tutorial/tutorial-general-03.png
