---
title: "information om aaaSizing för ett VNET i Azure RemoteApp | Microsoft Docs"
description: "Lär dig mer om krav för Azure RemoteApp körs med ett VNET-hello IP-adress"
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
ms.openlocfilehash: f98b831af32c41740b258d122b3e18765be08d97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a>Storleksinformation för ett VNET i Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

När du använder Azure RemoteApp med ett virtuellt nätverk (VNET) använder RemoteApp IP-adresser inom hello undernät. Baserat på hello skalan för RemoteApp-tjänsten måste tooensure att ditt undernät har tillräckligt med IP-adresser som är tillgängliga för RemoteApp-virtuella datorer. Den här vägledningen storlek är inte perfekt baserat på hur RemoteApp dynamiskt startar och roterar av virtuella datorer i en samling, hjälper den dig att beräkna undernätets intervall. Detta är särskilt viktigt eftersom när en RemoteApp-tjänsten är placerad i ett VNET, kan du öka hello undernätets storlek utan att ta bort RemoteApp.

Du bör ha 100 IP-adresser som är tillgängliga för varje RemoteApp-samling som du vill toorun med högsta kapacitet. Om du har en RemoteApp-samlingen i hello standardplan och du vill toohave hello maximalt 500 användare, bör du ha 100 IP-adresser för samlingen. Dessutom måste 100 IP-adresser för en RemoteApp-samling i hello grundläggande planen som har 800 användare. Om du planerar toohave färre användare (mindre än hello maximala), kan du minska hello IP-adresser som behövs per samling. hello minsta undernät kravet är 30 IP-adresser (/ 27).

Kolla hello följande information toomake att ditt virtuella nätverk har konfigurerats och fungerar propertly:

* [Migrera från en personlig VNET tooan Azure VNET](remoteapp-migratevnet.md)
* [Validera hello Azure VNET toouse med Azure RemoteApp](remoteapp-vnet.md)

