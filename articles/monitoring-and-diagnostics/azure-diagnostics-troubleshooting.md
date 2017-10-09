---
title: aaaTroubleshooting Azure Diagnostics | Microsoft Docs
description: "Felsökning av problem när du använder Azure-diagnostik i Azure Virtual Machines, Service Fabric och Cloud Services."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 66469bce-d457-4d1e-b550-a08d2be4d28c
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/12/2017
ms.author: robb
ms.openlocfilehash: daaf9fa4c40982eb9ba04030d7e8ea1ad9fe085b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-troubleshooting"></a>Azure Diagnostics felsökning
Felsökning av information relevanta toousing Azure-diagnostik. Läs mer om Azure diagnostics [översikt över Azure Diagnostics](azure-diagnostics.md).

## <a name="logical-components"></a>Logiska komponenter
**Starta diagnostik-plugin-programmet (DiagnosticsPluginLauncher.exe)**: startar hello Azure Diagnostics tillägg. Agerar som hello post rapporttjänstplatsen.

**Diagnostik-plugin-programmet (DiagnosticsPlugin.exe)**: Main process som startas av hello programstart ovan och konfigurerar hello Monitoring Agent, startar den och hanterar dess livslängd. 

**Monitoring Agent (MonAgent\*.exe processer)**: dessa processer hello bulk hello arbete, d.v.s. övervakning, samling och överföring av hello diagnostikdata.  

## <a name="logartifact-paths"></a>Log-artefakt sökvägar
Här följer hello sökvägar toosome viktiga loggar och artefakter. Vi hålla hänvisar toothese i hela hello resten av dokumentet hello:
### <a name="cloud-services"></a>Molntjänster
| Artefakt | Sökväg |
| --- | --- |
| **Azure Diagnostics-konfigurationsfil** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<version > \Config.txt |
| **Loggfiler** | C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<version > \ |
| **Lokal lagring för diagnostikdata** | C:\Resources\Directory\<CloudServiceDeploymentID >.\< RoleName >. DiagnosticStore\WAD0107\Tables |
| **Monitoring Agent-konfigurationsfil** | C:\Resources\Directory\<CloudServiceDeploymentID >.\< RoleName >. DiagnosticStore\WAD0107\Configuration\MaConfig.xml |
| **Azure diagnostics extension-paketet** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<version > |
| **Verktyget loggsökvägen-samling** | %SystemDrive%\Packages\GuestAgent\ |
| **MonAgentHost loggfil** | C:\Resources\Directory\<CloudServiceDeploymentID >.\< RoleName >. DiagnosticStore\WAD0107\Configuration\MonAgentHost. < seq_num > .log |

### <a name="virtual-machines"></a>Virtuella datorer
| Artefakt | Sökväg |
| --- | --- |
| **Azure Diagnostics-konfigurationsfil** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<version > \RuntimeSettings |
| **Loggfiler** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<version > \Logs\ |
| **Lokal lagring för diagnostikdata** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion > \WAD0107\Tables |
| **Monitoring Agent-konfigurationsfil** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion > \WAD0107\Configuration\MaConfig.xml |
| **Statusfil** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<version > \Status |
| **Azure diagnostics extension-paketet** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion >|
| **Verktyget loggsökvägen-samling** | C:\WindowsAzure\Packages |
| **MonAgentHost loggfil** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion > \WAD0107\Configuration\MonAgentHost. < seq_num > .log |

## <a name="metric-data-doesnt-show-in-azure-portal"></a>Mått data visas inte i Azure-portalen
Azure-diagnostik innehåller en massa mått data som kan visas i Azure-portalen. Om du har problem med att visa dessa data i portalen, kontrollera hello diagnostiklagringskonto -> WADMetrics\* tabell toosee om hello motsvarande mått poster förekommer. Här hello PartitionKey av hello tabell är hello resurs-ID för den virtuella datorn eller virtuella datorn och hello RowKey är hello måttnamnet t.ex. namn på prestandaräknare.

