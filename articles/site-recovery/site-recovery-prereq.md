---
title: "使用 Azure Site Recovery 複寫至 Azure 的必要條件 | Microsoft Docs"
description: "本文摘要說明使用 Azure Site Recovery 服務將 VM 和實體機器複寫至 Azure 的必要條件。"
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/27/2017
ms.author: rajanaki
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 5ff598af73b6be727753ecac5b99f28bae19a417
ms.lasthandoff: 04/17/2017

---

#  <a name="prerequisites-for-replication-to-azure-by-using-azure-site-recovery"></a>使用 Azure Site Recovery 複寫至 Azure 的必要條件


Azure Site Recovery 服務可藉由將內部部署實體伺服器和虛擬機器的複寫協調至雲端 (Azure) 或次要資料中心，協助您建立商務持續性和災害復原 (BCDR) 策略。 當主要位置運作中斷時，您可以容錯移轉至次要位置，讓應用程式和工作負載保持可用。 當主要位置恢復正常作業時，您即可容錯回復至該位置。 如需有關 Site Recovery 的詳細資訊，請參閱[什麼是 Site Recovery？](site-recovery-overview.md)。

本文摘要說明開始 Site Recovery 至 Azure 複寫的必要條件。

在本文下方張貼意見，或在 [Azure Recovery Services Forum (Azure 復原服務論壇)](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) 提出技術問題。


## <a name="azure-requirements"></a>Azure 需求

