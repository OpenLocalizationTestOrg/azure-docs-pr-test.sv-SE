---
title: "aaaAdd anpassad Service Fabric-hälsorapporter | Microsoft Docs"
description: "Beskriver hur toosend anpassade hälsorapporter tooAzure Service Fabric hälsa entiteter. Ger rekommendationer för att utforma och implementera kvalitet hälsorapporter."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 0a00a7d2-510e-47d0-8aa8-24c851ea847f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: oanapl
ms.openlocfilehash: 12c9f664e2a457b4e1e8f340873ca60ebcefb097
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-custom-service-fabric-health-reports"></a>Lägga till anpassade hälsorapporter i Service Fabric
Azure Service Fabric introducerar en [hälsomodell](service-fabric-health-introduction.md) utformad tooflag ohälsosamt kluster och villkoren för programmet på specifika enheter. Hej hälsomodell använder **hälsa rapportörer** (systemkomponenter och watchdogs). hello målet är enkelt och snabbt diagnos och reparation. Service-skrivare måste toothink förskott om hälsotillstånd. Eventuella villkor som kan påverka hälsa redovisas, särskilt om kan hjälpa flaggan problem Stäng toohello rot. hello hälsoinformation kan spara tid och pengar på felsökning och undersökning. hello användbarhet är särskilt tydligt när hello-tjänsten är igång och körs i skala i hello molnet (privat eller Azure).

