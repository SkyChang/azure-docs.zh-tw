---
title: 教學課程：Azure Active Directory 與 dmarcian 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 dmarcian 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/01/2019
ms.author: jeedes
ms.openlocfilehash: 8868b17766513ba1e93b25bf2aeff6553c62ba62
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "88536150"
---
# <a name="tutorial-integrate-dmarcian-with-azure-active-directory"></a>教學課程：整合 dmarcian 與 Azure Active Directory

在本教學課程中，您將了解如何整合 dmarcian 與 Azure Active Directory (Azure AD)。 在整合 dmarcian 與 Azure AD 時，您可以︰

* 在 Azure AD 中控制可存取 dmarcian 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 dmarcian。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。

## <a name="prerequisites"></a>Prerequisites

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用 dmarcian 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* dmarcian 支援由 **SP 和 IDP** 起始的 SSO

## <a name="adding-dmarcian-from-the-gallery"></a>從資源庫新增 dmarcian

若要設定 dmarcian 與 Azure AD 整合，您需要從資源庫將 dmarcian 新增至受控 SaaS 應用程式清單中。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory]  服務。
1. 巡覽至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 若要新增應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中輸入 **dmarcian**。
1. 從結果面板選取 [dmarcian]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。


## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

以名為 **B.Simon** 的測試使用者，設定及測試與 dmarcian 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 dmarcian 中相關使用者之間的連結關聯性。

