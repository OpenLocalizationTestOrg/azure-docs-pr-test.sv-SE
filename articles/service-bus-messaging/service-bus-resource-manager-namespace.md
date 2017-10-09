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
# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a><span data-ttu-id="178cc-103">Skapa ett namnområde för Service Bus med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="178cc-103">Create a Service Bus namespace using an Azure Resource Manager template</span></span>

<span data-ttu-id="178cc-104">Den här artikeln beskriver hur toouse en Azure Resource Manager-mall som skapar en Service Bus-namnrymd av typen **Messaging** med en Standard/enkel SKU.</span><span class="sxs-lookup"><span data-stu-id="178cc-104">This article describes how toouse an Azure Resource Manager template that creates a Service Bus namespace of type **Messaging** with a Standard/Basic SKU.</span></span> <span data-ttu-id="178cc-105">hello artikel definierar också hello-parametrar som anges för hello körning av hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="178cc-105">hello article also defines hello parameters that are specified for hello execution of hello deployment.</span></span> <span data-ttu-id="178cc-106">Du kan använda den här mallen för din egen distribution eller anpassa den toomeet dina krav.</span><span class="sxs-lookup"><span data-stu-id="178cc-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="178cc-107">Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="178cc-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="178cc-108">Hello fullständig mallen finns hello [mall för Service Bus-namnområde] [ Service Bus namespace template] på GitHub.</span><span class="sxs-lookup"><span data-stu-id="178cc-108">For hello complete template, see hello [Service Bus namespace template][Service Bus namespace template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="178cc-109">hello följande Azure Resource Manager-mallar är tillgängliga för hämtning och distribution.</span><span class="sxs-lookup"><span data-stu-id="178cc-109">hello following Azure Resource Manager templates are available for download and deployment.</span></span> 
> 
> * [<span data-ttu-id="178cc-110">Skapa ett namnområde för Service Bus med kön</span><span class="sxs-lookup"><span data-stu-id="178cc-110">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="178cc-111">Skapa ett namnområde för Service Bus med ämnet och prenumerationen</span><span class="sxs-lookup"><span data-stu-id="178cc-111">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> * [<span data-ttu-id="178cc-112">Skapa ett namnområde för Service Bus med kön och auktorisering regel</span><span class="sxs-lookup"><span data-stu-id="178cc-112">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="178cc-113">Skapa ett namnområde för Service Bus med ämne, prenumeration och regel</span><span class="sxs-lookup"><span data-stu-id="178cc-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="178cc-114">toocheck för hello senaste mallar, besök hello [Azure-Snabbstartsmallar] [ Azure Quickstart Templates] galleriet och Sök efter Service Bus.</span><span class="sxs-lookup"><span data-stu-id="178cc-114">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Service Bus.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="178cc-115">Vad vill du distribuera?</span><span class="sxs-lookup"><span data-stu-id="178cc-115">What will you deploy?</span></span>
<span data-ttu-id="178cc-116">Med den här mallen du ska distribuera en Service Bus-namnrymd med ett [Basic, Standard och Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU.</span><span class="sxs-lookup"><span data-stu-id="178cc-116">With this template, you will deploy a Service Bus namespace with a [Basic, Standard, or Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU.</span></span>

<span data-ttu-id="178cc-117">toorun Hej distributionen automatiskt, klickar du på följande knapp hello:</span><span class="sxs-lookup"><span data-stu-id="178cc-117">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="178cc-118">[![Distribuera tooAzure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="178cc-118">[![Deploy tooAzure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="178cc-119">Parametrar</span><span class="sxs-lookup"><span data-stu-id="178cc-119">Parameters</span></span>
<span data-ttu-id="178cc-120">Med Azure Resource Manager kan du definiera parametrar för värden som du vill toospecify när hello mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="178cc-120">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="178cc-121">hello mallen innehåller ett avsnitt som heter `Parameters` som innehåller alla hello parametervärden.</span><span class="sxs-lookup"><span data-stu-id="178cc-121">hello template includes a section called `Parameters` that contains all of hello parameter values.</span></span> <span data-ttu-id="178cc-122">Du bör definiera en parameter för de värden som varierar baserat på hello-projekt som du distribuerar eller på hello-miljö som du distribuerar till.</span><span class="sxs-lookup"><span data-stu-id="178cc-122">You should define a parameter for those values that will vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="178cc-123">Definiera inte parametrar för värden som behålls alltid hello samma.</span><span class="sxs-lookup"><span data-stu-id="178cc-123">Do not define parameters for values that will always stay hello same.</span></span> <span data-ttu-id="178cc-124">Varje parametervärdet används i hello mallen toodefine hello resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="178cc-124">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="178cc-125">Den här mallen definierar hello följande parametrar.</span><span class="sxs-lookup"><span data-stu-id="178cc-125">This template defines hello following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="178cc-126">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="178cc-126">serviceBusNamespaceName</span></span>
<span data-ttu-id="178cc-127">hello namnet på hello Service Bus-namnområde toocreate.</span><span class="sxs-lookup"><span data-stu-id="178cc-127">hello name of hello Service Bus namespace toocreate.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of hello Service Bus namespace" 
    }
}
```

### <a name="servicebussku"></a><span data-ttu-id="178cc-128">serviceBusSKU</span><span class="sxs-lookup"><span data-stu-id="178cc-128">serviceBusSKU</span></span>
<span data-ttu-id="178cc-129">hello namnet på hello Service Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) toocreate.</span><span class="sxs-lookup"><span data-stu-id="178cc-129">hello name of hello Service Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) toocreate.</span></span>

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

<span data-ttu-id="178cc-130">hello mallen definierar hello värden som tillåts för den här parametern (Basic, Standard och Premium) och tilldelas ett standardvärde (Standard), om inget värde anges.</span><span class="sxs-lookup"><span data-stu-id="178cc-130">hello template defines hello values that are permitted for this parameter (Basic, Standard, or Premium) and assigns a default value (Standard) if no value is specified.</span></span>

<span data-ttu-id="178cc-131">Läs mer om prissättningen för Service Bus [Service Bus priser och fakturering][Service Bus pricing and billing].</span><span class="sxs-lookup"><span data-stu-id="178cc-131">For more information about Service Bus pricing, see [Service Bus pricing and billing][Service Bus pricing and billing].</span></span>

### <a name="servicebusapiversion"></a><span data-ttu-id="178cc-132">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="178cc-132">serviceBusApiVersion</span></span>
<span data-ttu-id="178cc-133">hello Service Bus-API-version av hello mallen.</span><span class="sxs-lookup"><span data-stu-id="178cc-133">hello Service Bus API version of hello template.</span></span>

```json
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by hello template" 
       } 
```

## <a name="resources-toodeploy"></a><span data-ttu-id="178cc-134">Resurser toodeploy</span><span class="sxs-lookup"><span data-stu-id="178cc-134">Resources toodeploy</span></span>
### <a name="service-bus-namespace"></a><span data-ttu-id="178cc-135">Service Bus-namnrymd</span><span class="sxs-lookup"><span data-stu-id="178cc-135">Service Bus namespace</span></span>
<span data-ttu-id="178cc-136">Skapar en standard Service Bus-namnrymd av typen **Messaging**.</span><span class="sxs-lookup"><span data-stu-id="178cc-136">Creates a standard Service Bus namespace of type **Messaging**.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="178cc-137">Kommandon toorun distribution</span><span class="sxs-lookup"><span data-stu-id="178cc-137">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="178cc-138">PowerShell</span><span class="sxs-lookup"><span data-stu-id="178cc-138">PowerShell</span></span>
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a><span data-ttu-id="178cc-139">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="178cc-139">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a><span data-ttu-id="178cc-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="178cc-140">Next steps</span></span>
<span data-ttu-id="178cc-141">Nu när du har skapat och distribuerat resurser med Azure Resource Manager, lär du dig hur toomanage dessa resurser genom att läsa dessa artiklar:</span><span class="sxs-lookup"><span data-stu-id="178cc-141">Now that you've created and deployed resources using Azure Resource Manager, learn how toomanage these resources by reading these articles:</span></span>

* [<span data-ttu-id="178cc-142">Hantera Service Bus med PowerShell</span><span class="sxs-lookup"><span data-stu-id="178cc-142">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="178cc-143">Hantera Service Bus-resurser med hello Service Bus Explorer</span><span class="sxs-lookup"><span data-stu-id="178cc-143">Manage Service Bus resources with hello Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace template]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Service Bus pricing and billing]: service-bus-pricing-billing.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
