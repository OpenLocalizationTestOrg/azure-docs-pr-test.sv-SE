---
title: "aaaPerformance överväganden för Azure Traffic Manager | Microsoft Docs"
description: "Förstå prestanda på Traffic Manager och hur tootest prestandan för din webbplats när du använder Traffic Manager"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 3ba5dfa1-2922-43f1-9a23-d06969c4a516
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: fd4e6cb221a2ceee63ec57237ee90fd714e91db8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performance-considerations-for-traffic-manager"></a>Prestandaöverväganden för Traffic Manager

Den här sidan förklarar prestandaöverväganden med Traffic Manager. Tänk hello följande scenario:

Du har instanser av webbplatsen i hello WestUS och EastAsia regioner. En av hello instanser misslyckas hello hälsokontrollen för hello traffic manager avsökningen. Programmet trafik är riktat toohello felfri region. Den här redundansen förväntas men prestanda kan vara ett problem utifrån hello svarstiden för hello trafik nu tooa avlägsna region.

## <a name="performance-considerations-for-traffic-manager"></a>Prestandaöverväganden för Traffic Manager

hello endast prestandapåverkan Traffic Manager kan ha på din webbplats är hello första DNS-sökning. En DNS-begäran för hello namnet på Traffic Manager-profilen hanteras av hello Microsoft DNS-rotserver värdar hello trafficmanager.net zonen. Traffic Manager fylls och regelbundet uppdaterar hello Microsoft DNS-rotservrar baserat på hello Traffic Manager-princip och hello avsökningen resultat. Så även under hello första DNS-sökning skickas ingen DNS-frågor tooTraffic Manager.

Traffic Manager består av flera komponenter: DNS-namn-servrar, en API-tjänsten, hello lagringsskikt och en slutpunkt övervakningstjänsten. Om en Traffic Manager-tjänstkomponent misslyckas, finns det ingen effekt på hello DNS-namnet som associeras med Traffic Manager-profilen. hello poster i hello Microsoft DNS-servrar ändras inte. Dock slutpunktsövervakning och DNS-uppdatering inte sker. Traffic Manager är därför inte kan tooupdate DNS toopoint tooyour växling vid fel plats när din primära plats kraschar.

DNS-namnmatchningen är snabb och resultat cachelagras. hello hastigheten på hello första DNS-sökning beror på hello DNS-servrar hello klienten använder för namnmatchning. En klient kan normalt slutföras en DNS-sökning i ~ 50 ms. hello resultaten av hello sökning cachelagras för hello varaktighet hello DNS-Time-to-live (TTL). hello standard TTL för Traffic Manager är 300 sekunder.

Trafiken flödar inte via Traffic Manager. När hello DNS-sökning är klar har hello-klienten en IP-adress för en instans av webbplatsen. hello klienten ansluter direkt toothat adress och klarar inte via Traffic Manager. hello Traffic Manager-princip som du väljer påverkas inte hello DNS-prestanda. Men kan en prestanda routning-metod påverka hello upplevelsen. Om din princip dirigerar trafik från Nordamerika tooan instansen finns i Asien, kan hello Nätverksfördröjningen för dessa sessioner vara ett prestandaproblem.

## <a name="measuring-traffic-manager-performance"></a>Mäter Traffic Manager-prestanda

Det finns flera webbplatser som du kan använda toounderstand hello prestanda och beteendet för en Traffic Manager-profilen. Många av dessa platser är gratis men kan ha begränsningar. Vissa webbplatser erbjuder förbättrad övervakning och rapportering för en avgift.

hello verktyg på dessa webbplatser mäta DNS svarstiderna och visa hello matcha IP-adresser för klientplatser runt hälsningsmeddelande. De flesta av dessa verktyg cachelagrar inte hello DNS-resultat. Därför visar hello verktyg hello fullständig DNS-sökning varje gång ett test körs. När du testar din egen klient prestanda du bara hello fullständig DNS-sökning en gång under hello TTL-värde.

## <a name="sample-tools-toomeasure-dns-performance"></a>Exempel verktyg toomeasure DNS-prestanda

* [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS erbjuder många prestandaverktyg. hello jämförelse av DNS-verktyget kan du se hur lång tid det tar tooresolve DNS-namn och hur som jämför tooother DNS-leverantörer.

* [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    En av hello enklaste verktyg är WebSitePulse. Ange hello URL toosee DNS-matchningstid, första byten, sista byten och andra prestandastatistik. Du kan välja mellan tre olika test-platser. I det här exemplet ser du att hello första körningen visar att DNS-sökning tar 0.204 sek.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Eftersom hello resultat cachelagras hello andra test för hello samma DNS-sökning för Traffic Manager-slutpunkten hello tar 0.002 sek.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

* [Kanada App syntetiska Övervakare](https://asm.ca.com/en/checkit.php)

    Kallades hello Watchmouse Kontrollera webbplats verktyget, platsen visar hello DNS-matchning tid från flera geografiska områden samtidigt. Ange hello URL toosee DNS-matchningstid, tid och hastighet från flera geografiska platser. Använd det här testet toosee vilka värdtjänsten returneras för olika platser runt hello world.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

* [Pingdom](http://tools.pingdom.com/)

    Det här verktyget innehåller prestandastatistik för varje element i en webbsida. hello sidan analys fliken visar hello procentandelen tid som krävs för DNS-sökning.

* [Vad är min DNS?](http://www.whatsmydns.net/)

    Den här platsen har en DNS-sökning från 20 olika platser och visar hello resultat på en karta.

* [Gräva webbgränssnitt](http://www.digwebinterface.com)

    Den här platsen visar mer detaljerad DNS-information, inklusive skapa CNAME-poster och A-poster. Kontrollera att du kontrollerar du hello 'Färga utdata' och 'statistik, under Alternativ och välj 'All' under Nameservers.

## <a name="next-steps"></a>Nästa steg

[Om Traffic Manager-trafikroutningsmetoder](traffic-manager-routing-methods.md)

[Testa inställningarna för Traffic Manager](traffic-manager-testing-settings.md)

[Åtgärder för Traffic Manager (REST API-referens)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Traffic Manager-Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=400769)

