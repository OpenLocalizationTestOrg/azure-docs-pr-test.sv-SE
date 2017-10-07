---
title: aaaAzure Active Directory B2C | Microsoft Docs
description: "Hur profile toobuild ett webbprogram som har sign-upp/inloggning, redigera och lösenordsåterställning med hjälp av Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: barbaraselden
ms.assetid: 30261336-d7a5-4a6d-8c1a-7943ad76ed25
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: 187f99a8dd50d212de4f0517f552cdbbe5a8edf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-web-app-with-azure-active-directory-b2c-sign-up-sign-in-profile-edit-and-password-reset"></a>Skapa en ASP.NET-webbapp med Azure Active Directory B2C profil för registrering, inloggning, redigera och återställning av lösenord

I den här självstudiekursen lär du dig att:

> [!div class="checklist"]
> * Lägg till Azure AD B2C identitet funktioner tooyour webbapp
> * Registrera ditt webbprogram i din Azure AD B2C-katalog
> * Skapa en användare sign-upp/inloggning, Redigera profil och principen för lösenordsåterställning för ditt webbprogram

## <a name="prerequisites"></a>Krav

- Du måste ansluta din B2C-klient tooan Azure-konto. Du kan skapa ett kostnadsfritt Azure-konto [här](https://azure.microsoft.com/en-us/).
- Du behöver [Microsoft Visual Studio](https://www.visualstudio.com/) eller liknande program tooview och ändra hello exempelkod.

## <a name="create-an-azure-ad-b2c-directory"></a>Skapa en Azure AD B2C-katalog

Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient. En katalog är en behållare för alla användare, appar, grupper och mer. Om du inte har någon redan skapar du en B2C-katalog innan du fortsätter i den här guiden.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

> [!NOTE]
> 
> Du måste tooconnect hello B2C-klient tooyour Azure-prenumeration. När du har valt **skapa**väljer hello **länka ett befintligt Azure AD B2C-klient toomy Azure-prenumeration** alternativet, och klicka sedan på hello **Azure AD B2C-klient** listrutan, Välj hello-klient som du vill tooassociate.

## <a name="create-and-register-an-application"></a>Skapa och registrera ett program

Sedan måste toocreate och registrera hello app i B2C-katalogen. Detta ger information att Azure AD B2C måste toosecurely kommunicera med din app. 

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

När du är klar har du både en API och det ursprungliga programmet i inställningarna för programmet.

## <a name="create-policies-on-your-b2c-tenant"></a>Skapa principer på din B2C-klient

I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md). Det här kodexemplet innehåller tre identitetsupplevelser: **registrering och inloggning i**, **profilen redigera**, och **lösenordsåterställning**.  Du behöver toocreate en princip av varje typ, enligt beskrivningen i hello [referensartikeln om principer](active-directory-b2c-reference-policies.md). Vara säker på att tooselect hello visa name-attribut eller anspråk och toocopy ned hello namnet på din princip för senare användning för varje princip.

### <a name="add-your-identity-providers"></a>Lägg till din identitetsleverantörer

Välj inställningarna för **identitetsleverantörer** och välj signup användarnamn eller e-registreringen.

### <a name="create-a-sign-up-and-sign-in-policy"></a>Skapa en princip för registrering och inloggning

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

### <a name="create-a-profile-editing-policy"></a>Skapa en profil Redigera princip

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

### <a name="create-a-password-reset-policy"></a>Skapa en princip för lösenordsåterställning

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

När du har skapat dina principer du är klar toobuild din app.

## <a name="download-hello-sample-code"></a>Hämta hello exempelkod

hello-koden för den här självstudiekursen underhålls på [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi). Du kan klona hello exemplet genom att köra:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

När du har hämtat hello exempelkod igång öppna hello Visual Studio SLN-filen tooget. hello lösningsfilen innehåller två projekt: `TaskWebApp` och `TaskService`. `TaskWebApp`är hello MVC-webbapp som hello användaren interagerar med. `TaskService`är hello appens backend-webb-API som lagrar varje användares att göra-lista. Den här artikeln handlar enbart hello `TaskWebApp` program. toolearn hur toobuild `TaskService` med Azure AD B2C finns i [våra självstudier för .NET web api](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="update-code-toouse-your-tenant-and-policies"></a>Uppdatera koden toouse klient- och principer

Exemplet är konfigurerade toouse hello principer och klient-ID för vår demo-klient. tooconnect den egna tooyour-klient behöver du tooopen `web.config` i hello `TaskWebApp` projektet och Ersätt hello följande värden:

* `ida:Tenant` med namnet på din klientorganisation
* `ida:ClientId` med program-ID:t för din webbapp
* `ida:ClientSecret` med den hemliga nyckeln för din webbapp
* `ida:SignUpSignInPolicyId` med namnet på din registrerings- eller inloggningsprincip
* `ida:EditProfilePolicyId` med namnet på din profilredigeringsprincip
* `ida:ResetPasswordPolicyId` med namnet på din lösenordsåterställningsprincip

## <a name="launch-hello-app"></a>Starta hello-appen
Starta hello appen från Visual Studio. Navigera toohello uppgiftslista fliken och Observera hello URL: en är: https://login.microsoftonline.com/*YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*& client_id = *YourclientID*...

Registrera dig för hello appen genom att använda din e-postadress eller användaren namn. Logga ut, och sedan logga in igen och profilredigering hello eller återställa hello lösenord. Logga ut och logga in som en annan användare. 

## <a name="add-social-idps"></a>Lägg till sociala IDPs

För närvarande hello app stöder bara användaren registrering och inloggning med hjälp av **lokala konton**; konton som lagras i din B2C-katalog som använder ett användarnamn och lösenord. Med hjälp av Azure AD B2C kan du lägga till stöd för andra **identitetsleverantörer** (IDPs) utan att ändra någon av din kod.

tooadd sociala IDPs tooyour appen, börja med att följa hello detaljerade instruktioner i dessa artiklar. För varje IDP du vill toosupport, du behöver tooregister ett program i systemet och hämta ett klient-ID.

* [Konfigurera Facebook som en IDP](active-directory-b2c-setup-fb-app.md)
* [Konfigurera Google som en IDP](active-directory-b2c-setup-goog-app.md)
* [Ställa in Amazon som en IDP](active-directory-b2c-setup-amzn-app.md)
* [Ställa in LinkedIn som en IDP](active-directory-b2c-setup-li-app.md)

När du har lagt till hello identitet providers tooyour B2C-katalog, redigera av dina tre principer tooinclude hello nya IDPs enligt beskrivningen i hello [referensartikeln om principer](active-directory-b2c-reference-policies.md). När du har sparat dina principer, kör du hello appen igen.  Du bör se hello nya IDPs läggas till som alternativ för inloggning och registreringen i var och en av dina identitetsupplevelser.

Du kan experimentera med dina principer och studera hello effekten på din exempelapp. Lägg till eller ta bort IDPs, manipulera programanspråken eller ändra registreringsattribut. Experimentera tills du kan se hur principer, autentiseringsbegäranden och OWIN knyta ihop.

## <a name="sample-code-walkthrough"></a>Exempel kod genomgång
hello följande avsnitt visar hur hello exempelkod för programmet är konfigurerat. Du kan använda den som en vägledning i framtida app-utveckling.

### <a name="add-authentication-support"></a>Lägga till stöd för autentisering

Du kan nu konfigurera din app toouse Azure AD B2C. Appen kommunicerar med Azure AD B2C genom att skicka OpenID Connect autentiseringsbegäranden. hello begäranden styr hello användarupplevelse tooexecute vill att din app genom att ange hello princip. Du kan använda Microsofts OWIN-biblioteket toosend dessa begäranden, köra principer, hantera användarsessioner och mycket mer.

#### <a name="install-owin"></a>Installera OWIN

toobegin, lägga till hello OWIN mellanprogram NuGet paket toohello projekt med hjälp av hello Visual Studio Package Manager-konsolen.

```Console
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

#### <a name="add-an-owin-startup-class"></a>Lägga till en OWIN-startklass

Lägg till en OWIN Start klassen toohello API kallas `Startup.cs`.  Högerklicka på hello-projektet, Välj **Lägg till** och **nytt objekt**, och sök sedan efter OWIN. Hej OWIN mellanprogram ska anropa hello `Configuration(…)` metod när appen startar.

I vårt exempel vi ändrat hello klassdeklarationen för`public partial class Startup` och implementeras hello andra delar av hello klassen i `App_Start\Startup.Auth.cs`. I hello `Configuration` metod, vi har lagt till ett anrop för`ConfigureAuth`, som har definierats i `Startup.Auth.cs`. Efter hello ändringar `Startup.cs` ser ut som följande hello:

```CSharp
// Startup.cs

public partial class Startup
{
    // hello OWIN middleware will invoke this method when hello app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of hello class
        ConfigureAuth(app);
    }
}
```

#### <a name="configure-hello-authentication-middleware"></a>Konfigurera hello mellanprogram för autentisering

Öppna hello filen `App_Start\Startup.Auth.cs` och implementera hello `ConfigureAuth(...)` metod. Hej parametrar som du anger i `OpenIdConnectAuthenticationOptions` fungerar som koordinater för din app toocommunicate med Azure AD B2C. Om du inte anger vissa parametrar används hello standardvärdet. Till exempel vi inte anger hello `ResponseType` i exemplet hello så hello standardvärdet `code id_token` kommer att användas i varje utgående begäran tooAzure AD B2C.

Du måste också tooset in cookie-autentisering. Hej OpenID Connect mellanprogram använder cookies användarsessioner toomaintain, bland annat.

```CSharp
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // Initialize variables ...

    // Configure hello OWIN middleware
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseCookieAuthentication(new CookieAuthenticationOptions());
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Generate hello metadata address using hello tenant and policy information
                MetadataAddress = String.Format(AadInstance, Tenant, DefaultPolicy),

                // These are standard OpenID Connect parameters, with values pulled from web.config
                ClientId = ClientId,
                RedirectUri = RedirectUri,
                PostLogoutRedirectUri = RedirectUri,

                // Specify hello callbacks for each type of notifications
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    RedirectToIdentityProvider = OnRedirectToIdentityProvider,
                    AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    AuthenticationFailed = OnAuthenticationFailed,
                },

                // Specify hello claims toovalidate
                TokenValidationParameters = new TokenValidationParameters
                {
                    NameClaimType = "name"
                },

                // Specify hello scope by appending all of hello scopes requested into one string (seperated by a blank space)
                Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
            }
        );
    }

    // Implement hello "Notification" methods...
}
```

I `OpenIdConnectAuthenticationOptions` ovan, vi definierar en uppsättning Återanropsfunktioner för specifika meddelanden som tas emot av hello OpenID Connect mellanprogram. Dessa beteenden definieras med hjälp av en `OpenIdConnectAuthenticationNotifications` objekt och lagras i hello `Notifications` variabeln. I vårt exempel definiera vi tre olika återanrop beroende på hello-händelse.

### <a name="using-different-policies"></a>Med hjälp av olika principer

Hej `RedirectToIdentityProvider` avisering utlöses när en förfrågan görs tooAzure AD B2C. I hello Återanropsfunktionen `OnRedirectToIdentityProvider`, vi kontrollerar hello utgående anropet om vi vill toouse en annan princip. I ordning toodo lösenord återställa eller redigera en profil måste toouse hello motsvarande princip som hello princip i stället för hello ”registrering eller inloggning” standardprincipen för lösenordsåterställning.

I vårt exempel, när en användare vill tooreset hello lösenord eller redigera hello profil vi lägga till hello princip vi föredrar toouse till hello OWIN kontext. Som kan göras genom att göra hello följande:

```CSharp
    // Let hello middleware know you are trying toouse hello edit profile policy
    HttpContext.GetOwinContext().Set("Policy", EditProfilePolicyId);
