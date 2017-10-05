---
title: "Skapa chaos och redundans tester för Azure mikrotjänster | Microsoft Docs"
description: "Med hjälp av Service Fabric chaos test och redundans, testa scenarier för att framkalla fel och verifiera att tjänster."
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
ms.openlocfilehash: d06026c750e01ad5825338a78d9af331265f434a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="testability-scenarios"></a><span data-ttu-id="11b7c-103">Möjlighet att testa scenarier</span><span class="sxs-lookup"><span data-stu-id="11b7c-103">Testability scenarios</span></span>
<span data-ttu-id="11b7c-104">Stora distribuerade system som molninfrastrukturer är natur instabilt.</span><span class="sxs-lookup"><span data-stu-id="11b7c-104">Large distributed systems like cloud infrastructures are inherently unreliable.</span></span> <span data-ttu-id="11b7c-105">Azure Service Fabric ger utvecklare möjligheten att skriva tjänster som körs ovanpå instabilt infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="11b7c-105">Azure Service Fabric gives developers the ability to write services to run on top of unreliable infrastructures.</span></span> <span data-ttu-id="11b7c-106">För att kunna skriva hög kvalitet services utvecklare behöver kan orsaka sådana instabilt infrastruktur för att testa stabiliteten för sina tjänster.</span><span class="sxs-lookup"><span data-stu-id="11b7c-106">In order to write high-quality services, developers need to be able to induce such unreliable infrastructure to test the stability of their services.</span></span>

<span data-ttu-id="11b7c-107">Fel Analysis Service ger utvecklare möjligheten att orsaka fel åtgärder för att testa tjänster med fel.</span><span class="sxs-lookup"><span data-stu-id="11b7c-107">The Fault Analysis Service gives developers the ability to induce fault actions to test services in the presence of failures.</span></span> <span data-ttu-id="11b7c-108">Dock får riktade simulerade fel du endast hittills.</span><span class="sxs-lookup"><span data-stu-id="11b7c-108">However, targeted simulated faults will get you only so far.</span></span> <span data-ttu-id="11b7c-109">Om du vill ta den testa ytterligare, du kan använda testscenarier i Service Fabric: ett chaos test och ett redundanstest.</span><span class="sxs-lookup"><span data-stu-id="11b7c-109">To take the testing further, you can use the test scenarios in Service Fabric: a chaos test and a failover test.</span></span> <span data-ttu-id="11b7c-110">Dessa scenarier simulera kontinuerlig överlagrad fel, både korrekt och städat i hela klustret under en längre tid.</span><span class="sxs-lookup"><span data-stu-id="11b7c-110">These scenarios simulate continuous interleaved faults, both graceful and ungraceful, throughout the cluster over extended periods of time.</span></span> <span data-ttu-id="11b7c-111">När du har konfigurerat ett test med frekvensen och typ av fel kan du starta den via C#-API: er eller PowerShell, för att generera fel i klustret och din tjänst.</span><span class="sxs-lookup"><span data-stu-id="11b7c-111">Once a test is configured with the rate and kind of faults, it can be started through either C# APIs or PowerShell, to generate faults in the cluster and your service.</span></span>

