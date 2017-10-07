---
title: "aaaCreate uppgifter tooprepare jobb och fullständig jobb på datornoderna - Azure Batch | Microsoft Docs"
description: "Använd jobbet nivå förberedelse uppgifter toominimize data transfer tooAzure Batch-beräkningsnoder och släpp uppgifter för rensning av noden på jobbet har slutförts."
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
ms.openlocfilehash: fd5fb47ae6700281e63048c49a1241f4e935baba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-job-preparation-and-job-release-tasks-on-batch-compute-nodes"></a><span data-ttu-id="dc3c4-103">Kör jobbförberedelse och jobbet versionen uppgifter på Batch-beräkningsnoder</span><span class="sxs-lookup"><span data-stu-id="dc3c4-103">Run job preparation and job release tasks on Batch compute nodes</span></span>

 <span data-ttu-id="dc3c4-104">En Azure Batch-jobbet kräver ofta någon form av installationsprogrammet innan dess aktiviteter utförs och jobbet efter Underhåll när dess aktiviteter är slutförda.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-104">An Azure Batch job often requires some form of setup before its tasks are executed, and post-job maintenance when its tasks are completed.</span></span> <span data-ttu-id="dc3c4-105">Du kan behöver toodownload vanliga uppgiften indata tooyour compute-noder eller överför uppgiften utgående data tooAzure lagring när hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-105">You might need toodownload common task input data tooyour compute nodes, or upload task output data tooAzure Storage after hello job completes.</span></span> <span data-ttu-id="dc3c4-106">Du kan använda **jobbet förberedelse** och **jobbet versionen** uppgifter tooperform dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-106">You can use **job preparation** and **job release** tasks tooperform these operations.</span></span>

## <a name="what-are-job-preparation-and-release-tasks"></a><span data-ttu-id="dc3c4-107">Vad är jobbförberedelseuppgiften och viktiga uppgifter?</span><span class="sxs-lookup"><span data-stu-id="dc3c4-107">What are job preparation and release tasks?</span></span>
<span data-ttu-id="dc3c4-108">Innan du kör ett jobb uppgifter hello jobbet förberedelse aktiviteten körs på alla compute-noder schemalagda toorun åtminstone en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-108">Before a job's tasks run, hello job preparation task runs on all compute nodes scheduled toorun at least one task.</span></span> <span data-ttu-id="dc3c4-109">När hello jobbet är slutfört, körs hello jobbet versionen aktiviteten på varje nod i hello-pool som körts minst en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-109">Once hello job is completed, hello job release task runs on each node in hello pool that executed at least one task.</span></span> <span data-ttu-id="dc3c4-110">Du kan ange en toobe för kommandoraden som anropas när en jobbförberedelseuppgiften precis som med normal batchaktiviteter eller uppgiften körs.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-110">As with normal Batch tasks, you can specify a command line toobe invoked when a job preparation or release task is run.</span></span>

<span data-ttu-id="dc3c4-111">Jobbet förberedelse och version aktiviteter finns bekant Batch aktivitet funktioner, till exempel Filhämtning ([resursfiler][net_job_prep_resourcefiles]), utökade körning, anpassade miljövariabler, varaktighet för maximal körning av, försök igen antal och kvarhållningstiden för filen.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-111">Job preparation and release tasks offer familiar Batch task features such as file download ([resource files][net_job_prep_resourcefiles]), elevated execution, custom environment variables, maximum execution duration, retry count, and file retention time.</span></span>

<span data-ttu-id="dc3c4-112">I följande avsnitt hello, lär du dig hur toouse hello [JobPreparationTask] [ net_job_prep] och [JobReleaseTask] [ net_job_release] påträffades i hello [Batch .NET] [ api_net] bibliotek.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-112">In hello following sections, you'll learn how toouse hello [JobPreparationTask][net_job_prep] and [JobReleaseTask][net_job_release] classes found in hello [Batch .NET][api_net] library.</span></span>

