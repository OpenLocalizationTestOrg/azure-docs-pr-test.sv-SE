---
title: "aaaHow tooview Azure Service Fabric entiteternas samman hälsa | Microsoft Docs"
description: "Beskriver hur tooquery, visa och utvärdera Azure Service Fabric entiteter aggregerade hälsa genom hälsoförfrågningar och allmänna frågor."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: fa34c52d-3a74-4b90-b045-ad67afa43fe5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: oanapl
ms.openlocfilehash: add810551cac26d2b4ff81b57d94ddd780c2cc2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-service-fabric-health-reports"></a>Visa Service Fabric-hälsorapporter
Azure Service Fabric introducerar en [hälsomodell](service-fabric-health-introduction.md) med hälsa entiteter där systemkomponenter och watchdogs kan rapporten lokala villkor som de övervakar. Hej [hälsoarkivet](service-fabric-health-introduction.md#health-store) aggregerar alla hälsa data toodetermine om enheter är felfria.

hello klustret fylls automatiskt med hälsorapporter som skickas av hello systemkomponenter. Läs mer på [Använd systemhälsa rapporterar tootroubleshoot](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Service Fabric innehåller flera olika sätt tooget hello samman hälsotillståndet för hello enheter:

* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) eller andra visualiseringsverktyg för
* Hälsoförfrågningar (via PowerShell API eller REST)
* Allmänna frågor som returnerar en lista över enheter som har hälsa som en av hello-egenskaper (via PowerShell API eller REST)

toodemonstrate dessa alternativ, vi använder en lokal kluster med fem noder och hello [fabric: / WordCount programmet](http://aka.ms/servicefabric-wordcountapp). Hej **fabric: / WordCount** program innehåller två standardtjänster är en tillståndskänslig tjänst av typen `WordCountServiceType`, och en tillståndslös tjänst av typen `WordCountWebServiceType`. Du har ändrat hello `ApplicationManifest.xml` toorequire sju mål repliker för hello tillståndskänslig service och en partition. Eftersom det finns bara fem noder i klustret hello, rapport hello systemkomponenter en varning om hello service partitionen eftersom den är under hello målantalet.

```xml
<Service Name="WordCountService">
<<<<<<< HEAD
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="3">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
=======
  <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
    <UniformInt64Partition PartitionCount="[WordCountService_PartitionCount]" LowKey="1" HighKey="26" />
  </StatefulService>
>>>>>>> 5e84dbdd8e45a5d6b36f435a550b7433b873bf11
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Hälsa i Service Fabric Explorer
Service Fabric Explorer ger en visuell översikt över hello klustret. Du kan se som i hello bilden nedan:

* Hej programmet **fabric: / WordCount** är röd (fel) eftersom den har en felhändelse som rapporterats av **MyWatchdog** för hello egenskapen **tillgänglighet**.
* En av dess tjänster, **fabric: / WordCount/WordCountService** är gul (i varning). hello-tjänsten har konfigurerats med sju repliker och hello klustret har fem noder så att två repicas inte kan placeras. Även om det inte visas här, hello-tjänsten är gult på grund av en rapport från `System.FM` säger som `Partition is below target replica or instance count`. hello gul partition utlösare hello gul service.
* hello kluster är red på grund av programmet hello röd.

hello utvärdering använder standardprinciper från hello klustermanifestet och programmanifestet. De strikta principer och tolererar inte att ett fel.

Vy över hello kluster med Service Fabric Explorer:

![Vy över hello kluster med Service Fabric Explorer.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [!NOTE]
> Läs mer om [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
>
>

## <a name="health-queries"></a>Hälsoförfrågningar
Service Fabric exponerar hälsoförfrågningar för varje hello stöds [entitetstyper](service-fabric-health-introduction.md#health-entities-and-hierarchy). De kan nås via hello API, med metoder på [FabricClient.HealthManager](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet), PowerShell-cmdletar och REST. De här frågorna returnera hela hälsotillståndet information om hello entitet: hello samman hälsotillstånd, entiteten hälsa händelser, underordnade hälsotillstånd (i förekommande fall), ohälsosamt utvärderingar (när hello entiteten inte är felfri) och underordnade hälsostatistik (när tillämpligt).

> [!NOTE]
> En entitet hälsa returneras när den är helt fylld i hello health store. hello enheten måste vara aktiva (inte tas bort) och har en system-rapport. Dess överordnade entiteter på hello hierarkin kedja måste också ha systemrapporter. Om någon av dessa villkor inte är uppfyllda hello hälsa frågar returnera en [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) med [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) `FabricHealthEntityNotFound` som visar varför hello entitet returneras inte.
>
>

Hej hälsoförfrågningar måste klara i hello entitet identifierare som är beroende av hello entitetstypen. hello frågor accepterar valfria hälsa principparametrar. Om inga hälsoprinciper anges hello [hälsoprinciper](service-fabric-health-introduction.md#health-policies) från hello klustret eller programmanifestet används för utvärdering. Om hello manifesten får inte innehålla en definition för hälsoprinciper, som hello standard hälsoprinciper används för utvärdering. hello standard hälsoprinciper inte tolererar att ett fel. hello frågor även godta filter för att returnera endast delvis underordnade objekt eller händelser--hello dem som respektera hello angivna filtren har använts. Ett annat filter kan exklusive hello underordnade statistik.

> [!NOTE]
> hello utdatafilter tillämpas på hello serversidan så hello meddelandestorlek svar minskas. Vi rekommenderar att du använder hello utdatafilter toolimit hello data returneras i stället använda filter i hello på klientsidan.
>
>

En entitets hälsotillstånd innehåller:

* hello samman hälsotillståndet för hello entitet. Beräknad av hello health store baserat på entiteten hälsorapporter, underordnade hälsotillstånd (i förekommande fall) och hälsoprinciper. Läs mer om [entiteten hälsoutvärderingen](service-fabric-health-introduction.md#health-evaluation).  
* hello hälsa händelser på hello entitet.
* hello insamling av hälsotillstånd för alla underordnade för hello entiteter som kan ha underordnade. hello hälsotillstånd innehåller entitet identifierare och hello aggregerade hälsotillstånd. tooget hela hälsotillståndet för ett barn anropa hello frågan hälsa för hello underordnade entitetstypen och ange hello underordnade identifierare.
* hello ohälsosamt utvärderingar den punkt toohello rapportera som utlöste hello tillståndet för hello entiteten om hello entiteten inte är felfri. hello-utvärderingar är rekursiv, som innehåller hello underordnade hälsa utvärderingar som utlöste aktuellt hälsotillstånd. Till exempel rapporterade en watchdog ett fel mot en replik. Hej programhälsan visar en ohälsosamt utvärdering på grund av feltillstånd tooan-tjänst. hello-tjänsten är i feltillstånd på grund av tooa partition i fel. hello partitionen är i feltillstånd på grund av tooa replik i fel. hello replik är felfri på grund av hälsorapport för toohello watchdog fel.
* hello hälsostatistik för alla underordnade typer av hello entiteter som har underordnade. Kluster visar hello Totalt antal program, tjänster, partitioner, repliker och distribuerats entiteter i hello klustret. Tjänstens hälsa visar hello totala antalet partitioner och repliker under hello angivna tjänsten.

## <a name="get-cluster-health"></a>Hämta kluster
Returnerar hello hälsotillstånd hello klustret entiteten och innehåller hello hälsotillstånd för program och noder (underordnade hello kluster). Indata:

* [Valfritt] hello klustret hälsoprincip för tooevaluate hello noder och hello klusterhändelser.
* [Valfritt] hello hälsa princip programavbildningen, används hello hälsoprinciper toooverride hello application manifest principer.
* [Valfritt] Filter för händelser, noder och program som anger vilka poster som är intressanta och returneras i hello resultatet (till exempel fel, eller både varningar och fel). Alla händelser, noder och program är används tooevaluate hello samman entitetshälsa, oavsett hello filter.
* [Valfritt] Filtrera tooexclude hälsostatistik.
* [Valfritt] Filtrera tooinclude fabric: / System hälsostatistik i hello hälsostatistik. Gäller endast när hello hälsostatistik inte utesluts. Som standard innehåller hello hälsostatistik bara statistik för inte hello System-program och program.

### <a name="api"></a>API
tooget hälsa bör du skapa en `FabricClient` och anrop hello [GetClusterHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthasync) metod på dess **HealthManager**.

hello hämtar följande anrop hello klustret hälsotillstånd:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

hello följande kod hämtar hello klustret hälsa genom att använda en anpassad klustret hälsoprincip och filtrerar för noder och program. Det anger att hello hälsostatistik inkluderar hello fabric: / statistik för filsystemet. Den skapar [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthquerydescription), som innehåller information om hello.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var healthStatisticsFilter = new ClusterHealthStatisticsFilter()
{
    ExcludeHealthStatistics = false,
    IncludeSystemApplicationHealthStatistics = true
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
    HealthStatisticsFilter = healthStatisticsFilter
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
hello cmdlet tooget hello klustret hälsa är [Get-ServiceFabricClusterHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealth). Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

hello hello klustret är fem noder och hello systemprogram fabric: / WordCount konfigurerats enligt beskrivningen.

hello följande cmdlet hämtar kluster med hjälp av standard hälsoprinciper. hello aggregerade hälsotillstånd är en varning, eftersom hello fabric: / WordCount-program finns i varningen. Observera hur hello ohälsosamt utvärderingar tillhandahåller information om hello villkor som utlöste hello samman hälsa.

```xml
PS D:\ServiceFabric> Get-ServiceFabricClusterHealth


AggregatedHealthState   : Warning
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.
                          
                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.
                          
                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                          
                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.
                          
                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                          
                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                          
                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                          
                          
NodeHealthStates        : 
                          NodeName              : _Node_4
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_3
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_1
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_0
                          AggregatedHealthState : Ok
                          
ApplicationHealthStates : 
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok
                          
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning
                          
HealthEvents            : None
HealthStatistics        : 
                          Node                  : 5 Ok, 0 Warning, 0 Error
                          Replica               : 6 Ok, 0 Warning, 0 Error
                          Partition             : 1 Ok, 1 Warning, 0 Error
                          Service               : 1 Ok, 1 Warning, 0 Error
                          DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                          DeployedApplication   : 5 Ok, 0 Warning, 0 Error
                          Application           : 0 Ok, 1 Warning, 0 Error
```

hello hämtar följande PowerShell-cmdlet hello hälsotillstånd hello kluster med hjälp av en princip för anpassade program. Den filtrerar resultaten tooget endast program och noderna i felet eller varningen. Därför kan returneras inga noder, som de är alla felfritt. Endast hello fabric: / WordCount programmet respekterar hello program filter. Eftersom hello anpassad princip anger tooconsider varningar som fel för hello fabric: / WordCount programmet hello programmet utvärderas som fel och är därför hello klustret.

```powershell
PS D:\ServiceFabric> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error" -ExcludeHealthStatistics


AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.
                          
                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.
                          
                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                          
                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                          
                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                          
                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                          
                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.
                          
                          
NodeHealthStates        : None
ApplicationHealthStates : 
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error
                          
HealthEvents            : None
```

### <a name="rest"></a>REST
Du kan hämta klustret hälsotillstånd med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-by-using-a-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.

## <a name="get-node-health"></a>Hämta nod hälsa
Returnerar hello hälsotillståndet för en nod entitet och innehåller hello hälsa händelser som rapporterats för hello nod. Indata:

* [Krävs] hello nodnamn som identifierar hello-nod.
* [Valfritt] hello klustret hälsa principinställningar används tooevaluate hälsa.
* [Valfritt] Filter för händelser som anger vilka poster som är intressanta och returneras i hello resultatet (till exempel fel, eller både varningar och fel). Alla händelser har använt tooevaluate hello samman entitetshälsa, oavsett hello filter.

### <a name="api"></a>API
tooget noden hälsa genom hello API, skapa en `FabricClient` och anrop hello [GetNodeHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getnodehealthasync) metod på dess HealthManager.

hello hämtar följande kod hello nod hälsan för hello angivna nodnamn:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

hello följande kod hämtar hello nod hälsa för hello angivna nodnamnet och överför i händelsefilter och anpassad princip via [NodeHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.nodehealthquerydescription):

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
hello cmdlet tooget hello nod hälsa är [Get-ServiceFabricNodeHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnodehealth). Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.
hello följande cmdlet hämtar hello nod hälsa genom att använda standard hälsoprinciper:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 7/13/2017 4:39:23 PM
                        ReceivedAt            : 7/13/2017 4:40:47 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 4:40:47 PM, LastWarning = 1/1/0001 12:00:00 AM
```

hello hämtar följande cmdlet hello hälsotillståndet för alla noder i klustret hello:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_4                     Ok
_Node_3                     Ok
_Node_2                     Ok
_Node_1                     Ok
_Node_0                     Ok
```

### <a name="rest"></a>REST
Du kan hämta nod hälsotillstånd med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node-by-using-a-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.

## <a name="get-application-health"></a>Hämta programmets hälsotillstånd
Returnerar hello hälsa för en entitet i programmet. Den innehåller hello hälsotillstånd hello distribuerade program och tjänsten underordnade. Indata:

* [Krävs] hello programnamnet (URI) som identifierar hello program.
* [Valfritt] hello programmets hälsoprincip används toooverride hello application manifest principer.
* [Valfritt] Filter för händelser, tjänster och distribuerade program som anger vilka poster returneras i hello resultatet (till exempel fel, eller både varningar och fel) är av intresse. Alla händelser, tjänster och distribuerade program kan du använda tooevaluate hello samman entitetshälsa, oavsett hello filter.
* [Valfritt] Filtrera tooexclude hello hälsostatistik. Om inget anges hello hälsostatistik inkluderar hello ok, varning och antal fel för alla program barn: tjänster, partitioner, repliker, distribuerade program och distribueras servicepaket.

### <a name="api"></a>API
tooget programmets hälsotillstånd, skapa en `FabricClient` och anrop hello [GetApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getapplicationhealthasync) metod på dess HealthManager.

hello hämtar följande kod hello programhälsan hello angivna programnamnet (URI):

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

hello följande kod hämtar hello programhälsan hello angivna programnamnet (URI) med filter och anpassade principer som angetts via [ApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.applicationhealthquerydescription).

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
hello cmdlet tooget hello programhälsan är [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps). Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

hello följande cmdlet returnerar hello hälsotillstånd hello **fabric: / WordCount** program:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                                  
                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                                  
ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok
                                  
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning
                                  
DeployedApplicationHealthStates : 
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok
                                  
HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/13/2017 5:57:05 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM
                                  
HealthStatistics                : 
                                  Replica               : 6 Ok, 0 Warning, 0 Error
                                  Partition             : 1 Ok, 1 Warning, 0 Error
                                  Service               : 1 Ok, 1 Warning, 0 Error
                                  DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                                  DeployedApplication   : 5 Ok, 0 Warning, 0 Error
```

Hej följande PowerShell-cmdlet överför i anpassade principer. Den filtrerar underordnade och händelser.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                                  
                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.
                                  
ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error
                                  
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>REST
Du kan hämta programmets hälsotillstånd med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application-by-using-an-application-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.

## <a name="get-service-health"></a>Hämta tjänstens hälsa
Returnerar hello hälsotillståndet för en tjänst-enhet. Den innehåller hello partition hälsotillstånd. Indata:

* [Krävs] hello tjänstnamn (URI) som identifierar hello-tjänst.
* [Valfritt] hello programmets hälsoprincip används toooverride hello application manifest princip.
* [Valfritt] Filter för händelser och partitioner som anger vilka poster returneras i hello resultatet (till exempel fel, eller både varningar och fel) är av intresse. Alla händelser och partitioner är används tooevaluate hello samman entitetshälsa, oavsett hello filter.
* [Valfritt] Filtrera tooexclude hälsostatistik. Om inget anges Hej hälsa statistik visar hello ok, varning och fel antal för alla partitioner och repliker av hello-tjänsten.

### <a name="api"></a>API
tjänstens hälsa för tooget via hello API, skapa en `FabricClient` och anrop hello [GetServiceHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getservicehealthasync) metod på dess HealthManager.

hello hämtar följande exempel hello hälsotillståndet för en tjänst med angivet tjänstnamn (URI):

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

hello följande kod hämtar hello tjänstens hälsa för hello angivet tjänstnamn (URI), ange filter och anpassade principer via [ServiceHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.servicehealthquerydescription):

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
tjänstens hälsa för hello cmdlet tooget hello är [Get-ServiceFabricServiceHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicehealth). Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

hello följande cmdlet hämtar hello tjänstens hälsa med hjälp av standard hälsoprinciper:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                        
                        Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                        
                            Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
PartitionHealthStates : 
                        PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                        AggregatedHealthState : Warning
                        
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 15
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                        
HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
                        Partition             : 0 Ok, 1 Warning, 0 Error
```

### <a name="rest"></a>REST
Du kan hämta tjänstens hälsa med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-by-using-a-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.

## <a name="get-partition-health"></a>Hämta partitionen hälsa
Returnerar hello hälsotillståndet för en partition entitet. Den innehåller hello replik hälsotillstånd. Indata:

* [Krävs] hello partition ID (GUID) som identifierar hello partition.
* [Valfritt] hello programmets hälsoprincip används toooverride hello application manifest princip.
* [Valfritt] Filter för händelser och repliker som anger vilka poster returneras i hello resultatet (till exempel fel, eller både varningar och fel) är av intresse. Alla händelser och repliker är används tooevaluate hello samman entitetshälsa, oavsett hello filter.
* [Valfritt] Filtrera tooexclude hälsostatistik. Om inget annat anges, visar hello hälsostatistik hur många repliker är i ok, varning och fel tillstånd.

### <a name="api"></a>API
tooget partition hälsa genom hello API, skapa en `FabricClient` och anrop hello [GetPartitionHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getpartitionhealthasync) metod på dess HealthManager. toospecify valfria parametrar, skapa [PartitionHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.partitionhealthquerydescription).

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShell
hello cmdlet tooget hello partition hälsa är [Get-ServiceFabricPartitionHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricpartitionhealth). Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

hello följande cmdlet hämtar hello hälsotillstånd för alla partitioner i hello **fabric: / WordCount/WordCountService** tjänsten och filtrerar bort repliken hälsotillstånd:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 72
                        SentAt                : 7/13/2017 5:57:29 PM
                        ReceivedAt            : 7/13/2017 5:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/P RD _Node_2 Up 131444422260002646
                          N/S RD _Node_4 Up 131444422293113678
                          N/S RD _Node_3 Up 131444422293113679
                          N/S RD _Node_1 Up 131444422293118720
                          N/S RD _Node_0 Up 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5.)
                        
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 7/13/2017 5:57:48 PM, LastError = 1/1/0001 12:00:00 AM
                        
                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131444445174851664
                        SentAt                : 7/13/2017 6:35:17 PM
                        ReceivedAt            : 7/13/2017 6:35:18 PM
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
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None
                        
                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None
                        
                        
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/13/2017 5:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
                        
HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a>REST
Du kan hämta partitionen hälsotillstånd med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition-by-using-a-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.

## <a name="get-replica-health"></a>Hämta replik hälsa
Returnerar hello hälsotillståndet för en tillståndskänslig service replik eller en tillståndslös tjänstinstans. Indata:

* [Krävs] hello partitions ID (GUID) och replik-ID som identifierar hello replik.
* [Valfritt] hello applikationsparametrarna hälsa principen används toooverride hello application manifest principer.
* [Valfritt] Filter för händelser som anger vilka poster som är intressanta och returneras i hello resultatet (till exempel fel, eller både varningar och fel). Alla händelser har använt tooevaluate hello samman entitetshälsa, oavsett hello filter.

### <a name="api"></a>API
tooget hello replik hälsa genom hello API, skapa en `FabricClient` och anrop hello [GetReplicaHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getreplicahealthasync) metod på dess HealthManager. toospecify avancerade parametrar, Använd [ReplicaHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.replicahealthquerydescription).

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>PowerShell
hello cmdlet tooget hello replik hälsa är [Get-ServiceFabricReplicaHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricreplicahealth). Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

hello hämtar följande cmdlet hello hälsotillstånd hello primära repliken för alla partitioner i hello-tjänsten:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Du kan hämta replik hälsotillstånd med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica-by-using-a-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.

## <a name="get-deployed-application-health"></a>Hämta distribuerade programhälsan
Returnerar hello hälsotillståndet för ett program som distribuerats på en nod entitet. Den innehåller hello distribuerade tjänsten paketet hälsotillstånd. Indata:

* [Krävs] hello programnamn (URI) och nodnamnet (sträng) som identifierar hello distribuerat program.
* [Valfritt] hello programmets hälsoprincip används toooverride hello application manifest principer.
* [Valfritt] Filter för händelser och distribuerade tjänstpaket som anger vilka poster som är intressanta och returneras i hello resultatet (till exempel fel, eller både varningar och fel). Alla händelser och distribuerade tjänstpaket är används tooevaluate hello samman entitetshälsa, oavsett hello filter.
* [Valfritt] Filtrera tooexclude hälsostatistik. Om inget annat anges, visar hello hälsostatistik hello antal distribuerade tjänstpaket i ok, varning och fel hälsotillstånd.

### <a name="api"></a>API
tooget hello hälsotillståndet för ett program som distribuerats på en nod via hello API, skapa en `FabricClient` och anrop hello [GetDeployedApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync) metod på dess HealthManager. toospecify valfria parametrar, använda [DeployedApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedapplicationhealthquerydescription).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShell
hello cmdlet tooget hello distribuerade programhälsan är [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps). Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet. toofind ut där ett program har distribuerats, kör [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) och titta på hello distribuerade program underordnade.

hello följande cmdlet hämtar hello hälsotillstånd hello **fabric: / WordCount** program som distribuerats på **_Node_2**.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_0


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_0
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok
                                     
                                     ServiceManifestName   : WordCountWebServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok
                                     
HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131444422261848308
                                     SentAt                : 7/13/2017 5:57:06 PM
                                     ReceivedAt            : 7/13/2017 5:57:17 PM
                                     TTL                   : Infinite
                                     Description           : hello application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/13/2017 5:57:17 PM, LastWarning = 1/1/0001 12:00:00 AM
                                     
HealthStatistics                   : 
                                     DeployedServicePackage : 2 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a>REST
Du kan hämta distribuerade programhälsan med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application-by-using-a-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.

## <a name="get-deployed-service-package-health"></a>Hämta distribuerade tjänstens hälsa för paketet
Returnerar hello hälsotillståndet för en distribuerad tjänst paketet entitet. Indata:

* [Krävs] hello programnamn (URI), nodnamnet (sträng) och manifestet tjänstnamn (sträng) som identifierar hello distribueras tjänstepaketet.
* [Valfritt] hello programmets hälsoprincip används toooverride hello application manifest princip.
* [Valfritt] Filter för händelser som anger vilka poster som är intressanta och returneras i hello resultatet (till exempel fel, eller både varningar och fel). Alla händelser har använt tooevaluate hello samman entitetshälsa, oavsett hello filter.

### <a name="api"></a>API
tooget hello hälsotillståndet för en distribuerad tjänstpaket via hello API, skapa en `FabricClient` och anrop hello [GetDeployedServicePackageHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync) metod på dess HealthManager. toospecify valfria parametrar, använda [DeployedServicePackageHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedservicepackagehealthquerydescription).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShell
hello cmdlet tooget hello distribueras tjänstens paketet hälsa är [Get-ServiceFabricDeployedServicePackageHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicepackagehealth). Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet. toosee där ett program har distribuerats, kör [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) och titta på hello distribuerade program. toosee vars servicepaket finns i ett program, titta på hello distribuerade tjänsten paketet barn i hello [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps) utdata.

hello följande cmdlet hämtar hello hälsotillstånd hello **WordCountServicePkg** tjänstpaket av hello **fabric: / WordCount** program som distribuerats på **_Node_2**. hello entiteten har **System.Hosting** rapporter för service-paket och startpunkten aktiveringen och typ av tjänst att registreringen har lyckats.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_2
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131444422267693359
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131444422267903345
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131444422272458374
                             SentAt                : 7/13/2017 5:57:07 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Du kan hämta distribuerade tjänstens hälsa för paketet med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package-by-using-a-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.

## <a name="health-chunk-queries"></a>Segmentet hälsoförfrågningar
hello hälsoförfrågningar segment kan returnera flera nivåer klustret underordnade (rekursivt) per inkommande trafik. Den stöder avancerade filter som gör att många alternativ att välja hello underordnade toobe returneras. hello filter kan ange underordnade genom hello Unik identifierare eller andra gruppidentifierare och/eller hälsotillstånd. Som standard ingår inga underordnade, som skillnad från toohealth kommandon som alltid innehåller underordnade på första nivån.

Hej [hälsoförfrågningar](service-fabric-view-entities-aggregated-health.md#health-queries) returnerar endast första nivån underordnade hello angetts entitet per krävs filter. tooget hello underordnade hello underordnade, måste du anropa ytterligare hälsa API: er för varje entitet av intresse. På liknande sätt tooget hello hälsotillståndet för specifika enheter, måste du anropa en hälsa API för varje entitet. hello segmentet frågan avancerad filtrering kan du toorequest flera objekt av intresse för en fråga, minimera hello meddelandestorlek och hello antalet meddelanden.

hello-värdet för hello segmentet frågan är att du kan få hälsotillståndet för flera kluster entiteter (potentiellt alla kluster entiteter från krävs rot) i ett anrop. Exempelvis kan du ange komplexa hälsa fråga:

* Returnerar endast program i fel och för dessa program innehåller alla tjänster i varning eller fel. Inkludera alla partitioner för returnerade tjänster.
* Returnera endast hello hälsotillståndet hos fyra program som anges av deras namn.
* Returnera endast hello hälsotillståndet hos program för en typ för önskat program.
* Returnera alla distribuerade enheter på en nod. Returnerar alla program, alla distribuerade program på hello nod och alla hello distribuerat tjänstpaket på noden.
* Returnera alla repliker i fel. Returnerar alla program, tjänster, partitioner och endast repliker i fel.
* Returnera alla program. Inkludera alla partitioner för en angiven tjänst.

Hello hälsa segmentet frågan exponeras för närvarande endast för hello klustret entiteten. Den returnerar ett kluster hälsa segment som innehåller:

* hello klustret samman hälsotillstånd.
* hello hälsa tillstånd segmentet listan över noder som respektera inkommande trafik.
* hello hälsa tillstånd segmentet lista över program som respektera inkommande trafik. Varje program hälsa tillstånd segment innehåller en segmentet lista med alla tjänster som respektera inkommande trafik och ett segment lista med alla distribuerade program som respektera hello filter. Samma för hello underordnade tjänster och distribuerade program. På så sätt kan alla entiteter i hello klustret kan potentiellt returneras om det krävs hierarkiskt.

### <a name="cluster-health-chunk-query"></a>Klustret hälsa segmentet fråga
Returnerar hello hälsotillstånd hello klustret entiteten och innehåller hello hierarkiska hälsa tillstånd mängder krävs underordnade. Indata:

* [Valfritt] hello klustret hälsoprincip för tooevaluate hello noder och hello klusterhändelser.
* [Valfritt] hello hälsa princip programavbildningen, används hello hälsoprinciper toooverride hello application manifest principer.
* [Valfritt] Filter för noder och program som anger vilka transaktioner är intressanta och returneras i hello resultat. hello filter är särskilda tooan entitet/grupp av enheter eller tillämpliga tooall entiteter på den nivån. hello listan över filter kan innehålla ett allmänt filter och/eller filter för specifika identifierare toofine grain entiteter som returneras av hello frågan. Om den är tom returneras inte hello underordnade som standard.
  Läs mer om hello filter på [NodeHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.nodehealthstatefilter) och [ApplicationHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthstatefilter). hello programmet filter kan rekursivt ange avancerade filter för barn.

hello segmentet resultatet innehåller hello underordnade som respektera hello filter.

För närvarande returneras hello segmentet inte ohälsosamt utvärderingar eller entiteten händelser. Den extra informationen kan hämtas med hjälp av hello befintliga kluster hälsa frågan.

### <a name="api"></a>API
tooget klustret hälsa dela in, skapa en `FabricClient` och anrop hello [GetClusterHealthChunkAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync) metod på dess **HealthManager**. Du kan skicka in [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthchunkquerydescription) toodescribe hälsoprinciper och avancerade filter.

hello hämtar följande kod klustret hälsa segment med avancerade filter.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except hello ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
hello cmdlet tooget hello klustret hälsa är [Get-ServiceFabricClusterChunkHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealthchunk). Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.

hello hämtar följande kod noder endast om de är i fel utom en viss nod som alltid ska returneras.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in hello cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters


HealthState                  : Warning
NodeHealthStateChunks        : 
                               TotalCount            : 1
                               
                               NodeName              : _Node_1
                               HealthState           : Ok
                               
ApplicationHealthStateChunks : None
```

hello följande cmdlet hämtar klustret segment med filter för programmet.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 1
                               
                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks : 
                                TotalCount            : 1
                               
                                ServiceName           : fabric:/WordCount/WordCountService
                                HealthState           : Error
                                PartitionHealthStateChunks : 
                                    TotalCount            : 1
                               
                                    PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                                    HealthState           : Error
                                    ReplicaHealthStateChunks : 
                                        TotalCount            : 5
                               
                                        ReplicaOrInstanceId   : 131444422293118720
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293118721
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293113678
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293113679
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422260002646
                                        HealthState           : Error
```

hello returnerar följande cmdlet alla distribuerade enheter på en nod.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 2
                               
                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1
                               
                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1
                               
                                    ServiceManifestName   : FAS
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
                               
                               
                               
                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1
                               
                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1
                               
                                    ServiceManifestName   : WordCountServicePkg
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
```

### <a name="rest"></a>REST
Du kan hämta klustret hälsa segment med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-using-health-chunks) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/health-of-cluster) som innehåller hälsoprinciper och avancerade filter som beskrivs i hello brödtext.