> [!WARNING]
> <span data-ttu-id="11b7c-112">ChaosTestScenario ersätts av en mer flexibel, service-baserade Chaos.</span><span class="sxs-lookup"><span data-stu-id="11b7c-112">ChaosTestScenario is being replaced by a more resilient, service-based Chaos.</span></span> <span data-ttu-id="11b7c-113">Se den nya artikeln [styrs Chaos](service-fabric-controlled-chaos.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="11b7c-113">Please refer to the new article [Controlled Chaos](service-fabric-controlled-chaos.md) for more details.</span></span>
> 
> 

## <a name="chaos-test"></a><span data-ttu-id="11b7c-114">Chaos test</span><span class="sxs-lookup"><span data-stu-id="11b7c-114">Chaos test</span></span>
<span data-ttu-id="11b7c-115">Chaos scenariot genererar fel över hela Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="11b7c-115">The chaos scenario generates faults across the entire Service Fabric cluster.</span></span> <span data-ttu-id="11b7c-116">Scenariot komprimerar fel som normalt visas i månader eller år till några timmar.</span><span class="sxs-lookup"><span data-stu-id="11b7c-116">The scenario compresses faults generally seen in months or years to a few hours.</span></span> <span data-ttu-id="11b7c-117">Kombinationen av överlagrad fel med hög feltolerans hastighet hittar specialfall som annars saknas.</span><span class="sxs-lookup"><span data-stu-id="11b7c-117">The combination of interleaved faults with the high fault rate finds corner cases that are otherwise missed.</span></span> <span data-ttu-id="11b7c-118">Detta leder till en betydande förbättringar i koden Tjänstkvalitet.</span><span class="sxs-lookup"><span data-stu-id="11b7c-118">This leads to a significant improvement in the code quality of the service.</span></span>

### <a name="faults-simulated-in-the-chaos-test"></a><span data-ttu-id="11b7c-119">Fel simulerade i chaos-test</span><span class="sxs-lookup"><span data-stu-id="11b7c-119">Faults simulated in the chaos test</span></span>
* <span data-ttu-id="11b7c-120">Starta om en nod</span><span class="sxs-lookup"><span data-stu-id="11b7c-120">Restart a node</span></span>
* <span data-ttu-id="11b7c-121">Starta om distribuerade kodpaketet</span><span class="sxs-lookup"><span data-stu-id="11b7c-121">Restart a deployed code package</span></span>
* <span data-ttu-id="11b7c-122">Ta bort en replik</span><span class="sxs-lookup"><span data-stu-id="11b7c-122">Remove a replica</span></span>
* <span data-ttu-id="11b7c-123">Starta om en replik</span><span class="sxs-lookup"><span data-stu-id="11b7c-123">Restart a replica</span></span>
* <span data-ttu-id="11b7c-124">Flytta en primär replik (valfritt)</span><span class="sxs-lookup"><span data-stu-id="11b7c-124">Move a primary replica (optional)</span></span>
* <span data-ttu-id="11b7c-125">Flytta en sekundär replik (valfritt)</span><span class="sxs-lookup"><span data-stu-id="11b7c-125">Move a secondary replica (optional)</span></span>

<span data-ttu-id="11b7c-126">Chaos testet körs flera iterationer av fel och klustret verifieringar för den angivna tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="11b7c-126">The chaos test runs multiple iterations of faults and cluster validations for the specified period of time.</span></span> <span data-ttu-id="11b7c-127">Den tid som krävs för klustret att hålla och verifiering lyckas kan också konfigureras.</span><span class="sxs-lookup"><span data-stu-id="11b7c-127">The time spent for the cluster to stabilize and for validation to succeed is also configurable.</span></span> <span data-ttu-id="11b7c-128">Scenariot misslyckas när du klickar på ett enda fel i klusterverifieringen.</span><span class="sxs-lookup"><span data-stu-id="11b7c-128">The scenario fails when you hit a single failure in cluster validation.</span></span>

<span data-ttu-id="11b7c-129">Tänk dig ett test inställd på att köras under en timme med högst tre samtidiga fel.</span><span class="sxs-lookup"><span data-stu-id="11b7c-129">For example, consider a test set to run for one hour with a maximum of three concurrent faults.</span></span> <span data-ttu-id="11b7c-130">Testet framkalla tre fel och sedan kontrollera hälsotillstånd för klustret.</span><span class="sxs-lookup"><span data-stu-id="11b7c-130">The test will induce three faults, and then validate the cluster health.</span></span> <span data-ttu-id="11b7c-131">Testet ska gå igenom föregående steg tills klustret blir ohälsosamt eller skickar en timme.</span><span class="sxs-lookup"><span data-stu-id="11b7c-131">The test will iterate through the previous step till the cluster becomes unhealthy or one hour passes.</span></span> <span data-ttu-id="11b7c-132">Om klustret blir ohälsosamt i varje iteration, inte hålla d.v.s. inom en konfigurerad tid, testet misslyckas med ett undantag.</span><span class="sxs-lookup"><span data-stu-id="11b7c-132">If the cluster becomes unhealthy in any iteration, i.e. it does not stabilize within a configured time, the test will fail with an exception.</span></span> <span data-ttu-id="11b7c-133">Det här undantaget anger att något är fel och måste ytterligare undersökning.</span><span class="sxs-lookup"><span data-stu-id="11b7c-133">This exception indicates that something has gone wrong and needs further investigation.</span></span>

<span data-ttu-id="11b7c-134">I sin nuvarande form startar fel generation motorn i testet chaos endast säker fel.</span><span class="sxs-lookup"><span data-stu-id="11b7c-134">In its current form, the fault generation engine in the chaos test induces only safe faults.</span></span> <span data-ttu-id="11b7c-135">Det innebär att externa fel saknas, ett kvorum eller dataförlust inträffar aldrig.</span><span class="sxs-lookup"><span data-stu-id="11b7c-135">This means that in the absence of external faults, a quorum or data loss will never occur.</span></span>

### <a name="important-configuration-options"></a><span data-ttu-id="11b7c-136">Viktiga konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="11b7c-136">Important configuration options</span></span>
* <span data-ttu-id="11b7c-137">**TimeToRun**: Total tid som testet ska köras innan du avslutar med framgång.</span><span class="sxs-lookup"><span data-stu-id="11b7c-137">**TimeToRun**: Total time that the test will run before finishing with success.</span></span> <span data-ttu-id="11b7c-138">Testet kan slutföras tidigare i stället för en misslyckad validering.</span><span class="sxs-lookup"><span data-stu-id="11b7c-138">The test can finish earlier in lieu of a validation failure.</span></span>
* <span data-ttu-id="11b7c-139">**MaxClusterStabilizationTimeout**: maximal mängd väntetiden för att klustret ska bli felfri innan testet.</span><span class="sxs-lookup"><span data-stu-id="11b7c-139">**MaxClusterStabilizationTimeout**: Maximum amount of time to wait for the cluster to become healthy before failing the test.</span></span> <span data-ttu-id="11b7c-140">De kontroller som utförs är om klustret hälsa är OK, tjänstens hälsa är OK, mål replikuppsättning uppnås för service-partition och inga InBuild-repliker finns.</span><span class="sxs-lookup"><span data-stu-id="11b7c-140">The checks performed are whether cluster health is OK, service health is OK, the target replica set size is achieved for the service partition, and no InBuild replicas exist.</span></span>
* <span data-ttu-id="11b7c-141">**MaxConcurrentFaults**: maximalt antal samtidiga fel framkallas i varje iteration.</span><span class="sxs-lookup"><span data-stu-id="11b7c-141">**MaxConcurrentFaults**: Maximum number of concurrent faults induced in each iteration.</span></span> <span data-ttu-id="11b7c-142">Ju fler, mer aggressivt testet därför ledde till övergången kombinationer och mer komplexa växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="11b7c-142">The higher the number, the more aggressive the test, hence resulting in more complex failovers and transition combinations.</span></span> <span data-ttu-id="11b7c-143">Testet garanterar att externa fel saknas det inte kommer att ett kvorum och förlust av data, oavsett hur hög den här konfigurationen är.</span><span class="sxs-lookup"><span data-stu-id="11b7c-143">The test guarantees that in absence of external faults there will not be a quorum or data loss, irrespective of how high this configuration is.</span></span>
* <span data-ttu-id="11b7c-144">**EnableMoveReplicaFaults**: aktiverar eller inaktiverar de fel som orsakar flytta primära eller sekundära repliker.</span><span class="sxs-lookup"><span data-stu-id="11b7c-144">**EnableMoveReplicaFaults**: Enables or disables the faults that are causing the move of the primary or secondary replicas.</span></span> <span data-ttu-id="11b7c-145">Dessa fel är inaktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="11b7c-145">These faults are disabled by default.</span></span>
* <span data-ttu-id="11b7c-146">**WaitTimeBetweenIterations**: tid som ska förflyta mellan iterationer, dvs. efter en runda av fel och motsvarande validering.</span><span class="sxs-lookup"><span data-stu-id="11b7c-146">**WaitTimeBetweenIterations**: Amount of time to wait between iterations, i.e. after a round of faults and corresponding validation.</span></span>

### <a name="how-to-run-the-chaos-test"></a><span data-ttu-id="11b7c-147">Hur du kör testet chaos</span><span class="sxs-lookup"><span data-stu-id="11b7c-147">How to run the chaos test</span></span>
<span data-ttu-id="11b7c-148">C#-exempel</span><span class="sxs-lookup"><span data-stu-id="11b7c-148">C# sample</span></span>

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

        // The chaos test scenario should run at least 60 minutes or until it fails.
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

        // Create the scenario class and execute it asynchronously.
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

<span data-ttu-id="11b7c-149">PowerShell</span><span class="sxs-lookup"><span data-stu-id="11b7c-149">PowerShell</span></span>

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```


## <a name="failover-test"></a><span data-ttu-id="11b7c-150">Redundanstest</span><span class="sxs-lookup"><span data-stu-id="11b7c-150">Failover test</span></span>
<span data-ttu-id="11b7c-151">Testscenario redundans är en version av chaos test-scenario som riktar sig till en specifik tjänst partition.</span><span class="sxs-lookup"><span data-stu-id="11b7c-151">The failover test scenario is a version of the chaos test scenario that targets a specific service partition.</span></span> <span data-ttu-id="11b7c-152">Den testar effekten av växling vid fel på en specifik tjänst-partition och lämna de andra tjänsterna påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="11b7c-152">It tests the effect of failover on a specific service partition while leaving the other services unaffected.</span></span> <span data-ttu-id="11b7c-153">När den har konfigurerats med partitionsinformation mål och andra parametrar, körs som ett verktyg för klientsidan som använder antingen C#-API: er eller PowerShell för att generera fel för en partition med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="11b7c-153">Once it's configured with the target partition information and other parameters, it runs as a client-side tool that uses either C# APIs or PowerShell to generate faults for a service partition.</span></span> <span data-ttu-id="11b7c-154">Scenariot upprepas i en sekvens av simulerade fel och tjänsten validering när affärslogik som körs på sidan för att tillhandahålla en arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="11b7c-154">The scenario iterates through a sequence of simulated faults and service validation while your business logic runs on the side to provide a workload.</span></span> <span data-ttu-id="11b7c-155">Ett fel i tjänsten validering indikerar ett problem som kräver ytterligare undersökning.</span><span class="sxs-lookup"><span data-stu-id="11b7c-155">A failure in service validation indicates an issue that needs further investigation.</span></span>

### <a name="faults-simulated-in-the-failover-test"></a><span data-ttu-id="11b7c-156">Fel simulerade i failover-test</span><span class="sxs-lookup"><span data-stu-id="11b7c-156">Faults simulated in the failover test</span></span>
* <span data-ttu-id="11b7c-157">Starta om en distribuerade kodpaketet som partitionen finns</span><span class="sxs-lookup"><span data-stu-id="11b7c-157">Restart a deployed code package where the partition is hosted</span></span>
* <span data-ttu-id="11b7c-158">Ta bort en replik av primära och sekundära eller tillståndslösa instans</span><span class="sxs-lookup"><span data-stu-id="11b7c-158">Remove a primary/secondary replica or stateless instance</span></span>
* <span data-ttu-id="11b7c-159">Starta om en primär och sekundär replik (om en beständig tjänst)</span><span class="sxs-lookup"><span data-stu-id="11b7c-159">Restart a primary secondary replica (if a persisted service)</span></span>
* <span data-ttu-id="11b7c-160">Flytta en primär replik</span><span class="sxs-lookup"><span data-stu-id="11b7c-160">Move a primary replica</span></span>
* <span data-ttu-id="11b7c-161">Flytta en sekundär replik</span><span class="sxs-lookup"><span data-stu-id="11b7c-161">Move a secondary replica</span></span>
* <span data-ttu-id="11b7c-162">Starta om partitionen</span><span class="sxs-lookup"><span data-stu-id="11b7c-162">Restart the partition</span></span>

<span data-ttu-id="11b7c-163">Testet redundans startar ett valt fel och kör sedan verifieringen på en tjänst till dess stabilitet.</span><span class="sxs-lookup"><span data-stu-id="11b7c-163">The failover test induces a chosen fault and then runs validation on the service to ensure its stability.</span></span> <span data-ttu-id="11b7c-164">Testet redundans startar bara ett fel i taget, till skillnad mot möjliga flera fel i chaos test.</span><span class="sxs-lookup"><span data-stu-id="11b7c-164">The failover test induces only one fault at a time, as opposed to possible multiple faults in the chaos test.</span></span> <span data-ttu-id="11b7c-165">Testet misslyckas om tjänsten partitionen inte hålla inom den konfigurerade tidsgränsen efter varje fel.</span><span class="sxs-lookup"><span data-stu-id="11b7c-165">If the service partition does not stabilize within the configured timeout after each fault, the test fails.</span></span> <span data-ttu-id="11b7c-166">Testet startar endast säker fel.</span><span class="sxs-lookup"><span data-stu-id="11b7c-166">The test induces only safe faults.</span></span> <span data-ttu-id="11b7c-167">Det innebär att externa fel saknas, ett kvorum och förlust av data inte utförs.</span><span class="sxs-lookup"><span data-stu-id="11b7c-167">This means that in absence of external failures, a quorum or data loss will not occur.</span></span>

### <a name="important-configuration-options"></a><span data-ttu-id="11b7c-168">Viktiga konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="11b7c-168">Important configuration options</span></span>
* <span data-ttu-id="11b7c-169">**PartitionSelector**: Selector-objekt som anger den partition som ska gälla.</span><span class="sxs-lookup"><span data-stu-id="11b7c-169">**PartitionSelector**: Selector object that specifies the partition that needs to be targeted.</span></span>
* <span data-ttu-id="11b7c-170">**TimeToRun**: Total tid som testet ska köras innan du avslutar.</span><span class="sxs-lookup"><span data-stu-id="11b7c-170">**TimeToRun**: Total time that the test will run before finishing.</span></span>
* <span data-ttu-id="11b7c-171">**MaxServiceStabilizationTimeout**: maximal mängd väntetiden för att klustret ska bli felfri innan testet.</span><span class="sxs-lookup"><span data-stu-id="11b7c-171">**MaxServiceStabilizationTimeout**: Maximum amount of time to wait for the cluster to become healthy before failing the test.</span></span> <span data-ttu-id="11b7c-172">De kontroller som utförs är om tjänstens hälsa är OK, mål replikuppsättning uppnås för alla partitioner och inga InBuild-repliker finns.</span><span class="sxs-lookup"><span data-stu-id="11b7c-172">The checks performed are whether service health is OK, the target replica set size is achieved for all partitions, and no InBuild replicas exist.</span></span>
* <span data-ttu-id="11b7c-173">**WaitTimeBetweenFaults**: lång tid ska vänta mellan varje fel- och validering cykel.</span><span class="sxs-lookup"><span data-stu-id="11b7c-173">**WaitTimeBetweenFaults**: Amount of time to wait between every fault and validation cycle.</span></span>

### <a name="how-to-run-the-failover-test"></a><span data-ttu-id="11b7c-174">Hur du kör testet för växling vid fel</span><span class="sxs-lookup"><span data-stu-id="11b7c-174">How to run the failover test</span></span>
<span data-ttu-id="11b7c-175">**C#**</span><span class="sxs-lookup"><span data-stu-id="11b7c-175">**C#**</span></span>

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

        // The chaos test scenario should run at least 60 minutes or until it fails.
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

        // Create the scenario class and execute it asynchronously.
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


<span data-ttu-id="11b7c-176">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="11b7c-176">**PowerShell**</span></span>

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/SampleApp/SampleService"

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindSingleton
```
