---
title: "Azure AD v2 Android komma igång - testa | Microsoft Docs"
description: "Hur en Android-app kan få en åtkomst-token och anropa API: erna som kräver åtkomst-token från Azure Active Directory v2 slutpunkten eller Microsoft Graph API"
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
ms.openlocfilehash: 6df64f4820f8409bd8897d5ac24f81bffeeef102
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a><span data-ttu-id="ff31c-103">Testa din kod</span><span class="sxs-lookup"><span data-stu-id="ff31c-103">Test your code</span></span>

1. <span data-ttu-id="ff31c-104">Distribuera din kod till din emulatorn.</span><span class="sxs-lookup"><span data-stu-id="ff31c-104">Deploy your code to your device/emulator.</span></span>
2. <span data-ttu-id="ff31c-105">Använd när du är redo att testa Microsoft Azure Active Directory (organisationskonto) eller ett Account (live.com, outlook.com) för att logga in.</span><span class="sxs-lookup"><span data-stu-id="ff31c-105">When you're ready to test, use a Microsoft Azure Active Directory (organizational account) or a Microsoft Account (live.com, outlook.com) account to sign in.</span></span> 

<span data-ttu-id="ff31c-106">![Exempel skärmbild](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
</span><span class="sxs-lookup"><span data-stu-id="ff31c-106">![Sample screen shot](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
</span></span><br/><br/><span data-ttu-id="ff31c-107">
![Logga in](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)</span><span class="sxs-lookup"><span data-stu-id="ff31c-107">
![Sign-in](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)</span></span>

### <a name="consent"></a><span data-ttu-id="ff31c-108">Medgivande</span><span class="sxs-lookup"><span data-stu-id="ff31c-108">Consent</span></span>
<span data-ttu-id="ff31c-109">Första gången en användare loggar in på ditt program, de kommer att visas en medgivande skärm som liknar den nedan, där de måste uttryckligen acceptera:</span><span class="sxs-lookup"><span data-stu-id="ff31c-109">The first time a user signs in to your application, they will be presented with a consent screen similar to the below, where they need to explicitly accept:</span></span> 

![Medgivande](media/active-directory-mobileanddesktopapp-android-test/androidconsent.png)


### <a name="expected-results"></a><span data-ttu-id="ff31c-111">Förväntat resultat</span><span class="sxs-lookup"><span data-stu-id="ff31c-111">Expected results</span></span>
<span data-ttu-id="ff31c-112">Du bör se resultatet av ett anrop till Microsoft Graph API ”jag” slutpunkt som används till att hämta användarprofil - https://graph.microsoft.com/v1.0/me.</span><span class="sxs-lookup"><span data-stu-id="ff31c-112">You should see the results of a call to Microsoft Graph API ‘me’ endpoint used to to obtain the user profile - https://graph.microsoft.com/v1.0/me.</span></span> <span data-ttu-id="ff31c-113">En lista över vanliga Microsoft Graph slutpunkter finns [artikel](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).</span><span class="sxs-lookup"><span data-stu-id="ff31c-113">For a list of common Microsoft Graph endpoints, please see this [article](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="ff31c-114">Mer information om scope och delegerade behörigheter</span><span class="sxs-lookup"><span data-stu-id="ff31c-114">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="ff31c-115">Microsoft Graph API kräver den `user.read` omfång att läsa användarens profil.</span><span class="sxs-lookup"><span data-stu-id="ff31c-115">The Microsoft Graph API requires the `user.read` scope to read the user's profile.</span></span> <span data-ttu-id="ff31c-116">Detta scope läggs automatiskt som standard i alla program som registreras på vår registreringsportal.</span><span class="sxs-lookup"><span data-stu-id="ff31c-116">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="ff31c-117">Vissa andra API: er för Microsoft Graph samt anpassade API: er för backend-servern kan kräva ytterligare scope.</span><span class="sxs-lookup"><span data-stu-id="ff31c-117">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="ff31c-118">Till exempel för Microsoft Graph omfånget `Calendars.Read` krävs för att visa en lista med användarens kalendrar.</span><span class="sxs-lookup"><span data-stu-id="ff31c-118">For example, for Microsoft Graph, the scope `Calendars.Read` is required to list the user’s calendars.</span></span> <span data-ttu-id="ff31c-119">För att komma åt användarens kalender i en kontext med ett program måste du lägga till den `Calendars.Read` delegerad behörighet att programmet registreringsinformation och Lägg sedan till den `Calendars.Read` omfånget för den `acquireTokenSilentAsync` anropa.</span><span class="sxs-lookup"><span data-stu-id="ff31c-119">In order to access the user’s calendar in a context of an application, you need to add the `Calendars.Read` delegated permission to the application registration’s information and then add the `Calendars.Read` scope to the `acquireTokenSilentAsync` call.</span></span> <span data-ttu-id="ff31c-120">Användaren kan uppmanas att ytterligare medgivanden när du ökar antalet scope.</span><span class="sxs-lookup"><span data-stu-id="ff31c-120">The user may be prompted for additional consents as you increase the number of scopes.</span></span>

<!--end-collapse-->
