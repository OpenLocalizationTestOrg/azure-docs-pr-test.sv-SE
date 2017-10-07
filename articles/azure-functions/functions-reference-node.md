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
# <a name="azure-functions-javascript-developer-guide"></a>Utvecklarhandbok för Azure Functions JavaScript
> [!div class="op_single_selector"]
> * [C#-skript](functions-reference-csharp.md)
> * [F # skript](functions-reference-fsharp.md)
> * [JavaScript](functions-reference-node.md)
> 
> 

Hej JavaScript-upplevelse för Azure Functions gör det enkelt tooexport en funktion som har skickats som en `context` objekt för att kommunicera med hello runtime och för att ta emot och skicka data via bindningar.

Den här artikeln förutsätter att du redan har läst hello [Azure Functions för utvecklare](functions-reference.md).

## <a name="exporting-a-function"></a>Exportera en funktion
Alla JavaScript-funktioner måste exportera en enda `function` via `module.exports` för hello runtime toofind hello funktionen och kör den. Den här funktionen måste alltid innehålla ett `context` objekt.

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

Bindningar för `direction === "in"` skickas vidare som funktionsargument, vilket innebär att du kan använda [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) toodynamically hantera nya indata (till exempel med hjälp av `arguments.length` tooiterate över alla indata). Den här funktionen är praktisk när du har bara en utlösare och inga ytterligare indata, eftersom du kan komma åt data utlösaren förutsägbart utan refererar till din `context` objekt.

hello argument överförs alltid längs toohello funktion i hello ordning som de sker i *function.json*, även om du inte anger dem i export-instruktionen. Om du har till exempel `function(context, a, b)` och ändra också`function(context, a)`, du kan fortfarande få hello värdet för `b` i Funktionskoden genom att referera för`arguments[3]`.

Alla bindningar oavsett riktning, skickas även längs hello `context` objekt (se följande skript hello). 

## <a name="context-object"></a>Context-objektet
hello runtime använder en `context` objektet toopass data tooand från funktionen och toolet du kommunicera med hello runtime.

hello context-objektet är alltid hello första parametern tooa funktion och måste tas med eftersom den innehåller metoder som `context.done` och `context.log`, som är nödvändiga toouse hello runtime korrekt. Du kan kalla hello objektet vad du vill att (till exempel `ctx` eller `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

### <a name="contextbindings-property"></a>Egenskapen Context.Bindings

```
context.bindings
```
Returnerar ett namngivet objekt som innehåller alla inkommande och utgående data. Till exempel hello bindning definitionen i din *function.json* kan du komma åt hello innehållet i hello kö från hello `context.bindings.myInput` objekt. 

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

### <a name="contextdone-method"></a>Context.Done-metoden
```
context.done([err],[propertyBag])
```

Informerar hello körningen som koden har slutförts. Du måste anropa `context.done`, eller annan hello runtime aldrig vet att din funktion är klar och hello körning gör timeout. 

Hej `context.done` metoden kan du toopass säkerhetskopiera både en användardefinierad fel toohello runtime och en egenskapsuppsättning egenskaper som skriver över hello egenskaperna på hello `context.bindings` objekt.

```javascript
// Even though we set myOutput toohave:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object toohello done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// hello done method will overwrite hello myOutput binding toobe: 
//  -> text: hello there, world, noNumber: true
```

### <a name="contextlog-method"></a>Context.log-metoden  

```
context.log(message)
```
Låter dig toowrite toohello konsolen direktuppspelningsloggar på hello standardnivå för spårning. På `context.log`, ytterligare loggning metoder är tillgängliga som gör att du kan skriva toohello konsolen loggen på andra spårningsnivåer:


| Metod                 | Beskrivning                                |
| ---------------------- | ------------------------------------------ |
| **fel (_meddelandet_)**   | Skriver tooerror nivå loggningen eller lägre.   |
| **Varna (_meddelandet_)**    | Skriver toowarning nivå loggningen eller lägre. |
| **Info (_meddelandet_)**    | Skriver tooinfo nivå loggningen eller lägre.    |
| **utförlig (_meddelandet_)** | Skriver tooverbose nivå loggning.           |

hello skriver följande exempel toohello konsolen vid Spårningsnivån för hello varning:

```javascript
context.log.warn("Something has happened."); 
```
Du kan ange hello spårningsnivå tröskelvärde för loggning i hello host.json filen eller stänga av den.  Mer information om hur toowrite toohello loggar finns hello nästa avsnitt.

## <a name="binding-data-type"></a>Datatypen för bindning

toodefine hello datatyp för ett inkommande bindningen använder hello `dataType` egenskap i hello bindning definition. Till exempel tooread hello innehållet i en HTTP-begäran i binärformat, Använd hello typen `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Andra alternativ för `dataType` är `stream` och `string`.

## <a name="writing-trace-output-toohello-console"></a>Skrivning trace utdata toohello konsolen 

I funktion, använder du hello `context.log` metoder toowrite trace utdata toohello konsolen. Nu kan du inte använda `console.log` toowrite toohello-konsolen.

När du anropar `context.log()`, meddelandet skrivs toohello konsolen vid hello standard spårningsnivån, vilket är hello _info_ spårningsnivå. hello skriver följande kod toohello konsolen vid hello info spårningsnivån:

```javascript
context.log({hello: 'world'});  
```

hello är föregående kod likvärdiga toohello följande kod:

```javascript
context.log.info({hello: 'world'});  
```

hello skriver följande kod toohello konsolen vid hello Felnivån:

```javascript
context.log.error("An error has occurred.");  
```

Eftersom _fel_ hello högsta trace genom den här skrivs spåret toohello utdata på alla spårningsnivåer som loggning är aktiverat.  


Alla `context.log` -metoder stöder hello samma parameter-format som stöds av hello Node.js [util.format metoden](https://nodejs.org/api/util.html#util_util_format_format). Överväg följande kod, som skriver toohello konsolen genom att använda hello standard spårningsnivån hello:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

Du kan också skriva hello samma kod i hello följande format:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-hello-trace-level-for-console-logging"></a>Konfigurera hello Spårningsnivån för konsolen loggning

Funktioner kan du definiera hello tröskelvärdet Spårningsnivån för att skriva toohello konsolen, vilket gör det enkelt toocontrol hello sätt spårningar skrivs toohello konsolen från dina funktioner. tooset hello tröskelvärdet för alla spårningar skrivs toohello konsolen, Använd hello `tracing.consoleLevel` egenskap i hello host.json fil. Den här inställningen gäller tooall funktioner i appen funktion. hello anger följande exempel hello trace tröskelvärdet tooenable utförlig loggning:

```json
{ 
    "tracing": {      
        "consoleLevel": "verbose"     
    }
}  
```

Värden för **consoleLevel** motsvarar toohello namnen på hello `context.log` metoder. toodisable alla spårningsloggning toohello konsolen ange **consoleLevel** too_off_. Mer information om hello host.json fil finns hello [host.json referensavsnittet](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).

## <a name="http-triggers-and-bindings"></a>HTTP-utlösare och bindningar

HTTP- och webhook-utlösare och HTTP-utdataformat bindningar använda begäran och svar objekt toorepresent hello HTTP meddelanden.  

### <a name="request-object"></a>Request-objektet

Hej `request` objektet har hello följande egenskaper:

| Egenskap      | Beskrivning                                                    |
| ------------- | -------------------------------------------------------------- |
| _brödtext_        | Ett objekt som innehåller hello brödtext hello-begäran.               |
| _rubriker_     | Ett objekt som innehåller hello-huvuden för begäran.                   |
| _metoden_      | hello HTTP-metoden för hello-begäran.                                |
| _originalUrl_ | hello-URL för hello-begäran.                                        |
| _parametrar_      | Ett objekt som innehåller hello routning parametrar för hello-begäran. |
| _frågan_       | Ett objekt som innehåller frågeparametrar Hej.                  |
| _rawBody_     | hello meddelandetext hello som en sträng.                           |


### <a name="response-object"></a>Objektet Response

Hej `response` objektet har hello följande egenskaper:

| Egenskap  | Beskrivning                                               |
| --------- | --------------------------------------------------------- |
| _brödtext_    | Ett objekt som innehåller hello brödtext hello svar.         |
| _rubriker_ | Ett objekt som innehåller hello-svarshuvuden.             |
| _isRaw_   | Anger att formateringen hoppas över hello svaret.    |
| _status_  | hello HTTP-statuskoden hello svar.                     |

### <a name="accessing-hello-request-and-response"></a>Åtkomst till hello förfrågan och svar 

När du arbetar med HTTP-utlösare kan du komma åt hello HTTP-begäran och svar objekt i något av tre sätt:

+ Från hello namnet indata och utdata bindningar. På så sätt kan hello hello http-utlösare och bindningar arbete samma som för andra bindningen. hello följande exempel anger hello svarsobjekt med hjälp av en namngiven `response` bindning: 

    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```

+ Från `req` och `res` egenskaper för hello `context` objekt. På så sätt kan du kan använda hello vanliga mönstret tooaccess HTTP data från hello context-objektet i stället för att toouse hello fullständig `context.bindings.name` mönster. följande exempel visar hur hello tooaccess hello `req` och `res` objekt på hello `context`:

    ```javascript
    // You can access your http request off hello context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ Genom att anropa `context.done()`. En särskild typ av HTTP-bindning returnerar hello-svar som skickas toohello `context.done()` metod. hello följande HTTP utdata bindning definierar en `$return` utdataparameter:

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    Den här bindningen utdata förväntar du toosupply hello svar när du anropar `done()`enligt följande:

    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version-and-package-management"></a>Noden version och hantering av paketet
hello nod version är låst på `6.5.0`. Vi undersöker att lägga till stöd för flera versioner och gör det konfigureras.

hello följande steg kan du inkludera paket i funktionen appen: 

1. Gå för`https://<function_app_name>.scm.azurewebsites.net`.

2. Klicka på **Debug konsolen** > **CMD**.

3. Gå för`D:\home\site\wwwroot`, och dra sedan i package.json filen toohello **wwwroot** mappen på hello övre delen av hello-sidan.  
    Du kan också ladda upp filer tooyour funktionsapp på andra sätt. Mer information finns i [hur tooupdate fungerar programfilerna](functions-reference.md#fileupdate). 

4. När hello package.json filen har överförts kör hello `npm install` i hello **Kudu fjärrkörning konsolen**.  
    Den här åtgärden hämtar hello-paket som anges i hello package.json-filen och startar om hello funktionsapp.

När hello paket som du behöver är installerade måste du importera dem tooyour funktionen genom att anropa `require('packagename')`, som i följande exempel hello:

```javascript
// Import hello underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

Du ska definiera en `package.json` filen hello roten i appen funktion. Definiera hello-filen låter alla funktioner i hello app dela hello samma cachelagrade paket, vilket ger hello bästa prestanda. Om det uppstår en versionskonflikt måste du lösa det genom att lägga till en `package.json` i hello-mappen på en specifik funktion.  

## <a name="environment-variables"></a>Miljövariabler
tooget en miljövariabel eller en app Inställningsvärdet använda `process.env`som visas i följande kodexempel hello:

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
## <a name="considerations-for-javascript-functions"></a>Överväganden för JavaScript-funktioner

När du arbetar med JavaScript-funktioner, Tänk på hello i hello följande två avsnitt.

### <a name="choose-single-core-app-service-plans"></a>Välj enkel kärna App Service-planer

När du skapar en funktionsapp som använder hello App Service-plan, rekommenderar vi att du väljer en plan för enkel kärna i stället för en plan med flera kärnor. Idag funktioner körs effektivare JavaScript-funktioner på enkel kärna virtuella datorer och med större virtuella datorer inte ger hello förväntades prestandaförbättringar. Vid behov, du kan skala ut genom att lägga till flera enkel kärna VM-instanser manuellt eller kan du aktivera automatisk skalning. Mer information finns i [skala instansantalet manuellt eller automatiskt](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).    

### <a name="typescript-and-coffeescript-support"></a>Stöd för maskin- och CoffeeScript
Eftersom direktstöd ännu inte finns för kompilering av automatisk maskin- eller CoffeeScript via hello runtime, måste stödet toobe hanteras utanför hello körning vid tidpunkten för distribution. 

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello följande resurser:

* [Metodtips för Azure Functions](functions-best-practices.md)
* [Azure Functions, info för utvecklare](functions-reference.md)
* [Azure Functions C# för utvecklare](functions-reference-csharp.md)
* [Azure Functions F # för utvecklare](functions-reference-fsharp.md)
* [Azure Functions-utlösare och bindningar](functions-triggers-bindings.md)

