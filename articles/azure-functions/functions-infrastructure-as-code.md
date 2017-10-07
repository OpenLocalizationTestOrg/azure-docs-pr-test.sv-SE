---
title: "aaaAutomate resurs distributionen för en funktionsapp i Azure Functions | Microsoft Docs"
description: "Lär dig hur toobuild en Azure Resource Manager-mall som distribuerar appen funktion."
services: Functions
documtationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "Azure functions, funktioner, serverlösa arkitektur, infrastruktur som kod, azure resource manager"
ms.assetid: d20743e3-aab6-442c-a836-9bcea09bfd32
ms.server: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: glenga
ms.openlocfilehash: b0df0d4ef9fe93213f7b1cb1d1e6b4e14f8b3a30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a><span data-ttu-id="9c519-104">Automatisera resurs distribution för din funktionsapp i Azure Functions</span><span class="sxs-lookup"><span data-stu-id="9c519-104">Automate resource deployment for your function app in Azure Functions</span></span>

<span data-ttu-id="9c519-105">Du kan använda en funktionsapp för toodeploy en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="9c519-105">You can use an Azure Resource Manager template toodeploy a function app.</span></span> <span data-ttu-id="9c519-106">Den här artikeln beskrivs hello nödvändiga resurser och parametrar för att göra detta.</span><span class="sxs-lookup"><span data-stu-id="9c519-106">This article outlines hello required resources and parameters for doing so.</span></span> <span data-ttu-id="9c519-107">Du kan behöva toodeploy ytterligare resurser, beroende på hello [utlösare och bindningar](functions-triggers-bindings.md) i funktionen appen.</span><span class="sxs-lookup"><span data-stu-id="9c519-107">You might need toodeploy additional resources, depending on hello [triggers and bindings](functions-triggers-bindings.md) in your function app.</span></span>

<span data-ttu-id="9c519-108">Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9c519-108">For more information about creating templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="9c519-109">För exempelmallar, se:</span><span class="sxs-lookup"><span data-stu-id="9c519-109">For sample templates, see:</span></span>
- <span data-ttu-id="9c519-110">[funktionsapp på förbrukning plan]</span><span class="sxs-lookup"><span data-stu-id="9c519-110">[Function app on Consumption plan]</span></span>
- <span data-ttu-id="9c519-111">[funktionsapp på Azure App Service-plan]</span><span class="sxs-lookup"><span data-stu-id="9c519-111">[Function app on Azure App Service plan]</span></span>

## <a name="required-resources"></a><span data-ttu-id="9c519-112">Resurser som krävs</span><span class="sxs-lookup"><span data-stu-id="9c519-112">Required resources</span></span>

<span data-ttu-id="9c519-113">En funktionsapp kräver följande resurser:</span><span class="sxs-lookup"><span data-stu-id="9c519-113">A function app requires these resources:</span></span>

* <span data-ttu-id="9c519-114">En [Azure Storage](../storage/index.md) konto</span><span class="sxs-lookup"><span data-stu-id="9c519-114">An [Azure Storage](../storage/index.md) account</span></span>
* <span data-ttu-id="9c519-115">En värd plan (förbrukning plan eller programtjänstplanen)</span><span class="sxs-lookup"><span data-stu-id="9c519-115">A hosting plan (Consumption plan or App Service plan)</span></span>
* <span data-ttu-id="9c519-116">En funktionsapp</span><span class="sxs-lookup"><span data-stu-id="9c519-116">A function app</span></span> 

### <a name="storage-account"></a><span data-ttu-id="9c519-117">Lagringskonto</span><span class="sxs-lookup"><span data-stu-id="9c519-117">Storage account</span></span>

