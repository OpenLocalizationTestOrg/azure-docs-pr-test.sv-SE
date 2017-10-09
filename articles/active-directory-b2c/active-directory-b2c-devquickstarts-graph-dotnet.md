---
title: "Azure Active Directory B2C: Använd hello Graph API | Microsoft Docs"
description: "Hur hello toocall Graph API för en B2C-klient med hjälp av en identitet tooautomate hello programprocessen."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f9904516-d9f7-43b1-ae4f-e4d9eb1c67a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/07/2017
ms.author: parakhj
ms.openlocfilehash: a17cdc4adf57dbf22592d99ef8ecde41652763fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-use-hello-graph-api"></a><span data-ttu-id="55a7e-103">Azure AD B2C: Använda hello Graph API</span><span class="sxs-lookup"><span data-stu-id="55a7e-103">Azure AD B2C: Use hello Graph API</span></span>
<span data-ttu-id="55a7e-104">Azure Active Directory (AD Azure) B2C hyresgäster tenderar toobe mycket stora.</span><span class="sxs-lookup"><span data-stu-id="55a7e-104">Azure Active Directory (Azure AD) B2C tenants tend toobe very large.</span></span> <span data-ttu-id="55a7e-105">Detta innebär att många vanliga klient hanteringsuppgifter måste toobe utföras via programmering.</span><span class="sxs-lookup"><span data-stu-id="55a7e-105">This means that many common tenant management tasks need toobe performed programmatically.</span></span> <span data-ttu-id="55a7e-106">En primär exempel är användarhantering.</span><span class="sxs-lookup"><span data-stu-id="55a7e-106">A primary example is user management.</span></span> <span data-ttu-id="55a7e-107">Du kan behöva toomigrate en befintlig användare store tooa B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="55a7e-107">You might need toomigrate an existing user store tooa B2C tenant.</span></span> <span data-ttu-id="55a7e-108">Du kanske vill toohost användarregistrering på sidan och skapa användarkonton i Azure AD B2C-katalogen hello bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="55a7e-108">You may want toohost user registration on your own page and create user accounts in your Azure AD B2C directory behind hello scenes.</span></span> <span data-ttu-id="55a7e-109">Dessa typer av uppgifter kräver hello möjlighet toocreate, läsa, uppdatera och ta bort användarkonton.</span><span class="sxs-lookup"><span data-stu-id="55a7e-109">These types of tasks require hello ability toocreate, read, update, and delete user accounts.</span></span> <span data-ttu-id="55a7e-110">Du kan utföra dessa uppgifter med hjälp av hello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="55a7e-110">You can do these tasks by using hello Azure AD Graph API.</span></span>

<span data-ttu-id="55a7e-111">För B2C-innehavare finns det två primära lägen kommunicera med hello Graph API.</span><span class="sxs-lookup"><span data-stu-id="55a7e-111">For B2C tenants, there are two primary modes of communicating with hello Graph API.</span></span>

* <span data-ttu-id="55a7e-112">För interaktiv, körs en gång aktiviteter, bör du fungerar som ett administratörskontot i hello B2C-klient när du utför hello uppgifter.</span><span class="sxs-lookup"><span data-stu-id="55a7e-112">For interactive, run-once tasks, you should act as an administrator account in hello B2C tenant when you perform hello tasks.</span></span> <span data-ttu-id="55a7e-113">Det här läget kräver en administratör toosign in med autentiseringsuppgifter innan som administratören kan utföra alla anrop toohello Graph API.</span><span class="sxs-lookup"><span data-stu-id="55a7e-113">This mode requires an administrator toosign in with credentials before that admin can perform any calls toohello Graph API.</span></span>
* <span data-ttu-id="55a7e-114">Du bör använda någon typ av konto som du anger för automatisk, kontinuerlig uppgifter med hello behörighet som krävs för tooperform hanteringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="55a7e-114">For automated, continuous tasks, you should use some type of service account that you provide with hello necessary privileges tooperform management tasks.</span></span> <span data-ttu-id="55a7e-115">Du kan göra detta genom att registrera ett program och autentisera tooAzure AD i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="55a7e-115">In Azure AD, you can do this by registering an application and authenticating tooAzure AD.</span></span> <span data-ttu-id="55a7e-116">Detta görs med hjälp av en **program-ID** som använder hello [OAuth 2.0 klientens autentiseringsuppgifter bevilja](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api).</span><span class="sxs-lookup"><span data-stu-id="55a7e-116">This is done by using an **Application ID** that uses hello [OAuth 2.0 client credentials grant](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api).</span></span> <span data-ttu-id="55a7e-117">I det här fallet fungerar hello programmet som själva inte som en användare toocall hello Graph API.</span><span class="sxs-lookup"><span data-stu-id="55a7e-117">In this case, hello application acts as itself, not as a user, toocall hello Graph API.</span></span>

<span data-ttu-id="55a7e-118">I den här artikeln handlar om hur tooperform hello automated användningsfall.</span><span class="sxs-lookup"><span data-stu-id="55a7e-118">In this article, we'll discuss how tooperform hello automated-use case.</span></span> <span data-ttu-id="55a7e-119">toodemonstrate, ska vi skapa en .NET 4.5 `B2CGraphClient` som utför användare skapa, läsa, uppdatera och ta bort CRUD-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="55a7e-119">toodemonstrate, we'll build a .NET 4.5 `B2CGraphClient` that performs user create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="55a7e-120">hello-klienten har en Windows-kommandoradsgränssnittet (CLI) som gör att du tooinvoke olika sätt.</span><span class="sxs-lookup"><span data-stu-id="55a7e-120">hello client will have a Windows command-line interface (CLI) that allows you tooinvoke various methods.</span></span> <span data-ttu-id="55a7e-121">Dock skrivs hello kod toobehave i ett icke-interaktiv, automatiserat sätt.</span><span class="sxs-lookup"><span data-stu-id="55a7e-121">However, hello code is written toobehave in a noninteractive, automated fashion.</span></span>

