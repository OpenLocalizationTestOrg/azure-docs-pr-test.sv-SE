---
title: "aaaCommunicate med valfri slutpunkt över HTTP - Azure Logic Apps | Microsoft Docs"
description: "Skapa logikappar som kan kommunicera med valfri slutpunkt över HTTP"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: e11c6b4d-65a5-4d2d-8e13-38150db09c0b
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: 9793601839437a2b880bdb81e15881270cacc963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-http-action"></a><span data-ttu-id="b4f75-103">Kom igång med hello HTTP-åtgärd</span><span class="sxs-lookup"><span data-stu-id="b4f75-103">Get started with hello HTTP action</span></span>

<span data-ttu-id="b4f75-104">Med hello HTTP-åtgärd, kan du utöka arbetsflöden för din organisation och kommunicera tooany endpoint via HTTP.</span><span class="sxs-lookup"><span data-stu-id="b4f75-104">With hello HTTP action, you can extend workflows for your organization and communicate tooany endpoint over HTTP.</span></span>

<span data-ttu-id="b4f75-105">Du kan:</span><span class="sxs-lookup"><span data-stu-id="b4f75-105">You can:</span></span>

* <span data-ttu-id="b4f75-106">Skapa logik app arbetsflöden som aktiverar (utlösare) när en webbplats som du hanterar kraschar.</span><span class="sxs-lookup"><span data-stu-id="b4f75-106">Create logic app workflows that activate (trigger) when a website that you manage goes down.</span></span>
* <span data-ttu-id="b4f75-107">Kommunicera tooany endpoint via HTTP tooextend dina arbetsflöden till andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="b4f75-107">Communicate tooany endpoint over HTTP tooextend your workflows into other services.</span></span>

