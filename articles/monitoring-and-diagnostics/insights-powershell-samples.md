---
title: "aaaAzure övervakaren PowerShell Snabbstart-exempel. | Microsoft Docs"
description: "Använd PowerShell tooaccess Azure-Monitor-funktioner, till exempel Autoskala, aviseringar, webhooks och söka aktivitetsloggar."
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
ms.openlocfilehash: 6eece0b0227e0bbf08225bd330d0601169911f55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitor-powershell-quick-start-samples"></a><span data-ttu-id="fd2f5-104">Azure-Monitor PowerShell Snabbstart-exempel</span><span class="sxs-lookup"><span data-stu-id="fd2f5-104">Azure Monitor PowerShell quick start samples</span></span>
<span data-ttu-id="fd2f5-105">Den här artikeln innehåller exempel på PowerShell-kommandon toohelp du komma åt Azure-Monitor-funktioner.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-105">This article shows you sample PowerShell commands toohelp you access Azure Monitor features.</span></span> <span data-ttu-id="fd2f5-106">Azure övervakaren kan tooAutoScale molntjänster, virtuella datorer, och Web Apps och toosend aviseringsmeddelanden eller anropet webbadresser som baseras på värden för konfigurerade telemetridata.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-106">Azure Monitor allows you tooAutoScale Cloud Services, Virtual Machines, and Web Apps and toosend alert notifications or call web URLs based on values of configured telemetry data.</span></span>

> [!NOTE]
> <span data-ttu-id="fd2f5-107">Azure övervakaren är hello nytt namn för vad anropades ”Azure Insights” förrän den 25 september 2016.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-107">Azure Monitor is hello new name for what was called "Azure Insights" until Sept 25th, 2016.</span></span> <span data-ttu-id="fd2f5-108">Hello namnområden och därmed hello följande kommandon fortfarande innehåller dock insikter ”hello”.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-108">However, hello namespaces and thus hello following commands still contain hello "insights".</span></span>
> 
> 

## <a name="set-up-powershell"></a><span data-ttu-id="fd2f5-109">Konfigurera PowerShell</span><span class="sxs-lookup"><span data-stu-id="fd2f5-109">Set up PowerShell</span></span>
<span data-ttu-id="fd2f5-110">Om du inte redan gjort ställa in PowerShell toorun på datorn.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-110">If you haven't already, set up PowerShell toorun on your computer.</span></span> <span data-ttu-id="fd2f5-111">Mer information finns i [hur tooInstall och konfigurera PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fd2f5-111">For more information, see [How tooInstall and Configure PowerShell](/powershell/azure/overview).</span></span>

## <a name="examples-in-this-article"></a><span data-ttu-id="fd2f5-112">Exemplen i den här artikeln</span><span class="sxs-lookup"><span data-stu-id="fd2f5-112">Examples in this article</span></span>
<span data-ttu-id="fd2f5-113">hello exemplen i hello artikeln visar hur du kan använda Azure-Monitor-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-113">hello examples in hello article illustrate how you can use Azure Monitor cmdlets.</span></span> <span data-ttu-id="fd2f5-114">Du kan också granska hello hela listan med Azure-Monitor PowerShell-cmdlets på [Azure-Monitor (insikter) Cmdlets](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).</span><span class="sxs-lookup"><span data-stu-id="fd2f5-114">You can also review hello entire list of Azure Monitor PowerShell cmdlets at [Azure Monitor (Insights) Cmdlets](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).</span></span>

## <a name="sign-in-and-use-subscriptions"></a><span data-ttu-id="fd2f5-115">Logga in och använda prenumerationer</span><span class="sxs-lookup"><span data-stu-id="fd2f5-115">Sign in and use subscriptions</span></span>
<span data-ttu-id="fd2f5-116">Logga först in tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-116">First, log in tooyour Azure subscription.</span></span>

```PowerShell
Login-AzureRmAccount
```

