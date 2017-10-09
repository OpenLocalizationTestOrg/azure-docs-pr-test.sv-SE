---
title: "aaaGet igång med att leverera innehåll på begäran med hjälp av .NET | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom hello stegen för att implementera ett program för leverans av på begäran med Azure Media Services med hjälp av .NET."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 388b8928-9aa9-46b1-b60a-a918da75bd7b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 4ca9394bd581e1d9062e5a008a410b2c058e017e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a><span data-ttu-id="839da-103">Kom igång med att leverera innehåll på begäran med hjälp av .NET SDK</span><span class="sxs-lookup"><span data-stu-id="839da-103">Get started with delivering content on demand using .NET SDK</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="839da-104">Den här självstudiekursen vägleder dig genom hello stegen för att implementera en grundläggande Video-on-Demand (VoD) innehållsleverans tjänst med Azure Media Services (AMS)-program med hjälp av hello Azure Media Services .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="839da-104">This tutorial walks you through hello steps of implementing a basic Video-on-Demand (VoD) content delivery service with Azure Media Services (AMS) application using hello Azure Media Services .NET SDK.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="839da-105">Krav</span><span class="sxs-lookup"><span data-stu-id="839da-105">Prerequisites</span></span>

<span data-ttu-id="839da-106">hello följande är obligatoriska toocomplete hello kursen:</span><span class="sxs-lookup"><span data-stu-id="839da-106">hello following are required toocomplete hello tutorial:</span></span>

* <span data-ttu-id="839da-107">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="839da-107">An Azure account.</span></span> <span data-ttu-id="839da-108">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="839da-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="839da-109">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="839da-109">A Media Services account.</span></span> <span data-ttu-id="839da-110">toocreate Media Services-konto finns [hur tooCreate Media Services-konto](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="839da-110">toocreate a Media Services account, see [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="839da-111">.NET Framework 4.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="839da-111">.NET Framework 4.0 or later.</span></span>
* <span data-ttu-id="839da-112">Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="839da-112">Visual Studio.</span></span>

<span data-ttu-id="839da-113">Den här självstudiekursen innehåller hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="839da-113">This tutorial includes hello following tasks:</span></span>

1. <span data-ttu-id="839da-114">Starta strömmande slutpunkten (med hello Azure-portalen).</span><span class="sxs-lookup"><span data-stu-id="839da-114">Start streaming endpoint (using hello Azure portal).</span></span>
2. <span data-ttu-id="839da-115">Skapar och konfigurerar ett Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="839da-115">Create and configure a Visual Studio project.</span></span>
3. <span data-ttu-id="839da-116">Ansluta toohello Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="839da-116">Connect toohello Media Services account.</span></span>
2. <span data-ttu-id="839da-117">Överföra en videofil.</span><span class="sxs-lookup"><span data-stu-id="839da-117">Upload a video file.</span></span>
3. <span data-ttu-id="839da-118">Koda hello källfilen till en uppsättning MP4-filer med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="839da-118">Encode hello source file into a set of adaptive bitrate MP4 files.</span></span>
4. <span data-ttu-id="839da-119">Publicera hello tillgången och get strömning och progressiv nedladdning URL: er.</span><span class="sxs-lookup"><span data-stu-id="839da-119">Publish hello asset and get streaming and progressive download URLs.</span></span>  
5. <span data-ttu-id="839da-120">Spela upp ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="839da-120">Play your content.</span></span>

## <a name="overview"></a><span data-ttu-id="839da-121">Översikt</span><span class="sxs-lookup"><span data-stu-id="839da-121">Overview</span></span>
<span data-ttu-id="839da-122">Den här självstudiekursen vägleder dig genom hello stegen för att implementera ett program för leverans av Video-on-Demand (VoD) med Azure Media Services (AMS) SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="839da-122">This tutorial walks you through hello steps of implementing a Video-on-Demand (VoD) content delivery application using Azure Media Services (AMS) SDK for .NET.</span></span>

