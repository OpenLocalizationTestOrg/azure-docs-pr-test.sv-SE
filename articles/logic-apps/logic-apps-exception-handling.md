---
title: Fel & undantagshantering - Azure Logic Apps | Microsoft Docs
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
ms.openlocfilehash: 9af2f71b3d288cc6f4e271d0915545d43a1249bc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a><span data-ttu-id="68c68-103">Hantera fel och undantag i Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="68c68-103">Handle errors and exceptions in Azure Logic Apps</span></span>

<span data-ttu-id="68c68-104">Med Azure Logikappar innehåller omfattande verktyg och mönster som hjälper dig att kontrollera din integreringar är stabilt och motståndskraftiga mot fel.</span><span class="sxs-lookup"><span data-stu-id="68c68-104">Azure Logic Apps provides rich tools and patterns to help you make sure your integrations are robust and resilient against failures.</span></span> <span data-ttu-id="68c68-105">Alla integration arkitektur utgör utmaningen i att se till att korrekt hantera driftavbrott eller problem från beroende system.</span><span class="sxs-lookup"><span data-stu-id="68c68-105">Any integration architecture poses the challenge of making sure to appropriately handle downtime or issues from dependent systems.</span></span> <span data-ttu-id="68c68-106">Logic Apps gör felhantering en förstklassig miljö, vilket ger dig de verktyg du behöver för att fungera på undantag och fel i dina arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="68c68-106">Logic Apps makes handling errors a first-class experience, giving you the tools you need to act on exceptions and errors in your workflows.</span></span>

## <a name="retry-policies"></a><span data-ttu-id="68c68-107">Försök principer</span><span class="sxs-lookup"><span data-stu-id="68c68-107">Retry policies</span></span>

<span data-ttu-id="68c68-108">En återförsöksprincip är den mest grundläggande typ av undantag och felhantering.</span><span class="sxs-lookup"><span data-stu-id="68c68-108">A retry policy is the most basic type of exception and error handling.</span></span> <span data-ttu-id="68c68-109">Om en inledande begäran misslyckades eller orsakade timeout (alla förfrågningar som resulterar i en 429 eller 5xx-svar), den här principen definierar om åtgärden bör försöka.</span><span class="sxs-lookup"><span data-stu-id="68c68-109">If an initial request timed out or failed (any request that results in a 429 or 5xx response), this policy defines whether the action should retry.</span></span> <span data-ttu-id="68c68-110">Som standard gör alla åtgärder 4 ytterligare gånger över 20 sekunders intervall.</span><span class="sxs-lookup"><span data-stu-id="68c68-110">By default, all actions retry 4 additional times over 20-second intervals.</span></span> <span data-ttu-id="68c68-111">Om den första begäranden tar emot en `500 Internal Server Error` svar, arbetsflödesmotorn pausar i 20 sekunder och försöker begäran igen.</span><span class="sxs-lookup"><span data-stu-id="68c68-111">So if the first request receives a `500 Internal Server Error` response, the workflow engine pauses for 20 seconds, and attempts the request again.</span></span> <span data-ttu-id="68c68-112">Om efter alla försök det fortfarande är undantag eller fel, arbetsflödet fortsätter och markerar Åtgärdsstatus som `Failed`.</span><span class="sxs-lookup"><span data-stu-id="68c68-112">If after all retries, the response is still an exception or failure, the workflow continues and marks the action status as `Failed`.</span></span>

<span data-ttu-id="68c68-113">Du kan konfigurera principer för försök i den **indata** för en viss åtgärd.</span><span class="sxs-lookup"><span data-stu-id="68c68-113">You can configure retry policies in the **inputs** for a particular action.</span></span> <span data-ttu-id="68c68-114">Du kan till exempel konfigurera en återförsöksprincip för upp till 4 gånger över 1 timme intervall.</span><span class="sxs-lookup"><span data-stu-id="68c68-114">For example, you can configure a retry policy to try as many as 4 times over 1-hour intervals.</span></span> <span data-ttu-id="68c68-115">Fullständig information om indataparametrar finns [arbetsflödesåtgärder och utlösare][retryPolicyMSDN].</span><span class="sxs-lookup"><span data-stu-id="68c68-115">For full details about input properties, see [Workflow Actions and Triggers][retryPolicyMSDN].</span></span>

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

<span data-ttu-id="68c68-116">Om du vill att HTTP-åtgärd försök 4 gånger och vänta i 10 minuter mellan varje försök, använder du följande definition:</span><span class="sxs-lookup"><span data-stu-id="68c68-116">If you wanted your HTTP action to retry 4 times and wait 10 minutes between each attempt, you would use the following definition:</span></span>

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

