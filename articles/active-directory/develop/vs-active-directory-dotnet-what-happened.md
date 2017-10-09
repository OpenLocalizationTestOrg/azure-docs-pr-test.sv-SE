---
title: "aaaChanges görs tooa MVC projektet när du ansluter tooAzure AD | Microsoft Docs"
description: "Beskriver vad som händer tooyour MVC-projektet när du ansluter tooAzure AD med hjälp av Visual Studio anslutna tjänster"
services: active-directory
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 8b24adde-547e-4ffe-824a-2029ba210216
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 5e6d4ce5331eacca5fc83429017ae454fadcc8e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-happened-toomy-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Vad hände toomy MVC-projektet (Visual Studio Azure Active Directory ansluten service)?
> [!div class="op_single_selector"]
> * [Komma igång](vs-active-directory-dotnet-getting-started.md)
> * [Vad hände](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a>Referenser har lagts till
### <a name="nuget-package-references"></a>NuGet-paketet refererar till
* **Microsoft.IdentityModel.Protocol.Extensions**
* **Microsoft.Owin**
* **Microsoft.Owin.Host.SystemWeb**
* **Microsoft.Owin.Security**
* **Microsoft.Owin.Security.Cookies**
* **Microsoft.Owin.Security.OpenIdConnect**
* **Owin**
* **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>.NET-referenser
* **Microsoft.IdentityModel.Protocol.Extensions**
* **Microsoft.Owin**
* **Microsoft.Owin.Host.SystemWeb**
* **Microsoft.Owin.Security**
* **Microsoft.Owin.Security.Cookies**
* **Microsoft.Owin.Security.OpenIdConnect**
* **Owin**
* **System.IdentityModel**
* **System.IdentityModel.Tokens.Jwt**
* **Avsnittsgruppen**

## <a name="code-has-been-added"></a>Koden har lagts till
### <a name="code-files-were-added-tooyour-project"></a>Kodfiler har lagts till tooyour projekt
En autentisering startklass **App_Start/Startup.Auth.cs** har lagts till tooyour projekt som innehåller Startlogik för Azure AD-autentisering. Dessutom lades en domänkontrollant klass, Controllers/AccountController.cs som innehåller **SignIn()** och **SignOut()** metoder. Slutligen en del av en vy, **Views/Shared/_LoginPartial.cshtml** har lagts till som innehåller en länk för åtgärden för inloggning/utloggning.

### <a name="startup-code-was-added-tooyour-project"></a>Startkoden har lagts till tooyour projekt
Om du redan har en startklass i projektet, hello **Configuration** metoden var uppdaterade tooinclude ett anrop för**ConfigureAuth(app)**. Annars en startklass har lagts till tooyour projekt.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>Din app.config eller web.config har nya konfigurationsvärden
hello följande konfigurationsposter har lagts till.

    <appSettings>
        <add key="ida:ClientId" value="ClientId from hello new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="hello selected Azure AD Domain" />
        <add key="ida:TenantId" value="hello Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>En App i Azure Active Directory (AD) skapades
En Azure AD-program har skapats i hello-katalog som du valde i guiden hello.

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-toomy-project"></a>Om jag markerad *inaktivera enskilda användarkonton autentisering*, vilka ytterligare ändringar har gjorts toomy projekt?
NuGet-paketet refererar till har tagits bort och filer har tagits bort och säkerhetskopieras. Beroende på hello tillstånd i ditt projekt kanske toomanually ta bort filer eller ytterligare referenser eller ändra koden efter behov.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet-paketreferenser tas bort (för de finns)
* **Microsoft.AspNet.Identity.Core**
* **Microsoft.AspNet.Identity.EntityFramework**
* **Microsoft.AspNet.Identity.Owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Kodfiler säkerhetskopieras och ta bort (de finns)
Var och en av följande filer säkerhetskopierades och tas bort från hello-projekt. Säkerhetskopiera filer finns i en ' säkerhetskopieringsmappen ' hello roten i hello projektkatalogen.

* **App_Start\IdentityConfig.CS**
* **Controllers\ManageController.CS**
* **Models\IdentityModels.CS**
* **Models\ManageViewModels.CS**

### <a name="code-files-backed-up-for-those-present"></a>Kodfiler säkerhetskopieras (för de finns)
Var och en av följande filer har säkerhetskopierats innan som ersätts. Säkerhetskopiera filer finns i en ' säkerhetskopieringsmappen ' hello roten i hello projektkatalogen.

* **Startup.CS**
* **App_Start\Startup.auth.CS**
* **Controllers\AccountController.CS**
* **Views\Shared\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-toomy-project"></a>Om jag markerad *läsa katalogdata*, vilka ytterligare ändringar har gjorts toomy projekt?
Ytterligare referenser har lagts till.

### <a name="additional-nuget-package-references"></a>Ytterligare referenser för NuGet-paket
* **EntityFramework**
* **Microsoft.Azure.ActiveDirectory.GraphClient**
* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.IdentityModel.Clients.ActiveDirectory**
* **System.Spatial**

### <a name="additional-net-references"></a>Ytterligare referenser för .NET
* **EntityFramework**
* **EntityFramework.SqlServer**
* **Microsoft.Azure.ActiveDirectory.GraphClient**
* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.IdentityModel.Clients.ActiveDirectory**
* **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
* **System.Spatial**

### <a name="additional-code-files-were-added-tooyour-project"></a>Ytterligare kodfiler har lagts till tooyour projekt
Två filer har lagts till toosupport tokencachelagring: **Models\ADALTokenCache.cs** och **Models\ApplicationDbContext.cs**.  En ytterligare domänkontrollant och visa lades till tooillustrate åtkomst till användarens profilinformation som använder Azure graph API: er.  De här filerna är **Controllers\UserProfileController.cs** och **Views\UserProfile\Index.cshtml**.

### <a name="additional-startup-code-was-added-tooyour-project"></a>Ytterligare startkoden har lagts till tooyour projekt
I hello **startup.auth.cs** -fil, en ny **OpenIdConnectAuthenticationNotifications** objekt lades toohello **meddelanden** tillhör hello  **OpenIdConnectAuthenticationOptions**.  Detta är tooenable ta emot hello OAuth-kod och byta ut den för en åtkomst-token.

### <a name="additional-changes-were-made-tooyour-appconfig-or-webconfig"></a>Ytterligare ändringar har gjorts tooyour app.config eller web.config
hello har följande ytterligare konfigurationsposter lagts till.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

hello har följande avsnitt och anslutningssträngen lagts till.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


### <a name="your-azure-active-directory-app-was-updated"></a>Din Azure Active Directory App har uppdaterats
Din Azure Active Directory App har uppdaterats tooinclude hello *läsa katalogdata* behörighet och nyckeln skapades som användes sedan som hello *ida: ClientSecret* i hello  **Web.config** fil.

## <a name="next-steps"></a>Nästa steg
- [Lär dig mer om Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

