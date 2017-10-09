---
title: aaaAzure Active Directory B2C | Microsoft Docs
description: "Hur toobuild en .NET-webb-app och anropa ett webb-API: et med Azure Active Directory B2C och OAuth 2.0-åtkomsttoken."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: 
ms.assetid: d3888556-2647-4a42-b068-027f9374aa61
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: 9b248e3bf18968e12aae73c07083fa8278befb3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-call-a-net-web-api-from-a-net-web-app"></a>Azure AD B2C: Anropa .NET-webb-API från en .NET-webbapp

Med hjälp av Azure AD B2C kan du lägga till kraftfulla identity management funktioner tooyour webbappar och webb-API: er. Den här artikeln beskrivs hur toorequest komma åt token och göra anrop från en .NET ”uppgiftslistan” web app tooa .NET webb-api.

Den här artikeln inte beskriver hur tooimplement inloggning, registrering och -hantering med Azure AD B2C-profilen. Den fokuserar på anropande webb-API: er när hello användaren redan är autentiserad. Om du inte redan gjort bör du:

* Kom igång med en [.NET-webbapp](active-directory-b2c-devquickstarts-web-dotnet-susi.md)
* Kom igång med en [.NET webb-api](active-directory-b2c-devquickstarts-api-dotnet.md)

## <a name="prerequisite"></a>Krav

toobuild ett webbprogram som anropar ett webb-api, som du behöver:

