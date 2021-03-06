---
title: "排定的事件與 Azure 的中繼資料服務 | Microsoft Docs"
description: "在具影響力的事件發生之前，對虛擬機器上的這類事件做出回應。"
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 28d8e1f2-8e61-4fbe-bfe8-80a68443baba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/10/2016
ms.author: zivr
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: 627aa117ded0aaa519052d4ea1a1995ba2e363ee
ms.contentlocale: zh-tw
ms.lasthandoff: 05/17/2017


---
# <a name="azure-metadata-service---scheduled-events-preview"></a>Azure 中繼資料服務 - 排定的事件 (預覽)

> [!NOTE] 
> 若您同意使用規定即可取得預覽。 如需詳細資訊，請參閱 [Microsoft Azure 預覽版增補使用條款。] (https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/)
>

排定的事件是 Azure 中繼資料服務下的其中一個子服務，會呈現即將發生的事件 (例如，重新開機) 相關資訊，您的應用程式才能做好準備以及限制中斷。 它適用於所有 Azure 虛擬機器類型 (包括 PaaS 和 IaaS)。 排定的事件讓虛擬機器有時間執行預防性工作並將事件的影響降至最低。 


## <a name="introduction---why-scheduled-events"></a>簡介 - 為何使用排定的事件？

有了排定的事件，您可以採取措施限制對您服務的影響。 多重執行個體的工作負載 (其使用複寫技術來維護狀態) 可能易受發生於多個執行個體的經常中斷所影響。 例如中斷可能會導致耗費資源的工作 (例如，重建索引) 或甚至是複本遺失。 在其他許多情況下，使用正常關機順序可改善整體服務可用性。 例如，完成 (或取消) 進行中的交易、將其他工作重新指派給叢集中的其他 VM (手動容錯移轉)，即可從負載平衡器集區中移除虛擬機器。 在某些情況下，通知系統管理員有關即將發生的事件，或甚至只是記錄這類事件都有助於改善雲端裝載的應用程式服務性。
Azure 中繼資料服務會在下列使用案例中呈現排定的事件︰
-    平台起始的維護 (例如，主機作業系統推出)
-    使用者起始的呼叫 (例如，使用者重新啟動或重新部署 VM)


## <a name="scheduled-events---the-basics"></a>排定的事件 - 基本概念  

Azure 中繼資料服務會公開有關使用 VM 內的 REST 端點執行虛擬機器的資訊。 此資訊可透過不可路由傳送的 IP 取得，因此會會在 VM 之外公開。

### <a name="scope"></a>Scope
排定的事件會對雲端服務中的所有虛擬機器或可用性設定組中的所有虛擬機器呈現。 因此，您應該檢查事件中的 [資源] 欄位以找出哪些 VM 即將受到影響。 

### <a name="discover-the-endpoint"></a>探索端點
在虛擬機器建立在虛擬網路 (VNet) 之中的情況下，可從非路由傳送的 IP：169.254.169.254 提供中繼資料服務。否則雲端服務和傳統 VM 在預設的情況下，需要額外的邏輯來探索要使用的端點。 請參閱此範例以了解如何 [探索主機端點] (https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm)

### <a name="versioning"></a>版本控制 
執行個體中繼資料服務已建立版本。 版本是必要項目，且目前版本為 2017-03-01

> [!NOTE] 
> 先前排定事件的預覽版支援作為 API 版本的 {latest}。 此格式將不再受到支援且之後會遭到取代。
>


### <a name="using-headers"></a>使用標頭
當您查詢中繼資料服務時，您必須提供下列標頭「中繼資料︰true」。 

### <a name="enable-scheduled-events"></a>啟用排定的事件
第一次呼叫排定的事件時，Azure 會在您的虛擬機器上以隱含方式啟用此功能。 因此，您可能會在第一次呼叫中遇到長達兩分鐘的延遲回應。

### <a name="testing-your-logic-with-user-initiated-operations"></a>透過使用者起始的作業測試您的邏輯
若要測試您的邏輯，您可以使用 Azure 入口網站、API、CLI 或 PowerShell 來起始會導致排定事件的作業。 重新啟動虛擬機器會導致事件類型相當於重新開機的排定事件。 重新部署虛擬機器會導致事件類型相當於重新部署的排定事件。
在這兩種情況下，使用者起始的作業會花費較長時間來完成，因為排定事件需要更多時間讓應用程式順利關閉。 

