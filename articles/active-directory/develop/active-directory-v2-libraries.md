---
title: "aaaAzure autentiseringsbibliotek för Active Directory v2.0 | Microsoft Docs"
description: "Kompatibel klientbibliotek och server mellanprogram bibliotek och relaterade biblioteket, källa och exempel länkar för hello Azure Active Directory v2.0-slutpunkten."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 19cec615-e51f-4141-9f8c-aaf38ff9f746
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: d7affdaac3a087b951d54d96fa68edde2a065172
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory v2.0-autentiseringsbibliotek
hello Azure Active Directory (AD Azure) v2.0-slutpunkten stöder hello branschstandard OAuth 2.0 och OpenID Connect 1.0-protokollet. Du kan använda olika bibliotek från Microsoft och andra företag med hello v2.0-slutpunkten.

När du skapar ett program som använder hello v2.0-slutpunkten rekommenderar vi att du använder bibliotek som har skrivits av protokollet domän experter som följer en Security Development Lifecycle (SDL)-metod som [hello en följt av Microsoft] [Microsoft-SDL]. Om du väljer toohand kod stöd för hello protokoll, rekommenderar vi du följer SDL-metoder och noggrann toohello säkerhetsaspekter i hello standarder specifikationerna för varje protokoll.

## <a name="types-of-libraries"></a>Typer av bibliotek
Azure AD v2.0 fungerar med två typer av bibliotek:

* **Klientbibliotek**. Interna klienter och servrar använder du klienten bibliotek tooget åtkomst-token för att anropa en resurs, till exempel Microsoft Graph.
* **Servern mellanprogram bibliotek**. Webbappar som använder server mellanprogram bibliotek för användarinloggning. Webb-API: er Använd server mellanprogram bibliotek toovalidate token som skickas av interna klienter eller av andra servrar.

## <a name="library-support"></a>Stöd
Eftersom du kan välja något standardkompatibel bibliotek när du använder hello v2.0-slutpunkten är det viktigt tooknow där toogo för support. Kontakta hello biblioteket ägare problem och funktioner som efterfrågas i biblioteket kod. Kontakta Microsoft-problem och funktioner som efterfrågas i hello protokollimplementering för tjänsten på klientsidan.

Bibliotek finns i två kategorier för support:

* **Stöds av Microsoft**. Microsoft tillhandahåller korrigeringar för dessa bibliotek och har utfört SDL vederbörlig möda åt på dessa bibliotek.
* **Kompatibel**. Microsoft har testat dessa bibliotek i grundläggande scenarier och bekräftat att den fungerar med hello v2.0-slutpunkten. Microsoft inte tillhandahåller åtgärdar för dessa bibliotek och inte har gjort en genomgång av dessa bibliotek. Problem och funktionsförfrågningar ska vara riktad toohello biblioteket öppen källkod projektet.

En lista över bibliotek som fungerar med hello v2.0-slutpunkten finns hello nästa avsnitt i den här artikeln.


## <a name="microsoft-supported-client-libraries"></a>Stöds av Microsoft klientbibliotek

> [!IMPORTANT]
> Hej MSAL preview bibliotek är lämpliga för användning i en produktionsmiljö. Vi tillhandahåller hello samma nivå produktions-stöd för dessa bibliotek som vi gör våra aktuella produktions-bibliotek (ADAL). Hello förhandsversionen kan vi göra ändringar toohello MSAL API, interna format och andra mekanismer för dessa bibliotek utan föregående meddelande, som du kommer att krävs tootake tillsammans med felkorrigeringar eller funktionsförbättringar. Detta kan påverka ditt program. Till exempel en ändra toohello cache-format som kan påverka dina användare, till exempel att de toosign i igen. En API-ändring kan kräva att du tooupdate din kod. När vi tillhandahåller hello allmän tillgänglighet versionen vi kräver att du tooupdate toohello allmän tillgänglighet version i sex månader som program som skrivits med en förhandsgranskning fungerar versionen av biblioteket inte längre.

