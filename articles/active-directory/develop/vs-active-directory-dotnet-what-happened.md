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
# <a name="what-happened-toomy-mvc-project-visual-studio-azure-active-directory-connected-service"></a><span data-ttu-id="83b58-103">Vad hände toomy MVC-projektet (Visual Studio Azure Active Directory ansluten service)?</span><span class="sxs-lookup"><span data-stu-id="83b58-103">What happened toomy MVC project (Visual Studio Azure Active Directory connected service)?</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="83b58-104">Komma igång</span><span class="sxs-lookup"><span data-stu-id="83b58-104">Getting Started</span></span>](vs-active-directory-dotnet-getting-started.md)
> * [<span data-ttu-id="83b58-105">Vad hände</span><span class="sxs-lookup"><span data-stu-id="83b58-105">What Happened</span></span>](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a><span data-ttu-id="83b58-106">Referenser har lagts till</span><span class="sxs-lookup"><span data-stu-id="83b58-106">References have been added</span></span>
### <a name="nuget-package-references"></a><span data-ttu-id="83b58-107">NuGet-paketet refererar till</span><span class="sxs-lookup"><span data-stu-id="83b58-107">NuGet package references</span></span>
* <span data-ttu-id="83b58-108">**Microsoft.IdentityModel.Protocol.Extensions**</span><span class="sxs-lookup"><span data-stu-id="83b58-108">**Microsoft.IdentityModel.Protocol.Extensions**</span></span>
* <span data-ttu-id="83b58-109">**Microsoft.Owin**</span><span class="sxs-lookup"><span data-stu-id="83b58-109">**Microsoft.Owin**</span></span>
* <span data-ttu-id="83b58-110">**Microsoft.Owin.Host.SystemWeb**</span><span class="sxs-lookup"><span data-stu-id="83b58-110">**Microsoft.Owin.Host.SystemWeb**</span></span>
* <span data-ttu-id="83b58-111">**Microsoft.Owin.Security**</span><span class="sxs-lookup"><span data-stu-id="83b58-111">**Microsoft.Owin.Security**</span></span>
* <span data-ttu-id="83b58-112">**Microsoft.Owin.Security.Cookies**</span><span class="sxs-lookup"><span data-stu-id="83b58-112">**Microsoft.Owin.Security.Cookies**</span></span>
* <span data-ttu-id="83b58-113">**Microsoft.Owin.Security.OpenIdConnect**</span><span class="sxs-lookup"><span data-stu-id="83b58-113">**Microsoft.Owin.Security.OpenIdConnect**</span></span>
* <span data-ttu-id="83b58-114">**Owin**</span><span class="sxs-lookup"><span data-stu-id="83b58-114">**Owin**</span></span>
* <span data-ttu-id="83b58-115">**System.IdentityModel.Tokens.Jwt**</span><span class="sxs-lookup"><span data-stu-id="83b58-115">**System.IdentityModel.Tokens.Jwt**</span></span>

### <a name="net-references"></a><span data-ttu-id="83b58-116">.NET-referenser</span><span class="sxs-lookup"><span data-stu-id="83b58-116">.NET references</span></span>
* <span data-ttu-id="83b58-117">**Microsoft.IdentityModel.Protocol.Extensions**</span><span class="sxs-lookup"><span data-stu-id="83b58-117">**Microsoft.IdentityModel.Protocol.Extensions**</span></span>
* <span data-ttu-id="83b58-118">**Microsoft.Owin**</span><span class="sxs-lookup"><span data-stu-id="83b58-118">**Microsoft.Owin**</span></span>
* <span data-ttu-id="83b58-119">**Microsoft.Owin.Host.SystemWeb**</span><span class="sxs-lookup"><span data-stu-id="83b58-119">**Microsoft.Owin.Host.SystemWeb**</span></span>
* <span data-ttu-id="83b58-120">**Microsoft.Owin.Security**</span><span class="sxs-lookup"><span data-stu-id="83b58-120">**Microsoft.Owin.Security**</span></span>
* <span data-ttu-id="83b58-121">**Microsoft.Owin.Security.Cookies**</span><span class="sxs-lookup"><span data-stu-id="83b58-121">**Microsoft.Owin.Security.Cookies**</span></span>
* <span data-ttu-id="83b58-122">**Microsoft.Owin.Security.OpenIdConnect**</span><span class="sxs-lookup"><span data-stu-id="83b58-122">**Microsoft.Owin.Security.OpenIdConnect**</span></span>
* <span data-ttu-id="83b58-123">**Owin**</span><span class="sxs-lookup"><span data-stu-id="83b58-123">**Owin**</span></span>
* <span data-ttu-id="83b58-124">**System.IdentityModel**</span><span class="sxs-lookup"><span data-stu-id="83b58-124">**System.IdentityModel**</span></span>
* <span data-ttu-id="83b58-125">**System.IdentityModel.Tokens.Jwt**</span><span class="sxs-lookup"><span data-stu-id="83b58-125">**System.IdentityModel.Tokens.Jwt**</span></span>
* <span data-ttu-id="83b58-126">**Avsnittsgruppen**</span><span class="sxs-lookup"><span data-stu-id="83b58-126">**System.Runtime.Serialization**</span></span>

## <a name="code-has-been-added"></a><span data-ttu-id="83b58-127">Koden har lagts till</span><span class="sxs-lookup"><span data-stu-id="83b58-127">Code has been added</span></span>
### <a name="code-files-were-added-tooyour-project"></a><span data-ttu-id="83b58-128">Kodfiler har lagts till tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="83b58-128">Code files were added tooyour project</span></span>
<span data-ttu-id="83b58-129">En autentisering startklass **App_Start/Startup.Auth.cs** har lagts till tooyour projekt som innehåller Startlogik för Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="83b58-129">An authentication startup class, **App_Start/Startup.Auth.cs** was added tooyour project containing startup logic for Azure AD authentication.</span></span> <span data-ttu-id="83b58-130">Dessutom lades en domänkontrollant klass, Controllers/AccountController.cs som innehåller **SignIn()** och **SignOut()** metoder.</span><span class="sxs-lookup"><span data-stu-id="83b58-130">Also, a controller class, Controllers/AccountController.cs was added which contains **SignIn()** and **SignOut()** methods.</span></span> <span data-ttu-id="83b58-131">Slutligen en del av en vy, **Views/Shared/_LoginPartial.cshtml** har lagts till som innehåller en länk för åtgärden för inloggning/utloggning.</span><span class="sxs-lookup"><span data-stu-id="83b58-131">Finally, a partial view, **Views/Shared/_LoginPartial.cshtml** was added containing an action link for SignIn/SignOut.</span></span>

### <a name="startup-code-was-added-tooyour-project"></a><span data-ttu-id="83b58-132">Startkoden har lagts till tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="83b58-132">Startup code was added tooyour project</span></span>
<span data-ttu-id="83b58-133">Om du redan har en startklass i projektet, hello **Configuration** metoden var uppdaterade tooinclude ett anrop för**ConfigureAuth(app)**.</span><span class="sxs-lookup"><span data-stu-id="83b58-133">If you already had a Startup class in your project, hello **Configuration** method was updated tooinclude a call too**ConfigureAuth(app)**.</span></span> <span data-ttu-id="83b58-134">Annars en startklass har lagts till tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="83b58-134">Otherwise, a Startup class was added tooyour project.</span></span>

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a><span data-ttu-id="83b58-135">Din app.config eller web.config har nya konfigurationsvärden</span><span class="sxs-lookup"><span data-stu-id="83b58-135">Your app.config or web.config has new configuration values</span></span>
<span data-ttu-id="83b58-136">hello följande konfigurationsposter har lagts till.</span><span class="sxs-lookup"><span data-stu-id="83b58-136">hello following configuration entries have been added.</span></span>

    <appSettings>
        <add key="ida:ClientId" value="ClientId from hello new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="hello selected Azure AD Domain" />
        <add key="ida:TenantId" value="hello Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a><span data-ttu-id="83b58-137">En App i Azure Active Directory (AD) skapades</span><span class="sxs-lookup"><span data-stu-id="83b58-137">An Azure Active Directory (AD) App was created</span></span>
<span data-ttu-id="83b58-138">En Azure AD-program har skapats i hello-katalog som du valde i guiden hello.</span><span class="sxs-lookup"><span data-stu-id="83b58-138">An Azure AD Application was created in hello directory that you selected in hello wizard.</span></span>

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-toomy-project"></a><span data-ttu-id="83b58-139">Om jag markerad *inaktivera enskilda användarkonton autentisering*, vilka ytterligare ändringar har gjorts toomy projekt?</span><span class="sxs-lookup"><span data-stu-id="83b58-139">If I checked *disable Individual User Accounts authentication*, what additional changes were made toomy project?</span></span>
<span data-ttu-id="83b58-140">NuGet-paketet refererar till har tagits bort och filer har tagits bort och säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="83b58-140">NuGet package references were removed, and files were removed and backed up.</span></span> <span data-ttu-id="83b58-141">Beroende på hello tillstånd i ditt projekt kanske toomanually ta bort filer eller ytterligare referenser eller ändra koden efter behov.</span><span class="sxs-lookup"><span data-stu-id="83b58-141">Depending on hello state of your project, you may have toomanually remove additional references or files, or modify code as appropriate.</span></span>

### <a name="nuget-package-references-removed-for-those-present"></a><span data-ttu-id="83b58-142">NuGet-paketreferenser tas bort (för de finns)</span><span class="sxs-lookup"><span data-stu-id="83b58-142">NuGet package references removed (for those present)</span></span>
* <span data-ttu-id="83b58-143">**Microsoft.AspNet.Identity.Core**</span><span class="sxs-lookup"><span data-stu-id="83b58-143">**Microsoft.AspNet.Identity.Core**</span></span>
* <span data-ttu-id="83b58-144">**Microsoft.AspNet.Identity.EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="83b58-144">**Microsoft.AspNet.Identity.EntityFramework**</span></span>
* <span data-ttu-id="83b58-145">**Microsoft.AspNet.Identity.Owin**</span><span class="sxs-lookup"><span data-stu-id="83b58-145">**Microsoft.AspNet.Identity.Owin**</span></span>

### <a name="code-files-backed-up-and-removed-for-those-present"></a><span data-ttu-id="83b58-146">Kodfiler säkerhetskopieras och ta bort (de finns)</span><span class="sxs-lookup"><span data-stu-id="83b58-146">Code files backed up and removed (for those present)</span></span>
<span data-ttu-id="83b58-147">Var och en av följande filer säkerhetskopierades och tas bort från hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="83b58-147">Each of following files was backed up and removed from hello project.</span></span> <span data-ttu-id="83b58-148">Säkerhetskopiera filer finns i en ' säkerhetskopieringsmappen ' hello roten i hello projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="83b58-148">Backup files are located in a 'Backup' folder at hello root of hello project's directory.</span></span>

* <span data-ttu-id="83b58-149">**App_Start\IdentityConfig.CS**</span><span class="sxs-lookup"><span data-stu-id="83b58-149">**App_Start\IdentityConfig.cs**</span></span>
* <span data-ttu-id="83b58-150">**Controllers\ManageController.CS**</span><span class="sxs-lookup"><span data-stu-id="83b58-150">**Controllers\ManageController.cs**</span></span>
* <span data-ttu-id="83b58-151">**Models\IdentityModels.CS**</span><span class="sxs-lookup"><span data-stu-id="83b58-151">**Models\IdentityModels.cs**</span></span>
* <span data-ttu-id="83b58-152">**Models\ManageViewModels.CS**</span><span class="sxs-lookup"><span data-stu-id="83b58-152">**Models\ManageViewModels.cs**</span></span>

### <a name="code-files-backed-up-for-those-present"></a><span data-ttu-id="83b58-153">Kodfiler säkerhetskopieras (för de finns)</span><span class="sxs-lookup"><span data-stu-id="83b58-153">Code files backed up (for those present)</span></span>
<span data-ttu-id="83b58-154">Var och en av följande filer har säkerhetskopierats innan som ersätts.</span><span class="sxs-lookup"><span data-stu-id="83b58-154">Each of following files was backed up before being replaced.</span></span> <span data-ttu-id="83b58-155">Säkerhetskopiera filer finns i en ' säkerhetskopieringsmappen ' hello roten i hello projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="83b58-155">Backup files are located in a 'Backup' folder at hello root of hello project's directory.</span></span>

* <span data-ttu-id="83b58-156">**Startup.CS**</span><span class="sxs-lookup"><span data-stu-id="83b58-156">**Startup.cs**</span></span>
* <span data-ttu-id="83b58-157">**App_Start\Startup.auth.CS**</span><span class="sxs-lookup"><span data-stu-id="83b58-157">**App_Start\Startup.Auth.cs**</span></span>
* <span data-ttu-id="83b58-158">**Controllers\AccountController.CS**</span><span class="sxs-lookup"><span data-stu-id="83b58-158">**Controllers\AccountController.cs**</span></span>
* <span data-ttu-id="83b58-159">**Views\Shared\_LoginPartial.cshtml**</span><span class="sxs-lookup"><span data-stu-id="83b58-159">**Views\Shared\_LoginPartial.cshtml**</span></span>

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-toomy-project"></a><span data-ttu-id="83b58-160">Om jag markerad *läsa katalogdata*, vilka ytterligare ändringar har gjorts toomy projekt?</span><span class="sxs-lookup"><span data-stu-id="83b58-160">If I checked *Read directory data*, what additional changes were made toomy project?</span></span>
<span data-ttu-id="83b58-161">Ytterligare referenser har lagts till.</span><span class="sxs-lookup"><span data-stu-id="83b58-161">Additional references have been added.</span></span>

### <a name="additional-nuget-package-references"></a><span data-ttu-id="83b58-162">Ytterligare referenser för NuGet-paket</span><span class="sxs-lookup"><span data-stu-id="83b58-162">Additional NuGet package references</span></span>
* <span data-ttu-id="83b58-163">**EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="83b58-163">**EntityFramework**</span></span>
* <span data-ttu-id="83b58-164">**Microsoft.Azure.ActiveDirectory.GraphClient**</span><span class="sxs-lookup"><span data-stu-id="83b58-164">**Microsoft.Azure.ActiveDirectory.GraphClient**</span></span>
* <span data-ttu-id="83b58-165">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="83b58-165">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="83b58-166">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="83b58-166">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="83b58-167">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="83b58-167">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="83b58-168">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span><span class="sxs-lookup"><span data-stu-id="83b58-168">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span></span>
* <span data-ttu-id="83b58-169">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="83b58-169">**System.Spatial**</span></span>

### <a name="additional-net-references"></a><span data-ttu-id="83b58-170">Ytterligare referenser för .NET</span><span class="sxs-lookup"><span data-stu-id="83b58-170">Additional .NET references</span></span>
* <span data-ttu-id="83b58-171">**EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="83b58-171">**EntityFramework**</span></span>
* <span data-ttu-id="83b58-172">**EntityFramework.SqlServer**</span><span class="sxs-lookup"><span data-stu-id="83b58-172">**EntityFramework.SqlServer**</span></span>
* <span data-ttu-id="83b58-173">**Microsoft.Azure.ActiveDirectory.GraphClient**</span><span class="sxs-lookup"><span data-stu-id="83b58-173">**Microsoft.Azure.ActiveDirectory.GraphClient**</span></span>
* <span data-ttu-id="83b58-174">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="83b58-174">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="83b58-175">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="83b58-175">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="83b58-176">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="83b58-176">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="83b58-177">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span><span class="sxs-lookup"><span data-stu-id="83b58-177">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span></span>
* <span data-ttu-id="83b58-178">**Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**</span><span class="sxs-lookup"><span data-stu-id="83b58-178">**Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**</span></span>
* <span data-ttu-id="83b58-179">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="83b58-179">**System.Spatial**</span></span>

### <a name="additional-code-files-were-added-tooyour-project"></a><span data-ttu-id="83b58-180">Ytterligare kodfiler har lagts till tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="83b58-180">Additional Code files were added tooyour project</span></span>
<span data-ttu-id="83b58-181">Två filer har lagts till toosupport tokencachelagring: **Models\ADALTokenCache.cs** och **Models\ApplicationDbContext.cs**.</span><span class="sxs-lookup"><span data-stu-id="83b58-181">Two files were added toosupport token caching: **Models\ADALTokenCache.cs** and **Models\ApplicationDbContext.cs**.</span></span>  <span data-ttu-id="83b58-182">En ytterligare domänkontrollant och visa lades till tooillustrate åtkomst till användarens profilinformation som använder Azure graph API: er.</span><span class="sxs-lookup"><span data-stu-id="83b58-182">An additional controller and view were added tooillustrate accessing user profile information using Azure graph APIs.</span></span>  <span data-ttu-id="83b58-183">De här filerna är **Controllers\UserProfileController.cs** och **Views\UserProfile\Index.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="83b58-183">These files are **Controllers\UserProfileController.cs** and **Views\UserProfile\Index.cshtml**.</span></span>

### <a name="additional-startup-code-was-added-tooyour-project"></a><span data-ttu-id="83b58-184">Ytterligare startkoden har lagts till tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="83b58-184">Additional Startup code was added tooyour project</span></span>
<span data-ttu-id="83b58-185">I hello **startup.auth.cs** -fil, en ny **OpenIdConnectAuthenticationNotifications** objekt lades toohello **meddelanden** tillhör hello  **OpenIdConnectAuthenticationOptions**.</span><span class="sxs-lookup"><span data-stu-id="83b58-185">In hello **startup.auth.cs** file, a new **OpenIdConnectAuthenticationNotifications** object was added toohello **Notifications** member of hello **OpenIdConnectAuthenticationOptions**.</span></span>  <span data-ttu-id="83b58-186">Detta är tooenable ta emot hello OAuth-kod och byta ut den för en åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="83b58-186">This is tooenable receiving hello OAuth code and exchanging it for an access token.</span></span>

### <a name="additional-changes-were-made-tooyour-appconfig-or-webconfig"></a><span data-ttu-id="83b58-187">Ytterligare ändringar har gjorts tooyour app.config eller web.config</span><span class="sxs-lookup"><span data-stu-id="83b58-187">Additional changes were made tooyour app.config or web.config</span></span>
<span data-ttu-id="83b58-188">hello har följande ytterligare konfigurationsposter lagts till.</span><span class="sxs-lookup"><span data-stu-id="83b58-188">hello following additional configuration entries have been added.</span></span>

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

<span data-ttu-id="83b58-189">hello har följande avsnitt och anslutningssträngen lagts till.</span><span class="sxs-lookup"><span data-stu-id="83b58-189">hello following configuration sections and connection string have been added.</span></span>

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


### <a name="your-azure-active-directory-app-was-updated"></a><span data-ttu-id="83b58-190">Din Azure Active Directory App har uppdaterats</span><span class="sxs-lookup"><span data-stu-id="83b58-190">Your Azure Active Directory App was updated</span></span>
<span data-ttu-id="83b58-191">Din Azure Active Directory App har uppdaterats tooinclude hello *läsa katalogdata* behörighet och nyckeln skapades som användes sedan som hello *ida: ClientSecret* i hello  **Web.config** fil.</span><span class="sxs-lookup"><span data-stu-id="83b58-191">Your Azure Active Directory App was updated tooinclude hello *Read directory data* permission and an additional key was created which was then used as hello *ida:ClientSecret* in hello **web.config** file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83b58-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="83b58-192">Next steps</span></span>
- [<span data-ttu-id="83b58-193">Lär dig mer om Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="83b58-193">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

