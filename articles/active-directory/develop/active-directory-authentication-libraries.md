---
title: aaaAzure Active Directory-Autentiseringsbibliotek | Microsoft Docs
description: "hello Azure AD Authentication Library (ADAL) kan klienten programvaruutvecklare tooeasily autentisera användare toocloud eller lokala Active Directory (AD) och sedan hämta åtkomsttoken för att skydda API-anrop."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: mbaldwin
ms.assetid: 2e4fc79a-0285-40be-8c77-65edee408a22
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 20fae18807ef03463ab1bc218e5f3548b5bd5717
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory-Autentiseringsbibliotek
hello Azure Active Directory Authentication Library (ADAL) aktiverar klienten programmet utvecklare tooeasily autentisera användare toocloud eller lokala Active Directory (AD) och få åtkomst-token för att skydda API-anrop. ADAL underlättar autentisering för utvecklare med hjälp av funktioner som:
 - stöd för asynkrona metodanrop
 - en konfigurerbar token-cache att komma åt token och uppdatera token
 - automatisk token uppdatering när en åtkomst-token upphör att gälla och en uppdateringstoken är tillgänglig
 - och mycket mer
 
Genom att hantera de flesta av hello komplexitet ADAL hjälper utvecklare att fokusera på affärslogiken och skydda enkelt resurser, utan att vara expert på säkerhet.

ADAL är tillgängligt på en mängd olika plattformar.

### <a name="client-libraries"></a>Klientbibliotek

| Plattform | Bibliotek | Ladda ned | Källkoden | Exempel | Referens
| --- | --- | --- | --- | --- | --- |
| .NET-klient Windows Store UWP Xamarin-iOS och Android |MSAL för .NET (förhandsgranskning) |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client/1.1.0-preview) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Skrivbordsapp](~/articles/active-directory/develop/guidedsetups/active-directory-windesktop.md) |[Referens](https://docs.microsoft.com/dotnet/api/?view=identityclient-1.1.0-preview) | 
| JavaScript |MSAL för JavaScript (förhandsgranskning) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [Den enda sidan App](~/articles/active-directory/develop/GuidedSetups/active-directory-javascriptspa.md) | [Referens](https://htmlpreview.github.io/?https://raw.githubusercontent.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/docs/classes/_useragentapplication_.msal.useragentapplication.html) | 
| iOS |MSAL för iOS (förhandsgranskning) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [iOS-app](~/articles/active-directory/develop/GuidedSetups/active-directory-ios.md) | [Referens](https://azuread.github.io/docs/objc/) |
| Android |MSAL för Android (förhandsgranskning) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Android-app](~/articles/active-directory/develop/GuidedSetups/active-directory-android.md) | [Referens](http://javadoc.io/doc/com.microsoft.identity.client/msal/0.1.1) |
| .NET-klient Windows Store UWP Xamarin-iOS och Android |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Skrivbordsapp](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-dotnet) |[Referens](https://docs.microsoft.com/dotnet/api/?view=identitymodelclientsad-3.13.9) | 
| .NET-klient Windows Store, Windows Phone 8.1 |ADAL .NET v2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.4) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.4) | [Skrivbordsapp](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) | | 
| JavaScript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[Den enda sidan App](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS macOS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[iOS-app](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-ios) | [Referens](https://cocoapods.org/pods/ADAL)|
| Android |ADAL |[hello centrallager](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Android-app](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-android) | [JavaDocs](http://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | | |
| Java |ADAL4J |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Java-webbapp](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-java) | |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) | | |
### <a name="server-libraries"></a>Server-bibliotek 

| Plattform | Bibliotek | Ladda ned | Källkoden | Exempel | Referens
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN för AzureAD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[CodePlex](http://katanaproject.codeplex.com) |[MVC-App](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |OWIN för OpenIDConnect |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[CodePlex](http://katanaproject.codeplex.com) |[Webbapp](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| Node.js |Azure AD-Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Webb-API](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |
| .NET |OWIN för WS-Federation |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[CodePlex](http://katanaproject.codeplex.com) |[MVC-Webbapp](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |Tillägg för Identity-protokollet för .NET 4.5 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |JWT-hanterare för .NET 4.5 |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |



## <a name="scenarios"></a>Scenarier

Här följer tre vanliga scenarier där ADAL kan användas för att autentisera en klient som ansluter till en fjärresurs:  

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Autentisering av användare av en native client-program som körs på en enhet 

I detta scenario har en utvecklare ett WPF-klientprogram som behöver tooaccess en fjärresurs som skyddas av Azure AD, till exempel ett webb-API. Han har en Azure-prenumeration, vet hur tooinvoke hello underordnat webb-API och vet hello Azure AD-klient som hello använder webb-API. Därför kan han använda ADAL toofacilitate autentisering med Azure AD, genom att helt delegera hello autentisering upplevelse tooADAL eller genom att uttryckligen hantera autentiseringsuppgifter. ADAL gör det enkelt tooauthenticate hello användaren, får en åtkomst-token och uppdateringstoken från Azure AD och använder hello åtkomst-token toomake begäranden toohello webb-API.

Kodexempel som visar det här scenariot med autentisering tooAzure AD, se [internt klientprogram WPF tooWeb API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>Autentisering av ett konfidentiellt klientprogram som körs på en webbserver

I det här scenariot kan har en utvecklare ett program som körs på en server som måste tooaccess en fjärresurs som skyddas av Azure AD, till exempel ett webb-API. Han har en Azure-prenumeration, vet hur tooinvoke hello underordnade tjänsten och vet använder hello Azure AD-klient hello webb-API. Därför kan han använda ADAL toofacilitate autentisering med Azure AD genom att uttryckligen hantera hello programs autentiseringsuppgifter. ADAL gör det enkelt tooretrieve en token från Azure AD med hjälp av hello programmets klientreferensen och sedan använda denna token toomake begäranden toohello webb-API. ADAL också handtag hantera hello livstid hello åtkomst-token av cachelagringen och förnya efter behov. Kodexempel som visar det här scenariot, se [Daemon konsolen programmet tooWeb API](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Autentisering av ett konfidentiellt klientprogram som körs på en server för en användares räkning 

I det här scenariot kan har en utvecklare ett program som körs på en server som måste tooaccess en fjärresurs som skyddas av Azure AD, till exempel ett webb-API. hello begäran måste också toobe gjort åt en Azure AD-användare. Han har en Azure-prenumeration, vet hur tooinvoke hello underordnat webb-API och vet hello Azure AD-klient hello-tjänsten använder. När hello användare är autentiserade toohello webbprogram, kan hello programmet hämta ett auktoriseringskod för hello användare från Azure AD. hello webbprogrammet kan sedan använda ADAL tooobtain en åtkomst-token och uppdateringstoken för en användare som använder hello kod och klient-autentiseringsuppgifter som är associerade med programmet hello från Azure AD. När hello webbprogram som har tillgång till hello åtkomst-token kan anropa det hello web API förrän hello-token upphör att gälla. När hello-token upphör att gälla kan hello webbprogram använda ADAL tooget en ny åtkomsttoken med hjälp av hello uppdateringstoken som tidigare har tagits emot. Kodexempel som visar det här scenariot, se [Native client tooWeb API tooWeb API](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof).

## <a name="see-also"></a>Se även

- [hello Azure Active Directory-guide för utvecklare](active-directory-developers-guide.md)
- [Autentiseringsscenarier för Azure Active directory](active-directory-authentication-scenarios.md)
- [Azure Active Directory-kodexempel](active-directory-code-samples.md)
