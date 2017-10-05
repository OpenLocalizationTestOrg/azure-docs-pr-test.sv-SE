---
title: "JavaScript-utvecklare för Azure Functions | Microsoft Docs"
description: "Förstå hur du utvecklar funktioner med hjälp av JavaScript."
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
ms.openlocfilehash: 7ea81ed47f391fbce1432c2b11ac176ab6c04ae0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-javascript-developer-guide"></a><span data-ttu-id="56c9e-104">Utvecklarhandbok för Azure Functions JavaScript</span><span class="sxs-lookup"><span data-stu-id="56c9e-104">Azure Functions JavaScript developer guide</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="56c9e-105">C#-skript</span><span class="sxs-lookup"><span data-stu-id="56c9e-105">C# script</span></span>](functions-reference-csharp.md)
> * [<span data-ttu-id="56c9e-106">F # skript</span><span class="sxs-lookup"><span data-stu-id="56c9e-106">F# script</span></span>](functions-reference-fsharp.md)
> * [<span data-ttu-id="56c9e-107">JavaScript</span><span class="sxs-lookup"><span data-stu-id="56c9e-107">JavaScript</span></span>](functions-reference-node.md)
> 
> 

<span data-ttu-id="56c9e-108">JavaScript-upplevelsen för Azure Functions gör det enkelt att exportera en funktion som har skickats som en `context` objekt för att kommunicera med körningen och för att ta emot och skicka data via bindningar.</span><span class="sxs-lookup"><span data-stu-id="56c9e-108">The JavaScript experience for Azure Functions makes it easy to export a function, which is passed as a `context` object for communicating with the runtime and for receiving and sending data via bindings.</span></span>

<span data-ttu-id="56c9e-109">Den här artikeln förutsätter att du redan har läst den [Azure Functions för utvecklare](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="56c9e-109">This article assumes that you've already read the [Azure Functions developer reference](functions-reference.md).</span></span>

## <a name="exporting-a-function"></a><span data-ttu-id="56c9e-110">Exportera en funktion</span><span class="sxs-lookup"><span data-stu-id="56c9e-110">Exporting a function</span></span>
<span data-ttu-id="56c9e-111">Alla JavaScript-funktioner måste exportera en enda `function` via `module.exports` för runtime hitta funktionen och kör den.</span><span class="sxs-lookup"><span data-stu-id="56c9e-111">All JavaScript functions must export a single `function` via `module.exports` for the runtime to find the function and run it.</span></span> <span data-ttu-id="56c9e-112">Den här funktionen måste alltid innehålla ett `context` objekt.</span><span class="sxs-lookup"><span data-stu-id="56c9e-112">This function must always include a `context` object.</span></span>

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

<span data-ttu-id="56c9e-113">Bindningar för `direction === "in"` skickas vidare som funktionsargument, vilket innebär att du kan använda [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) dynamiskt hantera nya indata (till exempel med hjälp av `arguments.length` och iterera över alla indata).</span><span class="sxs-lookup"><span data-stu-id="56c9e-113">Bindings of `direction === "in"` are passed along as function arguments, which means that you can use [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) to dynamically handle new inputs (for example, by using `arguments.length` to iterate over all your inputs).</span></span> <span data-ttu-id="56c9e-114">Den här funktionen är praktisk när du har bara en utlösare och inga ytterligare indata, eftersom du kan komma åt data utlösaren förutsägbart utan refererar till din `context` objekt.</span><span class="sxs-lookup"><span data-stu-id="56c9e-114">This functionality is convenient when you have only a trigger and no additional inputs, because you can predictably access your trigger data without referencing your `context` object.</span></span>

<span data-ttu-id="56c9e-115">Argumenten är alltid överföras till funktionen i den ordning som de sker i *function.json*, även om du inte anger dem i export-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="56c9e-115">The arguments are always passed along to the function in the order in which they occur in *function.json*, even if you don't specify them in your exports statement.</span></span> <span data-ttu-id="56c9e-116">Om du har till exempel `function(context, a, b)` och ändra den till `function(context, a)`, du kan fortfarande få värdet för `b` i Funktionskoden genom att referera till `arguments[3]`.</span><span class="sxs-lookup"><span data-stu-id="56c9e-116">For example, if you have `function(context, a, b)` and change it to `function(context, a)`, you can still get the value of `b` in function code by referring to `arguments[3]`.</span></span>

<span data-ttu-id="56c9e-117">Alla bindningar oavsett riktning, skickas även vidare den `context` objekt (se följande skript).</span><span class="sxs-lookup"><span data-stu-id="56c9e-117">All bindings, regardless of direction, are also passed along on the `context` object (see the following script).</span></span> 

## <a name="context-object"></a><span data-ttu-id="56c9e-118">Context-objektet</span><span class="sxs-lookup"><span data-stu-id="56c9e-118">context object</span></span>
<span data-ttu-id="56c9e-119">Körtidsbiblioteket använder en `context` objekt att skicka data till och från din funktion och så att du kan kommunicera med körningen.</span><span class="sxs-lookup"><span data-stu-id="56c9e-119">The runtime uses a `context` object to pass data to and from your function and to let you communicate with the runtime.</span></span>

<span data-ttu-id="56c9e-120">Context-objektet är alltid den första parametern till en funktion och måste tas med eftersom den innehåller metoder som `context.done` och `context.log`, vilket krävs för att använda körningsmiljön på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="56c9e-120">The context object is always the first parameter to a function and must be included because it has methods such as `context.done` and `context.log`, which are required to use the runtime correctly.</span></span> <span data-ttu-id="56c9e-121">Du kan kalla objektet vad du vill (till exempel `ctx` eller `c`).</span><span class="sxs-lookup"><span data-stu-id="56c9e-121">You can name the object whatever you would like (for example, `ctx` or `c`).</span></span>

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

### <a name="contextbindings-property"></a><span data-ttu-id="56c9e-122">Egenskapen Context.Bindings</span><span class="sxs-lookup"><span data-stu-id="56c9e-122">context.bindings property</span></span>

```
context.bindings
```
<span data-ttu-id="56c9e-123">Returnerar ett namngivet objekt som innehåller alla inkommande och utgående data.</span><span class="sxs-lookup"><span data-stu-id="56c9e-123">Returns a named object that contains all your input and output data.</span></span> <span data-ttu-id="56c9e-124">Till exempel följande bindning definition i din *function.json* kan du få åtkomst till innehållet i kö från den `context.bindings.myInput` objekt.</span><span class="sxs-lookup"><span data-stu-id="56c9e-124">For example, the following binding definition in your *function.json* lets you access the contents of the queue from the `context.bindings.myInput` object.</span></span> 

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
    ...
}
```

```javascript
// myInput contains the input data, which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

### <a name="contextdone-method"></a><span data-ttu-id="56c9e-125">Context.Done-metoden</span><span class="sxs-lookup"><span data-stu-id="56c9e-125">context.done method</span></span>
```
context.done([err],[propertyBag])
```

<span data-ttu-id="56c9e-126">Informerar körningsmiljön koden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="56c9e-126">Informs the runtime that your code has finished.</span></span> <span data-ttu-id="56c9e-127">Du måste anropa `context.done`, eller annan körningsmiljön aldrig vet att din funktion är klar och körningen gör timeout.</span><span class="sxs-lookup"><span data-stu-id="56c9e-127">You must call `context.done`, or else the runtime never knows that your function is complete, and the execution will time out.</span></span> 

<span data-ttu-id="56c9e-128">Den `context.done` metod kan du skicka både en användardefinierad fel körningsmiljön och en egenskapsuppsättning egenskaper som åsidosätter egenskaper på den `context.bindings` objekt.</span><span class="sxs-lookup"><span data-stu-id="56c9e-128">The `context.done` method allows you to pass back both a user-defined error to the runtime and a property bag of properties that overwrite the properties on the `context.bindings` object.</span></span>

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

### <a name="contextlog-method"></a><span data-ttu-id="56c9e-129">Context.log-metoden</span><span class="sxs-lookup"><span data-stu-id="56c9e-129">context.log method</span></span>  

```
context.log(message)
```
<span data-ttu-id="56c9e-130">Gör att du kan skriva till direktuppspelningsloggar konsolen på standardnivå för spårning.</span><span class="sxs-lookup"><span data-stu-id="56c9e-130">Allows you to write to the streaming console logs at the default trace level.</span></span> <span data-ttu-id="56c9e-131">På `context.log`, ytterligare loggning metoder är tillgängliga som gör att du kan skriva till konsolen loggen på andra spårningsnivåer:</span><span class="sxs-lookup"><span data-stu-id="56c9e-131">On `context.log`, additional logging methods are available that let you write to the console log at other trace levels:</span></span>


| <span data-ttu-id="56c9e-132">Metod</span><span class="sxs-lookup"><span data-stu-id="56c9e-132">Method</span></span>                 | <span data-ttu-id="56c9e-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="56c9e-133">Description</span></span>                                |
| ---------------------- | ------------------------------------------ |
| <span data-ttu-id="56c9e-134">**fel (_meddelandet_)**</span><span class="sxs-lookup"><span data-stu-id="56c9e-134">**error(_message_)**</span></span>   | <span data-ttu-id="56c9e-135">Skriver till Felnivån loggningen eller lägre.</span><span class="sxs-lookup"><span data-stu-id="56c9e-135">Writes to error level logging, or lower.</span></span>   |
| <span data-ttu-id="56c9e-136">**Varna (_meddelandet_)**</span><span class="sxs-lookup"><span data-stu-id="56c9e-136">**warn(_message_)**</span></span>    | <span data-ttu-id="56c9e-137">Skriver till varningsnivå loggningen eller lägre.</span><span class="sxs-lookup"><span data-stu-id="56c9e-137">Writes to warning level logging, or lower.</span></span> |
| <span data-ttu-id="56c9e-138">**Info (_meddelandet_)**</span><span class="sxs-lookup"><span data-stu-id="56c9e-138">**info(_message_)**</span></span>    | <span data-ttu-id="56c9e-139">Skriver till info-nivån loggningen eller lägre.</span><span class="sxs-lookup"><span data-stu-id="56c9e-139">Writes to info level logging, or lower.</span></span>    |
| <span data-ttu-id="56c9e-140">**utförlig (_meddelandet_)**</span><span class="sxs-lookup"><span data-stu-id="56c9e-140">**verbose(_message_)**</span></span> | <span data-ttu-id="56c9e-141">Skriver till nivån utförlig loggning.</span><span class="sxs-lookup"><span data-stu-id="56c9e-141">Writes to verbose level logging.</span></span>           |

<span data-ttu-id="56c9e-142">I följande exempel skriver till konsolen vid spårningsnivån varning:</span><span class="sxs-lookup"><span data-stu-id="56c9e-142">The following example writes to the console at the warning trace level:</span></span>

```javascript
context.log.warn("Something has happened."); 
```
<span data-ttu-id="56c9e-143">Du kan ange spårningsnivå tröskelvärdet för loggning i filen host.json eller stänga av den.</span><span class="sxs-lookup"><span data-stu-id="56c9e-143">You can set the trace-level threshold for logging in the host.json file, or turn it off.</span></span>  <span data-ttu-id="56c9e-144">Mer information om hur du kan skriva till loggar finns i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="56c9e-144">For more information about how to write to the logs, see the next section.</span></span>

## <a name="binding-data-type"></a><span data-ttu-id="56c9e-145">Datatypen för bindning</span><span class="sxs-lookup"><span data-stu-id="56c9e-145">Binding data type</span></span>

<span data-ttu-id="56c9e-146">Om du vill definiera datatypen för en indatabindning den `dataType` egenskapen i Bindningsdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="56c9e-146">To define the data type for an input binding, use the `dataType` property in the binding definition.</span></span> <span data-ttu-id="56c9e-147">Till exempel använda typen om du vill läsa innehållet i en HTTP-begäran i binärformat `binary`:</span><span class="sxs-lookup"><span data-stu-id="56c9e-147">For example, to read the content of an HTTP request in binary format, use the type `binary`:</span></span>

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

<span data-ttu-id="56c9e-148">Andra alternativ för `dataType` är `stream` och `string`.</span><span class="sxs-lookup"><span data-stu-id="56c9e-148">Other options for `dataType` are `stream` and `string`.</span></span>

## <a name="writing-trace-output-to-the-console"></a><span data-ttu-id="56c9e-149">Skrivning av spårningsutdata till konsolen</span><span class="sxs-lookup"><span data-stu-id="56c9e-149">Writing trace output to the console</span></span> 

<span data-ttu-id="56c9e-150">I funktion, använder du den `context.log` metoder för att skriva av spårningsutdata till konsolen.</span><span class="sxs-lookup"><span data-stu-id="56c9e-150">In Functions, you use the `context.log` methods to write trace output to the console.</span></span> <span data-ttu-id="56c9e-151">Nu kan du inte använda `console.log` att skriva till konsolen.</span><span class="sxs-lookup"><span data-stu-id="56c9e-151">At this point, you cannot use `console.log` to write to the console.</span></span>

<span data-ttu-id="56c9e-152">När du anropar `context.log()`, meddelandet skrivs till konsolen på trace nivå, som är den _info_ spårningsnivå.</span><span class="sxs-lookup"><span data-stu-id="56c9e-152">When you call `context.log()`, your message is written to the console at the default trace level, which is the _info_ trace level.</span></span> <span data-ttu-id="56c9e-153">Följande kod skriver till konsolen vid spårningsnivån info:</span><span class="sxs-lookup"><span data-stu-id="56c9e-153">The following code writes to the console at the info trace level:</span></span>

```javascript
context.log({hello: 'world'});  
```

<span data-ttu-id="56c9e-154">Föregående kod motsvarar följande kod:</span><span class="sxs-lookup"><span data-stu-id="56c9e-154">The preceding code is equivalent to the following code:</span></span>

```javascript
context.log.info({hello: 'world'});  
```

<span data-ttu-id="56c9e-155">Följande kod skriver till konsolen vid Felnivån:</span><span class="sxs-lookup"><span data-stu-id="56c9e-155">The following code writes to the console at the error level:</span></span>

```javascript
context.log.error("An error has occurred.");  
```

<span data-ttu-id="56c9e-156">Eftersom _fel_ är den högsta spårningen genom den här skrivs spåret till utdata med alla spårningsnivåer som loggning är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="56c9e-156">Because _error_ is the highest trace level, this trace is written to the output at all trace levels as long as logging is enabled.</span></span>  


<span data-ttu-id="56c9e-157">Alla `context.log` -metoder stöder samma parameter format som stöds av Node.js [util.format metoden](https://nodejs.org/api/util.html#util_util_format_format).</span><span class="sxs-lookup"><span data-stu-id="56c9e-157">All `context.log` methods support the same parameter format that's supported by the Node.js [util.format method](https://nodejs.org/api/util.html#util_util_format_format).</span></span> <span data-ttu-id="56c9e-158">Överväg följande kod, som skriver till konsolen genom att använda trace Standardnivå:</span><span class="sxs-lookup"><span data-stu-id="56c9e-158">Consider the following code, which writes to the console by using the default trace level:</span></span>

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

<span data-ttu-id="56c9e-159">Du kan också skriva samma kod i följande format:</span><span class="sxs-lookup"><span data-stu-id="56c9e-159">You can also write the same code in the following format:</span></span>

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-the-trace-level-for-console-logging"></a><span data-ttu-id="56c9e-160">Konfigurera Spårningsnivån för konsolen loggning</span><span class="sxs-lookup"><span data-stu-id="56c9e-160">Configure the trace level for console logging</span></span>

<span data-ttu-id="56c9e-161">Funktioner kan du definiera spårningsnivån tröskelvärdet för att skriva till konsolen, vilket gör det enkelt att styra sätt spårningar skrivs till konsolen från dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="56c9e-161">Functions lets you define the threshold trace level for writing to the console, which makes it easy to control the way traces are written to the console from your functions.</span></span> <span data-ttu-id="56c9e-162">Om du vill ange ett tröskelvärde för alla spår som skrivs till konsolen använda den `tracing.consoleLevel` egenskapen i filen host.json.</span><span class="sxs-lookup"><span data-stu-id="56c9e-162">To set the threshold for all traces written to the console, use the `tracing.consoleLevel` property in the host.json file.</span></span> <span data-ttu-id="56c9e-163">Den här inställningen gäller för alla funktioner i appen funktion.</span><span class="sxs-lookup"><span data-stu-id="56c9e-163">This setting applies to all functions in your function app.</span></span> <span data-ttu-id="56c9e-164">I följande exempel anger tröskelvärdet spårning för att aktivera utförlig loggning:</span><span class="sxs-lookup"><span data-stu-id="56c9e-164">The following example sets the trace threshold to enable verbose logging:</span></span>

```json
{ 
    "tracing": {      
        "consoleLevel": "verbose"     
    }
}  
```

<span data-ttu-id="56c9e-165">Värden för **consoleLevel** motsvarar namnen på de `context.log` metoder.</span><span class="sxs-lookup"><span data-stu-id="56c9e-165">Values of **consoleLevel** correspond to the names of the `context.log` methods.</span></span> <span data-ttu-id="56c9e-166">Om du vill inaktivera alla spårningsloggning i konsolen, ange **consoleLevel** till _av_.</span><span class="sxs-lookup"><span data-stu-id="56c9e-166">To disable all trace logging to the console, set **consoleLevel** to _off_.</span></span> <span data-ttu-id="56c9e-167">Mer information om filen host.json finns i [host.json referensavsnittet](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="56c9e-167">For more information about the host.json file, see the [host.json reference topic](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>

## <a name="http-triggers-and-bindings"></a><span data-ttu-id="56c9e-168">HTTP-utlösare och bindningar</span><span class="sxs-lookup"><span data-stu-id="56c9e-168">HTTP triggers and bindings</span></span>

<span data-ttu-id="56c9e-169">HTTP- och webhook-utlösare och HTTP-utdataformat bindningar använder förfrågan och svar objekt för att representera HTTP-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="56c9e-169">HTTP and webhook triggers and HTTP output bindings use request and response objects to represent the HTTP messaging.</span></span>  

### <a name="request-object"></a><span data-ttu-id="56c9e-170">Request-objektet</span><span class="sxs-lookup"><span data-stu-id="56c9e-170">Request object</span></span>

<span data-ttu-id="56c9e-171">Den `request` objektet har följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="56c9e-171">The `request` object has the following properties:</span></span>

| <span data-ttu-id="56c9e-172">Egenskap</span><span class="sxs-lookup"><span data-stu-id="56c9e-172">Property</span></span>      | <span data-ttu-id="56c9e-173">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="56c9e-173">Description</span></span>                                                    |
| ------------- | -------------------------------------------------------------- |
| <span data-ttu-id="56c9e-174">_brödtext_</span><span class="sxs-lookup"><span data-stu-id="56c9e-174">_body_</span></span>        | <span data-ttu-id="56c9e-175">Ett objekt som innehåller brödtexten i begäran.</span><span class="sxs-lookup"><span data-stu-id="56c9e-175">An object that contains the body of the request.</span></span>               |
| <span data-ttu-id="56c9e-176">_rubriker_</span><span class="sxs-lookup"><span data-stu-id="56c9e-176">_headers_</span></span>     | <span data-ttu-id="56c9e-177">Ett objekt som innehåller huvuden för begäran.</span><span class="sxs-lookup"><span data-stu-id="56c9e-177">An object that contains the request headers.</span></span>                   |
| <span data-ttu-id="56c9e-178">_metoden_</span><span class="sxs-lookup"><span data-stu-id="56c9e-178">_method_</span></span>      | <span data-ttu-id="56c9e-179">HTTP-metod för begäran.</span><span class="sxs-lookup"><span data-stu-id="56c9e-179">The HTTP method of the request.</span></span>                                |
| <span data-ttu-id="56c9e-180">_originalUrl_</span><span class="sxs-lookup"><span data-stu-id="56c9e-180">_originalUrl_</span></span> | <span data-ttu-id="56c9e-181">URL för begäran.</span><span class="sxs-lookup"><span data-stu-id="56c9e-181">The URL of the request.</span></span>                                        |
| <span data-ttu-id="56c9e-182">_parametrar_</span><span class="sxs-lookup"><span data-stu-id="56c9e-182">_params_</span></span>      | <span data-ttu-id="56c9e-183">Ett objekt som innehåller parametrar routning av begäran.</span><span class="sxs-lookup"><span data-stu-id="56c9e-183">An object that contains the routing parameters of the request.</span></span> |
| <span data-ttu-id="56c9e-184">_frågan_</span><span class="sxs-lookup"><span data-stu-id="56c9e-184">_query_</span></span>       | <span data-ttu-id="56c9e-185">Ett objekt som innehåller Frågeparametrar.</span><span class="sxs-lookup"><span data-stu-id="56c9e-185">An object that contains the query parameters.</span></span>                  |
| <span data-ttu-id="56c9e-186">_rawBody_</span><span class="sxs-lookup"><span data-stu-id="56c9e-186">_rawBody_</span></span>     | <span data-ttu-id="56c9e-187">Innehållet i meddelandet som en sträng.</span><span class="sxs-lookup"><span data-stu-id="56c9e-187">The body of the message as a string.</span></span>                           |


### <a name="response-object"></a><span data-ttu-id="56c9e-188">Objektet Response</span><span class="sxs-lookup"><span data-stu-id="56c9e-188">Response object</span></span>

<span data-ttu-id="56c9e-189">Den `response` objektet har följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="56c9e-189">The `response` object has the following properties:</span></span>

| <span data-ttu-id="56c9e-190">Egenskap</span><span class="sxs-lookup"><span data-stu-id="56c9e-190">Property</span></span>  | <span data-ttu-id="56c9e-191">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="56c9e-191">Description</span></span>                                               |
| --------- | --------------------------------------------------------- |
| <span data-ttu-id="56c9e-192">_brödtext_</span><span class="sxs-lookup"><span data-stu-id="56c9e-192">_body_</span></span>    | <span data-ttu-id="56c9e-193">Ett objekt som innehåller brödtexten i svaret.</span><span class="sxs-lookup"><span data-stu-id="56c9e-193">An object that contains the body of the response.</span></span>         |
| <span data-ttu-id="56c9e-194">_rubriker_</span><span class="sxs-lookup"><span data-stu-id="56c9e-194">_headers_</span></span> | <span data-ttu-id="56c9e-195">Ett objekt som innehåller svarshuvuden.</span><span class="sxs-lookup"><span data-stu-id="56c9e-195">An object that contains the response headers.</span></span>             |
| <span data-ttu-id="56c9e-196">_isRaw_</span><span class="sxs-lookup"><span data-stu-id="56c9e-196">_isRaw_</span></span>   | <span data-ttu-id="56c9e-197">Anger att formateringen ignoreras för svaret.</span><span class="sxs-lookup"><span data-stu-id="56c9e-197">Indicates that formatting is skipped for the response.</span></span>    |
| <span data-ttu-id="56c9e-198">_status_</span><span class="sxs-lookup"><span data-stu-id="56c9e-198">_status_</span></span>  | <span data-ttu-id="56c9e-199">HTTP-statuskod i svaret.</span><span class="sxs-lookup"><span data-stu-id="56c9e-199">The HTTP status code of the response.</span></span>                     |

### <a name="accessing-the-request-and-response"></a><span data-ttu-id="56c9e-200">Åtkomst till förfrågan och svar</span><span class="sxs-lookup"><span data-stu-id="56c9e-200">Accessing the request and response</span></span> 

<span data-ttu-id="56c9e-201">När du arbetar med HTTP-utlösare kan du komma åt HTTP-begäran och svar objekt i något av tre sätt:</span><span class="sxs-lookup"><span data-stu-id="56c9e-201">When you work with HTTP triggers, you can access the HTTP request and response objects in any of three ways:</span></span>

+ <span data-ttu-id="56c9e-202">Från namngivna indata och utdata bindningar.</span><span class="sxs-lookup"><span data-stu-id="56c9e-202">From the named input and output bindings.</span></span> <span data-ttu-id="56c9e-203">På så sätt kan fungerar http-utlösare och bindningar på samma sätt som andra bindningen.</span><span class="sxs-lookup"><span data-stu-id="56c9e-203">In this way, the HTTP trigger and bindings work the same as any other binding.</span></span> <span data-ttu-id="56c9e-204">I följande exempel anger response-objektet med en namngiven `response` bindning:</span><span class="sxs-lookup"><span data-stu-id="56c9e-204">The following example sets the response object by using a named `response` binding:</span></span> 

    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```

+ <span data-ttu-id="56c9e-205">Från `req` och `res` egenskaper på den `context` objekt.</span><span class="sxs-lookup"><span data-stu-id="56c9e-205">From `req` and `res` properties on the `context` object.</span></span> <span data-ttu-id="56c9e-206">På så sätt kan du använda det vanliga mönstret att komma åt HTTP data från context-objektet i stället för att använda fullständigt `context.bindings.name` mönster.</span><span class="sxs-lookup"><span data-stu-id="56c9e-206">In this way, you can use the conventional pattern to access HTTP data from the context object, instead of having to use the full `context.bindings.name` pattern.</span></span> <span data-ttu-id="56c9e-207">I följande exempel visas hur du kommer åt den `req` och `res` objekt på den `context`:</span><span class="sxs-lookup"><span data-stu-id="56c9e-207">The following example shows how to access the `req` and `res` objects on the `context`:</span></span>

    ```javascript
    // You can access your http request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ <span data-ttu-id="56c9e-208">Genom att anropa `context.done()`.</span><span class="sxs-lookup"><span data-stu-id="56c9e-208">By calling `context.done()`.</span></span> <span data-ttu-id="56c9e-209">En särskild typ av HTTP-bindning returnerar svaret som skickas till den `context.done()` metoden.</span><span class="sxs-lookup"><span data-stu-id="56c9e-209">A special kind of HTTP binding returns the response that is passed to the `context.done()` method.</span></span> <span data-ttu-id="56c9e-210">Följande HTTP-utdatabindning definierar en `$return` utdataparameter:</span><span class="sxs-lookup"><span data-stu-id="56c9e-210">The following HTTP output binding defines a `$return` output parameter:</span></span>

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    <span data-ttu-id="56c9e-211">Den här bindningen utdata förväntar du kan ange svaret när du anropar `done()`enligt följande:</span><span class="sxs-lookup"><span data-stu-id="56c9e-211">This output binding expects you to supply the response when you call `done()`, as follows:</span></span>

    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version-and-package-management"></a><span data-ttu-id="56c9e-212">Noden version och hantering av paketet</span><span class="sxs-lookup"><span data-stu-id="56c9e-212">Node version and Package Management</span></span>
<span data-ttu-id="56c9e-213">Nod-versionen är låst på `6.5.0`.</span><span class="sxs-lookup"><span data-stu-id="56c9e-213">The node version is currently locked at `6.5.0`.</span></span> <span data-ttu-id="56c9e-214">Vi undersöker att lägga till stöd för flera versioner och gör det konfigureras.</span><span class="sxs-lookup"><span data-stu-id="56c9e-214">We're investigating adding support for more versions and making it configurable.</span></span>

<span data-ttu-id="56c9e-215">Följande steg kan du inkludera paket i funktionen appen:</span><span class="sxs-lookup"><span data-stu-id="56c9e-215">The following steps let you include packages in your function app:</span></span> 

1. <span data-ttu-id="56c9e-216">Gå till `https://<function_app_name>.scm.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="56c9e-216">Go to `https://<function_app_name>.scm.azurewebsites.net`.</span></span>

2. <span data-ttu-id="56c9e-217">Klicka på **Debug konsolen** > **CMD**.</span><span class="sxs-lookup"><span data-stu-id="56c9e-217">Click **Debug Console** > **CMD**.</span></span>

3. <span data-ttu-id="56c9e-218">Gå till `D:\home\site\wwwroot`, och dra sedan filen package.json till den **wwwroot** mappen på den övre delen av sidan.</span><span class="sxs-lookup"><span data-stu-id="56c9e-218">Go to `D:\home\site\wwwroot`, and then drag your package.json file to the **wwwroot** folder at the top half of the page.</span></span>  
    <span data-ttu-id="56c9e-219">Du kan även överföra filer till funktionen appen på andra sätt.</span><span class="sxs-lookup"><span data-stu-id="56c9e-219">You can upload files to your function app in other ways also.</span></span> <span data-ttu-id="56c9e-220">Mer information finns i [hur du uppdaterar funktionen programfilerna](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="56c9e-220">For more information, see [How to update function app files](functions-reference.md#fileupdate).</span></span> 

4. <span data-ttu-id="56c9e-221">När filen package.json överförs kör den `npm install` i den **Kudu fjärrkörning konsolen**.</span><span class="sxs-lookup"><span data-stu-id="56c9e-221">After the package.json file is uploaded, run the `npm install` command in the **Kudu remote execution console**.</span></span>  
    <span data-ttu-id="56c9e-222">Den här åtgärden hämtar de paket som anges i package.json-filen och startar om funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="56c9e-222">This action downloads the packages indicated in the package.json file and restarts the function app.</span></span>

<span data-ttu-id="56c9e-223">När du har installerat de paket som du behöver du ska importera dem till din funktion genom att anropa `require('packagename')`, som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="56c9e-223">After the packages you need are installed, you import them to your function by calling `require('packagename')`, as in the following example:</span></span>

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

<span data-ttu-id="56c9e-224">Du ska definiera en `package.json` filen i roten på en funktionsapp i.</span><span class="sxs-lookup"><span data-stu-id="56c9e-224">You should define a `package.json` file at the root of your function app.</span></span> <span data-ttu-id="56c9e-225">Definiera filen kan alla funktioner i appen som delar samma cachelagrade paket, vilket ger bästa prestanda.</span><span class="sxs-lookup"><span data-stu-id="56c9e-225">Defining the file lets all functions in the app share the same cached packages, which gives the best performance.</span></span> <span data-ttu-id="56c9e-226">Om det uppstår en versionskonflikt måste du lösa det genom att lägga till en `package.json` fil i mappen på en specifik funktion.</span><span class="sxs-lookup"><span data-stu-id="56c9e-226">If a version conflict arises, you can resolve it by adding a `package.json` file in the folder of a specific function.</span></span>  

## <a name="environment-variables"></a><span data-ttu-id="56c9e-227">Miljövariabler</span><span class="sxs-lookup"><span data-stu-id="56c9e-227">Environment variables</span></span>
<span data-ttu-id="56c9e-228">För att få en miljövariabel eller en app som inställningsvärde kan använda `process.env`som visas i följande kodexempel:</span><span class="sxs-lookup"><span data-stu-id="56c9e-228">To get an environment variable or an app setting value, use `process.env`, as shown in the following code example:</span></span>

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
## <a name="considerations-for-javascript-functions"></a><span data-ttu-id="56c9e-229">Överväganden för JavaScript-funktioner</span><span class="sxs-lookup"><span data-stu-id="56c9e-229">Considerations for JavaScript functions</span></span>

<span data-ttu-id="56c9e-230">När du arbetar med JavaScript-funktioner måste du vara medveten om överväganden i följande två avsnitt.</span><span class="sxs-lookup"><span data-stu-id="56c9e-230">When you work with JavaScript functions, be aware of the considerations in the following two sections.</span></span>

### <a name="choose-single-core-app-service-plans"></a><span data-ttu-id="56c9e-231">Välj enkel kärna App Service-planer</span><span class="sxs-lookup"><span data-stu-id="56c9e-231">Choose single-core App Service plans</span></span>

<span data-ttu-id="56c9e-232">När du skapar en funktionsapp som använder App Service-plan, rekommenderar vi att du väljer en plan för enkel kärna i stället för en plan med flera kärnor.</span><span class="sxs-lookup"><span data-stu-id="56c9e-232">When you create a function app that uses the App Service plan, we recommend that you select a single-core plan rather than a plan with multiple cores.</span></span> <span data-ttu-id="56c9e-233">Idag funktioner körs effektivare JavaScript-funktioner på enkel kärna virtuella datorer och med större virtuella datorer inte ger förväntade prestandaförbättringarna.</span><span class="sxs-lookup"><span data-stu-id="56c9e-233">Today, Functions runs JavaScript functions more efficiently on single-core VMs, and using larger VMs does not produce the expected performance improvements.</span></span> <span data-ttu-id="56c9e-234">Vid behov, du kan skala ut genom att lägga till flera enkel kärna VM-instanser manuellt eller kan du aktivera automatisk skalning.</span><span class="sxs-lookup"><span data-stu-id="56c9e-234">When necessary, you can manually scale out by adding more single-core VM instances, or you can enable auto-scale.</span></span> <span data-ttu-id="56c9e-235">Mer information finns i [skala instansantalet manuellt eller automatiskt](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="56c9e-235">For more information, see [Scale instance count manually or automatically](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).</span></span>    

### <a name="typescript-and-coffeescript-support"></a><span data-ttu-id="56c9e-236">Stöd för maskin- och CoffeeScript</span><span class="sxs-lookup"><span data-stu-id="56c9e-236">TypeScript and CoffeeScript support</span></span>
<span data-ttu-id="56c9e-237">Eftersom direktstöd ännu inte finns för kompilering av automatisk maskin- eller CoffeeScript via körningen, behöver stödet hanteras utanför körning, vid tidpunkten för distribution.</span><span class="sxs-lookup"><span data-stu-id="56c9e-237">Because direct support does not yet exist for auto-compiling TypeScript or CoffeeScript via the runtime, such support needs to be handled outside the runtime, at deployment time.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="56c9e-238">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="56c9e-238">Next steps</span></span>
<span data-ttu-id="56c9e-239">Mer information finns i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="56c9e-239">For more information, see the following resources:</span></span>

* [<span data-ttu-id="56c9e-240">Metodtips för Azure Functions</span><span class="sxs-lookup"><span data-stu-id="56c9e-240">Best practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="56c9e-241">Azure Functions, info för utvecklare</span><span class="sxs-lookup"><span data-stu-id="56c9e-241">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="56c9e-242">Azure Functions C# för utvecklare</span><span class="sxs-lookup"><span data-stu-id="56c9e-242">Azure Functions C# developer reference</span></span>](functions-reference-csharp.md)
* [<span data-ttu-id="56c9e-243">Azure Functions F # för utvecklare</span><span class="sxs-lookup"><span data-stu-id="56c9e-243">Azure Functions F# developer reference</span></span>](functions-reference-fsharp.md)
* [<span data-ttu-id="56c9e-244">Azure Functions-utlösare och bindningar</span><span class="sxs-lookup"><span data-stu-id="56c9e-244">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)

