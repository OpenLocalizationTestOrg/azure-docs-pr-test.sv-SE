---
title: "aaaAzure Active Directory v2.0-scope, behörigheter och medgivande | Microsoft Docs"
description: "En beskrivning av auktorisering hello Azure AD v2.0-slutpunkten, inklusive scope, behörigheter och samtycke."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 8f98cbf0-a71d-4e34-babf-e644ad9ff423
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 5721d368c435868bfb4ae91cff7fbb9bc4a79b66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scopes-permissions-and-consent-in-hello-azure-active-directory-v20-endpoint"></a>Scope, behörigheter och medgivande i hello Azure Active Directory v2.0-slutpunkten
Appar som integreras med Azure Active Directory (AD Azure) följer en auktoriseringsmodell som ger användare kontroll över hur en app kan komma åt sina data. hello v2.0 implementering av hello auktoriseringsmodellen har uppdaterats och ändras hur en app måste interagera med Azure AD. Den här artikeln beskriver hello grundläggande koncept för den här auktoriseringsmodellen, inklusive scope, behörigheter och samtycke.

> [!NOTE]
> hello v2.0-slutpunkten har inte stöd för alla Azure Active Directory-scenarier och funktioner. toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
>
>

## <a name="scopes-and-permissions"></a>Omfång och behörigheter
Azure AD implementerar hello [OAuth 2.0](active-directory-v2-protocols.md) authorization protocol. OAuth 2.0 är en metod som en app från tredje part kan komma åt webbaserat resurser för en användares räkning. Alla webbaserat resurser som kan integreras med Azure AD har ett resurs-ID eller *program-ID URI*. Till exempel omfattar vissa av Microsofts webbaserat resurser:

* hello Office 365 Unified e-API:`https://outlook.office.com`
* hello Azure AD Graph API:`https://graph.windows.net`
* Microsoft Graph:`https://graph.microsoft.com`

