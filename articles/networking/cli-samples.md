---
title: aaaAzure CLI prover | Microsoft Docs
description: Azure CLI-exempel
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: kumud
ms.openlocfilehash: 8001b7e72480cfd0122325f7fb81c32aaad072d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-networking"></a>Azure CLI-exempel för nätverk

hello innehåller följande tabell länkar toobash skript som skapats med hello Azure CLI.

| | |
|-|-|
|**Anslutning mellan Azure-resurser**||
| [Skapa ett virtuellt nätverk för flera nivåer program](./scripts/virtual-network-cli-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | Skapar ett virtuellt nätverk med frontend och backend-undernät. Trafik toohello frontend undernät är begränsad tooHTTP och SSH, medan trafik toohello backend-undernät är begränsad tooMySQL, port 3306. |
| [Peer-två virtuella nätverk](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | Skapar och ansluter två virtuella nätverk i hello samma region. |
| [Vidarebefordra trafik via en virtuell nätverksenhet](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | Skapar ett virtuellt nätverk med frontend och backend-undernät och en virtuell dator som kan tooroute trafik mellan hello två undernät. |
| [Filtrera inkommande och utgående nätverkstrafik för VM](./scripts/virtual-network-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | Skapar ett virtuellt nätverk med frontend och backend-undernät. Inkommande nätverkstrafik toohello frontend undernät är begränsad tooHTTP, HTTPS och SSH. Utgående trafik toohello Internet från hello backend-undernät är inte tillåtet. |
|**Läsa in belastningsutjämning och trafik riktning**||
| [Läsa in belastningsutjämna trafik tooVMs för hög tillgänglighet](./scripts/load-balancer-linux-cli-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | Skapar flera virtuella datorer i en hög tillgänglighet och konfiguration för Utjämning av nätverksbelastning. |
| [Belastningsutjämning för flera webbplatser på virtuella datorer](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | Skapar två virtuella datorer med flera IP-konfigurationer, domänanslutna tooan Azure Tillgänglighetsuppsättning, tillgängligt via en Azure belastningsutjämnare. |
| [Direkt-trafik över flera regioner för hög tillgänglighet](./scripts/traffic-manager-cli-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  Skapar två apptjänstplaner, två webbappar, en traffic manager-profil och två traffic manager-slutpunkter. |
| | |
