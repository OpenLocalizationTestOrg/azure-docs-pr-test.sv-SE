---
title: aaaError & undantagshantering - Azure Logic Apps | Microsoft Docs
description: "Mönster för fel- och undantagshantering i Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 326a252310c8dfb154e583f91c9421675e448d1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a>Hantera fel och undantag i Azure Logic Apps

Med Azure Logikappar innehåller omfattande verktyg och mönster toohelp du kontrollera att din integreringar är stabilt och motståndskraftiga mot fel. Alla integration arkitektur utgör hello utmaningen i att göra att tooappropriately referensen driftstopp eller utfärdar från beroende system. Logic Apps enkelt att hantera fel hello en förstklassig miljö, vilket ger dig verktyg du behöver tooact på undantag och fel i dina arbetsflöden.

## <a name="retry-policies"></a>Försök principer

En återförsöksprincip är hello mest grundläggande typ av undantag och felhantering. Om en inledande begäran misslyckades eller orsakade timeout (alla förfrågningar som resulterar i en 429 eller 5xx-svar), den här principen definierar om hello åtgärden bör försöka. Som standard gör alla åtgärder 4 ytterligare gånger över 20 sekunders intervall. Om hello första begäran tar emot en `500 Internal Server Error` svar, hello arbetsflödesmotorn pausar i 20 sekunder och försök hello begäran igen. Om efter alla försök hello svar är fortfarande undantag eller fel, fortsätter hello arbetsflödet och markerar hello Åtgärdsstatus som `Failed`.

