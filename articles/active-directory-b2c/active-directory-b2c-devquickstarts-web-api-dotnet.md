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
# <a name="azure-ad-b2c-call-a-net-web-api-from-a-net-web-app"></a><span data-ttu-id="e22e0-103">Azure AD B2C: Anropa .NET-webb-API från en .NET-webbapp</span><span class="sxs-lookup"><span data-stu-id="e22e0-103">Azure AD B2C: Call a .NET web API from a .NET web app</span></span>

<span data-ttu-id="e22e0-104">Med hjälp av Azure AD B2C kan du lägga till kraftfulla identity management funktioner tooyour webbappar och webb-API: er.</span><span class="sxs-lookup"><span data-stu-id="e22e0-104">By using Azure AD B2C, you can add powerful identity management features tooyour web apps and web APIs.</span></span> <span data-ttu-id="e22e0-105">Den här artikeln beskrivs hur toorequest komma åt token och göra anrop från en .NET ”uppgiftslistan” web app tooa .NET webb-api.</span><span class="sxs-lookup"><span data-stu-id="e22e0-105">This article discusses how toorequest access tokens and make calls from a .NET "to-do list" web app tooa .NET web api.</span></span>

<span data-ttu-id="e22e0-106">Den här artikeln inte beskriver hur tooimplement inloggning, registrering och -hantering med Azure AD B2C-profilen.</span><span class="sxs-lookup"><span data-stu-id="e22e0-106">This article does not cover how tooimplement sign-in, sign-up and profile management with Azure AD B2C.</span></span> <span data-ttu-id="e22e0-107">Den fokuserar på anropande webb-API: er när hello användaren redan är autentiserad.</span><span class="sxs-lookup"><span data-stu-id="e22e0-107">It focuses on calling web APIs after hello user is already authenticated.</span></span> <span data-ttu-id="e22e0-108">Om du inte redan gjort bör du:</span><span class="sxs-lookup"><span data-stu-id="e22e0-108">If you haven't already, you should:</span></span>

