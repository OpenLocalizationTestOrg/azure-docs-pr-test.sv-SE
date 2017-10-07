---
title: "användningsstatistik för aaaAnalyze med Azure CDN avancerade HTTP rapporter | Microsoft Docs"
description: "Lär dig hur toocreate avancerade http-rapporter i Microsoft Azure CDN. De här rapporterna ger detaljerad information om CDN-aktivitet."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: ef90adc1-580e-4955-8ff1-bde3f3cafc5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 3184ec00d089613e25c62762f93043cb4ba68394
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-usage-statistics-with-azure-cdn-advanced-http-reports"></a>Analysera användningsstatistik med Azure CDN avancerade http-rapporter
## <a name="overview"></a>Översikt
Det här dokumentet beskrivs avancerade http-rapportering i Microsoft Azure CDN. De här rapporterna ger detaljerad information om CDN-aktivitet.

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Åtkomst till avancerade HTTP-rapporter
1. Klicka på hello hello CDN-profilbladet **hantera** knappen.
   
    ![CDN-profilbladet hantera knappen](./media/cdn-advanced-http-reports/cdn-manage-btn.png)
   
    hello CDN-hanteringsportalen öppnas.
2. Hovra över hello **Analytics** fliken och sedan hovra över hello **avancerade HTTP rapporter** utfällbar.  Klicka på **HTTP stora plattform**.
   
    ![CDN-hanteringsportalen - avancerade rapporter-menyn](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)
   
    Rapportalternativ visas.

## <a name="geography-reports-map-based"></a>Geografi rapporter (karta-baserat)
Det finns fem rapporter som utnyttjar en karta tooindicate hello regioner som ditt innehåll som begärs. Dessa rapporter är World karta, USA karta, Kanada karta, Europa kartan och karta för Asien/Stillahavsområdet.

Varje karta-baserad rapport rangordnar geografiska enheter (d.v.s. länder, tillstånd och regioner) enligt toohello procentandel träffar som kommer från den regionen. Dessutom kan tillhandahålls en karta toohelp visualisera hello platser där ditt innehåll som begärs. Det är kan toodo så av färgkodade varje region enligt toohello mängden begäran inträffade i den regionen. Lättare skuggad regioner ange lägre behovet av innehåll, medan mörka områdena anger högre nivåer av behovet av ditt innehåll.

Detaljerad information om trafik och bandbredden för varje region tillhandahålls direkt under hello kartan. Detta gör att du tooview hello totala antalet träffar, hello procentandel träffar, hello totala mängden data som överförs (i GB) och hello procentandelen data överförs för varje region. Visa en beskrivning för var och en av de här måtten. Slutligen, när du hovrar över en region (d.v.s. land, delstat eller provins) hello namn och hello procentandel träffar som uppstått i hello region visas som en knappbeskrivning.

En kort beskrivning finns nedan för varje typ av geografisk karta-baserade rapporten.

| Rapportnamn | Beskrivning |
| --- | --- |
| World karta |Den här rapporten kan du tooview hello efterfrågan för CDN-innehåll. Varje land är färgkodade på hello world kartan tooindicate hello procentandel träffar som kommer från den regionen. |
| USA: S karta |Den här rapporten kan du tooview hello begäran för CDN-innehåll i hello USA. Varje tillstånd är färgkodade på den här kartan tooindicate hello procentandel träffar som kommer från den regionen. |
| Kanada karta |Den här rapporten kan du tooview hello begäran för din CDN-innehåll i Kanada. Varje region är färgkodade på den här kartan tooindicate hello procentandel träffar som kommer från den regionen. |
| Europa karta |Den här rapporten kan du tooview hello begäran för din CDN-innehåll i Europa. Varje land färgkodade på den här kartan tooindicate hello procentandel träffar som kommer från den regionen. |
| Asien Pacific karta |Den här rapporten kan du tooview hello begäran för din CDN-innehåll i Asien. Varje land färgkodade på den här kartan tooindicate hello procentandel träffar som kommer från den regionen. |

