---
title: "aaaJavaScript för utvecklare för Azure Functions | Microsoft Docs"
description: "Förstå hur toodevelop fungerar med hjälp av JavaScript."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "azure-funktioner, funktioner, händelsebearbetning, webhooks, dynamisk beräkning, serverlös arkitektur"
ms.assetid: 45dedd78-3ff9-411f-bb4b-16d29a11384c
ms.service: functions
ms.devlang: nodejs
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: glenga
ms.openlocfilehash: 6220b42f965b6ee2463341aaf270836623fdf7fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-javascript-developer-guide"></a><span data-ttu-id="67845-104">Utvecklarhandbok för Azure Functions JavaScript</span><span class="sxs-lookup"><span data-stu-id="67845-104">Azure Functions JavaScript developer guide</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="67845-105">C#-skript</span><span class="sxs-lookup"><span data-stu-id="67845-105">C# script</span></span>](functions-reference-csharp.md)
> * [<span data-ttu-id="67845-106">F # skript</span><span class="sxs-lookup"><span data-stu-id="67845-106">F# script</span></span>](functions-reference-fsharp.md)
> * [<span data-ttu-id="67845-107">JavaScript</span><span class="sxs-lookup"><span data-stu-id="67845-107">JavaScript</span></span>](functions-reference-node.md)
> 
> 

<span data-ttu-id="67845-108">Hej JavaScript-upplevelse för Azure Functions gör det enkelt tooexport en funktion som har skickats som en `context` objekt för att kommunicera med hello runtime och för att ta emot och skicka data via bindningar.</span><span class="sxs-lookup"><span data-stu-id="67845-108">hello JavaScript experience for Azure Functions makes it easy tooexport a function, which is passed as a `context` object for communicating with hello runtime and for receiving and sending data via bindings.</span></span>

<span data-ttu-id="67845-109">Den här artikeln förutsätter att du redan har läst hello [Azure Functions för utvecklare](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="67845-109">This article assumes that you've already read hello [Azure Functions developer reference](functions-reference.md).</span></span>

## <a name="exporting-a-function"></a><span data-ttu-id="67845-110">Exportera en funktion</span><span class="sxs-lookup"><span data-stu-id="67845-110">Exporting a function</span></span>
<span data-ttu-id="67845-111">Alla JavaScript-funktioner måste exportera en enda `function` via `module.exports` för hello runtime toofind hello funktionen och kör den.</span><span class="sxs-lookup"><span data-stu-id="67845-111">All JavaScript functions must export a single `function` via `module.exports` for hello runtime toofind hello function and run it.</span></span> <span data-ttu-id="67845-112">Den här funktionen måste alltid innehålla ett `context` objekt.</span><span class="sxs-lookup"><span data-stu-id="67845-112">This function must always include a `context` object.</span></span>

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by hello arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

<span data-ttu-id="67845-113">Bindningar för `direction === "in"` skickas vidare som funktionsargument, vilket innebär att du kan använda [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) toodynamically hantera nya indata (till exempel med hjälp av `arguments.length` tooiterate över alla indata).</span><span class="sxs-lookup"><span data-stu-id="67845-113">Bindings of `direction === "in"` are passed along as function arguments, which means that you can use [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) toodynamically handle new inputs (for example, by using `arguments.length` tooiterate over all your inputs).</span></span> <span data-ttu-id="67845-114">Den här funktionen är praktisk när du har bara en utlösare och inga ytterligare indata, eftersom du kan komma åt data utlösaren förutsägbart utan refererar till din `context` objekt.</span><span class="sxs-lookup"><span data-stu-id="67845-114">This functionality is convenient when you have only a trigger and no additional inputs, because you can predictably access your trigger data without referencing your `context` object.</span></span>

<span data-ttu-id="67845-115">hello argument överförs alltid längs toohello funktion i hello ordning som de sker i *function.json*, även om du inte anger dem i export-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="67845-115">hello arguments are always passed along toohello function in hello order in which they occur in *function.json*, even if you don't specify them in your exports statement.</span></span> <span data-ttu-id="67845-116">Om du har till exempel `function(context, a, b)` och ändra också`function(context, a)`, du kan fortfarande få hello värdet för `b` i Funktionskoden genom att referera för`arguments[3]`.</span><span class="sxs-lookup"><span data-stu-id="67845-116">For example, if you have `function(context, a, b)` and change it too`function(context, a)`, you can still get hello value of `b` in function code by referring too`arguments[3]`.</span></span>

<span data-ttu-id="67845-117">Alla bindningar oavsett riktning, skickas även längs hello `context` objekt (se följande skript hello).</span><span class="sxs-lookup"><span data-stu-id="67845-117">All bindings, regardless of direction, are also passed along on hello `context` object (see hello following script).</span></span> 

## <a name="context-object"></a><span data-ttu-id="67845-118">Context-objektet</span><span class="sxs-lookup"><span data-stu-id="67845-118">context object</span></span>
<span data-ttu-id="67845-119">hello runtime använder en `context` objektet toopass data tooand från funktionen och toolet du kommunicera med hello runtime.</span><span class="sxs-lookup"><span data-stu-id="67845-119">hello runtime uses a `context` object toopass data tooand from your function and toolet you communicate with hello runtime.</span></span>

<span data-ttu-id="67845-120">hello context-objektet är alltid hello första parametern tooa funktion och måste tas med eftersom den innehåller metoder som `context.done` och `context.log`, som är nödvändiga toouse hello runtime korrekt.</span><span class="sxs-lookup"><span data-stu-id="67845-120">hello context object is always hello first parameter tooa function and must be included because it has methods such as `context.done` and `context.log`, which are required toouse hello runtime correctly.</span></span> <span data-ttu-id="67845-121">Du kan kalla hello objektet vad du vill att (till exempel `ctx` eller `c`).</span><span class="sxs-lookup"><span data-stu-id="67845-121">You can name hello object whatever you would like (for example, `ctx` or `c`).</span></span>

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

### <a name="contextbindings-property"></a><span data-ttu-id="67845-122">Egenskapen Context.Bindings</span><span class="sxs-lookup"><span data-stu-id="67845-122">context.bindings property</span></span>

```
context.bindings
```
<span data-ttu-id="67845-123">Returnerar ett namngivet objekt som innehåller alla inkommande och utgående data.</span><span class="sxs-lookup"><span data-stu-id="67845-123">Returns a named object that contains all your input and output data.</span></span> <span data-ttu-id="67845-124">Till exempel hello bindning definitionen i din *function.json* kan du komma åt hello innehållet i hello kö från hello `context.bindings.myInput` objekt.</span><span class="sxs-lookup"><span data-stu-id="67845-124">For example, hello following binding definition in your *function.json* lets you access hello contents of hello queue from hello `context.bindings.myInput` object.</span></span> 

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
    ...
}
```

```javascript
// myInput contains hello input data, which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

### <a name="contextdone-method"></a><span data-ttu-id="67845-125">Context.Done-metoden</span><span class="sxs-lookup"><span data-stu-id="67845-125">context.done method</span></span>
```
context.done([err],[propertyBag])
```

<span data-ttu-id="67845-126">Informerar hello körningen som koden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="67845-126">Informs hello runtime that your code has finished.</span></span> <span data-ttu-id="67845-127">Du måste anropa `context.done`, eller annan hello runtime aldrig vet att din funktion är klar och hello körning gör timeout.</span><span class="sxs-lookup"><span data-stu-id="67845-127">You must call `context.done`, or else hello runtime never knows that your function is complete, and hello execution will time out.</span></span> 

<span data-ttu-id="67845-128">Hej `context.done` metoden kan du toopass säkerhetskopiera både en användardefinierad fel toohello runtime och en egenskapsuppsättning egenskaper som skriver över hello egenskaperna på hello `context.bindings` objekt.</span><span class="sxs-lookup"><span data-stu-id="67845-128">hello `context.done` method allows you toopass back both a user-defined error toohello runtime and a property bag of properties that overwrite hello properties on hello `context.bindings` object.</span></span>

```javascript
// Even though we set myOutput toohave:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object toohello done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// hello done method will overwrite hello myOutput binding toobe: 
//  -> text: hello there, world, noNumber: true
```

### <a name="contextlog-method"></a><span data-ttu-id="67845-129">Context.log-metoden</span><span class="sxs-lookup"><span data-stu-id="67845-129">context.log method</span></span>  

```
context.log(message)
```
<span data-ttu-id="67845-130">Låter dig toowrite toohello konsolen direktuppspelningsloggar på hello standardnivå för spårning.</span><span class="sxs-lookup"><span data-stu-id="67845-130">Allows you toowrite toohello streaming console logs at hello default trace level.</span></span> <span data-ttu-id="67845-131">På `context.log`, ytterligare loggning metoder är tillgängliga som gör att du kan skriva toohello konsolen loggen på andra spårningsnivåer:</span><span class="sxs-lookup"><span data-stu-id="67845-131">On `context.log`, additional logging methods are available that let you write toohello console log at other trace levels:</span></span>


| <span data-ttu-id="67845-132">Metod</span><span class="sxs-lookup"><span data-stu-id="67845-132">Method</span></span>                 | <span data-ttu-id="67845-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="67845-133">Description</span></span>                                |
| ---------------------- | ------------------------------------------ |
| <span data-ttu-id="67845-134">**fel (_meddelandet_)**</span><span class="sxs-lookup"><span data-stu-id="67845-134">**error(_message_)**</span></span>   | <span data-ttu-id="67845-135">Skriver tooerror nivå loggningen eller lägre.</span><span class="sxs-lookup"><span data-stu-id="67845-135">Writes tooerror level logging, or lower.</span></span>   |
| <span data-ttu-id="67845-136">**Varna (_meddelandet_)**</span><span class="sxs-lookup"><span data-stu-id="67845-136">**warn(_message_)**</span></span>    | <span data-ttu-id="67845-137">Skriver toowarning nivå loggningen eller lägre.</span><span class="sxs-lookup"><span data-stu-id="67845-137">Writes toowarning level logging, or lower.</span></span> |
| <span data-ttu-id="67845-138">**Info (_meddelandet_)**</span><span class="sxs-lookup"><span data-stu-id="67845-138">**info(_message_)**</span></span>    | <span data-ttu-id="67845-139">Skriver tooinfo nivå loggningen eller lägre.</span><span class="sxs-lookup"><span data-stu-id="67845-139">Writes tooinfo level logging, or lower.</span></span>    |
| <span data-ttu-id="67845-140">**utförlig (_meddelandet_)**</span><span class="sxs-lookup"><span data-stu-id="67845-140">**verbose(_message_)**</span></span> | <span data-ttu-id="67845-141">Skriver tooverbose nivå loggning.</span><span class="sxs-lookup"><span data-stu-id="67845-141">Writes tooverbose level logging.</span></span>           |

<span data-ttu-id="67845-142">hello skriver följande exempel toohello konsolen vid Spårningsnivån för hello varning:</span><span class="sxs-lookup"><span data-stu-id="67845-142">hello following example writes toohello console at hello warning trace level:</span></span>

```javascript
context.log.warn("Something has happened."); 
```
<span data-ttu-id="67845-143">Du kan ange hello spårningsnivå tröskelvärde för loggning i hello host.json filen eller stänga av den.</span><span class="sxs-lookup"><span data-stu-id="67845-143">You can set hello trace-level threshold for logging in hello host.json file, or turn it off.</span></span>  <span data-ttu-id="67845-144">Mer information om hur toowrite toohello loggar finns hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="67845-144">For more information about how toowrite toohello logs, see hello next section.</span></span>

## <a name="binding-data-type"></a><span data-ttu-id="67845-145">Datatypen för bindning</span><span class="sxs-lookup"><span data-stu-id="67845-145">Binding data type</span></span>

<span data-ttu-id="67845-146">toodefine hello datatyp för ett inkommande bindningen använder hello `dataType` egenskap i hello bindning definition.</span><span class="sxs-lookup"><span data-stu-id="67845-146">toodefine hello data type for an input binding, use hello `dataType` property in hello binding definition.</span></span> <span data-ttu-id="67845-147">Till exempel tooread hello innehållet i en HTTP-begäran i binärformat, Använd hello typen `binary`:</span><span class="sxs-lookup"><span data-stu-id="67845-147">For example, tooread hello content of an HTTP request in binary format, use hello type `binary`:</span></span>

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

<span data-ttu-id="67845-148">Andra alternativ för `dataType` är `stream` och `string`.</span><span class="sxs-lookup"><span data-stu-id="67845-148">Other options for `dataType` are `stream` and `string`.</span></span>

## <a name="writing-trace-output-toohello-console"></a><span data-ttu-id="67845-149">Skrivning trace utdata toohello konsolen</span><span class="sxs-lookup"><span data-stu-id="67845-149">Writing trace output toohello console</span></span> 

<span data-ttu-id="67845-150">I funktion, använder du hello `context.log` metoder toowrite trace utdata toohello konsolen.</span><span class="sxs-lookup"><span data-stu-id="67845-150">In Functions, you use hello `context.log` methods toowrite trace output toohello console.</span></span> <span data-ttu-id="67845-151">Nu kan du inte använda `console.log` toowrite toohello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="67845-151">At this point, you cannot use `console.log` toowrite toohello console.</span></span>

<span data-ttu-id="67845-152">När du anropar `context.log()`, meddelandet skrivs toohello konsolen vid hello standard spårningsnivån, vilket är hello _info_ spårningsnivå.</span><span class="sxs-lookup"><span data-stu-id="67845-152">When you call `context.log()`, your message is written toohello console at hello default trace level, which is hello _info_ trace level.</span></span> <span data-ttu-id="67845-153">hello skriver följande kod toohello konsolen vid hello info spårningsnivån:</span><span class="sxs-lookup"><span data-stu-id="67845-153">hello following code writes toohello console at hello info trace level:</span></span>

```javascript
context.log({hello: 'world'});  
```

<span data-ttu-id="67845-154">hello är föregående kod likvärdiga toohello följande kod:</span><span class="sxs-lookup"><span data-stu-id="67845-154">hello preceding code is equivalent toohello following code:</span></span>

```javascript
context.log.info({hello: 'world'});  
```

<span data-ttu-id="67845-155">hello skriver följande kod toohello konsolen vid hello Felnivån:</span><span class="sxs-lookup"><span data-stu-id="67845-155">hello following code writes toohello console at hello error level:</span></span>

```javascript
context.log.error("An error has occurred.");  
```

<span data-ttu-id="67845-156">Eftersom _fel_ hello högsta trace genom den här skrivs spåret toohello utdata på alla spårningsnivåer som loggning är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="67845-156">Because _error_ is hello highest trace level, this trace is written toohello output at all trace levels as long as logging is enabled.</span></span>  


<span data-ttu-id="67845-157">Alla `context.log` -metoder stöder hello samma parameter-format som stöds av hello Node.js [util.format metoden](https://nodejs.org/api/util.html#util_util_format_format).</span><span class="sxs-lookup"><span data-stu-id="67845-157">All `context.log` methods support hello same parameter format that's supported by hello Node.js [util.format method](https://nodejs.org/api/util.html#util_util_format_format).</span></span> <span data-ttu-id="67845-158">Överväg följande kod, som skriver toohello konsolen genom att använda hello standard spårningsnivån hello:</span><span class="sxs-lookup"><span data-stu-id="67845-158">Consider hello following code, which writes toohello console by using hello default trace level:</span></span>

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

<span data-ttu-id="67845-159">Du kan också skriva hello samma kod i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="67845-159">You can also write hello same code in hello following format:</span></span>

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-hello-trace-level-for-console-logging"></a><span data-ttu-id="67845-160">Konfigurera hello Spårningsnivån för konsolen loggning</span><span class="sxs-lookup"><span data-stu-id="67845-160">Configure hello trace level for console logging</span></span>

<span data-ttu-id="67845-161">Funktioner kan du definiera hello tröskelvärdet Spårningsnivån för att skriva toohello konsolen, vilket gör det enkelt toocontrol hello sätt spårningar skrivs toohello konsolen från dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="67845-161">Functions lets you define hello threshold trace level for writing toohello console, which makes it easy toocontrol hello way traces are written toohello console from your functions.</span></span> <span data-ttu-id="67845-162">tooset hello tröskelvärdet för alla spårningar skrivs toohello konsolen, Använd hello `tracing.consoleLevel` egenskap i hello host.json fil.</span><span class="sxs-lookup"><span data-stu-id="67845-162">tooset hello threshold for all traces written toohello console, use hello `tracing.consoleLevel` property in hello host.json file.</span></span> <span data-ttu-id="67845-163">Den här inställningen gäller tooall funktioner i appen funktion.</span><span class="sxs-lookup"><span data-stu-id="67845-163">This setting applies tooall functions in your function app.</span></span> <span data-ttu-id="67845-164">hello anger följande exempel hello trace tröskelvärdet tooenable utförlig loggning:</span><span class="sxs-lookup"><span data-stu-id="67845-164">hello following example sets hello trace threshold tooenable verbose logging:</span></span>

```json
{ 
    "tracing": {      
        "consoleLevel": "verbose"     
    }
}  
```

<span data-ttu-id="67845-165">Värden för **consoleLevel** motsvarar toohello namnen på hello `context.log` metoder.</span><span class="sxs-lookup"><span data-stu-id="67845-165">Values of **consoleLevel** correspond toohello names of hello `context.log` methods.</span></span> <span data-ttu-id="67845-166">toodisable alla spårningsloggning toohello konsolen ange **consoleLevel** too_off_.</span><span class="sxs-lookup"><span data-stu-id="67845-166">toodisable all trace logging toohello console, set **consoleLevel** too_off_.</span></span> <span data-ttu-id="67845-167">Mer information om hello host.json fil finns hello [host.json referensavsnittet](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="67845-167">For more information about hello host.json file, see hello [host.json reference topic](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>

## <a name="http-triggers-and-bindings"></a><span data-ttu-id="67845-168">HTTP-utlösare och bindningar</span><span class="sxs-lookup"><span data-stu-id="67845-168">HTTP triggers and bindings</span></span>

<span data-ttu-id="67845-169">HTTP- och webhook-utlösare och HTTP-utdataformat bindningar använda begäran och svar objekt toorepresent hello HTTP meddelanden.</span><span class="sxs-lookup"><span data-stu-id="67845-169">HTTP and webhook triggers and HTTP output bindings use request and response objects toorepresent hello HTTP messaging.</span></span>  

### <a name="request-object"></a><span data-ttu-id="67845-170">Request-objektet</span><span class="sxs-lookup"><span data-stu-id="67845-170">Request object</span></span>

<span data-ttu-id="67845-171">Hej `request` objektet har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="67845-171">hello `request` object has hello following properties:</span></span>

| <span data-ttu-id="67845-172">Egenskap</span><span class="sxs-lookup"><span data-stu-id="67845-172">Property</span></span>      | <span data-ttu-id="67845-173">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="67845-173">Description</span></span>                                                    |
| ------------- | -------------------------------------------------------------- |
| <span data-ttu-id="67845-174">_brödtext_</span><span class="sxs-lookup"><span data-stu-id="67845-174">_body_</span></span>        | <span data-ttu-id="67845-175">Ett objekt som innehåller hello brödtext hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="67845-175">An object that contains hello body of hello request.</span></span>               |
| <span data-ttu-id="67845-176">_rubriker_</span><span class="sxs-lookup"><span data-stu-id="67845-176">_headers_</span></span>     | <span data-ttu-id="67845-177">Ett objekt som innehåller hello-huvuden för begäran.</span><span class="sxs-lookup"><span data-stu-id="67845-177">An object that contains hello request headers.</span></span>                   |
| <span data-ttu-id="67845-178">_metoden_</span><span class="sxs-lookup"><span data-stu-id="67845-178">_method_</span></span>      | <span data-ttu-id="67845-179">hello HTTP-metoden för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="67845-179">hello HTTP method of hello request.</span></span>                                |
| <span data-ttu-id="67845-180">_originalUrl_</span><span class="sxs-lookup"><span data-stu-id="67845-180">_originalUrl_</span></span> | <span data-ttu-id="67845-181">hello-URL för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="67845-181">hello URL of hello request.</span></span>                                        |
| <span data-ttu-id="67845-182">_parametrar_</span><span class="sxs-lookup"><span data-stu-id="67845-182">_params_</span></span>      | <span data-ttu-id="67845-183">Ett objekt som innehåller hello routning parametrar för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="67845-183">An object that contains hello routing parameters of hello request.</span></span> |
| <span data-ttu-id="67845-184">_frågan_</span><span class="sxs-lookup"><span data-stu-id="67845-184">_query_</span></span>       | <span data-ttu-id="67845-185">Ett objekt som innehåller frågeparametrar Hej.</span><span class="sxs-lookup"><span data-stu-id="67845-185">An object that contains hello query parameters.</span></span>                  |
| <span data-ttu-id="67845-186">_rawBody_</span><span class="sxs-lookup"><span data-stu-id="67845-186">_rawBody_</span></span>     | <span data-ttu-id="67845-187">hello meddelandetext hello som en sträng.</span><span class="sxs-lookup"><span data-stu-id="67845-187">hello body of hello message as a string.</span></span>                           |


### <a name="response-object"></a><span data-ttu-id="67845-188">Objektet Response</span><span class="sxs-lookup"><span data-stu-id="67845-188">Response object</span></span>

<span data-ttu-id="67845-189">Hej `response` objektet har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="67845-189">hello `response` object has hello following properties:</span></span>

| <span data-ttu-id="67845-190">Egenskap</span><span class="sxs-lookup"><span data-stu-id="67845-190">Property</span></span>  | <span data-ttu-id="67845-191">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="67845-191">Description</span></span>                                               |
| --------- | --------------------------------------------------------- |
| <span data-ttu-id="67845-192">_brödtext_</span><span class="sxs-lookup"><span data-stu-id="67845-192">_body_</span></span>    | <span data-ttu-id="67845-193">Ett objekt som innehåller hello brödtext hello svar.</span><span class="sxs-lookup"><span data-stu-id="67845-193">An object that contains hello body of hello response.</span></span>         |
| <span data-ttu-id="67845-194">_rubriker_</span><span class="sxs-lookup"><span data-stu-id="67845-194">_headers_</span></span> | <span data-ttu-id="67845-195">Ett objekt som innehåller hello-svarshuvuden.</span><span class="sxs-lookup"><span data-stu-id="67845-195">An object that contains hello response headers.</span></span>             |
| <span data-ttu-id="67845-196">_isRaw_</span><span class="sxs-lookup"><span data-stu-id="67845-196">_isRaw_</span></span>   | <span data-ttu-id="67845-197">Anger att formateringen hoppas över hello svaret.</span><span class="sxs-lookup"><span data-stu-id="67845-197">Indicates that formatting is skipped for hello response.</span></span>    |
| <span data-ttu-id="67845-198">_status_</span><span class="sxs-lookup"><span data-stu-id="67845-198">_status_</span></span>  | <span data-ttu-id="67845-199">hello HTTP-statuskoden hello svar.</span><span class="sxs-lookup"><span data-stu-id="67845-199">hello HTTP status code of hello response.</span></span>                     |

### <a name="accessing-hello-request-and-response"></a><span data-ttu-id="67845-200">Åtkomst till hello förfrågan och svar</span><span class="sxs-lookup"><span data-stu-id="67845-200">Accessing hello request and response</span></span> 

<span data-ttu-id="67845-201">När du arbetar med HTTP-utlösare kan du komma åt hello HTTP-begäran och svar objekt i något av tre sätt:</span><span class="sxs-lookup"><span data-stu-id="67845-201">When you work with HTTP triggers, you can access hello HTTP request and response objects in any of three ways:</span></span>

+ <span data-ttu-id="67845-202">Från hello namnet indata och utdata bindningar.</span><span class="sxs-lookup"><span data-stu-id="67845-202">From hello named input and output bindings.</span></span> <span data-ttu-id="67845-203">På så sätt kan hello hello http-utlösare och bindningar arbete samma som för andra bindningen.</span><span class="sxs-lookup"><span data-stu-id="67845-203">In this way, hello HTTP trigger and bindings work hello same as any other binding.</span></span> <span data-ttu-id="67845-204">hello följande exempel anger hello svarsobjekt med hjälp av en namngiven `response` bindning:</span><span class="sxs-lookup"><span data-stu-id="67845-204">hello following example sets hello response object by using a named `response` binding:</span></span> 

    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```

+ <span data-ttu-id="67845-205">Från `req` och `res` egenskaper för hello `context` objekt.</span><span class="sxs-lookup"><span data-stu-id="67845-205">From `req` and `res` properties on hello `context` object.</span></span> <span data-ttu-id="67845-206">På så sätt kan du kan använda hello vanliga mönstret tooaccess HTTP data från hello context-objektet i stället för att toouse hello fullständig `context.bindings.name` mönster.</span><span class="sxs-lookup"><span data-stu-id="67845-206">In this way, you can use hello conventional pattern tooaccess HTTP data from hello context object, instead of having toouse hello full `context.bindings.name` pattern.</span></span> <span data-ttu-id="67845-207">följande exempel visar hur hello tooaccess hello `req` och `res` objekt på hello `context`:</span><span class="sxs-lookup"><span data-stu-id="67845-207">hello following example shows how tooaccess hello `req` and `res` objects on hello `context`:</span></span>

    ```javascript
    // You can access your http request off hello context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ <span data-ttu-id="67845-208">Genom att anropa `context.done()`.</span><span class="sxs-lookup"><span data-stu-id="67845-208">By calling `context.done()`.</span></span> <span data-ttu-id="67845-209">En särskild typ av HTTP-bindning returnerar hello-svar som skickas toohello `context.done()` metod.</span><span class="sxs-lookup"><span data-stu-id="67845-209">A special kind of HTTP binding returns hello response that is passed toohello `context.done()` method.</span></span> <span data-ttu-id="67845-210">hello följande HTTP utdata bindning definierar en `$return` utdataparameter:</span><span class="sxs-lookup"><span data-stu-id="67845-210">hello following HTTP output binding defines a `$return` output parameter:</span></span>

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    <span data-ttu-id="67845-211">Den här bindningen utdata förväntar du toosupply hello svar när du anropar `done()`enligt följande:</span><span class="sxs-lookup"><span data-stu-id="67845-211">This output binding expects you toosupply hello response when you call `done()`, as follows:</span></span>

    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version-and-package-management"></a><span data-ttu-id="67845-212">Noden version och hantering av paketet</span><span class="sxs-lookup"><span data-stu-id="67845-212">Node version and Package Management</span></span>
<span data-ttu-id="67845-213">hello nod version är låst på `6.5.0`.</span><span class="sxs-lookup"><span data-stu-id="67845-213">hello node version is currently locked at `6.5.0`.</span></span> <span data-ttu-id="67845-214">Vi undersöker att lägga till stöd för flera versioner och gör det konfigureras.</span><span class="sxs-lookup"><span data-stu-id="67845-214">We're investigating adding support for more versions and making it configurable.</span></span>

<span data-ttu-id="67845-215">hello följande steg kan du inkludera paket i funktionen appen:</span><span class="sxs-lookup"><span data-stu-id="67845-215">hello following steps let you include packages in your function app:</span></span> 

1. <span data-ttu-id="67845-216">Gå för`https://<function_app_name>.scm.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="67845-216">Go too`https://<function_app_name>.scm.azurewebsites.net`.</span></span>

2. <span data-ttu-id="67845-217">Klicka på **Debug konsolen** > **CMD**.</span><span class="sxs-lookup"><span data-stu-id="67845-217">Click **Debug Console** > **CMD**.</span></span>

3. <span data-ttu-id="67845-218">Gå för`D:\home\site\wwwroot`, och dra sedan i package.json filen toohello **wwwroot** mappen på hello övre delen av hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="67845-218">Go too`D:\home\site\wwwroot`, and then drag your package.json file toohello **wwwroot** folder at hello top half of hello page.</span></span>  
    <span data-ttu-id="67845-219">Du kan också ladda upp filer tooyour funktionsapp på andra sätt.</span><span class="sxs-lookup"><span data-stu-id="67845-219">You can upload files tooyour function app in other ways also.</span></span> <span data-ttu-id="67845-220">Mer information finns i [hur tooupdate fungerar programfilerna](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="67845-220">For more information, see [How tooupdate function app files](functions-reference.md#fileupdate).</span></span> 

4. <span data-ttu-id="67845-221">När hello package.json filen har överförts kör hello `npm install` i hello **Kudu fjärrkörning konsolen**.</span><span class="sxs-lookup"><span data-stu-id="67845-221">After hello package.json file is uploaded, run hello `npm install` command in hello **Kudu remote execution console**.</span></span>  
    <span data-ttu-id="67845-222">Den här åtgärden hämtar hello-paket som anges i hello package.json-filen och startar om hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="67845-222">This action downloads hello packages indicated in hello package.json file and restarts hello function app.</span></span>

<span data-ttu-id="67845-223">När hello paket som du behöver är installerade måste du importera dem tooyour funktionen genom att anropa `require('packagename')`, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="67845-223">After hello packages you need are installed, you import them tooyour function by calling `require('packagename')`, as in hello following example:</span></span>

```javascript
// Import hello underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

<span data-ttu-id="67845-224">Du ska definiera en `package.json` filen hello roten i appen funktion.</span><span class="sxs-lookup"><span data-stu-id="67845-224">You should define a `package.json` file at hello root of your function app.</span></span> <span data-ttu-id="67845-225">Definiera hello-filen låter alla funktioner i hello app dela hello samma cachelagrade paket, vilket ger hello bästa prestanda.</span><span class="sxs-lookup"><span data-stu-id="67845-225">Defining hello file lets all functions in hello app share hello same cached packages, which gives hello best performance.</span></span> <span data-ttu-id="67845-226">Om det uppstår en versionskonflikt måste du lösa det genom att lägga till en `package.json` i hello-mappen på en specifik funktion.</span><span class="sxs-lookup"><span data-stu-id="67845-226">If a version conflict arises, you can resolve it by adding a `package.json` file in hello folder of a specific function.</span></span>  

## <a name="environment-variables"></a><span data-ttu-id="67845-227">Miljövariabler</span><span class="sxs-lookup"><span data-stu-id="67845-227">Environment variables</span></span>
<span data-ttu-id="67845-228">tooget en miljövariabel eller en app Inställningsvärdet använda `process.env`som visas i följande kodexempel hello:</span><span class="sxs-lookup"><span data-stu-id="67845-228">tooget an environment variable or an app setting value, use `process.env`, as shown in hello following code example:</span></span>

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));

    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```
## <a name="considerations-for-javascript-functions"></a><span data-ttu-id="67845-229">Överväganden för JavaScript-funktioner</span><span class="sxs-lookup"><span data-stu-id="67845-229">Considerations for JavaScript functions</span></span>

<span data-ttu-id="67845-230">När du arbetar med JavaScript-funktioner, Tänk på hello i hello följande två avsnitt.</span><span class="sxs-lookup"><span data-stu-id="67845-230">When you work with JavaScript functions, be aware of hello considerations in hello following two sections.</span></span>

### <a name="choose-single-core-app-service-plans"></a><span data-ttu-id="67845-231">Välj enkel kärna App Service-planer</span><span class="sxs-lookup"><span data-stu-id="67845-231">Choose single-core App Service plans</span></span>

<span data-ttu-id="67845-232">När du skapar en funktionsapp som använder hello App Service-plan, rekommenderar vi att du väljer en plan för enkel kärna i stället för en plan med flera kärnor.</span><span class="sxs-lookup"><span data-stu-id="67845-232">When you create a function app that uses hello App Service plan, we recommend that you select a single-core plan rather than a plan with multiple cores.</span></span> <span data-ttu-id="67845-233">Idag funktioner körs effektivare JavaScript-funktioner på enkel kärna virtuella datorer och med större virtuella datorer inte ger hello förväntades prestandaförbättringar.</span><span class="sxs-lookup"><span data-stu-id="67845-233">Today, Functions runs JavaScript functions more efficiently on single-core VMs, and using larger VMs does not produce hello expected performance improvements.</span></span> <span data-ttu-id="67845-234">Vid behov, du kan skala ut genom att lägga till flera enkel kärna VM-instanser manuellt eller kan du aktivera automatisk skalning.</span><span class="sxs-lookup"><span data-stu-id="67845-234">When necessary, you can manually scale out by adding more single-core VM instances, or you can enable auto-scale.</span></span> <span data-ttu-id="67845-235">Mer information finns i [skala instansantalet manuellt eller automatiskt](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="67845-235">For more information, see [Scale instance count manually or automatically](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).</span></span>    

### <a name="typescript-and-coffeescript-support"></a><span data-ttu-id="67845-236">Stöd för maskin- och CoffeeScript</span><span class="sxs-lookup"><span data-stu-id="67845-236">TypeScript and CoffeeScript support</span></span>
<span data-ttu-id="67845-237">Eftersom direktstöd ännu inte finns för kompilering av automatisk maskin- eller CoffeeScript via hello runtime, måste stödet toobe hanteras utanför hello körning vid tidpunkten för distribution.</span><span class="sxs-lookup"><span data-stu-id="67845-237">Because direct support does not yet exist for auto-compiling TypeScript or CoffeeScript via hello runtime, such support needs toobe handled outside hello runtime, at deployment time.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="67845-238">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="67845-238">Next steps</span></span>
<span data-ttu-id="67845-239">Mer information finns i hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="67845-239">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="67845-240">Metodtips för Azure Functions</span><span class="sxs-lookup"><span data-stu-id="67845-240">Best practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="67845-241">Azure Functions, info för utvecklare</span><span class="sxs-lookup"><span data-stu-id="67845-241">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="67845-242">Azure Functions C# för utvecklare</span><span class="sxs-lookup"><span data-stu-id="67845-242">Azure Functions C# developer reference</span></span>](functions-reference-csharp.md)
* [<span data-ttu-id="67845-243">Azure Functions F # för utvecklare</span><span class="sxs-lookup"><span data-stu-id="67845-243">Azure Functions F# developer reference</span></span>](functions-reference-fsharp.md)
* [<span data-ttu-id="67845-244">Azure Functions-utlösare och bindningar</span><span class="sxs-lookup"><span data-stu-id="67845-244">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)

