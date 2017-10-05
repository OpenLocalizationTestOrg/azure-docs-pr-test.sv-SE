---
title: "Arbetsflödesåtgärder och utlösare - Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: bd3f1d225b974ebde889738bb435825658d1e1e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="workflow-actions-and-triggers-for-azure-logic-apps"></a>Arbetsflödesåtgärder och utlösare för Logic Apps i Azure

Logikappar består av utlösare och åtgärder. Det finns sex olika typer av utlösare. Varje typ har olika gränssnitt och olika beteenden. Du kan också lära dig om andra uppgifter genom att titta på information om den [språk i arbetsflödesdefinitionen](logic-apps-workflow-definition-language.md).  
  
Läs vidare för att lära dig mer om utlösare och åtgärder och hur du kan använda dem för att skapa logikappar för att förbättra din affärsprocesser och arbetsflöden.  
  
### <a name="triggers"></a>Utlösare  

En utlösare Anger anrop som kan initiera en körning av arbetsflödet logik app. Här följer två olika sätt att starta en körning av arbetsflödet:  
  
-   En avsökning utlösare  

-   En push-utlösare - genom att anropa den [arbetsflöde Service REST API](https://docs.microsoft.com/rest/api/logic/workflows)  
  
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
    "splitOn" : "<property to create runs for>",
    "operationOptions": "<operation options on the trigger>"
}
```

### <a name="trigger-types-and-their-inputs"></a>Typer av utlösare och deras indata  

Du kan använda dessa typer av utlösare:
  
-   **Begära** \- gör logikappen en slutpunkt att anropa  
  
-   **Återkommande** \- utlöses baserat på ett definierat schema  
  
-   **HTTP** \- avsöker en HTTP-slutpunkt för webbprogram. HTTP-slutpunkten måste motsvara en specifik utlösande kontraktet \- antingen med hjälp av en 202\-asynk.mönster, eller genom att returnera en matris  
  
-   **ApiConnection** \- Utlös avsökningar t.ex. HTTP, men den kan du utnyttja den [Microsoft-hanterade API: er](https://docs.microsoft.com/azure/connectors/apis-list)  
  
-   **HTTPWebhook** \- öppnas en slutpunkt som liknar den manuella utlösaren, men också anropar till en angiven URL för att registrera och avregistrera  
  
-   **ApiConnectionWebhook** \- Operates som HTTPWebhook utlösaren genom att dra nytta av de Microsoft-hanterade API: er       
    Varje typ av utlösare som har en annan uppsättning **indata** som definierar sitt beteende.  
  
## <a name="request-trigger"></a>Begäran utlösare  

Den här utlösaren fungerar som en slutpunkt som anropas via en HTTP-begäran att anropa logikappen. Det ser ut som i det här exemplet en utlösare för begäran:  
  
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
|Schemat|Nej|En JSON-schema som kontrollerar den inkommande begäranden. Användbart för att hjälpa efterföljande arbetsflödessteg veta vilka egenskaper som ska referera till.|

Om du vill anropa den här slutpunkten måste du anropa den *listCallbackUrl* API. Se [arbetsflöde Service REST API](https://docs.microsoft.com/rest/api/logic/workflows).  
  
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

Som du ser är ett enkelt sätt att köra ett arbetsflöde.  
  
|Elementnamn|Krävs|Beskrivning|  
|----------------|------------|---------------|  
|frekvens|Ja|Hur ofta utlösaren körs. Använd endast ett av dessa värden: sekund, minut, timma, dag, vecka, månad eller år|  
|intervall|Ja|Intervallet för den angivna frekvensen för återkommande|  
|startTime|Nej|Om en startTime anges utan en UTC-förskjutningen, används den här tidszonen.|  
|Tidszon|Nej|Om en startTime anges utan en UTC-förskjutningen, används den här tidszonen.|  
  
Du kan även schemalägga en utlösare för att starta körning vid en viss tidpunkt i framtiden. Om du vill starta en vecka rapport varje måndag kan du schemalägga logikappen att starta varje måndag genom att skapa följande utlösaren:  

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

HTTP-utlösare avsöker den angivna slutpunkten och kontrollera svaret för att avgöra om arbetsflödet ska köras. Objektet indata tar uppsättningen parametrar som krävs för att konstruera ett HTTP-anrop:  
  
|Elementnamn|Krävs|Beskrivning|Typ|  
|----------------|------------|---------------|--------|  
|Metoden|Ja|Kan vara något av följande metoder för HTTP: GET, POST, PUT, DELETE, KORRIGERINGSFIL eller HEAD|Sträng|  
|URI: N|Ja|Http eller https slutpunkten som anropas. Högst 2 kilobyte.|Sträng|  
|frågor|Nej|Ett objekt som representerar frågeparametrar att lägga till URL: en. Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` till URL: en.|Objekt|  
|Rubriker|Nej|Ett objekt som representerar alla huvuden som skickas till begäran. Till exempel ange språk och Skriv begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|Objekt|  
|Brödtext|Nej|Ett objekt som representerar nyttolasten som skickas till slutpunkten.|Objekt|  
|retryPolicy|Nej|Ett objekt som kan du anpassa försök beteendet för 4xx eller 5xx-fel.|Objekt|  
|Autentisering|Nej|Representerar den metod som ska autentisera begäran. Mer information om det här objektet finns [Scheduler utgående autentisering](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication). Utöver scheduler, är en mer stöds egenskap: `authority` det här värdet är som standard `https://login.windows.net` om anges, men du kan använda en annan publik som`https://login.windows\-ppe.net`|Objekt|  
  
