---
title: "aaaMedia strömning optimering via hello Azure Content Delivery Network"
description: "Optimera strömmande media-filer för smooth leverans"
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
ms.openlocfilehash: a05a86204708c7ea7ef1f9be04323cdda6a2d403
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="media-streaming-optimization-via-hello-azure-content-delivery-network"></a>Direktuppspelning optimering via hello Azure Content Delivery Network 
 
Användning av video med hög definition ökar på hello Internet, vilket skapar svårigheter för effektiv överföringen av stora filer. Kunder räknar jämn uppspelning av video på begäran eller live video tillgångar på olika nätverk och klienter över hela hälsningsmeddelande. Snabba och effektiva leveransmekanismen för direktuppspelning filer är kritiska tooensure en smidig och roligare användarfunktioner.  

Live strömmande medier är särskilt svårt toodeliver på grund av hello stora storlekar och antalet samtidiga användare. Långa fördröjningar orsaka användare tooleave. Eftersom levande dataströmmar inte kan cachelagras i förväg och stora svarstiderna inte är godkända tooviewers, levereras video fragment inom rimlig tid. 

hello begäran mönster direktuppspelning av ger också några nya utmaningar. När en direktsänd dataström med populära eller en nya serie släpps video på begäran, tusentalsavgränsare toomillions av användare kan begära hello dataström vid hello samtidigt. I det här fallet överväldigande smart begäran konsolidering är viktiga toonot hello ursprung servrar om hello tillgångar inte cachelagras ännu.
 
hello Azure Content Delivery Network från Akamai erbjuder en funktion som ger dig strömmande media tillgångar effektivt toousers över hello jordglob i större skala. hello funktionen minskar svarstiderna eftersom det reducerar hello belastningen på hello ursprung servrar. Den här funktionen är tillgänglig med hello Standard Akamai prisnivå. 

hello Azure Content Delivery Network från Verizon levererar strömmande medier direkt i hello allmänna web Leveranstyp optimering.
 
## <a name="configure-an-endpoint-toooptimize-media-streaming-in-hello-azure-content-delivery-network-from-akamai"></a>Konfigurera en slutpunkt toooptimize direktuppspelning i hello Azure Content Delivery Network från Akamai
 
Du kan konfigurera din content delivery network (CDN) slutpunkt toooptimize leverans för stora filer via hello Azure-portalen. Du kan också använda vår REST-API: er eller någon av hello klienten SDK toodo detta. hello visar följande steg hello processen via hello Azure-portalen:

1. tooadd en ny slutpunkt på hello **CDN-profilen** väljer **Endpoint**.
  
    ![Ny slutpunkt](./media/cdn-media-streaming-optimization/01_Adding.png)

2. I hello **optimerade för** listrutan, Välj **Video på begäran direktuppspelning** för video-on-demand tillgångar. Om du gör en kombination av levande och video-on-demand strömning Välj **allmänna direktuppspelning**.

    ![Strömning markerat](./media/cdn-media-streaming-optimization/02_Creating.png) 
 
När du har skapat hello endpoint gäller hello-optimering för alla filer som matchar vissa villkor. hello följande avsnitt beskriver den här processen. 
 
## <a name="media-streaming-optimizations-for-hello-azure-content-delivery-network-from-akamai"></a>Direktuppspelning optimeringar för hello Azure Content Delivery Network från Akamai
 
Media strömmande optimering från Akamai börjar gälla för live eller video-on-demand strömmande medier som använder enskilda media fragment för leverans. Den här processen skiljer sig från en enda stor tillgång överförs via progressiv hämtning eller genom att använda byteintervall begäranden. Information om formatet för leverans av media finns [optimering för stora filer](cdn-large-file-optimization.md).


hello använder allmänna leverans eller video-on-demand media leverans optimering medietyper en CDN med backend-optimeringar toodeliver media tillgångar snabbare. De kan också använda konfigurationer för media tillgångar baserat på bästa praxis lärt dig över tid.

### <a name="caching"></a>Cachelagring

