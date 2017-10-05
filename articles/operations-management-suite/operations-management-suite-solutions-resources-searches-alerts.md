---
title: "Sparade sökningar och aviseringar i OMS-lösningar | Microsoft Docs"
description: "Lösningar i OMS inkluderar vanligtvis sparade sökningar i logganalys att analysera data som samlas in av lösningen.  De kan också definiera varningar som meddelar användaren eller automatiskt utföra åtgärder som svar på ett allvarligt problem.  Den här artikeln beskriver hur du definierar logganalys sparade sökningar och aviseringar i en ARM-mall så att de kan tas med i hanteringslösningar."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 21c42a747a08c5386c65d10190baf0054a7adef8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="adding-log-analytics-saved-searches-and-alerts-to-oms-management-solution-preview"></a><span data-ttu-id="f0558-105">Lägga till logganalys sparade sökningar och aviseringar till OMS-hanteringslösning (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="f0558-105">Adding Log Analytics saved searches and alerts to OMS management solution (Preview)</span></span>

> [!NOTE]
> <span data-ttu-id="f0558-106">Den här är dokumentationen preliminär för att skapa lösningar för hantering i OMS som för närvarande finns i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="f0558-106">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="f0558-107">Ett schema som beskrivs nedan kan ändras.</span><span class="sxs-lookup"><span data-stu-id="f0558-107">Any schema described below is subject to change.</span></span>   


<span data-ttu-id="f0558-108">[Lösningar för hantering i OMS](operations-management-suite-solutions.md) inkluderar vanligtvis [sparade sökningar](../log-analytics/log-analytics-log-searches.md) i logganalys att analysera data som samlas in av lösningen.</span><span class="sxs-lookup"><span data-stu-id="f0558-108">[Management solutions in OMS](operations-management-suite-solutions.md) will typically include [saved searches](../log-analytics/log-analytics-log-searches.md) in Log Analytics to analyze data collected by the solution.</span></span>  <span data-ttu-id="f0558-109">De kan också definiera [aviseringar](../log-analytics/log-analytics-alerts.md) att meddela användaren eller automatiskt utföra åtgärder som svar på ett allvarligt problem.</span><span class="sxs-lookup"><span data-stu-id="f0558-109">They may also define [alerts](../log-analytics/log-analytics-alerts.md) to notify the user or automatically take action in response to a critical issue.</span></span>  <span data-ttu-id="f0558-110">Den här artikeln beskriver hur du definierar logganalys sparade sökningar och aviseringar i en [resurshantering mallen](../resource-manager-template-walkthrough.md) så att de kan tas med i [hanteringslösningar](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="f0558-110">This article describes how to define Log Analytics saved searches and alerts in a [Resource Management template](../resource-manager-template-walkthrough.md) so they can be included in [management solutions](operations-management-suite-solutions-creating.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f0558-111">Exemplen i den här artikeln använder parametrar och variabler som är obligatoriska eller vanligt att hanteringslösningar och beskrivs i [och skapa lösningar för hantering i Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span><span class="sxs-lookup"><span data-stu-id="f0558-111">The samples in this article use parameters and variables that are either required or common to management solutions  and described in [Creating management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="f0558-112">Krav</span><span class="sxs-lookup"><span data-stu-id="f0558-112">Prerequisites</span></span>
<span data-ttu-id="f0558-113">Den här artikeln förutsätter att du redan är bekant med [skapar en lösning för](operations-management-suite-solutions-creating.md) och struktur för ett [ARM-mallen](../resource-group-authoring-templates.md) och lösningsfilen.</span><span class="sxs-lookup"><span data-stu-id="f0558-113">This article assumes that you're already familiar with how to [create a management solution](operations-management-suite-solutions-creating.md) and the structure of an [ARM template](../resource-group-authoring-templates.md) and solution file.</span></span>


## <a name="log-analytics-workspace"></a><span data-ttu-id="f0558-114">Log Analytics-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="f0558-114">Log Analytics Workspace</span></span>
<span data-ttu-id="f0558-115">Alla resurser i logganalys finns i en [arbetsytan](../log-analytics/log-analytics-manage-access.md).</span><span class="sxs-lookup"><span data-stu-id="f0558-115">All resources in Log Analytics are contained in a [workspace](../log-analytics/log-analytics-manage-access.md).</span></span>  <span data-ttu-id="f0558-116">Enligt beskrivningen i [OMS arbetsytan och Automation-konto](operations-management-suite-solutions.md#oms-workspace-and-automation-account) arbetsytan ingår inte i hanteringslösningen men det måste finnas innan lösningen är installerad.</span><span class="sxs-lookup"><span data-stu-id="f0558-116">As described in [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) the workspace isn't included in the management solution but must exist before the solution is installed.</span></span>  <span data-ttu-id="f0558-117">Lösning installationen misslyckas om det inte är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="f0558-117">If it isn't available, then the solution install will fail.</span></span>

<span data-ttu-id="f0558-118">Namnet på arbetsytan är namnet på varje logganalys-resurs.</span><span class="sxs-lookup"><span data-stu-id="f0558-118">The name of the workspace is in the name of each Log Analytics resource.</span></span>  <span data-ttu-id="f0558-119">Detta görs i lösningen med den **arbetsytan** parameter som i följande exempel på en savedsearch resurs.</span><span class="sxs-lookup"><span data-stu-id="f0558-119">This is done in the solution with the **workspace** parameter as in the following example of a savedsearch resource.</span></span>

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"


## <a name="saved-searches"></a><span data-ttu-id="f0558-120">Sparade sökningar</span><span class="sxs-lookup"><span data-stu-id="f0558-120">Saved Searches</span></span>
<span data-ttu-id="f0558-121">Inkludera [sparade sökningar](../log-analytics/log-analytics-log-searches.md) i en lösning för att tillåta användare att fråga efter data som samlas in av din lösning.</span><span class="sxs-lookup"><span data-stu-id="f0558-121">Include [saved searches](../log-analytics/log-analytics-log-searches.md) in a solution to allow users to query data collected by your solution.</span></span>  <span data-ttu-id="f0558-122">Sparade sökningar visas under **Favoriter** i OMS-portalen och **sparade sökningar** i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f0558-122">Saved searches will appear under **Favorites** in the OMS portal and **Saved Searches** in the Azure portal .</span></span>  <span data-ttu-id="f0558-123">En sparad sökning krävs också för varje avisering.</span><span class="sxs-lookup"><span data-stu-id="f0558-123">A saved search is also required for each alert.</span></span>   

<span data-ttu-id="f0558-124">[Logganalys sparad sökning](../log-analytics/log-analytics-log-searches.md) resurser har en typ av `Microsoft.OperationalInsights/workspaces/savedSearches` och har följande struktur.</span><span class="sxs-lookup"><span data-stu-id="f0558-124">[Log Analytics saved search](../log-analytics/log-analytics-log-searches.md) resources have a type of `Microsoft.OperationalInsights/workspaces/savedSearches` and have the following structure.</span></span>  <span data-ttu-id="f0558-125">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra parameternamn.</span><span class="sxs-lookup"><span data-stu-id="f0558-125">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
        ],
        "tags": { },
        "properties": {
            "etag": "*",
            "query": "[variables('SavedSearch').Query]",
            "displayName": "[variables('SavedSearch').DisplayName]",
            "category": "[variables('SavedSearch').Category]"
        }
    }



<span data-ttu-id="f0558-126">Var och en av egenskaperna för en sparad sökning beskrivs i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="f0558-126">Each of the properties of a saved search are described in the following table.</span></span> 

| <span data-ttu-id="f0558-127">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f0558-127">Property</span></span> | <span data-ttu-id="f0558-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f0558-128">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="f0558-129">category</span><span class="sxs-lookup"><span data-stu-id="f0558-129">category</span></span> | <span data-ttu-id="f0558-130">Kategorin för den sparade sökningen.</span><span class="sxs-lookup"><span data-stu-id="f0558-130">The category for the saved search.</span></span>  <span data-ttu-id="f0558-131">Alla sparade sökningar i samma lösning kommer ofta att dela en enda kategori så att de grupperas tillsammans i konsolen.</span><span class="sxs-lookup"><span data-stu-id="f0558-131">Any saved searches in the same solution will often share a single category so they are grouped together in the console.</span></span> |
| <span data-ttu-id="f0558-132">visningsnamn</span><span class="sxs-lookup"><span data-stu-id="f0558-132">displayname</span></span> | <span data-ttu-id="f0558-133">Namnet som visas för den sparade sökningen i portalen.</span><span class="sxs-lookup"><span data-stu-id="f0558-133">Name to display for the saved search in the portal.</span></span> |
| <span data-ttu-id="f0558-134">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="f0558-134">query</span></span> | <span data-ttu-id="f0558-135">Frågan ska köras.</span><span class="sxs-lookup"><span data-stu-id="f0558-135">Query to run.</span></span> |

> [!NOTE]
> <span data-ttu-id="f0558-136">Du kan behöva använda escape-tecken i fråga om den innehåller tecken som kan tolkas som JSON.</span><span class="sxs-lookup"><span data-stu-id="f0558-136">You may need to use escape characters in the query if it includes characters that could be interpreted as JSON.</span></span>  <span data-ttu-id="f0558-137">Om din fråga var till exempel **typ: AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write”**, bör vara skriven i lösningsfilen som **typ: AzureActivity OperationName:\"Microsoft.Compute/virtualMachines/write\"**.</span><span class="sxs-lookup"><span data-stu-id="f0558-137">For example, if your query was **Type:AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write"**, it should be written in the solution file as **Type:AzureActivity OperationName:\"Microsoft.Compute/virtualMachines/write\"**.</span></span>

## <a name="alerts"></a><span data-ttu-id="f0558-138">Aviseringar</span><span class="sxs-lookup"><span data-stu-id="f0558-138">Alerts</span></span>
<span data-ttu-id="f0558-139">[Logga Analytics varningar](../log-analytics/log-analytics-alerts.md) skapas av Varningsregler som kör en sparad sökning med regelbundna intervall.</span><span class="sxs-lookup"><span data-stu-id="f0558-139">[Log Analytics alerts](../log-analytics/log-analytics-alerts.md) are created by alert rules that run a saved search on a regular interval.</span></span>  <span data-ttu-id="f0558-140">Om resultatet av frågan matchar de angivna villkoren, skapas en avisering post och en eller flera åtgärder körs.</span><span class="sxs-lookup"><span data-stu-id="f0558-140">If the results of the query match specified criteria, an alert record is created and one or more actions are run.</span></span>  

<span data-ttu-id="f0558-141">Varningsregler i en hanteringslösning består av följande tre olika resurser.</span><span class="sxs-lookup"><span data-stu-id="f0558-141">Alert rules in a management solution are made up of the following three different resources.</span></span>

- <span data-ttu-id="f0558-142">**Sparad sökning.**</span><span class="sxs-lookup"><span data-stu-id="f0558-142">**Saved search.**</span></span>  <span data-ttu-id="f0558-143">Definierar sökningen loggen som ska köras.</span><span class="sxs-lookup"><span data-stu-id="f0558-143">Defines the log search that will be run.</span></span>  <span data-ttu-id="f0558-144">Flera Varningsregler kan dela en sparad sökning.</span><span class="sxs-lookup"><span data-stu-id="f0558-144">Multiple alert rules can share a single saved search.</span></span>
- <span data-ttu-id="f0558-145">**Schema.**</span><span class="sxs-lookup"><span data-stu-id="f0558-145">**Schedule.**</span></span>  <span data-ttu-id="f0558-146">Definierar hur ofta loggen sökningen ska köras.</span><span class="sxs-lookup"><span data-stu-id="f0558-146">Defines how often the log search will be run.</span></span>  <span data-ttu-id="f0558-147">Varje varningsregeln ska ha ett och endast ett schema.</span><span class="sxs-lookup"><span data-stu-id="f0558-147">Each alert rule will have one and only one schedule.</span></span>
- <span data-ttu-id="f0558-148">**Åtgärd för aviseringen.**</span><span class="sxs-lookup"><span data-stu-id="f0558-148">**Alert action.**</span></span>  <span data-ttu-id="f0558-149">Varje regel för varning har en åtgärd resurs med en typ av **avisering** som definierar information om aviseringen, till exempel kriterier för när en avisering post skapas och den avisering allvarlighetsgrad.</span><span class="sxs-lookup"><span data-stu-id="f0558-149">Each alert rule will have one action resource with a type of **Alert** that defines the details of the alert such as the criteria for when an alert record will be created and the alert's severity.</span></span>  <span data-ttu-id="f0558-150">Åtgärden resursen kommer du även definiera ett e-post och runbook-svar.</span><span class="sxs-lookup"><span data-stu-id="f0558-150">The action resource will optionally define a mail and runbook response.</span></span>
- <span data-ttu-id="f0558-151">**Webhook-åtgärd (valfritt).**</span><span class="sxs-lookup"><span data-stu-id="f0558-151">**Webhook action (optional).**</span></span>  <span data-ttu-id="f0558-152">Om varningsregeln ska anropa en webhook, så det krävs en ytterligare åtgärd resurs med en typ av **Webhook**.</span><span class="sxs-lookup"><span data-stu-id="f0558-152">If the alert rule will call a webhook, then it requires an additional action resource with a type of **Webhook**.</span></span>    

<span data-ttu-id="f0558-153">Sparad sökning resurser som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="f0558-153">Saved search resources are described above.</span></span>  <span data-ttu-id="f0558-154">De andra resurserna beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="f0558-154">The other resources are described below.</span></span>


### <a name="schedule-resource"></a><span data-ttu-id="f0558-155">Schema för resurs</span><span class="sxs-lookup"><span data-stu-id="f0558-155">Schedule resource</span></span>

<span data-ttu-id="f0558-156">En sparad sökning kan ha en eller flera scheman med ett schema som representerar en separat avisering regel.</span><span class="sxs-lookup"><span data-stu-id="f0558-156">A saved search can have one or more schedules with each schedule representing a separate alert rule.</span></span> <span data-ttu-id="f0558-157">Schemat definierar hur ofta sökningen ska köras och det tidsintervall under vilken data ska hämtas.</span><span class="sxs-lookup"><span data-stu-id="f0558-157">The schedule defines how often the search is run and the time interval over which the data is retrieved.</span></span>  <span data-ttu-id="f0558-158">Schemalägga resurser har en typ av `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` och har följande struktur.</span><span class="sxs-lookup"><span data-stu-id="f0558-158">Schedule resources have a type of `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` and have the following structure.</span></span> <span data-ttu-id="f0558-159">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra parameternamn.</span><span class="sxs-lookup"><span data-stu-id="f0558-159">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 


    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name)]"
        ],
        "properties": {
            "etag": "*",
            "interval": "[variables('Schedule').Interval]",
            "queryTimeSpan": "[variables('Schedule').TimeSpan]",
            "enabled": "[variables('Schedule').Enabled]"
        }
    }



