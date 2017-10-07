---
title: "aaaAnalyze gränsnodprestanda i Azure CDN | Microsoft Docs"
description: "Analysera gränsnodprestanda i Microsoft Azure CDN. Edge prestanda Analytics ger detaljerad information trafik och bandbredd användning för hello CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 8cc596a7-3e01-4f76-af7b-a05a1421517e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 35361d1c5e27fc6b8536c29e33c2ed217ee4d17e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Analysera gränsnodsprestanda i Microsoft Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Översikt
Edge prestanda analytics ger detaljerad information trafik och bandbredd användning för hello CDN. Den här informationen kan sedan använda toogenerate trender statistik, som gör att du toogain information om hur dina tillgångar som cachelagrade och levererat tooyour klienter. Detta gör i sin tur tooform en strategi för hur toooptimize hello överföringen av innehåll och toodetermine vilka problem ska åtgärdas toobetter utnyttjar hello CDN. Därför kan inte bara för att kunna tooimprove data leverans prestanda, men du kommer även att kunna tooreduce CDN-kostnader.

> [!NOTE]
> Alla rapporter använder UTC/GMT-notering när du anger ett datum/tid.
> 
> 

## <a name="reports-and-log-collection"></a>Rapporter och Logginsamling
CDN-aktivitet måste samlas in av hello Edge prestanda Analytics modulen innan den kan även generera rapporter på den. Samlingen processen när en dag och den täcker hello-aktivitet som utfördes under hello föregående dag. Detta innebär att en rapport statistiken representerar ett exempel på hello dag statistik för närvarande hello den bearbetades och inte nödvändigtvis innehålla hello fullständig uppsättning data för hello aktuell dag. de här rapporterna hello huvudfunktion är tooassess prestanda. De bör inte användas för fakturering eller exakta numeriska statistik.

> [!NOTE]
> hello rådata som Edge prestanda analysrapporter genereras är tillgänglig för minst 90 dagar.
> 
> 

## <a name="dashboard"></a>Instrumentpanel
hello Edge prestanda instrumentpanelen spårar aktuell och historisk CDN-trafik via ett diagram och statistik. Använd den här instrumentpanelen toodetect senaste och långsiktig trender på hello prestanda för CDN-trafik för ditt konto.

Den här instrumentpanelen består av:

* Ett interaktivt diagram som tillåter hello visualisering av nyckelvärden och trender.
* En tidslinje som ger en uppfattning om långsiktiga mönster för nyckelvärden och trender.
* Nyckelvärden och statistisk information om hur våra CDN-nätverkets förbättrar trafiken mätt av övergripande prestanda, användning och effektivitet.

### <a name="accessing-hello-edge-performance-dashboard"></a>Åtkomst till hello edge prestanda instrumentpanelen
1. Klicka på hello hello CDN-profilbladet **hantera** knappen.
   
    ![CDN-profilbladet hantera knappen](./media/cdn-edge-performance/cdn-manage-btn.png)
   
    hello CDN-hanteringsportalen öppnas.
2. Hovra över hello **Analytics** fliken och sedan hovra över hello **kant prestanda långsammare Analytics** utfällbar.  Klicka på **instrumentpanelen**.
   
    instrumentpanelen för hello edge nod visas.

### <a name="chart"></a>Diagrammet
hello instrumentpanelen innehåller ett diagram som visar ett mått över hello hello tidslinjen som visas direkt under den valda tidsperioden.  En tidslinje diagram in toohello sista två år CDN aktivitet visas direkt under hello diagrammet.

#### <a name="using-hello-chart"></a>Med hjälp av hello diagram
* Som standard diagrammet hello effektivitet cachelagrade för hello senaste 30 dagarna.
* Det här diagrammet genereras från data som sammanställs dagligen.
* Hovra över en dag på hello linjediagram visar hello måttet datum och hello värdet på det datumet.
* Klicka på Markera helger tootoggle övertäckning lätta grå lodräta staplar som representerar helger i hello schema. Den här typen av överlägget är användbart för att identifiera trafikmönster över helger.
* Klicka på Visa ett år sedan tootoggle övertäckning hello föregående årets aktiviteten över hello samma tidsperiod i hello schema. Den här typen av jämförelse ger inblick i långsiktiga CDN användningsmönster. hello övre högra hörnet av hello diagram innehåller en förklaring som visar hello färgkod för varje linjediagram.

