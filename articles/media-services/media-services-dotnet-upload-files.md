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
# <a name="upload-files-into-a-media-services-account-using-net"></a><span data-ttu-id="c3673-103">Ladda upp filer till ett Media Services-konto med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="c3673-103">Upload files into a Media Services account using .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c3673-104">.NET</span><span class="sxs-lookup"><span data-stu-id="c3673-104">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="c3673-105">REST</span><span class="sxs-lookup"><span data-stu-id="c3673-105">REST</span></span>](media-services-rest-upload-files.md)
> * [<span data-ttu-id="c3673-106">Portal</span><span class="sxs-lookup"><span data-stu-id="c3673-106">Portal</span></span>](media-services-portal-upload-files.md)
> 
> 

<span data-ttu-id="c3673-107">I Media Services överför du (eller för in) dina digitala filer till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="c3673-107">In Media Services, you upload (or ingest) your digital files into an asset.</span></span> <span data-ttu-id="c3673-108">Hej **tillgången** entitet kan innehålla video, ljud, bilder, miniatyrsamlingar, text spår och textning filer (och hello metadata om dessa filer.)  När hello filerna har överförts lagras innehållet på ett säkert sätt i hello molnet för ytterligare bearbetning och strömning.</span><span class="sxs-lookup"><span data-stu-id="c3673-108">hello **Asset** entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and hello metadata about these files.)  Once hello files are uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span>

<span data-ttu-id="c3673-109">hello filerna i hello tillgången kallas **Tillgångsfiler**.</span><span class="sxs-lookup"><span data-stu-id="c3673-109">hello files in hello asset are called **Asset Files**.</span></span> <span data-ttu-id="c3673-110">Hej **AssetFile** instansen och hello faktiska mediefilen är två distinkta objekt.</span><span class="sxs-lookup"><span data-stu-id="c3673-110">hello **AssetFile** instance and hello actual media file are two distinct objects.</span></span> <span data-ttu-id="c3673-111">Hej AssetFile instans innehåller metadata om hello mediefilen när hello mediefilen innehåller hello faktiskt medieinnehåll.</span><span class="sxs-lookup"><span data-stu-id="c3673-111">hello AssetFile instance contains metadata about hello media file, while hello media file contains hello actual media content.</span></span>

