---
title: "Automatiskt skala upp Azure Event Hubs genomflödesenheter | Microsoft Docs"
description: "Aktivera automatisk ökar på ett namnområde för att automatiskt skala upp enheter"
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: shvija;sethm
ms.openlocfilehash: b085091ea7bfd601efb0eee84144ddd091422d6e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a><span data-ttu-id="a26ca-103">Automatiskt skala upp Azure Event Hubs genomflödesenheter</span><span class="sxs-lookup"><span data-stu-id="a26ca-103">Automatically scale up Azure Event Hubs throughput units</span></span>

## <a name="overview"></a><span data-ttu-id="a26ca-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a26ca-104">Overview</span></span>

<span data-ttu-id="a26ca-105">Händelsehubbar i Azure är en mycket skalbar dataströmning plattform.</span><span class="sxs-lookup"><span data-stu-id="a26ca-105">Azure Event Hubs is a highly scalable data streaming platform.</span></span> <span data-ttu-id="a26ca-106">Händelsehubbar kunder öka därför ofta sin användning efter onboarding till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a26ca-106">As such, Event Hubs customers often increase their usage after onboarding to the service.</span></span> <span data-ttu-id="a26ca-107">Detta ökar krävs att du ökar förbestämt genomflödesenheter (TUs) för att skala Händelsehubbar och hantera större överföringshastighet.</span><span class="sxs-lookup"><span data-stu-id="a26ca-107">Such increases require increasing the predetermined throughput units (TUs) to scale Event Hubs and handle larger transfer rates.</span></span> <span data-ttu-id="a26ca-108">Den *automatiskt öka* funktion i Händelsehubbar skalas automatiskt antalet TUs användning behov.</span><span class="sxs-lookup"><span data-stu-id="a26ca-108">The *Auto-inflate* feature of Event Hubs automatically scales up the number of TUs to meet usage needs.</span></span> <span data-ttu-id="a26ca-109">Öka TUs förhindrar begränsning scenarier där:</span><span class="sxs-lookup"><span data-stu-id="a26ca-109">Increasing TUs prevents throttling scenarios, in which:</span></span>

* <span data-ttu-id="a26ca-110">Data ingång priser överskrida ange TUs.</span><span class="sxs-lookup"><span data-stu-id="a26ca-110">Data ingress rates exceed set TUs.</span></span>
* <span data-ttu-id="a26ca-111">Begäran om data utgång priser överskrider ange TUs.</span><span class="sxs-lookup"><span data-stu-id="a26ca-111">Data egress request rates exceed set TUs.</span></span>

## <a name="how-auto-inflate-works"></a><span data-ttu-id="a26ca-112">Så här fungerar automatiskt öka</span><span class="sxs-lookup"><span data-stu-id="a26ca-112">How Auto-inflate works</span></span>

<span data-ttu-id="a26ca-113">Event Hubs trafik styrs av genomflödesenheter.</span><span class="sxs-lookup"><span data-stu-id="a26ca-113">Event Hubs traffic is controlled by throughput units.</span></span> <span data-ttu-id="a26ca-114">En enda TU kan 1 MB per sekund på inkommande trafik och två gånger att mängden utgång.</span><span class="sxs-lookup"><span data-stu-id="a26ca-114">A single TU allows 1 MB per second of ingress and twice that amount of egress.</span></span> <span data-ttu-id="a26ca-115">Standard Händelsehubbar kan konfigureras med 1-20 genomflödesenheter.</span><span class="sxs-lookup"><span data-stu-id="a26ca-115">Standard Event Hubs can be configured with 1-20 throughput units.</span></span> <span data-ttu-id="a26ca-116">Öka automatiskt kan du börja litet med minsta obligatoriska genomflödesenheter.</span><span class="sxs-lookup"><span data-stu-id="a26ca-116">Auto-inflate enables you to start small with the minimum required throughput units.</span></span> <span data-ttu-id="a26ca-117">Funktionen skalas sedan automatiskt till den maximala gränsen på genomflödesenheter du behöver, beroende på ökade trafiken.</span><span class="sxs-lookup"><span data-stu-id="a26ca-117">The feature then scales automatically to the maximum limit of throughput units you need, depending on the increase in your traffic.</span></span> <span data-ttu-id="a26ca-118">Automatiskt öka ger följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a26ca-118">Auto-inflate provides the following benefits:</span></span>

- <span data-ttu-id="a26ca-119">En effektiv skalning mekanism börja litet och skala upp medan du växer.</span><span class="sxs-lookup"><span data-stu-id="a26ca-119">An efficient scaling mechanism to start small and scale up as you grow.</span></span>
- <span data-ttu-id="a26ca-120">Skala automatiskt till den angivna övre gränsen utan begränsning problem.</span><span class="sxs-lookup"><span data-stu-id="a26ca-120">Automatically scale to the specified upper limit without throttling issues.</span></span>
- <span data-ttu-id="a26ca-121">Mer kontroll över skalning, som du styr när och hur mycket skala.</span><span class="sxs-lookup"><span data-stu-id="a26ca-121">More control over scaling, as you control when and how much to scale.</span></span>

