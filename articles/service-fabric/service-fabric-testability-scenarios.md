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
# <a name="testability-scenarios"></a><span data-ttu-id="a071a-103">Möjlighet att testa scenarier</span><span class="sxs-lookup"><span data-stu-id="a071a-103">Testability scenarios</span></span>
<span data-ttu-id="a071a-104">Stora distribuerade system som molninfrastrukturer är natur instabilt.</span><span class="sxs-lookup"><span data-stu-id="a071a-104">Large distributed systems like cloud infrastructures are inherently unreliable.</span></span> <span data-ttu-id="a071a-105">Azure Service Fabric ger utvecklare hello möjlighet toowrite services toorun ovanpå instabilt infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="a071a-105">Azure Service Fabric gives developers hello ability toowrite services toorun on top of unreliable infrastructures.</span></span> <span data-ttu-id="a071a-106">I ordning toowrite hög kvalitet services behöver utvecklare toobe kan tooinduce sådana instabilt infrastruktur tootest hello stabiliteten för sina tjänster.</span><span class="sxs-lookup"><span data-stu-id="a071a-106">In order toowrite high-quality services, developers need toobe able tooinduce such unreliable infrastructure tootest hello stability of their services.</span></span>

<span data-ttu-id="a071a-107">hello fel Analysis Service ger utvecklare hello möjlighet tooinduce fel åtgärder tootest tjänster i hello förekomst av fel.</span><span class="sxs-lookup"><span data-stu-id="a071a-107">hello Fault Analysis Service gives developers hello ability tooinduce fault actions tootest services in hello presence of failures.</span></span> <span data-ttu-id="a071a-108">Dock får riktade simulerade fel du endast hittills.</span><span class="sxs-lookup"><span data-stu-id="a071a-108">However, targeted simulated faults will get you only so far.</span></span> <span data-ttu-id="a071a-109">tootake hello testa ytterligare, du kan använda hello testscenarier i Service Fabric: ett chaos test och ett redundanstest.</span><span class="sxs-lookup"><span data-stu-id="a071a-109">tootake hello testing further, you can use hello test scenarios in Service Fabric: a chaos test and a failover test.</span></span> <span data-ttu-id="a071a-110">Dessa scenarier simulera kontinuerlig överlagrad fel, både korrekt och städat i hela klustret hello under en längre tid.</span><span class="sxs-lookup"><span data-stu-id="a071a-110">These scenarios simulate continuous interleaved faults, both graceful and ungraceful, throughout hello cluster over extended periods of time.</span></span> <span data-ttu-id="a071a-111">När du har konfigurerat ett test med hello frekvensen och typ av fel kan du starta den C#-API: er eller PowerShell, toogenerate fel i hello kluster och din tjänst.</span><span class="sxs-lookup"><span data-stu-id="a071a-111">Once a test is configured with hello rate and kind of faults, it can be started through either C# APIs or PowerShell, toogenerate faults in hello cluster and your service.</span></span>

