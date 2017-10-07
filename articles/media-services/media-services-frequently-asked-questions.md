---
title: "aaaAzure Media Services vanliga frågor och svar | Microsoft Docs"
description: "Vanliga frågor och svar (FAQ)"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5374f7f4-c189-43ef-8b7f-f2f4141e2748
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 6d48a5c1291f3c2559d8445921d571718d0a0a6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions"></a><span data-ttu-id="2d4cd-103">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="2d4cd-103">Frequently asked questions</span></span>

<span data-ttu-id="2d4cd-104">Den här artikeln tar vanliga frågor och svar som skapats av hello Azure Media Services (AMS)-användargruppen.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-104">This article addresses frequently asked questions raised by hello Azure Media Services (AMS) user community.</span></span>

## <a name="general-ams-faqs"></a><span data-ttu-id="2d4cd-105">Allmänna AMS vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="2d4cd-105">General AMS FAQs</span></span>
<span data-ttu-id="2d4cd-106">F: hur du skala indexering?</span><span class="sxs-lookup"><span data-stu-id="2d4cd-106">Q: How do you scale indexing?</span></span>

<span data-ttu-id="2d4cd-107">S: hello reserverade enheter är hello samma för kodning och indexering uppgifter.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-107">A: hello reserved units are hello same for Encoding and Indexing tasks.</span></span> <span data-ttu-id="2d4cd-108">Följ instruktionerna på [hur tooScale Encoding-reserverade enheter](media-services-scale-media-processing-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2d4cd-108">Follow instructions on [How tooScale Encoding Reserved Units](media-services-scale-media-processing-overview.md).</span></span> <span data-ttu-id="2d4cd-109">**Obs** indexeraren prestanda inte påverkas av reserverade enhetstyp.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-109">**Note** that Indexer performance is not affected by Reserved Unit Type.</span></span>

<span data-ttu-id="2d4cd-110">F: Jag har överförts, kodat och publicerade en video.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-110">Q: I uploaded, encoded, and published a video.</span></span> <span data-ttu-id="2d4cd-111">Vad skulle vara hello orsak hello video agerar inte när jag försöker toostream den?</span><span class="sxs-lookup"><span data-stu-id="2d4cd-111">What would be hello reason hello video does not play when I try toostream it?</span></span>

<span data-ttu-id="2d4cd-112">S: ett av vanligaste orsakerna är att du inte har hello strömningsslutpunkt från vilken du försöker tooplayback i hello hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-112">A: One of hello most common reasons is you do not have hello streaming endpoint from which you are trying tooplayback in hello **Running** state.</span></span>  

<span data-ttu-id="2d4cd-113">F: kan jag göra sammansättning på en direktsänd dataström?</span><span class="sxs-lookup"><span data-stu-id="2d4cd-113">Q: Can I do compositing on a live stream?</span></span>

<span data-ttu-id="2d4cd-114">S: sammansättning på live dataströmmar för närvarande inte finns i Azure Media Services måste du toopre-utgöra på datorn.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-114">A: Compositing on live streams is currently not offered in Azure Media Services, so you would need toopre-compose on your computer.</span></span>

<span data-ttu-id="2d4cd-115">F: kan jag använda Azure CDN med Liveströmning?</span><span class="sxs-lookup"><span data-stu-id="2d4cd-115">Q: Can I use Azure CDN with Live Streaming?</span></span>

<span data-ttu-id="2d4cd-116">S: Media Services stöder integration med Azure CDN (Mer information finns i [hur tooManage Strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md)).</span><span class="sxs-lookup"><span data-stu-id="2d4cd-116">A: Media Services supports integration with Azure CDN (for more information, see [How tooManage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)).</span></span>  <span data-ttu-id="2d4cd-117">Du kan använda direktsänd strömning med CDN.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-117">You can use Live streaming with CDN.</span></span> <span data-ttu-id="2d4cd-118">Azure Media Services tillhandahåller Smooth Streaming, HLS och MPEG-DASH-utdata.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-118">Azure Media Services provides Smooth Streaming, HLS and MPEG-DASH outputs.</span></span> <span data-ttu-id="2d4cd-119">Alla dessa format använder HTTP för att överföra data och få fördelarna med HTTP-cachelagring.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-119">All these formats use HTTP for transferring data and get benefits of HTTP caching.</span></span> <span data-ttu-id="2d4cd-120">Direktsänd strömning faktiska ljud och data är indelat toofragments och den här enskilda fragment hämta cachelagras i CDN.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-120">In live streaming actual video/audio data is divided toofragments and this individual fragments get cached in CDN.</span></span> <span data-ttu-id="2d4cd-121">Endast data måste toobe uppdateras är hello manifestet data.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-121">Only data needs toobe refreshed is hello manifest data.</span></span> <span data-ttu-id="2d4cd-122">CDN uppdaterar regelbundet manifestet data.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-122">CDN periodically refreshes manifest data.</span></span>

<span data-ttu-id="2d4cd-123">F: kan Azure Media services stöder lagra avbildningar?</span><span class="sxs-lookup"><span data-stu-id="2d4cd-123">Q: Does Azure Media services support storing images?</span></span>

