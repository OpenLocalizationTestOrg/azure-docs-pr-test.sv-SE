---
title: "Köra uppgifter parallellt att använda beräkningsresurser effektivt - Azure Batch | Microsoft Docs"
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
ms.openlocfilehash: 6903552d907a1ddb21d3b678e2d224b4b5e35b77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="run-tasks-concurrently-to-maximize-usage-of-batch-compute-nodes"></a><span data-ttu-id="31c58-103">Köra aktiviteter samtidigt för att maximera användningen av Batch-beräkningsnoder</span><span class="sxs-lookup"><span data-stu-id="31c58-103">Run tasks concurrently to maximize usage of Batch compute nodes</span></span> 

<span data-ttu-id="31c58-104">Genom att köra mer än en uppgift samtidigt på varje beräkningsnod i Azure Batch-pool, kan du maximera resursanvändning på ett mindre antal noder i poolen.</span><span class="sxs-lookup"><span data-stu-id="31c58-104">By running more than one task simultaneously on each compute node in your Azure Batch pool, you can maximize resource usage on a smaller number of nodes in the pool.</span></span> <span data-ttu-id="31c58-105">Detta kan resultera i kortare jobbtider och den lägre kostnaden för vissa arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="31c58-105">For some workloads, this can result in shorter job times and lower cost.</span></span>

<span data-ttu-id="31c58-106">När vissa scenarier utnyttja för en enskild uppgift trafikklass alla resurser för en nod, utnyttja flera situationer så att flera aktiviteter som ska dela dessa resurser:</span><span class="sxs-lookup"><span data-stu-id="31c58-106">While some scenarios benefit from dedicating all of a node's resources to a single task, several situations benefit from allowing multiple tasks to share those resources:</span></span>

* <span data-ttu-id="31c58-107">**Minimera dataöverföring** när en uppgift ska kunna dela data.</span><span class="sxs-lookup"><span data-stu-id="31c58-107">**Minimizing data transfer** when tasks are able to share data.</span></span> <span data-ttu-id="31c58-108">I det här scenariot kan du kraftigt minska avgifter för överföring av data genom att kopiera delade data till ett mindre antal noder och aktiviteter körs parallellt på varje nod.</span><span class="sxs-lookup"><span data-stu-id="31c58-108">In this scenario, you can dramatically reduce data transfer charges by copying shared data to a smaller number of nodes and executing tasks in parallel on each node.</span></span> <span data-ttu-id="31c58-109">Detta gäller särskilt om data som ska kopieras till varje nod måste överföras mellan geografiska områden.</span><span class="sxs-lookup"><span data-stu-id="31c58-109">This especially applies if the data to be copied to each node must be transferred between geographic regions.</span></span>
* <span data-ttu-id="31c58-110">**Maximera minnesanvändning** när uppgifter kräver mycket minne, men endast under korta perioder tid och vid variabel tidpunkter under körningen.</span><span class="sxs-lookup"><span data-stu-id="31c58-110">**Maximizing memory usage** when tasks require a large amount of memory, but only during short periods of time, and at variable times during execution.</span></span> <span data-ttu-id="31c58-111">Du kan använda färre, men större, compute-noder med mer minne för att effektivt hantera sådana toppar.</span><span class="sxs-lookup"><span data-stu-id="31c58-111">You can employ fewer, but larger, compute nodes with more memory to efficiently handle such spikes.</span></span> <span data-ttu-id="31c58-112">Dessa noder skulle ha flera aktiviteter som körs parallellt på varje nod, men varje uppgift vill utnyttja de noder stort minne vid olika tidpunkter.</span><span class="sxs-lookup"><span data-stu-id="31c58-112">These nodes would have multiple tasks running in parallel on each node, but each task would take advantage of the nodes' plentiful memory at different times.</span></span>
* <span data-ttu-id="31c58-113">**Minimera antalet gränser för noden** när kommunikationen mellan noder krävs inom en pool.</span><span class="sxs-lookup"><span data-stu-id="31c58-113">**Mitigating node number limits** when inter-node communication is required within a pool.</span></span> <span data-ttu-id="31c58-114">Programpooler som konfigurerats för kommunikationen mellan noder för närvarande begränsad till 50 compute-noder.</span><span class="sxs-lookup"><span data-stu-id="31c58-114">Currently, pools configured for inter-node communication are limited to 50 compute nodes.</span></span> <span data-ttu-id="31c58-115">Om varje nod i denna pool kan köra uppgifter parallellt, kan ett större antal uppgifter köras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="31c58-115">If each node in such a pool is able to execute tasks in parallel, a greater number of tasks can be executed simultaneously.</span></span>
* <span data-ttu-id="31c58-116">**Replikering av en lokal beräkningskluster**, till exempel när du först flytta en beräknings-miljö till Azure.</span><span class="sxs-lookup"><span data-stu-id="31c58-116">**Replicating an on-premises compute cluster**, such as when you first move a compute environment to Azure.</span></span> <span data-ttu-id="31c58-117">Om din aktuella lösningen lokalt körs flera uppgifter per compute-nod, kan du öka det maximala antalet nod uppgifter för närmare spegling konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="31c58-117">If your current on-premises solution executes multiple tasks per compute node, you can increase the maximum number of node tasks to more closely mirror that configuration.</span></span>

