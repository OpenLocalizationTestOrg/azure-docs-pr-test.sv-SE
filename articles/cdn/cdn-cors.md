---
title: aaaUsing Azure CDN med CORS | Microsoft Docs
description: "Lär dig hur toouse hello Azure Content Delivery Network (CDN) toowith Cross-Origin Resource Sharing (CORS)."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 86740a96-4269-4060-aba3-a69f00e6f14e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 6c743b56c32a2d3aacc9a77094cb87d61b95d2f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-cdn-with-cors"></a>Med hjälp av Azure CDN med CORS
## <a name="what-is-cors"></a>Vad är CORS?
CORS (Cross Origin Resource Sharing) är en HTTP-funktion som gör att ett webbprogram som körs i en domän tooaccess resurser i en annan domän. I ordning tooreduce hello risken för webbplatser scripting angrepp, alla moderna webbläsare för att implementera säkerhetsbegränsning kallas [samma ursprung princip](http://www.w3.org/Security/wiki/Same_Origin_Policy).  Detta förhindrar att en webbsida från anropa API: er i en annan domän.  CORS ger ett säkert sätt tooallow ett ursprung (hello ursprungsdomän) toocall API: er i ett ursprung.

## <a name="how-it-works"></a>Hur det fungerar
Det finns två typer av CORS-förfrågningar, *enkla begäranden* och *komplexa begäranden.*

### <a name="for-simple-requests"></a>För enkla begäranden:

1. hello webbläsaren skickar hello CORS-begäran med en ytterligare **ursprung** HTTP-huvud. hello-värdet för det här sidhuvudet är hello ursprung som betjänats hello överordnad sida som har definierats som hello kombination av *protokoll,* *domän,* och *port.*  En sida från https://www.contoso.com försöker tooaccess användardata i hello fabrikam.com ursprung, skickas hello följande huvudet i begäran toofabrikam.com:

   `Origin: https://www.contoso.com`

2. hello servern svarar med hello följande:

   * En **Access Control-Tillåt-ursprung** rubrik i sitt svar som anger vilken plats ursprung är tillåtna. Exempel:

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * En HTTP-felkod: till exempel 403 om hello server inte tillåter hello cross-origin begäran när du har kontrollerat hello ursprung sidhuvud

   * En **Access Control-Tillåt-ursprung** huvud med jokertecken som gör att alla ursprung:

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a>För komplexa begäranden:

En komplex begäran är en begäran om CORS där hello webbläsare krävs toosend en *preflight-begäran* (d.v.s. en preliminär avsökning) innan du skickar hello faktiska CORS-begäran. hello preflight-begäran frågar hello server behörighet om hello ursprungliga CORS-begäran kan fortsätta och är en `OPTIONS` begära toohello samma URL.

> [!TIP]
> Visa mer information om CORS-flöden och vanliga fallgropar hello [Guide tooCORS för REST API: er](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).
>
>

## <a name="wildcard-or-single-origin-scenarios"></a>Jokertecken eller enskild ursprung scenarier
CORS i Azure CDN fungerar automatiskt med ingen ytterligare konfiguration när hello **Access Control-Tillåt-ursprung** huvud är inställt toowildcard (*) eller ett enda ursprung.  hello CDN cachelagrar hello första svar och efterföljande begäranden använder hello samma sidhuvud.

Om begäran har redan gjorts toohello CDN tidigare tooCORS anges på hello din ursprung, behöver du toopurge innehåll på din slutpunkt innehåll tooreload hello innehåll med hello **Access Control-Tillåt-ursprung** huvud.

## <a name="multiple-origin-scenarios"></a>Scenarier med flera ursprung
Om du behöver tooallow en specifik lista över ursprung toobe tillåts för CORS saker lite mer komplicerad. hello problemet uppstår när hello CDN cachelagrar hello **Access Control-Tillåt-ursprung** huvud för hello första CORS-ursprunget.  När en annan CORS-ursprunget gör en efterföljande begäran, hello CDN fungerar hello cachelagrade **Access Control-Tillåt-ursprung** rubriken, som inte matchar.  Det finns flera sätt toocorrect detta.

### <a name="azure-cdn-premium-from-verizon"></a>Azure CDN Premium från Verizon
Hej bästa sätt tooenable är toouse **Azure CDN Premium från Verizon**, vilket visar att vissa avancerade funktioner. 

Du behöver för[skapa en regel](cdn-rules-engine.md) toocheck hello **ursprung** sidhuvud på hello-begäran.  Om det är ett giltigt ursprung din regel kommer att ange hello **Access Control-Tillåt-ursprung** huvud med hello ursprung i hello-begäran.  Om hello ursprung har angetts i hello **ursprung** huvud är inte tillåtet, regeln bör utelämna hello **Access Control-Tillåt-ursprung** huvudet som gör att hello webbläsarbegäran tooreject hello. 

Det finns två sätt toodo detta med hello regelmotor.  I båda fallen hello **Access Control-Tillåt-ursprung** huvudet från hello filens ursprungsserver ignoreras helt, hello CDN regelmotor helt hanterar hello tillåtna CORS ursprung.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Ett reguljärt uttryck med alla giltiga ursprung
I det här fallet ska du skapa ett reguljärt uttryck som innehåller alla hello ursprung önskade tooallow: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> **Azure CDN från Verizon** använder [Perl kompatibel reguljära uttryck](http://pcre.org/) som motorn för reguljära uttryck.  Du kan använda ett verktyg som [reguljära uttryck 101](https://regex101.com/) toovalidate reguljära uttryck.  Observera att hello ”/” tecken är giltig i reguljära uttryck och behöver inte toobe undantaget, men undantagstecken tecknet anses vara bästa praxis och förväntas av vissa regex verifierare.
> 
> 

Om hello reguljära uttryck matchar, ersätts regeln hello **Access Control-Tillåt-ursprung** huvud (eventuella) från hello ursprung med hello ursprung som skickade hello-begäran.  Du kan också lägga till ytterligare CORS-huvuden som **-kontroll-Tillåt-åtkomstmetoder**.

![Exempel på regler med reguljära uttryck](./media/cdn-cors/cdn-cors-regex.png)

#### <a name="request-header-rule-for-each-origin"></a>Begäran huvud-regel för varje ursprung.
I stället för reguljära uttryck kan du istället skapa en separat regel för varje ursprung du vill använda hello tooallow **begära huvud med jokertecken** [matchar villkoret](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1). Som med hello reguljärt uttryck metoden motorn hello regler fristående anger hello CORS huvuden. 

![Regler som exempel utan reguljärt uttryck](./media/cdn-cors/cdn-cors-no-regex.png)

> [!TIP]
> I hello-exemplet ovan, hello användning av jokertecken hello * visar hello regler motorn toomatch både HTTP och HTTPS.
> 
> 

### <a name="azure-cdn-standard"></a>Standard Azure CDN
På Azure CDN Standard profiler hello mekanism tooallow för flera ursprung utan hello hello jokertecken ursprung är toouse [fråga cachelagring av frågesträngar](cdn-query-string.md).  Du måste ha tooenable frågan sträng inställningen för hello CDN-slutpunkten och sedan använda en unik frågesträng för begäranden från alla domäner som är tillåtna. Detta resulterar i hello CDN cachelagring ett separat objekt för varje unik frågesträng. Den här metoden är inte idealiskt, men eftersom den resulterar i flera kopior av hello samma fil cachelagrade på hello CDN.  