<span data-ttu-id="f0558-160">I följande tabell beskrivs egenskaperna för schemalägga resurser.</span><span class="sxs-lookup"><span data-stu-id="f0558-160">The properties for schedule resources are described in the following table.</span></span>

| <span data-ttu-id="f0558-161">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="f0558-161">Element name</span></span> | <span data-ttu-id="f0558-162">Krävs</span><span class="sxs-lookup"><span data-stu-id="f0558-162">Required</span></span> | <span data-ttu-id="f0558-163">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f0558-163">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="f0558-164">aktiverad</span><span class="sxs-lookup"><span data-stu-id="f0558-164">enabled</span></span>       | <span data-ttu-id="f0558-165">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-165">Yes</span></span> | <span data-ttu-id="f0558-166">Anger om aviseringen är aktiverad när den skapas.</span><span class="sxs-lookup"><span data-stu-id="f0558-166">Specifies whether the alert is enabled when it's created.</span></span> |
| <span data-ttu-id="f0558-167">intervall</span><span class="sxs-lookup"><span data-stu-id="f0558-167">interval</span></span>      | <span data-ttu-id="f0558-168">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-168">Yes</span></span> | <span data-ttu-id="f0558-169">Hur ofta frågan körs i minuter.</span><span class="sxs-lookup"><span data-stu-id="f0558-169">How often the query runs in minutes.</span></span> |
| <span data-ttu-id="f0558-170">QueryTimeSpan</span><span class="sxs-lookup"><span data-stu-id="f0558-170">queryTimeSpan</span></span> | <span data-ttu-id="f0558-171">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-171">Yes</span></span> | <span data-ttu-id="f0558-172">Lång tid i minuter som ska utvärdera resultaten över.</span><span class="sxs-lookup"><span data-stu-id="f0558-172">Length of time in minutes over which to evaluate results.</span></span> |