```

Och du kan implementera hello Återanropsfunktionen `OnRedirectToIdentityProvider` hello följande:

```CSharp
/*
*  On each call tooAzure AD B2C, check if a policy (e.g. hello profile edit or password reset policy) has been specified in hello OWIN context.
*  If so, use that policy when making hello call. Also, don't request a code (since it won't be needed).
*/
private Task OnRedirectToIdentityProvider(RedirectToIdentityProviderNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    var policy = notification.OwinContext.Get<string>("Policy");

    if (!string.IsNullOrEmpty(policy) && !policy.Equals(DefaultPolicy))
    {
        notification.ProtocolMessage.Scope = OpenIdConnectScopes.OpenId;
        notification.ProtocolMessage.ResponseType = OpenIdConnectResponseTypes.IdToken;
        notification.ProtocolMessage.IssuerAddress = notification.ProtocolMessage.IssuerAddress.Replace(DefaultPolicy, policy);
    }

    return Task.FromResult(0);
}
```

### <a name="handling-authorization-codes"></a>Hantera auktoriseringskoder

Hej `AuthorizationCodeReceived` avisering utlöses när en Auktoriseringskoden tas emot. Hej OpenID Connect mellanprogram stöder inte utbyte koder för åtkomst-token. Du kan manuellt utväxla hello koden för hello token i en callback-funktion. Mer information finns i hello [dokumentationen](active-directory-b2c-devquickstarts-web-api-dotnet.md) som förklarar hur.

### <a name="handling-errors"></a>Felhantering

Hej `AuthenticationFailed` avisering utlöses när autentiseringen misslyckas. Du kan hantera hello fel som du vill i dess Återanropsmetoden. Dock bör du lägga till en kontroll för hello felkod `AADB2C90118`. Under hello körningen av hello ”registrering eller inloggning” hello användaren har hello möjlighet tooselect en **har du glömt lösenordet?** länk. I den här händelsen skickar Azure AD B2C appen den felkod som att appen ska gör en begäran med hello principen för lösenordsåterställning i stället.

```CSharp
/*
* Catch any failures received by hello authentication middleware and handle appropriately
*/
private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    // Handle hello error code that Azure AD B2C throws when trying tooreset a password from hello login page
    // because password reset is not supported by a "sign-up or sign-in policy"
    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If hello user clicked hello reset password link, redirect toohello reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```

### <a name="send-authentication-requests-tooazure-ad"></a>Skicka autentisering begäranden tooAzure AD

Appen är nu korrekt konfigurerade toocommunicate med Azure AD B2C med hjälp av autentiseringsprotokollet för hello OpenID Connect. OWIN hanterar hello information utforma autentiseringsmeddelanden, verifiera token från Azure AD B2C och upprätthålla användarsessioner. Allt som fortfarande är tooinitiate flödet för varje användare.

När en användare väljer **logga upp/inloggning**, **Redigera profil**, eller **Återställ lösenord** i hello webbapp hello associerade åtgärden anropas i `Controllers\AccountController.cs`:

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting toosign up or sign in
*/
public void SignUpSignIn()
{
    // Use hello default policy tooprocess hello sign up / sign in flow
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge();
        return;
    }

    Response.Redirect("/");
}

/*
*  Called when requesting tooedit a profile
*/
public void EditProfile()
{
    if (Request.IsAuthenticated)
    {
        // Let hello middleware know you are trying toouse hello edit profile policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
        HttpContext.GetOwinContext().Set("Policy", Startup.EditProfilePolicyId);

        // Set hello page tooredirect tooafter editing hello profile
        var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
        HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

        return;
    }

    Response.Redirect("/");

}

/*
*  Called when requesting tooreset a password
*/
public void ResetPassword()
{
    // Let hello middleware know you are trying toouse hello reset password policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
    HttpContext.GetOwinContext().Set("Policy", Startup.ResetPasswordPolicyId);

    // Set hello page tooredirect tooafter changing passwords
    var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
    HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

    return;
}
```

