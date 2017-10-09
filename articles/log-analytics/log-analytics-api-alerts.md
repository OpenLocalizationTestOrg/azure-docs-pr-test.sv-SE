---
title: aaaUsing OMS-Log Analytics avisering REST-API
description: "hello Log Analytics avisering REST-API kan du toocreate och hantera aviseringar i logganalys som är en del av Operations Management Suite (OMS).  Den här artikeln innehåller information om hello API och flera exempel för att utföra olika åtgärder."
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
ms.openlocfilehash: 418dc7eb71d6151c6380b8925f1f147a0e13b178
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a><span data-ttu-id="0f668-104">Skapa och hantera Varningsregler i logganalys med REST API</span><span class="sxs-lookup"><span data-stu-id="0f668-104">Create and manage alert rules in Log Analytics with REST API</span></span>
<span data-ttu-id="0f668-105">hello Log Analytics avisering REST-API kan du toocreate och hantera aviseringar i Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="0f668-105">hello Log Analytics Alert REST API allows you toocreate and manage alerts in Operations Management Suite (OMS).</span></span>  <span data-ttu-id="0f668-106">Den här artikeln innehåller information om hello API och flera exempel för att utföra olika åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0f668-106">This article provides details of hello API and several examples for performing different operations.</span></span>

<span data-ttu-id="0f668-107">hello Log Analytics Sök REST API är RESTful och kan nås via hello Azure Resource Manager REST API.</span><span class="sxs-lookup"><span data-stu-id="0f668-107">hello Log Analytics Search REST API is RESTful and can be accessed via hello Azure Resource Manager REST API.</span></span> <span data-ttu-id="0f668-108">I det här dokumentet hittar du exempel där hello API kan nås från ett PowerShell-kommandorad med [ARMClient](https://github.com/projectkudu/ARMClient), en öppen källkod kommandoradsverktyg som förenklar anropar hello Azure Resource Manager API.</span><span class="sxs-lookup"><span data-stu-id="0f668-108">In this document you will find examples where hello API is accessed from a PowerShell command line using  [ARMClient](https://github.com/projectkudu/ARMClient), an open source command line tool that simplifies invoking hello Azure Resource Manager API.</span></span> <span data-ttu-id="0f668-109">hello användning av ARMClient och PowerShell är en av många alternativ tooaccess hello Log Analytics Sök-API.</span><span class="sxs-lookup"><span data-stu-id="0f668-109">hello use of ARMClient and PowerShell is one of many options tooaccess hello Log Analytics Search API.</span></span> <span data-ttu-id="0f668-110">Du kan använda hello RESTful Azure Resource Manager API toomake anrop tooOMS arbetsytor och utföra sökkommandon i dem med dessa verktyg.</span><span class="sxs-lookup"><span data-stu-id="0f668-110">With these tools you can utilize hello RESTful Azure Resource Manager API toomake calls tooOMS workspaces and perform search commands within them.</span></span> <span data-ttu-id="0f668-111">hello API kommer utdata sökresultat tooyou i JSON-format, vilket gör att du toouse hello sökresultat på många olika sätt programmässigt.</span><span class="sxs-lookup"><span data-stu-id="0f668-111">hello API will output search results tooyou in JSON format, allowing you toouse hello search results in many different ways programmatically.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0f668-112">Krav</span><span class="sxs-lookup"><span data-stu-id="0f668-112">Prerequisites</span></span>
<span data-ttu-id="0f668-113">Aviseringar kan för närvarande kan endast skapas med en sparad sökning i logganalys.</span><span class="sxs-lookup"><span data-stu-id="0f668-113">Currently, alerts can only be created with a saved search in Log Analytics.</span></span>  <span data-ttu-id="0f668-114">Du kan se toohello [loggen Sök REST API](log-analytics-log-search-api.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="0f668-114">You can refer toohello [Log Search REST API](log-analytics-log-search-api.md) for more information.</span></span>

## <a name="schedules"></a><span data-ttu-id="0f668-115">Scheman</span><span class="sxs-lookup"><span data-stu-id="0f668-115">Schedules</span></span>
<span data-ttu-id="0f668-116">En sparad sökning kan ha ett eller flera scheman.</span><span class="sxs-lookup"><span data-stu-id="0f668-116">A saved search can have one or more schedules.</span></span> <span data-ttu-id="0f668-117">hello schema definierar hur ofta hello sökningen ska köras och hello tidsintervall under vilka villkor hello identifieras.</span><span class="sxs-lookup"><span data-stu-id="0f668-117">hello schedule defines how often hello search is run and hello time interval over which hello criteria is identified.</span></span>
<span data-ttu-id="0f668-118">Scheman har hello egenskaper i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="0f668-118">Schedules have hello properties in hello following table.</span></span>

| <span data-ttu-id="0f668-119">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0f668-119">Property</span></span> | <span data-ttu-id="0f668-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0f668-120">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0f668-121">intervall</span><span class="sxs-lookup"><span data-stu-id="0f668-121">Interval</span></span> |<span data-ttu-id="0f668-122">Hur ofta hello sökning körs.</span><span class="sxs-lookup"><span data-stu-id="0f668-122">How often hello search is run.</span></span> <span data-ttu-id="0f668-123">Mätt i minuter.</span><span class="sxs-lookup"><span data-stu-id="0f668-123">Measured in minutes.</span></span> |
| <span data-ttu-id="0f668-124">QueryTimeSpan</span><span class="sxs-lookup"><span data-stu-id="0f668-124">QueryTimeSpan</span></span> |<span data-ttu-id="0f668-125">hello tidsintervall under vilka hello villkor utvärderas.</span><span class="sxs-lookup"><span data-stu-id="0f668-125">hello time interval over which hello criteria is evaluated.</span></span> <span data-ttu-id="0f668-126">Måste vara lika tooor större än intervall.</span><span class="sxs-lookup"><span data-stu-id="0f668-126">Must be equal tooor greater than Interval.</span></span> <span data-ttu-id="0f668-127">Mätt i minuter.</span><span class="sxs-lookup"><span data-stu-id="0f668-127">Measured in minutes.</span></span> |
| <span data-ttu-id="0f668-128">Version</span><span class="sxs-lookup"><span data-stu-id="0f668-128">Version</span></span> |<span data-ttu-id="0f668-129">hello API-version som används.</span><span class="sxs-lookup"><span data-stu-id="0f668-129">hello API version being used.</span></span>  <span data-ttu-id="0f668-130">För närvarande är ska detta alltid anges too1.</span><span class="sxs-lookup"><span data-stu-id="0f668-130">Currently, this should always be set too1.</span></span> |

<span data-ttu-id="0f668-131">Anta till exempel att en fråga med ett intervall på 15 minuter och Timespan 30 minuter.</span><span class="sxs-lookup"><span data-stu-id="0f668-131">For example, consider an event query with an Interval of 15 minutes and a Timespan of 30 minutes.</span></span> <span data-ttu-id="0f668-132">I det här fallet hello-frågan skulle köras var 15: e minut och skulle att utlösa en avisering om hello kriterier fortfarande tooresolve tootrue under ett 30 minuters intervall.</span><span class="sxs-lookup"><span data-stu-id="0f668-132">In this case, hello query would be run every 15 minutes, and an alert would be triggered if hello criteria continued tooresolve tootrue over a 30 minute span.</span></span>

### <a name="retrieving-schedules"></a><span data-ttu-id="0f668-133">Hämtar scheman</span><span class="sxs-lookup"><span data-stu-id="0f668-133">Retrieving schedules</span></span>
<span data-ttu-id="0f668-134">Använd hello hämta metoden tooretrieve alla scheman för en sparad sökning.</span><span class="sxs-lookup"><span data-stu-id="0f668-134">Use hello Get method tooretrieve all schedules for a saved search.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

<span data-ttu-id="0f668-135">Använd hello Get-metod med en schema-ID tooretrieve ett visst schema för en sparad sökning.</span><span class="sxs-lookup"><span data-stu-id="0f668-135">Use hello Get method with a schedule ID tooretrieve a particular schedule for a saved search.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

<span data-ttu-id="0f668-136">Följande är ett exempelsvar för ett schema.</span><span class="sxs-lookup"><span data-stu-id="0f668-136">Following is a sample response for a schedule.</span></span>

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

### <a name="creating-a-schedule"></a><span data-ttu-id="0f668-137">Skapa ett schema</span><span class="sxs-lookup"><span data-stu-id="0f668-137">Creating a schedule</span></span>
<span data-ttu-id="0f668-138">Använda hello Put-metoden med ett unikt schema-ID toocreate ett nytt schema.</span><span class="sxs-lookup"><span data-stu-id="0f668-138">Use hello Put method with a unique schedule ID toocreate a new schedule.</span></span>  <span data-ttu-id="0f668-139">Observera att två scheman inte kan ha hello samma ID även om de är kopplade till olika sparade sökningar.</span><span class="sxs-lookup"><span data-stu-id="0f668-139">Note that two schedules cannot have hello same ID even if they are associated with different saved searches.</span></span>  <span data-ttu-id="0f668-140">När du skapar ett schema i hello OMS-konsolen, skapas ett GUID för hello schema-ID.</span><span class="sxs-lookup"><span data-stu-id="0f668-140">When you create a schedule in hello OMS console, a GUID is created for hello schedule ID.</span></span>

> [!NOTE]
> <span data-ttu-id="0f668-141">hello namn för alla sparade sökningar, scheman och åtgärder som skapats med hello Log Analytics API måste vara i gemener.</span><span class="sxs-lookup"><span data-stu-id="0f668-141">hello name for all saved searches, schedules, and actions created with hello Log Analytics API must be in lowercase.</span></span>

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a><span data-ttu-id="0f668-142">Ändringar i schemat</span><span class="sxs-lookup"><span data-stu-id="0f668-142">Editing a schedule</span></span>
<span data-ttu-id="0f668-143">Använda hello Put-metoden med ett befintligt schema-ID för hello samma sparade söka toomodify som schemalägger.</span><span class="sxs-lookup"><span data-stu-id="0f668-143">Use hello Put method with an existing schedule ID for hello same saved search toomodify that schedule.</span></span>  <span data-ttu-id="0f668-144">hello brödtexten i begäran hello måste innehålla hello etag hello schema.</span><span class="sxs-lookup"><span data-stu-id="0f668-144">hello body of hello request must include hello etag of hello schedule.</span></span>

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a><span data-ttu-id="0f668-145">Ta bort scheman</span><span class="sxs-lookup"><span data-stu-id="0f668-145">Deleting schedules</span></span>
<span data-ttu-id="0f668-146">Använda hello Delete-metoden med en schema-ID toodelete ett schema.</span><span class="sxs-lookup"><span data-stu-id="0f668-146">Use hello Delete method with a schedule ID toodelete a schedule.</span></span>

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a><span data-ttu-id="0f668-147">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="0f668-147">Actions</span></span>
<span data-ttu-id="0f668-148">Ett schema kan ha flera åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0f668-148">A schedule can have multiple actions.</span></span> <span data-ttu-id="0f668-149">En åtgärd kan definiera en eller flera processer tooperform som skickar ett e-post eller starta en runbook eller den kan definiera ett tröskelvärde som bestämmer när hello resultaten av en sökning som matchar vissa villkor.</span><span class="sxs-lookup"><span data-stu-id="0f668-149">An action may define one or more processes tooperform such as sending a mail or starting a runbook, or it may define a threshold that determines when hello results of a search match some criteria.</span></span>  <span data-ttu-id="0f668-150">Vissa åtgärder definiera både så att hello processer utförs när hello tröskelvärdet har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="0f668-150">Some actions will define both so that hello processes are performed when hello threshold is met.</span></span>

<span data-ttu-id="0f668-151">Alla åtgärder har hello egenskaper i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="0f668-151">All actions have hello properties in hello following table.</span></span>  <span data-ttu-id="0f668-152">Olika typer av aviseringar har olika ytterligare egenskaper som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="0f668-152">Different types of alerts have different additional properties which are described below.</span></span>

| <span data-ttu-id="0f668-153">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0f668-153">Property</span></span> | <span data-ttu-id="0f668-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0f668-154">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0f668-155">Typ</span><span class="sxs-lookup"><span data-stu-id="0f668-155">Type</span></span> |<span data-ttu-id="0f668-156">Typ av hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0f668-156">Type of hello action.</span></span>  <span data-ttu-id="0f668-157">Hello möjliga värden är för närvarande aviseringen och Webhooken.</span><span class="sxs-lookup"><span data-stu-id="0f668-157">Currently hello possible values are Alert and Webhook.</span></span> |
| <span data-ttu-id="0f668-158">Namn</span><span class="sxs-lookup"><span data-stu-id="0f668-158">Name</span></span> |<span data-ttu-id="0f668-159">Visningsnamn för hello aviseringen.</span><span class="sxs-lookup"><span data-stu-id="0f668-159">Display name for hello alert.</span></span> |
| <span data-ttu-id="0f668-160">Version</span><span class="sxs-lookup"><span data-stu-id="0f668-160">Version</span></span> |<span data-ttu-id="0f668-161">hello API-version som används.</span><span class="sxs-lookup"><span data-stu-id="0f668-161">hello API version being used.</span></span>  <span data-ttu-id="0f668-162">För närvarande är ska detta alltid anges too1.</span><span class="sxs-lookup"><span data-stu-id="0f668-162">Currently, this should always be set too1.</span></span> |

### <a name="retrieving-actions"></a><span data-ttu-id="0f668-163">Hämta åtgärder</span><span class="sxs-lookup"><span data-stu-id="0f668-163">Retrieving actions</span></span>
<span data-ttu-id="0f668-164">Använd hello hämta metoden tooretrieve alla åtgärder för ett schema.</span><span class="sxs-lookup"><span data-stu-id="0f668-164">Use hello Get method tooretrieve all actions for a schedule.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

<span data-ttu-id="0f668-165">Använd hello Get-metod med hello åtgärd ID tooretrieve en viss åtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="0f668-165">Use hello Get method with hello action ID tooretrieve a particular action for a schedule.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a><span data-ttu-id="0f668-166">Skapa eller redigera åtgärder</span><span class="sxs-lookup"><span data-stu-id="0f668-166">Creating or editing actions</span></span>
<span data-ttu-id="0f668-167">Använda hello Put-metoden med en åtgärds-ID som är unikt toohello schema toocreate en ny åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0f668-167">Use hello Put method with an action ID that is unique toohello schedule toocreate a new action.</span></span>  <span data-ttu-id="0f668-168">När du skapar en åtgärd i hello OMS-konsolen är ett GUID för hello åtgärds-ID.</span><span class="sxs-lookup"><span data-stu-id="0f668-168">When you create an action in hello OMS console, a GUID is for hello action ID.</span></span>

> [!NOTE]
> <span data-ttu-id="0f668-169">hello namn för alla sparade sökningar, scheman och åtgärder som skapats med hello Log Analytics API måste vara i gemener.</span><span class="sxs-lookup"><span data-stu-id="0f668-169">hello name for all saved searches, schedules, and actions created with hello Log Analytics API must be in lowercase.</span></span>

<span data-ttu-id="0f668-170">Med en befintlig åtgärd ID för hello samma sparade söka toomodify som schemalägger hello Put-metoden.</span><span class="sxs-lookup"><span data-stu-id="0f668-170">Use hello Put method with an existing action ID for hello same saved search toomodify that schedule.</span></span>  <span data-ttu-id="0f668-171">hello brödtexten i begäran hello måste innehålla hello etag hello schema.</span><span class="sxs-lookup"><span data-stu-id="0f668-171">hello body of hello request must include hello etag of hello schedule.</span></span>

<span data-ttu-id="0f668-172">hello begäran format för att skapa en ny åtgärd varierar beroende på typen så exemplen finns i hello avsnitten nedan.</span><span class="sxs-lookup"><span data-stu-id="0f668-172">hello request format for creating a new action varies by action type so these examples are provided in hello sections below.</span></span>

### <a name="deleting-actions"></a><span data-ttu-id="0f668-173">Ta bort åtgärder</span><span class="sxs-lookup"><span data-stu-id="0f668-173">Deleting actions</span></span>
<span data-ttu-id="0f668-174">Använda hello Delete-metoden med hello åtgärd ID toodelete en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0f668-174">Use hello Delete method with hello action ID toodelete an action.</span></span>

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a><span data-ttu-id="0f668-175">Aviseringsåtgärder</span><span class="sxs-lookup"><span data-stu-id="0f668-175">Alert Actions</span></span>
<span data-ttu-id="0f668-176">Ett schema ska ha en Aviseringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="0f668-176">A Schedule should have one and only one Alert action.</span></span>  <span data-ttu-id="0f668-177">Aviseringsåtgärder har en eller flera av hello avsnitt i den följande tabellen hello.</span><span class="sxs-lookup"><span data-stu-id="0f668-177">Alert actions have one or more of hello sections in hello following table.</span></span>  <span data-ttu-id="0f668-178">Alla beskrivs i mer detalj nedan.</span><span class="sxs-lookup"><span data-stu-id="0f668-178">Each is described in further detail below.</span></span>

| <span data-ttu-id="0f668-179">Avsnittet</span><span class="sxs-lookup"><span data-stu-id="0f668-179">Section</span></span> | <span data-ttu-id="0f668-180">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0f668-180">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0f668-181">Tröskelvärde</span><span class="sxs-lookup"><span data-stu-id="0f668-181">Threshold</span></span> |<span data-ttu-id="0f668-182">Kriterier för när hello-åtgärden körs.</span><span class="sxs-lookup"><span data-stu-id="0f668-182">Criteria for when hello action is run.</span></span> |
| <span data-ttu-id="0f668-183">EmailNotification</span><span class="sxs-lookup"><span data-stu-id="0f668-183">EmailNotification</span></span> |<span data-ttu-id="0f668-184">Skicka e-post toomultiple mottagare.</span><span class="sxs-lookup"><span data-stu-id="0f668-184">Send mail toomultiple recipients.</span></span> |
| <span data-ttu-id="0f668-185">Reparation</span><span class="sxs-lookup"><span data-stu-id="0f668-185">Remediation</span></span> |<span data-ttu-id="0f668-186">Starta en runbook i Azure Automation tooattempt toocorrect identifierade problem.</span><span class="sxs-lookup"><span data-stu-id="0f668-186">Start a runbook in Azure Automation tooattempt toocorrect identified issue.</span></span> |

#### <a name="thresholds"></a><span data-ttu-id="0f668-187">Tröskelvärden</span><span class="sxs-lookup"><span data-stu-id="0f668-187">Thresholds</span></span>
<span data-ttu-id="0f668-188">En åtgärd måste ha ett och endast ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="0f668-188">An Alert action should have one and only one threshold.</span></span>  <span data-ttu-id="0f668-189">När hello resultaten av en sparad sökning matchar hello tröskelvärdet i en åtgärd som är associerade med sökningen kör några andra processer i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="0f668-189">When hello results of a saved search match hello threshold in an action associated with that search, then any other processes in that action are run.</span></span>  <span data-ttu-id="0f668-190">En åtgärd kan också innehålla endast ett tröskelvärde så att den kan användas med åtgärder för andra typer som inte innehåller tröskelvärden.</span><span class="sxs-lookup"><span data-stu-id="0f668-190">An action can also contain only a threshold so that it can be used with actions of other types that don’t contain thresholds.</span></span>

<span data-ttu-id="0f668-191">Tröskelvärden har hello egenskaper i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="0f668-191">Thresholds have hello properties in hello following table.</span></span>

| <span data-ttu-id="0f668-192">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0f668-192">Property</span></span> | <span data-ttu-id="0f668-193">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0f668-193">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0f668-194">Operatorn</span><span class="sxs-lookup"><span data-stu-id="0f668-194">Operator</span></span> |<span data-ttu-id="0f668-195">Operator för hello tröskelvärde jämförelse.</span><span class="sxs-lookup"><span data-stu-id="0f668-195">Operator for hello threshold comparison.</span></span> <br> <span data-ttu-id="0f668-196">gt = större än</span><span class="sxs-lookup"><span data-stu-id="0f668-196">gt = Greater Than</span></span> <br> <span data-ttu-id="0f668-197">lt = mindre än</span><span class="sxs-lookup"><span data-stu-id="0f668-197">lt = Less Than</span></span> |
| <span data-ttu-id="0f668-198">Värde</span><span class="sxs-lookup"><span data-stu-id="0f668-198">Value</span></span> |<span data-ttu-id="0f668-199">Värdet för hello tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="0f668-199">Value for hello threshold.</span></span> |

<span data-ttu-id="0f668-200">Anta till exempel att en fråga med ett intervall på 15 minuter, Timespan 30 minuter och ett tröskelvärde som är större än 10.</span><span class="sxs-lookup"><span data-stu-id="0f668-200">For example, consider an event query with an Interval of 15 minutes, a Timespan of 30 minutes, and a Threshold of greater than 10.</span></span> <span data-ttu-id="0f668-201">I det här fallet hello-frågan skulle köras var 15: e minut och skulle att utlösa en avisering om det returnerade 10 händelser som har skapats under ett 30 minuters intervall.</span><span class="sxs-lookup"><span data-stu-id="0f668-201">In this case, hello query would be run every 15 minutes, and an alert would be triggered if it returned 10 events that were created over a 30 minute span.</span></span>

<span data-ttu-id="0f668-202">Följande är ett exempelsvar för en åtgärd med bara ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="0f668-202">Following is a sample response for an action with only a threshold.</span></span>  

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

<span data-ttu-id="0f668-203">Använda hello Put-metoden med en unik åtgärd ID toocreate en ny tröskelvärdet åtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="0f668-203">Use hello Put method with a unique action ID toocreate a new threshold action for a schedule.</span></span>  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

<span data-ttu-id="0f668-204">Använda hello Put-metoden med en befintlig åtgärd ID toomodify en åtgärd för tröskelvärde för ett schema.</span><span class="sxs-lookup"><span data-stu-id="0f668-204">Use hello Put method with an existing action ID toomodify a threshold action for a schedule.</span></span>  <span data-ttu-id="0f668-205">hello brödtexten i begäran hello måste innehålla hello etag hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0f668-205">hello body of hello request must include hello etag of hello action.</span></span>

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a><span data-ttu-id="0f668-206">E-postavisering</span><span class="sxs-lookup"><span data-stu-id="0f668-206">Email Notification</span></span>
<span data-ttu-id="0f668-207">E-postaviseringar skicka e-post tooone eller flera mottagare.</span><span class="sxs-lookup"><span data-stu-id="0f668-207">Email Notifications send mail tooone or more recipients.</span></span>  <span data-ttu-id="0f668-208">De inkluderar hello egenskaper i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="0f668-208">They include hello properties in hello following table.</span></span>

| <span data-ttu-id="0f668-209">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0f668-209">Property</span></span> | <span data-ttu-id="0f668-210">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0f668-210">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0f668-211">mottagare</span><span class="sxs-lookup"><span data-stu-id="0f668-211">Recipients</span></span> |<span data-ttu-id="0f668-212">Lista över e-postadresser.</span><span class="sxs-lookup"><span data-stu-id="0f668-212">List of mail addresses.</span></span> |
| <span data-ttu-id="0f668-213">Ämne</span><span class="sxs-lookup"><span data-stu-id="0f668-213">Subject</span></span> |<span data-ttu-id="0f668-214">hello ämne hello e-post.</span><span class="sxs-lookup"><span data-stu-id="0f668-214">hello subject of hello mail.</span></span> |
| <span data-ttu-id="0f668-215">Bifogad fil</span><span class="sxs-lookup"><span data-stu-id="0f668-215">Attachment</span></span> |<span data-ttu-id="0f668-216">Bifogade filer stöds inte för närvarande, så att det alltid har värdet ”None”.</span><span class="sxs-lookup"><span data-stu-id="0f668-216">Attachments are not currently supported, so this will always have a value of “None”.</span></span> |

<span data-ttu-id="0f668-217">Följande är ett exempelsvar för en e-postavisering åtgärd med ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="0f668-217">Following is a sample response for an email notification action with a threshold.</span></span>  

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
            "Subject": "This is hello subject",
            "Attachment": "None"
        },
        "Version": 1
    }

