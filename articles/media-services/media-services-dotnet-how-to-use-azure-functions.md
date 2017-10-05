---
title: Utveckla Azure Functions med Media Services
description: "Det här avsnittet visar hur du börja utveckla Azure Functions med Media Services med Azure-portalen."
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
ms.openlocfilehash: 35d539855572fef6c00de614a4e57738a8abd075
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
#<a name="develop-azure-functions-with-media-services"></a>Utveckla Azure Functions med Media Services

Det här avsnittet visar hur du kommer igång med att skapa Azure-funktioner som använder Media Services. Azure-funktion som definierats i det här avsnittet övervakar en lagringsbehållare konto med namnet **inkommande** för nya MP4-filer. När en fil har släppts till lagringsbehållare, körs blob-utlösaren funktionen.

Om du vill utforska och distribuera Azure Functions som använder Azure Media Services kolla [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration). Den här lagringsplatsen innehåller exempel som använder Media Services för att visa arbetsflöden som rör vill föra in innehåll direkt från blob storage-kodning och skriva innehållet tillbaka till blob storage. Den innehåller också ett exempel på hur du övervakar jobbet meddelanden via WebHooks och köer i Azure. Du kan också utveckla dina funktioner baserat på exemplen i den [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) databasen. Om du vill distribuera funktionerna, trycker du på den **till Azure** knappen.

## <a name="prerequisites"></a>Krav

