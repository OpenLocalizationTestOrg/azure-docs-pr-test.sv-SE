---
title: aaaAzure CDN regler motorn funktioner | Microsoft Docs
description: "I referensdokumentationen för Azure CDN regler motorn matchar villkoren och funktioner."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: c10b8ef58e3d209b12fbb0ac2173e1ca51ff7538
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cdn-rules-engine-features"></a>Azure CDN regler motorn funktioner
Det här avsnittet innehåller detaljerade beskrivningar av hello tillgängliga funktioner för Azure Content Delivery Network (CDN) [regelmotor](cdn-rules-engine.md).

hello tredje del av en regel är hello-funktion. En funktion definierar hello typ av åtgärd som ska tillämpas toohello typ av begäran med en uppsättning villkor för matchning.

## <a name="access"></a>Åtkomst

Dessa funktioner är utformade toocontrol åtkomst toocontent.


Namn | Syfte
-----|--------
Neka åtkomst | Anger om alla begäranden avvisas med ett 403 förbjuden svar.
Token Auth | Anger om Token-baserad autentisering ska vara tillämpade tooa begäran.
Token Auth DOS-kod | Anger hello typ av svar som ska returneras tooa användare när en begäran nekas på grund av tooToken-baserad autentisering.
Token Auth Ignorera skiftläge för URL | Anger om URL: en jämförelser av Token-baserad autentisering ska vara skiftlägeskänslig.
Tokenparameter Auth | Anger om hello-tokenbaserad autentisering frågesträngparametern ska ändras.

### <a name="deny-access"></a>Neka åtkomst
**Syfte**: Anger om alla begäranden avvisas med ett 403 förbjuden svar.

Värde | Resultat
------|-------
Enabled| Gör att alla begäranden som uppfyller hello matchande kriterier toobe nekas med ett 403 förbjuden svar.
Disabled| Återställer hello standardbeteendet. hello standardbeteendet är tooallow hello ursprung toodetermine hello servertyp av svar som ska returneras.

**Standardbeteende**: inaktiverad

> [!TIP]
   > En möjlig användning för den här funktionen är tooassociate den med ett huvud för begäran matchar villkoret tooblock åtkomst tooHTTP referenter som använder infogade länkar tooyour innehåll.

### <a name="token-auth"></a>Token Auth
**Syfte:** anger om Token-baserad autentisering ska vara tillämpade tooa begäran.

Om Token-baserad autentisering är aktiverad hanteras endast begäranden som tillhandahåller en krypterad token och uppfyller toohello krav som anges av säkerhetstoken.

hello krypteringsnyckel som kommer att använda tooencrypt och dekryptera token värdena bestäms av den primärnyckeln och alternativ för säkerhetskopiering nyckel på sidan Token Auth. Tänk på att krypteringsnycklarna är plattformsspecifika.

Värde | Resultat
------|---------
Enabled | Skyddar hello begärt innehåll med Token-baserad autentisering. Endast begäranden från klienter som anger en giltig token och uppfyller dess krav att användas. FTP-transaktioner är undantagna från Token-baserad autentisering.
Disabled| Återställer hello standardbeteendet. Hej standardbeteendet är tooallow din Token-baserad autentisering configuration toodetermine om en begäran som ska skyddas.

**Standardbeteende:** inaktiverad.

###<a name="token-auth-denial-code"></a>Token Auth DOS-kod
**Syfte:** anger hello typ av svar som ska returneras tooa användare när en begäran nekas på grund av tooToken-baserad autentisering.

hello tillgängliga svarskoder visas nedan.

Svarskod|Svaret namn|Beskrivning
----------------|-----------|--------
301|Flytta permanent|Den här statuskoden omdirigerar obehöriga användare toohello URL som anges i huvudet plats.
302|Hitta|Den här statuskoden omdirigerar obehöriga användare toohello URL som anges i huvudet plats. Den här statuskoden är hello standardmetod för att utföra en omdirigering.
307|Tillfällig omdirigering|Den här statuskoden omdirigerar obehöriga användare toohello URL som anges i huvudet plats.
401|Behörighet saknas|Kombinera den här statuskoden med de svar WWW-Authenticate-huvud kan du tooprompt en användare för autentisering.
403|Tillåts inte|Detta är standard 403 förbjuden status hälsningsmeddelande att en obehörig användare kommer att se när försök tooaccess skyddat innehåll.
404|Filen hittades inte|Den här statuskoden anger att hello HTTP-klienten har kan toocommunicate med hello server men hello begärda innehållet inte hittades.

#### <a name="url-redirection"></a>URL-omdirigering

Den här funktionen har stöd för användardefinierade URL-URL: en omdirigering tooa när den är konfigurerad tooreturn statuskod 3xx. URL: en användardefinierad kan anges genom att utföra hello följande steg:

1. Välj en 3xx svarskod för hello Auth Denial Tokenkod funktion.
2. Välj alternativet för valfritt namn för huvudet ”plats”.
3. Ange URL: en valfri huvudvärde alternativet toohello som önskas.

Om en URL inte har definierats för en 3xx statuskod, sedan returneras hello standard svar sida för en 3xx statuskod toohello användare.

URL-omdirigering gäller endast för 3xx svarskoder.

Alternativet valfritt huvudvärde stöder alfanumeriska tecken, citattecken och blanksteg.

#### <a name="authentication"></a>Autentisering

Den här funktionen stöder hello kapaciteten tooinclude WWW-autentisera huvud när svarar tooan otillåten begäran om innehåll som skyddas av Token-baserad autentisering. Om WWW-Authenticate-huvud har ställts in för ”basic” i konfigurationen, uppmanas hello obehöriga användare för autentiseringsuppgifter.

hello ovan konfiguration kan uppnås genom att utföra hello följande steg:

1. Välj ”401” som hello svarskod hello Tokenkod för Auth-DOS-funktionen.
2. Välj ”WWW autentisera” alternativet valfria huvudets namn.
3. Ange alternativ för valfritt värde för ”grundläggande”.

WWW-Authenticate-huvud gäller endast för 401 svarskoder.

### <a name="token-auth-ignore-url-case"></a>Token Auth Ignorera skiftläge för URL
**Syfte:** anger om URL: en jämförelser av Token-baserad autentisering ska vara skiftlägeskänslig.

hello-parametrar som påverkas av den här funktionen är:

- ec_url_allow
- ec_ref_allow
- ec_ref_deny

Giltiga värden är:

Värde|Resultat
---|----
Enabled|Gör våra gränsservern tooignore fallet när URL: er för tokenbaserad autentiseringsparametrar.
Disabled|Återställer hello standardbeteendet. hello standardbeteendet är för URL: en jämförelse för Tokenautentisering toobe skiftlägeskänsligt.

**Standardbeteende:** inaktiverad.
 
### <a name="token-auth-parameter"></a>Tokenparameter Auth
**Syfte:** avgör om hello-tokenbaserad autentisering frågesträngparametern ska ändras.

Viktig information:

- Alternativet värde definierar hello parameternamn för frågesträng som en token kan anges.
- Alternativet värde kan inte anges för ”ec_token”.
- Kontrollera att hello namn har definierats i alternativet värde 
- innehåller giltiga tecken i URL: en.

Värde|Resultat
----|----
Enabled|Alternativet värde definierar hello parameternamn för frågesträng som token ska definieras.
Disabled|En token kan anges som ett odefinierat frågesträngparametern i hello begärd URL.

**Standardbeteende:** inaktiverad. En token kan anges som ett odefinierat frågesträngparametern i hello begärd URL.

## <a name="caching"></a>Cachelagring

Dessa funktioner är utformade toocustomize när och hur innehållet cachelagras.