<span data-ttu-id="f0558-173">Resursen schema ska beror på den sparade sökningen så att den har skapats innan schemat.</span><span class="sxs-lookup"><span data-stu-id="f0558-173">The schedule resource should depend on the saved search so that it's created before the schedule.</span></span>


### <a name="actions"></a><span data-ttu-id="f0558-174">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="f0558-174">Actions</span></span>
<span data-ttu-id="f0558-175">Det finns två typer av åtgärden resursen som anges av den **typen** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="f0558-175">There are two types of action resource specified by the **Type** property.</span></span>  <span data-ttu-id="f0558-176">Ett schema kräver en **avisering** åtgärd som definierar varningsregeln och vilka åtgärder som vidtas när en avisering skapas.</span><span class="sxs-lookup"><span data-stu-id="f0558-176">A schedule requires one **Alert** action which defines the details of the alert rule and what actions are taken when an alert is created.</span></span>  <span data-ttu-id="f0558-177">Det kan också innehålla en **Webhook** åtgärd om en webhook ska anropas från aviseringen.</span><span class="sxs-lookup"><span data-stu-id="f0558-177">It may also include a **Webhook** action if a webhook should be called from the alert.</span></span>  

<span data-ttu-id="f0558-178">Åtgärden resurser har en typ av `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.</span><span class="sxs-lookup"><span data-stu-id="f0558-178">Action resources have a type of `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.</span></span>  

