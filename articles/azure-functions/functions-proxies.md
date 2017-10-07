---
title: aaaWork med proxyservrar i Azure Functions | Microsoft Docs
description: "Översikt över hur toouse Azure Functions proxyservrar"
services: functions
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/11/2017
ms.author: mahender
ms.openlocfilehash: 4d94c89e8f8f2d2c771b01bae142bf9a4f3b7f2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-azure-functions-proxies-preview"></a>Arbeta med Azure Functions-proxyservrar (förhandsgranskning)

> [!NOTE] 
> Azure Functions proxyservrar är för närvarande under förhandsgranskning. Det är gratis när i förhandsgranskningen men standardfunktioner fakturering tillämpar tooproxy körningar. Mer information finns i [priser för Azure Functions](https://azure.microsoft.com/pricing/details/functions/).

Den här artikeln förklarar hur tooconfigure och arbeta med Azure Functions proxyservrar. Med den här funktionen kan du ange slutpunkter på appen funktion som implementeras av en annan resurs. Du kan använda dessa proxyservrar toobreak stora API i flera funktionen appar (som en mikrotjänster arkitektur) och ändå kunna erbjuda en enda API-yta för klienter.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]


## <a name="enable"></a>Aktivera Azure Functions proxyservrar

Proxy aktiveras inte som standard. Du kan skapa proxyservrar när hello-funktionen är inaktiverad, men de kommer inte att köra. tooenable proxyservrar hello följande:

1. Öppna hello [Azure-portalen], och sedan gå tooyour funktionsapp.
2. Välj **fungerar appinställningar**.
3. Växeln **aktivera Azure Functions proxyservrar (förhandsgranskning)** för**på**.

Du kan också returnera här tooupdate hello proxy runtime när nya funktioner blir tillgängliga.


## <a name="create"></a>Skapa en proxy

Det här avsnittet visar hur toocreate en proxy i hello Functions-portalen.

1. Öppna hello [Azure-portalen], och sedan gå tooyour funktionsapp.
2. I hello vänster och välj **ny proxy**.
3. Ange ett namn för proxyservern.
4. Konfigurera hello-slutpunkt som är exponerad på den här funktionen appen genom att ange hello **flödesmallen** och **HTTP-metoderna**. Dessa parametrar fungerar bl.a toohello regler för [http-utlösare].
5. Ange hello **backend-URL** tooanother slutpunkt. Den här slutpunkten kan vara en funktion i en annan funktionsapp eller kan det bero på andra API: er. hello värdet behöver inte toobe statiskt och det kan referera till [programinställningar] och [parametrar från hello ursprungliga klientbegäran].
6. Klicka på **Skapa**.

Proxyservern finns nu som en ny slutpunkt i appen funktion. Ur klienten är det motsvarande tooan HttpTrigger i Azure Functions. Du kan testa den nya proxyserverkonfigurationen genom att kopiera hello Proxy-URL och testa med din favorit HTTP-klient.

## <a name="modify-requests-responses"></a>Ändra begäranden och -svar

Med Azure Functions-proxyservrar, kan du ändra begäranden tooand svar från hello serverdel. Dessa omvandlingar kan använda variabler som definieras i [använda variabler].

### <a name="modify-backend-request"></a>Ändra hello backend-begäran

Som standard initieras hello backend-begäran som en kopia av hello ursprungliga begäran. Dessutom toosetting hello backend-URL, du kan göra ändringar toohello HTTP-metod, rubriker och fråga string-parametrar. hello ändrade värden kan referera till [programinställningar] och [parametrar från hello ursprungliga klientbegäran].

Det finns för närvarande inga portaler för att ändra backend-begäranden. toolearn hur tooapply funktionen från proxies.json, se [definiera ett requestOverrides objekt].

### <a name="modify-response"></a>Ändra hello svar

Som standard initieras hello Klientsvar som en kopia av hello backend-svar. Du kan göra ändringar toohello Svarets statuskod, orsaksfrasen, rubriker och brödtext. hello ändrade värden kan referera till [programinställningar], [parametrar från hello ursprungliga klientbegäran], och [parametrar från hello backend-svar].

Det finns för närvarande inga portaler för att ändra svar. toolearn hur tooapply funktionen från proxies.json, se [definiera ett responseOverrides objekt].

## <a name="using-variables"></a>Använda variabler

hello-konfiguration för en proxy behöver inte toobe statisk. Du kan villkor den toouse variabler från hello ursprungliga begäran, hello backend-svar eller programinställningar.

