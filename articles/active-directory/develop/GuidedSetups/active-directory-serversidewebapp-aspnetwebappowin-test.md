---
title: "aaaAzure AD v2 ASP.NET Web Server komma igång - Test | Microsoft Docs"
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
ms.openlocfilehash: 99c7525b9146605142180962fc2a61b3c953c064
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a><span data-ttu-id="bdc8a-103">Testa din kod</span><span class="sxs-lookup"><span data-stu-id="bdc8a-103">Test your code</span></span>

<span data-ttu-id="bdc8a-104">Tryck på `F5` toorun ditt projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-104">Press `F5` toorun your project in Visual Studio.</span></span> <span data-ttu-id="bdc8a-105">hello vid webbläsaren öppnas och leder dig direkt för*http://localhost: {port}* där du ser hello *logga in med Microsoft* knappen.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-105">hello browser will open and direct you too*http://localhost:{port}* where you’ll see hello *Sign in with Microsoft* button.</span></span> <span data-ttu-id="bdc8a-106">Gå vidare och på den toosign i.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-106">Go ahead and click it toosign in.</span></span>

<span data-ttu-id="bdc8a-107">När du är klar tootest, Använd kontot toosign i ett arbets- eller skolkonto (Azure Active Directory) eller en personlig (live.com, outlook.com).</span><span class="sxs-lookup"><span data-stu-id="bdc8a-107">When you're ready tootest, use a work or school (Azure Active Directory), or a personal (live.com, outlook.com) account toosign in.</span></span> 

![Logga in med Microsoft webbläsarfönster](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin.png)

