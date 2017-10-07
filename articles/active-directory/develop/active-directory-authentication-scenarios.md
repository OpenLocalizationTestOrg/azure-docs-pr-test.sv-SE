---
title: "aaaAuthentication scenarier för Azure AD | Microsoft Docs"
description: "En översikt över hello fem vanligaste autentiseringsscenarier för Azure Active Directory (AAD)"
services: active-directory
documentationcenter: dev-center-name
author: skwan
manager: mbaldwin
editor: 
ms.assetid: 0c84e7d0-16aa-4897-82f2-f53c6c990fd9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: skwan
ms.custom: aaddev
ms.openlocfilehash: 5b391e3f096fcb52934c4a8aff08688352bfd3dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-scenarios-for-azure-ad"></a>Autentiseringsscenarier för Azure AD
Azure Active Directory (AD Azure) förenklar autentisering för utvecklare genom att tillhandahålla identitet som en tjänst med stöd för standardiserade protokoll, till exempel OAuth 2.0 och OpenID Connect samt bibliotek med öppen källkod för olika plattformar toohelp du Börja koda snabbt. Det här dokumentet hjälper dig att du förstår hello har stöd för olika scenarier för Azure AD och visar hur tooget startas. Den är uppdelad i hello följande avsnitt:

* [Grunderna för autentisering i Azure AD](#basics-of-authentication-in-azure-ad)
* [Anspråk i säkerhetstoken för Azure AD](#claims-in-azure-ad-security-tokens)
* [Grunderna för att registrera ett program i Azure AD](#basics-of-registering-an-application-in-azure-ad)
* [Programtyper och scenarier](#application-types-and-scenarios)
  
  * [Web webbläsare tooWeb program](#web-browser-to-web-application)
  * [Sida program (SPA)](#single-page-application-spa)
  * [Interna program tooWeb API](#native-application-to-web-api)
  * [Web Application tooWeb API](#web-application-to-web-api)
  * [Daemon eller serverprogrammet tooWeb API](#daemon-or-server-application-to-web-api)

## <a name="basics-of-authentication-in-azure-ad"></a>Grunderna för autentisering i Azure AD
Om du är bekant med grundläggande begrepp för autentisering i Azure AD kan läsa det här avsnittet. I annat fall kanske du tooskip ned för[programtyper och scenarier](#application-types-and-scenarios).

Nu ska vi titta hello enklaste scenariot där identitet krävs: en användare i en webbläsare behöver tooauthenticate tooa webbprogram. Det här scenariot beskrivs mer detaljerat i hello [webbläsare tooWeb programmet](#web-browser-to-web-application) avsnitt, men det är användbart när du börjar punkt tooillustrate hello funktionerna i Azure AD och få en överblick hur hello scenariot fungerar. Överväg följande diagram i det här scenariot hello:

![Översikt över inloggning tooweb program](./media/active-directory-authentication-scenarios/basics_of_auth_in_aad.png)

Med hello diagrammet ovan i åtanke, här är vad du behöver tooknow om dess olika komponenter:

* Azure AD är hello identitetsleverantör, ansvarar för att verifiera hello identiteten för användare och program som finns i en organisation directory och slutligen utfärdar säkerhetstoken vid autentisering av användare och program.
* Ett program som vill toooutsource autentisering tooAzure AD måste registreras i Azure AD, som registrerar och som unikt identifierar hello app i hello directory.
* Utvecklare kan använda hello öppen källkod Azure AD authentication bibliotek toomake autentisering enkelt genom att hantera hello protokollet information för dig. Se [Azure Active Directory-Autentiseringsbibliotek](active-directory-authentication-libraries.md) för mer information.

• När en användare har autentiserats, hello programmet måste verifiera hello användarens säkerhets-token tooensure att autentiseringen lyckades för hello avsedd parter. Utvecklare kan använda hello tillhandahålls autentisering bibliotek toohandle validering av en token från Azure AD, inklusive token JWT (JSON Web) eller SAML 2.0. Om du vill tooperform validering manuellt finns hello [JWT-Token hanteraren](https://msdn.microsoft.com/library/dn205065.aspx) dokumentation.

> [!IMPORTANT]
> Azure AD använder kryptering med offentlig nyckel toosign token och verifiera att de är giltiga. Se [viktig Information om signering nyckeln förnyelse i Azure AD](active-directory-signing-key-rollover.md) mer information om nödvändiga logiken i hello måste du ha i dina program tooensure den alltid uppdateras med senaste hello-nycklar.
> 
> 

• hello flödet av begäranden och -svar för hello autentiseringsprocessen bestäms av hello autentiseringsprotokoll som används, till exempel OAuth 2.0, OpenID Connect WS-Federation och SAML 2.0. Dessa protokoll beskrivs i detalj i hello [Azure Active Directory-autentiseringsprotokoll](active-directory-authentication-protocols.md) avsnittet och i hello avsnitten nedan.

> [!NOTE]
> Azure AD stöder hello OAuth 2.0 och OpenID Connect standarder som gör omfattande använder ägar-token, inklusive ägar-token som JWTs. Ett ägartoken är en förenklad säkerhetstoken som ger hello ”ägar” åtkomst tooa skyddad resurs. På detta sätt är hello ”ägar” någon part som kan utgöra hello-token. Även om en part måste de först autentisera med Azure AD tooreceive hello ägar-token om hello krävs åtgärder inte vidtas toosecure hello token i överföringen och lagringen, kan de snappas upp och används av en oavsiktlig part. Även om vissa säkerhetstoken har en inbyggd mekanism för att förhindra att obehöriga personer använder dem, ägar-token har inte den här mekanismen och måste transporteras i en säker kanal, till exempel transport layer security (HTTPS). Om ett ägartoken överförs i hello rensa, en hello man i mitten-attack kan användas av skadliga tooacquire hello-token från en och använda det för en obehörig åtkomst tooa skyddad resurs. hello tillämpas samma säkerhetsprinciper när lagring eller cachelagring ägar-token för senare användning. Se alltid till att ditt program överför och lagrar ägar-token på ett säkert sätt. Flera säkerheten på ägar-token finns [RFC 6750 avsnitt 5](http://tools.ietf.org/html/rfc6750).
> 
> 

Nu när du har en översikt över hello grunderna, Läs hello avsnitt nedan toounderstand hur etablering fungerar i Azure AD och har stöd för vanliga scenarier för hello Azure AD.

## <a name="claims-in-azure-ad-security-tokens"></a>Anspråk i säkerhetstoken för Azure AD
Säkerhetstoken som utfärdats av Azure AD innehåller anspråk eller intyg för information om hello ämne som har verifierats. Dessa anspråk kan användas av hello program för olika aktiviteter. Exempelvis kan de använda toovalidate hello token, identifiera hello ämne directory-klient, användarinformation, fastställa hello ämne auktorisering och så vidare. hello anspråk finns i alla angivna säkerhetstoken är beroende av hello typ av token, hello typ av autentiseringsuppgifter som används för tooauthenticate hello användar- och hello tillämpningsprogrammets konfiguration. En kort beskrivning av varje typ av anspråk som orsakat av Azure AD har angetts i hello nedan. Mer information finns för[stöds Token och anspråkstyper](active-directory-token-and-claims.md).

| Begär | Beskrivning |
| --- | --- |
| Program-ID:t |Identifierar hello-program som använder hello-token. |
| Målgrupp |Identifierar hello mottagande resurs hello token är avsett för. |
| Application Authentication Context Class Reference |Anger hur hello-klienten har autentiserat (offentliga klient kontra konfidentiell klienten). |
| Autentisering snabbmeddelanden |Poster hello datum och tid då hello autentisering inträffade. |
| Autentiseringsmetod |Anger hur hello ämnet för hello token autentiserades (lösenord, certifikat, etc.). |
| Förnamn |Ger hello förnamn hello användaren som angetts i Azure AD. |
| Grupper |Innehåller objekt-ID: N för Azure AD-grupper hello användaren är medlem i. |
| Identitetsprovider |Poster hello identitetsleverantör som autentiserats hello ämnet för hello-token. |
| Utfärdat till |Poster hello tid vid vilken hello token har utfärdats, används ofta för token dokumentens. |
| Utfärdaren |Identifierar hello STS som orsakat hello token samt hello Azure AD-klient. |
| Efternamn |Ger hello efternamn hello användaren som angetts i Azure AD. |
| Namn |Ger en mänsklig läsbar värde som identifierar hello ämnet för hello-token. |
| Objekt-Id |Innehåller en ändras, unik identifierare för hello ämne i Azure AD. |
| Roller |Innehåller eget namn för roller med Azure AD-programmet hello användaren har beviljats. |
| Omfång |Anger hello behörigheter beviljade toohello klientprogrammet. |
| Ämne |Anger hello principal om vilka hello Assert token information. |
| Klient-Id |Innehåller inte ändras, unik identifierare för hello directory-klient som utfärdade hello-token. |
| Livslängd för token |Definierar hello tidsintervall inom vilket en token är giltig. |
| Användarens huvudnamn |Innehåller hello användarens huvudnamn hello ämne. |
| Version |Innehåller hello versionsnumret för hello-token. |

## <a name="basics-of-registering-an-application-in-azure-ad"></a>Grunderna för att registrera ett program i Azure AD
Alla program som outsources autentisering tooAzure AD måste registreras i en katalog. Det här steget handlar om Azure AD om ditt program, inklusive hello URL där det är placerad, hello URL toosend svarar efter autentisering hello URI tooidentify ditt program och mycket mer. Den här informationen krävs för några viktiga orsaker:

* Azure AD måste koordinater toocommunicate med hello program när du arbetar med inloggning eller utbyte token. Dessa omfattar hello följande:
  
  * Program-ID-URI: hello identifierare för ett program. Det här värdet skickas tooAzure AD under autentisering tooindicate vilka programmet hello anroparen vill en token för. Det här värdet ingår dessutom i hello token så att programmet hello vet var hello avsedda mål.
  * Svara URL och omdirigerings-URI: hello gäller en webb-API eller ett webbprogram, hello Reply URL är hello plats toowhich Azure AD att skicka hello autentiseringssvar, inklusive en token om autentiseringen lyckades. I det ursprungliga programmet hello fallet hello omdirigerings-URI är en unik identifierare toowhich Azure AD att omdirigera hello användaragent i en OAuth 2.0-begäran.
  * Program-ID: hello ID för ett program som genereras av Azure AD när programmet hello registreras. När du begär en Auktoriseringskoden eller token, skickas hello program-ID och nyckel tooAzure AD under autentiseringen.
  * : Hello nyckeln som skickas tillsammans med ett program-ID vid autentisering tooAzure AD toocall webb-API.
* Azure AD-behov tooensure hello programmet hello behörighet tooaccess dina katalogdata, andra program i din organisation, och så vidare

Etablering blir tydligare när du förstår att det finns två typer av program som utvecklats och integrerade med Azure AD:

* Enkel klient program: en enskild klient-programmet är avsett att användas i en organisation. Detta är vanligtvis line-of-business (LoB)-appar som skrivits av en enterprise-utvecklare. En enskild klient räcker toobe nås av användare i en katalog och därför kan det bara behöver toobe etableras i en katalog. Programmen registreras vanligtvis av en utvecklare i hello organisation.
* Flera innehavare program: ett program med flera innehavare är avsedd att användas i många organisationer är inte en organisation. Detta är vanligtvis programvara som en-tjänst (SaaS)-program som skrivits av en oberoende programvaruleverantör (ISV). Program med flera klienter behöver toobe etablerad på varje katalog där de kommer att användas, vilket kräver att användaren eller administratören medgivande tooregister dem. Medgivande processen startas när ett program som har registrerats i katalogen hello och ges åtkomst toohello Graph API eller kanske en annan webb-API. När en användare eller administratör från en annan organisation loggar du in toouse hello program, visas en dialogruta som visar hello behörigheter hello programmet kräver. hello användaren eller administratören kan sedan medgivande toohello programmet, vilket ger hello programmet åtkomst toohello anges data och slutligen registren hello program i deras katalog. Mer information finns i [översikt över hello medgivande Framework](active-directory-integrating-applications.md#overview-of-the-consent-framework).

Vissa ytterligare överväganden uppstå när du utvecklar ett program med flera innehavare i stället för en enskild klient-program. Till exempel om du gör dina program tillgängliga toousers i flera kataloger, måste en mekanism toodetermine vilken de inte finns i. En enskild klient programmet måste bara toolook i en egen katalog för en användare när ett program med flera innehavare behöver tooidentify en viss användare från alla hello kataloger i Azure AD. tooaccomplish den här uppgiften Azure AD innehåller en gemensam autentisering ändpunkt där alla program med flera innehavare kan dirigera inloggning-förfrågningar, i stället för en slutpunkt för innehavare. Den här slutpunkten är https://login.microsoftonline.com/common för alla kataloger i Azure AD, medan en slutpunkt för klient kan vara https://login.microsoftonline.com/contoso.onmicrosoft.com. vanliga hello-slutpunkten är särskilt viktigt tooconsider när du utvecklar programmet eftersom du behöver hello nödvändiga logiken toohandle flera innehavare under inloggning, utloggning, och token validering.

Om du utvecklar för närvarande en enskild klient programmet men vill toomake den tillgängliga toomany organisationer kan du enkelt göra ändringar toohello programmet och dess konfiguration i Azure AD toomake det flera klienter som är kompatibla. Dessutom använder Azure AD hello samma signeringsnyckel för alla token i alla kataloger, om du tillhandahåller autentisering i en enskild klient eller ett program för flera innehavare.

Varje scenario som beskrivs i det här dokumentet innehåller en del som beskriver etablering krav. Mer information om ett program i Azure AD-etablering och hello skillnaderna mellan enstaka och flera innehavare program, se [integrera program med Azure Active Directory](active-directory-integrating-applications.md) för mer information. Fortsätta att läsa toounderstand hello vanliga scenarier för program i Azure AD.

## <a name="application-types-and-scenarios"></a>Programtyper och scenarier
Var och en av hello-scenarier som beskrivs i det här dokumentet kan utvecklats med olika språk och plattformar. De är alla backas upp av fullständig kodexempel som är tillgängliga i vår [kodexempel guide](active-directory-code-samples.md), eller direkt från hello motsvarande [GitHub-lagringsplatser som exempel](https://github.com/Azure-Samples?utf8=%E2%9C%93&query=active-directory). Dessutom, om ditt program måste en viss del eller segment i ett scenario för slutpunkt till slutpunkt, kan i de flesta fall funktionen läggas oberoende av varandra. Till exempel om du har en inbyggd program som anropar ett webb-API, du kan enkelt lägga till ett webbprogram som anropar också hello webb-API. hello följande diagram illustrerar dessa scenarier och programtyper, och hur olika komponenter kan läggas till:

![Programtyper och scenarier](./media/active-directory-authentication-scenarios/application_types_and_scenarios.png)

Dessa är hello fem primära programmet scenarier som stöds av Azure AD:

* [Web webbläsare tooWeb programmet](#web-browser-to-web-application): en användare behöver toosign i tooa webbprogram som skyddas av Azure AD.
* [Enkel sida program (SPA)](#single-page-application-spa): en användare behöver toosign i tooa sida program som skyddas av Azure AD.
* [Interna program tooWeb API](#native-application-to-web-api): en intern program som körs på en mobiltelefon, surfplatta eller dator måste tooauthenticate en användare tooget resurser från ett webb-API som skyddas av Azure AD.
* [Webb-API för programmet tooWeb](#web-application-to-web-api): ett webbprogram måste tooget resurser från ett webb-API som skyddas av Azure AD.
* [Daemon eller serverprogrammet tooWeb API](#daemon-or-server-application-to-web-api): en daemon-program eller ett serverprogram utan användargränssnitt för web måste tooget resurser från ett webb-API som skyddas av Azure AD.

### <a name="web-browser-tooweb-application"></a>Web webbläsare tooWeb program
Det här avsnittet beskrivs ett program som autentiserar användare i en webbläsare tooa web webbprogram. I det här scenariot hello webbprogrammet leder hello användarens webbläsare toosign dem i tooAzure AD. Azure AD returnerar ett svar via hello användarens webbläsare, som innehåller anspråk om hello användare i en säkerhetstoken. Det här scenariot stöder inloggning med hello WS-Federation, SAML 2.0 och OpenID Connect protokoll.

#### <a name="diagram"></a>Diagram
![Autentiseringsflödet för tooweb webbläsare](./media/active-directory-authentication-scenarios/web_browser_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Beskrivning av protokollet flöde
1. När en användare besöker hello programmet och behöver toosign i, omdirigeras via en begäran om inloggning toohello autentisering slutpunkt i Azure AD.
2. hello användare loggar in på hello-inloggningssida.
3. Om autentiseringen lyckas skapar en autentiseringstoken Azure AD och returnerar ett inloggning svar toohello program Reply-URL som konfigurerades i hello Azure-portalen. För ett produktionsprogram vara svars-URL: en HTTPS. hello returnerade token innehåller anspråk om hello användare och Azure AD som krävs av hello program toovalidate hello-token.
4. programmet hello verifierar hello token med hjälp av en offentlig signeringsnyckel och utfärdaren information tillgänglig vid hello federationsdokument metadata för Azure AD. När programmet hello validerar hello token, startar Azure AD en ny session med hello användare. Den här sessionen kan hello användaren tooaccess hello programmet tills den förfaller.

#### <a name="code-samples"></a>Kodexempel
Se hello kodexempel för webbläsare tooWeb Programscenarier. Och kom tillbaka ofta--vi lägga till nya prov alla hello tid. [Web webbläsare tooWeb programmet](active-directory-code-samples.md#web-browser-to-web-application).

#### <a name="registering"></a>Registrera
* Enskild klient: Om du skapar ett program för din organisation, den måste registreras i företagets katalog med hjälp av hello Azure-portalen.
* Flera innehavare: Om du skapar ett program som kan användas av användare utanför organisationen, den måste vara registrerat i företagets katalog, men även måste registreras i varje organisations katalog som kommer att använda programmet hello. toomake programmet finns i deras katalog, du kan inkludera en registreringsprocessen för kunderna att de kan tooconsent tooyour program. När de registrerar sig för ditt program, kommer de att visas en dialogruta som visar hello behörigheter hello programmet krävs och hello alternativet tooconsent. Beroende på hello krävs behörighet kan en administratör i hello andra organisationen kanske krävs toogive medgivande. När hello användaren eller administratören godkänner, registreras hello program i deras directory. Mer information finns i [integrera program med Azure Active Directory](active-directory-integrating-applications.md).

#### <a name="token-expiration"></a>Token upphör att gälla
hello användarens session upphör att gälla när hello livstid hello-token som utfärdas av Azure AD upphör att gälla. Programmet kan förkorta denna tidsperiod om så önskas, till exempel logga ut användare baserat på en period av inaktivitet. När hello sessionen upphör att gälla måste hello användaren ange toosign i igen.

### <a name="single-page-application-spa"></a>Sida program (SPA)
Det här avsnittet beskrivs autentisering för en enda sida program som använder Azure AD och hello OAuth 2.0 implicit auktorisering bevilja toosecure dess webb-API tillbaka avslutas. Den enda sidan program är vanligtvis strukturerade som ett JavaScript presentation lager (klientdel) som körs i hello webbläsaren och en serverdel för webb-API som körs på en server och implementerar hello programmets affärslogik. toolearn mer om hello implicit authorization grant och hjälper dig att du bestämmer dig för om det är bäst för ditt program scenario finns [förstå hello OAuth2 implicit bevilja flödet i Azure Active Directory](active-directory-dev-understanding-oauth2-implicit-grant.md).

I detta scenario när hello användaren loggar in, hello JavaScript front end använder [Active Directory Authentication Library för JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js/tree/dev) och hello implicit auktorisering bevilja tooobtain en ID-token (id_token) från Azure AD. hello token cachelagras och hello klienten bifogar toohello begäran som hello ägar-token när du anropar tooits Web API serverdel som skyddas med hello OWIN mellanprogram. 

#### <a name="diagram"></a>Diagram
![Den enda Page Application diagram](./media/active-directory-authentication-scenarios/single_page_app.png)

#### <a name="description-of-protocol-flow"></a>Beskrivning av protokollet flöde
1. hello användaren navigerar toohello webbprogram.
2. hello-programmet returnerar hello JavaScript klientdelen (presentation layer) toohello webbläsare.
3. hello användare initierar inloggning, till exempel genom att klicka på ett tecken i länken. hello webbläsaren skickar en GET toohello Azure AD auktorisering endpoint toorequest en ID-token. Denna begäran innehåller hello program-ID och reply URL i frågeparametrar Hej.
4. Azure AD verifierar hello Reply URL mot hello registrerad Reply-URL som konfigurerades i hello Azure-portalen.
5. hello användare loggar in på hello-inloggningssida.
6. Om autentiseringen lyckas skapar en ID-token Azure AD och returnerar som en URL (#)-fragmentet toohello programmets Reply-URL. För ett produktionsprogram vara svars-URL: en HTTPS. hello returnerade token innehåller anspråk om hello användare och Azure AD som krävs av hello program toovalidate hello-token.
7. hello JavaScript klientkod som körs i hello webbläsare extraherar hello-token från hello svar toouse på säkra anrop toohello programmets web API tillbaka avslutas.
8. Hej webbläsare anrop hello programmets web API tillbaka avslutas med hello åtkomst-token i hello authorization-huvud.

#### <a name="code-samples"></a>Kodexempel
Se hello kodexempel för scenarier med enda sidan program (SPA). Vara säker på att toocheck tillbaka ofta--vi lägga till nya prov alla hello tid. [Enkel Page Application (SPA)](active-directory-code-samples.md#single-page-application-spa).

#### <a name="registering"></a>Registrera
* Enskild klient: Om du skapar ett program för din organisation, den måste registreras i företagets katalog med hjälp av hello Azure-portalen.
* Flera innehavare: Om du skapar ett program som kan användas av användare utanför organisationen, den måste vara registrerat i företagets katalog, men även måste registreras i varje organisations katalog som kommer att använda programmet hello. toomake programmet finns i deras katalog, du kan inkludera en registreringsprocessen för kunderna att de kan tooconsent tooyour program. När de registrerar sig för ditt program, kommer de att visas en dialogruta som visar hello behörigheter hello programmet krävs och hello alternativet tooconsent. Beroende på hello krävs behörighet kan en administratör i hello andra organisationen kanske krävs toogive medgivande. När hello användaren eller administratören godkänner, registreras hello program i deras directory. Mer information finns i [integrera program med Azure Active Directory](active-directory-integrating-applications.md).

När du har registrerat hello programmet måste det vara konfigurerade toouse Implicit bevilja för OAuth 2.0-protokollet. Det här protokollet är inaktiverat för program som standard. tooenable hello OAuth2 Implicit Grant-protokollet för programmet, redigera dess programmanifestet från hello Azure-portalen och ange hello ”oauth2AllowImplicitFlow” värdet tootrue. Detaljerade instruktioner finns [aktiverar OAuth 2.0 Implicit Grant för den enda sidan program](active-directory-integrating-applications.md).

#### <a name="token-expiration"></a>Token upphör att gälla
När du använder ADAL.js toomanage autentisering med Azure AD kan du dra nytta av flera funktioner som underlättar uppdatera en utgången token samt hämta token för ytterligare web API-resurser som kan anropas av hello program. När hello användare autentiseras korrekt med Azure AD, upprätta en session som skyddas av en cookie för hello användare mellan hello webbläsare och Azure AD. Det är viktigt toonote som hello sessionen finns mellan hello användare och Azure AD och inte mellan hello användar- och hello webbprogram som körs på hello-servern. När en token upphör att gälla använder ADAL.js den här sessionen toosilently få en annan token. Den gör detta med hjälp av en dold iFrame toosend och ta emot hello-begäran med hello OAuth Implicit Grant-protokollet. ADAL.js kan också använda samma mekanism toosilently hämta åtkomsttoken från Azure AD för andra webb-API-resurser att hello programmet anrop så länge dessa resurser stöder resursdelning för korsande ursprung (CORS) är registrerade i hello användarens katalog och alla nödvändiga medgivande gavs av hello användaren under inloggning.

### <a name="native-application-tooweb-api"></a>Interna program tooWeb API
Det här avsnittet beskrivs en programspecifika som anropar ett webb-API för en användares räkning. Det här scenariot bygger på hello OAuth 2.0 auktorisering kodtypen bevilja med en offentlig klient enligt beskrivningen i avsnittet 4.1 i hello [OAuth 2.0-specifikationen](http://tools.ietf.org/html/rfc6749). hello det ursprungliga programmet hämtar en åtkomst-token för hello användare med hjälp av hello OAuth 2.0-protokollet. Åtkomsttoken som sedan skickas i hello begäran toohello webb-API, som ger användare hello och returnerar hello önskad resurs.

#### <a name="diagram"></a>Diagram
![Interna program tooWeb API-Diagram](./media/active-directory-authentication-scenarios/native_app_to_web_api.png)

#### <a name="authentication-flow-for-native-application-tooapi"></a>Autentiseringsflödet för det ursprungliga programmet tooAPI
#### <a name="description-of-protocol-flow"></a>Beskrivning av protokollet flöde
Om du använder hello AD-Autentiseringsbibliotek hanteras de flesta av hello protokolldetaljer som beskrivs nedan, till exempel hello webbläsare popup-fönster, tokencachelagring och hantering av uppdaterings-tokens.

1. Med en webbläsare som popup-gör hello det ursprungliga programmet en begäran toohello autentiseringsslutpunkt i Azure AD. Denna begäran innehåller hello program-ID och hello omdirigerings-URI för hello det ursprungliga programmet som visas i hello Azure-portalen och hello program-ID URI för hello webb-API. Om hello användaren inte har redan är inloggad de inte ange toosign i igen
2. Azure AD autentiserar användaren hello. Om det är ett program med flera innehavare och medgivande är obligatoriska toouse hello programmet kommer hello användaren att nödvändiga tooconsent om de inte redan gjort det.. Efter att godkänna och vid autentiseringen, utfärdar Azure AD en auktorisering kod svar tillbaka toohello klientprogrammets omdirigerings-URI.
3. När Azure AD utfärdar ett auktorisering kod svar tillbaka toohello omdirigerings-URI, hello klienten programmet slutar webbläsare interaktion och extrakt hello Auktoriseringskoden från hello svar. Med den här auktoriseringskod hello klientprogrammet skickar en begäran tooAzure Annonsens tokenslutpunkten som innehåller hello auktoriseringskod, information om hello klientprogram (program-ID och omdirigerings-URI) och hello resurs (program-ID URI för hello webb-API).
4. hello auktoriseringskod och information om hello klientprogrammet och webb-API verifieras av Azure AD. När valideringen har lyckats, Azure AD returnerar två token: en JWT-åtkomsttoken och en uppdatering JWT-token. Dessutom kan returnerar Azure AD grundläggande information om hello användare, till exempel deras visas namnet och klient-ID.
5. Via HTTPS använder hello klientprogrammet hello returneras JWT åtkomst-token tooadd hello JWT sträng med en ”ägar” beteckning i hello Authorization-huvud för hello begäran toohello webb-API. hello webb-API verifierar sedan hello JWT-token och om verifieringen lyckas returnerar hello önskad resurs.
6. När hello åtkomst-token upphör att gälla kommer hello klientprogrammet ett felmeddelande som anger hello användare behöver tooauthenticate igen. Om programmet hello har en giltig uppdateringstoken, kan det vara används tooacquire som en ny åtkomsttoken utan att fråga hello användaren toosign i igen. Om hello uppdateringstoken upphör att gälla, måste programmet hello toointeractively autentisera hello användare igen.

> [!NOTE]
> Hej uppdateringstoken utfärdat av Azure AD kan använda tooaccess flera resurser. Om du har ett klientprogram som har behörigheten toocall två web API: er kan hello uppdateringstoken vara samt används tooget en åtkomst-token toohello andra webb-API.
> 
> 

#### <a name="code-samples"></a>Kodexempel
Se hello kodexempel för scenarier med det ursprungliga programmet tooWeb API. Och kom tillbaka ofta--vi lägga till nya prov alla hello tid. [Interna program tooWeb API](active-directory-code-samples.md#native-application-to-web-api).

#### <a name="registering"></a>Registrera
* Enskild klient: Båda hello programspecifika och hello webb-API som måste registreras i hello samma katalog i Azure AD. hello webb-API kan vara konfigurerade tooexpose en uppsättning behörigheter som används toolimit hello interna programmets åtkomst tooits resurser. hello klientprogrammet sedan väljer hello önskade behörigheter hello ”behörigheter tooOther program” nedrullningsbara menyn i hello Azure-portalen.
* Flera innehavare: Först hello programspecifika bara registrerad i hello utvecklare eller utgivarens katalog. Andra är hello programspecifika konfigurerade tooindicate hello kräver toobe fungerar. Den här listan med behörigheter som krävs visas i en dialogruta när en användare eller administratör i hello målkatalogen ger medgivande toohello program, vilket gör den tillgänglig tootheir organisation. Vissa program kräver bara användarnivå behörigheter, som alla användare i organisationen hello kan godkänna. Andra applikationer kräver på administratörsnivå som en användare i organisationen hello inte kan samtycka till. Endast en directory-administratören kan ge medgivande tooapplications som kräver den här nivån av behörigheter. När hello användaren eller administratören godkänner, registreras endast hello webb-API i sina kataloger. Mer information finns i [integrera program med Azure Active Directory](active-directory-integrating-applications.md).

#### <a name="token-expiration"></a>Token upphör att gälla
När hello programspecifika använder dess tillstånd koden tooget en JWT-token för åtkomst, emot tas även en uppdatering JWT-token. När hello åtkomst-token upphör att gälla hello uppdateringstoken kan vara används toore-autentisera hello användare utan att de toosign i igen. Den här uppdateringstoken är och sedan använda tooauthenticate hello användare, vilket resulterar i en ny åtkomst-token och uppdatera token.

### <a name="web-application-tooweb-api"></a>Web Application tooWeb API
Det här avsnittet beskrivs ett webbprogram som behöver tooget resurser från ett webb-API. I det här scenariot, det finns två identitetstyper av som hello webbprogrammet kan använda tooauthenticate och anropa hello webb-API: en Programidentitet eller en delegerad användaridentitet.

*Tillämpningsprogrammets identitet:* det här scenariot använder OAuth 2.0 klientens autentiseringsuppgifter bevilja tooauthenticate som programmet hello och åtkomst hello webb-API. När du använder en Programidentitet, hello web API kan endast upptäcka att anropar hello webbprogram, som hello får webb-API inte någon information om hello användare. Om programmet hello tar emot information om hello användare, skickas via hello protokoll och den inte är signerad av Azure AD. hello webb-API förtroenden att hello webbprogrammet autentiserade hello användare. Därför kan kallas det här mönstret betrodda undersystemet.

*Delegerad användaridentitet:* det här scenariot kan utföras på två sätt: OpenID Connect och OAuth 2.0 auktorisering kod bevilja en konfidentiell klient. hello webbprogrammet hämtar en åtkomst-token för hello användare, vilket bevisar toohello webb-API som hello användare autentiserats toohello webbprogram och hello webbprogrammet var. kan tooobtain en delegerad användare identitet toocall hello webb-API. Åtkomsttoken skickas i hello begäran toohello webb-API, som ger användare hello och returnerar hello önskad resurs.

#### <a name="diagram"></a>Diagram
![Web Application tooWeb API-diagram](./media/active-directory-authentication-scenarios/web_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Beskrivning av protokollet flöde
Både hello Programidentitet och delegerade identitet användartyper diskuteras i hello flödet nedan. hello viktigaste skillnaden mellan dem är att hello delegerad användaridentitet först skaffa en auktoriseringskod innan hello användare kan logga in och få åtkomst till toohello webb-API.

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Tillämpningsprogrammets identitet med OAuth 2.0-klienten autentiseringsuppgifter bevilja
1. En användare är inloggad i tooAzure AD i hello webbprogram (se hello [webbläsare tooWeb programmet](#web-browser-to-web-application) ovan).
2. hello webbprogrammet måste tooacquire en åtkomst-token så att den kan autentisera toohello webb-API och hämta hello önskad resurs. Den gör en begäran tooAzure Annonsens tokenslutpunkten, hello autentiseringsuppgifter, program-ID och API: er webbprogrammet ID URI.
3. Azure AD autentiserar hello program och returnerar en JWT åtkomst-token som används toocall hello webb-API.
4. Via HTTPS använder hello webbprogrammet hello returneras JWT åtkomst-token tooadd hello JWT sträng med en ”ägar” beteckning i hello Authorization-huvud för hello begäran toohello webb-API. hello webb-API verifierar sedan hello JWT-token och om verifieringen lyckas returnerar hello önskad resurs.

##### <a name="delegated-user-identity-with-openid-connect"></a>Delegerad användaridentiteter med OpenID Connect
1. En användare är inloggad i tooa webbprogram med hjälp av Azure AD (se hello [webbläsare tooWeb programmet](#web-browser-to-web-application) ovan). Om hello användaren av hello webbprogrammet inte har ännu godkänt tooallowing hello web application toocall hello webb-API i dess ställe, måste hello användaren tooconsent. hello program visas hello behörigheter som krävs och om någon av dessa är på administratörsnivå normal användare i hello katalogen inte kan tooconsent. Medgivande processen gäller bara toomulti klient program, inte en organisation program som har redan hello programmet hello behörighet. När hello användaren inloggad, emot hello webbprogrammet en ID-token med information om hello användare, samt en Auktoriseringskoden.
2. Använder hello auktoriseringskod som utfärdats av Azure AD, hello webbprogrammet skickar en begäran tooAzure Annonsens tokenslutpunkten som innehåller hello auktoriseringskod, information om hello klientprogram (program-ID och omdirigerings-URI) och hello önskad resurs ( program-ID URI för hello webb-API).
3. hello auktoriseringskod och information om hello webbprogram och webb-API verifieras av Azure AD. När valideringen har lyckats, Azure AD returnerar två token: en JWT-åtkomsttoken och en uppdatering JWT-token.
4. Via HTTPS använder hello webbprogrammet hello returneras JWT åtkomst-token tooadd hello JWT sträng med en ”ägar” beteckning i hello Authorization-huvud för hello begäran toohello webb-API. hello webb-API verifierar sedan hello JWT-token och om verifieringen lyckas returnerar hello önskad resurs.

##### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Delegerad användaridentiteter med OAuth 2.0 Authorization kod Grant
1. En användare är redan inloggad tooa webbprogrammet, vars autentiseringsmekanism är oberoende av Azure AD.
2. hello webbprogram kräver tillstånd code tooacquire ett åtkomsttoken, så att den skickar en begäran via hello webbläsare tooAzure Annonsens autentiseringsslutpunkt tillhandahåller hello program-ID och omdirigerings-URI för hello webbprogrammet efter lyckad autentisering. hello användare loggar in tooAzure AD.
3. Om hello användaren av hello webbprogrammet inte har ännu godkänt tooallowing hello web application toocall hello webb-API i dess ställe, måste hello användaren tooconsent. hello program visas hello behörigheter som krävs och om någon av dessa är på administratörsnivå normal användare i hello katalogen inte kan tooconsent. Den här medgivande gäller tooboth enkla och flera innehavare program.  Hello enstaka klient om kan en administratör utföra admin medgivande tooconsent åt sina användare.  Detta kan göras med hjälp av hello `Grant Permissions` knapp i hello [Azure Portal](https://portal.azure.com). 
4. När hello användaren har godkänt får hello webbprogrammet hello auktoriseringskod måste tooacquire en åtkomst-token.
5. Använder hello auktoriseringskod som utfärdats av Azure AD, hello webbprogrammet skickar en begäran tooAzure Annonsens tokenslutpunkten som innehåller hello auktoriseringskod, information om hello klientprogram (program-ID och omdirigerings-URI) och hello önskad resurs ( program-ID URI för hello webb-API).
6. hello auktoriseringskod och information om hello webbprogram och webb-API verifieras av Azure AD. När valideringen har lyckats, Azure AD returnerar två token: en JWT-åtkomsttoken och en uppdatering JWT-token.
7. Via HTTPS använder hello webbprogrammet hello returneras JWT åtkomst-token tooadd hello JWT sträng med en ”ägar” beteckning i hello Authorization-huvud för hello begäran toohello webb-API. hello webb-API verifierar sedan hello JWT-token och om verifieringen lyckas returnerar hello önskad resurs.

#### <a name="code-samples"></a>Kodexempel
Se hello kodexempel för webbprogram tooWeb API scenarier. Och kom tillbaka ofta--vi lägga till nya prov alla hello tid. Web [programmet tooWeb API](active-directory-code-samples.md#web-application-to-web-api).

#### <a name="registering"></a>Registrera
* Enskild klient: För både hello Programidentitet och delegerad användare identitet fall hello webbprogram och hello webb-API som måste registreras i hello samma katalog i Azure AD. hello webb-API kan vara konfigurerade tooexpose en uppsättning behörigheter som används toolimit hello webbprogrammets åtkomst tooits resurser. Om en delegerad användaridentitetstypen används måste hello webbprogrammet tooselect hello önskade behörigheter hello ”behörigheter tooOther program” nedrullningsbara menyn i hello Azure-portalen. Det här steget är inte nödvändigt om hello programtyp identitet som används.
* Flera innehavare: Först hello webbprogram är konfigurerade tooindicate hello kräver toobe fungerar. Den här listan med behörigheter som krävs visas i en dialogruta när en användare eller administratör i hello målkatalogen ger medgivande toohello program, vilket gör den tillgänglig tootheir organisation. Vissa program kräver bara användarnivå behörigheter, som alla användare i organisationen hello kan godkänna. Andra applikationer kräver på administratörsnivå som en användare i organisationen hello inte kan samtycka till. Endast en directory-administratören kan ge medgivande tooapplications som kräver den här nivån av behörigheter. När hello användaren eller administratören medgivanden hello web application och hello web API registreras både i sina kataloger.

#### <a name="token-expiration"></a>Token upphör att gälla
När hello webbprogrammet använder dess tillstånd koden tooget en JWT-token för åtkomst, får den också en uppdatering JWT-token. När hello åtkomst-token upphör att gälla hello uppdateringstoken kan vara används toore-autentisera hello användare utan att de toosign i igen. Den här uppdateringstoken är och sedan använda tooauthenticate hello användare, vilket resulterar i en ny åtkomst-token och uppdatera token.

### <a name="daemon-or-server-application-tooweb-api"></a>Daemon eller serverprogrammet tooWeb API
Det här avsnittet beskriver en daemon eller server-programmet som behöver tooget resurser från ett webb-API. Det finns två underordnade scenarier som gäller toothis avsnittet: en daemon som behöver toocall en webb-API som bygger på OAuth 2.0 autentiseringsuppgifter bevilja klienttyp; och ett serverprogram (till exempel ett webb-API) som behöver toocall ett webb-API, som bygger på OAuth 2.0 On-Behalf-Of utkast till en specifikation.

Hello scenariot när en daemon-program behöver toocall webb-API är viktiga toounderstand några saker. Först är interaktion från användaren inte möjligt med ett daemon-program som kräver hello programmet toohave sin egen identitet. Ett exempel på en daemon-program är batchjobb eller en tjänst i operativsystemet som körs i bakgrunden hello. Den här typen av program begär en åtkomst-token med hjälp av dess Programidentitet och presentera dess program-ID och autentiseringsuppgifter (lösenordet eller certifikatet) program-ID URI tooAzure AD. Efter en lyckad autentisering får hello daemon en åtkomst-token från Azure AD, som kan använda toocall hello webb-API.

Hello scenario när ett serverprogram måste toocall ett webb-API, är det bra toouse ett exempel. Anta att en användare har autentiserats på det ursprungliga programmet och interna programmet måste toocall webb-API. Azure AD utfärdar en JWT åtkomst-token toocall hello webb-API. Om hello webb-API måste toocall ett annat underordnat webb-API, kan som användas hello på-flöde toodelegate hello användarens identitet och autentisera toohello andra nivån webb-API.

#### <a name="diagram"></a>Diagram
![Daemon eller serverprogrammet tooWeb API-diagram](./media/active-directory-authentication-scenarios/daemon_server_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Beskrivning av protokollet flöde
##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Tillämpningsprogrammets identitet med OAuth 2.0-klienten autentiseringsuppgifter bevilja
1. Först måste hello serverprogrammet tooauthenticate med Azure AD som själva, utan att någon mänsklig interaktion, till exempel en dialogruta för interaktiv inloggning. Den gör en begäran tooAzure Annonsens tokenslutpunkten, hello autentiseringsuppgifter, program-ID och program-ID-URI.
2. Azure AD autentiserar hello program och returnerar en JWT åtkomst-token som används toocall hello webb-API.
3. Via HTTPS använder hello webbprogrammet hello returneras JWT åtkomst-token tooadd hello JWT sträng med en ”ägar” beteckning i hello Authorization-huvud för hello begäran toohello webb-API. hello webb-API verifierar sedan hello JWT-token och om verifieringen lyckas returnerar hello önskad resurs.

##### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>Delegerad användaridentiteter med OAuth 2.0-specifikationen för On-Behalf-Of utkast
hello flödet beskrivs nedan förutsätter att en användare har autentiserats på ett annat program (till exempel programspecifika) och deras användaridentitet har använt tooacquire en åtkomst-token toohello första nivån web API.

1. hello programspecifika skickar hello åtkomst-token toohello första nivån webb-API.
2. hello första nivån web API skickar en begäran tooAzure Annonsens tokenslutpunkten, att tillhandahålla sina program-ID och autentiseringsuppgifter, samt hello användarens åtkomsttoken. Dessutom hello-begäran skickas med en on_behalf_of parameter som anger hello web API begär en ny token toocall ett underordnat webb-API för hello ursprungliga användares räkning.
3. Azure AD verifierar att hello första nivån webb-API har behörigheter tooaccess hello andra nivån web API och validerar hello-begäran returnerar en JWT-token för åtkomst och en JWT uppdatera token toohello första nivån web API.
4. Via HTTPS hello hello första nivån web API-anrop sedan andra nivån webb-API genom att lägga till hello token sträng i hello Authorization-huvud i hello-begäran. hello första nivån webb-API kan fortsätta toocall hello andra nivån webb-API som hello åtkomst-token och uppdateringstoken är giltiga.

#### <a name="code-samples"></a>Kodexempel
Se hello kodexempel för Daemon eller serverprogrammet tooWeb API-scenarier. Och kom tillbaka ofta--vi lägga till nya prov alla hello tid. [Server eller Daemon program tooWeb API](active-directory-code-samples.md#server-or-daemon-application-to-web-api)

#### <a name="registering"></a>Registrera
* Enskild klient: För både hello Programidentitet och delegerad användare identitet fall hello daemon eller server-programmet måste registreras i hello samma katalog i Azure AD. hello webb-API kan vara konfigurerade tooexpose en uppsättning behörigheter som används toolimit hello daemon eller åtkomst tooits serverresurser. Om en delegerad användaridentitetstypen används måste hello serverprogrammet tooselect hello önskade behörigheter hello ”behörigheter tooOther program” nedrullningsbara menyn i hello Azure-portalen. Det här steget är inte nödvändigt om hello programtyp identitet som används.
* Flera innehavare: Först hello daemon eller server-programmet är konfigurerade tooindicate hello kräver toobe fungerar. Den här listan med behörigheter som krävs visas i en dialogruta när en användare eller administratör i hello målkatalogen ger medgivande toohello program, vilket gör den tillgänglig tootheir organisation. Vissa program kräver bara användarnivå behörigheter, som alla användare i organisationen hello kan godkänna. Andra applikationer kräver på administratörsnivå som en användare i organisationen hello inte kan samtycka till. Endast en directory-administratören kan ge medgivande tooapplications som kräver den här nivån av behörigheter. När hello användaren eller administratören godkänner, är både hello web API: er registrerade i sina kataloger.

#### <a name="token-expiration"></a>Token upphör att gälla
När hello första program använder dess tillstånd koden tooget en JWT-token för åtkomst, emot tas även en uppdatering JWT-token. När hello åtkomst-token upphör att gälla hello uppdateringstoken kan vara används toore-autentisera hello användare utan att fråga efter autentiseringsuppgifter. Den här uppdateringstoken är och sedan använda tooauthenticate hello användare, vilket resulterar i en ny åtkomst-token och uppdatera token.

## <a name="see-also"></a>Se även
[Azure Active Directory-Guide för utvecklare](active-directory-developers-guide.md)

[Azure Active Directory-kodexempel](active-directory-code-samples.md)

[Viktig Information om att signera nyckelförnyelse i Azure AD](active-directory-signing-key-rollover.md)

[OAuth 2.0 i Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx)