<span data-ttu-id="2d4cd-124">S: Om du bara behöver toostore JPEG eller PNG-bilder, bör du behålla dem i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-124">A: If you are just looking toostore JPEG or PNG images, you should keep those in Azure Blob Storage.</span></span> <span data-ttu-id="2d4cd-125">Det finns ingen fördel tooputting dem i Media Services-konto om du inte vill tookeep dem som är associerade med din Video eller ljud tillgångar.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-125">There is no benefit tooputting them in your Media Services account unless you want tookeep them associated with your Video or Audio Assets.</span></span> <span data-ttu-id="2d4cd-126">Eller om du kan ha en behovet toouse hello bilder som överlägg i hello-videokodare. Media Encoder Standard stöder överlägg bilder ovanpå videor och som är vad listas JPEG och PNG som stöds indata format.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-126">Or if you might have a need toouse hello images as overlays in hello video encoder.Media Encoder Standard supports overlaying images on top of videos, and that is what it lists JPEG and PNG as supported input formats.</span></span> <span data-ttu-id="2d4cd-127">Mer information finns i [skapar överlägg](media-services-advanced-encoding-with-mes.md#overlay).</span><span class="sxs-lookup"><span data-stu-id="2d4cd-127">For more information, see [Creating Overlays](media-services-advanced-encoding-with-mes.md#overlay).</span></span>

<span data-ttu-id="2d4cd-128">F: hur kan kopiera resurser från en tooanother för Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-128">Q: How can I copy assets from one Media Services account tooanother.</span></span>

<span data-ttu-id="2d4cd-129">S: toocopy tillgångar från en tooanother för Media Services-konto med hjälp av .NET, Använd [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) tilläggsmetoden som är tillgängliga i hello [Azure Media Services .NET SDK-tilläggen](https://github.com/Azure/azure-sdk-for-media-services-extensions/) databasen.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-129">A: toocopy assets from one Media Services account tooanother using .NET, use [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) extension method available in hello [Azure Media Services .NET SDK Extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions/) repository.</span></span> <span data-ttu-id="2d4cd-130">Mer information finns i [detta](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) forum tråd.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-130">For more information, see [this](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) forum thread.</span></span>

<span data-ttu-id="2d4cd-131">F: vad hello stöds tecken för namngivning av filer när du arbetar med AMS?</span><span class="sxs-lookup"><span data-stu-id="2d4cd-131">Q: What are hello supported characters for naming files when working with AMS?</span></span>

<span data-ttu-id="2d4cd-132">S: Media Services använder hello värdet hello IAssetFile.Name egenskapen när du skapar URL: er för hello direktuppspelning av innehåll (till exempel http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Därför tillåts procent-encoding inte.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-132">A: Media Services uses hello value of hello IAssetFile.Name property when building URLs for hello streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="2d4cd-133">Hej värdet för hello **namn** egenskapen får inte ha någon av följande hello [procent-encoding-reserverade tecken](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? [] # % ”.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-133">hello value of hello **Name** property cannot have any of hello following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="2d4cd-134">Dessutom det kan bara finnas ett '.'</span><span class="sxs-lookup"><span data-stu-id="2d4cd-134">Also, there can only be one ‘.’</span></span> <span data-ttu-id="2d4cd-135">för hello filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-135">for hello file name extension.</span></span>

<span data-ttu-id="2d4cd-136">F: hur tooconnect med hjälp av REST?</span><span class="sxs-lookup"><span data-stu-id="2d4cd-136">Q: How tooconnect using REST?</span></span>

<span data-ttu-id="2d4cd-137">S: för information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="2d4cd-137">A: For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> <span data-ttu-id="2d4cd-138">När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-138">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="2d4cd-139">Du måste göra följande anrop toohello ny URI.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-139">You must make subsequent calls toohello new URI.</span></span> 

<span data-ttu-id="2d4cd-140">F: hur kan rotera en video under hello kodning processen.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-140">Q: How can I rotate a video during hello encoding process.</span></span>

<span data-ttu-id="2d4cd-141">S: hello [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) stöder rotation av vinklar 270-90/180.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-141">A: hello [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) supports rotation by angles of 90/180/270.</span></span> <span data-ttu-id="2d4cd-142">hello standardbeteendet är ”automatisk”, där den försöker toodetect hello rotation metadata i hello inkommande MP4/MOV-fil och kompensera för den.</span><span class="sxs-lookup"><span data-stu-id="2d4cd-142">hello default behavior is "Auto", where it tries toodetect hello rotation metadata in hello incoming MP4/MOV file and compensate for it.</span></span> <span data-ttu-id="2d4cd-143">Inkludera hello följande **källor** elementet tooone hello json förinställningar som definierats [här](media-services-mes-presets-overview.md):</span><span class="sxs-lookup"><span data-stu-id="2d4cd-143">Include hello following **Sources** element tooone of hello json presets defined [here](media-services-mes-presets-overview.md):</span></span>

    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...


## <a name="media-services-learning-paths"></a><span data-ttu-id="2d4cd-144">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="2d4cd-144">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="2d4cd-145">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="2d4cd-145">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
