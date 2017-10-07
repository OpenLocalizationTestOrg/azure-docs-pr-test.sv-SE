---
title: aaaDevelop Azure Functions med Media Services
description: "Det här avsnittet visar hur toostart utveckla Azure Functions Media Services med hello Azure-portalen."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 51bdcb01-1846-4e1f-bd90-70020ab471b0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/21/2017
ms.author: juliako
ms.openlocfilehash: 3b2c2fb498fea399c862dfbdb63033d06cabf6d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#<a name="develop-azure-functions-with-media-services"></a><span data-ttu-id="33918-103">Utveckla Azure Functions med Media Services</span><span class="sxs-lookup"><span data-stu-id="33918-103">Develop Azure Functions with Media Services</span></span>

<span data-ttu-id="33918-104">Det här avsnittet visar hur tooget igång med att skapa Azure-funktioner som använder Media Services.</span><span class="sxs-lookup"><span data-stu-id="33918-104">This topic shows you how tooget started with creating Azure Functions that use Media Services.</span></span> <span data-ttu-id="33918-105">hello Azure-funktion som definierats i det här avsnittet övervakar en lagringsbehållare konto med namnet **inkommande** för nya MP4-filer.</span><span class="sxs-lookup"><span data-stu-id="33918-105">hello Azure Function defined in this topic monitors a storage account container named **input** for new MP4 files.</span></span> <span data-ttu-id="33918-106">När en fil har släppts i hello lagringsbehållaren körs hello blob-utlösaren hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="33918-106">Once a file is dropped into hello storage container, hello blob trigger will execute hello function.</span></span>

