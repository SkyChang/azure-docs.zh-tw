---
title: 教學課程：Azure Active Directory 單一登入 (SSO) 與 Lenses.io 整合 | Microsoft Docs
description: 在本教學課程中，您會了解如何設定 Azure Active Directory 與 Lenses.io 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/02/2020
ms.author: jeedes
ms.openlocfilehash: 48a1e50d451abb429e9bc33308909b368283644f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "88661447"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-the-lensesio-dataops-portal"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 Lenses.io DataOps 入口網站整合

在此教學課程中，您將了解如何整合 [Lenses.io](https://lenses.io/) DataOps 入口網站與 Azure Active Directory (Azure AD)。 在整合 Lenses.io 與 Azure AD 後，您可以︰

* 在 Azure AD 中控制可存取 Lenses.io 入口網站的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 Lenses。
* 在 Azure 入口網站中集中管理您的帳戶。

若要深入了解軟體即服務 (SaaS) 應用程式與 Azure AD 的整合，請參閱[什麼是搭配 Azure AD 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)。

## <a name="prerequisites"></a>必要條件

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* Lenses 入口網站的執行個體。 您可以從數個[部署選項](https://lenses.io/product/deployment/)中進行選擇。
* 支援單一登入 (SSO) 的 Lenses.io [授權](https://lenses.io/product/pricing/) \(英文\)。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* Lenses.io 支援由服務提供者 (SP) 起始的 SSO。

* 您可以在設定 Lenses.io 後，強制執行工作階段控制項。 工作階段控制項可即時保護組織的敏感性資料免於外洩和遭到滲透。 工作階段控制項會從條件式存取延伸。 [了解如何使用 Microsoft Cloud App Security 來強制執行工作階段控制項](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)。

## <a name="add-lensesio-from-the-gallery"></a>從資源庫新增 Lenses.io

若要設定將 Lenses.io 整合到 Azure AD 中，請將 Lenses.io 新增到受控 SaaS 應用程式清單：

1. 使用公司或學校帳戶或個人 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左窗格中選取 [Azure Active Directory]  服務。
1. 移至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 選取 [新增應用程式]  。
1. 在 [從資源庫新增] 區段的搜尋方塊中輸入 **Lenses.io**。
1. 從結果面板選取 [Lenses.io]，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。

## <a name="configure-and-test-azure-ad-sso-for-lensesio"></a>設定和測試 Lenses.io 的 Azure AD SSO

您可以建立名為 *B.Simon* 的測試使用者，設定及測試與 Lenses.io 入口網站搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 Lenses.io 中相關使用者之間的連結關聯性。

完成以下步驟：

1. [設定 Azure AD SSO](#configure-azure-ad-sso)，讓您的使用者能夠使用此功能。
    1. [建立 Azure AD 測試使用者和群組](#create-an-azure-ad-test-user-and-group)，以 B.Simon 測試 Azure AD SSO。
    1. [指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)，讓 B.Simon 能夠使用 Azure AD SSO。
1. [設定 Lenses.io SSO](#configure-lensesio-sso) 以在應用程式端設定 SSO 設定。
    1. [建立 Lenses.io 測試群組權限](#create-lensesio-test-group-permissions)，以控制 B.Simon 可以存取 Lenses.io 中的哪些內容 (授權)。
1. [測試 SSO](#test-sso)，以驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟，在 Azure 入口網站中啟用 Azure AD SSO：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Lenses.io] 應用程式整合頁面上，尋找 [管理] 區段，然後選取 [單一登入]。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入] 頁面上，選取 [基本 SAML 組態] 的編輯/畫筆圖示，以編輯設定。

   ![螢幕擷取畫面，顯示用於編輯基本 SAML 設定的圖示。](common/edit-urls.png)

1. 在 [基本 SAML 設定] 區段中，於下列文字輸入方塊中輸入值：

    a. **登入 URL**：輸入具有下列模式的 URL：`https://<CUSTOMER_LENSES_BASE_URL>`。 例如 `https://lenses.my.company.com`。

    b. **識別碼 (實體識別碼)** ：輸入具有下列模式的 URL：`https://<CUSTOMER_LENSES_BASE_URL>`。 例如 `https://lenses.my.company.com`。

    c. **回覆 URL**：輸入具有下列模式的 URL：`https://<CUSTOMER_LENSES_BASE_URL>/api/v2/auth/saml/callback?client_name=SAML2Client`。 例如 `https://lenses.my.company.com/api/v2/auth/saml/callback?client_name=SAML2Client`。

    > [!NOTE]
    > 這些都不是真正的值。 使用實際的登入 URL、回覆 URL 以及 Lenses 入口網站執行個體的基底 URL 識別碼來更新這些值。 如需詳細資訊，請參閱 [Lenses.io SSO 文件](https://docs.lenses.io/install_setup/configuration/security.html#single-sign-on-sso-saml-2-0)。

1. 在 [以 SAML 設定單一登入] 頁面上，移至 [SAML 簽署憑證] 區段。 尋找 **同盟中繼資料 XML**，然後選取 [下載] 以下載憑證並將其儲存到電腦上。

    ![顯示憑證下載連結的螢幕擷取畫面。](common/metadataxml.png)

1. 在 [設定 Lenses.io] 區段中，使用所下載的 XML 檔案，針對您的 Azure SSO 設定 Lenses。

### <a name="create-an-azure-ad-test-user-and-group"></a>建立 Azure AD 測試使用者與群組

在 Azure 入口網站中，您將建立名為 B.Simon 的測試使用者。 接著，您將建立一個測試群組，以控制 B.Simon 在 Lenses 中擁有的存取權。

您可以在 [Lenses SSO 文件](https://docs.lenses.io/install_setup/configuration/security.html#id3) \(英文\) 中了解 Lenses 如何使用群組成員資格對應進行授權。

**若要建立測試使用者：**

1. 在 Azure 入口網站的左窗格上，依序選取 [Azure Active Directory]、[使用者] 和 [所有使用者]。
1. 在畫面頂端選取 [新增使用者]。
1. 在 [使用者] 屬性中，執行下列步驟：
   1. 在 [名稱] 方塊中，輸入 **B.Simon**。  
   1. 在 [使用者名稱] 方塊中，輸入 username@companydomain.extension。 例如： B.Simon@contoso.com 。
   1. 選取 [顯示密碼]  核取方塊。 記下 [密碼] 方塊中顯示的密碼。
   1. 選取 [建立]  。

**建立群組：**

1. 移至 [Azure Active Directory]，然後選取 [群組]。
1. 選取畫面頂端的 [新增群組]。
1. 在 [群組屬性] 中，遵循這些步驟：
   1. 在 [群組類型] 方塊中，選取 [安全性]。
   1. 在 [群組名稱] 方塊中，輸入 **LensesUsers**。
   1. 選取 [建立]  。
1. 選取 **LensesUsers** 群組，並複製 [物件識別碼] (例如 f8b5c1ec-45de-4abd-af5c-e874091fb5f7)。 您會在 Lenses 中使用此識別碼來將該群組的使用者對應至[正確權限](https://docs.lenses.io/install_setup/configuration/security.html#id3) \(英文\)。  

**將群組指派給測試使用者：**

1. 移至 [Azure Active Directory]，然後選取 [使用者]。
1. 選取測試使用者 **B.Simon**。
1. 選取 [群組]。
1. 選取畫面頂端的 [新增成員資格]。
1. 搜尋並選取 [LensesUsers]。
1. 按一下 [選取]。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 Lenses.io 的存取權授與 B.Simon，讓其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]，然後選取 [所有應用程式]。
1. 在應用程式清單上，選取 [Lenses.io]。
1. 在應用程式概觀頁面上的 [管理] 區段中，選取 [使用者與群組]。

   ![顯示 [使用者和群組] 連結的螢幕擷取畫面。](common/users-groups-blade.png)

1. 選取 [新增使用者]  。

   ![顯示 [新增使用者] 連結的螢幕擷取畫面。](common/add-assign-user.png)

1. 在 [新增指派]  對話方塊中，選取 [使用者和群組]  。
1. 在 [使用者和群組] 對話方塊的 [使用者] 清單中，選取 [B.Simon]。 然後按一下畫面底部的 [選取]**** 按鈕。
1. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色] 對話方塊的清單中選擇適當的使用者角色。 然後按一下畫面底部的 [選取]**** 按鈕。
1. 在 [新增指派] 對話方塊中，選取 [指派] 按鈕。

## <a name="configure-lensesio-sso"></a>設定 Lenses.io SSO

若要在 **Lenses.io** 入口網站上設定 SSO，請在 Lenses 執行個體上安裝已下載的**同盟中繼資料 XML**，並[設定 Lenses 以啟用 SSO](https://docs.lenses.io/install_setup/configuration/security.html#configure-lenses) \(英文\)。

### <a name="create-lensesio-test-group-permissions"></a>建立 Lenses.io 測試群組權限

1. 若要在 Lenses 中建立群組，請使用 **LensesUsers** 群組的**物件識別碼**。 這是您在使用者[建立區段](#create-an-azure-ad-test-user-and-group)中複製的識別碼。
1. 為 B.Simon 指派所需的權限。

如需詳細資訊，請參閱 [Azure - Lenses 群組對應](https://docs.lenses.io/install_setup/configuration/security.html#azure-groups)。

## <a name="test-sso"></a>測試 SSO

在本節中，請使用存取面板來測試您的 Azure AD SSO 組態。

當您在存取面板上選取 [Lenses.io] 圖格時，應該會自動登入您的 Lenses.io 入口網站。 如需詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [在您的 Lenses.io 執行個體中設定 SSO](https://docs.lenses.io/install_setup/configuration/security.html#single-sign-on-sso-saml-2-0)

- [有關如何整合 SaaS 應用程式與 Azure AD 的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure AD 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure AD 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [嘗試搭配 Azure AD 使用 Lenses.io](https://aad.portal.azure.com/)

- [什麼是 Microsoft Cloud App Security 中的工作階段控制項？](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [如何使用進階可見性和控制項保護 Lenses.io](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
