---
title: "aaa hur tooget en Medieprocessor instans med hjälp av REST | Microsoft Docs"
description: "Lär dig hur toocreate media processor komponenten tooencode konvertera format, kryptera eller dekryptera medieinnehåll för Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: f9ff1997-0da6-4528-aaed-792837e5be41
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 9f423648ab73c90405c64895ce0f5b6a457862e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-a-media-processor-instance"></a><span data-ttu-id="3f16e-103">Hur tooget en Medieprocessor-instans</span><span class="sxs-lookup"><span data-stu-id="3f16e-103">How tooget a Media Processor instance</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3f16e-104">.NET</span><span class="sxs-lookup"><span data-stu-id="3f16e-104">.NET</span></span>](media-services-get-media-processor.md)
> * [<span data-ttu-id="3f16e-105">REST</span><span class="sxs-lookup"><span data-stu-id="3f16e-105">REST</span></span>](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="3f16e-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="3f16e-106">Overview</span></span>
<span data-ttu-id="3f16e-107">Formatera konvertering krypterar eller dekrypterar medieinnehåll i Media Services en medieprocessor är en komponent som hanterar en specifik bearbetning aktivitet, till exempel kodning.</span><span class="sxs-lookup"><span data-stu-id="3f16e-107">In Media Services a media processor is a component that handles a specific processing task, such as encoding, format conversion, encrypting, or decrypting media content.</span></span> <span data-ttu-id="3f16e-108">Du vanligtvis skapa en medieprocessor när du skapar en uppgift tooencode, kryptera eller konvertera hello-formatet för medieinnehåll.</span><span class="sxs-lookup"><span data-stu-id="3f16e-108">You typically create a media processor when you are creating a task tooencode, encrypt, or convert hello format of media content.</span></span>

## <a name="azure-media-processors"></a><span data-ttu-id="3f16e-109">Azure media-processorer</span><span class="sxs-lookup"><span data-stu-id="3f16e-109">Azure media processors</span></span> 

<span data-ttu-id="3f16e-110">följande ämne hello innehåller listor över media processorer:</span><span class="sxs-lookup"><span data-stu-id="3f16e-110">hello following topic provides lists of media processors:</span></span>

* [<span data-ttu-id="3f16e-111">Kodning media-processorer</span><span class="sxs-lookup"><span data-stu-id="3f16e-111">Encoding media processors</span></span>](scenarios-and-availability.md#encoding-media-processors)
* [<span data-ttu-id="3f16e-112">Analytics-medieprocessorer</span><span class="sxs-lookup"><span data-stu-id="3f16e-112">Analytics media processors</span></span>](scenarios-and-availability.md#analytics-media-processors)

>[!NOTE]
><span data-ttu-id="3f16e-113">Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="3f16e-113">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="3f16e-114">Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="3f16e-114">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-toomedia-services"></a><span data-ttu-id="3f16e-115">Ansluta tooMedia tjänster</span><span class="sxs-lookup"><span data-stu-id="3f16e-115">Connect tooMedia Services</span></span>

<span data-ttu-id="3f16e-116">Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="3f16e-116">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="3f16e-117">När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="3f16e-117">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="3f16e-118">Du måste göra följande anrop toohello ny URI.</span><span class="sxs-lookup"><span data-stu-id="3f16e-118">You must make subsequent calls toohello new URI.</span></span>

## <a name="get-a-media-processor"></a><span data-ttu-id="3f16e-119">Hämta en medieprocessor</span><span class="sxs-lookup"><span data-stu-id="3f16e-119">Get a media processor</span></span>

<span data-ttu-id="3f16e-120">hello följande REST-anrop som visar hur tooget en medieprocessor instansen efter namn (i det här fallet **Media Encoder Standard**).</span><span class="sxs-lookup"><span data-stu-id="3f16e-120">hello following REST call shows how tooget a media processor instance by name (in this case, **Media Encoder Standard**).</span></span> 

<span data-ttu-id="3f16e-121">Begäran:</span><span class="sxs-lookup"><span data-stu-id="3f16e-121">Request:</span></span>

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="3f16e-122">Svar:</span><span class="sxs-lookup"><span data-stu-id="3f16e-122">Response:</span></span>

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


## <a name="media-services-learning-paths"></a><span data-ttu-id="3f16e-123">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="3f16e-123">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3f16e-124">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="3f16e-124">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="3f16e-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3f16e-125">Next Steps</span></span>
<span data-ttu-id="3f16e-126">Nu när du vet hur tooget en media processor gå toohello [hur tooEncode en tillgång](media-services-rest-get-started.md) avsnittet som visar hur hello toouse Media Encoder Standard tooencode en tillgång.</span><span class="sxs-lookup"><span data-stu-id="3f16e-126">Now that you know how tooget a media processor instance, go toohello [How tooEncode an Asset](media-services-rest-get-started.md) topic which will show you how toouse hello Media Encoder Standard tooencode an asset.</span></span>

