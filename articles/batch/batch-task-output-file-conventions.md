---
title: "Spara jobb- och utdata till Azure Storage med filen konventioner-biblioteket för .NET - Azure Batch | Microsoft Docs"
description: "Lär dig hur du använder filen konventioner för Azure Batch-biblioteket för .NET för att bevara Batch aktivitets- och utdata till Azure Storage och visa beständiga utdata i Azure-portalen."
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
ms.openlocfilehash: a9de327c20463469bc91d9720aa17333a36f919e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="persist-job-and-task-data-to-azure-storage-with-the-batch-file-conventions-library-for-net-to-persist"></a><span data-ttu-id="e625d-103">Spara jobb- och data till Azure Storage med Batch filen konventioner biblioteket för .NET att bevara</span><span class="sxs-lookup"><span data-stu-id="e625d-103">Persist job and task data to Azure Storage with the Batch File Conventions library for .NET to persist</span></span> 

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

<span data-ttu-id="e625d-104">Ett sätt att spara uppgiftsdata är att använda den [filen konventioner för Azure Batch-biblioteket för .NET][nuget_package].</span><span class="sxs-lookup"><span data-stu-id="e625d-104">One way to persist task data is to use the [Azure Batch File Conventions library for .NET][nuget_package].</span></span> <span data-ttu-id="e625d-105">Filen konventioner biblioteket gör enklare att lagra utdata för aktiviteten till Azure-lagring och hämtning av den.</span><span class="sxs-lookup"><span data-stu-id="e625d-105">The File Conventions library simplifies the process of storing task output data to Azure Storage and retrieving it.</span></span> <span data-ttu-id="e625d-106">Du kan använda filen konventioner biblioteket i både uppgiften och klienten kod &mdash; i uppgiftskoden för spara filer och i klientkod till listan och hämta dem.</span><span class="sxs-lookup"><span data-stu-id="e625d-106">You can use the File Conventions library in both task and client code &mdash; in task code for persisting files, and in client code to list and retrieve them.</span></span> <span data-ttu-id="e625d-107">Uppgiftskoden kan också använda biblioteket för att hämta utdata från överordnad uppgifter, som i en [uppgift beroenden](batch-task-dependencies.md) scenario.</span><span class="sxs-lookup"><span data-stu-id="e625d-107">Your task code can also use the library to retrieve the output of upstream tasks, such as in a [task dependencies](batch-task-dependencies.md) scenario.</span></span> 

<span data-ttu-id="e625d-108">Om du vill hämta utdatafilerna till bibliotek för filen konventioner hittar du filerna för ett visst jobb eller en aktivitet genom efter ID och syfte.</span><span class="sxs-lookup"><span data-stu-id="e625d-108">To retrieve output files with the File Conventions library, you can locate the files for a given job or task by listing them by ID and purpose.</span></span> <span data-ttu-id="e625d-109">Behöver du inte känner till namn eller platser för filer.</span><span class="sxs-lookup"><span data-stu-id="e625d-109">You don't need to know the names or locations of the files.</span></span> <span data-ttu-id="e625d-110">Du kan till exempel använda filen konventioner biblioteket för att lista alla mellanliggande filer för en viss uppgift eller få en preview-fil för ett visst jobb.</span><span class="sxs-lookup"><span data-stu-id="e625d-110">For example, you can use the File Conventions library to list all intermediate files for a given task, or get a preview file for a given job.</span></span>

> [!TIP]
> <span data-ttu-id="e625d-111">Från och med version 2017-05-01, stöder API för Batch-tjänsten bestående av utdata till Azure Storage för aktiviteter och jobb manager aktiviteter som körs på pooler som har skapats med den virtuella datorkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="e625d-111">Starting with version 2017-05-01, the Batch service API supports persisting output data to Azure Storage for tasks and job manager tasks that run on pools created with the virtual machine configuration.</span></span> <span data-ttu-id="e625d-112">API för Batch-tjänsten gör det enkelt att bevara utdata från inom den kod som skapar en uppgift och fungerar som ett alternativ i filen konventioner-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="e625d-112">The Batch service API provides a simple way to persist output from within the code that creates a task and serves as an alternative to the File Conventions library.</span></span> <span data-ttu-id="e625d-113">Du kan ändra din Batch klientprogram att bevara utdata utan att behöva uppdatera de program där aktiviteten körs.</span><span class="sxs-lookup"><span data-stu-id="e625d-113">You can modify your Batch client applications to persist output without needing to update the application that your task is running.</span></span> <span data-ttu-id="e625d-114">Mer information finns i [spara aktivitetsdata till Azure Storage med Batch-tjänsten API](batch-task-output-files.md).</span><span class="sxs-lookup"><span data-stu-id="e625d-114">For more information, see [Persist task data to Azure Storage with the Batch service API](batch-task-output-files.md).</span></span>
> 
> 

