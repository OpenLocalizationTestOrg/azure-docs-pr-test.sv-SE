---
title: "aaaGet igång med att leverera VoD med hello Azure-portalen | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom hello stegen för att implementera en grundläggande Video-on-Demand (VoD) innehållsleverans tjänst med Azure Media Services (AMS)-program med hjälp av hello Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 6c98fcfa-39e6-43a5-83a5-d4954788f8a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 5c1c1b1f74ec1f1301120fe8e5a5ae183fe0338f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-hello-azure-portal"></a><span data-ttu-id="40909-103">Komma igång med att leverera innehåll på begäran med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="40909-103">Get started with delivering content on demand using hello Azure portal</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="40909-104">Den här självstudiekursen vägleder dig genom hello stegen för att implementera en grundläggande Video-on-Demand (VoD) innehållsleverans tjänst med Azure Media Services (AMS)-program med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="40909-104">This tutorial walks you through hello steps of implementing a basic Video-on-Demand (VoD) content delivery service with Azure Media Services (AMS) application using hello Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40909-105">Krav</span><span class="sxs-lookup"><span data-stu-id="40909-105">Prerequisites</span></span>
<span data-ttu-id="40909-106">hello följande är obligatoriska toocomplete hello kursen:</span><span class="sxs-lookup"><span data-stu-id="40909-106">hello following are required toocomplete hello tutorial:</span></span>