Namn | Syfte
-----|--------
Parametrar för bandbredd | Anger om bandbreddsbegränsning parametrarna (d.v.s. ec_rate och ec_prebuf) ska vara aktiv.
Begränsning av bandbredd | Begränsar hello bandbredd för hello-svar som tillhandahålls av våra edge-servrar.
Kringgå Cache | Anger om hello begäran kan utnyttja våra teknik för cachelagring.
Cache-Control sidhuvud behandling | Kontroller hello generering av Cache-Control-huvuden av hello edge-server när externa maximal ålder funktionen är aktiv.
Cachenyckel frågesträng | Anger om hello cachenyckel ska inkludera eller exkludera frågan string-parametrar som är associerade med en begäran.
Omarbetning av cache-nyckel | Skriver om hello cache-nyckeln som associeras med en begäran.
Slutföra Cache Fill | Anger vad som händer när en begäran resulterar i en partiell cache-miss på edge-server.
Komprimera filtyper | Definierar hello-filformat som ska komprimeras på hello-servern.
Internt Max-åldern som standard | Anger hello maximal ålder för standardintervallet för edge server tooorigin server Cachevalidering.
Upphör att gälla sidhuvud behandling | Kontroller hello generering av Expires-huvuden genom en gränsserver när hello externa maximal ålder funktionen är aktiv.
Externa maximal ålder | Anger intervallet för hello maximal ålder för webbläsaren tooedge server Cachevalidering.
Tvinga inre maximal ålder | Anger intervallet för hello maximal ålder för edge server tooorigin server Cachevalidering.
H.264-stöd (http-progressiv nedladdning) | Anger hello typer av H.264 filformat som kanske används toostream innehåll.
Kontrollera No-Cache-begäran | Anger om en HTTP-klientens no-cache-begäranden ska vidarebefordras toohello ursprungsservern.
Ignorera ursprung No-Cache | Anger om våra CDN ignoreras vissa direktiv som hanteras från en ursprungsservern.
Ignorera Unsatisfiable intervall | Anger hello-svar som ska returneras tooclients när en förfrågan genererar statuskod 416 begärt intervall inte kunde hanteras.
Internt Max-inaktuell | Anger hur länge tidigare hello normal förfallotid cachelagrade tillgång kan hanteras från en gränsserver när hello edge-server är toorevalidate hello cachelagrade tillgång med hello ursprungsservern.
Partiell Cache delning | Anger om en begäran kan generera delvis cachelagrat innehåll.
Prevalidate cachelagrat innehåll | Anger om cachelagrat innehåll ska vara tillämplig för tidig validering innan TTL-upphör att gälla.
Uppdatera noll Byte cachefiler | Anger hur en HTTP-klientens begäran för en tillgång 0 byte cache hanteras av våra edge-servrar.
Ange Cacheable ställs statuskoder | Definierar hello statuskoder som kan resultera i cachelagrat innehåll.
Inaktuella leverans av innehåll vid fel | Avgör om upphört att gälla cachelagrat innehåll levereras när ett fel uppstår under Cachevalidering eller när hämtning hello begär innehåll från hello kunden ursprungsservern.
Inaktuella när Revalidate | Förbättrar prestanda genom att tillåta att våra servrar kant tooserve inaktuell klient toohello beställaren medan Omverifiering äger rum.
Kommentar | hello kommentarfunktionen kan en anteckning toobe som lagts till i en regel.

###<a name="bandwidth-parameters"></a>Parametrar för bandbredd
**Syfte:** anger om bandbreddsbegränsning parametrarna (d.v.s. ec_rate och ec_prebuf) ska vara aktiv.

Bandbredd bandbreddsbegränsning parametrarna avgör om hello överföringshastighet för begäran om en klient ska vara begränsad tooa anpassade hastighet.

Värde|Resultat
--|--
Enabled|Tillåter våra servrar edge toohonor bandbreddsbegränsning begäranden.
Disabled|Gör att våra servrar edge tooignore bandbreddsbegränsning parametrar. hello begärt innehållet ska tillhandahållas normalt (d.v.s. utan begränsning av bandbredd).

**Standardbeteende:** aktiverat.

###<a name="bandwidth-throttling"></a>Begränsning av bandbredd
**Syfte:** begränsas hello bandbredd för hello-svar som tillhandahålls av våra edge-servrar.

Båda av följande alternativ för hello måste vara definierade tooproperly konfigurera begränsning av bandbredd.

Alternativ|Beskrivning
--|--
Kilobyte per sekund|Ange det här alternativet toohello Maximal bandbredd (Kb per sekund) som kanske används toodeliver hello svar.
Prebuf sekunder|Ange det här alternativet toohello antalet sekunder som våra servrar edge väntar tills bandbreddsbegränsning bandbredd. hello syftet med den här tidsperioden obegränsad bandbredd är tooprevent mediaspelare från problem hackar eller buffring problem på grund av toobandwidth begränsning.

**Standardbeteende:** inaktiverad.

###<a name="bypass-cache"></a>Kringgå Cache
**Syfte:** avgör om hello begäran kan utnyttja våra teknik för cachelagring.

Värde|Resultat
--|--
Enabled|Gör alla begäranden toofall via toohello ursprungsservern, även om hello innehåll var tidigare cachelagrats på edge-servrar.
Disabled|Gör edge servrar toocache tillgångar enligt toohello princip som definierats i dess svarshuvuden.

**Standardbeteende:**

- **Stora HTTP:** inaktiverad

<!---
- **ADN:** Enabled
--->

###<a name="cache-control-header-treatment"></a>Cache-kontrollen sidhuvud behandling
**Syfte:** styr hello generering av Cache-Control-huvuden av hello edge-server när externa maximal ålder funktionen är aktiv.

Hej enklaste sättet tooachieve denna typ av konfiguration är tooplace hello externa maximal ålder och hello Cache-Control sidhuvud behandling funktioner i hello samma instruktion.

Värde|Resultat
--|--
Skriv över|Garanterar att hello följande åtgärder utförs:<br/> -Skriver över Cache-Control-huvudet som genererats av hello ursprungsservern. <br/>-Lägger till Cache-Control-huvudet som produceras av hello externa maximal ålder funktionen toohello svar.
Skicka vidare|Garanterar att Cache-Control-huvudet som produceras av hello externa maximal ålder funktionen aldrig har lagts till toohello svar. <br/> Om hello ursprungsservern producerar en Cache-Control-rubrik, skickas den via toohello slutanvändaren. <br/> Om hello ursprungsservern inte ger ett Cache-Control-huvud och sedan det här alternativet kan orsak hello svar huvud toonot innehålla en Cache-Control-rubrik.
Lägg till om de saknas|Om en Cache-Control-huvudet inte togs emot från hello ursprungsservern, läggs det här alternativet Cache-Control-huvudet som genereras av funktionen för hello externa maximal ålder. Det här alternativet är användbart för att säkerställa att alla tillgångar tilldelas en Cache-Control-rubrik.
Ta bort| Det här alternativet innebär att ett Cache-Control-huvud inte ingår hello huvudsvar. Om en Cache-Control-rubrik har redan tilldelats, sedan rensas den från hello huvudsvar.

**Standardbeteende:** skrivas över.

###<a name="cache-key-query-string"></a>Cachenyckel frågesträng
**Syfte:** avgör om cache-nyckel ska inkludera eller exkludera frågan string-parametrar som är associerade med en begäran.

Viktig information:

- Ange en eller flera frågan sträng parametern namn. Varje parameternamn måste avgränsas med ett blanksteg.
- Den här funktionen avgör om frågesträngparametrar ska inkluderas eller uteslutas från hello cache-nyckel. Ytterligare information har angetts för varje alternativ nedan.

Typ|Beskrivning
--|--
 Inkludera|  Anger att varje angiven parameter ska tas med i hello cache-nyckel. En unik cachenyckel genereras för varje begäran som innehåller ett unikt värde för en frågesträngsparameter som definierats i den här funktionen. 
 Omfatta alla  |Anger att en unik cachenyckel kommer att skapas för varje begäran tooan tillgång som innehåller en unik frågesträng. Denna typ av konfiguration rekommenderas vanligtvis inte eftersom det kan leda tooa liten procentandel cacheträffar. Detta ökar hello belastningen på hello ursprungsservern eftersom den har tooserve fler begäranden. Den här konfigurationen duplicerar hello cachelagring av frågesträngar som kallas ”unik-cachen” på sidan cachelagring av frågesträng. 
 Exkludera | Anger att endast hello angivna parametrar kommer att uteslutas från hello cache-nyckel. Alla andra frågan string-parametrar ska inkluderas i hello cache-nyckel. 
 Undanta alla  |Anger att alla frågeparametrar sträng kommer att uteslutas från hello cache-nyckel. Den här konfigurationen duplicerar hello standard cachelagring av frågesträngar som kallas ”standard-cache” på sidan cachelagring av frågesträng. 

hello kraften hos regelmotor för HTTP kan du toocustomize hello sätt där cachelagring av frågesträngar i fråga har implementerats. Du kan till exempel ange att frågan cachelagring av frågesträngar bara utföras på vissa platser eller filtyper.

Om du vill att tooduplicate hello cachelagring av frågesträng beteendet kallas ”no-cache” på sidan cachelagring av frågesträng, behöver du toocreate en regel som innehåller en URL-frågan med jokertecken matchar villkor och en kringgå Cache-funktion. hello med jokertecken URL-frågan matchar villkoret ska anges tooan asterisk (*).

#### <a name="sample-scenarios"></a>Exempelscenarier

Exempel för den här funktionen tillhandahålls nedan. Ett exempel på begäran och hello standard cache-nyckeln tillhandahålls nedan.

- **Exempel på begäran:** http://wpc.0001.&lt; Domän&gt;/800001/Origin/folder/asset.htm?sessionid=1234 och språk = EN & userid = 01
- **Standard cachenyckel:** /800001/Origin/folder/asset.htm

