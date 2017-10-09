---
title: "aaaUnderstanding hello OAuth2 implicit bevilja flödet i Azure AD | Microsoft Docs"
description: "Lär dig mer om Azure Active Directory-implementeringen av hello OAuth2 implicit bevilja flödet, och om det är bäst för ditt program."
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 90e42ff9-43b0-4b4f-a222-51df847b2a8d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/15/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 3402e6619709e1a5b1e790ffd79dc62139552d9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-hello-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Förstå hello OAuth2 implicit bevilja flödet i Azure Active Directory (AD)
Hej OAuth2 implicit bevilja är kända för att bevilja hello hello längsta lista med säkerhetsfrågor i hello OAuth2-specifikationen. Och ännu, som är hello-metod som implementeras av ADAL JS och hello en rekommenderas när du skriver SPA-program. Vad ger? Det är en fråga kompromisser: och som det visar sig hello implicit bevilja hello bästa sättet att försöka för program som använder ett Web API via JavaScript från en webbläsare.

## <a name="what-is-hello-oauth2-implicit-grant"></a>Vad är hello OAuth2 implicit bevilja?
Hej quintessential [OAuth2 auktorisering kod bevilja](https://tools.ietf.org/html/rfc6749#section-1.3.1) är hello authorization grant som använder två separata slutpunkter. Hej autentiseringsslutpunkt används hello användaren interaktion steget, vilket resulterar i en Auktoriseringskoden. Hej tokenslutpunkten används sedan av hello-klienten för att utbyta hello-koden för en åtkomst-token och ofta en uppdateringstoken. Webbprogram är obligatoriska toopresent sina egna program autentiseringsuppgifter toohello tokenslutpunkten, så att hello auktorisering kan servern autentisera hello-klienten.

Hej [OAuth2 implicit bevilja](https://tools.ietf.org/html/rfc6749#section-1.3.2) är en variant av andra stöd för auktorisering. Det gör att en klient tooobtain en åtkomst-token (och id_token när du använder [OpenId Connect](http://openid.net/specs/openid-connect-core-1_0.html)) direkt från hello autentiseringsslutpunkt utan att kontakta hello tokenslutpunkten eller autentisera hello klienten. Den här variant är särskilt utformat för JavaScript-baserade program som körs i en webbläsare: i hello ursprungliga OAuth2-specifikationen token returneras i ett URI-fragment. Som gör hello token bits tillgängliga toohello JavaScript-kod i hello-klienten, men det garanterar att de inte inkluderas i omdirigeringar mot hello-server. Returnerar token via webbläsaren omdirigerar direkt från hello autentiseringsslutpunkt. Det har också hello nytta av att ta bort eventuella krav för mellan ursprungsanrop, vilket är nödvändigt om hello JavaScript-programmet är obligatoriska toocontact hello-token för slutpunkt.

En viktig egenskap för hello OAuth2 implicit bevilja är hello fakta som sådana flöden returnerar aldrig uppdatera token toohello klienten. Som vi kommer att se i nästa avsnitt om hello, som är inte verkligen behövs och faktum är ett säkerhetsproblem.

## <a name="suitable-scenarios-for-hello-oauth2-implicit-grant"></a>Lämpliga scenarier för hello OAuth2 implicit bevilja
Eftersom hello OAuth2-specifikationen själva deklarerar har hello implicit bevilja planeras tooenable användaragent program – som är toosay JavaScript-program som körs inom en webbläsare. hello definierar egenskap för sådana program är att JavaScript-kod används för åtkomst till serverresurser (vanligtvis en webb-API) och för att uppdatera programmet hello UX därefter. Se program, t.ex. Gmail eller Outlook Web Access: när du väljer ett meddelande i Inkorgen endast hello-meddelande visualiseringen panelen ändrar toodisplay hello nya markeringen, medan hello resten av hello sidan förblir oförändrat. Detta är till skillnad från traditionella omdirigerings-baserade webbappar, där varje användarinteraktion resulterar i ett återanslående helsida och en helsida återgivningen av hello nya svar från servern.

Program som tar hello JavaScript-baserade metoden tooits extrema kallas den enda sidan program eller SPA: hello idé är att programmen bara hantera ett inledande HTML-sidan och associerad JavaScript med alla efterföljande interaktioner som styrs av Webb-API-anrop utförs via JavaScript. Dock hybrid metoder, där programmet hello är främst återanslående-driven men utför tillfällig JS-anrop, är inte ovanliga – hello diskussion om implicita flödet för användning som är relevant för dem också.

Omdirigerings-baserade program skydda vanligtvis begäranden via cookies, men att metoden inte fungerar även för JavaScript-program. Cookies fungerar endast mot hello-domän som har genererats för, medan JavaScript-anrop kan riktas mot andra domäner. I själva verket som blir ofta hello fallet: Se program anropar Microsoft Graph API, Office-API, Azure API – alla som finns utanför hello domän från där programmet hello hanteras. En ökande trend för JavaScript-program är toohave inga backend på alla förlitande 100% på 3 part webb-API: er tooimplement deras funktioner.

Är för närvarande hello bästa metoden för att skydda anrop tooa Web API toouse hello OAuth2 ägar-token metod, där varje anrop åtföljs av en OAuth2-åtkomsttoken. hello Web API undersöker hello inkommande åtkomst-token och, om det hittas i den hello nödvändiga scope, beviljar åtkomst toohello den begärda åtgärden. hello implicita flödet är det lätt för JavaScript-program tooobtain åtkomst-token för en webb-API ger många fördelar i respektera toocookies:

* Token kan erhållas på ett tillförlitligt sätt utan mellan ursprungsanrop – obligatorisk registrering av hello omdirigerings-URI toowhich token är returnera garanterar att token inte förskjuts
* JavaScript-program kan hämta så många åtkomsttoken som de behöver för så många Web API: er de är riktade mot – utan begränsning för domäner
* HTML5-funktioner som session eller lokal lagring ger fullständig kontroll över tokencachelagring och livstidshantering cookies management är täckande toohello app
* Åtkomsttoken inte mottagliga tooCross-plats begäran attacker med förfalskning (CSRF)

hello implicit bevilja flödet utfärdar inte uppdaterings-tokens främst av säkerhetsskäl. En uppdateringstoken omfång inte som snävare som åtkomsttoken, bevilja mycket mer kraft inflicting därför mycket större skada om den är känd. Token levereras i hello URL i hello implicita flödet, därför hello risk för avlyssning är högre än i hello auktorisering kod bevilja.

Observera dock att JavaScript-programmet har en annan funktion förfogande för att förnya åtkomsttoken utan att fråga hello användarens autentiseringsuppgifter för flera gånger. hello program kan använda en dold iframe tooperform nya token-förfrågningar mot hello autentiseringsslutpunkt Azure AD: så länge hello webbläsaren fortfarande har en aktiv session (läsa: har en sessionscookie) hello autentiseringsbegäran mot hello Azure AD-domän kan inträffa har utan interaktion från användaren.

Den här modellen ger hello JavaScript programmet hello möjlighet tooindependently förnya åtkomst-token och även få nya för ett nytt API (förutsatt att hello användaren tidigare godkänt för dessa. Detta förhindrar hello tillagt belastningen för hämtning, hantera och skydda ett högt värde artefakt, till exempel en uppdateringstoken. hello artefakt som medger hello tyst förnyelse, hello Azure AD sessions-cookie hanteras utanför hello-programmet. En annan fördel med den här metoden är en användare kan logga ut från Azure AD, med hjälp av hello program som är inloggad på Azure AD, som körs i något av hello flikar i webbläsaren. Detta resulterar i hello borttagning av hello Azure AD sessions-cookie och hello JavaScript-programmet förlorar automatiskt hello möjlighet toorenew token för hello loggat ut användaren.

## <a name="is-hello-implicit-grant-suitable-for-my-app"></a>Passar hello implicit bevilja min app?
hello implicit bevilja presenterar flera risker än andra stöd och hello områden du behöver toopay uppmärksamhet tooare väl dokumenterat. Till exempel [missbruk av åtkomst-Token tooImpersonate resurs-ägare i Implicit flöda] [ OAuth2-Spec-Implicit-Misuse] och [Hotmodell för OAuth 2.0- och säkerhetsaspekter] [ OAuth2-Threat-Model-And-Security-Implications]). Hello högre riskprofil är dock i stort sett på grund av toohello faktum att det är tänkt tooenable program som kör active kod som hanteras av en fjärresurs tooa webbläsare. Om du planerar en SPA-arkitektur har inga backend-komponenter eller avser tooinvoke webb-API via JavaScript, bör du använda hello implicita flödet för token.

Om ditt program är en intern klient, inte en bra anpassning hello implicita flödet. hello frånvaron av hello Azure AD sessions-cookie hello kontexten för en intern klient deprives programmet hello innebär att underhålla en lång livslängd session. Vilket innebär att ditt program uppmanar flera gånger hello användaren när åtkomsttoken för nya resurser.

Om du utvecklar ett program som innehåller en serverdel och använda ett API från dess serverdelskoden är hello implicita flödet inte heller passar bra. Andra stöd ger mycket mer. Hej OAuth2 klienten autentiseringsuppgifter bevilja innehåller till exempel hello möjlighet tooobtain token som återspeglar hello behörigheter tilldelas toohello själva programmet, som skillnad från toouser delegeringar. Det innebär att hello-klienten har hello möjlighet toomaintain Programmeringsåtkomst tooresources även om en användare inte aktivt används i en session och så vidare. Inte bara den men sådana ger högre säkerhetsgarantier. Till exempel åtkomsttoken aldrig transit via hello användarens webbläsare, de inte riskerar att sparas i hello webbhistorik och så vidare. hello klientprogrammet kan också utföra stark autentisering när du begär en token.

## <a name="next-steps"></a>Nästa steg
* En fullständig lista över resurser för utvecklare, inklusive referensinformation för hello protokoll och OAuth2 auktorisering bevilja flöden stöd av Azure AD finns toohello [Azure AD-Guide för utvecklare][AAD-Developers-Guide]
* Se [hur toointegrate ett program med Azure AD] [ ACOM-How-To-Integrate] för ytterligare djupet på hello integration programprocessen.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819
