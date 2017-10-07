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
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a><span data-ttu-id="0369d-103">Hantera fel och undantag i Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="0369d-103">Handle errors and exceptions in Azure Logic Apps</span></span>

<span data-ttu-id="0369d-104">Med Azure Logikappar innehåller omfattande verktyg och mönster toohelp du kontrollera att din integreringar är stabilt och motståndskraftiga mot fel.</span><span class="sxs-lookup"><span data-stu-id="0369d-104">Azure Logic Apps provides rich tools and patterns toohelp you make sure your integrations are robust and resilient against failures.</span></span> <span data-ttu-id="0369d-105">Alla integration arkitektur utgör hello utmaningen i att göra att tooappropriately referensen driftstopp eller utfärdar från beroende system.</span><span class="sxs-lookup"><span data-stu-id="0369d-105">Any integration architecture poses hello challenge of making sure tooappropriately handle downtime or issues from dependent systems.</span></span> <span data-ttu-id="0369d-106">Logic Apps enkelt att hantera fel hello en förstklassig miljö, vilket ger dig verktyg du behöver tooact på undantag och fel i dina arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="0369d-106">Logic Apps makes handling errors a first-class experience, giving you hello tools you need tooact on exceptions and errors in your workflows.</span></span>

## <a name="retry-policies"></a><span data-ttu-id="0369d-107">Försök principer</span><span class="sxs-lookup"><span data-stu-id="0369d-107">Retry policies</span></span>

<span data-ttu-id="0369d-108">En återförsöksprincip är hello mest grundläggande typ av undantag och felhantering.</span><span class="sxs-lookup"><span data-stu-id="0369d-108">A retry policy is hello most basic type of exception and error handling.</span></span> <span data-ttu-id="0369d-109">Om en inledande begäran misslyckades eller orsakade timeout (alla förfrågningar som resulterar i en 429 eller 5xx-svar), den här principen definierar om hello åtgärden bör försöka.</span><span class="sxs-lookup"><span data-stu-id="0369d-109">If an initial request timed out or failed (any request that results in a 429 or 5xx response), this policy defines whether hello action should retry.</span></span> <span data-ttu-id="0369d-110">Som standard gör alla åtgärder 4 ytterligare gånger över 20 sekunders intervall.</span><span class="sxs-lookup"><span data-stu-id="0369d-110">By default, all actions retry 4 additional times over 20-second intervals.</span></span> <span data-ttu-id="0369d-111">Om hello första begäran tar emot en `500 Internal Server Error` svar, hello arbetsflödesmotorn pausar i 20 sekunder och försök hello begäran igen.</span><span class="sxs-lookup"><span data-stu-id="0369d-111">So if hello first request receives a `500 Internal Server Error` response, hello workflow engine pauses for 20 seconds, and attempts hello request again.</span></span> <span data-ttu-id="0369d-112">Om efter alla försök hello svar är fortfarande undantag eller fel, fortsätter hello arbetsflödet och markerar hello Åtgärdsstatus som `Failed`.</span><span class="sxs-lookup"><span data-stu-id="0369d-112">If after all retries, hello response is still an exception or failure, hello workflow continues and marks hello action status as `Failed`.</span></span>

<span data-ttu-id="0369d-113">Du kan konfigurera principer för försök i hello **indata** för en viss åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0369d-113">You can configure retry policies in hello **inputs** for a particular action.</span></span> <span data-ttu-id="0369d-114">Du kan till exempel konfigurera en försök princip tootry upp till 4 gånger över 1 timme intervall.</span><span class="sxs-lookup"><span data-stu-id="0369d-114">For example, you can configure a retry policy tootry as many as 4 times over 1-hour intervals.</span></span> <span data-ttu-id="0369d-115">Fullständig information om indataparametrar finns [arbetsflödesåtgärder och utlösare][retryPolicyMSDN].</span><span class="sxs-lookup"><span data-stu-id="0369d-115">For full details about input properties, see [Workflow Actions and Triggers][retryPolicyMSDN].</span></span>

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

<span data-ttu-id="0369d-116">Om du vill att din HTTP-åtgärd tooretry 4 gånger och vänta i 10 minuter mellan varje försök använder hello definitionen:</span><span class="sxs-lookup"><span data-stu-id="0369d-116">If you wanted your HTTP action tooretry 4 times and wait 10 minutes between each attempt, you would use hello following definition:</span></span>

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

