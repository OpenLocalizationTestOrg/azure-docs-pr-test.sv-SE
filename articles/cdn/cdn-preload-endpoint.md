---
title: "aaaPre belastningen tillgångar på en Azure CDN-slutpunkt | Microsoft Docs"
description: "Lär dig hur toopre belastningen cachelagrat innehåll på en Azure CDN-slutpunkt."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 08ac4b834f1ac8ce59d22e65fa8adea11bafcf17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a><span data-ttu-id="505be-103">Läsa in tillgångar för en Azure CDN-slutpunkt i förväg</span><span class="sxs-lookup"><span data-stu-id="505be-103">Pre-load assets on an Azure CDN endpoint</span></span>
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

<span data-ttu-id="505be-104">Som standard cachelagras först tillgångar när de begärs.</span><span class="sxs-lookup"><span data-stu-id="505be-104">By default, assets are first cached as they are requested.</span></span> <span data-ttu-id="505be-105">Detta innebär att hello första begäran från varje region kan ta längre tid eftersom hello edge-servrar inte har hello innehåll cachelagras och behöver tooforward hello begäran toohello ursprungsservern.</span><span class="sxs-lookup"><span data-stu-id="505be-105">This means that hello first request from each region may take longer, since hello edge servers will not have hello content cached and will need tooforward hello request toohello origin server.</span></span> <span data-ttu-id="505be-106">Den här första träffar latens undviks förinläsning innehåll.</span><span class="sxs-lookup"><span data-stu-id="505be-106">Pre-loading content avoids this first hit latency.</span></span>

<span data-ttu-id="505be-107">Dessutom kan tooproviding en bättre kundupplevelse förinläsning dina cachelagrade tillgångar också minska nätverkstrafiken på hello ursprungsservern.</span><span class="sxs-lookup"><span data-stu-id="505be-107">In addition tooproviding a better customer experience, pre-loading your cached assets can also reduce network traffic on hello origin server.</span></span>

> [!NOTE]
> <span data-ttu-id="505be-108">Förinläsning tillgångar är användbart för stora händelser eller innehåll som blir tillgängliga samtidigt tooa stort antal användare, till exempel en ny film version eller en programuppdatering.</span><span class="sxs-lookup"><span data-stu-id="505be-108">Pre-loading assets is useful for  large events or content that becomes simultaneously available tooa large number of users, such as a new movie release or a software update.</span></span>
> 
> 

<span data-ttu-id="505be-109">Den här självstudiekursen vägleder dig genom förinläsning cachelagrat innehåll på alla noder i Azure CDN-kant.</span><span class="sxs-lookup"><span data-stu-id="505be-109">This tutorial walks you through pre-loading cached content on all Azure CDN edge nodes.</span></span>