#### <a name="alert-actions"></a><span data-ttu-id="f0558-179">Aviseringsåtgärder</span><span class="sxs-lookup"><span data-stu-id="f0558-179">Alert actions</span></span>

<span data-ttu-id="f0558-180">Varje schemat har en **avisering** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="f0558-180">Every schedule will have one **Alert** action.</span></span>  <span data-ttu-id="f0558-181">Detta definierar information om aviseringen och eventuellt meddelande- och reparationsloggarna åtgärder.</span><span class="sxs-lookup"><span data-stu-id="f0558-181">This defines the details of the alert and optionally notification and remediation actions.</span></span>  <span data-ttu-id="f0558-182">Ett meddelande skickas ett e-postmeddelande till en eller flera adresser.</span><span class="sxs-lookup"><span data-stu-id="f0558-182">A notification sends an email to one or more addresses.</span></span>  <span data-ttu-id="f0558-183">En startar en runbook i Azure Automation för att försöka åtgärda identifierade problem.</span><span class="sxs-lookup"><span data-stu-id="f0558-183">A remediation starts a runbook in Azure Automation to attempt to remediate the detected issue.</span></span>

<span data-ttu-id="f0558-184">Aviseringsåtgärder har följande struktur.</span><span class="sxs-lookup"><span data-stu-id="f0558-184">Alert actions have the following structure.</span></span>  <span data-ttu-id="f0558-185">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra parameternamn.</span><span class="sxs-lookup"><span data-stu-id="f0558-185">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 



    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Alert').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
        ],
        "properties": {
            "etag": "*",
            "type": "Alert",
            "name": "[variables('Alert').Name]",
            "description": "[variables('Alert').Description]",
            "severity": "[variables('Alert').Severity]",
            "threshold": {
                "operator": "[variables('Alert').Threshold.Operator]",
                "value": "[variables('Alert').Threshold.Value]",
                "metricsTrigger": {
                    "triggerCondition": "[variables('Alert').Threshold.Trigger.Condition]",
                    "operator": "[variables('Alert').Trigger.Operator]",
                    "value": "[variables('Alert').Trigger.Value]"
                },
            },
            "emailNotification": {
                "recipients": [
                    "[variables('Alert').Recipients]"
                ],
                "subject": "[variables('Alert').Subject]"
            },
            "remediation": {
                "runbookName": "[variables('Alert').Remedition.RunbookName]",
                "webhookUri": "[variables('Alert').Remedition.WebhookUri]"
            }
        }
    }

<span data-ttu-id="f0558-186">Egenskaper för Aviseringsåtgärd resurser beskrivs i följande tabeller.</span><span class="sxs-lookup"><span data-stu-id="f0558-186">The properties for Alert action resources are described in the following tables.</span></span>

| <span data-ttu-id="f0558-187">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="f0558-187">Element name</span></span> | <span data-ttu-id="f0558-188">Krävs</span><span class="sxs-lookup"><span data-stu-id="f0558-188">Required</span></span> | <span data-ttu-id="f0558-189">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f0558-189">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="f0558-190">Typ</span><span class="sxs-lookup"><span data-stu-id="f0558-190">Type</span></span> | <span data-ttu-id="f0558-191">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-191">Yes</span></span> | <span data-ttu-id="f0558-192">Typ av åtgärd.</span><span class="sxs-lookup"><span data-stu-id="f0558-192">Type of the action.</span></span>  <span data-ttu-id="f0558-193">Detta blir **avisering** för aviseringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="f0558-193">This will be **Alert** for alert actions.</span></span> |
| <span data-ttu-id="f0558-194">Namn</span><span class="sxs-lookup"><span data-stu-id="f0558-194">Name</span></span> | <span data-ttu-id="f0558-195">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-195">Yes</span></span> | <span data-ttu-id="f0558-196">Visningsnamn för aviseringen.</span><span class="sxs-lookup"><span data-stu-id="f0558-196">Display name for the alert.</span></span>  <span data-ttu-id="f0558-197">Detta är det namn som visas i konsolen för regeln.</span><span class="sxs-lookup"><span data-stu-id="f0558-197">This is the name that's displayed in the console for the alert rule.</span></span> |
| <span data-ttu-id="f0558-198">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f0558-198">Description</span></span> | <span data-ttu-id="f0558-199">Nej</span><span class="sxs-lookup"><span data-stu-id="f0558-199">No</span></span> | <span data-ttu-id="f0558-200">Valfri beskrivning av aviseringen.</span><span class="sxs-lookup"><span data-stu-id="f0558-200">Optional description of the alert.</span></span> |
| <span data-ttu-id="f0558-201">Allvarsgrad</span><span class="sxs-lookup"><span data-stu-id="f0558-201">Severity</span></span> | <span data-ttu-id="f0558-202">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-202">Yes</span></span> | <span data-ttu-id="f0558-203">Allvarlighetsgrad för aviseringen posten från följande värden:</span><span class="sxs-lookup"><span data-stu-id="f0558-203">Severity of the alert record from the following values:</span></span><br><br> <span data-ttu-id="f0558-204">**Kritiska**</span><span class="sxs-lookup"><span data-stu-id="f0558-204">**Critical**</span></span><br><span data-ttu-id="f0558-205">**Varning**</span><span class="sxs-lookup"><span data-stu-id="f0558-205">**Warning**</span></span><br><span data-ttu-id="f0558-206">**Information**</span><span class="sxs-lookup"><span data-stu-id="f0558-206">**Informational**</span></span> |


