---
title: "Skapa uppgifter för att förbereda jobb och fullständig jobb på datornoderna - Azure Batch | Microsoft Docs"
description: "Använd jobbet nivå förberedande uppgifter för att minimera överföring av data till Azure Batch-beräkningsnoder och släpp aktiviteter för rensning av noden på jobbet har slutförts."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 63d9d4f1-8521-4bbb-b95a-c4cad73692d3
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6a2525c02ce7bd3969469d2e28a5fccc948f89b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="run-job-preparation-and-job-release-tasks-on-batch-compute-nodes"></a><span data-ttu-id="7074b-103">Kör jobbförberedelse och jobbet versionen uppgifter på Batch-beräkningsnoder</span><span class="sxs-lookup"><span data-stu-id="7074b-103">Run job preparation and job release tasks on Batch compute nodes</span></span>

 <span data-ttu-id="7074b-104">En Azure Batch-jobbet kräver ofta någon form av installationsprogrammet innan dess aktiviteter utförs och jobbet efter Underhåll när dess aktiviteter är slutförda.</span><span class="sxs-lookup"><span data-stu-id="7074b-104">An Azure Batch job often requires some form of setup before its tasks are executed, and post-job maintenance when its tasks are completed.</span></span> <span data-ttu-id="7074b-105">Du kan behöva hämta vanliga uppgiften indata till compute-noder eller överföra utdata för aktiviteten till Azure Storage när jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7074b-105">You might need to download common task input data to your compute nodes, or upload task output data to Azure Storage after the job completes.</span></span> <span data-ttu-id="7074b-106">Du kan använda **jobbet förberedelse** och **jobbet versionen** åtgärder att utföra dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7074b-106">You can use **job preparation** and **job release** tasks to perform these operations.</span></span>

## <a name="what-are-job-preparation-and-release-tasks"></a><span data-ttu-id="7074b-107">Vad är jobbförberedelseuppgiften och viktiga uppgifter?</span><span class="sxs-lookup"><span data-stu-id="7074b-107">What are job preparation and release tasks?</span></span>
<span data-ttu-id="7074b-108">Innan du kör ett jobb uppgifter jobbförberedelseuppgift jobb som körs på alla compute-noder som är schemalagda att köras minst en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7074b-108">Before a job's tasks run, the job preparation task runs on all compute nodes scheduled to run at least one task.</span></span> <span data-ttu-id="7074b-109">När jobbet är slutfört, körs jobbet versionen aktiviteten på varje nod i den pool som körs minst en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7074b-109">Once the job is completed, the job release task runs on each node in the pool that executed at least one task.</span></span> <span data-ttu-id="7074b-110">Du kan ange en kommandorad som ska anropas när en förberedelse eller viktig aktivitet körs precis som med normal batchaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="7074b-110">As with normal Batch tasks, you can specify a command line to be invoked when a job preparation or release task is run.</span></span>

<span data-ttu-id="7074b-111">Jobbet förberedelse och version aktiviteter finns bekant Batch aktivitet funktioner, till exempel Filhämtning ([resursfiler][net_job_prep_resourcefiles]), utökade körning, anpassade miljövariabler, varaktighet för maximal körning av, försök igen antal och kvarhållningstiden för filen.</span><span class="sxs-lookup"><span data-stu-id="7074b-111">Job preparation and release tasks offer familiar Batch task features such as file download ([resource files][net_job_prep_resourcefiles]), elevated execution, custom environment variables, maximum execution duration, retry count, and file retention time.</span></span>

<span data-ttu-id="7074b-112">I följande avsnitt du lär dig hur du använder den [JobPreparationTask] [ net_job_prep] och [JobReleaseTask] [ net_job_release] klasser hittades i [ Batch-.NET] [ api_net] bibliotek.</span><span class="sxs-lookup"><span data-stu-id="7074b-112">In the following sections, you'll learn how to use the [JobPreparationTask][net_job_prep] and [JobReleaseTask][net_job_release] classes found in the [Batch .NET][api_net] library.</span></span>