<span data-ttu-id="0f668-218">Använda hello Put-metoden med en unik åtgärd ID toocreate en ny e-åtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="0f668-218">Use hello Put method with a unique action ID toocreate a new e-mail action for a schedule.</span></span>  <span data-ttu-id="0f668-219">hello följande exempel skapas ett e-postmeddelande med en tröskel så hello e-post skickas när hello resultaten av hello sparad sökning hello överskrids.</span><span class="sxs-lookup"><span data-stu-id="0f668-219">hello following example creates an email notification with a threshold so hello mail is sent when hello results of hello saved search exceed hello threshold.</span></span>

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

<span data-ttu-id="0f668-220">Använda hello Put-metoden med en befintlig åtgärd ID toomodify en e-åtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="0f668-220">Use hello Put method with an existing action ID toomodify an e-mail action for a schedule.</span></span>  <span data-ttu-id="0f668-221">hello brödtexten i begäran hello måste innehålla hello etag hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0f668-221">hello body of hello request must include hello etag of hello action.</span></span>

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

#### <a name="remediation-actions"></a><span data-ttu-id="0f668-222">Reparationsåtgärder</span><span class="sxs-lookup"><span data-stu-id="0f668-222">Remediation actions</span></span>
<span data-ttu-id="0f668-223">Reparationer startar en runbook i Azure Automation som försöker toocorrect hello problem som identifieras av hello avisering.</span><span class="sxs-lookup"><span data-stu-id="0f668-223">Remediations start a runbook in Azure Automation that attempts toocorrect hello problem identified by hello alert.</span></span>  <span data-ttu-id="0f668-224">Du måste skapa en webhook för hello-runbook som används i en Reparationsåtgärd och anger sedan hello URI i hello WebhookUri egenskapen.</span><span class="sxs-lookup"><span data-stu-id="0f668-224">You must create a webhook for hello runbook used in a remediation action and then specify hello URI in hello WebhookUri property.</span></span>  <span data-ttu-id="0f668-225">När du skapar den här åtgärden med hello OMS-konsolen, skapas automatiskt en ny webhook för hello runbook.</span><span class="sxs-lookup"><span data-stu-id="0f668-225">When you create this action using hello OMS console, a new webhook is automatically created for hello runbook.</span></span>

