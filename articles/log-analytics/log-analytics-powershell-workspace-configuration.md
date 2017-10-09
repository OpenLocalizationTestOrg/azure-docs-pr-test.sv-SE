---
title: aaaUse PowerShell tooCreate och konfigurera en Log Analytics-arbetsyta | Microsoft Docs
description: "Logga Analytics använder data från servrar i din lokala eller molnbaserade infrastrukturen. Du kan samla in Maskindata från Azure storage när genereras av Azure-diagnostik."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: 3b9b7ade-3374-4596-afb1-51b695f481c2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: article
ms.date: 11/21/2016
ms.author: richrund
ms.openlocfilehash: a6d66194204cc58de6aafb687a19fe9611e0c58e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-log-analytics-using-powershell"></a><span data-ttu-id="fac96-104">Hantera Log Analytics med PowerShell</span><span class="sxs-lookup"><span data-stu-id="fac96-104">Manage Log Analytics using PowerShell</span></span>
<span data-ttu-id="fac96-105">Du kan använda hello [Log Analytics PowerShell-cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) tooperform olika funktioner i logganalys från en kommandorad eller som en del av ett skript.</span><span class="sxs-lookup"><span data-stu-id="fac96-105">You can use hello [Log Analytics PowerShell cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) tooperform various functions in Log Analytics from a command line or as part of a script.</span></span>  <span data-ttu-id="fac96-106">Exempel på hello uppgifter du kan utföra med PowerShell:</span><span class="sxs-lookup"><span data-stu-id="fac96-106">Examples of hello tasks you can perform with PowerShell include:</span></span>

* <span data-ttu-id="fac96-107">Skapa en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="fac96-107">Create a workspace</span></span>
* <span data-ttu-id="fac96-108">Lägg till eller ta bort en lösning</span><span class="sxs-lookup"><span data-stu-id="fac96-108">Add or remove a solution</span></span>
* <span data-ttu-id="fac96-109">Importera och exportera sparade sökningar</span><span class="sxs-lookup"><span data-stu-id="fac96-109">Import and export saved searches</span></span>
* <span data-ttu-id="fac96-110">Skapa en datorgrupp</span><span class="sxs-lookup"><span data-stu-id="fac96-110">Create a computer group</span></span>
* <span data-ttu-id="fac96-111">Aktivera insamling av IIS-loggar från datorer med Windows hello-agenten installerad</span><span class="sxs-lookup"><span data-stu-id="fac96-111">Enable collection of IIS logs from computers with hello Windows agent installed</span></span>
* <span data-ttu-id="fac96-112">Samla in prestandaräknare från Linux och Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="fac96-112">Collect performance counters from Linux and Windows computers</span></span>
* <span data-ttu-id="fac96-113">Samla in händelser från syslog på Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="fac96-113">Collect events from syslog on Linux computers</span></span> 
* <span data-ttu-id="fac96-114">Samla in händelser från händelseloggarna i Windows</span><span class="sxs-lookup"><span data-stu-id="fac96-114">Collect events from Windows event logs</span></span>
* <span data-ttu-id="fac96-115">Samla in anpassade händelseloggar</span><span class="sxs-lookup"><span data-stu-id="fac96-115">Collect custom event logs</span></span>
* <span data-ttu-id="fac96-116">Lägg till hello log analytics agent tooan virtuella Azure-datorn</span><span class="sxs-lookup"><span data-stu-id="fac96-116">Add hello log analytics agent tooan Azure virtual machine</span></span>
* <span data-ttu-id="fac96-117">Konfigurera log analytics tooindex data som samlas in med hjälp av Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="fac96-117">Configure log analytics tooindex data collected using Azure diagnostics</span></span>

