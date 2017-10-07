---
title: "aaaDeveloper vägledning för Azure Active Directory villkorlig åtkomst | Microsoft Docs"
description: "Guide för utvecklare och scenarier för villkorlig åtkomst i Azure AD"
services: active-directory
keywords: 
author: danieldobalian
manager: mbaldwin
editor: PatAltimore
ms.author: dadobali
ms.date: 07/19/2017
ms.assetid: 115bdab2-e1fd-4403-ac15-d4195e24ac95
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.openlocfilehash: 589393f5d084d64872b372d895dc889f300592bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="developer-guidance-for-azure-active-directory-conditional-access"></a>Guide för utvecklare för villkorlig åtkomst i Azure Active Directory

Azure Active Directory (AD) och erbjuder flera sätt toosecure din app och skydda en tjänst.  En av dessa unika funktioner är villkorlig åtkomst.  Villkorlig åtkomst kan utvecklare och enterprise-kunder tooprotect tjänster i en mängd olika sätt, inklusive:

* Multi-Factor Authentication
* Att tillåta att Intune endast registrerade enheter tooaccess specifika tjänster
* Begränsa användarplatser och IP-adressintervall

Mer information om hello alla funktioner för villkorlig åtkomst finns [villkorlig åtkomst i hello klassiska Azure-portalen](../active-directory-conditional-access.md). 

I den här artikeln fokuserar vi på vilka villkorlig åtkomst: toodevelopers bygga appar för Azure AD.  Den förutsätter kunskaper om [enda](active-directory-integrating-applications.md) och [flera innehavare](active-directory-devhowto-multi-tenant-overview.md) appar och [vanliga autentisering mönster](active-directory-authentication-scenarios.md).

Vi kommer fördjupa dig i hello effekten av att komma åt resurser som du inte har kontroll över och som kan ha principer för villkorlig åtkomst tillämpas.  Dessutom undersöka vi hello villkorlig åtkomst i hello på-flöde, webbappar, komma åt hello Microsoft Graph och anropar API: er.

## <a name="how-does-conditional-access-impact-an-app"></a>Hur påverkar en app av villkorlig åtkomst?

### <a name="app-topologies-impacted"></a>App-topologier som påverkas

I de flesta vanliga fall villkorlig åtkomst ändra inte appens beteende, eller kräver ändringar från hello-utvecklare.  Endast i vissa fall när en app, indirekt eller obevakat begär en token för en tjänst, kräver en app kod ändrar toohandle villkorlig åtkomst ”utmaningar”.  Det kan vara så enkelt som att utföra en begäran för interaktiv inloggning. 

Mer specifikt hello följande scenarier kräver kod toohandle villkorlig åtkomst ”utmaningar”: 

* Appar som har åtkomst till hello Microsoft Graph
* Appar som utför hello på-flöde
* Appar som har åtkomst till flera services-resurser
* Ensidesappar med ADAL.js