<span data-ttu-id="0369d-117">Mer information om syntax som stöds finns i hello [återförsöksprincip avsnittet i arbetsflödesåtgärder och utlösare][retryPolicyMSDN].</span><span class="sxs-lookup"><span data-stu-id="0369d-117">For more information on supported syntax, see hello [retry-policy section in Workflow Actions and Triggers][retryPolicyMSDN].</span></span>

## <a name="catch-failures-with-hello-runafter-property"></a><span data-ttu-id="0369d-118">Catch-fel med hello RunAfter egenskapen</span><span class="sxs-lookup"><span data-stu-id="0369d-118">Catch failures with hello RunAfter property</span></span>

<span data-ttu-id="0369d-119">Varje logik app åtgärd anger vilka åtgärder som måste slutföras innan hello åtgärden startar, t.ex. sortera hello steg i arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="0369d-119">Each logic app action declares which actions must finish before hello action starts, like ordering hello steps in your workflow.</span></span> <span data-ttu-id="0369d-120">Hello åtgärdsdefinition den här ordningen är känt som hello `runAfter` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="0369d-120">In hello action definition, this ordering is known as hello `runAfter` property.</span></span> <span data-ttu-id="0369d-121">Den här egenskapen är ett objekt som beskriver vilka åtgärder och status för åtgärden köra hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0369d-121">This property is an object that describes which actions and action statuses execute hello action.</span></span> <span data-ttu-id="0369d-122">Alla åtgärder som har lagts till via hello logik App Designer är som standard för`runAfter` hello föregående steg om hello föregående steg `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="0369d-122">By default, all actions added through hello Logic App Designer are set too`runAfter` hello previous step if hello previous step `Succeeded`.</span></span> <span data-ttu-id="0369d-123">Du kan dock anpassa det här värdet toofire åtgärder när tidigare åtgärder har `Failed`, `Skipped`, eller en möjlig uppsättning med dessa värden.</span><span class="sxs-lookup"><span data-stu-id="0369d-123">However, you can customize this value toofire actions when previous actions have `Failed`, `Skipped`, or a possible set of these values.</span></span> <span data-ttu-id="0369d-124">Om du vill tooadd ett objekt tooa avses Service Bus-ämne efter en viss åtgärd `Insert_Row` misslyckas, kan du använda följande hello `runAfter` konfiguration:</span><span class="sxs-lookup"><span data-stu-id="0369d-124">If you wanted tooadd an item tooa designated Service Bus topic after a specific action `Insert_Row` fails, you could use hello following `runAfter` configuration:</span></span>

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

<span data-ttu-id="0369d-125">Meddelande hello `runAfter` egenskapen toofire om hello `Insert_Row` åtgärden är `Failed`.</span><span class="sxs-lookup"><span data-stu-id="0369d-125">Notice hello `runAfter` property is set toofire if hello `Insert_Row` action is `Failed`.</span></span> <span data-ttu-id="0369d-126">toorun hello åtgärd om hello Åtgärdsstatus är `Succeeded`, `Failed`, eller `Skipped`, använder du följande syntax:</span><span class="sxs-lookup"><span data-stu-id="0369d-126">toorun hello action if hello action status is `Succeeded`, `Failed`, or `Skipped`, use this syntax:</span></span>

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> <span data-ttu-id="0369d-127">Åtgärder som körs och slutföras efter en föregående åtgärd har misslyckats, markeras som `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="0369d-127">Actions that run and complete successfully after a preceding action has failed, are marked as `Succeeded`.</span></span> <span data-ttu-id="0369d-128">Den här funktionen innebär att om du har fånga alla fel i ett arbetsflöde, hello köras automatiskt har markerats som `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="0369d-128">This behavior means that if you successfully catch all failures in a workflow, hello run itself is marked as `Succeeded`.</span></span>

## <a name="scopes-and-results-tooevaluate-actions"></a><span data-ttu-id="0369d-129">Omfång och resultat tooevaluate åtgärder</span><span class="sxs-lookup"><span data-stu-id="0369d-129">Scopes and results tooevaluate actions</span></span>

