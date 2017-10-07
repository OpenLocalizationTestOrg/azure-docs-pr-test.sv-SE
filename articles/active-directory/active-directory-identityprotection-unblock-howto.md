---
title: "aaaAzure identitetsskydd för Active Directory - hur toounblock användare | Microsoft Docs"
description: "Lär dig hur avblockera användare som har blockerats av en princip för Azure Active Directory Identity Protection."
services: active-directory
keywords: "Azure active directory identitetsskydd avblockera användare"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: a953d425-a3ef-41f8-a55d-0202c3f250a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: cdda2808822888f76aa75cf46478738c94df51a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection---how-toounblock-users"></a><span data-ttu-id="aadc1-104">Azure Active Directory Identity Protection - hur toounblock användare</span><span class="sxs-lookup"><span data-stu-id="aadc1-104">Azure Active Directory Identity Protection - How toounblock users</span></span>
<span data-ttu-id="aadc1-105">Med Azure Active Directory identitetsskydd, kan du konfigurera principer tooblock användare om hello konfigurerad villkor är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="aadc1-105">With Azure Active Directory Identity Protection, you can configure policies tooblock users if hello configured conditions are satisfied.</span></span> <span data-ttu-id="aadc1-106">Normalt en blockerad användare kontakter help desk toobecome avblockerad.</span><span class="sxs-lookup"><span data-stu-id="aadc1-106">Typically, a blocked user contacts help desk toobecome unblocked.</span></span> <span data-ttu-id="aadc1-107">Detta avsnitt beskrivs hello steg som du kan utföra toounblock en blockerad användare.</span><span class="sxs-lookup"><span data-stu-id="aadc1-107">This topics explains hello steps you can perform toounblock a blocked user.</span></span>

## <a name="determine-hello-reason-for-blocking"></a><span data-ttu-id="aadc1-108">Ta reda på orsaken hello för blockering</span><span class="sxs-lookup"><span data-stu-id="aadc1-108">Determine hello reason for blocking</span></span>
<span data-ttu-id="aadc1-109">Som ett första steg toounblock en användare måste toodetermine hello typen av princip som har blockerats hello användaren eftersom nästa steg är beroende av den.</span><span class="sxs-lookup"><span data-stu-id="aadc1-109">As a first step toounblock a user, you need toodetermine hello type of policy that has blocked hello user because your next steps are depending on it.</span></span>
<span data-ttu-id="aadc1-110">Med Azure Active Directory identitetsskydd, kan en användare antingen blockeras av inloggning riskprincipen eller en användarprofil för risk.</span><span class="sxs-lookup"><span data-stu-id="aadc1-110">With Azure Active Directory Identity Protection, a user can be either blocked by a sign-in risk policy or a user risk policy.</span></span>

<span data-ttu-id="aadc1-111">Du kan få hello typen av princip som har blockerat en användare från hello rubriken i hello dialogruta som angavs toohello användaren under ett försök att logga in:</span><span class="sxs-lookup"><span data-stu-id="aadc1-111">You can get hello type of policy that has blocked a user from hello heading in hello dialog that was presented toohello user during a sign-in attempt:</span></span>

| <span data-ttu-id="aadc1-112">Princip</span><span class="sxs-lookup"><span data-stu-id="aadc1-112">Policy</span></span> | <span data-ttu-id="aadc1-113">Användardialogrutan</span><span class="sxs-lookup"><span data-stu-id="aadc1-113">User dialog</span></span> |
| --- | --- |
| <span data-ttu-id="aadc1-114">Logga in risk</span><span class="sxs-lookup"><span data-stu-id="aadc1-114">Sign-in risk</span></span> |![Blockerade inloggning](./media/active-directory-identityprotection-unblock-howto/02.png) |
| <span data-ttu-id="aadc1-116">Användaren risk</span><span class="sxs-lookup"><span data-stu-id="aadc1-116">User risk</span></span> |![Blockerade konto](./media/active-directory-identityprotection-unblock-howto/104.png) |

<span data-ttu-id="aadc1-118">En användare som har blockerats av:</span><span class="sxs-lookup"><span data-stu-id="aadc1-118">A user that is blocked by:</span></span>

* <span data-ttu-id="aadc1-119">Logga in riskprincipen kallas också misstänkta inloggning</span><span class="sxs-lookup"><span data-stu-id="aadc1-119">A sign-in risk policy is also known as suspicious sign-in</span></span>
* <span data-ttu-id="aadc1-120">En risk användarprincip kallas även för ett konto i fara</span><span class="sxs-lookup"><span data-stu-id="aadc1-120">A user risk policy is also known as an account at risk</span></span>

## <a name="unblocking-suspicious-sign-ins"></a><span data-ttu-id="aadc1-121">Avblockera misstänkta inloggningar</span><span class="sxs-lookup"><span data-stu-id="aadc1-121">Unblocking suspicious sign-ins</span></span>
<span data-ttu-id="aadc1-122">toounblock en misstänkt inloggning har hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="aadc1-122">toounblock a suspicious sign-in, you have hello following options:</span></span>