hello Service Fabric-rapportörer Övervakare identifiera villkor av intresse. De rapport om dessa villkor baserat på deras lokala vyn. Hej [hälsoarkivet](service-fabric-health-introduction.md#health-store) aggregerar hälsa data som skickas av alla rapportörer toodetermine om enheter är globalt felfria. hello modellen är avsedda toobe omfattande, flexibel och enkel toouse. hello kvaliteten på hello hälsorapporter anger hello riktighet hello hälsotillståndsvy hello-klustret. Falska positiva identifieringar som visar felaktigt ohälsosamt problem kan påverka uppgraderingar och andra tjänster som använder hälsa data. Exempel på sådana tjänster är reparation och mekanismer för aviseringar. Därför tänka är nödvändiga tooprovide rapporter som samlar in villkor för intresse för hello bästa möjliga sättet.

toodesign och implementera hälsa reporting watchdogs komponenterna, och system måste:

* Definiera villkor för hello intressanta, hello sättet som den är övervakad och hello inverkan på hello klustret eller programmet funktioner. Baserat på den här informationen kan besluta om hello rapporten egenskapen och hälsa hälsotillstånd.
* Fastställa hello [entiteten](service-fabric-health-introduction.md#health-entities-and-hierarchy) hello rapporten avser.
* Avgöra var reporting hello utförs, inifrån hello tjänsten eller från en intern eller extern watchdog.
* Definiera en datakälla som används för tooidentify hello-rapport.
* Välj en reporting strategi regelbundet eller på övergångar. hello rekommenderat sätt är med jämna mellanrum, eftersom det kräver att enklare koden och är mindre risk tooerrors.
* Avgör hur lång hello-rapport för ohälsosamt villkor ska vara i hello health store och hur det ska vara avmarkerade. Med den här informationen kan bestämma hello rapporten tid toolive och ta bort på förfallodatum beteende.

Som tidigare nämnts kan kan reporting göras från:

* hello övervakade replik för Service Fabric-tjänsten.
* Internt watchdogs distribueras som en Service Fabric-tjänsten till exempel ett Service Fabric tillståndslös som övervakar villkor och skickar rapporter. Hej watchdogs kan vara distribueras ett alla noder eller kan vara tillhör toohello övervakas tjänst.
* Internt watchdogs som körs på hello Service Fabric-noder, men som *inte* implementeras som Service Fabric-tjänster.
* Externa watchdogs avsökningen hello resursen från *utanför* hello Service Fabric-klustret (till exempel övervakningstjänsten som Gomez).

> [!NOTE]
> Out of box hello fylls hello kluster med hälsorapporter som skickas av hello systemkomponenter. Läs mer på [med systemhälsa rapporter för felsökning](service-fabric-understand-and-troubleshoot-with-system-health-reports.md). hello Användarrapporter måste skickas [hälsa entiteter](service-fabric-health-introduction.md#health-entities-and-hierarchy) som redan har skapats av hello system.
> 
> 

En gång hello hälsa reporting design är avmarkerad, hälsorapporter kan skickas enkelt. Du kan använda [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) tooreport hälsa om hello klustret inte [säker](service-fabric-cluster-security.md) eller om hello fabric-klienten har administratörsrättigheter. Reporting kan göras via hello API med hjälp av [FabricClient.HealthManager.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth), via PowerShell eller via REST. Konfigurationen rattar batch-rapporter för bättre prestanda.

> [!NOTE]
> Rapporten hälsa är synkron och representerar endast hello validering arbete på hello på klientsidan. hello att hello rapporten accepteras av hello hälsa klient eller hello `Partition` eller `CodePackageActivationContext` objekt innebär inte att tillämpas i hello store. Skickas asynkront och eventuellt batchar med andra rapporter. hello bearbetning på servern hello misslyckas fortfarande: hello sekvensnummer kan vara inaktuella, hello entiteten på vilka hello rapporten måste tillämpas har tagits bort, osv.
> 
> 

## <a name="health-client"></a>Hälsotillstånd klienten
hello hälsorapporter skickas toohello health store via en hälsa-klient finns i hello fabric-klienten. hello hälsa klient kan konfigureras med hello följande inställningar:

* **HealthReportSendInterval**: hello fördröjning mellan hello tid hello rapporten är tillagda toohello klienten hello tid och det skickas toohello health store. Använda toobatch rapporter i ett enda meddelande, i stället för att skicka ett meddelande för varje rapport. hello batchbearbetning förbättrar prestanda. Standard: 30 sekunder.
* **HealthReportRetrySendInterval**: hello intervall på vilka hello hälsa klienten skickar ackumulerade hälsa rapporterar toohello health store. Standard: 30 sekunder.
* **HealthOperationTimeout**: hello tidsgränsen för ett rapportmeddelande skickas toohello health store. Om ett meddelande på grund av timeout, klientens hello hälsa den tills hello hälsoarkivet bekräftar att hello rapporten har bearbetats. Standard: två minuter.

> [!NOTE]
> När hello rapporter grupperas hello fabric-klienten måste hållas aktiva för minst hello HealthReportSendInterval tooensure som de skickas. Om hello-meddelande försvinner eller hello health store kan inte använda dem på grund av fel tootransient hello fabric-klienten måste hållas alive längre toogive den en chansen tooretry.
> 
> 

hello buffring på hello klient tar hello unika hello rapporter i beräkningen. Till exempel om en viss felaktiga rapport rapporterar 100 rapporter per sekund på hello samma egenskap för hello hello rapporter har ersatts med den senaste versionen av hello samma entitet. Högst en sådan rapport finns i kön för hello-klienten. Om batchbearbetning konfigureras, skickas hello antal rapporter toohello health store är bara en per skicka intervall. Den här rapporten är hello senaste tillagda rapporten som visar hello hello entitet mest aktuella tillstånd.
Ange konfigurationsparametrar när `FabricClient` har skapats genom att skicka [FabricClientSettings](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclientsettings) med hello önskade värden hälsa-relaterade poster.

hello följande exempel skapas en fabric-klienten och anger hello rapporter ska skickas när de har lagts till. På timeout och fel som kan göras inträffa försök varje 40 sekunder.

```csharp
var clientSettings = new FabricClientSettings()
{
    HealthOperationTimeout = TimeSpan.FromSeconds(120),
    HealthReportSendInterval = TimeSpan.FromSeconds(0),
    HealthReportRetrySendInterval = TimeSpan.FromSeconds(40),
};
var fabricClient = new FabricClient(clientSettings);
```

Rekommenderar vi att hello standard fabric klientinställningar som angetts `HealthReportSendInterval` too30 sekunder. Den här inställningen garanterar optimala prestanda på grund av toobatching. Kritiska rapporter som måste skickas så snart som möjligt, använda `HealthReportSendOptions` med Immediate `true` i [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) API. Omedelbar rapporter kringgå hello batchbearbetning intervall. Använd den här flaggan med försiktighet; Vi vill tootake nytta av hello hälsa klienten batchbearbetning när det är möjligt. Omedelbar skicka är också praktisk när du stänger hello fabric-klienten (till exempel hello processen har fastställt ogiltigt tillstånd och behöver tooshut ned tooprevent sidoeffekter). Det garanterar en bästa skicka hello ackumulerade rapporter. När en rapport läggs med omedelbar flaggan batchar hello hälsa klienten alla hello ackumulerade rapporter sedan senaste överföringen.

Samma parametrar kan anges när en anslutning tooa klustret har skapats med hjälp av PowerShell. hello följande exempel startas en anslutning tooa lokala klustret:

```powershell
PS C:\> Connect-ServiceFabricCluster -HealthOperationTimeoutInSec 120 -HealthReportSendIntervalInSec 0 -HealthReportRetrySendIntervalInSec 40
True

ConnectionEndpoint   :
FabricClientSettings : {
                       ClientFriendlyName                   : PowerShell-1944858a-4c6d-465f-89c7-9021c12ac0bb
                       PartitionLocationCacheLimit          : 100000
                       PartitionLocationCacheBucketCount    : 1024
                       ServiceChangePollInterval            : 00:02:00
                       ConnectionInitializationTimeout      : 00:00:02
                       KeepAliveInterval                    : 00:00:20
                       HealthOperationTimeout               : 00:02:00
                       HealthReportSendInterval             : 00:00:00
                       HealthReportRetrySendInterval        : 00:00:40
                       NotificationGatewayConnectionTimeout : 00:00:00
                       NotificationCacheUpdateTimeout       : 00:00:00
                       }
GatewayInformation   : {
                       NodeAddress                          : localhost:19000
                       NodeId                               : 1880ec88a3187766a6da323399721f53
                       NodeInstanceId                       : 130729063464981219
                       NodeName                             : Node.1
                       }
```

På liknande sätt tooAPI, rapporter kan skickas med `-Immediate` växla toobe skickas direkt, oavsett hello `HealthReportSendInterval` värde.

För övriga skickas hello rapporterna toohello Service Fabric-gateway som har en intern fabric-klienten. Den här klienten är som standard konfigurerade toosend rapporter batchar med 30 sekunders mellanrum. Du kan ändra hello batch intervall med hello klustret Konfigurationsinställningen `HttpGatewayHealthReportSendInterval` på `HttpGateway`. Som tidigare nämnts kan ett bättre alternativ är toosend hello rapporter med `Immediate` SANT. 

> [!NOTE]
> tooensure som obehörig services kan inte rapportera hälsa mot hello entiteter i hello kluster, konfigurera hello server tooaccept begäranden endast från skyddade klienter. Hej `FabricClient` används för rapportering måste ha Säkerhetsaktiverade toobe kan toocommunicate med hello-kluster (till exempel med Kerberos eller certifikat-autentisering). Läs mer om [kluster säkerhet](service-fabric-cluster-security.md).
> 
> 

## <a name="report-from-within-low-privilege-services"></a>Rapport från i låg behörighet services
Om Service Fabric-tjänster inte har admin åtkomst toohello kluster, kan du rapportera hälsa på entiteter från hello aktuell kontext via `Partition` eller `CodePackageActivationContext`.

* För tillståndslösa tjänster använder [IStatelessServicePartition.ReportInstanceHealth](https://docs.microsoft.com/dotnet/api/system.fabric.istatelessservicepartition.reportinstancehealth) tooreport på hello aktuella tjänstinstansen.
* Tillståndskänsliga tjänster använder [IStatefulServicePartition.ReportReplicaHealth](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition.reportreplicahealth) tooreport på aktuella repliken.
* Använd [IServicePartition.ReportPartitionHealth](https://docs.microsoft.com/dotnet/api/system.fabric.iservicepartition.reportpartitionhealth) tooreport på hello aktuell partition entitet.
* Använd [CodePackageActivationContext.ReportApplicationHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportapplicationhealth) tooreport på aktuella programmet.
* Använd [CodePackageActivationContext.ReportDeployedApplicationHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedapplicationhealth) tooreport på hello aktuella program som distribuerats på hello aktuella noden.
* Använd [CodePackageActivationContext.ReportDeployedServicePackageHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedservicepackagehealth) tooreport på inget tjänstepaket för hello-program som distribuerats på hello aktuella noden.

> [!NOTE]
> Internt hello `Partition` och hello `CodePackageActivationContext` håller en hälsa-klient som konfigurerats med standardinställningar. Enligt beskrivningen för hello [hälsa klienten](service-fabric-report-health.md#health-client), rapporter batchbearbetas och skickas på en timer. hello-objekt ska vara kvar alive toohave en chans toosend hello-rapport.
> 
> 

Du kan ange `HealthReportSendOptions` när du skickar rapporter via `Partition` och `CodePackageActivationContext` hälsa API: er. Om du har kritiska rapporter som måste skickas så snart som möjligt, Använd `HealthReportSendOptions` med Immediate `true`. Omedelbar rapporter kringgå hello batchbearbetning intervall på hello interna hälsa klienten. Som tidigare nämnts, Använd den här flaggan med försiktighet; Vi vill tootake nytta av hello hälsa klienten batchbearbetning när det är möjligt.

## <a name="design-health-reporting"></a>Utforma hälsa reporting
hello identifierar första steget i att generera rapporter för hög kvalitet hello villkor som kan påverka hello hälsotillståndet för hello-tjänsten. Eventuella villkor som kan hjälpa flaggan problem i hello tjänst eller ett kluster när den startar-- eller ännu bättre innan ett problem inträffar--kan potentiellt spara miljarder dollar. hello fördelar mindre stillestånd, färre nattetid för att undersöka och reparera problem och högre nöjda.

När hello villkor har identifierats, måste watchdog skrivare toofigure ut hello bästa sätt toomonitor dem för balans mellan kostnader och användbarhet. Anta till exempel att en tjänst som utför komplexa beräkningar som använder vissa temporära filer på en nätverksresurs. En watchdog kan övervaka hello resursen tooensure att det finns tillräckligt med utrymme. Det kan lyssna efter meddelanden om ändringar av filen eller katalogen. Det kan rapportera en varning om ett tröskelvärde för Summa har nåtts och rapportera ett fel om hello resursen är full. På en varning startade en reparation system Rensa äldre filer på hello resursen. En reparation system kan flytta hello-replik tooanother tjänstnoden på ett fel. Observera hur hello villkoret tillstånd beskrivs vad gäller hälsa: hello hello villkor som kan uppfattas som Felfri (ok) eller ohälsosamt (varning eller fel).

Efter hello övervakning information har angetts måste en watchdog skrivare toofigure ut hur tooimplement hello watchdog. Om hello villkor kan fastställas från inom hello service, kan hello watchdog ingå i själva hello övervakade tjänsten. Hello-Tjänstkod kan exempelvis kontrollera hello resursen användning och rapportera varje gång den försöker toowrite en fil. hello nytta av den här metoden är att reporting är enkel. Vara måste försiktig tooprevent watchdog buggar från påverka hello-funktionerna.

Rapportering i hello övervakas service är inte alltid ett alternativ. En watchdog inom hello-tjänsten kanske inte kan toodetect hello villkor. Det får inte ha hello logik och data toomake hello bestämning. hello arbetet med att övervaka hello villkor kan vara hög. hello villkor får även inte vara specifika tooa service, men i stället påverkar samverkan mellan tjänster. Ett annat alternativ är toohave watchdogs i hello kluster som separata processer. Hej watchdogs övervaka hello villkor och rapport, utan att påverka hello tjänster på något sätt. Till exempel dessa watchdogs kan genomföras som tillståndslösa tjänster i hello samma program som distribueras på alla noder eller hello samma noder som hello-tjänst.

Ibland kan är en watchdog som körs i klustret hello inte ett alternativ antingen. Om hello övervakas villkoret är hello tillgänglighet eller funktionerna i hello tjänsten som användarna ser den, är det bästa toohave hello watchdogs i hello samma placera som hello användaren klienter. Där de kan testa hello åtgärder i hello samma hur användare ringa upp dem. Du kan till exempel ha en watchdog som finns utanför hello kluster utfärdar begäranden toohello tjänsten och kontrollerar hello svarstid och hello resultatet är korrekt. (För en Kalkylatorn tjänst, till exempel 2 + 2 returnerar 4 inom en rimlig tid?)

När hello watchdog information har avslutats, bör du bestämma på en käll-ID som unikt identifierar den. Om flera watchdogs av samma typ bor i hello hello kluster, måste de rapportera på olika enheter, eller om de rapporterar på hello samma entitet, Använd olika käll-ID eller egenskap. Det här sättet sina rapporter kan samexistera. hello-egenskapen för hello hälsorapport ska avbilda hello övervakas villkor. (Exempelvis hello ovan hello egenskapen kunde **ShareSize**.) Om flera rapporter gäller toohello samma villkor, hello egenskapen innehålla vissa dynamisk information som tillåter toocoexist rapporter. Om flera resurser behöver toobe övervakas, hello egenskapsnamn kan exempelvis **ShareSize sharename**.

> [!NOTE]
> Gör *inte* Använd hello health store tookeep statusinformation. Endast hälsa-relaterad information ska rapporteras som hälsa, som den här informationen påverkar hello hälsa utvärdering för en entitet. hello health store har inte utformats som ett allmänt Arkiv. Används hälsa utvärdering logik tooaggregate alla data i hello hälsotillstånd. Skicka information orelaterade toohealth (som rapporterar status med ett hälsotillstånd OK) inte påverkar hello samman hälsotillstånd, men det kan påverka hello prestanda för hello health store.
> 
> 

hello är nästa beslut vilken entitet tooreport på. För mesta hello hello villkoret tydligt hello idetifies du entiteten. Välja hello entitet med bästa möjliga granularitet. Om ett villkor som påverkar alla repliker i en partition, en rapport om hello partition, inte på hello-tjänsten. Det finns specialfall där flera tankar krävs dock. Om hello villkor påverkar en entitet, till exempel en replik, men hello är önskan toohave hello villkor som flaggats för mer än hello replik livslängd redovisas på hello partition. I annat fall när hello repliken tas bort rensar hello hälsoarkivet alla rapporter. Watchdog-skrivare måste du tänka hello livslängd hello enhetens och hello rapporten. Det måste vara tydlig när en rapport måste rensas från en butik (till exempel när ett fel rapporteras på en enhet inte längre gäller).

Nu ska vi titta på ett exempel som sätter ihop hello punkter I som beskrivs. Överväg ett Service Fabric-program består av en master tillståndskänslig beständig tjänst och sekundära tillståndslösa tjänster som distribueras på alla noder (en sekundär service typ för varje typ av aktivitet). hello-hanteraren har en kö för bearbetning som innehåller kommandon toobe körs av sekundärservrar. hello sekundärservrar utför hello inkommande begäranden och skicka tillbaka bekräftelse signaler. Ett villkor som kan övervakas är hello hello master bearbetningskön. Om hello master Kölängd når ett tröskelvärde, rapporterade en varning. hello varning anger att hello sekundärservrar inte kan hantera hello belastning. Om hello kön når hello maxlängd och kommandon släpps, ett fel rapporteras, som hello tjänsten går inte att återställa. hello rapporter kan finnas på hello egenskapen **QueueStatus**. hello watchdog finns i hello-tjänsten och det har skickats på hello master primära repliken. hello tid toolive är två minuter och skickas regelbundet var 30: e sekund. Om hello primära kraschar rensas hello rapporten automatiskt från store. Om hello service repliken är in, men det är ett dödläge eller andra problem med hello rapporten går ut i hello health store. I det här fallet utvärderas hello entiteten vid fel.

Ytterligare villkor som kan övervakas är körningstid för aktiviteten. hello master distribuerar uppgifter toohello sekundärservrar baserat på hello aktivitetstyp. Beroende på hello design kunde hello master avsöka hello sekundärservrar för aktivitetsstatus. Det kan också vänta tills sekundärservrar toosend tillbaka bekräftelse signaler när de görs. I andra fall hello måste vara försiktig toodetect situationer där sekundärservrar die eller meddelanden går förlorade. Ett alternativ är för hello master toosend toohello en ping-begäran samma sekundär som skickar tillbaka dess status. Om ingen status tas emot hello master anser att det är ett fel och schemaläggs automatiskt hello uppgift på nytt. Det här beteendet förutsätts att hello uppgifter idempotent.

hello övervakas villkor kan översättas som en varning om hello aktiviteten inte har gjort i en viss tid (**t1**, till exempel 10 minuter). Om hello åtgärden inte slutförs i tid (**t2**, till exempel 20 minuter), hello övervakas villkor kan översättas som fel. Den här reporting kan du göra detta på flera olika sätt:

* hello master primära repliken rapporter om sig själv med jämna mellanrum. Du kan ha en egenskap för alla väntande aktiviteter i hello kö. Om minst en uppgift tar längre tid, hello rapportera status för hello egenskapen **PendingTasks** är en varning eller fel efter behov. Om det finns inga väntande aktiviteter eller alla aktiviteter startade körningen är hello rapportera status OK. hello uppgifter är beständiga. Om hello primära kraschar fortsätta hello nyligen upphöja primära tooreport korrekt.
* En annan watchdog process (i hello molnet eller externa) kontrollerar hello uppgifter (från utanför, baserat på hello önskad aktivitet resultatet) toosee om de är slutförda. Om de inte följa hello tröskelvärden, skickas en rapport på hello-huvudtjänsten. En rapport som också skickas med varje aktivitet som innehåller hello aktivitets-ID som **PendingTask + taskId**. Rapporter ska skickas endast ohälsosamt tillstånd. Ange tid toolive tooa några minuter och markera hello rapporter toobe tas bort när de upphör att gälla tooensure rensning.
* sekundär hello som kör en uppgift rapporterar när det tar längre tid än förväntat toorun den. Rapporterar på hello tjänstinstans för hello egenskapen **PendingTasks**. hello rapporten lokaliserar hello service-instans som har problem, men det fånga inte hello situation där hello instans dör. hello rapporter rensas sedan. Det kan rapportera om hello sekundär service. Om hello sekundära är klar hello uppgiften rensar hello sekundära instans hello rapport från hello store. hello rapporten fånga inte hello situation där hello bekräftelsemeddelande går förlorad och hello aktiviteten är inte klar hello master utgångspunkt från.

Men hello reporting görs i hello fall som beskrivs ovan, fångas hello rapporter i programmets hälsotillstånd när hälsa utvärderas.

## <a name="report-periodically-vs-on-transition"></a>Rapport med jämna mellanrum kontra om övergång
Med hjälp av modellen för hälsotillståndsrapportering hello skicka watchdogs rapporter med jämna mellanrum eller övergångar. hello bör sätt för watchdog rapportering med jämna mellanrum, eftersom hello koden är mycket enklare och mindre känslig tooerrors. Hej watchdogs måste arbetar toobe så enkelt som möjligt tooavoid buggar som utlöser felaktiga rapporter. Felaktig *ohälsosamt* rapporter påverka hälsa utvärderingar och scenarier baserat på hälsa, inklusive uppgraderingar. Felaktig *felfri* rapporter dölja problem i hello-kluster, vilket inte är önskvärd.

Hello watchdog kan implementeras med en timer för periodiska rapporteringen. På en timer-återanrop hello watchdog Kontrollera hello tillstånd och skicka en rapport som baseras på hello aktuella tillstånd. Det finns inga behov toosee vilken rapport har skickats tidigare eller se alla optimeringar vad gäller meddelanden. hello hälsa klienten har batchbearbetning logik toohelp med prestanda. Medan hello hälsa klienten hålls alive, nytt försök internt tills hello rapporten bekräftas av hello health store eller hello watchdog genererar en nyare rapport med hello samma entitet, egenskap och källa.

Rapportering i övergångar krävs noggrann hantering av tillstånd. hello watchdog övervakar vissa villkor och rapporterar endast när hello villkoren ändras. hello upp och med den här metoden är att färre rapporter behövs. hello Nackdelen är att hello logiken för hello watchdog komplexa. hello watchdog måste upprätthålla hello villkor eller hello rapporter så att de kan vara inspekterade toodetermine tillstånd ändras. På redundanskluster, måste vara försiktig med rapporter som har lagts till, men ännu inte skickats toohello health store. hello sekvensnummer måste vara allt större. Om inte, avvisas hello rapporter som inaktiva. Synkroniseringen kan behövas mellan hello tillståndet för hello rapport och hello tillståndet för hello health store i hello sällsynta fall där dataförlust uppstår.

Rapportering av övergångar passar för tjänster som rapporterar på själva via `Partition` eller `CodePackageActivationContext`. När hello lokalt objekt (replikerings- eller distribuerat tjänstpaket / distribuerat program) är bort alla rapporter tas också bort. Den här automatisk rensning sänker hello behovet av att synkroniseringen mellan rapport och health store. Om hello rapporten avser överordnade partitionen eller det överordnade programmet, iakttas försiktighet på redundans tooavoid inaktuella rapporter i hello health store. Logik måste läggas toomaintain hello rätt tillstånd och avmarkera hello rapport från store när de inte behövs längre.

## <a name="implement-health-reporting"></a>Implementera hälsa reporting
När hello entiteten och rapportera information är klar, kan skickar hälsorapporter göras via hello API: et, PowerShell eller REST.

### <a name="api"></a>API
tooreport via hello API du behöver toocreate hälsa specifika toohello entitet rapporttyp de vill tooreport på. Ge hello rapporten tooa hälsa klienten. Du kan också skapa en hälsoinformation och skickar den toocorrect rapporteringsmetoder på `Partition` eller `CodePackageActivationContext` tooreport på aktuella entiteter.

hello följande exempel visar periodiska reporting från en watchdog inom hello kluster. hello watchdog kontrollerar om en extern resurs kan nås inifrån en nod. hello resursen krävs av ett service manifest hello-programmet. Om hello resursen är tillgänglig, kan hello andra tjänster inom hello program fortfarande fungerar korrekt. Därför hello rapporten skickas på hello distribuerad tjänst paketet enhet var 30: e sekund.

```csharp
private static Uri ApplicationName = new Uri("fabric:/WordCount");
private static string ServiceManifestName = "WordCount.Service";
private static string NodeName = FabricRuntime.GetNodeContext().NodeName;
private static Timer ReportTimer = new Timer(new TimerCallback(SendReport), null, 30 * 1000, 30 * 1000);
private static FabricClient Client = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });

public static void SendReport(object obj)
{
    // Test whether hello resource can be accessed from hello node
    HealthState healthState = this.TestConnectivityToExternalResource();

    // Send report on deployed service package, as hello connectivity is needed by hello specific service manifest
    // and can be different on different nodes
    var deployedServicePackageHealthReport = new DeployedServicePackageHealthReport(
        ApplicationName,
        ServiceManifestName,
        NodeName,
        new HealthInformation("ExternalSourceWatcher", "Connectivity", healthState));

    // TODO: handle exception. Code omitted for snippet brevity.
    // Possible exceptions: FabricException with error codes
    // FabricHealthStaleReport (non-retryable, hello report is already queued on hello health client),
    // FabricHealthMaxReportsReached (retryable; user should retry with exponential delay until hello report is accepted).
    Client.HealthManager.ReportHealth(deployedServicePackageHealthReport);
}
```

### <a name="powershell"></a>PowerShell
Skicka hälsorapporter med  **skicka ServiceFabric*EntityType*HealthReport **.

hello följande exempel visar periodiska rapportering om CPU-värden på en nod. hello rapporter ska skickas med 30 sekunders mellanrum, och de har en tid toolive på två minuter. Om de upphör att gälla har hello rapport problem, så hello nod utvärderas vid fel. När hello CPU överskrider ett tröskelvärde har hello rapporten ett hälsotillstånd varning. När hello CPU förblir överskrider ett tröskelvärde för mer än hello konfigurerats tid, rapporteras den som ett fel. Annars skickar hello rapport ett hälsotillstånd OK.

```powershell
PS C:\> Send-ServiceFabricNodeHealthReport -NodeName Node.1 -HealthState Warning -SourceId PowershellWatcher -HealthProperty CPU -Description "CPU is above 80% threshold" -TimeToLiveSec 120

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='CPU', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 5
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : CPU
                        HealthState           : Warning
                        SequenceNumber        : 130741236814913394
                        SentAt                : 4/21/2015 9:01:21 PM
                        ReceivedAt            : 4/21/2015 9:01:21 PM
                        TTL                   : 00:02:00
                        Description           : CPU is above 80% threshold
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:01:21 PM
```

hello rapporterar följande exempel en varning om tillfälligt på en replik. Den först hämtar hello partitions-ID och hello replik-ID för hello-tjänsten som den är intresserad av. Skickar sedan en rapport från **PowershellWatcher** på hello egenskapen **ResourceDependency**. hello rapporten är av intresse för bara två minuter och det bort automatiskt från hello store.

```powershell
PS C:\> $partitionId = (Get-ServiceFabricPartition -ServiceName fabric:/WordCount/WordCount.Service).PartitionId

PS C:\> $replicaId = (Get-ServiceFabricReplica -PartitionId $partitionId | where {$_.ReplicaRole -eq "Primary"}).ReplicaId

PS C:\> Send-ServiceFabricReplicaHealthReport -PartitionId $partitionId -ReplicaId $replicaId -HealthState Warning -SourceId PowershellWatcher -HealthProperty ResourceDependency -Description "hello external resource that hello primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes." -TimeToLiveSec 120 -RemoveWhenExpired

PS C:\> Get-ServiceFabricReplicaHealth  -PartitionId $partitionId -ReplicaOrInstanceId $replicaId


PartitionId           : 8f82daff-eb68-4fd9-b631-7a37629e08c0
ReplicaId             : 130740415594605869
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='ResourceDependency', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130740768777734943
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : ResourceDependency
                        HealthState           : Warning
                        SequenceNumber        : 130741243777723555
                        SentAt                : 4/21/2015 9:12:57 PM
                        ReceivedAt            : 4/21/2015 9:12:57 PM
                        TTL                   : 00:02:00
                        Description           : hello external resource that hello primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:12:32 PM
```

### <a name="rest"></a>REST
Skicka hälsorapporter med POST-förfrågningar som går toohello önskad entiteten och har i beskrivningen av rapporten hello brödtext hello hälsa REST. Till exempel visas hur toosend REST- [kluster hälsorapporter](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-cluster) eller [tjänsten hälsorapporter](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service). Alla enheter som stöds.

## <a name="next-steps"></a>Nästa steg
Baserat på hello hälsa data kan service-skrivare och kluster/application-administratörer Se sätt tooconsume hello information. De kan exempelvis ställa in varningar baserat på status toocatch allvarliga problem innan de leda till avbrott. Administratörer kan också ställa in reparera system toofix problem automatiskt.

[Introduktion tooService hälsotillståndet för Infrastrukturresurser övervakning](service-fabric-health-introduction.md)

[Visa Service Fabric-hälsorapporter](service-fabric-view-entities-aggregated-health.md)

[Hur tooreport och kontrollera-tjänsten för hälsotillstånd](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Använda systemet hälsorapporter för felsökning](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Övervaka och diagnostisera tjänster lokalt](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md)