> [!NOTE]
> <span data-ttu-id="c3673-112">det gäller hello följande överväganden:</span><span class="sxs-lookup"><span data-stu-id="c3673-112">hello following considerations apply:</span></span>
> 
> * <span data-ttu-id="c3673-113">Media Services använder hello värdet hello IAssetFile.Name egenskapen när du skapar URL: er för hello direktuppspelning av innehåll (till exempel http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Därför tillåts procent-encoding inte.</span><span class="sxs-lookup"><span data-stu-id="c3673-113">Media Services uses hello value of hello IAssetFile.Name property when building URLs for hello streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="c3673-114">Hej värdet för hello **namn** egenskapen får inte ha någon av följande hello [procent-encoding-reserverade tecken](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? [] # % ”.</span><span class="sxs-lookup"><span data-stu-id="c3673-114">hello value of hello **Name** property cannot have any of hello following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="c3673-115">Dessutom det kan bara finnas ett '.' för hello filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="c3673-115">Also, there can only be one '.' for hello file name extension.</span></span>
> * <span data-ttu-id="c3673-116">hello längd på hello namn får inte vara större än 260 tecken.</span><span class="sxs-lookup"><span data-stu-id="c3673-116">hello length of hello name should not be greater than 260 characters.</span></span>
> * <span data-ttu-id="c3673-117">Det finns en gräns toohello maximal filstorlek som stöds för bearbetning i Media Services.</span><span class="sxs-lookup"><span data-stu-id="c3673-117">There is a limit toohello maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="c3673-118">Se [detta](media-services-quotas-and-limitations.md) avsnittet för information om hello filstorleksbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="c3673-118">Please see [this](media-services-quotas-and-limitations.md) topic for details about hello file size limitation.</span></span>
> * <span data-ttu-id="c3673-119">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="c3673-119">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="c3673-120">Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer).</span><span class="sxs-lookup"><span data-stu-id="c3673-120">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="c3673-121">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c3673-121">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>
> 

<span data-ttu-id="c3673-122">När du skapar tillgångar, kan du ange hello följande krypteringsalternativ för.</span><span class="sxs-lookup"><span data-stu-id="c3673-122">When you create assets, you can specify hello following encryption options.</span></span> 

* <span data-ttu-id="c3673-123">**Ingen** – Ingen kryptering används.</span><span class="sxs-lookup"><span data-stu-id="c3673-123">**None** - No encryption is used.</span></span> <span data-ttu-id="c3673-124">Detta är standardvärdet för hello.</span><span class="sxs-lookup"><span data-stu-id="c3673-124">This is hello default value.</span></span> <span data-ttu-id="c3673-125">Observera att när du använder det här alternativet om ditt innehåll inte skyddas under överföringen eller i vila i lagring.</span><span class="sxs-lookup"><span data-stu-id="c3673-125">Note that when using this option your content is not protected in transit or at rest in storage.</span></span>
  <span data-ttu-id="c3673-126">Använd det här alternativet om du planerar toodeliver en MP4 med progressivt nedladdning.</span><span class="sxs-lookup"><span data-stu-id="c3673-126">If you plan toodeliver an MP4 using progressive download, use this option.</span></span> 
* <span data-ttu-id="c3673-127">**CommonEncryption** – Använd det här alternativet om du överför innehåll som redan har krypterats och skyddats med vanlig kryptering eller PlayReady DRM (till exempel Smooth Streaming som skyddas med PlayReady DRM).</span><span class="sxs-lookup"><span data-stu-id="c3673-127">**CommonEncryption** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="c3673-128">**EnvelopeEncrypted** – Använd det här alternativet om du överför HLS som krypterats med AES.</span><span class="sxs-lookup"><span data-stu-id="c3673-128">**EnvelopeEncrypted** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="c3673-129">Observera att hello filer måste har kodats och krypterats av Transform Manager.</span><span class="sxs-lookup"><span data-stu-id="c3673-129">Note that hello files must have been encoded and encrypted by Transform Manager.</span></span>
* <span data-ttu-id="c3673-130">**StorageEncrypted** - krypterar innehållet lokalt med hjälp av AES 256 bitarskryptering och sedan överför tooAzure lagring där den lagras krypterat i vila.</span><span class="sxs-lookup"><span data-stu-id="c3673-130">**StorageEncrypted** - Encrypts your clear content locally using AES-256 bit encryption and then uploads it tooAzure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="c3673-131">Tillgångar som skyddas med Lagringskryptering okrypterad och placeras i en krypterad fil system tidigare tooencoding och eventuellt omkrypterade tidigare toouploading tillbaka som en ny utdatatillgång automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c3673-131">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior tooencoding, and optionally re-encrypted prior toouploading back as a new output asset.</span></span> <span data-ttu-id="c3673-132">hello primära användningsfall för Lagringskryptering är om du vill toosecure dina indata mediefiler hög kvalitet med stark kryptering i vila på disk.</span><span class="sxs-lookup"><span data-stu-id="c3673-132">hello primary use case for Storage Encryption is when you want toosecure your high quality input media files with strong encryption at rest on disk.</span></span>
  
    <span data-ttu-id="c3673-133">Media Services tillhandahåller på disklagring kryptering för dina tillgångar, inte över överföring som Digital Rights Manager (DRM).</span><span class="sxs-lookup"><span data-stu-id="c3673-133">Media Services provides on-disk storage encryption for your assets, not over-the-wire like Digital Rights Manager (DRM).</span></span>
  
    <span data-ttu-id="c3673-134">Om tillgången är lagringskrypterad, måste du konfigurera principen för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="c3673-134">If your asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="c3673-135">Mer information finns i [konfigurera tillgångsleveransprincip](media-services-dotnet-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="c3673-135">For more information see [Configuring asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md).</span></span>

<span data-ttu-id="c3673-136">Om du anger för din tillgång toobe krypteras med en **CommonEncrypted** alternativ, eller en **EnvelopeEncypted** alternativet behöver du tooassociate din tillgång med en **ContentKey**.</span><span class="sxs-lookup"><span data-stu-id="c3673-136">If you specify for your asset toobe encrypted with a **CommonEncrypted** option, or an **EnvelopeEncypted** option, you will need tooassociate your asset with a **ContentKey**.</span></span> <span data-ttu-id="c3673-137">Mer information finns i [hur toocreate en ContentKey](media-services-dotnet-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="c3673-137">For more information, see [How toocreate a ContentKey](media-services-dotnet-create-contentkey.md).</span></span> 

<span data-ttu-id="c3673-138">Om du anger för din tillgång toobe krypteras med en **StorageEncrypted** alternativ, hello Media Services SDK för .NET skapar en **StorateEncrypted** **ContentKey** för din tillgången.</span><span class="sxs-lookup"><span data-stu-id="c3673-138">If you specify for your asset toobe encrypted with a **StorageEncrypted** option, hello Media Services SDK for .NET will create a **StorateEncrypted** **ContentKey** for your asset.</span></span>

<span data-ttu-id="c3673-139">Det här avsnittet visar hur toouse Media Services .NET SDK samt Media Services .NET SDK-tillägg tooupload filer till en tillgång med Media Services.</span><span class="sxs-lookup"><span data-stu-id="c3673-139">This topic shows how toouse Media Services .NET SDK as well as Media Services .NET SDK extensions tooupload files into a Media Services asset.</span></span>

## <a name="upload-a-single-file-with-media-services-net-sdk"></a><span data-ttu-id="c3673-140">Ladda upp en enstaka fil med Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="c3673-140">Upload a single file with Media Services .NET SDK</span></span>
<span data-ttu-id="c3673-141">hello exempelkoden nedan använder .NET SDK tooupload en enskild fil.</span><span class="sxs-lookup"><span data-stu-id="c3673-141">hello sample code below uses .NET SDK tooupload a single file.</span></span> <span data-ttu-id="c3673-142">Hej AccessPolicy och lokaliserare skapas och förstörs av hello överför funktion.</span><span class="sxs-lookup"><span data-stu-id="c3673-142">hello AccessPolicy and Locator are created and destroyed by hello Upload function.</span></span> 


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


## <a name="upload-multiple-files-with-media-services-net-sdk"></a><span data-ttu-id="c3673-143">Överför flera filer med Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="c3673-143">Upload multiple files with Media Services .NET SDK</span></span>
<span data-ttu-id="c3673-144">Hej följande kod visar hur toocreate en tillgång och överför flera filer.</span><span class="sxs-lookup"><span data-stu-id="c3673-144">hello following code shows how toocreate an asset and upload multiple files.</span></span>

<span data-ttu-id="c3673-145">hello koden hello följande:</span><span class="sxs-lookup"><span data-stu-id="c3673-145">hello code does hello following:</span></span>

* <span data-ttu-id="c3673-146">Skapar en tom tillgång hello CreateEmptyAsset definieras i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="c3673-146">Creates an empty asset using hello CreateEmptyAsset method defined in hello previous step.</span></span>
* <span data-ttu-id="c3673-147">Skapar en **AccessPolicy** -instans som definierar hello behörigheter och varaktighet för åtkomst toohello tillgången.</span><span class="sxs-lookup"><span data-stu-id="c3673-147">Creates an **AccessPolicy** instance that defines hello permissions and duration of access toohello asset.</span></span>
* <span data-ttu-id="c3673-148">Skapar en **lokaliserare** instans som tillhandahåller åtkomst toohello tillgången.</span><span class="sxs-lookup"><span data-stu-id="c3673-148">Creates a **Locator** instance that provides access toohello asset.</span></span>
* <span data-ttu-id="c3673-149">Skapar en **BlobTransferClient** instans.</span><span class="sxs-lookup"><span data-stu-id="c3673-149">Creates a **BlobTransferClient** instance.</span></span> <span data-ttu-id="c3673-150">Den här typen representerar en klient som körs på hello Azure BLOB-objekt.</span><span class="sxs-lookup"><span data-stu-id="c3673-150">This type represents a client that operates on hello Azure blobs.</span></span> <span data-ttu-id="c3673-151">I det här exemplet använder vi hello toomonitor hello överför klienten.</span><span class="sxs-lookup"><span data-stu-id="c3673-151">In this example we use hello client toomonitor hello upload progress.</span></span> 
* <span data-ttu-id="c3673-152">Räknar upp via filer i hello angiven katalog och skapar en **AssetFile** -instans för varje fil.</span><span class="sxs-lookup"><span data-stu-id="c3673-152">Enumerates through files in hello specified directory and creates an **AssetFile** instance for each file.</span></span>
* <span data-ttu-id="c3673-153">Överföringar hello filer till Media Services med hello **UploadAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="c3673-153">Uploads hello files into Media Services using hello **UploadAsync** method.</span></span> 

> [!NOTE]
> <span data-ttu-id="c3673-154">Använd hello UploadAsync metoden tooensure som hello anrop inte blockerar och hello filer överförs parallellt.</span><span class="sxs-lookup"><span data-stu-id="c3673-154">Use hello UploadAsync method tooensure that hello calls are not blocking and hello files are uploaded in parallel.</span></span>
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



<span data-ttu-id="c3673-155">När du hämtar ett stort antal tillgångar, Tänk hello följande.</span><span class="sxs-lookup"><span data-stu-id="c3673-155">When uploading a large number of assets, consider hello following.</span></span>

* <span data-ttu-id="c3673-156">Skapa en ny **CloudMediaContext** objekt per tråd.</span><span class="sxs-lookup"><span data-stu-id="c3673-156">Create a new **CloudMediaContext** object per thread.</span></span> <span data-ttu-id="c3673-157">Hej **CloudMediaContext** klass är inte trådsäker.</span><span class="sxs-lookup"><span data-stu-id="c3673-157">hello **CloudMediaContext** class is not thread safe.</span></span>
* <span data-ttu-id="c3673-158">Öka NumberOfConcurrentTransfers från hello standardvärdet 2 tooa högre värde som 5.</span><span class="sxs-lookup"><span data-stu-id="c3673-158">Increase NumberOfConcurrentTransfers from hello default value of 2 tooa higher value like 5.</span></span> <span data-ttu-id="c3673-159">Den här egenskapen påverkar alla förekomster av **CloudMediaContext**.</span><span class="sxs-lookup"><span data-stu-id="c3673-159">Setting this property affects all instances of **CloudMediaContext**.</span></span> 
* <span data-ttu-id="c3673-160">Hålla ParallelTransferThreadCount hello standardvärdet 10.</span><span class="sxs-lookup"><span data-stu-id="c3673-160">Keep ParallelTransferThreadCount at hello default value of 10.</span></span>

## <span data-ttu-id="c3673-161"><a id="ingest_in_bulk"></a>Fört in tillgångar i gång med hjälp av Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="c3673-161"><a id="ingest_in_bulk"></a>Ingesting Assets in Bulk using Media Services .NET SDK</span></span>
<span data-ttu-id="c3673-162">Överföringen av stora tillgångsfiler kan utgöra en flaskhals under skapande av tillgångsinformation.</span><span class="sxs-lookup"><span data-stu-id="c3673-162">Uploading large asset files can be a bottleneck during asset creation.</span></span> <span data-ttu-id="c3673-163">Fört in tillgångar i massutskick eller ”samtidigt vill föra in” innebär att Frikoppling tillgången skapas hello överföringen.</span><span class="sxs-lookup"><span data-stu-id="c3673-163">Ingesting Assets in Bulk or “Bulk Ingesting”, involves decoupling asset creation from hello upload process.</span></span> <span data-ttu-id="c3673-164">toouse en grupp ingesting metod, skapa ett manifest (IngestManifest) som beskriver hello tillgången och dess associerade filer.</span><span class="sxs-lookup"><span data-stu-id="c3673-164">toouse a bulk ingesting approach, create a manifest (IngestManifest) that describes hello asset and its associated files.</span></span> <span data-ttu-id="c3673-165">Sedan Använd hello överför metoden i ditt val tooupload hello tillhörande filer toohello manifest's blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="c3673-165">Then use hello upload method of your choice tooupload hello associated files toohello manifest’s blob container.</span></span> <span data-ttu-id="c3673-166">Microsoft Azure Media Services bevakar hello blob-behållare som är associerade med hello manifest.</span><span class="sxs-lookup"><span data-stu-id="c3673-166">Microsoft Azure Media Services watches hello blob container associated with hello manifest.</span></span> <span data-ttu-id="c3673-167">När en fil har överförts toohello blob-behållare, Slutför Microsoft Azure Media Services hello tillgången skapas baserat på hello konfigurationen av hello tillgång i hello manifest (IngestManifestAsset).</span><span class="sxs-lookup"><span data-stu-id="c3673-167">Once a file is uploaded toohello blob container, Microsoft Azure Media Services completes hello asset creation based on hello configuration of hello asset in hello manifest (IngestManifestAsset).</span></span>

<span data-ttu-id="c3673-168">toocreate nya IngestManifest anropa hello Create-metoden som exponeras av hello hello CloudMediaContext IngestManifests mängden.</span><span class="sxs-lookup"><span data-stu-id="c3673-168">toocreate a new IngestManifest call hello Create method exposed by hello IngestManifests collection on hello CloudMediaContext.</span></span> <span data-ttu-id="c3673-169">Den här metoden skapar en ny IngestManifest med hello manifestnamnet som du anger.</span><span class="sxs-lookup"><span data-stu-id="c3673-169">This method will create a new IngestManifest with hello manifest name you provide.</span></span>

    IIngestManifest manifest = context.IngestManifests.Create(name);

<span data-ttu-id="c3673-170">Skapa hello tillgångar som ska associeras med hello bulk IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="c3673-170">Create hello assets that will be associated with hello bulk IngestManifest.</span></span> <span data-ttu-id="c3673-171">Konfigurera hello önskad krypteringsalternativen på hello tillgång för att föra in samtidigt.</span><span class="sxs-lookup"><span data-stu-id="c3673-171">Configure hello desired encryption options on hello asset for bulk ingesting.</span></span>

    // Create hello assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

<span data-ttu-id="c3673-172">En IngestManifestAsset associerar en tillgång med bulk IngestManifest för att föra in samtidigt.</span><span class="sxs-lookup"><span data-stu-id="c3673-172">An IngestManifestAsset associates an Asset with a bulk IngestManifest for bulk ingesting.</span></span> <span data-ttu-id="c3673-173">Här associerar även hello AssetFiles som utgör varje tillgång.</span><span class="sxs-lookup"><span data-stu-id="c3673-173">It also associates hello AssetFiles that will make up each Asset.</span></span> <span data-ttu-id="c3673-174">toocreate en IngestManifestAsset använda hello Create-metoden på hello serverkontext.</span><span class="sxs-lookup"><span data-stu-id="c3673-174">toocreate an IngestManifestAsset, use hello Create method on hello server context.</span></span>

<span data-ttu-id="c3673-175">hello visar exemplet nedan att lägga till två nya IngestManifestAssets som kopplar hello två tillgångar tidigare skapade toohello bulk mata in manifestet.</span><span class="sxs-lookup"><span data-stu-id="c3673-175">hello following example demonstrates adding two new IngestManifestAssets that associate hello two assets previously created toohello bulk ingest manifest.</span></span> <span data-ttu-id="c3673-176">Varje IngestManifestAsset associerar även en uppsättning filer som överförs för varje tillgång vid massinläsning vill föra in.</span><span class="sxs-lookup"><span data-stu-id="c3673-176">Each IngestManifestAsset also associates a set of files that will be uploaded for each asset during bulk ingesting.</span></span>  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;

    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });

<span data-ttu-id="c3673-177">Du kan använda en hög hastighet klientprogrammet kan överföra hello tillgången filer toohello blobblagringsbehållare URI som tillhandahålls av hello **IIngestManifest.BlobStorageUriForUpload** -egenskapen för hello IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="c3673-177">You can use any high speed client application capable of uploading hello asset files toohello blob storage container URI provided by hello **IIngestManifest.BlobStorageUriForUpload** property of hello IngestManifest.</span></span> <span data-ttu-id="c3673-178">En tjänst för överföringen av viktiga hög hastighet är [Aspera på begäran för Azure-programmet](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6).</span><span class="sxs-lookup"><span data-stu-id="c3673-178">One notable high speed upload service is [Aspera On Demand for Azure Application](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6).</span></span> <span data-ttu-id="c3673-179">Du kan också skriva kod tooupload hello tillgångar filer som visas i följande kodexempel hello.</span><span class="sxs-lookup"><span data-stu-id="c3673-179">You can also write code tooupload hello assets files as shown in hello following code example.</span></span>

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

<span data-ttu-id="c3673-180">hello-kod för att överföra hello tillgångsfiler för hello prov som används i det här avsnittet visas i följande kodexempel hello.</span><span class="sxs-lookup"><span data-stu-id="c3673-180">hello code for uploading hello asset files for hello sample used in this topic is shown in hello following code example.</span></span>

    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);


