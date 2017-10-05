---
title: "Skala bearbetning genom att lägga till kodning enheter – Azure media |  Microsoft Docs"
description: "Lär dig hur du hur du lägger till kodning enheter med .NET"
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
ms.openlocfilehash: 72a8729d22a9e76c8076d7a3347619a2163e4f09
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-scale-encoding-with-net-sdk"></a><span data-ttu-id="09d39-103">Hur du skalar kodning med .NET SDK:n</span><span class="sxs-lookup"><span data-stu-id="09d39-103">How to scale encoding with .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="09d39-104">Portal</span><span class="sxs-lookup"><span data-stu-id="09d39-104">Portal</span></span>](media-services-portal-scale-media-processing.md)
> * [<span data-ttu-id="09d39-105">.NET</span><span class="sxs-lookup"><span data-stu-id="09d39-105">.NET</span></span>](media-services-dotnet-encoding-units.md)
> * [<span data-ttu-id="09d39-106">REST</span><span class="sxs-lookup"><span data-stu-id="09d39-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [<span data-ttu-id="09d39-107">Java</span><span class="sxs-lookup"><span data-stu-id="09d39-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="09d39-108">PHP</span><span class="sxs-lookup"><span data-stu-id="09d39-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="09d39-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="09d39-109">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="09d39-110">Se till att granska den [översikt](media-services-scale-media-processing-overview.md) avsnittet om du vill ha mer information om skalning media bearbetning av avsnittet.</span><span class="sxs-lookup"><span data-stu-id="09d39-110">Make sure to review the [overview](media-services-scale-media-processing-overview.md) topic to get more information about scaling media processing topic.</span></span>
> 
> 

<span data-ttu-id="09d39-111">Om du vill ändra typ av enhet och antalet kodningsreserverade enheter med hjälp av .NET SDK, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="09d39-111">To change the reserved unit type and the number of encoding reserved units using .NET SDK, do the following:</span></span>

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

## <a name="opening-a-support-ticket"></a><span data-ttu-id="09d39-112">Öppna ett supportärende</span><span class="sxs-lookup"><span data-stu-id="09d39-112">Opening a Support Ticket</span></span>
<span data-ttu-id="09d39-113">Som standard kan alla Media Services-konto skala upp till 25 kodning och 5 på begäran reserverade enheter för strömning.</span><span class="sxs-lookup"><span data-stu-id="09d39-113">By default every Media Services account can scale to up to 25 Encoding and 5 On-Demand Streaming Reserved Units.</span></span> <span data-ttu-id="09d39-114">Du kan begära en högre gräns genom att öppna ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="09d39-114">You can request a higher limit by opening a support ticket.</span></span>

### <a name="open-a-support-ticket"></a><span data-ttu-id="09d39-115">Öppna ett supportärende</span><span class="sxs-lookup"><span data-stu-id="09d39-115">Open a support ticket</span></span>
<span data-ttu-id="09d39-116">Om du vill öppna en supportbegäran biljetten gör du följande:</span><span class="sxs-lookup"><span data-stu-id="09d39-116">To open a support ticket do the following:</span></span>

1. <span data-ttu-id="09d39-117">Klicka på [få Support](https://manage.windowsazure.com/?getsupport=true).</span><span class="sxs-lookup"><span data-stu-id="09d39-117">Click [Get Support](https://manage.windowsazure.com/?getsupport=true).</span></span> <span data-ttu-id="09d39-118">Om du inte är inloggad uppmanas du att ange dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="09d39-118">If you are not logged in, you will be prompted to enter your credentials.</span></span>
2. <span data-ttu-id="09d39-119">Välj din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="09d39-119">Select your subscription.</span></span>
3. <span data-ttu-id="09d39-120">Välj ”Technical” under typ av stöd.</span><span class="sxs-lookup"><span data-stu-id="09d39-120">Under support type, select "Technical".</span></span>
4. <span data-ttu-id="09d39-121">Klicka på ”Skapa biljett”.</span><span class="sxs-lookup"><span data-stu-id="09d39-121">Click on "Create Ticket".</span></span>
5. <span data-ttu-id="09d39-122">Välj ”Azure Media Services” i produktlistan över visas på nästa sida.</span><span class="sxs-lookup"><span data-stu-id="09d39-122">Select "Azure Media Services" in the product list presented on the next page.</span></span>
6. <span data-ttu-id="09d39-123">Välj ”typ” som passar ditt problem.</span><span class="sxs-lookup"><span data-stu-id="09d39-123">Select a "Problem type" that is appropriate for your issue.</span></span>
7. <span data-ttu-id="09d39-124">Klicka på Fortsätt.</span><span class="sxs-lookup"><span data-stu-id="09d39-124">Click Continue.</span></span>
8. <span data-ttu-id="09d39-125">Följ instruktionerna på nästa sida och anger sedan information om problemet.</span><span class="sxs-lookup"><span data-stu-id="09d39-125">Follow instructions on next page and then enter details about your issue.</span></span>
9. <span data-ttu-id="09d39-126">Klicka på Skicka för att öppna biljetten.</span><span class="sxs-lookup"><span data-stu-id="09d39-126">Click submit to open the ticket.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="09d39-127">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="09d39-127">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="09d39-128">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="09d39-128">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