<span data-ttu-id="fac96-118">Den här artikeln innehåller två kodexempel som visar några av hello-funktioner som du kan utföra från PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fac96-118">This article provides two code samples that illustrate some of hello functions that you can perform from PowerShell.</span></span>  <span data-ttu-id="fac96-119">Du kan se toohello [Log Analytics PowerShell cmdlet-referens](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) för andra funktioner.</span><span class="sxs-lookup"><span data-stu-id="fac96-119">You can refer toohello [Log Analytics PowerShell cmdlet reference](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) for other functions.</span></span>

> [!NOTE]
> <span data-ttu-id="fac96-120">Logganalys kallades tidigare åtgärdsinformation, som det är därför hello-namnet som används i hello-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="fac96-120">Log Analytics was previously called Operational Insights, which is why it is hello name used in hello cmdlets.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="fac96-121">Krav</span><span class="sxs-lookup"><span data-stu-id="fac96-121">Prerequisites</span></span>
<span data-ttu-id="fac96-122">De här exemplen fungerar med version 2.3.0 eller senare av hello AzureRm.OperationalInsights modul.</span><span class="sxs-lookup"><span data-stu-id="fac96-122">These examples work with version 2.3.0 or later of hello AzureRm.OperationalInsights module.</span></span>


## <a name="create-and-configure-a-log-analytics-workspace"></a><span data-ttu-id="fac96-123">Skapa och konfigurera en Log Analytics-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="fac96-123">Create and configure a Log Analytics Workspace</span></span>
<span data-ttu-id="fac96-124">hello följande skriptexempel illustrerar hur du:</span><span class="sxs-lookup"><span data-stu-id="fac96-124">hello following script sample illustrates how to:</span></span>

1. <span data-ttu-id="fac96-125">Skapa en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="fac96-125">Create a workspace</span></span>
2. <span data-ttu-id="fac96-126">Lista hello tillgängliga lösningar</span><span class="sxs-lookup"><span data-stu-id="fac96-126">List hello available solutions</span></span>
3. <span data-ttu-id="fac96-127">Lägg till lösningar toohello arbetsytan</span><span class="sxs-lookup"><span data-stu-id="fac96-127">Add solutions toohello workspace</span></span>
4. <span data-ttu-id="fac96-128">Importera sparade sökningar</span><span class="sxs-lookup"><span data-stu-id="fac96-128">Import saved searches</span></span>
5. <span data-ttu-id="fac96-129">Exportera sparade sökningar</span><span class="sxs-lookup"><span data-stu-id="fac96-129">Export saved searches</span></span>
6. <span data-ttu-id="fac96-130">Skapa en datorgrupp</span><span class="sxs-lookup"><span data-stu-id="fac96-130">Create a computer group</span></span>
7. <span data-ttu-id="fac96-131">Aktivera insamling av IIS-loggar från datorer med Windows hello-agenten installerad</span><span class="sxs-lookup"><span data-stu-id="fac96-131">Enable collection of IIS logs from computers with hello Windows agent installed</span></span>
8. <span data-ttu-id="fac96-132">Samla in logisk Disk prestandaräknarna från Linux-datorer (% noder i procent; Ledigt utrymme i MB; Använt utrymme; i % Disköverföringar/sek; Diskläsningar/sek; Diskskrivningar/sek)</span><span class="sxs-lookup"><span data-stu-id="fac96-132">Collect Logical Disk perf counters from Linux computers (% Used Inodes; Free Megabytes; % Used Space; Disk Transfers/sec; Disk Reads/sec; Disk Writes/sec)</span></span>
9. <span data-ttu-id="fac96-133">Samla in syslog-händelser från Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="fac96-133">Collect syslog events from Linux computers</span></span>
10. <span data-ttu-id="fac96-134">Samla in händelser för fel- och varningsmeddelandena från hello programhändelseloggen från Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="fac96-134">Collect Error and Warning events from hello Application Event Log from Windows computers</span></span>
11. <span data-ttu-id="fac96-135">Tillgängligt minne i megabyte prestandaräknaren samla in från Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="fac96-135">Collect Memory Available Mbytes performance counter from Windows computers</span></span>
12. <span data-ttu-id="fac96-136">Samla in en anpassad logg</span><span class="sxs-lookup"><span data-stu-id="fac96-136">Collect a custom log</span></span> 

