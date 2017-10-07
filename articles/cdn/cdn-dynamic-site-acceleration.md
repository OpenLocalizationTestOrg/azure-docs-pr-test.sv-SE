---
title: aaaDynamic plats Acceleration via Azure CDN
description: "Dynamiska acceleration ingående"
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
ms.date: 08/02/2017
ms.author: v-semcev
ms.openlocfilehash: 37e6312ae5e83448f2d87c95ef48c387355748bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-site-acceleration-via-azure-cdn"></a>Dynamiska Acceleration via Azure CDN

Med hello nedbrytning av sociala medier, elektronisk handel och hello hyper anpassad webbplats skapas snabbt ökade andelen hello innehåll hanteras tooend användare i realtid. Användarna förväntar sig att snabb, tillförlitlig och anpassade web-upplevelser, oberoende av deras webbläsare, plats, enhet eller nätverk. Hello mycket innovationer som gör att dessa erfarenheter så att också långsamma sidan hämtningar och hello kvaliteten på hello användarfunktioner medföra risker. 

Standard CDN kapaciteten inkluderar hello möjlighet toocache filer närmare tooend användare toospeed upp överföringen av statiska filer. Men med dynamiska webbprogram cachelagring innehållet i platser är inte möjligt eftersom hello server genererar hello innehåll i svaret toouser beteende. Snabbare hello överföringen av innehållet är mer komplexa än traditionella edge cachelagring och kräver en slutpunkt till slutpunkt-lösning som finjusterar varje element längs hello hela sökvägen från Start toodelivery. Med Azure CDN dynamiska plats Acceleration (DSA), bättre hello webbsidor med dynamiskt innehåll mätbart prestanda.

Azure CDN från Akamai och Verizon erbjuder DSA optimering via hello **optimerade för** menyn under skapande av slutpunkten.

## <a name="configuring-cdn-endpoint-tooaccelerate-delivery-of-dynamic-files"></a>Konfigurera leverans av CDN-slutpunkten tooaccelerate dynamiska filer

Du kan konfigurera din slutpunkt toooptimize leverans av CDN dynamiska filer via Azure-portalen genom att välja hello **dynamiska acceleration** alternativ under hello **optimerade för** egenskap under Skapa en hello slutpunkt. Du kan också använda vår REST-API: er eller någon av hello klient-SDK: er toodo hello samma sak programmässigt. 

### <a name="probe-path"></a>Avsökningen sökväg
Avsökningen sökvägen är en specifik funktion tooDynamic plats Acceleration och ett giltigt krävs för att skapa. DSA använder en liten *avsökningen sökvägen* filen placeras på hello ursprung toooptimize routning nätverkskonfigurationer för hello CDN. Du kan ladda ned och ladda upp vår Exempelplats filen tooyour eller använda en befintlig tillgång på din ursprung är ungefär 10 KB för hello avsökningen sökväg i stället om hello tillgångsinformation finns.

> [!Note]
> DSA debiteras extra. Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/cdn/) för mer information.

hello följande skärmdumpar visar hello processen via Azure-portalen.
 
![Lägga till en ny CDN-slutpunkt](./media/cdn-dynamic-site-acceleration/01_Endpoint_Profile.png) 

*Bild 1: Lägga till en ny CDN-slutpunkt från hello CDN-profilen*
 
![Skapa en ny CDN-slutpunkt med DSA](./media/cdn-dynamic-site-acceleration/02_Optimized_DSA.png)  

*Bild 2: Skapa en CDN-slutpunkt med dynamiska acceleration optimering markerat*

När hello CDN-slutpunkten har skapats gäller hello DSA optimeringar för alla filer som matchar vissa villkor. hello följande avsnitt beskriver DSA optimering i detalj.

## <a name="dsa-optimization-using-azure-cdn"></a>DSA-optimering med hjälp av Azure CDN

Dynamisk plats Acceleration på Azure CDN snabbare leverans av dynamisk tillgångar med hjälp av hello följande metoder:

-   Väg optimering
-   TCP-optimeringar
-   Objektet Prefetch (Akamai)
-   Komprimering av mobila avbildningen (Akamai)

### <a name="route-optimization"></a>Väg optimering

