---
title: "Azure Cosmos DB 做為金鑰值存放區 – 成本概觀 | Microsoft Docs"
description: "了解使用 Azure Cosmos DB 做為金鑰值存放區的低成本。"
keywords: "金鑰值存放區"
services: cosmosdb
author: ArnoMicrosoft
manager: jhubbard
editor: 
tags: 
documentationcenter: 
ms.assetid: 7f765c17-8549-4509-9475-46394fc3a218
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: acomet
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: ca1ec638caf9a2452bc6ce3b5c2bf970a9688c74
ms.contentlocale: zh-tw
ms.lasthandoff: 05/10/2017

---

# <a name="azure-cosmos-db-as-a-key-value-store--cost-overview"></a>Azure Cosmos DB 做為金鑰值存放區 – 成本概觀

Azure Cosmos DB 是全域散發的多模型資料庫服務，可用來輕鬆建置具高可用性的大規模應用程式。 根據預設，Azure Cosmos DB 會自動有效率地編製它內嵌之所有資料的索引。 這樣可在任何種類的資料上進行快速且一致的 [SQL](documentdb-sql-query.md) (和 [JavaScript](documentdb-programming.md)) 查詢。 

本文說明 Azure Cosmos DB 做為金鑰值存放區時，進行簡單寫入與讀取作業的成本。 寫入作業包括文件的插入、取代、刪除和更新插入。 除了保證 99.99% 的高可用性，Azure Cosmos DB 還分別提供保證低於 10 毫秒延遲的讀取，以及低於 15 毫秒延遲的 (索引) 寫入 (99 百分位數)。 

## <a name="why-we-use-request-units-rus"></a>為什麼我們要使用「要求單位」(RU)

Azure Cosmos DB 效能是以分割區已佈建的[要求單位](documentdb-request-units.md) (RU) 數量為基礎。 佈建為第二個資料粒度，且以 RU/秒為單位購買 ([不應該與每小時計費混淆](https://azure.microsoft.com/pricing/details/documentdb/))。 RU 應該被視為可簡化佈建應用程式必要輸送量的貨幣。 我們的客戶不必去區別讀取和寫入容量單位。 RU 的單一貨幣模型可有效率地共用讀取和寫入之間已佈建的容量。 此佈建容量模型可讓服務提供可預測且一致的輸送量、保證低延遲以及高可用性。 最後，我們使用 RU 來建立輸送量的模型，但每個佈建的 RU 也會有定義的資源數量 (記憶體、核心)。 RU/秒不只是 IOPS。

做為全域散發的資料庫系統，Azure Cosmos DB 是除了高可用性以外，唯一就延遲、輸送量和一致性提供 SLA 的 Azure 服務。 您所佈建的輸送量會套用到與您 Azure Cosmos DB 資料庫帳戶相關聯的每一個區域。 針對讀取，Azure Cosmos DB 提供多個定義完善的[一致性層級](documentdb-consistency-levels.md)，以供您選擇。 Azure Cosmos DB 是全域散發的多模型資料庫服務，可用來輕鬆建置具高可用性的大規模[全域散發](documentdb-distribute-data-globally.md)應用程式。 根據預設，Cosmos DB 會自動有效率地編製它內嵌之所有資料的索引。 這樣可在任何種類的資料上進行快速且一致的 [SQL](documentdb-sql-query.md) (和 [JavaScript](documentdb-programming.md)) 查詢。 

本文說明 Cosmos DB 做為金鑰值存放區時，進行簡單寫入與讀取作業的成本。 寫入作業包括文件的插入、取代、刪除和更新插入。 除了保證 99.99% 的高可用性，Cosmos DB 還分別提供保證低於 10 毫秒延遲的讀取，以及低於 15 毫秒延遲的 (索引) 寫入 (99 百分位數)。 

## <a name="why-we-use-request-units-rus"></a>為什麼我們要使用「要求單位」(RU)

Cosmos DB 效能是以分割區已佈建的[要求單位](documentdb-request-units.md) (RU) 數量為基礎。 佈建為第二個資料粒度，且以 RU/秒和 RU/分為單位購買 ([不應該與每小時計費混淆](https://azure.microsoft.com/pricing/details/documentdb/))。 RU 應該被視為可簡化佈建應用程式必要輸送量的貨幣。 我們的客戶不必去區別讀取和寫入容量單位。 RU 的單一貨幣模型可有效率地共用讀取和寫入之間已佈建的容量。 此佈建容量模型可讓服務提供可預測且一致的輸送量、保證低延遲以及高可用性。 最後，我們使用 RU 來建立輸送量的模型，但每個佈建的 RU 也會有定義的資源數量 (記憶體、核心)。 RU/秒不只是 IOPS。

做為全域散發的資料庫系統，Cosmos DB 是除了高可用性以外，唯一就延遲、輸送量和一致性提供 SLA 的 Azure 服務。 您所佈建的輸送量會套用到與您 Cosmos DB 資料庫帳戶相關聯的每一個區域。 針對讀取，Cosmos DB 提供多個定義完善的[一致性層級](documentdb-consistency-levels.md)，以供您選擇。 

下表顯示根據 1KB 和 100KB 的文件大小，執行讀取和寫入交易所需要的 RU 數目。

|項目大小|1 次讀取|1 次寫入|
|-------------|------|-------|
|1 KB|1 RU|5 RU|
|100 KB|10 RU|50 RU|

## <a name="cost-of-reads-and-writes"></a>讀取和寫入的成本

如果您佈建 1,000 RU/每秒，數量會達 3.6 百萬 RU/每小時，且該小時將會花費 $0.08 (美國和歐洲)。 針對 1KB 大小的文件，這表示以您的佈建輸送量，您將會使用 3.6 百萬次讀取或 0.72 百萬次寫入 (3.6 百萬 RU / 5)。 標準化至百萬讀取和寫入，成本會是 $0.022/每百萬次讀取 ($0.08 / 3.6) 和 $0.111/每百萬次寫入 ($0.08 / 0.72)。 每百萬成本會變成最小值，如下表所示。

|項目大小|1 百萬次讀取|1 百萬次寫入|
|-------------|-------|--------|
|1 KB|$0.022|$0.111|
|100 KB|$0.222|$1.111|


大部分基本的 Blob 或物件存放區的服務收費，為每百萬次讀取交易 $0.40，以及每百萬次寫入交易 $5。 如果以最佳方式使用，Cosmos DB 可以比其他的解決方案節省多達 98% 的成本 (針對 1KB 交易)。

## <a name="next-steps"></a>後續步驟

敬請期待最佳化 Cosmos DB 資源佈建的新文章。 在此同時，歡迎使用我們的 [RU 計算機 (英文)](https://www.documentdb.com/capacityplanner)。