##### <a name="threshold"></a><span data-ttu-id="f0558-207">Tröskelvärde</span><span class="sxs-lookup"><span data-stu-id="f0558-207">Threshold</span></span>
<span data-ttu-id="f0558-208">Det här avsnittet krävs.</span><span class="sxs-lookup"><span data-stu-id="f0558-208">This section is required.</span></span>  <span data-ttu-id="f0558-209">Den definierar egenskaperna för tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="f0558-209">It defines the properties for the alert threshold.</span></span>

| <span data-ttu-id="f0558-210">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="f0558-210">Element name</span></span> | <span data-ttu-id="f0558-211">Krävs</span><span class="sxs-lookup"><span data-stu-id="f0558-211">Required</span></span> | <span data-ttu-id="f0558-212">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f0558-212">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="f0558-213">Operatorn</span><span class="sxs-lookup"><span data-stu-id="f0558-213">Operator</span></span> | <span data-ttu-id="f0558-214">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-214">Yes</span></span> | <span data-ttu-id="f0558-215">Operator för jämförelse från följande värden:</span><span class="sxs-lookup"><span data-stu-id="f0558-215">Operator for the comparison from the following values:</span></span><br><br><span data-ttu-id="f0558-216">**gt = större än<br>lt = mindre än**</span><span class="sxs-lookup"><span data-stu-id="f0558-216">**gt = greater than<br>lt = less than**</span></span> |
| <span data-ttu-id="f0558-217">Värde</span><span class="sxs-lookup"><span data-stu-id="f0558-217">Value</span></span> | <span data-ttu-id="f0558-218">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-218">Yes</span></span> | <span data-ttu-id="f0558-219">Värde att jämföra resultatet.</span><span class="sxs-lookup"><span data-stu-id="f0558-219">The value to compare the results.</span></span> |


##### <a name="metricstrigger"></a><span data-ttu-id="f0558-220">MetricsTrigger</span><span class="sxs-lookup"><span data-stu-id="f0558-220">MetricsTrigger</span></span>
<span data-ttu-id="f0558-221">Det här avsnittet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="f0558-221">This section is optional.</span></span>  <span data-ttu-id="f0558-222">Inkludera det för ett mått mätning aviseringen.</span><span class="sxs-lookup"><span data-stu-id="f0558-222">Include it for a metric measurement alert.</span></span>

> [!NOTE]
> <span data-ttu-id="f0558-223">Mått mätning aviseringar är för närvarande i förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="f0558-223">Metric measurement alerts are currently in public preview.</span></span> 

| <span data-ttu-id="f0558-224">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="f0558-224">Element name</span></span> | <span data-ttu-id="f0558-225">Krävs</span><span class="sxs-lookup"><span data-stu-id="f0558-225">Required</span></span> | <span data-ttu-id="f0558-226">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f0558-226">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="f0558-227">TriggerCondition</span><span class="sxs-lookup"><span data-stu-id="f0558-227">TriggerCondition</span></span> | <span data-ttu-id="f0558-228">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-228">Yes</span></span> | <span data-ttu-id="f0558-229">Anger om tröskelvärdet för totala antalet överträdelser eller på varandra följande överträdelser från följande värden:</span><span class="sxs-lookup"><span data-stu-id="f0558-229">Specifies whether the threshold is for total number of breaches or consecutive breaches from the following values:</span></span><br><br><span data-ttu-id="f0558-230">**Totalt antal<br>i följd**</span><span class="sxs-lookup"><span data-stu-id="f0558-230">**Total<br>Consecutive**</span></span> |
| <span data-ttu-id="f0558-231">Operatorn</span><span class="sxs-lookup"><span data-stu-id="f0558-231">Operator</span></span> | <span data-ttu-id="f0558-232">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-232">Yes</span></span> | <span data-ttu-id="f0558-233">Operator för jämförelse från följande värden:</span><span class="sxs-lookup"><span data-stu-id="f0558-233">Operator for the comparison from the following values:</span></span><br><br><span data-ttu-id="f0558-234">**gt = större än<br>lt = mindre än**</span><span class="sxs-lookup"><span data-stu-id="f0558-234">**gt = greater than<br>lt = less than**</span></span> |
| <span data-ttu-id="f0558-235">Värde</span><span class="sxs-lookup"><span data-stu-id="f0558-235">Value</span></span> | <span data-ttu-id="f0558-236">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-236">Yes</span></span> | <span data-ttu-id="f0558-237">Antal gånger som villkoret måste uppfyllas för att utlösa en avisering.</span><span class="sxs-lookup"><span data-stu-id="f0558-237">Number of the times the criteria must be met to trigger the alert.</span></span> |

##### <a name="throttling"></a><span data-ttu-id="f0558-238">Begränsning</span><span class="sxs-lookup"><span data-stu-id="f0558-238">Throttling</span></span>
<span data-ttu-id="f0558-239">Det här avsnittet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="f0558-239">This section is optional.</span></span>  <span data-ttu-id="f0558-240">Inkludera det här avsnittet om du vill undertrycka aviseringar från samma regel för vissa tidsperiod när en avisering har skapats.</span><span class="sxs-lookup"><span data-stu-id="f0558-240">Include this section if you want to suppress alerts from the same rule for some amount of time after an alert is created.</span></span>

