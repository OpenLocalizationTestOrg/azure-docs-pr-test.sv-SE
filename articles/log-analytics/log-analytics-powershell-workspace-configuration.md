---
title: "Använda PowerShell för att skapa och konfigurera en Log Analytics-arbetsyta | Microsoft Docs"
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
ms.openlocfilehash: 6807ab67e3593da82c147669b29bfdae3b6c967c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-log-analytics-using-powershell"></a><span data-ttu-id="b5ca6-104">Hantera Log Analytics med PowerShell</span><span class="sxs-lookup"><span data-stu-id="b5ca6-104">Manage Log Analytics using PowerShell</span></span>
<span data-ttu-id="b5ca6-105">Du kan använda den [Log Analytics PowerShell-cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) att utföra olika funktioner i logganalys från en kommandorad eller som en del av ett skript.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-105">You can use the [Log Analytics PowerShell cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) to perform various functions in Log Analytics from a command line or as part of a script.</span></span>  <span data-ttu-id="b5ca6-106">Exempel på uppgifter som du kan utföra med PowerShell:</span><span class="sxs-lookup"><span data-stu-id="b5ca6-106">Examples of the tasks you can perform with PowerShell include:</span></span>

* <span data-ttu-id="b5ca6-107">Skapa en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="b5ca6-107">Create a workspace</span></span>
* <span data-ttu-id="b5ca6-108">Lägg till eller ta bort en lösning</span><span class="sxs-lookup"><span data-stu-id="b5ca6-108">Add or remove a solution</span></span>
* <span data-ttu-id="b5ca6-109">Importera och exportera sparade sökningar</span><span class="sxs-lookup"><span data-stu-id="b5ca6-109">Import and export saved searches</span></span>
* <span data-ttu-id="b5ca6-110">Skapa en datorgrupp</span><span class="sxs-lookup"><span data-stu-id="b5ca6-110">Create a computer group</span></span>
* <span data-ttu-id="b5ca6-111">Aktivera insamling av IIS-loggar från datorer med Windows-agenten installerad</span><span class="sxs-lookup"><span data-stu-id="b5ca6-111">Enable collection of IIS logs from computers with the Windows agent installed</span></span>
* <span data-ttu-id="b5ca6-112">Samla in prestandaräknare från Linux och Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="b5ca6-112">Collect performance counters from Linux and Windows computers</span></span>
* <span data-ttu-id="b5ca6-113">Samla in händelser från syslog på Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="b5ca6-113">Collect events from syslog on Linux computers</span></span> 
* <span data-ttu-id="b5ca6-114">Samla in händelser från händelseloggarna i Windows</span><span class="sxs-lookup"><span data-stu-id="b5ca6-114">Collect events from Windows event logs</span></span>
* <span data-ttu-id="b5ca6-115">Samla in anpassade händelseloggar</span><span class="sxs-lookup"><span data-stu-id="b5ca6-115">Collect custom event logs</span></span>
* <span data-ttu-id="b5ca6-116">Lägga till log analytics agenten till en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="b5ca6-116">Add the log analytics agent to an Azure virtual machine</span></span>
* <span data-ttu-id="b5ca6-117">Konfigurera logganalys index data som samlas in med hjälp av Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="b5ca6-117">Configure log analytics to index data collected using Azure diagnostics</span></span>

<span data-ttu-id="b5ca6-118">Den här artikeln innehåller två kodexempel som visar några av de funktioner som du kan utföra från PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-118">This article provides two code samples that illustrate some of the functions that you can perform from PowerShell.</span></span>  <span data-ttu-id="b5ca6-119">Du kan referera till den [Log Analytics PowerShell cmdlet-referens](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) för andra funktioner.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-119">You can refer to the [Log Analytics PowerShell cmdlet reference](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) for other functions.</span></span>

> [!NOTE]
> <span data-ttu-id="b5ca6-120">Logganalys kallades tidigare åtgärdsinformation, därför är det namn som används i cmdlets.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-120">Log Analytics was previously called Operational Insights, which is why it is the name used in the cmdlets.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="b5ca6-121">Krav</span><span class="sxs-lookup"><span data-stu-id="b5ca6-121">Prerequisites</span></span>
<span data-ttu-id="b5ca6-122">De här exemplen fungerar med version 2.3.0 eller senare av modulen AzureRm.OperationalInsights.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-122">These examples work with version 2.3.0 or later of the AzureRm.OperationalInsights module.</span></span>


## <a name="create-and-configure-a-log-analytics-workspace"></a><span data-ttu-id="b5ca6-123">Skapa och konfigurera en Log Analytics-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="b5ca6-123">Create and configure a Log Analytics Workspace</span></span>
<span data-ttu-id="b5ca6-124">Följande skriptexempel illustrerar hur du:</span><span class="sxs-lookup"><span data-stu-id="b5ca6-124">The following script sample illustrates how to:</span></span>

