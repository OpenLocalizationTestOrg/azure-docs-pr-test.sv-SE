---
title: "aaaCollect loggar med hjälp av Azure-diagnostik | Microsoft Docs"
description: "Den här artikeln beskriver hur tooset in Azure-diagnostik toocollect loggar från ett Service Fabric-kluster som körs i Azure."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 9f7e1fa5-6543-4efd-b53f-39510f18df56
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: afbcfbe972b1847ef33bf0539b4398794b1bd56b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a><span data-ttu-id="de1b4-103">Samla in loggar med Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="de1b4-103">Collect logs by using Azure Diagnostics</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="de1b4-104">Windows</span><span class="sxs-lookup"><span data-stu-id="de1b4-104">Windows</span></span>](service-fabric-diagnostics-how-to-setup-wad.md)
> * [<span data-ttu-id="de1b4-105">Linux</span><span class="sxs-lookup"><span data-stu-id="de1b4-105">Linux</span></span>](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

<span data-ttu-id="de1b4-106">När du kör ett Azure Service Fabric-kluster, är det en bra idé toocollect hello loggar från alla hello noder på en central plats.</span><span class="sxs-lookup"><span data-stu-id="de1b4-106">When you're running an Azure Service Fabric cluster, it's a good idea toocollect hello logs from all hello nodes in a central location.</span></span> <span data-ttu-id="de1b4-107">Med hello loggar på en central plats hjälper dig att analysera och felsöka problem i klustret eller problem i hello program och tjänster som körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="de1b4-107">Having hello logs in a central location helps you analyze and troubleshoot issues in your cluster, or issues in hello applications and services running in that cluster.</span></span>

<span data-ttu-id="de1b4-108">Ett sätt tooupload och samla in loggar är toouse hello Azure-diagnostik-tillägg, vilka överföringar loggar tooAzure lagring, Azure Application Insights eller Händelsehubbar i Azure.</span><span class="sxs-lookup"><span data-stu-id="de1b4-108">One way tooupload and collect logs is toouse hello Azure Diagnostics extension, which uploads logs tooAzure Storage, Azure Application Insights, or Azure Event Hubs.</span></span> <span data-ttu-id="de1b4-109">hello loggar är inte så användbart direkt i lagring eller i Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="de1b4-109">hello logs are not that useful directly in storage or in Event Hubs.</span></span> <span data-ttu-id="de1b4-110">Men du kan använda en extern process tooread hello händelser från lagring och placera dem i en produkt som [logganalys](../log-analytics/log-analytics-service-fabric.md) eller en annan lösning för parsning av loggen.</span><span class="sxs-lookup"><span data-stu-id="de1b4-110">But you can use an external process tooread hello events from storage and place them in a product such as [Log Analytics](../log-analytics/log-analytics-service-fabric.md) or another log-parsing solution.</span></span> <span data-ttu-id="de1b4-111">[Azure Application Insights](https://azure.microsoft.com/services/application-insights/) levereras med en omfattande sökning och analyser Loggtjänsten inbyggda.</span><span class="sxs-lookup"><span data-stu-id="de1b4-111">[Azure Application Insights](https://azure.microsoft.com/services/application-insights/) comes with a comprehensive log search and analytics service built-in.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de1b4-112">Krav</span><span class="sxs-lookup"><span data-stu-id="de1b4-112">Prerequisites</span></span>
<span data-ttu-id="de1b4-113">Du kan använda dessa verktyg tooperform vissa hello åtgärder i det här dokumentet:</span><span class="sxs-lookup"><span data-stu-id="de1b4-113">You use these tools tooperform some of hello operations in this document:</span></span>

* <span data-ttu-id="de1b4-114">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (relaterade tooAzure molntjänster men har bra information och exempel)</span><span class="sxs-lookup"><span data-stu-id="de1b4-114">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (related tooAzure Cloud Services but has good information and examples)</span></span>
* [<span data-ttu-id="de1b4-115">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="de1b4-115">Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="de1b4-116">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="de1b4-116">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="de1b4-117">Azure Resource Manager-klienten</span><span class="sxs-lookup"><span data-stu-id="de1b4-117">Azure Resource Manager client</span></span>](https://github.com/projectkudu/ARMClient)
* [<span data-ttu-id="de1b4-118">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="de1b4-118">Azure Resource Manager template</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-sources-that-you-might-want-toocollect"></a><span data-ttu-id="de1b4-119">Logga källor som du kanske vill toocollect</span><span class="sxs-lookup"><span data-stu-id="de1b4-119">Log sources that you might want toocollect</span></span>
* <span data-ttu-id="de1b4-120">**Service Fabric loggar**: orsakat från hello plattform toostandard ETW Event Tracing for Windows () och EventSource kanaler.</span><span class="sxs-lookup"><span data-stu-id="de1b4-120">**Service Fabric logs**: Emitted from hello platform toostandard Event Tracing for Windows (ETW) and EventSource channels.</span></span> <span data-ttu-id="de1b4-121">Loggar kan vara något av flera olika typer:</span><span class="sxs-lookup"><span data-stu-id="de1b4-121">Logs can be one of several types:</span></span>
  * <span data-ttu-id="de1b4-122">Operativa händelser: loggar för åtgärder som hello Service Fabric-plattformen utför.</span><span class="sxs-lookup"><span data-stu-id="de1b4-122">Operational events: Logs for operations that hello Service Fabric platform performs.</span></span> <span data-ttu-id="de1b4-123">Exempel: skapa program och tjänster, nod tillståndsändringar och information om uppgradering.</span><span class="sxs-lookup"><span data-stu-id="de1b4-123">Examples include creation of applications and services, node state changes, and upgrade information.</span></span>
  * [<span data-ttu-id="de1b4-124">Reliable Actors programmering modellen händelser</span><span class="sxs-lookup"><span data-stu-id="de1b4-124">Reliable Actors programming model events</span></span>](service-fabric-reliable-actors-diagnostics.md)
  * [<span data-ttu-id="de1b4-125">Reliable Services programming modellen händelser</span><span class="sxs-lookup"><span data-stu-id="de1b4-125">Reliable Services programming model events</span></span>](service-fabric-reliable-services-diagnostics.md)
* <span data-ttu-id="de1b4-126">**Programhändelser**: händelser orsakat från din tjänst kod och skrivits hello EventSource hjälpklass i hello Visual Studio mallar.</span><span class="sxs-lookup"><span data-stu-id="de1b4-126">**Application events**: Events emitted from your service's code and written out by using hello EventSource helper class provided in hello Visual Studio templates.</span></span> <span data-ttu-id="de1b4-127">Mer information om hur toowrite loggar från ditt program finns [övervaka och diagnostisera tjänster i en inställning för lokal dator development](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="de1b4-127">For more information on how toowrite logs from your application, see [Monitor and diagnose services in a local machine development setup](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>

## <a name="deploy-hello-diagnostics-extension"></a><span data-ttu-id="de1b4-128">Distribuera hello diagnostik tillägg</span><span class="sxs-lookup"><span data-stu-id="de1b4-128">Deploy hello Diagnostics extension</span></span>
<span data-ttu-id="de1b4-129">hello första steget i att samla in loggar är toodeploy hello diagnostik tillägg på varje hello virtuella datorer i hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="de1b4-129">hello first step in collecting logs is toodeploy hello Diagnostics extension on each of hello VMs in hello Service Fabric cluster.</span></span> <span data-ttu-id="de1b4-130">hello diagnostik tillägget samlar in loggar på varje virtuell dator och överför dem toohello storage-konto som du anger.</span><span class="sxs-lookup"><span data-stu-id="de1b4-130">hello Diagnostics extension collects logs on each VM and uploads them toohello storage account that you specify.</span></span> <span data-ttu-id="de1b4-131">hello steg variera något beroende på om du använder hello Azure-portalen eller Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="de1b4-131">hello steps vary a little based on whether you use hello Azure portal or Azure Resource Manager.</span></span> <span data-ttu-id="de1b4-132">hello stegen variera beroende på om hello distribution är en del av klustret har skapats eller är i ett kluster som redan finns.</span><span class="sxs-lookup"><span data-stu-id="de1b4-132">hello steps also vary based on whether hello deployment is part of cluster creation or is for a cluster that already exists.</span></span> <span data-ttu-id="de1b4-133">Nu ska vi titta på hello steg för varje scenario.</span><span class="sxs-lookup"><span data-stu-id="de1b4-133">Let's look at hello steps for each scenario.</span></span>

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-through-hello-portal"></a><span data-ttu-id="de1b4-134">Distribuera hello diagnostik tillägget som en del av klustret har skapats via hello portal</span><span class="sxs-lookup"><span data-stu-id="de1b4-134">Deploy hello Diagnostics extension as part of cluster creation through hello portal</span></span>
<span data-ttu-id="de1b4-135">toodeploy hello diagnostik tillägget toohello virtuella datorer i hello klustret som en del av klustret skapas, Använd hello diagnostik inställningar panelen visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="de1b4-135">toodeploy hello Diagnostics extension toohello VMs in hello cluster as part of cluster creation, you use hello Diagnostics settings panel shown in hello following image.</span></span> <span data-ttu-id="de1b4-136">tooenable Reliable Actors eller Reliable Services händelseinsamling, kontrollera att diagnostik är inställd för**på** (hello standardinställningen).</span><span class="sxs-lookup"><span data-stu-id="de1b4-136">tooenable Reliable Actors or Reliable Services event collection, ensure that Diagnostics is set too**On** (hello default setting).</span></span> <span data-ttu-id="de1b4-137">När du har skapat hello klustret kan du inte ändra dessa inställningar med hjälp av hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="de1b4-137">After you create hello cluster, you can't change these settings by using hello portal.</span></span>

![Azure diagnostikinställningar i hello-portalen för att skapa klustret](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

<span data-ttu-id="de1b4-139">hello Azure-teamet *kräver* stöd loggar toohelp lösa eventuella supportförfrågningar som du skapar.</span><span class="sxs-lookup"><span data-stu-id="de1b4-139">hello Azure support team *requires* support logs toohelp resolve any support requests that you create.</span></span> <span data-ttu-id="de1b4-140">De här loggarna har samlats in i realtid och lagras i en hello lagringskonton som skapats i hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="de1b4-140">These logs are collected in real time and are stored in one of hello storage accounts created in hello resource group.</span></span> <span data-ttu-id="de1b4-141">hello diagnostikinställningarna konfigurera händelser på programnivå.</span><span class="sxs-lookup"><span data-stu-id="de1b4-141">hello Diagnostics settings configure application-level events.</span></span> <span data-ttu-id="de1b4-142">Här ingår [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) händelser, [Reliable Services](service-fabric-reliable-services-diagnostics.md) händelser och vissa på systemnivå Service Fabric händelser toobe lagras i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="de1b4-142">These events include [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) events, [Reliable Services](service-fabric-reliable-services-diagnostics.md) events, and some system-level Service Fabric events toobe stored in Azure Storage.</span></span>

<span data-ttu-id="de1b4-143">Produkter som [Elasticsearch](https://www.elastic.co/guide/index.html) eller en egen process kan hämta hello händelser från hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="de1b4-143">Products such as [Elasticsearch](https://www.elastic.co/guide/index.html) or your own process can get hello events from hello storage account.</span></span> <span data-ttu-id="de1b4-144">Det finns för närvarande inga sätt toofilter eller rensa hello händelser som skickas toohello tabell.</span><span class="sxs-lookup"><span data-stu-id="de1b4-144">There is currently no way toofilter or groom hello events that are sent toohello table.</span></span> <span data-ttu-id="de1b4-145">Om du inte implementerar en process tooremove händelser från hello tabellen fortsätter hello tabell toogrow.</span><span class="sxs-lookup"><span data-stu-id="de1b4-145">If you don't implement a process tooremove events from hello table, hello table will continue toogrow.</span></span>

<span data-ttu-id="de1b4-146">När du skapar ett kluster med hjälp av hello portal, vi rekommenderar starkt att du hämtar hello mall **innan du klickar på OK** toocreate hello klustret.</span><span class="sxs-lookup"><span data-stu-id="de1b4-146">When you're creating a cluster by using hello portal, we highly recommend that you download hello template **before you click OK** toocreate hello cluster.</span></span> <span data-ttu-id="de1b4-147">Mer information finns för[ställer in ett Service Fabric-kluster med en Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="de1b4-147">For details, refer too[Set up a Service Fabric cluster by using an Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="de1b4-148">Du behöver hello malländringar toomake senare, eftersom du inte kan göra några ändringar med hjälp av hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="de1b4-148">You'll need hello template toomake changes later, because you can't make some changes by using hello portal.</span></span>

<span data-ttu-id="de1b4-149">Du kan exportera mallar från hello-portalen med hjälp av hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="de1b4-149">You can export templates from hello portal by using hello following steps.</span></span> <span data-ttu-id="de1b4-150">Dessa mallar kan dock vara svårare toouse eftersom de kan ha null-värden som saknar nödvändig information.</span><span class="sxs-lookup"><span data-stu-id="de1b4-150">However, these templates can be more difficult toouse because they might have null values that are missing required information.</span></span>

1. <span data-ttu-id="de1b4-151">Öppna din resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="de1b4-151">Open your resource group.</span></span>
2. <span data-ttu-id="de1b4-152">Välj **inställningar** toodisplay hello Inställningar Kontrollpanelen.</span><span class="sxs-lookup"><span data-stu-id="de1b4-152">Select **Settings** toodisplay hello settings panel.</span></span>
3. <span data-ttu-id="de1b4-153">Välj **distributioner** toodisplay hello distribution historikpanelen.</span><span class="sxs-lookup"><span data-stu-id="de1b4-153">Select **Deployments** toodisplay hello deployment history panel.</span></span>
4. <span data-ttu-id="de1b4-154">Välj en toodisplay hello distributionsinformation för hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="de1b4-154">Select a deployment toodisplay hello details of hello deployment.</span></span>
5. <span data-ttu-id="de1b4-155">Välj **Exportmallen** toodisplay hello mallen panelen.</span><span class="sxs-lookup"><span data-stu-id="de1b4-155">Select **Export Template** toodisplay hello template panel.</span></span>
6. <span data-ttu-id="de1b4-156">Välj **spara toofile** tooexport en .zip-fil som innehåller hello mallen, parameter och PowerShell-filer.</span><span class="sxs-lookup"><span data-stu-id="de1b4-156">Select **Save toofile** tooexport a .zip file that contains hello template, parameter, and PowerShell files.</span></span>

<span data-ttu-id="de1b4-157">När du exporterar hello filer måste toomake en ändring.</span><span class="sxs-lookup"><span data-stu-id="de1b4-157">After you export hello files, you need toomake a modification.</span></span> <span data-ttu-id="de1b4-158">Redigera hello parameters.json filen och ta bort hello **adminPassword** element.</span><span class="sxs-lookup"><span data-stu-id="de1b4-158">Edit hello parameters.json file and remove hello **adminPassword** element.</span></span> <span data-ttu-id="de1b4-159">Detta innebär att en fråga efter hello lösenord när hello-skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="de1b4-159">This will cause a prompt for hello password when hello deployment script is run.</span></span> <span data-ttu-id="de1b4-160">När du kör hello distributionsskriptet, kanske toofix null-värden.</span><span class="sxs-lookup"><span data-stu-id="de1b4-160">When you're running hello deployment script, you might have toofix null parameter values.</span></span>

<span data-ttu-id="de1b4-161">toouse hello ned mallen tooupdate en konfiguration:</span><span class="sxs-lookup"><span data-stu-id="de1b4-161">toouse hello downloaded template tooupdate a configuration:</span></span>

1. <span data-ttu-id="de1b4-162">Extrahera hello innehållet tooa mapp på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="de1b4-162">Extract hello contents tooa folder on your local computer.</span></span>
2. <span data-ttu-id="de1b4-163">Ändra hello innehållet tooreflect hello ny konfiguration.</span><span class="sxs-lookup"><span data-stu-id="de1b4-163">Modify hello contents tooreflect hello new configuration.</span></span>
3. <span data-ttu-id="de1b4-164">Starta PowerShell och ändra toohello mapp där du extraherade hello innehåll.</span><span class="sxs-lookup"><span data-stu-id="de1b4-164">Start PowerShell and change toohello folder where you extracted hello content.</span></span>
4. <span data-ttu-id="de1b4-165">Kör **deploy.ps1** och Fyll i hello prenumerations-ID, hello resursgruppens namn (Använd hello samma tooupdate hello konfiguration), och en unik distributionsnamnet.</span><span class="sxs-lookup"><span data-stu-id="de1b4-165">Run **deploy.ps1** and fill in hello subscription ID, hello resource group name (use hello same name tooupdate hello configuration), and a unique deployment name.</span></span>

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a><span data-ttu-id="de1b4-166">Distribuera hello diagnostik tillägget som en del av klustret har skapats med hjälp av Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="de1b4-166">Deploy hello Diagnostics extension as part of cluster creation by using Azure Resource Manager</span></span>
<span data-ttu-id="de1b4-167">toocreate ett kluster med hjälp av hanteraren för filserverresurser måste tooadd hello diagnostik configuration JSON toohello fullständig klustret Resource Manager-mallen innan du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="de1b4-167">toocreate a cluster by using Resource Manager, you need tooadd hello Diagnostics configuration JSON toohello full cluster Resource Manager template before you create hello cluster.</span></span> <span data-ttu-id="de1b4-168">Vi tillhandahåller en fem-VM klustret Resource Manager exempelmall med diagnostik konfiguration läggs tooit som en del av våra exempel för Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="de1b4-168">We provide a sample five-VM cluster Resource Manager template with Diagnostics configuration added tooit as part of our Resource Manager template samples.</span></span> <span data-ttu-id="de1b4-169">Du kan se den på den här platsen i galleriet för hello Azure-exempel: [kluster med fem noder med diagnostik Resource Manager mallen exempel](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).</span><span class="sxs-lookup"><span data-stu-id="de1b4-169">You can see it at this location in hello Azure Samples gallery: [Five-node cluster with Diagnostics Resource Manager template sample](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).</span></span>

<span data-ttu-id="de1b4-170">toosee hello diagnostik inställningen i hello Resource Manager-mall, öppna hello azuredeploy.json filen och Sök efter **IaaSDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="de1b4-170">toosee hello Diagnostics setting in hello Resource Manager template, open hello azuredeploy.json file and search for **IaaSDiagnostics**.</span></span> <span data-ttu-id="de1b4-171">toocreate ett kluster med hjälp av den här mallen, Välj hello **distribuera tooAzure** knappen på hello föregående länk.</span><span class="sxs-lookup"><span data-stu-id="de1b4-171">toocreate a cluster by using this template, select hello **Deploy tooAzure** button available at hello previous link.</span></span>

<span data-ttu-id="de1b4-172">Du kan också du kan hämta hello Resource Manager exempel, göra ändringar tooit och skapa ett kluster med hello ändrade mallen med hjälp av hello `New-AzureRmResourceGroupDeployment` i Azure PowerShell-fönster.</span><span class="sxs-lookup"><span data-stu-id="de1b4-172">Alternatively, you can download hello Resource Manager sample, make changes tooit, and create a cluster with hello modified template by using hello `New-AzureRmResourceGroupDeployment` command in an Azure PowerShell window.</span></span> <span data-ttu-id="de1b4-173">Se följande kod för hello-parametrar som du skickar i toohello kommandot hello.</span><span class="sxs-lookup"><span data-stu-id="de1b4-173">See hello following code for hello parameters that you pass in toohello command.</span></span> <span data-ttu-id="de1b4-174">Detaljerad information om hur toodeploy en resurs gruppen med hjälp av PowerShell finns i artikeln hello [distribuera en resursgrupp med hello Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="de1b4-174">For detailed information on how toodeploy a resource group by using PowerShell, see hello article [Deploy a resource group with hello Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-hello-diagnostics-extension-tooan-existing-cluster"></a><span data-ttu-id="de1b4-175">Distribuera hello diagnostik tillägget tooan befintligt kluster</span><span class="sxs-lookup"><span data-stu-id="de1b4-175">Deploy hello Diagnostics extension tooan existing cluster</span></span>
<span data-ttu-id="de1b4-176">Om du har ett befintligt kluster som inte har distribuerats diagnostik, eller om du vill toomodify en befintlig konfiguration, kan du lägga till eller uppdatera det.</span><span class="sxs-lookup"><span data-stu-id="de1b4-176">If you have an existing cluster that doesn't have Diagnostics deployed, or if you want toomodify an existing configuration, you can add or update it.</span></span> <span data-ttu-id="de1b4-177">Ändra hello Resource Manager-mall som används toocreate hello befintligt kluster eller hämta hello mallen från hello portal som tidigare beskrivits.</span><span class="sxs-lookup"><span data-stu-id="de1b4-177">Modify hello Resource Manager template that's used toocreate hello existing cluster or download hello template from hello portal as described earlier.</span></span> <span data-ttu-id="de1b4-178">Ändra hello template.json filen genom att utföra följande uppgifter hello.</span><span class="sxs-lookup"><span data-stu-id="de1b4-178">Modify hello template.json file by performing hello following tasks.</span></span>

<span data-ttu-id="de1b4-179">Lägga till en ny mall för storage resource toohello genom att lägga till toohello resurser avsnitt.</span><span class="sxs-lookup"><span data-stu-id="de1b4-179">Add a new storage resource toohello template by adding toohello resources section.</span></span>

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

 <span data-ttu-id="de1b4-180">Lägg sedan till avsnittet toohello parametrar precis efter hello storage-konto definitioner mellan `supportLogStorageAccountName` och `vmNodeType0Name`.</span><span class="sxs-lookup"><span data-stu-id="de1b4-180">Next, add toohello parameters section just after hello storage account definitions, between `supportLogStorageAccountName` and `vmNodeType0Name`.</span></span> <span data-ttu-id="de1b4-181">Ersätt hello platshållartexten *lagringskontonamnet här* med hello namnet hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="de1b4-181">Replace hello placeholder text *storage account name goes here* with hello name of hello storage account.</span></span>

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
<span data-ttu-id="de1b4-182">Sedan uppdatera hello `VirtualMachineProfile` i hello template.json filen genom att lägga till följande kod i hello tillägg matris hello.</span><span class="sxs-lookup"><span data-stu-id="de1b4-182">Then, update hello `VirtualMachineProfile` section of hello template.json file by adding hello following code within hello extensions array.</span></span> <span data-ttu-id="de1b4-183">Vara säker på att tooadd kommatecken i hello början eller slutet av hello, beroende på om det har infogats.</span><span class="sxs-lookup"><span data-stu-id="de1b4-183">Be sure tooadd a comma at hello beginning or hello end, depending on where it's inserted.</span></span>

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

<span data-ttu-id="de1b4-184">När du ändrar hello template.json enligt kan publicera hello Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="de1b4-184">After you modify hello template.json file as described, republish hello Resource Manager template.</span></span> <span data-ttu-id="de1b4-185">Om hello mallen exporterades publicerar kör hello deploy.ps1 filen hello mallen.</span><span class="sxs-lookup"><span data-stu-id="de1b4-185">If hello template was exported, running hello deploy.ps1 file republishes hello template.</span></span> <span data-ttu-id="de1b4-186">När du har distribuerat, se till att **ProvisioningState** är **lyckades**.</span><span class="sxs-lookup"><span data-stu-id="de1b4-186">After you deploy, ensure that **ProvisioningState** is **Succeeded**.</span></span>

## <a name="update-diagnostics-toocollect-health-and-load-events"></a><span data-ttu-id="de1b4-187">Uppdatera diagnostik toocollect hälsa och Läs in händelser</span><span class="sxs-lookup"><span data-stu-id="de1b4-187">Update diagnostics toocollect health and load events</span></span>

<span data-ttu-id="de1b4-188">Från och med hello 5.4 versionen av Service Fabric, är hälsotillstånd och Läs in mått händelser tillgängliga för samlingen.</span><span class="sxs-lookup"><span data-stu-id="de1b4-188">Starting with hello 5.4 release of Service Fabric, health and load metric events are available for collection.</span></span> <span data-ttu-id="de1b4-189">Dessa händelser återspeglar händelser som genererats av systemet hello eller din kod med hjälp av hello hälsa eller läsa in reporting API: er som [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) eller [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span><span class="sxs-lookup"><span data-stu-id="de1b4-189">These events reflect events generated by hello system or your code by using hello health or load reporting APIs such as [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) or [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span></span> <span data-ttu-id="de1b4-190">Detta gör för att sammanställa och visa systemhälsa över tid och aviseringar baserat på händelser hälsa eller belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="de1b4-190">This allows for aggregating and viewing system health over time and for alerting based on health or load events.</span></span> <span data-ttu-id="de1b4-191">tooview lägga till dessa händelser i Loggboken i Visual Studio-diagnostik ”Microsoft-ServiceFabric:4:0x4000000000000008” toohello lista över ETW-providers.</span><span class="sxs-lookup"><span data-stu-id="de1b4-191">tooview these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000008" toohello list of ETW providers.</span></span>

<span data-ttu-id="de1b4-192">toocollect hello händelser, ändra hello resource manager-mall tooinclude</span><span class="sxs-lookup"><span data-stu-id="de1b4-192">toocollect hello events, modify hello resource manager template tooinclude</span></span>

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

## <a name="update-diagnostics-toocollect-and-upload-logs-from-new-eventsource-channels"></a><span data-ttu-id="de1b4-193">Uppdatera diagnostik toocollect och ladda upp loggar från den nya EventSource kanaler</span><span class="sxs-lookup"><span data-stu-id="de1b4-193">Update Diagnostics toocollect and upload logs from new EventSource channels</span></span>
<span data-ttu-id="de1b4-194">tooupdate diagnostik toocollect loggar från nya EventSource kanaler som representerar ett nytt program som du är om toodeploy, utför samma steg som hello hello [föregående avsnitt](#deploywadarm) för inställning av diagnostik för en befintlig kluster.</span><span class="sxs-lookup"><span data-stu-id="de1b4-194">tooupdate Diagnostics toocollect logs from new EventSource channels that represent a new application that you're about toodeploy, perform hello same steps as in hello [previous section](#deploywadarm) for setup of Diagnostics for an existing cluster.</span></span>

<span data-ttu-id="de1b4-195">Uppdatera hello `EtwEventSourceProviderConfiguration` under hello template.json tooadd poster för hello nya EventSource kanaler innan du använder hello konfigurationen uppdatera med hjälp av hello `New-AzureRmResourceGroupDeployment` PowerShell-kommando.</span><span class="sxs-lookup"><span data-stu-id="de1b4-195">Update hello `EtwEventSourceProviderConfiguration` section in hello template.json file tooadd entries for hello new EventSource channels before you apply hello configuration update by using hello `New-AzureRmResourceGroupDeployment` PowerShell command.</span></span> <span data-ttu-id="de1b4-196">hello händelsekälla hello namn har definierats som en del av din kod i hello Visual Studio-genererade ServiceEventSource.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="de1b4-196">hello name of hello event source is defined as part of your code in hello Visual Studio-generated ServiceEventSource.cs file.</span></span>

<span data-ttu-id="de1b4-197">T.ex, om din händelsekälla heter Mina Eventsource, lägger du till följande kod tooplace hello händelser från Mina Eventsource i en tabell med namnet MyDestinationTableName hello.</span><span class="sxs-lookup"><span data-stu-id="de1b4-197">For example, if your event source is named My-Eventsource, add hello following code tooplace hello events from My-Eventsource into a table named MyDestinationTableName.</span></span>

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

<span data-ttu-id="de1b4-198">toocollect prestandaräknare eller händelseloggar, ändra hello Resource Manager-mall med hjälp av hello-exempel finns i [skapa en virtuell Windows-dator med övervakning och diagnostik med en Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="de1b4-198">toocollect performance counters or event logs, modify hello Resource Manager template by using hello examples provided in [Create a Windows virtual machine with monitoring and diagnostics by using an Azure Resource Manager template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="de1b4-199">Kubfilen hello Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="de1b4-199">Then, republish hello Resource Manager template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="de1b4-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="de1b4-200">Next steps</span></span>
<span data-ttu-id="de1b4-201">toounderstand i detalj vilka händelser som du ska leta efter vid felsökning, se hello diagnostiska händelser som orsakat för [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) och [Reliable Services](service-fabric-reliable-services-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="de1b4-201">toounderstand in more detail what events you should look for while troubleshooting issues, see hello diagnostic events emitted for [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) and [Reliable Services](service-fabric-reliable-services-diagnostics.md).</span></span>

## <a name="related-articles"></a><span data-ttu-id="de1b4-202">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="de1b4-202">Related articles</span></span>
* [<span data-ttu-id="de1b4-203">Lär dig hur toocollect prestandaräknare eller loggar med hjälp av hello diagnostik tillägg</span><span class="sxs-lookup"><span data-stu-id="de1b4-203">Learn how toocollect performance counters or logs by using hello Diagnostics extension</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="de1b4-204">Service Fabric-lösning i logganalys</span><span class="sxs-lookup"><span data-stu-id="de1b4-204">Service Fabric solution in Log Analytics</span></span>](../log-analytics/log-analytics-service-fabric.md)

