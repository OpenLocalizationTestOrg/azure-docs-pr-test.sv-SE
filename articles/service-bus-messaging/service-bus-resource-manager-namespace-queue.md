---
title: "aaaCreate Azure Service Bus-namnrymd och kö med Azure Resource Manager-mall | Microsoft Docs"
description: "Skapa ett Service Bus-namnområde och en kö med Azure Resource Manager-mall"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a6bfb5fd-7b98-4588-8aa1-9d5f91b599b6
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: f230878b7c557bdd80d74da0de5a85ba4ee99ef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a>Skapa ett Service Bus-namnområde och en kö med en Azure Resource Manager-mall

Den här artikeln visar hur toouse en Azure Resource Manager-mall som skapar en Service Bus-namnrymd och en kö inom det här namnområdet. Du får lära dig hur toodefine vilka resurser har distribuerats och hur toodefine parametrar som anges när hello distributionen körs. Du kan använda den här mallen för din egen distribution eller anpassa den toomeet dina krav.

Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar][Authoring Azure Resource Manager templates].

Hello fullständig mallen finns hello [mallen för Service Bus-namnområde och kön] [ Service Bus namespace and queue template] på GitHub.

> [!NOTE]
> hello följande Azure Resource Manager-mallar är tillgängliga för hämtning och distribution.
> 
> * [Skapa ett namnområde för Service Bus med kön och auktorisering regel](service-bus-resource-manager-namespace-auth-rule.md)
> * [Skapa ett namnområde för Service Bus med ämnet och prenumerationen](service-bus-resource-manager-namespace-topic.md)
> * [Skapa ett namnområde för Service Bus](service-bus-resource-manager-namespace.md)
> * [Skapa ett namnområde för Service Bus med ämne, prenumeration och regel](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> toocheck för hello senaste mallar, besök hello [Azure-Snabbstartsmallar] [ Azure Quickstart Templates] galleriet och Sök efter ”Service Bus”.
> 
> 

## <a name="what-will-you-deploy"></a>Vad vill du distribuera?

Med den här mallen distribuerar du en Service Bus-namnrymd med en kö.

[Service Bus-köer](service-bus-queues-topics-subscriptions.md#queues) erbjuder First In, First Out (FIFO) meddelandet leverans tooone eller flera konkurrerande konsumenter.

toorun Hej distributionen automatiskt, klickar du på följande knapp hello:

[![Distribuera tooAzure](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parametrar

Med Azure Resource Manager kan du definiera parametrar för värden som du vill toospecify när hello mallen distribueras. hello mallen innehåller ett avsnitt som heter `Parameters` som innehåller alla hello parametervärden. Du bör definiera en parameter för de värden som varierar baserat på hello-projekt som du distribuerar eller på hello-miljö som du distribuerar till. Definiera inte parametrar för värden som behålls alltid hello samma. Varje parametervärdet används i hello mallen toodefine hello resurser som distribueras.

hello mallen definierar hello följande parametrar.

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

### <a name="servicebusqueuename"></a>serviceBusQueueName
hello namnet på hello kö som skapats i hello Service Bus-namnrymd.

```json
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion
hello Service Bus-API-version av hello mallen.

```json
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-toodeploy"></a>Resurser toodeploy
Skapar en standard Service Bus-namnrymd av typen **Messaging**, med en kö.

```json
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]",
            }
        }]
    }]
```

## <a name="commands-toorun-deployment"></a>Kommandon toorun distribution
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Nästa steg
Nu när du har skapat och distribuerat resurser med Azure Resource Manager, lär du dig hur toomanage dessa resurser genom att visa dessa artiklar:

* [Hantera Service Bus med PowerShell](service-bus-manage-with-ps.md)
* [Hantera Service Bus-resurser med hello Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace and queue template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus queues]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
