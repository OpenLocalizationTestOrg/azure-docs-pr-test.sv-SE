---
title: "Använda åtgärder för förfrågan och svar | Microsoft Docs"
description: "Översikt över förfrågan och svar utlösare och åtgärden i en Azure logikapp"
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 566924a4-0988-4d86-9ecd-ad22507858c0
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: e45b07d709927af64cfba28dfb0d8ee9cb8893b3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-request-and-response-components"></a><span data-ttu-id="c3d54-103">Kom igång med komponenter för förfrågan och svar</span><span class="sxs-lookup"><span data-stu-id="c3d54-103">Get started with the request and response components</span></span>
<span data-ttu-id="c3d54-104">Med förfrågan och svar komponenterna i en logikapp, kan du svara i realtid på händelser.</span><span class="sxs-lookup"><span data-stu-id="c3d54-104">With the request and response components in a logic app, you can respond in real time to events.</span></span>

<span data-ttu-id="c3d54-105">Du kan till exempel:</span><span class="sxs-lookup"><span data-stu-id="c3d54-105">For example, you can:</span></span>

* <span data-ttu-id="c3d54-106">Svara på en HTTP-begäran med data från en lokal databas via en logikapp.</span><span class="sxs-lookup"><span data-stu-id="c3d54-106">Respond to an HTTP request with data from an on-premises database through a logic app.</span></span>
* <span data-ttu-id="c3d54-107">Utlös en logikapp från en extern webhook-händelse.</span><span class="sxs-lookup"><span data-stu-id="c3d54-107">Trigger a logic app from an external webhook event.</span></span>
* <span data-ttu-id="c3d54-108">Anropa en logikapp med en åtgärd för förfrågan och svar från inom en annan logikapp.</span><span class="sxs-lookup"><span data-stu-id="c3d54-108">Call a logic app with a request and response action from within another logic app.</span></span>