##### <a name="include"></a>Inkludera

Exempel på konfiguration:

- **Typ:** inkluderar
- **Parametrar:** språk

Den här typen av konfiguration skulle generera hello följande fråga sträng parametern cache-nyckel:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="include-all"></a>Omfatta alla

Exempel på konfiguration:

- **Typ:** omfattar alla

Den här typen av konfiguration skulle generera hello följande fråga sträng parametern cache-nyckel:

    /800001/Origin/folder/asset.htm?sessionid=1234&language=EN&userid=01

##### <a name="exclude"></a>Exkludera

Exempel på konfiguration:

- **Typ:** undanta
- **Parametrar:** sessionid användar-ID

Den här typen av konfiguration skulle generera hello följande fråga sträng parametern cache-nyckel:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="exclude-all"></a>Undanta alla

Exempel på konfiguration:

- **Typ:** utesluta alla

Den här typen av konfiguration skulle generera hello följande fråga sträng parametern cache-nyckel:

    /800001/Origin/folder/asset.htm

###<a name="cache-key-rewrite"></a>Omarbetning av cache-nyckel
**Syfte:** omskrivningar hello cachenyckel som är associerade med en begäran.

En cachenyckel är hello relativ sökväg som identifierar en tillgång för hello cachelagring. Våra servrar kommer med andra ord att söka efter en cachelagrad version av en tillgång enligt tooits sökväg som definierats av dess cache-nyckel.

Konfigurera den här funktionen genom att definiera både hello följande alternativ:

Alternativ|Beskrivning
--|--
Ursprungliga sökväg| Definiera hello relativ sökväg toohello typer av begäranden vars cachenyckel ska skrivas. En relativ sökväg kan definieras genom att välja en grundläggande ursprungssökväg och sedan definiera ett mönster för reguljärt uttryck.
Ny sökväg|Definiera hello relativ sökväg för hello ny cache-nyckel. En relativ sökväg kan definieras genom att välja en grundläggande ursprungssökväg och sedan definiera ett mönster för reguljärt uttryck. Den här relativa sökvägen kan konstrueras dynamiskt via hello användning av HTTP-variabler
**Standardbeteende:** en begäran cachenyckel bestäms av hello URI-begäran.

###<a name="complete-cache-fill"></a>Slutföra Cache Fill
**Syfte:** avgör vad som händer när en begäran resulterar i en partiell cache-miss på edge-server.

En partiell cache-miss beskriver hello Cachestatus för en tillgång som inte är helt hämtade tooan edge-server. Om en tillgång endast delvis cachelagras på edge-server, sedan vidarebefordras hello nästa begäran om tillgången igen toohello ursprungsservern.
<!---
This feature is not available for hello ADN platform. hello typical traffic on this platform consists of relatively small assets. hello size of hello assets served through these platforms helps mitigate hello effects of partial cache misses, since hello next request will typically result in hello asset being cached on that POP.
--->
En partiell cache-miss inträffar vanligtvis när en användare avbryter en hämtning eller för tillgångar som begärs endast med hjälp av HTTP-begäranden för intervallet. Den här funktionen är mest användbara för stora tillgångar där användare kommer inte vanligtvis hämta dem från start toofinish (t.ex. videor). Därför kan aktiveras den här funktionen som standard på hello stora http-plattformen. Den är inaktiverad på alla plattformar.

Det rekommenderas tooleave hello standardkonfigurationen för hello HTTP stora plattform, eftersom det minskar hello belastning på servern kunden ursprung och öka hello hastighet som dina kunder hämta ditt innehåll.

På grund av toohello sätt i vilken cache inställningar spåras, den här funktionen kan inte associeras med hello följande matchar villkor: Edge Cname, begär sidhuvud Literal, begär huvud med jokertecken, URL-frågan Literal och URL: en fråga med jokertecken.

Värde|Resultat
--|--
Enabled|Återställer hello standardbeteendet. hello standardbeteendet är tooforce hello edge server tooinitiate bakgrundshämtning hello tillgångsinformation från hello ursprungsservern. Efter vilken, hello tillgång ska vara i hello edge-serverns lokala cacheminne.
Disabled|Förhindrar att en gränsserver utför en bakgrundshämtning för hello tillgång. Det innebär att nästa hello-begäran för tillgången från den regionen kommer en kant server toorequest från hello kunden ursprungsservern.

**Standardbeteende:** aktiverat.

###<a name="compress-file-types"></a>Komprimera filtyper
**Syfte:** definierar hello-filformat som ska komprimeras på hello-servern.

Ett filformat kan anges med hjälp av dess Internet medietyp (d.v.s. Content-Type). Internet medietyp är plattformsoberoende metadata som gör att våra servrar tooidentify hello filformat för en viss tillgång. En lista över vanliga Internet medietyper finns nedan.

Internet-medietyp|Beskrivning
--|--
Text/plain|Filer med oformaterad text
text/html| HTML-filer
text/css|Sammanhängande formatmallar (CSS)
javascript-program/x|Javascript
program eller javascript|Javascript
Viktig information:

- Ange flera Internet-medietyper genom att avgränsa dem med ett blanksteg. 
- Den här funktionen Komprimera endast tillgångar vars storlek är mindre än 1 MB. Kommer inte att komprimera större tillgångar våra servrar.
- Vissa typer av innehåll, till exempel bilder, video och ljud tillgångar (t.ex., JPG, MP3, MP4, etc.) redan har komprimerats. Ytterligare komprimering på dessa typer av tillgångar kommer inte försämras avsevärt, filstorlek. Därför rekommenderas det att du inte aktivera komprimering på dessa typer av tillgångar.
- Jokertecken, till exempel asterisker stöds inte.
- Kontrollera att tooset alternativet inaktiverat komprimering på sidan komprimering för hello plattform toowhich regeln tillämpas innan du lägger till den här funktionen tooa regeln.

###<a name="default-internal-max-age"></a>Internt Max-åldern som standard
**Syfte:** avgör hello standardintervallet maximal ålder för edge server tooorigin server Cachevalidering. Med andra ord hello mängden tid som ska gå innan en gränsserver kontrollerar om en cachelagrad tillgång matchar hello-tillgångar lagras på hello ursprungsservern.

Viktig information:

- Den här åtgärden tar bara för svar från en ursprungsserver som inte tilldelar en beteckning maximal ålder i Cache-Control eller Expires-huvudet.
- Den här åtgärden kommer inte ske för tillgångar som inte anses Cacheable ställs.
- Den här åtgärden påverkar inte webbläsaren tooedge server cache revalidations. Dessa typer av revalidations bestäms av Cache-Control eller Expires-huvuden skickas toohello webbläsare, som kan anpassas med funktionen externa maximal ålder.
- hello resultaten av den här åtgärden har inte synliga påverkar hello svarshuvuden och hello innehåll som returneras från kant-servrar för ditt innehåll, men det kan påverka hello mängden Omverifiering av trafik som skickas från servrar tooyour ursprung gränsservern.
- Konfigurera den här funktionen genom att:
    - Att välja hello statuskod som en intern max-åldern som standard kan tillämpas.
    - Ange ett heltalsvärde och sedan välja hello önskad tidsenhet (d.v.s. sekunder, minuter, timmar, etc.). Det här värdet definierar hello interna maximal ålder standardintervallet.

- Inställningen hello tidsenhet för tilldelar ”Off” interna maximal ålder standardintervallet 7 dagar för begäranden som inte har tilldelats en maximal ålder indikation i sin Cache-Control eller Expires-huvudet.
- På grund av toohello sätt i vilken cache inställningar spåras, kan den här funktionen inte associeras med hello följande matchar villkor: 
    - Kant 
    - CNAME-post
    - Begäran sidhuvud Literal
    - Begäran huvud med jokertecken
    - Metod för begäran
    - URL-frågan Literal
    - URL: en fråga med jokertecken

**Standardvärde:** 7 dagar

###<a name="expires-header-treatment"></a>Upphör att gälla sidhuvud behandling
**Syfte:** styr hello generering av Expires-huvuden genom en gränsserver när funktionen externa maximal ålder är aktiv.

Hej enklaste sättet tooachieve denna typ av konfiguration är tooplace hello externa maximal ålder och hello upphör att gälla sidhuvud behandling funktioner i hello samma instruktion.

