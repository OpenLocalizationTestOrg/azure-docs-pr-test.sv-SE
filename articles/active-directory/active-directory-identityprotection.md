---
title: Azure Active Directory identitetsskydd | Microsoft Docs
description: "Lär dig hur Azure AD Identity Protection gör att du kan begränsa möjligheten för en angripare som utnyttjar en komprometterad identitet eller en enhet och att skydda en identitet eller en enhet som har tidigare eller misstänks vara hotad."
services: active-directory
keywords: "Azure active directory identitetsskydd, cloud app discovery, hantera program, säkerhet, risk, risknivå, säkerhetsproblem och säkerhetsprincip"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 0c7a8d68c0df729441e3f7faa5cd06066db1261d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-identity-protection"></a><span data-ttu-id="75e23-104">Identitetsskydd för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="75e23-104">Azure Active Directory Identity Protection</span></span>

<span data-ttu-id="75e23-105">Azure Active Directory Identity Protection är en funktion i Azure AD Premium P2-version som gör det möjligt att:</span><span class="sxs-lookup"><span data-stu-id="75e23-105">Azure Active Directory Identity Protection is a feature of the Azure AD Premium P2 edition that enables you to:</span></span>

- <span data-ttu-id="75e23-106">Identifiera potentiella problem som påverkar din organisations identiteter</span><span class="sxs-lookup"><span data-stu-id="75e23-106">Detect potential vulnerabilities affecting your organization’s identities</span></span>

- <span data-ttu-id="75e23-107">Konfigurera automatiska svar på identifierade misstänkta åtgärder som är relaterade till din organisations identiteter</span><span class="sxs-lookup"><span data-stu-id="75e23-107">Configure automated responses to detected suspicious actions that are related to your organization’s identities</span></span>  

- <span data-ttu-id="75e23-108">Undersöka misstänkt incidenter och vidta lämpliga åtgärder för att lösa dem.</span><span class="sxs-lookup"><span data-stu-id="75e23-108">Investigate suspicious incidents and take appropriate action to resolve them</span></span>   


## <a name="getting-started"></a><span data-ttu-id="75e23-109">Komma igång</span><span class="sxs-lookup"><span data-stu-id="75e23-109">Getting started</span></span>

<span data-ttu-id="75e23-110">Microsoft skyddar molnbaserade identiteter för mer än en tio åren.</span><span class="sxs-lookup"><span data-stu-id="75e23-110">Microsoft secures cloud-based identities for more than a decade.</span></span> <span data-ttu-id="75e23-111">Med Azure Active Directory Identity Protection kan i din miljö, du använda samma skyddssystem som Microsoft använder för att skydda identiteter.</span><span class="sxs-lookup"><span data-stu-id="75e23-111">With Azure Active Directory Identity Protection, in your environment, you can use the same protection systems Microsoft uses to secure identities.</span></span>

<span data-ttu-id="75e23-112">Det stora flertalet av säkerhetsintrång sker när angripare får tillgång till en miljö genom att stjäla en användares identitet.</span><span class="sxs-lookup"><span data-stu-id="75e23-112">The vast majority of security breaches take place when attackers gain access to an environment by stealing a user’s identity.</span></span> <span data-ttu-id="75e23-113">Angripare har blivit allt mer effektivt utnyttja från tredje part överträdelser och använda avancerade nätfiskeattacker åren har.</span><span class="sxs-lookup"><span data-stu-id="75e23-113">Over the years, attackers have become increasingly effective in leveraging third party breaches and using sophisticated phishing attacks.</span></span> <span data-ttu-id="75e23-114">När en angripare får åtkomst till även låg Privilegierade konton, är det relativt enkelt att få åtkomst till viktiga företagets resurser via lateral förflyttning.</span><span class="sxs-lookup"><span data-stu-id="75e23-114">As soon as an attacker gains access to even low privileged user accounts, it is relatively easy for them to gain access to important company resources through lateral movement.</span></span>

<span data-ttu-id="75e23-115">Till följd av detta måste du:</span><span class="sxs-lookup"><span data-stu-id="75e23-115">As a consequence of this, you need to:</span></span>

- <span data-ttu-id="75e23-116">Skydda alla identiteter oavsett deras behörighetsnivå</span><span class="sxs-lookup"><span data-stu-id="75e23-116">Protect all identities regardless of their privilege level</span></span>

- <span data-ttu-id="75e23-117">Proaktivt förhindra avslöjade identiteter som missbrukat</span><span class="sxs-lookup"><span data-stu-id="75e23-117">Proactively prevent compromised identities from being abused</span></span>

<span data-ttu-id="75e23-118">Identifiering av avslöjade identiteter finns ingen enkel aktivitet.</span><span class="sxs-lookup"><span data-stu-id="75e23-118">Discovering compromised identities is no easy task.</span></span> <span data-ttu-id="75e23-119">Azure Active Directory använder anpassningsbar maskininlärningsalgoritmer och heuristik för att identifiera avvikelser och misstänkta incidenter som indikerar potentiellt komprometteras identiteter.</span><span class="sxs-lookup"><span data-stu-id="75e23-119">Azure Active Directory uses adaptive machine learning algorithms and heuristics to detect anomalies and suspicious incidents that indicate potentially compromised identities.</span></span> <span data-ttu-id="75e23-120">Med dessa data kan genererar identitetsskydd rapporter och aviseringar som hjälper dig att utvärdera de identifierade problem och vidta lämplig lösning eller reparationsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="75e23-120">Using this data, Identity Protection generates reports and alerts that enable you to evaluate the detected issues and take appropriate mitigation or remediation actions.</span></span>

<span data-ttu-id="75e23-121">Azure Active Directory Identity Protection är mer än en övervakning och rapportering av verktyget.</span><span class="sxs-lookup"><span data-stu-id="75e23-121">Azure Active Directory Identity Protection is more than a monitoring and reporting tool.</span></span> <span data-ttu-id="75e23-122">Du kan konfigurera risk-baserade principer som svarar på identifierade problem automatiskt när en angiven risknivå har uppnåtts för att skydda din organisations identiteter.</span><span class="sxs-lookup"><span data-stu-id="75e23-122">To protect your organization's identities, you can configure risk-based policies that automatically respond to detected issues when a specified risk level has been reached.</span></span> <span data-ttu-id="75e23-123">Dessa principer, utöver andra kontroller för villkorlig åtkomst som tillhandahålls av Azure Active Directory och EMS, kan automatiskt blockera eller initiera anpassningsbar reparationsåtgärder inklusive lösenordsåterställning och multifaktorautentisering tvingande.</span><span class="sxs-lookup"><span data-stu-id="75e23-123">These policies, in addition to other conditional access controls provided by Azure Active Directory and EMS, can either automatically block or initiate adaptive remediation actions including password resets and multi-factor authentication enforcement.</span></span>


#### <a name="identity-protection-capabilities"></a><span data-ttu-id="75e23-124">Identitetsfunktionerna</span><span class="sxs-lookup"><span data-stu-id="75e23-124">Identity Protection capabilities</span></span>

<span data-ttu-id="75e23-125">**Identifiera säkerhetsrisker och riskfyllda konton:**</span><span class="sxs-lookup"><span data-stu-id="75e23-125">**Detecting vulnerabilities and risky accounts:**</span></span>  

* <span data-ttu-id="75e23-126">Att tillhandahålla anpassade rekommendationer för att förbättra övergripande säkerhetstillståndet genom att markera säkerhetsrisker</span><span class="sxs-lookup"><span data-stu-id="75e23-126">Providing custom recommendations to improve overall security posture by highlighting vulnerabilities</span></span>
* <span data-ttu-id="75e23-127">Beräkning av risknivåer för inloggning</span><span class="sxs-lookup"><span data-stu-id="75e23-127">Calculating sign-in risk levels</span></span>
* <span data-ttu-id="75e23-128">Beräkning av risknivåer för användaren</span><span class="sxs-lookup"><span data-stu-id="75e23-128">Calculating user risk levels</span></span>


<span data-ttu-id="75e23-129">**Undersöka riskhändelser:**</span><span class="sxs-lookup"><span data-stu-id="75e23-129">**Investigating risk events:**</span></span>

* <span data-ttu-id="75e23-130">Skicka meddelanden för riskhändelser</span><span class="sxs-lookup"><span data-stu-id="75e23-130">Sending notifications for risk events</span></span>
* <span data-ttu-id="75e23-131">Undersöka riskhändelser med hjälp av relevanta och sammanhangsberoende information</span><span class="sxs-lookup"><span data-stu-id="75e23-131">Investigating risk events using relevant and contextual information</span></span>
* <span data-ttu-id="75e23-132">Tillhandahåller grundläggande arbetsflöden för att spåra undersökningar</span><span class="sxs-lookup"><span data-stu-id="75e23-132">Providing basic workflows to track investigations</span></span>
* <span data-ttu-id="75e23-133">Enkel åtkomst till åtgärder, till exempel återställning av lösenord</span><span class="sxs-lookup"><span data-stu-id="75e23-133">Providing easy access to remediation actions such as password reset</span></span>

