---
title: "Skapa prenumeration för Azure Service Bus-avsnittet och regeln med hjälp av Azure Resource Manager-mall | Microsoft Docs"
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
ms.openlocfilehash: 35e67d86b42358c4ce28b41beae1ee8e1896e939
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a><span data-ttu-id="ca3d9-103">Skapa ett namnområde för Service Bus med ämne, prenumeration och regel med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="ca3d9-103">Create a Service Bus namespace with topic, subscription, and rule using an Azure Resource Manager template</span></span>

<span data-ttu-id="ca3d9-104">Den här artikeln visar hur du använder en Azure Resource Manager-mall som skapar en Service Bus-namnrymd med ett ämne, prenumeration och regeln (filter).</span><span class="sxs-lookup"><span data-stu-id="ca3d9-104">This article shows how to use an Azure Resource Manager template that creates a Service Bus namespace with a topic, subscription, and rule (filter).</span></span> <span data-ttu-id="ca3d9-105">Du lär dig hur du definierar vilka resurser har distribuerats och hur du definierar parametrar som anges när distributionen körs.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-105">You learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="ca3d9-106">Du kan använda den här mallen för dina egna distributioner eller anpassa den så att den uppfyller dina krav</span><span class="sxs-lookup"><span data-stu-id="ca3d9-106">You can use this template for your own deployments, or customize it to meet your requirements</span></span>

