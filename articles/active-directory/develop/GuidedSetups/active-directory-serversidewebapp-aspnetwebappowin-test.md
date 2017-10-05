---
title: "Azure AD v2 ASP.NET Web Server komma igång - testa | Microsoft Docs"
description: "Implementera Microsoft logga In på en ASP.NET-lösning med ett traditionellt webbläsarbaserade program med OpenID Connect standard"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 00cb963e85111274c36c3a84489894811ad2dabd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a><span data-ttu-id="462b0-103">Testa din kod</span><span class="sxs-lookup"><span data-stu-id="462b0-103">Test your code</span></span>

<span data-ttu-id="462b0-104">Tryck på `F5` köra projektet i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="462b0-104">Press `F5` to run your project in Visual Studio.</span></span> <span data-ttu-id="462b0-105">Webbläsaren öppnas och be dig *http://localhost: {port}* där du ser den *logga in med Microsoft* knappen.</span><span class="sxs-lookup"><span data-stu-id="462b0-105">The browser will open and direct you to *http://localhost:{port}* where you’ll see the *Sign in with Microsoft* button.</span></span> <span data-ttu-id="462b0-106">Gå vidare och genom att klicka på Logga in.</span><span class="sxs-lookup"><span data-stu-id="462b0-106">Go ahead and click it to sign in.</span></span>

<span data-ttu-id="462b0-107">När du är redo att testa kan du använda ett arbets- eller skolkonto (Azure Active Directory) eller ett konto för personal (live.com, outlook.com) för att logga in.</span><span class="sxs-lookup"><span data-stu-id="462b0-107">When you're ready to test, use a work or school (Azure Active Directory), or a personal (live.com, outlook.com) account to sign in.</span></span> 

![Logga in med Microsoft webbläsarfönster](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin.png)

