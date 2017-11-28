---
title: "aaaOverride HTTP beteende med hjälp av hello Azure CDN regelmotor | Microsoft Docs"
description: "hello regelmotor kan du toocustomize hur Azure CDN hanteras HTTP-begäranden som blockerar hello leverans av vissa typer av innehåll, definiera en princip för cachelagring och ändra HTTP-huvuden."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 625a912b-91f2-485d-8991-128cc194ee71
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: dd7194be9dbda43180c64568d3e1f52c5c513a7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="override-http-behavior-using-hello-azure-cdn-rules-engine"></a><span data-ttu-id="ad715-103">Åsidosätta HTTP beteende med hjälp av hello Azure CDN regelmotor</span><span class="sxs-lookup"><span data-stu-id="ad715-103">Override HTTP behavior using hello Azure CDN rules engine</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="ad715-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="ad715-104">Overview</span></span>
<span data-ttu-id="ad715-105">hello regelmotor kan toocustomize hur HTTP-begäranden hanteras som blockerar hello leverans av vissa typer av innehåll, definiera en princip för cachelagring och ändra HTTP-huvuden.</span><span class="sxs-lookup"><span data-stu-id="ad715-105">hello rules engine allows you toocustomize how HTTP requests are handled, such as blocking hello delivery of certain types of content, defining a caching policy, and modifying HTTP headers.</span></span>  <span data-ttu-id="ad715-106">Den här kursen visar att skapa en regel som ändrar hello cachelagring av frågesträngar CDN tillgångar.</span><span class="sxs-lookup"><span data-stu-id="ad715-106">This tutorial will demonstrate creating a rule that will change hello caching behavior of CDN assets.</span></span>  <span data-ttu-id="ad715-107">Det finns också videoinnehåll i hello ”[finns också](#see-also)” avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ad715-107">There's also video content available in hello "[See also](#see-also)" section.</span></span>

   > [!TIP] 
   > <span data-ttu-id="ad715-108">En referens toohello syntax i detalj finns [regler modulreferens](cdn-rules-engine-reference.md).</span><span class="sxs-lookup"><span data-stu-id="ad715-108">For a reference toohello syntax in detail, see [Rules Engine Reference](cdn-rules-engine-reference.md).</span></span>
   > 


## <a name="tutorial"></a><span data-ttu-id="ad715-109">Självstudier</span><span class="sxs-lookup"><span data-stu-id="ad715-109">Tutorial</span></span>
1. <span data-ttu-id="ad715-110">Klicka på hello hello CDN-profilbladet **hantera** knappen.</span><span class="sxs-lookup"><span data-stu-id="ad715-110">From hello CDN profile blade, click hello **Manage** button.</span></span>
   
    ![CDN-profilbladet hantera knappen](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    <span data-ttu-id="ad715-112">hello CDN-hanteringsportalen öppnas.</span><span class="sxs-lookup"><span data-stu-id="ad715-112">hello CDN management portal opens.</span></span>
2. <span data-ttu-id="ad715-113">Klicka på hello **HTTP stora** TABB, följt av **regelmotor**.</span><span class="sxs-lookup"><span data-stu-id="ad715-113">Click on hello **HTTP Large** tab, followed by **Rules Engine**.</span></span>
   
    <span data-ttu-id="ad715-114">Alternativ för en ny regel visas.</span><span class="sxs-lookup"><span data-stu-id="ad715-114">Options for a new rule are displayed.</span></span>
   
    ![CDN nya alternativ för regel](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="ad715-116">hello ordning i vilken flera regler påverkar hur de ska hanteras.</span><span class="sxs-lookup"><span data-stu-id="ad715-116">hello order in which multiple rules are listed affects how they are handled.</span></span> <span data-ttu-id="ad715-117">En efterföljande regel kan åsidosätta hello-åtgärder som angetts av en tidigare regel.</span><span class="sxs-lookup"><span data-stu-id="ad715-117">A subsequent rule may override hello actions specified by a previous rule.</span></span>
   > 
   > 
3. <span data-ttu-id="ad715-118">Ange ett namn i hello **namn / beskrivning** textruta.</span><span class="sxs-lookup"><span data-stu-id="ad715-118">Enter a name in hello **Name / Description** textbox.</span></span>
4. <span data-ttu-id="ad715-119">Identifiera hello typ av förfrågningar hello regeln gäller för.</span><span class="sxs-lookup"><span data-stu-id="ad715-119">Identify hello type of requests hello rule will apply to.</span></span>  <span data-ttu-id="ad715-120">Som standard hello **alltid** matchar villkoret är markerad.</span><span class="sxs-lookup"><span data-stu-id="ad715-120">By default, hello **Always** match condition is selected.</span></span>  <span data-ttu-id="ad715-121">Du kommer att använda **alltid** för den här självstudiekursen, så klicka på OK.</span><span class="sxs-lookup"><span data-stu-id="ad715-121">You'll use **Always** for this tutorial, so leave that selected.</span></span>
   
   ![CDN matchar villkoret](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!TIP]
   > <span data-ttu-id="ad715-123">Det finns många typer av matchar villkoren som är tillgängliga i listrutan hello.</span><span class="sxs-lookup"><span data-stu-id="ad715-123">There are many types of match conditions available in hello dropdown.</span></span>  <span data-ttu-id="ad715-124">Klicka på hello blå informationsmeddelande ikonen toohello förklarar till vänster i hello matchar villkoret hello valt villkor i detalj.</span><span class="sxs-lookup"><span data-stu-id="ad715-124">Clicking on hello blue informational icon toohello left of hello match condition will explain hello currently selected condition in detail.</span></span>
   > 
   >  <span data-ttu-id="ad715-125">Hello fullständig lista över villkorsuttryck i detalj, se [regler motorn villkorsuttryck](cdn-rules-engine-reference-match-conditions.md).</span><span class="sxs-lookup"><span data-stu-id="ad715-125">For hello full list of conditional expressions in detail, see [Rules Engine Conditional Expressions](cdn-rules-engine-reference-match-conditions.md).</span></span>
   >  
   > <span data-ttu-id="ad715-126">Hello fullständig lista över matchar villkoren i detalj, se [regler motorn matchar villkor](cdn-rules-engine-reference-match-conditions.md).</span><span class="sxs-lookup"><span data-stu-id="ad715-126">For hello full list of match conditions in detail, see [Rules Engine Match Conditions](cdn-rules-engine-reference-match-conditions.md).</span></span>
   > 
   > 
5. <span data-ttu-id="ad715-127">Klicka på hello  **+**  knappen för nästa**funktioner** tooadd en ny funktion.</span><span class="sxs-lookup"><span data-stu-id="ad715-127">Click hello **+** button next too**Features** tooadd a new feature.</span></span>  <span data-ttu-id="ad715-128">Välj i listrutan hello hello vänster **kraft interna Max-ålder**.</span><span class="sxs-lookup"><span data-stu-id="ad715-128">In hello dropdown on hello left, select **Force Internal Max-Age**.</span></span>  <span data-ttu-id="ad715-129">Ange i hello textrutan som visas, **300**.</span><span class="sxs-lookup"><span data-stu-id="ad715-129">In hello textbox that appears, enter **300**.</span></span>  <span data-ttu-id="ad715-130">Lämna hello återstående standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="ad715-130">Leave hello remaining default values.</span></span>
   
   ![CDN-funktion](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > <span data-ttu-id="ad715-132">Som matchar villkoren klickar du på hello blå informationsmeddelande ikonen toohello vänsterkant hello nya visar funktionen information om den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ad715-132">As with match conditions, clicking hello blue informational icon toohello left of hello new feature will display details about this feature.</span></span>  <span data-ttu-id="ad715-133">Hello gäller **kraft interna Max-ålder**, vi åsidosätter hello tillgången **Cache-Control** och **Expires** huvuden toocontrol när hello CDN kantnod uppdateras hello tillgångsinformation från hello ursprunget.</span><span class="sxs-lookup"><span data-stu-id="ad715-133">In hello case of **Force Internal Max-Age**, we are overriding hello asset's **Cache-Control** and **Expires** headers toocontrol when hello CDN edge node will refresh hello asset from hello origin.</span></span>  <span data-ttu-id="ad715-134">Vårt exempel 300 sekunder innebär hello CDN kantnod cachelagras hello tillgången i 5 minuter innan du uppdaterar hello tillgångsinformation från sin ursprungsplats.</span><span class="sxs-lookup"><span data-stu-id="ad715-134">Our example of 300 seconds means hello CDN edge node will cache hello asset for 5 minutes before refreshing hello asset from its origin.</span></span>
   > 
   > <span data-ttu-id="ad715-135">Hello fullständig lista över funktioner i detalj finns [regler motorn funktionen information](cdn-rules-engine-reference-features.md).</span><span class="sxs-lookup"><span data-stu-id="ad715-135">For hello full list of features in detail, see [Rules Engine Feature Details](cdn-rules-engine-reference-features.md).</span></span>
   > 
   > 
6. <span data-ttu-id="ad715-136">Klicka på hello **Lägg till** knappen toosave hello ny regel.</span><span class="sxs-lookup"><span data-stu-id="ad715-136">Click hello **Add** button toosave hello new rule.</span></span>  <span data-ttu-id="ad715-137">hello ny regel nu väntar på godkännande.</span><span class="sxs-lookup"><span data-stu-id="ad715-137">hello new rule is now awaiting approval.</span></span> <span data-ttu-id="ad715-138">När den har godkänts hello status ändras från **väntande XML** för**Active XML**.</span><span class="sxs-lookup"><span data-stu-id="ad715-138">Once it has been approved, hello status will change from **Pending XML** too**Active XML**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="ad715-139">Regler ändringarna kan ta too90 minuter toopropagate via hello CDN.</span><span class="sxs-lookup"><span data-stu-id="ad715-139">Rules changes may take up too90 minutes toopropagate through hello CDN.</span></span>
   > 
   > 

## <a name="see-also"></a><span data-ttu-id="ad715-140">Se även</span><span class="sxs-lookup"><span data-stu-id="ad715-140">See also</span></span>
* [<span data-ttu-id="ad715-141">Azure CDN-översikt</span><span class="sxs-lookup"><span data-stu-id="ad715-141">Azure CDN Overview</span></span>](cdn-overview.md)
* [<span data-ttu-id="ad715-142">Regler modulreferens</span><span class="sxs-lookup"><span data-stu-id="ad715-142">Rules Engine Reference</span></span>](cdn-rules-engine-reference.md)
* [<span data-ttu-id="ad715-143">Regler motorn matchar villkor</span><span class="sxs-lookup"><span data-stu-id="ad715-143">Rules Engine Match Conditions</span></span>](cdn-rules-engine-reference-match-conditions.md)
* [<span data-ttu-id="ad715-144">Regler motorn villkorsuttryck</span><span class="sxs-lookup"><span data-stu-id="ad715-144">Rules Engine Conditional Expressions</span></span>](cdn-rules-engine-reference-conditional-expressions.md)
* [<span data-ttu-id="ad715-145">Regler motorn funktioner</span><span class="sxs-lookup"><span data-stu-id="ad715-145">Rules Engine Features</span></span>](cdn-rules-engine-reference-features.md)
* [<span data-ttu-id="ad715-146">Åsidosätta HTTP standardinställningar med hjälp av hello regelmotor</span><span class="sxs-lookup"><span data-stu-id="ad715-146">Overriding default HTTP behavior using hello rules engine</span></span>](cdn-rules-engine.md)
* <span data-ttu-id="ad715-147">[Azure fredagar: Azure CDN kraftfulla nya Premiumfunktioner](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)</span><span class="sxs-lookup"><span data-stu-id="ad715-147">[Azure Fridays: Azure CDN's powerful new Premium Features](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)</span></span>