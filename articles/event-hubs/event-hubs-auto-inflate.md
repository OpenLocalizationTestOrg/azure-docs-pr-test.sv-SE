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
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Automatiskt skala upp Azure Event Hubs genomflödesenheter

## <a name="overview"></a>Översikt

Händelsehubbar i Azure är en mycket skalbar dataströmning plattform. Händelsehubbar kunder ökar därför ofta sin användning efter onboarding toohello. Detta ökar kräver ökande hello förutbestämd genomströmning enheter (TUs) tooscale Händelsehubbar och hantera större överföringshastighet. Hej *automatiskt öka* funktion i Händelsehubbar skalas automatiskt hello antal TUs toomeet användarbehov. Öka TUs förhindrar begränsning scenarier där:

* Data ingång priser överskrida ange TUs.
* Begäran om data utgång priser överskrider ange TUs.

## <a name="how-auto-inflate-works"></a>Så här fungerar automatiskt öka

Event Hubs trafik styrs av genomflödesenheter. En enda TU kan 1 MB per sekund på inkommande trafik och två gånger att mängden utgång. Standard Händelsehubbar kan konfigureras med 1-20 genomflödesenheter. Öka automatiskt kan du toostart liten med hello minsta nödvändiga genomflödesenheter. hello funktionen skalas sedan automatiskt toohello högsta antalet enheter du behöver, beroende på hello ökning av trafiken. Automatiskt öka innehåller hello följande fördelar:

- En effektiv skalning mekanism toostart små och skalas upp dig växa.
- Skala automatiskt toohello övre gränsen utan bandbreddsbegränsning problem.
- Mer kontroll över skalning, som du styr när och hur mycket tooscale.

## <a name="enable-auto-inflate-on-a-namespace"></a>Aktivera automatisk ökar på ett namnområde

Du kan aktivera eller inaktivera automatisk ökar på ett namnområde med någon av följande metoder hello:

1. Hej [Azure-portalen](https://portal.azure.com).
2. En Azure Resource Manager-mall.

### <a name="enable-auto-inflate-through-hello-portal"></a>Aktivera automatisk öka hello-portalen

När du skapar ett namnområde för Händelsehubbar kan du aktivera hello automatiskt öka funktion på ett namnområde:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

Med det här alternativet är aktiverat, kan du börja litet på dina enheter och skala upp som kräver din användning. hello påverkar övre gräns för inflationen inte prissättning, vilken beror på hello antalet TUs används per timme.

Du kan också aktivera automatisk ökar med hello **skala** alternativet på hello inställningsbladet i hello portal:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a>Aktivera automatisk-ökar med en Azure Resource Manager-mall

Du kan aktivera automatisk ökar under en Azure Resource Manager för malldistribution. Till exempel ange hello `isAutoInflateEnabled` egenskapen för**SANT** och ange `maximumThroughputUnits` too10.

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

Hello fullständig mallen finns hello [skapa Händelsehubbar namnområde och aktivera ökar](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) mall på GitHub.

## <a name="next-steps"></a>Nästa steg

Mer information om Händelsehubbar genom att besöka hello följande länkar:

* [Event Hubs-översikt](event-hubs-what-is-event-hubs.md)
* [Skapa en händelsehubb](event-hubs-create.md)
