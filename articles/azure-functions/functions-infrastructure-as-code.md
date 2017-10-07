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
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a>Automatisera resurs distribution för din funktionsapp i Azure Functions

Du kan använda en funktionsapp för toodeploy en Azure Resource Manager-mall. Den här artikeln beskrivs hello nödvändiga resurser och parametrar för att göra detta. Du kan behöva toodeploy ytterligare resurser, beroende på hello [utlösare och bindningar](functions-triggers-bindings.md) i funktionen appen.

Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).

För exempelmallar, se:
- [funktionsapp på förbrukning plan]
- [funktionsapp på Azure App Service-plan]

## <a name="required-resources"></a>Resurser som krävs

En funktionsapp kräver följande resurser:

* En [Azure Storage](../storage/index.md) konto
* En värd plan (förbrukning plan eller programtjänstplanen)
* En funktionsapp 

### <a name="storage-account"></a>Lagringskonto

Ett Azure storage-konto krävs för en funktionsapp. Du behöver en generella-konto som har stöd för blobbar, tabeller, köer och filer. Mer information finns i [Azure Functions lagringskraven för kontot](functions-create-function-app-portal.md#storage-account-requirements).

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

Dessutom hello egenskaper `AzureWebJobsStorage` och `AzureWebJobsDashboard` måste anges som app-inställningar i hello platskonfiguration. hello Azure Functions-runtime använder hello `AzureWebJobsStorage` toocreate interna köer för anslutningssträngen. Hej anslutningssträngen `AzureWebJobsDashboard` är används toolog tooAzure tabell lagrings- och hello **övervakaren** fliken hello-portalen.

Dessa egenskaper anges i hello `appSettings` samling i hello `siteConfig` objekt:

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

### <a name="hosting-plan"></a>Värd för planen

hello definition av hello värd plan varierar beroende på om du använder en förbrukning eller App Service-plan. Se [distribuera en funktionsapp på hello förbrukning plan](#consumption) och [distribuera en funktionsapp på hello programtjänstplanen](#app-service-plan).

### <a name="function-app"></a>Funktionsapp

hello funktionen appresursen definieras med hjälp av en resurs av typen **Microsoft.Web/Site** och typ **functionapp**:

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

## <a name="deploy-a-function-app-on-hello-consumption-plan"></a>Distribuera en funktionsapp på hello förbrukning plan

Du kan köra en funktionsapp i två olika lägen: hello förbrukning planerings- och hello App Service-plan. hello förbrukning plan tilldelar automatiskt datorkraft när koden körs skalas ut som behövs toohandle belastning och skalas när koden inte körs. Därför du inte toopay för inaktiv virtuella datorer och du har inte tooreserve kapacitet i förväg. toolearn mer information om värd planer, se [Azure Functions användnings- och App Service-planer](functions-scale.md).

En exempelmall av Azure Resource Manager finns [funktionsapp på förbrukning plan].

### <a name="create-a-consumption-plan"></a>Skapa en plan för förbrukning

En plan för förbrukning är en särskild typ av resurs ”serverfarm”. Du anger det med hjälp av hello `Dynamic` värde för hello `computeMode` och `sku` egenskaper:

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

### <a name="create-a-function-app"></a>Skapa en funktionsapp

Dessutom kan en plan för förbrukning kräver två ytterligare inställningar i hello platskonfiguration: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` och `WEBSITE_CONTENTSHARE`. Dessa egenskaper konfigurera hello storage-konto och sökvägen där hello Funktionskoden app och konfiguration lagras.

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

## <a name="deploy-a-function-app-on-hello-app-service-plan"></a>Distribuera en funktionsapp på hello App Service-plan

Hello App Service-plan körs funktionen appen på dedikerade virtuella datorer på Basic, Standard och Premium-SKU: er, liknande tooweb appar. Mer information om hur hello programtjänstplan fungerar finns hello [Azure App Service-planer djupgående översikt över](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

En exempelmall av Azure Resource Manager finns [funktionsapp på Azure App Service-plan].

### <a name="create-an-app-service-plan"></a>Skapa en App Service-plan

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

### <a name="create-a-function-app"></a>Skapa en funktionsapp 

När du har valt ett skalningsalternativ, skapa en funktionsapp. hello appen är hello-behållare som innehåller alla funktioner.

En funktionsapp har många underordnade resurser som du kan använda i din distribution, inklusive inställningar för appen och ange alternativ för källa. Du kan också välja tooremove hello **sourcecontrols** underordnade resursen och använda en annan [distributionsalternativet](functions-continuous-deployment.md) i stället.

> [!IMPORTANT]
> toosuccessfully distribuera ditt program med hjälp av Azure Resource Manager, är det viktigt toounderstand hur resurser har distribuerats i Azure. I följande exempel hello, tillämpas på den översta nivån konfigurationer med hjälp av **siteConfig**. Det är viktigt tooset konfigurationerna en överst nivå eftersom de förmedla information toohello funktioner distribution och runtime engine. Översta information krävs innan hello underordnade **sourcecontrols/web** resurs används. Även om det är möjligt tooconfigure de här inställningarna i hello underordnade **config/appSettings** resurs, i vissa fall måste du distribuera appen funktionen *innan* **config/appSettings**  tillämpas. Till exempel när du använder funktioner med [Logikappar](../logic-apps/index.md), dina funktioner är beroende av en annan resurs.

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
> Den här mallen använder hello [projekt](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) app inställningar-värde som anger hello baskatalog i vilka hello funktioner distributionsmotorn (Kudu) söker efter distribuerbara kod. I vår databas våra funktioner finns i en undermapp till hello **src** mapp. Så i föregående exempel hello, vi värdet hello app inställningar för`src`. Om dina funktioner finns i hello roten för din databas, eller om du inte distribuerar från källkontroll, kan du ta bort det här värdet för app-inställningar.

## <a name="deploy-your-template"></a>Distribuera mallen

Du kan använda något av följande sätt toodeploy hello mallen:

* [PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [Azure Portal](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [REST-API](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-tooazure-button"></a>TooAzure knappen distribuera

Ersätt ```<url-encoded-path-to-azuredeploy-json>``` med en [URL-kodade](https://www.bing.com/search?q=url+encode) version av hello rådata sökvägen till din `azuredeploy.json` -filen i GitHub.

Här är ett exempel som använder markdown:

```markdown
[![Deploy tooAzure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

Här är ett exempel som använder HTML:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a>Nästa steg

Läs mer om hur toodevelop och konfigurera Azure Functions.

* [Azure Functions, info för utvecklare](functions-reference.md)
* [Hur fungerar tooconfigure Azure app-inställningar](functions-how-to-use-azure-function-app-settings.md)
* [Skapa din första Azure-funktion](functions-create-first-azure-function.md)

<!-- LINKS -->

[funktionsapp på förbrukning plan]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[funktionsapp på Azure App Service-plan]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json
