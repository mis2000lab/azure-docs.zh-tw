---
title: "使用 Robomongo 連絡 Azure Cosmos DB | Microsoft Docs"
description: "了解如何使用 Robomongo 搭配 Azure Cosmos DB：適用於 MongoDB 的 API 帳戶"
keywords: robomongo
services: cosmosdb
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/28/2017
ms.author: anhoh
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: bad2d57a6252bb30e4c8435c52da19c95304d8da
ms.contentlocale: zh-tw
ms.lasthandoff: 05/10/2017


---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>使用 Robomongo 搭配 Azure Cosmos DB：適用於 MongoDB 的 API 帳戶
若要使用 Robomongo 連線到 Azure Cosmos DB：適用於 MongoDB 的 API 帳戶，您必須︰

* 下載並安裝 [Robomongo](https://robomongo.org/)
* 具備您的 Azure Cosmos DB：適用於 MongoDB 的 API 帳戶[連接字串](documentdb-connect-mongodb-account.md)資訊

## <a name="connect-using-robomongo"></a>使用 Robomongo 來連接
若要將 Azure Cosmos DB：適用於 MongoDB 的 API 帳戶新增至 Robomongo MongoDB 連線，請執行下列步驟。

1. 使用[這裡](documentdb-connect-mongodb-account.md)的指示來擷取 Azure Cosmos DB：適用於 MongoDB 的 API 帳戶的連線資訊。

    ![[連接字串] 刀鋒視窗的螢幕擷取畫面](./media/documentdb-mongodb-robomongo/connectionstringblade.png)
2. 執行 *Robomongo.exe*

3. 按一下 [檔案] 下的 [連接] 按鈕來管理您的連接。 然後，按一下 [MongoDB 連接] 視窗中的 [建立]，將會開啟 [連接設定] 視窗。

4. 在 [連接設定] 視窗中，選擇名稱。 然後，從步驟 1 的連接資訊中，找出 [主機] 和 [連接埠]，分別輸入至 [位址] 和 [連接埠]。

    ![Robomongo 管理連線的螢幕擷取畫面](./media/documentdb-mongodb-robomongo/manageconnections.png)
5. 在 [驗證] 索引標籤上，按一下 [執行驗證]。 然後，輸入您的 [資料庫] \(預設值是 *Admin*)、[使用者名稱] 和 [密碼]。
在步驟 1 的連接資訊中，可以找到 [使用者名稱] 和 [密碼]。

    ![Robomongo 驗證索引標籤的螢幕擷取畫面](./media/documentdb-mongodb-robomongo/authentication.png)
6. 在 [SSL] 索引標籤上，勾選 [使用 SSL 通訊協定]，然後將 [驗證方法] 變更為 [自我簽署憑證]。

    ![Robomongo SSL 索引標籤的螢幕擷取畫面](./media/documentdb-mongodb-robomongo/SSL.png)
7. 最後，按一下 [測試] 以確認您能夠連接，然後按一下 [儲存]。

## <a name="next-steps"></a>後續步驟
* 瀏覽 Azure Cosmos DB：適用於 MongoDB 的 API [範例](documentdb-mongodb-samples.md)。