```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need toobe unique - Get-Random helps with this for hello example code
$Location = "westeurope"

# List of solutions tooenable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches tooimport
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log toocollect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create hello resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create hello workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

#List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group based on a query
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Create a computer group based on names (up too5000)
$computerGroup = """servername1.contoso.com"",""servername2.contoso.com"",""servername3.contoso.com"",""servername4.contoso.com"""
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Named Servers" -DisplayName "Named Servers" -Category "My Saved Searches" -Query $computerGroup -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-tooindex-azure-diagnostics"></a><span data-ttu-id="fac96-137">Konfigurera logganalys tooindex Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="fac96-137">Configuring Log Analytics tooindex Azure diagnostics</span></span>
<span data-ttu-id="fac96-138">Hello resurser måste toohave Azure diagnostics aktiverade och konfigurerade toowrite tooa logganalys-arbetsytan för övervakning utan Agent för resurser i Azure.</span><span class="sxs-lookup"><span data-stu-id="fac96-138">For agentless monitoring of Azure resources, hello resources need toohave Azure diagnostics enabled and configured toowrite tooa Log Analytics workspace.</span></span> <span data-ttu-id="fac96-139">Den här metoden skickar data direkt tooLog Analytics och kräver inte data toobe skrivs tooa storage-konto.</span><span class="sxs-lookup"><span data-stu-id="fac96-139">This approach sends data directly tooLog Analytics and does not require data toobe written tooa storage account.</span></span> <span data-ttu-id="fac96-140">Resurser som stöds är:</span><span class="sxs-lookup"><span data-stu-id="fac96-140">Supported resources include:</span></span>

| <span data-ttu-id="fac96-141">Resurstyp</span><span class="sxs-lookup"><span data-stu-id="fac96-141">Resource Type</span></span> | <span data-ttu-id="fac96-142">Logs</span><span class="sxs-lookup"><span data-stu-id="fac96-142">Logs</span></span> | <span data-ttu-id="fac96-143">Mått</span><span class="sxs-lookup"><span data-stu-id="fac96-143">Metrics</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fac96-144">Application Gateways</span><span class="sxs-lookup"><span data-stu-id="fac96-144">Application Gateways</span></span>    | <span data-ttu-id="fac96-145">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-145">Yes</span></span> | <span data-ttu-id="fac96-146">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-146">Yes</span></span> |
| <span data-ttu-id="fac96-147">Automation-konton</span><span class="sxs-lookup"><span data-stu-id="fac96-147">Automation accounts</span></span>     | <span data-ttu-id="fac96-148">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-148">Yes</span></span> | |
| <span data-ttu-id="fac96-149">Batch-konton</span><span class="sxs-lookup"><span data-stu-id="fac96-149">Batch accounts</span></span>          | <span data-ttu-id="fac96-150">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-150">Yes</span></span> | <span data-ttu-id="fac96-151">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-151">Yes</span></span> |
| <span data-ttu-id="fac96-152">Data Lake analytics</span><span class="sxs-lookup"><span data-stu-id="fac96-152">Data Lake analytics</span></span>     | <span data-ttu-id="fac96-153">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-153">Yes</span></span> | | 
| <span data-ttu-id="fac96-154">Data Lake store</span><span class="sxs-lookup"><span data-stu-id="fac96-154">Data Lake store</span></span>         | <span data-ttu-id="fac96-155">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-155">Yes</span></span> | |
| <span data-ttu-id="fac96-156">Elastisk SQL-poolen</span><span class="sxs-lookup"><span data-stu-id="fac96-156">Elastic SQL Pool</span></span>        |     | <span data-ttu-id="fac96-157">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-157">Yes</span></span> |
| <span data-ttu-id="fac96-158">Event Hub namnområde</span><span class="sxs-lookup"><span data-stu-id="fac96-158">Event Hub namespace</span></span>     |     | <span data-ttu-id="fac96-159">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-159">Yes</span></span> |
| <span data-ttu-id="fac96-160">IoT-hubbar</span><span class="sxs-lookup"><span data-stu-id="fac96-160">IoT Hubs</span></span>                |     | <span data-ttu-id="fac96-161">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-161">Yes</span></span> |
| <span data-ttu-id="fac96-162">Key Vault</span><span class="sxs-lookup"><span data-stu-id="fac96-162">Key Vault</span></span>               | <span data-ttu-id="fac96-163">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-163">Yes</span></span> | |
| <span data-ttu-id="fac96-164">Belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="fac96-164">Load Balancers</span></span>          | <span data-ttu-id="fac96-165">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-165">Yes</span></span> | |
| <span data-ttu-id="fac96-166">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="fac96-166">Logic Apps</span></span>              | <span data-ttu-id="fac96-167">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-167">Yes</span></span> | <span data-ttu-id="fac96-168">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-168">Yes</span></span> |
| <span data-ttu-id="fac96-169">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="fac96-169">Network Security Groups</span></span> | <span data-ttu-id="fac96-170">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-170">Yes</span></span> | |
| <span data-ttu-id="fac96-171">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="fac96-171">Redis Cache</span></span>             |     | <span data-ttu-id="fac96-172">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-172">Yes</span></span> |
| <span data-ttu-id="fac96-173">Söktjänster</span><span class="sxs-lookup"><span data-stu-id="fac96-173">Search services</span></span>         | <span data-ttu-id="fac96-174">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-174">Yes</span></span> | <span data-ttu-id="fac96-175">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-175">Yes</span></span> |
| <span data-ttu-id="fac96-176">Service Bus-namnrymd</span><span class="sxs-lookup"><span data-stu-id="fac96-176">Service Bus namespace</span></span>   |     | <span data-ttu-id="fac96-177">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-177">Yes</span></span> |
| <span data-ttu-id="fac96-178">SQL (v12)</span><span class="sxs-lookup"><span data-stu-id="fac96-178">SQL (v12)</span></span>               |     | <span data-ttu-id="fac96-179">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-179">Yes</span></span> |
| <span data-ttu-id="fac96-180">Webbplatser</span><span class="sxs-lookup"><span data-stu-id="fac96-180">Web Sites</span></span>               |     | <span data-ttu-id="fac96-181">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-181">Yes</span></span> |
| <span data-ttu-id="fac96-182">Webbservergrupper</span><span class="sxs-lookup"><span data-stu-id="fac96-182">Web Server farms</span></span>        |     | <span data-ttu-id="fac96-183">Ja</span><span class="sxs-lookup"><span data-stu-id="fac96-183">Yes</span></span> |

<span data-ttu-id="fac96-184">Hello information av hello tillgängliga mått finns för[stöds mått med Azure-Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="fac96-184">For hello details of hello available metrics, refer too[supported metrics with Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span></span>

<span data-ttu-id="fac96-185">Hello information hello tillgängliga loggar finns för[stöds tjänster och schemat för diagnostikloggar](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).</span><span class="sxs-lookup"><span data-stu-id="fac96-185">For hello details of hello available logs, refer too[supported services and schema for diagnostic logs](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).</span></span>

```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

