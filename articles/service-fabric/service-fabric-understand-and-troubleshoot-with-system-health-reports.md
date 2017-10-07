---
title: "aaaTroubleshoot med systemet hälsorapporter | Microsoft Docs"
description: "Beskriver hello hälsorapporter som skickas av Azure Service Fabric-komponenter och deras användning för felsökning kluster eller problem."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 52574ea7-eb37-47e0-a20a-101539177625
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: oanapl
ms.openlocfilehash: c77a6cdd0440ce5d354cd8760f40151f674a3529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-system-health-reports-tootroubleshoot"></a>Använd system hälsa rapporter tootroubleshoot
Azure Service Fabric-komponenter rapport out of box hello på alla entiteter i hello klustret. Hej [hälsoarkivet](service-fabric-health-introduction.md#health-store) skapar och tar bort enheter baserat på hello systemrapporter. Även ordnar dem i en hierarki som samlar in entiteten interaktioner.

> [!NOTE]
> toounderstand hälsorelaterade begrepp och Läs mer på [Service Fabric-hälsomodell](service-fabric-health-introduction.md).
> 
> 

System hälsorapporter ger inblick i klustret och programfunktioner och flaggan problem med hjälp av hälsotillstånd. System hälsorapporter Kontrollera att entiteter implementeras och fungerar korrekt från hello Service Fabric perspektiv för program och tjänster. hello-rapporter tillhandahåller inte någon hälsoövervakning av hello affärslogik hello service eller detekteringen av låsta processer. Användaren tjänster kan utöka hello hälsa data med information specifik tootheir logik.

> [!NOTE]
> Watchdogs hälsorapporter visas endast *när* hello systemkomponenter skapa en entitet. När du tar bort en entitet bort hello health store automatiskt alla hälsorapporter som associeras med den. hello samma sak gäller när en ny instans av entiteten hello skapas (till exempel skapas en ny tillståndskänslig beständiga replik tjänstinstans). Alla rapporter som är kopplat till hello gamla instansen tas bort och rensa från hello store.
> 
> 

hello system komponenten rapporter identifieras av hello källa som börjar med hello ”**System.**” prefix. Watchdogs kan inte använda samma prefix för deras källor hello som avvisas rapporter med ogiltiga parametrar.
Nu ska vi titta på vissa system rapporterar toounderstand vad utlöser dem och hur toocorrect hello möjliga problem med de representerar.

> [!NOTE]
> Service Fabric fortsätter tooadd rapporter på villkor intressanta som förbättrar inblick i vad som händer i hello kluster och program. Befintliga rapporter kan förbättras med mer information toohelp felsöka hello problem snabbare.
> 
> 

## <a name="cluster-system-health-reports"></a>Klustret system hälsorapporter
hello klustret hälsa entitet skapas automatiskt i hello health store. Om allt fungerar korrekt, har den inte en rapport.

### <a name="neighborhood-loss"></a>Förlust av nätverket
**System.Federation** rapporterar ett fel när den identifierar en förlust av nätverket. hello rapporten är från enskilda noder och hello nod-ID ingår i hello egenskapsnamn. Om en nätverket förlorat i hello hela Service Fabric ringen kan förvänta du vanligtvis två händelser (båda sidor av hello lucka rapporten). Om flera neighborhoods går förlorade, finns det flera händelser.

hello rapporten anger hello globala kontraktstidsgräns som hello tid toolive. hello rapporten skickas varje hälften av hello TTL-tiden för så länge villkoret hello förblir aktiv. hello händelse tas bort automatiskt när den upphör. Ta bort när utgångna beteendet garanteras att hello rapporten rensas från hello health store korrekt, även om hello reporting nod är nere.

* **SourceId**: System.Federation
* **Egenskapen**: börjar med **nätverket** och innehåller information om noden
* **Nästa steg**: undersöka varför hello nätverket går förlorad (till exempel kontrollera hello kommunikation mellan klusternoder).

## <a name="node-system-health-reports"></a>Noden system hälsorapporter
**System.FM**, som representerar hello Failover Manager service, är hello utfärdare som hanterar information om klusternoderna. Varje nod bör ha en rapport från System.FM som visar sitt tillstånd. hello noden enheter tas bort när hello nodens tillstånd tas bort (se [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).

### <a name="node-updown"></a>Noden upp/ned
System.FM rapporter som OK när hello nod ansluter till hello ringen (det är igång). Ett fel rapporteras när hello nod avgår hello ringen (den är nere, antingen för att uppgradera eller helt enkelt eftersom den inte kunde). hello hälsa hierarki som skapats av hello health store vidtar åtgärder på distribuerade entiteter i samband med System.FM noden rapporter. Betraktar hello nod entiteter för alla distribuerade virtuella överordnad. hello distribueras entiteter på noden är tillgängliga via frågor om hello nod rapporteras som upp av System.FM, med hello samma instans som hello-instans som är associerade med hello entiteter. När System.FM rapporterar hello noden är nere eller startas om (en ny instans), rensas hello hälsoarkivet automatiskt hello distribueras entiteter som kan finnas endast på hello ned noden eller på hello tidigare instans av hello-nod.

* **SourceId**: System.FM
* **Egenskapen**: tillstånd
* **Nästa steg**: om hello nod avser en uppgradering, det ska gå tillbaka när den har uppgraderats. I det här fallet bör hello hälsotillstånd växla tillbaka tooOK. Om hello noden inte gå tillbaka eller om det misslyckas måste hello problemet mer undersökning.

hello visas följande exempel hello System.FM händelse med ett hälsotillstånd OK för noden:

```powershell
PS C:\> Get-ServiceFabricNodeHealth  _Node_0

NodeName              : _Node_0
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 8
                        SentAt                : 7/14/2017 4:54:51 PM
                        ReceivedAt            : 7/14/2017 4:55:14 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```


### <a name="certificate-expiration"></a>Certifikatet upphör att gälla
**System.FabricNode** rapporterar en varning när certifikat som används av hello nod är nära förfallodatum. Det finns tre certifikat per nod: **Certificate_cluster**, **Certificate_server**, och **Certificate_default_client**. När hello förfallodatum är minst två veckor, är hälsotillståndet för hello rapporten OK. När hello upphör att gälla inom två veckor är hello rapporttypen en varning. TTL-värde på dessa händelser är oändligt och de tas bort när en nod lämnar hello klustret.

* **SourceId**: System.FabricNode
* **Egenskapen**: börjar med **certifikat** och innehåller mer information om hello certifikattyp
* **Nästa steg**: uppdatera hello certifikat om de är nära förfallodatum.

### <a name="load-capacity-violation"></a>Läsa in kapacitetsöverträdelse
hello Service Fabric-Belastningsutjämnarens rapporterar en varning när den identifierar en kapacitetsöverträdelse för noden.

* **SourceId**: System.PLB
* **Egenskapen**: börjar med **kapacitet**
* **Nästa steg**: Kontrollera tillhandahålls mått och visa hello aktuella kapaciteten på hello-nod.

## <a name="application-system-health-reports"></a>Programmet system hälsorapporter
**System.CM**, som representerar hello Cluster Manager-tjänsten är hello utfärdare som hanterar information om ett program.

### <a name="state"></a>Status
System.CM rapporterar som OK när programmet hello har skapats eller uppdaterats. Informerar hello hälsoarkivet när programmet hello har tagits bort, så att den kan tas bort från store.

* **SourceId**: System.CM
* **Egenskapen**: tillstånd
* **Nästa steg**: om programmet hello har skapats eller uppdaterats, ska det finnas hello Klusterhanterare hälsorapport. I annat fall söker hello tillståndet för hello-programmet genom att utfärda en fråga (till exempel hello PowerShell-cmdleten **ServiceFabricApplication Get - ApplicationName *applicationName***).

hello följande exempel visar hello händelsen på hello **fabric: / WordCount** program:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/14/2017 4:55:10 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="service-system-health-reports"></a>Tjänsten system hälsorapporter
**System.FM**, som representerar hello Failover Manager service, är hello utfärdare som hanterar information om tjänster.

### <a name="state"></a>Status
System.FM rapporterar som OK när hello-tjänsten har skapats. Det tar bort hello entitet från hello health store när hello-tjänsten har tagits bort.

* **SourceId**: System.FM
* **Egenskapen**: tillstånd

hello följande exempel visar hello händelsen hello tjänsten **fabric: / WordCount/WordCountWebService**:

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountWebService -ExcludeHealthStatistics


ServiceName           : fabric:/WordCount/WordCountWebService
AggregatedHealthState : Ok
PartitionHealthStates : 
                        PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
                        AggregatedHealthState : Ok
                        
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 14
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="service-correlation-error"></a>Fel i tjänsten korrelation
**System.PLB** rapporterar ett fel när den identifierar att uppdatera en tjänst toobe korrelerade med en annan tjänst skapas en tillhörighetskedja. hello rapporten rensas när lyckade uppdatering sker.

* **SourceId**: System.PLB
* **Egenskapen**: ServiceDescription
* **Nästa steg**: Kontrollera hello korrelerade beskrivningar.

## <a name="partition-system-health-reports"></a>Partitionen system hälsorapporter
**System.FM**, som representerar hello Failover Manager service, är hello utfärdare som hanterar information om tjänsten partitioner.

### <a name="state"></a>Status
System.FM rapporterar som OK när hello partition har skapats och är felfri. Det tar bort hello entitet från hello health store när hello partition tas bort.

Om hello partitionen är mindre än hello minsta antal, rapporterar ett fel. Om hello partitionen är inte mindre än hello minsta antal, men det är mindre än hello mål replik antal, rapporterar en varning. Om hello partitionen förlorar kvorum, rapporterar System.FM ett fel.

Andra viktiga händelser inkludera en varning när hello omkonfiguration tar längre tid än förväntat och när hello build tar längre tid än förväntat. hello förväntade tider för hello bygg- och omkonfiguration kan konfigureras baserat på tjänsten scenarier. Till exempel om en tjänst har en terabyte tillstånd, till exempel SQL-databas tar hello build längre tid än för en tjänst med en liten mängd tillstånd.

* **SourceId**: System.FM
* **Egenskapen**: tillstånd
* **Nästa steg**: om hello hälsotillstånd inte är OK, är det möjligt att vissa repliker inte har skapats, öppna eller upphöjt tooprimary eller sekundär korrekt. I många fall är hello orsaken ett service-fel i hello öppna eller ändra roll implementering.

hello som följande exempel visar en felfri partition:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountWebService | Get-ServiceFabricPartitionHealth -ExcludeHealthStatistics -ReplicasFilter None

PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 70
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

hello visar följande exempel hello hälsotillståndet för en partition som är lägre än målet replikantalet. hello nästa steg är tooget hello partition beskrivning, som visar hur den är konfigurerad: **MinReplicaSetSize** är tre och **TargetReplicaSetSize** sju. Hämta sedan hello antalet noder i klustret hello: fem. Så i det här fallet två repliker kan inte placeras eftersom hello mål antal repliker är högre än hello antalet noder som är tillgängliga.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None -ExcludeHealthStatistics


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 123
                        SentAt                : 7/14/2017 4:55:39 PM
                        ReceivedAt            : 7/14/2017 4:55:44 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/S RD _Node_2 Up 131444422260002646
                          N/S RD _Node_4 Up 131444422293113678
                          N/S RD _Node_3 Up 131444422293113679
                          N/S RD _Node_1 Up 131444422293118720
                          N/P RD _Node_0 Up 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5.)
                        
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:55:44 PM, LastOk = 1/1/0001 12:00:00 AM
                        
                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131445250939703027
                        SentAt                : 7/14/2017 4:58:13 PM
                        ReceivedAt            : 7/14/2017 4:58:14 PM
                        TTL                   : 00:01:05
                        Description           : hello Load Balancer was unable toofind a placement for one or more of hello Service's Replicas:
                        Secondary replica could not be placed due toohello following constraints and properties:  
                        TargetReplicaSetSize: 7
                        Placement Constraint: N/A
                        Parent Service: N/A
                        
                        Constraint Elimination Sequence:
                        Existing Secondary Replicas eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        Existing Primary Replica eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.
                        
                        Nodes Eliminated By Constraints:
                        
                        Existing Secondary Replicas -- Nodes with Partition's Existing Secondary Replicas/Instances:
                        --
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        
                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None
                        
                        
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:56:14 PM, LastOk = 1/1/0001 12:00:00 AM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | select MinReplicaSetSize,TargetReplicaSetSize

MinReplicaSetSize TargetReplicaSetSize
----------------- --------------------
                2                    7                        

PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a>Replik begränsningsfel
**System.PLB** rapporterar en varning om den identifierar en replik begränsningsfel och går inte att placera alla partition repliker. hello rapportinformationen visar vilka begränsningar och egenskaper förhindra hello replik placering.

* **SourceId**: System.PLB
* **Egenskapen**: börjar med **ReplicaConstraintViolation**

## <a name="replica-system-health-reports"></a>Replik system hälsorapporter
**System.RA**, som representerar hello omkonfiguration agent-komponenten är hello utfärdare för hello replikens tillstånd.

### <a name="state"></a>Status
**System.RA** rapporterar OK när hello repliken har skapats.

* **SourceId**: System.RA
* **Egenskapen**: tillstånd

hello som följande exempel visar en felfri replik:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422293118721
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131445248920273536
                        SentAt                : 7/14/2017 4:54:52 PM
                        ReceivedAt            : 7/14/2017 4:55:13 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_0
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:13 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="replica-open-status"></a>Replik öppen status
hello beskrivning av den här hälsorapport innehåller hello Starttid (Coordinated Universal Time) när hello API-anrop anropades.

**System.RA** rapporterar en varning om hello replik öppna tar längre tid än hello konfigurerats period (standard: 30 minuter). Om hello API påverkar tjänsttillgänglighet, utfärdas hello rapporten mycket snabbare (en konfigurerbar intervall, med en standard på 30 sekunder). hello tid, mätt innehåller hello tidsåtgång för hello replikatorn öppna och hello-tjänst som är öppna. hello egenskapen ändringar tooOK om hello öppnar har slutförts.

* **SourceId**: System.RA
* **Egenskapen**: **ReplicaOpenStatus**
* **Nästa steg**: om hello hälsotillstånd inte är OK, undersöka varför hello replik öppna tar längre tid än förväntat.

### <a name="slow-service-api-call"></a>Långsam service API-anrop
**System.RAP** och **System.Replicator** rapportera en varning om en anropet toohello service användarkod tar längre tid än hello konfigurerad tid. hello varning rensas när hello anropet har slutförts.

* **SourceId**: System.RAP eller System.Replicator
* **Egenskapen**: hello namnet på hello långsam API. hello beskrivningen innehåller mer information om hello tid hello API har väntande.
* **Nästa steg**: undersöka varför hello anropet tar längre tid än förväntat.

hello som följande exempel visar en partition i förlorar kvorum och hello undersökningen steg klar toofigure reda på varför. En av hello repliker har ett varningshälsotillstånd så att du får dess hälsa. Det visar att hello tjänståtgärd tar längre tid än väntat, en händelse som rapporterats av System.RAP. När informationen har tagits emot, hello nästa steg är toolook på hello service-koden och undersöka det. Det här fallet hello **RunAsync** implementering av hello tillståndskänslig service utlöser ett undantag. hello repliker omarbetning så att du inte kan se alla repliker i hello varningstillstånd. Du kan försök hämtning hello hälsotillstånd och söka efter skillnader i hello replik-ID. I vissa fall kan hello återförsök ge ledtrådar.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth -ExcludeHealthStatistics

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 3
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

När du startar hello felande program under hello felsökare visar hello diagnostiska händelser windows hello undantag utlöstes från RunAsync:

![Visual Studio 2015 diagnostiska händelser: RunAsync fel i fabric: / HelloWorldStatefulApplication.][1]

Visual Studio 2015 diagnostiska händelser: RunAsync fel i **fabric: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Replikeringskön är full
**System.Replicator** rapporterar en varning när hello Replikeringskön är full. På primära hello blir Replikeringskön vanligtvis full eftersom en eller flera sekundära repliker är långsam tooacknowledge åtgärder. På sekundära hello inträffar detta när hello-tjänsten är långsam tooapply hello åtgärder. hello varning rensas när hello kön är inte längre fullständig.

* **SourceId**: System.Replicator
* **Egenskapen**: **PrimaryReplicationQueueStatus** eller **SecondaryReplicationQueueStatus**, beroende på hello replikroll

### <a name="slow-naming-operations"></a>Långsam Naming åtgärder
**System.NamingService** rapporterar hälsotillståndet för dess primära repliken när en Naming åtgärden tar längre tid än godkända. Exempel på Naming operations är [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) eller [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync). Flera metoder kan hittas under FabricClient, till exempel [tjänsten metoder för hantering av](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) eller [metoder för hantering av egenskapen](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).

> [!NOTE]
> hello Naming service matchar namnen tooa plats i hello klustret och gör att användare toomanage Tjänstenamn och egenskaper. Det är ett Service Fabric partitionerad beständiga tjänsten. En av hello partitioner representerar hello Authority Owner som innehåller metadata om alla Service Fabric-namn och tjänster. hello Service Fabric-namn är mappade toodifferent partitioner, kallas Name Owner partitioner så hello-tjänsten kan utökas. Läs mer om [Naming service](service-fabric-architecture.md).
> 
> 

När en Naming åtgärden tar längre tid än förväntat hello åtgärden har flaggats med en varning-rapport på hello *primära repliken av hello Naming service partition som fungerar hello åtgärden*. Om hello-åtgärden har slutförts, är hello varning avmarkerad. Om hello-åtgärden har slutförts med ett fel, innehåller hello hälsa rapporten information om hello-fel.

* **SourceId**: System.NamingService
* **Egenskapen**: börjar med prefixet **Duration_** och identifierar hello går långsamt och hello Service Fabric-namnet på vilka hello åtgärden utförs. Till exempel om skapar tjänst med namnet fabric: / MyApp/MyService tar för lång tid, hello-egenskapen är Duration_AOCreateService.fabric:/MyApp/MyService. AO punkter toohello roll hello Naming partition för det här namnet och åtgärden.
* **Nästa steg**: Kontrollera varför hello Naming åtgärden misslyckas. Varje åtgärd kan ha olika rotorsaker. Till exempel ta bort tjänsten kan ha fastnat på en nod eftersom hello programvärd håller kraschar på en nod på grund av tooa användaren programfel i hello service-kod.

hello som följande exempel visar en åtgärd för att skapa tjänsten. hello-åtgärden tog längre tid än hello konfigurerats varaktighet. AO återförsök och skickar tooNO arbete. Inga senaste åtgärden slutförts hello med Timeout. I det här fallet är hello samma replik primär för både hello AO och inga roller.

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : hello AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : hello NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>DeployedApplication system hälsorapporter
**System.Hosting** är hello auktoritet på distribuerade entiteter.

### <a name="activation"></a>Aktivering
System.Hosting rapporterar som OK när ett program har aktiverats på hello-nod. Annars rapporterar ett fel.

* **SourceId**: System.Hosting
* **Egenskapen**: aktivering, inklusive hello distributionen version
* **Nästa steg**: om hello program inte är felfritt, undersöka varför hello-aktiveringen misslyckades.

hello visar följande exempel lyckad aktivering:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ExcludeHealthStatistics

ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_1
                                     AggregatedHealthState : Ok
                                     
HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131445249083836329
                                     SentAt                : 7/14/2017 4:55:08 PM
                                     ReceivedAt            : 7/14/2017 4:55:14 PM
                                     TTL                   : Infinite
                                     Description           : hello application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>Ladda ned
**System.Hosting** rapporterar ett fel om inte hello programmet paketet ned.

* **SourceId**: System.Hosting
* **Egenskapen**:  **hämta:*RolloutVersion***
* **Nästa steg**: undersöka varför hello hämtning misslyckades på hello-nod.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage system hälsorapporter
**System.Hosting** är hello auktoritet på distribuerade entiteter.

### <a name="service-package-activation"></a>Aktivering av Service-paket
System.Hosting rapporter som OK om hello service paketet aktiveringen på hello nod har genomförts. Annars rapporterar ett fel.

* **SourceId**: System.Hosting
* **Egenskapen**: aktivering
* **Nästa steg**: undersöka varför hello-aktiveringen misslyckades.

### <a name="code-package-activation"></a>Aktivering av kod paketet
**System.Hosting** rapporter som OK för varje kodpaketet om hello aktiveringen lyckas. Om hello aktiveringen misslyckas rapporterar en varning som konfigurerats. Om **CodePackage** misslyckas tooactivate eller avslutas med ett fel som är större än konfigurerad hello **CodePackageHealthErrorThreshold**, värd rapporterar ett fel. Om en tjänstepaketet innehåller flera kod paket, genereras en aktivering-rapporten för varje kriterium.

* **SourceId**: System.Hosting
* **Egenskapen**: använder hello prefixet **CodePackageActivation** och innehåller hello namn hello kodpaketet och hello startpunkt som  **CodePackageActivation:* CodePackageName*:*SetupEntryPoint/EntryPoint*** (till exempel **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Typ av registrering
**System.Hosting** rapporterar som OK om hello tjänsttypen har registrerats. Ett fel rapporteras om hello inte registrerats i tid (som konfigurerats med hjälp av **ServiceTypeRegistrationTimeout**). Om hello runtime är stängd hello service type har avregistrerats hello nod och värd rapporter en varning.

* **SourceId**: System.Hosting
* **Egenskapen**: använder hello prefixet **ServiceTypeRegistration** och innehåller hello tjänstnamnet typen (till exempel **ServiceTypeRegistration:FileStoreServiceType**)

hello som följande exempel visar en felfri distribuerat tjänstpaket:

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_1
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131445249084026346
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : hello ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131445249084306362
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : hello CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131445249088096842
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : hello ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>Ladda ned
**System.Hosting** rapporterar ett fel om inte hello service paketet ned.

* **SourceId**: System.Hosting
* **Egenskapen**:  **hämta:*RolloutVersion***
* **Nästa steg**: undersöka varför hello hämtning misslyckades på hello-nod.

### <a name="upgrade-validation"></a>Validering av uppgradering
**System.Hosting** rapporterar ett fel om valideringen under hello uppgraderingen misslyckas eller om hello uppgraderingen misslyckas på hello-nod.

* **SourceId**: System.Hosting
* **Egenskapen**: använder hello prefixet **FabricUpgradeValidation** och innehåller hello uppgraderingsversion
* **Beskrivning**: pekar toohello fel uppstod

## <a name="next-steps"></a>Nästa steg
[Visa Service Fabric-hälsorapporter](service-fabric-view-entities-aggregated-health.md)

[Hur tooreport och kontrollera-tjänsten för hälsotillstånd](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Övervaka och diagnostisera tjänster lokalt](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md)

