---
title: "aaaSimulate fel i Azure mikrotjänster | Microsoft Docs"
description: "Den här artikeln handlar om hello datatillgång åtgärder finns i Microsoft Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: motanv
manager: timlt
editor: toddabel
ms.assetid: ed53ca5c-4d5e-4b48-93c9-e386f32d8b7a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv;heeldin
ms.openlocfilehash: 5bdda1c0c5a40b243ab956c4791afd52e11c4089
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="testability-actions"></a>Möjlighet att testa åtgärder
I ordning toosimulate en instabilt infrastruktur ger Azure Service Fabric du hello utvecklare med sätt toosimulate olika verkliga fel och tillståndsövergångar. Dessa visas som datatillgång åtgärder. hello åtgärder är hello lågnivå-API: er som orsakar en viss feltolerans injection, tillståndsövergång eller validering. Du kan skriva omfattande testscenarier för dina tjänster genom att kombinera dessa åtgärder.

Service Fabric innehåller några vanliga testscenarier består av dessa åtgärder. Vi rekommenderar starkt att du använder dessa inbyggda scenarier som väljs noggrant tootest vanliga tillståndsövergångar och fel fall. Åtgärder kan dock använda toocreate anpassade testscenarier när du vill tooadd täckning för scenarier som inte omfattas av inbyggda hello-scenarier ännu eller som är anpassade skräddarsydda för ditt program.

C#-implementeringar av hello åtgärder finns i hello System.Fabric.dll sammansättning. hello System Fabric PowerShell-modulen finns i hello Microsoft.ServiceFabric.Powershell.dll sammansättning. Som en del av runtime-installation är hello ServiceFabric PowerShell-modulen installerad tooallow lätt att använda.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Korrekt kontra städat fel åtgärder
Möjlighet att testa åtgärder indelas i två huvudsakliga buckets.

* Städat fel: dessa fel simulera fel som omstarter av datorn och krascher. I sådana fall fel hello körningskontexten processens plötsligt. Detta innebär att ingen rensning av hello tillstånd kan köra innan programmet hello startas igen.
* Korrekt fel: dessa fel simulera korrekt åtgärder som flyttar replik och således som utlösts av belastningsutjämning. I sådana fall hello tjänsten hämtar ett meddelande om hello Stäng och kan rensa hello tillstånd innan du avslutar.

För bättre kvalitet verifiering köra hello-tjänsten och företag arbetsbelastning när att olika korrekt och städat fel. Städat fel utöva scenarier där hello tjänstprocessen plötsligt avslutas hello mitten av vissa arbetsflöde. Detta testar hello Återställningssökväg när hello service replikeringen har återställts av Service Fabric. Detta hjälper testa datakonsekvens och huruvida hello Tjänststatus behålls korrekt efter fel. hello andra uppsättning fel (hello korrekt misslyckanden) testa att hello service korrekt reagerar tooreplicas flyttas runt av Service Fabric. Detta testar hantering av annullering i hello RunAsync metoden. hello-tjänsten måste toocheck för hello annullering token som angetts korrekt spara sitt tillstånd och avsluta hello RunAsync metoden.

