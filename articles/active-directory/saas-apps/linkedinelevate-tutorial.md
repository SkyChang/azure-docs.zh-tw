---
title: 教學課程：Azure Active Directory 單一登入 (SSO) 與 LinkedIn Elevate 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 LinkedIn Elevate 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/21/2019
ms.author: jeedes
ms.openlocfilehash: 1f4569a45b9ed0eee7c375e660df97925335313b
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88549791"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-linkedin-elevate"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 LinkedIn Elevate 整合

在本教學課程中，您將了解如何整合 LinkedIn Elevate 與 Azure Active Directory (Azure AD)。 在整合 LinkedIn Elevate 與 Azure AD 時，您可以︰

* 在 Azure AD 中控制可存取 LinkedIn Elevate 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 LinkedIn Elevate。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。

## <a name="prerequisites"></a>Prerequisites

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 啟用 LinkedIn Elevate 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。



* LinkedIn Elevate 支援 **SP 和 IDP** 起始的 SSO
* LinkedIn Elevate 支援 **Just In Time** 使用者佈建
* LinkedIn Elevate 支援[**自動化**使用者佈建](linkedinelevate-provisioning-tutorial.md)

## <a name="adding-linkedin-elevate-from-the-gallery"></a>從資源庫新增 LinkedIn Elevate

若要設定將 LinkedIn Elevate 整合到 Azure AD 中，您需要從資源庫將 LinkedIn Elevate 新增到受控 SaaS 應用程式清單。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory]  服務。
1. 巡覽至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 若要新增應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中輸入 **LinkedIn Elevate**。
1. 從結果面板選取 [LinkedIn Elevate]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。


## <a name="configure-and-test-azure-ad-single-sign-on-for-linkedin-elevate"></a>設定及測試 LinkedIn Elevate 的 Azure AD 單一登入

以名為 **B.Simon** 的測試使用者，設定及測試與 LinkedIn Elevate 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 LinkedIn Elevate 中相關使用者之間的連結關聯性。

若要設定及測試與 LinkedIn Elevate 搭配運作的 Azure AD SSO，請完成下列建置組塊：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
    1. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
    1. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
1. **[設定 LinkedIn Elevate SSO](#configure-linkedin-elevate-sso)** - 在應用程式端設定單一登入設定。
    1. **[建立 LinkedIn Elevate 測試使用者](#create-linkedin-elevate-test-user)** - 使 LinkedIn Elevate 中對應的 B.Simon 連結到該使用者在 Azure AD 中的代表項目。
1. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [LinkedIn Elevate]  應用程式整合頁面上，尋找 [管理]  區段並選取 [單一登入]  。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [基本 SAML 設定]  區段上，如果您想要以 **IDP** 起始模式設定應用程式，請輸入下列欄位的值：

    a. 在 [識別碼]  文字方塊中，輸入 [實體識別碼]  值，您將會從 Linkedin 入口網站複製本教學課程稍後說明的實體識別碼值。

    b. 在 [回覆 URL]  文字方塊中，輸入 [判斷提示取用者存取 (ACS) URL]  值，您將會從 Linkedin 入口網站複製本教學課程稍後說明的判斷提示取用者存取 (ACS) URL 值。

5. 如果您想要以 **SP** 起始模式設定應用程式，請按一下 [設定其他 URL]  ，然後執行下列步驟：

    在 [登入 URL]  文字方塊中，以下列模式輸入 URL︰`https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>`

1. LinkedIn Elevate 應用程式需要特定格式的 SAML 判斷提示，這會需要您將自訂屬性對應新增至您的 SAML 權杖屬性組態。 下列螢幕擷取畫面顯示預設屬性清單，其中的 **nameidentifier** 與 **user.userprincipalname** 相對應。 LinkedIn Elevate 應用程式要求 nameidentifier 需與**user.mail** 相對應，因此您必須按一下 [編輯] 圖示以編輯屬性對應，並變更屬性對應。

    ![image](common/edit-attribute.png)

1. 除了上述屬性外，LinkedIn Elevate 應用程式還需要在 SAML 回應中多傳回幾個屬性，如下所示。 這些屬性也會預先填入，但您可以根據您的需求來檢閱這些屬性。

    | 名稱 | 來源屬性|
    | -------| -------------|
    | department | user.department |

1. 在 [以 SAML 設定單一登入]  頁面上的 [SAML 簽署憑證]  區段中，尋找 [同盟中繼資料 XML]  ，然後選取 [下載]  ，以下載憑證並將其儲存在電腦上。

    ![憑證下載連結](common/metadataxml.png)

1. 在 [設定 LinkedIn Elevate]  區段上，依據您的需求複製適當的 URL。

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

在本節中，您會將 LinkedIn Elevate 的存取權授與 B.Simon，讓其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [LinkedIn Elevate]  。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中選取 [使用者和群組]  。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組]  對話方塊的 [使用者] 清單中選取 [B.Simon]  ，然後按一下畫面底部的 [選取]  按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色]  對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取]  按鈕。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

## <a name="configure-linkedin-elevate-sso"></a>設定 LinkedIn Elevate SSO

1. 在不同的網頁瀏覽器視窗中，以管理員身分登入您的 LinkedIn Elevate 租用戶。

1. 在 [帳戶中心]  中，按一下 [設定]  底下的 [全域設定]  。 此外，從下拉式清單選取 [提升 - 提升 AAD 測試]  。

    ![設定單一登入](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_01.png)

1. 按一下**或按一下這裡以從表單載入和複製個別欄位**，然後執行下列步驟：

    ![設定單一登入](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_03.png)

    a. 複製 [實體識別碼]  並將其貼到 Azure 入口網站中 [基本 SAML 組態]  中的 [識別碼]  文字方塊內。

    b. 複製 [判斷提示取用者存取 (ACS) Url]  並將其貼到 Azure 入口網站中 [基本 SAML 組態]  中的 [回覆 URL]  文字方塊內。

1. 移至 [LinkedIn 系統管理員設定]  區段。 按一下 [上傳 XML 檔案] 選項，將您從 Azure 入口網站下載的 XML 檔案上傳。

    ![設定單一登入](./media/linkedinelevate-tutorial/tutorial_linkedin_metadata_03.png)

1. 按一下 [開啟]  以啟用 SSO。 SSO 狀態將會從 [未連線]  變更為 [已連線] 

    ![設定單一登入](./media/linkedinelevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="create-linkedin-elevate-test-user"></a>建立 LinkedIn Elevate 測試使用者

LinkedIn Elevate 應用程式支援即時使用者佈建，且在驗證後會在應用程式中自動建立使用者。 在 LinkedIn Elevate 入口網站的系統管理設定頁面上，將 [自動指派授權]  切換成啟用即時佈建，這個動作也會將授權指派給使用者。 LinkedIn Elevate 也支援自動使用者佈建，您可以在[這裡](linkedinelevate-provisioning-tutorial.md)找到關於如何設定自動使用者佈建的更多詳細資料。

   ![建立 Azure AD 測試使用者](./media/linkedinelevate-tutorial/LinkedinUserprovswitch.png)

## <a name="test-sso"></a>測試 SSO 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 LinkedIn Elevate 圖格時，應該會自動登入您設定 SSO 的 LinkedIn Elevate。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [嘗試搭配 Azure AD 使用 LinkedIn Elevate](https://aad.portal.azure.com/)

