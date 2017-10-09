---
title: aaaProvision webbprogram med Redis-Cache
description: "Använd Azure Resource Manager-mall toodeploy webbprogram med Redis-Cache."
services: app-service
documentationcenter: 
author: steved0x
manager: erickson-doug
editor: 
ms.assetid: 6e99c71f-ef8e-4570-a307-e4c059e60c35
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: sdanie
ms.openlocfilehash: b95b5e230dc40c1157940c2017cba836975b6930
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-plus-redis-cache-using-a-template"></a><span data-ttu-id="be2dc-103">Skapa en Webbapp plus Redis-Cache med hjälp av en mall</span><span class="sxs-lookup"><span data-stu-id="be2dc-103">Create a Web App plus Redis Cache using a template</span></span>
<span data-ttu-id="be2dc-104">I det här avsnittet får du lära dig hur toocreate en Azure Resource Manager-mall som distribuerar en Azure-Webbapp med Redis-cache.</span><span class="sxs-lookup"><span data-stu-id="be2dc-104">In this topic, you will learn how toocreate an Azure Resource Manager template that deploys an Azure Web App with Redis cache.</span></span> <span data-ttu-id="be2dc-105">Du får lära dig hur toodefine vilka resurser har distribuerats och hur toodefine parametrar som anges när hello distributionen körs.</span><span class="sxs-lookup"><span data-stu-id="be2dc-105">You will learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="be2dc-106">Du kan använda den här mallen för din egen distribution eller anpassa den toomeet dina krav.</span><span class="sxs-lookup"><span data-stu-id="be2dc-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="be2dc-107">Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="be2dc-107">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="be2dc-108">Hello fullständig mall, se [webbprogram med Redis-Cache mallen](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="be2dc-108">For hello complete template, see [Web App with Redis Cache template](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).</span></span>

## <a name="what-you-will-deploy"></a><span data-ttu-id="be2dc-109">Vad du ska distribuera</span><span class="sxs-lookup"><span data-stu-id="be2dc-109">What you will deploy</span></span>
<span data-ttu-id="be2dc-110">I den här mallen kan du distribuera:</span><span class="sxs-lookup"><span data-stu-id="be2dc-110">In this template, you will deploy:</span></span>

* <span data-ttu-id="be2dc-111">Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="be2dc-111">Azure Web App</span></span>
* <span data-ttu-id="be2dc-112">Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="be2dc-112">Azure Redis Cache.</span></span>

<span data-ttu-id="be2dc-113">toorun Hej distributionen automatiskt, klickar du på följande knapp hello:</span><span class="sxs-lookup"><span data-stu-id="be2dc-113">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="be2dc-114">[![Distribuera tooAzure](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="be2dc-114">[![Deploy tooAzure](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)</span></span>

## <a name="parameters-toospecify"></a><span data-ttu-id="be2dc-115">Parametrarna toospecify</span><span class="sxs-lookup"><span data-stu-id="be2dc-115">Parameters toospecify</span></span>
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

[!INCLUDE [cache-deploy-parameters](../../includes/cache-deploy-parameters.md)]

## <a name="variables-for-names"></a><span data-ttu-id="be2dc-116">Variabler för namn</span><span class="sxs-lookup"><span data-stu-id="be2dc-116">Variables for names</span></span>
<span data-ttu-id="be2dc-117">Den här mallen använder variabler tooconstruct namn för hello resurser.</span><span class="sxs-lookup"><span data-stu-id="be2dc-117">This template uses variables tooconstruct names for hello resources.</span></span> <span data-ttu-id="be2dc-118">Den använder hello [uniqueString](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) fungerar tooconstruct ett värde baserat på resurs-id för gruppen.</span><span class="sxs-lookup"><span data-stu-id="be2dc-118">It uses hello [uniqueString](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) function tooconstruct a value based on the resource group id.</span></span>

    "variables": {
      "hostingPlanName": "[concat('hostingplan', uniqueString(resourceGroup().id))]",
      "webSiteName": "[concat('webSite', uniqueString(resourceGroup().id))]",
      "cacheName": "[concat('cache', uniqueString(resourceGroup().id))]"
    },


## <a name="resources-toodeploy"></a><span data-ttu-id="be2dc-119">Resurser toodeploy</span><span class="sxs-lookup"><span data-stu-id="be2dc-119">Resources toodeploy</span></span>
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="redis-cache"></a><span data-ttu-id="be2dc-120">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="be2dc-120">Redis Cache</span></span>
<span data-ttu-id="be2dc-121">Skapar hello Azure Redis-Cache som används med hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="be2dc-121">Creates hello Azure Redis Cache that is used with hello web app.</span></span> <span data-ttu-id="be2dc-122">hello namn hello-cache har angetts i hello **cacheName** variabeln.</span><span class="sxs-lookup"><span data-stu-id="be2dc-122">hello name of hello cache is specified in hello **cacheName** variable.</span></span>

<span data-ttu-id="be2dc-123">hello mallen skapar hello-cache i hello samma plats som hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="be2dc-123">hello template creates hello cache in hello same location as hello resource group.</span></span>

    {
      "name": "[variables('cacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [ ],
      "tags": {
        "displayName": "cache"
      },
      "properties": {
        "sku": {
          "name": "[parameters('cacheSKUName')]",
          "family": "[parameters('cacheSKUFamily')]",
          "capacity": "[parameters('cacheSKUCapacity')]"
        }
      }
    }


### <a name="web-app"></a><span data-ttu-id="be2dc-124">Webbapp</span><span class="sxs-lookup"><span data-stu-id="be2dc-124">Web app</span></span>
<span data-ttu-id="be2dc-125">Skapar hello webbprogrammet med namnet som angetts i hello **webSiteName** variabeln.</span><span class="sxs-lookup"><span data-stu-id="be2dc-125">Creates hello web app with name specified in hello **webSiteName** variable.</span></span>

<span data-ttu-id="be2dc-126">Observera att hello webbprogram har konfigurerats med appen och ange egenskaper som möjliggör toowork med hello Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="be2dc-126">Notice that hello web app is configured with app setting properties that enable it toowork with hello Redis Cache.</span></span> <span data-ttu-id="be2dc-127">Den här appen inställningar skapas dynamiskt baserat på värden som anges under distributionen.</span><span class="sxs-lookup"><span data-stu-id="be2dc-127">This app settings are dynamically created based on values provided during deployment.</span></span>

    {
      "apiVersion": "2015-08-01",
      "name": "[variables('webSiteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Web/serverFarms/', variables('hostingPlanName'))]",
        "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
      ],
      "tags": {
        "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', variables('hostingPlanName'))]": "empty",
        "displayName": "Website"
      },
      "properties": {
        "name": "[variables('webSiteName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "type": "config",
          "name": "appsettings",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', variables('webSiteName'))]",
            "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
          ],
          "properties": {
            "CacheConnection": "[concat(variables('cacheName'),'.redis.cache.windows.net,abortConnect=false,ssl=true,password=', listKeys(resourceId('Microsoft.Cache/Redis', variables('cacheName')), '2015-08-01').primaryKey)]"
          }
        }
      ]
    }

## <a name="commands-toorun-deployment"></a><span data-ttu-id="be2dc-128">Kommandon toorun distribution</span><span class="sxs-lookup"><span data-stu-id="be2dc-128">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="be2dc-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="be2dc-129">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="be2dc-130">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="be2dc-130">Azure CLI</span></span>
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -g ExampleDeployGroup
