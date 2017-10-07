---
title: "aaaRun aktiviteter i parallella toouse beräkningsresurser effektivt - Azure Batch | Microsoft Docs"
description: "Öka effektiviteten och sänka kostnaderna genom att använda färre datornoder och kör samtidiga uppgifter på varje nod i ett Azure Batch-pool"
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 538a067c-1f6e-44eb-a92b-8d51c33d3e1a
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 05df4b7d8e0bc595168a97faa231b7c90fe81980
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-tasks-concurrently-toomaximize-usage-of-batch-compute-nodes"></a>Köra aktiviteter samtidigt toomaximize användning av Batch-beräkningsnoder 

Genom att köra mer än en uppgift samtidigt på varje beräkningsnod i Azure Batch-pool, kan du maximera resursanvändning på ett mindre antal noder i hello pool. Detta kan resultera i kortare jobbtider och den lägre kostnaden för vissa arbetsbelastningar.

Även om vissa scenarier utnyttja trafikklass alla en nod resurser tooa enskild aktivitet, utnyttja flera situationer så att flera aktiviteter tooshare resurserna:

* **Minimera dataöverföring** när uppgifter ska kunna tooshare data. I det här scenariot kan du avsevärt minska avgifter för överföring av data genom att kopiera delade data tooa mindre antal noder och köra uppgifter parallellt på varje nod. Detta gäller särskilt om hello data toobe kopierade tooeach nod måste överföras mellan geografiska områden.
* **Maximera minnesanvändning** när uppgifter kräver mycket minne, men endast under korta perioder tid och vid variabel tidpunkter under körningen. Du kan använda färre, men större, compute-noder med mer minne tooefficiently hanterar sådana toppar. Dessa noder skulle ha flera aktiviteter som körs parallellt på varje nod, men varje uppgift skulle utnyttja hello noder stort minne vid olika tidpunkter.
* **Minimera antalet gränser för noden** när kommunikationen mellan noder krävs inom en pool. Programpooler som konfigurerats för kommunikationen mellan noder för närvarande begränsad too50 compute-noder. Om varje nod i denna pool är kan tooexecute aktiviteter parallellt, kan ett större antal uppgifter köras samtidigt.
* **Replikering av en lokal beräkningskluster**, till exempel när du först flytta en beräkning miljö tooAzure. Om lokala lösningen körs flera uppgifter per compute-nod, kan du öka hello maximalt antal nod uppgifter toomore nära spegling av konfigurationen.

## <a name="example-scenario"></a>Exempelscenario
Som ett exempel tooillustrate Hej fördelarna med parallella uppgiftskörningen, anta att tillämpningsprogrammet aktivitet har krav på processor och minne så att [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md) noder är tillräckliga. Men i ordning toofinish hello-jobbet i hello krävs tid 1 000 av dessa noder krävs.

Istället för att använda Standard\_D1 noder som har 1 processorkärna och du kan använda [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) noder som har 16 kärnor och aktivera parallell aktivitetskörningen. Därför *16 gånger färre noder* kunde användas--i stället för 1 000 noder endast 63 skulle krävas. Dessutom om stora filer eller referensdata krävs för varje nod, bättre varaktighet för jobbet och effektivitet igen eftersom hello data är kopierade tooonly 16 noder.

## <a name="enable-parallel-task-execution"></a>Aktivera körning av parallell uppgift
Du kan konfigurera compute-noder för körning av parallell uppgift på hello poolen nivå. Ange hello med hello Batch .NET-biblioteket, [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] egenskapen när du skapar en pool. Om du använder hello Batch REST API, ange hello [maxTasksPerNode] [ rest_addpool] element i hello begärandetexten under skapande av poolen.

Azure Batch kan tooset maximala uppgifter per nod toofour gånger (4 x) hello antal kärnor på noden. Till exempel om hello har konfigurerats med noderna i storlek ”stor” (fyra kärnor) sedan `maxTasksPerNode` kan endast ange too16. Mer information om hello antalet kärnor för varje hello nod storlekar finns [storlekar för molntjänster](../cloud-services/cloud-services-sizes-specs.md). Mer information om tjänsten begränsar finns [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md).

> [!TIP]
> Vara säker på att tootake till kontot hello `maxTasksPerNode` värdet när du skapar en [Autoskala formeln] [ enable_autoscaling] för din pool. Till exempel en formel som utvärderas `$RunningTasks` kraftigt kan påverkas av en ökning av uppgifter per nod. Se [automatiskt skala compute-noder i en Azure Batch-pool](batch-automatic-scaling.md) för mer information.
>
>

