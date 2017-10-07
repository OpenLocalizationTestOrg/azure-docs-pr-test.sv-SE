---
title: "aaaCreate chaos och redundans tester för Azure mikrotjänster | Microsoft Docs"
description: "Med hjälp av hello Service Fabric chaos test och redundans testa scenarier tooinduce fel och verifiera hello tillförlitligheten för dina tjänster."
services: service-fabric
documentationcenter: .net
author: motanv
manager: rsinha
editor: toddabel
ms.assetid: 8eee7e89-404a-4605-8f00-7e4d4fb17553
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv
ms.openlocfilehash: 1cac4f9e0e4a6c8416d5220d1537b5110decd1f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="testability-scenarios"></a>Möjlighet att testa scenarier
Stora distribuerade system som molninfrastrukturer är natur instabilt. Azure Service Fabric ger utvecklare hello möjlighet toowrite services toorun ovanpå instabilt infrastruktur. I ordning toowrite hög kvalitet services behöver utvecklare toobe kan tooinduce sådana instabilt infrastruktur tootest hello stabiliteten för sina tjänster.

hello fel Analysis Service ger utvecklare hello möjlighet tooinduce fel åtgärder tootest tjänster i hello förekomst av fel. Dock får riktade simulerade fel du endast hittills. tootake hello testa ytterligare, du kan använda hello testscenarier i Service Fabric: ett chaos test och ett redundanstest. Dessa scenarier simulera kontinuerlig överlagrad fel, både korrekt och städat i hela klustret hello under en längre tid. När du har konfigurerat ett test med hello frekvensen och typ av fel kan du starta den C#-API: er eller PowerShell, toogenerate fel i hello kluster och din tjänst.

> [!WARNING]
> ChaosTestScenario ersätts av en mer flexibel, service-baserade Chaos. Se toohello ny artikel [styrs Chaos](service-fabric-controlled-chaos.md) för mer information.
> 
> 

## <a name="chaos-test"></a>Chaos test
hello chaos scenariot genererar fel över hello hela Service Fabric-kluster. hello scenariot komprimerar fel som normalt visas i månader eller år tooa några timmar. hello kombination av överlagrad fel med hög feltolerans hello hittar specialfall som annars saknas. Detta leder tooa betydande förbättringar i hello kod tjänstkvalitet hello.

### <a name="faults-simulated-in-hello-chaos-test"></a>Fel simulerade i hello chaos test
* Starta om en nod
* Starta om distribuerade kodpaketet
* Ta bort en replik
* Starta om en replik
* Flytta en primär replik (valfritt)
* Flytta en sekundär replik (valfritt)

hello chaos testet körs flera iterationer av fel och klustret verifieringar för hello angiven tidsperiod. hello tidsåtgången för hello klustret toostabilize och validering toosucceed kan också konfigureras. hello scenariot misslyckas när du klickar på ett enda fel i klusterverifieringen.

Anta till exempel att ett test ange toorun under en timme med högst tre samtidiga fel. hello test framkalla tre fel och sedan Validera hello klustret hälsa. hello test kommer att gå igenom hello föregående steg tills hello klustret blir ohälsosamt eller skickar en timme. Om hello klustret blir ohälsosamt i varje iteration, inte hålla d.v.s. inom en konfigurerad tid, hello testet misslyckas med ett undantag. Det här undantaget anger att något är fel och måste ytterligare undersökning.

I sin nuvarande form startar hello fel generation motorn i hello chaos test endast säker fel. Det innebär att hello saknas externa fel, ett kvorum eller data aldrig går förlorad.

### <a name="important-configuration-options"></a>Viktiga konfigurationsalternativ
* **TimeToRun**: Total tid att hello testet körs innan du avslutar med framgång. hello test kan slutföras tidigare i stället för en misslyckad validering.
* **MaxClusterStabilizationTimeout**: maximal mängd tid toowait för hello klustret toobecome felfri innan hello test. hello kontrollerar är om klustret hälsa är OK, tjänstens hälsa är OK, hello Målstorlek på replikuppsättningen uppnås för hello service partition och inga InBuild-repliker finns.
* **MaxConcurrentFaults**: maximalt antal samtidiga fel framkallas i varje iteration. Hej högre hello nummer, hello mer aggressivt hello test, därför ledde till övergången kombinationer och mer komplexa växling vid fel. hello test garanterar att externa fel saknas det inte kommer att ett kvorum och förlust av data, oavsett hur hög den här konfigurationen är.
* **EnableMoveReplicaFaults**: aktiverar eller inaktiverar hello-fel som orsakar hello flyttas hello primära eller sekundära repliker. Dessa fel är inaktiverade som standard.
* **WaitTimeBetweenIterations**: mängden tid toowait mellan iterationer, dvs. efter en runda av fel och motsvarande validering.

