---
title: "aaaAzure AD .NET web API komma igång | Microsoft Docs"
description: "Hur toobuild en .NET MVC webb-API som kan integreras med Azure AD för autentisering och auktorisering."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 91c93e1fe18855f5648076e59e2ccf081eec34bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="help-protect-a-web-api-by-using-bearer-tokens-from-azure-ad"></a>Skydda ett webb-API med ägar-token från Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Om du utvecklar ett program som ger åtkomst till tooprotected resurser måste tooknow hur tooprevent obefogat åt toothose resurser.
Azure Active Directory (AD Azure) gör det enkelt och enkelt toohelp skydda ett webb-API med OAuth 2.0-ägar åtkomst-token med bara några få rader med kod.

Du kan göra detta skydd genom att använda hello Microsofts implementering av hello community-driven OWIN mellanprogram ingår i .NET Framework 4.5 i ASP.NET web apps. Vi använder här OWIN toobuild en ”tooDo listan” webb-API som:

* Anger vilket API: er är skyddad.
* Verifierar att hello webb-API-anrop kan innehålla en giltig åtkomsttoken.

toobuild hello tooDo lista API, måste du först:

1. Registrera ett program med Azure AD.
2. Ställ in hello app toouse hello OWIN autentisering pipeline.
3. Konfigurera en klient programmet toocall hello webb-API.

tooget har startats [hämta hello app stommen](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) eller [hämta hello slutförts exempel](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Varje är en lösning för Visual Studio 2013. Du måste också en Azure AD-klient i vilka tooregister ditt program. Om du inte redan har en [Lär dig hur tooget en](active-directory-howto-tenant.md).

## <a name="step-1-register-an-application-with-azure-ad"></a>Steg 1: Registrera ett program med Azure AD
toohelp skydda ditt program måste du först behöver toocreate ett program i din klient ger Azure AD med några uppgifter.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).

2. Klicka på ditt konto hello översta fältet. I hello **Directory** Välj hello Azure AD-klient där du vill att tooregister ditt program.

3. Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.

4. Klicka på **App registreringar**, och välj sedan **Lägg till**.

5. Följ anvisningarna för hello och skapa en ny **webbprogram och/eller webb-API**.
  * **Namnet** beskriver toousers ditt program. Ange **tooDo tjänsten**.
  * **Omdirigerings-Uri** är en kombination av schemat och strängen som Azure AD använder tooreturn eventuella tokens som din app har begärt. Ange `https://localhost:44321/` för det här värdet.

6. Från hello **inställningar** -> **egenskaper** för programmet, uppdatera hello App-ID-URI. Ange en klient-specifika identifierare. Ange till exempel `https://contoso.onmicrosoft.com/TodoListService`.

7. Spara hello-konfigurationen. Lämna hello portal öppen, eftersom du måste också tooregister klientprogrammet inom kort.

## <a name="step-2-set-up-hello-app-toouse-hello-owin-authentication-pipeline"></a>Steg 2: Konfigurera hello app toouse hello OWIN autentisering pipeline
toovalidate inkommande begäranden och token måste tooset upp ditt program toocommunicate med Azure AD.

1. toobegin, öppna hello lösningen och lägga till hello OWIN mellanprogram NuGet-paket toohello TodoListService projekt med hjälp av hello Package Manager-konsolen.

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. Lägg till en OWIN klassen toohello TodoListService Startprojekt kallas `Startup.cs`.  Högerklicka på hello projektet, Välj **Lägg till** > **nytt objekt**, och sök sedan efter **OWIN**. Hej OWIN mellanprogram ska anropa hello `Configuration(…)` metod när appen startar.

3. Ändra hello klassdeklarationen för`public partial class Startup`. Vi har redan implementerats en del av den här klassen för dig i en annan fil. I hello `Configuration(…)` metod, gör ett anrop för`ConfgureAuth(…)` tooset in verifiering för ditt webbprogram.

    ```C#
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. Öppna hello filen `App_Start\Startup.Auth.cs` och implementera hello `ConfigureAuth(…)` metod. Hej parametrar som du anger i `WindowsAzureActiveDirectoryBearerAuthenticationOptions` fungerar som koordinater för din app toocommunicate med Azure AD.

    ```C#
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
            });
    }
    ```

5. Nu kan du använda `[Authorize]` attribut toohelp skydda dina domänkontrollanter och åtgärder med JSON-Webbtoken (JWT) ägar-autentisering. Skapa snygga hello `Controllers\TodoListController.cs` klass med en auktorisera-tagg. Detta tvingar hello användaren toosign i innan sidan.

    ```C#
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    När en auktoriserad anroparen har anropar en hello `TodoListController` API: er, hello åtgärden kanske behöver komma åt tooinformation om hello anroparen. OWIN tillhandahåller åtkomst toohello anspråk i hello ägar-token via hello `ClaimsPrincpal` objekt.  

6. Ett vanligt krav för webb-API: er är toovalidate hello ”scope” finns i hello-token. Detta säkerställer att hello-användaren har godkänt toohello behörigheter krävs tooaccess hello tooDo tjänsten.

    ```C#
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is hello default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "hello Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. Öppna hello `web.config` filen i hello roten för hello TodoListService projekt och ange dina konfigurationsvärden i hello `<appSettings>` avsnitt.
  * `ida:Tenant`är din Azure AD-klient – till exempel contoso.onmicrosoft.com hello namn.
  * `ida:Audience`är hello App-ID-URI för hello-program som du angav i hello Azure-portalen.

## <a name="step-3-configure-a-client-application-and-run-hello-service"></a>Steg 3: Konfigurera ett klientprogram och köra hello-tjänsten
Innan du kan se hello tooDo tjänsten i åtgärden måste tooconfigure hello tooDo lista klienten så att den kan hämta token från Azure AD att anrop toohello tjänst.

1. Gå tillbaka toohello [Azure-portalen](https://portal.azure.com).

2. Skapa ett nytt program i Azure AD-klienten och välj **internt klientprogram** hello resulterande fråga.
  * **Namnet** beskriver toousers ditt program.
  * Ange `http://TodoListClient/` för hello **omdirigerings-Uri** värde.

3. När du har slutfört registreringen tilldelar en unik program-ID tooyour app i Azure AD. Du behöver det här värdet i hello nästa steg, så kopiera den från hello appen på sidan.

4. Från hello **inställningar** väljer **nödvändiga behörigheter**, och välj sedan **Lägg till**. Leta upp och välj hello tooDo tjänsten, Lägg till hello **åtkomst TodoListService** behörighet under **delegerade behörigheter**, och klicka sedan på **klar**.

5. Öppna i Visual Studio `App.config` i hello TodoListClient projektet och sedan ange dina konfigurationsvärden i hello `<appSettings>` avsnitt.

  * `ida:Tenant`är din Azure AD-klient – till exempel contoso.onmicrosoft.com hello namn.
  * `ida:ClientId`är hello app-ID som du kopierade från hello Azure-portalen.
  * `todo:TodoListResourceId`är hello App-ID-URI för hello tooDo lista tjänstprogram som du angav i hello Azure-portalen.

## <a name="next-steps"></a>Nästa steg
Slutligen, rensa, skapa och kör varje projekt. Om du inte redan gjort nu är hello tid toocreate en ny användare i din klient med en *. onmicrosoft.com-domän. Logga in toohello tooDo lista klienten med den aktuella användaren och lägga till vissa uppgifter toohello användarens att göra-lista.

Referens hello slutförts exemplet (utan dina konfigurationsvärden) är tillgängliga i [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Du kan nu gå vidare toomore identitet scenarier.

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
