---
title: "Utvärdera Service Fabric-program med Azure Log Analytics med hjälp av PowerShell | Microsoft Docs"
description: "Du kan använda Service Fabric-lösning i logganalys som använder PowerShell för att bedöma risken och hälsotillståndet för Service Fabric program, micro-tjänster, noder och kluster."
services: log-analytics
documentationcenter: 
author: niniikhena
manager: jochan
editor: 
ms.assetid: 2047b3fa-96b1-4230-af5d-a4c331d973ce
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: nini
ms.openlocfilehash: ca86787e344aa5e9e68934dee6e9e83aeb4cc340
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="assess-azure-service-fabric-applications-and-micro-services-with-powershell"></a><span data-ttu-id="6db37-103">Utvärdera Azure Service Fabric-program och micro-tjänster med PowerShell</span><span class="sxs-lookup"><span data-stu-id="6db37-103">Assess Azure Service Fabric applications and micro-services with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6db37-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6db37-104">Resource Manager</span></span>](log-analytics-service-fabric-azure-resource-manager.md)
> * [<span data-ttu-id="6db37-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6db37-105">PowerShell</span></span>](log-analytics-service-fabric.md)
>
>


![Service Fabric-symbolen](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

<span data-ttu-id="6db37-107">Den här artikeln beskriver hur du använder Service Fabric-lösning i Log Analytics för att identifiera och felsöka problem i Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="6db37-107">This article describes how to use the Service Fabric solution in Log Analytics to help identify and troubleshoot issues across your Service Fabric cluster.</span></span> <span data-ttu-id="6db37-108">Det kan du se hur utför Service Fabric-noder och hur dina program och tjänster micro körs.</span><span class="sxs-lookup"><span data-stu-id="6db37-108">It helps you see how your Service Fabric nodes are performing and how your applications and micro-services are running.</span></span>

<span data-ttu-id="6db37-109">Service Fabric-lösningen använder Azure-diagnostik data från din Service Fabric virtuella datorer genom att samla in data från Azure BOMULLSTUSS tabellerna.</span><span class="sxs-lookup"><span data-stu-id="6db37-109">The Service Fabric solution uses Azure Diagnostics data from your Service Fabric VMs, by collecting this data from your Azure WAD tables.</span></span> <span data-ttu-id="6db37-110">Logganalys läser sedan följande Service Fabric framework händelser:</span><span class="sxs-lookup"><span data-stu-id="6db37-110">Log Analytics then reads the following Service Fabric framework events:</span></span>

- <span data-ttu-id="6db37-111">**Händelser för tillförlitlig tjänst**</span><span class="sxs-lookup"><span data-stu-id="6db37-111">**Reliable Service Events**</span></span>
- <span data-ttu-id="6db37-112">**Aktören händelser**</span><span class="sxs-lookup"><span data-stu-id="6db37-112">**Actor Events**</span></span>
- <span data-ttu-id="6db37-113">**Operativa händelser**</span><span class="sxs-lookup"><span data-stu-id="6db37-113">**Operational Events**</span></span>
- <span data-ttu-id="6db37-114">**Anpassade ETW-händelser**</span><span class="sxs-lookup"><span data-stu-id="6db37-114">**Custom ETW events**</span></span>

<span data-ttu-id="6db37-115">Service Fabric-lösningen instrumentpanelen visar viktiga problem och relevanta händelser i Service Fabric-miljö.</span><span class="sxs-lookup"><span data-stu-id="6db37-115">The Service Fabric solution dashboard shows you notable issues and relevant events in your Service Fabric environment.</span></span>

## <a name="installing-and-configuring-the-solution"></a><span data-ttu-id="6db37-116">Installera och konfigurera lösningen</span><span class="sxs-lookup"><span data-stu-id="6db37-116">Installing and configuring the solution</span></span>
<span data-ttu-id="6db37-117">Följ dessa tre enkla steg för att installera och konfigurera lösningen:</span><span class="sxs-lookup"><span data-stu-id="6db37-117">Follow these three easy steps to install and configure the solution:</span></span>

1. <span data-ttu-id="6db37-118">Koppla Azure-prenumeration som du använde för att skapa alla resurser i klustret, inklusive lagringskonton med din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="6db37-118">Associate the Azure subscription that you used to create all cluster resources, including storage accounts, with your workspace.</span></span> <span data-ttu-id="6db37-119">Se [Kom igång med logganalys](log-analytics-get-started.md) information om hur du skapar en logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="6db37-119">See [Get started with Log Analytics](log-analytics-get-started.md) for information about creating a Log Analytics workspace.</span></span>
2. <span data-ttu-id="6db37-120">Konfigurera Log Analytics för att samla in och visa loggar för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6db37-120">Configure Log Analytics to collect and view Service Fabric logs.</span></span>
3. <span data-ttu-id="6db37-121">Aktivera Service Fabric-lösningen i din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="6db37-121">Enable the Service Fabric solution in your workspace.</span></span>

## <a name="configure-log-analytics-to-collect-and-view-service-fabric-logs"></a><span data-ttu-id="6db37-122">Konfigurera Log Analytics för att samla in och visa loggar för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6db37-122">Configure Log Analytics to collect and view Service Fabric logs</span></span>
<span data-ttu-id="6db37-123">Lär dig hur du konfigurerar Log Analytics för att hämta Service Fabric-loggar i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6db37-123">In this section, you learn how to configure Log Analytics to retrieve Service Fabric logs.</span></span> <span data-ttu-id="6db37-124">Loggarna kan du visa, analysera och felsöka problem i ditt kluster eller i program och tjänster som körs i klustret, med hjälp av OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="6db37-124">The logs allow you to view, analyze, and troubleshoot issues in your cluster or in the applications and services running in that cluster, using the OMS portal.</span></span>

> [!NOTE]
> <span data-ttu-id="6db37-125">Konfigurera Azure-diagnostik-tillägget för att ladda upp loggar för storage-tabeller.</span><span class="sxs-lookup"><span data-stu-id="6db37-125">Configure the Azure Diagnostics extension to upload the logs for storage tables.</span></span> <span data-ttu-id="6db37-126">Tabellerna måste matcha ser Log Analytics för.</span><span class="sxs-lookup"><span data-stu-id="6db37-126">The tables must match what Log Analytics looks for.</span></span> <span data-ttu-id="6db37-127">Mer information finns i [hur du samlar in loggar med Azure-diagnostik](../service-fabric/service-fabric-diagnostics-how-to-setup-wad.md).</span><span class="sxs-lookup"><span data-stu-id="6db37-127">For more information, see [How to collect logs with Azure Diagnostics](../service-fabric/service-fabric-diagnostics-how-to-setup-wad.md).</span></span> <span data-ttu-id="6db37-128">Konfiguration av inställningar exemplen i den här artikeln visar bör du vilka namnen på storage-tabeller.</span><span class="sxs-lookup"><span data-stu-id="6db37-128">The configuration settings examples in this article show you what the names of the storage tables should be.</span></span> <span data-ttu-id="6db37-129">När diagnostik ställs in på klustret och laddar upp loggar till ett lagringskonto, är nästa steg att konfigurera Log Analytics för att samla in dessa loggar.</span><span class="sxs-lookup"><span data-stu-id="6db37-129">Once Diagnostics is set up on the cluster and is uploading logs to a storage account, the next step is to configure Log Analytics to collect these logs.</span></span>
>
>

<span data-ttu-id="6db37-130">Se till att du uppdaterar den **EtwEventSourceProviderConfiguration** under den **template.json** fil att lägga till poster för de nya EventSources innan du använder konfigurationen uppdatera genom att köra **deploy.ps1**.</span><span class="sxs-lookup"><span data-stu-id="6db37-130">Ensure that you update the **EtwEventSourceProviderConfiguration** section in the **template.json** file to add entries for the new EventSources before you apply the configuration update by running **deploy.ps1**.</span></span> <span data-ttu-id="6db37-131">Tabellen för överföring är samma som (ETWEventTable).</span><span class="sxs-lookup"><span data-stu-id="6db37-131">The table for upload is the same as (ETWEventTable).</span></span> <span data-ttu-id="6db37-132">För tillfället logganalys endast läsa programmet ETW-händelser från den *WADETWEventTable* tabell.</span><span class="sxs-lookup"><span data-stu-id="6db37-132">At the moment, Log Analytics can only read application ETW events from the *WADETWEventTable* table.</span></span>

<span data-ttu-id="6db37-133">Följande verktyg används för att utföra vissa åtgärder i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="6db37-133">The following tools are used to perform some of the operations in this section:</span></span>

* <span data-ttu-id="6db37-134">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6db37-134">Azure PowerShell</span></span>
* [<span data-ttu-id="6db37-135">Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="6db37-135">Operations Management Suite</span></span>](http://www.microsoft.com/oms)

### <a name="configure-a-log-analytics-workspace-to-show-the-cluster-logs"></a><span data-ttu-id="6db37-136">Konfigurera en logganalys-arbetsyta om du vill visa loggarna kluster</span><span class="sxs-lookup"><span data-stu-id="6db37-136">Configure a Log Analytics workspace to show the cluster logs</span></span>

<span data-ttu-id="6db37-137">Konfigurera arbetsytan om du vill dra loggar från Azure storage-tabeller när du har skapat en logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="6db37-137">After you create a Log Analytics workspace, configure the workspace to pull logs from the Azure storage tables.</span></span> <span data-ttu-id="6db37-138">Kör följande PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="6db37-138">Then, run the following PowerShell script:</span></span>

```
<#
    This script will configure an Operations Management Suite workspace (previously called an Operational Insights workspace) to read Diagnostics from an Azure Storage account.
    It will enable all supported data types (currently Service Fabric Events, ETW Events and IIS Logs).
    It supports Resource Manager storage accounts.
    If you have more than one Azure Subscription, you will be prompted for the subscription to configure.
    If you have more than one Log Analytics workspace you will be prompted for the workspace to configure.
    It will then look through your Service Fabric clusters, and configure your Log Analytics workspace to read Diagnostics from storage accounts that are connected to that cluster and have diagnostics enabled.
#>

try
{
    Get-AzureRMContext
}
catch [System.Management.Automation.PSInvalidOperationException]
{
    Add-AzureRmAccount
}

$validTables = "WADServiceFabric*EventTable", "WADETWEventTable"
function Select-Subscription {
      $subscription = ""
      $allSubscriptions = Get-AzureRmSubscription
      switch ($allSubscriptions.Count) {
             0 {Write-Error "No Operations Management Suite workspaces found"}
             1 {return $allSubscriptions}
        default {
            $uiPrompt = "Enter the number corresponding to the Azure subscription you would like to work with.`n"

            $count = 1
            foreach ($subscription in $allSubscriptions) {
                $uiPrompt += "$count. " + $subscription.Name + " (" + $subscription.Id + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $subscription = $allSubscriptions[$answer]
             Write-Host $subscription.Id
        }  
    }
    return $subscription
}

