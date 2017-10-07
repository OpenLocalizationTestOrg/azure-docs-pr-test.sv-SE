---
title: aaaAzure Active Directory B2C | Microsoft Docs
description: "Hur profile toobuild ett webbprogram som har sign-upp/inloggning, redigera och lösenordsåterställning med hjälp av Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: barbaraselden
ms.assetid: 30261336-d7a5-4a6d-8c1a-7943ad76ed25
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: 187f99a8dd50d212de4f0517f552cdbbe5a8edf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-web-app-with-azure-active-directory-b2c-sign-up-sign-in-profile-edit-and-password-reset"></a><span data-ttu-id="1b1c7-103">Skapa en ASP.NET-webbapp med Azure Active Directory B2C profil för registrering, inloggning, redigera och återställning av lösenord</span><span class="sxs-lookup"><span data-stu-id="1b1c7-103">Create an ASP.NET web app with Azure Active Directory B2C sign-up, sign-in, profile edit, and password reset</span></span>

<span data-ttu-id="1b1c7-104">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="1b1c7-104">This tutorial shows you how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1b1c7-105">Lägg till Azure AD B2C identitet funktioner tooyour webbapp</span><span class="sxs-lookup"><span data-stu-id="1b1c7-105">Add Azure AD B2C identity features tooyour web app</span></span>
> * <span data-ttu-id="1b1c7-106">Registrera ditt webbprogram i din Azure AD B2C-katalog</span><span class="sxs-lookup"><span data-stu-id="1b1c7-106">Register your web app in your Azure AD B2C directory</span></span>
> * <span data-ttu-id="1b1c7-107">Skapa en användare sign-upp/inloggning, Redigera profil och principen för lösenordsåterställning för ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="1b1c7-107">Create a user sign-up/sign-in, profile edit, and password reset policy for your web app</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1b1c7-108">Krav</span><span class="sxs-lookup"><span data-stu-id="1b1c7-108">Prerequisites</span></span>

