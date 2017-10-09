---
title: "aaaAzure AD v2 Windows Desktop komma igång - Test | Microsoft Docs"
description: "Hur Windows Desktop .NET (XAML) program anropar en API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten"
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
ms.openlocfilehash: 0ae9612e1585c54a3fe35ba9d18f92554099b2c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a><span data-ttu-id="c8c47-103">Testa din kod</span><span class="sxs-lookup"><span data-stu-id="c8c47-103">Test your code</span></span>

<span data-ttu-id="c8c47-104">Ordna tootest programmet, tryck på `F5` toorun ditt projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8c47-104">In order tootest your application, press `F5` toorun your project in Visual Studio.</span></span> <span data-ttu-id="c8c47-105">Main-fönster visas:</span><span class="sxs-lookup"><span data-stu-id="c8c47-105">Your Main Window should appear:</span></span>

![Exempel skärmbild](media/active-directory-mobileanddesktopapp-windowsdesktop-test/samplescreenshot.png)

<span data-ttu-id="c8c47-107">När du är klar tootest du *anropa Microsoft Graph API* och använda Microsoft Azure Active Directory (organisationskonto) eller en Account (live.com, outlook.com) konto toosign i.</span><span class="sxs-lookup"><span data-stu-id="c8c47-107">When you're ready tootest, click *Call Microsoft Graph API* and use a Microsoft Azure Active Directory (organizational account) or a Microsoft Account (live.com, outlook.com) account toosign in.</span></span> <span data-ttu-id="c8c47-108">Den hello är första gången, visas ett fönster som ber användaren toosign i:</span><span class="sxs-lookup"><span data-stu-id="c8c47-108">It it is hello first time, you will see a window asking user toosign in:</span></span>

![Logga in](media/active-directory-mobileanddesktopapp-windowsdesktop-test/signinscreenshot.png)

### <a name="consent"></a><span data-ttu-id="c8c47-110">Medgivande</span><span class="sxs-lookup"><span data-stu-id="c8c47-110">Consent</span></span>
<span data-ttu-id="c8c47-111">hello första gången du loggar in tooyour program visas med en medgivande skärm liknande toohello nedan, där du måste acceptera tooexplicitly:</span><span class="sxs-lookup"><span data-stu-id="c8c47-111">hello first time you sign in tooyour application, you will be presented with a consent screen similar toohello below, where you need tooexplicitly accept:</span></span>

![Medgivande skärmen](media/active-directory-mobileanddesktopapp-windowsdesktop-test/consentscreen.png)

### <a name="expected-results"></a><span data-ttu-id="c8c47-113">Förväntat resultat</span><span class="sxs-lookup"><span data-stu-id="c8c47-113">Expected results</span></span>
<span data-ttu-id="c8c47-114">Användarens profilinformation som returneras av hello Microsoft Graph API-anrop på API-anrop resultat hello-skärmen bör visas.</span><span class="sxs-lookup"><span data-stu-id="c8c47-114">You should see user profile information returned by hello Microsoft Graph API call on hello API Call Results screen.</span></span>

<span data-ttu-id="c8c47-115">Du bör också se grundläggande information om hello-token har köpt `AcquireTokenAsync` eller `AcquireTokenSilentAsync` i rutan för Token information om hello:</span><span class="sxs-lookup"><span data-stu-id="c8c47-115">You  should also see basic information about hello token acquired via `AcquireTokenAsync` or `AcquireTokenSilentAsync` in hello Token Info box:</span></span>