#### <a name="updating-hello-chart"></a>Uppdatera hello diagram
* Tidsintervall: Utför en av följande hello:
  * Välj önskad region för hello hello tidslinjen. hello diagrammet uppdateras med data som motsvarar toohello tidsperiod.
  * Dubbelklicka på hello diagram toodisplay alla tillgängliga historiska data in tooa högst två år.
* Mått: Klicka på hello diagram ikonen som visas nästa toohello önskade mått. hello diagram och hello tidslinjen uppdateras med data för hello motsvarande mått.

### <a name="key-metrics-and-statistics"></a>Viktiga mått och statistik
#### <a name="efficiency-metrics"></a>Effektivitet mått
hello syftet med de här måtten är toosee om cachens effektivitet kan förbättras. hello huvudsakliga fördelar cachens effektivitet är:

* Minskad belastningen på hello ursprungsservern vilket kan leda till:
  * Bättre prestanda för web-server.
  * Minskade driftskostnader.
* Bättre data leverans acceleration eftersom fler begäranden hanteras direkt från hello CDN.

| Fält | Beskrivning |
| --- | --- |
| Cachens effektivitet |Anger hello procentandel av data som överförs som skickades från cachen. Detta mått åtgärder när en cachelagrad version av hello begärt innehåll behandlades direkt från hello CDN (kant servrar) toorequesters (t.ex. webbläsare) |
| Antal träffa i skriptmotorcache |Anger hello procentandelen förfrågningar som har sett från cachen. Detta mått åtgärder när en cachelagrad version av hello begärt innehåll behandlades direkt från hello CDN (kant servrar) toorequesters (t.ex. webbläsare). |
| % av Remote byte - ingen Cache-konfiguration |Anger hello procentandelen trafik som skickades från ursprung servrar toohello CDN (kant-servrar) som inte cachelagras på grund av hello kringgå cachefunktionen (HTTP-regelmotor). |
| % av Remote byte - Cache har upphört att gälla |Anger hello procentandelen trafik som skickades från ursprung servrar toohello CDN (kant servrar) på grund av inaktuella Omverifiering av innehåll. |

#### <a name="usage-metrics"></a>Användningsstatistik
hello syftet med de här måtten är tooprovide inblick i hello följande kostnaden skärande åtgärder:

* Minimera driftskostnaderna via hello CDN.
* Minska omkostnader CDN genom cachens effektivitet och komprimering.

> [!NOTE]
> Trafik volymen siffrorna representerar trafik som användes vid beräkning av förhållandet och procenttal och kan bara visa en del av hello totala trafiken för högvolyms-kunder.
> 
> 

| Fält | Beskrivning |
| --- | --- |
| Ave byte ut |Anger hello genomsnittliga antalet byte som överförs för varje begäran från hello CDN (kant servrar) toohello beställaren (t.ex. webbläsare). |
| Inga cachelagrade Config-Byte |Anger hello procentandelen trafik från hello CDN (kant servrar) toohello beställaren (t.ex. webbläsare) som inte cachelagras på grund av toohello kringgå cachefunktionen. |
| Komprimerade Byte hastighet |Anger hello procentandelen trafik som skickas från hello CDN (kant servrar) toorequesters (t.ex. webbläsare) i komprimerat format. |
| Byte ut |Anger hello mängden data i byte som har levererats från hello CDN (kant servrar) toohello beställaren (t.ex. webbläsare). |
| Byte i |Anger hello mängden data i byte som skickats från beställare (t.ex. webbläsare) toohello CDN (kant-servrar). |
| Byte fjärr |Anger hello mängden data i byte som skickats från CDN och kunden ursprung servrar toohello CDN (kant-servrar). |

#### <a name="performance-metrics"></a>Prestandamått
hello syftet med de här måtten är tootrack CDN prestandan för trafiken.

| Fält | Beskrivning |
| --- | --- |
| Överföringshastighet |Anger hello genomsnittligt som överfördes innehåll från hello CDN tooa beställaren. |
| Varaktighet |Anger hello genomsnittstiden, i millisekunder som det tog toodeliver en tillgång tooa beställaren (t.ex. webbläsare). |
| Komprimerade begärandehastighet |Anger hello procentandel träffar som har levererats från hello CDN (kant servrar) toohello beställaren (t.ex. webbläsare) i komprimerat format. |
| 4xx Felfrekvens |Anger hello procentandel träffar som genererade en 4xx statuskod. |
| Frekvens för 5xx-fel |Anger hello procentandel träffar som genererade en 5xx-statuskod. |
| Träffar |Anger hello antal begäranden för CDN-innehåll. |

