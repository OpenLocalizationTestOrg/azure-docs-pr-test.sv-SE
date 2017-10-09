---
title: "aaaFind nästa hopp med Azure Network Watcher nexthop - Azure CLI 2.0 | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan hitta vad hello nästa hopptyp är och IP-adress med hjälp av nästa hopp med Azure CLI."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 0700c274-3e0d-4dca-acfa-3ceac8990613
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 77c2bde51274bd5c64e7a2467f95139af620ca30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-azure-cli-20"></a>Ta reda på vilka hello nästa hopptyp med hello nexthop-funktionen i Azure Nätverksbevakaren använder Azure CLI 2.0

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Azure REST-API](network-watcher-check-next-hop-rest.md)

Nästa hopp är en funktion i Nätverksbevakaren som ger hello möjlighet hämta hello nästa hopptyp och IP-adress baserat på en angiven virtuell dator. Den här funktionen är användbart för att fastställa om trafik som lämnar en virtuell dator som passerar en gateway, internet eller virtuella nätverk tooget tooits mål.

Den här artikeln använder våra nästa generations CLI för hello management resursdistributionsmodell, Azure CLI 2.0, som är tillgänglig för Windows, Mac och Linux.

tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Innan du börjar

I det här scenariot använder du hello Azure CLI toofind hello nästa hopptyp och IP-adress.

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren. hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.

## <a name="scenario"></a>Scenario

hello-scenario som beskrivs i den här artikeln används nästa hopp, en funktion i Nätverksbevakaren som söker efter hello nästa hopptyp och IP-adress för en resurs. Besök toolearn mer om nästa hopp [nästa hopp översikt](network-watcher-next-hop-overview.md).


## <a name="get-next-hop"></a>Hämta nästa hopp

tooget hello nästa hopp vi kallar hello `az network watcher show-next-hop` cmdlet. Vi skickar hello cmdlet hello Nätverksbevakaren resursgrupp, hello NetworkWatcher, virtuella Id, källans IP-adress och mål-IP-adress. I det här exemplet är IP-måladress hello tooa VM i ett annat virtuellt nätverk. Det finns en virtuell nätverksgateway mellan hello två virtuella nätverk.

Om du inte har gjort det ännu, installerar och konfigurerar hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login). Kör följande kommando hello:

```azurecli
az network watcher show-next-hop --resource-group <resourcegroupName> --vm <vmNameorID> --source-ip <source-ip> --dest-ip <destination-ip>

```

> [!NOTE]
Om hello VM har flera nätverkskort och IP-vidarebefordring är aktiverad på någon av hello nätverkskort, sedan hello NIC-parameter (-i nic-id) måste anges. Annars är valfria.

## <a name="review-results"></a>Granska resultaten

När du är färdig tillhandahålls hello resultat. hello nästa hopp IP-adress returneras samt hello typ av resurs som det är.

```azurecli
{
    "nextHopIpAddress": null,
    "nextHopType": "Internet",
    "routeTableId": "System Route"
}
```

hello visar följande lista hello tillgängliga NextHopType värden:

**Nästa Hopptyp**

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* Ingen

## <a name="next-steps"></a>Nästa steg

Lär dig hur tooreview säkerhet grupp nätverksinställningarna via programmering genom att besöka [NSG granskning med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md)
