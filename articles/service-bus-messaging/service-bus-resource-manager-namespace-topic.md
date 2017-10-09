---
title: "aaaCreate Azure Service Bus-namnområde avsnittet prenumeration med hjälp av Azure Resource Manager-mall | Microsoft Docs"
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
ms.openlocfilehash: 9b5f7d8710e598b73c0a7ea3daf8c300f7fa9ecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a><span data-ttu-id="12d73-103">Skapa ett namnområde för Service Bus med ämne och en prenumeration med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="12d73-103">Create a Service Bus namespace with topic and subscription using an Azure Resource Manager template</span></span>

<span data-ttu-id="12d73-104">Den här artikeln visar hur toouse en Azure Resource Manager-mall som skapar en Service Bus-namnrymd och ett ämne och en prenumeration inom det här namnområdet.</span><span class="sxs-lookup"><span data-stu-id="12d73-104">This article shows how toouse an Azure Resource Manager template that creates a Service Bus namespace and a topic and subscription within that namespace.</span></span> <span data-ttu-id="12d73-105">Du får lära dig hur toodefine vilka resurser har distribuerats och hur toodefine parametrar som anges när hello distributionen körs.</span><span class="sxs-lookup"><span data-stu-id="12d73-105">You will learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="12d73-106">Du kan använda den här mallen för din egen distribution eller anpassa den toomeet dina krav</span><span class="sxs-lookup"><span data-stu-id="12d73-106">You can use this template for your own deployments, or customize it toomeet your requirements</span></span>

<span data-ttu-id="12d73-107">Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="12d73-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="12d73-108">Hello fullständig mallen finns hello [Service Bus-namnrymd med ämnet och prenumerationen] [ Service Bus namespace with topic and subscription] mall.</span><span class="sxs-lookup"><span data-stu-id="12d73-108">For hello complete template, see hello [Service Bus namespace with topic and subscription][Service Bus namespace with topic and subscription] template.</span></span>

> [!NOTE]
> <span data-ttu-id="12d73-109">hello följande Azure Resource Manager-mallar är tillgängliga för hämtning och distribution.</span><span class="sxs-lookup"><span data-stu-id="12d73-109">hello following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="12d73-110">Skapa ett namnområde för Service Bus</span><span class="sxs-lookup"><span data-stu-id="12d73-110">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="12d73-111">Skapa ett namnområde för Service Bus med kön</span><span class="sxs-lookup"><span data-stu-id="12d73-111">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="12d73-112">Skapa ett namnområde för Service Bus med kön och auktorisering regel</span><span class="sxs-lookup"><span data-stu-id="12d73-112">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="12d73-113">Skapa ett namnområde för Service Bus med ämne, prenumeration och regel</span><span class="sxs-lookup"><span data-stu-id="12d73-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="12d73-114">toocheck för hello senaste mallar, besök hello [Azure-Snabbstartsmallar] [ Azure Quickstart Templates] galleriet och Sök efter ”Service Bus”.</span><span class="sxs-lookup"><span data-stu-id="12d73-114">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for "Service Bus."</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="12d73-115">Vad vill du distribuera?</span><span class="sxs-lookup"><span data-stu-id="12d73-115">What will you deploy?</span></span>

<span data-ttu-id="12d73-116">Med den här mallen distribuerar du en Service Bus-namnrymd med ämne och en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="12d73-116">With this template, you will deploy a Service Bus namespace with topic and subscription.</span></span>

<span data-ttu-id="12d73-117">[Service Bus-ämnen och prenumerationer](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) tillhandahålla en en-till-många-kommunikation i en *Publicera/prenumerera* mönster.</span><span class="sxs-lookup"><span data-stu-id="12d73-117">[Service Bus topics and subscriptions](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) provide a one-to-many form of communication, in a *publish/subscribe* pattern.</span></span>

