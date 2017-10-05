---
title: "Anropa, utlösare eller kapsla arbetsflöden med HTTP-slutpunkter – Azure Logic Apps | Microsoft Docs"
description: "Konfigurera HTTP-slutpunkter för anrop, utlösare eller kapsla arbetsflöden för Azure Logic Apps"
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
ms.openlocfilehash: c92692db23ac59f67890e26cce6b2d3272e8901d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="call-trigger-or-nest-workflows-with-http-endpoints-in-logic-apps"></a><span data-ttu-id="5178c-104">Anropa utlösare eller kapsla arbetsflöden med HTTP-slutpunkter i logikappar</span><span class="sxs-lookup"><span data-stu-id="5178c-104">Call, trigger, or nest workflows with HTTP endpoints in logic apps</span></span>

<span data-ttu-id="5178c-105">Du kan internt exponera synkron HTTP-slutpunkter som utlösare för logic apps så att du kan utlösa eller anropa dina logic apps via en annan URL.</span><span class="sxs-lookup"><span data-stu-id="5178c-105">You can natively expose synchronous HTTP endpoints as triggers on logic apps so that you can trigger or call your logic apps through a URL.</span></span> <span data-ttu-id="5178c-106">Du kan också kapsla arbetsflöden i dina logic apps med hjälp av ett mönster för callable slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="5178c-106">You can also nest workflows in your logic apps by using a pattern of callable endpoints.</span></span>

<span data-ttu-id="5178c-107">Du kan lägga till dessa utlösare så att dina logic apps kan ta emot inkommande begäranden om du vill skapa HTTP-slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="5178c-107">To create HTTP endpoints, you can add these triggers so that your logic apps can receive incoming requests:</span></span>

* [<span data-ttu-id="5178c-108">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="5178c-108">Request</span></span>](../connectors/connectors-native-reqres.md)