> [!TIP]
> <span data-ttu-id="dc3c4-113">Jobbet förberedelse och version aktiviteter är särskilt användbart i miljöer med ”delat poolen”, som en pool av datornoderna kvarstår mellan jobbkörningar och används av många jobb.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-113">Job preparation and release tasks are especially helpful in "shared pool" environments, in which a pool of compute nodes persists between job runs and is used by many jobs.</span></span>
> 
> 

## <a name="when-toouse-job-preparation-and-release-tasks"></a><span data-ttu-id="dc3c4-114">När toouse jobbet förberedelse och släpp aktiviteter</span><span class="sxs-lookup"><span data-stu-id="dc3c4-114">When toouse job preparation and release tasks</span></span>
<span data-ttu-id="dc3c4-115">Jobbförberedelseuppgiften och versionen jobbuppgifter är passar bra för hello följande situationer:</span><span class="sxs-lookup"><span data-stu-id="dc3c4-115">Job preparation and job release tasks are a good fit for hello following situations:</span></span>

<span data-ttu-id="dc3c4-116">**Hämta vanliga aktivitetsdata**</span><span class="sxs-lookup"><span data-stu-id="dc3c4-116">**Download common task data**</span></span>

<span data-ttu-id="dc3c4-117">Batchjobb kräver ofta en gemensam uppsättning data som indata för hello jobbets uppgifter.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-117">Batch jobs often require a common set of data as input for hello job's tasks.</span></span> <span data-ttu-id="dc3c4-118">Dagliga risk analys beräkningar är exempelvis marknaden data projektspecifika, men vanliga tooall uppgifter i hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-118">For example, in daily risk analysis calculations, market data is job-specific, yet common tooall tasks in hello job.</span></span> <span data-ttu-id="dc3c4-119">Den här marknaden data, ofta flera gigabyte i storlek, bör vara hämtade tooeach beräkningsnod bara en gång så att alla aktiviteter som körs på hello kan använda den.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-119">This market data, often several gigabytes in size, should be downloaded tooeach compute node only once so that any task that runs on hello node can use it.</span></span> <span data-ttu-id="dc3c4-120">Använd en **förberedelse projektaktivitet** toodownload data tooeach noden innan hello körningen av hello jobbet har andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-120">Use a **job preparation task** toodownload this data tooeach node before hello execution of hello job's other tasks.</span></span>

<span data-ttu-id="dc3c4-121">**Ta bort jobb- och utdata**</span><span class="sxs-lookup"><span data-stu-id="dc3c4-121">**Delete job and task output**</span></span>

<span data-ttu-id="dc3c4-122">I en ”delad pool” miljö, där en pool compute-noder inte är inaktiverade mellan jobb kan behöva du toodelete jobbdata mellan körs.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-122">In a "shared pool" environment, where a pool's compute nodes are not decommissioned between jobs, you may need toodelete job data between runs.</span></span> <span data-ttu-id="dc3c4-123">Du kan behöva tooconserve ledigt diskutrymme på hello noder eller uppfyller organisationens säkerhetsprinciper.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-123">You might need tooconserve disk space on hello nodes, or satisfy your organization's security policies.</span></span> <span data-ttu-id="dc3c4-124">Använd en **versionen projektaktivitet** toodelete data som hämtas av en projektaktivitet förberedelse, eller genererats under körningen av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-124">Use a **job release task** toodelete data that was downloaded by a job preparation task, or generated during task execution.</span></span>

<span data-ttu-id="dc3c4-125">**Kvarhållning av logg**</span><span class="sxs-lookup"><span data-stu-id="dc3c4-125">**Log retention**</span></span>

<span data-ttu-id="dc3c4-126">Du kanske vill tookeep en kopia av loggfiler som dina aktiviteter genererar eller kanske kraschdumpfiler som genereras av misslyckade program.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-126">You might want tookeep a copy of log files that your tasks generate, or perhaps crash dump files that can be generated by failed applications.</span></span> <span data-ttu-id="dc3c4-127">Använd en **versionen projektaktivitet** i sådana fall toocompress och överföra den här informationen tooan [Azure Storage] [ azure_storage] konto.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-127">Use a **job release task** in such cases toocompress and upload this data tooan [Azure Storage][azure_storage] account.</span></span>

