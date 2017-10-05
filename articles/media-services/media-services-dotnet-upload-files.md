---
title: "Ladda upp filer till ett Media Services-konto med hjälp av .NET | Microsoft Docs"
description: "Lär dig mer om att få medieinnehåll i Media Services genom att skapa och ladda upp tillgångar."
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
ms.openlocfilehash: ec8c1da633374ba684f6a0a895c542ee76ef73b8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="upload-files-into-a-media-services-account-using-net"></a><span data-ttu-id="0d636-103">Ladda upp filer till ett Media Services-konto med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="0d636-103">Upload files into a Media Services account using .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0d636-104">.NET</span><span class="sxs-lookup"><span data-stu-id="0d636-104">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="0d636-105">REST</span><span class="sxs-lookup"><span data-stu-id="0d636-105">REST</span></span>](media-services-rest-upload-files.md)
> * [<span data-ttu-id="0d636-106">Portal</span><span class="sxs-lookup"><span data-stu-id="0d636-106">Portal</span></span>](media-services-portal-upload-files.md)
> 
> 

<span data-ttu-id="0d636-107">I Media Services överför du (eller för in) dina digitala filer till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="0d636-107">In Media Services, you upload (or ingest) your digital files into an asset.</span></span> <span data-ttu-id="0d636-108">Den **tillgången** entitet kan innehålla video, ljud, bilder, miniatyrsamlingar, text spår och textning filer (och metadata om dessa filer.)  När filerna har överförts lagras innehållet på ett säkert sätt i molnet för ytterligare bearbetning och strömning.</span><span class="sxs-lookup"><span data-stu-id="0d636-108">The **Asset** entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and the metadata about these files.)  Once the files are uploaded, your content is stored securely in the cloud for further processing and streaming.</span></span>

<span data-ttu-id="0d636-109">Filerna i tillgången kallas **Tillgångsfiler**.</span><span class="sxs-lookup"><span data-stu-id="0d636-109">The files in the asset are called **Asset Files**.</span></span> <span data-ttu-id="0d636-110">Den **AssetFile** instansen och den faktiska mediefilen är två distinkta objekt.</span><span class="sxs-lookup"><span data-stu-id="0d636-110">The **AssetFile** instance and the actual media file are two distinct objects.</span></span> <span data-ttu-id="0d636-111">AssetFile-instans innehåller metadata om filen media när mediefilen innehåller faktiskt medieinnehåll.</span><span class="sxs-lookup"><span data-stu-id="0d636-111">The AssetFile instance contains metadata about the media file, while the media file contains the actual media content.</span></span>

