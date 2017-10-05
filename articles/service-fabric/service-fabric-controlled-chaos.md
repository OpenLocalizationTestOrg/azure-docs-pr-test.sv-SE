---
title: Framkalla Chaos i Service Fabric-kluster | Microsoft Docs
description: "Använda fel Injection och klustret Analysis Service API: er för att hantera Chaos i klustret."
services: service-fabric
documentationcenter: .net
author: motanv
manager: anmola
editor: motanv
ms.assetid: 2bd13443-3478-4382-9a5a-1f6c6b32bfc9
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: motanv
ms.openlocfilehash: 3b3b93bc9ec5ecdcfc289e5b62e84de6aa4172ed
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a><span data-ttu-id="058ae-103">Framkalla kontrollerade Chaos i Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="058ae-103">Induce controlled Chaos in Service Fabric clusters</span></span>
<span data-ttu-id="058ae-104">Stora distribuerade system som molninfrastrukturer är natur instabilt.</span><span class="sxs-lookup"><span data-stu-id="058ae-104">Large-scale distributed systems like cloud infrastructures are inherently unreliable.</span></span> <span data-ttu-id="058ae-105">Azure Service Fabric kan utvecklare skriva tillförlitliga distribuerade tjänster på ett instabilt infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="058ae-105">Azure Service Fabric enables developers to write reliable distributed services on top of an unreliable infrastructure.</span></span> <span data-ttu-id="058ae-106">Om du vill skriva robust distribuerade tjänster på en infrastruktur med instabilt behöver utvecklare för att kunna testa stabiliteten i sina tjänster medan den underliggande instabilt infrastrukturen gå igenom komplicerade tillståndsövergångar på grund av fel.</span><span class="sxs-lookup"><span data-stu-id="058ae-106">To write robust distributed services on top of an unreliable infrastructure, developers need to be able to test the stability of their services while the underlying unreliable infrastructure is going through complicated state transitions due to faults.</span></span>

