---
title: aaaProvision Redis-Cache med Azure Resource Manager | Microsoft Docs
description: "Använd Azure Resource Manager mallen toodeploy ett Azure Redis-Cache."
services: app-service
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: ce6f5372-7038-4655-b1c5-108f7c148282
ms.service: cache
ms.workload: web
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: sdanie
ms.openlocfilehash: 46e7b3b2493ac51dbe6bab0b086304802afc5d48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-redis-cache-using-a-template"></a><span data-ttu-id="e1be0-103">Skapa en Redis Cache med hjälp av en mall</span><span class="sxs-lookup"><span data-stu-id="e1be0-103">Create a Redis Cache using a template</span></span>
<span data-ttu-id="e1be0-104">I det här avsnittet lär du dig hur toocreate en Azure Resource Manager-mall som distribuerar ett Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="e1be0-104">In this topic, you learn how toocreate an Azure Resource Manager template that deploys an Azure Redis Cache.</span></span> <span data-ttu-id="e1be0-105">hello cache kan användas med en befintlig lagring konto tookeep diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="e1be0-105">hello cache can be used with an existing storage account tookeep diagnostic data.</span></span> <span data-ttu-id="e1be0-106">Du också lära dig hur toodefine vilka resurser har distribuerats och hur toodefine parametrar som anges när hello distributionen körs.</span><span class="sxs-lookup"><span data-stu-id="e1be0-106">You also learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="e1be0-107">Du kan använda den här mallen för din egen distribution eller anpassa den toomeet dina krav.</span><span class="sxs-lookup"><span data-stu-id="e1be0-107">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="e1be0-108">För närvarande diagnostikinställningar delas för alla cacheminnen i hello samma region för en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e1be0-108">Currently, diagnostic settings are shared for all caches in hello same region for a subscription.</span></span> <span data-ttu-id="e1be0-109">Uppdatera en cacheminnet i hello region påverkar alla cacheminnen i hello region.</span><span class="sxs-lookup"><span data-stu-id="e1be0-109">Updating one cache in hello region affects all other caches in hello region.</span></span>

