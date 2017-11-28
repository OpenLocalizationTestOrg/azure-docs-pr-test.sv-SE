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
# <a name="what-happened-toomy-webapi-project-visual-studio-azure-active-directory-connected-service"></a><span data-ttu-id="7d893-103">Vad hände toomy WebApi-projektet (Visual Studio Azure Active Directory ansluten service)</span><span class="sxs-lookup"><span data-stu-id="7d893-103">What happened toomy WebApi project (Visual Studio Azure Active Directory connected service)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7d893-104">Komma igång</span><span class="sxs-lookup"><span data-stu-id="7d893-104">Getting Started</span></span>](vs-active-directory-webapi-getting-started.md)
> * [<span data-ttu-id="7d893-105">Vad hände</span><span class="sxs-lookup"><span data-stu-id="7d893-105">What Happened</span></span>](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a><span data-ttu-id="7d893-106">Referenser har lagts till</span><span class="sxs-lookup"><span data-stu-id="7d893-106">References have been added</span></span>
### <a name="nuget-package-references"></a><span data-ttu-id="7d893-107">NuGet-paketet refererar till</span><span class="sxs-lookup"><span data-stu-id="7d893-107">NuGet package references</span></span>
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

### <a name="net-references"></a><span data-ttu-id="7d893-108">.NET-referenser</span><span class="sxs-lookup"><span data-stu-id="7d893-108">.NET references</span></span>
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

## <a name="code-changes"></a><span data-ttu-id="7d893-109">Kodändringar</span><span class="sxs-lookup"><span data-stu-id="7d893-109">Code changes</span></span>
### <a name="code-files-were-added-tooyour-project"></a><span data-ttu-id="7d893-110">Kodfiler har lagts till tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="7d893-110">Code files were added tooyour project</span></span>
<span data-ttu-id="7d893-111">En autentisering startklass **App_Start/Startup.Auth.cs** har lagts till tooyour projekt som innehåller Startlogik för Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="7d893-111">An authentication startup class, **App_Start/Startup.Auth.cs** was added tooyour project containing startup logic for Azure AD authentication.</span></span>

### <a name="startup-code-was-added-tooyour-project"></a><span data-ttu-id="7d893-112">Startkoden har lagts till tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="7d893-112">Startup code was added tooyour project</span></span>
<span data-ttu-id="7d893-113">Om du redan har en startklass i projektet, hello **Configuration** metoden var uppdaterade tooinclude ett anrop för`ConfigureAuth(app)`.</span><span class="sxs-lookup"><span data-stu-id="7d893-113">If you already had a Startup class in your project, hello **Configuration** method was updated tooinclude a call too`ConfigureAuth(app)`.</span></span> <span data-ttu-id="7d893-114">Annars en startklass har lagts till tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="7d893-114">Otherwise, a Startup class was added tooyour project.</span></span>

### <a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a><span data-ttu-id="7d893-115">Din app.config eller web.config-fil har nya konfigurationsvärden.</span><span class="sxs-lookup"><span data-stu-id="7d893-115">Your app.config or web.config file has new configuration values.</span></span>
<span data-ttu-id="7d893-116">hello följande konfigurationsposter har lagts till.</span><span class="sxs-lookup"><span data-stu-id="7d893-116">hello following configuration entries have been added.</span></span>

```
    <appSettings>
            <add key="ida:ClientId" value="ClientId from hello new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="hello App ID Uri from hello wizard" />
    </appSettings>`
```

### <a name="an-azure-ad-app-was-created"></a><span data-ttu-id="7d893-117">En Azure AD App har skapats</span><span class="sxs-lookup"><span data-stu-id="7d893-117">An Azure AD App was created</span></span>
<span data-ttu-id="7d893-118">En Azure AD-program har skapats i hello-katalog som du valde i guiden hello.</span><span class="sxs-lookup"><span data-stu-id="7d893-118">An Azure AD Application was created in hello directory that you selected in hello wizard.</span></span>

[<span data-ttu-id="7d893-119">Lär dig mer om Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7d893-119">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-toomy-project"></a><span data-ttu-id="7d893-120">Om jag markerad *inaktivera enskilda användarkonton autentisering*, vilka ytterligare ändringar har gjorts toomy projekt?</span><span class="sxs-lookup"><span data-stu-id="7d893-120">If I checked *disable Individual User Accounts authentication*, what additional changes were made toomy project?</span></span>
<span data-ttu-id="7d893-121">NuGet-paketet refererar till har tagits bort och filer har tagits bort och säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="7d893-121">NuGet package references were removed, and files were removed and backed up.</span></span> <span data-ttu-id="7d893-122">Beroende på hello tillstånd i ditt projekt kanske toomanually ta bort filer eller ytterligare referenser eller ändra koden efter behov.</span><span class="sxs-lookup"><span data-stu-id="7d893-122">Depending on hello state of your project, you may have toomanually remove additional references or files, or modify code as appropriate.</span></span>

### <a name="nuget-package-references-removed-for-those-present"></a><span data-ttu-id="7d893-123">NuGet-paketreferenser tas bort (för de finns)</span><span class="sxs-lookup"><span data-stu-id="7d893-123">NuGet package references removed (for those present)</span></span>
* `Microsoft.AspNet.Identity.Core`
* `Microsoft.AspNet.Identity.EntityFramework`
* `Microsoft.AspNet.Identity.Owin`

### <a name="code-files-backed-up-and-removed-for-those-present"></a><span data-ttu-id="7d893-124">Kodfiler säkerhetskopieras och ta bort (de finns)</span><span class="sxs-lookup"><span data-stu-id="7d893-124">Code files backed up and removed (for those present)</span></span>
<span data-ttu-id="7d893-125">Var och en av följande filer säkerhetskopierades och tas bort från hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="7d893-125">Each of following files was backed up and removed from hello project.</span></span> <span data-ttu-id="7d893-126">Säkerhetskopiera filer finns i en ' säkerhetskopieringsmappen ' hello roten i hello projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="7d893-126">Backup files are located in a 'Backup' folder at hello root of hello project's directory.</span></span>

* `App_Start\IdentityConfig.cs`
* `Controllers\AccountController.cs`
* `Controllers\ManageController.cs`
* `Models\IdentityModels.cs`
* `Providers\ApplicationOAuthProvider.cs`

### <a name="code-files-backed-up-for-those-present"></a><span data-ttu-id="7d893-127">Kodfiler säkerhetskopieras (för de finns)</span><span class="sxs-lookup"><span data-stu-id="7d893-127">Code files backed up (for those present)</span></span>
<span data-ttu-id="7d893-128">Var och en av följande filer har säkerhetskopierats innan som ersätts.</span><span class="sxs-lookup"><span data-stu-id="7d893-128">Each of following files was backed up before being replaced.</span></span> <span data-ttu-id="7d893-129">Säkerhetskopiera filer finns i en ' säkerhetskopieringsmappen ' hello roten i hello projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="7d893-129">Backup files are located in a 'Backup' folder at hello root of hello project's directory.</span></span>

* `Startup.cs`
* `App_Start\Startup.Auth.cs`

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-toomy-project"></a><span data-ttu-id="7d893-130">Om jag markerad *läsa katalogdata*, vilka ytterligare ändringar har gjorts toomy projekt?</span><span class="sxs-lookup"><span data-stu-id="7d893-130">If I checked *Read directory data*, what additional changes were made toomy project?</span></span>
### <a name="additional-changes-were-made-tooyour-appconfig-or-webconfig"></a><span data-ttu-id="7d893-131">Ytterligare ändringar har gjorts tooyour app.config eller web.config</span><span class="sxs-lookup"><span data-stu-id="7d893-131">Additional changes were made tooyour app.config or web.config</span></span>
<span data-ttu-id="7d893-132">hello har följande ytterligare konfigurationsposter lagts till.</span><span class="sxs-lookup"><span data-stu-id="7d893-132">hello following additional configuration entries have been added.</span></span>

```
    <appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

### <a name="your-azure-active-directory-app-was-updated"></a><span data-ttu-id="7d893-133">Din Azure Active Directory App har uppdaterats</span><span class="sxs-lookup"><span data-stu-id="7d893-133">Your Azure Active Directory App was updated</span></span>
<span data-ttu-id="7d893-134">Din Azure Active Directory App har uppdaterats tooinclude hello *läsa katalogdata* behörighet och nyckeln skapades som användes sedan som hello *ida: lösenord* i hello `web.config` fil.</span><span class="sxs-lookup"><span data-stu-id="7d893-134">Your Azure Active Directory App was updated tooinclude hello *Read directory data* permission and an additional key was created which was then used as hello *ida:Password* in hello `web.config` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d893-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7d893-135">Next steps</span></span>
- [<span data-ttu-id="7d893-136">Lär dig mer om Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7d893-136">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

