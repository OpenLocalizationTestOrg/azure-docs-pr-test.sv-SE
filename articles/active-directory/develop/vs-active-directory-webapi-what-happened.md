---
title: "aaaChanges görs tooa WebApi projekt när du ansluter tooAzure AD | Microsoft Docs"
description: "Beskriver vad som händer tooyour WebApi projekt när du ansluter tooAzure AD med hjälp av Visual Studio"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 57630aee-26a2-4326-9dbb-ea2a66daa8b0
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 1ea77b6c75b2dc273219fa6c43f02c7a7c5312ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-happened-toomy-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Vad hände toomy WebApi-projektet (Visual Studio Azure Active Directory ansluten service)
> [!div class="op_single_selector"]
> * [Komma igång](vs-active-directory-webapi-getting-started.md)
> * [Vad hände](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a>Referenser har lagts till
### <a name="nuget-package-references"></a>NuGet-paketet refererar till
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

### <a name="net-references"></a>.NET-referenser
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

## <a name="code-changes"></a>Kodändringar
### <a name="code-files-were-added-tooyour-project"></a>Kodfiler har lagts till tooyour projekt
En autentisering startklass **App_Start/Startup.Auth.cs** har lagts till tooyour projekt som innehåller Startlogik för Azure AD-autentisering.

### <a name="startup-code-was-added-tooyour-project"></a>Startkoden har lagts till tooyour projekt
Om du redan har en startklass i projektet, hello **Configuration** metoden var uppdaterade tooinclude ett anrop för`ConfigureAuth(app)`. Annars en startklass har lagts till tooyour projekt.

### <a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>Din app.config eller web.config-fil har nya konfigurationsvärden.
hello följande konfigurationsposter har lagts till.

```
    <appSettings>
            <add key="ida:ClientId" value="ClientId from hello new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="hello App ID Uri from hello wizard" />
    </appSettings>`
```

### <a name="an-azure-ad-app-was-created"></a>En Azure AD App har skapats
En Azure AD-program har skapats i hello-katalog som du valde i guiden hello.

[Lär dig mer om Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-toomy-project"></a>Om jag markerad *inaktivera enskilda användarkonton autentisering*, vilka ytterligare ändringar har gjorts toomy projekt?
NuGet-paketet refererar till har tagits bort och filer har tagits bort och säkerhetskopieras. Beroende på hello tillstånd i ditt projekt kanske toomanually ta bort filer eller ytterligare referenser eller ändra koden efter behov.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet-paketreferenser tas bort (för de finns)
* `Microsoft.AspNet.Identity.Core`
* `Microsoft.AspNet.Identity.EntityFramework`
* `Microsoft.AspNet.Identity.Owin`

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Kodfiler säkerhetskopieras och ta bort (de finns)
Var och en av följande filer säkerhetskopierades och tas bort från hello-projekt. Säkerhetskopiera filer finns i en ' säkerhetskopieringsmappen ' hello roten i hello projektkatalogen.

* `App_Start\IdentityConfig.cs`
* `Controllers\AccountController.cs`
* `Controllers\ManageController.cs`
* `Models\IdentityModels.cs`
* `Providers\ApplicationOAuthProvider.cs`

### <a name="code-files-backed-up-for-those-present"></a>Kodfiler säkerhetskopieras (för de finns)
Var och en av följande filer har säkerhetskopierats innan som ersätts. Säkerhetskopiera filer finns i en ' säkerhetskopieringsmappen ' hello roten i hello projektkatalogen.

* `Startup.cs`
* `App_Start\Startup.Auth.cs`

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-toomy-project"></a>Om jag markerad *läsa katalogdata*, vilka ytterligare ändringar har gjorts toomy projekt?
### <a name="additional-changes-were-made-tooyour-appconfig-or-webconfig"></a>Ytterligare ändringar har gjorts tooyour app.config eller web.config
hello har följande ytterligare konfigurationsposter lagts till.

```
    <appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

### <a name="your-azure-active-directory-app-was-updated"></a>Din Azure Active Directory App har uppdaterats
Din Azure Active Directory App har uppdaterats tooinclude hello *läsa katalogdata* behörighet och nyckeln skapades som användes sedan som hello *ida: lösenord* i hello `web.config` fil.

## <a name="next-steps"></a>Nästa steg
- [Lär dig mer om Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

