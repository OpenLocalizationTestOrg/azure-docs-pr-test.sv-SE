---
title: "aaaMicrosoft Azure Media Services scenarier och tillgängligheten för funktioner i Datacenter | Microsoft Docs"
description: "Det här avsnittet ger en översikt över Microsoft Azure Media Services-scenarier och tillgängligheten för funktioner och tjänster i datacenter."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 3dbab6998ed5da738baf8f1e2fb096dfba336e19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scenarios-and-availability-of-media-services-features-across-datacenters"></a><span data-ttu-id="ec918-103">Scenarier och tillgängligheten för Media Services-funktioner i datacenter</span><span class="sxs-lookup"><span data-stu-id="ec918-103">Scenarios and availability of Media Services features across datacenters</span></span>

<span data-ttu-id="ec918-104">Microsoft Azure Media Services (AMS) kan du ladda upp toosecurely, lagra, koda och paketera video-eller ljudinnehåll för både på begäran och live strömmande leverans toovarious klienter (till exempel TV, datorer och mobila enheter).</span><span class="sxs-lookup"><span data-stu-id="ec918-104">Microsoft Azure Media Services (AMS) enables you toosecurely upload, store, encode, and package video or audio content for both on-demand and live streaming delivery toovarious clients (for example, TV, PC, and mobile devices).</span></span>

