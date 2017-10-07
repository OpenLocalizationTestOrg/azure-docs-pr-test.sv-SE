---
title: "aaaAdd hello frågeåtgärden i logikappar | Microsoft Docs"
description: "Översikt över hello frågeåtgärden för att utföra åtgärder som filter matris."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 34e702c7-f9e5-4885-9266-fc7404adecfe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2016
ms.author: jehollan
ms.openlocfilehash: 3d4be901e7e6bf1b644057648930667ab34f2124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-query-action"></a><span data-ttu-id="cda84-103">Kom igång med hello frågeåtgärden</span><span class="sxs-lookup"><span data-stu-id="cda84-103">Get started with hello query action</span></span>
<span data-ttu-id="cda84-104">Med åtgärden hello frågan kan du arbeta med batchar och matriser tooaccomplish arbetsflöden för att:</span><span class="sxs-lookup"><span data-stu-id="cda84-104">By using hello query action, you can work with batches and arrays tooaccomplish workflows to:</span></span>

* <span data-ttu-id="cda84-105">Skapa en aktivitet för alla viktiga poster från en databas.</span><span class="sxs-lookup"><span data-stu-id="cda84-105">Create a task for all high-priority records from a database.</span></span>
* <span data-ttu-id="cda84-106">Spara alla bifogade PDF-filer för e-postmeddelanden till en Azure blob.</span><span class="sxs-lookup"><span data-stu-id="cda84-106">Save all PDF attachments for emails into an Azure blob.</span></span>

