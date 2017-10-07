---
title: "aaaScheduled händelser för virtuella Windows-datorer i Azure | Microsoft Docs"
description: "Schemalagda händelser med hjälp av hello Azure Metadata-tjänsten för på Windows-datorer."
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
ms.date: 08/14/2017
ms.author: zivr
ms.openlocfilehash: c9f5f332a5d77e8d54d1ae8bdaadafc1a14f3b77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-metadata-service-scheduled-events-preview-for-windows-vms"></a>Metadata Azure: Schemalagda händelser (förhandsversion) för virtuella Windows-datorer

> [!NOTE] 
> Förhandsgranskningar görs tillgängliga tooyou hello förutsatt att du godkänner toohello villkor för användning. Mer information finns i [de kompletterande villkoren för användning av Microsoft Azure-förhandsversioner](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

Schemalagda händelser är en av hello subservices under hello Azure Metadata Service. Ansvarar för att visa information om kommande händelser (till exempel omstart) så att programmet kan förbereda för dem och begränsa avbrott. Den är tillgänglig för alla typer av Azure virtuella datorer inklusive PaaS och IaaS. Schemalagda händelser ger virtuella tid tooperform förebyggande aktiviteterna toominimize hello effekten av en händelse. 

Schemalagda händelser är tillgänglig för både Linux och Windows-datorer. Information om schemalagda händelser på Linux finns [schemalagda händelser för virtuella Linux-datorer](../windows/scheduled-events.md).

## <a name="why-scheduled-events"></a>Varför schemalagda händelser?

Schemalagda händelser, kan du vidta åtgärder för toolimit hello effekten av plattform intiated underhåll eller användare utför åtgärder på din tjänst. 

Flera instanser arbetsbelastningar som använder replikeringstillståndet tekniker toomaintain kanske sårbara toooutages händer i flera instanser. Exempel driftsavbrott kan resultera i dyra aktiviteter (till exempel återskapande index) eller även en replik går förlorade. 

I många andra fall hello övergripande tjänsttillgängligheten kan förbättras genom att utföra en korrekt avslutningssekvens som Slutför (eller annullerar) pågående transaktioner, tilldela uppgifter tooother virtuella datorer i hello kluster (manuell redundans) eller ta bort hello Den virtuella datorn från poolen network load belastningsutjämnaren. 

Finns det fall där tala med administratören om en kommande händelse eller logga sådan händelse att förbättra hello brukbarheten av program som finns i molnet hello.

Azure Metadata Service hämtar schemalagda händelser i hello följande användningsfall:
-   Plattform som initierade Underhåll (till exempel värd-OS-installationen)
-   Användarinitierad anrop (till exempel användaren startar om eller återdistribuerar en virtuell dator)


## <a name="hello-basics"></a>hello-grunderna  

Metadata i Azure-tjänsten visar information om att köra virtuella datorer med en REST-slutpunkt som är tillgänglig från hello VM. hello-information är tillgänglig via en icke-dirigerbara IP-adress så att inte exponeras utanför hello VM.

### <a name="scope"></a>Omfång
Schemalagda händelser är på ytan tooall virtuella datorer i en molnbaserad tjänst eller tooall virtuella datorer i en Tillgänglighetsuppsättning. Därför bör du kontrollera hello `Resources` i hello händelse tooidentify som virtuella datorer ska toobe påverkas. 

### <a name="discovering-hello-endpoint"></a>Identifiera hello slutpunkt
I hello fall där en virtuell dator skapas ett virtuellt nätverk (VNet) hello metadata-tjänsten är tillgänglig från en statisk icke-dirigerbara IP `169.254.169.254`.
Om hello virtuell dator inte skapas inom ett virtuellt nätverk hello fall för molntjänster och klassiska virtuella datorer, är obligatoriska toodiscover hello endpoint toouse ytterligare logik. Se hur för toothis exempel toolearn[identifiera hello värden slutpunkt](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).

### <a name="versioning"></a>Versionshantering 
hello instans Metadata Service är en ny version. Versioner är obligatoriska och hello aktuella versionen är `2017-03-01`.

> [!NOTE] 
> Tidigare förhandsvisningarna av schemalagda händelser stöds {senaste} som hello api-versionen. Det här formatet stöds inte längre och kommer att inaktualiseras i hello framtida.

### <a name="using-headers"></a>Med hjälp av rubriker
När du frågar hello Metadata Service måste du ange hello huvud `Metadata: true` tooensure hello begäran omdirigerades inte oavsiktligt.

### <a name="enabling-scheduled-events"></a>Aktivera schemalagd händelser
hello aktiverar första gången du skapar en begäran om schemalagda händelser Azure implicit hello funktion på den virtuella datorn. Därför bör du förväntar dig fördröjd svar i din första anropet av in tootwo i minuter.

### <a name="user-initiated-maintenance"></a>Användarinitierad Underhåll
Användaren initierade underhålla virtuella datorer via hello Azure-portalen API, CLI eller PowerShell resulterar i en schemalagd händelse. Detta kan tootest hello Underhåll förberedelse logik i ditt program och dina program tooprepare för användarinitierad underhåll.

Starta om en virtuell dator schemalägger en händelse med typen `Reboot`. Omdistribuera en virtuell dator schemalägger en händelse med typen `Redeploy`.

> [!NOTE] 
> För närvarande kan högst 10 användarinitierad underhållsåtgärder samtidigt schemaläggas. Den här gränsen mjukas upp före schemalagda händelser allmän tillgänglighet.

> [!NOTE] 
> Användarinitierad Underhåll ledde schemalagda händelser kan för närvarande inte konfigureras. Konfigurationsmöjligheter är planerad för framtida versioner.

## <a name="using-hello-api"></a>Med hello-API

### <a name="query-for-events"></a>Fråga efter händelser
Du kan fråga efter schemalagda händelser genom att göra hello följande anropa:

```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

Svaret innehåller en matris med schemalagda händelser. En tom matris innebär att det inte finns för närvarande inga händelser som är schemalagda.
I hello fall där det finns schemalagda händelser, hello svaret innehåller en matris med händelser: 
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},              
        }
    ]
}
```

### <a name="event-properties"></a>Egenskaper för händelse
|Egenskap  |  Beskrivning |
| - | - |
| Händelse-ID | Globalt unik identifierare för den här händelsen. <br><br> Exempel: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| Händelsetyp | Inverkan som gör att den här händelsen. <br><br> Värden: <br><ul><li> `Freeze`: hello virtuella datorn är schemalagt toopause för några sekunder. hello CPU har pausats, men det finns ingen inverkan på minnet, öppna filer eller nätverksanslutningar. <li>`Reboot`: hello virtuella datorn är schemalagt för omstart (icke-beständig minne är förlorade). <li>`Redeploy`: hello virtuella datorn är schemalagt toomove tooanother nod (tillfälliga diskar går förlorade). |
| ResourceType | Typ av resurs som påverkar den här händelsen. <br><br> Värden: <ul><li>`VirtualMachine`|
| Resurser| Lista över resurser som påverkar den här händelsen. Detta är garanterat toocontain datorer från högst ett [Uppdateringsdomän](manage-availability.md), men får inte innehålla alla datorer i hello UD. <br><br> Exempel: <br><ul><li> [”FrontEnd_IN_0”, ”BackEnd_IN_0”] |
| Händelsestatus | Status för den här händelsen. <br><br> Värden: <ul><li>`Scheduled`: Den här händelsen är schemalagda toostart efter hello tid som anges i hello `NotBefore` egenskapen.<li>`Started`: Den här händelsen har startats.</ul> Inte `Completed` eller liknande status tillhandahålls någonsin; hello händelsen inte längre kommer att returneras när hello händelse har slutförts.
| Inte före| Tid som den här händelsen kan starta. <br><br> Exempel: <br><ul><li> 2016-09-19T18:29:47Z  |

### <a name="event-scheduling"></a>Schemaläggning av händelse
Varje händelse är schemalagd kortaste tid i hello framtida baserat på händelsetyp. Nu visas i en händelse `NotBefore` egenskapen. 

|Händelsetyp  | Minsta meddelande |
| - | - |
| Lås| 15 minuter |
| Starta om | 15 minuter |
| Omdistribuera | 10 minuter |

### <a name="starting-an-event"></a>Starta en händelse 

När du har lärt dig i en kommande händelse och slutföra din logik för att korrekt avslutning, kan du godkänna hello utestående händelsen genom att göra en `POST` anropa toohello metadatatjänsten med hello `EventId`. Detta anger att den korta minsta hello-meddelande tooAzure (när det är möjligt). 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> Bekräfta en händelse kan hello händelse tooproceed för alla `Resources` i hello-händelse, inte bara hello virtuella om hello-händelse. Därför du tooelect en ledande toocoordinate hello bekräftelse, vilket kan vara så enkelt som hello första dator i hello `Resources` fältet.


## <a name="powershell-sample"></a>PowerShell-exempel 

hello följande exempelfrågor hello metadatatjänsten för schemalagda händelser och godkänner alla utestående händelser.

```PowerShell
# How tooget scheduled events 
function GetScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How tooapprove a scheduled event
function ApproveScheduledEvent($eventId, $docIncarnation, $uri)
{    
    # Create hello Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests; "DocumentIncarnation" = $docIncarnation} 
    
    # Convert tooJSON string
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with hello following: `n" $approvalString

    # Post approval string tooscheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

function HandleScheduledEvents($scheduledEvents)
{
    # Add logic for handling events here
}

######### Sample Scheduled Events Interaction #########

# Set up hello scheduled events URI for a VNET-enabled VM
$localHostIP = "169.254.169.254"
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2017-03-01' -f $localHostIP 

# Get events
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


## <a name="c-sample"></a>C\# exempel 

hello är följande exempel en enkel klient som kommunicerar med hello metadata-tjänsten.

```csharp
public class ScheduledEventsClient
{
    private readonly string scheduledEventsEndpoint;
    private readonly string defaultIpAddress = "169.254.169.254"; 

    // Set up hello scheduled events URI for a VNET-enabled VM
    public ScheduledEventsClient()
    {
        scheduledEventsEndpoint = string.Format("http://{0}/metadata/scheduledevents?api-version=2017-03-01", defaultIpAddress);
    }

    // Get events
    public string GetScheduledEvents()
    {
        Uri cloudControlUri = new Uri(scheduledEventsEndpoint);
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Metadata", "true");
            return webClient.DownloadString(cloudControlUri);
        }   
    }

    // Approve events
    public void ApproveScheduledEvents(string jsonPost)
    {
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Content-Type", "application/json");
            webClient.UploadString(scheduledEventsEndpoint, jsonPost);
        }
    }
}
```

Schemalagda händelser kan representeras med hjälp av följande datastrukturer hello:

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

hello följande exempelfrågor hello metadatatjänsten för schemalagda händelser och godkänner alla utestående händelser.

```csharp
public class Program
{
    static ScheduledEventsClient client;

    static void Main(string[] args)
    {
        client = new ScheduledEventsClient();

        while (true)
        {
            string json = client.GetDocument();
            ScheduledEventsDocument scheduledEventsDocument = JsonConvert.DeserializeObject<ScheduledEventsDocument>(json);

            HandleEvents(scheduledEventsDocument.Events);

            // Wait for user response
            Console.WriteLine("Press Enter tooapprove executing events\n");
            Console.ReadLine();

            // Approve events
            ScheduledEventsApproval scheduledEventsApprovalDocument = new ScheduledEventsApproval()
            {
                DocumentIncarnation = scheduledEventsDocument.DocumentIncarnation
            };
        
            foreach (CloudControlEvent event in scheduledEventsDocument.Events)
            {
                scheduledEventsApprovalDocument.StartRequests.Add(new StartRequest(event.EventId));
            }

            if (scheduledEventsApprovalDocument.StartRequests.Count > 0)
            {
                // Serialize using Newtonsoft.Json
                string approveEventsJsonDocument =
                    JsonConvert.SerializeObject(scheduledEventsApprovalDocument);

                Console.WriteLine($"Approving events with json: {approveEventsJsonDocument}\n");
                client.ApproveScheduledEvents(approveEventsJsonDocument);
            }

            Console.WriteLine("Complete. Press enter toorepeat\n\n");
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

## <a name="python-sample"></a>Python-exempel 

hello följande exempelfrågor hello metadatatjänsten för schemalagda händelser och godkänner alla utestående händelser.

```python
#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01"
headers = "{Metadata:true}"
this_host = socket.gethostname()

def get_scheduled_events():
   req = urllib2.Request(metadata_url)
   req.add_header('Metadata', 'true')
   resp = urllib2.urlopen(req)
   data = json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid = evt['EventId']
        status = evt['EventStatus']
        resources = evt['Resources']
        eventtype = evt['EventType']
        resourcetype = evt['ResourceType']
        notbefore = evt['NotBefore'].replace(" ","_")
        if this_host in resources:
            print "+ Scheduled Event. This host is scheduled for " + eventype + " not before " + notbefore
            # Add logic for handling events here

def main():
   data = get_scheduled_events()
   handle_scheduled_events(data)
   
if __name__ == '__main__':
  main()
  sys.exit(0)
```

## <a name="next-steps"></a>Nästa steg 

- Läs mer om hello API: er som är tillgängliga i hello [instans Metadata tjänsten](instance-metadata-service.md).
- Lär dig mer om [planerat underhåll för Windows-datorer i Azure](planned-maintenance.md).