<span data-ttu-id="839da-123">hello självstudierna innehåller hello grundläggande Media Services-arbetsflödet och hello vanligaste programmeringsobjekt och uppgifter som krävs för Media Services-utveckling.</span><span class="sxs-lookup"><span data-stu-id="839da-123">hello tutorial introduces hello basic Media Services workflow and hello most common programming objects and tasks required for Media Services development.</span></span> <span data-ttu-id="839da-124">På hello kursen hello är klar kan ska du kunna toostream eller progressivt hämta en exempelmediefil som överförs, kodat och hämtat.</span><span class="sxs-lookup"><span data-stu-id="839da-124">At hello completion of hello tutorial, you will be able toostream or progressively download a sample media file that you uploaded, encoded, and downloaded.</span></span>

### <a name="ams-model"></a><span data-ttu-id="839da-125">AMS-modell</span><span class="sxs-lookup"><span data-stu-id="839da-125">AMS model</span></span>

<span data-ttu-id="839da-126">hello följande bild visar några av de vanligaste hello objekt när du utvecklar program VoD mot hello Media Services OData-modellen.</span><span class="sxs-lookup"><span data-stu-id="839da-126">hello following image shows some of hello most commonly used objects when developing VoD applications against hello Media Services OData model.</span></span>

<span data-ttu-id="839da-127">Klicka på hello avbildningen tooview den full storlek.</span><span class="sxs-lookup"><span data-stu-id="839da-127">Click hello image tooview it full size.</span></span>  

<a href="./media/media-services-dotnet-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-dotnet-get-started/media-services-overview-object-model-small.png"></a> 