Om hello resurs-ID är fel, kontrollera diagnostik Configuration -> mått -> ResourceId toosee om hello resurs-ID är korrekt.

Om det finns inga data för hello specifikt mått, kontrollera diagnostik Configuration -> PerformanceCounter toosee om hello mått (prestandaräknaren) ingår. Vi aktivera hello följande räknare som standard.
- \Processor(_Total)\% processortid
- \Memory\Tillgängliga byte
- \ASP.NET program (__totalt__) \Requests/Sec
- \ASP.NET program (__totalt__) \Errors totalt/sek
- \ASP.NET\Requests i kö
- \ASP.NET\Requests avvisade
- \Processor(W3wp)\% processortid
- \Process (w3wp) \Private byte
- \Process(WaIISHost)\% processortid
- \Process (WaIISHost) \Private byte
- \Process(WaWorkerHost)\% processortid
- \Process (WaWorkerHost) \Private byte
- \Memory\Page fel per sekund
- \.NET CLR-minne (_globala_)\% tid i GC i
- \LogicalDisk (C:) \Disk skrivna byte/s
- \LogicalDisk (C:) \Disk-lästa byte/s
- \LogicalDisk (D:) \Disk skrivna byte/s
- \LogicalDisk (D:) \Disk-lästa byte/s

Om hello-konfigurationen är korrekt, men du fortfarande inte kan se hello mätvärden, följer du hello riktlinjerna nedan för hello ytterligare undersökning.


## <a name="azure-diagnostics-is-not-starting"></a>Startar inte Azure-diagnostik
Titta på **DiagnosticsPluginLauncher.log** och **DiagnosticsPlugin.log** filer från hello platsen för hello loggfiler angivna ovan för information om varför diagnostik misslyckades-toostart. 

Om de här loggarna visar `Monitoring Agent not reporting success after launch`, betyder det gick inte att starta MonAgentHost.exe. Titta på hello loggar för den i hello-plats som anges för `MonAgentHost log file` i hello ovan.

hello sista raden i hello loggfiler innehåller hello slutkod.  

