---
title: "Med hjälp av OMS Log Analytics avisering REST API"
description: "Log Analytics avisering REST-API kan du skapa och hantera aviseringar i logganalys som är en del av Operations Management Suite (OMS).  Den här artikeln innehåller information om API: et och flera exempel för att utföra olika åtgärder."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 628ad256-7181-4a0d-9e68-4ed60c0f3f04
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5ce72ffef4394bf3bbe39fa420c4fcaa965ae35c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a><span data-ttu-id="ae73b-104">Skapa och hantera Varningsregler i logganalys med REST API</span><span class="sxs-lookup"><span data-stu-id="ae73b-104">Create and manage alert rules in Log Analytics with REST API</span></span>
<span data-ttu-id="ae73b-105">Log Analytics avisering REST-API kan du skapa och hantera aviseringar i Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="ae73b-105">The Log Analytics Alert REST API allows you to create and manage alerts in Operations Management Suite (OMS).</span></span>  <span data-ttu-id="ae73b-106">Den här artikeln innehåller information om API: et och flera exempel för att utföra olika åtgärder.</span><span class="sxs-lookup"><span data-stu-id="ae73b-106">This article provides details of the API and several examples for performing different operations.</span></span>

<span data-ttu-id="ae73b-107">Log Analytics Sök REST API är RESTful och kan nås via Azure Resource Manager REST API.</span><span class="sxs-lookup"><span data-stu-id="ae73b-107">The Log Analytics Search REST API is RESTful and can be accessed via the Azure Resource Manager REST API.</span></span> <span data-ttu-id="ae73b-108">I det här dokumentet hittar du exempel där API: et kan nås från ett PowerShell-kommandorad med [ARMClient](https://github.com/projectkudu/ARMClient), en öppen källkod kommandoradsverktyg som förenklar anropar API: et för Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ae73b-108">In this document you will find examples where the API is accessed from a PowerShell command line using  [ARMClient](https://github.com/projectkudu/ARMClient), an open source command line tool that simplifies invoking the Azure Resource Manager API.</span></span> <span data-ttu-id="ae73b-109">Användning av ARMClient och PowerShell är en av många alternativ för att komma åt Log Analytics Sök-API.</span><span class="sxs-lookup"><span data-stu-id="ae73b-109">The use of ARMClient and PowerShell is one of many options to access the Log Analytics Search API.</span></span> <span data-ttu-id="ae73b-110">Med dessa verktyg kan du använda RESTful Azure Resource Manager API för att göra anrop till OMS arbetsytor och utföra sökkommandon i dem.</span><span class="sxs-lookup"><span data-stu-id="ae73b-110">With these tools you can utilize the RESTful Azure Resource Manager API to make calls to OMS workspaces and perform search commands within them.</span></span> <span data-ttu-id="ae73b-111">API: et utdata sökresultat för dig i JSON-format, så att du kan använda sökresultatet på många olika sätt programmässigt.</span><span class="sxs-lookup"><span data-stu-id="ae73b-111">The API will output search results to you in JSON format, allowing you to use the search results in many different ways programmatically.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ae73b-112">Krav</span><span class="sxs-lookup"><span data-stu-id="ae73b-112">Prerequisites</span></span>
<span data-ttu-id="ae73b-113">Aviseringar kan för närvarande kan endast skapas med en sparad sökning i logganalys.</span><span class="sxs-lookup"><span data-stu-id="ae73b-113">Currently, alerts can only be created with a saved search in Log Analytics.</span></span>  <span data-ttu-id="ae73b-114">Du kan referera till den [loggen Sök REST API](log-analytics-log-search-api.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="ae73b-114">You can refer to the [Log Search REST API](log-analytics-log-search-api.md) for more information.</span></span>

## <a name="schedules"></a><span data-ttu-id="ae73b-115">Scheman</span><span class="sxs-lookup"><span data-stu-id="ae73b-115">Schedules</span></span>
<span data-ttu-id="ae73b-116">En sparad sökning kan ha ett eller flera scheman.</span><span class="sxs-lookup"><span data-stu-id="ae73b-116">A saved search can have one or more schedules.</span></span> <span data-ttu-id="ae73b-117">Schemat definierar hur ofta sökningen ska köras och det tidsintervall under vilken kriterierna som identifieras.</span><span class="sxs-lookup"><span data-stu-id="ae73b-117">The schedule defines how often the search is run and the time interval over which the criteria is identified.</span></span>
<span data-ttu-id="ae73b-118">Scheman har egenskaperna i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="ae73b-118">Schedules have the properties in the following table.</span></span>

| <span data-ttu-id="ae73b-119">Egenskap</span><span class="sxs-lookup"><span data-stu-id="ae73b-119">Property</span></span> | <span data-ttu-id="ae73b-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ae73b-120">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ae73b-121">intervall</span><span class="sxs-lookup"><span data-stu-id="ae73b-121">Interval</span></span> |<span data-ttu-id="ae73b-122">Hur ofta sökningen körs.</span><span class="sxs-lookup"><span data-stu-id="ae73b-122">How often the search is run.</span></span> <span data-ttu-id="ae73b-123">Mätt i minuter.</span><span class="sxs-lookup"><span data-stu-id="ae73b-123">Measured in minutes.</span></span> |
| <span data-ttu-id="ae73b-124">QueryTimeSpan</span><span class="sxs-lookup"><span data-stu-id="ae73b-124">QueryTimeSpan</span></span> |<span data-ttu-id="ae73b-125">Det tidsintervall som villkoren utvärderas.</span><span class="sxs-lookup"><span data-stu-id="ae73b-125">The time interval over which the criteria is evaluated.</span></span> <span data-ttu-id="ae73b-126">Måste vara lika med eller större än intervall.</span><span class="sxs-lookup"><span data-stu-id="ae73b-126">Must be equal to or greater than Interval.</span></span> <span data-ttu-id="ae73b-127">Mätt i minuter.</span><span class="sxs-lookup"><span data-stu-id="ae73b-127">Measured in minutes.</span></span> |
| <span data-ttu-id="ae73b-128">Version</span><span class="sxs-lookup"><span data-stu-id="ae73b-128">Version</span></span> |<span data-ttu-id="ae73b-129">API-versionen som används.</span><span class="sxs-lookup"><span data-stu-id="ae73b-129">The API version being used.</span></span>  <span data-ttu-id="ae73b-130">Detta bör för närvarande alltid anges till 1.</span><span class="sxs-lookup"><span data-stu-id="ae73b-130">Currently, this should always be set to 1.</span></span> |

<span data-ttu-id="ae73b-131">Anta till exempel att en fråga med ett intervall på 15 minuter och Timespan 30 minuter.</span><span class="sxs-lookup"><span data-stu-id="ae73b-131">For example, consider an event query with an Interval of 15 minutes and a Timespan of 30 minutes.</span></span> <span data-ttu-id="ae73b-132">I det här fallet frågan skulle köras var 15: e minut och en avisering ska utlösas om villkoren fortsatte att matcha till true över ett 30 minuters intervall.</span><span class="sxs-lookup"><span data-stu-id="ae73b-132">In this case, the query would be run every 15 minutes, and an alert would be triggered if the criteria continued to resolve to true over a 30 minute span.</span></span>

### <a name="retrieving-schedules"></a><span data-ttu-id="ae73b-133">Hämtar scheman</span><span class="sxs-lookup"><span data-stu-id="ae73b-133">Retrieving schedules</span></span>
<span data-ttu-id="ae73b-134">Använd Get-metoden för att hämta alla scheman för en sparad sökning.</span><span class="sxs-lookup"><span data-stu-id="ae73b-134">Use the Get method to retrieve all schedules for a saved search.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

<span data-ttu-id="ae73b-135">Använd Get-metoden med ett schema-ID för att hämta ett visst schema för en sparad sökning.</span><span class="sxs-lookup"><span data-stu-id="ae73b-135">Use the Get method with a schedule ID to retrieve a particular schedule for a saved search.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

<span data-ttu-id="ae73b-136">Följande är ett exempelsvar för ett schema.</span><span class="sxs-lookup"><span data-stu-id="ae73b-136">Following is a sample response for a schedule.</span></span>

```json
{
    "value": [{
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
            "Interval": 15,
            "QueryTimeSpan": 15
        }
    }]
}
```

### <a name="creating-a-schedule"></a><span data-ttu-id="ae73b-137">Skapa ett schema</span><span class="sxs-lookup"><span data-stu-id="ae73b-137">Creating a schedule</span></span>
<span data-ttu-id="ae73b-138">Använda Put-metoden med ett unikt schema-ID för att skapa ett nytt schema.</span><span class="sxs-lookup"><span data-stu-id="ae73b-138">Use the Put method with a unique schedule ID to create a new schedule.</span></span>  <span data-ttu-id="ae73b-139">Observera att två scheman inte kan ha samma ID även om de är kopplade till olika sparade sökningar.</span><span class="sxs-lookup"><span data-stu-id="ae73b-139">Note that two schedules cannot have the same ID even if they are associated with different saved searches.</span></span>  <span data-ttu-id="ae73b-140">När du skapar ett schema i OMS-konsolen, skapas ett GUID för schema-ID.</span><span class="sxs-lookup"><span data-stu-id="ae73b-140">When you create a schedule in the OMS console, a GUID is created for the schedule ID.</span></span>

> [!NOTE]
> <span data-ttu-id="ae73b-141">Namnet på alla sparade sökningar, scheman och åtgärder som skapats med API: et för Log Analytics måste vara i gemener.</span><span class="sxs-lookup"><span data-stu-id="ae73b-141">The name for all saved searches, schedules, and actions created with the Log Analytics API must be in lowercase.</span></span>

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a><span data-ttu-id="ae73b-142">Ändringar i schemat</span><span class="sxs-lookup"><span data-stu-id="ae73b-142">Editing a schedule</span></span>
<span data-ttu-id="ae73b-143">Använda Put-metoden med ett befintligt schema-ID för samma sparad sökning om du vill ändra schemat.</span><span class="sxs-lookup"><span data-stu-id="ae73b-143">Use the Put method with an existing schedule ID for the same saved search to modify that schedule.</span></span>  <span data-ttu-id="ae73b-144">Brödtexten i begäran måste innehålla etag för schemat.</span><span class="sxs-lookup"><span data-stu-id="ae73b-144">The body of the request must include the etag of the schedule.</span></span>

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a><span data-ttu-id="ae73b-145">Ta bort scheman</span><span class="sxs-lookup"><span data-stu-id="ae73b-145">Deleting schedules</span></span>
<span data-ttu-id="ae73b-146">Använd Delete-metoden med ett schema-ID för att ta bort ett schema.</span><span class="sxs-lookup"><span data-stu-id="ae73b-146">Use the Delete method with a schedule ID to delete a schedule.</span></span>

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a><span data-ttu-id="ae73b-147">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="ae73b-147">Actions</span></span>
<span data-ttu-id="ae73b-148">Ett schema kan ha flera åtgärder.</span><span class="sxs-lookup"><span data-stu-id="ae73b-148">A schedule can have multiple actions.</span></span> <span data-ttu-id="ae73b-149">En åtgärd kan definiera en eller flera processer för att utföra som skickar ett e-post eller starta en runbook eller den kan definiera ett tröskelvärde som bestämmer när resultatet av en sökning som matchar vissa villkor.</span><span class="sxs-lookup"><span data-stu-id="ae73b-149">An action may define one or more processes to perform such as sending a mail or starting a runbook, or it may define a threshold that determines when the results of a search match some criteria.</span></span>  <span data-ttu-id="ae73b-150">Vissa åtgärder definiera både så att processerna som utförs när tröskelvärdet är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="ae73b-150">Some actions will define both so that the processes are performed when the threshold is met.</span></span>

<span data-ttu-id="ae73b-151">Alla åtgärder ha egenskaperna i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="ae73b-151">All actions have the properties in the following table.</span></span>  <span data-ttu-id="ae73b-152">Olika typer av aviseringar har olika ytterligare egenskaper som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="ae73b-152">Different types of alerts have different additional properties which are described below.</span></span>

| <span data-ttu-id="ae73b-153">Egenskap</span><span class="sxs-lookup"><span data-stu-id="ae73b-153">Property</span></span> | <span data-ttu-id="ae73b-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ae73b-154">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ae73b-155">Typ</span><span class="sxs-lookup"><span data-stu-id="ae73b-155">Type</span></span> |<span data-ttu-id="ae73b-156">Typ av åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ae73b-156">Type of the action.</span></span>  <span data-ttu-id="ae73b-157">Möjliga värden är för närvarande aviseringen och Webhooken.</span><span class="sxs-lookup"><span data-stu-id="ae73b-157">Currently the possible values are Alert and Webhook.</span></span> |
| <span data-ttu-id="ae73b-158">Namn</span><span class="sxs-lookup"><span data-stu-id="ae73b-158">Name</span></span> |<span data-ttu-id="ae73b-159">Visningsnamn för aviseringen.</span><span class="sxs-lookup"><span data-stu-id="ae73b-159">Display name for the alert.</span></span> |
| <span data-ttu-id="ae73b-160">Version</span><span class="sxs-lookup"><span data-stu-id="ae73b-160">Version</span></span> |<span data-ttu-id="ae73b-161">API-versionen som används.</span><span class="sxs-lookup"><span data-stu-id="ae73b-161">The API version being used.</span></span>  <span data-ttu-id="ae73b-162">Detta bör för närvarande alltid anges till 1.</span><span class="sxs-lookup"><span data-stu-id="ae73b-162">Currently, this should always be set to 1.</span></span> |

### <a name="retrieving-actions"></a><span data-ttu-id="ae73b-163">Hämta åtgärder</span><span class="sxs-lookup"><span data-stu-id="ae73b-163">Retrieving actions</span></span>
<span data-ttu-id="ae73b-164">Använd Get-metoden för att hämta alla åtgärder för ett schema.</span><span class="sxs-lookup"><span data-stu-id="ae73b-164">Use the Get method to retrieve all actions for a schedule.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

<span data-ttu-id="ae73b-165">Använd Get-metoden med åtgärds-ID för att hämta en viss åtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="ae73b-165">Use the Get method with the action ID to retrieve a particular action for a schedule.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a><span data-ttu-id="ae73b-166">Skapa eller redigera åtgärder</span><span class="sxs-lookup"><span data-stu-id="ae73b-166">Creating or editing actions</span></span>
<span data-ttu-id="ae73b-167">Använda Put-metoden med en åtgärds-ID som är unik för schema för att skapa en ny åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ae73b-167">Use the Put method with an action ID that is unique to the schedule to create a new action.</span></span>  <span data-ttu-id="ae73b-168">När du skapar en åtgärd i OMS-konsolen är ett GUID för åtgärds-ID.</span><span class="sxs-lookup"><span data-stu-id="ae73b-168">When you create an action in the OMS console, a GUID is for the action ID.</span></span>

> [!NOTE]
> <span data-ttu-id="ae73b-169">Namnet på alla sparade sökningar, scheman och åtgärder som skapats med API: et för Log Analytics måste vara i gemener.</span><span class="sxs-lookup"><span data-stu-id="ae73b-169">The name for all saved searches, schedules, and actions created with the Log Analytics API must be in lowercase.</span></span>

<span data-ttu-id="ae73b-170">Använda Put-metoden med en befintlig åtgärds-ID för samma sparad sökning om du vill ändra schemat.</span><span class="sxs-lookup"><span data-stu-id="ae73b-170">Use the Put method with an existing action ID for the same saved search to modify that schedule.</span></span>  <span data-ttu-id="ae73b-171">Brödtexten i begäran måste innehålla etag för schemat.</span><span class="sxs-lookup"><span data-stu-id="ae73b-171">The body of the request must include the etag of the schedule.</span></span>

<span data-ttu-id="ae73b-172">Det begärandeformatet för att skapa en ny åtgärd varierar beroende på typ så att de här exemplen finns i avsnitten nedan.</span><span class="sxs-lookup"><span data-stu-id="ae73b-172">The request format for creating a new action varies by action type so these examples are provided in the sections below.</span></span>

### <a name="deleting-actions"></a><span data-ttu-id="ae73b-173">Ta bort åtgärder</span><span class="sxs-lookup"><span data-stu-id="ae73b-173">Deleting actions</span></span>
<span data-ttu-id="ae73b-174">Använda Delete-metoden med åtgärds-ID för att ta bort en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ae73b-174">Use the Delete method with the action ID to delete an action.</span></span>

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a><span data-ttu-id="ae73b-175">Aviseringsåtgärder</span><span class="sxs-lookup"><span data-stu-id="ae73b-175">Alert Actions</span></span>
<span data-ttu-id="ae73b-176">Ett schema ska ha en Aviseringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="ae73b-176">A Schedule should have one and only one Alert action.</span></span>  <span data-ttu-id="ae73b-177">Aviseringsåtgärder har en eller flera av avsnitten i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="ae73b-177">Alert actions have one or more of the sections in the following table.</span></span>  <span data-ttu-id="ae73b-178">Alla beskrivs i mer detalj nedan.</span><span class="sxs-lookup"><span data-stu-id="ae73b-178">Each is described in further detail below.</span></span>

| <span data-ttu-id="ae73b-179">Avsnittet</span><span class="sxs-lookup"><span data-stu-id="ae73b-179">Section</span></span> | <span data-ttu-id="ae73b-180">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ae73b-180">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ae73b-181">Tröskelvärde</span><span class="sxs-lookup"><span data-stu-id="ae73b-181">Threshold</span></span> |<span data-ttu-id="ae73b-182">Kriterier för när instruktionen körs.</span><span class="sxs-lookup"><span data-stu-id="ae73b-182">Criteria for when the action is run.</span></span> |
| <span data-ttu-id="ae73b-183">EmailNotification</span><span class="sxs-lookup"><span data-stu-id="ae73b-183">EmailNotification</span></span> |<span data-ttu-id="ae73b-184">Skicka e-post till flera mottagare.</span><span class="sxs-lookup"><span data-stu-id="ae73b-184">Send mail to multiple recipients.</span></span> |
| <span data-ttu-id="ae73b-185">Reparation</span><span class="sxs-lookup"><span data-stu-id="ae73b-185">Remediation</span></span> |<span data-ttu-id="ae73b-186">Starta en runbook i Azure Automation för att försöka åtgärda identifierade problem.</span><span class="sxs-lookup"><span data-stu-id="ae73b-186">Start a runbook in Azure Automation to attempt to correct identified issue.</span></span> |

#### <a name="thresholds"></a><span data-ttu-id="ae73b-187">Tröskelvärden</span><span class="sxs-lookup"><span data-stu-id="ae73b-187">Thresholds</span></span>
<span data-ttu-id="ae73b-188">En åtgärd måste ha ett och endast ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="ae73b-188">An Alert action should have one and only one threshold.</span></span>  <span data-ttu-id="ae73b-189">När resultatet av en sparad sökning matchar tröskelvärdet i en åtgärd som är associerade med sökningen kör några andra processer i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="ae73b-189">When the results of a saved search match the threshold in an action associated with that search, then any other processes in that action are run.</span></span>  <span data-ttu-id="ae73b-190">En åtgärd kan också innehålla endast ett tröskelvärde så att den kan användas med åtgärder för andra typer som inte innehåller tröskelvärden.</span><span class="sxs-lookup"><span data-stu-id="ae73b-190">An action can also contain only a threshold so that it can be used with actions of other types that don’t contain thresholds.</span></span>

<span data-ttu-id="ae73b-191">Tröskelvärden har egenskaperna i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="ae73b-191">Thresholds have the properties in the following table.</span></span>

| <span data-ttu-id="ae73b-192">Egenskap</span><span class="sxs-lookup"><span data-stu-id="ae73b-192">Property</span></span> | <span data-ttu-id="ae73b-193">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ae73b-193">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ae73b-194">Operatorn</span><span class="sxs-lookup"><span data-stu-id="ae73b-194">Operator</span></span> |<span data-ttu-id="ae73b-195">Operator för tröskelvärde jämförelse.</span><span class="sxs-lookup"><span data-stu-id="ae73b-195">Operator for the threshold comparison.</span></span> <br> <span data-ttu-id="ae73b-196">gt = större än</span><span class="sxs-lookup"><span data-stu-id="ae73b-196">gt = Greater Than</span></span> <br> <span data-ttu-id="ae73b-197">lt = mindre än</span><span class="sxs-lookup"><span data-stu-id="ae73b-197">lt = Less Than</span></span> |
| <span data-ttu-id="ae73b-198">Värde</span><span class="sxs-lookup"><span data-stu-id="ae73b-198">Value</span></span> |<span data-ttu-id="ae73b-199">Värdet för tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="ae73b-199">Value for the threshold.</span></span> |

<span data-ttu-id="ae73b-200">Anta till exempel att en fråga med ett intervall på 15 minuter, Timespan 30 minuter och ett tröskelvärde som är större än 10.</span><span class="sxs-lookup"><span data-stu-id="ae73b-200">For example, consider an event query with an Interval of 15 minutes, a Timespan of 30 minutes, and a Threshold of greater than 10.</span></span> <span data-ttu-id="ae73b-201">I det här fallet frågan skulle köras var 15: e minut och skulle att utlösa en avisering om det returnerade 10 händelser som har skapats under ett 30 minuters intervall.</span><span class="sxs-lookup"><span data-stu-id="ae73b-201">In this case, the query would be run every 15 minutes, and an alert would be triggered if it returned 10 events that were created over a 30 minute span.</span></span>

<span data-ttu-id="ae73b-202">Följande är ett exempelsvar för en åtgärd med bara ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="ae73b-202">Following is a sample response for an action with only a threshold.</span></span>  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

<span data-ttu-id="ae73b-203">Använda Put-metoden med en unik åtgärds-ID för att skapa en ny tröskelvärdet åtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="ae73b-203">Use the Put method with a unique action ID to create a new threshold action for a schedule.</span></span>  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

<span data-ttu-id="ae73b-204">Använda Put-metoden med en befintlig åtgärds-ID om du vill ändra en åtgärd för tröskelvärde för ett schema.</span><span class="sxs-lookup"><span data-stu-id="ae73b-204">Use the Put method with an existing action ID to modify a threshold action for a schedule.</span></span>  <span data-ttu-id="ae73b-205">Brödtexten i begäran måste innehålla etag för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="ae73b-205">The body of the request must include the etag of the action.</span></span>

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a><span data-ttu-id="ae73b-206">E-postavisering</span><span class="sxs-lookup"><span data-stu-id="ae73b-206">Email Notification</span></span>
<span data-ttu-id="ae73b-207">E-postaviseringar skicka e-post till en eller flera mottagare.</span><span class="sxs-lookup"><span data-stu-id="ae73b-207">Email Notifications send mail to one or more recipients.</span></span>  <span data-ttu-id="ae73b-208">De omfattar egenskaperna i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="ae73b-208">They include the properties in the following table.</span></span>

| <span data-ttu-id="ae73b-209">Egenskap</span><span class="sxs-lookup"><span data-stu-id="ae73b-209">Property</span></span> | <span data-ttu-id="ae73b-210">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ae73b-210">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ae73b-211">mottagare</span><span class="sxs-lookup"><span data-stu-id="ae73b-211">Recipients</span></span> |<span data-ttu-id="ae73b-212">Lista över e-postadresser.</span><span class="sxs-lookup"><span data-stu-id="ae73b-212">List of mail addresses.</span></span> |
| <span data-ttu-id="ae73b-213">Ämne</span><span class="sxs-lookup"><span data-stu-id="ae73b-213">Subject</span></span> |<span data-ttu-id="ae73b-214">Ämnet för e-postmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="ae73b-214">The subject of the mail.</span></span> |
| <span data-ttu-id="ae73b-215">Bifogad fil</span><span class="sxs-lookup"><span data-stu-id="ae73b-215">Attachment</span></span> |<span data-ttu-id="ae73b-216">Bifogade filer stöds inte för närvarande, så att det alltid har värdet ”None”.</span><span class="sxs-lookup"><span data-stu-id="ae73b-216">Attachments are not currently supported, so this will always have a value of “None”.</span></span> |

<span data-ttu-id="ae73b-217">Följande är ett exempelsvar för en e-postavisering åtgärd med ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="ae73b-217">Following is a sample response for an email notification action with a threshold.</span></span>  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

<span data-ttu-id="ae73b-218">Använda Put-metoden med en unik åtgärds-ID för att skapa en ny e-åtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="ae73b-218">Use the Put method with a unique action ID to create a new e-mail action for a schedule.</span></span>  <span data-ttu-id="ae73b-219">I följande exempel skapas ett e-postmeddelande med en tröskel så skickas när resultatet av den sparade sökningen överskrider tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="ae73b-219">The following example creates an email notification with a threshold so the mail is sent when the results of the saved search exceed the threshold.</span></span>

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

<span data-ttu-id="ae73b-220">Använda Put-metoden med en befintlig åtgärds-ID om du vill ändra en e-åtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="ae73b-220">Use the Put method with an existing action ID to modify an e-mail action for a schedule.</span></span>  <span data-ttu-id="ae73b-221">Brödtexten i begäran måste innehålla etag för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="ae73b-221">The body of the request must include the etag of the action.</span></span>

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

#### <a name="remediation-actions"></a><span data-ttu-id="ae73b-222">Reparationsåtgärder</span><span class="sxs-lookup"><span data-stu-id="ae73b-222">Remediation actions</span></span>
<span data-ttu-id="ae73b-223">Reparationer startar en runbook i Azure Automation som försöker åtgärda problemet som identifieras av aviseringen.</span><span class="sxs-lookup"><span data-stu-id="ae73b-223">Remediations start a runbook in Azure Automation that attempts to correct the problem identified by the alert.</span></span>  <span data-ttu-id="ae73b-224">Du måste skapa en webhook för den runbook som används i en Reparationsåtgärd och anger sedan URI: N i egenskapen WebhookUri.</span><span class="sxs-lookup"><span data-stu-id="ae73b-224">You must create a webhook for the runbook used in a remediation action and then specify the URI in the WebhookUri property.</span></span>  <span data-ttu-id="ae73b-225">När du skapar den här åtgärden med hjälp av OMS-konsolen, skapas automatiskt en ny webhook för runbook.</span><span class="sxs-lookup"><span data-stu-id="ae73b-225">When you create this action using the OMS console, a new webhook is automatically created for the runbook.</span></span>

<span data-ttu-id="ae73b-226">Reparationer innehålla egenskaperna i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="ae73b-226">Remediations include the properties in the following table.</span></span>

| <span data-ttu-id="ae73b-227">Egenskap</span><span class="sxs-lookup"><span data-stu-id="ae73b-227">Property</span></span> | <span data-ttu-id="ae73b-228">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ae73b-228">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ae73b-229">RunbookName</span><span class="sxs-lookup"><span data-stu-id="ae73b-229">RunbookName</span></span> |<span data-ttu-id="ae73b-230">Namnet på runbook.</span><span class="sxs-lookup"><span data-stu-id="ae73b-230">Name of the runbook.</span></span> <span data-ttu-id="ae73b-231">Detta måste matcha en publicerad runbook i automation-kontot som konfigurerats i Automation-lösningen i OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="ae73b-231">This must match a published runbook in the automation account configured in the Automation Solution in your OMS workspace.</span></span> |
| <span data-ttu-id="ae73b-232">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="ae73b-232">WebhookUri</span></span> |<span data-ttu-id="ae73b-233">URI för webhooken.</span><span class="sxs-lookup"><span data-stu-id="ae73b-233">URI of the webhook.</span></span> |
| <span data-ttu-id="ae73b-234">Förfallodatum</span><span class="sxs-lookup"><span data-stu-id="ae73b-234">Expiry</span></span> |<span data-ttu-id="ae73b-235">Förfallodatum och tid för webhooken.</span><span class="sxs-lookup"><span data-stu-id="ae73b-235">The expiration date and time of the webhook.</span></span>  <span data-ttu-id="ae73b-236">Om webhooken inte har ett förfallodatum, kan det vara ett giltigt framtida datum.</span><span class="sxs-lookup"><span data-stu-id="ae73b-236">If the webhook doesn’t have an expiration, then this can be any valid future date.</span></span> |

<span data-ttu-id="ae73b-237">Följande är ett exempelsvar för en Reparationsåtgärd med ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="ae73b-237">Following is a sample response for a remediation action with a threshold.</span></span>

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

<span data-ttu-id="ae73b-238">Använda Put-metoden med en unik åtgärds-ID för att skapa en ny Reparationsåtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="ae73b-238">Use the Put method with a unique action ID to create a new remediation action for a schedule.</span></span>  <span data-ttu-id="ae73b-239">I följande exempel skapas en med ett tröskelvärde så runbook startas om resultatet av den sparade sökningen överstiger tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="ae73b-239">The following example creates a remediation with a threshold so the runbook is started when the results of the saved search exceed the threshold.</span></span>

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

<span data-ttu-id="ae73b-240">Använda Put-metoden med en befintlig åtgärds-ID om du vill ändra en Reparationsåtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="ae73b-240">Use the Put method with an existing action ID to modify a remediation action for a schedule.</span></span>  <span data-ttu-id="ae73b-241">Brödtexten i begäran måste innehålla etag för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="ae73b-241">The body of the request must include the etag of the action.</span></span>

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a><span data-ttu-id="ae73b-242">Exempel</span><span class="sxs-lookup"><span data-stu-id="ae73b-242">Example</span></span>
<span data-ttu-id="ae73b-243">Följande är en komplett exempel att skapa en ny e-postavisering.</span><span class="sxs-lookup"><span data-stu-id="ae73b-243">Following is a complete example to create a new email alert.</span></span>  <span data-ttu-id="ae73b-244">Detta skapar ett nytt schema tillsammans med en åtgärd som innehåller ett tröskelvärde och e-post.</span><span class="sxs-lookup"><span data-stu-id="ae73b-244">This creates a new schedule along with an action containing a threshold and email.</span></span>

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $resourceGroup  = "MyResourceGroup"    
    $workspaceName    = "MyWorkspace"
    $searchId       = "MySearch"
    $scheduleId     = "MySchedule"
    $thresholdId    = "MyThreshold"
    $actionId       = "MyEmailAction"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/?api-version=2015-03-20 $scheduleJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Severity':'Warning', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/actions/$actionId/?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a><span data-ttu-id="ae73b-245">Webhook-åtgärder</span><span class="sxs-lookup"><span data-stu-id="ae73b-245">Webhook actions</span></span>
<span data-ttu-id="ae73b-246">Webhook-åtgärder kan du starta en process genom att anropa en URL och du kan också tillhandahålla en nyttolast som ska skickas.</span><span class="sxs-lookup"><span data-stu-id="ae73b-246">Webhook actions start a process by calling a URL and optionally providing a payload to be sent.</span></span>  <span data-ttu-id="ae73b-247">De liknar reparationsåtgärder förutom de är avsedda för webhooks som kan anropa processer än Azure Automation-runbooks.</span><span class="sxs-lookup"><span data-stu-id="ae73b-247">They are similar to Remediation actions except they are meant for webhooks that may invoke processes other than Azure Automation runbooks.</span></span>  <span data-ttu-id="ae73b-248">De ger också ytterligare alternativ för att tillhandahålla en nyttolast som ska levereras till fjärrprocessen.</span><span class="sxs-lookup"><span data-stu-id="ae73b-248">They also provide the additional option of providing a payload to be delivered to the remote process.</span></span>

<span data-ttu-id="ae73b-249">Webhook-åtgärder har inte ett tröskelvärde men i stället bör läggas till ett schema som har en åtgärd med ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="ae73b-249">Webhook actions do not have a threshold but instead should be added to a schedule that has an Alert action with a threshold.</span></span>  <span data-ttu-id="ae73b-250">Du kan lägga till flera Webhook-åtgärder som alla körs när tröskelvärdet är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="ae73b-250">You can add multiple Webhook actions that will all be run when the threshold is met.</span></span>

<span data-ttu-id="ae73b-251">Webhook-åtgärder innehålla egenskaperna i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="ae73b-251">Webhook actions include the properties in the following table.</span></span>

| <span data-ttu-id="ae73b-252">Egenskap</span><span class="sxs-lookup"><span data-stu-id="ae73b-252">Property</span></span> | <span data-ttu-id="ae73b-253">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ae73b-253">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ae73b-254">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="ae73b-254">WebhookUri</span></span> |<span data-ttu-id="ae73b-255">Ämnet för e-postmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="ae73b-255">The subject of the mail.</span></span> |
| <span data-ttu-id="ae73b-256">CustomPayload</span><span class="sxs-lookup"><span data-stu-id="ae73b-256">CustomPayload</span></span> |<span data-ttu-id="ae73b-257">Anpassad nyttolast skickas till webhooken.</span><span class="sxs-lookup"><span data-stu-id="ae73b-257">Custom payload to be sent to the webhook.</span></span>  <span data-ttu-id="ae73b-258">Formatet beror på vad webhooken förväntas.</span><span class="sxs-lookup"><span data-stu-id="ae73b-258">The format will depend on what the webhook is expecting.</span></span> |

<span data-ttu-id="ae73b-259">Följande är ett exempelsvar för webhook åtgärden och en associerad åtgärd med ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="ae73b-259">Following is a sample response for webhook action and an associated alert action with a threshold.</span></span>

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a><span data-ttu-id="ae73b-260">Skapa eller redigera en webhook-åtgärd</span><span class="sxs-lookup"><span data-stu-id="ae73b-260">Create or edit a webhook action</span></span>
<span data-ttu-id="ae73b-261">Använda Put-metoden med en unik åtgärds-ID för att skapa en ny webhook-åtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="ae73b-261">Use the Put method with a unique action ID to create a new webhook action for a schedule.</span></span>  <span data-ttu-id="ae73b-262">I följande exempel skapas en Webhook-åtgärd och en åtgärd med ett tröskelvärde så att webhooken ska utlösas när resultatet av den sparade sökningen överstiger tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="ae73b-262">The following example creates a Webhook action and an Alert action with a threshold so that the webhook will be triggered when the results of the saved search exceed the threshold.</span></span>

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

<span data-ttu-id="ae73b-263">Använda Put-metoden med en befintlig åtgärds-ID om du vill ändra en webhook-åtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="ae73b-263">Use the Put method with an existing action ID to modify a webhook action for a schedule.</span></span>  <span data-ttu-id="ae73b-264">Brödtexten i begäran måste innehålla etag för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="ae73b-264">The body of the request must include the etag of the action.</span></span>

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a><span data-ttu-id="ae73b-265">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ae73b-265">Next steps</span></span>
* <span data-ttu-id="ae73b-266">Använd den [REST API för att utföra sökningar loggen](log-analytics-log-search-api.md) i logganalys.</span><span class="sxs-lookup"><span data-stu-id="ae73b-266">Use the [REST API to perform log searches](log-analytics-log-search-api.md) in Log Analytics.</span></span>

