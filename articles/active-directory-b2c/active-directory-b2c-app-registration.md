---
title: 'Azure Active Directory B2C: Programregistrering | Microsoft Docs'
description: Registrera ditt program med Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: PatAltimore
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: parakhj
ms.openlocfilehash: 3d4fe2fa10d848c8b29e4d22d284c0d378f07ae0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-register-your-application"></a><span data-ttu-id="d46b0-103">Azure Active Directory B2C: Registrera ditt program</span><span class="sxs-lookup"><span data-stu-id="d46b0-103">Azure Active Directory B2C: Register your application</span></span>

<span data-ttu-id="d46b0-104">Med hjälp av den här snabbstarten registrerar du ett program i en Microsoft Azure Active Directory (Azure AD) B2C-klientorganisation på några få minuter.</span><span class="sxs-lookup"><span data-stu-id="d46b0-104">This Quickstart helps you register an application in a Microsoft Azure Active Directory (Azure AD) B2C tenant in a few minutes.</span></span> <span data-ttu-id="d46b0-105">När du är färdig är programmet klart att användas i Azure B2C-klientorganisationen.</span><span class="sxs-lookup"><span data-stu-id="d46b0-105">When you're finished, your application is registered for use in the Azure B2C tenant.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d46b0-106">Krav</span><span class="sxs-lookup"><span data-stu-id="d46b0-106">Prerequisites</span></span>

