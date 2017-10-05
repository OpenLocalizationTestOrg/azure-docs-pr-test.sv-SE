---
title: "Logga in som inträffar med Azure AD Identity Protection | Microsoft Docs"
description: "En översikt över användarens upplevelse när identitetsskydd har minskas eller åtgärdad en användare eller när multifaktorautentisering krävs av en princip."
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
ms.openlocfilehash: e45936280b51fb2e54012a688fceddcc8dabe984
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a><span data-ttu-id="120d8-104">Logga in som inträffar med Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="120d8-104">Sign-in experiences with Azure AD Identity Protection</span></span>
<span data-ttu-id="120d8-105">Med Azure Active Directory identitetsskydd kan du:</span><span class="sxs-lookup"><span data-stu-id="120d8-105">With Azure Active Directory Identity Protection, you can:</span></span>

* <span data-ttu-id="120d8-106">användaren måste registrera sig för multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="120d8-106">require users to register for multi-factor authentication</span></span>
* <span data-ttu-id="120d8-107">Hantera riskfyllda inloggningar och avslöjade användare</span><span class="sxs-lookup"><span data-stu-id="120d8-107">handle risky sign-ins and compromised users</span></span>

<span data-ttu-id="120d8-108">Svaret för systemet på dessa problem påverkar på en användares inloggning eftersom bara direkt logga in med ett användarnamn och lösenord inte möjligt längre.</span><span class="sxs-lookup"><span data-stu-id="120d8-108">The response of the system to these issues has an impact on a user's sign-in experience because just directly signing-in by providing a user name and a password won't be possible anymore.</span></span> <span data-ttu-id="120d8-109">Ytterligare steg krävs för att hämta en användare på ett säkert sätt tillbaka till företag.</span><span class="sxs-lookup"><span data-stu-id="120d8-109">Additional steps are required to get a user safely back into business.</span></span>

<span data-ttu-id="120d8-110">Det här avsnittet ger en översikt över en användares inloggning i samtliga fall som kan uppstå.</span><span class="sxs-lookup"><span data-stu-id="120d8-110">This topic gives you an overview of a user's sign-in experience for all cases that can occur.</span></span>

<span data-ttu-id="120d8-111">**Multi-Factor Authentication**</span><span class="sxs-lookup"><span data-stu-id="120d8-111">**Multi-factor authentication**</span></span>

* <span data-ttu-id="120d8-112">Registreringen för multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="120d8-112">Multi-factor authentication registration</span></span>

<span data-ttu-id="120d8-113">**Logga in i fara**</span><span class="sxs-lookup"><span data-stu-id="120d8-113">**Sign-in at risk**</span></span>

* <span data-ttu-id="120d8-114">Återställning för riskfyllda inloggning</span><span class="sxs-lookup"><span data-stu-id="120d8-114">Risky sign-in recovery</span></span>
* <span data-ttu-id="120d8-115">Riskfyllda inloggning blockeras</span><span class="sxs-lookup"><span data-stu-id="120d8-115">Risky sign-in blocked</span></span>
* <span data-ttu-id="120d8-116">Multifaktorautentisering registrering under en riskfyllda inloggning</span><span class="sxs-lookup"><span data-stu-id="120d8-116">Multi-factor authentication registration during a risky sign-in</span></span>

<span data-ttu-id="120d8-117">**Användare i fara**</span><span class="sxs-lookup"><span data-stu-id="120d8-117">**User at risk**</span></span>

* <span data-ttu-id="120d8-118">Komprometterat kontoåterställning</span><span class="sxs-lookup"><span data-stu-id="120d8-118">Compromised account recovery</span></span>
* <span data-ttu-id="120d8-119">Komprometterat konto blockeras</span><span class="sxs-lookup"><span data-stu-id="120d8-119">Compromised account blocked</span></span>