hello samma sak gäller för resurser från tredje part som har integrerat med Azure AD. Något av dessa resurser kan också definiera en uppsättning behörigheter som kan använda toodivide hello funktionerna i den här resursen i mindre delar. Exempelvis [Microsoft Graph](https://graph.microsoft.io) har definierat behörigheter toodo hello följande uppgifter, bland annat:

* Läsa användarens kalender
* Skriva tooa användares kalender
* Skicka e-post som en användare

Genom att definiera typerna av behörigheter har hello resursen detaljerad kontroll över dess data och hur hello data visas. En tredjeparts-app kan begära dessa behörigheter från en app-användare. hello app användare måste godkänna hello behörigheter innan hello appen kan fungera för hello användares räkning. Genom att dela upp hello resursens funktioner i mindre behörighetsgrupper kan appar från tredje part vara inbyggda toorequest endast hello särskilda behörigheter de behöver tooperform deras funktion. Användarna kan veta exakt hur en app använder sina data och de kan vara mer säker hello appen inte fungerar med skadliga åtgärder.

I Azure AD och OAuth dessa typer av behörigheter kallas *scope*. Ibland är refererad tooas *oAuth2Permissions*. Ett omfång representeras i Azure AD som ett strängvärde. Fortsätter med hello Microsoft Graph exempel är hello scope-värde för varje behörighet:

* Läsa användarens kalender med hjälp av`Calendar.Read`
* Skriva tooa användares kalender med hjälp av`Mail.ReadWrite`
* En användare med hjälp av genom att skicka e-post`Mail.Send`

En app kan begära dessa behörigheter genom att ange hello scope i begäranden toohello v2.0-slutpunkten.

## <a name="openid-connect-scopes"></a>OpenID Connect scope
hello v2.0 implementering av OpenID Connect har några väldefinierade scope som inte gäller tooa specifik resurs: `openid`, `email`, `profile`, och `offline_access`.

### <a name="openid"></a>openid
Om en app utför logga in med hjälp av [OpenID Connect](active-directory-v2-protocols.md), den måste begära hello `openid` omfång. Hej `openid` scope visas på hello arbetskonto medgivande sida som hello ”logga in dig på” behörighet och godkännande sida på hello personligt Microsoft-konto som hello ”visa din profil och Anslut tooapps och tjänster med ditt Microsoft-konto” behörighet. Med den här behörigheten kan en app kan ta emot en unik identifierare för hello användaren i hello form av hello `sub` anspråk. Det ger även hello app åtkomst toohello användarinformationen slutpunkt. Hej `openid` omfång kan användas i hello v2.0 tokenslutpunkten tooacquire ID-token, som kan vara används toosecure HTTP-anrop mellan olika komponenter i en app.

### <a name="email"></a>E-post
Hej `email` omfång kan användas med hello `openid` scope och övriga. Den ger hello app åtkomst toohello användarens primära e-postadress i formatet hello hello `email` anspråk. Hej `email` anspråk ingår i en token endast om en e-postadress som är associerad med hello-användarkonto som inte är alltid fallet hello. Om det använder hello `email` omfång, din app ska vara förberedd toohandle ett ärende i vilka hello `email` anspråk finns inte i hello-token.

### <a name="profile"></a>Profil
Hej `profile` omfång kan användas med hello `openid` scope och övriga. Den ger hello app åtkomst tooa stor mängd information om hello användare. hello information som den kan komma åt inkluderar, men begränsat inte till, hello användarens förnamn, efternamn, prioriterade användarnamn och objekt-ID. En fullständig lista över hello profil anspråk som är tillgängliga i hello id_tokens parameter för en viss användare finns hello [v2.0 tokens referens](active-directory-v2-tokens.md).

### <a name="offlineaccess"></a>offline_access
Hej [ `offline_access` omfång](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) ger din app åtkomst tooresources hello användarens räkning som en längre tid. Hello arbete konto medgivande på sidan visas detta scope som hello ”åtkomst till data när som helst” behörighet. Hello personliga Microsoft-konto medgivande på sidan visas den som hello ”komma åt din information när som helst” behörighet. När en användare godkänner hello `offline_access` omfång, din app kan ta emot uppdaterings-tokens från token hello v2.0-slutpunkten. Uppdateringstoken är långlivade. Din app kan få nya åtkomsttoken när äldsta upphör.

Om din app inte begär hello `offline_access` omfång och kan den inte fick uppdaterings-tokens. Detta innebär att när du lösa in en auktoriseringskod i hello [OAuth 2.0-auktoriseringskodflödet](active-directory-v2-protocols.md), får du endast en åtkomst-token från hello `/token` slutpunkt. hello åtkomst-token är giltig för en kort tid. hello åtkomst-token upphör vanligtvis på en timme. Då din app måste tooredirect hello användaren tillbaka toohello `/authorize` endpoint tooget en ny auktoriseringskod. Under den här omdirigering, beroende på hello typen av app måste kanske hello användare behöver tooenter sina autentiseringsuppgifter igen eller medgivande igen toopermissions.

Mer information om hur tooget och använda uppdatera token finns hello [protokollreferens för v2.0](active-directory-v2-protocols.md).

## <a name="requesting-individual-user-consent"></a>Begära samtycke för enskilda användare
I en [OpenID Connect och OAuth 2.0](active-directory-v2-protocols.md) auktorisering begär en app kan begära hello behörigheter som krävs med hjälp av hello `scope` Frågeparametern. När en användare loggar in tooan app, skickar hello appen en begäran som hello följande exempel (med radbrytningar som lagts till för läsbarhet):

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendar.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

Hej `scope` parametern är en blankstegsavgränsad lista över scope som hello app begär. Varje område anges genom att lägga till hello scope värdet toohello resursens identifierare (hello program-ID URI). I exemplet begäran hello måste hello appen behörighet tooread hello användarens kalender och skicka e-post som hello användare.

När hello användaren anger sina autentiseringsuppgifter hello v2.0-slutpunkten söker efter en matchande post av *användargodkännande*. Om hello användaren inte har godkänt tooany av hello begärt behörigheter i hello tidigare, hello v2.0-slutpunkten frågar hello användaren toogrant hello begärt behörigheter.

![Fungerar konto medgivande](../../media/active-directory-v2-flows/work_account_consent.png)

När hello användaren godkänner hello behörighet, registreras hello medgivande så att hello användaren inte har tooconsent igen på efterföljande kontoinloggningar.

## <a name="requesting-consent-for-an-entire-tenant"></a>Begäran om godkännande för en hel klient
Ofta när organisationen köper en licens eller prenumeration för ett program, hello organisationen vill toofully etablera hello program till sina anställda. Som en del av den här processen kan kan en administratör ge ditt medgivande för hello programmet tooact uppdrag anställda. Om Hej administratör ger ditt medgivande för hela hello-klient, ser hello organisationens anställda en medgivande sida för hello program.

toorequest medgivande för alla användare i en klient, din app kan använda hello adminslutpunkten medgivande.

## <a name="admin-restricted-scopes"></a>Begränsat scope
Vissa höga behörigheter i hello Microsoft ekosystem kan anges för*begränsat*. Exempel på dessa typer av scope är hello följande behörigheter:

* Läsa katalogdata i en organisation med hjälp av`Directory.Read`
* Skriva data tooan organisations katalog med hjälp av`Directory.ReadWrite`
* Läsa säkerhetsgrupper i organisationens katalog med hjälp av`Groups.Read.All`

Även om en konsument-användare kan ge ett program åtkomst toothis typ av data, begränsad organisationens användare från att bevilja åtkomst toohello samma uppsättning känsliga företagsdata. Om ditt program begär åtkomst tooone dessa behörigheter från en organisations användare, får hello användaren ett felmeddelande som säger att de inte är auktoriserade tooconsent tooyour app-behörigheter.

Om din app kräver tooadmin begränsat scope för organisationer kan begära du dem direkt från en företagsadministratör också med hjälp av hello admin medgivande slutpunkt som beskrivs nedan.

När en administratör ger dessa behörigheter via Hej administratör godkänna endpoint, beviljas medgivande för alla användare i hello-klient.

## <a name="using-hello-admin-consent-endpoint"></a>Med hjälp av hello admin medgivande-slutpunkten
Om du gör följande appen kan samla in behörigheter för alla användare i en klient, inklusive begränsat scope. toosee kodexempel som implementerar hello steg finns hello [begränsat scope exempel](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

### <a name="request-hello-permissions-in-hello-app-registration-portal"></a>Begär hello behörighet i portalen för registrering av hello-app
1. Gå tooyour programmet hello [Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller [skapa en app](active-directory-v2-app-registration.md) om du inte redan har gjort.
2. Leta upp hello **Microsoft Graph behörigheter** avsnittet och Lägg sedan till hello behörigheter som krävs för din app.
3. Se till att du **spara** hello appregistrering.

### <a name="recommended-sign-hello-user-in-tooyour-app"></a>Rekommenderat: Logga hello användaren i tooyour app
Vanligtvis när du skapar ett program som använder hello adminslutpunkten medgivande, hello appen måste en sida eller en vy i vilka Hej administratör kan godkänna hello app-behörigheter. Den här sidan kan vara en del av registreringen hello app-flöde, en del av hello app-inställningar, eller så kan vara ett dedikerat ”ansluta” flöde. I många fall klokt det för hello app tooshow detta ”ansluta” Visa endast när en användare har loggat in med ett arbets- eller skolkonto Microsoft-konto.

När du registrerar hello användaren i tooyour app kan du identifiera hello organisation toowhich Hej administratör tillhör innan ber dem tooapprove hello-behörighet. Även om det inte är absolut nödvändigt, hjälper som dig att skapa en mer intuitiv upplevelse för din organisations användare. toosign hello användare i Följ våra [v2.0-protokollet självstudier](active-directory-v2-protocols.md).

### <a name="request-hello-permissions-from-a-directory-admin"></a>Begär hello behörighet från en katalogadministratör
När du är klar toorequest behörigheter från din organisations administratör kan du omdirigera hello användaren toohello v2.0 *medgivande adminslutpunkten*.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting hello below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parameter | Villkor | Beskrivning |
| --- | --- | --- |
| Klient |Krävs |hello directory-klient som du vill toorequest tillstånd. Kan anges i GUID- eller format för eget namn. |
| client_id |Krävs |hello program-ID som hello [Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) tilldelade tooyour app. |
| redirect_uri |Krävs |hello omdirigerings-URI där du vill att hello svar toobe skickas för din app toohandle. Den måste matcha en hello omdirigerings-URI: er som du har registrerat i portalen för registrering av hello-app. |
| state |Rekommenderas |Ett värde som ingår i hello-begäran som returneras också hello token svar. Det kan vara en sträng med innehåll. Du kan använda hello tooencode statusinformation om hello användarens tillstånd i hello app innan hello autentiseringsbegäran inträffade, exempelvis hello sida eller visa de befann sig i. |

Azure AD kräver nu en klient administratören toosign i toocomplete hello begäran. Hej administratör uppmanas tooapprove alla hello behörigheter som du har begärt för din app i portalen för registrering av hello-app.

#### <a name="successful-response"></a>Lyckat svar
Om Hej administratör godkänner hello behörigheter för din app, hello lyckat svar som ser ut så här:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Beskrivning |
| --- | --- | --- |
| Klient |hello directory-klient som beviljats behörighet programmet hello det begäras, i GUID-format. |
| state |Ett värde som ingår i hello-begäran som också kommer att returneras i hello token svar. Det kan vara en sträng med innehåll. hello tillstånd är används tooencode information om hello användarens tillstånd i hello app innan hello autentiseringsbegäran inträffade, exempelvis hello sida eller vy som de var på. |
| admin_consent |Ställs in för**SANT**. |

#### <a name="error-response"></a>Felsvar
Om Hej administratör inte godkänner hello behörigheter för din app misslyckades hello svar ser ut så här:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Beskrivning |
| --- | --- | --- |
| fel |Ett felkod sträng som kan använda tooclassify typer av fel som inträffar och kan vara används tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa utvecklare identifiera hello orsaken till felet. |

När du har fått ett lyckat svar från hello adminslutpunkten medgivande, har din app fått hello behörigheter begärde. Därefter kan du begära en token för hello-resurs som du vill.

## <a name="using-permissions"></a>Med behörigheter
När hello användaren godkänner toopermissions för din app, kan din app hämta åtkomsttoken som representerar appens behörighet tooaccess en resurs i vissa kapacitet. En åtkomst-token kan endast användas för en enskild resurs, men kodad i hello åtkomst-token är varje behörighet som din app har beviljats för den här resursen. tooacquire en åtkomst-token appen kan göra en förfrågan toohello v2.0 tokenslutpunkten, så här:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

Du kan använda hello resulterande åtkomst-token i HTTP-begäranden toohello resursen. På ett tillförlitligt sätt anger toohello resurs att din app har hello rätt behörighet tooperform en viss uppgift.  

Mer information om hello OAuth 2.0-protokollet och hur tooget åtkomsttoken, se hello [protokollreferens för v2.0-slutpunkten](active-directory-v2-protocols.md).
