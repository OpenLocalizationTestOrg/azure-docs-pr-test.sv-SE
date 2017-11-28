---
title: Azure AD v2 JS SPA interaktiv installation - Test | Microsoft Docs
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
ms.openlocfilehash: c888760ab311e8ac08b1e625bb837f91047db645
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
## <a name="test-your-code"></a><span data-ttu-id="587db-103">Testa din kod</span><span class="sxs-lookup"><span data-stu-id="587db-103">Test your code</span></span>

> ### <a name="testing-with-visual-studio"></a><span data-ttu-id="587db-104">Testa med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="587db-104">Testing with Visual Studio</span></span>
> <span data-ttu-id="587db-105">Om du använder Visual Studio trycker du på `F5` köra projektet: webbläsaren öppnas och dirigerar dig till *http://localhost: {port}* där du ser den *anropa Microsoft Graph API* knappen.</span><span class="sxs-lookup"><span data-stu-id="587db-105">If you are using Visual Studio, press `F5` to run your project: the browser opens and directs you to *http://localhost:{port}* where you see the *Call Microsoft Graph API* button.</span></span>

<p/><!-- -->

> ### <a name="testing-with-python-or-another-web-server"></a><span data-ttu-id="587db-106">Testa med Python eller en annan webbserver</span><span class="sxs-lookup"><span data-stu-id="587db-106">Testing with Python or another web server</span></span>
> <span data-ttu-id="587db-107">Om du inte använder Visual Studio, kontrollera att servern har startats och den är konfigurerad för att lyssna på en TCP-port baserat på den mapp som innehåller din *index.html* fil.</span><span class="sxs-lookup"><span data-stu-id="587db-107">If you are not using Visual Studio, make sure your web server is started and it is configured to listen to a TCP port based on the folder containing your *index.html* file.</span></span> <span data-ttu-id="587db-108">För Python, du kan börja lyssna på porten genom att köra det i kommandot fråga / terminal, från appens mapp:</span><span class="sxs-lookup"><span data-stu-id="587db-108">For Python, you can start listening to the port by running the in the command prompt/ terminal, from the app's folder:</span></span>
> 
> ```bash
> python -m http.server 8080
> ```
>  <span data-ttu-id="587db-109">Öppna webbläsaren och skriv sedan *http://localhost: 8080* eller *http://localhost: {port}* - där den *port* motsvarar den port som servern lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="587db-109">Then, open the browser and type *http://localhost:8080* or *http://localhost:{port}* - where the *port* corresponds to the port that your web server is listening to.</span></span> <span data-ttu-id="587db-110">Du bör se innehållet i index.html sidan med de *anropa Microsoft Graph API* knappen.</span><span class="sxs-lookup"><span data-stu-id="587db-110">You should see the contents of your index.html page with the *Call Microsoft Graph API* button.</span></span>

## <a name="test-your-application"></a><span data-ttu-id="587db-111">Testa programmet</span><span class="sxs-lookup"><span data-stu-id="587db-111">Test your application</span></span>

<span data-ttu-id="587db-112">När webbläsaren läser du in din *index.html*, klicka på den *anropa Microsoft Graph API* knappen.</span><span class="sxs-lookup"><span data-stu-id="587db-112">After the browser loads your *index.html*, click the *Call Microsoft Graph API* button.</span></span> <span data-ttu-id="587db-113">Om det här är första gången omdirigerar dig till Microsoft Azure Active Directory v2 slutpunkten, där du uppmanas att logga in i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="587db-113">If this is the first time, the browser redirects you to the Microsoft Azure Active Directory v2 endpoint, where you are  prompted to sign in.</span></span>
 
![Exempel skärmbild](media/active-directory-singlepageapp-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="consent"></a><span data-ttu-id="587db-115">Medgivande</span><span class="sxs-lookup"><span data-stu-id="587db-115">Consent</span></span>
<span data-ttu-id="587db-116">Första gången du loggar in på ditt program, visas en medgivande skärm som liknar följande, där du måste acceptera:</span><span class="sxs-lookup"><span data-stu-id="587db-116">The very first time you sign in to your application, you are presented with a consent screen similar to the following, where you need to accept:</span></span>

 ![Medgivande skärmen](media/active-directory-singlepageapp-javascriptspa-test/javascriptspaconsent.png)


### <a name="expected-results"></a><span data-ttu-id="587db-118">Förväntat resultat</span><span class="sxs-lookup"><span data-stu-id="587db-118">Expected results</span></span>
<span data-ttu-id="587db-119">Du bör se användarens profilinformation som returneras av svar för Microsoft Graph API-anrop.</span><span class="sxs-lookup"><span data-stu-id="587db-119">You should see user profile information returned by the Microsoft Graph API call response.</span></span>
 
 ![Resultat](media/active-directory-singlepageapp-javascriptspa-test/javascriptsparesults.png)

<span data-ttu-id="587db-121">Du också se grundläggande information om token har införskaffade i den *åtkomst-Token* och *ID Token anspråk* rutor.</span><span class="sxs-lookup"><span data-stu-id="587db-121">You also see basic information about the token acquired in the *Access Token* and *ID Token Claims* boxes.</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="587db-122">Mer information om scope och delegerade behörigheter</span><span class="sxs-lookup"><span data-stu-id="587db-122">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="587db-123">Microsoft Graph API kräver den `user.read` omfång att läsa användarens profil.</span><span class="sxs-lookup"><span data-stu-id="587db-123">The Microsoft Graph API requires the `user.read` scope to read the user's profile.</span></span> <span data-ttu-id="587db-124">Detta scope läggs automatiskt som standard i alla program som registreras på vår registreringsportal.</span><span class="sxs-lookup"><span data-stu-id="587db-124">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="587db-125">Vissa andra API: er för Microsoft Graph samt anpassade API: er för backend-servern kan kräva ytterligare scope.</span><span class="sxs-lookup"><span data-stu-id="587db-125">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="587db-126">Till exempel för Microsoft Graph omfånget `Calendars.Read` krävs för att visa en lista med användarens kalendrar.</span><span class="sxs-lookup"><span data-stu-id="587db-126">For example, for Microsoft Graph, the scope `Calendars.Read` is required to list the user’s calendars.</span></span> <span data-ttu-id="587db-127">För att komma åt användarens kalender i kontexten för ett program måste du lägga till den `Calendars.Read` delegerad behörighet att programmet registreringsinformation och Lägg sedan till den `Calendars.Read` omfånget för den `acquireTokenSilent` anropa.</span><span class="sxs-lookup"><span data-stu-id="587db-127">In order to access the user’s calendar in the context of an application, you need to add the `Calendars.Read` delegated permission to the application registration’s information and then add the `Calendars.Read` scope to the `acquireTokenSilent` call.</span></span> <span data-ttu-id="587db-128">Användaren kan uppmanas att ytterligare medgivanden när du ökar antalet scope.</span><span class="sxs-lookup"><span data-stu-id="587db-128">The user may be prompted for additional consents as you increase the number of scopes.</span></span>

<span data-ttu-id="587db-129">Om en serverdel API inte kräver ett omfång (rekommenderas inte), kan du använda den `clientId` som scope i den `acquireTokenSilent` och/eller `acquireTokenRedirect` anrop.</span><span class="sxs-lookup"><span data-stu-id="587db-129">If a backend API does not require a scope (not recommended), you can use the `clientId` as the scope in the `acquireTokenSilent` and/or `acquireTokenRedirect` calls.</span></span>

<!--end-collapse-->
