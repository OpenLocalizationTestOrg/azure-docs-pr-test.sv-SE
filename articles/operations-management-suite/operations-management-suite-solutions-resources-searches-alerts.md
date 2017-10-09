---
title: "aaaSaved sökningar och aviseringar i OMS-lösningar | Microsoft Docs"
description: "Lösningar i OMS inkluderar vanligtvis sparade sökningar i logganalys tooanalyze data som samlas in av hello-lösning.  De kan också definiera aviseringar toonotify hello användaren eller automatiskt utföra åtgärder i svaret tooa kritiska problem.  Den här artikeln beskriver hur toodefine logganalys sparade sökningar och aviseringar i en ARM-mall så att de kan tas med i hanteringslösningar."
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
ms.openlocfilehash: 93d7c5bbf061473833ca6c0a8e4d8e10d923f3ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="adding-log-analytics-saved-searches-and-alerts-toooms-management-solution-preview"></a><span data-ttu-id="dff50-105">Lägga till logganalys sparade sökningar och aviseringar tooOMS lösning (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="dff50-105">Adding Log Analytics saved searches and alerts tooOMS management solution (Preview)</span></span>

> [!NOTE]
> <span data-ttu-id="dff50-106">Den här är dokumentationen preliminär för att skapa lösningar för hantering i OMS som för närvarande finns i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="dff50-106">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="dff50-107">Ett schema som beskrivs nedan är ämne toochange.</span><span class="sxs-lookup"><span data-stu-id="dff50-107">Any schema described below is subject toochange.</span></span>   


<span data-ttu-id="dff50-108">[Lösningar för hantering i OMS](operations-management-suite-solutions.md) inkluderar vanligtvis [sparade sökningar](../log-analytics/log-analytics-log-searches.md) i logganalys tooanalyze data som samlas in av hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="dff50-108">[Management solutions in OMS](operations-management-suite-solutions.md) will typically include [saved searches](../log-analytics/log-analytics-log-searches.md) in Log Analytics tooanalyze data collected by hello solution.</span></span>  <span data-ttu-id="dff50-109">De kan också definiera [aviseringar](../log-analytics/log-analytics-alerts.md) toonotify hello användaren eller automatiskt utföra åtgärder i svaret tooa kritiska problem.</span><span class="sxs-lookup"><span data-stu-id="dff50-109">They may also define [alerts](../log-analytics/log-analytics-alerts.md) toonotify hello user or automatically take action in response tooa critical issue.</span></span>  <span data-ttu-id="dff50-110">Den här artikeln beskriver hur toodefine logganalys sparade sökningar och aviseringar i en [resurshantering mallen](../resource-manager-template-walkthrough.md) så att de kan tas med i [hanteringslösningar](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="dff50-110">This article describes how toodefine Log Analytics saved searches and alerts in a [Resource Management template](../resource-manager-template-walkthrough.md) so they can be included in [management solutions](operations-management-suite-solutions-creating.md).</span></span>

> [!NOTE]
> <span data-ttu-id="dff50-111">hello exempel i den här artikeln använder parametrar och variabler är antingen obligatorisk eller vanliga toomanagement lösningar som beskrivs i [och skapa lösningar för hantering i Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span><span class="sxs-lookup"><span data-stu-id="dff50-111">hello samples in this article use parameters and variables that are either required or common toomanagement solutions  and described in [Creating management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="dff50-112">Krav</span><span class="sxs-lookup"><span data-stu-id="dff50-112">Prerequisites</span></span>
<span data-ttu-id="dff50-113">Den här artikeln förutsätter att du redan är bekant med för[skapar en lösning för](operations-management-suite-solutions-creating.md) och hello struktur för ett [ARM-mallen](../resource-group-authoring-templates.md) och lösningsfilen.</span><span class="sxs-lookup"><span data-stu-id="dff50-113">This article assumes that you're already familiar with how too[create a management solution](operations-management-suite-solutions-creating.md) and hello structure of an [ARM template](../resource-group-authoring-templates.md) and solution file.</span></span>


## <a name="log-analytics-workspace"></a><span data-ttu-id="dff50-114">Log Analytics-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="dff50-114">Log Analytics Workspace</span></span>
<span data-ttu-id="dff50-115">Alla resurser i logganalys finns i en [arbetsytan](../log-analytics/log-analytics-manage-access.md).</span><span class="sxs-lookup"><span data-stu-id="dff50-115">All resources in Log Analytics are contained in a [workspace](../log-analytics/log-analytics-manage-access.md).</span></span>  <span data-ttu-id="dff50-116">Enligt beskrivningen i [OMS arbetsytan och Automation-konto](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello arbetsytan ingår inte i hello hanteringslösning men det måste finnas innan hello lösningen är installerad.</span><span class="sxs-lookup"><span data-stu-id="dff50-116">As described in [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello workspace isn't included in hello management solution but must exist before hello solution is installed.</span></span>  <span data-ttu-id="dff50-117">Hello lösning installationen misslyckas om den inte är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="dff50-117">If it isn't available, then hello solution install will fail.</span></span>

<span data-ttu-id="dff50-118">hello heter hello arbetsytan i hello namn för varje logganalys-resurs.</span><span class="sxs-lookup"><span data-stu-id="dff50-118">hello name of hello workspace is in hello name of each Log Analytics resource.</span></span>  <span data-ttu-id="dff50-119">Detta görs i hello lösningen med hello **arbetsytan** parameter som hello följande exempel på en savedsearch resurs.</span><span class="sxs-lookup"><span data-stu-id="dff50-119">This is done in hello solution with hello **workspace** parameter as in hello following example of a savedsearch resource.</span></span>

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"


## <a name="saved-searches"></a><span data-ttu-id="dff50-120">Sparade sökningar</span><span class="sxs-lookup"><span data-stu-id="dff50-120">Saved Searches</span></span>
<span data-ttu-id="dff50-121">Inkludera [sparade sökningar](../log-analytics/log-analytics-log-searches.md) i en lösning tooallow användare tooquery data som samlas in av din lösning.</span><span class="sxs-lookup"><span data-stu-id="dff50-121">Include [saved searches](../log-analytics/log-analytics-log-searches.md) in a solution tooallow users tooquery data collected by your solution.</span></span>  <span data-ttu-id="dff50-122">Sparade sökningar visas under **Favoriter** i hello OMS-portalen och **sparade sökningar** i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dff50-122">Saved searches will appear under **Favorites** in hello OMS portal and **Saved Searches** in hello Azure portal .</span></span>  <span data-ttu-id="dff50-123">En sparad sökning krävs också för varje avisering.</span><span class="sxs-lookup"><span data-stu-id="dff50-123">A saved search is also required for each alert.</span></span>   

<span data-ttu-id="dff50-124">[Logganalys sparad sökning](../log-analytics/log-analytics-log-searches.md) resurser har en typ av `Microsoft.OperationalInsights/workspaces/savedSearches` och ha hello följande struktur.</span><span class="sxs-lookup"><span data-stu-id="dff50-124">[Log Analytics saved search](../log-analytics/log-analytics-log-searches.md) resources have a type of `Microsoft.OperationalInsights/workspaces/savedSearches` and have hello following structure.</span></span>  <span data-ttu-id="dff50-125">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn.</span><span class="sxs-lookup"><span data-stu-id="dff50-125">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

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



<span data-ttu-id="dff50-126">Var och en av hello egenskaperna för en sparad sökning beskrivs i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="dff50-126">Each of hello properties of a saved search are described in hello following table.</span></span> 

| <span data-ttu-id="dff50-127">Egenskap</span><span class="sxs-lookup"><span data-stu-id="dff50-127">Property</span></span> | <span data-ttu-id="dff50-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dff50-128">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="dff50-129">category</span><span class="sxs-lookup"><span data-stu-id="dff50-129">category</span></span> | <span data-ttu-id="dff50-130">hello kategori för hello sparad sökning.</span><span class="sxs-lookup"><span data-stu-id="dff50-130">hello category for hello saved search.</span></span>  <span data-ttu-id="dff50-131">Alla sparade sökningar i hello som delar samma lösning ofta en enda kategori så att de grupperas tillsammans i hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="dff50-131">Any saved searches in hello same solution will often share a single category so they are grouped together in hello console.</span></span> |
| <span data-ttu-id="dff50-132">visningsnamn</span><span class="sxs-lookup"><span data-stu-id="dff50-132">displayname</span></span> | <span data-ttu-id="dff50-133">Namnet toodisplay för hello sparad sökning i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="dff50-133">Name toodisplay for hello saved search in hello portal.</span></span> |
| <span data-ttu-id="dff50-134">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="dff50-134">query</span></span> | <span data-ttu-id="dff50-135">Fråga toorun.</span><span class="sxs-lookup"><span data-stu-id="dff50-135">Query toorun.</span></span> |

> [!NOTE]
> <span data-ttu-id="dff50-136">Du kan behöva toouse escape-tecken i hello fråga om den innehåller tecken som kan tolkas som JSON.</span><span class="sxs-lookup"><span data-stu-id="dff50-136">You may need toouse escape characters in hello query if it includes characters that could be interpreted as JSON.</span></span>  <span data-ttu-id="dff50-137">Om din fråga var till exempel **typ: AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write”**, bör vara skriven i hello lösningsfilen som **typ: AzureActivity OperationName:\" Microsoft.Compute/virtualMachines/write\"**.</span><span class="sxs-lookup"><span data-stu-id="dff50-137">For example, if your query was **Type:AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write"**, it should be written in hello solution file as **Type:AzureActivity OperationName:\"Microsoft.Compute/virtualMachines/write\"**.</span></span>

## <a name="alerts"></a><span data-ttu-id="dff50-138">Aviseringar</span><span class="sxs-lookup"><span data-stu-id="dff50-138">Alerts</span></span>
<span data-ttu-id="dff50-139">[Logga Analytics varningar](../log-analytics/log-analytics-alerts.md) skapas av Varningsregler som kör en sparad sökning med regelbundna intervall.</span><span class="sxs-lookup"><span data-stu-id="dff50-139">[Log Analytics alerts](../log-analytics/log-analytics-alerts.md) are created by alert rules that run a saved search on a regular interval.</span></span>  <span data-ttu-id="dff50-140">Om hello resultaten av hello-frågan matchar angivet villkor, skapas en avisering post och en eller flera åtgärder körs.</span><span class="sxs-lookup"><span data-stu-id="dff50-140">If hello results of hello query match specified criteria, an alert record is created and one or more actions are run.</span></span>  

<span data-ttu-id="dff50-141">Varningsregler i en hanteringslösning består av följande tre olika resurser hello.</span><span class="sxs-lookup"><span data-stu-id="dff50-141">Alert rules in a management solution are made up of hello following three different resources.</span></span>

- <span data-ttu-id="dff50-142">**Sparad sökning.**</span><span class="sxs-lookup"><span data-stu-id="dff50-142">**Saved search.**</span></span>  <span data-ttu-id="dff50-143">Definierar hello loggen sökning som ska köras.</span><span class="sxs-lookup"><span data-stu-id="dff50-143">Defines hello log search that will be run.</span></span>  <span data-ttu-id="dff50-144">Flera Varningsregler kan dela en sparad sökning.</span><span class="sxs-lookup"><span data-stu-id="dff50-144">Multiple alert rules can share a single saved search.</span></span>
- <span data-ttu-id="dff50-145">**Schema.**</span><span class="sxs-lookup"><span data-stu-id="dff50-145">**Schedule.**</span></span>  <span data-ttu-id="dff50-146">Definierar hur ofta hello loggen sökningen ska köras.</span><span class="sxs-lookup"><span data-stu-id="dff50-146">Defines how often hello log search will be run.</span></span>  <span data-ttu-id="dff50-147">Varje varningsregeln ska ha ett och endast ett schema.</span><span class="sxs-lookup"><span data-stu-id="dff50-147">Each alert rule will have one and only one schedule.</span></span>
- <span data-ttu-id="dff50-148">**Åtgärd för aviseringen.**</span><span class="sxs-lookup"><span data-stu-id="dff50-148">**Alert action.**</span></span>  <span data-ttu-id="dff50-149">Varje regel för varning har en åtgärd resurs med en typ av **avisering** som definierar hello information om hello aviseringen, till exempel hello kriterier för när en avisering ska skapas och hello aviseringens allvarlighetsgrad.</span><span class="sxs-lookup"><span data-stu-id="dff50-149">Each alert rule will have one action resource with a type of **Alert** that defines hello details of hello alert such as hello criteria for when an alert record will be created and hello alert's severity.</span></span>  <span data-ttu-id="dff50-150">hello åtgärd resursen kommer du även definiera ett e-post och runbook-svar.</span><span class="sxs-lookup"><span data-stu-id="dff50-150">hello action resource will optionally define a mail and runbook response.</span></span>
- <span data-ttu-id="dff50-151">**Webhook-åtgärd (valfritt).**</span><span class="sxs-lookup"><span data-stu-id="dff50-151">**Webhook action (optional).**</span></span>  <span data-ttu-id="dff50-152">Om hello varningsregeln ska anropa en webhook, så det krävs en ytterligare åtgärd resurs med en typ av **Webhook**.</span><span class="sxs-lookup"><span data-stu-id="dff50-152">If hello alert rule will call a webhook, then it requires an additional action resource with a type of **Webhook**.</span></span>    

<span data-ttu-id="dff50-153">Sparad sökning resurser som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="dff50-153">Saved search resources are described above.</span></span>  <span data-ttu-id="dff50-154">hello beskrivs andra resurser som nedan.</span><span class="sxs-lookup"><span data-stu-id="dff50-154">hello other resources are described below.</span></span>


### <a name="schedule-resource"></a><span data-ttu-id="dff50-155">Schema för resurs</span><span class="sxs-lookup"><span data-stu-id="dff50-155">Schedule resource</span></span>

<span data-ttu-id="dff50-156">En sparad sökning kan ha en eller flera scheman med ett schema som representerar en separat avisering regel.</span><span class="sxs-lookup"><span data-stu-id="dff50-156">A saved search can have one or more schedules with each schedule representing a separate alert rule.</span></span> <span data-ttu-id="dff50-157">hello definierar schema hur ofta hello sökning körs och hello tidsintervall under vilka hello data hämtas.</span><span class="sxs-lookup"><span data-stu-id="dff50-157">hello schedule defines how often hello search is run and hello time interval over which hello data is retrieved.</span></span>  <span data-ttu-id="dff50-158">Schemalägga resurser har en typ av `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` och ha hello följande struktur.</span><span class="sxs-lookup"><span data-stu-id="dff50-158">Schedule resources have a type of `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` and have hello following structure.</span></span> <span data-ttu-id="dff50-159">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn.</span><span class="sxs-lookup"><span data-stu-id="dff50-159">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 


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



<span data-ttu-id="dff50-160">hello egenskaper för schema resurser beskrivs i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="dff50-160">hello properties for schedule resources are described in hello following table.</span></span>

| <span data-ttu-id="dff50-161">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="dff50-161">Element name</span></span> | <span data-ttu-id="dff50-162">Krävs</span><span class="sxs-lookup"><span data-stu-id="dff50-162">Required</span></span> | <span data-ttu-id="dff50-163">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dff50-163">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="dff50-164">aktiverad</span><span class="sxs-lookup"><span data-stu-id="dff50-164">enabled</span></span>       | <span data-ttu-id="dff50-165">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-165">Yes</span></span> | <span data-ttu-id="dff50-166">Anger om hello avisering aktiveras när den skapas.</span><span class="sxs-lookup"><span data-stu-id="dff50-166">Specifies whether hello alert is enabled when it's created.</span></span> |
| <span data-ttu-id="dff50-167">interval</span><span class="sxs-lookup"><span data-stu-id="dff50-167">interval</span></span>      | <span data-ttu-id="dff50-168">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-168">Yes</span></span> | <span data-ttu-id="dff50-169">Hur ofta hello fråga körs i minuter.</span><span class="sxs-lookup"><span data-stu-id="dff50-169">How often hello query runs in minutes.</span></span> |
| <span data-ttu-id="dff50-170">QueryTimeSpan</span><span class="sxs-lookup"><span data-stu-id="dff50-170">queryTimeSpan</span></span> | <span data-ttu-id="dff50-171">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-171">Yes</span></span> | <span data-ttu-id="dff50-172">Lång tid i minuter under vilka tooevaluate resultat.</span><span class="sxs-lookup"><span data-stu-id="dff50-172">Length of time in minutes over which tooevaluate results.</span></span> |

<span data-ttu-id="dff50-173">hello schema resursen ska beror på hello sparad sökning så att den har skapats innan hello schema.</span><span class="sxs-lookup"><span data-stu-id="dff50-173">hello schedule resource should depend on hello saved search so that it's created before hello schedule.</span></span>


### <a name="actions"></a><span data-ttu-id="dff50-174">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="dff50-174">Actions</span></span>
<span data-ttu-id="dff50-175">Det finns två typer av åtgärden resursen som anges av hello **typen** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="dff50-175">There are two types of action resource specified by hello **Type** property.</span></span>  <span data-ttu-id="dff50-176">Ett schema kräver en **avisering** åtgärd som definierar hello information om hello varningsregeln och vilka åtgärder som vidtas när en avisering skapas.</span><span class="sxs-lookup"><span data-stu-id="dff50-176">A schedule requires one **Alert** action which defines hello details of hello alert rule and what actions are taken when an alert is created.</span></span>  <span data-ttu-id="dff50-177">Det kan också innehålla en **Webhook** åtgärd om en webhook ska anropas från hello avisering.</span><span class="sxs-lookup"><span data-stu-id="dff50-177">It may also include a **Webhook** action if a webhook should be called from hello alert.</span></span>  

<span data-ttu-id="dff50-178">Åtgärden resurser har en typ av `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.</span><span class="sxs-lookup"><span data-stu-id="dff50-178">Action resources have a type of `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.</span></span>  

#### <a name="alert-actions"></a><span data-ttu-id="dff50-179">Aviseringsåtgärder</span><span class="sxs-lookup"><span data-stu-id="dff50-179">Alert actions</span></span>

<span data-ttu-id="dff50-180">Varje schemat har en **avisering** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="dff50-180">Every schedule will have one **Alert** action.</span></span>  <span data-ttu-id="dff50-181">Detta definierar hello information om hello aviseringen och eventuellt meddelande- och reparationsloggarna åtgärder.</span><span class="sxs-lookup"><span data-stu-id="dff50-181">This defines hello details of hello alert and optionally notification and remediation actions.</span></span>  <span data-ttu-id="dff50-182">Ett meddelande skickas ett e-tooone eller flera adresser.</span><span class="sxs-lookup"><span data-stu-id="dff50-182">A notification sends an email tooone or more addresses.</span></span>  <span data-ttu-id="dff50-183">En startar en runbook i Azure Automation tooattempt tooremediate hello identifierat problemet.</span><span class="sxs-lookup"><span data-stu-id="dff50-183">A remediation starts a runbook in Azure Automation tooattempt tooremediate hello detected issue.</span></span>

<span data-ttu-id="dff50-184">Aviseringsåtgärder har hello följande struktur.</span><span class="sxs-lookup"><span data-stu-id="dff50-184">Alert actions have hello following structure.</span></span>  <span data-ttu-id="dff50-185">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn.</span><span class="sxs-lookup"><span data-stu-id="dff50-185">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 



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

<span data-ttu-id="dff50-186">hello egenskaper för Aviseringsåtgärd resurser beskrivs i följande tabeller hello.</span><span class="sxs-lookup"><span data-stu-id="dff50-186">hello properties for Alert action resources are described in hello following tables.</span></span>

| <span data-ttu-id="dff50-187">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="dff50-187">Element name</span></span> | <span data-ttu-id="dff50-188">Krävs</span><span class="sxs-lookup"><span data-stu-id="dff50-188">Required</span></span> | <span data-ttu-id="dff50-189">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dff50-189">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="dff50-190">Typ</span><span class="sxs-lookup"><span data-stu-id="dff50-190">Type</span></span> | <span data-ttu-id="dff50-191">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-191">Yes</span></span> | <span data-ttu-id="dff50-192">Typ av hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="dff50-192">Type of hello action.</span></span>  <span data-ttu-id="dff50-193">Detta blir **avisering** för aviseringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="dff50-193">This will be **Alert** for alert actions.</span></span> |
| <span data-ttu-id="dff50-194">Namn</span><span class="sxs-lookup"><span data-stu-id="dff50-194">Name</span></span> | <span data-ttu-id="dff50-195">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-195">Yes</span></span> | <span data-ttu-id="dff50-196">Visningsnamn för hello aviseringen.</span><span class="sxs-lookup"><span data-stu-id="dff50-196">Display name for hello alert.</span></span>  <span data-ttu-id="dff50-197">Detta är hello-namnet som visas i hello varningsregeln hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="dff50-197">This is hello name that's displayed in hello console for hello alert rule.</span></span> |
| <span data-ttu-id="dff50-198">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dff50-198">Description</span></span> | <span data-ttu-id="dff50-199">Nej</span><span class="sxs-lookup"><span data-stu-id="dff50-199">No</span></span> | <span data-ttu-id="dff50-200">Valfri beskrivning av hello avisering.</span><span class="sxs-lookup"><span data-stu-id="dff50-200">Optional description of hello alert.</span></span> |
| <span data-ttu-id="dff50-201">Allvarsgrad</span><span class="sxs-lookup"><span data-stu-id="dff50-201">Severity</span></span> | <span data-ttu-id="dff50-202">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-202">Yes</span></span> | <span data-ttu-id="dff50-203">Allvarlighetsgrad för aviseringen hello-posten från hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="dff50-203">Severity of hello alert record from hello following values:</span></span><br><br> <span data-ttu-id="dff50-204">**Kritiska**</span><span class="sxs-lookup"><span data-stu-id="dff50-204">**Critical**</span></span><br><span data-ttu-id="dff50-205">**Varning**</span><span class="sxs-lookup"><span data-stu-id="dff50-205">**Warning**</span></span><br><span data-ttu-id="dff50-206">**Information**</span><span class="sxs-lookup"><span data-stu-id="dff50-206">**Informational**</span></span> |


##### <a name="threshold"></a><span data-ttu-id="dff50-207">Tröskelvärde</span><span class="sxs-lookup"><span data-stu-id="dff50-207">Threshold</span></span>
<span data-ttu-id="dff50-208">Det här avsnittet krävs.</span><span class="sxs-lookup"><span data-stu-id="dff50-208">This section is required.</span></span>  <span data-ttu-id="dff50-209">Den definierar hello egenskaper för hello tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="dff50-209">It defines hello properties for hello alert threshold.</span></span>

| <span data-ttu-id="dff50-210">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="dff50-210">Element name</span></span> | <span data-ttu-id="dff50-211">Krävs</span><span class="sxs-lookup"><span data-stu-id="dff50-211">Required</span></span> | <span data-ttu-id="dff50-212">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dff50-212">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="dff50-213">Operatorn</span><span class="sxs-lookup"><span data-stu-id="dff50-213">Operator</span></span> | <span data-ttu-id="dff50-214">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-214">Yes</span></span> | <span data-ttu-id="dff50-215">Operatorn hello jämförelse från hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="dff50-215">Operator for hello comparison from hello following values:</span></span><br><br><span data-ttu-id="dff50-216">**gt = större än<br>lt = mindre än**</span><span class="sxs-lookup"><span data-stu-id="dff50-216">**gt = greater than<br>lt = less than**</span></span> |
| <span data-ttu-id="dff50-217">Värde</span><span class="sxs-lookup"><span data-stu-id="dff50-217">Value</span></span> | <span data-ttu-id="dff50-218">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-218">Yes</span></span> | <span data-ttu-id="dff50-219">hello värdet toocompare hello resultat.</span><span class="sxs-lookup"><span data-stu-id="dff50-219">hello value toocompare hello results.</span></span> |


##### <a name="metricstrigger"></a><span data-ttu-id="dff50-220">MetricsTrigger</span><span class="sxs-lookup"><span data-stu-id="dff50-220">MetricsTrigger</span></span>
<span data-ttu-id="dff50-221">Det här avsnittet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="dff50-221">This section is optional.</span></span>  <span data-ttu-id="dff50-222">Inkludera det för ett mått mätning aviseringen.</span><span class="sxs-lookup"><span data-stu-id="dff50-222">Include it for a metric measurement alert.</span></span>

> [!NOTE]
> <span data-ttu-id="dff50-223">Mått mätning aviseringar är för närvarande i förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="dff50-223">Metric measurement alerts are currently in public preview.</span></span> 

| <span data-ttu-id="dff50-224">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="dff50-224">Element name</span></span> | <span data-ttu-id="dff50-225">Krävs</span><span class="sxs-lookup"><span data-stu-id="dff50-225">Required</span></span> | <span data-ttu-id="dff50-226">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dff50-226">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="dff50-227">TriggerCondition</span><span class="sxs-lookup"><span data-stu-id="dff50-227">TriggerCondition</span></span> | <span data-ttu-id="dff50-228">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-228">Yes</span></span> | <span data-ttu-id="dff50-229">Anger om hello tröskelvärde för totala antalet överträdelser eller på varandra följande överträdelser från hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="dff50-229">Specifies whether hello threshold is for total number of breaches or consecutive breaches from hello following values:</span></span><br><br><span data-ttu-id="dff50-230">**Totalt antal<br>i följd**</span><span class="sxs-lookup"><span data-stu-id="dff50-230">**Total<br>Consecutive**</span></span> |
| <span data-ttu-id="dff50-231">Operatorn</span><span class="sxs-lookup"><span data-stu-id="dff50-231">Operator</span></span> | <span data-ttu-id="dff50-232">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-232">Yes</span></span> | <span data-ttu-id="dff50-233">Operatorn hello jämförelse från hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="dff50-233">Operator for hello comparison from hello following values:</span></span><br><br><span data-ttu-id="dff50-234">**gt = större än<br>lt = mindre än**</span><span class="sxs-lookup"><span data-stu-id="dff50-234">**gt = greater than<br>lt = less than**</span></span> |
| <span data-ttu-id="dff50-235">Värde</span><span class="sxs-lookup"><span data-stu-id="dff50-235">Value</span></span> | <span data-ttu-id="dff50-236">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-236">Yes</span></span> | <span data-ttu-id="dff50-237">Antalet hello tider hello villkor måste vara uppfyllda tootrigger hello avisering.</span><span class="sxs-lookup"><span data-stu-id="dff50-237">Number of hello times hello criteria must be met tootrigger hello alert.</span></span> |

##### <a name="throttling"></a><span data-ttu-id="dff50-238">Begränsning</span><span class="sxs-lookup"><span data-stu-id="dff50-238">Throttling</span></span>
<span data-ttu-id="dff50-239">Det här avsnittet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="dff50-239">This section is optional.</span></span>  <span data-ttu-id="dff50-240">Inkludera det här avsnittet om du vill toosuppress aviseringar från hello samma regel för vissa tidsperiod när en avisering har skapats.</span><span class="sxs-lookup"><span data-stu-id="dff50-240">Include this section if you want toosuppress alerts from hello same rule for some amount of time after an alert is created.</span></span>

| <span data-ttu-id="dff50-241">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="dff50-241">Element name</span></span> | <span data-ttu-id="dff50-242">Krävs</span><span class="sxs-lookup"><span data-stu-id="dff50-242">Required</span></span> | <span data-ttu-id="dff50-243">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dff50-243">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="dff50-244">DurationInMinutes</span><span class="sxs-lookup"><span data-stu-id="dff50-244">DurationInMinutes</span></span> | <span data-ttu-id="dff50-245">Ja om begränsning element som ingår</span><span class="sxs-lookup"><span data-stu-id="dff50-245">Yes if Throttling element included</span></span> | <span data-ttu-id="dff50-246">Antal minuter toosuppress aviseringar efter en från hello samma varningsregeln har skapats.</span><span class="sxs-lookup"><span data-stu-id="dff50-246">Number of minutes toosuppress alerts after one from hello same alert rule is created.</span></span> |

##### <a name="emailnotification"></a><span data-ttu-id="dff50-247">EmailNotification</span><span class="sxs-lookup"><span data-stu-id="dff50-247">EmailNotification</span></span>
 <span data-ttu-id="dff50-248">Det här avsnittet är valfria inkludera den om du vill att hello avisering toosend e tooone eller flera mottagare.</span><span class="sxs-lookup"><span data-stu-id="dff50-248">This section is optional  Include it if you want hello alert toosend mail tooone or more recipients.</span></span>

| <span data-ttu-id="dff50-249">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="dff50-249">Element name</span></span> | <span data-ttu-id="dff50-250">Krävs</span><span class="sxs-lookup"><span data-stu-id="dff50-250">Required</span></span> | <span data-ttu-id="dff50-251">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dff50-251">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="dff50-252">mottagare</span><span class="sxs-lookup"><span data-stu-id="dff50-252">Recipients</span></span> | <span data-ttu-id="dff50-253">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-253">Yes</span></span> | <span data-ttu-id="dff50-254">Kommaavgränsad lista med e-adresser toosend meddelande när en avisering skapas som i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="dff50-254">Comma delimited list of email addresses toosend notification when an alert is created such as in hello following example.</span></span><br><br><span data-ttu-id="dff50-255">**[ "recipient1@contoso.com", "recipient2@contoso.com" ]**</span><span class="sxs-lookup"><span data-stu-id="dff50-255">**[ "recipient1@contoso.com", "recipient2@contoso.com" ]**</span></span> |
| <span data-ttu-id="dff50-256">Ämne</span><span class="sxs-lookup"><span data-stu-id="dff50-256">Subject</span></span> | <span data-ttu-id="dff50-257">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-257">Yes</span></span> | <span data-ttu-id="dff50-258">Ämnesrad hello e-post.</span><span class="sxs-lookup"><span data-stu-id="dff50-258">Subject line of hello mail.</span></span> |
| <span data-ttu-id="dff50-259">Bifogad fil</span><span class="sxs-lookup"><span data-stu-id="dff50-259">Attachment</span></span> | <span data-ttu-id="dff50-260">Nej</span><span class="sxs-lookup"><span data-stu-id="dff50-260">No</span></span> | <span data-ttu-id="dff50-261">Bifogade filer stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="dff50-261">Attachments are not currently supported.</span></span>  <span data-ttu-id="dff50-262">Om det här elementet finns det ska vara **ingen**.</span><span class="sxs-lookup"><span data-stu-id="dff50-262">If this element is included, it should be **None**.</span></span> |


##### <a name="remediation"></a><span data-ttu-id="dff50-263">Reparation</span><span class="sxs-lookup"><span data-stu-id="dff50-263">Remediation</span></span>
<span data-ttu-id="dff50-264">Det här avsnittet är valfritt att inkludera det om du vill att en runbook toostart i svaret toohello avisering.</span><span class="sxs-lookup"><span data-stu-id="dff50-264">This section is optional  Include it if you want a runbook toostart in response toohello alert.</span></span> |

| <span data-ttu-id="dff50-265">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="dff50-265">Element name</span></span> | <span data-ttu-id="dff50-266">Krävs</span><span class="sxs-lookup"><span data-stu-id="dff50-266">Required</span></span> | <span data-ttu-id="dff50-267">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dff50-267">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="dff50-268">RunbookName</span><span class="sxs-lookup"><span data-stu-id="dff50-268">RunbookName</span></span> | <span data-ttu-id="dff50-269">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-269">Yes</span></span> | <span data-ttu-id="dff50-270">Namnet på hello runbook toostart.</span><span class="sxs-lookup"><span data-stu-id="dff50-270">Name of hello runbook toostart.</span></span> |
| <span data-ttu-id="dff50-271">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="dff50-271">WebhookUri</span></span> | <span data-ttu-id="dff50-272">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-272">Yes</span></span> | <span data-ttu-id="dff50-273">URI för hello webhook för hello runbook.</span><span class="sxs-lookup"><span data-stu-id="dff50-273">Uri of hello webhook for hello runbook.</span></span> |
| <span data-ttu-id="dff50-274">Förfallodatum</span><span class="sxs-lookup"><span data-stu-id="dff50-274">Expiry</span></span> | <span data-ttu-id="dff50-275">Nej</span><span class="sxs-lookup"><span data-stu-id="dff50-275">No</span></span> | <span data-ttu-id="dff50-276">Datum och tid då hello reparation upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="dff50-276">Date and time that hello remediation expires.</span></span> |

#### <a name="webhook-actions"></a><span data-ttu-id="dff50-277">Webhook-åtgärder</span><span class="sxs-lookup"><span data-stu-id="dff50-277">Webhook actions</span></span>

<span data-ttu-id="dff50-278">Webhook-åtgärder kan du starta en process genom att anropa en URL och du kan också tillhandahålla en nyttolast toobe skickas.</span><span class="sxs-lookup"><span data-stu-id="dff50-278">Webhook actions start a process by calling a URL and optionally providing a payload toobe sent.</span></span> <span data-ttu-id="dff50-279">De är liknande tooRemediation åtgärder förutom de är avsedda för webhooks som kan anropa processer än Azure Automation-runbooks.</span><span class="sxs-lookup"><span data-stu-id="dff50-279">They are similar tooRemediation actions except they are meant for webhooks that may invoke processes other than Azure Automation runbooks.</span></span> <span data-ttu-id="dff50-280">De ger också hello ytterligare alternativ för att tillhandahålla en nyttolast toobe levereras toohello fjärrprocess.</span><span class="sxs-lookup"><span data-stu-id="dff50-280">They also provide hello additional option of providing a payload toobe delivered toohello remote process.</span></span>

<span data-ttu-id="dff50-281">Om aviseringen ska anropa en webhook, så den behöver en åtgärd resurs med en typ av **Webhook** i tillägg toohello **avisering** åtgärd resurs.</span><span class="sxs-lookup"><span data-stu-id="dff50-281">If your alert will call a webhook, then it will need an action resource with a type of **Webhook** in addition toohello **Alert** action resource.</span></span>  

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

<span data-ttu-id="dff50-282">hello egenskaper för Webhook åtgärd resurser beskrivs i följande tabeller hello.</span><span class="sxs-lookup"><span data-stu-id="dff50-282">hello properties for Webhook action resources are described in hello following tables.</span></span>

| <span data-ttu-id="dff50-283">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="dff50-283">Element name</span></span> | <span data-ttu-id="dff50-284">Krävs</span><span class="sxs-lookup"><span data-stu-id="dff50-284">Required</span></span> | <span data-ttu-id="dff50-285">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dff50-285">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="dff50-286">typ</span><span class="sxs-lookup"><span data-stu-id="dff50-286">type</span></span> | <span data-ttu-id="dff50-287">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-287">Yes</span></span> | <span data-ttu-id="dff50-288">Typ av hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="dff50-288">Type of hello action.</span></span>  <span data-ttu-id="dff50-289">Detta blir **Webhook** för webhook-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="dff50-289">This will be **Webhook** for webhook actions.</span></span> |
| <span data-ttu-id="dff50-290">namn</span><span class="sxs-lookup"><span data-stu-id="dff50-290">name</span></span> | <span data-ttu-id="dff50-291">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-291">Yes</span></span> | <span data-ttu-id="dff50-292">Visningsnamn för hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="dff50-292">Display name for hello action.</span></span>  <span data-ttu-id="dff50-293">Detta visas inte i hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="dff50-293">This is not displayed in hello console.</span></span> |
| <span data-ttu-id="dff50-294">wehookUri</span><span class="sxs-lookup"><span data-stu-id="dff50-294">wehookUri</span></span> | <span data-ttu-id="dff50-295">Ja</span><span class="sxs-lookup"><span data-stu-id="dff50-295">Yes</span></span> | <span data-ttu-id="dff50-296">URI för hello webhooken.</span><span class="sxs-lookup"><span data-stu-id="dff50-296">Uri for hello webhook.</span></span> |
| <span data-ttu-id="dff50-297">CustomPayload</span><span class="sxs-lookup"><span data-stu-id="dff50-297">customPayload</span></span> | <span data-ttu-id="dff50-298">Nej</span><span class="sxs-lookup"><span data-stu-id="dff50-298">No</span></span> | <span data-ttu-id="dff50-299">Anpassad nyttolast toobe skickas toohello webhooken.</span><span class="sxs-lookup"><span data-stu-id="dff50-299">Custom payload toobe sent toohello webhook.</span></span> <span data-ttu-id="dff50-300">hello format beror på vilken hello webhook förväntas.</span><span class="sxs-lookup"><span data-stu-id="dff50-300">hello format will depend on what hello webhook is expecting.</span></span> |




## <a name="sample"></a><span data-ttu-id="dff50-301">Exempel</span><span class="sxs-lookup"><span data-stu-id="dff50-301">Sample</span></span>

<span data-ttu-id="dff50-302">Följande är ett exempel på en lösning som omfattar som innehåller hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="dff50-302">Following is a sample of a solution that include that includes hello following resources:</span></span>

- <span data-ttu-id="dff50-303">Sparad sökning</span><span class="sxs-lookup"><span data-stu-id="dff50-303">Saved search</span></span>
- <span data-ttu-id="dff50-304">Schema</span><span class="sxs-lookup"><span data-stu-id="dff50-304">Schedule</span></span>
- <span data-ttu-id="dff50-305">Aviseringsåtgärden</span><span class="sxs-lookup"><span data-stu-id="dff50-305">Alert action</span></span>
- <span data-ttu-id="dff50-306">Webhook-åtgärd</span><span class="sxs-lookup"><span data-stu-id="dff50-306">Webhook action</span></span>

<span data-ttu-id="dff50-307">Hej exempel använder [standardlösningen parametrar](operations-management-suite-solutions-solution-file.md#parameters) variabler som ofta används i en lösning som motsats toohardcoding värdena i resursdefinitionerna hello.</span><span class="sxs-lookup"><span data-stu-id="dff50-307">hello sample uses [standard solution parameters](operations-management-suite-solutions-solution-file.md#parameters) variables that would commonly be used in a solution as opposed toohardcoding values in hello resource definitions.</span></span>

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
              "Description": "List of recipients for hello email alert separated by semicolon"
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


<span data-ttu-id="dff50-308">hello följande parameterfilen ger exempel värden för den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="dff50-308">hello following parameter file provides samples values for this solution.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="dff50-309">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dff50-309">Next steps</span></span>
* <span data-ttu-id="dff50-310">[Lägga till vyer](operations-management-suite-solutions-resources-views.md) tooyour hanteringslösning.</span><span class="sxs-lookup"><span data-stu-id="dff50-310">[Add views](operations-management-suite-solutions-resources-views.md) tooyour management solution.</span></span>
* <span data-ttu-id="dff50-311">[Lägg till Automation-runbooks och andra resurser](operations-management-suite-solutions-resources-automation.md) tooyour hanteringslösning.</span><span class="sxs-lookup"><span data-stu-id="dff50-311">[Add Automation runbooks and other resources](operations-management-suite-solutions-resources-automation.md) tooyour management solution.</span></span>