1. <span data-ttu-id="b5ca6-125">Skapa en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="b5ca6-125">Create a workspace</span></span>
2. <span data-ttu-id="b5ca6-126">Visa en lista över tillgängliga lösningar</span><span class="sxs-lookup"><span data-stu-id="b5ca6-126">List the available solutions</span></span>
3. <span data-ttu-id="b5ca6-127">Lägg till lösningar på arbetsytan</span><span class="sxs-lookup"><span data-stu-id="b5ca6-127">Add solutions to the workspace</span></span>
4. <span data-ttu-id="b5ca6-128">Importera sparade sökningar</span><span class="sxs-lookup"><span data-stu-id="b5ca6-128">Import saved searches</span></span>
5. <span data-ttu-id="b5ca6-129">Exportera sparade sökningar</span><span class="sxs-lookup"><span data-stu-id="b5ca6-129">Export saved searches</span></span>
6. <span data-ttu-id="b5ca6-130">Skapa en datorgrupp</span><span class="sxs-lookup"><span data-stu-id="b5ca6-130">Create a computer group</span></span>
7. <span data-ttu-id="b5ca6-131">Aktivera insamling av IIS-loggar från datorer med Windows-agenten installerad</span><span class="sxs-lookup"><span data-stu-id="b5ca6-131">Enable collection of IIS logs from computers with the Windows agent installed</span></span>
8. <span data-ttu-id="b5ca6-132">Samla in logisk Disk prestandaräknarna från Linux-datorer (% noder i procent; Ledigt utrymme i MB; Använt utrymme; i % Disköverföringar/sek; Diskläsningar/sek; Diskskrivningar/sek)</span><span class="sxs-lookup"><span data-stu-id="b5ca6-132">Collect Logical Disk perf counters from Linux computers (% Used Inodes; Free Megabytes; % Used Space; Disk Transfers/sec; Disk Reads/sec; Disk Writes/sec)</span></span>
9. <span data-ttu-id="b5ca6-133">Samla in syslog-händelser från Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="b5ca6-133">Collect syslog events from Linux computers</span></span>
10. <span data-ttu-id="b5ca6-134">Samla in händelser för fel- och varningsmeddelandena i programmets händelselogg från Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="b5ca6-134">Collect Error and Warning events from the Application Event Log from Windows computers</span></span>
11. <span data-ttu-id="b5ca6-135">Tillgängligt minne i megabyte prestandaräknaren samla in från Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="b5ca6-135">Collect Memory Available Mbytes performance counter from Windows computers</span></span>
12. <span data-ttu-id="b5ca6-136">Samla in en anpassad logg</span><span class="sxs-lookup"><span data-stu-id="b5ca6-136">Collect a custom log</span></span> 