<span data-ttu-id="b4f75-108">tooget igång med hello HTTP-åtgärden i en logikapp finns [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="b4f75-108">tooget started using hello HTTP action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-hello-http-trigger"></a><span data-ttu-id="b4f75-109">Använda hello HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="b4f75-109">Use hello HTTP trigger</span></span>
<span data-ttu-id="b4f75-110">En utlösare är en händelse som kan använda toostart hello arbetsflöde som har definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="b4f75-110">A trigger is an event that can be used toostart hello workflow that is defined in a logic app.</span></span> <span data-ttu-id="b4f75-111">[Mer information om utlösare](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b4f75-111">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="b4f75-112">Här är en Exempelsekvens av hur tooset in hello HTTP utlösare i hello logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="b4f75-112">Here’s an example sequence of how tooset up hello HTTP trigger in hello Logic App Designer.</span></span>

1. <span data-ttu-id="b4f75-113">Lägg till hello HTTP-utlösaren i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="b4f75-113">Add hello HTTP trigger in your logic app.</span></span>
2. <span data-ttu-id="b4f75-114">Fyll i hello parametrar för hello HTTP-slutpunkt som du vill toopoll.</span><span class="sxs-lookup"><span data-stu-id="b4f75-114">Fill in hello parameters for hello HTTP endpoint that you want toopoll.</span></span>
3. <span data-ttu-id="b4f75-115">Ändra hello upprepningsintervallet på hur ofta den ska avsöka.</span><span class="sxs-lookup"><span data-stu-id="b4f75-115">Modify hello recurrence interval on how frequently it should poll.</span></span>

   <span data-ttu-id="b4f75-116">Hej logikapp utlöses nu med allt innehåll som returneras vid varje kontroll.</span><span class="sxs-lookup"><span data-stu-id="b4f75-116">hello logic app now fires with any content that is returned during each check.</span></span>

   ![HTTP-utlösare](./media/connectors-native-http/using-trigger.png)

### <a name="how-hello-http-trigger-works"></a><span data-ttu-id="b4f75-118">Så här fungerar hello http-utlösare</span><span class="sxs-lookup"><span data-stu-id="b4f75-118">How hello HTTP trigger works</span></span>

<span data-ttu-id="b4f75-119">hello HTTP-utlösaren skickar en anropet tooHTTP slutpunkt med återkommande intervall.</span><span class="sxs-lookup"><span data-stu-id="b4f75-119">hello HTTP trigger sends a call tooHTTP endpoint on a recurring interval.</span></span> <span data-ttu-id="b4f75-120">Som standard gör HTTP-svarskoden som är lägre än 300 en logik app toorun.</span><span class="sxs-lookup"><span data-stu-id="b4f75-120">By default, any HTTP response code that is lower than 300 causes a logic app toorun.</span></span> <span data-ttu-id="b4f75-121">toospecify om hello logikapp ska utlösas du kan redigera hello logikapp i kodvy och Lägg till ett villkor som utvärderar efter hello http-anrop.</span><span class="sxs-lookup"><span data-stu-id="b4f75-121">toospecify whether hello logic app should fire, you can edit hello logic app in code view, and add a condition that evaluates after hello HTTP call.</span></span> <span data-ttu-id="b4f75-122">Här är ett exempel på en HTTP-utlösare som utlöses när hello returnerade statuskoden är större än eller lika med för`400`.</span><span class="sxs-lookup"><span data-stu-id="b4f75-122">Here's an example of an HTTP trigger that fires when hello returned status code is greater than or equal too`400`.</span></span>

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

<span data-ttu-id="b4f75-123">Fullständig information om parametrar för hello HTTP-utlösaren finns på [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).</span><span class="sxs-lookup"><span data-stu-id="b4f75-123">Full details about hello HTTP trigger parameters are available on [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).</span></span>

## <a name="use-hello-http-action"></a><span data-ttu-id="b4f75-124">Använd hello HTTP-åtgärd</span><span class="sxs-lookup"><span data-stu-id="b4f75-124">Use hello HTTP action</span></span>

<span data-ttu-id="b4f75-125">En åtgärd är en åtgärd som utförs av hello arbetsflöde som har definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="b4f75-125">An action is an operation that is carried out by hello workflow that is defined in a logic app.</span></span> 
<span data-ttu-id="b4f75-126">[Mer information om åtgärder](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b4f75-126">[Learn more about actions](connectors-overview.md).</span></span>

1. <span data-ttu-id="b4f75-127">Välj **nytt steg** > **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="b4f75-127">Choose **New Step** > **Add an action**.</span></span>
3. <span data-ttu-id="b4f75-128">Skriv i sökrutan för hello åtgärd **http** toolist hello HTTP-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b4f75-128">In hello action search box, type **http** toolist hello HTTP actions.</span></span>
   
    ![Välj hello HTTP-åtgärd](./media/connectors-native-http/using-action-1.png)

4. <span data-ttu-id="b4f75-130">Lägg till eventuella parametrar för hello HTTP-anrop.</span><span class="sxs-lookup"><span data-stu-id="b4f75-130">Add any required parameters for hello HTTP call.</span></span>
   
    ![Fullständig hello HTTP-åtgärd](./media/connectors-native-http/using-action-2.png)

5. <span data-ttu-id="b4f75-132">På hello designer verktygsfältet **spara**.</span><span class="sxs-lookup"><span data-stu-id="b4f75-132">On hello designer toolbar, click **Save**.</span></span> <span data-ttu-id="b4f75-133">Din logikapp sparas och publiceras (aktiverad) på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="b4f75-133">Your logic app is saved and published (activated) at hello same time.</span></span>

## <a name="http-trigger"></a><span data-ttu-id="b4f75-134">HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="b4f75-134">HTTP trigger</span></span>
<span data-ttu-id="b4f75-135">Här följer hello information för hello utlösare som har stöd för den här anslutningen.</span><span class="sxs-lookup"><span data-stu-id="b4f75-135">Here are hello details for hello trigger that this connector supports.</span></span> <span data-ttu-id="b4f75-136">hello HTTP-anslutningen har en utlösare.</span><span class="sxs-lookup"><span data-stu-id="b4f75-136">hello HTTP connector has one trigger.</span></span>

| <span data-ttu-id="b4f75-137">Utlösare</span><span class="sxs-lookup"><span data-stu-id="b4f75-137">Trigger</span></span> | <span data-ttu-id="b4f75-138">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b4f75-138">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b4f75-139">HTTP</span><span class="sxs-lookup"><span data-stu-id="b4f75-139">HTTP</span></span> |<span data-ttu-id="b4f75-140">Gör ett HTTP-anrop och returnerar hello svar innehåll.</span><span class="sxs-lookup"><span data-stu-id="b4f75-140">Makes an HTTP call and returns hello response content.</span></span> |

## <a name="http-action"></a><span data-ttu-id="b4f75-141">HTTP-åtgärd</span><span class="sxs-lookup"><span data-stu-id="b4f75-141">HTTP action</span></span>
<span data-ttu-id="b4f75-142">Här följer hello information för hello-åtgärd som har stöd för den här anslutningen.</span><span class="sxs-lookup"><span data-stu-id="b4f75-142">Here are hello details for hello action that this connector supports.</span></span> <span data-ttu-id="b4f75-143">hello HTTP-anslutningen har en möjlig åtgärd.</span><span class="sxs-lookup"><span data-stu-id="b4f75-143">hello HTTP connector has one possible action.</span></span>

| <span data-ttu-id="b4f75-144">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="b4f75-144">Action</span></span> | <span data-ttu-id="b4f75-145">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b4f75-145">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b4f75-146">HTTP</span><span class="sxs-lookup"><span data-stu-id="b4f75-146">HTTP</span></span> |<span data-ttu-id="b4f75-147">Gör ett HTTP-anrop och returnerar hello svar innehåll.</span><span class="sxs-lookup"><span data-stu-id="b4f75-147">Makes an HTTP call and returns hello response content.</span></span> |

## <a name="http-details"></a><span data-ttu-id="b4f75-148">HTTP-information</span><span class="sxs-lookup"><span data-stu-id="b4f75-148">HTTP details</span></span>
<span data-ttu-id="b4f75-149">hello följande tabeller beskrivs hello nödvändiga och valfria indatafält för hello åtgärd och hello motsvarande utdata information som är associerade med åtgärden hello.</span><span class="sxs-lookup"><span data-stu-id="b4f75-149">hello following tables describe hello required and optional input fields for hello action and hello corresponding output details that are associated with using hello action.</span></span>

#### <a name="http-request"></a><span data-ttu-id="b4f75-150">HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="b4f75-150">HTTP request</span></span>
<span data-ttu-id="b4f75-151">hello följande är inmatningsfält för hello åtgärden, vilket gör en utgående HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="b4f75-151">hello following are input fields for hello action, which makes an HTTP outbound request.</span></span>
<span data-ttu-id="b4f75-152">A * innebär att det är ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="b4f75-152">A * means that it is a required field.</span></span>

| <span data-ttu-id="b4f75-153">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="b4f75-153">Display name</span></span> | <span data-ttu-id="b4f75-154">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="b4f75-154">Property name</span></span> | <span data-ttu-id="b4f75-155">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b4f75-155">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4f75-156">Metoden *</span><span class="sxs-lookup"><span data-stu-id="b4f75-156">Method*</span></span> |<span data-ttu-id="b4f75-157">Metoden</span><span class="sxs-lookup"><span data-stu-id="b4f75-157">method</span></span> |<span data-ttu-id="b4f75-158">hello HTTP-verbet toouse</span><span class="sxs-lookup"><span data-stu-id="b4f75-158">hello HTTP verb toouse</span></span> |
| <span data-ttu-id="b4f75-159">URI *</span><span class="sxs-lookup"><span data-stu-id="b4f75-159">URI*</span></span> |<span data-ttu-id="b4f75-160">URI: N</span><span class="sxs-lookup"><span data-stu-id="b4f75-160">uri</span></span> |<span data-ttu-id="b4f75-161">hello URI för hello HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="b4f75-161">hello URI for hello HTTP request</span></span> |
| <span data-ttu-id="b4f75-162">Rubriker</span><span class="sxs-lookup"><span data-stu-id="b4f75-162">Headers</span></span> |<span data-ttu-id="b4f75-163">Rubriker</span><span class="sxs-lookup"><span data-stu-id="b4f75-163">headers</span></span> |<span data-ttu-id="b4f75-164">Ett JSON-objekt i HTTP-huvuden tooinclude</span><span class="sxs-lookup"><span data-stu-id="b4f75-164">A JSON object of HTTP headers tooinclude</span></span> |
| <span data-ttu-id="b4f75-165">Innehåll</span><span class="sxs-lookup"><span data-stu-id="b4f75-165">Body</span></span> |<span data-ttu-id="b4f75-166">Brödtext</span><span class="sxs-lookup"><span data-stu-id="b4f75-166">body</span></span> |<span data-ttu-id="b4f75-167">hello text för HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="b4f75-167">hello HTTP request body</span></span> |
| <span data-ttu-id="b4f75-168">Autentisering</span><span class="sxs-lookup"><span data-stu-id="b4f75-168">Authentication</span></span> |<span data-ttu-id="b4f75-169">Autentisering</span><span class="sxs-lookup"><span data-stu-id="b4f75-169">authentication</span></span> |<span data-ttu-id="b4f75-170">Mer information i hello [autentisering](#authentication) avsnitt</span><span class="sxs-lookup"><span data-stu-id="b4f75-170">Details in hello [Authentication](#authentication) section</span></span> |

<br>

#### <a name="output-details"></a><span data-ttu-id="b4f75-171">Information för utdata</span><span class="sxs-lookup"><span data-stu-id="b4f75-171">Output details</span></span>
<span data-ttu-id="b4f75-172">hello nedan visas utdata information för hello HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="b4f75-172">hello following are output details for hello HTTP response.</span></span>

| <span data-ttu-id="b4f75-173">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="b4f75-173">Property name</span></span> | <span data-ttu-id="b4f75-174">Datatyp</span><span class="sxs-lookup"><span data-stu-id="b4f75-174">Data type</span></span> | <span data-ttu-id="b4f75-175">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b4f75-175">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4f75-176">Rubriker</span><span class="sxs-lookup"><span data-stu-id="b4f75-176">Headers</span></span> |<span data-ttu-id="b4f75-177">Objektet</span><span class="sxs-lookup"><span data-stu-id="b4f75-177">object</span></span> |<span data-ttu-id="b4f75-178">Svarsrubriker</span><span class="sxs-lookup"><span data-stu-id="b4f75-178">Response headers</span></span> |
| <span data-ttu-id="b4f75-179">Innehåll</span><span class="sxs-lookup"><span data-stu-id="b4f75-179">Body</span></span> |<span data-ttu-id="b4f75-180">Objektet</span><span class="sxs-lookup"><span data-stu-id="b4f75-180">object</span></span> |<span data-ttu-id="b4f75-181">Objektet Response</span><span class="sxs-lookup"><span data-stu-id="b4f75-181">Response object</span></span> |
| <span data-ttu-id="b4f75-182">Statuskod</span><span class="sxs-lookup"><span data-stu-id="b4f75-182">Status Code</span></span> |<span data-ttu-id="b4f75-183">int</span><span class="sxs-lookup"><span data-stu-id="b4f75-183">int</span></span> |<span data-ttu-id="b4f75-184">HTTP-statuskod</span><span class="sxs-lookup"><span data-stu-id="b4f75-184">HTTP status code</span></span> |

## <a name="authentication"></a><span data-ttu-id="b4f75-185">Autentisering</span><span class="sxs-lookup"><span data-stu-id="b4f75-185">Authentication</span></span>
<span data-ttu-id="b4f75-186">hello Logic Apps-funktionen kan du toouse olika typer av autentisering mot HTTP-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="b4f75-186">hello Logic Apps feature allows you toouse different types of authentication against HTTP endpoints.</span></span> <span data-ttu-id="b4f75-187">Du kan använda den här autentiseringen med hello **HTTP**,  **[HTTP + Swagger](connectors-native-http-swagger.md)**, och  **[HTTP Webhook](connectors-native-webhook.md)**  kopplingar.</span><span class="sxs-lookup"><span data-stu-id="b4f75-187">You can use this authentication with hello **HTTP**, **[HTTP + Swagger](connectors-native-http-swagger.md)**, and **[HTTP Webhook](connectors-native-webhook.md)** connectors.</span></span> <span data-ttu-id="b4f75-188">hello följande typer av autentisering kan konfigureras:</span><span class="sxs-lookup"><span data-stu-id="b4f75-188">hello following types of authentication are configurable:</span></span>

* [<span data-ttu-id="b4f75-189">Grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="b4f75-189">Basic authentication</span></span>](#basic-authentication)
* [<span data-ttu-id="b4f75-190">Autentisering av klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="b4f75-190">Client certificate authentication</span></span>](#client-certificate-authentication)
* [<span data-ttu-id="b4f75-191">Azure Active Directory (AD Azure) OAuth-autentisering</span><span class="sxs-lookup"><span data-stu-id="b4f75-191">Azure Active Directory (Azure AD) OAuth authentication</span></span>](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a><span data-ttu-id="b4f75-192">Grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="b4f75-192">Basic authentication</span></span>

<span data-ttu-id="b4f75-193">hello efter autentiseringsobjekt krävs för grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="b4f75-193">hello following authentication object is needed for basic authentication.</span></span>
<span data-ttu-id="b4f75-194">A * innebär att det är ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="b4f75-194">A * means that it is a required field.</span></span>

| <span data-ttu-id="b4f75-195">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="b4f75-195">Property name</span></span> | <span data-ttu-id="b4f75-196">Datatyp</span><span class="sxs-lookup"><span data-stu-id="b4f75-196">Data type</span></span> | <span data-ttu-id="b4f75-197">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b4f75-197">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4f75-198">Typen *</span><span class="sxs-lookup"><span data-stu-id="b4f75-198">Type*</span></span> |<span data-ttu-id="b4f75-199">typ</span><span class="sxs-lookup"><span data-stu-id="b4f75-199">type</span></span> |<span data-ttu-id="b4f75-200">Typ av autentisering (måste vara `Basic` för grundläggande autentisering)</span><span class="sxs-lookup"><span data-stu-id="b4f75-200">Type of authentication (must be `Basic` for basic authentication)</span></span> |
| <span data-ttu-id="b4f75-201">Användarnamn *</span><span class="sxs-lookup"><span data-stu-id="b4f75-201">Username*</span></span> |<span data-ttu-id="b4f75-202">användarnamn</span><span class="sxs-lookup"><span data-stu-id="b4f75-202">username</span></span> |<span data-ttu-id="b4f75-203">Användaren namnet tooauthenticate</span><span class="sxs-lookup"><span data-stu-id="b4f75-203">User name tooauthenticate</span></span> |
| <span data-ttu-id="b4f75-204">Lösenord *</span><span class="sxs-lookup"><span data-stu-id="b4f75-204">Password*</span></span> |<span data-ttu-id="b4f75-205">lösenord</span><span class="sxs-lookup"><span data-stu-id="b4f75-205">password</span></span> |<span data-ttu-id="b4f75-206">Lösenord tooauthenticate</span><span class="sxs-lookup"><span data-stu-id="b4f75-206">Password tooauthenticate</span></span> |

> [!TIP]
> <span data-ttu-id="b4f75-207">Om du vill toouse ett lösenord som inte kan hämtas från hello definition använder en `securestring` parametern och hello `@parameters()`  
>  [definition arbetsflödesfunktion](http://aka.ms/logicappdocs).</span><span class="sxs-lookup"><span data-stu-id="b4f75-207">If you want toouse a password that cannot be retrieved from hello definition, use a `securestring` parameter and hello `@parameters()` 
> [workflow definition function](http://aka.ms/logicappdocs).</span></span>

<span data-ttu-id="b4f75-208">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b4f75-208">For example:</span></span>

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a><span data-ttu-id="b4f75-209">Autentisering med klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="b4f75-209">Client certificate authentication</span></span>

<span data-ttu-id="b4f75-210">hello krävs följande autentiseringsobjekt för autentisering av klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="b4f75-210">hello following authentication object is needed for client certificate authentication.</span></span> <span data-ttu-id="b4f75-211">A * innebär att det är ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="b4f75-211">A * means that it is a required field.</span></span>

| <span data-ttu-id="b4f75-212">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="b4f75-212">Property name</span></span> | <span data-ttu-id="b4f75-213">Datatyp</span><span class="sxs-lookup"><span data-stu-id="b4f75-213">Data type</span></span> | <span data-ttu-id="b4f75-214">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b4f75-214">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4f75-215">Typen *</span><span class="sxs-lookup"><span data-stu-id="b4f75-215">Type*</span></span> |<span data-ttu-id="b4f75-216">typ</span><span class="sxs-lookup"><span data-stu-id="b4f75-216">type</span></span> |<span data-ttu-id="b4f75-217">Hej typ av autentisering (måste vara `ClientCertificate` för SSL-klientcertifikat)</span><span class="sxs-lookup"><span data-stu-id="b4f75-217">hello type of authentication (must be `ClientCertificate` for SSL client certificates)</span></span> |
| <span data-ttu-id="b4f75-218">PFX *</span><span class="sxs-lookup"><span data-stu-id="b4f75-218">PFX*</span></span> |<span data-ttu-id="b4f75-219">Pfx</span><span class="sxs-lookup"><span data-stu-id="b4f75-219">pfx</span></span> |<span data-ttu-id="b4f75-220">hello Base64-kodad innehållet i hello Personal Information Exchange (PFX)-fil</span><span class="sxs-lookup"><span data-stu-id="b4f75-220">hello Base64-encoded contents of hello Personal Information Exchange (PFX) file</span></span> |
| <span data-ttu-id="b4f75-221">Lösenord *</span><span class="sxs-lookup"><span data-stu-id="b4f75-221">Password*</span></span> |<span data-ttu-id="b4f75-222">lösenord</span><span class="sxs-lookup"><span data-stu-id="b4f75-222">password</span></span> |<span data-ttu-id="b4f75-223">hello lösenord tooaccess hello PFX-fil</span><span class="sxs-lookup"><span data-stu-id="b4f75-223">hello password tooaccess hello PFX file</span></span> |

> [!TIP]
> <span data-ttu-id="b4f75-224">toouse en parameter som inte är läsbara i hello definition när du har sparat hello logikappen som du kan använda en `securestring` parametern och hello `@parameters()`  
>  [definition arbetsflödesfunktion](http://aka.ms/logicappdocs).</span><span class="sxs-lookup"><span data-stu-id="b4f75-224">toouse a parameter that won't be readable in hello definition after saving hello logic app, you can use a `securestring` parameter and hello `@parameters()` 
> [workflow definition function](http://aka.ms/logicappdocs).</span></span>

<span data-ttu-id="b4f75-225">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b4f75-225">For example:</span></span>

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a><span data-ttu-id="b4f75-226">Azure AD OAuth-autentisering</span><span class="sxs-lookup"><span data-stu-id="b4f75-226">Azure AD OAuth authentication</span></span>
<span data-ttu-id="b4f75-227">hello efter autentiseringsobjekt krävs för Azure AD OAuth-autentisering.</span><span class="sxs-lookup"><span data-stu-id="b4f75-227">hello following authentication object is needed for Azure AD OAuth authentication.</span></span> <span data-ttu-id="b4f75-228">A * innebär att det är ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="b4f75-228">A * means that it is a required field.</span></span>

| <span data-ttu-id="b4f75-229">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="b4f75-229">Property name</span></span> | <span data-ttu-id="b4f75-230">Datatyp</span><span class="sxs-lookup"><span data-stu-id="b4f75-230">Data type</span></span> | <span data-ttu-id="b4f75-231">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b4f75-231">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4f75-232">Typen *</span><span class="sxs-lookup"><span data-stu-id="b4f75-232">Type*</span></span> |<span data-ttu-id="b4f75-233">typ</span><span class="sxs-lookup"><span data-stu-id="b4f75-233">type</span></span> |<span data-ttu-id="b4f75-234">Hej typ av autentisering (måste vara `ActiveDirectoryOAuth` för Azure AD OAuth)</span><span class="sxs-lookup"><span data-stu-id="b4f75-234">hello type of authentication (must be `ActiveDirectoryOAuth` for Azure AD OAuth)</span></span> |
| <span data-ttu-id="b4f75-235">Klient *</span><span class="sxs-lookup"><span data-stu-id="b4f75-235">Tenant*</span></span> |<span data-ttu-id="b4f75-236">Klient</span><span class="sxs-lookup"><span data-stu-id="b4f75-236">tenant</span></span> |<span data-ttu-id="b4f75-237">hello klient-ID för hello Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="b4f75-237">hello tenant identifier for hello Azure AD tenant</span></span> |
| <span data-ttu-id="b4f75-238">Målgruppen *</span><span class="sxs-lookup"><span data-stu-id="b4f75-238">Audience*</span></span> |<span data-ttu-id="b4f75-239">målgrupp</span><span class="sxs-lookup"><span data-stu-id="b4f75-239">audience</span></span> |<span data-ttu-id="b4f75-240">hello-resurs som du begär godkännande toouse.</span><span class="sxs-lookup"><span data-stu-id="b4f75-240">hello resource you are requesting authorization toouse.</span></span> <span data-ttu-id="b4f75-241">Exempel: `https://management.core.windows.net/`</span><span class="sxs-lookup"><span data-stu-id="b4f75-241">For example: `https://management.core.windows.net/`</span></span> |
| <span data-ttu-id="b4f75-242">Klient -ID *</span><span class="sxs-lookup"><span data-stu-id="b4f75-242">Client ID*</span></span> |<span data-ttu-id="b4f75-243">clientId</span><span class="sxs-lookup"><span data-stu-id="b4f75-243">clientId</span></span> |<span data-ttu-id="b4f75-244">Hej klient-ID för hello Azure AD-program</span><span class="sxs-lookup"><span data-stu-id="b4f75-244">hello client identifier for hello Azure AD application</span></span> |
| <span data-ttu-id="b4f75-245">Hemligt *</span><span class="sxs-lookup"><span data-stu-id="b4f75-245">Secret*</span></span> |<span data-ttu-id="b4f75-246">hemligt</span><span class="sxs-lookup"><span data-stu-id="b4f75-246">secret</span></span> |<span data-ttu-id="b4f75-247">hello hemligheten för hello-klient som begär hello-token</span><span class="sxs-lookup"><span data-stu-id="b4f75-247">hello secret of hello client that is requesting hello token</span></span> |

> [!TIP]
> <span data-ttu-id="b4f75-248">Du kan använda en `securestring` parametern och hello `@parameters()` [definition arbetsflödesfunktion](http://aka.ms/logicappdocs) toouse en parameter som inte är läsbara i hello definition när du har sparat.</span><span class="sxs-lookup"><span data-stu-id="b4f75-248">You can use a `securestring` parameter and hello `@parameters()` [workflow definition function](http://aka.ms/logicappdocs) toouse a parameter that won't be readable in hello definition after saving.</span></span>
> 
> 

<span data-ttu-id="b4f75-249">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b4f75-249">For example:</span></span>

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a><span data-ttu-id="b4f75-250">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b4f75-250">Next steps</span></span>
<span data-ttu-id="b4f75-251">Prova nu hello plattform och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="b4f75-251">Now, try out hello platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="b4f75-252">Du kan utforska hello andra tillgängliga kopplingar i Logic Apps genom att titta på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="b4f75-252">You can explore hello other available connectors in Logic Apps by looking at our [APIs list](apis-list.md).</span></span>

