---
title: aaaAzure AD B2C | Microsoft Docs
description: "Hur toobuild en .NET-webb-API med hjälp av Azure Active Directory B2C kan skyddas med OAuth 2.0-åtkomsttoken för autentisering."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: 
ms.assetid: 7146ed7f-2eb5-49e9-8d8b-ea1a895e1966
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: d45364216deda38ef44b60dd11e86d9a089ad509
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: Skapa ett .NET-webb-API

Med Azure Active Directory (Active AD) B2C kan du skydda ett webb-API med hjälp av OAuth 2.0-åtkomsttoken. Dessa token kan klienten appar tooauthenticate toohello API. Den här artikeln visar hur toocreate en .NET MVC ”uppgiftslistan” API som kan användare med ditt program tooCRUD uppgifter. hello web API skyddas med Azure AD B2C och tillåter endast autentiserade användare toomanage sina att göra-lista.

## <a name="create-an-azure-ad-b2c-directory"></a>Skapa en Azure AD B2C-katalog

Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient. En katalog är en behållare för alla användare, appar, grupper och mer. Om du inte redan har en B2C-katalog [skapar du en](active-directory-b2c-get-started.md) innan du fortsätter den här guiden.

> [!NOTE]
> hello klientprogrammet och webb-API måste använda hello samma Azure AD B2C-katalog.
>

## <a name="create-a-web-api"></a>Skapa ett webb-API

Sedan måste toocreate en web API-app i B2C-katalogen. Det ger Azure AD den information som den behöver toosecurely kommunicera med din app. toocreate en app, Följ [instruktionerna](active-directory-b2c-app-registration.md). Se till att:

* Inkludera en **webbapp** eller **webb-API** i hello program.
* Använd hello **omdirigerings-URI** `https://localhost:44332/` för hello webbprogrammet. Detta är hello standardplatsen för hello klienten för webbappen i det här kodexemplet.
* Kopiera hello **program-ID** som är tilldelade tooyour app. Du behöver det senare.
* Ange en appidentifierare i **App-ID-URI**.
* Lägg till behörigheter via hello **publicerade scope** menyn.

  [!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Skapa dina principer

I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md). Du behöver toocreate en princip toocommunicate med Azure AD B2C. Vi rekommenderar med hello kombineras sign-upp/inloggningsprincip, enligt beskrivningen i hello [referensartikeln om principer](active-directory-b2c-reference-policies.md). Se till att göra följande när du skapar en princip:

* Välj **Visningsnamn** och andra registreringsattribut i principen.
* Välj **Visningsnamn** och **Objekt-ID** som programanspråk för varje princip. Du kan också välja andra anspråk.
* Kopiera hello **namn** på varje princip när du har skapat den. Du behöver hello principnamn senare.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

När du har skapat hello princip du är klar toobuild din app.

## <a name="download-hello-code"></a>Hämta hello koden

hello-koden för den här självstudiekursen underhålls på [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi). Du kan klona hello exemplet genom att köra:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