![Logga in med Microsoft webbläsarfönster](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a><span data-ttu-id="462b0-110">Förväntat resultat</span><span class="sxs-lookup"><span data-stu-id="462b0-110">Expected results</span></span>
<span data-ttu-id="462b0-111">Efter inloggning omdirigeras användaren till startsidan för webbplatsen som är HTTPS-URL som anges i programmet registreringsinformation i portalen för registrering av Microsoft-program.</span><span class="sxs-lookup"><span data-stu-id="462b0-111">After sign-in, the user is redirected to the home page of your web site which is the HTTPS URL specified in your application registration information in the Microsoft Application Registration Portal.</span></span> <span data-ttu-id="462b0-112">Den här sidan visas nu *Hello {User}* och en länk till utloggning och en länk till finns användaranspråk – som är en länk till auktorisera controller skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="462b0-112">This page now shows *Hello {User}* and a link to sign-out, and a link to see the user’s claims – which is a link to the Authorize controller created earlier.</span></span>

### <a name="see-users-claims"></a><span data-ttu-id="462b0-113">Se användarens anspråk</span><span class="sxs-lookup"><span data-stu-id="462b0-113">See user's claims</span></span>
<span data-ttu-id="462b0-114">Välj länken för att se användarens anspråk.</span><span class="sxs-lookup"><span data-stu-id="462b0-114">Select the hyperlink to see the user's claims.</span></span> <span data-ttu-id="462b0-115">Detta leder dig till domänkontrollanten och visa som endast är tillgängliga för användare som autentiseras.</span><span class="sxs-lookup"><span data-stu-id="462b0-115">This leads you to the controller and view that is only available to users that are authenticated.</span></span>

#### <a name="expected-results"></a><span data-ttu-id="462b0-116">Förväntat resultat</span><span class="sxs-lookup"><span data-stu-id="462b0-116">Expected results</span></span>
 <span data-ttu-id="462b0-117">Du bör se en tabell som innehåller de grundläggande egenskaperna för den inloggade användaren:</span><span class="sxs-lookup"><span data-stu-id="462b0-117">You should see a table containing the basic properties of the logged user:</span></span>

| <span data-ttu-id="462b0-118">Egenskap</span><span class="sxs-lookup"><span data-stu-id="462b0-118">Property</span></span> | <span data-ttu-id="462b0-119">Värde</span><span class="sxs-lookup"><span data-stu-id="462b0-119">Value</span></span> | <span data-ttu-id="462b0-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="462b0-120">Description</span></span>|
|---|---|---|
| <span data-ttu-id="462b0-121">Namn</span><span class="sxs-lookup"><span data-stu-id="462b0-121">Name</span></span> | <span data-ttu-id="462b0-122">{Fullständig användarnamn}</span><span class="sxs-lookup"><span data-stu-id="462b0-122">{User Full Name}</span></span> | <span data-ttu-id="462b0-123">Användaren förnamn och efternamn</span><span class="sxs-lookup"><span data-stu-id="462b0-123">The user’s first and last name</span></span>
|<span data-ttu-id="462b0-124">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="462b0-124">Username</span></span> | <span>user@domain.com</span>| <span data-ttu-id="462b0-125">Användarnamnet som används för att identifiera den inloggade användaren</span><span class="sxs-lookup"><span data-stu-id="462b0-125">The username used to identify the logged user</span></span>
| <span data-ttu-id="462b0-126">Ämne</span><span class="sxs-lookup"><span data-stu-id="462b0-126">Subject</span></span>| <span data-ttu-id="462b0-127">{Ämne}</span><span class="sxs-lookup"><span data-stu-id="462b0-127">{Subject}</span></span>|<span data-ttu-id="462b0-128">En sträng för att identifiera användaren loggar in på Internet</span><span class="sxs-lookup"><span data-stu-id="462b0-128">A string to uniquely identify the user logon across the web</span></span>|
| <span data-ttu-id="462b0-129">Klient-ID:t</span><span class="sxs-lookup"><span data-stu-id="462b0-129">Tenant ID</span></span>| <span data-ttu-id="462b0-130">{Guid}</span><span class="sxs-lookup"><span data-stu-id="462b0-130">{Guid}</span></span>| <span data-ttu-id="462b0-131">En *guid* som unikt representerar användarens Azure Active Directory-organisation.</span><span class="sxs-lookup"><span data-stu-id="462b0-131">A *guid* to uniquely represent the user’s Azure Active Directory organization.</span></span>|

<span data-ttu-id="462b0-132">Dessutom visas en tabell, inklusive alla användaranspråk som ingår i autentiseringsbegäran.</span><span class="sxs-lookup"><span data-stu-id="462b0-132">In addition, you will see a table including all user claims included in authentication request.</span></span> <span data-ttu-id="462b0-133">En lista över alla anspråk i en Id-Token och förklaring finns [artikel](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "lista av anspråk i Token Id").</span><span class="sxs-lookup"><span data-stu-id="462b0-133">For a list of all claims in an Id Token and explanation please see this [article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "List of Claims in Id Token").</span></span>


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a><span data-ttu-id="462b0-134">Testa åtkomst till en metod som har en *[auktorisera]* attribut (valfritt)</span><span class="sxs-lookup"><span data-stu-id="462b0-134">Test accessing a method that has an *[Authorize]* attribute (Optional)</span></span>
<span data-ttu-id="462b0-135">I det här steget ska du testa åtkomst till autentiserad domänkontrollant som en anonym användare:</span><span class="sxs-lookup"><span data-stu-id="462b0-135">In this step, you will test accessing the Authenticated controller as an anonymous user:</span></span><br/>
<span data-ttu-id="462b0-136">Välj en länk till utloggning användaren och slutföra utloggning.</span><span class="sxs-lookup"><span data-stu-id="462b0-136">Select the link to sign-out the user and complete the sign-out process.</span></span><br/>
<span data-ttu-id="462b0-137">Skriv nu i din webbläsare http://localhost: {port} / autentiserad åtkomst till din domänkontrollant som är skyddat med den `[Authorize]` attribut</span><span class="sxs-lookup"><span data-stu-id="462b0-137">Now in your browser, type http://localhost:{port}/authenticated to access your controller that is protected with the `[Authorize]` attribute</span></span>

#### <a name="expected-results"></a><span data-ttu-id="462b0-138">Förväntat resultat</span><span class="sxs-lookup"><span data-stu-id="462b0-138">Expected results</span></span>
<span data-ttu-id="462b0-139">Du får uppmaningen att kräva att du autentisera för att visa.</span><span class="sxs-lookup"><span data-stu-id="462b0-139">You should receive the prompt requiring you to authenticate to see the view.</span></span>

## <a name="additional-information"></a><span data-ttu-id="462b0-140">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="462b0-140">Additional information</span></span>

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a><span data-ttu-id="462b0-141">Skydda hela webbplatsen</span><span class="sxs-lookup"><span data-stu-id="462b0-141">Protect your entire web site</span></span>
<span data-ttu-id="462b0-142">För att skydda hela webbplatsen, lägger du till den `AuthorizeAttribute` till `GlobalFilters` i `Global.asax` `Application_Start` metod:</span><span class="sxs-lookup"><span data-stu-id="462b0-142">To protect your entire web site, add the `AuthorizeAttribute` to `GlobalFilters` in `Global.asax` `Application_Start` method:</span></span>

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

> [!NOTE]
> <span data-ttu-id="462b0-143">**Hur du begränsa användare från en enda organisation att logga in på ditt program**</span><span class="sxs-lookup"><span data-stu-id="462b0-143">**How to restrict users from only one organization to sign in to your application**</span></span>

> <span data-ttu-id="462b0-144">Som standard personliga konton (inklusive outlook.com och live.com) samt fungerar och skolkonton från företaget eller organisationen som har integrerat med Azure Active Directory kan logga in på ditt program.</span><span class="sxs-lookup"><span data-stu-id="462b0-144">By default, personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has integrated with Azure Active Directory can sign-in to your application.</span></span> 

> <span data-ttu-id="462b0-145">Om du vill att programmet ska acceptera inloggningar från en enda organisation i Azure Active Directory, ersätter den `Tenant` parameter i *web.config* från `Common` till innehavarens namn för organisationen – exempel, *contoso.onmicrosoft.com*.</span><span class="sxs-lookup"><span data-stu-id="462b0-145">If you want your application to accept sign-ins from only one Azure Active Directory organization, replace the `Tenant` parameter in *web.config* from `Common` to the tenant name of the organization – example, *contoso.onmicrosoft.com*.</span></span> <span data-ttu-id="462b0-146">Sedan kan ändra den `ValidateIssuer` argument i din *OWIN-startklass* till `true`.</span><span class="sxs-lookup"><span data-stu-id="462b0-146">After that, change the `ValidateIssuer` argument in your *OWIN Startup class* to `true`.</span></span>

> <span data-ttu-id="462b0-147">Om du vill tillåta att användare från en lista med specifika organisationer, ange `ValidateIssuer` till true och använda den `ValidIssuers` parametern för att ange en lista över organisationer.</span><span class="sxs-lookup"><span data-stu-id="462b0-147">To allow users from only a list of specific organizations, set `ValidateIssuer` to true and use the `ValidIssuers` parameter to specify a list of organizations.</span></span>

> <span data-ttu-id="462b0-148">Ett annat alternativ är att implementera en anpassad metod för att verifiera de utfärdare som använder IssuerValidator parameter.</span><span class="sxs-lookup"><span data-stu-id="462b0-148">Another option is to implement a custom method to validate the issuers using IssuerValidator parameter.</span></span> <span data-ttu-id="462b0-149">Mer information om `TokenValidationParameters`, se [detta](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "TokenValidationParameters MSDN-artikel") MSDN-artikel.</span><span class="sxs-lookup"><span data-stu-id="462b0-149">For more information about `TokenValidationParameters`, please see [this](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "TokenValidationParameters MSDN article") MSDN article.</span></span>

