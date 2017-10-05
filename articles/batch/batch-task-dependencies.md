---
title: "Använd aktiviteten beroenden för att köra uppgifter baserat på slutförande av andra åtgärder - Azure Batch | Microsoft Docs"
description: "Skapa uppgifter som är beroende av andra uppgifter för bearbetning av MapReduce format och liknande stordata slutförs arbetsbelastningar i Azure Batch."
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
ms.openlocfilehash: 465306d2de8d1dbe6ba1f0cd74be720b78a50de3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-task-dependencies-to-run-tasks-that-depend-on-other-tasks"></a><span data-ttu-id="0e671-103">Skapa uppgift beroenden för att köra uppgifter som är beroende av andra aktiviteter</span><span class="sxs-lookup"><span data-stu-id="0e671-103">Create task dependencies to run tasks that depend on other tasks</span></span>

<span data-ttu-id="0e671-104">Du kan definiera beroenden aktivitet om du vill köra en aktivitet eller en uppsättning uppgifter som endast när en överordnad aktivitet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="0e671-104">You can define task dependencies to run a task or set of tasks only after a parent task has completed.</span></span> <span data-ttu-id="0e671-105">Vissa scenarier där aktiviteten beroenden är användbara är:</span><span class="sxs-lookup"><span data-stu-id="0e671-105">Some scenarios where task dependencies are useful include:</span></span>

* <span data-ttu-id="0e671-106">MapReduce-format arbetsbelastningar i molnet.</span><span class="sxs-lookup"><span data-stu-id="0e671-106">MapReduce-style workloads in the cloud.</span></span>
* <span data-ttu-id="0e671-107">Jobb vars databearbetningsuppgifter kan uttryckas som en riktat acykliskt diagram (DAG).</span><span class="sxs-lookup"><span data-stu-id="0e671-107">Jobs whose data processing tasks can be expressed as a directed acyclic graph (DAG).</span></span>
* <span data-ttu-id="0e671-108">Förrendering och efter återgivning processer, där varje uppgift måste slutföras innan nästa uppgift kan börja.</span><span class="sxs-lookup"><span data-stu-id="0e671-108">Pre-rendering and post-rendering processes, where each task must complete before the next task can begin.</span></span>
* <span data-ttu-id="0e671-109">Ett annat jobb som underordnade aktiviteter beror på utdata från överordnad uppgifter.</span><span class="sxs-lookup"><span data-stu-id="0e671-109">Any other job in which downstream tasks depend on the output of upstream tasks.</span></span>

<span data-ttu-id="0e671-110">Du kan skapa aktiviteter som är schemalagda för körning på datornoderna efter slutförandet av en eller flera överordnade uppgifter med Batch uppgiften beroenden.</span><span class="sxs-lookup"><span data-stu-id="0e671-110">With Batch task dependencies, you can create tasks that are scheduled for execution on compute nodes after the completion of one or more parent tasks.</span></span> <span data-ttu-id="0e671-111">Du kan till exempel skapa ett jobb som återger varje bildruta i en 3D-film med separat, parallella aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="0e671-111">For example, you can create a job that renders each frame of a 3D movie with separate, parallel tasks.</span></span> <span data-ttu-id="0e671-112">Slutliga uppgift--”merge uppgiften”--sammanslagningar renderade ramar fullständig film förrän alla ramar har gjorts korrekt.</span><span class="sxs-lookup"><span data-stu-id="0e671-112">The final task--the "merge task"--merges the rendered frames into the complete movie only after all frames have been successfully rendered.</span></span>