#### <a name="secure-traffic-metrics"></a>Skydda trafiken mått
hello syftet med de här måtten är tootrack CDN prestanda för HTTPS-trafik.

| Fält | Beskrivning |
| --- | --- |
| Säker cachens effektivitet |Anger hello procentandel av data som överförs för HTTPS-begäranden som har hanteras från cachen. Mätvärdet mäter en cachelagrad version av hello begäran behandlades innehållet direkt från hello CDN (kant servrar) toorequesters (t.ex. webbläsare) via HTTPS. |
| Säker överföringshastighet |Anger hello genomsnittligt som överfördes innehåll från hello CDN (kant servrar) toorequesters (till exempel webbservrar) via HTTPS. |
| Genomsnittlig varaktighet för säker |Anger hello genomsnittstiden, i millisekunder som det tog toodeliver en tillgång tooa beställaren (t.ex. webbläsare) via HTTPS. |
| Säker träffar |Anger hello HTTPS-begäranden för CDN-innehåll. |
| Säker byte ut |Anger hello mängd HTTPS-trafik, i byte som har levererats från hello CDN (kant servrar) toohello beställaren (t.ex. webbläsare). |

## <a name="reports"></a>Rapporter
Varje rapport i den här modulen innehåller ett diagram och statistik över användning av nätverksbandbredd och trafik för olika typer av mått (t.ex. HTTP-statuskoder cache statuskoder begärande-URL, osv.). Den här informationen kan vara används toodelve djupet i hur innehållet kommer tooyour klienter och finjustera toofine CDN beteende tooimprove leverans prestandadata.

### <a name="accessing-hello-edge-performance-reports"></a>Åtkomst till hello edge prestandarapporter
1. Klicka på hello hello CDN-profilbladet **hantera** knappen.
   
    ![CDN-profilbladet hantera knappen](./media/cdn-edge-performance/cdn-manage-btn.png)
   
    hello CDN-hanteringsportalen öppnas.
2. Hovra över hello **Analytics** fliken och sedan hovra över hello **kant prestanda långsammare Analytics** utfällbar.  Klicka på **HTTP stort objekt**.
   
    hello visas edge nod analytics rapporter.