> [!TIP]
> <span data-ttu-id="7074b-113">Jobbet förberedelse och version aktiviteter är särskilt användbart i miljöer med ”delat poolen”, som en pool av datornoderna kvarstår mellan jobbkörningar och används av många jobb.</span><span class="sxs-lookup"><span data-stu-id="7074b-113">Job preparation and release tasks are especially helpful in "shared pool" environments, in which a pool of compute nodes persists between job runs and is used by many jobs.</span></span>
> 
> 

## <a name="when-to-use-job-preparation-and-release-tasks"></a><span data-ttu-id="7074b-114">När du ska använda jobbförberedelseuppgiften och släpp aktiviteter</span><span class="sxs-lookup"><span data-stu-id="7074b-114">When to use job preparation and release tasks</span></span>
<span data-ttu-id="7074b-115">Jobbförberedelseuppgiften och versionen jobbuppgifter är passar bra för följande situationer:</span><span class="sxs-lookup"><span data-stu-id="7074b-115">Job preparation and job release tasks are a good fit for the following situations:</span></span>

<span data-ttu-id="7074b-116">**Hämta vanliga aktivitetsdata**</span><span class="sxs-lookup"><span data-stu-id="7074b-116">**Download common task data**</span></span>

<span data-ttu-id="7074b-117">Batchjobb kräver ofta en gemensam uppsättning data som indata för jobbets uppgifter.</span><span class="sxs-lookup"><span data-stu-id="7074b-117">Batch jobs often require a common set of data as input for the job's tasks.</span></span> <span data-ttu-id="7074b-118">Till exempel i dagliga risk analys beräkningar är marknaden data projektspecifika ännu gemensamma för alla uppgifter i jobbet.</span><span class="sxs-lookup"><span data-stu-id="7074b-118">For example, in daily risk analysis calculations, market data is job-specific, yet common to all tasks in the job.</span></span> <span data-ttu-id="7074b-119">Informationen marknaden ofta flera gigabyte i storlek, ska hämtas till varje compute-nod bara en gång så att alla aktiviteter som körs på noden kan använda den.</span><span class="sxs-lookup"><span data-stu-id="7074b-119">This market data, often several gigabytes in size, should be downloaded to each compute node only once so that any task that runs on the node can use it.</span></span> <span data-ttu-id="7074b-120">Använd en **förberedelse projektaktivitet** att hämta dessa data till varje nod innan körningen av jobbet har andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="7074b-120">Use a **job preparation task** to download this data to each node before the execution of the job's other tasks.</span></span>

<span data-ttu-id="7074b-121">**Ta bort jobb- och utdata**</span><span class="sxs-lookup"><span data-stu-id="7074b-121">**Delete job and task output**</span></span>

<span data-ttu-id="7074b-122">I en ”delad pool” miljö, där en pool compute-noder inte är inaktiverade mellan jobb, kan du behöva ta bort jobbdata mellan körs.</span><span class="sxs-lookup"><span data-stu-id="7074b-122">In a "shared pool" environment, where a pool's compute nodes are not decommissioned between jobs, you may need to delete job data between runs.</span></span> <span data-ttu-id="7074b-123">Du kan behöva spara diskutrymme på noderna eller uppfyller organisationens säkerhetsprinciper.</span><span class="sxs-lookup"><span data-stu-id="7074b-123">You might need to conserve disk space on the nodes, or satisfy your organization's security policies.</span></span> <span data-ttu-id="7074b-124">Använd en **versionen projektaktivitet** att ta bort data som hämtas av en projektaktivitet förberedelse, eller genereras under körning av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="7074b-124">Use a **job release task** to delete data that was downloaded by a job preparation task, or generated during task execution.</span></span>

<span data-ttu-id="7074b-125">**Kvarhållning av logg**</span><span class="sxs-lookup"><span data-stu-id="7074b-125">**Log retention**</span></span>

<span data-ttu-id="7074b-126">Du kanske vill behålla en kopia av loggfiler som dina aktiviteter genererar eller kanske kraschdumpfiler som genereras av misslyckade program.</span><span class="sxs-lookup"><span data-stu-id="7074b-126">You might want to keep a copy of log files that your tasks generate, or perhaps crash dump files that can be generated by failed applications.</span></span> <span data-ttu-id="7074b-127">Använd en **versionen projektaktivitet** i sådana fall att komprimera och överföra data till en [Azure Storage] [ azure_storage] konto.</span><span class="sxs-lookup"><span data-stu-id="7074b-127">Use a **job release task** in such cases to compress and upload this data to an [Azure Storage][azure_storage] account.</span></span>

