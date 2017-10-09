---
title: "aaaIntroduction toonext hopp i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över hello Nätverksbevakaren nästa hopp-funktion"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 916af736d0d52abadeafed746f0f8a0173b11033
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toonext-hop-in-azure-network-watcher"></a>Introduktion toonext hopp i Azure Nätverksbevakaren

Trafik från en virtuell dator skickas tooa mål baserat på hello effektiva vägar som är associerade med ett nätverkskort. Nästa hopp hämtar hello nästa hopptyp och IP-adressen för ett paket från en specifik virtuell dator och nätverkskort. Detta hjälper toodetermine om hello paketet tas dirigerad toohello mål eller är hello trafik som svart holed. En felaktig konfiguration av vägar av hello användare, där en trafik är riktat tooan lokal plats eller en virtuell installation, kan leda tooconnectivity problem. Nästa hopp returnerar också hello vägtabell associerad med hello nästa hopp. När du frågar ett nexthop om hello flödet definieras som en användardefinierad väg, returneras den rutten. Annars returnerar nästa hopp ”Systemväg”.

![Nästa hopp-översikt][1]

hello följer en lista över hello nästa hopptyper som kan returneras när du frågar nästa hopp.

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* Ingen

### <a name="next-steps"></a>Nästa steg

Lär dig hur toouse nästa hopp toofind problem med nätverksanslutningen genom att besöka [Kontrollera hello nästa hopp på en virtuell dator](network-watcher-check-next-hop-portal.md)

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png