> [!TIP]
> <span data-ttu-id="dc3c4-128">Ett annat sätt toopersist loggar och andra jobb- och utdata data är toouse hello [Azure Batch filen konventioner](batch-task-output.md) bibliotek.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-128">Another way toopersist logs and other job and task output data is toouse hello [Azure Batch File Conventions](batch-task-output.md) library.</span></span>
> 
> 

## <a name="job-preparation-task"></a><span data-ttu-id="dc3c4-129">Förberedelse för projektaktivitet</span><span class="sxs-lookup"><span data-stu-id="dc3c4-129">Job preparation task</span></span>
<span data-ttu-id="dc3c4-130">Innan körningen av ett jobb uppgifter utför Batch hello projektaktivitet förberedelse på varje beräkningsnod som är schemalagda toorun en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-130">Before execution of a job's tasks, Batch executes hello job preparation task on each compute node that is scheduled toorun a task.</span></span> <span data-ttu-id="dc3c4-131">Som standard väntar hello Batch-tjänsten för hello jobbet förberedelse uppgiften toobe slutförts innan du kör hello aktiviteter schemalagda tooexecute på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-131">By default, hello Batch service waits for hello job preparation task toobe completed before running hello tasks scheduled tooexecute on hello node.</span></span> <span data-ttu-id="dc3c4-132">Du kan dock konfigurera hello-tjänsten inte toowait.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-132">However, you can configure hello service not toowait.</span></span> <span data-ttu-id="dc3c4-133">Om hello nod har startats om hello jobbet uppgiften körs igen, men du kan också inaktivera det här beteendet.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-133">If hello node restarts, hello job preparation task runs again, but you can also disable this behavior.</span></span>

<span data-ttu-id="dc3c4-134">hello jobbet uppgiften körs bara på noder som är schemalagda toorun en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-134">hello job preparation task is executed only on nodes that are scheduled toorun a task.</span></span> <span data-ttu-id="dc3c4-135">Detta förhindrar hello onödiga körningen av en jobbförberedelseuppgift om en nod inte har tilldelats en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-135">This prevents hello unnecessary execution of a preparation task in case a node is not assigned a task.</span></span> <span data-ttu-id="dc3c4-136">Detta kan inträffa när hello antal uppgifter för ett jobb är mindre än hello antalet noder i en pool.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-136">This can occur when hello number of tasks for a job is less than hello number of nodes in a pool.</span></span> <span data-ttu-id="dc3c4-137">Det gäller även när [samtidiga uppgiftskörningen](batch-parallel-node-tasks.md) är aktiverat, vilket lämnar vissa noder inaktiv om hello uppgiften antalet är lägre än hello Totalt antal möjliga samtidiga uppgifter.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-137">It also applies when [concurrent task execution](batch-parallel-node-tasks.md) is enabled, which leaves some nodes idle if hello task count is lower than hello total possible concurrent tasks.</span></span> <span data-ttu-id="dc3c4-138">Genom att köra inte hello projektaktivitet förberedelse för inaktiv noder kan lägga du mindre pengar på kostnader för överföring av data.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-138">By not running hello job preparation task on idle nodes, you can spend less money on data transfer charges.</span></span>

> [!NOTE]
> <span data-ttu-id="dc3c4-139">[JobPreparationTask] [ net_job_prep_cloudjob] skiljer sig från [CloudPool.StartTask] [ pool_starttask] i att JobPreparationTask kör hello början av varje jobb medan startuppgift har ställts körs bara när en beräkningsnod först ansluter till en pool eller startar om.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-139">[JobPreparationTask][net_job_prep_cloudjob] differs from [CloudPool.StartTask][pool_starttask] in that JobPreparationTask executes at hello start of each job, whereas StartTask executes only when a compute node first joins a pool or restarts.</span></span>
> 
> 

