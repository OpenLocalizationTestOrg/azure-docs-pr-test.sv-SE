---
title: aaaAzure http-funktioner och webhook bindningar | Microsoft Docs
description: "Förstå hur toouse HTTP och webhook utlösare och bindningar i Azure Functions."
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
ms.openlocfilehash: c23b7a1443d492ed78c595e97d1d778a7ab12416
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-http-and-webhook-bindings"></a><span data-ttu-id="9ed94-104">Azure Functions HTTP och webhook bindningar</span><span class="sxs-lookup"><span data-stu-id="9ed94-104">Azure Functions HTTP and webhook bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="9ed94-105">Den här artikeln förklarar hur tooconfigure och arbeta med HTTP-utlösare och bindningar i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="9ed94-105">This article explains how tooconfigure and work with HTTP triggers and bindings in Azure Functions.</span></span>
<span data-ttu-id="9ed94-106">Med dessa kan använda du Azure Functions toobuild serverlösa API: er och svara toowebhooks.</span><span class="sxs-lookup"><span data-stu-id="9ed94-106">With these, you can use Azure Functions toobuild serverless APIs and respond toowebhooks.</span></span>

<span data-ttu-id="9ed94-107">Azure Functions erbjuder följande bindningar hello:</span><span class="sxs-lookup"><span data-stu-id="9ed94-107">Azure Functions provides hello following bindings:</span></span>
- <span data-ttu-id="9ed94-108">En [HTTP-utlösaren](#httptrigger) kan du anropa en funktion med en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="9ed94-108">An [HTTP trigger](#httptrigger) lets you invoke a function with an HTTP request.</span></span> <span data-ttu-id="9ed94-109">Detta kan vara anpassade toorespond för[webhooks](#hooktrigger).</span><span class="sxs-lookup"><span data-stu-id="9ed94-109">This can be customized toorespond too[webhooks](#hooktrigger).</span></span>
- <span data-ttu-id="9ed94-110">En [HTTP-utdatabindning](#output) kan du toorespond toohello begäran.</span><span class="sxs-lookup"><span data-stu-id="9ed94-110">An [HTTP output binding](#output) allows you toorespond toohello request.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a><span data-ttu-id="9ed94-111">HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="9ed94-111">HTTP trigger</span></span>
<span data-ttu-id="9ed94-112">hello HTTP-utlösaren körs din funktion i svaret tooan HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="9ed94-112">hello HTTP trigger will execute your function in response tooan HTTP request.</span></span> <span data-ttu-id="9ed94-113">Du kan anpassa den toorespond tooa viss URL eller uppsättning HTTP-metoderna.</span><span class="sxs-lookup"><span data-stu-id="9ed94-113">You can customize it toorespond tooa particular URL or set of HTTP methods.</span></span> <span data-ttu-id="9ed94-114">En HTTP-utlösare kan också vara konfigurerade toorespond toowebhooks.</span><span class="sxs-lookup"><span data-stu-id="9ed94-114">An HTTP trigger can also be configured toorespond toowebhooks.</span></span> 

<span data-ttu-id="9ed94-115">Om du använder hello Functions-portalen kan du också sätta igång direkt med en fördefinierad mall.</span><span class="sxs-lookup"><span data-stu-id="9ed94-115">If using hello Functions portal, you can also get started right away using a pre-made template.</span></span> <span data-ttu-id="9ed94-116">Välj **nya funktionen** och välj ”API och Webhooks” Hej **scenariot** listrutan.</span><span class="sxs-lookup"><span data-stu-id="9ed94-116">Select **New function** and choose "API & Webhooks" from hello **Scenario** dropdown.</span></span> <span data-ttu-id="9ed94-117">Välj en av hello mallar och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="9ed94-117">Select one of hello templates and click **Create**.</span></span>

<span data-ttu-id="9ed94-118">Som standard svarar ett HTTP-utlösaren toohello begäran med ett 200 OK HTTP-statuskoden och en brödtext.</span><span class="sxs-lookup"><span data-stu-id="9ed94-118">By default, an HTTP trigger will respond toohello request with an HTTP 200 OK status code and an empty body.</span></span> <span data-ttu-id="9ed94-119">toomodify Hej svar, konfigurera en [HTTP-utdatabindning](#output)</span><span class="sxs-lookup"><span data-stu-id="9ed94-119">toomodify hello response, configure an [HTTP output binding](#output)</span></span>

### <a name="configuring-an-http-trigger"></a><span data-ttu-id="9ed94-120">Konfigurera en HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="9ed94-120">Configuring an HTTP trigger</span></span>
<span data-ttu-id="9ed94-121">En HTTP-utlösare definieras genom att inkludera en JSON-objekt liknande toohello efter i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="9ed94-121">An HTTP trigger is defined by including a JSON object similar toohello following in hello `bindings` array of function.json:</span></span>

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
<span data-ttu-id="9ed94-122">hello bindningen stöder hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="9ed94-122">hello binding supports hello following properties:</span></span>

* <span data-ttu-id="9ed94-123">**namnet** : krävs – hello variabelnamn som används i Funktionskoden för hello begäran eller begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="9ed94-123">**name** : Required - hello variable name used in function code for hello request or request body.</span></span> <span data-ttu-id="9ed94-124">Se [arbetar med en HTTP-utlösare från kod](#httptriggerusage).</span><span class="sxs-lookup"><span data-stu-id="9ed94-124">See [Working with an HTTP trigger from code](#httptriggerusage).</span></span>
* <span data-ttu-id="9ed94-125">**typen** : krävs – måste anges för ”httpTrigger”.</span><span class="sxs-lookup"><span data-stu-id="9ed94-125">**type** : Required - must be set too"httpTrigger".</span></span>
* <span data-ttu-id="9ed94-126">**riktning** : krävs – måste anges för ”i”.</span><span class="sxs-lookup"><span data-stu-id="9ed94-126">**direction** : Required - must be set too"in".</span></span>
* <span data-ttu-id="9ed94-127">_authLevel_ : Detta avgör vilka nycklar, eventuella måste toobe finns på hello begäran i ordning tooinvoke hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="9ed94-127">_authLevel_ : This determines what keys, if any, need toobe present on hello request in order tooinvoke hello function.</span></span> <span data-ttu-id="9ed94-128">Se [arbeta med nycklar](#keys) nedan.</span><span class="sxs-lookup"><span data-stu-id="9ed94-128">See [Working with keys](#keys) below.</span></span> <span data-ttu-id="9ed94-129">hello-värdet kan vara något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="9ed94-129">hello value can be one of hello following:</span></span>
    * <span data-ttu-id="9ed94-130">_anonym_: Nej API-nyckeln är obligatorisk.</span><span class="sxs-lookup"><span data-stu-id="9ed94-130">_anonymous_: No API key is required.</span></span>
    * <span data-ttu-id="9ed94-131">_funktionen_: en funktionsspecifika API-nyckel krävs.</span><span class="sxs-lookup"><span data-stu-id="9ed94-131">_function_: A function-specific API key is required.</span></span> <span data-ttu-id="9ed94-132">Detta är standardvärdet för hello om ingen anges.</span><span class="sxs-lookup"><span data-stu-id="9ed94-132">This is hello default value if none is provided.</span></span>
    * <span data-ttu-id="9ed94-133">_Admin_ : hello huvudnyckel krävs.</span><span class="sxs-lookup"><span data-stu-id="9ed94-133">_admin_ : hello master key is required.</span></span>
* <span data-ttu-id="9ed94-134">**metoder** : Detta är en matris med hello HTTP-metoderna toowhich hello funktionen kommer att svara.</span><span class="sxs-lookup"><span data-stu-id="9ed94-134">**methods** : This is an array of hello HTTP methods toowhich hello function will respond.</span></span> <span data-ttu-id="9ed94-135">Om inget anges svarar hello funktionen tooall HTTP-metoder.</span><span class="sxs-lookup"><span data-stu-id="9ed94-135">If not specified, hello function will respond tooall HTTP methods.</span></span> <span data-ttu-id="9ed94-136">Se [anpassa hello HTTP-slutpunkt](#url).</span><span class="sxs-lookup"><span data-stu-id="9ed94-136">See [Customizing hello HTTP endpoint](#url).</span></span>
* <span data-ttu-id="9ed94-137">**väg** : Detta definierar hello flödesmallen styra toowhich begära webbadresserna funktionen kommer att svara.</span><span class="sxs-lookup"><span data-stu-id="9ed94-137">**route** : This defines hello route template, controlling toowhich request URLs your function will respond.</span></span> <span data-ttu-id="9ed94-138">hello standardvärdet om ingen anges är `<functionname>`.</span><span class="sxs-lookup"><span data-stu-id="9ed94-138">hello default value if none is provided is `<functionname>`.</span></span> <span data-ttu-id="9ed94-139">Se [anpassa hello HTTP-slutpunkt](#url).</span><span class="sxs-lookup"><span data-stu-id="9ed94-139">See [Customizing hello HTTP endpoint](#url).</span></span>
* <span data-ttu-id="9ed94-140">**webHookType** : Detta konfigurerar hello HTTP utlösaren tooact som en webhook reciever för hello angivna providern.</span><span class="sxs-lookup"><span data-stu-id="9ed94-140">**webHookType** : This configures hello HTTP trigger tooact as a webhook reciever for hello specified provider.</span></span> <span data-ttu-id="9ed94-141">Hej _metoder_ egenskapen ska inte anges om detta är valt.</span><span class="sxs-lookup"><span data-stu-id="9ed94-141">hello _methods_ property should not be set if this is chosen.</span></span> <span data-ttu-id="9ed94-142">Se [svarar toowebhooks](#hooktrigger).</span><span class="sxs-lookup"><span data-stu-id="9ed94-142">See [Responding toowebhooks](#hooktrigger).</span></span> <span data-ttu-id="9ed94-143">hello-värdet kan vara något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="9ed94-143">hello value can be one of hello following:</span></span>
    * <span data-ttu-id="9ed94-144">_genericJson_ : en generella webhook slutpunkt utan logik för en specifik provider.</span><span class="sxs-lookup"><span data-stu-id="9ed94-144">_genericJson_ : A general purpose webhook endpoint without logic for a specific provider.</span></span>
    * <span data-ttu-id="9ed94-145">_github_ : hello funktionen svarar tooGitHub webhooks.</span><span class="sxs-lookup"><span data-stu-id="9ed94-145">_github_ : hello function will respond tooGitHub webhooks.</span></span> <span data-ttu-id="9ed94-146">Hej _authLevel_ egenskapen ska inte anges om detta är valt.</span><span class="sxs-lookup"><span data-stu-id="9ed94-146">hello _authLevel_ property should not be set if this is chosen.</span></span>
    * <span data-ttu-id="9ed94-147">_slack_ : hello funktionen svarar tooSlack webhooks.</span><span class="sxs-lookup"><span data-stu-id="9ed94-147">_slack_ : hello function will respond tooSlack webhooks.</span></span> <span data-ttu-id="9ed94-148">Hej _authLevel_ egenskapen ska inte anges om detta är valt.</span><span class="sxs-lookup"><span data-stu-id="9ed94-148">hello _authLevel_ property should not be set if this is chosen.</span></span>

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a><span data-ttu-id="9ed94-149">Arbeta med en HTTP-utlösare från kod</span><span class="sxs-lookup"><span data-stu-id="9ed94-149">Working with an HTTP trigger from code</span></span>
<span data-ttu-id="9ed94-150">För C# och F #, kan du deklarera hello typ av din utlösaren inkommande toobe antingen `HttpRequestMessage` eller en anpassad typ.</span><span class="sxs-lookup"><span data-stu-id="9ed94-150">For C# and F# functions, you can declare hello type of your trigger input toobe either `HttpRequestMessage` or a custom type.</span></span> <span data-ttu-id="9ed94-151">Om du väljer `HttpRequestMessage`, och du får fullständig åtkomst toohello request-objektet.</span><span class="sxs-lookup"><span data-stu-id="9ed94-151">If you choose `HttpRequestMessage`, then you will get full access toohello request object.</span></span> <span data-ttu-id="9ed94-152">För en anpassad typ (till exempel en POCO) försöker funktioner tooparse hello begärantext som JSON toopopulate hello objektets egenskaper.</span><span class="sxs-lookup"><span data-stu-id="9ed94-152">For a custom type (such as a POCO), Functions will attempt tooparse hello request body as JSON toopopulate hello object properties.</span></span>

<span data-ttu-id="9ed94-153">För Node.js-funktion ger hello Functions-runtime hello begärandetexten i stället för hello request-objektet.</span><span class="sxs-lookup"><span data-stu-id="9ed94-153">For Node.js functions, hello Functions runtime provides hello request body instead of hello request object.</span></span>

<span data-ttu-id="9ed94-154">Se [http-utlösaren exempel](#httptriggersample) till exempel användningsområden.</span><span class="sxs-lookup"><span data-stu-id="9ed94-154">See [HTTP trigger samples](#httptriggersample) for example usages.</span></span>


<a name="output"></a>
## <a name="http-response-output-binding"></a><span data-ttu-id="9ed94-155">HTTP-svar utdatabindning</span><span class="sxs-lookup"><span data-stu-id="9ed94-155">HTTP response output binding</span></span>
<span data-ttu-id="9ed94-156">Använd hello HTTP utdata bindning toorespond toohello HTTP-begäran avsändare.</span><span class="sxs-lookup"><span data-stu-id="9ed94-156">Use hello HTTP output binding toorespond toohello HTTP request sender.</span></span> <span data-ttu-id="9ed94-157">Den här bindningen kräver en HTTP-utlösare och låter dig toocustomize hello svar som är associerade med hello utlösarbegäran.</span><span class="sxs-lookup"><span data-stu-id="9ed94-157">This binding requires an HTTP trigger and allows you toocustomize hello response associated with hello trigger's request.</span></span> <span data-ttu-id="9ed94-158">Om en HTTP-utdatabindning anges inte är en HTTP-utlösare returneras HTTP 200 OK utan en brödtext.</span><span class="sxs-lookup"><span data-stu-id="9ed94-158">If an HTTP output binding is not provided, an HTTP trigger will return HTTP 200 OK with an empty body.</span></span> 

### <a name="configuring-an-http-output-binding"></a><span data-ttu-id="9ed94-159">Konfigurera en HTTP-utdatabindning</span><span class="sxs-lookup"><span data-stu-id="9ed94-159">Configuring an HTTP output binding</span></span>
<span data-ttu-id="9ed94-160">hello HTTP utdata bindning definieras genom att inkludera en JSON-objekt liknande toohello efter i hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="9ed94-160">hello HTTP output binding is defined by including a JSON object similar toohello following in hello `bindings` array of function.json:</span></span>

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
<span data-ttu-id="9ed94-161">hello bindningen innehåller hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="9ed94-161">hello binding contains hello following properties:</span></span>

* <span data-ttu-id="9ed94-162">**namnet** : krävs – hello variabelnamn som används i Funktionskoden för hello svar.</span><span class="sxs-lookup"><span data-stu-id="9ed94-162">**name** : Required - hello variable name used in function code for hello response.</span></span> <span data-ttu-id="9ed94-163">Se [arbetar med en HTTP-utdatabindning från kod](#outputusage).</span><span class="sxs-lookup"><span data-stu-id="9ed94-163">See [Working with an HTTP output binding from code](#outputusage).</span></span>
* <span data-ttu-id="9ed94-164">**typen** : krävs – måste anges för ”http”.</span><span class="sxs-lookup"><span data-stu-id="9ed94-164">**type** : Required - must be set too"http".</span></span>
* <span data-ttu-id="9ed94-165">**riktning** : krävs – måste anges för ”out”.</span><span class="sxs-lookup"><span data-stu-id="9ed94-165">**direction** : Required - must be set too"out".</span></span>

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a><span data-ttu-id="9ed94-166">Arbeta med en HTTP-utdatabindning från kod</span><span class="sxs-lookup"><span data-stu-id="9ed94-166">Working with an HTTP output binding from code</span></span>
<span data-ttu-id="9ed94-167">Du kan använda hello utdata parameter (t.ex. ”res”) toorespond toohello HTTP- eller webhook anroparen.</span><span class="sxs-lookup"><span data-stu-id="9ed94-167">You can use hello output parameter (e.g., "res") toorespond toohello http or webhook caller.</span></span> <span data-ttu-id="9ed94-168">Du kan också använda standarden `Request.CreateResponse()` (C#) eller `context.res` (Node.JS) mönster tooreturn ditt svar.</span><span class="sxs-lookup"><span data-stu-id="9ed94-168">Alternatively, you can use the standard `Request.CreateResponse()` (C#) or `context.res` (Node.JS) pattern tooreturn your response.</span></span> <span data-ttu-id="9ed94-169">Exempel på hur toouse hello senare finns [http-utlösaren prover](#httptriggersample) och [Webhook utlösaren exempel](#hooktriggersample).</span><span class="sxs-lookup"><span data-stu-id="9ed94-169">For examples on how toouse hello latter method, see [HTTP trigger samples](#httptriggersample) and [Webhook trigger samples](#hooktriggersample).</span></span>


<a name="hooktrigger"></a>
## <a name="responding-toowebhooks"></a><span data-ttu-id="9ed94-170">Svara toowebhooks</span><span class="sxs-lookup"><span data-stu-id="9ed94-170">Responding toowebhooks</span></span>
<span data-ttu-id="9ed94-171">En HTTP-utlösare med hello _webHookType_ egenskapen vara konfigurerade toorespond för[webhooks](https://en.wikipedia.org/wiki/Webhook).</span><span class="sxs-lookup"><span data-stu-id="9ed94-171">An HTTP trigger with hello _webHookType_ property will be configured toorespond too[webhooks](https://en.wikipedia.org/wiki/Webhook).</span></span> <span data-ttu-id="9ed94-172">hello grundläggande konfiguration använder hello ”genericJson” inställningen.</span><span class="sxs-lookup"><span data-stu-id="9ed94-172">hello basic configuration uses hello "genericJson" setting.</span></span> <span data-ttu-id="9ed94-173">Detta begränsar begäranden tooonly de använder HTTP POST och hello `application/json` innehållstyp.</span><span class="sxs-lookup"><span data-stu-id="9ed94-173">This restricts requests tooonly those using HTTP POST and with hello `application/json` content type.</span></span>

<span data-ttu-id="9ed94-174">hello utlösare kan dessutom vara skräddarsydda tooa specifika webhook-providern (t.ex. [GitHub](https://developer.github.com/webhooks/) och [Slack](https://api.slack.com/outgoing-webhooks)).</span><span class="sxs-lookup"><span data-stu-id="9ed94-174">hello trigger can additionally be tailored tooa specific webhook provider (e.g., [GitHub](https://developer.github.com/webhooks/) and [Slack](https://api.slack.com/outgoing-webhooks)).</span></span> <span data-ttu-id="9ed94-175">Om en provider anges kan hello Functions-runtime ta hand om hello providern valideringslogik för dig.</span><span class="sxs-lookup"><span data-stu-id="9ed94-175">If a provider is specified, hello Functions runtime can take care of hello provider's validation logic for you.</span></span>  

### <a name="configuring-github-as-a-webhook-provider"></a><span data-ttu-id="9ed94-176">Konfigurera GitHub som en webhook-provider</span><span class="sxs-lookup"><span data-stu-id="9ed94-176">Configuring GitHub as a webhook provider</span></span>
<span data-ttu-id="9ed94-177">toorespond tooGitHub webhooks, först skapa din funktion med en HTTP-utlösare och ange hello _webHookType_ egenskapen för ”github”.</span><span class="sxs-lookup"><span data-stu-id="9ed94-177">toorespond tooGitHub webhooks, first create your function with an HTTP Trigger, and set hello _webHookType_ property too"github".</span></span> <span data-ttu-id="9ed94-178">Kopiera sedan dess [URL](#url) och [API-nyckel](#keys) till dina GitHub-lagringsplatsen **lägga till webhook** sidan.</span><span class="sxs-lookup"><span data-stu-id="9ed94-178">Then copy its [URL](#url) and [API key](#keys) into your GitHub repository's **Add webhook** page.</span></span> <span data-ttu-id="9ed94-179">Se Githubs [skapar Webhooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) dokumentationen för mer information.</span><span class="sxs-lookup"><span data-stu-id="9ed94-179">See GitHub's [Creating Webhooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) documentation for more.</span></span>

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

### <a name="configuring-slack-as-a-webhook-provider"></a><span data-ttu-id="9ed94-180">Konfigurera Slack som en webhook-provider</span><span class="sxs-lookup"><span data-stu-id="9ed94-180">Configuring Slack as a webhook provider</span></span>
<span data-ttu-id="9ed94-181">hello Slack webhook genererar en token för du i stället för där du kan ange den, så måste du konfigurera en funktionsspecifika nyckel med hello-token från Slack.</span><span class="sxs-lookup"><span data-stu-id="9ed94-181">hello Slack webhook generates a token for you instead of letting you specify it, so you must configure a function-specific key with hello token from Slack.</span></span> <span data-ttu-id="9ed94-182">Se [arbeta med nycklar](#keys).</span><span class="sxs-lookup"><span data-stu-id="9ed94-182">See [Working with keys](#keys).</span></span>

<a name="url"></a>
## <a name="customizing-hello-http-endpoint"></a><span data-ttu-id="9ed94-183">Anpassa hello HTTP-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="9ed94-183">Customizing hello HTTP endpoint</span></span>
<span data-ttu-id="9ed94-184">Som standard när du skapar en funktion för ett HTTP-utlösare eller WebHook, hello funktionen är adresserbara med en väg hello formuläret:</span><span class="sxs-lookup"><span data-stu-id="9ed94-184">By default when you create a function for an HTTP trigger, or WebHook, hello function is addressable with a route of hello form:</span></span>

    http://<yourapp>.azurewebsites.net/api/<funcname> 

<span data-ttu-id="9ed94-185">Du kan anpassa den här vägen med hello valfria `route` egenskapen hello HTTP-utlösaren input bindning.</span><span class="sxs-lookup"><span data-stu-id="9ed94-185">You can customize this route using hello optional `route` property on hello HTTP trigger's input binding.</span></span> <span data-ttu-id="9ed94-186">Hej exempelvis följande *function.json* filen definierar en `route` för en HTTP-utlösare:</span><span class="sxs-lookup"><span data-stu-id="9ed94-186">As an example, hello following *function.json* file defines a `route` property for an HTTP trigger:</span></span>

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

<span data-ttu-id="9ed94-187">Med den här konfigurationen är hello funktion nu adresserbara med hello efter vägen i stället för hello ursprungliga flödet.</span><span class="sxs-lookup"><span data-stu-id="9ed94-187">Using this configuration, hello function is now addressable with hello following route instead of hello original route.</span></span>

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

<span data-ttu-id="9ed94-188">Detta gör hello Funktionskoden toosupport två parametrar i hello-adress, ”kategorier” och ”id”.</span><span class="sxs-lookup"><span data-stu-id="9ed94-188">This allows hello function code toosupport two parameters in hello address, "category" and "id".</span></span> <span data-ttu-id="9ed94-189">Du kan använda någon [Web API-väg begränsningen](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) med parametrarna.</span><span class="sxs-lookup"><span data-stu-id="9ed94-189">You can use any [Web API Route Constraint](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) with your parameters.</span></span> <span data-ttu-id="9ed94-190">Hej följande C#-Funktionskoden använder båda parametrarna.</span><span class="sxs-lookup"><span data-stu-id="9ed94-190">hello following C# function code makes use of both parameters.</span></span>

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

<span data-ttu-id="9ed94-191">Här är Node.js funktionen kod toouse hello samma vägparametrar.</span><span class="sxs-lookup"><span data-stu-id="9ed94-191">Here is Node.js function code toouse hello same route parameters.</span></span>

```javascript
    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;

        if (!id) {
            context.res = {
                // status: 200, /* Defaults too200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults too200 */
                body: category + " item with id = " + id + " was requested."
            };
        }

        context.done();
    } 
```

<span data-ttu-id="9ed94-192">Som standard alla funktionen vägar föregås *api*.</span><span class="sxs-lookup"><span data-stu-id="9ed94-192">By default, all function routes are prefixed with *api*.</span></span> <span data-ttu-id="9ed94-193">Du kan också anpassa eller ta bort hello-adressprefix med hello `http.routePrefix` egenskap i din *host.json* fil.</span><span class="sxs-lookup"><span data-stu-id="9ed94-193">You can also customize or remove hello prefix using hello `http.routePrefix` property in your *host.json* file.</span></span> <span data-ttu-id="9ed94-194">hello följande exempel tar bort hello *api* väg prefix genom att använda en tom sträng för hello prefix i hello *host.json* fil.</span><span class="sxs-lookup"><span data-stu-id="9ed94-194">hello following example removes hello *api* route prefix by using an empty string for hello prefix in hello *host.json* file.</span></span>

```json
    {
      "http": {
        "routePrefix": ""
      }
    }
```

<span data-ttu-id="9ed94-195">Detaljerad information om hur tooupdate hello *host.json* filen för din funktion finns [hur tooupdate fungerar programfilerna](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="9ed94-195">For detailed information on how tooupdate hello *host.json* file for your function, See, [How tooupdate function app files](functions-reference.md#fileupdate).</span></span> 

<span data-ttu-id="9ed94-196">Mer information om andra egenskaper som du kan konfigurera i din *host.json* fil, se [host.json referens](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="9ed94-196">For information on other properties you can configure in your *host.json* file, see [host.json reference](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>


<a name="keys"></a>
## <a name="working-with-keys"></a><span data-ttu-id="9ed94-197">Arbeta med nycklar</span><span class="sxs-lookup"><span data-stu-id="9ed94-197">Working with keys</span></span>
<span data-ttu-id="9ed94-198">HttpTriggers kan utnyttja nycklar för extra säkerhet.</span><span class="sxs-lookup"><span data-stu-id="9ed94-198">HttpTriggers can leverage keys for added security.</span></span> <span data-ttu-id="9ed94-199">Standard HttpTrigger kan använda dem som en API-nyckel som kräver hello viktiga toobe finns på hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="9ed94-199">A standard HttpTrigger can use these as an API key, requiring hello key toobe present on hello request.</span></span> <span data-ttu-id="9ed94-200">Webhooks kan använda nycklar tooauthorize begäranden i en mängd olika sätt beroende på vilken hello-leverantör har stöd för.</span><span class="sxs-lookup"><span data-stu-id="9ed94-200">Webhooks can use keys tooauthorize requests in a variety of ways, depending on what hello provider supports.</span></span>

<span data-ttu-id="9ed94-201">Nycklar lagras som en del av din funktionsapp i Azure och krypterat i vila.</span><span class="sxs-lookup"><span data-stu-id="9ed94-201">Keys are stored as part of your function app in Azure and are encrypted at rest.</span></span> <span data-ttu-id="9ed94-202">tooview dina nycklar, skapa nya eller sammanslagning nycklar toonew värden går tooone av dina funktioner i hello-portalen och väljer ”hantera”.</span><span class="sxs-lookup"><span data-stu-id="9ed94-202">tooview your keys, create new ones, or roll keys toonew values, navigate tooone of your functions within hello portal and select "Manage."</span></span> 

<span data-ttu-id="9ed94-203">Det finns två typer av nycklar:</span><span class="sxs-lookup"><span data-stu-id="9ed94-203">There are two types of keys:</span></span>
- <span data-ttu-id="9ed94-204">**Värdnycklar**: nycklarna delas av alla funktioner i hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="9ed94-204">**Host keys**: These keys are shared by all functions within hello function app.</span></span> <span data-ttu-id="9ed94-205">När det används som en API-nyckel, kan dessa åtkomst tooany funktion inom hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="9ed94-205">When used as an API key, these allow access tooany function within hello function app.</span></span>
- <span data-ttu-id="9ed94-206">**Funktionstangenter**: nycklarna gäller endast toohello funktioner som de har definierats.</span><span class="sxs-lookup"><span data-stu-id="9ed94-206">**Function keys**: These keys apply only toohello specific functions under which they are defined.</span></span> <span data-ttu-id="9ed94-207">När det används som en API-nyckel, kan dessa endast toothat funktion för åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="9ed94-207">When used as an API key, these only allow access toothat function.</span></span>

<span data-ttu-id="9ed94-208">Varje nyckel som heter referens och det finns en standard-nyckel (med namnet ”standard”) på hello funktionen och värden.</span><span class="sxs-lookup"><span data-stu-id="9ed94-208">Each key is named for reference, and there is a default key (named "default") at hello function and host level.</span></span> <span data-ttu-id="9ed94-209">Hej **huvudnyckeln** är en standard värd-nyckel med namnet ”_master” som har definierats för varje funktionsapp och det går inte att återkallas.</span><span class="sxs-lookup"><span data-stu-id="9ed94-209">hello **master key** is a default host key named "_master" that is defined for each function app and cannot be revoked.</span></span> <span data-ttu-id="9ed94-210">Det ger en administrativ åtkomst toohello runtime API: er.</span><span class="sxs-lookup"><span data-stu-id="9ed94-210">It provides administrative access toohello runtime APIs.</span></span> <span data-ttu-id="9ed94-211">Med hjälp av `"authLevel": "admin"` i hello bindning JSON kräver den här nyckeln toobe visas på hello begäran; andra nyckeln resulterar i ett autentiseringsfel.</span><span class="sxs-lookup"><span data-stu-id="9ed94-211">Using `"authLevel": "admin"` in hello binding JSON will require this key toobe presented on hello request; any other key will result in a authorization failure.</span></span>

> [!NOTE]
> <span data-ttu-id="9ed94-212">På grund av toohello utökade behörigheter med hello huvudnyckeln ska dela den här nyckeln med tredje part eller distribuera den i native client-program.</span><span class="sxs-lookup"><span data-stu-id="9ed94-212">Due toohello elevated permissions granted by hello master key, you should not share this key with third parties or distribute it in native client applications.</span></span> <span data-ttu-id="9ed94-213">Var försiktig när du väljer hello admin åtkomstnivå.</span><span class="sxs-lookup"><span data-stu-id="9ed94-213">Exercise caution when choosing hello admin authorization level.</span></span>
> 
> 

### <a name="api-key-authorization"></a><span data-ttu-id="9ed94-214">Auktorisering av innehållsnyckel API</span><span class="sxs-lookup"><span data-stu-id="9ed94-214">API key authorization</span></span>
<span data-ttu-id="9ed94-215">Som standard kräver en HttpTrigger en API-nyckel i hello HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="9ed94-215">By default, an HttpTrigger requires an API key in hello HTTP request.</span></span> <span data-ttu-id="9ed94-216">HTTP-begäran ser så normalt ut så här:</span><span class="sxs-lookup"><span data-stu-id="9ed94-216">So your HTTP request normally looks like this:</span></span>

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

<span data-ttu-id="9ed94-217">hello nyckel kan ingå i en fråga string-variabel med namnet `code`, enligt ovan, eller det kan ingå i en `x-functions-key` HTTP-huvudet.</span><span class="sxs-lookup"><span data-stu-id="9ed94-217">hello key can be included in a query string variable named `code`, as above, or it can be included in an `x-functions-key` HTTP header.</span></span> <span data-ttu-id="9ed94-218">hello-värdet för hello nyckeln kan vara valfri Funktionstangent för som definierats för hello funktion eller valfri tangent för värden.</span><span class="sxs-lookup"><span data-stu-id="9ed94-218">hello value of hello key can be any function key defined for hello function, or any host key.</span></span>

<span data-ttu-id="9ed94-219">Du kan välja tooallow begäranden utan nycklar eller ange att hello huvudnyckel måste användas genom att ändra hello `authLevel` egenskap i hello bindning JSON (se [HTTP-utlösaren](#httptrigger)).</span><span class="sxs-lookup"><span data-stu-id="9ed94-219">You can choose tooallow requests without keys or specify that hello master key must be used by changing hello `authLevel` property in hello binding JSON (see [HTTP trigger](#httptrigger)).</span></span>

### <a name="keys-and-webhooks"></a><span data-ttu-id="9ed94-220">Nycklar och webhooks</span><span class="sxs-lookup"><span data-stu-id="9ed94-220">Keys and webhooks</span></span>
<span data-ttu-id="9ed94-221">Webhook-auktorisering hanteras av hello webhook reciever komponent, en del av hello HttpTrigger och hello mekanism varierar beroende på hello webhook-typen.</span><span class="sxs-lookup"><span data-stu-id="9ed94-221">Webhook authorization is handled by hello webhook reciever component, part of hello HttpTrigger, and hello mechanism varies based on hello webhook type.</span></span> <span data-ttu-id="9ed94-222">Varje mekanism matchar, men förlitar sig på en nyckel.</span><span class="sxs-lookup"><span data-stu-id="9ed94-222">Each mechanism does, however rely on a key.</span></span> <span data-ttu-id="9ed94-223">Som standard används hello funktionen nyckel med namnet ”standard”.</span><span class="sxs-lookup"><span data-stu-id="9ed94-223">By default, hello function key named "default" will be used.</span></span> <span data-ttu-id="9ed94-224">Om du vill toouse en annan nyckel måste tooconfigure hello webhook provider toosend hello nyckelnamn med hello begäran i något av följande sätt hello:</span><span class="sxs-lookup"><span data-stu-id="9ed94-224">If you wish toouse a different key, you will need tooconfigure hello webhook provider toosend hello key name with hello request in one of hello following ways:</span></span>

- <span data-ttu-id="9ed94-225">**Frågesträng**: hello providern skickar hello nyckelnamn i hello `clientid` frågesträngparametern (t.ex. `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).</span><span class="sxs-lookup"><span data-stu-id="9ed94-225">**Query string**: hello provider passes hello key name in hello `clientid` query string parameter (e.g., `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).</span></span>
- <span data-ttu-id="9ed94-226">**Förfrågningshuvudet**: hello providern skickar hello nyckelnamn i hello `x-functions-clientid` huvud.</span><span class="sxs-lookup"><span data-stu-id="9ed94-226">**Request header**: hello provider passes hello key name in hello `x-functions-clientid` header.</span></span>

> [!NOTE]
> <span data-ttu-id="9ed94-227">Funktionstangenter företräde framför värdnycklar.</span><span class="sxs-lookup"><span data-stu-id="9ed94-227">Function keys take precedence over host keys.</span></span> <span data-ttu-id="9ed94-228">Om två nycklar har definierats med samma namn, hello hello funktionen nyckeln kommer att användas.</span><span class="sxs-lookup"><span data-stu-id="9ed94-228">If two keys are defined with hello same name, hello function key will be used.</span></span>
> 
> 


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a><span data-ttu-id="9ed94-229">HTTP-utlösaren prover</span><span class="sxs-lookup"><span data-stu-id="9ed94-229">HTTP trigger samples</span></span>
<span data-ttu-id="9ed94-230">Anta att du har följande HTTP-utlösaren i hello hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="9ed94-230">Suppose you have hello following HTTP trigger in hello `bindings` array of function.json:</span></span>

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

<span data-ttu-id="9ed94-231">Se hello språkspecifika prov som söker efter en `name` parameter i hello frågesträng eller hello brödtext hello HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="9ed94-231">See hello language-specific sample that looks for a `name` parameter either in hello query string or hello body of hello HTTP request.</span></span>

* [<span data-ttu-id="9ed94-232">C#</span><span class="sxs-lookup"><span data-stu-id="9ed94-232">C#</span></span>](#httptriggercsharp)
* [<span data-ttu-id="9ed94-233">F#</span><span class="sxs-lookup"><span data-stu-id="9ed94-233">F#</span></span>](#httptriggerfsharp)
* [<span data-ttu-id="9ed94-234">Node.js</span><span class="sxs-lookup"><span data-stu-id="9ed94-234">Node.js</span></span>](#httptriggernodejs)


<a name="httptriggercsharp"></a>
### <a name="http-trigger-sample-in-c"></a><span data-ttu-id="9ed94-235">HTTP-utlösaren exemplet i C#</span><span class="sxs-lookup"><span data-stu-id="9ed94-235">HTTP trigger sample in C#</span></span> #
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

    // Set name tooquery string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on hello query string or in hello request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

<span data-ttu-id="9ed94-236">Du kan också binda tooa POCO i stället för `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="9ed94-236">You can also bind tooa POCO instead of `HttpRequestMessage`.</span></span> <span data-ttu-id="9ed94-237">Detta kommer ur från hello brödtexten i begäran hello parsade som JSON.</span><span class="sxs-lookup"><span data-stu-id="9ed94-237">This will be hydrated from hello body of hello request, parsed as JSON.</span></span> <span data-ttu-id="9ed94-238">På liknande sätt toohello HTTP-svarsutdata bindning kan skickas till en typ och detta returneras som hello svarstexten med statuskod 200.</span><span class="sxs-lookup"><span data-stu-id="9ed94-238">Similarly, a type can be passed toohello HTTP response output binding, and this will be returned as hello response body, with a 200 status code.</span></span>
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
### <a name="http-trigger-sample-in-f"></a><span data-ttu-id="9ed94-239">HTTP-utlösaren exemplet i F #</span><span class="sxs-lookup"><span data-stu-id="9ed94-239">HTTP trigger sample in F#</span></span> #
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
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on hello query string or in hello request body")
    } |> Async.StartAsTask
```

<span data-ttu-id="9ed94-240">Du behöver en `project.json` fil som använder NuGet tooreference hello `FSharp.Interop.Dynamic` och `Dynamitey` sammansättningar, så här:</span><span class="sxs-lookup"><span data-stu-id="9ed94-240">You need a `project.json` file that uses NuGet tooreference hello `FSharp.Interop.Dynamic` and `Dynamitey` assemblies, like this:</span></span>

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

<span data-ttu-id="9ed94-241">Detta kommer att använda NuGet toofetch dina beroenden och hänvisa dem i skriptet.</span><span class="sxs-lookup"><span data-stu-id="9ed94-241">This will use NuGet toofetch your dependencies and will reference them in your script.</span></span>

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a><span data-ttu-id="9ed94-242">HTTP-utlösaren exemplet i Node.JS</span><span class="sxs-lookup"><span data-stu-id="9ed94-242">HTTP trigger sample in Node.JS</span></span>
```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults too200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on hello query string or in hello request body"
        };
    }
    context.done();
};
```



<a name="hooktriggersample"></a>
## <a name="webhook-samples"></a><span data-ttu-id="9ed94-243">Webhook-exempel</span><span class="sxs-lookup"><span data-stu-id="9ed94-243">Webhook samples</span></span>
<span data-ttu-id="9ed94-244">Anta att du har följande webhook utlösaren i hello hello `bindings` matris med function.json:</span><span class="sxs-lookup"><span data-stu-id="9ed94-244">Suppose you have hello following webhook trigger in hello `bindings` array of function.json:</span></span>

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

<span data-ttu-id="9ed94-245">Se hello språkspecifika exempel loggar GitHub problemet kommentarer.</span><span class="sxs-lookup"><span data-stu-id="9ed94-245">See hello language-specific sample that logs GitHub issue comments.</span></span>

* [<span data-ttu-id="9ed94-246">C#</span><span class="sxs-lookup"><span data-stu-id="9ed94-246">C#</span></span>](#hooktriggercsharp)
* [<span data-ttu-id="9ed94-247">F#</span><span class="sxs-lookup"><span data-stu-id="9ed94-247">F#</span></span>](#hooktriggerfsharp)
* [<span data-ttu-id="9ed94-248">Node.js</span><span class="sxs-lookup"><span data-stu-id="9ed94-248">Node.js</span></span>](#hooktriggernodejs)

<a name="hooktriggercsharp"></a>

### <a name="webhook-sample-in-c"></a><span data-ttu-id="9ed94-249">Webhook-exempel i C#</span><span class="sxs-lookup"><span data-stu-id="9ed94-249">Webhook sample in C#</span></span> #
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

### <a name="webhook-sample-in-f"></a><span data-ttu-id="9ed94-250">Webhook-exempel i F #</span><span class="sxs-lookup"><span data-stu-id="9ed94-250">Webhook sample in F#</span></span> #
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

### <a name="webhook-sample-in-nodejs"></a><span data-ttu-id="9ed94-251">Webhook-exempel i Node.JS</span><span class="sxs-lookup"><span data-stu-id="9ed94-251">Webhook sample in Node.JS</span></span>
```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```


## <a name="next-steps"></a><span data-ttu-id="9ed94-252">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9ed94-252">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

