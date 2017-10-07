---
title: aaaInduce Chaos i Service Fabric-kluster | Microsoft Docs
description: "Med hjälp av fel Injection och klustret Analysis Service API: er toomanage Chaos i hello kluster."
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
ms.openlocfilehash: 7e87cae22645fc4ba52e258471d8f3a4ffdb1cce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a><span data-ttu-id="1d08c-103">Framkalla kontrollerade Chaos i Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="1d08c-103">Induce controlled Chaos in Service Fabric clusters</span></span>
<span data-ttu-id="1d08c-104">Stora distribuerade system som molninfrastrukturer är natur instabilt.</span><span class="sxs-lookup"><span data-stu-id="1d08c-104">Large-scale distributed systems like cloud infrastructures are inherently unreliable.</span></span> <span data-ttu-id="1d08c-105">Azure Service Fabric kan utvecklare toowrite tillförlitliga distribuerade tjänster på ett instabilt infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="1d08c-105">Azure Service Fabric enables developers toowrite reliable distributed services on top of an unreliable infrastructure.</span></span> <span data-ttu-id="1d08c-106">toowrite robust distribuerade tjänster på ett instabilt infrastruktur, utvecklare behöver toobe kan tootest hello stabiliteten i sina tjänster medan hello underliggande instabilt infrastruktur gå igenom komplicerade tillståndsövergångar på grund av toofaults.</span><span class="sxs-lookup"><span data-stu-id="1d08c-106">toowrite robust distributed services on top of an unreliable infrastructure, developers need toobe able tootest hello stability of their services while hello underlying unreliable infrastructure is going through complicated state transitions due toofaults.</span></span>

