---
title: "aaaHow tooCreate ett media-processor med hello Azure Media Services SDK för .NET | Microsoft Docs"
description: "Lär dig hur toocreate media processor komponenten tooencode konvertera format, kryptera eller dekryptera medieinnehåll för Azure Media Services. Kodexemplen är skrivna i C# och använder hello Media Services SDK för .NET."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: dbf9496f-c6f0-42a7-aa36-70f89dcb8ea2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: f133565cc1321d366013f17302adc8bc7585b251
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-get-a-media-processor-instance"></a><span data-ttu-id="4cc86-104">Så här: hämta en Media-processor</span><span class="sxs-lookup"><span data-stu-id="4cc86-104">How to: Get a Media Processor Instance</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4cc86-105">.NET</span><span class="sxs-lookup"><span data-stu-id="4cc86-105">.NET</span></span>](media-services-get-media-processor.md)
> * [<span data-ttu-id="4cc86-106">REST</span><span class="sxs-lookup"><span data-stu-id="4cc86-106">REST</span></span>](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="4cc86-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="4cc86-107">Overview</span></span>
<span data-ttu-id="4cc86-108">Formatera konvertering krypterar eller dekrypterar medieinnehåll i Media Services en medieprocessor är en komponent som hanterar en specifik bearbetning aktivitet, till exempel kodning.</span><span class="sxs-lookup"><span data-stu-id="4cc86-108">In Media Services a media processor is a component that handles a specific processing task, such as encoding, format conversion, encrypting, or decrypting media content.</span></span> <span data-ttu-id="4cc86-109">Du vanligtvis skapa en medieprocessor när du skapar en uppgift tooencode, kryptera eller konvertera hello-formatet för medieinnehåll.</span><span class="sxs-lookup"><span data-stu-id="4cc86-109">You typically create a media processor when you are creating a task tooencode, encrypt, or convert hello format of media content.</span></span>

## <a name="azure-media-processors"></a><span data-ttu-id="4cc86-110">Azure media-processorer</span><span class="sxs-lookup"><span data-stu-id="4cc86-110">Azure media processors</span></span> 

<span data-ttu-id="4cc86-111">följande ämne hello innehåller listor över media processorer:</span><span class="sxs-lookup"><span data-stu-id="4cc86-111">hello following topic provides lists of media processors:</span></span>

* [<span data-ttu-id="4cc86-112">Kodning media-processorer</span><span class="sxs-lookup"><span data-stu-id="4cc86-112">Encoding media processors</span></span>](scenarios-and-availability.md#encoding-media-processors)
* [<span data-ttu-id="4cc86-113">Analytics-medieprocessorer</span><span class="sxs-lookup"><span data-stu-id="4cc86-113">Analytics media processors</span></span>](scenarios-and-availability.md#analytics-media-processors)

## <a name="get-media-processor"></a><span data-ttu-id="4cc86-114">Hämta Medieprocessor</span><span class="sxs-lookup"><span data-stu-id="4cc86-114">Get Media Processor</span></span>

<span data-ttu-id="4cc86-115">Hej följa metoden visar hur tooget en media-processor.</span><span class="sxs-lookup"><span data-stu-id="4cc86-115">hello following method shows how tooget a media processor instance.</span></span> <span data-ttu-id="4cc86-116">hello kodexemplet förutsätter vi hello användning av en modul-nivå variabel med namnet **_context** tooreference hello-serverkontext enligt beskrivningen i avsnittet hello [så här: ansluta programmässigt Services tooMedia](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="4cc86-116">hello code example assumes hello use of a module-level variable named **_context** tooreference hello server context as described in hello section [How to: Connect tooMedia Services Programmatically](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


## <a name="media-services-learning-paths"></a><span data-ttu-id="4cc86-117">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="4cc86-117">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="4cc86-118">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="4cc86-118">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="4cc86-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4cc86-119">Next Steps</span></span>
<span data-ttu-id="4cc86-120">Nu när du vet hur tooget en media processor gå toohello [hur tooEncode en tillgång](media-services-dotnet-encode-with-media-encoder-standard.md) avsnittet som visar hur hello toouse Media Encoder Standard tooencode en tillgång.</span><span class="sxs-lookup"><span data-stu-id="4cc86-120">Now that you know how tooget a media processor instance, go toohello [How tooEncode an Asset](media-services-dotnet-encode-with-media-encoder-standard.md) topic which will show you how toouse hello Media Encoder Standard tooencode an asset.</span></span>

