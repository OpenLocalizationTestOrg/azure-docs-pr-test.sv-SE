---
title: "aaaCreate en Händelsehubbar i Azure-namnområde och konsumenten-grupp med en mall | Microsoft Docs"
description: "Skapa ett namnområde för Händelsehubbar med en händelsehubb och en konsumentgrupp med hjälp av Azure Resource Manager-mallar"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/12/2017
ms.author: sethm;shvija
ms.openlocfilehash: 74b0d6b3fbe848705e2c20e628aa4e5269b53edb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a><span data-ttu-id="e9bc1-103">Skapa ett namnområde för Händelsehubbar med nav- och konsumenten händelsegruppen med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="e9bc1-103">Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template</span></span>

<span data-ttu-id="e9bc1-104">Den här artikeln visar hur toouse en Azure Resource Manager-mall som skapar ett namnområde av typen Event Hubs med en händelsehubb och en konsumentgrupp.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-104">This article shows how toouse an Azure Resource Manager template that creates a namespace of type Event Hubs, with one event hub and one consumer group.</span></span> <span data-ttu-id="e9bc1-105">hello artikeln visar hur toodefine vilka resurser har distribuerats och hur toodefine parametrar som anges när hello distributionen körs.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-105">hello article shows how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="e9bc1-106">Du kan använda den här mallen för din egen distribution eller anpassa den toomeet dina krav</span><span class="sxs-lookup"><span data-stu-id="e9bc1-106">You can use this template for your own deployments, or customize it toomeet your requirements</span></span>

<span data-ttu-id="e9bc1-107">Mer information om att skapa mallar finns i [Redigera Azure Resource Manager-mallar][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="e9bc1-107">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="e9bc1-108">Hello fullständig mallen finns hello [NAV- och konsumenten grupp händelsemallen] [ Event Hub and consumer group template] på GitHub.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-108">For hello complete template, see hello [Event hub and consumer group template][Event Hub and consumer group template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="e9bc1-109">toocheck för hello senaste mallar, besök hello [Azure-Snabbstartsmallar] [ Azure Quickstart Templates] galleriet och Sök efter Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-109">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Event Hubs.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="e9bc1-110">Vad vill du distribuera?</span><span class="sxs-lookup"><span data-stu-id="e9bc1-110">What will you deploy?</span></span>
<span data-ttu-id="e9bc1-111">Med den här mallen distribuerar du ett Händelsehubbar namnområde med en händelsehubb och en konsumentgrupp.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-111">With this template, you will deploy an Event Hubs namespace with an event hub and a consumer group.</span></span>

<span data-ttu-id="e9bc1-112">[Händelsehubbar](event-hubs-what-is-event-hubs.md) är en händelsebearbetning tjänsten används tooprovide händelse- och ingångsanspråk tooAzure i massiv skala med kort svarstid och hög tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-112">[Event Hubs](event-hubs-what-is-event-hubs.md) is an event processing service used tooprovide event and telemetry ingress tooAzure at massive scale, with low latency and high reliability.</span></span>

<span data-ttu-id="e9bc1-113">toorun Hej distributionen automatiskt, klickar du på följande knapp hello:</span><span class="sxs-lookup"><span data-stu-id="e9bc1-113">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="e9bc1-114">[![Distribuera tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="e9bc1-114">[![Deploy tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="e9bc1-115">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e9bc1-115">Parameters</span></span>
<span data-ttu-id="e9bc1-116">Med Azure Resource Manager kan du definiera parametrar för värden som du vill toospecify när hello mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-116">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="e9bc1-117">hello mallen innehåller ett avsnitt som heter `Parameters` som innehåller alla hello parametervärden.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-117">hello template includes a section called `Parameters` that contains all hello parameter values.</span></span> <span data-ttu-id="e9bc1-118">Du bör definiera en parameter för de värden som varierar baserat på hello-projekt som du distribuerar eller på hello miljö toowhich som du distribuerar.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-118">You should define a parameter for those values that will vary, based on hello project you are deploying or based on hello environment toowhich you are deploying.</span></span> <span data-ttu-id="e9bc1-119">Definiera inte parametrar för värden som alltid hello samma.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-119">Do not define parameters for values that always stay hello same.</span></span> <span data-ttu-id="e9bc1-120">Varje parametervärdet i hello mallen definierar hello resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-120">Each parameter value in hello template defines hello resources that are deployed.</span></span>

<span data-ttu-id="e9bc1-121">hello mallen definierar hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="e9bc1-121">hello template defines hello following parameters:</span></span>

### <a name="eventhubnamespacename"></a><span data-ttu-id="e9bc1-122">eventHubNamespaceName</span><span class="sxs-lookup"><span data-stu-id="e9bc1-122">eventHubNamespaceName</span></span>
<span data-ttu-id="e9bc1-123">hello namnet på hello Händelsehubbar namnområde toocreate.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-123">hello name of hello Event Hubs namespace toocreate.</span></span>

```json
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a><span data-ttu-id="e9bc1-124">eventHubName</span><span class="sxs-lookup"><span data-stu-id="e9bc1-124">eventHubName</span></span>
<span data-ttu-id="e9bc1-125">hello namnet på hello händelsehubb skapas i hello Händelsehubbar namnområde.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-125">hello name of hello event hub created in hello Event Hubs namespace.</span></span>

```json
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a><span data-ttu-id="e9bc1-126">eventHubConsumerGroupName</span><span class="sxs-lookup"><span data-stu-id="e9bc1-126">eventHubConsumerGroupName</span></span>
<span data-ttu-id="e9bc1-127">hello namnet på hello konsumentgrupp som skapats för hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-127">hello name of hello consumer group created for hello event hub.</span></span>

```json
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a><span data-ttu-id="e9bc1-128">apiVersion</span><span class="sxs-lookup"><span data-stu-id="e9bc1-128">apiVersion</span></span>
<span data-ttu-id="e9bc1-129">hello API-version av hello mallen.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-129">hello API version of hello template.</span></span>

```json
"apiVersion": {
"type": "string"
}
```

## <a name="resources-toodeploy"></a><span data-ttu-id="e9bc1-130">Resurser toodeploy</span><span class="sxs-lookup"><span data-stu-id="e9bc1-130">Resources toodeploy</span></span>
<span data-ttu-id="e9bc1-131">Skapar ett namnområde av typen **EventHubs**, med en händelsehubb och en konsumentgrupp.</span><span class="sxs-lookup"><span data-stu-id="e9bc1-131">Creates a namespace of type **EventHubs**, with an event hub and a consumer group.</span></span>

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-toorun-deployment"></a><span data-ttu-id="e9bc1-132">Kommandon toorun distribution</span><span class="sxs-lookup"><span data-stu-id="e9bc1-132">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="e9bc1-133">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e9bc1-133">PowerShell</span></span>
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a><span data-ttu-id="e9bc1-134">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e9bc1-134">Azure CLI</span></span>
```cli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

## <a name="next-steps"></a><span data-ttu-id="e9bc1-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e9bc1-135">Next steps</span></span>
<span data-ttu-id="e9bc1-136">Mer information om Händelsehubbar genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="e9bc1-136">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="e9bc1-137">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="e9bc1-137">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="e9bc1-138">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="e9bc1-138">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="e9bc1-139">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="e9bc1-139">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