> [!TIP]
> <span data-ttu-id="7074b-128">Ett annat sätt att bevara loggar och andra jobb- och utdata data är att använda den [Azure Batch filen konventioner](batch-task-output.md) bibliotek.</span><span class="sxs-lookup"><span data-stu-id="7074b-128">Another way to persist logs and other job and task output data is to use the [Azure Batch File Conventions](batch-task-output.md) library.</span></span>
> 
> 

## <a name="job-preparation-task"></a><span data-ttu-id="7074b-129">Förberedelse för projektaktivitet</span><span class="sxs-lookup"><span data-stu-id="7074b-129">Job preparation task</span></span>
<span data-ttu-id="7074b-130">Innan körningen av ett jobb uppgifter utför Batch förberedelse projektaktivitet på varje beräkningsnod som kommer att köra en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7074b-130">Before execution of a job's tasks, Batch executes the job preparation task on each compute node that is scheduled to run a task.</span></span> <span data-ttu-id="7074b-131">Som standard väntar Batch-tjänsten för förberedelse projektaktivitet ska slutföras innan du kör uppgifter som är schemalagda att köras på noden.</span><span class="sxs-lookup"><span data-stu-id="7074b-131">By default, the Batch service waits for the job preparation task to be completed before running the tasks scheduled to execute on the node.</span></span> <span data-ttu-id="7074b-132">Du kan dock konfigurera tjänsten som inte ska vänta.</span><span class="sxs-lookup"><span data-stu-id="7074b-132">However, you can configure the service not to wait.</span></span> <span data-ttu-id="7074b-133">Om noden har startats om förberedelse projektaktivitet körs igen, men du kan också inaktivera det här beteendet.</span><span class="sxs-lookup"><span data-stu-id="7074b-133">If the node restarts, the job preparation task runs again, but you can also disable this behavior.</span></span>

<span data-ttu-id="7074b-134">Förberedelse av projektaktivitet körs bara på noder som är schemalagda att köra en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7074b-134">The job preparation task is executed only on nodes that are scheduled to run a task.</span></span> <span data-ttu-id="7074b-135">Detta förhindrar att onödiga körningen av en jobbförberedelseuppgift om en nod inte har tilldelats en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7074b-135">This prevents the unnecessary execution of a preparation task in case a node is not assigned a task.</span></span> <span data-ttu-id="7074b-136">Detta kan inträffa när antalet aktiviteter för ett jobb är mindre än antalet noder i en pool.</span><span class="sxs-lookup"><span data-stu-id="7074b-136">This can occur when the number of tasks for a job is less than the number of nodes in a pool.</span></span> <span data-ttu-id="7074b-137">Det gäller även när [samtidiga uppgiftskörningen](batch-parallel-node-tasks.md) är aktiverad, vilket lämnar vissa noder inaktiv om antal uppgifter är lägre än totalt antal möjliga samtidiga uppgifter.</span><span class="sxs-lookup"><span data-stu-id="7074b-137">It also applies when [concurrent task execution](batch-parallel-node-tasks.md) is enabled, which leaves some nodes idle if the task count is lower than the total possible concurrent tasks.</span></span> <span data-ttu-id="7074b-138">Genom att inte köra förberedelse projektaktivitet på inaktiv noder kan lägga du mindre pengar på kostnader för överföring av data.</span><span class="sxs-lookup"><span data-stu-id="7074b-138">By not running the job preparation task on idle nodes, you can spend less money on data transfer charges.</span></span>

> [!NOTE]
> <span data-ttu-id="7074b-139">[JobPreparationTask] [ net_job_prep_cloudjob] skiljer sig från [CloudPool.StartTask] [ pool_starttask] i att JobPreparationTask körs i början av varje jobb medan startuppgift har ställts körs bara när en beräkningsnod först ansluter till en pool eller startar om.</span><span class="sxs-lookup"><span data-stu-id="7074b-139">[JobPreparationTask][net_job_prep_cloudjob] differs from [CloudPool.StartTask][pool_starttask] in that JobPreparationTask executes at the start of each job, whereas StartTask executes only when a compute node first joins a pool or restarts.</span></span>
> 
> 

