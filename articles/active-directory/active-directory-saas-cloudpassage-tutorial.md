---
title: "教學課程：Azure Active Directory 與 CloudPassage 整合 | Microsoft Docs"
description: "了解如何設定 Azure Active Directory 與 CloudPassage 之間的單一登入。"
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: bfe1f14e-74e4-4680-ac9e-f7355e1c94cc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 1a4a1c3763fa13afcb7a93269a210b7dc4270e60
ms.openlocfilehash: 17b80f63ae27c6e42b81d13e221ac5acda970fdd
ms.lasthandoff: 02/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-cloudpassage"></a>教學課程：Azure Active Directory 與 CloudPassage 整合
本教學課程旨在說明如何整合 CloudPassage 與 Azure Active Directory (Azure AD)。  

CloudPassage 與 Azure AD 整合提供下列優點： 

* 您可以在 Azure AD 中控制可存取 CloudPassage 的人員 
* 您可以讓使用者使用其 Azure AD 帳戶自動登入 CloudPassage 單一登入 (SSO)
* 您可以在 Azure Active Directory 集中管理您的帳戶 

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>先決條件
若要設定 Azure AD 與 CloudPassage 整合，您需要下列項目：

* Azure AD 訂用帳戶
* 已啟用 CloudPassage SSO 的訂用帳戶

>[!NOTE]
>若要測試本教學課程中的步驟，我們不建議使用生產環境。 
> 

若要測試本教學課程中的步驟，您應該遵循這些建議：