Du kan också använda OWIN toosign ut hello användare från hello app. I `Controllers\AccountController.cs` har vi:

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting toosign out
*/
public void SignOut()
{
    // toosign out hello user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

I tillägg tooexplicitly anropar en princip, kan du använda en `[Authorize]` tagg i dina domänkontrollanter som kör en princip om hello användaren inte är inloggad. Öppna `Controllers\HomeController.cs` och Lägg till hello `[Authorize]` taggen toohello anspråk domänkontrollant.  OWIN väljer hello senaste principen konfigureras när hello `[Authorize]` taggen med samma namn.

```CSharp
// Controllers\HomeController.cs

// You can use hello Authorize decorator tooexecute a certain policy if hello user is not already signed into hello app.
[Authorize]
public ActionResult Claims()
{
  ...
```

### <a name="display-user-information"></a>Visa användarinformation

När du autentiserar användare med OpenID Connect Azure AD B2C returnerar en ID-token toohello app som innehåller **anspråk**. Dessa är intyg om hello användare. Du kan använda anspråk toopersonalize din app.

Öppna hello `Controllers\HomeController.cs` fil. Du kan komma åt användaranspråk i dina domänkontrollanter via hello `ClaimsPrincipal.Current` säkerhetsobjekt.

```CSharp
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Du kan komma åt alla anspråk att programmet tar emot i hello samma sätt.  En lista över alla hello anspråk hello appen tar emot finns tillgänglig på hello **anspråk** sidan.