<span data-ttu-id="0f668-226">Reparationer inkludera hello egenskaper i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="0f668-226">Remediations include hello properties in hello following table.</span></span>

| <span data-ttu-id="0f668-227">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0f668-227">Property</span></span> | <span data-ttu-id="0f668-228">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0f668-228">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0f668-229">RunbookName</span><span class="sxs-lookup"><span data-stu-id="0f668-229">RunbookName</span></span> |<span data-ttu-id="0f668-230">Namnet på hello runbook.</span><span class="sxs-lookup"><span data-stu-id="0f668-230">Name of hello runbook.</span></span> <span data-ttu-id="0f668-231">Detta måste matcha en publicerad runbook i hello automation-kontot som konfigurerats i hello Automation-lösningen i OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="0f668-231">This must match a published runbook in hello automation account configured in hello Automation Solution in your OMS workspace.</span></span> |
| <span data-ttu-id="0f668-232">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="0f668-232">WebhookUri</span></span> |<span data-ttu-id="0f668-233">Hej webhook-URI.</span><span class="sxs-lookup"><span data-stu-id="0f668-233">URI of hello webhook.</span></span> |
| <span data-ttu-id="0f668-234">Förfallodatum</span><span class="sxs-lookup"><span data-stu-id="0f668-234">Expiry</span></span> |<span data-ttu-id="0f668-235">hello utgångsdatum och utgångstid av hello webhooken.</span><span class="sxs-lookup"><span data-stu-id="0f668-235">hello expiration date and time of hello webhook.</span></span>  <span data-ttu-id="0f668-236">Om hello webhook inte har ett förfallodatum, kan det vara ett giltigt framtida datum.</span><span class="sxs-lookup"><span data-stu-id="0f668-236">If hello webhook doesn’t have an expiration, then this can be any valid future date.</span></span> |