HTTP-utlösaren kräver HTTP-API för att överensstämma med ett specifikt mönster för att fungera med din logikapp. Det krävs följande fält:  
  
|Svar|Beskrivning|  
|------------|---------------|  
|Statuskod|Statuskod 200 \(OK\) framkalla en körning. Andra statuskod göra inte en körning.|  
|Försök\-efter huvudet|Antalet sekunder tills logikappen avsöker slutpunkten igen.|  
|Platshuvud|URL: en att anropa nästa avsökningsintervall. Om inget anges används den ursprungliga URL: en.|  
  
Här följer några exempel på olika beteenden för olika typer av begäranden:  
  
|Svarskod|Försök\-efter|Beteende|  
|-----------------|----------------|------------|  
|200|\(Ingen\)|Inte en giltig utlösare, försök igen\-efter krävs, eller annan motorn aldrig avsöker för nästa begäran.|  
|202|60|Inte Utlös arbetsflödet. Nästa försök sker i en minut.|  
|200|10|Köra arbetsflödet och Sök igen efter mer innehåll om 10 sekunder.|  
|400|\(Ingen\)|Felaktig begäran inte köra arbetsflödet. Om det finns inga **försök princip** definierats standardprincipen används. Utlösaren är inte längre giltig när antalet försök har nåtts.|  
|500|\(Ingen\)|Arbetsflödet körs inte på Server-fel.  Om det finns inga **försök princip** definierats standardprincipen används. Utlösaren är inte längre giltig när antalet försök har nåtts.|  
  
Det ser ut som i det här exemplet utdata för en HTTP-utlösare:  
  
|Elementnamn|Beskrivning|Typ|  
|----------------|---------------|--------|  
|Rubriker|Sidhuvuden för http-svaret.|Objekt|  
|Brödtext|Brödtexten i http-svaret.|Objekt|  
  
## <a name="api-connection-trigger"></a>Utlösare för API-anslutningen  

