---
title: "aaaCall, utlösa eller kapsla arbetsflöden med HTTP-slutpunkter – Azure Logic Apps | Microsoft Docs"
description: "Konfigurera HTTP-slutpunkter toocall, utlösare eller kapslade arbetsflöden för Azure Logic Apps"
services: logic-apps
keywords: "arbetsflöden, HTTP-slutpunkter"
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 73ba2a70-03e9-4982-bfc8-ebfaad798bc2
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 03/31/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 072a314c3bff75ab7696f86bb063bb7c03c4ae89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="call-trigger-or-nest-workflows-with-http-endpoints-in-logic-apps"></a><span data-ttu-id="0b3e7-104">Anropa utlösare eller kapsla arbetsflöden med HTTP-slutpunkter i logikappar</span><span class="sxs-lookup"><span data-stu-id="0b3e7-104">Call, trigger, or nest workflows with HTTP endpoints in logic apps</span></span>

<span data-ttu-id="0b3e7-105">Du kan internt exponera synkron HTTP-slutpunkter som utlösare för logic apps så att du kan utlösa eller anropa dina logic apps via en annan URL.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-105">You can natively expose synchronous HTTP endpoints as triggers on logic apps so that you can trigger or call your logic apps through a URL.</span></span> <span data-ttu-id="0b3e7-106">Du kan också kapsla arbetsflöden i dina logic apps med hjälp av ett mönster för callable slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-106">You can also nest workflows in your logic apps by using a pattern of callable endpoints.</span></span>

<span data-ttu-id="0b3e7-107">toocreate http-slutpunkter kan du lägga till dessa utlösare så att dina logic apps kan ta emot inkommande begäranden:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-107">toocreate HTTP endpoints, you can add these triggers so that your logic apps can receive incoming requests:</span></span>

* [<span data-ttu-id="0b3e7-108">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="0b3e7-108">Request</span></span>](../connectors/connectors-native-reqres.md)

