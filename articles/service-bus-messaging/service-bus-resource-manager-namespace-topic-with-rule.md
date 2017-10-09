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
# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a><span data-ttu-id="35bdb-103">Skapa ett namnområde för Service Bus med ämne, prenumeration och regel med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="35bdb-103">Create a Service Bus namespace with topic, subscription, and rule using an Azure Resource Manager template</span></span>

<span data-ttu-id="35bdb-104">Den här artikeln visar hur toouse en Azure Resource Manager-mall som skapar en Service Bus-namnrymd med ett ämne, prenumeration och regeln (filter).</span><span class="sxs-lookup"><span data-stu-id="35bdb-104">This article shows how toouse an Azure Resource Manager template that creates a Service Bus namespace with a topic, subscription, and rule (filter).</span></span> <span data-ttu-id="35bdb-105">Du lär dig hur toodefine vilka resurser har distribuerats och hur toodefine parametrar som anges när hello distributionen körs.</span><span class="sxs-lookup"><span data-stu-id="35bdb-105">You learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="35bdb-106">Du kan använda den här mallen för din egen distribution eller anpassa den toomeet dina krav</span><span class="sxs-lookup"><span data-stu-id="35bdb-106">You can use this template for your own deployments, or customize it toomeet your requirements</span></span>