<span data-ttu-id="fd2f5-117">Detta kräver toosign i.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-117">This requires you toosign in.</span></span> <span data-ttu-id="fd2f5-118">När du gör ditt konto, visas TenantID och standard prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-118">Once you do, your Account, TenantID and default Subscription ID are displayed.</span></span> <span data-ttu-id="fd2f5-119">Alla hello Azure-cmdlets arbete i prenumerationen standard hello kontext.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-119">All hello Azure cmdlets work in hello context of your default subscription.</span></span> <span data-ttu-id="fd2f5-120">tooview hello listan över prenumerationer som du har åtkomst till, Använd hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-120">tooview hello list of subscriptions you have access to, use hello following command.</span></span>

```PowerShell
Get-AzureRmSubscription
```

<span data-ttu-id="fd2f5-121">toochange arbeta kontexten tooa olika prenumerationen, Använd hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-121">toochange your working context tooa different subscription, use hello following command.</span></span>

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-activity-log-for-a-subscription"></a><span data-ttu-id="fd2f5-122">Hämta aktivitetsloggen för en prenumeration</span><span class="sxs-lookup"><span data-stu-id="fd2f5-122">Retrieve Activity log for a subscription</span></span>
<span data-ttu-id="fd2f5-123">Använd hello `Get-AzureRmLog` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-123">Use hello `Get-AzureRmLog` cmdlet.</span></span>  <span data-ttu-id="fd2f5-124">hello nedan följer några vanliga exempel.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-124">hello following are some common examples.</span></span>

<span data-ttu-id="fd2f5-125">Hämta loggposter från denna tid/datum toopresent:</span><span class="sxs-lookup"><span data-stu-id="fd2f5-125">Get log entries from this time/date toopresent:</span></span>

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

<span data-ttu-id="fd2f5-126">Hämta loggposter ett värdeområde tid/datum:</span><span class="sxs-lookup"><span data-stu-id="fd2f5-126">Get log entries between a time/date range:</span></span>

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

<span data-ttu-id="fd2f5-127">Hämta loggposter från en viss resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="fd2f5-127">Get log entries from a specific resource group:</span></span>

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

<span data-ttu-id="fd2f5-128">Hämta loggposter från en viss resurs-leverantör ett värdeområde tid/datum:</span><span class="sxs-lookup"><span data-stu-id="fd2f5-128">Get log entries from a specific resource provider between a time/date range:</span></span>

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

<span data-ttu-id="fd2f5-129">Hämta alla loggposter med en specifik anropare:</span><span class="sxs-lookup"><span data-stu-id="fd2f5-129">Get all log entries with a specific caller:</span></span>

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

<span data-ttu-id="fd2f5-130">följande kommando hämtar hello senaste 1 000 händelser från hello aktivitetsloggen hello:</span><span class="sxs-lookup"><span data-stu-id="fd2f5-130">hello following command retrieves hello last 1000 events from hello activity log:</span></span>

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

