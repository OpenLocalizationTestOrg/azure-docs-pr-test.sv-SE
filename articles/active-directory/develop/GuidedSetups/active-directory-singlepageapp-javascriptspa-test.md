---
title: aaaAzure AD v2 JS SPA interaktiv installation - Test | Microsoft Docs
description: "Hur JavaScript SPA program anropar en API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: b2339431a070b5c4ad4058e6c1a9b19b83c84c1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a><span data-ttu-id="87eb5-103">Testa din kod</span><span class="sxs-lookup"><span data-stu-id="87eb5-103">Test your code</span></span>

> ### <a name="testing-with-visual-studio"></a><span data-ttu-id="87eb5-104">Testa med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87eb5-104">Testing with Visual Studio</span></span>
> <span data-ttu-id="87eb5-105">Om du använder Visual Studio trycker du på `F5` toorun projektet: hello webbläsaren öppnas och dirigerar dig för*http://localhost: {port}* där du ser hello *anropa Microsoft Graph API* knappen.</span><span class="sxs-lookup"><span data-stu-id="87eb5-105">If you are using Visual Studio, press `F5` toorun your project: hello browser opens and directs you too*http://localhost:{port}* where you see hello *Call Microsoft Graph API* button.</span></span>

<p/><!-- -->

> ### <a name="testing-with-python-or-another-web-server"></a><span data-ttu-id="87eb5-106">Testa med Python eller en annan webbserver</span><span class="sxs-lookup"><span data-stu-id="87eb5-106">Testing with Python or another web server</span></span>
> <span data-ttu-id="87eb5-107">Om du inte använder Visual Studio, kontrollera din webbserver är igång och den är konfigurerad toolisten tooa TCP-port utifrån hello mappen som innehåller din *index.html* fil.</span><span class="sxs-lookup"><span data-stu-id="87eb5-107">If you are not using Visual Studio, make sure your web server is started and it is configured toolisten tooa TCP port based on hello folder containing your *index.html* file.</span></span> <span data-ttu-id="87eb5-108">För Python, kan du börja lyssna toohello port genom att köra hello i hello kommando fråga / terminal, från hello app mappen:</span><span class="sxs-lookup"><span data-stu-id="87eb5-108">For Python, you can start listening toohello port by running hello in hello command prompt/ terminal, from hello app's folder:</span></span>
> 
> ```bash
> python -m http.server 8080
> ```
>  <span data-ttu-id="87eb5-109">Öppna hello webbläsare och skriv sedan *http://localhost: 8080* eller *http://localhost: {port}* - när hello *port* motsvarar toohello-port som webbservern lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="87eb5-109">Then, open hello browser and type *http://localhost:8080* or *http://localhost:{port}* - where hello *port* corresponds toohello port that your web server is listening to.</span></span> <span data-ttu-id="87eb5-110">Du bör se hello innehållet i index.html-sida med hello *anropa Microsoft Graph API* knappen.</span><span class="sxs-lookup"><span data-stu-id="87eb5-110">You should see hello contents of your index.html page with hello *Call Microsoft Graph API* button.</span></span>

## <a name="test-your-application"></a><span data-ttu-id="87eb5-111">Testa programmet</span><span class="sxs-lookup"><span data-stu-id="87eb5-111">Test your application</span></span>

<span data-ttu-id="87eb5-112">Efter hello webbläsaren läser du in din *index.html*, klicka på hello *anropa Microsoft Graph API* knappen.</span><span class="sxs-lookup"><span data-stu-id="87eb5-112">After hello browser loads your *index.html*, click hello *Call Microsoft Graph API* button.</span></span> <span data-ttu-id="87eb5-113">Om det är hello första gången hello webbläsaren omdirigeras du toohello Microsoft Azure Active Directory v2 slutpunkt, där du har uppmanas toosign i.</span><span class="sxs-lookup"><span data-stu-id="87eb5-113">If this is hello first time, hello browser redirects you toohello Microsoft Azure Active Directory v2 endpoint, where you are  prompted toosign in.</span></span>
 