Värde|Resultat
--|--
Skriv över|Garanterar att hello följande åtgärder utförs:<br/>-Skriver över Expires-huvudet som genererats av hello ursprungsservern.<br/>-Lägger till Expires-huvudet som produceras av hello externa maximal ålder funktionen toohello svar.
Skicka vidare|Garanterar att Expires-huvudet som produceras av hello externa maximal ålder funktionen aldrig har lagts till toohello svar. <br/> Om hello ursprungsservern producerar Expires-rubriken, skickas de via toohello slutanvändaren. <br/>Om hello ursprungsservern inte ger Expires-rubriken och sedan det här alternativet kan orsak hello svar huvud toonot innehålla Expires-rubriken.
Lägg till om de saknas| Om Expires-rubriken inte togs emot från hello ursprungsservern, läggs det här alternativet Expires-huvudet som genereras av funktionen för hello externa maximal ålder. Det här alternativet är användbart för att säkerställa att alla tillgångar tilldelas Expires-rubriken.
Ta bort| Garanterar att Expires-rubriken inte ingår hello huvudsvar. Om Expires-rubriken har redan tilldelats, sedan rensas den från hello huvudsvar.

**Standardbeteende:** skriva över

###<a name="external-max-age"></a>Externa maximal ålder
**Syfte:** avgör hello maximal ålder intervall för webbläsaren tooedge server Cachevalidering. Med andra ord hello mängden tid som ska gå innan en webbläsare kan söka efter en ny version av en tillgång från en edge-server.

Den här funktionen aktiveras genererar Cache-Control: max-ålder och upphör att gälla meddelandehuvuden från våra servrar kant och skicka dem toohello HTTP-klienten. Dessa huvuden skriver över de som skapats i hello ursprungsservern. Cache-Control sidhuvud behandling och upphör att gälla behandling-huvud-funktioner kan du emellertid att använda tooalter problemet.

Viktig information:

- Den här åtgärden påverkar inte edge server tooorigin server cache revalidations. Dessa typer av revalidations bestäms av Expires-Cache-Control-huvuden som togs emot från hello ursprungsservern och kan anpassas med standard interna Max-ålder och funktionerna för inre Tvingad maximal-ålder.
- Konfigurera den här funktionen genom att ange ett heltalsvärde och välja hello önskad tidsenhet (d.v.s. sekunder, minuter, timmar, etc.).
- Ställa in den här funktionen tooa negativt värde leder vår edge servrar toosend en Cache-Control: no-cache och en Expires-tid som anges i hello tidigare med varje svar toohello webbläsare. Även om HTTP-klienter inte kommer att cachelagra hello svar, påverkar inte den här inställningen våra edge servrar möjlighet toocache hello svar från hello ursprungsservern.
- Inställningen hello tidsenhet för inaktiveras ”Off” den här funktionen. Expires-Cache-Control-huvuden cachelagras med hello ursprungsservern hello svar skickas via toohello webbläsare.

**Standardbeteende:** ut

###<a name="force-internal-max-age"></a>Tvinga inre maximal ålder
**Syfte:** avgör hello maximal ålder intervall för edge server tooorigin server Cachevalidering. Med andra ord hello mängden tid som ska gå innan en gränsserver kan kontrollera om en cachelagrad tillgång matchar hello-tillgångar lagras på hello ursprungsservern.

Viktig information:

- Den här funktionen åsidosätter hello maximal ålder intervall som anges i Cache-Control eller Expires-huvuden som genereras från en ursprungsservern.
- Webbläsaren tooedge server cache revalidations påverkas inte av den här funktionen. Dessa typer av revalidations bestäms av Cache-Control eller Expires-huvuden skickas toohello webbläsare.
- Den här funktionen har inte synliga påverka hello-svar som levereras av en kant server toohello beställaren. Det kan dock ha en effekt på hello mängden Omverifiering av trafik som skickas från våra servrar toohello ursprung gränsservern.
- Konfigurera den här funktionen genom att:
    - Att välja hello statuskod som en intern maximal ålder tillämpas.
    - Ange ett heltalsvärde och välja hello önskad tidsenhet (d.v.s. sekunder, minuter, timmar, etc.). Det här värdet definierar hello begäran maximal ålder intervall.

- Inställningen hello tidsenhet för inaktiverar ”Off” den här funktionen. Ett internt maximal ålder intervall kommer inte att tilldela toorequested tillgångar. Om hello ursprungliga huvudet inte innehåller instruktioner för cachelagring, sedan cachelagras hello tillgången enligt toohello active inställningen i funktionen standard interna Max-ålder.
- På grund av toohello sätt i vilken cache inställningar spåras, kan den här funktionen inte associeras med hello följande matchar villkor: 
    - Kant 
    - CNAME-post
    - Begäran sidhuvud Literal
    - Begäran huvud med jokertecken
    - Metod för begäran
    - URL-frågan Literal
    - URL: en fråga med jokertecken

**Standardbeteende:** ut

###<a name="h264-support-http-progressive-download"></a>H.264-stöd (http-progressiv nedladdning)
**Syfte:** avgör hello typer av H.264 filformat som kanske används toostream innehåll.

Viktig information:

- Definiera en blankstegsavgränsad tillåtna H.264 filename tillägg i alternativet filnamnstillägg. Alternativet filnamnstillägg åsidosätter hello standardbeteendet. Att stöda MP4 och F4V genom att inkludera dessa filnamnstillägg när det här alternativet. 
- Se till att tooinclude en tid när du anger varje filnamnstillägg (t.ex. .mp4 .f4v).

**Standardbeteende:** HTTP progressiv nedladdning stöder MP4 och F4V media som standard.

###<a name="honor-no-cache-request"></a>Kontrollera no-cache-begäran
**Syfte:** avgör om en HTTP-klientens no-cache-begäranden ska vidarebefordras toohello ursprungsservern.

En begäran om no-cache inträffar när hello HTTP-klienten skickar en Cache-Control: no-cache och/eller Pragma:no-cache-huvud i hello HTTP-begäran.

Värde|Resultat
--|--
Enabled|Gör ett HTTP-klientens no-cache begär toobe vidarebefordrade toohello ursprungsservern och hello ursprungsservern returnerar hello svarshuvuden och hello brödtext via hello gränsservern tillbaka toohello HTTP-klienten.
Disabled|Återställer hello standardbeteendet. hello standardbeteendet är tooprevent no-cache-begäranden från att vidarebefordras toohello ursprungsservern.

För all trafik för produktion, är det mycket rekommenderas tooleave funktionen i inaktiverad standardtillståndet. Annars kommer inte att skärma ursprung servrar från slutanvändare som oavsiktligt utlösa många no-cache-begäranden när du uppdaterar webbsidor eller från hello många vanliga mediespelare som är kodat toosend no-cache-huvud med varje video begäran. Dock den här funktionen kan vara användbar tooapply toocertain icke-produktion eller testa kataloger, i ordning tooallow ny innehåll toobe hämtas på begäran från hello ursprungsservern.

hello Cachestatus som rapporteras för en begäran tillåts toobe vidarebefordras tooan ursprungsservern på grund av toothis funktionen är TCP_Client_Refresh_Miss. Rapporten status för Cache som är tillgängliga i hello Core rapportmodulen, ger statistisk information genom att Cachestatus. Detta ger dig tootrack hello antalet och procentandelen förfrågningar som vidarebefordras tooan ursprungsservern på grund av toothis-funktionen.

**Standardbeteende:** inaktiverad.

###<a name="ignore-origin-no-cache"></a>Ignorera ursprung no-cache
**Syfte:** avgör om våra CDN kommer att ignorera hello följande direktiv som hanteras från en ursprungsservern:

- Cache-Control: privat
- Cache-Control: no-store
- Cache-Control: no-cache
- Pragma: no-cache

Viktig information:

- Konfigurera den här funktionen genom att definiera en mellanslags-avgränsad lista över statuskoder som hello ovan direktiven kommer att ignoreras.
- hello uppsättning koder för giltig status för den här funktionen är: 200, 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504, och 505.
- Inaktivera funktionen genom att ange det värde som tooa är tomt.
- På grund av toohello sätt i vilken cache inställningar spåras, kan den här funktionen inte associeras med hello följande matchar villkor: 
    - Kant 
    - CNAME-post
    - Begäran sidhuvud Literal
    - Begäran huvud med jokertecken
    - Metod för begäran
    - URL-frågan Literal
    - URL: en fråga med jokertecken

**Standardbeteende:** standardbeteendet är toohonor hello ovan direktiven.

###<a name="ignore-unsatisfiable-ranges"></a>Ignorera Unsatisfiable intervall 
**Syfte:** anger hello-svar som ska returneras tooclients när en förfrågan genererar statuskod 416 begärt intervall inte kunde hanteras.

Som standard returneras statuskoden när hello angivna byte-intervall begäran inte kan tillgodoses av en gränsserver och huvudfältet ett intervall om begäran har inte angetts.

Värde|Resultat
-|-
Enabled|Förhindrar att våra edge-servrar svarar tooan ogiltig byteintervall begäran med statuskod 416 begärt intervall inte kunde hanteras. Våra servrar kommer i stället leverera hello begärda tillgången och returnera ett 200 OK till hello-klienten.
Disabled|Återställer hello standardbeteendet. hello standardbeteendet är toohonor 416 begärt intervall inte kunde hanteras statuskoden.

