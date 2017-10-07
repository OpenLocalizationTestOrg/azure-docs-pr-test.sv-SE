---
title: "aaaWhat skiljer sig åt i hello Azure AD v2.0-slutpunkten? | Microsoft Docs"
description: "En jämförelse mellan hello ursprungliga Azure AD och hello v2.0 slutpunkter."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: e7ed196f9053fc21db799cd6bc513ba5c2b92885
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="whats-different-about-hello-v20-endpoint"></a>Vad är skillnaden mellan hello v2.0-slutpunkten?
Om du är bekant med Azure Active Directory eller ha integrerade appar med Azure AD i hello tidigare, kan det finnas vissa skillnader i hello v2.0-slutpunkten som du inte förväntar dig.  Det här dokumentet visar dessa skillnader för din förståelse.

> [!NOTE]
> Inte alla Azure Active Directory-scenarier och funktioner som stöds av hello v2.0-slutpunkten.  toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
>

## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Microsoft-konton och Azure AD-konton
hello v2.0-slutpunkten kan utvecklare toowrite appar som accepterar logga in från både Microsoft Accounts och Azure AD-konton med hjälp av en enda auth-slutpunkt.  Det här ger du hello möjlighet toowrite appen helt konto-oberoende; Det kan vara ignorant av hello typ av konto som hello användaren loggar in med.  Naturligtvis kan du *kan* göra din app medveten om hello typ av konto som används i en viss session, men du behöver.

