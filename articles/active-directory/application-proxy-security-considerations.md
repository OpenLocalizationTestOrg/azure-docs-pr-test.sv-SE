---
title: "aaaSecurity överväganden för Azure AD Application Proxy | Microsoft Docs"
description: "Beskriver säkerhetsaspekter för att använda Azure AD Application Proxy"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: ebd14b9d1fc8f4629c5916e5a910595727d935d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-considerations-for-accessing-apps-remotely-with-azure-ad-application-proxy"></a>Säkerhetsaspekter för att komma åt appar med Azure AD Application Proxy

Den här artikeln förklarar hur Azure Active Directory Application Proxy ger en säker tjänst för att publicera och åtkomst till dina program via fjärranslutning.

följande diagram visar hur Azure AD hello kan säker fjärråtkomst tooyour lokala program.

 ![Diagram över säker fjärråtkomst via Azure AD Application Proxy](./media/application-proxy-security-considerations/secure-remote-access.png)

## <a name="security-benefits"></a>Säkerhetsfördelarna

Azure AD Application Proxy ger följande säkerhetsfördelarna hello:

### <a name="authenticated-access"></a>Autentiserad åtkomst 

Om du väljer toouse Azure Active Directory förautentisering kan endast autentiserade anslutningar ansluta till nätverket.

Azure AD Application Proxy är beroende av hello Azure AD säkerhetstokentjänst (STS) för alla autentisering.  Förautentisering, blockeras sin natur som ett stort antal anonyma attacker eftersom endast autentiserade identiteter kan komma åt hello backend-programmet.

Om du väljer genomströmning som metod för förautentisering kan få du inte förmånen. 

### <a name="conditional-access"></a>Villkorlig åtkomst

Tillämpa rikare kontrollerna för principer innan anslutningar upprättas tooyour nätverk.

Med [villkorlig åtkomst](active-directory-conditional-access-azuread-connected-apps.md), kan du definiera begränsningar på vilken trafik som är tillåten tooaccess backend-program. Du kan skapa principer som begränsar inloggningar baserat på plats, stark autentisering och risken för användarprofil.

Du kan också använda principer för villkorlig åtkomst tooconfigure Multifaktorautentisering, lägga till ett annat lager av säkerhet tooyour användarautentisering. 

### <a name="traffic-termination"></a>Avslutning av trafik

All trafik avslutas i hello molnet.

Eftersom Azure AD Application Proxy är en omvänd proxy, avbryts alla trafik tooback slutpunkt program vid hello-tjänsten. hello session kan hämta återupprätta endast med hello backend-server, vilket innebär att dina backend-servrar inte är exponeras toodirect HTTP-trafik. Den här konfigurationen innebär att du skyddas bättre från riktade attacker.

### <a name="all-access-is-outbound"></a>All åtkomst är utgående 

Du behöver inte tooopen inkommande anslutningar toohello företagets nätverk.

Application Proxy kopplingar kan bara använda utgående anslutningar toohello Azure AD Application Proxy-tjänsten, vilket innebär att det ingen behövs tooopen portar i brandväggen för inkommande anslutningar. Traditionella proxyservrar krävs ett perimeternätverk (även kallat *DMZ*, *demilitariserad zon*, eller *avskärmat undernät*) och får åtkomst till toounauthenticated anslutningar till hello nätverksgräns. Det här scenariot krävs för många ytterligare investeringar i webbprogram brandväggen produkter tooanalyze trafik och ger ytterligare skydd toohello miljö. Med Application Proxy behöver du inte ett perimeternätverk eftersom alla anslutningar är utgående och sker över en säker kanal.

Läs mer om kopplingar [förstå Azure AD Application Proxy kopplingar](application-proxy-understand-connectors.md).

### <a name="cloud-scale-analytics-and-machine-learning"></a>Skalbar molnlagring analyser och maskininlärning 

Hämta senaste säkerhetsskydd.

Eftersom det är en del av Azure Active Directory Application Proxy kan utnyttja [Azure AD Identity Protection](active-directory-identityprotection.md), med machine learning-driven intelligence och data från hello Microsoft Security Response Center och Digital Crimes Unit. Tillsammans vi proaktivt identifiera avslöjade konton och erbjuder realtidsskydd från hög risk inloggningar. Vi ta hänsyn till flera faktorer, till exempel åtkomst från infekterade enheter via anonymizing nätverk, och från ovanliga och inte troligt platser.

Många av dessa rapporter och händelser som är redan tillgängliga via ett API för integrering med din säkerhetsinformation och händelse-hanteringssystem (SIEM).