## <a name="job-release-task"></a><span data-ttu-id="dc3c4-140">Versionen för projektaktivitet</span><span class="sxs-lookup"><span data-stu-id="dc3c4-140">Job release task</span></span>
<span data-ttu-id="dc3c4-141">När ett jobb har markerats som slutförd, körs hello jobbet versionen uppgiften på varje nod i hello-pool som körts minst en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-141">Once a job is marked as completed, hello job release task is executed on each node in hello pool that executed at least one task.</span></span> <span data-ttu-id="dc3c4-142">Du kan markera ett jobb som slutförd genom att utfärda en avsluta begäran.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-142">You mark a job as completed by issuing a terminate request.</span></span> <span data-ttu-id="dc3c4-143">Hej Batch-tjänsten och sedan anger hello jobbets status för*avslutar*, avbryts alla aktiva eller köra uppgifter som är kopplad till hello jobbet och kör hello projektaktivitet versionen.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-143">hello Batch service then sets hello job state too*terminating*, terminates any active or running tasks associated with hello job, and runs hello job release task.</span></span> <span data-ttu-id="dc3c4-144">hello jobbet flyttar toohello *slutförts* tillstånd.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-144">hello job then moves toohello *completed* state.</span></span>

> [!NOTE]
> <span data-ttu-id="dc3c4-145">Borttagning av jobbet körs även hello projektaktivitet versionen.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-145">Job deletion also executes hello job release task.</span></span> <span data-ttu-id="dc3c4-146">Men om ett jobb har redan avslutats, körs hello versionen inte aktiviteten en gång om jobbet hello senare tas bort.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-146">However, if a job has already been terminated, hello release task is not run a second time if hello job is later deleted.</span></span>
> 
> 

## <a name="job-prep-and-release-tasks-with-batch-net"></a><span data-ttu-id="dc3c4-147">Jobbet prep och släpp aktiviteter med Batch .NET</span><span class="sxs-lookup"><span data-stu-id="dc3c4-147">Job prep and release tasks with Batch .NET</span></span>
<span data-ttu-id="dc3c4-148">toouse en projektaktivitet förberedelse, tilldela en [JobPreparationTask] [ net_job_prep] objekt tooyour jobbet [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] egenskapen .</span><span class="sxs-lookup"><span data-stu-id="dc3c4-148">toouse a job preparation task, assign a [JobPreparationTask][net_job_prep] object tooyour job's [CloudJob.JobPreparationTask][net_job_prep_cloudjob] property.</span></span> <span data-ttu-id="dc3c4-149">På liknande sätt kan initiera en [JobReleaseTask] [ net_job_release] och tilldela den tooyour jobbet [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] egenskapen tooset hello Jobbets uppgiften.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-149">Similarly, initialize a [JobReleaseTask][net_job_release] and assign it tooyour job's [CloudJob.JobReleaseTask][net_job_prep_cloudjob] property tooset hello job's release task.</span></span>

<span data-ttu-id="dc3c4-150">I det här kodstycket `myBatchClient` är en instans av [BatchClient][net_batch_client], och `myPool` är en befintlig adresspool inom hello Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-150">In this code snippet, `myBatchClient` is an instance of [BatchClient][net_batch_client], and `myPool` is an existing pool within hello Batch account.</span></span>