<span data-ttu-id="9c519-118">Ett Azure storage-konto krävs för en funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="9c519-118">An Azure storage account is required for a function app.</span></span> <span data-ttu-id="9c519-119">Du behöver en generella-konto som har stöd för blobbar, tabeller, köer och filer.</span><span class="sxs-lookup"><span data-stu-id="9c519-119">You need a general purpose account that supports blobs, tables, queues, and files.</span></span> <span data-ttu-id="9c519-120">Mer information finns i [Azure Functions lagringskraven för kontot](functions-create-function-app-portal.md#storage-account-requirements).</span><span class="sxs-lookup"><span data-stu-id="9c519-120">For more information, see [Azure Functions storage account requirements](functions-create-function-app-portal.md#storage-account-requirements).</span></span>

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
        "accountType": "[parameters('storageAccountType')]"
    }
}
```

<span data-ttu-id="9c519-121">Dessutom hello egenskaper `AzureWebJobsStorage` och `AzureWebJobsDashboard` måste anges som app-inställningar i hello platskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="9c519-121">In addition, hello properties `AzureWebJobsStorage` and `AzureWebJobsDashboard` must be specified as app settings in hello site configuration.</span></span> <span data-ttu-id="9c519-122">hello Azure Functions-runtime använder hello `AzureWebJobsStorage` toocreate interna köer för anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="9c519-122">hello Azure Functions runtime uses hello `AzureWebJobsStorage` connection string toocreate internal queues.</span></span> <span data-ttu-id="9c519-123">Hej anslutningssträngen `AzureWebJobsDashboard` är används toolog tooAzure tabell lagrings- och hello **övervakaren** fliken hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c519-123">hello connection string `AzureWebJobsDashboard` is used toolog tooAzure Table storage and power hello **Monitor** tab in hello portal.</span></span>

<span data-ttu-id="9c519-124">Dessa egenskaper anges i hello `appSettings` samling i hello `siteConfig` objekt:</span><span class="sxs-lookup"><span data-stu-id="9c519-124">These properties are specified in hello `appSettings` collection in hello `siteConfig` object:</span></span>

```json
"appSettings": [
    {
        "name": "AzureWebJobsStorage",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    },
    {
        "name": "AzureWebJobsDashboard",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    }
```    

### <a name="hosting-plan"></a><span data-ttu-id="9c519-125">Värd för planen</span><span class="sxs-lookup"><span data-stu-id="9c519-125">Hosting plan</span></span>

<span data-ttu-id="9c519-126">hello definition av hello värd plan varierar beroende på om du använder en förbrukning eller App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="9c519-126">hello definition of hello hosting plan varies, depending on whether you use a Consumption or App Service plan.</span></span> <span data-ttu-id="9c519-127">Se [distribuera en funktionsapp på hello förbrukning plan](#consumption) och [distribuera en funktionsapp på hello programtjänstplanen](#app-service-plan).</span><span class="sxs-lookup"><span data-stu-id="9c519-127">See [Deploy a function app on hello Consumption plan](#consumption) and [Deploy a function app on hello App Service plan](#app-service-plan).</span></span>

### <a name="function-app"></a><span data-ttu-id="9c519-128">Funktionsapp</span><span class="sxs-lookup"><span data-stu-id="9c519-128">Function app</span></span>

<span data-ttu-id="9c519-129">hello funktionen appresursen definieras med hjälp av en resurs av typen **Microsoft.Web/Site** och typ **functionapp**:</span><span class="sxs-lookup"><span data-stu-id="9c519-129">hello function app resource is defined by using a resource of type **Microsoft.Web/Site** and kind **functionapp**:</span></span>

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ]
```

<a name="consumption"></a>

## <a name="deploy-a-function-app-on-hello-consumption-plan"></a><span data-ttu-id="9c519-130">Distribuera en funktionsapp på hello förbrukning plan</span><span class="sxs-lookup"><span data-stu-id="9c519-130">Deploy a function app on hello Consumption plan</span></span>

<span data-ttu-id="9c519-131">Du kan köra en funktionsapp i två olika lägen: hello förbrukning planerings- och hello App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="9c519-131">You can run a function app in two different modes: hello Consumption plan and hello App Service plan.</span></span> <span data-ttu-id="9c519-132">hello förbrukning plan tilldelar automatiskt datorkraft när koden körs skalas ut som behövs toohandle belastning och skalas när koden inte körs.</span><span class="sxs-lookup"><span data-stu-id="9c519-132">hello Consumption plan automatically allocates compute power when your code is running, scales out as necessary toohandle load, and then scales down when code is not running.</span></span> <span data-ttu-id="9c519-133">Därför du inte toopay för inaktiv virtuella datorer och du har inte tooreserve kapacitet i förväg.</span><span class="sxs-lookup"><span data-stu-id="9c519-133">So, you don't have toopay for idle VMs, and you don't have tooreserve capacity in advance.</span></span> <span data-ttu-id="9c519-134">toolearn mer information om värd planer, se [Azure Functions användnings- och App Service-planer](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="9c519-134">toolearn more about hosting plans, see [Azure Functions Consumption and App Service plans](functions-scale.md).</span></span>

<span data-ttu-id="9c519-135">En exempelmall av Azure Resource Manager finns [funktionsapp på förbrukning plan].</span><span class="sxs-lookup"><span data-stu-id="9c519-135">For a sample Azure Resource Manager template, see [Function app on Consumption plan].</span></span>

### <a name="create-a-consumption-plan"></a><span data-ttu-id="9c519-136">Skapa en plan för förbrukning</span><span class="sxs-lookup"><span data-stu-id="9c519-136">Create a Consumption plan</span></span>

<span data-ttu-id="9c519-137">En plan för förbrukning är en särskild typ av resurs ”serverfarm”.</span><span class="sxs-lookup"><span data-stu-id="9c519-137">A Consumption plan is a special type of "serverfarm" resource.</span></span> <span data-ttu-id="9c519-138">Du anger det med hjälp av hello `Dynamic` värde för hello `computeMode` och `sku` egenskaper:</span><span class="sxs-lookup"><span data-stu-id="9c519-138">You specify it by using hello `Dynamic` value for hello `computeMode` and `sku` properties:</span></span>

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "computeMode": "Dynamic",
        "sku": "Dynamic"
    }
}
```

### <a name="create-a-function-app"></a><span data-ttu-id="9c519-139">Skapa en funktionsapp</span><span class="sxs-lookup"><span data-stu-id="9c519-139">Create a function app</span></span>

<span data-ttu-id="9c519-140">Dessutom kan en plan för förbrukning kräver två ytterligare inställningar i hello platskonfiguration: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` och `WEBSITE_CONTENTSHARE`.</span><span class="sxs-lookup"><span data-stu-id="9c519-140">In addition, a Consumption plan requires two additional settings in hello site configuration: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` and `WEBSITE_CONTENTSHARE`.</span></span> <span data-ttu-id="9c519-141">Dessa egenskaper konfigurera hello storage-konto och sökvägen där hello Funktionskoden app och konfiguration lagras.</span><span class="sxs-lookup"><span data-stu-id="9c519-141">These properties configure hello storage account and file path where hello function app code and configuration are stored.</span></span>

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsDashboard",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTSHARE",
                    "value": "[toLower(variables('functionAppName'))]"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~1"
                }
            ]
        }
    }
}
```                    

