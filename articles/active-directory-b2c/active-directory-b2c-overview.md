---
title: "Översikt – Azure AD B2C | Microsoft Docs"
description: Utveckla konsumentinriktade program med Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: c465dbde-f800-4f2e-8814-0ff5f5dae610
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: saeedakhter-msft
ms.openlocfilehash: abfd742e710458de3193dc5051de7818a112376c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-focus-on-your-app-let-us-worry-about-sign-up-and-sign-in"></a>Azure AD B2C: Fokusera på din app och låt oss ta hand om registrering och inloggning

Azure AD-B2C är en molntjänst för att hantera identiteter för webbappar och mobilappar. Det är en global tjänst med hög tillgänglighet som kan skalas toohundreds miljoner identiteter. Azure AD B2C bygger på en säker plattform i företagsklass och ser till att dina program, ditt företag och dina användare är skyddade.

Azure AD B2C kan dina program tooauthenticate med minimal konfiguration:

* **Sociala konton** (till exempel Facebook, Google, LinkedIn med mera)
* **Företagskonton** (med öppna standardprotokoll, OpenID Connect eller SAML)
* **Lokala konton** (e-postadress och lösenord eller användarnamn och lösenord)

## <a name="get-started"></a>Kom igång

Skaffa först en egen klient genom att använda hello steg som beskrivs i [skapa en Azure AD B2C-klient](active-directory-b2c-get-started.md).

Välj ditt apputvecklingsscenario:

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobilappar och skrivbordsappar](../active-directory/develop/media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobilappar och skrivbordsappar</center> | [Översikt](active-directory-b2c-reference-oauth-code.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br /><br />[iOS](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal)<br /><br />[Android](https://github.com/Azure-Samples/active-directory-b2c-android-native-msal) | [.NET](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop)<br /><br />[Xamarin](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) |  |
| <center>![Web Apps](../active-directory/develop/media/active-directory-developers-guide/Web_app.png)<br />Web Apps</center> | [Översikt](active-directory-b2c-reference-oidc.md)<br /><br />[ASP.NET](active-directory-b2c-devquickstarts-web-dotnet-susi.md)<br /><br />[ASP.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapp) | [Node.js](active-directory-b2c-devquickstarts-web-node.md) |  |
| <center>![Appar med en sida](../active-directory/develop/media/active-directory-developers-guide/SPA.png)<br />Appar med en sida</center> | [Översikt](active-directory-b2c-reference-spa.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)<br /><br /> |  |  |
| <center>![Webb-API:er](../active-directory/develop/media/active-directory-developers-guide/Web_API.png)<br />Webb-API:er</center> | [ASP.NET](active-directory-b2c-devquickstarts-api-dotnet.md)<br /><br /> [ASP.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)<br /><br /> [Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) | [Anropa ett .NET-webb-API](active-directory-b2c-devquickstarts-web-api-dotnet.md) |

## <a name="whats-new"></a>Nyheter

Kom tillbaka ofta toolearn om framtida ändringar toohello Azure Active Directory B2C. Vi kommer även att twittra om eventuella uppdateringar med @AzureAD.

* Dessutom för (allmän tillgänglighet) ”inbyggda principer” Hej [”anpassade principer”](active-directory-b2c-overview-custom.md) funktion är nu tillgänglig som förhandsversion.  Anpassade principer är för identity-tekniker som behöver kontroll över hello sammansättning av deras identitet.
* Hej [åtkomst-Token](https://azure.microsoft.com/en-us/blog/azure-ad-b2c-access-tokens-now-in-public-preview) funktion är nu tillgänglig som förhandsversion.
* [Allmän tillgänglighet för Europabaserade Azure AD B2C](https://azure.microsoft.com/en-us/blog/azuread-b2c-ga-eu/)-kataloger har tillkännagetts.
* Kolla in vårt ständigt växande bibliotek med [kodexempel på Github](https://github.com/Azure-Samples?q=b2c)!

## <a name="how-tooarticles"></a>Hur tooarticles

Lär dig hur toouse specifika funktioner i Azure Active Directory B2C:

* Konfigurera [Facebook-](active-directory-b2c-setup-fb-app.md), [Google+-](active-directory-b2c-setup-goog-app.md), [Microsoft-](active-directory-b2c-setup-msa-app.md), [Amazon-](active-directory-b2c-setup-amzn-app.md) och [LinkedIn-](active-directory-b2c-setup-li-app.md)konton för användning i dina konsumentinriktade program.
* [Använd anpassade attribut toocollect information om dina användare](active-directory-b2c-reference-custom-attr.md).
* [Aktivera Azure Multi-Factor Authentication i dina konsumentinriktade program](active-directory-b2c-reference-mfa.md).
* [Konfigurera lösenordsåterställning via självbetjäning för dina användare](active-directory-b2c-reference-sspr.md).
* [Anpassa hello utseende och känslan av registrering, logga in och andra konsumentinriktade sidor](active-directory-b2c-reference-ui-customization.md) som hanteras av Azure Active Directory B2C.
* [Använd hello Azure Active Directory Graph API tooprogrammatically skapa, läsa, uppdatera och ta bort användare](active-directory-b2c-devquickstarts-graph-dotnet.md) i Azure Active Directory B2C-klient.

## <a name="next-steps"></a>Nästa steg

Dessa länkar är användbara för att utforska hello tjänsten i mer detalj:

* Se hello [Azure Active Directory B2C-prisinformation](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
* Granska våra [kodexempel](https://azure.microsoft.com/en-us/resources/samples/?service=active-directory&term=b2c) för Azure Active Directory B2C. 
* Få hjälp på Stack Overflow genom att använda hello [azure-ad-b2c](http://stackoverflow.com/questions/tagged/azure-ad-b2c) tagg.
* Ge oss dina synpunkter med hjälp av [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c), vi vill toohear dem!
* Granska hello [Protokollreferens för Azure AD B2C](active-directory-b2c-reference-protocols.md).
* Granska hello [Azure AD B2C tokenreferens](active-directory-b2c-reference-tokens.md).
* Läs hello [Azure Active Directory B2C vanliga frågor och svar](active-directory-b2c-faqs.md).
* [Skapa supportförfrågningar för Azure Active Directory B2C](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Hämta säkerhetsuppdateringar för våra produkter

Vi rekommenderar att du tooget meddelanden om när säkerhetsincidenter genom att besöka [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera tooSecurity Advisory-aviseringar.