## <a name="job-release-task"></a><span data-ttu-id="7074b-140">Versionen för projektaktivitet</span><span class="sxs-lookup"><span data-stu-id="7074b-140">Job release task</span></span>
<span data-ttu-id="7074b-141">När ett jobb har markerats som slutförd, körs jobbet viktig aktivitet på varje nod i den pool som körs minst en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7074b-141">Once a job is marked as completed, the job release task is executed on each node in the pool that executed at least one task.</span></span> <span data-ttu-id="7074b-142">Du kan markera ett jobb som slutförd genom att utfärda en avsluta begäran.</span><span class="sxs-lookup"><span data-stu-id="7074b-142">You mark a job as completed by issuing a terminate request.</span></span> <span data-ttu-id="7074b-143">Batch-tjänsten sedan anger jobbets status till *avslutar*, avbryts alla aktiva eller köra uppgifter som är kopplad till jobbet och kör jobbet versionen aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="7074b-143">The Batch service then sets the job state to *terminating*, terminates any active or running tasks associated with the job, and runs the job release task.</span></span> <span data-ttu-id="7074b-144">Jobbet flyttas sedan till den *slutförts* tillstånd.</span><span class="sxs-lookup"><span data-stu-id="7074b-144">The job then moves to the *completed* state.</span></span>

> [!NOTE]
> <span data-ttu-id="7074b-145">Borttagning av jobbet körs också viktig projektaktivitet.</span><span class="sxs-lookup"><span data-stu-id="7074b-145">Job deletion also executes the job release task.</span></span> <span data-ttu-id="7074b-146">Men om ett jobb har redan avslutats, körs släpp uppgift inte en gång om jobbet senare tas bort.</span><span class="sxs-lookup"><span data-stu-id="7074b-146">However, if a job has already been terminated, the release task is not run a second time if the job is later deleted.</span></span>
> 
> 

## <a name="job-prep-and-release-tasks-with-batch-net"></a><span data-ttu-id="7074b-147">Jobbet prep och släpp aktiviteter med Batch .NET</span><span class="sxs-lookup"><span data-stu-id="7074b-147">Job prep and release tasks with Batch .NET</span></span>
<span data-ttu-id="7074b-148">Om du vill använda en projektaktivitet förberedelse, tilldela en [JobPreparationTask] [ net_job_prep] objekt till ditt jobb [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] egenskapen.</span><span class="sxs-lookup"><span data-stu-id="7074b-148">To use a job preparation task, assign a [JobPreparationTask][net_job_prep] object to your job's [CloudJob.JobPreparationTask][net_job_prep_cloudjob] property.</span></span> <span data-ttu-id="7074b-149">På liknande sätt kan initiera en [JobReleaseTask] [ net_job_release] och tilldela den till ditt jobb [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] egenskapen anges jobbet Släpp uppgiften.</span><span class="sxs-lookup"><span data-stu-id="7074b-149">Similarly, initialize a [JobReleaseTask][net_job_release] and assign it to your job's [CloudJob.JobReleaseTask][net_job_prep_cloudjob] property to set the job's release task.</span></span>

<span data-ttu-id="7074b-150">I det här kodstycket `myBatchClient` är en instans av [BatchClient][net_batch_client], och `myPool` är en befintlig adresspool i Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="7074b-150">In this code snippet, `myBatchClient` is an instance of [BatchClient][net_batch_client], and `myPool` is an existing pool within the Batch account.</span></span>

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

