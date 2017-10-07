---
title: "åtgärder för aaaUse förfrågan och svar | Microsoft Docs"
description: "Översikt över hello förfrågan och svar trigger och action i en Azure logikapp"
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
ms.openlocfilehash: 24c378cc12d5f3f65116d5e59278236186a99662
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-request-and-response-components"></a><span data-ttu-id="9e439-103">Kom igång med hello komponenter för förfrågan och svar</span><span class="sxs-lookup"><span data-stu-id="9e439-103">Get started with hello request and response components</span></span>
<span data-ttu-id="9e439-104">Med hello förfrågan och svar komponenter i en logikapp, kan du svara i realtid tooevents.</span><span class="sxs-lookup"><span data-stu-id="9e439-104">With hello request and response components in a logic app, you can respond in real time tooevents.</span></span>

<span data-ttu-id="9e439-105">Du kan till exempel:</span><span class="sxs-lookup"><span data-stu-id="9e439-105">For example, you can:</span></span>

* <span data-ttu-id="9e439-106">Svara tooan HTTP-begäran med data från en lokal databas via en logikapp.</span><span class="sxs-lookup"><span data-stu-id="9e439-106">Respond tooan HTTP request with data from an on-premises database through a logic app.</span></span>
* <span data-ttu-id="9e439-107">Utlös en logikapp från en extern webhook-händelse.</span><span class="sxs-lookup"><span data-stu-id="9e439-107">Trigger a logic app from an external webhook event.</span></span>
* <span data-ttu-id="9e439-108">Anropa en logikapp med en åtgärd för förfrågan och svar från inom en annan logikapp.</span><span class="sxs-lookup"><span data-stu-id="9e439-108">Call a logic app with a request and response action from within another logic app.</span></span>

