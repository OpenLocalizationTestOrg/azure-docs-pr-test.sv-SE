---
title: "aaaFind nästa hopp med Azure Network Watcher nästa hopp - REST | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan hitta vad hello nästa hopptyp är och ip-adressen med nästa hopp med hello Azure REST API"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2216c059-45ba-4214-8304-e56769b779a6
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: a2b61b355aae8ae513ebd44837184fbc6cfd668c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-aure-network-watcher-using-azure-rest-api"></a>Ta reda på vilka hello nästa hopptyp med hello nexthop-funktionen i Azure Nätverksbevakaren med hjälp av Azure REST API

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Azure REST-API](network-watcher-check-next-hop-rest.md)

Nästa hopp är en funktion i Nätverksbevakaren som ger hello möjlighet hämta hello nästa hopptyp och IP-adress baserat på en angiven virtuell dator. Den här funktionen är användbart för att fastställa om trafik som lämnar en virtuell dator som passerar en gateway, internet eller virtuella nätverk tooget tooits mål.

## <a name="before-you-begin"></a>Innan du börjar

ARMclient är används toocall hello REST-API med hjälp av PowerShell. ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.

## <a name="scenario"></a>Scenario

hello-scenario som beskrivs i den här artikeln används nästa hopp, en funktion i Nätverksbevakaren som söker efter hello nästa hopptyp och IP-adress för en resurs. Besök toolearn mer om nästa hopp [nästa hopp översikt](network-watcher-next-hop-overview.md).

I det här scenariot kommer du att:

* Hämta hello nästa hopp för en virtuell dator.

## <a name="log-in-with-armclient"></a>Logga in med ARMClient

Logga in tooarmclient med dina autentiseringsuppgifter för Azure.

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>Hämta en virtuell dator

Kör följande skript tooreturn hello en virtuell dator. Den här informationen behövs för att köra nästa hopp.

följande kod hello måste värden för hello följande variabler:

- **subscriptionId** -hello prenumerations-Id toouse.
- **resourceGroupName** - hello namn på en resursgrupp som innehåller virtuella datorer.

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

Från hello följande utdata, används hello id för hello virtuell dator i följande exempel hello:

```json
...
,
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="get-next-hop"></a>Hämta nästa hopp

När du har skapat hello authorization-huvud kan hello nästa hopp från en virtuell dator hämtas. hello måste följande värden ersättas för hello kod exempel toowork.

> [!Important]
> För nätverket Watcher REST API-anrop hello resursgruppens namn i hello Begärd URI är hello resursgruppen som innehåller hello Nätverksbevakaren, inte hello resurser som du utför hello diagnostiska åtgärder på.

```powershell
$sourceIP = "10.0.0.4"
$destinationIP = "8.8.8.8"
$resourceGroupName = <resource group name>
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'sourceIpAddress': '${sourceIP}',
    'destinationIpAddress': '${destinationIP}',
    'targetNicResourceId' : '${targetNic}'
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/nextHop?api-version=2016-12-01" $requestBody
```

> [!NOTE]
> Nästa hopp kräver att hello Virtuella datorresursen fördelas toorun.

## <a name="results"></a>Resultat

hello är följande fragment ett exempel på hello utdata togs emot. hello resulterar hello följande värden:

* **nextHopType** -värdet är ett av följande värden hello: Internet VirtualAppliance, VirtualNetworkGateway, VnetLocal, HyperNetGateway eller None.
* **nextHopIpAddress** -hello IP-adressen för hello nästa hopp.
* **routeTableId** - hello-värdet är antingen en uri för hello vägtabell associerad med hello väg, eller om ingen användardefinierad väg definierade hello värdet för *Systemväg* returneras.

hello följande finns hello resultat i json-format.

```json
{
  "nextHopType": "Internet",
  "routeTableId": "System Route"
}
```

## <a name="next-steps"></a>Nästa steg

När du har kan toofind ut hello nästa hopp för en virtuell dator kan du visa hello säkerheten för dina nätverksresurser genom att besöka [Säkerhetsvyn översikt](network-watcher-security-group-view-overview.md)














