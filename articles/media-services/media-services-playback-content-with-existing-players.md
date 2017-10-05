---
title: "Använd befintlig spelare att spela upp ditt innehåll - Azure | Microsoft Docs"
description: "Det här avsnittet innehåller befintliga spelare som du kan använda att spela upp ditt innehåll."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: juliako
ms.openlocfilehash: 48f373b013b1192c353352b801876d706d91dd28
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="playing-your-content-with-existing-players"></a><span data-ttu-id="7c31b-103">Spela upp ditt innehåll med befintliga spelare</span><span class="sxs-lookup"><span data-stu-id="7c31b-103">Playing your content with existing players</span></span>
<span data-ttu-id="7c31b-104">Azure Media Services stöder många populära strömmande format, till exempel Smooth Streaming HTTP Live Streaming och MPEG-Dash.</span><span class="sxs-lookup"><span data-stu-id="7c31b-104">Azure Media Services supports many popular streaming formats, such as Smooth Streaming, HTTP Live Streaming, and MPEG-Dash.</span></span> <span data-ttu-id="7c31b-105">Det här avsnittet hänvisar till befintliga spelare som du kan använda för att testa din dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="7c31b-105">This topic points you to existing players that you can use to test your streams.</span></span>

### <a name="the-azure-portal-media-services-content-player"></a><span data-ttu-id="7c31b-106">I Azure portal Media Services-innehållsspelaren</span><span class="sxs-lookup"><span data-stu-id="7c31b-106">The Azure portal Media Services content player</span></span>
<span data-ttu-id="7c31b-107">Den **Azure** portalen har en innehållsspelare som du kan använda för att testa videon.</span><span class="sxs-lookup"><span data-stu-id="7c31b-107">The **Azure** portal provides a content player that you can use to test your video.</span></span>

<span data-ttu-id="7c31b-108">Klicka på önskad video (se till att det var [publicerade](media-services-portal-publish.md)) och klicka på den **spela upp** längst ned i portalen.</span><span class="sxs-lookup"><span data-stu-id="7c31b-108">Click on the desired video (make sure it was [published](media-services-portal-publish.md)) and click the **Play** button at the bottom of the portal.</span></span>

<span data-ttu-id="7c31b-109">Vissa förutsättningar gäller:</span><span class="sxs-lookup"><span data-stu-id="7c31b-109">Some considerations apply:</span></span>

