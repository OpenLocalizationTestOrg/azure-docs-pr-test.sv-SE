---
title: "Ändringar i ett projekt med MVC när du ansluter till Azure AD | Microsoft Docs"
description: "Beskriver vad som händer i projektet MVC när du ansluter till Azure AD med hjälp av Visual Studio anslutna tjänster"
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
ms.openlocfilehash: 095411a7fc854f4dce11921adb0f57c5389a8e13
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a><span data-ttu-id="217ff-103">Vad hände med min MVC-projektet (Visual Studio Azure Active Directory ansluten service)?</span><span class="sxs-lookup"><span data-stu-id="217ff-103">What happened to my MVC project (Visual Studio Azure Active Directory connected service)?</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="217ff-104">Komma igång</span><span class="sxs-lookup"><span data-stu-id="217ff-104">Getting Started</span></span>](vs-active-directory-dotnet-getting-started.md)
> * [<span data-ttu-id="217ff-105">Vad hände</span><span class="sxs-lookup"><span data-stu-id="217ff-105">What Happened</span></span>](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a><span data-ttu-id="217ff-106">Referenser har lagts till</span><span class="sxs-lookup"><span data-stu-id="217ff-106">References have been added</span></span>
### <a name="nuget-package-references"></a><span data-ttu-id="217ff-107">NuGet-paketet refererar till</span><span class="sxs-lookup"><span data-stu-id="217ff-107">NuGet package references</span></span>
* <span data-ttu-id="217ff-108">**Microsoft.IdentityModel.Protocol.Extensions**</span><span class="sxs-lookup"><span data-stu-id="217ff-108">**Microsoft.IdentityModel.Protocol.Extensions**</span></span>
* <span data-ttu-id="217ff-109">**Microsoft.Owin**</span><span class="sxs-lookup"><span data-stu-id="217ff-109">**Microsoft.Owin**</span></span>
* <span data-ttu-id="217ff-110">**Microsoft.Owin.Host.SystemWeb**</span><span class="sxs-lookup"><span data-stu-id="217ff-110">**Microsoft.Owin.Host.SystemWeb**</span></span>
* <span data-ttu-id="217ff-111">**Microsoft.Owin.Security**</span><span class="sxs-lookup"><span data-stu-id="217ff-111">**Microsoft.Owin.Security**</span></span>
* <span data-ttu-id="217ff-112">**Microsoft.Owin.Security.Cookies**</span><span class="sxs-lookup"><span data-stu-id="217ff-112">**Microsoft.Owin.Security.Cookies**</span></span>
* <span data-ttu-id="217ff-113">**Microsoft.Owin.Security.OpenIdConnect**</span><span class="sxs-lookup"><span data-stu-id="217ff-113">**Microsoft.Owin.Security.OpenIdConnect**</span></span>
* <span data-ttu-id="217ff-114">**Owin**</span><span class="sxs-lookup"><span data-stu-id="217ff-114">**Owin**</span></span>
* <span data-ttu-id="217ff-115">**System.IdentityModel.Tokens.Jwt**</span><span class="sxs-lookup"><span data-stu-id="217ff-115">**System.IdentityModel.Tokens.Jwt**</span></span>

### <a name="net-references"></a><span data-ttu-id="217ff-116">.NET-referenser</span><span class="sxs-lookup"><span data-stu-id="217ff-116">.NET references</span></span>
* <span data-ttu-id="217ff-117">**Microsoft.IdentityModel.Protocol.Extensions**</span><span class="sxs-lookup"><span data-stu-id="217ff-117">**Microsoft.IdentityModel.Protocol.Extensions**</span></span>
* <span data-ttu-id="217ff-118">**Microsoft.Owin**</span><span class="sxs-lookup"><span data-stu-id="217ff-118">**Microsoft.Owin**</span></span>
* <span data-ttu-id="217ff-119">**Microsoft.Owin.Host.SystemWeb**</span><span class="sxs-lookup"><span data-stu-id="217ff-119">**Microsoft.Owin.Host.SystemWeb**</span></span>
* <span data-ttu-id="217ff-120">**Microsoft.Owin.Security**</span><span class="sxs-lookup"><span data-stu-id="217ff-120">**Microsoft.Owin.Security**</span></span>
* <span data-ttu-id="217ff-121">**Microsoft.Owin.Security.Cookies**</span><span class="sxs-lookup"><span data-stu-id="217ff-121">**Microsoft.Owin.Security.Cookies**</span></span>
* <span data-ttu-id="217ff-122">**Microsoft.Owin.Security.OpenIdConnect**</span><span class="sxs-lookup"><span data-stu-id="217ff-122">**Microsoft.Owin.Security.OpenIdConnect**</span></span>
* <span data-ttu-id="217ff-123">**Owin**</span><span class="sxs-lookup"><span data-stu-id="217ff-123">**Owin**</span></span>
* <span data-ttu-id="217ff-124">**System.IdentityModel**</span><span class="sxs-lookup"><span data-stu-id="217ff-124">**System.IdentityModel**</span></span>
* <span data-ttu-id="217ff-125">**System.IdentityModel.Tokens.Jwt**</span><span class="sxs-lookup"><span data-stu-id="217ff-125">**System.IdentityModel.Tokens.Jwt**</span></span>
* <span data-ttu-id="217ff-126">**Avsnittsgruppen**</span><span class="sxs-lookup"><span data-stu-id="217ff-126">**System.Runtime.Serialization**</span></span>

## <a name="code-has-been-added"></a><span data-ttu-id="217ff-127">Koden har lagts till</span><span class="sxs-lookup"><span data-stu-id="217ff-127">Code has been added</span></span>
### <a name="code-files-were-added-to-your-project"></a><span data-ttu-id="217ff-128">Kodfiler har lagts till i ditt projekt</span><span class="sxs-lookup"><span data-stu-id="217ff-128">Code files were added to your project</span></span>
<span data-ttu-id="217ff-129">En autentisering startklass **App_Start/Startup.Auth.cs** har lagts till i ditt projekt som innehåller Startlogik för Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="217ff-129">An authentication startup class, **App_Start/Startup.Auth.cs** was added to your project containing startup logic for Azure AD authentication.</span></span> <span data-ttu-id="217ff-130">Dessutom lades en domänkontrollant klass, Controllers/AccountController.cs som innehåller **SignIn()** och **SignOut()** metoder.</span><span class="sxs-lookup"><span data-stu-id="217ff-130">Also, a controller class, Controllers/AccountController.cs was added which contains **SignIn()** and **SignOut()** methods.</span></span> <span data-ttu-id="217ff-131">Slutligen en del av en vy, **Views/Shared/_LoginPartial.cshtml** har lagts till som innehåller en länk för åtgärden för inloggning/utloggning.</span><span class="sxs-lookup"><span data-stu-id="217ff-131">Finally, a partial view, **Views/Shared/_LoginPartial.cshtml** was added containing an action link for SignIn/SignOut.</span></span>

### <a name="startup-code-was-added-to-your-project"></a><span data-ttu-id="217ff-132">Startkoden har lagts till i ditt projekt</span><span class="sxs-lookup"><span data-stu-id="217ff-132">Startup code was added to your project</span></span>
<span data-ttu-id="217ff-133">Om du redan har en startklass i projektet, den **Configuration** metod har uppdaterats med ett anrop till **ConfigureAuth(app)**.</span><span class="sxs-lookup"><span data-stu-id="217ff-133">If you already had a Startup class in your project, the **Configuration** method was updated to include a call to **ConfigureAuth(app)**.</span></span> <span data-ttu-id="217ff-134">Annars en startklass har lagts till i projektet.</span><span class="sxs-lookup"><span data-stu-id="217ff-134">Otherwise, a Startup class was added to your project.</span></span>

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a><span data-ttu-id="217ff-135">Din app.config eller web.config har nya konfigurationsvärden</span><span class="sxs-lookup"><span data-stu-id="217ff-135">Your app.config or web.config has new configuration values</span></span>
<span data-ttu-id="217ff-136">Följande konfigurationsposter har lagts till.</span><span class="sxs-lookup"><span data-stu-id="217ff-136">The following configuration entries have been added.</span></span>

    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a><span data-ttu-id="217ff-137">En App i Azure Active Directory (AD) skapades</span><span class="sxs-lookup"><span data-stu-id="217ff-137">An Azure Active Directory (AD) App was created</span></span>
<span data-ttu-id="217ff-138">En Azure AD-program har skapats i den katalog som du valde i guiden.</span><span class="sxs-lookup"><span data-stu-id="217ff-138">An Azure AD Application was created in the directory that you selected in the wizard.</span></span>

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a><span data-ttu-id="217ff-139">Om jag markerad *inaktivera enskilda användarkonton autentisering*, vilka ytterligare ändringar har gjorts i projektet?</span><span class="sxs-lookup"><span data-stu-id="217ff-139">If I checked *disable Individual User Accounts authentication*, what additional changes were made to my project?</span></span>
<span data-ttu-id="217ff-140">NuGet-paketet refererar till har tagits bort och filer har tagits bort och säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="217ff-140">NuGet package references were removed, and files were removed and backed up.</span></span> <span data-ttu-id="217ff-141">Beroende på ditt projekt status kan du behöva manuellt ta bort filer eller ytterligare referenser eller ändra koden efter behov.</span><span class="sxs-lookup"><span data-stu-id="217ff-141">Depending on the state of your project, you may have to manually remove additional references or files, or modify code as appropriate.</span></span>

### <a name="nuget-package-references-removed-for-those-present"></a><span data-ttu-id="217ff-142">NuGet-paketreferenser tas bort (för de finns)</span><span class="sxs-lookup"><span data-stu-id="217ff-142">NuGet package references removed (for those present)</span></span>
* <span data-ttu-id="217ff-143">**Microsoft.AspNet.Identity.Core**</span><span class="sxs-lookup"><span data-stu-id="217ff-143">**Microsoft.AspNet.Identity.Core**</span></span>
* <span data-ttu-id="217ff-144">**Microsoft.AspNet.Identity.EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="217ff-144">**Microsoft.AspNet.Identity.EntityFramework**</span></span>
* <span data-ttu-id="217ff-145">**Microsoft.AspNet.Identity.Owin**</span><span class="sxs-lookup"><span data-stu-id="217ff-145">**Microsoft.AspNet.Identity.Owin**</span></span>

### <a name="code-files-backed-up-and-removed-for-those-present"></a><span data-ttu-id="217ff-146">Kodfiler säkerhetskopieras och ta bort (de finns)</span><span class="sxs-lookup"><span data-stu-id="217ff-146">Code files backed up and removed (for those present)</span></span>
<span data-ttu-id="217ff-147">Var och en av följande filer har säkerhetskopierats och tas bort från projektet.</span><span class="sxs-lookup"><span data-stu-id="217ff-147">Each of following files was backed up and removed from the project.</span></span> <span data-ttu-id="217ff-148">Säkerhetskopiera filer finns i en ”Backup”-mapp i roten på den projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="217ff-148">Backup files are located in a 'Backup' folder at the root of the project's directory.</span></span>

* <span data-ttu-id="217ff-149">**App_Start\IdentityConfig.CS**</span><span class="sxs-lookup"><span data-stu-id="217ff-149">**App_Start\IdentityConfig.cs**</span></span>
* <span data-ttu-id="217ff-150">**Controllers\ManageController.CS**</span><span class="sxs-lookup"><span data-stu-id="217ff-150">**Controllers\ManageController.cs**</span></span>
* <span data-ttu-id="217ff-151">**Models\IdentityModels.CS**</span><span class="sxs-lookup"><span data-stu-id="217ff-151">**Models\IdentityModels.cs**</span></span>
* <span data-ttu-id="217ff-152">**Models\ManageViewModels.CS**</span><span class="sxs-lookup"><span data-stu-id="217ff-152">**Models\ManageViewModels.cs**</span></span>

### <a name="code-files-backed-up-for-those-present"></a><span data-ttu-id="217ff-153">Kodfiler säkerhetskopieras (för de finns)</span><span class="sxs-lookup"><span data-stu-id="217ff-153">Code files backed up (for those present)</span></span>
<span data-ttu-id="217ff-154">Var och en av följande filer har säkerhetskopierats innan som ersätts.</span><span class="sxs-lookup"><span data-stu-id="217ff-154">Each of following files was backed up before being replaced.</span></span> <span data-ttu-id="217ff-155">Säkerhetskopiera filer finns i en ”Backup”-mapp i roten på den projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="217ff-155">Backup files are located in a 'Backup' folder at the root of the project's directory.</span></span>

* <span data-ttu-id="217ff-156">**Startup.CS**</span><span class="sxs-lookup"><span data-stu-id="217ff-156">**Startup.cs**</span></span>
* <span data-ttu-id="217ff-157">**App_Start\Startup.auth.CS**</span><span class="sxs-lookup"><span data-stu-id="217ff-157">**App_Start\Startup.Auth.cs**</span></span>
* <span data-ttu-id="217ff-158">**Controllers\AccountController.CS**</span><span class="sxs-lookup"><span data-stu-id="217ff-158">**Controllers\AccountController.cs**</span></span>
* <span data-ttu-id="217ff-159">**Views\Shared\_LoginPartial.cshtml**</span><span class="sxs-lookup"><span data-stu-id="217ff-159">**Views\Shared\_LoginPartial.cshtml**</span></span>

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a><span data-ttu-id="217ff-160">Om jag markerad *läsa katalogdata*, vilka ytterligare ändringar har gjorts i projektet?</span><span class="sxs-lookup"><span data-stu-id="217ff-160">If I checked *Read directory data*, what additional changes were made to my project?</span></span>
<span data-ttu-id="217ff-161">Ytterligare referenser har lagts till.</span><span class="sxs-lookup"><span data-stu-id="217ff-161">Additional references have been added.</span></span>

### <a name="additional-nuget-package-references"></a><span data-ttu-id="217ff-162">Ytterligare referenser för NuGet-paket</span><span class="sxs-lookup"><span data-stu-id="217ff-162">Additional NuGet package references</span></span>
* <span data-ttu-id="217ff-163">**EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="217ff-163">**EntityFramework**</span></span>
* <span data-ttu-id="217ff-164">**Microsoft.Azure.ActiveDirectory.GraphClient**</span><span class="sxs-lookup"><span data-stu-id="217ff-164">**Microsoft.Azure.ActiveDirectory.GraphClient**</span></span>
* <span data-ttu-id="217ff-165">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="217ff-165">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="217ff-166">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="217ff-166">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="217ff-167">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="217ff-167">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="217ff-168">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span><span class="sxs-lookup"><span data-stu-id="217ff-168">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span></span>
* <span data-ttu-id="217ff-169">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="217ff-169">**System.Spatial**</span></span>

### <a name="additional-net-references"></a><span data-ttu-id="217ff-170">Ytterligare referenser för .NET</span><span class="sxs-lookup"><span data-stu-id="217ff-170">Additional .NET references</span></span>
* <span data-ttu-id="217ff-171">**EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="217ff-171">**EntityFramework**</span></span>
* <span data-ttu-id="217ff-172">**EntityFramework.SqlServer**</span><span class="sxs-lookup"><span data-stu-id="217ff-172">**EntityFramework.SqlServer**</span></span>
* <span data-ttu-id="217ff-173">**Microsoft.Azure.ActiveDirectory.GraphClient**</span><span class="sxs-lookup"><span data-stu-id="217ff-173">**Microsoft.Azure.ActiveDirectory.GraphClient**</span></span>
* <span data-ttu-id="217ff-174">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="217ff-174">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="217ff-175">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="217ff-175">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="217ff-176">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="217ff-176">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="217ff-177">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span><span class="sxs-lookup"><span data-stu-id="217ff-177">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span></span>
* <span data-ttu-id="217ff-178">**Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**</span><span class="sxs-lookup"><span data-stu-id="217ff-178">**Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**</span></span>
* <span data-ttu-id="217ff-179">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="217ff-179">**System.Spatial**</span></span>

### <a name="additional-code-files-were-added-to-your-project"></a><span data-ttu-id="217ff-180">Ytterligare kodfiler har lagts till i ditt projekt</span><span class="sxs-lookup"><span data-stu-id="217ff-180">Additional Code files were added to your project</span></span>
<span data-ttu-id="217ff-181">Två filer har lagts till stöd för tokencachelagring: **Models\ADALTokenCache.cs** och **Models\ApplicationDbContext.cs**.</span><span class="sxs-lookup"><span data-stu-id="217ff-181">Two files were added to support token caching: **Models\ADALTokenCache.cs** and **Models\ApplicationDbContext.cs**.</span></span>  <span data-ttu-id="217ff-182">En ytterligare domänkontrollant och visa har lagts till för att illustrera åtkomst till användarens profilinformation som använder Azure graph API: er.</span><span class="sxs-lookup"><span data-stu-id="217ff-182">An additional controller and view were added to illustrate accessing user profile information using Azure graph APIs.</span></span>  <span data-ttu-id="217ff-183">De här filerna är **Controllers\UserProfileController.cs** och **Views\UserProfile\Index.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="217ff-183">These files are **Controllers\UserProfileController.cs** and **Views\UserProfile\Index.cshtml**.</span></span>

### <a name="additional-startup-code-was-added-to-your-project"></a><span data-ttu-id="217ff-184">Ytterligare startkoden har lagts till i ditt projekt</span><span class="sxs-lookup"><span data-stu-id="217ff-184">Additional Startup code was added to your project</span></span>
<span data-ttu-id="217ff-185">I den **startup.auth.cs** -fil, en ny **OpenIdConnectAuthenticationNotifications** objekt lades till i **meddelanden** medlem i den  **OpenIdConnectAuthenticationOptions**.</span><span class="sxs-lookup"><span data-stu-id="217ff-185">In the **startup.auth.cs** file, a new **OpenIdConnectAuthenticationNotifications** object was added to the **Notifications** member of the **OpenIdConnectAuthenticationOptions**.</span></span>  <span data-ttu-id="217ff-186">Detta är att ta emot OAuth-koden och byta ut den för en åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="217ff-186">This is to enable receiving the OAuth code and exchanging it for an access token.</span></span>

### <a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a><span data-ttu-id="217ff-187">Ytterligare ändringar har gjorts till web.config eller app.config</span><span class="sxs-lookup"><span data-stu-id="217ff-187">Additional changes were made to your app.config or web.config</span></span>
<span data-ttu-id="217ff-188">Följande ytterligare konfigurationsposter har lagts till.</span><span class="sxs-lookup"><span data-stu-id="217ff-188">The following additional configuration entries have been added.</span></span>

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

<span data-ttu-id="217ff-189">Följande avsnitt och anslutningssträngen har lagts till.</span><span class="sxs-lookup"><span data-stu-id="217ff-189">The following configuration sections and connection string have been added.</span></span>

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


### <a name="your-azure-active-directory-app-was-updated"></a><span data-ttu-id="217ff-190">Din Azure Active Directory App har uppdaterats</span><span class="sxs-lookup"><span data-stu-id="217ff-190">Your Azure Active Directory App was updated</span></span>
<span data-ttu-id="217ff-191">Din Azure Active Directory App har uppdaterats med den *läsa katalogdata* behörighet och nyckeln skapades som användes som sedan den *ida: ClientSecret* i den  **Web.config** fil.</span><span class="sxs-lookup"><span data-stu-id="217ff-191">Your Azure Active Directory App was updated to include the *Read directory data* permission and an additional key was created which was then used as the *ida:ClientSecret* in the **web.config** file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="217ff-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="217ff-192">Next steps</span></span>
- [<span data-ttu-id="217ff-193">Lär dig mer om Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="217ff-193">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