**需求** | **詳細資料**
--- | ---
**Azure 帳戶** | [Microsoft Azure](http://azure.microsoft.com/) 帳戶。<br/><br/> 您可以從 [免費試用](https://azure.microsoft.com/pricing/free-trial/)開始。
**Site Recovery 服務** | 如需有關 Site Recovery 價格的詳細資訊，請參閱 [Site Recovery 價格](https://azure.microsoft.com/pricing/details/site-recovery/)。 |
**Azure 儲存體** | 您需要 Azure 儲存體帳戶來儲存複寫的資料，而且它與復原服務保存庫必須位於相同區域中。 複寫的資料會儲存在 Azure 儲存體，而在發生容錯移轉時會建立 Azure VM。<br/><br/> 依您想要針對容錯移轉的 Azure VM 使用的資源模型而定，可以在 [Azure Resource Manager 模型](../storage/storage-create-storage-account.md)或[傳統模型](../storage/storage-create-storage-account-classic-portal.md)中設定帳戶。<br/><br/>您可以使用[異地備援儲存體](../storage/storage-redundancy.md#geo-redundant-storage)或本地備援儲存體。 我們建議使用異地備援儲存體，以便在發生區域性停電或無法復原主要區域時，能夠恢復資料。<br/><br/> 您可使用標準或進階儲存體。 [進階儲存體](https://docs.microsoft.com/azure/storage/storage-premium-storage)通常是用於需要持續高 IO 效能和低延遲性以裝載 IO 密集型工作負載的虛擬機器。 如果使用進階儲存體存放複寫資料，您也需要標準儲存體帳戶來儲存複寫記錄檔，這些記錄檔會擷取內部部署資料進行中的變更。<br/><br/>
**儲存體限制** | 不論是跨資源群組，或是在訂用帳戶之內或跨訂用帳戶，您都不能移動 Site Recovery 中使用的儲存體帳戶。<br/><br/> 印度中部與印度南部目前不支援複寫至進階儲存體帳戶。
**Azure 網路** | 您需要 Azure 網路供 Azure VM 在容錯移轉之後連接，而且它與復原服務保存庫必須位於相同區域中。<br/><br/> 在 Azure 入口網站中，您可以在 [Resource Manager 模型](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)或[傳統模型](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)中建立網路。<br/><br/> 如果您從 System Center Virtual Machine Manager 複寫至 Azure，您將設定 Virtual Machine Manager VM 網路與 Azure 網路之間的網路對應，以確保 Azure VM 在容錯移轉之後連線到適當的網路。
**網路限制** | 不論是跨資源群組，或是在訂用帳戶之內或跨訂用帳戶，您都不能移動 Site Recovery 中使用的網路帳戶。
**網路對應** | 如果您在 Virtual Machine Manager 雲端中複寫 Hyper-V VM，則需要設定網路對應，以便容錯移轉之後，讓 Azure VM 在建立時連接至適當的網路。

>[!NOTE]
>以下各節說明客戶環境中各種元件的必要條件。 如需有關支援特定設定的詳細資訊，請參閱[支援矩陣](site-recovery-support-matrix.md)。
>

## <a name="disaster-recovery-of-vmware-virtual-machines-or-physical-windows-or-linux-servers-to-azure"></a>VMware 虛擬機器或實體 Windows 或 Linux 伺服器至 Azure 的災害復原
除了在 [Azure 需求](#Azure requirements)提及的元件，以下是 VMware 虛擬機器或實體 Windows 或 Linux 伺服器災害復原所需的元件。


### <a name="configuration-server-or-additional-process-server-you-will-need-to-set-up-an-on-premises-machine-as-the-configuration-server-to-coordinate-communications-between-the-on-premises-site-and-azure-and-to-manage-data-replication-brbr"></a>**設定伺服器或其他處理伺服器**︰您需要設定一台內部部署電腦作為設定伺服器，以協調內部部署網站和 Azure 之間的通訊，以及管理資料複寫。 <br></br>

1. **VMware vCenter 或 vSphere 主機**

| **元件** | **需求** |
| --- | --- |
| **vSphere** | 一個或多個 VMware vSphere Hypervisor。<br/><br/>Hypervisor 應該執行 vSphere 6.0、5.5 或 5.1 版 (含最新更新)。<br/><br/>我們建議 vSphere 主機與 vCenter 伺服器應位於和處理序伺服器相同的網路中。 這是設定伺服器所在的網路，除非您已設定了專用的處理序伺服器。 |
| **vCenter** | 建議您部署 VMware vCenter 伺服器來管理您的 vSphere 主機。 它應該執行 vCenter 6.0 或 5.5 版 (含最新更新)。<br/><br/>**限制**：Site Recovery 不支援跨 vCenter vMotion。 完成重新保護作業後，主要目標虛擬機器亦不支援儲存體 DRS 和 Storage vMotion。||

1. **複寫之機器的必要條件**


| **元件** | **需求** |
| --- | --- |
| **內部部署** (VMware VM) | 複寫的 VM 應該安裝並執行 VMware 工具。<br/><br/> VM 應該要符合建立 Azure VM 的 [Azure 必要條件](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements)。<br/><br/>受保護機器上的個別磁碟容量不可超過 1,023 GB。 <br/><br/>安裝磁碟機上必須至少有 2 GB 的可用空間來進行元件安裝。<br/><br/>如果您想要啟用多部 VM 一致性，應該將 VM 本機防火牆上的連接埠 20004 開啟。<br/><br/>機器名稱應包含介於 1 到 63 個字元 (字母、數字和連字號)。 名稱必須以字母或數字開頭，並以字母或數字結尾。 您可以在為機器啟用複寫後修改 Azure 名稱。<br/><br/> |
| **Windows 機器** (實體或 VMware) | 機器應該執行受支援的 64 位元作業系統：Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 (至少為 SP1)。<br/><br/> 作業系統應安裝在 C 磁碟機上。OS 磁碟應該是 Windows 基本磁碟而非動態磁碟。 資料磁碟可以為動態。<br/><br/>|
| **Linux 機器** (實體或 VMware) | 您需要受支援的 64 位元作業系統：Red Hat Enterprise Linux 6.7、6.8、7.1 或 7.2；Centos 6.5、6.6、6.7、6.8、7.0、7.1 或 7.2；Oracle Enterprise Linux 6.4 或 6.5 (執行 Red Hat 相容核心或 Unbreakable Enterprise Kernel 第 3 版 (UEK3)、SUSE Linux Enterprise Server 11 SP3、SUSE Linux Enterprise Server 11 SP4。<br/><br/>受保護機器上您的 /etc/hosts 檔案應該包含將本機主機名稱對應到所有網路介面卡相關聯之 IP 位址的項目。<br/><br/>如果您想要在容錯轉移之後使用安全殼層用戶端 (ssh) 連線到執行 Linux 的 Azure 虛擬機器，請確認受保護機器上的安全殼層服務已設為在系統開機時自動啟動，且防火牆規則允許 ssh 與其連線。<br/><br/>主機名稱、掛接點、裝置名稱和 Linux 系統路徑和檔案名稱 (例如 /etc/; /usr) 僅可使用英文。<br/><br/>下列目錄必須一律位於來源伺服器的同個磁碟 (OS 磁碟) (若設為獨立資料分割/檔案系統)：/ (root)、/boot、/usr、/usr/local、/var、/ 等等<br/><br/>諸如中繼資料總和檢查碼等 XFS v5 功能，目前不受 XFS 檔案系統的 ASR 所支援。 請確定 XFS 檔案系統未使用任何 v5 功能。 您可使用 xfs_info 公用程式來檢查資料分割的 XFS 超級區塊。 若 ftype 設為 1，則會使用 XFSv5 功能。<br/><br/>在 Red Hat Enterprise Linux 7 和 CentOS 7 伺服器上，必須安裝和提供可用的 lsof 公用程式。<br/><br/>


## <a name="disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager"></a>Hyper-V 虛擬機器至 Azure (無 Virtual Machine Manager) 的災害復原

除了在 [Azure 需求](#Azure requirements)提及的元件，以下是 Virtual Machine Manager 雲端中 Hyper-V 虛擬機器災害復原所需的元件。

| **必要條件** | **詳細資料** |
| --- | --- |
| **Hyper-V 主機** |一或多部執行含最新更新且已啟用 Hyper-V 角色之 Windows Server 2012 R2 或執行 Microsoft Hyper-V Server 2012 R2 的內部部署伺服器。<br/><br/>Hyper-V 伺服器應包含一或多部虛擬機器。<br/><br/>Hyper-V 伺服器應該直接或透過 Proxy 連接到網際網路。<br/><br/>Hyper-V 伺服器應該已安裝 [KB2961977](https://support.microsoft.com/kb/2961977) 中提及的修正程式。
|**Provider 和代理程式**| 在 Azure Site Recovery 部署期間，您會安裝 Azure Site Recovery Provider。 Provider 安裝也會在每部執行要保護之虛擬機器的 Hyper-V 伺服器上安裝 Azure 復原服務代理程式。 <br/><br/>Site Recovery 保存庫中的所有 Hyper-V 伺服器應該有相同版本的提供者和代理程式。<br/><br/>提供者必須透過網際網路連接到 Azure Site Recovery。 可以直接或透過 Proxy 傳送流量。 不支援 HTTPS 型 Proxy。 Proxy 伺服器應該要允許對下列位置的存取：<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/>如果您在伺服器上有以 IP 位址為基礎的防火牆規則，請確定這些規則允許對 Azure 的通訊。<br/><br/> 允許 [Azure 資料中心 IP 範圍](https://www.microsoft.com/download/confirmation.aspx?id=41653)和 HTTPS (443) 連接埠。<br/><br/> 允許訂用帳戶的 Azure 區域和美國西部使用 IP 位址範圍 (用於存取控制和身分識別管理)。


## <a name="disaster-recovery-of-hyper-v-virtual-machines-in-virtual-machine-manager-clouds-to-azure"></a>在 Virtual Machine Manager 雲端中的 Hyper-V 虛擬機器至 Azure 的災害復原

除了在 [Azure 需求](#Azure requirements)提及的元件，以下是 Virtual Machine Manager 雲端中 Hyper-V 虛擬機器災害復原所需的元件。

| **必要條件** | **詳細資料** |
| --- | --- |
| **Virtual Machine Manager** |在 **System Center 2012 R2 或更新版本**上執行的一或多個 Virtual Machine Manager 伺服器。 每部 Virtual Machine Manager 伺服器應設定一或多個雲端。 <br/><br/>雲端應該包含：<br>- 一或多個 Virtual Machine Manager 主機群組。<br/>- 每個主機群組中的一或多個 Hyper-V 主機伺服器或叢集。<br/><br/>如需設定 Virtual Machine Manager 雲端的詳細資訊，請參閱[如何在 VMM 2012 中建立雲端](http://social.technet.microsoft.com/wiki/contents/articles/2729.how-to-create-a-cloud-in-vmm-2012.aspx)。 |
| **Hyper-V** |Hyper-V 主機伺服器必須至少執行 Windows Server 2012 R2 (具有 Hyper-V 角色) 或 Microsoft Hyper-V Server 2012 R2 且已安裝最新的更新。<br/><br/> Hyper-V 伺服器應該包含一或多部 VM。<br/><br/> 包含您想要複寫之 VM 的 Hyper-V 主機伺服器或叢集必須在 Virtual Machine Manager 雲端中管理。<br/><br/>Hyper-V 伺服器必須直接或透過 Proxy 連接到網際網路。<br/><br/>Hyper-V 伺服器必須已安裝 [2961977](https://support.microsoft.com/kb/2961977) 文章中提及的修正程式。<br/><br/>Hyper-V 主機伺服器需要網際網路存取權以將資料複寫至 Azure。 |
| **Provider 和代理程式** |在 Azure Site Recovery 部署期間，請在 Virtual Machine Manager 伺服器上安裝 Azure Site Recovery Provider 以及在 Hyper-V 主機上安裝復原服務代理程式。 Provider 和代理程式必須透過網際網路直接連接，或透過 Proxy 連接至 Azure。 不支援 HTTPS 型 Proxy。 Virtual Machine Manager 伺服器上的 Proxy 伺服器和 Hyper-V 主機應該允許存取︰ <br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>如果您在 Virtual Machine Manager 伺服器上有以 IP 位址為基礎的防火牆規則，請確定這些規則允許對 Azure 進行通訊。<br/><br/> 允許 [Azure 資料中心 IP 範圍](https://www.microsoft.com/download/confirmation.aspx?id=41653)和 HTTPS (443) 連接埠。<br/><br/>允許訂用帳戶的 Azure 區域和美國西部使用 IP 位址範圍 (用於存取控制和身分識別管理)。<br/><br/> |

### <a name="replicated-machine-prerequisites"></a>複寫之機器的必要條件
| **元件** | **詳細資料** |
| --- | --- |
| **受保護的 VM** | Site Recovery 支援 [Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx) 支援的所有作業系統。<br/><br/>VM 應該要符合建立 Azure VM 的 [Azure 必要條件](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements)。 機器名稱應包含介於 1 到 63 個字元 (字母、數字和連字號)。 名稱必須以字母或數字開頭，並以字母或數字結尾。 <br/><br/>您可以在啟用 VM 的複寫後修改此名稱。 <br/><br/> 受保護機器上的個別磁碟容量不可超過 1,023 GB。 VM 最多可以有 16 個磁碟 (因此最多可達 16 TB)。<br/><br/>


## <a name="disaster-recovery-of-hyper-v-virtual-machines-in-virtual-machine-manager-clouds-to-a-customer-owned-site"></a>在 Virtual Machine Manager 雲端中的 Hyper-V 虛擬機器至客戶所擁有網站的災害復原

除了在 [Azure 需求](#Azure requirements)提及的元件，以下是 Virtual Machine Manager 雲端中 Hyper-V 虛擬機器至客戶所擁有網站的災害復原所需元件。

| **元件** | **詳細資料** |
| --- | --- |
| **Virtual Machine Manager** |  我們建議您在主要網站中部署 Virtual Machine Manager 伺服器，以及在次要網站中部署 Virtual Machine Manager 伺服器。<br/><br/> 您可以[在單一 VMM 伺服器上的雲端之間進行複寫](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment)。 若要這樣做，您必須在 Virtual Machine Manager 伺服器上至少設定兩個雲端。<br/><br/> Virtual Machine Manager 伺服器至少應執行含有最新更新的 System Center 2012 SP1。<br/><br/> 每個 Virtual Machine Manager 伺服器至少必須有一或多個雲端。 所有的雲端都必須設定 Hyper-V 容量設定檔。 <br/><br/>雲端必須包含一或多個 Virtual Machine Manager 主機群組。 如需設定 Virtual Machine Manager 雲端的詳細資訊，請參閱 [Azure Site Recovery 部署的準備](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)。 |
| **Hyper-V** | Hyper-V 伺服器必須至少執行具備 Hyper-V 角色並已安裝最新更新的 Windows Server 2012。<br/><br/> Hyper-V 伺服器應該包含一或多部 VM。<br/><br/>  Hyper-V 主機伺服器應該位於主要和次要 VMM 雲端的主機群組中。<br/><br/> 如果您在 Windows Server 2012 R2 上的叢集中執行 Hyper-V，我們建議安裝[更新 2961977](https://support.microsoft.com/kb/2961977)。<br/><br/> 如果您是在 Windows Server 2012 上的叢集中執行 Hyper-V，且您的叢集是靜態 IP 位址型叢集，則不會自動建立叢集代理。 您必須手動設定叢集代理。 如需叢集代理的詳細資訊，請參閱[設定複本代理角色叢集對叢集複寫](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx)。 |
| **提供者** | 在 Site Recovery 部署期間，請在 Virtual Machine Manager 伺服器上安裝 Azure Site Recovery Provider。 Provider 會透過 HTTPS 443 與 Site Recovery 通訊來協調複寫。 資料複寫是透過 LAN 或 VPN 連線在主要和次要 Hyper-V 伺服器之間進行。<br/><br/> Virtual Machine Manager 伺服器上執行的提供者需要存取這些 URL：<br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>提供者必須允許從 Virtual Machine Manager 伺服器至 [Azure 資料中心 IP 範圍](https://www.microsoft.com/download/confirmation.aspx?id=41653)的防火牆通訊，並允許 HTTPS (443) 通訊協定。 |


## <a name="url-access"></a>URL 存取
這些 URL 應可透過 VMware、VMM 和 Hyper-V 主機伺服器提供。

|**URL** | **VMM 至 VMM** | **VMM 至 Azure** | **Hyper-V 至 Azure** | **VMware 至 Azure** |
|--- | --- | --- | --- | --- |
|``*.accesscontrol.windows.net`` | 允許 | 允許 | 允許 | 允許 |
|``*.backup.windowsazure.com`` | 不需要 | 允許 | 允許 | 允許 |
|``*.hypervrecoverymanager.windowsazure.com`` | 允許 | 允許 | 允許 | 允許 |
|``*.store.core.windows.net`` | 允許 | 允許 | 允許 | 允許 |
|``*.blob.core.windows.net`` | 不需要 | 允許 | 允許 | 允許 |
|``https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi`` | 不需要 | 不需要 | 不需要 | 允許 SQL 下載 |
|``time.windows.com`` | 允許 | 允許 | 允許 | 允許|
|``time.nist.gov`` | 允許 | 允許 | 允許 | 允許 |

