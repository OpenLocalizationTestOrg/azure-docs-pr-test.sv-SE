---
title: "Lägg till en fördröjning i logikappar | Microsoft Docs"
description: "Översikt över fördröjning och fördröjning-tills åtgärder och hur du använder dem med en Azure logikapp."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 915f48bf-3bd8-4656-be73-91a941d0afcd
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 5f4f7052d48b4ca4ed91212d970551141e78e852
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-delay-and-delay-until-actions"></a><span data-ttu-id="0e629-103">Kom igång med fördröjning och fördröjning-tills åtgärder</span><span class="sxs-lookup"><span data-stu-id="0e629-103">Get started with the delay and delay-until actions</span></span>
<span data-ttu-id="0e629-104">Med hjälp av fördröjningen och ”fördröjning-tills” åtgärder, kan du slutföra arbetsflödet scenarier.</span><span class="sxs-lookup"><span data-stu-id="0e629-104">By using the delay and "delay-until" actions, you can complete workflow scenarios.</span></span>

<span data-ttu-id="0e629-105">Du kan till exempel:</span><span class="sxs-lookup"><span data-stu-id="0e629-105">For example, you can:</span></span>

* <span data-ttu-id="0e629-106">Vänta tills en veckodag att skicka en statusuppdatering via e-post.</span><span class="sxs-lookup"><span data-stu-id="0e629-106">Wait until a weekday to send a status update over email.</span></span>
* <span data-ttu-id="0e629-107">Fördröjer arbetsflödet tills ett HTTP-anrop har tid att slutföra innan du fortsätter och hämta resultatet.</span><span class="sxs-lookup"><span data-stu-id="0e629-107">Delay the workflow until an HTTP call has time to finish before resuming and retrieving the result.</span></span>

