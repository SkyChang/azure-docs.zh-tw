---
title: 教學課程：Azure Active Directory 單一登入 (SSO) 與 Dynatrace 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Dynatrace 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/22/2019
ms.author: jeedes
ms.openlocfilehash: a1f052df9b233fd564ed7e16c04c32c0c7234e1a
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88555649"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-dynatrace"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 Dynatrace 整合

在本教學課程中，您會了解如何將 Dynatrace 與 Azure Active Directory (Azure AD) 進行整合。 在整合 Dynatrace 與 Azure AD 時，您可以︰

* 在 Azure AD 中控制可存取 Dynatrace 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 Dynatrace。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。

## <a name="prerequisites"></a>Prerequisites

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用 Dynatrace 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* Dynatrace 支援由 **SP 和 IDP** 起始的 SSO
* Dynatrace 支援 **Just In Time** 使用者佈建

> [!NOTE]
> 此應用程式的識別碼都是固定字串值。 在一個租用戶中只能設定一個執行個體。

## <a name="adding-dynatrace-from-the-gallery"></a>從資源庫新增 Dynatrace

若要設定將 Dynatrace 整合到 Azure AD 中，您需要從資源庫將 Dynatrace 新增到受控 SaaS 應用程式清單。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory]  服務。
1. 巡覽至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 若要新增應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中輸入 **Dynatrace**。
1. 從結果面板選取 [Dynatrace]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。

## <a name="configure-and-test-azure-ad-single-sign-on-for-dynatrace"></a>設定及測試 Dynatrace 的 Azure AD 單一登入

以名為 **B.Simon** 的測試使用者，設定及測試與 Dynatrace 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 Dynatrace 中相關使用者之間的連結關聯性。

若要設定及測試與 Dynatrace 搭配運作的 Azure AD SSO，請完成下列建置組塊：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
    * **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
    * **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
1. **[設定 Dynatrace SSO](#configure-dynatrace-sso)** - 在應用程式端設定單一登入設定。
    * **[建立 Dynatrace 測試使用者](#create-dynatrace-test-user)** - 在 Dynatrace 中建立 B.Simon 的對應項目，且該項目與 Azure AD 中代表使用者的項目連結。
1. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Dynatrace]  應用程式整合頁面上，尋找 [管理]  區段並選取 [單一登入]  。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [基本 SAML 組態]  區段中，已預先以 **IDP** 起始的模式設定好應用程式，並已經為 Azure 預先填入必要的 URL。 使用者必須按一下 [儲存]  按鈕，才能儲存設定。

1. 按一下 [設定其他 URL]  ，並完成下列步驟，以 **SP** 起始模式設定應用程式：

    在 [登入 URL]  文字方塊中，輸入 URL：`https://sso.dynatrace.com/`

1. 在 [以 SAML 設定單一登入]  頁面上的 [SAML 簽署憑證]  區段中，尋找**同盟中繼資料 XML**。 選取 [下載]  以下載憑證，並將其儲存在您的電腦上。

    ![憑證下載連結](common/metadataxml.png)

1. 在 [SAML 簽署憑證]  區段中，選取 [編輯]  按鈕以開啟 [SAML 簽署憑證]  對話方塊。 完成下列步驟：

    ![編輯 SAML 簽署憑證](common/edit-certificate.png)

    a. [簽署選項]  設定已預先填入。 請根據您的組織來檢閱設定。

    b. 按一下 [檔案]  。

    ![Communifire 簽署選項](./media/dynatrace-tutorial/tutorial-dynatrace-signing-option.png)

1. 在 [設定 Dynatrace]  區段中，根據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

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

在本節中，您會將 Dynatrace 的存取權授與 B.Simon，讓其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [Dynatrace]  。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中，選取 [使用者和群組]  。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者及群組]  對話方塊的 [使用者] 清單中選取 [B.Simon]  ，然後按一下畫面底部的 [選取]  按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色]  對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取]  按鈕。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

## <a name="configure-dynatrace-sso"></a>設定 Dynatrace SSO

若要在 **Dynatrace** 端設定單一登入，您必須將從 Azure 入口網站下載的**同盟中繼資料 XML** 檔案和複製的適當 URL 傳送給 [Dynatrace](https://www.dynatrace.com/support/help/shortlink/users-sso-hub)。 您可以遵循 Dynatrace 網站上的指示來設定兩端的 SAML SSO 連線。

### <a name="create-dynatrace-test-user"></a>建立 Dynatrace 測試使用者

本節會在 Dynatrace 中建立名為 B.Simon 的使用者。 Dynatrace 支援依預設啟用的 Just-In-Time 使用者佈建。 在這一節沒有您需要進行的動作項目。 如果 Dynatrace 中還沒有任何使用者存在，在驗證之後就會建立新的使用者。

## <a name="test-sso"></a>測試 SSO

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [Dynatrace] 圖格時，應該會自動登入您已設定 SSO 的 Dynatrace。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [嘗試搭配 Azure AD 使用 Dynatrace](https://aad.portal.azure.com/)