### <a name="request-parameters"></a>Parametrarna som referens

Du kan använda parametrarna som anger toohello backend-URL: en egenskap eller som en del av ändra begäranden och -svar. Vissa parametrar kan bindas från hello flödesmallen som anges i grundläggande hello-proxykonfigurationen och andra kan komma från egenskaperna för hello inkommande begäran.

#### <a name="route-template-parameters"></a>Vidarebefordra mallparametrar
Parametrar som används i hello flödesmallen är tillgängliga toobe som refereras av namn. hello parameternamn omges av klammerparenteser ({}).

Om exempelvis en proxy som har en flödesmallen `/pets/{petId}`, hello backend-URL kan innehålla hello värdet för `{petId}`, som i `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`. Om hello flödesmallen avslutas med ett jokertecken som `/api/{*restOfPath}`, hello värdet `{restOfPath}` är en strängrepresentation av hello återstående sökvägssegment från hello inkommande begäran.

#### <a name="additional-request-parameters"></a>Parametrar för ytterligare begäran
Dessutom toohello vidarebefordra mallparametrar, hello följande värden kan användas i konfigurationsvärden:

* **{request.method} **: hello HTTP-metod som används på hello ursprungliga begäran.
* **{request.headers. \<HeaderName\>}**: ett huvud som kan läsas från hello ursprungliga begäran. Ersätt * \<HeaderName\> * med hello namnet på hello-huvud som du vill tooread. Om hello-huvud inte ingår i hello begäran, blir hello värdet hello tom sträng.
* **{request.querystring. \<ParameterName\>}**: en frågesträngsparameter som kan läsas från hello ursprungliga begäran. Ersätt * \<ParameterName\> * med hello namnet på hello-parameter som du vill tooread. Om hello-parametern inte finns på hello begäran, blir hello värdet hello tom sträng.

### <a name="response-parameters"></a>Referens för backend-svarsparametrar

Svarsparametrar kan användas som en del av ändra hello svar toohello klienten. hello följande värden kan användas i konfigurationsvärden:

* **{backend.response.statusCode} **: hello HTTP-statuskod som returneras hello backend-svaret.
* **{backend.response.statusReason} **: hello HTTP orsaksfrasen som returneras hello backend-svaret.
* **{backend.response.headers. \<HeaderName\>}**: ett huvud som kan läsas från hello backend-svar. Ersätt * \<HeaderName\> * hello namnet på hello-huvud ska tooread. Om hello-huvud inte ingår i hello begäran, blir hello värdet hello tom sträng.

### <a name="use-appsettings"></a>Referens för programinställningar

Du kan också referera [programinställningar som definierats för hello funktionsapp](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) genom att omge hello inställningsnamn med procenttecken (%).

Till exempel backend-URL: en *https://%ORDER_PROCESSING_HOST%/api/orders* skulle ha ”% ORDER_PROCESSING_HOST %” ersätts med hello hello ORDER_PROCESSING_HOST inställningens värde.

> [!TIP] 
> Använd programinställningar för backend-värdar när du har flera distributioner eller testmiljöer. På så sätt kan du se till att du alltid pratar toohello tillbaka slut för den miljön.

## <a name="advanced-configuration"></a>Avancerad konfiguration

