---
title: "Azure 診斷記錄檔概觀 | Microsoft Docs"
description: "認識 Azure 診斷記錄檔，並了解如何利用它們來了解 Azure 資源內發生的事件。"
author: johnkemnetz
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: johnkem; magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 1978ecda9c635ace713b43f620300a06f4c609ba
ms.contentlocale: zh-tw
ms.lasthandoff: 05/10/2017


---
# <a name="collect-and-consume-diagnostic-data-from-your-azure-resources"></a>收集並取用來自 Azure 資源的診斷資料

## <a name="what-are-azure-diagnostic-logs"></a>Azure 診斷記錄是什麼
**Azure 診斷記錄檔** 是由資源發出的記錄檔，提供有關該資源之作業的豐富、經常性資料。 這些記錄的內容會依資源類型而有所不同。 例如，Windows 事件系統記錄是適用於 VM、Blob 和資料表的一個診斷記錄類別，而佇列記錄則是適用於儲存體帳戶的診斷記錄類別。

診斷記錄不同於[活動記錄 (之前稱為稽核記錄或作業記錄)](monitoring-overview-activity-logs.md)。 活動記錄能讓您了解訂用帳戶中的資源所執行之作業。 診斷記錄能讓您了解資源自行執行的作業。

並非所有資源皆支援此處所述的新型診斷記錄檔。 本文有一個區段會列出哪些資源類型支援新的診斷記錄。

