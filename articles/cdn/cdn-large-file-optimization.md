---
title: aaaLarge filen download optimering via hello Azure Content Delivery Network
description: "Optimering av stora filhämtningar förklaras i djup"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: v-semcev
ms.openlocfilehash: 2646979bfb38e997037bcff5b1cdda34e22c394a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="large-file-download-optimization-via-hello-azure-content-delivery-network"></a>Optimering av stora filer download via hello Azure Content Delivery Network

Filstorlekarna av innehåll som levereras via hello Internet fortsätter toogrow på grund av tooenhanced funktioner, förbättrad grafik och medieinnehåll. Den här tillväxt drivs av många faktorer: bredband intrång, större billiga lagringsenheter, omfattande fler HD-video- och Internet-anslutna enheter (IoT). Snabba och effektiva leveransmekanismen för stora filer är kritiska tooensure en smidig och roligare användarfunktioner.

Överföringen av stora filer har flera utmaningar. Först kan hello genomsnittstiden toodownload en stor fil vara viktiga eftersom program inte kan hämta alla data i tur och ordning. I vissa fall kan den hämta hello sista delen av en fil innan den första delen av hello program. Om bara en liten del av en fil har begärts eller användaren håller en hämtning kan hello download misslyckas. hello download kan också fördröjas förrän efter hello innehållsleveransnätverk (CDN) hämtar hello hela filen från hello ursprungsservern. 

Andra anger hello fördröjning mellan en användares dator och hello-filen hello hastighet med vilken de kan visa innehållet. Dessutom påverkar överbelastning och kapacitet nätverksproblem också genomflöde. Större avstånd mellan servrar och användare skapa ytterligare möjligheter till paketet förlust toooccur, vilket minskar kvalitet. hello minskad kvalitet på grund av begränsad genomflöde och ökad paketförlust kan öka hello väntetiden för en fil download toofinish. 

Det tredje många stora filer levereras inte i sin helhet. Användare kan avbryta hämtningen halva eller titta på endast hello första bara några minuter efter en lång MP4-video. Programvara och media leverans företag vill därför toodeliver endast hello del av en fil som krävs. Effektiv distribution av hello begärt delar minskar hello utgående trafik från hello ursprungsservern. Effektiv distribution minskar också hello minne och i/o-belastning på hello ursprungsservern. 

hello Azure Content Delivery Network från Akamai erbjuder en funktion som levererar av stora filer effektivt toousers över hello jordglob i större skala. hello funktionen minskar svarstiderna eftersom det reducerar hello belastningen på hello ursprung servrar. Den här funktionen är tillgänglig med hello Standard Akamai prisnivå.

## <a name="configure-a-cdn-endpoint-toooptimize-delivery-of-large-files"></a>Konfigurera en CDN-slutpunkten toooptimize överföringen av stora filer

Du kan konfigurera din slutpunkt toooptimize leverans av CDN för stora filer via hello Azure-portalen. Du kan också använda vår REST-API: er eller någon av hello klienten SDK toodo detta. hello visar följande steg hello processen via hello Azure-portalen.

1. tooadd en ny slutpunkt på hello **CDN-profilen** väljer **Endpoint**.

    ![Ny slutpunkt](./media/cdn-large-file-optimization/01_Adding.png)  
 
2. I hello **optimerade för** listrutan, Välj **stora Filhämtning**.

    ![Optimering av stora filer markerat](./media/cdn-large-file-optimization/02_Creating.png)


När du har skapat hello CDN-slutpunkten gäller hello stora filer optimeringar för alla filer som matchar vissa villkor. hello följande avsnitt beskriver den här processen.

## <a name="optimize-for-delivery-of-large-files-with-hello-azure-content-delivery-network-from-akamai"></a>Optimera för överföringen av stora filer med hello Azure Content Delivery Network från Akamai

funktionen för typen för optimering av stora filer hello Aktiverar optimeringar och konfigurationer toodeliver stora nätverksfiler snabbare och mer responsively. Allmän web med Akamai cachelagrar filer endast under 1,8 GB och kan tunneln (inte cache) filer upp too150 GB. Optimering av stora filer cachelagrar filer upp too150 GB.

Optimering av stora filer är effektiv när vissa villkor är uppfyllda. Villkor är hur hello ursprungsservern fungerar och hello storlekar och typer av hello-filer som har begärts. Innan vi går in information om detta bör du förstå hur hello optimering fungerar. 

### <a name="object-chunking"></a>Objekt-högoptimerat 

hello Azure Content Delivery Network från Akamai använder en teknik som kallas objektet högoptimerat. När en stor fil begärs hämtar hello CDN mindre delar av hello-fil från hello ursprung. När hello CDN edge/POP servern tar emot en begäran om filen full eller byte-intervall, kontrollerar den om hello filtypen stöds denna optimering. Den kontrollerar också om hello filtyp uppfyller hello filen storlek. Om hello filstorlek är större än 10 MB, begär hello CDN gränsservern hello-fil från hello ursprung i block 2 MB. 

När hello segmentet når hello CDN edge, har den cachelagras och hanteras direkt toohello användare. hello CDN sedan prefetches hello nästa segment parallellt. Den här prefetch säkerställer att hello innehåll förblir en segment i hello användare, vilket minskar svarstiden. Den här processen fortsätter tills hello hela filen laddas ned (om så krävs) och alla byteintervall är tillgängliga (om så krävs), eller hello klienten avslutar anslutningen hello. 

