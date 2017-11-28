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
# <a name="workflow-actions-and-triggers-for-azure-logic-apps"></a><span data-ttu-id="82298-102">Arbetsflödesåtgärder och utlösare för Logic Apps i Azure</span><span class="sxs-lookup"><span data-stu-id="82298-102">Workflow actions and triggers for Azure Logic Apps</span></span>

<span data-ttu-id="82298-103">Logikappar består av utlösare och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="82298-103">Logic apps consist of triggers and actions.</span></span> <span data-ttu-id="82298-104">Det finns sex olika typer av utlösare.</span><span class="sxs-lookup"><span data-stu-id="82298-104">There are six types of triggers.</span></span> <span data-ttu-id="82298-105">Varje typ har olika gränssnitt och olika beteenden.</span><span class="sxs-lookup"><span data-stu-id="82298-105">Each type has different interface and different behavior.</span></span> <span data-ttu-id="82298-106">Du kan också lära dig om andra uppgifter genom att titta på hello information om hello [språk i arbetsflödesdefinitionen](logic-apps-workflow-definition-language.md).</span><span class="sxs-lookup"><span data-stu-id="82298-106">You can also learn about other details by looking at hello details of hello [Workflow Definition Language](logic-apps-workflow-definition-language.md).</span></span>  
  
<span data-ttu-id="82298-107">Läsa på toolearn mer information om utlösare och åtgärder och hur du kan använda dem toobuild logic apps tooimprove dina affärsprocesser och arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="82298-107">Read on toolearn more about triggers and actions and how you might use them toobuild logic apps tooimprove your business processes and workflows.</span></span>  
  
### <a name="triggers"></a><span data-ttu-id="82298-108">Utlösare</span><span class="sxs-lookup"><span data-stu-id="82298-108">Triggers</span></span>  

<span data-ttu-id="82298-109">En utlösare Anger hello-anrop som kan initiera en körning av arbetsflödet logik app.</span><span class="sxs-lookup"><span data-stu-id="82298-109">A trigger specifies hello calls that can initiate a run of your logic app workflow.</span></span> <span data-ttu-id="82298-110">Här följer hello två olika sätt tooinitiate arbetsflödet körs:</span><span class="sxs-lookup"><span data-stu-id="82298-110">Here are hello two different ways tooinitiate a run of your workflow:</span></span>  
  
-   <span data-ttu-id="82298-111">En avsökning utlösare</span><span class="sxs-lookup"><span data-stu-id="82298-111">A polling trigger</span></span>  

