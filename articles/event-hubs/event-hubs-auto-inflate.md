---
title: "aaaAutomatically skalas upp Azure Event Hubs genomflödesenheter | Microsoft Docs"
description: "Aktivera automatisk ökar i namnområdet tooautomatically skala upp enheter"
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
ms.openlocfilehash: 0f5216bcd619ccddc1fd4063a2f0131bfa36c7d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a><span data-ttu-id="e924a-103">Automatiskt skala upp Azure Event Hubs genomflödesenheter</span><span class="sxs-lookup"><span data-stu-id="e924a-103">Automatically scale up Azure Event Hubs throughput units</span></span>

## <a name="overview"></a><span data-ttu-id="e924a-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e924a-104">Overview</span></span>

<span data-ttu-id="e924a-105">Händelsehubbar i Azure är en mycket skalbar dataströmning plattform.</span><span class="sxs-lookup"><span data-stu-id="e924a-105">Azure Event Hubs is a highly scalable data streaming platform.</span></span> <span data-ttu-id="e924a-106">Händelsehubbar kunder ökar därför ofta sin användning efter onboarding toohello.</span><span class="sxs-lookup"><span data-stu-id="e924a-106">As such, Event Hubs customers often increase their usage after onboarding toohello service.</span></span> <span data-ttu-id="e924a-107">Detta ökar kräver ökande hello förutbestämd genomströmning enheter (TUs) tooscale Händelsehubbar och hantera större överföringshastighet.</span><span class="sxs-lookup"><span data-stu-id="e924a-107">Such increases require increasing hello predetermined throughput units (TUs) tooscale Event Hubs and handle larger transfer rates.</span></span> <span data-ttu-id="e924a-108">Hej *automatiskt öka* funktion i Händelsehubbar skalas automatiskt hello antal TUs toomeet användarbehov.</span><span class="sxs-lookup"><span data-stu-id="e924a-108">hello *Auto-inflate* feature of Event Hubs automatically scales up hello number of TUs toomeet usage needs.</span></span> <span data-ttu-id="e924a-109">Öka TUs förhindrar begränsning scenarier där:</span><span class="sxs-lookup"><span data-stu-id="e924a-109">Increasing TUs prevents throttling scenarios, in which:</span></span>

* <span data-ttu-id="e924a-110">Data ingång priser överskrida ange TUs.</span><span class="sxs-lookup"><span data-stu-id="e924a-110">Data ingress rates exceed set TUs.</span></span>
* <span data-ttu-id="e924a-111">Begäran om data utgång priser överskrider ange TUs.</span><span class="sxs-lookup"><span data-stu-id="e924a-111">Data egress request rates exceed set TUs.</span></span>

## <a name="how-auto-inflate-works"></a><span data-ttu-id="e924a-112">Så här fungerar automatiskt öka</span><span class="sxs-lookup"><span data-stu-id="e924a-112">How Auto-inflate works</span></span>

<span data-ttu-id="e924a-113">Event Hubs trafik styrs av genomflödesenheter.</span><span class="sxs-lookup"><span data-stu-id="e924a-113">Event Hubs traffic is controlled by throughput units.</span></span> <span data-ttu-id="e924a-114">En enda TU kan 1 MB per sekund på inkommande trafik och två gånger att mängden utgång.</span><span class="sxs-lookup"><span data-stu-id="e924a-114">A single TU allows 1 MB per second of ingress and twice that amount of egress.</span></span> <span data-ttu-id="e924a-115">Standard Händelsehubbar kan konfigureras med 1-20 genomflödesenheter.</span><span class="sxs-lookup"><span data-stu-id="e924a-115">Standard Event Hubs can be configured with 1-20 throughput units.</span></span> <span data-ttu-id="e924a-116">Öka automatiskt kan du toostart liten med hello minsta nödvändiga genomflödesenheter.</span><span class="sxs-lookup"><span data-stu-id="e924a-116">Auto-inflate enables you toostart small with hello minimum required throughput units.</span></span> <span data-ttu-id="e924a-117">hello funktionen skalas sedan automatiskt toohello högsta antalet enheter du behöver, beroende på hello ökning av trafiken.</span><span class="sxs-lookup"><span data-stu-id="e924a-117">hello feature then scales automatically toohello maximum limit of throughput units you need, depending on hello increase in your traffic.</span></span> <span data-ttu-id="e924a-118">Automatiskt öka innehåller hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e924a-118">Auto-inflate provides hello following benefits:</span></span>

