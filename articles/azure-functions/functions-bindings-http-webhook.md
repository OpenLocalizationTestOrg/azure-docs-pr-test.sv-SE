---
title: Azure Functions HTTP och webhook bindningar | Microsoft Docs
description: "Förstå hur du använder HTTP och webhook utlösare och bindningar i Azure Functions."
services: functions
documentationcenter: na
author: mattchenderson
manager: erikre
editor: 
tags: 
keywords: "Azure functions, funktioner, händelse bearbetning, webhooks, dynamiska beräknings-, serverlösa arkitektur, HTTP, API REST"
ms.assetid: 2b12200d-63d8-4ec1-9da8-39831d5a51b1
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/18/2016
ms.author: mahender
ms.openlocfilehash: 71c0d22c4b1824078982b9d1cc76645f947ae603
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-functions-http-and-webhook-bindings"></a><span data-ttu-id="b8dba-104">Azure Functions HTTP och webhook bindningar</span><span class="sxs-lookup"><span data-stu-id="b8dba-104">Azure Functions HTTP and webhook bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="b8dba-105">Den här artikeln beskriver hur du konfigurerar och arbetar med HTTP-utlösare och bindningar i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="b8dba-105">This article explains how to configure and work with HTTP triggers and bindings in Azure Functions.</span></span>
<span data-ttu-id="b8dba-106">Med dessa kan använda du Azure Functions för att bygga serverlösa API: er och svara på webhooks.</span><span class="sxs-lookup"><span data-stu-id="b8dba-106">With these, you can use Azure Functions to build serverless APIs and respond to webhooks.</span></span>

