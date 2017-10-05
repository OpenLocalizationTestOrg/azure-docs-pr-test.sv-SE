---
title: "Vanliga frågor och svar om Azure Media Services | Microsoft Docs"
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
ms.openlocfilehash: 48f3924d44a084d61c1d38002cd5098094001acb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="frequently-asked-questions"></a><span data-ttu-id="c27da-103">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="c27da-103">Frequently asked questions</span></span>

<span data-ttu-id="c27da-104">Den här artikeln tar upp vanliga frågor och svar som skapats av användare som Azure Media Services (AMS).</span><span class="sxs-lookup"><span data-stu-id="c27da-104">This article addresses frequently asked questions raised by the Azure Media Services (AMS) user community.</span></span>

## <a name="general-ams-faqs"></a><span data-ttu-id="c27da-105">Allmänna AMS vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="c27da-105">General AMS FAQs</span></span>
<span data-ttu-id="c27da-106">F: hur du skala indexering?</span><span class="sxs-lookup"><span data-stu-id="c27da-106">Q: How do you scale indexing?</span></span>

<span data-ttu-id="c27da-107">S: reserverade enheter är desamma för kodning och indexering uppgifter.</span><span class="sxs-lookup"><span data-stu-id="c27da-107">A: The reserved units are the same for Encoding and Indexing tasks.</span></span> <span data-ttu-id="c27da-108">Följ instruktionerna på [så skala Encoding-reserverade enheter](media-services-scale-media-processing-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c27da-108">Follow instructions on [How to Scale Encoding Reserved Units](media-services-scale-media-processing-overview.md).</span></span> <span data-ttu-id="c27da-109">**Obs** indexeraren prestanda inte påverkas av reserverade enhetstyp.</span><span class="sxs-lookup"><span data-stu-id="c27da-109">**Note** that Indexer performance is not affected by Reserved Unit Type.</span></span>

<span data-ttu-id="c27da-110">F: Jag har överförts, kodat och publicerade en video.</span><span class="sxs-lookup"><span data-stu-id="c27da-110">Q: I uploaded, encoded, and published a video.</span></span> <span data-ttu-id="c27da-111">Vad är orsaken videon inte upp när jag försöker att strömma den?</span><span class="sxs-lookup"><span data-stu-id="c27da-111">What would be the reason the video does not play when I try to stream it?</span></span>

<span data-ttu-id="c27da-112">S: en av de vanligaste orsakerna är att du inte har den strömningsslutpunkt från vilken du vill spela upp i den **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="c27da-112">A: One of the most common reasons is you do not have the streaming endpoint from which you are trying to playback in the **Running** state.</span></span>  

<span data-ttu-id="c27da-113">F: kan jag göra sammansättning på en direktsänd dataström?</span><span class="sxs-lookup"><span data-stu-id="c27da-113">Q: Can I do compositing on a live stream?</span></span>

<span data-ttu-id="c27da-114">S: sammansättning på live dataströmmar finns för närvarande inte i Azure Media Services så måste du skapa före på datorn.</span><span class="sxs-lookup"><span data-stu-id="c27da-114">A: Compositing on live streams is currently not offered in Azure Media Services, so you would need to pre-compose on your computer.</span></span>

<span data-ttu-id="c27da-115">F: kan jag använda Azure CDN med Liveströmning?</span><span class="sxs-lookup"><span data-stu-id="c27da-115">Q: Can I use Azure CDN with Live Streaming?</span></span>

<span data-ttu-id="c27da-116">S: Media Services stöder integration med Azure CDN (Mer information finns i [hur du hanterar Strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md)).</span><span class="sxs-lookup"><span data-stu-id="c27da-116">A: Media Services supports integration with Azure CDN (for more information, see [How to Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)).</span></span>  <span data-ttu-id="c27da-117">Du kan använda direktsänd strömning med CDN.</span><span class="sxs-lookup"><span data-stu-id="c27da-117">You can use Live streaming with CDN.</span></span> <span data-ttu-id="c27da-118">Azure Media Services tillhandahåller Smooth Streaming, HLS och MPEG-DASH-utdata.</span><span class="sxs-lookup"><span data-stu-id="c27da-118">Azure Media Services provides Smooth Streaming, HLS and MPEG-DASH outputs.</span></span> <span data-ttu-id="c27da-119">Alla dessa format använder HTTP för att överföra data och få fördelarna med HTTP-cachelagring.</span><span class="sxs-lookup"><span data-stu-id="c27da-119">All these formats use HTTP for transferring data and get benefits of HTTP caching.</span></span> <span data-ttu-id="c27da-120">I direktsänd strömning faktiska ljud och data delas till fragment och den här enskilda fragment hämta cachelagras i CDN.</span><span class="sxs-lookup"><span data-stu-id="c27da-120">In live streaming actual video/audio data is divided to fragments and this individual fragments get cached in CDN.</span></span> <span data-ttu-id="c27da-121">Endast data måste uppdateras är manifestet data.</span><span class="sxs-lookup"><span data-stu-id="c27da-121">Only data needs to be refreshed is the manifest data.</span></span> <span data-ttu-id="c27da-122">CDN uppdaterar regelbundet manifestet data.</span><span class="sxs-lookup"><span data-stu-id="c27da-122">CDN periodically refreshes manifest data.</span></span>

<span data-ttu-id="c27da-123">F: kan Azure Media services stöder lagra avbildningar?</span><span class="sxs-lookup"><span data-stu-id="c27da-123">Q: Does Azure Media services support storing images?</span></span>

<span data-ttu-id="c27da-124">S: Om du bara vill lagra JPEG eller PNG-avbildningar, bör du behålla dem i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="c27da-124">A: If you are just looking to store JPEG or PNG images, you should keep those in Azure Blob Storage.</span></span> <span data-ttu-id="c27da-125">Det finns ingen fördel med att placera dem i ditt Media Services-konto om du inte vill hålla dem som är associerade med din Video eller ljud tillgångar.</span><span class="sxs-lookup"><span data-stu-id="c27da-125">There is no benefit to putting them in your Media Services account unless you want to keep them associated with your Video or Audio Assets.</span></span> <span data-ttu-id="c27da-126">Eller om du kanske behöver använda avbildningar som överlägg i video kodaren. Media Encoder Standard stöder överlägg bilder ovanpå videor och som är vad listas JPEG och PNG som stöds indata format.</span><span class="sxs-lookup"><span data-stu-id="c27da-126">Or if you might have a need to use the images as overlays in the video encoder.Media Encoder Standard supports overlaying images on top of videos, and that is what it lists JPEG and PNG as supported input formats.</span></span> <span data-ttu-id="c27da-127">Mer information finns i [skapar överlägg](media-services-advanced-encoding-with-mes.md#overlay).</span><span class="sxs-lookup"><span data-stu-id="c27da-127">For more information, see [Creating Overlays](media-services-advanced-encoding-with-mes.md#overlay).</span></span>

<span data-ttu-id="c27da-128">F: hur kan jag kopiera resurser från ett Media Services-konto till en annan.</span><span class="sxs-lookup"><span data-stu-id="c27da-128">Q: How can I copy assets from one Media Services account to another.</span></span>

<span data-ttu-id="c27da-129">S: att kopiera resurser från ett Media Services-konto till en annan med .NET kan använda [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) tilläggsmetoden som är tillgängliga i den [Azure Media Services .NET SDK-tilläggen](https://github.com/Azure/azure-sdk-for-media-services-extensions/) databasen.</span><span class="sxs-lookup"><span data-stu-id="c27da-129">A: To copy assets from one Media Services account to another using .NET, use [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) extension method available in the [Azure Media Services .NET SDK Extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions/) repository.</span></span> <span data-ttu-id="c27da-130">Mer information finns i [detta](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) forum tråd.</span><span class="sxs-lookup"><span data-stu-id="c27da-130">For more information, see [this](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) forum thread.</span></span>

<span data-ttu-id="c27da-131">F: Vad är tecken som stöds för namngivning av filer när du arbetar med AMS?</span><span class="sxs-lookup"><span data-stu-id="c27da-131">Q: What are the supported characters for naming files when working with AMS?</span></span>

<span data-ttu-id="c27da-132">S: Media Services använder värdet för egenskapen IAssetFile.Name när du skapar URL: er för strömning innehållet (till exempel http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Därför tillåts procent-encoding inte.</span><span class="sxs-lookup"><span data-stu-id="c27da-132">A: Media Services uses the value of the IAssetFile.Name property when building URLs for the streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="c27da-133">Värdet för den **namn** egenskapen får inte ha något av följande [procent-encoding-reserverade tecken](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? [] # % ”.</span><span class="sxs-lookup"><span data-stu-id="c27da-133">The value of the **Name** property cannot have any of the following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="c27da-134">Dessutom det kan bara finnas ett '.'</span><span class="sxs-lookup"><span data-stu-id="c27da-134">Also, there can only be one ‘.’</span></span> <span data-ttu-id="c27da-135">för filnamnstillägget.</span><span class="sxs-lookup"><span data-stu-id="c27da-135">for the file name extension.</span></span>

<span data-ttu-id="c27da-136">F: hur du ansluter med hjälp av REST?</span><span class="sxs-lookup"><span data-stu-id="c27da-136">Q: How to connect using REST?</span></span>

<span data-ttu-id="c27da-137">S: information om hur du ansluter till AMS API finns [åtkomst till Azure Media Services-API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="c27da-137">A: For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> <span data-ttu-id="c27da-138">När du har anslutit till https://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="c27da-138">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="c27da-139">Du måste göra följande anrop till en ny URI.</span><span class="sxs-lookup"><span data-stu-id="c27da-139">You must make subsequent calls to the new URI.</span></span> 

<span data-ttu-id="c27da-140">F: hur kan rotera en video under kodningen.</span><span class="sxs-lookup"><span data-stu-id="c27da-140">Q: How can I rotate a video during the encoding process.</span></span>

<span data-ttu-id="c27da-141">A: det [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) stöder rotation av vinklar 270-90/180.</span><span class="sxs-lookup"><span data-stu-id="c27da-141">A: The [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) supports rotation by angles of 90/180/270.</span></span> <span data-ttu-id="c27da-142">Standardinställningen är ”automatisk” där försöker identifiera rotation metadata i filen inkommande MP4/MOV och kompensera för den.</span><span class="sxs-lookup"><span data-stu-id="c27da-142">The default behavior is "Auto", where it tries to detect the rotation metadata in the incoming MP4/MOV file and compensate for it.</span></span> <span data-ttu-id="c27da-143">Inkludera följande **källor** element till en json-förinställningar definierats [här](media-services-mes-presets-overview.md):</span><span class="sxs-lookup"><span data-stu-id="c27da-143">Include the following **Sources** element to one of the json presets defined [here](media-services-mes-presets-overview.md):</span></span>

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


## <a name="media-services-learning-paths"></a><span data-ttu-id="c27da-144">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="c27da-144">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c27da-145">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="c27da-145">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