<span data-ttu-id="75e23-134">**Principer för risk-baserad villkorlig åtkomst:**</span><span class="sxs-lookup"><span data-stu-id="75e23-134">**Risk-based conditional access policies:**</span></span>

* <span data-ttu-id="75e23-135">Princip för att minska riskfyllda inloggningar genom att blockera inloggningar eller att kräva multifaktorautentisering utmaningar.</span><span class="sxs-lookup"><span data-stu-id="75e23-135">Policy to mitigate risky sign-ins by blocking sign-ins or requiring multi-factor authentication challenges.</span></span>
* <span data-ttu-id="75e23-136">Princip för att blockera eller säker riskfyllda användarkonton</span><span class="sxs-lookup"><span data-stu-id="75e23-136">Policy to block or secure risky user accounts</span></span>
* <span data-ttu-id="75e23-137">Princip för att kräva att användarna registrera sig för multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="75e23-137">Policy to require users to register for multi-factor authentication</span></span>



## <a name="identity-protection-roles"></a><span data-ttu-id="75e23-138">Identity Protection roller</span><span class="sxs-lookup"><span data-stu-id="75e23-138">Identity Protection roles</span></span>

<span data-ttu-id="75e23-139">För att belastningsutjämna hanteringsaktiviteter runt implementeringen Identity Protection kan du tilldela flera roller.</span><span class="sxs-lookup"><span data-stu-id="75e23-139">To load balance the management activities around your Identity Protection implementation, you can assign several roles.</span></span> <span data-ttu-id="75e23-140">Azure AD Identity Protection stöder 3 directory roller:</span><span class="sxs-lookup"><span data-stu-id="75e23-140">Azure AD Identity Protection supports 3 directory roles:</span></span>

| <span data-ttu-id="75e23-141">Roll</span><span class="sxs-lookup"><span data-stu-id="75e23-141">Role</span></span>                         | <span data-ttu-id="75e23-142">Kan göra</span><span class="sxs-lookup"><span data-stu-id="75e23-142">Can do</span></span>                          | <span data-ttu-id="75e23-143">Det går inte att göra</span><span class="sxs-lookup"><span data-stu-id="75e23-143">Cannot do</span></span>
| :--                          | ---                                |  ---   |
| <span data-ttu-id="75e23-144">Global administratör</span><span class="sxs-lookup"><span data-stu-id="75e23-144">Global administrator</span></span>         | <span data-ttu-id="75e23-145">Fullständig åtkomst till identitetsskydd, publicera identitetsskydd</span><span class="sxs-lookup"><span data-stu-id="75e23-145">Full access to Identity Protection, Onboard Identity Protection</span></span>| |
| <span data-ttu-id="75e23-146">Säkerhetsadministratör</span><span class="sxs-lookup"><span data-stu-id="75e23-146">Security administrator</span></span>       | <span data-ttu-id="75e23-147">Fullständig åtkomst till Identity Protection</span><span class="sxs-lookup"><span data-stu-id="75e23-147">Full access to Identity Protection</span></span> | <span data-ttu-id="75e23-148">Publicera identitetsskydd, återställa lösenord för en användare</span><span class="sxs-lookup"><span data-stu-id="75e23-148">Onboard Identity Protection,  reset passwords for a user</span></span> |
| <span data-ttu-id="75e23-149">Säkerhetsläsare</span><span class="sxs-lookup"><span data-stu-id="75e23-149">Security reader</span></span>              | <span data-ttu-id="75e23-150">Endast klar åtkomst till Identity Protection</span><span class="sxs-lookup"><span data-stu-id="75e23-150">Ready-only access to Identity Protection</span></span> | <span data-ttu-id="75e23-151">Publicera identitetsskydd, remidiate användare konfigurera principer, återställa lösenord</span><span class="sxs-lookup"><span data-stu-id="75e23-151">Onboard Identity Protection, remidiate users, configure policies,  reset passwords</span></span> |