<span data-ttu-id="7074b-151">Som tidigare nämnts är körs den här uppgiften när ett jobb avslutas eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="7074b-151">As mentioned earlier, the release task is executed when a job is terminated or deleted.</span></span> <span data-ttu-id="7074b-152">Avsluta ett jobb med [JobOperations.TerminateJobAsync][net_job_terminate].</span><span class="sxs-lookup"><span data-stu-id="7074b-152">Terminate a job with [JobOperations.TerminateJobAsync][net_job_terminate].</span></span> <span data-ttu-id="7074b-153">Ta bort ett jobb med [JobOperations.DeleteJobAsync][net_job_delete].</span><span class="sxs-lookup"><span data-stu-id="7074b-153">Delete a job with [JobOperations.DeleteJobAsync][net_job_delete].</span></span> <span data-ttu-id="7074b-154">Du vanligtvis avsluta eller ta bort ett jobb när dess aktiviteter har slutförts eller när en tidsgräns som du har definierat har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="7074b-154">You typically terminate or delete a job when its tasks are completed, or when a timeout that you've defined has been reached.</span></span>

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a><span data-ttu-id="7074b-155">Kodexempel på GitHub</span><span class="sxs-lookup"><span data-stu-id="7074b-155">Code sample on GitHub</span></span>
<span data-ttu-id="7074b-156">Om du vill se jobbförberedelseuppgiften och viktiga uppgifter i praktiken, ta en titt på [JobPrepRelease] [ job_prep_release_sample] exempelprojektet på GitHub.</span><span class="sxs-lookup"><span data-stu-id="7074b-156">To see job preparation and release tasks in action, check out the [JobPrepRelease][job_prep_release_sample] sample project on GitHub.</span></span> <span data-ttu-id="7074b-157">Detta konsolprogram gör följande:</span><span class="sxs-lookup"><span data-stu-id="7074b-157">This console application does the following:</span></span>

1. <span data-ttu-id="7074b-158">Skapar en pool med två noder som ”liten”.</span><span class="sxs-lookup"><span data-stu-id="7074b-158">Creates a pool with two "small" nodes.</span></span>
2. <span data-ttu-id="7074b-159">Skapar ett jobb med jobbförberedelseuppgiften, version och standarduppgifter.</span><span class="sxs-lookup"><span data-stu-id="7074b-159">Creates a job with job preparation, release, and standard tasks.</span></span>
3. <span data-ttu-id="7074b-160">Kör jobbet förberedelse aktiviteten som först skriver nod-ID till en textfil i en nod ”delade” katalogen.</span><span class="sxs-lookup"><span data-stu-id="7074b-160">Runs the job preparation task, which first writes the node ID to a text file in a node's "shared" directory.</span></span>
4. <span data-ttu-id="7074b-161">Kör en aktivitet på varje nod som skriver dess aktivitets-ID till samma textfil.</span><span class="sxs-lookup"><span data-stu-id="7074b-161">Runs a task on each node that writes its task ID to the same text file.</span></span>
5. <span data-ttu-id="7074b-162">När alla aktiviteter har slutförts (eller tidsgränsen uppnås), skriver du innehållet på varje nod textfil till konsolen.</span><span class="sxs-lookup"><span data-stu-id="7074b-162">Once all tasks are completed (or the timeout is reached), prints the contents of each node's text file to the console.</span></span>
6. <span data-ttu-id="7074b-163">När jobbet har slutförts körs projektaktiviteten versionen om du vill ta bort filen från noden.</span><span class="sxs-lookup"><span data-stu-id="7074b-163">When the job is completed, runs the job release task to delete the file from the node.</span></span>
7. <span data-ttu-id="7074b-164">Skriver ut slutkoder jobbet förberedelse och version uppgifter för varje nod som de körs.</span><span class="sxs-lookup"><span data-stu-id="7074b-164">Prints the exit codes of the job preparation and release tasks for each node on which they executed.</span></span>
8. <span data-ttu-id="7074b-165">Pausar körningen för att bekräfta borttagning av jobbet och/eller poolen.</span><span class="sxs-lookup"><span data-stu-id="7074b-165">Pauses execution to allow confirmation of job and/or pool deletion.</span></span>

<span data-ttu-id="7074b-166">Utdata från exempelprogrammet som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="7074b-166">Output from the sample application is similar to the following:</span></span>

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