**Standardbeteende:** inaktiverad.

###<a name="internal-max-stale"></a>Internt Max-inaktuell
**Syfte:** styr hur länge senaste hello normal förfallotiden för en cachelagrad tillgång kan hanteras från en gränsserver när hello edge-server är toorevalidate hello cachelagras tillgång med hello ursprungsservern.

När en tillgång maximal ålder tiden går ut normalt skickar hello edge-server en Omverifiering begäran toohello ursprungsservern. hello ursprungsservern sedan svarar med antingen en 304 inte har ändrats ge hello edge-server en ny lån på hello cachelagrade tillgång eller med 200 OK för att ange hello edge-server med en uppdaterad version av hello cachelagrade tillgång.

Om hello edge-server är tooestablish en anslutning med hello ursprungsservern vid försök sådana Omverifiering, kontroller interna Max-föråldrade funktionen för om och hur länge hello edge-server kan fortsätta tooserve hello nu föråldrade tillgången.

Observera att detta tidsintervall startar när hello tillgången max-ålder upphör att gälla, inte när hello misslyckade Omverifiering inträffar. Därför är hello längsta tid under vilken en tillgång kan hanteras utan lyckad validering hello mängden tid som anges av hello kombination av maximal ålder plus max inaktuell. Till exempel om en tillgång cachelagrades 9:00 med en maximal ålder på 30 minuter och max-inaktuell 15 minuter, därefter en misslyckad validering försöka på skulle 9:44 resultera i en slutanvändare mottagande hello inaktuella cachelagrade tillgång, medan en misslyckad validering försöket 9:46 skulle resultera i hello slutanvändaren en 504 Gateway-Timeout.

Ett värde som konfigurerats för den här funktionen har ersatts av Cache-Control: måste-verifiera eller cachelagra-kontroll: proxy-verifiera huvuden som togs emot från hello ursprungsservern. Om någon av dessa huvuden tas emot från hello ursprungsservern när en tillgång till en början cachelagras, kommer hello edge-server inte att uppfylla en inaktuella cachelagrade tillgång. I så fall, om hello edge-server är toorevalidate med hello ursprung när hello tillgången maximal ålder intervall har upphört att gälla, returnerar hello gränsservern en 504 Gateway-Timeout.

Viktig information:

- Konfigurera den här funktionen genom att:
    - Att välja hello statuskod som en aktuell max tillämpas.
    - Ange ett heltalsvärde och sedan välja hello önskad tidsenhet (d.v.s. sekunder, minuter, timmar, etc.). Det här värdet definierar hello interna max aktuell som ska användas.

- Inställningen hello tidsenhet för inaktiveras ”Off” den här funktionen. En cachelagrad tillgång hanteras inte utöver dess normala förfallotid.
- På grund av toohello sätt i vilken cache inställningar spåras, kan den här funktionen inte associeras med hello följande matchar villkor: 
    - Kant 
    - CNAME-post
    - Begäran sidhuvud Literal
    - Begäran huvud med jokertecken
    - Metod för begäran
    - URL-frågan Literal
    - URL: en fråga med jokertecken

**Standardbeteende:** 2 minuter

###<a name="partial-cache-sharing"></a>Partiell Cache delning
**Syfte:** avgör om en begäran kan generera delvis cachelagrat innehåll.

Partiell cacheminnet kanske sedan används toofulfill nya begäranden för innehållet tills hello begärt innehåll cachelagras fullständigt.

Värde|Resultat
-|-
Enabled|Begäranden kan generera delvis cachelagrat innehåll.
Disabled|Begäranden kan bara generera fullständigt cachelagrade versionen av hello begärda innehållet.

**Standardbeteende:** inaktiverad.

###<a name="prevalidate-cached-content"></a>Prevalidate cachelagrat innehåll
**Syfte:** anger om cachelagrat innehåll ska vara tillämplig för tidig validering innan TTL-upphör att gälla.

Definiera hello mängden tid som tidigare toohello gälla hello begärde innehållets TTL-värdet under vilken den är aktuell för tidig validering.

Viktig information:

- Välja ”Off” som hello tidsenhet kräver validering tootake plats när hello cachelagrat innehåll TTL har gått ut. Tid anges inte och kommer att ignoreras.

**Standardbeteende:** ut. Validering kan endast utföras efter hello cachelagrat innehåll TTL har upphört att gälla.

###<a name="refresh-zero-byte-cache-files"></a>Uppdatera cachefiler noll Byte
**Syfte:** bestämmer hur ett HTTP-klientens begäran om en 0 byte cache tillgång hanteras av vår edge-servrar.

Giltiga värden är:

Värde|Resultat
--|--
Enabled|Gör att vår edge server toore fetch hello tillgångsinformation från hello ursprungsservern.
Disabled|Återställer hello standardbeteendet. hello standardbeteendet är tooserve in giltig cache tillgångar på begäran.
Den här funktionen krävs inte för rätt cachelagring och leverans av innehåll, men kan användas som en lösning. Dynamiskt innehåll generatorer på ursprung servrar kan till exempel oavsiktligt resultera i 0 byte svar skickas toohello kant-servrar. Dessa typer av svar cachelagras vanligtvis av våra edge-servrar. Om du vet att 0 byte svar inte är ett giltigt svar 

för sådana innehåll, sedan den här funktionen kan förhindra att dessa typer av tillgångar hanteras tooyour klienter.

**Standardbeteende:** inaktiverad.

###<a name="set-cacheable-status-codes"></a>Ange Cacheable ställs statuskoder
**Syfte:** definierar hello statuskoder som kan resultera i cachelagrat innehåll.

Som standard är cachelagring endast aktiverad för 200 OK svar.

Definiera en blankstegsavgränsad hello önskad statuskoder.

Viktig information:

- Aktivera funktionen Ignorera ursprung No-Cache också. Om funktionen inte är aktiverad kan inte 200 OK svar cachelagras.
- hello uppsättning koder för giltig status för den här funktionen är: 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504, och 505.
- Den här funktionen kan inte vara används toodisable cachelagring för svar som genererar en 200 OK statuskod.

**Standardbeteende:** cachelagring är endast aktiverad för svar som genererar en 200 OK statuskod.
###<a name="stale-content-delivery-on-error"></a>Inaktuella leverans av innehåll vid fel
**Syfte:** 

Avgör om upphört att gälla cachelagrat innehåll levereras när ett fel uppstår under Cachevalidering eller när hämtning hello begär innehåll från hello kunden ursprungsservern.

Värde|Resultat
-|-
Enabled|Inaktuella innehållet ska tillhandahållas toohello beställaren när ett fel uppstår under en anslutning tooan ursprungsservern.
Disabled|hello ursprungsserver-fel vidarebefordras toohello beställaren.

**Standardbeteende:** inaktiverad

###<a name="stale-while-revalidate"></a>Inaktuella när Revalidate
**Syfte:** förbättrar prestanda genom att tillåta att våra servrar edge tooserve inaktuella innehåll toohello beställaren samtidigt Omverifiering sker.

Viktig information:

- hello-funktionen för den här funktionen varierar bl.a toohello valda tidsenhet.
    - **Tidsenhet:** ange hur lång tid och välj en tid enheten (t.ex. sekunder, minuter, timmar, etc.) tooallow inaktuella leverans av innehåll. Den här typen av installationsprogrammet tillåter hello CDN tooextend hello lång tid som den kan leverera innehåll innan du kräver verifiering enligt toohello följande formel:**TTL** + **inaktuella medan verifiera tid** 
    - **Av:** väljer ”Off” toorequire validering innan en begäran för inaktuella innehåll kan hanteras.
        - Ange hur lång tid eftersom den är och kommer att ignoreras.

**Standardbeteende:** ut. Validering måste utföras innan hello begärt innehåll kan hanteras.

###<a name="comment"></a>Kommentar
**Syfte:** tillåter en anteckning toobe som lagts till i en regel.

Ett sätt att använda för den här funktionen är tooprovide ytterligare information om hello generella i en regel eller varför en viss matchar villkoret eller funktion har lagts till toohello regeln.

Viktig information:

- Högst 150 tecken kan anges.
- Se till att tooonly använder alfanumeriska tecken.
- Hello funktionssätt hello regel påverkas inte av den här funktionen. Det är enbart avsedd tooprovide ett område där du kan ange information för framtida bruk eller som kan bidra till att när du felsöker hello regeln.
 
## <a name="headers"></a>Rubriker

Dessa funktioner är utformade tooadd, ändra eller ta bort rubriker från hello begäran eller ett svar.