## <a name="multi-factor-authentication-registration"></a><span data-ttu-id="120d8-120">Registreringen för multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="120d8-120">Multi-factor authentication registration</span></span>
<span data-ttu-id="120d8-121">Den bästa användarupplevelsen för både komprometterat konto recovery trafikflöde och riskfyllda inloggning-flödet är när användaren själv kan återställa.</span><span class="sxs-lookup"><span data-stu-id="120d8-121">The best user experience for both, the compromised account recovery flow and the risky sign-in flow, is when the user can self-recover.</span></span> <span data-ttu-id="120d8-122">Om användarna har registrerats för multifaktorautentisering, har de redan ett telefonnummer som är associerad med ett konto som kan användas för att överföra säkerhetsutmaningar.</span><span class="sxs-lookup"><span data-stu-id="120d8-122">If users are registered for multi-factor authentication, they already have a phone number associated with their account that can be used to pass security challenges.</span></span> <span data-ttu-id="120d8-123">Ingen hjälp supportavdelningen eller administratören inblandning behövs för att återställa från kontot har komprometterats.</span><span class="sxs-lookup"><span data-stu-id="120d8-123">No help desk or administrator involvement is needed to recover from account compromise.</span></span> <span data-ttu-id="120d8-124">Det har alltså bör få dina användare som har registrerats för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="120d8-124">Thus, it’s highly recommended to get your users registered for multi-factor authentication.</span></span> 

<span data-ttu-id="120d8-125">Administratörer kan:</span><span class="sxs-lookup"><span data-stu-id="120d8-125">Administrators can:</span></span>

* <span data-ttu-id="120d8-126">Ange en princip som kräver att användare skapar sina konton för ytterligare säkerhetsverifiering.</span><span class="sxs-lookup"><span data-stu-id="120d8-126">set a policy that requires users to set up their accounts for additional security verification.</span></span> 
* <span data-ttu-id="120d8-127">Tillåt hoppa över multifaktorautentisering registrering för upp till 30 dagar, om de vill ge användarna en respitperiod innan du registrerar.</span><span class="sxs-lookup"><span data-stu-id="120d8-127">allow skipping multi-factor authentication registration for up to 30 days, in case they want to give users a grace period before registering.</span></span>

<span data-ttu-id="120d8-128">**Multifaktorautentisering registreringen har tre steg:**</span><span class="sxs-lookup"><span data-stu-id="120d8-128">**The multi-factor authentication registration has three steps:**</span></span>

