---
title: "flera instanser av aaaUse uppgifter toorun MPI program – Azure Batch | Microsoft Docs"
description: "Lär dig hur tooexecute Message Passing Interface (MPI) program med hjälp av hello flera instanser aktiviteten Skriv i Azure Batch."
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
ms.openlocfilehash: b0e3295a6aeb76267c26d5504bcff59de3dc5e22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-multi-instance-tasks-toorun-message-passing-interface-mpi-applications-in-batch"></a><span data-ttu-id="a347b-103">Använda flera instanser uppgifter toorun Message Passing Interface (MPI) program i Batch</span><span class="sxs-lookup"><span data-stu-id="a347b-103">Use multi-instance tasks toorun Message Passing Interface (MPI) applications in Batch</span></span>

<span data-ttu-id="a347b-104">Flera instanser uppgifter kan du toorun en Azure Batch-uppgift på flera compute-noder samtidigt.</span><span class="sxs-lookup"><span data-stu-id="a347b-104">Multi-instance tasks allow you toorun an Azure Batch task on multiple compute nodes simultaneously.</span></span> <span data-ttu-id="a347b-105">Dessa uppgifter gör det möjligt för höga prestanda scenarier som program för Message Passing Interface (MPI) i gruppen.</span><span class="sxs-lookup"><span data-stu-id="a347b-105">These tasks enable high performance computing scenarios like Message Passing Interface (MPI) applications in Batch.</span></span> <span data-ttu-id="a347b-106">I den här artikeln får du lära dig hur tooexecute flera instanser aktiviteter med hjälp av hello [Batch .NET] [ api_net] bibliotek.</span><span class="sxs-lookup"><span data-stu-id="a347b-106">In this article, you learn how tooexecute multi-instance tasks using hello [Batch .NET][api_net] library.</span></span>

> [!NOTE]
> <span data-ttu-id="a347b-107">Hello exemplen i den här artikeln fokuserar på Batch .NET, MS-MPI och datornoder för Windows är hello flera instanser uppgiften begrepp som beskrivs här tillämpliga tooother plattformar och tekniker (Python och Intel MPI för Linux-noder, till exempel).</span><span class="sxs-lookup"><span data-stu-id="a347b-107">While hello examples in this article focus on Batch .NET, MS-MPI, and Windows compute nodes, hello multi-instance task concepts discussed here are applicable tooother platforms and technologies (Python and Intel MPI on Linux nodes, for example).</span></span>
>
>

## <a name="multi-instance-task-overview"></a><span data-ttu-id="a347b-108">Översikt över uppgifter för flera instanser</span><span class="sxs-lookup"><span data-stu-id="a347b-108">Multi-instance task overview</span></span>
<span data-ttu-id="a347b-109">I batchen, varje uppgift är normalt körs på en enda beräkningsnod--du skickar flera uppgifter tooa jobb, och hello Batch-tjänsten scheman för varje aktivitet för körning på en nod.</span><span class="sxs-lookup"><span data-stu-id="a347b-109">In Batch, each task is normally executed on a single compute node--you submit multiple tasks tooa job, and hello Batch service schedules each task for execution on a node.</span></span> <span data-ttu-id="a347b-110">Men genom att konfigurera en aktivitet **inställningar för flera instanser**, anger du Batch tooinstead skapa en primär aktivitet och flera underaktiviteter som sedan körs på flera noder.</span><span class="sxs-lookup"><span data-stu-id="a347b-110">However, by configuring a task's **multi-instance settings**, you tell Batch tooinstead create one primary task and several subtasks that are then executed on multiple nodes.</span></span>

<span data-ttu-id="a347b-111">![Översikt över uppgifter för flera instanser][1]</span><span class="sxs-lookup"><span data-stu-id="a347b-111">![Multi-instance task overview][1]</span></span>

<span data-ttu-id="a347b-112">När du skickar en uppgift med flera instanser inställningar tooa jobbet utför Batch flera steg unikt toomulti-instans uppgifter:</span><span class="sxs-lookup"><span data-stu-id="a347b-112">When you submit a task with multi-instance settings tooa job, Batch performs several steps unique toomulti-instance tasks:</span></span>