-   <span data-ttu-id="82298-112">En push-utlösare - genom att anropa hello [arbetsflöde Service REST API](https://docs.microsoft.com/rest/api/logic/workflows)</span><span class="sxs-lookup"><span data-stu-id="82298-112">A push trigger - by calling hello [Workflow Service REST API](https://docs.microsoft.com/rest/api/logic/workflows)</span></span>  
  
<span data-ttu-id="82298-113">Alla utlösare innehålla de översta elementen:</span><span class="sxs-lookup"><span data-stu-id="82298-113">All triggers contain these top-level elements:</span></span>  
  
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

### <a name="trigger-types-and-their-inputs"></a><span data-ttu-id="82298-114">Typer av utlösare och deras indata</span><span class="sxs-lookup"><span data-stu-id="82298-114">Trigger types and their inputs</span></span>  

<span data-ttu-id="82298-115">Du kan använda dessa typer av utlösare:</span><span class="sxs-lookup"><span data-stu-id="82298-115">You can use these types of triggers:</span></span>
  
-   <span data-ttu-id="82298-116">**Begära** \- gör hello logikapp en slutpunkt för du toocall</span><span class="sxs-lookup"><span data-stu-id="82298-116">**Request** \- Makes hello logic app an endpoint for you toocall</span></span>  
  
-   <span data-ttu-id="82298-117">**Återkommande** \- utlöses baserat på ett definierat schema</span><span class="sxs-lookup"><span data-stu-id="82298-117">**Recurrence** \- Fires based on a defined schedule</span></span>  
  
-   <span data-ttu-id="82298-118">**HTTP** \- avsöker en HTTP-slutpunkt för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="82298-118">**HTTP** \- Polls an HTTP web endpoint.</span></span> <span data-ttu-id="82298-119">hello HTTP-slutpunkten måste uppfylla tooa specifik utlösande kontraktet \- antingen med hjälp av en 202\-asynk.mönster, eller genom att returnera en matris</span><span class="sxs-lookup"><span data-stu-id="82298-119">hello HTTP endpoint must conform tooa specific triggering contract \- either by using a 202\-async pattern, or by returning an array</span></span>  
  
-   <span data-ttu-id="82298-120">**ApiConnection** \- Utlös avsökningar som hello HTTP, men den kan du utnyttja hello [Microsoft-hanterade API: er](https://docs.microsoft.com/azure/connectors/apis-list)</span><span class="sxs-lookup"><span data-stu-id="82298-120">**ApiConnection** \- Polls like hello HTTP trigger, however, it takes advantage of hello [Microsoft-managed APIs](https://docs.microsoft.com/azure/connectors/apis-list)</span></span>  
  
-   <span data-ttu-id="82298-121">**HTTPWebhook** \- öppnas en slutpunkt och liknande toohello manuell utlösare, men också anropar ut tooa angivna URL: en tooregister och avregistrera</span><span class="sxs-lookup"><span data-stu-id="82298-121">**HTTPWebhook** \- Opens an endpoint, similar toohello Manual trigger, however, it also calls out tooa specified URL tooregister and unregister</span></span>  
  
-   <span data-ttu-id="82298-122">**ApiConnectionWebhook** \- fungerar som hello HTTPWebhook utlösaren genom att utnyttja hello Microsoft-hanterade API: er</span><span class="sxs-lookup"><span data-stu-id="82298-122">**ApiConnectionWebhook** \- Operates like hello HTTPWebhook trigger by taking advantage of hello Microsoft-managed APIs</span></span>       
    <span data-ttu-id="82298-123">Varje typ av utlösare som har en annan uppsättning **indata** som definierar sitt beteende.</span><span class="sxs-lookup"><span data-stu-id="82298-123">Each trigger type has a different set of **inputs** that defines its behavior.</span></span>  
  
## <a name="request-trigger"></a><span data-ttu-id="82298-124">Begäran utlösare</span><span class="sxs-lookup"><span data-stu-id="82298-124">Request trigger</span></span>  

<span data-ttu-id="82298-125">Den här utlösaren fungerar som en slutpunkt som anropas via en HTTP-begäran tooinvoke logikappen.</span><span class="sxs-lookup"><span data-stu-id="82298-125">This trigger serves as an endpoint that you call via an HTTP Request tooinvoke your logic app.</span></span> <span data-ttu-id="82298-126">Det ser ut som i det här exemplet en utlösare för begäran:</span><span class="sxs-lookup"><span data-stu-id="82298-126">A request trigger looks like this example:</span></span>  
  
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

<span data-ttu-id="82298-127">Det finns även en valfri egenskap som kallas **schemat**:</span><span class="sxs-lookup"><span data-stu-id="82298-127">There is also an optional property called **schema**:</span></span>  
  
|<span data-ttu-id="82298-128">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="82298-128">Element name</span></span>|<span data-ttu-id="82298-129">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-129">Required</span></span>|<span data-ttu-id="82298-130">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-130">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="82298-131">Schemat</span><span class="sxs-lookup"><span data-stu-id="82298-131">schema</span></span>|<span data-ttu-id="82298-132">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-132">No</span></span>|<span data-ttu-id="82298-133">En JSON-schema som validerar hello inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="82298-133">A JSON schema that validates hello incoming request.</span></span> <span data-ttu-id="82298-134">Användbart för att hjälpa efterföljande arbetsflödessteg veta vilka egenskaper tooreference.</span><span class="sxs-lookup"><span data-stu-id="82298-134">Useful for helping subsequent workflow steps know which properties tooreference.</span></span>|

<span data-ttu-id="82298-135">tooinvoke den här slutpunkten måste toocall hello *listCallbackUrl* API.</span><span class="sxs-lookup"><span data-stu-id="82298-135">tooinvoke this endpoint, you need toocall hello *listCallbackUrl* API.</span></span> <span data-ttu-id="82298-136">Se [arbetsflöde Service REST API](https://docs.microsoft.com/rest/api/logic/workflows).</span><span class="sxs-lookup"><span data-stu-id="82298-136">See [Workflow Service REST API](https://docs.microsoft.com/rest/api/logic/workflows).</span></span>  
  
## <a name="recurrence-trigger"></a><span data-ttu-id="82298-137">Utlösare för upprepning</span><span class="sxs-lookup"><span data-stu-id="82298-137">Recurrence trigger</span></span>  

<span data-ttu-id="82298-138">En utlösare för upprepning är något som körs baserat på ett definierat schema.</span><span class="sxs-lookup"><span data-stu-id="82298-138">A Recurrence trigger is one that runs based on a defined schedule.</span></span> <span data-ttu-id="82298-139">Dessa utlösare kan se ut det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="82298-139">Such a trigger might look like this example:</span></span>  

```json
"dailyReport" : {
    "type": "recurrence",
    "recurrence": {
        "frequency": "Day",
        "interval": "1"
    }
}
```

<span data-ttu-id="82298-140">Som du ser är ett enkelt sätt toorun ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="82298-140">As you can see, it is a simple way toorun a workflow.</span></span>  
  
|<span data-ttu-id="82298-141">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="82298-141">Element name</span></span>|<span data-ttu-id="82298-142">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-142">Required</span></span>|<span data-ttu-id="82298-143">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-143">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="82298-144">frequency</span><span class="sxs-lookup"><span data-stu-id="82298-144">frequency</span></span>|<span data-ttu-id="82298-145">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-145">Yes</span></span>|<span data-ttu-id="82298-146">Hur ofta hello utlösaren körs.</span><span class="sxs-lookup"><span data-stu-id="82298-146">How often hello trigger executes.</span></span> <span data-ttu-id="82298-147">Använd endast ett av dessa värden: sekund, minut, timma, dag, vecka, månad eller år</span><span class="sxs-lookup"><span data-stu-id="82298-147">Use only one of these possible values: second, minute, hour, day, week, month, or year</span></span>|  
|<span data-ttu-id="82298-148">interval</span><span class="sxs-lookup"><span data-stu-id="82298-148">interval</span></span>|<span data-ttu-id="82298-149">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-149">Yes</span></span>|<span data-ttu-id="82298-150">Hello angivna frekvensen för hello återkommande tidsintervall</span><span class="sxs-lookup"><span data-stu-id="82298-150">Interval of hello given frequency for hello recurrence</span></span>|  
|<span data-ttu-id="82298-151">startTime</span><span class="sxs-lookup"><span data-stu-id="82298-151">startTime</span></span>|<span data-ttu-id="82298-152">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-152">No</span></span>|<span data-ttu-id="82298-153">Om en startTime anges utan en UTC-förskjutningen, används den här tidszonen.</span><span class="sxs-lookup"><span data-stu-id="82298-153">If a startTime is provided without a UTC offset, this timeZone is used.</span></span>|  
|<span data-ttu-id="82298-154">Tidszon</span><span class="sxs-lookup"><span data-stu-id="82298-154">timeZone</span></span>|<span data-ttu-id="82298-155">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-155">no</span></span>|<span data-ttu-id="82298-156">Om en startTime anges utan en UTC-förskjutningen, används den här tidszonen.</span><span class="sxs-lookup"><span data-stu-id="82298-156">If a startTime is provided without a UTC offset, this timeZone is used.</span></span>|  
  
<span data-ttu-id="82298-157">Du kan även schemalägga en utlösare toostart körs på någon punkt i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="82298-157">You can also schedule a trigger toostart executing at some point in hello future.</span></span> <span data-ttu-id="82298-158">Till exempel om du vill att en vecka rapportera varje måndag toostart kan du schemalägga hello logik app toostart varje måndag genom att skapa hello följande utlösare:</span><span class="sxs-lookup"><span data-stu-id="82298-158">For example, if you want toostart a weekly report every Monday you can schedule hello logic app toostart every Monday by creating hello following trigger:</span></span>  

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

## <a name="http-trigger"></a><span data-ttu-id="82298-159">HTTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="82298-159">HTTP trigger</span></span>  

<span data-ttu-id="82298-160">HTTP-utlösare avsöker den angivna slutpunkten och kontrollera hello svar toodetermine om hello arbetsflödet ska köras.</span><span class="sxs-lookup"><span data-stu-id="82298-160">HTTP triggers poll a specified endpoint and check hello response toodetermine whether hello workflow should be executed.</span></span> <span data-ttu-id="82298-161">hello indata objektet tar hello uppsättning parametrar krävs tooconstruct ett HTTP-anrop:</span><span class="sxs-lookup"><span data-stu-id="82298-161">hello inputs object takes hello set of parameters required tooconstruct an HTTP call:</span></span>  
  
|<span data-ttu-id="82298-162">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="82298-162">Element name</span></span>|<span data-ttu-id="82298-163">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-163">Required</span></span>|<span data-ttu-id="82298-164">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-164">Description</span></span>|<span data-ttu-id="82298-165">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-165">Type</span></span>|  
|----------------|------------|---------------|--------|  
|<span data-ttu-id="82298-166">Metoden</span><span class="sxs-lookup"><span data-stu-id="82298-166">method</span></span>|<span data-ttu-id="82298-167">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-167">yes</span></span>|<span data-ttu-id="82298-168">Kan vara något av följande HTTP-metoderna hello: GET, POST, PUT, DELETE, KORRIGERINGSFIL eller HEAD</span><span class="sxs-lookup"><span data-stu-id="82298-168">Can be one of hello following HTTP methods: GET, POST, PUT, DELETE, PATCH, or HEAD</span></span>|<span data-ttu-id="82298-169">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-169">String</span></span>|  
|<span data-ttu-id="82298-170">URI: N</span><span class="sxs-lookup"><span data-stu-id="82298-170">uri</span></span>|<span data-ttu-id="82298-171">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-171">yes</span></span>|<span data-ttu-id="82298-172">hello http eller https-slutpunkt som anropas.</span><span class="sxs-lookup"><span data-stu-id="82298-172">hello http or https endpoint that is called.</span></span> <span data-ttu-id="82298-173">Högst 2 kilobyte.</span><span class="sxs-lookup"><span data-stu-id="82298-173">Maximum of 2 kilobytes.</span></span>|<span data-ttu-id="82298-174">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-174">String</span></span>|  
|<span data-ttu-id="82298-175">frågor</span><span class="sxs-lookup"><span data-stu-id="82298-175">queries</span></span>|<span data-ttu-id="82298-176">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-176">No</span></span>|<span data-ttu-id="82298-177">Ett objekt som representerar hello frågan parametrar tooadd toohello URL.</span><span class="sxs-lookup"><span data-stu-id="82298-177">An object representing hello query parameters tooadd toohello URL.</span></span> <span data-ttu-id="82298-178">Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` toohello URL.</span><span class="sxs-lookup"><span data-stu-id="82298-178">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|<span data-ttu-id="82298-179">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-179">Object</span></span>|  
|<span data-ttu-id="82298-180">Rubriker</span><span class="sxs-lookup"><span data-stu-id="82298-180">headers</span></span>|<span data-ttu-id="82298-181">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-181">No</span></span>|<span data-ttu-id="82298-182">Ett objekt som representerar alla hello-huvuden som skickas toohello begäran.</span><span class="sxs-lookup"><span data-stu-id="82298-182">An object representing each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="82298-183">Till exempel tooset hello språk och typen på en begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="82298-183">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|<span data-ttu-id="82298-184">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-184">Object</span></span>|  
|<span data-ttu-id="82298-185">Brödtext</span><span class="sxs-lookup"><span data-stu-id="82298-185">body</span></span>|<span data-ttu-id="82298-186">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-186">No</span></span>|<span data-ttu-id="82298-187">Ett objekt som representerar hello-nyttolast som har skickats toohello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="82298-187">An object representing hello payload that is sent toohello endpoint.</span></span>|<span data-ttu-id="82298-188">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-188">Object</span></span>|  
|<span data-ttu-id="82298-189">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="82298-189">retryPolicy</span></span>|<span data-ttu-id="82298-190">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-190">No</span></span>|<span data-ttu-id="82298-191">Ett objekt som kan du anpassa hello försök beteende för 4xx eller 5xx-fel.</span><span class="sxs-lookup"><span data-stu-id="82298-191">An object that lets you customize hello retry behavior for 4xx or 5xx errors.</span></span>|<span data-ttu-id="82298-192">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-192">Object</span></span>|  
|<span data-ttu-id="82298-193">Autentisering</span><span class="sxs-lookup"><span data-stu-id="82298-193">authentication</span></span>|<span data-ttu-id="82298-194">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-194">No</span></span>|<span data-ttu-id="82298-195">Representerar hello metod som hello begäran ska autentiseras.</span><span class="sxs-lookup"><span data-stu-id="82298-195">Represents hello method that hello request should be authenticated.</span></span> <span data-ttu-id="82298-196">Mer information om det här objektet finns [Scheduler utgående autentisering](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span><span class="sxs-lookup"><span data-stu-id="82298-196">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span></span> <span data-ttu-id="82298-197">Utöver scheduler, är en mer stöds egenskap: `authority` det här värdet är som standard `https://login.windows.net` om anges, men du kan använda en annan publik som`https://login.windows\-ppe.net`</span><span class="sxs-lookup"><span data-stu-id="82298-197">Beyond scheduler, there is one more supported property: `authority` By default, this value is `https://login.windows.net` when not specified, but you can use a different audience like `https://login.windows\-ppe.net`</span></span>|<span data-ttu-id="82298-198">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-198">Object</span></span>|  
  
<span data-ttu-id="82298-199">hello HTTP-utlösaren kräver hello HTTP API tooconform med ett specifikt mönster toowork tillsammans med din logikapp.</span><span class="sxs-lookup"><span data-stu-id="82298-199">hello HTTP trigger requires hello HTTP API tooconform with a specific pattern toowork well with your logic app.</span></span> <span data-ttu-id="82298-200">Det krävs hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="82298-200">It requires hello following fields:</span></span>  
  
|<span data-ttu-id="82298-201">Svar</span><span class="sxs-lookup"><span data-stu-id="82298-201">Response</span></span>|<span data-ttu-id="82298-202">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-202">Description</span></span>|  
|------------|---------------|  
|<span data-ttu-id="82298-203">Statuskod</span><span class="sxs-lookup"><span data-stu-id="82298-203">Status code</span></span>|<span data-ttu-id="82298-204">Statuskod 200 \(OK\) toocause körs.</span><span class="sxs-lookup"><span data-stu-id="82298-204">Status code 200 \(OK\) toocause a run.</span></span> <span data-ttu-id="82298-205">Andra statuskod göra inte en körning.</span><span class="sxs-lookup"><span data-stu-id="82298-205">Any other status code doesn't cause a run.</span></span>|  
|<span data-ttu-id="82298-206">Försök\-efter huvudet</span><span class="sxs-lookup"><span data-stu-id="82298-206">Retry\-after header</span></span>|<span data-ttu-id="82298-207">Antalet sekunder tills hello logikapp avsöker hello slutpunkt igen.</span><span class="sxs-lookup"><span data-stu-id="82298-207">Number of seconds until hello logic app polls hello endpoint again.</span></span>|  
|<span data-ttu-id="82298-208">Platshuvud</span><span class="sxs-lookup"><span data-stu-id="82298-208">Location header</span></span>|<span data-ttu-id="82298-209">hello URL toocall på hello nästa avsökningsintervall.</span><span class="sxs-lookup"><span data-stu-id="82298-209">hello URL toocall on hello next polling interval.</span></span> <span data-ttu-id="82298-210">Om inget anges används hello ursprungliga URL: en.</span><span class="sxs-lookup"><span data-stu-id="82298-210">If not specified, hello original URL is used.</span></span>|  
  
<span data-ttu-id="82298-211">Här följer några exempel på olika beteenden för olika typer av begäranden:</span><span class="sxs-lookup"><span data-stu-id="82298-211">Here are some examples of different behaviors for different types of requests:</span></span>  
  
|<span data-ttu-id="82298-212">Svarskod</span><span class="sxs-lookup"><span data-stu-id="82298-212">Response code</span></span>|<span data-ttu-id="82298-213">Försök\-efter</span><span class="sxs-lookup"><span data-stu-id="82298-213">Retry\-After</span></span>|<span data-ttu-id="82298-214">Beteende</span><span class="sxs-lookup"><span data-stu-id="82298-214">Behavior</span></span>|  
|-----------------|----------------|------------|  
|<span data-ttu-id="82298-215">200</span><span class="sxs-lookup"><span data-stu-id="82298-215">200</span></span>|<span data-ttu-id="82298-216">\(Ingen\)</span><span class="sxs-lookup"><span data-stu-id="82298-216">\(none\)</span></span>|<span data-ttu-id="82298-217">Inte en giltig utlösare, försök igen\-efter är obligatorisk eller annan hello motor aldrig söker efter nästa hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="82298-217">Not a valid trigger, Retry\-After is required, or else hello engine never polls for hello next request.</span></span>|  
|<span data-ttu-id="82298-218">202</span><span class="sxs-lookup"><span data-stu-id="82298-218">202</span></span>|<span data-ttu-id="82298-219">60</span><span class="sxs-lookup"><span data-stu-id="82298-219">60</span></span>|<span data-ttu-id="82298-220">Utlöses inte hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="82298-220">Do not trigger hello workflow.</span></span> <span data-ttu-id="82298-221">hello försöker sker i en minut.</span><span class="sxs-lookup"><span data-stu-id="82298-221">hello next attempt happens in one minute.</span></span>|  
|<span data-ttu-id="82298-222">200</span><span class="sxs-lookup"><span data-stu-id="82298-222">200</span></span>|<span data-ttu-id="82298-223">10</span><span class="sxs-lookup"><span data-stu-id="82298-223">10</span></span>|<span data-ttu-id="82298-224">Kör hello arbetsflöde och Sök igen efter mer innehåll om 10 sekunder.</span><span class="sxs-lookup"><span data-stu-id="82298-224">Run hello workflow, and check again for more content in 10 seconds.</span></span>|  
|<span data-ttu-id="82298-225">400</span><span class="sxs-lookup"><span data-stu-id="82298-225">400</span></span>|<span data-ttu-id="82298-226">\(Ingen\)</span><span class="sxs-lookup"><span data-stu-id="82298-226">\(none\)</span></span>|<span data-ttu-id="82298-227">Felaktig begäran körs inte hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="82298-227">Bad request, do not run hello workflow.</span></span> <span data-ttu-id="82298-228">Om det finns inga **försök princip** definierats hello standardprincipen används.</span><span class="sxs-lookup"><span data-stu-id="82298-228">If there is no **Retry Policy** defined, then hello default policy is used.</span></span> <span data-ttu-id="82298-229">När hello antalet försök har nåtts, är hello utlösare inte längre giltig.</span><span class="sxs-lookup"><span data-stu-id="82298-229">After hello number of retries has been reached, hello trigger is no longer valid.</span></span>|  
|<span data-ttu-id="82298-230">500</span><span class="sxs-lookup"><span data-stu-id="82298-230">500</span></span>|<span data-ttu-id="82298-231">\(Ingen\)</span><span class="sxs-lookup"><span data-stu-id="82298-231">\(none\)</span></span>|<span data-ttu-id="82298-232">Hello arbetsflödet körs inte på Server-fel.</span><span class="sxs-lookup"><span data-stu-id="82298-232">Server error, do not run hello workflow.</span></span>  <span data-ttu-id="82298-233">Om det finns inga **försök princip** definierats hello standardprincipen används.</span><span class="sxs-lookup"><span data-stu-id="82298-233">If there is no **Retry Policy** defined, then hello default policy is used.</span></span> <span data-ttu-id="82298-234">När hello antalet försök har nåtts, är hello utlösare inte längre giltig.</span><span class="sxs-lookup"><span data-stu-id="82298-234">After hello number of retries has been reached, hello trigger is no longer valid.</span></span>|  
  
<span data-ttu-id="82298-235">Det ser ut som i det här exemplet Hello utdata för en HTTP-utlösare:</span><span class="sxs-lookup"><span data-stu-id="82298-235">hello outputs of an HTTP trigger look like this example:</span></span>  
  
|<span data-ttu-id="82298-236">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="82298-236">Element name</span></span>|<span data-ttu-id="82298-237">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-237">Description</span></span>|<span data-ttu-id="82298-238">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-238">Type</span></span>|  
|----------------|---------------|--------|  
|<span data-ttu-id="82298-239">Rubriker</span><span class="sxs-lookup"><span data-stu-id="82298-239">headers</span></span>|<span data-ttu-id="82298-240">hello-huvuden hello http-svar.</span><span class="sxs-lookup"><span data-stu-id="82298-240">hello headers of hello http response.</span></span>|<span data-ttu-id="82298-241">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-241">Object</span></span>|  
|<span data-ttu-id="82298-242">Brödtext</span><span class="sxs-lookup"><span data-stu-id="82298-242">body</span></span>|<span data-ttu-id="82298-243">hello brödtext hello http-svar.</span><span class="sxs-lookup"><span data-stu-id="82298-243">hello body of hello http response.</span></span>|<span data-ttu-id="82298-244">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-244">Object</span></span>|  
  
## <a name="api-connection-trigger"></a><span data-ttu-id="82298-245">Utlösare för API-anslutningen</span><span class="sxs-lookup"><span data-stu-id="82298-245">API Connection trigger</span></span>  

<span data-ttu-id="82298-246">hello API anslutning utlösaren är liknande toohello http-utlösaren i dess grundläggande funktioner.</span><span class="sxs-lookup"><span data-stu-id="82298-246">hello API connection trigger is similar toohello HTTP trigger in its basic functionality.</span></span> <span data-ttu-id="82298-247">Dock är hello parametrar för att identifiera hello åtgärd olika.</span><span class="sxs-lookup"><span data-stu-id="82298-247">However, hello parameters for identifying hello action are different.</span></span> <span data-ttu-id="82298-248">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="82298-248">Here is an example:</span></span>  
  
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

|<span data-ttu-id="82298-249">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="82298-249">Element name</span></span>|<span data-ttu-id="82298-250">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-250">Required</span></span>|<span data-ttu-id="82298-251">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-251">Type</span></span>|<span data-ttu-id="82298-252">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-252">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="82298-253">värden</span><span class="sxs-lookup"><span data-stu-id="82298-253">host</span></span>|<span data-ttu-id="82298-254">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-254">Yes</span></span>||<span data-ttu-id="82298-255">Hej ApiApp finns gateway och ID: t.</span><span class="sxs-lookup"><span data-stu-id="82298-255">hello ApiApp hosted gateway and id.</span></span>|  
|<span data-ttu-id="82298-256">Metoden</span><span class="sxs-lookup"><span data-stu-id="82298-256">method</span></span>|<span data-ttu-id="82298-257">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-257">Yes</span></span>|<span data-ttu-id="82298-258">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-258">String</span></span>|<span data-ttu-id="82298-259">Kan vara något av följande HTTP-metoderna hello: **hämta**, **POST**, **PLACERA**, **ta bort**, **korrigering**, eller  **HUVUDET**</span><span class="sxs-lookup"><span data-stu-id="82298-259">Can be one of hello following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="82298-260">frågor</span><span class="sxs-lookup"><span data-stu-id="82298-260">queries</span></span>|<span data-ttu-id="82298-261">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-261">No</span></span>|<span data-ttu-id="82298-262">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-262">Object</span></span>|<span data-ttu-id="82298-263">Representerar hello frågan parametrar toobe lägga toohello URL.</span><span class="sxs-lookup"><span data-stu-id="82298-263">Represents hello query parameters toobe added toohello URL.</span></span> <span data-ttu-id="82298-264">Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` toohello URL.</span><span class="sxs-lookup"><span data-stu-id="82298-264">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|  
|<span data-ttu-id="82298-265">Rubriker</span><span class="sxs-lookup"><span data-stu-id="82298-265">headers</span></span>|<span data-ttu-id="82298-266">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-266">No</span></span>|<span data-ttu-id="82298-267">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-267">Object</span></span>|<span data-ttu-id="82298-268">Representerar varje hello-huvuden som skickas toohello begäran.</span><span class="sxs-lookup"><span data-stu-id="82298-268">Represents each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="82298-269">Till exempel tooset hello språk och typen på en begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="82298-269">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="82298-270">Brödtext</span><span class="sxs-lookup"><span data-stu-id="82298-270">body</span></span>|<span data-ttu-id="82298-271">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-271">No</span></span>|<span data-ttu-id="82298-272">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-272">Object</span></span>|<span data-ttu-id="82298-273">Representerar hello-nyttolast som har skickats toohello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="82298-273">Represents hello payload that is sent toohello endpoint.</span></span>|  
|<span data-ttu-id="82298-274">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="82298-274">retryPolicy</span></span>|<span data-ttu-id="82298-275">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-275">No</span></span>|<span data-ttu-id="82298-276">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-276">Object</span></span>|<span data-ttu-id="82298-277">Låter dig toocustomize hello försök beteende för 4xx eller 5xx-fel.</span><span class="sxs-lookup"><span data-stu-id="82298-277">Allows you toocustomize hello retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="82298-278">Autentisering</span><span class="sxs-lookup"><span data-stu-id="82298-278">authentication</span></span>|<span data-ttu-id="82298-279">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-279">No</span></span>|<span data-ttu-id="82298-280">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-280">Object</span></span>|<span data-ttu-id="82298-281">Representerar hello metod som hello begäran ska autentiseras.</span><span class="sxs-lookup"><span data-stu-id="82298-281">Represents hello method that hello request should be authenticated.</span></span> <span data-ttu-id="82298-282">Mer information om det här objektet finns [Scheduler utgående autentisering](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)</span><span class="sxs-lookup"><span data-stu-id="82298-282">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)</span></span>|  
  
<span data-ttu-id="82298-283">hello egenskaper för värden är:</span><span class="sxs-lookup"><span data-stu-id="82298-283">hello properties for host are:</span></span>  
  
|<span data-ttu-id="82298-284">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="82298-284">Element name</span></span>|<span data-ttu-id="82298-285">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-285">Required</span></span>|<span data-ttu-id="82298-286">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-286">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="82298-287">API-runtimeUrl</span><span class="sxs-lookup"><span data-stu-id="82298-287">api runtimeUrl</span></span>|<span data-ttu-id="82298-288">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-288">Yes</span></span>|<span data-ttu-id="82298-289">hello-slutpunkten för hello hanterade API.</span><span class="sxs-lookup"><span data-stu-id="82298-289">hello endpoint of hello managed API.</span></span>|  
|<span data-ttu-id="82298-290">Anslutningens namn</span><span class="sxs-lookup"><span data-stu-id="82298-290">connection name</span></span>||<span data-ttu-id="82298-291">Måste en referens tooa parameter anropas `$connection` och är hello på hello hanterade API-anslutningen som hello arbetsflödet använder.</span><span class="sxs-lookup"><span data-stu-id="82298-291">Must be a reference tooa parameter called `$connection` and is hello name of hello managed API connection that hello workflow uses.</span></span>|
  
<span data-ttu-id="82298-292">hello utdata för en utlösare för API-anslutning är:</span><span class="sxs-lookup"><span data-stu-id="82298-292">hello outputs of an API connection trigger are:</span></span>
  
|<span data-ttu-id="82298-293">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="82298-293">Element name</span></span>|<span data-ttu-id="82298-294">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-294">Type</span></span>|<span data-ttu-id="82298-295">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-295">Description</span></span>|  
|----------------|--------|---------------|  
|<span data-ttu-id="82298-296">Rubriker</span><span class="sxs-lookup"><span data-stu-id="82298-296">headers</span></span>|<span data-ttu-id="82298-297">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-297">Object</span></span>|<span data-ttu-id="82298-298">hello-huvuden hello http-svar.</span><span class="sxs-lookup"><span data-stu-id="82298-298">hello headers of hello http response.</span></span>|  
|<span data-ttu-id="82298-299">Brödtext</span><span class="sxs-lookup"><span data-stu-id="82298-299">body</span></span>|<span data-ttu-id="82298-300">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-300">Object</span></span>|<span data-ttu-id="82298-301">hello brödtext hello http-svar.</span><span class="sxs-lookup"><span data-stu-id="82298-301">hello body of hello http response.</span></span>|  
  
## <a name="httpwebhook-trigger"></a><span data-ttu-id="82298-302">HTTPWebhook utlösare</span><span class="sxs-lookup"><span data-stu-id="82298-302">HTTPWebhook trigger</span></span>  

<span data-ttu-id="82298-303">Hej HTTPWebhook utlösaren öppnas en slutpunkt, liknande toohello manuell utlösare, men hello HTTPWebhook utlösaren anropar också ut tooa angivna URL: en tooregister och avregistrera.</span><span class="sxs-lookup"><span data-stu-id="82298-303">hello HTTPWebhook trigger opens an endpoint, similar toohello manual trigger, but hello HTTPWebhook trigger also calls out tooa specified URL tooregister and unregister.</span></span> <span data-ttu-id="82298-304">Här är ett exempel på hur en HTTPWebhook-utlösare kan se ut:</span><span class="sxs-lookup"><span data-stu-id="82298-304">Here's an example of what an HTTPWebhook trigger might look like:</span></span>  

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

<span data-ttu-id="82298-305">Många av dessa avsnitt är valfria och hello beteendet för hello Webhook beror på vilka avsnitt som eller utelämnas.</span><span class="sxs-lookup"><span data-stu-id="82298-305">Many of these sections are optional, and hello behavior of hello Webhook depends on which sections are provided or omitted.</span></span>  
<span data-ttu-id="82298-306">hello egenskaperna för en Webhook är följande:</span><span class="sxs-lookup"><span data-stu-id="82298-306">hello properties of a Webhook are as follows:</span></span>  
  
|<span data-ttu-id="82298-307">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="82298-307">Element name</span></span>|<span data-ttu-id="82298-308">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-308">Required</span></span>|<span data-ttu-id="82298-309">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-309">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="82298-310">prenumerera på</span><span class="sxs-lookup"><span data-stu-id="82298-310">subscribe</span></span>|<span data-ttu-id="82298-311">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-311">No</span></span>|<span data-ttu-id="82298-312">hello utgående begäran som anropas när hello utlösare skapas och utför hello registreringen.</span><span class="sxs-lookup"><span data-stu-id="82298-312">hello outgoing request that is called when hello trigger is created and performs hello initial registration.</span></span>|  
|<span data-ttu-id="82298-313">avbryta prenumerationen</span><span class="sxs-lookup"><span data-stu-id="82298-313">unsubscribe</span></span>|<span data-ttu-id="82298-314">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-314">No</span></span>|<span data-ttu-id="82298-315">hello utgående begäran när hello utlösaren tas bort.</span><span class="sxs-lookup"><span data-stu-id="82298-315">hello outgoing request when hello trigger is deleted.</span></span>|  
  
-   <span data-ttu-id="82298-316">**Prenumerera på** är hello utgående anrop som har gjorts toostart lyssnande tooevents.</span><span class="sxs-lookup"><span data-stu-id="82298-316">**Subscribe** is hello outgoing call that's made toostart listening tooevents.</span></span> <span data-ttu-id="82298-317">Det här anropet börjar med hello har samma uppsättning parametrar som hello vanliga HTTP-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="82298-317">This call starts with hello same set of parameters that hello normal HTTP actions do.</span></span> <span data-ttu-id="82298-318">Utgående anropet görs varje gång hello ändringar av arbetsflödet på något sätt, till exempel när hello autentiseringsuppgifter samlas eller hello utlösaren input parametrar ändringen.</span><span class="sxs-lookup"><span data-stu-id="82298-318">This outgoing call is made any time hello workflow changes in any way, for example, whenever hello credentials are rolled, or hello trigger's input parameters change.</span></span>
  
    <span data-ttu-id="82298-319">toosupport detta anrop, det finns en ny funktion: `@listCallbackUrl()`.</span><span class="sxs-lookup"><span data-stu-id="82298-319">toosupport this call, there is a new function: `@listCallbackUrl()`.</span></span> <span data-ttu-id="82298-320">Den här funktionen returnerar en unik URL för den här specifika utlösaren i det här arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="82298-320">This function returns a unique URL for this specific trigger in this workflow.</span></span> <span data-ttu-id="82298-321">Det motsvarar hello unika identifieraren för hello-slutpunkter som använder hello Service REST.</span><span class="sxs-lookup"><span data-stu-id="82298-321">It represents hello unique identifier for hello endpoints that use hello Service REST.</span></span>  
  
-   <span data-ttu-id="82298-322">**Avbryta prenumerationen** anropas när en åtgärd återgivningar utlösaren är ogiltig, inklusive:</span><span class="sxs-lookup"><span data-stu-id="82298-322">**Unsubscribe** is called when an operation renders this trigger invalid, including:</span></span>  
  
    -   <span data-ttu-id="82298-323">Ta bort eller inaktivera hello utlösare</span><span class="sxs-lookup"><span data-stu-id="82298-323">Deleting or disabling hello trigger</span></span>  
  
    -   <span data-ttu-id="82298-324">Ta bort eller inaktivera hello-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="82298-324">Deleting or disabling hello workflow</span></span>  
  
    -   <span data-ttu-id="82298-325">Ta bort eller inaktivera hello prenumeration</span><span class="sxs-lookup"><span data-stu-id="82298-325">Deleting or disabling hello subscription</span></span>  
  
    <span data-ttu-id="82298-326">Hej logikapp automatiskt anropar hello avbryta prenumerationen åtgärd.</span><span class="sxs-lookup"><span data-stu-id="82298-326">hello logic app automatically calls hello unsubscribe action.</span></span> <span data-ttu-id="82298-327">hello parametrar toothis funktion är hello samma som hello http-utlösare.</span><span class="sxs-lookup"><span data-stu-id="82298-327">hello parameters toothis function are hello same as hello HTTP trigger.</span></span>  
  
    <span data-ttu-id="82298-328">hello är utdata för hello HTTPWebhook utlösare hello innehållet i hello inkommande begäran:</span><span class="sxs-lookup"><span data-stu-id="82298-328">hello outputs of hello HTTPWebhook trigger are hello contents of hello incoming request:</span></span>  
  
|<span data-ttu-id="82298-329">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="82298-329">Element name</span></span>|<span data-ttu-id="82298-330">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-330">Type</span></span>|<span data-ttu-id="82298-331">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-331">Description</span></span>|  
|-----------------|--------|---------------|  
|<span data-ttu-id="82298-332">Rubriker</span><span class="sxs-lookup"><span data-stu-id="82298-332">headers</span></span>|<span data-ttu-id="82298-333">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-333">Object</span></span>|<span data-ttu-id="82298-334">hello-huvuden hello HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="82298-334">hello headers of hello http request.</span></span>|  
|<span data-ttu-id="82298-335">Brödtext</span><span class="sxs-lookup"><span data-stu-id="82298-335">body</span></span>|<span data-ttu-id="82298-336">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-336">Object</span></span>|<span data-ttu-id="82298-337">hello brödtext hello HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="82298-337">hello body of hello http request.</span></span>|  

<span data-ttu-id="82298-338">Gränserna för ett webhook-åtgärden kan anges i hello samma sätt som [HTTP asynkron gränser](#asynchronous-limits).</span><span class="sxs-lookup"><span data-stu-id="82298-338">Limits on a webhook action can be specified in hello same manner as [HTTP Asynchronous Limits](#asynchronous-limits).</span></span>
  

## <a name="conditions"></a><span data-ttu-id="82298-339">Villkor</span><span class="sxs-lookup"><span data-stu-id="82298-339">Conditions</span></span>  

<span data-ttu-id="82298-340">Du kan använda en eller flera villkor toodetermine om hello arbetsflödet ska köras eller inte för alla utlösare.</span><span class="sxs-lookup"><span data-stu-id="82298-340">For any trigger, you can use one or more conditions toodetermine whether hello workflow should run or not.</span></span> <span data-ttu-id="82298-341">Exempel:</span><span class="sxs-lookup"><span data-stu-id="82298-341">For example:</span></span>  

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

<span data-ttu-id="82298-342">I det här fallet hello rapporten endast utlösare när hello arbetsflöde `sendReports` tootrue är angiven.</span><span class="sxs-lookup"><span data-stu-id="82298-342">In this case, hello report only triggers while hello workflow's `sendReports` parameter is set tootrue.</span></span> <span data-ttu-id="82298-343">Slutligen får villkor referera hello statuskod hello utlösare.</span><span class="sxs-lookup"><span data-stu-id="82298-343">Finally, conditions may reference hello status code of hello trigger.</span></span> <span data-ttu-id="82298-344">Exempelvis kan startar du ett arbetsflöde endast när din webbplats returnerar en statuskod 500, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="82298-344">For example, you could kick off a workflow only when your website returns a status code 500, as follows:</span></span>
  
```  
"conditions": [  
        {  
          "expression": "@equals(triggers().code, 'InternalServerError')"  
        }  
      ]  
```  
  
> [!NOTE]  
> <span data-ttu-id="82298-345">När ett uttryck som refererar till hello statuskod hello utlösare \(på något sätt\), hello standardbeteendet \(utlösaren endast på 200 \(OK\) \) ersätts.</span><span class="sxs-lookup"><span data-stu-id="82298-345">When any expression references hello status code of hello trigger \(in any way\), hello default behavior \(trigger only on 200 \(OK\)\) is replaced.</span></span> <span data-ttu-id="82298-346">Till exempel om du vill tootrigger både statuskod 200 och statuskod 201 ha tooinclude: `@or(equals(triggers().code, 200),equals(triggers().code,201))` som dina villkor.</span><span class="sxs-lookup"><span data-stu-id="82298-346">For example, if you want tootrigger on both status code 200 and status code 201, you have tooinclude: `@or(equals(triggers().code, 200),equals(triggers().code,201))` as your condition.</span></span>  
  
## <a name="start-multiple-runs-for-a-request"></a><span data-ttu-id="82298-347">Starta flera körs för en begäran</span><span class="sxs-lookup"><span data-stu-id="82298-347">Start multiple runs for a request</span></span>

<span data-ttu-id="82298-348">tookick av flera körningar för en enskild begäran `splitOn` är användbart, till exempel om du vill toopoll en slutpunkt som kan ha flera nya objekt mellan avsökningsintervall.</span><span class="sxs-lookup"><span data-stu-id="82298-348">tookick off multiple runs for a single request, `splitOn` is useful, for example, when you want toopoll an endpoint that can have multiple new items between polling intervals.</span></span>
  
<span data-ttu-id="82298-349">Med `splitOn`, du anger hello-egenskapen i hello svar-nyttolast som innehåller hello matris av objekt, som du vill toouse toostart hello utlösaren körs.</span><span class="sxs-lookup"><span data-stu-id="82298-349">With `splitOn`, you specify hello property inside hello response payload that contains hello array of items, each of which you want toouse toostart a run of hello trigger.</span></span> <span data-ttu-id="82298-350">Anta att du har en API som returnerar hello efter svar:</span><span class="sxs-lookup"><span data-stu-id="82298-350">For example, imagine you have an API that returns hello following response:</span></span>  
  
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
  
<span data-ttu-id="82298-351">Din logikapp behöver endast hello rader innehåll, så du kan skapa en utlösare som det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="82298-351">Your logic app only needs hello Rows content, so you can construct your trigger like this example:</span></span>  
  
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
  
<span data-ttu-id="82298-352">Sedan hello arbetsflödesdefinitionen `@triggerBody().name` returnerar `mycoolrow` för hello först köra och `another row` för hello andra kör.</span><span class="sxs-lookup"><span data-stu-id="82298-352">Then, in hello workflow definition, `@triggerBody().name` returns `mycoolrow` for hello first run, and `another row` for hello second run.</span></span> <span data-ttu-id="82298-353">hello utlösaren utdata ser ut som i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="82298-353">hello trigger outputs look like this example:</span></span>  
  
```json
{
    "body" : {
        "id" : 938109381,
        "name" : "another row"
    }
}
```

<span data-ttu-id="82298-354">Om du använder `SplitOn`, du kan inte hämta hello egenskaper som är utanför hello matris i det här fallet hello `Status` fältet.</span><span class="sxs-lookup"><span data-stu-id="82298-354">So if you use `SplitOn`, you can't get hello properties that are outside hello array, in this case, hello `Status` field.</span></span>  
  
> [!NOTE]  
> <span data-ttu-id="82298-355">I det här exemplet använder vi hello `?` operatorn toobe kan tooavoid ett fel om hello `Rows` egenskapen finns inte.</span><span class="sxs-lookup"><span data-stu-id="82298-355">In this example, we use hello `?` operator toobe able tooavoid a failure if hello `Rows` property is not present.</span></span> 
  
## <a name="single-run-instance"></a><span data-ttu-id="82298-356">Kör instans</span><span class="sxs-lookup"><span data-stu-id="82298-356">Single run instance</span></span>

<span data-ttu-id="82298-357">Du kan konfigurera utlösare som har en upprepning egenskapen tooonly brand om alla aktiva körningar har slutförts.</span><span class="sxs-lookup"><span data-stu-id="82298-357">You can configure triggers that have a recurrence property tooonly fire if all active runs have completed.</span></span> <span data-ttu-id="82298-358">Om en schemalagd upprepning inträffar när det finns en pågående kör hoppar över hello utlösare och väntar tills hello nästa schemalagda återkommande intervall toocheck igen.</span><span class="sxs-lookup"><span data-stu-id="82298-358">If a scheduled recurrence occurs while there is an in-progress run, hello trigger skips and waits until hello next scheduled recurrence interval toocheck again.</span></span>

<span data-ttu-id="82298-359">Du kan konfigurera den här inställningen via hello åtgärden alternativ:</span><span class="sxs-lookup"><span data-stu-id="82298-359">You can configure this setting through hello operation options:</span></span>

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

## <a name="types-and-inputs"></a><span data-ttu-id="82298-360">Typer och indata</span><span class="sxs-lookup"><span data-stu-id="82298-360">Types and inputs</span></span>  

<span data-ttu-id="82298-361">Det finns många typer av åtgärder med unika beteende.</span><span class="sxs-lookup"><span data-stu-id="82298-361">There are many types of actions, each with unique behavior.</span></span> <span data-ttu-id="82298-362">Åtgärder för samlingen kan innehålla flera andra åtgärder i sig själv.</span><span class="sxs-lookup"><span data-stu-id="82298-362">Collection actions may contain many other actions within itself.</span></span>

### <a name="standard-actions"></a><span data-ttu-id="82298-363">Standardåtgärder</span><span class="sxs-lookup"><span data-stu-id="82298-363">Standard actions</span></span>  

-   <span data-ttu-id="82298-364">**HTTP** den här åtgärden kräver en HTTP-slutpunkt för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="82298-364">**HTTP** This action calls an HTTP web endpoint.</span></span>  
  
-   <span data-ttu-id="82298-365">**ApiConnection** \- åtgärden fungerar som en hello HTTP-åtgärd, men använder hello Microsoft-hanterade API: er.</span><span class="sxs-lookup"><span data-stu-id="82298-365">**ApiConnection** \- This action behaves like hello HTTP action, but uses hello Microsoft-managed APIs.</span></span>  
  
-   <span data-ttu-id="82298-366">**ApiConnectionWebhook** \- som HTTPWebhook, men använder hello Microsoft-hanterade API: er.</span><span class="sxs-lookup"><span data-stu-id="82298-366">**ApiConnectionWebhook** \- Like HTTPWebhook, but uses hello Microsoft-managed APIs.</span></span>  
  
-   <span data-ttu-id="82298-367">**Svaret** \- åtgärden definierar ett svar för ett inkommande samtal.</span><span class="sxs-lookup"><span data-stu-id="82298-367">**Response** \- This action defines a response for an incoming call.</span></span>  
  
-   <span data-ttu-id="82298-368">**Vänta** \- åtgärden enkel väntar på ett fast belopp tid eller tills en viss tid.</span><span class="sxs-lookup"><span data-stu-id="82298-368">**Wait** \- This simple action waits a fixed amount of time or until a specific time.</span></span>  
  
-   <span data-ttu-id="82298-369">**Arbetsflödet** \- representerar ett inkapslat arbetsflöde för den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="82298-369">**Workflow** \- This action represents a nested workflow.</span></span>  

-   <span data-ttu-id="82298-370">**Funktionen** \- åtgärden representerar en Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="82298-370">**Function** \- This action represents an Azure Function.</span></span>

### <a name="collection-actions"></a><span data-ttu-id="82298-371">Åtgärder för samlingen</span><span class="sxs-lookup"><span data-stu-id="82298-371">Collection actions</span></span>

-   <span data-ttu-id="82298-372">**Scope** \- den här åtgärden är en logisk gruppering av andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="82298-372">**Scope** \- This action is a logical grouping of other actions.</span></span>

-   <span data-ttu-id="82298-373">**Villkoret** \- åtgärden utvärderar ett uttryck och kör hello motsvarande resultatet grenen.</span><span class="sxs-lookup"><span data-stu-id="82298-373">**Condition** \- This action evaluates an expression and executes hello corresponding result branch.</span></span>

-   <span data-ttu-id="82298-374">**ForEach** \- slingor åtgärden upprepas i en matris och utför inre åtgärder för varje objekt.</span><span class="sxs-lookup"><span data-stu-id="82298-374">**ForEach** \- This looping action iterates through an array and performs inner actions for each item.</span></span>

-   <span data-ttu-id="82298-375">**Tills** \- åtgärden slingor kör inre åtgärder tills ett villkor resulterar tootrue.</span><span class="sxs-lookup"><span data-stu-id="82298-375">**Until** \- This looping action executes inner actions until a condition results tootrue.</span></span>
  
<span data-ttu-id="82298-376">Varje typ av åtgärd har en annan uppsättning **indata** som definierar beteendet för en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="82298-376">Each type of action has a different set of **inputs** that define an action's behavior.</span></span>  
  
## <a name="http-action"></a><span data-ttu-id="82298-377">HTTP-åtgärd</span><span class="sxs-lookup"><span data-stu-id="82298-377">HTTP action</span></span>  

<span data-ttu-id="82298-378">HTTP-åtgärder anropa den angivna slutpunkten och kontrollera hello svar toodetermine om hello arbetsflödet ska köras.</span><span class="sxs-lookup"><span data-stu-id="82298-378">HTTP actions call a specified endpoint and check hello response toodetermine whether hello workflow should run.</span></span> <span data-ttu-id="82298-379">Hej **indata** objektet tar hello uppsättning parametrar krävs tooconstruct hello HTTP anropet:</span><span class="sxs-lookup"><span data-stu-id="82298-379">hello **inputs** object takes hello set of parameters required tooconstruct hello HTTP call:</span></span>  
  
|<span data-ttu-id="82298-380">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="82298-380">Element name</span></span>|<span data-ttu-id="82298-381">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-381">Required</span></span>|<span data-ttu-id="82298-382">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-382">Type</span></span>|<span data-ttu-id="82298-383">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-383">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="82298-384">Metoden</span><span class="sxs-lookup"><span data-stu-id="82298-384">method</span></span>|<span data-ttu-id="82298-385">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-385">Yes</span></span>|<span data-ttu-id="82298-386">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-386">String</span></span>|<span data-ttu-id="82298-387">Kan vara något av följande HTTP-metoderna hello: **hämta**, **POST**, **PLACERA**, **ta bort**, **korrigering**, eller  **HUVUDET**</span><span class="sxs-lookup"><span data-stu-id="82298-387">Can be one of hello following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="82298-388">URI: N</span><span class="sxs-lookup"><span data-stu-id="82298-388">uri</span></span>|<span data-ttu-id="82298-389">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-389">Yes</span></span>|<span data-ttu-id="82298-390">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-390">String</span></span>|<span data-ttu-id="82298-391">hello http eller https-slutpunkt som anropas.</span><span class="sxs-lookup"><span data-stu-id="82298-391">hello http or https endpoint that is called.</span></span> <span data-ttu-id="82298-392">Maximal längd är 2 kB.</span><span class="sxs-lookup"><span data-stu-id="82298-392">Maximum length is 2 kilobytes.</span></span>|  
|<span data-ttu-id="82298-393">frågor</span><span class="sxs-lookup"><span data-stu-id="82298-393">queries</span></span>|<span data-ttu-id="82298-394">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-394">No</span></span>|<span data-ttu-id="82298-395">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-395">Object</span></span>|<span data-ttu-id="82298-396">Representerar hello frågan parametrar tooadd toohello URL.</span><span class="sxs-lookup"><span data-stu-id="82298-396">Represents hello query parameters tooadd toohello URL.</span></span> <span data-ttu-id="82298-397">Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` toohello URL.</span><span class="sxs-lookup"><span data-stu-id="82298-397">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|  
|<span data-ttu-id="82298-398">Rubriker</span><span class="sxs-lookup"><span data-stu-id="82298-398">headers</span></span>|<span data-ttu-id="82298-399">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-399">No</span></span>|<span data-ttu-id="82298-400">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-400">Object</span></span>|<span data-ttu-id="82298-401">Representerar varje hello-huvuden som skickas toohello begäran.</span><span class="sxs-lookup"><span data-stu-id="82298-401">Represents each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="82298-402">Till exempel tooset hello språk och typen på en begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="82298-402">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="82298-403">Brödtext</span><span class="sxs-lookup"><span data-stu-id="82298-403">body</span></span>|<span data-ttu-id="82298-404">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-404">No</span></span>|<span data-ttu-id="82298-405">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-405">Object</span></span>|<span data-ttu-id="82298-406">Representerar hello-nyttolast som har skickats toohello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="82298-406">Represents hello payload that is sent toohello endpoint.</span></span>|  
|<span data-ttu-id="82298-407">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="82298-407">retryPolicy</span></span>|<span data-ttu-id="82298-408">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-408">No</span></span>|<span data-ttu-id="82298-409">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-409">Object</span></span>|<span data-ttu-id="82298-410">Kan du anpassa hello försök beteende för 4xx eller 5xx-fel.</span><span class="sxs-lookup"><span data-stu-id="82298-410">Lets you customize hello retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="82298-411">operationsOptions</span><span class="sxs-lookup"><span data-stu-id="82298-411">operationsOptions</span></span>|<span data-ttu-id="82298-412">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-412">No</span></span>|<span data-ttu-id="82298-413">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-413">String</span></span>|<span data-ttu-id="82298-414">Definierar hello särskilda beteenden toooverride.</span><span class="sxs-lookup"><span data-stu-id="82298-414">Defines hello set of special behaviors toooverride.</span></span>|  
|<span data-ttu-id="82298-415">Autentisering</span><span class="sxs-lookup"><span data-stu-id="82298-415">authentication</span></span>|<span data-ttu-id="82298-416">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-416">No</span></span>|<span data-ttu-id="82298-417">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-417">Object</span></span>|<span data-ttu-id="82298-418">Representerar hello metod som hello begäran ska autentiseras.</span><span class="sxs-lookup"><span data-stu-id="82298-418">Represents hello method that hello request should be authenticated.</span></span> <span data-ttu-id="82298-419">Mer information om det här objektet finns [Scheduler utgående autentisering](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span><span class="sxs-lookup"><span data-stu-id="82298-419">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span></span> <span data-ttu-id="82298-420">Utöver scheduler, är en mer stöds egenskap: `authority`.</span><span class="sxs-lookup"><span data-stu-id="82298-420">Beyond scheduler, there is one more supported property: `authority`.</span></span> <span data-ttu-id="82298-421">Detta är som standard `https://login.windows.net` om anges, men du kan använda en annan publik som`https://login.windows\-ppe.net`</span><span class="sxs-lookup"><span data-stu-id="82298-421">By default, this is `https://login.windows.net` when not specified, but you can use a different audience like `https://login.windows\-ppe.net`</span></span>|  
  
<span data-ttu-id="82298-422">HTTP-åtgärder \(och API-anslutningen\) åtgärder stöd försök principer.</span><span class="sxs-lookup"><span data-stu-id="82298-422">HTTP actions \(and API Connection\) actions support retry policies.</span></span> <span data-ttu-id="82298-423">En återförsöksprincip gäller toointermittent fel, kännetecknas som HTTP-status 408, 429 och 5xx i tillägg tooany undantag.</span><span class="sxs-lookup"><span data-stu-id="82298-423">A retry policy applies toointermittent failures, characterized as HTTP status codes 408, 429, and 5xx, in addition tooany connectivity exceptions.</span></span> <span data-ttu-id="82298-424">Den här principen nedan används hello *retryPolicy* objekt som är definierat som visas här:</span><span class="sxs-lookup"><span data-stu-id="82298-424">This policy is described using hello *retryPolicy* object defined as shown here:</span></span>
  
```json
"retryPolicy" : {
    "type": "<type-of-retry-policy>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```
  
<span data-ttu-id="82298-425">hello återförsöksintervall har angetts i hello ISO 8601-format.</span><span class="sxs-lookup"><span data-stu-id="82298-425">hello retry interval is specified in hello ISO 8601 format.</span></span> <span data-ttu-id="82298-426">Det här intervallet har en standard och minsta 20 sekunder medan hello maxvärdet är en timme.</span><span class="sxs-lookup"><span data-stu-id="82298-426">This interval has a default and minimum value of 20 seconds, while hello maximum value is one hour.</span></span> <span data-ttu-id="82298-427">hello standard och maximalt antal för nya försök är fyra timmar.</span><span class="sxs-lookup"><span data-stu-id="82298-427">hello default and maximum retry count is four hours.</span></span> <span data-ttu-id="82298-428">Om hello försök principdefinitionen anges en `fixed` strategi som används med försök antal och intervall standardvärden.</span><span class="sxs-lookup"><span data-stu-id="82298-428">If hello retry policy definition is not specified, a `fixed` strategy is used with default retry count and interval values.</span></span> <span data-ttu-id="82298-429">toodisable hello återförsöksprincip, ange dess för`None`.</span><span class="sxs-lookup"><span data-stu-id="82298-429">toodisable hello retry policy, set its type too`None`.</span></span>  
  
<span data-ttu-id="82298-430">Till exempel återförsök hello följande åtgärd hämtning hello nyheter två gånger om återkommande fel totalt tre körningar med en fördröjning på 30 sekunder mellan varje försök:</span><span class="sxs-lookup"><span data-stu-id="82298-430">For example, hello following action retries fetching hello latest news two times, if there are intermittent failures, for a total of three executions, with a 30-second delay between each attempt:</span></span>  
  
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
### <a name="asynchronous-patterns"></a><span data-ttu-id="82298-431">Asynkront mönster</span><span class="sxs-lookup"><span data-stu-id="82298-431">Asynchronous patterns</span></span>

<span data-ttu-id="82298-432">Som standard stöder alla HTTP-baserade åtgärder hello standard asynkron åtgärd mönster.</span><span class="sxs-lookup"><span data-stu-id="82298-432">By default, all HTTP-based actions support hello standard asynchronous operation pattern.</span></span> <span data-ttu-id="82298-433">Så om hello fjärrservern visar hello begäran är godkänd för behandling med en 202 \(godkända\) svar, hello Logic Apps motorn håller avsökning hello-URL som anges i hello svar platshuvud tills en terminal tillstånd \(inte\-202 svar\).</span><span class="sxs-lookup"><span data-stu-id="82298-433">So if hello remote server indicates that hello request is accepted for processing with a 202 \(Accepted\) response, hello Logic Apps engine keeps polling hello URL specified in hello response's location header until reaching a terminal state \(a non\-202 response\).</span></span>  
  
<span data-ttu-id="82298-434">toodisable hello asynkron beteende tidigare beskrivs, ange en `DisableAsyncPattern` alternativ i hello åtgärd indata.</span><span class="sxs-lookup"><span data-stu-id="82298-434">toodisable hello asynchronous behavior previously described, set a `DisableAsyncPattern` option in hello action inputs.</span></span> <span data-ttu-id="82298-435">I det här fallet är hello utdata från hello åtgärden baserad på hello inledande 202 svaret från hello-servern.</span><span class="sxs-lookup"><span data-stu-id="82298-435">In this case, hello output of hello action is based on hello initial 202 response from hello server.</span></span>  
  
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

#### <a name="asynchronous-limits"></a><span data-ttu-id="82298-436">Asynkron gränser</span><span class="sxs-lookup"><span data-stu-id="82298-436">Asynchronous Limits</span></span>

<span data-ttu-id="82298-437">Ett asynkront mönster kan begränsas i dess duration tooa angivet tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="82298-437">An asynchronous pattern can be limited in its duration tooa specific time interval.</span></span>  <span data-ttu-id="82298-438">Om hello tidsintervall långa utan att ett avslutat tillstånd hello status för hello åtgärd markeras `Cancelled` med koden `ActionTimedOut`.</span><span class="sxs-lookup"><span data-stu-id="82298-438">If hello time interval elapses without reaching a terminal state, hello status of hello action will be marked `Cancelled` with a code of `ActionTimedOut`.</span></span>  <span data-ttu-id="82298-439">hello gränsen timeout anges i ISO 8601-format.</span><span class="sxs-lookup"><span data-stu-id="82298-439">hello limit timeout is specified in ISO 8601 format.</span></span>  <span data-ttu-id="82298-440">Gränser kan anges med hello följande syntax:</span><span class="sxs-lookup"><span data-stu-id="82298-440">Limits can be specified with hello following syntax:</span></span>

``` json
"<action-name>": {
    "type": "workflow|webhook|http|apiconnectionwebhook|apiconnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="api-connection"></a><span data-ttu-id="82298-441">API-anslutningen</span><span class="sxs-lookup"><span data-stu-id="82298-441">API Connection</span></span>  

<span data-ttu-id="82298-442">API-anslutningen är en åtgärd som refererar till en Microsoft-hanterad koppling.</span><span class="sxs-lookup"><span data-stu-id="82298-442">API Connection is an action that references a Microsoft-managed connector.</span></span>
<span data-ttu-id="82298-443">Den här åtgärden kräver en giltig referens tooa-anslutning och information om hello API och parametrar som krävs.</span><span class="sxs-lookup"><span data-stu-id="82298-443">This action requires a reference tooa valid connection, and information on hello API and parameters required.</span></span>

|<span data-ttu-id="82298-444">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="82298-444">Element name</span></span>|<span data-ttu-id="82298-445">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-445">Required</span></span>|<span data-ttu-id="82298-446">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-446">Type</span></span>|<span data-ttu-id="82298-447">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-447">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="82298-448">värden</span><span class="sxs-lookup"><span data-stu-id="82298-448">host</span></span>|<span data-ttu-id="82298-449">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-449">Yes</span></span>|<span data-ttu-id="82298-450">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-450">Object</span></span>|<span data-ttu-id="82298-451">Representerar hello connector information, till exempel hello referens och runtimeUrl toohello anslutningsobjekt</span><span class="sxs-lookup"><span data-stu-id="82298-451">Represents hello connector information such as hello runtimeUrl and reference toohello connection object</span></span>|
|<span data-ttu-id="82298-452">Metoden</span><span class="sxs-lookup"><span data-stu-id="82298-452">method</span></span>|<span data-ttu-id="82298-453">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-453">Yes</span></span>|<span data-ttu-id="82298-454">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-454">String</span></span>|<span data-ttu-id="82298-455">Kan vara något av följande HTTP-metoderna hello: **hämta**, **POST**, **PLACERA**, **ta bort**, **korrigering**, eller  **HUVUDET**</span><span class="sxs-lookup"><span data-stu-id="82298-455">Can be one of hello following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="82298-456">Sökväg</span><span class="sxs-lookup"><span data-stu-id="82298-456">path</span></span>|<span data-ttu-id="82298-457">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-457">Yes</span></span>|<span data-ttu-id="82298-458">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-458">String</span></span>|<span data-ttu-id="82298-459">hello sökväg hello API-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="82298-459">hello path of hello API operation.</span></span>|  
|<span data-ttu-id="82298-460">frågor</span><span class="sxs-lookup"><span data-stu-id="82298-460">queries</span></span>|<span data-ttu-id="82298-461">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-461">No</span></span>|<span data-ttu-id="82298-462">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-462">Object</span></span>|<span data-ttu-id="82298-463">Representerar hello frågan parametrar tooadd toohello URL.</span><span class="sxs-lookup"><span data-stu-id="82298-463">Represents hello query parameters tooadd toohello URL.</span></span> <span data-ttu-id="82298-464">Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` toohello URL.</span><span class="sxs-lookup"><span data-stu-id="82298-464">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|  
|<span data-ttu-id="82298-465">Rubriker</span><span class="sxs-lookup"><span data-stu-id="82298-465">headers</span></span>|<span data-ttu-id="82298-466">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-466">No</span></span>|<span data-ttu-id="82298-467">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-467">Object</span></span>|<span data-ttu-id="82298-468">Representerar varje hello-huvuden som skickas toohello begäran.</span><span class="sxs-lookup"><span data-stu-id="82298-468">Represents each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="82298-469">Till exempel tooset hello språk och typen på en begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="82298-469">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="82298-470">Brödtext</span><span class="sxs-lookup"><span data-stu-id="82298-470">body</span></span>|<span data-ttu-id="82298-471">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-471">No</span></span>|<span data-ttu-id="82298-472">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-472">Object</span></span>|<span data-ttu-id="82298-473">Representerar hello-nyttolast som har skickats toohello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="82298-473">Represents hello payload that is sent toohello endpoint.</span></span>|  
|<span data-ttu-id="82298-474">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="82298-474">retryPolicy</span></span>|<span data-ttu-id="82298-475">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-475">No</span></span>|<span data-ttu-id="82298-476">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-476">Object</span></span>|<span data-ttu-id="82298-477">Kan du anpassa hello försök beteende för 4xx eller 5xx-fel.</span><span class="sxs-lookup"><span data-stu-id="82298-477">Lets you customize hello retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="82298-478">operationsOptions</span><span class="sxs-lookup"><span data-stu-id="82298-478">operationsOptions</span></span>|<span data-ttu-id="82298-479">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-479">No</span></span>|<span data-ttu-id="82298-480">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-480">String</span></span>|<span data-ttu-id="82298-481">Definierar hello särskilda beteenden toooverride.</span><span class="sxs-lookup"><span data-stu-id="82298-481">Defines hello set of special behaviors toooverride.</span></span>|  

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

## <a name="api-connection-webhook-action"></a><span data-ttu-id="82298-482">API-anslutningen webhook åtgärd</span><span class="sxs-lookup"><span data-stu-id="82298-482">API Connection webhook action</span></span>

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

<span data-ttu-id="82298-483">Gränserna för ett webhook-åtgärden kan anges i hello samma sätt som [HTTP asynkron gränser](#asynchronous-limits).</span><span class="sxs-lookup"><span data-stu-id="82298-483">Limits on a webhook action can be specified in hello same manner as [HTTP Asynchronous Limits](#asynchronous-limits).</span></span>
  
## <a name="response-action"></a><span data-ttu-id="82298-484">Svaret åtgärd</span><span class="sxs-lookup"><span data-stu-id="82298-484">Response action</span></span>  

<span data-ttu-id="82298-485">Den här typen innehåller hello hela svaret nyttolast från en HTTP-begäran och ett statusCode, brödtext och rubriker:</span><span class="sxs-lookup"><span data-stu-id="82298-485">This action type contains hello entire response payload from an HTTP request and includes a statusCode, body, and headers:</span></span>  
  
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
  
<span data-ttu-id="82298-486">hello svar åtgärd har särskilda begränsningar som inte gäller tooother åtgärder.</span><span class="sxs-lookup"><span data-stu-id="82298-486">hello response action has special restrictions that don't apply tooother actions.</span></span> <span data-ttu-id="82298-487">Närmare bestämt:</span><span class="sxs-lookup"><span data-stu-id="82298-487">Specifically:</span></span>  
  
-   <span data-ttu-id="82298-488">Responsåtgärder får inte vara parallellt i en definition av eftersom en inkommande begäran om toohello deterministiska svar krävs.</span><span class="sxs-lookup"><span data-stu-id="82298-488">Response actions cannot be parallel in a definition because a deterministic response toohello incoming request is required.</span></span>  
  
-   <span data-ttu-id="82298-489">Om en åtgärd för svar uppnås när hello inkommande begäran tog emot ett svar, hello anses misslyckades \(konflikt\), och därför hello kör `Failed`.</span><span class="sxs-lookup"><span data-stu-id="82298-489">If a response action is reached after hello incoming request has received a response, hello action is considered failed \(conflict\), and as a result, hello run is `Failed`.</span></span>  
  
-   <span data-ttu-id="82298-490">Ett arbetsflöde med responsåtgärder kan inte ha `splitOn` i dess utlösare eftersom ett anrop gör många körs.</span><span class="sxs-lookup"><span data-stu-id="82298-490">A workflow with Response actions cannot have `splitOn` in its trigger because one call causes many runs.</span></span> <span data-ttu-id="82298-491">Det bör därför verifieras när hello flödet är PUT- och orsaken en felaktig begäran.</span><span class="sxs-lookup"><span data-stu-id="82298-491">As a result, this should be validated when hello flow is PUT and cause a Bad Request.</span></span>  
  
## <a name="wait-action"></a><span data-ttu-id="82298-492">Vänta åtgärd</span><span class="sxs-lookup"><span data-stu-id="82298-492">Wait action</span></span>  

<span data-ttu-id="82298-493">Hej `wait` åtgärd pausar körningen av arbetsflödet för hello angivna intervallet.</span><span class="sxs-lookup"><span data-stu-id="82298-493">hello `wait` action suspends workflow execution for hello specified interval.</span></span> <span data-ttu-id="82298-494">Du kan använda den här fragment exempelvis toowait 15 minuter:</span><span class="sxs-lookup"><span data-stu-id="82298-494">For example, toowait 15 minutes, you can use this snippet:</span></span>  
  
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
  
<span data-ttu-id="82298-495">Du kan också toowait tills en särskild tidpunkt, du kan använda det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="82298-495">Alternatively, toowait until a specific moment in time, you can use this example:</span></span>  
  
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
> <span data-ttu-id="82298-496">hello vänta varaktighet kan anges antingen med hjälp av hello **intervall** objekt eller hello **tills** objekt, men inte båda.</span><span class="sxs-lookup"><span data-stu-id="82298-496">hello wait duration can be either specified using hello **interval** object or hello **until** object, but not both.</span></span>  
  
|<span data-ttu-id="82298-497">Namn</span><span class="sxs-lookup"><span data-stu-id="82298-497">Name</span></span>|<span data-ttu-id="82298-498">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-498">Required</span></span>|<span data-ttu-id="82298-499">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-499">Type</span></span>|<span data-ttu-id="82298-500">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-500">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="82298-501">interval</span><span class="sxs-lookup"><span data-stu-id="82298-501">interval</span></span>|<span data-ttu-id="82298-502">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-502">No</span></span>|<span data-ttu-id="82298-503">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-503">Object</span></span>|<span data-ttu-id="82298-504">Hej väntans varaktighet baserat på tid.</span><span class="sxs-lookup"><span data-stu-id="82298-504">hello wait duration based on amount of time.</span></span>|  
|<span data-ttu-id="82298-505">intervall</span><span class="sxs-lookup"><span data-stu-id="82298-505">interval unit</span></span>|<span data-ttu-id="82298-506">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-506">Yes</span></span>|<span data-ttu-id="82298-507">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-507">String</span></span>|<span data-ttu-id="82298-508">En av dessa intervall: sekund, minut, timma, dag, vecka, månad, år.</span><span class="sxs-lookup"><span data-stu-id="82298-508">One of these intervals: second, minute, hour, day, week, month, year.</span></span>|  
|<span data-ttu-id="82298-509">intervall för antal</span><span class="sxs-lookup"><span data-stu-id="82298-509">interval count</span></span>|<span data-ttu-id="82298-510">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-510">Yes</span></span>|<span data-ttu-id="82298-511">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-511">String</span></span>|<span data-ttu-id="82298-512">Varaktighet baserat på hello angiven interna enhet.</span><span class="sxs-lookup"><span data-stu-id="82298-512">Duration based on hello given internal unit.</span></span>|  
|<span data-ttu-id="82298-513">fram till</span><span class="sxs-lookup"><span data-stu-id="82298-513">until</span></span>|<span data-ttu-id="82298-514">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-514">No</span></span>|<span data-ttu-id="82298-515">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-515">Object</span></span>|<span data-ttu-id="82298-516">Hej väntans varaktighet baserat på en punkt i tiden.</span><span class="sxs-lookup"><span data-stu-id="82298-516">hello wait duration based on a point in time.</span></span>|  
|<span data-ttu-id="82298-517">tills tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="82298-517">until timestamp</span></span>|<span data-ttu-id="82298-518">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-518">Yes</span></span>|<span data-ttu-id="82298-519">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-519">String</span></span>|<span data-ttu-id="82298-520">Sträng &#124; hello tidpunkt i UTC när hello vänta upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="82298-520">String&#124;hello point in time in UTC when hello wait expires.</span></span>|  

## <a name="query-action"></a><span data-ttu-id="82298-521">Frågeåtgärden</span><span class="sxs-lookup"><span data-stu-id="82298-521">Query action</span></span>

<span data-ttu-id="82298-522">Hej `query` åtgärd kan du filtrera en matris baserat på ett villkor.</span><span class="sxs-lookup"><span data-stu-id="82298-522">hello `query` action lets you filter an array based on a condition.</span></span> <span data-ttu-id="82298-523">Till exempel tooselect tal som är större än 2, du kan använda:</span><span class="sxs-lookup"><span data-stu-id="82298-523">For example, tooselect numbers greater than 2, you can use:</span></span>

```json
"FilterNumbers" : {
    "type": "query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

<span data-ttu-id="82298-524">Hej utdata från hello `query` åtgärden är en matris som har element från hello Indatamatrisen som uppfyller villkoret hello.</span><span class="sxs-lookup"><span data-stu-id="82298-524">hello output from hello `query` action is an array that has elements from hello input array that satisfy hello condition.</span></span>

> [!NOTE]
> <span data-ttu-id="82298-525">Om inga värden uppfyller hello `where` villkor, hello resultatet är en tom matris.</span><span class="sxs-lookup"><span data-stu-id="82298-525">If no values satisfy hello `where` condition, hello result is an empty array.</span></span>

|<span data-ttu-id="82298-526">Namn</span><span class="sxs-lookup"><span data-stu-id="82298-526">Name</span></span>|<span data-ttu-id="82298-527">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-527">Required</span></span>|<span data-ttu-id="82298-528">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-528">Type</span></span>|<span data-ttu-id="82298-529">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-529">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="82298-530">Från</span><span class="sxs-lookup"><span data-stu-id="82298-530">from</span></span>|<span data-ttu-id="82298-531">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-531">Yes</span></span>|<span data-ttu-id="82298-532">Matris</span><span class="sxs-lookup"><span data-stu-id="82298-532">Array</span></span>|<span data-ttu-id="82298-533">hello källmatrisen.</span><span class="sxs-lookup"><span data-stu-id="82298-533">hello source array.</span></span>|
|<span data-ttu-id="82298-534">där</span><span class="sxs-lookup"><span data-stu-id="82298-534">where</span></span>|<span data-ttu-id="82298-535">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-535">Yes</span></span>|<span data-ttu-id="82298-536">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-536">String</span></span>|<span data-ttu-id="82298-537">hello villkoret tooapply tooeach element i källmatrisen hello.</span><span class="sxs-lookup"><span data-stu-id="82298-537">hello condition tooapply tooeach element of hello source array.</span></span>|

## <a name="select-action"></a><span data-ttu-id="82298-538">Välj åtgärd</span><span class="sxs-lookup"><span data-stu-id="82298-538">Select action</span></span>

<span data-ttu-id="82298-539">Hej `select` du kan projicera varje element i en matris till ett nytt värde.</span><span class="sxs-lookup"><span data-stu-id="82298-539">hello `select` action lets you project each element of an array into a new value.</span></span>
<span data-ttu-id="82298-540">Till exempel tooconvert en matris av talen i en matris av objekt, kan du använda:</span><span class="sxs-lookup"><span data-stu-id="82298-540">For example, tooconvert an array of numbers into an array of objects, you can use:</span></span>

```json
"SelectNumbers" : {
    "type": "select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

<span data-ttu-id="82298-541">Hej utdata från hello `select` åtgärden är en matris som har hello samma kardinalitet som hello Indatamatrisen, där varje element omvandlas som definierats av hello `select` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="82298-541">hello output of hello `select` action is an array that has hello same cardinality as hello input array, with each element transformed as defined by hello `select` property.</span></span> <span data-ttu-id="82298-542">Om hello-indata är en tom matris, hello utdata är också en tom matris.</span><span class="sxs-lookup"><span data-stu-id="82298-542">If hello input is an empty array, hello output is also an empty array.</span></span>

|<span data-ttu-id="82298-543">Namn</span><span class="sxs-lookup"><span data-stu-id="82298-543">Name</span></span>|<span data-ttu-id="82298-544">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-544">Required</span></span>|<span data-ttu-id="82298-545">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-545">Type</span></span>|<span data-ttu-id="82298-546">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-546">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="82298-547">Från</span><span class="sxs-lookup"><span data-stu-id="82298-547">from</span></span>|<span data-ttu-id="82298-548">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-548">Yes</span></span>|<span data-ttu-id="82298-549">Matris</span><span class="sxs-lookup"><span data-stu-id="82298-549">Array</span></span>|<span data-ttu-id="82298-550">hello källmatrisen.</span><span class="sxs-lookup"><span data-stu-id="82298-550">hello source array.</span></span>|
|<span data-ttu-id="82298-551">Välj</span><span class="sxs-lookup"><span data-stu-id="82298-551">select</span></span>|<span data-ttu-id="82298-552">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-552">Yes</span></span>|<span data-ttu-id="82298-553">Alla</span><span class="sxs-lookup"><span data-stu-id="82298-553">Any</span></span>|<span data-ttu-id="82298-554">hello projektion tooapply tooeach element i källmatrisen hello.</span><span class="sxs-lookup"><span data-stu-id="82298-554">hello projection tooapply tooeach element of hello source array.</span></span>|

## <a name="terminate-action"></a><span data-ttu-id="82298-555">Åtgärden Avbryt</span><span class="sxs-lookup"><span data-stu-id="82298-555">Terminate action</span></span>

<span data-ttu-id="82298-556">hello avbrottsåtgärd avbryts körningen av hello arbetsflödet körs, avbryts alla pågående åtgärder och hoppa över eventuella återstående åtgärder.</span><span class="sxs-lookup"><span data-stu-id="82298-556">hello Terminate action stops execution of hello workflow run, aborting any in-flight actions, and skipping any remaining actions.</span></span> <span data-ttu-id="82298-557">Till exempel tooterminate körs med statusen **misslyckades**, kan du använda följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="82298-557">For example, tooterminate a run with status **Failed**, you can use hello following snippet:</span></span>

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
> <span data-ttu-id="82298-558">Åtgärder som redan har slutförts påverkas inte av hello Avbryt.</span><span class="sxs-lookup"><span data-stu-id="82298-558">Actions already completed are not affected by hello terminate action.</span></span>

|<span data-ttu-id="82298-559">Namn</span><span class="sxs-lookup"><span data-stu-id="82298-559">Name</span></span>|<span data-ttu-id="82298-560">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-560">Required</span></span>|<span data-ttu-id="82298-561">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-561">Type</span></span>|<span data-ttu-id="82298-562">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-562">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="82298-563">runStatus</span><span class="sxs-lookup"><span data-stu-id="82298-563">runStatus</span></span>|<span data-ttu-id="82298-564">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-564">Yes</span></span>|<span data-ttu-id="82298-565">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-565">String</span></span>|<span data-ttu-id="82298-566">hello mål Körningsstatus.</span><span class="sxs-lookup"><span data-stu-id="82298-566">hello target run status.</span></span> <span data-ttu-id="82298-567">Antingen **misslyckades** eller **avbröts**.</span><span class="sxs-lookup"><span data-stu-id="82298-567">Either **Failed** or **Cancelled**.</span></span>|
|<span data-ttu-id="82298-568">runError</span><span class="sxs-lookup"><span data-stu-id="82298-568">runError</span></span>|<span data-ttu-id="82298-569">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-569">No</span></span>|<span data-ttu-id="82298-570">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-570">Object</span></span>|<span data-ttu-id="82298-571">hello felinformation.</span><span class="sxs-lookup"><span data-stu-id="82298-571">hello error details.</span></span> <span data-ttu-id="82298-572">Endast när **runStatus** har angetts för**misslyckades**.</span><span class="sxs-lookup"><span data-stu-id="82298-572">Only supported when **runStatus** is set too**Failed**.</span></span>|
|<span data-ttu-id="82298-573">runError kod</span><span class="sxs-lookup"><span data-stu-id="82298-573">runError code</span></span>|<span data-ttu-id="82298-574">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-574">No</span></span>|<span data-ttu-id="82298-575">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-575">String</span></span>|<span data-ttu-id="82298-576">hello kör felkoden.</span><span class="sxs-lookup"><span data-stu-id="82298-576">hello run error code.</span></span>|
|<span data-ttu-id="82298-577">runError meddelande</span><span class="sxs-lookup"><span data-stu-id="82298-577">runError message</span></span>|<span data-ttu-id="82298-578">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-578">No</span></span>|<span data-ttu-id="82298-579">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-579">String</span></span>|<span data-ttu-id="82298-580">hello kör felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="82298-580">hello run error message.</span></span>|

## <a name="compose-action"></a><span data-ttu-id="82298-581">Skriv åtgärd</span><span class="sxs-lookup"><span data-stu-id="82298-581">Compose action</span></span>

<span data-ttu-id="82298-582">hello Skriv åtgärd kan du skapa ett godtyckligt objekt.</span><span class="sxs-lookup"><span data-stu-id="82298-582">hello Compose action lets you construct an arbitrary object.</span></span> <span data-ttu-id="82298-583">hello utdata från hello utgöra åtgärd är hello resultat av utvärderingen av indata.</span><span class="sxs-lookup"><span data-stu-id="82298-583">hello output of hello compose action is hello result of evaluating its inputs.</span></span> <span data-ttu-id="82298-584">Du kan till exempel använda hello utgöra åtgärd toomerge utdata för flera åtgärder:</span><span class="sxs-lookup"><span data-stu-id="82298-584">For example, you can use hello compose action toomerge outputs of multiple actions:</span></span>

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
> <span data-ttu-id="82298-585">Hej **Compose** åtgärd kan vara används tooconstruct inga utdata, inklusive objekt, matriser och andra typer som stöds av logikappar som XML och binary.</span><span class="sxs-lookup"><span data-stu-id="82298-585">hello **Compose** action can be used tooconstruct any output, including objects, arrays, and any other type natively supported by logic apps like XML and binary.</span></span>

## <a name="table-action"></a><span data-ttu-id="82298-586">Åtgärden för tabellen</span><span class="sxs-lookup"><span data-stu-id="82298-586">Table action</span></span>

<span data-ttu-id="82298-587">Hej `table` kan du tooconvert en matris med objekt till en **CSV** eller **HTML** tabell.</span><span class="sxs-lookup"><span data-stu-id="82298-587">hello `table` allows you tooconvert an array of items into a **CSV** or **HTML** table.</span></span>

<span data-ttu-id="82298-588">Anta att @triggerBodyär)</span><span class="sxs-lookup"><span data-stu-id="82298-588">Suppose @triggerBody() is</span></span>

```json
[{
  "id": 0,
  "name": "apples"
},{
  "id": 1, 
  "name": "oranges"
}]
```

<span data-ttu-id="82298-589">Och låta hello åtgärd definieras som</span><span class="sxs-lookup"><span data-stu-id="82298-589">And let hello action be defined as</span></span>

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html"
    }
}
```

<span data-ttu-id="82298-590">hello ovan skulle skapa</span><span class="sxs-lookup"><span data-stu-id="82298-590">hello above would produce</span></span>

<table><thead><tr><th><span data-ttu-id="82298-591">id</span><span class="sxs-lookup"><span data-stu-id="82298-591">id</span></span></th><th><span data-ttu-id="82298-592">namn</span><span class="sxs-lookup"><span data-stu-id="82298-592">name</span></span></th></tr></thead><tbody><tr><td><span data-ttu-id="82298-593">0</span><span class="sxs-lookup"><span data-stu-id="82298-593">0</span></span></td><td><span data-ttu-id="82298-594">Äpplen</span><span class="sxs-lookup"><span data-stu-id="82298-594">apples</span></span></td></tr><tr><td><span data-ttu-id="82298-595">1</span><span class="sxs-lookup"><span data-stu-id="82298-595">1</span></span></td><td><span data-ttu-id="82298-596">orange</span><span class="sxs-lookup"><span data-stu-id="82298-596">oranges</span></span></td></tr></tbody></table><span data-ttu-id="82298-597">"</span><span class="sxs-lookup"><span data-stu-id="82298-597">"</span></span>

<span data-ttu-id="82298-598">I ordning toocustomize hello tabellen du vill kan du ange hello kolumner explicit.</span><span class="sxs-lookup"><span data-stu-id="82298-598">In order toocustomize hello table, you can specify hello columns explicitly.</span></span> <span data-ttu-id="82298-599">Exempel:</span><span class="sxs-lookup"><span data-stu-id="82298-599">For example:</span></span>

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

<span data-ttu-id="82298-600">hello ovan skulle skapa</span><span class="sxs-lookup"><span data-stu-id="82298-600">hello above would produce</span></span>

<table><thead><tr><th><span data-ttu-id="82298-601">Skapa id</span><span class="sxs-lookup"><span data-stu-id="82298-601">produce id</span></span></th><th><span data-ttu-id="82298-602">description</span><span class="sxs-lookup"><span data-stu-id="82298-602">description</span></span></th></tr></thead><tbody><tr><td><span data-ttu-id="82298-603">0</span><span class="sxs-lookup"><span data-stu-id="82298-603">0</span></span></td><td><span data-ttu-id="82298-604">ny äpplen</span><span class="sxs-lookup"><span data-stu-id="82298-604">fresh apples</span></span></td></tr><tr><td><span data-ttu-id="82298-605">1</span><span class="sxs-lookup"><span data-stu-id="82298-605">1</span></span></td><td><span data-ttu-id="82298-606">ny orange</span><span class="sxs-lookup"><span data-stu-id="82298-606">fresh oranges</span></span></td></tr></tbody></table><span data-ttu-id="82298-607">"</span><span class="sxs-lookup"><span data-stu-id="82298-607">"</span></span>

<span data-ttu-id="82298-608">Om hello `from` egenskapsvärdet är en tom matris, hello utdata är en tom tabell.</span><span class="sxs-lookup"><span data-stu-id="82298-608">If hello `from` property value is an empty array, hello output is an empty table.</span></span>

|<span data-ttu-id="82298-609">Namn</span><span class="sxs-lookup"><span data-stu-id="82298-609">Name</span></span>|<span data-ttu-id="82298-610">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-610">Required</span></span>|<span data-ttu-id="82298-611">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-611">Type</span></span>|<span data-ttu-id="82298-612">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-612">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="82298-613">Från</span><span class="sxs-lookup"><span data-stu-id="82298-613">from</span></span>|<span data-ttu-id="82298-614">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-614">Yes</span></span>|<span data-ttu-id="82298-615">Matris</span><span class="sxs-lookup"><span data-stu-id="82298-615">Array</span></span>|<span data-ttu-id="82298-616">hello källmatrisen.</span><span class="sxs-lookup"><span data-stu-id="82298-616">hello source array.</span></span>|
|<span data-ttu-id="82298-617">Format</span><span class="sxs-lookup"><span data-stu-id="82298-617">format</span></span>|<span data-ttu-id="82298-618">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-618">Yes</span></span>|<span data-ttu-id="82298-619">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-619">String</span></span>|<span data-ttu-id="82298-620">hello-format, antingen **CSV** eller **HTML**.</span><span class="sxs-lookup"><span data-stu-id="82298-620">hello format, either **CSV** or **HTML**.</span></span>|
|<span data-ttu-id="82298-621">Kolumner</span><span class="sxs-lookup"><span data-stu-id="82298-621">columns</span></span>|<span data-ttu-id="82298-622">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-622">No</span></span>|<span data-ttu-id="82298-623">Matris</span><span class="sxs-lookup"><span data-stu-id="82298-623">Array</span></span>|<span data-ttu-id="82298-624">hello kolumner.</span><span class="sxs-lookup"><span data-stu-id="82298-624">hello columns.</span></span> <span data-ttu-id="82298-625">Tillåter toooverride hello standard form hello tabellen.</span><span class="sxs-lookup"><span data-stu-id="82298-625">Allows toooverride hello default shape of hello table.</span></span>|
|<span data-ttu-id="82298-626">kolumnrubrik</span><span class="sxs-lookup"><span data-stu-id="82298-626">column header</span></span>|<span data-ttu-id="82298-627">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-627">No</span></span>|<span data-ttu-id="82298-628">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-628">String</span></span>|<span data-ttu-id="82298-629">hello rubrik för hello-kolumn.</span><span class="sxs-lookup"><span data-stu-id="82298-629">hello header of hello column.</span></span>|
|<span data-ttu-id="82298-630">värde i kolumnen</span><span class="sxs-lookup"><span data-stu-id="82298-630">column value</span></span>|<span data-ttu-id="82298-631">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-631">Yes</span></span>|<span data-ttu-id="82298-632">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-632">String</span></span>|<span data-ttu-id="82298-633">hello värdet för hello kolumnen.</span><span class="sxs-lookup"><span data-stu-id="82298-633">hello value of hello column.</span></span>|

## <a name="workflow-action"></a><span data-ttu-id="82298-634">Arbetsflödesåtgärd</span><span class="sxs-lookup"><span data-stu-id="82298-634">Workflow action</span></span>   

|<span data-ttu-id="82298-635">Namn</span><span class="sxs-lookup"><span data-stu-id="82298-635">Name</span></span>|<span data-ttu-id="82298-636">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-636">Required</span></span>|<span data-ttu-id="82298-637">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-637">Type</span></span>|<span data-ttu-id="82298-638">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-638">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="82298-639">värd-id</span><span class="sxs-lookup"><span data-stu-id="82298-639">host id</span></span>|<span data-ttu-id="82298-640">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-640">Yes</span></span>|<span data-ttu-id="82298-641">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-641">String</span></span>|<span data-ttu-id="82298-642">hello resurs-ID för hello arbetsflöde som du vill toocall.</span><span class="sxs-lookup"><span data-stu-id="82298-642">hello resource ID of hello workflow that you want toocall.</span></span>|  
|<span data-ttu-id="82298-643">värden Utlösarnamn</span><span class="sxs-lookup"><span data-stu-id="82298-643">host triggerName</span></span>|<span data-ttu-id="82298-644">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-644">Yes</span></span>|<span data-ttu-id="82298-645">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-645">String</span></span>|<span data-ttu-id="82298-646">hello namnet hello utlösare som du vill tooinvoke.</span><span class="sxs-lookup"><span data-stu-id="82298-646">hello name of hello trigger that you want tooinvoke.</span></span>|  
|<span data-ttu-id="82298-647">frågor</span><span class="sxs-lookup"><span data-stu-id="82298-647">queries</span></span>|<span data-ttu-id="82298-648">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-648">No</span></span>|<span data-ttu-id="82298-649">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-649">Object</span></span>|<span data-ttu-id="82298-650">Representerar hello frågan parametrar tooadd toohello URL.</span><span class="sxs-lookup"><span data-stu-id="82298-650">Represents hello query parameters tooadd toohello URL.</span></span> <span data-ttu-id="82298-651">Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` toohello URL.</span><span class="sxs-lookup"><span data-stu-id="82298-651">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|  
|<span data-ttu-id="82298-652">Rubriker</span><span class="sxs-lookup"><span data-stu-id="82298-652">headers</span></span>|<span data-ttu-id="82298-653">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-653">No</span></span>|<span data-ttu-id="82298-654">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-654">Object</span></span>|<span data-ttu-id="82298-655">Representerar varje hello-huvuden som skickas toohello begäran.</span><span class="sxs-lookup"><span data-stu-id="82298-655">Represents each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="82298-656">Till exempel tooset hello språk och typen på en begäran:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="82298-656">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="82298-657">Brödtext</span><span class="sxs-lookup"><span data-stu-id="82298-657">body</span></span>|<span data-ttu-id="82298-658">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-658">No</span></span>|<span data-ttu-id="82298-659">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-659">Object</span></span>|<span data-ttu-id="82298-660">Representerar hello nyttolasten skickas toohello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="82298-660">Represents hello payload sent toohello endpoint.</span></span>|  
  
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
  
<span data-ttu-id="82298-661">En åtkomstkontroll görs på hello arbetsflöde \(mer specifikt hello utlösaren\), vilket innebär att du behöver komma åt toohello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="82298-661">An access check is made on hello workflow \(more specifically, hello trigger\), meaning you need access toohello workflow.</span></span>  
  
<span data-ttu-id="82298-662">hello matar ut från hello `workflow` åtgärd baserat på vad du har definierat i hello `response` åtgärden i hello underordnat arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="82298-662">hello outputs from hello `workflow` action are based on what you defined in hello `response` action in hello child workflow.</span></span> <span data-ttu-id="82298-663">Om du inte har definierat någon `response` åtgärd och sedan hello utdata är tomma.</span><span class="sxs-lookup"><span data-stu-id="82298-663">If you have not defined any `response` action, then hello outputs are empty.</span></span>  

## <a name="function-action"></a><span data-ttu-id="82298-664">Funktionen åtgärd</span><span class="sxs-lookup"><span data-stu-id="82298-664">Function action</span></span>   

|<span data-ttu-id="82298-665">Namn</span><span class="sxs-lookup"><span data-stu-id="82298-665">Name</span></span>|<span data-ttu-id="82298-666">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-666">Required</span></span>|<span data-ttu-id="82298-667">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-667">Type</span></span>|<span data-ttu-id="82298-668">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-668">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="82298-669">funktionen id</span><span class="sxs-lookup"><span data-stu-id="82298-669">function id</span></span>|<span data-ttu-id="82298-670">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-670">Yes</span></span>|<span data-ttu-id="82298-671">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-671">String</span></span>|<span data-ttu-id="82298-672">hello resurs-ID för hello-funktionen som du vill tooinvoke.</span><span class="sxs-lookup"><span data-stu-id="82298-672">hello resource ID of hello function that you want tooinvoke.</span></span>|  
|<span data-ttu-id="82298-673">Metoden</span><span class="sxs-lookup"><span data-stu-id="82298-673">method</span></span>|<span data-ttu-id="82298-674">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-674">No</span></span>|<span data-ttu-id="82298-675">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-675">String</span></span>|<span data-ttu-id="82298-676">hello HTTP-metoden används tooinvoke hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="82298-676">hello HTTP method used tooinvoke hello function.</span></span> <span data-ttu-id="82298-677">Som standard är det `POST` om inget värde anges.</span><span class="sxs-lookup"><span data-stu-id="82298-677">By default, it is `POST` when not specified.</span></span>|  
|<span data-ttu-id="82298-678">frågor</span><span class="sxs-lookup"><span data-stu-id="82298-678">queries</span></span>|<span data-ttu-id="82298-679">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-679">No</span></span>|<span data-ttu-id="82298-680">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-680">Object</span></span>|<span data-ttu-id="82298-681">Representerar hello frågan parametrar tooadd toohello URL.</span><span class="sxs-lookup"><span data-stu-id="82298-681">Represents hello query parameters tooadd toohello URL.</span></span> <span data-ttu-id="82298-682">Till exempel `"queries" : { "api-version": "2015-02-01" }` lägger till `?api-version=2015-02-01` toohello URL.</span><span class="sxs-lookup"><span data-stu-id="82298-682">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|  
|<span data-ttu-id="82298-683">Rubriker</span><span class="sxs-lookup"><span data-stu-id="82298-683">headers</span></span>|<span data-ttu-id="82298-684">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-684">No</span></span>|<span data-ttu-id="82298-685">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-685">Object</span></span>|<span data-ttu-id="82298-686">Representerar varje hello-huvuden som skickas toohello begäran.</span><span class="sxs-lookup"><span data-stu-id="82298-686">Represents each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="82298-687">Till exempel tooset hello språk och typen på en begäran: `"headers" : { "Accept-Language": "en-us" }`.</span><span class="sxs-lookup"><span data-stu-id="82298-687">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us" }`.</span></span>|  
|<span data-ttu-id="82298-688">Brödtext</span><span class="sxs-lookup"><span data-stu-id="82298-688">body</span></span>|<span data-ttu-id="82298-689">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-689">No</span></span>|<span data-ttu-id="82298-690">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-690">Object</span></span>|<span data-ttu-id="82298-691">Representerar hello nyttolasten skickas toohello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="82298-691">Represents hello payload sent toohello endpoint.</span></span>|  

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

<span data-ttu-id="82298-692">När du sparar hello logikapp kan göra vi några kontroller för hello refererar till funktionen:</span><span class="sxs-lookup"><span data-stu-id="82298-692">When you save hello logic app, we perform some checks on hello referenced function:</span></span>
-   <span data-ttu-id="82298-693">Du måste toohave åtkomst toohello funktion.</span><span class="sxs-lookup"><span data-stu-id="82298-693">You need toohave access toohello function.</span></span>
-   <span data-ttu-id="82298-694">Endast är standard HTTP-utlösare eller allmänna JSON webhook utlösare tillåten.</span><span class="sxs-lookup"><span data-stu-id="82298-694">Only standard HTTP trigger or generic JSON webhook trigger is allowed.</span></span>
-   <span data-ttu-id="82298-695">Det får inte innehålla någon väg som har definierats.</span><span class="sxs-lookup"><span data-stu-id="82298-695">It should not have any route defined.</span></span>
-   <span data-ttu-id="82298-696">Endast ”fungera” och ”anonym” åtkomstnivå är tillåten.</span><span class="sxs-lookup"><span data-stu-id="82298-696">Only "function" and "anonymous" authorization level is allowed.</span></span>

<span data-ttu-id="82298-697">hello utlösaren URL hämtas, cachelagras och används vid körning.</span><span class="sxs-lookup"><span data-stu-id="82298-697">hello trigger URL is retrieved, cached, and used at runtime.</span></span> <span data-ttu-id="82298-698">Om någon åtgärd upphäver hello cachelagras URL, så misslyckas hello åtgärden vid körning.</span><span class="sxs-lookup"><span data-stu-id="82298-698">So if any operation invalidates hello cached URL, hello action fails at runtime.</span></span> <span data-ttu-id="82298-699">toowork runt problemet, spara hello logikappen igen, vilket orsakar logik app tooretrieve och cachelagra hello utlösaren URL igen.</span><span class="sxs-lookup"><span data-stu-id="82298-699">toowork around this, save hello logic app again, which will cause logic app tooretrieve and cache hello trigger URL again.</span></span>

## <a name="collection-actions-scopes-and-loops"></a><span data-ttu-id="82298-700">Samling åtgärder (scope och slingor)</span><span class="sxs-lookup"><span data-stu-id="82298-700">Collection actions (scopes and loops)</span></span>

<span data-ttu-id="82298-701">Vissa åtgärdstyper av kan innehålla åtgärder i själva.</span><span class="sxs-lookup"><span data-stu-id="82298-701">Some action types can contain actions within themselves.</span></span> <span data-ttu-id="82298-702">Referens-åtgärder i en samling kan refereras direkt utanför hello samling.</span><span class="sxs-lookup"><span data-stu-id="82298-702">Reference actions within a collection can be referenced directly outside of hello collection.</span></span> <span data-ttu-id="82298-703">Om du har definierat `http` i en omfattning `@body('http')` fortfarande är giltigt var som helst i ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="82298-703">If you defined `http` in a scope, `@body('http')` is still valid anywhere in a workflow.</span></span> <span data-ttu-id="82298-704">Åtgärder i en samling kan `runAfter` endast andra åtgärder i hello samma samling.</span><span class="sxs-lookup"><span data-stu-id="82298-704">Actions within a collection can `runAfter` only other actions within hello same collection.</span></span>

## <a name="scope-action"></a><span data-ttu-id="82298-705">Scope-åtgärd</span><span class="sxs-lookup"><span data-stu-id="82298-705">Scope action</span></span>

<span data-ttu-id="82298-706">Hej `scope` åtgärd kan du logiskt gruppera åtgärder i ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="82298-706">hello `scope` action lets you logically group actions in a workflow.</span></span>

|<span data-ttu-id="82298-707">Namn</span><span class="sxs-lookup"><span data-stu-id="82298-707">Name</span></span>|<span data-ttu-id="82298-708">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-708">Required</span></span>|<span data-ttu-id="82298-709">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-709">Type</span></span>|<span data-ttu-id="82298-710">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-710">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="82298-711">åtgärder</span><span class="sxs-lookup"><span data-stu-id="82298-711">actions</span></span>|<span data-ttu-id="82298-712">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-712">Yes</span></span>|<span data-ttu-id="82298-713">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-713">Object</span></span>|<span data-ttu-id="82298-714">Inre åtgärder tooexecute inom hello omfång</span><span class="sxs-lookup"><span data-stu-id="82298-714">Inner actions tooexecute within hello scope</span></span>|

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

## <a name="foreach-action"></a><span data-ttu-id="82298-715">ForEach-åtgärd</span><span class="sxs-lookup"><span data-stu-id="82298-715">ForEach action</span></span>

<span data-ttu-id="82298-716">Åtgärden slingor upprepas i en matris och utför inre åtgärder för varje objekt.</span><span class="sxs-lookup"><span data-stu-id="82298-716">This looping action iterates through an array and performs inner actions for each item.</span></span> <span data-ttu-id="82298-717">Som standard kör hello foreach loop parallellt (20 körningar parallellt i taget).</span><span class="sxs-lookup"><span data-stu-id="82298-717">By default, hello foreach loop executes in parallel (20 executions in parallel at a time).</span></span> <span data-ttu-id="82298-718">Du kan ange körning regler med hjälp av hello `operationOptions` parameter.</span><span class="sxs-lookup"><span data-stu-id="82298-718">You can set execution rules using hello `operationOptions` parameter.</span></span>

|<span data-ttu-id="82298-719">Namn</span><span class="sxs-lookup"><span data-stu-id="82298-719">Name</span></span>|<span data-ttu-id="82298-720">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-720">Required</span></span>|<span data-ttu-id="82298-721">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-721">Type</span></span>|<span data-ttu-id="82298-722">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-722">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="82298-723">åtgärder</span><span class="sxs-lookup"><span data-stu-id="82298-723">actions</span></span>|<span data-ttu-id="82298-724">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-724">Yes</span></span>|<span data-ttu-id="82298-725">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-725">Object</span></span>|<span data-ttu-id="82298-726">Inre åtgärder tooexecute inom hello loop</span><span class="sxs-lookup"><span data-stu-id="82298-726">Inner actions tooexecute within hello loop</span></span>|
|<span data-ttu-id="82298-727">foreach</span><span class="sxs-lookup"><span data-stu-id="82298-727">foreach</span></span>|<span data-ttu-id="82298-728">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-728">Yes</span></span>|<span data-ttu-id="82298-729">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-729">string</span></span>|<span data-ttu-id="82298-730">hello matris tooiterate över</span><span class="sxs-lookup"><span data-stu-id="82298-730">hello array tooiterate over</span></span>|
|<span data-ttu-id="82298-731">operationOptions</span><span class="sxs-lookup"><span data-stu-id="82298-731">operationOptions</span></span>|<span data-ttu-id="82298-732">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-732">no</span></span>|<span data-ttu-id="82298-733">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-733">string</span></span>|<span data-ttu-id="82298-734">Åtgärden alternativ för beteende.</span><span class="sxs-lookup"><span data-stu-id="82298-734">Any operation options for behavior.</span></span> <span data-ttu-id="82298-735">För närvarande endast stöd för `sequential` tooexecute iterationer sekventiellt (standard är parallella)</span><span class="sxs-lookup"><span data-stu-id="82298-735">Currently only supports `sequential` tooexecute iterations sequentially (default behavior is parallel)</span></span>|

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

## <a name="until-action"></a><span data-ttu-id="82298-736">Förrän åtgärden</span><span class="sxs-lookup"><span data-stu-id="82298-736">Until action</span></span>

<span data-ttu-id="82298-737">Åtgärden slingor kör inre åtgärder tills ett villkor resulterar tootrue.</span><span class="sxs-lookup"><span data-stu-id="82298-737">This looping action executes inner actions until a condition results tootrue.</span></span>

|<span data-ttu-id="82298-738">Namn</span><span class="sxs-lookup"><span data-stu-id="82298-738">Name</span></span>|<span data-ttu-id="82298-739">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-739">Required</span></span>|<span data-ttu-id="82298-740">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-740">Type</span></span>|<span data-ttu-id="82298-741">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-741">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="82298-742">åtgärder</span><span class="sxs-lookup"><span data-stu-id="82298-742">actions</span></span>|<span data-ttu-id="82298-743">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-743">Yes</span></span>|<span data-ttu-id="82298-744">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-744">Object</span></span>|<span data-ttu-id="82298-745">Inre åtgärder tooexecute inom hello loop</span><span class="sxs-lookup"><span data-stu-id="82298-745">Inner actions tooexecute within hello loop</span></span>|
|<span data-ttu-id="82298-746">uttryck</span><span class="sxs-lookup"><span data-stu-id="82298-746">expression</span></span>|<span data-ttu-id="82298-747">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-747">Yes</span></span>|<span data-ttu-id="82298-748">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-748">string</span></span>|<span data-ttu-id="82298-749">hello uttryck tooevaluate efter varje iteration</span><span class="sxs-lookup"><span data-stu-id="82298-749">hello expression tooevaluate after each iteration</span></span>|
|<span data-ttu-id="82298-750">Gränsen</span><span class="sxs-lookup"><span data-stu-id="82298-750">limit</span></span>|<span data-ttu-id="82298-751">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-751">yes</span></span>|<span data-ttu-id="82298-752">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-752">Object</span></span>|<span data-ttu-id="82298-753">hello gränser för hello loop - minst en gräns måste anges</span><span class="sxs-lookup"><span data-stu-id="82298-753">hello limits for hello loop - at least one limit must be defined</span></span>|
|<span data-ttu-id="82298-754">Antal</span><span class="sxs-lookup"><span data-stu-id="82298-754">count</span></span>|<span data-ttu-id="82298-755">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-755">no</span></span>|<span data-ttu-id="82298-756">int</span><span class="sxs-lookup"><span data-stu-id="82298-756">int</span></span>|<span data-ttu-id="82298-757">hello begränsa toohello antalet upprepningar som kan utföras</span><span class="sxs-lookup"><span data-stu-id="82298-757">hello limit toohello number of iterations that can be performed</span></span>|
|<span data-ttu-id="82298-758">timeout</span><span class="sxs-lookup"><span data-stu-id="82298-758">timeout</span></span>|<span data-ttu-id="82298-759">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-759">no</span></span>|<span data-ttu-id="82298-760">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-760">string</span></span>|<span data-ttu-id="82298-761">hello tidsgräns för hur länge det ska köras i slinga.</span><span class="sxs-lookup"><span data-stu-id="82298-761">hello timeout for how long it should loop.</span></span>  <span data-ttu-id="82298-762">ISO 8601-format</span><span class="sxs-lookup"><span data-stu-id="82298-762">ISO 8601 format</span></span>|


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

## <a name="conditions---if-action"></a><span data-ttu-id="82298-763">Villkor - om åtgärden</span><span class="sxs-lookup"><span data-stu-id="82298-763">Conditions - If Action</span></span>

<span data-ttu-id="82298-764">Hej `If` åtgärd kan du utvärdera ett villkor och köra en gren baserat på om hello uttrycket utvärderas för`true`.</span><span class="sxs-lookup"><span data-stu-id="82298-764">hello `If` action lets you evaluate a condition and execute a branch based on whether hello expression evaluates too`true`.</span></span>

|<span data-ttu-id="82298-765">Namn</span><span class="sxs-lookup"><span data-stu-id="82298-765">Name</span></span>|<span data-ttu-id="82298-766">Krävs</span><span class="sxs-lookup"><span data-stu-id="82298-766">Required</span></span>|<span data-ttu-id="82298-767">Typ</span><span class="sxs-lookup"><span data-stu-id="82298-767">Type</span></span>|<span data-ttu-id="82298-768">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="82298-768">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="82298-769">åtgärder</span><span class="sxs-lookup"><span data-stu-id="82298-769">actions</span></span>|<span data-ttu-id="82298-770">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-770">Yes</span></span>|<span data-ttu-id="82298-771">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-771">Object</span></span>|<span data-ttu-id="82298-772">Inre åtgärder tooexecute när uttrycket utvärderas för`true`</span><span class="sxs-lookup"><span data-stu-id="82298-772">Inner actions tooexecute when expression evaluates too`true`</span></span>|
|<span data-ttu-id="82298-773">uttryck</span><span class="sxs-lookup"><span data-stu-id="82298-773">expression</span></span>|<span data-ttu-id="82298-774">Ja</span><span class="sxs-lookup"><span data-stu-id="82298-774">Yes</span></span>|<span data-ttu-id="82298-775">Sträng</span><span class="sxs-lookup"><span data-stu-id="82298-775">string</span></span>|<span data-ttu-id="82298-776">hello uttryck tooevaluate</span><span class="sxs-lookup"><span data-stu-id="82298-776">hello expression tooevaluate</span></span>|
|<span data-ttu-id="82298-777">annan</span><span class="sxs-lookup"><span data-stu-id="82298-777">else</span></span>|<span data-ttu-id="82298-778">Nej</span><span class="sxs-lookup"><span data-stu-id="82298-778">no</span></span>|<span data-ttu-id="82298-779">Objekt</span><span class="sxs-lookup"><span data-stu-id="82298-779">Object</span></span>|<span data-ttu-id="82298-780">Inre åtgärder tooexecute när uttrycket utvärderas för`false`</span><span class="sxs-lookup"><span data-stu-id="82298-780">Inner actions tooexecute when expression evaluates too`false`</span></span>|
  
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
  
<span data-ttu-id="82298-781">hello visas följande tabell exempel på hur villkor kan använda uttryck i en åtgärd:</span><span class="sxs-lookup"><span data-stu-id="82298-781">hello following table shows examples of how conditions can use expressions in an action:</span></span>  
  
|<span data-ttu-id="82298-782">JSON-värde</span><span class="sxs-lookup"><span data-stu-id="82298-782">JSON value</span></span>|<span data-ttu-id="82298-783">Resultat</span><span class="sxs-lookup"><span data-stu-id="82298-783">Result</span></span>|  
|--------------|----------|  
|`"expression": "@parameters('hasSpecialAction')"`|<span data-ttu-id="82298-784">Ett värde som utvärderar tootrue gör det här villkoret toopass.</span><span class="sxs-lookup"><span data-stu-id="82298-784">Any value that would evaluate tootrue causes this condition toopass.</span></span> <span data-ttu-id="82298-785">Endast booleskt uttryck stöds.</span><span class="sxs-lookup"><span data-stu-id="82298-785">Only Boolean expressions are supported.</span></span> <span data-ttu-id="82298-786">tooconvert andra typer tooBoolean, Använd funktioner `empty`, `equals`.</span><span class="sxs-lookup"><span data-stu-id="82298-786">tooconvert other types tooBoolean, use functions `empty`, `equals`.</span></span>|  
|`"expression": "@greater(actions('act1').output.value, parameters('threshold'))"`|<span data-ttu-id="82298-787">Jämförelse funktioner stöds.</span><span class="sxs-lookup"><span data-stu-id="82298-787">Comparison functions are supported.</span></span> <span data-ttu-id="82298-788">Exempelvis hello här körs bara hello åtgärden när hello utdata från act1 är större än hello tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="82298-788">For hello example here, hello action only executes when hello output of act1 is greater than hello threshold.</span></span>|  
|`"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"`|<span data-ttu-id="82298-789">Logik funktionerna finns även stöds toocreate kapslade booleska uttryck.</span><span class="sxs-lookup"><span data-stu-id="82298-789">Logic functions are also supported toocreate nested Boolean expressions.</span></span> <span data-ttu-id="82298-790">I det här fallet körs hello åtgärden när hello utdata från act1 är tröskelvärdet hello eller under 100.</span><span class="sxs-lookup"><span data-stu-id="82298-790">In this case, hello action executes when hello output of act1 is above hello threshold or below 100.</span></span>|  
|`"expression": "@equals(length(actions('act1').outputs.errors), 0))"`|<span data-ttu-id="82298-791">Du kan använda matrisen funktioner toocheck om en matris har några objekt.</span><span class="sxs-lookup"><span data-stu-id="82298-791">You can use array functions toocheck if an array has any items.</span></span> <span data-ttu-id="82298-792">I det här fallet körs hello åtgärden när hello fel matrisen är tom.</span><span class="sxs-lookup"><span data-stu-id="82298-792">In this case, hello action executes when hello errors array is empty.</span></span>| 
|`"expression": "parameters('hasSpecialAction')"`|<span data-ttu-id="82298-793">Fel - inte ett giltigt tillstånd eftersom @ krävs för villkor.</span><span class="sxs-lookup"><span data-stu-id="82298-793">Error - not a valid condition because @ is required for conditions.</span></span>|  
  
<span data-ttu-id="82298-794">Om ett villkor utvärderas har, hello villkor har markerats som `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="82298-794">If a condition evaluates successfully, hello condition is marked as `Succeeded`.</span></span> <span data-ttu-id="82298-795">Åtgärder i antingen hello `actions` eller `else` objekt utvärdera för`Succeeded` när den exekveras och lyckades, `Failed` när den exekveras och misslyckades, eller `Skipped` när den grenen körs inte.</span><span class="sxs-lookup"><span data-stu-id="82298-795">Actions within either hello `actions` or `else` objects evaluate too`Succeeded` when executed and succeeded, `Failed` when executed and failed, or `Skipped` when that branch is not executed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="82298-796">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="82298-796">Next steps</span></span>

[<span data-ttu-id="82298-797">Arbetsflödet Service REST-API</span><span class="sxs-lookup"><span data-stu-id="82298-797">Workflow Service REST API</span></span>](https://docs.microsoft.com/rest/api/logic/workflows)