## <a name="general-queries"></a>Allmänna frågor
Allmänna frågor returnera en lista över Service Fabric-enheter på en viss typ av. De är tillgängliga via hello API (via hello metoder i **FabricClient.QueryManager**), PowerShell-cmdletar och REST. De här frågorna aggregera underfrågor från flera komponenter. En av dem är hello [hälsoarkivet](service-fabric-health-introduction.md#health-store), som fyller på hello samman hälsotillståndet för varje frågeresultatet.  

> [!NOTE]
> Allmänna frågor returnera hello samman hälsotillståndet för hello entitet och innehåller inte omfattande hälsa data. Om en enhet inte är felfri, kan du följa upp med hälsa frågor tooget alla dess hälsa information, inklusive händelser, underordnade hälsotillstånd och feltillstånd utvärderingar.
>
>

Om allmänna frågor returnerar ett okänt hälsotillståndet för en entitet, är det möjligt att hello health store inte har slutförts data om hello entitet. Det är också möjligt att en underfråga toohello hälsoarkivet inte lyckas (t.ex, det uppstod ett kommunikationsfel eller hello health store har begränsats). Följa upp med en fråga för hälsa för hello entiteten. Om hello underfråga påträffade tillfälliga fel, till exempel nätverksproblem, kan den här uppföljning frågan lyckas. Det kan också ge mer information från hello health store om varför exponeras inte hello entitet.

Hej frågor som innehåller **HealthState** för entiteter är:

* Nodlistan: returnerar hello listan noder i klustret för hello (växlat minne).
  * API: [FabricClient.QueryClient.GetNodeListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getnodelistasync)
  * PowerShell: Get-ServiceFabricNode
* Programlista: returnerar hello lista över program i hello kluster (växlat minne).
  * API: [FabricClient.QueryClient.GetApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync)
  * PowerShell: Get-ServiceFabricApplication
* Tjänstlista: returnerar hello lista över tjänster i ett program (växlat minne).
  * API: [FabricClient.QueryClient.GetServiceListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync)
  * PowerShell: Get-ServiceFabricService
* Partitionslista: returnerar hello lista över partitioner i en tjänst (växlat minne).
  * API: [FabricClient.QueryClient.GetPartitionListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getpartitionlistasync)
  * PowerShell: Get-ServiceFabricPartition
* Replikeringslistan: returnerar hello listan med repliker i en partition (växlat minne).
  * API: [FabricClient.QueryClient.GetReplicaListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getreplicalistasync)
  * PowerShell: Get-ServiceFabricReplica
* Distribuerat programlista: returnerar hello listan med distribuerade program på en nod.
  * API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync)
  * PowerShell: Get-ServiceFabricDeployedApplication
