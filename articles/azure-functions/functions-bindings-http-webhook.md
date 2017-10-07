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
# <a name="azure-functions-http-and-webhook-bindings"></a>Azure Functions HTTP och webhook bindningar
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Den här artikeln förklarar hur tooconfigure och arbeta med HTTP-utlösare och bindningar i Azure Functions.
Med dessa kan använda du Azure Functions toobuild serverlösa API: er och svara toowebhooks.

Azure Functions erbjuder följande bindningar hello:
- En [HTTP-utlösaren](#httptrigger) kan du anropa en funktion med en HTTP-begäran. Detta kan vara anpassade toorespond för[webhooks](#hooktrigger).
- En [HTTP-utdatabindning](#output) kan du toorespond toohello begäran.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a>HTTP-utlösare
hello HTTP-utlösaren körs din funktion i svaret tooan HTTP-begäran. Du kan anpassa den toorespond tooa viss URL eller uppsättning HTTP-metoderna. En HTTP-utlösare kan också vara konfigurerade toorespond toowebhooks. 

Om du använder hello Functions-portalen kan du också sätta igång direkt med en fördefinierad mall. Välj **nya funktionen** och välj ”API och Webhooks” Hej **scenariot** listrutan. Välj en av hello mallar och klicka på **skapa**.

Som standard svarar ett HTTP-utlösaren toohello begäran med ett 200 OK HTTP-statuskoden och en brödtext. toomodify Hej svar, konfigurera en [HTTP-utdatabindning](#output)

### <a name="configuring-an-http-trigger"></a>Konfigurera en HTTP-utlösare
En HTTP-utlösare definieras genom att inkludera en JSON-objekt liknande toohello efter i hello `bindings` matris med function.json:

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
hello bindningen stöder hello följande egenskaper:

* **namnet** : krävs – hello variabelnamn som används i Funktionskoden för hello begäran eller begärandetexten. Se [arbetar med en HTTP-utlösare från kod](#httptriggerusage).
* **typen** : krävs – måste anges för ”httpTrigger”.
* **riktning** : krävs – måste anges för ”i”.
* _authLevel_ : Detta avgör vilka nycklar, eventuella måste toobe finns på hello begäran i ordning tooinvoke hello-funktionen. Se [arbeta med nycklar](#keys) nedan. hello-värdet kan vara något av följande hello:
    * _anonym_: Nej API-nyckeln är obligatorisk.
    * _funktionen_: en funktionsspecifika API-nyckel krävs. Detta är standardvärdet för hello om ingen anges.
    * _Admin_ : hello huvudnyckel krävs.
* **metoder** : Detta är en matris med hello HTTP-metoderna toowhich hello funktionen kommer att svara. Om inget anges svarar hello funktionen tooall HTTP-metoder. Se [anpassa hello HTTP-slutpunkt](#url).
* **väg** : Detta definierar hello flödesmallen styra toowhich begära webbadresserna funktionen kommer att svara. hello standardvärdet om ingen anges är `<functionname>`. Se [anpassa hello HTTP-slutpunkt](#url).
* **webHookType** : Detta konfigurerar hello HTTP utlösaren tooact som en webhook reciever för hello angivna providern. Hej _metoder_ egenskapen ska inte anges om detta är valt. Se [svarar toowebhooks](#hooktrigger). hello-värdet kan vara något av följande hello:
    * _genericJson_ : en generella webhook slutpunkt utan logik för en specifik provider.
    * _github_ : hello funktionen svarar tooGitHub webhooks. Hej _authLevel_ egenskapen ska inte anges om detta är valt.
    * _slack_ : hello funktionen svarar tooSlack webhooks. Hej _authLevel_ egenskapen ska inte anges om detta är valt.

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a>Arbeta med en HTTP-utlösare från kod
För C# och F #, kan du deklarera hello typ av din utlösaren inkommande toobe antingen `HttpRequestMessage` eller en anpassad typ. Om du väljer `HttpRequestMessage`, och du får fullständig åtkomst toohello request-objektet. För en anpassad typ (till exempel en POCO) försöker funktioner tooparse hello begärantext som JSON toopopulate hello objektets egenskaper.

För Node.js-funktion ger hello Functions-runtime hello begärandetexten i stället för hello request-objektet.

Se [http-utlösaren exempel](#httptriggersample) till exempel användningsområden.


<a name="output"></a>
## <a name="http-response-output-binding"></a>HTTP-svar utdatabindning
Använd hello HTTP utdata bindning toorespond toohello HTTP-begäran avsändare. Den här bindningen kräver en HTTP-utlösare och låter dig toocustomize hello svar som är associerade med hello utlösarbegäran. Om en HTTP-utdatabindning anges inte är en HTTP-utlösare returneras HTTP 200 OK utan en brödtext. 

### <a name="configuring-an-http-output-binding"></a>Konfigurera en HTTP-utdatabindning
hello HTTP utdata bindning definieras genom att inkludera en JSON-objekt liknande toohello efter i hello `bindings` matris med function.json:

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
hello bindningen innehåller hello följande egenskaper:

* **namnet** : krävs – hello variabelnamn som används i Funktionskoden för hello svar. Se [arbetar med en HTTP-utdatabindning från kod](#outputusage).
* **typen** : krävs – måste anges för ”http”.
* **riktning** : krävs – måste anges för ”out”.

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a>Arbeta med en HTTP-utdatabindning från kod
Du kan använda hello utdata parameter (t.ex. ”res”) toorespond toohello HTTP- eller webhook anroparen. Du kan också använda standarden `Request.CreateResponse()` (C#) eller `context.res` (Node.JS) mönster tooreturn ditt svar. Exempel på hur toouse hello senare finns [http-utlösaren prover](#httptriggersample) och [Webhook utlösaren exempel](#hooktriggersample).


<a name="hooktrigger"></a>
## <a name="responding-toowebhooks"></a>Svara toowebhooks
En HTTP-utlösare med hello _webHookType_ egenskapen vara konfigurerade toorespond för[webhooks](https://en.wikipedia.org/wiki/Webhook). hello grundläggande konfiguration använder hello ”genericJson” inställningen. Detta begränsar begäranden tooonly de använder HTTP POST och hello `application/json` innehållstyp.

hello utlösare kan dessutom vara skräddarsydda tooa specifika webhook-providern (t.ex. [GitHub](https://developer.github.com/webhooks/) och [Slack](https://api.slack.com/outgoing-webhooks)). Om en provider anges kan hello Functions-runtime ta hand om hello providern valideringslogik för dig.  

### <a name="configuring-github-as-a-webhook-provider"></a>Konfigurera GitHub som en webhook-provider
toorespond tooGitHub webhooks, först skapa din funktion med en HTTP-utlösare och ange hello _webHookType_ egenskapen för ”github”. Kopiera sedan dess [URL](#url) och [API-nyckel](#keys) till dina GitHub-lagringsplatsen **lägga till webhook** sidan. Se Githubs [skapar Webhooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) dokumentationen för mer information.

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

### <a name="configuring-slack-as-a-webhook-provider"></a>Konfigurera Slack som en webhook-provider
hello Slack webhook genererar en token för du i stället för där du kan ange den, så måste du konfigurera en funktionsspecifika nyckel med hello-token från Slack. Se [arbeta med nycklar](#keys).

<a name="url"></a>
## <a name="customizing-hello-http-endpoint"></a>Anpassa hello HTTP-slutpunkt
Som standard när du skapar en funktion för ett HTTP-utlösare eller WebHook, hello funktionen är adresserbara med en väg hello formuläret:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Du kan anpassa den här vägen med hello valfria `route` egenskapen hello HTTP-utlösaren input bindning. Hej exempelvis följande *function.json* filen definierar en `route` för en HTTP-utlösare:

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

Med den här konfigurationen är hello funktion nu adresserbara med hello efter vägen i stället för hello ursprungliga flödet.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

Detta gör hello Funktionskoden toosupport två parametrar i hello-adress, ”kategorier” och ”id”. Du kan använda någon [Web API-väg begränsningen](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) med parametrarna. Hej följande C#-Funktionskoden använder båda parametrarna.

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

Här är Node.js funktionen kod toouse hello samma vägparametrar.

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

Som standard alla funktionen vägar föregås *api*. Du kan också anpassa eller ta bort hello-adressprefix med hello `http.routePrefix` egenskap i din *host.json* fil. hello följande exempel tar bort hello *api* väg prefix genom att använda en tom sträng för hello prefix i hello *host.json* fil.

```json
    {
      "http": {
        "routePrefix": ""
      }
    }
```

Detaljerad information om hur tooupdate hello *host.json* filen för din funktion finns [hur tooupdate fungerar programfilerna](functions-reference.md#fileupdate). 

Mer information om andra egenskaper som du kan konfigurera i din *host.json* fil, se [host.json referens](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).


<a name="keys"></a>
## <a name="working-with-keys"></a>Arbeta med nycklar
HttpTriggers kan utnyttja nycklar för extra säkerhet. Standard HttpTrigger kan använda dem som en API-nyckel som kräver hello viktiga toobe finns på hello-begäran. Webhooks kan använda nycklar tooauthorize begäranden i en mängd olika sätt beroende på vilken hello-leverantör har stöd för.

Nycklar lagras som en del av din funktionsapp i Azure och krypterat i vila. tooview dina nycklar, skapa nya eller sammanslagning nycklar toonew värden går tooone av dina funktioner i hello-portalen och väljer ”hantera”. 

Det finns två typer av nycklar:
- **Värdnycklar**: nycklarna delas av alla funktioner i hello funktionsapp. När det används som en API-nyckel, kan dessa åtkomst tooany funktion inom hello funktionsapp.
- **Funktionstangenter**: nycklarna gäller endast toohello funktioner som de har definierats. När det används som en API-nyckel, kan dessa endast toothat funktion för åtkomst till.

Varje nyckel som heter referens och det finns en standard-nyckel (med namnet ”standard”) på hello funktionen och värden. Hej **huvudnyckeln** är en standard värd-nyckel med namnet ”_master” som har definierats för varje funktionsapp och det går inte att återkallas. Det ger en administrativ åtkomst toohello runtime API: er. Med hjälp av `"authLevel": "admin"` i hello bindning JSON kräver den här nyckeln toobe visas på hello begäran; andra nyckeln resulterar i ett autentiseringsfel.

> [!NOTE]
> På grund av toohello utökade behörigheter med hello huvudnyckeln ska dela den här nyckeln med tredje part eller distribuera den i native client-program. Var försiktig när du väljer hello admin åtkomstnivå.
> 
> 

### <a name="api-key-authorization"></a>Auktorisering av innehållsnyckel API
Som standard kräver en HttpTrigger en API-nyckel i hello HTTP-begäran. HTTP-begäran ser så normalt ut så här:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

hello nyckel kan ingå i en fråga string-variabel med namnet `code`, enligt ovan, eller det kan ingå i en `x-functions-key` HTTP-huvudet. hello-värdet för hello nyckeln kan vara valfri Funktionstangent för som definierats för hello funktion eller valfri tangent för värden.

Du kan välja tooallow begäranden utan nycklar eller ange att hello huvudnyckel måste användas genom att ändra hello `authLevel` egenskap i hello bindning JSON (se [HTTP-utlösaren](#httptrigger)).

### <a name="keys-and-webhooks"></a>Nycklar och webhooks
Webhook-auktorisering hanteras av hello webhook reciever komponent, en del av hello HttpTrigger och hello mekanism varierar beroende på hello webhook-typen. Varje mekanism matchar, men förlitar sig på en nyckel. Som standard används hello funktionen nyckel med namnet ”standard”. Om du vill toouse en annan nyckel måste tooconfigure hello webhook provider toosend hello nyckelnamn med hello begäran i något av följande sätt hello:

- **Frågesträng**: hello providern skickar hello nyckelnamn i hello `clientid` frågesträngparametern (t.ex. `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).
- **Förfrågningshuvudet**: hello providern skickar hello nyckelnamn i hello `x-functions-clientid` huvud.

> [!NOTE]
> Funktionstangenter företräde framför värdnycklar. Om två nycklar har definierats med samma namn, hello hello funktionen nyckeln kommer att användas.
> 
> 


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a>HTTP-utlösaren prover
Anta att du har följande HTTP-utlösaren i hello hello `bindings` matris med function.json:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

Se hello språkspecifika prov som söker efter en `name` parameter i hello frågesträng eller hello brödtext hello HTTP-begäran.

* [C#](#httptriggercsharp)
* [F#](#httptriggerfsharp)
* [Node.js](#httptriggernodejs)


<a name="httptriggercsharp"></a>
### <a name="http-trigger-sample-in-c"></a>HTTP-utlösaren exemplet i C# #
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

Du kan också binda tooa POCO i stället för `HttpRequestMessage`. Detta kommer ur från hello brödtexten i begäran hello parsade som JSON. På liknande sätt toohello HTTP-svarsutdata bindning kan skickas till en typ och detta returneras som hello svarstexten med statuskod 200.
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
### <a name="http-trigger-sample-in-f"></a>HTTP-utlösaren exemplet i F # #
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

Du behöver en `project.json` fil som använder NuGet tooreference hello `FSharp.Interop.Dynamic` och `Dynamitey` sammansättningar, så här:

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

Detta kommer att använda NuGet toofetch dina beroenden och hänvisa dem i skriptet.

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a>HTTP-utlösaren exemplet i Node.JS
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
## <a name="webhook-samples"></a>Webhook-exempel
Anta att du har följande webhook utlösaren i hello hello `bindings` matris med function.json:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

Se hello språkspecifika exempel loggar GitHub problemet kommentarer.

* [C#](#hooktriggercsharp)
* [F#](#hooktriggerfsharp)
* [Node.js](#hooktriggernodejs)

<a name="hooktriggercsharp"></a>

### <a name="webhook-sample-in-c"></a>Webhook-exempel i C# #
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

### <a name="webhook-sample-in-f"></a>Webhook-exempel i F # #
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

### <a name="webhook-sample-in-nodejs"></a>Webhook-exempel i Node.JS
```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```


## <a name="next-steps"></a>Nästa steg
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