<span data-ttu-id="c3673-181">Du kan fastställa hello fortskrider hello samtidigt vill föra in för alla resurser som är associerade med en **IngestManifest** genom att avsöka hello statistik-egenskapen för hello **IngestManifest**.</span><span class="sxs-lookup"><span data-stu-id="c3673-181">You can determine hello progress of hello bulk ingesting for all assets associated with an **IngestManifest** by polling hello Statistics property of hello **IngestManifest**.</span></span> <span data-ttu-id="c3673-182">I ordning tooupdate statusinformation, måste du använda en ny **CloudMediaContext** varje gång du avsöka hello statistik egenskapen.</span><span class="sxs-lookup"><span data-stu-id="c3673-182">In order tooupdate progress information, you must use a new **CloudMediaContext** each time you poll hello Statistics property.</span></span>

<span data-ttu-id="c3673-183">hello exemplet nedan visar avsöker en IngestManifest av dess **Id**.</span><span class="sxs-lookup"><span data-stu-id="c3673-183">hello following example demonstrates polling an IngestManifest by its **Id**.</span></span>

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



## <a name="upload-files-using-net-sdk-extensions"></a><span data-ttu-id="c3673-184">Överföra filer med hjälp av .NET SDK-tillägg</span><span class="sxs-lookup"><span data-stu-id="c3673-184">Upload files using .NET SDK Extensions</span></span>
<span data-ttu-id="c3673-185">hello exemplet nedan visar hur tooupload en enstaka fil med hjälp av .NET SDK-tillägg.</span><span class="sxs-lookup"><span data-stu-id="c3673-185">hello example below shows how tooupload a single file using .NET SDK Extensions.</span></span> <span data-ttu-id="c3673-186">I det här fallet hello **CreateFromFile** metod används, men hello asynkrona versionen är också tillgänglig (**CreateFromFileAsync**).</span><span class="sxs-lookup"><span data-stu-id="c3673-186">In this case hello **CreateFromFile** method is used, but hello asynchronous version is also available (**CreateFromFileAsync**).</span></span> <span data-ttu-id="c3673-187">Hej **CreateFromFile** metod kan du ange hello filnamn, kryptering och ett återanrop i ordning tooreport hello Överföringsförlopp hello-filen.</span><span class="sxs-lookup"><span data-stu-id="c3673-187">hello **CreateFromFile** method lets you specify hello file name, encryption option, and a callback in order tooreport hello upload progress of hello file.</span></span>

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