## <a name="using-the-api"></a>使用 API

### <a name="query-for-events"></a>查詢事件
您只要進行下列呼叫，即可查詢排定的事件：

    curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01


回應包含排定的事件陣列。 空白陣列表示目前沒有任何排定的事件。
在有排定事件的情況下，回應會包含事件陣列︰ 

    {
     "DocumentIncarnation":{IncarnationID},
     "Events":[
          {
                "EventId":{eventID},
                "EventType":"Reboot" | "Redeploy" | "Freeze",
                "ResourceType":"VirtualMachine",
                "Resources":[{resourceName}],
                "EventStatus":"Scheduled" | "Started",
                "NotBefore":{timeInUTC},              
         }
     ]
    }
    
### <a name="event-properties"></a>事件屬性
|屬性  |  說明 |
| - | - |
| EventId |事件的全域唯一識別碼。 <br><br> 範例： <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| EventType | 事件造成的影響。 <br><br> 值： <br><ul><li> <i>凍結</i>︰虛擬機器已排定會暫停幾秒鐘。 這不會影響記憶體、開啟的檔案或網路連線。 <li> <i>重新開機</i>︰虛擬機器已排定要重新開機 (已抹除記憶體)。<li> <i>重新部署</i>︰虛擬機器已排定要移至另一個節點 (暫時磁碟會遺失)。 |
| ResourceType | 受事件影響的資源類型。 <br><br> 值： <ul><li>VirtualMachine|
| 資源| 受事件影響的資源清單。 <br><br> 範例： <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| 事件狀態 | 事件的狀態。 <br><br> 值： <ul><li><i>已排程︰</i>事件已排定在 <i>NotBefore</i> 屬性中指定的時間之後啟動。<li><i>已啟動</i>︰已啟動事件。</i>
| NotBefore| 之後可能會啟動事件的時間。 <br><br> 範例： <br><ul><li> 2016-09-19T18:29:47Z  |

### <a name="event-scheduling"></a>事件排程
系統會根據事件類型，為每個事件在未來安排最少的時間量。 此時間會反映於事件的 <i>NotBefore</i> 屬性。 

|EventType  | 最短時間通知 |
| - | - |
| 凍結| 15 分鐘 |
| 重新啟動 | 15 分鐘 |
| 重新部署 | 10 分鐘 |

### <a name="starting-an-event-expedite"></a>啟動事件 (加速)

得知即將發生的事件並完成您的正常關機邏輯之後，您可以進行 **POST** 呼叫來指示 Azure 加速移動 (可能的話) 
 

## <a name="powershell-sample"></a>PowerShell 範例 

下列範例會讀取中繼資料伺服器來找出已排定的事件，並核准這些事件。

```PowerShell
# How to get scheduled events 
function GetScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How to approve a scheduled event
function ApproveScheduledEvent($eventId, $docIncarnation, $uri)
{    
    # Create the Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests; "DocumentIncarnation" = $docIncarnation} 
    
    # Convert to JSON string
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with the following: `n" $approvalString

    # Post approval string to scheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

# Add logic relevant to your service here
function HandleScheduledEvents($scheduledEvents)
{

}

######### Sample Scheduled Events Interaction #########

# Set up the scheduled events uri for VNET enabled VM
$localHostIP = "169.254.169.254"
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2017-03-01' -f $localHostIP 


# Get the document
$scheduledEvents = GetScheduledEvents $scheduledEventURI


# Handle events however is best for your service
HandleScheduledEvents $scheduledEvents


# Approve events when ready (optional)
foreach($event in $scheduledEvents.Events)
{
    Write-Host "Current Event: `n" $event
    $entry = Read-Host "`nApprove event? Y/N"
    if($entry -eq "Y" -or $entry -eq "y")
    {
    ApproveScheduledEvent $event.EventId $scheduledEvents.DocumentIncarnation $scheduledEventURI 
    }
}
``` 


## <a name="c-sample"></a>C\# 範例 
下列範例是用來與中繼資料服務進行通訊的用戶端呈現 API
```csharp
   public class ScheduledEventsClient
    {
        private readonly string scheduledEventsEndpoint;
        private readonly string defaultIpAddress = "169.254.169.254"; 

        public ScheduledEventsClient()
        {
            scheduledEventsEndpoint = string.Format("http://{0}/metadata/scheduledevents?api-version=2017-03-01", defaultIpAddress);
        }
        /// Retrieve Scheduled Events 
        public string GetDocument()
        {
            Uri cloudControlUri = new Uri(scheduledEventsEndpoint);
            using (var webClient = new WebClient())
            {
                webClient.Headers.Add("Metadata", "true");
                return webClient.DownloadString(cloudControlUri);
            }   
        }

        /// Issues a post request to the scheduled events endpoint with the given json string
        public void PostResponse(string jsonPost)
        {
            using (var webClient = new WebClient())
            {
                webClient.Headers.Add("Content-Type", "application/json");
                webClient.UploadString(scheduledEventsEndpoint, jsonPost);
            }
        }
    }

