---
title: "Logic Apps B2B 整合帳戶災害復原：Azure App Service | Microsoft Docs"
description: "Logic Apps B2B 災害復原"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: padmavc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 197df490690754730425231f358fde31d17dcfad
ms.contentlocale: zh-tw
ms.lasthandoff: 05/10/2017


---

# <a name="logic-apps-b2b-cross-region-disaster-recovery"></a>Logic Apps B2B 跨區域災害復原
B2B 工作負載涉及金錢交易，例如訂單和發票。 在災害事件期間，企業務必快速復原，才可符合與合作夥伴達成的商務層級 SLA。 本文示範如何建置 B2B 工作負載的商務持續性計劃。 

* 災害復原整備 
* 在災難事件期間容錯移轉到次要區域 
* 發生災難事件後切換回主要區域

## <a name="disaster-recovery-readiness"></a>災害復原整備  

1. 識別次要地區，並在次要地區中建立[整合帳戶](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) 

2. 對於執行狀態需要複寫到次要區域整合帳戶的必要訊息流程，新增夥伴、結構描述和合約。

    > [!Tip]
    > 請確定區域之間的整合帳戶成品命名慣例保持一致。 
    > 
    > 

3. 若要提取主要區域的執行狀態，請在次要區域中建立邏輯應用程式。  它應該有**觸發程序**和**動作**。  觸發程序應該連接至主要區域整合帳戶，而且動作應該連接至次要區域整合帳戶。  按照時間間隔，觸發程序會輪詢主要區域執行狀態資料表提取新的記錄 (如果有)，而且動作會將這些新的記錄更新至次要區域整合帳戶。 這有助於從主要區域將累加式執行階段狀態更新到次要區域。

4. 邏輯應用程式整合帳戶中的商務持續性是設計來按照 B2B 通訊協定 (X12、AS2 和 EDIFACT) 進行支援。  若要了解詳細的步驟，請選取個別的連結。

5. 建議也將所有主要區域的資源部署在次要地區。 主要區域的資源包括 Azure SQL Database 或 Azure Cosmos DB、Azure 服務匯流排/用來傳訊的 Azure 事件中樞，Azure API 管理，以及 Azure App Service 的 Logic Apps 功能。   

6. 建立從主要區域到次要地區的連線。 若要提取主要區域的執行狀態，請在次要地區中建立邏輯應用程式。 它應該有觸發程序和動作。 觸發程序需連線至主要區域整合帳戶。 動作需連線至次要地區整合帳戶。 根據時間間隔，觸發程序會輪詢主要區域執行狀態資料表，並提取新的記錄 (如果有)。 動作會將它們更新至次要地區整合帳戶。 此程序有助於將累加式執行階段狀態從主要區域更新到次要地區。

Logic Apps 整合帳戶中的商務持續性會根據 B2B 通訊協定 X12、AS2 和 EDIFACT 提供支援。 如需使用 X12 和 AS2 的詳細步驟，請參閱本文中的 [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) 和 [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2)。

## <a name="fail-over-to-a-secondary-region-during-a-disaster-event"></a>在災難事件期間容錯移轉到次要地區
在災難事件期間，沒有商務持續性的主要區域可用時，會將流量導向次要區域。 次要地區可協助企業將職能快速復原，來符合與其合作夥伴達成的 RPO/RTO。 此外，從一個區域容錯移轉至另一個區域僅需要最低程度的作業過程。 

將控制編號從主要區域複製到次要地區會有預期的延遲。 若要避免在災害事件期間將重複產生的控制編號傳送給合作夥伴，建議使用 [PowerShell Cmdlet](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery) 在次要地區合約中增加控制編號。

## <a name="fall-back-to-a-primary-region-post-disaster-event"></a>發生災難事件後切換回主要區域
若要在可用時切換回主要區域，請遵循下列步驟︰

1. 停止接受次要地區中的合作夥伴發出的訊息。  

2. 使用 [PowerShell Cmdlet](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery) 對所有主要區域合約增加所產生的控制編號。  

3. 將資料流從次要地區導向主要區域。

4. 查看是否已啟用在次要地區中建立邏輯應用程式以從主要區域提取執行狀態。

## <a name="x12"></a>X12 
EDI X12 文件的商務持續性是根據控制編號：
* 收到合作夥伴發出的控制編號 (輸入訊息)  
* 產生控制編號 (輸出訊息) 並傳送給合作夥伴 
    
    > [!Tip]
    > 您也可以使用 [X12 快速啟動範本](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/)來建立 Logic Apps。 建立主要和次要整合帳戶是使用範本的必要條件。 這個範本可讓您建立 2 個 Logic Apps，一個用於接收的控制編號，另一個用於產生的控制編號。 會在 Logic Apps 中建立個別的觸發程序和動作，將觸發程序連線至主要整合帳戶，並將動作連線至次要整合帳戶。
    > 
    >

### <a name="control-numbers-received-from-partners"></a>收到合作夥伴發出的控制編號

1. 啟用合約接收設定上的重複檢查   
![X12 搜尋](./media/logic-apps-enterprise-integration-b2b-business-continuity/dupcheck.png)  

2. 在次要地區中建立[邏輯應用程式](../logic-apps/logic-apps-create-a-logic-app.md)。 

3. 搜尋 **X12**，並選取 [X12 - 修改接收的控制編號時]。   
![X12 搜尋](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN1.png)

4. 觸發程序會提示您建立整合帳戶的連線。 觸發程序需連線至主要區域整合帳戶。 輸入連線名稱，並選取清單中的 [主要區域整合帳戶]，然後按一下 [建立]。  
![主要區域整合帳戶名稱](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN2.png)

