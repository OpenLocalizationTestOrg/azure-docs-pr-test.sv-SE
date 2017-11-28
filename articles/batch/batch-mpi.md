---
title: "Använda flera instanser uppgifter för att köra program MPI - Azure Batch | Microsoft Docs"
description: "Lär dig mer om att köra Message Passing Interface (MPI)-program med flera instanser aktivitetstypen i Azure Batch."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 83e34bd7-a027-4b1b-8314-759384719327
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: 5/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 77d12d6d48b22dfb3e7f09f273dffc11401bb15f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-batch"></a><span data-ttu-id="6a7cc-103">Använda flera instanser aktiviteter för att köra program för Message Passing Interface (MPI) i Batch</span><span class="sxs-lookup"><span data-stu-id="6a7cc-103">Use multi-instance tasks to run Message Passing Interface (MPI) applications in Batch</span></span>

<span data-ttu-id="6a7cc-104">Flera instanser uppgifter kan du köra en Azure Batch-uppgiften på flera compute-noder samtidigt.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-104">Multi-instance tasks allow you to run an Azure Batch task on multiple compute nodes simultaneously.</span></span> <span data-ttu-id="6a7cc-105">Dessa uppgifter gör det möjligt för höga prestanda scenarier som program för Message Passing Interface (MPI) i gruppen.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-105">These tasks enable high performance computing scenarios like Message Passing Interface (MPI) applications in Batch.</span></span> <span data-ttu-id="6a7cc-106">I den här artikeln beskrivs hur du kan köra flera instanser aktiviteter med hjälp av den [Batch .NET] [ api_net] bibliotek.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-106">In this article, you learn how to execute multi-instance tasks using the [Batch .NET][api_net] library.</span></span>

> [!NOTE]
> <span data-ttu-id="6a7cc-107">Exemplen i den här artikeln fokuserar på Batch .NET, MS-MPI och Windows compute-noder, gäller flera instanser uppgiften begrepp som beskrivs här för andra plattformar och tekniker (Python och Intel MPI för Linux-noder, till exempel).</span><span class="sxs-lookup"><span data-stu-id="6a7cc-107">While the examples in this article focus on Batch .NET, MS-MPI, and Windows compute nodes, the multi-instance task concepts discussed here are applicable to other platforms and technologies (Python and Intel MPI on Linux nodes, for example).</span></span>
>
>

## <a name="multi-instance-task-overview"></a><span data-ttu-id="6a7cc-108">Översikt över uppgifter för flera instanser</span><span class="sxs-lookup"><span data-stu-id="6a7cc-108">Multi-instance task overview</span></span>
<span data-ttu-id="6a7cc-109">I batchen, varje uppgift är normalt körs på en enda beräkningsnod--du skickar in flera aktiviteter till ett jobb, och scheman varje aktivitet för körning på en nod för Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-109">In Batch, each task is normally executed on a single compute node--you submit multiple tasks to a job, and the Batch service schedules each task for execution on a node.</span></span> <span data-ttu-id="6a7cc-110">Men genom att konfigurera en aktivitet **inställningar för flera instanser**, anger du Batch för att skapa en primär aktivitet och flera underaktiviteter som sedan körs på flera noder i stället.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-110">However, by configuring a task's **multi-instance settings**, you tell Batch to instead create one primary task and several subtasks that are then executed on multiple nodes.</span></span>

<span data-ttu-id="6a7cc-111">![Översikt över uppgifter för flera instanser][1]</span><span class="sxs-lookup"><span data-stu-id="6a7cc-111">![Multi-instance task overview][1]</span></span>

<span data-ttu-id="6a7cc-112">När du skickar en aktivitet med flera instanser för att ett jobb utför Batch flera steg unika till flera instanser uppgifter:</span><span class="sxs-lookup"><span data-stu-id="6a7cc-112">When you submit a task with multi-instance settings to a job, Batch performs several steps unique to multi-instance tasks:</span></span>