```
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0
```
Om du hittar en **negativa** slutkod finns toohello [avsluta kod tabell](#azure-diagnostics-plugin-exit-codes) i hello [referenser](#references).

## <a name="diagnostics-data-is-not-logged-tooazure-storage"></a>Diagnostikdata är inte inloggad tooAzure lagring
Avgör om inga data visas eller bara vissa hello data inte visas.

### <a name="diagnostics-infrastructure-logs"></a>Diagnostik infrastruktur loggar
Diagnostik infrastruktur loggar är där azure diagnostics loggar alla fel som körs i. Kontrollera att du har aktiverat ([så?](#how-to-check-diagnostics-extension-configuration)) samla in diagnostik infrastruktur loggar i konfigurationen och snabbt leta efter eventuella relevanta fel som visas i hello `DiagnosticInfrastructureLogsTable` tabellen i konfigurerade storage-konto.

### <a name="no-data-is-showing-up"></a>Inga data visas
hello vanligaste orsaken till att händelsedata saknas helt är felaktigt angiven lagring kontoinformation.

Lösning: Korrigera diagnostik-konfiguration och installera om diagnostik.

Om hello storage-konto är korrekt konfigurerad, remote skrivbord till hello datorn och kontrollera att DiagnosticsPlugin.exe och MonAgentCore.exe körs. Om de inte körs följer [startar inte Azure-diagnostik](#azure-diagnostics-is-not-starting). Om hello processer körs, hoppa för[är data som hämtats komma lokalt](#is-data-getting-captured-locally) och följ den här guiden därifrån på.

### <a name="part-of-hello-data-is-missing"></a>En del av hello data saknas
Om du får vissa data men inte andra. Detta innebär hello datainsamling / överföring pipeline har angetts korrekt. Följ hello underavsnitt här toonarrow ned vilken hello problemet är:
#### <a name="is-collection-configured"></a>Samlingen konfigureras: 
Diagnostik-konfigurationen innehåller hello del som instruerar för en viss typ av data toobe samlas in. [Granska konfigurationen](#how-to-check-diagnostics-extension-configuration) toomake säker på att du inte söker efter data du har inte konfigurerats för samlingen.
#### <a name="is-hello-host-generating-data"></a>Hello värden genererar data:
- **Prestandaräknare**: öppna perfmon och kontrollera hello räknaren.
- **Spårningsloggar**: fjärrskrivbord till hello VM och lägga till en TextWriterTraceListener toohello app config-fil.  Se http://msdn.microsoft.com/library/sk36c28t.aspx tooset in hello text lyssnare.  Se till att hello `<trace>` element har `<trace autoflush="true">`.<br />
Om du inte ser spårningsloggar aviseringsstorm generera följer [mer om Trace Logs saknas](#more-about-trace-logs-missing).
- **ETW-spårning**: fjärrskrivbord till hello VM och installera förhandsgranskning.  I förhandsgranskning kör Arkiv ->-kommandot User > lyssna etwprovder1, etwprovider2 osv.  Observera att hello lyssna-kommandot är skiftlägeskänsligt och det får inte finnas blanksteg mellan hello kommaavgränsad lista över ETW-providers.  Om hello kommandot misslyckas toorun, kan du klicka hello ”loggar” i hello ned till höger i hello förhandsgranskning verktyget toosee försök toorun och vilka hello resultatet var vilken var.  Under förutsättning att hello indata är rätt sedan ett nytt fönster visas och i några sekunder börjar du se ETW-spårning.
- **Händelseloggar**: fjärrskrivbord till hello VM. Öppna `Event Viewer` och kontrollera hello-händelser finns.
#### <a name="is-data-getting-captured-locally"></a>Data komma avbildas lokalt:
Kontrollera nästa hello data komma fångas lokalt.
hello data lagras lokalt i `*.tsf` filer i [hello lokal lagring för diagnostikdata](#log-artifacts-path). Olika typer av loggar hämta som samlas in i olika `.tsf` filer. hello namn är liknande toohello tabellnamn i azure-lagring. Till exempel `Performance Counters` hämta samlas in i `PerformanceCountersTable.tsf`, händelseloggar hämta som samlas in i `WindowsEventLogsTable.tsf`. Använd hello-instruktioner i [lokala loggen extrahering](#local-log-extraction) avsnittet tooopen hello lokal insamling filer och kontrollera att du kan se dem komma in på disk.

Om du inte se loggar komma in lokalt och redan har kontrollerat att hello värden genererar data, har troligen konfigurationsproblem. Granska konfigurationen noggrant för hello relevant avsnitt. Granska även hello configuration genereras för MonitoringAgent [MaConfig.xml](#log-artifacts-path) och kontrollera att det finns vissa avsnitt som beskriver hello relevant logg källa och att det inte går förlorad i översättningen mellan azure diagnostics-konfiguration och övervakningskonfigurationen för agenten.
#### <a name="is-data-getting-transferred"></a>Data komma överförs:
Om du har verifierat hello data komma fångas lokalt men du fortfarande ser inte det i ditt lagringskonto: 
- Först och främst måste du kontrollera att du har angett rätt lagringskontot och att du inte har flyttats över nycklar etc.for hello angivna lagringskontot. För molntjänster, ibland vi se att personer som inte uppdaterar sina `useDevelopmentStorage=true`.
- Om lagringskontot är korrekt. Kontrollera att du inte har några nätverksbegränsningar som inte tillåter hello komponenter tooreach offentlig lagring slutpunkter. Enkelriktade toodo som är tooremote skrivbordet till hello datorn och försök toowrite något toohello samma lagringsutrymme konto själv.
- Du kan dessutom försök och se vilka fel rapporterats av Monitoring Agent. Övervakningsagent skriver loggar i `maeventtable.tsf` finns i [hello lokal lagring för diagnostikdata](#log-artifacts-path). Följ instruktionerna i [lokala loggen extrahering](#local-log-extraction) avsnittet tooopen den här filen och försök och ta reda på om det finns `errors` som anger fel tooread lokala filer eller skriva toostorage.

### <a name="capturing--archiving-logs"></a>Samla in / Arkivera loggar
Du har gått igenom hello senare steg, men kunde inte ta reda på vad som gick fel och funderar på att kontakta supporten. hello första kanske du ombeds är toocollect loggar från din dator. Du kan spara tid genom att göra den själv. Kör hello `CollectGuestLogs.exe` utility på [loggen samling verktyget sökväg](#log-artifacts-path) och genererar en zip-fil med alla relevanta azure loggar i hello samma mapp.

## <a name="diagnostics-data-tables-not-found"></a>Diagnostikdata tabeller som inte hittades
hello tabeller i Azure storage ETW-händelser som är namngivna med hello följande kod:

```C#
        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;
```

Här är ett exempel:

```XML
        <EtwEventSourceProviderConfiguration provider="prov1">
          <Event id="1" />
          <Event id="2" eventDestination="dest1" />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider="prov2">
          <DefaultEvents eventDestination="dest2" />
        </EtwEventSourceProviderConfiguration>
```
```JSON
"EtwEventSourceProviderConfiguration": [
    {
        "provider": "prov1",
        "Event": [
            {
                "id": 1
            },
            {
                "id": 2,
                "eventDestination": "dest1"
            }
        ],
        "DefaultEvents": {
            "eventDestination": "DefaultEventDestination",
            "sinks": ""
        }
    },
    {
        "provider": "prov2",
        "DefaultEvents": {
            "eventDestination": "dest2"
        }
    }
]
```
Som genererar 4 tabeller:

| Händelse | Tabellnamnet |
| --- | --- |
| Provider = ”prov1” &lt;händelse-id = ”1” /&gt; |WADEvent + MD5("prov1") + ”1” |
| Provider = ”prov1” &lt;händelse-id = ”2” eventDestination = ”dest1” /&gt; |WADdest1 |
| Provider = ”prov1” &lt;DefaultEvents /&gt; |WADDefault+MD5("prov1") |
| Provider = ”prov2” &lt;DefaultEvents eventDestination = ”dest2” /&gt; |WADdest2 |

## <a name="references"></a>Referenser

### <a name="how-toocheck-diagnostics-extension-configuration"></a>Hur toocheck konfiguration för diagnostik
Hej enklaste sättet toocheck konfigurationen av tillägget är toonavigate toohttp://resources.azure.com, navigera toohello virtuella datorer eller moln-tjänsten på vilka hello Azure Diagnostics tillägg (IaaSDiagnostics / PaaDiagnostics) i fråga.

Alternativt fjärrskrivbord till hello datorn och titta på hello Azure Diagnostics konfigurationsfilen beskrivs i hello lämpliga [här](#log-artifacts-path).

I antingen case söka efter **Microsoft.Azure.Diagnostics** sedan för hello **xmlCfg** eller **WadCfg** fältet. 

Om virtuella datorer om hello WadCfg fältet finns, betyder det hello config är i JSON. Om hello xmlCfg fältet finns innebär hello config är XML och det är base64-kodad. Du behöver för[avkoda den](http://www.bing.com/search?q=base64+decoder) toosee hello XML som har lästs in av diagnostik.

För Molntjänsten roll om du väljer hello konfigurationen från disken, hello data är base64-kodad så du behöver för[avkoda den](http://www.bing.com/search?q=base64+decoder) toosee hello XML som har lästs in av diagnostik.

### <a name="azure-diagnostics-plugin-exit-codes"></a>Azure Diagnostics slutkoder för plugin-program
hello-plugin-programmet returnerar hello följande koder:

| Slutkod | Beskrivning |
| --- | --- |
| 0 |Lyckades. |
| -1 |Allmänt fel. |
| -2 |Det går inte tooload hello rcf fil.<p>Den här internt fel ska bara hända om hello gäst Agentstart plugin-program är felaktigt, manuellt startas på hello VM. |
| -3 |Det går inte att läsa in konfigurationsfilen för hello diagnostik.<p><p>Lösning: På grund av en konfigurationsfil som inte skickar schemavalidering. hello-lösning är tooprovide en konfigurationsfil som uppfyller hello schemat. |
| -4 |En annan instans av hello övervakningsagent diagnostik använder redan hello lokala resursbiblioteket.<p><p>Lösning: Ange ett annat värde för **LocalResourceDirectory**. |
| -6 |hello gäst Agentstart plugin-programmet försökte toolaunch diagnostik med en ogiltig kommandorad.<p><p>Den här internt fel ska bara hända om hello gäst Agentstart plugin-program är felaktigt, manuellt startas på hello VM. |
| -10 |Diagnostik-plugin-programmet hello avslutades med ett ohanterat undantag. |
| -11 |hello gäst-agenten kunde toocreate hello processen som ansvarar för att starta och övervaka hello övervakningsagent.<p><p>Lösning: Kontrollera att tillräckligt med systemresurser för är tillgängliga toolaunch nya processer.<p> |
| -101 |Ogiltiga argument vid anrop av hello diagnostik plugin-programmet.<p><p>Den här internt fel ska bara hända om hello gäst Agentstart plugin-program är felaktigt, manuellt startas på hello VM. |
| -102 |hello är plugin-programmet tooinitialize sig själv.<p><p>Lösning: Kontrollera att tillräckligt med systemresurser för är tillgängliga toolaunch nya processer. |
| -103 |hello är plugin-programmet tooinitialize sig själv. Det är särskilt toocreate hello loggaren objekt.<p><p>Lösning: Kontrollera att tillräckligt med systemresurser för är tillgängliga toolaunch nya processer. |
| -104 |Det går inte tooload hello rcf fil från hello gästagenten.<p><p>Den här internt fel ska bara hända om hello gäst Agentstart plugin-program är felaktigt, manuellt startas på hello VM. |
| -105 |Diagnostik-plugin-programmet hello går inte att öppna konfigurationsfilen för hello diagnostik.<p><p>Den här internt fel ska bara hända om hello diagnostik plugin-program är felaktigt, manuellt startas på hello VM. |
| -106 |Det går inte att läsa konfigurationsfilen för hello diagnostik.<p><p>Lösning: På grund av en konfigurationsfil som inte skickar schemavalidering. Hello-lösning är därför tooprovide en konfigurationsfil som uppfyller hello schemat. Se [hur toocheck konfiguration för diagnostik](#how-to-check-diagnostics-extension-configuration). |
| -107 |hello resurs directory pass toohello övervakningsagent är ogiltig.<p><p>Den här internt fel ska bara hända om hello övervakningsagent har startats manuellt, felaktigt, på hello VM.</p> |
| -108 |Tooconvert hello diagnostik konfigurationsfilen till hello monitoring agent-konfigurationsfilen.<p><p>Den här internt fel ska bara hända om diagnostik-plugin-programmet hello anropas manuellt med en ogiltig konfigurationsfil. |
| -110 |Allmänna diagnostik konfigurationsfel.<p><p>Den här internt fel ska bara hända om diagnostik-plugin-programmet hello anropas manuellt med en ogiltig konfigurationsfil. |
| -111 |Det går inte toostart hello-övervakningsagenten.<p><p>Lösning: Kontrollera att det finns tillräckligt med systemresurser för. |
| -112 |Allmänt fel |

### <a name="local-log-extraction"></a>Lokal logg extrahering
Hej övervakningsagent samlar in loggar och artefakter som `.tsf` filer. `.tsf`filen kan inte läsas men du kan konvertera den till en `.csv` på följande sätt: 

```
<Azure diagnostics extension package>\Monitor\x64\table2csv.exe <relevantLogFile>.tsf
```
En ny fil kallas `<relevantLogFile>.csv` kommer att skapas i hello samma sökväg som hello motsvarande `.tsf` fil.

**Obs**: behöver du bara toorun verktyget mot hello huvudsakliga tsf fil (t.ex. PerformanceCountersTable.tsf). hello tillhörande filer (t.ex. PerformanceCountersTables_\*\*001.tsf PerformanceCountersTables_\*\*002. tsf etc.) kommer automatiskt att bearbetas.

### <a name="more-about-trace-logs-missing"></a>Mer information om spårningsloggar saknas

**Obs:** detta gäller främst toocloud services bara om du har konfigurerat hello DiagnosticsMonitorTraceListener på ett program som körs på IaaS-VM. 

- Se till att hello DiagnosticMonitorTraceListener konfigureras i hello web.config eller app.config.  Detta är konfigurerat som standard i molntjänstprojekt, men vissa kunder kommentera det out vilket leder till hello trace instruktioner toonot samlas in av diagnostik. 
- Om inte komma skrivs från hello OnStart eller kör metoden kontrollerar du att hello DiagnosticMonitorTraceListener är i hello app.config.  Som standard är det i hello web.config, men som endast gäller toocode som körs i w3wp.exe; Du behöver det i app.config toocapture spår som körs i WaIISHost.exe.
- Kontrollera att du använder Diagnostics.Trace.TraceXXX i stället för Diagnostics.Debug.WriteXXX.  hello felsökningsuttryck kommer att tas bort från en slutversion.
- Kontrollera att hello kompileras koden har faktiskt hello Diagnostics.Trace rader (Använd Reflector, ildasm eller ILSpy tooverify).  Diagnostics.Trace kommandon tas bort från hello kompileras binära såvida du inte använder hello TRACE villkorlig kompilering symbolen.  Om du använder msbuild toobuild hello projektet är ett vanligt problem toorun till.

## <a name="known-issues-and-mitigations"></a>Kända problem och åtgärder
Här följer en lista över kända problem med kända åtgärder:

**1. .NET 4.5 beroende:**

BOMULLSTUSS har beroende runtime på framework .NET 4.5 eller senare. För närvarande hello för att skriva detta, alla datorer som har etablerats för molntjänster, samt alla officiella azure grundläggande avbildningar av virtuella datorer .NET 4.5 eller senare installerat. Det är dock fortfarande möjligt tooland i en situation där försök toorun BOMULLSTUSS på en dator som inte har .NET 4.5 eller senare. Detta händer när du skapar din dator från en gamla avbildningen eller ögonblicksbild; eller aktiverar dina egna anpassade disk.

Detta visar vanligtvis som en slutkod **255** när du kör DiagnosticsPluginLauncher.exe. Felet inträffar på grund av toohello ohanterat undantag: 
```
System.IO.FileLoadException: Could not load file or assembly 'System.Threading.Tasks, Version=1.5.11.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies
```

**Lösning:** installera .NET 4.5 eller senare på datorn.

**2. Räknare för prestandadata i lagring men visas inte i portalen**

Virtuella datorer portaler visar vissa prestandaräknare som standard. Om du inte kan se dem och du vet hello data komma genereras eftersom den är tillgänglig för lagring. Kontrollera:
- Om hello data i lagring har räknarnamn på engelska. Om hello räknarnamn inte är på engelska portal mått diagrammet är inte kan toorecognize den.
- Om du använder jokertecken (\*) i ditt namn på prestandaräknare, hello portal inte kan toocorrelate hello konfigurerad och samlas in räknaren.

**Minskning**: ändra hello datorns språk tooEnglish för Systemkonton. Region -> Kontrollpanelen -> administrativa inställningar -> -> Avmarkera ”Välkommen och systemkonton” så att hello anpassade språk inte är tillämpas toosystem konto. Kontrollera också att du inte använda jokertecken om du vill använda portalen toobe din primära förbrukning upplevelse.