<span data-ttu-id="0e629-108">Om du vill komma igång med åtgärden fördröjning i en logikapp, se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="0e629-108">To get started using the delay action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-the-delay-actions"></a><span data-ttu-id="0e629-109">Använd åtgärderna fördröjning</span><span class="sxs-lookup"><span data-stu-id="0e629-109">Use the delay actions</span></span>
<span data-ttu-id="0e629-110">En åtgärd är en åtgärd som utförs av arbetsflödet som definieras i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="0e629-110">An action is an operation that is carried out by the workflow that is defined in a logic app.</span></span> <span data-ttu-id="0e629-111">[Mer information om åtgärder](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0e629-111">[Learn more about actions](connectors-overview.md).</span></span>

<span data-ttu-id="0e629-112">Här är en Exempelsekvens av hur du använder en fördröjning steg i en logikapp:</span><span class="sxs-lookup"><span data-stu-id="0e629-112">Here’s an example sequence of how to use a delay step in a logic app:</span></span>

1. <span data-ttu-id="0e629-113">När du lägger till en utlösare, klickar du på **nytt steg** att lägga till en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0e629-113">After adding a trigger, click **New Step** to add an action.</span></span>
2. <span data-ttu-id="0e629-114">Sök efter **fördröjning** så att åtgärderna som fördröjning.</span><span class="sxs-lookup"><span data-stu-id="0e629-114">Search for **delay** to bring up the delay actions.</span></span> <span data-ttu-id="0e629-115">I det här exemplet väljer vi **fördröjning**.</span><span class="sxs-lookup"><span data-stu-id="0e629-115">In this example, we will select **Delay**.</span></span>
   
    ![Fördröjning åtgärder](./media/connectors-native-delay/using-action-1.png)
3. <span data-ttu-id="0e629-117">Slutför eventuella egenskaper för åtgärden för att konfigurera fördröjningen.</span><span class="sxs-lookup"><span data-stu-id="0e629-117">Complete any of the action properties to configure the delay.</span></span>
   
    ![Fördröjning config](./media/connectors-native-delay/using-action-2.png)
4. <span data-ttu-id="0e629-119">Klicka på **spara** att publicera och aktivera logikappen.</span><span class="sxs-lookup"><span data-stu-id="0e629-119">Click **Save** to publish and activate the logic app.</span></span>

## <a name="action-details"></a><span data-ttu-id="0e629-120">Åtgärdsinformation</span><span class="sxs-lookup"><span data-stu-id="0e629-120">Action details</span></span>
<span data-ttu-id="0e629-121">Återkommande utlösaren har följande egenskaper som kan konfigureras.</span><span class="sxs-lookup"><span data-stu-id="0e629-121">The recurrence trigger has the following properties that can be configured.</span></span>

### <a name="delay-action"></a><span data-ttu-id="0e629-122">Fördröjning åtgärd</span><span class="sxs-lookup"><span data-stu-id="0e629-122">Delay action</span></span>
<span data-ttu-id="0e629-123">Den här åtgärden fördröjningar kör för ett visst tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="0e629-123">This action delays the run for a certain time interval.</span></span>
<span data-ttu-id="0e629-124">A * innebär att det är ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="0e629-124">A * means that it is a required field.</span></span>

| <span data-ttu-id="0e629-125">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="0e629-125">Display name</span></span> | <span data-ttu-id="0e629-126">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="0e629-126">Property name</span></span> | <span data-ttu-id="0e629-127">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0e629-127">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0e629-128">Antal *</span><span class="sxs-lookup"><span data-stu-id="0e629-128">Count*</span></span> |<span data-ttu-id="0e629-129">Antal</span><span class="sxs-lookup"><span data-stu-id="0e629-129">count</span></span> |<span data-ttu-id="0e629-130">Antalet tidsenheter fördröjning</span><span class="sxs-lookup"><span data-stu-id="0e629-130">The number of time units to delay</span></span> |
| <span data-ttu-id="0e629-131">A *</span><span class="sxs-lookup"><span data-stu-id="0e629-131">Unit*</span></span> |<span data-ttu-id="0e629-132">enhet</span><span class="sxs-lookup"><span data-stu-id="0e629-132">unit</span></span> |<span data-ttu-id="0e629-133">Tidsenhet: `Second`, `Minute`, `Hour`, eller`Day`</span><span class="sxs-lookup"><span data-stu-id="0e629-133">The unit of time: `Second`, `Minute`, `Hour`, or `Day`</span></span> |

<br>

### <a name="delay-until-action"></a><span data-ttu-id="0e629-134">Fördröjning-förrän åtgärden</span><span class="sxs-lookup"><span data-stu-id="0e629-134">Delay-until action</span></span>
<span data-ttu-id="0e629-135">Den här åtgärden fördröjningar körs tills angivet datum/tid.</span><span class="sxs-lookup"><span data-stu-id="0e629-135">This action delays the run until a specified date/time.</span></span>
<span data-ttu-id="0e629-136">A * innebär att det är ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="0e629-136">A * means that it is a required field.</span></span>

| <span data-ttu-id="0e629-137">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="0e629-137">Display name</span></span> | <span data-ttu-id="0e629-138">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="0e629-138">Property name</span></span> | <span data-ttu-id="0e629-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0e629-139">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0e629-140">År *</span><span class="sxs-lookup"><span data-stu-id="0e629-140">Year*</span></span> |<span data-ttu-id="0e629-141">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="0e629-141">timestamp</span></span> |<span data-ttu-id="0e629-142">År till fördröja till (GMT)</span><span class="sxs-lookup"><span data-stu-id="0e629-142">The year to delay until (GMT)</span></span> |
| <span data-ttu-id="0e629-143">Månad *</span><span class="sxs-lookup"><span data-stu-id="0e629-143">Month*</span></span> |<span data-ttu-id="0e629-144">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="0e629-144">timestamp</span></span> |<span data-ttu-id="0e629-145">Månad för att fördröja till (GMT)</span><span class="sxs-lookup"><span data-stu-id="0e629-145">The month to delay until (GMT)</span></span> |
| <span data-ttu-id="0e629-146">Dag *</span><span class="sxs-lookup"><span data-stu-id="0e629-146">Day*</span></span> |<span data-ttu-id="0e629-147">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="0e629-147">timestamp</span></span> |<span data-ttu-id="0e629-148">Dag för att fördröja till (GMT)</span><span class="sxs-lookup"><span data-stu-id="0e629-148">The day to delay until (GMT)</span></span> |

<br>

## <a name="next-steps"></a><span data-ttu-id="0e629-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0e629-149">Next steps</span></span>
<span data-ttu-id="0e629-150">Prova nu, plattform och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="0e629-150">Now, try out the platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="0e629-151">Du kan utforska andra tillgängliga kopplingar i logikappar genom att titta på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="0e629-151">You can explore the other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