- <span data-ttu-id="1b1c7-109">Du måste ansluta din B2C-klient tooan Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-109">You must connect your B2C Tenant tooan Azure account.</span></span> <span data-ttu-id="1b1c7-110">Du kan skapa ett kostnadsfritt Azure-konto [här](https://azure.microsoft.com/en-us/).</span><span class="sxs-lookup"><span data-stu-id="1b1c7-110">You can create a free Azure account [here](https://azure.microsoft.com/en-us/).</span></span>
- <span data-ttu-id="1b1c7-111">Du behöver [Microsoft Visual Studio](https://www.visualstudio.com/) eller liknande program tooview och ändra hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-111">You need [Microsoft Visual Studio](https://www.visualstudio.com/) or a similar program tooview and modify hello sample code.</span></span>

## <a name="create-an-azure-ad-b2c-directory"></a><span data-ttu-id="1b1c7-112">Skapa en Azure AD B2C-katalog</span><span class="sxs-lookup"><span data-stu-id="1b1c7-112">Create an Azure AD B2C directory</span></span>

<span data-ttu-id="1b1c7-113">Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-113">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="1b1c7-114">En katalog är en behållare för alla användare, appar, grupper och mer.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-114">A directory is a container for all your users, apps, groups, and more.</span></span> <span data-ttu-id="1b1c7-115">Om du inte har någon redan skapar du en B2C-katalog innan du fortsätter i den här guiden.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-115">If you don't have one already, create a B2C directory before you continue in this guide.</span></span>

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

> [!NOTE]
> 
> <span data-ttu-id="1b1c7-116">Du måste tooconnect hello B2C-klient tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-116">You need tooconnect hello B2C Tenant tooyour Azure subscription.</span></span> <span data-ttu-id="1b1c7-117">När du har valt **skapa**väljer hello **länka ett befintligt Azure AD B2C-klient toomy Azure-prenumeration** alternativet, och klicka sedan på hello **Azure AD B2C-klient** listrutan, Välj hello-klient som du vill tooassociate.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-117">After selecting **Create**, select hello **Link an existing Azure AD B2C Tenant toomy Azure subscription** option, and then in hello **Azure AD B2C Tenant** drop down, select hello tenant you want tooassociate.</span></span>

## <a name="create-and-register-an-application"></a><span data-ttu-id="1b1c7-118">Skapa och registrera ett program</span><span class="sxs-lookup"><span data-stu-id="1b1c7-118">Create and register an application</span></span>

<span data-ttu-id="1b1c7-119">Sedan måste toocreate och registrera hello app i B2C-katalogen.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-119">Next, you need toocreate and register hello app in your B2C directory.</span></span> <span data-ttu-id="1b1c7-120">Detta ger information att Azure AD B2C måste toosecurely kommunicera med din app.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-120">This provides information that Azure AD B2C needs toosecurely communicate with your app.</span></span> 

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

<span data-ttu-id="1b1c7-121">När du är klar har du både en API och det ursprungliga programmet i inställningarna för programmet.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-121">When you are done, you will have both an API and a native application in your application settings.</span></span>

## <a name="create-policies-on-your-b2c-tenant"></a><span data-ttu-id="1b1c7-122">Skapa principer på din B2C-klient</span><span class="sxs-lookup"><span data-stu-id="1b1c7-122">Create policies on your B2C tenant</span></span>

<span data-ttu-id="1b1c7-123">I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="1b1c7-123">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="1b1c7-124">Det här kodexemplet innehåller tre identitetsupplevelser: **registrering och inloggning i**, **profilen redigera**, och **lösenordsåterställning**.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-124">This code sample contains three identity experiences: **sign-up & sign-in**, **profile edit**, and **password reset**.</span></span>  <span data-ttu-id="1b1c7-125">Du behöver toocreate en princip av varje typ, enligt beskrivningen i hello [referensartikeln om principer](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="1b1c7-125">You need toocreate one policy of each type, as described in hello [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="1b1c7-126">Vara säker på att tooselect hello visa name-attribut eller anspråk och toocopy ned hello namnet på din princip för senare användning för varje princip.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-126">For each policy, be sure tooselect hello Display name attribute or claim, and toocopy down hello name of your policy for later use.</span></span>

### <a name="add-your-identity-providers"></a><span data-ttu-id="1b1c7-127">Lägg till din identitetsleverantörer</span><span class="sxs-lookup"><span data-stu-id="1b1c7-127">Add your identity providers</span></span>

<span data-ttu-id="1b1c7-128">Välj inställningarna för **identitetsleverantörer** och välj signup användarnamn eller e-registreringen.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-128">From your settings, select **Identity Providers** and choose Username signup or Email signup.</span></span>

### <a name="create-a-sign-up-and-sign-in-policy"></a><span data-ttu-id="1b1c7-129">Skapa en princip för registrering och inloggning</span><span class="sxs-lookup"><span data-stu-id="1b1c7-129">Create a sign-up and sign-in policy</span></span>

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

### <a name="create-a-profile-editing-policy"></a><span data-ttu-id="1b1c7-130">Skapa en profil Redigera princip</span><span class="sxs-lookup"><span data-stu-id="1b1c7-130">Create a profile editing policy</span></span>

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

### <a name="create-a-password-reset-policy"></a><span data-ttu-id="1b1c7-131">Skapa en princip för lösenordsåterställning</span><span class="sxs-lookup"><span data-stu-id="1b1c7-131">Create a password reset policy</span></span>

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

<span data-ttu-id="1b1c7-132">När du har skapat dina principer du är klar toobuild din app.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-132">After you create your policies, you're ready toobuild your app.</span></span>

## <a name="download-hello-sample-code"></a><span data-ttu-id="1b1c7-133">Hämta hello exempelkod</span><span class="sxs-lookup"><span data-stu-id="1b1c7-133">Download hello sample code</span></span>

<span data-ttu-id="1b1c7-134">hello-koden för den här självstudiekursen underhålls på [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span><span class="sxs-lookup"><span data-stu-id="1b1c7-134">hello code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="1b1c7-135">Du kan klona hello exemplet genom att köra:</span><span class="sxs-lookup"><span data-stu-id="1b1c7-135">You can clone hello sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="1b1c7-136">När du har hämtat hello exempelkod igång öppna hello Visual Studio SLN-filen tooget.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-136">After you download hello sample code, open hello Visual Studio .sln file tooget started.</span></span> <span data-ttu-id="1b1c7-137">hello lösningsfilen innehåller två projekt: `TaskWebApp` och `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-137">hello solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="1b1c7-138">`TaskWebApp`är hello MVC-webbapp som hello användaren interagerar med.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-138">`TaskWebApp` is hello MVC web application that hello user interacts with.</span></span> <span data-ttu-id="1b1c7-139">`TaskService`är hello appens backend-webb-API som lagrar varje användares att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-139">`TaskService` is hello app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="1b1c7-140">Den här artikeln handlar enbart hello `TaskWebApp` program.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-140">This article will only discuss hello `TaskWebApp` application.</span></span> <span data-ttu-id="1b1c7-141">toolearn hur toobuild `TaskService` med Azure AD B2C finns i [våra självstudier för .NET web api](active-directory-b2c-devquickstarts-api-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="1b1c7-141">toolearn how toobuild `TaskService` using Azure AD B2C, see [our .NET web api tutorial](active-directory-b2c-devquickstarts-api-dotnet.md).</span></span>

## <a name="update-code-toouse-your-tenant-and-policies"></a><span data-ttu-id="1b1c7-142">Uppdatera koden toouse klient- och principer</span><span class="sxs-lookup"><span data-stu-id="1b1c7-142">Update code toouse your tenant and policies</span></span>

<span data-ttu-id="1b1c7-143">Exemplet är konfigurerade toouse hello principer och klient-ID för vår demo-klient.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-143">Our sample is configured toouse hello policies and client ID of our demo tenant.</span></span> <span data-ttu-id="1b1c7-144">tooconnect den egna tooyour-klient behöver du tooopen `web.config` i hello `TaskWebApp` projektet och Ersätt hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="1b1c7-144">tooconnect it tooyour own tenant, you need tooopen `web.config` in hello `TaskWebApp` project and replace hello following values:</span></span>

* <span data-ttu-id="1b1c7-145">`ida:Tenant` med namnet på din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="1b1c7-145">`ida:Tenant` with your tenant name</span></span>
* <span data-ttu-id="1b1c7-146">`ida:ClientId` med program-ID:t för din webbapp</span><span class="sxs-lookup"><span data-stu-id="1b1c7-146">`ida:ClientId` with your web app application ID</span></span>
* <span data-ttu-id="1b1c7-147">`ida:ClientSecret` med den hemliga nyckeln för din webbapp</span><span class="sxs-lookup"><span data-stu-id="1b1c7-147">`ida:ClientSecret` with your web app secret key</span></span>
* <span data-ttu-id="1b1c7-148">`ida:SignUpSignInPolicyId` med namnet på din registrerings- eller inloggningsprincip</span><span class="sxs-lookup"><span data-stu-id="1b1c7-148">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
* <span data-ttu-id="1b1c7-149">`ida:EditProfilePolicyId` med namnet på din profilredigeringsprincip</span><span class="sxs-lookup"><span data-stu-id="1b1c7-149">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
* <span data-ttu-id="1b1c7-150">`ida:ResetPasswordPolicyId` med namnet på din lösenordsåterställningsprincip</span><span class="sxs-lookup"><span data-stu-id="1b1c7-150">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>

## <a name="launch-hello-app"></a><span data-ttu-id="1b1c7-151">Starta hello-appen</span><span class="sxs-lookup"><span data-stu-id="1b1c7-151">Launch hello app</span></span>
<span data-ttu-id="1b1c7-152">Starta hello appen från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-152">From within Visual Studio, launch hello app.</span></span> <span data-ttu-id="1b1c7-153">Navigera toohello uppgiftslista fliken och Observera hello URL: en är: https://login.microsoftonline.com/*YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*& client_id = *YourclientID*...</span><span class="sxs-lookup"><span data-stu-id="1b1c7-153">Navigate toohello To-Do List tab, and note hello URl is: https://login.microsoftonline.com/*YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*&client_id=*YourclientID*.....</span></span>

<span data-ttu-id="1b1c7-154">Registrera dig för hello appen genom att använda din e-postadress eller användaren namn.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-154">Sign up for hello app by using your email address or user name.</span></span> <span data-ttu-id="1b1c7-155">Logga ut, och sedan logga in igen och profilredigering hello eller återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-155">Sign out, then sign in again and edit hello profile or reset hello password.</span></span> <span data-ttu-id="1b1c7-156">Logga ut och logga in som en annan användare.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-156">Sign out and sign in as a different user.</span></span> 

## <a name="add-social-idps"></a><span data-ttu-id="1b1c7-157">Lägg till sociala IDPs</span><span class="sxs-lookup"><span data-stu-id="1b1c7-157">Add social IDPs</span></span>

<span data-ttu-id="1b1c7-158">För närvarande hello app stöder bara användaren registrering och inloggning med hjälp av **lokala konton**; konton som lagras i din B2C-katalog som använder ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-158">Currently, hello app supports only user sign-up and sign-in by using **local accounts**; accounts stored in your B2C directory that use a user name and password.</span></span> <span data-ttu-id="1b1c7-159">Med hjälp av Azure AD B2C kan du lägga till stöd för andra **identitetsleverantörer** (IDPs) utan att ändra någon av din kod.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-159">By using Azure AD B2C, you can add support for other **identity providers** (IDPs) without changing any of your code.</span></span>

<span data-ttu-id="1b1c7-160">tooadd sociala IDPs tooyour appen, börja med att följa hello detaljerade instruktioner i dessa artiklar.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-160">tooadd social IDPs tooyour app, begin by following hello detailed instructions in these articles.</span></span> <span data-ttu-id="1b1c7-161">För varje IDP du vill toosupport, du behöver tooregister ett program i systemet och hämta ett klient-ID.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-161">For each IDP you want toosupport, you need tooregister an application in that system and obtain a client ID.</span></span>

* [<span data-ttu-id="1b1c7-162">Konfigurera Facebook som en IDP</span><span class="sxs-lookup"><span data-stu-id="1b1c7-162">Set up Facebook as an IDP</span></span>](active-directory-b2c-setup-fb-app.md)
* [<span data-ttu-id="1b1c7-163">Konfigurera Google som en IDP</span><span class="sxs-lookup"><span data-stu-id="1b1c7-163">Set up Google as an IDP</span></span>](active-directory-b2c-setup-goog-app.md)
* [<span data-ttu-id="1b1c7-164">Ställa in Amazon som en IDP</span><span class="sxs-lookup"><span data-stu-id="1b1c7-164">Set up Amazon as an IDP</span></span>](active-directory-b2c-setup-amzn-app.md)
* [<span data-ttu-id="1b1c7-165">Ställa in LinkedIn som en IDP</span><span class="sxs-lookup"><span data-stu-id="1b1c7-165">Set up LinkedIn as an IDP</span></span>](active-directory-b2c-setup-li-app.md)

<span data-ttu-id="1b1c7-166">När du har lagt till hello identitet providers tooyour B2C-katalog, redigera av dina tre principer tooinclude hello nya IDPs enligt beskrivningen i hello [referensartikeln om principer](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="1b1c7-166">After you add hello identity providers tooyour B2C directory, edit each of your three policies tooinclude hello new IDPs, as described in hello [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="1b1c7-167">När du har sparat dina principer, kör du hello appen igen.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-167">After you save your policies, run hello app again.</span></span>  <span data-ttu-id="1b1c7-168">Du bör se hello nya IDPs läggas till som alternativ för inloggning och registreringen i var och en av dina identitetsupplevelser.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-168">You should see hello new IDPs added as sign-in and sign-up options in each of your identity experiences.</span></span>

<span data-ttu-id="1b1c7-169">Du kan experimentera med dina principer och studera hello effekten på din exempelapp.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-169">You can experiment with your policies and observe hello effect on your sample app.</span></span> <span data-ttu-id="1b1c7-170">Lägg till eller ta bort IDPs, manipulera programanspråken eller ändra registreringsattribut.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-170">Add or remove IDPs, manipulate application claims, or change sign-up attributes.</span></span> <span data-ttu-id="1b1c7-171">Experimentera tills du kan se hur principer, autentiseringsbegäranden och OWIN knyta ihop.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-171">Experiment until you can see how policies, authentication requests, and OWIN tie together.</span></span>

## <a name="sample-code-walkthrough"></a><span data-ttu-id="1b1c7-172">Exempel kod genomgång</span><span class="sxs-lookup"><span data-stu-id="1b1c7-172">Sample code walkthrough</span></span>
<span data-ttu-id="1b1c7-173">hello följande avsnitt visar hur hello exempelkod för programmet är konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-173">hello following sections show you how hello sample application code is configured.</span></span> <span data-ttu-id="1b1c7-174">Du kan använda den som en vägledning i framtida app-utveckling.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-174">You may use this as a guide in your future app development.</span></span>

### <a name="add-authentication-support"></a><span data-ttu-id="1b1c7-175">Lägga till stöd för autentisering</span><span class="sxs-lookup"><span data-stu-id="1b1c7-175">Add authentication support</span></span>

<span data-ttu-id="1b1c7-176">Du kan nu konfigurera din app toouse Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-176">You can now configure your app toouse Azure AD B2C.</span></span> <span data-ttu-id="1b1c7-177">Appen kommunicerar med Azure AD B2C genom att skicka OpenID Connect autentiseringsbegäranden.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-177">Your app communicates with Azure AD B2C by sending OpenID Connect authentication requests.</span></span> <span data-ttu-id="1b1c7-178">hello begäranden styr hello användarupplevelse tooexecute vill att din app genom att ange hello princip.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-178">hello requests dictate hello user experience your app wants tooexecute by specifying hello policy.</span></span> <span data-ttu-id="1b1c7-179">Du kan använda Microsofts OWIN-biblioteket toosend dessa begäranden, köra principer, hantera användarsessioner och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-179">You can use Microsoft's OWIN library toosend these requests, execute policies, manage user sessions, and more.</span></span>

#### <a name="install-owin"></a><span data-ttu-id="1b1c7-180">Installera OWIN</span><span class="sxs-lookup"><span data-stu-id="1b1c7-180">Install OWIN</span></span>

<span data-ttu-id="1b1c7-181">toobegin, lägga till hello OWIN mellanprogram NuGet paket toohello projekt med hjälp av hello Visual Studio Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-181">toobegin, add hello OWIN middleware NuGet packages toohello project by using hello Visual Studio Package Manager Console.</span></span>

```Console
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

#### <a name="add-an-owin-startup-class"></a><span data-ttu-id="1b1c7-182">Lägga till en OWIN-startklass</span><span class="sxs-lookup"><span data-stu-id="1b1c7-182">Add an OWIN startup class</span></span>

<span data-ttu-id="1b1c7-183">Lägg till en OWIN Start klassen toohello API kallas `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-183">Add an OWIN startup class toohello API called `Startup.cs`.</span></span>  <span data-ttu-id="1b1c7-184">Högerklicka på hello-projektet, Välj **Lägg till** och **nytt objekt**, och sök sedan efter OWIN.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-184">Right-click on hello project, select **Add** and **New Item**, and then search for OWIN.</span></span> <span data-ttu-id="1b1c7-185">Hej OWIN mellanprogram ska anropa hello `Configuration(…)` metod när appen startar.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-185">hello OWIN middleware will invoke hello `Configuration(…)` method when your app starts.</span></span>

<span data-ttu-id="1b1c7-186">I vårt exempel vi ändrat hello klassdeklarationen för`public partial class Startup` och implementeras hello andra delar av hello klassen i `App_Start\Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-186">In our sample, we changed hello class declaration too`public partial class Startup` and implemented hello other part of hello class in `App_Start\Startup.Auth.cs`.</span></span> <span data-ttu-id="1b1c7-187">I hello `Configuration` metod, vi har lagt till ett anrop för`ConfigureAuth`, som har definierats i `Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-187">Inside hello `Configuration` method, we added a call too`ConfigureAuth`, which is defined in `Startup.Auth.cs`.</span></span> <span data-ttu-id="1b1c7-188">Efter hello ändringar `Startup.cs` ser ut som följande hello:</span><span class="sxs-lookup"><span data-stu-id="1b1c7-188">After hello modifications, `Startup.cs` looks like hello following:</span></span>

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

#### <a name="configure-hello-authentication-middleware"></a><span data-ttu-id="1b1c7-189">Konfigurera hello mellanprogram för autentisering</span><span class="sxs-lookup"><span data-stu-id="1b1c7-189">Configure hello authentication middleware</span></span>

<span data-ttu-id="1b1c7-190">Öppna hello filen `App_Start\Startup.Auth.cs` och implementera hello `ConfigureAuth(...)` metod.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-190">Open hello file `App_Start\Startup.Auth.cs` and implement hello `ConfigureAuth(...)` method.</span></span> <span data-ttu-id="1b1c7-191">Hej parametrar som du anger i `OpenIdConnectAuthenticationOptions` fungerar som koordinater för din app toocommunicate med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-191">hello parameters you provide in `OpenIdConnectAuthenticationOptions` serve as coordinates for your app toocommunicate with Azure AD B2C.</span></span> <span data-ttu-id="1b1c7-192">Om du inte anger vissa parametrar används hello standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-192">If you do not specify certain parameters, it will use hello default value.</span></span> <span data-ttu-id="1b1c7-193">Till exempel vi inte anger hello `ResponseType` i exemplet hello så hello standardvärdet `code id_token` kommer att användas i varje utgående begäran tooAzure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-193">For example, we do not specify hello `ResponseType` in hello sample, so hello default value `code id_token` will be used in each outgoing request tooAzure AD B2C.</span></span>

<span data-ttu-id="1b1c7-194">Du måste också tooset in cookie-autentisering.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-194">You also need tooset up cookie authentication.</span></span> <span data-ttu-id="1b1c7-195">Hej OpenID Connect mellanprogram använder cookies användarsessioner toomaintain, bland annat.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-195">hello OpenID Connect middleware uses cookies toomaintain user sessions, among other things.</span></span>

```CSharp
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // Initialize variables ...

    // Configure hello OWIN middleware
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseCookieAuthentication(new CookieAuthenticationOptions());
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Generate hello metadata address using hello tenant and policy information
                MetadataAddress = String.Format(AadInstance, Tenant, DefaultPolicy),

                // These are standard OpenID Connect parameters, with values pulled from web.config
                ClientId = ClientId,
                RedirectUri = RedirectUri,
                PostLogoutRedirectUri = RedirectUri,

                // Specify hello callbacks for each type of notifications
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    RedirectToIdentityProvider = OnRedirectToIdentityProvider,
                    AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    AuthenticationFailed = OnAuthenticationFailed,
                },

                // Specify hello claims toovalidate
                TokenValidationParameters = new TokenValidationParameters
                {
                    NameClaimType = "name"
                },

                // Specify hello scope by appending all of hello scopes requested into one string (seperated by a blank space)
                Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
            }
        );
    }

    // Implement hello "Notification" methods...
}
```

<span data-ttu-id="1b1c7-196">I `OpenIdConnectAuthenticationOptions` ovan, vi definierar en uppsättning Återanropsfunktioner för specifika meddelanden som tas emot av hello OpenID Connect mellanprogram.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-196">In `OpenIdConnectAuthenticationOptions` above, we define a set of callback functions for specific notifications that are received by hello OpenID Connect middleware.</span></span> <span data-ttu-id="1b1c7-197">Dessa beteenden definieras med hjälp av en `OpenIdConnectAuthenticationNotifications` objekt och lagras i hello `Notifications` variabeln.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-197">These behaviors are defined using a `OpenIdConnectAuthenticationNotifications` object and stored into hello `Notifications` variable.</span></span> <span data-ttu-id="1b1c7-198">I vårt exempel definiera vi tre olika återanrop beroende på hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-198">In our sample, we define three different callbacks depending on hello event.</span></span>

### <a name="using-different-policies"></a><span data-ttu-id="1b1c7-199">Med hjälp av olika principer</span><span class="sxs-lookup"><span data-stu-id="1b1c7-199">Using different policies</span></span>

<span data-ttu-id="1b1c7-200">Hej `RedirectToIdentityProvider` avisering utlöses när en förfrågan görs tooAzure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-200">hello `RedirectToIdentityProvider` notification is triggered whenever a request is made tooAzure AD B2C.</span></span> <span data-ttu-id="1b1c7-201">I hello Återanropsfunktionen `OnRedirectToIdentityProvider`, vi kontrollerar hello utgående anropet om vi vill toouse en annan princip.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-201">In hello callback function `OnRedirectToIdentityProvider`, we check in hello outgoing call if we want toouse a different policy.</span></span> <span data-ttu-id="1b1c7-202">I ordning toodo lösenord återställa eller redigera en profil måste toouse hello motsvarande princip som hello princip i stället för hello ”registrering eller inloggning” standardprincipen för lösenordsåterställning.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-202">In order toodo a password reset or edit a profile, you need toouse hello corresponding policy such as hello password reset policy instead of hello default "Sign-up or Sign-in" policy.</span></span>

<span data-ttu-id="1b1c7-203">I vårt exempel, när en användare vill tooreset hello lösenord eller redigera hello profil vi lägga till hello princip vi föredrar toouse till hello OWIN kontext.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-203">In our sample, when a user wants tooreset hello password or edit hello profile, we add hello policy we prefer toouse into hello OWIN context.</span></span> <span data-ttu-id="1b1c7-204">Som kan göras genom att göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="1b1c7-204">That can be done by doing hello following:</span></span>

```CSharp
    // Let hello middleware know you are trying toouse hello edit profile policy
    HttpContext.GetOwinContext().Set("Policy", EditProfilePolicyId);
```

<span data-ttu-id="1b1c7-205">Och du kan implementera hello Återanropsfunktionen `OnRedirectToIdentityProvider` hello följande:</span><span class="sxs-lookup"><span data-stu-id="1b1c7-205">And you can implement hello callback function `OnRedirectToIdentityProvider` by doing hello following:</span></span>

```CSharp
/*
*  On each call tooAzure AD B2C, check if a policy (e.g. hello profile edit or password reset policy) has been specified in hello OWIN context.
*  If so, use that policy when making hello call. Also, don't request a code (since it won't be needed).
*/
private Task OnRedirectToIdentityProvider(RedirectToIdentityProviderNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    var policy = notification.OwinContext.Get<string>("Policy");

    if (!string.IsNullOrEmpty(policy) && !policy.Equals(DefaultPolicy))
    {
        notification.ProtocolMessage.Scope = OpenIdConnectScopes.OpenId;
        notification.ProtocolMessage.ResponseType = OpenIdConnectResponseTypes.IdToken;
        notification.ProtocolMessage.IssuerAddress = notification.ProtocolMessage.IssuerAddress.Replace(DefaultPolicy, policy);
    }

    return Task.FromResult(0);
}
```

### <a name="handling-authorization-codes"></a><span data-ttu-id="1b1c7-206">Hantera auktoriseringskoder</span><span class="sxs-lookup"><span data-stu-id="1b1c7-206">Handling authorization codes</span></span>

<span data-ttu-id="1b1c7-207">Hej `AuthorizationCodeReceived` avisering utlöses när en Auktoriseringskoden tas emot.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-207">hello `AuthorizationCodeReceived` notification is triggered when an authorization code is received.</span></span> <span data-ttu-id="1b1c7-208">Hej OpenID Connect mellanprogram stöder inte utbyte koder för åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-208">hello OpenID Connect middleware does not support exchanging codes for access tokens.</span></span> <span data-ttu-id="1b1c7-209">Du kan manuellt utväxla hello koden för hello token i en callback-funktion.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-209">You can manually exchange hello code for hello token in a callback function.</span></span> <span data-ttu-id="1b1c7-210">Mer information finns i hello [dokumentationen](active-directory-b2c-devquickstarts-web-api-dotnet.md) som förklarar hur.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-210">For more information, please look at hello [documentation](active-directory-b2c-devquickstarts-web-api-dotnet.md) that explains how.</span></span>

### <a name="handling-errors"></a><span data-ttu-id="1b1c7-211">Felhantering</span><span class="sxs-lookup"><span data-stu-id="1b1c7-211">Handling errors</span></span>

<span data-ttu-id="1b1c7-212">Hej `AuthenticationFailed` avisering utlöses när autentiseringen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-212">hello `AuthenticationFailed` notification is triggered when authentication fails.</span></span> <span data-ttu-id="1b1c7-213">Du kan hantera hello fel som du vill i dess Återanropsmetoden.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-213">In its callback method, you can handle hello errors as you wish.</span></span> <span data-ttu-id="1b1c7-214">Dock bör du lägga till en kontroll för hello felkod `AADB2C90118`.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-214">You should however add a check for hello error code `AADB2C90118`.</span></span> <span data-ttu-id="1b1c7-215">Under hello körningen av hello ”registrering eller inloggning” hello användaren har hello möjlighet tooselect en **har du glömt lösenordet?** länk.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-215">During hello execution of hello "Sign-up or Sign-in" policy, hello user has hello opportunity tooselect a **Forgot your password?** link.</span></span> <span data-ttu-id="1b1c7-216">I den här händelsen skickar Azure AD B2C appen den felkod som att appen ska gör en begäran med hello principen för lösenordsåterställning i stället.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-216">In this event, Azure AD B2C sends your app that error code indicating that your app should make a request using hello password reset policy instead.</span></span>

```CSharp
/*
* Catch any failures received by hello authentication middleware and handle appropriately
*/
private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    // Handle hello error code that Azure AD B2C throws when trying tooreset a password from hello login page
    // because password reset is not supported by a "sign-up or sign-in policy"
    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If hello user clicked hello reset password link, redirect toohello reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```

### <a name="send-authentication-requests-tooazure-ad"></a><span data-ttu-id="1b1c7-217">Skicka autentisering begäranden tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="1b1c7-217">Send authentication requests tooAzure AD</span></span>

<span data-ttu-id="1b1c7-218">Appen är nu korrekt konfigurerade toocommunicate med Azure AD B2C med hjälp av autentiseringsprotokollet för hello OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-218">Your app is now properly configured toocommunicate with Azure AD B2C by using hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="1b1c7-219">OWIN hanterar hello information utforma autentiseringsmeddelanden, verifiera token från Azure AD B2C och upprätthålla användarsessioner.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-219">OWIN manages hello details of crafting authentication messages, validating tokens from Azure AD B2C, and maintaining user session.</span></span> <span data-ttu-id="1b1c7-220">Allt som fortfarande är tooinitiate flödet för varje användare.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-220">All that remains is tooinitiate each user's flow.</span></span>

<span data-ttu-id="1b1c7-221">När en användare väljer **logga upp/inloggning**, **Redigera profil**, eller **Återställ lösenord** i hello webbapp hello associerade åtgärden anropas i `Controllers\AccountController.cs`:</span><span class="sxs-lookup"><span data-stu-id="1b1c7-221">When a user selects **Sign up/Sign in**, **Edit profile**, or **Reset password** in hello web app, hello associated action is invoked in `Controllers\AccountController.cs`:</span></span>

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting toosign up or sign in
*/
public void SignUpSignIn()
{
    // Use hello default policy tooprocess hello sign up / sign in flow
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge();
        return;
    }

    Response.Redirect("/");
}

/*
*  Called when requesting tooedit a profile
*/
public void EditProfile()
{
    if (Request.IsAuthenticated)
    {
        // Let hello middleware know you are trying toouse hello edit profile policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
        HttpContext.GetOwinContext().Set("Policy", Startup.EditProfilePolicyId);

        // Set hello page tooredirect tooafter editing hello profile
        var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
        HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

        return;
    }

    Response.Redirect("/");

}

/*
*  Called when requesting tooreset a password
*/
public void ResetPassword()
{
    // Let hello middleware know you are trying toouse hello reset password policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
    HttpContext.GetOwinContext().Set("Policy", Startup.ResetPasswordPolicyId);

    // Set hello page tooredirect tooafter changing passwords
    var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
    HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

    return;
}
```

<span data-ttu-id="1b1c7-222">Du kan också använda OWIN toosign ut hello användare från hello app.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-222">You can also use OWIN toosign out hello user from hello app.</span></span> <span data-ttu-id="1b1c7-223">I `Controllers\AccountController.cs` har vi:</span><span class="sxs-lookup"><span data-stu-id="1b1c7-223">In `Controllers\AccountController.cs` we have:</span></span>

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting toosign out
*/
public void SignOut()
{
    // toosign out hello user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

<span data-ttu-id="1b1c7-224">I tillägg tooexplicitly anropar en princip, kan du använda en `[Authorize]` tagg i dina domänkontrollanter som kör en princip om hello användaren inte är inloggad.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-224">In addition tooexplicitly invoking a policy, you can use a `[Authorize]` tag in your controllers that executes a policy if hello user is not signed in.</span></span> <span data-ttu-id="1b1c7-225">Öppna `Controllers\HomeController.cs` och Lägg till hello `[Authorize]` taggen toohello anspråk domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-225">Open `Controllers\HomeController.cs` and add hello `[Authorize]` tag toohello claims controller.</span></span>  <span data-ttu-id="1b1c7-226">OWIN väljer hello senaste principen konfigureras när hello `[Authorize]` taggen med samma namn.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-226">OWIN selects hello last policy configured when hello `[Authorize]` tag is hit.</span></span>

```CSharp
// Controllers\HomeController.cs

// You can use hello Authorize decorator tooexecute a certain policy if hello user is not already signed into hello app.
[Authorize]
public ActionResult Claims()
{
  ...
```

### <a name="display-user-information"></a><span data-ttu-id="1b1c7-227">Visa användarinformation</span><span class="sxs-lookup"><span data-stu-id="1b1c7-227">Display user information</span></span>

<span data-ttu-id="1b1c7-228">När du autentiserar användare med OpenID Connect Azure AD B2C returnerar en ID-token toohello app som innehåller **anspråk**.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-228">When you authenticate users by using OpenID Connect, Azure AD B2C returns an ID token toohello app that contains **claims**.</span></span> <span data-ttu-id="1b1c7-229">Dessa är intyg om hello användare.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-229">These are assertions about hello user.</span></span> <span data-ttu-id="1b1c7-230">Du kan använda anspråk toopersonalize din app.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-230">You can use claims toopersonalize your app.</span></span>

<span data-ttu-id="1b1c7-231">Öppna hello `Controllers\HomeController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-231">Open hello `Controllers\HomeController.cs` file.</span></span> <span data-ttu-id="1b1c7-232">Du kan komma åt användaranspråk i dina domänkontrollanter via hello `ClaimsPrincipal.Current` säkerhetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-232">You can access user claims in your controllers via hello `ClaimsPrincipal.Current` security principal object.</span></span>

```CSharp
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

<span data-ttu-id="1b1c7-233">Du kan komma åt alla anspråk att programmet tar emot i hello samma sätt.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-233">You can access any claim that your application receives in hello same way.</span></span>  <span data-ttu-id="1b1c7-234">En lista över alla hello anspråk hello appen tar emot finns tillgänglig på hello **anspråk** sidan.</span><span class="sxs-lookup"><span data-stu-id="1b1c7-234">A list of all hello claims hello app receives is available for you on hello **Claims** page.</span></span>