```
使用下列資料結構可以剖析排定的事件 

```csharp
    public class ScheduledEventsDocument
    {
        public string DocumentIncarnation;
        public List<CloudControlEvent> Events { get; set; }
    }

    public class CloudControlEvent
    {
        public string EventId { get; set; }
        public string EventStatus { get; set; }
        public string EventType { get; set; }
        public string ResourceType { get; set; }
        public List<string> Resources { get; set; }
        public DateTime? NotBefore { get; set; }
    }

    public class ScheduledEventsApproval
    {
        public string DocumentIncarnation;
        public List<StartRequest> StartRequests = new List<StartRequest>();
    }

    public class StartRequest
    {
        [JsonProperty("EventId")]
        private string eventId;

        public StartRequest(string eventId)
        {
            this.eventId = eventId;
        }
    }

```

使用用戶端來擷取、處理及認可事件的範例程式︰   

```csharp
public class Program
    {
    static ScheduledEventsClient client;
    static void Main(string[] args)
    {
        while (true)
        {
            client = new ScheduledEventsClient();
            string json = client.GetDocument();
            ScheduledEventsDocument scheduledEventsDocument = JsonConvert.DeserializeObject<ScheduledEventsDocument>(json);

            HandleEvents(scheduledEventsDocument.Events);

            // Wait for user response
            Console.WriteLine("Press Enter to approve executing events\n");
            Console.ReadLine();

            // Approve events
            ScheduledEventsApproval scheduledEventsApprovalDocument = new ScheduledEventsApproval()
        {
            DocumentIncarnation = scheduledEventsDocument.DocumentIncarnation
        };
        
            foreach (CloudControlEvent ccevent in scheduledEventsDocument.Events)
            {
                scheduledEventsApprovalDocument.StartRequests.Add(new StartRequest(ccevent.EventId));
            }
            if (scheduledEventsApprovalDocument.StartRequests.Count > 0)
            {
                // Serialize using Newtonsoft.Json
                string approveEventsJsonDocument =
                    JsonConvert.SerializeObject(scheduledEventsApprovalDocument);

                Console.WriteLine($"Approving events with json: {approveEventsJsonDocument}\n");
                client.PostResponse(approveEventsJsonDocument);
            }

            Console.WriteLine("Complete. Press enter to repeat\n\n");
            Console.ReadLine();
            Console.Clear();
        }
    }

    private static void HandleEvents(List<CloudControlEvent> events)
    {
        // Add logic for handling events here
    }
}

```

## <a name="python-sample"></a>Python 範例 

```python


#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url="http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01"
headers="{Metadata:true}"
this_host=socket.gethostname()

def get_scheduled_events():
   req=urllib2.Request(metadata_url)
   req.add_header('Metadata','true')
   resp=urllib2.urlopen(req)
   data=json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid=evt['EventId']
        status=evt['EventStatus']
        resources=evt['Resources'][0]
        eventype=evt['EventType']
        restype=evt['ResourceType']
        notbefore=evt['NotBefore'].replace(" ","_")
        if this_host in evt['Resources'][0]:
            print "+ Scheduled Event. This host is scheduled for " + eventype + " not before " + notbefore
            print "++ Add you logic here"

def main():
   data=get_scheduled_events()
   handle_scheduled_events(data)
   

if __name__ == '__main__':
  main()
  sys.exit(0)


```
## <a name="next-steps"></a>後續步驟 
[Azure 中虛擬機器預定進行的維修](linux/planned-maintenance.md)
[執行個體中繼資料服務](virtual-machines-instancemetadataservice-overview.md)

