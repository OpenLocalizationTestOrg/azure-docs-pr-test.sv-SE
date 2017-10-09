---
title: "aaaScale strömningsslutpunkter med hello Azure-portalen | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom stegen för hello av skalning strömningsslutpunkter med hello Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 1008b3a3-2fa1-4146-85bd-2cf43cd1e00e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: e466edf9232558b9e270f54ee2849cd9b22ad121
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-streaming-endpoints-with-hello-azure-portal"></a><span data-ttu-id="24601-103">Skala strömningsslutpunkter med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="24601-103">Scale streaming endpoints with hello Azure portal</span></span>
## <a name="overview"></a><span data-ttu-id="24601-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="24601-104">Overview</span></span>

> [!NOTE]
> <span data-ttu-id="24601-105">toocomplete den här självstudiekursen kommer du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="24601-105">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="24601-106">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="24601-106">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="24601-107">**Premium**-slutpunkter för direktuppspelning passar för avancerade arbetsbelastningar och tillhandahåller dedikerad och skalbar bandbreddskapacitet.</span><span class="sxs-lookup"><span data-stu-id="24601-107">**Premium** streaming endpoints are suitable for advanced workloads, providing dedicated and scalable bandwidth capacity.</span></span> <span data-ttu-id="24601-108">Kunder som har en **Premium**-slutpunkt för direktuppspelning får som standard en direktuppspelande enhet (SU).</span><span class="sxs-lookup"><span data-stu-id="24601-108">Customers that have a **Premium** streaming endpoint, by default get one streaming unit (SU).</span></span> <span data-ttu-id="24601-109">hello strömmande slutpunkten kan skalas genom att lägga till SUs.</span><span class="sxs-lookup"><span data-stu-id="24601-109">hello streaming endpoint can be scaled by adding SUs.</span></span> <span data-ttu-id="24601-110">Varje SU ger ytterligare bandbredd kapacitet toohello program.</span><span class="sxs-lookup"><span data-stu-id="24601-110">Each SU provides additional bandwidth capacity toohello application.</span></span> <span data-ttu-id="24601-111">Mer information om typer av slutpunkter och CDN konfiguration finns hello [Strömningsslutpunkt översikt](media-services-portal-manage-streaming-endpoints.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="24601-111">For more information about streaming endpoint types and CDN configuration, see hello [Streaming Endpoint overview](media-services-portal-manage-streaming-endpoints.md) topic.</span></span>
 
<span data-ttu-id="24601-112">Det här avsnittet visar hur tooscale strömmande slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="24601-112">This topic shows how tooscale a streaming endpoint.</span></span>

<span data-ttu-id="24601-113">Mer information om priser finns i [Prisuppgifter för Media Services](http://go.microsoft.com/fwlink/?LinkId=275107).</span><span class="sxs-lookup"><span data-stu-id="24601-113">For information about pricing details, see [Media Services Pricing Details](http://go.microsoft.com/fwlink/?LinkId=275107).</span></span>

## <a name="scale-streaming-endpoints"></a><span data-ttu-id="24601-114">Skala slutpunkter för strömning</span><span class="sxs-lookup"><span data-stu-id="24601-114">Scale streaming endpoints</span></span>

<span data-ttu-id="24601-115">toochange hello antalet strömmande enheter hello följande:</span><span class="sxs-lookup"><span data-stu-id="24601-115">toochange hello number of streaming units, do hello following:</span></span>

1. <span data-ttu-id="24601-116">I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="24601-116">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="24601-117">I hello **inställningar** väljer **Strömningsslutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="24601-117">In hello **Settings** window, select **Streaming endpoints**.</span></span>
3. <span data-ttu-id="24601-118">Klicka på hello strömningsslutpunkt som du vill tooscale.</span><span class="sxs-lookup"><span data-stu-id="24601-118">Click on hello streaming endpoint that you want tooscale.</span></span> 

    [!NOTE] <span data-ttu-id="24601-119">Du kan skala **Premium** strömningsslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="24601-119">You can only scale **Premium** streaming endpoints.</span></span>

4. <span data-ttu-id="24601-120">Flytta hello skjutreglaget toospecify hello antalet strömningsenheter.</span><span class="sxs-lookup"><span data-stu-id="24601-120">Move hello slider toospecify hello number of streaming units.</span></span>

    ![Strömmande slutpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a><span data-ttu-id="24601-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="24601-122">Next steps</span></span>
<span data-ttu-id="24601-123">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="24601-123">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="24601-124">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="24601-124">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