När du har hämtat hello exempelkod igång öppna hello Visual Studio SLN-filen tooget. hello lösningsfilen innehåller två projekt: `TaskWebApp` och `TaskService`. `TaskWebApp`är en MVC-webbapp som hello användaren interagerar med. `TaskService`är hello appens backend-webb-API som lagrar varje användares att göra-lista. Den här artikeln handlar enbart hello `TaskService` program. toolearn hur toobuild `TaskWebApp` med Azure AD B2C finns i [våra självstudier för .NET web app](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

### <a name="update-hello-azure-ad-b2c-configuration"></a>Uppdatera hello Azure AD B2C-konfiguration

Exemplet är konfigurerade toouse hello principer och klient-ID för vår demo-klient. Om du vill att toouse en egen klient behöver du toodo hello följande:

1. Öppna `web.config` i hello `TaskService` projektet och Ersätt hello värden för
    * `ida:Tenant` med namnet på din klientorganisation
    * `ida:ClientId` med program-ID:t för ditt webb-API
    * `ida:SignUpSignInPolicyId` med namnet på din registrerings- eller inloggningsprincip

2. Öppna `web.config` i hello `TaskWebApp` projektet och Ersätt hello värden för
    * `ida:Tenant` med namnet på din klientorganisation
    * `ida:ClientId` med program-ID:t för din webbapp
    * `ida:ClientSecret` med den hemliga nyckeln för din webbapp
    * `ida:SignUpSignInPolicyId` med namnet på din registrerings- eller inloggningsprincip
    * `ida:EditProfilePolicyId` med namnet på din profilredigeringsprincip
    * `ida:ResetPasswordPolicyId` med namnet på din lösenordsåterställningsprincip


## <a name="secure-hello-api"></a>Skydda hello-API

Om du har en klient som anropar ditt API kan du skydda API:et (t.ex `TaskService`) med hjälp av OAuth 2.0-ägartoken. Detta säkerställer att varje begäran tooyour API bara är giltiga om hello-begäran har ett ägartoken. API:et kan acceptera och validera ägartoken med hjälp av Microsofts OWIN-bibliotek (Open Web Interface för .NET).

### <a name="install-owin"></a>Installera OWIN

Börja med att installera hello OWIN OAuth-autentisering pipeline med hjälp av hello Visual Studio Package Manager-konsolen.

```Console
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

Detta installerar hello OWIN mellanprogram som kommer att acceptera och validera ägar-token.

### <a name="add-an-owin-startup-class"></a>Lägga till en OWIN-startklass

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

### <a name="configure-oauth-20-authentication"></a>Konfigurera OAuth 2.0-autentisering

Öppna hello filen `App_Start\Startup.Auth.cs`, och implementera hello `ConfigureAuth(...)` metod. Det kan till exempel se ut så hello följande:

```CSharp
// App_Start\Startup.Auth.cs

 public partial class Startup
    {
        // These values are pulled from web.config
        public static string AadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
        public static string Tenant = ConfigurationManager.AppSettings["ida:Tenant"];
        public static string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
        public static string SignUpSignInPolicy = ConfigurationManager.AppSettings["ida:SignUpSignInPolicyId"];
        public static string DefaultPolicy = SignUpSignInPolicy;

        /*
         * Configure hello authorization OWIN middleware.
         */
        public void ConfigureAuth(IAppBuilder app)
        {
            TokenValidationParameters tvps = new TokenValidationParameters
            {
                // Accept only those tokens where hello audience of hello token is equal toohello client ID of this app
                ValidAudience = ClientId,
                AuthenticationType = Startup.DefaultPolicy
            };

            app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
            {
                // This SecurityTokenProvider fetches hello Azure AD B2C metadata & signing keys from hello OpenIDConnect metadata endpoint
                AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(AadInstance, Tenant, DefaultPolicy)))
            });
        }
    }
```

### <a name="secure-hello-task-controller"></a>Skydda uppgiftskontrollanten hello

När hello appen är konfigurerad toouse OAuth 2.0-autentisering, du kan skydda ditt webb-API genom att lägga till en `[Authorize]` tagg toohello uppgiftskontrollanten. Detta är hello domänkontrollant där alla manipulering för att göra-listan äger rum, därför bör du skydda hello hela kontrollanten på klassnivå hello. Du kan också lägga till hello `[Authorize]` tagga tooindividual åtgärder för mer detaljerad kontroll.

```CSharp
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-hello-token"></a>Hämta användarinformation från hello-token

`TasksController`lagrar uppgifter i en databas där varje uppgift har en associerad användare som ”äger” hello-aktivitet. hello ägaren identifieras med hello användarens **objekt-ID**. (Det är därför du behövs tooadd hello objekt-ID som ett program i alla dina principer.)

```CSharp
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

### <a name="validate-hello-permissions-in-hello-token"></a>Validera hello behörigheter i hello-token

Ett vanligt krav för webb-API: er är toovalidate hello ”scope” finns i hello-token. Detta säkerställer att hello-användaren har godkänt toohello behörigheter krävs tooaccess hello att göra tjänsten.

```CSharp
public IEnumerable<Models.Task> Get()
{
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "read")
    {
        throw new HttpResponseException(new HttpResponseMessage {
            StatusCode = HttpStatusCode.Unauthorized,
            ReasonPhrase = "hello Scope claim does not contain 'read' or scope claim not found"
        });
    }
    ...
}
```

## <a name="run-hello-sample-app"></a>Kör hello sample-appen

Slutligen bygger du och kör både `TaskWebApp` och `TaskService`. Skapa några uppgifter i hello användarens att göra-lista och Observera hur de finns kvar i hello API när du stoppa och starta om hello-klienten.

## <a name="edit-your-policies"></a>Redigera dina principer

När du har skyddat ett API med hjälp av Azure AD B2C, kan du experimentera med din inloggning-i/Sign-upp principen och visa hello effekterna (eller avsaknad av) på hello API. Du kan ändra hello programanspråken i hello principer och ändra hello användarinformation som finns tillgängliga i hello webb-API. Anspråk som du lägger till kommer att vara tillgängliga tooyour .NET MVC webb-API i hello `ClaimsPrincipal` objekt, enligt beskrivningen tidigare i den här artikeln.
