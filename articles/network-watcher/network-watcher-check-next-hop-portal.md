---
title: "aaaFind nästa hopp med Azure Network Watcher nexthop - Azure-portalen | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan hitta vad hello nästa hopptyp är och ip-adressen med nästa hopp med hello Azure-portalen"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 7b459dcf-4077-424e-a774-f7bfa34c5975
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b64a2a5275c15aa8bdb10601de4ae1504a9ab551
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-hello-portal"></a>Ta reda på vilka hello nästa hopptyp med hello nästa hopp-funktionen i Azure Nätverksbevakaren med hello-portalen

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Azure REST-API](network-watcher-check-next-hop-rest.md)

Nästa hopp är en funktion i Nätverksbevakaren som ger hello möjlighet hämta hello nästa hopptyp och IP-adress baserat på en angiven virtuell dator. Den här funktionen är användbart för att fastställa om trafik som lämnar en virtuell dator som passerar en gateway, internet eller virtuella nätverk tooget tooits mål.

## <a name="before-you-begin"></a>Innan du börjar

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren. hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.

## <a name="scenario"></a>Scenario

hello-scenario som beskrivs i den här artikeln används nästa hopp toofind hello nästa hopptyp och IP-adress för en resurs. Besök toolearn mer om nästa hopp [nästa hopp översikt](network-watcher-next-hop-overview.md).

I det här scenariot kommer du att:

* Hämta hello nästa hopp från en virtuell dator.

## <a name="get-next-hop"></a>Hämta nästa hopp

### <a name="step-1"></a>Steg 1

Navigera tooyour Nätverksbevakaren resurs i hello Azure-portalen.

### <a name="step-2"></a>Steg 2

Klicka på **nästa hopp** i hello navigeringsfönstret, Välj hello virtuell dator och nätverksgränssnitt, fyller du i hello källa och mål-IP och klicka på hello **nästa hopp** knappen.

> [!NOTE]
> Nästa hopp kräver att hello Virtuella datorresursen fördelas toorun.

![Hämta nästa hopp-översikt][1]

### <a name="step-3"></a>Steg 3

När hello åtgärden är klar, tillhandahålls hello resultat. Hej IP-adress och typ av enhet hello nästa hopp är visas. hello följande tabell visar hello tillgängliga returnerade värden i hello-portalen.

**Nästa Hopptyp**

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* Ingen

Om en anpassad väg har använt tooroute visas den här trafiken hello användardefinierad väg (UDR) samt med hello resultat.

![Nästa hopp-resultat][2]

## <a name="next-steps"></a>Nästa steg

Lär dig hur tooreview säkerhet grupp nätverksinställningarna via programmering genom att besöka [NSG granskning med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md)

[1]: ./media/network-watcher-check-next-hop-portal/figure1.png
[2]: ./media/network-watcher-check-next-hop-portal/figure2.png