> [!NOTE]
> <span data-ttu-id="0d636-112">Följande gäller:</span><span class="sxs-lookup"><span data-stu-id="0d636-112">The following considerations apply:</span></span>
> 
> * <span data-ttu-id="0d636-113">Media Services använder värdet för egenskapen IAssetFile.Name när du skapar URL: er för strömning innehållet (till exempel http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Därför tillåts procent-encoding inte.</span><span class="sxs-lookup"><span data-stu-id="0d636-113">Media Services uses the value of the IAssetFile.Name property when building URLs for the streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="0d636-114">Värdet för den **namn** egenskapen får inte ha något av följande [procent-encoding-reserverade tecken](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? [] # % ”.</span><span class="sxs-lookup"><span data-stu-id="0d636-114">The value of the **Name** property cannot have any of the following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="0d636-115">Dessutom det kan bara finnas ett '.' för filnamnstillägget.</span><span class="sxs-lookup"><span data-stu-id="0d636-115">Also, there can only be one '.' for the file name extension.</span></span>
> * <span data-ttu-id="0d636-116">Längden på namnet får inte vara större än 260 tecken.</span><span class="sxs-lookup"><span data-stu-id="0d636-116">The length of the name should not be greater than 260 characters.</span></span>
> * <span data-ttu-id="0d636-117">Det finns en gräns för maximal filstorlek för bearbetning i Media Services.</span><span class="sxs-lookup"><span data-stu-id="0d636-117">There is a limit to the maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="0d636-118">Information om filstorleksbegränsningen finns i [det här](media-services-quotas-and-limitations.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0d636-118">Please see [this](media-services-quotas-and-limitations.md) topic for details about the file size limitation.</span></span>
> * <span data-ttu-id="0d636-119">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="0d636-119">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="0d636-120">Du bör använda samma princip-ID om du alltid använder samma dagar/åtkomstbehörigheter, till exempel principer för positionerare som är avsedda att vara på plats under en längre tid (icke-överföringsprinciper).</span><span class="sxs-lookup"><span data-stu-id="0d636-120">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="0d636-121">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0d636-121">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>
> 

<span data-ttu-id="0d636-122">När du skapar tillgångar, anger du följande krypteringsalternativ.</span><span class="sxs-lookup"><span data-stu-id="0d636-122">When you create assets, you can specify the following encryption options.</span></span> 

* <span data-ttu-id="0d636-123">**Ingen** – Ingen kryptering används.</span><span class="sxs-lookup"><span data-stu-id="0d636-123">**None** - No encryption is used.</span></span> <span data-ttu-id="0d636-124">Detta är standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="0d636-124">This is the default value.</span></span> <span data-ttu-id="0d636-125">Observera att när du använder det här alternativet om ditt innehåll inte skyddas under överföringen eller i vila i lagring.</span><span class="sxs-lookup"><span data-stu-id="0d636-125">Note that when using this option your content is not protected in transit or at rest in storage.</span></span>
  <span data-ttu-id="0d636-126">Om du planerar att leverera en MP4 med progressivt nedladdning ska du använda det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="0d636-126">If you plan to deliver an MP4 using progressive download, use this option.</span></span> 
* <span data-ttu-id="0d636-127">**CommonEncryption** – Använd det här alternativet om du överför innehåll som redan har krypterats och skyddats med vanlig kryptering eller PlayReady DRM (till exempel Smooth Streaming som skyddas med PlayReady DRM).</span><span class="sxs-lookup"><span data-stu-id="0d636-127">**CommonEncryption** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="0d636-128">**EnvelopeEncrypted** – Använd det här alternativet om du överför HLS som krypterats med AES.</span><span class="sxs-lookup"><span data-stu-id="0d636-128">**EnvelopeEncrypted** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="0d636-129">Observera att filerna måste ha kodats och krypterats av Transform Manager.</span><span class="sxs-lookup"><span data-stu-id="0d636-129">Note that the files must have been encoded and encrypted by Transform Manager.</span></span>
* <span data-ttu-id="0d636-130">**StorageEncrypted** - krypterar innehållet lokalt med hjälp av AES 256 bitarskryptering och överför den till Azure Storage där den lagras krypterat i vila.</span><span class="sxs-lookup"><span data-stu-id="0d636-130">**StorageEncrypted** - Encrypts your clear content locally using AES-256 bit encryption and then uploads it to Azure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="0d636-131">Tillgångar som skyddas med Lagringskryptering avkrypteras automatiskt och placeras i ett krypterat filsystem före kodning och kan krypteras igen innan de överförs tillbaka som en ny utdatatillgång.</span><span class="sxs-lookup"><span data-stu-id="0d636-131">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior to encoding, and optionally re-encrypted prior to uploading back as a new output asset.</span></span> <span data-ttu-id="0d636-132">Det primära användningsfallet för Lagringskryptering är när du vill skydda dina hög kvalitet inkommande mediefiler med stark kryptering i vila på disk.</span><span class="sxs-lookup"><span data-stu-id="0d636-132">The primary use case for Storage Encryption is when you want to secure your high quality input media files with strong encryption at rest on disk.</span></span>
  
    <span data-ttu-id="0d636-133">Media Services tillhandahåller på disklagring kryptering för dina tillgångar, inte över överföring som Digital Rights Manager (DRM).</span><span class="sxs-lookup"><span data-stu-id="0d636-133">Media Services provides on-disk storage encryption for your assets, not over-the-wire like Digital Rights Manager (DRM).</span></span>
  
    <span data-ttu-id="0d636-134">Om tillgången är lagringskrypterad, måste du konfigurera principen för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="0d636-134">If your asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="0d636-135">Mer information finns i [konfigurera tillgångsleveransprincip](media-services-dotnet-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="0d636-135">For more information see [Configuring asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md).</span></span>

<span data-ttu-id="0d636-136">Om du anger för din tillgång som ska krypteras med en **CommonEncrypted** alternativ, eller en **EnvelopeEncypted** alternativet måste du associera din tillgång med en **ContentKey**.</span><span class="sxs-lookup"><span data-stu-id="0d636-136">If you specify for your asset to be encrypted with a **CommonEncrypted** option, or an **EnvelopeEncypted** option, you will need to associate your asset with a **ContentKey**.</span></span> <span data-ttu-id="0d636-137">Mer information finns i [hur du skapar en ContentKey](media-services-dotnet-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="0d636-137">For more information, see [How to create a ContentKey](media-services-dotnet-create-contentkey.md).</span></span> 

<span data-ttu-id="0d636-138">Om du anger för din tillgång som ska krypteras med en **StorageEncrypted** alternativ, Media Services SDK för .NET skapar en **StorateEncrypted** **ContentKey** för din tillgången.</span><span class="sxs-lookup"><span data-stu-id="0d636-138">If you specify for your asset to be encrypted with a **StorageEncrypted** option, the Media Services SDK for .NET will create a **StorateEncrypted** **ContentKey** for your asset.</span></span>

<span data-ttu-id="0d636-139">Det här avsnittet visar hur du använder Media Services .NET SDK samt Media Services .NET SDK-tillägg för att överföra filer till en tillgång med Media Services.</span><span class="sxs-lookup"><span data-stu-id="0d636-139">This topic shows how to use Media Services .NET SDK as well as Media Services .NET SDK extensions to upload files into a Media Services asset.</span></span>

## <a name="upload-a-single-file-with-media-services-net-sdk"></a><span data-ttu-id="0d636-140">Ladda upp en enstaka fil med Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="0d636-140">Upload a single file with Media Services .NET SDK</span></span>
<span data-ttu-id="0d636-141">Exempelkoden nedan använder .NET SDK för att överföra en fil.</span><span class="sxs-lookup"><span data-stu-id="0d636-141">The sample code below uses .NET SDK to upload a single file.</span></span> <span data-ttu-id="0d636-142">AccessPolicy och lokaliserare skapas och förstörs av funktionen överföringen.</span><span class="sxs-lookup"><span data-stu-id="0d636-142">The AccessPolicy and Locator are created and destroyed by the Upload function.</span></span> 


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


## <a name="upload-multiple-files-with-media-services-net-sdk"></a><span data-ttu-id="0d636-143">Överför flera filer med Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="0d636-143">Upload multiple files with Media Services .NET SDK</span></span>
<span data-ttu-id="0d636-144">Följande kod visar hur du skapar en tillgång och överför flera filer.</span><span class="sxs-lookup"><span data-stu-id="0d636-144">The following code shows how to create an asset and upload multiple files.</span></span>

<span data-ttu-id="0d636-145">Koden gör följande:</span><span class="sxs-lookup"><span data-stu-id="0d636-145">The code does the following:</span></span>

* <span data-ttu-id="0d636-146">Skapar en tom tillgång med metoden CreateEmptyAsset som definierats i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="0d636-146">Creates an empty asset using the CreateEmptyAsset method defined in the previous step.</span></span>
* <span data-ttu-id="0d636-147">Skapar en **AccessPolicy** -instans som definierar behörigheter och varaktighet för åtkomst till tillgången.</span><span class="sxs-lookup"><span data-stu-id="0d636-147">Creates an **AccessPolicy** instance that defines the permissions and duration of access to the asset.</span></span>
* <span data-ttu-id="0d636-148">Skapar en **lokaliserare** instans som ger åtkomst till tillgången.</span><span class="sxs-lookup"><span data-stu-id="0d636-148">Creates a **Locator** instance that provides access to the asset.</span></span>
* <span data-ttu-id="0d636-149">Skapar en **BlobTransferClient** instans.</span><span class="sxs-lookup"><span data-stu-id="0d636-149">Creates a **BlobTransferClient** instance.</span></span> <span data-ttu-id="0d636-150">Den här typen representerar en klient som körs på Azure-blobbar.</span><span class="sxs-lookup"><span data-stu-id="0d636-150">This type represents a client that operates on the Azure blobs.</span></span> <span data-ttu-id="0d636-151">I det här exemplet använder vi klienten övervakar överför.</span><span class="sxs-lookup"><span data-stu-id="0d636-151">In this example we use the client to monitor the upload progress.</span></span> 
* <span data-ttu-id="0d636-152">Räknar upp via filer i den angivna katalogen och skapar en **AssetFile** -instans för varje fil.</span><span class="sxs-lookup"><span data-stu-id="0d636-152">Enumerates through files in the specified directory and creates an **AssetFile** instance for each file.</span></span>
* <span data-ttu-id="0d636-153">Överför filer till Media Services med hjälp av den **UploadAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="0d636-153">Uploads the files into Media Services using the **UploadAsync** method.</span></span> 

> [!NOTE]
> <span data-ttu-id="0d636-154">Använd metoden UploadAsync så att anrop inte blockerar och filerna har överförts parallellt.</span><span class="sxs-lookup"><span data-stu-id="0d636-154">Use the UploadAsync method to ensure that the calls are not blocking and the files are uploaded in parallel.</span></span>
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

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }

    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



<span data-ttu-id="0d636-155">Tänk på följande när du hämtar ett stort antal tillgångar.</span><span class="sxs-lookup"><span data-stu-id="0d636-155">When uploading a large number of assets, consider the following.</span></span>

* <span data-ttu-id="0d636-156">Skapa en ny **CloudMediaContext** objekt per tråd.</span><span class="sxs-lookup"><span data-stu-id="0d636-156">Create a new **CloudMediaContext** object per thread.</span></span> <span data-ttu-id="0d636-157">Den **CloudMediaContext** klass är inte trådsäker.</span><span class="sxs-lookup"><span data-stu-id="0d636-157">The **CloudMediaContext** class is not thread safe.</span></span>
* <span data-ttu-id="0d636-158">Öka NumberOfConcurrentTransfers från standardvärdet 2 till ett högre värde som 5.</span><span class="sxs-lookup"><span data-stu-id="0d636-158">Increase NumberOfConcurrentTransfers from the default value of 2 to a higher value like 5.</span></span> <span data-ttu-id="0d636-159">Den här egenskapen påverkar alla förekomster av **CloudMediaContext**.</span><span class="sxs-lookup"><span data-stu-id="0d636-159">Setting this property affects all instances of **CloudMediaContext**.</span></span> 
* <span data-ttu-id="0d636-160">Hålla ParallelTransferThreadCount standardvärdet 10.</span><span class="sxs-lookup"><span data-stu-id="0d636-160">Keep ParallelTransferThreadCount at the default value of 10.</span></span>

## <span data-ttu-id="0d636-161"><a id="ingest_in_bulk"></a>Fört in tillgångar i gång med hjälp av Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="0d636-161"><a id="ingest_in_bulk"></a>Ingesting Assets in Bulk using Media Services .NET SDK</span></span>
<span data-ttu-id="0d636-162">Överföringen av stora tillgångsfiler kan utgöra en flaskhals under skapande av tillgångsinformation.</span><span class="sxs-lookup"><span data-stu-id="0d636-162">Uploading large asset files can be a bottleneck during asset creation.</span></span> <span data-ttu-id="0d636-163">Fört in tillgångar i massutskick eller ”samtidigt vill föra in” innebär att Frikoppling tillgången skapas överföringen.</span><span class="sxs-lookup"><span data-stu-id="0d636-163">Ingesting Assets in Bulk or “Bulk Ingesting”, involves decoupling asset creation from the upload process.</span></span> <span data-ttu-id="0d636-164">Skapa ett manifest (IngestManifest) som beskriver tillgången och dess associerade filer om du vill använda en grupp som vill föra in metod.</span><span class="sxs-lookup"><span data-stu-id="0d636-164">To use a bulk ingesting approach, create a manifest (IngestManifest) that describes the asset and its associated files.</span></span> <span data-ttu-id="0d636-165">Sedan använda metoden överför du väljer att ladda upp filerna i manifestet blob-behållaren.</span><span class="sxs-lookup"><span data-stu-id="0d636-165">Then use the upload method of your choice to upload the associated files to the manifest’s blob container.</span></span> <span data-ttu-id="0d636-166">Microsoft Azure Media Services bevakar blob-behållare som är associerade med manifestet.</span><span class="sxs-lookup"><span data-stu-id="0d636-166">Microsoft Azure Media Services watches the blob container associated with the manifest.</span></span> <span data-ttu-id="0d636-167">När en fil har överförts till blob-behållare, Slutför Microsoft Azure Media Services tillgång skapas baserat på konfigurationen av tillgången i manifestet (IngestManifestAsset).</span><span class="sxs-lookup"><span data-stu-id="0d636-167">Once a file is uploaded to the blob container, Microsoft Azure Media Services completes the asset creation based on the configuration of the asset in the manifest (IngestManifestAsset).</span></span>

<span data-ttu-id="0d636-168">Om du vill skapa en ny IngestManifest anropa metoden skapa som exponeras av IngestManifests samlingen på CloudMediaContext.</span><span class="sxs-lookup"><span data-stu-id="0d636-168">To create a new IngestManifest call the Create method exposed by the IngestManifests collection on the CloudMediaContext.</span></span> <span data-ttu-id="0d636-169">Den här metoden skapar en ny IngestManifest med manifestnamnet som du anger.</span><span class="sxs-lookup"><span data-stu-id="0d636-169">This method will create a new IngestManifest with the manifest name you provide.</span></span>

    IIngestManifest manifest = context.IngestManifests.Create(name);

<span data-ttu-id="0d636-170">Skapa tillgångar som ska associeras med flesta IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="0d636-170">Create the assets that will be associated with the bulk IngestManifest.</span></span> <span data-ttu-id="0d636-171">Konfigurera önskade krypteringsalternativ för tillgångsinformation för att föra in samtidigt.</span><span class="sxs-lookup"><span data-stu-id="0d636-171">Configure the desired encryption options on the asset for bulk ingesting.</span></span>

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

<span data-ttu-id="0d636-172">En IngestManifestAsset associerar en tillgång med bulk IngestManifest för att föra in samtidigt.</span><span class="sxs-lookup"><span data-stu-id="0d636-172">An IngestManifestAsset associates an Asset with a bulk IngestManifest for bulk ingesting.</span></span> <span data-ttu-id="0d636-173">Här associerar även AssetFiles som utgör varje tillgång.</span><span class="sxs-lookup"><span data-stu-id="0d636-173">It also associates the AssetFiles that will make up each Asset.</span></span> <span data-ttu-id="0d636-174">Använd Create-metoden för att skapa en IngestManifestAsset på serverkontext.</span><span class="sxs-lookup"><span data-stu-id="0d636-174">To create an IngestManifestAsset, use the Create method on the server context.</span></span>

<span data-ttu-id="0d636-175">Exemplet nedan visar att lägga till två nya IngestManifestAssets som kopplar två tillgångar skapat för flesta mata in manifestet.</span><span class="sxs-lookup"><span data-stu-id="0d636-175">The following example demonstrates adding two new IngestManifestAssets that associate the two assets previously created to the bulk ingest manifest.</span></span> <span data-ttu-id="0d636-176">Varje IngestManifestAsset associerar även en uppsättning filer som överförs för varje tillgång vid massinläsning vill föra in.</span><span class="sxs-lookup"><span data-stu-id="0d636-176">Each IngestManifestAsset also associates a set of files that will be uploaded for each asset during bulk ingesting.</span></span>  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;

    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });

<span data-ttu-id="0d636-177">Du kan använda en hög hastighet klientprogrammet kan överföra tillgångsfiler till blob storage-behållare URI som tillhandahålls av den **IIngestManifest.BlobStorageUriForUpload** -egenskapen för IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="0d636-177">You can use any high speed client application capable of uploading the asset files to the blob storage container URI provided by the **IIngestManifest.BlobStorageUriForUpload** property of the IngestManifest.</span></span> <span data-ttu-id="0d636-178">En tjänst för överföringen av viktiga hög hastighet är [Aspera på begäran för Azure-programmet](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6).</span><span class="sxs-lookup"><span data-stu-id="0d636-178">One notable high speed upload service is [Aspera On Demand for Azure Application](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6).</span></span> <span data-ttu-id="0d636-179">Du kan också skriva kod för att överföra tillgångar filerna som visas i följande kodexempel.</span><span class="sxs-lookup"><span data-stu-id="0d636-179">You can also write code to upload the assets files as shown in the following code example.</span></span>

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

<span data-ttu-id="0d636-180">Koden för uppladdning av tillgångsfiler för används i det här avsnittet visas i följande kodexempel.</span><span class="sxs-lookup"><span data-stu-id="0d636-180">The code for uploading the asset files for the sample used in this topic is shown in the following code example.</span></span>

    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);


<span data-ttu-id="0d636-181">Du kan se förloppet för den grupp vill föra in för alla resurser som är associerade med en **IngestManifest** genom att avsöka egenskapen statistik för den **IngestManifest**.</span><span class="sxs-lookup"><span data-stu-id="0d636-181">You can determine the progress of the bulk ingesting for all assets associated with an **IngestManifest** by polling the Statistics property of the **IngestManifest**.</span></span> <span data-ttu-id="0d636-182">För att kunna uppdatera statusinformation, måste du använda en ny **CloudMediaContext** varje gång du avsöka egenskapen statistik.</span><span class="sxs-lookup"><span data-stu-id="0d636-182">In order to update progress information, you must use a new **CloudMediaContext** each time you poll the Statistics property.</span></span>

<span data-ttu-id="0d636-183">Exemplet nedan visar avsöker en IngestManifest av dess **Id**.</span><span class="sxs-lookup"><span data-stu-id="0d636-183">The following example demonstrates polling an IngestManifest by its **Id**.</span></span>

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



## <a name="upload-files-using-net-sdk-extensions"></a><span data-ttu-id="0d636-184">Överföra filer med hjälp av .NET SDK-tillägg</span><span class="sxs-lookup"><span data-stu-id="0d636-184">Upload files using .NET SDK Extensions</span></span>
<span data-ttu-id="0d636-185">Exemplet nedan visar hur du laddar upp en enstaka fil med .NET SDK-tillägg.</span><span class="sxs-lookup"><span data-stu-id="0d636-185">The example below shows how to upload a single file using .NET SDK Extensions.</span></span> <span data-ttu-id="0d636-186">I det här fallet den **CreateFromFile** metod används, men den asynkrona versionen är också tillgänglig (**CreateFromFileAsync**).</span><span class="sxs-lookup"><span data-stu-id="0d636-186">In this case the **CreateFromFile** method is used, but the asynchronous version is also available (**CreateFromFileAsync**).</span></span> <span data-ttu-id="0d636-187">Den **CreateFromFile** metoden kan du ange filnamnet, kryptering och ett återanrop för att rapportera filen Överföringsförlopp.</span><span class="sxs-lookup"><span data-stu-id="0d636-187">The **CreateFromFile** method lets you specify the file name, encryption option, and a callback in order to report the upload progress of the file.</span></span>

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

<span data-ttu-id="0d636-188">I följande exempel anropar UploadFile funktion och anger lagringskryptering som alternativ för skapande av tillgångsinformation.</span><span class="sxs-lookup"><span data-stu-id="0d636-188">The following example calls UploadFile function and specifies storage encryption as the asset creation option.</span></span>  

    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);

