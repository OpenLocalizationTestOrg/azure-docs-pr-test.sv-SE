---
title: "Skapa Azure Service Bus-namnområde avsnittet prenumeration med hjälp av Azure Resource Manager-mall | Microsoft Docs"
description: "Skapa ett namnområde för Service Bus med ämnet och prenumerationen med Azure Resource Manager-mall"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: d3d55200-5c60-4b5f-822d-59974cafff0e
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: 8dd48787e7b788d249085b3110484de1a2c1d265
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a><span data-ttu-id="60366-103">Skapa ett namnområde för Service Bus med ämne och en prenumeration med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="60366-103">Create a Service Bus namespace with topic and subscription using an Azure Resource Manager template</span></span>

<span data-ttu-id="60366-104">Den här artikeln visar hur du använder en Azure Resource Manager-mall som skapar en Service Bus-namnrymd och ett ämne och en prenumeration inom det här namnområdet.</span><span class="sxs-lookup"><span data-stu-id="60366-104">This article shows how to use an Azure Resource Manager template that creates a Service Bus namespace and a topic and subscription within that namespace.</span></span> <span data-ttu-id="60366-105">Du kommer lära dig hur du definierar vilka resurser har distribuerats och hur du definierar parametrar som anges när distributionen körs.</span><span class="sxs-lookup"><span data-stu-id="60366-105">You will learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="60366-106">Du kan använda den här mallen för dina egna distributioner eller anpassa den så att den uppfyller dina krav</span><span class="sxs-lookup"><span data-stu-id="60366-106">You can use this template for your own deployments, or customize it to meet your requirements</span></span>

<span data-ttu-id="60366-107">Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="60366-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="60366-108">Den fullständiga mallen finns i [Service Bus-namnrymd med ämnet och prenumerationen] [ Service Bus namespace with topic and subscription] mall.</span><span class="sxs-lookup"><span data-stu-id="60366-108">For the complete template, see the [Service Bus namespace with topic and subscription][Service Bus namespace with topic and subscription] template.</span></span>

> [!NOTE]
> <span data-ttu-id="60366-109">Följande Azure Resource Manager-mallar är tillgängliga för hämtning och distribution.</span><span class="sxs-lookup"><span data-stu-id="60366-109">The following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="60366-110">Skapa ett namnområde för Service Bus</span><span class="sxs-lookup"><span data-stu-id="60366-110">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="60366-111">Skapa ett namnområde för Service Bus med kön</span><span class="sxs-lookup"><span data-stu-id="60366-111">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="60366-112">Skapa ett namnområde för Service Bus med kön och auktorisering regel</span><span class="sxs-lookup"><span data-stu-id="60366-112">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="60366-113">Skapa ett namnområde för Service Bus med ämne, prenumeration och regel</span><span class="sxs-lookup"><span data-stu-id="60366-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="60366-114">Om du vill söka efter de senaste mallarna finns i [Azure-Snabbstartsmallar] [ Azure Quickstart Templates] galleriet och Sök efter ”Service Bus”.</span><span class="sxs-lookup"><span data-stu-id="60366-114">To check for the latest templates, visit the [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for "Service Bus."</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="60366-115">Vad vill du distribuera?</span><span class="sxs-lookup"><span data-stu-id="60366-115">What will you deploy?</span></span>

<span data-ttu-id="60366-116">Med den här mallen distribuerar du en Service Bus-namnrymd med ämne och en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="60366-116">With this template, you will deploy a Service Bus namespace with topic and subscription.</span></span>

<span data-ttu-id="60366-117">[Service Bus-ämnen och prenumerationer](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) tillhandahålla en en-till-många-kommunikation i en *Publicera/prenumerera* mönster.</span><span class="sxs-lookup"><span data-stu-id="60366-117">[Service Bus topics and subscriptions](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) provide a one-to-many form of communication, in a *publish/subscribe* pattern.</span></span>

