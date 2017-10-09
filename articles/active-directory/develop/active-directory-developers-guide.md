---
title: "aaaAzure Active Directory för utvecklare | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över inloggning med Microsofts arbets- och skolkonton med hjälp av Azure Active Directory."
services: active-directory
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 4dbbea6c1e0b8a70c0c36ddd1caec5658130a003
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-for-developers"></a>Azure Active Directory för utvecklare
Azure Active Directory är en tjänst i molnet identitet som gör att utvecklare toosecurely inloggning alla användare med ett arbets- eller skolkonto konto backas upp av Microsoft.  här hello dokumentationen visar hur tooadd Azure AD stöder tooyour program med hjälp av industry standard autentiseringsprotokollen OAuth & OpenID Connect.

| | |
| --- | --- |
|[Grundläggande om Auth](active-directory-authentication-scenarios.md) | En introduktion tooauthentication med Azure AD |
|[Typer av program](active-directory-authentication-scenarios.md#application-types-and-scenarios) | En översikt över hello autentiseringsscenarier som stöds av Azure AD |                                
                                                                              
## <a name="get-started"></a>Kom igång
Dessa interaktiv inställningar vägleder dig genom att använda våra bibliotek toosign för autentisering i Azure Active Directory-användare.

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobilappar och skrivbordsappar](./media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobilappar och skrivbordsappar</center> | [Översikt](active-directory-authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](active-directory-devquickstarts-ios.md)<br /><br />[Android](active-directory-devquickstarts-android.md) | [.NET](active-directory-devquickstarts-dotnet.md)<br /><br />[Windows](active-directory-devquickstarts-windowsstore.md)<br /><br />[Xamarin](active-directory-devquickstarts-xamarin.md) | [Cordova](active-directory-devquickstarts-cordova.md)<br /><br />[OAuth 2.0](active-directory-protocols-oauth-code.md) |
| <center>![Web Apps](./media/active-directory-developers-guide/Web_app.png)<br />Web Apps</center> | [Översikt](active-directory-authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](active-directory-devquickstarts-webapp-dotnet.md)<br /><br />[Java](active-directory-devquickstarts-webapp-java.md) | [NodeJS](active-directory-devquickstarts-openidconnect-nodejs.md)<br /><br />[OpenID Connect 1.0](active-directory-protocols-openid-connect-code.md) |  |
| <center>![Appar med en sida](./media/active-directory-developers-guide/SPA.png)<br />Appar med en sida</center> | [Översikt](active-directory-authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |  |
| <center>![Webb-API:er](./media/active-directory-developers-guide/Web_API.png)<br />Webb-API:er</center> | [Översikt](active-directory-authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[NodeJS](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Tjänst-till-tjänst](./media/active-directory-developers-guide/Service_App.png)<br />Tjänst-till-tjänst</center> | [Översikt](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#server-or-daemon-application-to-web-api)<br /><br />[Autentiseringsuppgifter för OAuth 2.0-klient](active-directory-protocols-oauth-service-to-service.md) |  |

## <a name="guides"></a>Guider
Dessa artiklar informera hur tooperform vanliga uppgifter med Azure Active Directory.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[Appregistrering](active-directory-integrating-applications.md)           | Hur tooregister en app i Azure AD |
|[Appar för flera klienter](active-directory-devhowto-multi-tenant-overview.md)    | Hur toosign i ett Microsoft-arbetskonto |
|[OAuth och OpenID Connect](active-directory-protocols-openid-connect-code.md)| Hur toosign i användare och anropa webb-API: er med hjälp av vår modern autentisering-protokoll |
|[Fler guider ...](active-directory-developers-guide-index.md#guides)        |     |

## <a name="reference"></a>Referens
De här artiklarna innehåller detaljerad information om API:er, protokollmeddelanden och de termer som används i Azure Active Directory.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Autentiseringsbibliotek (ADAL)](active-directory-authentication-libraries.md)   | En översikt över hello bibliotek & SDK: er som tillhandahålls av Azure AD |
| [Kodexempel](active-directory-code-samples.md)                                  | En lista över alla Azure AD-kodexempel |
| [Ordlista](active-directory-dev-glossary.md)                                      | Termer och definitioner av ord som används i den här dokumentationen |
| [Mer referensmaterial ...](active-directory-developers-guide-index.md#reference)|     |

## <a name="help--support"></a>Hjälp och support
Dessa är hello bästa platser tooget hjälp med att utveckla på Azure Active Directory.

|  |  
|---|
|[Stackspill`azure-active-directory` och `adal`taggar](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)      |
|[Feedback om Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)|
| [Prova Microsoft Dev Chat (gratis under en begränsad tid)](http://aka.ms/devchat) |

<br />

> [!NOTE]
> Om du behöver toosign i Microsoft personliga konton kan du behöva tooconsider med hello [Azure AD v2.0-slutpunkten](active-directory-appmodel-v2-overview.md).  hello Azure AD v2.0-slutpunkten är hello enandet av Microsoft personliga konton & Microsoft work konton (från Azure AD) i en enda autentiseringssystemet.