Om hello Azure Content Delivery Network från Akamai identifierar hello tillgången är en strömmande manifest eller fragment, använder olika tidpunkter för cachelagring upphör att gälla från Internet leverans. (Se hello fullständiga listan i den följande tabellen hello.) Som alltid funktion cache-control eller Expires-huvuden som skickas från hello ursprunget. Om hello tillgången inte är en tillgång med media, cachelagrar den med hjälp av hello giltighetstid gånger allmänna webben.

hello kort tid för negativ cachelagring är användbart för ursprung avlastning när många användare begär ett fragment som ännu inte finns. Ett exempel är en direktsänd dataström där hälsningspaket är inte tillgängliga från hello ursprunget den andra. hello längre cachelagring intervall hjälper också att avlasta begäranden från hello ursprung eftersom videoinnehåll vanligtvis inte ändras.
 

|    | Allmänt<br> webbprogram<br>leverans | Allmänt<br> media<br> strömning | Video-on-demand <br>media<br> strömning  
--- | --- | --- | ---
Cachelagring: positivt <br> HTTP 200, 203, 300, <br> 301, 302 och 410 | 7 dagar |365 dagar | 365 dagar   
Cachelagring: negativt <br> HTTP 204, 305, 404, <br> och 405 | Ingen | 1 sekund | 1 sekund
 
### <a name="deal-with-origin-failure"></a>Hantera ursprung fel  

Allmän media och leverans av video-on-demand media också ha ursprung timeout och ett försök logga baserat på bästa praxis för mönster av vanliga begäranden. Till exempel eftersom allmänna media levereras för live och leverans av video-on-demand media, använder en kortare timeout på grund av toohello tidskritiska uppbyggnad direktuppspelad.

När en anslutning timeout återförsök hello CDN flera gånger innan den skickar en ”504 - Gateway-Timeout” fel toohello klient. 

När en fil matchar hello typ och storlek villkor fillistan, använder hello CDN hello beteendet för direktuppspelning. I annat fall används allmänt web leverans.
   
### <a name="conditions-for-media-streaming-optimization"></a>Villkor för direktuppspelning optimering 

hello i den följande tabellen listas hello uppsättning villkor toobe nöjd för direktuppspelning optimering: 
 
Strömmande typer som stöds | Filnamnstillägg  
--- | ---  
Apple HLS | m3u8, m3u, m3ub, nyckel, ts, aac
Adobe hårddiskar | f4m, f4x, drmmeta, bootstrap, f4f,<br>Godtogs Fragm URL-struktur <br> (matchar regex: ^(/.*)Seq(\d+)-Frag(\d+)
STRECK | MPD, streck, divx, ismv, m4s, m4v, mp4, mp4v, <br> sidx, webm, mp4a, m4a, isma
Smooth streaming | manifest-, fragment/QualityLevels / /
  

 
## <a name="media-streaming-optimizations-for-hello-azure-content-delivery-network-from-verizon"></a>Direktuppspelning optimeringar för hello Azure Content Delivery Network från Verizon

hello Azure Content Delivery Network från Verizon levererar strömmande media tillgångar direkt med hjälp av hello allmänna web Leveranstyp optimering. Några funktioner på hello CDN att direkt leverera media tillgångar som standard.

### <a name="partial-cache-sharing"></a>Partiell cache delning

Partiell cache delar kan hello CDN tooserve delvis cachelagrat innehåll toonew begäranden. Till exempel om hello första begäran toohello CDN resulterar i en cache-miss, hello begäran skickas toohello ursprung. Även om den här ofullständigt innehåll läses in i hello CDN cache, kan andra begäranden toohello CDN börja hämta dessa data. 

### <a name="cache-fill-wait-time"></a>Väntetiden för cache fill

 hello fill vänta tid cachefunktionen tvingar hello edge server toohold alla efterföljande begäranden om hello samma resurs förrän huvuden som tas emot från hello ursprungsservern HTTP-svar. Om HTTP-svarshuvuden hello ursprung anländer innan hello timer upphör att gälla, visas alla begäranden som har spärra utanför hello växande cache. Vid hello samma tid, hello cache fylls med hjälp av data från hello ursprung. Väntetiden för hello cache fill är som standard too3 000 millisekunder. 

