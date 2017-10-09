---
title: "aaaIntroduction tootopology i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över hello Nätverksbevakaren topologi funktioner"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e753a435-38e0-482b-846b-121cb547555c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 7fa1c5518e4a25a5db999d898a9ee19fd0121db7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tootopology-in-azure-network-watcher"></a>Introduktion tootopology i Azure Nätverksbevakaren

Topologi returnerar ett diagram över nätverksresurser i ett virtuellt nätverk. hello diagrammet visar hello sambandet mellan hello resurser toorepresent hello slutet tooend nätverksanslutning.

![Översikt över labbtopologi][1]

I hello portal returnerar topologi hello resursobjekt på en per virtuellt nätverk basis. hello relationer illustreras med linjer mellan hello resurser resurser utanför hello Nätverksbevakaren region, även om hello resurs grupp inte ska visas. hello resurser returneras i hello portal-vy är en delmängd av hello nätverkskomponenter som är visas i diagram registreringen. toosee hello fullständig lista över nätverksresurser som du kan använda [PowerShell](network-watcher-topology-powershell.md) eller [REST](network-watcher-topology-rest.md)

> [!NOTE]
> En instans av Nätverksbevakaren krävs i varje region som du vill ha toorun topologi.

Som resurser returneras hello anslutning mellan dem modelleras under två relationer.

- **Inneslutning** -exempel: virtuellt nätverk innehåller ett undernät som innehåller ett nätverkskort
- **Associerade** -exempel: ett nätverkskort är kopplat till en virtuell dator

### <a name="next-steps"></a>Nästa steg

Lär dig hur toouse PowerShell tooretrieve hello topologi visa genom att besöka [Nätverksbevakaren topologi med PowerShell](network-watcher-topology-powershell.md)

<!--Image references-->

[1]: ./media/network-watcher-topology-overview/topology.png
