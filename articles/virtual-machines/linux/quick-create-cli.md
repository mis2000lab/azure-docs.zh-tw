---
title: "Azure 快速入門 - 建立 VM CLI | Microsoft Docs"
description: "快速了解如何使用 Azure CLI 建立虛擬機器。"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: nepeters
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: 2f1f63d14468467c8cf3956324beb829adce296f
ms.contentlocale: zh-tw
ms.lasthandoff: 05/17/2017

---

# <a name="create-a-linux-virtual-machine-with-the-azure-cli"></a>使用 Azure CLI 建立 Linux 虛擬機器

Azure CLI 可用來從命令列或在指令碼中建立和管理 Azure 資源。 本指南詳細說明如何使用 Azure CLI 來部署執行 Ubuntu 伺服器的虛擬機器。 一旦部署伺服器，就會建立 SSH 連線，以及安裝 NGINX Web 伺服器。

如果您沒有 Azure 訂用帳戶，請在開始前建立 [免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。

本快速入門需要 Azure CLI 2.0.4 版或更新版本。 執行 `az --version` 以尋找版本。 如果您需要安裝或升級，請參閱[安裝 Azure CLI 2.0]( /cli/azure/install-azure-cli)。 您也可以從瀏覽器使用 [Cloud Shell](/azure/cloud-shell/quickstart)。

## <a name="log-in-to-azure"></a>登入 Azure 

使用 [az login](/cli/azure/#login) 命令登入 Azure 訂用帳戶並遵循畫面上的指示。

```azurecli
az login
```

## <a name="create-a-resource-group"></a>建立資源群組

使用 [az group create](/cli/azure/group#create) 命令來建立資源群組。 Azure 資源群組是在其中部署與管理 Azure 資源的邏輯容器。 

下列範例會在 eastus 位置建立名為 myResourceGroup 的資源群組。

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Create virtual machine

使用 [az vm create](/cli/azure/vm#create) 命令來建立 VM。 

下列範例會建立名為 myVM 的 VM，並建立 SSH 金鑰 (如果它們不存在於預設金鑰位置)。 若要使用一組特定金鑰，請使用 `--ssh-key-value` 選項。  

```azurecli
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

建立 VM 後，Azure CLI 會顯示類似下列範例的資訊。 記下 `publicIpAddress`。 此位址用來存取 VM。

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a>針對 Web 流量開啟連接埠 80 

依預設只能透過 SSH 連線至 Azure 中部署的 Linux 虛擬機器。 如果此 VM 即將成為 Web 伺服器，您需要從網際網路開啟連接埠 80。 使用 [az vm open-port](/cli/azure/vm#open-port) 命令來開啟所需的連接埠。  
 
 ```azurecli 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="ssh-into-your-vm"></a>透過 SSH 連線到您的 VM

使用下列命令，建立與虛擬機器的 SSH 工作階段。 務必以虛擬機器的正確公用 IP 位址取代 *<publicIpAddress>*。  在上面的範例中，我們的 IP 位址是 40.68.254.142。

```bash 
ssh <publicIpAddress>
```

## <a name="install-nginx"></a>安裝 NGINX

使用下列 bash 指令碼來更新套件來源及安裝最新的 NGINX 套件。 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a>檢視 NGINX 歡迎使用頁面

安裝 NGINX 後，現在經由網際網路在您的 VM 上開啟連接埠 80 - 您可以使用所選的網頁瀏覽器來檢視預設 NGINX 歡迎使用畫面。 請務必使用您上面記載的 publicIpAddress 來瀏覽預設網頁。 

![預設 NGINX 網站](./media/quick-create-cli/nginx.png) 


## <a name="delete-virtual-machine"></a>刪除虛擬機器

若不再需要，您可以使用 [az group delete](/cli/azure/group#delete) 命令來移除資源群組、VM 和所有相關資源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>後續步驟

在此快速入門中，您已部署簡單的虛擬機器、網路安全性群組規則，並已安裝 Web 伺服器。 若要深入了解 Azure 虛擬機器，請繼續 Linux VM 的教學課程。


> [!div class="nextstepaction"]
> [Azure Linux 虛擬機器教學課程](./tutorial-manage-vm.md)

