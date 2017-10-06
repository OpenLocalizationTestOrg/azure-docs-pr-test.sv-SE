---
title: "aaaAzure AD v2 Android komma igång - Test | Microsoft Docs"
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
ms.openlocfilehash: 499f32b46fd44cca0e52179bced49b311135d8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a><span data-ttu-id="ddbe8-103">Testa din kod</span><span class="sxs-lookup"><span data-stu-id="ddbe8-103">Test your code</span></span>

1. <span data-ttu-id="ddbe8-104">Distribuera din kod tooyour/emulatorn.</span><span class="sxs-lookup"><span data-stu-id="ddbe8-104">Deploy your code tooyour device/emulator.</span></span>
2. <span data-ttu-id="ddbe8-105">När du är klar tootest kan använda Microsoft Azure Active Directory (organisationskonto) eller en Account (live.com, outlook.com) konto toosign i.</span><span class="sxs-lookup"><span data-stu-id="ddbe8-105">When you're ready tootest, use a Microsoft Azure Active Directory (organizational account) or a Microsoft Account (live.com, outlook.com) account toosign in.</span></span> 

<span data-ttu-id="ddbe8-106">![Exempel skärmbild](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
</span><span class="sxs-lookup"><span data-stu-id="ddbe8-106">![Sample screen shot](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
</span></span><br/><br/><span data-ttu-id="ddbe8-107">
![Logga in](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)</span><span class="sxs-lookup"><span data-stu-id="ddbe8-107">
![Sign-in](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)</span></span>

### <a name="consent"></a><span data-ttu-id="ddbe8-108">Medgivande</span><span class="sxs-lookup"><span data-stu-id="ddbe8-108">Consent</span></span>
<span data-ttu-id="ddbe8-109">hello första gången en användare loggar in tooyour program, de kommer att visas ett medgivande skärm liknande toohello nedan, där de måste acceptera tooexplicitly:</span><span class="sxs-lookup"><span data-stu-id="ddbe8-109">hello first time a user signs in tooyour application, they will be presented with a consent screen similar toohello below, where they need tooexplicitly accept:</span></span> 

![Medgivande](media/active-directory-mobileanddesktopapp-android-test/androidconsent.png)


### <a name="expected-results"></a><span data-ttu-id="ddbe8-111">Förväntat resultat</span><span class="sxs-lookup"><span data-stu-id="ddbe8-111">Expected results</span></span>
<span data-ttu-id="ddbe8-112">Du bör se hello resultatet av ett anrop tooMicrosoft Graph API ”jag” endpoint används tootooobtain hello användarprofil - https://graph.microsoft.com/v1.0/me.</span><span class="sxs-lookup"><span data-stu-id="ddbe8-112">You should see hello results of a call tooMicrosoft Graph API ‘me’ endpoint used tootooobtain hello user profile - https://graph.microsoft.com/v1.0/me.</span></span> <span data-ttu-id="ddbe8-113">En lista över vanliga Microsoft Graph slutpunkter finns [artikel](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).</span><span class="sxs-lookup"><span data-stu-id="ddbe8-113">For a list of common Microsoft Graph endpoints, please see this [article](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="ddbe8-114">Mer information om scope och delegerade behörigheter</span><span class="sxs-lookup"><span data-stu-id="ddbe8-114">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="ddbe8-115">hello Microsoft Graph API kräver hello `user.read` omfång tooread hello användarens profil.</span><span class="sxs-lookup"><span data-stu-id="ddbe8-115">hello Microsoft Graph API requires hello `user.read` scope tooread hello user's profile.</span></span> <span data-ttu-id="ddbe8-116">Detta scope läggs automatiskt som standard i alla program som registreras på vår registreringsportal.</span><span class="sxs-lookup"><span data-stu-id="ddbe8-116">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="ddbe8-117">Vissa andra API: er för Microsoft Graph samt anpassade API: er för backend-servern kan kräva ytterligare scope.</span><span class="sxs-lookup"><span data-stu-id="ddbe8-117">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="ddbe8-118">Till exempel hello omfånget för Microsoft Graph `Calendars.Read` är nödvändiga toolist hello användares kalendrar.</span><span class="sxs-lookup"><span data-stu-id="ddbe8-118">For example, for Microsoft Graph, hello scope `Calendars.Read` is required toolist hello user’s calendars.</span></span> <span data-ttu-id="ddbe8-119">I ordning tooaccess hello användarens kalender i en kontext med ett program måste du tooadd hello `Calendars.Read` delegerad behörighet toohello programmet registreringens information och Lägg sedan till hello `Calendars.Read` omfång toohello `acquireTokenSilentAsync` anropa.</span><span class="sxs-lookup"><span data-stu-id="ddbe8-119">In order tooaccess hello user’s calendar in a context of an application, you need tooadd hello `Calendars.Read` delegated permission toohello application registration’s information and then add hello `Calendars.Read` scope toohello `acquireTokenSilentAsync` call.</span></span> <span data-ttu-id="ddbe8-120">hello användare kan uppmanas att ytterligare medgivanden när du ökar hello antal scope.</span><span class="sxs-lookup"><span data-stu-id="ddbe8-120">hello user may be prompted for additional consents as you increase hello number of scopes.</span></span>

<!--end-collapse-->