若要設定及測試與 dmarcian 搭配運作的 Azure AD SSO，請完成下列建置組塊：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
2. **[設定 dmarcian SSO](#configure-dmarcian-sso)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
5. **[建立 dmarcian 測試使用者](#create-dmarcian-test-user)** - 使 dmarcian 中對應的 B.Simon 連結到該使用者在 Azure AD 中的代表項目。
6. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

### <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [dmarcian]  應用程式整合頁面上，尋找 [管理]  區段並選取 [單一登入]  。
1. 在 [選取單一登入方法]  頁面上，選取 [SAML]  。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 組態]  區段上，若您想要以 **IDP** 起始模式設定應用程式，請執行下列步驟：

    a. 在 [識別碼]  文字方塊中，使用下列模式來輸入 URL：

    ```http
    https://us.dmarcian.com/sso/saml/<ACCOUNT_ID>/sp.xml
    https://dmarcian-eu.com/sso/saml/<ACCOUNT_ID>/sp.xml
    https://dmarcian-ap.com/sso/saml/<ACCOUNT_ID>/sp.xml
    ```

    b. 在 [回覆 URL]  文字方塊中，使用下列模式來輸入 URL：

    ```http
    https://us.dmarcian.com/login/<ACCOUNT_ID>/handle/
    https://dmarcian-eu.com/login/<ACCOUNT_ID>/handle/
    https://dmarcian-ap.com/login/<ACCOUNT_ID>/handle/
    ```

5. 如果您想要以 **SP** 起始模式設定應用程式，請按一下 [設定其他 URL]  ，然後執行下列步驟：

    在 [登入 URL]  文字方塊中，以下列模式輸入 URL︰
    
    ```http
    https://us.dmarcian.com/login/<ACCOUNT_ID>
    https://dmarcian-eu.com/login/<ACCOUNT_ID>
    https://dmarciam-ap.com/login/<ACCOUNT_ID>
    ```
     
    > [!NOTE] 
    > 這些都不是真正的值。 您將會使用實際的「識別碼」、「回覆 URL」及「登入 URL」來更新值，稍後會在本教學課程中說明。

4. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，按一下 [複製] 按鈕以複製 [應用程式同盟中繼資料 URL]  ，並將其儲存在您的電腦上。

    ![憑證下載連結](common/copy-metadataurl.png)

### <a name="configure-dmarcian-sso"></a>設定 dmarcian SSO

1. 若要自動執行 dmarcian 內的設定，您必須按一下 [安裝擴充功能]  來安裝「我的應用程式安全登入瀏覽器擴充功能」  。

    ![我的應用程式擴充功能](common/install-myappssecure-extension.png)

2. 將擴充功能新增至瀏覽器之後，按一下 [設定 dmarcian]  便會將您導向到 dmarcian 應用程式。 請從該處提供用以登入 dmarcian 的管理員認證。 瀏覽器擴充功能會自動為您設定應用程式，並自動執行步驟 3 到 6。

    ![設定組態](common/setup-sso.png)

3. 如果您想要手動設定 dmarcian，請開啟新的網頁瀏覽器視窗，並以系統管理員身分登入 dmarcian 公司網站，然後執行下列步驟：

4. 按一下右上角的 [設定檔]  ，然後瀏覽至 [喜好設定]  。

    ![喜好設定](./media/dmarcian-tutorial/tutorial_dmarcian_pref.png)

5. 向下捲動並按一下 [單一登入]  區段，然後按一下 [設定]  。

    ![單一](./media/dmarcian-tutorial/tutorial_dmarcian_sso.png)

6. 在 [SAML 單一登入]  頁面上，將 [狀態]  設定為 [已啟用]  並執行下列步驟：

    ![驗證](./media/dmarcian-tutorial/tutorial_dmarcian_auth.png)

    * 在 [將 dmarcian 新增至您的識別提供者]  區段下方按一下 [複製]  ，以複製執行個體的 [判斷提示取用者服務 URL]  ，並將其貼到 Azure 入口網站上的 [基本 SAML 組態]  區段的 [回覆 URL]  文字方塊中。

    * 在 [將 dmarcian 新增至您的識別提供者]  區段下方按一下 [複製]  ，以複製執行個體的 [實體識別碼]  ，並將其貼到 Azure 入口網站上的 [基本 SAML 組態]  區段的 [識別碼]  文字方塊中。

    * 在 [設定驗證]  區段下的 [識別提供者中繼資料]  文字方塊中，貼上您從 Azure 入口網站複製的 [應用程式同盟中繼資料 URL]  。

    * 在 [設定驗證]  區段下的 [屬性陳述式]  文字方塊中，貼上 URL `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    * 在 [設定登入 URL]  區段下方，複製執行個體的 [登入 URL]  ，並將其貼在 Azure 入口網站上的 [基本 SAML 組態]  區段的 [登入 URL]  文字方塊中。

        > [!Note]
        > 您可以根據您的組織修改 [登入 URL]  。

    * 按一下 [檔案]  。

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

在本節中，您將在 Azure 入口網站中建立名為 B.Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]  、[使用者]  和 [所有使用者]  。
1. 在畫面頂端選取 [新增使用者]  。
1. 在 [使用者]  屬性中，執行下列步驟：
   1. 在 [名稱]  欄位中，輸入 `B.Simon`。  
   1. 在 [使用者名稱]  欄位中，輸入 username@companydomain.extension。 例如： `B.Simon@contoso.com` 。
   1. 選取 [顯示密碼]  核取方塊，然後記下 [密碼]  方塊中顯示的值。
   1. 按一下頁面底部的 [新增]  。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 dmarcian 的存取權授與 B.Simon，讓其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [dmarcian]  。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中選取 [使用者和群組]  。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組]  對話方塊的 [使用者] 清單中選取 [B.Simon]  ，然後按一下畫面底部的 [選取]  按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色]  對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取]  按鈕。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

### <a name="create-dmarcian-test-user"></a>建立 dmarcian 測試使用者

若要讓 Azure AD 使用者能夠登入 dmarcian，必須將這些使用者佈建到 dmarcian。 在 dmarcian 中，需手動進行佈建。

**若要佈建使用者帳戶，請執行下列步驟：**

1. 以安全性系統管理員身分登入 dmarcian。

2. 按一下右上角的 [設定檔]  ，然後瀏覽至 [管理使用者]  。

    ![使用者](./media/dmarcian-tutorial/tutorial_dmarcian_user.png)

3. 在 [SSO 使用者]  區段右側，按一下 [新增使用者]  。

    ![新增使用者](./media/dmarcian-tutorial/tutorial_dmarcian_addnewuser.png)

4. 在 [新增使用者]  區段中，執行下列步驟：

    ![新增使用者](./media/dmarcian-tutorial/tutorial_dmarcian_save.png)

    a. 在 [新使用者電子郵件]  文字方塊中，輸入使用者的電子郵件地址，例如 **brittasimon\@contoso.com**。

    b. 如果您想要將系統管理員權限提供給使用者，請選取 [讓使用者為系統管理員]  。

    c. 按一下 [新增使用者]  。

### <a name="test-sso"></a>測試 SSO 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [dmarcian] 圖格時，應該會自動登入您已設定 SSO 的 dmarcian。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

