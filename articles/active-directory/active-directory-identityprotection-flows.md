---
title: aaaSign i upplevelser med Azure AD Identity Protection | Microsoft Docs
description: "Ger en översikt över hello användarupplevelsen när identitetsskydd har minskas eller åtgärdad en användare eller när multifaktorautentisering krävs av en princip."
services: active-directory
keywords: "Azure active directory identitetsskydd, cloud app discovery, hantera program, säkerhet, risk, risknivå, säkerhetsproblem och säkerhetsprincip"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: de5bf637-75a7-4104-b6d8-03686372a319
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: fbdca5b86ed93d0a2f2b6df1dd0150da9c0c85c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a><span data-ttu-id="93ad5-104">Logga in som inträffar med Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="93ad5-104">Sign-in experiences with Azure AD Identity Protection</span></span>
<span data-ttu-id="93ad5-105">Med Azure Active Directory identitetsskydd kan du:</span><span class="sxs-lookup"><span data-stu-id="93ad5-105">With Azure Active Directory Identity Protection, you can:</span></span>

* <span data-ttu-id="93ad5-106">Kräv användare tooregister för multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="93ad5-106">require users tooregister for multi-factor authentication</span></span>
* <span data-ttu-id="93ad5-107">Hantera riskfyllda inloggningar och avslöjade användare</span><span class="sxs-lookup"><span data-stu-id="93ad5-107">handle risky sign-ins and compromised users</span></span>

<span data-ttu-id="93ad5-108">hello svar hello system toothese problem påverkar på en användares inloggning eftersom bara direkt logga in genom att ange ett användarnamn och lösenord inte är möjligt längre.</span><span class="sxs-lookup"><span data-stu-id="93ad5-108">hello response of hello system toothese issues has an impact on a user's sign-in experience because just directly signing-in by providing a user name and a password won't be possible anymore.</span></span> <span data-ttu-id="93ad5-109">Ytterligare steg är nödvändiga tooget som en användare på ett säkert sätt tillbaka till företag.</span><span class="sxs-lookup"><span data-stu-id="93ad5-109">Additional steps are required tooget a user safely back into business.</span></span>

<span data-ttu-id="93ad5-110">Det här avsnittet ger en översikt över en användares inloggning i samtliga fall som kan uppstå.</span><span class="sxs-lookup"><span data-stu-id="93ad5-110">This topic gives you an overview of a user's sign-in experience for all cases that can occur.</span></span>

<span data-ttu-id="93ad5-111">**Multi-Factor Authentication**</span><span class="sxs-lookup"><span data-stu-id="93ad5-111">**Multi-factor authentication**</span></span>

* <span data-ttu-id="93ad5-112">Registreringen för multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="93ad5-112">Multi-factor authentication registration</span></span>

<span data-ttu-id="93ad5-113">**Logga in i fara**</span><span class="sxs-lookup"><span data-stu-id="93ad5-113">**Sign-in at risk**</span></span>

* <span data-ttu-id="93ad5-114">Återställning för riskfyllda inloggning</span><span class="sxs-lookup"><span data-stu-id="93ad5-114">Risky sign-in recovery</span></span>
* <span data-ttu-id="93ad5-115">Riskfyllda inloggning blockeras</span><span class="sxs-lookup"><span data-stu-id="93ad5-115">Risky sign-in blocked</span></span>
* <span data-ttu-id="93ad5-116">Multifaktorautentisering registrering under en riskfyllda inloggning</span><span class="sxs-lookup"><span data-stu-id="93ad5-116">Multi-factor authentication registration during a risky sign-in</span></span>

<span data-ttu-id="93ad5-117">**Användare i fara**</span><span class="sxs-lookup"><span data-stu-id="93ad5-117">**User at risk**</span></span>

* <span data-ttu-id="93ad5-118">Komprometterat kontoåterställning</span><span class="sxs-lookup"><span data-stu-id="93ad5-118">Compromised account recovery</span></span>
* <span data-ttu-id="93ad5-119">Komprometterat konto blockeras</span><span class="sxs-lookup"><span data-stu-id="93ad5-119">Compromised account blocked</span></span>