1. <span data-ttu-id="120d8-129">I det första steget är att användaren får ett meddelande om krav för att ange kontot för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="120d8-129">In the first step, the user gets a notification about the requirement to set the account up for multi-factor authentication.</span></span> 
   
    <span data-ttu-id="120d8-130">![Reparation](./media/active-directory-identityprotection-flows/140.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="120d8-130">![Remediation](./media/active-directory-identityprotection-flows/140.png "Remediation")</span></span>
2. <span data-ttu-id="120d8-131">Om du vill konfigurera multifaktorautentisering måste du låta systemet vet hur du vill bli kontaktad.</span><span class="sxs-lookup"><span data-stu-id="120d8-131">To set multi-factor authentication up, you need to let the system know how you want to be contacted.</span></span>
   
    <span data-ttu-id="120d8-132">![Reparation](./media/active-directory-identityprotection-flows/141.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="120d8-132">![Remediation](./media/active-directory-identityprotection-flows/141.png "Remediation")</span></span>
3. <span data-ttu-id="120d8-133">Systemet skickar en uppmaning att du och du måste svara.</span><span class="sxs-lookup"><span data-stu-id="120d8-133">The system submits a challenge to you and you need to respond.</span></span>
   
    <span data-ttu-id="120d8-134">![Reparation](./media/active-directory-identityprotection-flows/142.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="120d8-134">![Remediation](./media/active-directory-identityprotection-flows/142.png "Remediation")</span></span>

## <a name="risky-sign-in-recovery"></a><span data-ttu-id="120d8-135">Återställning för riskfyllda inloggning</span><span class="sxs-lookup"><span data-stu-id="120d8-135">Risky sign-in recovery</span></span>
<span data-ttu-id="120d8-136">När en administratör har konfigurerat en princip för inloggning risker, meddelas berörda användare när de försöker att logga in.</span><span class="sxs-lookup"><span data-stu-id="120d8-136">When an administrator has configured a policy for sign-in risks, the affected users are notified when they try to sign-in.</span></span> 

<span data-ttu-id="120d8-137">**Flödet för riskfyllda inloggning har två steg:**</span><span class="sxs-lookup"><span data-stu-id="120d8-137">**The risky sign-in flow has two steps:**</span></span> 

1. <span data-ttu-id="120d8-138">Användaren informeras om att något annorlunda identifierades om deras inloggning, till exempel loggar in från en ny plats, enhet eller app.</span><span class="sxs-lookup"><span data-stu-id="120d8-138">The user is informed that something unusual was detected about their sign-in, such as signing in from a new location, device, or app.</span></span> 
   
    <span data-ttu-id="120d8-139">![Reparation](./media/active-directory-identityprotection-flows/120.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="120d8-139">![Remediation](./media/active-directory-identityprotection-flows/120.png "Remediation")</span></span>
2. <span data-ttu-id="120d8-140">Användaren krävs för att bevisa sin identitet genom att lösa en säkerhetskontrollen.</span><span class="sxs-lookup"><span data-stu-id="120d8-140">The user is required to prove their identity by solving a security challenge.</span></span> <span data-ttu-id="120d8-141">Om användaren har registrerats för multifaktorautentisering behöver de följa med en säkerhetskod till deras telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="120d8-141">If the user is registered for multi-factor authentication they need to round-trip a security code to their phone number.</span></span> <span data-ttu-id="120d8-142">Eftersom det är bara en riskfyllda inloggning, inte en komprometterad konto inte användaren att ändra lösenordet i det här flödet.</span><span class="sxs-lookup"><span data-stu-id="120d8-142">Since this is a just a risky sign in and not a compromised account, the user won’t have to change the password in this flow.</span></span> 
   
    <span data-ttu-id="120d8-143">![Reparation](./media/active-directory-identityprotection-flows/121.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="120d8-143">![Remediation](./media/active-directory-identityprotection-flows/121.png "Remediation")</span></span>

## <a name="risky-sign-in-blocked"></a><span data-ttu-id="120d8-144">Riskfyllda inloggning blockeras</span><span class="sxs-lookup"><span data-stu-id="120d8-144">Risky sign-in blocked</span></span>
<span data-ttu-id="120d8-145">Administratörer kan också välja att ange en princip för inloggning risken att blockera användare vid inloggning beroende på vilken risknivå av.</span><span class="sxs-lookup"><span data-stu-id="120d8-145">Administrators can also choose to set a Sign-In Risk policy to block users upon sign-in depending on the risk level.</span></span> <span data-ttu-id="120d8-146">Slutanvändare måste kontakta en administratör eller supportavdelningen om du behöver blockerad, eller de kan försöka logga in från en bekant plats eller en enhet.</span><span class="sxs-lookup"><span data-stu-id="120d8-146">To get unblocked, end users must contact an administrator or help desk, or they can try signing in from a familiar location or device.</span></span> <span data-ttu-id="120d8-147">Automatisk återställning av lösa multifaktorautentisering är inte ett alternativ i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="120d8-147">Self-recovering by solving multi-factor authentication is not an option in this case.</span></span>

<span data-ttu-id="120d8-148">![Reparation](./media/active-directory-identityprotection-flows/200.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="120d8-148">![Remediation](./media/active-directory-identityprotection-flows/200.png "Remediation")</span></span>

## <a name="compromised-account-recovery"></a><span data-ttu-id="120d8-149">Komprometterat kontoåterställning</span><span class="sxs-lookup"><span data-stu-id="120d8-149">Compromised account recovery</span></span>
<span data-ttu-id="120d8-150">När en säkerhetsprincip för användaren risk har konfigurerats kan användare som uppfyller användaren risknivå anges i principen (och därför antas komprometteras) måste gå igenom användaren röjande recovery flödet innan de kan logga in.</span><span class="sxs-lookup"><span data-stu-id="120d8-150">When a user risk security policy has been configured, users who meet the user risk level specified in the policy (and are therefore assumed compromised) must go through the user compromise recovery flow before they can sign-in.</span></span> 

<span data-ttu-id="120d8-151">**Användaren har komprometterats recovery flödet har tre steg:**</span><span class="sxs-lookup"><span data-stu-id="120d8-151">**The user compromise recovery flow has three steps:**</span></span>

1. <span data-ttu-id="120d8-152">Användaren informeras om att deras kontosäkerhet är i fara på grund av misstänkt aktivitet eller läcka ut autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="120d8-152">The user is informed that their account security is at risk because of suspicious activity or leaked credentials.</span></span>
   
    <span data-ttu-id="120d8-153">![Reparation](./media/active-directory-identityprotection-flows/101.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="120d8-153">![Remediation](./media/active-directory-identityprotection-flows/101.png "Remediation")</span></span>
2. <span data-ttu-id="120d8-154">Användaren krävs för att bevisa sin identitet genom att lösa en säkerhetskontrollen.</span><span class="sxs-lookup"><span data-stu-id="120d8-154">The user is required to prove their identity by solving a security challenge.</span></span> <span data-ttu-id="120d8-155">Om användaren har registrerats för multifaktorautentisering återställa de själva från komprometteras.</span><span class="sxs-lookup"><span data-stu-id="120d8-155">If the user is registered for multi-factor authentication they can self-recover from being compromised.</span></span> <span data-ttu-id="120d8-156">De måste följa med en säkerhetskod till deras telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="120d8-156">They will need to round-trip a security code to their phone number.</span></span> 
   
   <span data-ttu-id="120d8-157">![Reparation](./media/active-directory-identityprotection-flows/110.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="120d8-157">![Remediation](./media/active-directory-identityprotection-flows/110.png "Remediation")</span></span>
3. <span data-ttu-id="120d8-158">Slutligen, om användaren tvingas att ändra sina lösenord eftersom någon annan kan ha haft tillgång till sitt konto.</span><span class="sxs-lookup"><span data-stu-id="120d8-158">Finally, the user is forced to change their password since someone else may have had access to their account.</span></span> 
   <span data-ttu-id="120d8-159">Skärmdumpar av det här upplevelsen är lägre än.</span><span class="sxs-lookup"><span data-stu-id="120d8-159">Screenshots of this experience are below.</span></span>
   
   <span data-ttu-id="120d8-160">![Reparation](./media/active-directory-identityprotection-flows/111.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="120d8-160">![Remediation](./media/active-directory-identityprotection-flows/111.png "Remediation")</span></span>

## <a name="compromised-account-blocked"></a><span data-ttu-id="120d8-161">Komprometterat konto blockeras</span><span class="sxs-lookup"><span data-stu-id="120d8-161">Compromised account blocked</span></span>
<span data-ttu-id="120d8-162">För att få en användare som har blockerats av en användare risk säkerhetsprincip avblockerad, måste användaren kontakta administratören eller supportavdelningen.</span><span class="sxs-lookup"><span data-stu-id="120d8-162">To get a user that was blocked by a user risk security policy unblocked, the user must contact an administrator or help desk.</span></span> <span data-ttu-id="120d8-163">Automatisk återställning av lösa multifaktorautentisering är inte ett alternativ i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="120d8-163">Self-recovering by solving multi-factor authentication is not an option in this case.</span></span>

<span data-ttu-id="120d8-164">![Reparation](./media/active-directory-identityprotection-flows/104.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="120d8-164">![Remediation](./media/active-directory-identityprotection-flows/104.png "Remediation")</span></span>

## <a name="reset-password"></a><span data-ttu-id="120d8-165">Återställa lösenord</span><span class="sxs-lookup"><span data-stu-id="120d8-165">Reset password</span></span>
<span data-ttu-id="120d8-166">Om avslöjade användare blockeras från att logga in kan en administratör generera ett tillfälligt lösenord för dessa.</span><span class="sxs-lookup"><span data-stu-id="120d8-166">If compromised users are blocked from signing in, an administrator can generate a temporary password for them.</span></span> <span data-ttu-id="120d8-167">Användarna måste ändra sina lösenord under en nästa inloggning.</span><span class="sxs-lookup"><span data-stu-id="120d8-167">The users will have to change their password during a next sign-in.</span></span>

<span data-ttu-id="120d8-168">![Reparation](./media/active-directory-identityprotection-flows/160.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="120d8-168">![Remediation](./media/active-directory-identityprotection-flows/160.png "Remediation")</span></span>

## <a name="see-also"></a><span data-ttu-id="120d8-169">Se även</span><span class="sxs-lookup"><span data-stu-id="120d8-169">See also</span></span>
* [<span data-ttu-id="120d8-170">Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="120d8-170">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md) 