## <a name="get-an-azure-ad-b2c-tenant"></a><span data-ttu-id="55a7e-122">Skaffa en Azure AD B2C-klient</span><span class="sxs-lookup"><span data-stu-id="55a7e-122">Get an Azure AD B2C tenant</span></span>
<span data-ttu-id="55a7e-123">Innan du kan skapa program eller användare eller interagera med Azure AD alls, behöver du en Azure AD B2C-klient och ett globalt administratörskonto i hello-klient.</span><span class="sxs-lookup"><span data-stu-id="55a7e-123">Before you can create applications or users, or interact with Azure AD at all, you will need an Azure AD B2C tenant and a global administrator account in hello tenant.</span></span> <span data-ttu-id="55a7e-124">Om du inte redan har en klient [Kom igång med Azure AD B2C](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="55a7e-124">If you don't have a tenant already, [get started with Azure AD B2C](active-directory-b2c-get-started.md).</span></span>

## <a name="register-your-application-in-your-tenant"></a><span data-ttu-id="55a7e-125">Registrera ditt program i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="55a7e-125">Register your application in your tenant</span></span>
<span data-ttu-id="55a7e-126">När du har en B2C-klient måste tooregister ditt program via hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="55a7e-126">After you have a B2C tenant, you need tooregister your application via hello [Azure Portal](https://portal.azure.com).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="55a7e-127">toouse hello Graph API med din B2C-klient, måste tooregister ett dedikerat program med hjälp av hello generisk *App registreringar* bladet i hello Azure-portalen **inte** Azure AD B2C  *Program* bladet.</span><span class="sxs-lookup"><span data-stu-id="55a7e-127">toouse hello Graph API with your B2C tenant, you will need tooregister a dedicated application by using hello generic *App Registrations* blade in hello Azure Portal, **NOT** Azure AD B2C's *Applications* blade.</span></span> <span data-ttu-id="55a7e-128">Du kan inte återanvända hello redan befintliga B2C program som du har registrerat i hello Azure AD B2C *program* bladet.</span><span class="sxs-lookup"><span data-stu-id="55a7e-128">You can't reuse hello already-existing B2C applications that you registered in hello Azure AD B2C's *Applications* blade.</span></span>

1. <span data-ttu-id="55a7e-129">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="55a7e-129">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="55a7e-130">Välj din Azure AD B2C-klient genom att välja kontot i hello övre högra hörnet av hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="55a7e-130">Choose your Azure AD B2C tenant by selecting your account in hello top right corner of hello page.</span></span>
3. <span data-ttu-id="55a7e-131">Hello vänstra navigeringsfönstret och välj **fler tjänster**, klickar du på **App registreringar**, och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="55a7e-131">In hello left-hand navigation pane, choose **More Services**, click **App Registrations**, and click **Add**.</span></span>
4. <span data-ttu-id="55a7e-132">Följ anvisningarna för hello och skapa ett nytt program.</span><span class="sxs-lookup"><span data-stu-id="55a7e-132">Follow hello prompts and create a new application.</span></span> 
    1. <span data-ttu-id="55a7e-133">Välj **Webbapp / API** som hello programtyp.</span><span class="sxs-lookup"><span data-stu-id="55a7e-133">Select **Web App / API** as hello Application Type.</span></span>    
    2. <span data-ttu-id="55a7e-134">Ange **någon omdirigerings-URI** (t.ex. https://B2CGraphAPI) eftersom den inte är relevanta för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="55a7e-134">Provide **any redirect URI** (e.g. https://B2CGraphAPI) as it's not relevant for this example.</span></span>  
5. <span data-ttu-id="55a7e-135">hello programmet kommer nu visas i hello lista över program klickar du på den tooobtain hello **program-ID** (även kallat klient-ID).</span><span class="sxs-lookup"><span data-stu-id="55a7e-135">hello application will now show up in hello list of applications, click on it tooobtain hello **Application ID** (also known as Client ID).</span></span> <span data-ttu-id="55a7e-136">Kopiera den som du behöver i ett senare avsnitt.</span><span class="sxs-lookup"><span data-stu-id="55a7e-136">Copy it as you'll need it in a later section.</span></span>
6. <span data-ttu-id="55a7e-137">I hello inställningar-bladet klickar du på **nycklar** och lägga till en ny nyckel (även kallat klienthemlighet).</span><span class="sxs-lookup"><span data-stu-id="55a7e-137">In hello Settings blade, click on **Keys** and add a new key (also known as client secret).</span></span> <span data-ttu-id="55a7e-138">Också kopiera den för användning i ett senare avsnitt.</span><span class="sxs-lookup"><span data-stu-id="55a7e-138">Also copy it for use in a later section.</span></span>

## <a name="configure-create-read-and-update-permissions-for-your-application"></a><span data-ttu-id="55a7e-139">Konfigurera skapa, läsa och uppdatera behörigheterna för ditt program</span><span class="sxs-lookup"><span data-stu-id="55a7e-139">Configure create, read and update permissions for your application</span></span>
<span data-ttu-id="55a7e-140">Nu måste tooconfigure ditt program tooget alla hello krävs behörigheter toocreate, läsa, uppdatera och ta bort användare.</span><span class="sxs-lookup"><span data-stu-id="55a7e-140">Now you need tooconfigure your application tooget all hello required permissions toocreate, read, update and delete users.</span></span>

1. <span data-ttu-id="55a7e-141">Om du fortsätter i hello Azure portal App registreringar bladet, Välj ditt program.</span><span class="sxs-lookup"><span data-stu-id="55a7e-141">Continuing in hello Azure portal's App Registrations blade, select your application.</span></span>
2. <span data-ttu-id="55a7e-142">I hello inställningar-bladet klickar du på **nödvändiga behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="55a7e-142">In hello Settings blade, click on **Required permissions**.</span></span>
3. <span data-ttu-id="55a7e-143">I hello behörighet bladet, klickar du på **Windows Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="55a7e-143">In hello Required permissions blade, click on **Windows Azure Active Directory**.</span></span>
4. <span data-ttu-id="55a7e-144">Hello Aktivera åtkomst bladet välj hello **läsning och skrivning katalogdata** tillstånd från **programbehörigheter** och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="55a7e-144">In hello Enable Access  blade, select hello **Read and write directory data** permission from **Application Permissions** and click **Save**.</span></span>
5. <span data-ttu-id="55a7e-145">Tillbaka i bladet för hello behörigheter som krävs, klicka på hello **bevilja med** knappen.</span><span class="sxs-lookup"><span data-stu-id="55a7e-145">Finally, back in hello Required permissions blade, click on hello **Grant Permissions** button.</span></span>

<span data-ttu-id="55a7e-146">Nu har du ett program som har behörigheten toocreate, läsa och uppdatera användare från din B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="55a7e-146">You now have an application that has permission toocreate, read and update users from your B2C tenant.</span></span>

## <a name="configure-delete-permissions-for-your-application"></a><span data-ttu-id="55a7e-147">Konfigurera behörighet att ta bort programmet</span><span class="sxs-lookup"><span data-stu-id="55a7e-147">Configure delete permissions for your application</span></span>
<span data-ttu-id="55a7e-148">För närvarande hello *läsning och skrivning katalogdata* behörighet har **inte** inkluderar hello möjlighet toodo eventuella borttagningar, till exempel ta bort användare.</span><span class="sxs-lookup"><span data-stu-id="55a7e-148">Currently, hello *Read and write directory data* permission does **NOT** include hello ability toodo any deletions such as deleting users.</span></span> <span data-ttu-id="55a7e-149">Om du vill toogive programmet hello möjlighet toodelete användarna behöver du toodo dessa extra steg som rör PowerShell, annars kan du hoppa över toohello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="55a7e-149">If you want toogive your application hello ability toodelete users, you'll need toodo these extra steps that involve PowerShell, otherwise, you can skip toohello next section.</span></span>

<span data-ttu-id="55a7e-150">Först ladda ned och installera hello [Microsoft Online Services-inloggningsassistent](http://go.microsoft.com/fwlink/?LinkID=286152).</span><span class="sxs-lookup"><span data-stu-id="55a7e-150">First, download and install hello [Microsoft Online Services Sign-In Assistant](http://go.microsoft.com/fwlink/?LinkID=286152).</span></span> <span data-ttu-id="55a7e-151">Hämta och installera hello [64-bitars Azure Active Directory-modulen för Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).</span><span class="sxs-lookup"><span data-stu-id="55a7e-151">Then download and install hello [64-bit Azure Active Directory module for Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).</span></span>

<span data-ttu-id="55a7e-152">När du har installerat hello PowerShell-modulen öppna PowerShell och Anslut tooyour B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="55a7e-152">After you install hello PowerShell module, open PowerShell and connect tooyour B2C tenant.</span></span> <span data-ttu-id="55a7e-153">När du har kört `Get-Credential`, du uppmanas att ett användarnamn och lösenord, ange hello användarnamnet och lösenordet för administratörskontot din B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="55a7e-153">After you run `Get-Credential`, you will be prompted for a user name and password, Enter hello user name and password of your B2C tenant administrator account.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="55a7e-154">Du behöver ett administratörskontot för toouse en B2C-klient som är **lokala** toohello B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="55a7e-154">You need toouse a B2C tenant administrator account that is **local** toohello B2C tenant.</span></span> <span data-ttu-id="55a7e-155">Dessa konton se ut så här: myusername@myb2ctenant.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="55a7e-155">These accounts look like this: myusername@myb2ctenant.onmicrosoft.com.</span></span>

```powershell
Connect-MsolService
```

<span data-ttu-id="55a7e-156">Nu ska vi använda hello **program-ID** i hello-skriptet nedan tooassign hello programmet hello användaren konto administratörsroll som gör att den toodelete användare.</span><span class="sxs-lookup"><span data-stu-id="55a7e-156">Now we'll use hello **Application ID** in hello script below tooassign hello application hello user account administrator role which will allow it toodelete users.</span></span> <span data-ttu-id="55a7e-157">Dessa roller har välkända identifierare, så behöver du toodo matas in din **program-ID** i hello skriptet nedan.</span><span class="sxs-lookup"><span data-stu-id="55a7e-157">These roles have well-known identifiers, so all you need toodo is input your **Application ID** in hello script below.</span></span>

```powershell
$applicationId = "<YOUR_APPLICATION_ID>"
$sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
```

<span data-ttu-id="55a7e-158">Ditt program nu har även behörighet toodelete användare från din B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="55a7e-158">Your application now also has permissions toodelete users from your B2C tenant.</span></span>

## <a name="download-configure-and-build-hello-sample-code"></a><span data-ttu-id="55a7e-159">Hämta, konfigurera och skapa hello exempelkod</span><span class="sxs-lookup"><span data-stu-id="55a7e-159">Download, configure, and build hello sample code</span></span>
<span data-ttu-id="55a7e-160">Först hämta hello exempelkod och få det körs.</span><span class="sxs-lookup"><span data-stu-id="55a7e-160">First, download hello sample code and get it running.</span></span> <span data-ttu-id="55a7e-161">Sedan tar vi en närmare titt på den.</span><span class="sxs-lookup"><span data-stu-id="55a7e-161">Then we will take a closer look at it.</span></span>  <span data-ttu-id="55a7e-162">Du kan [hämta hello exempelkod som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip).</span><span class="sxs-lookup"><span data-stu-id="55a7e-162">You can [download hello sample code as a .zip file](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip).</span></span> <span data-ttu-id="55a7e-163">Du kan också klona den i en katalog som du väljer:</span><span class="sxs-lookup"><span data-stu-id="55a7e-163">You can also clone it into a directory of your choice:</span></span>

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

<span data-ttu-id="55a7e-164">Öppna hello `B2CGraphClient\B2CGraphClient.sln` Visual Studio-lösning i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55a7e-164">Open hello `B2CGraphClient\B2CGraphClient.sln` Visual Studio solution in Visual Studio.</span></span> <span data-ttu-id="55a7e-165">I hello `B2CGraphClient` projekt, öppna hello fil `App.config`.</span><span class="sxs-lookup"><span data-stu-id="55a7e-165">In hello `B2CGraphClient` project, open hello file `App.config`.</span></span> <span data-ttu-id="55a7e-166">Ersätt hello tre appinställningar med egna värden:</span><span class="sxs-lookup"><span data-stu-id="55a7e-166">Replace hello three app settings with your own values:</span></span>

```
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{hello ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{hello Key from above}" />
</appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

<span data-ttu-id="55a7e-167">Högerklicka sedan på hello `B2CGraphClient` lösning och återskapa hello-exempel.</span><span class="sxs-lookup"><span data-stu-id="55a7e-167">Next, right-click on hello `B2CGraphClient` solution and rebuild hello sample.</span></span> <span data-ttu-id="55a7e-168">Om du lyckas kan du bör nu ha en `B2C.exe` körbar fil som finns i `B2CGraphClient\bin\Debug`.</span><span class="sxs-lookup"><span data-stu-id="55a7e-168">If you are successful, you should now have a `B2C.exe` executable file located in `B2CGraphClient\bin\Debug`.</span></span>

## <a name="build-user-crud-operations-by-using-hello-graph-api"></a><span data-ttu-id="55a7e-169">Skapa användaren CRUD-åtgärder med hjälp av hello Graph API</span><span class="sxs-lookup"><span data-stu-id="55a7e-169">Build user CRUD operations by using hello Graph API</span></span>
<span data-ttu-id="55a7e-170">toouse hello B2CGraphClient, öppna en `cmd` Windows kommandot fråga och ändra din katalog toohello `Debug` directory.</span><span class="sxs-lookup"><span data-stu-id="55a7e-170">toouse hello B2CGraphClient, open a `cmd` Windows command prompt and change your directory toohello `Debug` directory.</span></span> <span data-ttu-id="55a7e-171">Kör sedan hello `B2C Help` kommando.</span><span class="sxs-lookup"><span data-stu-id="55a7e-171">Then run hello `B2C Help` command.</span></span>

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

<span data-ttu-id="55a7e-172">Då visas en kort beskrivning av varje kommando.</span><span class="sxs-lookup"><span data-stu-id="55a7e-172">This will display a brief description of each command.</span></span> <span data-ttu-id="55a7e-173">Varje gång du anropar någon av dessa kommandon `B2CGraphClient` gör en begäran toohello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="55a7e-173">Each time you invoke one of these commands, `B2CGraphClient` makes a request toohello Azure AD Graph API.</span></span>

### <a name="get-an-access-token"></a><span data-ttu-id="55a7e-174">Hämta en åtkomsttoken</span><span class="sxs-lookup"><span data-stu-id="55a7e-174">Get an access token</span></span>
<span data-ttu-id="55a7e-175">Alla begäran toohello Graph API kräver en åtkomst-token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="55a7e-175">Any request toohello Graph API requires an access token for authentication.</span></span> <span data-ttu-id="55a7e-176">`B2CGraphClient`använder hello öppen källkod Active Directory Authentication Library (ADAL) toohelp hämta åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="55a7e-176">`B2CGraphClient` uses hello open-source Active Directory Authentication Library (ADAL) toohelp acquire access tokens.</span></span> <span data-ttu-id="55a7e-177">ADAL underlättar token förvärv genom att tillhandahålla ett enkelt API och tar hand om vissa viktig information, till exempel cachelagring åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="55a7e-177">ADAL makes token acquisition easier by providing a simple API and taking care of some important details, such as caching access tokens.</span></span> <span data-ttu-id="55a7e-178">Du har inte toouse ADAL tooget token om.</span><span class="sxs-lookup"><span data-stu-id="55a7e-178">You don't have toouse ADAL tooget tokens, though.</span></span> <span data-ttu-id="55a7e-179">Du kan också hämta token genom att utforma HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="55a7e-179">You can also get tokens by crafting HTTP requests.</span></span>

> [!NOTE]
> <span data-ttu-id="55a7e-180">Det här kodexemplet använder ADAL v2 i ordning toocommunicate med hello Graph API.</span><span class="sxs-lookup"><span data-stu-id="55a7e-180">This code sample uses ADAL v2 in order toocommunicate with hello Graph API.</span></span>  <span data-ttu-id="55a7e-181">Du måste använda ADAL v2 och v3 i ordning tooget åtkomst-token som kan användas med hello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="55a7e-181">You must use ADAL v2 or v3 in order tooget access tokens which can be used with hello Azure AD Graph API.</span></span>
> 
> 

<span data-ttu-id="55a7e-182">När `B2CGraphClient` körs, skapas en instans av hello `B2CGraphClient` klass.</span><span class="sxs-lookup"><span data-stu-id="55a7e-182">When `B2CGraphClient` runs, it creates an instance of hello `B2CGraphClient` class.</span></span> <span data-ttu-id="55a7e-183">hello konstruktören för den här klassen ställer in en scaffold-teknik för ADAL-autentisering:</span><span class="sxs-lookup"><span data-stu-id="55a7e-183">hello constructor for this class sets up an ADAL authentication scaffolding:</span></span>

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // hello client_id, client_secret, and tenant are provided in Program.cs, which pulls hello values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // hello AuthenticationContext is ADAL's primary class, in which you indicate hello tenant toouse.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // hello ClientCredential is where you pass in your client_id and client_secret, which are
    // provided tooAzure AD in order tooreceive an access_token by using hello app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

<span data-ttu-id="55a7e-184">Vi använder hello `B2C Get-User` kommandot som exempel.</span><span class="sxs-lookup"><span data-stu-id="55a7e-184">We'll use hello `B2C Get-User` command as an example.</span></span> <span data-ttu-id="55a7e-185">När `B2C Get-User` anropas utan några ytterligare indata hello CLI anrop hello `B2CGraphClient.GetAllUsers(...)` metod.</span><span class="sxs-lookup"><span data-stu-id="55a7e-185">When `B2C Get-User` is invoked without any additional inputs, hello CLI calls hello `B2CGraphClient.GetAllUsers(...)` method.</span></span> <span data-ttu-id="55a7e-186">Den här metoden anropar `B2CGraphClient.SendGraphGetRequest(...)`, som skickar en HTTP GET-begäran toohello Graph API.</span><span class="sxs-lookup"><span data-stu-id="55a7e-186">This method calls `B2CGraphClient.SendGraphGetRequest(...)`, which submits an HTTP GET request toohello Graph API.</span></span> <span data-ttu-id="55a7e-187">Innan `B2CGraphClient.SendGraphGetRequest(...)` skickar Hej GET-begäran, först hämtar en åtkomst-token med hjälp av ADAL:</span><span class="sxs-lookup"><span data-stu-id="55a7e-187">Before `B2CGraphClient.SendGraphGetRequest(...)` sends hello GET request, it first gets an access token by using ADAL:</span></span>

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL tooacquire a token by using hello app's identity (hello credential)
    // hello first parameter is hello resource we want an access_token for; in this case, hello Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

<span data-ttu-id="55a7e-188">Du kan hämta ett åtkomsttoken för hello Graph API genom att anropa hello ADAL `AuthenticationContext.AcquireToken(...)` metod.</span><span class="sxs-lookup"><span data-stu-id="55a7e-188">You can get an access token for hello Graph API by calling hello ADAL `AuthenticationContext.AcquireToken(...)` method.</span></span> <span data-ttu-id="55a7e-189">ADAL returnerar en `access_token` som representerar hello Programidentitet.</span><span class="sxs-lookup"><span data-stu-id="55a7e-189">ADAL then returns an `access_token` that represents hello application's identity.</span></span>

### <a name="read-users"></a><span data-ttu-id="55a7e-190">Läs användare</span><span class="sxs-lookup"><span data-stu-id="55a7e-190">Read users</span></span>
<span data-ttu-id="55a7e-191">När du vill tooget en lista med användare eller hämta en viss användare från hello Graph API, kan du skicka en HTTP `GET` begära toohello `/users` slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="55a7e-191">When you want tooget a list of users or get a particular user from hello Graph API, you can send an HTTP `GET` request toohello `/users` endpoint.</span></span> <span data-ttu-id="55a7e-192">En begäran om alla hello användare i en klient som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="55a7e-192">A request for all of hello users in a tenant looks like this:</span></span>

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

<span data-ttu-id="55a7e-193">toosee denna begäran, kör:</span><span class="sxs-lookup"><span data-stu-id="55a7e-193">toosee this request, run:</span></span>

 ```
 > B2C Get-User
 ```

<span data-ttu-id="55a7e-194">Det finns två viktiga saker toonote:</span><span class="sxs-lookup"><span data-stu-id="55a7e-194">There are two important things toonote:</span></span>

* <span data-ttu-id="55a7e-195">hello åtkomsttoken har köpt via ADAL läggs toohello `Authorization` sidhuvud med hjälp av hello `Bearer` schema.</span><span class="sxs-lookup"><span data-stu-id="55a7e-195">hello access token acquired via ADAL is added toohello `Authorization` header by using hello `Bearer` scheme.</span></span>
* <span data-ttu-id="55a7e-196">För B2C-innehavare, måste du använda hello frågeparameter `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="55a7e-196">For B2C tenants, you must use hello query parameter `api-version=1.6`.</span></span>

<span data-ttu-id="55a7e-197">Båda dessa uppgifter hanteras i hello `B2CGraphClient.SendGraphGetRequest(...)` metoden:</span><span class="sxs-lookup"><span data-stu-id="55a7e-197">Both of these details are handled in hello `B2CGraphClient.SendGraphGetRequest(...)` method:</span></span>

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure toouse hello 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append hello access token for hello Graph API toohello Authorization header of hello request by using hello Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a><span data-ttu-id="55a7e-198">Skapa användarkonton för konsumenten</span><span class="sxs-lookup"><span data-stu-id="55a7e-198">Create consumer user accounts</span></span>
<span data-ttu-id="55a7e-199">När du skapar användarkonton i din B2C-klient kan du skicka en HTTP `POST` begära toohello `/users` slutpunkt:</span><span class="sxs-lookup"><span data-stu-id="55a7e-199">When you create user accounts in your B2C tenant, you can send an HTTP `POST` request toohello `/users` endpoint:</span></span>

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required toocreate consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier hello user uses toosign in toohello account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set too'LocalAccount'
    "displayName": "Joe Consumer",                // a value that can be used for displaying toohello end user
    "mailNickname": "joec",                        // an email alias for hello user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set toofalse
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

<span data-ttu-id="55a7e-200">De flesta av de här egenskaperna i den här förfrågan är nödvändiga toocreate konsumentanvändare.</span><span class="sxs-lookup"><span data-stu-id="55a7e-200">Most of these properties in this request are required toocreate consumer users.</span></span> <span data-ttu-id="55a7e-201">toolearn mer, klickar du på [här](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser).</span><span class="sxs-lookup"><span data-stu-id="55a7e-201">toolearn more, click [here](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser).</span></span> <span data-ttu-id="55a7e-202">Observera att hello `//` kommentarer har inkluderats för bilden.</span><span class="sxs-lookup"><span data-stu-id="55a7e-202">Note that hello `//` comments have been included for illustration.</span></span> <span data-ttu-id="55a7e-203">Ta inte med dem i en faktiska begäran.</span><span class="sxs-lookup"><span data-stu-id="55a7e-203">Do not include them in an actual request.</span></span>

<span data-ttu-id="55a7e-204">toosee hello begäran att köra något av följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="55a7e-204">toosee hello request, run one of hello following commands:</span></span>

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

<span data-ttu-id="55a7e-205">Hej `Create-User` kommandot tar emot en JSON-fil som indataparameter.</span><span class="sxs-lookup"><span data-stu-id="55a7e-205">hello `Create-User` command takes a .json file as an input parameter.</span></span> <span data-ttu-id="55a7e-206">Innehåller en JSON-representation av ett användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="55a7e-206">This contains a JSON representation of a user object.</span></span> <span data-ttu-id="55a7e-207">Det finns två exempel JSON-filer i hello exempelkod: `usertemplate-email.json` och `usertemplate-username.json`.</span><span class="sxs-lookup"><span data-stu-id="55a7e-207">There are two sample .json files in hello sample code: `usertemplate-email.json` and `usertemplate-username.json`.</span></span> <span data-ttu-id="55a7e-208">Du kan ändra dessa filer toosuit dina behov.</span><span class="sxs-lookup"><span data-stu-id="55a7e-208">You can modify these files toosuit your needs.</span></span> <span data-ttu-id="55a7e-209">Dessutom toohello obligatoriska fält ovan, flera valfria fälten som du kan använda ingår i dessa filer.</span><span class="sxs-lookup"><span data-stu-id="55a7e-209">In addition toohello required fields above, several optional fields that you can use are included in these files.</span></span> <span data-ttu-id="55a7e-210">Information om hello valfria fält finns i hello [Azure AD Graph API Entitetsreferens](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).</span><span class="sxs-lookup"><span data-stu-id="55a7e-210">Details on hello optional fields can be found in hello [Azure AD Graph API entity reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).</span></span>

<span data-ttu-id="55a7e-211">Du kan se hur hello POST-begäran har skapats i `B2CGraphClient.SendGraphPostRequest(...)`.</span><span class="sxs-lookup"><span data-stu-id="55a7e-211">You can see how hello POST request is constructed in `B2CGraphClient.SendGraphPostRequest(...)`.</span></span>

* <span data-ttu-id="55a7e-212">En åtkomst-token toohello bifogas `Authorization` huvudet i begäran hello.</span><span class="sxs-lookup"><span data-stu-id="55a7e-212">It attaches an access token toohello `Authorization` header of hello request.</span></span>
* <span data-ttu-id="55a7e-213">Anger `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="55a7e-213">It sets `api-version=1.6`.</span></span>
* <span data-ttu-id="55a7e-214">Hello brödtext hello begäran innehåller hello JSON-användarobjektet.</span><span class="sxs-lookup"><span data-stu-id="55a7e-214">It includes hello JSON user object in hello body of hello request.</span></span>

> [!NOTE]
> <span data-ttu-id="55a7e-215">Om hello konton som du vill toomigrate från en befintlig användararkivet har lägre lösenordssäkerhet än hello [starka lösenord tillämpas av Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), kan du inaktivera hello starkt lösenordskrav med hello `DisableStrongPassword`värde i hello `passwordPolicies` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="55a7e-215">If hello accounts that you want toomigrate from an existing user store has lower password strength than hello [strong password strength enforced by Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), you can disable hello strong password requirement using hello `DisableStrongPassword` value in hello `passwordPolicies` property.</span></span> <span data-ttu-id="55a7e-216">Du kan till exempel ändra hello skapa användarbegäran som anges ovan på följande sätt: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.</span><span class="sxs-lookup"><span data-stu-id="55a7e-216">For instance, you can modify hello create user request provided above as follows: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.</span></span>
> 
> 

### <a name="update-consumer-user-accounts"></a><span data-ttu-id="55a7e-217">Uppdatera konsumenten användarkonton</span><span class="sxs-lookup"><span data-stu-id="55a7e-217">Update consumer user accounts</span></span>
<span data-ttu-id="55a7e-218">När du uppdaterar användarobjekt är hello processen liknande toohello som du använder toocreate användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="55a7e-218">When you update user objects, hello process is similar toohello one you use toocreate user objects.</span></span> <span data-ttu-id="55a7e-219">Men den här processen använder hello HTTP `PATCH` metoden:</span><span class="sxs-lookup"><span data-stu-id="55a7e-219">But this process uses hello HTTP `PATCH` method:</span></span>

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",                // this request updates only hello user's displayName
}
```

<span data-ttu-id="55a7e-220">Försök tooupdate en användare genom att uppdatera JSON-filer med nya data.</span><span class="sxs-lookup"><span data-stu-id="55a7e-220">Try tooupdate a user by updating your JSON files with new data.</span></span> <span data-ttu-id="55a7e-221">Du kan sedan använda `B2CGraphClient` toorun något av följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="55a7e-221">You can then use `B2CGraphClient` toorun one of these commands:</span></span>

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

<span data-ttu-id="55a7e-222">Inspektera hello `B2CGraphClient.SendGraphPatchRequest(...)` metod för information om hur toosend denna begäran.</span><span class="sxs-lookup"><span data-stu-id="55a7e-222">Inspect hello `B2CGraphClient.SendGraphPatchRequest(...)` method for details on how toosend this request.</span></span>

### <a name="search-users"></a><span data-ttu-id="55a7e-223">Söka efter användare</span><span class="sxs-lookup"><span data-stu-id="55a7e-223">Search users</span></span>
<span data-ttu-id="55a7e-224">Du kan söka efter användare i din B2C-klient på ett par olika sätt.</span><span class="sxs-lookup"><span data-stu-id="55a7e-224">You can search for users in your B2C tenant in a couple of ways.</span></span> <span data-ttu-id="55a7e-225">En med hjälp av hello användarens objekt-ID eller två, med hjälp av hello användarens inloggning Identiferare (d.v.s. hello `signInNames` egenskap).</span><span class="sxs-lookup"><span data-stu-id="55a7e-225">One, using hello user's object ID or two, using hello user's sign-in identifer (i.e., hello `signInNames` property).</span></span>

<span data-ttu-id="55a7e-226">Kör något av följande kommandon toosearch för en viss användare hello:</span><span class="sxs-lookup"><span data-stu-id="55a7e-226">Run one of hello following commands toosearch for a specific user:</span></span>

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

<span data-ttu-id="55a7e-227">Här följer några exempel:</span><span class="sxs-lookup"><span data-stu-id="55a7e-227">Here are a couple of examples:</span></span>

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a><span data-ttu-id="55a7e-228">Ta bort användare</span><span class="sxs-lookup"><span data-stu-id="55a7e-228">Delete users</span></span>
<span data-ttu-id="55a7e-229">hello-processen för att ta bort en användare är enkelt.</span><span class="sxs-lookup"><span data-stu-id="55a7e-229">hello process for deleting a user is straightforward.</span></span> <span data-ttu-id="55a7e-230">Använd hello HTTP `DELETE` metoden och konstruktion hello URL: en med hello korrigera objekt-ID:</span><span class="sxs-lookup"><span data-stu-id="55a7e-230">Use hello HTTP `DELETE` method and construct hello URL with hello correct object ID:</span></span>

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

<span data-ttu-id="55a7e-231">toosee exempelvis ange det här kommandot och visa hello delete begäran som är tryckt toohello konsolen:</span><span class="sxs-lookup"><span data-stu-id="55a7e-231">toosee an example, enter this command and view hello delete request that is printed toohello console:</span></span>

```
> B2C Delete-User <object-id-of-user>
```

<span data-ttu-id="55a7e-232">Inspektera hello `B2CGraphClient.SendGraphDeleteRequest(...)` metod för information om hur toosend denna begäran.</span><span class="sxs-lookup"><span data-stu-id="55a7e-232">Inspect hello `B2CGraphClient.SendGraphDeleteRequest(...)` method for details on how toosend this request.</span></span>

<span data-ttu-id="55a7e-233">Du kan utföra många andra åtgärder med hello Azure AD Graph API i tillägg toouser management.</span><span class="sxs-lookup"><span data-stu-id="55a7e-233">You can perform many other actions with hello Azure AD Graph API in addition toouser management.</span></span> <span data-ttu-id="55a7e-234">Den [Azure AD Graph API-referens](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) innehåller information om varje åtgärd, tillsammans med exempel begäranden.</span><span class="sxs-lookup"><span data-stu-id="55a7e-234">The [Azure AD Graph API reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) provides details on each action, along with sample requests.</span></span>

## <a name="use-custom-attributes"></a><span data-ttu-id="55a7e-235">Använd anpassade attribut</span><span class="sxs-lookup"><span data-stu-id="55a7e-235">Use custom attributes</span></span>
<span data-ttu-id="55a7e-236">De flesta konsumenten program måste toostore någon typ av anpassad profil användarinformation.</span><span class="sxs-lookup"><span data-stu-id="55a7e-236">Most consumer applications need toostore some type of custom user profile information.</span></span> <span data-ttu-id="55a7e-237">Ett sätt som du kan göra detta är toodefine ett anpassat attribut i din B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="55a7e-237">One way you can do this is toodefine a custom attribute in your B2C tenant.</span></span> <span data-ttu-id="55a7e-238">Du kan sedan behandla den attributet hello samma sätt som du hanterar andra egenskapen på ett användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="55a7e-238">You can then treat that attribute hello same way you treat any other property on a user object.</span></span> <span data-ttu-id="55a7e-239">Du kan uppdatera hello attribut, ta bort hello attribut, fråga efter hello attribut skickar hello attribut som anspråk i inloggning-token och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="55a7e-239">You can update hello attribute, delete hello attribute, query by hello attribute, send hello attribute as a claim in sign-in tokens, and more.</span></span>

<span data-ttu-id="55a7e-240">toodefine ett anpassat attribut i din B2C-klient finns hello [referens för B2C-attributet](active-directory-b2c-reference-custom-attr.md).</span><span class="sxs-lookup"><span data-stu-id="55a7e-240">toodefine a custom attribute in your B2C tenant, see hello [B2C custom attribute reference](active-directory-b2c-reference-custom-attr.md).</span></span>

<span data-ttu-id="55a7e-241">Du kan visa hello anpassade attribut som definierats i din B2C-klient med hjälp av `B2CGraphClient`:</span><span class="sxs-lookup"><span data-stu-id="55a7e-241">You can view hello custom attributes defined in your B2C tenant by using `B2CGraphClient`:</span></span>

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

<span data-ttu-id="55a7e-242">hello utdata av dessa funktioner visar hello information om varje anpassade attribut som:</span><span class="sxs-lookup"><span data-stu-id="55a7e-242">hello output of these functions reveals hello details of each custom attribute, such as:</span></span>

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

<span data-ttu-id="55a7e-243">Du kan använda hello fullständigt namn som `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, som en egenskap på din användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="55a7e-243">You can use hello full name, such as `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, as a property on your user objects.</span></span>  <span data-ttu-id="55a7e-244">Uppdatera JSON-fil med nya hello-egenskapen och ett värde för egenskapen hello och kör sedan:</span><span class="sxs-lookup"><span data-stu-id="55a7e-244">Update your .json file with hello new property and a value for hello property, and then run:</span></span>

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

<span data-ttu-id="55a7e-245">Med hjälp av `B2CGraphClient`, du har ett tjänstprogram som kan hantera B2C-klient användarna programmässigt.</span><span class="sxs-lookup"><span data-stu-id="55a7e-245">By using `B2CGraphClient`, you have a service application that can manage your B2C tenant users programmatically.</span></span> <span data-ttu-id="55a7e-246">`B2CGraphClient`använder en egen programmet identitet tooauthenticate toohello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="55a7e-246">`B2CGraphClient` uses its own application identity tooauthenticate toohello Azure AD Graph API.</span></span> <span data-ttu-id="55a7e-247">Token får också med hjälp av en klienthemlighet.</span><span class="sxs-lookup"><span data-stu-id="55a7e-247">It also acquires tokens by using a client secret.</span></span> <span data-ttu-id="55a7e-248">Kom ihåg några viktiga punkter för B2C-appar som du använda den här funktionaliteten i ditt program:</span><span class="sxs-lookup"><span data-stu-id="55a7e-248">As you incorporate this functionality into your application, remember a few key points for B2C apps:</span></span>

* <span data-ttu-id="55a7e-249">Du behöver toogrant hello programmet hello rätt behörigheter i hello-klient.</span><span class="sxs-lookup"><span data-stu-id="55a7e-249">You need toogrant hello application hello proper permissions in hello tenant.</span></span>
* <span data-ttu-id="55a7e-250">Du måste toouse ADAL (inte MSAL) tooget åtkomsttoken för tillfället.</span><span class="sxs-lookup"><span data-stu-id="55a7e-250">For now, you need toouse ADAL (not MSAL) tooget access tokens.</span></span> <span data-ttu-id="55a7e-251">(Du kan också skicka protokollmeddelanden direkt, utan att använda ett bibliotek.)</span><span class="sxs-lookup"><span data-stu-id="55a7e-251">(You can also send protocol messages directly, without using a library.)</span></span>
* <span data-ttu-id="55a7e-252">När du anropar hello Graph API, Använd `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="55a7e-252">When you call hello Graph API, use `api-version=1.6`.</span></span>
* <span data-ttu-id="55a7e-253">När du skapar och uppdaterar konsumentanvändare krävs några egenskaper, enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="55a7e-253">When you create and update consumer users, a few properties are required, as described above.</span></span>

<span data-ttu-id="55a7e-254">Om du har några frågor eller begäran om åtgärder som du vill att tooperform med hello Graph API på din B2C-klient, lämna en kommentar på den här artikeln eller filen ett problem i hello GitHub exempel databasen.</span><span class="sxs-lookup"><span data-stu-id="55a7e-254">If you have any questions or requests for actions you would like tooperform by using hello Graph API on your B2C tenant, leave a comment on this article or file an issue in hello GitHub code sample repository.</span></span>

