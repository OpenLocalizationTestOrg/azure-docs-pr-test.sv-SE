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
# <a name="workflow-actions-and-triggers-for-azure-logic-apps"></a><span data-ttu-id="46f9e-102">Arbetsflödesåtgärder och utlösare för Logic Apps i Azure</span><span class="sxs-lookup"><span data-stu-id="46f9e-102">Workflow actions and triggers for Azure Logic Apps</span></span>

<span data-ttu-id="46f9e-103">Logikappar består av utlösare och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="46f9e-103">Logic apps consist of triggers and actions.</span></span> <span data-ttu-id="46f9e-104">Det finns sex olika typer av utlösare.</span><span class="sxs-lookup"><span data-stu-id="46f9e-104">There are six types of triggers.</span></span> <span data-ttu-id="46f9e-105">Varje typ har olika gränssnitt och olika beteenden.</span><span class="sxs-lookup"><span data-stu-id="46f9e-105">Each type has different interface and different behavior.</span></span> <span data-ttu-id="46f9e-106">Du kan också lära dig om andra uppgifter genom att titta på information om den [språk i arbetsflödesdefinitionen](logic-apps-workflow-definition-language.md).</span><span class="sxs-lookup"><span data-stu-id="46f9e-106">You can also learn about other details by looking at the details of the [Workflow Definition Language](logic-apps-workflow-definition-language.md).</span></span>  
  
<span data-ttu-id="46f9e-107">Läs vidare för att lära dig mer om utlösare och åtgärder och hur du kan använda dem för att skapa logikappar för att förbättra din affärsprocesser och arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="46f9e-107">Read on to learn more about triggers and actions and how you might use them to build logic apps to improve your business processes and workflows.</span></span>  
  
### <a name="triggers"></a><span data-ttu-id="46f9e-108">Utlösare</span><span class="sxs-lookup"><span data-stu-id="46f9e-108">Triggers</span></span>  

<span data-ttu-id="46f9e-109">En utlösare Anger anrop som kan initiera en körning av arbetsflödet logik app.</span><span class="sxs-lookup"><span data-stu-id="46f9e-109">A trigger specifies the calls that can initiate a run of your logic app workflow.</span></span> <span data-ttu-id="46f9e-110">Här följer två olika sätt att starta en körning av arbetsflödet:</span><span class="sxs-lookup"><span data-stu-id="46f9e-110">Here are the two different ways to initiate a run of your workflow:</span></span>  
  
-   <span data-ttu-id="46f9e-111">En avsökning utlösare</span><span class="sxs-lookup"><span data-stu-id="46f9e-111">A polling trigger</span></span>  