- Innan du kan skapa din första funktion måste du ha ett aktivt Azure-konto. Om du inte redan har ett Azure-konto, [finns kostnadsfria konton tillgängliga](https://azure.microsoft.com/free/).
- Om du ska skapa Azure-funktioner som utför åtgärder på ditt konto i Azure Media Services (AMS) eller lyssna på händelser som skickats av Media Services, bör du skapa en AMS-konto som beskrivs [här](media-services-portal-create-account.md).
- Förstå [hur du använder Azure functions](../azure-functions/functions-overview.md). Granska även:
    - [Azure functions HTTP och webhook bindningar](../azure-functions/functions-triggers-bindings.md)
    - [Så här konfigurerar du Azure-funktion app-inställningar](../azure-functions/functions-how-to-use-azure-function-app-settings.md)
    
## <a name="considerations"></a>Överväganden

-  Azure Functions körs under förbrukning planen har 5 minuter timeout begränsa.

## <a name="create-a-function-app"></a>Skapa en funktionsapp

1. Gå till [Azure Portal](http://portal.azure.com) och logga in med ditt Azure-konto.
2. Skapa en funktionsapp enligt [här](../azure-functions/functions-create-function-app-portal.md).

>[!NOTE]
> Ett lagringskonto som du anger i den **StorageConnection** miljövariabeln (se nästa steg) måste vara i samma region som din app.

## <a name="configure-function-app-settings"></a>Konfigurera funktionen app-inställningar

Det är praktiskt att lägga till miljövariabler som används i hela din funktioner när du utvecklar Media Services-funktioner. Klicka på länken konfigurera App-inställningar om du vill konfigurera inställningar för appen. Mer information finns i [hur du konfigurerar inställningar för Azure-funktion app](../azure-functions/functions-how-to-use-azure-function-app-settings.md). 

Exempel:

![Inställningar](./media/media-services-azure-functions/media-services-azure-functions001.png)

Funktionen som definieras i den här artikeln förutsätter att du har följande miljövariabler i app-inställningar:

**AMSAccount** : *AMS kontonamn* (t.ex. testams)

**AMSKey** : *AMS kontonyckel* (t.ex. IHOySnH + XX3LGPfraE5fKPl0EnzvEPKkOPKCr59aiMM =)

**MediaServicesStorageAccountName** : *lagringskontonamnet* (t.ex. testamsstorage)

**MediaServicesStorageAccountKey** : *lagringskontonyckel* (t.ex. xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXXX ==)

**StorageConnection** : *lagringsanslutning* (t.ex. DefaultEndpointsProtocol = https; AccountName = testamsstorage; AccountKey = xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXX ==)

## <a name="create-a-function"></a>Skapa en funktion

När funktionen appen har distribuerats, du kan hitta den bland **Apptjänster** Azure Functions.

1. Välj funktionen appen och klicka på **nya funktionen**.
2. Välj den **C#** språk och **databearbetning** scenario.
3. Välj **BlobTrigger** mall. Den här funktionen ska utlösas när en blob har överförts till den **inkommande** behållare. Den **inkommande** namn har angetts i den **sökväg**, i nästa steg.

    ![Filer](./media/media-services-azure-functions/media-services-azure-functions004.png)

4. När du har valt **BlobTrigger**, några fler kontroller visas på sidan.

    ![Filer](./media/media-services-azure-functions/media-services-azure-functions005.png)

4. Klicka på **Skapa**. 


## <a name="files"></a>Filer

Din Azure-funktion är associerad med kod och andra filer som beskrivs i det här avsnittet. Som standard, en funktion som är associerad med **function.json** och **run.csx** (C#)-filer. Du måste lägga till en **project.json** fil. Resten av det här avsnittet innehåller definitioner för de här filerna.

![Filer](./media/media-services-azure-functions/media-services-azure-functions003.png)

### <a name="functionjson"></a>Function.JSON

Filen function.json definierar bindningarna som funktionen och andra konfigurationsinställningar. Körningsmiljön använder den här filen för att fastställa händelser att övervaka och hur du överför data till och returnera data från funktionen körning. Mer information finns i [Azure functions HTTP och webhook bindningar](../azure-functions/functions-reference.md#function-code).

>[!NOTE]
>Ange den **inaktiverad** egenskapen **SANT** att förhindra att funktionen utförs. 


Här är ett exempel på **function.json** fil.

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

### <a name="projectjson"></a>Project.JSON

Filen project.json innehåller beroenden. Här är ett exempel på **project.json** fil som innehåller de nödvändiga .NET Azure Media Services-paketen från Nuget. Observera att versionsnumren ändras med de senaste uppdateringarna till paket, så att du bekräftar du de senaste versionerna. 

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
    
### <a name="runcsx"></a>Run.csx

Detta är C#-koden för din funktion.  Funktionen som anges nedan Övervakare en lagringsbehållare konto med namnet **inkommande** (som är vad som har angetts i sökvägen) för nya MP4-filer. När en fil har släppts till lagringsbehållare, körs blob-utlösaren funktionen.
    
Exemplet definieras i detta avsnitt visar 

1. hur att mata in en tillgång till ett Media Services-konto (genom att kopiera en blobb till en tillgång AMS) och 
2. hur man skickar ett kodningsjobb som använder Media Encoder Standard förinställda ”anpassningsbar strömning”.

I verkligheten-scenario som du förmodligen vill spåra jobbförloppet och sedan publicera den kodade tillgången. Mer information finns i [Använd Azure WebHooks att övervaka Media Services jobbet meddelanden](media-services-dotnet-check-job-progress-with-webhooks.md). Fler exempel finns [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).  

När du är klar definiera din funktion klickar du på **spara och kör**.

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
        // NOTE that the variables {fileName} here come from the path setting in function.json
        // and are passed into the  Run method signature above. We can use this to make decisions on what type of file
        // was dropped into the input container for the function. 

        // No need to do any Retry strategy in this function, By default, the SDK calls a function up to 5 times for a 
        // given blob. If the fifth try fails, the SDK adds a message to a queue named webjobs-blobtrigger-poison.

        log.Info($"C# Blob trigger function processed: {fileName}.mp4");
        log.Info($"Using Azure Media Services account : {_mediaServicesAccountName}");


        try
        {
        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(
                _mediaServicesAccountName,
                _mediaServicesAccountKey);

        // Used the chached credentials to create CloudMediaContext.
        _context = new CloudMediaContext(_cachedCredentials);

        // Step 1:  Copy the Blob into a new Input Asset for the Job
        // ***NOTE: Ideally we would have a method to ingest a Blob directly here somehow. 
        // using code from this sample - https://azure.microsoft.com/en-us/documentation/articles/media-services-copying-existing-blob/

        StorageCredentials mediaServicesStorageCredentials =
            new StorageCredentials(_storageAccountName, _storageAccountKey);

        IAsset newAsset = CreateAssetFromBlob(myBlob, fileName, log).GetAwaiter().GetResult();

        // Step 2: Create an Encoding Job

        // Declare a new encoding job with the Standard encoder
        IJob job = _context.Jobs.Create("Azure Function - MES Job");

        // Get a media processor reference, and pass to it the name of the 
        // processor to use for the specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Create a task with the encoding details, using a custom preset
        ITask task = job.Tasks.AddNew("Encode with Adaptive Streaming",
            processor,
            "Adaptive Streaming",
            TaskOptions.None); 

        // Specify the input asset to be encoded.
        task.InputAssets.Add(newAsset);

        // Add an output asset to contain the results of the job. 
        // This output is specified as AssetCreationOptions.None, which 
        // means the output asset is not encrypted. 
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
    /// Creates a new asset and copies blobs from the specifed storage account.
    /// </summary>
    /// <param name="blob">The specified blob.</param>
    /// <returns>The new asset.</returns>
    public static async Task<IAsset> CreateAssetFromBlobAsync(CloudBlockBlob blob, string assetName, TraceWriter log)
    {
         //Get a reference to the storage account that is associated with the Media Services account. 
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

        // Get the destination asset container reference
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

        // Get hold of the destination blob
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
##<a name="test-your-function"></a>Testa din funktion

Om du vill testa funktionen måste du ladda upp en MP4-fil i den **inkommande** behållare för det lagringskonto som du angav i anslutningssträngen.  

## <a name="next-step"></a>Nästa steg

Du är nu redo att börja utveckla ett Media Services-program. 
 
Mer information och fullständiga prover/lösningar med Azure Functions och Logic Apps med Azure Media Services för att skapa en anpassad innehåll arbetsflöden finns i [Media Services .NET funktioner Integraiton exempel på GitHub](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)

Se även [Använd Azure WebHooks att övervaka Media Services jobbet meddelanden med .NET](media-services-dotnet-check-job-progress-with-webhooks.md). 

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