Väg optimering är viktig eftersom hello Internet är en dynamisk plats där trafik och tillfälligt avbrott ändras ständigt hello nätverkets topologi. hello Border Gateway Protocol (BGP) är hello routingprotokoll av hello Internet, men det kan finnas snabbare vägar via mellanliggande av förekomst (PoP) servrar. 

Väg optimering väljer hello mest optimala sökvägen toohello ursprung så att en plats är kontinuerligt tillgängliga och dynamiskt innehåll levereras tooend användare via hello snabbaste och mest tillförlitliga möjliga vägen. 

Hej Akamai nätverk använder tekniker toocollect realtidsdata och jämför olika sökvägar via olika noder i hello Akamai server samt hello BGP standardvägen mellan hello öppna Internet toodetermine hello snabbaste vägen mellan hello ursprung och hello CDN-kant. Dessa tekniker undvika Internet överbelastning punkter och lång vägar. 

På liknande sätt hello hello Verizon nätverk använder en kombination av Anycast DNS, hög kapacitet stöder POP- och statuskontroller, toodetermine hello bästa gateways toobest vidarebefordra data från klienten toohello ursprung.
 
Därför helt dynamiska och transaktionell innehållet levereras snabbare och mer tillförlitligt tooend användare, även om det är uncacheable. 

### <a name="tcp-optimizations"></a>TCP-optimeringar

Transmission Control Protocol (TCP) är hello standard av hello Internet-protokollsviten används toodeliver information mellan program på ett IP-nätverk.  Det finns flera tillbaka som standard och tillbaka begäranden krävs tooset upp en TCP-anslutning, samt gränser tooavoid nätverk congestions, vilket resulterar i ineffektiviteter i större skala. Azure CDN från Akamai behandlar det här problemet genom att optimera tre områden: 

 - Ta bort långsam start
 - Utnyttja beständiga anslutningar
 - justering av TCP-parametrar för paketet (Akamai)

#### <a name="eliminating-slow-start"></a>Ta bort långsam start

*Långsamma start* är en del av hello TCP-protokollet som förhindrar överbelastning på nätverket genom att begränsa hello mängden data som skickas över hello nätverk. Den startar med små överbelastning fönsterstorlek mellan avsändare och mottagare tills hello maximala nås eller paketförlust har identifierats.

Azure CDN från Akamai och Verizon eliminerar långsam startar i tre steg:

1.  Både Akamai och Verizons nätverk använder hälsa och bandbredd övervakning toomeasure hello bandbredden för anslutningar mellan edge PoP-servrar.
2. hello mått delas mellan edge PoP-servrar så att varje server är medveten om hello nätverksförhållanden och Servertillstånd för hello andra POP-servrar runtom.  
3. hello CDN edge servrar är nu kan toomake antaganden om vissa överföring parametrar, till exempel vilka hello optimal fönsterstorlek ska vara vid kommunikation med andra CDN edge-servrar i dess närhet. Det här steget innebär hello inledande överbelastning fönstrets storlek kan ökas om hello hälsotillstånd hello anslutning mellan hello CDN edge servrar kan högre paket dataöverföringar.  

#### <a name="leveraging-persistent-connections"></a>Utnyttja beständiga anslutningar

Använder en CDN ansluta färre unika datorer tooyour ursprungsservern direkt jämfört med användare som ansluter direkt tooyour ursprung. Azure CDN från Akamai och Verizon adresspooler även användare begäranden tillsammans tooestablish färre anslutningar med hello ursprung.

Som tidigare nämnts är utför TCP-anslutningar flera begäranden fram och tillbaka i en handskakning tooestablish en ny anslutning. Beständiga anslutningar, även kallat ”HTTP Keep-Alive”, återanvända befintliga TCP-anslutningar för flera HTTP-begäranden toosave fram och åter gånger och snabba upp överföringen. 

hello Verizon nätverket skickar också periodiska keep alive-paket över hello TCP-anslutning tooprevent en öppen anslutning stängs.

#### <a name="tuning-tcp-packet-parameters"></a>Justera parametrarna för TCP-paket