```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
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

# Custom Log to collect
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

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
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

# Create a computer group based on names (up to 5000)
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

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a><span data-ttu-id="b5ca6-137">Konfigurera logganalys att indexera Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="b5ca6-137">Configuring Log Analytics to index Azure diagnostics</span></span>
<span data-ttu-id="b5ca6-138">Resurser behöver ha Azure diagnostics aktiverad och konfigurerad för att skriva till logganalys-arbetsytan för övervakning utan Agent för resurser i Azure.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-138">For agentless monitoring of Azure resources, the resources need to have Azure diagnostics enabled and configured to write to a Log Analytics workspace.</span></span> <span data-ttu-id="b5ca6-139">Den här metoden skickar data direkt till Log Analytics och kräver inte data skrivs till ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-139">This approach sends data directly to Log Analytics and does not require data to be written to a storage account.</span></span> <span data-ttu-id="b5ca6-140">Resurser som stöds är:</span><span class="sxs-lookup"><span data-stu-id="b5ca6-140">Supported resources include:</span></span>

| <span data-ttu-id="b5ca6-141">Resurstyp</span><span class="sxs-lookup"><span data-stu-id="b5ca6-141">Resource Type</span></span> | <span data-ttu-id="b5ca6-142">Logs</span><span class="sxs-lookup"><span data-stu-id="b5ca6-142">Logs</span></span> | <span data-ttu-id="b5ca6-143">Mått</span><span class="sxs-lookup"><span data-stu-id="b5ca6-143">Metrics</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b5ca6-144">Application Gateways</span><span class="sxs-lookup"><span data-stu-id="b5ca6-144">Application Gateways</span></span>    | <span data-ttu-id="b5ca6-145">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-145">Yes</span></span> | <span data-ttu-id="b5ca6-146">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-146">Yes</span></span> |
| <span data-ttu-id="b5ca6-147">Automation-konton</span><span class="sxs-lookup"><span data-stu-id="b5ca6-147">Automation accounts</span></span>     | <span data-ttu-id="b5ca6-148">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-148">Yes</span></span> | |
| <span data-ttu-id="b5ca6-149">Batch-konton</span><span class="sxs-lookup"><span data-stu-id="b5ca6-149">Batch accounts</span></span>          | <span data-ttu-id="b5ca6-150">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-150">Yes</span></span> | <span data-ttu-id="b5ca6-151">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-151">Yes</span></span> |
| <span data-ttu-id="b5ca6-152">Data Lake analytics</span><span class="sxs-lookup"><span data-stu-id="b5ca6-152">Data Lake analytics</span></span>     | <span data-ttu-id="b5ca6-153">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-153">Yes</span></span> | | 
| <span data-ttu-id="b5ca6-154">Data Lake store</span><span class="sxs-lookup"><span data-stu-id="b5ca6-154">Data Lake store</span></span>         | <span data-ttu-id="b5ca6-155">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-155">Yes</span></span> | |
| <span data-ttu-id="b5ca6-156">Elastisk SQL-poolen</span><span class="sxs-lookup"><span data-stu-id="b5ca6-156">Elastic SQL Pool</span></span>        |     | <span data-ttu-id="b5ca6-157">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-157">Yes</span></span> |
| <span data-ttu-id="b5ca6-158">Event Hub namnområde</span><span class="sxs-lookup"><span data-stu-id="b5ca6-158">Event Hub namespace</span></span>     |     | <span data-ttu-id="b5ca6-159">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-159">Yes</span></span> |
| <span data-ttu-id="b5ca6-160">IoT-hubbar</span><span class="sxs-lookup"><span data-stu-id="b5ca6-160">IoT Hubs</span></span>                |     | <span data-ttu-id="b5ca6-161">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-161">Yes</span></span> |
| <span data-ttu-id="b5ca6-162">Key Vault</span><span class="sxs-lookup"><span data-stu-id="b5ca6-162">Key Vault</span></span>               | <span data-ttu-id="b5ca6-163">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-163">Yes</span></span> | |
| <span data-ttu-id="b5ca6-164">Belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="b5ca6-164">Load Balancers</span></span>          | <span data-ttu-id="b5ca6-165">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-165">Yes</span></span> | |
| <span data-ttu-id="b5ca6-166">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="b5ca6-166">Logic Apps</span></span>              | <span data-ttu-id="b5ca6-167">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-167">Yes</span></span> | <span data-ttu-id="b5ca6-168">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-168">Yes</span></span> |
| <span data-ttu-id="b5ca6-169">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="b5ca6-169">Network Security Groups</span></span> | <span data-ttu-id="b5ca6-170">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-170">Yes</span></span> | |
| <span data-ttu-id="b5ca6-171">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="b5ca6-171">Redis Cache</span></span>             |     | <span data-ttu-id="b5ca6-172">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-172">Yes</span></span> |
| <span data-ttu-id="b5ca6-173">Söktjänster</span><span class="sxs-lookup"><span data-stu-id="b5ca6-173">Search services</span></span>         | <span data-ttu-id="b5ca6-174">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-174">Yes</span></span> | <span data-ttu-id="b5ca6-175">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-175">Yes</span></span> |
| <span data-ttu-id="b5ca6-176">Service Bus-namnrymd</span><span class="sxs-lookup"><span data-stu-id="b5ca6-176">Service Bus namespace</span></span>   |     | <span data-ttu-id="b5ca6-177">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-177">Yes</span></span> |
| <span data-ttu-id="b5ca6-178">SQL (v12)</span><span class="sxs-lookup"><span data-stu-id="b5ca6-178">SQL (v12)</span></span>               |     | <span data-ttu-id="b5ca6-179">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-179">Yes</span></span> |
| <span data-ttu-id="b5ca6-180">Webbplatser</span><span class="sxs-lookup"><span data-stu-id="b5ca6-180">Web Sites</span></span>               |     | <span data-ttu-id="b5ca6-181">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-181">Yes</span></span> |
| <span data-ttu-id="b5ca6-182">Webbservergrupper</span><span class="sxs-lookup"><span data-stu-id="b5ca6-182">Web Server farms</span></span>        |     | <span data-ttu-id="b5ca6-183">Ja</span><span class="sxs-lookup"><span data-stu-id="b5ca6-183">Yes</span></span> |

<span data-ttu-id="b5ca6-184">Mer information om tillgänglig statistik avser [stöds mått med Azure-Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="b5ca6-184">For the details of the available metrics, refer to [supported metrics with Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span></span>

<span data-ttu-id="b5ca6-185">Mer information om de tillgängliga loggarna avser [stöds tjänster och schemat för diagnostikloggar](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).</span><span class="sxs-lookup"><span data-stu-id="b5ca6-185">For the details of the available logs, refer to [supported services and schema for diagnostic logs](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).</span></span>

```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