<a name="app-service-plan"></a> 

## <a name="deploy-a-function-app-on-hello-app-service-plan"></a><span data-ttu-id="9c519-142">Distribuera en funktionsapp på hello App Service-plan</span><span class="sxs-lookup"><span data-stu-id="9c519-142">Deploy a function app on hello App Service plan</span></span>

<span data-ttu-id="9c519-143">Hello App Service-plan körs funktionen appen på dedikerade virtuella datorer på Basic, Standard och Premium-SKU: er, liknande tooweb appar.</span><span class="sxs-lookup"><span data-stu-id="9c519-143">In hello App Service plan, your function app runs on dedicated VMs on Basic, Standard, and Premium SKUs, similar tooweb apps.</span></span> <span data-ttu-id="9c519-144">Mer information om hur hello programtjänstplan fungerar finns hello [Azure App Service-planer djupgående översikt över](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9c519-144">For details about how hello App Service plan works, see hello [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span> 

<span data-ttu-id="9c519-145">En exempelmall av Azure Resource Manager finns [funktionsapp på Azure App Service-plan].</span><span class="sxs-lookup"><span data-stu-id="9c519-145">For a sample Azure Resource Manager template, see [Function app on Azure App Service plan].</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="9c519-146">Skapa en App Service-plan</span><span class="sxs-lookup"><span data-stu-id="9c519-146">Create an App Service plan</span></span>

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "sku": "[parameters('sku')]",
        "workerSize": "[parameters('workerSize')]",
        "hostingEnvironment": "",
        "numberOfWorkers": 1
    }
}
```

### <a name="create-a-function-app"></a><span data-ttu-id="9c519-147">Skapa en funktionsapp</span><span class="sxs-lookup"><span data-stu-id="9c519-147">Create a function app</span></span> 

<span data-ttu-id="9c519-148">När du har valt ett skalningsalternativ, skapa en funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="9c519-148">After you've selected a scaling option, create a function app.</span></span> <span data-ttu-id="9c519-149">hello appen är hello-behållare som innehåller alla funktioner.</span><span class="sxs-lookup"><span data-stu-id="9c519-149">hello app is hello container that holds all your functions.</span></span>

<span data-ttu-id="9c519-150">En funktionsapp har många underordnade resurser som du kan använda i din distribution, inklusive inställningar för appen och ange alternativ för källa.</span><span class="sxs-lookup"><span data-stu-id="9c519-150">A function app has many child resources that you can use in your deployment, including app settings and source control options.</span></span> <span data-ttu-id="9c519-151">Du kan också välja tooremove hello **sourcecontrols** underordnade resursen och använda en annan [distributionsalternativet](functions-continuous-deployment.md) i stället.</span><span class="sxs-lookup"><span data-stu-id="9c519-151">You also might choose tooremove hello **sourcecontrols** child resource, and use a different [deployment option](functions-continuous-deployment.md) instead.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9c519-152">toosuccessfully distribuera ditt program med hjälp av Azure Resource Manager, är det viktigt toounderstand hur resurser har distribuerats i Azure.</span><span class="sxs-lookup"><span data-stu-id="9c519-152">toosuccessfully deploy your application by using Azure Resource Manager, it's important toounderstand how resources are deployed in Azure.</span></span> <span data-ttu-id="9c519-153">I följande exempel hello, tillämpas på den översta nivån konfigurationer med hjälp av **siteConfig**.</span><span class="sxs-lookup"><span data-stu-id="9c519-153">In hello following example, top-level configurations are applied by using **siteConfig**.</span></span> <span data-ttu-id="9c519-154">Det är viktigt tooset konfigurationerna en överst nivå eftersom de förmedla information toohello funktioner distribution och runtime engine.</span><span class="sxs-lookup"><span data-stu-id="9c519-154">It's important tooset these configurations at a top level, because they convey information toohello Functions runtime and deployment engine.</span></span> <span data-ttu-id="9c519-155">Översta information krävs innan hello underordnade **sourcecontrols/web** resurs används.</span><span class="sxs-lookup"><span data-stu-id="9c519-155">Top-level information is required before hello child **sourcecontrols/web** resource is applied.</span></span> <span data-ttu-id="9c519-156">Även om det är möjligt tooconfigure de här inställningarna i hello underordnade **config/appSettings** resurs, i vissa fall måste du distribuera appen funktionen *innan* **config/appSettings**  tillämpas.</span><span class="sxs-lookup"><span data-stu-id="9c519-156">Although it's possible tooconfigure these settings in hello child-level **config/appSettings** resource, in some cases your function app must be deployed *before* **config/appSettings** is applied.</span></span> <span data-ttu-id="9c519-157">Till exempel när du använder funktioner med [Logikappar](../logic-apps/index.md), dina funktioner är beroende av en annan resurs.</span><span class="sxs-lookup"><span data-stu-id="9c519-157">For example, when you are using functions with [Logic Apps](../logic-apps/index.md), your functions are a dependency of another resource.</span></span>

```json
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('appName')]",
  "type": "Microsoft.Web/sites",
  "kind": "functionapp",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Web/serverfarms', parameters('appName'))]"
  ],
  "properties": {
     "serverFarmId": "[variables('appServicePlanName')]",
     "siteConfig": {
        "alwaysOn": true,
        "appSettings": [
            { "name": "FUNCTIONS_EXTENSION_VERSION", "value": "~1" },
            { "name": "Project", "value": "src" }
        ]
     }
  },
  "resources": [
     {
        "apiVersion": "2015-08-01",
        "name": "appsettings",
        "type": "config",
        "dependsOn": [
          "[resourceId('Microsoft.Web/Sites', parameters('appName'))]",
          "[resourceId('Microsoft.Web/Sites/sourcecontrols', parameters('appName'), 'web')]",
          "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
        ],
        "properties": {
          "AzureWebJobsStorage": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]",
          "AzureWebJobsDashboard": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
        }
     },
     {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites/', parameters('appName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('sourceCodeRepositoryURL')]",
            "branch": "[parameters('sourceCodeBranch')]",
            "IsManualIntegration": "[parameters('sourceCodeManualIntegration')]"
          }
     }
  ]
}
```
> [!TIP]
> <span data-ttu-id="9c519-158">Den här mallen använder hello [projekt](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) app inställningar-värde som anger hello baskatalog i vilka hello funktioner distributionsmotorn (Kudu) söker efter distribuerbara kod.</span><span class="sxs-lookup"><span data-stu-id="9c519-158">This template uses hello [Project](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) app settings value, which sets hello base directory in which hello Functions deployment engine (Kudu) looks for deployable code.</span></span> <span data-ttu-id="9c519-159">I vår databas våra funktioner finns i en undermapp till hello **src** mapp.</span><span class="sxs-lookup"><span data-stu-id="9c519-159">In our repository, our functions are in a subfolder of hello **src** folder.</span></span> <span data-ttu-id="9c519-160">Så i föregående exempel hello, vi värdet hello app inställningar för`src`.</span><span class="sxs-lookup"><span data-stu-id="9c519-160">So, in hello preceding example, we set hello app settings value too`src`.</span></span> <span data-ttu-id="9c519-161">Om dina funktioner finns i hello roten för din databas, eller om du inte distribuerar från källkontroll, kan du ta bort det här värdet för app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="9c519-161">If your functions are in hello root of your repository, or if you are not deploying from source control, you can remove this app settings value.</span></span>

## <a name="deploy-your-template"></a><span data-ttu-id="9c519-162">Distribuera mallen</span><span class="sxs-lookup"><span data-stu-id="9c519-162">Deploy your template</span></span>

<span data-ttu-id="9c519-163">Du kan använda något av följande sätt toodeploy hello mallen:</span><span class="sxs-lookup"><span data-stu-id="9c519-163">You can use any of hello following ways toodeploy your template:</span></span>

* [<span data-ttu-id="9c519-164">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9c519-164">PowerShell</span></span>](../azure-resource-manager/resource-group-template-deploy.md)
* [<span data-ttu-id="9c519-165">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9c519-165">Azure CLI</span></span>](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [<span data-ttu-id="9c519-166">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9c519-166">Azure portal</span></span>](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [<span data-ttu-id="9c519-167">REST-API</span><span class="sxs-lookup"><span data-stu-id="9c519-167">REST API</span></span>](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-tooazure-button"></a><span data-ttu-id="9c519-168">TooAzure knappen distribuera</span><span class="sxs-lookup"><span data-stu-id="9c519-168">Deploy tooAzure button</span></span>

<span data-ttu-id="9c519-169">Ersätt ```<url-encoded-path-to-azuredeploy-json>``` med en [URL-kodade](https://www.bing.com/search?q=url+encode) version av hello rådata sökvägen till din `azuredeploy.json` -filen i GitHub.</span><span class="sxs-lookup"><span data-stu-id="9c519-169">Replace ```<url-encoded-path-to-azuredeploy-json>``` with a [URL-encoded](https://www.bing.com/search?q=url+encode) version of hello raw path of your `azuredeploy.json` file in GitHub.</span></span>

<span data-ttu-id="9c519-170">Här är ett exempel som använder markdown:</span><span class="sxs-lookup"><span data-stu-id="9c519-170">Here is an example that uses markdown:</span></span>

```markdown
[![Deploy tooAzure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

<span data-ttu-id="9c519-171">Här är ett exempel som använder HTML:</span><span class="sxs-lookup"><span data-stu-id="9c519-171">Here is an example that uses HTML:</span></span>

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a><span data-ttu-id="9c519-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9c519-172">Next steps</span></span>

<span data-ttu-id="9c519-173">Läs mer om hur toodevelop och konfigurera Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="9c519-173">Learn more about how toodevelop and configure Azure Functions.</span></span>

* [<span data-ttu-id="9c519-174">Azure Functions, info för utvecklare</span><span class="sxs-lookup"><span data-stu-id="9c519-174">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="9c519-175">Hur fungerar tooconfigure Azure app-inställningar</span><span class="sxs-lookup"><span data-stu-id="9c519-175">How tooconfigure Azure function app settings</span></span>](functions-how-to-use-azure-function-app-settings.md)
* [<span data-ttu-id="9c519-176">Skapa din första Azure-funktion</span><span class="sxs-lookup"><span data-stu-id="9c519-176">Create your first Azure function</span></span>](functions-create-first-azure-function.md)

<!-- LINKS -->

[funktionsapp på förbrukning plan]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[funktionsapp på Azure App Service-plan]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json