<span data-ttu-id="12d73-118">toorun Hej distributionen automatiskt, klickar du på följande knapp hello:</span><span class="sxs-lookup"><span data-stu-id="12d73-118">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="12d73-119">[![Distribuera tooAzure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="12d73-119">[![Deploy tooAzure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="12d73-120">Parametrar</span><span class="sxs-lookup"><span data-stu-id="12d73-120">Parameters</span></span>

<span data-ttu-id="12d73-121">Med Azure Resource Manager kan du definiera parametrar för värden som du vill toospecify när hello mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="12d73-121">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="12d73-122">hello mallen innehåller ett avsnitt som heter `Parameters` som innehåller alla hello parametervärden.</span><span class="sxs-lookup"><span data-stu-id="12d73-122">hello template includes a section called `Parameters` that contains all of hello parameter values.</span></span> <span data-ttu-id="12d73-123">Du bör definiera en parameter för de värden som varierar baserat på hello-projekt som du distribuerar eller på hello-miljö som du distribuerar till.</span><span class="sxs-lookup"><span data-stu-id="12d73-123">You should define a parameter for those values that will vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="12d73-124">Definiera inte parametrar för värden som behålls alltid hello samma.</span><span class="sxs-lookup"><span data-stu-id="12d73-124">Do not define parameters for values that will always stay hello same.</span></span> <span data-ttu-id="12d73-125">Varje parametervärdet används i hello mallen toodefine hello resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="12d73-125">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="12d73-126">hello mallen definierar hello följande parametrar.</span><span class="sxs-lookup"><span data-stu-id="12d73-126">hello template defines hello following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="12d73-127">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="12d73-127">serviceBusNamespaceName</span></span>
<span data-ttu-id="12d73-128">hello namnet på hello Service Bus-namnområde toocreate.</span><span class="sxs-lookup"><span data-stu-id="12d73-128">hello name of hello Service Bus namespace toocreate.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a><span data-ttu-id="12d73-129">serviceBusTopicName</span><span class="sxs-lookup"><span data-stu-id="12d73-129">serviceBusTopicName</span></span>
<span data-ttu-id="12d73-130">hello namnet på hello ämne som skapats i hello Service Bus-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="12d73-130">hello name of hello topic created in hello Service Bus namespace.</span></span>

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a><span data-ttu-id="12d73-131">serviceBusSubscriptionName</span><span class="sxs-lookup"><span data-stu-id="12d73-131">serviceBusSubscriptionName</span></span>
<span data-ttu-id="12d73-132">hello namnet på hello-prenumeration som skapats i hello Service Bus-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="12d73-132">hello name of hello subscription created in hello Service Bus namespace.</span></span>

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a><span data-ttu-id="12d73-133">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="12d73-133">serviceBusApiVersion</span></span>
<span data-ttu-id="12d73-134">hello Service Bus-API-version av hello mallen.</span><span class="sxs-lookup"><span data-stu-id="12d73-134">hello Service Bus API version of hello template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-toodeploy"></a><span data-ttu-id="12d73-135">Resurser toodeploy</span><span class="sxs-lookup"><span data-stu-id="12d73-135">Resources toodeploy</span></span>
<span data-ttu-id="12d73-136">Skapar en standard Service Bus-namnrymd av typen **Messaging**, med ämnet och prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="12d73-136">Creates a standard Service Bus namespace of type **Messaging**, with topic and subscription.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="12d73-137">Kommandon toorun distribution</span><span class="sxs-lookup"><span data-stu-id="12d73-137">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="12d73-138">PowerShell</span><span class="sxs-lookup"><span data-stu-id="12d73-138">PowerShell</span></span>
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="12d73-139">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="12d73-139">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="12d73-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12d73-140">Next steps</span></span>
<span data-ttu-id="12d73-141">Nu när du har skapat och distribuerat resurser med Azure Resource Manager, lär du dig hur toomanage dessa resurser genom att visa dessa artiklar:</span><span class="sxs-lookup"><span data-stu-id="12d73-141">Now that you've created and deployed resources using Azure Resource Manager, learn how toomanage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="12d73-142">Hantera Service Bus med PowerShell</span><span class="sxs-lookup"><span data-stu-id="12d73-142">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="12d73-143">Hantera Service Bus-resurser med hello Service Bus Explorer</span><span class="sxs-lookup"><span data-stu-id="12d73-143">Manage Service Bus resources with hello Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus namespace with topic and subscription]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/