<span data-ttu-id="60366-118">Klicka på följande knapp för att köra distributionen automatiskt:</span><span class="sxs-lookup"><span data-stu-id="60366-118">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="60366-119">[![Distribuera till Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="60366-119">[![Deploy to Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="60366-120">Parametrar</span><span class="sxs-lookup"><span data-stu-id="60366-120">Parameters</span></span>

<span data-ttu-id="60366-121">Med Azure Resource Manager kan du definiera parametrar för värden som du vill ange när mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="60366-121">With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="60366-122">Mallen innehåller ett avsnitt som heter `Parameters` som innehåller alla parametervärdena.</span><span class="sxs-lookup"><span data-stu-id="60366-122">The template includes a section called `Parameters` that contains all of the parameter values.</span></span> <span data-ttu-id="60366-123">Du bör definiera en parameter för de värden som varierar baserat på projektet som du distribuerar eller baserat på miljön som du distribuerar till.</span><span class="sxs-lookup"><span data-stu-id="60366-123">You should define a parameter for those values that will vary based on the project you are deploying or based on the environment you are deploying to.</span></span> <span data-ttu-id="60366-124">Definiera inte parametrar för värden som alltid inte ändras.</span><span class="sxs-lookup"><span data-stu-id="60366-124">Do not define parameters for values that will always stay the same.</span></span> <span data-ttu-id="60366-125">Varje parametervärde används i mallen för att definiera de resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="60366-125">Each parameter value is used in the template to define the resources that are deployed.</span></span>

<span data-ttu-id="60366-126">Mallen definierar följande parametrar.</span><span class="sxs-lookup"><span data-stu-id="60366-126">The template defines the following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="60366-127">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="60366-127">serviceBusNamespaceName</span></span>
<span data-ttu-id="60366-128">Namnet på namnområdet för Service Bus för att skapa.</span><span class="sxs-lookup"><span data-stu-id="60366-128">The name of the Service Bus namespace to create.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a><span data-ttu-id="60366-129">serviceBusTopicName</span><span class="sxs-lookup"><span data-stu-id="60366-129">serviceBusTopicName</span></span>
<span data-ttu-id="60366-130">Namnet på det ämne som skapats i Service Bus-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="60366-130">The name of the topic created in the Service Bus namespace.</span></span>

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a><span data-ttu-id="60366-131">serviceBusSubscriptionName</span><span class="sxs-lookup"><span data-stu-id="60366-131">serviceBusSubscriptionName</span></span>
<span data-ttu-id="60366-132">Namnet på den prenumeration som skapats i Service Bus-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="60366-132">The name of the subscription created in the Service Bus namespace.</span></span>

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a><span data-ttu-id="60366-133">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="60366-133">serviceBusApiVersion</span></span>
<span data-ttu-id="60366-134">Service Bus-API-versionen av mallen.</span><span class="sxs-lookup"><span data-stu-id="60366-134">The Service Bus API version of the template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-to-deploy"></a><span data-ttu-id="60366-135">Resurser som ska distribueras</span><span class="sxs-lookup"><span data-stu-id="60366-135">Resources to deploy</span></span>
<span data-ttu-id="60366-136">Skapar en standard Service Bus-namnrymd av typen **Messaging**, med ämnet och prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="60366-136">Creates a standard Service Bus namespace of type **Messaging**, with topic and subscription.</span></span>

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
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]",
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {}
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a><span data-ttu-id="60366-137">Kommandon för att köra distributionen</span><span class="sxs-lookup"><span data-stu-id="60366-137">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="60366-138">PowerShell</span><span class="sxs-lookup"><span data-stu-id="60366-138">PowerShell</span></span>
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="60366-139">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="60366-139">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="60366-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="60366-140">Next steps</span></span>
<span data-ttu-id="60366-141">Nu när du har skapat och distribuerat resurser med Azure Resource Manager, lär du dig hur du hanterar dessa resurser genom att visa dessa artiklar:</span><span class="sxs-lookup"><span data-stu-id="60366-141">Now that you've created and deployed resources using Azure Resource Manager, learn how to manage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="60366-142">Hantera Service Bus med PowerShell</span><span class="sxs-lookup"><span data-stu-id="60366-142">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="60366-143">Hantera Service Bus-resurser med Service Bus Explorer</span><span class="sxs-lookup"><span data-stu-id="60366-143">Manage Service Bus resources with the Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus namespace with topic and subscription]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/