* <span data-ttu-id="40909-107">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="40909-107">An Azure account.</span></span> <span data-ttu-id="40909-108">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="40909-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
* <span data-ttu-id="40909-109">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="40909-109">A Media Services account.</span></span> <span data-ttu-id="40909-110">toocreate Media Services-konto finns [hur tooCreate Media Services-konto](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="40909-110">toocreate a Media Services account, see [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>

<span data-ttu-id="40909-111">Den här självstudiekursen innehåller hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="40909-111">This tutorial includes hello following tasks:</span></span>

1. <span data-ttu-id="40909-112">Starta slutpunkt för direktuppspelning.</span><span class="sxs-lookup"><span data-stu-id="40909-112">Start streaming endpoint.</span></span>
2. <span data-ttu-id="40909-113">Överföra en videofil.</span><span class="sxs-lookup"><span data-stu-id="40909-113">Upload a video file.</span></span>
3. <span data-ttu-id="40909-114">Koda hello källfilen till en uppsättning MP4-filer med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="40909-114">Encode hello source file into a set of adaptive bitrate MP4 files.</span></span>
4. <span data-ttu-id="40909-115">Publicera hello tillgången och get strömning och progressiv nedladdning URL: er.</span><span class="sxs-lookup"><span data-stu-id="40909-115">Publish hello asset and get streaming and progressive download URLs.</span></span>  
5. <span data-ttu-id="40909-116">Spela upp ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="40909-116">Play your content.</span></span>

## <a name="start-streaming-endpoints"></a><span data-ttu-id="40909-117">Starta slutpunkter för direktuppspelning</span><span class="sxs-lookup"><span data-stu-id="40909-117">Start streaming endpoints</span></span> 

<span data-ttu-id="40909-118">När du arbetar med Azure Media Services är en av hello vanligaste scenarierna att leverera video via strömning med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="40909-118">When working with Azure Media Services one of hello most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="40909-119">Media Services tillhandahåller en dynamisk paketering som gör att du toodeliver dina MP4-kodade innehåll i strömningsformat som stöds av Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, utan att behöva toostore tillsammans med anpassad bithastighet versioner av var och en av dessa strömningsformat.</span><span class="sxs-lookup"><span data-stu-id="40909-119">Media Services provides dynamic packaging, which allows you toodeliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having toostore pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="40909-120">När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="40909-120">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="40909-121">toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="40909-121">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 

<span data-ttu-id="40909-122">toostart Hej strömmande slutpunkten, hello följande:</span><span class="sxs-lookup"><span data-stu-id="40909-122">toostart hello streaming endpoint, do hello following:</span></span>

1. <span data-ttu-id="40909-123">Logga in på hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="40909-123">Log in at hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="40909-124">Streaming slutpunkter på hello inställningar i fönstret.</span><span class="sxs-lookup"><span data-stu-id="40909-124">In hello Settings window, click Streaming endpoints.</span></span> 
3. <span data-ttu-id="40909-125">Klicka på hello standard strömmande slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="40909-125">Click hello default streaming endpoint.</span></span> 

    <span data-ttu-id="40909-126">hello visas standard information om den STRÖMNINGSSLUTPUNKT.</span><span class="sxs-lookup"><span data-stu-id="40909-126">hello DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="40909-127">Klicka på hello Start-ikonen.</span><span class="sxs-lookup"><span data-stu-id="40909-127">Click hello Start icon.</span></span>
5. <span data-ttu-id="40909-128">Klicka på hello spara knappen toosave ändringarna.</span><span class="sxs-lookup"><span data-stu-id="40909-128">Click hello Save button toosave your changes.</span></span>

## <a name="upload-files"></a><span data-ttu-id="40909-129">Överföra filer</span><span class="sxs-lookup"><span data-stu-id="40909-129">Upload files</span></span>
<span data-ttu-id="40909-130">toostream videor med Azure Media Services behöver du tooupload hello källvideorna, koda dem till flera olika bithastigheter och publicera hello resultat.</span><span class="sxs-lookup"><span data-stu-id="40909-130">toostream videos using Azure Media Services, you need tooupload hello source videos, encode them into multiple bitrates, and publish hello result.</span></span> <span data-ttu-id="40909-131">hello första steget beskrivs i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="40909-131">hello first step is covered in this section.</span></span> 

1. <span data-ttu-id="40909-132">I hello **inställningen** -fönstret klickar du på **tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="40909-132">In hello **Setting** window, click **Assets**.</span></span>
   
    ![Överföra filer](./media/media-services-portal-vod-get-started/media-services-upload.png)
2. <span data-ttu-id="40909-134">Klicka på hello **överför** knappen.</span><span class="sxs-lookup"><span data-stu-id="40909-134">Click hello **Upload** button.</span></span>
   
    <span data-ttu-id="40909-135">Hej **överför en videotillgång** visas.</span><span class="sxs-lookup"><span data-stu-id="40909-135">hello **Upload a video asset** window appears.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="40909-136">Det finns inga filstorleksbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="40909-136">There is no file size limitation.</span></span>
   > 
   > 
3. <span data-ttu-id="40909-137">Bläddra toohello önskade videon på datorn, markerar du den och tryck på OK.</span><span class="sxs-lookup"><span data-stu-id="40909-137">Browse toohello desired video on your computer, select it, and hit OK.</span></span>  
   
    <span data-ttu-id="40909-138">hello överföringen startar och du kan se hello förloppet under filnamnet hello.</span><span class="sxs-lookup"><span data-stu-id="40909-138">hello upload starts and you can see hello progress under hello file name.</span></span>  

<span data-ttu-id="40909-139">När hello överföringen har slutförts visas hello nya tillgången i hello **tillgångar** fönster.</span><span class="sxs-lookup"><span data-stu-id="40909-139">Once hello upload completes, you see hello new asset listed in hello **Assets** window.</span></span> 

## <a name="encode-assets"></a><span data-ttu-id="40909-140">Koda tillgångar</span><span class="sxs-lookup"><span data-stu-id="40909-140">Encode assets</span></span>

<span data-ttu-id="40909-141">När du arbetar med Azure Media Services är ett av hello vanligaste scenarierna att leverera med anpassningsbar bithastighet strömmande tooyour klienter.</span><span class="sxs-lookup"><span data-stu-id="40909-141">When working with Azure Media Services one of hello most common scenarios is delivering adaptive bitrate streaming tooyour clients.</span></span> <span data-ttu-id="40909-142">Media Services stöder följande strömningstekniker med anpassningsbar bithastighet hello: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="40909-142">Media Services supports hello following adaptive bitrate streaming technologies: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span> <span data-ttu-id="40909-143">tooprepare videor för anpassningsbar bithastighet strömning måste tooencode källfilerna video i flera bithastigheter filer.</span><span class="sxs-lookup"><span data-stu-id="40909-143">tooprepare your videos for adaptive bitrate streaming, you need tooencode your source video into multi-bitrate files.</span></span> <span data-ttu-id="40909-144">Du bör använda hello **Media Encoder Standard** kodare tooencode videor.</span><span class="sxs-lookup"><span data-stu-id="40909-144">You should use hello **Media Encoder Standard** encoder tooencode your videos.</span></span>  

<span data-ttu-id="40909-145">Media Services tillhandahåller också dynamisk paketering som gör att du toodeliver din multibithastighet MP4s i hello följande strömningsformat: MPEG DASH, HLS, Smooth Streaming, utan att behöva toorepackage till dessa strömningsformat.</span><span class="sxs-lookup"><span data-stu-id="40909-145">Media Services also provides dynamic packaging, which allows you toodeliver your multi-bitrate MP4s in hello following streaming formats: MPEG DASH, HLS, Smooth Streaming, without you having toorepackage into these streaming formats.</span></span> <span data-ttu-id="40909-146">Med dynamisk paketering behöver du bara toostore och betala för hello filer i ett enda lagringsformat och Media Services bygger och fungerar hello lämplig respons baserat på begäranden från en klient.</span><span class="sxs-lookup"><span data-stu-id="40909-146">With dynamic packaging, you only need toostore and pay for hello files in single storage format and Media Services builds and serves hello appropriate response based on requests from a client.</span></span>

<span data-ttu-id="40909-147">tootake nytta av dynamisk paketering behöver du behöver tooencode källfilen till en uppsättning med flera bithastigheter MP4-filer (hello kodningsstegen senare i det här avsnittet).</span><span class="sxs-lookup"><span data-stu-id="40909-147">tootake advantage of dynamic packaging, you need tooencode your source file into a set of multi-bitrate MP4 files (hello encoding steps are demonstrated later in this section).</span></span>

### <a name="toouse-hello-portal-tooencode"></a><span data-ttu-id="40909-148">toouse hello portal tooencode</span><span class="sxs-lookup"><span data-stu-id="40909-148">toouse hello portal tooencode</span></span>
<span data-ttu-id="40909-149">Det här avsnittet beskrivs hello åtgärder du kan vidta tooencode ditt innehåll med Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="40909-149">This section describes hello steps you can take tooencode your content with Media Encoder Standard.</span></span>

1. <span data-ttu-id="40909-150">I hello **inställningar** väljer **tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="40909-150">In hello **Settings** window, select **Assets**.</span></span>  
2. <span data-ttu-id="40909-151">I hello **tillgångar** fönster, Välj hello tillgång som du vill att tooencode.</span><span class="sxs-lookup"><span data-stu-id="40909-151">In hello **Assets** window, select hello asset that you would like tooencode.</span></span>
3. <span data-ttu-id="40909-152">Tryck på hello **koda** knappen.</span><span class="sxs-lookup"><span data-stu-id="40909-152">Press hello **Encode** button.</span></span>
4. <span data-ttu-id="40909-153">I hello **koda en tillgång** fönster, Välj hello ”Media Encoder Standard” processor och en förinställning.</span><span class="sxs-lookup"><span data-stu-id="40909-153">In hello **Encode an asset** window, select hello "Media Encoder Standard" processor and a preset.</span></span> <span data-ttu-id="40909-154">Information om förinställningar finns i [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) (autogenerera en bithastighetsstege) och [Task Presets for MES](media-services-mes-presets-overview.md) (Uppgiftsförinställningar för MES).</span><span class="sxs-lookup"><span data-stu-id="40909-154">For information about presets, see [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) and [Task Presets for MES](media-services-mes-presets-overview.md).</span></span> <span data-ttu-id="40909-155">Om du planerar toocontrol vilka kodning förinställda används, Kom ihåg: det är viktigt tooselect hello förinställning som är mest lämpliga för din indatavideo.</span><span class="sxs-lookup"><span data-stu-id="40909-155">If you plan toocontrol which encoding preset is used, keep this in mind: it is important tooselect hello preset that is most appropriate for your input video.</span></span> <span data-ttu-id="40909-156">Till exempel om du vet att din indatavideo har en upplösning på 1 920 x 1 080 bildpunkter, du kan använda hello ”H264 Multibithastighet 1080p” förinställda.</span><span class="sxs-lookup"><span data-stu-id="40909-156">For example, if you know your input video has a resolution of 1920x1080 pixels, then you could use hello "H264 Multiple Bitrate 1080p" preset.</span></span> <span data-ttu-id="40909-157">Om du har en video med låg upplösning (640 x 360) bör du inte använda förinställningen ”H264 multibithastighet 1080p”.</span><span class="sxs-lookup"><span data-stu-id="40909-157">If you have a low resolution (640x360) video, then you should not be using "H264 Multiple Bitrate 1080p" preset.</span></span>
   
   <span data-ttu-id="40909-158">Har en möjlighet att redigera hello av hello utdatatillgången och hello namn för hello jobbet för enklare hantering.</span><span class="sxs-lookup"><span data-stu-id="40909-158">For easier management, you have an option of editing hello name of hello output asset, and hello name of hello job.</span></span>
   
   ![Koda tillgångar](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. <span data-ttu-id="40909-160">Tryck på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="40909-160">Press **Create**.</span></span>

### <a name="monitor-encoding-job-progress"></a><span data-ttu-id="40909-161">Övervaka förloppet för kodningsjobb</span><span class="sxs-lookup"><span data-stu-id="40909-161">Monitor encoding job progress</span></span>
<span data-ttu-id="40909-162">toomonitor hello fortskrider hello kodning jobb, klicka på **inställningar** (överst hello på hello sidan) och välj sedan **jobb**.</span><span class="sxs-lookup"><span data-stu-id="40909-162">toomonitor hello progress of hello encoding job, click **Settings** (at hello top of hello page) and then select **Jobs**.</span></span>

![Jobb](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a><span data-ttu-id="40909-164">Publicera innehåll</span><span class="sxs-lookup"><span data-stu-id="40909-164">Publish content</span></span>
<span data-ttu-id="40909-165">tooprovide din användare en URL som kan använda toostream eller hämta ditt innehåll du först måste för ”publicera” din tillgång genom att skapa en positionerare.</span><span class="sxs-lookup"><span data-stu-id="40909-165">tooprovide your user with a  URL that can be used toostream or download your content, you first need too"publish" your asset by creating a locator.</span></span> <span data-ttu-id="40909-166">Positionerare ger åtkomst toofiles i hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="40909-166">Locators provide access toofiles contained in hello asset.</span></span> <span data-ttu-id="40909-167">Media Services stöder två typer av lokaliserare:</span><span class="sxs-lookup"><span data-stu-id="40909-167">Media Services supports two types of locators:</span></span> 

* <span data-ttu-id="40909-168">Streaming (OnDemandOrigin)-positionerare som används för anpassad strömning (till exempel toostream MPEG DASH, HLS eller Smooth Streaming).</span><span class="sxs-lookup"><span data-stu-id="40909-168">Streaming (OnDemandOrigin) locators, used for adaptive streaming (for example, toostream MPEG DASH, HLS, or Smooth Streaming).</span></span> <span data-ttu-id="40909-169">toocreate en strömningslokaliserare måste din tillgång innehålla en .ism-fil.</span><span class="sxs-lookup"><span data-stu-id="40909-169">toocreate a streaming locator your asset must contain an .ism file.</span></span> 
* <span data-ttu-id="40909-170">Progressiva SAS-lokaliserare, som används för leverans av video via progressiv hämtning.</span><span class="sxs-lookup"><span data-stu-id="40909-170">Progressive (SAS) locators, used for delivery of video via progressive download.</span></span>

<span data-ttu-id="40909-171">En strömmande URL har följande format hello och du kan använda den tooplay Smooth Streaming-tillgångar.</span><span class="sxs-lookup"><span data-stu-id="40909-171">A streaming URL has hello following format and you can use it tooplay Smooth Streaming assets.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

<span data-ttu-id="40909-172">Lägg till toobuild en HLS-strömnings-URL (format = m3u8 aapl) toohello URL.</span><span class="sxs-lookup"><span data-stu-id="40909-172">toobuild an HLS streaming URL, append (format=m3u8-aapl) toohello URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

<span data-ttu-id="40909-173">toobuild en MPEG DASH-strömnings-URL, Lägg till (format = mpd-tid-csf) toohello URL.</span><span class="sxs-lookup"><span data-stu-id="40909-173">toobuild an  MPEG DASH streaming URL, append (format=mpd-time-csf) toohello URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


<span data-ttu-id="40909-174">En SAS-URL har följande format hello.</span><span class="sxs-lookup"><span data-stu-id="40909-174">A SAS URL has hello following format.</span></span>

    {blob container name}/{asset name}/{file name}/{SAS signature}

> [!NOTE]
> <span data-ttu-id="40909-175">Om du använde hello portal toocreate lokaliserare före mars 2015, skapades lokaliserare med ett utgångsdatum för två år.</span><span class="sxs-lookup"><span data-stu-id="40909-175">If you used hello portal toocreate locators before March 2015, locators with a two-year expiration date were created.</span></span>  
> 
> 

<span data-ttu-id="40909-176">tooupdate ett förfallodatum för en lokaliserare, Använd [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) eller [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API: er.</span><span class="sxs-lookup"><span data-stu-id="40909-176">tooupdate an expiration date on a locator, use [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) or [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) APIs.</span></span> <span data-ttu-id="40909-177">När du uppdaterar en SAS-lokaliserare hello utgångsdatum ändras hello URL: en.</span><span class="sxs-lookup"><span data-stu-id="40909-177">When you update hello expiration date of a SAS locator, hello URL changes.</span></span>

### <a name="toouse-hello-portal-toopublish-an-asset"></a><span data-ttu-id="40909-178">toouse hello portal toopublish en tillgång</span><span class="sxs-lookup"><span data-stu-id="40909-178">toouse hello portal toopublish an asset</span></span>
<span data-ttu-id="40909-179">toouse hello portal toopublish en tillgång, hello följande:</span><span class="sxs-lookup"><span data-stu-id="40909-179">toouse hello portal toopublish an asset, do hello following:</span></span>

1. <span data-ttu-id="40909-180">Välj **Inställningar** > **Tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="40909-180">Select **Settings** > **Assets**.</span></span>
2. <span data-ttu-id="40909-181">Välj hello tillgång som du vill toopublish.</span><span class="sxs-lookup"><span data-stu-id="40909-181">Select hello asset that you want toopublish.</span></span>
3. <span data-ttu-id="40909-182">Klicka på hello **publicera** knappen.</span><span class="sxs-lookup"><span data-stu-id="40909-182">Click hello **Publish** button.</span></span>
4. <span data-ttu-id="40909-183">Välj typen av hello-positionerare.</span><span class="sxs-lookup"><span data-stu-id="40909-183">Select hello locator type.</span></span>
5. <span data-ttu-id="40909-184">Tryck på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="40909-184">Press **Add**.</span></span>
   
    ![Publicera](./media/media-services-portal-vod-get-started/media-services-publish1.png)

<span data-ttu-id="40909-186">hello URL läggs toohello lista över **publicerade URL: er**.</span><span class="sxs-lookup"><span data-stu-id="40909-186">hello URL is added toohello list of **Published URLs**.</span></span>

## <a name="play-content-from-hello-portal"></a><span data-ttu-id="40909-187">Spela upp innehåll från hello-portalen</span><span class="sxs-lookup"><span data-stu-id="40909-187">Play content from hello portal</span></span>
<span data-ttu-id="40909-188">hello Azure-portalen har en innehållsspelare som du kan använda tootest videon.</span><span class="sxs-lookup"><span data-stu-id="40909-188">hello Azure portal provides a content player that you can use tootest your video.</span></span>

<span data-ttu-id="40909-189">Hello önskad video och klicka sedan på hello **spela upp** knappen.</span><span class="sxs-lookup"><span data-stu-id="40909-189">Click hello desired video and then click hello **Play** button.</span></span>

![Publicera](./media/media-services-portal-vod-get-started/media-services-play.png)

<span data-ttu-id="40909-191">Vissa förutsättningar gäller:</span><span class="sxs-lookup"><span data-stu-id="40909-191">Some considerations apply:</span></span>

* <span data-ttu-id="40909-192">toobegin direktuppspelning, starta körs hello **standard** strömmande slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="40909-192">toobegin streaming, start running hello **default** streaming endpoint.</span></span>
* <span data-ttu-id="40909-193">Kontrollera att hello videon har publicerats.</span><span class="sxs-lookup"><span data-stu-id="40909-193">Make sure hello video has been published.</span></span>
* <span data-ttu-id="40909-194">Detta **Media player** spelar upp från hello standard strömmande slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="40909-194">This **Media player** plays from hello default streaming endpoint.</span></span> <span data-ttu-id="40909-195">Om du vill tooplay från ett standardvärde strömmande slutpunkten, klicka på toocopy hello URL och Använd en annan spelare.</span><span class="sxs-lookup"><span data-stu-id="40909-195">If you want tooplay from a non-default streaming endpoint, click toocopy hello URL and use another player.</span></span> <span data-ttu-id="40909-196">Till exempel [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="40909-196">For example, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="40909-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="40909-197">Next steps</span></span>
<span data-ttu-id="40909-198">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="40909-198">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="40909-199">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="40909-199">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