hello-proxyservrar som du konfigurerar lagras i en proxies.json-fil som finns i hello roten för en funktion programkatalogen. Du kan manuellt redigera den här filen och distribuera det som en del av din app när du använder någon av hello [distributionsmetoder](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) som stöder funktioner. hello-funktionen måste vara [aktiverat](#enable) för hello filen toobe bearbetas. 

> [!TIP] 
> Om du inte har angett något hello distributionsmetoder, kan du också fungera med hello proxies.json filen i hello-portalen. Gå tooyour funktionsapp, Välj **plattformsfunktioner**, och välj sedan **App Service Editor**. På så sätt, kan du visa hello hela filen strukturen för din funktionsapp och göra ändringar.

Proxies.JSON definieras av ett proxyservrar-objekt som består av namngivna proxyservrar och deras definitioner. Du kan också om redigeringsprogram stöder detta, kan du referera en [JSON-schema](http://json.schemastore.org/proxies) för kod slutförande. En exempelfil kan se ut hello följande:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>"
        }
    }
}
```

Varje proxy har ett eget namn som *proxy1* i föregående exempel hello. hello motsvarande definition proxyobjekt definieras av hello följande egenskaper:

* **matchCondition**: krävs – ett objekt som definierar hello-begäranden som utlöser hello körningen av denna proxy. Den innehåller två egenskaper som delas med [http-utlösare]:
    * _metoder_: en matris med hello HTTP-metoder som hello proxy svarar. Om den inte är angiven svarar hello proxy tooall HTTP-metoderna för hello flödet.
    * _väg_: krävs--definierar hello flödesmallen styra som begära webbadresserna din proxyserver svarar på. Till skillnad från i HTTP-utlösare har inget standardvärde.
* **backendUri**: hello Webbadress för hello backend-resurs toowhich hello begäran måste vara via proxy. Det här värdet kan referera till programinställningar och parametrar från hello ursprungliga klientbegäran. Om den här egenskapen inte är inkluderad svarar Azure Functions med en HTTP-200 OK.
* **requestOverrides**: ett objekt som definierar transformationer toohello backend-begäran. Se [definiera ett requestOverrides objekt].
* **responseOverrides**: ett objekt som definierar transformationer toohello klientsvar. Se [definiera ett responseOverrides objekt].

> [!NOTE] 
> hello flödet egenskapen Azure Functions proxyservrar inte att behandla hello routePrefix-egenskapen för Värdkonfiguration för hello funktioner. Om du vill tooinclude ett prefix, till exempel /api, måste det ingå i hello flödet egenskap.

### <a name="requestOverrides"></a>Definiera ett requestOverrides-objekt

Hej requestOverrides objektet definierar ändringar som gjorts toohello begäran när hello backend-resursen anropas. hello objekt definieras av hello följande egenskaper:

* **backend.Request.Method**: hello HTTP-metod som har använt toocall hello-serverdel.
* **backend.Request.QueryString. \<ParameterName\>**: en frågesträngsparameter som kan anges för anrop hello toohello serverdel. Ersätt * \<ParameterName\> * med hello namnet på hello-parameter som du vill tooset. Om hello tom sträng ingår inte hello parametern på hello backend-begäran.
* **backend.Request.headers. \<HeaderName\>**: ett huvud som kan anges för anrop hello toohello serverdel. Ersätt * \<HeaderName\> * med hello namnet på hello-huvud som du vill tooset. Om du tillhandahåller hello tom sträng ingår inte hello sidhuvud på hello backend-begäran.

Värden kan referera till programinställningar och parametrar från hello ursprungliga klientbegäran.

En exempelkonfiguration kan se ut hello följande:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>",
            "requestOverrides": {
                "backend.request.headers.Accept": "application/xml",
                "backend.request.headers.x-functions-key": "%ANOTHERAPP_API_KEY%"
            }
        }
    }
}
```

### <a name="responseOverrides"></a>Definiera ett responseOverrides-objekt

Hej requestOverrides objektet definierar ändringar som görs toohello svar som har gått tillbaka toohello klienten. hello objekt definieras av hello följande egenskaper:

* **response.statusCode**: hello HTTP-status kod toobe returnerade toohello klienten.
* **response.statusReason**: hello HTTP orsak frasen toobe returnerade toohello klienten.
* **Response.body**: hello strängrepresentation av hello brödtext toobe returnerade toohello klienten.
* **Response.headers. \<HeaderName\>**: ett huvud som kan anges för hello svar toohello klienten. Ersätt * \<HeaderName\> * med hello namnet på hello-huvud som du vill tooset. Om du tillhandahåller hello tom sträng ingår inte hello sidhuvud hello svaret.

Värden kan referera till programinställningar, parametrar från hello ursprungliga klientbegäran och parametrar från hello backend-svar.

En exempelkonfiguration kan se ut hello följande:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "responseOverrides": {
                "response.body": "Hello, {test}",
                "response.headers.Content-Type": "text/plain"
            }
        }
    }
}
```
> [!NOTE] 
> I det här exemplet hello brödtext anges direkt, så ingen `backendUri` egenskapen krävs. hello exempel visas hur du kan använda Azure Functions proxyservrar för mocking API: er.

[Azure Portal]: https://portal.azure.com
[HTTP-utlösare]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger
[Modify hello back-end request]: #modify-backend-request
[Modify hello response]: #modify-response
[Definiera ett requestOverrides-objekt]: #requestOverrides
[Definiera ett responseOverrides-objekt]: #responseOverrides
[programinställningar]: #use-appsettings
[Använda variabler]: #using-variables
[parametrar från hello ursprungliga klientbegäran]: #request-parameters
[parametrar från hello backend-svar]: #response-parameters