```csharp
// Create hello CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify hello command lines for hello job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign hello job preparation task toohello job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign hello job release task toohello job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

<span data-ttu-id="dc3c4-151">Som tidigare nämnts körs hello versionen uppgiften när ett jobb avslutas eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-151">As mentioned earlier, hello release task is executed when a job is terminated or deleted.</span></span> <span data-ttu-id="dc3c4-152">Avsluta ett jobb med [JobOperations.TerminateJobAsync][net_job_terminate].</span><span class="sxs-lookup"><span data-stu-id="dc3c4-152">Terminate a job with [JobOperations.TerminateJobAsync][net_job_terminate].</span></span> <span data-ttu-id="dc3c4-153">Ta bort ett jobb med [JobOperations.DeleteJobAsync][net_job_delete].</span><span class="sxs-lookup"><span data-stu-id="dc3c4-153">Delete a job with [JobOperations.DeleteJobAsync][net_job_delete].</span></span> <span data-ttu-id="dc3c4-154">Du vanligtvis avsluta eller ta bort ett jobb när dess aktiviteter har slutförts eller när en tidsgräns som du har definierat har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-154">You typically terminate or delete a job when its tasks are completed, or when a timeout that you've defined has been reached.</span></span>

```csharp
// Terminate hello job toomark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a><span data-ttu-id="dc3c4-155">Kodexempel på GitHub</span><span class="sxs-lookup"><span data-stu-id="dc3c4-155">Code sample on GitHub</span></span>
<span data-ttu-id="dc3c4-156">toosee förberedelse och version jobbuppgifter i åtgärden, kolla hello [JobPrepRelease] [ job_prep_release_sample] exempelprojektet på GitHub.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-156">toosee job preparation and release tasks in action, check out hello [JobPrepRelease][job_prep_release_sample] sample project on GitHub.</span></span> <span data-ttu-id="dc3c4-157">Detta konsolprogram hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc3c4-157">This console application does hello following:</span></span>

1. <span data-ttu-id="dc3c4-158">Skapar en pool med två noder som ”liten”.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-158">Creates a pool with two "small" nodes.</span></span>
2. <span data-ttu-id="dc3c4-159">Skapar ett jobb med jobbförberedelseuppgiften, version och standarduppgifter.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-159">Creates a job with job preparation, release, and standard tasks.</span></span>
3. <span data-ttu-id="dc3c4-160">Kör hello förberedelse projektaktivitet som först skriver hello nod-ID tooa textfil i en nod ”delade” katalogen.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-160">Runs hello job preparation task, which first writes hello node ID tooa text file in a node's "shared" directory.</span></span>
4. <span data-ttu-id="dc3c4-161">Kör en aktivitet på varje nod som skriver dess aktivitet ID toohello samma textfil.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-161">Runs a task on each node that writes its task ID toohello same text file.</span></span>
5. <span data-ttu-id="dc3c4-162">När alla aktiviteter har slutförts (eller hello tidsgränsen uppnås), skriver du hello innehållet för varje nod text filen toohello konsol.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-162">Once all tasks are completed (or hello timeout is reached), prints hello contents of each node's text file toohello console.</span></span>
6. <span data-ttu-id="dc3c4-163">När hello jobbet har slutförts körs hello viktig uppgift toodelete hello fil från hello-nod.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-163">When hello job is completed, runs hello job release task toodelete hello file from hello node.</span></span>
7. <span data-ttu-id="dc3c4-164">Utskrifter hello slutkoder av hello jobbförberedelseuppgiften och släpp aktiviteter för varje nod som de körs.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-164">Prints hello exit codes of hello job preparation and release tasks for each node on which they executed.</span></span>
8. <span data-ttu-id="dc3c4-165">Pausar körningen tooallow bekräftelse av jobb och/eller pool borttagning.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-165">Pauses execution tooallow confirmation of job and/or pool deletion.</span></span>

<span data-ttu-id="dc3c4-166">Utdata från hello exempelprogrammet är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="dc3c4-166">Output from hello sample application is similar toohello following:</span></span>

```
Attempting toocreate pool: JobPrepReleaseSamplePool
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

Waiting for job JobPrepReleaseSampleJob tooreach state Completed
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

Sample complete, hit ENTER tooexit...
```

> [!NOTE]
> <span data-ttu-id="dc3c4-167">På grund av toohello variabeln skapa och starta tiden för noder i en ny pool (vissa noder är redo för uppgifter före andra), kan du se olika utdata.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-167">Due toohello variable creation and start time of nodes in a new pool (some nodes are ready for tasks before others), you may see different output.</span></span> <span data-ttu-id="dc3c4-168">Eftersom hello slutföras snabbt, kan en av noderna hello poolen specifikt köra alla hello jobbets uppgifter.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-168">Specifically, because hello tasks complete quickly, one of hello pool's nodes may execute all of hello job's tasks.</span></span> <span data-ttu-id="dc3c4-169">Om detta händer ser du att hello jobbet prep och släpp aktiviteter finns inte för hello-nod som körs inga aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-169">If this occurs, you will notice that hello job prep and release tasks do not exist for hello node that executed no tasks.</span></span>
> 
> 