<span data-ttu-id="0f668-237">Följande är ett exempelsvar för en Reparationsåtgärd med ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="0f668-237">Following is a sample response for a remediation action with a threshold.</span></span>

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

<span data-ttu-id="0f668-238">Använda hello Put-metoden med en unik åtgärd ID toocreate nya Reparationsåtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="0f668-238">Use hello Put method with a unique action ID toocreate a new remediation action for a schedule.</span></span>  <span data-ttu-id="0f668-239">hello följande exempel skapas en med ett tröskelvärde så hello runbook startas när hello resultaten av hello sparad sökning hello överskrids.</span><span class="sxs-lookup"><span data-stu-id="0f668-239">hello following example creates a remediation with a threshold so hello runbook is started when hello results of hello saved search exceed hello threshold.</span></span>

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

<span data-ttu-id="0f668-240">Använda hello Put-metoden med en befintlig åtgärd ID toomodify en Reparationsåtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="0f668-240">Use hello Put method with an existing action ID toomodify a remediation action for a schedule.</span></span>  <span data-ttu-id="0f668-241">hello brödtexten i begäran hello måste innehålla hello etag hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0f668-241">hello body of hello request must include hello etag of hello action.</span></span>

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a><span data-ttu-id="0f668-242">Exempel</span><span class="sxs-lookup"><span data-stu-id="0f668-242">Example</span></span>
<span data-ttu-id="0f668-243">Följande är en komplett exempel toocreate en ny e-postavisering.</span><span class="sxs-lookup"><span data-stu-id="0f668-243">Following is a complete example toocreate a new email alert.</span></span>  <span data-ttu-id="0f668-244">Detta skapar ett nytt schema tillsammans med en åtgärd som innehåller ett tröskelvärde och e-post.</span><span class="sxs-lookup"><span data-stu-id="0f668-244">This creates a new schedule along with an action containing a threshold and email.</span></span>

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $resourceGroup  = "MyResourceGroup"    
    $workspaceName    = "MyWorkspace"
    $searchId       = "MySearch"
    $scheduleId     = "MySchedule"
    $thresholdId    = "MyThreshold"
    $actionId       = "MyEmailAction"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/?api-version=2015-03-20 $scheduleJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Severity':'Warning', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/actions/$actionId/?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a><span data-ttu-id="0f668-245">Webhook-åtgärder</span><span class="sxs-lookup"><span data-stu-id="0f668-245">Webhook actions</span></span>