## <a name="example-scenario"></a><span data-ttu-id="31c58-118">Exempelscenario</span><span class="sxs-lookup"><span data-stu-id="31c58-118">Example scenario</span></span>
<span data-ttu-id="31c58-119">Som exempel för att illustrera fördelarna med parallella uppgiftskörningen, anta att att tillämpningsprogrammet aktivitet har krav på processor och minne så att [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md) noder är tillräckliga.</span><span class="sxs-lookup"><span data-stu-id="31c58-119">As an example to illustrate the benefits of parallel task execution, let's say that your task application has CPU and memory requirements such that [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md) nodes are sufficient.</span></span> <span data-ttu-id="31c58-120">Men 1 000 av dessa noder krävs för att slutföra jobbet inom den nödvändiga tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="31c58-120">But, in order to finish the job in the required time, 1,000 of these nodes are needed.</span></span>

<span data-ttu-id="31c58-121">Istället för att använda Standard\_D1 noder som har 1 processorkärna och du kan använda [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) noder som har 16 kärnor och aktivera parallell aktivitetskörningen.</span><span class="sxs-lookup"><span data-stu-id="31c58-121">Instead of using Standard\_D1 nodes that have 1 CPU core, you could use [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) nodes that have 16 cores each, and enable parallel task execution.</span></span> <span data-ttu-id="31c58-122">Därför *16 gånger färre noder* kunde användas--i stället för 1 000 noder endast 63 skulle krävas.</span><span class="sxs-lookup"><span data-stu-id="31c58-122">Therefore, *16 times fewer nodes* could be used--instead of 1,000 nodes, only 63 would be required.</span></span> <span data-ttu-id="31c58-123">Dessutom om stora filer eller referensdata krävs för varje nod, bättre varaktighet för jobbet och effektivitet igen eftersom data kopieras till 16 noder.</span><span class="sxs-lookup"><span data-stu-id="31c58-123">Additionally, if large application files or reference data are required for each node, job duration and efficiency are again improved since the data is copied to only 16 nodes.</span></span>

## <a name="enable-parallel-task-execution"></a><span data-ttu-id="31c58-124">Aktivera körning av parallell uppgift</span><span class="sxs-lookup"><span data-stu-id="31c58-124">Enable parallel task execution</span></span>
<span data-ttu-id="31c58-125">Du kan konfigurera compute-noder för körning av parallell uppgift på pool-nivå.</span><span class="sxs-lookup"><span data-stu-id="31c58-125">You configure compute nodes for parallel task execution at the pool level.</span></span> <span data-ttu-id="31c58-126">Med Batch .NET-biblioteket, ange den [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] egenskapen när du skapar en pool.</span><span class="sxs-lookup"><span data-stu-id="31c58-126">With the Batch .NET library, set the [CloudPool.MaxTasksPerComputeNode][maxtasks_net] property when you create a pool.</span></span> <span data-ttu-id="31c58-127">Om du använder Batch REST API, ange den [maxTasksPerNode] [ rest_addpool] element i begärandetexten under skapande av poolen.</span><span class="sxs-lookup"><span data-stu-id="31c58-127">If you are using the Batch REST API, set the [maxTasksPerNode][rest_addpool] element in the request body during pool creation.</span></span>