## <a name="enable-auto-inflate-on-a-namespace"></a><span data-ttu-id="a26ca-122">Aktivera automatisk ökar på ett namnområde</span><span class="sxs-lookup"><span data-stu-id="a26ca-122">Enable Auto-inflate on a namespace</span></span>

<span data-ttu-id="a26ca-123">Du kan aktivera eller inaktivera automatisk ökar på ett namnområde med någon av följande metoder:</span><span class="sxs-lookup"><span data-stu-id="a26ca-123">You can enable or disable Auto-inflate on a namespace using either of the following methods:</span></span>

1. <span data-ttu-id="a26ca-124">Den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a26ca-124">The [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="a26ca-125">En Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="a26ca-125">An Azure Resource Manager template.</span></span>

### <a name="enable-auto-inflate-through-the-portal"></a><span data-ttu-id="a26ca-126">Aktivera automatisk öka via portalen</span><span class="sxs-lookup"><span data-stu-id="a26ca-126">Enable Auto-inflate through the portal</span></span>

<span data-ttu-id="a26ca-127">När du skapar ett namnområde för Händelsehubbar kan du aktivera funktionen automatiskt öka på ett namnområde:</span><span class="sxs-lookup"><span data-stu-id="a26ca-127">You can enable the Auto-inflate feature on a namespace when creating an Event Hubs namespace:</span></span>
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

<span data-ttu-id="a26ca-128">Med det här alternativet är aktiverat, kan du börja litet på dina enheter och skala upp som kräver din användning.</span><span class="sxs-lookup"><span data-stu-id="a26ca-128">With this option enabled, you can start small on your throughput units and scale up as your usage needs increase.</span></span> <span data-ttu-id="a26ca-129">Den övre gränsen för inflationen påverkar inte prissättning, vilken beror på antalet TUs används per timme.</span><span class="sxs-lookup"><span data-stu-id="a26ca-129">The upper limit for inflation does not affect pricing, which depends on the number of TUs used per hour.</span></span>

<span data-ttu-id="a26ca-130">Du kan också aktivera automatisk-ökar med hjälp av den **skala** alternativet på inställningsbladet i portalen:</span><span class="sxs-lookup"><span data-stu-id="a26ca-130">You can also enable Auto-inflate using the **Scale** option on the settings blade in the portal:</span></span>
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a><span data-ttu-id="a26ca-131">Aktivera automatisk-ökar med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="a26ca-131">Enable Auto-Inflate using an Azure Resource Manager template</span></span>

<span data-ttu-id="a26ca-132">Du kan aktivera automatisk ökar under en Azure Resource Manager för malldistribution.</span><span class="sxs-lookup"><span data-stu-id="a26ca-132">You can enable Auto-inflate during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="a26ca-133">Till exempel den `isAutoInflateEnabled` egenskapen **SANT** och ange `maximumThroughputUnits` till 10.</span><span class="sxs-lookup"><span data-stu-id="a26ca-133">For example, set the `isAutoInflateEnabled` property to **true** and set `maximumThroughputUnits` to 10.</span></span>

```json
"resources": [
        {
            "apiVersion": "2017-04-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.EventHub/Namespaces",
            "location": "[variables('location')]",
            "sku": {
                "name": "Standard",
                "tier": "Standard"
            },
            "properties": {
                "isAutoInflateEnabled": true,
                "maximumThroughputUnits": 10
            },
            "resources": [
                {
                    "apiVersion": "2017-04-01",
                    "name": "[parameters('eventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
                    ],
                    "properties": {},
                    "resources": [
                        {
                            "apiVersion": "2017-04-01",
                            "name": "[parameters('consumerGroupName')]",
                            "type": "ConsumerGroups",
                            "dependsOn": [
                                "[parameters('eventHubName')]"
                            ],
                            "properties": {}
                        }
                    ]
                }
            ]
        }
    ]
```

<span data-ttu-id="a26ca-134">Fullständig mallen finns i [skapa Händelsehubbar namnområde och aktivera ökar](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) mall på GitHub.</span><span class="sxs-lookup"><span data-stu-id="a26ca-134">For the complete template, see the [Create Event Hubs namespace and enable inflate](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) template on GitHub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a26ca-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a26ca-135">Next steps</span></span>

<span data-ttu-id="a26ca-136">Du kan lära dig mer om Event Hubs genom att gå till följande länkar:</span><span class="sxs-lookup"><span data-stu-id="a26ca-136">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="a26ca-137">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="a26ca-137">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="a26ca-138">Skapa en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="a26ca-138">Create an Event Hub</span></span>](event-hubs-create.md)