## <a name="testability-actions-list"></a>Möjlighet att testa åtgärdslista
| Åtgärd | Beskrivning | Hanterade API | PowerShell-cmdlet | Korrekt/städat fel |
| --- | --- | --- | --- | --- |
| CleanTestState |Tar bort alla hello testtillstånd från hello kluster vid en felaktig avstängning av hello test-drivrutinen. |CleanTestStateAsync |Remove-ServiceFabricTestState |Inte tillämpligt |
| InvokeDataLoss |Startar förlust av data i en partition med tjänsten. |InvokeDataLossAsync |Invoke-ServiceFabricPartitionDataLoss |Korrekt |
| InvokeQuorumLoss |Placerar en given tillståndskänslig service partition i förlorar kvorum. |InvokeQuorumLossAsync |Anropa ServiceFabricQuorumLoss |Korrekt |
| Flytta primära |Flyttar hello angetts primära repliken av en tillståndskänslig service toohello angivna klusternoden. |MovePrimaryAsync |Flytta ServiceFabricPrimaryReplica |Korrekt |
| Flytta sekundär |Flyttar hello aktuella sekundär replik av en tillståndskänslig service tooa annan klusternod. |MoveSecondaryAsync |Flytta ServiceFabricSecondaryReplica |Korrekt |
| RemoveReplica |Simulerar ett replik fel genom att ta bort en replik från ett kluster. Detta kommer att stängas hello replik och övergår det toorole None, ta bort dess status från hello kluster. |RemoveReplicaAsync |Ta bort ServiceFabricReplica |Korrekt |
| RestartDeployedCodePackage |Simulerar ett fel i koden paketet genom att starta om en kodpaketet har distribuerats på en nod i ett kluster. Detta avbryter hello kod paketet processen, vilket startar om alla hello användaren service repliker i den här processen. |RestartDeployedCodePackageAsync |Starta om ServiceFabricDeployedCodePackage |Städat |
| RestartNode |Simulerar ett nodfel för Service Fabric-kluster genom att starta om en nod. |RestartNodeAsync |Starta om ServiceFabricNode |Städat |
| RestartPartition |Simulerar ett datacenter blackout eller kluster blackout scenario genom att starta om vissa eller alla repliker för en partition. |RestartPartitionAsync |Restart-ServiceFabricPartition |Korrekt |
| RestartReplica |Simulerar ett fel för repliken genom att starta om en beständiga replik i ett kluster, stänga hello replik och sedan öppna den igen. |RestartReplicaAsync |Starta om ServiceFabricReplica |Korrekt |
| Startnod |Startar en nod i ett kluster som har redan stoppats. |StartNodeAsync |Start-ServiceFabricNode |Inte tillämpligt |
| StopNode |Simulerar ett nodfel genom att stoppa en nod i ett kluster. hello nod förblir ned förrän Startnod anropas. |StopNodeAsync |Stop-ServiceFabricNode |Städat |
| ValidateApplication |Verifierar hello tillgänglighet och hälsotillståndet för alla Service Fabric-tjänster i ett program, vanligtvis efter att vissa fel till hello system. |ValidateApplicationAsync |Testa ServiceFabricApplication |Inte tillämpligt |
| ValidateService |Verifierar hello tillgänglighet och hälsotillståndet för ett Service Fabric-tjänsten, vanligtvis efter att vissa fel till hello system. |ValidateServiceAsync |Testa ServiceFabricService |Inte tillämpligt |

## <a name="running-a-testability-action-using-powershell"></a>Köra en datatillgång-åtgärd med hjälp av PowerShell
De här självstudierna visar hur toorun en möjlighet att testa åtgärden med hjälp av PowerShell. Du får lära dig hur toorun en datatillgång-åtgärd mot en lokal (en-box)-kluster eller ett Azure-kluster. Microsoft.Fabric.Powershell.dll--hello Service Fabric PowerShell-modulen--installeras automatiskt när du installerar hello Microsoft Service Fabric MSI. hello modulen laddas automatiskt när du öppnar en PowerShell-kommandotolk.

Självstudiekurs segment:

* [Köra en åtgärd mot ett kluster med en ruta](#run-an-action-against-a-one-box-cluster)
* [Köra en åtgärd mot ett Azure-kluster](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Köra en åtgärd mot ett kluster med en ruta
toorun en datatillgång-åtgärd mot en lokala klustret först ansluta toohello klustret och öppna hello PowerShell-Kommandotolken i administratörsläge. Låt oss titta på hello **omstart ServiceFabricNode** åtgärd.

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Här hello åtgärd **omstart ServiceFabricNode** körs på en nod med namnet ”Nod1”. hello slutförande läge anger att den inte ska kontrollera om hello starta om noden åtgärden faktiskt har slutförts. Att ange hello slutförande-läge som ”verifiera” innebär att den tooverify om hello omstart åtgärden faktiskt har slutförts. Istället för att ange hello noden direkt med namnet kan du ange den via en partition nyckel och hello typ av repliken, enligt följande:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Starta om ServiceFabricNode** ska använda toorestart ett Service Fabric-nod i ett kluster. Detta förhindrar hello Fabric.exe-processen, vilket startar om alla hello system-tjänsten och användaren service repliker på noden. Med den här API-tootest kan din tjänst upptäcka buggar längs hello failover återställning sökvägar. Det hjälper att simulera nodfel i hello kluster.

hello följande skärmbild visar hello **omstart ServiceFabricNode** datatillgång kommandot i åtgärden.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

hello utdata av hello först **Get-ServiceFabricNode** (en cmdlet från hello Service Fabric PowerShell-modulen) visar den lokala hello-klustret har fem noder: Node.1 tooNode.5. Efter hello datatillgång åtgärd (cmdlet) **omstart ServiceFabricNode** körs på hello nod med namnet Node.4 vi se hello nodens drifttid har återställts.

### <a name="run-an-action-against-an-azure-cluster"></a>Köra en åtgärd mot ett Azure-kluster
Kör en datatillgång-åtgärd (med hjälp av PowerShell) mot ett Azure-kluster är liknande toorunning hello-åtgärd mot en lokala klustret. hello endast skillnaden är att innan du kan köra hello-åtgärden, i stället för anslutande toohello lokala klustret och du behöver tooconnect toohello Azure kluster först.

## <a name="running-a-testability-action-using-c35"></a>Köra en datatillgång åtgärd med C & #35.
toorun en möjlighet att testa åtgärden med hjälp av C#, måste du först tooconnect toohello kluster med hjälp av FabricClient. Skaffa hello parametrar som behövs toorun hello åtgärd. Olika parametrar som kan användas för toorun hello samma åtgärd.
Titta på hello RestartServiceFabricNode åtgärd, enkelriktade toorun är det med hjälp av noden hello information (nodnamnet och nod-instans-ID) i hello kluster.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Parametern förklaring:

* **CompleteMode** anger att hello-läge inte ska kontrollera om hello omstart åtgärden faktiskt har slutförts. Att ange hello slutförande-läge som ”verifiera” innebär att den tooverify om hello omstart åtgärden faktiskt har slutförts.  
* **OperationTimeout** anger hello tiden för hello åtgärden toofinish innan en TimeoutException undantag.
* **CancellationToken** gör en väntande samtal toobe avbröts.

Du kan ange den via en partition nyckel och hello typ av replik i stället för att ange hello noden direkt med sitt namn.

Mer information finns i [PartitionSelector och ReplicaSelector](#partition_replica_selector).

```csharp
// Add a reference tooSystem.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart hello node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way toorestart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>PartitionSelector och ReplicaSelector
### <a name="partitionselector"></a>PartitionSelector
PartitionSelector är en hjälp som visas i datatillgång är används tooselect en specifik partition på vilka tooperform hello datatillgång åtgärder. Det kan vara används tooselect en specifik partition om hello partitions-ID är känt i förväg. Eller, du kan ange hello partitionsnyckel och hello åtgärden kommer att lösa hello partitions-ID internt. Du kan också ha hello kan välja att en slumpmässig partition.

toouse helper, skapa hello PartitionSelector objekt och välj hello partition med hjälp av någon av hello Select * metoder. Ange sedan hello PartitionSelector objektet toohello API som kräver. Om inget alternativ har valts standard tooa slumpmässiga partition.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>ReplicaSelector
ReplicaSelector är en hjälp som visas i datatillgång är används toohelp väljer du en replik på vilka tooperform någon av hello datatillgång åtgärder. Det kan vara används tooselect en specifik replik om hello-replik-ID är känt i förväg. Dessutom har hello möjlighet att välja en primär replik eller en slumpmässig sekundär. ReplicaSelector härleds från PartitionSelector, så du måste tooselect både hello replik och hello partition där du vill tooperform hello datatillgång igen.

toouse helper, skapa ett ReplicaSelector objekt och ange hello önskemål tooselect hello replik och hello partition. Du kan sedan överföra den till hello API som kräver. Om inget alternativ har valts standard tooa slumpmässiga replik och slumpmässiga partition.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select hello primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select hello replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Nästa steg
* [Möjlighet att testa scenarier](service-fabric-testability-scenarios.md)
* Hur tootest din tjänst
  * [Simulera fel under tjänstens arbetsbelastningar](service-fabric-testability-workload-tests.md)
  * [Tjänst-till-tjänst kommunikationsfel](service-fabric-testability-scenarios-service-communication.md)