* 除非必要，否則您不應使用生產環境，。
* 如果您沒有 Azure AD 測試環境，可以取得[一個月免費試用的訂用帳戶](https://azure.microsoft.com/pricing/free-trial/)。 

## <a name="scenario-description"></a>案例描述
此教學課程的目標是讓您在測試環境中測試 Azure AD SSO。  

本教學課程中說明的案例由二個主要建置組塊組成：

1. 從資源庫新增 CloudPassage 
2. 設定並測試 Azure AD SSO

## <a name="add-cloudpassage-from-the-gallery"></a>從資源庫新增 CloudPassage
若要設定 CloudPassage 與 Azure AD 整合，您需要從資源庫將 CloudPassage 新增到受管理的 SaaS App 清單。

**若要從資源庫新增 CloudPassage，請執行下列步驟：**
1. 在 **Azure 傳統入口網站**中，按一下左方瀏覽窗格的 [Active Directory]。 
   
    ![Active Directory][1]
2. 從 [目錄]  清單中，選取要啟用目錄整合的目錄。
3. 若要開啟應用程式檢視，請在目錄檢視中，按一下頂端功能表中的 [應用程式]  。
   
    ![應用程式][2]
4. 按一下頁面底部的 [新增]  。
   
    ![應用程式][3]
5. 在 [欲執行動作] 對話方塊上，按一下 [從資源庫中新增應用程式]。
   
    ![應用程式][4]
6. 在搜尋方塊中，輸入 **CloudPassage**。
   
    ![應用程式][5]
7. 在結果窗格中，選取 [CloudPassage]，然後按一下 [完成] 以新增應用程式。
   
    ![應用程式][6]

## <a name="configure-and-test-azure-ad-sso"></a>設定並測試 Azure AD SSO
本節目標是說明如何以名為 "Britta Simon" 的測試使用者為基礎，設定及測試透過 CloudPassage 使用 Azure AD (SSO)。

若要讓 SSO 運作，Azure AD 必須知道 CloudPassage 與 Azure AD 中互相對應的使用者。 換句話說，必須在 Azure AD 使用者和 CloudPassage 中的相關使用者之間建立連結關聯性。  

建立此連結關聯性的方法，是將 Azure AD 中**使用者名稱**的值，指派為 CloudPassage 中 **Username** 的值。

若要設定及測試與 CloudPassage 搭配運作的 Azure AD SSO，您需要完成下列建置組塊：

1. **[設定 Azure AD 單一登入](#configuring-azure-ad-single-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[建立 Azure AD 測試使用者](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
3. **[建立 CloudPassage 測試使用者](#creating-a-halogen-software-test-user)** - 使 CloudPassage 中對應的 Britta Simon 連結到她在 Azure AD 中的代表項目。
4. **[指派 Azure AD 測試使用者](#assigning-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[測試單一登入](#testing-single-sign-on)** - 驗證組態是否能運作。

### <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO
本節的目標是要在 Azure 傳統入口網站中啟用 Azure AD SSO，並在您的 CloudPassage 應用程式中設定 SSO。  

您的 CloudPassage 應用程式需要特定格式的 SAML 判斷提示，其會要求您將自訂屬性對應新增到 SAML Token 屬性設定。 

以下螢幕擷取畫面顯示上述的範例。

![設定單一登入][21]

**若要使用 CloudPassage 設定 Azure AD 單一登入，請執行下列步驟：**

1. 在 Azure AD 傳統入口網站的 **CloudPassage** 應用程式整合頁面上，按一下 [設定單一登入] 來開啟 [設定單一登入] 對話方塊。
   
    ![設定單一登入][7]
2. 在 [您希望使用者如何登入 CloudPassage] 頁面上，選取 [Azure AD 單一登入]，然後按 [下一步]。
   
    ![設定單一登入][8]
3. 在 [設定 App 設定]  對話方塊頁面執行下列步驟： 
   
    ![設定 App 設定][9]   
  1. 在 [登入 URL] 文字方塊中，輸入您的使用者用來登入 CloudPassage App 的 URL (例如：*https://portal.cloudpassage.com/saml/init/accountid*)。  
  2. 在 [回覆 URL] 文字方塊中，輸入您的 AssertionConsumerService URL (例如：*https://portal.cloudpassage.com/saml/consume/accountid*)。 您也可以在 CloudPassage 入口網站的 [單一登入設定] 區段中按一下 [SSO 安裝文件]，為這個屬性設定值。
  
    ![設定單一登入][10]   
  3. 按 [下一步] 。
4. 在 [設定在 CloudPassage 單一登入] 頁面上，按一下 [下載憑證]，然後在本機電腦上儲存憑證檔案。 
   
    ![設定單一登入][11]
5. 在不同的瀏覽器視窗中，以系統管理員身分登入您的 CloudPassage 公司網站。
6. 在頂端功能表中，按一下 [設定]，然後按一下 [網站管理]。 
   
    ![設定單一登入][12]
7. 按一下 [驗證設定]  索引標籤。 
   
    ![設定單一登入][13]
8. 在 [單一登入設定]  區段中，執行下列步驟： 
   
    ![設定單一登入][14]
  1. 在 Azure 傳統入口網站的 [設定在 CloudPassage 單一登入] 對話方塊頁面上，複製 [簽發者 URL] 值，然後將它貼到 [SAML 簽發者] 文字方塊中。
  2. 在 Azure 傳統入口網站的 [設定在 CloudPassage 單一登入] 對話方塊頁面上，複製 [服務提供者 (SP) 啟始的端點] 值，然後將它貼到 [SAML 端點 URL] 文字方塊中。
  3. 在 Azure 傳統入口網站的 [設定在 CloudPassage 單一登入] 對話方塊頁面上，複製 [登出 URL] 值，然後將它貼到 [登出登陸頁面] 文字方塊中。
  4. 從您下載的憑證建立 **Base-64 編碼**檔案。 

    >[AZURE.TIP] 如需詳細資訊，請參閱 [如何將二進位憑證轉換成文字檔](http://youtu.be/PlgrzUZ-Y1o)。
  5. 在記事本中開啟 Base-64 編碼的憑證、將其內容複製到剪貼簿，然後將它貼到 [X.509 憑證] 文字方塊中。
  6. 按一下 [儲存] 。


1. 在 Azure AD 傳統入口網站上，選取單一登入設定確認，然後按一下 [下一步] 。 
   
    ![設定單一登入][15]
2. 在 [單一登入確認] 頁面上，按一下 [完成]。 
   
   ![設定單一登入][16]
3. 在頂端功能表中，按一下 [屬性] 以開啟 [SAML Token 屬性] 對話方塊。 
   
   ![設定單一登入][17]
4. 若要加入必要的使用者屬性，可針對下表中的每一列執行下列步驟： 
   
   | 屬性名稱 | 屬性值 |
   | --- | --- |
   | firstname |user.givenname |
   | lastname |user.surname |
   | 電子郵件 |user.mail |
  1. 按一下 [加入使用者屬性] 。 

    ![設定單一登入][18]
  2. 在 [屬性名稱] 文字方塊中，輸入為該資料列顯示的屬性名稱並做為 [屬性值]，然後選取為該資料列顯示的屬性值。 

    ![設定單一登入][19]
  3. 按一下頁面底部的 [新增] 。
1. 在底部功能表中按一下 [套用變更] 。 
   
   ![設定單一登入][20]

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者
本節的目標是要在 Azure 傳統入口網站中建立一個名為 Britta Simon 的測試使用者。  

![建立 Azure AD 測試使用者](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_01.png)

**若要在 Azure AD 中建立測試使用者，請執行下列步驟：**

1. 在 **Azure 傳統入口網站**中，按一下左方瀏覽窗格的 [Active Directory]。
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_02.png) 
2. 從 [目錄]  清單中，選取要啟用目錄整合的目錄。
3. 若要顯示使用者清單，請按一下頂端功能表的 [使用者] 。
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_03.png) 
4. 若要開啟 [新增使用者] 對話方塊，請按一下底部工具列上的 [新增使用者]。 
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_04.png) 
5. 在 [告訴我們這位使用者]  對話方塊頁面上，執行下列步驟： 
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_05.png) 
  1. 針對 [使用者類型]，選取 [您組織中的新使用者]。
  2. 在 [使用者名稱] 文字方塊中，輸入 **BrittaSimon**。
  3. 按 [下一步]。
6. 在 [使用者設定檔]  對話方塊頁面上，執行下列步驟： 
   
   ![建立 Azure AD 測試使用者](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_06.png) 
  1. 在 [名字] 文字方塊中，輸入 **Britta**。  
  2. 在 [姓氏] 文字方塊中，輸入 **Simon**。
  3. 在 [顯示名稱] 文字方塊中，輸入 **Britta Simon**。
  4. 在 [角色] 清單中選取 [使用者]。
  5. 按 [下一步] 。
7. 在 [取得暫時密碼] 對話方塊頁面上，按一下 [建立]。
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_07.png) 
8. 在 [取得暫時密碼]  對話方塊頁面上，執行下列步驟：
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_08.png) 
  1. 記下 [新密碼] 的值。
  2. 按一下頁面底部的 [新增] 。   

### <a name="create-a-cloudpassage-test-user"></a>建立 CloudPassage 測試使用者
本節目標是在 CloudPassage 中建立名為 Britta Simon 的使用者。

**若要在 CloudPassage 中建立名為 Britta Simon 的使用者，請執行以下步驟：**
1. 以系統管理員身分登入您的 **CloudPassage** 公司網站。 
2. 在頂端工具列中，按一下 [設定]，然後按一下 [網站管理]。 
   
   ![建立 CloudPassage 測試使用者][22] 
3. 按一下 [使用者] 索引標籤，然後按一下 [新增使用者]。 
   
   ![建立 CloudPassage 測試使用者][23]
4. 在 [新增使用者]  區段中，執行下列步驟： 
   
   ![建立 CloudPassage 測試使用者][24]
  1. 在 [名字] 文字方塊中，輸入 Britta。 
  2. 在 [姓氏] 文字方塊中，輸入 Simon。
  3. 在 [使用者名稱] 文字方塊、[電子郵件] 文字方塊和 [重新輸入電子郵件] 文字方塊中，輸入 Britta 在 Azure AD 中的使用者名稱。
  4. 對於 [存取類型]，選取 [啟用 Halo 入口網站存取]。
  5. 按一下 [新增] 。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者
本節目標是授與 Britta Simon 對 CloudPassage 的存取權，讓她能夠使用 Azure SSO。

![指派使用者][30]

**若要將 Britta Simon 指派到 CloudPassage，請執行以下步驟：**

1. 在 Azure 傳統入口網站中，若要開啟應用程式檢視，請在目錄檢視中，按一下頂端功能表中的 [應用程式]  。
   
    ![指派使用者][26]
2. 在應用程式清單中，選取 [CloudPassage] 。
   
    ![指派使用者][27]
3. 在頂端的功能表中，按一下 [使用者] 。
   
    ![指派使用者][25]
4. 在 [使用者] 清單中，選取 [Britta Simon] 。
5. 在底部的工具列中，按一下 [指派] 。
   
    ![指派使用者][29]

### <a name="test-single-sign-on"></a>測試單一登入
本節的目標是要使用「存取面板」來測試您的 Azure AD SSO 組態。

當您在存取面板中按一下 [CloudPassage] 磚時，應該會自動登入您的 CloudPassage 應用程式。

## <a name="additional-resources"></a>其他資源
* [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](active-directory-saas-tutorial-list.md)
* [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_01.png
[6]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_02.png
[7]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_05.png
[8]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_03.png
[9]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_04.png
[10]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_05.png
[11]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_06.png
[12]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_10.png
[16]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_11.png
[17]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_10.png
[18]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_11.png
[19]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_12.png
[20]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_14.png
[21]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_14.png
[22]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_17.png
[25]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_15.png
[26]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_12.png
[27]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_13.png
[28]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[29]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_16.png
[30]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_17.png






















