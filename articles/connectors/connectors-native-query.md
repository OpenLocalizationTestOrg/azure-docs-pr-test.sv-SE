---
title: "Lägg till instruktionen frågan i logikappar | Microsoft Docs"
description: "Översikt över frågeåtgärden för att utföra åtgärder som filter matris."
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
ms.openlocfilehash: a992fa17a07d6167297c4cf5fe9fb3b58181d7df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-query-action"></a><span data-ttu-id="a2551-103">Kom igång med frågeåtgärden</span><span class="sxs-lookup"><span data-stu-id="a2551-103">Get started with the query action</span></span>
<span data-ttu-id="a2551-104">Genom att använda åtgärden fråga kan arbeta du med batchar och matriser för att utföra arbetsflöden för att:</span><span class="sxs-lookup"><span data-stu-id="a2551-104">By using the query action, you can work with batches and arrays to accomplish workflows to:</span></span>

* <span data-ttu-id="a2551-105">Skapa en aktivitet för alla viktiga poster från en databas.</span><span class="sxs-lookup"><span data-stu-id="a2551-105">Create a task for all high-priority records from a database.</span></span>
* <span data-ttu-id="a2551-106">Spara alla bifogade PDF-filer för e-postmeddelanden till en Azure blob.</span><span class="sxs-lookup"><span data-stu-id="a2551-106">Save all PDF attachments for emails into an Azure blob.</span></span>