Namn | Syfte
-----|--------
Ålder svarshuvud | Anger om en ålder svarshuvud ska inkluderas i hello toohello beställaren för svaret.
Felsöka Cache-svarshuvuden | Anger om ett svar kan omfatta hello X EC Debug-Svarsrubrik som innehåller information om hello princip för hello begärda tillgång.
Ändra klienten huvudet i begäran | Skriver över, lägger till eller tar bort en rubrik i en begäran.
Ändra klienten svarshuvud | Skriver över, lägger till eller tar bort ett sidhuvud från ett svar.
Ange klientens IP-anpassad rubrik | Tillåter hello hello begärande klientens IP-adress toobe tillagda toohello begäran som en anpassad begärandehuvudet.

###<a name="age-response-header"></a>Ålder svarshuvud
**Syfte**: avgör om en ålder svarshuvud ska tas med i hello svaret toohello beställaren.
Värde|Resultat
--|--
Enabled | hello ålder svarshuvud inkluderas i hello toohello beställaren för svaret.
Disabled | hello ålder svarshuvud kommer att uteslutas från hello toohello beställaren för svaret.

**Standardbeteende**: inaktiverad.

###<a name="debug-cache-response-headers"></a>Felsöka Cache-svarshuvuden
**Syfte:** avgör om ett svar kan innehålla rubriken X EC Debug som innehåller information om hello princip för hello begärda tillgång.

Felsöka cachesvar huvuden som ska inkluderas i hello svar när båda hello följande är sant:

- hello Debug svar huvuden cachefunktionen har aktiverats på önskad hello-begäran.
- hello ovan begäran definierar hello debug cache-svarshuvuden som ska tas med i hello-svar.

Felsöka cachesvar huvuden kan begäras genom att inkludera följande huvud hello och hello önskade direktiven i hello begäran:

X-EC-Debug: _Directive1_,_Directive2_,_DirectiveN_

**Exempel:**

X-EC-Debug: x-ec-cache,x-ec-check-cacheable,x-ec-cache-key,x-ec-cache-state

Värde|Resultat
-|-
Enabled|Begäranden för debug cache-svarshuvuden returnerar ett svar som innehåller X EC Debug-huvudet.
Disabled|Rubriken X EC Debug utesluts från hello svar.

**Standardbeteende:** inaktiverad.

###<a name="modify-client-response-header"></a>Ändra klienten svarshuvud
**Syfte:** varje begäran innehåller en uppsättning [begärandehuvuden]() som beskriver den. Den här funktionen kan antingen:

- Lägg till eller skriva över hello-värdet som tilldelas tooa huvudet i begäran. Om hello angivna begärandehuvudet inte finns, sedan den här funktionen lägger till det toohello begäran.
- Ta bort ett huvud från hello-begäran.

Begäranden som vidarebefordras tooan ursprungsservern visar hello ändringar som gjorts av den här funktionen.

En av hello följande åtgärder kan utföras på ett huvud:

Alternativ|Beskrivning|Exempel
-|-|-
Lägg till|hello anges värdet kommer att läggas till toend av huvudvärde hello befintliga begäran.|**Begära huvudvärde (klient):**Value1 <br/> **Begära huvudvärde (regelmotor HTTP):** Value2 <br/>**Ny begäran huvudvärde:** Value1Value2
Skriv över|hello begäran huvudvärde kommer att ange toohello specifika värdet.|**Begära huvudvärde (klient):**Value1 <br/>**Begära huvudvärde (regelmotor HTTP):** Value2 <br/>**Ny begäran huvudvärde:** Value2 <br/>
Ta bort|Tar bort hello angivna begärandehuvudet.|**Begära huvudvärde (klient):**Value1 <br/> **Ändra konfigurationen för klienten begärandehuvudet:** Delete hello huvudet i begäran i fråga. <br/>**Resultat:** hello angivna huvudet i begäran inte vidarebefordras toohello ursprungsservern.

Viktig information:

- Se till att hello-värdet som anges i alternativet är en exakt matchning för hello önskade begärandehuvudet.
- Om beaktas inte i hello syftet att identifiera en rubrik. Något av hello följande variationer av Cache-Control huvudets namn kan till exempel vara används tooidentify den:
    - cache-control
    - CACHE-CONTROL
    - cachE-Control
- Se till att tooonly använda alfanumeriska tecken, bindestreck och understreck när du anger ett huvudnamn.
- Om du tar bort ett sidhuvud förhindras från att vidarebefordras tooan ursprungsservern av vår edge-servrar.
- hello följande huvuden är reserverade och kan inte ändras av den här funktionen:
    - vidarebefordras
    - värden
    - via
    - Varning
    - x vidarebefordras för
    - Alla huvud-namn som börjar med ”x ec” är reserverade.

###<a name="modify-client-response-header"></a>Ändra klienten svarshuvud
Varje svar innehåller en uppsättning [svarshuvuden]() som beskriver den. Den här funktionen kan antingen:

- Lägg till eller skriva över hello-värdet som tilldelas tooa Svarsrubrik. Om hello angivna begärandehuvudet inte finns, sedan den här funktionen lägger till det toohello svar.
- Ta bort en svarshuvud från hello svar.

Som standard definieras svar huvudvärden genom en ursprungsservern och våra edge-servrar.

En av hello följande åtgärder kan utföras på en svarshuvud:

Alternativ|Beskrivning|Exempel
-|-|-
Lägg till|hello anges värdet kommer att läggas till toend av huvudvärde hello befintliga begäran.|**Svaret huvudvärde (klient):**Value1 <br/> **Svaret huvudvärde (regelmotor HTTP):** Value2 <br/>**Ny svar huvudvärde:** Value1Value2
Skriv över|hello begäran huvudvärde kommer att ange toohello specifika värdet.|**Svaret huvudvärde (klient):**Value1 <br/>**Svaret huvudvärde (regelmotor HTTP):** Value2 <br/>**Ny svar huvudvärde:** Value2 <br/>
Ta bort|Tar bort hello angivna begärandehuvudet.|**Begära huvudvärde (klient):** Value1 <br/> **Ändra konfigurationen för klienten begär huvud:** Delete hello svarshuvud i fråga. <br/>**Resultat:** hello angivna svarshuvudet inte vidarebefordras toohello beställaren.

Viktig information:

- Se till att hello-värdet som anges i alternativet är en exakt matchning för hello önskade Svarsrubrik. 
- Om beaktas inte i hello syftet att identifiera en rubrik. Något av hello följande variationer av Cache-Control huvudets namn kan till exempel vara används tooidentify den:
    - cache-control
    - CACHE-CONTROL
    - cachE-Control
- Om du tar bort ett sidhuvud förhindras från att vidarebefordras toohello beställaren.
- hello följande huvuden är reserverade och kan inte ändras av den här funktionen:
    - Acceptera-kodning
    - ålder
    - anslutning
    - Innehållskodning
    - innehållslängden
    - innehåll-intervall
    - Datum
    - server
    - släpvagn
    - Transfer-encoding
    - Uppgradera
    - variera
    - via
    - Varning
    - Alla huvud-namn som börjar med ”x ec” är reserverade.

###<a name="set-client-ip-custom-header"></a>Ange klientens IP-anpassad rubrik
**Syfte:** lägger till en anpassad rubrik som identifierar hello begärande klienten med IP-adress toohello begäran.

Alternativet huvudet definierar hello namn hello anpassade begärandehuvudet där hello klientens IP-adress ska lagras.

Den här funktionen kan en kund ursprung server toofind ut klienternas IP-adresser via ett anpassat huvud. Om hello begäran skickades från cache, kommer inte hello ursprungsservern informeras om hello klientens IP-adress. Därför rekommenderas det att den här funktionen användas med ADN eller tillgångar som cachelagras.

Kontrollera att det angivna huvud hello-namnet inte matchar något av följande hello:

- Standard begäran sidhuvud namn. En lista över standard sidhuvud namn finns i [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).
- Namn på reserverade huvud:
    - vidarebefordras för
    - värden
    - variera
    - via
    - Varning
    - x vidarebefordras för
    - Alla huvud-namn som börjar med ”x ec” är reserverade.
 
## <a name="logs"></a>Logs

Dessa funktioner är utformade toocustomize hello data som lagras i raw loggfiler.

Namn | Syfte
-----|--------
Anpassad logg fält 1 | Anger hello format och hello-innehåll som ska tilldelas toohello anpassad logg fält i en binär loggfil.
Loggen frågesträng | Anger om en frågesträng lagras tillsammans med hello URL: en i åtkomstloggar.

###<a name="custom-log-field-1"></a>Anpassad logg fält 1
**Syfte:** anger hello format och hello-innehåll som ska tilldelas toohello anpassad logg fält i en binär loggfil.