| <span data-ttu-id="f0558-241">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="f0558-241">Element name</span></span> | <span data-ttu-id="f0558-242">Krävs</span><span class="sxs-lookup"><span data-stu-id="f0558-242">Required</span></span> | <span data-ttu-id="f0558-243">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f0558-243">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="f0558-244">DurationInMinutes</span><span class="sxs-lookup"><span data-stu-id="f0558-244">DurationInMinutes</span></span> | <span data-ttu-id="f0558-245">Ja om begränsning element som ingår</span><span class="sxs-lookup"><span data-stu-id="f0558-245">Yes if Throttling element included</span></span> | <span data-ttu-id="f0558-246">Antal minuter för att undertrycka aviseringar när en från samma varningsregeln har skapats.</span><span class="sxs-lookup"><span data-stu-id="f0558-246">Number of minutes to suppress alerts after one from the same alert rule is created.</span></span> |

##### <a name="emailnotification"></a><span data-ttu-id="f0558-247">EmailNotification</span><span class="sxs-lookup"><span data-stu-id="f0558-247">EmailNotification</span></span>
 <span data-ttu-id="f0558-248">Det här avsnittet är valfritt att inkludera det om du vill att aviseringen ska skicka e-post till en eller flera mottagare.</span><span class="sxs-lookup"><span data-stu-id="f0558-248">This section is optional  Include it if you want the alert to send mail to one or more recipients.</span></span>

| <span data-ttu-id="f0558-249">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="f0558-249">Element name</span></span> | <span data-ttu-id="f0558-250">Krävs</span><span class="sxs-lookup"><span data-stu-id="f0558-250">Required</span></span> | <span data-ttu-id="f0558-251">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f0558-251">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="f0558-252">mottagare</span><span class="sxs-lookup"><span data-stu-id="f0558-252">Recipients</span></span> | <span data-ttu-id="f0558-253">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-253">Yes</span></span> | <span data-ttu-id="f0558-254">Kommaavgränsad lista över e-postadresser för att skicka en avisering när en avisering skapas som i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="f0558-254">Comma delimited list of email addresses to send notification when an alert is created such as in the following example.</span></span><br><br><span data-ttu-id="f0558-255">**[ "recipient1@contoso.com", "recipient2@contoso.com" ]**</span><span class="sxs-lookup"><span data-stu-id="f0558-255">**[ "recipient1@contoso.com", "recipient2@contoso.com" ]**</span></span> |
| <span data-ttu-id="f0558-256">Ämne</span><span class="sxs-lookup"><span data-stu-id="f0558-256">Subject</span></span> | <span data-ttu-id="f0558-257">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-257">Yes</span></span> | <span data-ttu-id="f0558-258">Ämnesrad i e-postmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="f0558-258">Subject line of the mail.</span></span> |
| <span data-ttu-id="f0558-259">Bifogad fil</span><span class="sxs-lookup"><span data-stu-id="f0558-259">Attachment</span></span> | <span data-ttu-id="f0558-260">Nej</span><span class="sxs-lookup"><span data-stu-id="f0558-260">No</span></span> | <span data-ttu-id="f0558-261">Bifogade filer stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="f0558-261">Attachments are not currently supported.</span></span>  <span data-ttu-id="f0558-262">Om det här elementet finns det ska vara **ingen**.</span><span class="sxs-lookup"><span data-stu-id="f0558-262">If this element is included, it should be **None**.</span></span> |


##### <a name="remediation"></a><span data-ttu-id="f0558-263">Reparation</span><span class="sxs-lookup"><span data-stu-id="f0558-263">Remediation</span></span>
<span data-ttu-id="f0558-264">Det här avsnittet är valfritt att inkludera det om du vill att en runbook att starta som svar på aviseringen.</span><span class="sxs-lookup"><span data-stu-id="f0558-264">This section is optional  Include it if you want a runbook to start in response to the alert.</span></span> |

| <span data-ttu-id="f0558-265">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="f0558-265">Element name</span></span> | <span data-ttu-id="f0558-266">Krävs</span><span class="sxs-lookup"><span data-stu-id="f0558-266">Required</span></span> | <span data-ttu-id="f0558-267">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f0558-267">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="f0558-268">RunbookName</span><span class="sxs-lookup"><span data-stu-id="f0558-268">RunbookName</span></span> | <span data-ttu-id="f0558-269">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-269">Yes</span></span> | <span data-ttu-id="f0558-270">Namnet på runbook att starta.</span><span class="sxs-lookup"><span data-stu-id="f0558-270">Name of the runbook to start.</span></span> |
| <span data-ttu-id="f0558-271">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="f0558-271">WebhookUri</span></span> | <span data-ttu-id="f0558-272">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-272">Yes</span></span> | <span data-ttu-id="f0558-273">URI för webhook för runbook.</span><span class="sxs-lookup"><span data-stu-id="f0558-273">Uri of the webhook for the runbook.</span></span> |
| <span data-ttu-id="f0558-274">Förfallodatum</span><span class="sxs-lookup"><span data-stu-id="f0558-274">Expiry</span></span> | <span data-ttu-id="f0558-275">Nej</span><span class="sxs-lookup"><span data-stu-id="f0558-275">No</span></span> | <span data-ttu-id="f0558-276">Datum och tid då reparationen upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="f0558-276">Date and time that the remediation expires.</span></span> |

#### <a name="webhook-actions"></a><span data-ttu-id="f0558-277">Webhook-åtgärder</span><span class="sxs-lookup"><span data-stu-id="f0558-277">Webhook actions</span></span>

<span data-ttu-id="f0558-278">Webhook-åtgärder kan du starta en process genom att anropa en URL och du kan också tillhandahålla en nyttolast som ska skickas.</span><span class="sxs-lookup"><span data-stu-id="f0558-278">Webhook actions start a process by calling a URL and optionally providing a payload to be sent.</span></span> <span data-ttu-id="f0558-279">De liknar reparationsåtgärder förutom de är avsedda för webhooks som kan anropa processer än Azure Automation-runbooks.</span><span class="sxs-lookup"><span data-stu-id="f0558-279">They are similar to Remediation actions except they are meant for webhooks that may invoke processes other than Azure Automation runbooks.</span></span> <span data-ttu-id="f0558-280">De ger också ytterligare alternativ för att tillhandahålla en nyttolast som ska levereras till fjärrprocessen.</span><span class="sxs-lookup"><span data-stu-id="f0558-280">They also provide the additional option of providing a payload to be delivered to the remote process.</span></span>

