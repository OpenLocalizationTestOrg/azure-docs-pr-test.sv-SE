---
title: "Stöd för aaaCross-Origin Resource delning (CORS) | Microsoft Docs"
description: "Lär dig hur tooenable CORS-stöd för hello Microsoft Azure Storage-tjänster."
services: storage
documentationcenter: .net
author: cbrooksmsft
manager: carmonm
editor: tysonn
ms.assetid: a0229595-5b64-4898-b8d6-fa2625ea6887
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 2/22/2017
ms.author: cbrooks
ms.openlocfilehash: 0a6ec3bf6999c5faa7f0912dc2a47921aa01d3d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cross-origin-resource-sharing-cors-support-for-hello-azure-storage-services"></a>Stöd för Cross-Origin Resource Sharing (CORS) för hello Azure Storage-tjänster
Från och med version 2013-08-15, stöd hello Azure-lagringstjänster för Cross-Origin Resource Sharing (CORS) för hello Blob, tabell, kö och filen tjänster. CORS är en HTTP-funktion som gör att ett webbprogram som körs i en domän tooaccess resurser i en annan domän. Webbläsare som implementerar en säkerhetsbegränsningar som kallas [samma ursprung princip](http://www.w3.org/Security/wiki/Same_Origin_Policy) som förhindrar att en webbsida från anropa API: er i en annan domän. CORS ger ett säkert sätt tooallow en domän (hello ursprungsdomän) toocall API: er i en annan domän. Se hello [CORS-specifikationen](http://www.w3.org/TR/cors/) information om CORS.

Du kan ange CORS-regler individuellt för varje hello storage-tjänster genom att anropa [ange egenskaper för Blob-tjänsten](https://msdn.microsoft.com/library/hh452235.aspx), [ange egenskaper för kön Service](https://msdn.microsoft.com/library/hh452232.aspx), och [ange tjänsten Tabellegenskaper](https://msdn.microsoft.com/library/hh452240.aspx). När du har angett hello CORS-regler för hello tjänsten kommer en korrekt autentiserad begäran mot hello tjänsten från en annan domän att utvärderade toodetermine om det är tillåtet enligt toohello regler som du har angett.

> [!NOTE]
> Observera att CORS inte är en autentiseringsmetod. Alla förfrågningar som görs mot en lagringsresurs när CORS är aktiverat måste antingen ha en korrekt autentisering signatur eller måste göras mot en offentlig resurs.
> 
> 

## <a name="understanding-cors-requests"></a>Förstå CORS-begäranden
En CORS-begäran från en ursprungsdomän kan bestå av två separata begäranden:

* En preflight-begäran som frågar hello CORS begränsningar av hello-tjänsten. hello preflight-begäran krävs om hello begäran metoden är en [enkel metod](http://www.w3.org/TR/cors/), vilket innebär att GET, HEAD eller POST.
* hello faktiska begäran görs mot hello önskade resurs.

### <a name="preflight-request"></a>Preflight-begäran
hello preflight-begäran frågor hello CORS begränsningar som har upprättats för hello lagring av hello Kontoägare. hello webbläsare (eller andra användaragent) skickar en alternativ-begäran som innehåller hello begärandehuvuden, metod och ursprung domän. hello lagringstjänsten utvärderar hello avsedda åtgärden baserat på en förkonfigurerad uppsättning CORS-regler som anger vilka ursprungsdomäner och begära metoder och begärandehuvuden kan anges för en faktiska begäran mot en lagringsresurs.

Om CORS har aktiverats för hello-tjänsten och det finns en CORS-regel som matchar hello preflight-begäran, hello-tjänsten svarar med statuskod 200 (OK) och inkluderar hello krävs åtkomstkontroll rubriker hello svar.

Om CORS inte har aktiverats för hello tjänsten eller inga CORS-regeln matchar hello preflight-begäran, svarar hello-tjänsten med statuskod 403 (förbjuden).

Om hello OPTIONS-begäran inte innehåller hello krävs CORS huvuden (hello ursprung och --begäran-metoden för åtkomstkontroll huvuden), svarar hello-tjänsten med statuskod: 400 (felaktig begäran).

Observera att en preflight-begäran ska utvärderas mot hello-tjänsten (Blob, köer och tabell) och inte mot hello begärd resurs. Hej kontoägaren måste ha aktiverat CORS som en del av hello service egenskaper för hello begäran toosucceed.

### <a name="actual-request"></a>Faktiska begäran
När hello preflight-begäran har godkänts och hello svar returneras skickar hello webbläsaren hello faktiska begäran mot hello storage-resursen. hello webbläsare kommer att neka hello faktiska begäran omedelbart om hello preflight-begäran avvisas.

hello faktiska begäran behandlas som normal begäran mot hello storage-tjänst. hello förekomst av hello ursprung huvudet anger att hello-begäran är en CORS-begäran och hello tjänsten kontrollerar hello CORS-regler för matchning. Om en matchning hittas hello åtkomstkontroll huvuden tillagda toohello svar och skickas tillbaka toohello klienten. Om en matchning hittas returneras inte hello CORS-åtkomstkontroll huvuden.

## <a name="enabling-cors-for-hello-azure-storage-services"></a>Aktivera CORS för hello Azure Storage-tjänster
CORS-regler är inställda på hello servicenivå, så du behöver tooenable eller inaktivera CORS för varje tjänst (Blob, köer och tabellen) separat. Som standard inaktiveras CORS för varje tjänst. tooenable CORS måste tooset hello lämpliga egenskaper med version 2013-08-15 eller senare, och Lägg till CORS regler toohello tjänstegenskaper. Mer information om hur tooenable eller inaktivera CORS för en tjänst och hur tooset CORS regler, kontrollera finns för[ange egenskaper för Blob-tjänsten](https://msdn.microsoft.com/library/hh452235.aspx), [ange egenskaper för kön Service](https://msdn.microsoft.com/library/hh452232.aspx), och [ange tabell Egenskaper](https://msdn.microsoft.com/library/hh452240.aspx).

Här är ett exempel på en enda CORS-regel som angetts via en åtgärden Ange egenskaper för tjänsten:

```xml
<Cors>    
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
        <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
        <MaxAgeInSeconds>200</MaxAgeInSeconds>
    </CorsRule>
<Cors>
```

Varje element som ingår i hello CORS regeln beskrivs nedan:

* **AllowedOrigins**: hello ursprungsdomäner som tillåts toomake en begäran mot hello storage-tjänsten via CORS. hello är ursprung hello-domän som begäran kommer från vilka hello. Observera att hello ursprung måste vara en exakt skiftlägeskänslig matchning med hello ursprung att hello användaren ålder skickar toohello service. Du kan också använda hello jokertecknet ' *' tooallow alla ursprung domäner toomake begäranden via CORS. Hej domäner i hello-exemplet ovan, [http://www.contoso.com](http://www.contoso.com) och [http://www.fabrikam.com](http://www.fabrikam.com) kan göra förfrågningar mot hello-tjänsten med hjälp av CORS.
* **AllowedMethods**: hello metoder (http-begäran verb) hello ursprungsdomänen kan använda för en CORS-begäran. I hello-exemplet ovan tillåts bara PUT- och GET-begäranden.
* **AllowedHeaders**: hello begärandehuvuden hello ursprungsdomänen får ange på hello CORS-begäran. Alla metadata huvuden som börjar med x-ms-metadata, x-ms-meta-mål- och x-ms-meta-abc i hello exemplet ovan är tillåtna. Observera att hello jokertecknet ' *' anger att alla huvud som börjar med hello angetts prefix är tillåtet.
* **ExposedHeaders**: hello svarshuvuden som kan skickas i hello svar toohello CORS begäran och visas av hello webbläsare toohello begäran utfärdare. Hello-exemplet ovan, hello webbläsaren är instrueras tooexpose alla huvud som börjar med x-ms-metadata.
* **MaxAgeInSeconds**: hello längsta tid att webbläsaren ska cachelagrar hello preliminära OPTIONS-begäran.

hello Azure-lagringstjänster stöd för att ange prefix huvuden för båda hello **AllowedHeaders** och **ExposedHeaders** element. Du kan ange en gemensam prefixet toothat kategori tooallow en kategori av huvuden. Ange till exempel *x-ms-meta** som ett prefix huvud upprättar en regel som matchar alla huvuden som börjar med x-ms-metadata.

hello följande begränsningar gäller tooCORS regler:

* Du kan ange upp toofive CORS-regler per lagringstjänsten (Blob, tabeller och köer).
* hello maximal storlek för alla CORS regler inställningar på hello begäran, exklusive XML-taggar får inte överstiga 2 KB.
* hello en tillåtna sidhuvud, exponerade sidhuvud eller tillåtna ursprung kan inte vara längre än 256 tecken.
* Tillåtna sidhuvuden och exponerade huvuden kan vara antingen:
  * Literalen sidhuvud, där hello exakt huvudets namn tillhandahålls som **x-ms-meta-bearbetas**. Högst 64 literal huvuden kan anges på hello-begäran.
  * Prefixet sidhuvud, där prefixet hello sidhuvud tillhandahålls som **x-ms-metadata***. Ange ett prefix i det här sättet tillåter eller visar de huvuden som börjar med hello angivna prefix. Högst två prefix huvuden kan anges på hello-begäran.
* Hej metoder (eller HTTP-verb) som angetts i hello **AllowedMethods** element måste uppfylla toohello metoder som stöds av Azure storage-tjänst API: er. Metoder som stöds är DELETE, GET, HEAD, MERGE, POST, alternativ och PUT.

## <a name="understanding-cors-rule-evaluation-logic"></a>Förstå CORS regellogik utvärdering
När storage-tjänst tar emot en begäran preliminära eller faktiska, utvärderar begäran baserat på hello CORS-regler som du har skapat för hello-tjänsten via hello lämpliga egenskaper för tjänsten igen. CORS-regler utvärderas i hello ordning som de har ställts in i hello begäran brödtext hello åtgärden Ange egenskaper för tjänsten.

CORS-regler utvärderas enligt följande:

1. Först hello ursprungsdomän hello begäran kontrolleras mot hello-domäner som anges för hello **AllowedOrigins** element. Om hello ursprungsdomän ingår i listan hello eller alla domäner tillåts med hello jokertecknet ' *', regler utvärdering fortsätter. Om hello ursprungsdomän inte är inkluderad misslyckas hello begäran.
2. Därefter hello-metoden (eller HTTP-verb) för hello begäran kontrolleras mot hello metoder som anges i hello **AllowedMethods** element. Om hello metoden ingår i hello listan, fortsätter regler utvärdering; annars misslyckas hello-begäran.
3. Om hello-begäran matchar en regel i sin ursprungliga domän och dess metod, regeln är valda tooprocess hello begäran och inga ytterligare regler utvärderas. Innan hello begäran kan fungera dock huvuden som anges i begäran Hej kontrolleras mot hello rubriker som finns i hello **AllowedHeaders** element. Hello begäran misslyckas om hello tillåtna huvuden inte matchar hello-huvuden som skickas.

Eftersom hello regler bearbetas i hello ordning de befinner sig i begärandetexten för hello rekommenderar bästa praxis att ange hello mest restriktiva regler med hänsyn till tooorigins först i hello listan så att dessa utvärderas först. Ange regler som är mindre restriktivt – till exempel en regel tooallow alla ursprung – hello slutet av hello-listan.

### <a name="example--cors-rules-evaluation"></a>Exempel – CORS regler utvärdering
hello visas följande exempel en partiell begärantext för en åtgärd tooset CORS-regler för hello storage-tjänster. Se [ange egenskaper för Blob-tjänsten](https://msdn.microsoft.com/library/hh452235.aspx), [ange egenskaper för kön Service](https://msdn.microsoft.com/library/hh452232.aspx), och [ange egenskaper för tabellen Service](https://msdn.microsoft.com/library/hh452240.aspx) information om hur du skapar hello begäran.

```xml
<Cors>
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
        <AllowedMethods>PUT,HEAD</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
    </CorsRule>
    <CorsRule>
        <AllowedOrigins>*</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
    </CorsRule>
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
        <AllowedMethods>GET</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
    </CorsRule>
</Cors>
```

Överväg därefter hello följande CORS begäranden:

| Förfrågan |  |  | Svar |  |
| --- | --- | --- | --- | --- |
| **Metoden** |**Ursprung** |**Huvuden för begäran** |**Regeln matchar** |**Resultat** |
| **PLACERA** |http://www.contoso.com |x-ms-blob-content-type |Första regeln |Lyckades |
| **HÄMTA** |http://www.contoso.com |x-ms-blob-content-type |Andra regeln |Lyckades |
| **HÄMTA** |http://www.contoso.com |x-ms-client-request-id |Andra regeln |Fel |

hello första begäran matchar hello första regeln – hello ursprungsdomän matchar hello tillåtna ursprung, hello metoden matchar hello tillåtna metoder och hello sidhuvud matchar hello tillåtna huvuden – och så lyckas.

andra hello-begäran matchar inte hello första regeln eftersom hello-metod inte matchar hello tillåtna metoder. Den, men matchar hello andra regeln, så den lyckas.

hello tredje begäran matchar hello andra regeln i dess ursprungsdomän och metod, så inga ytterligare regler utvärderas. Dock hello *x-ms-client-request-id-huvudet* tillåts inte av hello andra regeln, så hello begäran misslyckas trots hello att hello semantiken för hello tredje regeln skulle ha gjort det toosucceed.

> [!NOTE]
> Även om det här exemplet visar en mindre begränsande regel innan en mer begränsande i allmänhet hello bästa praxis är toolist hello mest restriktiva regler först.
> 
> 

## <a name="understanding-how-hello-vary-header-is-set"></a>Förstå hur hello Vary-huvud har angetts
Hej *variera* huvud är en standard HTTP/1.1-rubrik som består av en uppsättning begäran huvudfält som meddela hello webbläsare eller användaren agent om hello villkor som valts av hello server tooprocess hello begäran. Hej *variera* huvud används huvudsakligen för cachelagring av proxyservrar, webbläsare och andra CDN-lösningar, som använder den toodetermine hur hello svaret ska cachelagras. Mer information finns i hello-specifikationen för hello [Vary-huvud](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

När hello webbläsare eller en annan användaragent cachelagrar hello svar från en CORS-begäran, cachelagras hello ursprungsdomän som hello tillåtna ursprung. När en andra domänproblem Hej samma begäran om en lagringsresurs medan hello cache är aktiv, hello användaragent hämtar hello cachelagrade ursprungsdomän. andra hello-domänen matchar inte hello cachelagrade domän, så hello begäran misslyckas när den annars skulle lyckas. I vissa fall, Azure Storage anger hello Vary-huvud för**ursprung** tooinstruct hello användaren toosend hello efterföljande CORS begäran toohello agenttjänsten om hello begär domän skiljer sig från hello cachelagras ursprung.

Azure-lagring anger hello *variera* huvud för**ursprung** för faktiska GET/HEAD-begäranden i hello följande fall:

* När hello begäran ursprung matchar exakt hello tillåtna ursprung som definieras av en regel för CORS. toobe en exakt matchning hello CORS regeln får inte innehålla ett jokertecken ' * ' tecken.
* Det finns ingen regel matchande hello begäran ursprung, men CORS är aktiverat för hello storage-tjänst.

I hello fall där en GET/HEAD-begäran matchar en CORS-regel som tillåter alla ursprung tyder hello svar på att alla ursprung är tillåtna hello användarcachen agent kan efterföljande förfrågningar från alla ursprungsdomän medan hello cache är aktiv.

Observera att för begäranden med andra metoder än GET/HEAD hello lagringstjänster inte ska ange hello Vary-huvud, eftersom svar toothese metoder inte cachelagras av användaragenter.

hello följande tabell visar hur Azure storage svarar tooGET/HEAD-begäranden baserat på hello tidigare nämnts fall:

| Förfrågan | Kontoinställningen och resultat av utvärderingen av distributionsregeln |  |  | Svar |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Ursprung huvudet finns på begäran** |**CORS-regler som angetts för denna tjänst** |**Det finns matchande regel som tillåter alla ursprung (*)** |**Matchande regel som finns för ursprung exakt matchning** |**Svaret innehåller Vary-huvud set tooOrigin** |**Svaret innehåller Access Control-tillåtna-ursprung ”:*”** |**Svaret innehåller Access-Control-exponeras-rubriker** |
| Nej |Nej |Nej |Nej |Nej |Nej |Nej |
| Nej |Ja |Nej |Nej |Ja |Nej |Nej |
| Nej |Ja |Ja |Nej |Nej |Ja |Ja |
| Ja |Nej |Nej |Nej |Nej |Nej |Nej |
| Ja |Ja |Nej |Ja |Ja |Nej |Ja |
| Ja |Ja |Nej |Nej |Ja |Nej |Nej |
| Ja |Ja |Ja |Nej |Nej |Ja |Ja |

## <a name="billing-for-cors-requests"></a>Fakturering för CORS-begäranden
Lyckad preflight begär debiteras om du har aktiverat CORS för någon av hello lagringstjänster för ditt konto (genom att anropa [ange egenskaper för Blob-tjänsten](https://msdn.microsoft.com/library/hh452235.aspx), [ange egenskaper för kön Service](https://msdn.microsoft.com/library/hh452232.aspx), eller [ Ange egenskaper för tabellen](https://msdn.microsoft.com/library/hh452240.aspx)). toominimize kostnader, Överväg att ange hello **MaxAgeInSeconds** element i din CORS regler tooa stora värden så att hello användaragent cachelagrar hello-begäran.

Kommer inte att debiteras misslyckade preflight-begäranden.

## <a name="next-steps"></a>Nästa steg
[Ange egenskaper för Blob-tjänst](https://msdn.microsoft.com/library/hh452235.aspx)

[Ange egenskaper för kön](https://msdn.microsoft.com/library/hh452232.aspx)

[Ange egenskaper för tabellen](https://msdn.microsoft.com/library/hh452240.aspx)

[W3C Cross-Origin Resource Sharing specifikation](http://www.w3.org/TR/cors/)