<span data-ttu-id="0369d-130">Liknande toohow som du kan köra efter enskilda åtgärder du kan också gruppera åtgärder i en [omfång](../logic-apps/logic-apps-loops-and-scopes.md), som fungerar som en logisk gruppering av åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0369d-130">Similar toohow you can run after individual actions, you can also group actions together inside a [scope](../logic-apps/logic-apps-loops-and-scopes.md), which act as a logical grouping of actions.</span></span> <span data-ttu-id="0369d-131">Scope är användbara både för att organisera dina logic app åtgärder och för att utföra sammanställd utvärderingar på hello status för ett omfång.</span><span class="sxs-lookup"><span data-stu-id="0369d-131">Scopes are useful both for organizing your logic app actions, and for performing aggregate evaluations on hello status of a scope.</span></span> <span data-ttu-id="0369d-132">hello scope själva får status när alla åtgärder i ett scope är klar.</span><span class="sxs-lookup"><span data-stu-id="0369d-132">hello scope itself receives a status after all actions in a scope have finished.</span></span> <span data-ttu-id="0369d-133">hello scope status bestäms med hello samma kriterier som körs.</span><span class="sxs-lookup"><span data-stu-id="0369d-133">hello scope status is determined with hello same criteria as a run.</span></span> <span data-ttu-id="0369d-134">Om hello sista åtgärd i en körning grenen är `Failed` eller `Aborted`, hello status är `Failed`.</span><span class="sxs-lookup"><span data-stu-id="0369d-134">If hello final action in an execution branch is `Failed` or `Aborted`, hello status is `Failed`.</span></span>

<span data-ttu-id="0369d-135">toofire specifika åtgärder efter fel som inträffade i hello omfång som du kan använda `runAfter` med en omfattning som är markerad `Failed`.</span><span class="sxs-lookup"><span data-stu-id="0369d-135">toofire specific actions for any failures that happened within hello scope, you can use `runAfter` with a scope that is marked `Failed`.</span></span> <span data-ttu-id="0369d-136">Om *alla* åtgärder i hello omfång misslyckas, kör ett scope misslyckas kan du skapa en enda åtgärd toocatch fel.</span><span class="sxs-lookup"><span data-stu-id="0369d-136">If *any* actions in hello scope fail, running after a scope fails lets you create a single action toocatch failures.</span></span>

### <a name="getting-hello-context-of-failures-with-results"></a><span data-ttu-id="0369d-137">Få hello kontext fel med resultat</span><span class="sxs-lookup"><span data-stu-id="0369d-137">Getting hello context of failures with results</span></span>

<span data-ttu-id="0369d-138">Även om det är användbart fånga fel från ett scope, kan du också kontexten toohelp du förstå exakt vilka åtgärder som misslyckades, och eventuella fel eller statuskoder som returnerades.</span><span class="sxs-lookup"><span data-stu-id="0369d-138">Although catching failures from a scope is useful, you might also want context toohelp you understand exactly which actions failed, and any errors or status codes that were returned.</span></span> <span data-ttu-id="0369d-139">Hej `@result()` arbetsflödesfunktion ger kontext om hello resultatet av alla åtgärder i en omfattning.</span><span class="sxs-lookup"><span data-stu-id="0369d-139">hello `@result()` workflow function provides context about hello result of all actions in a scope.</span></span>

<span data-ttu-id="0369d-140">`@result()`tar en enda parameter, scope-namn och returnerar en matris med alla hello åtgärd resultat från i omfattningen.</span><span class="sxs-lookup"><span data-stu-id="0369d-140">`@result()` takes a single parameter, scope name, and returns an array of all hello action results from within that scope.</span></span> <span data-ttu-id="0369d-141">Dessa åtgärder objekt innefattar hello samma attribut som hello `@actions()` objektet, inklusive åtgärd starttid, sluttid för åtgärd, Åtgärdsstatus, åtgärden indata, åtgärden Korrelations-ID: N och åtgärden matar ut.</span><span class="sxs-lookup"><span data-stu-id="0369d-141">These action objects include hello same attributes as hello `@actions()` object, including action start time, action end time, action status, action inputs, action correlation IDs, and action outputs.</span></span> <span data-ttu-id="0369d-142">toosend kontexten för alla åtgärder som misslyckades i ett omfång, du lätt kan koppla en `@result()` fungerar med en `runAfter`.</span><span class="sxs-lookup"><span data-stu-id="0369d-142">toosend context of any actions that failed within a scope, you can easily pair an `@result()` function with a `runAfter`.</span></span>

