---
title: aaaAzure Active Directory-kodexempel | Microsoft Docs
description: "Ett index över kodexempel för Azure Active Directory, ordnade efter scenario."
services: active-directory
documentationcenter: dev-center-name
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: mbaldwin
ms.custom: aaddev
ms.openlocfilehash: 921ce08766cc6e29a6db2c4f38d216e6de11730f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-code-samples"></a>Azure Active Directory-kodexempel
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Du kan använda Microsoft Azure Active Directory (AD Azure) tooadd autentisering och auktorisering tooyour webbprogram och webb-API: er. Det här avsnittet länkar du toosamples som visar hur du gör och kodstycken som du kan använda i dina program. På hello teckentabellen till exempel, hittar du detaljerad Läs-mig avsnitt som hjälper till med krav, installation och konfiguration. Hello koden är kommenterade toohelp du förstår hello kritiska sektioner.

toounderstand hello enkelt scenario för varje typ av exemplet finns Autentiseringsscenarier för Azure AD.

Bidra tooour exemplen på GitHub: [Microsoft Azure Active Directory-exempel och dokumentation](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-tooweb-application"></a>Web webbläsare tooWeb program
De här exemplen visar hur toowrite ett webbprogram som leder hello användarens webbläsare toosign dem i tooAzure AD.

| Språk-plattformen | Exempel | Beskrivning |
| --- | --- | --- |
| C# / .NET |[WebApp-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) |Använd OpenID Connect (ASP.Net OpenID Connect OWIN mellanprogram) tooauthenticate användare från en Azure AD-klient. |
| C# / .NET |[WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) |Ett flera innehavare .NET MVC webbprogram som använder OpenID Connect (ASP.Net OpenID Connect OWIN mellanprogram) tooauthenticate användare från flera Azure AD-klienter. |
| C# / .NET |[WebApp-WSFederation-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) |Använda WS-Federation (ASP.Net WS-Federation OWIN mellanprogram) tooauthenticate användare från en Azure AD-klient. |

## <a name="single-page-application-spa"></a>Sida program (SPA)
Det här exemplet visas hur toowrite en enstaka sida program säkras med Azure AD.  

| Språk-plattformen | Exempel | Beskrivning |
| --- | --- | --- |
| JavaScript, C# / .NET |[SinglePageApp DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) |Använd ADAL för JavaScript och Azure AD toosecure en AngularJS-baserad sida app har implementerats med en ASP.NET web API-serverdel. |

## <a name="native-application-tooweb-api"></a>Interna program tooWeb API
Följande kodexempel visar hur toobuild interna klientprogram som kan anropar webb-API: er som skyddas av Azure AD. De använder [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) och [OAuth 2.0 i Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Språk-plattformen | Exempel | Beskrivning |
| --- | --- | --- |
| Javascript |[NativeClient-MultiTarget-Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) |Använda ADAL hello-plugin för Apache Cordova toobuild en Apache Cordova-app som anropar ett webb-API och använder Azure AD för autentisering. |
| C# / .NET |[NativeClient DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) |Ett .NET WPF-program som anropar ett webb-API som skyddas med hjälp av Azure AD. |
| C# / .NET |[NativeClient WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) |Windows Store-programmet som anropar ett webb-API som skyddas med Azure AD. |
| C# / .NET |[NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) |Windows Store-programmet anropa ett webb-API som skyddas med Azure AD för flera innehavare. |
| C# / .NET |[WebAPI-OnBehalfOf-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) |En native client-program som anropar ett webb-API, som hämtar en token tooact hello ursprungliga användarens räkning och använder sedan hello token toocall en annan webb-API. |
| C# / .NET |[NativeClient WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) |Windows Store-programmet för Windows Phone 8.1 som anropar ett webb-API som skyddas av Azure AD. |
| ObjC |[NativeClient-iOS](https://github.com/Azure-Samples/active-directory-ios) |Ett iOS-program som anropar ett webb-API som kräver Azure AD för autentisering. |
| C# / .NET |[WebAPI-ManuallyValidateJwt-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) |En native client-program som innehåller logik tooprocess JWT-token i ett webb-API, istället för att använda OWIN mellanprogram. |
| C# / Xamarin |[NativeClient Xamarin Android](https://github.com/Azure-Samples/active-directory-xamarin-android) |En Xamarin bindning toohello interna Azure AD Authentication Library (ADAL) för hello Android-biblioteket. |
| C# / Xamarin |[NativeClient-Xamarin-iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) |En Xamarin bindning toohello interna Azure AD Authentication Library (ADAL) för iOS. |
| C# / Xamarin |[NativeClient-MultiTarget-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) |En Xamarin-projektet som riktar sig till fem plattformar och anropar ett webb-API som skyddas av Azure AD. |
| C# / .NET |[NativeClient fjärradministrerade DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) |En intern program som utför icke-interaktiv autentisering och anropar ett webb-API som skyddas av Azure AD. |

## <a name="web-application-tooweb-api"></a>Web Application tooWeb API
Följande kodexempel visar hur använda [OAuth 2.0 i Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) toobuild webbprogram som anropa webb-API: er som skyddas av Azure AD.

| Språk-plattformen | Exempel | Beskrivning |
| --- | --- | --- |
| C# / .NET |[WebApp-WebAPI-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) |Anropa ett webb-API med hello inloggade användarens behörighet. |
| C# / .NET |[WebApp-WebAPI-OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) |Anropa ett webb-API med hello programbehörigheter. |
| C# / .NET |[WebApp-WebAPI-OAuth2-UserIdentity-Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) |Lägg till auktorisering med [OAuth 2.0 i Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooan befintligt webbprogram så kan det anropa ett webb-API. |
| JavaScript |[WebAPI Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) |Konfigurera en REST API-tjänst som är integrerad med Azure AD för API-skydd. Innehåller en Node.js-server med en webb-API. |
| C# / .NET |[WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |En flera innehavare MVC-webbprogram som använder OpenID Connect (ASP.Net OpenID Connect OWIN mellanprogram) tooauthenticate användare från en Azure AD-klient. Använder en auktorisering kod tooinvoke hello Graph API. |

## <a name="server-or-daemon-application-tooweb-api"></a>Server eller Daemon program tooWeb API
Följande kodexempel visar hur toobuild en daemon eller serverprogram som hämtar resurser från ett webb-API med hjälp av [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) och [OAuth 2.0 i Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Språk-plattformen | Exempel | Beskrivning |
| --- | --- | --- |
| C# / .NET |[Daemon för DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) |Ett konsolprogram anropar ett webb-API. Hej klientreferensen är ett lösenord. |
| C# / .NET |[DotNet-CertificateCredential-Daemon](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) |Ett konsolprogram som anropar ett webb-API. Hej klientreferensen är ett certifikat. |

## <a name="calling-azure-ad-graph-api"></a>Anropar Azure AD Graph API
Följande kodexempel visar hur toobuild program som anropar hello Azure AD Graph API tooread och skriva katalogdata.

| Språk-plattformen | Exempel | Beskrivning |
| --- | --- | --- |
| Java |[WebApp-GraphAPI-Java](https://github.com/Azure-Samples/active-directory-java-graphapi-web) |Ett webbprogram som använder hello Graph API tooaccess Azure AD directory-data. |
| PHP |[WebApp-GraphAPI-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) |Ett webbprogram som använder hello Graph API tooaccess Azure AD directory-data. |
| C# / .NET |[WebApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) |Ett webbprogram som använder hello Graph API tooaccess Azure AD directory-data. |
| C# / .NET |[ConsoleApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) |Den här konsolen appen visar vanliga läsning och skrivning anrop toohello Graph API och visar hur tooexecute användaren licens tilldelning och uppdatera en användares miniatyrbild och länkar. |
| C# / .NET |[ConsoleApp-GraphAPI-DiffQuery-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) |Ett konsolprogram som använder hello differentiell frågan i hello Graph API tooget periodiska ändrar toouser objekt i en Azure AD-klient. |
| C# / .NET |[WebApp-GraphAPI-DirectoryExtensions-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) |Ett MVC-program som använder Graph API frågor toogenerate en enkel företagets Organisationsschema. |
| PHP |[WebApp-GraphAPI-DirectoryExtensions-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) |Ett PHP-program som anropar hello Graph API tooregister ett tillägg och sedan läsa, uppdatera och ta bort värden i hello-attributet för anknytning. |

## <a name="authorization"></a>Auktorisering
Dessa code exempel visar hur toouse Azure AD för auktorisering.

| Språk-plattformen | Exempel | Beskrivning |
| --- | --- | --- |
| C# / .NET |[WebApp-GroupClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) |Utföra rollbaserad åtkomstkontroll (RBAC) med hjälp av Azure Active Directory gruppanspråk i ett program som är integrerade med Azure AD. |
| C# / .NET |[WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) |Utföra rollbaserad åtkomstkontroll (RBAC) med hjälp av Azure Active Directory application roller i ett program som är integrerade med Azure AD. |

## <a name="legacy-walkthroughs"></a>Äldre genomgång
Dessa genomgång använda något äldre teknik, men kan fortfarande vara av intresse.

| Språk-plattformen | Exempel | Beskrivning |
| --- | --- | --- |
| C# / .NET |[Roll- och ACL-baserade auktorisering i en Microsoft Azure AD-program](http://go.microsoft.com/fwlink/?LinkId=331694) |Utför rollbaserad auktorisering (RBAC) och ACL-baserad auktorisering i ett program som är integrerade med Azure AD. |
| C# / .NET |[AAL - Windows Store-app service tooREST - autentisering](http://go.microsoft.com/fwlink/?LinkId=330605) |Använd [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) (tidigare AAL) för Windows Store Beta tooadd användaren autentisering funktioner tooa Windows Store-app. |
| C# / .NET |[ADAL - interna tooREST apptjänst - autentisering med AAD via dialogrutan](http://go.microsoft.com/fwlink/?LinkId=259814) |Använd [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) tooadd användaren funktioner tooa WPF-klientautentisering. |
| C# / .NET |[ADAL - interna tooREST apptjänst - autentisering med ACS via dialogrutan](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |Använd [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) och [Access Control Service 2.0 (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) tooadd användaren funktioner tooa WPF-klientautentisering. |
| C# / .NET |[ADAL - Server tooServer autentisering](http://go.microsoft.com/fwlink/?LinkId=259816) |Använd [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) toosecure-anrop från en server side processen tooan MVC4 Web API REST-tjänst. |
| C# / .NET |[Lägga till inloggning tooYour Web program med hjälp av Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) |Konfigurera en .NET-program tooperform web enkel inloggning mot enterprise Azure AD-katalogen. |
| C# / .NET |[Utveckla webbprogram med flera innehavare med Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) |Använda Azure AD tooadd toohello enkel inloggning och directory åtkomstfunktioner för en .NET-program toowork över flera organisationer. |
| JAVA |[Java-Sample-appen för Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkId=263969) |Använd hello Graph API tooaccess katalogdata från Azure AD. |
| PHP |[PHP Exempelapp för Azure AD Graph API](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) |Använd hello Graph API tooaccess katalogdata från Azure AD. |
| C# / .NET |[Exempelapp för Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkID=262648) |Använd hello Graph API tooaccess katalogdata från Azure AD. |
| C# / .NET |[Exempelapp för Azure AD Graph differentiell frågan](http://go.microsoft.com/fwlink/?LinkId=275398) |Använd hello differentiell fråga i hello Graph API tooget periodiska ändringar toouser objekt i en Azure AD-klient. |
| C# / .NET |[Sample-appen för att integrera med flera innehavare molnet program för Azure AD](http://go.microsoft.com/fwlink/?LinkId=275397) |Integrera ett program med flera innehavare i Azure AD. |
| C# / .NET |[Skydda en Windows Store-programmet och REST-webbtjänst med hjälp av Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) |Skapa en enkel webb-API-resurs och ett klientprogram för Windows Store med hjälp av Azure AD och hello [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md). |
| C# / .NET |[Med hjälp av hello Graph API tooQuery Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) |Konfigurera en Microsoft .NET-program toouse hello Azure AD Graph API tooaccess data från en klient Azure AD-katalog. |

## <a name="see-also"></a>Se även
##### <a name="other-resources"></a>Andra resurser
[Azure Active Directory-Guide för utvecklare](active-directory-developers-guide.md)

[Azure AD Graph API konceptuell och referenser](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD Graph API hjälpbibliotek](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
