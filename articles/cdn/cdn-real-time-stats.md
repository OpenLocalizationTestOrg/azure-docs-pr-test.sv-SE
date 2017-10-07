---
title: aaaReal statistik i Azure CDN | Microsoft Docs
description: "Realtid statistik innehåller data i realtid om Azure CDN hello prestanda när du levererar innehåll tooyour klienter."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: c7989340-1172-4315-acbb-186ba34dd52a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 68900a5092b767e45c1fdf9cef2cd03f55f38a6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-stats-in-microsoft-azure-cdn"></a>Realtid statistik i Microsoft Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Översikt
Det här dokumentet förklarar realtid statistik i Microsoft Azure CDN.  Den här funktionen ger realtidsdata, såsom bandbredd, cache status och samtidiga anslutningar tooyour CDN-profilen när du levererar innehåll tooyour klienter. Detta gör att kontinuerlig övervakning av hello hälsotillståndet för din tjänst när som helst, inklusive go live-händelser.

följande diagram hello är tillgängliga:

* [Bandbredd](#bandwidth)
* [Statuskoder](#status-codes)
* [Cache-status](#cache-statuses)
* [Anslutningar](#connections)

## <a name="accessing-real-time-stats"></a>Åtkomst till realtid statistik
1. I hello [Azure Portal](https://portal.azure.com), bläddra tooyour CDN-profilen.
   
    ![CDN-profilbladet](./media/cdn-real-time-stats/cdn-profile-blade.png)
2. Klicka på hello hello CDN-profilbladet **hantera** knappen.
   
    ![CDN-profilbladet hantera knappen](./media/cdn-real-time-stats/cdn-manage-btn.png)
   
    hello CDN-hanteringsportalen öppnas.
3. Hovra över hello **Analytics** fliken och sedan hovra över hello **realtid Stats** utfällbar.  Klicka på **HTTP stort objekt**.
   
    ![CDN-hanteringsportalen](./media/cdn-real-time-stats/cdn-premium-portal.png)
   
    hello realtid stats diagram visas.

Var och en av hello diagram visar realtid statistik för hello valda tidsintervallet startar när hello sidan läses in.  hello diagram uppdateras automatiskt varje några sekunder.  Hej **uppdatera diagrammet** knappen, om den finns raderar hello diagram, efter vilken visas endast hello valda data.

## <a name="bandwidth"></a>Bandbredd
![Bandbredd-diagram](./media/cdn-real-time-stats/cdn-bandwidth.png)

Hej **bandbredd** diagram som visar hello mängden bandbredd som används för hello aktuella plattformen över hello valda tidsintervallet. hello skuggade delen av diagrammet hello anger bandbreddsanvändning. hello exakta mängden bandbredd som används visas direkt under hello linjediagram.

## <a name="status-codes"></a>Statuskoder
![Diagram för status-kod](./media/cdn-real-time-stats/cdn-status-codes.png)

Hej **statuskoder** diagram visar hur ofta vissa HTTP-svarskoder sker via hello valda tidsintervallet.

> [!TIP]
> En beskrivning av varje alternativ för HTTP-status koden finns [Azure CDN HTTP-statuskoder](https://msdn.microsoft.com/library/mt759238.aspx).
> 
> 

En lista över HTTP-statuskoder visas direkt ovanför hello diagram. Den här listan visar varje statuskod som kan ingå i hello linjediagram och hello aktuellt antal händelser per sekund för denna statuskod. Som standard visas en rad för var och en av dessa statuskoder i hello graph. Du kan dock välja tooonly övervakaren hello statuskoder som har särskild betydelse för din CDN-konfiguration. toodo, markera önskad hello-statuskoder och avmarkera alla andra alternativ och klicka på **uppdatera diagrammet**. 

Du kan dölja loggdata för en viss statuskod tillfälligt.  Klicka på hello statuskod som du vill toohide hello förklaringen direkt under hello diagram. hello-statuskoden ska vara dolda direkt från hello diagram. Om du klickar på den statuskoden igen kommer att alternativet toobe visas igen.

## <a name="cache-statuses"></a>Cache-status
![Diagram för cache-status](./media/cdn-real-time-stats/cdn-cache-status.png)

Hej **Cache statusar** diagram visar hur ofta vissa typer av cache-status sker via hello valda tidsintervallet. 

> [!TIP]
> En beskrivning av varje cache status kod alternativ finns [Azure CDN Cache-statuskoder](https://msdn.microsoft.com/library/mt759237.aspx).
> 
> 

En lista över cache statuskoder visas direkt ovanför hello diagram. Den här listan visar varje statuskod som kan ingå i hello linjediagram och hello aktuellt antal händelser per sekund för denna statuskod. Som standard visas en rad för var och en av dessa statuskoder i hello graph. Du kan dock välja tooonly övervakaren hello statuskoder som har särskild betydelse för din CDN-konfiguration. toodo, markera önskad hello-statuskoder och avmarkera alla andra alternativ och klicka på **uppdatera diagrammet**. 

Du kan dölja loggdata för en viss statuskod tillfälligt.  Klicka på hello statuskod som du vill toohide hello förklaringen direkt under hello diagram. hello-statuskoden ska vara dolda direkt från hello diagram. Om du klickar på den statuskoden igen kommer att alternativet toobe visas igen.

## <a name="connections"></a>Anslutningar
![Anslutningar diagram](./media/cdn-real-time-stats/cdn-connections.png)

Det här diagrammet visar hur många anslutningar har etablerat tooyour kant-servrar. Varje begäran för en tillgång som passerar genom våra CDN resultat i en anslutning.

## <a name="next-steps"></a>Nästa steg
* Håll dig informerad med [aviseringar i realtid i Azure CDN](cdn-real-time-alerts.md)
* Gräva djupare med [avancerade http-rapporter](cdn-advanced-http-reports.md)
* Analysera [användningsmönster](cdn-analyze-usage-patterns.md)