<span data-ttu-id="0369d-143">tooexecute åtgärden *för varje* åtgärd i en omfattning som `Failed`filter hello matris med resultat tooactions som har misslyckats, kan du koppla `@result()` med en  **[Filter matris](../connectors/connectors-native-query.md)**  åtgärd och en  **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)**  loop.</span><span class="sxs-lookup"><span data-stu-id="0369d-143">tooexecute an action *for each* action in a scope that `Failed`, filter hello array of results tooactions that failed, you can pair `@result()` with a **[Filter Array](../connectors/connectors-native-query.md)** action and a **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)** loop.</span></span> <span data-ttu-id="0369d-144">Du kan ta hello filtrerade resultat matris och utföra en åtgärd för varje fel med hello **ForEach** loop.</span><span class="sxs-lookup"><span data-stu-id="0369d-144">You can take hello filtered result array and perform an action for each failure using hello **ForEach** loop.</span></span> <span data-ttu-id="0369d-145">Här är ett exempel, följt av en detaljerad förklaring som skickar en HTTP POST-begäran med hello svarstexten för alla åtgärder som inte omfattas hello `My_Scope`.</span><span class="sxs-lookup"><span data-stu-id="0369d-145">Here's an example, followed by a detailed explanation, that sends an HTTP POST request with hello response body of any actions that failed within hello scope `My_Scope`.</span></span>

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

<span data-ttu-id="0369d-146">Här följer en detaljerad genomgång toodescribe händer:</span><span class="sxs-lookup"><span data-stu-id="0369d-146">Here's a detailed walkthrough toodescribe what happens:</span></span>

1. <span data-ttu-id="0369d-147">tooget hello resultatet av alla åtgärder inom `My_Scope`, hello **Filter matris** åtgärdsfilter `@result('My_Scope')`.</span><span class="sxs-lookup"><span data-stu-id="0369d-147">tooget hello result of all actions within `My_Scope`, hello **Filter Array** action filters `@result('My_Scope')`.</span></span>

2. <span data-ttu-id="0369d-148">Hej villkor för **Filter matris** valfri `@result()` objekt som har status som är lika för`Failed`.</span><span class="sxs-lookup"><span data-stu-id="0369d-148">hello condition for **Filter Array** is any `@result()` item that has status equal too`Failed`.</span></span> <span data-ttu-id="0369d-149">Det här villkoret filtrerar hello matris med alla åtgärd resultat från `My_Scope` tooan matris med endast misslyckades åtgärden resultat.</span><span class="sxs-lookup"><span data-stu-id="0369d-149">This condition filters hello array with all action results from `My_Scope` tooan array with only failed action results.</span></span>

3. <span data-ttu-id="0369d-150">Utföra en **för varje** åtgärd på hello **filtrerade matris** matar ut.</span><span class="sxs-lookup"><span data-stu-id="0369d-150">Perform a **For Each** action on hello **Filtered Array** outputs.</span></span> <span data-ttu-id="0369d-151">Det här steget utför en åtgärd *för varje* misslyckades åtgärden resultat som tidigare har filtrerats.</span><span class="sxs-lookup"><span data-stu-id="0369d-151">This step performs an action *for each* failed action result that was previously filtered.</span></span>

    <span data-ttu-id="0369d-152">Om en enda åtgärd i hello omfång misslyckades hello åtgärder i hello `foreach` bara körs en gång.</span><span class="sxs-lookup"><span data-stu-id="0369d-152">If a single action in hello scope failed, hello actions in hello `foreach` run only once.</span></span> 
    <span data-ttu-id="0369d-153">Många misslyckade åtgärder gör att en åtgärd per fel.</span><span class="sxs-lookup"><span data-stu-id="0369d-153">Many failed actions cause one action per failure.</span></span>

4. <span data-ttu-id="0369d-154">Skicka en HTTP POST på hello `foreach` objektet brödtext för svar eller `@item()['outputs']['body']`.</span><span class="sxs-lookup"><span data-stu-id="0369d-154">Send an HTTP POST on hello `foreach` item response body, or `@item()['outputs']['body']`.</span></span> <span data-ttu-id="0369d-155">Hej `@result()` form är hello samma som hello `@actions()` form och kan parsas hello samma sätt.</span><span class="sxs-lookup"><span data-stu-id="0369d-155">hello `@result()` item shape is hello same as hello `@actions()` shape, and can be parsed hello same way.</span></span>

5. <span data-ttu-id="0369d-156">Innehåller två anpassade huvuden med namn på hello misslyckad åtgärd `@item()['name']` och hello misslyckades kör klienten spårnings-ID `@item()['clientTrackingId']`.</span><span class="sxs-lookup"><span data-stu-id="0369d-156">Include two custom headers with hello failed action name `@item()['name']` and hello failed run client tracking ID `@item()['clientTrackingId']`.</span></span>

