---
title: "aaaUse aktivitet beroenden toorun aktiviteter baserat på hello slutförande av andra åtgärder - Azure Batch | Microsoft Docs"
description: "Skapa uppgifter som är beroende av hello slutförande av andra uppgifter för bearbetning av MapReduce format och liknande stordata för arbetsbelastningar i Azure Batch."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: b8d12db5-ca30-4c7d-993a-a05af9257210
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: faf08ec38cb30b1f66acd51e256c31aea6215c62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-task-dependencies-toorun-tasks-that-depend-on-other-tasks"></a><span data-ttu-id="39947-103">Skapa uppgift beroenden toorun uppgifter som är beroende av andra aktiviteter</span><span class="sxs-lookup"><span data-stu-id="39947-103">Create task dependencies toorun tasks that depend on other tasks</span></span>

<span data-ttu-id="39947-104">Du kan definiera uppgiften beroenden toorun en aktivitet eller en uppsättning uppgifter som endast när en överordnad aktivitet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="39947-104">You can define task dependencies toorun a task or set of tasks only after a parent task has completed.</span></span> <span data-ttu-id="39947-105">Vissa scenarier där aktiviteten beroenden är användbara är:</span><span class="sxs-lookup"><span data-stu-id="39947-105">Some scenarios where task dependencies are useful include:</span></span>

* <span data-ttu-id="39947-106">MapReduce-format arbetsbelastningar i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="39947-106">MapReduce-style workloads in hello cloud.</span></span>
* <span data-ttu-id="39947-107">Jobb vars databearbetningsuppgifter kan uttryckas som en riktat acykliskt diagram (DAG).</span><span class="sxs-lookup"><span data-stu-id="39947-107">Jobs whose data processing tasks can be expressed as a directed acyclic graph (DAG).</span></span>
* <span data-ttu-id="39947-108">Förrendering och efter återgivning processer, där varje uppgift måste slutföras innan du kan börja hello nästa uppgift.</span><span class="sxs-lookup"><span data-stu-id="39947-108">Pre-rendering and post-rendering processes, where each task must complete before hello next task can begin.</span></span>
* <span data-ttu-id="39947-109">Ett annat jobb som underordnade aktiviteter beroende hello utdata från överordnad uppgifter.</span><span class="sxs-lookup"><span data-stu-id="39947-109">Any other job in which downstream tasks depend on hello output of upstream tasks.</span></span>

<span data-ttu-id="39947-110">Du kan skapa aktiviteter som är schemalagda för körning på datornoderna efter hello slutförande av en eller flera överordnade uppgifter med Batch uppgiften beroenden.</span><span class="sxs-lookup"><span data-stu-id="39947-110">With Batch task dependencies, you can create tasks that are scheduled for execution on compute nodes after hello completion of one or more parent tasks.</span></span> <span data-ttu-id="39947-111">Du kan till exempel skapa ett jobb som återger varje bildruta i en 3D-film med separat, parallella aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="39947-111">For example, you can create a job that renders each frame of a 3D movie with separate, parallel tasks.</span></span> <span data-ttu-id="39947-112">hello sista aktivitet - hello ”merge task”--sammanslagningar hello renderas ramar hello fullständig film när alla ramar har gjorts.</span><span class="sxs-lookup"><span data-stu-id="39947-112">hello final task--hello "merge task"--merges hello rendered frames into hello complete movie only after all frames have been successfully rendered.</span></span>