1. <span data-ttu-id="6a7cc-113">Batch-tjänsten skapar en **primära** och flera **underaktiviteter** baserat på inställningarna för flera instanser.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-113">The Batch service creates one **primary** and several **subtasks** based on the multi-instance settings.</span></span> <span data-ttu-id="6a7cc-114">Det totala antalet aktiviteter (primära plus alla underaktiviteter) stämmer med antalet **instanser** (datornoder) du anger i inställningarna för flera instanser.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-114">The total number of tasks (primary plus all subtasks) matches the number of **instances** (compute nodes) you specify in the multi-instance settings.</span></span>
2. <span data-ttu-id="6a7cc-115">Batch anger en av compute-noder som den **master**, och scheman för den primära aktiviteten ska köras i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-115">Batch designates one of the compute nodes as the **master**, and schedules the primary task to execute on the master.</span></span> <span data-ttu-id="6a7cc-116">Den schemaläggs underaktiviteter ska köras på resten av compute-noder som tilldelats aktiviteten med flera instanser, en delaktivitet per nod.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-116">It schedules the subtasks to execute on the remainder of the compute nodes allocated to the multi-instance task, one subtask per node.</span></span>
3. <span data-ttu-id="6a7cc-117">Den primära servern och alla underaktiviteter hämta någon **vanliga resursfiler** du anger i inställningarna för flera instanser.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-117">The primary and all subtasks download any **common resource files** you specify in the multi-instance settings.</span></span>
4. <span data-ttu-id="6a7cc-118">Efter den vanliga resursen filer har laddats ned, primärt och underaktiviteter kör den **samordning kommandot** du anger i inställningarna för flera instanser.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-118">After the common resource files have been downloaded, the primary and subtasks execute the **coordination command** you specify in the multi-instance settings.</span></span> <span data-ttu-id="6a7cc-119">Kommandot samordning används vanligtvis för att förbereda noder för att köra uppgiften.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-119">The coordination command is typically used to prepare nodes for executing the task.</span></span> <span data-ttu-id="6a7cc-120">Detta kan inkludera startar Bakgrundstjänster (exempelvis [Microsoft MPI][msmpi_msdn]'s `smpd.exe`) och verifiera att noderna är redo att bearbeta meddelanden mellan noder.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-120">This can include starting background services (such as [Microsoft MPI][msmpi_msdn]'s `smpd.exe`) and verifying that the nodes are ready to process inter-node messages.</span></span>
5. <span data-ttu-id="6a7cc-121">Den främsta uppgiften körs den **kommandot programmet** på huvudnoden *när* samordning-kommandot har slutförts som den primära servern och alla underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-121">The primary task executes the **application command** on the master node *after* the coordination command has been completed successfully by the primary and all subtasks.</span></span> <span data-ttu-id="6a7cc-122">Kommandot program är kommandoraden för aktiviteten med flera instanser och utförs endast för den primära aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-122">The application command is the command line of the multi-instance task itself, and is executed only by the primary task.</span></span> <span data-ttu-id="6a7cc-123">I en [MS-MPI][msmpi_msdn]-baserad lösning, det är där du kör MPI-aktiverade appen med `mpiexec.exe`.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-123">In an [MS-MPI][msmpi_msdn]-based solution, this is where you execute your MPI-enabled application using `mpiexec.exe`.</span></span>

> [!NOTE]
> <span data-ttu-id="6a7cc-124">Även om det är funktionellt distinkta ”flera instanser aktiviteten” är inte en unik Uppgiftstyp som den [startuppgift har ställts] [ net_starttask] eller [JobPreparationTask] [ net_jobprep].</span><span class="sxs-lookup"><span data-stu-id="6a7cc-124">Though it is functionally distinct, the "multi-instance task" is not a unique task type like the [StartTask][net_starttask] or [JobPreparationTask][net_jobprep].</span></span> <span data-ttu-id="6a7cc-125">Aktiviteten med flera instanser är helt enkelt en standard batchaktiviteten ([CloudTask] [ net_task] i Batch .NET) vars flera instanser inställningar har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-125">The multi-instance task is simply a standard Batch task ([CloudTask][net_task] in Batch .NET) whose multi-instance settings have been configured.</span></span> <span data-ttu-id="6a7cc-126">I den här artikeln vi refererar till detta som den **flera instanser uppgiften**.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-126">In this article, we refer to this as the **multi-instance task**.</span></span>
>
>

## <a name="requirements-for-multi-instance-tasks"></a><span data-ttu-id="6a7cc-127">Krav för flera instanser uppgifter</span><span class="sxs-lookup"><span data-stu-id="6a7cc-127">Requirements for multi-instance tasks</span></span>
<span data-ttu-id="6a7cc-128">Flera instanser uppgifter kräver en pool med **kommunikationen mellan noder aktiverat**, och med **samtidiga uppgiftskörningen inaktiveras**.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-128">Multi-instance tasks require a pool with **inter-node communication enabled**, and with **concurrent task execution disabled**.</span></span> <span data-ttu-id="6a7cc-129">Om du vill inaktivera samtidiga uppgiftskörningen, ange den [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool#Microsoft_Azure_Batch_CloudPool_MaxTasksPerComputeNode) egenskap till 1.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-129">To disable concurrent task execution, set the [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool#Microsoft_Azure_Batch_CloudPool_MaxTasksPerComputeNode) property to 1.</span></span>

<span data-ttu-id="6a7cc-130">Det här kodstycket visar hur du skapar en pool för flera instanser aktiviteter med hjälp av Batch .NET-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-130">This code snippet shows how to create a pool for multi-instance tasks using the Batch .NET library.</span></span>

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicatedComputeNodes: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

> [!NOTE]
> <span data-ttu-id="6a7cc-131">Om du försöker köra en aktivitet för flera instanser i en pool med dessa kommunikation har inaktiverats, eller med en *maxTasksPerNode* värde större än 1, aldrig schemaläggs--på obestämd tid förblir det ”aktiv”.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-131">If you try to run a multi-instance task in a pool with internode communication disabled, or with a *maxTasksPerNode* value greater than 1, the task is never scheduled--it remains indefinitely in the "active" state.</span></span> 
>
> <span data-ttu-id="6a7cc-132">Flera instanser uppgifter kan köra endast på noder i pooler som skapats efter 14 December 2015.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-132">Multi-instance tasks can execute only on nodes in pools created after 14 December 2015.</span></span>
>
>

### <a name="use-a-starttask-to-install-mpi"></a><span data-ttu-id="6a7cc-133">Använd en startuppgift har ställts för att installera MPI</span><span class="sxs-lookup"><span data-stu-id="6a7cc-133">Use a StartTask to install MPI</span></span>
<span data-ttu-id="6a7cc-134">För att köra MPI program med flera instanser uppgiften, måste du först installera en MPI-implementering (MS-MPI eller Intel MPI, till exempel) på beräkningsnoder i poolen.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-134">To run MPI applications with a multi-instance task, you first need to install an MPI implementation (MS-MPI or Intel MPI, for example) on the compute nodes in the pool.</span></span> <span data-ttu-id="6a7cc-135">Detta är ett bra tillfälle att använda en [startuppgift har ställts][net_starttask], som körs varje gång en nod ansluter till en pool eller startas.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-135">This is a good time to use a [StartTask][net_starttask], which executes whenever a node joins a pool, or is restarted.</span></span> <span data-ttu-id="6a7cc-136">Det här kodstycket skapar en startuppgift har ställts som anger installationspaketet för MS-MPI som en [resursfilen][net_resourcefile].</span><span class="sxs-lookup"><span data-stu-id="6a7cc-136">This code snippet creates a StartTask that specifies the MS-MPI setup package as a [resource file][net_resourcefile].</span></span> <span data-ttu-id="6a7cc-137">Aktiviteten starta kommandoraden körs efter resursfilen laddas ned till noden.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-137">The start task's command line is executed after the resource file is downloaded to the node.</span></span> <span data-ttu-id="6a7cc-138">I det här fallet utför kommandoraden en obevakad installation av MS-MPI.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-138">In this case, the command line performs an unattended install of MS-MPI.</span></span>

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin)),
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a><span data-ttu-id="6a7cc-139">Direktåtkomst till fjärrminne (RDMA)</span><span class="sxs-lookup"><span data-stu-id="6a7cc-139">Remote direct memory access (RDMA)</span></span>
<span data-ttu-id="6a7cc-140">När du väljer en [RDMA-kompatibla storlek](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) , till exempel A9 för beräkningsnoder i Batch-pool MPI-program kan dra nytta av nätverk för Azures hög prestanda, låg latens direkt fjärrminne access (RDMA).</span><span class="sxs-lookup"><span data-stu-id="6a7cc-140">When you choose an [RDMA-capable size](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) such as A9 for the compute nodes in your Batch pool, your MPI application can take advantage of Azure's high-performance, low-latency remote direct memory access (RDMA) network.</span></span>

