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
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Framkalla kontrollerade Chaos i Service Fabric-kluster
Stora distribuerade system som molninfrastrukturer är natur instabilt. Azure Service Fabric kan utvecklare toowrite tillförlitliga distribuerade tjänster på ett instabilt infrastruktur. toowrite robust distribuerade tjänster på ett instabilt infrastruktur, utvecklare behöver toobe kan tootest hello stabiliteten i sina tjänster medan hello underliggande instabilt infrastruktur gå igenom komplicerade tillståndsövergångar på grund av toofaults.

Hej [fel Injection och analys klustertjänsten](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (även kallat hello fel Analysis Service) ger utvecklare möjligheten för hello tooinduce hos tootest sina tjänster. Dessa mål simulerade fel, som [startar om en partition](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), kan hjälpa dig att utöva hello vanligaste tillståndsövergångar. Men buggar riktade simulerade fel prioriterar per definition och därmed missa som visar upp endast i tillståndsövergångar svårt att förutse, långa och komplicerade ordning. Du kan använda Chaos för en oprioriterad testning.

Chaos simulerar periodiska, överlagrad fel (korrekt och städat) under hela hello kluster under en längre tid. När du har konfigurerat Chaos med hello hastighet och hello slags fel, kan du starta Chaos via C# eller Powershell API toostart genererar fel i hello kluster och i dina tjänster. Du kan konfigurera Chaos toorun för en angiven tidsperiod (till exempel under en timme), efter vilken Chaos stoppar automatiskt, eller så kan du anropa StopChaos API (C# eller Powershell) toostop den när som helst.

> [!NOTE]
> I sin nuvarande form startar Chaos endast säker fel, vilket innebär att externa fel hello saknas en förlorar kvorum eller förlust av data sker aldrig.
>

När Chaos körs, ger olika händelser som samlar in hello kör för tillfället hello hello tillstånd. En ExecutingFaultsEvent innehåller till exempel alla hello fel Chaos beslutat tooexecute i den iterationen. En ValidationFailedEvent innehåller hello information om valideringsfelet (hälsa eller stabilitet problem) som hittades vid verifiering av hello hello-klustret. Du kan anropa hello GetChaosReport API (C# eller Powershell) tooget hello rapport över Chaos körs. Dessa händelser hämta kvar i en [tillförlitliga ordlista](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), som har en trunkering princip enligt två konfigurationer: **MaxStoredChaosEventCount** (standardvärdet är 25000) och **StoredActionCleanupIntervalInSeconds** (standardvärdet är 3600). Varje *StoredActionCleanupIntervalInSeconds* Chaos kontroller och alla men hello senaste *MaxStoredChaosEventCount* händelser, rensas från hello tillförlitliga ordlistan.

## <a name="faults-induced-in-chaos"></a>Fel i Chaos
Chaos genererar fel över hello hela Service Fabric-kluster och komprimerar fel som visas i månader eller år till några timmar. hello kombination av överlagrad fel med hög feltolerans hello hittar specialfall som annars kan missas. Den här övningen kaotisk leads tooa betydande förbättringar i hello kod tjänstkvalitet hello.

Chaos startar fel från hello följande kategorier:

* Starta om en nod
* Starta om distribuerade kodpaketet
* Ta bort en replik
* Starta om en replik
* Flytta en primär replik (konfigureras)
* Flytta en sekundär replik (konfigureras)

Chaos körs i flera iterationer. Varje iteration består av fel och klusterverifieringen för hello angett period. Du kan konfigurera hello tidsåtgången för hello klustret toostabilize och validering toosucceed. Om det finns ett fel i klusterverifieringen Chaos genererar och en ValidationFailedEvent med hello UTC-tidsstämpel och hello information om felet kvarstår. Anta till exempel att en instans av Chaos som har angetts toorun för en timme med högst tre samtidiga fel. Chaos startar tre fel och validerar hello klustret hälsa. Det går igenom hello föregående steg tills den är uttryckligen stoppad via hello StopChaosAsync API eller en timme skickar. Om hello klustret blir ohälsosamt i varje iteration (dvs, den inte hålla inom hello skickades i MaxClusterStabilizationTimeout), Chaos genererar en ValidationFailedEvent. Den här händelsen tyder på att något är fel kan behöva ytterligare undersökning.

Du kan använda GetChaosReport API (powershell eller C#) tooget som hos Chaos framkallas. hello API hämtar hello nästa segment av hello Chaos rapporter utifrån hello skickades i fortsättningstoken eller hello skickades i-tidsintervall. Du kan antingen ange hello ContinuationToken tooget hello nästa segment av hello Chaos rapport eller du kan ange hello-tidsintervall via StartTimeUtc och EndTimeUtc, men du kan inte ange både hello ContinuationToken och hello tidsintervall i hello samma anrop. När det finns fler än 100 Chaos händelser, returneras hello Chaos rapporten segment där ett segment innehåller fler än 100 Chaos händelser.

## <a name="important-configuration-options"></a>Viktiga konfigurationsalternativ
* **TimeToRun**: Total tid som Chaos kör innan den är klar med framgång. Du kan stoppa Chaos innan den har körts under hello TimeToRun via hello StopChaos API.

* **MaxClusterStabilizationTimeout**: hello maximala mängden tid toowait för hello klustret toobecome felfri innan du skapar en ValidationFailedEvent. Den här vänta är tooreduce hello belastningen på hello klustret medan den återställs. hello kontrollerna utförs är:
  * Om hello klustret hälsa är OK
  * Om hello tjänstens hälsa är OK
  * Om hello mål replikuppsättningen storleken uppnås för hello service partition
  * Att det inte finns några InBuild-repliker
* **MaxConcurrentFaults**: hello maximalt antal samtidiga fel som framkallas i varje iteration. hello högre hello nummer, hello mer aggressivt Chaos är och hello redundans och hello tillstånd övergången kombinationer som hello klustret passerar är också mer komplexa. 

> [!NOTE]
> Oavsett hur högt värde *MaxConcurrentFaults* har Chaos garanterar - hello saknas externa fel - det finns ingen förlorar kvorum eller förlust av data.
>

* **EnableMoveReplicaFaults**: aktiverar eller inaktiverar hello-fel som orsakar hello primära eller sekundära repliker toomove. Dessa fel är inaktiverade som standard.
* **WaitTimeBetweenIterations**: hello mängden tid toowait mellan iterationer. Det vill säga hello tid Chaos pausar efter med utförs en runda av fel och att ha slutfört hello motsvarande validering hello hälsotillståndet hos hello klustret. Hej högre hello värde hello lägre är hello genomsnittlig fel injection hastighet.
* **WaitTimeBetweenFaults**: hello mängden tid toowait mellan två på varandra följande fel i en enda iteration. Hej högre hello-värde, hello lägre hello samtidighet på (eller hello överlapp mellan) fel.
* **ClusterHealthPolicy**: hälsoprincip för klustret är används toovalidate hello hälsotillstånd hello klustret between Chaos iterationer. Om hello klustret hälsa är felaktigt eller om ett oväntat undantag inträffar under körning av fel Chaos ska vänta i 30 minuter innan hello nästa hälsokontroll - tooprovide hello kluster med vissa toorecuperate tid.
* **Kontexten**: en samling (sträng, sträng) anger nyckel-värdepar. hello kartan kan vara används toorecord information om hello Chaos kör. Det får inte finnas fler än 100 par och varje sträng (nyckel eller ett värde) får innehålla högst 4095 tecken. Den här kartan anges av hello starter hello Chaos kör toooptionally store hello kontexten om hello specifika kör.

## <a name="how-toorun-chaos"></a>Hur toorun Chaos

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
