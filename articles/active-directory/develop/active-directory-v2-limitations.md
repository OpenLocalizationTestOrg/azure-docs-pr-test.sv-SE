---
title: "aaaAzure Active Directory v2.0-slutpunkten begränsningar och restriktioner | Microsoft Docs"
description: "En lista över begränsningar och restriktioner för hello Azure AD v2.0-slutpunkten."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: a99289c0-e6ce-410c-94f6-c279387b4f66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: bcbb7239f1d117002d16ac23dca8f1feb13a9161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="should-i-use-hello-v20-endpoint"></a>Bör jag använda hello v2.0-slutpunkten?
När du skapar program som integreras med Azure Active Directory måste toodecide om hello v2.0-slutpunkten och verifieringsprotokollen uppfyller dina behov. Azure Active Directorys ursprungliga slutpunkt stöds fortfarande helt och på vissa sätt, är fler funktioner än version 2.0. Dock hello v2.0-slutpunkten [introducerar betydande fördelar](active-directory-v2-compare.md) för utvecklare.

Här är förenklad rekommendationer för utvecklare vid denna tidpunkt:

* Om du måste stödja personliga Microsoft-konton i ditt program, Använd hello v2.0-slutpunkten. Men innan du gör det, måste du kontrollera att du förstår hello begränsningar som diskuterar vi i den här artikeln.
* Om ditt program behöver bara toosupport Microsoft work och skolkonton, Använd inte hello v2.0-slutpunkten. I stället referera tooour [Utvecklarhandbok för Azure AD](active-directory-developers-guide.md).

Med tiden kan växa hello v2.0-slutpunkten tooeliminate hello restriktioner som anges här, så att du behöver bara toouse hello v2.0-slutpunkten. I hello tiden är är den här artikeln avsedd toohelp du avgöra om hello v2.0-slutpunkten är rätt för dig. Vi kommer att fortsätta tooupdate den här artikeln tooreflect hello aktuell status för hello v2.0-slutpunkten. Tooreevaluate tillbaka dina krav mot v2.0-funktioner.

Om du har en befintlig Azure AD-app som inte använder hello v2.0-slutpunkten finns inget behov av toostart från början. I framtida hello ger vi ett sätt för du toouse dina befintliga Azure AD-program med hello v2.0-slutpunkten.

## <a name="restrictions-on-app-types"></a>Begränsningar för appar
För närvarande stöds hello följande typer av appar som inte av hello v2.0-slutpunkten. En beskrivning av apptyper som stöds finns i [apptyper för hello Azure Active Directory v2.0-slutpunkten](active-directory-v2-flows.md).