## <a name="multi-factor-authentication-registration"></a><span data-ttu-id="93ad5-120">Registreringen för multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="93ad5-120">Multi-factor authentication registration</span></span>
<span data-ttu-id="93ad5-121">Hej bästa användarupplevelsen för både, hello komprometterat konto recovery flödet och hello flödet för riskfyllda inloggning, när hello användaren själv kan återställa.</span><span class="sxs-lookup"><span data-stu-id="93ad5-121">hello best user experience for both, hello compromised account recovery flow and hello risky sign-in flow, is when hello user can self-recover.</span></span> <span data-ttu-id="93ad5-122">Om användarna har registrerats för multifaktorautentisering, har de redan ett telefonnummer som är associerad med ett konto som kan använda toopass säkerhetsutmaningar.</span><span class="sxs-lookup"><span data-stu-id="93ad5-122">If users are registered for multi-factor authentication, they already have a phone number associated with their account that can be used toopass security challenges.</span></span> <span data-ttu-id="93ad5-123">Ingen hjälp supportavdelningen eller administratören inblandning är nödvändiga toorecover från kontot angrepp.</span><span class="sxs-lookup"><span data-stu-id="93ad5-123">No help desk or administrator involvement is needed toorecover from account compromise.</span></span> <span data-ttu-id="93ad5-124">Därför har hög rekommendationen tooget användarna registrerad för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="93ad5-124">Thus, it’s highly recommended tooget your users registered for multi-factor authentication.</span></span> 

<span data-ttu-id="93ad5-125">Administratörer kan:</span><span class="sxs-lookup"><span data-stu-id="93ad5-125">Administrators can:</span></span>

* <span data-ttu-id="93ad5-126">Ange en princip som kräver att användare tooset upp sina konton för ytterligare säkerhetsverifiering.</span><span class="sxs-lookup"><span data-stu-id="93ad5-126">set a policy that requires users tooset up their accounts for additional security verification.</span></span> 
* <span data-ttu-id="93ad5-127">Tillåt hoppa över multifaktorautentisering registrering för in too30 dagar, om de vill toogive användare respitperiod innan du registrerar.</span><span class="sxs-lookup"><span data-stu-id="93ad5-127">allow skipping multi-factor authentication registration for up too30 days, in case they want toogive users a grace period before registering.</span></span>

<span data-ttu-id="93ad5-128">**Hej multifaktorautentisering registrering har tre steg:**</span><span class="sxs-lookup"><span data-stu-id="93ad5-128">**hello multi-factor authentication registration has three steps:**</span></span>

