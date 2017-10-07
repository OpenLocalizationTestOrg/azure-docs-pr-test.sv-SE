---
title: "aaaView Azure Nätverksbevakaren topologi - Azure CLI | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse Azure CLI tooquery nätverkets topologi."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 5cd279d7-3ab0-4813-aaa4-6a648bf74e7b
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: afa7e7dd844ecb2ab4c616ba99fa0a433f1e4ade
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-azure-cli"></a>Visa Nätverksbevakaren topologi med Azure CLI

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-topology-powershell.md)
> - [CLI 1.0](network-watcher-topology-cli-nodejs.md)
> - [CLI 2.0](network-watcher-topology-cli.md)
> - [REST-API](network-watcher-topology-rest.md)

hello topologi funktion i Nätverksbevakaren ger en bild av hello nätverksresurser i en prenumeration. Hello-portalen visas den här visualiseringen tooyou automatiskt. hello informationen bakom hello topologiska vyn i hello portal kan hämtas via PowerShell.
Den här funktionen gör hello topologiinformation mer flexibla hello data kan användas av andra verktyg toobuild ut hello visualiseringen.

Den här artikeln använder plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux. Nätverksbevakaren använder för närvarande Azure CLI 1.0 för CLI-stöd.

hello sammankoppling modelleras under två relationer.

- **Inneslutning** -exempel: innehåller ett undernät för virtuellt nätverk innehåller ett nätverkskort
- **Associerade** -exempel: NIC som är kopplad till en virtuell dator

hello finns följande lista egenskaper som returneras när du frågar hello topologi REST API.

* **namnet** - hello namn på hello resurs
* **ID** -hello hello resurs-uri.
* **plats** -hello plats där hello resurserna finns.
* **kopplingarna** -en lista över kopplingar toohello refererar till objektet.
    * **namnet** -hello namnet på hello refererar till resursen.
    * **resourceId** -hello resourceId är hello uri hello-resurs som refereras i hello association.
    * **associationType** -hello förhållandet mellan hello underordnat objekt och hello överordnade refererar till det här värdet. Giltiga värden är **innehåller** eller **associerade**.

## <a name="before-you-begin"></a>Innan du börjar

I det här scenariot kan du använda hello `network watcher topology` cmdlet tooretrieve hello topologiinformation. Det finns också en artikel om hur för[hämta nätverkets topologi med REST API](network-watcher-topology-rest.md).

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.

## <a name="scenario"></a>Scenario

hello-scenario som beskrivs i den här artikeln hämtar hello topologi svar för en viss resursgrupp.

## <a name="retrieve-topology"></a>Hämta topologi

Hej `network watcher topology` cmdlet hämtar hello topologin för en viss resursgrupp. Lägga till hello argumentet ”--json” tooview hello oput i json-format

```azurecli
azure network watcher topology -g resourceGroupName -n networkWatcherName -r topologyResourceGroupName --json
```

## <a name="results"></a>Resultat

hello resultaten har egenskapen name ”resurser”, som innehåller hello json svarstexten för hello `network watcher topology` cmdlet.  hello svaret innehåller hello resurser i hello säkerhetsgrupp för nätverk och deras associationer (det vill säga innehåller, associerade).

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "createdDateTime": "2017-02-17T22:20:59.461Z",
  "lastModified": "2016-12-19T22:23:02.546Z",
  "resources": [
    {
      "name": "testrg-vnet",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
      "location": "westcentralus",
      "associations": [
        {
          "name": "default",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "default",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
      "location": "westcentralus",
      "associations": []
    },
    {
      "name": "testclient",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testclient",
      "location": "westcentralus",
      "associations": [
        {
          "name": "testNic",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testNic",
          "associationType": "Contains"
        }
      ]
    },
    ...    
  ]
}
```

## <a name="next-steps"></a>Nästa steg

Mer information om hello säkerhetsregler som tillämpade tooyour nätverksresurser genom att besöka [Säkerhetsöversikt grupp vy](network-watcher-security-group-view-overview.md)
