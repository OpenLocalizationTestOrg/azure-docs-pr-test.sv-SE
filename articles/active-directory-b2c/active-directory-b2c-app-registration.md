---
title: 'Azure Active Directory B2C: Programregistrering | Microsoft Docs'
description: Hur tooregister ditt program med Azure Active Directory B2C
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
ms.openlocfilehash: bd58e123751db387d6c8f16bd010291ba698b1a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-register-your-application"></a><span data-ttu-id="8e65e-103">Azure Active Directory B2C: Registrera ditt program</span><span class="sxs-lookup"><span data-stu-id="8e65e-103">Azure Active Directory B2C: Register your application</span></span>

<span data-ttu-id="8e65e-104">Med hjälp av den här snabbstarten registrerar du ett program i en Microsoft Azure Active Directory (Azure AD) B2C-klientorganisation på några få minuter.</span><span class="sxs-lookup"><span data-stu-id="8e65e-104">This Quickstart helps you register an application in a Microsoft Azure Active Directory (Azure AD) B2C tenant in a few minutes.</span></span> <span data-ttu-id="8e65e-105">När du är klar registreras ditt program för användning i hello Azure B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="8e65e-105">When you're finished, your application is registered for use in hello Azure B2C tenant.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8e65e-106">Krav</span><span class="sxs-lookup"><span data-stu-id="8e65e-106">Prerequisites</span></span>