<span data-ttu-id="68c68-117">Mer information om syntax som stöds finns i [återförsöksprincip avsnittet i arbetsflödesåtgärder och utlösare][retryPolicyMSDN].</span><span class="sxs-lookup"><span data-stu-id="68c68-117">For more information on supported syntax, see the [retry-policy section in Workflow Actions and Triggers][retryPolicyMSDN].</span></span>

## <a name="catch-failures-with-the-runafter-property"></a><span data-ttu-id="68c68-118">Catch-fel med egenskapen RunAfter</span><span class="sxs-lookup"><span data-stu-id="68c68-118">Catch failures with the RunAfter property</span></span>

<span data-ttu-id="68c68-119">Varje logik app åtgärd anger vilka åtgärder som måste slutföras innan åtgärden startar, t.ex. sortera stegen i arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="68c68-119">Each logic app action declares which actions must finish before the action starts, like ordering the steps in your workflow.</span></span> <span data-ttu-id="68c68-120">I åtgärdsdefinitionen den här ordningen kallas den `runAfter` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="68c68-120">In the action definition, this ordering is known as the `runAfter` property.</span></span> <span data-ttu-id="68c68-121">Den här egenskapen är ett objekt som beskriver vilka åtgärder och status för åtgärden att utföra åtgärden.</span><span class="sxs-lookup"><span data-stu-id="68c68-121">This property is an object that describes which actions and action statuses execute the action.</span></span> <span data-ttu-id="68c68-122">Alla åtgärder som har lagts till via logik App Designer är som standard `runAfter` föregående steg om det förra steget `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="68c68-122">By default, all actions added through the Logic App Designer are set to `runAfter` the previous step if the previous step `Succeeded`.</span></span> <span data-ttu-id="68c68-123">Du kan anpassa värdet för att utlösa åtgärder när tidigare åtgärder har `Failed`, `Skipped`, eller en möjlig uppsättning med dessa värden.</span><span class="sxs-lookup"><span data-stu-id="68c68-123">However, you can customize this value to fire actions when previous actions have `Failed`, `Skipped`, or a possible set of these values.</span></span> <span data-ttu-id="68c68-124">Om du vill lägga till ett objekt till en avsedda Service Bus-ämne efter en viss åtgärd `Insert_Row` misslyckas, kan du använda följande `runAfter` konfiguration:</span><span class="sxs-lookup"><span data-stu-id="68c68-124">If you wanted to add an item to a designated Service Bus topic after a specific action `Insert_Row` fails, you could use the following `runAfter` configuration:</span></span>

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

<span data-ttu-id="68c68-125">Observera den `runAfter` -egenskapen anges eller om den `Insert_Row` åtgärden är `Failed`.</span><span class="sxs-lookup"><span data-stu-id="68c68-125">Notice the `runAfter` property is set to fire if the `Insert_Row` action is `Failed`.</span></span> <span data-ttu-id="68c68-126">Att köra instruktionen om Åtgärdsstatus är `Succeeded`, `Failed`, eller `Skipped`, använder du följande syntax:</span><span class="sxs-lookup"><span data-stu-id="68c68-126">To run the action if the action status is `Succeeded`, `Failed`, or `Skipped`, use this syntax:</span></span>

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> <span data-ttu-id="68c68-127">Åtgärder som körs och slutföras efter en föregående åtgärd har misslyckats, markeras som `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="68c68-127">Actions that run and complete successfully after a preceding action has failed, are marked as `Succeeded`.</span></span> <span data-ttu-id="68c68-128">Detta innebär att om du har fånga alla fel i ett arbetsflöde, kör själva har markerats som `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="68c68-128">This behavior means that if you successfully catch all failures in a workflow, the run itself is marked as `Succeeded`.</span></span>

## <a name="scopes-and-results-to-evaluate-actions"></a><span data-ttu-id="68c68-129">Omfång och resultat för att utvärdera åtgärder</span><span class="sxs-lookup"><span data-stu-id="68c68-129">Scopes and results to evaluate actions</span></span>

<span data-ttu-id="68c68-130">Liknar hur du kan köra efter enskilda åtgärder du kan också gruppera åtgärder i en [omfång](../logic-apps/logic-apps-loops-and-scopes.md), som fungerar som en logisk gruppering av åtgärder.</span><span class="sxs-lookup"><span data-stu-id="68c68-130">Similar to how you can run after individual actions, you can also group actions together inside a [scope](../logic-apps/logic-apps-loops-and-scopes.md), which act as a logical grouping of actions.</span></span> <span data-ttu-id="68c68-131">Scope är användbara både för att organisera dina logic app åtgärder och för att utföra sammanställd utvärderingar på status för ett omfång.</span><span class="sxs-lookup"><span data-stu-id="68c68-131">Scopes are useful both for organizing your logic app actions, and for performing aggregate evaluations on the status of a scope.</span></span> <span data-ttu-id="68c68-132">Området själva får status när alla åtgärder i ett scope är klar.</span><span class="sxs-lookup"><span data-stu-id="68c68-132">The scope itself receives a status after all actions in a scope have finished.</span></span> <span data-ttu-id="68c68-133">Scope-status bestäms med samma kriterier som körs.</span><span class="sxs-lookup"><span data-stu-id="68c68-133">The scope status is determined with the same criteria as a run.</span></span> <span data-ttu-id="68c68-134">Om sista åtgärden i en körning grenen `Failed` eller `Aborted`, status är `Failed`.</span><span class="sxs-lookup"><span data-stu-id="68c68-134">If the final action in an execution branch is `Failed` or `Aborted`, the status is `Failed`.</span></span>

<span data-ttu-id="68c68-135">Du kan använda för att utlösa specifika åtgärder efter fel som inträffade inom omfånget `runAfter` med en omfattning som är markerad `Failed`.</span><span class="sxs-lookup"><span data-stu-id="68c68-135">To fire specific actions for any failures that happened within the scope, you can use `runAfter` with a scope that is marked `Failed`.</span></span> <span data-ttu-id="68c68-136">Om *alla* det gick inte att utföra åtgärder i omfånget, kör ett scope misslyckas kan du skapa en enda åtgärd för att fånga fel.</span><span class="sxs-lookup"><span data-stu-id="68c68-136">If *any* actions in the scope fail, running after a scope fails lets you create a single action to catch failures.</span></span>

### <a name="getting-the-context-of-failures-with-results"></a><span data-ttu-id="68c68-137">Få kontext fel med resultat</span><span class="sxs-lookup"><span data-stu-id="68c68-137">Getting the context of failures with results</span></span>

<span data-ttu-id="68c68-138">Fånga fel från ett scope är användbart, men du kanske också vill kontext för att hjälpa dig att förstå exakt vilka åtgärder som misslyckades, och eventuella fel eller statuskoder som returnerades.</span><span class="sxs-lookup"><span data-stu-id="68c68-138">Although catching failures from a scope is useful, you might also want context to help you understand exactly which actions failed, and any errors or status codes that were returned.</span></span> <span data-ttu-id="68c68-139">Den `@result()` arbetsflödesfunktion ger kontext om resultatet av alla åtgärder i en omfattning.</span><span class="sxs-lookup"><span data-stu-id="68c68-139">The `@result()` workflow function provides context about the result of all actions in a scope.</span></span>

<span data-ttu-id="68c68-140">`@result()`tar en enda parameter, scope-namn och returnerar en matris med alla åtgärd resultat från i omfattningen.</span><span class="sxs-lookup"><span data-stu-id="68c68-140">`@result()` takes a single parameter, scope name, and returns an array of all the action results from within that scope.</span></span> <span data-ttu-id="68c68-141">Åtgärd eller skriva in samma attribut som den `@actions()` objektet, inklusive åtgärd starttid, sluttid för åtgärd, Åtgärdsstatus, åtgärden indata, åtgärden Korrelations-ID: N och åtgärden matar ut.</span><span class="sxs-lookup"><span data-stu-id="68c68-141">These action objects include the same attributes as the `@actions()` object, including action start time, action end time, action status, action inputs, action correlation IDs, and action outputs.</span></span> <span data-ttu-id="68c68-142">Om du vill skicka kontexten för alla åtgärder som misslyckades i ett omfång som du lätt kan koppla en `@result()` fungerar med en `runAfter`.</span><span class="sxs-lookup"><span data-stu-id="68c68-142">To send context of any actions that failed within a scope, you can easily pair an `@result()` function with a `runAfter`.</span></span>

<span data-ttu-id="68c68-143">Att köra en åtgärd *för varje* åtgärd i en omfattning som `Failed`, filtrera matris av resultaten till åtgärder som har misslyckats, kan du koppla `@result()` med en  **[Filter matris](../connectors/connectors-native-query.md)**  åtgärd och en  **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)**  loop.</span><span class="sxs-lookup"><span data-stu-id="68c68-143">To execute an action *for each* action in a scope that `Failed`, filter the array of results to actions that failed, you can pair `@result()` with a **[Filter Array](../connectors/connectors-native-query.md)** action and a **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)** loop.</span></span> <span data-ttu-id="68c68-144">Du kan ta matrisen filtrerade resultat och utföra en åtgärd för varje fel med hjälp av den **ForEach** loop.</span><span class="sxs-lookup"><span data-stu-id="68c68-144">You can take the filtered result array and perform an action for each failure using the **ForEach** loop.</span></span> <span data-ttu-id="68c68-145">Här är ett exempel, följt av en detaljerad förklaring som skickar en HTTP POST-begäran med brödtext för svar för alla åtgärder som inte omfattas `My_Scope`.</span><span class="sxs-lookup"><span data-stu-id="68c68-145">Here's an example, followed by a detailed explanation, that sends an HTTP POST request with the response body of any actions that failed within the scope `My_Scope`.</span></span>

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

<span data-ttu-id="68c68-146">Här följer en detaljerad genomgång som beskriver vad som händer:</span><span class="sxs-lookup"><span data-stu-id="68c68-146">Here's a detailed walkthrough to describe what happens:</span></span>

1. <span data-ttu-id="68c68-147">Att hämta resultatet av alla åtgärder inom `My_Scope`, **Filter matris** åtgärdsfilter `@result('My_Scope')`.</span><span class="sxs-lookup"><span data-stu-id="68c68-147">To get the result of all actions within `My_Scope`, the **Filter Array** action filters `@result('My_Scope')`.</span></span>

2. <span data-ttu-id="68c68-148">Villkoret för **Filter matris** valfri `@result()` objekt som har status som är lika med `Failed`.</span><span class="sxs-lookup"><span data-stu-id="68c68-148">The condition for **Filter Array** is any `@result()` item that has status equal to `Failed`.</span></span> <span data-ttu-id="68c68-149">Det här villkoret filtrerar matris med alla åtgärd resultat från `My_Scope` till en matris med endast misslyckade åtgärden resultat.</span><span class="sxs-lookup"><span data-stu-id="68c68-149">This condition filters the array with all action results from `My_Scope` to an array with only failed action results.</span></span>

3. <span data-ttu-id="68c68-150">Utföra en **för varje** åtgärd på den **filtrerade matris** matar ut.</span><span class="sxs-lookup"><span data-stu-id="68c68-150">Perform a **For Each** action on the **Filtered Array** outputs.</span></span> <span data-ttu-id="68c68-151">Det här steget utför en åtgärd *för varje* misslyckades åtgärden resultat som tidigare har filtrerats.</span><span class="sxs-lookup"><span data-stu-id="68c68-151">This step performs an action *for each* failed action result that was previously filtered.</span></span>

    <span data-ttu-id="68c68-152">Om en enda åtgärd i omfånget misslyckats åtgärder i den `foreach` bara körs en gång.</span><span class="sxs-lookup"><span data-stu-id="68c68-152">If a single action in the scope failed, the actions in the `foreach` run only once.</span></span> 
    <span data-ttu-id="68c68-153">Många misslyckade åtgärder gör att en åtgärd per fel.</span><span class="sxs-lookup"><span data-stu-id="68c68-153">Many failed actions cause one action per failure.</span></span>

4. <span data-ttu-id="68c68-154">Skicka en HTTP POST på den `foreach` objektet brödtext för svar eller `@item()['outputs']['body']`.</span><span class="sxs-lookup"><span data-stu-id="68c68-154">Send an HTTP POST on the `foreach` item response body, or `@item()['outputs']['body']`.</span></span> <span data-ttu-id="68c68-155">Den `@result()` form är samma som den `@actions()` form och kan parsas på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="68c68-155">The `@result()` item shape is the same as the `@actions()` shape, and can be parsed the same way.</span></span>

5. <span data-ttu-id="68c68-156">Innehåller två anpassade huvuden med misslyckade åtgärdsnamn `@item()['name']` och den kör klienten spårnings-ID `@item()['clientTrackingId']`.</span><span class="sxs-lookup"><span data-stu-id="68c68-156">Include two custom headers with the failed action name `@item()['name']` and the failed run client tracking ID `@item()['clientTrackingId']`.</span></span>

<span data-ttu-id="68c68-157">Här är ett exempel på en enda referens `@result()` objektet, visar den `name`, `body`, och `clientTrackingId` egenskaper som parsas i föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="68c68-157">For reference, here's an example of a single `@result()` item, showing the `name`, `body`, and `clientTrackingId` properties that are parsed in the previous example.</span></span> <span data-ttu-id="68c68-158">Utanför en `foreach`, `@result()` returnerar en matris med de här objekten.</span><span class="sxs-lookup"><span data-stu-id="68c68-158">Outside of a `foreach`, `@result()` returns an array of these objects.</span></span>

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

<span data-ttu-id="68c68-159">Du kan använda uttryck som visats för att utföra olika undantagshantering mönster.</span><span class="sxs-lookup"><span data-stu-id="68c68-159">To perform different exception handling patterns, you can use the expressions shown previously.</span></span> <span data-ttu-id="68c68-160">Du kan välja att utföra en enda undantagshantering åtgärd utanför omfånget som accepterar hela filtrerade matrisen fel och ta bort den `foreach`.</span><span class="sxs-lookup"><span data-stu-id="68c68-160">You might choose to execute a single exception handling action outside the scope that accepts the entire filtered array of failures, and remove the `foreach`.</span></span> <span data-ttu-id="68c68-161">Du kan även inkludera andra användbara egenskaper från den `@result()` svar som har visats.</span><span class="sxs-lookup"><span data-stu-id="68c68-161">You can also include other useful properties from the `@result()` response shown previously.</span></span>

## <a name="azure-diagnostics-and-telemetry"></a><span data-ttu-id="68c68-162">Azure-diagnostik och telemetri</span><span class="sxs-lookup"><span data-stu-id="68c68-162">Azure Diagnostics and telemetry</span></span>

<span data-ttu-id="68c68-163">Tidigare mönstren är bra sätt att hantera fel och undantag inom en körning, men du kan också identifiera och svara på fel som är oberoende av körningen sig själv.</span><span class="sxs-lookup"><span data-stu-id="68c68-163">The previous patterns are great way to handle errors and exceptions within a run, but you can also identify and respond to errors independent of the run itself.</span></span> 
<span data-ttu-id="68c68-164">[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) gör det enkelt att skicka alla arbetsflödeshändelser (inklusive status för alla kör och åtgärden) till ett Azure Storage-konto eller ett Azure-Händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="68c68-164">[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) provides a simple way to send all workflow events (including all run and action statuses) to an Azure Storage account or an Azure Event Hub.</span></span> <span data-ttu-id="68c68-165">Om du vill utvärdera kör status, kan du övervaka loggar och mått eller publicera dem i alla övervakningsverktyg som du föredrar.</span><span class="sxs-lookup"><span data-stu-id="68c68-165">To evaluate run statuses, you can monitor the logs and metrics, or publish them into any monitoring tool you prefer.</span></span> <span data-ttu-id="68c68-166">En potentiell alternativ är att strömma alla händelser via Azure Event Hub i [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="68c68-166">One potential option is to stream all the events through Azure Event Hub into [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span></span> <span data-ttu-id="68c68-167">Du kan skriva live frågor av alla avvikelser, medelvärden och fel från diagnostiska loggar i Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="68c68-167">In Stream Analytics, you can write live queries off any anomalies, averages, or failures from the diagnostic logs.</span></span> <span data-ttu-id="68c68-168">Stream Analytics kan enkelt utdata till andra datakällor som köer, ämnen, SQL, Azure Cosmos DB och Power BI.</span><span class="sxs-lookup"><span data-stu-id="68c68-168">Stream Analytics can easily output to other data sources like queues, topics, SQL, Azure Cosmos DB, and Power BI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="68c68-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="68c68-169">Next Steps</span></span>

* [<span data-ttu-id="68c68-170">Se hur en kund bygger felhantering med Azure Logikappar</span><span class="sxs-lookup"><span data-stu-id="68c68-170">See how a customer builds error handling with Azure Logic Apps</span></span>](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [<span data-ttu-id="68c68-171">Hitta mer Logic Apps exempel och scenarier</span><span class="sxs-lookup"><span data-stu-id="68c68-171">Find more Logic Apps examples and scenarios</span></span>](../logic-apps/logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="68c68-172">Lär dig hur du skapar automatiserad distribution för logic apps</span><span class="sxs-lookup"><span data-stu-id="68c68-172">Learn how to create automated deployments for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
* [<span data-ttu-id="68c68-173">Skapa och distribuera Logic Apps i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68c68-173">Build and deploy logic apps with Visual Studio</span></span>](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