<span data-ttu-id="1d08c-107">Hej [fel Injection och analys klustertjänsten](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (även kallat hello fel Analysis Service) ger utvecklare möjligheten för hello tooinduce hos tootest sina tjänster.</span><span class="sxs-lookup"><span data-stu-id="1d08c-107">hello [Fault Injection and Cluster Analysis Service](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (also known as hello Fault Analysis Service) gives developers hello ability tooinduce faults tootest their services.</span></span> <span data-ttu-id="1d08c-108">Dessa mål simulerade fel, som [startar om en partition](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), kan hjälpa dig att utöva hello vanligaste tillståndsövergångar.</span><span class="sxs-lookup"><span data-stu-id="1d08c-108">These targeted simulated faults, like [restarting a partition](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), can help exercise hello most common state transitions.</span></span> <span data-ttu-id="1d08c-109">Men buggar riktade simulerade fel prioriterar per definition och därmed missa som visar upp endast i tillståndsövergångar svårt att förutse, långa och komplicerade ordning.</span><span class="sxs-lookup"><span data-stu-id="1d08c-109">However targeted simulated faults are biased by definition and thus may miss bugs that show up only in hard-to-predict, long and complicated sequence of state transitions.</span></span> <span data-ttu-id="1d08c-110">Du kan använda Chaos för en oprioriterad testning.</span><span class="sxs-lookup"><span data-stu-id="1d08c-110">For an unbiased testing, you can use Chaos.</span></span>

<span data-ttu-id="1d08c-111">Chaos simulerar periodiska, överlagrad fel (korrekt och städat) under hela hello kluster under en längre tid.</span><span class="sxs-lookup"><span data-stu-id="1d08c-111">Chaos simulates periodic, interleaved faults (both graceful and ungraceful) throughout hello cluster over extended periods of time.</span></span> <span data-ttu-id="1d08c-112">När du har konfigurerat Chaos med hello hastighet och hello slags fel, kan du starta Chaos via C# eller Powershell API toostart genererar fel i hello kluster och i dina tjänster.</span><span class="sxs-lookup"><span data-stu-id="1d08c-112">Once you have configured Chaos with hello rate and hello kind of faults, you can start Chaos through C# or Powershell API toostart generating faults in hello cluster and in your services.</span></span> <span data-ttu-id="1d08c-113">Du kan konfigurera Chaos toorun för en angiven tidsperiod (till exempel under en timme), efter vilken Chaos stoppar automatiskt, eller så kan du anropa StopChaos API (C# eller Powershell) toostop den när som helst.</span><span class="sxs-lookup"><span data-stu-id="1d08c-113">You can configure Chaos toorun for a specified time period (for example, for one hour), after which Chaos stops automatically, or you can call StopChaos API (C# or Powershell) toostop it at any time.</span></span>

> [!NOTE]
> <span data-ttu-id="1d08c-114">I sin nuvarande form startar Chaos endast säker fel, vilket innebär att externa fel hello saknas en förlorar kvorum eller förlust av data sker aldrig.</span><span class="sxs-lookup"><span data-stu-id="1d08c-114">In its current form, Chaos induces only safe faults, which implies that in hello absence of external faults a quorum loss, or data loss never occurs.</span></span>
>

<span data-ttu-id="1d08c-115">När Chaos körs, ger olika händelser som samlar in hello kör för tillfället hello hello tillstånd.</span><span class="sxs-lookup"><span data-stu-id="1d08c-115">While Chaos is running, it produces different events that capture hello state of hello run at hello moment.</span></span> <span data-ttu-id="1d08c-116">En ExecutingFaultsEvent innehåller till exempel alla hello fel Chaos beslutat tooexecute i den iterationen.</span><span class="sxs-lookup"><span data-stu-id="1d08c-116">For example, an ExecutingFaultsEvent contains all hello faults that Chaos has decided tooexecute in that iteration.</span></span> <span data-ttu-id="1d08c-117">En ValidationFailedEvent innehåller hello information om valideringsfelet (hälsa eller stabilitet problem) som hittades vid verifiering av hello hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="1d08c-117">A ValidationFailedEvent contains hello details of a validation failure (health or stability issues) that was found during hello validation of hello cluster.</span></span> <span data-ttu-id="1d08c-118">Du kan anropa hello GetChaosReport API (C# eller Powershell) tooget hello rapport över Chaos körs.</span><span class="sxs-lookup"><span data-stu-id="1d08c-118">You can invoke hello GetChaosReport API (C# or Powershell) tooget hello report of Chaos runs.</span></span> <span data-ttu-id="1d08c-119">Dessa händelser hämta kvar i en [tillförlitliga ordlista](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), som har en trunkering princip enligt två konfigurationer: **MaxStoredChaosEventCount** (standardvärdet är 25000) och **StoredActionCleanupIntervalInSeconds** (standardvärdet är 3600).</span><span class="sxs-lookup"><span data-stu-id="1d08c-119">These events get persisted in a [reliable dictionary](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), which has a truncation policy dictated by two configurations: **MaxStoredChaosEventCount** (default value is 25000) and **StoredActionCleanupIntervalInSeconds** (default value is 3600).</span></span> <span data-ttu-id="1d08c-120">Varje *StoredActionCleanupIntervalInSeconds* Chaos kontroller och alla men hello senaste *MaxStoredChaosEventCount* händelser, rensas från hello tillförlitliga ordlistan.</span><span class="sxs-lookup"><span data-stu-id="1d08c-120">Every *StoredActionCleanupIntervalInSeconds* Chaos checks and all but hello most recent *MaxStoredChaosEventCount* events, are purged from hello reliable dictionary.</span></span>

## <a name="faults-induced-in-chaos"></a><span data-ttu-id="1d08c-121">Fel i Chaos</span><span class="sxs-lookup"><span data-stu-id="1d08c-121">Faults induced in Chaos</span></span>
<span data-ttu-id="1d08c-122">Chaos genererar fel över hello hela Service Fabric-kluster och komprimerar fel som visas i månader eller år till några timmar.</span><span class="sxs-lookup"><span data-stu-id="1d08c-122">Chaos generates faults across hello entire Service Fabric cluster and compresses faults that are seen in months or years into a few hours.</span></span> <span data-ttu-id="1d08c-123">hello kombination av överlagrad fel med hög feltolerans hello hittar specialfall som annars kan missas.</span><span class="sxs-lookup"><span data-stu-id="1d08c-123">hello combination of interleaved faults with hello high fault rate finds corner cases that may otherwise be missed.</span></span> <span data-ttu-id="1d08c-124">Den här övningen kaotisk leads tooa betydande förbättringar i hello kod tjänstkvalitet hello.</span><span class="sxs-lookup"><span data-stu-id="1d08c-124">This exercise of Chaos leads tooa significant improvement in hello code quality of hello service.</span></span>

<span data-ttu-id="1d08c-125">Chaos startar fel från hello följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="1d08c-125">Chaos induces faults from hello following categories:</span></span>

* <span data-ttu-id="1d08c-126">Starta om en nod</span><span class="sxs-lookup"><span data-stu-id="1d08c-126">Restart a node</span></span>
* <span data-ttu-id="1d08c-127">Starta om distribuerade kodpaketet</span><span class="sxs-lookup"><span data-stu-id="1d08c-127">Restart a deployed code package</span></span>
* <span data-ttu-id="1d08c-128">Ta bort en replik</span><span class="sxs-lookup"><span data-stu-id="1d08c-128">Remove a replica</span></span>
* <span data-ttu-id="1d08c-129">Starta om en replik</span><span class="sxs-lookup"><span data-stu-id="1d08c-129">Restart a replica</span></span>
* <span data-ttu-id="1d08c-130">Flytta en primär replik (konfigureras)</span><span class="sxs-lookup"><span data-stu-id="1d08c-130">Move a primary replica (configurable)</span></span>
* <span data-ttu-id="1d08c-131">Flytta en sekundär replik (konfigureras)</span><span class="sxs-lookup"><span data-stu-id="1d08c-131">Move a secondary replica (configurable)</span></span>

<span data-ttu-id="1d08c-132">Chaos körs i flera iterationer.</span><span class="sxs-lookup"><span data-stu-id="1d08c-132">Chaos runs in multiple iterations.</span></span> <span data-ttu-id="1d08c-133">Varje iteration består av fel och klusterverifieringen för hello angett period.</span><span class="sxs-lookup"><span data-stu-id="1d08c-133">Each iteration consists of faults and cluster validation for hello specified period.</span></span> <span data-ttu-id="1d08c-134">Du kan konfigurera hello tidsåtgången för hello klustret toostabilize och validering toosucceed.</span><span class="sxs-lookup"><span data-stu-id="1d08c-134">You can configure hello time spent for hello cluster toostabilize and for validation toosucceed.</span></span> <span data-ttu-id="1d08c-135">Om det finns ett fel i klusterverifieringen Chaos genererar och en ValidationFailedEvent med hello UTC-tidsstämpel och hello information om felet kvarstår.</span><span class="sxs-lookup"><span data-stu-id="1d08c-135">If a failure is found in cluster validation, Chaos generates and persists a ValidationFailedEvent with hello UTC timestamp and hello failure details.</span></span> <span data-ttu-id="1d08c-136">Anta till exempel att en instans av Chaos som har angetts toorun för en timme med högst tre samtidiga fel.</span><span class="sxs-lookup"><span data-stu-id="1d08c-136">For example, consider an instance of Chaos that is set toorun for an hour with a maximum of three concurrent faults.</span></span> <span data-ttu-id="1d08c-137">Chaos startar tre fel och validerar hello klustret hälsa.</span><span class="sxs-lookup"><span data-stu-id="1d08c-137">Chaos induces three faults, and then validates hello cluster health.</span></span> <span data-ttu-id="1d08c-138">Det går igenom hello föregående steg tills den är uttryckligen stoppad via hello StopChaosAsync API eller en timme skickar.</span><span class="sxs-lookup"><span data-stu-id="1d08c-138">It iterates through hello previous step until it is explicitly stopped through hello StopChaosAsync API or one-hour passes.</span></span> <span data-ttu-id="1d08c-139">Om hello klustret blir ohälsosamt i varje iteration (dvs, den inte hålla inom hello skickades i MaxClusterStabilizationTimeout), Chaos genererar en ValidationFailedEvent.</span><span class="sxs-lookup"><span data-stu-id="1d08c-139">If hello cluster becomes unhealthy in any iteration (that is, it does not stabilize within hello passed-in MaxClusterStabilizationTimeout), Chaos generates a ValidationFailedEvent.</span></span> <span data-ttu-id="1d08c-140">Den här händelsen tyder på att något är fel kan behöva ytterligare undersökning.</span><span class="sxs-lookup"><span data-stu-id="1d08c-140">This event indicates that something has gone wrong and might need further investigation.</span></span>

<span data-ttu-id="1d08c-141">Du kan använda GetChaosReport API (powershell eller C#) tooget som hos Chaos framkallas.</span><span class="sxs-lookup"><span data-stu-id="1d08c-141">tooget which faults Chaos induced, you can use GetChaosReport API (powershell or C#).</span></span> <span data-ttu-id="1d08c-142">hello API hämtar hello nästa segment av hello Chaos rapporter utifrån hello skickades i fortsättningstoken eller hello skickades i-tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="1d08c-142">hello API gets hello next segment of hello Chaos report based on hello passed-in continuation token or hello passed-in time-range.</span></span> <span data-ttu-id="1d08c-143">Du kan antingen ange hello ContinuationToken tooget hello nästa segment av hello Chaos rapport eller du kan ange hello-tidsintervall via StartTimeUtc och EndTimeUtc, men du kan inte ange både hello ContinuationToken och hello tidsintervall i hello samma anrop.</span><span class="sxs-lookup"><span data-stu-id="1d08c-143">You can either specify hello ContinuationToken tooget hello next segment of hello Chaos report or you can specify hello time-range through StartTimeUtc and EndTimeUtc, but you cannot specify both hello ContinuationToken and hello time-range in hello same call.</span></span> <span data-ttu-id="1d08c-144">När det finns fler än 100 Chaos händelser, returneras hello Chaos rapporten segment där ett segment innehåller fler än 100 Chaos händelser.</span><span class="sxs-lookup"><span data-stu-id="1d08c-144">When there are more than 100 Chaos events, hello Chaos report is returned in segments where a segment contains no more than 100 Chaos events.</span></span>

## <a name="important-configuration-options"></a><span data-ttu-id="1d08c-145">Viktiga konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="1d08c-145">Important configuration options</span></span>
* <span data-ttu-id="1d08c-146">**TimeToRun**: Total tid som Chaos kör innan den är klar med framgång.</span><span class="sxs-lookup"><span data-stu-id="1d08c-146">**TimeToRun**: Total time that Chaos runs before it finishes with success.</span></span> <span data-ttu-id="1d08c-147">Du kan stoppa Chaos innan den har körts under hello TimeToRun via hello StopChaos API.</span><span class="sxs-lookup"><span data-stu-id="1d08c-147">You can stop Chaos before it has run for hello TimeToRun period through hello StopChaos API.</span></span>

* <span data-ttu-id="1d08c-148">**MaxClusterStabilizationTimeout**: hello maximala mängden tid toowait för hello klustret toobecome felfri innan du skapar en ValidationFailedEvent.</span><span class="sxs-lookup"><span data-stu-id="1d08c-148">**MaxClusterStabilizationTimeout**: hello maximum amount of time toowait for hello cluster toobecome healthy before producing a ValidationFailedEvent.</span></span> <span data-ttu-id="1d08c-149">Den här vänta är tooreduce hello belastningen på hello klustret medan den återställs.</span><span class="sxs-lookup"><span data-stu-id="1d08c-149">This wait is tooreduce hello load on hello cluster while it is recovering.</span></span> <span data-ttu-id="1d08c-150">hello kontrollerna utförs är:</span><span class="sxs-lookup"><span data-stu-id="1d08c-150">hello checks performed are:</span></span>
  * <span data-ttu-id="1d08c-151">Om hello klustret hälsa är OK</span><span class="sxs-lookup"><span data-stu-id="1d08c-151">If hello cluster health is OK</span></span>
  * <span data-ttu-id="1d08c-152">Om hello tjänstens hälsa är OK</span><span class="sxs-lookup"><span data-stu-id="1d08c-152">If hello service health is OK</span></span>
  * <span data-ttu-id="1d08c-153">Om hello mål replikuppsättningen storleken uppnås för hello service partition</span><span class="sxs-lookup"><span data-stu-id="1d08c-153">If hello target replica set size is achieved for hello service partition</span></span>
  * <span data-ttu-id="1d08c-154">Att det inte finns några InBuild-repliker</span><span class="sxs-lookup"><span data-stu-id="1d08c-154">That no InBuild replicas exist</span></span>
* <span data-ttu-id="1d08c-155">**MaxConcurrentFaults**: hello maximalt antal samtidiga fel som framkallas i varje iteration.</span><span class="sxs-lookup"><span data-stu-id="1d08c-155">**MaxConcurrentFaults**: hello maximum number of concurrent faults that are induced in each iteration.</span></span> <span data-ttu-id="1d08c-156">hello högre hello nummer, hello mer aggressivt Chaos är och hello redundans och hello tillstånd övergången kombinationer som hello klustret passerar är också mer komplexa.</span><span class="sxs-lookup"><span data-stu-id="1d08c-156">hello higher hello number, hello more aggressive Chaos is and hello failovers and hello state transition combinations that hello cluster goes through are also more complex.</span></span> 

> [!NOTE]
> <span data-ttu-id="1d08c-157">Oavsett hur högt värde *MaxConcurrentFaults* har Chaos garanterar - hello saknas externa fel - det finns ingen förlorar kvorum eller förlust av data.</span><span class="sxs-lookup"><span data-stu-id="1d08c-157">Regardless how high a value *MaxConcurrentFaults* has, Chaos guarantees - in hello absence of external faults - there is no quorum loss or data loss.</span></span>
>

* <span data-ttu-id="1d08c-158">**EnableMoveReplicaFaults**: aktiverar eller inaktiverar hello-fel som orsakar hello primära eller sekundära repliker toomove.</span><span class="sxs-lookup"><span data-stu-id="1d08c-158">**EnableMoveReplicaFaults**: Enables or disables hello faults that cause hello primary or secondary replicas toomove.</span></span> <span data-ttu-id="1d08c-159">Dessa fel är inaktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="1d08c-159">These faults are disabled by default.</span></span>
* <span data-ttu-id="1d08c-160">**WaitTimeBetweenIterations**: hello mängden tid toowait mellan iterationer.</span><span class="sxs-lookup"><span data-stu-id="1d08c-160">**WaitTimeBetweenIterations**: hello amount of time toowait between iterations.</span></span> <span data-ttu-id="1d08c-161">Det vill säga hello tid Chaos pausar efter med utförs en runda av fel och att ha slutfört hello motsvarande validering hello hälsotillståndet hos hello klustret.</span><span class="sxs-lookup"><span data-stu-id="1d08c-161">That is, hello amount of time Chaos will pause after having executed a round of faults and having finished hello corresponding validation of hello health of hello cluster.</span></span> <span data-ttu-id="1d08c-162">Hej högre hello värde hello lägre är hello genomsnittlig fel injection hastighet.</span><span class="sxs-lookup"><span data-stu-id="1d08c-162">hello higher hello value, hello lower is hello average fault injection rate.</span></span>
* <span data-ttu-id="1d08c-163">**WaitTimeBetweenFaults**: hello mängden tid toowait mellan två på varandra följande fel i en enda iteration.</span><span class="sxs-lookup"><span data-stu-id="1d08c-163">**WaitTimeBetweenFaults**: hello amount of time toowait between two consecutive faults in a single iteration.</span></span> <span data-ttu-id="1d08c-164">Hej högre hello-värde, hello lägre hello samtidighet på (eller hello överlapp mellan) fel.</span><span class="sxs-lookup"><span data-stu-id="1d08c-164">hello higher hello value, hello lower hello concurrency of (or hello overlap between) faults.</span></span>
* <span data-ttu-id="1d08c-165">**ClusterHealthPolicy**: hälsoprincip för klustret är används toovalidate hello hälsotillstånd hello klustret between Chaos iterationer.</span><span class="sxs-lookup"><span data-stu-id="1d08c-165">**ClusterHealthPolicy**: Cluster health policy is used toovalidate hello health of hello cluster in between Chaos iterations.</span></span> <span data-ttu-id="1d08c-166">Om hello klustret hälsa är felaktigt eller om ett oväntat undantag inträffar under körning av fel Chaos ska vänta i 30 minuter innan hello nästa hälsokontroll - tooprovide hello kluster med vissa toorecuperate tid.</span><span class="sxs-lookup"><span data-stu-id="1d08c-166">If hello cluster health is in error or if an unexpected exception happens during fault execution, Chaos will wait for 30 minutes before hello next health-check - tooprovide hello cluster with some time toorecuperate.</span></span>
* <span data-ttu-id="1d08c-167">**Kontexten**: en samling (sträng, sträng) anger nyckel-värdepar.</span><span class="sxs-lookup"><span data-stu-id="1d08c-167">**Context**: A collection of (string, string) type key-value pairs.</span></span> <span data-ttu-id="1d08c-168">hello kartan kan vara används toorecord information om hello Chaos kör.</span><span class="sxs-lookup"><span data-stu-id="1d08c-168">hello map can be used toorecord information about hello Chaos run.</span></span> <span data-ttu-id="1d08c-169">Det får inte finnas fler än 100 par och varje sträng (nyckel eller ett värde) får innehålla högst 4095 tecken.</span><span class="sxs-lookup"><span data-stu-id="1d08c-169">There cannot be more than 100 such pairs and each string (key or value) can be at most 4095 characters long.</span></span> <span data-ttu-id="1d08c-170">Den här kartan anges av hello starter hello Chaos kör toooptionally store hello kontexten om hello specifika kör.</span><span class="sxs-lookup"><span data-stu-id="1d08c-170">This map is set by hello starter of hello Chaos run toooptionally store hello context about hello specific run.</span></span>

## <a name="how-toorun-chaos"></a><span data-ttu-id="1d08c-171">Hur toorun Chaos</span><span class="sxs-lookup"><span data-stu-id="1d08c-171">How toorun Chaos</span></span>

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
                Console.WriteLine("An instance of Chaos is already running in hello cluster.");
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
                // If a StoppedEvent is found, exit hello loop.
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