<span data-ttu-id="fac96-186">Du kan också använda hello föregående cmdlet toocollect loggar från resurser som är i olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="fac96-186">You can also use hello preceding cmdlet toocollect logs from resources that are in different subscriptions.</span></span> <span data-ttu-id="fac96-187">hello-cmdlet är kan toowork över prenumerationer eftersom du tillhandahåller hello-id för båda hello-resurs som skapar loggar och hello arbetsytan hello loggar skickas till.</span><span class="sxs-lookup"><span data-stu-id="fac96-187">hello cmdlet is able toowork across subscriptions since you are providing hello id of both hello resource creating logs and hello workspace hello logs are sent to.</span></span>


## <a name="configuring-log-analytics-tooindex-azure-diagnostics-from-storage"></a><span data-ttu-id="fac96-188">Konfigurera logganalys tooindex Azure diagnostics från lagring</span><span class="sxs-lookup"><span data-stu-id="fac96-188">Configuring Log Analytics tooindex Azure diagnostics from storage</span></span>
<span data-ttu-id="fac96-189">toocollect loggdata från inom en instans av en klassiska molnbaserad tjänst eller ett service fabric-kluster som körs, måste tooAzure för toofirst skrivåtgärder hello datalagring.</span><span class="sxs-lookup"><span data-stu-id="fac96-189">toocollect log data from within a running instance of a classic cloud service or a service fabric cluster, you need toofirst write hello data tooAzure storage.</span></span> <span data-ttu-id="fac96-190">Logganalys är konfigurerad toocollect hello loggar från hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="fac96-190">Log Analytics is then configured toocollect hello logs from hello storage account.</span></span> <span data-ttu-id="fac96-191">Resurser som stöds är:</span><span class="sxs-lookup"><span data-stu-id="fac96-191">Supported resources include:</span></span>

