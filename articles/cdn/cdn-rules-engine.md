---
title: "Åsidosätta HTTP beteende med hjälp av Azure CDN regelmotor | Microsoft Docs"
description: "Motorn regler kan du anpassa hur Azure CDN hanteras HTTP-begäranden som blockerar leveransen av vissa typer av innehåll, definiera en princip för cachelagring och ändra HTTP-huvuden."
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
ms.openlocfilehash: abfe283476206b181018d187675b47112dc5ad2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="override-http-behavior-using-the-azure-cdn-rules-engine"></a><span data-ttu-id="8649b-103">Åsidosätta HTTP beteende med hjälp av Azure CDN regelmotor</span><span class="sxs-lookup"><span data-stu-id="8649b-103">Override HTTP behavior using the Azure CDN rules engine</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="8649b-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="8649b-104">Overview</span></span>
<span data-ttu-id="8649b-105">Motorn regler kan du anpassa hur HTTP-begäranden hanteras som blockerar leveransen av vissa typer av innehåll, definiera en princip för cachelagring och ändra HTTP-huvuden.</span><span class="sxs-lookup"><span data-stu-id="8649b-105">The rules engine allows you to customize how HTTP requests are handled, such as blocking the delivery of certain types of content, defining a caching policy, and modifying HTTP headers.</span></span>  <span data-ttu-id="8649b-106">Den här kursen visar att skapa en regel som ändrar cachelagringsbeteendet CDN tillgångar.</span><span class="sxs-lookup"><span data-stu-id="8649b-106">This tutorial will demonstrate creating a rule that will change the caching behavior of CDN assets.</span></span>  <span data-ttu-id="8649b-107">Det finns också videoinnehåll i den ”[finns också](#see-also)” avsnittet.</span><span class="sxs-lookup"><span data-stu-id="8649b-107">There's also video content available in the "[See also](#see-also)" section.</span></span>

   > [!TIP] 
   > <span data-ttu-id="8649b-108">En referens till syntax i detalj finns [regler modulreferens](cdn-rules-engine-reference.md).</span><span class="sxs-lookup"><span data-stu-id="8649b-108">For a reference to the syntax in detail, see [Rules Engine Reference](cdn-rules-engine-reference.md).</span></span>
   > 


## <a name="tutorial"></a><span data-ttu-id="8649b-109">Självstudier</span><span class="sxs-lookup"><span data-stu-id="8649b-109">Tutorial</span></span>
1. <span data-ttu-id="8649b-110">CDN-profilbladet klickar du på den **hantera** knappen.</span><span class="sxs-lookup"><span data-stu-id="8649b-110">From the CDN profile blade, click the **Manage** button.</span></span>
   
    ![CDN-profilbladet hantera knappen](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    <span data-ttu-id="8649b-112">CDN-hanteringsportalen öppnas.</span><span class="sxs-lookup"><span data-stu-id="8649b-112">The CDN management portal opens.</span></span>
2. <span data-ttu-id="8649b-113">Klicka på den **HTTP stora** TABB, följt av **regelmotor**.</span><span class="sxs-lookup"><span data-stu-id="8649b-113">Click on the **HTTP Large** tab, followed by **Rules Engine**.</span></span>
   
    <span data-ttu-id="8649b-114">Alternativ för en ny regel visas.</span><span class="sxs-lookup"><span data-stu-id="8649b-114">Options for a new rule are displayed.</span></span>
   
    ![CDN nya alternativ för regel](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="8649b-116">Den ordning i vilken flera regler påverkar hur de ska hanteras.</span><span class="sxs-lookup"><span data-stu-id="8649b-116">The order in which multiple rules are listed affects how they are handled.</span></span> <span data-ttu-id="8649b-117">En efterföljande regel kan åsidosätta de åtgärder som anges av en tidigare regel.</span><span class="sxs-lookup"><span data-stu-id="8649b-117">A subsequent rule may override the actions specified by a previous rule.</span></span>
   > 
   > 
3. <span data-ttu-id="8649b-118">Ange ett namn i den **namn / beskrivning** textruta.</span><span class="sxs-lookup"><span data-stu-id="8649b-118">Enter a name in the **Name / Description** textbox.</span></span>
4. <span data-ttu-id="8649b-119">Identifiera vilken typ av förfrågningar som regeln gäller för.</span><span class="sxs-lookup"><span data-stu-id="8649b-119">Identify the type of requests the rule will apply to.</span></span>  <span data-ttu-id="8649b-120">Som standard den **alltid** matchar villkoret är markerad.</span><span class="sxs-lookup"><span data-stu-id="8649b-120">By default, the **Always** match condition is selected.</span></span>  <span data-ttu-id="8649b-121">Du kommer att använda **alltid** för den här självstudiekursen, så klicka på OK.</span><span class="sxs-lookup"><span data-stu-id="8649b-121">You'll use **Always** for this tutorial, so leave that selected.</span></span>
   
   ![CDN matchar villkoret](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!TIP]
   > <span data-ttu-id="8649b-123">Det finns många typer av matchar villkoren som är tillgängliga i listrutan.</span><span class="sxs-lookup"><span data-stu-id="8649b-123">There are many types of match conditions available in the dropdown.</span></span>  <span data-ttu-id="8649b-124">Klicka på ikonen blå information till vänster om matchar villkoret förklarar markerade villkoret i detalj.</span><span class="sxs-lookup"><span data-stu-id="8649b-124">Clicking on the blue informational icon to the left of the match condition will explain the currently selected condition in detail.</span></span>
   > 
   >  <span data-ttu-id="8649b-125">En fullständig lista över villkorsuttryck i detalj finns [regler motorn villkorsuttryck](cdn-rules-engine-reference-match-conditions.md).</span><span class="sxs-lookup"><span data-stu-id="8649b-125">For the full list of conditional expressions in detail, see [Rules Engine Conditional Expressions](cdn-rules-engine-reference-match-conditions.md).</span></span>
   >  
   > <span data-ttu-id="8649b-126">En fullständig lista över matchar villkoren i detalj finns [regler motorn matchar villkor](cdn-rules-engine-reference-match-conditions.md).</span><span class="sxs-lookup"><span data-stu-id="8649b-126">For the full list of match conditions in detail, see [Rules Engine Match Conditions](cdn-rules-engine-reference-match-conditions.md).</span></span>
   > 
   > 
5. <span data-ttu-id="8649b-127">Klicka på den  **+**  knappen bredvid **funktioner** att lägga till en ny funktion.</span><span class="sxs-lookup"><span data-stu-id="8649b-127">Click the **+** button next to **Features** to add a new feature.</span></span>  <span data-ttu-id="8649b-128">Välj i listrutan till vänster, **kraft interna Max-ålder**.</span><span class="sxs-lookup"><span data-stu-id="8649b-128">In the dropdown on the left, select **Force Internal Max-Age**.</span></span>  <span data-ttu-id="8649b-129">Ange i textrutan som visas, **300**.</span><span class="sxs-lookup"><span data-stu-id="8649b-129">In the textbox that appears, enter **300**.</span></span>  <span data-ttu-id="8649b-130">Lämna de återstående standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="8649b-130">Leave the remaining default values.</span></span>
   
   ![CDN-funktion](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > <span data-ttu-id="8649b-132">Som matchar villkoren visas klickar till vänster om den nya funktionen informationsmeddelande ikonen information om den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="8649b-132">As with match conditions, clicking the blue informational icon to the left of the new feature will display details about this feature.</span></span>  <span data-ttu-id="8649b-133">I fall av **kraft interna Max-ålder**, vi åsidosätter tillgångens **Cache-Control** och **upphör att gälla** rubriker för att styra när kantnod CDN uppdateras tillgången från ursprung.</span><span class="sxs-lookup"><span data-stu-id="8649b-133">In the case of **Force Internal Max-Age**, we are overriding the asset's **Cache-Control** and **Expires** headers to control when the CDN edge node will refresh the asset from the origin.</span></span>  <span data-ttu-id="8649b-134">Vårt exempel 300 sekunder innebär kantnod CDN cachelagrar tillgången i 5 minuter innan du uppdaterar tillgången från sin ursprungsplats.</span><span class="sxs-lookup"><span data-stu-id="8649b-134">Our example of 300 seconds means the CDN edge node will cache the asset for 5 minutes before refreshing the asset from its origin.</span></span>
   > 
   > <span data-ttu-id="8649b-135">En fullständig lista över funktioner i detalj finns [regler motorn funktionen information](cdn-rules-engine-reference-features.md).</span><span class="sxs-lookup"><span data-stu-id="8649b-135">For the full list of features in detail, see [Rules Engine Feature Details](cdn-rules-engine-reference-features.md).</span></span>
   > 
   > 
6. <span data-ttu-id="8649b-136">Klicka på den **Lägg till** för att spara den nya regeln.</span><span class="sxs-lookup"><span data-stu-id="8649b-136">Click the **Add** button to save the new rule.</span></span>  <span data-ttu-id="8649b-137">Den nya regeln är nu väntar på godkännande.</span><span class="sxs-lookup"><span data-stu-id="8649b-137">The new rule is now awaiting approval.</span></span> <span data-ttu-id="8649b-138">När den har blivit godkänd status ändras från **väntande XML** till **Active XML**.</span><span class="sxs-lookup"><span data-stu-id="8649b-138">Once it has been approved, the status will change from **Pending XML** to **Active XML**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="8649b-139">Regler ändringarna kan ta upp till 90 minuter att sprida via CDN.</span><span class="sxs-lookup"><span data-stu-id="8649b-139">Rules changes may take up to 90 minutes to propagate through the CDN.</span></span>
   > 
   > 

## <a name="see-also"></a><span data-ttu-id="8649b-140">Se även</span><span class="sxs-lookup"><span data-stu-id="8649b-140">See also</span></span>
* [<span data-ttu-id="8649b-141">Azure CDN-översikt</span><span class="sxs-lookup"><span data-stu-id="8649b-141">Azure CDN Overview</span></span>](cdn-overview.md)
* [<span data-ttu-id="8649b-142">Regler modulreferens</span><span class="sxs-lookup"><span data-stu-id="8649b-142">Rules Engine Reference</span></span>](cdn-rules-engine-reference.md)
* [<span data-ttu-id="8649b-143">Regler motorn matchar villkor</span><span class="sxs-lookup"><span data-stu-id="8649b-143">Rules Engine Match Conditions</span></span>](cdn-rules-engine-reference-match-conditions.md)
* [<span data-ttu-id="8649b-144">Regler motorn villkorsuttryck</span><span class="sxs-lookup"><span data-stu-id="8649b-144">Rules Engine Conditional Expressions</span></span>](cdn-rules-engine-reference-conditional-expressions.md)
* [<span data-ttu-id="8649b-145">Regler motorn funktioner</span><span class="sxs-lookup"><span data-stu-id="8649b-145">Rules Engine Features</span></span>](cdn-rules-engine-reference-features.md)
* [<span data-ttu-id="8649b-146">Åsidosätta standardbeteendet i HTTP-motorn regler</span><span class="sxs-lookup"><span data-stu-id="8649b-146">Overriding default HTTP behavior using the rules engine</span></span>](cdn-rules-engine.md)
* <span data-ttu-id="8649b-147">[Azure fredagar: Azure CDN kraftfulla nya Premiumfunktioner](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)</span><span class="sxs-lookup"><span data-stu-id="8649b-147">[Azure Fridays: Azure CDN's powerful new Premium Features](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)</span></span>