<span data-ttu-id="839da-128">Du kan visa hela modellen hello [här](https://media.windows.net/API/$metadata?api-version=2.15).</span><span class="sxs-lookup"><span data-stu-id="839da-128">You can view hello whole model [here](https://media.windows.net/API/$metadata?api-version=2.15).</span></span>  

## <a name="start-streaming-endpoints-using-hello-azure-portal"></a><span data-ttu-id="839da-129">Starta strömningsslutpunkter med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="839da-129">Start streaming endpoints using hello Azure portal</span></span>

<span data-ttu-id="839da-130">När du arbetar med Azure Media Services är en av hello vanligaste scenarierna att leverera video via strömning med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="839da-130">When working with Azure Media Services one of hello most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="839da-131">Media Services tillhandahåller en dynamisk paketering som gör att du toodeliver dina MP4-kodade innehåll i strömningsformat som stöds av Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, utan att behöva toostore tillsammans med anpassad bithastighet versioner av var och en av dessa strömningsformat.</span><span class="sxs-lookup"><span data-stu-id="839da-131">Media Services provides dynamic packaging, which allows you toodeliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having toostore pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="839da-132">När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="839da-132">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="839da-133">toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="839da-133">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span>

<span data-ttu-id="839da-134">toostart Hej strömmande slutpunkten, hello följande:</span><span class="sxs-lookup"><span data-stu-id="839da-134">toostart hello streaming endpoint, do hello following:</span></span>

1. <span data-ttu-id="839da-135">Logga in på hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="839da-135">Log in at hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="839da-136">Streaming slutpunkter på hello inställningar i fönstret.</span><span class="sxs-lookup"><span data-stu-id="839da-136">In hello Settings window, click Streaming endpoints.</span></span>
3. <span data-ttu-id="839da-137">Klicka på hello standard strömmande slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="839da-137">Click hello default streaming endpoint.</span></span>

    <span data-ttu-id="839da-138">hello visas standard information om den STRÖMNINGSSLUTPUNKT.</span><span class="sxs-lookup"><span data-stu-id="839da-138">hello DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="839da-139">Klicka på hello Start-ikonen.</span><span class="sxs-lookup"><span data-stu-id="839da-139">Click hello Start icon.</span></span>
5. <span data-ttu-id="839da-140">Klicka på hello spara knappen toosave ändringarna.</span><span class="sxs-lookup"><span data-stu-id="839da-140">Click hello Save button toosave your changes.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="839da-141">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="839da-141">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="839da-142">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="839da-142">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="839da-143">Skapa en ny mapp (mappen kan finnas var som helst på den lokala enheten) och kopiera en .mp4-fil som du vill att tooencode och dataströmmen eller progressivt hämta.</span><span class="sxs-lookup"><span data-stu-id="839da-143">Create a new folder (folder can be anywhere on your local drive) and copy an .mp4 file that you want tooencode and stream or progressively download.</span></span> <span data-ttu-id="839da-144">I det här exemplet används sökvägen för hello ”C:\VideoFiles”.</span><span class="sxs-lookup"><span data-stu-id="839da-144">In this example, hello "C:\VideoFiles" path is used.</span></span>

## <a name="connect-toohello-media-services-account"></a><span data-ttu-id="839da-145">Ansluta toohello Media Services-konto</span><span class="sxs-lookup"><span data-stu-id="839da-145">Connect toohello Media Services account</span></span>

<span data-ttu-id="839da-146">När du använder Media Services med .NET, måste du använda hello **CloudMediaContext** klass för de flesta Media Services-programmeringsuppgifter: ansluta tooMedia Services-konto, skapa, uppdatera, åtkomst till och ta bort följande hello objekt: tillgångar, tillgångsfiler, jobb, åtkomstprinciper, positionerare o.s.v..</span><span class="sxs-lookup"><span data-stu-id="839da-146">When using Media Services with .NET, you must use hello **CloudMediaContext** class for most Media Services programming tasks: connecting tooMedia Services account; creating, updating, accessing, and deleting hello following objects: assets, asset files, jobs, access policies, locators, etc.</span></span>

<span data-ttu-id="839da-147">Skriv över hello programklass som standard med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="839da-147">Overwrite hello default Program class with hello following code.</span></span> <span data-ttu-id="839da-148">hello kod visar hur tooread hello anslutningsvärdena från hello App.config-fil och toocreate hello **CloudMediaContext** objekt i ordning tooconnect tooMedia Services.</span><span class="sxs-lookup"><span data-stu-id="839da-148">hello code demonstrates how tooread hello connection values from hello App.config file and how toocreate hello **CloudMediaContext** object in order tooconnect tooMedia Services.</span></span> <span data-ttu-id="839da-149">Mer information finns i [ansluter toohello Media Services API](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="839da-149">For more information, see [connecting toohello Media Services API](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

<span data-ttu-id="839da-150">Se till att tooupdate hello filen namnet och sökvägen toowhere du har din mediefilen.</span><span class="sxs-lookup"><span data-stu-id="839da-150">Make sure tooupdate hello file name and path toowhere you have your media file.</span></span>

<span data-ttu-id="839da-151">Hej **Main** anropar metoder som definieras ytterligare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="839da-151">hello **Main** function calls methods that will be defined further in this section.</span></span>

> [!NOTE]
> <span data-ttu-id="839da-152">Du kommer att få kompileringsfel tills du lägger till definitioner för alla hello-funktioner.</span><span class="sxs-lookup"><span data-stu-id="839da-152">You will be getting compilation errors until you add definitions for all hello functions.</span></span>

    class Program
    {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static CloudMediaContext _context = null;

        static void Main(string[] args)
        {
        try
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            // Add calls toomethods defined in this section.
            // Make sure tooupdate hello file name and path toowhere you have your media file.
            IAsset inputAsset =
            UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

            IAsset encodedAsset =
            EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

            PublishAssetGetURLs(encodedAsset);
        }
        catch (Exception exception)
        {
            // Parse hello XML error message in hello Media Services response and create a new
            // exception with its content.
            exception = MediaServicesExceptionParser.Parse(exception);

            Console.Error.WriteLine(exception.Message);
        }
        finally
        {
            Console.ReadLine();
        }
        }
    }

## <a name="create-a-new-asset-and-upload-a-video-file"></a><span data-ttu-id="839da-153">Skapa en ny tillgång och ladda upp en videofil</span><span class="sxs-lookup"><span data-stu-id="839da-153">Create a new asset and upload a video file</span></span>

<span data-ttu-id="839da-154">I Media Services överför du (eller för in) dina digitala filer till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="839da-154">In Media Services, you upload (or ingest) your digital files into an asset.</span></span> <span data-ttu-id="839da-155">Hej **tillgången** entitet kan innehålla video, ljud, bilder, miniatyrsamlingar, textspår och textning filer (och hello metadata om dessa filer.)  När hello filerna har överförts lagras innehållet på ett säkert sätt i hello molnet för ytterligare bearbetning och strömning.</span><span class="sxs-lookup"><span data-stu-id="839da-155">hello **Asset** entity can contain video, audio, images, thumbnail collections, text tracks, and closed caption files (and hello metadata about these files.)  Once hello files are uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span> <span data-ttu-id="839da-156">hello filerna i hello tillgången kallas **Tillgångsfiler**.</span><span class="sxs-lookup"><span data-stu-id="839da-156">hello files in hello asset are called **Asset Files**.</span></span>

<span data-ttu-id="839da-157">Hej **UploadFile** metod som definieras nedan kallas **CreateFromFile** (definieras i .NET SDK-tillägg).</span><span class="sxs-lookup"><span data-stu-id="839da-157">hello **UploadFile** method defined below calls **CreateFromFile** (defined in .NET SDK Extensions).</span></span> <span data-ttu-id="839da-158">**CreateFromFile** skapar en ny tillgång till vilka hello angivna källfilen överförs.</span><span class="sxs-lookup"><span data-stu-id="839da-158">**CreateFromFile** creates a new asset into which hello specified source file is uploaded.</span></span>

<span data-ttu-id="839da-159">Hej **CreateFromFile** metoden tar **AssetCreationOptions** kan du ange något av följande alternativ för skapande av tillgångsinformation hello:</span><span class="sxs-lookup"><span data-stu-id="839da-159">hello **CreateFromFile** method takes **AssetCreationOptions** which lets you specify one of hello following asset creation options:</span></span>

* <span data-ttu-id="839da-160">**Ingen** – Ingen kryptering används.</span><span class="sxs-lookup"><span data-stu-id="839da-160">**None** - No encryption is used.</span></span> <span data-ttu-id="839da-161">Detta är standardvärdet för hello.</span><span class="sxs-lookup"><span data-stu-id="839da-161">This is hello default value.</span></span> <span data-ttu-id="839da-162">Observera att när du använder det här alternativet skyddas inte innehållet under överföring eller i vila i lagringsutrymmet.</span><span class="sxs-lookup"><span data-stu-id="839da-162">Note that when using this option, your content is not protected in transit or at rest in storage.</span></span>
  <span data-ttu-id="839da-163">Använd det här alternativet om du planerar toodeliver en MP4 med progressivt nedladdning.</span><span class="sxs-lookup"><span data-stu-id="839da-163">If you plan toodeliver an MP4 using progressive download, use this option.</span></span>
* <span data-ttu-id="839da-164">**StorageEncrypted** -och använda det här alternativet tooencrypt innehållet lokalt med hjälp av Advanced Encryption Standard (AES) 256-bitars kryptering, som sedan överför tooAzure lagring där den lagras krypterat i vila.</span><span class="sxs-lookup"><span data-stu-id="839da-164">**StorageEncrypted** - Use this option tooencrypt your clear content locally using Advanced Encryption Standard (AES)-256 bit encryption, which then uploads it tooAzure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="839da-165">Tillgångar som skyddas med Lagringskryptering okrypterad och placeras i en krypterad fil system tidigare tooencoding och eventuellt omkrypterade tidigare toouploading tillbaka som en ny utdatatillgång automatiskt.</span><span class="sxs-lookup"><span data-stu-id="839da-165">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior tooencoding, and optionally re-encrypted prior toouploading back as a new output asset.</span></span> <span data-ttu-id="839da-166">hello primära användningsfall för Lagringskryptering är om du vill toosecure hög kvalitet inkommande mediefilerna med stark kryptering i vila på disk.</span><span class="sxs-lookup"><span data-stu-id="839da-166">hello primary use case for Storage Encryption is when you want toosecure your high-quality input media files with strong encryption at rest on disk.</span></span>
* <span data-ttu-id="839da-167">**CommonEncryptionProtected** – Använd det här alternativet om du överför innehåll som redan har krypterats och som skyddas med vanlig kryptering eller PlayReady DRM (till exempel Smooth Streaming som skyddas med PlayReady DRM).</span><span class="sxs-lookup"><span data-stu-id="839da-167">**CommonEncryptionProtected** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="839da-168">**EnvelopeEncryptionProtected** – Använd det här alternativet om du överför HLS som krypterats med AES.</span><span class="sxs-lookup"><span data-stu-id="839da-168">**EnvelopeEncryptionProtected** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="839da-169">Observera att hello filer måste har kodats och krypterats av Transform Manager.</span><span class="sxs-lookup"><span data-stu-id="839da-169">Note that hello files must have been encoded and encrypted by Transform Manager.</span></span>

<span data-ttu-id="839da-170">Hej **CreateFromFile** metod kan du även ange ett återanrop i ordning tooreport hello Överföringsförlopp hello-filen.</span><span class="sxs-lookup"><span data-stu-id="839da-170">hello **CreateFromFile** method also lets you specify a callback in order tooreport hello upload progress of hello file.</span></span>

<span data-ttu-id="839da-171">I följande exempel hello, anger vi **ingen** för hello tillgångsalternativen.</span><span class="sxs-lookup"><span data-stu-id="839da-171">In hello following example, we specify **None** for hello asset options.</span></span>

<span data-ttu-id="839da-172">Lägg till följande metodklassen toohello programmet hello.</span><span class="sxs-lookup"><span data-stu-id="839da-172">Add hello following method toohello Program class.</span></span>

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


## <a name="encode-hello-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a><span data-ttu-id="839da-173">Koda hello källfilen till en uppsättning MP4-filer med anpassningsbar bithastighet</span><span class="sxs-lookup"><span data-stu-id="839da-173">Encode hello source file into a set of adaptive bitrate MP4 files</span></span>
<span data-ttu-id="839da-174">När du har fört in tillgångar i Media Services kan media kodas, transmuxed, en vattenstämpel, och så vidare innan de skickas tooclients.</span><span class="sxs-lookup"><span data-stu-id="839da-174">After ingesting assets into Media Services, media can be encoded, transmuxed, watermarked, and so on, before it is delivered tooclients.</span></span> <span data-ttu-id="839da-175">Dessa aktiviteter schemaläggs och körs mot flera bakgrund rollen instanser tooensure hög prestanda och tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="839da-175">These activities are scheduled and run against multiple background role instances tooensure high performance and availability.</span></span> <span data-ttu-id="839da-176">Aktiviteterna kallas jobb och varje jobb består av atomiska uppgifter som hello faktiska arbetet i tillgångsfilen hello.</span><span class="sxs-lookup"><span data-stu-id="839da-176">These activities are called Jobs, and each Job is composed of atomic Tasks that do hello actual work on hello Asset file.</span></span>

<span data-ttu-id="839da-177">Som tidigare nämnts, när du arbetar med Azure Media Services ett av de vanligaste scenarierna för hello levererar strömning tooyour klienter med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="839da-177">As was mentioned earlier, when working with Azure Media Services, one of hello most common scenarios is delivering adaptive bitrate streaming tooyour clients.</span></span> <span data-ttu-id="839da-178">Media Services kan dynamiskt Paketera en uppsättning MP4-filer med anpassningsbar bithastighet till en hello följande format: HTTP Live Streaming (HLS), Smooth Streaming och MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="839da-178">Media Services can dynamically package a set of adaptive bitrate MP4 files into one of hello following formats: HTTP Live Streaming (HLS), Smooth Streaming, and MPEG DASH.</span></span>

<span data-ttu-id="839da-179">tootake nytta av dynamisk paketering behöver du tooencode eller omkoda din mezzaninfil (källa) till en uppsättning med anpassningsbar bithastighet MP4-filer eller Smooth Streaming-filer.</span><span class="sxs-lookup"><span data-stu-id="839da-179">tootake advantage of dynamic packaging, you need tooencode or transcode your mezzanine (source) file into a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files.</span></span>  

<span data-ttu-id="839da-180">hello följande kod visar hur toosubmit kodning jobbet.</span><span class="sxs-lookup"><span data-stu-id="839da-180">hello following code shows how toosubmit an encoding job.</span></span> <span data-ttu-id="839da-181">hello jobbet innehåller en uppgift som anger tootranscode hello mezzaninfil till en uppsättning med anpassningsbar bithastighet MP4s **Media Encoder Standard**.</span><span class="sxs-lookup"><span data-stu-id="839da-181">hello job contains one task that specifies tootranscode hello mezzanine file into a set of adaptive bitrate MP4s using **Media Encoder Standard**.</span></span> <span data-ttu-id="839da-182">hello koden skickar hello jobbet och väntar tills den är klar.</span><span class="sxs-lookup"><span data-stu-id="839da-182">hello code submits hello job and waits until it is completed.</span></span>

<span data-ttu-id="839da-183">När hello jobbet är slutfört, skulle du vara kan toostream din tillgång eller progressivt hämta MP4-filer som skapades på grund av omkodningen.</span><span class="sxs-lookup"><span data-stu-id="839da-183">Once hello job is completed, you would be able toostream your asset or progressively download MP4 files that were created as a result of transcoding.</span></span>

<span data-ttu-id="839da-184">Lägg till följande metodklassen toohello programmet hello.</span><span class="sxs-lookup"><span data-stu-id="839da-184">Add hello following method toohello Program class.</span></span>

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {

        // Prepare a job with a single task tootranscode hello specified asset
        // into a multi-bitrate asset.

        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "Adaptive Streaming",
            asset,
            "Adaptive Bitrate MP4",
            options);

        Console.WriteLine("Submitting transcoding job...");


        // Submit hello job and wait until it is completed.
        job.Submit();

        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;

        Console.WriteLine("Transcoding job finished.");

        IAsset outputAsset = job.OutputMediaAssets[0];

        return outputAsset;
    }

## <a name="publish-hello-asset-and-get-urls-for-streaming-and-progressive-download"></a><span data-ttu-id="839da-185">Publicera hello tillgången och få URL: er för strömning och progressiv hämtning</span><span class="sxs-lookup"><span data-stu-id="839da-185">Publish hello asset and get URLs for streaming and progressive download</span></span>

<span data-ttu-id="839da-186">toostream eller hämta en tillgång, du behöver för ”publicera” den genom att skapa en positionerare.</span><span class="sxs-lookup"><span data-stu-id="839da-186">toostream or download an asset, you first need too"publish" it by creating a locator.</span></span> <span data-ttu-id="839da-187">Positionerare ger åtkomst toofiles i hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="839da-187">Locators provide access toofiles contained in hello asset.</span></span> <span data-ttu-id="839da-188">Media Services stöder två typer av positionerare: OnDemandOrigin-positionerare som används toostream media (till exempel MPEG DASH, HLS eller Smooth Streaming) och signatur åtkomst (SAS)-positionerare används toodownload mediefiler (för mer information om SAS-positionerare finns [detta](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blogg).</span><span class="sxs-lookup"><span data-stu-id="839da-188">Media Services supports two types of locators: OnDemandOrigin locators, used toostream media (for example, MPEG DASH, HLS, or Smooth Streaming) and Access Signature (SAS) locators, used toodownload media files (for more information about SAS locators see [this](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog).</span></span>

### <a name="some-details-about-url-formats"></a><span data-ttu-id="839da-189">Information om URL-format</span><span class="sxs-lookup"><span data-stu-id="839da-189">Some details about URL formats</span></span>

<span data-ttu-id="839da-190">Du kan skapa hello URL: er som skulle vara används toostream eller hämta dina filer när du har skapat hello lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="839da-190">After you create hello locators, you can build hello URLs that would be used toostream or download your files.</span></span> <span data-ttu-id="839da-191">hello-exempel i den här självstudiekursen kommer utdata URL: er som du kan klistra in i lämplig webbläsare.</span><span class="sxs-lookup"><span data-stu-id="839da-191">hello sample in this tutorial will output URLs that you can paste in appropriate browsers.</span></span> <span data-ttu-id="839da-192">Det här avsnittet ger bara korta exempel på hur olika format ser ut.</span><span class="sxs-lookup"><span data-stu-id="839da-192">This section just gives short examples of what different formats look like.</span></span>

#### <a name="a-streaming-url-for-mpeg-dash-has-hello-following-format"></a><span data-ttu-id="839da-193">En strömmande URL för MPEG DASH har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="839da-193">A streaming URL for MPEG DASH has hello following format:</span></span>

<span data-ttu-id="839da-194">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=mpd-time-csf)**</span><span class="sxs-lookup"><span data-stu-id="839da-194">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=mpd-time-csf)**</span></span>

#### <a name="a-streaming-url-for-hls-has-hello-following-format"></a><span data-ttu-id="839da-195">En strömmande URL för HLS har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="839da-195">A streaming URL for HLS has hello following format:</span></span>

<span data-ttu-id="839da-196">{namn på slutpunkt för direktuppspelning-namn på mediaservicekonto}.streaming.mediaservices.windows.net/{lokalisator-ID}/{filnamn}.ism/Manifest**(format=m3u8-aapl)**</span><span class="sxs-lookup"><span data-stu-id="839da-196">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=m3u8-aapl)**</span></span>

