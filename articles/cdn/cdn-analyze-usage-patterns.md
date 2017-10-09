---
title: "aaaAnalyze Azure CDN användningsmönster | Microsoft Docs"
description: "Du kan visa användningsmönster för din CDN med hello följande rapporter: bandbredd, överförda Data, träffar, Cache status, Cache träffar förhållandet, IPV4/IPv6-Data överförs."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 5a0d9018-8bdb-48ff-84df-23648ebcf763
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d27e6f60acaed66abb27d860c3a3e2e81c9f60cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Analysera användningsmönster Azure CDN

[!INCLUDE[cdn-verizon-only](../../includes/cdn-verizon-only.md)]

hello guiden nedan går igenom hello steg tooview hello core rapporter via hello hantera portal för Verizon profiler. Du kan också exportera core analytics data toostorage, händelsehubb eller logganalys (oms) för både Verizon och Akamai profiler [hello azure-portalen](cdn-log-analysis.md).

Du kan visa användningsmönster för din CDN med hello följande rapporter:

* Bandbredd
* Data som överförs
* Träffar
* Cache-status
* Träffgrad för cache
* IPv4/IPv6-Data som överförs

## <a name="accessing-core-reports"></a>Åtkomst till Core rapporter
1. Klicka på hello hello CDN-profilbladet **hantera** knappen.
   
    ![CDN-profilbladet hantera knappen](./media/cdn-reports/cdn-manage-btn.png)
   
    hello CDN-hanteringsportalen öppnas.
2. Hovra över hello **Analytics** fliken och sedan hovra över hello **Core rapporter** utfällbar.  Klicka på hello önskade rapporten i hello-menyn.
   
    ![CDN-hanteringsportalen - Core rapporter-menyn](./media/cdn-reports/cdn-core-reports.png)

## <a name="bandwidth"></a>Bandbredd
hello bandbredd rapport består av ett diagram och en datatabell som visar hello bandbreddsanvändning för HTTP och HTTPS under en viss tidsperiod. Du kan visa hello bandbreddsanvändning över alla CDN POP eller en viss POP. Detta gör du tooview hello trafik toppar och distribution över CDN POP i Mbit/s.

* Välj alla Edge noder toosee trafik från alla noder eller välj en specifik region/nod hello listrutan.
* Välj datum intervallet tooview data för dagens denna vecka/denna månad, etc. eller ange anpassade datum och klicka på ”Gå” toomake att valet uppdateras.
* Du kan exportera och hämta hello data genom att klicka på hello excel-blad ikonen som visas bredvid för ”gå”.

hello rapporten uppdateras var femte minut.