## <a name="next-steps"></a><span data-ttu-id="0d636-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0d636-189">Next steps</span></span>

<span data-ttu-id="0d636-190">Du kan nu koda överförda tillgångar.</span><span class="sxs-lookup"><span data-stu-id="0d636-190">You can now encode your uploaded assets.</span></span> <span data-ttu-id="0d636-191">Mer information finns i [Koda tillgångar](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="0d636-191">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="0d636-192">Du kan också använda Azure Functions för att utlösa ett kodningsjobb baserat på en fil som skickas till den konfigurerade behållaren.</span><span class="sxs-lookup"><span data-stu-id="0d636-192">You can also use Azure Functions to trigger an encoding job based on a file arriving in the configured container.</span></span> <span data-ttu-id="0d636-193">Mer information finns i [det här exemplet](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="0d636-193">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="0d636-194">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="0d636-194">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0d636-195">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="0d636-195">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a><span data-ttu-id="0d636-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0d636-196">Next step</span></span>
<span data-ttu-id="0d636-197">Nu när du har överfört en tillgång till Media Services, gå till den [hur du hämtar en Medieprocessor] [ How to Get a Media Processor] avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0d636-197">Now that you have uploaded an asset to Media Services, go to the [How to Get a Media Processor][How to Get a Media Processor] topic.</span></span>

[How to Get a Media Processor]: media-services-get-media-processor.md

