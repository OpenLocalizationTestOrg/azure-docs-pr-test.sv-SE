---
title: "Ändringar i ett projekt för WebApi när du ansluter till Azure AD | Microsoft Docs"
description: "Beskriver vad som händer i projektet WebApi när du ansluter till Azure AD med hjälp av Visual Studio"
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
ms.openlocfilehash: 086e5a9622cad681cd282345d97e0c28ee7de2fa
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a><span data-ttu-id="e0c1b-103">Vad hände med min WebApi-projektet (Visual Studio Azure Active Directory ansluten service)</span><span class="sxs-lookup"><span data-stu-id="e0c1b-103">What happened to my WebApi project (Visual Studio Azure Active Directory connected service)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e0c1b-104">Komma igång</span><span class="sxs-lookup"><span data-stu-id="e0c1b-104">Getting Started</span></span>](vs-active-directory-webapi-getting-started.md)
> * [<span data-ttu-id="e0c1b-105">Vad hände</span><span class="sxs-lookup"><span data-stu-id="e0c1b-105">What Happened</span></span>](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a><span data-ttu-id="e0c1b-106">Referenser har lagts till</span><span class="sxs-lookup"><span data-stu-id="e0c1b-106">References have been added</span></span>
### <a name="nuget-package-references"></a><span data-ttu-id="e0c1b-107">NuGet-paketet refererar till</span><span class="sxs-lookup"><span data-stu-id="e0c1b-107">NuGet package references</span></span>
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

### <a name="net-references"></a><span data-ttu-id="e0c1b-108">.NET-referenser</span><span class="sxs-lookup"><span data-stu-id="e0c1b-108">.NET references</span></span>
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

## <a name="code-changes"></a><span data-ttu-id="e0c1b-109">Kodändringar</span><span class="sxs-lookup"><span data-stu-id="e0c1b-109">Code changes</span></span>
### <a name="code-files-were-added-to-your-project"></a><span data-ttu-id="e0c1b-110">Kodfiler har lagts till i ditt projekt</span><span class="sxs-lookup"><span data-stu-id="e0c1b-110">Code files were added to your project</span></span>
<span data-ttu-id="e0c1b-111">En autentisering startklass **App_Start/Startup.Auth.cs** har lagts till i ditt projekt som innehåller Startlogik för Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-111">An authentication startup class, **App_Start/Startup.Auth.cs** was added to your project containing startup logic for Azure AD authentication.</span></span>

### <a name="startup-code-was-added-to-your-project"></a><span data-ttu-id="e0c1b-112">Startkoden har lagts till i ditt projekt</span><span class="sxs-lookup"><span data-stu-id="e0c1b-112">Startup code was added to your project</span></span>
<span data-ttu-id="e0c1b-113">Om du redan har en startklass i projektet, den **Configuration** metod har uppdaterats med ett anrop till `ConfigureAuth(app)`.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-113">If you already had a Startup class in your project, the **Configuration** method was updated to include a call to `ConfigureAuth(app)`.</span></span> <span data-ttu-id="e0c1b-114">Annars en startklass har lagts till i projektet.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-114">Otherwise, a Startup class was added to your project.</span></span>

### <a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a><span data-ttu-id="e0c1b-115">Din app.config eller web.config-fil har nya konfigurationsvärden.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-115">Your app.config or web.config file has new configuration values.</span></span>
<span data-ttu-id="e0c1b-116">Följande konfigurationsposter har lagts till.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-116">The following configuration entries have been added.</span></span>

```
    <appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

### <a name="an-azure-ad-app-was-created"></a><span data-ttu-id="e0c1b-117">En Azure AD App har skapats</span><span class="sxs-lookup"><span data-stu-id="e0c1b-117">An Azure AD App was created</span></span>
<span data-ttu-id="e0c1b-118">En Azure AD-program har skapats i den katalog som du valde i guiden.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-118">An Azure AD Application was created in the directory that you selected in the wizard.</span></span>

[<span data-ttu-id="e0c1b-119">Lär dig mer om Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e0c1b-119">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a><span data-ttu-id="e0c1b-120">Om jag markerad *inaktivera enskilda användarkonton autentisering*, vilka ytterligare ändringar har gjorts i projektet?</span><span class="sxs-lookup"><span data-stu-id="e0c1b-120">If I checked *disable Individual User Accounts authentication*, what additional changes were made to my project?</span></span>
<span data-ttu-id="e0c1b-121">NuGet-paketet refererar till har tagits bort och filer har tagits bort och säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-121">NuGet package references were removed, and files were removed and backed up.</span></span> <span data-ttu-id="e0c1b-122">Beroende på ditt projekt status kan du behöva manuellt ta bort filer eller ytterligare referenser eller ändra koden efter behov.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-122">Depending on the state of your project, you may have to manually remove additional references or files, or modify code as appropriate.</span></span>

### <a name="nuget-package-references-removed-for-those-present"></a><span data-ttu-id="e0c1b-123">NuGet-paketreferenser tas bort (för de finns)</span><span class="sxs-lookup"><span data-stu-id="e0c1b-123">NuGet package references removed (for those present)</span></span>
* `Microsoft.AspNet.Identity.Core`
* `Microsoft.AspNet.Identity.EntityFramework`
* `Microsoft.AspNet.Identity.Owin`

### <a name="code-files-backed-up-and-removed-for-those-present"></a><span data-ttu-id="e0c1b-124">Kodfiler säkerhetskopieras och ta bort (de finns)</span><span class="sxs-lookup"><span data-stu-id="e0c1b-124">Code files backed up and removed (for those present)</span></span>
<span data-ttu-id="e0c1b-125">Var och en av följande filer har säkerhetskopierats och tas bort från projektet.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-125">Each of following files was backed up and removed from the project.</span></span> <span data-ttu-id="e0c1b-126">Säkerhetskopiera filer finns i en ”Backup”-mapp i roten på den projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-126">Backup files are located in a 'Backup' folder at the root of the project's directory.</span></span>

* `App_Start\IdentityConfig.cs`
* `Controllers\AccountController.cs`
* `Controllers\ManageController.cs`
* `Models\IdentityModels.cs`
* `Providers\ApplicationOAuthProvider.cs`

### <a name="code-files-backed-up-for-those-present"></a><span data-ttu-id="e0c1b-127">Kodfiler säkerhetskopieras (för de finns)</span><span class="sxs-lookup"><span data-stu-id="e0c1b-127">Code files backed up (for those present)</span></span>
<span data-ttu-id="e0c1b-128">Var och en av följande filer har säkerhetskopierats innan som ersätts.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-128">Each of following files was backed up before being replaced.</span></span> <span data-ttu-id="e0c1b-129">Säkerhetskopiera filer finns i en ”Backup”-mapp i roten på den projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-129">Backup files are located in a 'Backup' folder at the root of the project's directory.</span></span>

* `Startup.cs`
* `App_Start\Startup.Auth.cs`

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a><span data-ttu-id="e0c1b-130">Om jag markerad *läsa katalogdata*, vilka ytterligare ändringar har gjorts i projektet?</span><span class="sxs-lookup"><span data-stu-id="e0c1b-130">If I checked *Read directory data*, what additional changes were made to my project?</span></span>
### <a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a><span data-ttu-id="e0c1b-131">Ytterligare ändringar har gjorts till web.config eller app.config</span><span class="sxs-lookup"><span data-stu-id="e0c1b-131">Additional changes were made to your app.config or web.config</span></span>
<span data-ttu-id="e0c1b-132">Följande ytterligare konfigurationsposter har lagts till.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-132">The following additional configuration entries have been added.</span></span>

```
    <appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

### <a name="your-azure-active-directory-app-was-updated"></a><span data-ttu-id="e0c1b-133">Din Azure Active Directory App har uppdaterats</span><span class="sxs-lookup"><span data-stu-id="e0c1b-133">Your Azure Active Directory App was updated</span></span>
<span data-ttu-id="e0c1b-134">Din Azure Active Directory App har uppdaterats med den *läsa katalogdata* behörighet och nyckeln skapades användes sedan som den *ida: lösenord* i den `web.config` filen.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-134">Your Azure Active Directory App was updated to include the *Read directory data* permission and an additional key was created which was then used as the *ida:Password* in the `web.config` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0c1b-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e0c1b-135">Next steps</span></span>
- [<span data-ttu-id="e0c1b-136">Lär dig mer om Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e0c1b-136">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