Principer för villkorlig åtkomst kan vara tillämpade toohello app, men kan även vara tillämpade tooa web API appen har åtkomst till. toolearn mer om hur tooconfigure en villkorlig åtkomstprincip finns [komma igång med Azure Active Directory villkorlig åtkomst](../active-directory-conditional-access-azuread-connected-apps.md#configure-per-application-access-rules).

Beroende på scenario hello en enterprise-kund tillämpa och ta bort principer för villkorlig åtkomst när som helst.  För att din app toocontinue fungerar när en ny princip används måste tooimplement hello ”challenge” hantering. hello följande exempel illustrerar challenge hantering. 

### <a name="conditional-access-examples"></a>Exempel på villkorlig åtkomst

Vissa scenarier kräver kod ändringar toohandle villkorlig åtkomst medan andra fungerar som är.  Här följer några scenarier med hjälp av villkorlig åtkomst toodo multifaktorautentisering som ger viss insikt om hello skillnaden.

* Du skapar en enskild klient iOS-app och tillämpa en princip för villkorlig åtkomst.  hello appen loggar in en användare och begära inte åtkomst tooan API.  När hello användaren loggar in, hello principen aktiveras automatiskt och hello användare behöver tooperform multifaktorautentisering (MFA). 
* Du skapar ett webbprogram med flera innehavare som använder hello Microsoft Graph tooaccess Exchange, bland annat.  En enterprise-kunder som antar den här appen anger en princip för SharePoint Online.  När hello webbprogrammet begär en token för MS Graph, används en princip på alla Microsoft Service (särskilt tjänster som kan nås via diagram).  Slutanvändaren uppmanas att ange MFA. I fallet hello hello slutanvändaren har loggat in med ogiltig token, anspråk ”svårt” returneras toohello webbprogram.  
* Du skapar en intern app som använder en mellannivå service tooaccess hello Microsoft Graph.  En enterprise-kund på hello företag som använder den här appen gäller en princip tooExchange Online.  När en slutanvändare loggar in visar hello inbyggda appen begär åtkomst toohello mellannivå och skickar hello-token.  hello mellannivå utför på-flöde toorequest åtkomst toohello MS Graph.  Nu visas en anspråk ”utmaning” toohello mellannivå. hello mellannivå skickar hello challenge tillbaka toohello inbyggda appen, som måste toocomply med hello principen för villkorlig åtkomst.

### <a name="complying-with-a-conditional-access-policy"></a>Uppfyller en princip för villkorlig åtkomst

En princip för villkorlig åtkomst utvärderas för flera olika app topologier när hello-sessionen har upprättats.  Som en princip för villkorlig åtkomst fungerar på hello Granulariteten för appar och tjänster, hello punkt då den anropas kraftigt beror på hello scenario du försöker tooaccomplish.

Din app försöker tooaccess en tjänst med en princip för villkorlig åtkomst, kan det uppstå en utmaning för villkorlig åtkomst.  Denna utmaning har kodats i hello `claims` parameter som ingår i ett svar från Azure AD eller hello Microsoft Graph.  Här är ett exempel på den här challenge-parametern: 

```
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

Utvecklare kan ta denna utmaning och lägga till en ny begäran tooAzure AD.  Skicka det här tillståndet uppmanar hello slutanvändarens tooperform någon åtgärd krävs toocomply med hello principen för villkorlig åtkomst. I följande scenarier hello, beskrivs specifika hello fel och hur tooextract hello parametern. 

## <a name="scenarios"></a>Scenarier

### <a name="prerequisites"></a>Krav

Azure AD villkorlig åtkomst är en funktion som ingår i [Azure AD Premium](../active-directory-whatis.md#choose-an-edition).  Du kan lära dig mer om licensiering krav i hello [olicensierad användningsrapporten](../active-directory-conditional-access-unlicensed-usage-report.md).  Utvecklare kan ansluta till hello [Microsoft Developer Network](https://msdn.microsoft.com/dn308572.aspx), som innehåller en kostnadsfri prenumeration toohello Enterprise Mobility Suite som innehåller Azure AD Premium.

### <a name="considerations-for-specific-scenarios"></a>Överväganden för specifika scenarier

hello efter endast gäller i dessa scenarier för villkorlig åtkomst:

* Appar som har åtkomst till hello Microsoft Graph
* Appar som utför hello på-flöde
* Appar som har åtkomst till flera services-resurser
* Ensidesappar med ADAL.js

I följande avsnitt hello, ska vi gå in vanliga scenarier som är mer komplexa.  hello core fungerar principen är villkorlig åtkomst utvärderas principer för närvarande hello hello token har begärts för hello-tjänst som har en villkorlig åtkomstprincip som tillämpas om den som kan nås via hello Microsoft Graph.

### <a name="scenario-app-accessing-hello-microsoft-graph"></a>Scenario: App åtkomst till hello Microsoft Graph

I det här scenariot går vi igenom hello fallet när ett webbprogram begär åtkomst toohello Microsoft Graph. hello principen för villkorlig åtkomst kunde i det här fallet tilldelas tooSharePoint, Exchange och andra tjänster som används som en arbetsbelastning via hello Microsoft Graph.  I vårt exempel antar vi att det finns en princip för villkorlig åtkomst på Sharepoint Online.

![Appen åtkomst till hello Flödesdiagram för Microsoft Graph](media/active-directory-conditional-access-developer/app-accessing-microsoft-graph-scenario.png)

hello app först begär tillstånd toohello Microsoft Graph som kräver åtkomst till en underordnad arbetsbelastning utan villkorlig åtkomst.  hello begäran lyckas utan att anropa princip och hello appen tar emot token för Microsoft Graph.  Hello appen kan nu använda hello åtkomst-token i en ägar-begäran för hello-slutpunkt som begärdes. Nu måste hello appen tooaccess en Sharepoint Online-slutpunkt för Microsoft Graph, till exempel:`https://graph.microsoft.com/v1.0/me/mySite`

hello app har redan en giltig token för hello Microsoft Graph, så att den kan utföra hello ny begäran utan att utfärda en ny token. Denna begäran misslyckas och en utmaning anspråk utfärdas från Microsoft Graph i hello form av en HTTP 403 Åtkomst nekas med en ```WWW-Authenticate``` utmaning.
Här är ett exempel på hello svar: 

```
HTTP 403; Forbidden 
error=insufficient_claims
www-authenticate="Bearer realm="", authorization_uri="https://login.windows.net/common/oauth2/authorize", client_id="<GUID>", error=insufficient_claims, claims={"access_token":{"polids":{"essential":true,"values":["<GUID>"]}}}"
```

hello anspråk utmaning är inuti hello ```WWW-Authenticate``` rubriken, som kan vara parsade tooextract hello anspråk parametern för hello nästa begäran.  När det har lagts till toohello ny begäran vet tooevaluate hello principen för villkorlig åtkomst i Azure AD när du loggar in hello användar- och hello appen är nu efterlever hello principen för villkorlig åtkomst.  Upprepa hello begäran toohello Sharepoint Online lyckas endpoint.

Kodexempel som visar hur toohandle hello anspråk challenge finns toohello [.NET Desktop kodexemplet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) för ADAL .NET eller hello [On-behalf-of kodexemplet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca) för ADAL .NET.

### <a name="scenario-app-performing-hello-on-behalf-of-flow"></a>Scenario: App utför hello på-flöde

I det här scenariot går vi igenom hello fall där en intern app anropar en webbtjänstens/API.  I sin tur den här tjänsten har [hello ”på-” flöde](active-directory-authentication-scenarios.md#application-types-and-scenarios) toocall en underordnad tjänst.  I vårt fall vi har använt våra villkorlig åtkomst princip toohello underordnade-tjänsten (Web API 2) och använder en intern app i stället för en server-daemon-app. 

![Appen utför hello on-behalf-of flödesdiagram](media/active-directory-conditional-access-developer/app-performing-on-behalf-of-scenario.png)

hello inledande Tokenbegäran för Web API 1 frågar inte hello slutanvändarens för multifaktorautentisering som Web API 1 inte kanske alltid träffar hello underordnade API.  När Web API 1 försöker toorequest en token on-behalf-of-hello-användare för Web API 2, misslyckas hello begäran eftersom hello användaren inte har loggat in med multifaktorautentisering.

Azure AD returnerar ett HTTP-svar med vissa intressanta data: 

> [!NOTE]
> I den här instansen är det en beskrivning för multifaktorautentisering fel, men det finns en mängd olika `interaction_required` möjliga tillhörande tooconditional åtkomst.  

```
HTTP 400; Bad Request 
error=interaction_required
error_description=AADSTS50076: Due tooa configuration change made by your administrator, or because you moved tooa new location, you must use multi-factor authentication tooaccess '<Web API 2 App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
```

I vår Web API-1 vi fånga hello fel `error=interaction_required`, och skickar tillbaka hello `claims` challenge toohello skrivbordsapp.  Vid den punkten hello desktop-appen kan göra en ny `acquireToken()` anropa och Lägg till hello `claims`utmaning som en extra frågesträngparametern.  Den här nya begäran kräver hello användaren toodo multifaktorautentisering och skicka den här nya token tillbaka tooWeb API-1 och fullständig hello på-flöde.

tootry i det här scenariot finns våra [.NET kodexemplet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca).  Den visar hur toopass hello anspråk challenge tillbaka från Web API 1 toohello inbyggda appen och skapa en ny begäran i hello-klientappen. 

### <a name="scenario-app-accessing-multiple-services"></a>Scenario: App åtkomst till flera tjänster

I det här scenariot går vi igenom hello fall där ett webbprogram har åtkomst till två tjänster som har en princip för villkorlig åtkomst som tilldelats.  Beroende på din app-logik kan det finnas en sökväg som din app inte kräver åtkomst tooboth webbtjänster.  I det här scenariot spelar hello ordning i vilken du begära en token en viktig roll i hello slutanvändarens upplevelse.

Antar vi att vi har webbtjänsten A och B och webbtjänsten B har vår principen för villkorlig åtkomst tillämpas.  Medan hello inledande interaktiva auth begäran kräver godkännande för båda tjänsterna, krävs inte hello principen för villkorlig åtkomst i samtliga fall.  Om hello app begär en token för webbtjänsten B, hello princip anropas och efterföljande förfrågningar för webbtjänsten A också lyckas på följande sätt.

![Appen åtkomst till Flödesdiagram för flera tjänster](media/active-directory-conditional-access-developer/app-accessing-multiple-services-scenario.png)

Du kan också om hello app först begär en token för web service A, anropar hello slutanvändarens inte hello principen för villkorlig åtkomst.  Detta gör hello apputvecklaren toocontrol hello slutanvändarens upplevelse och tvinga inte hello villkorlig åtkomst princip toobe anropas i samtliga fall. hello komplicerade fallet är om hello app därefter begär en token för webbtjänsten B. Nu måste slutanvändaren hello toocomply med hello principen för villkorlig åtkomst.  När hello app försöker för`acquireToken`, det kan generera hello följande fel (illustreras i följande diagram hello): 

```
HTTP 400; Bad Request
error=interaction_required
error_description=AADSTS50076: Due tooa configuration change made by your administrator, or because you moved tooa new location, you must use multi-factor authentication tooaccess '<Web API App/Client ID>'.
claims={"access_token":{"polids":{"essential":true,"Values":["<GUID>"]}}}
``` 

![Appen åtkomst till flera tjänster som begär en ny token](media/active-directory-conditional-access-developer/app-accessing-multiple-services-new-token.png)

Om hello app använder ADAL hello-biblioteket, försöks alltid en token med fel tooacquire hello interaktivt.  När denna interaktiva begäran inträffar måste hello slutanvändaren hello möjlighet toocomply med hello villkorlig åtkomst.  Är SANT om inte hello-begäran är en `AcquireTokenSilentAsync` eller `PromptBehavior.Never` då hello appen måste en interaktiv tooperform ```AcquireToken``` toogive hello slutanvändning hello möjlighet toocomply med hello princip för begäran. 

### <a name="scenario-single-page-app-spa-using-adaljs"></a>Scenario: Enkel sida App (SPA) med hjälp av ADAL.js

Vi går igenom hello fallet när vi har en enda sida app (SPA) i det här scenariot, villkorlig åtkomst med hjälp av ADAL.js toocall skyddade webb-API.  Detta är en enkel arkitektur men vissa olika delarna som behöver toobe beaktas när du utvecklar runt villkorlig åtkomst.

ADAL.js, finns det några funktioner som hämta token: `login()`, `acquireToken(...)`, `acquireTokenPopup(…)`, och `acquireTokenRedirect(…)`. 

* `login()`hämtar en token ID via en interaktiv inloggning begäran men erhålla inte åtkomsttoken för alla tjänster (inklusive en villkorlig åtkomst i skyddade webb-API).  
* `acquireToken(…)`använda toosilently att skaffa ett åtkomsttoken, vilket innebär att Användargränssnittet inte visas i några omständigheter.  
* `acquireTokenPopup(…)`och `acquireTokenRedirect(…)` är båda används toointeractively begära en token för en resurs, vilket innebär att de alltid visar UI-inloggning.

När en app måste en åtkomst-token toocall webb-API, försöker den en `acquireToken(…)`.  Om token hello-sessionen har upphört att gälla eller måste toocomply med en princip för villkorlig åtkomst, sedan hello *acquireToken* fungera misslyckas och hello app använder `acquireTokenPopup()` eller `acquireTokenRedirect()`.

![En sida-app med Flödesdiagram för ADAL](media/active-directory-conditional-access-developer/spa-using-adal-scenario.png)

Låt oss gå igenom ett exempel med vårt scenario för villkorlig åtkomst.  hello slutanvändarens bara landat på hello platsen och har inte en session.  Vi utföra en `login()` anropa, hämta ett ID-token utan multifaktorautentisering.  Sedan träffar hello användaren en knapp som kräver hello toorequest AppData från ett webb-API.  hello app försöker toodo en `acquireToken()` anrop men misslyckas eftersom hello användaren inte har utfört multifaktorautentisering ännu och måste toocomply med hello principen för villkorlig åtkomst.

Azure AD skickar tillbaka hello efter HTTP-svar: 

```
HTTP 400; Bad Request 
error=interaction_required
error_description=AADSTS50076: Due tooa configuration change made by your administrator, or because you moved tooa new location, you must use multi-factor authentication tooaccess '<Web API App/Client ID>'.
```

Vår app måste toocatch hello `error=interaction_required`.  hello program kan sedan använda antingen `acquireTokenPopup()` eller `acquireTokenRedirect()` hello på samma resurs.  hello användaren är framtvingad toodo multifaktorautentisering. När hello användaren är klar hello multifaktorautentisering hello app utfärdas en ny åtkomsttoken för hello begärd resurs.

tootry i det här scenariot finns våra [JS SPA On-behalf-of kodexemplet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca).  Det här kodexemplet använder hello principen för villkorlig åtkomst och webb-API som du har registrerat tidigare med ett JS SPA toodemonstrate det här scenariot. Den visar hur tooproperly referensen hello anspråk utmaning och få en åtkomsttoken som kan användas för webb-API. Alternativt utcheckningen hello allmänna [Angular.js kodexemplet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) vägledning om en vinkel SPA


## <a name="see-also"></a>Se även

* toolearn mer om hello funktioner finns [villkorlig åtkomst i Azure AD](../active-directory-conditional-access.md).
* Läs mer Azure AD-kodexempel [Github-Repo över kodexempel](https://github.com/azure-samples?utf8=%E2%9C%93&q=active-directory). 
* Mer information om hello ADAL SDK och åtkomst hello-referensdokumentation finns [biblioteket guiden](active-directory-authentication-libraries.md).
* toolearn mer information om scenarier med flera innehavare finns [hur toosign användare hello flera innehavare mönster](active-directory-devhowto-multi-tenant-overview.md).