<span data-ttu-id="d46b0-107">Om du vill skapa ett program som accepterar registrering och inloggning av konsumenter måste du först registrera programmet med en Azure Active Directory B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="d46b0-107">To build an application that accepts consumer sign-up and sign-in, you first need to register the application with an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="d46b0-108">Skaffa en egen klient genom att följa stegen i [Skapa en Azure AD B2C-klient](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d46b0-108">Get your own tenant by using the steps outlined in [Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

<span data-ttu-id="d46b0-109">Program som har skapats från Azure AD B2C-bladet i Azure Portal måste hanteras från samma plats.</span><span class="sxs-lookup"><span data-stu-id="d46b0-109">Applications created from the Azure AD B2C blade in the Azure portal must be managed from the same location.</span></span> <span data-ttu-id="d46b0-110">Om du redigerar B2C-program med hjälp av PowerShell eller någon annan portal, stöds de inte och fungerar inte med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d46b0-110">If you edit the B2C applications using PowerShell or another portal, they become unsupported and do not work with Azure AD B2C.</span></span> <span data-ttu-id="d46b0-111">Mer information finns i avsnittet [felaktiga appar](#faulted-apps).</span><span class="sxs-lookup"><span data-stu-id="d46b0-111">See details in the [faulted apps](#faulted-apps) section.</span></span> 

## <a name="navigate-to-b2c-settings"></a><span data-ttu-id="d46b0-112">Gå till B2C-inställningar</span><span class="sxs-lookup"><span data-stu-id="d46b0-112">Navigate to B2C settings</span></span>

<span data-ttu-id="d46b0-113">Logga in på [Azure Portal](https://portal.azure.com/) som global administratör för B2C-klientorganisationen.</span><span class="sxs-lookup"><span data-stu-id="d46b0-113">Log in to the [Azure portal](https://portal.azure.com/) as the Global Administrator of the B2C tenant.</span></span> 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](../../includes/active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="d46b0-114">Välj nästa steg baserat på vilken typ av program du registrerar:</span><span class="sxs-lookup"><span data-stu-id="d46b0-114">Choose next steps based on the application type you are registering:</span></span>

* [<span data-ttu-id="d46b0-115">Registrera ett webbprogram</span><span class="sxs-lookup"><span data-stu-id="d46b0-115">Register a web application</span></span>](#register-a-web-app)
* [<span data-ttu-id="d46b0-116">Registrera ett webb-API</span><span class="sxs-lookup"><span data-stu-id="d46b0-116">Register a web API</span></span>](#register-a-web-api)
* [<span data-ttu-id="d46b0-117">Registrera ett mobilt eller inbyggt program</span><span class="sxs-lookup"><span data-stu-id="d46b0-117">Register a mobile or native application</span></span>](#register-a-mobile-or-native-app)
 
## <a name="register-a-web-app"></a><span data-ttu-id="d46b0-118">Registrera en webbapp</span><span class="sxs-lookup"><span data-stu-id="d46b0-118">Register a web app</span></span>

[!INCLUDE [active-directory-b2c-register-web-app](../../includes/active-directory-b2c-register-web-app.md)]

<span data-ttu-id="d46b0-119">Gör så här om webbprogrammet anropar ett webb-API som skyddas av Azure AD B2C:</span><span class="sxs-lookup"><span data-stu-id="d46b0-119">If your web application calls a web API secured by Azure AD B2C, perform these steps:</span></span>
   1. <span data-ttu-id="d46b0-120">Skapa en programhemlighet genom att gå till bladet **Nycklar** och klicka på **Generera nyckel**.</span><span class="sxs-lookup"><span data-stu-id="d46b0-120">Create an application secret by going to the **Keys** blade and clicking the **Generate Key** button.</span></span> <span data-ttu-id="d46b0-121">Anteckna **appnyckel**-värdet.</span><span class="sxs-lookup"><span data-stu-id="d46b0-121">Make note of the **App key** value.</span></span> <span data-ttu-id="d46b0-122">Du använder värdet som programhemlighet i programkoden.</span><span class="sxs-lookup"><span data-stu-id="d46b0-122">You use the value as the application secret in your application's code.</span></span>
   2. <span data-ttu-id="d46b0-123">Klicka på **API-åtkomst**, **Lägg till** och välj webb-API och områden (behörigheter).</span><span class="sxs-lookup"><span data-stu-id="d46b0-123">Click **API Access**, click **Add**, and select your web API and scopes (permissions).</span></span>

> [!NOTE]
> <span data-ttu-id="d46b0-124">En **programhemlighet** är en viktig autentiseringsuppgift och bör skyddas på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="d46b0-124">An **Application Secret** is an important security credential, and should be secured appropriately.</span></span>
> 

[<span data-ttu-id="d46b0-125">Gå vidare till **nästa steg**</span><span class="sxs-lookup"><span data-stu-id="d46b0-125">Jump to **next steps**</span></span>](#next-steps)

## <a name="register-a-web-api"></a><span data-ttu-id="d46b0-126">Registrera ett webb-API</span><span class="sxs-lookup"><span data-stu-id="d46b0-126">Register a web API</span></span>

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

<span data-ttu-id="d46b0-127">Klicka på **Publicerade områden** om du behöver lägga till fler områden.</span><span class="sxs-lookup"><span data-stu-id="d46b0-127">Click **Published scopes** to add more scopes as necessary.</span></span> <span data-ttu-id="d46b0-128">Som standard definieras området ”user_impersonation”.</span><span class="sxs-lookup"><span data-stu-id="d46b0-128">By default, the "user_impersonation" scope is defined.</span></span> <span data-ttu-id="d46b0-129">Området user_impersonation ger andra program möjlighet att komma åt det här API:et för den inloggade användarens räkning.</span><span class="sxs-lookup"><span data-stu-id="d46b0-129">The user_impersonation scope gives other applications the ability to access this api on behalf of the signed-in user.</span></span> <span data-ttu-id="d46b0-130">Om du vill kan området user_impersonation tas bort.</span><span class="sxs-lookup"><span data-stu-id="d46b0-130">If you wish, the user_impersonation scope can be removed.</span></span>

[<span data-ttu-id="d46b0-131">Gå vidare till **nästa steg**</span><span class="sxs-lookup"><span data-stu-id="d46b0-131">Jump to **next steps**</span></span>](#next-steps)

## <a name="register-a-mobile-or-native-app"></a><span data-ttu-id="d46b0-132">Registrera en mobil eller inbyggd app</span><span class="sxs-lookup"><span data-stu-id="d46b0-132">Register a mobile or native app</span></span>

[!INCLUDE [active-directory-b2c-register-mobile-native-app](../../includes/active-directory-b2c-register-mobile-native-app.md)]

[<span data-ttu-id="d46b0-133">Gå vidare till **nästa steg**</span><span class="sxs-lookup"><span data-stu-id="d46b0-133">Jump to **next steps**</span></span>](#next-steps)

## <a name="limitations"></a><span data-ttu-id="d46b0-134">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="d46b0-134">Limitations</span></span>

### <a name="choosing-a-web-app-or-api-reply-url"></a><span data-ttu-id="d46b0-135">Om du väljer en webbapp eller en api-svarswebbadress</span><span class="sxs-lookup"><span data-stu-id="d46b0-135">Choosing a web app or api reply URL</span></span>

<span data-ttu-id="d46b0-136">Appar som har registrerats med Azure AD B2C är för närvarande begränsade till en begränsad uppsättning svars-URL-värden.</span><span class="sxs-lookup"><span data-stu-id="d46b0-136">Currently, apps that are registered with Azure AD B2C are restricted to a limited set of reply URL values.</span></span> <span data-ttu-id="d46b0-137">Svars-URL för webbprogram och tjänster måste börja med schemat `https` och alla svars-URL-värden måste dela en enda DNS-domän.</span><span class="sxs-lookup"><span data-stu-id="d46b0-137">The reply URL for web apps and services must begin with the scheme `https`, and all reply URL values must share a single DNS domain.</span></span> <span data-ttu-id="d46b0-138">Exempelvis kan du registrera ett webbprogram som har en av dessa svars-URL: er:</span><span class="sxs-lookup"><span data-stu-id="d46b0-138">For example, you cannot register a web app that has one of these reply URLs:</span></span>

`https://login-east.contoso.com`

`https://login-west.contoso.com`

<span data-ttu-id="d46b0-139">Registreringssystemet jämför hela DNS-namnet på den befintliga svars-URL:en med DNS-namnet på den svars-URL som du lägger till.</span><span class="sxs-lookup"><span data-stu-id="d46b0-139">The registration system compares the whole DNS name of the existing reply URL to the DNS name of the reply URL that you are adding.</span></span> <span data-ttu-id="d46b0-140">Begäran om att lägga till DNS-namnet misslyckas om något av följande villkor föreligger:</span><span class="sxs-lookup"><span data-stu-id="d46b0-140">The request to add the DNS name fails if either of the following conditions is true:</span></span>

* <span data-ttu-id="d46b0-141">Hela DNS-namnet på den nya svars-URL:en motsvarar inte DNS-namnet på den befintliga svars-URL:en.</span><span class="sxs-lookup"><span data-stu-id="d46b0-141">The whole DNS name of the new reply URL does not match the DNS name of the existing reply URL.</span></span>
* <span data-ttu-id="d46b0-142">Hela DNS-namnet på den nya svars-URL:en är inte en underdomän till den befintliga svars-URL:en.</span><span class="sxs-lookup"><span data-stu-id="d46b0-142">The whole DNS name of the new reply URL is not a subdomain of the existing reply URL.</span></span>

<span data-ttu-id="d46b0-143">Till exempel om appen har svars-URL:</span><span class="sxs-lookup"><span data-stu-id="d46b0-143">For example, if the app has this reply URL:</span></span>

`https://login.contoso.com`

<span data-ttu-id="d46b0-144">Du kan lägga till data så här:</span><span class="sxs-lookup"><span data-stu-id="d46b0-144">You can add to it, like this:</span></span>

`https://login.contoso.com/new`

<span data-ttu-id="d46b0-145">I det här fallet matchar DNS-namnet exakt.</span><span class="sxs-lookup"><span data-stu-id="d46b0-145">In this case, the DNS name matches exactly.</span></span> <span data-ttu-id="d46b0-146">Du kan också göra detta:</span><span class="sxs-lookup"><span data-stu-id="d46b0-146">Or, you can do this:</span></span>

`https://new.login.contoso.com`

<span data-ttu-id="d46b0-147">I så fall måste du referera till DNS-underdomänen login.contoso.com. Om du vill ha en app som har login-east.contoso.com och login-west.contoso.com som svars-URL: er måste du lägga till dessa svars-URL: er i den här ordningen:</span><span class="sxs-lookup"><span data-stu-id="d46b0-147">In this case, you're referring to a DNS subdomain of login.contoso.com. If you want to have an app that has login-east.contoso.com and login-west.contoso.com as reply URLs, you must add those reply URLs in this order:</span></span>

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

<span data-ttu-id="d46b0-148">Du kan lägga till två senare eftersom de är underdomäner i den första reply-URL:en, contoso.com.</span><span class="sxs-lookup"><span data-stu-id="d46b0-148">You can add the latter two because they are subdomains of the first reply URL, contoso.com.</span></span>

### <a name="choosing-a-native-app-redirect-uri"></a><span data-ttu-id="d46b0-149">Om du väljer en omdirigerings-URI för en inbyggd app</span><span class="sxs-lookup"><span data-stu-id="d46b0-149">Choosing a native app redirect URI</span></span>

<span data-ttu-id="d46b0-150">Det finns två viktiga överväganden när du väljer en omdirigerings-URI för mobila/interna program:</span><span class="sxs-lookup"><span data-stu-id="d46b0-150">There are two important considerations when choosing a redirect URI for mobile/native applications:</span></span>

* <span data-ttu-id="d46b0-151">**Unik**: Schemat för omdirigerings-URI måste vara unikt för varje program.</span><span class="sxs-lookup"><span data-stu-id="d46b0-151">**Unique**: The scheme of the redirect URI should be unique for every application.</span></span> <span data-ttu-id="d46b0-152">I vårt exempel (com.onmicrosoft.contoso.appname://redirect/path) använder vi com.onmicrosoft.contoso.appname som schema.</span><span class="sxs-lookup"><span data-stu-id="d46b0-152">In our example (com.onmicrosoft.contoso.appname://redirect/path), we use com.onmicrosoft.contoso.appname as the scheme.</span></span> <span data-ttu-id="d46b0-153">Vi rekommenderar att detta mönster följs.</span><span class="sxs-lookup"><span data-stu-id="d46b0-153">We recommend following this pattern.</span></span> <span data-ttu-id="d46b0-154">Om två program delar samma schema visas en dialogruta för att ”välja app”.</span><span class="sxs-lookup"><span data-stu-id="d46b0-154">If two applications share the same scheme, the user sees a "choose app" dialog.</span></span> <span data-ttu-id="d46b0-155">Inloggningen misslyckas om användaren gör ett felaktigt val.</span><span class="sxs-lookup"><span data-stu-id="d46b0-155">If the user makes an incorrect choice, the login fails.</span></span>
* <span data-ttu-id="d46b0-156">**Fullständig**: Omdirigerings-URI måste ha ett schema och en sökväg.</span><span class="sxs-lookup"><span data-stu-id="d46b0-156">**Complete**: Redirect URI must have a scheme and a path.</span></span> <span data-ttu-id="d46b0-157">Sökvägen måste innehålla minst ett snedstreck efter domänen (till exempel fungerar //contoso/ medan //contoso misslyckas).</span><span class="sxs-lookup"><span data-stu-id="d46b0-157">The path must contain at least one forward slash after the domain (for example, //contoso/ works and //contoso fails).</span></span>

<span data-ttu-id="d46b0-158">Se till att det inte finns några specialtecken som understreck i omdirigerings-uri.</span><span class="sxs-lookup"><span data-stu-id="d46b0-158">Ensure there are no special characters like underscores in the redirect uri.</span></span>

### <a name="faulted-apps"></a><span data-ttu-id="d46b0-159">Felaktig appar</span><span class="sxs-lookup"><span data-stu-id="d46b0-159">Faulted apps</span></span>

<span data-ttu-id="d46b0-160">B2C program bör INTE redigeras:</span><span class="sxs-lookup"><span data-stu-id="d46b0-160">B2C applications should NOT be edited:</span></span>

* <span data-ttu-id="d46b0-161">På andra programhanteringsportaler, som den [klassiska Azure-portalen](https://manage.windowsazure.com/) & den [Programregistreringsportalen](https://apps.dev.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="d46b0-161">On other application management portals such as the [Azure classic portal](https://manage.windowsazure.com/) & the [Application Registration Portal](https://apps.dev.microsoft.com/).</span></span>
* <span data-ttu-id="d46b0-162">Med Graph API eller PowerShell</span><span class="sxs-lookup"><span data-stu-id="d46b0-162">Using Graph API or PowerShell</span></span>

<span data-ttu-id="d46b0-163">Om du redigerar B2C-programmet enligt ovan och försöker att redigera det på Azure AD B2C-funktionsbladet på Azure Portal, blir det en felaktig app och programmet kommer inte längre att kunna användas med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d46b0-163">If you edit the B2C application as described above and try to edit it again in the Azure AD B2C features blade on the Azure portal, it becomes a faulted app, and your application is no longer usable with Azure AD B2C.</span></span> <span data-ttu-id="d46b0-164">Du måste ta bort programmet och skapa det igen.</span><span class="sxs-lookup"><span data-stu-id="d46b0-164">You have to delete the application and create it again.</span></span>

<span data-ttu-id="d46b0-165">Ta bort appen genom att gå till den [Appregistreringsportalen](https://apps.dev.microsoft.com/) och ta bort det appen.</span><span class="sxs-lookup"><span data-stu-id="d46b0-165">To delete the app, go to the [Application Registration Portal](https://apps.dev.microsoft.com/) and delete the application there.</span></span> <span data-ttu-id="d46b0-166">Du måste vara ägare till appen (och inte bara en administratör för klienten) för att appen ska vara synlig.</span><span class="sxs-lookup"><span data-stu-id="d46b0-166">In order for the application to be visible, you need to be the owner of the application (and not just an admin of the tenant).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d46b0-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d46b0-167">Next steps</span></span>

<span data-ttu-id="d46b0-168">Nu när du har registrerat ett program med Azure AD B2C kan du gå [en av våra snabbstartsguider](active-directory-b2c-overview.md#get-started) för att komma igång.</span><span class="sxs-lookup"><span data-stu-id="d46b0-168">Now that you have an application registered with Azure AD B2C, you can complete one of [our quick-start tutorials](active-directory-b2c-overview.md#get-started) to get up and running.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d46b0-169">Skapa en ASP.NET-webbapp med registrering, inloggning och lösenordsåterställning</span><span class="sxs-lookup"><span data-stu-id="d46b0-169">Create an ASP.NET web app with sign-up, sign-in, and password reset</span></span>](active-directory-b2c-devquickstarts-web-dotnet-susi.md)