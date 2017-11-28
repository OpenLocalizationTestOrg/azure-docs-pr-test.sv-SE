---
title: "Azure Active Directory B2C: Använda Graph API | Microsoft Docs"
description: "Så att anropa Graph API för en B2C-klient med hjälp av en programidentitet för att automatisera processen."
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
ms.openlocfilehash: c838fcad21875c4f813159ad78d4c87129a40a86
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-ad-b2c-use-the-graph-api"></a><span data-ttu-id="49f07-103">Azure AD B2C: Använda Graph API</span><span class="sxs-lookup"><span data-stu-id="49f07-103">Azure AD B2C: Use the Graph API</span></span>
<span data-ttu-id="49f07-104">Azure Active Directory (AD Azure) B2C hyresgäster tenderar att vara mycket stora.</span><span class="sxs-lookup"><span data-stu-id="49f07-104">Azure Active Directory (Azure AD) B2C tenants tend to be very large.</span></span> <span data-ttu-id="49f07-105">Det innebär att många vanliga klient administrativa uppgifter måste utföras via programmering.</span><span class="sxs-lookup"><span data-stu-id="49f07-105">This means that many common tenant management tasks need to be performed programmatically.</span></span> <span data-ttu-id="49f07-106">En primär exempel är användarhantering.</span><span class="sxs-lookup"><span data-stu-id="49f07-106">A primary example is user management.</span></span> <span data-ttu-id="49f07-107">Du kan behöva migrera en befintlig användararkivet till en B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="49f07-107">You might need to migrate an existing user store to a B2C tenant.</span></span> <span data-ttu-id="49f07-108">Du kanske vill vara värd för användarregistrering på sidan och skapa användarkonton i Azure AD B2C-katalogen i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="49f07-108">You may want to host user registration on your own page and create user accounts in your Azure AD B2C directory behind the scenes.</span></span> <span data-ttu-id="49f07-109">Dessa typer av uppgifter kräver möjlighet att skapa, läsa, uppdatera och ta bort användarkonton.</span><span class="sxs-lookup"><span data-stu-id="49f07-109">These types of tasks require the ability to create, read, update, and delete user accounts.</span></span> <span data-ttu-id="49f07-110">Du kan utföra dessa uppgifter med hjälp av Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="49f07-110">You can do these tasks by using the Azure AD Graph API.</span></span>

<span data-ttu-id="49f07-111">För B2C-innehavare finns det två primära lägen kommunicera med Graph API.</span><span class="sxs-lookup"><span data-stu-id="49f07-111">For B2C tenants, there are two primary modes of communicating with the Graph API.</span></span>

* <span data-ttu-id="49f07-112">För interaktiv, körs en gång aktiviteter, bör du fungerar som ett administratörskontot i B2C-klient när du utför uppgifterna.</span><span class="sxs-lookup"><span data-stu-id="49f07-112">For interactive, run-once tasks, you should act as an administrator account in the B2C tenant when you perform the tasks.</span></span> <span data-ttu-id="49f07-113">Det här läget kräver att en administratör att logga in med autentiseringsuppgifterna som administratören ska kunna utföra alla anrop för Graph API.</span><span class="sxs-lookup"><span data-stu-id="49f07-113">This mode requires an administrator to sign in with credentials before that admin can perform any calls to the Graph API.</span></span>
* <span data-ttu-id="49f07-114">Du bör använda någon typ av tjänstkontot som att du förser med den behörighet som krävs för att utföra hanteringsuppgifter för automatisk, kontinuerlig uppgifter.</span><span class="sxs-lookup"><span data-stu-id="49f07-114">For automated, continuous tasks, you should use some type of service account that you provide with the necessary privileges to perform management tasks.</span></span> <span data-ttu-id="49f07-115">Du kan göra detta genom att registrera ett program och autentisera till Azure AD i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="49f07-115">In Azure AD, you can do this by registering an application and authenticating to Azure AD.</span></span> <span data-ttu-id="49f07-116">Detta görs med hjälp av en **program-ID** som använder den [OAuth 2.0 klientens autentiseringsuppgifter bevilja](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api).</span><span class="sxs-lookup"><span data-stu-id="49f07-116">This is done by using an **Application ID** that uses the [OAuth 2.0 client credentials grant](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api).</span></span> <span data-ttu-id="49f07-117">I det här fallet fungerar programmet som själva inte som en användare att anropa Graph API.</span><span class="sxs-lookup"><span data-stu-id="49f07-117">In this case, the application acts as itself, not as a user, to call the Graph API.</span></span>

<span data-ttu-id="49f07-118">I den här artikeln diskuterar vi hur du utför automatiserade användningsfall.</span><span class="sxs-lookup"><span data-stu-id="49f07-118">In this article, we'll discuss how to perform the automated-use case.</span></span> <span data-ttu-id="49f07-119">För att demonstrera, ska vi skapa en .NET 4.5 `B2CGraphClient` som utför användare skapa, läsa, uppdatera och ta bort CRUD-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="49f07-119">To demonstrate, we'll build a .NET 4.5 `B2CGraphClient` that performs user create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="49f07-120">Klienten har en Windows-kommandoradsgränssnittet (CLI) som gör att du kan anropa olika metoder.</span><span class="sxs-lookup"><span data-stu-id="49f07-120">The client will have a Windows command-line interface (CLI) that allows you to invoke various methods.</span></span> <span data-ttu-id="49f07-121">Men är koden skriven ska bete sig i ett icke-interaktiv, automatiserat sätt.</span><span class="sxs-lookup"><span data-stu-id="49f07-121">However, the code is written to behave in a noninteractive, automated fashion.</span></span>

