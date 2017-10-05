---
title: "Koda en tillgång med Media Encoder Standard med Azure-portalen | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom stegen för att koda en tillgång med Media Encoder Standard med Azure-portalen."
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
ms.openlocfilehash: a299245e285c4caa68988b184799cd6f4d13e080
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a><span data-ttu-id="43761-103">Koda en tillgång med Media Encoder Standard med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="43761-103">Encode an asset using Media Encoder Standard with the Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="43761-104">Du behöver ett Azure-konto för att slutföra den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="43761-104">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="43761-105">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="43761-105">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="43761-106">När du arbetar med Azure Media Services är ett av de vanligaste scenarierna att leverera strömning med anpassad bithastighet till dina klienter.</span><span class="sxs-lookup"><span data-stu-id="43761-106">When working with Azure Media Services one of the most common scenarios is delivering adaptive bitrate streaming to your clients.</span></span> <span data-ttu-id="43761-107">Media Services har stöd för följande strömningstekniker med anpassningsbar bithastighet: HTTP-liveuppspelning (HLS), jämn direktuppspelning, MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="43761-107">Media Services supports the following adaptive bitrate streaming technologies: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span> <span data-ttu-id="43761-108">För att förbereda dina videor för strömning med anpassad bithastighet måste du koda källvideon till filer i multibithastighet.</span><span class="sxs-lookup"><span data-stu-id="43761-108">To prepare your videos for adaptive bitrate streaming, you need to encode your source video into multi-bitrate files.</span></span> <span data-ttu-id="43761-109">Du bör använda kodaren **Media Encoder Standard** för att koda dina videor.</span><span class="sxs-lookup"><span data-stu-id="43761-109">You should use the **Media Encoder Standard** encoder to encode your videos.</span></span>  

<span data-ttu-id="43761-110">Media Services tillhandahåller också dynamisk paketering som gör att du kan leverera dina MP4s multibithastighet i följande strömningsformat: MPEG DASH, HLS, Smooth Streaming, utan att du behöver packa om till dessa strömningsformat.</span><span class="sxs-lookup"><span data-stu-id="43761-110">Media Services also provides dynamic packaging which allows you to deliver your multi-bitrate MP4s in the following streaming formats: MPEG DASH, HLS, Smooth Streaming, without you having to re-package into these streaming formats.</span></span> <span data-ttu-id="43761-111">Med dynamisk paketering behöver du bara lagra och betala för filerna i ett enda lagringsformat, och Media Services skapar och ger lämplig respons baserat på begäranden från en klient.</span><span class="sxs-lookup"><span data-stu-id="43761-111">With dynamic packaging you only need to store and pay for the files in single storage format and Media Services will build and serve the appropriate response based on requests from a client.</span></span>

<span data-ttu-id="43761-112">Om du vill dra nytta av dynamisk paketering måste du koda din källfil till en uppsättning MP4-filer med flera bithastigheter (kodningsstegen visas längre fram i det här avsnittet).</span><span class="sxs-lookup"><span data-stu-id="43761-112">To take advantage of dynamic packaging, you need to encode your source file into a set of multi-bitrate MP4 files (the encoding steps are demonstrated later in this section).</span></span>

<span data-ttu-id="43761-113">Om du vill skala media bearbetning, se [detta](media-services-portal-scale-media-processing.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="43761-113">To scale media processing, see [this](media-services-portal-scale-media-processing.md) topic.</span></span>

## <a name="encode-with-the-azure-portal"></a><span data-ttu-id="43761-114">Koda med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="43761-114">Encode with the Azure portal</span></span>
<span data-ttu-id="43761-115">I det här avsnittet beskrivs de steg som du kan vidta för att koda ditt innehåll med Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="43761-115">This section describes the steps you can take to encode your content with Media Encoder Standard.</span></span>

1. <span data-ttu-id="43761-116">Välj ditt Azure Media Services-konto i [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="43761-116">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="43761-117">I fönstret **Inställningar** väljer du **Tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="43761-117">In the **Settings** window, select **Assets**.</span></span>  
3. <span data-ttu-id="43761-118">I fönstret **Tillgångar** väljer du den tillgång som du vill koda.</span><span class="sxs-lookup"><span data-stu-id="43761-118">In the **Assets** window, select the asset that you would like to encode.</span></span>
4. <span data-ttu-id="43761-119">Tryck på knappen **Koda**.</span><span class="sxs-lookup"><span data-stu-id="43761-119">Press the **Encode** button.</span></span>
5. <span data-ttu-id="43761-120">I fönstret **Koda en tillgång** väljer du processorn ”Media Encoder Standard” och en förinställning.</span><span class="sxs-lookup"><span data-stu-id="43761-120">In the **Encode an asset** window, select the "Media Encoder Standard" processor and a preset.</span></span> <span data-ttu-id="43761-121">Information om förinställningar finns i [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) (autogenerera en bithastighetsstege) och [Task Presets for MES](media-services-mes-presets-overview.md) (Uppgiftsförinställningar för MES).</span><span class="sxs-lookup"><span data-stu-id="43761-121">For information about presets, see [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) and [Task Presets for MES](media-services-mes-presets-overview.md).</span></span> <span data-ttu-id="43761-122">Om du planerar att styra vilka kodningsförinställningar, kom ihåg att det är viktigt att välja den förinställning som är mest lämplig för din indatavideo.</span><span class="sxs-lookup"><span data-stu-id="43761-122">If you plan to control which encoding preset is used, keep this in mind: it is important to select the preset that is most appropriate for your input video.</span></span> <span data-ttu-id="43761-123">Om du till exempel vet att din indatavideo har en upplösning på 1 920 x 1 080 bildpunkter, kan du använda förinställningen ”H264 multibithastighet 1080p”.</span><span class="sxs-lookup"><span data-stu-id="43761-123">For example, if you know your input video has a resolution of 1920x1080 pixels, then you could use the "H264 Multiple Bitrate 1080p" preset.</span></span> <span data-ttu-id="43761-124">Om du har en video med låg upplösning (640 x 360) bör du inte använda förinställningen ”H264 multibithastighet 1080p”.</span><span class="sxs-lookup"><span data-stu-id="43761-124">If you have a low resolution (640x360) video, then you should not be using "H264 Multiple Bitrate 1080p" preset.</span></span>
   
   <span data-ttu-id="43761-125">Du kan redigera namnet på utdatatillgången och namnet på jobbet för enklare hantering.</span><span class="sxs-lookup"><span data-stu-id="43761-125">For easier management, you have an option of editing the name of the output asset, and the name of the job.</span></span>
   
   ![Koda tillgångar](./media/media-services-portal-vod-get-started/media-services-encode1.png)
6. <span data-ttu-id="43761-127">Tryck på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="43761-127">Press **Create**.</span></span>

## <a name="next-step"></a><span data-ttu-id="43761-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="43761-128">Next step</span></span>
<span data-ttu-id="43761-129">Du kan övervaka kodningsjobb med Azure-portalen, enligt beskrivningen i [detta](media-services-portal-check-job-progress.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="43761-129">You can monitor encoding job progress with the Azure portal, as described in [this](media-services-portal-check-job-progress.md) article.</span></span>  

## <a name="media-services-learning-paths"></a><span data-ttu-id="43761-130">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="43761-130">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="43761-131">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="43761-131">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

