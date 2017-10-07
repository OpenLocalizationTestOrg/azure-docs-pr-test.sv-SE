---
title: "aaaAzure Service Fabric händelse aggregeringen med Windows Azure-diagnostik | Microsoft Docs"
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
ms.openlocfilehash: 4827ce66620e61c5b4a8682db55952333113188a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a><span data-ttu-id="32499-103">Aggregering av händelse och med Windows Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="32499-103">Event aggregation and collection using Windows Azure Diagnostics</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="32499-104">Windows</span><span class="sxs-lookup"><span data-stu-id="32499-104">Windows</span></span>](service-fabric-diagnostics-event-aggregation-wad.md)
> * [<span data-ttu-id="32499-105">Linux</span><span class="sxs-lookup"><span data-stu-id="32499-105">Linux</span></span>](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

<span data-ttu-id="32499-106">När du kör ett Azure Service Fabric-kluster, är det en bra idé toocollect hello loggar från alla hello noder på en central plats.</span><span class="sxs-lookup"><span data-stu-id="32499-106">When you're running an Azure Service Fabric cluster, it's a good idea toocollect hello logs from all hello nodes in a central location.</span></span> <span data-ttu-id="32499-107">Med hello loggar på en central plats hjälper dig att analysera och felsöka problem i klustret eller problem i hello program och tjänster som körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="32499-107">Having hello logs in a central location helps you analyze and troubleshoot issues in your cluster, or issues in hello applications and services running in that cluster.</span></span>

<span data-ttu-id="32499-108">Ett sätt tooupload och samla in loggar är toouse hello Windows Azure Diagnostics (BOMULLSTUSS)-tillägg, som överför loggar tooAzure lagring och har dessutom hello alternativet toosend loggar tooAzure Application Insights eller Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="32499-108">One way tooupload and collect logs is toouse hello Windows Azure Diagnostics (WAD) extension, which uploads logs tooAzure Storage, and also has hello option toosend logs tooAzure Application Insights or Event Hubs.</span></span> <span data-ttu-id="32499-109">Du kan också använda en extern process tooread hello händelser från lagring och placerar dem på en analysis plattform produkt som [OMS logganalys](../log-analytics/log-analytics-service-fabric.md) eller en annan lösning för parsning av loggen.</span><span class="sxs-lookup"><span data-stu-id="32499-109">You can also use an external process tooread hello events from storage and place them in an analysis platform product, such as [OMS Log Analytics](../log-analytics/log-analytics-service-fabric.md) or another log-parsing solution.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="32499-110">Krav</span><span class="sxs-lookup"><span data-stu-id="32499-110">Prerequisites</span></span>
<span data-ttu-id="32499-111">Dessa verktyg är används tooperform vissa hello åtgärder i det här dokumentet:</span><span class="sxs-lookup"><span data-stu-id="32499-111">These tools are used tooperform some of hello operations in this document:</span></span>

* <span data-ttu-id="32499-112">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (relaterade tooAzure molntjänster men har bra information och exempel)</span><span class="sxs-lookup"><span data-stu-id="32499-112">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (related tooAzure Cloud Services but has good information and examples)</span></span>
* [<span data-ttu-id="32499-113">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="32499-113">Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="32499-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="32499-114">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="32499-115">Azure Resource Manager-klienten</span><span class="sxs-lookup"><span data-stu-id="32499-115">Azure Resource Manager client</span></span>](https://github.com/projectkudu/ARMClient)
* [<span data-ttu-id="32499-116">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="32499-116">Azure Resource Manager template</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-and-event-sources"></a><span data-ttu-id="32499-117">Loggen och händelsen källor</span><span class="sxs-lookup"><span data-stu-id="32499-117">Log and event sources</span></span>

### <a name="service-fabric-platform-events"></a><span data-ttu-id="32499-118">Service Fabric-plattformen händelser</span><span class="sxs-lookup"><span data-stu-id="32499-118">Service Fabric platform events</span></span>
<span data-ttu-id="32499-119">Enligt beskrivningen i [i den här artikeln](service-fabric-diagnostics-event-generation-infra.md)Service Fabric ställer du in med några out box loggning kanaler, av vilka hello följande kanaler enkelt konfigurerats med BOMULLSTUSS toosend övervakning och diagnostik tooa lagring datatabell eller någon annanstans:</span><span class="sxs-lookup"><span data-stu-id="32499-119">As discussed in [this article](service-fabric-diagnostics-event-generation-infra.md), Service Fabric sets you up with a few out-of-the-box logging channels, of which hello following channels are easily configured with WAD toosend monitoring and diagnostics data tooa storage table or elsewhere:</span></span>
  * <span data-ttu-id="32499-120">Operativa händelser: att åtgärder som hello Service Fabric-plattformen utför.</span><span class="sxs-lookup"><span data-stu-id="32499-120">Operational events: higher-level operations that hello Service Fabric platform performs.</span></span> <span data-ttu-id="32499-121">Exempel: skapa program och tjänster, nod tillståndsändringar och information om uppgradering.</span><span class="sxs-lookup"><span data-stu-id="32499-121">Examples include creation of applications and services, node state changes, and upgrade information.</span></span> <span data-ttu-id="32499-122">Dessa orsakat som loggar händelsen ETW Tracing for Windows)</span><span class="sxs-lookup"><span data-stu-id="32499-122">These are emitted as Event Tracing for Windows (ETW) logs</span></span>
  * [<span data-ttu-id="32499-123">Reliable Actors programmering modellen händelser</span><span class="sxs-lookup"><span data-stu-id="32499-123">Reliable Actors programming model events</span></span>](service-fabric-reliable-actors-diagnostics.md)
  * [<span data-ttu-id="32499-124">Reliable Services programming modellen händelser</span><span class="sxs-lookup"><span data-stu-id="32499-124">Reliable Services programming model events</span></span>](service-fabric-reliable-services-diagnostics.md)

### <a name="application-events"></a><span data-ttu-id="32499-125">Programhändelser</span><span class="sxs-lookup"><span data-stu-id="32499-125">Application events</span></span>
 <span data-ttu-id="32499-126">Händelser som orsakat från dina program och tjänster kod och skrivits hello EventSource hjälpklass som angetts i hello Visual Studio-mallar.</span><span class="sxs-lookup"><span data-stu-id="32499-126">Events emitted from your applications' and services' code and written out by using hello EventSource helper class provided in hello Visual Studio templates.</span></span> <span data-ttu-id="32499-127">Mer information om hur toowrite EventSource loggar från ditt program finns [övervaka och diagnostisera tjänster i en inställning för lokal dator development](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="32499-127">For more information on how toowrite EventSource logs from your application, see [Monitor and diagnose services in a local machine development setup](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>

## <a name="deploy-hello-diagnostics-extension"></a><span data-ttu-id="32499-128">Distribuera hello diagnostik tillägg</span><span class="sxs-lookup"><span data-stu-id="32499-128">Deploy hello Diagnostics extension</span></span>
<span data-ttu-id="32499-129">hello första steget i att samla in loggar är toodeploy hello diagnostik tillägg på varje hello virtuella datorer i hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="32499-129">hello first step in collecting logs is toodeploy hello Diagnostics extension on each of hello VMs in hello Service Fabric cluster.</span></span> <span data-ttu-id="32499-130">hello diagnostik tillägget samlar in loggar på varje virtuell dator och överför dem toohello storage-konto som du anger.</span><span class="sxs-lookup"><span data-stu-id="32499-130">hello Diagnostics extension collects logs on each VM and uploads them toohello storage account that you specify.</span></span> <span data-ttu-id="32499-131">hello steg variera något beroende på om du använder hello Azure-portalen eller Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="32499-131">hello steps vary a little based on whether you use hello Azure portal or Azure Resource Manager.</span></span> <span data-ttu-id="32499-132">hello stegen variera beroende på om hello distribution är en del av klustret har skapats eller är i ett kluster som redan finns.</span><span class="sxs-lookup"><span data-stu-id="32499-132">hello steps also vary based on whether hello deployment is part of cluster creation or is for a cluster that already exists.</span></span> <span data-ttu-id="32499-133">Nu ska vi titta på hello steg för varje scenario.</span><span class="sxs-lookup"><span data-stu-id="32499-133">Let's look at hello steps for each scenario.</span></span>

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a><span data-ttu-id="32499-134">Distribuera hello diagnostik tillägget som en del av klustret har skapats via Azure portal</span><span class="sxs-lookup"><span data-stu-id="32499-134">Deploy hello Diagnostics extension as part of cluster creation through Azure portal</span></span>
<span data-ttu-id="32499-135">toodeploy hello diagnostik tillägget toohello virtuella datorer i hello klustret som en del av klustret skapas, som du använder hello diagnostik inställningar panelens framgår hello följande bild – Kontrollera att diagnostik är inställd för**på** (hello standardinställningen) .</span><span class="sxs-lookup"><span data-stu-id="32499-135">toodeploy hello Diagnostics extension toohello VMs in hello cluster as part of cluster creation, you use hello Diagnostics settings panel shown in hello following image - ensure that Diagnostics is set too**On** (hello default setting).</span></span> <span data-ttu-id="32499-136">När du har skapat hello klustret kan du inte ändra dessa inställningar med hjälp av hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="32499-136">After you create hello cluster, you can't change these settings by using hello portal.</span></span>

![Azure diagnostikinställningar i hello-portalen för att skapa klustret](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics.png)

<span data-ttu-id="32499-138">När du skapar ett kluster med hjälp av hello portal, vi rekommenderar starkt att du hämtar hello mall **innan du klickar på OK** toocreate hello klustret.</span><span class="sxs-lookup"><span data-stu-id="32499-138">When you're creating a cluster by using hello portal, we highly recommend that you download hello template **before you click OK** toocreate hello cluster.</span></span> <span data-ttu-id="32499-139">Mer information finns för[ställer in ett Service Fabric-kluster med en Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="32499-139">For details, refer too[Set up a Service Fabric cluster by using an Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="32499-140">Du behöver hello malländringar toomake senare, eftersom du inte kan göra några ändringar med hjälp av hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="32499-140">You'll need hello template toomake changes later, because you can't make some changes by using hello portal.</span></span>

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a><span data-ttu-id="32499-141">Distribuera hello diagnostik tillägget som en del av klustret har skapats med hjälp av Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="32499-141">Deploy hello Diagnostics extension as part of cluster creation by using Azure Resource Manager</span></span>
<span data-ttu-id="32499-142">toocreate ett kluster med hjälp av hanteraren för filserverresurser måste tooadd hello diagnostik configuration JSON toohello fullständig klustret Resource Manager-mallen innan du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="32499-142">toocreate a cluster by using Resource Manager, you need tooadd hello Diagnostics configuration JSON toohello full cluster Resource Manager template before you create hello cluster.</span></span> <span data-ttu-id="32499-143">Vi tillhandahåller en fem-VM klustret Resource Manager exempelmall med diagnostik konfiguration läggs tooit som en del av våra exempel för Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="32499-143">We provide a sample five-VM cluster Resource Manager template with Diagnostics configuration added tooit as part of our Resource Manager template samples.</span></span> <span data-ttu-id="32499-144">Du kan se den på den här platsen i galleriet för hello Azure-exempel: [kluster med fem noder med diagnostik Resource Manager mallen exempel](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).</span><span class="sxs-lookup"><span data-stu-id="32499-144">You can see it at this location in hello Azure Samples gallery: [Five-node cluster with Diagnostics Resource Manager template sample](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).</span></span>

<span data-ttu-id="32499-145">toosee hello diagnostik inställningen i hello Resource Manager-mall, öppna hello azuredeploy.json filen och Sök efter **IaaSDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="32499-145">toosee hello Diagnostics setting in hello Resource Manager template, open hello azuredeploy.json file and search for **IaaSDiagnostics**.</span></span> <span data-ttu-id="32499-146">toocreate ett kluster med hjälp av den här mallen, Välj hello **distribuera tooAzure** knappen på hello föregående länk.</span><span class="sxs-lookup"><span data-stu-id="32499-146">toocreate a cluster by using this template, select hello **Deploy tooAzure** button available at hello previous link.</span></span>

<span data-ttu-id="32499-147">Du kan också du kan hämta hello Resource Manager exempel, göra ändringar tooit och skapa ett kluster med hello ändrade mallen med hjälp av hello `New-AzureRmResourceGroupDeployment` i Azure PowerShell-fönster.</span><span class="sxs-lookup"><span data-stu-id="32499-147">Alternatively, you can download hello Resource Manager sample, make changes tooit, and create a cluster with hello modified template by using hello `New-AzureRmResourceGroupDeployment` command in an Azure PowerShell window.</span></span> <span data-ttu-id="32499-148">Se följande kod för hello-parametrar som du skickar i toohello kommandot hello.</span><span class="sxs-lookup"><span data-stu-id="32499-148">See hello following code for hello parameters that you pass in toohello command.</span></span> <span data-ttu-id="32499-149">Detaljerad information om hur toodeploy en resurs gruppen med hjälp av PowerShell finns i artikeln hello [distribuera en resursgrupp med hello Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="32499-149">For detailed information on how toodeploy a resource group by using PowerShell, see hello article [Deploy a resource group with hello Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

### <a name="deploy-hello-diagnostics-extension-tooan-existing-cluster"></a><span data-ttu-id="32499-150">Distribuera hello diagnostik tillägget tooan befintligt kluster</span><span class="sxs-lookup"><span data-stu-id="32499-150">Deploy hello Diagnostics extension tooan existing cluster</span></span>
<span data-ttu-id="32499-151">Om du har ett befintligt kluster som inte har distribuerats diagnostik, eller om du vill toomodify en befintlig konfiguration, kan du lägga till eller uppdatera det.</span><span class="sxs-lookup"><span data-stu-id="32499-151">If you have an existing cluster that doesn't have Diagnostics deployed, or if you want toomodify an existing configuration, you can add or update it.</span></span> <span data-ttu-id="32499-152">Ändra hello Resource Manager-mall som används toocreate hello befintligt kluster eller hämta hello mallen från hello portal som tidigare beskrivits.</span><span class="sxs-lookup"><span data-stu-id="32499-152">Modify hello Resource Manager template that's used toocreate hello existing cluster or download hello template from hello portal as described earlier.</span></span> <span data-ttu-id="32499-153">Ändra hello template.json filen genom att utföra följande uppgifter hello.</span><span class="sxs-lookup"><span data-stu-id="32499-153">Modify hello template.json file by performing hello following tasks.</span></span>

<span data-ttu-id="32499-154">Lägga till en ny mall för storage resource toohello genom att lägga till toohello resurser avsnitt.</span><span class="sxs-lookup"><span data-stu-id="32499-154">Add a new storage resource toohello template by adding toohello resources section.</span></span>

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

 <span data-ttu-id="32499-155">Lägg sedan till avsnittet toohello parametrar precis efter hello storage-konto definitioner mellan `supportLogStorageAccountName` och `vmNodeType0Name`.</span><span class="sxs-lookup"><span data-stu-id="32499-155">Next, add toohello parameters section just after hello storage account definitions, between `supportLogStorageAccountName` and `vmNodeType0Name`.</span></span> <span data-ttu-id="32499-156">Ersätt hello platshållartexten *lagringskontonamnet här* med hello namnet hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="32499-156">Replace hello placeholder text *storage account name goes here* with hello name of hello storage account.</span></span>

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for hello application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for hello storage account that contains application diagnostics data from hello cluster"
      }
    },
```
<span data-ttu-id="32499-157">Sedan uppdatera hello `VirtualMachineProfile` i hello template.json filen genom att lägga till följande kod i hello tillägg matris hello.</span><span class="sxs-lookup"><span data-stu-id="32499-157">Then, update hello `VirtualMachineProfile` section of hello template.json file by adding hello following code within hello extensions array.</span></span> <span data-ttu-id="32499-158">Vara säker på att tooadd kommatecken i hello början eller slutet av hello, beroende på om det har infogats.</span><span class="sxs-lookup"><span data-stu-id="32499-158">Be sure tooadd a comma at hello beginning or hello end, depending on where it's inserted.</span></span>

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

<span data-ttu-id="32499-159">När du ändrar hello template.json enligt kan publicera hello Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="32499-159">After you modify hello template.json file as described, republish hello Resource Manager template.</span></span> <span data-ttu-id="32499-160">Om hello mallen exporterades publicerar kör hello deploy.ps1 filen hello mallen.</span><span class="sxs-lookup"><span data-stu-id="32499-160">If hello template was exported, running hello deploy.ps1 file republishes hello template.</span></span> <span data-ttu-id="32499-161">När du har distribuerat, se till att **ProvisioningState** är **lyckades**.</span><span class="sxs-lookup"><span data-stu-id="32499-161">After you deploy, ensure that **ProvisioningState** is **Succeeded**.</span></span>

## <a name="collect-health-and-load-events"></a><span data-ttu-id="32499-162">Samla in hälsa och läsa in händelser</span><span class="sxs-lookup"><span data-stu-id="32499-162">Collect health and load events</span></span>

<span data-ttu-id="32499-163">Från och med hello 5.4 versionen av Service Fabric, är hälsotillstånd och Läs in mått händelser tillgängliga för samlingen.</span><span class="sxs-lookup"><span data-stu-id="32499-163">Starting with hello 5.4 release of Service Fabric, health and load metric events are available for collection.</span></span> <span data-ttu-id="32499-164">Dessa händelser återspeglar händelser som genererats av systemet hello eller din kod med hjälp av hello hälsa eller läsa in reporting API: er som [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) eller [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span><span class="sxs-lookup"><span data-stu-id="32499-164">These events reflect events generated by hello system or your code by using hello health or load reporting APIs such as [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) or [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span></span> <span data-ttu-id="32499-165">Detta gör för att sammanställa och visa systemhälsa över tid och aviseringar baserat på händelser hälsa eller belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="32499-165">This allows for aggregating and viewing system health over time and for alerting based on health or load events.</span></span> <span data-ttu-id="32499-166">tooview lägga till dessa händelser i Loggboken i Visual Studio-diagnostik ”Microsoft-ServiceFabric:4:0x4000000000000008” toohello lista över ETW-providers.</span><span class="sxs-lookup"><span data-stu-id="32499-166">tooview these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000008" toohello list of ETW providers.</span></span>

<span data-ttu-id="32499-167">toocollect hello händelser, ändra hello Resource Manager-mall tooinclude</span><span class="sxs-lookup"><span data-stu-id="32499-167">toocollect hello events, modify hello Resource Manager template tooinclude</span></span>

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

## <a name="collect-reverse-proxy-events"></a><span data-ttu-id="32499-168">Samla in händelser för omvänd proxy</span><span class="sxs-lookup"><span data-stu-id="32499-168">Collect reverse proxy events</span></span>

<span data-ttu-id="32499-169">Från och med hello 5.7 versionen av Service Fabric [omvänd proxy](service-fabric-reverseproxy.md) händelser finns tillgängliga för samlingen.</span><span class="sxs-lookup"><span data-stu-id="32499-169">Starting with hello 5.7 release of Service Fabric, [reverse proxy](service-fabric-reverseproxy.md) events are available for collection.</span></span>
<span data-ttu-id="32499-170">Omvänd proxy skickar händelser till två kanaler, ett som innehåller fel händelser reflektion begära bearbetningsfel och hello andra ett som innehåller utförlig händelser om alla hello-begäranden som bearbetas på hello omvänd proxy.</span><span class="sxs-lookup"><span data-stu-id="32499-170">Reverse proxy emits events into two channels, one containing error events reflecting request processing failures and hello other one containing verbose events about all hello requests processed at hello reverse proxy.</span></span> 

1. <span data-ttu-id="32499-171">Samla in felhändelser: tooview lägga till dessa händelser i Loggboken i Visual Studio-diagnostik ”Microsoft-ServiceFabric:4:0x4000000000000010” toohello lista över ETW-providers.</span><span class="sxs-lookup"><span data-stu-id="32499-171">Collect error events: tooview these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000010" toohello list of ETW providers.</span></span>
<span data-ttu-id="32499-172">toocollect hello händelser från Azure kluster, ändra hello Resource Manager-mall tooinclude</span><span class="sxs-lookup"><span data-stu-id="32499-172">toocollect hello events from Azure clusters, modify hello Resource Manager template tooinclude</span></span>

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

2. <span data-ttu-id="32499-173">Samla in alla begära att bearbeta händelser: I Visual Studios diagnostiska Loggboken, uppdatering hello Microsoft ServiceFabric post i hello ETW providerlistan för ”Microsoft-ServiceFabric:4:0x4000000000000020”.</span><span class="sxs-lookup"><span data-stu-id="32499-173">Collect all request processing events: In Visual Studio's Diagnostic Event Viewer, update hello Microsoft-ServiceFabric entry in hello ETW provider list too"Microsoft-ServiceFabric:4:0x4000000000000020".</span></span>
<span data-ttu-id="32499-174">Ändra hello resource manager-mall tooinclude för Azure Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="32499-174">For Azure Service Fabric clusters, modify hello resource manager template tooinclude</span></span>

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
> <span data-ttu-id="32499-175">Det rekommenderas toojudiciously aktivera att samla in händelser från den här kanalen som detta samlar in all trafik via omvänd proxy för hello och snabbt förbruka lagringskapacitet.</span><span class="sxs-lookup"><span data-stu-id="32499-175">It is recommended toojudiciously enable collecting events from this channel as this collects all traffic through hello reverse proxy and can quickly consume storage capacity.</span></span>

<span data-ttu-id="32499-176">I Azure Service Fabric-kluster är samlas in och aggregeras i hello SystemEventTable hello händelser från alla hello-noder.</span><span class="sxs-lookup"><span data-stu-id="32499-176">For Azure Service Fabric clusters, hello events from all hello nodes are collected and aggregated in hello SystemEventTable.</span></span>
<span data-ttu-id="32499-177">För detaljerad felsökning av hello omvänd proxyhändelser, se hello [omvänd proxy diagnostik guiden](service-fabric-reverse-proxy-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="32499-177">For detailed troubleshooting of hello reverse proxy events, refer hello [reverse proxy diagnostics guide](service-fabric-reverse-proxy-diagnostics.md).</span></span>

## <a name="collect-from-new-eventsource-channels"></a><span data-ttu-id="32499-178">Samla in från den nya EventSource kanaler</span><span class="sxs-lookup"><span data-stu-id="32499-178">Collect from new EventSource channels</span></span>

<span data-ttu-id="32499-179">tooupdate diagnostik toocollect loggar från nya EventSource kanaler som representerar ett nytt program som du är om toodeploy, utföra hello samma steg som tidigare angivits för hello inställning av diagnostik för ett befintligt kluster.</span><span class="sxs-lookup"><span data-stu-id="32499-179">tooupdate Diagnostics toocollect logs from new EventSource channels that represent a new application that you're about toodeploy, perform hello same steps as previously described for hello setup of Diagnostics for an existing cluster.</span></span>

<span data-ttu-id="32499-180">Uppdatera hello `EtwEventSourceProviderConfiguration` under hello template.json tooadd poster för hello nya EventSource kanaler innan du använder hello konfigurationen uppdatera med hjälp av hello `New-AzureRmResourceGroupDeployment` PowerShell-kommando.</span><span class="sxs-lookup"><span data-stu-id="32499-180">Update hello `EtwEventSourceProviderConfiguration` section in hello template.json file tooadd entries for hello new EventSource channels before you apply hello configuration update by using hello `New-AzureRmResourceGroupDeployment` PowerShell command.</span></span> <span data-ttu-id="32499-181">hello händelsekälla hello namn har definierats som en del av din kod i hello Visual Studio-genererade ServiceEventSource.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="32499-181">hello name of hello event source is defined as part of your code in hello Visual Studio-generated ServiceEventSource.cs file.</span></span>

<span data-ttu-id="32499-182">T.ex, om din händelsekälla heter Mina Eventsource, lägger du till följande kod tooplace hello händelser från Mina Eventsource i en tabell med namnet MyDestinationTableName hello.</span><span class="sxs-lookup"><span data-stu-id="32499-182">For example, if your event source is named My-Eventsource, add hello following code tooplace hello events from My-Eventsource into a table named MyDestinationTableName.</span></span>

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

<span data-ttu-id="32499-183">toocollect prestandaräknare eller händelseloggar, ändra hello Resource Manager-mall med hjälp av hello-exempel finns i [skapa en virtuell Windows-dator med övervakning och diagnostik med en Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="32499-183">toocollect performance counters or event logs, modify hello Resource Manager template by using hello examples provided in [Create a Windows virtual machine with monitoring and diagnostics by using an Azure Resource Manager template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="32499-184">Kubfilen hello Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="32499-184">Then, republish hello Resource Manager template.</span></span>

## <a name="collect-performance-counters"></a><span data-ttu-id="32499-185">Samla in prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="32499-185">Collect Performance Counters</span></span>

<span data-ttu-id="32499-186">toocollect prestandamått från ditt kluster, lägga till hello prestandaräknare tooyour ”WadCfg > DiagnosticMonitorConfiguration” i hello Resource Manager-mall för klustret.</span><span class="sxs-lookup"><span data-stu-id="32499-186">toocollect performance metrics from your cluster, add hello performance counters tooyour "WadCfg > DiagnosticMonitorConfiguration" in hello Resource Manager template for your cluster.</span></span> <span data-ttu-id="32499-187">Se [prestandaräknare för Service Fabric](service-fabric-diagnostics-event-generation-perf.md) för prestandaräknare som rekommenderar vi att samla in.</span><span class="sxs-lookup"><span data-stu-id="32499-187">See [Service Fabric Performance Counters](service-fabric-diagnostics-event-generation-perf.md) for performance counters that we recommend collecting.</span></span>

<span data-ttu-id="32499-188">Exempelvis här vi ställer in en prestandaräknare Sampla var 15: e sekund (Detta kan ändras och sätt hello format ”PT\<tid >\<enhet >”, till exempel PT3M skulle prov på tre minuters mellanrum), och överförs toohello lämplig lagringstabellen var en minut.</span><span class="sxs-lookup"><span data-stu-id="32499-188">For example, here we set one performance counter, sampled every 15 seconds (this can be changed and follows hello format of "PT\<time>\<unit>", for example, PT3M would sample at three minute intervals), and transferred toohello appropriate storage table every one minute.</span></span>

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
  
<span data-ttu-id="32499-189">Om du använder en Application Insights-sink enligt beskrivningen i hello nedan och vill att dessa mått tooshow i Application Insights, gör du till tooadd hello sink namn i hello ”sänkor” avsnittet som ovan.</span><span class="sxs-lookup"><span data-stu-id="32499-189">If you are using an Application Insights sink, as described in hello section below, and want these metrics tooshow up in Application Insights, then make sure tooadd hello sink name in hello "sinks" section as shown above.</span></span> <span data-ttu-id="32499-190">Dessutom bör du skapa en separat tabell toosend dina prestandaräknare till, så att de inte tillräckligt med utrymme i hello data från hello andra loggning kanaler som du har aktiverat.</span><span class="sxs-lookup"><span data-stu-id="32499-190">Additionally, consider creating a separate table toosend your Performance Counters to, so they don't crowd out hello data coming from hello other logging channels you have enabled.</span></span>


## <a name="send-logs-tooapplication-insights"></a><span data-ttu-id="32499-191">Skicka loggar tooApplication insikter</span><span class="sxs-lookup"><span data-stu-id="32499-191">Send logs tooApplication Insights</span></span>

<span data-ttu-id="32499-192">Skicka övervakning och diagnostik data tooApplication insikter (AI) kan göras som en del av hello BOMULLSTUSS konfiguration.</span><span class="sxs-lookup"><span data-stu-id="32499-192">Sending monitoring and diagnostics data tooApplication Insights (AI) can be done as part of hello WAD configuration.</span></span> <span data-ttu-id="32499-193">Om du väljer toouse AI för händelseanalys och visualisering, läsa [Händelseanalys och visualisering med Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) tooset upp en AI Sink som en del av din ”WadCfg”.</span><span class="sxs-lookup"><span data-stu-id="32499-193">If you decide toouse AI for event analysis and visualization, read [Event Analysis and Visualization with Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) tooset up an AI Sink as part of your "WadCfg".</span></span>

## <a name="next-steps"></a><span data-ttu-id="32499-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32499-194">Next steps</span></span>

<span data-ttu-id="32499-195">När du har korrekt konfigurerat Azure diagnostics, visas data i Storage-tabeller från EventSource loggar och hello ETW.</span><span class="sxs-lookup"><span data-stu-id="32499-195">Once you have correctly configured Azure diagnostics, you will see data in your Storage tables from hello ETW and EventSource logs.</span></span> <span data-ttu-id="32499-196">Om du väljer toouse OMS gör Kibana eller några andra analyser och visualisering dataplattform som inte har konfigurerats i hello Resource Manager-mallen direkt till tooset in hello plattform på ditt val tooread i hello data från dessa lagringstabeller.</span><span class="sxs-lookup"><span data-stu-id="32499-196">If you choose toouse OMS, Kibana, or any other data analytics and visualization platform that is not directly configured in hello Resource Manager template, make sure tooset up hello platform of your choice tooread in hello data from these storage tables.</span></span> <span data-ttu-id="32499-197">Om du gör detta för OMS är relativt enkelt och förklaras i [händelse och logg analys genom OMS](service-fabric-diagnostics-event-analysis-oms.md).</span><span class="sxs-lookup"><span data-stu-id="32499-197">Doing this for OMS is relatively trivial, and is explained in [Event and log analysis through OMS](service-fabric-diagnostics-event-analysis-oms.md).</span></span> <span data-ttu-id="32499-198">Application Insights är lite specialfall på detta sätt, eftersom den kan konfigureras som en del av hello diagnostik tilläggets konfiguration, så finns toohello [lämplig artikel](service-fabric-diagnostics-event-analysis-appinsights.md) om du väljer toouse AI.</span><span class="sxs-lookup"><span data-stu-id="32499-198">Application Insights is a bit of a special case in this sense, since it can be configured as part of hello Diagnostics extension configuration, so refer toohello [appropriate article](service-fabric-diagnostics-event-analysis-appinsights.md) if you choose toouse AI.</span></span>

>[!NOTE]
><span data-ttu-id="32499-199">Det finns för närvarande inga sätt toofilter eller rensa hello händelser som skickas toohello tabell.</span><span class="sxs-lookup"><span data-stu-id="32499-199">There is currently no way toofilter or groom hello events that are sent toohello table.</span></span> <span data-ttu-id="32499-200">Om du inte implementerar en process tooremove händelser från hello tabellen fortsätter hello tabell toogrow.</span><span class="sxs-lookup"><span data-stu-id="32499-200">If you don't implement a process tooremove events from hello table, hello table will continue toogrow.</span></span> <span data-ttu-id="32499-201">För närvarande finns ett exempel på en rensning datatjänst som körs i hello [Watchdog exempel](https://github.com/Azure-Samples/service-fabric-watchdog-service), och det rekommenderas att du skriver en själv, om det finns en bra anledning du toostore loggar utöver en tidsram för 30 eller 90 dagar.</span><span class="sxs-lookup"><span data-stu-id="32499-201">Currently, there is an example of a data grooming service running in hello [Watchdog sample](https://github.com/Azure-Samples/service-fabric-watchdog-service), and it is recommended that you write one for yourself as well, unless there is a good reason for you toostore logs beyond a 30 or 90 day timeframe.</span></span>

* [<span data-ttu-id="32499-202">Lär dig hur toocollect prestandaräknare eller loggar med hjälp av hello diagnostik tillägg</span><span class="sxs-lookup"><span data-stu-id="32499-202">Learn how toocollect performance counters or logs by using hello Diagnostics extension</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="32499-203">Händelseanalys och visualisering med Application Insights</span><span class="sxs-lookup"><span data-stu-id="32499-203">Event Analysis and Visualization with Application Insights</span></span>](service-fabric-diagnostics-event-analysis-appinsights.md)
* [<span data-ttu-id="32499-204">Händelseanalys och visualisering med OMS</span><span class="sxs-lookup"><span data-stu-id="32499-204">Event Analysis and Visualization with OMS</span></span>](service-fabric-diagnostics-event-analysis-oms.md)