## <a name="get-an-azure-ad-b2c-tenant"></a><span data-ttu-id="49f07-122">Skaffa en Azure AD B2C-klient</span><span class="sxs-lookup"><span data-stu-id="49f07-122">Get an Azure AD B2C tenant</span></span>
<span data-ttu-id="49f07-123">Innan du kan skapa program eller användare eller interagera med Azure AD alls, behöver du en Azure AD B2C-klient och ett globalt administratörskonto i klienten.</span><span class="sxs-lookup"><span data-stu-id="49f07-123">Before you can create applications or users, or interact with Azure AD at all, you will need an Azure AD B2C tenant and a global administrator account in the tenant.</span></span> <span data-ttu-id="49f07-124">Om du inte redan har en klient [Kom igång med Azure AD B2C](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="49f07-124">If you don't have a tenant already, [get started with Azure AD B2C](active-directory-b2c-get-started.md).</span></span>

## <a name="register-your-application-in-your-tenant"></a><span data-ttu-id="49f07-125">Registrera ditt program i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="49f07-125">Register your application in your tenant</span></span>
<span data-ttu-id="49f07-126">När du har en B2C-klient, måste du registrera ditt program via den [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="49f07-126">After you have a B2C tenant, you need to register your application via the [Azure Portal](https://portal.azure.com).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="49f07-127">Om du vill använda Graph-API med din B2C-klient, måste du registrera en dedikerad program med hjälp av allmänna *App registreringar* bladet i Azure-Portal **inte** Azure AD B2C *program* bladet.</span><span class="sxs-lookup"><span data-stu-id="49f07-127">To use the Graph API with your B2C tenant, you will need to register a dedicated application by using the generic *App Registrations* blade in the Azure Portal, **NOT** Azure AD B2C's *Applications* blade.</span></span> <span data-ttu-id="49f07-128">Du kan inte återanvända de redan befintliga B2C-program som du har registrerat i Azure AD B2C *program* bladet.</span><span class="sxs-lookup"><span data-stu-id="49f07-128">You can't reuse the already-existing B2C applications that you registered in the Azure AD B2C's *Applications* blade.</span></span>

1. <span data-ttu-id="49f07-129">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="49f07-129">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="49f07-130">Välj din Azure AD B2C-klient genom att välja kontot i det övre högra hörnet på sidan.</span><span class="sxs-lookup"><span data-stu-id="49f07-130">Choose your Azure AD B2C tenant by selecting your account in the top right corner of the page.</span></span>
3. <span data-ttu-id="49f07-131">I det vänstra navigeringsfönstret väljer **fler tjänster**, klickar du på **App registreringar**, och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="49f07-131">In the left-hand navigation pane, choose **More Services**, click **App Registrations**, and click **Add**.</span></span>
4. <span data-ttu-id="49f07-132">Följ anvisningarna och skapa ett nytt program.</span><span class="sxs-lookup"><span data-stu-id="49f07-132">Follow the prompts and create a new application.</span></span> 
    1. <span data-ttu-id="49f07-133">Välj **Webbapp / API** typen av program.</span><span class="sxs-lookup"><span data-stu-id="49f07-133">Select **Web App / API** as the Application Type.</span></span>    
    2. <span data-ttu-id="49f07-134">Ange **någon omdirigerings-URI** (t.ex. https://B2CGraphAPI) eftersom den inte är relevanta för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="49f07-134">Provide **any redirect URI** (e.g. https://B2CGraphAPI) as it's not relevant for this example.</span></span>  
5. <span data-ttu-id="49f07-135">Det program kommer nu visas i listan med program, klickar du på den för att hämta den **program-ID** (även kallat klient-ID).</span><span class="sxs-lookup"><span data-stu-id="49f07-135">The application will now show up in the list of applications, click on it to obtain the **Application ID** (also known as Client ID).</span></span> <span data-ttu-id="49f07-136">Kopiera den som du behöver i ett senare avsnitt.</span><span class="sxs-lookup"><span data-stu-id="49f07-136">Copy it as you'll need it in a later section.</span></span>
6. <span data-ttu-id="49f07-137">I bladet inställningar klickar du på **nycklar** och lägga till en ny nyckel (även kallat klienthemlighet).</span><span class="sxs-lookup"><span data-stu-id="49f07-137">In the Settings blade, click on **Keys** and add a new key (also known as client secret).</span></span> <span data-ttu-id="49f07-138">Också kopiera den för användning i ett senare avsnitt.</span><span class="sxs-lookup"><span data-stu-id="49f07-138">Also copy it for use in a later section.</span></span>

## <a name="configure-create-read-and-update-permissions-for-your-application"></a><span data-ttu-id="49f07-139">Konfigurera skapa, läsa och uppdatera behörigheterna för ditt program</span><span class="sxs-lookup"><span data-stu-id="49f07-139">Configure create, read and update permissions for your application</span></span>
<span data-ttu-id="49f07-140">Nu måste du konfigurera ditt program att hämta alla behörigheter som krävs för att skapa, läsa, uppdatera och ta bort användare.</span><span class="sxs-lookup"><span data-stu-id="49f07-140">Now you need to configure your application to get all the required permissions to create, read, update and delete users.</span></span>

1. <span data-ttu-id="49f07-141">Fortsätter i Azure portal App registreringar bladet välj ditt program.</span><span class="sxs-lookup"><span data-stu-id="49f07-141">Continuing in the Azure portal's App Registrations blade, select your application.</span></span>
2. <span data-ttu-id="49f07-142">I bladet inställningar klickar du på **nödvändiga behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="49f07-142">In the Settings blade, click on **Required permissions**.</span></span>
3. <span data-ttu-id="49f07-143">I bladet behörighet klickar du på **Windows Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="49f07-143">In the Required permissions blade, click on **Windows Azure Active Directory**.</span></span>
4. <span data-ttu-id="49f07-144">I bladet Aktivera åtkomst väljer du den **läsning och skrivning katalogdata** tillstånd från **programbehörigheter** och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="49f07-144">In the Enable Access  blade, select the **Read and write directory data** permission from **Application Permissions** and click **Save**.</span></span>
5. <span data-ttu-id="49f07-145">Slutligen tillbaka i bladet behörighet klickar du på den **bevilja med** knappen.</span><span class="sxs-lookup"><span data-stu-id="49f07-145">Finally, back in the Required permissions blade, click on the **Grant Permissions** button.</span></span>

<span data-ttu-id="49f07-146">Nu har du ett program som har behörighet att skapa, läsa och uppdatera användare från din B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="49f07-146">You now have an application that has permission to create, read and update users from your B2C tenant.</span></span>

## <a name="configure-delete-permissions-for-your-application"></a><span data-ttu-id="49f07-147">Konfigurera behörighet att ta bort programmet</span><span class="sxs-lookup"><span data-stu-id="49f07-147">Configure delete permissions for your application</span></span>
<span data-ttu-id="49f07-148">För närvarande den *läsning och skrivning katalogdata* behörighet har **inte** omfattar möjligheten att göra eventuella borttagningar, till exempel ta bort användare.</span><span class="sxs-lookup"><span data-stu-id="49f07-148">Currently, the *Read and write directory data* permission does **NOT** include the ability to do any deletions such as deleting users.</span></span> <span data-ttu-id="49f07-149">Om du vill ge ditt program möjlighet att ta bort användare måste du utföra de här extra stegen som rör PowerShell, annars kan du gå till nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="49f07-149">If you want to give your application the ability to delete users, you'll need to do these extra steps that involve PowerShell, otherwise, you can skip to the next section.</span></span>

<span data-ttu-id="49f07-150">Först ladda ned och installera den [Microsoft Online Services-inloggningsassistent](http://go.microsoft.com/fwlink/?LinkID=286152).</span><span class="sxs-lookup"><span data-stu-id="49f07-150">First, download and install the [Microsoft Online Services Sign-In Assistant](http://go.microsoft.com/fwlink/?LinkID=286152).</span></span> <span data-ttu-id="49f07-151">Hämta och installera den [64-bitars Azure Active Directory-modulen för Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).</span><span class="sxs-lookup"><span data-stu-id="49f07-151">Then download and install the [64-bit Azure Active Directory module for Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).</span></span>

<span data-ttu-id="49f07-152">När du har installerat PowerShell-modulen öppna PowerShell och Anslut till din B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="49f07-152">After you install the PowerShell module, open PowerShell and connect to your B2C tenant.</span></span> <span data-ttu-id="49f07-153">När du har kört `Get-Credential`, uppmanas du att för ett användarnamn och lösenord, ange användarnamn och lösenord för administratörskontot din B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="49f07-153">After you run `Get-Credential`, you will be prompted for a user name and password, Enter the user name and password of your B2C tenant administrator account.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="49f07-154">Du måste använda ett administratörskonto för B2C-klient som är **lokala** till B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="49f07-154">You need to use a B2C tenant administrator account that is **local** to the B2C tenant.</span></span> <span data-ttu-id="49f07-155">Dessa konton se ut så här: myusername@myb2ctenant.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="49f07-155">These accounts look like this: myusername@myb2ctenant.onmicrosoft.com.</span></span>

```powershell
Connect-MsolService
```

<span data-ttu-id="49f07-156">Nu ska vi använda den **program-ID** i skriptet nedan för att tilldela användarens konto rollen administratör så att den att ta bort användare för programmet.</span><span class="sxs-lookup"><span data-stu-id="49f07-156">Now we'll use the **Application ID** in the script below to assign the application the user account administrator role which will allow it to delete users.</span></span> <span data-ttu-id="49f07-157">Dessa roller har välkända identifierare, så du behöver matas in din **program-ID** i skriptet nedan.</span><span class="sxs-lookup"><span data-stu-id="49f07-157">These roles have well-known identifiers, so all you need to do is input your **Application ID** in the script below.</span></span>

```powershell
$applicationId = "<YOUR_APPLICATION_ID>"
$sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
```

<span data-ttu-id="49f07-158">Ditt program nu har även behörighet att ta bort användare från din B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="49f07-158">Your application now also has permissions to delete users from your B2C tenant.</span></span>

## <a name="download-configure-and-build-the-sample-code"></a><span data-ttu-id="49f07-159">Hämta, konfigurera och skapa exempelkoden</span><span class="sxs-lookup"><span data-stu-id="49f07-159">Download, configure, and build the sample code</span></span>
<span data-ttu-id="49f07-160">Först hämta exempelkoden och få det körs.</span><span class="sxs-lookup"><span data-stu-id="49f07-160">First, download the sample code and get it running.</span></span> <span data-ttu-id="49f07-161">Sedan tar vi en närmare titt på den.</span><span class="sxs-lookup"><span data-stu-id="49f07-161">Then we will take a closer look at it.</span></span>  <span data-ttu-id="49f07-162">Du kan [hämta exempelkoden som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip).</span><span class="sxs-lookup"><span data-stu-id="49f07-162">You can [download the sample code as a .zip file](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip).</span></span> <span data-ttu-id="49f07-163">Du kan också klona den i en katalog som du väljer:</span><span class="sxs-lookup"><span data-stu-id="49f07-163">You can also clone it into a directory of your choice:</span></span>

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

<span data-ttu-id="49f07-164">Öppna den `B2CGraphClient\B2CGraphClient.sln` Visual Studio-lösning i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49f07-164">Open the `B2CGraphClient\B2CGraphClient.sln` Visual Studio solution in Visual Studio.</span></span> <span data-ttu-id="49f07-165">I den `B2CGraphClient` projekt, öppna filen `App.config`.</span><span class="sxs-lookup"><span data-stu-id="49f07-165">In the `B2CGraphClient` project, open the file `App.config`.</span></span> <span data-ttu-id="49f07-166">Ersätt tre appinställningarna med egna värden:</span><span class="sxs-lookup"><span data-stu-id="49f07-166">Replace the three app settings with your own values:</span></span>

```
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Key from above}" />
</appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

<span data-ttu-id="49f07-167">Högerklicka sedan på den `B2CGraphClient` lösningen och återskapa exemplet.</span><span class="sxs-lookup"><span data-stu-id="49f07-167">Next, right-click on the `B2CGraphClient` solution and rebuild the sample.</span></span> <span data-ttu-id="49f07-168">Om du lyckas kan du bör nu ha en `B2C.exe` körbar fil som finns i `B2CGraphClient\bin\Debug`.</span><span class="sxs-lookup"><span data-stu-id="49f07-168">If you are successful, you should now have a `B2C.exe` executable file located in `B2CGraphClient\bin\Debug`.</span></span>

## <a name="build-user-crud-operations-by-using-the-graph-api"></a><span data-ttu-id="49f07-169">Skapa användaren CRUD-åtgärder med hjälp av Graph-API</span><span class="sxs-lookup"><span data-stu-id="49f07-169">Build user CRUD operations by using the Graph API</span></span>
<span data-ttu-id="49f07-170">Om du vill använda B2CGraphClient, öppna en `cmd` Windows kommandot fråga och ändra katalogen till den `Debug` directory.</span><span class="sxs-lookup"><span data-stu-id="49f07-170">To use the B2CGraphClient, open a `cmd` Windows command prompt and change your directory to the `Debug` directory.</span></span> <span data-ttu-id="49f07-171">Kör sedan den `B2C Help` kommando.</span><span class="sxs-lookup"><span data-stu-id="49f07-171">Then run the `B2C Help` command.</span></span>

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

<span data-ttu-id="49f07-172">Då visas en kort beskrivning av varje kommando.</span><span class="sxs-lookup"><span data-stu-id="49f07-172">This will display a brief description of each command.</span></span> <span data-ttu-id="49f07-173">Varje gång du anropar någon av dessa kommandon `B2CGraphClient` gör en begäran till Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="49f07-173">Each time you invoke one of these commands, `B2CGraphClient` makes a request to the Azure AD Graph API.</span></span>

### <a name="get-an-access-token"></a><span data-ttu-id="49f07-174">Hämta en åtkomsttoken</span><span class="sxs-lookup"><span data-stu-id="49f07-174">Get an access token</span></span>
<span data-ttu-id="49f07-175">Alla förfrågningar för Graph API kräver en åtkomst-token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="49f07-175">Any request to the Graph API requires an access token for authentication.</span></span> <span data-ttu-id="49f07-176">`B2CGraphClient`använder den öppen källkod Active Directory Authentication Library (ADAL) för att få åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="49f07-176">`B2CGraphClient` uses the open-source Active Directory Authentication Library (ADAL) to help acquire access tokens.</span></span> <span data-ttu-id="49f07-177">ADAL underlättar token förvärv genom att tillhandahålla ett enkelt API och tar hand om vissa viktig information, till exempel cachelagring åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="49f07-177">ADAL makes token acquisition easier by providing a simple API and taking care of some important details, such as caching access tokens.</span></span> <span data-ttu-id="49f07-178">Du inte använder ADAL för att hämta token, men.</span><span class="sxs-lookup"><span data-stu-id="49f07-178">You don't have to use ADAL to get tokens, though.</span></span> <span data-ttu-id="49f07-179">Du kan också hämta token genom att utforma HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="49f07-179">You can also get tokens by crafting HTTP requests.</span></span>

> [!NOTE]
> <span data-ttu-id="49f07-180">Det här kodexemplet använder ADAL v2 för att kunna kommunicera med Graph API.</span><span class="sxs-lookup"><span data-stu-id="49f07-180">This code sample uses ADAL v2 in order to communicate with the Graph API.</span></span>  <span data-ttu-id="49f07-181">Du måste använda ADAL version 2 eller 3 för att få åtkomst-token som kan användas med Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="49f07-181">You must use ADAL v2 or v3 in order to get access tokens which can be used with the Azure AD Graph API.</span></span>
> 
> 

<span data-ttu-id="49f07-182">När `B2CGraphClient` körs, skapas en instans av den `B2CGraphClient` klass.</span><span class="sxs-lookup"><span data-stu-id="49f07-182">When `B2CGraphClient` runs, it creates an instance of the `B2CGraphClient` class.</span></span> <span data-ttu-id="49f07-183">Konstruktören för den här klassen ställer in en scaffold-teknik för ADAL-autentisering:</span><span class="sxs-lookup"><span data-stu-id="49f07-183">The constructor for this class sets up an ADAL authentication scaffolding:</span></span>

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

<span data-ttu-id="49f07-184">Vi använder den `B2C Get-User` kommandot som exempel.</span><span class="sxs-lookup"><span data-stu-id="49f07-184">We'll use the `B2C Get-User` command as an example.</span></span> <span data-ttu-id="49f07-185">När `B2C Get-User` anropas utan några ytterligare indata CLI-anrop i `B2CGraphClient.GetAllUsers(...)` metod.</span><span class="sxs-lookup"><span data-stu-id="49f07-185">When `B2C Get-User` is invoked without any additional inputs, the CLI calls the `B2CGraphClient.GetAllUsers(...)` method.</span></span> <span data-ttu-id="49f07-186">Den här metoden anropar `B2CGraphClient.SendGraphGetRequest(...)`, som skickar en HTTP GET-begäran för Graph API.</span><span class="sxs-lookup"><span data-stu-id="49f07-186">This method calls `B2CGraphClient.SendGraphGetRequest(...)`, which submits an HTTP GET request to the Graph API.</span></span> <span data-ttu-id="49f07-187">Innan `B2CGraphClient.SendGraphGetRequest(...)` skickar GET-begäran, först hämtar en åtkomst-token med hjälp av ADAL:</span><span class="sxs-lookup"><span data-stu-id="49f07-187">Before `B2CGraphClient.SendGraphGetRequest(...)` sends the GET request, it first gets an access token by using ADAL:</span></span>

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

<span data-ttu-id="49f07-188">Du kan hämta ett åtkomsttoken för Graph API genom att anropa ADAL `AuthenticationContext.AcquireToken(...)` metod.</span><span class="sxs-lookup"><span data-stu-id="49f07-188">You can get an access token for the Graph API by calling the ADAL `AuthenticationContext.AcquireToken(...)` method.</span></span> <span data-ttu-id="49f07-189">ADAL returnerar en `access_token` som representerar programmets identitet.</span><span class="sxs-lookup"><span data-stu-id="49f07-189">ADAL then returns an `access_token` that represents the application's identity.</span></span>

### <a name="read-users"></a><span data-ttu-id="49f07-190">Läs användare</span><span class="sxs-lookup"><span data-stu-id="49f07-190">Read users</span></span>
<span data-ttu-id="49f07-191">När du vill hämta en lista med användare eller hämta en viss användare från Graph API kan du skicka en HTTP `GET` begäran om att den `/users` slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="49f07-191">When you want to get a list of users or get a particular user from the Graph API, you can send an HTTP `GET` request to the `/users` endpoint.</span></span> <span data-ttu-id="49f07-192">En begäran för alla användare i en klient som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="49f07-192">A request for all of the users in a tenant looks like this:</span></span>

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

<span data-ttu-id="49f07-193">Om du vill se den här begäran, kör du:</span><span class="sxs-lookup"><span data-stu-id="49f07-193">To see this request, run:</span></span>

 ```
 > B2C Get-User
 ```

<span data-ttu-id="49f07-194">Det finns två viktiga saker att tänka på:</span><span class="sxs-lookup"><span data-stu-id="49f07-194">There are two important things to note:</span></span>

* <span data-ttu-id="49f07-195">Åtkomsttoken har köpt via ADAL har lagts till i den `Authorization` sidhuvud med hjälp av den `Bearer` schema.</span><span class="sxs-lookup"><span data-stu-id="49f07-195">The access token acquired via ADAL is added to the `Authorization` header by using the `Bearer` scheme.</span></span>
* <span data-ttu-id="49f07-196">För B2C-innehavare, måste du använda Frågeparametern `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="49f07-196">For B2C tenants, you must use the query parameter `api-version=1.6`.</span></span>

<span data-ttu-id="49f07-197">Båda dessa uppgifter hanteras i den `B2CGraphClient.SendGraphGetRequest(...)` metoden:</span><span class="sxs-lookup"><span data-stu-id="49f07-197">Both of these details are handled in the `B2CGraphClient.SendGraphGetRequest(...)` method:</span></span>

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a><span data-ttu-id="49f07-198">Skapa användarkonton för konsumenten</span><span class="sxs-lookup"><span data-stu-id="49f07-198">Create consumer user accounts</span></span>
<span data-ttu-id="49f07-199">När du skapar användarkonton i din B2C-klient kan du skicka en HTTP `POST` begäran om att den `/users` slutpunkt:</span><span class="sxs-lookup"><span data-stu-id="49f07-199">When you create user accounts in your B2C tenant, you can send an HTTP `POST` request to the `/users` endpoint:</span></span>

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",                // a value that can be used for displaying to the end user
    "mailNickname": "joec",                        // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

<span data-ttu-id="49f07-200">De flesta av de här egenskaperna i den här begäran krävs för att skapa konsumentanvändare.</span><span class="sxs-lookup"><span data-stu-id="49f07-200">Most of these properties in this request are required to create consumer users.</span></span> <span data-ttu-id="49f07-201">Mer information klickar du på [här](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser).</span><span class="sxs-lookup"><span data-stu-id="49f07-201">To learn more, click [here](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser).</span></span> <span data-ttu-id="49f07-202">Observera att den `//` kommentarer har inkluderats för bilden.</span><span class="sxs-lookup"><span data-stu-id="49f07-202">Note that the `//` comments have been included for illustration.</span></span> <span data-ttu-id="49f07-203">Ta inte med dem i en faktiska begäran.</span><span class="sxs-lookup"><span data-stu-id="49f07-203">Do not include them in an actual request.</span></span>

<span data-ttu-id="49f07-204">Om du vill se begäran, kör du något av följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="49f07-204">To see the request, run one of the following commands:</span></span>

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

<span data-ttu-id="49f07-205">Den `Create-User` kommandot tar emot en JSON-fil som indataparameter.</span><span class="sxs-lookup"><span data-stu-id="49f07-205">The `Create-User` command takes a .json file as an input parameter.</span></span> <span data-ttu-id="49f07-206">Innehåller en JSON-representation av ett användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="49f07-206">This contains a JSON representation of a user object.</span></span> <span data-ttu-id="49f07-207">Det finns två exempel JSON-filer i exempelkoden: `usertemplate-email.json` och `usertemplate-username.json`.</span><span class="sxs-lookup"><span data-stu-id="49f07-207">There are two sample .json files in the sample code: `usertemplate-email.json` and `usertemplate-username.json`.</span></span> <span data-ttu-id="49f07-208">Du kan ändra de här filerna så att de passar dina behov.</span><span class="sxs-lookup"><span data-stu-id="49f07-208">You can modify these files to suit your needs.</span></span> <span data-ttu-id="49f07-209">Förutom de obligatoriska fälten ovan ingår flera valfria fälten som du kan använda i dessa filer.</span><span class="sxs-lookup"><span data-stu-id="49f07-209">In addition to the required fields above, several optional fields that you can use are included in these files.</span></span> <span data-ttu-id="49f07-210">Information om de valfria fälten kan hittas i den [Azure AD Graph API Entitetsreferens](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).</span><span class="sxs-lookup"><span data-stu-id="49f07-210">Details on the optional fields can be found in the [Azure AD Graph API entity reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).</span></span>

<span data-ttu-id="49f07-211">Du kan se hur POST-begäran har skapats i `B2CGraphClient.SendGraphPostRequest(...)`.</span><span class="sxs-lookup"><span data-stu-id="49f07-211">You can see how the POST request is constructed in `B2CGraphClient.SendGraphPostRequest(...)`.</span></span>

* <span data-ttu-id="49f07-212">En åtkomsttoken att bifogas i `Authorization` huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="49f07-212">It attaches an access token to the `Authorization` header of the request.</span></span>
* <span data-ttu-id="49f07-213">Anger `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="49f07-213">It sets `api-version=1.6`.</span></span>
* <span data-ttu-id="49f07-214">Brödtexten i begäran innehåller JSON användarobjektet.</span><span class="sxs-lookup"><span data-stu-id="49f07-214">It includes the JSON user object in the body of the request.</span></span>

> [!NOTE]
> <span data-ttu-id="49f07-215">Om de konton som du vill migrera från en befintlig användararkivet har lägre lösenordssäkerhet än den [starka lösenord tillämpas av Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), kan du inaktivera krav på starkt lösenord med den `DisableStrongPassword` värde i den `passwordPolicies` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="49f07-215">If the accounts that you want to migrate from an existing user store has lower password strength than the [strong password strength enforced by Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), you can disable the strong password requirement using the `DisableStrongPassword` value in the `passwordPolicies` property.</span></span> <span data-ttu-id="49f07-216">Exempelvis kan du ändra användarens begäran om att skapa som angetts ovan på följande sätt: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.</span><span class="sxs-lookup"><span data-stu-id="49f07-216">For instance, you can modify the create user request provided above as follows: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.</span></span>
> 
> 

### <a name="update-consumer-user-accounts"></a><span data-ttu-id="49f07-217">Uppdatera konsumenten användarkonton</span><span class="sxs-lookup"><span data-stu-id="49f07-217">Update consumer user accounts</span></span>
<span data-ttu-id="49f07-218">När du uppdaterar användarobjekt liknar processen det du använder för att skapa användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="49f07-218">When you update user objects, the process is similar to the one you use to create user objects.</span></span> <span data-ttu-id="49f07-219">Men den här processen använder HTTP `PATCH` metoden:</span><span class="sxs-lookup"><span data-stu-id="49f07-219">But this process uses the HTTP `PATCH` method:</span></span>

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",                // this request updates only the user's displayName
}
```

<span data-ttu-id="49f07-220">Försök att uppdatera en användare genom att uppdatera JSON-filer med nya data.</span><span class="sxs-lookup"><span data-stu-id="49f07-220">Try to update a user by updating your JSON files with new data.</span></span> <span data-ttu-id="49f07-221">Du kan sedan använda `B2CGraphClient` att köra något av följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="49f07-221">You can then use `B2CGraphClient` to run one of these commands:</span></span>

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

<span data-ttu-id="49f07-222">Kontrollera den `B2CGraphClient.SendGraphPatchRequest(...)` metod för information om hur du skickar denna begäran.</span><span class="sxs-lookup"><span data-stu-id="49f07-222">Inspect the `B2CGraphClient.SendGraphPatchRequest(...)` method for details on how to send this request.</span></span>

### <a name="search-users"></a><span data-ttu-id="49f07-223">Söka efter användare</span><span class="sxs-lookup"><span data-stu-id="49f07-223">Search users</span></span>
<span data-ttu-id="49f07-224">Du kan söka efter användare i din B2C-klient på ett par olika sätt.</span><span class="sxs-lookup"><span data-stu-id="49f07-224">You can search for users in your B2C tenant in a couple of ways.</span></span> <span data-ttu-id="49f07-225">En med hjälp av användarens objekt-ID eller två, med hjälp av användarens inloggning Identiferare (d.v.s. de `signInNames` egenskap).</span><span class="sxs-lookup"><span data-stu-id="49f07-225">One, using the user's object ID or two, using the user's sign-in identifer (i.e., the `signInNames` property).</span></span>

<span data-ttu-id="49f07-226">Kör något av följande kommandon för att söka efter en viss användare:</span><span class="sxs-lookup"><span data-stu-id="49f07-226">Run one of the following commands to search for a specific user:</span></span>

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

<span data-ttu-id="49f07-227">Här följer några exempel:</span><span class="sxs-lookup"><span data-stu-id="49f07-227">Here are a couple of examples:</span></span>

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a><span data-ttu-id="49f07-228">Ta bort användare</span><span class="sxs-lookup"><span data-stu-id="49f07-228">Delete users</span></span>
<span data-ttu-id="49f07-229">Processen för att ta bort en användare är enkelt.</span><span class="sxs-lookup"><span data-stu-id="49f07-229">The process for deleting a user is straightforward.</span></span> <span data-ttu-id="49f07-230">Använda HTTP `DELETE` metoden och konstruktion URL med rätt objekt-ID:</span><span class="sxs-lookup"><span data-stu-id="49f07-230">Use the HTTP `DELETE` method and construct the URL with the correct object ID:</span></span>

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

<span data-ttu-id="49f07-231">Anger det här kommandot och visa begäran om borttagning som skrivs ut på konsolen om du vill se ett exempel:</span><span class="sxs-lookup"><span data-stu-id="49f07-231">To see an example, enter this command and view the delete request that is printed to the console:</span></span>

```
> B2C Delete-User <object-id-of-user>
```

<span data-ttu-id="49f07-232">Kontrollera den `B2CGraphClient.SendGraphDeleteRequest(...)` metod för information om hur du skickar denna begäran.</span><span class="sxs-lookup"><span data-stu-id="49f07-232">Inspect the `B2CGraphClient.SendGraphDeleteRequest(...)` method for details on how to send this request.</span></span>

<span data-ttu-id="49f07-233">Du kan utföra många andra åtgärder med Azure AD Graph API förutom användarhantering.</span><span class="sxs-lookup"><span data-stu-id="49f07-233">You can perform many other actions with the Azure AD Graph API in addition to user management.</span></span> <span data-ttu-id="49f07-234">Den [Azure AD Graph API-referens](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) innehåller information om varje åtgärd, tillsammans med exempel begäranden.</span><span class="sxs-lookup"><span data-stu-id="49f07-234">The [Azure AD Graph API reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) provides details on each action, along with sample requests.</span></span>

## <a name="use-custom-attributes"></a><span data-ttu-id="49f07-235">Använd anpassade attribut</span><span class="sxs-lookup"><span data-stu-id="49f07-235">Use custom attributes</span></span>
<span data-ttu-id="49f07-236">De flesta konsumenten program behöver lagra någon typ av anpassad profil användarinformation.</span><span class="sxs-lookup"><span data-stu-id="49f07-236">Most consumer applications need to store some type of custom user profile information.</span></span> <span data-ttu-id="49f07-237">Ett sätt som du kan göra detta är att definiera ett anpassat attribut i din B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="49f07-237">One way you can do this is to define a custom attribute in your B2C tenant.</span></span> <span data-ttu-id="49f07-238">Du kan sedan hantera attributet på samma sätt som du hanterar andra egenskapen på ett användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="49f07-238">You can then treat that attribute the same way you treat any other property on a user object.</span></span> <span data-ttu-id="49f07-239">Du kan uppdatera attributet, ta bort attributet, fråga med attributet, skicka attributet som ett anspråk i inloggning-token och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="49f07-239">You can update the attribute, delete the attribute, query by the attribute, send the attribute as a claim in sign-in tokens, and more.</span></span>

<span data-ttu-id="49f07-240">Om du vill definiera ett anpassat attribut i din B2C-klient, finns det [referens för B2C-attributet](active-directory-b2c-reference-custom-attr.md).</span><span class="sxs-lookup"><span data-stu-id="49f07-240">To define a custom attribute in your B2C tenant, see the [B2C custom attribute reference](active-directory-b2c-reference-custom-attr.md).</span></span>

<span data-ttu-id="49f07-241">Du kan visa de anpassade attribut som definierats i din B2C-klient med hjälp av `B2CGraphClient`:</span><span class="sxs-lookup"><span data-stu-id="49f07-241">You can view the custom attributes defined in your B2C tenant by using `B2CGraphClient`:</span></span>

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

<span data-ttu-id="49f07-242">Resultatet av dessa funktioner visar information om varje anpassade attribut som:</span><span class="sxs-lookup"><span data-stu-id="49f07-242">The output of these functions reveals the details of each custom attribute, such as:</span></span>

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

<span data-ttu-id="49f07-243">Du kan använda det fullständiga namnet som `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, som en egenskap på din användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="49f07-243">You can use the full name, such as `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, as a property on your user objects.</span></span>  <span data-ttu-id="49f07-244">Uppdatera JSON-fil med den nya egenskapen och ett värde för egenskapen och kör sedan:</span><span class="sxs-lookup"><span data-stu-id="49f07-244">Update your .json file with the new property and a value for the property, and then run:</span></span>

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

<span data-ttu-id="49f07-245">Med hjälp av `B2CGraphClient`, du har ett tjänstprogram som kan hantera B2C-klient användarna programmässigt.</span><span class="sxs-lookup"><span data-stu-id="49f07-245">By using `B2CGraphClient`, you have a service application that can manage your B2C tenant users programmatically.</span></span> <span data-ttu-id="49f07-246">`B2CGraphClient`använder en egen programidentitet för att autentisera till Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="49f07-246">`B2CGraphClient` uses its own application identity to authenticate to the Azure AD Graph API.</span></span> <span data-ttu-id="49f07-247">Token får också med hjälp av en klienthemlighet.</span><span class="sxs-lookup"><span data-stu-id="49f07-247">It also acquires tokens by using a client secret.</span></span> <span data-ttu-id="49f07-248">Kom ihåg några viktiga punkter för B2C-appar som du använda den här funktionaliteten i ditt program:</span><span class="sxs-lookup"><span data-stu-id="49f07-248">As you incorporate this functionality into your application, remember a few key points for B2C apps:</span></span>

* <span data-ttu-id="49f07-249">Du måste ge rätt behörighet i klienten för programmet.</span><span class="sxs-lookup"><span data-stu-id="49f07-249">You need to grant the application the proper permissions in the tenant.</span></span>
* <span data-ttu-id="49f07-250">Du måste använda ADAL (inte MSAL) för att få åtkomst-token för tillfället.</span><span class="sxs-lookup"><span data-stu-id="49f07-250">For now, you need to use ADAL (not MSAL) to get access tokens.</span></span> <span data-ttu-id="49f07-251">(Du kan också skicka protokollmeddelanden direkt, utan att använda ett bibliotek.)</span><span class="sxs-lookup"><span data-stu-id="49f07-251">(You can also send protocol messages directly, without using a library.)</span></span>
* <span data-ttu-id="49f07-252">När du anropar Graph API, Använd `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="49f07-252">When you call the Graph API, use `api-version=1.6`.</span></span>
* <span data-ttu-id="49f07-253">När du skapar och uppdaterar konsumentanvändare krävs några egenskaper, enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="49f07-253">When you create and update consumer users, a few properties are required, as described above.</span></span>

<span data-ttu-id="49f07-254">Om du har några frågor eller begäran om åtgärder som du vill utföra med hjälp av Graph API på din B2C-klient, lämna en kommentar på den här artikeln eller filen ett problem i GitHub-lagringsplatsen kod exempel.</span><span class="sxs-lookup"><span data-stu-id="49f07-254">If you have any questions or requests for actions you would like to perform by using the Graph API on your B2C tenant, leave a comment on this article or file an issue in the GitHub code sample repository.</span></span>