<span data-ttu-id="058ae-107">Den [fel Injection och analys klustertjänsten](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (även kallat fel Analysis Service) gör att utvecklare kan orsaka fel för att testa sina tjänster.</span><span class="sxs-lookup"><span data-stu-id="058ae-107">The [Fault Injection and Cluster Analysis Service](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (also known as the Fault Analysis Service) gives developers the ability to induce faults to test their services.</span></span> <span data-ttu-id="058ae-108">Dessa mål simulerade fel, som [startar om en partition](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), kan hjälpa dig att utöva de vanligaste tillståndsövergångar.</span><span class="sxs-lookup"><span data-stu-id="058ae-108">These targeted simulated faults, like [restarting a partition](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), can help exercise the most common state transitions.</span></span> <span data-ttu-id="058ae-109">Men buggar riktade simulerade fel prioriterar per definition och därmed missa som visar upp endast i tillståndsövergångar svårt att förutse, långa och komplicerade ordning.</span><span class="sxs-lookup"><span data-stu-id="058ae-109">However targeted simulated faults are biased by definition and thus may miss bugs that show up only in hard-to-predict, long and complicated sequence of state transitions.</span></span> <span data-ttu-id="058ae-110">Du kan använda Chaos för en oprioriterad testning.</span><span class="sxs-lookup"><span data-stu-id="058ae-110">For an unbiased testing, you can use Chaos.</span></span>

<span data-ttu-id="058ae-111">Chaos simulerar periodiska, överlagrad fel (korrekt och städat) i hela klustret under en längre tid.</span><span class="sxs-lookup"><span data-stu-id="058ae-111">Chaos simulates periodic, interleaved faults (both graceful and ungraceful) throughout the cluster over extended periods of time.</span></span> <span data-ttu-id="058ae-112">När du har konfigurerat Chaos med frekvensen och typ av fel, kan du starta Chaos via C# eller Powershell API ska börja generera fel i klustret och dina tjänster.</span><span class="sxs-lookup"><span data-stu-id="058ae-112">Once you have configured Chaos with the rate and the kind of faults, you can start Chaos through C# or Powershell API to start generating faults in the cluster and in your services.</span></span> <span data-ttu-id="058ae-113">Du kan konfigurera Chaos ska köras under en angiven tidsperiod (till exempel under en timme), efter vilken Chaos stoppar automatiskt eller så kan du anropa StopChaos API (C# eller Powershell) slutar när som helst.</span><span class="sxs-lookup"><span data-stu-id="058ae-113">You can configure Chaos to run for a specified time period (for example, for one hour), after which Chaos stops automatically, or you can call StopChaos API (C# or Powershell) to stop it at any time.</span></span>

> [!NOTE]
> <span data-ttu-id="058ae-114">I sin nuvarande form startar Chaos endast säker fel, vilket innebär att om externa fel en förlorar kvorum eller förlust av data sker aldrig.</span><span class="sxs-lookup"><span data-stu-id="058ae-114">In its current form, Chaos induces only safe faults, which implies that in the absence of external faults a quorum loss, or data loss never occurs.</span></span>
>

<span data-ttu-id="058ae-115">När Chaos körs, ger olika händelser som Spara tillståndet för körs för tillfället.</span><span class="sxs-lookup"><span data-stu-id="058ae-115">While Chaos is running, it produces different events that capture the state of the run at the moment.</span></span> <span data-ttu-id="058ae-116">En ExecutingFaultsEvent innehåller till exempel alla fel som Chaos har valt att köra i den iterationen.</span><span class="sxs-lookup"><span data-stu-id="058ae-116">For example, an ExecutingFaultsEvent contains all the faults that Chaos has decided to execute in that iteration.</span></span> <span data-ttu-id="058ae-117">En ValidationFailedEvent innehåller information om valideringsfelet (hälsa eller stabilitet problem) som hittades under valideringen av klustret.</span><span class="sxs-lookup"><span data-stu-id="058ae-117">A ValidationFailedEvent contains the details of a validation failure (health or stability issues) that was found during the validation of the cluster.</span></span> <span data-ttu-id="058ae-118">Du kan anropa GetChaosReport API (C# eller Powershell) för att få rapporten Chaos körs.</span><span class="sxs-lookup"><span data-stu-id="058ae-118">You can invoke the GetChaosReport API (C# or Powershell) to get the report of Chaos runs.</span></span> <span data-ttu-id="058ae-119">Dessa händelser hämta kvar i en [tillförlitliga ordlista](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), som har en trunkering princip enligt två konfigurationer: **MaxStoredChaosEventCount** (standardvärdet är 25000) och **StoredActionCleanupIntervalInSeconds** (standardvärdet är 3600).</span><span class="sxs-lookup"><span data-stu-id="058ae-119">These events get persisted in a [reliable dictionary](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), which has a truncation policy dictated by two configurations: **MaxStoredChaosEventCount** (default value is 25000) and **StoredActionCleanupIntervalInSeconds** (default value is 3600).</span></span> <span data-ttu-id="058ae-120">Varje *StoredActionCleanupIntervalInSeconds* Chaos kontroller och alla men den senaste *MaxStoredChaosEventCount* händelser, rensas från tillförlitliga ordlistan.</span><span class="sxs-lookup"><span data-stu-id="058ae-120">Every *StoredActionCleanupIntervalInSeconds* Chaos checks and all but the most recent *MaxStoredChaosEventCount* events, are purged from the reliable dictionary.</span></span>

## <a name="faults-induced-in-chaos"></a><span data-ttu-id="058ae-121">Fel i Chaos</span><span class="sxs-lookup"><span data-stu-id="058ae-121">Faults induced in Chaos</span></span>
<span data-ttu-id="058ae-122">Chaos genererar fel över hela Service Fabric-kluster och komprimerar fel som visas i månader eller år till några timmar.</span><span class="sxs-lookup"><span data-stu-id="058ae-122">Chaos generates faults across the entire Service Fabric cluster and compresses faults that are seen in months or years into a few hours.</span></span> <span data-ttu-id="058ae-123">Kombinationen av överlagrad fel med hög feltolerans hastighet hittar specialfall som annars kan missas.</span><span class="sxs-lookup"><span data-stu-id="058ae-123">The combination of interleaved faults with the high fault rate finds corner cases that may otherwise be missed.</span></span> <span data-ttu-id="058ae-124">Den här övningen kaotisk leder till en betydande förbättringar i koden Tjänstkvalitet.</span><span class="sxs-lookup"><span data-stu-id="058ae-124">This exercise of Chaos leads to a significant improvement in the code quality of the service.</span></span>

<span data-ttu-id="058ae-125">Chaos startar fel i följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="058ae-125">Chaos induces faults from the following categories:</span></span>

* <span data-ttu-id="058ae-126">Starta om en nod</span><span class="sxs-lookup"><span data-stu-id="058ae-126">Restart a node</span></span>
* <span data-ttu-id="058ae-127">Starta om distribuerade kodpaketet</span><span class="sxs-lookup"><span data-stu-id="058ae-127">Restart a deployed code package</span></span>
* <span data-ttu-id="058ae-128">Ta bort en replik</span><span class="sxs-lookup"><span data-stu-id="058ae-128">Remove a replica</span></span>
* <span data-ttu-id="058ae-129">Starta om en replik</span><span class="sxs-lookup"><span data-stu-id="058ae-129">Restart a replica</span></span>
* <span data-ttu-id="058ae-130">Flytta en primär replik (konfigureras)</span><span class="sxs-lookup"><span data-stu-id="058ae-130">Move a primary replica (configurable)</span></span>
* <span data-ttu-id="058ae-131">Flytta en sekundär replik (konfigureras)</span><span class="sxs-lookup"><span data-stu-id="058ae-131">Move a secondary replica (configurable)</span></span>

<span data-ttu-id="058ae-132">Chaos körs i flera iterationer.</span><span class="sxs-lookup"><span data-stu-id="058ae-132">Chaos runs in multiple iterations.</span></span> <span data-ttu-id="058ae-133">Varje iteration består av fel och klusterverifieringen för den angivna perioden.</span><span class="sxs-lookup"><span data-stu-id="058ae-133">Each iteration consists of faults and cluster validation for the specified period.</span></span> <span data-ttu-id="058ae-134">Du kan konfigurera den tid som krävs att hålla klustret och för verifiering ska lyckas.</span><span class="sxs-lookup"><span data-stu-id="058ae-134">You can configure the time spent for the cluster to stabilize and for validation to succeed.</span></span> <span data-ttu-id="058ae-135">Om det finns ett fel i klusterverifieringen Chaos genererar och en ValidationFailedEvent med UTC-tidsstämpel och information om felet kvarstår.</span><span class="sxs-lookup"><span data-stu-id="058ae-135">If a failure is found in cluster validation, Chaos generates and persists a ValidationFailedEvent with the UTC timestamp and the failure details.</span></span> <span data-ttu-id="058ae-136">Anta till exempel att en instans av Chaos som är konfigurerat för körning i en timma med högst tre samtidiga fel.</span><span class="sxs-lookup"><span data-stu-id="058ae-136">For example, consider an instance of Chaos that is set to run for an hour with a maximum of three concurrent faults.</span></span> <span data-ttu-id="058ae-137">Chaos startar tre fel och verifierar hälsa för klustret.</span><span class="sxs-lookup"><span data-stu-id="058ae-137">Chaos induces three faults, and then validates the cluster health.</span></span> <span data-ttu-id="058ae-138">Det går igenom den tidigare skickar steg tills den är uttryckligen stoppad via StopChaosAsync API eller en timme.</span><span class="sxs-lookup"><span data-stu-id="058ae-138">It iterates through the previous step until it is explicitly stopped through the StopChaosAsync API or one-hour passes.</span></span> <span data-ttu-id="058ae-139">Om klustret blir ohälsosamt i varje iteration (dvs, den inte hålla inom MaxClusterStabilizationTimeout skickades i), Chaos genererar en ValidationFailedEvent.</span><span class="sxs-lookup"><span data-stu-id="058ae-139">If the cluster becomes unhealthy in any iteration (that is, it does not stabilize within the passed-in MaxClusterStabilizationTimeout), Chaos generates a ValidationFailedEvent.</span></span> <span data-ttu-id="058ae-140">Den här händelsen tyder på att något är fel kan behöva ytterligare undersökning.</span><span class="sxs-lookup"><span data-stu-id="058ae-140">This event indicates that something has gone wrong and might need further investigation.</span></span>

<span data-ttu-id="058ae-141">Du kan använda GetChaosReport API (powershell eller C#) för att få vilka fel Chaos framkallas.</span><span class="sxs-lookup"><span data-stu-id="058ae-141">To get which faults Chaos induced, you can use GetChaosReport API (powershell or C#).</span></span> <span data-ttu-id="058ae-142">API: et hämtar Chaos rapporten utifrån skickades i fortsättningstoken eller skickades i-tidsintervallet nästa segment.</span><span class="sxs-lookup"><span data-stu-id="058ae-142">The API gets the next segment of the Chaos report based on the passed-in continuation token or the passed-in time-range.</span></span> <span data-ttu-id="058ae-143">Du kan antingen ange ContinuationToken för att få rapporten Chaos nästa segment eller du kan ange tidsintervall via StartTimeUtc och EndTimeUtc, men du kan inte ange både ContinuationToken och tidsintervallet i samma anropet.</span><span class="sxs-lookup"><span data-stu-id="058ae-143">You can either specify the ContinuationToken to get the next segment of the Chaos report or you can specify the time-range through StartTimeUtc and EndTimeUtc, but you cannot specify both the ContinuationToken and the time-range in the same call.</span></span> <span data-ttu-id="058ae-144">När det finns fler än 100 Chaos händelser, returneras Chaos rapporten segment där ett segment innehåller fler än 100 Chaos händelser.</span><span class="sxs-lookup"><span data-stu-id="058ae-144">When there are more than 100 Chaos events, the Chaos report is returned in segments where a segment contains no more than 100 Chaos events.</span></span>

## <a name="important-configuration-options"></a><span data-ttu-id="058ae-145">Viktiga konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="058ae-145">Important configuration options</span></span>
* <span data-ttu-id="058ae-146">**TimeToRun**: Total tid som Chaos kör innan den är klar med framgång.</span><span class="sxs-lookup"><span data-stu-id="058ae-146">**TimeToRun**: Total time that Chaos runs before it finishes with success.</span></span> <span data-ttu-id="058ae-147">Du kan stoppa Chaos innan den har körts för perioden TimeToRun via StopChaos-API.</span><span class="sxs-lookup"><span data-stu-id="058ae-147">You can stop Chaos before it has run for the TimeToRun period through the StopChaos API.</span></span>

* <span data-ttu-id="058ae-148">**MaxClusterStabilizationTimeout**: maximal mängd väntetiden för att klustret ska bli felfri innan du skapar en ValidationFailedEvent.</span><span class="sxs-lookup"><span data-stu-id="058ae-148">**MaxClusterStabilizationTimeout**: The maximum amount of time to wait for the cluster to become healthy before producing a ValidationFailedEvent.</span></span> <span data-ttu-id="058ae-149">Den här vänta är att minska belastningen på klustret när den återställs.</span><span class="sxs-lookup"><span data-stu-id="058ae-149">This wait is to reduce the load on the cluster while it is recovering.</span></span> <span data-ttu-id="058ae-150">De kontroller som utförs är:</span><span class="sxs-lookup"><span data-stu-id="058ae-150">The checks performed are:</span></span>
  * <span data-ttu-id="058ae-151">Om klustret hälsa är OK</span><span class="sxs-lookup"><span data-stu-id="058ae-151">If the cluster health is OK</span></span>
  * <span data-ttu-id="058ae-152">Om tjänstens hälsa är OK</span><span class="sxs-lookup"><span data-stu-id="058ae-152">If the service health is OK</span></span>
  * <span data-ttu-id="058ae-153">Om mål replikuppsättningen storleken uppnås för service-partition</span><span class="sxs-lookup"><span data-stu-id="058ae-153">If the target replica set size is achieved for the service partition</span></span>
  * <span data-ttu-id="058ae-154">Att det inte finns några InBuild-repliker</span><span class="sxs-lookup"><span data-stu-id="058ae-154">That no InBuild replicas exist</span></span>
* <span data-ttu-id="058ae-155">**MaxConcurrentFaults**: det maximala antalet samtidiga fel som framkallas i varje iteration.</span><span class="sxs-lookup"><span data-stu-id="058ae-155">**MaxConcurrentFaults**: The maximum number of concurrent faults that are induced in each iteration.</span></span> <span data-ttu-id="058ae-156">Högre nummer, desto mer aggressivt kaos är redundansväxlingarna och tillstånd övergången kombinationer som klustret passerar är också mer komplexa.</span><span class="sxs-lookup"><span data-stu-id="058ae-156">The higher the number, the more aggressive Chaos is and the failovers and the state transition combinations that the cluster goes through are also more complex.</span></span> 

> [!NOTE]
> <span data-ttu-id="058ae-157">Oavsett hur högt värde *MaxConcurrentFaults* har Chaos garanterar - om externa fel - det finns ingen förlorar kvorum eller förlust av data.</span><span class="sxs-lookup"><span data-stu-id="058ae-157">Regardless how high a value *MaxConcurrentFaults* has, Chaos guarantees - in the absence of external faults - there is no quorum loss or data loss.</span></span>
>

* <span data-ttu-id="058ae-158">**EnableMoveReplicaFaults**: aktiverar eller inaktiverar de fel som orsakar att flytta primära eller sekundära replikerna.</span><span class="sxs-lookup"><span data-stu-id="058ae-158">**EnableMoveReplicaFaults**: Enables or disables the faults that cause the primary or secondary replicas to move.</span></span> <span data-ttu-id="058ae-159">Dessa fel är inaktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="058ae-159">These faults are disabled by default.</span></span>
* <span data-ttu-id="058ae-160">**WaitTimeBetweenIterations**: tidsperiod som ska förflyta mellan iterationer.</span><span class="sxs-lookup"><span data-stu-id="058ae-160">**WaitTimeBetweenIterations**: The amount of time to wait between iterations.</span></span> <span data-ttu-id="058ae-161">Det vill säga pausar hur lång tid Chaos efter att ha utförs en runda av fel och har slutförts med motsvarande verifieringen av hälsotillståndet för klustret.</span><span class="sxs-lookup"><span data-stu-id="058ae-161">That is, the amount of time Chaos will pause after having executed a round of faults and having finished the corresponding validation of the health of the cluster.</span></span> <span data-ttu-id="058ae-162">Ju högre värdet Ju lägre är den genomsnittliga fel injection hastigheten.</span><span class="sxs-lookup"><span data-stu-id="058ae-162">The higher the value, the lower is the average fault injection rate.</span></span>
* <span data-ttu-id="058ae-163">**WaitTimeBetweenFaults**: tidsperiod som ska förflyta mellan två på varandra följande fel i en enda iteration.</span><span class="sxs-lookup"><span data-stu-id="058ae-163">**WaitTimeBetweenFaults**: The amount of time to wait between two consecutive faults in a single iteration.</span></span> <span data-ttu-id="058ae-164">Ju högre värdet är, desto lägre samtidighet på (eller överlapp mellan) hos.</span><span class="sxs-lookup"><span data-stu-id="058ae-164">The higher the value, the lower the concurrency of (or the overlap between) faults.</span></span>
* <span data-ttu-id="058ae-165">**ClusterHealthPolicy**: klustret hälsoprincip används för att kontrollera hälsotillståndet för klustret between Chaos iterationer.</span><span class="sxs-lookup"><span data-stu-id="058ae-165">**ClusterHealthPolicy**: Cluster health policy is used to validate the health of the cluster in between Chaos iterations.</span></span> <span data-ttu-id="058ae-166">Om klustret hälsa är felaktigt eller om ett oväntat undantag inträffar under körning av fel Chaos ska vänta i 30 minuter innan nästa-hälsotillståndskontroll - att ge lite tid att recuperate klustret.</span><span class="sxs-lookup"><span data-stu-id="058ae-166">If the cluster health is in error or if an unexpected exception happens during fault execution, Chaos will wait for 30 minutes before the next health-check - to provide the cluster with some time to recuperate.</span></span>
* <span data-ttu-id="058ae-167">**Kontexten**: en samling (sträng, sträng) anger nyckel-värdepar.</span><span class="sxs-lookup"><span data-stu-id="058ae-167">**Context**: A collection of (string, string) type key-value pairs.</span></span> <span data-ttu-id="058ae-168">Kartan kan användas för att registrera information om Chaos kör.</span><span class="sxs-lookup"><span data-stu-id="058ae-168">The map can be used to record information about the Chaos run.</span></span> <span data-ttu-id="058ae-169">Det får inte finnas fler än 100 par och varje sträng (nyckel eller ett värde) får innehålla högst 4095 tecken.</span><span class="sxs-lookup"><span data-stu-id="058ae-169">There cannot be more than 100 such pairs and each string (key or value) can be at most 4095 characters long.</span></span> <span data-ttu-id="058ae-170">Den här kartan anges av starter kaotisk kör för att lagra kontexten om specifika kör.</span><span class="sxs-lookup"><span data-stu-id="058ae-170">This map is set by the starter of the Chaos run to optionally store the context about the specific run.</span></span>

## <a name="how-to-run-chaos"></a><span data-ttu-id="058ae-171">Hur du kör Chaos</span><span class="sxs-lookup"><span data-stu-id="058ae-171">How to run Chaos</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.Add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