<span data-ttu-id="f0558-281">Om aviseringen ska anropa en webhook, så den behöver en åtgärd resurs med en typ av **Webhook** förutom den **avisering** åtgärd resurs.</span><span class="sxs-lookup"><span data-stu-id="f0558-281">If your alert will call a webhook, then it will need an action resource with a type of **Webhook** in addition to the **Alert** action resource.</span></span>  

    {
      "name": "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Webhook').Name)]",
      "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions/",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
      ],
      "properties": {
        "etag": "*",
        "type": "[variables('Alert').Webhook.Type]",
        "name": "[variables('Alert').Webhook.Name]",
        "webhookUri": "[variables('Alert').Webhook.webhookUri]",
        "customPayload": "[variables('Alert').Webhook.CustomPayLoad]"
      }
    }

<span data-ttu-id="f0558-282">Egenskaper för Webhook åtgärd resurser beskrivs i följande tabeller.</span><span class="sxs-lookup"><span data-stu-id="f0558-282">The properties for Webhook action resources are described in the following tables.</span></span>

| <span data-ttu-id="f0558-283">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="f0558-283">Element name</span></span> | <span data-ttu-id="f0558-284">Krävs</span><span class="sxs-lookup"><span data-stu-id="f0558-284">Required</span></span> | <span data-ttu-id="f0558-285">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f0558-285">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="f0558-286">typ</span><span class="sxs-lookup"><span data-stu-id="f0558-286">type</span></span> | <span data-ttu-id="f0558-287">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-287">Yes</span></span> | <span data-ttu-id="f0558-288">Typ av åtgärd.</span><span class="sxs-lookup"><span data-stu-id="f0558-288">Type of the action.</span></span>  <span data-ttu-id="f0558-289">Detta blir **Webhook** för webhook-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="f0558-289">This will be **Webhook** for webhook actions.</span></span> |
| <span data-ttu-id="f0558-290">namn</span><span class="sxs-lookup"><span data-stu-id="f0558-290">name</span></span> | <span data-ttu-id="f0558-291">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-291">Yes</span></span> | <span data-ttu-id="f0558-292">Visningsnamn för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="f0558-292">Display name for the action.</span></span>  <span data-ttu-id="f0558-293">Detta visas inte i konsolen.</span><span class="sxs-lookup"><span data-stu-id="f0558-293">This is not displayed in the console.</span></span> |
| <span data-ttu-id="f0558-294">wehookUri</span><span class="sxs-lookup"><span data-stu-id="f0558-294">wehookUri</span></span> | <span data-ttu-id="f0558-295">Ja</span><span class="sxs-lookup"><span data-stu-id="f0558-295">Yes</span></span> | <span data-ttu-id="f0558-296">URI för webhooken.</span><span class="sxs-lookup"><span data-stu-id="f0558-296">Uri for the webhook.</span></span> |
| <span data-ttu-id="f0558-297">CustomPayload</span><span class="sxs-lookup"><span data-stu-id="f0558-297">customPayload</span></span> | <span data-ttu-id="f0558-298">Nej</span><span class="sxs-lookup"><span data-stu-id="f0558-298">No</span></span> | <span data-ttu-id="f0558-299">Anpassad nyttolast skickas till webhooken.</span><span class="sxs-lookup"><span data-stu-id="f0558-299">Custom payload to be sent to the webhook.</span></span> <span data-ttu-id="f0558-300">Formatet beror på vad webhooken förväntas.</span><span class="sxs-lookup"><span data-stu-id="f0558-300">The format will depend on what the webhook is expecting.</span></span> |




## <a name="sample"></a><span data-ttu-id="f0558-301">Exempel</span><span class="sxs-lookup"><span data-stu-id="f0558-301">Sample</span></span>

<span data-ttu-id="f0558-302">Följande är ett exempel på en lösning som omfattar som innehåller följande resurser:</span><span class="sxs-lookup"><span data-stu-id="f0558-302">Following is a sample of a solution that include that includes the following resources:</span></span>

- <span data-ttu-id="f0558-303">Sparad sökning</span><span class="sxs-lookup"><span data-stu-id="f0558-303">Saved search</span></span>
- <span data-ttu-id="f0558-304">Schema</span><span class="sxs-lookup"><span data-stu-id="f0558-304">Schedule</span></span>
- <span data-ttu-id="f0558-305">Aviseringsåtgärden</span><span class="sxs-lookup"><span data-stu-id="f0558-305">Alert action</span></span>
- <span data-ttu-id="f0558-306">Webhook-åtgärd</span><span class="sxs-lookup"><span data-stu-id="f0558-306">Webhook action</span></span>