1. <span data-ttu-id="a347b-113">hello Batch-tjänsten skapar en **primära** och flera **underaktiviteter** baserat på inställningarna för hello flera instanser.</span><span class="sxs-lookup"><span data-stu-id="a347b-113">hello Batch service creates one **primary** and several **subtasks** based on hello multi-instance settings.</span></span> <span data-ttu-id="a347b-114">hello Totalt antal aktiviteter (primära plus alla underaktiviteter) stämmer hello antalet **instanser** (datornoder) du anger i inställningarna för hello flera instanser.</span><span class="sxs-lookup"><span data-stu-id="a347b-114">hello total number of tasks (primary plus all subtasks) matches hello number of **instances** (compute nodes) you specify in hello multi-instance settings.</span></span>
2. <span data-ttu-id="a347b-115">Batch anger en hello compute-noder som hello **master**, och scheman hello främsta uppgiften tooexecute på hello master.</span><span class="sxs-lookup"><span data-stu-id="a347b-115">Batch designates one of hello compute nodes as hello **master**, and schedules hello primary task tooexecute on hello master.</span></span> <span data-ttu-id="a347b-116">Den schemaläggs hello underaktiviteter tooexecute på hello resten av hello compute-noder allokerade toohello flera instanser aktivitet, en delaktivitet per nod.</span><span class="sxs-lookup"><span data-stu-id="a347b-116">It schedules hello subtasks tooexecute on hello remainder of hello compute nodes allocated toohello multi-instance task, one subtask per node.</span></span>
3. <span data-ttu-id="a347b-117">hello primära och alla underaktiviteter hämta någon **vanliga resursfiler** du anger i inställningarna för hello flera instanser.</span><span class="sxs-lookup"><span data-stu-id="a347b-117">hello primary and all subtasks download any **common resource files** you specify in hello multi-instance settings.</span></span>
4. <span data-ttu-id="a347b-118">När du har laddats ned hello vanliga resursfiler, hello primära och underaktiviteter köra hello **samordning kommandot** du anger i inställningarna för hello flera instanser.</span><span class="sxs-lookup"><span data-stu-id="a347b-118">After hello common resource files have been downloaded, hello primary and subtasks execute hello **coordination command** you specify in hello multi-instance settings.</span></span> <span data-ttu-id="a347b-119">hello samordning kommandot är brukar användas tooprepare noder för att köra hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a347b-119">hello coordination command is typically used tooprepare nodes for executing hello task.</span></span> <span data-ttu-id="a347b-120">Detta kan inkludera startar Bakgrundstjänster (exempelvis [Microsoft MPI][msmpi_msdn]'s `smpd.exe`) och verifiera att hello noder är klar tooprocess mellan noder meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a347b-120">This can include starting background services (such as [Microsoft MPI][msmpi_msdn]'s `smpd.exe`) and verifying that hello nodes are ready tooprocess inter-node messages.</span></span>
5. <span data-ttu-id="a347b-121">hello främsta uppgiften kör hello **kommandot programmet** på hello huvudnoden *när* hello samordning kommandot har slutförts av hello primära och alla underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a347b-121">hello primary task executes hello **application command** on hello master node *after* hello coordination command has been completed successfully by hello primary and all subtasks.</span></span> <span data-ttu-id="a347b-122">kommandot för programmet hello är hello kommandoraden för hello flera instanser själva aktiviteten och körs bara av hello främsta uppgiften.</span><span class="sxs-lookup"><span data-stu-id="a347b-122">hello application command is hello command line of hello multi-instance task itself, and is executed only by hello primary task.</span></span> <span data-ttu-id="a347b-123">I en [MS-MPI][msmpi_msdn]-baserad lösning, det är där du kör MPI-aktiverade appen med `mpiexec.exe`.</span><span class="sxs-lookup"><span data-stu-id="a347b-123">In an [MS-MPI][msmpi_msdn]-based solution, this is where you execute your MPI-enabled application using `mpiexec.exe`.</span></span>

> [!NOTE]
> <span data-ttu-id="a347b-124">Även om det är funktionellt distinkta hello ”flera instanser task” är inte en unik Uppgiftstyp som hello [startuppgift har ställts] [ net_starttask] eller [JobPreparationTask] [ net_jobprep].</span><span class="sxs-lookup"><span data-stu-id="a347b-124">Though it is functionally distinct, hello "multi-instance task" is not a unique task type like hello [StartTask][net_starttask] or [JobPreparationTask][net_jobprep].</span></span> <span data-ttu-id="a347b-125">hello flera instanser aktivitet är helt enkelt en standard Batch-aktivitet ([CloudTask] [ net_task] i Batch .NET) vars flera instanser inställningar har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="a347b-125">hello multi-instance task is simply a standard Batch task ([CloudTask][net_task] in Batch .NET) whose multi-instance settings have been configured.</span></span> <span data-ttu-id="a347b-126">I den här artikeln finns vi toothis som hello **flera instanser uppgiften**.</span><span class="sxs-lookup"><span data-stu-id="a347b-126">In this article, we refer toothis as hello **multi-instance task**.</span></span>
>
>

## <a name="requirements-for-multi-instance-tasks"></a><span data-ttu-id="a347b-127">Krav för flera instanser uppgifter</span><span class="sxs-lookup"><span data-stu-id="a347b-127">Requirements for multi-instance tasks</span></span>
<span data-ttu-id="a347b-128">Flera instanser uppgifter kräver en pool med **kommunikationen mellan noder aktiverat**, och med **samtidiga uppgiftskörningen inaktiveras**.</span><span class="sxs-lookup"><span data-stu-id="a347b-128">Multi-instance tasks require a pool with **inter-node communication enabled**, and with **concurrent task execution disabled**.</span></span> <span data-ttu-id="a347b-129">toodisable samtidiga uppgiftskörningen, ange hello [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool#Microsoft_Azure_Batch_CloudPool_MaxTasksPerComputeNode) egenskapen too1.</span><span class="sxs-lookup"><span data-stu-id="a347b-129">toodisable concurrent task execution, set hello [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool#Microsoft_Azure_Batch_CloudPool_MaxTasksPerComputeNode) property too1.</span></span>

<span data-ttu-id="a347b-130">Det här kodstycket visar hur toocreate poolen för flera instanser av åtgärder med hello Batch .NET-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="a347b-130">This code snippet shows how toocreate a pool for multi-instance tasks using hello Batch .NET library.</span></span>

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
> <span data-ttu-id="a347b-131">Om du försöker toorun en aktivitet med flera instanser i en pool med dessa kommunikation har inaktiverats eller med en *maxTasksPerNode* värde större än 1, hello aldrig schemaläggs--på obestämd tid förblir det hello ”aktiv”.</span><span class="sxs-lookup"><span data-stu-id="a347b-131">If you try toorun a multi-instance task in a pool with internode communication disabled, or with a *maxTasksPerNode* value greater than 1, hello task is never scheduled--it remains indefinitely in hello "active" state.</span></span> 
>
> <span data-ttu-id="a347b-132">Flera instanser uppgifter kan köra endast på noder i pooler som skapats efter 14 December 2015.</span><span class="sxs-lookup"><span data-stu-id="a347b-132">Multi-instance tasks can execute only on nodes in pools created after 14 December 2015.</span></span>
>
>

### <a name="use-a-starttask-tooinstall-mpi"></a><span data-ttu-id="a347b-133">Använd en startuppgift har ställts tooinstall MPI</span><span class="sxs-lookup"><span data-stu-id="a347b-133">Use a StartTask tooinstall MPI</span></span>
<span data-ttu-id="a347b-134">toorun MPI program med flera instanser uppgiften, måste du först tooinstall en MPI-implementering (MS-MPI eller Intel MPI, till exempel) på hello datornoder i hello pool.</span><span class="sxs-lookup"><span data-stu-id="a347b-134">toorun MPI applications with a multi-instance task, you first need tooinstall an MPI implementation (MS-MPI or Intel MPI, for example) on hello compute nodes in hello pool.</span></span> <span data-ttu-id="a347b-135">Detta är ett bra tillfälle toouse en [startuppgift har ställts][net_starttask], som körs varje gång en nod ansluter till en pool eller startas.</span><span class="sxs-lookup"><span data-stu-id="a347b-135">This is a good time toouse a [StartTask][net_starttask], which executes whenever a node joins a pool, or is restarted.</span></span> <span data-ttu-id="a347b-136">Det här kodstycket skapar en startuppgift har ställts som anger hello MS-MPI installationspaketet som en [resursfilen][net_resourcefile].</span><span class="sxs-lookup"><span data-stu-id="a347b-136">This code snippet creates a StartTask that specifies hello MS-MPI setup package as a [resource file][net_resourcefile].</span></span> <span data-ttu-id="a347b-137">uppgiften hello-start kommandoraden körs efter hello resursfilen är hämtade toohello nod.</span><span class="sxs-lookup"><span data-stu-id="a347b-137">hello start task's command line is executed after hello resource file is downloaded toohello node.</span></span> <span data-ttu-id="a347b-138">I det här fallet utför hello kommandoraden en obevakad installation av MS-MPI.</span><span class="sxs-lookup"><span data-stu-id="a347b-138">In this case, hello command line performs an unattended install of MS-MPI.</span></span>

```csharp
// Create a StartTask for hello pool which we use for installing MS-MPI on
// hello nodes as they join hello pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin)),
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit hello fully configured pool toohello Batch service tooactually create
// hello pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a><span data-ttu-id="a347b-139">Direktåtkomst till fjärrminne (RDMA)</span><span class="sxs-lookup"><span data-stu-id="a347b-139">Remote direct memory access (RDMA)</span></span>
<span data-ttu-id="a347b-140">När du väljer en [RDMA-kompatibla storlek](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) som A9 för hello compute-noder i Batch-pool, MPI-program kan dra nytta av nätverk för Azures hög prestanda, låg latens direkt fjärrminne access (RDMA).</span><span class="sxs-lookup"><span data-stu-id="a347b-140">When you choose an [RDMA-capable size](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) such as A9 for hello compute nodes in your Batch pool, your MPI application can take advantage of Azure's high-performance, low-latency remote direct memory access (RDMA) network.</span></span>

<span data-ttu-id="a347b-141">Leta efter hello-storlekar som angetts som ”RDMA-kompatibla” i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="a347b-141">Look for hello sizes specified as "RDMA capable" in hello following articles:</span></span>

* <span data-ttu-id="a347b-142">**CloudServiceConfiguration** pooler</span><span class="sxs-lookup"><span data-stu-id="a347b-142">**CloudServiceConfiguration** pools</span></span>

  * <span data-ttu-id="a347b-143">[Storlekar för molntjänster](../cloud-services/cloud-services-sizes-specs.md) (endast Windows)</span><span class="sxs-lookup"><span data-stu-id="a347b-143">[Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md) (Windows only)</span></span>
* <span data-ttu-id="a347b-144">**VirtualMachineConfiguration** pooler</span><span class="sxs-lookup"><span data-stu-id="a347b-144">**VirtualMachineConfiguration** pools</span></span>

  * <span data-ttu-id="a347b-145">[Storlekar för virtuella datorer i Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)</span><span class="sxs-lookup"><span data-stu-id="a347b-145">[Sizes for virtual machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)</span></span>
  * <span data-ttu-id="a347b-146">[Storlekar för virtuella datorer i Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)</span><span class="sxs-lookup"><span data-stu-id="a347b-146">[Sizes for virtual machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)</span></span>

> [!NOTE]
> <span data-ttu-id="a347b-147">tootake nytta av RDMA på [Linux datornoder](batch-linux-nodes.md), måste du använda **Intel MPI** på hello noder.</span><span class="sxs-lookup"><span data-stu-id="a347b-147">tootake advantage of RDMA on [Linux compute nodes](batch-linux-nodes.md), you must use **Intel MPI** on hello nodes.</span></span> <span data-ttu-id="a347b-148">Mer information om CloudServiceConfiguration och VirtualMachineConfiguration pooler finns hello avsnittet av hello [Batch funktionsöversikt](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="a347b-148">For more information on CloudServiceConfiguration and VirtualMachineConfiguration pools, see hello Pool section of hello [Batch feature overview](batch-api-basics.md).</span></span>
>
>

## <a name="create-a-multi-instance-task-with-batch-net"></a><span data-ttu-id="a347b-149">Skapa en flerinstans-uppgift med Batch .NET</span><span class="sxs-lookup"><span data-stu-id="a347b-149">Create a multi-instance task with Batch .NET</span></span>
<span data-ttu-id="a347b-150">Nu när vi har omfattas hello poolen krav och MPI installationen kan vi skapa hello flera instanser aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a347b-150">Now that we've covered hello pool requirements and MPI package installation, let's create hello multi-instance task.</span></span> <span data-ttu-id="a347b-151">I det här kodstycket vi skapa en standard [CloudTask][net_task], konfigurera dess [MultiInstanceSettings] [ net_multiinstance_prop] egenskapen.</span><span class="sxs-lookup"><span data-stu-id="a347b-151">In this snippet, we create a standard [CloudTask][net_task], then configure its [MultiInstanceSettings][net_multiinstance_prop] property.</span></span> <span data-ttu-id="a347b-152">Som tidigare nämnts hello flera instansuppgift är inte en distinkta aktivitetstyp, men en standard Batch-aktivitet som konfigurerats med inställningar för flera instanser.</span><span class="sxs-lookup"><span data-stu-id="a347b-152">As mentioned earlier, hello multi-instance task is not a distinct task type, but a standard Batch task configured with multi-instance settings.</span></span>

```csharp
// Create hello multi-instance task. Its command line is hello "application command"
// and will be executed *only* by hello primary, and only after hello primary and
// subtasks execute hello CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure hello task's MultiInstanceSettings. hello CoordinationCommandLine will be executed by
// hello primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit hello task toohello job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on hello nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a><span data-ttu-id="a347b-153">Primära aktiviteten och underaktiviteterna</span><span class="sxs-lookup"><span data-stu-id="a347b-153">Primary task and subtasks</span></span>
<span data-ttu-id="a347b-154">När du skapar hello flera instansinställningarna för en uppgift kan du ange hello antal compute-noder som är tooexecute hello aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a347b-154">When you create hello multi-instance settings for a task, you specify hello number of compute nodes that are tooexecute hello task.</span></span> <span data-ttu-id="a347b-155">När du skickar hello uppgiften tooa jobb hello Batch-tjänsten skapar en **primära** aktiviteten och det finns tillräckligt med **underaktiviteter** som matchar tillsammans hello antalet noder som du angav.</span><span class="sxs-lookup"><span data-stu-id="a347b-155">When you submit hello task tooa job, hello Batch service creates one **primary** task and enough **subtasks** that together match hello number of nodes you specified.</span></span>

<span data-ttu-id="a347b-156">Dessa aktiviteter har tilldelats ett heltal-id i hello intervallet 0 för*numberOfInstances* - 1.</span><span class="sxs-lookup"><span data-stu-id="a347b-156">These tasks are assigned an integer id in hello range of 0 too*numberOfInstances* - 1.</span></span> <span data-ttu-id="a347b-157">hello uppgift med id 0 är hello primära och alla andra ID: n är underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a347b-157">hello task with id 0 is hello primary task, and all other ids are subtasks.</span></span> <span data-ttu-id="a347b-158">Till exempel om du skapar hello följande inställningar för flera instanser för en uppgift hello främsta uppgiften skulle ha ett id 0 och hello underaktiviteter skulle ha ID 1 till 9.</span><span class="sxs-lookup"><span data-stu-id="a347b-158">For example, if you create hello following multi-instance settings for a task, hello primary task would have an id of 0, and hello subtasks would have ids 1 through 9.</span></span>

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a><span data-ttu-id="a347b-159">Huvudnod</span><span class="sxs-lookup"><span data-stu-id="a347b-159">Master node</span></span>
<span data-ttu-id="a347b-160">När du skickar en aktivitet med flera instanser hello Batch-tjänsten anger en hello compute-noder som hello ”huvudnoden” och scheman hello främsta uppgiften tooexecute på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="a347b-160">When you submit a multi-instance task, hello Batch service designates one of hello compute nodes as hello "master" node, and schedules hello primary task tooexecute on hello master node.</span></span> <span data-ttu-id="a347b-161">hello underaktiviteter är schemalagda tooexecute på hello resten av hello noder allokeras toohello flera instanser aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a347b-161">hello subtasks are scheduled tooexecute on hello remainder of hello nodes allocated toohello multi-instance task.</span></span>

## <a name="coordination-command"></a><span data-ttu-id="a347b-162">Samordning kommando</span><span class="sxs-lookup"><span data-stu-id="a347b-162">Coordination command</span></span>
<span data-ttu-id="a347b-163">Hej **samordning kommandot** körs både hello primära och underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a347b-163">hello **coordination command** is executed by both hello primary and subtasks.</span></span>

<span data-ttu-id="a347b-164">hello anrop av hello samordning kommandot blockerar--batchen inte körs kommandot för programmet hello tills hello samordning kommandot har returnerat har på alla underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a347b-164">hello invocation of hello coordination command is blocking--Batch does not execute hello application command until hello coordination command has returned successfully for all subtasks.</span></span> <span data-ttu-id="a347b-165">hello samordning kommandot bör därför startar alla nödvändiga Bakgrundstjänster, kontrollera att de är klara för användning och avsluta.</span><span class="sxs-lookup"><span data-stu-id="a347b-165">hello coordination command should therefore start any required background services, verify that they are ready for use, and then exit.</span></span> <span data-ttu-id="a347b-166">Till exempel kommandot samordning för en lösning med hjälp av MS-MPI version 7 startar hello SMPD-tjänsten på hello noden och sedan avslutas:</span><span class="sxs-lookup"><span data-stu-id="a347b-166">For example, this coordination command for a solution using MS-MPI version 7 starts hello SMPD service on hello node, then exits:</span></span>

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

<span data-ttu-id="a347b-167">Observera hello användning av `start` i det här kommandot samordning.</span><span class="sxs-lookup"><span data-stu-id="a347b-167">Note hello use of `start` in this coordination command.</span></span> <span data-ttu-id="a347b-168">Detta är nödvändigt eftersom hello `smpd.exe` program inte returnerar omedelbart efter körning.</span><span class="sxs-lookup"><span data-stu-id="a347b-168">This is required because hello `smpd.exe` application does not return immediately after execution.</span></span> <span data-ttu-id="a347b-169">Utan hello hello [starta] [ cmd_start] kommandot samordning kommandot returnerar inte och därför skulle blockera hello programmet kommandot körs.</span><span class="sxs-lookup"><span data-stu-id="a347b-169">Without hello use of hello [start][cmd_start] command, this coordination command would not return, and would therefore block hello application command from running.</span></span>

## <a name="application-command"></a><span data-ttu-id="a347b-170">Kommandot programmet</span><span class="sxs-lookup"><span data-stu-id="a347b-170">Application command</span></span>
<span data-ttu-id="a347b-171">När hello främsta uppgiften och alla underaktiviteter är klar hello samordning kommando körs hello flera instanser aktivitetens kommandoraden körs av hello främsta uppgiften *endast*.</span><span class="sxs-lookup"><span data-stu-id="a347b-171">Once hello primary task and all subtasks have finished executing hello coordination command, hello multi-instance task's command line is executed by hello primary task *only*.</span></span> <span data-ttu-id="a347b-172">Vi kallar detta hello **kommandot programmet** toodistinguish från hello samordning kommando.</span><span class="sxs-lookup"><span data-stu-id="a347b-172">We call this hello **application command** toodistinguish it from hello coordination command.</span></span>

<span data-ttu-id="a347b-173">MS-MPI program, Använd hello programmet kommandot tooexecute din MPI-aktiverat program med `mpiexec.exe`.</span><span class="sxs-lookup"><span data-stu-id="a347b-173">For MS-MPI applications, use hello application command tooexecute your MPI-enabled application with `mpiexec.exe`.</span></span> <span data-ttu-id="a347b-174">Här är till exempel ett programkommando för en lösning med hjälp av MS-MPI version 7:</span><span class="sxs-lookup"><span data-stu-id="a347b-174">For example, here is an application command for a solution using MS-MPI version 7:</span></span>

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

> [!NOTE]
> <span data-ttu-id="a347b-175">Eftersom MS-MPI `mpiexec.exe` använder hello `CCP_NODES` variabeln som standard (se [miljövariabler](#environment-variables)) hello-exemplet ovan kommandoraden för programmet utesluter den.</span><span class="sxs-lookup"><span data-stu-id="a347b-175">Because MS-MPI's `mpiexec.exe` uses hello `CCP_NODES` variable by default (see [Environment variables](#environment-variables)) hello example application command line above excludes it.</span></span>
>
>

## <a name="environment-variables"></a><span data-ttu-id="a347b-176">Miljövariabler</span><span class="sxs-lookup"><span data-stu-id="a347b-176">Environment variables</span></span>
<span data-ttu-id="a347b-177">Batch skapar flera [miljövariabler] [ msdn_env_var] specifika toomulti-instans uppgifter på hello compute-noder allokeras tooa flera instanser aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a347b-177">Batch creates several [environment variables][msdn_env_var] specific toomulti-instance tasks on hello compute nodes allocated tooa multi-instance task.</span></span> <span data-ttu-id="a347b-178">De här miljövariablerna kan referera till din kommandorader samordning och program som kan hello skript och program som de körs.</span><span class="sxs-lookup"><span data-stu-id="a347b-178">Your coordination and application command lines can reference these environment variables, as can hello scripts and programs they execute.</span></span>

<span data-ttu-id="a347b-179">hello skapas följande miljövariabler av hello Batch-tjänsten för användning av flera instanser uppgifter:</span><span class="sxs-lookup"><span data-stu-id="a347b-179">hello following environment variables are created by hello Batch service for use by multi-instance tasks:</span></span>

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

<span data-ttu-id="a347b-180">Fullständig information om detta och hello andra Batch compute-nod miljövariabler, inklusive innehållet och synlighet finns [Compute-nod miljövariabler][msdn_env_var].</span><span class="sxs-lookup"><span data-stu-id="a347b-180">For full details on these and hello other Batch compute node environment variables, including their contents and visibility, see [Compute node environment variables][msdn_env_var].</span></span>

> [!TIP]
> <span data-ttu-id="a347b-181">hello Batch Linux MPI kodexemplet innehåller ett exempel på hur många av de här miljövariablerna kan användas.</span><span class="sxs-lookup"><span data-stu-id="a347b-181">hello Batch Linux MPI code sample contains an example of how several of these environment variables can be used.</span></span> <span data-ttu-id="a347b-182">Hej [samordning cmd] [ coord_cmd_example] Bash skript hämtar vanlig tillämpning och indatafilerna från Azure Storage, aktiverar en Network File System (NFS) nätverksresurs på hello huvudnod och konfigurerar hello andra noder allokerat toohello flera instanser aktivitet som NFS-klienterna.</span><span class="sxs-lookup"><span data-stu-id="a347b-182">hello [coordination-cmd][coord_cmd_example] Bash script downloads common application and input files from Azure Storage, enables a Network File System (NFS) share on hello master node, and configures hello other nodes allocated toohello multi-instance task as NFS clients.</span></span>
>
>

## <a name="resource-files"></a><span data-ttu-id="a347b-183">Resursfiler</span><span class="sxs-lookup"><span data-stu-id="a347b-183">Resource files</span></span>
<span data-ttu-id="a347b-184">Det finns två uppsättningar av resursen filer tooconsider för flera instanser uppgifter: **vanliga resursfiler** som *alla* uppgifter hämta (både primär och underaktiviteter), och hello **resursfiler** angetts för hello flera instanser uppgift sig själv, vilket *endast hello primära* uppgift hämtningar.</span><span class="sxs-lookup"><span data-stu-id="a347b-184">There are two sets of resource files tooconsider for multi-instance tasks: **common resource files** that *all* tasks download (both primary and subtasks), and hello **resource files** specified for hello multi-instance task itself, which *only hello primary* task downloads.</span></span>

<span data-ttu-id="a347b-185">Du kan ange en eller flera **vanliga resursfiler** i inställningarna för hello flera instanser för en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a347b-185">You can specify one or more **common resource files** in hello multi-instance settings for a task.</span></span> <span data-ttu-id="a347b-186">Dessa vanliga resursfiler hämtas från [Azure Storage](../storage/common/storage-introduction.md) på varje nod **aktivitet delade katalogen** genom hello primära och alla underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a347b-186">These common resource files are downloaded from [Azure Storage](../storage/common/storage-introduction.md) into each node's **task shared directory** by hello primary and all subtasks.</span></span> <span data-ttu-id="a347b-187">Du kan komma åt hello uppgiften delad katalog från program och samordning kommandorader med hello `AZ_BATCH_TASK_SHARED_DIR` miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="a347b-187">You can access hello task shared directory from application and coordination command lines by using hello `AZ_BATCH_TASK_SHARED_DIR` environment variable.</span></span> <span data-ttu-id="a347b-188">Hej `AZ_BATCH_TASK_SHARED_DIR` sökvägen är identiska på varje nod allokerade toohello flera instanser aktivitet, vilket du kan dela ett enda samordning kommando mellan hello primära och alla underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a347b-188">hello `AZ_BATCH_TASK_SHARED_DIR` path is identical on every node allocated toohello multi-instance task, thus you can share a single coordination command between hello primary and all subtasks.</span></span> <span data-ttu-id="a347b-189">Batch ”delar inte” hello katalogen i en mening för fjärråtkomst, men du kan använda den som en monteringspunkt eller dela punkt som nämnts tidigare i hello tips på miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="a347b-189">Batch does not "share" hello directory in a remote access sense, but you can use it as a mount or share point as mentioned earlier in hello tip on environment variables.</span></span>

<span data-ttu-id="a347b-190">Resursfiler som du anger för hello själva flera instanser aktiviteten är hämtade toohello aktivitet arbetskatalogen `AZ_BATCH_TASK_WORKING_DIR`, som standard.</span><span class="sxs-lookup"><span data-stu-id="a347b-190">Resource files that you specify for hello multi-instance task itself are downloaded toohello task's working directory, `AZ_BATCH_TASK_WORKING_DIR`, by default.</span></span> <span data-ttu-id="a347b-191">Som tidigare nämnts kan däremot toocommon resursfiler endast hello primära aktiviteten hämtar resursfiler som angetts för hello flera instanser själva aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="a347b-191">As mentioned, in contrast toocommon resource files, only hello primary task downloads resource files specified for hello  multi-instance task itself.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a347b-192">Använd alltid hello miljövariabler `AZ_BATCH_TASK_SHARED_DIR` och `AZ_BATCH_TASK_WORKING_DIR` toorefer toothese kataloger i din kommandorader.</span><span class="sxs-lookup"><span data-stu-id="a347b-192">Always use hello environment variables `AZ_BATCH_TASK_SHARED_DIR` and `AZ_BATCH_TASK_WORKING_DIR` toorefer toothese directories in your command lines.</span></span> <span data-ttu-id="a347b-193">Försök inte tooconstruct hello sökvägar manuellt.</span><span class="sxs-lookup"><span data-stu-id="a347b-193">Do not attempt tooconstruct hello paths manually.</span></span>
>
>

## <a name="task-lifetime"></a><span data-ttu-id="a347b-194">Livslängd för aktiviteten</span><span class="sxs-lookup"><span data-stu-id="a347b-194">Task lifetime</span></span>
<span data-ttu-id="a347b-195">hello livstid hello främsta uppgiften kontroller hello livstid hello hela flera instanser aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a347b-195">hello lifetime of hello primary task controls hello lifetime of hello entire multi-instance task.</span></span> <span data-ttu-id="a347b-196">När primära hello avslutas avslutas alla hello underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a347b-196">When hello primary exits, all of hello subtasks are terminated.</span></span> <span data-ttu-id="a347b-197">hello avslutningskoden hello primära är hello avslutningskoden hello aktivitet och är därför används toodetermine hello lyckad eller misslyckad hello aktivitet för försök igen.</span><span class="sxs-lookup"><span data-stu-id="a347b-197">hello exit code of hello primary is hello exit code of hello task, and is therefore used toodetermine hello success or failure of hello task for retry purposes.</span></span>

<span data-ttu-id="a347b-198">Om någon av hello underaktiviteter misslyckas misslyckas avslutades med koden inte är noll, till exempel hello hela flera instanser.</span><span class="sxs-lookup"><span data-stu-id="a347b-198">If any of hello subtasks fail, exiting with a non-zero return code, for example, hello entire multi-instance task fails.</span></span> <span data-ttu-id="a347b-199">hello flera instansuppgift är sedan avslutas och igen in tooits gränsen för återförsök.</span><span class="sxs-lookup"><span data-stu-id="a347b-199">hello multi-instance task is then terminated and retried, up tooits retry limit.</span></span>

<span data-ttu-id="a347b-200">När du tar bort en aktivitet med flera instanser bort hello primära och alla underaktiviteter också av hello Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a347b-200">When you delete a multi-instance task, hello primary and all subtasks are also deleted by hello Batch service.</span></span> <span data-ttu-id="a347b-201">Alla underaktivitet kataloger och filer tas bort från hello compute-noder, precis som för en aktivitet som standard.</span><span class="sxs-lookup"><span data-stu-id="a347b-201">All subtask directories and their files are deleted from hello compute nodes, just as for a standard task.</span></span>

<span data-ttu-id="a347b-202">[TaskConstraints] [ net_taskconstraints] för en aktivitet med flera instanser, till exempel hello [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime] [ net_taskconstraint_maxwallclock], och [RetentionTime] [ net_taskconstraint_retention] gäller egenskaper, som de är för en aktivitet som standard och tillämpa toohello primära servern och alla underaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a347b-202">[TaskConstraints][net_taskconstraints] for a multi-instance task, such as hello [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock], and [RetentionTime][net_taskconstraint_retention] properties, are honored as they are for a standard task, and apply toohello primary and all subtasks.</span></span> <span data-ttu-id="a347b-203">Men om du ändrar hello [RetentionTime] [ net_taskconstraint_retention] egenskapen när du lägger till hello flera instanser toohello aktiviteten, den här ändringen är tillämpade endast toohello främsta uppgiften.</span><span class="sxs-lookup"><span data-stu-id="a347b-203">However, if you change hello [RetentionTime][net_taskconstraint_retention] property after adding hello multi-instance task toohello job, this change is applied only toohello primary task.</span></span> <span data-ttu-id="a347b-204">Alla hello underaktiviteter fortsätta toouse hello ursprungliga [RetentionTime][net_taskconstraint_retention].</span><span class="sxs-lookup"><span data-stu-id="a347b-204">All of hello subtasks continue toouse hello original [RetentionTime][net_taskconstraint_retention].</span></span>

<span data-ttu-id="a347b-205">En beräkningsnod senaste uppgiftslista visar hello-id för en delaktivitet om hello aktivitet var en del av en aktivitet med flera instanser.</span><span class="sxs-lookup"><span data-stu-id="a347b-205">A compute node's recent task list reflects hello id of a subtask if hello recent task was part of a multi-instance task.</span></span>

## <a name="obtain-information-about-subtasks"></a><span data-ttu-id="a347b-206">Hämta information om underaktiviteter</span><span class="sxs-lookup"><span data-stu-id="a347b-206">Obtain information about subtasks</span></span>
<span data-ttu-id="a347b-207">tooobtain information om underaktiviteter genom att använda hello Batch .NET-bibliotek, anrop hello [CloudTask.ListSubtasks] [ net_task_listsubtasks] metod.</span><span class="sxs-lookup"><span data-stu-id="a347b-207">tooobtain information on subtasks by using hello Batch .NET library, call hello [CloudTask.ListSubtasks][net_task_listsubtasks] method.</span></span> <span data-ttu-id="a347b-208">Den här metoden returnerar information om alla underaktiviteter och information om hello beräkningsnod som körts hello uppgifter.</span><span class="sxs-lookup"><span data-stu-id="a347b-208">This method returns information on all subtasks, and information about hello compute node that executed hello tasks.</span></span> <span data-ttu-id="a347b-209">Från den här informationen kan bestämma du rotkatalogen för varje underaktivitet, hello programpools-id, det aktuella tillståndet, slutkod med mera.</span><span class="sxs-lookup"><span data-stu-id="a347b-209">From this information, you can determine each subtask's root directory, hello pool id, its current state, exit code, and more.</span></span> <span data-ttu-id="a347b-210">Du kan använda den här informationen i kombination med hello [PoolOperations.GetNodeFile] [ poolops_getnodefile] metoden tooobtain hello underaktivitets filer.</span><span class="sxs-lookup"><span data-stu-id="a347b-210">You can use this information in combination with hello [PoolOperations.GetNodeFile][poolops_getnodefile] method tooobtain hello subtask's files.</span></span> <span data-ttu-id="a347b-211">Observera att den här metoden inte returnera information om hello primära aktivitet (id 0).</span><span class="sxs-lookup"><span data-stu-id="a347b-211">Note that this method does not return information for hello primary task (id 0).</span></span>

> [!NOTE]
> <span data-ttu-id="a347b-212">Om inte annat anges hello Batch .NET-metoder som fungerar på flera instanser [CloudTask] [ net_task] själva gäller *endast* toohello främsta uppgiften.</span><span class="sxs-lookup"><span data-stu-id="a347b-212">Unless otherwise stated, Batch .NET methods that operate on hello multi-instance [CloudTask][net_task] itself apply *only* toohello primary task.</span></span> <span data-ttu-id="a347b-213">Till exempel när du anropar hello [CloudTask.ListNodeFiles] [ net_task_listnodefiles] metoden för en uppgift med flera instanser returneras endast hello främsta uppgiften filer.</span><span class="sxs-lookup"><span data-stu-id="a347b-213">For example, when you call hello [CloudTask.ListNodeFiles][net_task_listnodefiles] method on a multi-instance task, only hello primary task's files are returned.</span></span>
>
>

<span data-ttu-id="a347b-214">hello följande kodavsnitt visar hur tooobtain underaktivitet information, samt begära filinnehållet från hello-noder som de körs.</span><span class="sxs-lookup"><span data-stu-id="a347b-214">hello following code snippet shows how tooobtain subtask information, as well as request file contents from hello nodes on which they executed.</span></span>

```csharp
// Obtain hello job and hello multi-instance task from hello Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain hello list of subtasks for hello task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over hello subtasks and print their stdout and stderr
// output if hello subtask has completed
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

## <a name="code-sample"></a><span data-ttu-id="a347b-215">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="a347b-215">Code sample</span></span>
<span data-ttu-id="a347b-216">Hej [MultiInstanceTasks] [ github_mpi] kodexempel på GitHub visar hur toouse flera instanser uppgift toorun en [MS-MPI] [ msmpi_msdn] programmet på Batch-beräkningsnoder.</span><span class="sxs-lookup"><span data-stu-id="a347b-216">hello [MultiInstanceTasks][github_mpi] code sample on GitHub demonstrates how toouse a multi-instance task toorun an [MS-MPI][msmpi_msdn] application on Batch compute nodes.</span></span> <span data-ttu-id="a347b-217">Gör så hello i [förberedelse](#preparation) och [körning](#execution) toorun hello exempel.</span><span class="sxs-lookup"><span data-stu-id="a347b-217">Follow hello steps in [Preparation](#preparation) and [Execution](#execution) toorun hello sample.</span></span>

### <a name="preparation"></a><span data-ttu-id="a347b-218">Förberedelse</span><span class="sxs-lookup"><span data-stu-id="a347b-218">Preparation</span></span>
1. <span data-ttu-id="a347b-219">Följ hello två första stegen i [hur toocompile och köra ett enkelt program MS-MPI][msmpi_howto].</span><span class="sxs-lookup"><span data-stu-id="a347b-219">Follow hello first two steps in [How toocompile and run a simple MS-MPI program][msmpi_howto].</span></span> <span data-ttu-id="a347b-220">Det här uppfyller hello prerequesites för hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="a347b-220">This satisfies hello prerequesites for hello following step.</span></span>
2. <span data-ttu-id="a347b-221">Skapa en *versionen* version av hello [MPIHelloWorld] [ helloworld_proj] MPI exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a347b-221">Build a *Release* version of hello [MPIHelloWorld][helloworld_proj] sample MPI program.</span></span> <span data-ttu-id="a347b-222">Detta är hello-program som körs på datornoderna av hello flera instanser aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a347b-222">This is hello program that will be run on compute nodes by hello multi-instance task.</span></span>
3. <span data-ttu-id="a347b-223">Skapa en zip-filen med `MPIHelloWorld.exe` (som du skapat i steg 2) och `MSMpiSetup.exe` (som du hämtade i steg 1).</span><span class="sxs-lookup"><span data-stu-id="a347b-223">Create a zip file containing `MPIHelloWorld.exe` (which you built step 2) and `MSMpiSetup.exe` (which you downloaded step 1).</span></span> <span data-ttu-id="a347b-224">Du måste ladda upp zip-fil som ett programpaket i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="a347b-224">You'll upload this zip file as an application package in hello next step.</span></span>
4. <span data-ttu-id="a347b-225">Använd hello [Azure-portalen] [ portal] toocreate en Batch [programmet](batch-application-packages.md) kallas ”MPIHelloWorld” och ange hello zip-filen som du skapade i föregående steg i hello versionsnummer ”1.0” hello programpaket.</span><span class="sxs-lookup"><span data-stu-id="a347b-225">Use hello [Azure portal][portal] toocreate a Batch [application](batch-application-packages.md) called "MPIHelloWorld", and specify hello zip file you created in hello previous step as version "1.0" of hello application package.</span></span> <span data-ttu-id="a347b-226">Se [ladda upp och hantera program](batch-application-packages.md#upload-and-manage-applications) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a347b-226">See [Upload and manage applications](batch-application-packages.md#upload-and-manage-applications) for more information.</span></span>

> [!TIP]
> <span data-ttu-id="a347b-227">Skapa en *versionen* version av `MPIHelloWorld.exe` så att du inte har tooinclude eventuella ytterligare beroenden (till exempel `msvcp140d.dll` eller `vcruntime140d.dll`) i programpaketet.</span><span class="sxs-lookup"><span data-stu-id="a347b-227">Build a *Release* version of `MPIHelloWorld.exe` so that you don't have tooinclude any additional dependencies (for example, `msvcp140d.dll` or `vcruntime140d.dll`) in your application package.</span></span>
>
>

### <a name="execution"></a><span data-ttu-id="a347b-228">Körning</span><span class="sxs-lookup"><span data-stu-id="a347b-228">Execution</span></span>
1. <span data-ttu-id="a347b-229">Hämta hello [azure-batch-samples] [ github_samples_zip] från GitHub.</span><span class="sxs-lookup"><span data-stu-id="a347b-229">Download hello [azure-batch-samples][github_samples_zip] from GitHub.</span></span>
2. <span data-ttu-id="a347b-230">Öppna hello MultiInstanceTasks **lösning** i Visual Studio 2015 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a347b-230">Open hello MultiInstanceTasks **solution** in Visual Studio 2015 or newer.</span></span> <span data-ttu-id="a347b-231">Hej `MultiInstanceTasks.sln` lösningsfilen finns:</span><span class="sxs-lookup"><span data-stu-id="a347b-231">hello `MultiInstanceTasks.sln` solution file is located in:</span></span>

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
3. <span data-ttu-id="a347b-232">Ange Batch- och autentiseringsuppgifterna för ditt konto i `AccountSettings.settings` i hello **Microsoft.Azure.Batch.Samples.Common** projekt.</span><span class="sxs-lookup"><span data-stu-id="a347b-232">Enter your Batch and Storage account credentials in `AccountSettings.settings` in hello **Microsoft.Azure.Batch.Samples.Common** project.</span></span>
4. <span data-ttu-id="a347b-233">**Skapa och köra** hello MultiInstanceTasks lösning tooexecute hello MPI exempelprogrammet på compute-noder i en Batch-pool.</span><span class="sxs-lookup"><span data-stu-id="a347b-233">**Build and run** hello MultiInstanceTasks solution tooexecute hello MPI sample application on compute nodes in a Batch pool.</span></span>
5. <span data-ttu-id="a347b-234">*Valfria*: Använd hello [Azure-portalen] [ portal] eller hello [Batch Explorer] [ batch_explorer] tooexamine hello exempel pool, jobb, och uppgiften (”MultiInstanceSamplePool”, ”MultiInstanceSampleJob”, ”MultiInstanceSampleTask”) innan du tar bort hello resurser.</span><span class="sxs-lookup"><span data-stu-id="a347b-234">*Optional*: Use hello [Azure portal][portal] or hello [Batch Explorer][batch_explorer] tooexamine hello sample pool, job, and task ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") before you delete hello resources.</span></span>

> [!TIP]
> <span data-ttu-id="a347b-235">Du kan hämta [Visual Studio Community] [ visual_studio] kostnadsfritt om du inte har Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a347b-235">You can download [Visual Studio Community][visual_studio] for free if you do not have Visual Studio.</span></span>
>
>

<span data-ttu-id="a347b-236">Utdata från `MultiInstanceTasks.exe` är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="a347b-236">Output from `MultiInstanceTasks.exe` is similar toohello following:</span></span>

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] toojob [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks toocomplete...

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

Sample complete, hit ENTER tooexit...
```

## <a name="next-steps"></a><span data-ttu-id="a347b-237">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a347b-237">Next steps</span></span>
* <span data-ttu-id="a347b-238">hello Microsoft HPC & Azure Batch-teamets blogg beskrivs [MPI stöd för Linux på Azure Batch][blog_mpi_linux], och innehåller information om hur du använder [OpenFOAM] [ openfoam] med Batch.</span><span class="sxs-lookup"><span data-stu-id="a347b-238">hello Microsoft HPC & Azure Batch Team blog discusses [MPI support for Linux on Azure Batch][blog_mpi_linux], and includes information on using [OpenFOAM][openfoam] with Batch.</span></span> <span data-ttu-id="a347b-239">Du kan hitta Python-kodexempel för hello [OpenFOAM exempel på GitHub][github_mpi].</span><span class="sxs-lookup"><span data-stu-id="a347b-239">You can find Python code samples for hello [OpenFOAM example on GitHub][github_mpi].</span></span>
* <span data-ttu-id="a347b-240">Lär dig hur för[skapa pooler med Linux datornoderna](batch-linux-nodes.md) för användning i Azure Batch MPI-lösningar.</span><span class="sxs-lookup"><span data-stu-id="a347b-240">Learn how too[create pools of Linux compute nodes](batch-linux-nodes.md) for use in your Azure Batch MPI solutions.</span></span>

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
