---
title: "aaaUpload filer till ett Media Services-konto med hjälp av .NET | Microsoft Docs"
description: "Lär dig hur tooget media innehåll i Media Services genom att skapa och ladda upp tillgångar."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: c9c86380-9395-4db8-acea-507c52066f73
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2017
ms.author: juliako
ms.openlocfilehash: 11c8a359b09efe04b54490fd48ac0cd7c366f8b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-net"></a>Ladda upp filer till ett Media Services-konto med hjälp av .NET
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> * [Portal](media-services-portal-upload-files.md)
> 
> 

I Media Services överför du (eller för in) dina digitala filer till en tillgång. Hej **tillgången** entitet kan innehålla video, ljud, bilder, miniatyrsamlingar, text spår och textning filer (och hello metadata om dessa filer.)  När hello filerna har överförts lagras innehållet på ett säkert sätt i hello molnet för ytterligare bearbetning och strömning.

hello filerna i hello tillgången kallas **Tillgångsfiler**. Hej **AssetFile** instansen och hello faktiska mediefilen är två distinkta objekt. Hej AssetFile instans innehåller metadata om hello mediefilen när hello mediefilen innehåller hello faktiskt medieinnehåll.

> [!NOTE]
> det gäller hello följande överväganden:
> 
> * Media Services använder hello värdet hello IAssetFile.Name egenskapen när du skapar URL: er för hello direktuppspelning av innehåll (till exempel http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Därför tillåts procent-encoding inte. Hej värdet för hello **namn** egenskapen får inte ha någon av följande hello [procent-encoding-reserverade tecken](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? [] # % ”. Dessutom det kan bara finnas ett '.' för hello filnamnstillägg.
> * hello längd på hello namn får inte vara större än 260 tecken.
> * Det finns en gräns toohello maximal filstorlek som stöds för bearbetning i Media Services. Se [detta](media-services-quotas-and-limitations.md) avsnittet för information om hello filstorleksbegränsningar.
> * Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy). Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer). Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.
> 

När du skapar tillgångar, kan du ange hello följande krypteringsalternativ för. 

* **Ingen** – Ingen kryptering används. Detta är standardvärdet för hello. Observera att när du använder det här alternativet om ditt innehåll inte skyddas under överföringen eller i vila i lagring.
  Använd det här alternativet om du planerar toodeliver en MP4 med progressivt nedladdning. 
* **CommonEncryption** – Använd det här alternativet om du överför innehåll som redan har krypterats och skyddats med vanlig kryptering eller PlayReady DRM (till exempel Smooth Streaming som skyddas med PlayReady DRM).
* **EnvelopeEncrypted** – Använd det här alternativet om du överför HLS som krypterats med AES. Observera att hello filer måste har kodats och krypterats av Transform Manager.
* **StorageEncrypted** - krypterar innehållet lokalt med hjälp av AES 256 bitarskryptering och sedan överför tooAzure lagring där den lagras krypterat i vila. Tillgångar som skyddas med Lagringskryptering okrypterad och placeras i en krypterad fil system tidigare tooencoding och eventuellt omkrypterade tidigare toouploading tillbaka som en ny utdatatillgång automatiskt. hello primära användningsfall för Lagringskryptering är om du vill toosecure dina indata mediefiler hög kvalitet med stark kryptering i vila på disk.
  
    Media Services tillhandahåller på disklagring kryptering för dina tillgångar, inte över överföring som Digital Rights Manager (DRM).
  
    Om tillgången är lagringskrypterad, måste du konfigurera principen för tillgångsleverans. Mer information finns i [konfigurera tillgångsleveransprincip](media-services-dotnet-configure-asset-delivery-policy.md).

Om du anger för din tillgång toobe krypteras med en **CommonEncrypted** alternativ, eller en **EnvelopeEncypted** alternativet behöver du tooassociate din tillgång med en **ContentKey**. Mer information finns i [hur toocreate en ContentKey](media-services-dotnet-create-contentkey.md). 

Om du anger för din tillgång toobe krypteras med en **StorageEncrypted** alternativ, hello Media Services SDK för .NET skapar en **StorateEncrypted** **ContentKey** för din tillgången.

Det här avsnittet visar hur toouse Media Services .NET SDK samt Media Services .NET SDK-tillägg tooupload filer till en tillgång med Media Services.

## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Ladda upp en enstaka fil med Media Services .NET SDK
hello exempelkoden nedan använder .NET SDK tooupload en enskild fil. Hej AccessPolicy och lokaliserare skapas och förstörs av hello överför funktion. 


        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }


## <a name="upload-multiple-files-with-media-services-net-sdk"></a>Överför flera filer med Media Services .NET SDK
Hej följande kod visar hur toocreate en tillgång och överför flera filer.

hello koden hello följande:

* Skapar en tom tillgång hello CreateEmptyAsset definieras i hello föregående steg.
* Skapar en **AccessPolicy** -instans som definierar hello behörigheter och varaktighet för åtkomst toohello tillgången.
* Skapar en **lokaliserare** instans som tillhandahåller åtkomst toohello tillgången.
* Skapar en **BlobTransferClient** instans. Den här typen representerar en klient som körs på hello Azure BLOB-objekt. I det här exemplet använder vi hello toomonitor hello överför klienten. 
* Räknar upp via filer i hello angiven katalog och skapar en **AssetFile** -instans för varje fil.
* Överföringar hello filer till Media Services med hello **UploadAsync** metod. 