<span data-ttu-id="cda84-107">tooget igång med hello frågeåtgärden i en logikapp finns [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="cda84-107">tooget started using hello query action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-hello-query-action"></a><span data-ttu-id="cda84-108">Använd hello frågan åtgärd</span><span class="sxs-lookup"><span data-stu-id="cda84-108">Use hello query action</span></span>
<span data-ttu-id="cda84-109">En åtgärd är en åtgärd som utförs av hello arbetsflöde som har definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="cda84-109">An action is an operation that is carried out by hello workflow that is defined in a logic app.</span></span> <span data-ttu-id="cda84-110">[Mer information om åtgärder](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cda84-110">[Learn more about actions](connectors-overview.md).</span></span>  

<span data-ttu-id="cda84-111">hello frågeåtgärden har för närvarande en åtgärd som kallas hello filter matris som exponeras i hello designer.</span><span class="sxs-lookup"><span data-stu-id="cda84-111">hello query action currently has one operation, called hello filter array, that is exposed in hello designer.</span></span> <span data-ttu-id="cda84-112">Detta gör du tooquery en matris och returnerar en uppsättning filtrerade resultat.</span><span class="sxs-lookup"><span data-stu-id="cda84-112">This allows you tooquery an array and return a set of filtered results.</span></span>

<span data-ttu-id="cda84-113">Här är hur du lägger till den i en logikapp:</span><span class="sxs-lookup"><span data-stu-id="cda84-113">Here's how you can add it in a logic app:</span></span>

1. <span data-ttu-id="cda84-114">Välj hello **nytt steg** knappen.</span><span class="sxs-lookup"><span data-stu-id="cda84-114">Select hello **New Step** button.</span></span>
2. <span data-ttu-id="cda84-115">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="cda84-115">Choose **Add an action**.</span></span>
3. <span data-ttu-id="cda84-116">Skriv i sökrutan för hello åtgärd **filter** toolist hello **Filter matris** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="cda84-116">In hello action search box, type **filter** toolist hello **Filter array** action.</span></span>
   
    ![Välj hello frågan åtgärd](./media/connectors-native-query/using-action-1.png)
4. <span data-ttu-id="cda84-118">Välj en matris toofilter.</span><span class="sxs-lookup"><span data-stu-id="cda84-118">Select an array toofilter.</span></span> <span data-ttu-id="cda84-119">(hello följande skärmbild visar hello matris av resultat från en Twitter-sökning.)</span><span class="sxs-lookup"><span data-stu-id="cda84-119">(hello following screenshot shows hello array of results from a Twitter search.)</span></span>
5. <span data-ttu-id="cda84-120">Skapa ett villkor tooevaluate på varje objekt.</span><span class="sxs-lookup"><span data-stu-id="cda84-120">Create a condition tooevaluate on each item.</span></span> <span data-ttu-id="cda84-121">(följande skärmbild hello filtrerar tweets från användare som har fler än 100 blandare.)</span><span class="sxs-lookup"><span data-stu-id="cda84-121">(hello following screenshot filters tweets from users who have more than 100 followers.)</span></span>
   
    ![Fullständig hello frågeåtgärden](./media/connectors-native-query/using-action-2.png)
   
    <span data-ttu-id="cda84-123">hello-åtgärden kommer att skrivas ut en ny matris som innehåller endast resultat som uppfyller kraven för hello filter.</span><span class="sxs-lookup"><span data-stu-id="cda84-123">hello action will output a new array that contains only results that met hello filter requirements.</span></span>
6. <span data-ttu-id="cda84-124">Klicka på hello övre vänstra hörnet av hello verktygsfältet toosave och din logik app kommer båda att spara och publicera (aktivera).</span><span class="sxs-lookup"><span data-stu-id="cda84-124">Click hello upper-left corner of hello toolbar toosave, and your logic app will both save and publish (activate).</span></span>

## <a name="query-action"></a><span data-ttu-id="cda84-125">Frågeåtgärden</span><span class="sxs-lookup"><span data-stu-id="cda84-125">Query action</span></span>
<span data-ttu-id="cda84-126">Här följer hello information för hello-åtgärd som har stöd för den här anslutningen.</span><span class="sxs-lookup"><span data-stu-id="cda84-126">Here are hello details for hello action that this connector supports.</span></span> <span data-ttu-id="cda84-127">hello-koppling har en möjlig åtgärd.</span><span class="sxs-lookup"><span data-stu-id="cda84-127">hello connector has one possible action.</span></span>

| <span data-ttu-id="cda84-128">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="cda84-128">Action</span></span> | <span data-ttu-id="cda84-129">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cda84-129">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cda84-130">Filtrera matris</span><span class="sxs-lookup"><span data-stu-id="cda84-130">Filter array</span></span> |<span data-ttu-id="cda84-131">Utvärderar ett villkor för varje objekt i en matris och returnerar hello resultat</span><span class="sxs-lookup"><span data-stu-id="cda84-131">Evaluates a condition for each item in an array and returns hello results</span></span> |

## <a name="action-details"></a><span data-ttu-id="cda84-132">Åtgärdsinformation</span><span class="sxs-lookup"><span data-stu-id="cda84-132">Action details</span></span>
<span data-ttu-id="cda84-133">hello frågeåtgärden levereras med en möjlig åtgärd.</span><span class="sxs-lookup"><span data-stu-id="cda84-133">hello query action comes with one possible action.</span></span> <span data-ttu-id="cda84-134">hello följande tabeller beskrivs hello nödvändiga och valfria indatafält för hello åtgärd och hello motsvarande utdata information som är associerade med åtgärden hello.</span><span class="sxs-lookup"><span data-stu-id="cda84-134">hello following tables describe hello required and optional input fields for hello action and hello corresponding output details that are associated with using hello action.</span></span>

### <a name="filter-array"></a><span data-ttu-id="cda84-135">Filtrera matris</span><span class="sxs-lookup"><span data-stu-id="cda84-135">Filter array</span></span>
<span data-ttu-id="cda84-136">hello följande är inmatningsfält för hello åtgärden, vilket gör en utgående HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="cda84-136">hello following are input fields for hello action, which makes an HTTP outbound request.</span></span>
<span data-ttu-id="cda84-137">A * innebär att det är ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="cda84-137">A * means that it is a required field.</span></span>

| <span data-ttu-id="cda84-138">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="cda84-138">Display name</span></span> | <span data-ttu-id="cda84-139">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="cda84-139">Property name</span></span> | <span data-ttu-id="cda84-140">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cda84-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cda84-141">Från *</span><span class="sxs-lookup"><span data-stu-id="cda84-141">From*</span></span> |<span data-ttu-id="cda84-142">Från</span><span class="sxs-lookup"><span data-stu-id="cda84-142">from</span></span> |<span data-ttu-id="cda84-143">hello matris toofilter</span><span class="sxs-lookup"><span data-stu-id="cda84-143">hello array toofilter</span></span> |
| <span data-ttu-id="cda84-144">Villkoret *</span><span class="sxs-lookup"><span data-stu-id="cda84-144">Condition*</span></span> |<span data-ttu-id="cda84-145">där</span><span class="sxs-lookup"><span data-stu-id="cda84-145">where</span></span> |<span data-ttu-id="cda84-146">hello villkoret tooevaluate för varje objekt</span><span class="sxs-lookup"><span data-stu-id="cda84-146">hello condition tooevaluate for each item</span></span> |

<br>

### <a name="output-details"></a><span data-ttu-id="cda84-147">Information för utdata</span><span class="sxs-lookup"><span data-stu-id="cda84-147">Output details</span></span>
<span data-ttu-id="cda84-148">hello nedan visas utdata information för hello HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="cda84-148">hello following are output details for hello HTTP response.</span></span>

| <span data-ttu-id="cda84-149">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="cda84-149">Property name</span></span> | <span data-ttu-id="cda84-150">Datatyp</span><span class="sxs-lookup"><span data-stu-id="cda84-150">Data type</span></span> | <span data-ttu-id="cda84-151">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cda84-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cda84-152">Filtrerade matris</span><span class="sxs-lookup"><span data-stu-id="cda84-152">Filtered array</span></span> |<span data-ttu-id="cda84-153">matris</span><span class="sxs-lookup"><span data-stu-id="cda84-153">array</span></span> |<span data-ttu-id="cda84-154">En matris som innehåller ett objekt för varje filtrerade resultat</span><span class="sxs-lookup"><span data-stu-id="cda84-154">An array that contains an object for each filtered result</span></span> |

## <a name="next-steps"></a><span data-ttu-id="cda84-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cda84-155">Next steps</span></span>
<span data-ttu-id="cda84-156">Prova nu hello plattform och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="cda84-156">Now, try out hello platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="cda84-157">Du kan utforska hello andra tillgängliga kopplingar i logikappar genom att titta på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="cda84-157">You can explore hello other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

