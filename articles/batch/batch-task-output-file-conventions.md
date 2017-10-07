---
title: "aaaPersist projekt- och utdata tooAzure lagring med hello filen konventioner bibliotek för .NET - Azure Batch | Microsoft Docs"
description: "Lär dig hur toouse filen konventioner för Azure Batch-biblioteket för .NET toopersist batchaktiviteten och jobbet utdata tooAzure lagring och visa hello beständiga utdata i hello Azure-portalen."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cf2ac8632a13d32438c1bdcf11b4b9649de1e2b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="persist-job-and-task-data-tooazure-storage-with-hello-batch-file-conventions-library-for-net-toopersist"></a><span data-ttu-id="764b1-103">Spara jobbet och uppgift data tooAzure lagring med hello Batch filen konventioner biblioteket för .NET toopersist</span><span class="sxs-lookup"><span data-stu-id="764b1-103">Persist job and task data tooAzure Storage with hello Batch File Conventions library for .NET toopersist</span></span> 

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

<span data-ttu-id="764b1-104">Enkelriktade toopersist aktivitetsdata är toouse hello [filen konventioner för Azure Batch-biblioteket för .NET][nuget_package].</span><span class="sxs-lookup"><span data-stu-id="764b1-104">One way toopersist task data is toouse hello [Azure Batch File Conventions library for .NET][nuget_package].</span></span> <span data-ttu-id="764b1-105">hello filen konventioner bibliotek förenklar hello processen att lagra uppgiftsutdata data tooAzure lagring och hämtning av den.</span><span class="sxs-lookup"><span data-stu-id="764b1-105">hello File Conventions library simplifies hello process of storing task output data tooAzure Storage and retrieving it.</span></span> <span data-ttu-id="764b1-106">Du kan använda hello filen konventioner biblioteket i både uppgiften och klienten kod &mdash; uppgiftskoden för spara filer och klienten Platskod toolist och hämta dem.</span><span class="sxs-lookup"><span data-stu-id="764b1-106">You can use hello File Conventions library in both task and client code &mdash; in task code for persisting files, and in client code toolist and retrieve them.</span></span> <span data-ttu-id="764b1-107">Uppgiftskoden kan också använda hello biblioteket tooretrieve hello utdata överordnade uppgifter, som i en [uppgift beroenden](batch-task-dependencies.md) scenario.</span><span class="sxs-lookup"><span data-stu-id="764b1-107">Your task code can also use hello library tooretrieve hello output of upstream tasks, such as in a [task dependencies](batch-task-dependencies.md) scenario.</span></span> 

<span data-ttu-id="764b1-108">tooretrieve utdatafiler med hello filen konventioner bibliotek, du kan hitta hello filer för ett visst jobb eller en aktivitet genom att registrera dem med ID: T och syftet.</span><span class="sxs-lookup"><span data-stu-id="764b1-108">tooretrieve output files with hello File Conventions library, you can locate hello files for a given job or task by listing them by ID and purpose.</span></span> <span data-ttu-id="764b1-109">Du behöver inte tooknow hello namn eller platser hello-filer.</span><span class="sxs-lookup"><span data-stu-id="764b1-109">You don't need tooknow hello names or locations of hello files.</span></span> <span data-ttu-id="764b1-110">Du kan till exempel använda hello filen konventioner biblioteket toolist alla mellanliggande filer för en viss uppgift eller få en preview-fil för ett visst jobb.</span><span class="sxs-lookup"><span data-stu-id="764b1-110">For example, you can use hello File Conventions library toolist all intermediate files for a given task, or get a preview file for a given job.</span></span>

> [!TIP]
> <span data-ttu-id="764b1-111">Från och med version 2017-05-01, stöder hello API för Batch-tjänsten bestående utgående data tooAzure lagring för aktiviteter och jobb manager aktiviteter som körs på pooler som har skapats med hello konfiguration av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="764b1-111">Starting with version 2017-05-01, hello Batch service API supports persisting output data tooAzure Storage for tasks and job manager tasks that run on pools created with hello virtual machine configuration.</span></span> <span data-ttu-id="764b1-112">hello API för Batch-tjänsten tillhandahåller ett enkelt sätt toopersist utdata från inom hello-kod som skapar en uppgift och fungerar som ett alternativt toohello filen konventioner bibliotek.</span><span class="sxs-lookup"><span data-stu-id="764b1-112">hello Batch service API provides a simple way toopersist output from within hello code that creates a task and serves as an alternative toohello File Conventions library.</span></span> <span data-ttu-id="764b1-113">Du kan ändra din Batch klienten program toopersist utdata utan att behöva tooupdate hello program som uppgiften körs.</span><span class="sxs-lookup"><span data-stu-id="764b1-113">You can modify your Batch client applications toopersist output without needing tooupdate hello application that your task is running.</span></span> <span data-ttu-id="764b1-114">Mer information finns i [spara aktiviteten data tooAzure lagring med hello API för Batch-tjänsten](batch-task-output-files.md).</span><span class="sxs-lookup"><span data-stu-id="764b1-114">For more information, see [Persist task data tooAzure Storage with hello Batch service API](batch-task-output-files.md).</span></span>
> 
> 

