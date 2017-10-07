---
title: "aaaWorkflow åtgärder och utlösare - Azure Logic Apps | Microsoft Docs"
description: 
services: logic-apps
author: MandiOhlinger
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 86a53bb3-01ba-4e83-89b7-c9a7074cb159
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/17/2016
ms.author: LADocs; mandia
ms.openlocfilehash: 857927b7d7df3fc9cdc4931ffdb613efde0db9f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="workflow-actions-and-triggers-for-azure-logic-apps"></a>Arbetsflödesåtgärder och utlösare för Logic Apps i Azure

Logikappar består av utlösare och åtgärder. Det finns sex olika typer av utlösare. Varje typ har olika gränssnitt och olika beteenden. Du kan också lära dig om andra uppgifter genom att titta på hello information om hello [språk i arbetsflödesdefinitionen](logic-apps-workflow-definition-language.md).  
  
Läsa på toolearn mer information om utlösare och åtgärder och hur du kan använda dem toobuild logic apps tooimprove dina affärsprocesser och arbetsflöden.  
  
### <a name="triggers"></a>Utlösare  

En utlösare Anger hello-anrop som kan initiera en körning av arbetsflödet logik app. Här följer hello två olika sätt tooinitiate arbetsflödet körs:  
  
-   En avsökning utlösare  

