---
title: aaaScale media bearbetning med hello Azure-portalen | Microsoft Docs
description: "Den här självstudiekursen vägleder dig genom stegen för hello skalning Media bearbetning med hello Azure-portalen."
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
ms.openlocfilehash: 89240c6f7579b8795e7b47f2b1c398b1d5477e20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-reserved-unit-type"></a><span data-ttu-id="4cd08-103">Ändra hello reserverade enhetstyp</span><span class="sxs-lookup"><span data-stu-id="4cd08-103">Change hello reserved unit type</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4cd08-104">.NET</span><span class="sxs-lookup"><span data-stu-id="4cd08-104">.NET</span></span>](media-services-dotnet-encoding-units.md)
> * [<span data-ttu-id="4cd08-105">Portal</span><span class="sxs-lookup"><span data-stu-id="4cd08-105">Portal</span></span>](media-services-portal-scale-media-processing.md)
> * [<span data-ttu-id="4cd08-106">REST</span><span class="sxs-lookup"><span data-stu-id="4cd08-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [<span data-ttu-id="4cd08-107">Java</span><span class="sxs-lookup"><span data-stu-id="4cd08-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="4cd08-108">PHP</span><span class="sxs-lookup"><span data-stu-id="4cd08-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="4cd08-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="4cd08-109">Overview</span></span>

<span data-ttu-id="4cd08-110">Ett Media Services-konto är kopplat till ett reserverat enhetstyp som bestämmer hello hastighet som media bearbeta uppgifter bearbetas.</span><span class="sxs-lookup"><span data-stu-id="4cd08-110">A Media Services account is associated with a Reserved Unit Type, which determines hello speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="4cd08-111">Du kan välja mellan följande hello reserverade enhetstyper: **S1**, **S2**, eller **S3**.</span><span class="sxs-lookup"><span data-stu-id="4cd08-111">You can pick between hello following reserved unit types: **S1**, **S2**, or **S3**.</span></span> <span data-ttu-id="4cd08-112">Till exempel hello samma kodningsjobbet körs snabbare när du använder hello **S2** reserverade enhetstyp jämföra toohello **S1** typen.</span><span class="sxs-lookup"><span data-stu-id="4cd08-112">For example, hello same encoding job runs faster when you use hello **S2** reserved unit type compare toohello **S1** type.</span></span>

<span data-ttu-id="4cd08-113">Dessutom toospecifying hello reserverade enhetstyp, kan du ange tooprovision till ditt konto med **reserverade enheter** (RUs).</span><span class="sxs-lookup"><span data-stu-id="4cd08-113">In addition toospecifying hello reserved unit type, you can specify tooprovision your account with **Reserved Units** (RUs).</span></span> <span data-ttu-id="4cd08-114">hello antalet etablerade RUs bestämmer hello antal media uppgifter som kan bearbetas samtidigt i en viss konto.</span><span class="sxs-lookup"><span data-stu-id="4cd08-114">hello number of provisioned RUs determines hello number of media tasks that can be processed concurrently in a given account.</span></span>

>[!NOTE]
><span data-ttu-id="4cd08-115">RU:er fungerar för parallellisera all bearbetning av media, inklusive indexeringsjobb med hjälp av Azure Media Indexer.</span><span class="sxs-lookup"><span data-stu-id="4cd08-115">RUs work for parallelizing all media processing, including indexing jobs using Azure Media Indexer.</span></span> <span data-ttu-id="4cd08-116">Men till skillnad från kodning bearbetas inte indexeringsjobb snabbare med snabbare reserverade enheter.</span><span class="sxs-lookup"><span data-stu-id="4cd08-116">However, unlike encoding, indexing jobs do not get processed faster with faster reserved units.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4cd08-117">Se till att tooreview hello [översikt](media-services-scale-media-processing-overview.md) avsnittet tooget mer information om att skala media bearbetning av avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4cd08-117">Make sure tooreview hello [overview](media-services-scale-media-processing-overview.md) topic tooget more information about scaling media processing topic.</span></span>
> 
> 

## <a name="scale-media-processing"></a><span data-ttu-id="4cd08-118">Skala mediebearbetning</span><span class="sxs-lookup"><span data-stu-id="4cd08-118">Scale media processing</span></span>
<span data-ttu-id="4cd08-119">toochange hello reserverad enhet typ och hello antalet reserverade enheter hello följande:</span><span class="sxs-lookup"><span data-stu-id="4cd08-119">toochange hello reserved unit type and hello number of reserved units, do hello following:</span></span>

1. <span data-ttu-id="4cd08-120">I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="4cd08-120">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="4cd08-121">I hello **inställningar** väljer **mediereserverade enheter**.</span><span class="sxs-lookup"><span data-stu-id="4cd08-121">In hello **Settings** window, select **Media reserved units**.</span></span>
   
    <span data-ttu-id="4cd08-122">toochange hello antalet reserverade enheter för hello valt typ av enhet använder hello **Media hanteras enheter** skjutreglaget.</span><span class="sxs-lookup"><span data-stu-id="4cd08-122">toochange hello number of reserved units for hello selected reserved unit type, use hello **Media Served Units** slider.</span></span>
   
    <span data-ttu-id="4cd08-123">toochange hello **RESERVERADE ENHETSTYP**, tryck på S1, S2 och S3.</span><span class="sxs-lookup"><span data-stu-id="4cd08-123">toochange hello **RESERVED UNIT TYPE**, press S1, S2, or S3.</span></span>
   
    ![Sidan processorer](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. <span data-ttu-id="4cd08-125">Tryck på hello spara knappen toosave ändringarna.</span><span class="sxs-lookup"><span data-stu-id="4cd08-125">Press hello SAVE button toosave your changes.</span></span>
   
    <span data-ttu-id="4cd08-126">hello nya reserverade enheter tilldelas när du trycker på Spara.</span><span class="sxs-lookup"><span data-stu-id="4cd08-126">hello new reserved units are allocated when you press SAVE.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4cd08-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4cd08-127">Next steps</span></span>
<span data-ttu-id="4cd08-128">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="4cd08-128">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="4cd08-129">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="4cd08-129">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

