---
title: "aaaEncode en tillgång med Media Encoder Standard med hello Azure-portalen | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom hello stegen för att koda en tillgång med Media Encoder Standard med hello Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 107d9e9a-71e9-43e5-b17c-6e00983aceab
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 0d118bbbe1fa9f4ba0bfa3ea3b10fb541d1d6379
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encode-an-asset-using-media-encoder-standard-with-hello-azure-portal"></a><span data-ttu-id="28e39-103">Koda en tillgång med Media Encoder Standard med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="28e39-103">Encode an asset using Media Encoder Standard with hello Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="28e39-104">toocomplete den här självstudiekursen kommer du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="28e39-104">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="28e39-105">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="28e39-105">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="28e39-106">När du arbetar med Azure Media Services är ett av hello vanligaste scenarierna att leverera med anpassningsbar bithastighet strömmande tooyour klienter.</span><span class="sxs-lookup"><span data-stu-id="28e39-106">When working with Azure Media Services one of hello most common scenarios is delivering adaptive bitrate streaming tooyour clients.</span></span> <span data-ttu-id="28e39-107">Media Services stöder följande strömningstekniker med anpassningsbar bithastighet hello: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="28e39-107">Media Services supports hello following adaptive bitrate streaming technologies: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span> <span data-ttu-id="28e39-108">tooprepare videor för anpassningsbar bithastighet strömning måste tooencode källfilerna video i flera bithastigheter filer.</span><span class="sxs-lookup"><span data-stu-id="28e39-108">tooprepare your videos for adaptive bitrate streaming, you need tooencode your source video into multi-bitrate files.</span></span> <span data-ttu-id="28e39-109">Du bör använda hello **Media Encoder Standard** kodare tooencode videor.</span><span class="sxs-lookup"><span data-stu-id="28e39-109">You should use hello **Media Encoder Standard** encoder tooencode your videos.</span></span>  

<span data-ttu-id="28e39-110">Media Services tillhandahåller också dynamisk paketering som gör toodeliver din multibithastighet MP4s i hello följande strömningsformat: MPEG DASH, HLS, Smooth Streaming, utan att behöva toore-paket till dessa strömningsformat.</span><span class="sxs-lookup"><span data-stu-id="28e39-110">Media Services also provides dynamic packaging which allows you toodeliver your multi-bitrate MP4s in hello following streaming formats: MPEG DASH, HLS, Smooth Streaming, without you having toore-package into these streaming formats.</span></span> <span data-ttu-id="28e39-111">Med dynamisk paketering behöver du bara toostore och betala för hello filer i ett enda lagringsformat och Media Services skapar och ger hello lämplig respons baserat på begäranden från en klient.</span><span class="sxs-lookup"><span data-stu-id="28e39-111">With dynamic packaging you only need toostore and pay for hello files in single storage format and Media Services will build and serve hello appropriate response based on requests from a client.</span></span>

<span data-ttu-id="28e39-112">tootake nytta av dynamisk paketering behöver du behöver tooencode källfilen till en uppsättning med flera bithastigheter MP4-filer (hello kodningsstegen senare i det här avsnittet).</span><span class="sxs-lookup"><span data-stu-id="28e39-112">tootake advantage of dynamic packaging, you need tooencode your source file into a set of multi-bitrate MP4 files (hello encoding steps are demonstrated later in this section).</span></span>

<span data-ttu-id="28e39-113">tooscale media bearbetning, se [detta](media-services-portal-scale-media-processing.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="28e39-113">tooscale media processing, see [this](media-services-portal-scale-media-processing.md) topic.</span></span>

## <a name="encode-with-hello-azure-portal"></a><span data-ttu-id="28e39-114">Koda med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="28e39-114">Encode with hello Azure portal</span></span>
<span data-ttu-id="28e39-115">Det här avsnittet beskrivs hello åtgärder du kan vidta tooencode ditt innehåll med Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="28e39-115">This section describes hello steps you can take tooencode your content with Media Encoder Standard.</span></span>

1. <span data-ttu-id="28e39-116">I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="28e39-116">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="28e39-117">I hello **inställningar** väljer **tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="28e39-117">In hello **Settings** window, select **Assets**.</span></span>  
3. <span data-ttu-id="28e39-118">I hello **tillgångar** fönster, Välj hello tillgång som du vill att tooencode.</span><span class="sxs-lookup"><span data-stu-id="28e39-118">In hello **Assets** window, select hello asset that you would like tooencode.</span></span>
4. <span data-ttu-id="28e39-119">Tryck på hello **koda** knappen.</span><span class="sxs-lookup"><span data-stu-id="28e39-119">Press hello **Encode** button.</span></span>
5. <span data-ttu-id="28e39-120">I hello **koda en tillgång** fönster, Välj hello ”Media Encoder Standard” processor och en förinställning.</span><span class="sxs-lookup"><span data-stu-id="28e39-120">In hello **Encode an asset** window, select hello "Media Encoder Standard" processor and a preset.</span></span> <span data-ttu-id="28e39-121">Information om förinställningar finns i [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) (autogenerera en bithastighetsstege) och [Task Presets for MES](media-services-mes-presets-overview.md) (Uppgiftsförinställningar för MES).</span><span class="sxs-lookup"><span data-stu-id="28e39-121">For information about presets, see [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) and [Task Presets for MES](media-services-mes-presets-overview.md).</span></span> <span data-ttu-id="28e39-122">Om du planerar toocontrol vilka kodning förinställda används, Kom ihåg: det är viktigt tooselect hello förinställning som är mest lämpliga för din indatavideo.</span><span class="sxs-lookup"><span data-stu-id="28e39-122">If you plan toocontrol which encoding preset is used, keep this in mind: it is important tooselect hello preset that is most appropriate for your input video.</span></span> <span data-ttu-id="28e39-123">Till exempel om du vet att din indatavideo har en upplösning på 1 920 x 1 080 bildpunkter, du kan använda hello ”H264 Multibithastighet 1080p” förinställda.</span><span class="sxs-lookup"><span data-stu-id="28e39-123">For example, if you know your input video has a resolution of 1920x1080 pixels, then you could use hello "H264 Multiple Bitrate 1080p" preset.</span></span> <span data-ttu-id="28e39-124">Om du har en video med låg upplösning (640 x 360) bör du inte använda förinställningen ”H264 multibithastighet 1080p”.</span><span class="sxs-lookup"><span data-stu-id="28e39-124">If you have a low resolution (640x360) video, then you should not be using "H264 Multiple Bitrate 1080p" preset.</span></span>
   
   <span data-ttu-id="28e39-125">Har en möjlighet att redigera hello av hello utdatatillgången och hello namn för hello jobbet för enklare hantering.</span><span class="sxs-lookup"><span data-stu-id="28e39-125">For easier management, you have an option of editing hello name of hello output asset, and hello name of hello job.</span></span>
   
   ![Koda tillgångar](./media/media-services-portal-vod-get-started/media-services-encode1.png)
6. <span data-ttu-id="28e39-127">Tryck på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="28e39-127">Press **Create**.</span></span>

## <a name="next-step"></a><span data-ttu-id="28e39-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="28e39-128">Next step</span></span>
<span data-ttu-id="28e39-129">Du kan övervaka kodningsjobb med hello Azure-portalen, enligt beskrivningen i [detta](media-services-portal-check-job-progress.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="28e39-129">You can monitor encoding job progress with hello Azure portal, as described in [this](media-services-portal-check-job-progress.md) article.</span></span>  

## <a name="media-services-learning-paths"></a><span data-ttu-id="28e39-130">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="28e39-130">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="28e39-131">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="28e39-131">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

