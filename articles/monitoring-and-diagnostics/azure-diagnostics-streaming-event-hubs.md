---
title: "aaaStreaming data i Azure-diagnostik i hello varm sökväg med Händelsehubbar | Microsoft Docs"
description: "Konfigurera Azure-diagnostik med Händelsehubbar avslutas tooend, inklusive anvisningar för vanliga scenarier."
services: event-hubs
documentationcenter: na
author: rboucher
manager: carmonm
editor: 
ms.assetid: edeebaac-1c47-4b43-9687-f28e7e1e446a
ms.service: monitoring-and-diagnostics
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: robb
ms.openlocfilehash: a2528ddd0688d1c23a8631e769ca016dd79e4159
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="streaming-azure-diagnostics-data-in-hello-hot-path-by-using-event-hubs"></a>Strömmande data i Azure-diagnostik i hello varm sökväg med hjälp av Händelsehubbar
Azure-diagnostik och ger flexibla sätt toocollect mått och loggar från cloud services virtuella maskiner (VMs) och överför resultat tooAzure lagring. Från och med tidsintervall för hello mars 2016 (SDK 2.9), kan du skicka diagnostik toocustom datakällor och dataöverföring varm sökväg i sekunder med hjälp av [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).

Datatyper som stöds är:

* ETW-händelser (Event Tracing for Windows)
* Prestandaräknare
* Windows-händelseloggar
* Programloggar
* Azure Diagnostics infrastruktur loggar

Den här artikeln visar hur tooconfigure Azure Diagnostics med Händelsehubbar från avslutas tooend. Vägledning ges också för hello följande vanliga scenarier:

* Hur toocustomize hello loggar och mått som skickas tooEvent hubbar
* Hur toochange konfigurationer i varje miljö
* Hur tooview Händelsehubbar strömma data
* Hur tootroubleshoot hello anslutning  

## <a name="prerequisites"></a>Krav
Event Hubs receieving data från Azure-diagnostik stöds i molntjänster, virtuella datorer, virtuella datorer och Service Fabric från och med hello Azure SDK 2.9 och hello motsvarande Azure-verktyg för Visual Studio.

