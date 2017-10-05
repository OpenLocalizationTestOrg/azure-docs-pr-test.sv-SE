---
title: "Azure Service Fabric händelse aggregeringen med Windows Azure-diagnostik | Microsoft Docs"
description: "Läs mer om sammanställa och samlar in händelser med hjälp av BOMULLSTUSS för övervakning och diagnostik av Azure Service Fabric-kluster."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: 5773361fdec4cb8ee54fa2856f6aa969d5dac4e9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a><span data-ttu-id="bdb85-103">Aggregering av händelse och med Windows Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="bdb85-103">Event aggregation and collection using Windows Azure Diagnostics</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bdb85-104">Windows</span><span class="sxs-lookup"><span data-stu-id="bdb85-104">Windows</span></span>](service-fabric-diagnostics-event-aggregation-wad.md)
> * [<span data-ttu-id="bdb85-105">Linux</span><span class="sxs-lookup"><span data-stu-id="bdb85-105">Linux</span></span>](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

<span data-ttu-id="bdb85-106">När du kör ett Azure Service Fabric-kluster, är det en bra idé att samla in loggar från alla noder i en central plats.</span><span class="sxs-lookup"><span data-stu-id="bdb85-106">When you're running an Azure Service Fabric cluster, it's a good idea to collect the logs from all the nodes in a central location.</span></span> <span data-ttu-id="bdb85-107">Med loggarna på en central plats hjälper dig att analysera och felsöka problem i klustret eller problem i program och tjänster som körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="bdb85-107">Having the logs in a central location helps you analyze and troubleshoot issues in your cluster, or issues in the applications and services running in that cluster.</span></span>

<span data-ttu-id="bdb85-108">Ett sätt att överföra och samla in loggar är att använda Windows Azure Diagnostics (BOMULLSTUSS)-tillägget, som överför loggar till Azure Storage och har även möjlighet att skicka loggar till Azure Application Insights eller Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="bdb85-108">One way to upload and collect logs is to use the Windows Azure Diagnostics (WAD) extension, which uploads logs to Azure Storage, and also has the option to send logs to Azure Application Insights or Event Hubs.</span></span> <span data-ttu-id="bdb85-109">Du kan också använda en extern process för att läsa händelser från lagring och placera dem i en analys plattform produkt som [OMS logganalys](../log-analytics/log-analytics-service-fabric.md) eller en annan lösning för parsning av loggen.</span><span class="sxs-lookup"><span data-stu-id="bdb85-109">You can also use an external process to read the events from storage and place them in an analysis platform product, such as [OMS Log Analytics](../log-analytics/log-analytics-service-fabric.md) or another log-parsing solution.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bdb85-110">Krav</span><span class="sxs-lookup"><span data-stu-id="bdb85-110">Prerequisites</span></span>
<span data-ttu-id="bdb85-111">Verktygen används för att utföra vissa åtgärder i det här dokumentet:</span><span class="sxs-lookup"><span data-stu-id="bdb85-111">These tools are used to perform some of the operations in this document:</span></span>

* <span data-ttu-id="bdb85-112">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (rör Azure Cloud Services men har bra information och exempel)</span><span class="sxs-lookup"><span data-stu-id="bdb85-112">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (related to Azure Cloud Services but has good information and examples)</span></span>
* [<span data-ttu-id="bdb85-113">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="bdb85-113">Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="bdb85-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="bdb85-114">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="bdb85-115">Azure Resource Manager-klienten</span><span class="sxs-lookup"><span data-stu-id="bdb85-115">Azure Resource Manager client</span></span>](https://github.com/projectkudu/ARMClient)
* [<span data-ttu-id="bdb85-116">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="bdb85-116">Azure Resource Manager template</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-and-event-sources"></a><span data-ttu-id="bdb85-117">Loggen och händelsen källor</span><span class="sxs-lookup"><span data-stu-id="bdb85-117">Log and event sources</span></span>

### <a name="service-fabric-platform-events"></a><span data-ttu-id="bdb85-118">Service Fabric-plattformen händelser</span><span class="sxs-lookup"><span data-stu-id="bdb85-118">Service Fabric platform events</span></span>
<span data-ttu-id="bdb85-119">Enligt beskrivningen i [i den här artikeln](service-fabric-diagnostics-event-generation-infra.md), Service Fabric ställer du in med några out box loggning kanaler, som följande kanaler enkelt är konfigurerade med BOMULLSTUSS att skicka övervakning och diagnostikdata till en tabell för lagring eller någon annanstans:</span><span class="sxs-lookup"><span data-stu-id="bdb85-119">As discussed in [this article](service-fabric-diagnostics-event-generation-infra.md), Service Fabric sets you up with a few out-of-the-box logging channels, of which the following channels are easily configured with WAD to send monitoring and diagnostics data to a storage table or elsewhere:</span></span>
  * <span data-ttu-id="bdb85-120">Operativa händelser: att åtgärder som utförs av Service Fabric-plattformen.</span><span class="sxs-lookup"><span data-stu-id="bdb85-120">Operational events: higher-level operations that the Service Fabric platform performs.</span></span> <span data-ttu-id="bdb85-121">Exempel: skapa program och tjänster, nod tillståndsändringar och information om uppgradering.</span><span class="sxs-lookup"><span data-stu-id="bdb85-121">Examples include creation of applications and services, node state changes, and upgrade information.</span></span> <span data-ttu-id="bdb85-122">Dessa orsakat som loggar händelsen ETW Tracing for Windows)</span><span class="sxs-lookup"><span data-stu-id="bdb85-122">These are emitted as Event Tracing for Windows (ETW) logs</span></span>
  * [<span data-ttu-id="bdb85-123">Reliable Actors programmering modellen händelser</span><span class="sxs-lookup"><span data-stu-id="bdb85-123">Reliable Actors programming model events</span></span>](service-fabric-reliable-actors-diagnostics.md)
  * [<span data-ttu-id="bdb85-124">Reliable Services programming modellen händelser</span><span class="sxs-lookup"><span data-stu-id="bdb85-124">Reliable Services programming model events</span></span>](service-fabric-reliable-services-diagnostics.md)

### <a name="application-events"></a><span data-ttu-id="bdb85-125">Programhändelser</span><span class="sxs-lookup"><span data-stu-id="bdb85-125">Application events</span></span>
 <span data-ttu-id="bdb85-126">Händelser som orsakat från program och tjänsterna kod och skrivs med hjälp av EventSource hjälpklass i Visual Studio-mallar.</span><span class="sxs-lookup"><span data-stu-id="bdb85-126">Events emitted from your applications' and services' code and written out by using the EventSource helper class provided in the Visual Studio templates.</span></span> <span data-ttu-id="bdb85-127">Mer information om hur du skriver EventSource loggar från ditt program finns [övervaka och diagnostisera tjänster i en inställning för lokal dator development](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="bdb85-127">For more information on how to write EventSource logs from your application, see [Monitor and diagnose services in a local machine development setup](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>

## <a name="deploy-the-diagnostics-extension"></a><span data-ttu-id="bdb85-128">Distribuera diagnostik-tillägget</span><span class="sxs-lookup"><span data-stu-id="bdb85-128">Deploy the Diagnostics extension</span></span>
<span data-ttu-id="bdb85-129">Det första steget i att samla in loggar är att distribuera diagnostik tillägg på var och en av de virtuella datorerna i Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="bdb85-129">The first step in collecting logs is to deploy the Diagnostics extension on each of the VMs in the Service Fabric cluster.</span></span> <span data-ttu-id="bdb85-130">Diagnostik-tillägget samlar in loggar på varje virtuell dator och överför dem till lagringskontot som du anger.</span><span class="sxs-lookup"><span data-stu-id="bdb85-130">The Diagnostics extension collects logs on each VM and uploads them to the storage account that you specify.</span></span> <span data-ttu-id="bdb85-131">Stegen variera något beroende på om du använder Azure-portalen eller Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="bdb85-131">The steps vary a little based on whether you use the Azure portal or Azure Resource Manager.</span></span> <span data-ttu-id="bdb85-132">Stegen variera beroende på om distributionen är en del av klustret har skapats eller är i ett kluster som redan finns.</span><span class="sxs-lookup"><span data-stu-id="bdb85-132">The steps also vary based on whether the deployment is part of cluster creation or is for a cluster that already exists.</span></span> <span data-ttu-id="bdb85-133">Nu ska vi titta på stegen för varje scenario.</span><span class="sxs-lookup"><span data-stu-id="bdb85-133">Let's look at the steps for each scenario.</span></span>

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a><span data-ttu-id="bdb85-134">Distribuera diagnostik-tillägget som en del av klustret har skapats via Azure portal</span><span class="sxs-lookup"><span data-stu-id="bdb85-134">Deploy the Diagnostics extension as part of cluster creation through Azure portal</span></span>
<span data-ttu-id="bdb85-135">Om du vill distribuera diagnostik-tillägg till de virtuella datorerna i klustret som en del av klustret skapas, Använd panelen diagnostik inställningar visas i följande bild - Kontrollera att diagnostik är **på** (standardinställningen).</span><span class="sxs-lookup"><span data-stu-id="bdb85-135">To deploy the Diagnostics extension to the VMs in the cluster as part of cluster creation, you use the Diagnostics settings panel shown in the following image - ensure that Diagnostics is set to **On** (the default setting).</span></span> <span data-ttu-id="bdb85-136">När du skapar klustret kan du inte ändra dessa inställningar med hjälp av portalen.</span><span class="sxs-lookup"><span data-stu-id="bdb85-136">After you create the cluster, you can't change these settings by using the portal.</span></span>

![Azure diagnostikinställningar för klustret skapas i portalen](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics.png)

<span data-ttu-id="bdb85-138">När du skapar ett kluster med hjälp av portalen, vi rekommenderar starkt att du hämtar mallen **innan du klickar på OK** att skapa klustret.</span><span class="sxs-lookup"><span data-stu-id="bdb85-138">When you're creating a cluster by using the portal, we highly recommend that you download the template **before you click OK** to create the cluster.</span></span> <span data-ttu-id="bdb85-139">Mer information finns i [ställer in ett Service Fabric-kluster med en Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="bdb85-139">For details, refer to [Set up a Service Fabric cluster by using an Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="bdb85-140">Du behöver mallen du vill göra ändringarna senare, eftersom du inte kan göra några ändringar med hjälp av portalen.</span><span class="sxs-lookup"><span data-stu-id="bdb85-140">You'll need the template to make changes later, because you can't make some changes by using the portal.</span></span>

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a><span data-ttu-id="bdb85-141">Distribuera diagnostik-tillägget som en del av klustret har skapats med hjälp av Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="bdb85-141">Deploy the Diagnostics extension as part of cluster creation by using Azure Resource Manager</span></span>
<span data-ttu-id="bdb85-142">Om du vill skapa ett kluster med hjälp av hanteraren för filserverresurser, som du behöver lägga till diagnostik konfigurationen JSON till hela klustret Resource Manager-mallen innan du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="bdb85-142">To create a cluster by using Resource Manager, you need to add the Diagnostics configuration JSON to the full cluster Resource Manager template before you create the cluster.</span></span> <span data-ttu-id="bdb85-143">Vi ger en fem-VM klustret Resource Manager exempelmall diagnostik-konfiguration som lagts till som en del av våra exempel för Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="bdb85-143">We provide a sample five-VM cluster Resource Manager template with Diagnostics configuration added to it as part of our Resource Manager template samples.</span></span> <span data-ttu-id="bdb85-144">Du kan se den på den här platsen i galleriet Azure-exempel: [kluster med fem noder med diagnostik Resource Manager mallen exempel](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).</span><span class="sxs-lookup"><span data-stu-id="bdb85-144">You can see it at this location in the Azure Samples gallery: [Five-node cluster with Diagnostics Resource Manager template sample](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).</span></span>

<span data-ttu-id="bdb85-145">Öppna filen azuredeploy.json för att se inställningen diagnostik i Resource Manager-mallen, och Sök efter **IaaSDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="bdb85-145">To see the Diagnostics setting in the Resource Manager template, open the azuredeploy.json file and search for **IaaSDiagnostics**.</span></span> <span data-ttu-id="bdb85-146">Om du vill skapa ett kluster med hjälp av den här mallen, Välj den **till Azure** knappen tillgänglig vid föregående länk.</span><span class="sxs-lookup"><span data-stu-id="bdb85-146">To create a cluster by using this template, select the **Deploy to Azure** button available at the previous link.</span></span>

<span data-ttu-id="bdb85-147">Du kan hämta Resource Manager-exempel, göra ändringar, och skapa ett kluster med den ändrade mallen med hjälp av den `New-AzureRmResourceGroupDeployment` i Azure PowerShell-fönster.</span><span class="sxs-lookup"><span data-stu-id="bdb85-147">Alternatively, you can download the Resource Manager sample, make changes to it, and create a cluster with the modified template by using the `New-AzureRmResourceGroupDeployment` command in an Azure PowerShell window.</span></span> <span data-ttu-id="bdb85-148">Se följande kod för de parametrar som du anger för att kommandot.</span><span class="sxs-lookup"><span data-stu-id="bdb85-148">See the following code for the parameters that you pass in to the command.</span></span> <span data-ttu-id="bdb85-149">Detaljerad information om hur du distribuerar en resursgrupp med hjälp av PowerShell finns i artikeln [distribuera en resursgrupp med Azure Resource Manager-mallen](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="bdb85-149">For detailed information on how to deploy a resource group by using PowerShell, see the article [Deploy a resource group with the Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a><span data-ttu-id="bdb85-150">Distribuera tillägget diagnostik till ett befintligt kluster</span><span class="sxs-lookup"><span data-stu-id="bdb85-150">Deploy the Diagnostics extension to an existing cluster</span></span>
<span data-ttu-id="bdb85-151">Om du har ett befintligt kluster som inte har distribuerats diagnostik eller om du vill ändra en befintlig konfiguration, kan du lägga till eller uppdatera det.</span><span class="sxs-lookup"><span data-stu-id="bdb85-151">If you have an existing cluster that doesn't have Diagnostics deployed, or if you want to modify an existing configuration, you can add or update it.</span></span> <span data-ttu-id="bdb85-152">Ändra Resource Manager-mallen som används för att skapa det befintliga klustret eller hämta mallen från portalen, enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="bdb85-152">Modify the Resource Manager template that's used to create the existing cluster or download the template from the portal as described earlier.</span></span> <span data-ttu-id="bdb85-153">Ändra filen template.json genom att utföra följande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="bdb85-153">Modify the template.json file by performing the following tasks.</span></span>

<span data-ttu-id="bdb85-154">Lägga till en ny storage-resurs i mallen genom att lägga till avsnittet resurser.</span><span class="sxs-lookup"><span data-stu-id="bdb85-154">Add a new storage resource to the template by adding to the resources section.</span></span>

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 <span data-ttu-id="bdb85-155">Lägg till avsnittet Parametrar precis efter storage-konto definitioner, mellan `supportLogStorageAccountName` och `vmNodeType0Name`.</span><span class="sxs-lookup"><span data-stu-id="bdb85-155">Next, add to the parameters section just after the storage account definitions, between `supportLogStorageAccountName` and `vmNodeType0Name`.</span></span> <span data-ttu-id="bdb85-156">Ersätt platshållartexten *lagringskontonamnet här* med namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="bdb85-156">Replace the placeholder text *storage account name goes here* with the name of the storage account.</span></span>

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
<span data-ttu-id="bdb85-157">Sedan uppdatera den `VirtualMachineProfile` i template.json-filen genom att lägga till följande kod i en matris för tillägg.</span><span class="sxs-lookup"><span data-stu-id="bdb85-157">Then, update the `VirtualMachineProfile` section of the template.json file by adding the following code within the extensions array.</span></span> <span data-ttu-id="bdb85-158">Glöm inte att lägga till ett kommatecken i början eller slutet, beroende på om det har infogats.</span><span class="sxs-lookup"><span data-stu-id="bdb85-158">Be sure to add a comma at the beginning or the end, depending on where it's inserted.</span></span>

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

<span data-ttu-id="bdb85-159">När du har ändrat filen template.json enligt publicera Resource Manager-mallen.</span><span class="sxs-lookup"><span data-stu-id="bdb85-159">After you modify the template.json file as described, republish the Resource Manager template.</span></span> <span data-ttu-id="bdb85-160">Om mallen exporterades, publicerar körs filen deploy.ps1 mallen.</span><span class="sxs-lookup"><span data-stu-id="bdb85-160">If the template was exported, running the deploy.ps1 file republishes the template.</span></span> <span data-ttu-id="bdb85-161">När du har distribuerat, se till att **ProvisioningState** är **lyckades**.</span><span class="sxs-lookup"><span data-stu-id="bdb85-161">After you deploy, ensure that **ProvisioningState** is **Succeeded**.</span></span>

## <a name="collect-health-and-load-events"></a><span data-ttu-id="bdb85-162">Samla in hälsa och läsa in händelser</span><span class="sxs-lookup"><span data-stu-id="bdb85-162">Collect health and load events</span></span>

<span data-ttu-id="bdb85-163">Från och med 5.4 för Service Fabric, är hälsotillstånd och Läs in mått händelser tillgängliga för samlingen.</span><span class="sxs-lookup"><span data-stu-id="bdb85-163">Starting with the 5.4 release of Service Fabric, health and load metric events are available for collection.</span></span> <span data-ttu-id="bdb85-164">Dessa händelser återspeglar händelser som genererats av systemet eller din kod med hjälp av hälsotillstånd eller läsa in reporting API: er som [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) eller [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span><span class="sxs-lookup"><span data-stu-id="bdb85-164">These events reflect events generated by the system or your code by using the health or load reporting APIs such as [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) or [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span></span> <span data-ttu-id="bdb85-165">Detta gör för att sammanställa och visa systemhälsa över tid och aviseringar baserat på händelser hälsa eller belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="bdb85-165">This allows for aggregating and viewing system health over time and for alerting based on health or load events.</span></span> <span data-ttu-id="bdb85-166">Visa dessa händelser i Loggboken för Visual Studio diagnostiska lägga till ”Microsoft-ServiceFabric:4:0x4000000000000008” i listan över ETW-providers.</span><span class="sxs-lookup"><span data-stu-id="bdb85-166">To view these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000008" to the list of ETW providers.</span></span>

<span data-ttu-id="bdb85-167">Ändra Resource Manager-mallen ska inkludera för att samla in händelser</span><span class="sxs-lookup"><span data-stu-id="bdb85-167">To collect the events, modify the Resource Manager template to include</span></span>

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387912",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

## <a name="collect-reverse-proxy-events"></a><span data-ttu-id="bdb85-168">Samla in händelser för omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="bdb85-168">Collect reverse proxy events</span></span>

<span data-ttu-id="bdb85-169">Från och med 5.7 för Service Fabric [omvänd proxy](service-fabric-reverseproxy.md) händelser finns tillgängliga för samlingen.</span><span class="sxs-lookup"><span data-stu-id="bdb85-169">Starting with the 5.7 release of Service Fabric, [reverse proxy](service-fabric-reverseproxy.md) events are available for collection.</span></span>
<span data-ttu-id="bdb85-170">Omvänd proxy skickar händelser till två kanaler, något som innehåller felhändelser reflektion fel och den andra som innehåller utförlig händelser för alla begäranden för begäranden som bearbetas på omvänd proxy.</span><span class="sxs-lookup"><span data-stu-id="bdb85-170">Reverse proxy emits events into two channels, one containing error events reflecting request processing failures and the other one containing verbose events about all the requests processed at the reverse proxy.</span></span> 

1. <span data-ttu-id="bdb85-171">Samla in felhändelser: visa dessa händelser i Loggboken för Visual Studio diagnostiska lägga till ”Microsoft-ServiceFabric:4:0x4000000000000010” i listan över ETW-providers.</span><span class="sxs-lookup"><span data-stu-id="bdb85-171">Collect error events: To view these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000010" to the list of ETW providers.</span></span>
<span data-ttu-id="bdb85-172">Ändra Resource Manager-mallen ska inkludera för att samla in händelser från Azure-kluster</span><span class="sxs-lookup"><span data-stu-id="bdb85-172">To collect the events from Azure clusters, modify the Resource Manager template to include</span></span>

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387920",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

2. <span data-ttu-id="bdb85-173">Samla in alla begära att bearbeta händelser: I Visual Studios diagnostiska Loggboken, uppdatera Microsoft-ServiceFabric post i listan ETW-provider till ”Microsoft-ServiceFabric:4:0x4000000000000020”.</span><span class="sxs-lookup"><span data-stu-id="bdb85-173">Collect all request processing events: In Visual Studio's Diagnostic Event Viewer, update the Microsoft-ServiceFabric entry in the ETW provider list to "Microsoft-ServiceFabric:4:0x4000000000000020".</span></span>
<span data-ttu-id="bdb85-174">Ändra resource manager-mall att inkludera för Azure Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="bdb85-174">For Azure Service Fabric clusters, modify the resource manager template to include</span></span>

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387936",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```
> <span data-ttu-id="bdb85-175">Det rekommenderas att aktivera klokt att samla in händelser från den här kanalen som detta samlar in all trafik via omvänd proxy och snabbt förbruka lagringskapacitet.</span><span class="sxs-lookup"><span data-stu-id="bdb85-175">It is recommended to judiciously enable collecting events from this channel as this collects all traffic through the reverse proxy and can quickly consume storage capacity.</span></span>

<span data-ttu-id="bdb85-176">I Azure Service Fabric-kluster är samlas in och samman i SystemEventTable händelser från alla noder.</span><span class="sxs-lookup"><span data-stu-id="bdb85-176">For Azure Service Fabric clusters, the events from all the nodes are collected and aggregated in the SystemEventTable.</span></span>
<span data-ttu-id="bdb85-177">För detaljerad felsökning av händelser för omvänd proxy, finns det [omvänd proxy diagnostik guiden](service-fabric-reverse-proxy-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="bdb85-177">For detailed troubleshooting of the reverse proxy events, refer the [reverse proxy diagnostics guide](service-fabric-reverse-proxy-diagnostics.md).</span></span>

## <a name="collect-from-new-eventsource-channels"></a><span data-ttu-id="bdb85-178">Samla in från den nya EventSource kanaler</span><span class="sxs-lookup"><span data-stu-id="bdb85-178">Collect from new EventSource channels</span></span>

<span data-ttu-id="bdb85-179">Om du vill uppdatera diagnostik för att samla in loggar från nya EventSource kanaler som representerar ett nytt program som du använder om att distribuera, utföra samma kluster steg enligt beskrivningen ovan för att installationen av diagnostik för ett befintligt.</span><span class="sxs-lookup"><span data-stu-id="bdb85-179">To update Diagnostics to collect logs from new EventSource channels that represent a new application that you're about to deploy, perform the same steps as previously described for the setup of Diagnostics for an existing cluster.</span></span>

<span data-ttu-id="bdb85-180">Uppdatera den `EtwEventSourceProviderConfiguration` -avsnittet i template.json att lägga till poster för de nya EventSource kanalerna innan du använder konfigurationen uppdatera med hjälp av den `New-AzureRmResourceGroupDeployment` PowerShell-kommando.</span><span class="sxs-lookup"><span data-stu-id="bdb85-180">Update the `EtwEventSourceProviderConfiguration` section in the template.json file to add entries for the new EventSource channels before you apply the configuration update by using the `New-AzureRmResourceGroupDeployment` PowerShell command.</span></span> <span data-ttu-id="bdb85-181">Namnet på händelsekällan definieras som en del av din kod i Visual Studio-genererade ServiceEventSource.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="bdb85-181">The name of the event source is defined as part of your code in the Visual Studio-generated ServiceEventSource.cs file.</span></span>

<span data-ttu-id="bdb85-182">Till exempel om din händelsekällan är Mina Eventsource, lägger du till följande kod för att placera händelser från Mina Eventsource i en tabell med namnet MyDestinationTableName.</span><span class="sxs-lookup"><span data-stu-id="bdb85-182">For example, if your event source is named My-Eventsource, add the following code to place the events from My-Eventsource into a table named MyDestinationTableName.</span></span>

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

<span data-ttu-id="bdb85-183">Om du vill samla in prestandaräknare eller händelseloggar, ändra Resource Manager-mallen med hjälp av exemplen i [skapa en virtuell Windows-dator med övervakning och diagnostik med en Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bdb85-183">To collect performance counters or event logs, modify the Resource Manager template by using the examples provided in [Create a Windows virtual machine with monitoring and diagnostics by using an Azure Resource Manager template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="bdb85-184">Kubfilen Resource Manager-mallen.</span><span class="sxs-lookup"><span data-stu-id="bdb85-184">Then, republish the Resource Manager template.</span></span>

## <a name="collect-performance-counters"></a><span data-ttu-id="bdb85-185">Samla in prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="bdb85-185">Collect Performance Counters</span></span>

<span data-ttu-id="bdb85-186">Lägg till prestandaräknare till din ”WadCfg > DiagnosticMonitorConfiguration” i Resource Manager-mall för klustret om du vill samla in prestandavärden från klustret.</span><span class="sxs-lookup"><span data-stu-id="bdb85-186">To collect performance metrics from your cluster, add the performance counters to your "WadCfg > DiagnosticMonitorConfiguration" in the Resource Manager template for your cluster.</span></span> <span data-ttu-id="bdb85-187">Se [prestandaräknare för Service Fabric](service-fabric-diagnostics-event-generation-perf.md) för prestandaräknare som rekommenderar vi att samla in.</span><span class="sxs-lookup"><span data-stu-id="bdb85-187">See [Service Fabric Performance Counters](service-fabric-diagnostics-event-generation-perf.md) for performance counters that we recommend collecting.</span></span>

<span data-ttu-id="bdb85-188">Exempelvis här vi ställer in en prestandaräknare Sampla var 15: e sekund (Detta kan ändras och följer formatet ”PT\<tid >\<enhet >”, till exempel PT3M skulle prov på tre minuters mellanrum), och överförs till lagringstabellen lämplig var en minut.</span><span class="sxs-lookup"><span data-stu-id="bdb85-188">For example, here we set one performance counter, sampled every 15 seconds (this can be changed and follows the format of "PT\<time>\<unit>", for example, PT3M would sample at three minute intervals), and transferred to the appropriate storage table every one minute.</span></span>

  ```json
  "PerformanceCounters": {
      "scheduledTransferPeriod": "PT1M",
      "PerformanceCounterConfiguration": [
          {
              "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
              "sampleRate": "PT15S",
              "unit": "Percent",
              "annotation": [
              ],
              "sinks": ""
          }
      ]
  }
  ```
  
<span data-ttu-id="bdb85-189">Om du använder en Application Insights-sink enligt beskrivningen i avsnittet nedan och vill använda de här måtten ska visas i Application Insights, se till att lägga till mottagare namn i avsnittet ”sänkor” som ovan.</span><span class="sxs-lookup"><span data-stu-id="bdb85-189">If you are using an Application Insights sink, as described in the section below, and want these metrics to show up in Application Insights, then make sure to add the sink name in the "sinks" section as shown above.</span></span> <span data-ttu-id="bdb85-190">Dessutom kan du skapa en separat tabell om du vill skicka dina prestandaräknare till, så att de inte tillräckligt med utrymme ut data från de andra loggning kanaler som du har aktiverat.</span><span class="sxs-lookup"><span data-stu-id="bdb85-190">Additionally, consider creating a separate table to send your Performance Counters to, so they don't crowd out the data coming from the other logging channels you have enabled.</span></span>


## <a name="send-logs-to-application-insights"></a><span data-ttu-id="bdb85-191">Skicka loggar till Application Insights</span><span class="sxs-lookup"><span data-stu-id="bdb85-191">Send logs to Application Insights</span></span>

<span data-ttu-id="bdb85-192">Övervakning och diagnostik data skickas till Application Insights (AI) kan göras som en del av konfigurationen BOMULLSTUSS.</span><span class="sxs-lookup"><span data-stu-id="bdb85-192">Sending monitoring and diagnostics data to Application Insights (AI) can be done as part of the WAD configuration.</span></span> <span data-ttu-id="bdb85-193">Om du vill använda AI för händelseanalys och visualisering läsa [Händelseanalys och visualisering med Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) att ställa in en AI Sink som en del av din ”WadCfg”.</span><span class="sxs-lookup"><span data-stu-id="bdb85-193">If you decide to use AI for event analysis and visualization, read [Event Analysis and Visualization with Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) to set up an AI Sink as part of your "WadCfg".</span></span>

## <a name="next-steps"></a><span data-ttu-id="bdb85-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bdb85-194">Next steps</span></span>

<span data-ttu-id="bdb85-195">När du har korrekt konfigurerat Azure diagnostics, visas data i Storage-tabeller från ETW och EventSource loggar.</span><span class="sxs-lookup"><span data-stu-id="bdb85-195">Once you have correctly configured Azure diagnostics, you will see data in your Storage tables from the ETW and EventSource logs.</span></span> <span data-ttu-id="bdb85-196">Om du väljer att använda OMS Kibana eller några andra analyser och visualisering dataplattform som inte har konfigurerats i Resource Manager-mallen direkt att se till att ställa in plattformen du väljer att läsa data från dessa lagringstabeller.</span><span class="sxs-lookup"><span data-stu-id="bdb85-196">If you choose to use OMS, Kibana, or any other data analytics and visualization platform that is not directly configured in the Resource Manager template, make sure to set up the platform of your choice to read in the data from these storage tables.</span></span> <span data-ttu-id="bdb85-197">Om du gör detta för OMS är relativt enkelt och förklaras i [händelse och logg analys genom OMS](service-fabric-diagnostics-event-analysis-oms.md).</span><span class="sxs-lookup"><span data-stu-id="bdb85-197">Doing this for OMS is relatively trivial, and is explained in [Event and log analysis through OMS](service-fabric-diagnostics-event-analysis-oms.md).</span></span> <span data-ttu-id="bdb85-198">Application Insights är lite specialfall på detta sätt, eftersom den kan konfigureras som en del av diagnostik tilläggets konfiguration, så finns det [lämplig artikel](service-fabric-diagnostics-event-analysis-appinsights.md) om du väljer att använda AI.</span><span class="sxs-lookup"><span data-stu-id="bdb85-198">Application Insights is a bit of a special case in this sense, since it can be configured as part of the Diagnostics extension configuration, so refer to the [appropriate article](service-fabric-diagnostics-event-analysis-appinsights.md) if you choose to use AI.</span></span>

>[!NOTE]
><span data-ttu-id="bdb85-199">Det finns för närvarande inget sätt att filtrera eller rensa de händelser som skickas till tabellen.</span><span class="sxs-lookup"><span data-stu-id="bdb85-199">There is currently no way to filter or groom the events that are sent to the table.</span></span> <span data-ttu-id="bdb85-200">Om du inte implementerar en process för att ta bort händelser från tabellen, i tabellen kommer att fortsätta att växa.</span><span class="sxs-lookup"><span data-stu-id="bdb85-200">If you don't implement a process to remove events from the table, the table will continue to grow.</span></span> <span data-ttu-id="bdb85-201">För närvarande finns ett exempel på en rensning datatjänst som körs i den [Watchdog exempel](https://github.com/Azure-Samples/service-fabric-watchdog-service), och det rekommenderas att du skriver en själv, såvida det inte finns en anledning att lagra loggar utöver en tidsram för 30 eller 90 dagar.</span><span class="sxs-lookup"><span data-stu-id="bdb85-201">Currently, there is an example of a data grooming service running in the [Watchdog sample](https://github.com/Azure-Samples/service-fabric-watchdog-service), and it is recommended that you write one for yourself as well, unless there is a good reason for you to store logs beyond a 30 or 90 day timeframe.</span></span>

* [<span data-ttu-id="bdb85-202">Lär dig att samla in prestandaräknare eller loggar med hjälp av diagnostik-tillägget</span><span class="sxs-lookup"><span data-stu-id="bdb85-202">Learn how to collect performance counters or logs by using the Diagnostics extension</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="bdb85-203">Händelseanalys och visualisering med Application Insights</span><span class="sxs-lookup"><span data-stu-id="bdb85-203">Event Analysis and Visualization with Application Insights</span></span>](service-fabric-diagnostics-event-analysis-appinsights.md)
* [<span data-ttu-id="bdb85-204">Händelseanalys och visualisering med OMS</span><span class="sxs-lookup"><span data-stu-id="bdb85-204">Event Analysis and Visualization with OMS</span></span>](service-fabric-diagnostics-event-analysis-oms.md)