<span data-ttu-id="6a7cc-141">Leta efter de storlekar som angetts som ”RDMA-kompatibla” i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="6a7cc-141">Look for the sizes specified as "RDMA capable" in the following articles:</span></span>

* <span data-ttu-id="6a7cc-142">**CloudServiceConfiguration** pooler</span><span class="sxs-lookup"><span data-stu-id="6a7cc-142">**CloudServiceConfiguration** pools</span></span>

  * <span data-ttu-id="6a7cc-143">[Storlekar för molntjänster](../cloud-services/cloud-services-sizes-specs.md) (endast Windows)</span><span class="sxs-lookup"><span data-stu-id="6a7cc-143">[Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md) (Windows only)</span></span>
* <span data-ttu-id="6a7cc-144">**VirtualMachineConfiguration** pooler</span><span class="sxs-lookup"><span data-stu-id="6a7cc-144">**VirtualMachineConfiguration** pools</span></span>

  * <span data-ttu-id="6a7cc-145">[Storlekar för virtuella datorer i Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)</span><span class="sxs-lookup"><span data-stu-id="6a7cc-145">[Sizes for virtual machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)</span></span>
  * <span data-ttu-id="6a7cc-146">[Storlekar för virtuella datorer i Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)</span><span class="sxs-lookup"><span data-stu-id="6a7cc-146">[Sizes for virtual machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)</span></span>

> [!NOTE]
> <span data-ttu-id="6a7cc-147">Dra nytta av RDMA [Linux datornoder](batch-linux-nodes.md), måste du använda **Intel MPI** på noderna.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-147">To take advantage of RDMA on [Linux compute nodes](batch-linux-nodes.md), you must use **Intel MPI** on the nodes.</span></span> <span data-ttu-id="6a7cc-148">Mer information om CloudServiceConfiguration och VirtualMachineConfiguration pooler finns i avsnittet Pool av den [Batch funktionsöversikt](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="6a7cc-148">For more information on CloudServiceConfiguration and VirtualMachineConfiguration pools, see the Pool section of the [Batch feature overview](batch-api-basics.md).</span></span>
>
>

## <a name="create-a-multi-instance-task-with-batch-net"></a><span data-ttu-id="6a7cc-149">Skapa en flerinstans-uppgift med Batch .NET</span><span class="sxs-lookup"><span data-stu-id="6a7cc-149">Create a multi-instance task with Batch .NET</span></span>
<span data-ttu-id="6a7cc-150">Nu när vi har omfattas poolen kraven och MPI installationen kan vi skapa aktiviteten med flera instanser.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-150">Now that we've covered the pool requirements and MPI package installation, let's create the multi-instance task.</span></span> <span data-ttu-id="6a7cc-151">I det här kodstycket vi skapa en standard [CloudTask][net_task], konfigurera dess [MultiInstanceSettings] [ net_multiinstance_prop] egenskapen.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-151">In this snippet, we create a standard [CloudTask][net_task], then configure its [MultiInstanceSettings][net_multiinstance_prop] property.</span></span> <span data-ttu-id="6a7cc-152">Som tidigare nämnts kan flera instanser aktiviteten är inte en distinkta aktivitetstyp, men en standard Batch-aktivitet som konfigurerats med inställningar för flera instanser.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-152">As mentioned earlier, the multi-instance task is not a distinct task type, but a standard Batch task configured with multi-instance settings.</span></span>

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a><span data-ttu-id="6a7cc-153">Primära aktiviteten och underaktiviteterna</span><span class="sxs-lookup"><span data-stu-id="6a7cc-153">Primary task and subtasks</span></span>
<span data-ttu-id="6a7cc-154">När du skapar flera instansinställningarna för en aktivitet kan ange du antalet compute-noder som ska köra aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-154">When you create the multi-instance settings for a task, you specify the number of compute nodes that are to execute the task.</span></span> <span data-ttu-id="6a7cc-155">När du skickar aktiviteten till ett jobb, Batch-tjänsten skapar en **primära** aktiviteten och det finns tillräckligt med **underaktiviteter** som tillsammans motsvarar antalet noder som du angav.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-155">When you submit the task to a job, the Batch service creates one **primary** task and enough **subtasks** that together match the number of nodes you specified.</span></span>