## <a name="when-do-i-use-hello-file-conventions-library-toopersist-task-output"></a><span data-ttu-id="764b1-115">När använder hello filen konventioner biblioteket toopersist uppgiftens utdata?</span><span class="sxs-lookup"><span data-stu-id="764b1-115">When do I use hello File Conventions library toopersist task output?</span></span>

<span data-ttu-id="764b1-116">Azure Batch tillhandahåller mer än ett sätt toopersist uppgiftens utdata.</span><span class="sxs-lookup"><span data-stu-id="764b1-116">Azure Batch provides more than one way toopersist task output.</span></span> <span data-ttu-id="764b1-117">hello filen konventioner är bäst lämpade toothese scenarier:</span><span class="sxs-lookup"><span data-stu-id="764b1-117">hello File Conventions is best suited toothese scenarios:</span></span>

- <span data-ttu-id="764b1-118">Du kan enkelt ändra hello koden för hello programmet att uppgiften körs toopersist filer med hjälp av hello filen konventioner bibliotek.</span><span class="sxs-lookup"><span data-stu-id="764b1-118">You can easily modify hello code for hello application that your task is running toopersist files using hello File Conventions library.</span></span>
- <span data-ttu-id="764b1-119">Du vill toostream data tooAzure lagring medan hello aktivitet körs.</span><span class="sxs-lookup"><span data-stu-id="764b1-119">You want toostream data tooAzure Storage while hello task is still running.</span></span>
- <span data-ttu-id="764b1-120">Vill du toopersist data från poolen som skapats med hello molnet tjänstkonfiguration eller hello konfiguration av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="764b1-120">You want toopersist data from pools created with either hello cloud service configuration or hello virtual machine configuration.</span></span>
- <span data-ttu-id="764b1-121">Klientprogrammet eller andra uppgifter i hello jobb behov toolocate och hämta utdata aktivitetsfiler genom ID eller syfte.</span><span class="sxs-lookup"><span data-stu-id="764b1-121">Your client application or other tasks in hello job needs toolocate and download task output files by ID or by purpose.</span></span> 
- <span data-ttu-id="764b1-122">Vill du tooview uppgiftens utdata i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="764b1-122">You want tooview task output in hello Azure portal.</span></span>

<span data-ttu-id="764b1-123">Om ditt scenario skiljer sig från de som visas ovan, kanske du måste tooconsider en annan metod.</span><span class="sxs-lookup"><span data-stu-id="764b1-123">If your scenario differs from those listed above, you may need tooconsider a different approach.</span></span> <span data-ttu-id="764b1-124">Mer information om andra alternativ för bestående uppgiftsutdata finns [spara projekt- och utdata tooAzure lagring](batch-task-output.md).</span><span class="sxs-lookup"><span data-stu-id="764b1-124">For more information on other options for persisting task output, see [Persist job and task output tooAzure Storage](batch-task-output.md).</span></span> 

## <a name="what-is-hello-batch-file-conventions-standard"></a><span data-ttu-id="764b1-125">Vad är hello Batch filen konventioner standard?</span><span class="sxs-lookup"><span data-stu-id="764b1-125">What is hello Batch File Conventions standard?</span></span>

<span data-ttu-id="764b1-126">Hej [Batch filen konventioner standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) innehåller ett namngivningsschema för hello mål behållare och blob sökvägar toowhich skrivs utdatafiler.</span><span class="sxs-lookup"><span data-stu-id="764b1-126">hello [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) provides a naming scheme for hello destination containers and blob paths toowhich your output files are written.</span></span> <span data-ttu-id="764b1-127">Filer beständiga tooAzure lagring som följer toohello filen konventioner som standard är automatiskt tillgängliga för visning i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="764b1-127">Files persisted tooAzure Storage that adhere toohello File Conventions standard are automatically available for viewing in hello Azure portal.</span></span> <span data-ttu-id="764b1-128">hello portalen är medveten om hello namngivningskonvention och därför kan visa filer som följer tooit.</span><span class="sxs-lookup"><span data-stu-id="764b1-128">hello portal is aware of hello naming convention and so can display files that adhere tooit.</span></span>

<span data-ttu-id="764b1-129">hello filen konventioner biblioteket för .NET namn automatiskt din behållare för lagring och uppgiften utdatafilerna enligt toohello filen konventioner som standard.</span><span class="sxs-lookup"><span data-stu-id="764b1-129">hello File Conventions library for .NET automatically names your storage containers and task output files according toohello File Conventions standard.</span></span> <span data-ttu-id="764b1-130">hello filen konventioner biblioteket innehåller också metoder tooquery utdatafilerna i Azure Storage enligt toojob ID, aktivitets-ID eller syfte.</span><span class="sxs-lookup"><span data-stu-id="764b1-130">hello File Conventions library also provides methods tooquery output files in Azure Storage according toojob ID, task ID, or purpose.</span></span>   