### <a name="how-toorun-hello-chaos-test"></a>Hur toorun hello chaos testa
C#-exempel

```csharp
using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunChaosTestScenarioAsync(clusterConnection).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunChaosTestScenarioAsync(string clusterConnection)
    {
        TimeSpan maxClusterStabilizationTimeout = TimeSpan.FromSeconds(180);
        uint maxConcurrentFaults = 3;
        bool enableMoveReplicaFaults = true;

        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // hello chaos test scenario should run at least 60 minutes or until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        ChaosTestScenarioParameters scenarioParameters = new ChaosTestScenarioParameters(
          maxClusterStabilizationTimeout,
          maxConcurrentFaults,
          enableMoveReplicaFaults,
          timeToRun);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create hello scenario class and execute it asynchronously.
        ChaosTestScenario chaosScenario = new ChaosTestScenario(fabricClient, scenarioParameters);

        try
        {
            await chaosScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```

PowerShell

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```


## <a name="failover-test"></a>Redundanstest
Hej Testscenario för växling vid fel är en version av hello chaos Testscenario som riktar sig till en specifik tjänst partition. Den testar hello effekten av växling vid fel på en specifik tjänst-partition och lämna hello andra tjänster påverkas inte. När den har konfigurerats med hello mål partitionsinformation och andra parametrar, körs som ett verktyg för klientsidan som använder antingen C#-API: er eller PowerShell toogenerate fel för en partition med tjänsten. hello scenario upprepas i en sekvens av simulerade fel och tjänsten validering när affärslogik som körs på hello sida tooprovide en arbetsbelastning. Ett fel i tjänsten validering indikerar ett problem som kräver ytterligare undersökning.

### <a name="faults-simulated-in-hello-failover-test"></a>Fel simulerade i hello redundanstest
* Starta om en distribuerade kodpaketet där hello partitionen finns
* Ta bort en replik av primära och sekundära eller tillståndslösa instans
* Starta om en primär och sekundär replik (om en beständig tjänst)
* Flytta en primär replik
* Flytta en sekundär replik
* Starta om hello partition

Hej redundanstest startar ett valt fel och kör verifieringen på hello service tooensure dess stabilitet. Hej redundanstest startar endast något fel på en tid, till skillnad från toopossible flera fel i hello chaos test. Hello testet misslyckas om hello service partitionen inte hålla inom hello konfigurerade tidsgränsen efter varje fel. hello test startar endast säker fel. Det innebär att externa fel saknas, ett kvorum och förlust av data inte utförs.

### <a name="important-configuration-options"></a>Viktiga konfigurationsalternativ
* **PartitionSelector**: Selector-objekt som anger hello-partition som behöver toobe som mål.
* **TimeToRun**: Total tid att hello testet körs innan du avslutar.
* **MaxServiceStabilizationTimeout**: maximal mängd tid toowait för hello klustret toobecome felfri innan hello test. hello kontrollerar är om tjänstens hälsa är OK, hello Målstorlek på replikuppsättningen uppnås för alla partitioner och inga InBuild-repliker finns.
* **WaitTimeBetweenFaults**: mängden tid toowait mellan varje fel- och validering cykel.

### <a name="how-toorun-hello-failover-test"></a>Hur toorun hello redundans testa
**C#**

```csharp
using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunFailoverTestScenarioAsync(clusterConnection, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunFailoverTestScenarioAsync(string clusterConnection, Uri serviceName)
    {
        TimeSpan maxServiceStabilizationTimeout = TimeSpan.FromSeconds(180);
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // hello chaos test scenario should run at least 60 minutes or until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        FailoverTestScenarioParameters scenarioParameters = new FailoverTestScenarioParameters(
          randomPartitionSelector,
          timeToRun,
          maxServiceStabilizationTimeout);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create hello scenario class and execute it asynchronously.
        FailoverTestScenario failoverScenario = new FailoverTestScenario(fabricClient, scenarioParameters);

        try
        {
            await failoverScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```


**PowerShell**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/SampleApp/SampleService"

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindSingleton
```
