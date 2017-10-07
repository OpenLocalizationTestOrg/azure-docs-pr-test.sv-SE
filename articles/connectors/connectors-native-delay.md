---
title: "aaaAdd en fördröjning i logikappar | Microsoft Docs"
description: "Översikt över hello fördröjning och fördröjning-tills åtgärder, och hur toouse dem med en logikapp i Azure."
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
ms.openlocfilehash: e5bc9d639adbddc01ee0f6a4c68716f586d4344a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-delay-and-delay-until-actions"></a><span data-ttu-id="b34bb-103">Kom igång med hello fördröjning och fördröjning-tills åtgärder</span><span class="sxs-lookup"><span data-stu-id="b34bb-103">Get started with hello delay and delay-until actions</span></span>
<span data-ttu-id="b34bb-104">Med hjälp av hello fördröjning och ”fördröjning-tills” åtgärder, kan du slutföra arbetsflödet scenarier.</span><span class="sxs-lookup"><span data-stu-id="b34bb-104">By using hello delay and "delay-until" actions, you can complete workflow scenarios.</span></span>

<span data-ttu-id="b34bb-105">Du kan till exempel:</span><span class="sxs-lookup"><span data-stu-id="b34bb-105">For example, you can:</span></span>

* <span data-ttu-id="b34bb-106">Vänta tills en veckodag toosend status uppdateras via e-post.</span><span class="sxs-lookup"><span data-stu-id="b34bb-106">Wait until a weekday toosend a status update over email.</span></span>
* <span data-ttu-id="b34bb-107">Fördröjning hello arbetsflöde till ett HTTP-anrop har tid toofinish innan du fortsätter och hämta hello resultatet.</span><span class="sxs-lookup"><span data-stu-id="b34bb-107">Delay hello workflow until an HTTP call has time toofinish before resuming and retrieving hello result.</span></span>