#### <a name="a-streaming-url-for-smooth-streaming-has-hello-following-format"></a><span data-ttu-id="839da-197">En strömmande URL för Smooth Streaming har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="839da-197">A streaming URL for Smooth Streaming has hello following format:</span></span>

<span data-ttu-id="839da-198">{namn på slutpunkt för direktuppspelning-namn på mediaservicekonto}.streaming.mediaservices.windows.net/lokalisator-ID}/{filnamn}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="839da-198">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>


#### <a name="a-sas-url-used-toodownload-files-has-hello-following-format"></a><span data-ttu-id="839da-199">En SAS-URL som används för toodownload filer har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="839da-199">A SAS URL used toodownload files has hello following format:</span></span>

<span data-ttu-id="839da-200">{blob-behållarnamn}/{tillgångsnamn}/{filnamn}/{SAS-signatur}</span><span class="sxs-lookup"><span data-stu-id="839da-200">{blob container name}/{asset name}/{file name}/{SAS signature}</span></span>

<span data-ttu-id="839da-201">Media Services .NET SDK-tillägg ger praktiska hjälpmetoder som returnerar formaterade URL: er för hello publicerade tillgången.</span><span class="sxs-lookup"><span data-stu-id="839da-201">Media Services .NET SDK extensions provide convenient helper methods that return formatted URLs for hello published asset.</span></span>