- <span data-ttu-id="e924a-119">En effektiv skalning mekanism toostart små och skalas upp dig växa.</span><span class="sxs-lookup"><span data-stu-id="e924a-119">An efficient scaling mechanism toostart small and scale up as you grow.</span></span>
- <span data-ttu-id="e924a-120">Skala automatiskt toohello övre gränsen utan bandbreddsbegränsning problem.</span><span class="sxs-lookup"><span data-stu-id="e924a-120">Automatically scale toohello specified upper limit without throttling issues.</span></span>
- <span data-ttu-id="e924a-121">Mer kontroll över skalning, som du styr när och hur mycket tooscale.</span><span class="sxs-lookup"><span data-stu-id="e924a-121">More control over scaling, as you control when and how much tooscale.</span></span>

## <a name="enable-auto-inflate-on-a-namespace"></a><span data-ttu-id="e924a-122">Aktivera automatisk ökar på ett namnområde</span><span class="sxs-lookup"><span data-stu-id="e924a-122">Enable Auto-inflate on a namespace</span></span>

<span data-ttu-id="e924a-123">Du kan aktivera eller inaktivera automatisk ökar på ett namnområde med någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="e924a-123">You can enable or disable Auto-inflate on a namespace using either of hello following methods:</span></span>

1. <span data-ttu-id="e924a-124">Hej [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e924a-124">hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e924a-125">En Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="e924a-125">An Azure Resource Manager template.</span></span>

### <a name="enable-auto-inflate-through-hello-portal"></a><span data-ttu-id="e924a-126">Aktivera automatisk öka hello-portalen</span><span class="sxs-lookup"><span data-stu-id="e924a-126">Enable Auto-inflate through hello portal</span></span>

<span data-ttu-id="e924a-127">När du skapar ett namnområde för Händelsehubbar kan du aktivera hello automatiskt öka funktion på ett namnområde:</span><span class="sxs-lookup"><span data-stu-id="e924a-127">You can enable hello Auto-inflate feature on a namespace when creating an Event Hubs namespace:</span></span>
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

<span data-ttu-id="e924a-128">Med det här alternativet är aktiverat, kan du börja litet på dina enheter och skala upp som kräver din användning.</span><span class="sxs-lookup"><span data-stu-id="e924a-128">With this option enabled, you can start small on your throughput units and scale up as your usage needs increase.</span></span> <span data-ttu-id="e924a-129">hello påverkar övre gräns för inflationen inte prissättning, vilken beror på hello antalet TUs används per timme.</span><span class="sxs-lookup"><span data-stu-id="e924a-129">hello upper limit for inflation does not affect pricing, which depends on hello number of TUs used per hour.</span></span>

<span data-ttu-id="e924a-130">Du kan också aktivera automatisk ökar med hello **skala** alternativet på hello inställningsbladet i hello portal:</span><span class="sxs-lookup"><span data-stu-id="e924a-130">You can also enable Auto-inflate using hello **Scale** option on hello settings blade in hello portal:</span></span>
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a><span data-ttu-id="e924a-131">Aktivera automatisk-ökar med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="e924a-131">Enable Auto-Inflate using an Azure Resource Manager template</span></span>

<span data-ttu-id="e924a-132">Du kan aktivera automatisk ökar under en Azure Resource Manager för malldistribution.</span><span class="sxs-lookup"><span data-stu-id="e924a-132">You can enable Auto-inflate during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="e924a-133">Till exempel ange hello `isAutoInflateEnabled` egenskapen för**SANT** och ange `maximumThroughputUnits` too10.</span><span class="sxs-lookup"><span data-stu-id="e924a-133">For example, set hello `isAutoInflateEnabled` property too**true** and set `maximumThroughputUnits` too10.</span></span>

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

<span data-ttu-id="e924a-134">Hello fullständig mallen finns hello [skapa Händelsehubbar namnområde och aktivera ökar](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) mall på GitHub.</span><span class="sxs-lookup"><span data-stu-id="e924a-134">For hello complete template, see hello [Create Event Hubs namespace and enable inflate](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) template on GitHub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e924a-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e924a-135">Next steps</span></span>

<span data-ttu-id="e924a-136">Mer information om Händelsehubbar genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="e924a-136">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="e924a-137">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="e924a-137">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="e924a-138">Skapa en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="e924a-138">Create an Event Hub</span></span>](event-hubs-create.md)
