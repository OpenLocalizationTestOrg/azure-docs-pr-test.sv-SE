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
#<a name="develop-azure-functions-with-media-services"></a>Utveckla Azure Functions med Media Services

Det här avsnittet visar hur tooget igång med att skapa Azure-funktioner som använder Media Services. hello Azure-funktion som definierats i det här avsnittet övervakar en lagringsbehållare konto med namnet **inkommande** för nya MP4-filer. När en fil har släppts i hello lagringsbehållaren körs hello blob-utlösaren hello-funktionen.

Om du vill tooexplore och distribuera Azure Functions som använder Azure Media Services kolla [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration). Den här lagringsplatsen innehåller exempel som använder Media Services tooshow arbetsflöden relaterade tooingesting innehåll direkt från blob storage-kodning och skriva innehållet tillbaka tooblob lagring. Den innehåller också ett exempel på hur toomonitor jobbet meddelanden via WebHooks och köer i Azure. Du kan också utveckla dina funktioner baserat på hello exemplen i hello [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) databasen. toodeploy hello funktion, tryck på hello **distribuera tooAzure** knappen.

## <a name="prerequisites"></a>Krav

- Innan du kan skapa din första funktion, behöver du toohave ett aktivt Azure-konto. Om du inte redan har ett Azure-konto, [finns kostnadsfria konton tillgängliga](https://azure.microsoft.com/free/).
- Om du ska toocreate Azure Functions som utför åtgärder på ditt konto i Azure Media Services (AMS) eller lyssna tooevents som skickats av Media Services, bör du skapa en AMS-konto som beskrivs [här](media-services-portal-create-account.md).
- Förstå [hur toouse Azure functions](../azure-functions/functions-overview.md). Granska även:
    - [Azure functions HTTP och webhook bindningar](../azure-functions/functions-triggers-bindings.md)
    - [Hur tooconfigure Azure-funktion app-inställningar](../azure-functions/functions-how-to-use-azure-function-app-settings.md)
    
## <a name="considerations"></a>Överväganden

-  Azure Functions körs under hello förbrukning plan har 5 minuter timeout begränsa.

## <a name="create-a-function-app"></a>Skapa en funktionsapp

1. Gå toohello [Azure-portalen](http://portal.azure.com) och logga in med ditt Azure-konto.
2. Skapa en funktionsapp enligt [här](../azure-functions/functions-create-function-app-portal.md).

>[!NOTE]
> Ett lagringskonto som du anger i hello **StorageConnection** miljövariabeln (se nästa steg i hello) måste vara i hello samma region som din app.

## <a name="configure-function-app-settings"></a>Konfigurera funktionen app-inställningar

När du utvecklar Media Services-funktioner är praktisk tooadd miljövariabler som används i hela din funktioner. tooconfigure appen klickar du på hello konfigurera Appinställningar länk. Mer information finns i [hur tooconfigure Azure-funktion appinställningar](../azure-functions/functions-how-to-use-azure-function-app-settings.md). 

Exempel:

![Inställningar](./media/media-services-azure-functions/media-services-azure-functions001.png)

hello-funktion som definierats i den här artikeln förutsätter att du har hello följande miljövariabler i app-inställningar:

**AMSAccount** : *AMS kontonamn* (t.ex. testams)

**AMSKey** : *AMS kontonyckel* (t.ex. IHOySnH + XX3LGPfraE5fKPl0EnzvEPKkOPKCr59aiMM =)

**MediaServicesStorageAccountName** : *lagringskontonamnet* (t.ex. testamsstorage)

**MediaServicesStorageAccountKey** : *lagringskontonyckel* (t.ex. xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXXX ==)

**StorageConnection** : *lagringsanslutning* (t.ex. DefaultEndpointsProtocol = https; AccountName = testamsstorage; AccountKey = xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXX ==)

## <a name="create-a-function"></a>Skapa en funktion

När funktionen appen har distribuerats, du kan hitta den bland **Apptjänster** Azure Functions.

1. Välj funktionen appen och klicka på **nya funktionen**.
2. Välj hello **C#** språk och **databearbetning** scenario.
3. Välj **BlobTrigger** mall. Den här funktionen ska utlösas när en blob har överförts till hello **inkommande** behållare. Hej **inkommande** namn har angetts i hello **sökväg**, i hello nästa steg.

    ![Filer](./media/media-services-azure-functions/media-services-azure-functions004.png)

4. När du har valt **BlobTrigger**, några fler kontroller ska visas på hello.

    ![Filer](./media/media-services-azure-functions/media-services-azure-functions005.png)

4. Klicka på **Skapa**. 


## <a name="files"></a>Filer

Din Azure-funktion är associerad med kod och andra filer som beskrivs i det här avsnittet. Som standard, en funktion som är associerad med **function.json** och **run.csx** (C#)-filer. Du behöver tooadd en **project.json** fil. hello resten av det här avsnittet visar hello definitioner för de här filerna.

![Filer](./media/media-services-azure-functions/media-services-azure-functions003.png)

### <a name="functionjson"></a>Function.JSON

Hej function.json filen definierar hello funktionsbindningar och andra konfigurationsinställningar. hello runtime använder den här filen toodetermine hello händelser toomonitor och hur toopass data till och returnera data från att fungera körning. Mer information finns i [Azure functions HTTP och webhook bindningar](../azure-functions/functions-reference.md#function-code).

>[!NOTE]
>Ange hello **inaktiverad** egenskapen för**SANT** tooprevent hello funktion från utförs. 


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

Hej project.json filen innehåller beroenden. Här är ett exempel på **project.json** fil som innehåller hello krävs .NET Azure Media Services-paket från Nuget. Observera att hello versionsnummer ändras med de senaste uppdateringarna toohello paket, så du måste bekräfta hello senaste versionerna. 

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

Det här är för din funktion hello C#-kod.  hello-funktionen som anges nedan Övervakare en lagringsbehållare konto med namnet **inkommande** (som är vad som har angetts i hello sökväg) för nya MP4-filer. När en fil har släppts i hello lagringsbehållaren körs hello blob-utlösaren hello-funktionen.
    
hello exemplet definieras i detta avsnitt visar 

1. hur tooingest en tillgång till ett Media Services-konto (genom att kopiera en blobb till en tillgång AMS) och 
2. hur toosubmit ett kodningsjobb som använder Media Encoder Standard ”anpassningsbar strömning” förinställda.

I hello verkligheten scenariot du förmodligen vill tootrack jobb pågår och sedan publicera den kodade tillgången. Mer information finns i [Använd Azure WebHooks toomonitor Media Services jobbet meddelanden](media-services-dotnet-check-job-progress-with-webhooks.md). Fler exempel finns [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).  

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
##<a name="test-your-function"></a>Testa din funktion

tootest din funktion behöver du en MP4-fil till hello tooupload **inkommande** behållare för hello storage-konto som du angav i hello anslutningssträngen.  

## <a name="next-step"></a>Nästa steg

Nu är du redo toostart utvecklingen av ett Media Services-program. 
 
Mer information och fullständiga prover/lösningar för att använda Azure Functions och Logic Apps med Azure Media Services toocreate skapande av anpassade innehåll arbetsflöden finns hello [Media Services .NET funktioner Integraiton exempel på GitHub](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)

Se även [Använd Azure WebHooks toomonitor Media Services jobbet meddelanden med .NET](media-services-dotnet-check-job-progress-with-webhooks.md). 

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

