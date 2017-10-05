---
title: "Azure PowerShell övervakaren Snabbstart-exempel. | Microsoft Docs"
description: "Använda PowerShell för att få åtkomst till Azure-Monitor-funktioner, till exempel Autoskala, aviseringar, webhooks och söka aktivitetsloggar."
author: kamathashwin
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c0761814-7148-4ab5-8c27-a2c9fa4cfef5
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: ashwink
ms.openlocfilehash: 48f064884c2a6d0a55cc58a44169ed03c62de46d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-monitor-powershell-quick-start-samples"></a><span data-ttu-id="62924-104">Azure-Monitor PowerShell Snabbstart-exempel</span><span class="sxs-lookup"><span data-stu-id="62924-104">Azure Monitor PowerShell quick start samples</span></span>
<span data-ttu-id="62924-105">Den här artikeln innehåller exempel av PowerShell-kommandon som hjälper dig att komma åt Azure-Monitor funktioner.</span><span class="sxs-lookup"><span data-stu-id="62924-105">This article shows you sample PowerShell commands to help you access Azure Monitor features.</span></span> <span data-ttu-id="62924-106">Azure-Monitor kan du Autoskala molntjänster, virtuella datorer och Web Apps att skicka aviseringar eller anropa webbadresser som baseras på värden för konfigurerade telemetridata.</span><span class="sxs-lookup"><span data-stu-id="62924-106">Azure Monitor allows you to AutoScale Cloud Services, Virtual Machines, and Web Apps and to send alert notifications or call web URLs based on values of configured telemetry data.</span></span>

> [!NOTE]
> <span data-ttu-id="62924-107">Azure övervakaren är det nya namnet för vad anropades ”Azure Insights” förrän den 25 september 2016.</span><span class="sxs-lookup"><span data-stu-id="62924-107">Azure Monitor is the new name for what was called "Azure Insights" until Sept 25th, 2016.</span></span> <span data-ttu-id="62924-108">Namnområden och därmed följande kommandon fortfarande innehåller dock ”insikter”.</span><span class="sxs-lookup"><span data-stu-id="62924-108">However, the namespaces and thus the following commands still contain the "insights".</span></span>
> 
> 

## <a name="set-up-powershell"></a><span data-ttu-id="62924-109">Konfigurera PowerShell</span><span class="sxs-lookup"><span data-stu-id="62924-109">Set up PowerShell</span></span>
<span data-ttu-id="62924-110">Om du inte redan gjort ange PowerShell ska köras på datorn.</span><span class="sxs-lookup"><span data-stu-id="62924-110">If you haven't already, set up PowerShell to run on your computer.</span></span> <span data-ttu-id="62924-111">Mer information finns i [installera och konfigurera PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="62924-111">For more information, see [How to Install and Configure PowerShell](/powershell/azure/overview).</span></span>

## <a name="examples-in-this-article"></a><span data-ttu-id="62924-112">Exemplen i den här artikeln</span><span class="sxs-lookup"><span data-stu-id="62924-112">Examples in this article</span></span>
<span data-ttu-id="62924-113">Exemplen i artikeln visar hur du kan använda Azure-Monitor-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="62924-113">The examples in the article illustrate how you can use Azure Monitor cmdlets.</span></span> <span data-ttu-id="62924-114">Du kan också granska hela listan med Azure-Monitor PowerShell-cmdlets på [Azure-Monitor (insikter) Cmdlets](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).</span><span class="sxs-lookup"><span data-stu-id="62924-114">You can also review the entire list of Azure Monitor PowerShell cmdlets at [Azure Monitor (Insights) Cmdlets](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).</span></span>

## <a name="sign-in-and-use-subscriptions"></a><span data-ttu-id="62924-115">Logga in och använda prenumerationer</span><span class="sxs-lookup"><span data-stu-id="62924-115">Sign in and use subscriptions</span></span>
<span data-ttu-id="62924-116">Först logga in på Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="62924-116">First, log in to your Azure subscription.</span></span>

```PowerShell
Login-AzureRmAccount
```