![Bandbredd-rapport](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Data som överförs
Den här rapporten innehåller ett diagram och en datatabell som visar hello trafik användning för HTTP och HTTPS under en viss tidsperiod. Du kan visa hello trafik användning över alla CDN POP eller en viss POP. Detta gör du tooview hello trafik toppar och distribution över CDN POP i GB.

* Välj alla Edge noder toosee trafik från alla anteckningar eller välj en specifik region/nod hello listrutan.
* Välj datum intervallet tooview data för dagens denna vecka/denna månad, etc. eller ange anpassade datum och klicka på ”Gå” toomake att valet uppdateras.
* Du kan exportera och hämta hello data genom att klicka på hello excel-blad ikonen som visas bredvid för ”gå”.

hello rapporten uppdateras var femte minut.

![Överförda data rapporten](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Träffar (statuskoder)
Den här rapporten beskrivs hello distribution av begäran statuskoder för ditt innehåll. Varje förfrågan om innehållet genererar en HTTP-statuskod. hello statuskod beskriver hur edge POP hanteras hello-begäran. Till exempel ange 2xx statuskoder hello begäran behandlades har tooa klienten medan en 4xx statuskod indikerar ett fel inträffade. Mer information om HTTP-statuskod finns [statuskoder](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

* Välj datum intervallet tooview data för dagens denna vecka/denna månad, etc. eller ange anpassade datum och klicka på ”Gå” toomake att valet uppdateras.
* Du kan exportera och hämta hello data genom att klicka på hello excel-blad finns bredvid för ”gå”.

![Träffar rapport](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Status för cachelagring
Den här rapporten beskrivs hello distribution av träffar och cachemissar för klientbegäran. Eftersom hello bästa prestanda kommer från cacheträffar, kan du optimera data leverans hastigheter genom att minimera cachemissar och cacheträffar på har upphört att gälla. Cachemissar kan minskas genom att konfigurera din ursprung server tooavoid tilldela ”no-cache” svarshuvuden, undvika frågesträng cachelagring utom där det är absolut nödvändigt och undvika icke Cacheable ställs svarskoder. Upphört att gälla cache träffar kan undvikas genom att göra en tillgång max-ålder så länge som möjligt toominimize hello antal begäranden toohello ursprungsservern.

![Rapport för cache-status](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Huvud-cache statusvärden är:
* TCP_HIT: Hanteras från kant. hello objektet var i cachen och inte har överskridit sin maximal ålder.
* TCP_MISS: Hanteras från ursprunget. hello objektet var inte i cachen och hello svaret var tillbaka tooorigin.
* TCP_EXPIRED _MISS: hanteras från ursprunget efter Omverifiering med ursprung. hello-objektet är i cacheminnet men har överskridit sin maximal ålder. En validering med ursprung resulterade i hello cacheobjektet ersättas med ett nytt svar från ursprunget.
* TCP_EXPIRED _HIT: hanteras från kant efter Omverifiering med ursprung. hello-objektet är i cacheminnet men har överskridit sin maximal ålder. En validering med hello ursprungsservern resulterade i hello cache-objekt som ska ändras.
* Välj datum intervallet tooview data för dagens denna vecka/denna månad, etc. eller ange anpassade datum och klicka på ”Gå” toomake att valet uppdateras.
* Du kan exportera och hämta hello data genom att klicka på hello excel-blad ikonen som visas bredvid för ”gå”.

### <a name="full-list-of-cache-statuses"></a>Fullständig lista över cache-status
* TCP_HIT – denna status rapporteras när en begäran hämtas direkt från hello POP toohello klienten. En tillgång hanteras direkt från en POP när cachelagras på hello POP närmaste toohello klient och den har en giltig time to live eller TTL-värde. TTL bestäms av hello följande svarshuvuden:
  
  * Cache-Control: s-maxage
  * Cache-Control: maximal ålder för
  * Upphör att gälla
* TCP_MISS – denna status anger att en cachelagrad version av hello begärda tillgången inte hittades på hello POP närmaste toohello klienten. hello tillgången kommer att begäras från en ursprungsserver eller en shield ursprungsservern. Om hello ursprungsservern eller hello ursprungsservern shield returnerar en tillgång, kommer de hanteras toohello klienten och cachelagras på både hello klient- och hello kant. Annars statuskod-200 (t.ex. 403 tillåts inte, det gick inte att hitta 404, etc.) ska returneras.
* TCP_EXPIRED _HIT – denna status rapporteras när en begäran som mål för en tillgång med en utgångna TTL-värde, till exempel när hello tillgången max-ålder har upphört att gälla, behandlades direkt från hello POP toohello klienten.
  
    En utgången begäran vanligtvis resulterar i en Omverifiering begäran toohello ursprungsservern. För en TCP_EXPIRED _HIT toooccur ange hello ursprungsserver att en nyare version av hello tillgångsinformation inte finns. Den här typen av situation uppdateras vanligtvis den tillgången Cache-Control och Expires-huvuden.
* TCP_EXPIRED _MISS – denna status rapporteras när en nyare version av en utgången cachelagrade tillgång hanteras från hello POP toohello klient. Detta inträffar när hello TTL-värde för den cachelagra resursen har upphört att gälla (t.ex. har upphört att gälla maximal ålder) och hello ursprungsservern returnerar en nyare version av tillgången. Den här nya versionen av hello tillgång hanteras toohello klienten i stället för hello cachelagrade versionen. Dessutom cachelagras den hello edge-server och hello-klienten.
* CONFIG_NOCACHE – denna status anger att en kund konfiguration på vår edge POP hindras hello tillgångsinformation från att cachelagras.
* Inget – denna status anger att en kontroll av innehåll dokumentens inte utfördes.
* TCP_ CLIENT_REFRESH _MISS – denna status rapporteras när HTTP-klienter (till exempel webbläsare) tvingar en kant POP tooretrieve en ny version av en inaktuella tillgång från hello ursprungsservern.
  
    Som standard förhindra våra servrar att en HTTP-klient tvinga vår edge servrar tooretrieve en ny version av hello tillgång från hello ursprungsservern.
* TCP_ PARTIAL_HIT – denna status rapporteras när en byteintervallbegäran resulterar i en träff för en delvis cachelagrade tillgång. hello begärda byte-intervallet hanteras direkt från hello POP toohello klient.
* UNCACHEABLE – denna status rapporteras när en tillgång Cache-Control och Expires-huvuden anger att det inte ska cachelagras på en POP eller av hello HTTP-klienten. Dessa typer av begäranden hanteras från hello ursprungsservern

## <a name="cache-hit-ratio"></a>Träffgrad för cache
Den här rapporten visar hello procentandelen cachelagrade begäranden som har hanteras direkt från cachen.

hello rapporten innehåller hello följande information:

* hello begärt innehåll har cachelagrats på hello POP närmaste toohello beställaren.
* hello begäran behandlades direkt från hello kanten av vårt nätverk.
* hello begäran kräver inte Omverifiering med hello ursprungsservern.

hello rapporten innehåller inte:

* Begäranden som nekats på grund av toocountry filtreringsalternativ.
* Begäranden om tillgångar vars huvuden tyda på att de inte ska cachelagras. Till exempel Cache-Control: privat Cache-Control: no-cache eller Pragma: no-cache-huvuden förhindrar att en tillgång cachelagras.
* Byte range-begäran för delvis cachelagrat innehåll.

hello formeln är: (TCP_ träffar / (TCP_ träffar + TCP_MISS)) * 100

* Välj datum intervallet tooview data för dagens denna vecka/denna månad, etc. eller ange anpassade datum och klicka på ”Gå” toomake att valet uppdateras.
* Du kan exportera och hämta hello data genom att klicka på hello excel-blad ikonen som visas bredvid för ”gå”.

![Antal träffar i rapport](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>IPv4/IPv6-Data som överförs
Den här rapporten visar hello trafikfördelning för användning i IPV4 eller IPV6.

![IPv4/IPv6-Data som överförs](./media/cdn-reports/cdn-ipv4-ipv6.png)

* Välj datum intervallet tooview data för dagens denna vecka/denna månad, etc. eller ange anpassade datum.
* Klicka på ”Gå” toomake att valet uppdateras.

## <a name="considerations"></a>Överväganden
Rapporter kan endast genereras inom hello senaste 18 månader.