Utlösare för API-anslutningen är samma som HTTP-utlösaren i dess grundläggande funktioner. Parametrar för att identifiera åtgärden är dock olika. Här är ett exempel:  
  
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
|värden|Ja||ApiApp finns gateway och ID: t.|  
|Metoden|Ja|Sträng|Kan vara något av följande metoder för HTTP: **hämta**, **POST**, **PLACERA**, **ta bort**, **korrigering**, eller **HEAD**|  
|frågor|Nej|Objekt|Representerar frågeparametrar som ska läggas till URL: en. Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` till URL: en.|  
|Rubriker|Nej|Objekt|Representerar var och en av huvuden som skickas till begäran. Till exempel ange språk och Skriv begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Brödtext|Nej|Objekt|Representerar nyttolasten som skickas till slutpunkten.|  
|retryPolicy|Nej|Objekt|Kan du anpassa försök beteendet för 4xx eller 5xx-fel.|  
|Autentisering|Nej|Objekt|Representerar den metod som ska autentisera begäran. Mer information om det här objektet finns [Scheduler utgående autentisering](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)|  
  
Egenskaper för värden är:  
  
|Elementnamn|Krävs|Beskrivning|  
|----------------|------------|---------------|  
|API-runtimeUrl|Ja|Hanterade API-slutpunkt.|  
|Anslutningens namn||Måste vara en referens till en parameter med namnet `$connection` och är namnet på den hanterade API-anslutningen som används i arbetsflödet.|
  
Utdata för en utlösare för API-anslutning är:
  
|Elementnamn|Typ|Beskrivning|  
|----------------|--------|---------------|  
|Rubriker|Objekt|Sidhuvuden för http-svaret.|  
|Brödtext|Objekt|Brödtexten i http-svaret.|  
  
## <a name="httpwebhook-trigger"></a>HTTPWebhook utlösare  

Utlösaren HTTPWebhook öppnar en slutpunkt som liknar den manuella utlösaren men HTTPWebhook utlösaren anropar också till en angiven URL för att registrera och avregistrera. Här är ett exempel på hur en HTTPWebhook-utlösare kan se ut:  

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

Många av dessa avsnitt är valfria och Webhooken beror på vilka avsnitt som eller utelämnas.  
Egenskaperna för en Webhook är följande:  
  
|Elementnamn|Krävs|Beskrivning|  
|----------------|------------|---------------|  
|prenumerera på|Nej|Utgående begäran som anropas när utlösaren skapas och utför registreringen.|  
|avbryta prenumerationen|Nej|Utgående begäran när utlösaren tas bort.|  
  
-   **Prenumerera på** är utgående anrop som har gjorts att starta lyssnar på händelser. Det här anropet börjar med samma uppsättning parametrar som gör de vanliga HTTP-åtgärderna. Utgående anropet görs någon gång arbetsflödet ändras på något sätt, till exempel när autentiseringsuppgifterna samlas eller ändra utlösarens indataparametrar.
  
    För att stödja det här anropet är en ny funktion: `@listCallbackUrl()`. Den här funktionen returnerar en unik URL för den här specifika utlösaren i det här arbetsflödet. Den motsvarar den unika identifieraren för slutpunkter som använder Service REST.  
  
-   **Avbryta prenumerationen** anropas när en åtgärd återgivningar utlösaren är ogiltig, inklusive:  
  
    -   Ta bort eller inaktivera utlösaren  
  
    -   Ta bort eller inaktivera arbetsflödet  
  
    -   Ta bort eller inaktivera prenumerationen  
  
    Åtgärden Avbryt prenumeration anropar automatiskt i logikappen. Parametrarna för den här funktionen är samma som HTTP-utlösaren.  
  
    Utdata för HTTPWebhook utlösaren är innehållet i den inkommande begäranden:  
  
|Elementnamn|Typ|Beskrivning|  
|-----------------|--------|---------------|  
|Rubriker|Objekt|Sidhuvuden för http-begäran.|  
|Brödtext|Objekt|Brödtexten i http-begäran.|  

Gränserna för ett webhook-åtgärder kan anges på samma sätt som [HTTP asynkron gränser](#asynchronous-limits).
  

## <a name="conditions"></a>Villkor  

Du kan använda ett eller flera villkor för alla utlösare för att avgöra om arbetsflödet ska köras eller inte. Exempel:  

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

I det här fallet rapporten endast utlösare när arbetsflödets `sendReports` parameter är angiven till true. Slutligen får villkor referera statuskod för utlösaren. Exempelvis kan startar du ett arbetsflöde endast när din webbplats returnerar en statuskod 500, enligt följande:
  
```  
"conditions": [  
        {  
          "expression": "@equals(triggers().code, 'InternalServerError')"  
        }  
      ]  