<span data-ttu-id="ca3d9-107">Mer information om att skapa mallar finns i [Redigera Azure Resource Manager-mallar][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="ca3d9-107">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="ca3d9-108">Läs mer om träning och mönster på Azure-resurser namngivningskonventioner, [rekommenderas namnkonventionerna för Azure-resurser][Recommended naming conventions for Azure resources].</span><span class="sxs-lookup"><span data-stu-id="ca3d9-108">For more information about practice and patterns on Azure resources naming conventions, see [Recommended naming conventions for Azure resources][Recommended naming conventions for Azure resources].</span></span>

<span data-ttu-id="ca3d9-109">Den fullständiga mallen finns i [Service Bus-namnrymd med ämne, prenumeration och regeln] [ Service Bus namespace with topic, subscription, and rule] mall.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-109">For the complete template, see the [Service Bus namespace with topic, subscription, and rule][Service Bus namespace with topic, subscription, and rule] template.</span></span>

> [!NOTE]
> <span data-ttu-id="ca3d9-110">Följande Azure Resource Manager-mallar är tillgängliga för hämtning och distribution.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-110">The following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="ca3d9-111">Skapa ett namnområde för Service Bus med kön och auktorisering regel</span><span class="sxs-lookup"><span data-stu-id="ca3d9-111">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="ca3d9-112">Skapa ett namnområde för Service Bus med kön</span><span class="sxs-lookup"><span data-stu-id="ca3d9-112">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="ca3d9-113">Skapa ett namnområde för Service Bus</span><span class="sxs-lookup"><span data-stu-id="ca3d9-113">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="ca3d9-114">Skapa ett namnområde för Service Bus med ämnet och prenumerationen</span><span class="sxs-lookup"><span data-stu-id="ca3d9-114">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> 
> <span data-ttu-id="ca3d9-115">Om du vill söka efter de senaste mallarna finns i [Azure-Snabbstartsmallar] [ Azure Quickstart Templates] galleriet och Sök efter Service Bus.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-115">To check for the latest templates, visit the [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Service Bus.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="ca3d9-116">Vad vill du distribuera?</span><span class="sxs-lookup"><span data-stu-id="ca3d9-116">What will you deploy?</span></span>

<span data-ttu-id="ca3d9-117">Med den här mallen kan du distribuera en Service Bus-namnrymd med ämne, prenumeration och regeln (filter).</span><span class="sxs-lookup"><span data-stu-id="ca3d9-117">With this template, you deploy a Service Bus namespace with topic, subscription, and rule (filter).</span></span>

<span data-ttu-id="ca3d9-118">[Service Bus-ämnen och prenumerationer](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) tillhandahålla en en-till-många-kommunikation i en *Publicera/prenumerera* mönster.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-118">[Service Bus topics and subscriptions](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) provide a one-to-many form of communication, in a *publish/subscribe* pattern.</span></span> <span data-ttu-id="ca3d9-119">När du använder ämnen och prenumerationer, kommunicerar inte komponenterna i ett distribuerat program direkt med varandra, utbyter de istället meddelanden via avsnittet som fungerar som en mellanhand. En prenumeration på ett ämne liknar en virtuell kö som tar emot kopior av meddelanden som har skickats till ämnet.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-119">When using topics and subscriptions, components of a distributed application do not communicate directly with each other, instead they exchange messages via topic that acts as an intermediary.A subscription to a topic resembles a virtual queue that receives copies of messages that were sent to the topic.</span></span> <span data-ttu-id="ca3d9-120">Ett filter på prenumerationen kan du ange vilka meddelanden som skickas till ett ämne ska visas inom en viss ämnesprenumeration.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-120">A filter on subscription enables you to specify which messages sent to a topic should appear within a specific topic subscription.</span></span>

## <a name="what-are-rules-filters"></a><span data-ttu-id="ca3d9-121">Vad är regler (filter)?</span><span class="sxs-lookup"><span data-stu-id="ca3d9-121">What are rules (filters)?</span></span>

<span data-ttu-id="ca3d9-122">I många fall är måste meddelanden som har specifika egenskaper bearbetas på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-122">In many scenarios, messages that have specific characteristics must be processed in different ways.</span></span> <span data-ttu-id="ca3d9-123">För att möjliggöra detta, kan du konfigurera prenumerationer för att söka efter meddelanden som har specifika egenskaper och sedan utföra ändringar i dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-123">To enable this, you can configure subscriptions to find messages that have specific properties and then perform modifications to those properties.</span></span> <span data-ttu-id="ca3d9-124">Du kan bara kopiera en delmängd av dessa meddelanden till prenumerationskön virtuella även om Service Bus prenumerationer visas alla meddelanden som skickas till ämnet.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-124">Although Service Bus subscriptions see all messages sent to the topic, you can only copy a subset of those messages to the virtual subscription queue.</span></span> <span data-ttu-id="ca3d9-125">Detta åstadkoms med hjälp av prenumerationsfilter.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-125">This is accomplished using subscription filters.</span></span> <span data-ttu-id="ca3d9-126">Mer information om regler (filter) finns [regler och åtgärder](service-bus-queues-topics-subscriptions.md#rules-and-actions).</span><span class="sxs-lookup"><span data-stu-id="ca3d9-126">To learn more about rules (filters), see [Rules and actions](service-bus-queues-topics-subscriptions.md#rules-and-actions).</span></span>

<span data-ttu-id="ca3d9-127">Klicka på följande knapp för att köra distributionen automatiskt:</span><span class="sxs-lookup"><span data-stu-id="ca3d9-127">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="ca3d9-128">[![Distribuera till Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="ca3d9-128">[![Deploy to Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="ca3d9-129">Parametrar</span><span class="sxs-lookup"><span data-stu-id="ca3d9-129">Parameters</span></span>

<span data-ttu-id="ca3d9-130">Med Azure Resource Manager bör du definiera parametrar för värden som du vill ange när mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-130">With Azure Resource Manager, you should define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="ca3d9-131">Mallen innehåller ett avsnitt som heter `Parameters` och som innehåller alla parametervärden.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-131">The template includes a section called `Parameters` that contains all the parameter values.</span></span> <span data-ttu-id="ca3d9-132">Du bör definiera en parameter för de värden som varierar utifrån det projekt som du distribuerar eller utifrån den miljö som du distribuerar till.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-132">You should define a parameter for those values that vary based on the project you are deploying or based on the environment you are deploying to.</span></span> <span data-ttu-id="ca3d9-133">Definiera inte parametrar för värden som aldrig ändras.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-133">Do not define parameters for values that always stay the same.</span></span> <span data-ttu-id="ca3d9-134">Varje parametervärde används i mallen för att definiera de resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-134">Each parameter value is used in the template to define the resources that are deployed.</span></span>

<span data-ttu-id="ca3d9-135">Mallen definierar följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="ca3d9-135">The template defines the following parameters:</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="ca3d9-136">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="ca3d9-136">serviceBusNamespaceName</span></span>
<span data-ttu-id="ca3d9-137">Namnet på namnområdet för Service Bus för att skapa.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-137">The name of the Service Bus namespace to create.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a><span data-ttu-id="ca3d9-138">serviceBusTopicName</span><span class="sxs-lookup"><span data-stu-id="ca3d9-138">serviceBusTopicName</span></span>
<span data-ttu-id="ca3d9-139">Namnet på det ämne som skapats i Service Bus-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-139">The name of the topic created in the Service Bus namespace.</span></span>

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a><span data-ttu-id="ca3d9-140">serviceBusSubscriptionName</span><span class="sxs-lookup"><span data-stu-id="ca3d9-140">serviceBusSubscriptionName</span></span>
<span data-ttu-id="ca3d9-141">Namnet på den prenumeration som skapats i Service Bus-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-141">The name of the subscription created in the Service Bus namespace.</span></span>

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```
### <a name="servicebusrulename"></a><span data-ttu-id="ca3d9-142">serviceBusRuleName</span><span class="sxs-lookup"><span data-stu-id="ca3d9-142">serviceBusRuleName</span></span>
<span data-ttu-id="ca3d9-143">Namnet på rule(filter) som skapats i Service Bus-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-143">The name of the rule(filter) created in the Service Bus namespace.</span></span>

```json
   "serviceBusRuleName": {
   "type": "string",
  }
```
### <a name="servicebusapiversion"></a><span data-ttu-id="ca3d9-144">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="ca3d9-144">serviceBusApiVersion</span></span>
<span data-ttu-id="ca3d9-145">Service Bus-API-versionen av mallen.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-145">The Service Bus API version of the template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-to-deploy"></a><span data-ttu-id="ca3d9-146">Resurser som ska distribueras</span><span class="sxs-lookup"><span data-stu-id="ca3d9-146">Resources to deploy</span></span>
<span data-ttu-id="ca3d9-147">Skapar en standard Service Bus-namnrymd av typen **Messaging**, med ämnet och prenumerationen och regler.</span><span class="sxs-lookup"><span data-stu-id="ca3d9-147">Creates a standard Service Bus namespace of type **Messaging**, with topic and subscription and rules.</span></span>

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

## <a name="commands-to-run-deployment"></a><span data-ttu-id="ca3d9-148">Kommandon för att köra distributionen</span><span class="sxs-lookup"><span data-stu-id="ca3d9-148">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="ca3d9-149">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ca3d9-149">PowerShell</span></span>
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="ca3d9-150">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ca3d9-150">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="ca3d9-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ca3d9-151">Next steps</span></span>
<span data-ttu-id="ca3d9-152">Nu när du har skapat och distribuerat resurser med Azure Resource Manager, lär du dig hur du hanterar dessa resurser genom att visa dessa artiklar:</span><span class="sxs-lookup"><span data-stu-id="ca3d9-152">Now that you've created and deployed resources using Azure Resource Manager, learn how to manage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="ca3d9-153">Hantera Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="ca3d9-153">Manage Azure Service Bus</span></span>](service-bus-management-libraries.md)
* [<span data-ttu-id="ca3d9-154">Hantera Service Bus med PowerShell</span><span class="sxs-lookup"><span data-stu-id="ca3d9-154">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="ca3d9-155">Hantera Service Bus-resurser med Service Bus Explorer</span><span class="sxs-lookup"><span data-stu-id="ca3d9-155">Manage Service Bus resources with the Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Recommended naming conventions for Azure resources]: ../guidance/guidance-naming-conventions.md
[Service Bus namespace with topic, subscription, and rule]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md

