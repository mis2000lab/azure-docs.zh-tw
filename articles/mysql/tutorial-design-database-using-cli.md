---
title: "設計您第一個適用於 MySQL 資料庫的 Azure 資料庫 - Azure CLI | Microsoft Docs"
description: "本教學課程說明如何使用 Azure CLI 2.0 來建立和管理「適用於 MySQL 的 Azure 資料庫」伺服器。"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql-database
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: portal
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 99238b9c0bcc7fa80e8c72cda25f7ed809cc5195
ms.contentlocale: zh-tw
ms.lasthandoff: 05/10/2017

---

# <a name="design-your-first-azure-database-for-mysql-database"></a>設計您第一個適用於 MySQL 資料庫的 Azure 資料庫

「適用於 MySQL 的 Azure 資料庫」是 Microsoft 雲端中以 MySQL Community Edition 資料庫引擎為基礎的關聯式資料庫服務。 在本教學課程中，您將使用 Azure CLI (命令列介面) 及其他公用程式來學習如何：

> [!div class="checklist"]
> * 建立適用於 MySQL 的 Azure 資料庫
> * 設定伺服器防火牆 使用 [mysql 命令列工具](https://dev.mysql.com/doc/refman/5.6/en/mysql.html)來建立資料庫
> * 載入範例資料
> * 查詢資料
> * 更新資料
> * 還原資料

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

## <a name="log-in-to-azure"></a>登入 Azure

使用 [az login](/cli/azure/#login) 命令登入 Azure 訂用帳戶並遵循畫面上的指示。

```azurecli
az login
```
依照命令提示字元指示在瀏覽器中開啟 URL https://aka.ms/devicelog，然後輸入在「命令提示字元」中產生的程式碼。

## <a name="create-a-resource-group"></a>建立資源群組
使用 [az group create](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) 命令來建立 [Azure 資源群組](https://docs.microsoft.com/cli/azure/group#create)。 資源群組是在其中以群組方式部署與管理 Azure 資源的邏輯容器。

下列範例會在 `westus` 位置建立名為 `mycliresource` 的資源群組。

```azurecli
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>建立適用於 MySQL 的 Azure 資料庫伺服器
使用 az mysql server create 命令來建立「適用於 MySQL 的 Azure 資料庫」伺服器。 一部伺服器可以管理多個資料庫。 一般而言，每個專案或每個使用者會使用個別的資料庫。

下列範例會在資源群組 `mycliresource` 的 `westus` 中建立名稱為 `mycliserver` 的「適用於 MySQL 的 Azure 資料庫」伺服器。 此伺服器具有一個名為 `myadmin` 且密碼為 `Password01!` 的系統管理員登入。 此伺服器是以 **Basic** (基本) 效能層級建立，並且伺服器中的所有資料庫之間共用 **50** 個計算單位。 您可以視應用程式需求而定，相應增加或減少計算和儲存體規模。

```azurecli
az mysql server create --resource-group mycliresource --name mycliserver
--location westus --user myadmin --password Password01!
--performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>設定防火牆規則
使用 az mysql server firewall-rule create 命令來建立「適用於 MySQL 的 Azure 資料庫」伺服器層級防火牆規則。 伺服器層級防火牆規則可允許外部應用程式 (例如 **mysql** 命令列工具或 MySQL Workbench) 穿過 Azure MySQL 服務防火牆連線到您的伺服器。 

下列範例會針對一個預先定義的位址範圍建立防火牆規則。 此範例會顯示整個可能的 IP 位址範圍。

```azurecli
az mysql server firewall-rule create --resource-group mycliresource
--server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0
--end-ip-address 255.255.255.255
```

## <a name="get-the-connection-information"></a>取得連線資訊

若要連線到您的伺服器，您必須提供主機資訊和存取認證。
```azurecli
az mysql server show --resource-group mycliresource --name mycliserver
```

結果會採用 JSON 格式。 請記下 **fullyQualifiedDomainName** 和 **administratorLogin**。
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mycliserver.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mycliresource/providers/Microsoft.DBforMySQL/servers/mycliserver",
  "location": "westus",
  "name": "mycliserver",
  "resourceGroup": "mycliresource",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "MYSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

## <a name="connect-to-the-server-using-mysql"></a>使用 mysql 來連線到伺服器
使用 [mysql 命令列工具](https://dev.mysql.com/doc/refman/5.6/en/mysql.html)來建立對「適用於 MySQL 的 Azure 資料庫」伺服器的連線。 在此範例中，命令是：
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a>建立空白資料庫
連線到伺服器之後，請建立一個空白資料庫。
```sql
mysql> CREATE DATABASE mysampledb;
```

在提示字元，執行下列命令以將連線切換到這個新建立的資料庫：
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>在資料庫中建立資料表
既然您已知道如何連線到「適用於 MySQL 資料庫的 Azure 資料庫」，我們可以了解一下如何完成一些基本工作。

首先，我們可以建立資料表並在其中載入一些資料。 我們將建立一個儲存清查資訊的資料表。
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>將資料載入到資料表
既然我們已有資料表，我們可以在其中插入一些資料。 在開啟的命令提示字元視窗，執行下列查詢以插入幾列資料。
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

您現在已將兩列範例資料插入到先前建立的資料表。

## <a name="query-and-update-the-data-in-the-tables"></a>查詢並更新資料表中的資料
執行下列查詢，以從資料庫資料表中擷取資訊。
```sql
SELECT * FROM inventory;
```

您也可以更新資料表中的資料。
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

當您擷取資料時，資料列會相應地更新。
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>將資料庫還原至先前的時間點
想像一下您不小心刪除了這個資料表。 這是您無法輕易復原的情況。 「適用於 MySQL 的 Azure 資料庫」可讓您返回到最長可達過去 35 天內的任何時間點，並將此時間點還原到新伺服器。 您可以使用這個新的伺服器來復原已刪除的資料。 下列步驟會將範例伺服器還原到新增資料表之前的時間點。

若要進行還原，您需要下列資訊︰

- 還原點：選取在變更伺服器之前的時間點。 必須大於或等於來源資料庫的最舊備份值。
- 目標伺服器︰提供要作為還原目的地的新伺服器名稱
- 來源伺服器︰提供要作為還原來源的伺服器名稱
- 位置︰您無法選取區域，預設是與來源伺服器相同的區域
- 定價層︰還原伺服器時，您無法變更此值。 它與來源伺服器相同。 

```azurecli
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

還原伺服器並[還原到刪除資料表之前的時間點](./howto-restore-server-portal.md)。 如果將伺服器還原到不同的時間點，將會從您指定的時間點 (前提是此時間點在您[服務層](./concepts-service-tiers.md)的保留期限內) 開始，建立重複的新伺服器作為原始伺服器。

## <a name="next-steps"></a>後續步驟
在本教學課程中，您已了解：
> [!div class="checklist"]
> * 建立適用於 MySQL 的 Azure 資料庫
> * 設定伺服器防火牆 使用 [mysql 命令列工具](https://dev.mysql.com/doc/refman/5.6/en/mysql.html)來建立資料庫
> * 載入範例資料
> * 查詢資料
> * 更新資料
> * 還原資料

[適用於 MySQL 的 Azure 資料庫 - Azuer CLI 範例](./sample-scripts-azure-cli.md)