## <a name="geography-reports-bar-charts"></a>Geografi rapporter (cirkeldiagram)
Det finns två ytterligare rapporter som ger statistisk information enligt toogeography, som är upp orter och upp länder. De här rapporterna rangordnas städer och länder, respektive enligt toohello antal träffar som kommer från dessa regioner. Vid genererar den här typen av rapporten, visar ett stapeldiagram hello översta 10 städer och länder som begärt innehåll över en viss plattform. Den här stapeldiagram kan du tooquickly utvärdera hello regioner som genererar hello högsta antal begäranden för ditt innehåll.

hello vänster sida av hello diagram (y-axeln) anger hur många träffar uppstod i hello regionen. Direkt under hello diagram (x-axeln) hittar du en etikett för varje hello översta 10 regioner.

### <a name="using-hello-bar-charts"></a>Med hjälp av hello stapeldiagram
* Om du hovrar över en stapel visas hello namn och hello totala antalet träffar som uppstått i hello region som en knappbeskrivning.
* hello verktygstipset för hello upp orter rapporten identifierar en stad med dess namn, region och landsförkortning.
* Om hello staden eller region (d.v.s. region) som en begäran kommer från inte kunde fastställas att det indikera att de är okänd. Om hello land är okänd, två frågetecken (d.v.s.?), visas.
* En rapport kan innehålla mått för ”Europa” eller hello ”Asien/Stillahavsområdet Region”. Dessa objekt är inte avsedda tooprovide statistisk information om alla IP-adresser i dessa regioner. I stället gäller de endast toorequests som kommer från IP-adresser som är utspridda över Europa eller Asien/Stillahavsområdet i stället för tooa viss ort eller land.

hello-data som har använt toogenerate hello liggande diagram kan visas under den. Innehåller hello total antal träffar hello procentandelen träffar hello mängden data som överförs (i GB) och hello procentandel av data som överförs för hello översta 250 regioner. Visa en beskrivning för var och en av de här måtten.

En kort beskrivning anges för båda typerna av rapporter nedan.

| Rapportnamn | Beskrivning |
| --- | --- |
| TOP orter |Den här rapporten rangordnar orter enligt toohello antal träffar som kommer från den regionen. |
| Övre länder |Den här rapporten rangordnar länder enligt toohello antal träffar som kommer från den regionen. |

## <a name="daily-summary"></a>Daglig sammanfattning
hello daglig sammanfattningsrapport över kan du tooview hello totala antalet träffar och data som överförs via en viss plattform dagligen. Den här informationen kan användas för tooquickly fram CDN aktivitet mönster. Till exempel hjälper den här rapporten dig att identifiera vilka dagar erfarna högre eller lägre än förväntad trafik.

Vid genererar den här typen av rapporten, ger ett liggande diagram en indikering som toohello plattformsspecifika begäran erfarna dagligen över hello tidsperiod som omfattas av hello rapport. Det gör det genom att visa en stapel för varje dag i hello rapporten. Till exempel välja hello tidsperiod kallas ”förra veckan” genererar ett stapeldiagram med sju staplar. Varje visar hello total antal träffar uppstod på den dagen.

hello vänster sida av hello diagram (y-axeln) indikerar hur många träffar inträffade på hello angivna datum. Direkt under hello diagram (x-axeln) hittar du en etikett som visar hello datum (Format: ÅÅÅÅ-MM-DD) för varje dag med i hello-rapporten.

> [!TIP]
> Om du hovrar över en stapel visas hello totala antalet träffar som inträffat på det datumet som en knappbeskrivning.
> 
> 

hello-data som har använt toogenerate hello liggande diagram kan visas under den. Finns det hello total antal träffar och hello mängden data som överförs (i GB) för varje dag som omfattas av hello rapport.

## <a name="by-hour"></a>Per timme
Hej för timme rapporten kan du tooview hello totala antalet träffar och data som överförs via en viss plattform timme. Den här informationen kan användas för tooquickly fram CDN aktivitet mönster. Till exempel hjälper den här rapporten dig identifiera hello tidsperioder under hello dag som uppstår högre eller lägre än förväntad trafik.