<span data-ttu-id="39947-113">Som standard är beroende aktiviteter schemalagda för körning endast när hello överordnade uppgiften har slutförts.</span><span class="sxs-lookup"><span data-stu-id="39947-113">By default, dependent tasks are scheduled for execution only after hello parent task has completed successfully.</span></span> <span data-ttu-id="39947-114">Du kan ange ett beroende åtgärden toooverride hello standardbeteende och köra uppgifter om hello överordnade misslyckas.</span><span class="sxs-lookup"><span data-stu-id="39947-114">You can specify a dependency action toooverride hello default behavior and run tasks when hello parent task fails.</span></span> <span data-ttu-id="39947-115">Se hello [beroende åtgärder](#dependency-actions) information.</span><span class="sxs-lookup"><span data-stu-id="39947-115">See hello [Dependency actions](#dependency-actions) section for details.</span></span>  

<span data-ttu-id="39947-116">Du kan skapa uppgifter som är beroende av andra aktiviteter i en-till-en- eller en-till-många-relation.</span><span class="sxs-lookup"><span data-stu-id="39947-116">You can create tasks that depend on other tasks in a one-to-one or one-to-many relationship.</span></span> <span data-ttu-id="39947-117">Du kan också skapa ett intervall beroende där en aktivitet beror på hello slutförande av en grupp aktiviteter inom ett angivet intervall för aktiviteten ID: N.</span><span class="sxs-lookup"><span data-stu-id="39947-117">You can also create a range dependency where a task depends on hello completion of a group of tasks within a specified range of task IDs.</span></span> <span data-ttu-id="39947-118">Du kan kombinera dessa tre grundläggande scenarier toocreate många-till-många-relationer.</span><span class="sxs-lookup"><span data-stu-id="39947-118">You can combine these three basic scenarios toocreate many-to-many relationships.</span></span>

## <a name="task-dependencies-with-batch-net"></a><span data-ttu-id="39947-119">Beroenden för uppgift med Batch .NET</span><span class="sxs-lookup"><span data-stu-id="39947-119">Task dependencies with Batch .NET</span></span>
<span data-ttu-id="39947-120">I den här artikeln tar vi upp hur tooconfigure samband med hjälp av hello [Batch .NET] [ net_msdn] bibliotek.</span><span class="sxs-lookup"><span data-stu-id="39947-120">In this article, we discuss how tooconfigure task dependencies by using hello [Batch .NET][net_msdn] library.</span></span> <span data-ttu-id="39947-121">Vi först att visa hur för[aktivera sambandet](#enable-task-dependencies) för dina projekt och sedan visa hur för[konfigurera en aktivitet med beroenden](#create-dependent-tasks).</span><span class="sxs-lookup"><span data-stu-id="39947-121">We first show you how too[enable task dependency](#enable-task-dependencies) on your jobs, and then demonstrate how too[configure a task with dependencies](#create-dependent-tasks).</span></span> <span data-ttu-id="39947-122">Vi beskriver också hur toospecify beroende åtgärden toorun beroende-uppgifter om hello överordnade misslyckas.</span><span class="sxs-lookup"><span data-stu-id="39947-122">We also describe how toospecify a dependency action toorun dependent tasks if hello parent fails.</span></span> <span data-ttu-id="39947-123">Dessutom diskuterar vi hello [beroende scenarier](#dependency-scenarios) som har stöd för Batch.</span><span class="sxs-lookup"><span data-stu-id="39947-123">Finally, we discuss hello [dependency scenarios](#dependency-scenarios) that Batch supports.</span></span>

## <a name="enable-task-dependencies"></a><span data-ttu-id="39947-124">Aktivera aktivitet beroenden</span><span class="sxs-lookup"><span data-stu-id="39947-124">Enable task dependencies</span></span>
<span data-ttu-id="39947-125">toouse aktivitet beroenden i Batch-program, måste du först konfigurera hello jobbet toouse aktivitet beroenden.</span><span class="sxs-lookup"><span data-stu-id="39947-125">toouse task dependencies in your Batch application, you must first configure hello job toouse task dependencies.</span></span> <span data-ttu-id="39947-126">I Batch .NET aktivera det på din [CloudJob] [ net_cloudjob] genom att ange dess [UsesTaskDependencies] [ net_usestaskdependencies] egenskapen för`true`:</span><span class="sxs-lookup"><span data-stu-id="39947-126">In Batch .NET, enable it on your [CloudJob][net_cloudjob] by setting its [UsesTaskDependencies][net_usestaskdependencies] property too`true`:</span></span>

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

<span data-ttu-id="39947-127">I föregående kodfragment hello, ”batchClient” är en instans av hello [BatchClient] [ net_batchclient] klass.</span><span class="sxs-lookup"><span data-stu-id="39947-127">In hello preceding code snippet, "batchClient" is an instance of hello [BatchClient][net_batchclient] class.</span></span>

## <a name="create-dependent-tasks"></a><span data-ttu-id="39947-128">Skapa beroende aktiviteter</span><span class="sxs-lookup"><span data-stu-id="39947-128">Create dependent tasks</span></span>
<span data-ttu-id="39947-129">toocreate en aktivitet som är beroende av hello slutförande av en eller flera överordnade uppgifter du kan ange andra uppgifter som hello uppgiften ”beror på” hello.</span><span class="sxs-lookup"><span data-stu-id="39947-129">toocreate a task that depends on hello completion of one or more parent tasks, you can specify that hello task "depends on" hello other tasks.</span></span> <span data-ttu-id="39947-130">Konfigurera hello i Batch .NET [CloudTask][net_cloudtask].[ DependsOn] [ net_dependson] egenskap med en instans av hello [TaskDependencies] [ net_taskdependencies] klass:</span><span class="sxs-lookup"><span data-stu-id="39947-130">In Batch .NET, configure hello [CloudTask][net_cloudtask].[DependsOn][net_dependson] property with an instance of hello [TaskDependencies][net_taskdependencies] class:</span></span>

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

<span data-ttu-id="39947-131">Det här kodstycket skapas en beroende aktivitet med aktivitets-ID ”blommor”.</span><span class="sxs-lookup"><span data-stu-id="39947-131">This code snippet creates a dependent task with task ID "Flowers".</span></span> <span data-ttu-id="39947-132">hello beroende ”blommor” uppgiften aktiviteter ”regn” och ”Sun”.</span><span class="sxs-lookup"><span data-stu-id="39947-132">hello "Flowers" task depends on tasks "Rain" and "Sun".</span></span> <span data-ttu-id="39947-133">Uppgiften ”blommor” är schemalagd toorun på en beräkningsnod bara efter aktiviteter som ”regn” och ”Sun” har slutförts.</span><span class="sxs-lookup"><span data-stu-id="39947-133">Task "Flowers" will be scheduled toorun on a compute node only after tasks "Rain" and "Sun" have completed successfully.</span></span>

> [!NOTE]
> <span data-ttu-id="39947-134">En aktivitet anses toobe har slutförts när den är i hello **slutförts** tillstånd och dess **slutkod** är `0`.</span><span class="sxs-lookup"><span data-stu-id="39947-134">A task is considered toobe completed successfully when it is in hello **completed** state and its **exit code** is `0`.</span></span> <span data-ttu-id="39947-135">I Batch .NET detta innebär en [CloudTask][net_cloudtask].[ Tillstånd] [ net_taskstate] egenskapsvärdet `Completed` och hello Cloudtask's [TaskExecutionInformation][net_taskexecutioninformation].[ ExitCode] [ net_exitcode] egenskapsvärdet är `0`.</span><span class="sxs-lookup"><span data-stu-id="39947-135">In Batch .NET, this means a [CloudTask][net_cloudtask].[State][net_taskstate] property value of `Completed` and hello CloudTask's [TaskExecutionInformation][net_taskexecutioninformation].[ExitCode][net_exitcode] property value is `0`.</span></span>
> 
> 

## <a name="dependency-scenarios"></a><span data-ttu-id="39947-136">Beroende scenarier</span><span class="sxs-lookup"><span data-stu-id="39947-136">Dependency scenarios</span></span>
<span data-ttu-id="39947-137">Det finns tre grundläggande uppgiften beroende scenarier som du kan använda i Azure Batch: 1, en-till-många och aktivitets-ID vara beroende.</span><span class="sxs-lookup"><span data-stu-id="39947-137">There are three basic task dependency scenarios that you can use in Azure Batch: one-to-one, one-to-many, and task ID range dependency.</span></span> <span data-ttu-id="39947-138">Dessa kan vara kombinerade tooprovide en fjärde scenario, många-till-många.</span><span class="sxs-lookup"><span data-stu-id="39947-138">These can be combined tooprovide a fourth scenario, many-to-many.</span></span>

| <span data-ttu-id="39947-139">Scenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="39947-139">Scenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span> | <span data-ttu-id="39947-140">Exempel</span><span class="sxs-lookup"><span data-stu-id="39947-140">Example</span></span> |  |
|:---:| --- | --- |
|  [<span data-ttu-id="39947-141">1</span><span class="sxs-lookup"><span data-stu-id="39947-141">One-to-one</span></span>](#one-to-one) |<span data-ttu-id="39947-142">*taskB* beror på *taskA*</span><span class="sxs-lookup"><span data-stu-id="39947-142">*taskB* depends on *taskA*</span></span> <p/> <span data-ttu-id="39947-143">*taskB* kommer inte schemaläggas för körning tills *taskA* har slutförts</span><span class="sxs-lookup"><span data-stu-id="39947-143">*taskB* will not be scheduled for execution until *taskA* has completed successfully</span></span> |<span data-ttu-id="39947-144">![Diagram: en uppgift beroende][1]</span><span class="sxs-lookup"><span data-stu-id="39947-144">![Diagram: one-to-one task dependency][1]</span></span> |
|  [<span data-ttu-id="39947-145">En-till-många</span><span class="sxs-lookup"><span data-stu-id="39947-145">One-to-many</span></span>](#one-to-many) |<span data-ttu-id="39947-146">*aktivitetC* är beroende av både *aktivitetA* och *aktivitetB*.</span><span class="sxs-lookup"><span data-stu-id="39947-146">*taskC* depends on both *taskA* and *taskB*</span></span> <p/> <span data-ttu-id="39947-147">*taskC* kommer inte schemaläggas för körning förrän både *taskA* och *taskB* har slutförts</span><span class="sxs-lookup"><span data-stu-id="39947-147">*taskC* will not be scheduled for execution until both *taskA* and *taskB* have completed successfully</span></span> |<span data-ttu-id="39947-148">![Diagram: en-till-många-uppgiften beroende][2]</span><span class="sxs-lookup"><span data-stu-id="39947-148">![Diagram: one-to-many task dependency][2]</span></span> |
|  [<span data-ttu-id="39947-149">ID-intervall för aktiviteten</span><span class="sxs-lookup"><span data-stu-id="39947-149">Task ID range</span></span>](#task-id-range) |<span data-ttu-id="39947-150">*taskD* beror på ett antal uppgifter</span><span class="sxs-lookup"><span data-stu-id="39947-150">*taskD* depends on a range of tasks</span></span> <p/> <span data-ttu-id="39947-151">*taskD* kommer inte schemaläggas för körning tills hello uppgifter med ID: N *1* via *10* har slutförts</span><span class="sxs-lookup"><span data-stu-id="39947-151">*taskD* will not be scheduled for execution until hello tasks with IDs *1* through *10* have completed successfully</span></span> |<span data-ttu-id="39947-152">![Diagram: ID-intervallet sambandet][3]</span><span class="sxs-lookup"><span data-stu-id="39947-152">![Diagram: Task id range dependency][3]</span></span> |

> [!TIP]
> <span data-ttu-id="39947-153">Du kan skapa **många-till-många** relationer, till exempel där uppgifter C, D, E och F varje beroende aktiviteter A och b Detta är användbart, till exempel i paralleliserad förbearbetning scenarier där underordnade aktiviteter är beroende av hello utdata från flera överordnade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="39947-153">You can create **many-to-many** relationships, such as where tasks C, D, E, and F each depend on tasks A and B. This is useful, for example, in parallelized preprocessing scenarios where your downstream tasks depend on hello output of multiple upstream tasks.</span></span>
> 
> <span data-ttu-id="39947-154">I hello exemplen i det här avsnittet körs en beroende aktivitet endast när hello överordnade uppgifter slutföras.</span><span class="sxs-lookup"><span data-stu-id="39947-154">In hello examples in this section, a dependent task runs only after hello parent tasks complete successfully.</span></span> <span data-ttu-id="39947-155">Det här beteendet är hello standardbeteendet för beroende aktivitet.</span><span class="sxs-lookup"><span data-stu-id="39947-155">This behavior is hello default behavior for a dependent task.</span></span> <span data-ttu-id="39947-156">Du kan köra en beroende aktivitet när en överordnad aktivitet inte genom att ange ett beroende åtgärden toooverride hello standardbeteendet.</span><span class="sxs-lookup"><span data-stu-id="39947-156">You can run a dependent task after a parent task fails by specifying a dependency action toooverride hello default behavior.</span></span> <span data-ttu-id="39947-157">Se hello [beroende åtgärder](#dependency-actions) information.</span><span class="sxs-lookup"><span data-stu-id="39947-157">See hello [Dependency actions](#dependency-actions) section for details.</span></span>

### <a name="one-to-one"></a><span data-ttu-id="39947-158">1</span><span class="sxs-lookup"><span data-stu-id="39947-158">One-to-one</span></span>
<span data-ttu-id="39947-159">En aktivitet beror på hello slutförande av en överordnad aktivitet i en-till-en-relation.</span><span class="sxs-lookup"><span data-stu-id="39947-159">In a one-to-one relationship, a task depends on hello successful completion of one parent task.</span></span> <span data-ttu-id="39947-160">toocreate Hej beroende, ange en enskild uppgift ID toohello [TaskDependencies][net_taskdependencies].[ OnId] [ net_onid] statisk metod när du fylla i hello [DependsOn] [ net_dependson] -egenskapen för [CloudTask] [ net_cloudtask].</span><span class="sxs-lookup"><span data-stu-id="39947-160">toocreate hello dependency, provide a single task ID toohello [TaskDependencies][net_taskdependencies].[OnId][net_onid] static method when you populate hello [DependsOn][net_dependson] property of [CloudTask][net_cloudtask].</span></span>

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a><span data-ttu-id="39947-161">En-till-många</span><span class="sxs-lookup"><span data-stu-id="39947-161">One-to-many</span></span>
<span data-ttu-id="39947-162">En aktivitet beror på hello slutförande av flera överordnade uppgifter i en en-till-många-relation.</span><span class="sxs-lookup"><span data-stu-id="39947-162">In a one-to-many relationship, a task depends on hello completion of multiple parent tasks.</span></span> <span data-ttu-id="39947-163">toocreate Hej beroende, tillhandahåller en uppsättning aktivitet-ID: N toohello [TaskDependencies][net_taskdependencies].[ OnIds] [ net_onids] statisk metod när du fylla i hello [DependsOn] [ net_dependson] -egenskapen för [CloudTask] [ net_cloudtask].</span><span class="sxs-lookup"><span data-stu-id="39947-163">toocreate hello dependency, provide a collection of task IDs toohello [TaskDependencies][net_taskdependencies].[OnIds][net_onids] static method when you populate hello [DependsOn][net_dependson] property of [CloudTask][net_cloudtask].</span></span>

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
``` 

### <a name="task-id-range"></a><span data-ttu-id="39947-164">ID-intervall för aktiviteten</span><span class="sxs-lookup"><span data-stu-id="39947-164">Task ID range</span></span>
<span data-ttu-id="39947-165">En aktivitet beror på hello hello slutförande av uppgifter vars ID-nummer som ligger inom ett intervall för ett beroende på ett intervall med överordnade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="39947-165">In a dependency on a range of parent tasks, a task depends on hello hello completion of tasks whose IDs lie within a range.</span></span>
<span data-ttu-id="39947-166">toocreate hello beroende ange hello första och sista uppgifts-ID i hello intervallet toohello [TaskDependencies][net_taskdependencies].[ OnIdRange] [ net_onidrange] statisk metod när du fylla i hello [DependsOn] [ net_dependson] -egenskapen för [CloudTask] [net_cloudtask].</span><span class="sxs-lookup"><span data-stu-id="39947-166">toocreate hello dependency, provide hello first and last task IDs in hello range toohello [TaskDependencies][net_taskdependencies].[OnIdRange][net_onidrange] static method when you populate hello [DependsOn][net_dependson] property of [CloudTask][net_cloudtask].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="39947-167">När du använder aktiviteten ID-intervall för dina beroenden hello aktivitets-ID i hello intervallet *måste* vara en sträng som representerar heltalsvärden.</span><span class="sxs-lookup"><span data-stu-id="39947-167">When you use task ID ranges for your dependencies, hello task IDs in hello range *must* be string representations of integer values.</span></span>
> 
> <span data-ttu-id="39947-168">Varje aktivitet i hello intervallet måste uppfylla hello beroende av slutförs eller genom att fylla i med ett fel som är mappade tooa beroende åtgärd som har angetts för**Satisfy**.</span><span class="sxs-lookup"><span data-stu-id="39947-168">Every task in hello range must satisfy hello dependency, either by completing successfully or by completing with a failure that’s mapped tooa dependency action set too**Satisfy**.</span></span> <span data-ttu-id="39947-169">Se hello [beroende åtgärder](#dependency-actions) information.</span><span class="sxs-lookup"><span data-stu-id="39947-169">See hello [Dependency actions](#dependency-actions) section for details.</span></span>
>
>

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // toouse a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters tooTaskIdRange,
    // but their ids (above) are string representations of hello ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="dependency-actions"></a><span data-ttu-id="39947-170">Beroende åtgärder</span><span class="sxs-lookup"><span data-stu-id="39947-170">Dependency actions</span></span>

<span data-ttu-id="39947-171">Som standard körs en beroende aktivitet eller uppsättning aktiviteter bara när en överordnad aktivitet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="39947-171">By default, a dependent task or set of tasks runs only after a parent task has completed successfully.</span></span> <span data-ttu-id="39947-172">I vissa fall kan kanske du vill toorun beroende aktiviteter även om hello överordnad aktivitet misslyckas.</span><span class="sxs-lookup"><span data-stu-id="39947-172">In some scenarios, you may want toorun dependent tasks even if hello parent task fails.</span></span> <span data-ttu-id="39947-173">Du kan åsidosätta standardbeteendet för hello genom att ange en beroende-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="39947-173">You can override hello default behavior by specifying a dependency action.</span></span> <span data-ttu-id="39947-174">En beroende åtgärd anger om en beroende aktivitet är berättigad toorun, baserat på hello lyckad eller misslyckad hello överordnad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="39947-174">A dependency action specifies whether a dependent task is eligible toorun, based on hello success or failure of hello parent task.</span></span> 

<span data-ttu-id="39947-175">Anta exempelvis att en beroende aktivitet väntar på data från hello hello överordnad aktivitet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="39947-175">For example, suppose that a dependent task is awaiting data from hello completion of hello upstream task.</span></span> <span data-ttu-id="39947-176">Om hello överordnade misslyckas kanske ändå hello beroende aktivitet kan toorun med äldre data.</span><span class="sxs-lookup"><span data-stu-id="39947-176">If hello upstream task fails, hello dependent task may still be able toorun using older data.</span></span> <span data-ttu-id="39947-177">I det här fallet kan en beroende-åtgärd ange hello beroende aktiviteten berättigade toorun trots hello fel i hello överordnad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="39947-177">In this case, a dependency action can specify that hello dependent task is eligible toorun despite hello failure of hello parent task.</span></span>

<span data-ttu-id="39947-178">En beroende-åtgärden är baserad på ett avsluta-villkor för hello överordnade aktivitet.</span><span class="sxs-lookup"><span data-stu-id="39947-178">A dependency action is based on an exit condition for hello parent task.</span></span> <span data-ttu-id="39947-179">Du kan ange en beroende-åtgärd för någon av följande avslutningsvillkor; hello för .NET, se hello [ExitConditions] [ net_exitconditions] klass för ytterligare information:</span><span class="sxs-lookup"><span data-stu-id="39947-179">You can specify a dependency action for any of hello following exit conditions; for .NET, see hello [ExitConditions][net_exitconditions] class for details:</span></span>

- <span data-ttu-id="39947-180">När en före bearbetning fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="39947-180">When a pre-processing error occurs.</span></span>
- <span data-ttu-id="39947-181">Felet uppstår när en filöverföring.</span><span class="sxs-lookup"><span data-stu-id="39947-181">When a file upload error occurs.</span></span> <span data-ttu-id="39947-182">Om hello aktivitet avslutas med slutkoden som angetts via **exitCodes** eller **exitCodeRanges**, och det uppstår ett fel vid, hello åtgärden som anges av hello avsluta koden har företräde.</span><span class="sxs-lookup"><span data-stu-id="39947-182">If hello task exits with an exit code that was specified via **exitCodes** or **exitCodeRanges**, and then encounters a file upload error, hello action specified by hello exit code takes precedence.</span></span>
- <span data-ttu-id="39947-183">När hello aktivitet avslutas med slutkoden definieras av hello **ExitCodes** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="39947-183">When hello task exits with an exit code defined by hello **ExitCodes** property.</span></span>
- <span data-ttu-id="39947-184">När hello aktivitet avslutas med slutkoden som ligger inom ett intervall som anges av hello **ExitCodeRanges** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="39947-184">When hello task exits with an exit code that falls within a range specified by hello **ExitCodeRanges** property.</span></span>
- <span data-ttu-id="39947-185">Hej standardfallet, om hello aktivitet avslutas med slutkoden inte definieras av **ExitCodes** eller **ExitCodeRanges**, eller om hello aktivitet avslutas med en före bearbetning fel och hello **PreProcessingError**  egenskapen har inte angetts eller om hello uppgiften misslyckas med en fil överför fel och hello **FileUploadError** egenskapen har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="39947-185">hello default case, if hello task exits with an exit code not defined by **ExitCodes** or **ExitCodeRanges**, or if hello task exits with a pre-processing error and hello **PreProcessingError** property is not set, or if hello task fails with a file upload error and hello **FileUploadError** property is not set.</span></span> 

<span data-ttu-id="39947-186">toospecify en beroende-åtgärd i .NET, ange hello [ExitOptions][net_exitoptions].[ DependencyAction] [ net_dependencyaction] -egenskapen för hello avsluta-villkor.</span><span class="sxs-lookup"><span data-stu-id="39947-186">toospecify a dependency action in .NET, set hello [ExitOptions][net_exitoptions].[DependencyAction][net_dependencyaction] property for hello exit condition.</span></span> <span data-ttu-id="39947-187">Hej **DependencyAction** egenskap tar ett av två värden:</span><span class="sxs-lookup"><span data-stu-id="39947-187">hello **DependencyAction** property takes one of two values:</span></span>

- <span data-ttu-id="39947-188">Inställningen hello **DependencyAction** egenskapen för**Satisfy** indikerar att beroende aktiviteter är berättigad toorun om hello överordnad aktivitet avslutas med ett angivet fel.</span><span class="sxs-lookup"><span data-stu-id="39947-188">Setting hello **DependencyAction** property too**Satisfy** indicates that dependent tasks are eligible toorun if hello parent task exits with a specified error.</span></span>
- <span data-ttu-id="39947-189">Inställningen hello **DependencyAction** egenskapen för**Block** visar att beroende aktiviteter inte berättigad toorun.</span><span class="sxs-lookup"><span data-stu-id="39947-189">Setting hello **DependencyAction** property too**Block** indicates that dependent tasks are not eligible toorun.</span></span>

<span data-ttu-id="39947-190">Hej standardinställningen för hello **DependencyAction** egenskapen är **Satisfy** för slutkoden 0, och **Block** för alla andra avslutningsvillkor.</span><span class="sxs-lookup"><span data-stu-id="39947-190">hello default setting for hello **DependencyAction** property is **Satisfy** for exit code 0, and **Block** for all other exit conditions.</span></span>

<span data-ttu-id="39947-191">hello följande kodavsnitt anger hello **DependencyAction** -egenskapen för en överordnad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="39947-191">hello following code snippet sets hello **DependencyAction** property for a parent task.</span></span> <span data-ttu-id="39947-192">Om hello överordnad aktivitet avslutas med ett före bearbetning fel eller med hello angivna felkoder hello beroende har aktivitet blockerats.</span><span class="sxs-lookup"><span data-stu-id="39947-192">If hello parent task exits with a pre-processing error, or with hello specified error codes, hello dependent task is blocked.</span></span> <span data-ttu-id="39947-193">Om hello överordnad aktivitet avslutas med ett noll-fel, är berättigad toorun hello beroende aktivitet.</span><span class="sxs-lookup"><span data-stu-id="39947-193">If hello parent task exits with any other non-zero error, hello dependent task is eligible toorun.</span></span>

```csharp
// Task A is hello parent task.
new CloudTask("A", "cmd.exe /c echo A")
{
    // Specify exit conditions for task A and their dependency actions.
    ExitConditions = new ExitConditions
    {
        // If task A exits with a pre-processing error, block any downstream tasks (in this example, task B).
        PreProcessingError = new ExitOptions
        {
            DependencyAction = DependencyAction.Block
        },
        // If task A exits with hello specified error codes, block any downstream tasks (in this example, task B).
        ExitCodes = new List<ExitCodeMapping>
        {
            new ExitCodeMapping(10, new ExitOptions() { DependencyAction = DependencyAction.Block }),
            new ExitCodeMapping(20, new ExitOptions() { DependencyAction = DependencyAction.Block })
        },
        // If task A succeeds or fails with any other error, any downstream tasks become eligible toorun 
        // (in this example, task B).
        Default = new ExitOptions
        {
            DependencyAction = DependencyAction.Satisfy
        }
    }
},
// Task B depends on task A. Whether it becomes eligible toorun depends on how task A exits.
new CloudTask("B", "cmd.exe /c echo B")
{
    DependsOn = TaskDependencies.OnId("A")
},
```

## <a name="code-sample"></a><span data-ttu-id="39947-194">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="39947-194">Code sample</span></span>
<span data-ttu-id="39947-195">Hej [TaskDependencies] [ github_taskdependencies] exempelprojektet är en av hello [kodexempel för Azure Batch] [ github_samples] på GitHub.</span><span class="sxs-lookup"><span data-stu-id="39947-195">hello [TaskDependencies][github_taskdependencies] sample project is one of hello [Azure Batch code samples][github_samples] on GitHub.</span></span> <span data-ttu-id="39947-196">Visual Studio-lösning visar:</span><span class="sxs-lookup"><span data-stu-id="39947-196">This Visual Studio solution demonstrates:</span></span>

- <span data-ttu-id="39947-197">Hur tooenable uppgift beroende på ett jobb</span><span class="sxs-lookup"><span data-stu-id="39947-197">How tooenable task dependency on a job</span></span>
- <span data-ttu-id="39947-198">Hur toocreate uppgifter som är beroende av andra aktiviteter</span><span class="sxs-lookup"><span data-stu-id="39947-198">How toocreate tasks that depend on other tasks</span></span>
- <span data-ttu-id="39947-199">Hur tooexecute de aktiviteter på en pool med compute-noder.</span><span class="sxs-lookup"><span data-stu-id="39947-199">How tooexecute those tasks on a pool of compute nodes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="39947-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="39947-200">Next steps</span></span>
### <a name="application-deployment"></a><span data-ttu-id="39947-201">Programdistribution</span><span class="sxs-lookup"><span data-stu-id="39947-201">Application deployment</span></span>
<span data-ttu-id="39947-202">Hej [programpaket](batch-application-packages.md) funktion i Batch ger en enkelt distribuera tooboth och version hello-program som dina aktiviteter att köra compute-noder.</span><span class="sxs-lookup"><span data-stu-id="39947-202">hello [application packages](batch-application-packages.md) feature of Batch provides an easy way tooboth deploy and version hello applications that your tasks execute on compute nodes.</span></span>

### <a name="installing-applications-and-staging-data"></a><span data-ttu-id="39947-203">Installera program och mellanlagring av data</span><span class="sxs-lookup"><span data-stu-id="39947-203">Installing applications and staging data</span></span>
<span data-ttu-id="39947-204">Se [installera program och mellanlagring av data på Batch-beräkningsnoder] [ forum_post] i hello Azure Batch-forum för en översikt över metoderna för att förbereda din noder toorun uppgifter.</span><span class="sxs-lookup"><span data-stu-id="39947-204">See [Installing applications and staging data on Batch compute nodes][forum_post] in hello Azure Batch forum for an overview of methods for preparing your nodes toorun tasks.</span></span> <span data-ttu-id="39947-205">Skrivs av en av hello Azure Batch-gruppmedlemmar, på det här är en bra introduktion på hello olika sätt toocopy program compute indata för aktivitet och andra filer tooyour-noder.</span><span class="sxs-lookup"><span data-stu-id="39947-205">Written by one of hello Azure Batch team members, this post is a good primer on hello different ways toocopy applications, task input data, and other files tooyour compute nodes.</span></span>

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_exitconditions]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitconditions
[net_exitoptions]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitoptions
[net_dependencyaction]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitoptions#Microsoft_Azure_Batch_ExitOptions_DependencyAction
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagram: 1-samband"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagram: en-till-många-beroende"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagram: ID-intervallet sambandet"