-   <span data-ttu-id="46f9e-112">En push-utlösare - genom att anropa den [arbetsflöde Service REST API](https://docs.microsoft.com/rest/api/logic/workflows)</span><span class="sxs-lookup"><span data-stu-id="46f9e-112">A push trigger - by calling the [Workflow Service REST API](https://docs.microsoft.com/rest/api/logic/workflows)</span></span>  
  
<span data-ttu-id="46f9e-113">Alla utlösare innehålla de översta elementen:</span><span class="sxs-lookup"><span data-stu-id="46f9e-113">All triggers contain these top-level elements:</span></span>  
  
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

### <a name="trigger-types-and-their-inputs"></a><span data-ttu-id="46f9e-114">Typer av utlösare och deras indata</span><span class="sxs-lookup"><span data-stu-id="46f9e-114">Trigger types and their inputs</span></span>  

<span data-ttu-id="46f9e-115">Du kan använda dessa typer av utlösare:</span><span class="sxs-lookup"><span data-stu-id="46f9e-115">You can use these types of triggers:</span></span>
  
-   <span data-ttu-id="46f9e-116">**Begära** \- gör logikappen en slutpunkt att anropa</span><span class="sxs-lookup"><span data-stu-id="46f9e-116">**Request** \- Makes the logic app an endpoint for you to call</span></span>  
  
-   <span data-ttu-id="46f9e-117">**Återkommande** \- utlöses baserat på ett definierat schema</span><span class="sxs-lookup"><span data-stu-id="46f9e-117">**Recurrence** \- Fires based on a defined schedule</span></span>  
  
-   <span data-ttu-id="46f9e-118">**HTTP** \- avsöker en HTTP-slutpunkt för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="46f9e-118">**HTTP** \- Polls an HTTP web endpoint.</span></span> <span data-ttu-id="46f9e-119">HTTP-slutpunkten måste motsvara en specifik utlösande kontraktet \- antingen med hjälp av en 202\-asynk.mönster, eller genom att returnera en matris</span><span class="sxs-lookup"><span data-stu-id="46f9e-119">The HTTP endpoint must conform to a specific triggering contract \- either by using a 202\-async pattern, or by returning an array</span></span>  
  
-   <span data-ttu-id="46f9e-120">**ApiConnection** \- Utlös avsökningar t.ex. HTTP, men den kan du utnyttja den [Microsoft-hanterade API: er](https://docs.microsoft.com/azure/connectors/apis-list)</span><span class="sxs-lookup"><span data-stu-id="46f9e-120">**ApiConnection** \- Polls like the HTTP trigger, however, it takes advantage of the [Microsoft-managed APIs](https://docs.microsoft.com/azure/connectors/apis-list)</span></span>  
  
-   <span data-ttu-id="46f9e-121">**HTTPWebhook** \- öppnas en slutpunkt som liknar den manuella utlösaren, men också anropar till en angiven URL för att registrera och avregistrera</span><span class="sxs-lookup"><span data-stu-id="46f9e-121">**HTTPWebhook** \- Opens an endpoint, similar to the Manual trigger, however, it also calls out to a specified URL to register and unregister</span></span>  
  
-   <span data-ttu-id="46f9e-122">**ApiConnectionWebhook** \- Operates som HTTPWebhook utlösaren genom att dra nytta av de Microsoft-hanterade API: er</span><span class="sxs-lookup"><span data-stu-id="46f9e-122">**ApiConnectionWebhook** \- Operates like the HTTPWebhook trigger by taking advantage of the Microsoft-managed APIs</span></span>       
    <span data-ttu-id="46f9e-123">Varje typ av utlösare som har en annan uppsättning **indata** som definierar sitt beteende.</span><span class="sxs-lookup"><span data-stu-id="46f9e-123">Each trigger type has a different set of **inputs** that defines its behavior.</span></span>  
  
## <a name="request-trigger"></a><span data-ttu-id="46f9e-124">Begäran utlösare</span><span class="sxs-lookup"><span data-stu-id="46f9e-124">Request trigger</span></span>  

<span data-ttu-id="46f9e-125">Den här utlösaren fungerar som en slutpunkt som anropas via en HTTP-begäran att anropa logikappen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-125">This trigger serves as an endpoint that you call via an HTTP Request to invoke your logic app.</span></span> <span data-ttu-id="46f9e-126">Det ser ut som i det här exemplet en utlösare för begäran:</span><span class="sxs-lookup"><span data-stu-id="46f9e-126">A request trigger looks like this example:</span></span>  
  
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

<span data-ttu-id="46f9e-127">Det finns även en valfri egenskap som kallas **schemat**:</span><span class="sxs-lookup"><span data-stu-id="46f9e-127">There is also an optional property called **schema**:</span></span>  
  
|<span data-ttu-id="46f9e-128">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="46f9e-128">Element name</span></span>|<span data-ttu-id="46f9e-129">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-129">Required</span></span>|<span data-ttu-id="46f9e-130">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-130">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="46f9e-131">Schemat</span><span class="sxs-lookup"><span data-stu-id="46f9e-131">schema</span></span>|<span data-ttu-id="46f9e-132">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-132">No</span></span>|<span data-ttu-id="46f9e-133">En JSON-schema som kontrollerar den inkommande begäranden.</span><span class="sxs-lookup"><span data-stu-id="46f9e-133">A JSON schema that validates the incoming request.</span></span> <span data-ttu-id="46f9e-134">Användbart för att hjälpa efterföljande arbetsflödessteg veta vilka egenskaper som ska referera till.</span><span class="sxs-lookup"><span data-stu-id="46f9e-134">Useful for helping subsequent workflow steps know which properties to reference.</span></span>|

<span data-ttu-id="46f9e-135">Om du vill anropa den här slutpunkten måste du anropa den *listCallbackUrl* API.</span><span class="sxs-lookup"><span data-stu-id="46f9e-135">To invoke this endpoint, you need to call the *listCallbackUrl* API.</span></span> <span data-ttu-id="46f9e-136">Se [arbetsflöde Service REST API](https://docs.microsoft.com/rest/api/logic/workflows).</span><span class="sxs-lookup"><span data-stu-id="46f9e-136">See [Workflow Service REST API](https://docs.microsoft.com/rest/api/logic/workflows).</span></span>  
  
## <a name="recurrence-trigger"></a><span data-ttu-id="46f9e-137">Utlösare för upprepning</span><span class="sxs-lookup"><span data-stu-id="46f9e-137">Recurrence trigger</span></span>  

<span data-ttu-id="46f9e-138">En utlösare för upprepning är något som körs baserat på ett definierat schema.</span><span class="sxs-lookup"><span data-stu-id="46f9e-138">A Recurrence trigger is one that runs based on a defined schedule.</span></span> <span data-ttu-id="46f9e-139">Dessa utlösare kan se ut det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="46f9e-139">Such a trigger might look like this example:</span></span>  

```json
"dailyReport" : {
    "type": "recurrence",
    "recurrence": {
        "frequency": "Day",
        "interval": "1"
    }
}
```

<span data-ttu-id="46f9e-140">Som du ser är ett enkelt sätt att köra ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="46f9e-140">As you can see, it is a simple way to run a workflow.</span></span>  
  
|<span data-ttu-id="46f9e-141">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="46f9e-141">Element name</span></span>|<span data-ttu-id="46f9e-142">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-142">Required</span></span>|<span data-ttu-id="46f9e-143">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-143">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="46f9e-144">frekvens</span><span class="sxs-lookup"><span data-stu-id="46f9e-144">frequency</span></span>|<span data-ttu-id="46f9e-145">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-145">Yes</span></span>|<span data-ttu-id="46f9e-146">Hur ofta utlösaren körs.</span><span class="sxs-lookup"><span data-stu-id="46f9e-146">How often the trigger executes.</span></span> <span data-ttu-id="46f9e-147">Använd endast ett av dessa värden: sekund, minut, timma, dag, vecka, månad eller år</span><span class="sxs-lookup"><span data-stu-id="46f9e-147">Use only one of these possible values: second, minute, hour, day, week, month, or year</span></span>|  
|<span data-ttu-id="46f9e-148">intervall</span><span class="sxs-lookup"><span data-stu-id="46f9e-148">interval</span></span>|<span data-ttu-id="46f9e-149">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-149">Yes</span></span>|<span data-ttu-id="46f9e-150">Intervallet för den angivna frekvensen för återkommande</span><span class="sxs-lookup"><span data-stu-id="46f9e-150">Interval of the given frequency for the recurrence</span></span>|  
|<span data-ttu-id="46f9e-151">startTime</span><span class="sxs-lookup"><span data-stu-id="46f9e-151">startTime</span></span>|<span data-ttu-id="46f9e-152">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-152">No</span></span>|<span data-ttu-id="46f9e-153">Om en startTime anges utan en UTC-förskjutningen, används den här tidszonen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-153">If a startTime is provided without a UTC offset, this timeZone is used.</span></span>|  
|<span data-ttu-id="46f9e-154">Tidszon</span><span class="sxs-lookup"><span data-stu-id="46f9e-154">timeZone</span></span>|<span data-ttu-id="46f9e-155">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-155">no</span></span>|<span data-ttu-id="46f9e-156">Om en startTime anges utan en UTC-förskjutningen, används den här tidszonen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-156">If a startTime is provided without a UTC offset, this timeZone is used.</span></span>|  
  
<span data-ttu-id="46f9e-157">Du kan även schemalägga en utlösare för att starta körning vid en viss tidpunkt i framtiden.</span><span class="sxs-lookup"><span data-stu-id="46f9e-157">You can also schedule a trigger to start executing at some point in the future.</span></span> <span data-ttu-id="46f9e-158">Om du vill starta en vecka rapport varje måndag kan du schemalägga logikappen att starta varje måndag genom att skapa följande utlösaren:</span><span class="sxs-lookup"><span data-stu-id="46f9e-158">For example, if you want to start a weekly report every Monday you can schedule the logic app to start every Monday by creating the following trigger:</span></span>  

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

## <a name="http-trigger"></a><span data-ttu-id="46f9e-159">HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="46f9e-159">HTTP trigger</span></span>  

<span data-ttu-id="46f9e-160">HTTP-utlösare avsöker den angivna slutpunkten och kontrollera svaret för att avgöra om arbetsflödet ska köras.</span><span class="sxs-lookup"><span data-stu-id="46f9e-160">HTTP triggers poll a specified endpoint and check the response to determine whether the workflow should be executed.</span></span> <span data-ttu-id="46f9e-161">Objektet indata tar uppsättningen parametrar som krävs för att konstruera ett HTTP-anrop:</span><span class="sxs-lookup"><span data-stu-id="46f9e-161">The inputs object takes the set of parameters required to construct an HTTP call:</span></span>  
  
|<span data-ttu-id="46f9e-162">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="46f9e-162">Element name</span></span>|<span data-ttu-id="46f9e-163">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-163">Required</span></span>|<span data-ttu-id="46f9e-164">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-164">Description</span></span>|<span data-ttu-id="46f9e-165">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-165">Type</span></span>|  
|----------------|------------|---------------|--------|  
|<span data-ttu-id="46f9e-166">Metoden</span><span class="sxs-lookup"><span data-stu-id="46f9e-166">method</span></span>|<span data-ttu-id="46f9e-167">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-167">yes</span></span>|<span data-ttu-id="46f9e-168">Kan vara något av följande metoder för HTTP: GET, POST, PUT, DELETE, KORRIGERINGSFIL eller HEAD</span><span class="sxs-lookup"><span data-stu-id="46f9e-168">Can be one of the following HTTP methods: GET, POST, PUT, DELETE, PATCH, or HEAD</span></span>|<span data-ttu-id="46f9e-169">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-169">String</span></span>|  
|<span data-ttu-id="46f9e-170">URI: N</span><span class="sxs-lookup"><span data-stu-id="46f9e-170">uri</span></span>|<span data-ttu-id="46f9e-171">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-171">yes</span></span>|<span data-ttu-id="46f9e-172">Http eller https slutpunkten som anropas.</span><span class="sxs-lookup"><span data-stu-id="46f9e-172">The http or https endpoint that is called.</span></span> <span data-ttu-id="46f9e-173">Högst 2 kilobyte.</span><span class="sxs-lookup"><span data-stu-id="46f9e-173">Maximum of 2 kilobytes.</span></span>|<span data-ttu-id="46f9e-174">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-174">String</span></span>|  
|<span data-ttu-id="46f9e-175">frågor</span><span class="sxs-lookup"><span data-stu-id="46f9e-175">queries</span></span>|<span data-ttu-id="46f9e-176">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-176">No</span></span>|<span data-ttu-id="46f9e-177">Ett objekt som representerar frågeparametrar att lägga till URL: en.</span><span class="sxs-lookup"><span data-stu-id="46f9e-177">An object representing the query parameters to add to the URL.</span></span> <span data-ttu-id="46f9e-178">Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` till URL: en.</span><span class="sxs-lookup"><span data-stu-id="46f9e-178">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|<span data-ttu-id="46f9e-179">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-179">Object</span></span>|  
|<span data-ttu-id="46f9e-180">Rubriker</span><span class="sxs-lookup"><span data-stu-id="46f9e-180">headers</span></span>|<span data-ttu-id="46f9e-181">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-181">No</span></span>|<span data-ttu-id="46f9e-182">Ett objekt som representerar alla huvuden som skickas till begäran.</span><span class="sxs-lookup"><span data-stu-id="46f9e-182">An object representing each of the headers that is sent to the request.</span></span> <span data-ttu-id="46f9e-183">Till exempel ange språk och Skriv begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="46f9e-183">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|<span data-ttu-id="46f9e-184">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-184">Object</span></span>|  
|<span data-ttu-id="46f9e-185">Brödtext</span><span class="sxs-lookup"><span data-stu-id="46f9e-185">body</span></span>|<span data-ttu-id="46f9e-186">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-186">No</span></span>|<span data-ttu-id="46f9e-187">Ett objekt som representerar nyttolasten som skickas till slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="46f9e-187">An object representing the payload that is sent to the endpoint.</span></span>|<span data-ttu-id="46f9e-188">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-188">Object</span></span>|  
|<span data-ttu-id="46f9e-189">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="46f9e-189">retryPolicy</span></span>|<span data-ttu-id="46f9e-190">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-190">No</span></span>|<span data-ttu-id="46f9e-191">Ett objekt som kan du anpassa försök beteendet för 4xx eller 5xx-fel.</span><span class="sxs-lookup"><span data-stu-id="46f9e-191">An object that lets you customize the retry behavior for 4xx or 5xx errors.</span></span>|<span data-ttu-id="46f9e-192">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-192">Object</span></span>|  
|<span data-ttu-id="46f9e-193">Autentisering</span><span class="sxs-lookup"><span data-stu-id="46f9e-193">authentication</span></span>|<span data-ttu-id="46f9e-194">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-194">No</span></span>|<span data-ttu-id="46f9e-195">Representerar den metod som ska autentisera begäran.</span><span class="sxs-lookup"><span data-stu-id="46f9e-195">Represents the method that the request should be authenticated.</span></span> <span data-ttu-id="46f9e-196">Mer information om det här objektet finns [Scheduler utgående autentisering](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span><span class="sxs-lookup"><span data-stu-id="46f9e-196">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span></span> <span data-ttu-id="46f9e-197">Utöver scheduler, är en mer stöds egenskap: `authority` det här värdet är som standard `https://login.windows.net` om anges, men du kan använda en annan publik som`https://login.windows\-ppe.net`</span><span class="sxs-lookup"><span data-stu-id="46f9e-197">Beyond scheduler, there is one more supported property: `authority` By default, this value is `https://login.windows.net` when not specified, but you can use a different audience like `https://login.windows\-ppe.net`</span></span>|<span data-ttu-id="46f9e-198">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-198">Object</span></span>|  
  
<span data-ttu-id="46f9e-199">HTTP-utlösaren kräver HTTP-API för att överensstämma med ett specifikt mönster för att fungera med din logikapp.</span><span class="sxs-lookup"><span data-stu-id="46f9e-199">The HTTP trigger requires the HTTP API to conform with a specific pattern to work well with your logic app.</span></span> <span data-ttu-id="46f9e-200">Det krävs följande fält:</span><span class="sxs-lookup"><span data-stu-id="46f9e-200">It requires the following fields:</span></span>  
  
|<span data-ttu-id="46f9e-201">Svar</span><span class="sxs-lookup"><span data-stu-id="46f9e-201">Response</span></span>|<span data-ttu-id="46f9e-202">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-202">Description</span></span>|  
|------------|---------------|  
|<span data-ttu-id="46f9e-203">Statuskod</span><span class="sxs-lookup"><span data-stu-id="46f9e-203">Status code</span></span>|<span data-ttu-id="46f9e-204">Statuskod 200 \(OK\) framkalla en körning.</span><span class="sxs-lookup"><span data-stu-id="46f9e-204">Status code 200 \(OK\) to cause a run.</span></span> <span data-ttu-id="46f9e-205">Andra statuskod göra inte en körning.</span><span class="sxs-lookup"><span data-stu-id="46f9e-205">Any other status code doesn't cause a run.</span></span>|  
|<span data-ttu-id="46f9e-206">Försök\-efter huvudet</span><span class="sxs-lookup"><span data-stu-id="46f9e-206">Retry\-after header</span></span>|<span data-ttu-id="46f9e-207">Antalet sekunder tills logikappen avsöker slutpunkten igen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-207">Number of seconds until the logic app polls the endpoint again.</span></span>|  
|<span data-ttu-id="46f9e-208">Platshuvud</span><span class="sxs-lookup"><span data-stu-id="46f9e-208">Location header</span></span>|<span data-ttu-id="46f9e-209">URL: en att anropa nästa avsökningsintervall.</span><span class="sxs-lookup"><span data-stu-id="46f9e-209">The URL to call on the next polling interval.</span></span> <span data-ttu-id="46f9e-210">Om inget anges används den ursprungliga URL: en.</span><span class="sxs-lookup"><span data-stu-id="46f9e-210">If not specified, the original URL is used.</span></span>|  
  
<span data-ttu-id="46f9e-211">Här följer några exempel på olika beteenden för olika typer av begäranden:</span><span class="sxs-lookup"><span data-stu-id="46f9e-211">Here are some examples of different behaviors for different types of requests:</span></span>  
  
|<span data-ttu-id="46f9e-212">Svarskod</span><span class="sxs-lookup"><span data-stu-id="46f9e-212">Response code</span></span>|<span data-ttu-id="46f9e-213">Försök\-efter</span><span class="sxs-lookup"><span data-stu-id="46f9e-213">Retry\-After</span></span>|<span data-ttu-id="46f9e-214">Beteende</span><span class="sxs-lookup"><span data-stu-id="46f9e-214">Behavior</span></span>|  
|-----------------|----------------|------------|  
|<span data-ttu-id="46f9e-215">200</span><span class="sxs-lookup"><span data-stu-id="46f9e-215">200</span></span>|<span data-ttu-id="46f9e-216">\(Ingen\)</span><span class="sxs-lookup"><span data-stu-id="46f9e-216">\(none\)</span></span>|<span data-ttu-id="46f9e-217">Inte en giltig utlösare, försök igen\-efter krävs, eller annan motorn aldrig avsöker för nästa begäran.</span><span class="sxs-lookup"><span data-stu-id="46f9e-217">Not a valid trigger, Retry\-After is required, or else the engine never polls for the next request.</span></span>|  
|<span data-ttu-id="46f9e-218">202</span><span class="sxs-lookup"><span data-stu-id="46f9e-218">202</span></span>|<span data-ttu-id="46f9e-219">60</span><span class="sxs-lookup"><span data-stu-id="46f9e-219">60</span></span>|<span data-ttu-id="46f9e-220">Inte Utlös arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="46f9e-220">Do not trigger the workflow.</span></span> <span data-ttu-id="46f9e-221">Nästa försök sker i en minut.</span><span class="sxs-lookup"><span data-stu-id="46f9e-221">The next attempt happens in one minute.</span></span>|  
|<span data-ttu-id="46f9e-222">200</span><span class="sxs-lookup"><span data-stu-id="46f9e-222">200</span></span>|<span data-ttu-id="46f9e-223">10</span><span class="sxs-lookup"><span data-stu-id="46f9e-223">10</span></span>|<span data-ttu-id="46f9e-224">Köra arbetsflödet och Sök igen efter mer innehåll om 10 sekunder.</span><span class="sxs-lookup"><span data-stu-id="46f9e-224">Run the workflow, and check again for more content in 10 seconds.</span></span>|  
|<span data-ttu-id="46f9e-225">400</span><span class="sxs-lookup"><span data-stu-id="46f9e-225">400</span></span>|<span data-ttu-id="46f9e-226">\(Ingen\)</span><span class="sxs-lookup"><span data-stu-id="46f9e-226">\(none\)</span></span>|<span data-ttu-id="46f9e-227">Felaktig begäran inte köra arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="46f9e-227">Bad request, do not run the workflow.</span></span> <span data-ttu-id="46f9e-228">Om det finns inga **försök princip** definierats standardprincipen används.</span><span class="sxs-lookup"><span data-stu-id="46f9e-228">If there is no **Retry Policy** defined, then the default policy is used.</span></span> <span data-ttu-id="46f9e-229">Utlösaren är inte längre giltig när antalet försök har nåtts.</span><span class="sxs-lookup"><span data-stu-id="46f9e-229">After the number of retries has been reached, the trigger is no longer valid.</span></span>|  
|<span data-ttu-id="46f9e-230">500</span><span class="sxs-lookup"><span data-stu-id="46f9e-230">500</span></span>|<span data-ttu-id="46f9e-231">\(Ingen\)</span><span class="sxs-lookup"><span data-stu-id="46f9e-231">\(none\)</span></span>|<span data-ttu-id="46f9e-232">Arbetsflödet körs inte på Server-fel.</span><span class="sxs-lookup"><span data-stu-id="46f9e-232">Server error, do not run the workflow.</span></span>  <span data-ttu-id="46f9e-233">Om det finns inga **försök princip** definierats standardprincipen används.</span><span class="sxs-lookup"><span data-stu-id="46f9e-233">If there is no **Retry Policy** defined, then the default policy is used.</span></span> <span data-ttu-id="46f9e-234">Utlösaren är inte längre giltig när antalet försök har nåtts.</span><span class="sxs-lookup"><span data-stu-id="46f9e-234">After the number of retries has been reached, the trigger is no longer valid.</span></span>|  
  
<span data-ttu-id="46f9e-235">Det ser ut som i det här exemplet utdata för en HTTP-utlösare:</span><span class="sxs-lookup"><span data-stu-id="46f9e-235">The outputs of an HTTP trigger look like this example:</span></span>  
  
|<span data-ttu-id="46f9e-236">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="46f9e-236">Element name</span></span>|<span data-ttu-id="46f9e-237">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-237">Description</span></span>|<span data-ttu-id="46f9e-238">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-238">Type</span></span>|  
|----------------|---------------|--------|  
|<span data-ttu-id="46f9e-239">Rubriker</span><span class="sxs-lookup"><span data-stu-id="46f9e-239">headers</span></span>|<span data-ttu-id="46f9e-240">Sidhuvuden för http-svaret.</span><span class="sxs-lookup"><span data-stu-id="46f9e-240">The headers of the http response.</span></span>|<span data-ttu-id="46f9e-241">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-241">Object</span></span>|  
|<span data-ttu-id="46f9e-242">Brödtext</span><span class="sxs-lookup"><span data-stu-id="46f9e-242">body</span></span>|<span data-ttu-id="46f9e-243">Brödtexten i http-svaret.</span><span class="sxs-lookup"><span data-stu-id="46f9e-243">The body of the http response.</span></span>|<span data-ttu-id="46f9e-244">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-244">Object</span></span>|  
  
## <a name="api-connection-trigger"></a><span data-ttu-id="46f9e-245">Utlösare för API-anslutningen</span><span class="sxs-lookup"><span data-stu-id="46f9e-245">API Connection trigger</span></span>  

<span data-ttu-id="46f9e-246">Utlösare för API-anslutningen är samma som HTTP-utlösaren i dess grundläggande funktioner.</span><span class="sxs-lookup"><span data-stu-id="46f9e-246">The API connection trigger is similar to the HTTP trigger in its basic functionality.</span></span> <span data-ttu-id="46f9e-247">Parametrar för att identifiera åtgärden är dock olika.</span><span class="sxs-lookup"><span data-stu-id="46f9e-247">However, the parameters for identifying the action are different.</span></span> <span data-ttu-id="46f9e-248">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="46f9e-248">Here is an example:</span></span>  
  
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

|<span data-ttu-id="46f9e-249">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="46f9e-249">Element name</span></span>|<span data-ttu-id="46f9e-250">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-250">Required</span></span>|<span data-ttu-id="46f9e-251">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-251">Type</span></span>|<span data-ttu-id="46f9e-252">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-252">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="46f9e-253">värden</span><span class="sxs-lookup"><span data-stu-id="46f9e-253">host</span></span>|<span data-ttu-id="46f9e-254">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-254">Yes</span></span>||<span data-ttu-id="46f9e-255">ApiApp finns gateway och ID: t.</span><span class="sxs-lookup"><span data-stu-id="46f9e-255">The ApiApp hosted gateway and id.</span></span>|  
|<span data-ttu-id="46f9e-256">Metoden</span><span class="sxs-lookup"><span data-stu-id="46f9e-256">method</span></span>|<span data-ttu-id="46f9e-257">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-257">Yes</span></span>|<span data-ttu-id="46f9e-258">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-258">String</span></span>|<span data-ttu-id="46f9e-259">Kan vara något av följande metoder för HTTP: **hämta**, **POST**, **PLACERA**, **ta bort**, **korrigering**, eller **HEAD**</span><span class="sxs-lookup"><span data-stu-id="46f9e-259">Can be one of the following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="46f9e-260">frågor</span><span class="sxs-lookup"><span data-stu-id="46f9e-260">queries</span></span>|<span data-ttu-id="46f9e-261">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-261">No</span></span>|<span data-ttu-id="46f9e-262">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-262">Object</span></span>|<span data-ttu-id="46f9e-263">Representerar frågeparametrar som ska läggas till URL: en.</span><span class="sxs-lookup"><span data-stu-id="46f9e-263">Represents the query parameters to be added to the URL.</span></span> <span data-ttu-id="46f9e-264">Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` till URL: en.</span><span class="sxs-lookup"><span data-stu-id="46f9e-264">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|  
|<span data-ttu-id="46f9e-265">Rubriker</span><span class="sxs-lookup"><span data-stu-id="46f9e-265">headers</span></span>|<span data-ttu-id="46f9e-266">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-266">No</span></span>|<span data-ttu-id="46f9e-267">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-267">Object</span></span>|<span data-ttu-id="46f9e-268">Representerar var och en av huvuden som skickas till begäran.</span><span class="sxs-lookup"><span data-stu-id="46f9e-268">Represents each of the headers that is sent to the request.</span></span> <span data-ttu-id="46f9e-269">Till exempel ange språk och Skriv begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="46f9e-269">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="46f9e-270">Brödtext</span><span class="sxs-lookup"><span data-stu-id="46f9e-270">body</span></span>|<span data-ttu-id="46f9e-271">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-271">No</span></span>|<span data-ttu-id="46f9e-272">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-272">Object</span></span>|<span data-ttu-id="46f9e-273">Representerar nyttolasten som skickas till slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="46f9e-273">Represents the payload that is sent to the endpoint.</span></span>|  
|<span data-ttu-id="46f9e-274">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="46f9e-274">retryPolicy</span></span>|<span data-ttu-id="46f9e-275">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-275">No</span></span>|<span data-ttu-id="46f9e-276">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-276">Object</span></span>|<span data-ttu-id="46f9e-277">Kan du anpassa försök beteendet för 4xx eller 5xx-fel.</span><span class="sxs-lookup"><span data-stu-id="46f9e-277">Allows you to customize the retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="46f9e-278">Autentisering</span><span class="sxs-lookup"><span data-stu-id="46f9e-278">authentication</span></span>|<span data-ttu-id="46f9e-279">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-279">No</span></span>|<span data-ttu-id="46f9e-280">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-280">Object</span></span>|<span data-ttu-id="46f9e-281">Representerar den metod som ska autentisera begäran.</span><span class="sxs-lookup"><span data-stu-id="46f9e-281">Represents the method that the request should be authenticated.</span></span> <span data-ttu-id="46f9e-282">Mer information om det här objektet finns [Scheduler utgående autentisering](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)</span><span class="sxs-lookup"><span data-stu-id="46f9e-282">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)</span></span>|  
  
<span data-ttu-id="46f9e-283">Egenskaper för värden är:</span><span class="sxs-lookup"><span data-stu-id="46f9e-283">The properties for host are:</span></span>  
  
|<span data-ttu-id="46f9e-284">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="46f9e-284">Element name</span></span>|<span data-ttu-id="46f9e-285">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-285">Required</span></span>|<span data-ttu-id="46f9e-286">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-286">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="46f9e-287">API-runtimeUrl</span><span class="sxs-lookup"><span data-stu-id="46f9e-287">api runtimeUrl</span></span>|<span data-ttu-id="46f9e-288">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-288">Yes</span></span>|<span data-ttu-id="46f9e-289">Hanterade API-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="46f9e-289">The endpoint of the managed API.</span></span>|  
|<span data-ttu-id="46f9e-290">Anslutningens namn</span><span class="sxs-lookup"><span data-stu-id="46f9e-290">connection name</span></span>||<span data-ttu-id="46f9e-291">Måste vara en referens till en parameter med namnet `$connection` och är namnet på den hanterade API-anslutningen som används i arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="46f9e-291">Must be a reference to a parameter called `$connection` and is the name of the managed API connection that the workflow uses.</span></span>|
  
<span data-ttu-id="46f9e-292">Utdata för en utlösare för API-anslutning är:</span><span class="sxs-lookup"><span data-stu-id="46f9e-292">The outputs of an API connection trigger are:</span></span>
  
|<span data-ttu-id="46f9e-293">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="46f9e-293">Element name</span></span>|<span data-ttu-id="46f9e-294">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-294">Type</span></span>|<span data-ttu-id="46f9e-295">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-295">Description</span></span>|  
|----------------|--------|---------------|  
|<span data-ttu-id="46f9e-296">Rubriker</span><span class="sxs-lookup"><span data-stu-id="46f9e-296">headers</span></span>|<span data-ttu-id="46f9e-297">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-297">Object</span></span>|<span data-ttu-id="46f9e-298">Sidhuvuden för http-svaret.</span><span class="sxs-lookup"><span data-stu-id="46f9e-298">The headers of the http response.</span></span>|  
|<span data-ttu-id="46f9e-299">Brödtext</span><span class="sxs-lookup"><span data-stu-id="46f9e-299">body</span></span>|<span data-ttu-id="46f9e-300">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-300">Object</span></span>|<span data-ttu-id="46f9e-301">Brödtexten i http-svaret.</span><span class="sxs-lookup"><span data-stu-id="46f9e-301">The body of the http response.</span></span>|  
  
## <a name="httpwebhook-trigger"></a><span data-ttu-id="46f9e-302">HTTPWebhook utlösare</span><span class="sxs-lookup"><span data-stu-id="46f9e-302">HTTPWebhook trigger</span></span>  

<span data-ttu-id="46f9e-303">Utlösaren HTTPWebhook öppnar en slutpunkt som liknar den manuella utlösaren men HTTPWebhook utlösaren anropar också till en angiven URL för att registrera och avregistrera.</span><span class="sxs-lookup"><span data-stu-id="46f9e-303">The HTTPWebhook trigger opens an endpoint, similar to the manual trigger, but the HTTPWebhook trigger also calls out to a specified URL to register and unregister.</span></span> <span data-ttu-id="46f9e-304">Här är ett exempel på hur en HTTPWebhook-utlösare kan se ut:</span><span class="sxs-lookup"><span data-stu-id="46f9e-304">Here's an example of what an HTTPWebhook trigger might look like:</span></span>  

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

<span data-ttu-id="46f9e-305">Många av dessa avsnitt är valfria och Webhooken beror på vilka avsnitt som eller utelämnas.</span><span class="sxs-lookup"><span data-stu-id="46f9e-305">Many of these sections are optional, and the behavior of the Webhook depends on which sections are provided or omitted.</span></span>  
<span data-ttu-id="46f9e-306">Egenskaperna för en Webhook är följande:</span><span class="sxs-lookup"><span data-stu-id="46f9e-306">The properties of a Webhook are as follows:</span></span>  
  
|<span data-ttu-id="46f9e-307">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="46f9e-307">Element name</span></span>|<span data-ttu-id="46f9e-308">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-308">Required</span></span>|<span data-ttu-id="46f9e-309">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-309">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="46f9e-310">prenumerera på</span><span class="sxs-lookup"><span data-stu-id="46f9e-310">subscribe</span></span>|<span data-ttu-id="46f9e-311">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-311">No</span></span>|<span data-ttu-id="46f9e-312">Utgående begäran som anropas när utlösaren skapas och utför registreringen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-312">The outgoing request that is called when the trigger is created and performs the initial registration.</span></span>|  
|<span data-ttu-id="46f9e-313">avbryta prenumerationen</span><span class="sxs-lookup"><span data-stu-id="46f9e-313">unsubscribe</span></span>|<span data-ttu-id="46f9e-314">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-314">No</span></span>|<span data-ttu-id="46f9e-315">Utgående begäran när utlösaren tas bort.</span><span class="sxs-lookup"><span data-stu-id="46f9e-315">The outgoing request when the trigger is deleted.</span></span>|  
  
-   <span data-ttu-id="46f9e-316">**Prenumerera på** är utgående anrop som har gjorts att starta lyssnar på händelser.</span><span class="sxs-lookup"><span data-stu-id="46f9e-316">**Subscribe** is the outgoing call that's made to start listening to events.</span></span> <span data-ttu-id="46f9e-317">Det här anropet börjar med samma uppsättning parametrar som gör de vanliga HTTP-åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="46f9e-317">This call starts with the same set of parameters that the normal HTTP actions do.</span></span> <span data-ttu-id="46f9e-318">Utgående anropet görs någon gång arbetsflödet ändras på något sätt, till exempel när autentiseringsuppgifterna samlas eller ändra utlösarens indataparametrar.</span><span class="sxs-lookup"><span data-stu-id="46f9e-318">This outgoing call is made any time the workflow changes in any way, for example, whenever the credentials are rolled, or the trigger's input parameters change.</span></span>
  
    <span data-ttu-id="46f9e-319">För att stödja det här anropet är en ny funktion: `@listCallbackUrl()`.</span><span class="sxs-lookup"><span data-stu-id="46f9e-319">To support this call, there is a new function: `@listCallbackUrl()`.</span></span> <span data-ttu-id="46f9e-320">Den här funktionen returnerar en unik URL för den här specifika utlösaren i det här arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="46f9e-320">This function returns a unique URL for this specific trigger in this workflow.</span></span> <span data-ttu-id="46f9e-321">Den motsvarar den unika identifieraren för slutpunkter som använder Service REST.</span><span class="sxs-lookup"><span data-stu-id="46f9e-321">It represents the unique identifier for the endpoints that use the Service REST.</span></span>  
  
-   <span data-ttu-id="46f9e-322">**Avbryta prenumerationen** anropas när en åtgärd återgivningar utlösaren är ogiltig, inklusive:</span><span class="sxs-lookup"><span data-stu-id="46f9e-322">**Unsubscribe** is called when an operation renders this trigger invalid, including:</span></span>  
  
    -   <span data-ttu-id="46f9e-323">Ta bort eller inaktivera utlösaren</span><span class="sxs-lookup"><span data-stu-id="46f9e-323">Deleting or disabling the trigger</span></span>  
  
    -   <span data-ttu-id="46f9e-324">Ta bort eller inaktivera arbetsflödet</span><span class="sxs-lookup"><span data-stu-id="46f9e-324">Deleting or disabling the workflow</span></span>  
  
    -   <span data-ttu-id="46f9e-325">Ta bort eller inaktivera prenumerationen</span><span class="sxs-lookup"><span data-stu-id="46f9e-325">Deleting or disabling the subscription</span></span>  
  
    <span data-ttu-id="46f9e-326">Åtgärden Avbryt prenumeration anropar automatiskt i logikappen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-326">The logic app automatically calls the unsubscribe action.</span></span> <span data-ttu-id="46f9e-327">Parametrarna för den här funktionen är samma som HTTP-utlösaren.</span><span class="sxs-lookup"><span data-stu-id="46f9e-327">The parameters to this function are the same as the HTTP trigger.</span></span>  
  
    <span data-ttu-id="46f9e-328">Utdata för HTTPWebhook utlösaren är innehållet i den inkommande begäranden:</span><span class="sxs-lookup"><span data-stu-id="46f9e-328">The outputs of the HTTPWebhook trigger are the contents of the incoming request:</span></span>  
  
|<span data-ttu-id="46f9e-329">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="46f9e-329">Element name</span></span>|<span data-ttu-id="46f9e-330">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-330">Type</span></span>|<span data-ttu-id="46f9e-331">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-331">Description</span></span>|  
|-----------------|--------|---------------|  
|<span data-ttu-id="46f9e-332">Rubriker</span><span class="sxs-lookup"><span data-stu-id="46f9e-332">headers</span></span>|<span data-ttu-id="46f9e-333">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-333">Object</span></span>|<span data-ttu-id="46f9e-334">Sidhuvuden för http-begäran.</span><span class="sxs-lookup"><span data-stu-id="46f9e-334">The headers of the http request.</span></span>|  
|<span data-ttu-id="46f9e-335">Brödtext</span><span class="sxs-lookup"><span data-stu-id="46f9e-335">body</span></span>|<span data-ttu-id="46f9e-336">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-336">Object</span></span>|<span data-ttu-id="46f9e-337">Brödtexten i http-begäran.</span><span class="sxs-lookup"><span data-stu-id="46f9e-337">The body of the http request.</span></span>|  

<span data-ttu-id="46f9e-338">Gränserna för ett webhook-åtgärder kan anges på samma sätt som [HTTP asynkron gränser](#asynchronous-limits).</span><span class="sxs-lookup"><span data-stu-id="46f9e-338">Limits on a webhook action can be specified in the same manner as [HTTP Asynchronous Limits](#asynchronous-limits).</span></span>
  

## <a name="conditions"></a><span data-ttu-id="46f9e-339">Villkor</span><span class="sxs-lookup"><span data-stu-id="46f9e-339">Conditions</span></span>  

<span data-ttu-id="46f9e-340">Du kan använda ett eller flera villkor för alla utlösare för att avgöra om arbetsflödet ska köras eller inte.</span><span class="sxs-lookup"><span data-stu-id="46f9e-340">For any trigger, you can use one or more conditions to determine whether the workflow should run or not.</span></span> <span data-ttu-id="46f9e-341">Exempel:</span><span class="sxs-lookup"><span data-stu-id="46f9e-341">For example:</span></span>  

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

<span data-ttu-id="46f9e-342">I det här fallet rapporten endast utlösare när arbetsflödets `sendReports` parameter är angiven till true.</span><span class="sxs-lookup"><span data-stu-id="46f9e-342">In this case, the report only triggers while the workflow's `sendReports` parameter is set to true.</span></span> <span data-ttu-id="46f9e-343">Slutligen får villkor referera statuskod för utlösaren.</span><span class="sxs-lookup"><span data-stu-id="46f9e-343">Finally, conditions may reference the status code of the trigger.</span></span> <span data-ttu-id="46f9e-344">Exempelvis kan startar du ett arbetsflöde endast när din webbplats returnerar en statuskod 500, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="46f9e-344">For example, you could kick off a workflow only when your website returns a status code 500, as follows:</span></span>
  
```  
"conditions": [  
        {  
          "expression": "@equals(triggers().code, 'InternalServerError')"  
        }  
      ]  
```  
  
> [!NOTE]  
> <span data-ttu-id="46f9e-345">När ett uttryck som refererar till statuskod för utlösaren \(på något sätt\), standardbeteendet \(utlösaren endast på 200 \(OK\) \) ersätts.</span><span class="sxs-lookup"><span data-stu-id="46f9e-345">When any expression references the status code of the trigger \(in any way\), the default behavior \(trigger only on 200 \(OK\)\) is replaced.</span></span> <span data-ttu-id="46f9e-346">Till exempel om du vill aktivera både statuskod 200 och statuskod 201 du behöver ta: `@or(equals(triggers().code, 200),equals(triggers().code,201))` som dina villkor.</span><span class="sxs-lookup"><span data-stu-id="46f9e-346">For example, if you want to trigger on both status code 200 and status code 201, you have to include: `@or(equals(triggers().code, 200),equals(triggers().code,201))` as your condition.</span></span>  
  
## <a name="start-multiple-runs-for-a-request"></a><span data-ttu-id="46f9e-347">Starta flera körs för en begäran</span><span class="sxs-lookup"><span data-stu-id="46f9e-347">Start multiple runs for a request</span></span>

<span data-ttu-id="46f9e-348">Att startar flera körningar för en enskild begäran `splitOn` är användbart, till exempel när du vill söka en slutpunkt som kan ha flera nya objekt mellan avsökningsintervall.</span><span class="sxs-lookup"><span data-stu-id="46f9e-348">To kick off multiple runs for a single request, `splitOn` is useful, for example, when you want to poll an endpoint that can have multiple new items between polling intervals.</span></span>
  
<span data-ttu-id="46f9e-349">Med `splitOn`, du anger egenskapen i nyttolasten av svar som innehåller ett antal objekt som du vill använda för att starta en körning av utlösaren.</span><span class="sxs-lookup"><span data-stu-id="46f9e-349">With `splitOn`, you specify the property inside the response payload that contains the array of items, each of which you want to use to start a run of the trigger.</span></span> <span data-ttu-id="46f9e-350">Anta att du har en API som returnerar följande svar:</span><span class="sxs-lookup"><span data-stu-id="46f9e-350">For example, imagine you have an API that returns the following response:</span></span>  
  
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
  
<span data-ttu-id="46f9e-351">Din logikapp kan du bara behöver rader innehåll, så du kan skapa en utlösare som det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="46f9e-351">Your logic app only needs the Rows content, so you can construct your trigger like this example:</span></span>  
  
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
  
<span data-ttu-id="46f9e-352">I arbetsflödesdefinitionen, `@triggerBody().name` returnerar `mycoolrow` för den första körningen och `another row` för andra kör.</span><span class="sxs-lookup"><span data-stu-id="46f9e-352">Then, in the workflow definition, `@triggerBody().name` returns `mycoolrow` for the first run, and `another row` for the second run.</span></span> <span data-ttu-id="46f9e-353">Utlösaren utdata ser ut det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="46f9e-353">The trigger outputs look like this example:</span></span>  
  
```json
{
    "body" : {
        "id" : 938109381,
        "name" : "another row"
    }
}
```

<span data-ttu-id="46f9e-354">Om du använder `SplitOn`, du kan inte hämta de egenskaper som är utanför matrisen, i det här fallet den `Status` fältet.</span><span class="sxs-lookup"><span data-stu-id="46f9e-354">So if you use `SplitOn`, you can't get the properties that are outside the array, in this case, the `Status` field.</span></span>  
  
> [!NOTE]  
> <span data-ttu-id="46f9e-355">I det här exemplet använder vi den `?` operatorn för att undvika ett fel om den `Rows` egenskapen finns inte.</span><span class="sxs-lookup"><span data-stu-id="46f9e-355">In this example, we use the `?` operator to be able to avoid a failure if the `Rows` property is not present.</span></span> 
  
## <a name="single-run-instance"></a><span data-ttu-id="46f9e-356">Kör instans</span><span class="sxs-lookup"><span data-stu-id="46f9e-356">Single run instance</span></span>

<span data-ttu-id="46f9e-357">Du kan konfigurera utlösare som har en upprepning egenskap eller bara om alla aktiva körningar har slutförts.</span><span class="sxs-lookup"><span data-stu-id="46f9e-357">You can configure triggers that have a recurrence property to only fire if all active runs have completed.</span></span> <span data-ttu-id="46f9e-358">Om en schemalagd upprepning inträffar när det finns en pågående kör utlösaren hoppar över och väntar tills nästa schemalagda Upprepningsintervall söka igen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-358">If a scheduled recurrence occurs while there is an in-progress run, the trigger skips and waits until the next scheduled recurrence interval to check again.</span></span>

<span data-ttu-id="46f9e-359">Du kan konfigurera den här inställningen i alternativen för åtgärden:</span><span class="sxs-lookup"><span data-stu-id="46f9e-359">You can configure this setting through the operation options:</span></span>

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

## <a name="types-and-inputs"></a><span data-ttu-id="46f9e-360">Typer och indata</span><span class="sxs-lookup"><span data-stu-id="46f9e-360">Types and inputs</span></span>  

<span data-ttu-id="46f9e-361">Det finns många typer av åtgärder med unika beteende.</span><span class="sxs-lookup"><span data-stu-id="46f9e-361">There are many types of actions, each with unique behavior.</span></span> <span data-ttu-id="46f9e-362">Åtgärder för samlingen kan innehålla flera andra åtgärder i sig själv.</span><span class="sxs-lookup"><span data-stu-id="46f9e-362">Collection actions may contain many other actions within itself.</span></span>

### <a name="standard-actions"></a><span data-ttu-id="46f9e-363">Standardåtgärder</span><span class="sxs-lookup"><span data-stu-id="46f9e-363">Standard actions</span></span>  

-   <span data-ttu-id="46f9e-364">**HTTP** den här åtgärden kräver en HTTP-slutpunkt för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="46f9e-364">**HTTP** This action calls an HTTP web endpoint.</span></span>  
  
-   <span data-ttu-id="46f9e-365">**ApiConnection** \- åtgärden fungerar som HTTP-åtgärden, men använder Microsoft-hanterade API: erna.</span><span class="sxs-lookup"><span data-stu-id="46f9e-365">**ApiConnection** \- This action behaves like the HTTP action, but uses the Microsoft-managed APIs.</span></span>  
  
-   <span data-ttu-id="46f9e-366">**ApiConnectionWebhook** \- som HTTPWebhook men använder Microsoft-hanterade API: erna.</span><span class="sxs-lookup"><span data-stu-id="46f9e-366">**ApiConnectionWebhook** \- Like HTTPWebhook, but uses the Microsoft-managed APIs.</span></span>  
  
-   <span data-ttu-id="46f9e-367">**Svaret** \- åtgärden definierar ett svar för ett inkommande samtal.</span><span class="sxs-lookup"><span data-stu-id="46f9e-367">**Response** \- This action defines a response for an incoming call.</span></span>  
  
-   <span data-ttu-id="46f9e-368">**Vänta** \- åtgärden enkel väntar på ett fast belopp tid eller tills en viss tid.</span><span class="sxs-lookup"><span data-stu-id="46f9e-368">**Wait** \- This simple action waits a fixed amount of time or until a specific time.</span></span>  
  
-   <span data-ttu-id="46f9e-369">**Arbetsflödet** \- representerar ett inkapslat arbetsflöde för den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="46f9e-369">**Workflow** \- This action represents a nested workflow.</span></span>  

-   <span data-ttu-id="46f9e-370">**Funktionen** \- åtgärden representerar en Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="46f9e-370">**Function** \- This action represents an Azure Function.</span></span>

### <a name="collection-actions"></a><span data-ttu-id="46f9e-371">Åtgärder för samlingen</span><span class="sxs-lookup"><span data-stu-id="46f9e-371">Collection actions</span></span>

-   <span data-ttu-id="46f9e-372">**Scope** \- den här åtgärden är en logisk gruppering av andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="46f9e-372">**Scope** \- This action is a logical grouping of other actions.</span></span>

-   <span data-ttu-id="46f9e-373">**Villkoret** \- åtgärden utvärderar ett uttryck och kör grenen motsvarande resultat.</span><span class="sxs-lookup"><span data-stu-id="46f9e-373">**Condition** \- This action evaluates an expression and executes the corresponding result branch.</span></span>

-   <span data-ttu-id="46f9e-374">**ForEach** \- slingor åtgärden upprepas i en matris och utför inre åtgärder för varje objekt.</span><span class="sxs-lookup"><span data-stu-id="46f9e-374">**ForEach** \- This looping action iterates through an array and performs inner actions for each item.</span></span>

-   <span data-ttu-id="46f9e-375">**Tills** \- åtgärden slingor kör inre åtgärder tills ett villkor resultatet till true.</span><span class="sxs-lookup"><span data-stu-id="46f9e-375">**Until** \- This looping action executes inner actions until a condition results to true.</span></span>
  
<span data-ttu-id="46f9e-376">Varje typ av åtgärd har en annan uppsättning **indata** som definierar beteendet för en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="46f9e-376">Each type of action has a different set of **inputs** that define an action's behavior.</span></span>  
  
## <a name="http-action"></a><span data-ttu-id="46f9e-377">HTTP-åtgärd</span><span class="sxs-lookup"><span data-stu-id="46f9e-377">HTTP action</span></span>  

<span data-ttu-id="46f9e-378">HTTP-åtgärder anropa den angivna slutpunkten och kontrollera svaret för att avgöra om arbetsflödet ska köras.</span><span class="sxs-lookup"><span data-stu-id="46f9e-378">HTTP actions call a specified endpoint and check the response to determine whether the workflow should run.</span></span> <span data-ttu-id="46f9e-379">Den **indata** objektet tar uppsättningen parametrar som krävs för att konstruera HTTP-anropet:</span><span class="sxs-lookup"><span data-stu-id="46f9e-379">The **inputs** object takes the set of parameters required to construct the HTTP call:</span></span>  
  
|<span data-ttu-id="46f9e-380">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="46f9e-380">Element name</span></span>|<span data-ttu-id="46f9e-381">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-381">Required</span></span>|<span data-ttu-id="46f9e-382">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-382">Type</span></span>|<span data-ttu-id="46f9e-383">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-383">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="46f9e-384">Metoden</span><span class="sxs-lookup"><span data-stu-id="46f9e-384">method</span></span>|<span data-ttu-id="46f9e-385">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-385">Yes</span></span>|<span data-ttu-id="46f9e-386">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-386">String</span></span>|<span data-ttu-id="46f9e-387">Kan vara något av följande metoder för HTTP: **hämta**, **POST**, **PLACERA**, **ta bort**, **korrigering**, eller **HEAD**</span><span class="sxs-lookup"><span data-stu-id="46f9e-387">Can be one of the following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="46f9e-388">URI: N</span><span class="sxs-lookup"><span data-stu-id="46f9e-388">uri</span></span>|<span data-ttu-id="46f9e-389">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-389">Yes</span></span>|<span data-ttu-id="46f9e-390">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-390">String</span></span>|<span data-ttu-id="46f9e-391">Http eller https slutpunkten som anropas.</span><span class="sxs-lookup"><span data-stu-id="46f9e-391">The http or https endpoint that is called.</span></span> <span data-ttu-id="46f9e-392">Maximal längd är 2 kB.</span><span class="sxs-lookup"><span data-stu-id="46f9e-392">Maximum length is 2 kilobytes.</span></span>|  
|<span data-ttu-id="46f9e-393">frågor</span><span class="sxs-lookup"><span data-stu-id="46f9e-393">queries</span></span>|<span data-ttu-id="46f9e-394">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-394">No</span></span>|<span data-ttu-id="46f9e-395">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-395">Object</span></span>|<span data-ttu-id="46f9e-396">Representerar frågeparametrar att lägga till URL: en.</span><span class="sxs-lookup"><span data-stu-id="46f9e-396">Represents the query parameters to add to the URL.</span></span> <span data-ttu-id="46f9e-397">Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` till URL: en.</span><span class="sxs-lookup"><span data-stu-id="46f9e-397">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|  
|<span data-ttu-id="46f9e-398">Rubriker</span><span class="sxs-lookup"><span data-stu-id="46f9e-398">headers</span></span>|<span data-ttu-id="46f9e-399">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-399">No</span></span>|<span data-ttu-id="46f9e-400">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-400">Object</span></span>|<span data-ttu-id="46f9e-401">Representerar var och en av huvuden som skickas till begäran.</span><span class="sxs-lookup"><span data-stu-id="46f9e-401">Represents each of the headers that is sent to the request.</span></span> <span data-ttu-id="46f9e-402">Till exempel ange språk och Skriv begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="46f9e-402">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="46f9e-403">Brödtext</span><span class="sxs-lookup"><span data-stu-id="46f9e-403">body</span></span>|<span data-ttu-id="46f9e-404">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-404">No</span></span>|<span data-ttu-id="46f9e-405">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-405">Object</span></span>|<span data-ttu-id="46f9e-406">Representerar nyttolasten som skickas till slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="46f9e-406">Represents the payload that is sent to the endpoint.</span></span>|  
|<span data-ttu-id="46f9e-407">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="46f9e-407">retryPolicy</span></span>|<span data-ttu-id="46f9e-408">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-408">No</span></span>|<span data-ttu-id="46f9e-409">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-409">Object</span></span>|<span data-ttu-id="46f9e-410">Kan du anpassa försök beteendet för 4xx eller 5xx-fel.</span><span class="sxs-lookup"><span data-stu-id="46f9e-410">Lets you customize the retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="46f9e-411">operationsOptions</span><span class="sxs-lookup"><span data-stu-id="46f9e-411">operationsOptions</span></span>|<span data-ttu-id="46f9e-412">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-412">No</span></span>|<span data-ttu-id="46f9e-413">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-413">String</span></span>|<span data-ttu-id="46f9e-414">Definierar de särskilda beteenden att åsidosätta.</span><span class="sxs-lookup"><span data-stu-id="46f9e-414">Defines the set of special behaviors to override.</span></span>|  
|<span data-ttu-id="46f9e-415">Autentisering</span><span class="sxs-lookup"><span data-stu-id="46f9e-415">authentication</span></span>|<span data-ttu-id="46f9e-416">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-416">No</span></span>|<span data-ttu-id="46f9e-417">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-417">Object</span></span>|<span data-ttu-id="46f9e-418">Representerar den metod som ska autentisera begäran.</span><span class="sxs-lookup"><span data-stu-id="46f9e-418">Represents the method that the request should be authenticated.</span></span> <span data-ttu-id="46f9e-419">Mer information om det här objektet finns [Scheduler utgående autentisering](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span><span class="sxs-lookup"><span data-stu-id="46f9e-419">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span></span> <span data-ttu-id="46f9e-420">Utöver scheduler, är en mer stöds egenskap: `authority`.</span><span class="sxs-lookup"><span data-stu-id="46f9e-420">Beyond scheduler, there is one more supported property: `authority`.</span></span> <span data-ttu-id="46f9e-421">Detta är som standard `https://login.windows.net` om anges, men du kan använda en annan publik som`https://login.windows\-ppe.net`</span><span class="sxs-lookup"><span data-stu-id="46f9e-421">By default, this is `https://login.windows.net` when not specified, but you can use a different audience like `https://login.windows\-ppe.net`</span></span>|  
  
<span data-ttu-id="46f9e-422">HTTP-åtgärder \(och API-anslutningen\) åtgärder stöd försök principer.</span><span class="sxs-lookup"><span data-stu-id="46f9e-422">HTTP actions \(and API Connection\) actions support retry policies.</span></span> <span data-ttu-id="46f9e-423">En återförsöksprincip som gäller för återkommande fel som betecknas som HTTP-statuskoder 408 och 429 5xx utöver eventuella undantag.</span><span class="sxs-lookup"><span data-stu-id="46f9e-423">A retry policy applies to intermittent failures, characterized as HTTP status codes 408, 429, and 5xx, in addition to any connectivity exceptions.</span></span> <span data-ttu-id="46f9e-424">Den här principen nedan används den *retryPolicy* objekt som är definierat som visas här:</span><span class="sxs-lookup"><span data-stu-id="46f9e-424">This policy is described using the *retryPolicy* object defined as shown here:</span></span>
  
```json
"retryPolicy" : {
    "type": "<type-of-retry-policy>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```
  
<span data-ttu-id="46f9e-425">Intervallet anges i ISO 8601-format.</span><span class="sxs-lookup"><span data-stu-id="46f9e-425">The retry interval is specified in the ISO 8601 format.</span></span> <span data-ttu-id="46f9e-426">Det här intervallet har standard och minst 20 sekunder medan det maximala värdet är en timme.</span><span class="sxs-lookup"><span data-stu-id="46f9e-426">This interval has a default and minimum value of 20 seconds, while the maximum value is one hour.</span></span> <span data-ttu-id="46f9e-427">Standard- och maximalt antal för nya försök är fyra timmar.</span><span class="sxs-lookup"><span data-stu-id="46f9e-427">The default and maximum retry count is four hours.</span></span> <span data-ttu-id="46f9e-428">Om principdefinitionen försök anges en `fixed` strategi som används med försök antal och intervall standardvärden.</span><span class="sxs-lookup"><span data-stu-id="46f9e-428">If the retry policy definition is not specified, a `fixed` strategy is used with default retry count and interval values.</span></span> <span data-ttu-id="46f9e-429">Om du vill inaktivera återförsöksprincipen sägs dess typ `None`.</span><span class="sxs-lookup"><span data-stu-id="46f9e-429">To disable the retry policy, set its type to `None`.</span></span>  
  
<span data-ttu-id="46f9e-430">Till exempel i följande nya försök hämta de senaste nyheterna två gånger om återkommande fel totalt tre körningar med en fördröjning på 30 sekunder mellan varje försök:</span><span class="sxs-lookup"><span data-stu-id="46f9e-430">For example, the following action retries fetching the latest news two times, if there are intermittent failures, for a total of three executions, with a 30-second delay between each attempt:</span></span>  
  
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
### <a name="asynchronous-patterns"></a><span data-ttu-id="46f9e-431">Asynkront mönster</span><span class="sxs-lookup"><span data-stu-id="46f9e-431">Asynchronous patterns</span></span>

<span data-ttu-id="46f9e-432">Som standard stöder alla HTTP-baserade åtgärder mönstret standard asynkron åtgärd.</span><span class="sxs-lookup"><span data-stu-id="46f9e-432">By default, all HTTP-based actions support the standard asynchronous operation pattern.</span></span> <span data-ttu-id="46f9e-433">Om fjärrservern anger att begäran är godkänd för behandling med en 202 \(godkända\) svar, Logic Apps motorn håller avsöker den URL som anges i svarshuvud plats tills ett avslutat tillstånd \(inte\-202 svar\).</span><span class="sxs-lookup"><span data-stu-id="46f9e-433">So if the remote server indicates that the request is accepted for processing with a 202 \(Accepted\) response, the Logic Apps engine keeps polling the URL specified in the response's location header until reaching a terminal state \(a non\-202 response\).</span></span>  
  
<span data-ttu-id="46f9e-434">Om du vill inaktivera beteendet asynkron som beskrevs tidigare, ange en `DisableAsyncPattern` alternativ på åtgärd-indata.</span><span class="sxs-lookup"><span data-stu-id="46f9e-434">To disable the asynchronous behavior previously described, set a `DisableAsyncPattern` option in the action inputs.</span></span> <span data-ttu-id="46f9e-435">I det här fallet är resultatet av åtgärden baserad på det första 202 svaret från servern.</span><span class="sxs-lookup"><span data-stu-id="46f9e-435">In this case, the output of the action is based on the initial 202 response from the server.</span></span>  
  
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

#### <a name="asynchronous-limits"></a><span data-ttu-id="46f9e-436">Asynkron gränser</span><span class="sxs-lookup"><span data-stu-id="46f9e-436">Asynchronous Limits</span></span>

<span data-ttu-id="46f9e-437">Du kan begränsa ett asynkront mönster varaktigheten för ett visst tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="46f9e-437">An asynchronous pattern can be limited in its duration to a specific time interval.</span></span>  <span data-ttu-id="46f9e-438">Om tidsintervallet långa utan att ett avslutat tillstånd åtgärdens status kommer att markeras `Cancelled` med koden `ActionTimedOut`.</span><span class="sxs-lookup"><span data-stu-id="46f9e-438">If the time interval elapses without reaching a terminal state, the status of the action will be marked `Cancelled` with a code of `ActionTimedOut`.</span></span>  <span data-ttu-id="46f9e-439">Begränsa tidsgränsen har angetts i ISO 8601-format.</span><span class="sxs-lookup"><span data-stu-id="46f9e-439">The limit timeout is specified in ISO 8601 format.</span></span>  <span data-ttu-id="46f9e-440">Gränser kan anges med följande syntax:</span><span class="sxs-lookup"><span data-stu-id="46f9e-440">Limits can be specified with the following syntax:</span></span>

``` json
"<action-name>": {
    "type": "workflow|webhook|http|apiconnectionwebhook|apiconnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="api-connection"></a><span data-ttu-id="46f9e-441">API-anslutningen</span><span class="sxs-lookup"><span data-stu-id="46f9e-441">API Connection</span></span>  

<span data-ttu-id="46f9e-442">API-anslutningen är en åtgärd som refererar till en Microsoft-hanterad koppling.</span><span class="sxs-lookup"><span data-stu-id="46f9e-442">API Connection is an action that references a Microsoft-managed connector.</span></span>
<span data-ttu-id="46f9e-443">Den här åtgärden kräver en referens till en giltig anslutning och information om API och parametrar som krävs.</span><span class="sxs-lookup"><span data-stu-id="46f9e-443">This action requires a reference to a valid connection, and information on the API and parameters required.</span></span>

|<span data-ttu-id="46f9e-444">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="46f9e-444">Element name</span></span>|<span data-ttu-id="46f9e-445">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-445">Required</span></span>|<span data-ttu-id="46f9e-446">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-446">Type</span></span>|<span data-ttu-id="46f9e-447">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-447">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="46f9e-448">värden</span><span class="sxs-lookup"><span data-stu-id="46f9e-448">host</span></span>|<span data-ttu-id="46f9e-449">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-449">Yes</span></span>|<span data-ttu-id="46f9e-450">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-450">Object</span></span>|<span data-ttu-id="46f9e-451">Representerar connector-information, till exempel runtimeUrl och referens till connection-objektet</span><span class="sxs-lookup"><span data-stu-id="46f9e-451">Represents the connector information such as the runtimeUrl and reference to the connection object</span></span>|
|<span data-ttu-id="46f9e-452">Metoden</span><span class="sxs-lookup"><span data-stu-id="46f9e-452">method</span></span>|<span data-ttu-id="46f9e-453">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-453">Yes</span></span>|<span data-ttu-id="46f9e-454">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-454">String</span></span>|<span data-ttu-id="46f9e-455">Kan vara något av följande metoder för HTTP: **hämta**, **POST**, **PLACERA**, **ta bort**, **korrigering**, eller **HEAD**</span><span class="sxs-lookup"><span data-stu-id="46f9e-455">Can be one of the following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="46f9e-456">Sökväg</span><span class="sxs-lookup"><span data-stu-id="46f9e-456">path</span></span>|<span data-ttu-id="46f9e-457">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-457">Yes</span></span>|<span data-ttu-id="46f9e-458">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-458">String</span></span>|<span data-ttu-id="46f9e-459">Sökvägen till API-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="46f9e-459">The path of the API operation.</span></span>|  
|<span data-ttu-id="46f9e-460">frågor</span><span class="sxs-lookup"><span data-stu-id="46f9e-460">queries</span></span>|<span data-ttu-id="46f9e-461">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-461">No</span></span>|<span data-ttu-id="46f9e-462">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-462">Object</span></span>|<span data-ttu-id="46f9e-463">Representerar frågeparametrar att lägga till URL: en.</span><span class="sxs-lookup"><span data-stu-id="46f9e-463">Represents the query parameters to add to the URL.</span></span> <span data-ttu-id="46f9e-464">Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` till URL: en.</span><span class="sxs-lookup"><span data-stu-id="46f9e-464">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|  
|<span data-ttu-id="46f9e-465">Rubriker</span><span class="sxs-lookup"><span data-stu-id="46f9e-465">headers</span></span>|<span data-ttu-id="46f9e-466">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-466">No</span></span>|<span data-ttu-id="46f9e-467">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-467">Object</span></span>|<span data-ttu-id="46f9e-468">Representerar var och en av huvuden som skickas till begäran.</span><span class="sxs-lookup"><span data-stu-id="46f9e-468">Represents each of the headers that is sent to the request.</span></span> <span data-ttu-id="46f9e-469">Till exempel ange språk och Skriv begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="46f9e-469">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="46f9e-470">Brödtext</span><span class="sxs-lookup"><span data-stu-id="46f9e-470">body</span></span>|<span data-ttu-id="46f9e-471">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-471">No</span></span>|<span data-ttu-id="46f9e-472">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-472">Object</span></span>|<span data-ttu-id="46f9e-473">Representerar nyttolasten som skickas till slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="46f9e-473">Represents the payload that is sent to the endpoint.</span></span>|  
|<span data-ttu-id="46f9e-474">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="46f9e-474">retryPolicy</span></span>|<span data-ttu-id="46f9e-475">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-475">No</span></span>|<span data-ttu-id="46f9e-476">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-476">Object</span></span>|<span data-ttu-id="46f9e-477">Kan du anpassa försök beteendet för 4xx eller 5xx-fel.</span><span class="sxs-lookup"><span data-stu-id="46f9e-477">Lets you customize the retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="46f9e-478">operationsOptions</span><span class="sxs-lookup"><span data-stu-id="46f9e-478">operationsOptions</span></span>|<span data-ttu-id="46f9e-479">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-479">No</span></span>|<span data-ttu-id="46f9e-480">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-480">String</span></span>|<span data-ttu-id="46f9e-481">Definierar de särskilda beteenden att åsidosätta.</span><span class="sxs-lookup"><span data-stu-id="46f9e-481">Defines the set of special behaviors to override.</span></span>|  

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

## <a name="api-connection-webhook-action"></a><span data-ttu-id="46f9e-482">API-anslutningen webhook åtgärd</span><span class="sxs-lookup"><span data-stu-id="46f9e-482">API Connection webhook action</span></span>

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

<span data-ttu-id="46f9e-483">Gränserna för ett webhook-åtgärder kan anges på samma sätt som [HTTP asynkron gränser](#asynchronous-limits).</span><span class="sxs-lookup"><span data-stu-id="46f9e-483">Limits on a webhook action can be specified in the same manner as [HTTP Asynchronous Limits](#asynchronous-limits).</span></span>
  
## <a name="response-action"></a><span data-ttu-id="46f9e-484">Svaret åtgärd</span><span class="sxs-lookup"><span data-stu-id="46f9e-484">Response action</span></span>  

<span data-ttu-id="46f9e-485">Den här typen innehåller hela svaret nyttolast från en HTTP-begäran och ett statusCode, brödtext och rubriker:</span><span class="sxs-lookup"><span data-stu-id="46f9e-485">This action type contains the entire response payload from an HTTP request and includes a statusCode, body, and headers:</span></span>  
  
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
  
<span data-ttu-id="46f9e-486">Instruktionen svar har särskilda begränsningar som inte gäller för andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="46f9e-486">The response action has special restrictions that don't apply to other actions.</span></span> <span data-ttu-id="46f9e-487">Närmare bestämt:</span><span class="sxs-lookup"><span data-stu-id="46f9e-487">Specifically:</span></span>  
  
-   <span data-ttu-id="46f9e-488">Responsåtgärder får inte vara parallellt i en definition av eftersom en deterministisk svar på inkommande begäran krävs.</span><span class="sxs-lookup"><span data-stu-id="46f9e-488">Response actions cannot be parallel in a definition because a deterministic response to the incoming request is required.</span></span>  
  
-   <span data-ttu-id="46f9e-489">Om en åtgärd för svar uppnås när den inkommande begäranden tog emot ett svar, åtgärden betraktas som misslyckad \(konflikt\), och därför inte `Failed`.</span><span class="sxs-lookup"><span data-stu-id="46f9e-489">If a response action is reached after the incoming request has received a response, the action is considered failed \(conflict\), and as a result, the run is `Failed`.</span></span>  
  
-   <span data-ttu-id="46f9e-490">Ett arbetsflöde med responsåtgärder kan inte ha `splitOn` i dess utlösare eftersom ett anrop gör många körs.</span><span class="sxs-lookup"><span data-stu-id="46f9e-490">A workflow with Response actions cannot have `splitOn` in its trigger because one call causes many runs.</span></span> <span data-ttu-id="46f9e-491">Det bör därför verifieras när flödet är PUT och orsaken en felaktig begäran.</span><span class="sxs-lookup"><span data-stu-id="46f9e-491">As a result, this should be validated when the flow is PUT and cause a Bad Request.</span></span>  
  
## <a name="wait-action"></a><span data-ttu-id="46f9e-492">Vänta åtgärd</span><span class="sxs-lookup"><span data-stu-id="46f9e-492">Wait action</span></span>  

<span data-ttu-id="46f9e-493">Den `wait` åtgärd pausar körningen av arbetsflödet för det angivna intervallet.</span><span class="sxs-lookup"><span data-stu-id="46f9e-493">The `wait` action suspends workflow execution for the specified interval.</span></span> <span data-ttu-id="46f9e-494">Du kan till exempel använda den här fragment för att vänta 15 minuter:</span><span class="sxs-lookup"><span data-stu-id="46f9e-494">For example, to wait 15 minutes, you can use this snippet:</span></span>  
  
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
  
<span data-ttu-id="46f9e-495">Om du vill vänta tills en särskild tidpunkt, kan du också använda det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="46f9e-495">Alternatively, to wait until a specific moment in time, you can use this example:</span></span>  
  
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
> <span data-ttu-id="46f9e-496">Vänta varaktighet kan anges antingen med hjälp av den **intervall** objekt eller **tills** objekt, men inte båda.</span><span class="sxs-lookup"><span data-stu-id="46f9e-496">The wait duration can be either specified using the **interval** object or the **until** object, but not both.</span></span>  
  
|<span data-ttu-id="46f9e-497">Namn</span><span class="sxs-lookup"><span data-stu-id="46f9e-497">Name</span></span>|<span data-ttu-id="46f9e-498">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-498">Required</span></span>|<span data-ttu-id="46f9e-499">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-499">Type</span></span>|<span data-ttu-id="46f9e-500">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-500">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="46f9e-501">intervall</span><span class="sxs-lookup"><span data-stu-id="46f9e-501">interval</span></span>|<span data-ttu-id="46f9e-502">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-502">No</span></span>|<span data-ttu-id="46f9e-503">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-503">Object</span></span>|<span data-ttu-id="46f9e-504">Vänta varaktighet baserat på tid.</span><span class="sxs-lookup"><span data-stu-id="46f9e-504">The wait duration based on amount of time.</span></span>|  
|<span data-ttu-id="46f9e-505">intervall</span><span class="sxs-lookup"><span data-stu-id="46f9e-505">interval unit</span></span>|<span data-ttu-id="46f9e-506">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-506">Yes</span></span>|<span data-ttu-id="46f9e-507">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-507">String</span></span>|<span data-ttu-id="46f9e-508">En av dessa intervall: sekund, minut, timma, dag, vecka, månad, år.</span><span class="sxs-lookup"><span data-stu-id="46f9e-508">One of these intervals: second, minute, hour, day, week, month, year.</span></span>|  
|<span data-ttu-id="46f9e-509">intervall för antal</span><span class="sxs-lookup"><span data-stu-id="46f9e-509">interval count</span></span>|<span data-ttu-id="46f9e-510">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-510">Yes</span></span>|<span data-ttu-id="46f9e-511">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-511">String</span></span>|<span data-ttu-id="46f9e-512">Varaktighet baserat på den angivna interna enheten.</span><span class="sxs-lookup"><span data-stu-id="46f9e-512">Duration based on the given internal unit.</span></span>|  
|<span data-ttu-id="46f9e-513">fram till</span><span class="sxs-lookup"><span data-stu-id="46f9e-513">until</span></span>|<span data-ttu-id="46f9e-514">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-514">No</span></span>|<span data-ttu-id="46f9e-515">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-515">Object</span></span>|<span data-ttu-id="46f9e-516">Vänta varaktighet baserat på en punkt i tiden.</span><span class="sxs-lookup"><span data-stu-id="46f9e-516">The wait duration based on a point in time.</span></span>|  
|<span data-ttu-id="46f9e-517">tills tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="46f9e-517">until timestamp</span></span>|<span data-ttu-id="46f9e-518">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-518">Yes</span></span>|<span data-ttu-id="46f9e-519">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-519">String</span></span>|<span data-ttu-id="46f9e-520">Sträng &#124; Punkten i tiden i UTC när väntetiden upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="46f9e-520">String&#124;The point in time in UTC when the wait expires.</span></span>|  

## <a name="query-action"></a><span data-ttu-id="46f9e-521">Frågeåtgärden</span><span class="sxs-lookup"><span data-stu-id="46f9e-521">Query action</span></span>

<span data-ttu-id="46f9e-522">Den `query` åtgärd kan du filtrera en matris baserat på ett villkor.</span><span class="sxs-lookup"><span data-stu-id="46f9e-522">The `query` action lets you filter an array based on a condition.</span></span> <span data-ttu-id="46f9e-523">Till exempel för att välja nummer som är större än 2, kan du använda:</span><span class="sxs-lookup"><span data-stu-id="46f9e-523">For example, to select numbers greater than 2, you can use:</span></span>

```json
"FilterNumbers" : {
    "type": "query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

<span data-ttu-id="46f9e-524">Utdata från den `query` åtgärden är en matris som har element från Indatamatrisen som uppfyller villkoret.</span><span class="sxs-lookup"><span data-stu-id="46f9e-524">The output from the `query` action is an array that has elements from the input array that satisfy the condition.</span></span>

> [!NOTE]
> <span data-ttu-id="46f9e-525">Om inga värden uppfyller de `where` villkoret, resultatet är en tom matris.</span><span class="sxs-lookup"><span data-stu-id="46f9e-525">If no values satisfy the `where` condition, the result is an empty array.</span></span>

|<span data-ttu-id="46f9e-526">Namn</span><span class="sxs-lookup"><span data-stu-id="46f9e-526">Name</span></span>|<span data-ttu-id="46f9e-527">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-527">Required</span></span>|<span data-ttu-id="46f9e-528">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-528">Type</span></span>|<span data-ttu-id="46f9e-529">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-529">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="46f9e-530">Från</span><span class="sxs-lookup"><span data-stu-id="46f9e-530">from</span></span>|<span data-ttu-id="46f9e-531">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-531">Yes</span></span>|<span data-ttu-id="46f9e-532">matris</span><span class="sxs-lookup"><span data-stu-id="46f9e-532">Array</span></span>|<span data-ttu-id="46f9e-533">Källmatrisen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-533">The source array.</span></span>|
|<span data-ttu-id="46f9e-534">där</span><span class="sxs-lookup"><span data-stu-id="46f9e-534">where</span></span>|<span data-ttu-id="46f9e-535">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-535">Yes</span></span>|<span data-ttu-id="46f9e-536">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-536">String</span></span>|<span data-ttu-id="46f9e-537">Villkoret som gäller för varje element i källmatrisen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-537">The condition to apply to each element of the source array.</span></span>|

## <a name="select-action"></a><span data-ttu-id="46f9e-538">Välj åtgärd</span><span class="sxs-lookup"><span data-stu-id="46f9e-538">Select action</span></span>

<span data-ttu-id="46f9e-539">Den `select` du kan projicera varje element i en matris till ett nytt värde.</span><span class="sxs-lookup"><span data-stu-id="46f9e-539">The `select` action lets you project each element of an array into a new value.</span></span>
<span data-ttu-id="46f9e-540">Till exempel om du vill konvertera en matris av talen i en array med objekt som kan du använda:</span><span class="sxs-lookup"><span data-stu-id="46f9e-540">For example, to convert an array of numbers into an array of objects, you can use:</span></span>

```json
"SelectNumbers" : {
    "type": "select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

<span data-ttu-id="46f9e-541">Utdata från den `select` åtgärden är en matris som har samma kardinalitet som Indatamatrisen, där varje element omvandlas som definieras av den `select` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-541">The output of the `select` action is an array that has the same cardinality as the input array, with each element transformed as defined by the `select` property.</span></span> <span data-ttu-id="46f9e-542">Om indata är en tom matris, är utdata också en tom matris.</span><span class="sxs-lookup"><span data-stu-id="46f9e-542">If the input is an empty array, the output is also an empty array.</span></span>

|<span data-ttu-id="46f9e-543">Namn</span><span class="sxs-lookup"><span data-stu-id="46f9e-543">Name</span></span>|<span data-ttu-id="46f9e-544">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-544">Required</span></span>|<span data-ttu-id="46f9e-545">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-545">Type</span></span>|<span data-ttu-id="46f9e-546">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-546">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="46f9e-547">Från</span><span class="sxs-lookup"><span data-stu-id="46f9e-547">from</span></span>|<span data-ttu-id="46f9e-548">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-548">Yes</span></span>|<span data-ttu-id="46f9e-549">matris</span><span class="sxs-lookup"><span data-stu-id="46f9e-549">Array</span></span>|<span data-ttu-id="46f9e-550">Källmatrisen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-550">The source array.</span></span>|
|<span data-ttu-id="46f9e-551">Välj</span><span class="sxs-lookup"><span data-stu-id="46f9e-551">select</span></span>|<span data-ttu-id="46f9e-552">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-552">Yes</span></span>|<span data-ttu-id="46f9e-553">Alla</span><span class="sxs-lookup"><span data-stu-id="46f9e-553">Any</span></span>|<span data-ttu-id="46f9e-554">Projektionen som ska gälla för varje element i källmatrisen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-554">The projection to apply to each element of the source array.</span></span>|

## <a name="terminate-action"></a><span data-ttu-id="46f9e-555">Åtgärden Avbryt</span><span class="sxs-lookup"><span data-stu-id="46f9e-555">Terminate action</span></span>

<span data-ttu-id="46f9e-556">Åtgärden Avsluta avbryts körningen av arbetsflödet kör, avbryts alla pågående åtgärder och hoppa över eventuella återstående åtgärder.</span><span class="sxs-lookup"><span data-stu-id="46f9e-556">The Terminate action stops execution of the workflow run, aborting any in-flight actions, and skipping any remaining actions.</span></span> <span data-ttu-id="46f9e-557">Till exempel för att avsluta en körning med statusen **misslyckades**, kan du använda följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="46f9e-557">For example, to terminate a run with status **Failed**, you can use the following snippet:</span></span>

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
> <span data-ttu-id="46f9e-558">Åtgärder som redan har slutförts påverkas inte av åtgärden Avsluta.</span><span class="sxs-lookup"><span data-stu-id="46f9e-558">Actions already completed are not affected by the terminate action.</span></span>

|<span data-ttu-id="46f9e-559">Namn</span><span class="sxs-lookup"><span data-stu-id="46f9e-559">Name</span></span>|<span data-ttu-id="46f9e-560">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-560">Required</span></span>|<span data-ttu-id="46f9e-561">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-561">Type</span></span>|<span data-ttu-id="46f9e-562">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-562">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="46f9e-563">runStatus</span><span class="sxs-lookup"><span data-stu-id="46f9e-563">runStatus</span></span>|<span data-ttu-id="46f9e-564">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-564">Yes</span></span>|<span data-ttu-id="46f9e-565">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-565">String</span></span>|<span data-ttu-id="46f9e-566">Målet kör status.</span><span class="sxs-lookup"><span data-stu-id="46f9e-566">The target run status.</span></span> <span data-ttu-id="46f9e-567">Antingen **misslyckades** eller **avbröts**.</span><span class="sxs-lookup"><span data-stu-id="46f9e-567">Either **Failed** or **Cancelled**.</span></span>|
|<span data-ttu-id="46f9e-568">runError</span><span class="sxs-lookup"><span data-stu-id="46f9e-568">runError</span></span>|<span data-ttu-id="46f9e-569">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-569">No</span></span>|<span data-ttu-id="46f9e-570">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-570">Object</span></span>|<span data-ttu-id="46f9e-571">Felinformation.</span><span class="sxs-lookup"><span data-stu-id="46f9e-571">The error details.</span></span> <span data-ttu-id="46f9e-572">Endast när **runStatus** är inställd på **misslyckades**.</span><span class="sxs-lookup"><span data-stu-id="46f9e-572">Only supported when **runStatus** is set to **Failed**.</span></span>|
|<span data-ttu-id="46f9e-573">runError kod</span><span class="sxs-lookup"><span data-stu-id="46f9e-573">runError code</span></span>|<span data-ttu-id="46f9e-574">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-574">No</span></span>|<span data-ttu-id="46f9e-575">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-575">String</span></span>|<span data-ttu-id="46f9e-576">Kör felkoden.</span><span class="sxs-lookup"><span data-stu-id="46f9e-576">The run error code.</span></span>|
|<span data-ttu-id="46f9e-577">runError meddelande</span><span class="sxs-lookup"><span data-stu-id="46f9e-577">runError message</span></span>|<span data-ttu-id="46f9e-578">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-578">No</span></span>|<span data-ttu-id="46f9e-579">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-579">String</span></span>|<span data-ttu-id="46f9e-580">Kör ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="46f9e-580">The run error message.</span></span>|

## <a name="compose-action"></a><span data-ttu-id="46f9e-581">Skriv åtgärd</span><span class="sxs-lookup"><span data-stu-id="46f9e-581">Compose action</span></span>

<span data-ttu-id="46f9e-582">Skriv åtgärd kan du skapa ett godtyckligt objekt.</span><span class="sxs-lookup"><span data-stu-id="46f9e-582">The Compose action lets you construct an arbitrary object.</span></span> <span data-ttu-id="46f9e-583">Utdata från åtgärden Skriv är ett resultat av utvärderingen av indata.</span><span class="sxs-lookup"><span data-stu-id="46f9e-583">The output of the compose action is the result of evaluating its inputs.</span></span> <span data-ttu-id="46f9e-584">Du kan till exempel använda Skriv åtgärd för att sammanfoga utdata för flera åtgärder:</span><span class="sxs-lookup"><span data-stu-id="46f9e-584">For example, you can use the compose action to merge outputs of multiple actions:</span></span>

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
> <span data-ttu-id="46f9e-585">Den **Compose** åtgärd kan användas för att skapa några utdata, inklusive objekt, matriser och andra typer som stöds av logikappar som XML och binary.</span><span class="sxs-lookup"><span data-stu-id="46f9e-585">The **Compose** action can be used to construct any output, including objects, arrays, and any other type natively supported by logic apps like XML and binary.</span></span>

## <a name="table-action"></a><span data-ttu-id="46f9e-586">Åtgärden för tabellen</span><span class="sxs-lookup"><span data-stu-id="46f9e-586">Table action</span></span>

<span data-ttu-id="46f9e-587">Den `table` kan du konvertera en matris med-objekt till en **CSV** eller **HTML** tabell.</span><span class="sxs-lookup"><span data-stu-id="46f9e-587">The `table` allows you to convert an array of items into a **CSV** or **HTML** table.</span></span>

<span data-ttu-id="46f9e-588">Anta att @triggerBodyär)</span><span class="sxs-lookup"><span data-stu-id="46f9e-588">Suppose @triggerBody() is</span></span>

```json
[{
  "id": 0,
  "name": "apples"
},{
  "id": 1, 
  "name": "oranges"
}]
```

<span data-ttu-id="46f9e-589">Och låta den åtgärd som definierats som</span><span class="sxs-lookup"><span data-stu-id="46f9e-589">And let the action be defined as</span></span>

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html"
    }
}
```

<span data-ttu-id="46f9e-590">Ovanstående skulle skapa</span><span class="sxs-lookup"><span data-stu-id="46f9e-590">The above would produce</span></span>

<table><thead><tr><th><span data-ttu-id="46f9e-591">id</span><span class="sxs-lookup"><span data-stu-id="46f9e-591">id</span></span></th><th><span data-ttu-id="46f9e-592">namn</span><span class="sxs-lookup"><span data-stu-id="46f9e-592">name</span></span></th></tr></thead><tbody><tr><td><span data-ttu-id="46f9e-593">0</span><span class="sxs-lookup"><span data-stu-id="46f9e-593">0</span></span></td><td><span data-ttu-id="46f9e-594">Äpplen</span><span class="sxs-lookup"><span data-stu-id="46f9e-594">apples</span></span></td></tr><tr><td><span data-ttu-id="46f9e-595">1</span><span class="sxs-lookup"><span data-stu-id="46f9e-595">1</span></span></td><td><span data-ttu-id="46f9e-596">orange</span><span class="sxs-lookup"><span data-stu-id="46f9e-596">oranges</span></span></td></tr></tbody></table><span data-ttu-id="46f9e-597">"</span><span class="sxs-lookup"><span data-stu-id="46f9e-597">"</span></span>

<span data-ttu-id="46f9e-598">För att anpassa tabellen kan du ange vilka kolumner explicit.</span><span class="sxs-lookup"><span data-stu-id="46f9e-598">In order to customize the table, you can specify the columns explicitly.</span></span> <span data-ttu-id="46f9e-599">Exempel:</span><span class="sxs-lookup"><span data-stu-id="46f9e-599">For example:</span></span>

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

<span data-ttu-id="46f9e-600">Ovanstående skulle skapa</span><span class="sxs-lookup"><span data-stu-id="46f9e-600">The above would produce</span></span>

<table><thead><tr><th><span data-ttu-id="46f9e-601">Skapa id</span><span class="sxs-lookup"><span data-stu-id="46f9e-601">produce id</span></span></th><th><span data-ttu-id="46f9e-602">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-602">description</span></span></th></tr></thead><tbody><tr><td><span data-ttu-id="46f9e-603">0</span><span class="sxs-lookup"><span data-stu-id="46f9e-603">0</span></span></td><td><span data-ttu-id="46f9e-604">ny äpplen</span><span class="sxs-lookup"><span data-stu-id="46f9e-604">fresh apples</span></span></td></tr><tr><td><span data-ttu-id="46f9e-605">1</span><span class="sxs-lookup"><span data-stu-id="46f9e-605">1</span></span></td><td><span data-ttu-id="46f9e-606">ny orange</span><span class="sxs-lookup"><span data-stu-id="46f9e-606">fresh oranges</span></span></td></tr></tbody></table><span data-ttu-id="46f9e-607">"</span><span class="sxs-lookup"><span data-stu-id="46f9e-607">"</span></span>

<span data-ttu-id="46f9e-608">Om den `from` egenskapsvärdet är en tom matris, utdata är en tom tabell.</span><span class="sxs-lookup"><span data-stu-id="46f9e-608">If the `from` property value is an empty array, the output is an empty table.</span></span>

|<span data-ttu-id="46f9e-609">Namn</span><span class="sxs-lookup"><span data-stu-id="46f9e-609">Name</span></span>|<span data-ttu-id="46f9e-610">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-610">Required</span></span>|<span data-ttu-id="46f9e-611">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-611">Type</span></span>|<span data-ttu-id="46f9e-612">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-612">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="46f9e-613">Från</span><span class="sxs-lookup"><span data-stu-id="46f9e-613">from</span></span>|<span data-ttu-id="46f9e-614">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-614">Yes</span></span>|<span data-ttu-id="46f9e-615">matris</span><span class="sxs-lookup"><span data-stu-id="46f9e-615">Array</span></span>|<span data-ttu-id="46f9e-616">Källmatrisen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-616">The source array.</span></span>|
|<span data-ttu-id="46f9e-617">Format</span><span class="sxs-lookup"><span data-stu-id="46f9e-617">format</span></span>|<span data-ttu-id="46f9e-618">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-618">Yes</span></span>|<span data-ttu-id="46f9e-619">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-619">String</span></span>|<span data-ttu-id="46f9e-620">Format, antingen **CSV** eller **HTML**.</span><span class="sxs-lookup"><span data-stu-id="46f9e-620">The format, either **CSV** or **HTML**.</span></span>|
|<span data-ttu-id="46f9e-621">Kolumner</span><span class="sxs-lookup"><span data-stu-id="46f9e-621">columns</span></span>|<span data-ttu-id="46f9e-622">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-622">No</span></span>|<span data-ttu-id="46f9e-623">matris</span><span class="sxs-lookup"><span data-stu-id="46f9e-623">Array</span></span>|<span data-ttu-id="46f9e-624">Kolumner.</span><span class="sxs-lookup"><span data-stu-id="46f9e-624">The columns.</span></span> <span data-ttu-id="46f9e-625">Tillåter för att åsidosätta standard formen i tabellen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-625">Allows to override the default shape of the table.</span></span>|
|<span data-ttu-id="46f9e-626">kolumnrubrik</span><span class="sxs-lookup"><span data-stu-id="46f9e-626">column header</span></span>|<span data-ttu-id="46f9e-627">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-627">No</span></span>|<span data-ttu-id="46f9e-628">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-628">String</span></span>|<span data-ttu-id="46f9e-629">Rubriken för kolumnen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-629">The header of the column.</span></span>|
|<span data-ttu-id="46f9e-630">värde i kolumnen</span><span class="sxs-lookup"><span data-stu-id="46f9e-630">column value</span></span>|<span data-ttu-id="46f9e-631">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-631">Yes</span></span>|<span data-ttu-id="46f9e-632">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-632">String</span></span>|<span data-ttu-id="46f9e-633">Värdet för kolumnen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-633">The value of the column.</span></span>|

## <a name="workflow-action"></a><span data-ttu-id="46f9e-634">Arbetsflödesåtgärd</span><span class="sxs-lookup"><span data-stu-id="46f9e-634">Workflow action</span></span>   

|<span data-ttu-id="46f9e-635">Namn</span><span class="sxs-lookup"><span data-stu-id="46f9e-635">Name</span></span>|<span data-ttu-id="46f9e-636">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-636">Required</span></span>|<span data-ttu-id="46f9e-637">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-637">Type</span></span>|<span data-ttu-id="46f9e-638">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-638">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="46f9e-639">värd-id</span><span class="sxs-lookup"><span data-stu-id="46f9e-639">host id</span></span>|<span data-ttu-id="46f9e-640">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-640">Yes</span></span>|<span data-ttu-id="46f9e-641">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-641">String</span></span>|<span data-ttu-id="46f9e-642">Resurs-ID för det arbetsflöde som du vill anropa.</span><span class="sxs-lookup"><span data-stu-id="46f9e-642">The resource ID of the workflow that you want to call.</span></span>|  
|<span data-ttu-id="46f9e-643">värden Utlösarnamn</span><span class="sxs-lookup"><span data-stu-id="46f9e-643">host triggerName</span></span>|<span data-ttu-id="46f9e-644">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-644">Yes</span></span>|<span data-ttu-id="46f9e-645">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-645">String</span></span>|<span data-ttu-id="46f9e-646">Namnet på utlösaren som du vill anropa.</span><span class="sxs-lookup"><span data-stu-id="46f9e-646">The name of the trigger that you want to invoke.</span></span>|  
|<span data-ttu-id="46f9e-647">frågor</span><span class="sxs-lookup"><span data-stu-id="46f9e-647">queries</span></span>|<span data-ttu-id="46f9e-648">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-648">No</span></span>|<span data-ttu-id="46f9e-649">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-649">Object</span></span>|<span data-ttu-id="46f9e-650">Representerar frågeparametrar att lägga till URL: en.</span><span class="sxs-lookup"><span data-stu-id="46f9e-650">Represents the query parameters to add to the URL.</span></span> <span data-ttu-id="46f9e-651">Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` till URL: en.</span><span class="sxs-lookup"><span data-stu-id="46f9e-651">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|  
|<span data-ttu-id="46f9e-652">Rubriker</span><span class="sxs-lookup"><span data-stu-id="46f9e-652">headers</span></span>|<span data-ttu-id="46f9e-653">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-653">No</span></span>|<span data-ttu-id="46f9e-654">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-654">Object</span></span>|<span data-ttu-id="46f9e-655">Representerar var och en av huvuden som skickas till begäran.</span><span class="sxs-lookup"><span data-stu-id="46f9e-655">Represents each of the headers that is sent to the request.</span></span> <span data-ttu-id="46f9e-656">Till exempel ange språk och Skriv begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="46f9e-656">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="46f9e-657">Brödtext</span><span class="sxs-lookup"><span data-stu-id="46f9e-657">body</span></span>|<span data-ttu-id="46f9e-658">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-658">No</span></span>|<span data-ttu-id="46f9e-659">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-659">Object</span></span>|<span data-ttu-id="46f9e-660">Representerar nyttolasten skickas till slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="46f9e-660">Represents the payload sent to the endpoint.</span></span>|  
  
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
  
<span data-ttu-id="46f9e-661">En åtkomstkontroll görs på arbetsflödet \(mer specifikt utlösaren\), vilket innebär att du behöver åtkomst till arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="46f9e-661">An access check is made on the workflow \(more specifically, the trigger\), meaning you need access to the workflow.</span></span>  
  
<span data-ttu-id="46f9e-662">Utdata från den `workflow` åtgärd baserat på vad du har definierat i på `response` åtgärd i arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="46f9e-662">The outputs from the `workflow` action are based on what you defined in the `response` action in the child workflow.</span></span> <span data-ttu-id="46f9e-663">Om du inte har definierat någon `response` åtgärd och sedan utdata är tomma.</span><span class="sxs-lookup"><span data-stu-id="46f9e-663">If you have not defined any `response` action, then the outputs are empty.</span></span>  

## <a name="function-action"></a><span data-ttu-id="46f9e-664">Funktionen åtgärd</span><span class="sxs-lookup"><span data-stu-id="46f9e-664">Function action</span></span>   

|<span data-ttu-id="46f9e-665">Namn</span><span class="sxs-lookup"><span data-stu-id="46f9e-665">Name</span></span>|<span data-ttu-id="46f9e-666">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-666">Required</span></span>|<span data-ttu-id="46f9e-667">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-667">Type</span></span>|<span data-ttu-id="46f9e-668">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-668">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="46f9e-669">funktionen id</span><span class="sxs-lookup"><span data-stu-id="46f9e-669">function id</span></span>|<span data-ttu-id="46f9e-670">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-670">Yes</span></span>|<span data-ttu-id="46f9e-671">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-671">String</span></span>|<span data-ttu-id="46f9e-672">Resurs-ID för den funktion som du vill anropa.</span><span class="sxs-lookup"><span data-stu-id="46f9e-672">The resource ID of the function that you want to invoke.</span></span>|  
|<span data-ttu-id="46f9e-673">Metoden</span><span class="sxs-lookup"><span data-stu-id="46f9e-673">method</span></span>|<span data-ttu-id="46f9e-674">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-674">No</span></span>|<span data-ttu-id="46f9e-675">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-675">String</span></span>|<span data-ttu-id="46f9e-676">HTTP-metoden som används för att anropa funktionen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-676">The HTTP method used to invoke the function.</span></span> <span data-ttu-id="46f9e-677">Som standard är det `POST` om inget värde anges.</span><span class="sxs-lookup"><span data-stu-id="46f9e-677">By default, it is `POST` when not specified.</span></span>|  
|<span data-ttu-id="46f9e-678">frågor</span><span class="sxs-lookup"><span data-stu-id="46f9e-678">queries</span></span>|<span data-ttu-id="46f9e-679">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-679">No</span></span>|<span data-ttu-id="46f9e-680">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-680">Object</span></span>|<span data-ttu-id="46f9e-681">Representerar frågeparametrar att lägga till URL: en.</span><span class="sxs-lookup"><span data-stu-id="46f9e-681">Represents the query parameters to add to the URL.</span></span> <span data-ttu-id="46f9e-682">Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` till URL: en.</span><span class="sxs-lookup"><span data-stu-id="46f9e-682">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|  
|<span data-ttu-id="46f9e-683">Rubriker</span><span class="sxs-lookup"><span data-stu-id="46f9e-683">headers</span></span>|<span data-ttu-id="46f9e-684">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-684">No</span></span>|<span data-ttu-id="46f9e-685">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-685">Object</span></span>|<span data-ttu-id="46f9e-686">Representerar var och en av huvuden som skickas till begäran.</span><span class="sxs-lookup"><span data-stu-id="46f9e-686">Represents each of the headers that is sent to the request.</span></span> <span data-ttu-id="46f9e-687">Till exempel ange språk och typ för en begäran: `"headers" : { "Accept-Language": "en-us" }`.</span><span class="sxs-lookup"><span data-stu-id="46f9e-687">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us" }`.</span></span>|  
|<span data-ttu-id="46f9e-688">Brödtext</span><span class="sxs-lookup"><span data-stu-id="46f9e-688">body</span></span>|<span data-ttu-id="46f9e-689">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-689">No</span></span>|<span data-ttu-id="46f9e-690">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-690">Object</span></span>|<span data-ttu-id="46f9e-691">Representerar nyttolasten skickas till slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="46f9e-691">Represents the payload sent to the endpoint.</span></span>|  

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

<span data-ttu-id="46f9e-692">När du sparar logikappen kan göra vi några kontroller på den angivna funktionen:</span><span class="sxs-lookup"><span data-stu-id="46f9e-692">When you save the logic app, we perform some checks on the referenced function:</span></span>
-   <span data-ttu-id="46f9e-693">Du måste ha tillgång till funktionen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-693">You need to have access to the function.</span></span>
-   <span data-ttu-id="46f9e-694">Endast är standard HTTP-utlösare eller allmänna JSON webhook utlösare tillåten.</span><span class="sxs-lookup"><span data-stu-id="46f9e-694">Only standard HTTP trigger or generic JSON webhook trigger is allowed.</span></span>
-   <span data-ttu-id="46f9e-695">Det får inte innehålla någon väg som har definierats.</span><span class="sxs-lookup"><span data-stu-id="46f9e-695">It should not have any route defined.</span></span>
-   <span data-ttu-id="46f9e-696">Endast ”fungera” och ”anonym” åtkomstnivå är tillåten.</span><span class="sxs-lookup"><span data-stu-id="46f9e-696">Only "function" and "anonymous" authorization level is allowed.</span></span>

<span data-ttu-id="46f9e-697">Utlösaren URL: en hämtas, cachelagras och används vid körning.</span><span class="sxs-lookup"><span data-stu-id="46f9e-697">The trigger URL is retrieved, cached, and used at runtime.</span></span> <span data-ttu-id="46f9e-698">Så om någon åtgärd upphäver den cachelagra URL, misslyckas åtgärden under körning.</span><span class="sxs-lookup"><span data-stu-id="46f9e-698">So if any operation invalidates the cached URL, the action fails at runtime.</span></span> <span data-ttu-id="46f9e-699">Undvik detta genom att spara logikappen igen, vilket leder till logikapp du hämtar och cachelagrar utlösaren URL: en igen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-699">To work around this, save the logic app again, which will cause logic app to retrieve and cache the trigger URL again.</span></span>

## <a name="collection-actions-scopes-and-loops"></a><span data-ttu-id="46f9e-700">Samling åtgärder (scope och slingor)</span><span class="sxs-lookup"><span data-stu-id="46f9e-700">Collection actions (scopes and loops)</span></span>

<span data-ttu-id="46f9e-701">Vissa åtgärdstyper av kan innehålla åtgärder i själva.</span><span class="sxs-lookup"><span data-stu-id="46f9e-701">Some action types can contain actions within themselves.</span></span> <span data-ttu-id="46f9e-702">Referens-åtgärder i en samling kan refereras direkt utanför samlingen.</span><span class="sxs-lookup"><span data-stu-id="46f9e-702">Reference actions within a collection can be referenced directly outside of the collection.</span></span> <span data-ttu-id="46f9e-703">Om du har definierat `http` i en omfattning `@body('http')` fortfarande är giltigt var som helst i ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="46f9e-703">If you defined `http` in a scope, `@body('http')` is still valid anywhere in a workflow.</span></span> <span data-ttu-id="46f9e-704">Åtgärder i en samling kan `runAfter` endast andra åtgärder i samma samling.</span><span class="sxs-lookup"><span data-stu-id="46f9e-704">Actions within a collection can `runAfter` only other actions within the same collection.</span></span>

## <a name="scope-action"></a><span data-ttu-id="46f9e-705">Scope-åtgärd</span><span class="sxs-lookup"><span data-stu-id="46f9e-705">Scope action</span></span>

<span data-ttu-id="46f9e-706">Den `scope` åtgärd kan du logiskt gruppera åtgärder i ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="46f9e-706">The `scope` action lets you logically group actions in a workflow.</span></span>

|<span data-ttu-id="46f9e-707">Namn</span><span class="sxs-lookup"><span data-stu-id="46f9e-707">Name</span></span>|<span data-ttu-id="46f9e-708">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-708">Required</span></span>|<span data-ttu-id="46f9e-709">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-709">Type</span></span>|<span data-ttu-id="46f9e-710">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-710">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="46f9e-711">åtgärder</span><span class="sxs-lookup"><span data-stu-id="46f9e-711">actions</span></span>|<span data-ttu-id="46f9e-712">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-712">Yes</span></span>|<span data-ttu-id="46f9e-713">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-713">Object</span></span>|<span data-ttu-id="46f9e-714">Inre åtgärder för att köra inom omfånget</span><span class="sxs-lookup"><span data-stu-id="46f9e-714">Inner actions to execute within the scope</span></span>|

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

## <a name="foreach-action"></a><span data-ttu-id="46f9e-715">ForEach-åtgärd</span><span class="sxs-lookup"><span data-stu-id="46f9e-715">ForEach action</span></span>

<span data-ttu-id="46f9e-716">Åtgärden slingor upprepas i en matris och utför inre åtgärder för varje objekt.</span><span class="sxs-lookup"><span data-stu-id="46f9e-716">This looping action iterates through an array and performs inner actions for each item.</span></span> <span data-ttu-id="46f9e-717">Som standard kör foreach-loop parallellt (20 körningar parallellt i taget).</span><span class="sxs-lookup"><span data-stu-id="46f9e-717">By default, the foreach loop executes in parallel (20 executions in parallel at a time).</span></span> <span data-ttu-id="46f9e-718">Du kan ange regler för körning med hjälp av den `operationOptions` parameter.</span><span class="sxs-lookup"><span data-stu-id="46f9e-718">You can set execution rules using the `operationOptions` parameter.</span></span>

|<span data-ttu-id="46f9e-719">Namn</span><span class="sxs-lookup"><span data-stu-id="46f9e-719">Name</span></span>|<span data-ttu-id="46f9e-720">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-720">Required</span></span>|<span data-ttu-id="46f9e-721">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-721">Type</span></span>|<span data-ttu-id="46f9e-722">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-722">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="46f9e-723">åtgärder</span><span class="sxs-lookup"><span data-stu-id="46f9e-723">actions</span></span>|<span data-ttu-id="46f9e-724">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-724">Yes</span></span>|<span data-ttu-id="46f9e-725">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-725">Object</span></span>|<span data-ttu-id="46f9e-726">Inre åtgärder för att köra inom loopen</span><span class="sxs-lookup"><span data-stu-id="46f9e-726">Inner actions to execute within the loop</span></span>|
|<span data-ttu-id="46f9e-727">foreach</span><span class="sxs-lookup"><span data-stu-id="46f9e-727">foreach</span></span>|<span data-ttu-id="46f9e-728">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-728">Yes</span></span>|<span data-ttu-id="46f9e-729">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-729">string</span></span>|<span data-ttu-id="46f9e-730">Matrisen och iterera över</span><span class="sxs-lookup"><span data-stu-id="46f9e-730">The array to iterate over</span></span>|
|<span data-ttu-id="46f9e-731">operationOptions</span><span class="sxs-lookup"><span data-stu-id="46f9e-731">operationOptions</span></span>|<span data-ttu-id="46f9e-732">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-732">no</span></span>|<span data-ttu-id="46f9e-733">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-733">string</span></span>|<span data-ttu-id="46f9e-734">Åtgärden alternativ för beteende.</span><span class="sxs-lookup"><span data-stu-id="46f9e-734">Any operation options for behavior.</span></span> <span data-ttu-id="46f9e-735">Stöder för närvarande endast `sequential` att utföra iterationer sekventiellt (standard är parallella)</span><span class="sxs-lookup"><span data-stu-id="46f9e-735">Currently only supports `sequential` to execute iterations sequentially (default behavior is parallel)</span></span>|

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

## <a name="until-action"></a><span data-ttu-id="46f9e-736">Förrän åtgärden</span><span class="sxs-lookup"><span data-stu-id="46f9e-736">Until action</span></span>

<span data-ttu-id="46f9e-737">Åtgärden slingor kör inre åtgärder tills ett villkor resultatet till true.</span><span class="sxs-lookup"><span data-stu-id="46f9e-737">This looping action executes inner actions until a condition results to true.</span></span>

|<span data-ttu-id="46f9e-738">Namn</span><span class="sxs-lookup"><span data-stu-id="46f9e-738">Name</span></span>|<span data-ttu-id="46f9e-739">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-739">Required</span></span>|<span data-ttu-id="46f9e-740">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-740">Type</span></span>|<span data-ttu-id="46f9e-741">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-741">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="46f9e-742">åtgärder</span><span class="sxs-lookup"><span data-stu-id="46f9e-742">actions</span></span>|<span data-ttu-id="46f9e-743">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-743">Yes</span></span>|<span data-ttu-id="46f9e-744">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-744">Object</span></span>|<span data-ttu-id="46f9e-745">Inre åtgärder för att köra inom loopen</span><span class="sxs-lookup"><span data-stu-id="46f9e-745">Inner actions to execute within the loop</span></span>|
|<span data-ttu-id="46f9e-746">uttryck</span><span class="sxs-lookup"><span data-stu-id="46f9e-746">expression</span></span>|<span data-ttu-id="46f9e-747">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-747">Yes</span></span>|<span data-ttu-id="46f9e-748">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-748">string</span></span>|<span data-ttu-id="46f9e-749">Uttrycket som ska utvärderas efter varje iteration</span><span class="sxs-lookup"><span data-stu-id="46f9e-749">The expression to evaluate after each iteration</span></span>|
|<span data-ttu-id="46f9e-750">Gränsen</span><span class="sxs-lookup"><span data-stu-id="46f9e-750">limit</span></span>|<span data-ttu-id="46f9e-751">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-751">yes</span></span>|<span data-ttu-id="46f9e-752">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-752">Object</span></span>|<span data-ttu-id="46f9e-753">Gränser för loop - minst en gräns måste anges</span><span class="sxs-lookup"><span data-stu-id="46f9e-753">The limits for the loop - at least one limit must be defined</span></span>|
|<span data-ttu-id="46f9e-754">Antal</span><span class="sxs-lookup"><span data-stu-id="46f9e-754">count</span></span>|<span data-ttu-id="46f9e-755">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-755">no</span></span>|<span data-ttu-id="46f9e-756">int</span><span class="sxs-lookup"><span data-stu-id="46f9e-756">int</span></span>|<span data-ttu-id="46f9e-757">Gränsen för antalet upprepningar som kan utföras</span><span class="sxs-lookup"><span data-stu-id="46f9e-757">The limit to the number of iterations that can be performed</span></span>|
|<span data-ttu-id="46f9e-758">Timeout</span><span class="sxs-lookup"><span data-stu-id="46f9e-758">timeout</span></span>|<span data-ttu-id="46f9e-759">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-759">no</span></span>|<span data-ttu-id="46f9e-760">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-760">string</span></span>|<span data-ttu-id="46f9e-761">Tidsgräns för hur länge det ska köras i slinga.</span><span class="sxs-lookup"><span data-stu-id="46f9e-761">The timeout for how long it should loop.</span></span>  <span data-ttu-id="46f9e-762">ISO 8601-format</span><span class="sxs-lookup"><span data-stu-id="46f9e-762">ISO 8601 format</span></span>|


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

## <a name="conditions---if-action"></a><span data-ttu-id="46f9e-763">Villkor - om åtgärden</span><span class="sxs-lookup"><span data-stu-id="46f9e-763">Conditions - If Action</span></span>

<span data-ttu-id="46f9e-764">Den `If` åtgärd kan du utvärdera ett villkor och köra en gren baserat på om uttrycket utvärderas till `true`.</span><span class="sxs-lookup"><span data-stu-id="46f9e-764">The `If` action lets you evaluate a condition and execute a branch based on whether the expression evaluates to `true`.</span></span>

|<span data-ttu-id="46f9e-765">Namn</span><span class="sxs-lookup"><span data-stu-id="46f9e-765">Name</span></span>|<span data-ttu-id="46f9e-766">Krävs</span><span class="sxs-lookup"><span data-stu-id="46f9e-766">Required</span></span>|<span data-ttu-id="46f9e-767">Typ</span><span class="sxs-lookup"><span data-stu-id="46f9e-767">Type</span></span>|<span data-ttu-id="46f9e-768">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46f9e-768">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="46f9e-769">åtgärder</span><span class="sxs-lookup"><span data-stu-id="46f9e-769">actions</span></span>|<span data-ttu-id="46f9e-770">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-770">Yes</span></span>|<span data-ttu-id="46f9e-771">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-771">Object</span></span>|<span data-ttu-id="46f9e-772">Inre åtgärder för att köras när uttrycket utvärderas till`true`</span><span class="sxs-lookup"><span data-stu-id="46f9e-772">Inner actions to execute when expression evaluates to `true`</span></span>|
|<span data-ttu-id="46f9e-773">uttryck</span><span class="sxs-lookup"><span data-stu-id="46f9e-773">expression</span></span>|<span data-ttu-id="46f9e-774">Ja</span><span class="sxs-lookup"><span data-stu-id="46f9e-774">Yes</span></span>|<span data-ttu-id="46f9e-775">Sträng</span><span class="sxs-lookup"><span data-stu-id="46f9e-775">string</span></span>|<span data-ttu-id="46f9e-776">Uttrycket som ska utvärderas</span><span class="sxs-lookup"><span data-stu-id="46f9e-776">The expression to evaluate</span></span>|
|<span data-ttu-id="46f9e-777">annan</span><span class="sxs-lookup"><span data-stu-id="46f9e-777">else</span></span>|<span data-ttu-id="46f9e-778">Nej</span><span class="sxs-lookup"><span data-stu-id="46f9e-778">no</span></span>|<span data-ttu-id="46f9e-779">Objekt</span><span class="sxs-lookup"><span data-stu-id="46f9e-779">Object</span></span>|<span data-ttu-id="46f9e-780">Inre åtgärder för att köras när uttrycket utvärderas till`false`</span><span class="sxs-lookup"><span data-stu-id="46f9e-780">Inner actions to execute when expression evaluates to `false`</span></span>|
  
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
  
<span data-ttu-id="46f9e-781">I följande tabell visas exempel på hur villkor kan använda uttryck i en åtgärd:</span><span class="sxs-lookup"><span data-stu-id="46f9e-781">The following table shows examples of how conditions can use expressions in an action:</span></span>  
  
|<span data-ttu-id="46f9e-782">JSON-värde</span><span class="sxs-lookup"><span data-stu-id="46f9e-782">JSON value</span></span>|<span data-ttu-id="46f9e-783">Resultat</span><span class="sxs-lookup"><span data-stu-id="46f9e-783">Result</span></span>|  
|--------------|----------|  
|`"expression": "@parameters('hasSpecialAction')"`|<span data-ttu-id="46f9e-784">Ett värde som kan utvärderas till SANT gör detta villkor att skicka.</span><span class="sxs-lookup"><span data-stu-id="46f9e-784">Any value that would evaluate to true causes this condition to pass.</span></span> <span data-ttu-id="46f9e-785">Endast booleskt uttryck stöds.</span><span class="sxs-lookup"><span data-stu-id="46f9e-785">Only Boolean expressions are supported.</span></span> <span data-ttu-id="46f9e-786">Konvertera andra typer till Boolean med funktioner `empty`, `equals`.</span><span class="sxs-lookup"><span data-stu-id="46f9e-786">To convert other types to Boolean, use functions `empty`, `equals`.</span></span>|  
|`"expression": "@greater(actions('act1').output.value, parameters('threshold'))"`|<span data-ttu-id="46f9e-787">Jämförelse funktioner stöds.</span><span class="sxs-lookup"><span data-stu-id="46f9e-787">Comparison functions are supported.</span></span> <span data-ttu-id="46f9e-788">Exempelvis den här körs åtgärden endast när utdata från act1 är större än tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="46f9e-788">For the example here, the action only executes when the output of act1 is greater than the threshold.</span></span>|  
|`"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"`|<span data-ttu-id="46f9e-789">Logik funktioner stöds även för att skapa kapslade booleska uttryck.</span><span class="sxs-lookup"><span data-stu-id="46f9e-789">Logic functions are also supported to create nested Boolean expressions.</span></span> <span data-ttu-id="46f9e-790">I det här fallet körs åtgärden när resultatet av act1 är över tröskelvärdet eller under 100.</span><span class="sxs-lookup"><span data-stu-id="46f9e-790">In this case, the action executes when the output of act1 is above the threshold or below 100.</span></span>|  
|`"expression": "@equals(length(actions('act1').outputs.errors), 0))"`|<span data-ttu-id="46f9e-791">Du kan använda matrisfunktioner för att kontrollera om en matris har några objekt.</span><span class="sxs-lookup"><span data-stu-id="46f9e-791">You can use array functions to check if an array has any items.</span></span> <span data-ttu-id="46f9e-792">I det här fallet körs åtgärden när fel matrisen är tom.</span><span class="sxs-lookup"><span data-stu-id="46f9e-792">In this case, the action executes when the errors array is empty.</span></span>| 
|`"expression": "parameters('hasSpecialAction')"`|<span data-ttu-id="46f9e-793">Fel - inte ett giltigt tillstånd eftersom @ krävs för villkor.</span><span class="sxs-lookup"><span data-stu-id="46f9e-793">Error - not a valid condition because @ is required for conditions.</span></span>|  
  
<span data-ttu-id="46f9e-794">Om ett villkor utvärderas har, villkor har markerats som `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="46f9e-794">If a condition evaluates successfully, the condition is marked as `Succeeded`.</span></span> <span data-ttu-id="46f9e-795">Åtgärder i antingen den `actions` eller `else` objekt utvärderas till `Succeeded` när den exekveras och lyckades, `Failed` när den exekveras och misslyckades, eller `Skipped` när den grenen körs inte.</span><span class="sxs-lookup"><span data-stu-id="46f9e-795">Actions within either the `actions` or `else` objects evaluate to `Succeeded` when executed and succeeded, `Failed` when executed and failed, or `Skipped` when that branch is not executed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="46f9e-796">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="46f9e-796">Next steps</span></span>

[<span data-ttu-id="46f9e-797">Arbetsflödet Service REST-API</span><span class="sxs-lookup"><span data-stu-id="46f9e-797">Workflow Service REST API</span></span>](https://docs.microsoft.com/rest/api/logic/workflows)