![Logga in med Microsoft webbläsarfönster](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a><span data-ttu-id="bdc8a-110">Förväntat resultat</span><span class="sxs-lookup"><span data-stu-id="bdc8a-110">Expected results</span></span>
<span data-ttu-id="bdc8a-111">Efter inloggning är hello användaren omdirigerade toohello startsidan på webbplatsen som är hello HTTPS-URL som anges i programmet registreringsinformation i hello portalen för registrering av Microsoft-program.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-111">After sign-in, hello user is redirected toohello home page of your web site which is hello HTTPS URL specified in your application registration information in hello Microsoft Application Registration Portal.</span></span> <span data-ttu-id="bdc8a-112">Den här sidan visas nu *Hello {User}* och en länk toosign ut och en länk toosee hello användarens anspråk – vilket är en länk toohello auktorisera styrenhet skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-112">This page now shows *Hello {User}* and a link toosign-out, and a link toosee hello user’s claims – which is a link toohello Authorize controller created earlier.</span></span>

### <a name="see-users-claims"></a><span data-ttu-id="bdc8a-113">Se användarens anspråk</span><span class="sxs-lookup"><span data-stu-id="bdc8a-113">See user's claims</span></span>
<span data-ttu-id="bdc8a-114">Välj hello hyperlink toosee hello användarens anspråk.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-114">Select hello hyperlink toosee hello user's claims.</span></span> <span data-ttu-id="bdc8a-115">Detta leder dig toohello domänkontrollant och visa som endast är tillgängliga toousers som autentiseras.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-115">This leads you toohello controller and view that is only available toousers that are authenticated.</span></span>

#### <a name="expected-results"></a><span data-ttu-id="bdc8a-116">Förväntat resultat</span><span class="sxs-lookup"><span data-stu-id="bdc8a-116">Expected results</span></span>
 <span data-ttu-id="bdc8a-117">Du bör se en tabell som innehåller hello grundläggande egenskaper för hello inloggade användare:</span><span class="sxs-lookup"><span data-stu-id="bdc8a-117">You should see a table containing hello basic properties of hello logged user:</span></span>

| <span data-ttu-id="bdc8a-118">Egenskap</span><span class="sxs-lookup"><span data-stu-id="bdc8a-118">Property</span></span> | <span data-ttu-id="bdc8a-119">Värde</span><span class="sxs-lookup"><span data-stu-id="bdc8a-119">Value</span></span> | <span data-ttu-id="bdc8a-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="bdc8a-120">Description</span></span>|
|---|---|---|
| <span data-ttu-id="bdc8a-121">Namn</span><span class="sxs-lookup"><span data-stu-id="bdc8a-121">Name</span></span> | <span data-ttu-id="bdc8a-122">{Fullständig användarnamn}</span><span class="sxs-lookup"><span data-stu-id="bdc8a-122">{User Full Name}</span></span> | <span data-ttu-id="bdc8a-123">hello användarens förnamn och efternamn</span><span class="sxs-lookup"><span data-stu-id="bdc8a-123">hello user’s first and last name</span></span>
|<span data-ttu-id="bdc8a-124">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="bdc8a-124">Username</span></span> | <span>user@domain.com</span>| <span data-ttu-id="bdc8a-125">hello användarnamn används tooidentify hello inloggade användare</span><span class="sxs-lookup"><span data-stu-id="bdc8a-125">hello username used tooidentify hello logged user</span></span>
| <span data-ttu-id="bdc8a-126">Ämne</span><span class="sxs-lookup"><span data-stu-id="bdc8a-126">Subject</span></span>| <span data-ttu-id="bdc8a-127">{Ämne}</span><span class="sxs-lookup"><span data-stu-id="bdc8a-127">{Subject}</span></span>|<span data-ttu-id="bdc8a-128">En sträng toouniquely identifiera hello användarinloggning mellan hello web</span><span class="sxs-lookup"><span data-stu-id="bdc8a-128">A string toouniquely identify hello user logon across hello web</span></span>|
| <span data-ttu-id="bdc8a-129">Klient-ID:t</span><span class="sxs-lookup"><span data-stu-id="bdc8a-129">Tenant ID</span></span>| <span data-ttu-id="bdc8a-130">{Guid}</span><span class="sxs-lookup"><span data-stu-id="bdc8a-130">{Guid}</span></span>| <span data-ttu-id="bdc8a-131">En *guid* toouniquely representerar hello användarens Azure Active Directory-organisation.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-131">A *guid* toouniquely represent hello user’s Azure Active Directory organization.</span></span>|

<span data-ttu-id="bdc8a-132">Dessutom visas en tabell, inklusive alla användaranspråk som ingår i autentiseringsbegäran.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-132">In addition, you will see a table including all user claims included in authentication request.</span></span> <span data-ttu-id="bdc8a-133">En lista över alla anspråk i en Id-Token och förklaring finns [artikel](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "lista av anspråk i Token Id").</span><span class="sxs-lookup"><span data-stu-id="bdc8a-133">For a list of all claims in an Id Token and explanation please see this [article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "List of Claims in Id Token").</span></span>


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a><span data-ttu-id="bdc8a-134">Testa åtkomst till en metod som har en *[auktorisera]* attribut (valfritt)</span><span class="sxs-lookup"><span data-stu-id="bdc8a-134">Test accessing a method that has an *[Authorize]* attribute (Optional)</span></span>
<span data-ttu-id="bdc8a-135">I det här steget ska du testa åtkomst till hello autentiserad domänkontrollant som en anonym användare:</span><span class="sxs-lookup"><span data-stu-id="bdc8a-135">In this step, you will test accessing hello Authenticated controller as an anonymous user:</span></span><br/>
<span data-ttu-id="bdc8a-136">Välj hello länka toosign ut hello användare och fullständig hello utloggning processen.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-136">Select hello link toosign-out hello user and complete hello sign-out process.</span></span><br/>
<span data-ttu-id="bdc8a-137">Skriv nu i din webbläsare http://localhost: {port} / autentiserade tooaccess styrenheten som skyddas med hello `[Authorize]` attribut</span><span class="sxs-lookup"><span data-stu-id="bdc8a-137">Now in your browser, type http://localhost:{port}/authenticated tooaccess your controller that is protected with hello `[Authorize]` attribute</span></span>

#### <a name="expected-results"></a><span data-ttu-id="bdc8a-138">Förväntat resultat</span><span class="sxs-lookup"><span data-stu-id="bdc8a-138">Expected results</span></span>
<span data-ttu-id="bdc8a-139">Du bör få hello-fråga kräver tooauthenticate toosee hello vyn.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-139">You should receive hello prompt requiring you tooauthenticate toosee hello view.</span></span>

## <a name="additional-information"></a><span data-ttu-id="bdc8a-140">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="bdc8a-140">Additional information</span></span>

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a><span data-ttu-id="bdc8a-141">Skydda hela webbplatsen</span><span class="sxs-lookup"><span data-stu-id="bdc8a-141">Protect your entire web site</span></span>
<span data-ttu-id="bdc8a-142">tooprotect hela webbplatsen, lägga till hello `AuthorizeAttribute` för`GlobalFilters` i `Global.asax` `Application_Start` metod:</span><span class="sxs-lookup"><span data-stu-id="bdc8a-142">tooprotect your entire web site, add hello `AuthorizeAttribute` too`GlobalFilters` in `Global.asax` `Application_Start` method:</span></span>

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

> [!NOTE]
> <span data-ttu-id="bdc8a-143">**Hur toorestrict användare från en enda organisation toosign i tooyour program**</span><span class="sxs-lookup"><span data-stu-id="bdc8a-143">**How toorestrict users from only one organization toosign in tooyour application**</span></span>

> <span data-ttu-id="bdc8a-144">Som standard personliga konton (inklusive outlook.com, live.com och andra) samt arbets- och skolkonton konton från alla företag eller organisation som har integrerat med Azure Active Directory kan logga in tooyour program.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-144">By default, personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has integrated with Azure Active Directory can sign-in tooyour application.</span></span> 

> <span data-ttu-id="bdc8a-145">Om du vill att din ansökan tooaccept inloggningar från en enda organisation i Azure Active Directory, ersätter hello `Tenant` parameter i *web.config* från `Common` toohello innehavarens namn för hello organisation – exempelvis *contoso.onmicrosoft.com*. Efter det att ändra hello `ValidateIssuer` argument i din *OWIN-startklass* för`true`.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-145">If you want your application tooaccept sign-ins from only one Azure Active Directory organization, replace hello `Tenant` parameter in *web.config* from `Common` toohello tenant name of hello organization – example, *contoso.onmicrosoft.com*. After that, change hello `ValidateIssuer` argument in your *OWIN Startup class* too`true`.</span></span>

> <span data-ttu-id="bdc8a-146">Ange tooallow användare från att endast en lista över specifika organisationer `ValidateIssuer` tootrue och använda hello `ValidIssuers` parametern toospecify en lista över organisationer.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-146">tooallow users from only a list of specific organizations, set `ValidateIssuer` tootrue and use hello `ValidIssuers` parameter toospecify a list of organizations.</span></span>

> <span data-ttu-id="bdc8a-147">Ett annat alternativ är tooimplement en anpassad metod toovalidate hello utfärdare IssuerValidator-parametern.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-147">Another option is tooimplement a custom method toovalidate hello issuers using IssuerValidator parameter.</span></span> <span data-ttu-id="bdc8a-148">Mer information om `TokenValidationParameters`, se [detta](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "TokenValidationParameters MSDN-artikel") MSDN-artikel.</span><span class="sxs-lookup"><span data-stu-id="bdc8a-148">For more information about `TokenValidationParameters`, please see [this](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "TokenValidationParameters MSDN article") MSDN article.</span></span>