<span data-ttu-id="f0558-307">Används [standardlösningen parametrar](operations-management-suite-solutions-solution-file.md#parameters) variabler som ofta används i en lösning i stället för hardcoding värden i resursdefinitionerna.</span><span class="sxs-lookup"><span data-stu-id="f0558-307">The sample uses [standard solution parameters](operations-management-suite-solutions-solution-file.md#parameters) variables that would commonly be used in a solution as opposed to hardcoding values in the resource definitions.</span></span>

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0",
        "parameters": {
          "workspaceName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Log Analytics workspace"
            }
          },
          "accountName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Automation account"
            }
          },
          "workspaceregionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Log Analytics workspace"
            }
          },
          "regionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Automation account"
            }
          },
          "pricingTier": {
            "type": "string",
            "metadata": {
              "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
          },
          "recipients": {
            "type": "string",
            "metadata": {
              "Description": "List of recipients for the email alert separated by semicolon"
            }
          }
        },
        "variables": {
          "SolutionName": "MySolution",
          "SolutionVersion": "1.0",
          "SolutionPublisher": "Contoso",
          "ProductName": "SampleSolution",
    
          "LogAnalyticsApiVersion": "2015-11-01-preview",
    
          "MySearch": {
            "displayName": "Error records by hour",
            "query": "Type=MyRecord_CL | measure avg(Rating_d) by Instance_s interval 60minutes",
            "category": "Samples",
            "name": "Samples-Count of data"
          },
          "MyAlert": {
            "Name": "[toLower(concat('myalert-',uniqueString(resourceGroup().id, deployment().name)))]",
            "DisplayName": "My alert rule",
            "Description": "Sample alert.  Fires when 3 error records found over hour interval.",
            "Severity": "Critical",
            "ThresholdOperator": "gt",
            "ThresholdValue": 3,
            "Schedule": {
              "Name": "[toLower(concat('myschedule-',uniqueString(resourceGroup().id, deployment().name)))]",
              "Interval": 15,
              "TimeSpan": 60
            },
            "MetricsTrigger": {
              "TriggerCondition": "Consecutive",
              "Operator": "gt",
              "Value": 3
            },
            "ThrottleMinutes": 60,
            "Notification": {
              "Recipients": [
                "[parameters('recipients')]"
              ],
              "Subject": "Sample alert"
            },
            "Remediation": {
              "RunbookName": "MyRemediationRunbook",
              "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=TluBFH3GpX4IEAnFoImoAWLTULkjD%2bTS0yscyrr7ogw%3d"
            },
            "Webhook": {
              "Name": "MyWebhook",
              "Uri": "https://MyService.com/webhook",
              "Payload": "{\"field1\":\"value1\",\"field2\":\"value2\"}"
            }
          }
        },
        "resources": [
          {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
            "location": "[parameters('workspaceRegionId')]",
            "tags": { },
            "type": "Microsoft.OperationsManagement/solutions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
            ],
            "properties": {
              "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
              "referencedResources": [
              ],
              "containedResources": [
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
              ]
            },
            "plan": {
              "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
              "Version": "[variables('SolutionVersion')]",
              "product": "[variables('ProductName')]",
              "publisher": "[variables('SolutionPublisher')]",
              "promotionCode": ""
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [ ],
            "tags": { },
            "properties": {
              "etag": "*",
              "query": "[variables('MySearch').query]",
              "displayName": "[variables('MySearch').displayName]",
              "category": "[variables('MySearch').category]"
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name)]"
            ],
            "properties": {
              "etag": "*",
              "interval": "[variables('MyAlert').Schedule.Interval]",
              "queryTimeSpan": "[variables('MyAlert').Schedule.TimeSpan]",
              "enabled": true
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/',  variables('MyAlert').Schedule.Name, '/',  variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/',  variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Alert",
              "Name": "[variables('MyAlert').DisplayName]",
              "Description": "[variables('MyAlert').Description]",
              "Severity": "[variables('MyAlert').Severity]",
              "Threshold": {
                "Operator": "[variables('MyAlert').ThresholdOperator]",
                "Value": "[variables('MyAlert').ThresholdValue]",
                "MetricsTrigger": {
                  "TriggerCondition": "[variables('MyAlert').MetricsTrigger.TriggerCondition]",
                  "Operator": "[variables('MyAlert').MetricsTrigger.Operator]",
                  "Value": "[variables('MyAlert').MetricsTrigger.Value]"
                }
              },
              "Throttling": {
                "DurationInMinutes": "[variables('MyAlert').ThrottleMinutes]"
              },
              "EmailNotification": {
                "Recipients": "[variables('MyAlert').Notification.Recipients]",
                "Subject": "[variables('MyAlert').Notification.Subject]",
                "Attachment": "None"
              },
              "Remediation": {
                "RunbookName": "[variables('MyAlert').Remediation.RunbookName]",
                "WebhookUri": "[variables('MyAlert').Remediation.WebhookUri]"
              }
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name, '/', variables('MyAlert').Webhook.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]",
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name, '/actions/',variables('MyAlert').Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Webhook",
              "Name": "[variables('MyAlert').Webhook.Name]",
              "WebhookUri": "[variables('MyAlert').Webhook.Uri]",
              "CustomPayload": "[variables('MyAlert').Webhook.Payload]"
            }
          }
        ]
    }


<span data-ttu-id="f0558-308">Följande parameterfilen innehåller exempel värden för den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="f0558-308">The following parameter file provides samples values for this solution.</span></span>

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspacename": {
                "value": "myWorkspace"
            },
            "accountName": {
                "value": "myAccount"
            },
            "workspaceregionId": {
                "value": "East US"
            },
            "regionId": {
                "value": "East US 2"
            },
            "pricingTier": {
                "value": "Free"
            },
            "recipients": {
                "value": "recipient1@contoso.com;recipient2@contoso.com"
            }
        }
    }


## <a name="next-steps"></a><span data-ttu-id="f0558-309">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f0558-309">Next steps</span></span>
* <span data-ttu-id="f0558-310">[Lägga till vyer](operations-management-suite-solutions-resources-views.md) att din lösning för hantering.</span><span class="sxs-lookup"><span data-stu-id="f0558-310">[Add views](operations-management-suite-solutions-resources-views.md) to your management solution.</span></span>
* <span data-ttu-id="f0558-311">[Lägg till Automation-runbooks och andra resurser](operations-management-suite-solutions-resources-automation.md) att din lösning för hantering.</span><span class="sxs-lookup"><span data-stu-id="f0558-311">[Add Automation runbooks and other resources](operations-management-suite-solutions-resources-automation.md) to your management solution.</span></span>

