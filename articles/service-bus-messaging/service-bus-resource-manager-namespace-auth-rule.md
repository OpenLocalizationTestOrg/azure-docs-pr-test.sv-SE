---
title: "auktoriseringsregeln aaaCreate Service Bus med hjälp av Azure Resource Manager-mall | Microsoft Docs"
description: "Skapa en regel för auktorisering av Service Bus för namnområdet och kön med Azure Resource Manager-mall"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 7f1443a0-5fa8-4d90-8637-1a977ef0b1f0
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: 48df97849281d3b47e9d722d4e821c874644be59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a><span data-ttu-id="06e5a-103">Skapa en regel för auktorisering av Service Bus för namnområdet och kö med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="06e5a-103">Create a Service Bus authorization rule for namespace and queue using an Azure Resource Manager template</span></span>

<span data-ttu-id="06e5a-104">Den här artikeln visar hur toouse en Azure Resource Manager-mall som skapar en [auktoriseringsregeln](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) för en Service Bus-namnrymd och kön.</span><span class="sxs-lookup"><span data-stu-id="06e5a-104">This article shows how toouse an Azure Resource Manager template that creates an [authorization rule](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) for a Service Bus namespace and queue.</span></span> <span data-ttu-id="06e5a-105">Du får lära dig hur toodefine vilka resurser har distribuerats och hur toodefine parametrar som anges när hello distributionen körs.</span><span class="sxs-lookup"><span data-stu-id="06e5a-105">You will learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="06e5a-106">Du kan använda den här mallen för din egen distribution eller anpassa den toomeet dina krav.</span><span class="sxs-lookup"><span data-stu-id="06e5a-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="06e5a-107">Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="06e5a-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="06e5a-108">Hello fullständig mallen finns hello [regelmall för Service Bus-auktorisering] [ Service Bus auth rule template] på GitHub.</span><span class="sxs-lookup"><span data-stu-id="06e5a-108">For hello complete template, see hello [Service Bus authorization rule template][Service Bus auth rule template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="06e5a-109">hello följande Azure Resource Manager-mallar är tillgängliga för hämtning och distribution.</span><span class="sxs-lookup"><span data-stu-id="06e5a-109">hello following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="06e5a-110">Skapa ett namnområde för Service Bus</span><span class="sxs-lookup"><span data-stu-id="06e5a-110">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="06e5a-111">Skapa ett namnområde för Service Bus med kön</span><span class="sxs-lookup"><span data-stu-id="06e5a-111">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="06e5a-112">Skapa ett namnområde för Service Bus med ämnet och prenumerationen</span><span class="sxs-lookup"><span data-stu-id="06e5a-112">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> * [<span data-ttu-id="06e5a-113">Skapa ett namnområde för Service Bus med ämne, prenumeration och regel</span><span class="sxs-lookup"><span data-stu-id="06e5a-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="06e5a-114">toocheck för hello senaste mallar, besök hello [Azure-Snabbstartsmallar] [ Azure Quickstart Templates] galleriet och Sök efter ”Service Bus”.</span><span class="sxs-lookup"><span data-stu-id="06e5a-114">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for "Service Bus."</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="06e5a-115">Vad vill du distribuera?</span><span class="sxs-lookup"><span data-stu-id="06e5a-115">What will you deploy?</span></span>
<span data-ttu-id="06e5a-116">Med den här mallen distribuerar du en regel för auktorisering av Service Bus för ett namnområde och meddelandeentitet (i det här fallet en kö).</span><span class="sxs-lookup"><span data-stu-id="06e5a-116">With this template, you will deploy a Service Bus authorization rule for a namespace and messaging entity (in this case, a queue).</span></span>

<span data-ttu-id="06e5a-117">Den här mallen använder [delade signatur åtkomst (SAS)](service-bus-sas.md) för autentisering.</span><span class="sxs-lookup"><span data-stu-id="06e5a-117">This template uses [Shared Access Signature (SAS)](service-bus-sas.md) for authentication.</span></span> <span data-ttu-id="06e5a-118">SAS kan program tooauthenticate tooService Bus med hjälp av en åtkomstnyckel som konfigurerats på hello namnområde eller hello messaging entitet (kö eller ett ämne) med vilka specifika rättigheter som är associerade.</span><span class="sxs-lookup"><span data-stu-id="06e5a-118">SAS enables applications tooauthenticate tooService Bus using an access key configured on hello namespace, or on hello messaging entity (queue or topic) with which specific rights are associated.</span></span> <span data-ttu-id="06e5a-119">Du kan sedan använda den här nyckeln toogenerate en SAS-token som klienter i sin tur kan använda tooauthenticate tooService Bus.</span><span class="sxs-lookup"><span data-stu-id="06e5a-119">You can then use this key toogenerate a SAS token that clients can in turn use tooauthenticate tooService Bus.</span></span>

<span data-ttu-id="06e5a-120">toorun Hej distributionen automatiskt, klickar du på följande knapp hello:</span><span class="sxs-lookup"><span data-stu-id="06e5a-120">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="06e5a-121">[![Distribuera tooAzure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="06e5a-121">[![Deploy tooAzure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="06e5a-122">Parametrar</span><span class="sxs-lookup"><span data-stu-id="06e5a-122">Parameters</span></span>

<span data-ttu-id="06e5a-123">Med Azure Resource Manager kan du definiera parametrar för värden som du vill toospecify när hello mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="06e5a-123">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="06e5a-124">hello mallen innehåller ett avsnitt som heter `Parameters` som innehåller alla hello parametervärden.</span><span class="sxs-lookup"><span data-stu-id="06e5a-124">hello template includes a section called `Parameters` that contains all of hello parameter values.</span></span> <span data-ttu-id="06e5a-125">Du bör definiera en parameter för de värden som varierar baserat på hello-projekt som du distribuerar eller på hello-miljö som du distribuerar till.</span><span class="sxs-lookup"><span data-stu-id="06e5a-125">You should define a parameter for those values that will vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="06e5a-126">Definiera inte parametrar för värden som behålls alltid hello samma.</span><span class="sxs-lookup"><span data-stu-id="06e5a-126">Do not define parameters for values that will always stay hello same.</span></span> <span data-ttu-id="06e5a-127">Varje parametervärdet används i hello mallen toodefine hello resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="06e5a-127">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="06e5a-128">hello mallen definierar hello följande parametrar.</span><span class="sxs-lookup"><span data-stu-id="06e5a-128">hello template defines hello following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="06e5a-129">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="06e5a-129">serviceBusNamespaceName</span></span>
<span data-ttu-id="06e5a-130">hello namnet på hello Service Bus-namnområde toocreate.</span><span class="sxs-lookup"><span data-stu-id="06e5a-130">hello name of hello Service Bus namespace toocreate.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="namespaceauthorizationrulename"></a><span data-ttu-id="06e5a-131">namespaceAuthorizationRuleName</span><span class="sxs-lookup"><span data-stu-id="06e5a-131">namespaceAuthorizationRuleName</span></span>
<span data-ttu-id="06e5a-132">hello namnet på hello auktoriseringsregeln för hello namnområde.</span><span class="sxs-lookup"><span data-stu-id="06e5a-132">hello name of hello authorization rule for hello namespace.</span></span>

```json
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a><span data-ttu-id="06e5a-133">serviceBusQueueName</span><span class="sxs-lookup"><span data-stu-id="06e5a-133">serviceBusQueueName</span></span>
<span data-ttu-id="06e5a-134">hello namnet på hello kön i hello Service Bus-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="06e5a-134">hello name of hello queue in hello Service Bus namespace.</span></span>

```json
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a><span data-ttu-id="06e5a-135">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="06e5a-135">serviceBusApiVersion</span></span>
<span data-ttu-id="06e5a-136">hello Service Bus-API-version av hello mallen.</span><span class="sxs-lookup"><span data-stu-id="06e5a-136">hello Service Bus API version of hello template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-toodeploy"></a><span data-ttu-id="06e5a-137">Resurser toodeploy</span><span class="sxs-lookup"><span data-stu-id="06e5a-137">Resources toodeploy</span></span>
<span data-ttu-id="06e5a-138">Skapar en standard Service Bus-namnrymd av typen **Messaging**, och en Service Bus auktoriseringsregel för namnområdet och entiteten.</span><span class="sxs-lookup"><span data-stu-id="06e5a-138">Creates a standard Service Bus namespace of type **Messaging**, and a Service Bus authorization rule for namespace and entity.</span></span>

```json
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## <a name="commands-toorun-deployment"></a><span data-ttu-id="06e5a-139">Kommandon toorun distribution</span><span class="sxs-lookup"><span data-stu-id="06e5a-139">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="06e5a-140">PowerShell</span><span class="sxs-lookup"><span data-stu-id="06e5a-140">PowerShell</span></span>
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="06e5a-141">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="06e5a-141">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="06e5a-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="06e5a-142">Next steps</span></span>
<span data-ttu-id="06e5a-143">Nu när du har skapat och distribuerat resurser med Azure Resource Manager, lär du dig hur toomanage dessa resurser genom att visa dessa artiklar:</span><span class="sxs-lookup"><span data-stu-id="06e5a-143">Now that you've created and deployed resources using Azure Resource Manager, learn how toomanage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="06e5a-144">Hantera Service Bus med PowerShell</span><span class="sxs-lookup"><span data-stu-id="06e5a-144">Manage Service Bus with PowerShell</span></span>](service-bus-powershell-how-to-provision.md)
* [<span data-ttu-id="06e5a-145">Hantera Service Bus-resurser med hello Service Bus Explorer</span><span class="sxs-lookup"><span data-stu-id="06e5a-145">Manage Service Bus resources with hello Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)
* [<span data-ttu-id="06e5a-146">Service Bus-autentisering och auktorisering</span><span class="sxs-lookup"><span data-stu-id="06e5a-146">Service Bus authentication and authorization</span></span>](service-bus-authentication-and-authorization.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus auth rule template]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