Till exempel om din app anropar hello [Microsoft Graph](https://graph.microsoft.io), vissa ytterligare funktioner och data blir tillgängliga tooenterprise användare, till exempel sina SharePoint-webbplatser eller katalogdata.  Men för många åtgärder, t.ex [läsa en användares e-post](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), hello kod kan skrivas exakt hello samma för både Microsoft Accounts och Azure AD-konton.  

Integrera din app med Azure AD och Microsoft Accounts är nu en enkel process.  Du kan använda en enda uppsättning slutpunkter, ett enda bibliotek och en enda app registrering toogain åtkomst tooboth hello konsumenten och enterprise världar.  toolearn mer om Hej v2.0-slutpunkten, kolla [hello översikt](active-directory-appmodel-v2-overview.md).

## <a name="new-app-registration-portal"></a>Portalen för registrering av nya app
tooregister en app som fungerar med hello v2.0-slutpunkten måste du använda en ny app-portalen för registrering: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Detta är hello portal där du kan hämta ett program-ID, anpassa hello utseendet på inloggningssidan för din app och mycket mer.  Allt du behöver tooaccess hello portal är en påslagen microsoftkonto - personlig eller arbete/Skol-konto.

## <a name="one-app-id-for-all-platforms"></a>En app-ID för alla plattformar
Om du har använt Azure Active Directory, har du förmodligen registrerad flera olika program för ett projekt.  Till exempel om du har skapat både en webbplats och en iOS-app hade tooregister dem separat, med två olika program-ID: N. hello Azure AD-portalen för registrering av app måste du toomake denna skillnad under registreringen:

![Gamla programregistrering UI](../../media/active-directory-v2-flows/old_app_registration.PNG)

På samma sätt om du har en webbplats och en serverdel webb-api, kanske du har registrerat varje som en separat app i Azure AD.  Eller om du en iOS-app och en Android-app måste du även kan har registrerat två olika appar.  Registrering av varje komponent i ett program ledde toosome oväntat beteende för utvecklare och sina kunder:

* Varje komponent visades som en separat app i hello Azure Active Directory-klient för varje kund.
* När en Innehavaradministratör tooapply princip för att hantera åtkomst till eller ta bort en app, de skulle ha toodo så för varje komponent i hello app.
* När kunder godkänt tooan program, visas varje komponent i hello medgivande skärmen som olika program.

Med hello v2.0-slutpunkten kan du nu registrera alla komponenter i ditt projekt som en enda app-registrering och använder ett enda program-Id för hela projektet.  Du kan lägga till flera ”plattformar” tooa varje projekt och lämna hello lämpliga uppgifter för varje plattform som du lägger till.  Naturligtvis kan du skapa så många appar som du vill beroende på dina krav, men bara en program-Id för hello flertalet fall bör vara nödvändigt.

Vårt mål är att det kommer leda tooa mer förenklad hantering och utvecklingsarbetet och skapa en mer samlad vy över ett projekt som du kanske arbetar på.

## <a name="scopes-not-resources"></a>Scope, inte resurser
I Azure Active Directory, en app kan fungera som en **resurs**, eller en mottagare av token.  En resurs kan definiera ett antal **scope** eller **oAuth2Permissions** som den förstår så att klientprogram toorequest token toothat resurs för en viss uppsättning scope.  Tänk hello Azure AD Graph API är ett exempel på en resurs:

* Resurs-ID eller `AppID URI`:`https://graph.windows.net/`
* Scope, eller `OAuth2Permissions`: `Directory.Read`, `Directory.Write`osv.  

Allt detta gäller för hello hello v2.0-slutpunkten.  En app kan fortfarande fungera som resurs, definiera omfång och identifieras av en URI.  Klientprogram kan fortfarande begära toothose scope.  Dock har hello sätt som en klient begär de behörigheterna som ändrats.  Godkänn begäran tooAzure AD kanske har tittat som i hello förbi en OAuth 2.0:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

Hej där **resurs** parameter anges vilken resurs hello-klientappen begär tillstånd för.  Azure AD beräknas hello behörigheter som krävs av hello appen baserat på statisk konfiguration i hello Azure-portalen och därefter utfärdade token.  Nu godkänna hello samma OAuth 2.0 begäran ser ut som:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Hej där **omfång** parametern anger vilken resurs och behörigheter hello app begär tillstånd för. hello önskade resursen finns fortfarande mycket i hello begäran - finns bara i hello värden för hello scope-parametern.  Parametern på hello-scope på detta sätt kan hello v2.0-slutpunkten toobe mer kompatibel med hello OAuth 2.0-specifikationen och justerar närmare med vanliga branschens praxis.  Det gör också att appar tooperform [inkrementell medgivande](#incremental-and-dynamic-consent), som beskrivs i nästa avsnitt om hello.

## <a name="incremental-and-dynamic-consent"></a>Inkrementell och dynamiska medgivande
Appar som registrerats i Azure AD som tidigare behövs toospecify sina OAuth 2.0-behörigheter som krävs i hello Azure-portalen vid tidpunkten för skapandet av app:

![Behörigheter för registrering av Användargränssnittet](../../media/active-directory-v2-flows/app_reg_permissions.PNG)

hello behörigheter för en app krävs konfigurerades **statiskt**.  När detta tillåts konfigurationen av hello app tooexist i hello Azure-portalen och hålls hello kod nice och enkel uppstår några problem för utvecklare:

* En app hade tooknow alla hello behörigheter behöver någonsin vid tidpunkten för skapandet av appen.  Lägga till behörigheter över tid var svåra.
* En app hade tooknow alla hello resurser som den skulle någonsin komma i förväg.  Det var svårt toocreate appar som kan komma åt ett godtyckligt antal resurser.
* En app hade toorequest alla hello behörigheter behöver någonsin vid hello användarens första inloggning.  I vissa fall kan detta ledde tooa mycket lång lista med behörigheter som rekommenderas inte slutanvändare från godkänner hello app-åtkomst på första inloggning.

Med hello v2.0-slutpunkten kan du ange behörigheter för hello din app måste **dynamiskt**, vid körning under vanlig användning av din app.  toodo så kan du ange hello scope app behov vid en viss tidpunkt genom att inkludera dem i hello `scope` parametern för en begäran om godkännande:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

hello ovan begär behörighet för hello app tooread en Azure AD-användare katalogdata, samt Skriv tootheir datakatalog.  Om hello användaren har godkänt toothose behörigheter i hello tidigare för den här specifika appen de kommer bara att ange sina autentiseringsuppgifter och loggas in hello app.  Om hello användaren inte har godkänt tooany dessa behörigheter, begär hello v2.0-slutpunkten hello användaren medgivande toothose behörigheter.  toolearn mer, du kan läsa mer om [behörigheter, medgivande och scope](active-directory-v2-scopes.md).

Att tillåta att en app toorequest behörigheter dynamiskt via hello `scope` parametern ger dig fullständig kontroll över din användarupplevelse.  Om du vill kan välja du toofrontload ditt medgivande får och fråga efter alla behörigheter i en begäran för ursprungliga tillståndet.  Eller om appen kräver ett stort antal behörigheter, kan du välja toogather dessa behörigheter från hello användaren inkrementellt, som försök toouse vissa funktioner i din app med tiden.

## <a name="well-known-scopes"></a>Välkända scope
#### <a name="offline-access"></a>Offline-åtkomst
Appar som använder hello v2.0-slutpunkten kan kräva hello användning av en ny välkända behörighet för appar – hello `offline_access` omfång.  Alla appar måste toorequest behörigheten om de behöver tooaccess resurser på en användares vägnar hello under en längre tidsperiod, även när hello användaren inte aktivt använder hello app.  Hej `offline_access` omfattningen visas toohello användare i ditt medgivande dialogrutor som ”åtkomst till data offline” som hello-användare måste acceptera att.  Begär hello `offline_access` behörighet kommer att aktivera din web app tooreceive OAuth 2.0 refresh_tokens från hello v2.0-slutpunkten.  Refresh_tokens är långlivade och kan utbyta för nya OAuth 2.0-access_tokens under långa perioder åtkomst.  

Om din app inte begär hello `offline_access` omfång, får den inte refresh_tokens.  Det innebär att när du lösa in en authorization_code i hello OAuth 2.0-auktoriseringskodflödet endast får du tillbaka en access_token från hello `/token` slutpunkt.  Den access_token förblir giltig för en kort tidsperiod (vanligtvis en timme), men till slut upphör att gälla.  Då i tid, din app behöver tooredirect hello användaren tillbaka toohello `/authorize` endpoint tooretrieve nya authorization_code.  Under den här omdirigering hello användare kan kan eller inte behöver tooenter sina autentiseringsuppgifter igen eller nytt medgivande toopermissions, beroende på hello hello typen av app.

Mer om OAuth 2.0 och refresh_tokens access_tokens, checka ut hello toolearn [protokollreferens för v2.0](active-directory-v2-protocols.md).

#### <a name="openid-profile-and-email"></a>OpenID profil och e-post
Tidigare skulle hello mest grundläggande OpenID Connect inloggning-flöde med Azure Active Directory innehåller en mängd information om hello användare i hello resulterande id_token.  hello anspråk i en id_token kan innehålla hello användarens namn, prioriterade användarnamn, e-postadress, objekt-ID och mer.

Vi kan nu begränsa hello information som hello `openid` omfång ger din appåtkomst till.  hello 'openid' omfång kommer endast att appen toosign hello användare i och ta emot en appspecifika identifierare för hello användare.  Om du vill tooobtain personligt identifierbar information (PII) om hello användare i din app behöver din app toorequest ytterligare behörigheter från hello användare.  Vi introducerar två nya scope – hello `email` och `profile` scope – som gör att du toodo så.

Hej `email` scope är mycket enkelt – det gör att din app åtkomst toohello användarens primära e-postadress via hello `email` anspråk i hello id_token.  Hej `profile` omfång ger din app åtkomst tooall andra grundläggande information om hello användare – deras namn, prioriterade username, objekt-ID och så vidare.

Detta gör att du toocode din app på en minimal avslöjande sätt – du kan endast be hello användaren för hello uppsättning information att appen kräver toodo fjäderlayoutjobbet.  Mer information om dessa scope finns för[hello v2.0 omfattningsreferens](active-directory-v2-scopes.md).

## <a name="token-claims"></a>Token anspråk
hello anspråk i token som utfärdats av hello v2.0-slutpunkten är inte identiska tootokens som utfärdats av slutpunkter hello allmänt tillgängliga Azure AD - appar som migrerar toohello ny tjänst anta inte att ett visst anspråk kommer att finnas i id_tokens eller access_tokens. toolearn om hello specifika anspråk som hänvisas till i v2.0 token finns hello [v2.0 tokenreferens](active-directory-v2-tokens.md).

## <a name="limitations"></a>Begränsningar
Det finns några begränsningar toobe medveten om när du använder hello v2.0 punkt.  Se toohello [v2.0 begränsningar doc](active-directory-v2-limitations.md) toosee om någon av dessa begränsningar gäller tooyour visst scenario.