1. [Skapa en Azure AD B2C-klient](active-directory-b2c-get-started.md).
2. [Registrera ett web api](active-directory-b2c-app-registration.md#register-a-web-api).
3. [Registrera en webbapp](active-directory-b2c-app-registration.md#register-a-web-app).
4. [Konfigurera principer](active-directory-b2c-reference-policies.md).
5. [Bevilja hello web app behörigheter toouse hello webb-api](active-directory-b2c-access-tokens.md#publishing-permissions).

> [!IMPORTANT]
> hello klientprogrammet och webb-API måste använda hello samma Azure AD B2C-katalog.
>

## <a name="download-hello-code"></a>Hämta hello koden

hello-koden för den här självstudiekursen underhålls på [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi). Du kan klona hello exemplet genom att köra:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

När du har hämtat hello exempelkod igång öppna hello Visual Studio SLN-filen tooget. hello lösningsfilen innehåller två projekt: `TaskWebApp` och `TaskService`. `TaskWebApp`är en MVC-webbapp som hello användaren interagerar med. `TaskService`är hello appens backend-webb-API som lagrar varje användares att göra-lista. Den här artikeln täcker inte skapa hello `TaskWebApp` webbapp eller hello `TaskService` webb-api. toolearn hur toobuild hello .NET web app med Azure AD B2C finns i vår [.NET-självstudiekurs om web app](active-directory-b2c-devquickstarts-web-dotnet-susi.md). toolearn hur toobuild hello .NET webb-API som kan skyddas med Azure AD B2C finns våra [.NET-självstudiekurs om webb-API](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="update-hello-azure-ad-b2c-configuration"></a>Uppdatera hello Azure AD B2C-konfiguration

Exemplet är konfigurerade toouse hello principer och klient-ID för vår demo-klient. Om du vill att toouse en egen klient:

1. Öppna `web.config` i hello `TaskService` projektet och Ersätt hello värden för

    * `ida:Tenant` med namnet på din klientorganisation
    * `ida:ClientId`med web api program-ID
    * `ida:SignUpSignInPolicyId` med namnet på din registrerings- eller inloggningsprincip

2. Öppna `web.config` i hello `TaskWebApp` projektet och Ersätt hello värden för

    * `ida:Tenant` med namnet på din klientorganisation
    * `ida:ClientId` med program-ID:t för din webbapp
    * `ida:ClientSecret` med den hemliga nyckeln för din webbapp
    * `ida:SignUpSignInPolicyId` med namnet på din registrerings- eller inloggningsprincip
    * `ida:EditProfilePolicyId` med namnet på din profilredigeringsprincip
    * `ida:ResetPasswordPolicyId` med namnet på din lösenordsåterställningsprincip



## <a name="requesting-and-saving-an-access-token"></a>Begär och spara en åtkomst-token

### <a name="specify-hello-permissions"></a>Ange hello behörigheter

I ordning toomake hello anropet toohello webb-API, behöver du tooauthenticate hello användare (med din sign-upp/inloggningsprincip) och [få en åtkomsttoken](active-directory-b2c-access-tokens.md) från Azure AD B2C. I ordning tooreceive en åtkomst-token, måste först du ange hello behörigheter som du vill att hello åtkomst-token toogrant. hello behörigheter har angetts i hello `scope` parameter när du gör hello begäran toohello `/authorize` slutpunkt. Till exempel tooacquire en åtkomst-token med hello ”läsa” behörighet toohello resursprogram som har hello App-ID-URI för `https://contoso.onmicrosoft.com/tasks`, hello Omfattningen skulle vara `https://contoso.onmicrosoft.com/tasks/read`.

toospecify hello scope i vårt exempel, öppna hello filen `App_Start\Startup.Auth.cs` och definiera hello `Scope` variabeln i OpenIdConnectAuthenticationOptions.

```CSharp
// App_Start\Startup.Auth.cs

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ...

            // Specify hello scope by appending all of hello scopes requested into one string (seperated by a blank space)
            Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
        }
    );
}
```

### <a name="exchange-hello-authorization-code-for-an-access-token"></a>Exchange hello auktoriseringskod för en åtkomst-token

När en användare har slutförts hello registrering eller inloggning erfarenhet får din app en Auktoriseringskoden från Azure AD B2C. Hej OWIN OpenID Connect mellanprogram lagrar hello koden, men kommer inte exchange för en åtkomst-token. Du kan använda hello [MSAL biblioteket](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) toomake hello exchange. I vårt exempel vi har konfigurerat ett återanrop i hello OpenID Connect mellanprogram när en Auktoriseringskoden tas emot. I hello återanrop vi använda MSAL tooexchange hello kod för en token och spara hello-token till hello-cachen.

```CSharp
/*
* Callback function when an authorization code is received
*/
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
    // Extract hello code from hello response notification
    var code = notification.Code;

    var userObjectId = notification.AuthenticationTicket.Identity.FindFirst(ObjectIdElement).Value;
    var authority = String.Format(AadInstance, Tenant, DefaultPolicy);
    var httpContext = notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase;

    // Exchange hello code for a token. Make sure toospecify hello necessary scopes
    ClientCredential cred = new ClientCredential(ClientSecret);
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                            RedirectUri, cred, new NaiveSessionCache(userObjectId, httpContext));
    var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { ReadTasksScope, WriteTasksScope }, code, DefaultPolicy);
}
```

## <a name="calling-hello-web-api"></a>Anropar hello webb-API

Det här avsnittet beskrivs hur toouse hello token togs emot under sign-upp/inloggning med Azure AD B2C i ordning tooaccess hello webb-API.

### <a name="retrieve-hello-saved-token-in-hello-controllers"></a>Hämta hello spara token i hello domänkontrollanter

Hej `TasksController` ansvarar för att kommunicera med hello webb-API för att skicka HTTP-begäranden toohello API tooread, skapa och ta bort uppgifter. Eftersom hello API skyddas av Azure AD B2C måste toofirst hämta hello token som du sparade i hello senare steg.

```CSharp
// Controllers\TasksController.cs

/*
* Uses MSAL tooretrieve hello token from hello cache
*/
private async void acquireToken(String[] scope)
{
    string userObjectID = ClaimsPrincipal.Current.FindFirst(Startup.ObjectIdElement).Value;
    string authority = String.Format(Startup.AadInstance, Startup.Tenant, Startup.DefaultPolicy);

    ClientCredential credential = new ClientCredential(Startup.ClientSecret);

    // Retrieve hello token using hello provided scopes
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                        Startup.RedirectUri, credential,
                                        new NaiveSessionCache(userObjectID, this.HttpContext));
    AuthenticationResult result = await app.AcquireTokenSilentAsync(scope);

    accessToken = result.Token;
}
```

### <a name="read-tasks-from-hello-web-api"></a>Läsa uppgifter från hello webb-API

När du har en token kan du bifoga den toohello HTTP `GET` begäran i hello `Authorization` huvud toosecurely anropet `TaskService`:

```CSharp
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try {

        // Retrieve hello token with hello specified scopes
        acquireToken(new string[] { Startup.ReadTasksScope });

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, apiEndpoint);

        // Add token toohello Authorization header and make hello request
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
        HttpResponseMessage response = await client.SendAsync(request);

        // Handle response ...
}

```

### <a name="create-and-delete-tasks-on-hello-web-api"></a>Skapa och ta bort uppgifter på hello webb-API

Följ hello samma mönstret när du skickar `POST` och `DELETE` toohello web API-begäranden med MSAL tooretrieve hello åtkomst-token från hello cache.

## <a name="run-hello-sample-app"></a>Kör hello sample-appen

Slutligen skapar och kör både hello-appar. Logga och logga in och skapa aktiviteter för hello inloggad användare. Logga ut och logga in som en annan användare. Skapa aktiviteter för användaren. Lägg märke till hur hello uppgifterna lagras per användare på hello API, eftersom hello API extraherar hello användarens identitet från hello-token som tas emot. Prova också spelar med hello scope. Ta bort hello-behörighet för ”skriva” och försök sedan lägga till en aktivitet. Kontrollera att toosign ut varje gång du ändrar hello omfång.

