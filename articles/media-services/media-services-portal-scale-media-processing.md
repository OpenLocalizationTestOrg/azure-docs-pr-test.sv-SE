---
title: "Skala media bearbetning med hjälp av Azure portal | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom stegen för skalning media bearbetning med Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: 46ca29d3e66701f2abcb185791089e94761984e8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="change-the-reserved-unit-type"></a><span data-ttu-id="2f32d-103">Ändra den reserverade enhetstypen</span><span class="sxs-lookup"><span data-stu-id="2f32d-103">Change the reserved unit type</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2f32d-104">.NET</span><span class="sxs-lookup"><span data-stu-id="2f32d-104">.NET</span></span>](media-services-dotnet-encoding-units.md)
> * [<span data-ttu-id="2f32d-105">Portal</span><span class="sxs-lookup"><span data-stu-id="2f32d-105">Portal</span></span>](media-services-portal-scale-media-processing.md)
> * [<span data-ttu-id="2f32d-106">REST</span><span class="sxs-lookup"><span data-stu-id="2f32d-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [<span data-ttu-id="2f32d-107">Java</span><span class="sxs-lookup"><span data-stu-id="2f32d-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="2f32d-108">PHP</span><span class="sxs-lookup"><span data-stu-id="2f32d-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="2f32d-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="2f32d-109">Overview</span></span>

<span data-ttu-id="2f32d-110">Ett Media Services-konto är kopplat till en typ av reserverad enhet som bestämmer hur snabbt mediebearbetningsuppgifter ska bearbetas.</span><span class="sxs-lookup"><span data-stu-id="2f32d-110">A Media Services account is associated with a Reserved Unit Type, which determines the speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="2f32d-111">Du kan välja mellan följande typer av reserverade enheter: **S1**, **S2** och **S3**.</span><span class="sxs-lookup"><span data-stu-id="2f32d-111">You can pick between the following reserved unit types: **S1**, **S2**, or **S3**.</span></span> <span data-ttu-id="2f32d-112">Samma kodningsjobb körs till exempel snabbare om du använder typen **S2** än om du använder typen **S1**.</span><span class="sxs-lookup"><span data-stu-id="2f32d-112">For example, the same encoding job runs faster when you use the **S2** reserved unit type compare to the **S1** type.</span></span>

<span data-ttu-id="2f32d-113">Förutom att ange typ av reserverad enhet kan du etablera **reserverade enheter** (RU:er) för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="2f32d-113">In addition to specifying the reserved unit type, you can specify to provision your account with **Reserved Units** (RUs).</span></span> <span data-ttu-id="2f32d-114">Antalet etablerade RU:er anger antalet medieuppgifter som kan bearbetas samtidigt i en viss konto.</span><span class="sxs-lookup"><span data-stu-id="2f32d-114">The number of provisioned RUs determines the number of media tasks that can be processed concurrently in a given account.</span></span>

>[!NOTE]
><span data-ttu-id="2f32d-115">RU:er fungerar för parallellisera all bearbetning av media, inklusive indexeringsjobb med hjälp av Azure Media Indexer.</span><span class="sxs-lookup"><span data-stu-id="2f32d-115">RUs work for parallelizing all media processing, including indexing jobs using Azure Media Indexer.</span></span> <span data-ttu-id="2f32d-116">Men till skillnad från kodning bearbetas inte indexeringsjobb snabbare med snabbare reserverade enheter.</span><span class="sxs-lookup"><span data-stu-id="2f32d-116">However, unlike encoding, indexing jobs do not get processed faster with faster reserved units.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2f32d-117">Se till att granska den [översikt](media-services-scale-media-processing-overview.md) avsnittet om du vill ha mer information om skalning media bearbetning av avsnittet.</span><span class="sxs-lookup"><span data-stu-id="2f32d-117">Make sure to review the [overview](media-services-scale-media-processing-overview.md) topic to get more information about scaling media processing topic.</span></span>
> 
> 

## <a name="scale-media-processing"></a><span data-ttu-id="2f32d-118">Skala mediebearbetning</span><span class="sxs-lookup"><span data-stu-id="2f32d-118">Scale media processing</span></span>
<span data-ttu-id="2f32d-119">Om du vill ändra vilken enhet och antalet reserverade enheter måste du göra följande:</span><span class="sxs-lookup"><span data-stu-id="2f32d-119">To change the reserved unit type and the number of reserved units, do the following:</span></span>

1. <span data-ttu-id="2f32d-120">Välj ditt Azure Media Services-konto i [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2f32d-120">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="2f32d-121">I den **inställningar** väljer **mediereserverade enheter**.</span><span class="sxs-lookup"><span data-stu-id="2f32d-121">In the **Settings** window, select **Media reserved units**.</span></span>
   
    <span data-ttu-id="2f32d-122">Du kan ändra antalet reserverade enheter för den valda reserverade enhetstypen den **Media hanteras enheter** skjutreglaget.</span><span class="sxs-lookup"><span data-stu-id="2f32d-122">To change the number of reserved units for the selected reserved unit type, use the **Media Served Units** slider.</span></span>
   
    <span data-ttu-id="2f32d-123">Så här ändrar du den **RESERVERADE ENHETSTYP**, tryck på S1, S2 och S3.</span><span class="sxs-lookup"><span data-stu-id="2f32d-123">To change the **RESERVED UNIT TYPE**, press S1, S2, or S3.</span></span>
   
    ![Sidan processorer](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. <span data-ttu-id="2f32d-125">Tryck på knappen SPARA för att spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="2f32d-125">Press the SAVE button to save your changes.</span></span>
   
    <span data-ttu-id="2f32d-126">Nya reserverade enheter tilldelas när du trycker på Spara.</span><span class="sxs-lookup"><span data-stu-id="2f32d-126">The new reserved units are allocated when you press SAVE.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f32d-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2f32d-127">Next steps</span></span>
<span data-ttu-id="2f32d-128">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="2f32d-128">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="2f32d-129">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="2f32d-129">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