### <a name="remote-access-as-a-service"></a>Fjärråtkomst som en tjänst

Du har inte tooworry om underhåll och korrigering lokala servrar.

Okorrigerade programvara fortfarande står för ett stort antal attacker. Azure AD Application Proxy är en Internet-skala som Microsoft äger, så du får alltid hello senaste säkerhetskorrigeringar och uppgraderingar.

tooimprove hello säkerheten för program som publicerats av Azure AD Application Proxy, vi blockera web crawler robotar från indexering och arkivera dina program. Varje gång en web crawler robot försöker hämta av roboten inställningarna för en publicerad appen Application Proxy svarar med en robots.txt-fil som innehåller `User-agent: * Disallow: /`.

## <a name="under-hello-hood"></a>Under huven hello

Azure AD Application Proxy består av två delar:

* Hej molnbaserad tjänst: den här tjänsten körs i Azure och är där hello externa klientanvändare anslutningar görs.
* [hello-lokala anslutningen](application-proxy-understand-connectors.md): komponenten lokalt hello connector lyssnar efter förfrågningar från hello Azure AD Application Proxy-tjänsten och hanterar anslutningar toohello interna program. 

Ett flöde mellan hello koppling och hello Application Proxy-tjänsten upprättas när:

* hello kopplingen har först ställts in.
* hello koppling hämtar konfigurationsinformation från hello Application Proxy-tjänsten.
* En användare kommer åt ett publicerat program.

>[!NOTE]
>All kommunikation som sker över SSL och de har sitt ursprung alltid på hello connector toohello Application Proxy-tjänsten. hello-tjänsten är endast utgående.

hello-anslutningen använder en klient certifikat tooauthenticate toohello Application Proxy-tjänsten för nästan alla samtal. hello endast undantag toothis processen är hello installationen steg där hello Klientcertifikatet har upprättats.

### <a name="installing-hello-connector"></a>Installerar hello connector

När hello connector installeras först, äga rum hello följande flödet händelser:

1. hello kopplingstjänsten registrering toohello händer som en del av hello installationen av hello-anslutningen. Användare är begärd tooenter sina Azure AD-administratörsautentiseringsuppgifter. Den token som erhållits från den här autentiseringen sedan presenteras toohello Azure AD Application Proxy-tjänsten.
2. hello Application Proxy-tjänsten utvärderar hello-token. Det garanterar att hello användaren är en företagsadministratör inom hello-klient som hello token har utfärdats för. Om hello användaren inte är administratör avslutas hello processen.
3. hello anslutningen genererar en certifikatbegäran för klienten och skickar den, tillsammans med hello token, toohello Application Proxy-tjänsten. hello-tjänsten i sin tur verifierar hello token och loggar hello klienten certifikatbegäran.
4. hello-anslutningen använder hello klientcertifikatet för framtida kommunikation med hello Application Proxy-tjänsten.
5. hello connector utför en inledande pull av hello systemkonfigurationsdata från hello-tjänsten med hjälp av dess klientcertifikatet och är nu redo tootake begäranden.

### <a name="updating-hello-configuration-settings"></a>Konfigurationsinställningarna för hello

När hello Application Proxy-tjänsten uppdaterar hello konfigurationsinställningar, äga rum hello följande flödet händelser:

1. hello connector ansluter toohello configuration slutpunkten inom hello Application Proxy-tjänsten med hjälp av dess klientcertifikat.
2. När hello Klientcertifikatet har verifierats, hello Application Proxy-tjänsten returnerar configuration data toohello anslutningen (till exempel hello connector grupp som hello anslutningen ska vara en del av).
3. Om hello aktuella certifikatet är mer än 180 dagar, genererar hello connector en ny certifikatbegäran som effektivt uppdaterar hello klientcertifikat varje 180 dagar.

### <a name="accessing-published-applications"></a>Åtkomst till publicerade program

När användare har åtkomst till ett publicerat program äger hello följande händelser rum mellan hello Application Proxy-tjänsten och hello Application Proxy connector:

1. [hello tjänsten autentiserar hello användare för hello app](#the-service-checks-the-configuration-settings-for-the-app)
2. [hello tjänsten placerar en begäran i kö för hello-koppling](#The-service-places-a-request-in-the-connector-queue)
3. [En koppling behandlar hello begäran från hello kö](#the-connector-receives-the-request-from-the-queue)
4. [hello anslutningen väntar på svar](#the-connector-waits-for-a-response)
5. [hello dataströmmar data toohello tjänstanvändaren](#the-service-streams-data-to-the-user)

toolearn mer information om vad sker i var och en av de här stegen att läsa.


#### <a name="1-hello-service-authenticates-hello-user-for-hello-app"></a>1. hello tjänsten autentiserar hello användare för hello app

Om du har konfigurerat hello app toouse genomströmning som dess förautentiseringsmetoden hello stegen i det här avsnittet är hoppas över.

Om du har konfigurerat hello app toopreauthenticate med Azure AD omdirigerade toohello Azure AD STS tooauthenticate användare, och hello följande steg utföras:

1. Application Proxy kontrollerar alla krav på villkorlig åtkomst för hello specifika program. Det här steget säkerställer att hello-användaren har tilldelats toohello program. Om tvåstegsverifiering krävs frågar hello autentisering sekvens hello användaren efter en andra autentiseringsmetod.

2. När alla kontroller har klarat hello Azure AD-STS utfärdar en signerade token för programmet hello och omdirigerar hello användaren tillbaka toohello Application Proxy-tjänsten.

3. Programproxy verifierar den hello-token har utfärdats toocorrect hello program. Den genomför också andra kontroller, till exempel att säkerställa att hello token har signerats av Azure AD och att det finns fortfarande i hello giltig fönster.

4. Application Proxy anger en krypterad autentisering cookie tooindicate som autentisering toohello program har inträffat. hello innehåller cookie ett förfallodatum tidsstämpel som baseras på hello-token från Azure AD och andra data som hello användarnamn som hello autentisering baseras på. hello cookien krypteras med en privat nyckel kända endast toohello Application Proxy-tjänsten.

5. Application Proxy omdirigeringar hello användaren tillbaka toohello ursprungligen begärde URL.

Om någon del av hello förautentisering steg misslyckas, hello användarens begäran nekas och hello användare visas ett meddelande som anger hello källan hello problemet.


#### <a name="2-hello-service-places-a-request-in-hello-connector-queue"></a>2. hello tjänsten placerar en begäran i kö för hello-koppling

Kopplingar håller en utgående anslutning öppen toohello Application Proxy-tjänsten. När en begäran kommer, köar hello service in hello begäran på en av hello öppna anslutningar för hello connector toopick upp.

hello-begäran innehåller objekt från hello program, till exempel hello begärandehuvuden, data från hello krypterad cookie, hello användaren gör hello begäran och hello be-ID. Även om data från hello krypterad cookie skickas med hello-begäran är inte hello autentiseringscookien sig själv.

#### <a name="3-hello-connector-processes-hello-request-from-hello-queue"></a>3. hello connector processer hello begäran från hello kö. 

Application Proxy utför på hello begäran en hello följande åtgärder:

* Om hello-begäran är en enkel åtgärd (t.ex, det finns inga data inom hello brödtext som med en RESTful *hämta* begäran), hello connector gör en anslutning toohello interna målresurs och sedan väntar på svar.

* Om hello begäran har data som är associerade med den i hello brödtext (till exempel en RESTful *POST* åtgärden), hello connector gör en utgående anslutning med hjälp av hello klienten certifikatet toohello Application Proxy instans. Den gör den här anslutningen toorequest hello data och öppna en anslutning toohello interna resurs. När den tar emot hello begäran från hello kopplingen hello Application Proxy-tjänsten börjar ta emot innehåll från hello användaren och vidarebefordrar toohello dataanslutning. hello koppling vidarebefordrar i sin tur hello data toohello interna resurser.

#### <a name="4-hello-connector-waits-for-a-response"></a>4. hello anslutningen väntar på svar.

När hello begäran och överföring av allt innehåll toohello tillbaka slutet är klar, hello anslutningen väntar på svar.

När den tar emot ett svar hello connector gör en utgående anslutning toohello Application Proxy-tjänst, tooreturn hello sidhuvud information och börja strömning hello returnerade data.

#### <a name="5-hello-service-streams-data-toohello-user"></a>5. hello dataströmmar data toohello tjänstanvändaren. 

Vissa bearbetningen av programmet hello kan inträffa här. Om du har konfigurerat Application Proxy tootranslate huvuden eller URL: er i ditt program sker bearbetningen som behövs för det här steget.


## <a name="next-steps"></a>Nästa steg

[Topologiöverväganden nätverk när du använder Azure AD Application Proxy](application-proxy-network-topology-considerations.md)

[Förstå Azure AD Application Proxy-kopplingar](application-proxy-understand-connectors.md)
