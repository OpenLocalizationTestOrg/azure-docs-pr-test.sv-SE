---
title: "aaa ”publicera innehåll med hello Azure-portalen | Microsoft Docs ”"
description: "Den här självstudiekursen vägleder dig genom stegen för hello publicera ditt innehåll med hello Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 92c364eb-5a5f-4f4e-8816-b162c031bb40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: a7a3867a6939b4b9da883176c6cc20c99d6c54e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-content-with-hello-azure-portal"></a><span data-ttu-id="71181-103">Publicera innehåll med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="71181-103">Publish content with hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="71181-104">Portal</span><span class="sxs-lookup"><span data-stu-id="71181-104">Portal</span></span>](media-services-portal-publish.md)
> * [<span data-ttu-id="71181-105">.NET</span><span class="sxs-lookup"><span data-stu-id="71181-105">.NET</span></span>](media-services-deliver-streaming-content.md)
> * [<span data-ttu-id="71181-106">REST</span><span class="sxs-lookup"><span data-stu-id="71181-106">REST</span></span>](media-services-rest-deliver-streaming-content.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="71181-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="71181-107">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="71181-108">toocomplete den här självstudiekursen kommer du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="71181-108">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="71181-109">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="71181-109">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="71181-110">tooprovide din användare en URL som kan använda toostream eller hämta ditt innehåll du först måste för ”publicera” din tillgång genom att skapa en positionerare.</span><span class="sxs-lookup"><span data-stu-id="71181-110">tooprovide your user with a  URL that can be used toostream or download your content, you first need too"publish" your asset by creating a locator.</span></span> <span data-ttu-id="71181-111">Positionerare ger åtkomst toofiles i hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="71181-111">Locators provide access toofiles contained in hello asset.</span></span> <span data-ttu-id="71181-112">Media Services stöder två typer av lokaliserare:</span><span class="sxs-lookup"><span data-stu-id="71181-112">Media Services supports two types of locators:</span></span> 

* <span data-ttu-id="71181-113">Streaming (OnDemandOrigin)-positionerare som används för anpassad strömning (till exempel toostream MPEG DASH, HLS eller Smooth Streaming).</span><span class="sxs-lookup"><span data-stu-id="71181-113">Streaming (OnDemandOrigin) locators, used for adaptive streaming (for example, toostream MPEG DASH, HLS, or Smooth Streaming).</span></span> <span data-ttu-id="71181-114">toocreate en strömningslokaliserare måste din tillgång innehålla en .ism-fil.</span><span class="sxs-lookup"><span data-stu-id="71181-114">toocreate a streaming locator your asset must contain an .ism file.</span></span> 
* <span data-ttu-id="71181-115">Progressiva SAS-lokaliserare, som används för leverans av video via progressiv hämtning.</span><span class="sxs-lookup"><span data-stu-id="71181-115">Progressive (SAS) locators, used for delivery of video via progressive download.</span></span>

<span data-ttu-id="71181-116">En strömmande URL har följande format hello och du kan använda den tooplay Smooth Streaming-tillgångar.</span><span class="sxs-lookup"><span data-stu-id="71181-116">A streaming URL has hello following format and you can use it tooplay Smooth Streaming assets.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

<span data-ttu-id="71181-117">Lägg till toobuild en HLS-strömnings-URL (format = m3u8 aapl) toohello URL.</span><span class="sxs-lookup"><span data-stu-id="71181-117">toobuild an HLS streaming URL, append (format=m3u8-aapl) toohello URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

<span data-ttu-id="71181-118">toobuild en MPEG DASH-strömnings-URL, Lägg till (format = mpd-tid-csf) toohello URL.</span><span class="sxs-lookup"><span data-stu-id="71181-118">toobuild an  MPEG DASH streaming URL, append (format=mpd-time-csf) toohello URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

<span data-ttu-id="71181-119">En SAS-URL har följande format hello.</span><span class="sxs-lookup"><span data-stu-id="71181-119">A SAS URL has hello following format.</span></span>

    {blob container name}/{asset name}/{file name}/{SAS signature}

<span data-ttu-id="71181-120">Mer information finns i [leverera Innehållsöversikt](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="71181-120">For more information, see [Delivering content overview](media-services-deliver-content-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="71181-121">Om du använde hello portal toocreate lokaliserare före mars 2015, skapades lokaliserare med ett utgångsdatum två år.</span><span class="sxs-lookup"><span data-stu-id="71181-121">If you used hello portal toocreate locators before March 2015, locators with a two year expiration date were created.</span></span>  
> 
> 

<span data-ttu-id="71181-122">tooupdate ett förfallodatum för en lokaliserare, Använd [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) eller [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API: er.</span><span class="sxs-lookup"><span data-stu-id="71181-122">tooupdate an expiration date on a locator, use [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) or [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) APIs.</span></span> <span data-ttu-id="71181-123">Observera att när du uppdaterar en SAS-lokaliserare hello utgångsdatum ändras hello-URL.</span><span class="sxs-lookup"><span data-stu-id="71181-123">Note that when you update hello expiration date of a SAS locator, hello URL changes.</span></span>

### <a name="toouse-hello-portal-toopublish-an-asset"></a><span data-ttu-id="71181-124">toouse hello portal toopublish en tillgång</span><span class="sxs-lookup"><span data-stu-id="71181-124">toouse hello portal toopublish an asset</span></span>
<span data-ttu-id="71181-125">toouse hello portal toopublish en tillgång, hello följande:</span><span class="sxs-lookup"><span data-stu-id="71181-125">toouse hello portal toopublish an asset, do hello following:</span></span>

1. <span data-ttu-id="71181-126">I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="71181-126">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="71181-127">Välj **Inställningar** > **Tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="71181-127">Select **Settings** > **Assets**.</span></span>
3. <span data-ttu-id="71181-128">Välj hello tillgång som du vill toopublish.</span><span class="sxs-lookup"><span data-stu-id="71181-128">Select hello asset that you want toopublish.</span></span>
4. <span data-ttu-id="71181-129">Klicka på hello **publicera** knappen.</span><span class="sxs-lookup"><span data-stu-id="71181-129">Click hello **Publish** button.</span></span>
5. <span data-ttu-id="71181-130">Välj typen av hello-positionerare.</span><span class="sxs-lookup"><span data-stu-id="71181-130">Select hello locator type.</span></span>
6. <span data-ttu-id="71181-131">Tryck på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="71181-131">Press **Add**.</span></span>
   
    ![Publicera](./media/media-services-portal-vod-get-started/media-services-publish1.png)

<span data-ttu-id="71181-133">hello URL läggs toohello lista över **publicerade URL: er**.</span><span class="sxs-lookup"><span data-stu-id="71181-133">hello URL will be added toohello list of **Published URLs**.</span></span>

## <a name="play-content-from-hello-portal"></a><span data-ttu-id="71181-134">Spela upp innehåll från hello-portalen</span><span class="sxs-lookup"><span data-stu-id="71181-134">Play content from hello portal</span></span>
<span data-ttu-id="71181-135">hello Azure-portalen har en innehållsspelare som du kan använda tootest videon.</span><span class="sxs-lookup"><span data-stu-id="71181-135">hello Azure portal provides a content player that you can use tootest your video.</span></span>

<span data-ttu-id="71181-136">Hello önskad video och klicka sedan på hello **spela upp** knappen.</span><span class="sxs-lookup"><span data-stu-id="71181-136">Click hello desired video and then click hello **Play** button.</span></span>

![Publicera](./media/media-services-portal-vod-get-started/media-services-play.png)

<span data-ttu-id="71181-138">Vissa förutsättningar gäller:</span><span class="sxs-lookup"><span data-stu-id="71181-138">Some considerations apply:</span></span>

* <span data-ttu-id="71181-139">Kontrollera att hello videon har publicerats.</span><span class="sxs-lookup"><span data-stu-id="71181-139">Make sure hello video has been published.</span></span>
* <span data-ttu-id="71181-140">Detta **Media player** spelar upp från hello standard strömmande slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="71181-140">This **Media player** plays from hello default streaming endpoint.</span></span> <span data-ttu-id="71181-141">Om du vill tooplay från ett standardvärde strömmande slutpunkten, klicka på toocopy hello URL och Använd en annan spelare.</span><span class="sxs-lookup"><span data-stu-id="71181-141">If you want tooplay from a non-default streaming endpoint, click toocopy hello URL and use another player.</span></span> <span data-ttu-id="71181-142">Till exempel [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="71181-142">For example, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>
* <span data-ttu-id="71181-143">Hej strömningsslutpunkt från vilken du strömning måste köras.</span><span class="sxs-lookup"><span data-stu-id="71181-143">hello streaming endpoint from which you are streaming must be running.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="71181-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="71181-144">Next steps</span></span>
<span data-ttu-id="71181-145">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="71181-145">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="71181-146">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="71181-146">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