<span data-ttu-id="6a7cc-156">Dessa aktiviteter tilldelas ett id för heltal i intervallet 0 till *numberOfInstances* - 1.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-156">These tasks are assigned an integer id in the range of 0 to *numberOfInstances* - 1.</span></span> <span data-ttu-id="6a7cc-157">Uppgift med id 0 är den primära och alla andra ID: n är underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-157">The task with id 0 is the primary task, and all other ids are subtasks.</span></span> <span data-ttu-id="6a7cc-158">Till exempel om du skapar följande inställningar för flera instanser för en aktivitet, främsta uppgiften skulle ha ett id 0 och underaktiviteterna skulle ha ID 1 till 9.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-158">For example, if you create the following multi-instance settings for a task, the primary task would have an id of 0, and the subtasks would have ids 1 through 9.</span></span>

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a><span data-ttu-id="6a7cc-159">Huvudnod</span><span class="sxs-lookup"><span data-stu-id="6a7cc-159">Master node</span></span>
<span data-ttu-id="6a7cc-160">När du skickar en aktivitet med flera instanser, Batch-tjänsten anger en datornoderna som ”” huvudnoden och schemalägger primära aktiviteten ska köras på huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-160">When you submit a multi-instance task, the Batch service designates one of the compute nodes as the "master" node, and schedules the primary task to execute on the master node.</span></span> <span data-ttu-id="6a7cc-161">Underaktiviteter är schemalagda att köras på resten av noderna tilldelas aktiviteten med flera instanser.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-161">The subtasks are scheduled to execute on the remainder of the nodes allocated to the multi-instance task.</span></span>

## <a name="coordination-command"></a><span data-ttu-id="6a7cc-162">Samordning kommando</span><span class="sxs-lookup"><span data-stu-id="6a7cc-162">Coordination command</span></span>
<span data-ttu-id="6a7cc-163">Den **samordning kommandot** körs av både den primära servern och underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-163">The **coordination command** is executed by both the primary and subtasks.</span></span>

<span data-ttu-id="6a7cc-164">Anrop av kommandot samordning blockerar--batchen inte körs kommandot programmet tills kommandot samordning har returnerat har på alla underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-164">The invocation of the coordination command is blocking--Batch does not execute the application command until the coordination command has returned successfully for all subtasks.</span></span> <span data-ttu-id="6a7cc-165">Kommandot samordning bör därför startar alla nödvändiga Bakgrundstjänster, kontrollera att de är klara för användning och avsluta.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-165">The coordination command should therefore start any required background services, verify that they are ready for use, and then exit.</span></span> <span data-ttu-id="6a7cc-166">Till exempel avslutas kommandot samordning för en lösning med hjälp av MS-MPI version 7 startar tjänsten SMPD på noden, sedan:</span><span class="sxs-lookup"><span data-stu-id="6a7cc-166">For example, this coordination command for a solution using MS-MPI version 7 starts the SMPD service on the node, then exits:</span></span>

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

<span data-ttu-id="6a7cc-167">Observera användningen av `start` i det här kommandot samordning.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-167">Note the use of `start` in this coordination command.</span></span> <span data-ttu-id="6a7cc-168">Detta är nödvändigt eftersom den `smpd.exe` program inte returnerar omedelbart efter körning.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-168">This is required because the `smpd.exe` application does not return immediately after execution.</span></span> <span data-ttu-id="6a7cc-169">Utan att använda den [starta] [ cmd_start] kommandot samordning kommandot returnerar inte och därför skulle blockera kommandot program från att köras.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-169">Without the use of the [start][cmd_start] command, this coordination command would not return, and would therefore block the application command from running.</span></span>

## <a name="application-command"></a><span data-ttu-id="6a7cc-170">Kommandot programmet</span><span class="sxs-lookup"><span data-stu-id="6a7cc-170">Application command</span></span>
<span data-ttu-id="6a7cc-171">När den primära aktiviteten och alla underaktiviteter är klar med kommandot samordning, körs kommandoraden för aktiviteten med flera instanser av den primära aktiviteten *endast*.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-171">Once the primary task and all subtasks have finished executing the coordination command, the multi-instance task's command line is executed by the primary task *only*.</span></span> <span data-ttu-id="6a7cc-172">Vi kallar detta den **kommandot programmet** att skilja den från kommandot samordning.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-172">We call this the **application command** to distinguish it from the coordination command.</span></span>