hello Huvudsyftet bakom anpassade fältet är tooallow toodetermine som värden i huvudet förfrågan och svar kommer att lagras i loggfilerna.

Som standard kallas hello anpassad loggfältet ”x-ec_custom-1”. Dock hello namnet på det här fältet kan anpassa den [Raw Logginställningar sidan]().

hello formatering som du bör använda toospecify begärande- och svarshuvuden definieras nedan.

Rubriktyp|Format|Exempel
-|-|-
Huvudet i begäran|%{[RequestHeader]()}[jag]() | % {Acceptera-Encoding} jag <br/> {Referent} jag <br/> % {Auktorisering} i
Svarshuvud|%{[ResponseHeader]()}[o]()| % {Ålder} o <br/> % {Content-Type} o <br/> % {Cookie} o

Viktig information:

- En anpassad logg-fältet kan innehålla en kombination av namn på huvudfält och oformaterad text.
- Giltiga tecken för det här fältet innehåller följande hello: alfanumeriska (d.v.s. 0-9, a-z eller A-Z), bindestreck, kolon, semikolon, apostrofer, kommatecken, punkter, understreck, likhetstecken, parenteser, hakparenteser och blanksteg. hello procentandel symbolen och klammerparenteser tillåts endast när används toospecify en huvudfält.
- hello stavning för varje fält angivna huvudet måste matcha hello önskade begäran och svar huvudets namn.
- Om du vill toospecify rekommenderas flera huvuden, då att du använder en avgränsare tooindicate varje sidhuvud. Du kan exempelvis använda en förkortning för varje rubrik. Exempelsyntax finns nedan.
    - AE: % {acceptera-Encoding} i A: % {auktorisering} i Datafält: % {Content-Type} o 

**Standardvärde:** -

###<a name="log-query-string"></a>Loggen frågesträng
**Syfte:** avgör om en frågesträng lagras tillsammans med hello URL: en i åtkomstloggar.

Värde|Resultat
-|-
Enabled|Tillåter hello lagring av frågesträngar när du registrerar URL: er i en åtkomst-loggen. Om en URL inte innehåller en frågesträng, har en effekt inte i det här alternativet.
Disabled|Återställer hello standardbeteendet. hello standardbeteendet är tooignore frågesträngar när du registrerar URL: er i en åtkomst-loggen.

**Standardbeteende:** inaktiverad.

<!---
## Optimize

These features determine whether a request will undergo hello optimizations provided by Edge Optimizer.

Name | Purpose
-----|--------
Edge Optimizer | Determines whether Edge Optimizer can be applied tooa request.
Edge Optimizer – Instantiate Configuration | Instantiates or activates hello Edge Optimizer configuration associated with a site.

###Edge Optimizer
**Purpose:** Determines whether Edge Optimizer can be applied tooa request.

If this feature has been enabled, then hello following criteria must also be met before hello request will be processed by Edge Optimizer:

- hello requested content must use an edge CNAME URL.
- hello edge CNAME referenced in hello URL must correspond tooa site whose configuration has been activated in a rule.

This feature requires the ADN platform and hello Edge Optimizer feature.

Value|Result
-|-
Enabled|Indicates that hello request is eligible for Edge Optimizer processing.
Disabled|Restores hello default behavior. hello default behavior is toodeliver content over the ADN platform without any additional processing.

**Default Behavior:** Disabled
 

###Edge Optimizer - Instantiate Configuration
**Purpose:** Instantiates or activates hello Edge Optimizer configuration associated with a site.

This feature requires the ADN platform and hello Edge Optimizer feature.

Key information:

- Instantiation of a site configuration is required before requests toohello corresponding edge CNAME can be processed by Edge Optimizer.
- This instantiation only needs toobe performed a single time per site configuration. A site configuration that has been instantiated will remain in that state until hello Edge Optimizer – Instantiate Configuration feature that references it is removed from hello rule.
- hello instantiation of a site configuration does not mean that all requests toohello corresponding edge CNAME will automatically be processed by Edge Optimizer. The Edge Optimizer feature determines whether an individual request will be processed.

If hello desired site does not appear in hello list, then you should edit its configuration and verify that the Active option has been marked.

**Default Behavior:** Site configurations are inactive by default.
--->

## <a name="origin"></a>Ursprung

Dessa funktioner är utformade toocontrol hur hello CDN kommunicerar med en ursprungsservern.

Namn | Syfte
-----|--------
Högsta antal Keep-Alive-begäranden | Definierar hello högsta antal begäranden för en Keep-Alive-anslutning innan den är stängd.
Proxy särskilda rubriker | Definierar hello CDN-specifika begärandehuvuden som vidarebefordras från en kant tooan ursprungsservern.


###<a name="maximum-keep-alive-requests"></a>Högsta antal Keep-Alive-begäranden
**Syfte:** definierar hello högsta antal begäranden för en Keep-Alive-anslutning innan den är stängd.

Ange hello maximalt antal begäranden tooa lågt värde rekommenderas inte och kan resultera i försämrade prestanda.

Viktig information:

- Ange det värdet hela heltal.
- Ta inte med kommatecken eller punkter i hello anges värdet.

**Standardvärde:** 10 000 begäranden

###<a name="proxy-special-headers"></a>Proxy särskilda rubriker
**Syfte:** definierar hello [CDN-specifika begärandehuvuden]() som vidarebefordras från en kant tooan ursprungsservern.

Viktig information:

- Varje CDN-specifika begärandehuvudet som definierats i den här funktionen kommer att vidarebefordras tooan ursprungsservern.
- Förhindra att ett CDN-specifikt huvud vidarebefordras tooan ursprungsservern genom att ta bort den från den här listan.

**Standardbeteende:** alla [CDN-specifika begärandehuvuden]() vidarebefordras toohello ursprungsservern.

## <a name="specialty"></a>Special

De här funktionerna ger avancerade funktioner som endast ska användas av avancerade användare.

Namn | Syfte
-----|--------
Cacheable ställs http-metoder | Bestämmer hello ytterligare HTTP-metoder som kan cachelagras i vårt nätverk.
Textstorleken för Cacheable ställs begäran | Definierar hello tröskelvärdet för att fastställa om en POST svaret kan cachelagras.

###<a name="cacheable-http-methods"></a>Cacheable ställs http-metoder
**Syfte:** bestämmer hello ytterligare HTTP-metoder som kan cachelagras i vårt nätverk.

Viktig information:

- Den här funktionen förutsätter att GET-svar alltid ska cachelagras. Hello hämta HTTP-metoden ska därför inte tas med när du ställer in den här funktionen.
- Den här funktionen stöder endast hello HTTP POST-metoden. Cachelagra efter svar genom att ange den här funktionen: POST 
- Som standard cachelagras bara begäranden vars brödtext är mindre än 14 Kb. Funktionen Cacheable ställs begäran brödtext storlek för att ange hello textstorleken för maximal begäran.

**Standardbeteende:** endast GET-svar cachelagras.

###<a name="cacheable-request-body-size"></a>Textstorleken för Cacheable ställs begäran

**Syfte:** anger hello tröskelvärdet för att fastställa om en POST svaret kan cachelagras.

Den här tröskeln bestäms genom att ange en högsta begäran textstorleken. Begäranden som innehåller en större brödtext i begäran cachelagras inte.

Viktig information:

- Den här funktionen kan bara användas när POST-svar är berättigade för cachelagring. Använd hello Cacheable ställs funktionen för HTTP-metoder för att aktivera cachelagring av POST-begäran.
- Hej begärandetexten beaktas för:
    - x-www-formuläret-urlencoded värden
    - Säkerställa en unik cache-nyckel
- Definiera en stor maximala begäran textstorleken kan påverka prestanda för leverans av data.
    - **Rekommenderat värde:** 14 Kb
    - **Minsta värde:** 1 Kb

**Standardbeteende:** 14 Kb
 
## <a name="url"></a>URL: EN

De här funktionerna kan en begäran toobe omdirigerad eller skrivas om tooa annan URL.

Namn | Syfte
-----|--------
Följ omdirigeringar | Anger om begäranden kan vara omdirigerade toohello värdnamn som definierats i hello platshuvud som returneras av en kund ursprungsservern.
URL: en omdirigering | Omdirigerar begäranden hello plats kopplingshuvud.
URL-omskrivning  | Skriver om hello URL-begäran.

###<a name="follow-redirects"></a>Följ omdirigeringar
**Syfte:** bestämmer om omdirigerade toohello värdnamn som definierats i plats-huvudet som returneras av en kund ursprungsservern kan vara.

Viktig information:

- Begäranden kan bara vara omdirigerade tooedge skapa CNAME-poster som motsvarar toohello samma plattform.

Värde|Resultat
-|-
Enabled|Begäranden kan omdirigeras.
Disabled|Begäranden omdirigeras inte.

