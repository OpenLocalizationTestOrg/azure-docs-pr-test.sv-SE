---
title: "Felsöka med systemet hälsorapporter | Microsoft Docs"
description: "Beskriver hälsorapporter som skickas av Azure Service Fabric-komponenter och deras användning för felsökning kluster eller problem."
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
ms.openlocfilehash: 54e20146b2f1e0ca6153b66319be70c6f7c2fb59
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="use-system-health-reports-to-troubleshoot"></a>Felsök med hjälp av systemhälsorapporter
Azure Service Fabric-komponenter rapporten direkt på alla entiteter i klustret. Den [hälsoarkivet](service-fabric-health-introduction.md#health-store) skapar och tar bort enheter baserat på systemrapporter. Även ordnar dem i en hierarki som samlar in entiteten interaktioner.

> [!NOTE]
> För att förstå hälsorelaterade begrepp, kan du läsa mer i [Service Fabric-hälsomodell](service-fabric-health-introduction.md).
> 
> 

System hälsorapporter ger inblick i klustret och programfunktioner och flaggan problem med hjälp av hälsotillstånd. För program och tjänster Kontrollera system hälsorapporter att entiteter implementeras och fungerar korrekt ur ett Service Fabric. Rapporterna ger inte några hälsoövervakning av affärslogiken för tjänsten eller detekteringen av låsta processer. Användaren tjänster kan utöka hälsa-data med information som är specifika för sin logik.

> [!NOTE]
> Watchdogs hälsorapporter visas endast *när* systemkomponenter skapa en entitet. När du tar bort en entitet bort health store automatiskt alla hälsorapporter som associeras med den. Detsamma gäller när en ny instans av entiteten skapas (till exempel skapas en ny tillståndskänslig beständiga replik tjänstinstans). Alla rapporter som är associerade med den gamla instansen tas bort och rensa från store.
> 
> 

Systemkomponenten rapporterar identifieras av källa som börjar med den ”**System.**” prefix. Watchdogs kan inte använda samma prefix för deras källor som avvisas rapporter med ogiltiga parametrar.
Nu ska vi titta på vissa systemrapporter att förstå vad utlöser dem och åtgärda eventuella problem som de representerar.

> [!NOTE]
> Service Fabric fortsätter att lägga till rapporter på villkor som förbättrar inblick i vad som händer i klustret och tillämpningen av intresse. Befintliga rapporter kan även förbättras med mer information för att felsöka problemet snabbare.
> 
> 

## <a name="cluster-system-health-reports"></a>Klustret system hälsorapporter
Entiteten hälsa kluster skapas automatiskt i health store. Om allt fungerar korrekt, har den inte en rapport.

### <a name="neighborhood-loss"></a>Förlust av nätverket
**System.Federation** rapporterar ett fel när den identifierar en förlust av nätverket. Rapporten är från enskilda noder och nod-ID ingår i namnet. Om en nätverket förlorat i hela Service Fabric ringen kan förvänta du vanligtvis två händelser (båda sidor av rapporten lucka). Om flera neighborhoods går förlorade, finns det flera händelser.

Rapporten anger globala kontraktstidsgräns som time to live. Rapporten skickas varje hälften av TTL-tiden för så länge villkoret förblir aktiv. Händelsen tas bort automatiskt när den upphör. Ta bort när utgångna beteendet garanteras att rapporten har rensats bort från health store korrekt, även om noden reporting är nere.

* **SourceId**: System.Federation
* **Egenskapen**: börjar med **nätverket** och innehåller information om noden
* **Nästa steg**: undersöka varför i nätverket går förlorad (till exempel Kontrollera kommunikationen mellan noder i klustret).

## <a name="node-system-health-reports"></a>Noden system hälsorapporter
**System.FM**, som representerar Failover Manager-tjänsten är utfärdaren som hanterar information om klusternoderna. Varje nod bör ha en rapport från System.FM som visar sitt tillstånd. Noden enheter tas bort när tillståndet noden tas bort (se [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync)).

### <a name="node-updown"></a>Noden upp/ned
System.FM rapporter som OK när noden ansluter ringen (det är igång). Ett fel rapporteras när noden avgår ringen (den är nere, antingen för att uppgradera eller helt enkelt eftersom den inte kunde). Hälsotillstånd hierarkin som skapats av health store vidtar åtgärder på distribuerade entiteter i samband med System.FM noden rapporter. Noden anser entiteter för alla distribuerade virtuella överordnad. Distribuerade enheterna på noden som är tillgängliga via frågor om noden rapporteras som drift av System.FM med samma instans som den instans som är associerade med entiteterna. När System.FM rapporterar att noden inte är igång eller startas om (en ny instans), rensas health store automatiskt de distribuerade entiteter som kan finnas endast på noden nedåt eller på den föregående versionen av noden.

* **SourceId**: System.FM
* **Egenskapen**: tillstånd
* **Nästa steg**: om noden avser en uppgradering, det ska gå tillbaka när den har uppgraderats. I det här fallet bör hälsotillståndet växla tillbaka till OK. Om noden inte gå tillbaka eller om det misslyckas måste problemet mer undersökning.

I följande exempel visas System.FM händelse med ett hälsotillstånd OK för noden:

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
**System.FabricNode** rapporterar en varning när certifikat som används av noden närmar sig förfallodatum. Det finns tre certifikat per nod: **Certificate_cluster**, **Certificate_server**, och **Certificate_default_client**. När upphör att gälla är minst två veckor, är hälsotillståndet för rapporten OK. När upphör att gälla inom två veckor är rapporttypen en varning. TTL-värde på dessa händelser är oändligt och de tas bort när en nod lämnar klustret.

* **SourceId**: System.FabricNode
* **Egenskapen**: börjar med **certifikat** och innehåller mer information om vilken certifikattyp
* **Nästa steg**: uppdatera certifikat om de är nära förfallodatum.

### <a name="load-capacity-violation"></a>Läsa in kapacitetsöverträdelse
Service Fabric-Belastningsutjämnarens rapporterar en varning när den identifierar en kapacitetsöverträdelse för noden.

* **SourceId**: System.PLB
* **Egenskapen**: börjar med **kapacitet**
* **Nästa steg**: Kontrollera att mått har angetts och visa den aktuella kapaciteten på noden.

## <a name="application-system-health-reports"></a>Programmet system hälsorapporter
**System.CM**, som representerar klustret Manager-tjänsten är utfärdaren som hanterar information om ett program.

### <a name="state"></a>Status
System.CM rapporterar som OK när programmet har skapats eller uppdaterats. Informerar arkivet hälsotillstånd när programmet har tagits bort, så att den kan tas bort från store.

* **SourceId**: System.CM
* **Egenskapen**: tillstånd
* **Nästa steg**: om programmet har skapats eller uppdaterats, ska det finnas hälsorapport Klusterhanteraren. I annat fall söker tillståndet för programmet genom att utfärda en fråga (till exempel PowerShell-cmdleten **ServiceFabricApplication Get - ApplicationName *applicationName***).

I följande exempel visas händelsen på den **fabric: / WordCount** program:

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
**System.FM**, som representerar Failover Manager-tjänsten är auktoritet som hanterar information om tjänster.

### <a name="state"></a>Status
System.FM rapporter som OK när tjänsten har skapats. Det tar bort enheten från health store när tjänsten har tagits bort.

* **SourceId**: System.FM
* **Egenskapen**: tillstånd

I följande exempel visas händelsen på tjänsten **fabric: / WordCount/WordCountWebService**:

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
**System.PLB** rapporterar ett fel när den identifierar att uppdatera en tjänst kan korreleras med en annan tjänst skapas en tillhörighetskedja. Rapporten är avmarkerad när uppdateringen sker.

* **SourceId**: System.PLB
* **Egenskapen**: ServiceDescription
* **Nästa steg**: Kontrollera korrelerade service beskrivningar.

## <a name="partition-system-health-reports"></a>Partitionen system hälsorapporter
**System.FM**, som representerar Failover Manager-tjänsten är utfärdaren som hanterar information om tjänsten partitioner.

### <a name="state"></a>Status
System.FM rapporterar som OK när partitionen har skapats och är felfri. Det tar bort enheten från health store när partitionen tas bort.

Om partitionen är mindre än det minsta antalet, rapporterar ett fel. Om partitionen är inte mindre än det minsta antalet, men den är under målet replikantalet, rapporterar en varning. Om partitionen förlorar kvorum, rapporterar System.FM ett fel.

Andra viktiga händelser inkludera en varning när omkonfiguration tar längre tid än förväntat och när bygga tar längre tid än förväntat. Den förväntade gånger för att bygga och omkonfiguration kan konfigureras baserat på tjänsten scenarier. Till exempel om en tjänst har en terabyte tillstånd, till exempel SQL-databas tar bygga längre tid än för en tjänst med en liten mängd tillstånd.

* **SourceId**: System.FM
* **Egenskapen**: tillstånd
* **Nästa steg**: om hälsotillståndet inte är OK, är det möjligt att repliker inte har skapats, öppnas, eller befordras till primär eller sekundär korrekt. I många fall är den grundläggande orsaken ett service-fel i implementeringen öppna eller ändra roll.

I följande exempel visas en felfri partition:

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

I följande exempel visar hälsotillståndet för en partition som är lägre än målet replikantalet. Nästa steg är att hämta partitionen beskrivning, som visar hur den är konfigurerad: **MinReplicaSetSize** är tre och **TargetReplicaSetSize** sju. Hämta sedan antalet noder i klustret: fem. Så i det här fallet två repliker kan inte placeras eftersom målet antal repliker är högre än antalet noder som är tillgängliga.

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
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        Secondary replica could not be placed due to the following constraints and properties:  
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
**System.PLB** rapporterar en varning om den identifierar en replik begränsningsfel och går inte att placera alla partition repliker. Rapportdetaljer visar vilka villkor och egenskaper förhindra replik-placering.

* **SourceId**: System.PLB
* **Egenskapen**: börjar med **ReplicaConstraintViolation**

## <a name="replica-system-health-reports"></a>Replik system hälsorapporter
**System.RA**, som representerar komponenten omkonfiguration agent är auktoritet för replik-tillstånd.

### <a name="state"></a>Status
**System.RA** rapporterar OK när repliken har skapats.

* **SourceId**: System.RA
* **Egenskapen**: tillstånd

I följande exempel visas en felfri replik:

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
Beskrivning av den här hälsorapport innehåller starttiden (Coordinated Universal Time) när API-anrop anropades.

**System.RA** rapporterar en varning om repliken öppna tar längre tid än den konfigurerade tidsperioden (standard: 30 minuter). Om API: et påverkar tjänsttillgänglighet, utfärdas rapporten mycket snabbare (en konfigurerbar intervall, med en standard på 30 sekunder). Tiden mäts innehåller den tid det tar för replikatorn öppen och öppna tjänsten. Egenskapen ändras till OK om öppna har slutförts.

* **SourceId**: System.RA
* **Egenskapen**: **ReplicaOpenStatus**
* **Nästa steg**: om hälsotillståndet inte är OK, undersöka varför repliken öppna tar längre tid än förväntat.

### <a name="slow-service-api-call"></a>Långsam service API-anrop
**System.RAP** och **System.Replicator** rapportera en varning om ett anrop till användarkod för tjänsten tar längre tid än den konfigurerade tidpunkten. Varningen rensas när anropet har slutförts.

* **SourceId**: System.RAP eller System.Replicator
* **Egenskapen**: namnet på den långsamma API. Beskrivningen ger mer information om den tid som API: et har väntande.
* **Nästa steg**: undersöka varför anropet tar längre tid än förväntat.

I följande exempel visas en partition i förlorar kvorum och undersökning stegen för att ta reda på varför. En av replikerna har ett varningshälsotillstånd så att du får dess hälsa. Det visar att en åtgärd i tjänsten tar längre tid än väntat, en händelse som rapporterats av System.RAP. När informationen har tagits emot, är nästa steg att titta på kod och undersöka det. För det här fallet den **RunAsync** implementeringen av tjänsten tillståndskänslig utlöser ett undantag. Replikerna omarbetning så att du inte kan se alla repliker i varningstillstånd. Du kan försöka komma hälsotillståndet och söka efter skillnader i replik-ID. I vissa fall kan återförsöken ge ledtrådar.

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

När du startar det felande programmet under felsökaren kan visa diagnostik händelser windows undantag utlöstes från RunAsync:

![Visual Studio 2015 diagnostiska händelser: RunAsync fel i fabric: / HelloWorldStatefulApplication.][1]

Visual Studio 2015 diagnostiska händelser: RunAsync fel i **fabric: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Replikeringskön är full
**System.Replicator** rapporterar en varning när Replikeringskön är full. På primärt blir Replikeringskön vanligtvis full eftersom en eller flera sekundära repliker tar lång tid att godkänna åtgärder. På sekundärt inträffar detta när tjänsten går långsamt att tillämpa åtgärderna. Varningen rensas när kön är inte längre fullständig.

* **SourceId**: System.Replicator
* **Egenskapen**: **PrimaryReplicationQueueStatus** eller **SecondaryReplicationQueueStatus**, beroende på rollen replik

### <a name="slow-naming-operations"></a>Långsam Naming åtgärder
**System.NamingService** rapporterar hälsotillståndet för dess primära repliken när en Naming åtgärden tar längre tid än godkända. Exempel på Naming operations är [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) eller [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync). Flera metoder kan hittas under FabricClient, till exempel [tjänsten metoder för hantering av](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) eller [metoder för hantering av egenskapen](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient).

> [!NOTE]
> Tjänsten Naming matchar tjänstnamn till en plats i klustret och gör att användarna kan hantera tjänstnamn och egenskaper. Det är ett Service Fabric partitionerad beständiga tjänsten. En av partitionerna representerar Authority Owner som innehåller metadata om alla Service Fabric-namn och tjänster. Service Fabric-namnen mappas till olika partitioner, kallas Name Owner partitioner, så att tjänsten kan utökas. Läs mer om [Naming service](service-fabric-architecture.md).
> 
> 

När en Naming åtgärden tar längre tid än förväntat åtgärden har flaggats med en varning-rapport på den *primära repliken för Naming service-partition som fungerar igen*. Om åtgärden slutförs är varningen avmarkerad. Om åtgärden har slutförts med ett fel, innehåller hälsa rapporten information om felet.

* **SourceId**: System.NamingService
* **Egenskapen**: börjar med prefixet **Duration_** och identifierar långsam igen och Service Fabric-namnet som åtgärden utförs. Till exempel om skapar tjänst med namnet fabric: / MyApp/MyService tar för lång tid, egenskapen är Duration_AOCreateService.fabric:/MyApp/MyService. AO pekar på rollen för Naming partition för det här namnet och åtgärden.
* **Nästa steg**: Kontrollera orsaken Naming åtgärden misslyckas. Varje åtgärd kan ha olika rotorsaker. Till exempel ta bort tjänsten kan ha fastnat på en nod eftersom programvärden håller kraschar på en nod på grund av ett programfel för användare i kod.

I följande exempel visas en Skapa-åtgärd i tjänsten. Åtgärden tog längre tid än den konfigurerade varaktigheten. AO återförsök och skickar arbetsobjekt till Nej. INTE slutföra den senaste åtgärden med Timeout. I det här fallet är samma replik primär för både AO och inga roller.

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
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
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
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>DeployedApplication system hälsorapporter
**System.Hosting** är auktoritet på distribuerade entiteter.

### <a name="activation"></a>Aktivering
System.Hosting rapporterar som OK när ett program har aktiverats på noden. Annars rapporterar ett fel.

* **SourceId**: System.Hosting
* **Egenskapen**: aktivering, inklusive distribution-version
* **Nästa steg**: om programmet är i feltillstånd, undersöka varför aktiveringen misslyckades.

I följande exempel visar lyckad aktivering:

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
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>Ladda ned
**System.Hosting** rapporterar ett fel om inte paketet ned programmet.

* **SourceId**: System.Hosting
* **Egenskapen**:  **hämta:*RolloutVersion***
* **Nästa steg**: undersöka varför hämtningen misslyckades på noden.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage system hälsorapporter
**System.Hosting** är auktoritet på distribuerade entiteter.

### <a name="service-package-activation"></a>Aktivering av Service-paket
System.Hosting rapporter som OK om tjänsten paketet aktiveringen på noden har genomförts. Annars rapporterar ett fel.

* **SourceId**: System.Hosting
* **Egenskapen**: aktivering
* **Nästa steg**: undersöka varför aktiveringen misslyckades.

### <a name="code-package-activation"></a>Aktivering av kod paketet
**System.Hosting** rapporter som OK för varje kodpaketet om aktiveringen lyckas. Om aktiveringen misslyckas rapporterar en varning som konfigurerats. Om **CodePackage** inte kan aktivera eller avslutas med ett fel som är större än den konfigurerade **CodePackageHealthErrorThreshold**, värd rapporterar ett fel. Om en tjänstepaketet innehåller flera kod paket, genereras en aktivering-rapporten för varje kriterium.

* **SourceId**: System.Hosting
* **Egenskapen**: namnområdesprefixet **CodePackageActivation** och innehåller namnet på kodpaketet och startpunkt som  **CodePackageActivation:*CodePackageName* :*SetupEntryPoint/EntryPoint*** (till exempel **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Typ av registrering
**System.Hosting** rapporterar som OK om tjänsttypen har registrerats. Ett fel rapporteras om den inte registrerats i tid (som konfigurerats med hjälp av **ServiceTypeRegistrationTimeout**). Om körningen är stängt, service-typen är avregistrera från noden och värd rapporterar en varning.

* **SourceId**: System.Hosting
* **Egenskapen**: namnområdesprefixet **ServiceTypeRegistration** och innehåller namnet på tjänsten (till exempel **ServiceTypeRegistration:FileStoreServiceType**)

I följande exempel visas en felfri distribuerat tjänstpaket:

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
                             Description           : The ServicePackage was activated successfully.
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
                             Description           : The CodePackage was activated successfully.
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
                             Description           : The ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>Ladda ned
**System.Hosting** rapporterar ett fel om inte paketet ned service.

* **SourceId**: System.Hosting
* **Egenskapen**:  **hämta:*RolloutVersion***
* **Nästa steg**: undersöka varför hämtningen misslyckades på noden.

### <a name="upgrade-validation"></a>Validering av uppgradering
**System.Hosting** rapporterar ett fel om valideringen under uppgraderingen misslyckas eller om uppgraderingen misslyckas på noden.

* **SourceId**: System.Hosting
* **Egenskapen**: namnområdesprefixet **FabricUpgradeValidation** och innehåller den uppgradera versionen
* **Beskrivning**: pekar på fel

## <a name="next-steps"></a>Nästa steg
[Visa Service Fabric-hälsorapporter](service-fabric-view-entities-aggregated-health.md)

[Hur du rapporten och kontrollera tjänstens hälsa](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Övervaka och diagnostisera tjänster lokalt](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md)

