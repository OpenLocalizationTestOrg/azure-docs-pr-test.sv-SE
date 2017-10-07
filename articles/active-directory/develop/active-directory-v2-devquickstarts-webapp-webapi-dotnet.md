---
title: "aaaAzure AD v2.0 .NET webb-app anropa API komma igång | Microsoft Docs"
description: "Hur toobuild en .NET MVC-Webbapp som anropar webbtjänster med hjälp av personliga Microsoft-konton och arbets- eller skolkonton för inloggning."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 56be906e-71de-469d-9a5c-9fc08aae4223
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 1a70791418bc2a7d1fdfbafb9b5126a033a32292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="calling-a-web-api-from-a-net-web-app"></a>Anropa ett webb-API från en .NET-webbapp
Du kan snabbt lägga till autentisering tooyour webbappar och webb-API: er med stöd för både personliga Microsoft-konton och arbets-eller skolkonton med hello v2.0-slutpunkten.  Här ska vi skapa en MVC-webbapp som loggar användarna in med OpenID Connect med hjälp från Microsofts OWIN mellanprogram.  hello webbprogrammet hämta OAuth 2.0-åtkomsttoken för en webb-api som skyddas av OAuth 2.0 som gör att skapa, läsa och ta bort på en viss användares ”uppgiftslistan”.

Den här självstudiekursen kommer fokuserar huvudsakligen på med MSAL tooacquire och använda åtkomst-token i en webbapp som beskrivs i fullständig [här](active-directory-v2-flows.md#web-apps).  Som förutsättningar, vill du kanske toofirst Lär dig hur för[lägga till webbapp för enkel inloggning tooa](active-directory-v2-devquickstarts-dotnet-web.md) eller hur för[att säkra ett webb-API](active-directory-v2-devquickstarts-dotnet-api.md).

> [!NOTE]
> Inte alla Azure Active Directory-scenarier och funktioner som stöds av hello v2.0-slutpunkten.  toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
> 
> 

## <a name="download-sample-code"></a>Hämta exempelkoden
hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).  toofollow längs kan du [hämta hello appens stomme som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) eller klona hello stommen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Du kan också [hämta hello slutförts appen som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) eller klona hello slutförts app:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Registrera en app
Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa dessa [detaljerade steg](active-directory-v2-app-registration.md).  Se till att:

* Kopiera hello **program-Id** tilldelade tooyour app måste den snart.
* Skapa en **App hemlighet** av hello **lösenord** typ och kopiera värdet för senare
* Lägg till hello **Web** plattform för din app.
* Ange rätt hello **omdirigerings-URI**. hello omdirigerings-uri anger tooAzure AD där autentisering svar som ska dirigeras - hello standard för den här kursen är `https://localhost:44326/`.

## <a name="install-owin"></a>Installera OWIN
Lägg till hello OWIN mellanprogram NuGet-paket toohello `TodoList-WebApp` projekt med hello Package Manager-konsolen.  hello OWIN mellanprogram kommer att använda tooissue inloggning och utloggningsförfrågningar, hantera hello användarens session och få information om hello användare, bland annat.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-hello-user-in"></a>Logga hello användare i
Nu konfigurera hello OWIN mellanprogram toouse hello [autentiseringsprotokollet OpenID Connect](active-directory-v2-protocols.md).  

* Öppna hello `web.config` filen i hello rot hello `TodoList-WebApp` projektet och ange din Apps konfigurationsvärden i hello `<appSettings>` avsnitt.
  * Hej `ida:ClientId` är hello **program-Id** tilldelade tooyour app i portalen för registrering av hello.
  * Hej `ida:ClientSecret` är hello **App hemlighet** du skapade i hello-portalen för registrering.
  * Hej `ida:RedirectUri` är hello **omdirigerings-Uri** du angav i hello-portalen.