## <a name="walkthrough"></a><span data-ttu-id="505be-110">Genomgång</span><span class="sxs-lookup"><span data-stu-id="505be-110">Walkthrough</span></span>
1. <span data-ttu-id="505be-111">I hello [Azure Portal](https://portal.azure.com), bläddra toohello CDN-profil som innehåller hello-slutpunkt som du vill toopre belastningen.</span><span class="sxs-lookup"><span data-stu-id="505be-111">In hello [Azure Portal](https://portal.azure.com), browse toohello CDN profile containing hello endpoint you wish toopre-load.</span></span>  <span data-ttu-id="505be-112">hello profil blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="505be-112">hello profile blade opens.</span></span>
2. <span data-ttu-id="505be-113">Klicka på hello slutpunkt i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="505be-113">Click hello endpoint in hello list.</span></span>  <span data-ttu-id="505be-114">hello endpoint blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="505be-114">hello endpoint blade opens.</span></span>
3. <span data-ttu-id="505be-115">Klicka på hello Läs in hello CDN-slutpunkten bladet.</span><span class="sxs-lookup"><span data-stu-id="505be-115">From hello CDN endpoint blade, click hello load button.</span></span>
   
    ![CDN-slutpunkten bladet](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    <span data-ttu-id="505be-117">hello belastningen blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="505be-117">hello Load blade opens.</span></span>
   
    ![CDN belastningen bladet](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. <span data-ttu-id="505be-119">Ange hello fullständig sökväg för varje resurs som du vill att tooload (t.ex. `/pictures/kitten.png`) i hello **sökväg** textruta.</span><span class="sxs-lookup"><span data-stu-id="505be-119">Enter hello full path of each asset you wish tooload (e.g., `/pictures/kitten.png`) in hello **Path** textbox.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="505be-120">Flera **sökväg** textrutor visas när du har angett texten tooallow toobuild en lista över flera resurser.</span><span class="sxs-lookup"><span data-stu-id="505be-120">More **Path** textboxes will appear after you enter text tooallow you toobuild a list of multiple assets.</span></span>  <span data-ttu-id="505be-121">Du kan ta bort tillgångar hello listan genom att klicka på ellipsknappen för hello (...).</span><span class="sxs-lookup"><span data-stu-id="505be-121">You can delete assets from hello list by clicking hello ellipsis (...) button.</span></span>
   > 
   > <span data-ttu-id="505be-122">Sökvägar måste vara en relativ URL som passar hello följande [reguljärt uttryck](https://msdn.microsoft.com/library/az24scfc.aspx):</span><span class="sxs-lookup"><span data-stu-id="505be-122">Paths must be a relative URL that fits hello following [regular expression](https://msdn.microsoft.com/library/az24scfc.aspx):</span></span>  
   > ><span data-ttu-id="505be-123">Läsa in sökväg för en enskild fil `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;</span><span class="sxs-lookup"><span data-stu-id="505be-123">Load a single file path `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;</span></span>  
   > ><span data-ttu-id="505be-124">Läsa in en enstaka fil med frågesträng`@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`</span><span class="sxs-lookup"><span data-stu-id="505be-124">Load a single file with query string `@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`</span></span>  
   > 
   > <span data-ttu-id="505be-125">Varje tillgång måste ha sin egen sökväg.</span><span class="sxs-lookup"><span data-stu-id="505be-125">Each asset must have its own path.</span></span>  <span data-ttu-id="505be-126">Det finns inga jokertecken funktioner för före inläsning tillgångar.</span><span class="sxs-lookup"><span data-stu-id="505be-126">There is no wildcard functionality for pre-loading assets.</span></span>
   > 
   > 
   
    ![Läs in](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. <span data-ttu-id="505be-128">Klicka på hello **belastningen** knappen.</span><span class="sxs-lookup"><span data-stu-id="505be-128">Click hello **Load** button.</span></span>
   
    ![Läs in](./media/cdn-preload-endpoint/cdn-load-button.png)

> [!NOTE]
> <span data-ttu-id="505be-130">Det finns en begränsning på 10 belastningsförfrågningar per minut per CDN-profilen.</span><span class="sxs-lookup"><span data-stu-id="505be-130">There is a limitation of 10 load requests per minute per CDN profile.</span></span> <span data-ttu-id="505be-131">50 sökvägar tillåts per begäran.</span><span class="sxs-lookup"><span data-stu-id="505be-131">50 paths are allowed per request.</span></span> <span data-ttu-id="505be-132">Varje sökväg har en sökvägens längd på högst 1024 tecken.</span><span class="sxs-lookup"><span data-stu-id="505be-132">Each path has a path-length limit of 1024 characters.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="505be-133">Se även</span><span class="sxs-lookup"><span data-stu-id="505be-133">See also</span></span>
* [<span data-ttu-id="505be-134">Rensa en Azure CDN-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="505be-134">Purge an Azure CDN endpoint</span></span>](cdn-purge-endpoint.md)
* [<span data-ttu-id="505be-135">Azure CDN REST API-referens - rensa eller i förväg läsa in en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="505be-135">Azure CDN REST API reference - Purge or Pre-Load an Endpoint</span></span>](https://msdn.microsoft.com/library/mt634451.aspx)