<span data-ttu-id="8e65e-107">toobuild ett program som accepterar konsumenten registrering och inloggning, måste du först tooregister hello program med en Azure Active Directory B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="8e65e-107">toobuild an application that accepts consumer sign-up and sign-in, you first need tooregister hello application with an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="8e65e-108">Skaffa en egen klient genom att använda hello steg som beskrivs i [skapa en Azure AD B2C-klient](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="8e65e-108">Get your own tenant by using hello steps outlined in [Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

<span data-ttu-id="8e65e-109">Program som har skapats från hello Azure AD B2C-bladet i hello Azure-portalen måste hanteras från hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="8e65e-109">Applications created from hello Azure AD B2C blade in hello Azure portal must be managed from hello same location.</span></span> <span data-ttu-id="8e65e-110">Om du redigerar hello B2C-program med hjälp av PowerShell eller en annan portal, blir stöds inte och fungerar inte med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="8e65e-110">If you edit hello B2C applications using PowerShell or another portal, they become unsupported and do not work with Azure AD B2C.</span></span> <span data-ttu-id="8e65e-111">Mer information finns i hello [fel appar](#faulted-apps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8e65e-111">See details in hello [faulted apps](#faulted-apps) section.</span></span> 

## <a name="navigate-toob2c-settings"></a><span data-ttu-id="8e65e-112">Navigera tooB2C inställningar</span><span class="sxs-lookup"><span data-stu-id="8e65e-112">Navigate tooB2C settings</span></span>

<span data-ttu-id="8e65e-113">Logga in toohello [Azure-portalen](https://portal.azure.com/) som hello Global administratör för hello B2C-klienten.</span><span class="sxs-lookup"><span data-stu-id="8e65e-113">Log in toohello [Azure portal](https://portal.azure.com/) as hello Global Administrator of hello B2C tenant.</span></span> 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](../../includes/active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="8e65e-114">Välj nästa steg baserat på hello programtyp som du registrerar:</span><span class="sxs-lookup"><span data-stu-id="8e65e-114">Choose next steps based on hello application type you are registering:</span></span>

* [<span data-ttu-id="8e65e-115">Registrera ett webbprogram</span><span class="sxs-lookup"><span data-stu-id="8e65e-115">Register a web application</span></span>](#register-a-web-app)
* [<span data-ttu-id="8e65e-116">Registrera ett webb-API</span><span class="sxs-lookup"><span data-stu-id="8e65e-116">Register a web API</span></span>](#register-a-web-api)
* [<span data-ttu-id="8e65e-117">Registrera ett mobilt eller inbyggt program</span><span class="sxs-lookup"><span data-stu-id="8e65e-117">Register a mobile or native application</span></span>](#register-a-mobile-or-native-app)
 
## <a name="register-a-web-app"></a><span data-ttu-id="8e65e-118">Registrera en webbapp</span><span class="sxs-lookup"><span data-stu-id="8e65e-118">Register a web app</span></span>

[!INCLUDE [active-directory-b2c-register-web-app](../../includes/active-directory-b2c-register-web-app.md)]

<span data-ttu-id="8e65e-119">Gör så här om webbprogrammet anropar ett webb-API som skyddas av Azure AD B2C:</span><span class="sxs-lookup"><span data-stu-id="8e65e-119">If your web application calls a web API secured by Azure AD B2C, perform these steps:</span></span>
   1. <span data-ttu-id="8e65e-120">Skapa en programhemlighet genom att gå toohello **nycklar** bladet och klicka på hello **Generera nyckel** knappen.</span><span class="sxs-lookup"><span data-stu-id="8e65e-120">Create an application secret by going toohello **Keys** blade and clicking hello **Generate Key** button.</span></span> <span data-ttu-id="8e65e-121">Anteckna hello **appkey** värde.</span><span class="sxs-lookup"><span data-stu-id="8e65e-121">Make note of hello **App key** value.</span></span> <span data-ttu-id="8e65e-122">Du kan använda hello-värde som hello programhemlighet i din programkod.</span><span class="sxs-lookup"><span data-stu-id="8e65e-122">You use hello value as hello application secret in your application's code.</span></span>
   2. <span data-ttu-id="8e65e-123">Klicka på **API-åtkomst**, **Lägg till** och välj webb-API och områden (behörigheter).</span><span class="sxs-lookup"><span data-stu-id="8e65e-123">Click **API Access**, click **Add**, and select your web API and scopes (permissions).</span></span>

> [!NOTE]
> <span data-ttu-id="8e65e-124">En **programhemlighet** är en viktig autentiseringsuppgift och bör skyddas på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="8e65e-124">An **Application Secret** is an important security credential, and should be secured appropriately.</span></span>
> 

[<span data-ttu-id="8e65e-125">Hoppa för**nästa steg**</span><span class="sxs-lookup"><span data-stu-id="8e65e-125">Jump too**next steps**</span></span>](#next-steps)

## <a name="register-a-web-api"></a><span data-ttu-id="8e65e-126">Registrera ett webb-API</span><span class="sxs-lookup"><span data-stu-id="8e65e-126">Register a web API</span></span>

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

<span data-ttu-id="8e65e-127">Klicka på **publicerade scope** tooadd mer scope som krävs.</span><span class="sxs-lookup"><span data-stu-id="8e65e-127">Click **Published scopes** tooadd more scopes as necessary.</span></span> <span data-ttu-id="8e65e-128">Som standard definieras hello ”user_impersonation” omfång.</span><span class="sxs-lookup"><span data-stu-id="8e65e-128">By default, hello "user_impersonation" scope is defined.</span></span> <span data-ttu-id="8e65e-129">Hej user_impersonation omfång ger andra program hello möjlighet tooaccess detta api hello inloggade användarens räkning.</span><span class="sxs-lookup"><span data-stu-id="8e65e-129">hello user_impersonation scope gives other applications hello ability tooaccess this api on behalf of hello signed-in user.</span></span> <span data-ttu-id="8e65e-130">Om du vill kan hello user_impersonation omfång tas bort.</span><span class="sxs-lookup"><span data-stu-id="8e65e-130">If you wish, hello user_impersonation scope can be removed.</span></span>

[<span data-ttu-id="8e65e-131">Hoppa för**nästa steg**</span><span class="sxs-lookup"><span data-stu-id="8e65e-131">Jump too**next steps**</span></span>](#next-steps)

## <a name="register-a-mobile-or-native-app"></a><span data-ttu-id="8e65e-132">Registrera en mobil eller inbyggd app</span><span class="sxs-lookup"><span data-stu-id="8e65e-132">Register a mobile or native app</span></span>

[!INCLUDE [active-directory-b2c-register-mobile-native-app](../../includes/active-directory-b2c-register-mobile-native-app.md)]

[<span data-ttu-id="8e65e-133">Hoppa för**nästa steg**</span><span class="sxs-lookup"><span data-stu-id="8e65e-133">Jump too**next steps**</span></span>](#next-steps)

## <a name="limitations"></a><span data-ttu-id="8e65e-134">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="8e65e-134">Limitations</span></span>

### <a name="choosing-a-web-app-or-api-reply-url"></a><span data-ttu-id="8e65e-135">Om du väljer en webbapp eller en api-svarswebbadress</span><span class="sxs-lookup"><span data-stu-id="8e65e-135">Choosing a web app or api reply URL</span></span>

<span data-ttu-id="8e65e-136">Appar som har registrerats med Azure AD B2C finns för närvarande begränsad tooa begränsad uppsättning reply URL-värden.</span><span class="sxs-lookup"><span data-stu-id="8e65e-136">Currently, apps that are registered with Azure AD B2C are restricted tooa limited set of reply URL values.</span></span> <span data-ttu-id="8e65e-137">Hej reply URL: en för webbprogram och tjänster måste börja med hello schema `https`, och alla svara URL-värden måste dela en enda DNS-domän.</span><span class="sxs-lookup"><span data-stu-id="8e65e-137">hello reply URL for web apps and services must begin with hello scheme `https`, and all reply URL values must share a single DNS domain.</span></span> <span data-ttu-id="8e65e-138">Exempelvis kan du registrera ett webbprogram som har en av dessa svars-URL: er:</span><span class="sxs-lookup"><span data-stu-id="8e65e-138">For example, you cannot register a web app that has one of these reply URLs:</span></span>

`https://login-east.contoso.com`

`https://login-west.contoso.com`

<span data-ttu-id="8e65e-139">hello registreringssystem jämför hello hela DNS-namnet på hello befintliga reply URL toohello DNS-namnet på hello reply-URL som du lägger till.</span><span class="sxs-lookup"><span data-stu-id="8e65e-139">hello registration system compares hello whole DNS name of hello existing reply URL toohello DNS name of hello reply URL that you are adding.</span></span> <span data-ttu-id="8e65e-140">hello begäran tooadd hello DNS-namnet misslyckas om någon av hello följande villkor föreligger:</span><span class="sxs-lookup"><span data-stu-id="8e65e-140">hello request tooadd hello DNS name fails if either of hello following conditions is true:</span></span>

* <span data-ttu-id="8e65e-141">hello hela DNS-namnet på hello nya reply URL matchar inte hello DNS-namnet på hello befintliga reply-URL.</span><span class="sxs-lookup"><span data-stu-id="8e65e-141">hello whole DNS name of hello new reply URL does not match hello DNS name of hello existing reply URL.</span></span>
* <span data-ttu-id="8e65e-142">hello hela DNS-namnet på hello nya reply-URL är inte en underdomän till hello befintliga reply-URL.</span><span class="sxs-lookup"><span data-stu-id="8e65e-142">hello whole DNS name of hello new reply URL is not a subdomain of hello existing reply URL.</span></span>

<span data-ttu-id="8e65e-143">Om hello har appen reply-URL:</span><span class="sxs-lookup"><span data-stu-id="8e65e-143">For example, if hello app has this reply URL:</span></span>

`https://login.contoso.com`

<span data-ttu-id="8e65e-144">Du kan lägga till tooit så här:</span><span class="sxs-lookup"><span data-stu-id="8e65e-144">You can add tooit, like this:</span></span>

`https://login.contoso.com/new`

<span data-ttu-id="8e65e-145">I det här fallet matchar hello DNS-namnet exakt.</span><span class="sxs-lookup"><span data-stu-id="8e65e-145">In this case, hello DNS name matches exactly.</span></span> <span data-ttu-id="8e65e-146">Du kan också göra detta:</span><span class="sxs-lookup"><span data-stu-id="8e65e-146">Or, you can do this:</span></span>

`https://new.login.contoso.com`

<span data-ttu-id="8e65e-147">I så fall måste refererar du tooa DNS-underdomänen av login.contoso.com. Om du vill toohave en app som har inloggningen east.contoso.com och inloggnings-west.contoso.com som svara URL: er, måste du lägga till dessa svars-URL: er i den här ordningen:</span><span class="sxs-lookup"><span data-stu-id="8e65e-147">In this case, you're referring tooa DNS subdomain of login.contoso.com. If you want toohave an app that has login-east.contoso.com and login-west.contoso.com as reply URLs, you must add those reply URLs in this order:</span></span>

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

<span data-ttu-id="8e65e-148">Du kan lägga till hello två senare eftersom de är underdomäner hello första reply URL contoso.com.</span><span class="sxs-lookup"><span data-stu-id="8e65e-148">You can add hello latter two because they are subdomains of hello first reply URL, contoso.com.</span></span>

### <a name="choosing-a-native-app-redirect-uri"></a><span data-ttu-id="8e65e-149">Om du väljer en omdirigerings-URI för en inbyggd app</span><span class="sxs-lookup"><span data-stu-id="8e65e-149">Choosing a native app redirect URI</span></span>

<span data-ttu-id="8e65e-150">Det finns två viktiga överväganden när du väljer en omdirigerings-URI för mobila/interna program:</span><span class="sxs-lookup"><span data-stu-id="8e65e-150">There are two important considerations when choosing a redirect URI for mobile/native applications:</span></span>

* <span data-ttu-id="8e65e-151">**Unik**: hello ordning med hello omdirigerings-URI måste vara unikt för varje program.</span><span class="sxs-lookup"><span data-stu-id="8e65e-151">**Unique**: hello scheme of hello redirect URI should be unique for every application.</span></span> <span data-ttu-id="8e65e-152">I vårt exempel (com.onmicrosoft.contoso.appname://redirect/path) använder vi com.onmicrosoft.contoso.appname som hello schema.</span><span class="sxs-lookup"><span data-stu-id="8e65e-152">In our example (com.onmicrosoft.contoso.appname://redirect/path), we use com.onmicrosoft.contoso.appname as hello scheme.</span></span> <span data-ttu-id="8e65e-153">Vi rekommenderar att detta mönster följs.</span><span class="sxs-lookup"><span data-stu-id="8e65e-153">We recommend following this pattern.</span></span> <span data-ttu-id="8e65e-154">Om två program delar hello samma schemat, hello användaren ser en dialogruta med ett ”Välj app”.</span><span class="sxs-lookup"><span data-stu-id="8e65e-154">If two applications share hello same scheme, hello user sees a "choose app" dialog.</span></span> <span data-ttu-id="8e65e-155">Om hello användare gör ett felaktigt val, misslyckas hello inloggningen.</span><span class="sxs-lookup"><span data-stu-id="8e65e-155">If hello user makes an incorrect choice, hello login fails.</span></span>
* <span data-ttu-id="8e65e-156">**Fullständig**: Omdirigerings-URI måste ha ett schema och en sökväg.</span><span class="sxs-lookup"><span data-stu-id="8e65e-156">**Complete**: Redirect URI must have a scheme and a path.</span></span> <span data-ttu-id="8e65e-157">hello sökväg måste innehålla minst ett snedstreck hello domän (till exempel //contoso/ fungerar och //contoso misslyckas).</span><span class="sxs-lookup"><span data-stu-id="8e65e-157">hello path must contain at least one forward slash after hello domain (for example, //contoso/ works and //contoso fails).</span></span>

<span data-ttu-id="8e65e-158">Se till att det inte finns några specialtecken som understreck i hello omdirigerings-uri.</span><span class="sxs-lookup"><span data-stu-id="8e65e-158">Ensure there are no special characters like underscores in hello redirect uri.</span></span>

### <a name="faulted-apps"></a><span data-ttu-id="8e65e-159">Felaktig appar</span><span class="sxs-lookup"><span data-stu-id="8e65e-159">Faulted apps</span></span>

<span data-ttu-id="8e65e-160">B2C program bör INTE redigeras:</span><span class="sxs-lookup"><span data-stu-id="8e65e-160">B2C applications should NOT be edited:</span></span>

* <span data-ttu-id="8e65e-161">På andra programhanteringsportaler, som den [klassiska Azure-portalen](https://manage.windowsazure.com/) & den [Programregistreringsportalen](https://apps.dev.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="8e65e-161">On other application management portals such as the [Azure classic portal](https://manage.windowsazure.com/) & the [Application Registration Portal](https://apps.dev.microsoft.com/).</span></span>
* <span data-ttu-id="8e65e-162">Med Graph API eller PowerShell</span><span class="sxs-lookup"><span data-stu-id="8e65e-162">Using Graph API or PowerShell</span></span>

<span data-ttu-id="8e65e-163">Om du redigerar hello B2C programmet enligt ovan och försök tooedit den igen i hello Azure AD B2C-funktionsbladet på hello Azure-portalen, blir den en felaktig app och programmet inte längre kan användas med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="8e65e-163">If you edit hello B2C application as described above and try tooedit it again in hello Azure AD B2C features blade on hello Azure portal, it becomes a faulted app, and your application is no longer usable with Azure AD B2C.</span></span> <span data-ttu-id="8e65e-164">Du har toodelete hello programmet och skapa den igen.</span><span class="sxs-lookup"><span data-stu-id="8e65e-164">You have toodelete hello application and create it again.</span></span>

<span data-ttu-id="8e65e-165">toodelete hello app, gå toohello [Programregistreringsportalen](https://apps.dev.microsoft.com/) och ta bort det hello-programmet.</span><span class="sxs-lookup"><span data-stu-id="8e65e-165">toodelete hello app, go toohello [Application Registration Portal](https://apps.dev.microsoft.com/) and delete hello application there.</span></span> <span data-ttu-id="8e65e-166">För hello programmet toobe synliga måste toobe hello ägare av programmet hello (och inte bara en administratör av hello innehavaren).</span><span class="sxs-lookup"><span data-stu-id="8e65e-166">In order for hello application toobe visible, you need toobe hello owner of hello application (and not just an admin of hello tenant).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8e65e-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8e65e-167">Next steps</span></span>

<span data-ttu-id="8e65e-168">Nu när du har registrerat ett program med Azure AD B2C kan du slutföra en av [våra snabbstartsguider](active-directory-b2c-overview.md#get-started) tooget igång.</span><span class="sxs-lookup"><span data-stu-id="8e65e-168">Now that you have an application registered with Azure AD B2C, you can complete one of [our quick-start tutorials](active-directory-b2c-overview.md#get-started) tooget up and running.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8e65e-169">Skapa en ASP.NET-webbapp med registrering, inloggning och lösenordsåterställning</span><span class="sxs-lookup"><span data-stu-id="8e65e-169">Create an ASP.NET web app with sign-up, sign-in, and password reset</span></span>](active-directory-b2c-devquickstarts-web-dotnet-susi.md)