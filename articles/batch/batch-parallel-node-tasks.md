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
# <a name="run-tasks-concurrently-toomaximize-usage-of-batch-compute-nodes"></a><span data-ttu-id="49158-103">Köra aktiviteter samtidigt toomaximize användning av Batch-beräkningsnoder</span><span class="sxs-lookup"><span data-stu-id="49158-103">Run tasks concurrently toomaximize usage of Batch compute nodes</span></span> 

<span data-ttu-id="49158-104">Genom att köra mer än en uppgift samtidigt på varje beräkningsnod i Azure Batch-pool, kan du maximera resursanvändning på ett mindre antal noder i hello pool.</span><span class="sxs-lookup"><span data-stu-id="49158-104">By running more than one task simultaneously on each compute node in your Azure Batch pool, you can maximize resource usage on a smaller number of nodes in hello pool.</span></span> <span data-ttu-id="49158-105">Detta kan resultera i kortare jobbtider och den lägre kostnaden för vissa arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="49158-105">For some workloads, this can result in shorter job times and lower cost.</span></span>

<span data-ttu-id="49158-106">Även om vissa scenarier utnyttja trafikklass alla en nod resurser tooa enskild aktivitet, utnyttja flera situationer så att flera aktiviteter tooshare resurserna:</span><span class="sxs-lookup"><span data-stu-id="49158-106">While some scenarios benefit from dedicating all of a node's resources tooa single task, several situations benefit from allowing multiple tasks tooshare those resources:</span></span>

* <span data-ttu-id="49158-107">**Minimera dataöverföring** när uppgifter ska kunna tooshare data.</span><span class="sxs-lookup"><span data-stu-id="49158-107">**Minimizing data transfer** when tasks are able tooshare data.</span></span> <span data-ttu-id="49158-108">I det här scenariot kan du avsevärt minska avgifter för överföring av data genom att kopiera delade data tooa mindre antal noder och köra uppgifter parallellt på varje nod.</span><span class="sxs-lookup"><span data-stu-id="49158-108">In this scenario, you can dramatically reduce data transfer charges by copying shared data tooa smaller number of nodes and executing tasks in parallel on each node.</span></span> <span data-ttu-id="49158-109">Detta gäller särskilt om hello data toobe kopierade tooeach nod måste överföras mellan geografiska områden.</span><span class="sxs-lookup"><span data-stu-id="49158-109">This especially applies if hello data toobe copied tooeach node must be transferred between geographic regions.</span></span>
* <span data-ttu-id="49158-110">**Maximera minnesanvändning** när uppgifter kräver mycket minne, men endast under korta perioder tid och vid variabel tidpunkter under körningen.</span><span class="sxs-lookup"><span data-stu-id="49158-110">**Maximizing memory usage** when tasks require a large amount of memory, but only during short periods of time, and at variable times during execution.</span></span> <span data-ttu-id="49158-111">Du kan använda färre, men större, compute-noder med mer minne tooefficiently hanterar sådana toppar.</span><span class="sxs-lookup"><span data-stu-id="49158-111">You can employ fewer, but larger, compute nodes with more memory tooefficiently handle such spikes.</span></span> <span data-ttu-id="49158-112">Dessa noder skulle ha flera aktiviteter som körs parallellt på varje nod, men varje uppgift skulle utnyttja hello noder stort minne vid olika tidpunkter.</span><span class="sxs-lookup"><span data-stu-id="49158-112">These nodes would have multiple tasks running in parallel on each node, but each task would take advantage of hello nodes' plentiful memory at different times.</span></span>
* <span data-ttu-id="49158-113">**Minimera antalet gränser för noden** när kommunikationen mellan noder krävs inom en pool.</span><span class="sxs-lookup"><span data-stu-id="49158-113">**Mitigating node number limits** when inter-node communication is required within a pool.</span></span> <span data-ttu-id="49158-114">Programpooler som konfigurerats för kommunikationen mellan noder för närvarande begränsad too50 compute-noder.</span><span class="sxs-lookup"><span data-stu-id="49158-114">Currently, pools configured for inter-node communication are limited too50 compute nodes.</span></span> <span data-ttu-id="49158-115">Om varje nod i denna pool är kan tooexecute aktiviteter parallellt, kan ett större antal uppgifter köras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="49158-115">If each node in such a pool is able tooexecute tasks in parallel, a greater number of tasks can be executed simultaneously.</span></span>
* <span data-ttu-id="49158-116">**Replikering av en lokal beräkningskluster**, till exempel när du först flytta en beräkning miljö tooAzure.</span><span class="sxs-lookup"><span data-stu-id="49158-116">**Replicating an on-premises compute cluster**, such as when you first move a compute environment tooAzure.</span></span> <span data-ttu-id="49158-117">Om lokala lösningen körs flera uppgifter per compute-nod, kan du öka hello maximalt antal nod uppgifter toomore nära spegling av konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="49158-117">If your current on-premises solution executes multiple tasks per compute node, you can increase hello maximum number of node tasks toomore closely mirror that configuration.</span></span>