<span data-ttu-id="0e671-113">Som standard är beroende aktiviteter schemalagda för körning efter att den överordnade uppgiften har slutförts.</span><span class="sxs-lookup"><span data-stu-id="0e671-113">By default, dependent tasks are scheduled for execution only after the parent task has completed successfully.</span></span> <span data-ttu-id="0e671-114">Du kan ange en beroende-åtgärd för att åsidosätta standardbeteendet och köra uppgifter om överordnade misslyckas.</span><span class="sxs-lookup"><span data-stu-id="0e671-114">You can specify a dependency action to override the default behavior and run tasks when the parent task fails.</span></span> <span data-ttu-id="0e671-115">Finns det [beroende åtgärder](#dependency-actions) information.</span><span class="sxs-lookup"><span data-stu-id="0e671-115">See the [Dependency actions](#dependency-actions) section for details.</span></span>  

<span data-ttu-id="0e671-116">Du kan skapa uppgifter som är beroende av andra aktiviteter i en-till-en- eller en-till-många-relation.</span><span class="sxs-lookup"><span data-stu-id="0e671-116">You can create tasks that depend on other tasks in a one-to-one or one-to-many relationship.</span></span> <span data-ttu-id="0e671-117">Du kan också skapa ett intervall beroende där en aktivitet beror på slutförande av en grupp aktiviteter inom ett angivet intervall för aktiviteten ID: N.</span><span class="sxs-lookup"><span data-stu-id="0e671-117">You can also create a range dependency where a task depends on the completion of a group of tasks within a specified range of task IDs.</span></span> <span data-ttu-id="0e671-118">Du kan kombinera dessa tre grundläggande scenarier för att skapa många-till-många-relationer.</span><span class="sxs-lookup"><span data-stu-id="0e671-118">You can combine these three basic scenarios to create many-to-many relationships.</span></span>

## <a name="task-dependencies-with-batch-net"></a><span data-ttu-id="0e671-119">Beroenden för uppgift med Batch .NET</span><span class="sxs-lookup"><span data-stu-id="0e671-119">Task dependencies with Batch .NET</span></span>
<span data-ttu-id="0e671-120">I den här artikeln diskuterar vi hur du konfigurerar samband med hjälp av den [Batch .NET] [ net_msdn] bibliotek.</span><span class="sxs-lookup"><span data-stu-id="0e671-120">In this article, we discuss how to configure task dependencies by using the [Batch .NET][net_msdn] library.</span></span> <span data-ttu-id="0e671-121">Vi först att visa hur till [aktivera sambandet](#enable-task-dependencies) för dina projekt och sedan visa hur du [konfigurera en aktivitet med beroenden](#create-dependent-tasks).</span><span class="sxs-lookup"><span data-stu-id="0e671-121">We first show you how to [enable task dependency](#enable-task-dependencies) on your jobs, and then demonstrate how to [configure a task with dependencies](#create-dependent-tasks).</span></span> <span data-ttu-id="0e671-122">Vi beskriver också hur du anger ett beroende åtgärden att köra beroende aktiviteter om överordnat misslyckas.</span><span class="sxs-lookup"><span data-stu-id="0e671-122">We also describe how to specify a dependency action to run dependent tasks if the parent fails.</span></span> <span data-ttu-id="0e671-123">Dessutom diskuterar vi den [beroende scenarier](#dependency-scenarios) som har stöd för Batch.</span><span class="sxs-lookup"><span data-stu-id="0e671-123">Finally, we discuss the [dependency scenarios](#dependency-scenarios) that Batch supports.</span></span>

## <a name="enable-task-dependencies"></a><span data-ttu-id="0e671-124">Aktivera aktivitet beroenden</span><span class="sxs-lookup"><span data-stu-id="0e671-124">Enable task dependencies</span></span>
<span data-ttu-id="0e671-125">Om du vill använda aktiviteten beroenden i Batch-program, måste du först konfigurera jobbet om du vill använda aktiviteten beroenden.</span><span class="sxs-lookup"><span data-stu-id="0e671-125">To use task dependencies in your Batch application, you must first configure the job to use task dependencies.</span></span> <span data-ttu-id="0e671-126">I Batch .NET aktivera det på din [CloudJob] [ net_cloudjob] genom att ange dess [UsesTaskDependencies] [ net_usestaskdependencies] egenskapen `true`:</span><span class="sxs-lookup"><span data-stu-id="0e671-126">In Batch .NET, enable it on your [CloudJob][net_cloudjob] by setting its [UsesTaskDependencies][net_usestaskdependencies] property to `true`:</span></span>

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

<span data-ttu-id="0e671-127">I det föregående kodfragmentet ”batchClient” är en instans av den [BatchClient] [ net_batchclient] klass.</span><span class="sxs-lookup"><span data-stu-id="0e671-127">In the preceding code snippet, "batchClient" is an instance of the [BatchClient][net_batchclient] class.</span></span>

## <a name="create-dependent-tasks"></a><span data-ttu-id="0e671-128">Skapa beroende aktiviteter</span><span class="sxs-lookup"><span data-stu-id="0e671-128">Create dependent tasks</span></span>
<span data-ttu-id="0e671-129">Du kan ange att aktiviteten ”beror på” andra uppgifter för att skapa en aktivitet som är beroende av slutförandet av en eller flera överordnade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="0e671-129">To create a task that depends on the completion of one or more parent tasks, you can specify that the task "depends on" the other tasks.</span></span> <span data-ttu-id="0e671-130">Batch .NET, konfigurera den [CloudTask][net_cloudtask].[ DependsOn] [ net_dependson] egenskap med en instans av den [TaskDependencies] [ net_taskdependencies] klass:</span><span class="sxs-lookup"><span data-stu-id="0e671-130">In Batch .NET, configure the [CloudTask][net_cloudtask].[DependsOn][net_dependson] property with an instance of the [TaskDependencies][net_taskdependencies] class:</span></span>

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

<span data-ttu-id="0e671-131">Det här kodstycket skapas en beroende aktivitet med aktivitets-ID ”blommor”.</span><span class="sxs-lookup"><span data-stu-id="0e671-131">This code snippet creates a dependent task with task ID "Flowers".</span></span> <span data-ttu-id="0e671-132">Aktiviteten ”blommor” är beroende av uppgifter ”regn” och ”Sun”.</span><span class="sxs-lookup"><span data-stu-id="0e671-132">The "Flowers" task depends on tasks "Rain" and "Sun".</span></span> <span data-ttu-id="0e671-133">Uppgiften ”blommor” kommer att schemaläggas att köras på en beräkningsnod bara efter aktiviteter som ”regn” och ”Sun” har slutförts.</span><span class="sxs-lookup"><span data-stu-id="0e671-133">Task "Flowers" will be scheduled to run on a compute node only after tasks "Rain" and "Sun" have completed successfully.</span></span>

> [!NOTE]
> <span data-ttu-id="0e671-134">En aktivitet anses vara har slutförts när den är i den **slutförts** tillstånd och dess **slutkod** är `0`.</span><span class="sxs-lookup"><span data-stu-id="0e671-134">A task is considered to be completed successfully when it is in the **completed** state and its **exit code** is `0`.</span></span> <span data-ttu-id="0e671-135">I Batch .NET detta innebär en [CloudTask][net_cloudtask].[ Tillstånd] [ net_taskstate] egenskapsvärdet `Completed` och CloudTask [TaskExecutionInformation][net_taskexecutioninformation].[ ExitCode] [ net_exitcode] egenskapsvärdet är `0`.</span><span class="sxs-lookup"><span data-stu-id="0e671-135">In Batch .NET, this means a [CloudTask][net_cloudtask].[State][net_taskstate] property value of `Completed` and the CloudTask's [TaskExecutionInformation][net_taskexecutioninformation].[ExitCode][net_exitcode] property value is `0`.</span></span>
> 
> 

## <a name="dependency-scenarios"></a><span data-ttu-id="0e671-136">Beroende scenarier</span><span class="sxs-lookup"><span data-stu-id="0e671-136">Dependency scenarios</span></span>
<span data-ttu-id="0e671-137">Det finns tre grundläggande uppgiften beroende scenarier som du kan använda i Azure Batch: 1, en-till-många och aktivitets-ID vara beroende.</span><span class="sxs-lookup"><span data-stu-id="0e671-137">There are three basic task dependency scenarios that you can use in Azure Batch: one-to-one, one-to-many, and task ID range dependency.</span></span> <span data-ttu-id="0e671-138">Dessa kan kombineras för att ge en fjärde scenario, många-till-många.</span><span class="sxs-lookup"><span data-stu-id="0e671-138">These can be combined to provide a fourth scenario, many-to-many.</span></span>

| <span data-ttu-id="0e671-139">Scenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="0e671-139">Scenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span> | <span data-ttu-id="0e671-140">Exempel</span><span class="sxs-lookup"><span data-stu-id="0e671-140">Example</span></span> |  |
|:---:| --- | --- |
|  [<span data-ttu-id="0e671-141">1</span><span class="sxs-lookup"><span data-stu-id="0e671-141">One-to-one</span></span>](#one-to-one) |<span data-ttu-id="0e671-142">*taskB* beror på *taskA*</span><span class="sxs-lookup"><span data-stu-id="0e671-142">*taskB* depends on *taskA*</span></span> <p/> <span data-ttu-id="0e671-143">*taskB* kommer inte schemaläggas för körning tills *taskA* har slutförts</span><span class="sxs-lookup"><span data-stu-id="0e671-143">*taskB* will not be scheduled for execution until *taskA* has completed successfully</span></span> |<span data-ttu-id="0e671-144">![Diagram: en uppgift beroende][1]</span><span class="sxs-lookup"><span data-stu-id="0e671-144">![Diagram: one-to-one task dependency][1]</span></span> |
|  [<span data-ttu-id="0e671-145">En-till-många</span><span class="sxs-lookup"><span data-stu-id="0e671-145">One-to-many</span></span>](#one-to-many) |<span data-ttu-id="0e671-146">*aktivitetC* är beroende av både *aktivitetA* och *aktivitetB*.</span><span class="sxs-lookup"><span data-stu-id="0e671-146">*taskC* depends on both *taskA* and *taskB*</span></span> <p/> <span data-ttu-id="0e671-147">*taskC* kommer inte schemaläggas för körning förrän både *taskA* och *taskB* har slutförts</span><span class="sxs-lookup"><span data-stu-id="0e671-147">*taskC* will not be scheduled for execution until both *taskA* and *taskB* have completed successfully</span></span> |<span data-ttu-id="0e671-148">![Diagram: en-till-många-uppgiften beroende][2]</span><span class="sxs-lookup"><span data-stu-id="0e671-148">![Diagram: one-to-many task dependency][2]</span></span> |
|  [<span data-ttu-id="0e671-149">ID-intervall för aktiviteten</span><span class="sxs-lookup"><span data-stu-id="0e671-149">Task ID range</span></span>](#task-id-range) |<span data-ttu-id="0e671-150">*taskD* beror på ett antal uppgifter</span><span class="sxs-lookup"><span data-stu-id="0e671-150">*taskD* depends on a range of tasks</span></span> <p/> <span data-ttu-id="0e671-151">*taskD* kommer inte schemaläggas för körning tills uppgifter med ID: N *1* via *10* har slutförts</span><span class="sxs-lookup"><span data-stu-id="0e671-151">*taskD* will not be scheduled for execution until the tasks with IDs *1* through *10* have completed successfully</span></span> |<span data-ttu-id="0e671-152">![Diagram: ID-intervallet sambandet][3]</span><span class="sxs-lookup"><span data-stu-id="0e671-152">![Diagram: Task id range dependency][3]</span></span> |

> [!TIP]
> <span data-ttu-id="0e671-153">Du kan skapa **många-till-många** relationer, till exempel där uppgifter C, D, E och F varje beroende aktiviteter A och b Detta är användbart, till exempel i paralleliserad förbearbetning scenarier där underordnade aktiviteter beror på utdata från flera överordnade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="0e671-153">You can create **many-to-many** relationships, such as where tasks C, D, E, and F each depend on tasks A and B. This is useful, for example, in parallelized preprocessing scenarios where your downstream tasks depend on the output of multiple upstream tasks.</span></span>
> 
> <span data-ttu-id="0e671-154">I exemplen i det här avsnittet är körs en beroende aktiviteten endast när de överordnade uppgifterna slutföras.</span><span class="sxs-lookup"><span data-stu-id="0e671-154">In the examples in this section, a dependent task runs only after the parent tasks complete successfully.</span></span> <span data-ttu-id="0e671-155">Detta är standardbeteendet för beroende aktivitet.</span><span class="sxs-lookup"><span data-stu-id="0e671-155">This behavior is the default behavior for a dependent task.</span></span> <span data-ttu-id="0e671-156">Du kan köra en beroende aktivitet när en överordnad aktivitet inte genom att ange en beroende-åtgärd om du vill åsidosätta standardbeteendet.</span><span class="sxs-lookup"><span data-stu-id="0e671-156">You can run a dependent task after a parent task fails by specifying a dependency action to override the default behavior.</span></span> <span data-ttu-id="0e671-157">Finns det [beroende åtgärder](#dependency-actions) information.</span><span class="sxs-lookup"><span data-stu-id="0e671-157">See the [Dependency actions](#dependency-actions) section for details.</span></span>

### <a name="one-to-one"></a><span data-ttu-id="0e671-158">1</span><span class="sxs-lookup"><span data-stu-id="0e671-158">One-to-one</span></span>
<span data-ttu-id="0e671-159">En aktivitet beror på slutförande av en överordnad aktivitet i en-till-en-relation.</span><span class="sxs-lookup"><span data-stu-id="0e671-159">In a one-to-one relationship, a task depends on the successful completion of one parent task.</span></span> <span data-ttu-id="0e671-160">Ange en enda aktivitets-ID till för att skapa beroendet av [TaskDependencies][net_taskdependencies].[ OnId] [ net_onid] statisk metod när du fylla i den [DependsOn] [ net_dependson] -egenskapen för [CloudTask] [ net_cloudtask].</span><span class="sxs-lookup"><span data-stu-id="0e671-160">To create the dependency, provide a single task ID to the [TaskDependencies][net_taskdependencies].[OnId][net_onid] static method when you populate the [DependsOn][net_dependson] property of [CloudTask][net_cloudtask].</span></span>

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a><span data-ttu-id="0e671-161">En-till-många</span><span class="sxs-lookup"><span data-stu-id="0e671-161">One-to-many</span></span>
<span data-ttu-id="0e671-162">En aktivitet beror på slutförande av flera överordnade uppgifter i en en-till-många-relation.</span><span class="sxs-lookup"><span data-stu-id="0e671-162">In a one-to-many relationship, a task depends on the completion of multiple parent tasks.</span></span> <span data-ttu-id="0e671-163">Om du vill skapa beroendet tillhandahåller en uppsättning aktivitets-ID för den [TaskDependencies][net_taskdependencies].[ OnIds] [ net_onids] statisk metod när du fylla i den [DependsOn] [ net_dependson] -egenskapen för [CloudTask] [ net_cloudtask].</span><span class="sxs-lookup"><span data-stu-id="0e671-163">To create the dependency, provide a collection of task IDs to the [TaskDependencies][net_taskdependencies].[OnIds][net_onids] static method when you populate the [DependsOn][net_dependson] property of [CloudTask][net_cloudtask].</span></span>

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

### <a name="task-id-range"></a><span data-ttu-id="0e671-164">ID-intervall för aktiviteten</span><span class="sxs-lookup"><span data-stu-id="0e671-164">Task ID range</span></span>
<span data-ttu-id="0e671-165">Ett beroende på ett intervall med överordnade uppgifter en aktivitet är beroende av slutförande av uppgifter vars ID-nummer som ligger inom ett intervall.</span><span class="sxs-lookup"><span data-stu-id="0e671-165">In a dependency on a range of parent tasks, a task depends on the the completion of tasks whose IDs lie within a range.</span></span>
<span data-ttu-id="0e671-166">Ange först och sista uppgifts-ID i intervallet till för att skapa beroendet av [TaskDependencies][net_taskdependencies].[ OnIdRange] [ net_onidrange] statisk metod när du fylla i den [DependsOn] [ net_dependson] -egenskapen för [CloudTask] [ net_cloudtask].</span><span class="sxs-lookup"><span data-stu-id="0e671-166">To create the dependency, provide the first and last task IDs in the range to the [TaskDependencies][net_taskdependencies].[OnIdRange][net_onidrange] static method when you populate the [DependsOn][net_dependson] property of [CloudTask][net_cloudtask].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0e671-167">När du använder aktiviteten ID-intervall för dina beroenden, aktivitets-ID i intervallet *måste* vara en sträng som representerar heltalsvärden.</span><span class="sxs-lookup"><span data-stu-id="0e671-167">When you use task ID ranges for your dependencies, the task IDs in the range *must* be string representations of integer values.</span></span>
> 
> <span data-ttu-id="0e671-168">Varje aktivitet i intervallet måste uppfylla beroendet av slutförs eller genom att fylla i med ett fel som är mappad till en beroende-åtgärder som har angetts som **Satisfy**.</span><span class="sxs-lookup"><span data-stu-id="0e671-168">Every task in the range must satisfy the dependency, either by completing successfully or by completing with a failure that’s mapped to a dependency action set to **Satisfy**.</span></span> <span data-ttu-id="0e671-169">Finns det [beroende åtgärder](#dependency-actions) information.</span><span class="sxs-lookup"><span data-stu-id="0e671-169">See the [Dependency actions](#dependency-actions) section for details.</span></span>
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
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="dependency-actions"></a><span data-ttu-id="0e671-170">Beroende åtgärder</span><span class="sxs-lookup"><span data-stu-id="0e671-170">Dependency actions</span></span>

<span data-ttu-id="0e671-171">Som standard körs en beroende aktivitet eller uppsättning aktiviteter bara när en överordnad aktivitet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="0e671-171">By default, a dependent task or set of tasks runs only after a parent task has completed successfully.</span></span> <span data-ttu-id="0e671-172">I vissa fall kanske du vill köra beroende aktiviteter även om den överordnade aktiviteten misslyckas.</span><span class="sxs-lookup"><span data-stu-id="0e671-172">In some scenarios, you may want to run dependent tasks even if the parent task fails.</span></span> <span data-ttu-id="0e671-173">Du kan åsidosätta standardbeteendet genom att ange en beroende-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0e671-173">You can override the default behavior by specifying a dependency action.</span></span> <span data-ttu-id="0e671-174">En beroende åtgärd anger om en beroende aktivitet kan köras, baserat på lyckad eller misslyckad på den överordnade aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0e671-174">A dependency action specifies whether a dependent task is eligible to run, based on the success or failure of the parent task.</span></span> 

<span data-ttu-id="0e671-175">Anta exempelvis att en beroende aktivitet väntar på data från den överordnade aktiviteten slutförs.</span><span class="sxs-lookup"><span data-stu-id="0e671-175">For example, suppose that a dependent task is awaiting data from the completion of the upstream task.</span></span> <span data-ttu-id="0e671-176">Om den överordnade aktiviteten misslyckas, kan den beroende aktiviteten fortfarande att kunna köras med äldre data.</span><span class="sxs-lookup"><span data-stu-id="0e671-176">If the upstream task fails, the dependent task may still be able to run using older data.</span></span> <span data-ttu-id="0e671-177">I det här fallet kan en beroende-åtgärd ange att den beroende aktiviteten kan köras trots fel på den överordnade aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0e671-177">In this case, a dependency action can specify that the dependent task is eligible to run despite the failure of the parent task.</span></span>

<span data-ttu-id="0e671-178">En beroende-åtgärden är baserad på ett avsluta-villkor för den överordnade aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0e671-178">A dependency action is based on an exit condition for the parent task.</span></span> <span data-ttu-id="0e671-179">Du kan ange en beroende-åtgärd för någon av följande avslutningsvillkor; för .NET, finns det [ExitConditions] [ net_exitconditions] klass för ytterligare information:</span><span class="sxs-lookup"><span data-stu-id="0e671-179">You can specify a dependency action for any of the following exit conditions; for .NET, see the [ExitConditions][net_exitconditions] class for details:</span></span>

- <span data-ttu-id="0e671-180">När en före bearbetning fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="0e671-180">When a pre-processing error occurs.</span></span>
- <span data-ttu-id="0e671-181">Felet uppstår när en filöverföring.</span><span class="sxs-lookup"><span data-stu-id="0e671-181">When a file upload error occurs.</span></span> <span data-ttu-id="0e671-182">Om aktiviteten avslutas med slutkoden som angetts via **exitCodes** eller **exitCodeRanges**, och sedan möten filöverföring fel, åtgärden som angetts av slutkoden företräde.</span><span class="sxs-lookup"><span data-stu-id="0e671-182">If the task exits with an exit code that was specified via **exitCodes** or **exitCodeRanges**, and then encounters a file upload error, the action specified by the exit code takes precedence.</span></span>
- <span data-ttu-id="0e671-183">När aktiviteten avslutas med slutkoden definieras av den **ExitCodes** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="0e671-183">When the task exits with an exit code defined by the **ExitCodes** property.</span></span>
- <span data-ttu-id="0e671-184">När aktiviteten avslutas med slutkoden som ligger inom ett intervall som anges av den **ExitCodeRanges** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="0e671-184">When the task exits with an exit code that falls within a range specified by the **ExitCodeRanges** property.</span></span>
- <span data-ttu-id="0e671-185">Standardfallet om aktiviteten avslutas med slutkoden inte definieras av **ExitCodes** eller **ExitCodeRanges**, eller om aktiviteten avslutas med ett fel som före bearbetning och **PreProcessingError** egenskapen inte har angetts eller om aktiviteten misslyckas med en fil överföringsfel och **FileUploadError** egenskapen har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="0e671-185">The default case, if the task exits with an exit code not defined by **ExitCodes** or **ExitCodeRanges**, or if the task exits with a pre-processing error and the **PreProcessingError** property is not set, or if the task fails with a file upload error and the **FileUploadError** property is not set.</span></span> 

<span data-ttu-id="0e671-186">Om du vill ange en beroende-åtgärd i .NET, ange den [ExitOptions][net_exitoptions].[ DependencyAction] [ net_dependencyaction] -egenskapen för villkoret för avslutning.</span><span class="sxs-lookup"><span data-stu-id="0e671-186">To specify a dependency action in .NET, set the [ExitOptions][net_exitoptions].[DependencyAction][net_dependencyaction] property for the exit condition.</span></span> <span data-ttu-id="0e671-187">Den **DependencyAction** egenskap tar ett av två värden:</span><span class="sxs-lookup"><span data-stu-id="0e671-187">The **DependencyAction** property takes one of two values:</span></span>

- <span data-ttu-id="0e671-188">Ange den **DependencyAction** egenskapen **Satisfy** anger att beroende aktiviteter som kan köra om den överordnade aktiviteten avslutas med ett angivet fel.</span><span class="sxs-lookup"><span data-stu-id="0e671-188">Setting the **DependencyAction** property to **Satisfy** indicates that dependent tasks are eligible to run if the parent task exits with a specified error.</span></span>
- <span data-ttu-id="0e671-189">Ange den **DependencyAction** egenskapen **Block** anger beroende aktiviteter inte som kan köra.</span><span class="sxs-lookup"><span data-stu-id="0e671-189">Setting the **DependencyAction** property to **Block** indicates that dependent tasks are not eligible to run.</span></span>

<span data-ttu-id="0e671-190">Standardinställningen för den **DependencyAction** egenskapen är **Satisfy** för slutkoden 0, och **Block** för alla andra avslutningsvillkor.</span><span class="sxs-lookup"><span data-stu-id="0e671-190">The default setting for the **DependencyAction** property is **Satisfy** for exit code 0, and **Block** for all other exit conditions.</span></span>

<span data-ttu-id="0e671-191">I följande kod fragment anger den **DependencyAction** -egenskapen för en överordnad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="0e671-191">The following code snippet sets the **DependencyAction** property for a parent task.</span></span> <span data-ttu-id="0e671-192">Om den överordnade aktiviteten avslutas före bearbetning fel eller med de angivna felkoderna, blockeras den beroende aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0e671-192">If the parent task exits with a pre-processing error, or with the specified error codes, the dependent task is blocked.</span></span> <span data-ttu-id="0e671-193">Om den överordnade aktiviteten avslutas med ett noll-fel, är den beroende aktiviteten kan köras.</span><span class="sxs-lookup"><span data-stu-id="0e671-193">If the parent task exits with any other non-zero error, the dependent task is eligible to run.</span></span>

```csharp
// Task A is the parent task.
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
        // If task A exits with the specified error codes, block any downstream tasks (in this example, task B).
        ExitCodes = new List<ExitCodeMapping>
        {
            new ExitCodeMapping(10, new ExitOptions() { DependencyAction = DependencyAction.Block }),
            new ExitCodeMapping(20, new ExitOptions() { DependencyAction = DependencyAction.Block })
        },
        // If task A succeeds or fails with any other error, any downstream tasks become eligible to run 
        // (in this example, task B).
        Default = new ExitOptions
        {
            DependencyAction = DependencyAction.Satisfy
        }
    }
},
// Task B depends on task A. Whether it becomes eligible to run depends on how task A exits.
new CloudTask("B", "cmd.exe /c echo B")
{
    DependsOn = TaskDependencies.OnId("A")
},
```

## <a name="code-sample"></a><span data-ttu-id="0e671-194">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="0e671-194">Code sample</span></span>
<span data-ttu-id="0e671-195">Den [TaskDependencies] [ github_taskdependencies] exempelprojektet är en av de [kodexempel för Azure Batch] [ github_samples] på GitHub.</span><span class="sxs-lookup"><span data-stu-id="0e671-195">The [TaskDependencies][github_taskdependencies] sample project is one of the [Azure Batch code samples][github_samples] on GitHub.</span></span> <span data-ttu-id="0e671-196">Visual Studio-lösning visar:</span><span class="sxs-lookup"><span data-stu-id="0e671-196">This Visual Studio solution demonstrates:</span></span>

- <span data-ttu-id="0e671-197">Så här aktiverar du aktiviteten beroende av ett jobb</span><span class="sxs-lookup"><span data-stu-id="0e671-197">How to enable task dependency on a job</span></span>
- <span data-ttu-id="0e671-198">Så här skapar du aktiviteter som är beroende av andra aktiviteter</span><span class="sxs-lookup"><span data-stu-id="0e671-198">How to create tasks that depend on other tasks</span></span>
- <span data-ttu-id="0e671-199">Hur du utför aktiviteterna i en pool med compute-noder.</span><span class="sxs-lookup"><span data-stu-id="0e671-199">How to execute those tasks on a pool of compute nodes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0e671-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0e671-200">Next steps</span></span>
### <a name="application-deployment"></a><span data-ttu-id="0e671-201">Programdistribution</span><span class="sxs-lookup"><span data-stu-id="0e671-201">Application deployment</span></span>
<span data-ttu-id="0e671-202">Den [programpaket](batch-application-packages.md) funktion i Batch ger ett enkelt sätt att både distribuera och de program som dina aktiviteter att köra datornoder version.</span><span class="sxs-lookup"><span data-stu-id="0e671-202">The [application packages](batch-application-packages.md) feature of Batch provides an easy way to both deploy and version the applications that your tasks execute on compute nodes.</span></span>

### <a name="installing-applications-and-staging-data"></a><span data-ttu-id="0e671-203">Installera program och mellanlagring av data</span><span class="sxs-lookup"><span data-stu-id="0e671-203">Installing applications and staging data</span></span>
<span data-ttu-id="0e671-204">Se [installera program och mellanlagring av data på Batch-beräkningsnoder] [ forum_post] i Azure Batch-forum för en översikt över metoderna för att förbereda din noder för att köra uppgifter.</span><span class="sxs-lookup"><span data-stu-id="0e671-204">See [Installing applications and staging data on Batch compute nodes][forum_post] in the Azure Batch forum for an overview of methods for preparing your nodes to run tasks.</span></span> <span data-ttu-id="0e671-205">Denna post är skrivs av en av gruppmedlemmarna Azure Batch en bra introduktion på olika sätt att kopiera program, indata för aktivitet och andra filer till compute-noder.</span><span class="sxs-lookup"><span data-stu-id="0e671-205">Written by one of the Azure Batch team members, this post is a good primer on the different ways to copy applications, task input data, and other files to your compute nodes.</span></span>

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

<span data-ttu-id="0e671-206">[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagram: 1-samband"</span><span class="sxs-lookup"><span data-stu-id="0e671-206">[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagram: one-to-one dependency"</span></span>
<span data-ttu-id="0e671-207">[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagram: en-till-många-beroende"</span><span class="sxs-lookup"><span data-stu-id="0e671-207">[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagram: one-to-many dependency"</span></span>
<span data-ttu-id="0e671-208">[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagram: ID-intervallet sambandet"</span><span class="sxs-lookup"><span data-stu-id="0e671-208">[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagram: task id range dependency"</span></span>