<span data-ttu-id="35bdb-107">Mer information om att skapa mallar finns i [Redigera Azure Resource Manager-mallar][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="35bdb-107">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="35bdb-108">Läs mer om träning och mönster på Azure-resurser namngivningskonventioner, [rekommenderas namnkonventionerna för Azure-resurser][Recommended naming conventions for Azure resources].</span><span class="sxs-lookup"><span data-stu-id="35bdb-108">For more information about practice and patterns on Azure resources naming conventions, see [Recommended naming conventions for Azure resources][Recommended naming conventions for Azure resources].</span></span>

<span data-ttu-id="35bdb-109">Hello fullständig mallen finns hello [Service Bus-namnrymd med ämne, prenumeration och regeln] [ Service Bus namespace with topic, subscription, and rule] mall.</span><span class="sxs-lookup"><span data-stu-id="35bdb-109">For hello complete template, see hello [Service Bus namespace with topic, subscription, and rule][Service Bus namespace with topic, subscription, and rule] template.</span></span>

> [!NOTE]
> <span data-ttu-id="35bdb-110">hello följande Azure Resource Manager-mallar är tillgängliga för hämtning och distribution.</span><span class="sxs-lookup"><span data-stu-id="35bdb-110">hello following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="35bdb-111">Skapa ett namnområde för Service Bus med kön och auktorisering regel</span><span class="sxs-lookup"><span data-stu-id="35bdb-111">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="35bdb-112">Skapa ett namnområde för Service Bus med kön</span><span class="sxs-lookup"><span data-stu-id="35bdb-112">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="35bdb-113">Skapa ett namnområde för Service Bus</span><span class="sxs-lookup"><span data-stu-id="35bdb-113">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="35bdb-114">Skapa ett namnområde för Service Bus med ämnet och prenumerationen</span><span class="sxs-lookup"><span data-stu-id="35bdb-114">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> 
> <span data-ttu-id="35bdb-115">toocheck för hello senaste mallar, besök hello [Azure-Snabbstartsmallar] [ Azure Quickstart Templates] galleriet och Sök efter Service Bus.</span><span class="sxs-lookup"><span data-stu-id="35bdb-115">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Service Bus.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="35bdb-116">Vad vill du distribuera?</span><span class="sxs-lookup"><span data-stu-id="35bdb-116">What will you deploy?</span></span>

<span data-ttu-id="35bdb-117">Med den här mallen kan du distribuera en Service Bus-namnrymd med ämne, prenumeration och regeln (filter).</span><span class="sxs-lookup"><span data-stu-id="35bdb-117">With this template, you deploy a Service Bus namespace with topic, subscription, and rule (filter).</span></span>

<span data-ttu-id="35bdb-118">[Service Bus-ämnen och prenumerationer](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) tillhandahålla en en-till-många-kommunikation i en *Publicera/prenumerera* mönster.</span><span class="sxs-lookup"><span data-stu-id="35bdb-118">[Service Bus topics and subscriptions](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) provide a one-to-many form of communication, in a *publish/subscribe* pattern.</span></span> <span data-ttu-id="35bdb-119">När du använder ämnen och prenumerationer, kommunicerar inte komponenterna i ett distribuerat program direkt med varandra, utbyter de istället meddelanden via avsnittet som fungerar som en mellanhand. Ett prenumeration tooa ämne liknar en virtuell kö som tar emot kopior av meddelanden som skickades toohello avsnittet.</span><span class="sxs-lookup"><span data-stu-id="35bdb-119">When using topics and subscriptions, components of a distributed application do not communicate directly with each other, instead they exchange messages via topic that acts as an intermediary.A subscription tooa topic resembles a virtual queue that receives copies of messages that were sent toohello topic.</span></span> <span data-ttu-id="35bdb-120">Ett filter för prenumerationen kan du toospecify vilka meddelanden som skickas tooa avsnittet ska visas inom en viss ämnesprenumeration.</span><span class="sxs-lookup"><span data-stu-id="35bdb-120">A filter on subscription enables you toospecify which messages sent tooa topic should appear within a specific topic subscription.</span></span>

## <a name="what-are-rules-filters"></a><span data-ttu-id="35bdb-121">Vad är regler (filter)?</span><span class="sxs-lookup"><span data-stu-id="35bdb-121">What are rules (filters)?</span></span>

<span data-ttu-id="35bdb-122">I många fall är måste meddelanden som har specifika egenskaper bearbetas på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="35bdb-122">In many scenarios, messages that have specific characteristics must be processed in different ways.</span></span> <span data-ttu-id="35bdb-123">tooenable, kan du konfigurera prenumerationer toofind meddelanden som har specifika egenskaper och sedan utföra ändringar toothose egenskaper.</span><span class="sxs-lookup"><span data-stu-id="35bdb-123">tooenable this, you can configure subscriptions toofind messages that have specific properties and then perform modifications toothose properties.</span></span> <span data-ttu-id="35bdb-124">Service Bus-prenumerationer finns alla meddelanden som skickas toohello avsnittet, men du kan bara kopiera en delmängd av dessa meddelanden toohello virtuella prenumerationskön.</span><span class="sxs-lookup"><span data-stu-id="35bdb-124">Although Service Bus subscriptions see all messages sent toohello topic, you can only copy a subset of those messages toohello virtual subscription queue.</span></span> <span data-ttu-id="35bdb-125">Detta åstadkoms med hjälp av prenumerationsfilter.</span><span class="sxs-lookup"><span data-stu-id="35bdb-125">This is accomplished using subscription filters.</span></span> <span data-ttu-id="35bdb-126">toolearn mer information om regler (filter), se [regler och åtgärder](service-bus-queues-topics-subscriptions.md#rules-and-actions).</span><span class="sxs-lookup"><span data-stu-id="35bdb-126">toolearn more about rules (filters), see [Rules and actions](service-bus-queues-topics-subscriptions.md#rules-and-actions).</span></span>

<span data-ttu-id="35bdb-127">toorun Hej distributionen automatiskt, klickar du på följande knapp hello:</span><span class="sxs-lookup"><span data-stu-id="35bdb-127">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="35bdb-128">[![Distribuera tooAzure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="35bdb-128">[![Deploy tooAzure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="35bdb-129">Parametrar</span><span class="sxs-lookup"><span data-stu-id="35bdb-129">Parameters</span></span>

<span data-ttu-id="35bdb-130">Med Azure Resource Manager bör du definiera parametrar för värden som du vill toospecify när hello mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="35bdb-130">With Azure Resource Manager, you should define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="35bdb-131">hello mallen innehåller ett avsnitt som heter `Parameters` som innehåller alla hello parametervärden.</span><span class="sxs-lookup"><span data-stu-id="35bdb-131">hello template includes a section called `Parameters` that contains all hello parameter values.</span></span> <span data-ttu-id="35bdb-132">Du bör definiera en parameter för de värden som varierar baserat på hello-projekt som du distribuerar eller på hello-miljö som du distribuerar till.</span><span class="sxs-lookup"><span data-stu-id="35bdb-132">You should define a parameter for those values that vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="35bdb-133">Definiera inte parametrar för värden som alltid hello samma.</span><span class="sxs-lookup"><span data-stu-id="35bdb-133">Do not define parameters for values that always stay hello same.</span></span> <span data-ttu-id="35bdb-134">Varje parametervärdet används i hello mallen toodefine hello resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="35bdb-134">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="35bdb-135">hello mallen definierar hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="35bdb-135">hello template defines hello following parameters:</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="35bdb-136">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="35bdb-136">serviceBusNamespaceName</span></span>
<span data-ttu-id="35bdb-137">hello namnet på hello Service Bus-namnområde toocreate.</span><span class="sxs-lookup"><span data-stu-id="35bdb-137">hello name of hello Service Bus namespace toocreate.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a><span data-ttu-id="35bdb-138">serviceBusTopicName</span><span class="sxs-lookup"><span data-stu-id="35bdb-138">serviceBusTopicName</span></span>
<span data-ttu-id="35bdb-139">hello namnet på hello ämne som skapats i hello Service Bus-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="35bdb-139">hello name of hello topic created in hello Service Bus namespace.</span></span>

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a><span data-ttu-id="35bdb-140">serviceBusSubscriptionName</span><span class="sxs-lookup"><span data-stu-id="35bdb-140">serviceBusSubscriptionName</span></span>
<span data-ttu-id="35bdb-141">hello namnet på hello-prenumeration som skapats i hello Service Bus-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="35bdb-141">hello name of hello subscription created in hello Service Bus namespace.</span></span>

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```
### <a name="servicebusrulename"></a><span data-ttu-id="35bdb-142">serviceBusRuleName</span><span class="sxs-lookup"><span data-stu-id="35bdb-142">serviceBusRuleName</span></span>
<span data-ttu-id="35bdb-143">hello namnet på hello rule(filter) som skapats i hello Service Bus-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="35bdb-143">hello name of hello rule(filter) created in hello Service Bus namespace.</span></span>

```json
   "serviceBusRuleName": {
   "type": "string",
  }
```
### <a name="servicebusapiversion"></a><span data-ttu-id="35bdb-144">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="35bdb-144">serviceBusApiVersion</span></span>
<span data-ttu-id="35bdb-145">hello Service Bus-API-version av hello mallen.</span><span class="sxs-lookup"><span data-stu-id="35bdb-145">hello Service Bus API version of hello template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-toodeploy"></a><span data-ttu-id="35bdb-146">Resurser toodeploy</span><span class="sxs-lookup"><span data-stu-id="35bdb-146">Resources toodeploy</span></span>
<span data-ttu-id="35bdb-147">Skapar en standard Service Bus-namnrymd av typen **Messaging**, med ämnet och prenumerationen och regler.</span><span class="sxs-lookup"><span data-stu-id="35bdb-147">Creates a standard Service Bus namespace of type **Messaging**, with topic and subscription and rules.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="35bdb-148">Kommandon toorun distribution</span><span class="sxs-lookup"><span data-stu-id="35bdb-148">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="35bdb-149">PowerShell</span><span class="sxs-lookup"><span data-stu-id="35bdb-149">PowerShell</span></span>
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="35bdb-150">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="35bdb-150">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="35bdb-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="35bdb-151">Next steps</span></span>
<span data-ttu-id="35bdb-152">Nu när du har skapat och distribuerat resurser med Azure Resource Manager, lär du dig hur toomanage dessa resurser genom att visa dessa artiklar:</span><span class="sxs-lookup"><span data-stu-id="35bdb-152">Now that you've created and deployed resources using Azure Resource Manager, learn how toomanage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="35bdb-153">Hantera Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="35bdb-153">Manage Azure Service Bus</span></span>](service-bus-management-libraries.md)
* [<span data-ttu-id="35bdb-154">Hantera Service Bus med PowerShell</span><span class="sxs-lookup"><span data-stu-id="35bdb-154">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="35bdb-155">Hantera Service Bus-resurser med hello Service Bus Explorer</span><span class="sxs-lookup"><span data-stu-id="35bdb-155">Manage Service Bus resources with hello Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Recommended naming conventions for Azure resources]: ../guidance/guidance-naming-conventions.md
[Service Bus namespace with topic, subscription, and rule]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md