| Plattform | Bibliotek | Ladda ned | Källkoden | Exempel | Referens
| --- | --- | --- | --- | --- | --- |
| .NET-klient Windows Store UWP Xamarin-iOS och Android | MSAL .NET (förhandsgranskning) |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Skrivbordsapp](guidedsetups/active-directory-mobileanddesktopapp-windowsdesktop-intro.md) |  |
| JavaScript | MSAL.js (förhandsgranskning) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [Den enda sidan App](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |  |
| iOS macOS | MSAL (förhandsgranskning) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [iOS-App](https://github.com/Azure-Samples/active-directory-msal-ios-swift) |  |
| Android | MSAL (förhandsgranskning) | [hello centrallager](https://repo1.maven.org/maven2/com/microsoft/identity/client/msal/) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Android-App](guidedsetups/active-directory-mobileanddesktopapp-android-intro.md) | [JavaDocs](http://javadoc.io/doc/com.microsoft.identity.client/msal) |

## <a name="microsoft-supported-server-middleware-libraries"></a>Stöds av Microsoft server mellanprogram bibliotek

| Plattform | Bibliotek | Ladda ned | Källkoden | Exempel | Referens
| --- | --- | --- | --- | --- | --- |
| .NET 4.x | OWIN OpenID Connect mellanprogram |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[CodePlex](http://katanaproject.codeplex.com) |[MVC-App](guidedsetups/active-directory-serversidewebapp-aspnetwebappowin-intro.md) | |
| .NET 4.x | OWIN OAuth ägar mellanprogram för AzureAD |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[CodePlex](http://katanaproject.codeplex.com) |  | |
| .NET 4.x | JWT-hanterare för .NET 4.5 | [NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt/4.0.4.403061554) | [GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET Core | ASP.NET OpenID Connect mellanprogram |[Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] |[ASP.NET-säkerhet (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] |[MVC-app](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2) |
| .NET Core | ASP.NET OAuth ägar mellanprogram |[Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] |[ASP.NET-säkerhet (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] |  |
| .NET Core | JWT-hanterare för .NET Core  |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |Azure AD-Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Webbprogram](active-directory-v2-devquickstarts-node-web.md)| |

## <a name="compatible-client-libraries"></a>Kompatibel klientbibliotek
| Plattform | Biblioteksnamn | Testad version | Källkod | Exempel |
|:---:|:---:|:---:|:---:|:---:|
| Android |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) |0.2.1 |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) |[Exempel på inbyggda appen](active-directory-v2-devquickstarts-android.md) |
| iOS |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |1.2.8 |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |[Exempel på inbyggda appen](active-directory-v2-devquickstarts-ios.md) |
| JavaScript |[Hello.js](https://adodson.com/hello.js/) |1.13.5 |[Hello.js](https://github.com/MrSwitch/hello.js) |[SPA](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |

## <a name="compatible-server-middleware-libraries"></a>Kompatibla servern mellanprogram bibliotek
| Plattform | Biblioteksnamn | Testad version | Källkod | Exempel |
|:---:|:---:|:---:|:---:|:---:|
| Java | [Scribe Java scribejava](https://github.com/scribejava/scribejava) | [Version 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | [ScribeJava](https://github.com/scribejava/scribejava/archive/scribejava-3.2.0.zip) | |
| PHP | [hello PHP Leagues oauth2-klient](https://github.com/thephpleague/oauth2-client) | [Version 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | [oauth2-klient](https://github.com/thephpleague/oauth2-client/archive/1.4.2.zip) | |
| Python Flask |[Flask OAuthlib](https://github.com/lepture/flask-oauthlib) |0.9.3 |[Flask OAuthlib](https://github.com/lepture/flask-oauthlib) |[Webbapp](https://github.com/Azure-Samples/active-directory-python-flask-graphapi-web-v2) |
| Ruby |[OmniAuth](https://github.com/omniauth/omniauth/wiki) |omniauth:1.3.1</br>omniauth-oauth2:1.4.0 |[OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) |  |

## <a name="related-content"></a>Relaterat innehåll
Mer information om hello Azure AD v2.0-slutpunkten finns hello [översikt över Azure AD app model v2.0][AAD-App-Model-V2-Overview].

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: ../active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:/
[ClientLib-Iosmac-Lib]:/
[ClientLib-Iosmac-Repo]:/
[ClientLib-Iosmac-Sample]:/
[ClientLib-Android-Lib]:/
[ClientLib-Android-Repo]:/
[ClientLib-Android-Sample]:/
[ClientLib-Js-Lib]:/
[ClientLib-Js-Repo]:/
[ClientLib-Js-Sample]:/

[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-node-web/