> [!NOTE]
> Använd hello UploadAsync metoden tooensure som hello anrop inte blockerar och hello filer överförs parallellt.
> 
> 

        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended toovalidate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading hello files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }

    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as hello upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



När du hämtar ett stort antal tillgångar, Tänk hello följande.

* Skapa en ny **CloudMediaContext** objekt per tråd. Hej **CloudMediaContext** klass är inte trådsäker.
* Öka NumberOfConcurrentTransfers från hello standardvärdet 2 tooa högre värde som 5. Den här egenskapen påverkar alla förekomster av **CloudMediaContext**. 
* Hålla ParallelTransferThreadCount hello standardvärdet 10.

## <a id="ingest_in_bulk"></a>Fört in tillgångar i gång med hjälp av Media Services .NET SDK
Överföringen av stora tillgångsfiler kan utgöra en flaskhals under skapande av tillgångsinformation. Fört in tillgångar i massutskick eller ”samtidigt vill föra in” innebär att Frikoppling tillgången skapas hello överföringen. toouse en grupp ingesting metod, skapa ett manifest (IngestManifest) som beskriver hello tillgången och dess associerade filer. Sedan Använd hello överför metoden i ditt val tooupload hello tillhörande filer toohello manifest's blob-behållare. Microsoft Azure Media Services bevakar hello blob-behållare som är associerade med hello manifest. När en fil har överförts toohello blob-behållare, Slutför Microsoft Azure Media Services hello tillgången skapas baserat på hello konfigurationen av hello tillgång i hello manifest (IngestManifestAsset).

toocreate nya IngestManifest anropa hello Create-metoden som exponeras av hello hello CloudMediaContext IngestManifests mängden. Den här metoden skapar en ny IngestManifest med hello manifestnamnet som du anger.

    IIngestManifest manifest = context.IngestManifests.Create(name);

Skapa hello tillgångar som ska associeras med hello bulk IngestManifest. Konfigurera hello önskad krypteringsalternativen på hello tillgång för att föra in samtidigt.

    // Create hello assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

En IngestManifestAsset associerar en tillgång med bulk IngestManifest för att föra in samtidigt. Här associerar även hello AssetFiles som utgör varje tillgång. toocreate en IngestManifestAsset använda hello Create-metoden på hello serverkontext.

hello visar exemplet nedan att lägga till två nya IngestManifestAssets som kopplar hello två tillgångar tidigare skapade toohello bulk mata in manifestet. Varje IngestManifestAsset associerar även en uppsättning filer som överförs för varje tillgång vid massinläsning vill föra in.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;

    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });

Du kan använda en hög hastighet klientprogrammet kan överföra hello tillgången filer toohello blobblagringsbehållare URI som tillhandahålls av hello **IIngestManifest.BlobStorageUriForUpload** -egenskapen för hello IngestManifest. En tjänst för överföringen av viktiga hög hastighet är [Aspera på begäran för Azure-programmet](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). Du kan också skriva kod tooupload hello tillgångar filer som visas i följande kodexempel hello.

    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);

            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);

            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);

            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });

        copytask.Start();
    }

hello-kod för att överföra hello tillgångsfiler för hello prov som används i det här avsnittet visas i följande kodexempel hello.

    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);


Du kan fastställa hello fortskrider hello samtidigt vill föra in för alla resurser som är associerade med en **IngestManifest** genom att avsöka hello statistik-egenskapen för hello **IngestManifest**. I ordning tooupdate statusinformation, måste du använda en ny **CloudMediaContext** varje gång du avsöka hello statistik egenskapen.

hello exemplet nedan visar avsöker en IngestManifest av dess **Id**.

    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();

          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);

                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }

             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }



## <a name="upload-files-using-net-sdk-extensions"></a>Överföra filer med hjälp av .NET SDK-tillägg
hello exemplet nedan visar hur tooupload en enstaka fil med hjälp av .NET SDK-tillägg. I det här fallet hello **CreateFromFile** metod används, men hello asynkrona versionen är också tillgänglig (**CreateFromFileAsync**). Hej **CreateFromFile** metod kan du ange hello filnamn, kryptering och ett återanrop i ordning tooreport hello Överföringsförlopp hello-filen.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }

hello följande exempel anropar UploadFile funktion och anger lagringskryptering som alternativ för skapande av hello tillgången.  

    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);

## <a name="next-steps"></a>Nästa steg

Du kan nu koda överförda tillgångar. Mer information finns i [Koda tillgångar](media-services-portal-encode.md).

Du kan också använda Azure Functions tootrigger ett kodningsjobb baserat på en fil som inkommer på hello konfigurerats behållare. Mer information finns i [det här exemplet](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Nästa steg
Nu när du har överfört en tillgång tooMedia tjänster gå toohello [hur tooGet en Medieprocessor] [ How tooGet a Media Processor] avsnittet.

[How tooGet a Media Processor]: media-services-get-media-processor.md

