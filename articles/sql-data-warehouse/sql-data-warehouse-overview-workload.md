---
title: "深入了解 Azure SQL 資料倉儲作業 |Microsoft Docs"
description: 'SQL Data Warehouse''s elasticity lets you grow, shrink, or pause compute power by using a sliding scale of data warehouse units (DWUs). This article explains the data warehouse metrics and how they relate to DWUs. '
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: cadffa9c-589d-4db7-888a-1f202a753bc5
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.translationtype: Human Translation
ms.sourcegitcommit: 43ab6a2f71ab51c50847b1ba5249f51c48e03fea
ms.openlocfilehash: 79fedaabc438bc4cd884af6b494e43d32361950a
ms.contentlocale: zh-tw
ms.lasthandoff: 01/24/2017


---
# <a name="data-warehouse-workload"></a>資料倉儲工作負載
資料倉儲工作負載是指所有針對資料倉儲所發生的作業。 資料倉儲工作負載包含將資料載入倉儲、對資料倉儲執行分析和報告、管理資料倉儲中的資料，以及從資料倉儲匯出資料的整個程序。 這些元件的廣度與深度多半與資料倉儲的成熟度相當。

## <a name="new-to-data-warehousing"></a>您是資料倉儲新手嗎？
資料倉儲是從一或多個資料來源載入的資料集合，並可用來執行商業智慧工作，例如報告和資料分析。

資料倉儲的特點在於查詢可以掃描數量較大的資料列、大範圍的資料，還可以傳回相對大量的結果以供用於分析和報告目的。 與小量交易層級的插入/更新/刪除作業相比，可載入相對大量的資料，也是資料倉儲的特點。

* 如果以掃描大量資料列或大範圍資料所需的最佳化查詢方式儲存資料，資料倉儲便能提供最佳效能。 如果資料是依資料行 (而非資料列) 來儲存和搜尋，這種掃描類型便能提供最佳效果。

> [!NOTE]
> 和用來報告和分析查詢的傳統二進位樹狀目錄相比，使用資料行儲存體的記憶體內部資料行存放區索引，最多可提升 10 倍壓縮速度與 100 倍查詢效能。 我們將資料行存放區索引視為在資料倉儲中儲存和掃描大量資料的標準。
> 
> 

* 資料倉儲和最佳化線上交易處理 (OLTP) 系統各有不同的需求。 OLTP 系統有許多插入、更新和刪除作業。 這些作業會搜尋資料表中的特定資料列。 如果資料以逐列方式儲存，資料表搜尋便能提供最佳效能。 資料可以使用分治法 (稱為二進位樹狀目錄或 btree 搜尋) 來排序和快速搜尋。

## <a name="data-loading"></a>載入資料
資料倉儲工作負載大部分都是在載入資料。 企業的 OLTP 系統通常忙於追蹤客戶一整天下來產生的商務交易變更。 通常在夜間進行的維護作業會定期將交易移動或複製到資料倉儲。 資料一旦進入資料倉儲，分析師就能夠執行分析，並根據資料做出商業決策。

* 傳統上，載入的程序稱為 ETL，也就是「擷取」(Extract)、「轉換」(Transform) 及「載入」(Load)。 資料通常需要進行轉換，才能與資料倉儲中的其他資料保持一致。 以前企業會使用專用的 ETL 伺服器來執行轉換。 現在要進行如此快速的大量平行處理程序，您可以先將資料載入 SQL 資料倉儲，然後再執行轉換。 這個稱為擷取、載入及轉換 (ELT) 的程序，正逐漸成為資料倉儲工作負載的全新標準。

> [!NOTE]
> 現在，使用 SQL Server 2016 就能即時在 OLTP 資料表上執行分析。 雖然這不會取代資料倉儲儲存和分析資料的需求，但的確能夠用來進行即時執行分析。
> 
> 

### <a name="reporting-and-analysis-queries"></a>報告和分析查詢
報告和分析查詢通常會根據幾個條件分成小型、中型和大型，但通常是根據時間。 大部分的資料倉儲中，還有混合快速執行與長時間執行查詢的工作負載。 在每個案例都務必判斷此混合方式以及它的頻率 (每小時、每天、月末、季末等等)。 請務必了解混合式查詢工作負載搭配並行存取，為資料倉儲適當規劃容量。

* 對於混合式的查詢工作負載而言，容量規劃是一項複雜的工作，尤其是當您需要很長的前置時間以將容量增加到資料倉儲時。 SQL 資料倉儲可讓容量規劃不再急迫，因為您可以隨時增加或縮減計算容量，而且能夠獨立地調整儲存體和計算容量。

### <a name="data-management"></a>資料管理
管理資料相當重要，尤其是當您知道不久之後可能會用盡磁碟空間。 資料倉儲通常會將資料分割成有意義的範圍，並儲存為資料表中的資料分割。 所有 SQL Server 式產品都可讓您將資料分割移入和移出資料表。 此資料分割切換可讓您將較舊的資料移至較便宜的儲存體，並將最新的資料保留在線上儲存體。

* 資料行存放區索引可支援經分割的資料表。 資料行存放區索引會使用經分割的資料表來管理和保存資料。 對於逐列儲存的資料表而言，資料分割在查詢效能方面的角色較為吃重。  
* PolyBase 在管理資料方面扮演著重要角色。 使用 PolyBase，您就能選擇將較舊的資料封存至 Hadoop 或 Azure blob 儲存體。  由於資料還在線上，這樣便能提供許多可用選項。  從 Hadoop 擷取資料可能需要較長的時間，但以擷取時間換取儲存體成本，可能會得不償失。

### <a name="exporting-data"></a>匯出資料
將資料提供給報告和分析使用的方法，就是將資料從資料倉儲傳送到專門執行報告和分析的伺服器。 這些伺服器稱為資料市集。 例如，您可以預先處理報告資料，然後將它從資料倉儲匯出至各個國家的伺服器中，以提供分佈各地的客戶和分析師使用。

* 您可以在每天晚上，將每天的資料快照集填入唯讀報表伺服器，以便產生報告。 這樣可為客戶提供較高的頻寬，同時降低資料倉儲的計算資源需求。 從安全性的角度來說，資料市集可減少存取資料倉儲的使用者數目。
* 如需進行分析，您可以在資料倉儲建立分析 Cube，然後針對資料倉儲執行分析，或是前置處理資料，然後再匯出到分析伺服器進行進一步分析。

## <a name="next-steps"></a>後續步驟
現在您已稍微了解 SQL 資料倉儲，請了解如何快速[建立 SQL 資料倉儲][create a SQL Data Warehouse]和[載入範例資料][load sample data]。

<!--Image references-->

<!--Article references-->
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other web references-->