* Azure Diagnostics tillägget 1.6 ([Azure SDK för .NET 2.9 eller senare](https://azure.microsoft.com/downloads/) riktar sig mot detta som standard)
* [Visual Studio 2013 eller senare](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* Befintliga konfigurationer av Azure-diagnostik i ett program med hjälp av en *.wadcfgx* fil och en av hello följande metoder:
  * Visual Studio: [konfigurera diagnostik för Azure-molntjänster och virtuella datorer](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
  * Windows PowerShell: [aktivera diagnostik i Azure Cloud Services med hjälp av PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md)
* Hubbar händelsenamnrymden etablerats per hello artikel [Kom igång med Händelsehubbar](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

## <a name="connect-azure-diagnostics-tooevent-hubs-sink"></a>Anslut Azure Diagnostics tooEvent hubbar sink
Som standard skickar Azure-diagnostik alltid loggar och mått tooan Azure Storage-konto. Ett program kan även skicka data tooEvent Hubs genom att lägga till en ny **egenskaperna** avsnitt under hello **PublicConfig** / **WadCfg** element av hello *.wadcfgx* fil. I Visual Studio hello *.wadcfgx* lagras i hello följande sökväg: **Molntjänstprojekt** > **roller** > **() RoleName)** > **diagnostics.wadcfgx** fil.

```xml
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```
```JSON
"SinksConfig": {
    "Sink": [
        {
            "name": "HotPath",
            "EventHub": {
                "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
                "SharedAccessKeyName": "SendRule"
            }
        }
    ]
}
```

I det här exemplet hello händelsehubb URL anges toohello fullständigt kvalificerade namnområdet för hello händelsehubb: Händelsehubbar namnområde + ”/” + händelsehubbens namn.  

Hej händelsehubb URL visas i hello [Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885) på hello Händelsehubbar instrumentpanel.  

Hej **Sink** namn kan ställas in tooany giltig sträng så länge hello samma värde används konsekvent i hela hello config-fil.

> [!NOTE]
> Det kan finnas ytterligare sänkor som *applicationInsights* konfigurerats i det här avsnittet. Azure Diagnostics tillåter en eller flera egenskaperna toobe definieras om varje mottagare deklareras även i hello **PrivateConfig** avsnitt.  
>
>

Hej Händelsehubbar sink måste också deklareras och definieras i hello **PrivateConfig** avsnitt i hello *.wadcfgx* config-fil.

```XML
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="{account name}" key="{account key}" endpoint="{optional storage endpoint}" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
</PrivateConfig>
```
```JSON
{
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{optional storage endpoint}",
    "EventHub": {
        "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    }
}
```

Hej `SharedAccessKeyName` värdet måste matcha en delad signatur åtkomst (SAS) nyckel och en princip som har definierats i hello **Händelsehubbar** namnområde. Bläddra toohello Händelsehubbar instrumentpanelen i hello [Azure-portalen](https://manage.windowsazure.com), klicka på hello **konfigurera** fliken och skapa en namngiven princip (till exempel ”SendRule”) som har *skicka* behörigheter. Hej **StorageAccount** också har deklarerats i **PrivateConfig**. Det behövs ingen här toochange värden om de fungerar. I det här exemplet lämnar vi hello värden tomt, vilket är ett tecken som att den underordnade resursen kommer att ange hello värden. Till exempel hello *ServiceConfiguration.Cloud.cscfg* miljö konfigurationsfil anger hello miljö som är lämpliga namn och nycklar.  

> [!WARNING]
> hello Event Hubs SAS-nyckeln lagras i klartext i hello *.wadcfgx* fil. Ofta den här nyckeln är markerad i toosource för källkodskontroll eller är tillgänglig som en tillgång i build-servern så bör du skydda den efter behov. Vi rekommenderar att du använder SAS-nyckeln här med *Skicka bara* behörigheter så att en obehörig användare inte kan skriva toohello händelsehubb, men lyssna tooit eller hantera den.
>
>

## <a name="configure-azure-diagnostics-toosend-logs-and-metrics-tooevent-hubs"></a>Konfigurera Azure-diagnostik toosend loggar och mått tooEvent hubbar
Som diskuterats tidigare alla standard- och anpassade diagnostikdata, det vill säga skickas loggar, mått och automatiskt tooAzure lagring i hello konfigurerade intervallen. Du kan ange vilken nod som rot eller löv i hello hierarkin toobe skickas toohello händelsehubb med Händelsehubbar och eventuella ytterligare mottagare. Detta inkluderar ETW-händelser, prestandaräknare, Windows-händelseloggar och programloggarna.   

Det är viktigt tooconsider hur många datapunkter ska verkligen vara överförs tooEvent Hubs. Vanligtvis dataöverföring utvecklare låg latens hot sökvägen som används och tolkas snabbt. System som övervakar aviseringar eller Autoskala regler är exempel. En utvecklare kan också konfigurera en alternativ analysis-butik eller Sök store – till exempel Azure Stream Analytics Elasticsearch, ett anpassat övervakningssystem eller en favorit övervakningssystemet från andra.

hello följande är några exempelkonfigurationer.

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "sinks": "HotPath",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        }
    ]
}
```

I hello ovan exempel hello sink är tillämpade toohello överordnade **PerformanceCounters** nod i hello-hierarkin, vilket innebär att alla underordnade **prestandaräknarna** skickas tooEvent Hubs.  

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Rejected",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Queued",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        }
    ]
}
```

I föregående exempel hello hello sink är tillämpade tooonly tre räknare: **begäranden i kö**, **begäranden avvisas**, och **% processortid**.  

hello följande exempel visas hur en utvecklare kan begränsa hello mängden data som skickats toobe hello kritiska mått som används för den här tjänstens hälsa.  

```XML
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```
```JSON
"Logs": {
    "scheduledTransferPeriod": "PT1M",
    "scheduledTransferLogLevelFilter": "Error",
    "sinks": "HotPath"
}
```

I det här exemplet hello sink är tillämpade toologs och filtrerad endast tooerror nivå trace.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Distribuera och uppdatera en molntjänster program och diagnostik config
Visual Studio ger hello enklaste sökvägen toodeploy hello program och Händelsehubbar sink-konfiguration. tooview och redigera hello-fil, öppna hello *.wadcfgx* filen i Visual Studio, redigera och spara den. hello sökvägen är **Molntjänstprojekt** > **roller** > **(RoleName)** > **diagnostics.wadcfgx**.  

Nu är alla och distributionen uppdatera åtgärder i Visual Studio, Visual Studio Team System och alla kommandon eller skript som är baserade på MSBuild och använda hello **/t: publicera** mål inkluderar hello *.wadcfgx*  hello paketering pågår. Dessutom distribuera distributioner och uppdateringar hello filen tooAzure med hello Azure Diagnostics agent filtillägg på dina virtuella datorer.

Aktiviteten i hello instrumentpanelen hello händelsehubbens visas direkt när du har distribuerat hello program och konfiguration av Azure-diagnostik. Detta anger att du är klar toomove på tooviewing hello hot sökväg data i hello lyssnare klient- eller verktyg som helst.  

I följande bild hello, visar hello Händelsehubbar instrumentpanelen felfria skickar diagnostik data toohello event hub börjar stund efter kl. Det är då hello programmet distribuerades med en uppdaterad *.wadcfgx* filen och hello mottagare har konfigurerats korrekt.

![][0]  

> [!NOTE]
> När du gör uppdateringar toohello Azure Diagnostics config-fil (.wadcfgx), bör du push hello uppdateringar toohello hela programmet samt hello konfiguration med hjälp av Visual Studio-publicering eller en Windows PowerShell-skript.  
>
>

## <a name="view-hot-path-data"></a>Visa hot sökväg data
Som tidigare diskuterats, finns det många användningsområden för lyssnande tooand Händelsehubbar data bearbetas.

En enkel metod är toocreate en test små konsolen programmet toolisten toohello händelsehubb och skriva ut hello utdataströmmen. Du kan placera hello följande kod, som beskrivs i detalj i [Kom igång med Händelsehubbar](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)), i ett konsolprogram.  

Observera att hello konsolprogram måste innehålla hello [händelse Processor värden NuGet-paketet](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).  

Kom ihåg tooreplace hello värden hakparenteser i hello **Main** funktion med värden för dina resurser.   

```csharp
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key toostop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sinks"></a>Felsöka Händelsehubbar handfat
* Hej händelsehubb visar inte inkommande eller utgående händelseaktivitet som förväntat.

    Kontrollera att din händelsehubb har etablerats. Alla anslutningsinformation i hello **PrivateConfig** avsnitt i *.wadcfgx* måste matcha hello värdena för din resurs som visas i hello-portalen. Se till att du har en SAS princip definierad (”SendRule” hello exempel) i hello-portalen och som *skicka* behörigheten.  
* När en uppdatering visar inte längre inkommande eller utgående händelseaktivitet hello händelsehubb.

    Kontrollera först att namnet är rätt som tidigare förklarats hello NAV- och händelseinformation. Ibland hello **PrivateConfig** återställs i en distributionsuppdatering. hello rekommenderas korrigering är toomake alla ändringar för*.wadcfgx* i hello projektet och sedan push-installera en uppdatering för hela programmet. Om det inte är möjligt, kontrollera att hello diagnostik update skickar en fullständig **PrivateConfig** som innehåller hello SAS-nyckel.  
* Jag har försökt hello förslag och hello händelsehubb fortfarande fungerar inte.

    Titta i hello Azure Storage-tabell som innehåller loggarna och fel för Azure-diagnostik själva: **WADDiagnosticInfrastructureLogsTable**. Ett alternativ är toouse ett verktyg som [Azure Lagringsutforskaren](http://www.storageexplorer.com) tooconnect toothis storage-konto, visa den här tabellen och Lägg till en fråga för tidsstämplar i hello senaste 24 timmarna. Du kan använda hello verktyget tooexport en CSV-fil och öppna den i ett program, till exempel Microsoft Excel. Excel gör det enkelt toosearch för kort strängar som **EventHubs**, toosee vilka fel rapporteras.  

## <a name="next-steps"></a>Nästa steg
• [Mer information om Händelsehubbar](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>Bilaga: Slutföra exempel på Azure-diagnostik en konfigurationsfil (.wadcfgx)
```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount>ACCOUNT_NAME</StorageAccount>
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="{account name}" key="{account key}" endpoint="{storage endpoint}" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

hello kompletterande *ServiceConfiguration.Cloud.cscfg* för det här exemplet ser ut som följande hello.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

Motsvarande Json-baserade inställningar för virtuella datorer är följande:
```JSON
"settings": {
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 4096,
            "sinks": "applicationInsights.errors",
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "Directories": {
                "scheduledTransferPeriod": "PT1M",
                "IISLogs": {
                    "containerName": "wad-iis-logfiles"
                },
                "FailedRequestLogs": {
                    "containerName": "wad-failedrequestlogs"
                }
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "sinks": "HotPath",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Memory\\Available MBytes",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\Bytes Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Errors Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Queued",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Rejected",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                        "sampleRate": "PT3M"
                    }
                ]
            },
            "WindowsEventLog": {
                "scheduledTransferPeriod": "PT1M",
                "DataSource": [
                    {
                        "name": "Application!*"
                    }
                ]
            },
            "Logs": {
                "scheduledTransferPeriod": "PT1M",
                "scheduledTransferLogLevelFilter": "Error",
                "sinks": "HotPath"
            }
        },
        "SinksConfig": {
            "Sink": [
                {
                    "name": "HotPath",
                    "EventHub": {
                        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
                        "SharedAccessKeyName": "SendRule"
                    }
                },
                {
                    "name": "applicationInsights",
                    "ApplicationInsights": "",
                    "Channels": {
                        "Channel": [
                            {
                                "logLevel": "Error",
                                "name": "errors"
                            }
                        ]
                    }
                }
            ]
        }
    },
    "StorageAccount": "{account name}"
}


"protectedSettings": {
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{storage endpoint}",
    "EventHub": {
        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "YOUR_KEY_HERE"
    }
}
```

## <a name="next-steps"></a>Nästa steg
Mer information om Händelsehubbar genom att besöka hello följande länkar:

* [Event Hubs-översikt](../event-hubs/event-hubs-what-is-event-hubs.md)
* [Skapa en Event Hub](../event-hubs/event-hubs-create.md)
* [Vanliga frågor och svar om Event Hubs](../event-hubs/event-hubs-faq.md)

<!-- Images. -->
[0]: ../event-hubs/media/event-hubs-streaming-azure-diags-data/dashboard.png
