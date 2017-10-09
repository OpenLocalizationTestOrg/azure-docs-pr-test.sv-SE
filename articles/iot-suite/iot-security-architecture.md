---
title: "aaaIoT säkerhetsarkitekturen | Microsoft Docs"
description: "IoT-arkitekturen säkerhetsriktlinjer och överväganden"
services: 
suite: iot-suite
documentationcenter: 
author: YuriDio
manager: timlt
editor: 
ms.assetid: 18ed3eb0-9406-44e1-8a3a-93dc6726c7ac
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: yurid
ms.openlocfilehash: 5b59133f6b1b45573318c3bd5b621d27b147d71b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-architecture"></a>Sakernas Internet-säkerhetsarkitekturen
När du designar ett system, är viktiga toounderstand hello potentiella hot toothat system och Lägg till lämpliga försvar därför hello system är utformad och konstruerad. Det är särskilt viktigt toodesign hello produkten från hello start med säkerhet i åtanke eftersom förstå hur en angripare kan vara kan toocompromise ett system som hjälper dig att se till att lämpliga åtgärder är på plats från hello början. 

## <a name="security-starts-with-a-threat-model"></a>Säkerhet som börjar med en hotmodell
Microsoft har länge använt hot modeller för sina produkter och gjort hello företagets hot modellera processen offentligt tillgängliga. hello företagets erfarenhet visar att hello modellering har oväntat fördelar utöver hello omedelbar förståelse för vilka hot är hello de flesta om. Till exempel skapar det också en väg för en öppen diskussion med andra utanför hello Utvecklingsteamet, vilket kan leda toonew idéer och förbättringar i hello produkten.

Hej syftar hotmodellering toounderstand hur en angripare kan vara kan toocompromise ett system och kontrollera sedan lämpliga åtgärder på plats. Hotmodellering tvingar hello design team tooconsider åtgärder som hello är utformat snarare än när ett system har distribuerats. Detta är ytterst viktigt eftersom onlineåterställningspunkter säkerhet försvar tooa myriad av enheter i hello fältet är omöjligt, tillförlitligt och lämnar kunder i fara.

Många utvecklingsgrupper göra en utmärkt jobb som avbildar hello funktionella krav för hello system som har nytta av kunder. Identifiera icke-uppenbara sätt att någon kan missbrukas hello system är dock mer utmanande. Hotmodellering hjälper dig att förstå vad en angripare kan göra utvecklingsgrupper och varför. Hotmodellering är en strukturerad process som skapar en diskussion om hello säkerhet designbeslut i hello system samt ändringar toohello design som görs hello vägen som påverkar säkerheten. En hotmodell är bara ett dokument, representerar den här dokumentationen också en bästa sättet att tooensure kontinuitet i knowledge kvarhållning av erfarenheter lärt dig och hjälpa nytt team komma igång snabbt. Slutligen är ett resultat av hotmodellering tooenable du tooconsider andra aspekter av säkerhet, till exempel vilka säkerhetsåtaganden gärna tooprovide tooyour kunder. Dessa åtaganden tillsammans med hotmodellering informera och enheten testning av din lösning för Sakernas Internet (IoT).

### <a name="when-toothreat-model"></a>När toothreat modellen
[Hotmodellering](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx) erbjudanden hello största värdet om det ingår i hello designfasen. När du designar har hello störst flexibilitet toomake ändringar tooeliminate hot. Eliminera hot avsiktligt är hello önskat utfall. Det är mycket enklare än att lägga till åtgärder, testa dem och att de fortfarande är aktuella och dessutom sådan eliminering är inte alltid möjligt. Det blir svårare tooeliminate hot som en produkt blir mer mogna och i sin tur slutligen kräver mer arbete och mycket svårare kompromisser än hot modeling tidigt under hello utveckling.

### <a name="what-toothreat-model"></a>Vilka toothreat modellen
Du bör koppla modellen hello lösning som en helt och även fokus i hello följande områden:

* hello funktioner för säkerhet och sekretess
* hello funktioner vars fel är relevanta säkerhet
* hello-funktioner som rör en förtroendegräns 

### <a name="who-threat-models"></a>Som hot modeller
Hotmodellering är en process som någon annan.  Det är en bra idé tootreat hello hot modelldokument som alla andra komponenter i hello lösning och verifiera den. Många utvecklingsgrupper göra en utmärkt jobb som avbildar hello funktionella krav för hello system som har nytta av kunder. Identifiera icke-uppenbara sätt att någon kan missbrukas hello system är dock mer utmanande. Hotmodellering hjälper dig att förstå vad en angripare kan göra utvecklingsgrupper och varför.