<span data-ttu-id="e1be0-110">Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="e1be0-110">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="e1be0-111">Hello fullständig mall, se [Redis-Cache mallen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="e1be0-111">For hello complete template, see [Redis Cache template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).</span></span>

> [!NOTE]
> <span data-ttu-id="e1be0-112">Resource Manager-mallar för nya hello [premiumnivån](cache-premium-tier-intro.md) är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="e1be0-112">Resource Manager templates for hello new [Premium tier](cache-premium-tier-intro.md) are available.</span></span> 
> 
> * [<span data-ttu-id="e1be0-113">Skapa en Premium Redis-Cache med kluster</span><span class="sxs-lookup"><span data-stu-id="e1be0-113">Create a Premium Redis Cache with clustering</span></span>](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
> * [<span data-ttu-id="e1be0-114">Skapa Premium Redis-Cache med-datapersistence</span><span class="sxs-lookup"><span data-stu-id="e1be0-114">Create Premium Redis Cache with data persistence</span></span>](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
> * [<span data-ttu-id="e1be0-115">Skapa Premium Redis-Cache med virtuella nätverk och valfritt kluster</span><span class="sxs-lookup"><span data-stu-id="e1be0-115">Create Premium Redis Cache with VNet and optional clustering</span></span>](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
> 
> <span data-ttu-id="e1be0-116">toocheck för hello senaste mallar, se [Azure-Snabbstartsmallar](https://azure.microsoft.com/documentation/templates/) och Sök efter `Redis Cache`.</span><span class="sxs-lookup"><span data-stu-id="e1be0-116">toocheck for hello latest templates, see [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates/) and search for `Redis Cache`.</span></span>
> 
> 

## <a name="what-you-will-deploy"></a><span data-ttu-id="e1be0-117">Vad du ska distribuera</span><span class="sxs-lookup"><span data-stu-id="e1be0-117">What you will deploy</span></span>
<span data-ttu-id="e1be0-118">I den här mallen ska du distribuera ett Azure Redis-Cache som använder ett befintligt lagringskonto för diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="e1be0-118">In this template, you will deploy an Azure Redis Cache that uses an existing storage account for diagnostic data.</span></span>

<span data-ttu-id="e1be0-119">toorun Hej distributionen automatiskt, klickar du på följande knapp hello:</span><span class="sxs-lookup"><span data-stu-id="e1be0-119">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="e1be0-120">[![Distribuera tooAzure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="e1be0-120">[![Deploy tooAzure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="e1be0-121">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e1be0-121">Parameters</span></span>
<span data-ttu-id="e1be0-122">Med Azure Resource Manager kan du definiera parametrar för värden som du vill toospecify när hello mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="e1be0-122">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="e1be0-123">hello mallen innehåller ett avsnitt som heter parametrar som innehåller alla hello parametervärden.</span><span class="sxs-lookup"><span data-stu-id="e1be0-123">hello template includes a section called Parameters that contains all of hello parameter values.</span></span>
<span data-ttu-id="e1be0-124">Du bör definiera en parameter för de värden som varierar baserat på hello-projekt som du distribuerar eller på hello-miljö som du distribuerar till.</span><span class="sxs-lookup"><span data-stu-id="e1be0-124">You should define a parameter for those values that vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="e1be0-125">Definiera inte parametrar för värden som alltid hello samma.</span><span class="sxs-lookup"><span data-stu-id="e1be0-125">Do not define parameters for values that always stay hello same.</span></span> <span data-ttu-id="e1be0-126">Varje parametervärdet används i hello mallen toodefine hello resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="e1be0-126">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span> 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a><span data-ttu-id="e1be0-127">redisCacheLocation</span><span class="sxs-lookup"><span data-stu-id="e1be0-127">redisCacheLocation</span></span>
<span data-ttu-id="e1be0-128">hello plats hello Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="e1be0-128">hello location of hello Redis Cache.</span></span> <span data-ttu-id="e1be0-129">För bästa prestanda hello används samma plats som hello app toobe används med hello cache.</span><span class="sxs-lookup"><span data-stu-id="e1be0-129">For best performance, use hello same location as hello app toobe used with hello cache.</span></span>

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a><span data-ttu-id="e1be0-130">existingDiagnosticsStorageAccountName</span><span class="sxs-lookup"><span data-stu-id="e1be0-130">existingDiagnosticsStorageAccountName</span></span>
<span data-ttu-id="e1be0-131">hello namnet på hello befintliga lagring konto toouse för diagnostik.</span><span class="sxs-lookup"><span data-stu-id="e1be0-131">hello name of hello existing storage account toouse for diagnostics.</span></span> 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a><span data-ttu-id="e1be0-132">enableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="e1be0-132">enableNonSslPort</span></span>
<span data-ttu-id="e1be0-133">Ett booleskt värde som anger om tooallow åtkomst via SSL-portar.</span><span class="sxs-lookup"><span data-stu-id="e1be0-133">A boolean value that indicates whether tooallow access via non-SSL ports.</span></span>

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a><span data-ttu-id="e1be0-134">diagnosticsStatus</span><span class="sxs-lookup"><span data-stu-id="e1be0-134">diagnosticsStatus</span></span>
<span data-ttu-id="e1be0-135">Ett värde som anger om diagnostik är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="e1be0-135">A value that indicates whether diagnostics is enabled.</span></span> <span data-ttu-id="e1be0-136">Använd ON eller OFF.</span><span class="sxs-lookup"><span data-stu-id="e1be0-136">Use ON or OFF.</span></span>

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## <a name="resources-toodeploy"></a><span data-ttu-id="e1be0-137">Resurser toodeploy</span><span class="sxs-lookup"><span data-stu-id="e1be0-137">Resources toodeploy</span></span>
### <a name="redis-cache"></a><span data-ttu-id="e1be0-138">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="e1be0-138">Redis Cache</span></span>
<span data-ttu-id="e1be0-139">Skapar hello Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="e1be0-139">Creates hello Azure Redis Cache.</span></span>

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-07-01",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-toorun-deployment"></a><span data-ttu-id="e1be0-140">Kommandon toorun distribution</span><span class="sxs-lookup"><span data-stu-id="e1be0-140">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="e1be0-141">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e1be0-141">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache

### <a name="azure-cli"></a><span data-ttu-id="e1be0-142">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e1be0-142">Azure CLI</span></span>
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