-   En push-utlösare - genom att anropa hello [arbetsflöde Service REST API](https://docs.microsoft.com/rest/api/logic/workflows)  
  
Alla utlösare innehålla de översta elementen:  
  
```json
"<name-of-the-trigger>" : {
    "type": "<type-of-trigger>",
    "inputs": { <settings-for-the-call> },
    "recurrence": {  
        "frequency": "Second|Minute|Hour|Week|Month|Year",
        "interval": "<recurrence interval in units of frequency>"
    },
    "conditions": [ <array-of-required-conditions > ],
    "splitOn" : "<property toocreate runs for>",
    "operationOptions": "<operation options on hello trigger>"
}
```

### <a name="trigger-types-and-their-inputs"></a>Typer av utlösare och deras indata  

Du kan använda dessa typer av utlösare:
  
-   **Begära** \- gör hello logikapp en slutpunkt för du toocall  
  
-   **Återkommande** \- utlöses baserat på ett definierat schema  
  
-   **HTTP** \- avsöker en HTTP-slutpunkt för webbprogram. hello HTTP-slutpunkten måste uppfylla tooa specifik utlösande kontraktet \- antingen med hjälp av en 202\-asynk.mönster, eller genom att returnera en matris  
  
-   **ApiConnection** \- Utlös avsökningar som hello HTTP, men den kan du utnyttja hello [Microsoft-hanterade API: er](https://docs.microsoft.com/azure/connectors/apis-list)  
  
-   **HTTPWebhook** \- öppnas en slutpunkt och liknande toohello manuell utlösare, men också anropar ut tooa angivna URL: en tooregister och avregistrera  
  
-   **ApiConnectionWebhook** \- fungerar som hello HTTPWebhook utlösaren genom att utnyttja hello Microsoft-hanterade API: er       
    Varje typ av utlösare som har en annan uppsättning **indata** som definierar sitt beteende.  
  
## <a name="request-trigger"></a>Begäran utlösare  

Den här utlösaren fungerar som en slutpunkt som anropas via en HTTP-begäran tooinvoke logikappen. Det ser ut som i det här exemplet en utlösare för begäran:  
  
```json
"<name-of-the-trigger>" : {
    "type" : "request",
    "kind": "http",
    "inputs" : {
        "schema" : {
            "properties" : {
                "myInputProperty1" : { "type" : "string" },
                "myInputProperty2" : { "type" : "number" }
            },
        "required" : [ "myInputProperty1" ],
        "type" : "object"
        }
    }
} 
```

Det finns även en valfri egenskap som kallas **schemat**:  
  
|Elementnamn|Krävs|Beskrivning|  
|----------------|------------|---------------|  
|Schemat|Nej|En JSON-schema som validerar hello inkommande begäran. Användbart för att hjälpa efterföljande arbetsflödessteg veta vilka egenskaper tooreference.|

tooinvoke den här slutpunkten måste toocall hello *listCallbackUrl* API. Se [arbetsflöde Service REST API](https://docs.microsoft.com/rest/api/logic/workflows).  
  
## <a name="recurrence-trigger"></a>Utlösare för upprepning  

En utlösare för upprepning är något som körs baserat på ett definierat schema. Dessa utlösare kan se ut det här exemplet:  

```json
"dailyReport" : {
    "type": "recurrence",
    "recurrence": {
        "frequency": "Day",
        "interval": "1"
    }
}
```

Som du ser är ett enkelt sätt toorun ett arbetsflöde.  
  
|Elementnamn|Krävs|Beskrivning|  
|----------------|------------|---------------|  
|frequency|Ja|Hur ofta hello utlösaren körs. Använd endast ett av dessa värden: sekund, minut, timma, dag, vecka, månad eller år|  
|interval|Ja|Hello angivna frekvensen för hello återkommande tidsintervall|  
|startTime|Nej|Om en startTime anges utan en UTC-förskjutningen, används den här tidszonen.|  
|Tidszon|Nej|Om en startTime anges utan en UTC-förskjutningen, används den här tidszonen.|  
  
Du kan även schemalägga en utlösare toostart körs på någon punkt i hello framtida. Till exempel om du vill att en vecka rapportera varje måndag toostart kan du schemalägga hello logik app toostart varje måndag genom att skapa hello följande utlösare:  

```json
"dailyReport" : {
    "type": "recurrence",
    "recurrence": {
        "frequency": "Week",
        "interval": "1",
        "startTime" : "2015-06-22T00:00:00Z"
    }
}
```

## <a name="http-trigger"></a>HTTP-utlösare  

HTTP-utlösare avsöker den angivna slutpunkten och kontrollera hello svar toodetermine om hello arbetsflödet ska köras. hello indata objektet tar hello uppsättning parametrar krävs tooconstruct ett HTTP-anrop:  
  
|Elementnamn|Krävs|Beskrivning|Typ|  
|----------------|------------|---------------|--------|  
|Metoden|Ja|Kan vara något av följande HTTP-metoderna hello: GET, POST, PUT, DELETE, KORRIGERINGSFIL eller HEAD|Sträng|  
|URI: N|Ja|hello http eller https-slutpunkt som anropas. Högst 2 kilobyte.|Sträng|  
|frågor|Nej|Ett objekt som representerar hello frågan parametrar tooadd toohello URL. Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` toohello URL.|Objekt|  
|Rubriker|Nej|Ett objekt som representerar alla hello-huvuden som skickas toohello begäran. Till exempel tooset hello språk och typen på en begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|Objekt|  
|Brödtext|Nej|Ett objekt som representerar hello-nyttolast som har skickats toohello slutpunkt.|Objekt|  
|retryPolicy|Nej|Ett objekt som kan du anpassa hello försök beteende för 4xx eller 5xx-fel.|Objekt|  
|Autentisering|Nej|Representerar hello metod som hello begäran ska autentiseras. Mer information om det här objektet finns [Scheduler utgående autentisering](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication). Utöver scheduler, är en mer stöds egenskap: `authority` det här värdet är som standard `https://login.windows.net` om anges, men du kan använda en annan publik som`https://login.windows\-ppe.net`|Objekt|  
  
hello HTTP-utlösaren kräver hello HTTP API tooconform med ett specifikt mönster toowork tillsammans med din logikapp. Det krävs hello följande fält:  
  
|Svar|Beskrivning|  
|------------|---------------|  
|Statuskod|Statuskod 200 \(OK\) toocause körs. Andra statuskod göra inte en körning.|  
|Försök\-efter huvudet|Antalet sekunder tills hello logikapp avsöker hello slutpunkt igen.|  
|Platshuvud|hello URL toocall på hello nästa avsökningsintervall. Om inget anges används hello ursprungliga URL: en.|  
  
Här följer några exempel på olika beteenden för olika typer av begäranden:  
  
|Svarskod|Försök\-efter|Beteende|  
|-----------------|----------------|------------|  
|200|\(Ingen\)|Inte en giltig utlösare, försök igen\-efter är obligatorisk eller annan hello motor aldrig söker efter nästa hello-begäran.|  
|202|60|Utlöses inte hello arbetsflöde. hello försöker sker i en minut.|  
|200|10|Kör hello arbetsflöde och Sök igen efter mer innehåll om 10 sekunder.|  
|400|\(Ingen\)|Felaktig begäran körs inte hello arbetsflöde. Om det finns inga **försök princip** definierats hello standardprincipen används. När hello antalet försök har nåtts, är hello utlösare inte längre giltig.|  
|500|\(Ingen\)|Hello arbetsflödet körs inte på Server-fel.  Om det finns inga **försök princip** definierats hello standardprincipen används. När hello antalet försök har nåtts, är hello utlösare inte längre giltig.|  
  
Det ser ut som i det här exemplet Hello utdata för en HTTP-utlösare:  
  
|Elementnamn|Beskrivning|Typ|  
|----------------|---------------|--------|  
|Rubriker|hello-huvuden hello http-svar.|Objekt|  
|Brödtext|hello brödtext hello http-svar.|Objekt|  
  
## <a name="api-connection-trigger"></a>Utlösare för API-anslutningen  

hello API anslutning utlösaren är liknande toohello http-utlösaren i dess grundläggande funktioner. Dock är hello parametrar för att identifiera hello åtgärd olika. Här är ett exempel:  
  
```json
"dailyReport" : {
    "type": "ApiConnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://myarticles.example.com/"
            },
        }
        "connection": {
            "name": "@parameters('$connections')['myconnection'].name"
        }
    },  
    "method": "POST",
    "body": {
        "category": "awesomest"
    }
}
```

|Elementnamn|Krävs|Typ|Beskrivning|  
|----------------|------------|--------|---------------|  
|värden|Ja||Hej ApiApp finns gateway och ID: t.|  
|Metoden|Ja|Sträng|Kan vara något av följande HTTP-metoderna hello: **hämta**, **POST**, **PLACERA**, **ta bort**, **korrigering**, eller  **HUVUDET**|  
|frågor|Nej|Objekt|Representerar hello frågan parametrar toobe lägga toohello URL. Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` toohello URL.|  
|Rubriker|Nej|Objekt|Representerar varje hello-huvuden som skickas toohello begäran. Till exempel tooset hello språk och typen på en begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Brödtext|Nej|Objekt|Representerar hello-nyttolast som har skickats toohello slutpunkt.|  
|retryPolicy|Nej|Objekt|Låter dig toocustomize hello försök beteende för 4xx eller 5xx-fel.|  
|Autentisering|Nej|Objekt|Representerar hello metod som hello begäran ska autentiseras. Mer information om det här objektet finns [Scheduler utgående autentisering](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)|  
  
hello egenskaper för värden är:  
  
|Elementnamn|Krävs|Beskrivning|  
|----------------|------------|---------------|  
|API-runtimeUrl|Ja|hello-slutpunkten för hello hanterade API.|  
|Anslutningens namn||Måste en referens tooa parameter anropas `$connection` och är hello på hello hanterade API-anslutningen som hello arbetsflödet använder.|
  
hello utdata för en utlösare för API-anslutning är:
  
|Elementnamn|Typ|Beskrivning|  
|----------------|--------|---------------|  
|Rubriker|Objekt|hello-huvuden hello http-svar.|  
|Brödtext|Objekt|hello brödtext hello http-svar.|  
  
## <a name="httpwebhook-trigger"></a>HTTPWebhook utlösare  

Hej HTTPWebhook utlösaren öppnas en slutpunkt, liknande toohello manuell utlösare, men hello HTTPWebhook utlösaren anropar också ut tooa angivna URL: en tooregister och avregistrera. Här är ett exempel på hur en HTTPWebhook-utlösare kan se ut:  

```json
"myappspottrigger": {
    "type": "httpWebhook",
    "inputs": {
        "subscribe": {
            "method": "POST",
            "uri": "https://pubsubhubbub.appspot.com/subscribe",
            "headers": { },
            "body": {
                "hub.callback": "@{listCallbackUrl()}",
                "hub.mode": "subscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "authentication": { },
            "retryPolicy": { }
        },
        "unsubscribe": {
            "url": "https://pubsubhubbub.appspot.com/subscribe",
            "body": {
                "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
                "hub.mode": "unsubscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "method": "POST",
            "authentication": { }
        }
    },
    "conditions": [ ]
    }
```

Många av dessa avsnitt är valfria och hello beteendet för hello Webhook beror på vilka avsnitt som eller utelämnas.  
hello egenskaperna för en Webhook är följande:  
  
|Elementnamn|Krävs|Beskrivning|  
|----------------|------------|---------------|  
|prenumerera på|Nej|hello utgående begäran som anropas när hello utlösare skapas och utför hello registreringen.|  
|avbryta prenumerationen|Nej|hello utgående begäran när hello utlösaren tas bort.|  
  
-   **Prenumerera på** är hello utgående anrop som har gjorts toostart lyssnande tooevents. Det här anropet börjar med hello har samma uppsättning parametrar som hello vanliga HTTP-åtgärder. Utgående anropet görs varje gång hello ändringar av arbetsflödet på något sätt, till exempel när hello autentiseringsuppgifter samlas eller hello utlösaren input parametrar ändringen.
  
    toosupport detta anrop, det finns en ny funktion: `@listCallbackUrl()`. Den här funktionen returnerar en unik URL för den här specifika utlösaren i det här arbetsflödet. Det motsvarar hello unika identifieraren för hello-slutpunkter som använder hello Service REST.  
  
-   **Avbryta prenumerationen** anropas när en åtgärd återgivningar utlösaren är ogiltig, inklusive:  
  
    -   Ta bort eller inaktivera hello utlösare  
  
    -   Ta bort eller inaktivera hello-arbetsflöde  
  
    -   Ta bort eller inaktivera hello prenumeration  
  
    Hej logikapp automatiskt anropar hello avbryta prenumerationen åtgärd. hello parametrar toothis funktion är hello samma som hello http-utlösare.  
  
    hello är utdata för hello HTTPWebhook utlösare hello innehållet i hello inkommande begäran:  
  
|Elementnamn|Typ|Beskrivning|  
|-----------------|--------|---------------|  
|Rubriker|Objekt|hello-huvuden hello HTTP-begäran.|  
|Brödtext|Objekt|hello brödtext hello HTTP-begäran.|  

Gränserna för ett webhook-åtgärden kan anges i hello samma sätt som [HTTP asynkron gränser](#asynchronous-limits).
  

## <a name="conditions"></a>Villkor  

Du kan använda en eller flera villkor toodetermine om hello arbetsflödet ska köras eller inte för alla utlösare. Exempel:  

```json
"dailyReport" : {
    "type": "recurrence",
    "conditions": [ {
        "expression": "@parameters('sendReports')"
    } ],
    "recurrence": {
        "frequency": "Day",
        "interval": "1"
    }
}
```

I det här fallet hello rapporten endast utlösare när hello arbetsflöde `sendReports` tootrue är angiven. Slutligen får villkor referera hello statuskod hello utlösare. Exempelvis kan startar du ett arbetsflöde endast när din webbplats returnerar en statuskod 500, enligt följande:
  
```  
"conditions": [  
        {  
          "expression": "@equals(triggers().code, 'InternalServerError')"  
        }  
      ]  
```  
  
> [!NOTE]  
> När ett uttryck som refererar till hello statuskod hello utlösare \(på något sätt\), hello standardbeteendet \(utlösaren endast på 200 \(OK\) \) ersätts. Till exempel om du vill tootrigger både statuskod 200 och statuskod 201 ha tooinclude: `@or(equals(triggers().code, 200),equals(triggers().code,201))` som dina villkor.  
  
## <a name="start-multiple-runs-for-a-request"></a>Starta flera körs för en begäran

tookick av flera körningar för en enskild begäran `splitOn` är användbart, till exempel om du vill toopoll en slutpunkt som kan ha flera nya objekt mellan avsökningsintervall.
  
Med `splitOn`, du anger hello-egenskapen i hello svar-nyttolast som innehåller hello matris av objekt, som du vill toouse toostart hello utlösaren körs. Anta att du har en API som returnerar hello efter svar:  
  
```json
{
    "Status" : "success",
    "Rows" : [
        {  
            "id" : 938109380,
            "name" : "mycoolrow"
        },
        {
            "id" : 938109381,
            "name" : "another row"
        }
    ]
}
```
  
Din logikapp behöver endast hello rader innehåll, så du kan skapa en utlösare som det här exemplet:  
  
```json
"mysplitter" : {
    "type" : "http",
    "recurrence": {
        "frequency": "Minute",
        "interval": "1"
    },
    "intputs" : {
        "uri" : "https://mydomain.com/myAPI",
        "method" : "GET"
    },
    "splitOn" : "@triggerBody()?.Rows"
}
```
  
Sedan hello arbetsflödesdefinitionen `@triggerBody().name` returnerar `mycoolrow` för hello först köra och `another row` för hello andra kör. hello utlösaren utdata ser ut som i det här exemplet:  
  
```json
{
    "body" : {
        "id" : 938109381,
        "name" : "another row"
    }
}
```

Om du använder `SplitOn`, du kan inte hämta hello egenskaper som är utanför hello matris i det här fallet hello `Status` fältet.  
  
> [!NOTE]  
> I det här exemplet använder vi hello `?` operatorn toobe kan tooavoid ett fel om hello `Rows` egenskapen finns inte. 
  
## <a name="single-run-instance"></a>Kör instans

Du kan konfigurera utlösare som har en upprepning egenskapen tooonly brand om alla aktiva körningar har slutförts. Om en schemalagd upprepning inträffar när det finns en pågående kör hoppar över hello utlösare och väntar tills hello nästa schemalagda återkommande intervall toocheck igen.

Du kan konfigurera den här inställningen via hello åtgärden alternativ:

```json
"triggers": {
    "mytrigger": {
        "type": "http",
        "inputs": { ... },
        "recurrence": { ... },
        "operationOptions": "singleInstance"
    }
}
```

## <a name="types-and-inputs"></a>Typer och indata  

Det finns många typer av åtgärder med unika beteende. Åtgärder för samlingen kan innehålla flera andra åtgärder i sig själv.

### <a name="standard-actions"></a>Standardåtgärder  

-   **HTTP** den här åtgärden kräver en HTTP-slutpunkt för webbprogram.  
  
-   **ApiConnection** \- åtgärden fungerar som en hello HTTP-åtgärd, men använder hello Microsoft-hanterade API: er.  
  
-   **ApiConnectionWebhook** \- som HTTPWebhook, men använder hello Microsoft-hanterade API: er.  
  
-   **Svaret** \- åtgärden definierar ett svar för ett inkommande samtal.  
  
-   **Vänta** \- åtgärden enkel väntar på ett fast belopp tid eller tills en viss tid.  
  
-   **Arbetsflödet** \- representerar ett inkapslat arbetsflöde för den här åtgärden.  

-   **Funktionen** \- åtgärden representerar en Azure-funktion.

### <a name="collection-actions"></a>Åtgärder för samlingen

-   **Scope** \- den här åtgärden är en logisk gruppering av andra åtgärder.

-   **Villkoret** \- åtgärden utvärderar ett uttryck och kör hello motsvarande resultatet grenen.

-   **ForEach** \- slingor åtgärden upprepas i en matris och utför inre åtgärder för varje objekt.

-   **Tills** \- åtgärden slingor kör inre åtgärder tills ett villkor resulterar tootrue.
  
Varje typ av åtgärd har en annan uppsättning **indata** som definierar beteendet för en åtgärd.  
  
## <a name="http-action"></a>HTTP-åtgärd  

HTTP-åtgärder anropa den angivna slutpunkten och kontrollera hello svar toodetermine om hello arbetsflödet ska köras. Hej **indata** objektet tar hello uppsättning parametrar krävs tooconstruct hello HTTP anropet:  
  
|Elementnamn|Krävs|Typ|Beskrivning|  
|----------------|------------|--------|---------------|  
|Metoden|Ja|Sträng|Kan vara något av följande HTTP-metoderna hello: **hämta**, **POST**, **PLACERA**, **ta bort**, **korrigering**, eller  **HUVUDET**|  
|URI: N|Ja|Sträng|hello http eller https-slutpunkt som anropas. Maximal längd är 2 kB.|  
|frågor|Nej|Objekt|Representerar hello frågan parametrar tooadd toohello URL. Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` toohello URL.|  
|Rubriker|Nej|Objekt|Representerar varje hello-huvuden som skickas toohello begäran. Till exempel tooset hello språk och typen på en begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Brödtext|Nej|Objekt|Representerar hello-nyttolast som har skickats toohello slutpunkt.|  
|retryPolicy|Nej|Objekt|Kan du anpassa hello försök beteende för 4xx eller 5xx-fel.|  
|operationsOptions|Nej|Sträng|Definierar hello särskilda beteenden toooverride.|  
|Autentisering|Nej|Objekt|Representerar hello metod som hello begäran ska autentiseras. Mer information om det här objektet finns [Scheduler utgående autentisering](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication). Utöver scheduler, är en mer stöds egenskap: `authority`. Detta är som standard `https://login.windows.net` om anges, men du kan använda en annan publik som`https://login.windows\-ppe.net`|  
  
HTTP-åtgärder \(och API-anslutningen\) åtgärder stöd försök principer. En återförsöksprincip gäller toointermittent fel, kännetecknas som HTTP-status 408, 429 och 5xx i tillägg tooany undantag. Den här principen nedan används hello *retryPolicy* objekt som är definierat som visas här:
  
```json
"retryPolicy" : {
    "type": "<type-of-retry-policy>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```
  
hello återförsöksintervall har angetts i hello ISO 8601-format. Det här intervallet har en standard och minsta 20 sekunder medan hello maxvärdet är en timme. hello standard och maximalt antal för nya försök är fyra timmar. Om hello försök principdefinitionen anges en `fixed` strategi som används med försök antal och intervall standardvärden. toodisable hello återförsöksprincip, ange dess för`None`.  
  
Till exempel återförsök hello följande åtgärd hämtning hello nyheter två gånger om återkommande fel totalt tre körningar med en fördröjning på 30 sekunder mellan varje försök:  
  
```json
"latestNews" : {
    "type": "http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT30S",
            "count": 2
        }
    }
}
```
### <a name="asynchronous-patterns"></a>Asynkront mönster

Som standard stöder alla HTTP-baserade åtgärder hello standard asynkron åtgärd mönster. Så om hello fjärrservern visar hello begäran är godkänd för behandling med en 202 \(godkända\) svar, hello Logic Apps motorn håller avsökning hello-URL som anges i hello svar platshuvud tills en terminal tillstånd \(inte\-202 svar\).  
  
toodisable hello asynkron beteende tidigare beskrivs, ange en `DisableAsyncPattern` alternativ i hello åtgärd indata. I det här fallet är hello utdata från hello åtgärden baserad på hello inledande 202 svaret från hello-servern.  
  
```json
"invokeLongRunningOperation" : {
    "type": "http",
    "inputs": {
        "method": "POST",
        "uri": "https://host.example.com/resources"
    },
    "operationOptions": "DisableAsyncPattern"
}
```

#### <a name="asynchronous-limits"></a>Asynkron gränser

Ett asynkront mönster kan begränsas i dess duration tooa angivet tidsintervall.  Om hello tidsintervall långa utan att ett avslutat tillstånd hello status för hello åtgärd markeras `Cancelled` med koden `ActionTimedOut`.  hello gränsen timeout anges i ISO 8601-format.  Gränser kan anges med hello följande syntax:

``` json
"<action-name>": {
    "type": "workflow|webhook|http|apiconnectionwebhook|apiconnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="api-connection"></a>API-anslutningen  

API-anslutningen är en åtgärd som refererar till en Microsoft-hanterad koppling.
Den här åtgärden kräver en giltig referens tooa-anslutning och information om hello API och parametrar som krävs.

|Elementnamn|Krävs|Typ|Beskrivning|  
|----------------|------------|--------|---------------|  
|värden|Ja|Objekt|Representerar hello connector information, till exempel hello referens och runtimeUrl toohello anslutningsobjekt|
|Metoden|Ja|Sträng|Kan vara något av följande HTTP-metoderna hello: **hämta**, **POST**, **PLACERA**, **ta bort**, **korrigering**, eller  **HUVUDET**|  
|Sökväg|Ja|Sträng|hello sökväg hello API-åtgärden.|  
|frågor|Nej|Objekt|Representerar hello frågan parametrar tooadd toohello URL. Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` toohello URL.|  
|Rubriker|Nej|Objekt|Representerar varje hello-huvuden som skickas toohello begäran. Till exempel tooset hello språk och typen på en begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Brödtext|Nej|Objekt|Representerar hello-nyttolast som har skickats toohello slutpunkt.|  
|retryPolicy|Nej|Objekt|Kan du anpassa hello försök beteende för 4xx eller 5xx-fel.|  
|operationsOptions|Nej|Sträng|Definierar hello särskilda beteenden toooverride.|  

```json
"Send_Email": {
    "type": "apiconnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "method": "post",
        "body": {
            "Subject": "New Tweet from @{triggerBody()['TweetedBy']}",
            "Body": "@{triggerBody()['TweetText']}",
            "To": "me@example.com"
        },
        "path": "/Mail"
    },
    "runAfter": {}
    }
```

## <a name="api-connection-webhook-action"></a>API-anslutningen webhook åtgärd

```json
"Send_approval_email": {
    "type": "apiconnectionwebhook",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "body": {
            "Message": {
                "Subject": "Approval Request",
                "Options": "Approve, Reject",
                "Importance": "Normal",
                "To": "me@email.com"
            }
        },
        "path": "/approvalmail",
        "authentication": "@parameters('$authentication')"
    },
    "runAfter": {}
}
```

Gränserna för ett webhook-åtgärden kan anges i hello samma sätt som [HTTP asynkron gränser](#asynchronous-limits).
  
## <a name="response-action"></a>Svaret åtgärd  

Den här typen innehåller hello hela svaret nyttolast från en HTTP-begäran och ett statusCode, brödtext och rubriker:  
  
```json
"myresponse" : {
    "type" : "response",
    "inputs" : {
        "statusCode" : 200,
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        }
    },
    "runAfter": {}
}
```
  
hello svar åtgärd har särskilda begränsningar som inte gäller tooother åtgärder. Närmare bestämt:  
  
-   Responsåtgärder får inte vara parallellt i en definition av eftersom en inkommande begäran om toohello deterministiska svar krävs.  
  
-   Om en åtgärd för svar uppnås när hello inkommande begäran tog emot ett svar, hello anses misslyckades \(konflikt\), och därför hello kör `Failed`.  
  
-   Ett arbetsflöde med responsåtgärder kan inte ha `splitOn` i dess utlösare eftersom ett anrop gör många körs. Det bör därför verifieras när hello flödet är PUT- och orsaken en felaktig begäran.  
  
## <a name="wait-action"></a>Vänta åtgärd  

Hej `wait` åtgärd pausar körningen av arbetsflödet för hello angivna intervallet. Du kan använda den här fragment exempelvis toowait 15 minuter:  
  
```json
"waitForFifteenMinutes" : {
    "type": "wait",
    "inputs": {
        "interval": {
            "unit" : "minute",
            "count" : 15
        }
    }
}
```  
  
Du kan också toowait tills en särskild tidpunkt, du kan använda det här exemplet:  
  
```json
"waitUntilOctober" : {
    "type": "wait",
    "inputs": {
        "until": {
            "timestamp" : "2016-10-01T00:00:00Z"
        }
    }
}
```
  
> [!NOTE]  
> hello vänta varaktighet kan anges antingen med hjälp av hello **intervall** objekt eller hello **tills** objekt, men inte båda.  
  
|Namn|Krävs|Typ|Beskrivning|  
|--------|------------|--------|---------------|  
|interval|Nej|Objekt|Hej väntans varaktighet baserat på tid.|  
|intervall|Ja|Sträng|En av dessa intervall: sekund, minut, timma, dag, vecka, månad, år.|  
|intervall för antal|Ja|Sträng|Varaktighet baserat på hello angiven interna enhet.|  
|fram till|Nej|Objekt|Hej väntans varaktighet baserat på en punkt i tiden.|  
|tills tidsstämpel|Ja|Sträng|Sträng &#124; hello tidpunkt i UTC när hello vänta upphör att gälla.|  

## <a name="query-action"></a>Frågeåtgärden

Hej `query` åtgärd kan du filtrera en matris baserat på ett villkor. Till exempel tooselect tal som är större än 2, du kan använda:

```json
"FilterNumbers" : {
    "type": "query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

Hej utdata från hello `query` åtgärden är en matris som har element från hello Indatamatrisen som uppfyller villkoret hello.

> [!NOTE]
> Om inga värden uppfyller hello `where` villkor, hello resultatet är en tom matris.

|Namn|Krävs|Typ|Beskrivning|
|--------|------------|--------|---------------|
|Från|Ja|Matris|hello källmatrisen.|
|där|Ja|Sträng|hello villkoret tooapply tooeach element i källmatrisen hello.|

## <a name="select-action"></a>Välj åtgärd

Hej `select` du kan projicera varje element i en matris till ett nytt värde.
Till exempel tooconvert en matris av talen i en matris av objekt, kan du använda:

```json
"SelectNumbers" : {
    "type": "select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

Hej utdata från hello `select` åtgärden är en matris som har hello samma kardinalitet som hello Indatamatrisen, där varje element omvandlas som definierats av hello `select` egenskapen. Om hello-indata är en tom matris, hello utdata är också en tom matris.

|Namn|Krävs|Typ|Beskrivning|
|--------|------------|--------|---------------|
|Från|Ja|Matris|hello källmatrisen.|
|Välj|Ja|Alla|hello projektion tooapply tooeach element i källmatrisen hello.|

## <a name="terminate-action"></a>Åtgärden Avbryt

hello avbrottsåtgärd avbryts körningen av hello arbetsflödet körs, avbryts alla pågående åtgärder och hoppa över eventuella återstående åtgärder. Till exempel tooterminate körs med statusen **misslyckades**, kan du använda följande fragment hello:

```json
"HandleUnexpectedResponse" : {
    "type": "terminate",
    "inputs": {
        "runStatus" : "failed",
        "runError": {
            "code": "UnexpectedResponse",
            "message": "Received an unexpected response.",
        }
    }
}
```

> [!NOTE]
> Åtgärder som redan har slutförts påverkas inte av hello Avbryt.

|Namn|Krävs|Typ|Beskrivning|
|--------|------------|--------|---------------|
|runStatus|Ja|Sträng|hello mål Körningsstatus. Antingen **misslyckades** eller **avbröts**.|
|runError|Nej|Objekt|hello felinformation. Endast när **runStatus** har angetts för**misslyckades**.|
|runError kod|Nej|Sträng|hello kör felkoden.|
|runError meddelande|Nej|Sträng|hello kör felmeddelande.|

## <a name="compose-action"></a>Skriv åtgärd

hello Skriv åtgärd kan du skapa ett godtyckligt objekt. hello utdata från hello utgöra åtgärd är hello resultat av utvärderingen av indata. Du kan till exempel använda hello utgöra åtgärd toomerge utdata för flera åtgärder:

```json
"composeUserRecord" : {
    "type": "compose",
    "inputs": {
        "firstName": "@actions('getUser').firstName",
        "alias": "@actions('getUser').alias",
        "thumbnailLink": "@actions('lookupThumbnail').url"
        }
    }
}
```

> [!NOTE]
> Hej **Compose** åtgärd kan vara används tooconstruct inga utdata, inklusive objekt, matriser och andra typer som stöds av logikappar som XML och binary.

## <a name="table-action"></a>Åtgärden för tabellen

Hej `table` kan du tooconvert en matris med objekt till en **CSV** eller **HTML** tabell.

Anta att @triggerBodyär)

```json
[{
  "id": 0,
  "name": "apples"
},{
  "id": 1, 
  "name": "oranges"
}]
```

Och låta hello åtgärd definieras som

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html"
    }
}
```

hello ovan skulle skapa

<table><thead><tr><th>id</th><th>namn</th></tr></thead><tbody><tr><td>0</td><td>Äpplen</td></tr><tr><td>1</td><td>orange</td></tr></tbody></table>"

I ordning toocustomize hello tabellen du vill kan du ange hello kolumner explicit. Exempel:

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html",
        "columns": [{
          "header": "produce id",
          "value": "@item().id"
        },{
          "header": "description",
          "value": "@concat('fresh ', item().name)"
        }]
    }
}
```

hello ovan skulle skapa

<table><thead><tr><th>Skapa id</th><th>description</th></tr></thead><tbody><tr><td>0</td><td>ny äpplen</td></tr><tr><td>1</td><td>ny orange</td></tr></tbody></table>"

Om hello `from` egenskapsvärdet är en tom matris, hello utdata är en tom tabell.

|Namn|Krävs|Typ|Beskrivning|
|--------|------------|--------|---------------|
|Från|Ja|Matris|hello källmatrisen.|
|Format|Ja|Sträng|hello-format, antingen **CSV** eller **HTML**.|
|Kolumner|Nej|Matris|hello kolumner. Tillåter toooverride hello standard form hello tabellen.|
|kolumnrubrik|Nej|Sträng|hello rubrik för hello-kolumn.|
|värde i kolumnen|Ja|Sträng|hello värdet för hello kolumnen.|

## <a name="workflow-action"></a>Arbetsflödesåtgärd   

|Namn|Krävs|Typ|Beskrivning|  
|--------|------------|--------|---------------|  
|värd-id|Ja|Sträng|hello resurs-ID för hello arbetsflöde som du vill toocall.|  
|värden Utlösarnamn|Ja|Sträng|hello namnet hello utlösare som du vill tooinvoke.|  
|frågor|Nej|Objekt|Representerar hello frågan parametrar tooadd toohello URL. Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` toohello URL.|  
|Rubriker|Nej|Objekt|Representerar varje hello-huvuden som skickas toohello begäran. Till exempel tooset hello språk och typen på en begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Brödtext|Nej|Objekt|Representerar hello nyttolasten skickas toohello slutpunkt.|  
  
```json
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName " : "mytrigger001"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },  
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "runAfter": {}
    }
```
  
En åtkomstkontroll görs på hello arbetsflöde \(mer specifikt hello utlösaren\), vilket innebär att du behöver komma åt toohello arbetsflöde.  
  
hello matar ut från hello `workflow` åtgärd baserat på vad du har definierat i hello `response` åtgärden i hello underordnat arbetsflöde. Om du inte har definierat någon `response` åtgärd och sedan hello utdata är tomma.  

## <a name="function-action"></a>Funktionen åtgärd   

|Namn|Krävs|Typ|Beskrivning|  
|--------|------------|--------|---------------|  
|funktionen id|Ja|Sträng|hello resurs-ID för hello-funktionen som du vill tooinvoke.|  
|Metoden|Nej|Sträng|hello HTTP-metoden används tooinvoke hello-funktionen. Som standard är det `POST` om inget värde anges.|  
|frågor|Nej|Objekt|Representerar hello frågan parametrar tooadd toohello URL. Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` toohello URL.|  
|Rubriker|Nej|Objekt|Representerar varje hello-huvuden som skickas toohello begäran. Till exempel tooset hello språk och typen på en begäran: `"headers" : { "Accept-Language": "en-us" }`.|  
|Brödtext|Nej|Objekt|Representerar hello nyttolasten skickas toohello slutpunkt.|  

```json
"myfunc" : {
    "type" : "Function",
    "inputs" : {
        "function" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Web/sites/myfuncapp/functions/myfunc"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },  
        "headers" : {
            "x-ms-date" : "@utcnow()"
        },
        "method" : "POST",
    "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "runAfter": {}
}
```

När du sparar hello logikapp kan göra vi några kontroller för hello refererar till funktionen:
-   Du måste toohave åtkomst toohello funktion.
-   Endast är standard HTTP-utlösare eller allmänna JSON webhook utlösare tillåten.
-   Det får inte innehålla någon väg som har definierats.
-   Endast ”fungera” och ”anonym” åtkomstnivå är tillåten.

hello utlösaren URL hämtas, cachelagras och används vid körning. Om någon åtgärd upphäver hello cachelagras URL, så misslyckas hello åtgärden vid körning. toowork runt problemet, spara hello logikappen igen, vilket orsakar logik app tooretrieve och cachelagra hello utlösaren URL igen.

## <a name="collection-actions-scopes-and-loops"></a>Samling åtgärder (scope och slingor)

Vissa åtgärdstyper av kan innehålla åtgärder i själva. Referens-åtgärder i en samling kan refereras direkt utanför hello samling. Om du har definierat `http` i en omfattning `@body('http')` fortfarande är giltigt var som helst i ett arbetsflöde. Åtgärder i en samling kan `runAfter` endast andra åtgärder i hello samma samling.

## <a name="scope-action"></a>Scope-åtgärd

Hej `scope` åtgärd kan du logiskt gruppera åtgärder i ett arbetsflöde.

|Namn|Krävs|Typ|Beskrivning|  
|--------|------------|--------|---------------|  
|åtgärder|Ja|Objekt|Inre åtgärder tooexecute inom hello omfång|

```json
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```

## <a name="foreach-action"></a>ForEach-åtgärd

Åtgärden slingor upprepas i en matris och utför inre åtgärder för varje objekt. Som standard kör hello foreach loop parallellt (20 körningar parallellt i taget). Du kan ange körning regler med hjälp av hello `operationOptions` parameter.

|Namn|Krävs|Typ|Beskrivning|  
|--------|------------|--------|---------------|  
|åtgärder|Ja|Objekt|Inre åtgärder tooexecute inom hello loop|
|foreach|Ja|Sträng|hello matris tooiterate över|
|operationOptions|Nej|Sträng|Åtgärden alternativ för beteende. För närvarande endast stöd för `sequential` tooexecute iterationer sekventiellt (standard är parallella)|

```json
"forEach_email": {
    "type": "foreach",
    "foreach": "@body('email_filter')",
    "actions": {
        "send_email": {
            "type": "ApiConnection",
            "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
            }
        }
    },
    "runAfter":{
        "email_filter": [ "Succeeded" ]
    }
}
```

## <a name="until-action"></a>Förrän åtgärden

Åtgärden slingor kör inre åtgärder tills ett villkor resulterar tootrue.

|Namn|Krävs|Typ|Beskrivning|  
|--------|------------|--------|---------------|  
|åtgärder|Ja|Objekt|Inre åtgärder tooexecute inom hello loop|
|uttryck|Ja|Sträng|hello uttryck tooevaluate efter varje iteration|
|Gränsen|Ja|Objekt|hello gränser för hello loop - minst en gräns måste anges|
|Antal|Nej|int|hello begränsa toohello antalet upprepningar som kan utföras|
|timeout|Nej|Sträng|hello tidsgräns för hur länge det ska köras i slinga.  ISO 8601-format|


```json
 "Until_succeeded": {
    "actions": {
        "Http": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "expression": "@equals(outputs('Http')['statusCode', 200)",
    "limit": {
        "count": 1000,
        "timeout": "PT1H"
    },
    "runAfter": {},
    "type": "Until"
}
```

## <a name="conditions---if-action"></a>Villkor - om åtgärden

Hej `If` åtgärd kan du utvärdera ett villkor och köra en gren baserat på om hello uttrycket utvärderas för`true`.

|Namn|Krävs|Typ|Beskrivning|  
|--------|------------|--------|---------------|  
|åtgärder|Ja|Objekt|Inre åtgärder tooexecute när uttrycket utvärderas för`true`|
|uttryck|Ja|Sträng|hello uttryck tooevaluate|
|annan|Nej|Objekt|Inre åtgärder tooexecute när uttrycket utvärderas för`false`|
  
```json
"My_condition": {
    "actions": {
        "If_true": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "else": {
        "actions": {
            "if_false": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://myurl"
                },
                "runAfter": {},
                "type": "Http"
            }
        }
    },
    "expression": "@equals(triggerBody(), json(true))",
    "runAfter": {},
    "type": "If"
}
```  
  
hello visas följande tabell exempel på hur villkor kan använda uttryck i en åtgärd:  
  
|JSON-värde|Resultat|  
|--------------|----------|  
|`"expression": "@parameters('hasSpecialAction')"`|Ett värde som utvärderar tootrue gör det här villkoret toopass. Endast booleskt uttryck stöds. tooconvert andra typer tooBoolean, Använd funktioner `empty`, `equals`.|  
|`"expression": "@greater(actions('act1').output.value, parameters('threshold'))"`|Jämförelse funktioner stöds. Exempelvis hello här körs bara hello åtgärden när hello utdata från act1 är större än hello tröskelvärdet.|  
|`"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"`|Logik funktionerna finns även stöds toocreate kapslade booleska uttryck. I det här fallet körs hello åtgärden när hello utdata från act1 är tröskelvärdet hello eller under 100.|  
|`"expression": "@equals(length(actions('act1').outputs.errors), 0))"`|Du kan använda matrisen funktioner toocheck om en matris har några objekt. I det här fallet körs hello åtgärden när hello fel matrisen är tom.| 
|`"expression": "parameters('hasSpecialAction')"`|Fel - inte ett giltigt tillstånd eftersom @ krävs för villkor.|  
  
Om ett villkor utvärderas har, hello villkor har markerats som `Succeeded`. Åtgärder i antingen hello `actions` eller `else` objekt utvärdera för`Succeeded` när den exekveras och lyckades, `Failed` när den exekveras och misslyckades, eller `Skipped` när den grenen körs inte.

## <a name="next-steps"></a>Nästa steg

[Arbetsflödet Service REST-API](https://docs.microsoft.com/rest/api/logic/workflows)