> [!NOTE]
> <span data-ttu-id="7074b-167">På grund av variabeln skapa och starta när noder i en ny pool (vissa noder är redo för uppgifter före andra), kan du se olika utdata.</span><span class="sxs-lookup"><span data-stu-id="7074b-167">Due to the variable creation and start time of nodes in a new pool (some nodes are ready for tasks before others), you may see different output.</span></span> <span data-ttu-id="7074b-168">Eftersom uppgifterna slutföra snabbt, kan en av noderna i den poolen specifikt köra alla uppgifter i jobbet.</span><span class="sxs-lookup"><span data-stu-id="7074b-168">Specifically, because the tasks complete quickly, one of the pool's nodes may execute all of the job's tasks.</span></span> <span data-ttu-id="7074b-169">Om detta händer ser du att jobbet Förbered dig och versionen uppgifter finns inte för noden som körs inga aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="7074b-169">If this occurs, you will notice that the job prep and release tasks do not exist for the node that executed no tasks.</span></span>
> 
> 

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a><span data-ttu-id="7074b-170">Granska jobbförberedelseuppgiften och versionen uppgifter i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7074b-170">Inspect job preparation and release tasks in the Azure portal</span></span>
<span data-ttu-id="7074b-171">När du kör exempelprogrammet som du kan använda den [Azure-portalen] [ portal] att visa egenskaperna för jobbet och dess uppgifter eller även hämta den delade textfil som ändras av jobbets uppgifter.</span><span class="sxs-lookup"><span data-stu-id="7074b-171">When you run the sample application, you can use the [Azure portal][portal] to view the properties of the job and its tasks, or even download the shared text file that is modified by the job's tasks.</span></span>

<span data-ttu-id="7074b-172">Skärmbilden nedan visar den **förberedelse uppgifter bladet** i Azure portal efter en körning av exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="7074b-172">The screenshot below shows the **Preparation tasks blade** in the Azure portal after a run of the sample application.</span></span> <span data-ttu-id="7074b-173">Navigera till den *JobPrepReleaseSampleJob* egenskaper när aktiviteterna har slutförts (men innan du tar bort dina jobb och poolen) och klicka på **förberedande uppgifter** eller **viktiga uppgifter**visa deras egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7074b-173">Navigate to the *JobPrepReleaseSampleJob* properties after your tasks have completed (but before deleting your job and pool) and click **Preparation tasks** or **Release tasks** to view their properties.</span></span>

![Förberedelse av jobbegenskaper i Azure-portalen][1]

## <a name="next-steps"></a><span data-ttu-id="7074b-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7074b-175">Next steps</span></span>
### <a name="application-packages"></a><span data-ttu-id="7074b-176">Programpaket</span><span class="sxs-lookup"><span data-stu-id="7074b-176">Application packages</span></span>
<span data-ttu-id="7074b-177">Förutom projektaktiviteten förberedelse, du kan också använda den [programpaket](batch-application-packages.md) funktion i Batch för att förbereda compute-noder för körning av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="7074b-177">In addition to the job preparation task, you can also use the [application packages](batch-application-packages.md) feature of Batch to prepare compute nodes for task execution.</span></span> <span data-ttu-id="7074b-178">Den här funktionen är särskilt användbar för att distribuera program som inte kräver kör ett installationsprogram, program som innehåller många (100 +) filer eller program som kräver strikt versionskontroll.</span><span class="sxs-lookup"><span data-stu-id="7074b-178">This feature is especially useful for deploying applications that do not require running an installer, applications that contain many (100+) files, or applications that require strict version control.</span></span>

### <a name="installing-applications-and-staging-data"></a><span data-ttu-id="7074b-179">Installera program och mellanlagring av data</span><span class="sxs-lookup"><span data-stu-id="7074b-179">Installing applications and staging data</span></span>
<span data-ttu-id="7074b-180">Den här MSDN-foruminlägg innehåller en översikt över flera metoder för att förbereda din noder för pågående aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="7074b-180">This MSDN forum post provides an overview of several methods of preparing your nodes for running tasks:</span></span>

<span data-ttu-id="7074b-181">[Installera program och mellanlagring av data på Batch-beräkningsnoder][forum_post]</span><span class="sxs-lookup"><span data-stu-id="7074b-181">[Installing applications and staging data on Batch compute nodes][forum_post]</span></span>

<span data-ttu-id="7074b-182">Skrivs av en Azure Batch-gruppmedlemmar beskrivs den flera metoder som du kan använda för att distribuera program och data för att compute-noder.</span><span class="sxs-lookup"><span data-stu-id="7074b-182">Written by one of the Azure Batch team members, it discusses several techniques that you can use to deploy applications and data to compute nodes.</span></span>

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
