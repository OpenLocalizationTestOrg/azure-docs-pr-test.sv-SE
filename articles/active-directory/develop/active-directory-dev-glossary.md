---
title: aaaAzure Active Directory Developer ordlista | Microsoft Docs
description: "En lista över villkor för vanliga begrepp för utvecklare av Azure Active Directory och funktioner."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 551512df-46fb-4219-a14b-9c9fc23998ba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 32a6a6194b8d01409159a2b60263bb66a87a843c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-developer-glossary"></a>Azure Active Directory developer ordlista
Den här artikeln innehåller definitioner för några av hello Azure Active Directory (AD) developer grundbegrepp, vilket underlättar vid utbildning om programutveckling för Azure AD.

## <a name="access-token"></a>Åtkomst-token
En typ av [säkerhetstoken](#security-token) utfärdats av en [auktorisering server](#authorization-server), och används av en [klientprogrammet](#client-application) i ordning tooaccess en [skyddade resursservern ](#resource-server). Vanligtvis i hello form av en [JSON-Webbtoken (JWT)][JWT], hello token innehåller hello auktorisering toohello klienten har beviljat hello [resursägare](#resource-owner), för en begärda nivån åtkomst. hello token innehåller alla tillämpliga [anspråk](#claim) om hello ämne, aktivera hello klienten programmet toouse den som en form av autentiseringsuppgifter vid åtkomst till en viss resurs. Detta eliminerar också hello behovet hello resurs ägare tooexpose autentiseringsuppgifter toohello klienten.

Åtkomsttoken är det ibland hänvisade tooas ”användare + App” eller ”App-Only” beroende på hello autentiseringsuppgifter som representeras. Till exempel när ett klientprogram använder den:

* [”Auktoriseringskoden” authorization grant](#authorization-grant)hello användaren autentiserar först som hello resurs-ägare, delegera auktorisering toohello klienten tooaccess hello resurs. hello autentiserar klienten efteråt när hello åtkomst-token. hello token kan ibland vara hänvisade toomore uttryckligen som en ”användare +” apptoken, eftersom den representerar båda hello-användaren som behörighet hello klientprogrammet och hello program.
* [”Klientens autentiseringsuppgifter” authorization grant](#authorization-grant)hello klienten ger hello ensam autentisering, fungerar utan hello resurs-ägarens autentisering/auktorisering, så hello token kan ibland vara hänvisade tooas ”App-Only”-token.

Se [Azure AD tokenreferens] [ AAD-Tokens-Claims] för mer information.

## <a name="application-manifest"></a>Applikationsmanifestet
En funktion som tillhandahålls av hello [Azure-portalen][AZURE-portal], som producerar en JSON-representation av hello programmets identitet konfiguration, används som en mekanism för att uppdatera de associerade [ Programmet] [ AAD-Graph-App-Entity] och [ServicePrincipal] [ AAD-Graph-Sp-Entity] entiteter. Se [förstå hello Azure Active Directory-programmanifestet] [ AAD-App-Manifest] för mer information.

## <a name="application-object"></a>Programobjektet
När du registrera/uppdatera ett program i hello [Azure-portalen][AZURE-portal], hello skapar/uppdateringar av Företagsportalen programobjekt och en motsvarande [tjänstens huvudnamn objekt](#service-principal-object)för den klienten. hello programobjektet *definierar* hello programmets identitet konfiguration globalt (över alla klienter där du har åtkomst till), som ger en mall som dess motsvarande service principal objekt är  *härledda* för lokalt under körning (i en viss klient).

Se [program och tjänstens huvudnamn objekt] [ AAD-App-SP-Objects] för mer information.

## <a name="application-registration"></a>Registrera programmet
Order tooallow i ett program toointegrate med och delegera identitets- och åtkomsthantering funktioner tooAzure AD, måste den vara registrerad med en Azure AD [klient](#tenant). När du registrerar ditt program med Azure AD tillhandahåller en identity-konfiguration för programmet, vilket gör att den toointegrate med Azure AD och använda funktioner som:

* Stabil hantering av enkel inloggning med Azure AD Identity Management och [OpenID Connect] [ OpenIDConnect] protokollimplementering
* Asynkrona åtkomst för[skyddade resurser](#resource-server) av [klientprogram](#client-application), via Azure AD OAuth 2.0 [auktorisering server](#authorization-server) implementering
* [Medgivande framework](#consent) för att hantera klienten åtkomst tooprotected resurser utifrån resurs ägare auktorisering.

Se [integrera program med Azure Active Directory] [ AAD-Integrating-Apps] för mer information.

## <a name="authentication"></a>Autentisering
hello act angrips part legitima autentiseringsuppgifter för att tillhandahålla hello grund för att skapa ett huvudnamn toobe säkerhet som används för identitets- och åtkomstkontroll. Under en [OAuth2 authorization grant](#authorization-grant) exempelvis hello part autentisera fyller hello rollen antingen [resursägare](#resource-owner) eller [klientprogrammet](#client-application), beroende på hello bevilja används.

## <a name="authorization"></a>Auktorisering
hello act att bevilja en autentiserad säkerhet huvudbehörighet toodo något. Det finns två primära användningsområden i hello Azure AD-programmeringsmodell:

* Under en [OAuth2 authorization grant](#authorization-grant) flöde: när hello [resursägare](#resource-owner) ger auktorisering toohello [klientprogrammet](#client-application), vilket gör att klientdatorer hello tooaccess hello resursägare resurser.
* Under resursåtkomst av hello-klienten: som implementeras av hello [resursservern](#resource-server), med hjälp av hello [anspråk](#claim) värden finns i hello [åtkomsttoken](#access-token) toomake åtkomstkontroll beslut baserat på den.

## <a name="authorization-code"></a>Auktoriseringskoden
En kort livslängd ”token” tillhandahålls tooa [klientprogrammet](#client-application) av hello [autentiseringsslutpunkt](#authorization-endpoint), som en del av hello ”Auktoriseringskoden” flödet, något av hello fyra OAuth2 [auktorisering ger ](#authorization-grant). hello kod returneras toohello klientprogrammet svar tooauthentication av en [resursägare](#resource-owner), som visar hello resursägare har delegerat auktorisering tooaccess hello begärda resurser. Som en del av hello flödet, Inlöst hello kod senare för en [åtkomsttoken](#access-token).

## <a name="authorization-endpoint"></a>autentiseringsslutpunkt
En av hello-slutpunkter som implementeras av hello [auktorisering server](#authorization-server), används toointeract med hello [resursägare](#resource-owner) i ordning tooprovide en [authorization grant](#authorization-grant) under en OAuth2-auktorisering bevilja flödet. Beroende på hello auktorisering bevilja flödet, hello faktiska bevilja som kan variera, inklusive en [Auktoriseringskoden](#authorization-code) eller [säkerhetstoken](#security-token).

Visa hello OAuth2 specifikation [auktorisering bevilja typer] [ OAuth2-AuthZ-Grant-Types] och [autentiseringsslutpunkt] [ OAuth2-AuthZ-Endpoint] avsnitt och hello [OpenIDConnect specifikationen] [ OpenIDConnect-AuthZ-Endpoint] för mer information.

## <a name="authorization-grant"></a>bevilja behörighet
Autentiseringsuppgifter som representerar hello [resursägare](#resource-owner) [auktorisering](#authorization) tooaccess dess skyddade resurser, beviljas tooa [klientprogrammet](#client-application). Ett klientprogram kan använda en av hello [fyra bevilja typer som definieras i hello OAuth2 auktorisering Framework] [ OAuth2-AuthZ-Grant-Types] tooobtain bidrag, beroende på typ/klientkrav: ”authorization kod grant” ” klientens autentiseringsuppgifter bevilja ”,” implicit bevilja ”och” lösenordsinformation för resurs-ägare bevilja ”. hello autentiseringsuppgifter som har returnerats toohello klienten är antingen en [åtkomsttoken](#access-token), eller en [Auktoriseringskoden](#authorization-code) (utväxlats senare för en åtkomst-token) beroende på hello typ av auktorisering bevilja används.

## <a name="authorization-server"></a>auktorisering server
Som definierats av hello [OAuth2 auktorisering Framework][OAuth2-Role-Def], hello server ansvarar för att utfärda access token toohello [klienten](#client-application) efter autentisering Hej [resursägare](#resource-owner) och hämta dess tillstånd. En [klientprogrammet](#client-application) samverkar med hello auktorisering servern vid körning via dess [auktorisering](#authorization-endpoint) och [token](#token-endpoint) slutpunkter i enlighet med hello OAuth2 definierats [auktorisering ger](#authorization-grant).

Hello gäller integration av Azure AD, Azure AD implementerar hello auktorisering serverrollen för Azure AD-program och Microsoft service API: er, till exempel [Microsoft Graph API: er][Microsoft-Graph].

## <a name="claim"></a>Anspråk
En [säkerhetstoken](#security-token) innehåller anspråk som innehåller intyg om en enhet (som en [klientprogrammet](#client-application) eller [resursägare](#resource-owner)) tooanother entitet (till exempel hello [resursservern](#resource-server)). Anspråk är namn/värde-par som vidarebefordrar fakta om hello token område (till exempel hello säkerhetsobjekt som autentiserades av hello [auktorisering server](#authorization-server)). hello anspråk finns i en viss token är beroende av flera variabler, inklusive hello typ av token, hello typ av autentiseringsuppgifter som används för tooauthenticate hello ämne, hello programkonfigurationen osv.

Se [Azure AD tokenreferens] [ AAD-Tokens-Claims] för mer information.

## <a name="client-application"></a>klientprogram
Som definierats av hello [OAuth2 auktorisering Framework][OAuth2-Role-Def], ett program som gör skyddade resursen begäranden på uppdrag av hello [resursägare](#resource-owner). hello termen ”klient” innebär inte någon särskild maskinvaruegenskaper implementering (till exempel om programmet hello utförs på en server, en stationär dator och andra enheter).  

En klient begär [auktorisering](#authorization) från en resurs ägare tooparticipate i en [OAuth2 authorization grant](#authorization-grant) flöda och kan komma åt API: er/data för hello resursägare räkning. Hej OAuth2 auktorisering Framework [definierar två typer av klienter][OAuth2-Client-Types]”konfidentiellt” och ”offentliga”, baserat på hello klienten möjlighet toomaintain hello sekretess för sina autentiseringsuppgifter. Program kan implementera en [webbklienten (konfidentiell)](#web-client) som körs på en webbserver, en [native client (offentlig)](#native-client) installeras på en enhet eller en [användaren-agent-baserad klient (offentlig)](#user-agent-based-client) som körs i en enhetens webbläsare.

## <a name="consent"></a>Medgivande
hello av en [resursägare](#resource-owner) bevilja tillstånd tooa [klientprogrammet](#client-application), tooaccess skyddade resurser under specifika [behörigheter](#permissions), uppdrag hello resursägare. Beroende på hello behörigheter som begärs av hello klient, att administratör eller användare ombeds medgivande tooallow åtkomst tootheir organisation/enskild data respektive. Observera att i en [flera innehavare](#multi-tenant-application) scenario, hello program [tjänstens huvudnamn](#service-principal-object) också registreras i hello innehavare för hello principer användare.

## <a name="id-token"></a>ID-token
En [OpenID Connect] [ OpenIDConnect-ID-Token] [säkerhetstoken](#security-token) som tillhandahålls av en [auktorisering server](#authorization-server) [autentiseringsslutpunkt](#authorization-endpoint), som innehåller [anspråk](#claim) tillhörande toohello autentisering av en slutanvändare [resursägare](#resource-owner). Som ett åtkomsttoken, visas också ID-token som ett digitalt signerat [JSON-Webbtoken (JWT)][JWT]. Till skillnad från en åtkomst-token men används anspråk för en ID-token inte för andra syften relaterade tooresource åtkomst och specifikt åtkomstkontroll.

Se [Azure AD tokenreferens] [ AAD-Tokens-Claims] för mer information.

## <a name="multi-tenant-application"></a>program med flera innehavare
En klass för program som kan logga in och [medgivande](#consent) av användare som har etablerats i Azure AD [klient](#tenant), inklusive klienter än hello en där hello-klienten har registrerats. [-Klienten](#native-client) program är flera innehavare som standard, medan [webbklienten](#web-client) och [webb-resurs/API](#resource-server) program har hello möjlighet tooselect mellan en eller flera innehavare. Däremot ett webbprogram som är registrerat som en klient, skulle bara tillåter inloggningar från användarkonton som har etablerats i samma klient som hello en hello där programmet hello har registrerats.

Se [hur hello toosign i alla Azure AD-användare med flera innehavare programmönster] [ AAD-Multi-Tenant-Overview] för mer information.

## <a name="native-client"></a>-klienten
En typ av [klientprogrammet](#client-application) som internt är installerad på en enhet. Eftersom all kod som körs på en enhet anses en ”offentliga” klient på grund av tooits oförmåga toostore autentiseringsuppgifter privat/konfidentiellt. Se [OAuth2 klienten typer och profiler] [ OAuth2-Client-Types] för mer information.

## <a name="permissions"></a>Behörigheter
En [klientprogrammet](#client-application) får åtkomst till tooa [resursservern](#resource-server) genom att deklarera behörighetsbegäranden. Det finns två typer:

* ”Delegerad” behörigheter, som anger [scope-baserade](#scopes) med delegerad tillstånd från hello inloggade [resursägare](#resource-owner), visas toohello resurs under körning som [”scp ”anspråk](#claim) i hello klienten [åtkomsttoken](#access-token).
* ”Program” behörigheter, som anger [rollbaserad](#roles) åtkomst med hjälp av programmet hello-klientens autentiseringsuppgifter/identitet, visas toohello resurs under körning som [”roller” anspråk](#claim) i hello klientens åtkomst-token.

De kan också ansluta under hello [medgivande](#consent) processen, vilket ger Hej administratör eller resurs ägare hello möjlighet toogrant/neka hello klienten åtkomst tooresources i klientorganisationen.

Behörighetsbegäranden konfigureras på hello ”program” / ”inställningar” fliken i hello [Azure-portalen][AZURE-portal], under ”nödvändiga behörigheter”, genom att välja hello önskad ”delegerade behörigheter” och ” Programbehörigheter ”(hello senare kräver medlemskap i rollen som Global administratör hello). Eftersom en [offentliga klienten](#client-application) på ett säkert sätt kan inte upprätthålla autentiseringsuppgifter, den kan bara begära delegerade behörigheter, medan en [konfidentiell klienten](#client-application) har hello möjlighet toorequest både delegerad och program behörigheter. hello klientens [programobjektet](#application-object) lagrar hello deklarerats behörigheter i dess [requiredResourceAccess egenskapen][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>resursägare
Som definierats av hello [OAuth2 auktorisering Framework][OAuth2-Role-Def], en enhet som kan att bevilja åtkomst tooa skyddad resurs. När hello resursägare är en person, är det hänvisade tooas en slutanvändare. Till exempel när en [klientprogrammet](#client-application) vill tooaccess postlåda via hello [Microsoft Graph API][Microsoft-Graph], krävs behörighet från hello resursägare hello postlåda.

## <a name="resource-server"></a>resursservern
Som definierats av hello [OAuth2 auktorisering Framework][OAuth2-Role-Def], en server att värdar skyddade resurser, kan ta emot och svara tooprotected resurs begär av [klienten program](#client-application) som utgör en [åtkomsttoken](#access-token). Kallas även en skyddad resurs-servern eller resursprogram.

Resursservern visar API: er och tillämpar åtkomst tooits skyddade resurser via [scope](#scopes) och [roller](#roles), med hjälp av hello OAuth 2.0 auktorisering Framework. Exempel: hello Azure AD Graph API som ger åtkomst till tooAzure AD-klientdata och hello Office 365-API: er som ger åtkomst till toodata, till exempel e-post och kalender. Båda dessa är också tillgängliga via hello [Microsoft Graph API][Microsoft-Graph].  

Precis som ett klientprogram, upprättas resurs programmets identitet konfiguration [registrering](#application-registration) tillhandahåller både hello program och tjänstens huvudnamn objekt i en Azure AD-klient. Vissa Microsoft API: er, till exempel hello Azure AD Graph API har redan registrerat tjänstens huvudnamn göras tillgänglig i alla klienter under etableringen.

## <a name="roles"></a>roles
Som [scope](#scopes), roller ge ett sätt för en [resursservern](#resource-server) toogovern åtkomst tooits skyddade resurser. Det finns två typer: rollen ”användare” implementerar rollbaserad åtkomstkontroll för användare eller grupper som behöver åtkomst toohello resurs, medan en ”program” roll implementerar hello samma för [klientprogram](#client-application) som kräver åtkomst.

Roller är definierad i resursen strängar (till exempel ”utgifter godkännare”, ”skrivskyddad”, ”Directory.ReadWrite.All”), hanteras i hello [Azure-portalen] [ AZURE-portal] via hello resursens [program manifest](#application-manifest), och lagras i hello resursens [appRoles egenskapen][AAD-Graph-Sp-Entity]. hello Azure-portalen är också används tooassign användare för ”användarroller”, och konfigurera klienten [programbehörigheter](#permissions) tooaccess en ”program”-roll.

En detaljerad beskrivning av hello programroller som exponeras av Azure AD Graph API finns [Behörighetsomfattningen för Graph API][AAD-Graph-Perm-Scopes]. En stegvis implementering exempel finns [rollbaserad åtkomstkontroll i molnprogram med Azure AD][Duyshant-Role-Blog].

## <a name="scopes"></a>Scope
Som [roller](#roles), scope ge ett sätt för en [resursservern](#resource-server) toogovern åtkomst tooits skyddade resurser. Scope är används tooimplement [scope-baserade] [ OAuth2-Access-Token-Scopes] åtkomstkontroll för en [klientprogrammet](#client-application) som har angetts delegerad åtkomst toohello resurs av sin ägare.

Scope är definierad i resursen strängar (till exempel ”Mail.Read”, ”Directory.ReadWrite.All”) hanteras i hello [Azure-portalen] [ AZURE-portal] via hello resursens [programmanifestet](#application-manifest), och lagras i hello resursens [oauth2Permissions egenskapen][AAD-Graph-Sp-Entity]. hello Azure-portalen är också används tooconfigure klientprogram [delegerad behörighet](#permissions) tooaccess ett scope.

En bästa praxis namngivningskonvention, är toouse formatet ”resource.operation.constraint”. En detaljerad beskrivning av hello scope som exponeras av Azure AD Graph API finns [Behörighetsomfattningen för Graph API][AAD-Graph-Perm-Scopes]. Scope som exponeras av Office 365-tjänster, se [Office 365-API-referens för behörigheter][O365-Perm-Ref].

## <a name="security-token"></a>säkerhetstoken
Ett signerat dokument som innehåller anspråk, till exempel en OAuth2-token eller SAML 2.0-kontroll. För en OAuth2 [authorization grant](#authorization-grant), en [åtkomsttoken](#access-token) (OAuth2) och en [ID Token](http://openid.net/specs/openid-connect-core-1_0.html#IDToken) typer av säkerhetstoken som implementeras som en [JSON-Webbtoken (JWT)][JWT].

## <a name="service-principal-object"></a>Tjänstens huvudnamn objekt
När du registrera/uppdatera ett program i hello [Azure-portalen][AZURE-portal], hello skapar/uppdateringar av Företagsportalen både en [programobjektet](#application-object) och en motsvarande tjänstens huvudnamn objekt för den klienten. hello programobjektet *definierar* hello programmets identitet konfiguration globalt (över alla klienter där hello associerade program har beviljats åtkomst), och är hello mall som dess motsvarande tjänst huvudnamn objekt är *härledda* för lokalt under körning (i en viss klient).

Se [program och tjänstens huvudnamn objekt] [ AAD-App-SP-Objects] för mer information.

## <a name="sign-in"></a>Logga in
hello av en [klientprogrammet](#client-application) initierar slutanvändaren autentisering och samlar in relaterade tillstånd för hello syfte för att skaffa en [säkerhetstoken](#security-token) och scope hello programmet session toothat tillstånd. Tillstånd kan innehålla artefakter, till exempel användarens profilinformation och information som har härletts från token anspråk.

hello logga in funktionen för ett program är brukar användas tooimplement enkel inloggning (SSO). Det kan också föregås av en ”registrering” funktion som hello startpunkten för en slutanvändare toogain åtkomst tooan program (vid första inloggningen). hello registrerar funktionen är används toogather och spara ytterligare tillstånd specifika toohello användare och kan kräva [användargodkännande](#consent).

## <a name="sign-out"></a>utloggning
Hej av icke autentiserande slutanvändaren frånkoppling hello användartillstånd som är associerade med hello [klientprogrammet](#client-application) session under [inloggning](#sign-in)

## <a name="tenant"></a>Klient
En instans av en Azure AD-katalog är refererad tooas en Azure AD-klient. Det finns en mängd olika funktioner, inklusive:

* registry-tjänsten för integrerade program
* autentisering av användarkonton och registrerade program
* REST-slutpunkter krävs toosupport olika protokoll, inklusive OAuth2 och SAML, inklusive hello [autentiseringsslutpunkt](#authorization-endpoint), [tokenslutpunkten](#token-endpoint) och hello ”vanliga” slutpunkt som används av [ program med flera klienter](#multi-tenant-application).

En klient är även associerat med en Azure AD eller Office 365-prenumeration under etablering av hello-prenumeration, att tillhandahålla Identity & Access Management-funktioner för hello prenumeration. Se [hur tooget ett Azure Active Directory-klient] [ AAD-How-To-Tenant] information om hello olika sätt som du kan få åtkomst till tooa klient. Se [hur Azure-prenumerationer är associerade med Azure Active Directory] [ AAD-How-Subscriptions-Assoc] information om hello relationen mellan prenumerationer och Azure AD-klient.

## <a name="token-endpoint"></a>token för slutpunkt
En av hello-slutpunkter som implementeras av hello [auktorisering server](#authorization-server) toosupport OAuth2 [auktorisering ger](#authorization-grant). Beroende på hello bevilja används tooacquire kan det vara en [åtkomsttoken](#access-token) (och relaterade ”uppdatera” token) tooa [klienten](#client-application), eller [ID token](#ID-token) när det används med hello [ OpenID Connect] [ OpenIDConnect] protokoll.

## <a name="user-agent-based-client"></a>Användare-agent-baserad klient
En typ av [klientprogrammet](#client-application) som hämtar koden från en webbserver och kör inom en användaragent (till exempel en webbläsare), till exempel en enda sida program (SPA). Eftersom all kod som körs på en enhet anses en ”offentliga” klient på grund av tooits oförmåga toostore autentiseringsuppgifter privat/konfidentiellt. Se [OAuth2 klienten typer och profiler] [ OAuth2-Client-Types] för mer information.

## <a name="user-principal"></a>användarens huvudnamn
Liknande toohello sätt en huvudnamn tjänstobjektet ska använda toorepresent en programinstans huvudnamn användarobjektet är en annan typ av säkerhetsobjekt, som representerar en användare. hello Azure AD Graph [användarentiteten] [ AAD-Graph-User-Entity] definierar hello schemat för ett användarobjekt, inklusive användarrelaterade egenskaper och efternamn, användarens huvudnamn, directory rollmedlemskap, t.ex. Detta ger hello identitet Användarkonfiguration för Azure AD tooestablish en annan användare vid körning. hello UPN är används toorepresent en autentiserad användare för enkel inloggning, registrering [medgivande](#consent) delegering, vilket gör besluten om åtkomstkontroll, osv.

## <a name="web-client"></a>Webbklient
En typ av [klientprogrammet](#client-application) som utför all kod på en webbserver och kan toofunction som en ”konfidentiellt” klient genom att lagra sina autentiseringsuppgifter på ett säkert sätt på hello-servern. Se [OAuth2 klienten typer och profiler] [ OAuth2-Client-Types] för mer information.

## <a name="next-steps"></a>Nästa steg
Hej [Azure AD-Guide för utvecklare] [ AAD-Dev-Guide] är hello portal toouse för alla Azure AD-utveckling relaterade ämnen, t.ex. en översikt över [programintegrationstyp] [ AAD-How-To-Integrate] och hello grunderna i [scenarier för autentiseringsmetoder som stöds och Azure AD authentication][AAD-Auth-Scenarios].

Använd hello följande kommentarer avsnittet tooprovide feedback och hjälp oss att förbättra och utforma innehållet, inklusive förfrågningar efter nya definitioner eller uppdaterar befintliga!

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ../active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-portal]: https://portal.azure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
