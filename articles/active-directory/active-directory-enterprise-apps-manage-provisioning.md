---
title: "Azure Active Directory 預覽版中企業應用程式的使用者佈建管理 |Microsoft Docs"
description: "了解如何使用 Azure Active Directory 預覽版管理企業應用程式的使用者帳戶佈建"
services: active-directory
documentationcenter: 
author: asmalser
manager: femila
editor: 
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/12/2016
ms.author: asmalser
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 103eade46452d979705e06dd77441f42d7a514b8


---
# <a name="preview-managing-user-account-provisioning-for-enterprise-apps-in-the-new-azure-portal"></a>預覽︰在新的 Azure 入口網站中管理企業應用程式的使用者帳戶佈建
這篇文章說明如何使用 [Azure 入口網站](https://portal.azure.com)來自動化管理使用者帳戶的佈建和解除佈建，以用於支援它的應用程式，尤其是透過 [Azure Active Directory 應用程式庫](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)的「精選」類別新增的應用程式。 這個 Azure 新入口網站中的管理體驗目前處於公開預覽狀態，本文章說明新功能以及一些在預覽期間的暫時性限制。 [預覽版有何功能？](active-directory-preview-explainer.md)

如需深入了解自動化使用者佈建，請參閱 [自動化使用 Azure Active Directory 對於 SaaS 應用程式的使用者佈建和解除佈建](active-directory-saas-app-provisioning.md)。

## <a name="finding-your-apps-in-the-new-portal"></a>在新的入口網站中尋找您的應用程式
自 2016 年 9 月起，目錄管理員在 [Azure 傳統入口網站](https://manage.windowsazure.com)中使用 [Azure Active Directory 應用程式庫](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)為了目錄中單一登入設定的所有應用程式，可以在新的 Azure 入口網站中檢視和管理。

這些應用程式可在新 Azure 入口網站的 [企業應用程式] 區段中找到，您可以透過左側的導覽區域中的 [更多服務] 功能表存取該區段。 企業應用程式是已部署並由您組織內的使用者所使用的應用程式。

![企業應用程式刀鋒視窗][0]

選取左側的 [所有應用程式]  連結會顯示已設定的所有應用程式的清單，包括已從資源庫新增的應用程式。 選取應用程式會載入該應用程式的資源刀鋒視窗，您可以在其中檢視該應用程式的報告，且可管理各種設定。

選取左側的 [佈建]  可以管理使用者帳戶的佈建設定。

![應用程式資源刀鋒視窗][1]

## <a name="provisioning-modes"></a>佈建模式
[佈建] 刀鋒視窗的最前方是 [模式] 功能表，會顯示企業應用程式支援哪些佈建模式，並可以設定它們。 可用的選項包括：

* **自動** - 如果 Azure AD 支援對此應用程式進行使用者帳戶的 API 型自動佈建和/或解除佈建，就會出現此選項。 選取此模式會顯示一介面，引導管理員設定 Azure AD 連接到應用程式的使用者管理 API、建立帳戶對應和工作流程定義 (定義使用者帳戶資料如何在 Azure AD 與應用程式之間的流動)、以及管理 Azure AD 佈建服務。
* **手動** - 如果 Azure AD 不支援對此應用程式自動佈建使用者帳戶，就會出現此選項。 此選項表示，必須根據該應用程式提供的使用者管理和佈建功能 (可包括 SAML Just-In-Time 佈建)，使用外部程序來管理儲存在應用程式中的使用者帳戶記錄。

## <a name="configuring-automatic-user-account-provisioning"></a>設定使用者帳戶自動佈建
選取 **自動** 選項會顯示分成四個部分的畫面︰

### <a name="admin-credentials"></a>管理員認證
可在這個地方輸入 Azure AD 連接到應用程式的使用者管理 API 所需的認證。 所需的輸入依應用程式而有所不同。 若要深入了解認證類型，以及針對特定應用程式的需求，請參閱 [針對該特定應用程式的設定教學課程](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning)。

選取 [測試連線]  按鈕，會讓 Azure AD 使用提供的認證嘗試連接到應用程式的佈建應用程式，可測試認證。

### <a name="mappings"></a>對應
管理員可在這個地方檢視及編輯 Azure AD 和目標應用程式之間可流動哪些使用者屬性，以及使用者帳戶何時佈建或更新。

在 Azure AD 使用者物件和每個 SaaS 應用程式的使用者物件之間，有一組預先設定的對應。 有些 app 則管理其他類型的物件，例如群組或連絡人。 選取資料表中這些對應的其中一個，會在右側顯示對應編輯器，可在此處檢視和自訂對應。

![應用程式資源刀鋒視窗][2]

在第一次預覽期間支援的自訂包括︰

* 啟用和停用特定物件的對應，例如 Azure AD 使用者物件對應至 SaaS 應用程式的使用者物件。
* 編輯哪些屬性會從 Azure AD 使用者物件流向應用程式的使用者物件。 如需有關屬性對應的詳細資訊，請參閱 [了解屬性對應類型](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types)。
* 篩選 Azure AD 應對目標應用程式執行哪些佈建動作，這是 Azure 入口網站的新功能。 您不需要讓 Azure AD 完全同步處理物件，可以限制執行的動作。 例如，只選取 **更新**，Azure AD 只會更新應用程式中的現有使用者帳戶，不會建立新的。 只有選取 **建立**，Azure 只會建立新的使用者帳戶，但不會更新現有的。 這項功能可讓管理員為帳戶建立和更新工作流程建立不同的對應。 可建立個別應用程式的多個對應的完整功能，計劃將在預覽版推出。

### <a name="settings"></a>設定
這個部分可讓管理員為所選的應用程式啟動和停止 Azure AD 佈建服務，以及選擇性地清除佈建快取並重新啟動服務。

如果是初次為應用程式啟用佈建，將 [佈建狀態] 變更為 [開啟] 即可開啟服務。 這會導致 Azure AD 佈建服務執行初始同步處理，此程序會讀取 [使用者和群組] 區段中指派的使用者，查詢他們的目標應用程式，並接著執行在 Azure AD [對應] 區段中定義的佈建動作。 在此程序中，佈建服務會儲存快取的資料 (有關管理哪些使用者帳戶)，所以不曾在指派範圍中的目標應用程式內的非受管理帳戶，不會受到解除佈建作業的影響。 初始同步處理之後，每隔十分鐘佈建服務會自動同步處理使用者和群組物件。

將 [佈建狀態] 變更為 [關閉] 只會暫停佈建服務。 在此狀態下，Azure 不會建立、更新或移除應用程式中的任何使用者或群組物件。 將狀態變更回開啟，服務會從停止的地方繼續。

選取 [清除目前的狀態，並重新啟動同步處理]  核取方塊然後儲存，會停止佈建服務，拋棄有關 Azure AD 管理哪些帳戶的快取資料，重新啟動服務，並再次執行初始同步處理。 此選項可讓管理員重新啟動佈建部署程序。

### <a name="synchronization-details"></a>同步處理詳細資料
這個部分提供佈建服務作業的詳細資訊，包括對應用程式執行佈建服務的第一次和最後一次時間，以及管理多少個使用者和群組物件。

這裡提供**佈建活動報表**的連結，以提供 Azure AD 和目標應用程式之間已建立、更新、移除的所有使用者和群組的記錄檔，以及提供**佈建錯誤報表**的連結，以針對無法讀取、建立、更新或移除的使用者和群組物件提供更詳細的錯誤訊息。 

[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG



<!--HONumber=Dec16_HO5-->