* <span data-ttu-id="fac96-192">Klassiska molntjänster (webb- och arbetsroller roller)</span><span class="sxs-lookup"><span data-stu-id="fac96-192">Classic cloud services (web and worker roles)</span></span>
* <span data-ttu-id="fac96-193">Service fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="fac96-193">Service fabric clusters</span></span>

<span data-ttu-id="fac96-194">följande exempel visar hur hello till:</span><span class="sxs-lookup"><span data-stu-id="fac96-194">hello following example shows how to:</span></span>

1. <span data-ttu-id="fac96-195">Lista över hello befintliga lagringskonton och platser som logganalys indexerar data från</span><span class="sxs-lookup"><span data-stu-id="fac96-195">List hello existing storage accounts and locations that Log Analytics will index data from</span></span>
2. <span data-ttu-id="fac96-196">Skapa en konfiguration tooread från ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="fac96-196">Create a configuration tooread from a storage account</span></span>
3. <span data-ttu-id="fac96-197">Uppdatera hello nyskapad tooindex konfigurationsdata från ytterligare platser</span><span class="sxs-lookup"><span data-stu-id="fac96-197">Update hello newly created configuration tooindex data from additional locations</span></span>
4. <span data-ttu-id="fac96-198">Ta bort hello nyskapad konfiguration</span><span class="sxs-lookup"><span data-stu-id="fac96-198">Delete hello newly created configuration</span></span>

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with hello storage account resource ID and hello storage account key for hello storage account you want tooLog Analytics too 
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles")

# Remove hello insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

<span data-ttu-id="fac96-199">Du kan också använda hello föregående skript toocollect loggar från lagringskonton för olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="fac96-199">You can also use hello preceding script toocollect logs from storage accounts in different subscriptions.</span></span> <span data-ttu-id="fac96-200">hello skript är kan toowork över prenumerationer eftersom du tillhandahåller hello storage-konto resurs-id eller en motsvarande åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="fac96-200">hello script is able toowork across subscriptions since you are providing hello storage account resource id and a corresponding access key.</span></span> <span data-ttu-id="fac96-201">När du ändrar hello åtkomstnyckel måste tooupdate hello lagring insight toohave hello ny nyckel.</span><span class="sxs-lookup"><span data-stu-id="fac96-201">When you change hello access key, you need tooupdate hello storage insight toohave hello new key.</span></span>


## <a name="next-steps"></a><span data-ttu-id="fac96-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fac96-202">Next steps</span></span>
* <span data-ttu-id="fac96-203">[Granska Log Analytics PowerShell-cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) för ytterligare information om hur du använder PowerShell konfigurationen för logganalys.</span><span class="sxs-lookup"><span data-stu-id="fac96-203">[Review Log Analytics PowerShell cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) for additional information on using PowerShell for configuration of Log Analytics.</span></span>

