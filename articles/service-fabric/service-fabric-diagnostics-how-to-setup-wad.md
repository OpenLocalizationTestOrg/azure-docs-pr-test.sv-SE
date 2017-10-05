---
title: "Samla in loggar med hjälp av Azure-diagnostik | Microsoft Docs"
description: "Den här artikeln beskriver hur du ställer in Azure-diagnostik för att samla in loggar från ett Service Fabric-kluster som körs i Azure."
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
ms.openlocfilehash: 190a8a393f2e7d74a792db4efa81f94a18b02221
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a><span data-ttu-id="b260b-103">Samla in loggar med Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="b260b-103">Collect logs by using Azure Diagnostics</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b260b-104">Windows</span><span class="sxs-lookup"><span data-stu-id="b260b-104">Windows</span></span>](service-fabric-diagnostics-how-to-setup-wad.md)
> * [<span data-ttu-id="b260b-105">Linux</span><span class="sxs-lookup"><span data-stu-id="b260b-105">Linux</span></span>](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

<span data-ttu-id="b260b-106">När du kör ett Azure Service Fabric-kluster, är det en bra idé att samla in loggar från alla noder i en central plats.</span><span class="sxs-lookup"><span data-stu-id="b260b-106">When you're running an Azure Service Fabric cluster, it's a good idea to collect the logs from all the nodes in a central location.</span></span> <span data-ttu-id="b260b-107">Med loggarna på en central plats hjälper dig att analysera och felsöka problem i klustret eller problem i program och tjänster som körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="b260b-107">Having the logs in a central location helps you analyze and troubleshoot issues in your cluster, or issues in the applications and services running in that cluster.</span></span>

<span data-ttu-id="b260b-108">Ett sätt att överföra och samla in loggar är att använda Azure-diagnostik-tillägget, som överför loggar till Azure Storage eller Azure Application Insights Händelsehubbar i Azure.</span><span class="sxs-lookup"><span data-stu-id="b260b-108">One way to upload and collect logs is to use the Azure Diagnostics extension, which uploads logs to Azure Storage, Azure Application Insights, or Azure Event Hubs.</span></span> <span data-ttu-id="b260b-109">Loggarna är inte så användbart direkt i lagring eller i Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="b260b-109">The logs are not that useful directly in storage or in Event Hubs.</span></span> <span data-ttu-id="b260b-110">Men du kan använda en extern process för att läsa händelser från lagring och placera dem i en produkt som [logganalys](../log-analytics/log-analytics-service-fabric.md) eller en annan lösning för parsning av loggen.</span><span class="sxs-lookup"><span data-stu-id="b260b-110">But you can use an external process to read the events from storage and place them in a product such as [Log Analytics](../log-analytics/log-analytics-service-fabric.md) or another log-parsing solution.</span></span> <span data-ttu-id="b260b-111">[Azure Application Insights](https://azure.microsoft.com/services/application-insights/) levereras med en omfattande sökning och analyser Loggtjänsten inbyggda.</span><span class="sxs-lookup"><span data-stu-id="b260b-111">[Azure Application Insights](https://azure.microsoft.com/services/application-insights/) comes with a comprehensive log search and analytics service built-in.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b260b-112">Krav</span><span class="sxs-lookup"><span data-stu-id="b260b-112">Prerequisites</span></span>
<span data-ttu-id="b260b-113">Du kan använda dessa verktyg för att utföra vissa åtgärder i det här dokumentet:</span><span class="sxs-lookup"><span data-stu-id="b260b-113">You use these tools to perform some of the operations in this document:</span></span>

* <span data-ttu-id="b260b-114">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (rör Azure Cloud Services men har bra information och exempel)</span><span class="sxs-lookup"><span data-stu-id="b260b-114">[Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (related to Azure Cloud Services but has good information and examples)</span></span>
* [<span data-ttu-id="b260b-115">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b260b-115">Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="b260b-116">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b260b-116">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="b260b-117">Azure Resource Manager-klienten</span><span class="sxs-lookup"><span data-stu-id="b260b-117">Azure Resource Manager client</span></span>](https://github.com/projectkudu/ARMClient)
* [<span data-ttu-id="b260b-118">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="b260b-118">Azure Resource Manager template</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-sources-that-you-might-want-to-collect"></a><span data-ttu-id="b260b-119">Loggen källor som du kanske vill samla in</span><span class="sxs-lookup"><span data-stu-id="b260b-119">Log sources that you might want to collect</span></span>
* <span data-ttu-id="b260b-120">**Service Fabric loggar**: orsakat från plattform till standard ETW Event Tracing for Windows () och EventSource kanaler.</span><span class="sxs-lookup"><span data-stu-id="b260b-120">**Service Fabric logs**: Emitted from the platform to standard Event Tracing for Windows (ETW) and EventSource channels.</span></span> <span data-ttu-id="b260b-121">Loggar kan vara något av flera olika typer:</span><span class="sxs-lookup"><span data-stu-id="b260b-121">Logs can be one of several types:</span></span>
  * <span data-ttu-id="b260b-122">Operativa händelser: loggar för åtgärder som utförs av Service Fabric-plattformen.</span><span class="sxs-lookup"><span data-stu-id="b260b-122">Operational events: Logs for operations that the Service Fabric platform performs.</span></span> <span data-ttu-id="b260b-123">Exempel: skapa program och tjänster, nod tillståndsändringar och information om uppgradering.</span><span class="sxs-lookup"><span data-stu-id="b260b-123">Examples include creation of applications and services, node state changes, and upgrade information.</span></span>
  * [<span data-ttu-id="b260b-124">Reliable Actors programmering modellen händelser</span><span class="sxs-lookup"><span data-stu-id="b260b-124">Reliable Actors programming model events</span></span>](service-fabric-reliable-actors-diagnostics.md)
  * [<span data-ttu-id="b260b-125">Reliable Services programming modellen händelser</span><span class="sxs-lookup"><span data-stu-id="b260b-125">Reliable Services programming model events</span></span>](service-fabric-reliable-services-diagnostics.md)
* <span data-ttu-id="b260b-126">**Programhändelser**: händelser orsakat från din tjänst kod och skrivs med hjälp av EventSource hjälpklass i Visual Studio-mallar.</span><span class="sxs-lookup"><span data-stu-id="b260b-126">**Application events**: Events emitted from your service's code and written out by using the EventSource helper class provided in the Visual Studio templates.</span></span> <span data-ttu-id="b260b-127">Mer information om hur du skriver loggar från ditt program finns [övervaka och diagnostisera tjänster i en inställning för lokal dator development](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="b260b-127">For more information on how to write logs from your application, see [Monitor and diagnose services in a local machine development setup](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>

## <a name="deploy-the-diagnostics-extension"></a><span data-ttu-id="b260b-128">Distribuera diagnostik-tillägget</span><span class="sxs-lookup"><span data-stu-id="b260b-128">Deploy the Diagnostics extension</span></span>
<span data-ttu-id="b260b-129">Det första steget i att samla in loggar är att distribuera diagnostik tillägg på var och en av de virtuella datorerna i Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="b260b-129">The first step in collecting logs is to deploy the Diagnostics extension on each of the VMs in the Service Fabric cluster.</span></span> <span data-ttu-id="b260b-130">Diagnostik-tillägget samlar in loggar på varje virtuell dator och överför dem till lagringskontot som du anger.</span><span class="sxs-lookup"><span data-stu-id="b260b-130">The Diagnostics extension collects logs on each VM and uploads them to the storage account that you specify.</span></span> <span data-ttu-id="b260b-131">Stegen variera något beroende på om du använder Azure-portalen eller Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b260b-131">The steps vary a little based on whether you use the Azure portal or Azure Resource Manager.</span></span> <span data-ttu-id="b260b-132">Stegen variera beroende på om distributionen är en del av klustret har skapats eller är i ett kluster som redan finns.</span><span class="sxs-lookup"><span data-stu-id="b260b-132">The steps also vary based on whether the deployment is part of cluster creation or is for a cluster that already exists.</span></span> <span data-ttu-id="b260b-133">Nu ska vi titta på stegen för varje scenario.</span><span class="sxs-lookup"><span data-stu-id="b260b-133">Let's look at the steps for each scenario.</span></span>

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a><span data-ttu-id="b260b-134">Distribuera diagnostik-tillägget som en del av klustret skapas via portalen</span><span class="sxs-lookup"><span data-stu-id="b260b-134">Deploy the Diagnostics extension as part of cluster creation through the portal</span></span>
<span data-ttu-id="b260b-135">Om du vill distribuera diagnostik-tillägg till de virtuella datorerna i klustret som en del av klustret har skapats, kan du använda panelen diagnostik inställningar visas i följande bild.</span><span class="sxs-lookup"><span data-stu-id="b260b-135">To deploy the Diagnostics extension to the VMs in the cluster as part of cluster creation, you use the Diagnostics settings panel shown in the following image.</span></span> <span data-ttu-id="b260b-136">Om du vill aktivera insamling av Reliable Actors eller Reliable Services, kontrollera att diagnostik är **på** (standardinställningen).</span><span class="sxs-lookup"><span data-stu-id="b260b-136">To enable Reliable Actors or Reliable Services event collection, ensure that Diagnostics is set to **On** (the default setting).</span></span> <span data-ttu-id="b260b-137">När du skapar klustret kan du inte ändra dessa inställningar med hjälp av portalen.</span><span class="sxs-lookup"><span data-stu-id="b260b-137">After you create the cluster, you can't change these settings by using the portal.</span></span>

![Azure diagnostikinställningar för klustret skapas i portalen](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

<span data-ttu-id="b260b-139">Azure supportgruppen *kräver* stöder loggar för att lösa eventuella supportförfrågningar som du skapar.</span><span class="sxs-lookup"><span data-stu-id="b260b-139">The Azure support team *requires* support logs to help resolve any support requests that you create.</span></span> <span data-ttu-id="b260b-140">De här loggarna har samlats in i realtid och lagras i något av de lagringskonton som skapats i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="b260b-140">These logs are collected in real time and are stored in one of the storage accounts created in the resource group.</span></span> <span data-ttu-id="b260b-141">Inställningarna för Webbprogramdiagnostik konfigurera händelser på programnivå.</span><span class="sxs-lookup"><span data-stu-id="b260b-141">The Diagnostics settings configure application-level events.</span></span> <span data-ttu-id="b260b-142">Här ingår [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) händelser, [Reliable Services](service-fabric-reliable-services-diagnostics.md) händelser och vissa på systemnivå Service Fabric-händelser som ska lagras i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b260b-142">These events include [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) events, [Reliable Services](service-fabric-reliable-services-diagnostics.md) events, and some system-level Service Fabric events to be stored in Azure Storage.</span></span>

<span data-ttu-id="b260b-143">Produkter som [Elasticsearch](https://www.elastic.co/guide/index.html) eller en egen process kan hämta händelser från lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="b260b-143">Products such as [Elasticsearch](https://www.elastic.co/guide/index.html) or your own process can get the events from the storage account.</span></span> <span data-ttu-id="b260b-144">Det finns för närvarande inget sätt att filtrera eller rensa de händelser som skickas till tabellen.</span><span class="sxs-lookup"><span data-stu-id="b260b-144">There is currently no way to filter or groom the events that are sent to the table.</span></span> <span data-ttu-id="b260b-145">Om du inte implementerar en process för att ta bort händelser från tabellen, i tabellen kommer att fortsätta att växa.</span><span class="sxs-lookup"><span data-stu-id="b260b-145">If you don't implement a process to remove events from the table, the table will continue to grow.</span></span>

<span data-ttu-id="b260b-146">När du skapar ett kluster med hjälp av portalen, vi rekommenderar starkt att du hämtar mallen **innan du klickar på OK** att skapa klustret.</span><span class="sxs-lookup"><span data-stu-id="b260b-146">When you're creating a cluster by using the portal, we highly recommend that you download the template **before you click OK** to create the cluster.</span></span> <span data-ttu-id="b260b-147">Mer information finns i [ställer in ett Service Fabric-kluster med en Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="b260b-147">For details, refer to [Set up a Service Fabric cluster by using an Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="b260b-148">Du behöver mallen du vill göra ändringarna senare, eftersom du inte kan göra några ändringar med hjälp av portalen.</span><span class="sxs-lookup"><span data-stu-id="b260b-148">You'll need the template to make changes later, because you can't make some changes by using the portal.</span></span>

<span data-ttu-id="b260b-149">Du kan exportera mallar från portalen med hjälp av följande steg.</span><span class="sxs-lookup"><span data-stu-id="b260b-149">You can export templates from the portal by using the following steps.</span></span> <span data-ttu-id="b260b-150">Dessa mallar kan dock vara svårare att använda eftersom de kan ha null-värden som saknar nödvändig information.</span><span class="sxs-lookup"><span data-stu-id="b260b-150">However, these templates can be more difficult to use because they might have null values that are missing required information.</span></span>

1. <span data-ttu-id="b260b-151">Öppna din resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="b260b-151">Open your resource group.</span></span>
2. <span data-ttu-id="b260b-152">Välj **inställningar** Visa inställningar.</span><span class="sxs-lookup"><span data-stu-id="b260b-152">Select **Settings** to display the settings panel.</span></span>
3. <span data-ttu-id="b260b-153">Välj **distributioner** Visa historik distribution.</span><span class="sxs-lookup"><span data-stu-id="b260b-153">Select **Deployments** to display the deployment history panel.</span></span>
4. <span data-ttu-id="b260b-154">Välj en distribution för att visa information om distributionen.</span><span class="sxs-lookup"><span data-stu-id="b260b-154">Select a deployment to display the details of the deployment.</span></span>
5. <span data-ttu-id="b260b-155">Välj **Exportmallen** visa mallen.</span><span class="sxs-lookup"><span data-stu-id="b260b-155">Select **Export Template** to display the template panel.</span></span>
6. <span data-ttu-id="b260b-156">Välj **spara till fil** att exportera en .zip-fil som innehåller mallen, parameter och PowerShell-filer.</span><span class="sxs-lookup"><span data-stu-id="b260b-156">Select **Save to file** to export a .zip file that contains the template, parameter, and PowerShell files.</span></span>

<span data-ttu-id="b260b-157">När du exporterar filerna som du behöver göra en ändring.</span><span class="sxs-lookup"><span data-stu-id="b260b-157">After you export the files, you need to make a modification.</span></span> <span data-ttu-id="b260b-158">Redigera filen parameters.JSON och ta bort den **adminPassword** element.</span><span class="sxs-lookup"><span data-stu-id="b260b-158">Edit the parameters.json file and remove the **adminPassword** element.</span></span> <span data-ttu-id="b260b-159">Detta innebär att en prompt för lösenordet när du kör skriptet för distribution.</span><span class="sxs-lookup"><span data-stu-id="b260b-159">This will cause a prompt for the password when the deployment script is run.</span></span> <span data-ttu-id="b260b-160">När du kör skriptet för distribution, kanske du måste åtgärda null-värden.</span><span class="sxs-lookup"><span data-stu-id="b260b-160">When you're running the deployment script, you might have to fix null parameter values.</span></span>

<span data-ttu-id="b260b-161">Använda nedladdade mallen för att uppdatera en konfiguration:</span><span class="sxs-lookup"><span data-stu-id="b260b-161">To use the downloaded template to update a configuration:</span></span>

1. <span data-ttu-id="b260b-162">Extrahera innehållet till en mapp på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="b260b-162">Extract the contents to a folder on your local computer.</span></span>
2. <span data-ttu-id="b260b-163">Ändra innehållet för att återspegla den nya konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b260b-163">Modify the contents to reflect the new configuration.</span></span>
3. <span data-ttu-id="b260b-164">Starta PowerShell och ändra till den mapp där du extraherade innehållet.</span><span class="sxs-lookup"><span data-stu-id="b260b-164">Start PowerShell and change to the folder where you extracted the content.</span></span>
4. <span data-ttu-id="b260b-165">Kör **deploy.ps1** och Fyll i prenumerations-ID, resursgruppens namn (Använd samma namn för att uppdatera konfigurationen av) och en unik distributionsnamnet.</span><span class="sxs-lookup"><span data-stu-id="b260b-165">Run **deploy.ps1** and fill in the subscription ID, the resource group name (use the same name to update the configuration), and a unique deployment name.</span></span>

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a><span data-ttu-id="b260b-166">Distribuera diagnostik-tillägget som en del av klustret har skapats med hjälp av Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b260b-166">Deploy the Diagnostics extension as part of cluster creation by using Azure Resource Manager</span></span>
<span data-ttu-id="b260b-167">Om du vill skapa ett kluster med hjälp av hanteraren för filserverresurser, som du behöver lägga till diagnostik konfigurationen JSON till hela klustret Resource Manager-mallen innan du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="b260b-167">To create a cluster by using Resource Manager, you need to add the Diagnostics configuration JSON to the full cluster Resource Manager template before you create the cluster.</span></span> <span data-ttu-id="b260b-168">Vi ger en fem-VM klustret Resource Manager exempelmall diagnostik-konfiguration som lagts till som en del av våra exempel för Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="b260b-168">We provide a sample five-VM cluster Resource Manager template with Diagnostics configuration added to it as part of our Resource Manager template samples.</span></span> <span data-ttu-id="b260b-169">Du kan se den på den här platsen i galleriet Azure-exempel: [kluster med fem noder med diagnostik Resource Manager mallen exempel](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).</span><span class="sxs-lookup"><span data-stu-id="b260b-169">You can see it at this location in the Azure Samples gallery: [Five-node cluster with Diagnostics Resource Manager template sample](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).</span></span>

<span data-ttu-id="b260b-170">Öppna filen azuredeploy.json för att se inställningen diagnostik i Resource Manager-mallen, och Sök efter **IaaSDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="b260b-170">To see the Diagnostics setting in the Resource Manager template, open the azuredeploy.json file and search for **IaaSDiagnostics**.</span></span> <span data-ttu-id="b260b-171">Om du vill skapa ett kluster med hjälp av den här mallen, Välj den **till Azure** knappen tillgänglig vid föregående länk.</span><span class="sxs-lookup"><span data-stu-id="b260b-171">To create a cluster by using this template, select the **Deploy to Azure** button available at the previous link.</span></span>

<span data-ttu-id="b260b-172">Du kan hämta Resource Manager-exempel, göra ändringar, och skapa ett kluster med den ändrade mallen med hjälp av den `New-AzureRmResourceGroupDeployment` i Azure PowerShell-fönster.</span><span class="sxs-lookup"><span data-stu-id="b260b-172">Alternatively, you can download the Resource Manager sample, make changes to it, and create a cluster with the modified template by using the `New-AzureRmResourceGroupDeployment` command in an Azure PowerShell window.</span></span> <span data-ttu-id="b260b-173">Se följande kod för de parametrar som du anger för att kommandot.</span><span class="sxs-lookup"><span data-stu-id="b260b-173">See the following code for the parameters that you pass in to the command.</span></span> <span data-ttu-id="b260b-174">Detaljerad information om hur du distribuerar en resursgrupp med hjälp av PowerShell finns i artikeln [distribuera en resursgrupp med Azure Resource Manager-mallen](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="b260b-174">For detailed information on how to deploy a resource group by using PowerShell, see the article [Deploy a resource group with the Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a><span data-ttu-id="b260b-175">Distribuera tillägget diagnostik till ett befintligt kluster</span><span class="sxs-lookup"><span data-stu-id="b260b-175">Deploy the Diagnostics extension to an existing cluster</span></span>
<span data-ttu-id="b260b-176">Om du har ett befintligt kluster som inte har distribuerats diagnostik eller om du vill ändra en befintlig konfiguration, kan du lägga till eller uppdatera det.</span><span class="sxs-lookup"><span data-stu-id="b260b-176">If you have an existing cluster that doesn't have Diagnostics deployed, or if you want to modify an existing configuration, you can add or update it.</span></span> <span data-ttu-id="b260b-177">Ändra Resource Manager-mallen som används för att skapa det befintliga klustret eller hämta mallen från portalen, enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="b260b-177">Modify the Resource Manager template that's used to create the existing cluster or download the template from the portal as described earlier.</span></span> <span data-ttu-id="b260b-178">Ändra filen template.json genom att utföra följande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="b260b-178">Modify the template.json file by performing the following tasks.</span></span>

<span data-ttu-id="b260b-179">Lägga till en ny storage-resurs i mallen genom att lägga till avsnittet resurser.</span><span class="sxs-lookup"><span data-stu-id="b260b-179">Add a new storage resource to the template by adding to the resources section.</span></span>

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

 <span data-ttu-id="b260b-180">Lägg till avsnittet Parametrar precis efter storage-konto definitioner, mellan `supportLogStorageAccountName` och `vmNodeType0Name`.</span><span class="sxs-lookup"><span data-stu-id="b260b-180">Next, add to the parameters section just after the storage account definitions, between `supportLogStorageAccountName` and `vmNodeType0Name`.</span></span> <span data-ttu-id="b260b-181">Ersätt platshållartexten *lagringskontonamnet här* med namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="b260b-181">Replace the placeholder text *storage account name goes here* with the name of the storage account.</span></span>

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
<span data-ttu-id="b260b-182">Sedan uppdatera den `VirtualMachineProfile` i template.json-filen genom att lägga till följande kod i en matris för tillägg.</span><span class="sxs-lookup"><span data-stu-id="b260b-182">Then, update the `VirtualMachineProfile` section of the template.json file by adding the following code within the extensions array.</span></span> <span data-ttu-id="b260b-183">Glöm inte att lägga till ett kommatecken i början eller slutet, beroende på om det har infogats.</span><span class="sxs-lookup"><span data-stu-id="b260b-183">Be sure to add a comma at the beginning or the end, depending on where it's inserted.</span></span>

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

<span data-ttu-id="b260b-184">När du har ändrat filen template.json enligt publicera Resource Manager-mallen.</span><span class="sxs-lookup"><span data-stu-id="b260b-184">After you modify the template.json file as described, republish the Resource Manager template.</span></span> <span data-ttu-id="b260b-185">Om mallen exporterades, publicerar körs filen deploy.ps1 mallen.</span><span class="sxs-lookup"><span data-stu-id="b260b-185">If the template was exported, running the deploy.ps1 file republishes the template.</span></span> <span data-ttu-id="b260b-186">När du har distribuerat, se till att **ProvisioningState** är **lyckades**.</span><span class="sxs-lookup"><span data-stu-id="b260b-186">After you deploy, ensure that **ProvisioningState** is **Succeeded**.</span></span>

## <a name="update-diagnostics-to-collect-health-and-load-events"></a><span data-ttu-id="b260b-187">Uppdatera diagnostik för att samla in hälsa och läsa in händelser</span><span class="sxs-lookup"><span data-stu-id="b260b-187">Update diagnostics to collect health and load events</span></span>

<span data-ttu-id="b260b-188">Från och med 5.4 för Service Fabric, är hälsotillstånd och Läs in mått händelser tillgängliga för samlingen.</span><span class="sxs-lookup"><span data-stu-id="b260b-188">Starting with the 5.4 release of Service Fabric, health and load metric events are available for collection.</span></span> <span data-ttu-id="b260b-189">Dessa händelser återspeglar händelser som genererats av systemet eller din kod med hjälp av hälsotillstånd eller läsa in reporting API: er som [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) eller [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span><span class="sxs-lookup"><span data-stu-id="b260b-189">These events reflect events generated by the system or your code by using the health or load reporting APIs such as [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) or [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx).</span></span> <span data-ttu-id="b260b-190">Detta gör för att sammanställa och visa systemhälsa över tid och aviseringar baserat på händelser hälsa eller belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="b260b-190">This allows for aggregating and viewing system health over time and for alerting based on health or load events.</span></span> <span data-ttu-id="b260b-191">Visa dessa händelser i Loggboken för Visual Studio diagnostiska lägga till ”Microsoft-ServiceFabric:4:0x4000000000000008” i listan över ETW-providers.</span><span class="sxs-lookup"><span data-stu-id="b260b-191">To view these events in Visual Studio's Diagnostic Event Viewer add "Microsoft-ServiceFabric:4:0x4000000000000008" to the list of ETW providers.</span></span>

<span data-ttu-id="b260b-192">Ändra resource manager-mall att inkludera för att samla in händelser</span><span class="sxs-lookup"><span data-stu-id="b260b-192">To collect the events, modify the resource manager template to include</span></span>

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

## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a><span data-ttu-id="b260b-193">Uppdatera diagnostik för att samla in och ladda upp loggar från den nya EventSource kanaler</span><span class="sxs-lookup"><span data-stu-id="b260b-193">Update Diagnostics to collect and upload logs from new EventSource channels</span></span>
<span data-ttu-id="b260b-194">Om du vill uppdatera diagnostik för att samla in loggar från nya EventSource kanaler som representerar ett nytt program som du håller på att distribuera utför du samma steg som i den [föregående avsnitt](#deploywadarm) för inställning av diagnostik för ett befintligt kluster.</span><span class="sxs-lookup"><span data-stu-id="b260b-194">To update Diagnostics to collect logs from new EventSource channels that represent a new application that you're about to deploy, perform the same steps as in the [previous section](#deploywadarm) for setup of Diagnostics for an existing cluster.</span></span>

<span data-ttu-id="b260b-195">Uppdatera den `EtwEventSourceProviderConfiguration` -avsnittet i template.json att lägga till poster för de nya EventSource kanalerna innan du använder konfigurationen uppdatera med hjälp av den `New-AzureRmResourceGroupDeployment` PowerShell-kommando.</span><span class="sxs-lookup"><span data-stu-id="b260b-195">Update the `EtwEventSourceProviderConfiguration` section in the template.json file to add entries for the new EventSource channels before you apply the configuration update by using the `New-AzureRmResourceGroupDeployment` PowerShell command.</span></span> <span data-ttu-id="b260b-196">Namnet på händelsekällan definieras som en del av din kod i Visual Studio-genererade ServiceEventSource.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="b260b-196">The name of the event source is defined as part of your code in the Visual Studio-generated ServiceEventSource.cs file.</span></span>

<span data-ttu-id="b260b-197">Till exempel om din händelsekällan är Mina Eventsource, lägger du till följande kod för att placera händelser från Mina Eventsource i en tabell med namnet MyDestinationTableName.</span><span class="sxs-lookup"><span data-stu-id="b260b-197">For example, if your event source is named My-Eventsource, add the following code to place the events from My-Eventsource into a table named MyDestinationTableName.</span></span>

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

<span data-ttu-id="b260b-198">Om du vill samla in prestandaräknare eller händelseloggar, ändra Resource Manager-mallen med hjälp av exemplen i [skapa en virtuell Windows-dator med övervakning och diagnostik med en Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b260b-198">To collect performance counters or event logs, modify the Resource Manager template by using the examples provided in [Create a Windows virtual machine with monitoring and diagnostics by using an Azure Resource Manager template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="b260b-199">Kubfilen Resource Manager-mallen.</span><span class="sxs-lookup"><span data-stu-id="b260b-199">Then, republish the Resource Manager template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b260b-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b260b-200">Next steps</span></span>
<span data-ttu-id="b260b-201">För att förstå vilka händelser som du ska leta efter vid felsökning av problem i detalj, se diagnostiska händelserna orsakat för [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) och [Reliable Services](service-fabric-reliable-services-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="b260b-201">To understand in more detail what events you should look for while troubleshooting issues, see the diagnostic events emitted for [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) and [Reliable Services](service-fabric-reliable-services-diagnostics.md).</span></span>

## <a name="related-articles"></a><span data-ttu-id="b260b-202">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="b260b-202">Related articles</span></span>
* [<span data-ttu-id="b260b-203">Lär dig att samla in prestandaräknare eller loggar med hjälp av diagnostik-tillägget</span><span class="sxs-lookup"><span data-stu-id="b260b-203">Learn how to collect performance counters or logs by using the Diagnostics extension</span></span>](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="b260b-204">Service Fabric-lösning i logganalys</span><span class="sxs-lookup"><span data-stu-id="b260b-204">Service Fabric solution in Log Analytics</span></span>](../log-analytics/log-analytics-service-fabric.md)

