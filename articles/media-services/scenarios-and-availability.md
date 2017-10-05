---
title: "Microsoft Azure Media Services-scenarier och tillgängligheten för funktioner i datacenter | Microsoft Docs"
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
ms.openlocfilehash: d9994dd7bfb6b6bf949a7708c07651d667929ae4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="scenarios-and-availability-of-media-services-features-across-datacenters"></a><span data-ttu-id="6ea5e-103">Scenarier och tillgängligheten för Media Services-funktioner i datacenter</span><span class="sxs-lookup"><span data-stu-id="6ea5e-103">Scenarios and availability of Media Services features across datacenters</span></span>

<span data-ttu-id="6ea5e-104">Microsoft Azure Media Services (AMS) gör det möjligt att på ett säkert sätt överföra, lagra, koda och paketera video- eller ljudinnehåll för att strömma både på begäran och live till olika klienter (till exempel TV, datorer och mobila enheter).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-104">Microsoft Azure Media Services (AMS) enables you to securely upload, store, encode, and package video or audio content for both on-demand and live streaming delivery to various clients (for example, TV, PC, and mobile devices).</span></span>

<span data-ttu-id="6ea5e-105">AMS körs på ett antal datacenter över hela världen.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-105">AMS operates in multiple data centers around the world.</span></span> <span data-ttu-id="6ea5e-106">Dessa datacenter är grupperade i geografiska regioner så att du kan välja var du vill bygga dina program.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-106">These data centers are grouped in to geographic regions, giving you flexibility in choosing where to build your applications.</span></span> <span data-ttu-id="6ea5e-107">Se [listan över regioner och deras platser](https://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-107">You can review the [list of regions and their locations](https://azure.microsoft.com/regions/).</span></span> 

<span data-ttu-id="6ea5e-108">Det här avsnittet beskriver vanliga scenarier för att leverera innehåll [live](#live_scenarios) eller [på begäran](#vod_scenarios).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-108">This topic shows common scenarios for delivering your content [live](#live_scenarios) or [on-demand](#vod_scenarios).</span></span> <span data-ttu-id="6ea5e-109">Avsnittet innehåller också information om tillgängligheten för mediefunktioner och tjänster i datacenter.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-109">The topic also provides details about availability of media features and services across data centers.</span></span>

## <a name="overview"></a><span data-ttu-id="6ea5e-110">Översikt</span><span class="sxs-lookup"><span data-stu-id="6ea5e-110">Overview</span></span>

### <a name="prerequisites"></a><span data-ttu-id="6ea5e-111">Krav</span><span class="sxs-lookup"><span data-stu-id="6ea5e-111">Prerequisites</span></span>

<span data-ttu-id="6ea5e-112">Om du vill börja använda Azure Media Services ska du ha följande:</span><span class="sxs-lookup"><span data-stu-id="6ea5e-112">To start using Azure Media Services, you should have the following:</span></span>

* <span data-ttu-id="6ea5e-113">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-113">An Azure account.</span></span> <span data-ttu-id="6ea5e-114">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="6ea5e-115">Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-115">For details, see [Azure Free Trial](https://azure.microsoft.com).</span></span>
* <span data-ttu-id="6ea5e-116">Ett Azure Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-116">An Azure Media Services account.</span></span> <span data-ttu-id="6ea5e-117">Mer information finns i [Skapa konto](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-117">For more information, see [Create Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="6ea5e-118">Slutpunkten för direktuppspelning som du vill spela upp innehåll från måste ha tillståndet **Körs**.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-118">The streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span>

    <span data-ttu-id="6ea5e-119">När ditt AMS-konto skapas läggs en **standard**-slutpunkt för direktuppspelning till på ditt konto med tillståndet **Stoppad**.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-119">When your AMS account is created, a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="6ea5e-120">Om du vill starta direktuppspelning av innehåll och dra nytta av dynamisk paketering och dynamisk kryptering måste slutpunkten för direktuppspelning ha tillståndet **Körs**.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-120">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint has to be in the **Running** state.</span></span>

### <a name="commonly-used-objects-when-developing-against-the-ams-odata-model"></a><span data-ttu-id="6ea5e-121">Vanliga objekt när du utvecklar mot AMS OData-modellen</span><span class="sxs-lookup"><span data-stu-id="6ea5e-121">Commonly used objects when developing against the AMS OData model</span></span>

<span data-ttu-id="6ea5e-122">Följande bild visar några av de vanligast använda objekten när du utvecklar mot Media Services OData-modellen.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-122">The following image shows some of the most commonly used objects when developing against the Media Services OData model.</span></span>

<span data-ttu-id="6ea5e-123">Klicka på bilden för att visa den i full storlek.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-123">Click the image to view it full size.</span></span>  

<a href="./media/media-services-overview/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-overview/media-services-overview-object-model-small.png"></a> 

<span data-ttu-id="6ea5e-124">Du kan visa hela modellen [här](https://media.windows.net/API/$metadata?api-version=2.15).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-124">You can view the whole model [here](https://media.windows.net/API/$metadata?api-version=2.15).</span></span>  

## <a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a><span data-ttu-id="6ea5e-125">Skydda lagrat innehåll och leverera strömmande media i klartext (okrypterat)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-125">Protect content in storage and deliver streaming media in the clear (non-encrypted)</span></span>

![VoD-arbetsflöde](./media/scenarios-and-availability/scenarios-and-availability01.png)

1. <span data-ttu-id="6ea5e-127">Överför en mediefil av hög kvalitet till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-127">Upload a high-quality media file into an asset.</span></span>

    <span data-ttu-id="6ea5e-128">Vi rekommenderar att du använder ett krypterat lagringsalternativ för tillgången så att du skyddar innehållet under överföringen och i vila i lagring.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-128">It is recommended to apply storage encryption option to your asset in order to protect your content during upload and while at rest in storage.</span></span>
2. <span data-ttu-id="6ea5e-129">Koda till en uppsättning MP4-filer med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-129">Encode to a set of adaptive bitrate MP4 files.</span></span>

    <span data-ttu-id="6ea5e-130">Vi rekommenderar att du använder ett krypterat lagringsalternativ för utdatatillgången så att du skyddar innehållet i vila.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-130">It is recommended to apply storage encryption option to the output asset in order to protect your content at rest.</span></span>
3. <span data-ttu-id="6ea5e-131">Konfigurera principen för tillgångsleverans (används av dynamisk paketering).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-131">Configure asset delivery policy (used by dynamic packaging).</span></span>

    <span data-ttu-id="6ea5e-132">Om tillgången är lagringskrypterad **måste** du konfigurera principen för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-132">If your asset is storage encrypted, you **must** configure asset delivery policy.</span></span>
4. <span data-ttu-id="6ea5e-133">Publicera tillgången genom att skapa en OnDemand-positionerare.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-133">Publish the asset by creating an OnDemand locator.</span></span>
5. <span data-ttu-id="6ea5e-134">Strömma publicerat innehåll.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-134">Stream published content.</span></span>

<span data-ttu-id="6ea5e-135">Information om tillgänglighet i datacenter finns i avsnittet [Tillgänglighet](#availability).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-135">For information about availability in datacenters, see the [Availability](#availability) section.</span></span>

## <a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a><span data-ttu-id="6ea5e-136">Skydda lagrat innehåll, leverera dynamiskt krypterade strömmande media</span><span class="sxs-lookup"><span data-stu-id="6ea5e-136">Protect content in storage, deliver dynamically encrypted streaming media</span></span>

![Skydda med PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

1. <span data-ttu-id="6ea5e-138">Överför en mediefil av hög kvalitet till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-138">Upload a high-quality media file into an asset.</span></span> <span data-ttu-id="6ea5e-139">Använd krypterat lagringsalternativ för tillgången.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-139">Apply storage encryption option to the asset.</span></span>
2. <span data-ttu-id="6ea5e-140">Koda till en uppsättning MP4-filer med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-140">Encode to a set of adaptive bitrate MP4 files.</span></span> <span data-ttu-id="6ea5e-141">Använd krypterat lagringsalternativ för utdatatillgången.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-141">Apply storage encryption option to the output asset.</span></span>
3. <span data-ttu-id="6ea5e-142">Skapa en innehållskrypteringsnyckel för den tillgång du vill kryptera dynamiskt under uppspelning.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-142">Create encryption content key for the asset you want to be dynamically encrypted during playback.</span></span>
4. <span data-ttu-id="6ea5e-143">Konfigurera principen för auktorisering av innehållsnyckel.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-143">Configure content key authorization policy.</span></span>
5. <span data-ttu-id="6ea5e-144">Konfigurera principen för tillgångsleverans (används av dynamisk paketering och dynamisk kryptering).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-144">Configure asset delivery policy (used by dynamic packaging and dynamic encryption).</span></span>
6. <span data-ttu-id="6ea5e-145">Publicera tillgången genom att skapa en OnDemand-positionerare.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-145">Publish the asset by creating an OnDemand locator.</span></span>
7. <span data-ttu-id="6ea5e-146">Strömma publicerat innehåll.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-146">Stream published content.</span></span>

<span data-ttu-id="6ea5e-147">Information om tillgänglighet i datacenter finns i avsnittet [Tillgänglighet](#availability).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-147">For information about availability in datacenters, see the [Availability](#availability) section.</span></span>

## <a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a><span data-ttu-id="6ea5e-148">Använd Media Analytics för att härleda insikter som går att åtgärda från dina videor</span><span class="sxs-lookup"><span data-stu-id="6ea5e-148">Use Media Analytics to derive actionable insights from your videos</span></span>

<span data-ttu-id="6ea5e-149">Media Analytics är en samling tal- och visionskomponenter som gör det enklare för organisationer och företag att härleda insikter som går att åtgärda från sina videofiler.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-149">Media Analytics is a collection of speech and vision components that make it easier for organizations and enterprises to derive actionable insights from their video files.</span></span> <span data-ttu-id="6ea5e-150">Mer information finns i [Översikt över Azure Media Services Analytics](media-services-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-150">For more information, see [Azure Media Services Analytics Overview](media-services-analytics-overview.md).</span></span>

1. <span data-ttu-id="6ea5e-151">Överför en mediefil av hög kvalitet till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-151">Upload a high-quality media file into an asset.</span></span>
2. <span data-ttu-id="6ea5e-152">Bearbeta videor med någon av de medieanalystjänster som beskrivs i [översikten över medieanalys](media-services-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-152">Process your videos with one of the Media Analytics services described in the [Media Analytics overview](media-services-analytics-overview.md) section.</span></span>
3. <span data-ttu-id="6ea5e-153">Media Analytics-medieprocessorer producerar MP4-filer eller JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-153">Media Analytics media processors produce MP4 files or JSON files.</span></span> <span data-ttu-id="6ea5e-154">Om en medieprocessor har producerat en MP4-fil kan du hämta filen progressivt.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-154">If a media processor produced an MP4 file, you can progressively download the file.</span></span> <span data-ttu-id="6ea5e-155">Om en medieprocessor har producerat en JSON-fil kan du hämta filen från Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-155">If a media processor produced a JSON file, you can download the file from the Azure blob storage.</span></span>

<span data-ttu-id="6ea5e-156">Information om tillgänglighet i datacenter finns i avsnittet [Tillgänglighet](#availability).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-156">For information about availability in datacenters, see the [Availability](#availability) section.</span></span>

## <a name="deliver-progressive-download"></a><span data-ttu-id="6ea5e-157">Leverera progressiv nedladdning</span><span class="sxs-lookup"><span data-stu-id="6ea5e-157">Deliver progressive download</span></span>

1. <span data-ttu-id="6ea5e-158">Överför en mediefil av hög kvalitet till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-158">Upload a high-quality media file into an asset.</span></span>
2. <span data-ttu-id="6ea5e-159">Koda till en enda MP4-fil.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-159">Encode to a single MP4 file.</span></span>
3. <span data-ttu-id="6ea5e-160">Publicera tillgången genom att skapa en positionerare på begäran eller en SAS-positionerare.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-160">Publish the asset by creating an OnDemand or SAS locator.</span></span>

    <span data-ttu-id="6ea5e-161">Om du använder en SAS-positionerare hämtas innehåll från Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-161">If using SAS locator, the content is downloaded from the Azure blob storage.</span></span> <span data-ttu-id="6ea5e-162">I så fall behöver du inte ha slutpunkter för direktuppspelning med tillståndet Startad.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-162">In this case, you do not need to have streaming endpoints in started state.</span></span>
4. <span data-ttu-id="6ea5e-163">Progressivt hämtat innehåll</span><span class="sxs-lookup"><span data-stu-id="6ea5e-163">Progressively download content.</span></span>

## <span data-ttu-id="6ea5e-164"><a id="live_scenarios"></a>Leverera liveuppspelningshändelser</span><span class="sxs-lookup"><span data-stu-id="6ea5e-164"><a id="live_scenarios"></a>Delivering live-streaming events</span></span> 

1. <span data-ttu-id="6ea5e-165">Infoga innehåll via olika protokoll för liveuppspelning (till exempel RTMP eller Smooth Streaming).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-165">Ingest live content using various live streaming protocols (for example RTMP or Smooth Streaming).</span></span>
2. <span data-ttu-id="6ea5e-166">(Valfritt) Koda strömmen till ström med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-166">(optionally) Encode your stream into adaptive bitrate stream.</span></span>
3. <span data-ttu-id="6ea5e-167">Förhandsgranska din liveuppspelning.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-167">Preview your live stream.</span></span>
4. <span data-ttu-id="6ea5e-168">Leverera innehållet via vanliga strömningsprotokoll (t.ex. MPEG DASH, Smooth, HLS) direkt till dina kunder eller till ett nätverk för innehållsleverans för vidare distribution.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-168">Deliver the content through common streaming protocols (for example, MPEG DASH, Smooth, HLS) directly to your customers, or to a Content Delivery Network (CDN) for further distribution.</span></span>

    <span data-ttu-id="6ea5e-169">ELLER</span><span class="sxs-lookup"><span data-stu-id="6ea5e-169">-or-</span></span>

    <span data-ttu-id="6ea5e-170">Spela in och lagra det infogade innehållet för att kunna strömma senare (Video-on-Demand).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-170">Record and store the ingested content in order to be streamed later (Video-on-Demand).</span></span>

<span data-ttu-id="6ea5e-171">Vid liveuppspelning kan du välja någon av följande vägar:</span><span class="sxs-lookup"><span data-stu-id="6ea5e-171">When doing live streaming, you can choose one of the following routes:</span></span>

### <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a><span data-ttu-id="6ea5e-172">Arbeta med kanaler som tar emot direktsänd ström med flera bithastigheter från lokala kodare (genomströmning)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-172">Working with channels that receive multi-bitrate live stream from on-premises encoders (pass-through)</span></span>

<span data-ttu-id="6ea5e-173">I följande diagram visas de huvudsakliga delarna i AMS-plattformen som ingår i arbetsflödet **Genomströmning**.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-173">The following diagram shows the major parts of the AMS platform that are involved in the **pass-through** workflow.</span></span>

![Live-arbetsflöde](./media/scenarios-and-availability/media-services-live-streaming-current.png)

<span data-ttu-id="6ea5e-175">Mer information finns i [Arbeta med kanaler som tar emot liveström med flera bithastigheter från lokala kodare](media-services-live-streaming-with-onprem-encoders.md).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-175">For more information, see [Working with Channels that Receive Multi-bitrate Live Stream from On-premises Encoders](media-services-live-streaming-with-onprem-encoders.md).</span></span>

### <a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a><span data-ttu-id="6ea5e-176">Arbeta med kanaler som är aktiverade för att utföra Live Encoding med Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="6ea5e-176">Working with channels that are enabled to perform live encoding with Azure Media Services</span></span>

<span data-ttu-id="6ea5e-177">I följande diagram visas de huvudsakliga delarna i AMS-plattformen som ingår i arbetsflödet för liveuppspelning där en kanal är aktiverad för att utföra Live Encoding med Media Services.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-177">The following diagram shows the major parts of the AMS platform that are involved in Live Streaming workflow where a Channel is enabled to perform live encoding with Media Services.</span></span>

![Live-arbetsflöde](./media/scenarios-and-availability/media-services-live-streaming-new.png)

<span data-ttu-id="6ea5e-179">Mer information finns i [Arbeta med kanaler som är aktiverade för att utföra Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-179">For more information, see [Working with Channels that are Enabled to Perform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="6ea5e-180">Information om tillgänglighet i datacenter finns i avsnittet [Tillgänglighet](#availability).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-180">For information about availability in datacenters, see the [Availability](#availability) section.</span></span>

## <a name="consuming-content"></a><span data-ttu-id="6ea5e-181">Konsumera innehåll</span><span class="sxs-lookup"><span data-stu-id="6ea5e-181">Consuming content</span></span>

<span data-ttu-id="6ea5e-182">Azure Media Services innehåller de verktyg du behöver för att skapa innehållsrika och dynamiska klientprogram för uppspelare för de flesta plattformar, bland annat: iOS-enheter, Android-enheter, Windows, Windows Phone, Xbox och digitalboxar.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-182">Azure Media Services provides the tools you need to create rich, dynamic client player applications for most platforms including: iOS Devices, Android Devices, Windows, Windows Phone, Xbox, and Set-top boxes.</span></span> <span data-ttu-id="6ea5e-183">Följande avsnitt innehåller länkar till SDK:er och spelarramverk som du kan använda för att utveckla egna klientprogram som kan använda strömmande media från Media Services.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-183">The following topic provides links to SDKs and Player Frameworks that you can use to develop your own client applications that can consume streaming media from Media Services.</span></span> <span data-ttu-id="6ea5e-184">Mer information finns i [Developing video player applications](media-services-develop-video-players.md) (Utveckla videospelarprogram)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-184">For more information, see [Developing video payer applications](media-services-develop-video-players.md)</span></span>

## <a name="enabling-azure-cdn"></a><span data-ttu-id="6ea5e-185">Aktivera Azure CDN</span><span class="sxs-lookup"><span data-stu-id="6ea5e-185">Enabling Azure CDN</span></span>

<span data-ttu-id="6ea5e-186">Media Services stöder integration med Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-186">Media Services supports integration with Azure CDN.</span></span> <span data-ttu-id="6ea5e-187">Information om hur du aktiverar Azure CDN finns i [Hur du hanterar strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-187">For information on how to enable Azure CDN, see [How to Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md).</span></span>

## <span data-ttu-id="6ea5e-188"><a id="scaling"></a>Skala ett Media Services-konto</span><span class="sxs-lookup"><span data-stu-id="6ea5e-188"><a id="scaling"></a>Scaling a Media Services account</span></span>

<span data-ttu-id="6ea5e-189">AMS-kunder kan skala slutpunkter för direktuppspelning, mediebearbetning och lagring i sina AMS-konton.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-189">AMS customers can scale streaming endpoints, media processing, and storage in their AMS accounts.</span></span>

* <span data-ttu-id="6ea5e-190">Media Services-kunder kan antingen välja en **Standard**-slutpunkt för direktuppspelning eller en **Premium**-slutpunkt för direktuppspelning.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-190">Media Services customers can choose either a **Standard** streaming endpoint or a **Premium** streaming endpoint.</span></span> <span data-ttu-id="6ea5e-191">En **Standard**-slutpunkt för direktuppspelning passar de flesta arbetsbelastningar för direktuppspelning.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-191">A **Standard** streaming endpoint is suitable for most streaming workloads.</span></span> <span data-ttu-id="6ea5e-192">Innehåller samma funktioner som **Premium**-slutpunkter för direktuppspelning och skalar utgående bandbredd automatiskt.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-192">It includes the same features as a **Premium** streaming endpoints and scales outbound bandwidth automatically.</span></span> 

    <span data-ttu-id="6ea5e-193">**Premium**-slutpunkter för direktuppspelning passar för avancerade arbetsbelastningar och tillhandahåller dedikerad och skalbar bandbreddskapacitet.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-193">**Premium** streaming endpoints are suitable for advanced workloads, providing dedicated and scalable bandwidth capacity.</span></span> <span data-ttu-id="6ea5e-194">Kunder som har en **Premium**-slutpunkt för direktuppspelning får som standard en direktuppspelande enhet (SU).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-194">Customers that have a **Premium** streaming endpoint, by default get one streaming unit (SU).</span></span> <span data-ttu-id="6ea5e-195">Du kan skala slutpunkten för direktuppspelning genom att lägga till direktuppspelande enheter.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-195">The streaming endpoint can be scaled by adding SUs.</span></span> <span data-ttu-id="6ea5e-196">Varje direktuppspelande enhet ger ytterligare bandbreddskapacitet till programmet.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-196">Each SU provides additional bandwidth capacity to the application.</span></span> <span data-ttu-id="6ea5e-197">Mer information om hur du skalar **Premium**-slutpunkter för direktuppspelning finns i avsnittet [Scaling streaming endpoints](media-services-portal-scale-streaming-endpoints.md) (Skala slutpunkter för direktuppspelning).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-197">For more information about scaling **Premium** streaming endpoints, see the [Scaling streaming endpoints](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

* <span data-ttu-id="6ea5e-198">Ett Media Services-konto är kopplat till en typ av reserverad enhet som bestämmer hur snabbt mediebearbetningsuppgifter ska bearbetas.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-198">A Media Services account is associated with a Reserved Unit Type, which determines the speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="6ea5e-199">Du kan välja mellan följande typer av reserverade enheter: **S1**, **S2** och **S3**.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-199">You can pick between the following reserved unit types: **S1**, **S2**, or **S3**.</span></span> <span data-ttu-id="6ea5e-200">Samma kodningsjobb körs till exempel snabbare om du använder typen **S2** än om du använder typen **S1**.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-200">For example, the same encoding job runs faster when you use the **S2** reserved unit type compare to the **S1** type.</span></span>

    <span data-ttu-id="6ea5e-201">Förutom att ange typ av reserverad enhet kan du etablera **reserverade enheter** (RU:er) för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-201">In addition to specifying the reserved unit type, you can specify to provision your account with **Reserved Units** (RUs).</span></span> <span data-ttu-id="6ea5e-202">Antalet etablerade RU:er anger antalet medieuppgifter som kan bearbetas samtidigt i en viss konto.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-202">The number of provisioned RUs determines the number of media tasks that can be processed concurrently in a given account.</span></span>

    >[!NOTE]
    ><span data-ttu-id="6ea5e-203">RU:er fungerar för parallellisera all bearbetning av media, inklusive indexeringsjobb med hjälp av Azure Media Indexer.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-203">RUs work for parallelizing all media processing, including indexing jobs using Azure Media Indexer.</span></span> <span data-ttu-id="6ea5e-204">Men till skillnad från kodning bearbetas inte indexeringsjobb snabbare med snabbare reserverade enheter.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-204">However, unlike encoding, indexing jobs do not get processed faster with faster reserved units.</span></span>

    <span data-ttu-id="6ea5e-205">Mer information finns i [Skala mediebearbetning](media-services-portal-scale-media-processing.md).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-205">For more information see, [Scale media processing](media-services-portal-scale-media-processing.md).</span></span>
* <span data-ttu-id="6ea5e-206">Du kan även skala ditt Media Services-konto genom att lägga till lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-206">You can also scale your Media Services account by adding storage accounts to it.</span></span> <span data-ttu-id="6ea5e-207">Varje lagringskonto är begränsat till 500 TB.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-207">Each storage account is limited to 500 TB.</span></span> <span data-ttu-id="6ea5e-208">Du kan välja att koppla flera lagringskonton för till ett enda Media Services-konto om du vill expandera din lagring utöver standardbegränsningarna.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-208">To expand your storage beyond the default limitations, you can choose to attach multiple storage accounts to a single Media Services account.</span></span> <span data-ttu-id="6ea5e-209">Mer information finns i [Hantera lagringskonton](meda-services-managing-multiple-storage-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-209">For more information, see [Manage storage accounts](meda-services-managing-multiple-storage-accounts.md).</span></span>

##<span data-ttu-id="6ea5e-210"><a id="availability"></a> Tillgänglighet för Media Services-funktioner i datacenter</span><span class="sxs-lookup"><span data-stu-id="6ea5e-210"><a id="availability"></a> Availability of Media Services features across datacenters</span></span>

<span data-ttu-id="6ea5e-211">Det här avsnittet innehåller information om tillgängligheten för Media Services-funktioner i datacenter.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-211">This section provides details about availability of Media Services features across datacenters.</span></span>

### <a name="ams-accounts"></a><span data-ttu-id="6ea5e-212">AMS-konton</span><span class="sxs-lookup"><span data-stu-id="6ea5e-212">AMS accounts</span></span>

#### <a name="availability"></a><span data-ttu-id="6ea5e-213">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="6ea5e-213">Availability</span></span>

<span data-ttu-id="6ea5e-214">Du kan skapa Media Services-konton i följande regioner: Nordeuropa, Västeuropa, västra USA, östra USA, sydöstra Asien, östra Asien, västra Japan, östra Japan, södra Brasilien, västra Indien, södra Indien och centrala Indien.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-214">You can create Media Services accounts in the following regions: North Europe, West Europe, West US, East US, Southeast Asia, East Asia, Japan West, Japan East, Brazil South, India West, India South, and India Central.</span></span> 

### <a name="streaming-endpoints"></a><span data-ttu-id="6ea5e-215">Slutpunkter för direktuppspelning</span><span class="sxs-lookup"><span data-stu-id="6ea5e-215">Streaming endpoints</span></span> 

<span data-ttu-id="6ea5e-216">Media Services-kunder kan antingen välja en **Standard**-slutpunkt för direktuppspelning eller en **Premium**-slutpunkt för direktuppspelning.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-216">Media Services customers can choose either a **Standard** streaming endpoint or a **Premium** streaming endpoint.</span></span> <span data-ttu-id="6ea5e-217">Mer information finns i avsnittet om [skalning](#scaling).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-217">For more information, see the [scaling](#scaling) section.</span></span>

#### <a name="availability"></a><span data-ttu-id="6ea5e-218">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="6ea5e-218">Availability</span></span>

|<span data-ttu-id="6ea5e-219">Namn</span><span class="sxs-lookup"><span data-stu-id="6ea5e-219">Name</span></span>|<span data-ttu-id="6ea5e-220">Status</span><span class="sxs-lookup"><span data-stu-id="6ea5e-220">Status</span></span>|<span data-ttu-id="6ea5e-221">Datacenter</span><span class="sxs-lookup"><span data-stu-id="6ea5e-221">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="6ea5e-222">Standard</span><span class="sxs-lookup"><span data-stu-id="6ea5e-222">Standard</span></span>|<span data-ttu-id="6ea5e-223">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-223">GA</span></span>|<span data-ttu-id="6ea5e-224">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-224">All</span></span>|
|<span data-ttu-id="6ea5e-225">Premium</span><span class="sxs-lookup"><span data-stu-id="6ea5e-225">Premium</span></span>|<span data-ttu-id="6ea5e-226">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-226">GA</span></span>|<span data-ttu-id="6ea5e-227">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-227">All</span></span>|

### <a name="live-encoding"></a><span data-ttu-id="6ea5e-228">Live Encoding</span><span class="sxs-lookup"><span data-stu-id="6ea5e-228">Live encoding</span></span>

#### <a name="availability"></a><span data-ttu-id="6ea5e-229">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="6ea5e-229">Availability</span></span>

<span data-ttu-id="6ea5e-230">Tillgänglig i alla datacenter förutom: Tyskland, södra Brasilien, västra Indien, södra Indien och centrala Indien.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-230">Available in all datacenters except: Germany, Brazil South, India West, India South, and India Central.</span></span> 

### <a name="encoding-media-processors"></a><span data-ttu-id="6ea5e-231">Mediebearbetare för kodning</span><span class="sxs-lookup"><span data-stu-id="6ea5e-231">Encoding media processors</span></span>

<span data-ttu-id="6ea5e-232">AMS erbjuder två kodare på begäran: **Media Encoder Standard** och **Media Encoder Premium Workflow**.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-232">AMS offers two on-demand encoders **Media Encoder Standard** and **Media Encoder Premium Workflow**.</span></span> <span data-ttu-id="6ea5e-233">Mer information finns i [Overview and comparison of Azure on-demand media encoders](media-services-encode-asset.md) (Översikt och jämförelse av Azures mediekodare på begäran).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-233">For more information, see [Overview and comparison of Azure on-demand media encoders](media-services-encode-asset.md).</span></span> 

#### <a name="availability"></a><span data-ttu-id="6ea5e-234">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="6ea5e-234">Availability</span></span>

|<span data-ttu-id="6ea5e-235">Namn på mediebearbetare</span><span class="sxs-lookup"><span data-stu-id="6ea5e-235">Media processor name</span></span>|<span data-ttu-id="6ea5e-236">Status</span><span class="sxs-lookup"><span data-stu-id="6ea5e-236">Status</span></span>|<span data-ttu-id="6ea5e-237">Datacenter</span><span class="sxs-lookup"><span data-stu-id="6ea5e-237">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="6ea5e-238">Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="6ea5e-238">Media Encoder Standard</span></span>|<span data-ttu-id="6ea5e-239">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-239">GA</span></span>|<span data-ttu-id="6ea5e-240">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-240">All</span></span>|
|<span data-ttu-id="6ea5e-241">Arbetsflöde för Media Encoder Premium</span><span class="sxs-lookup"><span data-stu-id="6ea5e-241">Media Encoder Premium Workflow</span></span>|<span data-ttu-id="6ea5e-242">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-242">GA</span></span>|<span data-ttu-id="6ea5e-243">Alla utom Kina</span><span class="sxs-lookup"><span data-stu-id="6ea5e-243">All except China</span></span>|

### <a name="analytics-media-processors"></a><span data-ttu-id="6ea5e-244">Mediebearbetare för analys</span><span class="sxs-lookup"><span data-stu-id="6ea5e-244">Analytics media processors</span></span>

<span data-ttu-id="6ea5e-245">Media Analytics är en samling tal- och visionskomponenter som gör det enklare för organisationer och företag att härleda insikter som det går att direkt agera utifrån från sina videofiler.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-245">Media Analytics is a collection of speech and vision components that makes it easier for organizations and enterprises to derive actionable insights from their video files.</span></span> <span data-ttu-id="6ea5e-246">Mer information finns i [Översikt över Azure Media Services Analytics](media-services-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-246">For more information, see [Azure Media Services Analytics Overview](media-services-analytics-overview.md).</span></span>

#### <a name="availability"></a><span data-ttu-id="6ea5e-247">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="6ea5e-247">Availability</span></span>

|<span data-ttu-id="6ea5e-248">Namn på mediebearbetare</span><span class="sxs-lookup"><span data-stu-id="6ea5e-248">Media processor name</span></span>|<span data-ttu-id="6ea5e-249">Status</span><span class="sxs-lookup"><span data-stu-id="6ea5e-249">Status</span></span>|<span data-ttu-id="6ea5e-250">Datacenter</span><span class="sxs-lookup"><span data-stu-id="6ea5e-250">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="6ea5e-251">Azure Media Face Detector</span><span class="sxs-lookup"><span data-stu-id="6ea5e-251">Azure Media Face Detector</span></span>|<span data-ttu-id="6ea5e-252">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="6ea5e-252">Preview</span></span>|<span data-ttu-id="6ea5e-253">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-253">All</span></span>|
|<span data-ttu-id="6ea5e-254">Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="6ea5e-254">Azure Media Hyperlapse</span></span>|<span data-ttu-id="6ea5e-255">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="6ea5e-255">Preview</span></span>|<span data-ttu-id="6ea5e-256">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-256">All</span></span>|
|<span data-ttu-id="6ea5e-257">Azure Media Indexer</span><span class="sxs-lookup"><span data-stu-id="6ea5e-257">Azure Media Indexer</span></span>|<span data-ttu-id="6ea5e-258">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-258">GA</span></span>|<span data-ttu-id="6ea5e-259">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-259">All</span></span>|
|<span data-ttu-id="6ea5e-260">Azure Media Motion Detector</span><span class="sxs-lookup"><span data-stu-id="6ea5e-260">Azure Media Motion Detector</span></span>|<span data-ttu-id="6ea5e-261">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="6ea5e-261">Preview</span></span>|<span data-ttu-id="6ea5e-262">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-262">All</span></span>|
|<span data-ttu-id="6ea5e-263">Azure Media OCR</span><span class="sxs-lookup"><span data-stu-id="6ea5e-263">Azure Media OCR</span></span>|<span data-ttu-id="6ea5e-264">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="6ea5e-264">Preview</span></span>|<span data-ttu-id="6ea5e-265">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-265">All</span></span>|
|<span data-ttu-id="6ea5e-266">Azure Media Redactor</span><span class="sxs-lookup"><span data-stu-id="6ea5e-266">Azure Media Redactor</span></span>|<span data-ttu-id="6ea5e-267">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="6ea5e-267">Preview</span></span>|<span data-ttu-id="6ea5e-268">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-268">All</span></span>|
|<span data-ttu-id="6ea5e-269">Azure Media Stabilizer</span><span class="sxs-lookup"><span data-stu-id="6ea5e-269">Azure Media Stabilizer</span></span>|<span data-ttu-id="6ea5e-270">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="6ea5e-270">Preview</span></span>|<span data-ttu-id="6ea5e-271">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-271">All</span></span>|
|<span data-ttu-id="6ea5e-272">Azure Media Video Thumbnails</span><span class="sxs-lookup"><span data-stu-id="6ea5e-272">Azure Media Video Thumbnails</span></span>|<span data-ttu-id="6ea5e-273">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="6ea5e-273">Preview</span></span>|<span data-ttu-id="6ea5e-274">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-274">All</span></span>|
|<span data-ttu-id="6ea5e-275">Azure Media Indexer 2</span><span class="sxs-lookup"><span data-stu-id="6ea5e-275">Azure Media Indexer 2</span></span>|<span data-ttu-id="6ea5e-276">Förhandsversion</span><span class="sxs-lookup"><span data-stu-id="6ea5e-276">Preview</span></span>|<span data-ttu-id="6ea5e-277">Allt utom Kina och federala myndigheter</span><span class="sxs-lookup"><span data-stu-id="6ea5e-277">All except China and Federal Government region</span></span>|

### <a name="protection"></a><span data-ttu-id="6ea5e-278">Skydd</span><span class="sxs-lookup"><span data-stu-id="6ea5e-278">Protection</span></span>

<span data-ttu-id="6ea5e-279">Med Microsoft Azure Media Services kan du skydda dina mediefiler från att filerna lämnar din dator och medan de lagras, bearbetas och levereras.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-279">Microsoft Azure Media Services enables you to secure your media from the time it leaves your computer through storage, processing, and delivery.</span></span> <span data-ttu-id="6ea5e-280">Mer information finns i avsnittet om att [skydda AMS-innehåll](media-services-content-protection-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-280">For more information, see [Protecting AMS content](media-services-content-protection-overview.md).</span></span>

#### <a name="availability"></a><span data-ttu-id="6ea5e-281">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="6ea5e-281">Availability</span></span>

|<span data-ttu-id="6ea5e-282">Kryptering</span><span class="sxs-lookup"><span data-stu-id="6ea5e-282">Encryption</span></span>|<span data-ttu-id="6ea5e-283">Status</span><span class="sxs-lookup"><span data-stu-id="6ea5e-283">Status</span></span>|<span data-ttu-id="6ea5e-284">Datacenter</span><span class="sxs-lookup"><span data-stu-id="6ea5e-284">Datacenters</span></span>|
|---|---|---| 
|<span data-ttu-id="6ea5e-285">Lagring</span><span class="sxs-lookup"><span data-stu-id="6ea5e-285">Storage</span></span>|<span data-ttu-id="6ea5e-286">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-286">GA</span></span>|<span data-ttu-id="6ea5e-287">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-287">All</span></span>|
|<span data-ttu-id="6ea5e-288">128-bitars AES-nycklar</span><span class="sxs-lookup"><span data-stu-id="6ea5e-288">AES-128 keys</span></span>|<span data-ttu-id="6ea5e-289">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-289">GA</span></span>|<span data-ttu-id="6ea5e-290">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-290">All</span></span>|
|<span data-ttu-id="6ea5e-291">Fairplay</span><span class="sxs-lookup"><span data-stu-id="6ea5e-291">Fairplay</span></span>|<span data-ttu-id="6ea5e-292">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-292">GA</span></span>|<span data-ttu-id="6ea5e-293">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-293">All</span></span>|
|<span data-ttu-id="6ea5e-294">PlayReady</span><span class="sxs-lookup"><span data-stu-id="6ea5e-294">PlayReady</span></span>|<span data-ttu-id="6ea5e-295">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-295">GA</span></span>|<span data-ttu-id="6ea5e-296">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-296">All</span></span>|
|<span data-ttu-id="6ea5e-297">Widevine</span><span class="sxs-lookup"><span data-stu-id="6ea5e-297">Widevine</span></span>|<span data-ttu-id="6ea5e-298">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-298">GA</span></span>|<span data-ttu-id="6ea5e-299">Alla utom Tyskland, federala myndigheter och Kina.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-299">All except Germany, Federal Government and China.</span></span>

### <a name="reserved-units-rus"></a><span data-ttu-id="6ea5e-300">Reserverade enheter (RU:er)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-300">Reserved units (RUs)</span></span>

<span data-ttu-id="6ea5e-301">Antalet etablerade reserverade enheter anger antalet medieuppgifter som kan bearbetas samtidigt i en viss konto.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-301">The number of provisioned reserved units determines the number of media tasks that can be processed concurrently in a given account.</span></span> 

<span data-ttu-id="6ea5e-302">Mer information finns i avsnittet om [skalning](#scaling).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-302">For more information, see the [scaling](#scaling) section.</span></span>

#### <a name="availability"></a><span data-ttu-id="6ea5e-303">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="6ea5e-303">Availability</span></span>

<span data-ttu-id="6ea5e-304">Tillgängligt i alla datacenter.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-304">Available in all datacenters.</span></span>

### <a name="reserved-unit-ru-type"></a><span data-ttu-id="6ea5e-305">Typ av reserverad enhet (RU)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-305">Reserved unit (RU) type</span></span>

<span data-ttu-id="6ea5e-306">Ett Media Services-konto är kopplat till en typ av reserverad enhet som bestämmer hur snabbt mediebearbetningsuppgifter ska bearbetas.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-306">A Media Services account is associated with a Reserved unit type, which determines the speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="6ea5e-307">Du kan välja mellan följande typer av reserverade enheter: S1, S2 och S3.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-307">You can pick between the following reserved unit types: S1, S2, or S3.</span></span>

<span data-ttu-id="6ea5e-308">Mer information finns i avsnittet om [skalning](#scaling).</span><span class="sxs-lookup"><span data-stu-id="6ea5e-308">For more information, see the [scaling](#scaling) section.</span></span>

#### <a name="availability"></a><span data-ttu-id="6ea5e-309">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="6ea5e-309">Availability</span></span>

|<span data-ttu-id="6ea5e-310">Namn på RU-typ</span><span class="sxs-lookup"><span data-stu-id="6ea5e-310">RU type name</span></span>|<span data-ttu-id="6ea5e-311">Status</span><span class="sxs-lookup"><span data-stu-id="6ea5e-311">Status</span></span>|<span data-ttu-id="6ea5e-312">Datacenter</span><span class="sxs-lookup"><span data-stu-id="6ea5e-312">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="6ea5e-313">S1</span><span class="sxs-lookup"><span data-stu-id="6ea5e-313">S1</span></span>|<span data-ttu-id="6ea5e-314">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-314">GA</span></span>|<span data-ttu-id="6ea5e-315">Alla</span><span class="sxs-lookup"><span data-stu-id="6ea5e-315">All</span></span>|
|<span data-ttu-id="6ea5e-316">S2</span><span class="sxs-lookup"><span data-stu-id="6ea5e-316">S2</span></span>|<span data-ttu-id="6ea5e-317">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-317">GA</span></span>|<span data-ttu-id="6ea5e-318">Alla utom södra Brasilien och västra Indien</span><span class="sxs-lookup"><span data-stu-id="6ea5e-318">All except Brazil South, and India West</span></span>|
|<span data-ttu-id="6ea5e-319">S3</span><span class="sxs-lookup"><span data-stu-id="6ea5e-319">S3</span></span>|<span data-ttu-id="6ea5e-320">Allmän tillgänglighet (GA)</span><span class="sxs-lookup"><span data-stu-id="6ea5e-320">GA</span></span>|<span data-ttu-id="6ea5e-321">Allt utom västra Indien</span><span class="sxs-lookup"><span data-stu-id="6ea5e-321">All except India West</span></span>|

## <a name="next-steps"></a><span data-ttu-id="6ea5e-322">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6ea5e-322">Next steps</span></span>

<span data-ttu-id="6ea5e-323">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="6ea5e-323">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6ea5e-324">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="6ea5e-324">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