5. **開始控制編號同步處理的日期時間**設定是選用的。 **頻率**可以設為**天**、**小時**、**分鐘**或**秒**的間隔。  
![日期時間和頻率](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN3.png)

6. 選取 [新增步驟] > [新增動作]。    
![新增動作](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN4.png)

7. 搜尋 **X12**，並選取 [X12 - 新增或更新接收的控制編號]。   
![已接收的控制編號修改](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN5.png)

8. 若要將動作連線到次要地區整合帳戶，請選取 [變更連線] > [新增新的連線]，取得可用整合帳戶的清單。 輸入連線名稱，並選取清單中的 [次要地區整合帳戶]，然後按一下 [建立]。   
![次要地區整合帳戶名稱](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN6.png)

9. 選取動態內容，並儲存邏輯應用程式。 
![動態內容](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN7.png)

10. 根據時間間隔，觸發程序會輪詢主要區域接收的控制編號資料表，並提取新的記錄。 動作會將它們更新至次要地區整合帳戶。 如果沒有更新，則觸發程序狀態將顯示為**忽略**。
![控制編號資料表](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN8.png)

### <a name="control-numbers-generated-and-sent-to-partners"></a>產生控制編號並傳送給合作夥伴
1. 在次要地區中建立[邏輯應用程式](../logic-apps/logic-apps-create-a-logic-app.md)。

2. 搜尋 **X12**，並選取 [X12 - 修改產生的控制編號時]。  
![已產生的控制編號修改](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN1.png)

3. 觸發程序會提示您建立整合帳戶的連線。 觸發程序需連線至主要區域整合帳戶。 輸入連線名稱，並選取清單中的 [主要區域整合帳戶]，然後按一下 [建立]。   
![主要區域整合帳戶名稱](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN2.png) 

4. **開始控制編號同步處理的日期時間**設定是選用的。 **頻率**可以設為**天**、**小時**、**分鐘**或**秒**的間隔。  
![日期時間和頻率](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN3.png)  

5. 選取 [新增步驟] > [新增動作]。  
![新增動作](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN4.png)

6. 搜尋 **X12**，並選取 [X12 - 新增或更新產生的控制編號時]。   
![所產生的控制編號新增或更新](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN5.png)

7. 若要將動作連線到次要整合帳戶，請選取 [變更連線] > [新增新的連線]，取得可用整合帳戶的清單。 輸入連線名稱，並選取清單中的 [次要地區整合帳戶]，然後按一下 [建立]。   
![次要地區整合帳戶名稱](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN6.png)

8. 選取動態內容，並儲存邏輯應用程式。 
![動態內容](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN7.png)

9. 根據時間間隔，觸發程序會輪詢主要區域接收的控制編號資料表，並提取新的記錄。 動作會將它們更新至次要地區整合帳戶。 如果沒有更新，則觸發程序狀態將顯示為**忽略**。  
![控制編號資料表](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN8.png)

根據時間間隔，累加式執行階段狀態會從主要區域複寫至次要地區。 在災難事件期間，沒有主要區域可用時，會對於商務持續性將流量導向次要區域。 

## <a name="as2"></a>AS2 
使用 AS2 通訊協定的文件之商務持續性是根據訊息識別碼和 MIC 值。

> [!Tip]
    > 您也可以使用 [AS2 快速啟動範本](https://github.com/Azure/azure-quickstart-templates/pull/3302)建立 Logic Apps。 建立主要和次要整合帳戶是使用範本的必要條件。 該範本可協助建立有觸發程序和動作的邏輯應用程式。 邏輯應用程式會建立從觸發程序到主要整合帳戶的連線，以及動作到建立次要整合帳戶的連線。
    > 
    >

1. 在次要地區中建立[邏輯應用程式](../logic-apps/logic-apps-create-a-logic-app.md)。  

2. 搜尋 **AS2**，並選取 [AS2 - 建立 MIC 值時]。   
![AS2 搜尋](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid1.png)

3. 觸發程序會提示您建立整合帳戶的連線。 觸發程序需連線至主要區域整合帳戶。 輸入連線名稱，並選取清單中的 [主要區域整合帳戶]，然後按一下 [建立]。   
![主要區域整合帳戶名稱](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid2.png)

4. **開始 MIC 值同步處理的日期時間**設定是選用的。 **頻率**可以設為**天**、**小時**、**分鐘**或**秒**的間隔。   
![日期時間和頻率](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid3.png)

5. 選取 [新增步驟] > [新增動作]。  
![新增動作](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid4.png)

6. 搜尋 **AS2**，並選取 [AS2 - 新增或更新 MIC]。  
![MIC 新增或更新](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid5.png)

7. 若要將動作連線到次要整合帳戶，請選取 [變更連線] > [新增新的連線]，取得可用整合帳戶的清單。 輸入連線名稱，並選取清單中的 [次要地區整合帳戶]，然後按一下 [建立]。    
![次要地區整合帳戶名稱](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid6.png)

8. 選取動態內容，並儲存邏輯應用程式。   
![動態內容](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid7.png)

9. 根據時間間隔，觸發程序會輪詢主要區域資料表，並提取新的記錄。 動作會將它們更新至次要地區整合帳戶。 如果沒有更新，則觸發程序狀態將顯示為**忽略**。  
![主要區域資料表](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid8.png)

根據時間間隔，累加式執行階段狀態會從主要區域複寫至次要地區。 在災難事件期間，沒有主要區域可用時，會對於商務持續性將流量導向次要區域。 

## <a name="next-steps"></a>後續步驟
[深入了解監視 B2B 訊息](logic-apps-monitor-b2b-message.md)。   