<span data-ttu-id="0f668-246">Webhook-åtgärder kan du starta en process genom att anropa en URL och du kan också tillhandahålla en nyttolast toobe skickas.</span><span class="sxs-lookup"><span data-stu-id="0f668-246">Webhook actions start a process by calling a URL and optionally providing a payload toobe sent.</span></span>  <span data-ttu-id="0f668-247">De är liknande tooRemediation åtgärder förutom de är avsedda för webhooks som kan anropa processer än Azure Automation-runbooks.</span><span class="sxs-lookup"><span data-stu-id="0f668-247">They are similar tooRemediation actions except they are meant for webhooks that may invoke processes other than Azure Automation runbooks.</span></span>  <span data-ttu-id="0f668-248">De ger också hello ytterligare alternativ för att tillhandahålla en nyttolast toobe levereras toohello fjärrprocess.</span><span class="sxs-lookup"><span data-stu-id="0f668-248">They also provide hello additional option of providing a payload toobe delivered toohello remote process.</span></span>

<span data-ttu-id="0f668-249">Webhook-åtgärder har inte ett tröskelvärde men i stället bör läggas tooa schema som har en åtgärd med ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="0f668-249">Webhook actions do not have a threshold but instead should be added tooa schedule that has an Alert action with a threshold.</span></span>  <span data-ttu-id="0f668-250">Du kan lägga till flera Webhook-åtgärder som alla körs när hello tröskelvärdet har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="0f668-250">You can add multiple Webhook actions that will all be run when hello threshold is met.</span></span>