<span data-ttu-id="b34bb-108">tooget igång med hello fördröjning åtgärden i en logikapp finns [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="b34bb-108">tooget started using hello delay action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-hello-delay-actions"></a><span data-ttu-id="b34bb-109">Använda hello fördröjning åtgärder</span><span class="sxs-lookup"><span data-stu-id="b34bb-109">Use hello delay actions</span></span>
<span data-ttu-id="b34bb-110">En åtgärd är en åtgärd som utförs av hello arbetsflöde som har definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="b34bb-110">An action is an operation that is carried out by hello workflow that is defined in a logic app.</span></span> <span data-ttu-id="b34bb-111">[Mer information om åtgärder](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b34bb-111">[Learn more about actions](connectors-overview.md).</span></span>

<span data-ttu-id="b34bb-112">Här är en Exempelsekvens av hur toouse en fördröjning steg i en logikapp:</span><span class="sxs-lookup"><span data-stu-id="b34bb-112">Here’s an example sequence of how toouse a delay step in a logic app:</span></span>

1. <span data-ttu-id="b34bb-113">När du lägger till en utlösare, klickar du på **nytt steg** tooadd en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="b34bb-113">After adding a trigger, click **New Step** tooadd an action.</span></span>
2. <span data-ttu-id="b34bb-114">Sök efter **fördröjning** toobring in hello fördröjning åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b34bb-114">Search for **delay** toobring up hello delay actions.</span></span> <span data-ttu-id="b34bb-115">I det här exemplet väljer vi **fördröjning**.</span><span class="sxs-lookup"><span data-stu-id="b34bb-115">In this example, we will select **Delay**.</span></span>
   
    ![Fördröjning åtgärder](./media/connectors-native-delay/using-action-1.png)
3. <span data-ttu-id="b34bb-117">Slutför eventuella hello åtgärden egenskaper tooconfigure hello fördröjning.</span><span class="sxs-lookup"><span data-stu-id="b34bb-117">Complete any of hello action properties tooconfigure hello delay.</span></span>
   
    ![Fördröjning config](./media/connectors-native-delay/using-action-2.png)
4. <span data-ttu-id="b34bb-119">Klicka på **spara** toopublish och aktivera hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="b34bb-119">Click **Save** toopublish and activate hello logic app.</span></span>

## <a name="action-details"></a><span data-ttu-id="b34bb-120">Åtgärdsinformation</span><span class="sxs-lookup"><span data-stu-id="b34bb-120">Action details</span></span>
<span data-ttu-id="b34bb-121">hello upprepning utlösaren har hello följande egenskaper som kan konfigureras.</span><span class="sxs-lookup"><span data-stu-id="b34bb-121">hello recurrence trigger has hello following properties that can be configured.</span></span>

### <a name="delay-action"></a><span data-ttu-id="b34bb-122">Fördröjning åtgärd</span><span class="sxs-lookup"><span data-stu-id="b34bb-122">Delay action</span></span>
<span data-ttu-id="b34bb-123">Den här åtgärden fördröjningar hello kör för ett visst tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="b34bb-123">This action delays hello run for a certain time interval.</span></span>
<span data-ttu-id="b34bb-124">A * innebär att det är ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="b34bb-124">A * means that it is a required field.</span></span>

| <span data-ttu-id="b34bb-125">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="b34bb-125">Display name</span></span> | <span data-ttu-id="b34bb-126">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="b34bb-126">Property name</span></span> | <span data-ttu-id="b34bb-127">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b34bb-127">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b34bb-128">Antal *</span><span class="sxs-lookup"><span data-stu-id="b34bb-128">Count*</span></span> |<span data-ttu-id="b34bb-129">Antal</span><span class="sxs-lookup"><span data-stu-id="b34bb-129">count</span></span> |<span data-ttu-id="b34bb-130">hello antalet tid enheter toodelay</span><span class="sxs-lookup"><span data-stu-id="b34bb-130">hello number of time units toodelay</span></span> |
| <span data-ttu-id="b34bb-131">A *</span><span class="sxs-lookup"><span data-stu-id="b34bb-131">Unit*</span></span> |<span data-ttu-id="b34bb-132">enhet</span><span class="sxs-lookup"><span data-stu-id="b34bb-132">unit</span></span> |<span data-ttu-id="b34bb-133">hello tidsenhet: `Second`, `Minute`, `Hour`, eller`Day`</span><span class="sxs-lookup"><span data-stu-id="b34bb-133">hello unit of time: `Second`, `Minute`, `Hour`, or `Day`</span></span> |

<br>

### <a name="delay-until-action"></a><span data-ttu-id="b34bb-134">Fördröjning-förrän åtgärden</span><span class="sxs-lookup"><span data-stu-id="b34bb-134">Delay-until action</span></span>
<span data-ttu-id="b34bb-135">Den här åtgärden fördröjningar hello körs tills angivet datum/tid.</span><span class="sxs-lookup"><span data-stu-id="b34bb-135">This action delays hello run until a specified date/time.</span></span>
<span data-ttu-id="b34bb-136">A * innebär att det är ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="b34bb-136">A * means that it is a required field.</span></span>

| <span data-ttu-id="b34bb-137">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="b34bb-137">Display name</span></span> | <span data-ttu-id="b34bb-138">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="b34bb-138">Property name</span></span> | <span data-ttu-id="b34bb-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b34bb-139">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b34bb-140">År *</span><span class="sxs-lookup"><span data-stu-id="b34bb-140">Year*</span></span> |<span data-ttu-id="b34bb-141">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="b34bb-141">timestamp</span></span> |<span data-ttu-id="b34bb-142">hello år toodelay tills (GMT)</span><span class="sxs-lookup"><span data-stu-id="b34bb-142">hello year toodelay until (GMT)</span></span> |
| <span data-ttu-id="b34bb-143">Månad *</span><span class="sxs-lookup"><span data-stu-id="b34bb-143">Month*</span></span> |<span data-ttu-id="b34bb-144">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="b34bb-144">timestamp</span></span> |<span data-ttu-id="b34bb-145">hello månad toodelay tills (GMT)</span><span class="sxs-lookup"><span data-stu-id="b34bb-145">hello month toodelay until (GMT)</span></span> |
| <span data-ttu-id="b34bb-146">Dag *</span><span class="sxs-lookup"><span data-stu-id="b34bb-146">Day*</span></span> |<span data-ttu-id="b34bb-147">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="b34bb-147">timestamp</span></span> |<span data-ttu-id="b34bb-148">hello dag toodelay tills (GMT)</span><span class="sxs-lookup"><span data-stu-id="b34bb-148">hello day toodelay until (GMT)</span></span> |

<br>

## <a name="next-steps"></a><span data-ttu-id="b34bb-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b34bb-149">Next steps</span></span>
<span data-ttu-id="b34bb-150">Prova nu hello plattform och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="b34bb-150">Now, try out hello platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="b34bb-151">Du kan utforska hello andra tillgängliga kopplingar i logikappar genom att titta på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="b34bb-151">You can explore hello other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