**Standardbeteende:** inaktiverad.
###<a name="url-redirect"></a>URL: en omdirigering
**Syfte:** omdirigerar begäranden via plats-huvudet.

hello konfigurationen för den här funktionen kräver inställningen hello följande alternativ:

Alternativ|Beskrivning
-|-
Kod|Välj hello svarskod som ska returneras toohello beställaren.
Källan & mönster| Dessa inställningar definierar ett mönster för begäran URI som identifierar hello typ av begäranden kan omdirigeras. Endast begäranden vars URL uppfyller båda av följande villkor hello omdirigeras: <br/> <br/> **Källa:** (eller innehålls åtkomstpunkt) Välj en relativ sökväg som identifierar en ursprungsservern. Detta är hello ”/XXXX/” avsnittet och namnet på slutpunkten. <br/> **Källa (mönster):** ett mönster som identifierar begäranden av relativ sökväg måste anges. Det här mönstret för reguljära uttryck måste ange en sökväg som startar direkt efter hello tidigare valt innehålls åtkomstpunkt (se ovan). <br/> – Kontrollera att hello begäran URI kriterier (d.v.s. källa & mönster) ovan inte står i konflikt med eventuella villkor för matchning som definierats för den här funktionen. <br/> – Se till att toospecify ett mönster. Med ett tomt värde som hello mönstret matchar endast begäranden toohello rotmapp hello valda ursprungsservern (t.ex. http://cdn.mydomain.com/).
Mål| Definiera hello URL toowhich hello ovan begäranden ska omdirigeras. <br/> Konstruera dynamiskt med den här URL: <br/> -Ett mönster för reguljärt uttryck <br/>-HTTP variabler <br/> Ersätt hello värden sparas i hello källa mönster i hello mål mönster med hjälp av $ _n_  där  _n_  identifierar ett värde av hello ordning som den hämtades. Till exempel representerar $1 hello första värde som avbildas i hello källa mönster, medan $2 representerar hello andra värdet. <br/> 
Det är starkt rekommenderat toouse en absolut URL. hello användning av en relativ URL kan omdirigera URL: er för CDN tooan ogiltig sökväg.

**Exempelscenario**

I det här exemplet visar vi hur tooredirect en kant CNAME-URL som löser toothis bas-URL för CDN: http://marketing.azureedge.net/brochures

Kvalificerade begäranden kommer att omdirigerade toothis grundläggande edge CNAME-URL: http://cdn.mydomain.com/resources

Den här URL: en omdirigering kan uppnås genom hello följande konfiguration:![](./media/cdn-rules-engine-reference/cdn-rules-engine-redirect.png)

**Viktiga punkter:**

- hello omdirigerings-URL: en funktion definierar hello begära webbadresserna som omdirigeras. Därför krävs inte ytterligare matchar villkoren. Även om hello matchar villkoret har definierats som ”Always” begär endast den punkt toohello ”broschyrer” mapp på hello ”marknadsföring” kund ursprung omdirigeras. 
- Alla matchande förfrågningar kommer att omdirigerade toohello kant CNAME-URL som definierats i alternativet mål. 
    - Exempelscenario #1: 
        - Exempel på begäran (CDN URL): http://marketing.azureedge.net/brochures/widgets.pdf 
        - URL-begäran (efter omdirigering): http://cdn.mydomain.com/resources/widgets.pdf  
    - Exempelscenario #2: 
        - Exempel på begäran (kant CNAME URL): http://marketing.mydomain.com/brochures/widgets.pdf 
        - URL-begäran (efter omdirigering): http://cdn.mydomain.com/resources/widgets.pdf Exempelscenario
    - Exempelscenario #3: 
        - Exempel på begäran (kant CNAME URL): http://brochures.mydomain.com/campaignA/final/productC.ppt 
        - URL-begäran (efter omdirigering): http://cdn.mydomain.com/resources/campaignA/final/productC.ppt  
- hello begära schemat (% {schema}) variabeln har utnyttjas i alternativet mål. Detta säkerställer att hello begäran schemat förblir oförändrad efter omdirigeringen.
- hello URL-segment som har hämtats från hello-begäran är tillagda toohello ny URL-adress via ”$1”.
 
###<a name="url-rewrite"></a>URL-omskrivning
**Syfte:** skriver om hello URL-begäran.

Viktig information:

- hello konfigurationen för den här funktionen kräver inställningen hello följande alternativ:

Alternativ|Beskrivning
-|-
 Källan & mönster | Dessa inställningar definierar ett mönster för begäran URI som identifierar hello typ av begäranden som kan skrivas. Kommer att skrivas om endast begäranden vars URL uppfyller båda hello följande kriterier: <br/>     - **Källa (eller innehålls åtkomstpunkt):** Välj en relativ sökväg som identifierar en ursprungsservern. Detta är hello ”/XXXX/” avsnittet och namnet på slutpunkten. <br/> - **Källa (mönster):** ett mönster som identifierar begäranden av relativ sökväg måste anges. Det här mönstret för reguljära uttryck måste ange en sökväg som startar direkt efter hello tidigare valt innehålls åtkomstpunkt (se ovan). <br/> Kontrollera att hello begäran URI villkoren (d.v.s. källa & mönster) ovan inte krockar med någon av hello matchar villkoren för den här funktionen. Se till att toospecify ett mönster. Med ett tomt värde som hello mönstret matchar endast begäranden toohello rotmapp hello valda ursprungsservern (t.ex. http://cdn.mydomain.com/). 
 Mål  |Definiera hello relativ URL toowhich hello ovan begäranden ska skrivas med: <br/>    1. Att välja en innehålls åtkomstpunkt som identifierar en ursprungsservern. <br/>    2. Definiera en relativ sökväg med hjälp av: <br/>        -Ett mönster för reguljärt uttryck <br/>        -HTTP variabler <br/> <br/> Ersätt hello värden sparas i hello källa mönster i hello mål mönster med hjälp av $ _n_  där  _n_  identifierar ett värde av hello ordning som den hämtades. Till exempel representerar $1 hello första värde som avbildas i hello källa mönster, medan $2 representerar hello andra värdet. 
 Den här funktionen gör våra servrar edge toorewrite hello URL utan att utföra en traditionell omdirigering. Detta innebär att hello beställaren får hello samma svar code som om hello skrivas om URL: en hade begärts.

**Exempelscenario 1**

I det här exemplet visar vi hur tooredirect en kant CNAME-URL som löser toothis bas-URL för CDN: http://marketing.azureedge.net/brochures/

Kvalificerade begäranden kommer att omdirigerade toothis grundläggande edge CNAME-URL: http://MyOrigin.azureedge.net/resources/

Den här URL: en omdirigering kan uppnås genom hello följande konfiguration:![](./media/cdn-rules-engine-reference/cdn-rules-engine-rewrite.png)

**Exempelscenario 2**

I det här exemplet visar vi hur tooredirect en kant CNAME-URL från versaler toolowercase med reguljära uttryck.

Den här URL: en omdirigering kan uppnås genom hello följande konfiguration:![](./media/cdn-rules-engine-reference/cdn-rules-engine-to-lowercase.png)


**Viktiga punkter:**

- hello URL-omskrivning om funktionen definierar hello begäran om URL: er som ska skrivas. Därför krävs inte ytterligare matchar villkoren. Även om hello matchar villkoret har definierats som ”Always” begär endast den punkt toohello ”broschyrer” mapp på hello ”marknadsföring” kund ursprung ska skrivas.

- hello URL-segment som har hämtats från hello-begäran är tillagda toohello ny URL-adress via ”$1”.



###<a name="compatibility"></a>Efterlevnad

Den här funktionen innehåller matchar villkoren som måste uppfyllas innan det kan tillämpade tooa begäran. Den här funktionen är inte kompatibel med hello följande matchar villkoren i ordning tooprevent ställa in motstridiga matchningsvillkor:

- SOM tal
- CDN ursprung
- Klientens IP-adress
- Kunden ursprung
- Schemat för begäran
- URL-sökväg-katalog
- URL-sökväg-tillägget
- URL-sökväg filnamn
- URL-sökväg Literal
- URL-sökväg Regex
- URL-sökväg med jokertecken
- URL-frågan Literal
- Frågeparametern för URL
- URL-frågan Regex
- URL: en fråga med jokertecken


## <a name="next-steps"></a>Nästa steg
* [Regler modulreferens](cdn-rules-engine-reference.md)
* [Regler motorn villkorsuttryck](cdn-rules-engine-reference-conditional-expressions.md)
* [Regler motorn matchar villkor](cdn-rules-engine-reference-match-conditions.md)
* [Åsidosätta HTTP standardinställningar med hjälp av hello regelmotor](cdn-rules-engine.md)
* [Azure CDN-översikt](cdn-overview.md)