1. <span data-ttu-id="aadc1-123">**Logga in från en bekant plats eller en enhet** – en vanlig orsak till blockerade misstänkta inloggningar är inloggningsförsök från okända platser eller enheter.</span><span class="sxs-lookup"><span data-stu-id="aadc1-123">**Sign-in from a familiar location or device** - A common reason for blocked suspicious sign-ins are sign-in attempts from unfamiliar locations or devices.</span></span> <span data-ttu-id="aadc1-124">Användarna kan snabbt se om detta är hello blockerar orsak genom toosign i från en bekant plats eller en enhet.</span><span class="sxs-lookup"><span data-stu-id="aadc1-124">Your users can quickly determine whether this is hello blocking reason by trying toosign-in from a familiar location or device.</span></span>
2. <span data-ttu-id="aadc1-125">**Undantas från principen** - om du tror att hello aktuella konfigurationen av din inloggningsprincip orsakar problem för specifika användare, kan du utesluta hello användare från den.</span><span class="sxs-lookup"><span data-stu-id="aadc1-125">**Exclude from policy** - If you think that hello current configuration of your sign-in policy is causing issues for specific users, you can exclude hello users from it.</span></span> <span data-ttu-id="aadc1-126">Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="aadc1-126">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>
3. <span data-ttu-id="aadc1-127">**Inaktivera principen** - om du tror att din principkonfiguration som orsakar problem för alla användare kan du inaktivera hello princip.</span><span class="sxs-lookup"><span data-stu-id="aadc1-127">**Disable policy** - If you think that your policy configuration is causing issues for all your users, you can disable hello policy.</span></span> <span data-ttu-id="aadc1-128">Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="aadc1-128">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>

## <a name="unblocking-accounts-at-risk"></a><span data-ttu-id="aadc1-129">Avblockera konton i fara</span><span class="sxs-lookup"><span data-stu-id="aadc1-129">Unblocking accounts at risk</span></span>
<span data-ttu-id="aadc1-130">toounblock ett konto i fara har hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="aadc1-130">toounblock an account at risk, you have hello following options:</span></span>

1. <span data-ttu-id="aadc1-131">**Återställ lösenord** -du kan återställa hello användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="aadc1-131">**Reset password** - You can reset hello user's password.</span></span> <span data-ttu-id="aadc1-132">Se [manuell säker lösenordsåterställning](active-directory-identityprotection.md#manual-secure-password-reset) för mer information.</span><span class="sxs-lookup"><span data-stu-id="aadc1-132">See [manual secure password reset](active-directory-identityprotection.md#manual-secure-password-reset) for more details.</span></span>
2. <span data-ttu-id="aadc1-133">**Ignorera alla riskhändelser** -hello användarprincip risk blockerar en användare om hello konfigurerade användaren risknivå för blockerar åtkomsten har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="aadc1-133">**Dismiss all risk events** - hello user risk policy blocks a user if hello configured user risk level for blocking access has been reached.</span></span> <span data-ttu-id="aadc1-134">Du kan minska en användare har rapporterat risknivå manuellt stänger riskhändelser.</span><span class="sxs-lookup"><span data-stu-id="aadc1-134">You can reduce a user's risk level by manually closing reported risk events.</span></span> <span data-ttu-id="aadc1-135">Mer information finns i [stänger riskhändelser manuellt](active-directory-identityprotection.md#closing-risk-events-manually).</span><span class="sxs-lookup"><span data-stu-id="aadc1-135">For more details, see [closing risk events manually](active-directory-identityprotection.md#closing-risk-events-manually).</span></span>
3. <span data-ttu-id="aadc1-136">**Undantas från principen** - om du tror att hello aktuella konfigurationen av din inloggningsprincip orsakar problem för specifika användare, kan du utesluta hello användare från den.</span><span class="sxs-lookup"><span data-stu-id="aadc1-136">**Exclude from policy** - If you think that hello current configuration of your sign-in policy is causing issues for specific users, you can exclude hello users from it.</span></span> <span data-ttu-id="aadc1-137">Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="aadc1-137">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>
4. <span data-ttu-id="aadc1-138">**Inaktivera principen** - om du tror att din principkonfiguration som orsakar problem för alla användare kan du inaktivera hello princip.</span><span class="sxs-lookup"><span data-stu-id="aadc1-138">**Disable policy** - If you think that your policy configuration is causing issues for all your users, you can disable hello policy.</span></span> <span data-ttu-id="aadc1-139">Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="aadc1-139">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aadc1-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aadc1-140">Next steps</span></span>
 <span data-ttu-id="aadc1-141">Vill du tooknow mer om Azure AD Identity Protection?</span><span class="sxs-lookup"><span data-stu-id="aadc1-141">Do you want tooknow more about Azure AD Identity Protection?</span></span> <span data-ttu-id="aadc1-142">Checka ut [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="aadc1-142">Check out [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>