<span data-ttu-id="31c58-128">Azure Batch kan du ange högsta uppgifter per nod upp till fyra gånger (4 x) antal kärnor på noden.</span><span class="sxs-lookup"><span data-stu-id="31c58-128">Azure Batch allows you to set maximum tasks per node up to four times (4x) the number of node cores.</span></span> <span data-ttu-id="31c58-129">Till exempel om den har konfigurerats med noderna i storlek ”stor” (fyra kärnor) sedan `maxTasksPerNode` kan anges till 16.</span><span class="sxs-lookup"><span data-stu-id="31c58-129">For example, if the pool is configured with nodes of size "Large" (four cores), then `maxTasksPerNode` may be set to 16.</span></span> <span data-ttu-id="31c58-130">Mer information om antalet kärnor för varje nod storlekar finns [storlekar för molntjänster](../cloud-services/cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="31c58-130">For details on the number of cores for each of the node sizes, see [Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md).</span></span> <span data-ttu-id="31c58-131">Mer information om tjänsten begränsar finns [kvoter och gränser för Azure Batch-tjänsten](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="31c58-131">For more information on service limits, see [Quotas and limits for the Azure Batch service](batch-quota-limit.md).</span></span>

> [!TIP]
> <span data-ttu-id="31c58-132">Se till att ta hänsyn till den `maxTasksPerNode` värdet när du skapar en [Autoskala formeln] [ enable_autoscaling] för din pool.</span><span class="sxs-lookup"><span data-stu-id="31c58-132">Be sure to take into account the `maxTasksPerNode` value when you construct an [autoscale formula][enable_autoscaling] for your pool.</span></span> <span data-ttu-id="31c58-133">Till exempel en formel som utvärderas `$RunningTasks` kraftigt kan påverkas av en ökning av uppgifter per nod.</span><span class="sxs-lookup"><span data-stu-id="31c58-133">For example, a formula that evaluates `$RunningTasks` could be dramatically affected by an increase in tasks per node.</span></span> <span data-ttu-id="31c58-134">Se [automatiskt skala compute-noder i en Azure Batch-pool](batch-automatic-scaling.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="31c58-134">See [Automatically scale compute nodes in an Azure Batch pool](batch-automatic-scaling.md) for more information.</span></span>
>
>

## <a name="distribution-of-tasks"></a><span data-ttu-id="31c58-135">Distribution av uppgifter</span><span class="sxs-lookup"><span data-stu-id="31c58-135">Distribution of tasks</span></span>
<span data-ttu-id="31c58-136">När compute-noder i en pool kan köra aktiviteter samtidigt, är det viktigt att du anger du hur aktiviteter ska distribueras mellan noderna i poolen.</span><span class="sxs-lookup"><span data-stu-id="31c58-136">When the compute nodes in a pool can execute tasks concurrently, it's important to specify how you want the tasks to be distributed across the nodes in the pool.</span></span>

<span data-ttu-id="31c58-137">Med hjälp av den [CloudPool.TaskSchedulingPolicy] [ task_schedule] egenskap, kan du ange att aktiviteter ska tilldelas jämnt över alla noder i poolen (”spridning”).</span><span class="sxs-lookup"><span data-stu-id="31c58-137">By using the [CloudPool.TaskSchedulingPolicy][task_schedule] property, you can specify that tasks should be assigned evenly across all nodes in the pool ("spreading").</span></span> <span data-ttu-id="31c58-138">Eller så kan du ange att så många aktiviteter som möjligt ska tilldelas till varje nod innan uppgifterna har tilldelats en annan nod i poolen (”packkorg”).</span><span class="sxs-lookup"><span data-stu-id="31c58-138">Or you can specify that as many tasks as possible should be assigned to each node before tasks are assigned to another node in the pool ("packing").</span></span>

<span data-ttu-id="31c58-139">Ett exempel på hur den här funktionen är viktig, bör du poolen med [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) noder (i exemplet ovan) som har konfigurerats med en [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] värde på 16.</span><span class="sxs-lookup"><span data-stu-id="31c58-139">As an example of how this feature is valuable, consider the pool of [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) nodes (in the example above) that is configured with a [CloudPool.MaxTasksPerComputeNode][maxtasks_net] value of 16.</span></span> <span data-ttu-id="31c58-140">Om den [CloudPool.TaskSchedulingPolicy] [ task_schedule] har konfigurerats med en [ComputeNodeFillType] [ fill_type] av *Pack*, den kommer att maximera användningen av alla 16 kärnor för varje nod och tillåta en [autoskalning poolen](batch-automatic-scaling.md) att Rensa oanvända noder från poolen (noder utan inga tilldelade aktiviteter).</span><span class="sxs-lookup"><span data-stu-id="31c58-140">If the [CloudPool.TaskSchedulingPolicy][task_schedule] is configured with a [ComputeNodeFillType][fill_type] of *Pack*, it would maximize usage of all 16 cores of each node and allow an [autoscaling pool](batch-automatic-scaling.md) to prune unused nodes from the pool (nodes without any tasks assigned).</span></span> <span data-ttu-id="31c58-141">Detta minimerar Resursanvändning och sparar pengar.</span><span class="sxs-lookup"><span data-stu-id="31c58-141">This minimizes resource usage and saves money.</span></span>

## <a name="batch-net-example"></a><span data-ttu-id="31c58-142">Batch .NET-exempel</span><span class="sxs-lookup"><span data-stu-id="31c58-142">Batch .NET example</span></span>
<span data-ttu-id="31c58-143">Detta [Batch .NET] [ api_net] API kodavsnitt visar en begäran om att skapa en pool som innehåller fyra stora noder med högst fyra uppgifter per nod.</span><span class="sxs-lookup"><span data-stu-id="31c58-143">This [Batch .NET][api_net] API code snippet shows a request to create a pool that contains four large nodes with a maximum of four tasks per node.</span></span> <span data-ttu-id="31c58-144">Det anger en schemaläggningstjänsten princip som fylls i varje nod med uppgifter före tilldela uppgifter till en annan nod i poolen.</span><span class="sxs-lookup"><span data-stu-id="31c58-144">It specifies a task scheduling policy that will fill each node with tasks prior to assigning tasks to another node in the pool.</span></span> <span data-ttu-id="31c58-145">Mer information om att lägga till pooler med Batch .NET API finns [BatchClient.PoolOperations.CreatePool][poolcreate_net].</span><span class="sxs-lookup"><span data-stu-id="31c58-145">For more information on adding pools by using the Batch .NET API, see [BatchClient.PoolOperations.CreatePool][poolcreate_net].</span></span>

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

## <a name="batch-rest-example"></a><span data-ttu-id="31c58-146">Batch REST-exempel</span><span class="sxs-lookup"><span data-stu-id="31c58-146">Batch REST example</span></span>
<span data-ttu-id="31c58-147">Detta [Batch REST] [ api_rest] API utdrag visar en begäran om att skapa en pool som innehåller två stora noder med högst fyra uppgifter per nod.</span><span class="sxs-lookup"><span data-stu-id="31c58-147">This [Batch REST][api_rest] API snippet shows a request to create a pool that contains two large nodes with a maximum of four tasks per node.</span></span> <span data-ttu-id="31c58-148">Mer information om att lägga till pooler med hjälp av REST-API finns [lägga till en pool med ett konto][rest_addpool].</span><span class="sxs-lookup"><span data-stu-id="31c58-148">For more information on adding pools by using the REST API, see [Add a pool to an account][rest_addpool].</span></span>

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
> <span data-ttu-id="31c58-149">Du kan ange den `maxTasksPerNode` element och [MaxTasksPerComputeNode] [ maxtasks_net] egenskapen endast vid tidpunkten för skapandet av poolen.</span><span class="sxs-lookup"><span data-stu-id="31c58-149">You can set the `maxTasksPerNode` element and [MaxTasksPerComputeNode][maxtasks_net] property only at pool creation time.</span></span> <span data-ttu-id="31c58-150">De kan inte ändras efter att poolen har redan skapats.</span><span class="sxs-lookup"><span data-stu-id="31c58-150">They cannot be modified after a pool has already been created.</span></span>
>
>

## <a name="code-sample"></a><span data-ttu-id="31c58-151">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="31c58-151">Code sample</span></span>
<span data-ttu-id="31c58-152">Den [ParallelNodeTasks] [ parallel_tasks_sample] projektet på GitHub illustrerar hur de [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] egenskapen.</span><span class="sxs-lookup"><span data-stu-id="31c58-152">The [ParallelNodeTasks][parallel_tasks_sample] project on GitHub illustrates the use of the [CloudPool.MaxTasksPerComputeNode][maxtasks_net] property.</span></span>

<span data-ttu-id="31c58-153">Den här C#-konsolprogram använder den [Batch .NET] [ api_net] bibliotek för att skapa en pool med en eller flera compute-noder.</span><span class="sxs-lookup"><span data-stu-id="31c58-153">This C# console application uses the [Batch .NET][api_net] library to create a pool with one or more compute nodes.</span></span> <span data-ttu-id="31c58-154">En konfigurerbar antalet aktiviteter som körs på dessa noder för att simulera variabeln belastning.</span><span class="sxs-lookup"><span data-stu-id="31c58-154">It executes a configurable number of tasks on those nodes to simulate variable load.</span></span> <span data-ttu-id="31c58-155">Utdata från programmet anger vilka noder utförs varje aktivitet.</span><span class="sxs-lookup"><span data-stu-id="31c58-155">Output from the application specifies which nodes executed each task.</span></span> <span data-ttu-id="31c58-156">Programmet innehåller också en sammanfattning av jobbparametrar och varaktighet.</span><span class="sxs-lookup"><span data-stu-id="31c58-156">The application also provides a summary of the job parameters and duration.</span></span> <span data-ttu-id="31c58-157">Översikt över delen av utdata från två olika körningar av exempelprogrammet visas nedan.</span><span class="sxs-lookup"><span data-stu-id="31c58-157">The summary portion of the output from two different runs of the sample application appears below.</span></span>

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

<span data-ttu-id="31c58-158">Den första körningen av exempelprogrammet visar att med en enda nod i poolen och standardinställningen för en aktivitet per nod, varaktighet för jobbet är över 30 minuter.</span><span class="sxs-lookup"><span data-stu-id="31c58-158">The first execution of the sample application shows that with a single node in the pool and the default setting of one task per node, the job duration is over 30 minutes.</span></span>

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

<span data-ttu-id="31c58-159">Andra kör av exemplet visar en betydande minskning i jobbet varaktighet.</span><span class="sxs-lookup"><span data-stu-id="31c58-159">The second run of the sample shows a significant decrease in job duration.</span></span> <span data-ttu-id="31c58-160">Det beror på att poolen har konfigurerats med fyra uppgifter per nod som möjliggör körning av parallell uppgift och slutför jobbet i nästan ett kvartal i tid.</span><span class="sxs-lookup"><span data-stu-id="31c58-160">This is because the pool was configured with four tasks per node, which allows for parallel task execution to complete the job in nearly a quarter of the time.</span></span>

> [!NOTE]
> <span data-ttu-id="31c58-161">Jobbet varaktigheter i sammanfattningar ovan omfattar inte tiden för skapandet av poolen.</span><span class="sxs-lookup"><span data-stu-id="31c58-161">The job durations in the summaries above do not include pool creation time.</span></span> <span data-ttu-id="31c58-162">Varje jobb ovan skickades till tidigare skapade pooler vars datornoderna fanns i den *inaktivt* tillstånd skicka för närvarande.</span><span class="sxs-lookup"><span data-stu-id="31c58-162">Each of the jobs above was submitted to previously created pools whose compute nodes were in the *Idle* state at submission time.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="31c58-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="31c58-163">Next steps</span></span>
### <a name="batch-explorer-heat-map"></a><span data-ttu-id="31c58-164">Batch Explorer termisk karta</span><span class="sxs-lookup"><span data-stu-id="31c58-164">Batch Explorer Heat Map</span></span>
<span data-ttu-id="31c58-165">Den [Azure Batch Explorer][batch_explorer], en Azure Batch [programexempel][github_samples], innehåller en *termisk karta*funktion som tillhandahåller visualisering av körning av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="31c58-165">The [Azure Batch Explorer][batch_explorer], one of the Azure Batch [sample applications][github_samples], contains a *Heat Map* feature that provides visualization of task execution.</span></span> <span data-ttu-id="31c58-166">När du kör den [ParallelTasks] [ parallel_tasks_sample] exempelprogrammet, kan du använda funktionen termisk karta enkelt visualisera körningen av parallella aktiviteter på varje nod.</span><span class="sxs-lookup"><span data-stu-id="31c58-166">When you're executing the [ParallelTasks][parallel_tasks_sample] sample application, you can use the Heat Map feature to easily visualize the execution of parallel tasks on each node.</span></span>

![Batch Explorer termisk karta][1]

<span data-ttu-id="31c58-168">*Batch Explorer termisk karta med en pool med fyra noder med varje nod fyra aktiviteter som körs*</span><span class="sxs-lookup"><span data-stu-id="31c58-168">*Batch Explorer Heat Map showing a pool of four nodes, with each node currently executing four tasks*</span></span>

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
