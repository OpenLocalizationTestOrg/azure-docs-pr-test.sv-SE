---
title: "aaaView Azure Nätverksbevakaren topologi - REST API | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse REST-API tooquery nätverkets topologi."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: de9af643-aea1-4c4c-89c5-21f1bf334c06
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 39292025bcd561f007c9e31271b1389be48ea79f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-rest-api"></a>Visa Nätverksbevakaren topologi med REST API

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-topology-powershell.md)
> - [CLI 1.0](network-watcher-topology-cli-nodejs.md)
> - [CLI 2.0](network-watcher-topology-cli.md)
> - [REST-API](network-watcher-topology-rest.md)

hello topologi funktion i Nätverksbevakaren ger en bild av hello nätverksresurser i en prenumeration. Hello-portalen visas den här visualiseringen tooyou automatiskt. hello informationen bakom hello topologiska vyn i hello portal kan hämtas via PowerShell.
Den här funktionen gör hello topologiinformation mer flexibla hello data kan användas av andra verktyg toobuild ut hello visualiseringen.

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

I det här scenariot kan du hämta hello topologiinformation. ARMclient är används toocall hello REST-API med hjälp av PowerShell. ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.

## <a name="scenario"></a>Scenario

hello-scenario som beskrivs i den här artikeln hämtar hello topologi svar för en viss resursgrupp.

## <a name="log-in-with-armclient"></a>Logga in med ARMClient

Logga in tooarmclient med dina autentiseringsuppgifter för Azure.

```PowerShell
armclient login
```

## <a name="retrieve-topology"></a>Hämta topologi

följande exempel hello begär hello topologi från hello REST API.  hello exempel är parametriserade tooallow flexibilitet att skapa ett exempel.  Ersätter alla värden med \< \> omgivande.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>" # Resource group name toorun topology on
$NWresourceGroupName = "<resource group name>" # Network Watcher resource group name
$networkWatcherName = "<network watcher name>"
$requestBody = @"
{
    'targetResourceGroupName': '${resourceGroupName}'
}
"@

armclient POST "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/topology?api-version=2016-07-01" $requestBody
```

hello efter svar är ett exempel på ett kortare svar som returneras när hämta topologin för en resursgrupp:

```json
{
  "id": "ecd6c860-9cf5-411a-bdba-512f8df7799f",
  "createdDateTime": "2017-01-18T04:13:07.1974591Z",
  "lastModified": "2017-01-17T22:11:52.3527348Z",
  "resources": [
    {
      "name": "virtualNetwork1",
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/{virtualNetworkName}",
      "location": "westcentralus",
      "associations": [
        {
          "name": "{subnetName}",
          "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/(virtualNetworkName)/subnets
/{subnetName}",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "webtestnsg-wjplxls65qcto",
      "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}
s65qcto",
      "associationType": "Associated"
    },
    ...
  ]
}
```

## <a name="next-steps"></a>Nästa steg

Lär dig hur toovisualize NSG-flöde loggar med Power BI genom att besöka [visualisera NSG flödar loggar med Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)