## <a name="when-do-i-use-the-file-conventions-library-to-persist-task-output"></a><span data-ttu-id="e625d-115">När använda filen konventioner biblioteket för att bevara uppgiftsutdata?</span><span class="sxs-lookup"><span data-stu-id="e625d-115">When do I use the File Conventions library to persist task output?</span></span>

<span data-ttu-id="e625d-116">Azure Batch tillhandahåller mer än ett sätt att bevara uppgiftens utdata.</span><span class="sxs-lookup"><span data-stu-id="e625d-116">Azure Batch provides more than one way to persist task output.</span></span> <span data-ttu-id="e625d-117">Fil-konventioner fungerar bäst i dessa scenarier:</span><span class="sxs-lookup"><span data-stu-id="e625d-117">The File Conventions is best suited to these scenarios:</span></span>

- <span data-ttu-id="e625d-118">Du kan enkelt ändra koden för det program som kör uppgiften för att spara filer med hjälp av filen konventioner-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="e625d-118">You can easily modify the code for the application that your task is running to persist files using the File Conventions library.</span></span>
- <span data-ttu-id="e625d-119">Vill du dataströmmen data till Azure Storage medan uppgiften körs.</span><span class="sxs-lookup"><span data-stu-id="e625d-119">You want to stream data to Azure Storage while the task is still running.</span></span>
- <span data-ttu-id="e625d-120">Du vill bevara data från poolen som skapats med molnkonfigurationen för tjänsten eller konfigurationen av virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e625d-120">You want to persist data from pools created with either the cloud service configuration or the virtual machine configuration.</span></span>
- <span data-ttu-id="e625d-121">Klientprogrammet eller andra uppgifter i jobbet måste leta upp och hämta utdata aktivitetsfiler genom ID eller syfte.</span><span class="sxs-lookup"><span data-stu-id="e625d-121">Your client application or other tasks in the job needs to locate and download task output files by ID or by purpose.</span></span> 
- <span data-ttu-id="e625d-122">Du vill visa uppgiftens utdata i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e625d-122">You want to view task output in the Azure portal.</span></span>

<span data-ttu-id="e625d-123">Om ditt scenario skiljer sig från de som visas ovan, kan du behöva överväga en annan metod.</span><span class="sxs-lookup"><span data-stu-id="e625d-123">If your scenario differs from those listed above, you may need to consider a different approach.</span></span> <span data-ttu-id="e625d-124">Mer information om andra alternativ för bestående uppgiftsutdata finns [spara projekt- och utdata till Azure Storage](batch-task-output.md).</span><span class="sxs-lookup"><span data-stu-id="e625d-124">For more information on other options for persisting task output, see [Persist job and task output to Azure Storage](batch-task-output.md).</span></span> 

## <a name="what-is-the-batch-file-conventions-standard"></a><span data-ttu-id="e625d-125">Vad är standard Batch filen konventioner?</span><span class="sxs-lookup"><span data-stu-id="e625d-125">What is the Batch File Conventions standard?</span></span>

<span data-ttu-id="e625d-126">Den [Batch filen konventioner standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) ger ett namngivningsschema för mål-behållare och blob sökvägar som skrivs utdatafiler.</span><span class="sxs-lookup"><span data-stu-id="e625d-126">The [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) provides a naming scheme for the destination containers and blob paths to which your output files are written.</span></span> <span data-ttu-id="e625d-127">Filer sparas i Azure-lagring som följer standarden filen konventioner automatiskt kan visas i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e625d-127">Files persisted to Azure Storage that adhere to the File Conventions standard are automatically available for viewing in the Azure portal.</span></span> <span data-ttu-id="e625d-128">Portalen så kan visa filer som följer den är medveten om namngivningskonventionen.</span><span class="sxs-lookup"><span data-stu-id="e625d-128">The portal is aware of the naming convention and so can display files that adhere to it.</span></span>

<span data-ttu-id="e625d-129">Filen konventioner-biblioteket för .NET namn automatiskt din behållare för lagring och uppgiften utdatafilerna enligt de konventioner för filen som är standard.</span><span class="sxs-lookup"><span data-stu-id="e625d-129">The File Conventions library for .NET automatically names your storage containers and task output files according to the File Conventions standard.</span></span> <span data-ttu-id="e625d-130">Filen konventioner biblioteket innehåller också metoder för att fråga utdatafilerna i Azure Storage enligt jobb-ID, aktivitets-ID eller syfte.</span><span class="sxs-lookup"><span data-stu-id="e625d-130">The File Conventions library also provides methods to query output files in Azure Storage according to job ID, task ID, or purpose.</span></span>   

