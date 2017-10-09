---
title: aaaCreate Service Bus-namnrymd med en Azure Resource Manager-mall | Microsoft Docs
description: "Använd Azure Resource Manager mallen toocreate ett Service Bus-namnområde"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: dc0d6482-6344-4cef-8644-d4573639f5e4
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: fddf370affe761a734991ae9b60c1e5825e54ef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Skapa ett namnområde för Service Bus med en Azure Resource Manager-mall

Den här artikeln beskriver hur toouse en Azure Resource Manager-mall som skapar en Service Bus-namnrymd av typen **Messaging** med en Standard/enkel SKU. hello artikel definierar också hello-parametrar som anges för hello körning av hello-distribution. Du kan använda den här mallen för din egen distribution eller anpassa den toomeet dina krav.

Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar][Authoring Azure Resource Manager templates].

Hello fullständig mallen finns hello [mall för Service Bus-namnområde] [ Service Bus namespace template] på GitHub.

> [!NOTE]
> hello följande Azure Resource Manager-mallar är tillgängliga för hämtning och distribution. 
> 
> * [Skapa ett namnområde för Service Bus med kön](service-bus-resource-manager-namespace-queue.md)
> * [Skapa ett namnområde för Service Bus med ämnet och prenumerationen](service-bus-resource-manager-namespace-topic.md)
> * [Skapa ett namnområde för Service Bus med kön och auktorisering regel](service-bus-resource-manager-namespace-auth-rule.md)
> * [Skapa ett namnområde för Service Bus med ämne, prenumeration och regel](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> toocheck för hello senaste mallar, besök hello [Azure-Snabbstartsmallar] [ Azure Quickstart Templates] galleriet och Sök efter Service Bus.
> 
> 

## <a name="what-will-you-deploy"></a>Vad vill du distribuera?
Med den här mallen du ska distribuera en Service Bus-namnrymd med ett [Basic, Standard och Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU.

toorun Hej distributionen automatiskt, klickar du på följande knapp hello:

[![Distribuera tooAzure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Parametrar
Med Azure Resource Manager kan du definiera parametrar för värden som du vill toospecify när hello mallen distribueras. hello mallen innehåller ett avsnitt som heter `Parameters` som innehåller alla hello parametervärden. Du bör definiera en parameter för de värden som varierar baserat på hello-projekt som du distribuerar eller på hello-miljö som du distribuerar till. Definiera inte parametrar för värden som behålls alltid hello samma. Varje parametervärdet används i hello mallen toodefine hello resurser som distribueras.

Den här mallen definierar hello följande parametrar.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
hello namnet på hello Service Bus-namnområde toocreate.

```json
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of hello Service Bus namespace" 
    }
}
```

### <a name="servicebussku"></a>serviceBusSKU
hello namnet på hello Service Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) toocreate.

```json
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Basic", 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "hello messaging tier for service Bus namespace" 
    } 

```

hello mallen definierar hello värden som tillåts för den här parametern (Basic, Standard och Premium) och tilldelas ett standardvärde (Standard), om inget värde anges.

Läs mer om prissättningen för Service Bus [Service Bus priser och fakturering][Service Bus pricing and billing].

### <a name="servicebusapiversion"></a>serviceBusApiVersion
hello Service Bus-API-version av hello mallen.

```json
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by hello template" 
       } 
```

## <a name="resources-toodeploy"></a>Resurser toodeploy
### <a name="service-bus-namespace"></a>Service Bus-namnrymd
Skapar en standard Service Bus-namnrymd av typen **Messaging**.

```json
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## <a name="commands-toorun-deployment"></a>Kommandon toorun distribution
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Nästa steg
Nu när du har skapat och distribuerat resurser med Azure Resource Manager, lär du dig hur toomanage dessa resurser genom att läsa dessa artiklar:

* [Hantera Service Bus med PowerShell](service-bus-manage-with-ps.md)
* [Hantera Service Bus-resurser med hello Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace template]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Service Bus pricing and billing]: service-bus-pricing-billing.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