* <span data-ttu-id="7c31b-110">**MEDIA SERVICES-INNEHÅLLSSPELAREN** spelar upp från strömningsslutpunkten som är standard.</span><span class="sxs-lookup"><span data-stu-id="7c31b-110">The **MEDIA SERVICES CONTENT PLAYER** plays from the default streaming endpoint.</span></span> <span data-ttu-id="7c31b-111">Använd en annan spelare om du vill spela upp från en strömningsslutpunkt som inte är standard.</span><span class="sxs-lookup"><span data-stu-id="7c31b-111">If you want to play from a non-default streaming endpoint, use another player.</span></span> <span data-ttu-id="7c31b-112">Till exempel [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="7c31b-112">For example, [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a><span data-ttu-id="7c31b-114">Azure Media Player</span><span class="sxs-lookup"><span data-stu-id="7c31b-114">Azure Media Player</span></span>
<span data-ttu-id="7c31b-115">Använd [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) att spela upp ditt innehåll (rensa eller skyddade) i något av följande format:</span><span class="sxs-lookup"><span data-stu-id="7c31b-115">Use [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) to playback your content (clear or protected) in any of the following formats:</span></span>

* <span data-ttu-id="7c31b-116">Smooth Streaming</span><span class="sxs-lookup"><span data-stu-id="7c31b-116">Smooth Streaming</span></span>
* <span data-ttu-id="7c31b-117">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="7c31b-117">MPEG DASH</span></span>
* <span data-ttu-id="7c31b-118">HLS</span><span class="sxs-lookup"><span data-stu-id="7c31b-118">HLS</span></span>
* <span data-ttu-id="7c31b-119">Progressiv MP4</span><span class="sxs-lookup"><span data-stu-id="7c31b-119">Progressive MP4</span></span>

### <a name="flash-player"></a><span data-ttu-id="7c31b-120">Flash Player</span><span class="sxs-lookup"><span data-stu-id="7c31b-120">Flash Player</span></span>
#### <a name="aes-encrypted-with-token"></a><span data-ttu-id="7c31b-121">AES-kryptering med Token</span><span class="sxs-lookup"><span data-stu-id="7c31b-121">AES-encrypted with Token</span></span>
[<span data-ttu-id="7c31b-122">http://aestoken.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="7c31b-122">http://aestoken.azurewebsites.net</span></span>](http://aestoken.azurewebsites.net)

### <a name="silverlight-players"></a><span data-ttu-id="7c31b-123">Silverlight spelare</span><span class="sxs-lookup"><span data-stu-id="7c31b-123">Silverlight Players</span></span>
#### <a name="monitoring"></a><span data-ttu-id="7c31b-124">Övervakning</span><span class="sxs-lookup"><span data-stu-id="7c31b-124">Monitoring</span></span>
[<span data-ttu-id="7c31b-125">http://SMF.cloudapp.NET/HealthMonitor</span><span class="sxs-lookup"><span data-stu-id="7c31b-125">http://smf.cloudapp.net/healthmonitor</span></span>](http://smf.cloudapp.net/healthmonitor)

#### <a name="playready-with-token"></a><span data-ttu-id="7c31b-126">PlayReady med Token</span><span class="sxs-lookup"><span data-stu-id="7c31b-126">PlayReady with Token</span></span>
[<span data-ttu-id="7c31b-127">http://sltoken.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="7c31b-127">http://sltoken.azurewebsites.net</span></span>](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a><span data-ttu-id="7c31b-128">STRECK spelare</span><span class="sxs-lookup"><span data-stu-id="7c31b-128">DASH Players</span></span>
[<span data-ttu-id="7c31b-129">http://dashplayer.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="7c31b-129">http://dashplayer.azurewebsites.net</span></span>](http://dashplayer.azurewebsites.net)

[<span data-ttu-id="7c31b-130">http://dashif.org</span><span class="sxs-lookup"><span data-stu-id="7c31b-130">http://dashif.org</span></span>](http://dashif.org)

### <a name="other"></a><span data-ttu-id="7c31b-131">Annat</span><span class="sxs-lookup"><span data-stu-id="7c31b-131">Other</span></span>
<span data-ttu-id="7c31b-132">Så här testar HLS webbadresser som du kan också använda:</span><span class="sxs-lookup"><span data-stu-id="7c31b-132">To test HLS URLs you can also use:</span></span>

* <span data-ttu-id="7c31b-133">**Safari** på en iOS-enhet eller</span><span class="sxs-lookup"><span data-stu-id="7c31b-133">**Safari** on an iOS device or</span></span>
* <span data-ttu-id="7c31b-134">**3ivx HLS Player** i Windows.</span><span class="sxs-lookup"><span data-stu-id="7c31b-134">**3ivx HLS Player** on Windows.</span></span>

## <a name="developing-video-players"></a><span data-ttu-id="7c31b-135">Utveckla videospelare</span><span class="sxs-lookup"><span data-stu-id="7c31b-135">Developing video players</span></span>
<span data-ttu-id="7c31b-136">Information om hur du utvecklar dina egna spelare finns [utveckla videospelare](media-services-develop-video-players.md)</span><span class="sxs-lookup"><span data-stu-id="7c31b-136">For information about how to develop your own players, see [Developing video players](media-services-develop-video-players.md)</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="7c31b-137">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="7c31b-137">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7c31b-138">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="7c31b-138">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