### <a name="how-toothreat-model"></a>Hur toothreat modellen
hello hot modellera processen består av fyra steg; hello stegen är:

* Modellen hello program
* Räkna upp hot
* Risken för hot
* Validera hello åtgärder

#### <a name="hello-process-steps"></a>hello steg
Tre tumregel tookeep i åtanke när du skapar en hotmodell:

1. Skapa ett diagram utanför Referensarkitektur. 
2. Starta första bredd. Få en översikt och förstå hello system som helhet innan du dyker djup.  Detta säkerställer att placerar du djupdykning i hello rätt.
3. Enheten hello process, låt inte hello processen enhet du. Om du hittar ett problem i hello modeling fasen och vill tooexplore det gå för den!  Tycker att du inte behöver toofollow här slavishly.  

#### <a name="threats"></a>Hot
hello fyra kärnor element i en hotmodell är:

* Processer (web services, Win32-tjänster, * nix Daemon osv. Observera att vissa avancerade entiteter (till exempel fältet gateways och sensorer) kan representeras som en processen när tekniska gå nedåt i dessa områden inte är möjlig.
* Datalager (var som helst data lagras, till exempel en konfigurationsfil eller databas)
* Dataflöde (där data flyttas mellan olika element i hello program)
* Externa enheter (något som interagerar med hello, men är inte under hello kontroll av programmet hello, exempel innehåller användare och satellit feeds)

Alla element i hello Arkitekturdiagram är ämne toovarious hot; Vi använder hello STRIDE mnemonik. Läs [hot Modeling igen, STRIDE](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) tooknow mer om hello STRIDE element.

Olika delar av diagram över hello är ämne toocertain STRIDE hot:

* Processer är ämne tooSTRIDE
* Dataflöden är ämne tooTID
* Datalager är ämne tooTID, och ibland R, om hello datalager loggfiler.
* Externa enheter är ämne tooSRD

## <a name="security-in-iot"></a>Säkerhet i IoT
Den anslutna särskilda enheter har ett stort antal potentiella interaktion ytan områden och interaktion mönster, som måste beaktas tooprovide ett ramverk för att skydda digital åtkomst toothose enheter. hello termen ”digitala åtkomst” är används här toodistinguish från alla åtgärder som utförs via direkt enhetsinteraktion där åtkomstsäkerhet tillhandahålls via fysiska åtkomstkontroll. Till exempel placera hello enhet till en plats med ett lås på hello dörren. Även om fysisk åtkomst kan nekas med hjälp av programvara och maskinvara, kan vidta åtgärder tooprevent fysisk åtkomst från ledande toosystem störningar. 

Vi utforska hello interaktion mönster vi ska titta på ”enhetskontroll” och ”enhetsdata” med hello samma nivå av uppmärksamhet. ”Enhetskontroll” kan klassificeras som all information som medföljer tooa enhet av part hello målet att påverka sitt beteende mot dess eller hello tillstånd i sin miljö. ”Enhetsdata” kan klassificeras som all information som en enhet genererar tooany motparten om dess tillstånd och hello observerats tillståndet för sin miljö.

I ordning toooptimize säkerhetsmetoder rekommenderar vi att en typisk IoT-arkitekturen delas upp i flera komponent-zoner som en del av hello hot modeling övningen. Dessa zoner beskrivs fullständigt i det här avsnittet och inkluderar:

* Enheten
* Fältet Gateway
* Molnet gateways, och
* Tjänster.

Zoner är breda sätt toosegment en lösning. varje zon har ofta krav data och autentisering och auktorisering. Zoner kan också använda tooisolation skada och begränsa hello effekten av låg förtroende zoner på högre förtroende zoner.

Varje zon skiljs åt av en förtroende gräns som anges som hello prickad röd linje i hello diagrammet nedan. Representerar en övergång av data/information från en källa tooanother. Under den här övergången kan hello informationen vara ämne tooSpoofing, Tampering, Repudiation, avslöjande av Information, Denial of Service och höjning av privilegier (STRIDE).

![IoT-säkerhetszoner](media/iot-security-architecture/iot-security-architecture-fig1.png) 

hello komponenter som beskrivs i varje gräns är också utsatts tooSTRIDE som aktiverar en fullständig 360 hot modeling vy över hello lösning. hello avsnitten nedan utveckla varje hello komponenter och specifika säkerhetsfrågor och lösningar som ska införas.

hello avsnitten som följer beskrivs standard komponenter som vanligtvis finns i dessa zoner.

### <a name="hello-device-zone"></a>hello enheten zon
Hej Enhetsmiljö är hello omedelbar fysiskt utrymme runt hello enheten där fysisk åtkomst och/eller ”lokala nätverk” peer-to-peer digital komma åt toohello enheten är möjligt. ”Lokala nätverk” antas toobe ett nätverk som är separat och isolerade från – men potentiellt bryggade för – hello offentliga Internet och innehåller alla kort Håll trådlösa alternativknapp-teknik som möjliggör peer-to-peer-kommunikation av enheter. Det gör *inte* omfattar alla nätverk virtualiseringsteknik skapar hello illusionen av ett lokalt nätverk och det också omfattar inte offentlig operatör nätverk som kräver någon två enheter toocommunicate i offentliga nätverk utrymme Om de vore tooenter peer-to-peer-kommunikation relationen.

### <a name="hello-field-gateway-zone"></a>hello fältet Gateway zon
Fältet gateway är en enhet/installation eller vissa generell server-programvara som fungerar som kommunikation aktiveraren och eventuella som ett system för enheten och enheten databearbetning hubb. hello fältet gateway zonen innehåller hello fältet själva gatewayen och alla enheter som är anslutna tooit. Hello namnet antyder fältet gateways fungerar utanför dedikerade databearbetning verksamhet, är vanligtvis plats bunden finns potentiellt ämne toophysical intrång och begränsad operativa redundans. Alla toosay som en gateway för fältet är ofta en sak en kan touch och sabotera samtidigt som du vet vad dess funktion är. 

En gateway för fältet skiljer sig från en router för enbart trafik i att den har en aktiv roll vid hantering av åtkomst som informationsflödet, vilket innebär att det är ett program åtgärdas entiteten och nätverksanslutning eller session terminal. En NAT-enhet eller brandvägg däremot inte uppfyller kraven som fältet gateways eftersom de inte är explicit anslutning eller session terminaler, men i stället en väg (eller block) anslutningar eller sessioner görs via dem. hello fältet gateway har två olika områden i ytan. En står hello-enheter som är anslutna tooit och representerar hello inuti hello zon och hello andra står alla externa parter och är hello kant hello zonen.   

### <a name="hello-cloud-gateway-zone"></a>hello molnet gateway zon
Molngatewayen är ett system som möjliggör kommunikation från samt toodevices eller fältet gateways från flera olika platser över offentligt nätverk utrymme, vanligtvis mot en molnbaserad kontroll- och analys systemet, en federation på sådana system. I vissa fall kan underlätta en molngateway omedelbart åtkomst toospecial syfte enheter från terminaler, till exempel surfplattor eller telefoner. I hello kontext som beskrivs här, ”moln” är endast avsedda toorefer tooa dedikerad databearbetning system som inte är bunden toohello samma plats som hello anslutna enheter eller gateways för fältet. Också förhindra riktade fysisk åtkomst operativa åtgärder i en zon för molnet och är inte nödvändigtvis exponerade tooa ”offentliga moln” infrastruktur.  

En molngateway kan potentiellt mappas till en överlägget tooinsulate hello molnet nätverksgateway för virtualisering och alla dess anslutna enheter eller gateways för fältet från annan nätverkstrafik. hello molnet själva gatewayen är varken ett system för enheten eller en bearbetning eller lagringsutrymmet för enhetsdata; Dessa anläggningar gränssnitt med hello molngateway. hello molnet gateway zonen innehåller hello molnet själva gatewayen tillsammans med alla fält gatewayer och enheter direkt eller indirekt anslutna tooit. hello kant hello zonen är en distinkta ytan där alla externa parter kommunicerar via.

### <a name="hello-services-zone"></a>hello services zon
”Tjänst” har definierats för den här kontexten som en programvarukomponent eller modul som samverkar med enheter via en gateway fältet eller molnet för insamling och analys, samt för kommandot och kontroll.  Tjänster är medlare. De fungerar under sin identitet mot gateways och andra undersystem, lagra och analysera data, självständigt problemet kommandon toodevices baserat på datainsikter eller scheman och visa information och kontroll funktioner tooauthorized slutanvändare.

### <a name="information-devices-vs-special-purpose-devices"></a>Information enheter jämfört med särskilda enheter
Datorer, telefoner och surfplattor är främst interaktiv information enheter. Telefoner och surfplattor är uttryckligen optimerade runt maximera batteri livslängd. De helst inaktivera delvis när inte omedelbart interagerar med en person eller när de inte tillhandahåller tjänster som spela musik eller sina ägare för steg tooa viss plats. Ur system fungerar enheterna information technology huvudsakligen som proxy för personer. De är ”personer motstånd” föreslå ”personer sensorer” samla in indata och åtgärder. 

Särskilda enheter, är från enkla temperatur sensorer toocomplex factory produktion rader med tusentals komponenter i dem, olika. Dessa enheter är mycket mer begränsade i syfte och även om de innehåller vissa användargränssnittet, de är i stort sett begränsade toointerfacing med eller integreras i tillgångar i fysiska hälsningsmeddelande. De mått och rapportera miljömässiga omständigheter, aktivera ventiler, styra servos, ljud larm, Växla ljus och utföra andra uppgifter. De hjälper toodo fungerar som en information-enhet är antingen för Allmänt för dyr, för stort eller för sprödbrott. hello konkreta syfte avgör omedelbart sina tekniska designen som korrekt hello tillgänglig monetära budget för produktions- och schemalagda livstid igen. hello kombination av dessa två viktiga faktorer begränsar hello tillgängliga operativa energi budget fysiska storleken och därmed tillgängligt lagringsutrymme beräknings- och säkerhetsfunktioner.  

Om något ”går fel” med automatisk eller fjärråtkomst kan kontrolleras enheter, till exempel obehöriga fysiska fel eller kontrollen logik fel toowillful intrång och hantering. hello produktion många kan förstöras, byggnader kan looted eller bränt och personer kan vara skadade eller även die. Detta är naturligtvis en helt annan klass skadan än någon ökar ett stulna kreditkort gränsen. hello Säkerhetsfältet för enheter som sätt att flytta och även för sensordata som slutligen leder till att kommandon som orsakar saker toomove måste vara högre än i e-handel eller banktjänstscenario. 

### <a name="device-control-and-device-data-interactions"></a>Kontroll av enhet och enheten data interaktioner
Den anslutna särskilda enheter har ett stort antal potentiella interaktion ytan områden och interaktion mönster, som måste beaktas tooprovide ett ramverk för att skydda digital åtkomst toothose enheter. hello termen ”digitala åtkomst” är används här toodistinguish från alla åtgärder som utförs via direkt enhetsinteraktion där åtkomstsäkerhet tillhandahålls via fysiska åtkomstkontroll. Till exempel placera hello enhet till en plats med ett lås på hello dörren. Även om fysisk åtkomst kan nekas med hjälp av programvara och maskinvara, kan vidta åtgärder tooprevent fysisk åtkomst från ledande toosystem störningar. 

Vi utforska hello interaktion mönster vi ska titta på ”enhetskontroll” och ”enhetsdata” med Hej kontrolleras när hotmodellering samma nivå. ”Enhetskontroll” kan klassificeras som all information som medföljer tooa enhet av part hello målet att påverka sitt beteende mot dess eller hello tillstånd i sin miljö. ”Enhetsdata” kan klassificeras som all information som en enhet genererar tooany motparten om dess tillstånd och hello observerats tillståndet för sin miljö. 

## <a name="threat-modeling-hello-azure-iot-reference-architecture"></a>Hot modeling hello Azure IoT-Referensarkitektur
Microsoft använder hello framework ovan toodo hot modellering för Azure IoT. I hello avsnittet nedan använder vi därför hello konkreta exempel på Azure IoT-Referensarkitektur toodemonstrate hur toothink om hot modellering för IoT och hur tooaddress hello hot identifieras. I vårt fall har identifierat fyra huvudsakliga områden i fokus:

* Enheter och datakällor
* Datatransport
* Enheten och händelsebearbetning, och
* Presentation

![Hot modellering för Azure IoT](media/iot-security-architecture/iot-security-architecture-fig2.png) 

hello diagrammet nedan ger en förenklad vy av Microsofts IoT-arkitekturen med en modell för Flödesdiagram för Data som används av hello hot Modeling verktyget:

![Hot modellering för Azure IoT verktyget MS hot modellering](media/iot-security-architecture/iot-security-architecture-fig3.png)

Det är viktigt toonote som hello arkitektur skiljer hello enheter och gateway-funktioner. Detta gör att hello användaren tooleverage gatewayenheter som är säkrare: de är kan kommunicera med hello gateway moln som använder säker protokoll, som vanligtvis kräver större bearbetning omkostnader som en ursprunglig enhet – till exempel en termostat - kunde Ange egen. I hello Azure-tjänster zon antar vi att hello Molngateway som representeras av hello Azure IoT Hub-tjänsten.

### <a name="device-and-data-sourcesdata-transport"></a>Transport-enheten och data källor/data
Det här avsnittet behandlar hello-arkitektur som beskrivs ovan via hello lins hotmodellering och ger en översikt över hur vi adressering vissa hello inbyggd frågor. Vi kommer att inriktas på hello grundelementen i en hotmodell:

* Processer (de under våra kontroll och externa objekt)
* Kommunikation (kallas även dataflöden)
* Lagring (kallas även för datalager)

#### <a name="processes"></a>Processer
Var och en av hello kategorier som beskrivs i hello Azure IoT-arkitekturen vi försök toomitigate ett antal olika hot över hello olika faser/informationen finns i: processen, kommunikation och lagring. Nedan ger vi en översikt över hello vanligaste för hello ”process” kategori, följt av en översikt över hur dessa kunde bäst undvikas: 

**(S)-förfalskning**: en angripare kan extrahera kryptografiska nyckelmaterial från en enhet, antingen på hello programvara eller maskinvara och komma åt hello system med en annan fysisk eller virtuell enhet under hello identitet hello enheten hello senare nyckelmaterial har tagits från. En bra bild är fjärransluten kontroller som kan aktivera valfri TV och som är populära prankster verktyg.

**Denial för tjänsten (D)**: en enhet kan återges inte klarar fungerar eller kommunikation med stör alternativknapp frekvenser eller skärande kablar. Till exempel rapporterar övervakning kamera som hade ström eller nätverket anslutningen avsiktligt blockerade inte data, alls.

**Manipulering (T)**: en angripare kan helt eller delvis ersätta hello-program som körs på hello enheten, vilket potentiellt hello ersatt programvara tooleverage hello äkta identitet hello enheten om hello nyckeln material eller hello kryptografiska verksamhet hålla viktiga material var tillgängliga toohello olaglig program. Till exempel att en angripare kan utnyttja extraherade viktiga väsentlig toointercept och utelämna data från hello enhet på hello kommunikation sökväg och Ersätt den med falska data som har verifierats med hello stulen nyckelmaterial.

**Avslöjande av information (I)**: om hello enheten kör behandlas för programvara, behandlas programvaran potentiellt kan läcka ut data toounauthorized parter. En angripare kan utnyttja extraherade viktiga väsentlig tooinject sig själv i sökvägen för hello kommunikation mellan hello enhet och en domänkontrollant eller fältet gateway eller molnet gateway toosiphon av information.

**Höjning av privilegier (E)**: en enhet som har en specifik funktion kan vara framtvingad toodo någonting annat. Till exempel en ventilen är programmerade tooopen hälften sätt kan vara att tooopen alla hello sätt.

| **Komponent** | **Hot** | **Lösning** | **Risk** | **Implementering** |
| --- | --- | --- | --- | --- |
| Enhet |S |Tilldela identitet toohello enhet och autentisera hello-enhet |Ersätt enhet eller en del av hello-enhet med en annan enhet. Hur vet vi pratar vi toohello rätt enhet? |Autentiserande hello enheten med Transport Layer Security (TLS) eller IPSec. Infrastrukturen ska ha stöd för med i förväg delad nyckel (PSK) på de enheter som inte kan hantera fullständig asymmetrisk kryptering. Använda Azure AD [OAuth](http://www.rfc-editor.org/in-notes/internet-drafts/draft-ietf-ace-oauth-authz-01.txt) |
| TRID |Till exempel gäller tamperproof mekanismer toohello enhet genom att göra det mycket svårt tooimpossible tooextract nycklar och andra kryptografiska material från hello enhet. |hello risken är om någon är manipulation hello enhet (fysiska störningar). Hur ska vi visst kan enheten inte har manipulerats. |hello mest effektiva minskning är en betrodd platform module (TPM) funktion som gör att lagra nycklar på särskilda-chip kretsar från vilka hello nycklar går inte att läsa, men kan endast användas för kryptografiska åtgärder som använder hello nyckel men lämna aldrig ut hello nyckel . Minne kryptering av hello enhet. Nyckelhantering för hello enhet. Signering hello kod. | |
| E |Med åtkomstkontroll av hello enhet. Auktoriseringsschema. |Om hello-enheten tillåter för enskilda åtgärder toobe utförs baserat på kommandon från en extern källa eller även komprometteras sensorer, kommer du att kunna hello attack tooperform operations annars inte tillgänglig. |Med auktorisering schemat för hello-enhet | |
| Fältet Gateway |S |Autentisera hello fältet gateway tooCloud Gateway (certifikatbaserad, PSK, anspråk baserat,...) |Om någon kan imitera fältet Gateway att den sig själv som en enhet. |TLS RSA/PSK, IPSec, [RFC 4279](https://tools.ietf.org/html/rfc4279). Alla hello samma nyckellagring och gäller för attestering av enheter i allmänhet – bästa möjliga är att använda TPM. 6LowPAN tillägget för IPSec-toosupport trådlösa Sensor nätverk (WSN). |
| TRID |Skydda hello fältet Gateway mot manipulation (TPM)? |Bedrägeri som lura hello molngateway tänker pratar toofield kan gateway leda till avslöjande av information och data manipulation |Minne-kryptering, TPM'S, autentisering. | |
| E |Åtkomstkontroll för fältet Gateway | | | |

Här följer några exempel på hot i den här kategorin:

Förfalskning: Kan en angripare extrahera kryptografiska nyckelmaterial från en enhet, antingen på hello programvara eller maskinvara nivå och därefter åtkomst hello system med en annan fysisk eller virtuell enhet under hello identitet för enheten hello hello nyckelmaterial har hämtas från.

**DOS-attack**: en enhet kan återges inte klarar fungerar eller kommunikation med stör alternativknapp frekvenser eller skärande kablar. Till exempel rapporterar övervakning kamera som hade ström eller nätverket anslutningen avsiktligt blockerade inte data, alls.

**Manipulering**: en angripare kan helt eller delvis ersätta hello-program som körs på hello enheten, vilket potentiellt hello ersatt programvara tooleverage hello äkta identitet hello enheten om hello nyckeln material eller hello kryptografiska verksamhet hålla viktiga material var tillgängliga toohello olaglig program.

**Manipulering**: kamera övervakning som visas en synliga spektrumet bild av ett tomt Hall kunde syftar på ett foto av sådana Hall. En rök eller brand sensor kunde reporting någon hålla en ljusare under den. I båda fallen hello enheten kan vara tekniskt fullständigt betrodd mot hello system, men den rapporteras behandlas för information.

**Manipulering**: en angripare kan utnyttja extraherade viktiga väsentlig toointercept och ignorera data från hello enhet på hello kommunikation sökväg och Ersätt den med falska data som har verifierats med nyckelmaterial hello blir stulen.

**Manipulering**: en angripare kan helt eller delvis ersätta hello-program som körs på hello enheten, vilket potentiellt hello ersatt programvara tooleverage hello äkta identitet hello enheten om hello nyckeln material eller hello kryptografiska verksamhet hålla viktiga material var tillgängliga toohello olaglig program.

**Avslöjande av information**: om hello enheten kör behandlas för programvara, behandlas programvaran potentiellt kan läcka ut data toounauthorized parter.

**Avslöjande av information**: en angripare kan utnyttja extraherade viktiga väsentlig tooinject sig själv i sökvägen för hello kommunikation mellan hello enhet och en domänkontrollant eller fältet gateway eller molnet gateway toosiphon av information.

**DOS-attack**: hello enheten kan vara avstängd eller aktiverat i ett läge där kommunikation är möjlig (vilket är avsiktlig i många industriella datorer).

**Manipulering**: hello enhet kan vara omkonfigurerats toooperate i ett tillstånd okänt toohello Kontrollera system (utanför kända kalibreringsparametrar) och därför ger data som kan tolkas

**Rättighetsökning**: en enhet som har en specifik funktion kan vara framtvingad toodo någonting annat. Till exempel en ventilen är programmerade tooopen hälften sätt kan vara att tooopen alla hello sätt.

**DOS-attack**: hello enheten kan omvandlas till ett tillstånd där kommunikation är möjlig.

**Manipulering**: hello enhet kan vara omkonfigurerats toooperate i ett tillstånd okänt toohello Kontrollera system (utanför kända kalibreringsparametrar) och därför ger data som kan tolkas.

**Förfalskning/Tampering/Repudiation**: om inte skyddade (vilket är det sällan hello med konsumenten remote kontroller) en angripare kan manipulera hello tillståndet för en enhet anonymt. En bra bild är fjärransluten kontroller som kan aktivera valfri TV och som är populära prankster verktyg.

#### <a name="communication"></a>Kommunikation
Hot runt kommunikationssökvägen mellan enheter, enheter och fältet gateways och gateway-enhet och molnet. hello tabellen nedan har vägledning runt öppna sockets hello enheten/VPN:

| **Komponent** | **Hot** | **Lösning** | **Risk** | **Implementering** |
| --- | --- | --- | --- | --- |
| Enheten IoT-hubb |TID |(D) TLS (PSK/RSA) tooencrypt hello trafik |Avlyssning eller stör hello kommunikationen mellan hello enheten och hello gateway |Säkerhet på protokollnivå hello. Med anpassade protokoll måste toofigure reda på hur tooprotect dem. I de flesta fall sker hello kommunikation från hello enheten toohello IoT-hubb (enheten inleder hello anslutning). |
| Enheten enhet |TID |(D) TLS (PSK/RSA) tooencrypt hello trafik. |Läsningen av data som överförs mellan enheter. Manipulering av hello data. Överbelastning hello-enhet med nya anslutningar |Säkerhet på protokollnivå för hello (MQTT/AMQP/HTTP/CoAP. Med anpassade protokoll måste toofigure reda på hur tooprotect dem. Hej för hello DoS hot är toopeer enheter via en moln- eller gateway och har endast act som klienter mot hello nätverk. Hej peering kan resultera i en direkt anslutning mellan hello peer-datorer efter att ha varit asynkrona av hello gateway |
| Extern enhet enhet |TID |Stark länkning av hello extern enhet toohello enhet |Tjuvlyssnande hello toohello anslutning. Störande hello kommunikation med hello-enhet |Länkning hello extern enhet toohello enheten NFC/Bluetooth LE på ett säkert sätt. Kontrollera hello operativa panelen av hello enhet (fysiska) |
| Fältet Gateway Cloud Gateway |TID |TLS (PSK/RSA) tooencrypt hello trafik. |Avlyssning eller stör hello kommunikationen mellan hello enheten och hello gateway |Säkerhet på protokollnivå för hello (MQTT/AMQP/HTTP/CoAP). Med anpassade protokoll måste toofigure reda på hur tooprotect dem. |
| Gateway för enhet moln |TID |TLS (PSK/RSA) tooencrypt hello trafik. |Avlyssning eller stör hello kommunikationen mellan hello enheten och hello gateway |Säkerhet på protokollnivå för hello (MQTT/AMQP/HTTP/CoAP). Med anpassade protokoll måste toofigure reda på hur tooprotect dem. |

Här följer några exempel på hot i den här kategorin:

**DOS-attack**: begränsad enheter är vanligtvis under DoS hot om de aktivt lyssna efter inkommande anslutningar eller oönskad datagram i ett nätverk, eftersom en angripare inte kan öppna många anslutningar parallellt och tjänsten dem eller tjänst dem mycket långsamt eller hello enhet kan vara översvämmat med oönskad trafik. I båda fallen renderas hello enheten effektivt oanvändbara hello nätverket.

**Förfalskning, avslöjande av Information**: begränsad enheter och särskilda enheter har ofta en för alla säkerhet verksamhet som lösenord eller PIN-skydd eller de helt förlitar sig på betrodda hello-nätverket, vilket innebär att de kommer att ge åtkomst tooinformation när en enhet är på hello samma nätverk och att nätverket är ofta endast skyddas av en delad nyckel. Som innebär att när hello delade hemliga toodevice eller nätverket lämnas, det är möjligt toocontrol hello enhet se data som sänds från hello enhet.  

**Förfalskning**: en angripare kan fånga upp eller delvis åsidosätta hello broadcast och spoof hello avsändaren (man hello mitten)

**Manipulering**: en angripare kan fånga upp eller delvis åsidosätta hello sändning och skicka falsk information 

**Avslöjande av information:** en angripare kan eavesdrop på en sändning och få information utan tillstånd **Denial of Service:** en angripare kan sylt hello-utsändningssignalen och neka information distribution

#### <a name="storage"></a>Lagring
Varje enhet och fältet gateway har någon form av lagring (tillfälliga för meddelandeköer hello data, operativsystem (OS) bildlagring).

| **Komponent** | **Hot** | **Lösning** | **Risk** | **Implementering** |
| --- | --- | --- | --- | --- |
| Enhetslagring |TRID |Lagringskryptering signering hello loggar |Läsning av data från hello lagring (personligt identifierbar information data), manipulera telemetridata. Manipulera i kö eller cachelagrad kommandot kontrolldata. Manipulera konfiguration eller inbyggd programvara uppdateringspaket kan medan cachelagras eller köade lokalt leda tooOS och/eller system komponenter komprometteras |Kryptering, message authentication code (MAC) eller digital signatur. Möjligt, starkt åtkomstkontroll via resursåtkomst styra där listor (ACL) eller behörigheter. |
| Enhetens OS-bild |TRID | |Manipulera OS / ersätta hello OS-komponenter |OS-partitionen är skrivskyddad, signerade OS-avbildning, kryptering |
| Fältet Gateway-lagring (queuing hello data) |TRID |Lagringskryptering signering hello loggar |Läsning av data från hello lagring (personligt identifierbar information data), manipulera telemetridata, manipulera i kö eller cachelagrad kommandot kontrolldata. Manipulera konfiguration eller inbyggd programvara uppdateringspaket (avsedda för enheter eller fältet gateway) kan medan cachelagras eller köade lokalt leda tooOS och/eller system komponenter komprometteras |BitLocker |
| Fältet Gateway OS-avbildningen |TRID | |Manipulera OS / ersätta hello OS-komponenter |OS-partitionen är skrivskyddad, signerade OS-avbildning, kryptering |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Enheten och händelsen bearbetning eller ett moln gateway zon
En molngateway är system som aktiverar fjärrkommunikation från och toodevices eller fältet gateways från flera olika platser över offentligt nätverk utrymme, vanligtvis mot en molnbaserad kontroll- och analys systemet, en federation på sådana system. I vissa fall kan underlätta en molngateway omedelbart åtkomst toospecial syfte enheter från terminaler, till exempel surfplattor eller telefoner. I hello kontext som beskrivs är här, ”moln” avsedd toorefer tooa dedikerad databearbetning system som inte är bunden toohello samma plats som hello anslutna enheter eller fältet gateways, och där operativa åtgärder för att förhindra fysisk åtkomst som mål men är inte nödvändigtvis tooa ”offentliga” molninfrastruktur.  En molngateway kan potentiellt mappas till en överlägget tooinsulate hello molnet nätverksgateway för virtualisering och alla dess anslutna enheter eller gateways för fältet från annan nätverkstrafik. hello molnet själva gatewayen är varken ett system för enheten eller en bearbetning eller lagringsutrymmet för enhetsdata; Dessa anläggningar gränssnitt med hello molngateway. hello molnet gateway zonen innehåller hello molnet själva gatewayen tillsammans med alla fält gatewayer och enheter direkt eller indirekt anslutna tooit.

Molngatewayen är främst anpassade inbyggd typ av program som körs som en tjänst med exponerade slutpunkter toowhich fältet gateway och enheter ansluter. Det måste därför utformas med säkerhet i åtanke. Följ [SDL](http://www.microsoft.com/sdl) processer för att designa och skapa den här tjänsten. 

#### <a name="services-zone"></a>Zonen för tjänster
Ett system för (eller domänkontrollant) är en programvarulösning som gränssnitt med en enhet eller en gateway för fältet eller gateway moln i hello syftet att styra en eller flera enheter och/eller toocollect och/eller store och/eller analysera data på enheten för presentation eller Syftet med efterföljande kontroll. System är hello endast enheter i hello omfånget för den här diskussionen som kan underlätta interaktion med personer omedelbart. hello undantag är mellanliggande fysisk kontroll på portalen innehåller på enheter som en växel som gör en person tooturn hello enheten ut eller ändra andra egenskaper och där det finns ingen funktionell motsvarande som kan nås digitalt. 

Mellanliggande fysisk kontroll på portalen innehåller är sådana där alla slags styr logik avgränsar hello funktionen av hello fysiska yta så att motsvarande funktion kan initieras via fjärranslutning eller inkommande står i konflikt med fjärråtkomst indata kan undvika – sådana intermediated kontrollen hämtar är begreppsmässigt bifogade tooa lokala system för att hello använder samma underliggande funktionaliteten som alla andra fjärrstyrning system som hello enheten kan vara anslutna tooin parallellt. Övre hot toohello cloud computing-lösningar kan läsas på [Cloud Security Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/) sidan.

## <a name="additional-resources"></a>Ytterligare resurser
Se toohello följande artiklar för ytterligare information:

* [SDL hot modellering verktyget](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
* [Microsoft Azure IoT Referensarkitektur](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)

## <a name="see-also"></a>Se även
toolearn mer om att skydda din IoT-lösningen finns [säkra din IoT-distribution][lnk-security-deployment].

Du kan även utforska några av hello andra funktioner och förmågor i hello IoT Suite förkonfigurerade lösningar:

* [Förutsägande Underhåll förkonfigurerade lösning: översikt][lnk-predictive-overview]
* [Vanliga frågor och svar om IoT Suite][lnk-faq]

Du kan läsa om IoT-hubb säkerhet i [kontroll åtkomst tooIoT hubb] [ lnk-devguide-security] i hello utvecklarhandboken för IoT-hubb.

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md

[lnk-security-deployment]: iot-suite-security-deployment.md
[lnk-devguide-security]: ../iot-hub/iot-hub-devguide-security.md