Azure CDN från Akamai också justerar hello parametrarna som styr server till server-anslutningar och minskar hello lång drag avrunda resor krävs tooretrieve innehåll inbäddat i hello platsen med hjälp av följande tekniker hello:

1.  Öka hello inledande överbelastning fönstret så att flera paket kan skickas utan att vänta på en bekräftelse.
2.  Minska hello inledande sändningsförsök timeout så att förlust identifieras och återöverföring sker snabbare.
3.  Minskar hello minimum och maximum skickar timeout tooreduce hello väntetiden innan förutsatt att paket gått förlorade under överföringen.

### <a name="object-prefetch-akamai-only"></a>Objektet Prefetch (Akamai)

De flesta webbplatser består av en HTML-sida som refererar till andra resurser, till exempel bilder och skript. Normalt när en klient begär en webbsida, hello webbläsaren först ned Parsar hello HTML-objekt och gör sedan ytterligare förfrågningar toolinked tillgångar som är nödvändiga toofully läsa hello-sidan. 

*Prefetch* är en teknik som tooretrieve bilder och skript inbäddat i hello HTML-sida medan hello HTML hanteras toohello webbläsare och innan hello webbläsare även gör dessa begäranden för objektet. 

Med hello **förhämtning** alternativet är aktiverat för närvarande hello när hello CDN fungerar hello HTML bassida toohello klientens webbläsare hello CDN Parsar hello HTML-fil och begär ytterligare för länkade resurser och lagra den i sin cache. När hello klienten gör hello begäranden för hello länkade tillgångar, har hello begärda objekten hello CDN edge-server redan och kan hantera dem direkt utan att en onödig kommunikation toohello ursprung. Denna optimering fördelar både Cacheable ställs och icke Cacheable ställs innehåll.

### <a name="adaptive-image-compression-akamai-only"></a>Komprimering av anpassningsbar avbildningen (Akamai)

Vissa enheter, särskilt mobila de långsammare nätverkshastigheter från tid tootime-upplevelse. I dessa scenarier är det bättre för hello användaren tooreceive mindre bilder i deras webbsida snabbare i stället för väntar på lång tid för fullständig upplösning.

Den här funktionen automatiskt övervakar nätverket kvalitet och använder standardmetoder för JPEG-komprimering när nätverkshastigheter är långsammare tooimprove leveranstiden.

Komprimering av anpassningsbar avbildningen | Filnamnstillägg  
--- | ---  
JPEG-komprimering | JPG, JPEG, jpe, .jig, .jgig, .jgi

## <a name="caching"></a>Cachelagring

Med DSA, är cachelagring inaktiverat som standard på hello CDN, även om hello ursprung inkluderar cache-control/upphör att gälla rubriker hello svar. Den här standardinställningen har inaktiverats eftersom DSA används vanligtvis för dynamiska tillgångar som inte ska cachelagras eftersom de unika tooeach klienten och aktivera cachelagring som standard kan dela det här beteendet.

Om du har en webbplats med en blandning av statiska och dynamiska tillgångar, är det bästa tootake en hybrid metoden tooget hello bästa prestanda. 

Om du använder ADN med Verizon Premium kan aktivera du cachelagring igen för särskilda fall med hello regelmotor.  

Ett alternativ är toouse två CDN-slutpunkter. Med DSA toodeliver dynamiska tillgångar och en slutpunkt med en statisk optimering skriver, till exempel allmänna web leverans toodelivery Cacheable ställs tillgångar. I ordning tooaccomplish detta alternativ ska du ändra din webbsida URL: er toolink direkt toohello tillgången på hello CDN-slutpunkt som du planerar toouse. 

Till exempel: `mydynamic.azureedge.net/index.html` är en dynamisk sida och har lästs in från hello DSA-slutpunkten.  hello html-sidan refererar till flera statiska resurser, t.ex JavaScript-bibliotek eller bilder som har lästs in från hello statisk CDN-slutpunkt som `mystatic.azureedge.net/banner.jpg` och `mystatic.azureedge.net/scripts.js`. 

Du hittar ett exempel [här](https://docs.microsoft.com/azure/cdn/cdn-cloud-service-with-cdn#controller) på hur toouse domänkontrollanter i en ASP.NET web-tooserve innehåll via en specifik CDN-URL.




