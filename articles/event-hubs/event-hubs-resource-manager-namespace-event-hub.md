---
title: "Skapa en Azure Event Hubs namnområde och konsumenter med hjälp av en mall | Microsoft Docs"
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
ms.openlocfilehash: eb9a80eec0326aaa605cb8b21aecbaeec94ff212
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a><span data-ttu-id="98d01-103">Skapa ett namnområde för Händelsehubbar med nav- och konsumenten händelsegruppen med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="98d01-103">Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template</span></span>

<span data-ttu-id="98d01-104">Den här artikeln visar hur du använder en Azure Resource Manager-mall som skapar ett namnområde av typen Event Hubs, med en händelsehubb och en konsumentgrupp.</span><span class="sxs-lookup"><span data-stu-id="98d01-104">This article shows how to use an Azure Resource Manager template that creates a namespace of type Event Hubs, with one event hub and one consumer group.</span></span> <span data-ttu-id="98d01-105">Artikeln visar hur du definierar vilka resurser har distribuerats och hur du definierar parametrar som anges när distributionen körs.</span><span class="sxs-lookup"><span data-stu-id="98d01-105">The article shows how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="98d01-106">Du kan använda den här mallen för dina egna distributioner eller anpassa den så att den uppfyller dina krav</span><span class="sxs-lookup"><span data-stu-id="98d01-106">You can use this template for your own deployments, or customize it to meet your requirements</span></span>

<span data-ttu-id="98d01-107">Mer information om att skapa mallar finns i [Redigera Azure Resource Manager-mallar][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="98d01-107">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="98d01-108">Den fullständiga mallen finns i [NAV- och konsumenten grupp händelsemallen] [ Event Hub and consumer group template] på GitHub.</span><span class="sxs-lookup"><span data-stu-id="98d01-108">For the complete template, see the [Event hub and consumer group template][Event Hub and consumer group template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="98d01-109">Om du vill söka efter de senaste mallarna kan du gå till galleriet [Azure-snabbstartsmallar][Azure Quickstart Templates] och söka efter Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="98d01-109">To check for the latest templates, visit the [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Event Hubs.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="98d01-110">Vad vill du distribuera?</span><span class="sxs-lookup"><span data-stu-id="98d01-110">What will you deploy?</span></span>
<span data-ttu-id="98d01-111">Med den här mallen distribuerar du ett Händelsehubbar namnområde med en händelsehubb och en konsumentgrupp.</span><span class="sxs-lookup"><span data-stu-id="98d01-111">With this template, you will deploy an Event Hubs namespace with an event hub and a consumer group.</span></span>

<span data-ttu-id="98d01-112">[Event Hubs](event-hubs-what-is-event-hubs.md) är en tjänst för händelsebearbetning som används för att tillhandahålla en händelse- och telemetriingång till Azure i massiv skala med kort svarstid och hög tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="98d01-112">[Event Hubs](event-hubs-what-is-event-hubs.md) is an event processing service used to provide event and telemetry ingress to Azure at massive scale, with low latency and high reliability.</span></span>

<span data-ttu-id="98d01-113">Klicka på följande knapp för att köra distributionen automatiskt:</span><span class="sxs-lookup"><span data-stu-id="98d01-113">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="98d01-114">[![Distribuera till Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="98d01-114">[![Deploy to Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="98d01-115">Parametrar</span><span class="sxs-lookup"><span data-stu-id="98d01-115">Parameters</span></span>
<span data-ttu-id="98d01-116">Med Azure Resource Manager kan du definiera parametrar för värden som du vill ange när mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="98d01-116">With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="98d01-117">Mallen innehåller ett avsnitt som heter `Parameters` och som innehåller alla parametervärden.</span><span class="sxs-lookup"><span data-stu-id="98d01-117">The template includes a section called `Parameters` that contains all the parameter values.</span></span> <span data-ttu-id="98d01-118">Du bör definiera en parameter för de värden som varierar baserat på projektet som du distribuerar eller baserat på miljön som du distribuerar.</span><span class="sxs-lookup"><span data-stu-id="98d01-118">You should define a parameter for those values that will vary, based on the project you are deploying or based on the environment to which you are deploying.</span></span> <span data-ttu-id="98d01-119">Definiera inte parametrar för värden som aldrig ändras.</span><span class="sxs-lookup"><span data-stu-id="98d01-119">Do not define parameters for values that always stay the same.</span></span> <span data-ttu-id="98d01-120">Varje parametervärdet i mallen definierar de resurser som har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="98d01-120">Each parameter value in the template defines the resources that are deployed.</span></span>

<span data-ttu-id="98d01-121">Mallen definierar följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="98d01-121">The template defines the following parameters:</span></span>

### <a name="eventhubnamespacename"></a><span data-ttu-id="98d01-122">eventHubNamespaceName</span><span class="sxs-lookup"><span data-stu-id="98d01-122">eventHubNamespaceName</span></span>
<span data-ttu-id="98d01-123">Namnet på namnområdet för Event Hubs som ska skapas.</span><span class="sxs-lookup"><span data-stu-id="98d01-123">The name of the Event Hubs namespace to create.</span></span>

```json
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a><span data-ttu-id="98d01-124">eventHubName</span><span class="sxs-lookup"><span data-stu-id="98d01-124">eventHubName</span></span>
<span data-ttu-id="98d01-125">Namnet på händelsehubben som skapats i namnområdet för Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="98d01-125">The name of the event hub created in the Event Hubs namespace.</span></span>

```json
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a><span data-ttu-id="98d01-126">eventHubConsumerGroupName</span><span class="sxs-lookup"><span data-stu-id="98d01-126">eventHubConsumerGroupName</span></span>
<span data-ttu-id="98d01-127">Namnet på konsumentgrupp som skapats för händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="98d01-127">The name of the consumer group created for the event hub.</span></span>

```json
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a><span data-ttu-id="98d01-128">apiVersion</span><span class="sxs-lookup"><span data-stu-id="98d01-128">apiVersion</span></span>
<span data-ttu-id="98d01-129">API-versionen av mallen.</span><span class="sxs-lookup"><span data-stu-id="98d01-129">The API version of the template.</span></span>

```json
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a><span data-ttu-id="98d01-130">Resurser som ska distribueras</span><span class="sxs-lookup"><span data-stu-id="98d01-130">Resources to deploy</span></span>
<span data-ttu-id="98d01-131">Skapar ett namnområde av typen **EventHubs**, med en händelsehubb och en konsumentgrupp.</span><span class="sxs-lookup"><span data-stu-id="98d01-131">Creates a namespace of type **EventHubs**, with an event hub and a consumer group.</span></span>

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

## <a name="commands-to-run-deployment"></a><span data-ttu-id="98d01-132">Kommandon för att köra distributionen</span><span class="sxs-lookup"><span data-stu-id="98d01-132">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="98d01-133">PowerShell</span><span class="sxs-lookup"><span data-stu-id="98d01-133">PowerShell</span></span>
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a><span data-ttu-id="98d01-134">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="98d01-134">Azure CLI</span></span>
```cli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

## <a name="next-steps"></a><span data-ttu-id="98d01-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="98d01-135">Next steps</span></span>
<span data-ttu-id="98d01-136">Du kan lära dig mer om Event Hubs genom att gå till följande länkar:</span><span class="sxs-lookup"><span data-stu-id="98d01-136">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="98d01-137">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="98d01-137">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="98d01-138">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="98d01-138">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="98d01-139">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="98d01-139">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