<span data-ttu-id="839da-202">hello följande kod använder .NET SDK-tilläggen toocreate positionerare och tooget strömning och progressiv överföring URL: er.</span><span class="sxs-lookup"><span data-stu-id="839da-202">hello following code uses .NET SDK Extensions toocreate locators and tooget streaming and progressive download URLs.</span></span> <span data-ttu-id="839da-203">hello koden visar även hur toodownload filer tooa lokal mapp.</span><span class="sxs-lookup"><span data-stu-id="839da-203">hello code also shows how toodownload files tooa local folder.</span></span>

<span data-ttu-id="839da-204">Lägg till följande metodklassen toohello programmet hello.</span><span class="sxs-lookup"><span data-stu-id="839da-204">Add hello following method toohello Program class.</span></span>

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish hello output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get hello Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and hello Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get hello URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  hello streaming URLs.
        Console.WriteLine("Use hello following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display hello URLs for progressive download.
        Console.WriteLine("Use hello following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download hello output asset tooa local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files tooa local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

## <a name="test-by-playing-your-content"></a><span data-ttu-id="839da-205">Testa genom att spela upp ditt innehåll</span><span class="sxs-lookup"><span data-stu-id="839da-205">Test by playing your content</span></span>

<span data-ttu-id="839da-206">När du kör hello-programmet som definieras i föregående avsnitt i hello hello URL: er liknande toohello följande visas i hello konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="839da-206">Once you run hello program defined in hello previous section, hello URLs similar toohello following will be displayed in hello console window.</span></span>

<span data-ttu-id="839da-207">URL:er för anpassningsbar strömning:</span><span class="sxs-lookup"><span data-stu-id="839da-207">Adaptive streaming URLs:</span></span>

<span data-ttu-id="839da-208">Smooth Streaming</span><span class="sxs-lookup"><span data-stu-id="839da-208">Smooth Streaming</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

<span data-ttu-id="839da-209">HLS</span><span class="sxs-lookup"><span data-stu-id="839da-209">HLS</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

<span data-ttu-id="839da-210">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="839da-210">MPEG DASH</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

<span data-ttu-id="839da-211">URL:er för progressiv nedladdning (ljud och video).</span><span class="sxs-lookup"><span data-stu-id="839da-211">Progressive download URLs (audio and video).</span></span>

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


<span data-ttu-id="839da-212">toostream videon, klistra in URL: en i hello URL: en textruta i hello [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="839da-212">toostream your video, paste your URL in hello URL textbox in hello [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

<span data-ttu-id="839da-213">tootest progressiv hämtning, klistra in en URL i en webbläsare (till exempel Internet Explorer, Chrome eller Safari).</span><span class="sxs-lookup"><span data-stu-id="839da-213">tootest progressive download, paste a URL into a browser (for example, Internet Explorer, Chrome, or Safari).</span></span>

<span data-ttu-id="839da-214">Mer information finns i följande avsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="839da-214">For more information, see hello following topics:</span></span>

- [<span data-ttu-id="839da-215">Spela upp ditt innehåll med befintliga spelare</span><span class="sxs-lookup"><span data-stu-id="839da-215">Playing your content with existing players</span></span>](media-services-playback-content-with-existing-players.md)
- [<span data-ttu-id="839da-216">Utveckla videospelarprogram</span><span class="sxs-lookup"><span data-stu-id="839da-216">Develop video player applications</span></span>](media-services-develop-video-players.md)
- [<span data-ttu-id="839da-217">Bädda in MPEG-DASH-anpassad direktuppspelad video i ett HTML5-program med DASH.js</span><span class="sxs-lookup"><span data-stu-id="839da-217">Embedding a MPEG-DASH Adaptive Streaming Video in an HTML5 Application with DASH.js</span></span>](media-services-embed-mpeg-dash-in-html5.md)

## <a name="download-sample"></a><span data-ttu-id="839da-218">Hämta exempel</span><span class="sxs-lookup"><span data-stu-id="839da-218">Download sample</span></span>
<span data-ttu-id="839da-219">hello följande kodexempel innehåller hello kod som du skapade i den här kursen: [exempel](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="839da-219">hello following code sample contains hello code that you created in this tutorial: [sample](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="839da-220">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="839da-220">Next Steps</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="839da-221">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="839da-221">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



<!-- Anchors. -->


<!-- URLs. -->
[Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
[Portal]: http://manage.windowsazure.com/