1. <span data-ttu-id="93ad5-129">I första steget hello får hello användaren ett meddelande om hello krav tooset hello kontot för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="93ad5-129">In hello first step, hello user gets a notification about hello requirement tooset hello account up for multi-factor authentication.</span></span> 
   
    <span data-ttu-id="93ad5-130">![Reparation](./media/active-directory-identityprotection-flows/140.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="93ad5-130">![Remediation](./media/active-directory-identityprotection-flows/140.png "Remediation")</span></span>
2. <span data-ttu-id="93ad5-131">tooset multifaktorautentisering upp, behöver du toolet hello systemet vet du hur toobe kontaktas.</span><span class="sxs-lookup"><span data-stu-id="93ad5-131">tooset multi-factor authentication up, you need toolet hello system know how you want toobe contacted.</span></span>
   
    <span data-ttu-id="93ad5-132">![Reparation](./media/active-directory-identityprotection-flows/141.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="93ad5-132">![Remediation](./media/active-directory-identityprotection-flows/141.png "Remediation")</span></span>
3. <span data-ttu-id="93ad5-133">hello system skickar en utmaning tooyou och du behöver toorespond.</span><span class="sxs-lookup"><span data-stu-id="93ad5-133">hello system submits a challenge tooyou and you need toorespond.</span></span>
   
    <span data-ttu-id="93ad5-134">![Reparation](./media/active-directory-identityprotection-flows/142.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="93ad5-134">![Remediation](./media/active-directory-identityprotection-flows/142.png "Remediation")</span></span>

## <a name="risky-sign-in-recovery"></a><span data-ttu-id="93ad5-135">Återställning för riskfyllda inloggning</span><span class="sxs-lookup"><span data-stu-id="93ad5-135">Risky sign-in recovery</span></span>
<span data-ttu-id="93ad5-136">När en administratör har konfigurerat en princip för inloggning risker, meddelas hello påverkade användare när de försöker toosign i.</span><span class="sxs-lookup"><span data-stu-id="93ad5-136">When an administrator has configured a policy for sign-in risks, hello affected users are notified when they try toosign-in.</span></span> 

<span data-ttu-id="93ad5-137">**hello riskfyllda inloggning flödet har två steg:**</span><span class="sxs-lookup"><span data-stu-id="93ad5-137">**hello risky sign-in flow has two steps:**</span></span> 

1. <span data-ttu-id="93ad5-138">hello användare informeras om att något annorlunda identifierades om deras inloggning, till exempel loggar in från en ny plats, enhet eller app.</span><span class="sxs-lookup"><span data-stu-id="93ad5-138">hello user is informed that something unusual was detected about their sign-in, such as signing in from a new location, device, or app.</span></span> 
   
    <span data-ttu-id="93ad5-139">![Reparation](./media/active-directory-identityprotection-flows/120.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="93ad5-139">![Remediation](./media/active-directory-identityprotection-flows/120.png "Remediation")</span></span>
2. <span data-ttu-id="93ad5-140">hello användaren är obligatoriska tooprove sin identitet genom att lösa en säkerhetskontrollen.</span><span class="sxs-lookup"><span data-stu-id="93ad5-140">hello user is required tooprove their identity by solving a security challenge.</span></span> <span data-ttu-id="93ad5-141">Om hello användare är registrerad för multi-Factor authentication måste de tooround resa säkerhet kod tootheir telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="93ad5-141">If hello user is registered for multi-factor authentication they need tooround-trip a security code tootheir phone number.</span></span> <span data-ttu-id="93ad5-142">Eftersom det är bara en riskfyllda inloggning och komprometterat konto, hello användaren inte har toochange hello lösenord i det här flödet.</span><span class="sxs-lookup"><span data-stu-id="93ad5-142">Since this is a just a risky sign in and not a compromised account, hello user won’t have toochange hello password in this flow.</span></span> 
   
    <span data-ttu-id="93ad5-143">![Reparation](./media/active-directory-identityprotection-flows/121.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="93ad5-143">![Remediation](./media/active-directory-identityprotection-flows/121.png "Remediation")</span></span>

## <a name="risky-sign-in-blocked"></a><span data-ttu-id="93ad5-144">Riskfyllda inloggning blockeras</span><span class="sxs-lookup"><span data-stu-id="93ad5-144">Risky sign-in blocked</span></span>
<span data-ttu-id="93ad5-145">Administratörer kan också välja tooset inloggning Risk princip tooblock användare vid inloggning beroende på hello risknivå.</span><span class="sxs-lookup"><span data-stu-id="93ad5-145">Administrators can also choose tooset a Sign-In Risk policy tooblock users upon sign-in depending on hello risk level.</span></span> <span data-ttu-id="93ad5-146">tooget avblockerad, slutanvändare måste kontakta en administratör eller supportavdelning eller de kan försöka logga in från en bekant plats eller en enhet.</span><span class="sxs-lookup"><span data-stu-id="93ad5-146">tooget unblocked, end users must contact an administrator or help desk, or they can try signing in from a familiar location or device.</span></span> <span data-ttu-id="93ad5-147">Automatisk återställning av lösa multifaktorautentisering är inte ett alternativ i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="93ad5-147">Self-recovering by solving multi-factor authentication is not an option in this case.</span></span>

<span data-ttu-id="93ad5-148">![Reparation](./media/active-directory-identityprotection-flows/200.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="93ad5-148">![Remediation](./media/active-directory-identityprotection-flows/200.png "Remediation")</span></span>

## <a name="compromised-account-recovery"></a><span data-ttu-id="93ad5-149">Komprometterat kontoåterställning</span><span class="sxs-lookup"><span data-stu-id="93ad5-149">Compromised account recovery</span></span>
<span data-ttu-id="93ad5-150">När en säkerhetsprincip för användaren risk har konfigurerats kan användare som uppfyller hello användaren risknivå anges i hello policy (och därför antas komprometteras) måste gå igenom hello användaren röjande recovery flödet innan de kan logga in.</span><span class="sxs-lookup"><span data-stu-id="93ad5-150">When a user risk security policy has been configured, users who meet hello user risk level specified in hello policy (and are therefore assumed compromised) must go through hello user compromise recovery flow before they can sign-in.</span></span> 

<span data-ttu-id="93ad5-151">**hello användaren röjande recovery flödet har tre steg:**</span><span class="sxs-lookup"><span data-stu-id="93ad5-151">**hello user compromise recovery flow has three steps:**</span></span>

1. <span data-ttu-id="93ad5-152">hello användare informeras om att deras kontosäkerhet är i fara på grund av misstänkt aktivitet eller läcka ut autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="93ad5-152">hello user is informed that their account security is at risk because of suspicious activity or leaked credentials.</span></span>
   
    <span data-ttu-id="93ad5-153">![Reparation](./media/active-directory-identityprotection-flows/101.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="93ad5-153">![Remediation](./media/active-directory-identityprotection-flows/101.png "Remediation")</span></span>
2. <span data-ttu-id="93ad5-154">hello användaren är obligatoriska tooprove sin identitet genom att lösa en säkerhetskontrollen.</span><span class="sxs-lookup"><span data-stu-id="93ad5-154">hello user is required tooprove their identity by solving a security challenge.</span></span> <span data-ttu-id="93ad5-155">Om hello användare är registrerad för multifaktorautentisering återställa de själva från komprometteras.</span><span class="sxs-lookup"><span data-stu-id="93ad5-155">If hello user is registered for multi-factor authentication they can self-recover from being compromised.</span></span> <span data-ttu-id="93ad5-156">De måste tooround resa säkerhet kod tootheir telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="93ad5-156">They will need tooround-trip a security code tootheir phone number.</span></span> 
   
   <span data-ttu-id="93ad5-157">![Reparation](./media/active-directory-identityprotection-flows/110.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="93ad5-157">![Remediation](./media/active-directory-identityprotection-flows/110.png "Remediation")</span></span>
3. <span data-ttu-id="93ad5-158">Slutligen hello användaren är framtvingad toochange sitt lösenord eftersom någon annan kan ha haft åtkomst tootheir konto.</span><span class="sxs-lookup"><span data-stu-id="93ad5-158">Finally, hello user is forced toochange their password since someone else may have had access tootheir account.</span></span> 
   <span data-ttu-id="93ad5-159">Skärmdumpar av det här upplevelsen är lägre än.</span><span class="sxs-lookup"><span data-stu-id="93ad5-159">Screenshots of this experience are below.</span></span>
   
   <span data-ttu-id="93ad5-160">![Reparation](./media/active-directory-identityprotection-flows/111.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="93ad5-160">![Remediation](./media/active-directory-identityprotection-flows/111.png "Remediation")</span></span>

## <a name="compromised-account-blocked"></a><span data-ttu-id="93ad5-161">Komprometterat konto blockeras</span><span class="sxs-lookup"><span data-stu-id="93ad5-161">Compromised account blocked</span></span>
<span data-ttu-id="93ad5-162">tooget en användare som har blockerats av en användare risk säkerhetsprincip avblockerad hello användaren måste kontakta en administratör eller supportavdelningen.</span><span class="sxs-lookup"><span data-stu-id="93ad5-162">tooget a user that was blocked by a user risk security policy unblocked, hello user must contact an administrator or help desk.</span></span> <span data-ttu-id="93ad5-163">Automatisk återställning av lösa multifaktorautentisering är inte ett alternativ i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="93ad5-163">Self-recovering by solving multi-factor authentication is not an option in this case.</span></span>

<span data-ttu-id="93ad5-164">![Reparation](./media/active-directory-identityprotection-flows/104.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="93ad5-164">![Remediation](./media/active-directory-identityprotection-flows/104.png "Remediation")</span></span>

## <a name="reset-password"></a><span data-ttu-id="93ad5-165">Återställa lösenord</span><span class="sxs-lookup"><span data-stu-id="93ad5-165">Reset password</span></span>
<span data-ttu-id="93ad5-166">Om avslöjade användare blockeras från att logga in kan en administratör generera ett tillfälligt lösenord för dessa.</span><span class="sxs-lookup"><span data-stu-id="93ad5-166">If compromised users are blocked from signing in, an administrator can generate a temporary password for them.</span></span> <span data-ttu-id="93ad5-167">hello användare har toochange sina lösenord under en nästa inloggning.</span><span class="sxs-lookup"><span data-stu-id="93ad5-167">hello users will have toochange their password during a next sign-in.</span></span>

<span data-ttu-id="93ad5-168">![Reparation](./media/active-directory-identityprotection-flows/160.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="93ad5-168">![Remediation](./media/active-directory-identityprotection-flows/160.png "Remediation")</span></span>

## <a name="see-also"></a><span data-ttu-id="93ad5-169">Se även</span><span class="sxs-lookup"><span data-stu-id="93ad5-169">See also</span></span>
* [<span data-ttu-id="93ad5-170">Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="93ad5-170">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md) 

