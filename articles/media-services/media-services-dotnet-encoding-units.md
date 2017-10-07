---
title: "aaaScale media bearbetning genom att lägga till kodning enheter – Azure |  Microsoft Docs"
description: "Lär dig hur toohow tooadd kodning enheter med .NET"
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 33f7625a-966a-4f06-bc09-bccd6e2a42b5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;milangada;
ms.openlocfilehash: b9f71a6487c5d136319a38a1598d60edfaa81b9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-encoding-with-net-sdk"></a><span data-ttu-id="62105-103">Hur tooscale encoding med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="62105-103">How tooscale encoding with .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="62105-104">Portal</span><span class="sxs-lookup"><span data-stu-id="62105-104">Portal</span></span>](media-services-portal-scale-media-processing.md)
> * [<span data-ttu-id="62105-105">.NET</span><span class="sxs-lookup"><span data-stu-id="62105-105">.NET</span></span>](media-services-dotnet-encoding-units.md)
> * [<span data-ttu-id="62105-106">REST</span><span class="sxs-lookup"><span data-stu-id="62105-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [<span data-ttu-id="62105-107">Java</span><span class="sxs-lookup"><span data-stu-id="62105-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="62105-108">PHP</span><span class="sxs-lookup"><span data-stu-id="62105-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="62105-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="62105-109">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="62105-110">Se till att tooreview hello [översikt](media-services-scale-media-processing-overview.md) avsnittet tooget mer information om att skala media bearbetning av avsnittet.</span><span class="sxs-lookup"><span data-stu-id="62105-110">Make sure tooreview hello [overview](media-services-scale-media-processing-overview.md) topic tooget more information about scaling media processing topic.</span></span>
> 
> 

<span data-ttu-id="62105-111">toochange hello reserverad enhet typ och hello antalet kodningsreserverade enheter med hjälp av .NET SDK hello följande:</span><span class="sxs-lookup"><span data-stu-id="62105-111">toochange hello reserved unit type and hello number of encoding reserved units using .NET SDK, do hello following:</span></span>

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds tooS1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

## <a name="opening-a-support-ticket"></a><span data-ttu-id="62105-112">Öppna ett supportärende</span><span class="sxs-lookup"><span data-stu-id="62105-112">Opening a Support Ticket</span></span>
<span data-ttu-id="62105-113">Som standard var Media Services-konto kan skala tooup too25 kodning och 5 på begäran reserverade enheter för strömning.</span><span class="sxs-lookup"><span data-stu-id="62105-113">By default every Media Services account can scale tooup too25 Encoding and 5 On-Demand Streaming Reserved Units.</span></span> <span data-ttu-id="62105-114">Du kan begära en högre gräns genom att öppna ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="62105-114">You can request a higher limit by opening a support ticket.</span></span>

### <a name="open-a-support-ticket"></a><span data-ttu-id="62105-115">Öppna ett supportärende</span><span class="sxs-lookup"><span data-stu-id="62105-115">Open a support ticket</span></span>
<span data-ttu-id="62105-116">tooopen ett supportärende hello följande:</span><span class="sxs-lookup"><span data-stu-id="62105-116">tooopen a support ticket do hello following:</span></span>

1. <span data-ttu-id="62105-117">Klicka på [få Support](https://manage.windowsazure.com/?getsupport=true).</span><span class="sxs-lookup"><span data-stu-id="62105-117">Click [Get Support](https://manage.windowsazure.com/?getsupport=true).</span></span> <span data-ttu-id="62105-118">Om du inte är inloggad som du kommer att tillfrågas tooenter dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="62105-118">If you are not logged in, you will be prompted tooenter your credentials.</span></span>
2. <span data-ttu-id="62105-119">Välj din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="62105-119">Select your subscription.</span></span>
3. <span data-ttu-id="62105-120">Välj ”Technical” under typ av stöd.</span><span class="sxs-lookup"><span data-stu-id="62105-120">Under support type, select "Technical".</span></span>
4. <span data-ttu-id="62105-121">Klicka på ”Skapa biljett”.</span><span class="sxs-lookup"><span data-stu-id="62105-121">Click on "Create Ticket".</span></span>
5. <span data-ttu-id="62105-122">Välj ”Azure Media Services” i hello produktlista visas på hello nästa sida.</span><span class="sxs-lookup"><span data-stu-id="62105-122">Select "Azure Media Services" in hello product list presented on hello next page.</span></span>
6. <span data-ttu-id="62105-123">Välj ”typ” som passar ditt problem.</span><span class="sxs-lookup"><span data-stu-id="62105-123">Select a "Problem type" that is appropriate for your issue.</span></span>
7. <span data-ttu-id="62105-124">Klicka på Fortsätt.</span><span class="sxs-lookup"><span data-stu-id="62105-124">Click Continue.</span></span>
8. <span data-ttu-id="62105-125">Följ instruktionerna på nästa sida och anger sedan information om problemet.</span><span class="sxs-lookup"><span data-stu-id="62105-125">Follow instructions on next page and then enter details about your issue.</span></span>
9. <span data-ttu-id="62105-126">Klicka på Skicka tooopen hello biljett.</span><span class="sxs-lookup"><span data-stu-id="62105-126">Click submit tooopen hello ticket.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="62105-127">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="62105-127">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="62105-128">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="62105-128">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

