---
title: "Azure Active Directory Identity Protection – hur du avblockerar användare | Microsoft Docs"
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
ms.openlocfilehash: ce6b2805e7281dff7752a73ada86be11d7e01fc3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-identity-protection---how-to-unblock-users"></a><span data-ttu-id="75935-104">Azure Active Directory Identity Protection - så att avblockera användare</span><span class="sxs-lookup"><span data-stu-id="75935-104">Azure Active Directory Identity Protection - How to unblock users</span></span>
<span data-ttu-id="75935-105">Med Azure Active Directory identitetsskydd, kan du konfigurera principer för att blockera användare om de konfigurerade villkor är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="75935-105">With Azure Active Directory Identity Protection, you can configure policies to block users if the configured conditions are satisfied.</span></span> <span data-ttu-id="75935-106">Normalt en blockerad användare kontakter supportavdelningen att bli blockerad.</span><span class="sxs-lookup"><span data-stu-id="75935-106">Typically, a blocked user contacts help desk to become unblocked.</span></span> <span data-ttu-id="75935-107">Detta avsnitt beskrivs de steg som du kan utföra för att låsa upp en blockerad användare.</span><span class="sxs-lookup"><span data-stu-id="75935-107">This topics explains the steps you can perform to unblock a blocked user.</span></span>

## <a name="determine-the-reason-for-blocking"></a><span data-ttu-id="75935-108">Ta reda på varför för blockering</span><span class="sxs-lookup"><span data-stu-id="75935-108">Determine the reason for blocking</span></span>
<span data-ttu-id="75935-109">Som ett första steg att avblockera en användare måste du bestämma vilken typ av princip som har blockerats användaren eftersom nästa steg är beroende av den.</span><span class="sxs-lookup"><span data-stu-id="75935-109">As a first step to unblock a user, you need to determine the type of policy that has blocked the user because your next steps are depending on it.</span></span>
<span data-ttu-id="75935-110">Med Azure Active Directory identitetsskydd, kan en användare antingen blockeras av inloggning riskprincipen eller en användarprofil för risk.</span><span class="sxs-lookup"><span data-stu-id="75935-110">With Azure Active Directory Identity Protection, a user can be either blocked by a sign-in risk policy or a user risk policy.</span></span>

<span data-ttu-id="75935-111">Du kan hämta typ av princip som har blockerat en användare från rubriken i den dialogruta som angavs för användaren under ett försök att logga in:</span><span class="sxs-lookup"><span data-stu-id="75935-111">You can get the type of policy that has blocked a user from the heading in the dialog that was presented to the user during a sign-in attempt:</span></span>

| <span data-ttu-id="75935-112">Princip</span><span class="sxs-lookup"><span data-stu-id="75935-112">Policy</span></span> | <span data-ttu-id="75935-113">Användardialogrutan</span><span class="sxs-lookup"><span data-stu-id="75935-113">User dialog</span></span> |
| --- | --- |
| <span data-ttu-id="75935-114">Logga in risk</span><span class="sxs-lookup"><span data-stu-id="75935-114">Sign-in risk</span></span> |![Blockerade inloggning](./media/active-directory-identityprotection-unblock-howto/02.png) |
| <span data-ttu-id="75935-116">Användaren risk</span><span class="sxs-lookup"><span data-stu-id="75935-116">User risk</span></span> |![Blockerade konto](./media/active-directory-identityprotection-unblock-howto/104.png) |

<span data-ttu-id="75935-118">En användare som har blockerats av:</span><span class="sxs-lookup"><span data-stu-id="75935-118">A user that is blocked by:</span></span>

* <span data-ttu-id="75935-119">Logga in riskprincipen kallas också misstänkta inloggning</span><span class="sxs-lookup"><span data-stu-id="75935-119">A sign-in risk policy is also known as suspicious sign-in</span></span>
* <span data-ttu-id="75935-120">En risk användarprincip kallas även för ett konto i fara</span><span class="sxs-lookup"><span data-stu-id="75935-120">A user risk policy is also known as an account at risk</span></span>

## <a name="unblocking-suspicious-sign-ins"></a><span data-ttu-id="75935-121">Avblockera misstänkta inloggningar</span><span class="sxs-lookup"><span data-stu-id="75935-121">Unblocking suspicious sign-ins</span></span>
<span data-ttu-id="75935-122">För att låsa upp en misstänkt inloggning, har du följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="75935-122">To unblock a suspicious sign-in, you have the following options:</span></span>