<span data-ttu-id="e625d-131">Om du utvecklar med ett annat språk än .NET, kan du implementera filen konventioner standard själv i ditt program.</span><span class="sxs-lookup"><span data-stu-id="e625d-131">If you are developing with a language other than .NET, you can implement the File Conventions standard yourself in your application.</span></span> <span data-ttu-id="e625d-132">Mer information finns i [om det Batch filen konventioner standard](batch-task-output.md#about-the-batch-file-conventions-standard).</span><span class="sxs-lookup"><span data-stu-id="e625d-132">For more information, see [About the Batch File Conventions standard](batch-task-output.md#about-the-batch-file-conventions-standard).</span></span>

## <a name="link-an-azure-storage-account-to-your-batch-account"></a><span data-ttu-id="e625d-133">Koppla ett Azure Storage-konto till Batch-kontot</span><span class="sxs-lookup"><span data-stu-id="e625d-133">Link an Azure Storage account to your Batch account</span></span>

<span data-ttu-id="e625d-134">För att bevara utdata till Azure Storage med hjälp av filen konventioner bibliotek, måste du först koppla ett Azure Storage-konto till Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="e625d-134">To persist output data to Azure Storage using the File Conventions library, you must first link an Azure Storage account to your Batch account.</span></span> <span data-ttu-id="e625d-135">Om du inte redan har gjort länka ett lagringskonto till Batch-kontot med hjälp av den [Azure-portalen](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="e625d-135">If you haven't done so already, link a Storage account to your Batch account by using the [Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="e625d-136">Navigera till ditt Batch-konto i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="e625d-136">Navigate to your Batch account in the Azure portal.</span></span> 
2. <span data-ttu-id="e625d-137">Under **inställningar**väljer **Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="e625d-137">Under **Settings**, select **Storage Account**.</span></span>
3. <span data-ttu-id="e625d-138">Om du inte redan har ett lagringskonto som är associerade med Batch-kontot, klickar du på **Storage-konto (ingen)**.</span><span class="sxs-lookup"><span data-stu-id="e625d-138">If you do not already have a Storage account associated with your Batch account, click **Storage Account (None)**.</span></span>
4. <span data-ttu-id="e625d-139">Välj ett lagringskonto i listan för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e625d-139">Select a Storage account from the list for your subscription.</span></span> <span data-ttu-id="e625d-140">För bästa prestanda använder du ett Azure Storage-konto som är i samma region som Batch-kontot var dina aktiviteter körs.</span><span class="sxs-lookup"><span data-stu-id="e625d-140">For best performance, use an Azure Storage account that is in the same region as the Batch account where your tasks are running.</span></span>

## <a name="persist-output-data"></a><span data-ttu-id="e625d-141">Spara utdata</span><span class="sxs-lookup"><span data-stu-id="e625d-141">Persist output data</span></span>

<span data-ttu-id="e625d-142">Skapa en behållare i Azure Storage för att bevara projekt- och utdata med filen konventioner biblioteket och sedan spara resultatet i behållaren.</span><span class="sxs-lookup"><span data-stu-id="e625d-142">To persist job and task output data with the File Conventions library, create a container in Azure Storage, then save the output to the container.</span></span> <span data-ttu-id="e625d-143">Använd den [Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage) i uppgiftskoden att överföra uppgiftsutdata till behållaren.</span><span class="sxs-lookup"><span data-stu-id="e625d-143">Use the [Azure Storage client library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage) in your task code to upload the task output to the container.</span></span> 

<span data-ttu-id="e625d-144">Mer information om hur du arbetar med behållare och blobbar i Azure Storage finns [komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="e625d-144">For more information about working with containers and blobs in Azure Storage, see [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span>

> [!WARNING]
> <span data-ttu-id="e625d-145">Alla utdata för jobb- och beständiga med filen konventioner biblioteket lagras i samma behållare.</span><span class="sxs-lookup"><span data-stu-id="e625d-145">All job and task outputs persisted with the File Conventions library are stored in the same container.</span></span> <span data-ttu-id="e625d-146">Om ett stort antal aktiviteter som försöker spara filer på samma gång [lagring throttling-begränsning](../storage/common/storage-performance-checklist.md#blobs) eventuellt.</span><span class="sxs-lookup"><span data-stu-id="e625d-146">If a large number of tasks try to persist files at the same time, [storage throttling limits](../storage/common/storage-performance-checklist.md#blobs) may be enforced.</span></span>
> 
> 

### <a name="create-storage-container"></a><span data-ttu-id="e625d-147">Skapa lagringsbehållaren</span><span class="sxs-lookup"><span data-stu-id="e625d-147">Create storage container</span></span>

<span data-ttu-id="e625d-148">För att bevara uppgiftens utdata till Azure Storage först skapa en behållare genom att anropa [CloudJob][net_cloudjob].[ PrepareOutputStorageAsync][net_prepareoutputasync].</span><span class="sxs-lookup"><span data-stu-id="e625d-148">To persist task output to Azure Storage, first create a container by calling [CloudJob][net_cloudjob].[PrepareOutputStorageAsync][net_prepareoutputasync].</span></span> <span data-ttu-id="e625d-149">Den här tilläggsmetoden tar en [CloudStorageAccount] [ net_cloudstorageaccount] objekt som parameter.</span><span class="sxs-lookup"><span data-stu-id="e625d-149">This extension method takes a [CloudStorageAccount][net_cloudstorageaccount] object as a parameter.</span></span> <span data-ttu-id="e625d-150">Den skapar en behållare med namnet enligt filen konventioner standard, så att innehållet inte är kan identifieras av Azure-portalen och hämtning av metoderna som beskrivs senare i artikeln.</span><span class="sxs-lookup"><span data-stu-id="e625d-150">It creates a container named according to the File Conventions standard,so that its contents are discoverable by the Azure portal and the retrieval methods discussed later in the article.</span></span>

<span data-ttu-id="e625d-151">Du vanligtvis placerar kod för att skapa en behållare i ditt klientprogram &mdash; det program som skapar din pooler, jobb och uppgifter.</span><span class="sxs-lookup"><span data-stu-id="e625d-151">You typically place the code to create a container in your client application &mdash; the application that creates your pools, jobs, and tasks.</span></span>

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a><span data-ttu-id="e625d-152">Lagra utdata för aktiviteten</span><span class="sxs-lookup"><span data-stu-id="e625d-152">Store task outputs</span></span>

<span data-ttu-id="e625d-153">Nu när du har skapat en behållare i Azure Storage uppgifter kan spara utdata till behållaren med hjälp av den [TaskOutputStorage] [ net_taskoutputstorage] klass hittades i filen konventioner-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="e625d-153">Now that you've prepared a container in Azure Storage, tasks can save output to the container by using the [TaskOutputStorage][net_taskoutputstorage] class found in the File Conventions library.</span></span>

<span data-ttu-id="e625d-154">I koden aktivitet, först skapa en [TaskOutputStorage] [ net_taskoutputstorage] objekt och sedan anropa när aktiviteten har slutförts sitt arbete, den [TaskOutputStorage] [ net_taskoutputstorage]. [SaveAsync] [ net_saveasync] metod för att spara dess utdata till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="e625d-154">In your task code, first create a [TaskOutputStorage][net_taskoutputstorage] object, then when the task has completed its work, call the [TaskOutputStorage][net_taskoutputstorage].[SaveAsync][net_saveasync] method to save its output to Azure Storage.</span></span>

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

<span data-ttu-id="e625d-155">Den `kind` parameter för den [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[ SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) metoden kategoriserar bevarade filer.</span><span class="sxs-lookup"><span data-stu-id="e625d-155">The `kind` parameter of the [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) method categorizes the persisted files.</span></span> <span data-ttu-id="e625d-156">Det finns fyra fördefinierade [TaskOutputKind] [ net_taskoutputkind] typer: `TaskOutput`, `TaskPreview`, `TaskLog`, och `TaskIntermediate.` du kan också definiera egna kategorier av utdata.</span><span class="sxs-lookup"><span data-stu-id="e625d-156">There are four predefined [TaskOutputKind][net_taskoutputkind] types: `TaskOutput`, `TaskPreview`, `TaskLog`, and `TaskIntermediate.` You can also define custom categories of output.</span></span>

<span data-ttu-id="e625d-157">Dessa utdata-typer kan du ange vilken typ av utdata när du senare fråga Batch för beständiga utdata för en viss uppgift.</span><span class="sxs-lookup"><span data-stu-id="e625d-157">These output types allow you to specify which type of outputs to list when you later query Batch for the persisted outputs of a given task.</span></span> <span data-ttu-id="e625d-158">När du listar utdata för en aktivitet med andra ord kan du filtrera listan på något av följande utdata.</span><span class="sxs-lookup"><span data-stu-id="e625d-158">In other words, when you list the outputs for a task, you can filter the list on one of the output types.</span></span> <span data-ttu-id="e625d-159">Till exempel ”ge mig den *preview* utdata för aktiviteten *109*”.</span><span class="sxs-lookup"><span data-stu-id="e625d-159">For example, "Give me the *preview* output for task *109*."</span></span> <span data-ttu-id="e625d-160">Mer om lista och hämta utdata visas i [hämta utdata](#retrieve-output) senare i artikeln.</span><span class="sxs-lookup"><span data-stu-id="e625d-160">More on listing and retrieving outputs appears in [Retrieve output](#retrieve-output) later in the article.</span></span>

> [!TIP]
> <span data-ttu-id="e625d-161">Vilken utdata avgör också var i Azure-portalen en viss fil visas: *TaskOutput*-kategoriserade filer visas **aktivitet utdatafiler**, och *TaskLog* filer som visas **uppgift loggar**.</span><span class="sxs-lookup"><span data-stu-id="e625d-161">The output kind also determines where in the Azure portal a particular file appears: *TaskOutput*-categorized files appear under **Task output files**, and *TaskLog* files appear under **Task logs**.</span></span>
> 
> 

### <a name="store-job-outputs"></a><span data-ttu-id="e625d-162">Lagra utdata för jobbet</span><span class="sxs-lookup"><span data-stu-id="e625d-162">Store job outputs</span></span>

<span data-ttu-id="e625d-163">Du kan lagra utdata som är associerad med ett hela jobb förutom lagra utdata för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="e625d-163">In addition to storing task outputs, you can store the outputs associated with an entire job.</span></span> <span data-ttu-id="e625d-164">I merge-åtgärd för ett jobb för återgivning av film kan du spara fullständigt återgivna film som en jobbutdata.</span><span class="sxs-lookup"><span data-stu-id="e625d-164">For example, in the merge task of a movie rendering job, you could persist the fully rendered movie as a job output.</span></span> <span data-ttu-id="e625d-165">När jobbet har slutförts ditt klientprogram kan visa och hämta utdata för jobbet, och inte behöver fråga enskilda aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="e625d-165">When your job is completed, your client application can list and retrieve the outputs for the job, and does not need to query the individual tasks.</span></span>

<span data-ttu-id="e625d-166">Lagra jobbutdata genom att anropa den [JobOutputStorage][net_joboutputstorage].[ SaveAsync] [ net_joboutputstorage_saveasync] metod, och ange den [JobOutputKind] [ net_joboutputkind] och filnamn:</span><span class="sxs-lookup"><span data-stu-id="e625d-166">Store job output by calling the [JobOutputStorage][net_joboutputstorage].[SaveAsync][net_joboutputstorage_saveasync] method, and specify the [JobOutputKind][net_joboutputkind] and filename:</span></span>

```csharp
CloudJob job = new JobOutputStorage(acct, jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

<span data-ttu-id="e625d-167">Med den **TaskOutputKind** typen för aktiviteten utdata, använder du den [JobOutputKind] [ net_joboutputkind] typ att kategorisera ett jobb har sparat filer.</span><span class="sxs-lookup"><span data-stu-id="e625d-167">As with the **TaskOutputKind** type for task outputs, you use the [JobOutputKind][net_joboutputkind] type to categorize a job's persisted files.</span></span> <span data-ttu-id="e625d-168">Den här parametern kan du senare fråga (lista) en viss typ av utdata.</span><span class="sxs-lookup"><span data-stu-id="e625d-168">This parameter allows you to later query for (list) a specific type of output.</span></span> <span data-ttu-id="e625d-169">Den **JobOutputKind** typ innehåller både utdata och förhandsgranska kategorier och kan du skapa egna kategorier.</span><span class="sxs-lookup"><span data-stu-id="e625d-169">The **JobOutputKind** type includes both output and preview categories, and supports creating custom categories.</span></span>

### <a name="store-task-logs"></a><span data-ttu-id="e625d-170">Lagrar uppgiften loggar</span><span class="sxs-lookup"><span data-stu-id="e625d-170">Store task logs</span></span>

<span data-ttu-id="e625d-171">Förutom att spara en fil till beständig lagring när en aktivitet eller jobbet har slutförts, du kan behöva spara filer som uppdateras under körningen av en aktivitet &mdash; loggfiler eller `stdout.txt` och `stderr.txt`, till exempel.</span><span class="sxs-lookup"><span data-stu-id="e625d-171">In addition to persisting a file to durable storage when a task or job completes, you may need to persist files that are updated during the execution of a task &mdash; log files or `stdout.txt` and `stderr.txt`, for example.</span></span> <span data-ttu-id="e625d-172">För detta ändamål biblioteket konventioner för Azure Batch-filen innehåller den [TaskOutputStorage][net_taskoutputstorage].[ SaveTrackedAsync] [ net_savetrackedasync] metod.</span><span class="sxs-lookup"><span data-stu-id="e625d-172">For this purpose, the Azure Batch File Conventions library provides the [TaskOutputStorage][net_taskoutputstorage].[SaveTrackedAsync][net_savetrackedasync] method.</span></span> <span data-ttu-id="e625d-173">Med [SaveTrackedAsync][net_savetrackedasync], kan du spåra uppdateringar till en fil på noden (vid ett intervall som du anger) och spara uppdateringarna till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="e625d-173">With [SaveTrackedAsync][net_savetrackedasync], you can track updates to a file on the node (at an interval that you specify) and persist those updates to Azure Storage.</span></span>

<span data-ttu-id="e625d-174">I följande kodavsnitt vi använda [SaveTrackedAsync] [ net_savetrackedasync] att uppdatera `stdout.txt` i Azure Storage var 15: e sekund under körningen av aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="e625d-174">In the following code snippet, we use [SaveTrackedAsync][net_savetrackedasync] to update `stdout.txt` in Azure Storage every 15 seconds during the execution of the task:</span></span>

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
     await Task.Delay(stdoutFlushDelay);
}
```

<span data-ttu-id="e625d-175">Avsnittet kommenterad `Code to process data and produce output file(s)` är en platshållare för den kod som normalt skulle utföra uppgiften.</span><span class="sxs-lookup"><span data-stu-id="e625d-175">The commented section `Code to process data and produce output file(s)` is a placeholder for the code that your task would normally perform.</span></span> <span data-ttu-id="e625d-176">Du kan till exempel ha kod som hämtar data från Azure Storage och utför transformering eller beräkning på den.</span><span class="sxs-lookup"><span data-stu-id="e625d-176">For example, you might have code that downloads data from Azure Storage and performs transformation or calculation on it.</span></span> <span data-ttu-id="e625d-177">Viktig del av den här fragment är visar hur du kan anpassa sådan kod i en `using` block att regelbundet uppdatera en fil med [SaveTrackedAsync][net_savetrackedasync].</span><span class="sxs-lookup"><span data-stu-id="e625d-177">The important part of this snippet is demonstrating how you can wrap such code in a `using` block to periodically update a file with [SaveTrackedAsync][net_savetrackedasync].</span></span>

<span data-ttu-id="e625d-178">Nod-agenten är ett program som körs på varje nod i poolen och ger och kommandokontroll gränssnittet mellan noden och Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e625d-178">The node agent is a program that runs on each node in the pool and provides the command-and-control interface between the node and the Batch service.</span></span> <span data-ttu-id="e625d-179">Den `Task.Delay` anrop måste anges i slutet av det här `using` block så att agenten nod har tid att rensa innehållet i standard out till filen stdout.txt på noden.</span><span class="sxs-lookup"><span data-stu-id="e625d-179">The `Task.Delay` call is required at the end of this `using` block to ensure that the node agent has time to flush the contents of standard out to the stdout.txt file on the node.</span></span> <span data-ttu-id="e625d-180">Utan den här fördröjningen är det möjligt att missa senaste några sekunder för utdata.</span><span class="sxs-lookup"><span data-stu-id="e625d-180">Without this delay, it is possible to miss the last few seconds of output.</span></span> <span data-ttu-id="e625d-181">Den här fördröjningen kan inte krävas för alla filer.</span><span class="sxs-lookup"><span data-stu-id="e625d-181">This delay may not be required for all files.</span></span>

> [!NOTE]
> <span data-ttu-id="e625d-182">När du aktiverar spårning med filen **SaveTrackedAsync**, endast *lägger till* till spårade fil sparas till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="e625d-182">When you enable file tracking with **SaveTrackedAsync**, only *appends* to the tracked file are persisted to Azure Storage.</span></span> <span data-ttu-id="e625d-183">Använd den här metoden för att hålla reda icke rotera loggfiler och andra filer som har skrivits till med tilläggsåtgärder till slutet av filen.</span><span class="sxs-lookup"><span data-stu-id="e625d-183">Use this method only for tracking non-rotating log files or other files that are written to with append operations to the end of the file.</span></span>
> 
> 

## <a name="retrieve-output-data"></a><span data-ttu-id="e625d-184">Hämta utdata</span><span class="sxs-lookup"><span data-stu-id="e625d-184">Retrieve output data</span></span>

<span data-ttu-id="e625d-185">När du hämtar din beständiga utdata med filen konventioner för Azure Batch-biblioteket, gör du det i en aktivitet till och jobbet-central sätt.</span><span class="sxs-lookup"><span data-stu-id="e625d-185">When you retrieve your persisted output using the Azure Batch File Conventions library, you do so in a task- and job-centric manner.</span></span> <span data-ttu-id="e625d-186">Du kan begära utdata för aktiviteten eller jobb utan att behöva känna till dess sökvägen i Azure Storage eller även dess filnamn.</span><span class="sxs-lookup"><span data-stu-id="e625d-186">You can request the output for given task or job without needing to know its path in Azure Storage, or even its file name.</span></span> <span data-ttu-id="e625d-187">Du kan i stället begära utdatafilerna av aktivitet eller jobb-ID.</span><span class="sxs-lookup"><span data-stu-id="e625d-187">Instead, you can request output files by task or job ID.</span></span>

<span data-ttu-id="e625d-188">Följande kodavsnitt går igenom ett jobb uppgifter, skriver ut information om utdatafilerna som för aktiviteten och hämtar sedan dess filer från lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="e625d-188">The following code snippet iterates through a job's tasks, prints some information about the output files for the task, and then downloads its files from Storage.</span></span>

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

## <a name="view-output-files-in-the-azure-portal"></a><span data-ttu-id="e625d-189">Visa utdatafiler i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e625d-189">View output files in the Azure portal</span></span>

<span data-ttu-id="e625d-190">Azure-portalen visar aktivitet utdatafilerna och loggar som sparas till en länkad Azure Storage-konto med hjälp av den [Batch filen konventioner standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions).</span><span class="sxs-lookup"><span data-stu-id="e625d-190">The Azure portal displays task output files and logs that are persisted to a linked Azure Storage account using the [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions).</span></span> <span data-ttu-id="e625d-191">Du kan också implementera dessa konventioner i på ett språk som du väljer, eller om du kan använda filen konventioner biblioteket i .NET-program.</span><span class="sxs-lookup"><span data-stu-id="e625d-191">You can implement these conventions yourself in the a language of your choice, or you can use the File Conventions library in your .NET applications.</span></span>

<span data-ttu-id="e625d-192">Om du vill aktivera visningen av dina utdatafiler i portalen måste du uppfylla följande krav:</span><span class="sxs-lookup"><span data-stu-id="e625d-192">To enable the display of your output files in the portal, you must satisfy the following requirements:</span></span>

1. <span data-ttu-id="e625d-193">[Koppla ett Azure Storage-konto](#requirement-linked-storage-account) till Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="e625d-193">[Link an Azure Storage account](#requirement-linked-storage-account) to your Batch account.</span></span>
2. <span data-ttu-id="e625d-194">Följa de fördefinierade regler för namngivning av behållare för lagring och filer när beständighet utdata.</span><span class="sxs-lookup"><span data-stu-id="e625d-194">Adhere to the predefined naming conventions for Storage containers and files when persisting outputs.</span></span> <span data-ttu-id="e625d-195">Du kan hitta definitionen för dessa regler i filen konventioner biblioteket [viktigt][github_file_conventions_readme].</span><span class="sxs-lookup"><span data-stu-id="e625d-195">You can find the definition of these conventions in the File Conventions library [README][github_file_conventions_readme].</span></span> <span data-ttu-id="e625d-196">Om du använder den [Azure Batch filen konventioner] [ nuget_package] biblioteket för att spara din utdata, dina filer sparas enligt filen konventioner standard.</span><span class="sxs-lookup"><span data-stu-id="e625d-196">If you use the [Azure Batch File Conventions][nuget_package] library to persist your output, your files are persisted according to the File Conventions standard.</span></span>

<span data-ttu-id="e625d-197">Om du vill visa loggar och utgående filer i Azure portal, navigerar du till aktivitet vars utdata som du är intresserad av, klickar sedan på antingen **sparad utdatafilerna** eller **sparade loggarna**.</span><span class="sxs-lookup"><span data-stu-id="e625d-197">To view task output files and logs in the Azure portal, navigate to the task whose output you are interested in, then click either **Saved output files** or **Saved logs**.</span></span> <span data-ttu-id="e625d-198">Den här bilden visar den **sparad utdatafilerna** för uppgiften med ID ”007”:</span><span class="sxs-lookup"><span data-stu-id="e625d-198">This image shows the **Saved output files** for the task with ID "007":</span></span>

<span data-ttu-id="e625d-199">![Resultat av åtgärder bladet i Azure-portalen][2]</span><span class="sxs-lookup"><span data-stu-id="e625d-199">![Task outputs blade in the Azure portal][2]</span></span>

## <a name="code-sample"></a><span data-ttu-id="e625d-200">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="e625d-200">Code sample</span></span>

<span data-ttu-id="e625d-201">Den [PersistOutputs] [ github_persistoutputs] exempelprojektet är en av de [kodexempel för Azure Batch] [ github_samples] på GitHub.</span><span class="sxs-lookup"><span data-stu-id="e625d-201">The [PersistOutputs][github_persistoutputs] sample project is one of the [Azure Batch code samples][github_samples] on GitHub.</span></span> <span data-ttu-id="e625d-202">Visual Studio-lösningen visar hur du använder filen konventioner för Azure Batch-biblioteket för att bevara uppgiftens utdata till beständig lagring.</span><span class="sxs-lookup"><span data-stu-id="e625d-202">This Visual Studio solution demonstrates how to use the Azure Batch File Conventions library to persist task output to durable storage.</span></span> <span data-ttu-id="e625d-203">Följ dessa steg om du vill köra exemplet:</span><span class="sxs-lookup"><span data-stu-id="e625d-203">To run the sample, follow these steps:</span></span>

1. <span data-ttu-id="e625d-204">Öppna projektet i **Visual Studio 2015 eller senare**.</span><span class="sxs-lookup"><span data-stu-id="e625d-204">Open the project in **Visual Studio 2015 or newer**.</span></span>
2. <span data-ttu-id="e625d-205">Lägg till grupp och lagring **kontoautentiseringsuppgifter** till **AccountSettings.settings** i Microsoft.Azure.Batch.Samples.Common-projektet.</span><span class="sxs-lookup"><span data-stu-id="e625d-205">Add your Batch and Storage **account credentials** to **AccountSettings.settings** in the Microsoft.Azure.Batch.Samples.Common project.</span></span>
3. <span data-ttu-id="e625d-206">**Skapa** (men inte kör) lösningen.</span><span class="sxs-lookup"><span data-stu-id="e625d-206">**Build** (but do not run) the solution.</span></span> <span data-ttu-id="e625d-207">Återställa NuGet-paket om du uppmanas till detta.</span><span class="sxs-lookup"><span data-stu-id="e625d-207">Restore any NuGet packages if prompted.</span></span>
4. <span data-ttu-id="e625d-208">Använda Azure-portalen för att överföra en [programpaket](batch-application-packages.md) för **PersistOutputsTask**.</span><span class="sxs-lookup"><span data-stu-id="e625d-208">Use the Azure portal to upload an [application package](batch-application-packages.md) for **PersistOutputsTask**.</span></span> <span data-ttu-id="e625d-209">Inkludera den `PersistOutputsTask.exe` och dess beroende sammansättningar i ZIP-paketet, ange program-ID till ”PersistOutputsTask” och programpaketets version ”1.0”.</span><span class="sxs-lookup"><span data-stu-id="e625d-209">Include the `PersistOutputsTask.exe` and its dependent assemblies in the .zip package, set the application ID to "PersistOutputsTask", and the application package version to "1.0".</span></span>
5. <span data-ttu-id="e625d-210">**Starta** (kör) den **PersistOutputs** projekt.</span><span class="sxs-lookup"><span data-stu-id="e625d-210">**Start** (run) the **PersistOutputs** project.</span></span>
6. <span data-ttu-id="e625d-211">När du uppmanas att välja beständiga teknik användas för att köra exemplet genom att ange **1** att köra exemplet med filen konventioner-biblioteket för att bevara uppgiftens utdata.</span><span class="sxs-lookup"><span data-stu-id="e625d-211">When prompted to choose the persistence technology to use for running the sample, enter **1** to run the sample using the File Conventions library to persist task output.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="e625d-212">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e625d-212">Next steps</span></span>

### <a name="get-the-batch-file-conventions-library-for-net"></a><span data-ttu-id="e625d-213">Hämta filen konventioner för Batch-biblioteket för .NET</span><span class="sxs-lookup"><span data-stu-id="e625d-213">Get the Batch File Conventions library for .NET</span></span>

<span data-ttu-id="e625d-214">Filen konventioner för Batch-biblioteket för .NET är tillgängligt på [NuGet][nuget_package].</span><span class="sxs-lookup"><span data-stu-id="e625d-214">The Batch File Conventions library for .NET is available on [NuGet][nuget_package].</span></span> <span data-ttu-id="e625d-215">Biblioteket utökar den [CloudJob] [ net_cloudjob] och [CloudTask] [ net_cloudtask] klasser med nya metoder.</span><span class="sxs-lookup"><span data-stu-id="e625d-215">The library extends the [CloudJob][net_cloudjob] and [CloudTask][net_cloudtask] classes with new methods.</span></span> <span data-ttu-id="e625d-216">Se även den [refererar dokumentationen](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) för filen konventioner-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="e625d-216">Also see the [reference documentation](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) for the File Conventions library.</span></span>

<span data-ttu-id="e625d-217">Den [källkod] [ github_file_conventions] för fil-konventioner bibliotek är tillgängligt på GitHub i Microsoft Azure SDK för .NET-databasen.</span><span class="sxs-lookup"><span data-stu-id="e625d-217">The [source code][github_file_conventions] for the File Conventions library is available on GitHub in the Microsoft Azure SDK for .NET repository.</span></span> 

### <a name="explore-other-approaches-for-persisting-output-data"></a><span data-ttu-id="e625d-218">Utforska andra metoder för bestående av utdata</span><span class="sxs-lookup"><span data-stu-id="e625d-218">Explore other approaches for persisting output data</span></span>

- <span data-ttu-id="e625d-219">Se [spara projekt- och utdata till Azure Storage](batch-task-output.md) för en översikt över bestående aktivitets- och data.</span><span class="sxs-lookup"><span data-stu-id="e625d-219">See [Persist job and task output to Azure Storage](batch-task-output.md) for an overview of persisting task and job data.</span></span>
- <span data-ttu-id="e625d-220">Se [spara aktivitetsdata till Azure Storage med Batch-tjänsten API](batch-task-output-files.md) information om hur du använder API för Batch-tjänsten för att bevara utdata.</span><span class="sxs-lookup"><span data-stu-id="e625d-220">See [Persist task data to Azure Storage with the Batch service API](batch-task-output-files.md) to learn how to use the Batch service API to persist output data.</span></span>

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
[2]: ./media/batch-task-output/task-output-02.png "Resultat av åtgärder bladet i Azure-portalen"