![診斷記錄與其他類型的記錄 ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

圖 1：診斷記錄與其他類型的記錄

## <a name="what-you-can-do-with-diagnostic-logs"></a>診斷記錄檔的用途
以下是您可以利用診斷記錄檔進行的事：

![診斷記錄檔的邏輯位置](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)


* 將診斷記錄檔儲存到[**儲存體帳戶**](monitoring-archive-diagnostic-logs.md)以利稽核或手動檢查。 您可以使用 **診斷設定**指定保留時間 (以天為單位)。
* [將診斷記錄檔串流至**事件中樞**](monitoring-stream-diagnostic-logs-to-event-hubs.md)，以供第三方服務或自訂的分析解決方案 (如 PowerBI) 擷取。
* 以 [OMS Log Analytics](../log-analytics/log-analytics-azure-storage.md) 分析記錄檔

您可以使用並非發出記錄的同一個訂用帳戶中的儲存體帳戶或事件中樞命名空間。 進行此設定的使用者必須具有這兩個訂用帳戶的適當 RBAC 存取權。

## <a name="diagnostic-settings"></a>診斷設定
非計算資源的診斷記錄檔要使用「診斷設定」來設定。 **診斷設定** ：

* 診斷記錄檔傳送至何處 (儲存體帳戶、事件中樞和/或 OMS Log Analytics)。
* 傳送何種類別的記錄檔。
* 每個記錄類別應該在儲存體帳戶中保留多久
    - 保留期為 0 天表示會永遠保留記錄。 否則，此值可以是 1 到 2147483647 之間的任意天數。
    - 如果有設定保留原則，但將儲存體帳戶的記錄檔儲存停用 (例如，如果只選取事件中樞或 OMS 選項)，保留原則不會有任何作用。
    - 保留原則是每天套用，因此在一天結束時 (UTC)，這一天超過保留原則的記錄會被刪除。 例如，如果您的保留原則為一天，在今天一開始，昨天之前的記錄檔會被刪除。

透過 Azure 入口網站中資源的 [診斷] 刀鋒視窗、透過 Azure PowerShell 和 CLI 命令、或是透過 [Azure 監視器 REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx)，可以輕鬆地設定這些設定。

> [!WARNING]
> 計算資源 (例如，VM 或 Service Fabric) 的診斷記錄檔和度量使用 [不同機制](../azure-diagnostics.md)來進行設定與選取輸出。
>
>

## <a name="how-to-enable-collection-of-diagnostic-logs"></a>如何啟用診斷記錄檔的收集
您可以在建立資源的過程中啟用收集診斷記錄檔，或是在資源建立之後透過入口網站中資源的刀鋒視窗啟用收集。 您也可以在任何時間點使用 Azure PowerShell 或 CLI 命令，或使用 Azure 監視器 REST API 啟用診斷記錄檔。

> [!TIP]
> 這些指示可能無法直接套用於每個資源。 請透過此頁面底部的結構描述連結，來了解適用於特定資源類型的特殊步驟。
>
>

[本文將說明如何使用資源範本在建立資源時啟用診斷設定](monitoring-enable-diagnostic-logs-using-template.md)

### <a name="enable-diagnostic-logs-in-the-portal"></a>在入口網站中啟用診斷記錄檔
當您以啟用 Windows 或 Linux Azure 診斷擴充功能的方式建立資源類型時，可以在 Azure 入口網站中啟用診斷記錄檔︰

1. 移至 [ **新增** ] 並選擇您感興趣的資源。
2. 設定基本設定並選取大小之後，在 [監視] 底下的 [設定] 刀鋒視窗中，選取 [啟用]，然後選擇您想要儲存診斷記錄檔的儲存體帳戶。 將診斷傳送至儲存體帳戶時，您將需要支付儲存和交易的一般數據傳輸費用。

   ![在資源建立期間啟用診斷記錄檔](./media/monitoring-overview-of-diagnostic-logs/enable-portal-new.png)
3. 按一下 [ **確定** ]，並建立資源。

針對非計算資源，若要在資源建立之後在 Azure 入口網站中啟用診斷記錄檔，可執行下列作業︰

1. 移至資源的刀鋒視窗，開啟 [診斷]  刀鋒視窗。
2. 按一下 [開啟]  並選取儲存體帳戶和/或事件中樞。

   ![在資源建立之後啟用診斷記錄檔](./media/monitoring-overview-of-diagnostic-logs/enable-portal-existing.png)
3. 在 [記錄] 下，選取您要收集或資料流哪些 [記錄檔分類]。
4. 按一下 [儲存] 。

### <a name="enable-diagnostic-logs-via-powershell"></a>透過 PowerShell 啟用診斷記錄檔
若要透過 Azure PowerShell Cmdlet 啟用診斷記錄檔，請使用下列命令。

若要啟用儲存體帳戶中的診斷記錄檔的儲存體，使用下列命令︰

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

儲存體帳戶識別碼是您要將記錄檔傳送至此的儲存體帳戶的資源識別碼。

若要啟用將診斷記錄檔串流至事件中樞，使用下列命令︰

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
```

服務匯流排規則識別碼是此格式的字串︰ `{service bus resource ID}/authorizationrules/{key name}`。

若要啟用將診斷記錄檔傳送到 Log Analytics 工作區，請使用此命令︰

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
```

您可以使用下列命令取得 Log Analytics 工作區的資源識別碼︰

```powershell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId
```

您可以結合這些參數讓多個輸出選項。

### <a name="enable-diagnostic-logs-via-cli"></a>透過 CLI 啟用診斷記錄檔
若要透過 Azure CLI 啟用診斷記錄檔，使用下列命令：

若要啟用儲存體帳戶中的診斷記錄檔的儲存體，使用下列命令︰

```azurecli
    azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
```

儲存體帳戶識別碼是您要將記錄檔傳送至此的儲存體帳戶的資源識別碼。

若要啟用將診斷記錄檔串流至事件中樞，使用下列命令︰

```azurecli
    azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

服務匯流排規則識別碼是此格式的字串︰ `{service bus resource ID}/authorizationrules/{key name}`。

若要啟用將診斷記錄檔傳送到 Log Analytics 工作區，請使用此命令︰

```azurecli
    azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
```

您可以結合這些參數讓多個輸出選項。

### <a name="enable-diagnostic-logs-via-rest-api"></a>透過 REST API 啟用診斷記錄檔
若要使用 Azure 監視器 REST API 變更診斷設定，請參閱[這份文件](https://msdn.microsoft.com/library/azure/dn931931.aspx)。

## <a name="manage-diagnostic-settings-in-the-portal"></a>在入口網站中管理診斷設定
請確定所有資源皆已設定了診斷設定。 瀏覽至入口網站中的 [監視] 刀鋒視窗，然後開啟 [診斷記錄] 刀鋒視窗。

![入口網站中的診斷記錄檔刀鋒視窗](./media/monitoring-overview-of-diagnostic-logs/manage-portal-nav.png)

您可能必須按一下 [更多服務] 來尋找 [監視] 刀鋒視窗。

在此刀鋒視窗中，您可以檢視和篩選所有支援診斷記錄的資源，以查看它們是否已啟用診斷。 您也可以檢查這些記錄流向哪一個儲存體帳戶、事件中樞和/或 Log Analytics 工作區。

![入口網站中的診斷記錄檔刀鋒視窗結果](./media/monitoring-overview-of-diagnostic-logs/manage-portal-blade.png)

按一下資源，即會顯示已儲存在儲存體帳戶的所有記錄，並提供您關閉或修改診斷設定的選項。 按一下 [下載] 圖示來下載一段特定時間的記錄。

![一項資源的診斷記錄檔刀鋒視窗](./media/monitoring-overview-of-diagnostic-logs/manage-portal-logs.png)

> [!NOTE]
> 僅在您已設定診斷設定以將它們儲存到儲存體帳戶時，診斷記錄才會出現在此檢視中並可供下載。
>
>

按一下**診斷設定**的連結會顯示 [診斷設定] 刀鋒視窗，您可以在其中啟用、停用、修改所選取資源的診斷設定。

## <a name="supported-services-and-schema-for-diagnostic-logs"></a>支援的服務以及診斷記錄檔的結構描述
診斷記錄檔的結構描述會根據資源和記錄類別而有所不同。   

| 服務 | 結構描述與文件 |
| --- | --- |
| API 管理 | 無法使用結構描述。 |
| 應用程式閘道 |[應用程式閘道的診斷記錄功能](../application-gateway/application-gateway-diagnostics.md) |
| Azure 自動化 |[Azure 自動化的記錄檔分析](../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Azure Batch 診斷記錄](../batch/batch-diagnostics.md) |
| Customer Insights | 無法使用結構描述。 |
| 內容傳遞網路 | 無法使用結構描述。 |
| 資料湖分析 |[存取 Azure Data Lake Analytics 的診斷記錄](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[存取 Azure Data Lake Store 的診斷記錄](../data-lake-store/data-lake-store-diagnostic-logs.md) |
| ExpressRoute | 無法使用結構描述。 |
| 事件中樞 |[Azure 事件中樞診斷記錄](../event-hubs/event-hubs-diagnostic-logs.md) |
| 金鑰保存庫 |[Azure 金鑰保存庫記錄](../key-vault/key-vault-logging.md) |
| 負載平衡器 |[Azure 負載平衡器的 Log Analytics](../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Logic Apps B2B 自訂追蹤結構描述](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| 網路安全性群組 |[網路安全性群組 (NSG) 的記錄檔分析](../virtual-network/virtual-network-nsg-manage-log.md) |
| [復原服務] | 無法使用結構描述。|
| 搜尋 |[啟用和使用搜尋流量分析](../search/search-traffic-analytics.md) |
| 伺服器管理 | 無法使用結構描述。 |
| 服務匯流排 |[Azure 服務匯流排診斷記錄](../service-bus-messaging/service-bus-diagnostic-logs.md) |
| 串流分析 |[作業診斷記錄](../stream-analytics/stream-analytics-job-diagnostic-logs.md) |

## <a name="supported-log-categories-per-resource-type"></a>每個資源類型支援的記錄檔類別
|資源類型|類別|類別顯示名稱|
|---|---|---|
|Microsoft.ApiManagement/service|GatewayLogs|ApiManagement 閘道的相關記錄檔|
|Microsoft.Automation/automationAccounts|JobLogs|作業記錄檔|
|Microsoft.Automation/automationAccounts|JobStreams|作業串流|
|Microsoft.Automation/automationAccounts|DscNodeStatus|Dsc 節點狀態|
|Microsoft.Batch/batchAccounts|ServiceLog|服務記錄檔|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|取得端點的計量，例如頻寬、輸出等資訊。|
|Microsoft.CustomerInsights/hubs|AuditEvents|AuditEvents|
|Microsoft.DataLakeAnalytics/accounts|稽核|稽核記錄檔|
|Microsoft.DataLakeAnalytics/accounts|要求|要求記錄檔|
|Microsoft.DataLakeStore/accounts|稽核|稽核記錄檔|
|Microsoft.DataLakeStore/accounts|要求|要求記錄檔|
|Microsoft.EventHub/namespaces|ArchiveLogs|封存記錄檔|
|Microsoft.EventHub/namespaces|OperationalLogs|作業記錄|
|Microsoft.EventHub/namespaces|AutoScaleLogs|自動調整規模記錄檔|
|Microsoft.KeyVault/vaults|AuditEvent|稽核記錄檔|
|Microsoft.Logic/workflows|WorkflowRuntime|工作流程執行階段診斷事件|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|整合帳戶追蹤事件|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|網路安全性群組事件|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|網路安全性群組規則計數器|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|負載平衡器警示事件|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|負載平衡器探查健全狀況狀態|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|應用程式閘道存取記錄檔|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|應用程式閘道效能記錄檔|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|應用程式閘道防火牆記錄檔|
|Microsoft.Network/expressRouteCircuits|GWMCountersTable|GWM 計數器的資料表|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Azure 備份報表資料|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Azure Site Recovery 作業|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Azure Site Recovery 事件|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Azure Site Recovery 複寫項目|
|Microsoft.Search/searchServices|OperationLogs|作業記錄|
|Microsoft.ServerManagement/nodes|RequestLogs|要求記錄檔|
|Microsoft.ServiceBus/namespaces|OperationalLogs|作業記錄|
|Microsoft.StreamAnalytics/streamingjobs|執行|執行|
|Microsoft.StreamAnalytics/streamingjobs|編寫|編寫|

## <a name="next-steps"></a>後續步驟

* [將診斷記錄串流至**事件中樞**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [使用 Azure 監視器 REST API 變更診斷設定](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [使用 Log Analytics 分析來自 Azure 儲存體的記錄](../log-analytics/log-analytics-azure-storage.md)