<span data-ttu-id="b5ca6-186">Du kan också använda cmdlet föregående för att samla in loggar från resurser som är i olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-186">You can also use the preceding cmdlet to collect logs from resources that are in different subscriptions.</span></span> <span data-ttu-id="b5ca6-187">Cmdlet kan fungera över prenumerationer eftersom du tillhandahåller id för både den resurs som skapar loggar och loggar som skickas till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-187">The cmdlet is able to work across subscriptions since you are providing the id of both the resource creating logs and the workspace the logs are sent to.</span></span>


## <a name="configuring-log-analytics-to-index-azure-diagnostics-from-storage"></a><span data-ttu-id="b5ca6-188">Konfigurera logganalys att indexera Azure diagnostics från lagring</span><span class="sxs-lookup"><span data-stu-id="b5ca6-188">Configuring Log Analytics to index Azure diagnostics from storage</span></span>
<span data-ttu-id="b5ca6-189">Du måste först skriva data till Azure-lagring för att samla in loggdata från en instans av en klassiska molnbaserad tjänst eller ett service fabric-kluster som körs.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-189">To collect log data from within a running instance of a classic cloud service or a service fabric cluster, you need to first write the data to Azure storage.</span></span> <span data-ttu-id="b5ca6-190">Logganalys konfigureras sedan för att samla in loggar från lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-190">Log Analytics is then configured to collect the logs from the storage account.</span></span> <span data-ttu-id="b5ca6-191">Resurser som stöds är:</span><span class="sxs-lookup"><span data-stu-id="b5ca6-191">Supported resources include:</span></span>

* <span data-ttu-id="b5ca6-192">Klassiska molntjänster (webb- och arbetsroller roller)</span><span class="sxs-lookup"><span data-stu-id="b5ca6-192">Classic cloud services (web and worker roles)</span></span>
* <span data-ttu-id="b5ca6-193">Service fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="b5ca6-193">Service fabric clusters</span></span>

<span data-ttu-id="b5ca6-194">Följande exempel visar hur du:</span><span class="sxs-lookup"><span data-stu-id="b5ca6-194">The following example shows how to:</span></span>

1. <span data-ttu-id="b5ca6-195">Visa en lista över de befintliga lagringskonton och platser som logganalys indexerar data från</span><span class="sxs-lookup"><span data-stu-id="b5ca6-195">List the existing storage accounts and locations that Log Analytics will index data from</span></span>
2. <span data-ttu-id="b5ca6-196">Skapa en konfiguration för att läsa från ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="b5ca6-196">Create a configuration to read from a storage account</span></span>
3. <span data-ttu-id="b5ca6-197">Uppdatera konfigurationen för nyskapade att indexera data från fler platser</span><span class="sxs-lookup"><span data-stu-id="b5ca6-197">Update the newly created configuration to index data from additional locations</span></span>
4. <span data-ttu-id="b5ca6-198">Ta bort den nyligen skapade konfigurationen</span><span class="sxs-lookup"><span data-stu-id="b5ca6-198">Delete the newly created configuration</span></span>

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

<span data-ttu-id="b5ca6-199">Du kan också använda skriptet för att samla in loggar från lagringskonton för olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-199">You can also use the preceding script to collect logs from storage accounts in different subscriptions.</span></span> <span data-ttu-id="b5ca6-200">Skriptet kan fungera över prenumerationer eftersom du tillhandahåller lagring konto resurs-id och en motsvarande snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-200">The script is able to work across subscriptions since you are providing the storage account resource id and a corresponding access key.</span></span> <span data-ttu-id="b5ca6-201">När du ändrar snabbtangent som du behöver uppdatera lagring kunskaper om du vill att den nya nyckeln.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-201">When you change the access key, you need to update the storage insight to have the new key.</span></span>


## <a name="next-steps"></a><span data-ttu-id="b5ca6-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b5ca6-202">Next steps</span></span>
* <span data-ttu-id="b5ca6-203">[Granska Log Analytics PowerShell-cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) för ytterligare information om hur du använder PowerShell konfigurationen för logganalys.</span><span class="sxs-lookup"><span data-stu-id="b5ca6-203">[Review Log Analytics PowerShell cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) for additional information on using PowerShell for configuration of Log Analytics.</span></span>