<span data-ttu-id="0369d-157">Här är ett exempel på en enda referens `@result()` artikeln, visar hello `name`, `body`, och `clientTrackingId` egenskaper som parsas i hello föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="0369d-157">For reference, here's an example of a single `@result()` item, showing hello `name`, `body`, and `clientTrackingId` properties that are parsed in hello previous example.</span></span> <span data-ttu-id="0369d-158">Utanför en `foreach`, `@result()` returnerar en matris med de här objekten.</span><span class="sxs-lookup"><span data-stu-id="0369d-158">Outside of a `foreach`, `@result()` returns an array of these objects.</span></span>

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

<span data-ttu-id="0369d-159">Du kan använda hello uttryck såg tidigare tooperform olika undantagshantering mönster.</span><span class="sxs-lookup"><span data-stu-id="0369d-159">tooperform different exception handling patterns, you can use hello expressions shown previously.</span></span> <span data-ttu-id="0369d-160">Du kan välja tooexecute en enda undantagshantering åtgärd utanför hello omfattning som accepterar hello hela filtrerade array fel och ta bort hello `foreach`.</span><span class="sxs-lookup"><span data-stu-id="0369d-160">You might choose tooexecute a single exception handling action outside hello scope that accepts hello entire filtered array of failures, and remove hello `foreach`.</span></span> <span data-ttu-id="0369d-161">Du kan även inkludera andra användbara egenskaper från hello `@result()` svar som har visats.</span><span class="sxs-lookup"><span data-stu-id="0369d-161">You can also include other useful properties from hello `@result()` response shown previously.</span></span>

## <a name="azure-diagnostics-and-telemetry"></a><span data-ttu-id="0369d-162">Azure-diagnostik och telemetri</span><span class="sxs-lookup"><span data-stu-id="0369d-162">Azure Diagnostics and telemetry</span></span>

<span data-ttu-id="0369d-163">hello tidigare mönstren är bra sätt toohandle fel och undantag inom en körning, men du kan också identifiera och svara tooerrors oberoende av hello köras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="0369d-163">hello previous patterns are great way toohandle errors and exceptions within a run, but you can also identify and respond tooerrors independent of hello run itself.</span></span> 
<span data-ttu-id="0369d-164">[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) ger ett enkelt sätt toosend alla arbetsflöde händelser (inklusive status för alla kör och åtgärden) tooan Azure Storage-konto eller ett Azure-Händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="0369d-164">[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) provides a simple way toosend all workflow events (including all run and action statuses) tooan Azure Storage account or an Azure Event Hub.</span></span> <span data-ttu-id="0369d-165">tooevaluate kör status, kan du övervaka hello loggar och mått eller publicera dem i alla övervakningsverktyg som du föredrar.</span><span class="sxs-lookup"><span data-stu-id="0369d-165">tooevaluate run statuses, you can monitor hello logs and metrics, or publish them into any monitoring tool you prefer.</span></span> <span data-ttu-id="0369d-166">Potentiella kan toostream alla hello händelser via Azure Event Hub i [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="0369d-166">One potential option is toostream all hello events through Azure Event Hub into [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span></span> <span data-ttu-id="0369d-167">I Stream Analytics kan du skriva live frågor av alla avvikelser, medelvärden och fel från hello diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="0369d-167">In Stream Analytics, you can write live queries off any anomalies, averages, or failures from hello diagnostic logs.</span></span> <span data-ttu-id="0369d-168">Stream Analytics kan enkelt skapa tooother datakällor som köer, ämnen, SQL, Azure Cosmos DB och Power BI.</span><span class="sxs-lookup"><span data-stu-id="0369d-168">Stream Analytics can easily output tooother data sources like queues, topics, SQL, Azure Cosmos DB, and Power BI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0369d-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0369d-169">Next Steps</span></span>

* [<span data-ttu-id="0369d-170">Se hur en kund bygger felhantering med Azure Logikappar</span><span class="sxs-lookup"><span data-stu-id="0369d-170">See how a customer builds error handling with Azure Logic Apps</span></span>](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [<span data-ttu-id="0369d-171">Hitta mer Logic Apps exempel och scenarier</span><span class="sxs-lookup"><span data-stu-id="0369d-171">Find more Logic Apps examples and scenarios</span></span>](../logic-apps/logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="0369d-172">Lär dig hur toocreate automatiserade distributioner för logikappar</span><span class="sxs-lookup"><span data-stu-id="0369d-172">Learn how toocreate automated deployments for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
* [<span data-ttu-id="0369d-173">Skapa och distribuera Logic Apps i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0369d-173">Build and deploy logic apps with Visual Studio</span></span>](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