### <a name="standalone-web-apis"></a>Fristående webb-API: er
Du kan använda hello v2.0-slutpunkten för[bygga en webb-API som skyddas med OAuth 2.0](active-directory-v2-flows.md#web-apis). Dock att webb-API kan endast ta emot token från ett program som har hello samma program-ID. Du kan inte komma åt en webb-API från en klient som har ett annat program-ID. hello klienten kommer inte att kunna toorequest eller skaffa behörigheter tooyour Web API.

toosee hur toobuild webb-API som accepterar token från en klient som har hello samma program-ID, se hello v2.0-slutpunkten Web API-exempel i vår [komma igång](active-directory-appmodel-v2-overview.md#getting-started) avsnitt.

## <a name="restrictions-on-app-registrations"></a>Begränsningar för app-registreringar
För närvarande för varje app som du vill toointegrate med hello v2.0-slutpunkten måste du skapa en appregistrering i hello nya [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Befintliga Azure AD eller Microsoft-konto-appar som inte är kompatibla med hello v2.0-slutpunkten. Appar som är registrerade i någon portal än hello Programregistreringsportalen är inte kompatibla med hello v2.0-slutpunkten. I framtida hello planerar vi tooprovide sätt-toouse ett befintligt program som en v2.0-app. För närvarande, men finns det ingen migreringssökväg för en befintlig app toowork med hello v2.0-slutpunkten.

Dessutom app registreringar som du skapar i hello [Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) har hello följande varningar:

* Bara två apphemligheter tillåts per program-ID.
* En appregistrering som registreras av en användare med ett personligt microsoftkonto kan visas och endast hanteras av en enda utvecklarkonto. Den kan inte delas mellan flera utvecklare.  Om du vill att tooshare app registreringen bland flera utvecklare, du kan skapa hello program loggar in på hello-portalen för registrering med Azure AD-kontot.
* Det finns flera begränsningar på hello format för hello omdirigerings-URI som är tillåtet. Mer information om omdirigerings-URI: er finns i hello nästa avsnitt.

## <a name="restrictions-on-redirect-uris"></a>Begränsningar för omdirigerings-URI: er
Appar som är registrerade i hello Programregistreringsportalen är för närvarande begränsad tooa begränsad uppsättning omdirigerings-URI-värden. Hej omdirigerings-URI för webbprogram och tjänster måste börja med hello schema `https`, och alla omdirigerings-URI-värden måste dela en enda DNS-domän. Exempelvis kan du registrera ett webbprogram som har en av dessa omdirigerings-URI: er:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

hello registreringssystem jämför hello hela DNS-namnet på hello befintliga omdirigerings-URI toohello DNS-namnet på hello omdirigerings-URI som du lägger till. hello begäran tooadd hello DNS-namnet misslyckas om någon av hello följande villkor föreligger:  

* hello hela DNS-namnet på hello nya omdirigerings-URI matchar inte hello DNS-namnet på hello befintliga omdirigerings-URI.
* hello hela DNS-namnet på hello nya omdirigerings-URI är inte en underdomän till hello befintliga omdirigerings-URI.

Om hello har appen den här omdirigerings-URI:

`https://login.contoso.com`

Du kan lägga till tooit så här:

`https://login.contoso.com/new`

I det här fallet matchar hello DNS-namnet exakt. Du kan också göra detta:

`https://new.login.contoso.com`

I så fall måste refererar du tooa DNS-underdomänen av login.contoso.com. Om du vill toohave en app som har inloggningen east.contoso.com och inloggnings-west.contoso.com som omdirigerings-URI: er, måste du lägga till de omdirigerings-URI: er i den här ordningen:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Du kan lägga till hello senare två eftersom de är underdomäner i hello först omdirigerings-URI, contoso.com. Den här begränsningen tas bort i en kommande version.

hur tooregister en app i hello Programregistreringsportalen, se toolearn [hur tooregister en app med hello v2.0-slutpunkten](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services-and-apis"></a>Begränsningar för tjänster och API: er
Stöder för närvarande, hello v2.0-slutpunkten inloggningen för en app som är registrerad i hello Programregistreringsportalen och som faller i hello lista över [stöds autentisering flöden](active-directory-v2-flows.md). De här apparna kan dock hämta OAuth 2.0-åtkomsttoken för en mycket begränsad uppsättning resurser. hello v2.0-slutpunkten problem åtkomst till token för:

* hello-app som begärt hello-token. En app kan hämta en åtkomst-token för sig själv, om hello logisk app består av flera olika komponenter eller nivåer. toosee det här scenariot i åtgärden, Kolla in våra [komma igång](active-directory-appmodel-v2-overview.md#getting-started) självstudier.
* Hej Outlook e-post, kalender och kontakter REST API: er, som finns på https://outlook.office.com. toolearn hur toowrite en app som har åtkomst till dessa API: er Se hello [Office komma igång](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) självstudier.
* Microsoft Graph API: er. Du kan lära dig mer om [Microsoft Graph](https://graph.microsoft.io) och hello data som är tillgängliga tooyou.

Inga andra tjänster stöds just nu. Flera Microsoft Online Services kommer läggas till i hello framtida dessutom toosupport för egna specialbyggt webb-API: er och tjänster.

## <a name="restrictions-on-libraries-and-sdks"></a>Begränsningar för bibliotek och SDK
Stöd för hello v2.0-slutpunkten är för närvarande begränsad. Om du vill toouse hello v2.0-slutpunkten i ett produktionsprogram, finns följande alternativ:

* Om du skapar ett webbprogram, kan du använda Microsoft allmänt tillgänglig serversidan mellanprogram tooperform inloggning och token validering på ett säkert sätt. Dessa inkluderar hello OWIN öppna ID Connect mellanprogram för ASP.NET och hello Node.js Passport plugin-programmet. Kodexempel som använder Microsoft mellanprogram finns i vår [komma igång](active-directory-appmodel-v2-overview.md#getting-started) avsnitt.
* Om du skapar en desktop eller mobile-programmet, kan du använda en av vår förhandsgranskning bibliotek för Microsoft-autentisering (MSAL).  Dessa bibliotek finns i en förhandsgranskning för stöd för produktion, så att den är säker toouse dem i program i produktion. Du kan läsa mer om hello användningsvillkor hello Förhandsgranska och hello tillgängliga bibliotek i vår [autentisering bibliotek referens](active-directory-v2-libraries.md).
* För plattformar som inte omfattas av Microsoft bibliotek kan du integrera med hello v2.0-slutpunkten genom att skicka och ta emot protokollmeddelanden i din programkod direkt. Hej v2.0 OpenID Connect och OAuth-protokollen [dokumenteras explicit](active-directory-v2-protocols.md) toohelp som du utför en sådan integration.
* Slutligen kan du använda öppen källkod öppna ID Connect och OAuth bibliotek toointegrate med hello v2.0-slutpunkten. hello v2.0-protokollet måste vara kompatibel med många öppen källkod protokollet bibliotek utan större ändringar. hello varierar dessa typer av bibliotek efter språk och plattformar. Hej [öppna ID Connect](http://openid.net/connect/) och [OAuth 2.0](http://oauth.net/2/) webbplatser underhålla en lista över populära implementeringar. Mer information finns i [Azure Active Directory v2.0 och autentisering bibliotek](active-directory-v2-libraries.md), och hello lista med öppen källkod klientbibliotek och exempel som har testats med hello v2.0-slutpunkten.

## <a name="restrictions-on-protocols"></a>Begränsningar för protokoll
hello v2.0-slutpunkten har inte stöd för SAML eller WS-Federation; den stöder endast öppna ID Connect och OAuth 2.0.  Inte alla funktioner och möjligheter för OAuth-protokoll har införts i hello v2.0-slutpunkten. Dessa protokollfunktioner som finns för närvarande *inte tillgänglig* i hello v2.0-slutpunkten:

* ID-token som utfärdas av hello v2.0-slutpunkten saknar ett `email` anspråk för hello användare, även om du skaffar tillstånd från hello användaren tooview sin e-post.
* Hej OpenID Connect användarinformationen slutpunkten har inte implementerats på hello v2.0-slutpunkten. Men alla data för användarprofiler som potentiellt skulle visas i den här slutpunkten är tillgänglig från hello Microsoft Graph `/me` slutpunkt.
* hello v2.0-slutpunkten stöder inte utfärdande roll eller grupp anspråk i ID-token.
* Hej [OAuth 2.0 resurs ägare lösenord autentiseringsuppgifter Grant](https://tools.ietf.org/html/rfc6749#section-4.3) stöds inte av hello v2.0-slutpunkten.

Kodspråk stöder hello v2.0-slutpunkten inte någon form av hello SAML eller WS-Federation-protokoll.

toobetter förstå hello omfattning protocol-funktioner som stöds i hello v2.0-slutpunkten, Läs igenom våra [referens för OpenID Connect och OAuth 2.0-protokollet](active-directory-v2-protocols.md).

## <a name="restrictions-for-work-and-school-accounts"></a>Begränsningar för arbets-och skolkonton
Om du har använt Active Directory Authentication Library (ADAL) i Windows-program, kan du har vidtagit nytta av Windows-integrerad autentisering, som använder hello Security Assertion Markup Language (SAML) assertion bevilja. Med denna tilldelning federerade användare av Azure AD klienter kan autentisera tyst med sina lokala Active Directory-instans utan att ange autentiseringsuppgifter. Hello SAML assertion bevilja stöds för närvarande inte på hello v2.0-slutpunkten.