* [<span data-ttu-id="5178c-109">API-anslutning Webhooken</span><span class="sxs-lookup"><span data-stu-id="5178c-109">API Connection Webhook</span></span>](logic-apps-workflow-actions-triggers.md#api-connection-trigger)

* [<span data-ttu-id="5178c-110">HTTP-webhook</span><span class="sxs-lookup"><span data-stu-id="5178c-110">HTTP Webhook</span></span>](../connectors/connectors-native-webhook.md)

   > [!NOTE]
   > <span data-ttu-id="5178c-111">Även om våra exempel använder den **begära** utlösare, du kan använda någon av de listade HTTP-utlösarna och identiskt gäller alla principer för andra typer av utlösare.</span><span class="sxs-lookup"><span data-stu-id="5178c-111">Although our examples use the **Request** trigger, you can use any of the listed HTTP triggers, and all principles identically apply to the other trigger types.</span></span>

## <a name="set-up-an-http-endpoint-for-your-logic-app"></a><span data-ttu-id="5178c-112">Ställ in en HTTP-slutpunkt för din logikapp</span><span class="sxs-lookup"><span data-stu-id="5178c-112">Set up an HTTP endpoint for your logic app</span></span>

<span data-ttu-id="5178c-113">Lägg till en utlösare som kan ta emot inkommande begäranden om du vill skapa en HTTP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="5178c-113">To create an HTTP endpoint, add a trigger that can receive incoming requests.</span></span>

1. <span data-ttu-id="5178c-114">Logga in på [Azure Portal](https://portal.azure.com "Azure Portal").</span><span class="sxs-lookup"><span data-stu-id="5178c-114">Sign in to the [Azure portal](https://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="5178c-115">Gå till din logikapp och öppna logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="5178c-115">Go to your logic app, and open Logic App Designer.</span></span>

2. <span data-ttu-id="5178c-116">Lägg till en utlösare som gör att din logikapp ta emot inkommande begäranden.</span><span class="sxs-lookup"><span data-stu-id="5178c-116">Add a trigger that lets your logic app receive incoming requests.</span></span> <span data-ttu-id="5178c-117">Till exempel lägga till den **begära** utlösaren till din logikapp.</span><span class="sxs-lookup"><span data-stu-id="5178c-117">For example, add the **Request** trigger to your logic app.</span></span>

3.  <span data-ttu-id="5178c-118">Under **begära brödtext JSON-Schema**, du kan även ange en JSON-schema för nyttolasten (data) som du förväntar dig utlösaren tar emot.</span><span class="sxs-lookup"><span data-stu-id="5178c-118">Under **Request Body JSON Schema**, you can optionally enter a JSON schema for the payload (data) that you expect the trigger to receive.</span></span>

    <span data-ttu-id="5178c-119">Det här schemat används i designer för att generera token som din logikapp kan använda för att använda parsa och skicka data från utlösaren genom arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="5178c-119">The designer uses this schema for generating tokens that your logic app can use to consume, parse, and pass data from the trigger through your workflow.</span></span> 
    <span data-ttu-id="5178c-120">Mer om [token genereras från JSON-scheman](#generated-tokens).</span><span class="sxs-lookup"><span data-stu-id="5178c-120">More about [tokens generated from JSON schemas](#generated-tokens).</span></span>

    <span data-ttu-id="5178c-121">Ange schemat visas i designern för det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="5178c-121">For this example, enter the schema shown in the designer:</span></span>

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

    ![Lägg till instruktionen begäran][1]

    > [!TIP]
    > 
    > <span data-ttu-id="5178c-123">Du kan skapa ett schema för en JSON-nyttolast med exempel från ett verktyg som [jsonschema.net](http://jsonschema.net/), eller i den **begära** utlösaren genom att välja **Använd exemplet nyttolast för att skapa schema**.</span><span class="sxs-lookup"><span data-stu-id="5178c-123">You can generate a schema for a sample JSON payload from a tool like [jsonschema.net](http://jsonschema.net/), or in the **Request** trigger by choosing **Use sample payload to generate schema**.</span></span> 
    > <span data-ttu-id="5178c-124">Ange ditt exempel nyttolasten och välj **klar**.</span><span class="sxs-lookup"><span data-stu-id="5178c-124">Enter your sample payload, and choose **Done**.</span></span>

    <span data-ttu-id="5178c-125">Till exempel det här exemplet nyttolast:</span><span class="sxs-lookup"><span data-stu-id="5178c-125">For example, this sample payload:</span></span>

    ```json
    {
       "address": "21 2nd Street, New York, New York"
    }
    ```

    <span data-ttu-id="5178c-126">genererar det här schemat:</span><span class="sxs-lookup"><span data-stu-id="5178c-126">generates this schema:</span></span>

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

4.  <span data-ttu-id="5178c-127">Spara din logikapp.</span><span class="sxs-lookup"><span data-stu-id="5178c-127">Save your logic app.</span></span> <span data-ttu-id="5178c-128">Under **HTTP POST till denna URL**, nu bör du hitta en URL som genererade motringning som det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="5178c-128">Under **HTTP POST to this URL**, you should now find a generated callback URL, like this example:</span></span>

    ![Genererade återanrop URL för slutpunkten](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

    <span data-ttu-id="5178c-130">Denna URL innehåller en delad signatur åtkomst (SAS)-nyckel i frågeparametrar som används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="5178c-130">This URL contains a Shared Access Signature (SAS) key in the query parameters that are used for authentication.</span></span> 
    <span data-ttu-id="5178c-131">Du kan också få HTTP slutpunkts-URL från din app översikt för logiken i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5178c-131">You can also get the HTTP endpoint URL from your logic app overview in the Azure portal.</span></span> <span data-ttu-id="5178c-132">Under **utlösaren historik**, Välj din utlösare:</span><span class="sxs-lookup"><span data-stu-id="5178c-132">Under **Trigger History**, select your trigger:</span></span>

    ![Hämta HTTP slutpunkts-URL från Azure-portalen][2]

    <span data-ttu-id="5178c-134">Eller du kan hämta Webbadressen genom att göra anropet:</span><span class="sxs-lookup"><span data-stu-id="5178c-134">Or you can get the URL by making this call:</span></span>

    ```
    POST https://management.azure.com/{logic-app-resourceID}/triggers/{myendpointtrigger}/listCallbackURL?api-version=2016-06-01
    ```

## <a name="change-the-http-method-for-your-trigger"></a><span data-ttu-id="5178c-135">Ändra HTTP-metod för utlösaren</span><span class="sxs-lookup"><span data-stu-id="5178c-135">Change the HTTP method for your trigger</span></span>

<span data-ttu-id="5178c-136">Som standard den **begäran** utlösaren förväntar sig en HTTP POST-begäran, men du kan använda en annan HTTP-metod.</span><span class="sxs-lookup"><span data-stu-id="5178c-136">By default, the **Request** trigger expects an HTTP POST request, but you can use a different HTTP method.</span></span> 

> [!NOTE]
> <span data-ttu-id="5178c-137">Du kan ange endast en metod.</span><span class="sxs-lookup"><span data-stu-id="5178c-137">You can specify only one method type.</span></span>

1. <span data-ttu-id="5178c-138">På din **begära** utlösa, Välj **visa avancerade alternativ**.</span><span class="sxs-lookup"><span data-stu-id="5178c-138">On your **Request** trigger, choose **Show advanced options**.</span></span>

2. <span data-ttu-id="5178c-139">Öppna den **metoden** lista.</span><span class="sxs-lookup"><span data-stu-id="5178c-139">Open the **Method** list.</span></span> <span data-ttu-id="5178c-140">Det här exemplet väljer du **hämta** så att du kan testa din HTTP slutpunkts-URL senare.</span><span class="sxs-lookup"><span data-stu-id="5178c-140">For this example, select **GET** so that you can test your HTTP endpoint's URL later.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5178c-141">Du kan välja andra HTTP-metoden eller ange en anpassad metod för egna logikapp.</span><span class="sxs-lookup"><span data-stu-id="5178c-141">You can select any other HTTP method, or specify a custom method for your own logic app.</span></span>

    ![Ändra HTTP-metoden](./media/logic-apps-http-endpoint/change-method.png)

## <a name="accept-parameters-through-your-http-endpoint-url"></a><span data-ttu-id="5178c-143">Acceptera parametrar via HTTP-slutpunkt-URL</span><span class="sxs-lookup"><span data-stu-id="5178c-143">Accept parameters through your HTTP endpoint URL</span></span>

<span data-ttu-id="5178c-144">Om du vill HTTP slutpunkts-URL att acceptera parametrar kan anpassa din utlösaren relativ sökväg.</span><span class="sxs-lookup"><span data-stu-id="5178c-144">When you want your HTTP endpoint URL to accept parameters, customize your trigger's relative path.</span></span>

1. <span data-ttu-id="5178c-145">På din **begära** utlösa, Välj **visa avancerade alternativ**.</span><span class="sxs-lookup"><span data-stu-id="5178c-145">On your **Request** trigger, choose **Show advanced options**.</span></span> 

2. <span data-ttu-id="5178c-146">Under **metoden**, ange HTTP-metod som du vill att din begäran om att använda.</span><span class="sxs-lookup"><span data-stu-id="5178c-146">Under **Method**, specify the HTTP method that you want your request to use.</span></span> <span data-ttu-id="5178c-147">Det här exemplet väljer du den **hämta** metod om du inte redan har gjort så att du kan testa din HTTP slutpunkts-URL.</span><span class="sxs-lookup"><span data-stu-id="5178c-147">For this example, select the **GET** method, if you haven't already, so that you can test your HTTP endpoint's URL.</span></span>

      > [!NOTE]
      > <span data-ttu-id="5178c-148">Du måste också explicit ange HTTP-metoden för utlösaren när du anger en relativ sökväg för utlösaren.</span><span class="sxs-lookup"><span data-stu-id="5178c-148">When you specify a relative path for your trigger, you must also explicitly specify an HTTP method for your trigger.</span></span>

3. <span data-ttu-id="5178c-149">Under **relativa sökvägen**, ange den relativa sökvägen för den parameter som URL: en ska acceptera, till exempel `customers/{customerID}`.</span><span class="sxs-lookup"><span data-stu-id="5178c-149">Under **Relative path**, specify the relative path for the parameter that your URL should accept, for example, `customers/{customerID}`.</span></span>

    ![Ange HTTP-metoden och relativ sökväg för parametern](./media/logic-apps-http-endpoint/relativeurl.png)

4. <span data-ttu-id="5178c-151">Använd parametern genom att lägga till en **svar** åtgärder för att din logikapp.</span><span class="sxs-lookup"><span data-stu-id="5178c-151">To use the parameter, add a **Response** action to your logic app.</span></span> <span data-ttu-id="5178c-152">(Välj under din utlösaren **nytt steg** > **lägga till en åtgärd** > **svar**)</span><span class="sxs-lookup"><span data-stu-id="5178c-152">(Under your trigger, choose **New step** > **Add an action** > **Response**)</span></span> 

5. <span data-ttu-id="5178c-153">I ditt svar **brödtext**, inkludera token för den parameter som du angav i ditt utlösaren relativ sökväg.</span><span class="sxs-lookup"><span data-stu-id="5178c-153">In your response's **Body**, include the token for the parameter that you specified in your trigger's relative path.</span></span>

    <span data-ttu-id="5178c-154">Till exempel för att returnera `Hello {customerID}`, uppdatera ditt svar **brödtext** med `Hello {customerID token}`.</span><span class="sxs-lookup"><span data-stu-id="5178c-154">For example, to return `Hello {customerID}`, update your response's **Body** with `Hello {customerID token}`.</span></span> 
    <span data-ttu-id="5178c-155">Listan över dynamiskt innehåll ska visas och visa den `customerID` token som du kan välja.</span><span class="sxs-lookup"><span data-stu-id="5178c-155">The dynamic content list should appear and show the `customerID` token for you to select.</span></span>

    ![Lägg till parametern brödtext för svar](./media/logic-apps-http-endpoint/relativeurlresponse.png)

    <span data-ttu-id="5178c-157">Din **brödtext** bör se ut som i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="5178c-157">Your **Body** should look like this example:</span></span>

    ![Svarstexten med parametern](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

6. <span data-ttu-id="5178c-159">Spara din logikapp.</span><span class="sxs-lookup"><span data-stu-id="5178c-159">Save your logic app.</span></span> 

    <span data-ttu-id="5178c-160">HTTP-slutpunkt-URL innehåller nu relativ sökväg, till exempel:</span><span class="sxs-lookup"><span data-stu-id="5178c-160">Your HTTP endpoint URL now includes the relative path, for example:</span></span> 

    <span data-ttu-id="5178c-161">https & # 58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...</span><span class="sxs-lookup"><span data-stu-id="5178c-161">https&#58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...</span></span>

7. <span data-ttu-id="5178c-162">Om du vill testa HTTP-slutpunkt, kopiera och klistra in den uppdaterade URL: en i ett nytt webbläsarfönster, men Ersätt `{customerID}` med `123456`, och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="5178c-162">To test your HTTP endpoint, copy and paste the updated URL into another browser window, but replace `{customerID}` with `123456`, and press Enter.</span></span>

    <span data-ttu-id="5178c-163">Webbläsaren bör visa den här texten:</span><span class="sxs-lookup"><span data-stu-id="5178c-163">Your browser should show this text:</span></span> 

    `Hello 123456`

<a name="generated-tokens"></a>
### <a name="tokens-generated-from-json-schemas-for-your-logic-app"></a><span data-ttu-id="5178c-164">Token genereras från JSON-scheman för din logikapp</span><span class="sxs-lookup"><span data-stu-id="5178c-164">Tokens generated from JSON schemas for your logic app</span></span>

<span data-ttu-id="5178c-165">När du anger ett JSON-schema i din **begära** utlösaren logik App Designer genererar token för egenskaper i schemat.</span><span class="sxs-lookup"><span data-stu-id="5178c-165">When you provide a JSON schema in your **Request** trigger, the Logic App Designer generates tokens for properties in that schema.</span></span> <span data-ttu-id="5178c-166">Du kan sedan använda dessa token för att skicka data via logik app arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="5178c-166">You can then use those tokens for passing data through your logic app workflow.</span></span>

<span data-ttu-id="5178c-167">Det här exemplet, om du lägger till den `title` och `name` egenskaper till JSON-schema, deras token är nu tillgänglig för användning i senare arbetsflödessteg.</span><span class="sxs-lookup"><span data-stu-id="5178c-167">For this example, if you add the `title` and `name` properties to your JSON schema, their tokens are now available to use in later workflow steps.</span></span> 

<span data-ttu-id="5178c-168">Här är fullständigt JSON-schema:</span><span class="sxs-lookup"><span data-stu-id="5178c-168">Here is the complete JSON schema:</span></span>

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

## <a name="create-nested-workflows-for-logic-apps"></a><span data-ttu-id="5178c-169">Skapa inkapslade arbetsflöden för logic apps</span><span class="sxs-lookup"><span data-stu-id="5178c-169">Create nested workflows for logic apps</span></span>

<span data-ttu-id="5178c-170">Du kan kapsla arbetsflöden i din logikapp genom att lägga till andra logikappar som kan ta emot begäranden.</span><span class="sxs-lookup"><span data-stu-id="5178c-170">You can nest workflows in your logic app by adding other logic apps that can receive requests.</span></span> <span data-ttu-id="5178c-171">För att inkludera dessa logikappar, lägger du till den **Logikappar i Azure - Välj ett arbetsflöde för Logic Apps** åtgärd att utlösaren.</span><span class="sxs-lookup"><span data-stu-id="5178c-171">To include these logic apps, add the **Azure Logic Apps - Choose a Logic Apps workflow** action to your trigger.</span></span> <span data-ttu-id="5178c-172">Sedan kan du välja från berättigade logic apps.</span><span class="sxs-lookup"><span data-stu-id="5178c-172">You can then select from eligible logic apps.</span></span>

![Lägg till en annan logikapp](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

## <a name="call-or-trigger-logic-apps-through-http-endpoints"></a><span data-ttu-id="5178c-174">Anropa eller utlösare logikappar via HTTP-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="5178c-174">Call or trigger logic apps through HTTP endpoints</span></span>

<span data-ttu-id="5178c-175">När du har skapat HTTP-slutpunkten kan du utlösa logikappen via en `POST` metod för att en fullständig URL.</span><span class="sxs-lookup"><span data-stu-id="5178c-175">After you create your HTTP endpoint, you can trigger your logic app through a `POST` method to the full URL.</span></span> <span data-ttu-id="5178c-176">Logikappar har inbyggt stöd för direkt åtkomst slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="5178c-176">Logic apps have built-in support for direct-access endpoints.</span></span>

## <a name="reference-content-from-an-incoming-request"></a><span data-ttu-id="5178c-177">Referensinnehåll från en inkommande begäran</span><span class="sxs-lookup"><span data-stu-id="5178c-177">Reference content from an incoming request</span></span>

<span data-ttu-id="5178c-178">Om det innehåll av typen `application/json`, kan du referera egenskaper från den inkommande begäranden.</span><span class="sxs-lookup"><span data-stu-id="5178c-178">If the content's type is `application/json`, you can reference properties from the incoming request.</span></span> <span data-ttu-id="5178c-179">Annars behandlas innehåll som en binär enhet som du kan skicka till andra API: er.</span><span class="sxs-lookup"><span data-stu-id="5178c-179">Otherwise, content is treated as a single binary unit that you can pass to other APIs.</span></span> <span data-ttu-id="5178c-180">Om du vill referera till det här innehållet i arbetsflödet, måste du konvertera innehållet.</span><span class="sxs-lookup"><span data-stu-id="5178c-180">To reference this content inside the workflow, you must convert that content.</span></span> <span data-ttu-id="5178c-181">Om du skickar till exempel `application/xml` innehåll, kan du använda `@xpath()` för ett XPath-extrahering eller `@json()` för att konvertera XML till JSON.</span><span class="sxs-lookup"><span data-stu-id="5178c-181">For example, if you pass `application/xml` content, you can use `@xpath()` for an XPath extraction, or `@json()` for converting XML to JSON.</span></span> <span data-ttu-id="5178c-182">Lär dig mer om [arbeta med innehållstyper](../logic-apps/logic-apps-content-type.md).</span><span class="sxs-lookup"><span data-stu-id="5178c-182">Learn about [working with content types](../logic-apps/logic-apps-content-type.md).</span></span>

<span data-ttu-id="5178c-183">Om du vill hämta utdata från en inkommande begäran, kan du använda den `@triggerOutputs()` funktion.</span><span class="sxs-lookup"><span data-stu-id="5178c-183">To get the output from an incoming request, you can use the `@triggerOutputs()` function.</span></span> <span data-ttu-id="5178c-184">Utdata kan se ut det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="5178c-184">The output might look like this example:</span></span>

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

<span data-ttu-id="5178c-185">Att få åtkomst till den `body` egenskapen mer specifikt kan du använda den `@triggerBody()` genväg.</span><span class="sxs-lookup"><span data-stu-id="5178c-185">To access the `body` property specifically, you can use the `@triggerBody()` shortcut.</span></span> 

## <a name="respond-to-requests"></a><span data-ttu-id="5178c-186">Svara på begäranden</span><span class="sxs-lookup"><span data-stu-id="5178c-186">Respond to requests</span></span>

<span data-ttu-id="5178c-187">Du kanske vill besvara vissa begäranden som startar en logikapp genom att returnera innehållet till anroparen.</span><span class="sxs-lookup"><span data-stu-id="5178c-187">You might want to respond to certain requests that start a logic app by returning content to the caller.</span></span> <span data-ttu-id="5178c-188">Du kan använda för att skapa statuskod, sidhuvud och brödtext för ditt svar i **svar** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="5178c-188">To construct the status code, header, and body for your response, you can use the **Response** action.</span></span> <span data-ttu-id="5178c-189">Den här åtgärden kan finnas var som helst i din logikapp, inte bara i slutet av arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="5178c-189">This action can appear anywhere in your logic app, not just at the end of your workflow.</span></span>

> [!NOTE] 
> <span data-ttu-id="5178c-190">Om din logikapp inte innehåller en **svar**, HTTP-slutpunkten svarar *omedelbart* med en **202 godkända** status.</span><span class="sxs-lookup"><span data-stu-id="5178c-190">If your logic app doesn't include a **Response**, the HTTP endpoint responds *immediately* with a **202 Accepted** status.</span></span> <span data-ttu-id="5178c-191">Dessutom för den ursprungliga begäranden hämta svaret, även alla steg som krävs för svaret måste slutföras inom den [tidsgränsen för begäran](./logic-apps-limits-and-config.md) såvida inte du anropa arbetsflödet som en kapslad logikapp.</span><span class="sxs-lookup"><span data-stu-id="5178c-191">Also, for the original request to get the response, all steps required for the response must finish within the [request timeout limit](./logic-apps-limits-and-config.md) unless you call the workflow as a nested logic app.</span></span> <span data-ttu-id="5178c-192">Om inget svar inträffar inom denna inkommande begäran på grund av timeout och tar emot HTTP-svaret **408 klientens**.</span><span class="sxs-lookup"><span data-stu-id="5178c-192">If no response happens within this limit, the incoming request times out and receives the HTTP response **408 Client timeout**.</span></span> <span data-ttu-id="5178c-193">För kapslade logic apps fortsätter logikappen överordnade att vänta tills slutförts, oavsett hur lång tid som krävs.</span><span class="sxs-lookup"><span data-stu-id="5178c-193">For nested logic apps, the parent logic app continues to wait for a response until completed, regardless of how much time is required.</span></span>

### <a name="construct-the-response"></a><span data-ttu-id="5178c-194">Skapa svaret</span><span class="sxs-lookup"><span data-stu-id="5178c-194">Construct the response</span></span>

<span data-ttu-id="5178c-195">Du kan inkludera mer än ett sidhuvud och varje typ av innehåll i brödtext för svar.</span><span class="sxs-lookup"><span data-stu-id="5178c-195">You can include more than one header and any type of content in the response body.</span></span> <span data-ttu-id="5178c-196">I vårt exempelsvar huvudet anger att svaret har innehållstyp `application/json`.</span><span class="sxs-lookup"><span data-stu-id="5178c-196">In our example response, the header specifies that the response has content type `application/json`.</span></span> <span data-ttu-id="5178c-197">och innehåller `title` och `name`, baserat på JSON-schema som tidigare har uppdaterats för den **begära** utlösare.</span><span class="sxs-lookup"><span data-stu-id="5178c-197">and the body contains `title` and `name`, based on the JSON schema updated previously for the **Request** trigger.</span></span>

![Åtgärd för HTTP-svar][3]

<span data-ttu-id="5178c-199">Svar har följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="5178c-199">Responses have these properties:</span></span>

| <span data-ttu-id="5178c-200">Egenskap</span><span class="sxs-lookup"><span data-stu-id="5178c-200">Property</span></span> | <span data-ttu-id="5178c-201">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5178c-201">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5178c-202">statusCode</span><span class="sxs-lookup"><span data-stu-id="5178c-202">statusCode</span></span> |<span data-ttu-id="5178c-203">Anger HTTP-statuskod för svarar på inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="5178c-203">Specifies the HTTP status code for responding to the incoming request.</span></span> <span data-ttu-id="5178c-204">Den här koden kan vara en giltig statuskod som börjar med 2xx, 4xx eller 5xx.</span><span class="sxs-lookup"><span data-stu-id="5178c-204">This code can be any valid status code that starts with 2xx, 4xx, or 5xx.</span></span> <span data-ttu-id="5178c-205">3xx statuskoder är inte tillåtna.</span><span class="sxs-lookup"><span data-stu-id="5178c-205">However, 3xx status codes are not permitted.</span></span> |
| <span data-ttu-id="5178c-206">Rubriker</span><span class="sxs-lookup"><span data-stu-id="5178c-206">headers</span></span> |<span data-ttu-id="5178c-207">Definierar valfritt antal huvuden ska ingå i svaret.</span><span class="sxs-lookup"><span data-stu-id="5178c-207">Defines any number of headers to include in the response.</span></span> |
| <span data-ttu-id="5178c-208">Brödtext</span><span class="sxs-lookup"><span data-stu-id="5178c-208">body</span></span> |<span data-ttu-id="5178c-209">Anger ett body-objekt som kan vara en sträng, en JSON-objekt eller även binära innehåll som refereras från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="5178c-209">Specifies a body object that can be a string, a JSON object, or even binary content referenced from a previous step.</span></span> |

<span data-ttu-id="5178c-210">Här är JSON-schema ser ut nu för den **svar** åtgärd:</span><span class="sxs-lookup"><span data-stu-id="5178c-210">Here's what the JSON schema looks like now for the **Response** action:</span></span>

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
> <span data-ttu-id="5178c-211">Välj för att visa fullständiga JSON-definitionen för din logikapp på logiken App Designer **vyn kod**.</span><span class="sxs-lookup"><span data-stu-id="5178c-211">To view the complete JSON definition for your logic app, on the Logic App Designer, choose **Code view**.</span></span>

## <a name="q--a"></a><span data-ttu-id="5178c-212">Frågor och svar</span><span class="sxs-lookup"><span data-stu-id="5178c-212">Q & A</span></span>

#### <a name="q-what-about-url-security"></a><span data-ttu-id="5178c-213">F: Vad händer om URL-säkerhet?</span><span class="sxs-lookup"><span data-stu-id="5178c-213">Q: What about URL security?</span></span>

<span data-ttu-id="5178c-214">S: azure genererar på ett säkert sätt logik app återanrop URL: er med hjälp av en delad signatur åtkomst (SAS).</span><span class="sxs-lookup"><span data-stu-id="5178c-214">A: Azure securely generates logic app callback URLs using a Shared Access Signature (SAS).</span></span> <span data-ttu-id="5178c-215">Den här signaturen passerar som en frågeparameter och måste verifieras innan din logikapp kan utlösas.</span><span class="sxs-lookup"><span data-stu-id="5178c-215">This signature passes through as a query parameter and must be validated before your logic app can fire.</span></span> <span data-ttu-id="5178c-216">Azure skapar signaturen med en unik kombination av en hemlig nyckel per logikapp, Utlösarnamnet och åtgärden som utförs.</span><span class="sxs-lookup"><span data-stu-id="5178c-216">Azure generates the signature using a unique combination of a secret key per logic app, the trigger name, and the operation that's performed.</span></span> <span data-ttu-id="5178c-217">Så att de inte kan skapa en giltig signatur om någon har åtkomst till nyckeln hemliga logik app.</span><span class="sxs-lookup"><span data-stu-id="5178c-217">So unless someone has access to the secret logic app key, they cannot generate a valid signature.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="5178c-218">Produktions- och säkra system rekommenderar vi mot anropa logikappen direkt från webbläsaren eftersom:</span><span class="sxs-lookup"><span data-stu-id="5178c-218">For production and secure systems, we strongly recommend against calling your logic app directly from the browser because:</span></span>
   > 
   > * <span data-ttu-id="5178c-219">Den delade åtkomstnyckeln som visas i URL: en.</span><span class="sxs-lookup"><span data-stu-id="5178c-219">The shared access key appears in the URL.</span></span>
   > * <span data-ttu-id="5178c-220">Du kan inte hantera säker innehåll principer på grund av delade domäner över Logikapp kunder.</span><span class="sxs-lookup"><span data-stu-id="5178c-220">You can't manage secure content policies due to shared domains across Logic App customers.</span></span>

#### <a name="q-can-i-configure-http-endpoints-further"></a><span data-ttu-id="5178c-221">F: kan jag konfigurera HTTP-slutpunkter ytterligare?</span><span class="sxs-lookup"><span data-stu-id="5178c-221">Q: Can I configure HTTP endpoints further?</span></span>

<span data-ttu-id="5178c-222">S: Ja, HTTP-slutpunkter stöd för mer avancerad konfiguration via [ **API Management**](../api-management/api-management-key-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="5178c-222">A: Yes, HTTP endpoints support more advanced configuration through [**API Management**](../api-management/api-management-key-concepts.md).</span></span> <span data-ttu-id="5178c-223">Den här tjänsten erbjuder också möjligheten för du hantera alla dina API: er, inklusive logikappar, konfigurera anpassade domännamn, till exempel använda flera autentiseringsmetoder och mycket mer konsekvent:</span><span class="sxs-lookup"><span data-stu-id="5178c-223">This service also offers the capability for you to consistently manage all your APIs, including logic apps, set up custom domain names, use more authentication methods, and more, for example:</span></span>

* [<span data-ttu-id="5178c-224">Ändra metod för begäran</span><span class="sxs-lookup"><span data-stu-id="5178c-224">Change the request method</span></span>](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#SetRequestMethod)
* [<span data-ttu-id="5178c-225">Ändra URL-segment i begäran</span><span class="sxs-lookup"><span data-stu-id="5178c-225">Change the URL segments of the request</span></span>](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#RewriteURL)
* <span data-ttu-id="5178c-226">Ställ in din API Management-domäner i den [Azure-portalen](https://portal.azure.com/ "Azure-portalen")</span><span class="sxs-lookup"><span data-stu-id="5178c-226">Set up your API Management domains in the [Azure portal](https://portal.azure.com/ "Azure portal")</span></span>
* <span data-ttu-id="5178c-227">Konfigurera princip för att söka efter grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="5178c-227">Set up policy to check for Basic authentication</span></span>

#### <a name="q-what-changed-when-the-schema-migrated-from-the-december-1-2014-preview"></a><span data-ttu-id="5178c-228">F: vad ändrats när schemat migreras från 1 December 2014 preview?</span><span class="sxs-lookup"><span data-stu-id="5178c-228">Q: What changed when the schema migrated from the December 1, 2014 preview?</span></span>

<span data-ttu-id="5178c-229">S: här är en sammanfattning om dessa ändringar:</span><span class="sxs-lookup"><span data-stu-id="5178c-229">A: Here's a summary about these changes:</span></span>

| <span data-ttu-id="5178c-230">1 december 2014 preview</span><span class="sxs-lookup"><span data-stu-id="5178c-230">December 1, 2014 preview</span></span> | <span data-ttu-id="5178c-231">Den 1 juni 2016</span><span class="sxs-lookup"><span data-stu-id="5178c-231">June 1, 2016</span></span> |
| --- | --- |
| <span data-ttu-id="5178c-232">Klicka på **HTTP-lyssnaren** API-App</span><span class="sxs-lookup"><span data-stu-id="5178c-232">Click **HTTP Listener** API App</span></span> |<span data-ttu-id="5178c-233">Klicka på **manuell utlösaren** (ingen API-App krävs)</span><span class="sxs-lookup"><span data-stu-id="5178c-233">Click **Manual trigger** (no API App required)</span></span> |
| <span data-ttu-id="5178c-234">HTTP-lyssnaren inställningen ”*skickar svar automatiskt*”</span><span class="sxs-lookup"><span data-stu-id="5178c-234">HTTP Listener setting "*Sends response automatically*"</span></span> |<span data-ttu-id="5178c-235">Innehåller en **svar** åtgärd eller inte i arbetsflödesdefinitionen</span><span class="sxs-lookup"><span data-stu-id="5178c-235">Either include a **Response** action or not in the workflow definition</span></span> |
| <span data-ttu-id="5178c-236">Konfigurera grundläggande eller OAuth-autentisering</span><span class="sxs-lookup"><span data-stu-id="5178c-236">Configure Basic or OAuth authentication</span></span> |<span data-ttu-id="5178c-237">via API-hantering</span><span class="sxs-lookup"><span data-stu-id="5178c-237">via API Management</span></span> |
| <span data-ttu-id="5178c-238">Konfigurera HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="5178c-238">Configure HTTP method</span></span> |<span data-ttu-id="5178c-239">Under **visa avancerade alternativ**, Välj en HTTP-metod</span><span class="sxs-lookup"><span data-stu-id="5178c-239">Under **Show advanced options**, choose an HTTP method</span></span> |
| <span data-ttu-id="5178c-240">Konfigurera relativ sökväg</span><span class="sxs-lookup"><span data-stu-id="5178c-240">Configure relative path</span></span> |<span data-ttu-id="5178c-241">Under **visa avancerade alternativ**, lägga till en relativ sökväg</span><span class="sxs-lookup"><span data-stu-id="5178c-241">Under **Show advanced options**, add a relative path</span></span> |
| <span data-ttu-id="5178c-242">Referera till inkommande brödtexten via`@triggerOutputs().body.Content`</span><span class="sxs-lookup"><span data-stu-id="5178c-242">Reference the incoming body through `@triggerOutputs().body.Content`</span></span> |<span data-ttu-id="5178c-243">Referens till`@triggerOutputs().body`</span><span class="sxs-lookup"><span data-stu-id="5178c-243">Reference through `@triggerOutputs().body`</span></span> |
| <span data-ttu-id="5178c-244">**Skicka HTTP-svar** åtgärd på HTTP-lyssnare</span><span class="sxs-lookup"><span data-stu-id="5178c-244">**Send HTTP response** action on the HTTP Listener</span></span> |<span data-ttu-id="5178c-245">Klicka på **svarar på HTTP-begäran** (ingen API-App krävs)</span><span class="sxs-lookup"><span data-stu-id="5178c-245">Click **Respond to HTTP request** (no API App required)</span></span> |

## <a name="get-help"></a><span data-ttu-id="5178c-246">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="5178c-246">Get help</span></span>

<span data-ttu-id="5178c-247">I [Azure Logic Apps-forumet](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) kan du ställa frågor, besvara frågor och se vad andra Azure Logic Apps-användare håller på med.</span><span class="sxs-lookup"><span data-stu-id="5178c-247">To ask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit the [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="5178c-248">På [webbplatsen för Azure Logic Apps-användarfeedback](http://aka.ms/logicapps-wish) kan du hjälpa till med att förbättra Azure Logic Apps och anslutningsapparna genom att rösta på förslag eller komma med egna förslag på förbättringar.</span><span class="sxs-lookup"><span data-stu-id="5178c-248">To help improve Azure Logic Apps and connectors, vote on or submit ideas at the [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5178c-249">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5178c-249">Next steps</span></span>

* [<span data-ttu-id="5178c-250">Skapa Logic App-definitioner</span><span class="sxs-lookup"><span data-stu-id="5178c-250">Author logic app definitions</span></span>](./logic-apps-author-definitions.md)
* [<span data-ttu-id="5178c-251">Hantera fel och undantag</span><span class="sxs-lookup"><span data-stu-id="5178c-251">Handle errors and exceptions</span></span>](./logic-apps-exception-handling.md)

[1]: ./media/logic-apps-http-endpoint/manualtrigger.png
[2]: ./media/logic-apps-http-endpoint/manualtriggerurl.png
[3]: ./media/logic-apps-http-endpoint/response.png