Vid genererar den här typen av rapporten, ger ett liggande diagram en indikering som toohello plattformsspecifika begäran orsakade timme över hello tidsperiod som omfattas av hello rapport. Det gör det genom att visa en stapel för varje timme som omfattas av hello rapport. Till exempel genererar att välja en 24-timmarsformat tidsperiod ett stapeldiagram med 24 staplar. Varje visar hello total antal träffar under den timmen.

hello vänster sida av hello diagram (y-axeln) anger hur många träffar inträffade på hello angiven timme. Direkt under hello diagram (x-axeln) hittar du en etikett som anger hello datum/tid (Format: ÅÅÅÅ-MM-DD hh: mm) för varje timme som inkluderas i hello rapporten. Tid rapporteras med 24-timmarsformat och anges med hjälp av hello UTC/GMT-tidszonen.

> [!TIP]
> Om du hovrar över ett fält visas hello totala antalet träffar som genomförts under den timmen som en knappbeskrivning.
> 
> 

hello-data som har använt toogenerate hello liggande diagram kan visas under den. Finns det hello total antal träffar och hello mängden data som överförs (i GB) för varje timme som omfattas av hello rapport.

## <a name="by-file"></a>Fil
hello av filen rapporten kan du tooview hello trafikmängden begäran och hello som uppstår under en viss plattform för hello mest begärt tillgångar. Ett stapeldiagram kommer att skapas på hello översta 10 mest efterfrågade tillgångar vid genererar den här typen av rapport över hello angivna tidsperioden.

> [!NOTE]
> Hello enligt den här rapporten edge CNAME URL: er är konverterade tootheir motsvarande CDN URL: er. Detta gör att en korrekt tally hello totala antalet träffar som är associerade med en tillgång oavsett hello CDN eller edge CNAME-URL som används för toorequest den.
> 
> 

hello vänster sida av hello diagram (y-axeln) anger hello antal begäranden för varje tillgång över hello angivna tidsperioden. Du hittar en etikett som anger hello filnamn för varje hello översta 10 begärda tillgångar direkt under hello diagram (x-axeln).

hello-data som har använt toogenerate hello liggande diagram kan visas under den. Innehåller följande information för varje hello översta 250 begärda tillgångar hello: relativ sökväg, hello totala antalet träffar, hello procentandel träffar, hello mängden data som överförs (i GB) och hello procentandelen data som överförs.

## <a name="by-file-detail"></a>Efter fil-information
hello av filen detaljerad rapport kan du tooview hello trafikmängden begäran och hello som uppstår under en viss plattform för en viss resurs. Vid hello är högst upp i den här rapporten hello filen information för alternativ. Det här alternativet innehåller en lista över dina mest efterfrågade tillgångar på hello valda plattformen. I ordning toogenerate en av filen detaljerad rapport behöver du tooselect hello önskade tillgångsinformation från hello alternativet filen information för. När du har som ett stapeldiagram visar hello mängden dagliga begäran som den genererade över hello angivna tidsperioden.

hello vänster sida av hello diagram (y-axeln) anger hello Totalt antal begäranden som en tillgång inträffade under en viss dag. Direkt under hello diagram (x-axeln) hittar du en etikett som visar hello datum (Format: ÅÅÅÅ-MM-DD) för vilka CDN-begäran för hello tillgången rapporterades.

hello-data som har använt toogenerate hello liggande diagram kan visas under den. Finns det hello total antal träffar och hello mängden data som överförs (i GB) för varje dag som omfattas av hello rapport.

## <a name="by-file-type"></a>Efter filtyp
hello filtyp rapporten kan du tooview hello trafikmängden begäran och hello drabbar filtyp. Vid genererar den här typen av rapporten, visar ett ringdiagram hello procentandel träffar som genererats av hello översta 10 filtyper.

> [!TIP]
> Om du hovrar över en sektor i hello ringdiagram medietyp hello Internet för filtypen visas som en knappbeskrivning.
> 
> 

hello-data som har använt toogenerate hello ringdiagram kan visas under den. Innehåller hello filen namnet tillägg/Internet medietyp, hello total antal träffar, hello procentandel träffar, hello mängden data som överförs (i GB) och hello procentandel av data som överförs för varje hello uppifrån 250 filtyper.