### <a name="inspect-job-preparation-and-release-tasks-in-hello-azure-portal"></a><span data-ttu-id="dc3c4-170">Granska jobbförberedelseuppgiften och versionen uppgifter i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dc3c4-170">Inspect job preparation and release tasks in hello Azure portal</span></span>
<span data-ttu-id="dc3c4-171">När du kör hello exempelprogrammet, kan du använda hello [Azure-portalen] [ portal] tooview hello egenskaper för hello jobb och dess uppgifter eller även hämta hello delade textfil som ändras av hello jobbets uppgifter.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-171">When you run hello sample application, you can use hello [Azure portal][portal] tooview hello properties of hello job and its tasks, or even download hello shared text file that is modified by hello job's tasks.</span></span>

<span data-ttu-id="dc3c4-172">hello skärmbilden nedan visar hello **förberedelse uppgifter bladet** i hello Azure-portalen efter en körning av hello exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-172">hello screenshot below shows hello **Preparation tasks blade** in hello Azure portal after a run of hello sample application.</span></span> <span data-ttu-id="dc3c4-173">Navigera toohello *JobPrepReleaseSampleJob* egenskaper när aktiviteterna har slutförts (men innan du tar bort dina jobb och pool) och klicka på **förberedelseuppgifter** eller **viktiga uppgifter** tooview deras egenskaper.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-173">Navigate toohello *JobPrepReleaseSampleJob* properties after your tasks have completed (but before deleting your job and pool) and click **Preparation tasks** or **Release tasks** tooview their properties.</span></span>

![Förberedelse av jobbegenskaper i Azure-portalen][1]

## <a name="next-steps"></a><span data-ttu-id="dc3c4-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dc3c4-175">Next steps</span></span>
### <a name="application-packages"></a><span data-ttu-id="dc3c4-176">Programpaket</span><span class="sxs-lookup"><span data-stu-id="dc3c4-176">Application packages</span></span>
<span data-ttu-id="dc3c4-177">I tillägg toohello förberedelse projektaktivitet, kan du också använda hello [programpaket](batch-application-packages.md) funktion i Batch tooprepare compute-noder för körning av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-177">In addition toohello job preparation task, you can also use hello [application packages](batch-application-packages.md) feature of Batch tooprepare compute nodes for task execution.</span></span> <span data-ttu-id="dc3c4-178">Den här funktionen är särskilt användbar för att distribuera program som inte kräver kör ett installationsprogram, program som innehåller många (100 +) filer eller program som kräver strikt versionskontroll.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-178">This feature is especially useful for deploying applications that do not require running an installer, applications that contain many (100+) files, or applications that require strict version control.</span></span>

### <a name="installing-applications-and-staging-data"></a><span data-ttu-id="dc3c4-179">Installera program och mellanlagring av data</span><span class="sxs-lookup"><span data-stu-id="dc3c4-179">Installing applications and staging data</span></span>
<span data-ttu-id="dc3c4-180">Den här MSDN-foruminlägg innehåller en översikt över flera metoder för att förbereda din noder för pågående aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="dc3c4-180">This MSDN forum post provides an overview of several methods of preparing your nodes for running tasks:</span></span>

<span data-ttu-id="dc3c4-181">[Installera program och mellanlagring av data på Batch-beräkningsnoder][forum_post]</span><span class="sxs-lookup"><span data-stu-id="dc3c4-181">[Installing applications and staging data on Batch compute nodes][forum_post]</span></span>

<span data-ttu-id="dc3c4-182">Skrivs av en av hello Azure Batch-gruppmedlemmar beskrivs den flera metoder som du kan använda toodeploy program och data toocompute noder.</span><span class="sxs-lookup"><span data-stu-id="dc3c4-182">Written by one of hello Azure Batch team members, it discusses several techniques that you can use toodeploy applications and data toocompute nodes.</span></span>

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