<span data-ttu-id="6a7cc-173">För MS-MPI program kan använda kommandot program för att köra ditt MPI-aktiverade program med `mpiexec.exe`.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-173">For MS-MPI applications, use the application command to execute your MPI-enabled application with `mpiexec.exe`.</span></span> <span data-ttu-id="6a7cc-174">Här är till exempel ett programkommando för en lösning med hjälp av MS-MPI version 7:</span><span class="sxs-lookup"><span data-stu-id="6a7cc-174">For example, here is an application command for a solution using MS-MPI version 7:</span></span>

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

> [!NOTE]
> <span data-ttu-id="6a7cc-175">Eftersom MS-MPI `mpiexec.exe` använder den `CCP_NODES` variabeln som standard (se [miljövariabler](#environment-variables)) exempel programmet kommandoraden ovan utesluter den.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-175">Because MS-MPI's `mpiexec.exe` uses the `CCP_NODES` variable by default (see [Environment variables](#environment-variables)) the example application command line above excludes it.</span></span>
>
>

## <a name="environment-variables"></a><span data-ttu-id="6a7cc-176">Miljövariabler</span><span class="sxs-lookup"><span data-stu-id="6a7cc-176">Environment variables</span></span>
<span data-ttu-id="6a7cc-177">Batch skapar flera [miljövariabler] [ msdn_env_var] specifika för flera instanser uppgifter på datornoderna som allokerats till en aktivitet med flera instanser.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-177">Batch creates several [environment variables][msdn_env_var] specific to multi-instance tasks on the compute nodes allocated to a multi-instance task.</span></span> <span data-ttu-id="6a7cc-178">Din kommandorader samordning och program kan referera till dessa miljövariabler som kan skript och program som de körs.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-178">Your coordination and application command lines can reference these environment variables, as can the scripts and programs they execute.</span></span>

<span data-ttu-id="6a7cc-179">Följande miljövariabler skapas av Batch-tjänsten för användning av flera instanser uppgifter:</span><span class="sxs-lookup"><span data-stu-id="6a7cc-179">The following environment variables are created by the Batch service for use by multi-instance tasks:</span></span>

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

<span data-ttu-id="6a7cc-180">Fullständig information om dessa och andra batchen compute-nod miljövariabler, inklusive innehållet och synlighet finns [Compute-nod miljövariabler][msdn_env_var].</span><span class="sxs-lookup"><span data-stu-id="6a7cc-180">For full details on these and the other Batch compute node environment variables, including their contents and visibility, see [Compute node environment variables][msdn_env_var].</span></span>

> [!TIP]
> <span data-ttu-id="6a7cc-181">Batch Linux MPI kodexemplet innehåller ett exempel på hur många av de här miljövariablerna kan användas.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-181">The Batch Linux MPI code sample contains an example of how several of these environment variables can be used.</span></span> <span data-ttu-id="6a7cc-182">Den [samordning cmd] [ coord_cmd_example] Bash-skript hämtar vanlig tillämpning och indata-filer från Azure Storage, aktiverar en Network File System (NFS) nätverksresurs på huvudnoden och konfigurerar de andra noderna allokeras till aktiviteten med flera instanser som NFS-klienterna.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-182">The [coordination-cmd][coord_cmd_example] Bash script downloads common application and input files from Azure Storage, enables a Network File System (NFS) share on the master node, and configures the other nodes allocated to the multi-instance task as NFS clients.</span></span>
>
>

## <a name="resource-files"></a><span data-ttu-id="6a7cc-183">Resursfiler</span><span class="sxs-lookup"><span data-stu-id="6a7cc-183">Resource files</span></span>
<span data-ttu-id="6a7cc-184">Det finns två uppsättningar med resursfiler väga in för flera instanser uppgifter: **vanliga resursfiler** som *alla* uppgifter hämta (både primär och underaktiviteter), och **resursfiler** angetts för flera instanser uppgift sig själv, vilket *endast primärt* uppgift hämtningar.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-184">There are two sets of resource files to consider for multi-instance tasks: **common resource files** that *all* tasks download (both primary and subtasks), and the **resource files** specified for the multi-instance task itself, which *only the primary* task downloads.</span></span>

<span data-ttu-id="6a7cc-185">Du kan ange en eller flera **vanliga resursfiler** i inställningarna för flera instanser för en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-185">You can specify one or more **common resource files** in the multi-instance settings for a task.</span></span> <span data-ttu-id="6a7cc-186">Dessa vanliga resursfiler hämtas från [Azure Storage](../storage/common/storage-introduction.md) på varje nod **aktivitet delade katalogen** av den primära servern och alla underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-186">These common resource files are downloaded from [Azure Storage](../storage/common/storage-introduction.md) into each node's **task shared directory** by the primary and all subtasks.</span></span> <span data-ttu-id="6a7cc-187">Du kan komma åt den delade katalogen aktivitet från program och samordning kommandorader med hjälp av den `AZ_BATCH_TASK_SHARED_DIR` miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-187">You can access the task shared directory from application and coordination command lines by using the `AZ_BATCH_TASK_SHARED_DIR` environment variable.</span></span> <span data-ttu-id="6a7cc-188">Den `AZ_BATCH_TASK_SHARED_DIR` sökvägen är identiska på alla noder som tilldelats aktiviteten med flera instanser, därför kan du dela ett enda samordning kommando mellan den primära servern och alla underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-188">The `AZ_BATCH_TASK_SHARED_DIR` path is identical on every node allocated to the multi-instance task, thus you can share a single coordination command between the primary and all subtasks.</span></span> <span data-ttu-id="6a7cc-189">Batch ”delar inte” katalogen i en mening för fjärråtkomst, men du kan använda den som en monteringspunkt eller dela punkt som nämnts tidigare i tips på miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-189">Batch does not "share" the directory in a remote access sense, but you can use it as a mount or share point as mentioned earlier in the tip on environment variables.</span></span>

<span data-ttu-id="6a7cc-190">Resursfiler som du anger för aktiviteten med flera instanser hämtas till aktivitetens arbetskatalogen `AZ_BATCH_TASK_WORKING_DIR`, som standard.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-190">Resource files that you specify for the multi-instance task itself are downloaded to the task's working directory, `AZ_BATCH_TASK_WORKING_DIR`, by default.</span></span> <span data-ttu-id="6a7cc-191">Som tidigare nämnts, till skillnad från vanliga resursfiler hämtar bara den primära aktiviteten resursfiler som angetts för aktiviteten med flera instanser.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-191">As mentioned, in contrast to common resource files, only the primary task downloads resource files specified for the  multi-instance task itself.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6a7cc-192">Använd alltid miljövariablerna `AZ_BATCH_TASK_SHARED_DIR` och `AZ_BATCH_TASK_WORKING_DIR` att referera till de här katalogerna i din kommandorader.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-192">Always use the environment variables `AZ_BATCH_TASK_SHARED_DIR` and `AZ_BATCH_TASK_WORKING_DIR` to refer to these directories in your command lines.</span></span> <span data-ttu-id="6a7cc-193">Försök inte att konstruera sökvägarna manuellt.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-193">Do not attempt to construct the paths manually.</span></span>
>
>

## <a name="task-lifetime"></a><span data-ttu-id="6a7cc-194">Livslängd för aktiviteten</span><span class="sxs-lookup"><span data-stu-id="6a7cc-194">Task lifetime</span></span>
<span data-ttu-id="6a7cc-195">Livslängden för den primära aktiviteten styr livslängden för aktiviteten hela flera instanser.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-195">The lifetime of the primary task controls the lifetime of the entire multi-instance task.</span></span> <span data-ttu-id="6a7cc-196">När primärt avslutas avslutas alla underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-196">When the primary exits, all of the subtasks are terminated.</span></span> <span data-ttu-id="6a7cc-197">Slutkoden för primärt är slutkoden för aktiviteten och därför används för att avgöra lyckats eller misslyckats i aktiviteten för försök igen.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-197">The exit code of the primary is the exit code of the task, and is therefore used to determine the success or failure of the task for retry purposes.</span></span>

<span data-ttu-id="6a7cc-198">Om någon av underaktiviteterna inte avslutas med koden inte är noll, till exempel hela flera instanser aktiviteten misslyckas.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-198">If any of the subtasks fail, exiting with a non-zero return code, for example, the entire multi-instance task fails.</span></span> <span data-ttu-id="6a7cc-199">Aktiviteten med flera instanser sedan avslutas och ett nytt försök, upp till gränsen för återförsök.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-199">The multi-instance task is then terminated and retried, up to its retry limit.</span></span>

<span data-ttu-id="6a7cc-200">När du tar bort en aktivitet med flera instanser bort den primära servern och alla underaktiviteter också av Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-200">When you delete a multi-instance task, the primary and all subtasks are also deleted by the Batch service.</span></span> <span data-ttu-id="6a7cc-201">Alla underaktivitet kataloger och filer tas bort från datornoderna, precis som för en aktivitet som standard.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-201">All subtask directories and their files are deleted from the compute nodes, just as for a standard task.</span></span>

<span data-ttu-id="6a7cc-202">[TaskConstraints] [ net_taskconstraints] för en aktivitet med flera instanser, som den [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime] [ net_taskconstraint_maxwallclock], och [RetentionTime] [ net_taskconstraint_retention] gäller egenskaper, som de är för en aktivitet som standard och tillämpas på den primära servern och alla underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-202">[TaskConstraints][net_taskconstraints] for a multi-instance task, such as the [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock], and [RetentionTime][net_taskconstraint_retention] properties, are honored as they are for a standard task, and apply to the primary and all subtasks.</span></span> <span data-ttu-id="6a7cc-203">Men om du ändrar den [RetentionTime] [ net_taskconstraint_retention] egenskapen när du lägger till flera instanser uppgiften i jobbet ändringen tillämpas endast på den primära aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-203">However, if you change the [RetentionTime][net_taskconstraint_retention] property after adding the multi-instance task to the job, this change is applied only to the primary task.</span></span> <span data-ttu-id="6a7cc-204">Alla underaktiviteter fortsätta att använda ursprungligt [RetentionTime][net_taskconstraint_retention].</span><span class="sxs-lookup"><span data-stu-id="6a7cc-204">All of the subtasks continue to use the original [RetentionTime][net_taskconstraint_retention].</span></span>

<span data-ttu-id="6a7cc-205">En beräkningsnod senaste uppgiftslista visar id för en delaktivitet om den senaste aktiviteten var en del av en aktivitet med flera instanser.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-205">A compute node's recent task list reflects the id of a subtask if the recent task was part of a multi-instance task.</span></span>

## <a name="obtain-information-about-subtasks"></a><span data-ttu-id="6a7cc-206">Hämta information om underaktiviteter</span><span class="sxs-lookup"><span data-stu-id="6a7cc-206">Obtain information about subtasks</span></span>
<span data-ttu-id="6a7cc-207">Om du vill få information om underaktiviteter genom att använda Batch .NET-bibliotek, anropa den [CloudTask.ListSubtasks] [ net_task_listsubtasks] metod.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-207">To obtain information on subtasks by using the Batch .NET library, call the [CloudTask.ListSubtasks][net_task_listsubtasks] method.</span></span> <span data-ttu-id="6a7cc-208">Den här metoden returnerar information om alla underaktiviteter och information om Beräkningsnoden som körts uppgifterna.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-208">This method returns information on all subtasks, and information about the compute node that executed the tasks.</span></span> <span data-ttu-id="6a7cc-209">Från den här informationen kan bestämma du rotkatalogen för varje underaktivitet, pool-id, det aktuella tillståndet, slutkod med mera.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-209">From this information, you can determine each subtask's root directory, the pool id, its current state, exit code, and more.</span></span> <span data-ttu-id="6a7cc-210">Du kan använda den här informationen i kombination med den [PoolOperations.GetNodeFile] [ poolops_getnodefile] metod för att hämta den underaktivitet filer.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-210">You can use this information in combination with the [PoolOperations.GetNodeFile][poolops_getnodefile] method to obtain the subtask's files.</span></span> <span data-ttu-id="6a7cc-211">Observera att den här metoden inte returnera information för den primära aktiviteten (id 0).</span><span class="sxs-lookup"><span data-stu-id="6a7cc-211">Note that this method does not return information for the primary task (id 0).</span></span>

> [!NOTE]
> <span data-ttu-id="6a7cc-212">Om inte annat anges Batch .NET-metoder som fungerar på flera instanser [CloudTask] [ net_task] själva gäller *endast* till den primära aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-212">Unless otherwise stated, Batch .NET methods that operate on the multi-instance [CloudTask][net_task] itself apply *only* to the primary task.</span></span> <span data-ttu-id="6a7cc-213">Till exempel när du anropar den [CloudTask.ListNodeFiles] [ net_task_listnodefiles] metoden för en uppgift med flera instanser bara den primära aktiviteten filer som ska returneras.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-213">For example, when you call the [CloudTask.ListNodeFiles][net_task_listnodefiles] method on a multi-instance task, only the primary task's files are returned.</span></span>
>
>

<span data-ttu-id="6a7cc-214">Följande kodavsnitt visar hur du hämtar information om underaktiviteter som begär innehållet från noder som de körs.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-214">The following code snippet shows how to obtain subtask information, as well as request file contents from the nodes on which they executed.</span></span>

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == SubtaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a><span data-ttu-id="6a7cc-215">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="6a7cc-215">Code sample</span></span>
<span data-ttu-id="6a7cc-216">Den [MultiInstanceTasks] [ github_mpi] kodexempel på GitHub visar hur du använder en aktivitet med flera instanser för att köra en [MS-MPI] [ msmpi_msdn] på Batch-beräkningsnoder.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-216">The [MultiInstanceTasks][github_mpi] code sample on GitHub demonstrates how to use a multi-instance task to run an [MS-MPI][msmpi_msdn] application on Batch compute nodes.</span></span> <span data-ttu-id="6a7cc-217">Följ stegen i [förberedelse](#preparation) och [körning](#execution) att köra exemplet.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-217">Follow the steps in [Preparation](#preparation) and [Execution](#execution) to run the sample.</span></span>

### <a name="preparation"></a><span data-ttu-id="6a7cc-218">Förberedelse</span><span class="sxs-lookup"><span data-stu-id="6a7cc-218">Preparation</span></span>
1. <span data-ttu-id="6a7cc-219">Följ de två första stegen i [så att kompilera och köra ett enkelt program MS-MPI][msmpi_howto].</span><span class="sxs-lookup"><span data-stu-id="6a7cc-219">Follow the first two steps in [How to compile and run a simple MS-MPI program][msmpi_howto].</span></span> <span data-ttu-id="6a7cc-220">Det här uppfyller prerequesites för följande steg.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-220">This satisfies the prerequesites for the following step.</span></span>
2. <span data-ttu-id="6a7cc-221">Skapa en *versionen* version av den [MPIHelloWorld] [ helloworld_proj] MPI exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-221">Build a *Release* version of the [MPIHelloWorld][helloworld_proj] sample MPI program.</span></span> <span data-ttu-id="6a7cc-222">Detta är det program som körs på datornoderna av aktiviteten med flera instanser.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-222">This is the program that will be run on compute nodes by the multi-instance task.</span></span>
3. <span data-ttu-id="6a7cc-223">Skapa en zip-filen med `MPIHelloWorld.exe` (som du skapat i steg 2) och `MSMpiSetup.exe` (som du hämtade i steg 1).</span><span class="sxs-lookup"><span data-stu-id="6a7cc-223">Create a zip file containing `MPIHelloWorld.exe` (which you built step 2) and `MSMpiSetup.exe` (which you downloaded step 1).</span></span> <span data-ttu-id="6a7cc-224">Du måste ladda upp zip-fil som ett programpaket i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-224">You'll upload this zip file as an application package in the next step.</span></span>
4. <span data-ttu-id="6a7cc-225">Använd den [Azure-portalen] [ portal] att skapa en Batch [programmet](batch-application-packages.md) kallas ”MPIHelloWorld” och ange zip-filen som du skapade i föregående steg versionsnummer ”1.0” i programpaket.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-225">Use the [Azure portal][portal] to create a Batch [application](batch-application-packages.md) called "MPIHelloWorld", and specify the zip file you created in the previous step as version "1.0" of the application package.</span></span> <span data-ttu-id="6a7cc-226">Se [ladda upp och hantera program](batch-application-packages.md#upload-and-manage-applications) för mer information.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-226">See [Upload and manage applications](batch-application-packages.md#upload-and-manage-applications) for more information.</span></span>

> [!TIP]
> <span data-ttu-id="6a7cc-227">Skapa en *versionen* version av `MPIHelloWorld.exe` så att du inte inkludera eventuella ytterligare beroenden (till exempel `msvcp140d.dll` eller `vcruntime140d.dll`) i programpaketet.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-227">Build a *Release* version of `MPIHelloWorld.exe` so that you don't have to include any additional dependencies (for example, `msvcp140d.dll` or `vcruntime140d.dll`) in your application package.</span></span>
>
>

### <a name="execution"></a><span data-ttu-id="6a7cc-228">Körning</span><span class="sxs-lookup"><span data-stu-id="6a7cc-228">Execution</span></span>
1. <span data-ttu-id="6a7cc-229">Hämta den [azure-batch-samples] [ github_samples_zip] från GitHub.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-229">Download the [azure-batch-samples][github_samples_zip] from GitHub.</span></span>
2. <span data-ttu-id="6a7cc-230">Öppna MultiInstanceTasks **lösning** i Visual Studio 2015 eller senare.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-230">Open the MultiInstanceTasks **solution** in Visual Studio 2015 or newer.</span></span> <span data-ttu-id="6a7cc-231">Den `MultiInstanceTasks.sln` lösningsfilen finns:</span><span class="sxs-lookup"><span data-stu-id="6a7cc-231">The `MultiInstanceTasks.sln` solution file is located in:</span></span>

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
3. <span data-ttu-id="6a7cc-232">Ange Batch- och autentiseringsuppgifterna för ditt konto i `AccountSettings.settings` i den **Microsoft.Azure.Batch.Samples.Common** projekt.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-232">Enter your Batch and Storage account credentials in `AccountSettings.settings` in the **Microsoft.Azure.Batch.Samples.Common** project.</span></span>
4. <span data-ttu-id="6a7cc-233">**Skapa och köra** MultiInstanceTasks-lösningen för att köra MPI exempelprogram på compute-noder i en Batch-pool.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-233">**Build and run** the MultiInstanceTasks solution to execute the MPI sample application on compute nodes in a Batch pool.</span></span>
5. <span data-ttu-id="6a7cc-234">*Valfria*: Använd den [Azure-portalen] [ portal] eller [Batch Explorer] [ batch_explorer] att undersöka exempel pool, jobb och aktivitet (” MultiInstanceSamplePool ”,” MultiInstanceSampleJob ”,” MultiInstanceSampleTask ”) innan du tar bort resurserna.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-234">*Optional*: Use the [Azure portal][portal] or the [Batch Explorer][batch_explorer] to examine the sample pool, job, and task ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") before you delete the resources.</span></span>

> [!TIP]
> <span data-ttu-id="6a7cc-235">Du kan hämta [Visual Studio Community] [ visual_studio] kostnadsfritt om du inte har Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-235">You can download [Visual Studio Community][visual_studio] for free if you do not have Visual Studio.</span></span>
>
>

<span data-ttu-id="6a7cc-236">Utdata från `MultiInstanceTasks.exe` liknar följande:</span><span class="sxs-lookup"><span data-stu-id="6a7cc-236">Output from `MultiInstanceTasks.exe` is similar to the following:</span></span>

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a><span data-ttu-id="6a7cc-237">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6a7cc-237">Next steps</span></span>
* <span data-ttu-id="6a7cc-238">Microsoft HPC & Azure Batch-teamets blogg beskrivs [MPI stöd för Linux på Azure Batch][blog_mpi_linux], och innehåller information om hur du använder [OpenFOAM] [ openfoam] med Batch.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-238">The Microsoft HPC & Azure Batch Team blog discusses [MPI support for Linux on Azure Batch][blog_mpi_linux], and includes information on using [OpenFOAM][openfoam] with Batch.</span></span> <span data-ttu-id="6a7cc-239">Du kan hitta Python kodexempel för den [OpenFOAM exempel på GitHub][github_mpi].</span><span class="sxs-lookup"><span data-stu-id="6a7cc-239">You can find Python code samples for the [OpenFOAM example on GitHub][github_mpi].</span></span>
* <span data-ttu-id="6a7cc-240">Lär dig hur du [skapa pooler med Linux datornoderna](batch-linux-nodes.md) för användning i Azure Batch MPI-lösningar.</span><span class="sxs-lookup"><span data-stu-id="6a7cc-240">Learn how to [create pools of Linux compute nodes](batch-linux-nodes.md) for use in your Azure Batch MPI solutions.</span></span>

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Översikt över flera instanser"