function Select-Workspace {
    $workspace = ""
    $allWorkspaces = Get-AzureRmOperationalInsightsWorkspace  

    switch ($allWorkspaces.Count) {
        0 {Write-Error "No Operations Management Suite workspaces found. `n"}
        1 {return $allWorkspaces}
        default {
            $uiPrompt = "Enter the number corresponding to the workspace you want to configure.`n"
            $count = 1
            foreach ($workspace in $allWorkspaces) {
                $uiPrompt += "$count. " + $workspace.Name + " (" + $workspace.CustomerId + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $workspace = $allWorkspaces[$answer]
             Write-Host $workspace.WorkspaceName
        }  
    }
    return $workspace
}

function Check-ETWProviderLogging {
     param(
     [string]$id,
     [string]$provider,
     [string]$expectedTable,
     [string]$table
    )       
         Write-Debug ("ID: $id Provider: $provider ExpectedTable $expectedTable ActualTable $table")
         if ( ($table -eq $null) -or ($table -eq ""))  
         {
             Write-Warning ("$id No configuration found for $provider. Configure Azure diagnostics to write to $expectedTable.")
         }  
         elseif ( $table -ne $expectedTable )
         {
             Write-Warning ("$id $provider events are being written to $table instead of WAD$expectedTable. Events will not be collected by Log Analytics")
         }  
         else
         {
             Write-Verbose "$id $provider events are being written to WAD$expectedTable (Correct configuration.)"
         }
 }

function Check-ServiceFabricScaleSetDiagnostics {
     param(
          [psobject]$scaleSetDiagnostics
   )
     $storageAccountsFound = @()
     Write-Verbose ("Checking " + $scaleSetDiagnostics)
     $sfReliableActorTable = $null
     $sfReliableServiceTable = $null
     $sfOperationalTable = $null

     Write-Debug $scaleSetDiagnostics
     $serviceFabricProviderList = ""
     $etwManifestProviderList = ""

     if ( $scaleSetDiagnostics.xmlCfg )  
      {
             Write-Debug ("Found XMLcfg")
             $xmlCfg = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($scaleSetDiagnostics.xmlCfg))
             Write-Debug $xmlCfg
             $etwProviders = Select-Xml -Content $xmlCfg -XPath "//EtwProviders"                 
             $serviceFabricProviderList = $etwProviders.Node.EtwEventSourceProviderConfiguration
             $etwManifestProviderList = $etwProviders.Node.EtwManifestProviderConfiguration
      } elseif ($scaleSetDiagnostics.WadCfg )  
     {
         Write-Debug ("Found WADcfg")
         Write-Debug $scaleSetDiagnostics.WadCfg
         $serviceFabricProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwEventSourceProviderConfiguration
         $etwManifestProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwManifestProviderConfiguration
     } else
     {
         Write-Error "Unable to parse Azure Diagnostics setting for $id"
             Write-Warning ("$id does not have diagnostics enabled")
     }
     foreach ($provider in $serviceFabricProviderList)  
     {
         Write-Debug ("Event Source Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
         if ($provider.Provider -eq "Microsoft-ServiceFabric-Actors")
         {
             $sfReliableActorTable = $provider.DefaultEvents.eventDestination  
         } elseif ($provider.Provider -eq "Microsoft-ServiceFabric-Services")  
         {  
             $sfReliableServiceTable = $provider.DefaultEvents.eventDestination  
         } else  
         {
             Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
         }
     }
     foreach ($provider in $etwManifestProviderList)
     {
         Write-Debug ("Manifest Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
         if ($provider.Provider -eq "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8")
         {
             $sfOperationalTable = $provider.DefaultEvents.eventDestination  
         } else  
         {
             Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
         }
     }

     Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Actors" "ServiceFabricReliableActorEventTable" $sfReliableActorTable
     Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Services" "ServiceFabricReliableServiceEventTable" $sfReliableServiceTable
     Check-ETWProviderLogging $id "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8 (System events)" "ServiceFabricSystemEventTable" $sfOperationalTable

     Write-Verbose ("StorageAccount: " + $scaleSetDiagnostics.StorageAccount)
     $storageAccountsFound += ($scaleSetDiagnostics.StorageAccount)
     return ($storageAccountsFound)
 }

function Select-StorageAccount {
    $allResources = Get-AzureRmResource #pulls in all resources
    $serviceFabricClusters = $allResources.Where({$_.ResourceType -eq "Microsoft.ServiceFabric/clusters"}) #pulls in all service fabric clusters in the resource
    $storageAccountList = @()
    foreach($cluster in $serviceFabricClusters) {
        Write-Host("Checking cluster: " + $cluster.Name)
         $scaleSet = $allResources.Where({($_.ResourceType -eq "Microsoft.Compute/virtualMachineScaleSets") -and ($_.ResourceGroupName -eq $cluster.ResourceGroupName)})

         foreach($set in $scaleSet) {
             $resource = Get-AzureRmResource -ResourceId $set.ResourceId
             $extensions = $resource.Properties.VirtualMachineProfile.ExtensionProfile.Extensions

             foreach($ext in $extensions) {
                 if ($ext.Properties.Publisher -eq "Microsoft.Azure.Diagnostics" -and $ext.Properties.Type -eq "IaaSDiagnostics") {
                     $storageAccountList += (Check-ServiceFabricScaleSetDiagnostics $ext.Properties.Settings)
                 }
             }
          }

         $storageAccountsToCheck = $allResources.Where({($_.ResourceType -eq "Microsoft.Storage/storageAccounts") -and ($_.ResourceName -in $storageAccountList)})

         if ($storageAccountsToCheck.Count -eq "0") {
                Write-Error "No storage accounts found"
           }
           else {
                    foreach ($storageAccount in $storageAccountsToCheck) {
                        Write-Host("Checking Storage Account: " + $storageAccount.Name)
                        $insightsName = $storageAccount.Name + $workspace.Name
                        $existingConfig = ""
                        try
                            {
                                $existingConfig = Get-AzureRmOperationalInsightsStorageInsight -Workspace $workspace -Name $insightsName -ErrorAction Stop
                            }
                        catch
                            {
                                # HTTP Not Found is returned if the storage insight doesn't exist
                            }
                        if ($existingConfig) {                         
                                  [array]$Tables = $existingConfig.Tables
                                   foreach($table in $validTables) {
                                         if($Tables -notcontains $table) {
                                               $Tables += $table
                                               $dirty = $true;
                                               Write-Host "Adding Table: $table";
                                         }
                                         else {
                                               Write-Host "$table is already configured.`n";
                                             }
                                      }
                                      # If any of the tables from the table list are not already monitored, then we add them
                                   if($dirty -eq $true) {
                                           Set-AzureRmOperationalInsightsStorageInsight -Workspace $workspace -Name $insightsName -Tables $Tables
                                           Write-Host "Updating Storage Insight. `n"
                                    }
                                    else {
                                           Write-Host "Storage Insight already updated."
                                  }
                          }
                     else {
                            $key = (Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.Name)[0].Value
                           New-AzureRmOperationalInsightsStorageInsight -Workspace $workspace -Name $insightsName -StorageAccountResourceId $storageAccount.ResourceId -StorageAccountKey $key -Tables $validTables
                            Write-Host "New Azure Storage Insight Configured. `n"
                           }
                    }
             }
      }
      return
     }

$subscription = Select-Subscription
$subscriptionId = $subscription.SubscriptionId
$subscription = Select-AzureRmSubscription -SubscriptionId $subscriptionId
$workspace = Select-Workspace
$storageAccount = Select-StorageAccount
```

<span data-ttu-id="6db37-139">När du har konfigurerat logganalys-arbetsytan att läsa från Azure-tabeller i storage-konto kan logga in på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6db37-139">After you've configured the Log Analytics workspace to read from the Azure tables in your storage account, log in to the Azure portal.</span></span> <span data-ttu-id="6db37-140">Välj logganalys-arbetsyta från **alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="6db37-140">Select the Log Analytics workspace from **All Resources**.</span></span> <span data-ttu-id="6db37-141">Antalet lagring konto loggar som är ansluten till arbetsytan visas.</span><span class="sxs-lookup"><span data-stu-id="6db37-141">The number of storage account logs connected to the workspace is displayed.</span></span> <span data-ttu-id="6db37-142">Välj den **Storage-konto loggar** panelen.</span><span class="sxs-lookup"><span data-stu-id="6db37-142">Select the **Storage account logs** tile.</span></span> <span data-ttu-id="6db37-143">Granska listan med lagring konto loggar för att kontrollera att ditt lagringskonto är ansluten till rätt arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="6db37-143">Review the list of storage account logs to verify that your storage account is connected to the correct workspace.</span></span>

![Storage-konto loggar](./media/log-analytics-service-fabric/sf1.png)

## <a name="enable-the-service-fabric-solution"></a><span data-ttu-id="6db37-145">Aktivera Service Fabric-lösning</span><span class="sxs-lookup"><span data-stu-id="6db37-145">Enable the Service Fabric solution</span></span>
<span data-ttu-id="6db37-146">Använd följande skript för att lägga till lösningen logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="6db37-146">Use the following script to add the solution to your Log Analytics workspace.</span></span> <span data-ttu-id="6db37-147">Kör skriptet i PowerShell med hjälp av Azure-prenumerationen som är kopplad till logganalys-arbetsytan som du vill aktivera Service Fabric-lösningen i.</span><span class="sxs-lookup"><span data-stu-id="6db37-147">Run the script in PowerShell, using the Azure subscription that is associated with the Log Analytics workspace that you want to enable the Service Fabric solution in.</span></span>

```
function Select-Subscription {
      $subscription = ""
      $allSubscriptions = Get-AzureRmSubscription
      switch ($allSubscriptions.Count) {
             0 {Write-Error "No Operations Management Suite workspaces found"}
             1 {return $allSubscriptions}
        default {
            $uiPrompt = "Enter the number corresponding to the Azure subscription you would like to work with.`n"
            $count = 1
            foreach ($subscription in $allSubscriptions) {
                $uiPrompt += "$count. " + $subscription.SubscriptionName + " (" + $subscription.SubscriptionId + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $subscription = $allSubscriptions[$answer]
             Write-Host $subscription.SubscriptionId
        }  
    }
    return $subscription
}

function Select-Workspace {
    $workspace = ""
    $allWorkspaces = Get-AzureRmOperationalInsightsWorkspace  
    switch ($allWorkspaces.Count) {
        0 {Write-Error "No Operations Management Suite workspaces found"}
        1 {return $allWorkspaces}
        default {
            $uiPrompt = "Enter the number corresponding to the workspace you want to configure.`n"
            $count = 1
            foreach ($workspace in $allWorkspaces) {
                $uiPrompt += "$count. " + $workspace.Name + " (" + $workspace.CustomerId + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $workspace = $allWorkspaces[$answer]
                           Write-Host $workspace.WorkspaceName
        }  
    }
    return $workspace
}
$subscription = Select-Subscription
$subscriptionId = $subscription.Id
$subscription = Select-AzureRmSubscription -SubscriptionId $subscriptionId
$workspace = Select-Workspace
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -IntelligencePackName "ServiceFabric" -Enabled $true
```

<span data-ttu-id="6db37-148">När du aktiverar lösningen, Service Fabric-panel har lagts till Log Analytics *översikt* sidan.</span><span class="sxs-lookup"><span data-stu-id="6db37-148">After you enable the solution, the Service Fabric tile is added to your Log Analytics *Overview* page.</span></span> <span data-ttu-id="6db37-149">På sidan visas en vy över anmärkningsvärda problem, till exempel runAsync fel och avbryta som inträffade under de senaste 24 timmarna.</span><span class="sxs-lookup"><span data-stu-id="6db37-149">The page shows a view of notable issues such as runAsync failures and cancellations that occurred in the last 24 hours.</span></span>

![Service Fabric-panelen](./media/log-analytics-service-fabric/sf2.png)

### <a name="view-service-fabric-events"></a><span data-ttu-id="6db37-151">Visa händelser för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6db37-151">View Service Fabric events</span></span>
<span data-ttu-id="6db37-152">Klicka på den **Service Fabric** öppna Service Fabric-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="6db37-152">Click the **Service Fabric** tile to open the Service Fabric dashboard.</span></span> <span data-ttu-id="6db37-153">Instrumentpanelen innehåller kolumnerna i tabellen nedan.</span><span class="sxs-lookup"><span data-stu-id="6db37-153">The dashboard includes the columns in the table that follows.</span></span> <span data-ttu-id="6db37-154">Varje kolumnen visas de översta 10 händelserna efter antal som matchar den kolumnen villkoren för det angivna tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="6db37-154">Each column lists the top 10 events by count matching that column's criteria for the specified time range.</span></span> <span data-ttu-id="6db37-155">Du kan köra en sökning i logg som innehåller hela listan genom att klicka på **se alla** längst ned rätt i varje kolumn, eller genom att klicka på kolumnrubriken.</span><span class="sxs-lookup"><span data-stu-id="6db37-155">You can run a log search that provides the entire list by clicking **See all** at the right bottom of each column, or by clicking the column header.</span></span>

| <span data-ttu-id="6db37-156">**Service Fabric-händelse**</span><span class="sxs-lookup"><span data-stu-id="6db37-156">**Service Fabric event**</span></span> | <span data-ttu-id="6db37-157">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="6db37-157">**description**</span></span> |
| --- | --- |
| <span data-ttu-id="6db37-158">Anmärkningsvärda problem</span><span class="sxs-lookup"><span data-stu-id="6db37-158">Notable Issues</span></span> | <span data-ttu-id="6db37-159">Visar problem, inklusive RunAsyncFailures, RunAsynCancellations och noden används.</span><span class="sxs-lookup"><span data-stu-id="6db37-159">Displays issues including RunAsyncFailures, RunAsynCancellations, and Node Downs.</span></span> |
| <span data-ttu-id="6db37-160">Operativa händelser</span><span class="sxs-lookup"><span data-stu-id="6db37-160">Operational Events</span></span> | <span data-ttu-id="6db37-161">Visar viktiga operativa händelser, inklusive uppgradering av programmet och distributioner.</span><span class="sxs-lookup"><span data-stu-id="6db37-161">Displays notable operational events including application upgrade and deployments.</span></span> |
| <span data-ttu-id="6db37-162">Händelser för tillförlitlig tjänst</span><span class="sxs-lookup"><span data-stu-id="6db37-162">Reliable Service Events</span></span> | <span data-ttu-id="6db37-163">Visar viktiga tillförlitlig tjänsthändelser, inklusive Runasyncinvocations.</span><span class="sxs-lookup"><span data-stu-id="6db37-163">Displays notable reliable service events including  Runasyncinvocations.</span></span> |
| <span data-ttu-id="6db37-164">Aktören händelser</span><span class="sxs-lookup"><span data-stu-id="6db37-164">Actor Events</span></span> | <span data-ttu-id="6db37-165">Visar viktiga aktören händelser som genererats av dina micro-tjänster.</span><span class="sxs-lookup"><span data-stu-id="6db37-165">Displays notable actor events generated by your micro-services.</span></span> <span data-ttu-id="6db37-166">Händelser innehåller undantag som utlöses av en aktörsmetod, aktören aktiveringar och avaktiveringar och så vidare.</span><span class="sxs-lookup"><span data-stu-id="6db37-166">Events include exceptions thrown by an actor method, actor activations and deactivations, and so on.</span></span> |
| <span data-ttu-id="6db37-167">Programhändelser</span><span class="sxs-lookup"><span data-stu-id="6db37-167">Application Events</span></span> | <span data-ttu-id="6db37-168">Visar alla anpassade ETW händelser som genererats av dina program.</span><span class="sxs-lookup"><span data-stu-id="6db37-168">Displays all custom ETW events generated by your applications.</span></span> |

![Service Fabric-instrumentpanelen](./media/log-analytics-service-fabric/sf3.png)

![Service Fabric-instrumentpanelen](./media/log-analytics-service-fabric/sf4.png)

<span data-ttu-id="6db37-171">I följande tabell visar metoder för insamling av data och annan information om hur data samlas in för Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="6db37-171">The following table shows data collection methods and other details about how data is collected for Service Fabric:</span></span>

| <span data-ttu-id="6db37-172">Plattform</span><span class="sxs-lookup"><span data-stu-id="6db37-172">platform</span></span> | <span data-ttu-id="6db37-173">Styr Agent</span><span class="sxs-lookup"><span data-stu-id="6db37-173">Direct Agent</span></span> | <span data-ttu-id="6db37-174">Operations Manager-agent</span><span class="sxs-lookup"><span data-stu-id="6db37-174">Operations Manager agent</span></span> | <span data-ttu-id="6db37-175">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="6db37-175">Azure Storage</span></span> | <span data-ttu-id="6db37-176">Operations Manager som krävs?</span><span class="sxs-lookup"><span data-stu-id="6db37-176">Operations Manager required?</span></span> | <span data-ttu-id="6db37-177">Operations Manager agent-data som skickas via management-grupp</span><span class="sxs-lookup"><span data-stu-id="6db37-177">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="6db37-178">Frekvens för samlingen</span><span class="sxs-lookup"><span data-stu-id="6db37-178">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="6db37-179">Windows</span><span class="sxs-lookup"><span data-stu-id="6db37-179">Windows</span></span> |  |  | <span data-ttu-id="6db37-180">&#8226;</span><span class="sxs-lookup"><span data-stu-id="6db37-180">&#8226;</span></span> |  |  |<span data-ttu-id="6db37-181">10 minuter</span><span class="sxs-lookup"><span data-stu-id="6db37-181">10 minutes</span></span> |

> [!NOTE]
> <span data-ttu-id="6db37-182">Ändra omfattningen för händelser med **Data baserat på senaste sju dagarna** överst på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="6db37-182">Change the scope of events with **Data based on last seven days** at the top of the dashboard.</span></span> <span data-ttu-id="6db37-183">Du kan också visa händelser som genererats under de senaste sju dagarna, en dag eller sex timmar.</span><span class="sxs-lookup"><span data-stu-id="6db37-183">You can also show events generated within the last seven days, one day, or six hours.</span></span> <span data-ttu-id="6db37-184">Du kan också välja **anpassade** att ange ett anpassat datumintervall.</span><span class="sxs-lookup"><span data-stu-id="6db37-184">Or, you can select **Custom** to specify a custom date range.</span></span>
>
>

## <a name="troubleshoot-your-service-fabric-and-log-analytics-configuration"></a><span data-ttu-id="6db37-185">Felsökning av konfigurationen av Service Fabric och logganalys</span><span class="sxs-lookup"><span data-stu-id="6db37-185">Troubleshoot your Service Fabric and Log Analytics configuration</span></span>
<span data-ttu-id="6db37-186">Om du behöver verifiera din konfiguration för logganalys eftersom det inte går att visa händelsedata i Log Analytics använder du följande skript.</span><span class="sxs-lookup"><span data-stu-id="6db37-186">If you need to verify your Log Analytics configuration because you are unable to view event data in Log Analytics, use the following script.</span></span> <span data-ttu-id="6db37-187">Följande åtgärder utförs:</span><span class="sxs-lookup"><span data-stu-id="6db37-187">It performs the following actions:</span></span>

1. <span data-ttu-id="6db37-188">Läser konfigurationen Service Fabric-diagnostik</span><span class="sxs-lookup"><span data-stu-id="6db37-188">Reads your Service Fabric diagnostics configuration</span></span>
2. <span data-ttu-id="6db37-189">Söker efter data som skrivs till register</span><span class="sxs-lookup"><span data-stu-id="6db37-189">Checks for data written into the tables</span></span>
3. <span data-ttu-id="6db37-190">Kontrollerar att logganalys är konfigurerad för att läsa från tabeller</span><span class="sxs-lookup"><span data-stu-id="6db37-190">Verifies that Log Analytics is configured to read from the tables</span></span>

```
<#
    Verify Service Fabric and Log Analytics configuration
    1. Read Service Fabric diagnostics configuration
    2. Check for data being written into the tables
    3. Verify Log Analytics is configured to read from the tables

    Supported tables:
    WADServiceFabricReliableActorEventTable
    WADServiceFabricReliableServiceEventTable
    WADServiceFabricSystemEventTable
    WADETWEventTable

    Script will write a warning for every misconfiguration detected
    To see items that are correctly configured set $VerbosePreference="Continue"
#>
Param
(
    [Parameter(Mandatory=$true,
    ValueFromPipeline=$true,
    Position=1)]
    [string]$workspaceName
)

$WADtables = @("WADServiceFabricReliableActorEventTable",
               "WADServiceFabricReliableServiceEventTable",
               "WADServiceFabricSystemEventTable",
               "WADETWEventTable"
               )

<#
    Check if OMS Log Analytics is configured to index service fabric events from the specified table
#>

function Check-OMSLogAnalyticsConfiguration {
    param(
    [psobject]$workspace,
    [psobject]$storageAccount,
    [string]$id
    )

    $existingInsights = Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

    if ($existingInsights)
    {
        $currentStorageAccountInsight = $existingInsights.Where({$_.StorageAccountResourceId -eq $storageAccount.ResourceId})

        if ("WADServiceFabric*EventTable" -in $currentStorageAccountInsight.Tables)
        {
            Write-Verbose ("OMS Log Analytics workspace " + $workspace.Name + " is configured to index service fabric actor, service and operational events from " + $storageAccount.Name)
        } else
        {
            Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + " is not configured to index service fabric actor, service and operational events from " + $storageAccount.Name)
        }
        if ("WADETWEventTable" -in $currentStorageAccountInsight.Tables)
        {
            Write-Verbose ("OMS Log Analytics workspace " + $workspace.Name + " is configured to index service fabric application events from " + $storageAccount.Name)
        } else
        {
            Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + " is not configured to index service fabric application events from " + $storageAccount.Name)
        }
    } else
    {
        Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + "is not configured to read service fabric events from " + $storageAccount.Name)
    }    
}

<#
    Check Azure table storage to confirm there is recent data written by Service Fabric
#>

function Check-TablesForData {
    param(
    [psobject]$storageAccount
    )

    $ctx = (Get-AzureRmStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.ResourceName).Context

    $createdTables = Get-AzureStorageTable -Context $ctx

    $recently = Get-Date -Format s ((Get-Date).AddMinutes(-20).ToUniversalTime())
    $recently = $recently + "Z"

    foreach ($table in $WADtables)
    {
        if ($table -in $createdTables.Name)
        {
            $tbl = Get-AzureStorageTable -Name $table -Context $ctx
            $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery
            $list = New-Object System.Collections.Generic.List[string]
            $list.Add("RowKey")
            $list.Add("ProviderName")
            $list.Add("Timestamp")
            $query.FilterString = "Timestamp gt datetime'$recently'"
            $query.SelectColumns = $list
            $query.TakeCount = 20
            $entities = $tbl.CloudTable.ExecuteQuery($query)
            Write-Debug $entities
            if ($entities.Count -gt 0)
            {
                Write-Verbose ("Data was written to $table in " + $storageAccount.ResourceName + "after $recently")
            } else
            {
                Write-Warning ("No data after $recently is in  $table in " + $storageAccount.ResourceName)
            }
        } else
        {
            Write-Warning ("$table does not exist in storage account " + $storageAccount.ResourceName)
        }
    }
}

<#
    Check if ETW provider is configured to log events to the expected table storage
#>
function Check-ETWProviderLogging {
    param(
    [string]$id,
    [string]$provider,
    [string]$expectedTable,
    [string]$table
    )      
        Write-Debug ("ID: $id Provider: $provider ExpectedTable $expectedTable ActualTable $table")
        if ( ($table -eq $null) -or ($table -eq ""))
        {
            Write-Warning ("$id No configuration found for $provider. Configure Azure diagnostics to write to $expectedTable.")
        }
        elseif ( $table -ne $expectedTable )
        {
            Write-Warning ("$id $provider events are being written to $table instead of WAD$expectedTable. Events will not be collected by Log Analytics")
        }
        else
        {
            Write-Verbose "$id $provider events are being written to WAD$expectedTable (Correct configuration.)"
        }
}

<#
    Check Azure Diagnostics Configuration for a Service Fabric cluster
#>
function Check-ServiceFabricScaleSetDiagnostics {
    param(
    [psobject]$scaleSetDiagnostics
    )

    $storageAccountsFound = @()
    Write-Verbose ("Checking " + $scaleSetDiagnostics)
    $sfReliableActorTable = $null
    $sfReliableServiceTable = $null
    $sfOperationalTable = $null
    Write-Debug $scaleSetDiagnostics
    $serviceFabricProviderList = ""
    $etwManifestProviderList = ""

    if ( $scaleSetDiagnostics.xmlCfg )
    {
        Write-Debug ("Found XMLcfg")
        $xmlCfg = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($scaleSetDiagnostics.xmlCfg))
        Write-Debug $xmlCfg
        $etwProviders = Select-Xml -Content $xmlCfg -XPath "//EtwProviders"                
        $serviceFabricProviderList = $etwProviders.Node.EtwEventSourceProviderConfiguration
        $etwManifestProviderList = $etwProviders.Node.EtwManifestProviderConfiguration
    } elseif ($scaleSetDiagnostics.WadCfg )
    {
        Write-Debug ("Found WADcfg")
        Write-Debug $scaleSetDiagnostics.WadCfg
        $serviceFabricProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwEventSourceProviderConfiguration
        $etwManifestProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwManifestProviderConfiguration
    } else
    {
        Write-Error "Unable to parse Azure Diagnostics setting for $id"
        Write-Warning ("$id does not have diagnostics enabled")
    }

    foreach ($provider in $serviceFabricProviderList)
    {
        Write-Debug ("Event Source Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
        if ($provider.Provider -eq "Microsoft-ServiceFabric-Actors")
        {
            $sfReliableActorTable = $provider.DefaultEvents.eventDestination
        } elseif ($provider.Provider -eq "Microsoft-ServiceFabric-Services")
        {
            $sfReliableServiceTable = $provider.DefaultEvents.eventDestination
        } else
        {
            Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
        }
    }
    foreach ($provider in $etwManifestProviderList)
    {
        Write-Debug ("Manifest Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
        if ($provider.Provider -eq "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8")
        {
            $sfOperationalTable = $provider.DefaultEvents.eventDestination
        } else
        {
            Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
        }
    }

    Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Actors" "ServiceFabricReliableActorEventTable" $sfReliableActorTable
    Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Services" "ServiceFabricReliableServiceEventTable" $sfReliableServiceTable
    Check-ETWProviderLogging $id "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8 (System events)" "ServiceFabricSystemEventTable" $sfOperationalTable

    Write-Verbose ("StorageAccount: " + $scaleSetDiagnostics.StorageAccount)

    $storageAccountsFound += ($scaleSetDiagnostics.StorageAccount)
    return ($storageAccountsFound)
}

# This script uses Get-AzureRmVMDiagnosticsExtension and needs a version where -Name is not a required parameter
Import-Module AzureRM.Compute -MinimumVersion 1.2.2

try
{
    Get-AzureRmContext
}
catch [System.Management.Automation.PSInvalidOperationException]
{
    Login-AzureRmAccount
}

$allResources = Get-AzureRmResource

$OMSworkspace = $allResources.Where({($_.ResourceType -eq "Microsoft.OperationalInsights/workspaces") -and ($_.ResourceName -eq $workspaceName)})

if ($OMSworkspace.Name -ne $workspaceName)
{
    Write-Error ("Unable to find Log Analytics Workspace " + $workspaceName)
}

$serviceFabricClusters = $allResources.Where({$_.ResourceType -eq "Microsoft.ServiceFabric/clusters"})
$storageAccountList = @()
foreach($cluster in $serviceFabricClusters) {
    Write-Verbose ("Checking cluster: " + $cluster.Name)
    $scaleSet = ($allResources.Where({($_.ResourceType -eq "Microsoft.Compute/virtualMachineScaleSets") -and ($_.ResourceGroupName -eq $cluster.ResourceGroupName)}))

    foreach($set in $scaleSet) {
        $resource = Get-AzureRmResource -ResourceId $set.ResourceId
        $extensions = $resource.Properties.VirtualMachineProfile.ExtensionProfile.Extensions
        foreach($ext in $extensions) {
            if ($ext.Properties.Publisher -eq "Microsoft.Azure.Diagnostics" -and $ext.Properties.Type -eq "IaaSDiagnostics") {
                $storageAccountList += (Check-ServiceFabricScaleSetDiagnostics $ext.Properties.Settings)
            }
        }
    }
}

$storageAccountList = $storageAccountList | Sort-Object | Get-Unique
$storageAccountsToCheck = ($allResources.Where({($_.ResourceType -eq "Microsoft.Storage/storageAccounts") -and ($_.ResourceName -in $storageAccountList)}))

foreach($storageAccount in $storageAccountsToCheck)
{
    Check-TablesForData $storageAccount
    Check-OMSLogAnalyticsConfiguration $OMSworkspace $storageAccount
}
 ```


## <a name="next-steps"></a><span data-ttu-id="6db37-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6db37-191">Next steps</span></span>
* <span data-ttu-id="6db37-192">Använd [loggen söker i logganalys](log-analytics-log-searches.md) att visa detaljerad information för Service Fabric-händelse.</span><span class="sxs-lookup"><span data-stu-id="6db37-192">Use [Log Searches in Log Analytics](log-analytics-log-searches.md) to view detailed Service Fabric event data.</span></span>