Läs mer på hello byte-intervall begäran [RFC 7233](https://tools.ietf.org/html/rfc7233).

hello CDN cachelagrar alla segment när de tas emot. hello hela filen har inte toobe som cachelagras på hello CDN-cachen. Efterföljande begäranden om hello filen eller byte områden hanteras från hello CDN cache. Om inte alla hello segment cachelagras på hello CDN är prefetch används toorequest segment från hello ursprunget. Denna optimering beroende hello möjligheten för hello ursprung server toosupport byte-intervall begäranden. _Om hello ursprungsservern inte stöder byte-intervall begäranden, inte denna optimering effektivt._ 

### <a name="caching"></a>Cachelagring
Optimering av stora filer använder olika cachelagring förfallodatum standardtiden från Internet leverans. Den skiljer mellan cachelagring av positiva och negativa cachelagring baserat på http-svarskoder. Om hello ursprungsservern anger en förfallotid via en cache-control eller expires-rubrik hello svar, godkänner hello CDN värdet. När hello ursprung inte ange och hello filen matchar de villkor som hello typ och storlek för den här typen av optimering, använder hello CDN hello standardvärden för optimering av stora filer. I annat fall används hello CDN standardvärden för allmän web leverans.


|    | Allmän web | Optimering av stora filer 
--- | --- | --- 
Cachelagring: positivt <br> HTTP 200, 203, 300, <br> 301, 302 och 410 | 7 dagar |1 dag  
Cachelagring: negativt <br> HTTP 204, 305, 404, <br> och 405 | Ingen | 1 sekund 

### <a name="deal-with-origin-failure"></a>Hantera ursprung fel

hello ursprung Läs-timeout längden ökar från två sekunderna för allmän web leverans tootwo minuter för hello stora optimering filtyp. Denna ökning konton för hello större fil storlekar tooavoid en för tidig timeout-anslutning.

När en anslutning timeout återförsök hello CDN flera gånger innan den skickar en ”504 - Gateway-Timeout” fel toohello klient. 

### <a name="conditions-for-large-file-optimization"></a>Villkor för optimering av stora filer

hello i den följande tabellen listas hello uppsättning villkor toobe nöjd för optimering av stora filer:

Villkor | Värden 
--- | --- 
Filtyper som stöds | 3g, 2, 3gp, asf, avi, bz2, dmg, exe, f4v, flv, <br> GZ, hdp, iso, jxr, m4v, mkv, mov, mp4, <br> MPEG, mpg, mts, pkg, qt, rm, swf, tar, <br> tgz, wdp, webm, webp, wma, wmv, zip  
Minsta filstorlek | 10 MB 
Maximal filstorlek | 150 GB 
Ursprung server egenskaper | Måste ha stöd för begäranden med byte-intervall 

## <a name="optimize-for-delivery-of-large-files-with-hello-azure-content-delivery-network-from-verizon"></a>Optimera för överföringen av stora filer med hello Azure Content Delivery Network från Verizon

hello Azure Content Delivery Network från Verizon ger stora filer utan ett tak för filstorlek. Ytterligare funktioner är aktiverad som standard toomake överföringen av stora filer snabbare.

### <a name="complete-cache-fill"></a>Fullständig cache fill

hello standard fullständig fill cachefunktionen kan hello CDN toopull en fil till hello-cachen när en inledande begäran avbrutna eller tappas bort. 

Fullständig cache fill är mest användbart för stora tillgångar. Normalt användarna inte ladda ned dem från start toofinish. De använder progressiv hämtning. hello standardbeteendet tvingar hello edge server tooinitiate bakgrundshämtning hello tillgångsinformation från hello ursprungsservern. Därefter kan är hello tillgången i hello edge-serverns lokala cacheminne. När fullständig hello-objektet är i hello cache, uppfyller hello gränsservern byte-intervall begäranden toohello CDN för hello cachelagrade objekt.

hello standardbeteendet kan inaktiveras via hello regelmotor i hello Verizon Premium-nivån.

### <a name="peer-cache-fill-hot-filing"></a>Peer-cache fyller hot arkivering

hello standard peer-cache fill hot ansökan funktionen använder en avancerad upphovsrättsskyddade algoritm. Den använder ytterligare edge cachelagring servrar som baseras på bandbredd och mängd begäranden mått toofulfill klientbegäranden för stora, hög populära objekt. Den här funktionen förhindrar att en situation där stort antal extra begäranden skickas tooa användarens ursprungsservern. 

### <a name="conditions-for-large-file-optimization"></a>Villkor för optimering av stora filer

hello optimering funktioner för Verizon är aktiverade som standard. Det finns ingen begränsning för maximal filstorlek. 

## <a name="additional-considerations"></a>Annat som är bra att tänka på

Överväg att hello följande ytterligare aspekter för den här typen av optimering.
 
### <a name="azure-content-delivery-network-from-akamai"></a>Azure Content Delivery Network från Akamai

- Hej högoptimerat processen genererar ytterligare begäranden toohello ursprungsservern. Hello den totala är mängden data som levereras från hello ursprung dock mycket mindre. Segment resulterar i bättre cachelagring egenskaper på hello CDN.

- Minne och i/o-belastning minskas vid hello ursprung eftersom mindre delar av hello fil levereras.

- För segment som cachelagras på hello CDN finns inga ytterligare begäranden toohello ursprung tills hello innehållet upphör att gälla eller avlägsnas från hello cache.

- Användare kan göra intervallet toohello CDN-begäranden och de är behandlas som normala filer. Optimering gäller endast om det är en ogiltig filtyp och hello byte-intervallet är mellan 10 MB och 150 GB. Om hello genomsnittliga filstorleken begärt är mindre än 10 MB, kanske du vill toouse allmänna web leverans i stället.

### <a name="azure-content-delivery-network-from-verizon"></a>Azure Content Delivery Network från Verizon

hello allmänna web Leveranstyp optimering kan ge stora filer.