<span data-ttu-id="62924-117">Detta kräver att du loggar in.</span><span class="sxs-lookup"><span data-stu-id="62924-117">This requires you to sign in.</span></span> <span data-ttu-id="62924-118">När du gör ditt konto, visas TenantID och standard prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="62924-118">Once you do, your Account, TenantID and default Subscription ID are displayed.</span></span> <span data-ttu-id="62924-119">Alla Azure cmdlets fungerar i samband med standard-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="62924-119">All the Azure cmdlets work in the context of your default subscription.</span></span> <span data-ttu-id="62924-120">Använd följande kommando om du vill visa listan över prenumerationer som du har åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="62924-120">To view the list of subscriptions you have access to, use the following command.</span></span>

```PowerShell
Get-AzureRmSubscription
```

<span data-ttu-id="62924-121">Använd följande kommando om du vill ändra fungerande kontexten till en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="62924-121">To change your working context to a different subscription, use the following command.</span></span>

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-activity-log-for-a-subscription"></a><span data-ttu-id="62924-122">Hämta aktivitetsloggen för en prenumeration</span><span class="sxs-lookup"><span data-stu-id="62924-122">Retrieve Activity log for a subscription</span></span>
<span data-ttu-id="62924-123">Använd den `Get-AzureRmLog` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="62924-123">Use the `Get-AzureRmLog` cmdlet.</span></span>  <span data-ttu-id="62924-124">Här följer några vanliga exempel.</span><span class="sxs-lookup"><span data-stu-id="62924-124">The following are some common examples.</span></span>

<span data-ttu-id="62924-125">Hämta loggposter från denna tid/datum presentera:</span><span class="sxs-lookup"><span data-stu-id="62924-125">Get log entries from this time/date to present:</span></span>

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

<span data-ttu-id="62924-126">Hämta loggposter ett värdeområde tid/datum:</span><span class="sxs-lookup"><span data-stu-id="62924-126">Get log entries between a time/date range:</span></span>

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

<span data-ttu-id="62924-127">Hämta loggposter från en viss resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="62924-127">Get log entries from a specific resource group:</span></span>

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

<span data-ttu-id="62924-128">Hämta loggposter från en viss resurs-leverantör ett värdeområde tid/datum:</span><span class="sxs-lookup"><span data-stu-id="62924-128">Get log entries from a specific resource provider between a time/date range:</span></span>

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

<span data-ttu-id="62924-129">Hämta alla loggposter med en specifik anropare:</span><span class="sxs-lookup"><span data-stu-id="62924-129">Get all log entries with a specific caller:</span></span>

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

<span data-ttu-id="62924-130">Följande kommando hämtar de senaste 1 000 händelserna från aktivitetsloggen:</span><span class="sxs-lookup"><span data-stu-id="62924-130">The following command retrieves the last 1000 events from the activity log:</span></span>

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

