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
# <a name="create-a-redis-cache-using-a-template"></a>Skapa en Redis Cache med hjälp av en mall
I det här avsnittet lär du dig hur toocreate en Azure Resource Manager-mall som distribuerar ett Azure Redis-Cache. hello cache kan användas med en befintlig lagring konto tookeep diagnostikdata. Du också lära dig hur toodefine vilka resurser har distribuerats och hur toodefine parametrar som anges när hello distributionen körs. Du kan använda den här mallen för din egen distribution eller anpassa den toomeet dina krav.

För närvarande diagnostikinställningar delas för alla cacheminnen i hello samma region för en prenumeration. Uppdatera en cacheminnet i hello region påverkar alla cacheminnen i hello region.

Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).

Hello fullständig mall, se [Redis-Cache mallen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

> [!NOTE]
> Resource Manager-mallar för nya hello [premiumnivån](cache-premium-tier-intro.md) är tillgängliga. 
> 
> * [Skapa en Premium Redis-Cache med kluster](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
> * [Skapa Premium Redis-Cache med-datapersistence](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
> * [Skapa Premium Redis-Cache med virtuella nätverk och valfritt kluster](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
> 
> toocheck för hello senaste mallar, se [Azure-Snabbstartsmallar](https://azure.microsoft.com/documentation/templates/) och Sök efter `Redis Cache`.
> 
> 

## <a name="what-you-will-deploy"></a>Vad du ska distribuera
I den här mallen ska du distribuera ett Azure Redis-Cache som använder ett befintligt lagringskonto för diagnostikdata.

toorun Hej distributionen automatiskt, klickar du på följande knapp hello:

[![Distribuera tooAzure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parametrar
Med Azure Resource Manager kan du definiera parametrar för värden som du vill toospecify när hello mallen distribueras. hello mallen innehåller ett avsnitt som heter parametrar som innehåller alla hello parametervärden.
Du bör definiera en parameter för de värden som varierar baserat på hello-projekt som du distribuerar eller på hello-miljö som du distribuerar till. Definiera inte parametrar för värden som alltid hello samma. Varje parametervärdet används i hello mallen toodefine hello resurser som distribueras. 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation
hello plats hello Redis-Cache. För bästa prestanda hello används samma plats som hello app toobe används med hello cache.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName
hello namnet på hello befintliga lagring konto toouse för diagnostik. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort
Ett booleskt värde som anger om tooallow åtkomst via SSL-portar.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus
Ett värde som anger om diagnostik är aktiverad. Använd ON eller OFF.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## <a name="resources-toodeploy"></a>Resurser toodeploy
### <a name="redis-cache"></a>Redis Cache
Skapar hello Azure Redis-Cache.

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



## <a name="commands-toorun-deployment"></a>Kommandon toorun distribution
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache

### <a name="azure-cli"></a>Azure CLI
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