## <a name="example-scenario"></a><span data-ttu-id="49158-118">Exempelscenario</span><span class="sxs-lookup"><span data-stu-id="49158-118">Example scenario</span></span>
<span data-ttu-id="49158-119">Som ett exempel tooillustrate Hej fördelarna med parallella uppgiftskörningen, anta att tillämpningsprogrammet aktivitet har krav på processor och minne så att [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md) noder är tillräckliga.</span><span class="sxs-lookup"><span data-stu-id="49158-119">As an example tooillustrate hello benefits of parallel task execution, let's say that your task application has CPU and memory requirements such that [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md) nodes are sufficient.</span></span> <span data-ttu-id="49158-120">Men i ordning toofinish hello-jobbet i hello krävs tid 1 000 av dessa noder krävs.</span><span class="sxs-lookup"><span data-stu-id="49158-120">But, in order toofinish hello job in hello required time, 1,000 of these nodes are needed.</span></span>

<span data-ttu-id="49158-121">Istället för att använda Standard\_D1 noder som har 1 processorkärna och du kan använda [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) noder som har 16 kärnor och aktivera parallell aktivitetskörningen.</span><span class="sxs-lookup"><span data-stu-id="49158-121">Instead of using Standard\_D1 nodes that have 1 CPU core, you could use [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) nodes that have 16 cores each, and enable parallel task execution.</span></span> <span data-ttu-id="49158-122">Därför *16 gånger färre noder* kunde användas--i stället för 1 000 noder endast 63 skulle krävas.</span><span class="sxs-lookup"><span data-stu-id="49158-122">Therefore, *16 times fewer nodes* could be used--instead of 1,000 nodes, only 63 would be required.</span></span> <span data-ttu-id="49158-123">Dessutom om stora filer eller referensdata krävs för varje nod, bättre varaktighet för jobbet och effektivitet igen eftersom hello data är kopierade tooonly 16 noder.</span><span class="sxs-lookup"><span data-stu-id="49158-123">Additionally, if large application files or reference data are required for each node, job duration and efficiency are again improved since hello data is copied tooonly 16 nodes.</span></span>

## <a name="enable-parallel-task-execution"></a><span data-ttu-id="49158-124">Aktivera körning av parallell uppgift</span><span class="sxs-lookup"><span data-stu-id="49158-124">Enable parallel task execution</span></span>
<span data-ttu-id="49158-125">Du kan konfigurera compute-noder för körning av parallell uppgift på hello poolen nivå.</span><span class="sxs-lookup"><span data-stu-id="49158-125">You configure compute nodes for parallel task execution at hello pool level.</span></span> <span data-ttu-id="49158-126">Ange hello med hello Batch .NET-biblioteket, [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] egenskapen när du skapar en pool.</span><span class="sxs-lookup"><span data-stu-id="49158-126">With hello Batch .NET library, set hello [CloudPool.MaxTasksPerComputeNode][maxtasks_net] property when you create a pool.</span></span> <span data-ttu-id="49158-127">Om du använder hello Batch REST API, ange hello [maxTasksPerNode] [ rest_addpool] element i hello begärandetexten under skapande av poolen.</span><span class="sxs-lookup"><span data-stu-id="49158-127">If you are using hello Batch REST API, set hello [maxTasksPerNode][rest_addpool] element in hello request body during pool creation.</span></span>

