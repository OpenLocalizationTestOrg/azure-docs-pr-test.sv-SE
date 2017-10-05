---
title: "Storleksinformation för ett VNET i Azure RemoteApp | Microsoft Docs"
description: "Lär dig mer om krav för IP-adress för Azure RemoteApp körs med ett virtuellt nätverk"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b6e1c4ba-0236-42b2-bced-69353bf211be
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 9375981db64ec4a1ae523e958423b5f5787cec33
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a>Storleksinformation för ett VNET i Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

När du använder Azure RemoteApp med ett virtuellt nätverk (VNET) använder RemoteApp IP-adresser i undernätet. Baserat på skalan för RemoteApp-tjänsten, som du behöver se till att ditt undernät har tillräckligt med IP-adresser som är tillgängliga för RemoteApp-virtuella datorer. Den här vägledningen storlek är inte perfekt baserat på hur RemoteApp dynamiskt startar och roterar av virtuella datorer i en samling, hjälper den dig att beräkna undernätets intervall. Detta är särskilt viktigt eftersom när en RemoteApp-tjänsten är placerad i ett VNET, kan du öka undernätets storlek utan att ta bort RemoteApp.

Du bör ha 100 IP-adresser som är tillgängliga för varje RemoteApp-samling som du vill köra med högsta kapacitet. Om du har en RemoteApp-samlingen i Standard-plan och du vill ha maximal 500 användare, bör du ha 100 IP-adresser för samlingen. Dessutom behöver du 100 IP-adresser för RemoteApp-samlingen i den grundläggande planen som har 800 användare. Om du planerar att ha färre användare (mindre än maximalt) kan du minska IP-adresser som behövs per samling. Kravet på minsta undernät är 30 IP-adresser (/ 27).

Checka ut följande information för att kontrollera att ditt virtuella nätverk är konfigurerad och igång propertly:

* [Migrera från ett personligt virtuellt nätverk till ett Azure VNET](remoteapp-migratevnet.md)
* [Verifiera Azure VNET att använda med Azure RemoteApp](remoteapp-vnet.md)