```  
  
> [!NOTE]  
> När ett uttryck som refererar till statuskod för utlösaren \(på något sätt\), standardbeteendet \(utlösaren endast på 200 \(OK\) \) ersätts. Till exempel om du vill aktivera både statuskod 200 och statuskod 201 du behöver ta: `@or(equals(triggers().code, 200),equals(triggers().code,201))` som dina villkor.  
  
## <a name="start-multiple-runs-for-a-request"></a>Starta flera körs för en begäran

Att startar flera körningar för en enskild begäran `splitOn` är användbart, till exempel när du vill söka en slutpunkt som kan ha flera nya objekt mellan avsökningsintervall.
  
Med `splitOn`, du anger egenskapen i nyttolasten av svar som innehåller ett antal objekt som du vill använda för att starta en körning av utlösaren. Anta att du har en API som returnerar följande svar:  
  
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
  
Din logikapp kan du bara behöver rader innehåll, så du kan skapa en utlösare som det här exemplet:  
  
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
  
I arbetsflödesdefinitionen, `@triggerBody().name` returnerar `mycoolrow` för den första körningen och `another row` för andra kör. Utlösaren utdata ser ut det här exemplet:  
  
```json
{
    "body" : {
        "id" : 938109381,
        "name" : "another row"
    }
}
```

Om du använder `SplitOn`, du kan inte hämta de egenskaper som är utanför matrisen, i det här fallet den `Status` fältet.  
  
> [!NOTE]  
> I det här exemplet använder vi den `?` operatorn för att undvika ett fel om den `Rows` egenskapen finns inte. 
  
## <a name="single-run-instance"></a>Kör instans

Du kan konfigurera utlösare som har en upprepning egenskap eller bara om alla aktiva körningar har slutförts. Om en schemalagd upprepning inträffar när det finns en pågående kör utlösaren hoppar över och väntar tills nästa schemalagda Upprepningsintervall söka igen.

Du kan konfigurera den här inställningen i alternativen för åtgärden:

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
  
-   **ApiConnection** \- åtgärden fungerar som HTTP-åtgärden, men använder Microsoft-hanterade API: erna.  
  
-   **ApiConnectionWebhook** \- som HTTPWebhook men använder Microsoft-hanterade API: erna.  
  
-   **Svaret** \- åtgärden definierar ett svar för ett inkommande samtal.  
  
-   **Vänta** \- åtgärden enkel väntar på ett fast belopp tid eller tills en viss tid.  
  
-   **Arbetsflödet** \- representerar ett inkapslat arbetsflöde för den här åtgärden.  

-   **Funktionen** \- åtgärden representerar en Azure-funktion.

### <a name="collection-actions"></a>Åtgärder för samlingen

-   **Scope** \- den här åtgärden är en logisk gruppering av andra åtgärder.

-   **Villkoret** \- åtgärden utvärderar ett uttryck och kör grenen motsvarande resultat.

-   **ForEach** \- slingor åtgärden upprepas i en matris och utför inre åtgärder för varje objekt.

-   **Tills** \- åtgärden slingor kör inre åtgärder tills ett villkor resultatet till true.
  
Varje typ av åtgärd har en annan uppsättning **indata** som definierar beteendet för en åtgärd.  
  
## <a name="http-action"></a>HTTP-åtgärd  

HTTP-åtgärder anropa den angivna slutpunkten och kontrollera svaret för att avgöra om arbetsflödet ska köras. Den **indata** objektet tar uppsättningen parametrar som krävs för att konstruera HTTP-anropet:  
  
|Elementnamn|Krävs|Typ|Beskrivning|  
|----------------|------------|--------|---------------|  
|Metoden|Ja|Sträng|Kan vara något av följande metoder för HTTP: **hämta**, **POST**, **PLACERA**, **ta bort**, **korrigering**, eller **HEAD**|  
|URI: N|Ja|Sträng|Http eller https slutpunkten som anropas. Maximal längd är 2 kB.|  
|frågor|Nej|Objekt|Representerar frågeparametrar att lägga till URL: en. Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` till URL: en.|  
|Rubriker|Nej|Objekt|Representerar var och en av huvuden som skickas till begäran. Till exempel ange språk och Skriv begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Brödtext|Nej|Objekt|Representerar nyttolasten som skickas till slutpunkten.|  
|retryPolicy|Nej|Objekt|Kan du anpassa försök beteendet för 4xx eller 5xx-fel.|  
|operationsOptions|Nej|Sträng|Definierar de särskilda beteenden att åsidosätta.|  
|Autentisering|Nej|Objekt|Representerar den metod som ska autentisera begäran. Mer information om det här objektet finns [Scheduler utgående autentisering](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication). Utöver scheduler, är en mer stöds egenskap: `authority`. Detta är som standard `https://login.windows.net` om anges, men du kan använda en annan publik som`https://login.windows\-ppe.net`|  
  