* <span data-ttu-id="e22e0-109">Kom igång med en [.NET-webbapp](active-directory-b2c-devquickstarts-web-dotnet-susi.md)</span><span class="sxs-lookup"><span data-stu-id="e22e0-109">Get started with a [.NET web app](active-directory-b2c-devquickstarts-web-dotnet-susi.md)</span></span>
* <span data-ttu-id="e22e0-110">Kom igång med en [.NET webb-api](active-directory-b2c-devquickstarts-api-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="e22e0-110">Get started with a [.NET web api](active-directory-b2c-devquickstarts-api-dotnet.md)</span></span>

## <a name="prerequisite"></a><span data-ttu-id="e22e0-111">Krav</span><span class="sxs-lookup"><span data-stu-id="e22e0-111">Prerequisite</span></span>

<span data-ttu-id="e22e0-112">toobuild ett webbprogram som anropar ett webb-api, som du behöver:</span><span class="sxs-lookup"><span data-stu-id="e22e0-112">toobuild a web application that calls a web api, you need to:</span></span>

1. <span data-ttu-id="e22e0-113">[Skapa en Azure AD B2C-klient](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e22e0-113">[Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>
2. <span data-ttu-id="e22e0-114">[Registrera ett web api](active-directory-b2c-app-registration.md#register-a-web-api).</span><span class="sxs-lookup"><span data-stu-id="e22e0-114">[Register a web api](active-directory-b2c-app-registration.md#register-a-web-api).</span></span>
3. <span data-ttu-id="e22e0-115">[Registrera en webbapp](active-directory-b2c-app-registration.md#register-a-web-app).</span><span class="sxs-lookup"><span data-stu-id="e22e0-115">[Register a web app](active-directory-b2c-app-registration.md#register-a-web-app).</span></span>
4. <span data-ttu-id="e22e0-116">[Konfigurera principer](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="e22e0-116">[Set up policies](active-directory-b2c-reference-policies.md).</span></span>
5. <span data-ttu-id="e22e0-117">[Bevilja hello web app behörigheter toouse hello webb-api](active-directory-b2c-access-tokens.md#publishing-permissions).</span><span class="sxs-lookup"><span data-stu-id="e22e0-117">[Grant hello web app permissions toouse hello web api](active-directory-b2c-access-tokens.md#publishing-permissions).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e22e0-118">hello klientprogrammet och webb-API måste använda hello samma Azure AD B2C-katalog.</span><span class="sxs-lookup"><span data-stu-id="e22e0-118">hello client application and web API must use hello same Azure AD B2C directory.</span></span>
>

## <a name="download-hello-code"></a><span data-ttu-id="e22e0-119">Hämta hello koden</span><span class="sxs-lookup"><span data-stu-id="e22e0-119">Download hello code</span></span>

<span data-ttu-id="e22e0-120">hello-koden för den här självstudiekursen underhålls på [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span><span class="sxs-lookup"><span data-stu-id="e22e0-120">hello code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="e22e0-121">Du kan klona hello exemplet genom att köra:</span><span class="sxs-lookup"><span data-stu-id="e22e0-121">You can clone hello sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="e22e0-122">När du har hämtat hello exempelkod igång öppna hello Visual Studio SLN-filen tooget.</span><span class="sxs-lookup"><span data-stu-id="e22e0-122">After you download hello sample code, open hello Visual Studio .sln file tooget started.</span></span> <span data-ttu-id="e22e0-123">hello lösningsfilen innehåller två projekt: `TaskWebApp` och `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="e22e0-123">hello solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="e22e0-124">`TaskWebApp`är en MVC-webbapp som hello användaren interagerar med.</span><span class="sxs-lookup"><span data-stu-id="e22e0-124">`TaskWebApp` is a MVC web application that hello user interacts with.</span></span> <span data-ttu-id="e22e0-125">`TaskService`är hello appens backend-webb-API som lagrar varje användares att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="e22e0-125">`TaskService` is hello app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="e22e0-126">Den här artikeln täcker inte skapa hello `TaskWebApp` webbapp eller hello `TaskService` webb-api.</span><span class="sxs-lookup"><span data-stu-id="e22e0-126">This article does not cover building hello `TaskWebApp` web app or hello `TaskService` web api.</span></span> <span data-ttu-id="e22e0-127">toolearn hur toobuild hello .NET web app med Azure AD B2C finns i vår [.NET-självstudiekurs om web app](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="e22e0-127">toolearn how toobuild hello .NET web app using Azure AD B2C, see our [.NET web app tutorial](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span> <span data-ttu-id="e22e0-128">toolearn hur toobuild hello .NET webb-API som kan skyddas med Azure AD B2C finns våra [.NET-självstudiekurs om webb-API](active-directory-b2c-devquickstarts-api-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="e22e0-128">toolearn how toobuild hello .NET web API secured using Azure AD B2C, see our [.NET web API tutorial](active-directory-b2c-devquickstarts-api-dotnet.md).</span></span>

### <a name="update-hello-azure-ad-b2c-configuration"></a><span data-ttu-id="e22e0-129">Uppdatera hello Azure AD B2C-konfiguration</span><span class="sxs-lookup"><span data-stu-id="e22e0-129">Update hello Azure AD B2C configuration</span></span>

<span data-ttu-id="e22e0-130">Exemplet är konfigurerade toouse hello principer och klient-ID för vår demo-klient.</span><span class="sxs-lookup"><span data-stu-id="e22e0-130">Our sample is configured toouse hello policies and client ID of our demo tenant.</span></span> <span data-ttu-id="e22e0-131">Om du vill att toouse en egen klient:</span><span class="sxs-lookup"><span data-stu-id="e22e0-131">If you would like toouse your own tenant:</span></span>

1. <span data-ttu-id="e22e0-132">Öppna `web.config` i hello `TaskService` projektet och Ersätt hello värden för</span><span class="sxs-lookup"><span data-stu-id="e22e0-132">Open `web.config` in hello `TaskService` project and replace hello values for</span></span>

    * <span data-ttu-id="e22e0-133">`ida:Tenant` med namnet på din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="e22e0-133">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="e22e0-134">`ida:ClientId`med web api program-ID</span><span class="sxs-lookup"><span data-stu-id="e22e0-134">`ida:ClientId` with your web api application ID</span></span>
    * <span data-ttu-id="e22e0-135">`ida:SignUpSignInPolicyId` med namnet på din registrerings- eller inloggningsprincip</span><span class="sxs-lookup"><span data-stu-id="e22e0-135">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>

2. <span data-ttu-id="e22e0-136">Öppna `web.config` i hello `TaskWebApp` projektet och Ersätt hello värden för</span><span class="sxs-lookup"><span data-stu-id="e22e0-136">Open `web.config` in hello `TaskWebApp` project and replace hello values for</span></span>

    * <span data-ttu-id="e22e0-137">`ida:Tenant` med namnet på din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="e22e0-137">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="e22e0-138">`ida:ClientId` med program-ID:t för din webbapp</span><span class="sxs-lookup"><span data-stu-id="e22e0-138">`ida:ClientId` with your web app application ID</span></span>
    * <span data-ttu-id="e22e0-139">`ida:ClientSecret` med den hemliga nyckeln för din webbapp</span><span class="sxs-lookup"><span data-stu-id="e22e0-139">`ida:ClientSecret` with your web app secret key</span></span>
    * <span data-ttu-id="e22e0-140">`ida:SignUpSignInPolicyId` med namnet på din registrerings- eller inloggningsprincip</span><span class="sxs-lookup"><span data-stu-id="e22e0-140">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
    * <span data-ttu-id="e22e0-141">`ida:EditProfilePolicyId` med namnet på din profilredigeringsprincip</span><span class="sxs-lookup"><span data-stu-id="e22e0-141">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
    * <span data-ttu-id="e22e0-142">`ida:ResetPasswordPolicyId` med namnet på din lösenordsåterställningsprincip</span><span class="sxs-lookup"><span data-stu-id="e22e0-142">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>



## <a name="requesting-and-saving-an-access-token"></a><span data-ttu-id="e22e0-143">Begär och spara en åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="e22e0-143">Requesting and saving an access token</span></span>

### <a name="specify-hello-permissions"></a><span data-ttu-id="e22e0-144">Ange hello behörigheter</span><span class="sxs-lookup"><span data-stu-id="e22e0-144">Specify hello permissions</span></span>

<span data-ttu-id="e22e0-145">I ordning toomake hello anropet toohello webb-API, behöver du tooauthenticate hello användare (med din sign-upp/inloggningsprincip) och [få en åtkomsttoken](active-directory-b2c-access-tokens.md) från Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="e22e0-145">In order toomake hello call toohello web API, you need tooauthenticate hello user (using your sign-up/sign-in policy) and [receive an access token](active-directory-b2c-access-tokens.md) from Azure AD B2C.</span></span> <span data-ttu-id="e22e0-146">I ordning tooreceive en åtkomst-token, måste först du ange hello behörigheter som du vill att hello åtkomst-token toogrant.</span><span class="sxs-lookup"><span data-stu-id="e22e0-146">In order tooreceive an access token, you first must specify hello permissions you would like hello access token toogrant.</span></span> <span data-ttu-id="e22e0-147">hello behörigheter har angetts i hello `scope` parameter när du gör hello begäran toohello `/authorize` slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="e22e0-147">hello permissions are specified in hello `scope` parameter when you make hello request toohello `/authorize` endpoint.</span></span> <span data-ttu-id="e22e0-148">Till exempel tooacquire en åtkomst-token med hello ”läsa” behörighet toohello resursprogram som har hello App-ID-URI för `https://contoso.onmicrosoft.com/tasks`, hello Omfattningen skulle vara `https://contoso.onmicrosoft.com/tasks/read`.</span><span class="sxs-lookup"><span data-stu-id="e22e0-148">For example, tooacquire an access token with hello “read” permission toohello resource application that has hello App ID URI of `https://contoso.onmicrosoft.com/tasks`, hello scope would be `https://contoso.onmicrosoft.com/tasks/read`.</span></span>

<span data-ttu-id="e22e0-149">toospecify hello scope i vårt exempel, öppna hello filen `App_Start\Startup.Auth.cs` och definiera hello `Scope` variabeln i OpenIdConnectAuthenticationOptions.</span><span class="sxs-lookup"><span data-stu-id="e22e0-149">toospecify hello scope in our sample, open hello file `App_Start\Startup.Auth.cs` and define hello `Scope` variable in OpenIdConnectAuthenticationOptions.</span></span>

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

### <a name="exchange-hello-authorization-code-for-an-access-token"></a><span data-ttu-id="e22e0-150">Exchange hello auktoriseringskod för en åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="e22e0-150">Exchange hello authorization code for an access token</span></span>

<span data-ttu-id="e22e0-151">När en användare har slutförts hello registrering eller inloggning erfarenhet får din app en Auktoriseringskoden från Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="e22e0-151">After an user completes hello sign-up or sign-in experience, your app will receive an authorization code from Azure AD B2C.</span></span> <span data-ttu-id="e22e0-152">Hej OWIN OpenID Connect mellanprogram lagrar hello koden, men kommer inte exchange för en åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="e22e0-152">hello OWIN OpenID Connect middleware will store hello code, but will not exchange it for an access token.</span></span> <span data-ttu-id="e22e0-153">Du kan använda hello [MSAL biblioteket](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) toomake hello exchange.</span><span class="sxs-lookup"><span data-stu-id="e22e0-153">You can use hello [MSAL library](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) toomake hello exchange.</span></span> <span data-ttu-id="e22e0-154">I vårt exempel vi har konfigurerat ett återanrop i hello OpenID Connect mellanprogram när en Auktoriseringskoden tas emot.</span><span class="sxs-lookup"><span data-stu-id="e22e0-154">In our sample, we configured a notification callback into hello OpenID Connect middleware whenever an authorization code is received.</span></span> <span data-ttu-id="e22e0-155">I hello återanrop vi använda MSAL tooexchange hello kod för en token och spara hello-token till hello-cachen.</span><span class="sxs-lookup"><span data-stu-id="e22e0-155">In hello callback, we use MSAL tooexchange hello code for a token and save hello token into hello cache.</span></span>

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

## <a name="calling-hello-web-api"></a><span data-ttu-id="e22e0-156">Anropar hello webb-API</span><span class="sxs-lookup"><span data-stu-id="e22e0-156">Calling hello web API</span></span>

<span data-ttu-id="e22e0-157">Det här avsnittet beskrivs hur toouse hello token togs emot under sign-upp/inloggning med Azure AD B2C i ordning tooaccess hello webb-API.</span><span class="sxs-lookup"><span data-stu-id="e22e0-157">This section discusses how toouse hello token received during sign-up/sign-in with Azure AD B2C in order tooaccess hello web API.</span></span>

### <a name="retrieve-hello-saved-token-in-hello-controllers"></a><span data-ttu-id="e22e0-158">Hämta hello spara token i hello domänkontrollanter</span><span class="sxs-lookup"><span data-stu-id="e22e0-158">Retrieve hello saved token in hello controllers</span></span>

<span data-ttu-id="e22e0-159">Hej `TasksController` ansvarar för att kommunicera med hello webb-API för att skicka HTTP-begäranden toohello API tooread, skapa och ta bort uppgifter.</span><span class="sxs-lookup"><span data-stu-id="e22e0-159">hello `TasksController` is responsible for communicating with hello web API and for sending HTTP requests toohello API tooread, create, and delete tasks.</span></span> <span data-ttu-id="e22e0-160">Eftersom hello API skyddas av Azure AD B2C måste toofirst hämta hello token som du sparade i hello senare steg.</span><span class="sxs-lookup"><span data-stu-id="e22e0-160">Because hello API is secured by Azure AD B2C, you need toofirst retrieve hello token you saved in hello above step.</span></span>

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

### <a name="read-tasks-from-hello-web-api"></a><span data-ttu-id="e22e0-161">Läsa uppgifter från hello webb-API</span><span class="sxs-lookup"><span data-stu-id="e22e0-161">Read tasks from hello web API</span></span>

<span data-ttu-id="e22e0-162">När du har en token kan du bifoga den toohello HTTP `GET` begäran i hello `Authorization` huvud toosecurely anropet `TaskService`:</span><span class="sxs-lookup"><span data-stu-id="e22e0-162">When you have a token, you can attach it toohello HTTP `GET` request in hello `Authorization` header toosecurely call `TaskService`:</span></span>

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

### <a name="create-and-delete-tasks-on-hello-web-api"></a><span data-ttu-id="e22e0-163">Skapa och ta bort uppgifter på hello webb-API</span><span class="sxs-lookup"><span data-stu-id="e22e0-163">Create and delete tasks on hello web API</span></span>

<span data-ttu-id="e22e0-164">Följ hello samma mönstret när du skickar `POST` och `DELETE` toohello web API-begäranden med MSAL tooretrieve hello åtkomst-token från hello cache.</span><span class="sxs-lookup"><span data-stu-id="e22e0-164">Follow hello same pattern when you send `POST` and `DELETE` requests toohello web API, using MSAL tooretrieve hello access token from hello cache.</span></span>

## <a name="run-hello-sample-app"></a><span data-ttu-id="e22e0-165">Kör hello sample-appen</span><span class="sxs-lookup"><span data-stu-id="e22e0-165">Run hello sample app</span></span>

<span data-ttu-id="e22e0-166">Slutligen skapar och kör både hello-appar.</span><span class="sxs-lookup"><span data-stu-id="e22e0-166">Finally, build and run both hello apps.</span></span> <span data-ttu-id="e22e0-167">Logga och logga in och skapa aktiviteter för hello inloggad användare.</span><span class="sxs-lookup"><span data-stu-id="e22e0-167">Sign up and sign in, and create tasks for hello signed-in user.</span></span> <span data-ttu-id="e22e0-168">Logga ut och logga in som en annan användare.</span><span class="sxs-lookup"><span data-stu-id="e22e0-168">Sign out and sign in as a different user.</span></span> <span data-ttu-id="e22e0-169">Skapa aktiviteter för användaren.</span><span class="sxs-lookup"><span data-stu-id="e22e0-169">Create tasks for that user.</span></span> <span data-ttu-id="e22e0-170">Lägg märke till hur hello uppgifterna lagras per användare på hello API, eftersom hello API extraherar hello användarens identitet från hello-token som tas emot.</span><span class="sxs-lookup"><span data-stu-id="e22e0-170">Notice how hello tasks are stored per-user on hello API, because hello API extracts hello user's identity from hello token it receives.</span></span> <span data-ttu-id="e22e0-171">Prova också spelar med hello scope.</span><span class="sxs-lookup"><span data-stu-id="e22e0-171">Also try playing with hello scopes.</span></span> <span data-ttu-id="e22e0-172">Ta bort hello-behörighet för ”skriva” och försök sedan lägga till en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="e22e0-172">Remove hello permission too"write" and then try adding a task.</span></span> <span data-ttu-id="e22e0-173">Kontrollera att toosign ut varje gång du ändrar hello omfång.</span><span class="sxs-lookup"><span data-stu-id="e22e0-173">Just make sure toosign out each time you change hello scope.</span></span>