<span data-ttu-id="764b1-131">Om du utvecklar med ett annat språk än .NET, kan du implementera hello filen konventioner standard själv i ditt program.</span><span class="sxs-lookup"><span data-stu-id="764b1-131">If you are developing with a language other than .NET, you can implement hello File Conventions standard yourself in your application.</span></span> <span data-ttu-id="764b1-132">Mer information finns i [om hello Batch filen konventioner standard](batch-task-output.md#about-the-batch-file-conventions-standard).</span><span class="sxs-lookup"><span data-stu-id="764b1-132">For more information, see [About hello Batch File Conventions standard](batch-task-output.md#about-the-batch-file-conventions-standard).</span></span>

## <a name="link-an-azure-storage-account-tooyour-batch-account"></a><span data-ttu-id="764b1-133">Länka ett Azure Storage-konto tooyour Batch-kontot</span><span class="sxs-lookup"><span data-stu-id="764b1-133">Link an Azure Storage account tooyour Batch account</span></span>

<span data-ttu-id="764b1-134">toopersist utdata data tooAzure lagring med hjälp av hello filen konventioner bibliotek, måste du först koppla ett Azure Storage-konto tooyour Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="764b1-134">toopersist output data tooAzure Storage using hello File Conventions library, you must first link an Azure Storage account tooyour Batch account.</span></span> <span data-ttu-id="764b1-135">Om du inte redan har gjort länka en Storage-konto tooyour Batch-kontot med hjälp av hello [Azure-portalen](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="764b1-135">If you haven't done so already, link a Storage account tooyour Batch account by using hello [Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="764b1-136">Navigera tooyour Batch-kontot i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="764b1-136">Navigate tooyour Batch account in hello Azure portal.</span></span> 
2. <span data-ttu-id="764b1-137">Under **inställningar**väljer **Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="764b1-137">Under **Settings**, select **Storage Account**.</span></span>
3. <span data-ttu-id="764b1-138">Om du inte redan har ett lagringskonto som är associerade med Batch-kontot, klickar du på **Storage-konto (ingen)**.</span><span class="sxs-lookup"><span data-stu-id="764b1-138">If you do not already have a Storage account associated with your Batch account, click **Storage Account (None)**.</span></span>
4. <span data-ttu-id="764b1-139">Välj ett lagringskonto hello listan för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="764b1-139">Select a Storage account from hello list for your subscription.</span></span> <span data-ttu-id="764b1-140">För bästa prestanda använder du ett Azure Storage-konto som är i hello samma region som hello Batch-kontot var dina aktiviteter körs.</span><span class="sxs-lookup"><span data-stu-id="764b1-140">For best performance, use an Azure Storage account that is in hello same region as hello Batch account where your tasks are running.</span></span>

## <a name="persist-output-data"></a><span data-ttu-id="764b1-141">Spara utdata</span><span class="sxs-lookup"><span data-stu-id="764b1-141">Persist output data</span></span>

<span data-ttu-id="764b1-142">toopersist projekt- och utdata med hello filen konventioner bibliotek, skapa en behållare i Azure Storage och sedan spara hello utdata toohello behållare.</span><span class="sxs-lookup"><span data-stu-id="764b1-142">toopersist job and task output data with hello File Conventions library, create a container in Azure Storage, then save hello output toohello container.</span></span> <span data-ttu-id="764b1-143">Använd hello [Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage) i din aktivitet kod tooupload hello uppgiften utdata toohello behållaren.</span><span class="sxs-lookup"><span data-stu-id="764b1-143">Use hello [Azure Storage client library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage) in your task code tooupload hello task output toohello container.</span></span> 

<span data-ttu-id="764b1-144">Mer information om hur du arbetar med behållare och blobbar i Azure Storage finns [komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="764b1-144">For more information about working with containers and blobs in Azure Storage, see [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span>

> [!WARNING]
> <span data-ttu-id="764b1-145">Alla utdata för jobb- och beständiga med hello filen konventioner biblioteket lagras i hello samma behållare.</span><span class="sxs-lookup"><span data-stu-id="764b1-145">All job and task outputs persisted with hello File Conventions library are stored in hello same container.</span></span> <span data-ttu-id="764b1-146">Om ett stort antal uppgifter försök toopersist filer på hello samma tid, [lagring throttling-begränsning](../storage/common/storage-performance-checklist.md#blobs) eventuellt.</span><span class="sxs-lookup"><span data-stu-id="764b1-146">If a large number of tasks try toopersist files at hello same time, [storage throttling limits](../storage/common/storage-performance-checklist.md#blobs) may be enforced.</span></span>
> 
> 

### <a name="create-storage-container"></a><span data-ttu-id="764b1-147">Skapa lagringsbehållaren</span><span class="sxs-lookup"><span data-stu-id="764b1-147">Create storage container</span></span>

<span data-ttu-id="764b1-148">toopersist aktivitet utdata tooAzure lagring, först skapa en behållare genom att anropa [CloudJob][net_cloudjob].[ PrepareOutputStorageAsync][net_prepareoutputasync].</span><span class="sxs-lookup"><span data-stu-id="764b1-148">toopersist task output tooAzure Storage, first create a container by calling [CloudJob][net_cloudjob].[PrepareOutputStorageAsync][net_prepareoutputasync].</span></span> <span data-ttu-id="764b1-149">Den här tilläggsmetoden tar en [CloudStorageAccount] [ net_cloudstorageaccount] objekt som parameter.</span><span class="sxs-lookup"><span data-stu-id="764b1-149">This extension method takes a [CloudStorageAccount][net_cloudstorageaccount] object as a parameter.</span></span> <span data-ttu-id="764b1-150">En behållare med namnet bl.a toohello filen konventioner standard, så att innehållet är synliga genom hello Azure-portalen och hello hämtning av metoderna som beskrivs senare i artikeln hello skapas.</span><span class="sxs-lookup"><span data-stu-id="764b1-150">It creates a container named according toohello File Conventions standard,so that its contents are discoverable by hello Azure portal and hello retrieval methods discussed later in hello article.</span></span>

<span data-ttu-id="764b1-151">Du vanligtvis placera hello koden toocreate en behållare i ditt klientprogram &mdash; hello program som skapar din pooler, jobb och uppgifter.</span><span class="sxs-lookup"><span data-stu-id="764b1-151">You typically place hello code toocreate a container in your client application &mdash; hello application that creates your pools, jobs, and tasks.</span></span>

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference toohello linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create hello blob storage container for hello outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a><span data-ttu-id="764b1-152">Lagra utdata för aktiviteten</span><span class="sxs-lookup"><span data-stu-id="764b1-152">Store task outputs</span></span>

<span data-ttu-id="764b1-153">Nu när du har skapat en behållare i Azure Storage uppgifter kan spara utdata toohello behållare med hello [TaskOutputStorage] [ net_taskoutputstorage] klass hittades i hello filen konventioner bibliotek.</span><span class="sxs-lookup"><span data-stu-id="764b1-153">Now that you've prepared a container in Azure Storage, tasks can save output toohello container by using hello [TaskOutputStorage][net_taskoutputstorage] class found in hello File Conventions library.</span></span>

<span data-ttu-id="764b1-154">I koden aktivitet, först skapa en [TaskOutputStorage] [ net_taskoutputstorage] objekt och sedan när hello har slutförts sitt arbete, anropa hello [TaskOutputStorage] [ net_taskoutputstorage]. [SaveAsync] [ net_saveasync] metoden toosave dess utdata tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="764b1-154">In your task code, first create a [TaskOutputStorage][net_taskoutputstorage] object, then when hello task has completed its work, call hello [TaskOutputStorage][net_taskoutputstorage].[SaveAsync][net_saveasync] method toosave its output tooAzure Storage.</span></span>

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code tooprocess data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

<span data-ttu-id="764b1-155">Hej `kind` parametern för hello [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[ SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) metoden kategoriserar hello beständiga filer.</span><span class="sxs-lookup"><span data-stu-id="764b1-155">hello `kind` parameter of hello [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) method categorizes hello persisted files.</span></span> <span data-ttu-id="764b1-156">Det finns fyra fördefinierade [TaskOutputKind] [ net_taskoutputkind] typer: `TaskOutput`, `TaskPreview`, `TaskLog`, och `TaskIntermediate.` du kan också definiera egna kategorier av utdata.</span><span class="sxs-lookup"><span data-stu-id="764b1-156">There are four predefined [TaskOutputKind][net_taskoutputkind] types: `TaskOutput`, `TaskPreview`, `TaskLog`, and `TaskIntermediate.` You can also define custom categories of output.</span></span>

<span data-ttu-id="764b1-157">Dessa utdata-typer kan du toospecify vilken typ av utdata toolist när du senare frågar Batch för hello beständiga utdata för en viss uppgift.</span><span class="sxs-lookup"><span data-stu-id="764b1-157">These output types allow you toospecify which type of outputs toolist when you later query Batch for hello persisted outputs of a given task.</span></span> <span data-ttu-id="764b1-158">Med andra ord, när du listar hello utdata för en aktivitet, kan du filtrera hello på någon av hello utdata typer.</span><span class="sxs-lookup"><span data-stu-id="764b1-158">In other words, when you list hello outputs for a task, you can filter hello list on one of hello output types.</span></span> <span data-ttu-id="764b1-159">Till exempel ”ge mig hello *preview* utdata för aktiviteten *109*”.</span><span class="sxs-lookup"><span data-stu-id="764b1-159">For example, "Give me hello *preview* output for task *109*."</span></span> <span data-ttu-id="764b1-160">Mer om lista och hämta utdata visas i [hämta utdata](#retrieve-output) senare i hello artikeln.</span><span class="sxs-lookup"><span data-stu-id="764b1-160">More on listing and retrieving outputs appears in [Retrieve output](#retrieve-output) later in hello article.</span></span>

> [!TIP]
> <span data-ttu-id="764b1-161">hello utdata typ avgör också var i hello Azure portal en viss fil visas: *TaskOutput*-kategoriserade filer visas **aktivitet utdatafiler**, och *TaskLog*filer visas **uppgift loggar**.</span><span class="sxs-lookup"><span data-stu-id="764b1-161">hello output kind also determines where in hello Azure portal a particular file appears: *TaskOutput*-categorized files appear under **Task output files**, and *TaskLog* files appear under **Task logs**.</span></span>
> 
> 

### <a name="store-job-outputs"></a><span data-ttu-id="764b1-162">Lagra utdata för jobbet</span><span class="sxs-lookup"><span data-stu-id="764b1-162">Store job outputs</span></span>

<span data-ttu-id="764b1-163">Dessutom toostoring resultat av åtgärder, kan du lagra hello utdata som är associerade med en hela jobbet.</span><span class="sxs-lookup"><span data-stu-id="764b1-163">In addition toostoring task outputs, you can store hello outputs associated with an entire job.</span></span> <span data-ttu-id="764b1-164">I hello merge uppgiften för ett jobb för återgivning av film kan du spara hello fullständigt renderas film som en jobbutdata.</span><span class="sxs-lookup"><span data-stu-id="764b1-164">For example, in hello merge task of a movie rendering job, you could persist hello fully rendered movie as a job output.</span></span> <span data-ttu-id="764b1-165">När jobbet har slutförts, ditt klientprogram kan visa och hämta hello utdata för hello jobb och har inte behöver tooquery hello enskilda aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="764b1-165">When your job is completed, your client application can list and retrieve hello outputs for hello job, and does not need tooquery hello individual tasks.</span></span>

<span data-ttu-id="764b1-166">Lagra jobbutdata genom att anropa hello [JobOutputStorage][net_joboutputstorage].[ SaveAsync] [ net_joboutputstorage_saveasync] metod, och ange hello [JobOutputKind] [ net_joboutputkind] och filnamn:</span><span class="sxs-lookup"><span data-stu-id="764b1-166">Store job output by calling hello [JobOutputStorage][net_joboutputstorage].[SaveAsync][net_joboutputstorage_saveasync] method, and specify hello [JobOutputKind][net_joboutputkind] and filename:</span></span>

```csharp
CloudJob job = new JobOutputStorage(acct, jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

<span data-ttu-id="764b1-167">Precis som med hello **TaskOutputKind** typen för aktiviteten utdata, använder du hello [JobOutputKind] [ net_joboutputkind] typen toocategorize ett jobb har sparat filer.</span><span class="sxs-lookup"><span data-stu-id="764b1-167">As with hello **TaskOutputKind** type for task outputs, you use hello [JobOutputKind][net_joboutputkind] type toocategorize a job's persisted files.</span></span> <span data-ttu-id="764b1-168">Den här parametern kan du fråga toolater (lista) en viss typ av utdata.</span><span class="sxs-lookup"><span data-stu-id="764b1-168">This parameter allows you toolater query for (list) a specific type of output.</span></span> <span data-ttu-id="764b1-169">Hej **JobOutputKind** typ innehåller både utdata och förhandsgranska kategorier och kan du skapa egna kategorier.</span><span class="sxs-lookup"><span data-stu-id="764b1-169">hello **JobOutputKind** type includes both output and preview categories, and supports creating custom categories.</span></span>

### <a name="store-task-logs"></a><span data-ttu-id="764b1-170">Lagrar uppgiften loggar</span><span class="sxs-lookup"><span data-stu-id="764b1-170">Store task logs</span></span>

<span data-ttu-id="764b1-171">Dessutom toopersisting en toodurable fillagring när en aktivitet eller jobbet har slutförts kan du behöva toopersist filer som uppdateras under hello körning av uppgiften &mdash; loggfiler eller `stdout.txt` och `stderr.txt`, till exempel.</span><span class="sxs-lookup"><span data-stu-id="764b1-171">In addition toopersisting a file toodurable storage when a task or job completes, you may need toopersist files that are updated during hello execution of a task &mdash; log files or `stdout.txt` and `stderr.txt`, for example.</span></span> <span data-ttu-id="764b1-172">För detta ändamål hello Azure Batch filen konventioner-bibliotek innehåller hello [TaskOutputStorage][net_taskoutputstorage].[ SaveTrackedAsync] [ net_savetrackedasync] metod.</span><span class="sxs-lookup"><span data-stu-id="764b1-172">For this purpose, hello Azure Batch File Conventions library provides hello [TaskOutputStorage][net_taskoutputstorage].[SaveTrackedAsync][net_savetrackedasync] method.</span></span> <span data-ttu-id="764b1-173">Med [SaveTrackedAsync][net_savetrackedasync], kan du spåra uppdateringar tooa fil på hello nod (på ett intervall som du anger) och spara dessa uppdateringar tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="764b1-173">With [SaveTrackedAsync][net_savetrackedasync], you can track updates tooa file on hello node (at an interval that you specify) and persist those updates tooAzure Storage.</span></span>

<span data-ttu-id="764b1-174">I följande kodavsnitt hello, vi använda [SaveTrackedAsync] [ net_savetrackedasync] tooupdate `stdout.txt` i Azure Storage var 15: e sekund vid hello körning av aktiviteten hello:</span><span class="sxs-lookup"><span data-stu-id="764b1-174">In hello following code snippet, we use [SaveTrackedAsync][net_savetrackedasync] tooupdate `stdout.txt` in Azure Storage every 15 seconds during hello execution of hello task:</span></span>

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// hello primary task logic is wrapped in a using statement that sends updates to
// hello stdout.txt blob in Storage every 15 seconds while hello task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code tooprocess data and produce output file(s) */

    // We are tracking hello disk file toosave our standard output, but the
    // node agent may take up too3 seconds tooflush hello stdout stream to
    // disk. So give hello file a moment toocatch up.
     await Task.Delay(stdoutFlushDelay);
}
```

<span data-ttu-id="764b1-175">hello kommenterats avsnittet `Code tooprocess data and produce output file(s)` är en platshållare för hello-kod som normalt skulle utföra uppgiften.</span><span class="sxs-lookup"><span data-stu-id="764b1-175">hello commented section `Code tooprocess data and produce output file(s)` is a placeholder for hello code that your task would normally perform.</span></span> <span data-ttu-id="764b1-176">Du kan till exempel ha kod som hämtar data från Azure Storage och utför transformering eller beräkning på den.</span><span class="sxs-lookup"><span data-stu-id="764b1-176">For example, you might have code that downloads data from Azure Storage and performs transformation or calculation on it.</span></span> <span data-ttu-id="764b1-177">hello viktig del av den här fragment visar hur du kan anpassa sådan kod i en `using` block tooperiodically uppdatera en fil med [SaveTrackedAsync][net_savetrackedasync].</span><span class="sxs-lookup"><span data-stu-id="764b1-177">hello important part of this snippet is demonstrating how you can wrap such code in a `using` block tooperiodically update a file with [SaveTrackedAsync][net_savetrackedasync].</span></span>

<span data-ttu-id="764b1-178">hello nod agent är ett program som körs på varje nod i hello pool och tillhandahåller hello och kommandokontroll gränssnitt mellan hello nod och hello Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="764b1-178">hello node agent is a program that runs on each node in hello pool and provides hello command-and-control interface between hello node and hello Batch service.</span></span> <span data-ttu-id="764b1-179">Hej `Task.Delay` anropet krävs hello slutet av det här `using` block tooensure som hello nod agent har tid tooflush hello innehållet i standard ut toohello stdout.txt fil på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="764b1-179">hello `Task.Delay` call is required at hello end of this `using` block tooensure that hello node agent has time tooflush hello contents of standard out toohello stdout.txt file on hello node.</span></span> <span data-ttu-id="764b1-180">Utan den här fördröjningen är det möjligt toomiss hello senaste några sekunder för utdata.</span><span class="sxs-lookup"><span data-stu-id="764b1-180">Without this delay, it is possible toomiss hello last few seconds of output.</span></span> <span data-ttu-id="764b1-181">Den här fördröjningen kan inte krävas för alla filer.</span><span class="sxs-lookup"><span data-stu-id="764b1-181">This delay may not be required for all files.</span></span>

> [!NOTE]
> <span data-ttu-id="764b1-182">När du aktiverar spårning med filen **SaveTrackedAsync**, endast *lägger till* toohello spårade filen är beständiga tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="764b1-182">When you enable file tracking with **SaveTrackedAsync**, only *appends* toohello tracked file are persisted tooAzure Storage.</span></span> <span data-ttu-id="764b1-183">Använd den här metoden endast för spårning av icke-rotera loggfiler eller andra filer som har skrivits toowith lägga till operations hello filens toohello slut.</span><span class="sxs-lookup"><span data-stu-id="764b1-183">Use this method only for tracking non-rotating log files or other files that are written toowith append operations toohello end of hello file.</span></span>
> 
> 

## <a name="retrieve-output-data"></a><span data-ttu-id="764b1-184">Hämta utdata</span><span class="sxs-lookup"><span data-stu-id="764b1-184">Retrieve output data</span></span>

<span data-ttu-id="764b1-185">När du hämtar utdatan beständiga använder hello Azure Batch filen konventioner bibliotek, gör du det i en aktivitet till och jobbet-central sätt.</span><span class="sxs-lookup"><span data-stu-id="764b1-185">When you retrieve your persisted output using hello Azure Batch File Conventions library, you do so in a task- and job-centric manner.</span></span> <span data-ttu-id="764b1-186">Du kan begära hello utdata för det angivna aktivitets- eller utan att behöva tooknow dess sökväg i Azure Storage eller även dess filnamn.</span><span class="sxs-lookup"><span data-stu-id="764b1-186">You can request hello output for given task or job without needing tooknow its path in Azure Storage, or even its file name.</span></span> <span data-ttu-id="764b1-187">Du kan i stället begära utdatafilerna av aktivitet eller jobb-ID.</span><span class="sxs-lookup"><span data-stu-id="764b1-187">Instead, you can request output files by task or job ID.</span></span>

<span data-ttu-id="764b1-188">hello följande kodavsnitt går igenom ett jobb uppgifter, skriver ut information om hello utdatafilerna för hello aktivitet och hämtar sedan dess filer från lagring.</span><span class="sxs-lookup"><span data-stu-id="764b1-188">hello following code snippet iterates through a job's tasks, prints some information about hello output files for hello task, and then downloads its files from Storage.</span></span>

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (OutputFileReference output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="view-output-files-in-hello-azure-portal"></a><span data-ttu-id="764b1-189">Visa utdatafiler i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="764b1-189">View output files in hello Azure portal</span></span>

<span data-ttu-id="764b1-190">hello Azure-portalen visar aktivitet utdatafilerna och loggar som är beständiga tooa länkad Azure Storage-konto med hello [Batch filen konventioner standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions).</span><span class="sxs-lookup"><span data-stu-id="764b1-190">hello Azure portal displays task output files and logs that are persisted tooa linked Azure Storage account using hello [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions).</span></span> <span data-ttu-id="764b1-191">Du kan också implementera dessa konventioner i hello ett annat språk eller du kan använda hello filen konventioner biblioteket i .NET-program.</span><span class="sxs-lookup"><span data-stu-id="764b1-191">You can implement these conventions yourself in hello a language of your choice, or you can use hello File Conventions library in your .NET applications.</span></span>

<span data-ttu-id="764b1-192">tooenable hello visningen av dina utdatafiler i hello portal, måste du uppfylla hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="764b1-192">tooenable hello display of your output files in hello portal, you must satisfy hello following requirements:</span></span>

1. <span data-ttu-id="764b1-193">[Koppla ett Azure Storage-konto](#requirement-linked-storage-account) tooyour Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="764b1-193">[Link an Azure Storage account](#requirement-linked-storage-account) tooyour Batch account.</span></span>
2. <span data-ttu-id="764b1-194">Följa toohello fördefinierade regler för namngivning av behållare för lagring och filer när beständighet utdata.</span><span class="sxs-lookup"><span data-stu-id="764b1-194">Adhere toohello predefined naming conventions for Storage containers and files when persisting outputs.</span></span> <span data-ttu-id="764b1-195">Du hittar hello definition av dessa konventioner i hello filen konventioner biblioteket [viktigt][github_file_conventions_readme].</span><span class="sxs-lookup"><span data-stu-id="764b1-195">You can find hello definition of these conventions in hello File Conventions library [README][github_file_conventions_readme].</span></span> <span data-ttu-id="764b1-196">Om du använder hello [Azure Batch filen konventioner] [ nuget_package] biblioteket toopersist din utdata, dina filer sparas enligt toohello filen konventioner som standard.</span><span class="sxs-lookup"><span data-stu-id="764b1-196">If you use hello [Azure Batch File Conventions][nuget_package] library toopersist your output, your files are persisted according toohello File Conventions standard.</span></span>

<span data-ttu-id="764b1-197">tooview uppgiftsutdata filer och loggar in hello Azure-portalen, navigera toohello aktivitet vars utdata som du är intresserad av, klickar sedan på antingen **sparad utdatafilerna** eller **sparade loggarna**.</span><span class="sxs-lookup"><span data-stu-id="764b1-197">tooview task output files and logs in hello Azure portal, navigate toohello task whose output you are interested in, then click either **Saved output files** or **Saved logs**.</span></span> <span data-ttu-id="764b1-198">Den här bilden visar hello **sparad utdatafilerna** för hello uppgiften med ID ”007”:</span><span class="sxs-lookup"><span data-stu-id="764b1-198">This image shows hello **Saved output files** for hello task with ID "007":</span></span>

<span data-ttu-id="764b1-199">![Resultat av åtgärder bladet i hello Azure-portalen][2]</span><span class="sxs-lookup"><span data-stu-id="764b1-199">![Task outputs blade in hello Azure portal][2]</span></span>

## <a name="code-sample"></a><span data-ttu-id="764b1-200">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="764b1-200">Code sample</span></span>

<span data-ttu-id="764b1-201">Hej [PersistOutputs] [ github_persistoutputs] exempelprojektet är en av hello [kodexempel för Azure Batch] [ github_samples] på GitHub.</span><span class="sxs-lookup"><span data-stu-id="764b1-201">hello [PersistOutputs][github_persistoutputs] sample project is one of hello [Azure Batch code samples][github_samples] on GitHub.</span></span> <span data-ttu-id="764b1-202">Den här Visual Studio-lösningen visar hur toouse hello Azure Batch filen konventioner biblioteket toopersist aktivitet utdata toodurable lagring.</span><span class="sxs-lookup"><span data-stu-id="764b1-202">This Visual Studio solution demonstrates how toouse hello Azure Batch File Conventions library toopersist task output toodurable storage.</span></span> <span data-ttu-id="764b1-203">toorun hello exempel, följer du dessa steg:</span><span class="sxs-lookup"><span data-stu-id="764b1-203">toorun hello sample, follow these steps:</span></span>

1. <span data-ttu-id="764b1-204">Öppna hello-projekt i **Visual Studio 2015 eller senare**.</span><span class="sxs-lookup"><span data-stu-id="764b1-204">Open hello project in **Visual Studio 2015 or newer**.</span></span>
2. <span data-ttu-id="764b1-205">Lägg till grupp och lagring **kontoautentiseringsuppgifter** för**AccountSettings.settings** i hello Microsoft.Azure.Batch.Samples.Common projekt.</span><span class="sxs-lookup"><span data-stu-id="764b1-205">Add your Batch and Storage **account credentials** too**AccountSettings.settings** in hello Microsoft.Azure.Batch.Samples.Common project.</span></span>
3. <span data-ttu-id="764b1-206">**Skapa** (men inte kör) hello lösning.</span><span class="sxs-lookup"><span data-stu-id="764b1-206">**Build** (but do not run) hello solution.</span></span> <span data-ttu-id="764b1-207">Återställa NuGet-paket om du uppmanas till detta.</span><span class="sxs-lookup"><span data-stu-id="764b1-207">Restore any NuGet packages if prompted.</span></span>
4. <span data-ttu-id="764b1-208">Använd hello Azure portal tooupload en [programpaket](batch-application-packages.md) för **PersistOutputsTask**.</span><span class="sxs-lookup"><span data-stu-id="764b1-208">Use hello Azure portal tooupload an [application package](batch-application-packages.md) for **PersistOutputsTask**.</span></span> <span data-ttu-id="764b1-209">Inkludera hello `PersistOutputsTask.exe` och dess beroende sammansättningar i hello ZIP-paketet, ange hello program-ID för ”PersistOutputsTask” och programmet hello Paketversion för ”1.0”.</span><span class="sxs-lookup"><span data-stu-id="764b1-209">Include hello `PersistOutputsTask.exe` and its dependent assemblies in hello .zip package, set hello application ID too"PersistOutputsTask", and hello application package version too"1.0".</span></span>
5. <span data-ttu-id="764b1-210">**Starta** (kör) hello **PersistOutputs** projekt.</span><span class="sxs-lookup"><span data-stu-id="764b1-210">**Start** (run) hello **PersistOutputs** project.</span></span>
6. <span data-ttu-id="764b1-211">När begärd toochoose hello beständiga teknik toouse körs hello exempel anger **1** toorun hello exempel med hjälp av hello filen konventioner biblioteket toopersist uppgiftens utdata.</span><span class="sxs-lookup"><span data-stu-id="764b1-211">When prompted toochoose hello persistence technology toouse for running hello sample, enter **1** toorun hello sample using hello File Conventions library toopersist task output.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="764b1-212">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="764b1-212">Next steps</span></span>

### <a name="get-hello-batch-file-conventions-library-for-net"></a><span data-ttu-id="764b1-213">Hämta hello Batch filen konventioner bibliotek för .NET</span><span class="sxs-lookup"><span data-stu-id="764b1-213">Get hello Batch File Conventions library for .NET</span></span>

<span data-ttu-id="764b1-214">hello Batch filen konventioner bibliotek för .NET är tillgängligt på [NuGet][nuget_package].</span><span class="sxs-lookup"><span data-stu-id="764b1-214">hello Batch File Conventions library for .NET is available on [NuGet][nuget_package].</span></span> <span data-ttu-id="764b1-215">hello biblioteket utökar hello [CloudJob] [ net_cloudjob] och [CloudTask] [ net_cloudtask] klasser med nya metoder.</span><span class="sxs-lookup"><span data-stu-id="764b1-215">hello library extends hello [CloudJob][net_cloudjob] and [CloudTask][net_cloudtask] classes with new methods.</span></span> <span data-ttu-id="764b1-216">Se även hello [refererar dokumentationen](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) för hello filen konventioner biblioteket.</span><span class="sxs-lookup"><span data-stu-id="764b1-216">Also see hello [reference documentation](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) for hello File Conventions library.</span></span>

<span data-ttu-id="764b1-217">Hej [källkod] [ github_file_conventions] för hello filen konventioner bibliotek är tillgängligt på GitHub i hello Microsoft Azure SDK för .NET-databasen.</span><span class="sxs-lookup"><span data-stu-id="764b1-217">hello [source code][github_file_conventions] for hello File Conventions library is available on GitHub in hello Microsoft Azure SDK for .NET repository.</span></span> 

### <a name="explore-other-approaches-for-persisting-output-data"></a><span data-ttu-id="764b1-218">Utforska andra metoder för bestående av utdata</span><span class="sxs-lookup"><span data-stu-id="764b1-218">Explore other approaches for persisting output data</span></span>

- <span data-ttu-id="764b1-219">Se [spara projekt- och utdata tooAzure lagring](batch-task-output.md) för en översikt över bestående aktivitets- och data.</span><span class="sxs-lookup"><span data-stu-id="764b1-219">See [Persist job and task output tooAzure Storage](batch-task-output.md) for an overview of persisting task and job data.</span></span>
- <span data-ttu-id="764b1-220">Se [spara aktiviteten data tooAzure lagring med hello API för Batch-tjänsten](batch-task-output-files.md) toolearn hur toouse hello API för Batch-tjänsten toopersist utdata.</span><span class="sxs-lookup"><span data-stu-id="764b1-220">See [Persist task data tooAzure Storage with hello Batch service API](batch-task-output-files.md) toolearn how toouse hello Batch service API toopersist output data.</span></span>

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Sparad utdatafiler och sparade loggarna väljare i portalen"
[2]: ./media/batch-task-output/task-output-02.png "Resultat av åtgärder bladet i hello Azure-portalen"