* [<span data-ttu-id="0b3e7-109">API-anslutning Webhooken</span><span class="sxs-lookup"><span data-stu-id="0b3e7-109">API Connection Webhook</span></span>](logic-apps-workflow-actions-triggers.md#api-connection-trigger)

* [<span data-ttu-id="0b3e7-110">HTTP-webhook</span><span class="sxs-lookup"><span data-stu-id="0b3e7-110">HTTP Webhook</span></span>](../connectors/connectors-native-webhook.md)

   > [!NOTE]
   > <span data-ttu-id="0b3e7-111">Även om våra exempel använder hello **begära** utlösare, kan du använda någon av hello visas http-utlösare och alla principer identiskt gäller toohello andra typer av utlösare.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-111">Although our examples use hello **Request** trigger, you can use any of hello listed HTTP triggers, and all principles identically apply toohello other trigger types.</span></span>

## <a name="set-up-an-http-endpoint-for-your-logic-app"></a><span data-ttu-id="0b3e7-112">Ställ in en HTTP-slutpunkt för din logikapp</span><span class="sxs-lookup"><span data-stu-id="0b3e7-112">Set up an HTTP endpoint for your logic app</span></span>

<span data-ttu-id="0b3e7-113">toocreate http-slutpunkt, lägga till en utlösare som kan ta emot inkommande begäranden.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-113">toocreate an HTTP endpoint, add a trigger that can receive incoming requests.</span></span>

1. <span data-ttu-id="0b3e7-114">Logga in toohello [Azure-portalen](https://portal.azure.com "Azure-portalen").</span><span class="sxs-lookup"><span data-stu-id="0b3e7-114">Sign in toohello [Azure portal](https://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="0b3e7-115">Gå tooyour logikapp och öppna logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-115">Go tooyour logic app, and open Logic App Designer.</span></span>

2. <span data-ttu-id="0b3e7-116">Lägg till en utlösare som gör att din logikapp ta emot inkommande begäranden.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-116">Add a trigger that lets your logic app receive incoming requests.</span></span> <span data-ttu-id="0b3e7-117">Lägg exempelvis till hello **begära** utlösaren tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-117">For example, add hello **Request** trigger tooyour logic app.</span></span>

3.  <span data-ttu-id="0b3e7-118">Under **begära brödtext JSON-Schema**, du kan även ange en JSON-schema för hello-nyttolasten (data) som du förväntar dig hello utlösaren tooreceive.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-118">Under **Request Body JSON Schema**, you can optionally enter a JSON schema for hello payload (data) that you expect hello trigger tooreceive.</span></span>

    <span data-ttu-id="0b3e7-119">hello-designer använder det här schemat för att generera token att din logikapp kan använda tooconsume parse och skicka data från hello utlösare genom arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-119">hello designer uses this schema for generating tokens that your logic app can use tooconsume, parse, and pass data from hello trigger through your workflow.</span></span> 
    <span data-ttu-id="0b3e7-120">Mer om [token genereras från JSON-scheman](#generated-tokens).</span><span class="sxs-lookup"><span data-stu-id="0b3e7-120">More about [tokens generated from JSON schemas](#generated-tokens).</span></span>

    <span data-ttu-id="0b3e7-121">Ange hello schemat visas i hello designer i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-121">For this example, enter hello schema shown in hello designer:</span></span>

    ```json
    {
      "type": "object",
      "properties": {
        "address": {
          "type": "string"
        }
      },
      "required": [
        "address"
      ]
    }
    ```

    ![Lägg till hello begäran åtgärd][1]

    > [!TIP]
    > 
    > <span data-ttu-id="0b3e7-123">Du kan skapa ett schema för en JSON-nyttolast med exempel från ett verktyg som [jsonschema.net](http://jsonschema.net/), eller i hello **begära** utlösaren genom att välja **Använd exemplet nyttolast toogenerate schemat**.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-123">You can generate a schema for a sample JSON payload from a tool like [jsonschema.net](http://jsonschema.net/), or in hello **Request** trigger by choosing **Use sample payload toogenerate schema**.</span></span> 
    > <span data-ttu-id="0b3e7-124">Ange ditt exempel nyttolasten och välj **klar**.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-124">Enter your sample payload, and choose **Done**.</span></span>

    <span data-ttu-id="0b3e7-125">Till exempel det här exemplet nyttolast:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-125">For example, this sample payload:</span></span>

    ```json
    {
       "address": "21 2nd Street, New York, New York"
    }
    ```

    <span data-ttu-id="0b3e7-126">genererar det här schemat:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-126">generates this schema:</span></span>

    ```json
    }
       "type": "object",
       "properties": {
          "address": {
             "type": "string" 
          }
       }
    }
    ```

4.  <span data-ttu-id="0b3e7-127">Spara din logikapp.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-127">Save your logic app.</span></span> <span data-ttu-id="0b3e7-128">Under **HTTP POST toothis URL**, nu bör du hitta en URL som genererade motringning som det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-128">Under **HTTP POST toothis URL**, you should now find a generated callback URL, like this example:</span></span>

    ![Genererade återanrop URL för slutpunkten](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

    <span data-ttu-id="0b3e7-130">Denna URL innehåller en delad signatur åtkomst (SAS)-nyckel i hello frågeparametrar som används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-130">This URL contains a Shared Access Signature (SAS) key in hello query parameters that are used for authentication.</span></span> 
    <span data-ttu-id="0b3e7-131">Du kan också få hello HTTP slutpunkts-URL från din app översikt för logiken i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-131">You can also get hello HTTP endpoint URL from your logic app overview in hello Azure portal.</span></span> <span data-ttu-id="0b3e7-132">Under **utlösaren historik**, Välj din utlösare:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-132">Under **Trigger History**, select your trigger:</span></span>

    ![Hämta HTTP slutpunkts-URL från Azure-portalen][2]

    <span data-ttu-id="0b3e7-134">Eller du kan hämta hello-URL genom att göra anropet:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-134">Or you can get hello URL by making this call:</span></span>

    ```
    POST https://management.azure.com/{logic-app-resourceID}/triggers/{myendpointtrigger}/listCallbackURL?api-version=2016-06-01
    ```

## <a name="change-hello-http-method-for-your-trigger"></a><span data-ttu-id="0b3e7-135">Ändra hello HTTP-metoden för utlösaren</span><span class="sxs-lookup"><span data-stu-id="0b3e7-135">Change hello HTTP method for your trigger</span></span>

<span data-ttu-id="0b3e7-136">Som standard hello **begäran** utlösaren förväntar sig en HTTP POST-begäran, men du kan använda en annan HTTP-metod.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-136">By default, hello **Request** trigger expects an HTTP POST request, but you can use a different HTTP method.</span></span> 

> [!NOTE]
> <span data-ttu-id="0b3e7-137">Du kan ange endast en metod.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-137">You can specify only one method type.</span></span>

1. <span data-ttu-id="0b3e7-138">På din **begära** utlösa, Välj **visa avancerade alternativ**.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-138">On your **Request** trigger, choose **Show advanced options**.</span></span>

2. <span data-ttu-id="0b3e7-139">Öppna hello **metoden** lista.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-139">Open hello **Method** list.</span></span> <span data-ttu-id="0b3e7-140">Det här exemplet väljer du **hämta** så att du kan testa din HTTP slutpunkts-URL senare.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-140">For this example, select **GET** so that you can test your HTTP endpoint's URL later.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0b3e7-141">Du kan välja andra HTTP-metoden eller ange en anpassad metod för egna logikapp.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-141">You can select any other HTTP method, or specify a custom method for your own logic app.</span></span>

    ![Ändra HTTP-metoden](./media/logic-apps-http-endpoint/change-method.png)

## <a name="accept-parameters-through-your-http-endpoint-url"></a><span data-ttu-id="0b3e7-143">Acceptera parametrar via HTTP-slutpunkt-URL</span><span class="sxs-lookup"><span data-stu-id="0b3e7-143">Accept parameters through your HTTP endpoint URL</span></span>

<span data-ttu-id="0b3e7-144">När du vill att din HTTP-slutpunkt URL tooaccept-parametrar kan du anpassa din utlösaren relativ sökväg.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-144">When you want your HTTP endpoint URL tooaccept parameters, customize your trigger's relative path.</span></span>

1. <span data-ttu-id="0b3e7-145">På din **begära** utlösa, Välj **visa avancerade alternativ**.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-145">On your **Request** trigger, choose **Show advanced options**.</span></span> 

2. <span data-ttu-id="0b3e7-146">Under **metoden**, ange hello HTTP-metod som du vill att din begäran toouse.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-146">Under **Method**, specify hello HTTP method that you want your request toouse.</span></span> <span data-ttu-id="0b3e7-147">Välj det här exemplet hello **hämta** metod om du inte redan har gjort så att du kan testa din HTTP slutpunkts-URL.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-147">For this example, select hello **GET** method, if you haven't already, so that you can test your HTTP endpoint's URL.</span></span>

      > [!NOTE]
      > <span data-ttu-id="0b3e7-148">Du måste också explicit ange HTTP-metoden för utlösaren när du anger en relativ sökväg för utlösaren.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-148">When you specify a relative path for your trigger, you must also explicitly specify an HTTP method for your trigger.</span></span>

3. <span data-ttu-id="0b3e7-149">Under **relativa sökvägen**, ange hello relativ sökväg för hello-parameter som URL: en ska acceptera, till exempel `customers/{customerID}`.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-149">Under **Relative path**, specify hello relative path for hello parameter that your URL should accept, for example, `customers/{customerID}`.</span></span>

    ![Ange hello HTTP-metoden och relativ sökväg för parametern](./media/logic-apps-http-endpoint/relativeurl.png)

4. <span data-ttu-id="0b3e7-151">toouse hello parametern, lägga till en **svar** åtgärd tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-151">toouse hello parameter, add a **Response** action tooyour logic app.</span></span> <span data-ttu-id="0b3e7-152">(Välj under din utlösaren **nytt steg** > **lägga till en åtgärd** > **svar**)</span><span class="sxs-lookup"><span data-stu-id="0b3e7-152">(Under your trigger, choose **New step** > **Add an action** > **Response**)</span></span> 

5. <span data-ttu-id="0b3e7-153">I ditt svar **brödtext**, inkludera hello token för hello-parameter som du angav i ditt utlösaren relativ sökväg.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-153">In your response's **Body**, include hello token for hello parameter that you specified in your trigger's relative path.</span></span>

    <span data-ttu-id="0b3e7-154">Till exempel tooreturn `Hello {customerID}`, uppdatera ditt svar **brödtext** med `Hello {customerID token}`.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-154">For example, tooreturn `Hello {customerID}`, update your response's **Body** with `Hello {customerID token}`.</span></span> 
    <span data-ttu-id="0b3e7-155">hello dynamisk innehåll lista bör visas och visa hello `customerID` token för du tooselect.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-155">hello dynamic content list should appear and show hello `customerID` token for you tooselect.</span></span>

    ![Lägg till parametern tooresponse brödtext](./media/logic-apps-http-endpoint/relativeurlresponse.png)

    <span data-ttu-id="0b3e7-157">Din **brödtext** bör se ut som i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-157">Your **Body** should look like this example:</span></span>

    ![Svarstexten med parametern](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

6. <span data-ttu-id="0b3e7-159">Spara din logikapp.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-159">Save your logic app.</span></span> 

    <span data-ttu-id="0b3e7-160">HTTP-slutpunkt-URL innehåller nu hello relativ sökväg, till exempel:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-160">Your HTTP endpoint URL now includes hello relative path, for example:</span></span> 

    <span data-ttu-id="0b3e7-161">https & # 58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...</span><span class="sxs-lookup"><span data-stu-id="0b3e7-161">https&#58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...</span></span>

7. <span data-ttu-id="0b3e7-162">tootest din HTTP-slutpunkt, kopiera och klistra in hello uppdaterade URL: en till ett nytt webbläsarfönster, men Ersätt `{customerID}` med `123456`, och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-162">tootest your HTTP endpoint, copy and paste hello updated URL into another browser window, but replace `{customerID}` with `123456`, and press Enter.</span></span>

    <span data-ttu-id="0b3e7-163">Webbläsaren bör visa den här texten:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-163">Your browser should show this text:</span></span> 

    `Hello 123456`

<a name="generated-tokens"></a>
### <a name="tokens-generated-from-json-schemas-for-your-logic-app"></a><span data-ttu-id="0b3e7-164">Token genereras från JSON-scheman för din logikapp</span><span class="sxs-lookup"><span data-stu-id="0b3e7-164">Tokens generated from JSON schemas for your logic app</span></span>

<span data-ttu-id="0b3e7-165">När du anger ett JSON-schema i din **begära** Utlös, hello logik App Designer genererar token för egenskaper i schemat.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-165">When you provide a JSON schema in your **Request** trigger, hello Logic App Designer generates tokens for properties in that schema.</span></span> <span data-ttu-id="0b3e7-166">Du kan sedan använda dessa token för att skicka data via logik app arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-166">You can then use those tokens for passing data through your logic app workflow.</span></span>

<span data-ttu-id="0b3e7-167">Det här exemplet, om du lägger till hello `title` och `name` egenskaper tooyour JSON-schema, deras token är nu tillgängliga toouse i senare arbetsflödessteg.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-167">For this example, if you add hello `title` and `name` properties tooyour JSON schema, their tokens are now available toouse in later workflow steps.</span></span> 

<span data-ttu-id="0b3e7-168">Här är hello fullständigt JSON-schema:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-168">Here is hello complete JSON schema:</span></span>

```json
{
   "type": "object",
   "properties": {
      "address": {
         "type": "string"
      },
      "title": {
         "type": "string"
      },
      "name": {
         "type": "string"
      }
   },
   "required": [
      "address",
      "title",
      "name"
   ]
}
```

## <a name="create-nested-workflows-for-logic-apps"></a><span data-ttu-id="0b3e7-169">Skapa inkapslade arbetsflöden för logic apps</span><span class="sxs-lookup"><span data-stu-id="0b3e7-169">Create nested workflows for logic apps</span></span>

<span data-ttu-id="0b3e7-170">Du kan kapsla arbetsflöden i din logikapp genom att lägga till andra logikappar som kan ta emot begäranden.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-170">You can nest workflows in your logic app by adding other logic apps that can receive requests.</span></span> <span data-ttu-id="0b3e7-171">tooinclude dessa logikappar lägga till hello **Logikappar i Azure - Välj ett arbetsflöde för Logic Apps** åtgärd tooyour utlösare.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-171">tooinclude these logic apps, add hello **Azure Logic Apps - Choose a Logic Apps workflow** action tooyour trigger.</span></span> <span data-ttu-id="0b3e7-172">Sedan kan du välja från berättigade logic apps.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-172">You can then select from eligible logic apps.</span></span>

![Lägg till en annan logikapp](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

## <a name="call-or-trigger-logic-apps-through-http-endpoints"></a><span data-ttu-id="0b3e7-174">Anropa eller utlösare logikappar via HTTP-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="0b3e7-174">Call or trigger logic apps through HTTP endpoints</span></span>

<span data-ttu-id="0b3e7-175">När du har skapat HTTP-slutpunkten kan du utlösa logikappen via en `POST` metoden toohello fullständiga URL: en.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-175">After you create your HTTP endpoint, you can trigger your logic app through a `POST` method toohello full URL.</span></span> <span data-ttu-id="0b3e7-176">Logikappar har inbyggt stöd för direkt åtkomst slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-176">Logic apps have built-in support for direct-access endpoints.</span></span>

## <a name="reference-content-from-an-incoming-request"></a><span data-ttu-id="0b3e7-177">Referensinnehåll från en inkommande begäran</span><span class="sxs-lookup"><span data-stu-id="0b3e7-177">Reference content from an incoming request</span></span>

<span data-ttu-id="0b3e7-178">Om hello innehåll typ är `application/json`, kan du referera egenskaper från hello inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-178">If hello content's type is `application/json`, you can reference properties from hello incoming request.</span></span> <span data-ttu-id="0b3e7-179">Annars behandlas innehåll som en binär enhet som du kan skicka tooother API: er.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-179">Otherwise, content is treated as a single binary unit that you can pass tooother APIs.</span></span> <span data-ttu-id="0b3e7-180">tooreference det här innehållet i hello arbetsflödet, måste du konvertera innehållet.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-180">tooreference this content inside hello workflow, you must convert that content.</span></span> <span data-ttu-id="0b3e7-181">Om du skickar till exempel `application/xml` innehåll, kan du använda `@xpath()` för ett XPath-extrahering eller `@json()` för konvertering av XML-tooJSON.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-181">For example, if you pass `application/xml` content, you can use `@xpath()` for an XPath extraction, or `@json()` for converting XML tooJSON.</span></span> <span data-ttu-id="0b3e7-182">Lär dig mer om [arbeta med innehållstyper](../logic-apps/logic-apps-content-type.md).</span><span class="sxs-lookup"><span data-stu-id="0b3e7-182">Learn about [working with content types](../logic-apps/logic-apps-content-type.md).</span></span>

<span data-ttu-id="0b3e7-183">tooget hello utdata från en inkommande begäran, kan du använda hello `@triggerOutputs()` funktion.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-183">tooget hello output from an incoming request, you can use hello `@triggerOutputs()` function.</span></span> <span data-ttu-id="0b3e7-184">hello utdata kan se ut det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-184">hello output might look like this example:</span></span>

```json
{
    "headers": {
        "content-type" : "application/json"
    },
    "body": {
        "myProperty" : "property value"
    }
}
```

<span data-ttu-id="0b3e7-185">tooaccess hello `body` egenskapen mer specifikt kan du använda hello `@triggerBody()` genväg.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-185">tooaccess hello `body` property specifically, you can use hello `@triggerBody()` shortcut.</span></span> 

## <a name="respond-toorequests"></a><span data-ttu-id="0b3e7-186">Svara toorequests</span><span class="sxs-lookup"><span data-stu-id="0b3e7-186">Respond toorequests</span></span>

<span data-ttu-id="0b3e7-187">Du kanske vill toorespond toocertain begäranden som startar en logikapp genom att returnera innehållet toohello anroparen.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-187">You might want toorespond toocertain requests that start a logic app by returning content toohello caller.</span></span> <span data-ttu-id="0b3e7-188">tooconstruct hello statuskoden rubrik och brödtext för ditt svar, du kan använda hello **svar** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-188">tooconstruct hello status code, header, and body for your response, you can use hello **Response** action.</span></span> <span data-ttu-id="0b3e7-189">Den här åtgärden kan finnas var som helst i din logikapp, inte bara i hello slutet av arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-189">This action can appear anywhere in your logic app, not just at hello end of your workflow.</span></span>

> [!NOTE] 
> <span data-ttu-id="0b3e7-190">Om din logikapp inte innehåller en **svar**, hello HTTP-ändpunkten svarar *omedelbart* med en **202 godkända** status.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-190">If your logic app doesn't include a **Response**, hello HTTP endpoint responds *immediately* with a **202 Accepted** status.</span></span> <span data-ttu-id="0b3e7-191">Dessutom för hello ursprungliga tooget hello svar på begäranden, även alla steg som krävs för hello svar måste slutföras inom hello [tidsgränsen för begäran](./logic-apps-limits-and-config.md) såvida inte du anropa hello arbetsflödet som en kapslad logikapp.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-191">Also, for hello original request tooget hello response, all steps required for hello response must finish within hello [request timeout limit](./logic-apps-limits-and-config.md) unless you call hello workflow as a nested logic app.</span></span> <span data-ttu-id="0b3e7-192">Om inget svar inträffar inom denna inkommande begäran om hello timeout och tar emot hello HTTP-svar **408 klientens**.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-192">If no response happens within this limit, hello incoming request times out and receives hello HTTP response **408 Client timeout**.</span></span> <span data-ttu-id="0b3e7-193">För kapslade logic apps fortsätter hello överordnade logikapp toowait svar tills slutförts, oavsett hur lång tid som krävs.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-193">For nested logic apps, hello parent logic app continues toowait for a response until completed, regardless of how much time is required.</span></span>

### <a name="construct-hello-response"></a><span data-ttu-id="0b3e7-194">Skapa hello svar</span><span class="sxs-lookup"><span data-stu-id="0b3e7-194">Construct hello response</span></span>

<span data-ttu-id="0b3e7-195">Du kan inkludera mer än ett sidhuvud och varje typ av innehåll i hello svarstexten.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-195">You can include more than one header and any type of content in hello response body.</span></span> <span data-ttu-id="0b3e7-196">I vårt exempelsvar hello huvudet anger att hello svar har innehållstyp `application/json`.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-196">In our example response, hello header specifies that hello response has content type `application/json`.</span></span> <span data-ttu-id="0b3e7-197">och hello brödtext innehåller `title` och `name`, baserat på hello JSON-schemat uppdateras tidigare för hello **begära** utlösare.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-197">and hello body contains `title` and `name`, based on hello JSON schema updated previously for hello **Request** trigger.</span></span>

![Åtgärd för HTTP-svar][3]

<span data-ttu-id="0b3e7-199">Svar har följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-199">Responses have these properties:</span></span>

| <span data-ttu-id="0b3e7-200">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0b3e7-200">Property</span></span> | <span data-ttu-id="0b3e7-201">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0b3e7-201">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0b3e7-202">statusCode</span><span class="sxs-lookup"><span data-stu-id="0b3e7-202">statusCode</span></span> |<span data-ttu-id="0b3e7-203">Anger hello HTTP-statuskoden för svarande toohello inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-203">Specifies hello HTTP status code for responding toohello incoming request.</span></span> <span data-ttu-id="0b3e7-204">Den här koden kan vara en giltig statuskod som börjar med 2xx, 4xx eller 5xx.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-204">This code can be any valid status code that starts with 2xx, 4xx, or 5xx.</span></span> <span data-ttu-id="0b3e7-205">3xx statuskoder är inte tillåtna.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-205">However, 3xx status codes are not permitted.</span></span> |
| <span data-ttu-id="0b3e7-206">Rubriker</span><span class="sxs-lookup"><span data-stu-id="0b3e7-206">headers</span></span> |<span data-ttu-id="0b3e7-207">Definierar valfritt antal huvuden tooinclude hello svar.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-207">Defines any number of headers tooinclude in hello response.</span></span> |
| <span data-ttu-id="0b3e7-208">Brödtext</span><span class="sxs-lookup"><span data-stu-id="0b3e7-208">body</span></span> |<span data-ttu-id="0b3e7-209">Anger ett body-objekt som kan vara en sträng, en JSON-objekt eller även binära innehåll som refereras från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-209">Specifies a body object that can be a string, a JSON object, or even binary content referenced from a previous step.</span></span> |

<span data-ttu-id="0b3e7-210">Här är vad hello JSON-schema som ser ut som nu för hello **svar** åtgärd:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-210">Here's what hello JSON schema looks like now for hello **Response** action:</span></span>

``` json
"Response": {
   "inputs": {
      "body": {
         "title": "@{triggerBody()?['title']}",
         "name": "@{triggerBody()?['name']}"
      },
      "headers": {
           "content-type": "application/json"
      },
      "statusCode": 200
   },
   "runAfter": {},
   "type": "Response"
}
```

> [!TIP]
> <span data-ttu-id="0b3e7-211">tooview hello fullständig JSON-definitionen för din logikapp på hello logik App Designer väljer **vyn kod**.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-211">tooview hello complete JSON definition for your logic app, on hello Logic App Designer, choose **Code view**.</span></span>

## <a name="q--a"></a><span data-ttu-id="0b3e7-212">Frågor och svar</span><span class="sxs-lookup"><span data-stu-id="0b3e7-212">Q & A</span></span>

#### <a name="q-what-about-url-security"></a><span data-ttu-id="0b3e7-213">F: Vad händer om URL-säkerhet?</span><span class="sxs-lookup"><span data-stu-id="0b3e7-213">Q: What about URL security?</span></span>

<span data-ttu-id="0b3e7-214">S: azure genererar på ett säkert sätt logik app återanrop URL: er med hjälp av en delad signatur åtkomst (SAS).</span><span class="sxs-lookup"><span data-stu-id="0b3e7-214">A: Azure securely generates logic app callback URLs using a Shared Access Signature (SAS).</span></span> <span data-ttu-id="0b3e7-215">Den här signaturen passerar som en frågeparameter och måste verifieras innan din logikapp kan utlösas.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-215">This signature passes through as a query parameter and must be validated before your logic app can fire.</span></span> <span data-ttu-id="0b3e7-216">Azure genererar hello signatur med hjälp av en unik kombination av en hemlig nyckel per logikapp, hello Utlösarnamn och hello-åtgärden som utförs.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-216">Azure generates hello signature using a unique combination of a secret key per logic app, hello trigger name, and hello operation that's performed.</span></span> <span data-ttu-id="0b3e7-217">Så att de inte kan skapa en giltig signatur såvida inte någon har appen åtkomstnyckel toohello hemliga logik.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-217">So unless someone has access toohello secret logic app key, they cannot generate a valid signature.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="0b3e7-218">Produktions- och säkra system rekommenderar vi mot anropa logikappen direkt från hello webbläsare eftersom:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-218">For production and secure systems, we strongly recommend against calling your logic app directly from hello browser because:</span></span>
   > 
   > * <span data-ttu-id="0b3e7-219">hello delade åtkomstnyckeln visas i hello-URL.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-219">hello shared access key appears in hello URL.</span></span>
   > * <span data-ttu-id="0b3e7-220">Du kan inte hantera säker innehåll principer på grund av tooshared domäner över Logikapp kunder.</span><span class="sxs-lookup"><span data-stu-id="0b3e7-220">You can't manage secure content policies due tooshared domains across Logic App customers.</span></span>

#### <a name="q-can-i-configure-http-endpoints-further"></a><span data-ttu-id="0b3e7-221">F: kan jag konfigurera HTTP-slutpunkter ytterligare?</span><span class="sxs-lookup"><span data-stu-id="0b3e7-221">Q: Can I configure HTTP endpoints further?</span></span>

<span data-ttu-id="0b3e7-222">S: Ja, HTTP-slutpunkter stöd för mer avancerad konfiguration via [ **API Management**](../api-management/api-management-key-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="0b3e7-222">A: Yes, HTTP endpoints support more advanced configuration through [**API Management**](../api-management/api-management-key-concepts.md).</span></span> <span data-ttu-id="0b3e7-223">Den här tjänsten erbjuder också hello kapaciteten för du tooconsistently hantera alla dina API: er, inklusive logikappar, konfigurera anpassade domännamn, använda flera autentiseringsmetoder och mycket mer, till exempel:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-223">This service also offers hello capability for you tooconsistently manage all your APIs, including logic apps, set up custom domain names, use more authentication methods, and more, for example:</span></span>

* [<span data-ttu-id="0b3e7-224">Ändra hello begäran metod</span><span class="sxs-lookup"><span data-stu-id="0b3e7-224">Change hello request method</span></span>](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#SetRequestMethod)
* [<span data-ttu-id="0b3e7-225">Ändra hello URL-segment i hello-begäran</span><span class="sxs-lookup"><span data-stu-id="0b3e7-225">Change hello URL segments of hello request</span></span>](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#RewriteURL)
* <span data-ttu-id="0b3e7-226">Ställ in din API Management-domäner i hello [Azure-portalen](https://portal.azure.com/ "Azure-portalen")</span><span class="sxs-lookup"><span data-stu-id="0b3e7-226">Set up your API Management domains in hello [Azure portal](https://portal.azure.com/ "Azure portal")</span></span>
* <span data-ttu-id="0b3e7-227">Ställa in principen toocheck för grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="0b3e7-227">Set up policy toocheck for Basic authentication</span></span>

#### <a name="q-what-changed-when-hello-schema-migrated-from-hello-december-1-2014-preview"></a><span data-ttu-id="0b3e7-228">F: vad ändrats när hello schemat migreras från hello 1 December 2014 preview?</span><span class="sxs-lookup"><span data-stu-id="0b3e7-228">Q: What changed when hello schema migrated from hello December 1, 2014 preview?</span></span>

<span data-ttu-id="0b3e7-229">S: här är en sammanfattning om dessa ändringar:</span><span class="sxs-lookup"><span data-stu-id="0b3e7-229">A: Here's a summary about these changes:</span></span>

| <span data-ttu-id="0b3e7-230">1 december 2014 preview</span><span class="sxs-lookup"><span data-stu-id="0b3e7-230">December 1, 2014 preview</span></span> | <span data-ttu-id="0b3e7-231">Den 1 juni 2016</span><span class="sxs-lookup"><span data-stu-id="0b3e7-231">June 1, 2016</span></span> |
| --- | --- |
| <span data-ttu-id="0b3e7-232">Klicka på **HTTP-lyssnaren** API-App</span><span class="sxs-lookup"><span data-stu-id="0b3e7-232">Click **HTTP Listener** API App</span></span> |<span data-ttu-id="0b3e7-233">Klicka på **manuell utlösaren** (ingen API-App krävs)</span><span class="sxs-lookup"><span data-stu-id="0b3e7-233">Click **Manual trigger** (no API App required)</span></span> |
| <span data-ttu-id="0b3e7-234">HTTP-lyssnaren inställningen ”*skickar svar automatiskt*”</span><span class="sxs-lookup"><span data-stu-id="0b3e7-234">HTTP Listener setting "*Sends response automatically*"</span></span> |<span data-ttu-id="0b3e7-235">Innehåller en **svar** åtgärd eller inte i Arbetsflödesdefinitionen för hello</span><span class="sxs-lookup"><span data-stu-id="0b3e7-235">Either include a **Response** action or not in hello workflow definition</span></span> |
| <span data-ttu-id="0b3e7-236">Konfigurera grundläggande eller OAuth-autentisering</span><span class="sxs-lookup"><span data-stu-id="0b3e7-236">Configure Basic or OAuth authentication</span></span> |<span data-ttu-id="0b3e7-237">via API-hantering</span><span class="sxs-lookup"><span data-stu-id="0b3e7-237">via API Management</span></span> |
| <span data-ttu-id="0b3e7-238">Konfigurera HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="0b3e7-238">Configure HTTP method</span></span> |<span data-ttu-id="0b3e7-239">Under **visa avancerade alternativ**, Välj en HTTP-metod</span><span class="sxs-lookup"><span data-stu-id="0b3e7-239">Under **Show advanced options**, choose an HTTP method</span></span> |
| <span data-ttu-id="0b3e7-240">Konfigurera relativ sökväg</span><span class="sxs-lookup"><span data-stu-id="0b3e7-240">Configure relative path</span></span> |<span data-ttu-id="0b3e7-241">Under **visa avancerade alternativ**, lägga till en relativ sökväg</span><span class="sxs-lookup"><span data-stu-id="0b3e7-241">Under **Show advanced options**, add a relative path</span></span> |
| <span data-ttu-id="0b3e7-242">Referens hello inkommande brödtext via`@triggerOutputs().body.Content`</span><span class="sxs-lookup"><span data-stu-id="0b3e7-242">Reference hello incoming body through `@triggerOutputs().body.Content`</span></span> |<span data-ttu-id="0b3e7-243">Referens till`@triggerOutputs().body`</span><span class="sxs-lookup"><span data-stu-id="0b3e7-243">Reference through `@triggerOutputs().body`</span></span> |
| <span data-ttu-id="0b3e7-244">**Skicka HTTP-svar** åtgärd på hello HTTP-lyssnare</span><span class="sxs-lookup"><span data-stu-id="0b3e7-244">**Send HTTP response** action on hello HTTP Listener</span></span> |<span data-ttu-id="0b3e7-245">Klicka på **svara tooHTTP begäran** (ingen API-App krävs)</span><span class="sxs-lookup"><span data-stu-id="0b3e7-245">Click **Respond tooHTTP request** (no API App required)</span></span> |

## <a name="get-help"></a><span data-ttu-id="0b3e7-246">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="0b3e7-246">Get help</span></span>

<span data-ttu-id="0b3e7-247">tooask frågor besvara frågor och lär dig vilka andra Azure Logikappar användarna gör besök hello [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="0b3e7-247">tooask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="0b3e7-248">toohelp förbättra Azure Logic Apps och kopplingar, rösta eller skicka in förslag på hello [Azure Logikappar användare feedbackwebbplats](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="0b3e7-248">toohelp improve Azure Logic Apps and connectors, vote on or submit ideas at hello [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b3e7-249">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0b3e7-249">Next steps</span></span>

* [<span data-ttu-id="0b3e7-250">Skapa Logic App-definitioner</span><span class="sxs-lookup"><span data-stu-id="0b3e7-250">Author logic app definitions</span></span>](./logic-apps-author-definitions.md)
* [<span data-ttu-id="0b3e7-251">Hantera fel och undantag</span><span class="sxs-lookup"><span data-stu-id="0b3e7-251">Handle errors and exceptions</span></span>](./logic-apps-exception-handling.md)

[1]: ./media/logic-apps-http-endpoint/manualtrigger.png
[2]: ./media/logic-apps-http-endpoint/manualtriggerurl.png
[3]: ./media/logic-apps-http-endpoint/response.png