<span data-ttu-id="c3d54-109">Om du vill komma igång med åtgärder för förfrågan och svar i en logikapp, se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="c3d54-109">To get started using the request and response actions in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-the-http-request-trigger"></a><span data-ttu-id="c3d54-110">Använda HTTP-begäran-utlösare</span><span class="sxs-lookup"><span data-stu-id="c3d54-110">Use the HTTP Request trigger</span></span>
<span data-ttu-id="c3d54-111">En utlösare är en händelse som kan användas för att starta arbetsflödet som definieras i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="c3d54-111">A trigger is an event that can be used to start the workflow that is defined in a logic app.</span></span> <span data-ttu-id="c3d54-112">[Mer information om utlösare](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c3d54-112">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="c3d54-113">Här är en Exempelsekvens av hur du ställer in en HTTP-begäran i logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="c3d54-113">Here’s an example sequence of how to set up an HTTP request in the Logic App Designer.</span></span>

1. <span data-ttu-id="c3d54-114">Lägg till utlösaren **begäran - när en HTTP-begäran tas emot** i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="c3d54-114">Add the trigger **Request - When an HTTP request is received** in your logic app.</span></span> <span data-ttu-id="c3d54-115">Du kan också medföra en JSON-schema (med hjälp av ett verktyg som [JSONSchema.net](http://jsonschema.net)) för begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="c3d54-115">You can optionally provide a JSON schema (by using a tool like [JSONSchema.net](http://jsonschema.net)) for the request body.</span></span> <span data-ttu-id="c3d54-116">Detta gör att generera token för egenskaper i HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="c3d54-116">This allows the designer to generate tokens for properties in the HTTP request.</span></span>
2. <span data-ttu-id="c3d54-117">Lägg till en annan åtgärd så att du kan spara logikappen.</span><span class="sxs-lookup"><span data-stu-id="c3d54-117">Add another action so that you can save the logic app.</span></span>
3. <span data-ttu-id="c3d54-118">Du kan hämta URL för HTTP-begäran från kortet begäran när du har sparat logikappen.</span><span class="sxs-lookup"><span data-stu-id="c3d54-118">After saving the logic app, you can get the HTTP request URL from the request card.</span></span>
4. <span data-ttu-id="c3d54-119">En HTTP POST (du kan använda ett verktyg som [Postman](https://www.getpostman.com/)) till URL som utlöser logikappen.</span><span class="sxs-lookup"><span data-stu-id="c3d54-119">An HTTP POST (you can use a tool like [Postman](https://www.getpostman.com/)) to the URL triggers the logic app.</span></span>

> [!NOTE]
> <span data-ttu-id="c3d54-120">Om du inte definierar en åtgärd för svar, en `202 ACCEPTED` omedelbart svar returneras till anroparen.</span><span class="sxs-lookup"><span data-stu-id="c3d54-120">If you don't define a response action, a `202 ACCEPTED` response is immediately returned to the caller.</span></span> <span data-ttu-id="c3d54-121">Du kan använda åtgärden svar för att anpassa ett svar.</span><span class="sxs-lookup"><span data-stu-id="c3d54-121">You can use the response action to customize a response.</span></span>
> 
> 

![Svaret utlösare](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a><span data-ttu-id="c3d54-123">Använd åtgärden HTTP-svar</span><span class="sxs-lookup"><span data-stu-id="c3d54-123">Use the HTTP Response action</span></span>
<span data-ttu-id="c3d54-124">HTTP-svar-åtgärden är endast giltig när du använder den i ett arbetsflöde som utlöses av en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="c3d54-124">The HTTP Response action is only valid when you use it in a workflow that is triggered by an HTTP request.</span></span> <span data-ttu-id="c3d54-125">Om du inte definierar en åtgärd för svar, en `202 ACCEPTED` omedelbart svar returneras till anroparen.</span><span class="sxs-lookup"><span data-stu-id="c3d54-125">If you don't define a response action, a `202 ACCEPTED` response is immediately returned to the caller.</span></span>  <span data-ttu-id="c3d54-126">Du kan lägga till en åtgärd som svar på något steg i arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="c3d54-126">You can add a response action at any step within the workflow.</span></span> <span data-ttu-id="c3d54-127">Logikappen upprätthåller bara den inkommande begäranden öppna en minut för svar.</span><span class="sxs-lookup"><span data-stu-id="c3d54-127">The logic app only keeps the incoming request open for one minute for a response.</span></span>  <span data-ttu-id="c3d54-128">Efter en minut, om inget svar skickades från arbetsflödet (och det finns ett svar-åtgärd i definitionen), en `504 GATEWAY TIMEOUT` returneras till anroparen.</span><span class="sxs-lookup"><span data-stu-id="c3d54-128">After one minute, if no response was sent from the workflow (and a response action exists in the definition), a `504 GATEWAY TIMEOUT` is returned to the caller.</span></span>

<span data-ttu-id="c3d54-129">Här är hur du lägger till en åtgärd för HTTP-svar:</span><span class="sxs-lookup"><span data-stu-id="c3d54-129">Here's how to add an HTTP Response action:</span></span>

1. <span data-ttu-id="c3d54-130">Välj den **nytt steg** knappen.</span><span class="sxs-lookup"><span data-stu-id="c3d54-130">Select the **New Step** button.</span></span>
2. <span data-ttu-id="c3d54-131">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="c3d54-131">Choose **Add an action**.</span></span>
3. <span data-ttu-id="c3d54-132">Skriv i sökrutan åtgärd **svar** att lista åtgärden svar.</span><span class="sxs-lookup"><span data-stu-id="c3d54-132">In the action search box, type **response** to list the Response action.</span></span>
   
    ![Välj åtgärden som svar](./media/connectors-native-reqres/using-action-1.png)
4. <span data-ttu-id="c3d54-134">Lägga till i alla parametrar som krävs för HTTP-svarsmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="c3d54-134">Add in any parameters that are required for the HTTP response message.</span></span>
   
    ![Utför åtgärden svar](./media/connectors-native-reqres/using-action-2.png)
5. <span data-ttu-id="c3d54-136">Klicka på det övre vänstra hörnet i verktygsfältet för att spara och logikappen både sparar och publicera (aktivera).</span><span class="sxs-lookup"><span data-stu-id="c3d54-136">Click the upper-left corner of the toolbar to save, and your logic app will both save and publish (activate).</span></span>

## <a name="request-trigger"></a><span data-ttu-id="c3d54-137">Begäran utlösare</span><span class="sxs-lookup"><span data-stu-id="c3d54-137">Request trigger</span></span>
<span data-ttu-id="c3d54-138">Här följer information om utlösaren som har stöd för den här anslutningen.</span><span class="sxs-lookup"><span data-stu-id="c3d54-138">Here are the details for the trigger that this connector supports.</span></span> <span data-ttu-id="c3d54-139">Det finns en enskild begäran-utlösare.</span><span class="sxs-lookup"><span data-stu-id="c3d54-139">There is a single request trigger.</span></span>

| <span data-ttu-id="c3d54-140">Utlösare</span><span class="sxs-lookup"><span data-stu-id="c3d54-140">Trigger</span></span> | <span data-ttu-id="c3d54-141">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c3d54-141">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c3d54-142">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="c3d54-142">Request</span></span> |<span data-ttu-id="c3d54-143">Inträffar när en HTTP-begäran tas emot</span><span class="sxs-lookup"><span data-stu-id="c3d54-143">Occurs when an HTTP request is received</span></span> |

## <a name="response-action"></a><span data-ttu-id="c3d54-144">Svaret åtgärd</span><span class="sxs-lookup"><span data-stu-id="c3d54-144">Response action</span></span>
<span data-ttu-id="c3d54-145">Här följer information om vad som har stöd för den här anslutningen.</span><span class="sxs-lookup"><span data-stu-id="c3d54-145">Here are the details for the action that this connector supports.</span></span> <span data-ttu-id="c3d54-146">Det finns ett enda svar-åtgärd som kan endast användas när den åtföljs av en begäran-utlösare.</span><span class="sxs-lookup"><span data-stu-id="c3d54-146">There is a single response action that can only be used when it is accompanied by a request trigger.</span></span>

| <span data-ttu-id="c3d54-147">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="c3d54-147">Action</span></span> | <span data-ttu-id="c3d54-148">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c3d54-148">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c3d54-149">Svar</span><span class="sxs-lookup"><span data-stu-id="c3d54-149">Response</span></span> |<span data-ttu-id="c3d54-150">Returnerar ett svar till korrelerade HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="c3d54-150">Returns a response to the correlated HTTP request</span></span> |

### <a name="trigger-and-action-details"></a><span data-ttu-id="c3d54-151">Trigger och action information</span><span class="sxs-lookup"><span data-stu-id="c3d54-151">Trigger and action details</span></span>
<span data-ttu-id="c3d54-152">I följande tabeller beskrivs indatafält för trigger och action och motsvarande utdata information.</span><span class="sxs-lookup"><span data-stu-id="c3d54-152">The following tables describe the input fields for the trigger and action, and the corresponding output details.</span></span>

#### <a name="request-trigger"></a><span data-ttu-id="c3d54-153">Begäran utlösare</span><span class="sxs-lookup"><span data-stu-id="c3d54-153">Request trigger</span></span>
<span data-ttu-id="c3d54-154">Följande är ett inmatningsfält för utlösaren från en inkommande HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="c3d54-154">The following is an input field for the trigger from an incoming HTTP request.</span></span>

| <span data-ttu-id="c3d54-155">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="c3d54-155">Display name</span></span> | <span data-ttu-id="c3d54-156">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="c3d54-156">Property name</span></span> | <span data-ttu-id="c3d54-157">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c3d54-157">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c3d54-158">JSON-Schema</span><span class="sxs-lookup"><span data-stu-id="c3d54-158">JSON Schema</span></span> |<span data-ttu-id="c3d54-159">Schemat</span><span class="sxs-lookup"><span data-stu-id="c3d54-159">schema</span></span> |<span data-ttu-id="c3d54-160">JSON-schema av text för HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="c3d54-160">The JSON schema of the HTTP request body</span></span> |

<br>

<span data-ttu-id="c3d54-161">**Information för utdata**</span><span class="sxs-lookup"><span data-stu-id="c3d54-161">**Output details**</span></span>

<span data-ttu-id="c3d54-162">Nedan visas information för utdata för begäran.</span><span class="sxs-lookup"><span data-stu-id="c3d54-162">The following are output details for the request.</span></span>

| <span data-ttu-id="c3d54-163">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="c3d54-163">Property name</span></span> | <span data-ttu-id="c3d54-164">Datatyp</span><span class="sxs-lookup"><span data-stu-id="c3d54-164">Data type</span></span> | <span data-ttu-id="c3d54-165">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c3d54-165">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c3d54-166">Rubriker</span><span class="sxs-lookup"><span data-stu-id="c3d54-166">Headers</span></span> |<span data-ttu-id="c3d54-167">Objektet</span><span class="sxs-lookup"><span data-stu-id="c3d54-167">object</span></span> |<span data-ttu-id="c3d54-168">Huvuden för begäran</span><span class="sxs-lookup"><span data-stu-id="c3d54-168">Request headers</span></span> |
| <span data-ttu-id="c3d54-169">Innehåll</span><span class="sxs-lookup"><span data-stu-id="c3d54-169">Body</span></span> |<span data-ttu-id="c3d54-170">Objektet</span><span class="sxs-lookup"><span data-stu-id="c3d54-170">object</span></span> |<span data-ttu-id="c3d54-171">Request-objektet</span><span class="sxs-lookup"><span data-stu-id="c3d54-171">Request object</span></span> |

#### <a name="response-action"></a><span data-ttu-id="c3d54-172">Svaret åtgärd</span><span class="sxs-lookup"><span data-stu-id="c3d54-172">Response action</span></span>
<span data-ttu-id="c3d54-173">Följande är inmatningsfält för åtgärden HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="c3d54-173">The following are input fields for the HTTP Response action.</span></span> <span data-ttu-id="c3d54-174">A * innebär att det är ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="c3d54-174">A * means that it is a required field.</span></span>

| <span data-ttu-id="c3d54-175">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="c3d54-175">Display name</span></span> | <span data-ttu-id="c3d54-176">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="c3d54-176">Property name</span></span> | <span data-ttu-id="c3d54-177">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c3d54-177">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c3d54-178">Status kod *</span><span class="sxs-lookup"><span data-stu-id="c3d54-178">Status Code*</span></span> |<span data-ttu-id="c3d54-179">statusCode</span><span class="sxs-lookup"><span data-stu-id="c3d54-179">statusCode</span></span> |<span data-ttu-id="c3d54-180">HTTP-statuskod</span><span class="sxs-lookup"><span data-stu-id="c3d54-180">The HTTP status code</span></span> |
| <span data-ttu-id="c3d54-181">Rubriker</span><span class="sxs-lookup"><span data-stu-id="c3d54-181">Headers</span></span> |<span data-ttu-id="c3d54-182">Rubriker</span><span class="sxs-lookup"><span data-stu-id="c3d54-182">headers</span></span> |<span data-ttu-id="c3d54-183">Ett JSON-objekt för alla svarshuvuden att inkludera</span><span class="sxs-lookup"><span data-stu-id="c3d54-183">A JSON object of any response headers to include</span></span> |
| <span data-ttu-id="c3d54-184">Innehåll</span><span class="sxs-lookup"><span data-stu-id="c3d54-184">Body</span></span> |<span data-ttu-id="c3d54-185">Brödtext</span><span class="sxs-lookup"><span data-stu-id="c3d54-185">body</span></span> |<span data-ttu-id="c3d54-186">Svarstexten</span><span class="sxs-lookup"><span data-stu-id="c3d54-186">The response body</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c3d54-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3d54-187">Next steps</span></span>
<span data-ttu-id="c3d54-188">Prova nu, plattform och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="c3d54-188">Now, try out the platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="c3d54-189">Du kan utforska andra tillgängliga kopplingar i logikappar genom att titta på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="c3d54-189">You can explore the other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