|<span data-ttu-id="c8c47-116">Egenskap</span><span class="sxs-lookup"><span data-stu-id="c8c47-116">Property</span></span>  |<span data-ttu-id="c8c47-117">Format</span><span class="sxs-lookup"><span data-stu-id="c8c47-117">Format</span></span>  |<span data-ttu-id="c8c47-118">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c8c47-118">Description</span></span> |
|---------|---------|---------|
|<span data-ttu-id="c8c47-119">Namn</span><span class="sxs-lookup"><span data-stu-id="c8c47-119">Name</span></span> | <span data-ttu-id="c8c47-120">{Fullständig användarnamn}</span><span class="sxs-lookup"><span data-stu-id="c8c47-120">{User Full name}</span></span> |<span data-ttu-id="c8c47-121">hello användarens förnamn och efternamn</span><span class="sxs-lookup"><span data-stu-id="c8c47-121">hello user’s first and last name</span></span>|
|<span data-ttu-id="c8c47-122">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="c8c47-122">Username</span></span> |<span>user@domain.com</span> |<span data-ttu-id="c8c47-123">hello användarnamn används tooidentify hello användare</span><span class="sxs-lookup"><span data-stu-id="c8c47-123">hello username used tooidentify hello user</span></span>|
|<span data-ttu-id="c8c47-124">Token upphör att gälla</span><span class="sxs-lookup"><span data-stu-id="c8c47-124">Token Expires</span></span> |<span data-ttu-id="c8c47-125">{DateTime}</span><span class="sxs-lookup"><span data-stu-id="c8c47-125">{DateTime}</span></span>         |<span data-ttu-id="c8c47-126">hello tid på vilka hello token upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="c8c47-126">hello time on which hello token expires.</span></span> <span data-ttu-id="c8c47-127">MSAL kommer förlänga giltighetstiden för hello du genom att förnya hello token vid behov</span><span class="sxs-lookup"><span data-stu-id="c8c47-127">MSAL will extend hello expiration date for you by renewing hello token when necessary</span></span>|
|<span data-ttu-id="c8c47-128">Åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="c8c47-128">Access token</span></span> |<span data-ttu-id="c8c47-129">{Sträng}</span><span class="sxs-lookup"><span data-stu-id="c8c47-129">{String}</span></span>         |<span data-ttu-id="c8c47-130">Hej tokensträng skickas som kommer att skickas tooHTTP förfrågningar som kräver ett authorization-huvud</span><span class="sxs-lookup"><span data-stu-id="c8c47-130">hello token string sent that will be sent tooHTTP requests that require an authorization header</span></span>|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="c8c47-131">Mer information om scope och delegerade behörigheter</span><span class="sxs-lookup"><span data-stu-id="c8c47-131">More information about scopes and delegated permissions</span></span>
<span data-ttu-id="c8c47-132">Graph API kräver hello `user.read` omfång tooread användarprofil.</span><span class="sxs-lookup"><span data-stu-id="c8c47-132">Graph API requires hello `user.read` scope tooread user profile.</span></span> <span data-ttu-id="c8c47-133">Detta scope läggs automatiskt som standard i alla program som registreras på vår registreringsportal.</span><span class="sxs-lookup"><span data-stu-id="c8c47-133">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="c8c47-134">Vissa andra Graph API: er samt anpassade API: er för backend-servern kräver ytterligare scope.</span><span class="sxs-lookup"><span data-stu-id="c8c47-134">Some other Graph APIs as well as custom APIs for your backend server require additional scopes.</span></span> <span data-ttu-id="c8c47-135">Till exempel för diagram, `Calendars.Read` är nödvändiga toolist användarkalendrar.</span><span class="sxs-lookup"><span data-stu-id="c8c47-135">For example, for Graph, `Calendars.Read` is required toolist user’s calendars.</span></span> <span data-ttu-id="c8c47-136">I ordning tooaccess hello användarens kalender i en kontext med ett program måste du tooadd `Calendars.Read` delegerade programmet registreringsinformation och Lägg sedan till `Calendars.Read` toohello `AcquireTokenAsync` anropa.</span><span class="sxs-lookup"><span data-stu-id="c8c47-136">In order tooaccess hello user’s calendar in a context of an application, you need tooadd `Calendars.Read` delegated application registration’s information and then add `Calendars.Read` toohello `AcquireTokenAsync` call.</span></span> <span data-ttu-id="c8c47-137">Användare kan uppmanas att ytterligare medgivanden när du ökar hello antal scope.</span><span class="sxs-lookup"><span data-stu-id="c8c47-137">User may be prompted for additional consents as you increase hello number of scopes.</span></span>

<!--end-collapse-->