Du kan konfigurera principer för försök i hello **indata** för en viss åtgärd. Du kan till exempel konfigurera en försök princip tootry upp till 4 gånger över 1 timme intervall. Fullständig information om indataparametrar finns [arbetsflödesåtgärder och utlösare][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Om du vill att din HTTP-åtgärd tooretry 4 gånger och vänta i 10 minuter mellan varje försök använder hello definitionen:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

Mer information om syntax som stöds finns i hello [återförsöksprincip avsnittet i arbetsflödesåtgärder och utlösare][retryPolicyMSDN].

## <a name="catch-failures-with-hello-runafter-property"></a>Catch-fel med hello RunAfter egenskapen

Varje logik app åtgärd anger vilka åtgärder som måste slutföras innan hello åtgärden startar, t.ex. sortera hello steg i arbetsflödet. Hello åtgärdsdefinition den här ordningen är känt som hello `runAfter` egenskapen. Den här egenskapen är ett objekt som beskriver vilka åtgärder och status för åtgärden köra hello-åtgärd. Alla åtgärder som har lagts till via hello logik App Designer är som standard för`runAfter` hello föregående steg om hello föregående steg `Succeeded`. Du kan dock anpassa det här värdet toofire åtgärder när tidigare åtgärder har `Failed`, `Skipped`, eller en möjlig uppsättning med dessa värden. Om du vill tooadd ett objekt tooa avses Service Bus-ämne efter en viss åtgärd `Insert_Row` misslyckas, kan du använda följande hello `runAfter` konfiguration:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Meddelande hello `runAfter` egenskapen toofire om hello `Insert_Row` åtgärden är `Failed`. toorun hello åtgärd om hello Åtgärdsstatus är `Succeeded`, `Failed`, eller `Skipped`, använder du följande syntax:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Åtgärder som körs och slutföras efter en föregående åtgärd har misslyckats, markeras som `Succeeded`. Den här funktionen innebär att om du har fånga alla fel i ett arbetsflöde, hello köras automatiskt har markerats som `Succeeded`.

## <a name="scopes-and-results-tooevaluate-actions"></a>Omfång och resultat tooevaluate åtgärder

Liknande toohow som du kan köra efter enskilda åtgärder du kan också gruppera åtgärder i en [omfång](../logic-apps/logic-apps-loops-and-scopes.md), som fungerar som en logisk gruppering av åtgärder. Scope är användbara både för att organisera dina logic app åtgärder och för att utföra sammanställd utvärderingar på hello status för ett omfång. hello scope själva får status när alla åtgärder i ett scope är klar. hello scope status bestäms med hello samma kriterier som körs. Om hello sista åtgärd i en körning grenen är `Failed` eller `Aborted`, hello status är `Failed`.

toofire specifika åtgärder efter fel som inträffade i hello omfång som du kan använda `runAfter` med en omfattning som är markerad `Failed`. Om *alla* åtgärder i hello omfång misslyckas, kör ett scope misslyckas kan du skapa en enda åtgärd toocatch fel.

### <a name="getting-hello-context-of-failures-with-results"></a>Få hello kontext fel med resultat

Även om det är användbart fånga fel från ett scope, kan du också kontexten toohelp du förstå exakt vilka åtgärder som misslyckades, och eventuella fel eller statuskoder som returnerades. Hej `@result()` arbetsflödesfunktion ger kontext om hello resultatet av alla åtgärder i en omfattning.

`@result()`tar en enda parameter, scope-namn och returnerar en matris med alla hello åtgärd resultat från i omfattningen. Dessa åtgärder objekt innefattar hello samma attribut som hello `@actions()` objektet, inklusive åtgärd starttid, sluttid för åtgärd, Åtgärdsstatus, åtgärden indata, åtgärden Korrelations-ID: N och åtgärden matar ut. toosend kontexten för alla åtgärder som misslyckades i ett omfång, du lätt kan koppla en `@result()` fungerar med en `runAfter`.

tooexecute åtgärden *för varje* åtgärd i en omfattning som `Failed`filter hello matris med resultat tooactions som har misslyckats, kan du koppla `@result()` med en  **[Filter matris](../connectors/connectors-native-query.md)**  åtgärd och en  **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)**  loop. Du kan ta hello filtrerade resultat matris och utföra en åtgärd för varje fel med hello **ForEach** loop. Här är ett exempel, följt av en detaljerad förklaring som skickar en HTTP POST-begäran med hello svarstexten för alla åtgärder som inte omfattas hello `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Här följer en detaljerad genomgång toodescribe händer:

1. tooget hello resultatet av alla åtgärder inom `My_Scope`, hello **Filter matris** åtgärdsfilter `@result('My_Scope')`.

2. Hej villkor för **Filter matris** valfri `@result()` objekt som har status som är lika för`Failed`. Det här villkoret filtrerar hello matris med alla åtgärd resultat från `My_Scope` tooan matris med endast misslyckades åtgärden resultat.

3. Utföra en **för varje** åtgärd på hello **filtrerade matris** matar ut. Det här steget utför en åtgärd *för varje* misslyckades åtgärden resultat som tidigare har filtrerats.

    Om en enda åtgärd i hello omfång misslyckades hello åtgärder i hello `foreach` bara körs en gång. 
    Många misslyckade åtgärder gör att en åtgärd per fel.

4. Skicka en HTTP POST på hello `foreach` objektet brödtext för svar eller `@item()['outputs']['body']`. Hej `@result()` form är hello samma som hello `@actions()` form och kan parsas hello samma sätt.

5. Innehåller två anpassade huvuden med namn på hello misslyckad åtgärd `@item()['name']` och hello misslyckades kör klienten spårnings-ID `@item()['clientTrackingId']`.

Här är ett exempel på en enda referens `@result()` artikeln, visar hello `name`, `body`, och `clientTrackingId` egenskaper som parsas i hello föregående exempel. Utanför en `foreach`, `@result()` returnerar en matris med de här objekten.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/folder-name/resource-name does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

Du kan använda hello uttryck såg tidigare tooperform olika undantagshantering mönster. Du kan välja tooexecute en enda undantagshantering åtgärd utanför hello omfattning som accepterar hello hela filtrerade array fel och ta bort hello `foreach`. Du kan även inkludera andra användbara egenskaper från hello `@result()` svar som har visats.

## <a name="azure-diagnostics-and-telemetry"></a>Azure-diagnostik och telemetri

hello tidigare mönstren är bra sätt toohandle fel och undantag inom en körning, men du kan också identifiera och svara tooerrors oberoende av hello köras automatiskt. 
[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) ger ett enkelt sätt toosend alla arbetsflöde händelser (inklusive status för alla kör och åtgärden) tooan Azure Storage-konto eller ett Azure-Händelsehubb. tooevaluate kör status, kan du övervaka hello loggar och mått eller publicera dem i alla övervakningsverktyg som du föredrar. Potentiella kan toostream alla hello händelser via Azure Event Hub i [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). I Stream Analytics kan du skriva live frågor av alla avvikelser, medelvärden och fel från hello diagnostikloggar. Stream Analytics kan enkelt skapa tooother datakällor som köer, ämnen, SQL, Azure Cosmos DB och Power BI.

## <a name="next-steps"></a>Nästa steg

* [Se hur en kund bygger felhantering med Azure Logikappar](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Hitta mer Logic Apps exempel och scenarier](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Lär dig hur toocreate automatiserade distributioner för logikappar](../logic-apps/logic-apps-create-deploy-template.md)
* [Skapa och distribuera Logic Apps i Visual Studio](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