> [!WARNING]
> <span data-ttu-id="a071a-112">ChaosTestScenario ersätts av en mer flexibel, service-baserade Chaos.</span><span class="sxs-lookup"><span data-stu-id="a071a-112">ChaosTestScenario is being replaced by a more resilient, service-based Chaos.</span></span> <span data-ttu-id="a071a-113">Se toohello ny artikel [styrs Chaos](service-fabric-controlled-chaos.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a071a-113">Please refer toohello new article [Controlled Chaos](service-fabric-controlled-chaos.md) for more details.</span></span>
> 
> 

## <a name="chaos-test"></a><span data-ttu-id="a071a-114">Chaos test</span><span class="sxs-lookup"><span data-stu-id="a071a-114">Chaos test</span></span>
<span data-ttu-id="a071a-115">hello chaos scenariot genererar fel över hello hela Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="a071a-115">hello chaos scenario generates faults across hello entire Service Fabric cluster.</span></span> <span data-ttu-id="a071a-116">hello scenariot komprimerar fel som normalt visas i månader eller år tooa några timmar.</span><span class="sxs-lookup"><span data-stu-id="a071a-116">hello scenario compresses faults generally seen in months or years tooa few hours.</span></span> <span data-ttu-id="a071a-117">hello kombination av överlagrad fel med hög feltolerans hello hittar specialfall som annars saknas.</span><span class="sxs-lookup"><span data-stu-id="a071a-117">hello combination of interleaved faults with hello high fault rate finds corner cases that are otherwise missed.</span></span> <span data-ttu-id="a071a-118">Detta leder tooa betydande förbättringar i hello kod tjänstkvalitet hello.</span><span class="sxs-lookup"><span data-stu-id="a071a-118">This leads tooa significant improvement in hello code quality of hello service.</span></span>

### <a name="faults-simulated-in-hello-chaos-test"></a><span data-ttu-id="a071a-119">Fel simulerade i hello chaos test</span><span class="sxs-lookup"><span data-stu-id="a071a-119">Faults simulated in hello chaos test</span></span>
* <span data-ttu-id="a071a-120">Starta om en nod</span><span class="sxs-lookup"><span data-stu-id="a071a-120">Restart a node</span></span>
* <span data-ttu-id="a071a-121">Starta om distribuerade kodpaketet</span><span class="sxs-lookup"><span data-stu-id="a071a-121">Restart a deployed code package</span></span>
* <span data-ttu-id="a071a-122">Ta bort en replik</span><span class="sxs-lookup"><span data-stu-id="a071a-122">Remove a replica</span></span>
* <span data-ttu-id="a071a-123">Starta om en replik</span><span class="sxs-lookup"><span data-stu-id="a071a-123">Restart a replica</span></span>
* <span data-ttu-id="a071a-124">Flytta en primär replik (valfritt)</span><span class="sxs-lookup"><span data-stu-id="a071a-124">Move a primary replica (optional)</span></span>
* <span data-ttu-id="a071a-125">Flytta en sekundär replik (valfritt)</span><span class="sxs-lookup"><span data-stu-id="a071a-125">Move a secondary replica (optional)</span></span>

<span data-ttu-id="a071a-126">hello chaos testet körs flera iterationer av fel och klustret verifieringar för hello angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="a071a-126">hello chaos test runs multiple iterations of faults and cluster validations for hello specified period of time.</span></span> <span data-ttu-id="a071a-127">hello tidsåtgången för hello klustret toostabilize och validering toosucceed kan också konfigureras.</span><span class="sxs-lookup"><span data-stu-id="a071a-127">hello time spent for hello cluster toostabilize and for validation toosucceed is also configurable.</span></span> <span data-ttu-id="a071a-128">hello scenariot misslyckas när du klickar på ett enda fel i klusterverifieringen.</span><span class="sxs-lookup"><span data-stu-id="a071a-128">hello scenario fails when you hit a single failure in cluster validation.</span></span>

<span data-ttu-id="a071a-129">Anta till exempel att ett test ange toorun under en timme med högst tre samtidiga fel.</span><span class="sxs-lookup"><span data-stu-id="a071a-129">For example, consider a test set toorun for one hour with a maximum of three concurrent faults.</span></span> <span data-ttu-id="a071a-130">hello test framkalla tre fel och sedan Validera hello klustret hälsa.</span><span class="sxs-lookup"><span data-stu-id="a071a-130">hello test will induce three faults, and then validate hello cluster health.</span></span> <span data-ttu-id="a071a-131">hello test kommer att gå igenom hello föregående steg tills hello klustret blir ohälsosamt eller skickar en timme.</span><span class="sxs-lookup"><span data-stu-id="a071a-131">hello test will iterate through hello previous step till hello cluster becomes unhealthy or one hour passes.</span></span> <span data-ttu-id="a071a-132">Om hello klustret blir ohälsosamt i varje iteration, inte hålla d.v.s. inom en konfigurerad tid, hello testet misslyckas med ett undantag.</span><span class="sxs-lookup"><span data-stu-id="a071a-132">If hello cluster becomes unhealthy in any iteration, i.e. it does not stabilize within a configured time, hello test will fail with an exception.</span></span> <span data-ttu-id="a071a-133">Det här undantaget anger att något är fel och måste ytterligare undersökning.</span><span class="sxs-lookup"><span data-stu-id="a071a-133">This exception indicates that something has gone wrong and needs further investigation.</span></span>

<span data-ttu-id="a071a-134">I sin nuvarande form startar hello fel generation motorn i hello chaos test endast säker fel.</span><span class="sxs-lookup"><span data-stu-id="a071a-134">In its current form, hello fault generation engine in hello chaos test induces only safe faults.</span></span> <span data-ttu-id="a071a-135">Det innebär att hello saknas externa fel, ett kvorum eller data aldrig går förlorad.</span><span class="sxs-lookup"><span data-stu-id="a071a-135">This means that in hello absence of external faults, a quorum or data loss will never occur.</span></span>

### <a name="important-configuration-options"></a><span data-ttu-id="a071a-136">Viktiga konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="a071a-136">Important configuration options</span></span>
* <span data-ttu-id="a071a-137">**TimeToRun**: Total tid att hello testet körs innan du avslutar med framgång.</span><span class="sxs-lookup"><span data-stu-id="a071a-137">**TimeToRun**: Total time that hello test will run before finishing with success.</span></span> <span data-ttu-id="a071a-138">hello test kan slutföras tidigare i stället för en misslyckad validering.</span><span class="sxs-lookup"><span data-stu-id="a071a-138">hello test can finish earlier in lieu of a validation failure.</span></span>
* <span data-ttu-id="a071a-139">**MaxClusterStabilizationTimeout**: maximal mängd tid toowait för hello klustret toobecome felfri innan hello test.</span><span class="sxs-lookup"><span data-stu-id="a071a-139">**MaxClusterStabilizationTimeout**: Maximum amount of time toowait for hello cluster toobecome healthy before failing hello test.</span></span> <span data-ttu-id="a071a-140">hello kontrollerar är om klustret hälsa är OK, tjänstens hälsa är OK, hello Målstorlek på replikuppsättningen uppnås för hello service partition och inga InBuild-repliker finns.</span><span class="sxs-lookup"><span data-stu-id="a071a-140">hello checks performed are whether cluster health is OK, service health is OK, hello target replica set size is achieved for hello service partition, and no InBuild replicas exist.</span></span>
* <span data-ttu-id="a071a-141">**MaxConcurrentFaults**: maximalt antal samtidiga fel framkallas i varje iteration.</span><span class="sxs-lookup"><span data-stu-id="a071a-141">**MaxConcurrentFaults**: Maximum number of concurrent faults induced in each iteration.</span></span> <span data-ttu-id="a071a-142">Hej högre hello nummer, hello mer aggressivt hello test, därför ledde till övergången kombinationer och mer komplexa växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="a071a-142">hello higher hello number, hello more aggressive hello test, hence resulting in more complex failovers and transition combinations.</span></span> <span data-ttu-id="a071a-143">hello test garanterar att externa fel saknas det inte kommer att ett kvorum och förlust av data, oavsett hur hög den här konfigurationen är.</span><span class="sxs-lookup"><span data-stu-id="a071a-143">hello test guarantees that in absence of external faults there will not be a quorum or data loss, irrespective of how high this configuration is.</span></span>
* <span data-ttu-id="a071a-144">**EnableMoveReplicaFaults**: aktiverar eller inaktiverar hello-fel som orsakar hello flyttas hello primära eller sekundära repliker.</span><span class="sxs-lookup"><span data-stu-id="a071a-144">**EnableMoveReplicaFaults**: Enables or disables hello faults that are causing hello move of hello primary or secondary replicas.</span></span> <span data-ttu-id="a071a-145">Dessa fel är inaktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="a071a-145">These faults are disabled by default.</span></span>
* <span data-ttu-id="a071a-146">**WaitTimeBetweenIterations**: mängden tid toowait mellan iterationer, dvs. efter en runda av fel och motsvarande validering.</span><span class="sxs-lookup"><span data-stu-id="a071a-146">**WaitTimeBetweenIterations**: Amount of time toowait between iterations, i.e. after a round of faults and corresponding validation.</span></span>

### <a name="how-toorun-hello-chaos-test"></a><span data-ttu-id="a071a-147">Hur toorun hello chaos testa</span><span class="sxs-lookup"><span data-stu-id="a071a-147">How toorun hello chaos test</span></span>
<span data-ttu-id="a071a-148">C#-exempel</span><span class="sxs-lookup"><span data-stu-id="a071a-148">C# sample</span></span>

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

<span data-ttu-id="a071a-149">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a071a-149">PowerShell</span></span>

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```


## <a name="failover-test"></a><span data-ttu-id="a071a-150">Redundanstest</span><span class="sxs-lookup"><span data-stu-id="a071a-150">Failover test</span></span>
<span data-ttu-id="a071a-151">Hej Testscenario för växling vid fel är en version av hello chaos Testscenario som riktar sig till en specifik tjänst partition.</span><span class="sxs-lookup"><span data-stu-id="a071a-151">hello failover test scenario is a version of hello chaos test scenario that targets a specific service partition.</span></span> <span data-ttu-id="a071a-152">Den testar hello effekten av växling vid fel på en specifik tjänst-partition och lämna hello andra tjänster påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="a071a-152">It tests hello effect of failover on a specific service partition while leaving hello other services unaffected.</span></span> <span data-ttu-id="a071a-153">När den har konfigurerats med hello mål partitionsinformation och andra parametrar, körs som ett verktyg för klientsidan som använder antingen C#-API: er eller PowerShell toogenerate fel för en partition med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a071a-153">Once it's configured with hello target partition information and other parameters, it runs as a client-side tool that uses either C# APIs or PowerShell toogenerate faults for a service partition.</span></span> <span data-ttu-id="a071a-154">hello scenario upprepas i en sekvens av simulerade fel och tjänsten validering när affärslogik som körs på hello sida tooprovide en arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="a071a-154">hello scenario iterates through a sequence of simulated faults and service validation while your business logic runs on hello side tooprovide a workload.</span></span> <span data-ttu-id="a071a-155">Ett fel i tjänsten validering indikerar ett problem som kräver ytterligare undersökning.</span><span class="sxs-lookup"><span data-stu-id="a071a-155">A failure in service validation indicates an issue that needs further investigation.</span></span>

### <a name="faults-simulated-in-hello-failover-test"></a><span data-ttu-id="a071a-156">Fel simulerade i hello redundanstest</span><span class="sxs-lookup"><span data-stu-id="a071a-156">Faults simulated in hello failover test</span></span>
* <span data-ttu-id="a071a-157">Starta om en distribuerade kodpaketet där hello partitionen finns</span><span class="sxs-lookup"><span data-stu-id="a071a-157">Restart a deployed code package where hello partition is hosted</span></span>
* <span data-ttu-id="a071a-158">Ta bort en replik av primära och sekundära eller tillståndslösa instans</span><span class="sxs-lookup"><span data-stu-id="a071a-158">Remove a primary/secondary replica or stateless instance</span></span>
* <span data-ttu-id="a071a-159">Starta om en primär och sekundär replik (om en beständig tjänst)</span><span class="sxs-lookup"><span data-stu-id="a071a-159">Restart a primary secondary replica (if a persisted service)</span></span>
* <span data-ttu-id="a071a-160">Flytta en primär replik</span><span class="sxs-lookup"><span data-stu-id="a071a-160">Move a primary replica</span></span>
* <span data-ttu-id="a071a-161">Flytta en sekundär replik</span><span class="sxs-lookup"><span data-stu-id="a071a-161">Move a secondary replica</span></span>
* <span data-ttu-id="a071a-162">Starta om hello partition</span><span class="sxs-lookup"><span data-stu-id="a071a-162">Restart hello partition</span></span>

<span data-ttu-id="a071a-163">Hej redundanstest startar ett valt fel och kör verifieringen på hello service tooensure dess stabilitet.</span><span class="sxs-lookup"><span data-stu-id="a071a-163">hello failover test induces a chosen fault and then runs validation on hello service tooensure its stability.</span></span> <span data-ttu-id="a071a-164">Hej redundanstest startar endast något fel på en tid, till skillnad från toopossible flera fel i hello chaos test.</span><span class="sxs-lookup"><span data-stu-id="a071a-164">hello failover test induces only one fault at a time, as opposed toopossible multiple faults in hello chaos test.</span></span> <span data-ttu-id="a071a-165">Hello testet misslyckas om hello service partitionen inte hålla inom hello konfigurerade tidsgränsen efter varje fel.</span><span class="sxs-lookup"><span data-stu-id="a071a-165">If hello service partition does not stabilize within hello configured timeout after each fault, hello test fails.</span></span> <span data-ttu-id="a071a-166">hello test startar endast säker fel.</span><span class="sxs-lookup"><span data-stu-id="a071a-166">hello test induces only safe faults.</span></span> <span data-ttu-id="a071a-167">Det innebär att externa fel saknas, ett kvorum och förlust av data inte utförs.</span><span class="sxs-lookup"><span data-stu-id="a071a-167">This means that in absence of external failures, a quorum or data loss will not occur.</span></span>

### <a name="important-configuration-options"></a><span data-ttu-id="a071a-168">Viktiga konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="a071a-168">Important configuration options</span></span>
* <span data-ttu-id="a071a-169">**PartitionSelector**: Selector-objekt som anger hello-partition som behöver toobe som mål.</span><span class="sxs-lookup"><span data-stu-id="a071a-169">**PartitionSelector**: Selector object that specifies hello partition that needs toobe targeted.</span></span>
* <span data-ttu-id="a071a-170">**TimeToRun**: Total tid att hello testet körs innan du avslutar.</span><span class="sxs-lookup"><span data-stu-id="a071a-170">**TimeToRun**: Total time that hello test will run before finishing.</span></span>
* <span data-ttu-id="a071a-171">**MaxServiceStabilizationTimeout**: maximal mängd tid toowait för hello klustret toobecome felfri innan hello test.</span><span class="sxs-lookup"><span data-stu-id="a071a-171">**MaxServiceStabilizationTimeout**: Maximum amount of time toowait for hello cluster toobecome healthy before failing hello test.</span></span> <span data-ttu-id="a071a-172">hello kontrollerar är om tjänstens hälsa är OK, hello Målstorlek på replikuppsättningen uppnås för alla partitioner och inga InBuild-repliker finns.</span><span class="sxs-lookup"><span data-stu-id="a071a-172">hello checks performed are whether service health is OK, hello target replica set size is achieved for all partitions, and no InBuild replicas exist.</span></span>
* <span data-ttu-id="a071a-173">**WaitTimeBetweenFaults**: mängden tid toowait mellan varje fel- och validering cykel.</span><span class="sxs-lookup"><span data-stu-id="a071a-173">**WaitTimeBetweenFaults**: Amount of time toowait between every fault and validation cycle.</span></span>

### <a name="how-toorun-hello-failover-test"></a><span data-ttu-id="a071a-174">Hur toorun hello redundans testa</span><span class="sxs-lookup"><span data-stu-id="a071a-174">How toorun hello failover test</span></span>
<span data-ttu-id="a071a-175">**C#**</span><span class="sxs-lookup"><span data-stu-id="a071a-175">**C#**</span></span>

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


<span data-ttu-id="a071a-176">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="a071a-176">**PowerShell**</span></span>

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/SampleApp/SampleService"

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindSingleton
```