* Öppna hello `web.config` filen i hello rot hello `TodoList-Service` projektet och Ersätt hello `ida:Audience` hello med samma **program-Id** som ovan.
* Öppna hello filen `App_Start\Startup.Auth.cs` och Lägg till `using` instruktioner för hello bibliotek från ovan.
* I Hej samma fil, implementera hello `ConfigureAuth(...)` metod.  Hej parametrar som du anger i `OpenIDConnectAuthenticationOptions` fungerar som koordinater för din app toocommunicate med Azure AD.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {

                    // hello `Authority` represents hello v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
                    // hello `Scope` describes hello permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                    // In a real application you could use issuer validation for additional checks, like making sure hello user's organization has signed up for your app, for instance.

                    ClientId = clientId,
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // hello `AuthorizationCodeReceived` notification is used toocapture and redeem hello authorization_code that hello v2.0 endpoint returns tooyour app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-tooget-access-tokens"></a>Använd MSAL tooget åtkomst-token
I hello `AuthorizationCodeReceived` meddelande vi vill toouse [OAuth 2.0 tillsammans med OpenID Connect](active-directory-v2-protocols.md) tooredeem hello authorization_code för en åtkomst-token toohello att göra tjänsten.  MSAL kan göra den här processen enkel du:

* Installera först hello förhandsversionen av MSAL:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```

* Och lägga till en annan `using` instruktionen toohello `App_Start\Startup.Auth.cs` -filen för MSAL.
* Lägg nu till en ny metod hello `OnAuthorizationCodeReceived` händelsehanterare.  Den här hanteraren använder MSAL tooacquire en åtkomst-token toohello API för att göra-lista och lagrar hello token i MSAL'S token-cache för senare:

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using hello web app's clientId as hello scope, since hello web app and service share hello same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

* I web apps har MSAL en extensible token-cache som kan använda toostore token.  Det här exemplet implementerar hello `NaiveSessionCache` som använder HTTP-session lagring toocache token.

<!-- TODO: Token Cache article -->


## <a name="call-hello-web-api"></a>Anropa hello webb-API
Nu är det dags tooactually använda hello access_token som du har införskaffade i steg 3.  Öppna hello webbapp `Controllers\TodoListController.cs` fil, vilket gör att alla hello CRUD begär toohello API för att göra-lista.

* Du kan använda MSAL igen här toofetch access_tokens från hello MSAL cache.  Lägg först till en `using` -instruktion för MSAL toothis fil.
  
    `using Microsoft.Identity.Client;`
* I hello `Index` åtgärdens, Använd MSAL `AcquireTokenSilentAsync` metoden tooget en access_token som kan använda tooread data från hello tjänsten för att göra-lista:

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using hello web app's clientId as hello scope, since hello web app and service share hello same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

* hello exempel läggs sedan till hello resulterande token toohello HTTP GET-begäran som hello `Authorization` sidhuvud, vilka hello uppgiftslista tjänsten använder tooauthenticate hello begäran.
* Om hello uppgiftslista tjänsten returnerar en `401 Unauthorized` svar, hello access_tokens i MSAL har blivit ogiltigt av någon anledning.  I det här fallet bör du ta bort alla access_tokens från hello MSAL cache och visa hello användaren ett meddelande om att de kanske behöver toosign i igen, vilket startar om hello token förvärv flödet.

```C#
// ...
// If hello call failed with access denied, then drop hello current access token from hello cache,
// and show hello user an error indicating they might need toosign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need toosign in again.");
}
// ...
```

* På liknande sätt, om MSAL är tooreturn en access_token av någon anledning, bör du instruera hello användaren toosign i igen.  Detta är lika enkelt som fångar in alla `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show hello user an error indicating they might need toosign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading tooDo List: " + ee.Message + " You might need toolog out and log back in.");
}
// ...
```

* hello exakt samma `AcquireTokenSilentAsync` anropet är implementd i hello `Create` och `Delete` åtgärder.  Du kan använda den här MSAL metoden tooget access_tokens när du behöver dem i din app i web apps.  MSAL hand tar om införskaffa cachelagring och uppdatera token för dig.

Slutligen skapar och kör appen!  Logga in med ett Account eller en Azure AD-kontot och Observera hur hello användaridentitet avspeglas i hello övre navigeringsfältet.  Lägg till och ta bort några objekt från hello användarens att göra-lista toosee hello OAuth 2.0 skyddade API-anrop i åtgärden.  Nu har du en webbprogrammet & webb-API, både skyddas med standardprotokollen, som kan autentisera användare med både sina personliga och arbete/skola konton.

För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) [här](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Nästa steg
För ytterligare resurser, kolla:

* [Utvecklarhandbok för hello v2.0 >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow ”azure-active-directory” taggen >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Hämta säkerhetsuppdateringar för våra produkter
Vi rekommenderar att du tooget meddelanden om när säkerhetsincidenter genom att besöka [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera tooSecurity Advisory-aviseringar.