<span data-ttu-id="75e23-152">Mer information finns i [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="75e23-152">For more details, see [Assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)</span></span>



## <a name="detection"></a><span data-ttu-id="75e23-153">Detection (Identifiering)</span><span class="sxs-lookup"><span data-stu-id="75e23-153">Detection</span></span>

### <a name="vulnerabilities"></a><span data-ttu-id="75e23-154">Säkerhetsrisker</span><span class="sxs-lookup"><span data-stu-id="75e23-154">Vulnerabilities</span></span>

<span data-ttu-id="75e23-155">Azure Active Directory-identitetsskydd analyser konfigurationen och identifierar säkerhetsrisker som kan påverka din användaridentitet.</span><span class="sxs-lookup"><span data-stu-id="75e23-155">Azure Active Directory Identity Protection analyses your configuration and detects vulnerabilities that can have an impact on your user's identities.</span></span> <span data-ttu-id="75e23-156">Mer information finns i [sårbarheter som identifieras av Azure Active Directory Identity Protection](active-directory-identityprotection-vulnerabilities.md).</span><span class="sxs-lookup"><span data-stu-id="75e23-156">For more details, see [Vulnerabilities detected by Azure Active Directory Identity Protection](active-directory-identityprotection-vulnerabilities.md).</span></span>

### <a name="risk-events"></a><span data-ttu-id="75e23-157">Riskhändelser</span><span class="sxs-lookup"><span data-stu-id="75e23-157">Risk events</span></span>

<span data-ttu-id="75e23-158">Azure Active Directory använder anpassningsbar maskininlärningsalgoritmer och heuristik för att identifiera misstänkta åtgärder som är relaterade till din användaridentitet.</span><span class="sxs-lookup"><span data-stu-id="75e23-158">Azure Active Directory uses adaptive machine learning algorithms and heuristics to detect suspicious actions that are related to your user's identities.</span></span> <span data-ttu-id="75e23-159">Systemet skapar en post för varje upptäckt misstänkt åtgärd.</span><span class="sxs-lookup"><span data-stu-id="75e23-159">The system creates a record for each detected suspicious action.</span></span> <span data-ttu-id="75e23-160">Dessa poster kallas även riskhändelser.</span><span class="sxs-lookup"><span data-stu-id="75e23-160">These records are also known as risk events.</span></span>  
<span data-ttu-id="75e23-161">Mer information finns i avsnittet om [Azure Active Directory-riskhändelser](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="75e23-161">For more details, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span>


## <a name="investigation"></a><span data-ttu-id="75e23-162">Undersökning</span><span class="sxs-lookup"><span data-stu-id="75e23-162">Investigation</span></span>
<span data-ttu-id="75e23-163">Din resa via Identity Protection startas normalt med identitetsskydd instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="75e23-163">Your journey through Identity Protection typically starts with the Identity Protection dashboard.</span></span>

<span data-ttu-id="75e23-164">![Reparation](./media/active-directory-identityprotection/1000.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="75e23-164">![Remediation](./media/active-directory-identityprotection/1000.png "Remediation")</span></span>

<span data-ttu-id="75e23-165">Instrumentpanelen ger dig tillgång till:</span><span class="sxs-lookup"><span data-stu-id="75e23-165">The dashboard gives you access to:</span></span>

* <span data-ttu-id="75e23-166">Rapporter som **användare som har flaggats för risk**, **riskerar händelser** och **säkerhetsrisker**</span><span class="sxs-lookup"><span data-stu-id="75e23-166">Reports such as **Users flagged for risk**, **Risk events** and **Vulnerabilities**</span></span>
* <span data-ttu-id="75e23-167">Inställningar, till exempel konfigurationen av din **säkerhetsprinciper**, **meddelanden** och **multifaktorautentisering registrering**</span><span class="sxs-lookup"><span data-stu-id="75e23-167">Settings such as the configuration of your **Security Policies**, **Notifications** and **multi-factor authentication registration**</span></span>

<span data-ttu-id="75e23-168">Det är vanligtvis utgångspunkten för undersökningar, som du kan granska aktiviteter, loggar och annan relevant information som rör en händelse för risk att bestämma om reparation eller minimering steg är nödvändiga, och hur identiteten var komprometteras och förstå hur avslöjade identiteten användes.</span><span class="sxs-lookup"><span data-stu-id="75e23-168">It is typically your starting point for investigation, which is the process of reviewing the activities, logs, and other relevant information related to a risk event to decide whether remediation or mitigation steps are necessary,  and how the identity was compromised, and understand how the compromised identity was used.</span></span>

<span data-ttu-id="75e23-169">Du kan koppla dina undersökningen aktiviteter till den [meddelanden](active-directory-identityprotection-notifications.md) Azure Active Directory Protection skickar per e-post.</span><span class="sxs-lookup"><span data-stu-id="75e23-169">You can tie your investigation activities to the [notifications](active-directory-identityprotection-notifications.md) Azure Active Directory Protection sends per email.</span></span>

<span data-ttu-id="75e23-170">Följande avsnitt kan du med mer information och de steg som är relaterade till en undersökning.</span><span class="sxs-lookup"><span data-stu-id="75e23-170">The following sections provide you with more details and the steps that are related to an investigation.</span></span>  


## <a name="risky-sign-ins"></a><span data-ttu-id="75e23-171">Riskfyllda inloggningar</span><span class="sxs-lookup"><span data-stu-id="75e23-171">Risky sign-ins</span></span>

<span data-ttu-id="75e23-172">Azure Active Directory identifierar [riskerar händelsetyper](active-directory-reporting-risk-events.md#risk-event-types) i realtid och offline.</span><span class="sxs-lookup"><span data-stu-id="75e23-172">Azure Active Directory detects [risk event types](active-directory-reporting-risk-events.md#risk-event-types) in real-time and offline.</span></span> <span data-ttu-id="75e23-173">Varje händelse för risker som har identifierats för en inloggning för en användare bidrar till ett logiskt koncept som kallas riskfyllda inloggning.</span><span class="sxs-lookup"><span data-stu-id="75e23-173">Each risk event that has been detected for a sign-in of a user contributes to a logical concept called risky sign-in.</span></span> <span data-ttu-id="75e23-174">Är en indikator för en inloggning försök som inte kanske har utförts av legitima ägaren till ett användarkonto riskfyllda loggar in.</span><span class="sxs-lookup"><span data-stu-id="75e23-174">A risky sign-in is an indicator for a sign-in attempt that might not have been performed by the legitimate owner of a user account.</span></span>


### <a name="sign-in-risk-level"></a><span data-ttu-id="75e23-175">Risknivå för inloggning</span><span class="sxs-lookup"><span data-stu-id="75e23-175">Sign-in risk level</span></span>

<span data-ttu-id="75e23-176">Logga in risknivå är en indikation (hög, medel eller låg) på sannolikheten att ett försök att logga in inte utfördes av legitima ägaren till ett användarkonto.</span><span class="sxs-lookup"><span data-stu-id="75e23-176">A sign-in risk level is an indication (High, Medium, or Low) of the likelihood that a sign-in attempt was not performed by the legitimate owner of a user account.</span></span>

### <a name="mitigating-sign-in-risk-events"></a><span data-ttu-id="75e23-177">Minimera riskhändelser för inloggning</span><span class="sxs-lookup"><span data-stu-id="75e23-177">Mitigating sign-in risk events</span></span>

<span data-ttu-id="75e23-178">En lösning är en åtgärd för att begränsa möjligheten för en angripare som utnyttjar en komprometterad identitet eller en enhet utan att återställa identitet eller enhet till ett säkert tillstånd.</span><span class="sxs-lookup"><span data-stu-id="75e23-178">A mitigation is an action to limit the ability of an attacker to exploit a compromised identity or device without restoring the identity or device to a safe state.</span></span> <span data-ttu-id="75e23-179">En lösning kan inte matchas tidigare inloggning riskhändelser som associeras med identiteten eller enhet.</span><span class="sxs-lookup"><span data-stu-id="75e23-179">A mitigation does not resolve previous sign-in risk events associated with the identity or device.</span></span>

<span data-ttu-id="75e23-180">Du kan konfigurera inloggning risk säkerhet policicies för att minimera riskfyllda inloggningar automatiskt.</span><span class="sxs-lookup"><span data-stu-id="75e23-180">To mitigate risky sign-ins automatically, you can configure sign-in risk security policicies.</span></span> <span data-ttu-id="75e23-181">Med dessa principer kan du risknivån för användaren eller logga in att blockera riskfyllda inloggningar eller kräver att användaren utför Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="75e23-181">Using these policies, you consider the risk level of the user or the sign-in to block risky sign-ins or require the user to perform multi-factor authentication.</span></span> <span data-ttu-id="75e23-182">Dessa åtgärder kan hindra en angripare från att en stulen identitet för att orsaka skada och ge lite tid att skydda identitet.</span><span class="sxs-lookup"><span data-stu-id="75e23-182">These actions may prevent an attacker from exploiting a stolen identity to cause damage, and may give you some time to secure the identity.</span></span>

### <a name="sign-in-risk-security-policy"></a><span data-ttu-id="75e23-183">Logga in risk säkerhetsprincip</span><span class="sxs-lookup"><span data-stu-id="75e23-183">Sign-in risk security policy</span></span>
<span data-ttu-id="75e23-184">Logga in riskprincipen är en princip för villkorlig åtkomst som utvärderar risken för en specifik inloggning och tillämpar åtgärder baserat på fördefinierade villkor och regler.</span><span class="sxs-lookup"><span data-stu-id="75e23-184">A sign-in risk policy is a conditional access policy that evaluates the risk to a specific sign-in and applies mitigations based on predefined conditions and rules.</span></span>

<span data-ttu-id="75e23-185">![Logga in riskprincipen](./media/active-directory-identityprotection/1014.png "inloggning riskprincipen")</span><span class="sxs-lookup"><span data-stu-id="75e23-185">![Sign-in risk policy](./media/active-directory-identityprotection/1014.png "Sign-in risk policy")</span></span>

<span data-ttu-id="75e23-186">Azure AD Identity Protection hjälper dig att hantera minskning av riskfyllda inloggningar genom att du kan:</span><span class="sxs-lookup"><span data-stu-id="75e23-186">Azure AD Identity Protection helps you manage the mitigation of risky sign-ins by enabling you to:</span></span>

* <span data-ttu-id="75e23-187">Ange de användare och grupper som principen gäller för:</span><span class="sxs-lookup"><span data-stu-id="75e23-187">Set the users and groups the policy applies to:</span></span>

    <span data-ttu-id="75e23-188">![Logga in riskprincipen](./media/active-directory-identityprotection/1015.png "inloggning riskprincipen")</span><span class="sxs-lookup"><span data-stu-id="75e23-188">![Sign-in risk policy](./media/active-directory-identityprotection/1015.png "Sign-in risk policy")</span></span>
* <span data-ttu-id="75e23-189">Ange inloggning risk nivån tröskelvärdet (låg, medel eller hög) som utlöser principen:</span><span class="sxs-lookup"><span data-stu-id="75e23-189">Set the sign-in risk level threshold (low, medium, or high) that triggers the policy:</span></span>

    <span data-ttu-id="75e23-190">![Logga in riskprincipen](./media/active-directory-identityprotection/1016.png "inloggning riskprincipen")</span><span class="sxs-lookup"><span data-stu-id="75e23-190">![Sign-in risk policy](./media/active-directory-identityprotection/1016.png "Sign-in risk policy")</span></span>
* <span data-ttu-id="75e23-191">Ange kontrollerna som tillämpas när principen utlöser:</span><span class="sxs-lookup"><span data-stu-id="75e23-191">Set the controls to be enforced when the policy triggers:</span></span>  

    <span data-ttu-id="75e23-192">![Logga in riskprincipen](./media/active-directory-identityprotection/1017.png "inloggning riskprincipen")</span><span class="sxs-lookup"><span data-stu-id="75e23-192">![Sign-in risk policy](./media/active-directory-identityprotection/1017.png "Sign-in risk policy")</span></span>
* <span data-ttu-id="75e23-193">Växla tillståndet för principen:</span><span class="sxs-lookup"><span data-stu-id="75e23-193">Switch the state of your policy:</span></span>

    <span data-ttu-id="75e23-194">![MFA-registrering](./media/active-directory-identityprotection/403.png "MFA-registrering")</span><span class="sxs-lookup"><span data-stu-id="75e23-194">![MFA Registration](./media/active-directory-identityprotection/403.png "MFA Registration")</span></span>
* <span data-ttu-id="75e23-195">Granska och utvärdera effekten av en ändring innan du aktiverar det:</span><span class="sxs-lookup"><span data-stu-id="75e23-195">Review and evaluate the impact of a change before activating it:</span></span>

    <span data-ttu-id="75e23-196">![Logga in riskprincipen](./media/active-directory-identityprotection/1018.png "inloggning riskprincipen")</span><span class="sxs-lookup"><span data-stu-id="75e23-196">![Sign-in risk policy](./media/active-directory-identityprotection/1018.png "Sign-in risk policy")</span></span>

#### <a name="what-you-need-to-know"></a><span data-ttu-id="75e23-197">Vad du behöver veta</span><span class="sxs-lookup"><span data-stu-id="75e23-197">What you need to know</span></span>
<span data-ttu-id="75e23-198">Du kan konfigurera en säkerhetsprincip för inloggning risk för att kräva multifaktorautentisering:</span><span class="sxs-lookup"><span data-stu-id="75e23-198">You can configure a sign-in risk security policy to require multi-factor authentication:</span></span>

<span data-ttu-id="75e23-199">![Logga in riskprincipen](./media/active-directory-identityprotection/1017.png "inloggning riskprincipen")</span><span class="sxs-lookup"><span data-stu-id="75e23-199">![Sign-in risk policy](./media/active-directory-identityprotection/1017.png "Sign-in risk policy")</span></span>

<span data-ttu-id="75e23-200">Men av säkerhetsskäl har fungerar den här inställningen bara för användare som redan har registrerats för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="75e23-200">However, for security reasons, this setting only works for users that have already been registered for multi-factor authentication.</span></span> <span data-ttu-id="75e23-201">Om du uppfyller villkoret för att kräva multifaktorautentisering för en användare som ännu inte har registrerats för multifaktorautentisering, är användaren blockerad.</span><span class="sxs-lookup"><span data-stu-id="75e23-201">If the condition to require multi-factor authentication is satisfied for a user who is not yet registered for multi-factor authentication, the user is blocked.</span></span>

<span data-ttu-id="75e23-202">Som bästa praxis, om du vill kräva Multi-Factor authentication för riskfyllda inloggningar, bör du:</span><span class="sxs-lookup"><span data-stu-id="75e23-202">As a best practice, if you want to require multi-factor authentication for risky sign-ins, you should:</span></span>

1. <span data-ttu-id="75e23-203">Aktivera den [multifaktorautentisering registrering princip](#multi-factor-authentication-registration-policy) för användare som påverkas.</span><span class="sxs-lookup"><span data-stu-id="75e23-203">Enable the [multi-factor authentication registration policy](#multi-factor-authentication-registration-policy) for the affected users.</span></span>
2. <span data-ttu-id="75e23-204">Kräv att berörda användare att logga in i en icke-riskfyllda session att utföra en MFA-registrering</span><span class="sxs-lookup"><span data-stu-id="75e23-204">Require the affected users to login in a non-risky session to perform a MFA registration</span></span>

<span data-ttu-id="75e23-205">Gör så här ser du till att multifaktorautentisering krävs för riskfyllda loggar in.</span><span class="sxs-lookup"><span data-stu-id="75e23-205">Completing these steps ensures that multi-factor authentication is required for a risky sign-in.</span></span>

#### <a name="best-practices"></a><span data-ttu-id="75e23-206">Bästa praxis</span><span class="sxs-lookup"><span data-stu-id="75e23-206">Best practices</span></span>
<span data-ttu-id="75e23-207">Om du väljer en **hög** tröskelvärdet minskar antalet gånger som en princip utlöses och minimera de negativa effekterna för användarna.</span><span class="sxs-lookup"><span data-stu-id="75e23-207">Choosing a **High** threshold reduces the number of times a policy is triggered and minimizes the impact to users.</span></span>  

<span data-ttu-id="75e23-208">Men det omfattar inte **låg** och **medel** inloggningar som flaggats för risk från principen som inte kan medföra att en angripare från att en komprometterad identitet.</span><span class="sxs-lookup"><span data-stu-id="75e23-208">However, it excludes **Low** and **Medium** sign-ins flagged for risk from the policy, which may not block an attacker from exploiting a compromised identity.</span></span>

<span data-ttu-id="75e23-209">När du ställer in principen.</span><span class="sxs-lookup"><span data-stu-id="75e23-209">When setting the policy,</span></span>

* <span data-ttu-id="75e23-210">Undanta användare som inte / kan inte ha multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="75e23-210">Exclude users who do not/cannot have multi-factor authentication</span></span>
* <span data-ttu-id="75e23-211">Undanta användare i språk om att aktivera principen inte är en praktisk (till exempel ingen åtkomst till supportavdelningen)</span><span class="sxs-lookup"><span data-stu-id="75e23-211">Exclude users in locales where enabling the policy is not practical (for example no access to helpdesk)</span></span>
* <span data-ttu-id="75e23-212">Undanta användare som kommer att generera en mängd FALSKT positiva (utvecklare, säkerhetsanalytiker)</span><span class="sxs-lookup"><span data-stu-id="75e23-212">Exclude users who are likely to generate a lot of false-positives (developers, security analysts)</span></span>
* <span data-ttu-id="75e23-213">Använd en **hög** tröskelvärde under inledande skala, eller om du måste minska utmaningar som ses av användare.</span><span class="sxs-lookup"><span data-stu-id="75e23-213">Use a **High** threshold during initial policy roll out, or if you must minimize challenges seen by end users.</span></span>
* <span data-ttu-id="75e23-214">Använd en **låg** tröskelvärde om din organisation kräver högre säkerhet.</span><span class="sxs-lookup"><span data-stu-id="75e23-214">Use a **Low**  threshold if your organization requires greater security.</span></span> <span data-ttu-id="75e23-215">Att välja en **låg** tröskelvärdet introducerar ytterligare användare logga in utmaningar, men ökad säkerhet.</span><span class="sxs-lookup"><span data-stu-id="75e23-215">Selecting a **Low** threshold introduces additional user sign-in challenges, but increased security.</span></span>

<span data-ttu-id="75e23-216">Rekommenderad standard för de flesta organisationer är att konfigurera en regel för en **medel** tröskelvärdet för att göra en avvägning mellan användbarhet och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="75e23-216">The recommended default for most organizations is to configure a rule for a **Medium** threshold to strike a balance between usability and security.</span></span>

<span data-ttu-id="75e23-217">Logga in riskprincipen är:</span><span class="sxs-lookup"><span data-stu-id="75e23-217">The sign-in risk policy is:</span></span>

* <span data-ttu-id="75e23-218">Tillämpas på alla webbläsartrafik och inloggningar som använder modern autentisering.</span><span class="sxs-lookup"><span data-stu-id="75e23-218">Applied to all browser traffic and sign-ins using modern authentication.</span></span>
* <span data-ttu-id="75e23-219">Tillämpas inte på program som använder äldre säkerhetsprotokoll genom att inaktivera WS-Trust-slutpunkt på den federerade IDP som AD FS.</span><span class="sxs-lookup"><span data-stu-id="75e23-219">Not applied to applications using older security protocols by disabling the WS-Trust endpoint at the federated IDP, such as ADFS.</span></span>

<span data-ttu-id="75e23-220">Den **riskhändelser** i Identity Protection-konsolen visar alla händelser:</span><span class="sxs-lookup"><span data-stu-id="75e23-220">The **Risk Events** page in the Identity Protection console lists all events:</span></span>

* <span data-ttu-id="75e23-221">Den här principen har tillämpats på</span><span class="sxs-lookup"><span data-stu-id="75e23-221">This policy was applied to</span></span>
* <span data-ttu-id="75e23-222">Du kan granska aktivitetens och avgöra om åtgärden har lämplig</span><span class="sxs-lookup"><span data-stu-id="75e23-222">You can review the activity and determine whether the action was appropriate or not</span></span>

<span data-ttu-id="75e23-223">En översikt över den relaterade användarupplevelsen finns:</span><span class="sxs-lookup"><span data-stu-id="75e23-223">For an overview of the related user experience, see:</span></span>

* [<span data-ttu-id="75e23-224">Återställning för riskfyllda inloggning</span><span class="sxs-lookup"><span data-stu-id="75e23-224">Risky sign-in recovery</span></span>](active-directory-identityprotection-flows.md#risky-sign-in-recovery)
* [<span data-ttu-id="75e23-225">Riskfyllda inloggning blockeras</span><span class="sxs-lookup"><span data-stu-id="75e23-225">Risky sign-in blocked</span></span>](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  
* [<span data-ttu-id="75e23-226">Logga in som inträffar med Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="75e23-226">Sign-in experiences with Azure AD Identity Protection</span></span>](active-directory-identityprotection-flows.md)  

<span data-ttu-id="75e23-227">**Att öppna dialogrutan relaterade configuration**:</span><span class="sxs-lookup"><span data-stu-id="75e23-227">**To open the related configuration dialog**:</span></span>

- <span data-ttu-id="75e23-228">På den **Azure AD Identity Protection** blad i den **konfigurera** klickar du på **inloggning riskprincipen**.</span><span class="sxs-lookup"><span data-stu-id="75e23-228">On the **Azure AD Identity Protection** blade, in the **Configure** section, click **Sign-in risk policy**.</span></span>

    <span data-ttu-id="75e23-229">![Användarprincip ridk](./media/active-directory-identityprotection/1014.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="75e23-229">![User ridk policy](./media/active-directory-identityprotection/1014.png "User ridk policy")</span></span>



## <a name="users-flagged-for-risk"></a><span data-ttu-id="75e23-230">Användare som har flaggats för risk</span><span class="sxs-lookup"><span data-stu-id="75e23-230">Users flagged for risk</span></span>

<span data-ttu-id="75e23-231">Alla aktiva [riskerar händelser](active-directory-identity-protection-risk-events.md) som identifierades av Azure Active Directory för en användare som bidrar till ett logiskt koncept som kallas användaren risk.</span><span class="sxs-lookup"><span data-stu-id="75e23-231">All active [risk events](active-directory-identity-protection-risk-events.md) that were detected by Azure Active Directory for a user contribute to a logical concept called user risk.</span></span> <span data-ttu-id="75e23-232">En användare som har flaggats för risk är en indikator för ett konto som kan ha drabbats.</span><span class="sxs-lookup"><span data-stu-id="75e23-232">A user flagged for risk is an indicator for a user account that might have been compromised.</span></span>

![Användare som har flaggats för risk](./media/active-directory-identityprotection/1200.png)


### <a name="user-risk-level"></a><span data-ttu-id="75e23-234">Risknivå för användaren</span><span class="sxs-lookup"><span data-stu-id="75e23-234">User risk level</span></span>

<span data-ttu-id="75e23-235">Risknivå för en användare är en indikation (hög, medel eller låg) på sannolikheten att användarens identitet har komprometterats.</span><span class="sxs-lookup"><span data-stu-id="75e23-235">A user risk level is an indication (High, Medium, or Low) of the likelihood that the user’s identity has been compromised.</span></span> <span data-ttu-id="75e23-236">Den beräknas baserat på riskhändelser för användare som är kopplade till en användarens identitet.</span><span class="sxs-lookup"><span data-stu-id="75e23-236">It is calculated based on the user risk events that are associated with a user's identity.</span></span>

<span data-ttu-id="75e23-237">Status för en risk händelse är antingen **Active** eller **stängd**.</span><span class="sxs-lookup"><span data-stu-id="75e23-237">The status of a risk event is either **Active** or **Closed**.</span></span> <span data-ttu-id="75e23-238">Endast riskerar händelser som är **Active** bidra till användaren risk nivån beräkningen.</span><span class="sxs-lookup"><span data-stu-id="75e23-238">Only risk events that are **Active** contribute to the user risk level calculation.</span></span>

<span data-ttu-id="75e23-239">Risknivå användaren beräknas med hjälp av följande indata:</span><span class="sxs-lookup"><span data-stu-id="75e23-239">The user risk level is calculated using the following inputs:</span></span>

* <span data-ttu-id="75e23-240">Aktiva riskhändelser påverka användaren</span><span class="sxs-lookup"><span data-stu-id="75e23-240">Active risk events impacting the user</span></span>
* <span data-ttu-id="75e23-241">Risknivå för dessa händelser</span><span class="sxs-lookup"><span data-stu-id="75e23-241">Risk level of these events</span></span>
* <span data-ttu-id="75e23-242">Om eventuella åtgärder har utförts</span><span class="sxs-lookup"><span data-stu-id="75e23-242">Whether any remediation actions have been taken</span></span>

<span data-ttu-id="75e23-243">![Användaren risker](./media/active-directory-identityprotection/1031.png "användaren risker")</span><span class="sxs-lookup"><span data-stu-id="75e23-243">![User risks](./media/active-directory-identityprotection/1031.png "User risks")</span></span>

<span data-ttu-id="75e23-244">Du kan använda risknivåer för användare för att skapa principer för villkorlig åtkomst som blockerar riskfyllda användare från att logga in eller tvinga dem att ändra sina lösenord på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="75e23-244">You can use the user risk levels to create conditional access policies that block risky users from signing in, or force them to securely change their password.</span></span>

### <a name="closing-risk-events-manually"></a><span data-ttu-id="75e23-245">Stänger riskhändelser manuellt</span><span class="sxs-lookup"><span data-stu-id="75e23-245">Closing risk events manually</span></span>

<span data-ttu-id="75e23-246">I de flesta fall ska vi ta åtgärder, till exempel ett säkert lösenord för att automatiskt stänga riskhändelser.</span><span class="sxs-lookup"><span data-stu-id="75e23-246">In most cases, you will take remediation actions such as a secure password reset to automatically close risk events.</span></span> <span data-ttu-id="75e23-247">Men kanske det inte alltid är möjligt.</span><span class="sxs-lookup"><span data-stu-id="75e23-247">However, this might not always be possible.</span></span>  
<span data-ttu-id="75e23-248">Detta gäller, till exempel när:</span><span class="sxs-lookup"><span data-stu-id="75e23-248">This is, for example, the case, when:</span></span>

* <span data-ttu-id="75e23-249">En användare med aktiva riskhändelser har tagits bort</span><span class="sxs-lookup"><span data-stu-id="75e23-249">A user with Active risk events has been deleted</span></span>
* <span data-ttu-id="75e23-250">En undersökningen visar att en händelse rapporteras risk utför av behöriga användare</span><span class="sxs-lookup"><span data-stu-id="75e23-250">An investigation reveals that a reported risk event has been perform by the legitimate user</span></span>

<span data-ttu-id="75e23-251">Eftersom riskhändelser som är **Active** bidra till den användaren vid beräkningen måste du kanske att sänka en risknivå manuellt genom att stänga riskhändelser manuellt.</span><span class="sxs-lookup"><span data-stu-id="75e23-251">Because risk events that are **Active** contribute to the user risk calculation, you may have to manually lower a risk level by closing risk events manually.</span></span>  
<span data-ttu-id="75e23-252">Under undersökningen, kan du vidta någon av dessa åtgärder för att ändra status för en risk händelse:</span><span class="sxs-lookup"><span data-stu-id="75e23-252">During the course of investigation, you can choose to take any of these actions to change the status of a risk event:</span></span>

<span data-ttu-id="75e23-253">![Åtgärder](./media/active-directory-identityprotection/34.png "åtgärder")</span><span class="sxs-lookup"><span data-stu-id="75e23-253">![Actions](./media/active-directory-identityprotection/34.png "Actions")</span></span>

* <span data-ttu-id="75e23-254">**Lös** - om när du undersöker en risk händelse du använde en lämplig Reparationsåtgärd utanför identitetsskydd och du tror att risk händelsen ska betraktas som stängd Markera händelsen som löst.</span><span class="sxs-lookup"><span data-stu-id="75e23-254">**Resolve** - If after investigating a risk event, you took an appropriate remediation action outside Identity Protection, and you believe that the risk event should be considered closed, mark the event as Resolved.</span></span> <span data-ttu-id="75e23-255">Löst händelser ska ange händelsen risk status är stängd och händelsen risk kommer inte längre att bidra till användaren risk.</span><span class="sxs-lookup"><span data-stu-id="75e23-255">Resolved events will set the risk event’s status to Closed and the risk event will no longer contribute to user risk.</span></span>
* <span data-ttu-id="75e23-256">**Markera som falska positiva** – i vissa fall kan du kan undersöka en händelse för risk och identifiera att det var felaktigt flaggas som en riskfyllda.</span><span class="sxs-lookup"><span data-stu-id="75e23-256">**Mark as false-positive** - In some cases, you may investigate a risk event and discover that it was incorrectly flagged as a risky.</span></span> <span data-ttu-id="75e23-257">Du kan minska antalet sådana händelser genom att markera händelsen risk som falska positiva.</span><span class="sxs-lookup"><span data-stu-id="75e23-257">You can help reduce the number of such occurrences by marking the risk event as False-positive.</span></span> <span data-ttu-id="75e23-258">Detta hjälper de maskininlärningsalgoritmer för att förbättra klassificeringen av liknande händelser i framtiden.</span><span class="sxs-lookup"><span data-stu-id="75e23-258">This will help the machine learning algorithms to improve the classification of similar events in the future.</span></span> <span data-ttu-id="75e23-259">Status för falska positiva händelser är att **stängd** och kommer inte längre att bidra till användaren risk.</span><span class="sxs-lookup"><span data-stu-id="75e23-259">The status of false-positive events is to **Closed** and they will no longer contribute to user risk.</span></span>
* <span data-ttu-id="75e23-260">**Ignorera** - om du inte har vidtagit några Reparationsåtgärd, men vill händelsen risken att tas bort från listan över aktiva, kan du markera en händelse för risk Ignorera och händelsestatus kommer att stängas.</span><span class="sxs-lookup"><span data-stu-id="75e23-260">**Ignore** - If you have not taken any remediation action, but want the risk event to be removed from the active list, you can mark a risk event Ignore and the event status will be Closed.</span></span> <span data-ttu-id="75e23-261">Ignorerade händelser bidrar inte till användaren risk.</span><span class="sxs-lookup"><span data-stu-id="75e23-261">Ignored events do not contribute to user risk.</span></span> <span data-ttu-id="75e23-262">Det här alternativet bör endast användas under ovanliga omständigheter.</span><span class="sxs-lookup"><span data-stu-id="75e23-262">This option should only be used under unusual circumstances.</span></span>
* <span data-ttu-id="75e23-263">**Återaktivera** -riskerar händelser som har stängts manuellt (genom att välja **lösa**, **falsklarm**, eller **Ignorera**) kan återaktiveras, ställa in händelsen tillbaka till **Active**.</span><span class="sxs-lookup"><span data-stu-id="75e23-263">**Reactivate** - Risk events that were manually closed (by choosing **Resolve**, **False positive**, or **Ignore**) can be reactivated, setting the event status back to **Active**.</span></span> <span data-ttu-id="75e23-264">Återaktiverade riskhändelser bidra till användaren risk nivån beräkningen.</span><span class="sxs-lookup"><span data-stu-id="75e23-264">Reactivated risk events contribute to the user risk level calculation.</span></span> <span data-ttu-id="75e23-265">Riskhändelser stängd genom reparation (t.ex. en säker lösenordsåterställning) kan inte aktiveras igen.</span><span class="sxs-lookup"><span data-stu-id="75e23-265">Risk events closed through remediation (such as a secure password reset) cannot be reactivated.</span></span>

<span data-ttu-id="75e23-266">**Att öppna dialogrutan relaterade configuration**:</span><span class="sxs-lookup"><span data-stu-id="75e23-266">**To open the related configuration dialog**:</span></span>

1. <span data-ttu-id="75e23-267">På den **Azure AD Identity Protection** bladet under **Undersök**, klickar du på **riskerar händelser**.</span><span class="sxs-lookup"><span data-stu-id="75e23-267">On the **Azure AD Identity Protection** blade, under **Investigate**, click **Risk events**.</span></span>

    <span data-ttu-id="75e23-268">![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1002.png "manuell lösenordsåterställning")</span><span class="sxs-lookup"><span data-stu-id="75e23-268">![Manual password reset](./media/active-directory-identityprotection/1002.png "Manual password reset")</span></span>
2. <span data-ttu-id="75e23-269">I den **riskerar händelser** klickar du på en risk.</span><span class="sxs-lookup"><span data-stu-id="75e23-269">In the **Risk events** list, click a risk.</span></span>

    <span data-ttu-id="75e23-270">![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1003.png "manuell lösenordsåterställning")</span><span class="sxs-lookup"><span data-stu-id="75e23-270">![Manual password reset](./media/active-directory-identityprotection/1003.png "Manual password reset")</span></span>
3. <span data-ttu-id="75e23-271">Högerklicka på en användare på bladet risk.</span><span class="sxs-lookup"><span data-stu-id="75e23-271">On the risk blade, right-click a user.</span></span>

    <span data-ttu-id="75e23-272">![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1004.png "manuell lösenordsåterställning")</span><span class="sxs-lookup"><span data-stu-id="75e23-272">![Manual password reset](./media/active-directory-identityprotection/1004.png "Manual password reset")</span></span>

### <a name="closing-all-risk-events-for-a-user-manually"></a><span data-ttu-id="75e23-273">Stänger alla riskhändelser för en användare manuellt</span><span class="sxs-lookup"><span data-stu-id="75e23-273">Closing all risk events for a user manually</span></span>
<span data-ttu-id="75e23-274">I stället för att stänga manuellt riskhändelser för en användare individuellt, ger Azure Active Directory-identitetsskydd dig en metod för att stänga alla händelser för en användare med ett enda klick.</span><span class="sxs-lookup"><span data-stu-id="75e23-274">Instead of manually closing risk events for a user individually, Azure Active Directory Identity Protection also provides you with a method to close all events for a user with one click.</span></span>

<span data-ttu-id="75e23-275">![Åtgärder](./media/active-directory-identityprotection/2222.png "åtgärder")</span><span class="sxs-lookup"><span data-stu-id="75e23-275">![Actions](./media/active-directory-identityprotection/2222.png "Actions")</span></span>

<span data-ttu-id="75e23-276">När du klickar på **stänga alla händelser**, alla händelser har stängts och den berörda användaren inte längre är i fara.</span><span class="sxs-lookup"><span data-stu-id="75e23-276">When you click **Dismiss all events**, all events are closed and the affected user is no longer at risk.</span></span>

### <a name="remediating-user-risk-events"></a><span data-ttu-id="75e23-277">Hälsostatus riskhändelser för användaren</span><span class="sxs-lookup"><span data-stu-id="75e23-277">Remediating user risk events</span></span>

<span data-ttu-id="75e23-278">En är en åtgärd för att skydda en identitet eller en enhet som har tidigare eller misstänks vara hotad.</span><span class="sxs-lookup"><span data-stu-id="75e23-278">A remediation is an action to secure an identity or a device that was previously suspected or known to be compromised.</span></span> <span data-ttu-id="75e23-279">En Reparationsåtgärd återställer identitet eller enhet till ett säkert tillstånd och löser tidigare riskhändelser som associeras med identiteten eller enhet.</span><span class="sxs-lookup"><span data-stu-id="75e23-279">A remediation action restores the identity or device to a safe state, and resolves previous risk events associated with the identity or device.</span></span>

<span data-ttu-id="75e23-280">Om du vill reparera användaren riskhändelser kan du:</span><span class="sxs-lookup"><span data-stu-id="75e23-280">To remediate user risk events, you can:</span></span>

* <span data-ttu-id="75e23-281">Utför ett säkert lösenord för att åtgärda användaren riskhändelser manuellt</span><span class="sxs-lookup"><span data-stu-id="75e23-281">Perform a secure password reset to remediate user risk events manually</span></span>
* <span data-ttu-id="75e23-282">Konfigurera en säkerhetsprincip för användaren risk för att minimera eller reparera riskhändelser användare automatiskt</span><span class="sxs-lookup"><span data-stu-id="75e23-282">Configure a user risk security policy to mitigate or remediate user risk events automatically</span></span>
* <span data-ttu-id="75e23-283">Ny avbildning infekterade enheter</span><span class="sxs-lookup"><span data-stu-id="75e23-283">Re-image the infected device</span></span>  

#### <a name="manual-secure-password-reset"></a><span data-ttu-id="75e23-284">Manuell säker lösenordsåterställning</span><span class="sxs-lookup"><span data-stu-id="75e23-284">Manual secure password reset</span></span>
<span data-ttu-id="75e23-285">En säker lösenordsåterställning är en effektiv åtgärder för många riskhändelser och när utföra automatiskt stängs händelserna risk och beräknar risknivå för användaren.</span><span class="sxs-lookup"><span data-stu-id="75e23-285">A secure password reset is an effective remediation for many risk events, and when performed, automatically closes these risk events and recalculates the user risk level.</span></span> <span data-ttu-id="75e23-286">Du kan använda identitetsskydd instrumentpanelen för att starta en återställning av lösenord för en riskfyllda användare.</span><span class="sxs-lookup"><span data-stu-id="75e23-286">You can use the Identity Protection dashboard to initiate a password reset for a risky user.</span></span>

<span data-ttu-id="75e23-287">Dialogrutan relaterade innehåller två olika metoder för att återställa ett lösenord:</span><span class="sxs-lookup"><span data-stu-id="75e23-287">The related dialog provides two different methods to reset a password:</span></span>

<span data-ttu-id="75e23-288">**Återställ lösenord** – Välj **användaren att återställa sina lösenord** att tillåta användaren att återställa själva om användaren har registrerats för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="75e23-288">**Reset password** - Select **Require the user to reset their password** to allow the user to self-recover if the user has registered for multi-factor authentication.</span></span> <span data-ttu-id="75e23-289">Vid användarens nästa inloggning, kommer användaren krävs för att lösa en multifaktorautentisering utmaning har och måste sedan ändra lösenordet.</span><span class="sxs-lookup"><span data-stu-id="75e23-289">During the user's next sign-in, the user will be required to solve a multi-factor authentication challenge successfully and then, forced to change the password.</span></span> <span data-ttu-id="75e23-290">Det här alternativet är inte tillgängligt om användarkontot inte är redan registrerad multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="75e23-290">This option isn't available if the user account is not already registered multi-factor authentication.</span></span>

<span data-ttu-id="75e23-291">**Tillfälligt lösenord** – Välj **generera ett tillfälligt lösenord** omedelbart ogiltigförklara det befintliga lösenordet och skapa ett nytt tillfälligt lösenord för användaren.</span><span class="sxs-lookup"><span data-stu-id="75e23-291">**Temporary password** - Select **Generate a temporary password** to immediately invalidate the existing password, and create a new temporary password for the user.</span></span> <span data-ttu-id="75e23-292">Skicka ett nytt tillfälligt lösenord till en alternativ e-postadress för användaren eller användarens chef.</span><span class="sxs-lookup"><span data-stu-id="75e23-292">Send the new temporary password to an alternate email address for the user or to the user's manager.</span></span> <span data-ttu-id="75e23-293">Eftersom lösenordet är tillfälligt, uppmanas användaren att ändra lösenord vid inloggning.</span><span class="sxs-lookup"><span data-stu-id="75e23-293">Because the password is temporary, the user will be prompted to change the password upon sign-in.</span></span>

<span data-ttu-id="75e23-294">![Principen](./media/active-directory-identityprotection/1005.png "princip")</span><span class="sxs-lookup"><span data-stu-id="75e23-294">![Policy](./media/active-directory-identityprotection/1005.png "Policy")</span></span>

<span data-ttu-id="75e23-295">**Att öppna dialogrutan relaterade configuration**:</span><span class="sxs-lookup"><span data-stu-id="75e23-295">**To open the related configuration dialog**:</span></span>

1. <span data-ttu-id="75e23-296">På den **Azure AD Identity Protection** bladet, klickar du på **användare som har flaggats för risk**.</span><span class="sxs-lookup"><span data-stu-id="75e23-296">On the **Azure AD Identity Protection** blade, click **Users flagged for risk**.</span></span>

    <span data-ttu-id="75e23-297">![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1006.png "manuell lösenordsåterställning")</span><span class="sxs-lookup"><span data-stu-id="75e23-297">![Manual password reset](./media/active-directory-identityprotection/1006.png "Manual password reset")</span></span>
2. <span data-ttu-id="75e23-298">Välj en användare med minst en riskhändelser från listan över användare.</span><span class="sxs-lookup"><span data-stu-id="75e23-298">From the list of users, select a user with at least one risk events.</span></span>

    <span data-ttu-id="75e23-299">![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1007.png "manuell lösenordsåterställning")</span><span class="sxs-lookup"><span data-stu-id="75e23-299">![Manual password reset](./media/active-directory-identityprotection/1007.png "Manual password reset")</span></span>
3. <span data-ttu-id="75e23-300">Klicka på bladet användare **Återställ lösenord**.</span><span class="sxs-lookup"><span data-stu-id="75e23-300">On the user blade, click **Reset password**.</span></span>

    <span data-ttu-id="75e23-301">![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1008.png "manuell lösenordsåterställning")</span><span class="sxs-lookup"><span data-stu-id="75e23-301">![Manual password reset](./media/active-directory-identityprotection/1008.png "Manual password reset")</span></span>

### <a name="user-risk-security-policy"></a><span data-ttu-id="75e23-302">Användaren risk säkerhetsprincip</span><span class="sxs-lookup"><span data-stu-id="75e23-302">User risk security policy</span></span>
<span data-ttu-id="75e23-303">En säkerhetsprincip för användaren risk är en princip för villkorlig åtkomst som utvärderar risknivå till en specifik användare och tillämpar reparation och minskning åtgärder baserat på fördefinierade villkor och regler.</span><span class="sxs-lookup"><span data-stu-id="75e23-303">A user risk security policy is a conditional access policy that evaluates the risk level to a specific user and applies remediation and mitigation actions based on predefined conditions and rules.</span></span>

<span data-ttu-id="75e23-304">![Användarprincip ridk](./media/active-directory-identityprotection/1009.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="75e23-304">![User ridk policy](./media/active-directory-identityprotection/1009.png "User ridk policy")</span></span>

<span data-ttu-id="75e23-305">Azure AD Identity Protection hjälper dig att hantera minskning och reparationen av användare som har flaggats för risken genom att du kan:</span><span class="sxs-lookup"><span data-stu-id="75e23-305">Azure AD Identity Protection helps you manage the mitigation and remediation of users flagged for risk by enabling you to:</span></span>

* <span data-ttu-id="75e23-306">Ange de användare och grupper som principen gäller för:</span><span class="sxs-lookup"><span data-stu-id="75e23-306">Set the users and groups the policy applies to:</span></span>

    <span data-ttu-id="75e23-307">![Användarprincip ridk](./media/active-directory-identityprotection/1010.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="75e23-307">![User ridk policy](./media/active-directory-identityprotection/1010.png "User ridk policy")</span></span>
* <span data-ttu-id="75e23-308">Ange användaren risk nivån tröskelvärdet (låg, medel eller hög) som utlöser principen:</span><span class="sxs-lookup"><span data-stu-id="75e23-308">Set the user risk level threshold (low, medium, or high) that triggers the policy:</span></span>

    <span data-ttu-id="75e23-309">![Användarprincip ridk](./media/active-directory-identityprotection/1011.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="75e23-309">![User ridk policy](./media/active-directory-identityprotection/1011.png "User ridk policy")</span></span>
* <span data-ttu-id="75e23-310">Ange kontrollerna som tillämpas när principen utlöser:</span><span class="sxs-lookup"><span data-stu-id="75e23-310">Set the controls to be enforced when the policy triggers:</span></span>

    <span data-ttu-id="75e23-311">![Användarprincip ridk](./media/active-directory-identityprotection/1012.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="75e23-311">![User ridk policy](./media/active-directory-identityprotection/1012.png "User ridk policy")</span></span>
* <span data-ttu-id="75e23-312">Växla tillståndet för principen:</span><span class="sxs-lookup"><span data-stu-id="75e23-312">Switch the state of your policy:</span></span>

    <span data-ttu-id="75e23-313">![Användarprincip ridk](./media/active-directory-identityprotection/403.png "MFA-registrering")</span><span class="sxs-lookup"><span data-stu-id="75e23-313">![User ridk policy](./media/active-directory-identityprotection/403.png "MFA Registration")</span></span>
* <span data-ttu-id="75e23-314">Granska och utvärdera effekten av en ändring innan du aktiverar det:</span><span class="sxs-lookup"><span data-stu-id="75e23-314">Review and evaluate the impact of a change before activating it:</span></span>

    <span data-ttu-id="75e23-315">![Användarprincip ridk](./media/active-directory-identityprotection/1013.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="75e23-315">![User ridk policy](./media/active-directory-identityprotection/1013.png "User ridk policy")</span></span>

<span data-ttu-id="75e23-316">Om du väljer en **hög** tröskelvärdet minskar antalet gånger som en princip utlöses och minimera de negativa effekterna för användarna.</span><span class="sxs-lookup"><span data-stu-id="75e23-316">Choosing a **High** threshold reduces the number of times a policy is triggered and minimizes the impact to users.</span></span>
<span data-ttu-id="75e23-317">Men det omfattar inte **låg** och **medel** användare som har flaggats för risk från principen som inte kan skydda identiteter eller enheter som har tidigare eller misstänks vara hotad.</span><span class="sxs-lookup"><span data-stu-id="75e23-317">However, it excludes **Low** and **Medium** users flagged for risk from the policy, which may not secure identities or devices that were previously suspected or known to be compromised.</span></span>

<span data-ttu-id="75e23-318">När du ställer in principen.</span><span class="sxs-lookup"><span data-stu-id="75e23-318">When setting the policy,</span></span>

* <span data-ttu-id="75e23-319">Undanta användare som kommer att generera en mängd FALSKT positiva (utvecklare, säkerhetsanalytiker)</span><span class="sxs-lookup"><span data-stu-id="75e23-319">Exclude users who are likely to generate a lot of false-positives (developers, security analysts)</span></span>
* <span data-ttu-id="75e23-320">Undanta användare i språk om att aktivera principen inte är en praktisk (till exempel ingen åtkomst till supportavdelningen)</span><span class="sxs-lookup"><span data-stu-id="75e23-320">Exclude users in locales where enabling the policy is not practical (for example no access to helpdesk)</span></span>
* <span data-ttu-id="75e23-321">Använd en **hög** tröskelvärde under inledande skala, eller om du måste minska utmaningar som ses av användare.</span><span class="sxs-lookup"><span data-stu-id="75e23-321">Use a **High** threshold during initial policy roll out, or if you must minimize challenges seen by end users.</span></span>
* <span data-ttu-id="75e23-322">Använd en **låg** tröskelvärde om din organisation kräver högre säkerhet.</span><span class="sxs-lookup"><span data-stu-id="75e23-322">Use a **Low** threshold if your organization requires greater security.</span></span> <span data-ttu-id="75e23-323">Att välja en **låg** tröskelvärdet introducerar ytterligare användare logga in utmaningar, men ökad säkerhet.</span><span class="sxs-lookup"><span data-stu-id="75e23-323">Selecting a **Low** threshold introduces additional user sign-in challenges, but increased security.</span></span>

<span data-ttu-id="75e23-324">Rekommenderad standard för de flesta organisationer är att konfigurera en regel för en **medel** tröskelvärdet för att göra en avvägning mellan användbarhet och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="75e23-324">The recommended default for most organizations is to configure a rule for a **Medium** threshold to strike a balance between usability and security.</span></span>

<span data-ttu-id="75e23-325">En översikt över den relaterade användarupplevelsen finns:</span><span class="sxs-lookup"><span data-stu-id="75e23-325">For an overview of the related user experience, see:</span></span>

* <span data-ttu-id="75e23-326">[Komprometterat konto recovery flödet](active-directory-identityprotection-flows.md#compromised-account-recovery).</span><span class="sxs-lookup"><span data-stu-id="75e23-326">[Compromised account recovery flow](active-directory-identityprotection-flows.md#compromised-account-recovery).</span></span>  
* <span data-ttu-id="75e23-327">[Komprometteras kontot blockerades flödet](active-directory-identityprotection-flows.md#compromised-account-blocked).</span><span class="sxs-lookup"><span data-stu-id="75e23-327">[Compromised account blocked flow](active-directory-identityprotection-flows.md#compromised-account-blocked).</span></span>  

<span data-ttu-id="75e23-328">**Att öppna dialogrutan relaterade configuration**:</span><span class="sxs-lookup"><span data-stu-id="75e23-328">**To open the related configuration dialog**:</span></span>

- <span data-ttu-id="75e23-329">På den **Azure AD Identity Protection** blad i den **konfigurera** klickar du på **risk användarprincip**.</span><span class="sxs-lookup"><span data-stu-id="75e23-329">On the **Azure AD Identity Protection** blade, in the **Configure** section, click **User risk policy**.</span></span>

    <span data-ttu-id="75e23-330">![Användarprincip ridk](./media/active-directory-identityprotection/1009.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="75e23-330">![User ridk policy](./media/active-directory-identityprotection/1009.png "User ridk policy")</span></span>

### <a name="mitigating-user-risk-events"></a><span data-ttu-id="75e23-331">Minimera riskhändelser för användaren</span><span class="sxs-lookup"><span data-stu-id="75e23-331">Mitigating user risk events</span></span>
<span data-ttu-id="75e23-332">Administratörer kan ange en säkerhetsprincip för användaren risken att blockera användare vid inloggning beroende på vilken risknivå av.</span><span class="sxs-lookup"><span data-stu-id="75e23-332">Administrators can set a user risk security policy to block users upon sign-in depending on the risk level.</span></span>

<span data-ttu-id="75e23-333">Blockera en inloggning:</span><span class="sxs-lookup"><span data-stu-id="75e23-333">Blocking a sign-in:</span></span>

* <span data-ttu-id="75e23-334">Förhindrar generering av riskhändelser för nya användare för den berörda användaren</span><span class="sxs-lookup"><span data-stu-id="75e23-334">Prevents the generation of new user risk events for the affected user</span></span>
* <span data-ttu-id="75e23-335">Gör att administratörer kan reparera riskhändelser som påverkar användarens identitet och återställa den till ett säkert tillstånd manuellt</span><span class="sxs-lookup"><span data-stu-id="75e23-335">Enables administrators to manually remediate the risk events affecting the user's identity and restore it to a secure state</span></span>



## <a name="multi-factor-authentication-registration-policy"></a><span data-ttu-id="75e23-336">Princip för registrering av multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="75e23-336">Multi-factor authentication registration policy</span></span>
<span data-ttu-id="75e23-337">Azure Multi-Factor authentication är en metod för att verifiera vem du är som kräver mer än bara ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="75e23-337">Azure multi-factor authentication is a method of verifying who you are that requires the use of more than just a username and password.</span></span> <span data-ttu-id="75e23-338">Det ger ett andra säkerhetslager till användarinloggningar och transaktioner.</span><span class="sxs-lookup"><span data-stu-id="75e23-338">It provides a second layer of security to user sign-ins and transactions.</span></span>  
<span data-ttu-id="75e23-339">Vi rekommenderar att du behöver Azure Multi-Factor authentication för användarinloggningar eftersom den:</span><span class="sxs-lookup"><span data-stu-id="75e23-339">We recommend that you require Azure multi-factor authentication for user sign-ins because it:</span></span>

* <span data-ttu-id="75e23-340">Ger stark autentisering med ett antal alternativ för enkel verifiering</span><span class="sxs-lookup"><span data-stu-id="75e23-340">Delivers strong authentication with a range of easy verification options</span></span>
* <span data-ttu-id="75e23-341">Spelar en viktig roll i att förbereda din organisation vill skydda och återställa från kontot kompromisser</span><span class="sxs-lookup"><span data-stu-id="75e23-341">Plays a key role in preparing your organization to protect and recover from account compromises</span></span>

<span data-ttu-id="75e23-342">![Användarprincip ridk](./media/active-directory-identityprotection/1019.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="75e23-342">![User ridk policy](./media/active-directory-identityprotection/1019.png "User ridk policy")</span></span>

<span data-ttu-id="75e23-343">Mer information finns i [vad är Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="75e23-343">For more details, see [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>

<span data-ttu-id="75e23-344">Azure AD Identity Protection hjälper dig att hantera lansering av multifaktorautentisering registrering genom att konfigurera en princip som gör att du kan:</span><span class="sxs-lookup"><span data-stu-id="75e23-344">Azure AD Identity Protection helps you manage the roll-out of multi-factor authentication registration by configuring a policy that enables you to:</span></span>

* <span data-ttu-id="75e23-345">Ange de användare och grupper som principen gäller för:</span><span class="sxs-lookup"><span data-stu-id="75e23-345">Set the users and groups the policy applies to:</span></span>

    <span data-ttu-id="75e23-346">![Principen för MFA](./media/active-directory-identityprotection/1020.png "MFA-principen")</span><span class="sxs-lookup"><span data-stu-id="75e23-346">![MFA policy](./media/active-directory-identityprotection/1020.png "MFA policy")</span></span>
* <span data-ttu-id="75e23-347">Ange kontrollerna som tillämpas när principen utlöser::</span><span class="sxs-lookup"><span data-stu-id="75e23-347">Set the controls to be enforced when the policy triggers::</span></span>  

    <span data-ttu-id="75e23-348">![Principen för MFA](./media/active-directory-identityprotection/1021.png "MFA-principen")</span><span class="sxs-lookup"><span data-stu-id="75e23-348">![MFA policy](./media/active-directory-identityprotection/1021.png "MFA policy")</span></span>
* <span data-ttu-id="75e23-349">Växla tillståndet för principen:</span><span class="sxs-lookup"><span data-stu-id="75e23-349">Switch the state of your policy:</span></span>

    <span data-ttu-id="75e23-350">![Principen för MFA](./media/active-directory-identityprotection/403.png "MFA-principen")</span><span class="sxs-lookup"><span data-stu-id="75e23-350">![MFA policy](./media/active-directory-identityprotection/403.png "MFA policy")</span></span>
* <span data-ttu-id="75e23-351">Visa den aktuella registreringsstatusen:</span><span class="sxs-lookup"><span data-stu-id="75e23-351">View the current registration status:</span></span>

    <span data-ttu-id="75e23-352">![Principen för MFA](./media/active-directory-identityprotection/1022.png "MFA-principen")</span><span class="sxs-lookup"><span data-stu-id="75e23-352">![MFA policy](./media/active-directory-identityprotection/1022.png "MFA policy")</span></span>

<span data-ttu-id="75e23-353">En översikt över den relaterade användarupplevelsen finns:</span><span class="sxs-lookup"><span data-stu-id="75e23-353">For an overview of the related user experience, see:</span></span>

* <span data-ttu-id="75e23-354">[Multifaktorautentisering registrering flödet](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).</span><span class="sxs-lookup"><span data-stu-id="75e23-354">[Multi-factor authentication registration flow](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).</span></span>  
* <span data-ttu-id="75e23-355">[Logga in som inträffar med Azure AD Identity Protection](active-directory-identityprotection-flows.md).</span><span class="sxs-lookup"><span data-stu-id="75e23-355">[Sign-in experiences with Azure AD Identity Protection](active-directory-identityprotection-flows.md).</span></span>  

<span data-ttu-id="75e23-356">**Att öppna dialogrutan relaterade configuration**:</span><span class="sxs-lookup"><span data-stu-id="75e23-356">**To open the related configuration dialog**:</span></span>

- <span data-ttu-id="75e23-357">På den **Azure AD Identity Protection** blad i den **konfigurera** klickar du på **multifaktorautentisering registrering**.</span><span class="sxs-lookup"><span data-stu-id="75e23-357">On the **Azure AD Identity Protection** blade, in the **Configure** section, click **Multi-factor authentication registration**.</span></span>

    <span data-ttu-id="75e23-358">![Principen för MFA](./media/active-directory-identityprotection/1019.png "MFA-principen")</span><span class="sxs-lookup"><span data-stu-id="75e23-358">![MFA policy](./media/active-directory-identityprotection/1019.png "MFA policy")</span></span>

## <a name="next-steps"></a><span data-ttu-id="75e23-359">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75e23-359">Next steps</span></span>
* [<span data-ttu-id="75e23-360">Channel 9: Azure AD och Identity visa: Identity Protection Preview</span><span class="sxs-lookup"><span data-stu-id="75e23-360">Channel 9: Azure AD and Identity Show: Identity Protection Preview</span></span>](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

* [<span data-ttu-id="75e23-361">Aktivera Identitetsskydd för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="75e23-361">Enabling Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-enable.md)

* [<span data-ttu-id="75e23-362">Sårbarheter som identifieras av Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="75e23-362">Vulnerabilities detected by Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-vulnerabilities.md)

* [<span data-ttu-id="75e23-363">Azure Active Directory-riskhändelser</span><span class="sxs-lookup"><span data-stu-id="75e23-363">Azure Active Directory risk events</span></span>](active-directory-identity-protection-risk-events.md)

* [<span data-ttu-id="75e23-364">Azure Active Directory Identity Protection-aviseringar</span><span class="sxs-lookup"><span data-stu-id="75e23-364">Azure Active Directory Identity Protection notifications</span></span>](active-directory-identityprotection-notifications.md)

* [<span data-ttu-id="75e23-365">Azure Active Directory-identitetsskydd playbook</span><span class="sxs-lookup"><span data-stu-id="75e23-365">Azure Active Directory Identity Protection playbook</span></span>](active-directory-identityprotection-playbook.md)

* [<span data-ttu-id="75e23-366">Azure Active Directory Identity Protection – ordlista</span><span class="sxs-lookup"><span data-stu-id="75e23-366">Azure Active Directory Identity Protection glossary</span></span>](active-directory-identityprotection-glossary.md)

* [<span data-ttu-id="75e23-367">Logga in som inträffar med Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="75e23-367">Sign-in experiences with Azure AD Identity Protection</span></span>](active-directory-identityprotection-flows.md)

* [<span data-ttu-id="75e23-368">Azure Active Directory Identity Protection - så att avblockera användare</span><span class="sxs-lookup"><span data-stu-id="75e23-368">Azure Active Directory Identity Protection - How to unblock users</span></span>](active-directory-identityprotection-unblock-howto.md)

* [<span data-ttu-id="75e23-369">Kom igång med Azure Active Directory Identity Protection och Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="75e23-369">Get started with Azure Active Directory Identity Protection and Microsoft Graph</span></span>](active-directory-identityprotection-graph-getting-started.md)