<span data-ttu-id="33918-107">Om du vill tooexplore och distribuera Azure Functions som använder Azure Media Services kolla [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span><span class="sxs-lookup"><span data-stu-id="33918-107">If you want tooexplore and deploy existing Azure Functions that use Azure Media Services, check out [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span></span> <span data-ttu-id="33918-108">Den här lagringsplatsen innehåller exempel som använder Media Services tooshow arbetsflöden relaterade tooingesting innehåll direkt från blob storage-kodning och skriva innehållet tillbaka tooblob lagring.</span><span class="sxs-lookup"><span data-stu-id="33918-108">This repository contains examples that use Media Services tooshow workflows related tooingesting content directly from blob storage, encoding, and writing content back tooblob storage.</span></span> <span data-ttu-id="33918-109">Den innehåller också ett exempel på hur toomonitor jobbet meddelanden via WebHooks och köer i Azure.</span><span class="sxs-lookup"><span data-stu-id="33918-109">It also includes examples of how toomonitor job notifications via WebHooks and Azure Queues.</span></span> <span data-ttu-id="33918-110">Du kan också utveckla dina funktioner baserat på hello exemplen i hello [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) databasen.</span><span class="sxs-lookup"><span data-stu-id="33918-110">You can also develop your Functions based on hello examples in hello [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) repository.</span></span> <span data-ttu-id="33918-111">toodeploy hello funktion, tryck på hello **distribuera tooAzure** knappen.</span><span class="sxs-lookup"><span data-stu-id="33918-111">toodeploy hello functions, press hello **Deploy tooAzure** button.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33918-112">Krav</span><span class="sxs-lookup"><span data-stu-id="33918-112">Prerequisites</span></span>

- <span data-ttu-id="33918-113">Innan du kan skapa din första funktion, behöver du toohave ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="33918-113">Before you can create your first function, you need toohave an active Azure account.</span></span> <span data-ttu-id="33918-114">Om du inte redan har ett Azure-konto, [finns kostnadsfria konton tillgängliga](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="33918-114">If you don't already have an Azure account, [free accounts are available](https://azure.microsoft.com/free/).</span></span>
- <span data-ttu-id="33918-115">Om du ska toocreate Azure Functions som utför åtgärder på ditt konto i Azure Media Services (AMS) eller lyssna tooevents som skickats av Media Services, bör du skapa en AMS-konto som beskrivs [här](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="33918-115">If you are going toocreate Azure Functions that perform actions on your Azure Media Services (AMS) account or listen tooevents sent by Media Services, you should create an AMS account, as described [here](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="33918-116">Förstå [hur toouse Azure functions](../azure-functions/functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="33918-116">Understanding of [how toouse Azure functions](../azure-functions/functions-overview.md).</span></span> <span data-ttu-id="33918-117">Granska även:</span><span class="sxs-lookup"><span data-stu-id="33918-117">Also, review:</span></span>
    - [<span data-ttu-id="33918-118">Azure functions HTTP och webhook bindningar</span><span class="sxs-lookup"><span data-stu-id="33918-118">Azure functions HTTP and webhook bindings</span></span>](../azure-functions/functions-triggers-bindings.md)
    - [<span data-ttu-id="33918-119">Hur tooconfigure Azure-funktion app-inställningar</span><span class="sxs-lookup"><span data-stu-id="33918-119">How tooconfigure Azure Function app settings</span></span>](../azure-functions/functions-how-to-use-azure-function-app-settings.md)
    
## <a name="considerations"></a><span data-ttu-id="33918-120">Överväganden</span><span class="sxs-lookup"><span data-stu-id="33918-120">Considerations</span></span>

-  <span data-ttu-id="33918-121">Azure Functions körs under hello förbrukning plan har 5 minuter timeout begränsa.</span><span class="sxs-lookup"><span data-stu-id="33918-121">Azure Functions running under hello Consumption plan have 5 minutes timeout limit.</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="33918-122">Skapa en funktionsapp</span><span class="sxs-lookup"><span data-stu-id="33918-122">Create a function app</span></span>

1. <span data-ttu-id="33918-123">Gå toohello [Azure-portalen](http://portal.azure.com) och logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="33918-123">Go toohello [Azure portal](http://portal.azure.com) and sign-in with your Azure account.</span></span>
2. <span data-ttu-id="33918-124">Skapa en funktionsapp enligt [här](../azure-functions/functions-create-function-app-portal.md).</span><span class="sxs-lookup"><span data-stu-id="33918-124">Create a function app as described [here](../azure-functions/functions-create-function-app-portal.md).</span></span>

>[!NOTE]
> <span data-ttu-id="33918-125">Ett lagringskonto som du anger i hello **StorageConnection** miljövariabeln (se nästa steg i hello) måste vara i hello samma region som din app.</span><span class="sxs-lookup"><span data-stu-id="33918-125">A storage account that you specify in hello **StorageConnection** environment variable (see hello next step) should be in hello same region as your app.</span></span>

## <a name="configure-function-app-settings"></a><span data-ttu-id="33918-126">Konfigurera funktionen app-inställningar</span><span class="sxs-lookup"><span data-stu-id="33918-126">Configure function app settings</span></span>

<span data-ttu-id="33918-127">När du utvecklar Media Services-funktioner är praktisk tooadd miljövariabler som används i hela din funktioner.</span><span class="sxs-lookup"><span data-stu-id="33918-127">When developing Media Services functions, it is handy tooadd environment variables that will be used throughout your functions.</span></span> <span data-ttu-id="33918-128">tooconfigure appen klickar du på hello konfigurera Appinställningar länk.</span><span class="sxs-lookup"><span data-stu-id="33918-128">tooconfigure app settings, click hello Configure App Settings link.</span></span> <span data-ttu-id="33918-129">Mer information finns i [hur tooconfigure Azure-funktion appinställningar](../azure-functions/functions-how-to-use-azure-function-app-settings.md).</span><span class="sxs-lookup"><span data-stu-id="33918-129">For more information, see  [How tooconfigure Azure Function app settings](../azure-functions/functions-how-to-use-azure-function-app-settings.md).</span></span> 

<span data-ttu-id="33918-130">Exempel:</span><span class="sxs-lookup"><span data-stu-id="33918-130">For example:</span></span>

![Inställningar](./media/media-services-azure-functions/media-services-azure-functions001.png)

<span data-ttu-id="33918-132">hello-funktion som definierats i den här artikeln förutsätter att du har hello följande miljövariabler i app-inställningar:</span><span class="sxs-lookup"><span data-stu-id="33918-132">hello function, defined in this article, assumes you have hello following environment variables in your app settings:</span></span>

<span data-ttu-id="33918-133">**AMSAccount** : *AMS kontonamn* (t.ex. testams)</span><span class="sxs-lookup"><span data-stu-id="33918-133">**AMSAccount** : *AMS account name* (e.g. testams)</span></span>

<span data-ttu-id="33918-134">**AMSKey** : *AMS kontonyckel* (t.ex. IHOySnH + XX3LGPfraE5fKPl0EnzvEPKkOPKCr59aiMM =)</span><span class="sxs-lookup"><span data-stu-id="33918-134">**AMSKey** : *AMS account key* (e.g. IHOySnH+XX3LGPfraE5fKPl0EnzvEPKkOPKCr59aiMM=)</span></span>

<span data-ttu-id="33918-135">**MediaServicesStorageAccountName** : *lagringskontonamnet* (t.ex. testamsstorage)</span><span class="sxs-lookup"><span data-stu-id="33918-135">**MediaServicesStorageAccountName** : *storage account name* (e.g., testamsstorage)</span></span>

<span data-ttu-id="33918-136">**MediaServicesStorageAccountKey** : *lagringskontonyckel* (t.ex. xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXXX ==)</span><span class="sxs-lookup"><span data-stu-id="33918-136">**MediaServicesStorageAccountKey** : *storage account key* (e.g., xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXXX==)</span></span>

<span data-ttu-id="33918-137">**StorageConnection** : *lagringsanslutning* (t.ex. DefaultEndpointsProtocol = https; AccountName = testamsstorage; AccountKey = xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXX ==)</span><span class="sxs-lookup"><span data-stu-id="33918-137">**StorageConnection** : *storage connection* (e.g., DefaultEndpointsProtocol=https;AccountName=testamsstorage;AccountKey=xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXX==)</span></span>

## <a name="create-a-function"></a><span data-ttu-id="33918-138">Skapa en funktion</span><span class="sxs-lookup"><span data-stu-id="33918-138">Create a function</span></span>

<span data-ttu-id="33918-139">När funktionen appen har distribuerats, du kan hitta den bland **Apptjänster** Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="33918-139">Once your function app is deployed, you can find it among **App Services** Azure Functions.</span></span>

1. <span data-ttu-id="33918-140">Välj funktionen appen och klicka på **nya funktionen**.</span><span class="sxs-lookup"><span data-stu-id="33918-140">Select your function app and click **New Function**.</span></span>
2. <span data-ttu-id="33918-141">Välj hello **C#** språk och **databearbetning** scenario.</span><span class="sxs-lookup"><span data-stu-id="33918-141">Choose hello **C#** language and **Data Processing** scenario.</span></span>
3. <span data-ttu-id="33918-142">Välj **BlobTrigger** mall.</span><span class="sxs-lookup"><span data-stu-id="33918-142">Choose **BlobTrigger** template.</span></span> <span data-ttu-id="33918-143">Den här funktionen ska utlösas när en blob har överförts till hello **inkommande** behållare.</span><span class="sxs-lookup"><span data-stu-id="33918-143">This function will be triggered whenever a blob is uploaded into hello **input** container.</span></span> <span data-ttu-id="33918-144">Hej **inkommande** namn har angetts i hello **sökväg**, i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="33918-144">hello **input** name is specified in hello **Path**, in hello next step.</span></span>

    ![Filer](./media/media-services-azure-functions/media-services-azure-functions004.png)

4. <span data-ttu-id="33918-146">När du har valt **BlobTrigger**, några fler kontroller ska visas på hello.</span><span class="sxs-lookup"><span data-stu-id="33918-146">Once you select **BlobTrigger**, some more controls will appear on hello page.</span></span>

    ![Filer](./media/media-services-azure-functions/media-services-azure-functions005.png)

4. <span data-ttu-id="33918-148">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="33918-148">Click **Create**.</span></span> 


## <a name="files"></a><span data-ttu-id="33918-149">Filer</span><span class="sxs-lookup"><span data-stu-id="33918-149">Files</span></span>

<span data-ttu-id="33918-150">Din Azure-funktion är associerad med kod och andra filer som beskrivs i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="33918-150">Your Azure function is associated with code files and other files that are described in this section.</span></span> <span data-ttu-id="33918-151">Som standard, en funktion som är associerad med **function.json** och **run.csx** (C#)-filer.</span><span class="sxs-lookup"><span data-stu-id="33918-151">By default, a function is associated with **function.json** and **run.csx** (C#) files.</span></span> <span data-ttu-id="33918-152">Du behöver tooadd en **project.json** fil.</span><span class="sxs-lookup"><span data-stu-id="33918-152">You will need tooadd a **project.json** file.</span></span> <span data-ttu-id="33918-153">hello resten av det här avsnittet visar hello definitioner för de här filerna.</span><span class="sxs-lookup"><span data-stu-id="33918-153">hello rest of this section shows hello definitions for these files.</span></span>

![Filer](./media/media-services-azure-functions/media-services-azure-functions003.png)

### <a name="functionjson"></a><span data-ttu-id="33918-155">Function.JSON</span><span class="sxs-lookup"><span data-stu-id="33918-155">function.json</span></span>

<span data-ttu-id="33918-156">Hej function.json filen definierar hello funktionsbindningar och andra konfigurationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="33918-156">hello function.json file defines hello function bindings and other configuration settings.</span></span> <span data-ttu-id="33918-157">hello runtime använder den här filen toodetermine hello händelser toomonitor och hur toopass data till och returnera data från att fungera körning.</span><span class="sxs-lookup"><span data-stu-id="33918-157">hello runtime uses this file toodetermine hello events toomonitor and how toopass data into and return data from function execution.</span></span> <span data-ttu-id="33918-158">Mer information finns i [Azure functions HTTP och webhook bindningar](../azure-functions/functions-reference.md#function-code).</span><span class="sxs-lookup"><span data-stu-id="33918-158">For more information, see [Azure functions HTTP and webhook bindings](../azure-functions/functions-reference.md#function-code).</span></span>

>[!NOTE]
><span data-ttu-id="33918-159">Ange hello **inaktiverad** egenskapen för**SANT** tooprevent hello funktion från utförs.</span><span class="sxs-lookup"><span data-stu-id="33918-159">Set hello **disabled** property too**true** tooprevent hello function from being executed.</span></span> 


<span data-ttu-id="33918-160">Här är ett exempel på **function.json** fil.</span><span class="sxs-lookup"><span data-stu-id="33918-160">Here is an example of **function.json** file.</span></span>

    {
    "bindings": [
      {
        "name": "myBlob",
        "type": "blobTrigger",
        "direction": "in",
        "path": "input/{fileName}.mp4",
        "connection": "StorageConnection"
      }
    ],
    "disabled": false
    }

### <a name="projectjson"></a><span data-ttu-id="33918-161">Project.JSON</span><span class="sxs-lookup"><span data-stu-id="33918-161">project.json</span></span>

<span data-ttu-id="33918-162">Hej project.json filen innehåller beroenden.</span><span class="sxs-lookup"><span data-stu-id="33918-162">hello project.json file contains dependencies.</span></span> <span data-ttu-id="33918-163">Här är ett exempel på **project.json** fil som innehåller hello krävs .NET Azure Media Services-paket från Nuget.</span><span class="sxs-lookup"><span data-stu-id="33918-163">Here is an example of **project.json** file that includes hello required .NET Azure Media Services packages from Nuget.</span></span> <span data-ttu-id="33918-164">Observera att hello versionsnummer ändras med de senaste uppdateringarna toohello paket, så du måste bekräfta hello senaste versionerna.</span><span class="sxs-lookup"><span data-stu-id="33918-164">Note that hello version numbers will change with latest updates toohello packages, so you should confirm hello most recent versions.</span></span> 

    {
      "frameworks": {
        "net46":{
          "dependencies": {
        "windowsazure.mediaservices": "3.8.0.5",
        "windowsazure.mediaservices.extensions": "3.8.0.3"
          }
        }
       }
    }
    
### <a name="runcsx"></a><span data-ttu-id="33918-165">Run.csx</span><span class="sxs-lookup"><span data-stu-id="33918-165">run.csx</span></span>

<span data-ttu-id="33918-166">Det här är för din funktion hello C#-kod.</span><span class="sxs-lookup"><span data-stu-id="33918-166">This is hello C# code for your function.</span></span>  <span data-ttu-id="33918-167">hello-funktionen som anges nedan Övervakare en lagringsbehållare konto med namnet **inkommande** (som är vad som har angetts i hello sökväg) för nya MP4-filer.</span><span class="sxs-lookup"><span data-stu-id="33918-167">hello function defined below monitors a storage account container named **input** (that is what was specified in hello path) for new MP4 files.</span></span> <span data-ttu-id="33918-168">När en fil har släppts i hello lagringsbehållaren körs hello blob-utlösaren hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="33918-168">Once a file is dropped into hello storage container, hello blob trigger will execute hello function.</span></span>
    
<span data-ttu-id="33918-169">hello exemplet definieras i detta avsnitt visar</span><span class="sxs-lookup"><span data-stu-id="33918-169">hello example defined in this section demonstrates</span></span> 

1. <span data-ttu-id="33918-170">hur tooingest en tillgång till ett Media Services-konto (genom att kopiera en blobb till en tillgång AMS) och</span><span class="sxs-lookup"><span data-stu-id="33918-170">how tooingest an asset into a Media Services account (by coping a blob into an AMS asset) and</span></span> 
2. <span data-ttu-id="33918-171">hur toosubmit ett kodningsjobb som använder Media Encoder Standard ”anpassningsbar strömning” förinställda.</span><span class="sxs-lookup"><span data-stu-id="33918-171">how toosubmit an encoding job that uses Media Encoder Standard's "Adaptive Streaming" preset .</span></span>

<span data-ttu-id="33918-172">I hello verkligheten scenariot du förmodligen vill tootrack jobb pågår och sedan publicera den kodade tillgången.</span><span class="sxs-lookup"><span data-stu-id="33918-172">In hello real life scenario, you most likely want tootrack job progress and then publish your encoded asset.</span></span> <span data-ttu-id="33918-173">Mer information finns i [Använd Azure WebHooks toomonitor Media Services jobbet meddelanden](media-services-dotnet-check-job-progress-with-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="33918-173">For more information, see [Use Azure WebHooks toomonitor Media Services job notifications](media-services-dotnet-check-job-progress-with-webhooks.md).</span></span> <span data-ttu-id="33918-174">Fler exempel finns [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span><span class="sxs-lookup"><span data-stu-id="33918-174">For more examples, see [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span></span>  

<span data-ttu-id="33918-175">När du är klar definiera din funktion klickar du på **spara och kör**.</span><span class="sxs-lookup"><span data-stu-id="33918-175">Once you are done defining your function click **Save and Run**.</span></span>

    #r "Microsoft.WindowsAzure.Storage"
    #r "Newtonsoft.Json"
    #r "System.Web"

    using System;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Net;
    using System.Threading;
    using System.Threading.Tasks;
    using System.IO;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage.Auth;

    private static readonly string _mediaServicesAccountName = Environment.GetEnvironmentVariable("AMSAccount");
    private static readonly string _mediaServicesAccountKey = Environment.GetEnvironmentVariable("AMSKey");

    static string _storageAccountName = Environment.GetEnvironmentVariable("MediaServicesStorageAccountName");
    static string _storageAccountKey = Environment.GetEnvironmentVariable("MediaServicesStorageAccountKey");

    private static CloudStorageAccount _destinationStorageAccount = null;

    // Field for service context.
    private static CloudMediaContext _context = null;
    private static MediaServicesCredentials _cachedCredentials = null;

    public static void Run(CloudBlockBlob myBlob, string fileName, TraceWriter log)
    {
        // NOTE that hello variables {fileName} here come from hello path setting in function.json
        // and are passed into hello  Run method signature above. We can use this toomake decisions on what type of file
        // was dropped into hello input container for hello function. 

        // No need toodo any Retry strategy in this function, By default, hello SDK calls a function up too5 times for a 
        // given blob. If hello fifth try fails, hello SDK adds a message tooa queue named webjobs-blobtrigger-poison.

        log.Info($"C# Blob trigger function processed: {fileName}.mp4");
        log.Info($"Using Azure Media Services account : {_mediaServicesAccountName}");


        try
        {
        // Create and cache hello Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(
                _mediaServicesAccountName,
                _mediaServicesAccountKey);

        // Used hello chached credentials toocreate CloudMediaContext.
        _context = new CloudMediaContext(_cachedCredentials);

        // Step 1:  Copy hello Blob into a new Input Asset for hello Job
        // ***NOTE: Ideally we would have a method tooingest a Blob directly here somehow. 
        // using code from this sample - https://azure.microsoft.com/en-us/documentation/articles/media-services-copying-existing-blob/

        StorageCredentials mediaServicesStorageCredentials =
            new StorageCredentials(_storageAccountName, _storageAccountKey);

        IAsset newAsset = CreateAssetFromBlob(myBlob, fileName, log).GetAwaiter().GetResult();

        // Step 2: Create an Encoding Job

        // Declare a new encoding job with hello Standard encoder
        IJob job = _context.Jobs.Create("Azure Function - MES Job");

        // Get a media processor reference, and pass tooit hello name of hello 
        // processor toouse for hello specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Create a task with hello encoding details, using a custom preset
        ITask task = job.Tasks.AddNew("Encode with Adaptive Streaming",
            processor,
            "Adaptive Streaming",
            TaskOptions.None); 

        // Specify hello input asset toobe encoded.
        task.InputAssets.Add(newAsset);

        // Add an output asset toocontain hello results of hello job. 
        // This output is specified as AssetCreationOptions.None, which 
        // means hello output asset is not encrypted. 
        task.OutputAssets.AddNew(fileName, AssetCreationOptions.None);

        job.Submit();
        log.Info("Job Submitted");

        }
        catch (Exception ex)
        {
        log.Error("ERROR: failed.");
        log.Info($"StackTrace : {ex.StackTrace}");
        throw ex;
        }
    }

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


    public static async Task<IAsset> CreateAssetFromBlob(CloudBlockBlob blob, string assetName, TraceWriter log){
        IAsset newAsset = null;

        try{
            Task<IAsset> copyAssetTask = CreateAssetFromBlobAsync(blob, assetName, log);
            newAsset = await copyAssetTask;
            log.Info($"Asset Copied : {newAsset.Id}");
        }
        catch(Exception ex){
            log.Info("Copy Failed");
            log.Info($"ERROR : {ex.Message}");
            throw ex;
        }

        return newAsset;
    }

    /// <summary>
    /// Creates a new asset and copies blobs from hello specifed storage account.
    /// </summary>
    /// <param name="blob">hello specified blob.</param>
    /// <returns>hello new asset.</returns>
    public static async Task<IAsset> CreateAssetFromBlobAsync(CloudBlockBlob blob, string assetName, TraceWriter log)
    {
         //Get a reference toohello storage account that is associated with hello Media Services account. 
        StorageCredentials mediaServicesStorageCredentials =
        new StorageCredentials(_storageAccountName, _storageAccountKey);
        _destinationStorageAccount = new CloudStorageAccount(mediaServicesStorageCredentials, false);

        // Create a new asset. 
        var asset = _context.Assets.Create(blob.Name, AssetCreationOptions.None);
        log.Info($"Created new asset {asset.Name}");

        IAccessPolicy writePolicy = _context.AccessPolicies.Create("writePolicy",
        TimeSpan.FromHours(4), AccessPermissions.Write);
        ILocator destinationLocator = _context.Locators.CreateLocator(LocatorType.Sas, asset, writePolicy);
        CloudBlobClient destBlobStorage = _destinationStorageAccount.CreateCloudBlobClient();

        // Get hello destination asset container reference
        string destinationContainerName = (new Uri(destinationLocator.Path)).Segments[1];
        CloudBlobContainer assetContainer = destBlobStorage.GetContainerReference(destinationContainerName);

        try{
        assetContainer.CreateIfNotExists();
        }
        catch (Exception ex)
        {
        log.Error ("ERROR:" + ex.Message);
        }

        log.Info("Created asset.");

        // Get hold of hello destination blob
        CloudBlockBlob destinationBlob = assetContainer.GetBlockBlobReference(blob.Name);

        // Copy Blob
        try
        {
        using (var stream = await blob.OpenReadAsync()) 
        {            
            await destinationBlob.UploadFromStreamAsync(stream);          
        }

        log.Info("Copy Complete.");

        var assetFile = asset.AssetFiles.Create(blob.Name);
        assetFile.ContentFileSize = blob.Properties.Length;
        assetFile.IsPrimary = true;
        assetFile.Update();
        asset.Update();
        }
        catch (Exception ex)
        {
        log.Error(ex.Message);
        log.Info (ex.StackTrace);
        log.Info ("Copy Failed.");
        throw;
        }

        destinationLocator.Delete();
        writePolicy.Delete();

        return asset;
    }
##<a name="test-your-function"></a><span data-ttu-id="33918-176">Testa din funktion</span><span class="sxs-lookup"><span data-stu-id="33918-176">Test your function</span></span>

<span data-ttu-id="33918-177">tootest din funktion behöver du en MP4-fil till hello tooupload **inkommande** behållare för hello storage-konto som du angav i hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="33918-177">tootest your function, you need tooupload an MP4 file into hello **input** container of hello storage account that you specified in hello connection string.</span></span>  

## <a name="next-step"></a><span data-ttu-id="33918-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="33918-178">Next step</span></span>

<span data-ttu-id="33918-179">Nu är du redo toostart utvecklingen av ett Media Services-program.</span><span class="sxs-lookup"><span data-stu-id="33918-179">At this point, you are ready toostart developing a Media Services application.</span></span> 
 
<span data-ttu-id="33918-180">Mer information och fullständiga prover/lösningar för att använda Azure Functions och Logic Apps med Azure Media Services toocreate skapande av anpassade innehåll arbetsflöden finns hello [Media Services .NET funktioner Integraiton exempel på GitHub](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)</span><span class="sxs-lookup"><span data-stu-id="33918-180">For more details and complete samples/solutions of using Azure Functions and Logic Apps with Azure Media Services toocreate custom content creation workflows, see hello [Media Services .NET Functions Integraiton Sample on GitHub](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)</span></span>

<span data-ttu-id="33918-181">Se även [Använd Azure WebHooks toomonitor Media Services jobbet meddelanden med .NET](media-services-dotnet-check-job-progress-with-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="33918-181">Also, see [Use Azure WebHooks toomonitor Media Services job notifications with .NET](media-services-dotnet-check-job-progress-with-webhooks.md).</span></span> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="33918-182">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="33918-182">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="33918-183">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="33918-183">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