<span data-ttu-id="fd2f5-131">`Get-AzureRmLog`har stöd för många parametrar.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-131">`Get-AzureRmLog` supports many other parameters.</span></span> <span data-ttu-id="fd2f5-132">Se hello `Get-AzureRmLog` referens för mer information.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-132">See hello `Get-AzureRmLog` reference for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="fd2f5-133">`Get-AzureRmLog`endast ger 15 dagar tidigare.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-133">`Get-AzureRmLog` only provides 15 days of history.</span></span> <span data-ttu-id="fd2f5-134">Med hjälp av hello **- MaxEvents** parametern kan du tooquery hello sista N händelser, utöver 15 dagar.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-134">Using hello **-MaxEvents** parameter allows you tooquery hello last N events, beyond 15 days.</span></span> <span data-ttu-id="fd2f5-135">tooaccess händelser som är äldre än 15 dagar använda hello REST API eller SDK (C# exempel med hjälp av hello SDK).</span><span class="sxs-lookup"><span data-stu-id="fd2f5-135">tooaccess events older than 15 days, use hello REST API or SDK (C# sample using hello SDK).</span></span> <span data-ttu-id="fd2f5-136">Om du inte inkluderar **StartTime**, då är standardvärdet för hello **EndTime** minus en timme.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-136">If you do not include **StartTime**, then hello default value is **EndTime** minus one hour.</span></span> <span data-ttu-id="fd2f5-137">Om du inte inkluderar **EndTime**, och sedan hello standardvärdet är aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-137">If you do not include **EndTime**, then hello default value is current time.</span></span> <span data-ttu-id="fd2f5-138">Det finns alltid i UTC.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-138">All times are in UTC.</span></span>
> 
> 

## <a name="retrieve-alerts-history"></a><span data-ttu-id="fd2f5-139">Hämta aviseringar historik</span><span class="sxs-lookup"><span data-stu-id="fd2f5-139">Retrieve alerts history</span></span>
<span data-ttu-id="fd2f5-140">tooview alla Varna händelser, du kan fråga hello Azure Resource Manager loggar med hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-140">tooview all alert events, you can query hello Azure Resource Manager logs using hello following examples.</span></span>

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

<span data-ttu-id="fd2f5-141">tooview hello historik för en specifik avisering regel, kan du använda hello `Get-AzureRmAlertHistory` cmdlet, skicka i hello resurs-ID för hello varningsregel.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-141">tooview hello history for a specific alert rule, you can use hello `Get-AzureRmAlertHistory` cmdlet, passing in hello resource ID of hello alert rule.</span></span>

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

<span data-ttu-id="fd2f5-142">Hej `Get-AzureRmAlertHistory` cmdlet har stöd för olika parametrar.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-142">hello `Get-AzureRmAlertHistory` cmdlet supports various parameters.</span></span> <span data-ttu-id="fd2f5-143">Mer information finns i [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).</span><span class="sxs-lookup"><span data-stu-id="fd2f5-143">More information, see [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).</span></span>

## <a name="retrieve-information-on-alert-rules"></a><span data-ttu-id="fd2f5-144">Hämta information om Varningsregler</span><span class="sxs-lookup"><span data-stu-id="fd2f5-144">Retrieve information on alert rules</span></span>
<span data-ttu-id="fd2f5-145">Alla hello följande kommandon fungerar på en resursgrupp med namnet ”montest”.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-145">All of hello following commands act on a Resource Group named "montest".</span></span>

<span data-ttu-id="fd2f5-146">Visa alla hello egenskaper för hello varningsregeln:</span><span class="sxs-lookup"><span data-stu-id="fd2f5-146">View all hello properties of hello alert rule:</span></span>

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

<span data-ttu-id="fd2f5-147">Hämta alla aviseringar på en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="fd2f5-147">Retrieve all alerts on a resource group:</span></span>

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

<span data-ttu-id="fd2f5-148">Hämta alla Varningsregler för en målresurs.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-148">Retrieve all alert rules set for a target resource.</span></span> <span data-ttu-id="fd2f5-149">Till exempel ange alla Varningsregler på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-149">For example, all alert rules set on a VM.</span></span>

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

<span data-ttu-id="fd2f5-150">`Get-AzureRmAlertRule`har stöd för andra parametrar.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-150">`Get-AzureRmAlertRule` supports other parameters.</span></span> <span data-ttu-id="fd2f5-151">Se [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-151">See [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) for more information.</span></span>

## <a name="create-metric-alerts"></a><span data-ttu-id="fd2f5-152">Skapa mått aviseringar</span><span class="sxs-lookup"><span data-stu-id="fd2f5-152">Create metric alerts</span></span>
<span data-ttu-id="fd2f5-153">Du kan använda hello `Add-AlertRule` cmdlet toocreate, uppdatera eller inaktivera en aviseringsregel.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-153">You can use hello `Add-AlertRule` cmdlet toocreate, update or disable an alert rule.</span></span>

<span data-ttu-id="fd2f5-154">Du kan skapa e-post och webhook-egenskaper med `New-AzureRmAlertRuleEmail` och `New-AzureRmAlertRuleWebhook`respektive.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-154">You can create email and webhook properties using  `New-AzureRmAlertRuleEmail` and `New-AzureRmAlertRuleWebhook`, respectively.</span></span> <span data-ttu-id="fd2f5-155">Tilldela dessa som åtgärder toohello i hello varningsregeln cmdlet **åtgärder** -egenskapen för hello varningsregel.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-155">In hello Alert rule cmdlet, assign these as actions toohello **Actions** property of hello Alert Rule.</span></span>

<span data-ttu-id="fd2f5-156">hello i den följande tabellen beskrivs hello parametrar och värden används toocreate en avisering med ett mått.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-156">hello following table describes hello parameters and values used toocreate an alert using a metric.</span></span>

| <span data-ttu-id="fd2f5-157">Parametern</span><span class="sxs-lookup"><span data-stu-id="fd2f5-157">parameter</span></span> | <span data-ttu-id="fd2f5-158">värde</span><span class="sxs-lookup"><span data-stu-id="fd2f5-158">value</span></span> |
| --- | --- |
| <span data-ttu-id="fd2f5-159">Namn</span><span class="sxs-lookup"><span data-stu-id="fd2f5-159">Name</span></span> |<span data-ttu-id="fd2f5-160">simpletestdiskwrite</span><span class="sxs-lookup"><span data-stu-id="fd2f5-160">simpletestdiskwrite</span></span> |
| <span data-ttu-id="fd2f5-161">Platsen för den här varningsregeln</span><span class="sxs-lookup"><span data-stu-id="fd2f5-161">Location of this alert rule</span></span> |<span data-ttu-id="fd2f5-162">Östra USA</span><span class="sxs-lookup"><span data-stu-id="fd2f5-162">East US</span></span> |
| <span data-ttu-id="fd2f5-163">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="fd2f5-163">ResourceGroup</span></span> |<span data-ttu-id="fd2f5-164">montest</span><span class="sxs-lookup"><span data-stu-id="fd2f5-164">montest</span></span> |
| <span data-ttu-id="fd2f5-165">TargetResourceId</span><span class="sxs-lookup"><span data-stu-id="fd2f5-165">TargetResourceId</span></span> |<span data-ttu-id="fd2f5-166">/subscriptions/S1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig</span><span class="sxs-lookup"><span data-stu-id="fd2f5-166">/subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig</span></span> |
| <span data-ttu-id="fd2f5-167">MetricName hello varning som skapas</span><span class="sxs-lookup"><span data-stu-id="fd2f5-167">MetricName of hello alert that is created</span></span> |<span data-ttu-id="fd2f5-168">\PhysicalDisk (_Total) \Disk Diskskrivningar/sek. Se hello `Get-MetricDefinitions` cmdlet om hur tooretrieve hello exakta mått namn</span><span class="sxs-lookup"><span data-stu-id="fd2f5-168">\PhysicalDisk(_Total)\Disk Writes/sec. See hello `Get-MetricDefinitions` cmdlet about how tooretrieve hello exact metric names</span></span> |
| <span data-ttu-id="fd2f5-169">Operatorn</span><span class="sxs-lookup"><span data-stu-id="fd2f5-169">operator</span></span> |<span data-ttu-id="fd2f5-170">GreaterThan</span><span class="sxs-lookup"><span data-stu-id="fd2f5-170">GreaterThan</span></span> |
| <span data-ttu-id="fd2f5-171">Tröskelvärde (antal per sekund i för det här måttet)</span><span class="sxs-lookup"><span data-stu-id="fd2f5-171">Threshold value (count/sec in for this metric)</span></span> |<span data-ttu-id="fd2f5-172">1</span><span class="sxs-lookup"><span data-stu-id="fd2f5-172">1</span></span> |
| <span data-ttu-id="fd2f5-173">Fönsterstorlek (format: mm: ss)</span><span class="sxs-lookup"><span data-stu-id="fd2f5-173">WindowSize (hh:mm:ss format)</span></span> |<span data-ttu-id="fd2f5-174">00:05:00</span><span class="sxs-lookup"><span data-stu-id="fd2f5-174">00:05:00</span></span> |
| <span data-ttu-id="fd2f5-175">Aggregator (statistik för hello måttet, som använder Genomsnittligt antal i det här fallet)</span><span class="sxs-lookup"><span data-stu-id="fd2f5-175">aggregator (statistic of hello metric, which uses Average count, in this case)</span></span> |<span data-ttu-id="fd2f5-176">Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="fd2f5-176">Average</span></span> |
| <span data-ttu-id="fd2f5-177">anpassad e-postmeddelanden (Strängmatrisen)</span><span class="sxs-lookup"><span data-stu-id="fd2f5-177">custom emails (string array)</span></span> |<span data-ttu-id="fd2f5-178">'foo@example.com','bar@example.com'</span><span class="sxs-lookup"><span data-stu-id="fd2f5-178">'foo@example.com','bar@example.com'</span></span> |
| <span data-ttu-id="fd2f5-179">Skicka e-tooowners, deltagare och läsare</span><span class="sxs-lookup"><span data-stu-id="fd2f5-179">send email tooowners, contributors and readers</span></span> |<span data-ttu-id="fd2f5-180">-SendToServiceOwners</span><span class="sxs-lookup"><span data-stu-id="fd2f5-180">-SendToServiceOwners</span></span> |

<span data-ttu-id="fd2f5-181">Skapa en e-åtgärd</span><span class="sxs-lookup"><span data-stu-id="fd2f5-181">Create an Email action</span></span>

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

<span data-ttu-id="fd2f5-182">Skapa en Webhook-åtgärd</span><span class="sxs-lookup"><span data-stu-id="fd2f5-182">Create a Webhook action</span></span>

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

<span data-ttu-id="fd2f5-183">Skapa hello varningsregeln på hello CPU % mått på en klassisk virtuell dator</span><span class="sxs-lookup"><span data-stu-id="fd2f5-183">Create hello alert rule on hello CPU% metric on a classic VM</span></span>

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

<span data-ttu-id="fd2f5-184">Hämta hello varningsregel</span><span class="sxs-lookup"><span data-stu-id="fd2f5-184">Retrieve hello alert rule</span></span>

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

<span data-ttu-id="fd2f5-185">hello Lägg till avisering cmdlet uppdateras även hello regel om det finns redan en varningsregel för hello angivna egenskaper.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-185">hello Add alert cmdlet also updates hello rule if an alert rule already exists for hello given properties.</span></span> <span data-ttu-id="fd2f5-186">toodisable en aviseringsregel inkluderar hello parametern **- DisableRule**.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-186">toodisable an alert rule, include hello parameter **-DisableRule**.</span></span>

## <a name="get-a-list-of-available-metrics-for-alerts"></a><span data-ttu-id="fd2f5-187">Hämta en lista över tillgängliga mått för aviseringar</span><span class="sxs-lookup"><span data-stu-id="fd2f5-187">Get a list of available metrics for alerts</span></span>
<span data-ttu-id="fd2f5-188">Du kan använda hello `Get-AzureRmMetricDefinition` cmdlet tooview hello lista över alla mätvärden för en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-188">You can use hello `Get-AzureRmMetricDefinition` cmdlet tooview hello list of all metrics for a specific resource.</span></span>

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

<span data-ttu-id="fd2f5-189">hello skapar följande exempel en tabell med hello mått namn och hello enhet för den.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-189">hello following example generates a table with hello metric Name and hello Unit for it.</span></span>

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

<span data-ttu-id="fd2f5-190">En fullständig lista över tillgängliga alternativ för `Get-AzureRmMetricDefinition` finns på [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).</span><span class="sxs-lookup"><span data-stu-id="fd2f5-190">A full list of available options for `Get-AzureRmMetricDefinition` is available at [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).</span></span>

## <a name="create-and-manage-autoscale-settings"></a><span data-ttu-id="fd2f5-191">Skapa och hantera Autoskala inställningar</span><span class="sxs-lookup"><span data-stu-id="fd2f5-191">Create and manage AutoScale settings</span></span>
<span data-ttu-id="fd2f5-192">En resurs, till exempel en webbapp VM, molntjänst eller Skaluppsättning för virtuell dator kan ha endast en autoskalningsinställning som konfigurerats för den.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-192">A resource, such as a Web app, VM, Cloud Service or Virtual Machine Scale Set can have only one autoscale setting configured for it.</span></span>
<span data-ttu-id="fd2f5-193">Varje autoskalningsinställning kan dock ha flera profiler.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-193">However, each autoscale setting can have multiple profiles.</span></span> <span data-ttu-id="fd2f5-194">Till exempel en för en prestandabaserad skala profil och en andra princip för en schemabaserade profil.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-194">For example, one for a performance-based scale profile and a second one for a schedule-based profile.</span></span> <span data-ttu-id="fd2f5-195">Varje profil kan ha flera regler som konfigurerats på den.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-195">Each profile can have multiple rules configured on it.</span></span> <span data-ttu-id="fd2f5-196">Läs mer om Autoskala [hur tooAutoscale ett program](../cloud-services/cloud-services-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="fd2f5-196">For more information about Autoscale, see [How tooAutoscale an Application](../cloud-services/cloud-services-how-to-scale.md).</span></span>

<span data-ttu-id="fd2f5-197">Här är hello steg kommer vi att använda:</span><span class="sxs-lookup"><span data-stu-id="fd2f5-197">Here are hello steps we will use:</span></span>

1. <span data-ttu-id="fd2f5-198">Skapa regler.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-198">Create rule(s).</span></span>
2. <span data-ttu-id="fd2f5-199">Skapa eller profilerna mappning hello regler som du tidigare skapade toohello profiler.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-199">Create profile(s) mapping hello rules that you created previously toohello profiles.</span></span>
3. <span data-ttu-id="fd2f5-200">Valfritt: Skapa meddelanden om autoskalning genom att konfigurera egenskaper för webhook och e-post.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-200">Optional: Create notifications for autoscale by configuring webhook and email properties.</span></span>
4. <span data-ttu-id="fd2f5-201">Skapa en autoskalningsinställning med ett namn på hello målresurs genom att mappa hello profiler och meddelanden som du skapade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-201">Create an autoscale setting with a name on hello target resource by mapping hello profiles and notifications that you created in hello previous steps.</span></span>

<span data-ttu-id="fd2f5-202">hello visar följande exempel hur du kan skapa en autoskalningsinställning för en Virtual Machine Scale Set för ett Windows-operativsystem baserade genom att använda hello CPU-användning mått.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-202">hello following examples show you how you can create an Autoscale setting for a Virtual Machine Scale Set for a Windows operating system based by using hello CPU utilization metric.</span></span>

<span data-ttu-id="fd2f5-203">Först skapa en regel tooscale ut, med en instans antal ökning.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-203">First, create a rule tooscale-out, with an instance count increase.</span></span>

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionValue 1
```        

<span data-ttu-id="fd2f5-204">Skapa en regel tooscale i, med en instans antal minska.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-204">Next, create a rule tooscale-in, with an instance count decrease.</span></span>

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionValue 1
```

<span data-ttu-id="fd2f5-205">Skapa sedan en profil för hello regler.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-205">Then, create a profile for hello rules.</span></span>

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

<span data-ttu-id="fd2f5-206">Skapa en webhook-egenskap.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-206">Create a webhook property.</span></span>

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

<span data-ttu-id="fd2f5-207">Skapa hello notification-egenskapen för hello autoskalningsinställning, inklusive e-post och hello webhook som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-207">Create hello notification property for hello autoscale setting, including email and hello webhook that you created previously.</span></span>

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

<span data-ttu-id="fd2f5-208">Skapa slutligen hello Autoskala inställningen tooadd hello profil som du skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-208">Finally, create hello autoscale setting tooadd hello profile that you created above.</span></span>

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

<span data-ttu-id="fd2f5-209">Mer information om hur du hanterar Autoskala inställningar finns [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).</span><span class="sxs-lookup"><span data-stu-id="fd2f5-209">For more information about managing Autoscale settings, see [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).</span></span>

## <a name="autoscale-history"></a><span data-ttu-id="fd2f5-210">Autoskala historik</span><span class="sxs-lookup"><span data-stu-id="fd2f5-210">Autoscale history</span></span>
<span data-ttu-id="fd2f5-211">hello följande exempel visar hur du kan visa senaste Autoskala och aviseringen händelser.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-211">hello following example shows you how you can view recent autoscale and alert events.</span></span> <span data-ttu-id="fd2f5-212">Använd hello loggen Sök tooview hello Autoskala aktivitetshistorik.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-212">Use hello activity log search tooview hello autoscale history.</span></span>

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

<span data-ttu-id="fd2f5-213">Du kan använda hello `Get-AzureRmAutoScaleHistory` cmdlet tooretrieve Autoskala historik.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-213">You can use hello `Get-AzureRmAutoScaleHistory` cmdlet tooretrieve AutoScale history.</span></span>

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

<span data-ttu-id="fd2f5-214">Mer information finns i [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).</span><span class="sxs-lookup"><span data-stu-id="fd2f5-214">For more information, see [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).</span></span>

### <a name="view-details-for-an-autoscale-setting"></a><span data-ttu-id="fd2f5-215">Visa information om en autoskalningsinställning</span><span class="sxs-lookup"><span data-stu-id="fd2f5-215">View details for an autoscale setting</span></span>
<span data-ttu-id="fd2f5-216">Du kan använda hello `Get-Autoscalesetting` cmdlet tooretrieve mer information om hello autoskalningsinställning.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-216">You can use hello `Get-Autoscalesetting` cmdlet tooretrieve more information about hello autoscale setting.</span></span>

<span data-ttu-id="fd2f5-217">hello följande exempel visas information om alla Autoskala inställningar i myrg1' hello resurs grupp'.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-217">hello following example shows details about all autoscale settings in hello resource group 'myrg1'.</span></span>

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

<span data-ttu-id="fd2f5-218">hello följande exempel visar information om alla Autoskala inställningar i myrg1' hello resurs grupp' och specifikt hello autoskalningsinställning med namnet 'MyScaleVMSSSetting'.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-218">hello following example shows details about all autoscale settings in hello resource group 'myrg1' and specifically hello autoscale setting named 'MyScaleVMSSSetting'.</span></span>

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a><span data-ttu-id="fd2f5-219">Ta bort en autoskalningsinställning</span><span class="sxs-lookup"><span data-stu-id="fd2f5-219">Remove an autoscale setting</span></span>
<span data-ttu-id="fd2f5-220">Du kan använda hello `Remove-Autoscalesetting` cmdlet toodelete en autoskalningsinställning.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-220">You can use hello `Remove-Autoscalesetting` cmdlet toodelete an autoscale setting.</span></span>

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-activity-log"></a><span data-ttu-id="fd2f5-221">Hantera loggen profiler för aktivitetsloggen</span><span class="sxs-lookup"><span data-stu-id="fd2f5-221">Manage log profiles for activity log</span></span>
<span data-ttu-id="fd2f5-222">Du kan skapa en *logga profil* och exportera data från din verksamhet loggen tooa storage-konto och du kan konfigurera datalagring för den.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-222">You can create a *log profile* and export data from your activity log tooa storage account and you can configure data retention for it.</span></span> <span data-ttu-id="fd2f5-223">Alternativt kan strömma du också hello data tooyour Event Hub.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-223">Optionally, you can also stream hello data tooyour Event Hub.</span></span> <span data-ttu-id="fd2f5-224">Observera att den här funktionen är för närvarande i förhandsvisning och du kan bara skapa en logg profil per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-224">Note that this feature is currently in Preview and you can only create one log profile per subscription.</span></span> <span data-ttu-id="fd2f5-225">Du kan använda följande cmdlets med din aktuella prenumeration toocreate hello och hantera profiler för loggen.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-225">You can use hello following cmdlets with your current subscription toocreate and manage log profiles.</span></span> <span data-ttu-id="fd2f5-226">Du kan också välja en viss prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-226">You can also choose a particular subscription.</span></span> <span data-ttu-id="fd2f5-227">Även om PowerShell standarder toohello aktuell prenumeration, du kan ändra den med hjälp av `Set-AzureRmContext`.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-227">Although PowerShell defaults toohello current subscription, you can always change that using `Set-AzureRmContext`.</span></span> <span data-ttu-id="fd2f5-228">Du kan konfigurera aktiviteten loggen tooroute data tooany storage-konto eller Händelsehubb i den prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-228">You can configure activity log tooroute data tooany storage account or Event Hub within that subscription.</span></span> <span data-ttu-id="fd2f5-229">Data skrivs som blob-filer i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-229">Data is written as blob files in JSON format.</span></span>

### <a name="get-a-log-profile"></a><span data-ttu-id="fd2f5-230">Hämta en logg-profil</span><span class="sxs-lookup"><span data-stu-id="fd2f5-230">Get a log profile</span></span>
<span data-ttu-id="fd2f5-231">toofetch din befintliga log-profiler kan använda hello `Get-AzureRmLogProfile` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-231">toofetch your existing log profiles, use hello `Get-AzureRmLogProfile` cmdlet.</span></span>

### <a name="add-a-log-profile-without-data-retention"></a><span data-ttu-id="fd2f5-232">Lägg till en logg profil utan datalagring</span><span class="sxs-lookup"><span data-stu-id="fd2f5-232">Add a log profile without data retention</span></span>
```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a><span data-ttu-id="fd2f5-233">Ta bort en logg-profil</span><span class="sxs-lookup"><span data-stu-id="fd2f5-233">Remove a log profile</span></span>
```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a><span data-ttu-id="fd2f5-234">Lägg till en logg-profil med datalagring</span><span class="sxs-lookup"><span data-stu-id="fd2f5-234">Add a log profile with data retention</span></span>
<span data-ttu-id="fd2f5-235">Du kan ange hello **- RetentionInDays** egenskap med hello antalet dagar som ett positivt heltal där hello data sparas.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-235">You can specify hello **-RetentionInDays** property with hello number of days, as a positive integer, where hello data is retained.</span></span>

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a><span data-ttu-id="fd2f5-236">Lägg till loggen profil med kvarhållning och EventHub</span><span class="sxs-lookup"><span data-stu-id="fd2f5-236">Add log profile with retention and EventHub</span></span>
<span data-ttu-id="fd2f5-237">I tillägg toorouting data toostorage-konto du kan också strömmas tooan Event Hub.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-237">In addition toorouting your data toostorage account, you can also stream it tooan Event Hub.</span></span> <span data-ttu-id="fd2f5-238">Observera att i den här förhandsgranskningen versionen och hello konto lagringskonfiguration är obligatorisk men Event Hub-konfigurationen är valfria.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-238">Note that in this preview release and hello storage account configuration is mandatory but Event Hub configuration is optional.</span></span>

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a><span data-ttu-id="fd2f5-239">Konfigurera diagnostik-loggar</span><span class="sxs-lookup"><span data-stu-id="fd2f5-239">Configure diagnostics logs</span></span>
<span data-ttu-id="fd2f5-240">Många Azure-tjänster ger ytterligare loggar och telemetri som kan vara konfigurerade toosave data i ditt Azure Storage-konto kan du skicka tooEvent NAV och/eller skickas tooan OMS logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-240">Many Azure services provide additional logs and telemetry that can be configured toosave data in your Azure Storage account, send tooEvent Hubs, and/or sent tooan OMS Log Analytics workspace.</span></span> <span data-ttu-id="fd2f5-241">Åtgärden kan endast utföras på en resurs-nivå och hello storage-konto eller händelse hubb ska finnas i hello samma region som hello målresurs där hello diagnostik inställningen konfigureras.</span><span class="sxs-lookup"><span data-stu-id="fd2f5-241">That operation can only be performed at a resource level and hello storage account or event hub should be present in hello same region as hello target resource where hello diagnostics setting is configured.</span></span>

### <a name="get-diagnostic-setting"></a><span data-ttu-id="fd2f5-242">Hämta diagnostikinställningen</span><span class="sxs-lookup"><span data-stu-id="fd2f5-242">Get diagnostic setting</span></span>
```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

<span data-ttu-id="fd2f5-243">Inaktivera diagnostikinställningen</span><span class="sxs-lookup"><span data-stu-id="fd2f5-243">Disable diagnostic setting</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

<span data-ttu-id="fd2f5-244">Aktivera diagnostikinställningen utan kvarhållning</span><span class="sxs-lookup"><span data-stu-id="fd2f5-244">Enable diagnostic setting without retention</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

<span data-ttu-id="fd2f5-245">Aktivera diagnostikinställningen med kvarhållning</span><span class="sxs-lookup"><span data-stu-id="fd2f5-245">Enable diagnostic setting with retention</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

<span data-ttu-id="fd2f5-246">Aktivera diagnostikinställningen med kvarhållning för en viss loggning kategori</span><span class="sxs-lookup"><span data-stu-id="fd2f5-246">Enable diagnostic setting with retention for a specific log category</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

<span data-ttu-id="fd2f5-247">Aktivera diagnostikinställningen för Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="fd2f5-247">Enable diagnostic setting for Event Hubs</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Enable $true
```

<span data-ttu-id="fd2f5-248">Aktivera diagnostikinställningen för OMS</span><span class="sxs-lookup"><span data-stu-id="fd2f5-248">Enable diagnostic setting for OMS</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -WorkspaceId 76d785fd-d1ce-4f50-8ca3-858fc819ca0f -Enabled $true

```
