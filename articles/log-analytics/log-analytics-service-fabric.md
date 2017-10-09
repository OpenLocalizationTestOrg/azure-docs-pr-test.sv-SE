---
title: "aaaAssess Service Fabric-program med Azure Log Analytics med hjälp av PowerShell | Microsoft Docs"
description: "Du kan använda hello Service Fabric-lösning i Log Analytics med hjälp av PowerShell tooassess hello risk och hälsotillståndet för Service Fabric-program, micro-tjänster, noder och kluster."
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
ms.openlocfilehash: 3f6d6c0df02d6d453b77e50b75b64bf7eb73bbbf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assess-azure-service-fabric-applications-and-micro-services-with-powershell"></a><span data-ttu-id="77393-103">Utvärdera Azure Service Fabric-program och micro-tjänster med PowerShell</span><span class="sxs-lookup"><span data-stu-id="77393-103">Assess Azure Service Fabric applications and micro-services with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="77393-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="77393-104">Resource Manager</span></span>](log-analytics-service-fabric-azure-resource-manager.md)
> * [<span data-ttu-id="77393-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="77393-105">PowerShell</span></span>](log-analytics-service-fabric.md)
>
>


![Service Fabric-symbolen](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

<span data-ttu-id="77393-107">Den här artikeln beskrivs hur toouse hello Service Fabric-lösning i logganalys toohelp identifiera och felsöka problem i Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="77393-107">This article describes how toouse hello Service Fabric solution in Log Analytics toohelp identify and troubleshoot issues across your Service Fabric cluster.</span></span> <span data-ttu-id="77393-108">Det kan du se hur utför Service Fabric-noder och hur dina program och tjänster micro körs.</span><span class="sxs-lookup"><span data-stu-id="77393-108">It helps you see how your Service Fabric nodes are performing and how your applications and micro-services are running.</span></span>

<span data-ttu-id="77393-109">hello Service Fabric-lösningen använder Azure-diagnostik data från din Service Fabric virtuella datorer genom att samla in data från Azure BOMULLSTUSS tabellerna.</span><span class="sxs-lookup"><span data-stu-id="77393-109">hello Service Fabric solution uses Azure Diagnostics data from your Service Fabric VMs, by collecting this data from your Azure WAD tables.</span></span> <span data-ttu-id="77393-110">Logganalys läser sedan hello följande Service Fabric framework händelser:</span><span class="sxs-lookup"><span data-stu-id="77393-110">Log Analytics then reads hello following Service Fabric framework events:</span></span>

- <span data-ttu-id="77393-111">**Händelser för tillförlitlig tjänst**</span><span class="sxs-lookup"><span data-stu-id="77393-111">**Reliable Service Events**</span></span>
- <span data-ttu-id="77393-112">**Aktören händelser**</span><span class="sxs-lookup"><span data-stu-id="77393-112">**Actor Events**</span></span>
- <span data-ttu-id="77393-113">**Operativa händelser**</span><span class="sxs-lookup"><span data-stu-id="77393-113">**Operational Events**</span></span>
- <span data-ttu-id="77393-114">**Anpassade ETW-händelser**</span><span class="sxs-lookup"><span data-stu-id="77393-114">**Custom ETW events**</span></span>

<span data-ttu-id="77393-115">hello Service Fabric-lösningen instrumentpanelen visar viktiga problem och relevanta händelser i Service Fabric-miljö.</span><span class="sxs-lookup"><span data-stu-id="77393-115">hello Service Fabric solution dashboard shows you notable issues and relevant events in your Service Fabric environment.</span></span>

## <a name="installing-and-configuring-hello-solution"></a><span data-ttu-id="77393-116">Installera och konfigurera hello lösning</span><span class="sxs-lookup"><span data-stu-id="77393-116">Installing and configuring hello solution</span></span>
<span data-ttu-id="77393-117">Följ dessa tre enkla steg tooinstall och konfigurera hello lösning:</span><span class="sxs-lookup"><span data-stu-id="77393-117">Follow these three easy steps tooinstall and configure hello solution:</span></span>

1. <span data-ttu-id="77393-118">Associera hello Azure-prenumeration som du använde toocreate alla resurser i klustret, inklusive lagringskonton med din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="77393-118">Associate hello Azure subscription that you used toocreate all cluster resources, including storage accounts, with your workspace.</span></span> <span data-ttu-id="77393-119">Se [Kom igång med logganalys](log-analytics-get-started.md) information om hur du skapar en logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="77393-119">See [Get started with Log Analytics](log-analytics-get-started.md) for information about creating a Log Analytics workspace.</span></span>
2. <span data-ttu-id="77393-120">Konfigurera logganalys toocollect och visa loggar för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="77393-120">Configure Log Analytics toocollect and view Service Fabric logs.</span></span>
3. <span data-ttu-id="77393-121">Aktivera hello Service Fabric-lösning i arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="77393-121">Enable hello Service Fabric solution in your workspace.</span></span>

## <a name="configure-log-analytics-toocollect-and-view-service-fabric-logs"></a><span data-ttu-id="77393-122">Konfigurera logganalys toocollect och visa loggar för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="77393-122">Configure Log Analytics toocollect and view Service Fabric logs</span></span>
<span data-ttu-id="77393-123">I det här avsnittet får du lära dig hur tooconfigure logganalys tooretrieve Service Fabric loggar.</span><span class="sxs-lookup"><span data-stu-id="77393-123">In this section, you learn how tooconfigure Log Analytics tooretrieve Service Fabric logs.</span></span> <span data-ttu-id="77393-124">Tillåt tooview hello loggar, analysera och felsöka problem i klustret eller hello program och tjänster som körs i klustret, med hjälp av hello OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="77393-124">hello logs allow you tooview, analyze, and troubleshoot issues in your cluster or in hello applications and services running in that cluster, using hello OMS portal.</span></span>

> [!NOTE]
> <span data-ttu-id="77393-125">Konfigurera hello Azure Diagnostics tillägget tooupload hello loggar för storage-tabeller.</span><span class="sxs-lookup"><span data-stu-id="77393-125">Configure hello Azure Diagnostics extension tooupload hello logs for storage tables.</span></span> <span data-ttu-id="77393-126">hello tabeller måste matcha ser Log Analytics för.</span><span class="sxs-lookup"><span data-stu-id="77393-126">hello tables must match what Log Analytics looks for.</span></span> <span data-ttu-id="77393-127">Mer information finns i [hur toocollect loggar med Azure-diagnostik](../service-fabric/service-fabric-diagnostics-how-to-setup-wad.md).</span><span class="sxs-lookup"><span data-stu-id="77393-127">For more information, see [How toocollect logs with Azure Diagnostics](../service-fabric/service-fabric-diagnostics-how-to-setup-wad.md).</span></span> <span data-ttu-id="77393-128">hello configuration inställningar exemplen i den här artikeln visar vilka hello namnen på hello lagring tabeller ska vara.</span><span class="sxs-lookup"><span data-stu-id="77393-128">hello configuration settings examples in this article show you what hello names of hello storage tables should be.</span></span> <span data-ttu-id="77393-129">När diagnostik ställs in på hello kluster och laddar upp loggar tooa storage-konto, hello nästa steg är tooconfigure logganalys toocollect loggarna.</span><span class="sxs-lookup"><span data-stu-id="77393-129">Once Diagnostics is set up on hello cluster and is uploading logs tooa storage account, hello next step is tooconfigure Log Analytics toocollect these logs.</span></span>
>
>

<span data-ttu-id="77393-130">Se till att du uppdaterar hello **EtwEventSourceProviderConfiguration** avsnitt i hello **template.json** filen tooadd poster för hello nya EventSources innan du använder hello konfigurationen uppdatera genom att kör **deploy.ps1**.</span><span class="sxs-lookup"><span data-stu-id="77393-130">Ensure that you update hello **EtwEventSourceProviderConfiguration** section in hello **template.json** file tooadd entries for hello new EventSources before you apply hello configuration update by running **deploy.ps1**.</span></span> <span data-ttu-id="77393-131">hello tabellen för överföringen är hello samma som (ETWEventTable).</span><span class="sxs-lookup"><span data-stu-id="77393-131">hello table for upload is hello same as (ETWEventTable).</span></span> <span data-ttu-id="77393-132">Hello tillfället logganalys endast kan läsa programmet ETW-händelser från hello *WADETWEventTable* tabell.</span><span class="sxs-lookup"><span data-stu-id="77393-132">At hello moment, Log Analytics can only read application ETW events from hello *WADETWEventTable* table.</span></span>

<span data-ttu-id="77393-133">hello följande verktyg är används tooperform vissa hello åtgärder i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="77393-133">hello following tools are used tooperform some of hello operations in this section:</span></span>

* <span data-ttu-id="77393-134">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="77393-134">Azure PowerShell</span></span>
* [<span data-ttu-id="77393-135">Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="77393-135">Operations Management Suite</span></span>](http://www.microsoft.com/oms)

### <a name="configure-a-log-analytics-workspace-tooshow-hello-cluster-logs"></a><span data-ttu-id="77393-136">Konfigurera en logganalys arbetsytan tooshow hello klustret loggar</span><span class="sxs-lookup"><span data-stu-id="77393-136">Configure a Log Analytics workspace tooshow hello cluster logs</span></span>

<span data-ttu-id="77393-137">Konfigurera hello arbetsytan toopull loggar från hello Azure storage-tabeller när du har skapat en logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="77393-137">After you create a Log Analytics workspace, configure hello workspace toopull logs from hello Azure storage tables.</span></span> <span data-ttu-id="77393-138">Kör följande PowerShell-skript hello:</span><span class="sxs-lookup"><span data-stu-id="77393-138">Then, run hello following PowerShell script:</span></span>

```
<#
    This script will configure an Operations Management Suite workspace (previously called an Operational Insights workspace) tooread Diagnostics from an Azure Storage account.
    It will enable all supported data types (currently Service Fabric Events, ETW Events and IIS Logs).
    It supports Resource Manager storage accounts.
    If you have more than one Azure Subscription, you will be prompted for hello subscription tooconfigure.
    If you have more than one Log Analytics workspace you will be prompted for hello workspace tooconfigure.
    It will then look through your Service Fabric clusters, and configure your Log Analytics workspace tooread Diagnostics from storage accounts that are connected toothat cluster and have diagnostics enabled.
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
            $uiPrompt = "Enter hello number corresponding toohello Azure subscription you would like toowork with.`n"

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
            $uiPrompt = "Enter hello number corresponding toohello workspace you want tooconfigure.`n"
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
             Write-Warning ("$id No configuration found for $provider. Configure Azure diagnostics toowrite too$expectedTable.")
         }  
         elseif ( $table -ne $expectedTable )
         {
             Write-Warning ("$id $provider events are being written too$table instead of WAD$expectedTable. Events will not be collected by Log Analytics")
         }  
         else
         {
             Write-Verbose "$id $provider events are being written tooWAD$expectedTable (Correct configuration.)"
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
         Write-Error "Unable tooparse Azure Diagnostics setting for $id"
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
    $serviceFabricClusters = $allResources.Where({$_.ResourceType -eq "Microsoft.ServiceFabric/clusters"}) #pulls in all service fabric clusters in hello resource
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
                                # HTTP Not Found is returned if hello storage insight doesn't exist
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
                                      # If any of hello tables from hello table list are not already monitored, then we add them
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

<span data-ttu-id="77393-139">När du har konfigurerat hello logganalys arbetsytan tooread från hello Azure tabeller i ditt lagringskonto, logga in toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="77393-139">After you've configured hello Log Analytics workspace tooread from hello Azure tables in your storage account, log in toohello Azure portal.</span></span> <span data-ttu-id="77393-140">Välj hello logganalys-arbetsytan från **alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="77393-140">Select hello Log Analytics workspace from **All Resources**.</span></span> <span data-ttu-id="77393-141">hello antalet storage-konto loggar anslutna toohello arbetsytan visas.</span><span class="sxs-lookup"><span data-stu-id="77393-141">hello number of storage account logs connected toohello workspace is displayed.</span></span> <span data-ttu-id="77393-142">Välj hello **Storage-konto loggar** panelen.</span><span class="sxs-lookup"><span data-stu-id="77393-142">Select hello **Storage account logs** tile.</span></span> <span data-ttu-id="77393-143">Granska hello lista över storage-konto loggar tooverify att ditt lagringskonto är rätt anslutna toohello-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="77393-143">Review hello list of storage account logs tooverify that your storage account is connected toohello correct workspace.</span></span>

![Storage-konto loggar](./media/log-analytics-service-fabric/sf1.png)

## <a name="enable-hello-service-fabric-solution"></a><span data-ttu-id="77393-145">Aktivera hello Service Fabric-lösning</span><span class="sxs-lookup"><span data-stu-id="77393-145">Enable hello Service Fabric solution</span></span>
<span data-ttu-id="77393-146">Använd hello följande skript tooadd hello lösning tooyour logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="77393-146">Use hello following script tooadd hello solution tooyour Log Analytics workspace.</span></span> <span data-ttu-id="77393-147">Kör hello skriptet i PowerShell använder hello Azure-prenumeration som är associerad med hello logganalys-arbetsytan som du vill använda tooenable hello Service Fabric-lösningen.</span><span class="sxs-lookup"><span data-stu-id="77393-147">Run hello script in PowerShell, using hello Azure subscription that is associated with hello Log Analytics workspace that you want tooenable hello Service Fabric solution in.</span></span>

```
function Select-Subscription {
      $subscription = ""
      $allSubscriptions = Get-AzureRmSubscription
      switch ($allSubscriptions.Count) {
             0 {Write-Error "No Operations Management Suite workspaces found"}
             1 {return $allSubscriptions}
        default {
            $uiPrompt = "Enter hello number corresponding toohello Azure subscription you would like toowork with.`n"
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
            $uiPrompt = "Enter hello number corresponding toohello workspace you want tooconfigure.`n"
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

<span data-ttu-id="77393-148">När du har aktiverat hello lösning hello Service Fabric panel har lagts tooyour logganalys *översikt* sidan.</span><span class="sxs-lookup"><span data-stu-id="77393-148">After you enable hello solution, hello Service Fabric tile is added tooyour Log Analytics *Overview* page.</span></span> <span data-ttu-id="77393-149">hello sidan visas en vy över anmärkningsvärda problem, till exempel runAsync fel och avbryta som uppstått i hello senaste 24 timmarna.</span><span class="sxs-lookup"><span data-stu-id="77393-149">hello page shows a view of notable issues such as runAsync failures and cancellations that occurred in hello last 24 hours.</span></span>

![Service Fabric-panelen](./media/log-analytics-service-fabric/sf2.png)

### <a name="view-service-fabric-events"></a><span data-ttu-id="77393-151">Visa händelser för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="77393-151">View Service Fabric events</span></span>
<span data-ttu-id="77393-152">Klicka på hello **Service Fabric** panelen tooopen hello Service Fabric dashboard.</span><span class="sxs-lookup"><span data-stu-id="77393-152">Click hello **Service Fabric** tile tooopen hello Service Fabric dashboard.</span></span> <span data-ttu-id="77393-153">hello instrumentpanelen innehåller hello kolumner i hello i tabellen nedan.</span><span class="sxs-lookup"><span data-stu-id="77393-153">hello dashboard includes hello columns in hello table that follows.</span></span> <span data-ttu-id="77393-154">Varje kolumn visar hello översta 10 händelser efter antal matchande att kolumnens sökvillkor för hello angivna tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="77393-154">Each column lists hello top 10 events by count matching that column's criteria for hello specified time range.</span></span> <span data-ttu-id="77393-155">Du kan köra en sökning i logg som innehåller hello hela listan genom att klicka på **se alla** längst ned hello rätt i varje kolumn, eller klicka på kolumnrubriken hello.</span><span class="sxs-lookup"><span data-stu-id="77393-155">You can run a log search that provides hello entire list by clicking **See all** at hello right bottom of each column, or by clicking hello column header.</span></span>

| <span data-ttu-id="77393-156">**Service Fabric-händelse**</span><span class="sxs-lookup"><span data-stu-id="77393-156">**Service Fabric event**</span></span> | <span data-ttu-id="77393-157">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="77393-157">**description**</span></span> |
| --- | --- |
| <span data-ttu-id="77393-158">Anmärkningsvärda problem</span><span class="sxs-lookup"><span data-stu-id="77393-158">Notable Issues</span></span> | <span data-ttu-id="77393-159">Visar problem, inklusive RunAsyncFailures, RunAsynCancellations och noden används.</span><span class="sxs-lookup"><span data-stu-id="77393-159">Displays issues including RunAsyncFailures, RunAsynCancellations, and Node Downs.</span></span> |
| <span data-ttu-id="77393-160">Operativa händelser</span><span class="sxs-lookup"><span data-stu-id="77393-160">Operational Events</span></span> | <span data-ttu-id="77393-161">Visar viktiga operativa händelser, inklusive uppgradering av programmet och distributioner.</span><span class="sxs-lookup"><span data-stu-id="77393-161">Displays notable operational events including application upgrade and deployments.</span></span> |
| <span data-ttu-id="77393-162">Händelser för tillförlitlig tjänst</span><span class="sxs-lookup"><span data-stu-id="77393-162">Reliable Service Events</span></span> | <span data-ttu-id="77393-163">Visar viktiga tillförlitlig tjänsthändelser, inklusive Runasyncinvocations.</span><span class="sxs-lookup"><span data-stu-id="77393-163">Displays notable reliable service events including  Runasyncinvocations.</span></span> |
| <span data-ttu-id="77393-164">Aktören händelser</span><span class="sxs-lookup"><span data-stu-id="77393-164">Actor Events</span></span> | <span data-ttu-id="77393-165">Visar viktiga aktören händelser som genererats av dina micro-tjänster.</span><span class="sxs-lookup"><span data-stu-id="77393-165">Displays notable actor events generated by your micro-services.</span></span> <span data-ttu-id="77393-166">Händelser innehåller undantag som utlöses av en aktörsmetod, aktören aktiveringar och avaktiveringar och så vidare.</span><span class="sxs-lookup"><span data-stu-id="77393-166">Events include exceptions thrown by an actor method, actor activations and deactivations, and so on.</span></span> |
| <span data-ttu-id="77393-167">Programhändelser</span><span class="sxs-lookup"><span data-stu-id="77393-167">Application Events</span></span> | <span data-ttu-id="77393-168">Visar alla anpassade ETW händelser som genererats av dina program.</span><span class="sxs-lookup"><span data-stu-id="77393-168">Displays all custom ETW events generated by your applications.</span></span> |

![Service Fabric-instrumentpanelen](./media/log-analytics-service-fabric/sf3.png)

![Service Fabric-instrumentpanelen](./media/log-analytics-service-fabric/sf4.png)

<span data-ttu-id="77393-171">hello visar följande tabell metoder för insamling av data och annan information om hur data samlas in för Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="77393-171">hello following table shows data collection methods and other details about how data is collected for Service Fabric:</span></span>

| <span data-ttu-id="77393-172">Plattform</span><span class="sxs-lookup"><span data-stu-id="77393-172">platform</span></span> | <span data-ttu-id="77393-173">Styr Agent</span><span class="sxs-lookup"><span data-stu-id="77393-173">Direct Agent</span></span> | <span data-ttu-id="77393-174">Operations Manager-agent</span><span class="sxs-lookup"><span data-stu-id="77393-174">Operations Manager agent</span></span> | <span data-ttu-id="77393-175">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="77393-175">Azure Storage</span></span> | <span data-ttu-id="77393-176">Operations Manager som krävs?</span><span class="sxs-lookup"><span data-stu-id="77393-176">Operations Manager required?</span></span> | <span data-ttu-id="77393-177">Operations Manager agent-data som skickas via management-grupp</span><span class="sxs-lookup"><span data-stu-id="77393-177">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="77393-178">Frekvens för samlingen</span><span class="sxs-lookup"><span data-stu-id="77393-178">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="77393-179">Windows</span><span class="sxs-lookup"><span data-stu-id="77393-179">Windows</span></span> |  |  | <span data-ttu-id="77393-180">&#8226;</span><span class="sxs-lookup"><span data-stu-id="77393-180">&#8226;</span></span> |  |  |<span data-ttu-id="77393-181">10 minuter</span><span class="sxs-lookup"><span data-stu-id="77393-181">10 minutes</span></span> |

> [!NOTE]
> <span data-ttu-id="77393-182">Ändra hello omfånget för händelser med **Data baserat på senaste sju dagarna** hello överst i hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="77393-182">Change hello scope of events with **Data based on last seven days** at hello top of hello dashboard.</span></span> <span data-ttu-id="77393-183">Du kan också visa händelser som genereras inom hello senaste sju dagarna, en dag eller sex timmar.</span><span class="sxs-lookup"><span data-stu-id="77393-183">You can also show events generated within hello last seven days, one day, or six hours.</span></span> <span data-ttu-id="77393-184">Du kan också välja **anpassade** toospecify datumintervall.</span><span class="sxs-lookup"><span data-stu-id="77393-184">Or, you can select **Custom** toospecify a custom date range.</span></span>
>
>

## <a name="troubleshoot-your-service-fabric-and-log-analytics-configuration"></a><span data-ttu-id="77393-185">Felsökning av konfigurationen av Service Fabric och logganalys</span><span class="sxs-lookup"><span data-stu-id="77393-185">Troubleshoot your Service Fabric and Log Analytics configuration</span></span>
<span data-ttu-id="77393-186">Om du behöver tooverify konfiguration för logganalys eftersom du tooview händelsedata i Log Analytics Använd hello följande skript.</span><span class="sxs-lookup"><span data-stu-id="77393-186">If you need tooverify your Log Analytics configuration because you are unable tooview event data in Log Analytics, use hello following script.</span></span> <span data-ttu-id="77393-187">Hello följande åtgärder utförs:</span><span class="sxs-lookup"><span data-stu-id="77393-187">It performs hello following actions:</span></span>

1. <span data-ttu-id="77393-188">Läser konfigurationen Service Fabric-diagnostik</span><span class="sxs-lookup"><span data-stu-id="77393-188">Reads your Service Fabric diagnostics configuration</span></span>
2. <span data-ttu-id="77393-189">Söker efter data som skrivs till hello register</span><span class="sxs-lookup"><span data-stu-id="77393-189">Checks for data written into hello tables</span></span>
3. <span data-ttu-id="77393-190">Verifierar att logganalys är konfigurerade tooread från hello tabeller</span><span class="sxs-lookup"><span data-stu-id="77393-190">Verifies that Log Analytics is configured tooread from hello tables</span></span>

```
<#
    Verify Service Fabric and Log Analytics configuration
    1. Read Service Fabric diagnostics configuration
    2. Check for data being written into hello tables
    3. Verify Log Analytics is configured tooread from hello tables

    Supported tables:
    WADServiceFabricReliableActorEventTable
    WADServiceFabricReliableServiceEventTable
    WADServiceFabricSystemEventTable
    WADETWEventTable

    Script will write a warning for every misconfiguration detected
    toosee items that are correctly configured set $VerbosePreference="Continue"
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
    Check if OMS Log Analytics is configured tooindex service fabric events from hello specified table
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
            Write-Verbose ("OMS Log Analytics workspace " + $workspace.Name + " is configured tooindex service fabric actor, service and operational events from " + $storageAccount.Name)
        } else
        {
            Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + " is not configured tooindex service fabric actor, service and operational events from " + $storageAccount.Name)
        }
        if ("WADETWEventTable" -in $currentStorageAccountInsight.Tables)
        {
            Write-Verbose ("OMS Log Analytics workspace " + $workspace.Name + " is configured tooindex service fabric application events from " + $storageAccount.Name)
        } else
        {
            Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + " is not configured tooindex service fabric application events from " + $storageAccount.Name)
        }
    } else
    {
        Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + "is not configured tooread service fabric events from " + $storageAccount.Name)
    }    
}

<#
    Check Azure table storage tooconfirm there is recent data written by Service Fabric
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
                Write-Verbose ("Data was written too$table in " + $storageAccount.ResourceName + "after $recently")
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
    Check if ETW provider is configured toolog events toohello expected table storage
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
            Write-Warning ("$id No configuration found for $provider. Configure Azure diagnostics toowrite too$expectedTable.")
        }
        elseif ( $table -ne $expectedTable )
        {
            Write-Warning ("$id $provider events are being written too$table instead of WAD$expectedTable. Events will not be collected by Log Analytics")
        }
        else
        {
            Write-Verbose "$id $provider events are being written tooWAD$expectedTable (Correct configuration.)"
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
        Write-Error "Unable tooparse Azure Diagnostics setting for $id"
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
    Write-Error ("Unable toofind Log Analytics Workspace " + $workspaceName)
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


## <a name="next-steps"></a><span data-ttu-id="77393-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="77393-191">Next steps</span></span>
* <span data-ttu-id="77393-192">Använd [loggen söker i logganalys](log-analytics-log-searches.md) tooview detaljerad Service Fabric händelsedata.</span><span class="sxs-lookup"><span data-stu-id="77393-192">Use [Log Searches in Log Analytics](log-analytics-log-searches.md) tooview detailed Service Fabric event data.</span></span>
