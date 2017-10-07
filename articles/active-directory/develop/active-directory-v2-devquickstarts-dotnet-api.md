---
title: "aaaAdd inloggning tooa .NET MVC webb-API med hjälp av hello Azure AD v2.0-slutpunkten | Microsoft Docs"
description: "Hur toobuild en .NET MVC webb-Api som accepterar token från både personliga Account och arbets-eller skolkonton."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e77bc4e0-d0c9-4075-a3f6-769e2c810206
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 4e517145422bb6e9368e82a7eef4a5c57cce530a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-an-mvc-web-api"></a>Skydda en MVC-webb-API
Med Azure Active Directory hello v2.0-slutpunkten kan du skydda ett webb-API med hjälp av [OAuth 2.0](active-directory-v2-protocols.md) åtkomsttoken, aktivera användare med både personliga Microsoft-konto och arbets- eller skolkonto konton toosecurely åtkomst till webb-API.

> [!NOTE]
> Inte alla Azure Active Directory-scenarier och funktioner som stöds av hello v2.0-slutpunkten.  toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
>
>

I ASP.NET webb-API: er, kan du göra detta med hjälp av Microsofts OWIN mellanprogram som ingår i .NET Framework 4.5.  Vi använder här OWIN toobuild en ”tooDo listan” MVC webb-API som gör att klienter toocreate och läsa uppgifter från en användares att göra-lista.  hello webb-API verifierar att inkommande begäranden innehåller en giltig åtkomst-token och avvisa alla begäranden som inte klarar valideringen på en skyddad väg.  Det här exemplet har skapats med Visual Studio 2015.

## <a name="download"></a>Ladda ned
hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).  toofollow längs kan du [hämta hello appens stomme som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) eller klona hello stommen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

hello stommen app innehåller alla hello formaterad exempelkod för ett enkelt API, men saknar de över hello identity-relaterade objekt. Om du inte vill toofollow längs, kan du i stället klona eller [hämta hello slutförts exempel](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Registrera en app
Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa dessa [detaljerade steg](active-directory-v2-app-registration.md).  Se till att:

* Kopiera hello **program-Id** tilldelade tooyour app måste den snart.

Visual studio-lösning innehåller också en ”TodoListClient”, vilket är en enkel WPF-program.  Hej TodoListClient är används toodemonstrate hur en användare loggar in och hur en klient kan skicka begäranden tooyour Web API.  I det här fallet både hello TodoListClient och hello TodoListService representeras av hello samma app.  tooconfigure Hej TodoListClient, bör du även:

* Lägg till hello **Mobile** plattform för din app.

## <a name="install-owin"></a>Installera OWIN
Nu när du har registrerat en app, behöver tooset upp din app toocommunicate med hello v2.0-slutpunkten i ordning toovalidate inkommande begäranden och token.

* toobegin, öppna hello lösningen och Lägg till hello OWIN mellanprogram NuGet-paket toohello TodoListService projektet med hello Package Manager-konsolen.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
PM> Install-Package Microsoft.IdentityModel.Protocol.Extensions -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>Konfigurera OAuth-autentisering
* Lägg till en OWIN klassen toohello TodoListService Startprojekt kallas `Startup.cs`.  Högerklicka på projektet hello--> **Lägg till** --> **nytt objekt** --> Sök efter ”OWIN”.  Hej OWIN mellanprogram ska anropa hello `Configuration(…)` metod när appen startar.
* Ändra hello klassdeklarationen för`public partial class Startup` -vi har implementerat en del av den här klassen som du redan i en annan fil.  I hello `Configuration(…)` metod, gör ett anrop tooConfgureAuth(...) tooset in verifiering för ditt webbprogram.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

* Öppna hello filen `App_Start\Startup.Auth.cs` och implementera hello `ConfigureAuth(…)` metod som ställer in hello Web API tooaccept token från hello v2.0-slutpunkten.

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, hello TodoListClient and TodoListService
                // are represented using hello same Application Id - we use
                // hello Application Id toorepresent hello audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that hello user's organization (if applicable) has
                // signed up for hello app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up hello OWIN pipeline toouse OAuth 2.0 Bearer authentication.
        // hello options provided here tell hello middleware about hello type of tokens
        // that will be recieved, which are JWTs for hello v2.0 endpoint.

        // NOTE: hello usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by hello v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used toofetch & use hello OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

* Nu kan du använda `[Authorize]` attribut tooprotect dina domänkontrollanter och åtgärder med OAuth 2.0-ägar-autentisering.  Skapa snygga hello `Controllers\TodoListController.cs` klass med en auktorisera-tagg.  Detta tvingar hello användaren toosign i innan sidan.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

* När en auktoriserad anroparen har anropar en hello `TodoListController` API: er, hello åtgärden kanske behöver komma åt tooinformation om hello anroparen.  OWIN tillhandahåller åtkomst toohello anspråk i hello ägar-token via hello `ClaimsPrincpal` objekt.  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use hello ClaimsPrincipal tooaccess information about the
    // user making hello call.  In this case, we use hello 'sub' or
    // NameIdentifier claim tooserve as a key for hello tasks in hello data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

* Slutligen öppna hello `web.config` filen i hello roten för hello TodoListService projekt och ange dina konfigurationsvärden i hello `<appSettings>` avsnitt.
  * Din `ida:Audience` är hello **program-Id** hello-appen som du angav i hello-portalen.

## <a name="configure-hello-client-app"></a>Konfigurera hello-klientappen
Innan du kan se hello Todo tjänsten i åtgärden måste tooconfigure hello Todo listan klienten så att den kan hämta token från hello v2.0-slutpunkten och att anrop toohello tjänst.

* Öppna i hello TodoListClient project `App.config` och ange dina konfigurationsvärden i hello `<appSettings>` avsnitt.
  * Din `ida:ClientId` program-Id som du kopierade från hello-portalen.

Slutligen, rensa, skapa och köra varje projekt!  Nu har du en .NET MVC webb-API som accepterar token från både personliga Microsoft-konton och arbets-eller skolkonton.  Logga in på hello TodoListClient och anropa web api tooadd uppgifter toohello användarens att göra-lista.

För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) [har angetts som en .zip här](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), eller kan du klona den från GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Nästa steg
Du kan nu gå vidare till ytterligare information.  Du kanske vill tootry:

[Anropa ett webb-API från ett webbprogram >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

För ytterligare resurser, kolla:

* [Utvecklarhandbok för hello v2.0 >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow ”azure-active-directory” taggen >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Hämta säkerhetsuppdateringar för våra produkter
Vi rekommenderar att du tooget meddelanden om när säkerhetsincidenter genom att besöka [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera tooSecurity Advisory-aviseringar.
