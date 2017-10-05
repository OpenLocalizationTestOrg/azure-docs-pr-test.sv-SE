---
title: Etablera webbprogram med Redis-Cache
description: "Använd Azure Resource Manager-mall för att distribuera webbprogram med Redis-Cache."
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
ms.openlocfilehash: 810c1cedd4fe0bd6ecdf9bd32dfb241f5f345300
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-plus-redis-cache-using-a-template"></a><span data-ttu-id="d5a3a-103">Skapa en Webbapp plus Redis-Cache med hjälp av en mall</span><span class="sxs-lookup"><span data-stu-id="d5a3a-103">Create a Web App plus Redis Cache using a template</span></span>
<span data-ttu-id="d5a3a-104">I det här avsnittet får du lära dig hur du skapar en Azure Resource Manager-mall som distribuerar en Azure-Webbapp med Redis-cache.</span><span class="sxs-lookup"><span data-stu-id="d5a3a-104">In this topic, you will learn how to create an Azure Resource Manager template that deploys an Azure Web App with Redis cache.</span></span> <span data-ttu-id="d5a3a-105">Du kommer lära dig hur du definierar vilka resurser har distribuerats och hur du definierar parametrar som anges när distributionen körs.</span><span class="sxs-lookup"><span data-stu-id="d5a3a-105">You will learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="d5a3a-106">Du kan använda den här mallen för dina egna distributioner eller anpassa den så att den uppfyller dina krav.</span><span class="sxs-lookup"><span data-stu-id="d5a3a-106">You can use this template for your own deployments, or customize it to meet your requirements.</span></span>

<span data-ttu-id="d5a3a-107">Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d5a3a-107">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="d5a3a-108">Den fullständiga mallen finns [webbprogram med Redis-Cache mallen](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="d5a3a-108">For the complete template, see [Web App with Redis Cache template](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).</span></span>

## <a name="what-you-will-deploy"></a><span data-ttu-id="d5a3a-109">Vad du ska distribuera</span><span class="sxs-lookup"><span data-stu-id="d5a3a-109">What you will deploy</span></span>
<span data-ttu-id="d5a3a-110">I den här mallen kan du distribuera:</span><span class="sxs-lookup"><span data-stu-id="d5a3a-110">In this template, you will deploy:</span></span>

* <span data-ttu-id="d5a3a-111">Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="d5a3a-111">Azure Web App</span></span>
* <span data-ttu-id="d5a3a-112">Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="d5a3a-112">Azure Redis Cache.</span></span>

<span data-ttu-id="d5a3a-113">Klicka på följande knapp för att köra distributionen automatiskt:</span><span class="sxs-lookup"><span data-stu-id="d5a3a-113">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="d5a3a-114">[![Distribuera till Azure](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="d5a3a-114">[![Deploy to Azure](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)</span></span>

## <a name="parameters-to-specify"></a><span data-ttu-id="d5a3a-115">Parametrar för att ange</span><span class="sxs-lookup"><span data-stu-id="d5a3a-115">Parameters to specify</span></span>
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

[!INCLUDE [cache-deploy-parameters](../../includes/cache-deploy-parameters.md)]

## <a name="variables-for-names"></a><span data-ttu-id="d5a3a-116">Variabler för namn</span><span class="sxs-lookup"><span data-stu-id="d5a3a-116">Variables for names</span></span>
<span data-ttu-id="d5a3a-117">Den här mallen använder variabler för att skapa namn för resurser.</span><span class="sxs-lookup"><span data-stu-id="d5a3a-117">This template uses variables to construct names for the resources.</span></span> <span data-ttu-id="d5a3a-118">Den använder den [uniqueString](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) funktion för att konstruera ett värde baserat på resurs-id för gruppen.</span><span class="sxs-lookup"><span data-stu-id="d5a3a-118">It uses the [uniqueString](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) function to construct a value based on the resource group id.</span></span>

    "variables": {
      "hostingPlanName": "[concat('hostingplan', uniqueString(resourceGroup().id))]",
      "webSiteName": "[concat('webSite', uniqueString(resourceGroup().id))]",
      "cacheName": "[concat('cache', uniqueString(resourceGroup().id))]"
    },


## <a name="resources-to-deploy"></a><span data-ttu-id="d5a3a-119">Resurser som ska distribueras</span><span class="sxs-lookup"><span data-stu-id="d5a3a-119">Resources to deploy</span></span>
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="redis-cache"></a><span data-ttu-id="d5a3a-120">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="d5a3a-120">Redis Cache</span></span>
<span data-ttu-id="d5a3a-121">Skapar Azure Redis-Cache som används med webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d5a3a-121">Creates the Azure Redis Cache that is used with the web app.</span></span> <span data-ttu-id="d5a3a-122">Namn på cache har angetts i den **cacheName** variabeln.</span><span class="sxs-lookup"><span data-stu-id="d5a3a-122">The name of the cache is specified in the **cacheName** variable.</span></span>

<span data-ttu-id="d5a3a-123">Cachen skapas på samma plats som resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d5a3a-123">The template creates the cache in the same location as the resource group.</span></span>

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


### <a name="web-app"></a><span data-ttu-id="d5a3a-124">Webbapp</span><span class="sxs-lookup"><span data-stu-id="d5a3a-124">Web app</span></span>
<span data-ttu-id="d5a3a-125">Skapar webbprogrammet med namnet som angetts i den **webSiteName** variabeln.</span><span class="sxs-lookup"><span data-stu-id="d5a3a-125">Creates the web app with name specified in the **webSiteName** variable.</span></span>

<span data-ttu-id="d5a3a-126">Observera att webbappen har konfigurerats med egenskaper för appen som möjliggör arbete med Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="d5a3a-126">Notice that the web app is configured with app setting properties that enable it to work with the Redis Cache.</span></span> <span data-ttu-id="d5a3a-127">Den här appen inställningar skapas dynamiskt baserat på värden som anges under distributionen.</span><span class="sxs-lookup"><span data-stu-id="d5a3a-127">This app settings are dynamically created based on values provided during deployment.</span></span>

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

## <a name="commands-to-run-deployment"></a><span data-ttu-id="d5a3a-128">Kommandon för att köra distributionen</span><span class="sxs-lookup"><span data-stu-id="d5a3a-128">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="d5a3a-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d5a3a-129">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="d5a3a-130">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d5a3a-130">Azure CLI</span></span>
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -g ExampleDeployGroup