HTTP-åtgärder \(och API-anslutningen\) åtgärder stöd försök principer. En återförsöksprincip som gäller för återkommande fel som betecknas som HTTP-statuskoder 408 och 429 5xx utöver eventuella undantag. Den här principen nedan används den *retryPolicy* objekt som är definierat som visas här:
  
```json
"retryPolicy" : {
    "type": "<type-of-retry-policy>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```
  
Intervallet anges i ISO 8601-format. Det här intervallet har standard och minst 20 sekunder medan det maximala värdet är en timme. Standard- och maximalt antal för nya försök är fyra timmar. Om principdefinitionen försök anges en `fixed` strategi som används med försök antal och intervall standardvärden. Om du vill inaktivera återförsöksprincipen sägs dess typ `None`.  
  
Till exempel i följande nya försök hämta de senaste nyheterna två gånger om återkommande fel totalt tre körningar med en fördröjning på 30 sekunder mellan varje försök:  
  
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

Som standard stöder alla HTTP-baserade åtgärder mönstret standard asynkron åtgärd. Om fjärrservern anger att begäran är godkänd för behandling med en 202 \(godkända\) svar, Logic Apps motorn håller avsöker den URL som anges i svarshuvud plats tills ett avslutat tillstånd \(inte\-202 svar\).  
  
Om du vill inaktivera beteendet asynkron som beskrevs tidigare, ange en `DisableAsyncPattern` alternativ på åtgärd-indata. I det här fallet är resultatet av åtgärden baserad på det första 202 svaret från servern.  
  
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

Du kan begränsa ett asynkront mönster varaktigheten för ett visst tidsintervall.  Om tidsintervallet långa utan att ett avslutat tillstånd åtgärdens status kommer att markeras `Cancelled` med koden `ActionTimedOut`.  Begränsa tidsgränsen har angetts i ISO 8601-format.  Gränser kan anges med följande syntax:

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
Den här åtgärden kräver en referens till en giltig anslutning och information om API och parametrar som krävs.