<span data-ttu-id="0f668-251">Webhook-åtgärder inkludera hello egenskaper i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="0f668-251">Webhook actions include hello properties in hello following table.</span></span>

| <span data-ttu-id="0f668-252">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0f668-252">Property</span></span> | <span data-ttu-id="0f668-253">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0f668-253">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0f668-254">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="0f668-254">WebhookUri</span></span> |<span data-ttu-id="0f668-255">hello ämne hello e-post.</span><span class="sxs-lookup"><span data-stu-id="0f668-255">hello subject of hello mail.</span></span> |
| <span data-ttu-id="0f668-256">CustomPayload</span><span class="sxs-lookup"><span data-stu-id="0f668-256">CustomPayload</span></span> |<span data-ttu-id="0f668-257">Anpassad nyttolast toobe skickas toohello webhooken.</span><span class="sxs-lookup"><span data-stu-id="0f668-257">Custom payload toobe sent toohello webhook.</span></span>  <span data-ttu-id="0f668-258">hello format beror på vilken hello webhook förväntas.</span><span class="sxs-lookup"><span data-stu-id="0f668-258">hello format will depend on what hello webhook is expecting.</span></span> |

<span data-ttu-id="0f668-259">Följande är ett exempelsvar för webhook åtgärden och en associerad åtgärd med ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="0f668-259">Following is a sample response for webhook action and an associated alert action with a threshold.</span></span>

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