<span data-ttu-id="ec918-105">AMS körs i flera Datacenter hello världen.</span><span class="sxs-lookup"><span data-stu-id="ec918-105">AMS operates in multiple data centers around hello world.</span></span> <span data-ttu-id="ec918-106">Dessa Datacenter grupperas i toogeographic regioner, vilket ger dig möjlighet att välja var toobuild dina program.</span><span class="sxs-lookup"><span data-stu-id="ec918-106">These data centers are grouped in toogeographic regions, giving you flexibility in choosing where toobuild your applications.</span></span> <span data-ttu-id="ec918-107">Du kan granska hello [lista över regioner och deras platser](https://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="ec918-107">You can review hello [list of regions and their locations](https://azure.microsoft.com/regions/).</span></span> 

<span data-ttu-id="ec918-108">Det här avsnittet beskriver vanliga scenarier för att leverera innehåll [live](#live_scenarios) eller [på begäran](#vod_scenarios).</span><span class="sxs-lookup"><span data-stu-id="ec918-108">This topic shows common scenarios for delivering your content [live](#live_scenarios) or [on-demand](#vod_scenarios).</span></span> <span data-ttu-id="ec918-109">hello avsnittet ger också information om tillgängligheten för mediafunktioner och tjänster över datacenter.</span><span class="sxs-lookup"><span data-stu-id="ec918-109">hello topic also provides details about availability of media features and services across data centers.</span></span>

## <a name="overview"></a><span data-ttu-id="ec918-110">Översikt</span><span class="sxs-lookup"><span data-stu-id="ec918-110">Overview</span></span>

### <a name="prerequisites"></a><span data-ttu-id="ec918-111">Krav</span><span class="sxs-lookup"><span data-stu-id="ec918-111">Prerequisites</span></span>

<span data-ttu-id="ec918-112">toostart med Azure Media Services bör du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="ec918-112">toostart using Azure Media Services, you should have hello following:</span></span>

* <span data-ttu-id="ec918-113">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ec918-113">An Azure account.</span></span> <span data-ttu-id="ec918-114">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="ec918-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="ec918-115">Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="ec918-115">For details, see [Azure Free Trial](https://azure.microsoft.com).</span></span>
* <span data-ttu-id="ec918-116">Ett Azure Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="ec918-116">An Azure Media Services account.</span></span> <span data-ttu-id="ec918-117">Mer information finns i [Skapa konto](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="ec918-117">For more information, see [Create Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="ec918-118">Hej strömningsslutpunkt från vilken du vill att toostream innehåll har toobe i hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="ec918-118">hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span>

    <span data-ttu-id="ec918-119">När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="ec918-119">When your AMS account is created, a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="ec918-120">toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering, hello strömmande slutpunkten har toobe i hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="ec918-120">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint has toobe in hello **Running** state.</span></span>

### <a name="commonly-used-objects-when-developing-against-hello-ams-odata-model"></a><span data-ttu-id="ec918-121">Vanliga objekt när du utvecklar mot hello AMS OData-modellen</span><span class="sxs-lookup"><span data-stu-id="ec918-121">Commonly used objects when developing against hello AMS OData model</span></span>

<span data-ttu-id="ec918-122">hello följande bild visar några av de vanligaste hello objekt när du utvecklar mot hello Media Services OData-modellen.</span><span class="sxs-lookup"><span data-stu-id="ec918-122">hello following image shows some of hello most commonly used objects when developing against hello Media Services OData model.</span></span>

<span data-ttu-id="ec918-123">Klicka på hello avbildningen tooview den full storlek.</span><span class="sxs-lookup"><span data-stu-id="ec918-123">Click hello image tooview it full size.</span></span>  

<a href="./media/media-services-overview/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-overview/media-services-overview-object-model-small.png"></a> 

<span data-ttu-id="ec918-124">Du kan visa hela modellen hello [här](https://media.windows.net/API/$metadata?api-version=2.15).</span><span class="sxs-lookup"><span data-stu-id="ec918-124">You can view hello whole model [here](https://media.windows.net/API/$metadata?api-version=2.15).</span></span>  

## <a name="protect-content-in-storage-and-deliver-streaming-media-in-hello-clear-non-encrypted"></a><span data-ttu-id="ec918-125">Skydda lagrat innehåll och leverera strömmande medier i hello avmarkera (okrypterat)</span><span class="sxs-lookup"><span data-stu-id="ec918-125">Protect content in storage and deliver streaming media in hello clear (non-encrypted)</span></span>

![VoD-arbetsflöde](./media/scenarios-and-availability/scenarios-and-availability01.png)

1. <span data-ttu-id="ec918-127">Överför en mediefil av hög kvalitet till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="ec918-127">Upload a high-quality media file into an asset.</span></span>

    <span data-ttu-id="ec918-128">Det rekommenderas tooapply storage kryptering alternativet tooyour tillgångar i ordning tooprotect ditt innehåll under överföra och medan i vila i lagringsutrymmet.</span><span class="sxs-lookup"><span data-stu-id="ec918-128">It is recommended tooapply storage encryption option tooyour asset in order tooprotect your content during upload and while at rest in storage.</span></span>
2. <span data-ttu-id="ec918-129">Koda tooa uppsättning MP4-filer med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="ec918-129">Encode tooa set of adaptive bitrate MP4 files.</span></span>

    <span data-ttu-id="ec918-130">Det rekommenderas tooapply storage kryptering alternativet toohello utdata tillgångar i ordning tooprotect innehållet i vila.</span><span class="sxs-lookup"><span data-stu-id="ec918-130">It is recommended tooapply storage encryption option toohello output asset in order tooprotect your content at rest.</span></span>
3. <span data-ttu-id="ec918-131">Konfigurera principen för tillgångsleverans (används av dynamisk paketering).</span><span class="sxs-lookup"><span data-stu-id="ec918-131">Configure asset delivery policy (used by dynamic packaging).</span></span>

    <span data-ttu-id="ec918-132">Om tillgången är lagringskrypterad **måste** du konfigurera principen för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="ec918-132">If your asset is storage encrypted, you **must** configure asset delivery policy.</span></span>
4. <span data-ttu-id="ec918-133">Publicera hello tillgång genom att skapa en OnDemand-positionerare.</span><span class="sxs-lookup"><span data-stu-id="ec918-133">Publish hello asset by creating an OnDemand locator.</span></span>
5. <span data-ttu-id="ec918-134">Strömma publicerat innehåll.</span><span class="sxs-lookup"><span data-stu-id="ec918-134">Stream published content.</span></span>

<span data-ttu-id="ec918-135">Information om tillgänglighet i datacenter finns hello [tillgänglighet](#availability) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ec918-135">For information about availability in datacenters, see hello [Availability](#availability) section.</span></span>

## <a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a><span data-ttu-id="ec918-136">Skydda lagrat innehåll, leverera dynamiskt krypterade strömmande media</span><span class="sxs-lookup"><span data-stu-id="ec918-136">Protect content in storage, deliver dynamically encrypted streaming media</span></span>

![Skydda med PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

1. <span data-ttu-id="ec918-138">Överför en mediefil av hög kvalitet till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="ec918-138">Upload a high-quality media file into an asset.</span></span> <span data-ttu-id="ec918-139">Tillämpa storage kryptering alternativet toohello tillgången.</span><span class="sxs-lookup"><span data-stu-id="ec918-139">Apply storage encryption option toohello asset.</span></span>
2. <span data-ttu-id="ec918-140">Koda tooa uppsättning MP4-filer med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="ec918-140">Encode tooa set of adaptive bitrate MP4 files.</span></span> <span data-ttu-id="ec918-141">Tillämpa storage kryptering alternativet toohello utdatatillgången.</span><span class="sxs-lookup"><span data-stu-id="ec918-141">Apply storage encryption option toohello output asset.</span></span>
3. <span data-ttu-id="ec918-142">Skapa en innehållskrypteringsnyckel för hello tillgång som du vill kryptera dynamiskt under uppspelning toobe.</span><span class="sxs-lookup"><span data-stu-id="ec918-142">Create encryption content key for hello asset you want toobe dynamically encrypted during playback.</span></span>
4. <span data-ttu-id="ec918-143">Konfigurera principen för auktorisering av innehållsnyckel.</span><span class="sxs-lookup"><span data-stu-id="ec918-143">Configure content key authorization policy.</span></span>
5. <span data-ttu-id="ec918-144">Konfigurera en princip för tillgångsleveranser (används av dynamisk paketering och dynamisk kryptering).</span><span class="sxs-lookup"><span data-stu-id="ec918-144">Configure asset delivery policy (used by dynamic packaging and dynamic encryption).</span></span>
6. <span data-ttu-id="ec918-145">Publicera hello tillgång genom att skapa en OnDemand-positionerare.</span><span class="sxs-lookup"><span data-stu-id="ec918-145">Publish hello asset by creating an OnDemand locator.</span></span>
7. <span data-ttu-id="ec918-146">Strömma publicerat innehåll.</span><span class="sxs-lookup"><span data-stu-id="ec918-146">Stream published content.</span></span>

<span data-ttu-id="ec918-147">Information om tillgänglighet i datacenter finns hello [tillgänglighet](#availability) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ec918-147">For information about availability in datacenters, see hello [Availability](#availability) section.</span></span>

## <a name="use-media-analytics-tooderive-actionable-insights-from-your-videos"></a><span data-ttu-id="ec918-148">Använd Media Analytics tooderive tillämplig insikter från dina videor</span><span class="sxs-lookup"><span data-stu-id="ec918-148">Use Media Analytics tooderive actionable insights from your videos</span></span>

<span data-ttu-id="ec918-149">Media Analytics är en samling tal- och visionskomponenter komponenter som gör det lättare för organisationer och företag tooderive tillämplig insikter från sina videofiler.</span><span class="sxs-lookup"><span data-stu-id="ec918-149">Media Analytics is a collection of speech and vision components that make it easier for organizations and enterprises tooderive actionable insights from their video files.</span></span> <span data-ttu-id="ec918-150">Mer information finns i [Översikt över Azure Media Services Analytics](media-services-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ec918-150">For more information, see [Azure Media Services Analytics Overview](media-services-analytics-overview.md).</span></span>

1. <span data-ttu-id="ec918-151">Överför en mediefil av hög kvalitet till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="ec918-151">Upload a high-quality media file into an asset.</span></span>
2. <span data-ttu-id="ec918-152">Bearbeta videor med något av hello Media Analytics-tjänster som beskrivs i hello [översikt över Media Analytics](media-services-analytics-overview.md) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ec918-152">Process your videos with one of hello Media Analytics services described in hello [Media Analytics overview](media-services-analytics-overview.md) section.</span></span>
3. <span data-ttu-id="ec918-153">Media Analytics-medieprocessorer producerar MP4-filer eller JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="ec918-153">Media Analytics media processors produce MP4 files or JSON files.</span></span> <span data-ttu-id="ec918-154">Om en medieprocessor har producerat en MP4-fil kan hämta du progressivt hello-filen.</span><span class="sxs-lookup"><span data-stu-id="ec918-154">If a media processor produced an MP4 file, you can progressively download hello file.</span></span> <span data-ttu-id="ec918-155">Om en medieprocessor har producerat en JSON-fil kan du ladda ned hello filen från hello Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="ec918-155">If a media processor produced a JSON file, you can download hello file from hello Azure blob storage.</span></span>

<span data-ttu-id="ec918-156">Information om tillgänglighet i datacenter finns hello [tillgänglighet](#availability) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ec918-156">For information about availability in datacenters, see hello [Availability](#availability) section.</span></span>

## <a name="deliver-progressive-download"></a><span data-ttu-id="ec918-157">Leverera progressiv nedladdning</span><span class="sxs-lookup"><span data-stu-id="ec918-157">Deliver progressive download</span></span>

1. <span data-ttu-id="ec918-158">Överför en mediefil av hög kvalitet till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="ec918-158">Upload a high-quality media file into an asset.</span></span>
2. <span data-ttu-id="ec918-159">Koda tooa enda MP4-fil.</span><span class="sxs-lookup"><span data-stu-id="ec918-159">Encode tooa single MP4 file.</span></span>
3. <span data-ttu-id="ec918-160">Publicera hello tillgång genom att skapa en OnDemand- eller SAS-positionerare.</span><span class="sxs-lookup"><span data-stu-id="ec918-160">Publish hello asset by creating an OnDemand or SAS locator.</span></span>

    <span data-ttu-id="ec918-161">Om du använder SAS-positionerare laddas hello innehållet ned från hello Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="ec918-161">If using SAS locator, hello content is downloaded from hello Azure blob storage.</span></span> <span data-ttu-id="ec918-162">I det här fallet behöver inte toohave strömningsslutpunkter i startat tillstånd.</span><span class="sxs-lookup"><span data-stu-id="ec918-162">In this case, you do not need toohave streaming endpoints in started state.</span></span>
4. <span data-ttu-id="ec918-163">Progressivt hämtat innehåll</span><span class="sxs-lookup"><span data-stu-id="ec918-163">Progressively download content.</span></span>

## <span data-ttu-id="ec918-164"><a id="live_scenarios"></a>Leverera liveuppspelningshändelser</span><span class="sxs-lookup"><span data-stu-id="ec918-164"><a id="live_scenarios"></a>Delivering live-streaming events</span></span> 

1. <span data-ttu-id="ec918-165">Infoga innehåll via olika protokoll för liveuppspelning (till exempel RTMP eller Smooth Streaming).</span><span class="sxs-lookup"><span data-stu-id="ec918-165">Ingest live content using various live streaming protocols (for example RTMP or Smooth Streaming).</span></span>
2. <span data-ttu-id="ec918-166">(Valfritt) Koda strömmen till ström med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="ec918-166">(optionally) Encode your stream into adaptive bitrate stream.</span></span>
3. <span data-ttu-id="ec918-167">Förhandsgranska din liveuppspelning.</span><span class="sxs-lookup"><span data-stu-id="ec918-167">Preview your live stream.</span></span>
4. <span data-ttu-id="ec918-168">leverera hello innehållet via vanliga strömningsprotokoll (till exempel MPEG DASH, Smooth, HLS) direkt tooyour kunder eller tooa innehåll innehållsleveransnätverk (CDN) för vidare distribution.</span><span class="sxs-lookup"><span data-stu-id="ec918-168">Deliver hello content through common streaming protocols (for example, MPEG DASH, Smooth, HLS) directly tooyour customers, or tooa Content Delivery Network (CDN) for further distribution.</span></span>

    <span data-ttu-id="ec918-169">ELLER</span><span class="sxs-lookup"><span data-stu-id="ec918-169">-or-</span></span>

    <span data-ttu-id="ec918-170">Post och store hello inhämtas innehåll i ordning toobe strömma senare (Video-on-Demand).</span><span class="sxs-lookup"><span data-stu-id="ec918-170">Record and store hello ingested content in order toobe streamed later (Video-on-Demand).</span></span>

<span data-ttu-id="ec918-171">Du kan välja en av följande hello dirigerar när du gör direktsänd strömning:</span><span class="sxs-lookup"><span data-stu-id="ec918-171">When doing live streaming, you can choose one of hello following routes:</span></span>

### <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a><span data-ttu-id="ec918-172">Arbeta med kanaler som tar emot liveström med flera bithastigheter från lokala kodare (genomströmning)</span><span class="sxs-lookup"><span data-stu-id="ec918-172">Working with channels that receive multi-bitrate live stream from on-premises encoders (pass-through)</span></span>

<span data-ttu-id="ec918-173">hello följande diagram visar hello huvudsakliga delarna i hello AMS-plattformen som ingår i hello **direkt** arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="ec918-173">hello following diagram shows hello major parts of hello AMS platform that are involved in hello **pass-through** workflow.</span></span>

![Live-arbetsflöde](./media/scenarios-and-availability/media-services-live-streaming-current.png)

<span data-ttu-id="ec918-175">Mer information finns i [Arbeta med kanaler som tar emot liveström med flera bithastigheter från lokala kodare](media-services-live-streaming-with-onprem-encoders.md).</span><span class="sxs-lookup"><span data-stu-id="ec918-175">For more information, see [Working with Channels that Receive Multi-bitrate Live Stream from On-premises Encoders](media-services-live-streaming-with-onprem-encoders.md).</span></span>

### <a name="working-with-channels-that-are-enabled-tooperform-live-encoding-with-azure-media-services"></a><span data-ttu-id="ec918-176">Arbeta med kanaler som är aktiverad tooperform live encoding med Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="ec918-176">Working with channels that are enabled tooperform live encoding with Azure Media Services</span></span>

<span data-ttu-id="ec918-177">hello följande diagram visar hello större delar av hello AMS-plattformen som ingår i arbetsflödet för Liveströmning där en kanal är aktiverad tooperform live encoding med Media Services.</span><span class="sxs-lookup"><span data-stu-id="ec918-177">hello following diagram shows hello major parts of hello AMS platform that are involved in Live Streaming workflow where a Channel is enabled tooperform live encoding with Media Services.</span></span>

![Live-arbetsflöde](./media/scenarios-and-availability/media-services-live-streaming-new.png)

<span data-ttu-id="ec918-179">Mer information finns i [arbeta med kanaler som är aktiverad tooPerform Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="ec918-179">For more information, see [Working with Channels that are Enabled tooPerform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="ec918-180">Information om tillgänglighet i datacenter finns hello [tillgänglighet](#availability) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ec918-180">For information about availability in datacenters, see hello [Availability](#availability) section.</span></span>

## <a name="consuming-content"></a><span data-ttu-id="ec918-181">Konsumera innehåll</span><span class="sxs-lookup"><span data-stu-id="ec918-181">Consuming content</span></span>

<span data-ttu-id="ec918-182">Azure Media Services innehåller hello verktyg du behöver toocreate omfattande dynamiska klientprogram för uppspelare för de flesta plattformar, bland annat: iOS-enheter, Android-enheter, Windows, Windows Phone, Xbox och fristående rutorna.</span><span class="sxs-lookup"><span data-stu-id="ec918-182">Azure Media Services provides hello tools you need toocreate rich, dynamic client player applications for most platforms including: iOS Devices, Android Devices, Windows, Windows Phone, Xbox, and Set-top boxes.</span></span> <span data-ttu-id="ec918-183">följande ämne hello innehåller länkar tooSDKs och Spelarramverk som du kan använda toodevelop egna klientprogram som kan använda strömmande media från Media Services.</span><span class="sxs-lookup"><span data-stu-id="ec918-183">hello following topic provides links tooSDKs and Player Frameworks that you can use toodevelop your own client applications that can consume streaming media from Media Services.</span></span> <span data-ttu-id="ec918-184">Mer information finns i [Developing video player applications](media-services-develop-video-players.md) (Utveckla videospelarprogram)</span><span class="sxs-lookup"><span data-stu-id="ec918-184">For more information, see [Developing video payer applications](media-services-develop-video-players.md)</span></span>

## <a name="enabling-azure-cdn"></a><span data-ttu-id="ec918-185">Aktivera Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ec918-185">Enabling Azure CDN</span></span>

<span data-ttu-id="ec918-186">Media Services stöder integration med Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="ec918-186">Media Services supports integration with Azure CDN.</span></span> <span data-ttu-id="ec918-187">Mer information om hur tooenable Azure CDN finns [hur tooManage Strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="ec918-187">For information on how tooenable Azure CDN, see [How tooManage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md).</span></span>

## <span data-ttu-id="ec918-188"><a id="scaling"></a>Skala ett Media Services-konto</span><span class="sxs-lookup"><span data-stu-id="ec918-188"><a id="scaling"></a>Scaling a Media Services account</span></span>

<span data-ttu-id="ec918-189">AMS-kunder kan skala slutpunkter för direktuppspelning, mediebearbetning och lagring i sina AMS-konton.</span><span class="sxs-lookup"><span data-stu-id="ec918-189">AMS customers can scale streaming endpoints, media processing, and storage in their AMS accounts.</span></span>

* <span data-ttu-id="ec918-190">Media Services-kunder kan antingen välja en **Standard**-slutpunkt för direktuppspelning eller en **Premium**-slutpunkt för direktuppspelning.</span><span class="sxs-lookup"><span data-stu-id="ec918-190">Media Services customers can choose either a **Standard** streaming endpoint or a **Premium** streaming endpoint.</span></span> <span data-ttu-id="ec918-191">En **Standard**-slutpunkt för direktuppspelning passar de flesta arbetsbelastningar för direktuppspelning.</span><span class="sxs-lookup"><span data-stu-id="ec918-191">A **Standard** streaming endpoint is suitable for most streaming workloads.</span></span> <span data-ttu-id="ec918-192">Hello samma funktioner som innehåller en **Premium** strömmande slutpunkter och skalar utgående bandbredd automatiskt.</span><span class="sxs-lookup"><span data-stu-id="ec918-192">It includes hello same features as a **Premium** streaming endpoints and scales outbound bandwidth automatically.</span></span> 

    <span data-ttu-id="ec918-193">**Premium**-slutpunkter för direktuppspelning passar för avancerade arbetsbelastningar och tillhandahåller dedikerad och skalbar bandbreddskapacitet.</span><span class="sxs-lookup"><span data-stu-id="ec918-193">**Premium** streaming endpoints are suitable for advanced workloads, providing dedicated and scalable bandwidth capacity.</span></span> <span data-ttu-id="ec918-194">Kunder som har en **Premium**-slutpunkt för direktuppspelning får som standard en direktuppspelande enhet (SU).</span><span class="sxs-lookup"><span data-stu-id="ec918-194">Customers that have a **Premium** streaming endpoint, by default get one streaming unit (SU).</span></span> <span data-ttu-id="ec918-195">hello strömmande slutpunkten kan skalas genom att lägga till SUs.</span><span class="sxs-lookup"><span data-stu-id="ec918-195">hello streaming endpoint can be scaled by adding SUs.</span></span> <span data-ttu-id="ec918-196">Varje SU ger ytterligare bandbredd kapacitet toohello program.</span><span class="sxs-lookup"><span data-stu-id="ec918-196">Each SU provides additional bandwidth capacity toohello application.</span></span> <span data-ttu-id="ec918-197">Mer information om att skala **Premium** strömningsslutpunkter, se hello [skalning strömningsslutpunkter](media-services-portal-scale-streaming-endpoints.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ec918-197">For more information about scaling **Premium** streaming endpoints, see hello [Scaling streaming endpoints](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

* <span data-ttu-id="ec918-198">Ett Media Services-konto är kopplat till ett reserverat enhetstyp som bestämmer hello hastighet som media bearbeta uppgifter bearbetas.</span><span class="sxs-lookup"><span data-stu-id="ec918-198">A Media Services account is associated with a Reserved Unit Type, which determines hello speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="ec918-199">Du kan välja mellan följande hello reserverade enhetstyper: **S1**, **S2**, eller **S3**.</span><span class="sxs-lookup"><span data-stu-id="ec918-199">You can pick between hello following reserved unit types: **S1**, **S2**, or **S3**.</span></span> <span data-ttu-id="ec918-200">Till exempel hello samma kodningsjobbet körs snabbare när du använder hello **S2** reserverade enhetstyp jämföra toohello **S1** typen.</span><span class="sxs-lookup"><span data-stu-id="ec918-200">For example, hello same encoding job runs faster when you use hello **S2** reserved unit type compare toohello **S1** type.</span></span>

    <span data-ttu-id="ec918-201">Dessutom toospecifying hello reserverade enhetstyp, kan du ange tooprovision till ditt konto med **reserverade enheter** (RUs).</span><span class="sxs-lookup"><span data-stu-id="ec918-201">In addition toospecifying hello reserved unit type, you can specify tooprovision your account with **Reserved Units** (RUs).</span></span> <span data-ttu-id="ec918-202">hello antalet etablerade RUs bestämmer hello antal media uppgifter som kan bearbetas samtidigt i en viss konto.</span><span class="sxs-lookup"><span data-stu-id="ec918-202">hello number of provisioned RUs determines hello number of media tasks that can be processed concurrently in a given account.</span></span>

    >[!NOTE]
    ><span data-ttu-id="ec918-203">RU:er fungerar för parallellisera all bearbetning av media, inklusive indexeringsjobb med hjälp av Azure Media Indexer.</span><span class="sxs-lookup"><span data-stu-id="ec918-203">RUs work for parallelizing all media processing, including indexing jobs using Azure Media Indexer.</span></span> <span data-ttu-id="ec918-204">Men till skillnad från kodning bearbetas inte indexeringsjobb snabbare med snabbare reserverade enheter.</span><span class="sxs-lookup"><span data-stu-id="ec918-204">However, unlike encoding, indexing jobs do not get processed faster with faster reserved units.</span></span>

    <span data-ttu-id="ec918-205">Mer information finns i [Skala mediebearbetning](media-services-portal-scale-media-processing.md).</span><span class="sxs-lookup"><span data-stu-id="ec918-205">For more information see, [Scale media processing](media-services-portal-scale-media-processing.md).</span></span>
* <span data-ttu-id="ec918-206">Du kan även skala ditt Media Services-konto genom att lägga till storage-konton tooit.</span><span class="sxs-lookup"><span data-stu-id="ec918-206">You can also scale your Media Services account by adding storage accounts tooit.</span></span> <span data-ttu-id="ec918-207">Varje lagringskonto är begränsat too500 TB.</span><span class="sxs-lookup"><span data-stu-id="ec918-207">Each storage account is limited too500 TB.</span></span> <span data-ttu-id="ec918-208">tooexpand din lagring utöver standardbegränsningarna hello du tooattach flera storage-konton tooa enda Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="ec918-208">tooexpand your storage beyond hello default limitations, you can choose tooattach multiple storage accounts tooa single Media Services account.</span></span> <span data-ttu-id="ec918-209">Mer information finns i [Hantera lagringskonton](meda-services-managing-multiple-storage-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="ec918-209">For more information, see [Manage storage accounts](meda-services-managing-multiple-storage-accounts.md).</span></span>

##<span data-ttu-id="ec918-210"><a id="availability"></a> Tillgänglighet för Media Services-funktioner i datacenter</span><span class="sxs-lookup"><span data-stu-id="ec918-210"><a id="availability"></a> Availability of Media Services features across datacenters</span></span>

<span data-ttu-id="ec918-211">Det här avsnittet innehåller information om tillgängligheten för Media Services-funktioner i datacenter.</span><span class="sxs-lookup"><span data-stu-id="ec918-211">This section provides details about availability of Media Services features across datacenters.</span></span>

### <a name="ams-accounts"></a><span data-ttu-id="ec918-212">AMS-konton</span><span class="sxs-lookup"><span data-stu-id="ec918-212">AMS accounts</span></span>

#### <a name="availability"></a><span data-ttu-id="ec918-213">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="ec918-213">Availability</span></span>

<span data-ttu-id="ec918-214">Du kan skapa Media Services-konton i hello följande områden: Nordeuropa, Västeuropa, västra USA, östra USA, Sydostasien, Östasien, västra Japan, östra, södra, västra Indien, södra Indien och centrala Indien.</span><span class="sxs-lookup"><span data-stu-id="ec918-214">You can create Media Services accounts in hello following regions: North Europe, West Europe, West US, East US, Southeast Asia, East Asia, Japan West, Japan East, Brazil South, India West, India South, and India Central.</span></span> 

### <a name="streaming-endpoints"></a><span data-ttu-id="ec918-215">Slutpunkter för direktuppspelning</span><span class="sxs-lookup"><span data-stu-id="ec918-215">Streaming endpoints</span></span> 

<span data-ttu-id="ec918-216">Media Services-kunder kan antingen välja en **Standard**-slutpunkt för direktuppspelning eller en **Premium**-slutpunkt för direktuppspelning.</span><span class="sxs-lookup"><span data-stu-id="ec918-216">Media Services customers can choose either a **Standard** streaming endpoint or a **Premium** streaming endpoint.</span></span> <span data-ttu-id="ec918-217">Mer information finns i hello [skalning](#scaling) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ec918-217">For more information, see hello [scaling](#scaling) section.</span></span>

#### <a name="availability"></a><span data-ttu-id="ec918-218">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="ec918-218">Availability</span></span>

|<span data-ttu-id="ec918-219">Namn</span><span class="sxs-lookup"><span data-stu-id="ec918-219">Name</span></span>|<span data-ttu-id="ec918-220">Status</span><span class="sxs-lookup"><span data-stu-id="ec918-220">Status</span></span>|<span data-ttu-id="ec918-221">Datacenter</span><span class="sxs-lookup"><span data-stu-id="ec918-221">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="ec918-222">Standard</span><span class="sxs-lookup"><span data-stu-id="ec918-222">Standard</span></span>|<span data-ttu-id="ec918-223">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="ec918-223">GA</span></span>|<span data-ttu-id="ec918-224">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-224">All</span></span>|
|<span data-ttu-id="ec918-225">Premium</span><span class="sxs-lookup"><span data-stu-id="ec918-225">Premium</span></span>|<span data-ttu-id="ec918-226">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="ec918-226">GA</span></span>|<span data-ttu-id="ec918-227">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-227">All</span></span>|

### <a name="live-encoding"></a><span data-ttu-id="ec918-228">Live Encoding</span><span class="sxs-lookup"><span data-stu-id="ec918-228">Live encoding</span></span>

#### <a name="availability"></a><span data-ttu-id="ec918-229">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="ec918-229">Availability</span></span>

<span data-ttu-id="ec918-230">Tillgänglig i alla datacenter förutom: Tyskland, södra Brasilien, västra Indien, södra Indien och centrala Indien.</span><span class="sxs-lookup"><span data-stu-id="ec918-230">Available in all datacenters except: Germany, Brazil South, India West, India South, and India Central.</span></span> 

### <a name="encoding-media-processors"></a><span data-ttu-id="ec918-231">Mediebearbetare för kodning</span><span class="sxs-lookup"><span data-stu-id="ec918-231">Encoding media processors</span></span>

<span data-ttu-id="ec918-232">AMS erbjuder två kodare på begäran: **Media Encoder Standard** och **Media Encoder Premium Workflow**.</span><span class="sxs-lookup"><span data-stu-id="ec918-232">AMS offers two on-demand encoders **Media Encoder Standard** and **Media Encoder Premium Workflow**.</span></span> <span data-ttu-id="ec918-233">Mer information finns i [Overview and comparison of Azure on-demand media encoders](media-services-encode-asset.md) (Översikt och jämförelse av Azures mediekodare på begäran).</span><span class="sxs-lookup"><span data-stu-id="ec918-233">For more information, see [Overview and comparison of Azure on-demand media encoders](media-services-encode-asset.md).</span></span> 

#### <a name="availability"></a><span data-ttu-id="ec918-234">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="ec918-234">Availability</span></span>

|<span data-ttu-id="ec918-235">Namn på mediebearbetare</span><span class="sxs-lookup"><span data-stu-id="ec918-235">Media processor name</span></span>|<span data-ttu-id="ec918-236">Status</span><span class="sxs-lookup"><span data-stu-id="ec918-236">Status</span></span>|<span data-ttu-id="ec918-237">Datacenter</span><span class="sxs-lookup"><span data-stu-id="ec918-237">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="ec918-238">Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="ec918-238">Media Encoder Standard</span></span>|<span data-ttu-id="ec918-239">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="ec918-239">GA</span></span>|<span data-ttu-id="ec918-240">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-240">All</span></span>|
|<span data-ttu-id="ec918-241">Arbetsflöde för Media Encoder Premium</span><span class="sxs-lookup"><span data-stu-id="ec918-241">Media Encoder Premium Workflow</span></span>|<span data-ttu-id="ec918-242">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="ec918-242">GA</span></span>|<span data-ttu-id="ec918-243">Alla utom Kina</span><span class="sxs-lookup"><span data-stu-id="ec918-243">All except China</span></span>|

### <a name="analytics-media-processors"></a><span data-ttu-id="ec918-244">Mediebearbetare för analys</span><span class="sxs-lookup"><span data-stu-id="ec918-244">Analytics media processors</span></span>

<span data-ttu-id="ec918-245">Media Analytics är en samling tal-och visionskomponenter som gör det enklare för organisationer och företag tooderive tillämplig insikter från sina videofiler.</span><span class="sxs-lookup"><span data-stu-id="ec918-245">Media Analytics is a collection of speech and vision components that makes it easier for organizations and enterprises tooderive actionable insights from their video files.</span></span> <span data-ttu-id="ec918-246">Mer information finns i [Översikt över Azure Media Services Analytics](media-services-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ec918-246">For more information, see [Azure Media Services Analytics Overview](media-services-analytics-overview.md).</span></span>

#### <a name="availability"></a><span data-ttu-id="ec918-247">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="ec918-247">Availability</span></span>

|<span data-ttu-id="ec918-248">Namn på mediebearbetare</span><span class="sxs-lookup"><span data-stu-id="ec918-248">Media processor name</span></span>|<span data-ttu-id="ec918-249">Status</span><span class="sxs-lookup"><span data-stu-id="ec918-249">Status</span></span>|<span data-ttu-id="ec918-250">Datacenter</span><span class="sxs-lookup"><span data-stu-id="ec918-250">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="ec918-251">Azure Media Face Detector</span><span class="sxs-lookup"><span data-stu-id="ec918-251">Azure Media Face Detector</span></span>|<span data-ttu-id="ec918-252">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="ec918-252">Preview</span></span>|<span data-ttu-id="ec918-253">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-253">All</span></span>|
|<span data-ttu-id="ec918-254">Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="ec918-254">Azure Media Hyperlapse</span></span>|<span data-ttu-id="ec918-255">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="ec918-255">Preview</span></span>|<span data-ttu-id="ec918-256">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-256">All</span></span>|
|<span data-ttu-id="ec918-257">Azure Media Indexer</span><span class="sxs-lookup"><span data-stu-id="ec918-257">Azure Media Indexer</span></span>|<span data-ttu-id="ec918-258">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="ec918-258">GA</span></span>|<span data-ttu-id="ec918-259">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-259">All</span></span>|
|<span data-ttu-id="ec918-260">Azure Media Motion Detector</span><span class="sxs-lookup"><span data-stu-id="ec918-260">Azure Media Motion Detector</span></span>|<span data-ttu-id="ec918-261">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="ec918-261">Preview</span></span>|<span data-ttu-id="ec918-262">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-262">All</span></span>|
|<span data-ttu-id="ec918-263">Azure Media OCR</span><span class="sxs-lookup"><span data-stu-id="ec918-263">Azure Media OCR</span></span>|<span data-ttu-id="ec918-264">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="ec918-264">Preview</span></span>|<span data-ttu-id="ec918-265">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-265">All</span></span>|
|<span data-ttu-id="ec918-266">Azure Media Redactor</span><span class="sxs-lookup"><span data-stu-id="ec918-266">Azure Media Redactor</span></span>|<span data-ttu-id="ec918-267">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="ec918-267">Preview</span></span>|<span data-ttu-id="ec918-268">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-268">All</span></span>|
|<span data-ttu-id="ec918-269">Azure Media Stabilizer</span><span class="sxs-lookup"><span data-stu-id="ec918-269">Azure Media Stabilizer</span></span>|<span data-ttu-id="ec918-270">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="ec918-270">Preview</span></span>|<span data-ttu-id="ec918-271">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-271">All</span></span>|
|<span data-ttu-id="ec918-272">Azure Media Video Thumbnails</span><span class="sxs-lookup"><span data-stu-id="ec918-272">Azure Media Video Thumbnails</span></span>|<span data-ttu-id="ec918-273">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="ec918-273">Preview</span></span>|<span data-ttu-id="ec918-274">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-274">All</span></span>|
|<span data-ttu-id="ec918-275">Azure Media Indexer 2</span><span class="sxs-lookup"><span data-stu-id="ec918-275">Azure Media Indexer 2</span></span>|<span data-ttu-id="ec918-276">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="ec918-276">Preview</span></span>|<span data-ttu-id="ec918-277">Allt utom Kina och federala myndigheter</span><span class="sxs-lookup"><span data-stu-id="ec918-277">All except China and Federal Government region</span></span>|

### <a name="protection"></a><span data-ttu-id="ec918-278">Skydd</span><span class="sxs-lookup"><span data-stu-id="ec918-278">Protection</span></span>

<span data-ttu-id="ec918-279">Microsoft Azure Media Services kan du toosecure media från hello tid den lämnar din dator via lagring, bearbetning och leverans.</span><span class="sxs-lookup"><span data-stu-id="ec918-279">Microsoft Azure Media Services enables you toosecure your media from hello time it leaves your computer through storage, processing, and delivery.</span></span> <span data-ttu-id="ec918-280">Mer information finns i avsnittet om att [skydda AMS-innehåll](media-services-content-protection-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ec918-280">For more information, see [Protecting AMS content](media-services-content-protection-overview.md).</span></span>

#### <a name="availability"></a><span data-ttu-id="ec918-281">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="ec918-281">Availability</span></span>

|<span data-ttu-id="ec918-282">Kryptering</span><span class="sxs-lookup"><span data-stu-id="ec918-282">Encryption</span></span>|<span data-ttu-id="ec918-283">Status</span><span class="sxs-lookup"><span data-stu-id="ec918-283">Status</span></span>|<span data-ttu-id="ec918-284">Datacenter</span><span class="sxs-lookup"><span data-stu-id="ec918-284">Datacenters</span></span>|
|---|---|---| 
|<span data-ttu-id="ec918-285">Lagring</span><span class="sxs-lookup"><span data-stu-id="ec918-285">Storage</span></span>|<span data-ttu-id="ec918-286">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="ec918-286">GA</span></span>|<span data-ttu-id="ec918-287">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-287">All</span></span>|
|<span data-ttu-id="ec918-288">128-bitars AES-nycklar</span><span class="sxs-lookup"><span data-stu-id="ec918-288">AES-128 keys</span></span>|<span data-ttu-id="ec918-289">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="ec918-289">GA</span></span>|<span data-ttu-id="ec918-290">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-290">All</span></span>|
|<span data-ttu-id="ec918-291">Fairplay</span><span class="sxs-lookup"><span data-stu-id="ec918-291">Fairplay</span></span>|<span data-ttu-id="ec918-292">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="ec918-292">GA</span></span>|<span data-ttu-id="ec918-293">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-293">All</span></span>|
|<span data-ttu-id="ec918-294">PlayReady</span><span class="sxs-lookup"><span data-stu-id="ec918-294">PlayReady</span></span>|<span data-ttu-id="ec918-295">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="ec918-295">GA</span></span>|<span data-ttu-id="ec918-296">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-296">All</span></span>|
|<span data-ttu-id="ec918-297">Widevine</span><span class="sxs-lookup"><span data-stu-id="ec918-297">Widevine</span></span>|<span data-ttu-id="ec918-298">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="ec918-298">GA</span></span>|<span data-ttu-id="ec918-299">Alla utom Tyskland, federala myndigheter och Kina.</span><span class="sxs-lookup"><span data-stu-id="ec918-299">All except Germany, Federal Government and China.</span></span>

### <a name="reserved-units-rus"></a><span data-ttu-id="ec918-300">Reserverade enheter (RU:er)</span><span class="sxs-lookup"><span data-stu-id="ec918-300">Reserved units (RUs)</span></span>

<span data-ttu-id="ec918-301">hello antalet etablerade reserverade enheter avgör hello antal media uppgifter som kan bearbetas samtidigt i en viss konto.</span><span class="sxs-lookup"><span data-stu-id="ec918-301">hello number of provisioned reserved units determines hello number of media tasks that can be processed concurrently in a given account.</span></span> 

<span data-ttu-id="ec918-302">Mer information finns i hello [skalning](#scaling) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ec918-302">For more information, see hello [scaling](#scaling) section.</span></span>

#### <a name="availability"></a><span data-ttu-id="ec918-303">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="ec918-303">Availability</span></span>

<span data-ttu-id="ec918-304">Tillgängligt i alla datacenter.</span><span class="sxs-lookup"><span data-stu-id="ec918-304">Available in all datacenters.</span></span>

### <a name="reserved-unit-ru-type"></a><span data-ttu-id="ec918-305">Typ av reserverad enhet (RU)</span><span class="sxs-lookup"><span data-stu-id="ec918-305">Reserved unit (RU) type</span></span>

<span data-ttu-id="ec918-306">Ett Media Services-konto är kopplat till en enhetstyp, som avgör hello hastighet som media bearbeta uppgifter bearbetas.</span><span class="sxs-lookup"><span data-stu-id="ec918-306">A Media Services account is associated with a Reserved unit type, which determines hello speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="ec918-307">Du kan välja mellan följande hello reserverade enhetstyper: S1, S2 och S3.</span><span class="sxs-lookup"><span data-stu-id="ec918-307">You can pick between hello following reserved unit types: S1, S2, or S3.</span></span>

<span data-ttu-id="ec918-308">Mer information finns i hello [skalning](#scaling) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ec918-308">For more information, see hello [scaling](#scaling) section.</span></span>

#### <a name="availability"></a><span data-ttu-id="ec918-309">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="ec918-309">Availability</span></span>

|<span data-ttu-id="ec918-310">Namn på RU-typ</span><span class="sxs-lookup"><span data-stu-id="ec918-310">RU type name</span></span>|<span data-ttu-id="ec918-311">Status</span><span class="sxs-lookup"><span data-stu-id="ec918-311">Status</span></span>|<span data-ttu-id="ec918-312">Datacenter</span><span class="sxs-lookup"><span data-stu-id="ec918-312">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="ec918-313">S1</span><span class="sxs-lookup"><span data-stu-id="ec918-313">S1</span></span>|<span data-ttu-id="ec918-314">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="ec918-314">GA</span></span>|<span data-ttu-id="ec918-315">Alla</span><span class="sxs-lookup"><span data-stu-id="ec918-315">All</span></span>|
|<span data-ttu-id="ec918-316">S2</span><span class="sxs-lookup"><span data-stu-id="ec918-316">S2</span></span>|<span data-ttu-id="ec918-317">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="ec918-317">GA</span></span>|<span data-ttu-id="ec918-318">Alla utom södra Brasilien och västra Indien</span><span class="sxs-lookup"><span data-stu-id="ec918-318">All except Brazil South, and India West</span></span>|
|<span data-ttu-id="ec918-319">S3</span><span class="sxs-lookup"><span data-stu-id="ec918-319">S3</span></span>|<span data-ttu-id="ec918-320">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="ec918-320">GA</span></span>|<span data-ttu-id="ec918-321">Allt utom västra Indien</span><span class="sxs-lookup"><span data-stu-id="ec918-321">All except India West</span></span>|

## <a name="next-steps"></a><span data-ttu-id="ec918-322">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ec918-322">Next steps</span></span>

<span data-ttu-id="ec918-323">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="ec918-323">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="ec918-324">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="ec918-324">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