| Rapport | Beskrivning |
| --- | --- |
| Daglig sammanfattning |Här kan du tooview dagliga trafik trender under en angiven tidsperiod. Varje stapel i det här diagrammet motsvarar ett visst datum. hello storleken på hello fältet anger hello totala antalet träffar som inträffat på det datumet. |
| Sammanfattning av varje timme |Här kan du tooview timvis trafik trender under en angiven tidsperiod. Varje stapel i det här diagrammet representerar en enskild timme på ett visst datum. hello storleken på hello fältet anger hello totala antalet träffar som genomförts under den timmen. |
| Protokoll |Visar hello fördelning av trafik mellan hello HTTP och HTTPS-protokoll. Ett ringdiagram anger hello procentandelen träffar som inträffade för varje typ av protokoll. |
| HTTP-metoder |Låter dig tooget är en snabb översikt över vilka HTTP-metoder som används toorequest dina data. Hello de vanligaste HTTP-begäran metoder normalt GET, HEAD och POST. Ett ringdiagram anger hello procentandel träffar som inträffat för varje typ av HTTP-begäran-metod. |
| URL: er |Innehåller ett diagram som visar hello översta 10 begärda URL: er. Ett fält visas för varje URL. hello höjd hello fältet anger hur många träffar som viss URL har genererat över hello tidsintervallet som omfattas av hello rapport. Statistik för de 100 främsta hello begärda URL: er visas direkt under det här diagrammet. |
| Skapa CNAME-poster |Innehåller ett diagram som visar hello översta 10 skapa CNAME-poster används toorequest tillgångar över hello tidsintervallet för en rapport. Statistik för de 100 främsta hello begärt skapa CNAME-poster visas direkt under det här diagrammet. |
| Ursprung |Innehåller ett diagram som visar hello topp 10 CDN eller kund ursprung servrar som begärdes tillgångar under en angiven tidsperiod. Statistik för de 100 främsta hello begärt CDN eller kund ursprung servrar visas direkt under det här diagrammet. Kunden ursprung servrar identifieras av hello namn har definierats i hello katalognamnet alternativet. |
| GEO-POP |Visar hur mycket av din trafik dirigeras via en viss punkt-för-förekomst (POP). hello förkortning med tre bokstäver representerar en POP i vårt CDN-nätverk. |
| Klienter |Innehåller ett diagram som visar hello översta 10 klienter som har begärt tillgångar under en angiven tidsperiod. Hello enligt den här rapporten hello alla begäranden som kommer från samma IP-adress anses toobe från hello samma klient. Statistik för hello översta 100 klienter visas direkt under det här diagrammet. Den här rapporten är användbar för att fastställa download aktivitet mönster för top-klienter. |
| Cache-status |Ger en detaljerad analys av cachebeteende, kan du få metoder för att förbättra hello övergripande slutanvändarens upplevelse. Eftersom hello bästa prestanda kommer från cacheträffar, kan du optimera data leverans hastigheter genom att minimera cachemissar och cacheträffar på har upphört att gälla. |
| Ingen information |Innehåller ett diagram som visar hello översta 10 URL: er för tillgångar som cache innehåll dokumentens inte kontrollerades under en angiven tidsperiod. Statistik för hello översta 100 URL: er för dessa typer av tillgångar visas direkt under det här diagrammet. |
| CONFIG_NOCACHE information |Innehåller ett diagram som visar hello topp 10 URL: er för tillgångar som inte har cachelagrats på grund av toohello kundens CDN konfiguration. Dessa typer av tillgångar har sett direkt från hello ursprungsservern. Statistik för hello översta 100 URL: er för dessa typer av tillgångar visas direkt under det här diagrammet. |
| UNCACHEABLE information |Innehåller ett diagram som visar hello översta 10 URL: er för tillgångar som inte kunde cachelagras på grund av toorequest huvuddata. Statistik för hello översta 100 URL: er för dessa typer av tillgångar visas direkt under det här diagrammet. |
| TCP_HIT information |Innehåller ett diagram som visar hello översta 10 URL: er för tillgångar som hanteras direkt från cachen. Statistik för hello översta 100 URL: er för dessa typer av tillgångar visas direkt under det här diagrammet. |
| TCP_MISS information |Innehåller ett diagram som visar hello översta 10 URL: er för tillgångar som har statusen TCP_MISS cache. Statistik för hello översta 100 URL: er för dessa typer av tillgångar visas direkt under det här diagrammet. |
| TCP_EXPIRED_HIT information |Innehåller ett diagram som visar hello översta 10 URL: er för inaktuella tillgångar som har sett direkt från hello POP. Statistik för hello översta 100 URL: er för dessa typer av tillgångar visas direkt under det här diagrammet. |
| TCP_EXPIRED_MISS information |Innehåller ett diagram som visar hello översta 10 URL: er för inaktuella tillgångar som en ny version hade toobe som hämtats från hello ursprungsservern. Statistik för hello översta 100 URL: er för dessa typer av tillgångar visas direkt under det här diagrammet. |
| TCP_CLIENT_REFRESH_MISS information |Innehåller ett diagram som visar hello översta 10 URL: er för tillgångar har hämtats från en ursprungsservern på grund av tooa no-cache-begäran från hello-klient. Statistik för hello översta 100 URL: er för dessa typer av begäranden visas direkt under det här diagrammet. |
| Klienttyper av begäranden |Anger hello begäranden som gjorts av HTTP-klienter (till exempel webbläsare). Den här rapporten innehåller en ringdiagram som tillhandahåller en mening som toohow begäranden hanteras. Information om bandbredd och trafik för varje typ av begäran visas under hello diagrammet. |
| Användaragent |Innehåller ett stapeldiagram som visar hello översta 10 användare agenter toorequest ditt innehåll genom vår CDN. En användaragent är vanligtvis en webbläsare, media player eller en mobiltelefon webbläsare. Statistik för hello översta 100 användaragenter visas direkt under det här diagrammet. |
| Referenter |Innehåller ett stapeldiagram som visar hello 10 referenter toocontent nås via vårt CDN. Vanligtvis är en referent hello URL till webbsidan hello eller resurs som länkar tooyour innehåll. Detaljerad information finns nedan hello diagram för hello 100 referenter. |
| Komprimeringstyper |Innehåller ett ringdiagram uppdelad begärda tillgångar av om de komprimerats med våra edge-servrar. hello procentandelen komprimerade tillgångar är fördelade på hello typ av komprimering som används. Detaljerad information finns nedan hello diagram för varje typ av komprimering och status. |
| Filtyper |Innehåller ett stapeldiagram som visar hello översta 10 filtyper som har begärt via vårt CDN för ditt konto. Hello av den här rapporten, en filtyp definieras av hello tillgången filnamnstillägg och medietyp för Internet (exempelvis .html \[text/html\], .htm \[text/html\], .aspx \[text/html\]osv.). Detaljerad information finns nedan hello diagram för hello översta 100 filtyper. |
| Unika filer |Innehåller ett diagram som visar hello Totalt antal unika tillgångar som begärdes under en viss dag under en angiven tidsperiod. |
| Token autentisering-sammanfattning |Innehåller ett cirkeldiagram som ger en snabb överblick på om begärda tillgångar skyddades av Token-baserad autentisering. Skyddade tillgångar visas i hello diagram enligt toohello resultaten av sin försök autentisering. |
| Token Auth neka information |Innehåller ett stapeldiagram som gör att du tooview hello översta 10 begäranden som har nekats på grund av tooToken-baserad autentisering. |
| HTTP-svarskoder |Visar en uppdelning av hello HTTP-statuskoder (t.ex. 200 OK 403 tillåts inte, det gick inte att hitta 404, etc.) som har levererats tooyour HTTP klienter av vår edge-servrar. Ett cirkeldiagram kan du tooquickly bedöma hur dina tillgångar har sett. Detaljerad statistiska data anges för varje svarskoden nedan hello diagram. |
| 404-fel |Innehåller ett stapeldiagram som gör att du tooview hello översta 10 begäranden som resulterade i en 404 gick inte att hitta svarskod. |
| 403-fel |Innehåller ett stapeldiagram som gör att du tooview hello översta 10 begäranden som resulterade i en 403 förbjuden svarskod. En 403 förbjuden svarskod inträffar när en begäran nekades av en kund ursprungsserver eller en gränsserver på vår POP. |
| 4xx fel |Innehåller ett stapeldiagram som gör att du tooview hello översta 10 begäranden som resulterade i en svarskod i hello 400 intervallet. Undantas från den här rapporten är 403 inte hittas och svarskoder 404 förbjuden. En svarskod 4xx inträffar vanligtvis när en begäran nekas på grund av ett klientfel. |
| 504 fel |Innehåller ett stapeldiagram som gör att du tooview hello översta 10 förfrågningar som har resulterat i en 504 Gateway-Timeout-svarskod. En 504 Gateway-Timeout-svarskod inträffar när en timeout inträffar när en HTTP-proxy försöker toocommunicate med en annan server. I vår CDN hello fall normalt en 504 Gateway-Timeout-svarskod när en gränsserver är tooestablish kommunikation med en kund ursprungsservern. |
| 502 fel |Innehåller ett stapeldiagram som gör att du tooview hello översta 10 begäranden som resulterade i en 502 svarskod felaktig Gateway. En 502 felaktig Gateway-svarskod inträffar när ett HTTP-protokollfel inträffar mellan en server och en HTTP-proxy. I vår CDN hello fall normalt en 502 felaktig Gateway-svarskod när en kund ursprungsservern returnerar ett ogiltigt svar tooan edge-server. Ett svar är ogiltigt om det inte går att parsa eller om den är ofullständig. |
| 5xx-fel |Innehåller ett stapeldiagram som gör att du tooview hello översta 10 begäranden som resulterade i en svarskod i hello 500 intervallet.  Undantas från den här rapporten är 502 felaktig Gateway och svarskoder 504 Gateway-Timeout. |

## <a name="see-also"></a>Se även
* [Azure CDN-översikt](cdn-overview.md)
* [Realtid statistik i Microsoft Azure CDN](cdn-real-time-stats.md)
* [Åsidosätta HTTP standardinställningar med hjälp av hello regelmotor](cdn-rules-engine.md)
* [Avancerade http-rapporter](cdn-advanced-http-reports.md)

