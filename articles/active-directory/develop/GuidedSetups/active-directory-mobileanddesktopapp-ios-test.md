---
title: "aaaAzure AD v2 iOS komma igång - Test | Microsoft Docs"
description: "Hur iOS (Swift)-program kan anropa ett API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten"
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
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 98c73eddabf9664feb19ac6878e9d7315b9aa79b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="test-querying-hello-microsoft-graph-api-from-your-ios-application"></a><span data-ttu-id="c8b5d-103">Testa fråga hello Microsoft Graph API från iOS-program</span><span class="sxs-lookup"><span data-stu-id="c8b5d-103">Test querying hello Microsoft Graph API from your iOS application</span></span>

<span data-ttu-id="c8b5d-104">Tryck på `Command`  +  `R` toorun hello koden i hello-simulatorn.</span><span class="sxs-lookup"><span data-stu-id="c8b5d-104">Press `Command` + `R` toorun hello code in hello simulator.</span></span>

![Exempel skärmbild](media/active-directory-mobileanddesktopapp-ios-test/iostestscreenshot.png)

<span data-ttu-id="c8b5d-106">När du är klar tootest trycker du på *'anropa Microsoft Graph API-* och du kommer att tillfrågas tootype ditt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="c8b5d-106">When you're ready tootest, tap *‘Call Microsoft Graph API’* and you will be prompted tootype your username and password.</span></span>

### <a name="consent"></a><span data-ttu-id="c8b5d-107">Medgivande</span><span class="sxs-lookup"><span data-stu-id="c8b5d-107">Consent</span></span>
<span data-ttu-id="c8b5d-108">hello första gången du loggar in tooyour program visas med en medgivande skärm liknande toohello nedan, där du måste acceptera tooexplicitly:</span><span class="sxs-lookup"><span data-stu-id="c8b5d-108">hello first time you sign in tooyour application, you will be presented with a consent screen similar toohello below, where you need tooexplicitly accept:</span></span>

![Medgivande skärmen](media/active-directory-mobileanddesktopapp-ios-test/iosconsentscreen.png)

### <a name="expected-results"></a><span data-ttu-id="c8b5d-110">Förväntat resultat</span><span class="sxs-lookup"><span data-stu-id="c8b5d-110">Expected results</span></span>
<span data-ttu-id="c8b5d-111">Du bör se användarens profilinformation som returneras av hello Microsoft Graph API-anrop i hello *loggning* avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c8b5d-111">You should see user profile information returned by hello Microsoft Graph API call in hello *Logging* section.</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="c8b5d-112">Mer information om scope och delegerade behörigheter</span><span class="sxs-lookup"><span data-stu-id="c8b5d-112">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="c8b5d-113">hello Microsoft Graph API kräver hello `user.read` omfång tooread hello användarens profil.</span><span class="sxs-lookup"><span data-stu-id="c8b5d-113">hello Microsoft Graph API requires hello `user.read` scope tooread hello user's profile.</span></span> <span data-ttu-id="c8b5d-114">Detta scope läggs automatiskt som standard i alla program som registreras på vår registreringsportal.</span><span class="sxs-lookup"><span data-stu-id="c8b5d-114">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="c8b5d-115">Vissa andra API: er för Microsoft Graph samt anpassade API: er för backend-servern kan kräva ytterligare scope.</span><span class="sxs-lookup"><span data-stu-id="c8b5d-115">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="c8b5d-116">Till exempel hello omfånget för Microsoft Graph `Calendars.Read` är nödvändiga toolist hello användares kalendrar.</span><span class="sxs-lookup"><span data-stu-id="c8b5d-116">For example, for Microsoft Graph, hello scope `Calendars.Read` is required toolist hello user’s calendars.</span></span> <span data-ttu-id="c8b5d-117">I ordning tooaccess hello användarens kalender i en kontext med ett program måste du tooadd hello `Calendars.Read` delegerad behörighet toohello programmet registreringens information och Lägg sedan till hello `Calendars.Read` omfång toohello `acquireTokenSilent` anropa.</span><span class="sxs-lookup"><span data-stu-id="c8b5d-117">In order tooaccess hello user’s calendar in a context of an application, you need tooadd hello `Calendars.Read` delegated permission toohello application registration’s information and then add hello `Calendars.Read` scope toohello `acquireTokenSilent` call.</span></span> <span data-ttu-id="c8b5d-118">hello användare kan uppmanas att ytterligare medgivanden när du ökar hello antal scope.</span><span class="sxs-lookup"><span data-stu-id="c8b5d-118">hello user may be prompted for additional consents as you increase hello number of scopes.</span></span>

<!--end-collapse-->