1. <span data-ttu-id="75935-123">**Logga in från en bekant plats eller en enhet** – en vanlig orsak till blockerade misstänkta inloggningar är inloggningsförsök från okända platser eller enheter.</span><span class="sxs-lookup"><span data-stu-id="75935-123">**Sign-in from a familiar location or device** - A common reason for blocked suspicious sign-ins are sign-in attempts from unfamiliar locations or devices.</span></span> <span data-ttu-id="75935-124">Användarna kan snabbt se om detta är orsaken till blockerande genom att logga in från en bekant plats eller en enhet.</span><span class="sxs-lookup"><span data-stu-id="75935-124">Your users can quickly determine whether this is the blocking reason by trying to sign-in from a familiar location or device.</span></span>
2. <span data-ttu-id="75935-125">**Undantas från principen** - om du tror att den aktuella konfigurationen av din inloggningsprincip orsakar problem för specifika användare, kan du undanta användare från den.</span><span class="sxs-lookup"><span data-stu-id="75935-125">**Exclude from policy** - If you think that the current configuration of your sign-in policy is causing issues for specific users, you can exclude the users from it.</span></span> <span data-ttu-id="75935-126">Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="75935-126">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>
3. <span data-ttu-id="75935-127">**Inaktivera principen** - om du tror att din principkonfiguration som orsakar problem för alla användare kan du inaktivera principen.</span><span class="sxs-lookup"><span data-stu-id="75935-127">**Disable policy** - If you think that your policy configuration is causing issues for all your users, you can disable the policy.</span></span> <span data-ttu-id="75935-128">Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="75935-128">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>

## <a name="unblocking-accounts-at-risk"></a><span data-ttu-id="75935-129">Avblockera konton i fara</span><span class="sxs-lookup"><span data-stu-id="75935-129">Unblocking accounts at risk</span></span>
<span data-ttu-id="75935-130">För att låsa upp ett konto i fara, har du följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="75935-130">To unblock an account at risk, you have the following options:</span></span>

1. <span data-ttu-id="75935-131">**Återställ lösenord** -du kan återställa användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="75935-131">**Reset password** - You can reset the user's password.</span></span> <span data-ttu-id="75935-132">Se [manuell säker lösenordsåterställning](active-directory-identityprotection.md#manual-secure-password-reset) för mer information.</span><span class="sxs-lookup"><span data-stu-id="75935-132">See [manual secure password reset](active-directory-identityprotection.md#manual-secure-password-reset) for more details.</span></span>
2. <span data-ttu-id="75935-133">**Ignorera alla riskhändelser** – användaren risk principen blockerar en användare om den konfigurerade användaren risknivå för blockerar åtkomsten har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="75935-133">**Dismiss all risk events** - The user risk policy blocks a user if the configured user risk level for blocking access has been reached.</span></span> <span data-ttu-id="75935-134">Du kan minska en användare har rapporterat risknivå manuellt stänger riskhändelser.</span><span class="sxs-lookup"><span data-stu-id="75935-134">You can reduce a user's risk level by manually closing reported risk events.</span></span> <span data-ttu-id="75935-135">Mer information finns i [stänger riskhändelser manuellt](active-directory-identityprotection.md#closing-risk-events-manually).</span><span class="sxs-lookup"><span data-stu-id="75935-135">For more details, see [closing risk events manually](active-directory-identityprotection.md#closing-risk-events-manually).</span></span>
3. <span data-ttu-id="75935-136">**Undantas från principen** - om du tror att den aktuella konfigurationen av din inloggningsprincip orsakar problem för specifika användare, kan du undanta användare från den.</span><span class="sxs-lookup"><span data-stu-id="75935-136">**Exclude from policy** - If you think that the current configuration of your sign-in policy is causing issues for specific users, you can exclude the users from it.</span></span> <span data-ttu-id="75935-137">Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="75935-137">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>
4. <span data-ttu-id="75935-138">**Inaktivera principen** - om du tror att din principkonfiguration som orsakar problem för alla användare kan du inaktivera principen.</span><span class="sxs-lookup"><span data-stu-id="75935-138">**Disable policy** - If you think that your policy configuration is causing issues for all your users, you can disable the policy.</span></span> <span data-ttu-id="75935-139">Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="75935-139">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="75935-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75935-140">Next steps</span></span>
 <span data-ttu-id="75935-141">Vill du veta mer om Azure AD Identity Protection?</span><span class="sxs-lookup"><span data-stu-id="75935-141">Do you want to know more about Azure AD Identity Protection?</span></span> <span data-ttu-id="75935-142">Checka ut [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="75935-142">Check out [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>