<span data-ttu-id="62924-131">`Get-AzureRmLog`har stöd för många parametrar.</span><span class="sxs-lookup"><span data-stu-id="62924-131">`Get-AzureRmLog` supports many other parameters.</span></span> <span data-ttu-id="62924-132">Finns det `Get-AzureRmLog` referens för mer information.</span><span class="sxs-lookup"><span data-stu-id="62924-132">See the `Get-AzureRmLog` reference for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="62924-133">`Get-AzureRmLog`endast ger 15 dagar tidigare.</span><span class="sxs-lookup"><span data-stu-id="62924-133">`Get-AzureRmLog` only provides 15 days of history.</span></span> <span data-ttu-id="62924-134">Med hjälp av den **- MaxEvents** parameter kan du fråga de sista N händelserna efter 15 dagar.</span><span class="sxs-lookup"><span data-stu-id="62924-134">Using the **-MaxEvents** parameter allows you to query the last N events, beyond 15 days.</span></span> <span data-ttu-id="62924-135">För åtkomst till händelser som är äldre än 15 dagar, använda REST API eller SDK (C#-exempel med hjälp av SDK).</span><span class="sxs-lookup"><span data-stu-id="62924-135">To access events older than 15 days, use the REST API or SDK (C# sample using the SDK).</span></span> <span data-ttu-id="62924-136">Om du inte inkluderar **StartTime**, då är standardvärdet **EndTime** minus en timme.</span><span class="sxs-lookup"><span data-stu-id="62924-136">If you do not include **StartTime**, then the default value is **EndTime** minus one hour.</span></span> <span data-ttu-id="62924-137">Om du inte inkluderar **EndTime**, och standardvärdet är aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="62924-137">If you do not include **EndTime**, then the default value is current time.</span></span> <span data-ttu-id="62924-138">Det finns alltid i UTC.</span><span class="sxs-lookup"><span data-stu-id="62924-138">All times are in UTC.</span></span>
> 
> 

## <a name="retrieve-alerts-history"></a><span data-ttu-id="62924-139">Hämta aviseringar historik</span><span class="sxs-lookup"><span data-stu-id="62924-139">Retrieve alerts history</span></span>
<span data-ttu-id="62924-140">Om du vill visa alla aviseringar händelser, kan du fråga Azure Resource Manager-loggar med hjälp av följande exempel.</span><span class="sxs-lookup"><span data-stu-id="62924-140">To view all alert events, you can query the Azure Resource Manager logs using the following examples.</span></span>

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

<span data-ttu-id="62924-141">Om du vill visa historiken för en specifik avisering regel som du kan använda den `Get-AzureRmAlertHistory` cmdlet, skicka i resurs-ID för regeln.</span><span class="sxs-lookup"><span data-stu-id="62924-141">To view the history for a specific alert rule, you can use the `Get-AzureRmAlertHistory` cmdlet, passing in the resource ID of the alert rule.</span></span>

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

<span data-ttu-id="62924-142">Den `Get-AzureRmAlertHistory` cmdlet har stöd för olika parametrar.</span><span class="sxs-lookup"><span data-stu-id="62924-142">The `Get-AzureRmAlertHistory` cmdlet supports various parameters.</span></span> <span data-ttu-id="62924-143">Mer information finns i [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).</span><span class="sxs-lookup"><span data-stu-id="62924-143">More information, see [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).</span></span>

## <a name="retrieve-information-on-alert-rules"></a><span data-ttu-id="62924-144">Hämta information om Varningsregler</span><span class="sxs-lookup"><span data-stu-id="62924-144">Retrieve information on alert rules</span></span>
<span data-ttu-id="62924-145">Alla följande kommandon fungerar på en resursgrupp med namnet ”montest”.</span><span class="sxs-lookup"><span data-stu-id="62924-145">All of the following commands act on a Resource Group named "montest".</span></span>

<span data-ttu-id="62924-146">Visa alla egenskaper för regeln:</span><span class="sxs-lookup"><span data-stu-id="62924-146">View all the properties of the alert rule:</span></span>

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

<span data-ttu-id="62924-147">Hämta alla aviseringar på en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="62924-147">Retrieve all alerts on a resource group:</span></span>

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

<span data-ttu-id="62924-148">Hämta alla Varningsregler för en målresurs.</span><span class="sxs-lookup"><span data-stu-id="62924-148">Retrieve all alert rules set for a target resource.</span></span> <span data-ttu-id="62924-149">Till exempel ange alla Varningsregler på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="62924-149">For example, all alert rules set on a VM.</span></span>

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

<span data-ttu-id="62924-150">`Get-AzureRmAlertRule`har stöd för andra parametrar.</span><span class="sxs-lookup"><span data-stu-id="62924-150">`Get-AzureRmAlertRule` supports other parameters.</span></span> <span data-ttu-id="62924-151">Se [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="62924-151">See [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) for more information.</span></span>

## <a name="create-metric-alerts"></a><span data-ttu-id="62924-152">Skapa mått aviseringar</span><span class="sxs-lookup"><span data-stu-id="62924-152">Create metric alerts</span></span>
<span data-ttu-id="62924-153">Du kan använda den `Add-AlertRule` för att skapa, uppdatera eller inaktivera en aviseringsregel.</span><span class="sxs-lookup"><span data-stu-id="62924-153">You can use the `Add-AlertRule` cmdlet to create, update or disable an alert rule.</span></span>

<span data-ttu-id="62924-154">Du kan skapa e-post och webhook-egenskaper med `New-AzureRmAlertRuleEmail` och `New-AzureRmAlertRuleWebhook`respektive.</span><span class="sxs-lookup"><span data-stu-id="62924-154">You can create email and webhook properties using  `New-AzureRmAlertRuleEmail` and `New-AzureRmAlertRuleWebhook`, respectively.</span></span> <span data-ttu-id="62924-155">I cmdleten varningsregeln tilldela dem som åtgärder för att den **åtgärder** -egenskapen för regeln.</span><span class="sxs-lookup"><span data-stu-id="62924-155">In the Alert rule cmdlet, assign these as actions to the **Actions** property of the Alert Rule.</span></span>

<span data-ttu-id="62924-156">I följande tabell beskrivs de parametrar och värden som används för att skapa en avisering med ett mått.</span><span class="sxs-lookup"><span data-stu-id="62924-156">The following table describes the parameters and values used to create an alert using a metric.</span></span>

| <span data-ttu-id="62924-157">Parametern</span><span class="sxs-lookup"><span data-stu-id="62924-157">parameter</span></span> | <span data-ttu-id="62924-158">värde</span><span class="sxs-lookup"><span data-stu-id="62924-158">value</span></span> |
| --- | --- |
| <span data-ttu-id="62924-159">Namn</span><span class="sxs-lookup"><span data-stu-id="62924-159">Name</span></span> |<span data-ttu-id="62924-160">simpletestdiskwrite</span><span class="sxs-lookup"><span data-stu-id="62924-160">simpletestdiskwrite</span></span> |
| <span data-ttu-id="62924-161">Platsen för den här varningsregeln</span><span class="sxs-lookup"><span data-stu-id="62924-161">Location of this alert rule</span></span> |<span data-ttu-id="62924-162">Östra USA</span><span class="sxs-lookup"><span data-stu-id="62924-162">East US</span></span> |
| <span data-ttu-id="62924-163">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="62924-163">ResourceGroup</span></span> |<span data-ttu-id="62924-164">montest</span><span class="sxs-lookup"><span data-stu-id="62924-164">montest</span></span> |
| <span data-ttu-id="62924-165">TargetResourceId</span><span class="sxs-lookup"><span data-stu-id="62924-165">TargetResourceId</span></span> |<span data-ttu-id="62924-166">/subscriptions/S1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig</span><span class="sxs-lookup"><span data-stu-id="62924-166">/subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig</span></span> |
| <span data-ttu-id="62924-167">MetricName för aviseringen som har skapats</span><span class="sxs-lookup"><span data-stu-id="62924-167">MetricName of the alert that is created</span></span> |<span data-ttu-id="62924-168">\PhysicalDisk (_Total) \Disk Diskskrivningar/sek. Finns det `Get-MetricDefinitions` cmdlet om hur du hämtar de exakta mått namn</span><span class="sxs-lookup"><span data-stu-id="62924-168">\PhysicalDisk(_Total)\Disk Writes/sec. See the `Get-MetricDefinitions` cmdlet about how to retrieve the exact metric names</span></span> |
| <span data-ttu-id="62924-169">Operatorn</span><span class="sxs-lookup"><span data-stu-id="62924-169">operator</span></span> |<span data-ttu-id="62924-170">GreaterThan</span><span class="sxs-lookup"><span data-stu-id="62924-170">GreaterThan</span></span> |
| <span data-ttu-id="62924-171">Tröskelvärde (antal per sekund i för det här måttet)</span><span class="sxs-lookup"><span data-stu-id="62924-171">Threshold value (count/sec in for this metric)</span></span> |<span data-ttu-id="62924-172">1</span><span class="sxs-lookup"><span data-stu-id="62924-172">1</span></span> |
| <span data-ttu-id="62924-173">Fönsterstorlek (format: mm: ss)</span><span class="sxs-lookup"><span data-stu-id="62924-173">WindowSize (hh:mm:ss format)</span></span> |<span data-ttu-id="62924-174">00:05:00</span><span class="sxs-lookup"><span data-stu-id="62924-174">00:05:00</span></span> |
| <span data-ttu-id="62924-175">Aggregator (statistik på måttet, som använder Genomsnittligt antal i det här fallet)</span><span class="sxs-lookup"><span data-stu-id="62924-175">aggregator (statistic of the metric, which uses Average count, in this case)</span></span> |<span data-ttu-id="62924-176">Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="62924-176">Average</span></span> |
| <span data-ttu-id="62924-177">anpassad e-postmeddelanden (Strängmatrisen)</span><span class="sxs-lookup"><span data-stu-id="62924-177">custom emails (string array)</span></span> |<span data-ttu-id="62924-178">'foo@example.com','bar@example.com'</span><span class="sxs-lookup"><span data-stu-id="62924-178">'foo@example.com','bar@example.com'</span></span> |
| <span data-ttu-id="62924-179">Skicka e-post till ägare, deltagare och läsare</span><span class="sxs-lookup"><span data-stu-id="62924-179">send email to owners, contributors and readers</span></span> |<span data-ttu-id="62924-180">-SendToServiceOwners</span><span class="sxs-lookup"><span data-stu-id="62924-180">-SendToServiceOwners</span></span> |

<span data-ttu-id="62924-181">Skapa en e-åtgärd</span><span class="sxs-lookup"><span data-stu-id="62924-181">Create an Email action</span></span>

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

<span data-ttu-id="62924-182">Skapa en Webhook-åtgärd</span><span class="sxs-lookup"><span data-stu-id="62924-182">Create a Webhook action</span></span>

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

<span data-ttu-id="62924-183">Skapa varningsregeln på processor % mått på en klassisk virtuell dator</span><span class="sxs-lookup"><span data-stu-id="62924-183">Create the alert rule on the CPU% metric on a classic VM</span></span>

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

<span data-ttu-id="62924-184">Hämta varningsregeln</span><span class="sxs-lookup"><span data-stu-id="62924-184">Retrieve the alert rule</span></span>

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

<span data-ttu-id="62924-185">Lägg till avisering cmdlet uppdateras också regeln om det finns redan en regel för varning för de angivna egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="62924-185">The Add alert cmdlet also updates the rule if an alert rule already exists for the given properties.</span></span> <span data-ttu-id="62924-186">Om du vill inaktivera en aviseringsregel, inkluderar du parametern **- DisableRule**.</span><span class="sxs-lookup"><span data-stu-id="62924-186">To disable an alert rule, include the parameter **-DisableRule**.</span></span>

## <a name="get-a-list-of-available-metrics-for-alerts"></a><span data-ttu-id="62924-187">Hämta en lista över tillgängliga mått för aviseringar</span><span class="sxs-lookup"><span data-stu-id="62924-187">Get a list of available metrics for alerts</span></span>
<span data-ttu-id="62924-188">Du kan använda den `Get-AzureRmMetricDefinition` för att visa en lista över alla mätvärden för en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="62924-188">You can use the `Get-AzureRmMetricDefinition` cmdlet to view the list of all metrics for a specific resource.</span></span>

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

<span data-ttu-id="62924-189">I följande exempel skapar en tabell med måttet namn och enhet för den.</span><span class="sxs-lookup"><span data-stu-id="62924-189">The following example generates a table with the metric Name and the Unit for it.</span></span>

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

<span data-ttu-id="62924-190">En fullständig lista över tillgängliga alternativ för `Get-AzureRmMetricDefinition` finns på [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).</span><span class="sxs-lookup"><span data-stu-id="62924-190">A full list of available options for `Get-AzureRmMetricDefinition` is available at [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).</span></span>

## <a name="create-and-manage-autoscale-settings"></a><span data-ttu-id="62924-191">Skapa och hantera Autoskala inställningar</span><span class="sxs-lookup"><span data-stu-id="62924-191">Create and manage AutoScale settings</span></span>
<span data-ttu-id="62924-192">En resurs, till exempel en webbapp VM, molntjänst eller Skaluppsättning för virtuell dator kan ha endast en autoskalningsinställning som konfigurerats för den.</span><span class="sxs-lookup"><span data-stu-id="62924-192">A resource, such as a Web app, VM, Cloud Service or Virtual Machine Scale Set can have only one autoscale setting configured for it.</span></span>
<span data-ttu-id="62924-193">Varje autoskalningsinställning kan dock ha flera profiler.</span><span class="sxs-lookup"><span data-stu-id="62924-193">However, each autoscale setting can have multiple profiles.</span></span> <span data-ttu-id="62924-194">Till exempel en för en prestandabaserad skala profil och en andra princip för en schemabaserade profil.</span><span class="sxs-lookup"><span data-stu-id="62924-194">For example, one for a performance-based scale profile and a second one for a schedule-based profile.</span></span> <span data-ttu-id="62924-195">Varje profil kan ha flera regler som konfigurerats på den.</span><span class="sxs-lookup"><span data-stu-id="62924-195">Each profile can have multiple rules configured on it.</span></span> <span data-ttu-id="62924-196">Läs mer om Autoskala [hur Autoskala ett program](../cloud-services/cloud-services-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="62924-196">For more information about Autoscale, see [How to Autoscale an Application](../cloud-services/cloud-services-how-to-scale.md).</span></span>

<span data-ttu-id="62924-197">Här följer de steg som vi använder:</span><span class="sxs-lookup"><span data-stu-id="62924-197">Here are the steps we will use:</span></span>

1. <span data-ttu-id="62924-198">Skapa regler.</span><span class="sxs-lookup"><span data-stu-id="62924-198">Create rule(s).</span></span>
2. <span data-ttu-id="62924-199">Skapa eller profilerna mappa de regler som du skapade tidigare till profilerna.</span><span class="sxs-lookup"><span data-stu-id="62924-199">Create profile(s) mapping the rules that you created previously to the profiles.</span></span>
3. <span data-ttu-id="62924-200">Valfritt: Skapa meddelanden om autoskalning genom att konfigurera egenskaper för webhook och e-post.</span><span class="sxs-lookup"><span data-stu-id="62924-200">Optional: Create notifications for autoscale by configuring webhook and email properties.</span></span>
4. <span data-ttu-id="62924-201">Skapa en autoskalningsinställning med ett namn på målresursen genom att mappa profiler och meddelanden som du skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="62924-201">Create an autoscale setting with a name on the target resource by mapping the profiles and notifications that you created in the previous steps.</span></span>

<span data-ttu-id="62924-202">I följande exempel visas hur du kan skapa en autoskalningsinställning för en Virtual Machine Scale Set för ett Windows-operativsystem baserade genom att använda mått för CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="62924-202">The following examples show you how you can create an Autoscale setting for a Virtual Machine Scale Set for a Windows operating system based by using the CPU utilization metric.</span></span>

<span data-ttu-id="62924-203">Först skapar du en regel för skalbar, med en instans antal ökning.</span><span class="sxs-lookup"><span data-stu-id="62924-203">First, create a rule to scale-out, with an instance count increase.</span></span>

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionValue 1
```        

<span data-ttu-id="62924-204">Sedan skapar en regel för att skala in, med en instans antal minska.</span><span class="sxs-lookup"><span data-stu-id="62924-204">Next, create a rule to scale-in, with an instance count decrease.</span></span>

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionValue 1
```

<span data-ttu-id="62924-205">Skapa sedan en profil för reglerna.</span><span class="sxs-lookup"><span data-stu-id="62924-205">Then, create a profile for the rules.</span></span>

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

<span data-ttu-id="62924-206">Skapa en webhook-egenskap.</span><span class="sxs-lookup"><span data-stu-id="62924-206">Create a webhook property.</span></span>

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

<span data-ttu-id="62924-207">Skapa meddelande-egenskapen för autoskalningsinställning, inklusive e-post och webhooken som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="62924-207">Create the notification property for the autoscale setting, including email and the webhook that you created previously.</span></span>

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

<span data-ttu-id="62924-208">Skapa slutligen autoskalningsinställningen som du lägger till den profil som du skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="62924-208">Finally, create the autoscale setting to add the profile that you created above.</span></span>

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

<span data-ttu-id="62924-209">Mer information om hur du hanterar Autoskala inställningar finns [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).</span><span class="sxs-lookup"><span data-stu-id="62924-209">For more information about managing Autoscale settings, see [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).</span></span>

## <a name="autoscale-history"></a><span data-ttu-id="62924-210">Autoskala historik</span><span class="sxs-lookup"><span data-stu-id="62924-210">Autoscale history</span></span>
<span data-ttu-id="62924-211">I följande exempel visas hur du kan visa senaste Autoskala och avisering händelser.</span><span class="sxs-lookup"><span data-stu-id="62924-211">The following example shows you how you can view recent autoscale and alert events.</span></span> <span data-ttu-id="62924-212">Använd aktiviteten loggen sökning om du vill visa Autoskala historiken.</span><span class="sxs-lookup"><span data-stu-id="62924-212">Use the activity log search to view the autoscale history.</span></span>

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

<span data-ttu-id="62924-213">Du kan använda den `Get-AzureRmAutoScaleHistory` för att hämta Autoskala historik.</span><span class="sxs-lookup"><span data-stu-id="62924-213">You can use the `Get-AzureRmAutoScaleHistory` cmdlet to retrieve AutoScale history.</span></span>

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

<span data-ttu-id="62924-214">Mer information finns i [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).</span><span class="sxs-lookup"><span data-stu-id="62924-214">For more information, see [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).</span></span>

### <a name="view-details-for-an-autoscale-setting"></a><span data-ttu-id="62924-215">Visa information om en autoskalningsinställning</span><span class="sxs-lookup"><span data-stu-id="62924-215">View details for an autoscale setting</span></span>
<span data-ttu-id="62924-216">Du kan använda den `Get-Autoscalesetting` för att hämta mer information om autoskalningsinställningen.</span><span class="sxs-lookup"><span data-stu-id="62924-216">You can use the `Get-Autoscalesetting` cmdlet to retrieve more information about the autoscale setting.</span></span>

<span data-ttu-id="62924-217">I följande exempel visas information om alla Autoskala inställningar i resursen grupp 'myrg1'.</span><span class="sxs-lookup"><span data-stu-id="62924-217">The following example shows details about all autoscale settings in the resource group 'myrg1'.</span></span>

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

<span data-ttu-id="62924-218">I följande exempel visas information om alla Autoskala inställningar i resursen grupp 'myrg1' och specifikt autoskalningsinställningen med namnet 'MyScaleVMSSSetting'.</span><span class="sxs-lookup"><span data-stu-id="62924-218">The following example shows details about all autoscale settings in the resource group 'myrg1' and specifically the autoscale setting named 'MyScaleVMSSSetting'.</span></span>

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a><span data-ttu-id="62924-219">Ta bort en autoskalningsinställning</span><span class="sxs-lookup"><span data-stu-id="62924-219">Remove an autoscale setting</span></span>
<span data-ttu-id="62924-220">Du kan använda den `Remove-Autoscalesetting` för att ta bort en autoskalningsinställning.</span><span class="sxs-lookup"><span data-stu-id="62924-220">You can use the `Remove-Autoscalesetting` cmdlet to delete an autoscale setting.</span></span>

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-activity-log"></a><span data-ttu-id="62924-221">Hantera loggen profiler för aktivitetsloggen</span><span class="sxs-lookup"><span data-stu-id="62924-221">Manage log profiles for activity log</span></span>
<span data-ttu-id="62924-222">Du kan skapa en *logga profil* och exportera data från din aktivitetsloggen till ett lagringskonto och du kan konfigurera datalagring för den.</span><span class="sxs-lookup"><span data-stu-id="62924-222">You can create a *log profile* and export data from your activity log to a storage account and you can configure data retention for it.</span></span> <span data-ttu-id="62924-223">Du kan också kan du också strömma data till din Event Hub.</span><span class="sxs-lookup"><span data-stu-id="62924-223">Optionally, you can also stream the data to your Event Hub.</span></span> <span data-ttu-id="62924-224">Observera att den här funktionen är för närvarande i förhandsvisning och du kan bara skapa en logg profil per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="62924-224">Note that this feature is currently in Preview and you can only create one log profile per subscription.</span></span> <span data-ttu-id="62924-225">Du kan använda följande cmdlets med din aktuella prenumeration för att skapa och hantera profiler för loggen.</span><span class="sxs-lookup"><span data-stu-id="62924-225">You can use the following cmdlets with your current subscription to create and manage log profiles.</span></span> <span data-ttu-id="62924-226">Du kan också välja en viss prenumeration.</span><span class="sxs-lookup"><span data-stu-id="62924-226">You can also choose a particular subscription.</span></span> <span data-ttu-id="62924-227">Även om PowerShell som standard den aktuella prenumerationen, du kan ändra den med hjälp av `Set-AzureRmContext`.</span><span class="sxs-lookup"><span data-stu-id="62924-227">Although PowerShell defaults to the current subscription, you can always change that using `Set-AzureRmContext`.</span></span> <span data-ttu-id="62924-228">Du kan konfigurera aktivitetsloggen att vidarebefordra data till storage-konto eller Händelsehubb i den prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="62924-228">You can configure activity log to route data to any storage account or Event Hub within that subscription.</span></span> <span data-ttu-id="62924-229">Data skrivs som blob-filer i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="62924-229">Data is written as blob files in JSON format.</span></span>

### <a name="get-a-log-profile"></a><span data-ttu-id="62924-230">Hämta en logg-profil</span><span class="sxs-lookup"><span data-stu-id="62924-230">Get a log profile</span></span>
<span data-ttu-id="62924-231">För att hämta din befintliga log-profiler använder det `Get-AzureRmLogProfile` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="62924-231">To fetch your existing log profiles, use the `Get-AzureRmLogProfile` cmdlet.</span></span>

### <a name="add-a-log-profile-without-data-retention"></a><span data-ttu-id="62924-232">Lägg till en logg profil utan datalagring</span><span class="sxs-lookup"><span data-stu-id="62924-232">Add a log profile without data retention</span></span>
```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a><span data-ttu-id="62924-233">Ta bort en logg-profil</span><span class="sxs-lookup"><span data-stu-id="62924-233">Remove a log profile</span></span>
```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a><span data-ttu-id="62924-234">Lägg till en logg-profil med datalagring</span><span class="sxs-lookup"><span data-stu-id="62924-234">Add a log profile with data retention</span></span>
<span data-ttu-id="62924-235">Du kan ange den **- RetentionInDays** egenskap med antalet dagar som ett positivt heltal, där data bevaras.</span><span class="sxs-lookup"><span data-stu-id="62924-235">You can specify the **-RetentionInDays** property with the number of days, as a positive integer, where the data is retained.</span></span>

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a><span data-ttu-id="62924-236">Lägg till loggen profil med kvarhållning och EventHub</span><span class="sxs-lookup"><span data-stu-id="62924-236">Add log profile with retention and EventHub</span></span>
<span data-ttu-id="62924-237">Förutom routning dina data till storage-konto strömma du den till en Händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="62924-237">In addition to routing your data to storage account, you can also stream it to an Event Hub.</span></span> <span data-ttu-id="62924-238">Observera att i den här förhandsversionen och lagringen kontokonfiguration är obligatorisk men Event Hub-konfigurationen är valfria.</span><span class="sxs-lookup"><span data-stu-id="62924-238">Note that in this preview release and the storage account configuration is mandatory but Event Hub configuration is optional.</span></span>

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a><span data-ttu-id="62924-239">Konfigurera diagnostik-loggar</span><span class="sxs-lookup"><span data-stu-id="62924-239">Configure diagnostics logs</span></span>
<span data-ttu-id="62924-240">Många Azure-tjänster ger ytterligare loggar och telemetri som kan vara konfigurerad för att spara data i Azure Storage-konto, skickar till Händelsehubbar och/eller skickas till en OMS logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="62924-240">Many Azure services provide additional logs and telemetry that can be configured to save data in your Azure Storage account, send to Event Hubs, and/or sent to an OMS Log Analytics workspace.</span></span> <span data-ttu-id="62924-241">Åtgärden kan endast utföras på en resurs-nivå och storage-konto eller händelse hubben ska finnas i samma region som målresursen där inställningen diagnostik konfigureras.</span><span class="sxs-lookup"><span data-stu-id="62924-241">That operation can only be performed at a resource level and the storage account or event hub should be present in the same region as the target resource where the diagnostics setting is configured.</span></span>

### <a name="get-diagnostic-setting"></a><span data-ttu-id="62924-242">Hämta diagnostikinställningen</span><span class="sxs-lookup"><span data-stu-id="62924-242">Get diagnostic setting</span></span>
```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

<span data-ttu-id="62924-243">Inaktivera diagnostikinställningen</span><span class="sxs-lookup"><span data-stu-id="62924-243">Disable diagnostic setting</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

<span data-ttu-id="62924-244">Aktivera diagnostikinställningen utan kvarhållning</span><span class="sxs-lookup"><span data-stu-id="62924-244">Enable diagnostic setting without retention</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

<span data-ttu-id="62924-245">Aktivera diagnostikinställningen med kvarhållning</span><span class="sxs-lookup"><span data-stu-id="62924-245">Enable diagnostic setting with retention</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

<span data-ttu-id="62924-246">Aktivera diagnostikinställningen med kvarhållning för en viss loggning kategori</span><span class="sxs-lookup"><span data-stu-id="62924-246">Enable diagnostic setting with retention for a specific log category</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

<span data-ttu-id="62924-247">Aktivera diagnostikinställningen för Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="62924-247">Enable diagnostic setting for Event Hubs</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Enable $true
```

<span data-ttu-id="62924-248">Aktivera diagnostikinställningen för OMS</span><span class="sxs-lookup"><span data-stu-id="62924-248">Enable diagnostic setting for OMS</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -WorkspaceId 76d785fd-d1ce-4f50-8ca3-858fc819ca0f -Enabled $true

```