<span data-ttu-id="b8dba-107">Azure Functions erbjuder följande Bindningar:</span><span class="sxs-lookup"><span data-stu-id="b8dba-107">Azure Functions provides the following bindings:</span></span>
- <span data-ttu-id="b8dba-108">En [HTTP-utlösaren](#httptrigger) kan du anropa en funktion med en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="b8dba-108">An [HTTP trigger](#httptrigger) lets you invoke a function with an HTTP request.</span></span> <span data-ttu-id="b8dba-109">Detta kan anpassas för att svara på [webhooks](#hooktrigger).</span><span class="sxs-lookup"><span data-stu-id="b8dba-109">This can be customized to respond to [webhooks](#hooktrigger).</span></span>
- <span data-ttu-id="b8dba-110">En [HTTP-utdatabindning](#output) kan du svara på begäran.</span><span class="sxs-lookup"><span data-stu-id="b8dba-110">An [HTTP output binding](#output) allows you to respond to the request.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a><span data-ttu-id="b8dba-111">HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="b8dba-111">HTTP trigger</span></span>
<span data-ttu-id="b8dba-112">HTTP-utlösaren körs din funktion som svar på en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="b8dba-112">The HTTP trigger will execute your function in response to an HTTP request.</span></span> <span data-ttu-id="b8dba-113">Du kan anpassa den för att svara på en viss URL eller uppsättning HTTP-metoderna.</span><span class="sxs-lookup"><span data-stu-id="b8dba-113">You can customize it to respond to a particular URL or set of HTTP methods.</span></span> <span data-ttu-id="b8dba-114">En HTTP-utlösare kan också konfigureras för att svara på webhooks.</span><span class="sxs-lookup"><span data-stu-id="b8dba-114">An HTTP trigger can also be configured to respond to webhooks.</span></span> 

<span data-ttu-id="b8dba-115">Om du använder Functions-portalen kan du också sätta igång direkt med en fördefinierad mall.</span><span class="sxs-lookup"><span data-stu-id="b8dba-115">If using the Functions portal, you can also get started right away using a pre-made template.</span></span> <span data-ttu-id="b8dba-116">Välj **nya funktionen** och välj ”API och Webhooks” den **scenariot** listrutan.</span><span class="sxs-lookup"><span data-stu-id="b8dba-116">Select **New function** and choose "API & Webhooks" from the **Scenario** dropdown.</span></span> <span data-ttu-id="b8dba-117">Välj en av mallar och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b8dba-117">Select one of the templates and click **Create**.</span></span>

<span data-ttu-id="b8dba-118">Som standard svarar ett HTTP-utlösaren på begäran med ett 200 OK HTTP-statuskoden och en brödtext.</span><span class="sxs-lookup"><span data-stu-id="b8dba-118">By default, an HTTP trigger will respond to the request with an HTTP 200 OK status code and an empty body.</span></span> <span data-ttu-id="b8dba-119">Om du vill ändra svarstypen, konfigurera en [HTTP-utdatabindning](#output)</span><span class="sxs-lookup"><span data-stu-id="b8dba-119">To modify the response, configure an [HTTP output binding](#output)</span></span>

### <a name="configuring-an-http-trigger"></a><span data-ttu-id="b8dba-120">Konfigurera en HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="b8dba-120">Configuring an HTTP trigger</span></span>
<span data-ttu-id="b8dba-121">En HTTP-utlösare definieras genom att inkludera en JSON-objekt som liknar följande i den `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="b8dba-121">An HTTP trigger is defined by including a JSON object similar to the following in the `bindings` array of function.json:</span></span>

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function",
    "methods": [ "get" ],
    "route": "values/{id}"
},
```
<span data-ttu-id="b8dba-122">Bindningen stöder följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="b8dba-122">The binding supports the following properties:</span></span>

* <span data-ttu-id="b8dba-123">**namnet** : krävs – variabelnamnet som används i Funktionskoden för begäran eller begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="b8dba-123">**name** : Required - the variable name used in function code for the request or request body.</span></span> <span data-ttu-id="b8dba-124">Se [arbetar med en HTTP-utlösare från kod](#httptriggerusage).</span><span class="sxs-lookup"><span data-stu-id="b8dba-124">See [Working with an HTTP trigger from code](#httptriggerusage).</span></span>
* <span data-ttu-id="b8dba-125">**typen** : krävs – måste vara inställd på ”httpTrigger”.</span><span class="sxs-lookup"><span data-stu-id="b8dba-125">**type** : Required - must be set to "httpTrigger".</span></span>
* <span data-ttu-id="b8dba-126">**riktning** : krävs – måste vara inställd på ”i”.</span><span class="sxs-lookup"><span data-stu-id="b8dba-126">**direction** : Required - must be set to "in".</span></span>
* <span data-ttu-id="b8dba-127">_authLevel_ : Detta avgör vad nycklar, eventuella måste finnas på begäran för att anropa funktionen.</span><span class="sxs-lookup"><span data-stu-id="b8dba-127">_authLevel_ : This determines what keys, if any, need to be present on the request in order to invoke the function.</span></span> <span data-ttu-id="b8dba-128">Se [arbeta med nycklar](#keys) nedan.</span><span class="sxs-lookup"><span data-stu-id="b8dba-128">See [Working with keys](#keys) below.</span></span> <span data-ttu-id="b8dba-129">Värdet kan vara något av följande:</span><span class="sxs-lookup"><span data-stu-id="b8dba-129">The value can be one of the following:</span></span>
    * <span data-ttu-id="b8dba-130">_anonym_: Nej API-nyckeln är obligatorisk.</span><span class="sxs-lookup"><span data-stu-id="b8dba-130">_anonymous_: No API key is required.</span></span>
    * <span data-ttu-id="b8dba-131">_funktionen_: en funktionsspecifika API-nyckel krävs.</span><span class="sxs-lookup"><span data-stu-id="b8dba-131">_function_: A function-specific API key is required.</span></span> <span data-ttu-id="b8dba-132">Detta är standardvärdet om ingen anges.</span><span class="sxs-lookup"><span data-stu-id="b8dba-132">This is the default value if none is provided.</span></span>
    * <span data-ttu-id="b8dba-133">_Admin_ : huvudnyckeln krävs.</span><span class="sxs-lookup"><span data-stu-id="b8dba-133">_admin_ : The master key is required.</span></span>
* <span data-ttu-id="b8dba-134">**metoder** : Detta är en matris med HTTP-metoderna som funktionen kommer att svara.</span><span class="sxs-lookup"><span data-stu-id="b8dba-134">**methods** : This is an array of the HTTP methods to which the function will respond.</span></span> <span data-ttu-id="b8dba-135">Om den inte anges kommer funktionen besvara alla HTTP-metoderna.</span><span class="sxs-lookup"><span data-stu-id="b8dba-135">If not specified, the function will respond to all HTTP methods.</span></span> <span data-ttu-id="b8dba-136">Se [anpassa HTTP-slutpunkten](#url).</span><span class="sxs-lookup"><span data-stu-id="b8dba-136">See [Customizing the HTTP endpoint](#url).</span></span>
* <span data-ttu-id="b8dba-137">**väg** : Detta definierar flödesmallen, kontrollera begära webbadresserna som funktionen kommer att svara.</span><span class="sxs-lookup"><span data-stu-id="b8dba-137">**route** : This defines the route template, controlling to which request URLs your function will respond.</span></span> <span data-ttu-id="b8dba-138">Standardvärdet om ingen anges är `<functionname>`.</span><span class="sxs-lookup"><span data-stu-id="b8dba-138">The default value if none is provided is `<functionname>`.</span></span> <span data-ttu-id="b8dba-139">Se [anpassa HTTP-slutpunkten](#url).</span><span class="sxs-lookup"><span data-stu-id="b8dba-139">See [Customizing the HTTP endpoint](#url).</span></span>
* <span data-ttu-id="b8dba-140">**webHookType** : Detta konfigurerar HTTP-trigger för att fungera som en webhook reciever för den angivna providern.</span><span class="sxs-lookup"><span data-stu-id="b8dba-140">**webHookType** : This configures the HTTP trigger to act as a webhook reciever for the specified provider.</span></span> <span data-ttu-id="b8dba-141">Den _metoder_ egenskapen ska inte anges om detta är valt.</span><span class="sxs-lookup"><span data-stu-id="b8dba-141">The _methods_ property should not be set if this is chosen.</span></span> <span data-ttu-id="b8dba-142">Se [svarar på webhooks](#hooktrigger).</span><span class="sxs-lookup"><span data-stu-id="b8dba-142">See [Responding to webhooks](#hooktrigger).</span></span> <span data-ttu-id="b8dba-143">Värdet kan vara något av följande:</span><span class="sxs-lookup"><span data-stu-id="b8dba-143">The value can be one of the following:</span></span>
    * <span data-ttu-id="b8dba-144">_genericJson_ : en generella webhook slutpunkt utan logik för en specifik provider.</span><span class="sxs-lookup"><span data-stu-id="b8dba-144">_genericJson_ : A general purpose webhook endpoint without logic for a specific provider.</span></span>
    * <span data-ttu-id="b8dba-145">_github_ : funktionen ska svara på GitHub webhooks.</span><span class="sxs-lookup"><span data-stu-id="b8dba-145">_github_ : The function will respond to GitHub webhooks.</span></span> <span data-ttu-id="b8dba-146">Den _authLevel_ egenskapen ska inte anges om detta är valt.</span><span class="sxs-lookup"><span data-stu-id="b8dba-146">The _authLevel_ property should not be set if this is chosen.</span></span>
    * <span data-ttu-id="b8dba-147">_slack_ : funktionen ska svara på Slack webhooks.</span><span class="sxs-lookup"><span data-stu-id="b8dba-147">_slack_ : The function will respond to Slack webhooks.</span></span> <span data-ttu-id="b8dba-148">Den _authLevel_ egenskapen ska inte anges om detta är valt.</span><span class="sxs-lookup"><span data-stu-id="b8dba-148">The _authLevel_ property should not be set if this is chosen.</span></span>

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a><span data-ttu-id="b8dba-149">Arbeta med en HTTP-utlösare från kod</span><span class="sxs-lookup"><span data-stu-id="b8dba-149">Working with an HTTP trigger from code</span></span>
<span data-ttu-id="b8dba-150">För C# och F #, kan du deklarera typen av din utlösare som indata till antingen `HttpRequestMessage` eller en anpassad typ.</span><span class="sxs-lookup"><span data-stu-id="b8dba-150">For C# and F# functions, you can declare the type of your trigger input to be either `HttpRequestMessage` or a custom type.</span></span> <span data-ttu-id="b8dba-151">Om du väljer `HttpRequestMessage`, och du får fullständig åtkomst till request-objektet.</span><span class="sxs-lookup"><span data-stu-id="b8dba-151">If you choose `HttpRequestMessage`, then you will get full access to the request object.</span></span> <span data-ttu-id="b8dba-152">För en anpassad typ (till exempel en POCO) försöker funktioner parsa begärandetexten som JSON att fylla i objektets egenskaper.</span><span class="sxs-lookup"><span data-stu-id="b8dba-152">For a custom type (such as a POCO), Functions will attempt to parse the request body as JSON to populate the object properties.</span></span>

<span data-ttu-id="b8dba-153">För Node.js-funktion ger Functions-runtime begärandetexten i stället för request-objektet.</span><span class="sxs-lookup"><span data-stu-id="b8dba-153">For Node.js functions, the Functions runtime provides the request body instead of the request object.</span></span>

<span data-ttu-id="b8dba-154">Se [http-utlösaren exempel](#httptriggersample) till exempel användningsområden.</span><span class="sxs-lookup"><span data-stu-id="b8dba-154">See [HTTP trigger samples](#httptriggersample) for example usages.</span></span>


<a name="output"></a>
## <a name="http-response-output-binding"></a><span data-ttu-id="b8dba-155">HTTP-svar utdatabindning</span><span class="sxs-lookup"><span data-stu-id="b8dba-155">HTTP response output binding</span></span>
<span data-ttu-id="b8dba-156">Använd HTTP-utdata bindning svarar på http-begäran avsändaren.</span><span class="sxs-lookup"><span data-stu-id="b8dba-156">Use the HTTP output binding to respond to the HTTP request sender.</span></span> <span data-ttu-id="b8dba-157">Den här bindningen kräver en HTTP-utlösare och kan du anpassa svaret som är associerade med utlösarens begäran.</span><span class="sxs-lookup"><span data-stu-id="b8dba-157">This binding requires an HTTP trigger and allows you to customize the response associated with the trigger's request.</span></span> <span data-ttu-id="b8dba-158">Om en HTTP-utdatabindning anges inte är en HTTP-utlösare returneras HTTP 200 OK utan en brödtext.</span><span class="sxs-lookup"><span data-stu-id="b8dba-158">If an HTTP output binding is not provided, an HTTP trigger will return HTTP 200 OK with an empty body.</span></span> 

### <a name="configuring-an-http-output-binding"></a><span data-ttu-id="b8dba-159">Konfigurera en HTTP-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="b8dba-159">Configuring an HTTP output binding</span></span>
<span data-ttu-id="b8dba-160">HTTP utdata bindning definieras genom att inkludera en JSON-objekt som liknar följande i den `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="b8dba-160">The HTTP output binding is defined by including a JSON object similar to the following in the `bindings` array of function.json:</span></span>

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
<span data-ttu-id="b8dba-161">Bindningen innehåller följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="b8dba-161">The binding contains the following properties:</span></span>

* <span data-ttu-id="b8dba-162">**namnet** : krävs – variabelnamnet som används i Funktionskoden för svaret.</span><span class="sxs-lookup"><span data-stu-id="b8dba-162">**name** : Required - the variable name used in function code for the response.</span></span> <span data-ttu-id="b8dba-163">Se [arbetar med en HTTP-utdatabindning från kod](#outputusage).</span><span class="sxs-lookup"><span data-stu-id="b8dba-163">See [Working with an HTTP output binding from code](#outputusage).</span></span>
* <span data-ttu-id="b8dba-164">**typen** : krävs – måste vara inställd på ”http”.</span><span class="sxs-lookup"><span data-stu-id="b8dba-164">**type** : Required - must be set to "http".</span></span>
* <span data-ttu-id="b8dba-165">**riktning** : krävs – måste vara inställd på ”out”.</span><span class="sxs-lookup"><span data-stu-id="b8dba-165">**direction** : Required - must be set to "out".</span></span>

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a><span data-ttu-id="b8dba-166">Arbeta med en HTTP-utdatabindning från kod</span><span class="sxs-lookup"><span data-stu-id="b8dba-166">Working with an HTTP output binding from code</span></span>
<span data-ttu-id="b8dba-167">Du kan använda output-parameter (t.ex. ”res”) svarar på http- eller webhook anroparen.</span><span class="sxs-lookup"><span data-stu-id="b8dba-167">You can use the output parameter (e.g., "res") to respond to the http or webhook caller.</span></span> <span data-ttu-id="b8dba-168">Du kan också använda standarden `Request.CreateResponse()` (C#) eller `context.res` (Node.JS) mönster för att returnera ditt svar.</span><span class="sxs-lookup"><span data-stu-id="b8dba-168">Alternatively, you can use the standard `Request.CreateResponse()` (C#) or `context.res` (Node.JS) pattern to return your response.</span></span> <span data-ttu-id="b8dba-169">Exempel på hur du använder den andra metoden finns i [http-utlösaren prover](#httptriggersample) och [Webhook utlösaren exempel](#hooktriggersample).</span><span class="sxs-lookup"><span data-stu-id="b8dba-169">For examples on how to use the latter method, see [HTTP trigger samples](#httptriggersample) and [Webhook trigger samples](#hooktriggersample).</span></span>


<a name="hooktrigger"></a>
## <a name="responding-to-webhooks"></a><span data-ttu-id="b8dba-170">Svara på webhooks</span><span class="sxs-lookup"><span data-stu-id="b8dba-170">Responding to webhooks</span></span>
<span data-ttu-id="b8dba-171">En HTTP-utlösare med den _webHookType_ egenskapen kommer att konfigureras för att svara på [webhooks](https://en.wikipedia.org/wiki/Webhook).</span><span class="sxs-lookup"><span data-stu-id="b8dba-171">An HTTP trigger with the _webHookType_ property will be configured to respond to [webhooks](https://en.wikipedia.org/wiki/Webhook).</span></span> <span data-ttu-id="b8dba-172">Den grundläggande konfigurationen använder inställningen ”genericJson”.</span><span class="sxs-lookup"><span data-stu-id="b8dba-172">The basic configuration uses the "genericJson" setting.</span></span> <span data-ttu-id="b8dba-173">Detta begränsar begäranden till endast de som använder HTTP POST och med den `application/json` innehållstyp.</span><span class="sxs-lookup"><span data-stu-id="b8dba-173">This restricts requests to only those using HTTP POST and with the `application/json` content type.</span></span>

<span data-ttu-id="b8dba-174">Utlösaren skräddarsys dessutom till en specifik webhook-provider (t.ex. [GitHub](https://developer.github.com/webhooks/) och [Slack](https://api.slack.com/outgoing-webhooks)).</span><span class="sxs-lookup"><span data-stu-id="b8dba-174">The trigger can additionally be tailored to a specific webhook provider (e.g., [GitHub](https://developer.github.com/webhooks/) and [Slack](https://api.slack.com/outgoing-webhooks)).</span></span> <span data-ttu-id="b8dba-175">Om en provider anges kan Functions-runtime ta hand om leverantörens valideringslogik åt dig.</span><span class="sxs-lookup"><span data-stu-id="b8dba-175">If a provider is specified, the Functions runtime can take care of the provider's validation logic for you.</span></span>  

### <a name="configuring-github-as-a-webhook-provider"></a><span data-ttu-id="b8dba-176">Konfigurera GitHub som en webhook-provider</span><span class="sxs-lookup"><span data-stu-id="b8dba-176">Configuring GitHub as a webhook provider</span></span>
<span data-ttu-id="b8dba-177">För att svara på GitHub webhooks, först skapa din funktion med en HTTP-utlösare och ange den _webHookType_ egenskapen till ”github”.</span><span class="sxs-lookup"><span data-stu-id="b8dba-177">To respond to GitHub webhooks, first create your function with an HTTP Trigger, and set the _webHookType_ property to "github".</span></span> <span data-ttu-id="b8dba-178">Kopiera sedan dess [URL](#url) och [API-nyckel](#keys) till dina GitHub-lagringsplatsen **lägga till webhook** sidan.</span><span class="sxs-lookup"><span data-stu-id="b8dba-178">Then copy its [URL](#url) and [API key](#keys) into your GitHub repository's **Add webhook** page.</span></span> <span data-ttu-id="b8dba-179">Se Githubs [skapar Webhooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) dokumentationen för mer information.</span><span class="sxs-lookup"><span data-stu-id="b8dba-179">See GitHub's [Creating Webhooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) documentation for more.</span></span>

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

### <a name="configuring-slack-as-a-webhook-provider"></a><span data-ttu-id="b8dba-180">Konfigurera Slack som en webhook-provider</span><span class="sxs-lookup"><span data-stu-id="b8dba-180">Configuring Slack as a webhook provider</span></span>
<span data-ttu-id="b8dba-181">Slack webhooken genererar en token för du i stället för där du kan ange den, så måste du konfigurera en funktionsspecifika nyckel med token från Slack.</span><span class="sxs-lookup"><span data-stu-id="b8dba-181">The Slack webhook generates a token for you instead of letting you specify it, so you must configure a function-specific key with the token from Slack.</span></span> <span data-ttu-id="b8dba-182">Se [arbeta med nycklar](#keys).</span><span class="sxs-lookup"><span data-stu-id="b8dba-182">See [Working with keys](#keys).</span></span>

<a name="url"></a>
## <a name="customizing-the-http-endpoint"></a><span data-ttu-id="b8dba-183">Anpassa HTTP-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="b8dba-183">Customizing the HTTP endpoint</span></span>
<span data-ttu-id="b8dba-184">Som standard när du skapar en funktion för ett HTTP-utlösare eller WebHook, är funktionen adresserbara med en väg i formatet:</span><span class="sxs-lookup"><span data-stu-id="b8dba-184">By default when you create a function for an HTTP trigger, or WebHook, the function is addressable with a route of the form:</span></span>

    http://<yourapp>.azurewebsites.net/api/<funcname> 

<span data-ttu-id="b8dba-185">Du kan anpassa den här vägen med det valfria `route` egenskapen för HTTP-utlösaren input bindning.</span><span class="sxs-lookup"><span data-stu-id="b8dba-185">You can customize this route using the optional `route` property on the HTTP trigger's input binding.</span></span> <span data-ttu-id="b8dba-186">Följande exempel *function.json* filen definierar en `route` för en HTTP-utlösare:</span><span class="sxs-lookup"><span data-stu-id="b8dba-186">As an example, the following *function.json* file defines a `route` property for an HTTP trigger:</span></span>

```json
    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }
```

<span data-ttu-id="b8dba-187">Med den här konfigurationen kan är funktionen nu adresserbara med följande vägen i stället för det ursprungliga flödet.</span><span class="sxs-lookup"><span data-stu-id="b8dba-187">Using this configuration, the function is now addressable with the following route instead of the original route.</span></span>

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

<span data-ttu-id="b8dba-188">Detta gör att Funktionskoden stöder två parametrar i adress, ”kategorier” och ”id”.</span><span class="sxs-lookup"><span data-stu-id="b8dba-188">This allows the function code to support two parameters in the address, "category" and "id".</span></span> <span data-ttu-id="b8dba-189">Du kan använda någon [Web API-väg begränsningen](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) med parametrarna.</span><span class="sxs-lookup"><span data-stu-id="b8dba-189">You can use any [Web API Route Constraint](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) with your parameters.</span></span> <span data-ttu-id="b8dba-190">Följande C# Funktionskoden använder båda parametrarna.</span><span class="sxs-lookup"><span data-stu-id="b8dba-190">The following C# function code makes use of both parameters.</span></span>

```csharp
    public static Task<HttpResponseMessage> Run(HttpRequestMessage req, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }
```

<span data-ttu-id="b8dba-191">Här är Node.js funktionskod för att använda samma vägparametrar.</span><span class="sxs-lookup"><span data-stu-id="b8dba-191">Here is Node.js function code to use the same route parameters.</span></span>

```javascript
    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;

        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }

        context.done();
    } 
```

<span data-ttu-id="b8dba-192">Som standard alla funktionen vägar föregås *api*.</span><span class="sxs-lookup"><span data-stu-id="b8dba-192">By default, all function routes are prefixed with *api*.</span></span> <span data-ttu-id="b8dba-193">Du kan också anpassa eller ta bort prefix med hjälp av den `http.routePrefix` egenskap i din *host.json* fil.</span><span class="sxs-lookup"><span data-stu-id="b8dba-193">You can also customize or remove the prefix using the `http.routePrefix` property in your *host.json* file.</span></span> <span data-ttu-id="b8dba-194">I följande exempel tar bort den *api* väg prefix genom att använda en tom sträng för prefix i den *host.json* fil.</span><span class="sxs-lookup"><span data-stu-id="b8dba-194">The following example removes the *api* route prefix by using an empty string for the prefix in the *host.json* file.</span></span>

```json
    {
      "http": {
        "routePrefix": ""
      }
    }
```

<span data-ttu-id="b8dba-195">Detaljerad information om hur du uppdaterar den *host.json* filen för din funktion finns [hur du uppdaterar funktionen programfilerna](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="b8dba-195">For detailed information on how to update the *host.json* file for your function, See, [How to update function app files](functions-reference.md#fileupdate).</span></span> 

<span data-ttu-id="b8dba-196">Mer information om andra egenskaper som du kan konfigurera i din *host.json* fil, se [host.json referens](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="b8dba-196">For information on other properties you can configure in your *host.json* file, see [host.json reference](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>


<a name="keys"></a>
## <a name="working-with-keys"></a><span data-ttu-id="b8dba-197">Arbeta med nycklar</span><span class="sxs-lookup"><span data-stu-id="b8dba-197">Working with keys</span></span>
<span data-ttu-id="b8dba-198">HttpTriggers kan utnyttja nycklar för extra säkerhet.</span><span class="sxs-lookup"><span data-stu-id="b8dba-198">HttpTriggers can leverage keys for added security.</span></span> <span data-ttu-id="b8dba-199">Standard HttpTrigger kan använda dem som en API-nyckel som kräver nyckeln måste finnas på begäran.</span><span class="sxs-lookup"><span data-stu-id="b8dba-199">A standard HttpTrigger can use these as an API key, requiring the key to be present on the request.</span></span> <span data-ttu-id="b8dba-200">Webhooks kan använda för att godkänna begäranden i en mängd olika sätt beroende på vad providern stöder.</span><span class="sxs-lookup"><span data-stu-id="b8dba-200">Webhooks can use keys to authorize requests in a variety of ways, depending on what the provider supports.</span></span>

<span data-ttu-id="b8dba-201">Nycklar lagras som en del av din funktionsapp i Azure och krypterat i vila.</span><span class="sxs-lookup"><span data-stu-id="b8dba-201">Keys are stored as part of your function app in Azure and are encrypted at rest.</span></span> <span data-ttu-id="b8dba-202">Om du vill visa dina nycklar, skapa nya eller återställa nycklar till nya värden, navigera till en av dina funktioner i portalen och välj ”hantera”.</span><span class="sxs-lookup"><span data-stu-id="b8dba-202">To view your keys, create new ones, or roll keys to new values, navigate to one of your functions within the portal and select "Manage."</span></span> 

<span data-ttu-id="b8dba-203">Det finns två typer av nycklar:</span><span class="sxs-lookup"><span data-stu-id="b8dba-203">There are two types of keys:</span></span>
- <span data-ttu-id="b8dba-204">**Värdnycklar**: nycklarna delas av alla funktioner i appen funktion.</span><span class="sxs-lookup"><span data-stu-id="b8dba-204">**Host keys**: These keys are shared by all functions within the function app.</span></span> <span data-ttu-id="b8dba-205">När det används som en API-nyckel, Tillåt dessa åtkomst till en funktion i funktionen appen.</span><span class="sxs-lookup"><span data-stu-id="b8dba-205">When used as an API key, these allow access to any function within the function app.</span></span>
- <span data-ttu-id="b8dba-206">**Funktionstangenter**: nycklarna gäller endast för specifika funktioner som de har definierats.</span><span class="sxs-lookup"><span data-stu-id="b8dba-206">**Function keys**: These keys apply only to the specific functions under which they are defined.</span></span> <span data-ttu-id="b8dba-207">När det används som en API-nyckel, tillåta dessa endast åtkomst till den funktionen.</span><span class="sxs-lookup"><span data-stu-id="b8dba-207">When used as an API key, these only allow access to that function.</span></span>

<span data-ttu-id="b8dba-208">Varje nyckel som heter referens och det finns en standard-nyckel (med namnet ”standard”) på funktionen och värden.</span><span class="sxs-lookup"><span data-stu-id="b8dba-208">Each key is named for reference, and there is a default key (named "default") at the function and host level.</span></span> <span data-ttu-id="b8dba-209">Den **huvudnyckeln** är en standard värd-nyckel med namnet ”_master” som har definierats för varje funktionsapp och det går inte att återkallas.</span><span class="sxs-lookup"><span data-stu-id="b8dba-209">The **master key** is a default host key named "_master" that is defined for each function app and cannot be revoked.</span></span> <span data-ttu-id="b8dba-210">Det ger administrativ åtkomst till runtime API: er.</span><span class="sxs-lookup"><span data-stu-id="b8dba-210">It provides administrative access to the runtime APIs.</span></span> <span data-ttu-id="b8dba-211">Med hjälp av `"authLevel": "admin"` i bindningen JSON kräver den här nyckeln som ska visas i begäran; andra nyckeln resulterar i ett autentiseringsfel.</span><span class="sxs-lookup"><span data-stu-id="b8dba-211">Using `"authLevel": "admin"` in the binding JSON will require this key to be presented on the request; any other key will result in a authorization failure.</span></span>

> [!NOTE]
> <span data-ttu-id="b8dba-212">På grund av de utökade behörigheter som tilldelats av huvudnyckeln, bör du inte dela den här nyckeln med tredje part eller distribuera den i native client-program.</span><span class="sxs-lookup"><span data-stu-id="b8dba-212">Due to the elevated permissions granted by the master key, you should not share this key with third parties or distribute it in native client applications.</span></span> <span data-ttu-id="b8dba-213">Var försiktig när du väljer admin åtkomstnivå.</span><span class="sxs-lookup"><span data-stu-id="b8dba-213">Exercise caution when choosing the admin authorization level.</span></span>
> 
> 

### <a name="api-key-authorization"></a><span data-ttu-id="b8dba-214">Auktorisering av innehållsnyckel API</span><span class="sxs-lookup"><span data-stu-id="b8dba-214">API key authorization</span></span>
<span data-ttu-id="b8dba-215">Som standard kräver en HttpTrigger en API-nyckel i HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="b8dba-215">By default, an HttpTrigger requires an API key in the HTTP request.</span></span> <span data-ttu-id="b8dba-216">HTTP-begäran ser så normalt ut så här:</span><span class="sxs-lookup"><span data-stu-id="b8dba-216">So your HTTP request normally looks like this:</span></span>

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

<span data-ttu-id="b8dba-217">Nyckeln kan ingå i en fråga string-variabel med namnet `code`, enligt ovan, eller det kan ingå i en `x-functions-key` HTTP-huvud.</span><span class="sxs-lookup"><span data-stu-id="b8dba-217">The key can be included in a query string variable named `code`, as above, or it can be included in an `x-functions-key` HTTP header.</span></span> <span data-ttu-id="b8dba-218">Värdet för nyckeln kan vara valfri Funktionstangent för som definierats för funktionen eller valfri tangent för värden.</span><span class="sxs-lookup"><span data-stu-id="b8dba-218">The value of the key can be any function key defined for the function, or any host key.</span></span>

<span data-ttu-id="b8dba-219">Du kan välja att tillåta begäranden utan nycklar eller ange att huvudnyckeln måste användas genom att ändra den `authLevel` egenskap i bindningen JSON (se [HTTP-utlösaren](#httptrigger)).</span><span class="sxs-lookup"><span data-stu-id="b8dba-219">You can choose to allow requests without keys or specify that the master key must be used by changing the `authLevel` property in the binding JSON (see [HTTP trigger](#httptrigger)).</span></span>

### <a name="keys-and-webhooks"></a><span data-ttu-id="b8dba-220">Nycklar och webhooks</span><span class="sxs-lookup"><span data-stu-id="b8dba-220">Keys and webhooks</span></span>
<span data-ttu-id="b8dba-221">Webhook-auktorisering hanteras av webhook reciever komponent, en del av HttpTrigger och mekanismen varierar beroende på vilken webhooken.</span><span class="sxs-lookup"><span data-stu-id="b8dba-221">Webhook authorization is handled by the webhook reciever component, part of the HttpTrigger, and the mechanism varies based on the webhook type.</span></span> <span data-ttu-id="b8dba-222">Varje mekanism matchar, men förlitar sig på en nyckel.</span><span class="sxs-lookup"><span data-stu-id="b8dba-222">Each mechanism does, however rely on a key.</span></span> <span data-ttu-id="b8dba-223">Funktionen nyckeln med namnet ”default” som standard kommer att användas.</span><span class="sxs-lookup"><span data-stu-id="b8dba-223">By default, the function key named "default" will be used.</span></span> <span data-ttu-id="b8dba-224">Om du vill använda en annan nyckel måste konfigurera webhook-providern för att skicka nyckelnamnet med förfrågan i något av följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b8dba-224">If you wish to use a different key, you will need to configure the webhook provider to send the key name with the request in one of the following ways:</span></span>

- <span data-ttu-id="b8dba-225">**Frågesträng**: providern skickar nyckelnamnet i den `clientid` frågesträngparametern (t.ex. `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).</span><span class="sxs-lookup"><span data-stu-id="b8dba-225">**Query string**: The provider passes the key name in the `clientid` query string parameter (e.g., `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).</span></span>
- <span data-ttu-id="b8dba-226">**Förfrågningshuvudet**: providern skickar nyckelnamnet i den `x-functions-clientid` rubrik.</span><span class="sxs-lookup"><span data-stu-id="b8dba-226">**Request header**: The provider passes the key name in the `x-functions-clientid` header.</span></span>

> [!NOTE]
> <span data-ttu-id="b8dba-227">Funktionstangenter företräde framför värdnycklar.</span><span class="sxs-lookup"><span data-stu-id="b8dba-227">Function keys take precedence over host keys.</span></span> <span data-ttu-id="b8dba-228">Om två nycklar har definierats med samma namn används funktionen nyckeln.</span><span class="sxs-lookup"><span data-stu-id="b8dba-228">If two keys are defined with the same name, the function key will be used.</span></span>
> 
> 


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a><span data-ttu-id="b8dba-229">HTTP-utlösaren prover</span><span class="sxs-lookup"><span data-stu-id="b8dba-229">HTTP trigger samples</span></span>
<span data-ttu-id="b8dba-230">Anta att du har följande HTTP-utlösaren den `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="b8dba-230">Suppose you have the following HTTP trigger in the `bindings` array of function.json:</span></span>

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

<span data-ttu-id="b8dba-231">I avsnittet språkspecifika exempel som söker efter en `name` parameter i frågesträngen eller brödtext för HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="b8dba-231">See the language-specific sample that looks for a `name` parameter either in the query string or the body of the HTTP request.</span></span>

* [<span data-ttu-id="b8dba-232">C#</span><span class="sxs-lookup"><span data-stu-id="b8dba-232">C#</span></span>](#httptriggercsharp)
* [<span data-ttu-id="b8dba-233">F#</span><span class="sxs-lookup"><span data-stu-id="b8dba-233">F#</span></span>](#httptriggerfsharp)
* [<span data-ttu-id="b8dba-234">Node.js</span><span class="sxs-lookup"><span data-stu-id="b8dba-234">Node.js</span></span>](#httptriggernodejs)


<a name="httptriggercsharp"></a>
### <a name="http-trigger-sample-in-c"></a><span data-ttu-id="b8dba-235">HTTP-utlösaren exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="b8dba-235">HTTP trigger sample in C#</span></span> #
```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

<span data-ttu-id="b8dba-236">Du kan också binda till en POCO i stället för `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="b8dba-236">You can also bind to a POCO instead of `HttpRequestMessage`.</span></span> <span data-ttu-id="b8dba-237">Detta kommer ur från brödtexten i begäran parsade som JSON.</span><span class="sxs-lookup"><span data-stu-id="b8dba-237">This will be hydrated from the body of the request, parsed as JSON.</span></span> <span data-ttu-id="b8dba-238">På samma sätt kan en typ som kan skickas till http-svarsutdata bindning och detta returneras som svarstexten med statuskod 200.</span><span class="sxs-lookup"><span data-stu-id="b8dba-238">Similarly, a type can be passed to the HTTP response output binding, and this will be returned as the response body, with a 200 status code.</span></span>
```csharp
using System.Net;
using System.Threading.Tasks;

public static string Run(CustomObject req, TraceWriter log)
{
    return "Hello " + req?.name;
}

public class CustomObject {
     public String name {get; set;}
}
}
```

<a name="httptriggerfsharp"></a>
### <a name="http-trigger-sample-in-f"></a><span data-ttu-id="b8dba-239">HTTP-utlösaren exemplet i F #</span><span class="sxs-lookup"><span data-stu-id="b8dba-239">HTTP trigger sample in F#</span></span> #
```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

<span data-ttu-id="b8dba-240">Du behöver en `project.json` fil som använder NuGet för att referera till den `FSharp.Interop.Dynamic` och `Dynamitey` sammansättningar, så här:</span><span class="sxs-lookup"><span data-stu-id="b8dba-240">You need a `project.json` file that uses NuGet to reference the `FSharp.Interop.Dynamic` and `Dynamitey` assemblies, like this:</span></span>

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

<span data-ttu-id="b8dba-241">Detta använder NuGet för att hämta dina beroenden och hänvisa dem i skriptet.</span><span class="sxs-lookup"><span data-stu-id="b8dba-241">This will use NuGet to fetch your dependencies and will reference them in your script.</span></span>

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a><span data-ttu-id="b8dba-242">HTTP-utlösaren exemplet i Node.JS</span><span class="sxs-lookup"><span data-stu-id="b8dba-242">HTTP trigger sample in Node.JS</span></span>
```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```



<a name="hooktriggersample"></a>
## <a name="webhook-samples"></a><span data-ttu-id="b8dba-243">Webhook-exempel</span><span class="sxs-lookup"><span data-stu-id="b8dba-243">Webhook samples</span></span>
<span data-ttu-id="b8dba-244">Anta att du har följande webhook-utlösaren den `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="b8dba-244">Suppose you have the following webhook trigger in the `bindings` array of function.json:</span></span>

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

<span data-ttu-id="b8dba-245">Se exemplet språkspecifika loggar GitHub problemet kommentarer.</span><span class="sxs-lookup"><span data-stu-id="b8dba-245">See the language-specific sample that logs GitHub issue comments.</span></span>

* [<span data-ttu-id="b8dba-246">C#</span><span class="sxs-lookup"><span data-stu-id="b8dba-246">C#</span></span>](#hooktriggercsharp)
* [<span data-ttu-id="b8dba-247">F#</span><span class="sxs-lookup"><span data-stu-id="b8dba-247">F#</span></span>](#hooktriggerfsharp)
* [<span data-ttu-id="b8dba-248">Node.js</span><span class="sxs-lookup"><span data-stu-id="b8dba-248">Node.js</span></span>](#hooktriggernodejs)

<a name="hooktriggercsharp"></a>

### <a name="webhook-sample-in-c"></a><span data-ttu-id="b8dba-249">Webhook-exempel i C#</span><span class="sxs-lookup"><span data-stu-id="b8dba-249">Webhook sample in C#</span></span> #
```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

<a name="hooktriggerfsharp"></a>

### <a name="webhook-sample-in-f"></a><span data-ttu-id="b8dba-250">Webhook-exempel i F #</span><span class="sxs-lookup"><span data-stu-id="b8dba-250">Webhook sample in F#</span></span> #
```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

<a name="hooktriggernodejs"></a>

### <a name="webhook-sample-in-nodejs"></a><span data-ttu-id="b8dba-251">Webhook-exempel i Node.JS</span><span class="sxs-lookup"><span data-stu-id="b8dba-251">Webhook sample in Node.JS</span></span>
```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```


## <a name="next-steps"></a><span data-ttu-id="b8dba-252">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b8dba-252">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