#### <a name="create-or-edit-a-webhook-action"></a><span data-ttu-id="0f668-260">Skapa eller redigera en webhook-åtgärd</span><span class="sxs-lookup"><span data-stu-id="0f668-260">Create or edit a webhook action</span></span>
<span data-ttu-id="0f668-261">Använda hello Put-metoden med en unik åtgärd ID toocreate en ny webhook-åtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="0f668-261">Use hello Put method with a unique action ID toocreate a new webhook action for a schedule.</span></span>  <span data-ttu-id="0f668-262">hello följande exempel skapas en Webhook-åtgärd och en åtgärd med ett tröskelvärde så att hello webhook ska utlösas när hello resultaten av hello sparad sökning hello överskrids.</span><span class="sxs-lookup"><span data-stu-id="0f668-262">hello following example creates a Webhook action and an Alert action with a threshold so that hello webhook will be triggered when hello results of hello saved search exceed hello threshold.</span></span>

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

<span data-ttu-id="0f668-263">Använda hello Put-metoden med en befintlig åtgärd ID toomodify en webhook-åtgärd för ett schema.</span><span class="sxs-lookup"><span data-stu-id="0f668-263">Use hello Put method with an existing action ID toomodify a webhook action for a schedule.</span></span>  <span data-ttu-id="0f668-264">hello brödtexten i begäran hello måste innehålla hello etag hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0f668-264">hello body of hello request must include hello etag of hello action.</span></span>

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a><span data-ttu-id="0f668-265">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0f668-265">Next steps</span></span>
* <span data-ttu-id="0f668-266">Använd hello [REST API tooperform loggen sökningar](log-analytics-log-search-api.md) i logganalys.</span><span class="sxs-lookup"><span data-stu-id="0f668-266">Use hello [REST API tooperform log searches](log-analytics-log-search-api.md) in Log Analytics.</span></span>