![Exempel skärmbild](media/active-directory-singlepageapp-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="consent"></a><span data-ttu-id="87eb5-115">Medgivande</span><span class="sxs-lookup"><span data-stu-id="87eb5-115">Consent</span></span>
<span data-ttu-id="87eb5-116">hello visas första gången du loggar in tooyour program, en medgivande skärm liknande toohello följande, där du behöver tooaccept:</span><span class="sxs-lookup"><span data-stu-id="87eb5-116">hello very first time you sign in tooyour application, you are presented with a consent screen similar toohello following, where you need tooaccept:</span></span>

 ![Medgivande skärmen](media/active-directory-singlepageapp-javascriptspa-test/javascriptspaconsent.png)


### <a name="expected-results"></a><span data-ttu-id="87eb5-118">Förväntat resultat</span><span class="sxs-lookup"><span data-stu-id="87eb5-118">Expected results</span></span>
<span data-ttu-id="87eb5-119">Du bör se användarens profilinformation som returneras av hello svar för Microsoft Graph API-anrop.</span><span class="sxs-lookup"><span data-stu-id="87eb5-119">You should see user profile information returned by hello Microsoft Graph API call response.</span></span>
 
 ![Resultat](media/active-directory-singlepageapp-javascriptspa-test/javascriptsparesults.png)

<span data-ttu-id="87eb5-121">Du också se grundläggande information om hello-token har införskaffade i hello *åtkomst-Token* och *ID Token anspråk* rutor.</span><span class="sxs-lookup"><span data-stu-id="87eb5-121">You also see basic information about hello token acquired in hello *Access Token* and *ID Token Claims* boxes.</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="87eb5-122">Mer information om scope och delegerade behörigheter</span><span class="sxs-lookup"><span data-stu-id="87eb5-122">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="87eb5-123">hello Microsoft Graph API kräver hello `user.read` omfång tooread hello användarens profil.</span><span class="sxs-lookup"><span data-stu-id="87eb5-123">hello Microsoft Graph API requires hello `user.read` scope tooread hello user's profile.</span></span> <span data-ttu-id="87eb5-124">Detta scope läggs automatiskt som standard i alla program som registreras på vår registreringsportal.</span><span class="sxs-lookup"><span data-stu-id="87eb5-124">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="87eb5-125">Vissa andra API: er för Microsoft Graph samt anpassade API: er för backend-servern kan kräva ytterligare scope.</span><span class="sxs-lookup"><span data-stu-id="87eb5-125">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="87eb5-126">Till exempel hello omfånget för Microsoft Graph `Calendars.Read` är nödvändiga toolist hello användares kalendrar.</span><span class="sxs-lookup"><span data-stu-id="87eb5-126">For example, for Microsoft Graph, hello scope `Calendars.Read` is required toolist hello user’s calendars.</span></span> <span data-ttu-id="87eb5-127">I ordning tooaccess hello användares kalender hello kontexten för ett program måste du tooadd hello `Calendars.Read` delegerad behörighet toohello programmet registreringens information och Lägg sedan till hello `Calendars.Read` omfång toohello `acquireTokenSilent` anropa.</span><span class="sxs-lookup"><span data-stu-id="87eb5-127">In order tooaccess hello user’s calendar in hello context of an application, you need tooadd hello `Calendars.Read` delegated permission toohello application registration’s information and then add hello `Calendars.Read` scope toohello `acquireTokenSilent` call.</span></span> <span data-ttu-id="87eb5-128">hello användare kan uppmanas att ytterligare medgivanden när du ökar hello antal scope.</span><span class="sxs-lookup"><span data-stu-id="87eb5-128">hello user may be prompted for additional consents as you increase hello number of scopes.</span></span>

<span data-ttu-id="87eb5-129">Om en serverdel API inte kräver ett omfång (rekommenderas inte), kan du använda hello `clientId` som hello omfattning i hello `acquireTokenSilent` och/eller `acquireTokenRedirect` anrop.</span><span class="sxs-lookup"><span data-stu-id="87eb5-129">If a backend API does not require a scope (not recommended), you can use hello `clientId` as hello scope in hello `acquireTokenSilent` and/or `acquireTokenRedirect` calls.</span></span>

<!--end-collapse-->