<span data-ttu-id="c3673-188">hello följande exempel anropar UploadFile funktion och anger lagringskryptering som alternativ för skapande av hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="c3673-188">hello following example calls UploadFile function and specifies storage encryption as hello asset creation option.</span></span>  

    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);

## <a name="next-steps"></a><span data-ttu-id="c3673-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3673-189">Next steps</span></span>

<span data-ttu-id="c3673-190">Du kan nu koda överförda tillgångar.</span><span class="sxs-lookup"><span data-stu-id="c3673-190">You can now encode your uploaded assets.</span></span> <span data-ttu-id="c3673-191">Mer information finns i [Koda tillgångar](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="c3673-191">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="c3673-192">Du kan också använda Azure Functions tootrigger ett kodningsjobb baserat på en fil som inkommer på hello konfigurerats behållare.</span><span class="sxs-lookup"><span data-stu-id="c3673-192">You can also use Azure Functions tootrigger an encoding job based on a file arriving in hello configured container.</span></span> <span data-ttu-id="c3673-193">Mer information finns i [det här exemplet](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="c3673-193">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="c3673-194">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="c3673-194">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c3673-195">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="c3673-195">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a><span data-ttu-id="c3673-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3673-196">Next step</span></span>
<span data-ttu-id="c3673-197">Nu när du har överfört en tillgång tooMedia tjänster gå toohello [hur tooGet en Medieprocessor] [ How tooGet a Media Processor] avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c3673-197">Now that you have uploaded an asset tooMedia Services, go toohello [How tooGet a Media Processor][How tooGet a Media Processor] topic.</span></span>

[How tooGet a Media Processor]: media-services-get-media-processor.md