<span data-ttu-id="a2551-107">Om du vill komma igång med åtgärden fråga i en logikapp, se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="a2551-107">To get started using the query action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-the-query-action"></a><span data-ttu-id="a2551-108">Använd åtgärden fråga</span><span class="sxs-lookup"><span data-stu-id="a2551-108">Use the query action</span></span>
<span data-ttu-id="a2551-109">En åtgärd är en åtgärd som utförs av arbetsflödet som definieras i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="a2551-109">An action is an operation that is carried out by the workflow that is defined in a logic app.</span></span> <span data-ttu-id="a2551-110">[Mer information om åtgärder](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a2551-110">[Learn more about actions](connectors-overview.md).</span></span>  

<span data-ttu-id="a2551-111">Åtgärden fråga har för närvarande en åtgärd som kallas filter-matrisen som visas i designern.</span><span class="sxs-lookup"><span data-stu-id="a2551-111">The query action currently has one operation, called the filter array, that is exposed in the designer.</span></span> <span data-ttu-id="a2551-112">På så sätt kan du fråga en matris och returnerar en uppsättning filtrerade resultat.</span><span class="sxs-lookup"><span data-stu-id="a2551-112">This allows you to query an array and return a set of filtered results.</span></span>

<span data-ttu-id="a2551-113">Här är hur du lägger till den i en logikapp:</span><span class="sxs-lookup"><span data-stu-id="a2551-113">Here's how you can add it in a logic app:</span></span>

1. <span data-ttu-id="a2551-114">Välj den **nytt steg** knappen.</span><span class="sxs-lookup"><span data-stu-id="a2551-114">Select the **New Step** button.</span></span>
2. <span data-ttu-id="a2551-115">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="a2551-115">Choose **Add an action**.</span></span>
3. <span data-ttu-id="a2551-116">Skriv i sökrutan åtgärd **filter** listan den **Filter matris** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a2551-116">In the action search box, type **filter** to list the **Filter array** action.</span></span>
   
    ![Välj frågeåtgärden som](./media/connectors-native-query/using-action-1.png)
4. <span data-ttu-id="a2551-118">Välj en matris för att filtrera.</span><span class="sxs-lookup"><span data-stu-id="a2551-118">Select an array to filter.</span></span> <span data-ttu-id="a2551-119">(Följande skärmbild visar matris av resultat från en Twitter-sökning.)</span><span class="sxs-lookup"><span data-stu-id="a2551-119">(The following screenshot shows the array of results from a Twitter search.)</span></span>
5. <span data-ttu-id="a2551-120">Skapa ett villkor som ska utvärderas på varje objekt.</span><span class="sxs-lookup"><span data-stu-id="a2551-120">Create a condition to evaluate on each item.</span></span> <span data-ttu-id="a2551-121">(Följande skärmbild filtrerar tweets från användare som har fler än 100 blandare.)</span><span class="sxs-lookup"><span data-stu-id="a2551-121">(The following screenshot filters tweets from users who have more than 100 followers.)</span></span>
   
    ![Utför åtgärden fråga](./media/connectors-native-query/using-action-2.png)
   
    <span data-ttu-id="a2551-123">Åtgärden kommer att skrivas ut en ny matris som innehåller endast resultat som uppfyller kraven för filtret.</span><span class="sxs-lookup"><span data-stu-id="a2551-123">The action will output a new array that contains only results that met the filter requirements.</span></span>
6. <span data-ttu-id="a2551-124">Klicka på det övre vänstra hörnet i verktygsfältet för att spara och logikappen både sparar och publicera (aktivera).</span><span class="sxs-lookup"><span data-stu-id="a2551-124">Click the upper-left corner of the toolbar to save, and your logic app will both save and publish (activate).</span></span>

## <a name="query-action"></a><span data-ttu-id="a2551-125">Frågeåtgärden</span><span class="sxs-lookup"><span data-stu-id="a2551-125">Query action</span></span>
<span data-ttu-id="a2551-126">Här följer information om vad som har stöd för den här anslutningen.</span><span class="sxs-lookup"><span data-stu-id="a2551-126">Here are the details for the action that this connector supports.</span></span> <span data-ttu-id="a2551-127">Kopplingen har en möjlig åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a2551-127">The connector has one possible action.</span></span>

| <span data-ttu-id="a2551-128">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="a2551-128">Action</span></span> | <span data-ttu-id="a2551-129">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a2551-129">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a2551-130">Filtrera matris</span><span class="sxs-lookup"><span data-stu-id="a2551-130">Filter array</span></span> |<span data-ttu-id="a2551-131">Utvärderar ett villkor för varje objekt i en matris och returnerar resultaten</span><span class="sxs-lookup"><span data-stu-id="a2551-131">Evaluates a condition for each item in an array and returns the results</span></span> |

## <a name="action-details"></a><span data-ttu-id="a2551-132">Åtgärdsinformation</span><span class="sxs-lookup"><span data-stu-id="a2551-132">Action details</span></span>
<span data-ttu-id="a2551-133">Åtgärden fråga innehåller en möjlig åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a2551-133">The query action comes with one possible action.</span></span> <span data-ttu-id="a2551-134">I följande tabeller beskrivs de obligatoriska och valfria indatafält för åtgärden och detaljer om motsvarande utdata som är associerade med hjälp av åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a2551-134">The following tables describe the required and optional input fields for the action and the corresponding output details that are associated with using the action.</span></span>

### <a name="filter-array"></a><span data-ttu-id="a2551-135">Filtrera matris</span><span class="sxs-lookup"><span data-stu-id="a2551-135">Filter array</span></span>
<span data-ttu-id="a2551-136">Följande är inmatningsfält för åtgärden, vilket gör en utgående HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="a2551-136">The following are input fields for the action, which makes an HTTP outbound request.</span></span>
<span data-ttu-id="a2551-137">A * innebär att det är ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="a2551-137">A * means that it is a required field.</span></span>

| <span data-ttu-id="a2551-138">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="a2551-138">Display name</span></span> | <span data-ttu-id="a2551-139">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="a2551-139">Property name</span></span> | <span data-ttu-id="a2551-140">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a2551-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a2551-141">Från *</span><span class="sxs-lookup"><span data-stu-id="a2551-141">From*</span></span> |<span data-ttu-id="a2551-142">Från</span><span class="sxs-lookup"><span data-stu-id="a2551-142">from</span></span> |<span data-ttu-id="a2551-143">Matrisen för att filtrera</span><span class="sxs-lookup"><span data-stu-id="a2551-143">The array to filter</span></span> |
| <span data-ttu-id="a2551-144">Villkoret *</span><span class="sxs-lookup"><span data-stu-id="a2551-144">Condition*</span></span> |<span data-ttu-id="a2551-145">där</span><span class="sxs-lookup"><span data-stu-id="a2551-145">where</span></span> |<span data-ttu-id="a2551-146">Villkoret ska utvärderas för varje objekt</span><span class="sxs-lookup"><span data-stu-id="a2551-146">The condition to evaluate for each item</span></span> |

<br>

### <a name="output-details"></a><span data-ttu-id="a2551-147">Information för utdata</span><span class="sxs-lookup"><span data-stu-id="a2551-147">Output details</span></span>
<span data-ttu-id="a2551-148">Nedan visas utdata information för HTTP-svaret.</span><span class="sxs-lookup"><span data-stu-id="a2551-148">The following are output details for the HTTP response.</span></span>

| <span data-ttu-id="a2551-149">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="a2551-149">Property name</span></span> | <span data-ttu-id="a2551-150">Datatyp</span><span class="sxs-lookup"><span data-stu-id="a2551-150">Data type</span></span> | <span data-ttu-id="a2551-151">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a2551-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a2551-152">Filtrerade matris</span><span class="sxs-lookup"><span data-stu-id="a2551-152">Filtered array</span></span> |<span data-ttu-id="a2551-153">matris</span><span class="sxs-lookup"><span data-stu-id="a2551-153">array</span></span> |<span data-ttu-id="a2551-154">En matris som innehåller ett objekt för varje filtrerade resultat</span><span class="sxs-lookup"><span data-stu-id="a2551-154">An array that contains an object for each filtered result</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a2551-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a2551-155">Next steps</span></span>
<span data-ttu-id="a2551-156">Prova nu, plattform och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="a2551-156">Now, try out the platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="a2551-157">Du kan utforska andra tillgängliga kopplingar i logikappar genom att titta på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="a2551-157">You can explore the other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

