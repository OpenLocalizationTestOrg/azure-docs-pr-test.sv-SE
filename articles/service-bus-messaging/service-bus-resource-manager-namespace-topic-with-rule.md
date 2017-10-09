---
title: "aaaCreate Azure Service Bus avsnittet prenumeration och regeln med hjälp av Azure Resource Manager-mall | Microsoft Docs"
description: "Skapa ett namnområde för Service Bus med ämne, prenumeration och regeln med hjälp av Azure Resource Manager-mall"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 9e0aaf58-0214-4bca-bd00-d29c08f9b1bc
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: dbc46da8491aee4d0c73bd4db90c696008920df4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Skapa ett namnområde för Service Bus med ämne, prenumeration och regel med en Azure Resource Manager-mall

Den här artikeln visar hur toouse en Azure Resource Manager-mall som skapar en Service Bus-namnrymd med ett ämne, prenumeration och regeln (filter). Du lär dig hur toodefine vilka resurser har distribuerats och hur toodefine parametrar som anges när hello distributionen körs. Du kan använda den här mallen för din egen distribution eller anpassa den toomeet dina krav

Mer information om att skapa mallar finns i [Redigera Azure Resource Manager-mallar][Authoring Azure Resource Manager templates].

Läs mer om träning och mönster på Azure-resurser namngivningskonventioner, [rekommenderas namnkonventionerna för Azure-resurser][Recommended naming conventions for Azure resources].

Hello fullständig mallen finns hello [Service Bus-namnrymd med ämne, prenumeration och regeln] [ Service Bus namespace with topic, subscription, and rule] mall.

> [!NOTE]
> hello följande Azure Resource Manager-mallar är tillgängliga för hämtning och distribution.
> 
> * [Skapa ett namnområde för Service Bus med kön och auktorisering regel](service-bus-resource-manager-namespace-auth-rule.md)
> * [Skapa ett namnområde för Service Bus med kön](service-bus-resource-manager-namespace-queue.md)
> * [Skapa ett namnområde för Service Bus](service-bus-resource-manager-namespace.md)
> * [Skapa ett namnområde för Service Bus med ämnet och prenumerationen](service-bus-resource-manager-namespace-topic.md)
> 
> toocheck för hello senaste mallar, besök hello [Azure-Snabbstartsmallar] [ Azure Quickstart Templates] galleriet och Sök efter Service Bus.
> 
> 

## <a name="what-will-you-deploy"></a>Vad vill du distribuera?

Med den här mallen kan du distribuera en Service Bus-namnrymd med ämne, prenumeration och regeln (filter).

[Service Bus-ämnen och prenumerationer](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) tillhandahålla en en-till-många-kommunikation i en *Publicera/prenumerera* mönster. När du använder ämnen och prenumerationer, kommunicerar inte komponenterna i ett distribuerat program direkt med varandra, utbyter de istället meddelanden via avsnittet som fungerar som en mellanhand. Ett prenumeration tooa ämne liknar en virtuell kö som tar emot kopior av meddelanden som skickades toohello avsnittet. Ett filter för prenumerationen kan du toospecify vilka meddelanden som skickas tooa avsnittet ska visas inom en viss ämnesprenumeration.

## <a name="what-are-rules-filters"></a>Vad är regler (filter)?

I många fall är måste meddelanden som har specifika egenskaper bearbetas på olika sätt. tooenable, kan du konfigurera prenumerationer toofind meddelanden som har specifika egenskaper och sedan utföra ändringar toothose egenskaper. Service Bus-prenumerationer finns alla meddelanden som skickas toohello avsnittet, men du kan bara kopiera en delmängd av dessa meddelanden toohello virtuella prenumerationskön. Detta åstadkoms med hjälp av prenumerationsfilter. toolearn mer information om regler (filter), se [regler och åtgärder](service-bus-queues-topics-subscriptions.md#rules-and-actions).

toorun Hej distributionen automatiskt, klickar du på följande knapp hello:

[![Distribuera tooAzure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Parametrar

Med Azure Resource Manager bör du definiera parametrar för värden som du vill toospecify när hello mallen distribueras. hello mallen innehåller ett avsnitt som heter `Parameters` som innehåller alla hello parametervärden. Du bör definiera en parameter för de värden som varierar baserat på hello-projekt som du distribuerar eller på hello-miljö som du distribuerar till. Definiera inte parametrar för värden som alltid hello samma. Varje parametervärdet används i hello mallen toodefine hello resurser som distribueras.

hello mallen definierar hello följande parametrar:

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
hello namnet på hello Service Bus-namnområde toocreate.

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName
hello namnet på hello ämne som skapats i hello Service Bus-namnrymd.

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName
hello namnet på hello-prenumeration som skapats i hello Service Bus-namnrymd.

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```
### <a name="servicebusrulename"></a>serviceBusRuleName
hello namnet på hello rule(filter) som skapats i hello Service Bus-namnrymd.

```json
   "serviceBusRuleName": {
   "type": "string",
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
Skapar en standard Service Bus-namnrymd av typen **Messaging**, med ämnet och prenumerationen och regler.

```json
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filter": {
                            "sqlExpression": "StoreName = 'Store1'"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

## <a name="commands-toorun-deployment"></a>Kommandon toorun distribution
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Nästa steg
Nu när du har skapat och distribuerat resurser med Azure Resource Manager, lär du dig hur toomanage dessa resurser genom att visa dessa artiklar:

* [Hantera Azure Service Bus](service-bus-management-libraries.md)
* [Hantera Service Bus med PowerShell](service-bus-manage-with-ps.md)
* [Hantera Service Bus-resurser med hello Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Recommended naming conventions for Azure resources]: ../guidance/guidance-naming-conventions.md
[Service Bus namespace with topic, subscription, and rule]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md

