---
title: "aaaCreate Azure Service Bus-resurser med hjälp av Azure Resource Manager-mallar | Microsoft Docs"
description: "Använd Azure Resource Manager mallar tooautomate hello skapandet av Service Bus-resurser"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 24f6a207-0fa4-49cf-8a58-963f9e2fd655
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: e539902cae307b63ae7c332580e2064761331ec5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Skapa Service Bus-resurser med hjälp av Azure Resource Manager-mallar

Den här artikeln beskriver hur toocreate och distribuera Service Bus-resurser med Azure Resource Manager-mallar, PowerShell och hello Service Bus-resursprovidern.

Azure Resource Manager-mallar hjälpa dig att definiera hello resurser toodeploy för en lösning och toospecify parametrar och variabler som gör att du tooinput värden för olika miljöer. hello mallen består av JSON och uttryck som du kan använda tooconstruct värden för din distribution. Detaljerad information om hur du skriver Azure Resource Manager-mallar och en beskrivning av hello mallformat finns [struktur och syntaxen för Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).

> [!NOTE]
> Hej exemplen i den här artikeln visar hur toouse Azure Resource Manager toocreate en Service Bus-namnrymd och meddelanden entitet (kön). Andra mall-exempel finns hello [Azure Quickstart mallgalleriet] [ Azure Quickstart Templates gallery] och Sök efter ”Service Bus”.
>
>

## <a name="service-bus-resource-manager-templates"></a>Service Bus Resource Manager-mallar

Dessa Service Bus Azure Resource Manager-mallar är tillgängliga för hämtning och distribution. Klicka på följande länkar för information om var och en, med länkar toohello mallar på GitHub hello:

* [Skapa ett namnområde för Service Bus](service-bus-resource-manager-namespace.md)
* [Skapa ett namnområde för Service Bus med kön](service-bus-resource-manager-namespace-queue.md)
* [Skapa ett namnområde för Service Bus med ämnet och prenumerationen](service-bus-resource-manager-namespace-topic.md)
* [Skapa ett namnområde för Service Bus med kön och auktorisering regel](service-bus-resource-manager-namespace-auth-rule.md)
* [Skapa ett namnområde för Service Bus med ämne, prenumeration och regel](service-bus-resource-manager-namespace-topic-with-rule.md)

## <a name="deploy-with-powershell"></a>Distribuera med PowerShell

hello nedan beskrivs hur toouse PowerShell toodeploy en Azure Resource Manager-mall som skapar en **Standard** tjänstnivån Service Bus-namnrymd och en kö inom det här namnområdet. Det här exemplet är baserad på hello [skapa ett namnområde för Service Bus med kön](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) mall. hello ungefärliga arbetsflödet är följande:

1. Installera PowerShell.
2. Skapa hello mall och (valfritt) en parameterfil.
3. Logga in tooyour Azure-konto i PowerShell.
4. Skapa en ny resursgrupp om det inte finns.
5. Testa hello-distribution.
6. Om du vill ange hello distribution läge.
7. Distribuera hello-mallen.

Fullständig information om hur du distribuerar Azure Resource Manager-mallar finns [distribuera resurser med Azure Resource Manager-mallar][Deploy resources with Azure Resource Manager templates].

### <a name="install-powershell"></a>Installera PowerShell

Installera Azure PowerShell genom att följa instruktionerna hello i [komma igång med Azure PowerShell](/powershell/azure/get-started-azureps).

### <a name="create-a-template"></a>Skapa en mall

Klona eller kopiera hello [201-servicebus-skapa-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) mall från GitHub:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of hello Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of hello Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by hello template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
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
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a>Skapa en fil med parametrar (valfritt)

toouse valfria parameterfilen kopiera hello [201-servicebus-skapa-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) fil. Ersätt hello värdet för `serviceBusNamespaceName` du vill toocreate i den här distributionen med namnet hello hello Service Bus-namnrymd och ersätter hello värdet för `serviceBusQueueName` hello namnet på hello kö du vill använda toocreate.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

Mer information finns i hello [parametrar](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) avsnittet.

### <a name="log-in-tooazure-and-set-hello-azure-subscription"></a>Logga in tooAzure och ange hello Azure-prenumeration

Kör hello följande kommando från en PowerShell-kommandotolk:

```powershell
Login-AzureRmAccount
```

Du kan ange toolog på tooyour Azure-konto. Efter inloggningen kör du följande kommando tooview hello tillgängliga prenumerationer.

```powershell
Get-AzureRMSubscription
```

Det här kommandot returnerar en lista över tillgängliga Azure-prenumerationer. Välj en prenumeration för hello aktuell session genom att köra följande kommando hello. Ersätt `<YourSubscriptionId>` med hello GUID för hello Azure-prenumeration som du vill toouse.

```powershell
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-hello-resource-group"></a>Ange hello resursgruppen.

Om du inte har en befintlig resurs, skapa en ny resursgrupp med hello ** New-AzureRmResourceGroup ** kommando. Ange hello namnet för hello resursgrupp och plats som du vill toouse. Exempel:

```powershell
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Om detta lyckas visas en sammanfattning av hello ny resursgrupp.

```powershell
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-hello-deployment"></a>Testa hello-distribution

Verifiera distributionen genom att köra hello `Test-AzureRmResourceGroupDeployment` cmdlet. När du testar hello distribution, ange parametrar exakt samma sätt som när du kör hello-distribution.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

### <a name="create-hello-deployment"></a>Skapa hello-distribution

toocreate hello ny distribution, kör hello `New-AzureRmResourceGroupDeployment` cmdlet, och ange hello nödvändiga parametrar när du tillfrågas. parametrarna för hello innehålla ett namn för din distribution, hello namnet på resursgruppen och hello sökväg eller URL toohello mallfilen. Om hello **läge** parametern anges hello standardvärdet **stegvis** används. Mer information finns i [inkrementell och fullständig distributioner](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).

Hej följande kommandotolkar du för hello tre parametrar i hello PowerShell-fönstret:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

toospecify en fil med parametrar i stället använda hello följande kommando.

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json -TemplateParameterFile <path tooparameters file>\azuredeploy.parameters.json
```

Du kan också använda infogade parametrar när du kör hello distributions-cmdlet. hello-kommandot är följande:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json -parameterName "parameterValue"
```

toorun en [fullständig](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) distribution, ange hello **läge** parameter för**Slutför**:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

### <a name="verify-hello-deployment"></a>Kontrollera distributionen av hello
Om hello resurser har distribuerats visas en sammanfattning av hello distribution i hello PowerShell-fönstret:

```powershell
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a>Nästa steg
Nu har du sett hello grundläggande arbetsflöde och kommandon för att distribuera en Azure Resource Manager-mall. Mer detaljerad information finns i hello följande länkar:

* [Översikt över Azure Resource Manager][Azure Resource Manager overview]
* [Distribuera resurser med Resource Manager-mallar och Azure PowerShell][Deploy resources with Azure Resource Manager templates]
* [Skapa Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md)

[Azure Resource Manager overview]: ../azure-resource-manager/resource-group-overview.md
[Deploy resources with Azure Resource Manager templates]: ../azure-resource-manager/resource-group-template-deploy.md
[Azure Quickstart Templates gallery]: https://azure.microsoft.com/documentation/templates/?term=service+bus
