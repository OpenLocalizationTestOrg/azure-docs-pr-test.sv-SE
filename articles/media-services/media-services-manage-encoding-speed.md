---
title: "aaa hantera hastighet och samtidighet på din encoding med Azure Media Services | Microsoft Docs"
description: "Den här artikeln ger en kort översikt över hur du kan hantera hastighet och samtidighet kodning jobb/aktiviteter med Azure Media Services."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 676313f8-a158-4e3a-a99b-2c29a341ecc9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: da52a6278a3d3b084dbf5a594db37df8447bb944
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#  <a name="manage-speed-and-concurrency-of-your-encoding"></a><span data-ttu-id="63c41-103">Hantera hastighet och samtidighet på din kodning</span><span class="sxs-lookup"><span data-stu-id="63c41-103">Manage speed and concurrency of your encoding</span></span>

<span data-ttu-id="63c41-104">Den här artikeln ger en kort översikt över hur du kan hantera hastighet och samtidighet kodning jobb/aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="63c41-104">This article gives a brief overview of how you can manage speed and concurrency of your encoding jobs/tasks.</span></span>

## <a name="overview"></a><span data-ttu-id="63c41-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="63c41-105">Overview</span></span>

<span data-ttu-id="63c41-106">I Media Services en **reserverade enhetstyp** anger hello hastighet som media bearbeta uppgifter bearbetas.</span><span class="sxs-lookup"><span data-stu-id="63c41-106">In Media Services, a **Reserved Unit Type** determines hello speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="63c41-107">Du kan välja mellan följande hello reserverade enhetstyper: **S1**, **S2**, eller **S3**.</span><span class="sxs-lookup"><span data-stu-id="63c41-107">You can pick between hello following reserved unit types: **S1**, **S2**, or **S3**.</span></span> <span data-ttu-id="63c41-108">Till exempel hello samma kodningsjobbet körs snabbare när du använder hello **S2** reserverade enhetstyp jämföra toohello **S1** typen.</span><span class="sxs-lookup"><span data-stu-id="63c41-108">For example, hello same encoding job runs faster when you use hello **S2** reserved unit type compare toohello **S1** type.</span></span> <span data-ttu-id="63c41-109">Hej [skalning kodning enheter](media-services-scale-media-processing-overview.md) avsnittet visas en tabell som hjälper dig att fatta beslut om att välja mellan olika kodning hastigheter.</span><span class="sxs-lookup"><span data-stu-id="63c41-109">hello [scaling encoding units](media-services-scale-media-processing-overview.md) topic shows a table that helps you make decision when choosing between different encoding speeds.</span></span>

<span data-ttu-id="63c41-110">Dessutom toospecifying hello reserverade enhetstyp, kan du ange tooprovision till ditt konto med **reserverade enheter**.</span><span class="sxs-lookup"><span data-stu-id="63c41-110">In addition toospecifying hello reserved unit type, you can specify tooprovision your account with **Reserved Units**.</span></span> <span data-ttu-id="63c41-111">hello antalet etablerade reserverade enheter avgör hello antal media uppgifter som kan bearbetas samtidigt i en viss konto.</span><span class="sxs-lookup"><span data-stu-id="63c41-111">hello number of provisioned reserved units determines hello number of media tasks that can be processed concurrently in a given account.</span></span> <span data-ttu-id="63c41-112">Till exempel om ditt konto har fem reserverade enheter fem media aktiviteter körs samtidigt så länge som det finns aktiviteter toobe bearbetas.</span><span class="sxs-lookup"><span data-stu-id="63c41-112">For example, if your account has five reserved units, then five media tasks will be running concurrently as long as there are tasks toobe processed.</span></span> <span data-ttu-id="63c41-113">hello återstående aktiviteter kommer att vänta i kön hello och ska hämta tas upp för bearbetning i tur och ordning när en aktivitet är klar.</span><span class="sxs-lookup"><span data-stu-id="63c41-113">hello remaining tasks will wait in hello queue and will get picked up for processing sequentially when a running task finishes.</span></span> <span data-ttu-id="63c41-114">Om ett konto inte har några reserverade enheter som har etablerats sedan hanteras uppgifter sekventiellt.</span><span class="sxs-lookup"><span data-stu-id="63c41-114">If an account does not have any reserved units provisioned, then tasks will be picked up sequentially.</span></span> <span data-ttu-id="63c41-115">I det här fallet hello väntetid mellan en aktivitet slutförs och hello nästa startar beror på hello tillgängligheten för resurser i hello system.</span><span class="sxs-lookup"><span data-stu-id="63c41-115">In this case, hello wait time between one task finishing and hello next one starting will depend on hello availability of resources in hello system.</span></span>

<span data-ttu-id="63c41-116">Mer detaljerad information och exempel som visar hur tooscale kodning enheter, se [detta](media-services-scale-media-processing-overview.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="63c41-116">For detailed information and examples that show how tooscale encoding units, see [this](media-services-scale-media-processing-overview.md) topic.</span></span>

## <a name="next-step"></a><span data-ttu-id="63c41-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="63c41-117">Next step</span></span>

[<span data-ttu-id="63c41-118">Kodning skalningsenheter</span><span class="sxs-lookup"><span data-stu-id="63c41-118">Scale encoding units</span></span>](media-services-scale-media-processing-overview.md)

## <a name="media-services-learning-paths"></a><span data-ttu-id="63c41-119">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="63c41-119">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="63c41-120">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="63c41-120">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