## <a name="by-directory"></a>Med Directory
hello av Directory rapporten kan du tooview hello trafikmängden begäran och hello som uppstår under en viss plattform för innehåll från en specifik katalog. Vid genererar den här typen av rapporten, visar ett stapeldiagram hello totala antalet träffar som genererats av innehållet i hello översta 10 kataloger.

### <a name="using-hello-bar-chart"></a>Med hjälp av hello liggande stapeldiagram
* Hovra över en stapel tooview hello relativ sökväg toohello motsvarande katalog.
* Innehåll som lagras i en undermapp i en katalog räknas inte vid beräkning av begäran av katalogen. Den här beräkningen endast förlitar sig på hello antalet begäranden som har genererats för innehåll som lagras i hello faktiska directory.
* Hello enligt den här rapporten edge CNAME URL: er är konverterade tootheir motsvarande CDN URL: er. Detta gör att en korrekt tally för all statistik som är associerade med en tillgång oavsett hello CDN eller edge CNAME-URL som används för toorequest den.

hello anger vänster sida av hello diagram (y-axeln) hello Totalt antal begäranden för hello innehåll som lagras i dina översta 10 kataloger. Varje stapel på hello diagrammet representerar en katalog. Använd hello färgkodade schemat toomatch upp en stapel tooa katalog som anges i hello upp 250 fullständig kataloger avsnitt.

hello-data som har använt toogenerate hello liggande diagram kan visas under den. Innehåller följande information för varje hello upp 250 kataloger hello: relativ sökväg, hello totala antalet träffar, hello procentandel träffar, hello mängden data som överförs (i GB) och hello procentandelen data som överförs.

## <a name="by-browser"></a>Webbläsare
hello av webbläsaren rapporten kan du tooview vilka webbläsare har använt toorequest innehåll. Vid genererar den här typen av rapporten, visar ett cirkeldiagram hello procentandelen förfrågningar som hanteras av hello översta 10 webbläsare.

### <a name="using-hello-pie-chart"></a>Med hjälp av hello cirkeldiagram
* Hovra över en sektor i hello cirkeldiagram tooview en webbläsare namn och version.
* För hello av den här rapporten är anses varje kombination unika webbläsarversion vara en annan webbläsare.
* hello segment med namnet ”andra” anger hello procentandelen förfrågningar som hanteras av andra webbläsare och versioner.

hello-data som har använt toogenerate hello cirkeldiagram kan visas under den. Du hittar det hello webbläsare typ/versionsnummer, hello total antal träffar och hello procentandelen träffar för varje hello vanliga 250 webbläsare.

## <a name="by-referrer"></a>Av referent
hello av referent rapporten kan du tooview hello referenter toocontent på hello valda plattformen. En referent anger hello värdnamn som en begäran har genererats. Vid genererar den här typen av rapporten, visar ett stapeldiagram hello mängden begäran (d.v.s. träffar) som genereras av hello 10 referenter.

hello vänster sida av hello diagram (y-axeln) anger hello Totalt antal begäranden som en tillgång inträffade för varje referent. Varje stapel på hello diagrammet representerar en referent. Använd hello färgkodade schemat toomatch upp en stapel tooa referent som anges i hello upp 250 referent avsnitt.

hello-data som har använt toogenerate hello liggande diagram kan visas under den. Du hittar det hello URL, hello total antal träffar och hello procentandelen träffar som genereras från varje hello 250 referenter.

## <a name="by-download"></a>Hämtning
hello genom att hämta rapporten kan du tooanalyze download mönster för dina mest efterfrågade innehåll. hello överkant hello rapporten innehåller ett stapeldiagram som jämför försökte hämtningar med slutförda hämtningar för hello topp 10 begärda tillgångar. Varje fält är färgkodade enligt toowhether är det en försök nedladdning (blå) eller slutförd nedladdning (grön).

> [!NOTE]
> Hello enligt den här rapporten edge CNAME URL: er är konverterade tootheir motsvarande CDN URL: er. Detta gör att en korrekt tally för all statistik som är associerade med en tillgång oavsett hello CDN eller edge CNAME-URL som används för toorequest den.
> 
> 