* Distribuera tjänsten paketlistan: returnerar hello lista över servicepaket i ett distribuerat program.
  * API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync)
  * PowerShell: Get-ServiceFabricDeployedApplication

> [!NOTE]
> Vissa av hello frågor returnerar växlingsbara resultat. hello returnera dessa frågor är en lista som härletts från [PagedList<T>](https://docs.microsoft.com/dotnet/api/system.fabric.query.pagedlist-1). Om hello resultatet inte ryms ett meddelande, returneras endast en sida och en ContinuationToken som spårar där uppräkningen avbröts. Fortsätt toocall hello samma fråga och skicka in hello fortsättningstoken från hello tidigare tooget nästa frågeresultaten.
>
>

### <a name="examples"></a>Exempel
hello hämtar följande kod hello felaktiga program i hello klustret:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

hello följande cmdlet hämtar hello programinformation för hello fabric: / WordCount-programmet. Observera att hälsotillståndet är på varningen.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

hello hämtar följande cmdlet hello-tjänster med ett hälsotillstånd fel:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Error"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Error
```

## <a name="cluster-and-application-upgrades"></a>Uppgradering av klustret och program
Under en övervakad uppgradering av hello kluster och program kontrollerar Service Fabric hälsa tooensure att allt är felfri. Om en enhet är i feltillstånd som utvärderas med hjälp av konfigurerade hälsoprinciper, gäller hello uppgraderingen uppgraderingen-specifika policys toodetermine hello nästa åtgärd. hello uppgraderingen kan vara pausas tooallow användarinteraktion (till exempel korrigera felvillkor eller ändra principer) eller den får automatiskt återställa toohello tidigare fungerande versionen.

Under en *klustret* uppgraderar du hello uppgraderingsstatus för klustret. hello uppgraderingsstatus innehåller felaktiga utvärderingar, vilka punkt toowhat är i feltillstånd i hello kluster. Om uppgraderingen hello återställs på grund av problem med toohealth ihåg hello uppgraderingsstatus hello senaste ohälsosamt orsaker. Den här informationen kan hjälpa administratörer att undersöka vad som gick fel när hello uppgraderingen återställde eller stoppats.

På liknande sätt under en *programmet* uppgraderar alla feltillstånd utvärderingar finns i hello uppgraderingsstatus för programmet.

hello följande visar hello programmet uppgraderingsstatus för ändrade fabric: / WordCount-programmet. En watchdog rapporterade ett fel på en av dess repliker. hello uppgraderingen rullande eftersom hello hälsokontroller inte uppfylls.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2017 5:23:26 PM
FailureTimestampUtc           : 4/21/2017 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

Läs mer om hello [uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-tootroubleshoot"></a>Använd hälsa utvärderingar tootroubleshoot
När det finns ett problem med hello kluster eller ett program, ser hello kluster eller ett program hälsa toopinpoint fel. hello ohälsosamt utvärderingar innehåller information om vilka utlösta hello aktuella tillstånd. Om du behöver, kan du detaljnivån i feltillstånd underordnade entiteter tooidentify hello orsaken.

Tänk dig ett program ohälsosamt eftersom det inte finns en felrapport på en av dess repliker. hello visar följande Powershell-cmdlet hello ohälsosamt utvärderingar:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount -EventsFilter None -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                                  
                                        Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.
                                  
                                        Unhealthy replica: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', ReplicaOrInstanceId='131444422260002646', AggregatedHealthState='Error'.
                                  
                                            Error event: SourceId='MyWatchdog', Property='Memory'.
                                  
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

Du kan titta på hello replik tooget mer information:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricReplicaHealth -ReplicaOrInstanceId 131444422260002646 -PartitionId af2e3e44-a8f8-45ac-9f31-4093eb897600


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Error
UnhealthyEvaluations  : 
                        Error event: SourceId='MyWatchdog', Property='Memory'.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                        
                        SourceId              : MyWatchdog
                        Property              : Memory
                        HealthState           : Error
                        SequenceNumber        : 131444451657749403
                        SentAt                : 7/13/2017 6:46:05 PM
                        ReceivedAt            : 7/13/2017 6:46:05 PM
                        TTL                   : Infinite
                        Description           : 
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 7/13/2017 6:46:05 PM, LastOk = 1/1/0001 12:00:00 AM
```

> [!NOTE]
> hello utvärderas ohälsosamt utvärderingar visar hello första skälet hello entiteten är toocurrent hälsotillstånd. Det kan finnas flera andra händelser som utlöser detta tillstånd, men de är inte återspeglas i hello utvärderingar. tooget mer information, ökad detaljnivå i hello hälsa entiteter toofigure ut alla hello ohälsosamt rapporter i hello kluster.
>
>

## <a name="next-steps"></a>Nästa steg
[Använd system hälsa rapporter tootroubleshoot](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Lägg till anpassade hälsorapporter för Service Fabric](service-fabric-report-health.md)

[Hur tooreport och kontrollera-tjänsten för hälsotillstånd](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Övervaka och diagnostisera tjänster lokalt](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md)