|Elementnamn|Krävs|Typ|Beskrivning|  
|----------------|------------|--------|---------------|  
|värden|Ja|Objekt|Representerar connector-information, till exempel runtimeUrl och referens till connection-objektet|
|Metoden|Ja|Sträng|Kan vara något av följande metoder för HTTP: **hämta**, **POST**, **PLACERA**, **ta bort**, **korrigering**, eller **HEAD**|  
|Sökväg|Ja|Sträng|Sökvägen till API-åtgärd.|  
|frågor|Nej|Objekt|Representerar frågeparametrar att lägga till URL: en. Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` till URL: en.|  
|Rubriker|Nej|Objekt|Representerar var och en av huvuden som skickas till begäran. Till exempel ange språk och Skriv begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Brödtext|Nej|Objekt|Representerar nyttolasten som skickas till slutpunkten.|  
|retryPolicy|Nej|Objekt|Kan du anpassa försök beteendet för 4xx eller 5xx-fel.|  
|operationsOptions|Nej|Sträng|Definierar de särskilda beteenden att åsidosätta.|  

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

Gränserna för ett webhook-åtgärder kan anges på samma sätt som [HTTP asynkron gränser](#asynchronous-limits).
  
## <a name="response-action"></a>Svaret åtgärd  

Den här typen innehåller hela svaret nyttolast från en HTTP-begäran och ett statusCode, brödtext och rubriker:  
  
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
  
Instruktionen svar har särskilda begränsningar som inte gäller för andra åtgärder. Närmare bestämt:  
  
-   Responsåtgärder får inte vara parallellt i en definition av eftersom en deterministisk svar på inkommande begäran krävs.  
  
-   Om en åtgärd för svar uppnås när den inkommande begäranden tog emot ett svar, åtgärden betraktas som misslyckad \(konflikt\), och därför inte `Failed`.  
  
-   Ett arbetsflöde med responsåtgärder kan inte ha `splitOn` i dess utlösare eftersom ett anrop gör många körs. Det bör därför verifieras när flödet är PUT och orsaken en felaktig begäran.  
  
## <a name="wait-action"></a>Vänta åtgärd  

Den `wait` åtgärd pausar körningen av arbetsflödet för det angivna intervallet. Du kan till exempel använda den här fragment för att vänta 15 minuter:  
  
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
  
Om du vill vänta tills en särskild tidpunkt, kan du också använda det här exemplet:  
  
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
> Vänta varaktighet kan anges antingen med hjälp av den **intervall** objekt eller **tills** objekt, men inte båda.  
  
|Namn|Krävs|Typ|Beskrivning|  
|--------|------------|--------|---------------|  
|intervall|Nej|Objekt|Vänta varaktighet baserat på tid.|  
|intervall|Ja|Sträng|En av dessa intervall: sekund, minut, timma, dag, vecka, månad, år.|  
|intervall för antal|Ja|Sträng|Varaktighet baserat på den angivna interna enheten.|  
|fram till|Nej|Objekt|Vänta varaktighet baserat på en punkt i tiden.|  
|tills tidsstämpel|Ja|Sträng|Sträng &#124; Punkten i tiden i UTC när väntetiden upphör att gälla.|  

## <a name="query-action"></a>Frågeåtgärden

Den `query` åtgärd kan du filtrera en matris baserat på ett villkor. Till exempel för att välja nummer som är större än 2, kan du använda:

```json
"FilterNumbers" : {
    "type": "query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

Utdata från den `query` åtgärden är en matris som har element från Indatamatrisen som uppfyller villkoret.

> [!NOTE]
> Om inga värden uppfyller de `where` villkoret, resultatet är en tom matris.

|Namn|Krävs|Typ|Beskrivning|
|--------|------------|--------|---------------|
|Från|Ja|matris|Källmatrisen.|
|där|Ja|Sträng|Villkoret som gäller för varje element i källmatrisen.|

## <a name="select-action"></a>Välj åtgärd

Den `select` du kan projicera varje element i en matris till ett nytt värde.
Till exempel om du vill konvertera en matris av talen i en array med objekt som kan du använda:

```json
"SelectNumbers" : {
    "type": "select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

Utdata från den `select` åtgärden är en matris som har samma kardinalitet som Indatamatrisen, där varje element omvandlas som definieras av den `select` egenskapen. Om indata är en tom matris, är utdata också en tom matris.

|Namn|Krävs|Typ|Beskrivning|
|--------|------------|--------|---------------|
|Från|Ja|matris|Källmatrisen.|
|Välj|Ja|Alla|Projektionen som ska gälla för varje element i källmatrisen.|

## <a name="terminate-action"></a>Åtgärden Avbryt

Åtgärden Avsluta avbryts körningen av arbetsflödet kör, avbryts alla pågående åtgärder och hoppa över eventuella återstående åtgärder. Till exempel för att avsluta en körning med statusen **misslyckades**, kan du använda följande utdrag:

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
> Åtgärder som redan har slutförts påverkas inte av åtgärden Avsluta.

|Namn|Krävs|Typ|Beskrivning|
|--------|------------|--------|---------------|
|runStatus|Ja|Sträng|Målet kör status. Antingen **misslyckades** eller **avbröts**.|
|runError|Nej|Objekt|Felinformation. Endast när **runStatus** är inställd på **misslyckades**.|
|runError kod|Nej|Sträng|Kör felkoden.|
|runError meddelande|Nej|Sträng|Kör ett felmeddelande.|

## <a name="compose-action"></a>Skriv åtgärd

Skriv åtgärd kan du skapa ett godtyckligt objekt. Utdata från åtgärden Skriv är ett resultat av utvärderingen av indata. Du kan till exempel använda Skriv åtgärd för att sammanfoga utdata för flera åtgärder:

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
> Den **Compose** åtgärd kan användas för att skapa några utdata, inklusive objekt, matriser och andra typer som stöds av logikappar som XML och binary.

## <a name="table-action"></a>Åtgärden för tabellen

Den `table` kan du konvertera en matris med-objekt till en **CSV** eller **HTML** tabell.

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

Och låta den åtgärd som definierats som

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html"
    }
}
```

Ovanstående skulle skapa

<table><thead><tr><th>id</th><th>namn</th></tr></thead><tbody><tr><td>0</td><td>Äpplen</td></tr><tr><td>1</td><td>orange</td></tr></tbody></table>"

För att anpassa tabellen kan du ange vilka kolumner explicit. Exempel:

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

Ovanstående skulle skapa

<table><thead><tr><th>Skapa id</th><th>Beskrivning</th></tr></thead><tbody><tr><td>0</td><td>ny äpplen</td></tr><tr><td>1</td><td>ny orange</td></tr></tbody></table>"

Om den `from` egenskapsvärdet är en tom matris, utdata är en tom tabell.

|Namn|Krävs|Typ|Beskrivning|
|--------|------------|--------|---------------|
|Från|Ja|matris|Källmatrisen.|
|Format|Ja|Sträng|Format, antingen **CSV** eller **HTML**.|
|Kolumner|Nej|matris|Kolumner. Tillåter för att åsidosätta standard formen i tabellen.|
|kolumnrubrik|Nej|Sträng|Rubriken för kolumnen.|
|värde i kolumnen|Ja|Sträng|Värdet för kolumnen.|

## <a name="workflow-action"></a>Arbetsflödesåtgärd   

|Namn|Krävs|Typ|Beskrivning|  
|--------|------------|--------|---------------|  
|värd-id|Ja|Sträng|Resurs-ID för det arbetsflöde som du vill anropa.|  
|värden Utlösarnamn|Ja|Sträng|Namnet på utlösaren som du vill anropa.|  
|frågor|Nej|Objekt|Representerar frågeparametrar att lägga till URL: en. Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` till URL: en.|  
|Rubriker|Nej|Objekt|Representerar var och en av huvuden som skickas till begäran. Till exempel ange språk och Skriv begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Brödtext|Nej|Objekt|Representerar nyttolasten skickas till slutpunkten.|  
  
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
  
En åtkomstkontroll görs på arbetsflödet \(mer specifikt utlösaren\), vilket innebär att du behöver åtkomst till arbetsflödet.  
  
Utdata från den `workflow` åtgärd baserat på vad du har definierat i på `response` åtgärd i arbetsflöden. Om du inte har definierat någon `response` åtgärd och sedan utdata är tomma.  

## <a name="function-action"></a>Funktionen åtgärd   

|Namn|Krävs|Typ|Beskrivning|  
|--------|------------|--------|---------------|  
|funktionen id|Ja|Sträng|Resurs-ID för den funktion som du vill anropa.|  
|Metoden|Nej|Sträng|HTTP-metoden som används för att anropa funktionen. Som standard är det `POST` om inget värde anges.|  
|frågor|Nej|Objekt|Representerar frågeparametrar att lägga till URL: en. Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` till URL: en.|  
|Rubriker|Nej|Objekt|Representerar var och en av huvuden som skickas till begäran. Till exempel ange språk och typ för en begäran: `"headers" : { "Accept-Language": "en-us" }`.|  
|Brödtext|Nej|Objekt|Representerar nyttolasten skickas till slutpunkten.|  

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

När du sparar logikappen kan göra vi några kontroller på den angivna funktionen:
-   Du måste ha tillgång till funktionen.
-   Endast är standard HTTP-utlösare eller allmänna JSON webhook utlösare tillåten.
-   Det får inte innehålla någon väg som har definierats.
-   Endast ”fungera” och ”anonym” åtkomstnivå är tillåten.

Utlösaren URL: en hämtas, cachelagras och används vid körning. Så om någon åtgärd upphäver den cachelagra URL, misslyckas åtgärden under körning. Undvik detta genom att spara logikappen igen, vilket leder till logikapp du hämtar och cachelagrar utlösaren URL: en igen.

## <a name="collection-actions-scopes-and-loops"></a>Samling åtgärder (scope och slingor)

Vissa åtgärdstyper av kan innehålla åtgärder i själva. Referens-åtgärder i en samling kan refereras direkt utanför samlingen. Om du har definierat `http` i en omfattning `@body('http')` fortfarande är giltigt var som helst i ett arbetsflöde. Åtgärder i en samling kan `runAfter` endast andra åtgärder i samma samling.

## <a name="scope-action"></a>Scope-åtgärd

Den `scope` åtgärd kan du logiskt gruppera åtgärder i ett arbetsflöde.

|Namn|Krävs|Typ|Beskrivning|  
|--------|------------|--------|---------------|  
|åtgärder|Ja|Objekt|Inre åtgärder för att köra inom omfånget|

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

Åtgärden slingor upprepas i en matris och utför inre åtgärder för varje objekt. Som standard kör foreach-loop parallellt (20 körningar parallellt i taget). Du kan ange regler för körning med hjälp av den `operationOptions` parameter.

|Namn|Krävs|Typ|Beskrivning|  
|--------|------------|--------|---------------|  
|åtgärder|Ja|Objekt|Inre åtgärder för att köra inom loopen|
|foreach|Ja|Sträng|Matrisen och iterera över|
|operationOptions|Nej|Sträng|Åtgärden alternativ för beteende. Stöder för närvarande endast `sequential` att utföra iterationer sekventiellt (standard är parallella)|

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

Åtgärden slingor kör inre åtgärder tills ett villkor resultatet till true.

|Namn|Krävs|Typ|Beskrivning|  
|--------|------------|--------|---------------|  
|åtgärder|Ja|Objekt|Inre åtgärder för att köra inom loopen|
|uttryck|Ja|Sträng|Uttrycket som ska utvärderas efter varje iteration|
|Gränsen|Ja|Objekt|Gränser för loop - minst en gräns måste anges|
|Antal|Nej|int|Gränsen för antalet upprepningar som kan utföras|
|Timeout|Nej|Sträng|Tidsgräns för hur länge det ska köras i slinga.  ISO 8601-format|


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

Den `If` åtgärd kan du utvärdera ett villkor och köra en gren baserat på om uttrycket utvärderas till `true`.

|Namn|Krävs|Typ|Beskrivning|  
|--------|------------|--------|---------------|  
|åtgärder|Ja|Objekt|Inre åtgärder för att köras när uttrycket utvärderas till`true`|
|uttryck|Ja|Sträng|Uttrycket som ska utvärderas|
|annan|Nej|Objekt|Inre åtgärder för att köras när uttrycket utvärderas till`false`|
  
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
  
I följande tabell visas exempel på hur villkor kan använda uttryck i en åtgärd:  
  
|JSON-värde|Resultat|  
|--------------|----------|  
|`"expression": "@parameters('hasSpecialAction')"`|Ett värde som kan utvärderas till SANT gör detta villkor att skicka. Endast booleskt uttryck stöds. Konvertera andra typer till Boolean med funktioner `empty`, `equals`.|  
|`"expression": "@greater(actions('act1').output.value, parameters('threshold'))"`|Jämförelse funktioner stöds. Exempelvis den här körs åtgärden endast när utdata från act1 är större än tröskelvärdet.|  
|`"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"`|Logik funktioner stöds även för att skapa kapslade booleska uttryck. I det här fallet körs åtgärden när resultatet av act1 är över tröskelvärdet eller under 100.|  
|`"expression": "@equals(length(actions('act1').outputs.errors), 0))"`|Du kan använda matrisfunktioner för att kontrollera om en matris har några objekt. I det här fallet körs åtgärden när fel matrisen är tom.| 
|`"expression": "parameters('hasSpecialAction')"`|Fel - inte ett giltigt tillstånd eftersom @ krävs för villkor.|  
  
Om ett villkor utvärderas har, villkor har markerats som `Succeeded`. Åtgärder i antingen den `actions` eller `else` objekt utvärderas till `Succeeded` när den exekveras och lyckades, `Failed` när den exekveras och misslyckades, eller `Skipped` när den grenen körs inte.

## <a name="next-steps"></a>Nästa steg

[Arbetsflödet Service REST-API](https://docs.microsoft.com/rest/api/logic/workflows)