hello vänster sida av hello diagram (y-axeln) anger hello filnamn för varje hello översta 10 begärda tillgångar. Direkt under hello diagram (x-axeln) hittar du etiketter som visar hello Totalt antal försök/avslutade hämtningar.

Direkt under hello stapeldiagram hello följande information visas för hello översta 250 begärda tillgångar: relativa sökvägen (inklusive filnamnet), hello antal gånger som det var hämtade toocompletion och hello antalet gånger som begärde hello procentandelen förfrågningar som resulterade i en fullständig hämtning.

> [!TIP]
> Vår CDN inte informeras av HTTP-klienten (d.v.s. webbläsare) när en tillgång helt har hämtats. Därför har vi toocalculate om en tillgång har helt hämtats bl.a toostatus koder och begäranden om byte-intervall. hello ser först öppna vi när den här beräkningen är om hello-begäran resulterar i en 200 OK statuskod. I så fall, sedan titta vi på byte-intervall begäranden tooensure de täcker hello hela tillgången. Slutligen jämföra vi hello mängden data som överförs toohello storleken på hello begärda tillgången. Om hello data som överförts är lika tooor större än storleken för hello och hello byteintervall begäranden är lämpliga för tillgången, och sedan hello träffar som ska räknas som en fullständig hämtning.
> 
> Du bör ha i åtanke hello följande punkter som får ändra hello konsekvens och korrektheten i den här rapporten på grund av toohello interpretive karaktär av den här rapporten.
> 
> * Trafikmönster förutsägas inte när användaren agenter fungerar annorlunda. Detta kan ge slutförda hämta resultat som är större än 100%.
> * Tillgångar som utnyttjar HTTP progressiv nedladdning representeras inte korrekt av den här rapporten. Detta är på grund av toousers försöka få toodifferent positioner i en video.
> 
> 

## <a name="by-404-errors"></a>Av 404-fel
hello av 404-fel rapporten kan du tooidentify hello typ av innehåll som genererar hello mest antal 404 gick inte att hitta statuskoder. hello överkant hello rapporten innehåller ett stapeldiagram för hello topp 10 tillgångar som en 404 gick inte att hitta statuskod returnerades. Den här stapeldiagram jämför hello Totalt antal begäranden med begäranden som resulterade i en 404 gick inte att hitta statuskod för dessa tillgångar. Varje fält är färgkodade. Ett gult fält används tooindicate som hello förfrågan resulterade i en 404 gick inte att hitta statuskod. En röd stapel används tooindicate hello Totalt antal begäranden för hello tillgång.

> [!NOTE]
> Observera följande hello hello enligt den här rapporten:
> 
> * En träff representerar en begäran för en tillgång oavsett statuskod.
> * Edge CNAME URL: er är konverterade tootheir motsvarande CDN URL: er. Detta gör att en korrekt tally för all statistik som är associerade med en tillgång oavsett hello CDN eller edge CNAME-URL som används för toorequest den.
> 
> 

hello vänster sida av hello diagram (y-axeln) anger hello filnamn för varje hello översta 10 begärda tillgångar som resulterade i en 404 gick inte att hitta statuskod. Direkt under hello diagram (x-axeln) hittar du etiketter som visar hello Totalt antal begäranden och hello antalet begäranden som resulterade i en 404 gick inte att hitta statuskod.

Direkt under hello stapeldiagram hello följande information visas för hello översta 250 begärda tillgångar: relativa sökvägen (inklusive filnamnet) hello antalet begäranden som resulterade i en 404 gick inte att hitta statuskod, hello totala antal gånger som hello tillgången var begärs och hello procentandelen förfrågningar som resulterade i en 404 gick inte att hitta statuskod.

## <a name="see-also"></a>Se även
* [Azure CDN-översikt](cdn-overview.md)
* [Realtid statistik i Microsoft Azure CDN](cdn-real-time-stats.md)
* [Åsidosätta HTTP standardinställningar med hjälp av hello regelmotor](cdn-rules-engine.md)
* [Analysera Edge prestanda](cdn-edge-performance.md)