<span data-ttu-id="9e439-109">tooget igång med hello förfrågan och svar åtgärder i en logikapp finns [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="9e439-109">tooget started using hello request and response actions in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-hello-http-request-trigger"></a><span data-ttu-id="9e439-110">Använd hello utlösare för HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="9e439-110">Use hello HTTP Request trigger</span></span>
<span data-ttu-id="9e439-111">En utlösare är en händelse som kan använda toostart hello arbetsflöde som har definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="9e439-111">A trigger is an event that can be used toostart hello workflow that is defined in a logic app.</span></span> <span data-ttu-id="9e439-112">[Mer information om utlösare](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9e439-112">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="9e439-113">Här är en Exempelsekvens av hur tooset in HTTP begäran i hello logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="9e439-113">Here’s an example sequence of how tooset up an HTTP request in hello Logic App Designer.</span></span>

1. <span data-ttu-id="9e439-114">Lägga till hello utlösare **begäran - när en HTTP-begäran tas emot** i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="9e439-114">Add hello trigger **Request - When an HTTP request is received** in your logic app.</span></span> <span data-ttu-id="9e439-115">Du kan också medföra en JSON-schema (med hjälp av ett verktyg som [JSONSchema.net](http://jsonschema.net)) för hello frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="9e439-115">You can optionally provide a JSON schema (by using a tool like [JSONSchema.net](http://jsonschema.net)) for hello request body.</span></span> <span data-ttu-id="9e439-116">På så sätt kan hello designer toogenerate token för egenskaper i hello HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="9e439-116">This allows hello designer toogenerate tokens for properties in hello HTTP request.</span></span>
2. <span data-ttu-id="9e439-117">Lägg till en annan åtgärd så att du kan spara hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="9e439-117">Add another action so that you can save hello logic app.</span></span>
3. <span data-ttu-id="9e439-118">Du kan hämta hello HTTP URL-begäran från hello begäran kort när du har sparat hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="9e439-118">After saving hello logic app, you can get hello HTTP request URL from hello request card.</span></span>
4. <span data-ttu-id="9e439-119">En HTTP POST (du kan använda ett verktyg som [Postman](https://www.getpostman.com/)) toohello URL utlösare hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="9e439-119">An HTTP POST (you can use a tool like [Postman](https://www.getpostman.com/)) toohello URL triggers hello logic app.</span></span>

> [!NOTE]
> <span data-ttu-id="9e439-120">Om du inte definierar en åtgärd för svar, en `202 ACCEPTED` svar returneras omedelbart toohello anroparen.</span><span class="sxs-lookup"><span data-stu-id="9e439-120">If you don't define a response action, a `202 ACCEPTED` response is immediately returned toohello caller.</span></span> <span data-ttu-id="9e439-121">Du kan använda hello svar åtgärden toocustomize ett svar.</span><span class="sxs-lookup"><span data-stu-id="9e439-121">You can use hello response action toocustomize a response.</span></span>
> 
> 

![Svaret utlösare](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-hello-http-response-action"></a><span data-ttu-id="9e439-123">Använd hello åtgärd för HTTP-svar</span><span class="sxs-lookup"><span data-stu-id="9e439-123">Use hello HTTP Response action</span></span>
<span data-ttu-id="9e439-124">hello HTTP-svar som åtgärden är endast giltig när du använder den i ett arbetsflöde som utlöses av en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="9e439-124">hello HTTP Response action is only valid when you use it in a workflow that is triggered by an HTTP request.</span></span> <span data-ttu-id="9e439-125">Om du inte definierar en åtgärd för svar, en `202 ACCEPTED` svar returneras omedelbart toohello anroparen.</span><span class="sxs-lookup"><span data-stu-id="9e439-125">If you don't define a response action, a `202 ACCEPTED` response is immediately returned toohello caller.</span></span>  <span data-ttu-id="9e439-126">Du kan lägga till en åtgärd som svar på något steg i hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="9e439-126">You can add a response action at any step within hello workflow.</span></span> <span data-ttu-id="9e439-127">Hej logikapp endast håller hello inkommande begäran öppen för en minut för svar.</span><span class="sxs-lookup"><span data-stu-id="9e439-127">hello logic app only keeps hello incoming request open for one minute for a response.</span></span>  <span data-ttu-id="9e439-128">Efter en minut, om inget svar skickades från hello arbetsflöde (och det finns ett svar-åtgärd i hello definition), en `504 GATEWAY TIMEOUT` returneras toohello anroparen.</span><span class="sxs-lookup"><span data-stu-id="9e439-128">After one minute, if no response was sent from hello workflow (and a response action exists in hello definition), a `504 GATEWAY TIMEOUT` is returned toohello caller.</span></span>

<span data-ttu-id="9e439-129">Här är hur tooadd åtgärden HTTP-svar:</span><span class="sxs-lookup"><span data-stu-id="9e439-129">Here's how tooadd an HTTP Response action:</span></span>

1. <span data-ttu-id="9e439-130">Välj hello **nytt steg** knappen.</span><span class="sxs-lookup"><span data-stu-id="9e439-130">Select hello **New Step** button.</span></span>
2. <span data-ttu-id="9e439-131">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="9e439-131">Choose **Add an action**.</span></span>
3. <span data-ttu-id="9e439-132">Skriv i sökrutan för hello åtgärd **svar** toolist hello svar åtgärd.</span><span class="sxs-lookup"><span data-stu-id="9e439-132">In hello action search box, type **response** toolist hello Response action.</span></span>
   
    ![Välj hello svar åtgärd](./media/connectors-native-reqres/using-action-1.png)
4. <span data-ttu-id="9e439-134">Lägga till i alla parametrar som krävs för hello HTTP-svarsmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="9e439-134">Add in any parameters that are required for hello HTTP response message.</span></span>
   
    ![Fullständig hello svar åtgärd](./media/connectors-native-reqres/using-action-2.png)
5. <span data-ttu-id="9e439-136">Klicka på hello övre vänstra hörnet av hello verktygsfältet toosave och din logik app kommer båda att spara och publicera (aktivera).</span><span class="sxs-lookup"><span data-stu-id="9e439-136">Click hello upper-left corner of hello toolbar toosave, and your logic app will both save and publish (activate).</span></span>

## <a name="request-trigger"></a><span data-ttu-id="9e439-137">Begäran utlösare</span><span class="sxs-lookup"><span data-stu-id="9e439-137">Request trigger</span></span>
<span data-ttu-id="9e439-138">Här följer hello information för hello utlösare som har stöd för den här anslutningen.</span><span class="sxs-lookup"><span data-stu-id="9e439-138">Here are hello details for hello trigger that this connector supports.</span></span> <span data-ttu-id="9e439-139">Det finns en enskild begäran-utlösare.</span><span class="sxs-lookup"><span data-stu-id="9e439-139">There is a single request trigger.</span></span>

| <span data-ttu-id="9e439-140">Utlösare</span><span class="sxs-lookup"><span data-stu-id="9e439-140">Trigger</span></span> | <span data-ttu-id="9e439-141">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9e439-141">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9e439-142">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="9e439-142">Request</span></span> |<span data-ttu-id="9e439-143">Inträffar när en HTTP-begäran tas emot</span><span class="sxs-lookup"><span data-stu-id="9e439-143">Occurs when an HTTP request is received</span></span> |

## <a name="response-action"></a><span data-ttu-id="9e439-144">Svaret åtgärd</span><span class="sxs-lookup"><span data-stu-id="9e439-144">Response action</span></span>
<span data-ttu-id="9e439-145">Här följer hello information för hello-åtgärd som har stöd för den här anslutningen.</span><span class="sxs-lookup"><span data-stu-id="9e439-145">Here are hello details for hello action that this connector supports.</span></span> <span data-ttu-id="9e439-146">Det finns ett enda svar-åtgärd som kan endast användas när den åtföljs av en begäran-utlösare.</span><span class="sxs-lookup"><span data-stu-id="9e439-146">There is a single response action that can only be used when it is accompanied by a request trigger.</span></span>

| <span data-ttu-id="9e439-147">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="9e439-147">Action</span></span> | <span data-ttu-id="9e439-148">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9e439-148">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9e439-149">Svar</span><span class="sxs-lookup"><span data-stu-id="9e439-149">Response</span></span> |<span data-ttu-id="9e439-150">Returnerar ett svar toohello korrelerade HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="9e439-150">Returns a response toohello correlated HTTP request</span></span> |

### <a name="trigger-and-action-details"></a><span data-ttu-id="9e439-151">Trigger och action information</span><span class="sxs-lookup"><span data-stu-id="9e439-151">Trigger and action details</span></span>
<span data-ttu-id="9e439-152">hello följande tabeller beskriver hello inmatningsfält för hello trigger och action och hello detaljer om motsvarande utdata.</span><span class="sxs-lookup"><span data-stu-id="9e439-152">hello following tables describe hello input fields for hello trigger and action, and hello corresponding output details.</span></span>

#### <a name="request-trigger"></a><span data-ttu-id="9e439-153">Begäran utlösare</span><span class="sxs-lookup"><span data-stu-id="9e439-153">Request trigger</span></span>
<span data-ttu-id="9e439-154">hello följande är ett inmatningsfält för hello utlösare från en inkommande HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="9e439-154">hello following is an input field for hello trigger from an incoming HTTP request.</span></span>

| <span data-ttu-id="9e439-155">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="9e439-155">Display name</span></span> | <span data-ttu-id="9e439-156">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="9e439-156">Property name</span></span> | <span data-ttu-id="9e439-157">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9e439-157">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9e439-158">JSON-Schema</span><span class="sxs-lookup"><span data-stu-id="9e439-158">JSON Schema</span></span> |<span data-ttu-id="9e439-159">Schemat</span><span class="sxs-lookup"><span data-stu-id="9e439-159">schema</span></span> |<span data-ttu-id="9e439-160">hello JSON-schema av text för hello HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="9e439-160">hello JSON schema of hello HTTP request body</span></span> |

<br>

<span data-ttu-id="9e439-161">**Information för utdata**</span><span class="sxs-lookup"><span data-stu-id="9e439-161">**Output details**</span></span>

<span data-ttu-id="9e439-162">hello nedan visas utdata information för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="9e439-162">hello following are output details for hello request.</span></span>

| <span data-ttu-id="9e439-163">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="9e439-163">Property name</span></span> | <span data-ttu-id="9e439-164">Datatyp</span><span class="sxs-lookup"><span data-stu-id="9e439-164">Data type</span></span> | <span data-ttu-id="9e439-165">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9e439-165">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9e439-166">Rubriker</span><span class="sxs-lookup"><span data-stu-id="9e439-166">Headers</span></span> |<span data-ttu-id="9e439-167">Objektet</span><span class="sxs-lookup"><span data-stu-id="9e439-167">object</span></span> |<span data-ttu-id="9e439-168">Huvuden för begäran</span><span class="sxs-lookup"><span data-stu-id="9e439-168">Request headers</span></span> |
| <span data-ttu-id="9e439-169">Innehåll</span><span class="sxs-lookup"><span data-stu-id="9e439-169">Body</span></span> |<span data-ttu-id="9e439-170">Objektet</span><span class="sxs-lookup"><span data-stu-id="9e439-170">object</span></span> |<span data-ttu-id="9e439-171">Request-objektet</span><span class="sxs-lookup"><span data-stu-id="9e439-171">Request object</span></span> |

#### <a name="response-action"></a><span data-ttu-id="9e439-172">Svaret åtgärd</span><span class="sxs-lookup"><span data-stu-id="9e439-172">Response action</span></span>
<span data-ttu-id="9e439-173">hello följande är inmatningsfält för hello åtgärd för HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="9e439-173">hello following are input fields for hello HTTP Response action.</span></span> <span data-ttu-id="9e439-174">A * innebär att det är ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="9e439-174">A * means that it is a required field.</span></span>

| <span data-ttu-id="9e439-175">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="9e439-175">Display name</span></span> | <span data-ttu-id="9e439-176">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="9e439-176">Property name</span></span> | <span data-ttu-id="9e439-177">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9e439-177">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9e439-178">Status kod *</span><span class="sxs-lookup"><span data-stu-id="9e439-178">Status Code*</span></span> |<span data-ttu-id="9e439-179">statusCode</span><span class="sxs-lookup"><span data-stu-id="9e439-179">statusCode</span></span> |<span data-ttu-id="9e439-180">hello HTTP-statuskod</span><span class="sxs-lookup"><span data-stu-id="9e439-180">hello HTTP status code</span></span> |
| <span data-ttu-id="9e439-181">Rubriker</span><span class="sxs-lookup"><span data-stu-id="9e439-181">Headers</span></span> |<span data-ttu-id="9e439-182">Rubriker</span><span class="sxs-lookup"><span data-stu-id="9e439-182">headers</span></span> |<span data-ttu-id="9e439-183">Alla svar huvuden tooinclude ett JSON-objekt</span><span class="sxs-lookup"><span data-stu-id="9e439-183">A JSON object of any response headers tooinclude</span></span> |
| <span data-ttu-id="9e439-184">Innehåll</span><span class="sxs-lookup"><span data-stu-id="9e439-184">Body</span></span> |<span data-ttu-id="9e439-185">Brödtext</span><span class="sxs-lookup"><span data-stu-id="9e439-185">body</span></span> |<span data-ttu-id="9e439-186">hello svarstexten</span><span class="sxs-lookup"><span data-stu-id="9e439-186">hello response body</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9e439-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9e439-187">Next steps</span></span>
<span data-ttu-id="9e439-188">Prova nu hello plattform och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="9e439-188">Now, try out hello platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="9e439-189">Du kan utforska hello andra tillgängliga kopplingar i logikappar genom att titta på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="9e439-189">You can explore hello other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