<span data-ttu-id="49158-128">Azure Batch kan tooset maximala uppgifter per nod toofour gånger (4 x) hello antal kärnor på noden.</span><span class="sxs-lookup"><span data-stu-id="49158-128">Azure Batch allows you tooset maximum tasks per node up toofour times (4x) hello number of node cores.</span></span> <span data-ttu-id="49158-129">Till exempel om hello har konfigurerats med noderna i storlek ”stor” (fyra kärnor) sedan `maxTasksPerNode` kan endast ange too16.</span><span class="sxs-lookup"><span data-stu-id="49158-129">For example, if hello pool is configured with nodes of size "Large" (four cores), then `maxTasksPerNode` may be set too16.</span></span> <span data-ttu-id="49158-130">Mer information om hello antalet kärnor för varje hello nod storlekar finns [storlekar för molntjänster](../cloud-services/cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="49158-130">For details on hello number of cores for each of hello node sizes, see [Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md).</span></span> <span data-ttu-id="49158-131">Mer information om tjänsten begränsar finns [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="49158-131">For more information on service limits, see [Quotas and limits for hello Azure Batch service](batch-quota-limit.md).</span></span>

> [!TIP]
> <span data-ttu-id="49158-132">Vara säker på att tootake till kontot hello `maxTasksPerNode` värdet när du skapar en [Autoskala formeln] [ enable_autoscaling] för din pool.</span><span class="sxs-lookup"><span data-stu-id="49158-132">Be sure tootake into account hello `maxTasksPerNode` value when you construct an [autoscale formula][enable_autoscaling] for your pool.</span></span> <span data-ttu-id="49158-133">Till exempel en formel som utvärderas `$RunningTasks` kraftigt kan påverkas av en ökning av uppgifter per nod.</span><span class="sxs-lookup"><span data-stu-id="49158-133">For example, a formula that evaluates `$RunningTasks` could be dramatically affected by an increase in tasks per node.</span></span> <span data-ttu-id="49158-134">Se [automatiskt skala compute-noder i en Azure Batch-pool](batch-automatic-scaling.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="49158-134">See [Automatically scale compute nodes in an Azure Batch pool](batch-automatic-scaling.md) for more information.</span></span>
>
>

## <a name="distribution-of-tasks"></a><span data-ttu-id="49158-135">Distribution av uppgifter</span><span class="sxs-lookup"><span data-stu-id="49158-135">Distribution of tasks</span></span>
<span data-ttu-id="49158-136">När hello beräkningsnoder i poolen kan köra aktiviteter samtidigt, är det viktigt toospecify du hur hello uppgifter toobe fördelad över hello noder i hello pool.</span><span class="sxs-lookup"><span data-stu-id="49158-136">When hello compute nodes in a pool can execute tasks concurrently, it's important toospecify how you want hello tasks toobe distributed across hello nodes in hello pool.</span></span>

<span data-ttu-id="49158-137">Med hjälp av hello [CloudPool.TaskSchedulingPolicy] [ task_schedule] egenskap, kan du ange att aktiviteter ska tilldelas jämnt över alla noder i hello pool (”spridning”).</span><span class="sxs-lookup"><span data-stu-id="49158-137">By using hello [CloudPool.TaskSchedulingPolicy][task_schedule] property, you can specify that tasks should be assigned evenly across all nodes in hello pool ("spreading").</span></span> <span data-ttu-id="49158-138">Eller så kan du ange att så många aktiviteter som möjligt bör tilldelas tooeach nod innan uppgifter som är tilldelade tooanother nod i hello pool (”packa”).</span><span class="sxs-lookup"><span data-stu-id="49158-138">Or you can specify that as many tasks as possible should be assigned tooeach node before tasks are assigned tooanother node in hello pool ("packing").</span></span>

<span data-ttu-id="49158-139">Ett exempel på hur den här funktionen är viktig, bör du hello-pool med [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) noder (i hello-exemplet ovan) som har konfigurerats med en [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] värde på 16.</span><span class="sxs-lookup"><span data-stu-id="49158-139">As an example of how this feature is valuable, consider hello pool of [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) nodes (in hello example above) that is configured with a [CloudPool.MaxTasksPerComputeNode][maxtasks_net] value of 16.</span></span> <span data-ttu-id="49158-140">Om hello [CloudPool.TaskSchedulingPolicy] [ task_schedule] har konfigurerats med en [ComputeNodeFillType] [ fill_type] av *Pack*, den kommer att maximera användningen av alla 16 kärnor för varje nod och tillåta en [autoskalning poolen](batch-automatic-scaling.md) tooprune oanvända noder från hello pool (noder utan inga tilldelade aktiviteter).</span><span class="sxs-lookup"><span data-stu-id="49158-140">If hello [CloudPool.TaskSchedulingPolicy][task_schedule] is configured with a [ComputeNodeFillType][fill_type] of *Pack*, it would maximize usage of all 16 cores of each node and allow an [autoscaling pool](batch-automatic-scaling.md) tooprune unused nodes from hello pool (nodes without any tasks assigned).</span></span> <span data-ttu-id="49158-141">Detta minimerar Resursanvändning och sparar pengar.</span><span class="sxs-lookup"><span data-stu-id="49158-141">This minimizes resource usage and saves money.</span></span>

## <a name="batch-net-example"></a><span data-ttu-id="49158-142">Batch .NET-exempel</span><span class="sxs-lookup"><span data-stu-id="49158-142">Batch .NET example</span></span>
<span data-ttu-id="49158-143">Detta [Batch .NET] [ api_net] API kodavsnitt visar en begäran toocreate en pool som innehåller fyra stora noder med högst fyra uppgifter per nod.</span><span class="sxs-lookup"><span data-stu-id="49158-143">This [Batch .NET][api_net] API code snippet shows a request toocreate a pool that contains four large nodes with a maximum of four tasks per node.</span></span> <span data-ttu-id="49158-144">Det anger en schemaläggningstjänsten princip som ska fylla varje nod med uppgifter tidigare tooassigning uppgifter tooanother nod i hello pool.</span><span class="sxs-lookup"><span data-stu-id="49158-144">It specifies a task scheduling policy that will fill each node with tasks prior tooassigning tasks tooanother node in hello pool.</span></span> <span data-ttu-id="49158-145">Mer information om att lägga till pooler med hjälp av hello Batch .NET-API finns [BatchClient.PoolOperations.CreatePool][poolcreate_net].</span><span class="sxs-lookup"><span data-stu-id="49158-145">For more information on adding pools by using hello Batch .NET API, see [BatchClient.PoolOperations.CreatePool][poolcreate_net].</span></span>

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

## <a name="batch-rest-example"></a><span data-ttu-id="49158-146">Batch REST-exempel</span><span class="sxs-lookup"><span data-stu-id="49158-146">Batch REST example</span></span>
<span data-ttu-id="49158-147">Detta [Batch REST] [ api_rest] API utdrag visar en begäran toocreate en pool som innehåller två stora noder med högst fyra uppgifter per nod.</span><span class="sxs-lookup"><span data-stu-id="49158-147">This [Batch REST][api_rest] API snippet shows a request toocreate a pool that contains two large nodes with a maximum of four tasks per node.</span></span> <span data-ttu-id="49158-148">Mer information om att lägga till pooler med hjälp av hello REST-API finns [Lägg till ett poolen tooan][rest_addpool].</span><span class="sxs-lookup"><span data-stu-id="49158-148">For more information on adding pools by using hello REST API, see [Add a pool tooan account][rest_addpool].</span></span>

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
> <span data-ttu-id="49158-149">Du kan ange hello `maxTasksPerNode` element och [MaxTasksPerComputeNode] [ maxtasks_net] egenskapen endast vid tidpunkten för skapandet av poolen.</span><span class="sxs-lookup"><span data-stu-id="49158-149">You can set hello `maxTasksPerNode` element and [MaxTasksPerComputeNode][maxtasks_net] property only at pool creation time.</span></span> <span data-ttu-id="49158-150">De kan inte ändras efter att poolen har redan skapats.</span><span class="sxs-lookup"><span data-stu-id="49158-150">They cannot be modified after a pool has already been created.</span></span>
>
>

## <a name="code-sample"></a><span data-ttu-id="49158-151">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="49158-151">Code sample</span></span>
<span data-ttu-id="49158-152">Hej [ParallelNodeTasks] [ parallel_tasks_sample] projektet på GitHub visar hello användning av hello [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] egenskapen.</span><span class="sxs-lookup"><span data-stu-id="49158-152">hello [ParallelNodeTasks][parallel_tasks_sample] project on GitHub illustrates hello use of hello [CloudPool.MaxTasksPerComputeNode][maxtasks_net] property.</span></span>

<span data-ttu-id="49158-153">Den här C#-konsolprogram använder hello [Batch .NET] [ api_net] biblioteket toocreate en pool med en eller flera compute-noder.</span><span class="sxs-lookup"><span data-stu-id="49158-153">This C# console application uses hello [Batch .NET][api_net] library toocreate a pool with one or more compute nodes.</span></span> <span data-ttu-id="49158-154">En konfigurerbar antalet aktiviteter som körs på dessa noder toosimulate variabeln belastningen.</span><span class="sxs-lookup"><span data-stu-id="49158-154">It executes a configurable number of tasks on those nodes toosimulate variable load.</span></span> <span data-ttu-id="49158-155">Utdata från programmet hello anger vilka noder utförs varje aktivitet.</span><span class="sxs-lookup"><span data-stu-id="49158-155">Output from hello application specifies which nodes executed each task.</span></span> <span data-ttu-id="49158-156">hello program innehåller också en sammanfattning av hello jobbparametrar och varaktighet.</span><span class="sxs-lookup"><span data-stu-id="49158-156">hello application also provides a summary of hello job parameters and duration.</span></span> <span data-ttu-id="49158-157">hello sammanfattning del av hello utdata från två olika körningar av hello exempelprogrammet visas nedan.</span><span class="sxs-lookup"><span data-stu-id="49158-157">hello summary portion of hello output from two different runs of hello sample application appears below.</span></span>

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

<span data-ttu-id="49158-158">hello första körningen av hello exempelprogrammet visar att med en enskild nod i hello pool och hello standardinställningen för en aktivitet per nod, hello jobbet varaktighet är under 30 minuter.</span><span class="sxs-lookup"><span data-stu-id="49158-158">hello first execution of hello sample application shows that with a single node in hello pool and hello default setting of one task per node, hello job duration is over 30 minutes.</span></span>

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

<span data-ttu-id="49158-159">hello kör andra av hello exemplet visar en betydande minskning i jobbet varaktighet.</span><span class="sxs-lookup"><span data-stu-id="49158-159">hello second run of hello sample shows a significant decrease in job duration.</span></span> <span data-ttu-id="49158-160">Det beror på att hello poolen har konfigurerats med fyra uppgifter per nod som möjliggör parallell körning toocomplete hello aktiviteten i nästan ett kvartal hello tid.</span><span class="sxs-lookup"><span data-stu-id="49158-160">This is because hello pool was configured with four tasks per node, which allows for parallel task execution toocomplete hello job in nearly a quarter of hello time.</span></span>

> [!NOTE]
> <span data-ttu-id="49158-161">hello jobbet varaktigheter i hello sammanfattningar ovan omfattar inte tiden för skapandet av poolen.</span><span class="sxs-lookup"><span data-stu-id="49158-161">hello job durations in hello summaries above do not include pool creation time.</span></span> <span data-ttu-id="49158-162">Var och en av hello jobb ovan har skickats toopreviously skapade pooler vars datornoderna fanns i hello *inaktivt* tillstånd skicka för närvarande.</span><span class="sxs-lookup"><span data-stu-id="49158-162">Each of hello jobs above was submitted toopreviously created pools whose compute nodes were in hello *Idle* state at submission time.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="49158-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49158-163">Next steps</span></span>
### <a name="batch-explorer-heat-map"></a><span data-ttu-id="49158-164">Batch Explorer termisk karta</span><span class="sxs-lookup"><span data-stu-id="49158-164">Batch Explorer Heat Map</span></span>
<span data-ttu-id="49158-165">Hej [Azure Batch Explorer][batch_explorer], en hello Azure Batch [programexempel][github_samples], innehåller en *termisk karta* funktion som tillhandahåller visualisering av körning av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="49158-165">hello [Azure Batch Explorer][batch_explorer], one of hello Azure Batch [sample applications][github_samples], contains a *Heat Map* feature that provides visualization of task execution.</span></span> <span data-ttu-id="49158-166">När du köra hello [ParallelTasks] [ parallel_tasks_sample] exempelprogrammet som du kan använda hello termisk karta funktionen tooeasily visualisera hello körning av parallella aktiviteter på varje nod.</span><span class="sxs-lookup"><span data-stu-id="49158-166">When you're executing hello [ParallelTasks][parallel_tasks_sample] sample application, you can use hello Heat Map feature tooeasily visualize hello execution of parallel tasks on each node.</span></span>

![Batch Explorer termisk karta][1]

<span data-ttu-id="49158-168">*Batch Explorer termisk karta med en pool med fyra noder med varje nod fyra aktiviteter som körs*</span><span class="sxs-lookup"><span data-stu-id="49158-168">*Batch Explorer Heat Map showing a pool of four nodes, with each node currently executing four tasks*</span></span>

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