## <a name="distribution-of-tasks"></a>Distribution av uppgifter
När hello beräkningsnoder i poolen kan köra aktiviteter samtidigt, är det viktigt toospecify du hur hello uppgifter toobe fördelad över hello noder i hello pool.

Med hjälp av hello [CloudPool.TaskSchedulingPolicy] [ task_schedule] egenskap, kan du ange att aktiviteter ska tilldelas jämnt över alla noder i hello pool (”spridning”). Eller så kan du ange att så många aktiviteter som möjligt bör tilldelas tooeach nod innan uppgifter som är tilldelade tooanother nod i hello pool (”packa”).

Ett exempel på hur den här funktionen är viktig, bör du hello-pool med [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) noder (i hello-exemplet ovan) som har konfigurerats med en [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] värde på 16. Om hello [CloudPool.TaskSchedulingPolicy] [ task_schedule] har konfigurerats med en [ComputeNodeFillType] [ fill_type] av *Pack*, den kommer att maximera användningen av alla 16 kärnor för varje nod och tillåta en [autoskalning poolen](batch-automatic-scaling.md) tooprune oanvända noder från hello pool (noder utan inga tilldelade aktiviteter). Detta minimerar Resursanvändning och sparar pengar.

## <a name="batch-net-example"></a>Batch .NET-exempel
Detta [Batch .NET] [ api_net] API kodavsnitt visar en begäran toocreate en pool som innehåller fyra stora noder med högst fyra uppgifter per nod. Det anger en schemaläggningstjänsten princip som ska fylla varje nod med uppgifter tidigare tooassigning uppgifter tooanother nod i hello pool. Mer information om att lägga till pooler med hjälp av hello Batch .NET-API finns [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicatedComputeNodes: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Batch REST-exempel
Detta [Batch REST] [ api_rest] API utdrag visar en begäran toocreate en pool som innehåller två stora noder med högst fyra uppgifter per nod. Mer information om att lägga till pooler med hjälp av hello REST-API finns [Lägg till ett poolen tooan][rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicatedComputeNodes":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [!NOTE]
> Du kan ange hello `maxTasksPerNode` element och [MaxTasksPerComputeNode] [ maxtasks_net] egenskapen endast vid tidpunkten för skapandet av poolen. De kan inte ändras efter att poolen har redan skapats.
>
>

## <a name="code-sample"></a>Kodexempel
Hej [ParallelNodeTasks] [ parallel_tasks_sample] projektet på GitHub visar hello användning av hello [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] egenskapen.

Den här C#-konsolprogram använder hello [Batch .NET] [ api_net] biblioteket toocreate en pool med en eller flera compute-noder. En konfigurerbar antalet aktiviteter som körs på dessa noder toosimulate variabeln belastningen. Utdata från programmet hello anger vilka noder utförs varje aktivitet. hello program innehåller också en sammanfattning av hello jobbparametrar och varaktighet. hello sammanfattning del av hello utdata från två olika körningar av hello exempelprogrammet visas nedan.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

hello första körningen av hello exempelprogrammet visar att med en enskild nod i hello pool och hello standardinställningen för en aktivitet per nod, hello jobbet varaktighet är under 30 minuter.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

hello kör andra av hello exemplet visar en betydande minskning i jobbet varaktighet. Det beror på att hello poolen har konfigurerats med fyra uppgifter per nod som möjliggör parallell körning toocomplete hello aktiviteten i nästan ett kvartal hello tid.

> [!NOTE]
> hello jobbet varaktigheter i hello sammanfattningar ovan omfattar inte tiden för skapandet av poolen. Var och en av hello jobb ovan har skickats toopreviously skapade pooler vars datornoderna fanns i hello *inaktivt* tillstånd skicka för närvarande.
>
>

## <a name="next-steps"></a>Nästa steg
### <a name="batch-explorer-heat-map"></a>Batch Explorer termisk karta
Hej [Azure Batch Explorer][batch_explorer], en hello Azure Batch [programexempel][github_samples], innehåller en *termisk karta* funktion som tillhandahåller visualisering av körning av aktiviteten. När du köra hello [ParallelTasks] [ parallel_tasks_sample] exempelprogrammet som du kan använda hello termisk karta funktionen tooeasily visualisera hello körning av parallella aktiviteter på varje nod.

![Batch Explorer termisk karta][1]

*Batch Explorer termisk karta med en pool med fyra noder med varje nod fyra aktiviteter som körs*

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

[1]: ./media/batch-parallel-node-tasks\heat_map.png
