---
title: "aaaQoS krav för ExpressRoute | Microsoft Docs"
description: "Den här sidan innehåller detaljerade krav för att konfigurera och hantera QoS för ExpressRoute-kretsar."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: db1c1447-0283-4a09-907b-ae481adc40c7
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: cherylmc
ms.openlocfilehash: 46cc81bd38ff50dd9e7a1bfdd0faa457ff7b2fa1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-qos-requirements"></a>QoS-krav för ExpressRoute
Skype för företag har olika arbetsbelastningar som kräver särskild QoS-behandling. Om du planerar tooconsume röst tjänster via ExpressRoute, bör du följa toohello krav som beskrivs nedan.

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> QoS-krav tillämpa toohello Microsoft endast peering. hello DSCP-värden i nätverkstrafiken tas emot på offentlig Azure-peering och privat Azure-peering kommer att återställa too0. 
> 
> 

hello innehåller följande tabell en lista över DSCP-markeringar som används av Skype för företag. Se för[hantera QoS för Skype för företag](https://technet.microsoft.com/library/gg405409.aspx) för mer information.

| **Trafikklass** | **Behandling (DSCP-markering)** | **Arbetsbelastningar i Skype för företag** |
| --- | --- | --- |
| **Röst** |EF (46) |Skype-/Lync-röst |
| **Interaktiv** |AF41 (34) |Video, VBSS |
| AF21 (18) |Appdelning | |
| **Standard** |AF11 (10) |Filöverföring |
| CS0 (0) |Annat | |

* Du bör klassificera hello arbetsbelastningar och markera hello rätt DSCP-värden. Följ riktlinjerna för hello [här](https://technet.microsoft.com/library/gg405409.aspx) om hur tooset DSCP-markeringar i nätverket.
* Du bör konfigurera och ge stöd för flera QoS-köer i nätverket. Röst måste vara en fristående klass och behandlas hello EF anges i RFC 3246. 
* Du kan bestämma hello queuing mekanism överbelastning princip och bandbreddsallokering per trafik klass. Men hello DSCP-markering för Skype för företag arbetsbelastningar måste bevaras. Om du använder DSCP-markeringar som inte listas här ovan, t.ex. AF31 (26), du måste skriva om too0 denna DSCP-värde innan du skickar hello paket tooMicrosoft. Microsoft skickar bara paket som markerats med hello DSCP-värde som visas i hello ovanför tabell. 

## <a name="next-steps"></a>Nästa steg
* Se toohello krav för [routning](expressroute-routing.md) och [NAT](expressroute-nat.md).
* Se hello följande länkar tooconfigure ExpressRoute-anslutning.
  
  * [Skapa en ExpressRoute-krets](expressroute-howto-circuit-classic.md)
  * [Konfigurera routning](expressroute-howto-routing-classic.md)
  * [Länka ett VNet tooan ExpressRoute-krets](expressroute-howto-linkvnet-classic.md)

