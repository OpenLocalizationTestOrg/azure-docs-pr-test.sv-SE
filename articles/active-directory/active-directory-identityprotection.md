---
title: aaaAzure Active Directory-identitetsskydd | Microsoft Docs
description: "Lär dig hur Azure AD Identity Protection kan du toolimit hello möjligheten för en angripare tooexploit en komprometterad identitet eller enheten och toosecure en identitet eller en enhet som tidigare var misstänkt eller kända toobe komprometteras."
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
ms.openlocfilehash: ecca4f3cdb65585687cf44a80024f26c7cab22ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection"></a><span data-ttu-id="03fc8-104">Identitetsskydd för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="03fc8-104">Azure Active Directory Identity Protection</span></span>

<span data-ttu-id="03fc8-105">Azure Active Directory Identity Protection är en funktion i hello Azure AD Premium P2-version som gör det möjligt att:</span><span class="sxs-lookup"><span data-stu-id="03fc8-105">Azure Active Directory Identity Protection is a feature of hello Azure AD Premium P2 edition that enables you to:</span></span>

- <span data-ttu-id="03fc8-106">Identifiera potentiella problem som påverkar din organisations identiteter</span><span class="sxs-lookup"><span data-stu-id="03fc8-106">Detect potential vulnerabilities affecting your organization’s identities</span></span>

- <span data-ttu-id="03fc8-107">Konfigurera automatiska svar toodetected misstänkta åtgärder som är relaterade tooyour organisations identiteter</span><span class="sxs-lookup"><span data-stu-id="03fc8-107">Configure automated responses toodetected suspicious actions that are related tooyour organization’s identities</span></span>  

- <span data-ttu-id="03fc8-108">Undersöka misstänkt incidenter och vidta lämplig åtgärd tooresolve dem.</span><span class="sxs-lookup"><span data-stu-id="03fc8-108">Investigate suspicious incidents and take appropriate action tooresolve them</span></span>   


## <a name="getting-started"></a><span data-ttu-id="03fc8-109">Komma igång</span><span class="sxs-lookup"><span data-stu-id="03fc8-109">Getting started</span></span>

<span data-ttu-id="03fc8-110">Microsoft skyddar molnbaserade identiteter för mer än en tio åren.</span><span class="sxs-lookup"><span data-stu-id="03fc8-110">Microsoft secures cloud-based identities for more than a decade.</span></span> <span data-ttu-id="03fc8-111">Med Azure Active Directory Identity Protection i din miljö kan du använda hello datorer med samma skydd Microsoft används toosecure identiteter.</span><span class="sxs-lookup"><span data-stu-id="03fc8-111">With Azure Active Directory Identity Protection, in your environment, you can use hello same protection systems Microsoft uses toosecure identities.</span></span>

<span data-ttu-id="03fc8-112">hello majoriteten av security överträdelser ta placera när angripare får åtkomst tooan miljö genom att stjäla en användares identitet.</span><span class="sxs-lookup"><span data-stu-id="03fc8-112">hello vast majority of security breaches take place when attackers gain access tooan environment by stealing a user’s identity.</span></span> <span data-ttu-id="03fc8-113">Angripare har blivit allt mer effektivt utnyttja från tredje part överträdelser och använda avancerade nätfiskeattacker hello års.</span><span class="sxs-lookup"><span data-stu-id="03fc8-113">Over hello years, attackers have become increasingly effective in leveraging third party breaches and using sophisticated phishing attacks.</span></span> <span data-ttu-id="03fc8-114">När en angripare får åtkomst tooeven låg Privilegierade konton, är det relativt enkelt för dem toogain åt tooimportant företagets resurser via lateral förflyttning.</span><span class="sxs-lookup"><span data-stu-id="03fc8-114">As soon as an attacker gains access tooeven low privileged user accounts, it is relatively easy for them toogain access tooimportant company resources through lateral movement.</span></span>

<span data-ttu-id="03fc8-115">Till följd av detta måste du:</span><span class="sxs-lookup"><span data-stu-id="03fc8-115">As a consequence of this, you need to:</span></span>

- <span data-ttu-id="03fc8-116">Skydda alla identiteter oavsett deras behörighetsnivå</span><span class="sxs-lookup"><span data-stu-id="03fc8-116">Protect all identities regardless of their privilege level</span></span>

- <span data-ttu-id="03fc8-117">Proaktivt förhindra avslöjade identiteter som missbrukat</span><span class="sxs-lookup"><span data-stu-id="03fc8-117">Proactively prevent compromised identities from being abused</span></span>

<span data-ttu-id="03fc8-118">Identifiering av avslöjade identiteter finns ingen enkel aktivitet.</span><span class="sxs-lookup"><span data-stu-id="03fc8-118">Discovering compromised identities is no easy task.</span></span> <span data-ttu-id="03fc8-119">Azure Active Directory använder anpassningsbar maskininlärningsalgoritmer och heuristik toodetect avvikelser och misstänkta incidenter som indikerar potentiellt komprometteras identiteter.</span><span class="sxs-lookup"><span data-stu-id="03fc8-119">Azure Active Directory uses adaptive machine learning algorithms and heuristics toodetect anomalies and suspicious incidents that indicate potentially compromised identities.</span></span> <span data-ttu-id="03fc8-120">Med dessa data kan identitetsskydd genererar rapporter och aviseringar som möjliggör tooevaluate hello identifierade problem och vidta lämplig lösning eller reparationsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="03fc8-120">Using this data, Identity Protection generates reports and alerts that enable you tooevaluate hello detected issues and take appropriate mitigation or remediation actions.</span></span>

<span data-ttu-id="03fc8-121">Azure Active Directory Identity Protection är mer än en övervakning och rapportering av verktyget.</span><span class="sxs-lookup"><span data-stu-id="03fc8-121">Azure Active Directory Identity Protection is more than a monitoring and reporting tool.</span></span> <span data-ttu-id="03fc8-122">tooprotect din organisations identiteter som du kan konfigurera risk-baserade principer som svarar toodetected problem automatiskt när en angiven risknivå har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="03fc8-122">tooprotect your organization's identities, you can configure risk-based policies that automatically respond toodetected issues when a specified risk level has been reached.</span></span> <span data-ttu-id="03fc8-123">Dessa principer dessutom tooother villkorlig åtkomst till kontroller som tillhandahålls av Azure Active Directory och EMS, kan blockera automatiskt eller initiera anpassningsbar reparationsåtgärder inklusive lösenordsåterställning och multifaktorautentisering tvingande.</span><span class="sxs-lookup"><span data-stu-id="03fc8-123">These policies, in addition tooother conditional access controls provided by Azure Active Directory and EMS, can either automatically block or initiate adaptive remediation actions including password resets and multi-factor authentication enforcement.</span></span>


#### <a name="identity-protection-capabilities"></a><span data-ttu-id="03fc8-124">Identitetsfunktionerna</span><span class="sxs-lookup"><span data-stu-id="03fc8-124">Identity Protection capabilities</span></span>

<span data-ttu-id="03fc8-125">**Identifiera säkerhetsrisker och riskfyllda konton:**</span><span class="sxs-lookup"><span data-stu-id="03fc8-125">**Detecting vulnerabilities and risky accounts:**</span></span>  

* <span data-ttu-id="03fc8-126">Att tillhandahålla anpassade rekommendationer tooimprove övergripande säkerhetstillståndet genom att markera säkerhetsrisker</span><span class="sxs-lookup"><span data-stu-id="03fc8-126">Providing custom recommendations tooimprove overall security posture by highlighting vulnerabilities</span></span>
* <span data-ttu-id="03fc8-127">Beräkning av risknivåer för inloggning</span><span class="sxs-lookup"><span data-stu-id="03fc8-127">Calculating sign-in risk levels</span></span>
* <span data-ttu-id="03fc8-128">Beräkning av risknivåer för användaren</span><span class="sxs-lookup"><span data-stu-id="03fc8-128">Calculating user risk levels</span></span>


<span data-ttu-id="03fc8-129">**Undersöka riskhändelser:**</span><span class="sxs-lookup"><span data-stu-id="03fc8-129">**Investigating risk events:**</span></span>

* <span data-ttu-id="03fc8-130">Skicka meddelanden för riskhändelser</span><span class="sxs-lookup"><span data-stu-id="03fc8-130">Sending notifications for risk events</span></span>
* <span data-ttu-id="03fc8-131">Undersöka riskhändelser med hjälp av relevanta och sammanhangsberoende information</span><span class="sxs-lookup"><span data-stu-id="03fc8-131">Investigating risk events using relevant and contextual information</span></span>
* <span data-ttu-id="03fc8-132">Tillhandahåller grundläggande arbetsflöden tootrack undersökningar</span><span class="sxs-lookup"><span data-stu-id="03fc8-132">Providing basic workflows tootrack investigations</span></span>
* <span data-ttu-id="03fc8-133">Att ge dig tillgång till tooremediation åtgärder, till exempel återställning av lösenord</span><span class="sxs-lookup"><span data-stu-id="03fc8-133">Providing easy access tooremediation actions such as password reset</span></span>

<span data-ttu-id="03fc8-134">**Principer för risk-baserad villkorlig åtkomst:**</span><span class="sxs-lookup"><span data-stu-id="03fc8-134">**Risk-based conditional access policies:**</span></span>

* <span data-ttu-id="03fc8-135">Principen toomitigate riskfyllda inloggningar genom att blockera inloggningar eller att kräva multifaktorautentisering utmaningar.</span><span class="sxs-lookup"><span data-stu-id="03fc8-135">Policy toomitigate risky sign-ins by blocking sign-ins or requiring multi-factor authentication challenges.</span></span>
* <span data-ttu-id="03fc8-136">Principen tooblock eller säker riskfyllda användarkonton</span><span class="sxs-lookup"><span data-stu-id="03fc8-136">Policy tooblock or secure risky user accounts</span></span>
* <span data-ttu-id="03fc8-137">Principen toorequire användare tooregister för multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="03fc8-137">Policy toorequire users tooregister for multi-factor authentication</span></span>



## <a name="identity-protection-roles"></a><span data-ttu-id="03fc8-138">Identity Protection roller</span><span class="sxs-lookup"><span data-stu-id="03fc8-138">Identity Protection roles</span></span>

<span data-ttu-id="03fc8-139">tooload saldo hello hanteringsaktiviteter runt implementeringen identitetsskydd, kan du tilldela flera roller.</span><span class="sxs-lookup"><span data-stu-id="03fc8-139">tooload balance hello management activities around your Identity Protection implementation, you can assign several roles.</span></span> <span data-ttu-id="03fc8-140">Azure AD Identity Protection stöder 3 directory roller:</span><span class="sxs-lookup"><span data-stu-id="03fc8-140">Azure AD Identity Protection supports 3 directory roles:</span></span>

| <span data-ttu-id="03fc8-141">Roll</span><span class="sxs-lookup"><span data-stu-id="03fc8-141">Role</span></span>                         | <span data-ttu-id="03fc8-142">Kan göra</span><span class="sxs-lookup"><span data-stu-id="03fc8-142">Can do</span></span>                          | <span data-ttu-id="03fc8-143">Det går inte att göra</span><span class="sxs-lookup"><span data-stu-id="03fc8-143">Cannot do</span></span>
| :--                          | ---                                |  ---   |
| <span data-ttu-id="03fc8-144">Global administratör</span><span class="sxs-lookup"><span data-stu-id="03fc8-144">Global administrator</span></span>         | <span data-ttu-id="03fc8-145">Fullständig åtkomst tooIdentity skydd, publicera identitetsskydd</span><span class="sxs-lookup"><span data-stu-id="03fc8-145">Full access tooIdentity Protection, Onboard Identity Protection</span></span>| |
| <span data-ttu-id="03fc8-146">Säkerhetsadministratör</span><span class="sxs-lookup"><span data-stu-id="03fc8-146">Security administrator</span></span>       | <span data-ttu-id="03fc8-147">Fullständig åtkomst tooIdentity skydd</span><span class="sxs-lookup"><span data-stu-id="03fc8-147">Full access tooIdentity Protection</span></span> | <span data-ttu-id="03fc8-148">Publicera identitetsskydd, återställa lösenord för en användare</span><span class="sxs-lookup"><span data-stu-id="03fc8-148">Onboard Identity Protection,  reset passwords for a user</span></span> |
| <span data-ttu-id="03fc8-149">Säkerhetsläsare</span><span class="sxs-lookup"><span data-stu-id="03fc8-149">Security reader</span></span>              | <span data-ttu-id="03fc8-150">Endast klar tooIdentity skydd</span><span class="sxs-lookup"><span data-stu-id="03fc8-150">Ready-only access tooIdentity Protection</span></span> | <span data-ttu-id="03fc8-151">Publicera identitetsskydd, remidiate användare konfigurera principer, återställa lösenord</span><span class="sxs-lookup"><span data-stu-id="03fc8-151">Onboard Identity Protection, remidiate users, configure policies,  reset passwords</span></span> |




<span data-ttu-id="03fc8-152">Mer information finns i [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="03fc8-152">For more details, see [Assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)</span></span>



## <a name="detection"></a><span data-ttu-id="03fc8-153">Detection (Identifiering)</span><span class="sxs-lookup"><span data-stu-id="03fc8-153">Detection</span></span>

### <a name="vulnerabilities"></a><span data-ttu-id="03fc8-154">Säkerhetsrisker</span><span class="sxs-lookup"><span data-stu-id="03fc8-154">Vulnerabilities</span></span>

<span data-ttu-id="03fc8-155">Azure Active Directory-identitetsskydd analyser konfigurationen och identifierar säkerhetsrisker som kan påverka din användaridentitet.</span><span class="sxs-lookup"><span data-stu-id="03fc8-155">Azure Active Directory Identity Protection analyses your configuration and detects vulnerabilities that can have an impact on your user's identities.</span></span> <span data-ttu-id="03fc8-156">Mer information finns i [sårbarheter som identifieras av Azure Active Directory Identity Protection](active-directory-identityprotection-vulnerabilities.md).</span><span class="sxs-lookup"><span data-stu-id="03fc8-156">For more details, see [Vulnerabilities detected by Azure Active Directory Identity Protection](active-directory-identityprotection-vulnerabilities.md).</span></span>

### <a name="risk-events"></a><span data-ttu-id="03fc8-157">Riskhändelser</span><span class="sxs-lookup"><span data-stu-id="03fc8-157">Risk events</span></span>

<span data-ttu-id="03fc8-158">Azure Active Directory använder anpassningsbar maskininlärning algoritmer och heuristik toodetect misstänkta åtgärder som är relaterade tooyour användaridentiteter.</span><span class="sxs-lookup"><span data-stu-id="03fc8-158">Azure Active Directory uses adaptive machine learning algorithms and heuristics toodetect suspicious actions that are related tooyour user's identities.</span></span> <span data-ttu-id="03fc8-159">hello skapas en post för varje upptäckt misstänkt åtgärd.</span><span class="sxs-lookup"><span data-stu-id="03fc8-159">hello system creates a record for each detected suspicious action.</span></span> <span data-ttu-id="03fc8-160">Dessa poster kallas även riskhändelser.</span><span class="sxs-lookup"><span data-stu-id="03fc8-160">These records are also known as risk events.</span></span>  
<span data-ttu-id="03fc8-161">Mer information finns i avsnittet om [Azure Active Directory-riskhändelser](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="03fc8-161">For more details, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span>


## <a name="investigation"></a><span data-ttu-id="03fc8-162">Undersökning</span><span class="sxs-lookup"><span data-stu-id="03fc8-162">Investigation</span></span>
<span data-ttu-id="03fc8-163">Din resa via Identity Protection startas normalt med hello identitetsskydd instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="03fc8-163">Your journey through Identity Protection typically starts with hello Identity Protection dashboard.</span></span>

<span data-ttu-id="03fc8-164">![Reparation](./media/active-directory-identityprotection/1000.png "reparation")</span><span class="sxs-lookup"><span data-stu-id="03fc8-164">![Remediation](./media/active-directory-identityprotection/1000.png "Remediation")</span></span>

<span data-ttu-id="03fc8-165">hello instrumentpanelen ger dig tillgång till:</span><span class="sxs-lookup"><span data-stu-id="03fc8-165">hello dashboard gives you access to:</span></span>

* <span data-ttu-id="03fc8-166">Rapporter som **användare som har flaggats för risk**, **riskerar händelser** och **säkerhetsrisker**</span><span class="sxs-lookup"><span data-stu-id="03fc8-166">Reports such as **Users flagged for risk**, **Risk events** and **Vulnerabilities**</span></span>
* <span data-ttu-id="03fc8-167">Inställningar, till exempel hello konfigurationen av din **säkerhetsprinciper**, **meddelanden** och **multifaktorautentisering registrering**</span><span class="sxs-lookup"><span data-stu-id="03fc8-167">Settings such as hello configuration of your **Security Policies**, **Notifications** and **multi-factor authentication registration**</span></span>

<span data-ttu-id="03fc8-168">Det är vanligtvis utgångspunkten för undersökningar, som är hello processen för att granska hello aktiviteter, loggar och annan relevant information relaterad tooa riskerar händelsen toodecide om reparation eller minimering steg är nödvändiga, och hur hello identity var komprometteras och förstå hur hello komprometteras identitet användes.</span><span class="sxs-lookup"><span data-stu-id="03fc8-168">It is typically your starting point for investigation, which is hello process of reviewing hello activities, logs, and other relevant information related tooa risk event toodecide whether remediation or mitigation steps are necessary,  and how hello identity was compromised, and understand how hello compromised identity was used.</span></span>

<span data-ttu-id="03fc8-169">Du kan koppla din undersökning aktiviteter toohello [meddelanden](active-directory-identityprotection-notifications.md) Azure Active Directory Protection skickar per e-post.</span><span class="sxs-lookup"><span data-stu-id="03fc8-169">You can tie your investigation activities toohello [notifications](active-directory-identityprotection-notifications.md) Azure Active Directory Protection sends per email.</span></span>

<span data-ttu-id="03fc8-170">hello följande avsnitt kan du med mer information och hello steg som är relaterade tooan undersökning.</span><span class="sxs-lookup"><span data-stu-id="03fc8-170">hello following sections provide you with more details and hello steps that are related tooan investigation.</span></span>  


## <a name="risky-sign-ins"></a><span data-ttu-id="03fc8-171">Riskfyllda inloggningar</span><span class="sxs-lookup"><span data-stu-id="03fc8-171">Risky sign-ins</span></span>

<span data-ttu-id="03fc8-172">Azure Active Directory identifierar [riskerar händelsetyper](active-directory-reporting-risk-events.md#risk-event-types) i realtid och offline.</span><span class="sxs-lookup"><span data-stu-id="03fc8-172">Azure Active Directory detects [risk event types](active-directory-reporting-risk-events.md#risk-event-types) in real-time and offline.</span></span> <span data-ttu-id="03fc8-173">Varje händelse för risker som har identifierats för en inloggning för en användare bidrar tooa logiskt koncept kallas riskfyllda inloggning.</span><span class="sxs-lookup"><span data-stu-id="03fc8-173">Each risk event that has been detected for a sign-in of a user contributes tooa logical concept called risky sign-in.</span></span> <span data-ttu-id="03fc8-174">Är en indikator för en inloggning försök som inte kanske har utförts av hello legitima ägare för ett användarkonto riskfyllda loggar in.</span><span class="sxs-lookup"><span data-stu-id="03fc8-174">A risky sign-in is an indicator for a sign-in attempt that might not have been performed by hello legitimate owner of a user account.</span></span>


### <a name="sign-in-risk-level"></a><span data-ttu-id="03fc8-175">Risknivå för inloggning</span><span class="sxs-lookup"><span data-stu-id="03fc8-175">Sign-in risk level</span></span>

<span data-ttu-id="03fc8-176">Logga in risknivå är en indikation (hög, medel eller låg) på hello sannolikheten att ett försök att logga in inte har utförts av hello legitima ägaren till ett användarkonto.</span><span class="sxs-lookup"><span data-stu-id="03fc8-176">A sign-in risk level is an indication (High, Medium, or Low) of hello likelihood that a sign-in attempt was not performed by hello legitimate owner of a user account.</span></span>

### <a name="mitigating-sign-in-risk-events"></a><span data-ttu-id="03fc8-177">Minimera riskhändelser för inloggning</span><span class="sxs-lookup"><span data-stu-id="03fc8-177">Mitigating sign-in risk events</span></span>

<span data-ttu-id="03fc8-178">En lösning är en åtgärd toolimit hello möjligheten för en angripare tooexploit en komprometterad identitet eller en enhet utan att återställa hello identitet eller enheten tooa felsäkert läge.</span><span class="sxs-lookup"><span data-stu-id="03fc8-178">A mitigation is an action toolimit hello ability of an attacker tooexploit a compromised identity or device without restoring hello identity or device tooa safe state.</span></span> <span data-ttu-id="03fc8-179">En lösning kan inte matchas tidigare inloggning riskhändelser som är associerade med hello identitet eller enhet.</span><span class="sxs-lookup"><span data-stu-id="03fc8-179">A mitigation does not resolve previous sign-in risk events associated with hello identity or device.</span></span>

<span data-ttu-id="03fc8-180">toomitigate riskfyllda inloggningar automatiskt, kan du konfigurera inloggning risk säkerhet policicies.</span><span class="sxs-lookup"><span data-stu-id="03fc8-180">toomitigate risky sign-ins automatically, you can configure sign-in risk security policicies.</span></span> <span data-ttu-id="03fc8-181">Med dessa principer kan du överväga hello risknivå för hello användare eller hello inloggning tooblock riskfyllda inloggningar eller kräver hello användaren tooperform Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="03fc8-181">Using these policies, you consider hello risk level of hello user or hello sign-in tooblock risky sign-ins or require hello user tooperform multi-factor authentication.</span></span> <span data-ttu-id="03fc8-182">Dessa åtgärder kan hindra en angripare från att stulna identitet toocause skador och ge några toosecure hello identiteten.</span><span class="sxs-lookup"><span data-stu-id="03fc8-182">These actions may prevent an attacker from exploiting a stolen identity toocause damage, and may give you some time toosecure hello identity.</span></span>

### <a name="sign-in-risk-security-policy"></a><span data-ttu-id="03fc8-183">Logga in risk säkerhetsprincip</span><span class="sxs-lookup"><span data-stu-id="03fc8-183">Sign-in risk security policy</span></span>
<span data-ttu-id="03fc8-184">Logga in riskprincipen är en princip för villkorlig åtkomst som utvärderar hello risk tooa specifika inloggning och tillämpar åtgärder baserat på fördefinierade villkor och regler.</span><span class="sxs-lookup"><span data-stu-id="03fc8-184">A sign-in risk policy is a conditional access policy that evaluates hello risk tooa specific sign-in and applies mitigations based on predefined conditions and rules.</span></span>

<span data-ttu-id="03fc8-185">![Logga in riskprincipen](./media/active-directory-identityprotection/1014.png "inloggning riskprincipen")</span><span class="sxs-lookup"><span data-stu-id="03fc8-185">![Sign-in risk policy](./media/active-directory-identityprotection/1014.png "Sign-in risk policy")</span></span>

<span data-ttu-id="03fc8-186">Azure AD Identity Protection hjälper dig att hantera hello minskning av riskfyllda inloggningar genom att du kan:</span><span class="sxs-lookup"><span data-stu-id="03fc8-186">Azure AD Identity Protection helps you manage hello mitigation of risky sign-ins by enabling you to:</span></span>

* <span data-ttu-id="03fc8-187">Ange hello användare och grupper hello-principen gäller för:</span><span class="sxs-lookup"><span data-stu-id="03fc8-187">Set hello users and groups hello policy applies to:</span></span>

    <span data-ttu-id="03fc8-188">![Logga in riskprincipen](./media/active-directory-identityprotection/1015.png "inloggning riskprincipen")</span><span class="sxs-lookup"><span data-stu-id="03fc8-188">![Sign-in risk policy](./media/active-directory-identityprotection/1015.png "Sign-in risk policy")</span></span>
* <span data-ttu-id="03fc8-189">Ange hello inloggning risk nivån tröskelvärde (låg, medel eller hög) som utlöser hello princip:</span><span class="sxs-lookup"><span data-stu-id="03fc8-189">Set hello sign-in risk level threshold (low, medium, or high) that triggers hello policy:</span></span>

    <span data-ttu-id="03fc8-190">![Logga in riskprincipen](./media/active-directory-identityprotection/1016.png "inloggning riskprincipen")</span><span class="sxs-lookup"><span data-stu-id="03fc8-190">![Sign-in risk policy](./media/active-directory-identityprotection/1016.png "Sign-in risk policy")</span></span>
* <span data-ttu-id="03fc8-191">Ange hello kontroller toobe tillämpas när hello princip utlöser:</span><span class="sxs-lookup"><span data-stu-id="03fc8-191">Set hello controls toobe enforced when hello policy triggers:</span></span>  

    <span data-ttu-id="03fc8-192">![Logga in riskprincipen](./media/active-directory-identityprotection/1017.png "inloggning riskprincipen")</span><span class="sxs-lookup"><span data-stu-id="03fc8-192">![Sign-in risk policy](./media/active-directory-identityprotection/1017.png "Sign-in risk policy")</span></span>
* <span data-ttu-id="03fc8-193">Växeln hello tillstånd för principen:</span><span class="sxs-lookup"><span data-stu-id="03fc8-193">Switch hello state of your policy:</span></span>

    <span data-ttu-id="03fc8-194">![MFA-registrering](./media/active-directory-identityprotection/403.png "MFA-registrering")</span><span class="sxs-lookup"><span data-stu-id="03fc8-194">![MFA Registration](./media/active-directory-identityprotection/403.png "MFA Registration")</span></span>
* <span data-ttu-id="03fc8-195">Granska och utvärdera hello resultatet av ändringen innan du aktiverar det:</span><span class="sxs-lookup"><span data-stu-id="03fc8-195">Review and evaluate hello impact of a change before activating it:</span></span>

    <span data-ttu-id="03fc8-196">![Logga in riskprincipen](./media/active-directory-identityprotection/1018.png "inloggning riskprincipen")</span><span class="sxs-lookup"><span data-stu-id="03fc8-196">![Sign-in risk policy](./media/active-directory-identityprotection/1018.png "Sign-in risk policy")</span></span>

#### <a name="what-you-need-tooknow"></a><span data-ttu-id="03fc8-197">Vad du behöver tooknow</span><span class="sxs-lookup"><span data-stu-id="03fc8-197">What you need tooknow</span></span>
<span data-ttu-id="03fc8-198">Du kan konfigurera en inloggning risk säkerhet princip toorequire multifaktorautentisering:</span><span class="sxs-lookup"><span data-stu-id="03fc8-198">You can configure a sign-in risk security policy toorequire multi-factor authentication:</span></span>

<span data-ttu-id="03fc8-199">![Logga in riskprincipen](./media/active-directory-identityprotection/1017.png "inloggning riskprincipen")</span><span class="sxs-lookup"><span data-stu-id="03fc8-199">![Sign-in risk policy](./media/active-directory-identityprotection/1017.png "Sign-in risk policy")</span></span>

<span data-ttu-id="03fc8-200">Men av säkerhetsskäl har fungerar den här inställningen bara för användare som redan har registrerats för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="03fc8-200">However, for security reasons, this setting only works for users that have already been registered for multi-factor authentication.</span></span> <span data-ttu-id="03fc8-201">Om hello villkoret toorequire multifaktorautentisering uppfylls för en användare som ännu inte har registrerats för multifaktorautentisering är hello användaren blockerad.</span><span class="sxs-lookup"><span data-stu-id="03fc8-201">If hello condition toorequire multi-factor authentication is satisfied for a user who is not yet registered for multi-factor authentication, hello user is blocked.</span></span>

<span data-ttu-id="03fc8-202">Som bästa praxis, om du vill toorequire Multi-Factor authentication för riskfyllda inloggningar, bör du:</span><span class="sxs-lookup"><span data-stu-id="03fc8-202">As a best practice, if you want toorequire multi-factor authentication for risky sign-ins, you should:</span></span>

1. <span data-ttu-id="03fc8-203">Aktivera hello [multifaktorautentisering registrering princip](#multi-factor-authentication-registration-policy) för hello påverkade användare.</span><span class="sxs-lookup"><span data-stu-id="03fc8-203">Enable hello [multi-factor authentication registration policy](#multi-factor-authentication-registration-policy) for hello affected users.</span></span>
2. <span data-ttu-id="03fc8-204">Kräv hello påverkade användare toologin i en icke-riskfyllda session tooperform en MFA-registrering</span><span class="sxs-lookup"><span data-stu-id="03fc8-204">Require hello affected users toologin in a non-risky session tooperform a MFA registration</span></span>

<span data-ttu-id="03fc8-205">Gör så här ser du till att multifaktorautentisering krävs för riskfyllda loggar in.</span><span class="sxs-lookup"><span data-stu-id="03fc8-205">Completing these steps ensures that multi-factor authentication is required for a risky sign-in.</span></span>

#### <a name="best-practices"></a><span data-ttu-id="03fc8-206">Bästa praxis</span><span class="sxs-lookup"><span data-stu-id="03fc8-206">Best practices</span></span>
<span data-ttu-id="03fc8-207">Om du väljer en **hög** tröskelvärdet minskar hello antalet gånger en princip utlöses och minimerar hello påverkan toousers.</span><span class="sxs-lookup"><span data-stu-id="03fc8-207">Choosing a **High** threshold reduces hello number of times a policy is triggered and minimizes hello impact toousers.</span></span>  

<span data-ttu-id="03fc8-208">Men det omfattar inte **låg** och **medel** inloggningar som flaggats för risk från hello-principen, som inte kan medföra att en angripare från att en komprometterad identitet.</span><span class="sxs-lookup"><span data-stu-id="03fc8-208">However, it excludes **Low** and **Medium** sign-ins flagged for risk from hello policy, which may not block an attacker from exploiting a compromised identity.</span></span>

<span data-ttu-id="03fc8-209">När inställningen hello principen</span><span class="sxs-lookup"><span data-stu-id="03fc8-209">When setting hello policy,</span></span>

* <span data-ttu-id="03fc8-210">Undanta användare som inte / kan inte ha multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="03fc8-210">Exclude users who do not/cannot have multi-factor authentication</span></span>
* <span data-ttu-id="03fc8-211">Undanta användare i där aktiverar hello principen inte är en praktisk språk (till exempel ingen åtkomst toohelpdesk)</span><span class="sxs-lookup"><span data-stu-id="03fc8-211">Exclude users in locales where enabling hello policy is not practical (for example no access toohelpdesk)</span></span>
* <span data-ttu-id="03fc8-212">Undanta användare som är sannolikt toogenerate mycket FALSKT positiva (utvecklare, säkerhetsanalytiker)</span><span class="sxs-lookup"><span data-stu-id="03fc8-212">Exclude users who are likely toogenerate a lot of false-positives (developers, security analysts)</span></span>
* <span data-ttu-id="03fc8-213">Använd en **hög** tröskelvärde under inledande skala, eller om du måste minska utmaningar som ses av användare.</span><span class="sxs-lookup"><span data-stu-id="03fc8-213">Use a **High** threshold during initial policy roll out, or if you must minimize challenges seen by end users.</span></span>
* <span data-ttu-id="03fc8-214">Använd en **låg** tröskelvärde om din organisation kräver högre säkerhet.</span><span class="sxs-lookup"><span data-stu-id="03fc8-214">Use a **Low**  threshold if your organization requires greater security.</span></span> <span data-ttu-id="03fc8-215">Att välja en **låg** tröskelvärdet introducerar ytterligare användare logga in utmaningar, men ökad säkerhet.</span><span class="sxs-lookup"><span data-stu-id="03fc8-215">Selecting a **Low** threshold introduces additional user sign-in challenges, but increased security.</span></span>

<span data-ttu-id="03fc8-216">hello rekommenderas standard för de flesta organisationer är tooconfigure en regel för en **medel** tröskelvärdet toostrike balans mellan användbarhet och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="03fc8-216">hello recommended default for most organizations is tooconfigure a rule for a **Medium** threshold toostrike a balance between usability and security.</span></span>

<span data-ttu-id="03fc8-217">hello inloggning riskprincipen är:</span><span class="sxs-lookup"><span data-stu-id="03fc8-217">hello sign-in risk policy is:</span></span>

* <span data-ttu-id="03fc8-218">Tillämpade tooall webbläsartrafik och inloggningar som använder modern autentisering.</span><span class="sxs-lookup"><span data-stu-id="03fc8-218">Applied tooall browser traffic and sign-ins using modern authentication.</span></span>
* <span data-ttu-id="03fc8-219">Ej tillämpad tooapplications med äldre säkerhetsprotokoll genom att inaktivera hello WS-Trust-slutpunkten på hello federerad IDP som AD FS.</span><span class="sxs-lookup"><span data-stu-id="03fc8-219">Not applied tooapplications using older security protocols by disabling hello WS-Trust endpoint at hello federated IDP, such as ADFS.</span></span>

<span data-ttu-id="03fc8-220">Hej **riskhändelser** i hello identitetsskydd konsolen visar alla händelser:</span><span class="sxs-lookup"><span data-stu-id="03fc8-220">hello **Risk Events** page in hello Identity Protection console lists all events:</span></span>

* <span data-ttu-id="03fc8-221">Den här principen har tillämpats på</span><span class="sxs-lookup"><span data-stu-id="03fc8-221">This policy was applied to</span></span>
* <span data-ttu-id="03fc8-222">Du kan granska hello aktiviteten och avgöra om hello åtgärden har lämplig</span><span class="sxs-lookup"><span data-stu-id="03fc8-222">You can review hello activity and determine whether hello action was appropriate or not</span></span>

<span data-ttu-id="03fc8-223">En översikt över hello relaterade användarupplevelsen, se:</span><span class="sxs-lookup"><span data-stu-id="03fc8-223">For an overview of hello related user experience, see:</span></span>

* [<span data-ttu-id="03fc8-224">Återställning för riskfyllda inloggning</span><span class="sxs-lookup"><span data-stu-id="03fc8-224">Risky sign-in recovery</span></span>](active-directory-identityprotection-flows.md#risky-sign-in-recovery)
* [<span data-ttu-id="03fc8-225">Riskfyllda inloggning blockeras</span><span class="sxs-lookup"><span data-stu-id="03fc8-225">Risky sign-in blocked</span></span>](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  
* [<span data-ttu-id="03fc8-226">Logga in som inträffar med Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="03fc8-226">Sign-in experiences with Azure AD Identity Protection</span></span>](active-directory-identityprotection-flows.md)  

<span data-ttu-id="03fc8-227">**tooopen hello relaterade konfigurationsdialogruta**:</span><span class="sxs-lookup"><span data-stu-id="03fc8-227">**tooopen hello related configuration dialog**:</span></span>

- <span data-ttu-id="03fc8-228">På hello **Azure AD Identity Protection** bladet i hello **konfigurera** klickar du på **inloggning riskprincipen**.</span><span class="sxs-lookup"><span data-stu-id="03fc8-228">On hello **Azure AD Identity Protection** blade, in hello **Configure** section, click **Sign-in risk policy**.</span></span>

    <span data-ttu-id="03fc8-229">![Användarprincip ridk](./media/active-directory-identityprotection/1014.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="03fc8-229">![User ridk policy](./media/active-directory-identityprotection/1014.png "User ridk policy")</span></span>



## <a name="users-flagged-for-risk"></a><span data-ttu-id="03fc8-230">Användare som har flaggats för risk</span><span class="sxs-lookup"><span data-stu-id="03fc8-230">Users flagged for risk</span></span>

<span data-ttu-id="03fc8-231">Alla aktiva [riskerar händelser](active-directory-identity-protection-risk-events.md) som identifierades av Azure Active Directory för en användare bidra tooa logiskt koncept kallas användaren risk.</span><span class="sxs-lookup"><span data-stu-id="03fc8-231">All active [risk events](active-directory-identity-protection-risk-events.md) that were detected by Azure Active Directory for a user contribute tooa logical concept called user risk.</span></span> <span data-ttu-id="03fc8-232">En användare som har flaggats för risk är en indikator för ett konto som kan ha drabbats.</span><span class="sxs-lookup"><span data-stu-id="03fc8-232">A user flagged for risk is an indicator for a user account that might have been compromised.</span></span>

![Användare som har flaggats för risk](./media/active-directory-identityprotection/1200.png)


### <a name="user-risk-level"></a><span data-ttu-id="03fc8-234">Risknivå för användaren</span><span class="sxs-lookup"><span data-stu-id="03fc8-234">User risk level</span></span>

<span data-ttu-id="03fc8-235">Risknivå för en användare är en indikation (hög, medel eller låg) på hello sannolikheten att hello användarens identitet har komprometterats.</span><span class="sxs-lookup"><span data-stu-id="03fc8-235">A user risk level is an indication (High, Medium, or Low) of hello likelihood that hello user’s identity has been compromised.</span></span> <span data-ttu-id="03fc8-236">Den beräknas baserat på hello användaren riskhändelser som är kopplade till en användarens identitet.</span><span class="sxs-lookup"><span data-stu-id="03fc8-236">It is calculated based on hello user risk events that are associated with a user's identity.</span></span>

<span data-ttu-id="03fc8-237">hello status för en risk händelse är antingen **Active** eller **stängd**.</span><span class="sxs-lookup"><span data-stu-id="03fc8-237">hello status of a risk event is either **Active** or **Closed**.</span></span> <span data-ttu-id="03fc8-238">Endast riskerar händelser som är **Active** bidra toohello användaren nivå vid beräkningen.</span><span class="sxs-lookup"><span data-stu-id="03fc8-238">Only risk events that are **Active** contribute toohello user risk level calculation.</span></span>

<span data-ttu-id="03fc8-239">hello användaren risknivå beräknas med hjälp av följande indata hello:</span><span class="sxs-lookup"><span data-stu-id="03fc8-239">hello user risk level is calculated using hello following inputs:</span></span>

* <span data-ttu-id="03fc8-240">Aktiva riskhändelser påverka hello användare</span><span class="sxs-lookup"><span data-stu-id="03fc8-240">Active risk events impacting hello user</span></span>
* <span data-ttu-id="03fc8-241">Risknivå för dessa händelser</span><span class="sxs-lookup"><span data-stu-id="03fc8-241">Risk level of these events</span></span>
* <span data-ttu-id="03fc8-242">Om eventuella åtgärder har utförts</span><span class="sxs-lookup"><span data-stu-id="03fc8-242">Whether any remediation actions have been taken</span></span>

<span data-ttu-id="03fc8-243">![Användaren risker](./media/active-directory-identityprotection/1031.png "användaren risker")</span><span class="sxs-lookup"><span data-stu-id="03fc8-243">![User risks](./media/active-directory-identityprotection/1031.png "User risks")</span></span>

<span data-ttu-id="03fc8-244">Du kan använda hello användaren risk nivåer toocreate principer för villkorlig åtkomst som blockera riskfyllda användare från att logga in eller tvinga dem toosecurely ändra sina lösenord.</span><span class="sxs-lookup"><span data-stu-id="03fc8-244">You can use hello user risk levels toocreate conditional access policies that block risky users from signing in, or force them toosecurely change their password.</span></span>

### <a name="closing-risk-events-manually"></a><span data-ttu-id="03fc8-245">Stänger riskhändelser manuellt</span><span class="sxs-lookup"><span data-stu-id="03fc8-245">Closing risk events manually</span></span>

<span data-ttu-id="03fc8-246">I de flesta fall ska vi ta reparationsåtgärder som en säker lösenordsåterställning tooautomatically Stäng riskhändelser.</span><span class="sxs-lookup"><span data-stu-id="03fc8-246">In most cases, you will take remediation actions such as a secure password reset tooautomatically close risk events.</span></span> <span data-ttu-id="03fc8-247">Men kanske det inte alltid är möjligt.</span><span class="sxs-lookup"><span data-stu-id="03fc8-247">However, this might not always be possible.</span></span>  
<span data-ttu-id="03fc8-248">Detta gäller, till exempel hello när:</span><span class="sxs-lookup"><span data-stu-id="03fc8-248">This is, for example, hello case, when:</span></span>

* <span data-ttu-id="03fc8-249">En användare med aktiva riskhändelser har tagits bort</span><span class="sxs-lookup"><span data-stu-id="03fc8-249">A user with Active risk events has been deleted</span></span>
* <span data-ttu-id="03fc8-250">En undersökningen visar att en händelse rapporteras risk utför av hello legitim användare</span><span class="sxs-lookup"><span data-stu-id="03fc8-250">An investigation reveals that a reported risk event has been perform by hello legitimate user</span></span>

<span data-ttu-id="03fc8-251">Eftersom riskhändelser som är **Active** bidra toohello användaren vid beräkningen kan du ha toomanually lägre en risknivå genom att stänga riskhändelser manuellt.</span><span class="sxs-lookup"><span data-stu-id="03fc8-251">Because risk events that are **Active** contribute toohello user risk calculation, you may have toomanually lower a risk level by closing risk events manually.</span></span>  
<span data-ttu-id="03fc8-252">Under hello undersökningar, kan du välja tootake någon av dessa åtgärder toochange hello status för en risk händelse:</span><span class="sxs-lookup"><span data-stu-id="03fc8-252">During hello course of investigation, you can choose tootake any of these actions toochange hello status of a risk event:</span></span>

<span data-ttu-id="03fc8-253">![Åtgärder](./media/active-directory-identityprotection/34.png "åtgärder")</span><span class="sxs-lookup"><span data-stu-id="03fc8-253">![Actions](./media/active-directory-identityprotection/34.png "Actions")</span></span>

* <span data-ttu-id="03fc8-254">**Lös** - om när du undersöker en risk händelse du använde en lämplig Reparationsåtgärd utanför identitetsskydd och du tror att hello risk händelsen ska betraktas stängs, markera hello händelse som löst.</span><span class="sxs-lookup"><span data-stu-id="03fc8-254">**Resolve** - If after investigating a risk event, you took an appropriate remediation action outside Identity Protection, and you believe that hello risk event should be considered closed, mark hello event as Resolved.</span></span> <span data-ttu-id="03fc8-255">Löst händelser anger hello risk händelse status tooClosed och hello risk händelsen kommer inte längre att bidra toouser risk.</span><span class="sxs-lookup"><span data-stu-id="03fc8-255">Resolved events will set hello risk event’s status tooClosed and hello risk event will no longer contribute toouser risk.</span></span>
* <span data-ttu-id="03fc8-256">**Markera som falska positiva** – i vissa fall kan du kan undersöka en händelse för risk och identifiera att det var felaktigt flaggas som en riskfyllda.</span><span class="sxs-lookup"><span data-stu-id="03fc8-256">**Mark as false-positive** - In some cases, you may investigate a risk event and discover that it was incorrectly flagged as a risky.</span></span> <span data-ttu-id="03fc8-257">Du kan minska hello antalet sådana händelser genom att markera hello risk händelse som falska positiva.</span><span class="sxs-lookup"><span data-stu-id="03fc8-257">You can help reduce hello number of such occurrences by marking hello risk event as False-positive.</span></span> <span data-ttu-id="03fc8-258">Detta hjälper hello algoritmer tooimprove hello klassificering av liknande händelser i hello framtida för maskininlärning.</span><span class="sxs-lookup"><span data-stu-id="03fc8-258">This will help hello machine learning algorithms tooimprove hello classification of similar events in hello future.</span></span> <span data-ttu-id="03fc8-259">hello status för falska positiva händelser är för**stängd** och de kommer inte längre att bidra toouser risk.</span><span class="sxs-lookup"><span data-stu-id="03fc8-259">hello status of false-positive events is too**Closed** and they will no longer contribute toouser risk.</span></span>
* <span data-ttu-id="03fc8-260">**Ignorera** - om du inte har vidtagit några Reparationsåtgärd, men vill hello risk händelse toobe bort från listan över aktiva hello, kan du markera en händelse för risk Ignorera och hello händelsestatus kommer att stängas.</span><span class="sxs-lookup"><span data-stu-id="03fc8-260">**Ignore** - If you have not taken any remediation action, but want hello risk event toobe removed from hello active list, you can mark a risk event Ignore and hello event status will be Closed.</span></span> <span data-ttu-id="03fc8-261">Ignorerade händelser bidrar inte toouser risk.</span><span class="sxs-lookup"><span data-stu-id="03fc8-261">Ignored events do not contribute toouser risk.</span></span> <span data-ttu-id="03fc8-262">Det här alternativet bör endast användas under ovanliga omständigheter.</span><span class="sxs-lookup"><span data-stu-id="03fc8-262">This option should only be used under unusual circumstances.</span></span>
* <span data-ttu-id="03fc8-263">**Återaktivera** -riskerar händelser som har stängts manuellt (genom att välja **lösa**, **falsklarm**, eller **Ignorera**) kan återaktiveras, ange hello händelsestatus tillbaka för**Active**.</span><span class="sxs-lookup"><span data-stu-id="03fc8-263">**Reactivate** - Risk events that were manually closed (by choosing **Resolve**, **False positive**, or **Ignore**) can be reactivated, setting hello event status back too**Active**.</span></span> <span data-ttu-id="03fc8-264">Återaktiverade riskhändelser bidrar toohello användaren nivå vid beräkningen.</span><span class="sxs-lookup"><span data-stu-id="03fc8-264">Reactivated risk events contribute toohello user risk level calculation.</span></span> <span data-ttu-id="03fc8-265">Riskhändelser stängd genom reparation (t.ex. en säker lösenordsåterställning) kan inte aktiveras igen.</span><span class="sxs-lookup"><span data-stu-id="03fc8-265">Risk events closed through remediation (such as a secure password reset) cannot be reactivated.</span></span>

<span data-ttu-id="03fc8-266">**tooopen hello relaterade konfigurationsdialogruta**:</span><span class="sxs-lookup"><span data-stu-id="03fc8-266">**tooopen hello related configuration dialog**:</span></span>

1. <span data-ttu-id="03fc8-267">På hello **Azure AD Identity Protection** bladet under **Undersök**, klickar du på **riskerar händelser**.</span><span class="sxs-lookup"><span data-stu-id="03fc8-267">On hello **Azure AD Identity Protection** blade, under **Investigate**, click **Risk events**.</span></span>

    <span data-ttu-id="03fc8-268">![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1002.png "manuell lösenordsåterställning")</span><span class="sxs-lookup"><span data-stu-id="03fc8-268">![Manual password reset](./media/active-directory-identityprotection/1002.png "Manual password reset")</span></span>
2. <span data-ttu-id="03fc8-269">I hello **riskerar händelser** klickar du på en risk.</span><span class="sxs-lookup"><span data-stu-id="03fc8-269">In hello **Risk events** list, click a risk.</span></span>

    <span data-ttu-id="03fc8-270">![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1003.png "manuell lösenordsåterställning")</span><span class="sxs-lookup"><span data-stu-id="03fc8-270">![Manual password reset](./media/active-directory-identityprotection/1003.png "Manual password reset")</span></span>
3. <span data-ttu-id="03fc8-271">Högerklicka på en användare på hello risk bladet.</span><span class="sxs-lookup"><span data-stu-id="03fc8-271">On hello risk blade, right-click a user.</span></span>

    <span data-ttu-id="03fc8-272">![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1004.png "manuell lösenordsåterställning")</span><span class="sxs-lookup"><span data-stu-id="03fc8-272">![Manual password reset](./media/active-directory-identityprotection/1004.png "Manual password reset")</span></span>

### <a name="closing-all-risk-events-for-a-user-manually"></a><span data-ttu-id="03fc8-273">Stänger alla riskhändelser för en användare manuellt</span><span class="sxs-lookup"><span data-stu-id="03fc8-273">Closing all risk events for a user manually</span></span>
<span data-ttu-id="03fc8-274">I stället för att stänga manuellt riskhändelser för en användare individuellt, ger Azure Active Directory-identitetsskydd dig en metod tooclose alla händelser för en användare med ett enda klick.</span><span class="sxs-lookup"><span data-stu-id="03fc8-274">Instead of manually closing risk events for a user individually, Azure Active Directory Identity Protection also provides you with a method tooclose all events for a user with one click.</span></span>

<span data-ttu-id="03fc8-275">![Åtgärder](./media/active-directory-identityprotection/2222.png "åtgärder")</span><span class="sxs-lookup"><span data-stu-id="03fc8-275">![Actions](./media/active-directory-identityprotection/2222.png "Actions")</span></span>

<span data-ttu-id="03fc8-276">När du klickar på **stänga alla händelser**, alla händelser har stängts och hello påverkade användare inte längre är i fara.</span><span class="sxs-lookup"><span data-stu-id="03fc8-276">When you click **Dismiss all events**, all events are closed and hello affected user is no longer at risk.</span></span>

### <a name="remediating-user-risk-events"></a><span data-ttu-id="03fc8-277">Hälsostatus riskhändelser för användaren</span><span class="sxs-lookup"><span data-stu-id="03fc8-277">Remediating user risk events</span></span>

<span data-ttu-id="03fc8-278">En är en åtgärd toosecure en identitet eller en enhet som har tidigare eller misstänks toobe komprometteras.</span><span class="sxs-lookup"><span data-stu-id="03fc8-278">A remediation is an action toosecure an identity or a device that was previously suspected or known toobe compromised.</span></span> <span data-ttu-id="03fc8-279">En Reparationsåtgärd återställer hello identitet eller enheten tooa säkert tillstånd och löser tidigare riskhändelser som är associerade med hello identitet eller enhet.</span><span class="sxs-lookup"><span data-stu-id="03fc8-279">A remediation action restores hello identity or device tooa safe state, and resolves previous risk events associated with hello identity or device.</span></span>

<span data-ttu-id="03fc8-280">tooremediate användaren riskhändelser, kan du:</span><span class="sxs-lookup"><span data-stu-id="03fc8-280">tooremediate user risk events, you can:</span></span>

* <span data-ttu-id="03fc8-281">Utför en riskhändelser för säkert lösenord Återställ tooremediate användare manuellt</span><span class="sxs-lookup"><span data-stu-id="03fc8-281">Perform a secure password reset tooremediate user risk events manually</span></span>
* <span data-ttu-id="03fc8-282">Konfigurera en användare risk säkerhet princip toomitigate eller reparera riskhändelser användare automatiskt</span><span class="sxs-lookup"><span data-stu-id="03fc8-282">Configure a user risk security policy toomitigate or remediate user risk events automatically</span></span>
* <span data-ttu-id="03fc8-283">Ny avbildning hello infekterade enheter</span><span class="sxs-lookup"><span data-stu-id="03fc8-283">Re-image hello infected device</span></span>  

#### <a name="manual-secure-password-reset"></a><span data-ttu-id="03fc8-284">Manuell säker lösenordsåterställning</span><span class="sxs-lookup"><span data-stu-id="03fc8-284">Manual secure password reset</span></span>
<span data-ttu-id="03fc8-285">En säker lösenordsåterställning är en effektiv åtgärder för många riskhändelser och när utföra automatiskt stängs händelserna risk och beräknar hello risknivå för användaren.</span><span class="sxs-lookup"><span data-stu-id="03fc8-285">A secure password reset is an effective remediation for many risk events, and when performed, automatically closes these risk events and recalculates hello user risk level.</span></span> <span data-ttu-id="03fc8-286">Du kan använda hello identitetsskydd instrumentpanelen tooinitiate en återställning av lösenord för en riskfyllda användare.</span><span class="sxs-lookup"><span data-stu-id="03fc8-286">You can use hello Identity Protection dashboard tooinitiate a password reset for a risky user.</span></span>

<span data-ttu-id="03fc8-287">hello innehåller relaterade två olika metoder tooreset lösenord:</span><span class="sxs-lookup"><span data-stu-id="03fc8-287">hello related dialog provides two different methods tooreset a password:</span></span>

<span data-ttu-id="03fc8-288">**Återställ lösenord** – Välj **kräver hello användaren tooreset sitt lösenord** tooallow hello användaren tooself återskapa om hello användaren har registrerats för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="03fc8-288">**Reset password** - Select **Require hello user tooreset their password** tooallow hello user tooself-recover if hello user has registered for multi-factor authentication.</span></span> <span data-ttu-id="03fc8-289">Under hello användarens nästa inloggning, kommer hello användaren att nödvändiga toosolve multifaktorautentisering utmaning har och sedan, framtvingad toochange hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="03fc8-289">During hello user's next sign-in, hello user will be required toosolve a multi-factor authentication challenge successfully and then, forced toochange hello password.</span></span> <span data-ttu-id="03fc8-290">Det här alternativet är inte tillgängligt om hello användarkonto inte är redan registrerad multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="03fc8-290">This option isn't available if hello user account is not already registered multi-factor authentication.</span></span>

<span data-ttu-id="03fc8-291">**Tillfälligt lösenord** – Välj **generera ett tillfälligt lösenord** tooimmediately ogiltigförklara hello befintliga lösenord och skapa ett nytt tillfälligt lösenord för hello användare.</span><span class="sxs-lookup"><span data-stu-id="03fc8-291">**Temporary password** - Select **Generate a temporary password** tooimmediately invalidate hello existing password, and create a new temporary password for hello user.</span></span> <span data-ttu-id="03fc8-292">Skicka hello nytt tillfälligt lösenord tooan alternativa e-postadress för hello användare eller toohello användarens chef.</span><span class="sxs-lookup"><span data-stu-id="03fc8-292">Send hello new temporary password tooan alternate email address for hello user or toohello user's manager.</span></span> <span data-ttu-id="03fc8-293">Eftersom hello lösenord är tillfällig kommer hello användaren att tillfrågas toochange hello lösenord vid inloggning.</span><span class="sxs-lookup"><span data-stu-id="03fc8-293">Because hello password is temporary, hello user will be prompted toochange hello password upon sign-in.</span></span>

<span data-ttu-id="03fc8-294">![Principen](./media/active-directory-identityprotection/1005.png "princip")</span><span class="sxs-lookup"><span data-stu-id="03fc8-294">![Policy](./media/active-directory-identityprotection/1005.png "Policy")</span></span>

<span data-ttu-id="03fc8-295">**tooopen hello relaterade konfigurationsdialogruta**:</span><span class="sxs-lookup"><span data-stu-id="03fc8-295">**tooopen hello related configuration dialog**:</span></span>

1. <span data-ttu-id="03fc8-296">På hello **Azure AD Identity Protection** bladet, klickar du på **användare som har flaggats för risk**.</span><span class="sxs-lookup"><span data-stu-id="03fc8-296">On hello **Azure AD Identity Protection** blade, click **Users flagged for risk**.</span></span>

    <span data-ttu-id="03fc8-297">![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1006.png "manuell lösenordsåterställning")</span><span class="sxs-lookup"><span data-stu-id="03fc8-297">![Manual password reset](./media/active-directory-identityprotection/1006.png "Manual password reset")</span></span>
2. <span data-ttu-id="03fc8-298">Välj en användare med minst en riskhändelser hello listan användare.</span><span class="sxs-lookup"><span data-stu-id="03fc8-298">From hello list of users, select a user with at least one risk events.</span></span>

    <span data-ttu-id="03fc8-299">![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1007.png "manuell lösenordsåterställning")</span><span class="sxs-lookup"><span data-stu-id="03fc8-299">![Manual password reset](./media/active-directory-identityprotection/1007.png "Manual password reset")</span></span>
3. <span data-ttu-id="03fc8-300">På bladet användare hello klickar du på **Återställ lösenord**.</span><span class="sxs-lookup"><span data-stu-id="03fc8-300">On hello user blade, click **Reset password**.</span></span>

    <span data-ttu-id="03fc8-301">![Manuell lösenordsåterställning](./media/active-directory-identityprotection/1008.png "manuell lösenordsåterställning")</span><span class="sxs-lookup"><span data-stu-id="03fc8-301">![Manual password reset](./media/active-directory-identityprotection/1008.png "Manual password reset")</span></span>

### <a name="user-risk-security-policy"></a><span data-ttu-id="03fc8-302">Användaren risk säkerhetsprincip</span><span class="sxs-lookup"><span data-stu-id="03fc8-302">User risk security policy</span></span>
<span data-ttu-id="03fc8-303">En säkerhetsprincip för användaren risk är en princip för villkorlig åtkomst som utvärderar hello risk nivå tooa specifik användare och tillämpar reparation och minskning åtgärder baserat på fördefinierade villkor och regler.</span><span class="sxs-lookup"><span data-stu-id="03fc8-303">A user risk security policy is a conditional access policy that evaluates hello risk level tooa specific user and applies remediation and mitigation actions based on predefined conditions and rules.</span></span>

<span data-ttu-id="03fc8-304">![Användarprincip ridk](./media/active-directory-identityprotection/1009.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="03fc8-304">![User ridk policy](./media/active-directory-identityprotection/1009.png "User ridk policy")</span></span>

<span data-ttu-id="03fc8-305">Azure AD Identity Protection hjälper dig att hantera hello minskning och reparationen av användare som har flaggats för risken genom att du kan:</span><span class="sxs-lookup"><span data-stu-id="03fc8-305">Azure AD Identity Protection helps you manage hello mitigation and remediation of users flagged for risk by enabling you to:</span></span>

* <span data-ttu-id="03fc8-306">Ange hello användare och grupper hello-principen gäller för:</span><span class="sxs-lookup"><span data-stu-id="03fc8-306">Set hello users and groups hello policy applies to:</span></span>

    <span data-ttu-id="03fc8-307">![Användarprincip ridk](./media/active-directory-identityprotection/1010.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="03fc8-307">![User ridk policy](./media/active-directory-identityprotection/1010.png "User ridk policy")</span></span>
* <span data-ttu-id="03fc8-308">Ange hello användaren risk nivån tröskelvärde (låg, medel eller hög) som utlöser hello princip:</span><span class="sxs-lookup"><span data-stu-id="03fc8-308">Set hello user risk level threshold (low, medium, or high) that triggers hello policy:</span></span>

    <span data-ttu-id="03fc8-309">![Användarprincip ridk](./media/active-directory-identityprotection/1011.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="03fc8-309">![User ridk policy](./media/active-directory-identityprotection/1011.png "User ridk policy")</span></span>
* <span data-ttu-id="03fc8-310">Ange hello kontroller toobe tillämpas när hello princip utlöser:</span><span class="sxs-lookup"><span data-stu-id="03fc8-310">Set hello controls toobe enforced when hello policy triggers:</span></span>

    <span data-ttu-id="03fc8-311">![Användarprincip ridk](./media/active-directory-identityprotection/1012.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="03fc8-311">![User ridk policy](./media/active-directory-identityprotection/1012.png "User ridk policy")</span></span>
* <span data-ttu-id="03fc8-312">Växeln hello tillstånd för principen:</span><span class="sxs-lookup"><span data-stu-id="03fc8-312">Switch hello state of your policy:</span></span>

    <span data-ttu-id="03fc8-313">![Användarprincip ridk](./media/active-directory-identityprotection/403.png "MFA-registrering")</span><span class="sxs-lookup"><span data-stu-id="03fc8-313">![User ridk policy](./media/active-directory-identityprotection/403.png "MFA Registration")</span></span>
* <span data-ttu-id="03fc8-314">Granska och utvärdera hello resultatet av ändringen innan du aktiverar det:</span><span class="sxs-lookup"><span data-stu-id="03fc8-314">Review and evaluate hello impact of a change before activating it:</span></span>

    <span data-ttu-id="03fc8-315">![Användarprincip ridk](./media/active-directory-identityprotection/1013.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="03fc8-315">![User ridk policy](./media/active-directory-identityprotection/1013.png "User ridk policy")</span></span>

<span data-ttu-id="03fc8-316">Om du väljer en **hög** tröskelvärdet minskar hello antalet gånger en princip utlöses och minimerar hello påverkan toousers.</span><span class="sxs-lookup"><span data-stu-id="03fc8-316">Choosing a **High** threshold reduces hello number of times a policy is triggered and minimizes hello impact toousers.</span></span>
<span data-ttu-id="03fc8-317">Men det omfattar inte **låg** och **medel** användare som har flaggats för risk från hello-principen, som inte kan skydda identiteter eller enheter som har tidigare eller misstänks toobe komprometteras.</span><span class="sxs-lookup"><span data-stu-id="03fc8-317">However, it excludes **Low** and **Medium** users flagged for risk from hello policy, which may not secure identities or devices that were previously suspected or known toobe compromised.</span></span>

<span data-ttu-id="03fc8-318">När inställningen hello principen</span><span class="sxs-lookup"><span data-stu-id="03fc8-318">When setting hello policy,</span></span>

* <span data-ttu-id="03fc8-319">Undanta användare som är sannolikt toogenerate mycket FALSKT positiva (utvecklare, säkerhetsanalytiker)</span><span class="sxs-lookup"><span data-stu-id="03fc8-319">Exclude users who are likely toogenerate a lot of false-positives (developers, security analysts)</span></span>
* <span data-ttu-id="03fc8-320">Undanta användare i där aktiverar hello principen inte är en praktisk språk (till exempel ingen åtkomst toohelpdesk)</span><span class="sxs-lookup"><span data-stu-id="03fc8-320">Exclude users in locales where enabling hello policy is not practical (for example no access toohelpdesk)</span></span>
* <span data-ttu-id="03fc8-321">Använd en **hög** tröskelvärde under inledande skala, eller om du måste minska utmaningar som ses av användare.</span><span class="sxs-lookup"><span data-stu-id="03fc8-321">Use a **High** threshold during initial policy roll out, or if you must minimize challenges seen by end users.</span></span>
* <span data-ttu-id="03fc8-322">Använd en **låg** tröskelvärde om din organisation kräver högre säkerhet.</span><span class="sxs-lookup"><span data-stu-id="03fc8-322">Use a **Low** threshold if your organization requires greater security.</span></span> <span data-ttu-id="03fc8-323">Att välja en **låg** tröskelvärdet introducerar ytterligare användare logga in utmaningar, men ökad säkerhet.</span><span class="sxs-lookup"><span data-stu-id="03fc8-323">Selecting a **Low** threshold introduces additional user sign-in challenges, but increased security.</span></span>

<span data-ttu-id="03fc8-324">hello rekommenderas standard för de flesta organisationer är tooconfigure en regel för en **medel** tröskelvärdet toostrike balans mellan användbarhet och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="03fc8-324">hello recommended default for most organizations is tooconfigure a rule for a **Medium** threshold toostrike a balance between usability and security.</span></span>

<span data-ttu-id="03fc8-325">En översikt över hello relaterade användarupplevelsen, se:</span><span class="sxs-lookup"><span data-stu-id="03fc8-325">For an overview of hello related user experience, see:</span></span>

* <span data-ttu-id="03fc8-326">[Komprometterat konto recovery flödet](active-directory-identityprotection-flows.md#compromised-account-recovery).</span><span class="sxs-lookup"><span data-stu-id="03fc8-326">[Compromised account recovery flow](active-directory-identityprotection-flows.md#compromised-account-recovery).</span></span>  
* <span data-ttu-id="03fc8-327">[Komprometteras kontot blockerades flödet](active-directory-identityprotection-flows.md#compromised-account-blocked).</span><span class="sxs-lookup"><span data-stu-id="03fc8-327">[Compromised account blocked flow](active-directory-identityprotection-flows.md#compromised-account-blocked).</span></span>  

<span data-ttu-id="03fc8-328">**tooopen hello relaterade konfigurationsdialogruta**:</span><span class="sxs-lookup"><span data-stu-id="03fc8-328">**tooopen hello related configuration dialog**:</span></span>

- <span data-ttu-id="03fc8-329">På hello **Azure AD Identity Protection** bladet i hello **konfigurera** klickar du på **risk användarprincip**.</span><span class="sxs-lookup"><span data-stu-id="03fc8-329">On hello **Azure AD Identity Protection** blade, in hello **Configure** section, click **User risk policy**.</span></span>

    <span data-ttu-id="03fc8-330">![Användarprincip ridk](./media/active-directory-identityprotection/1009.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="03fc8-330">![User ridk policy](./media/active-directory-identityprotection/1009.png "User ridk policy")</span></span>

### <a name="mitigating-user-risk-events"></a><span data-ttu-id="03fc8-331">Minimera riskhändelser för användaren</span><span class="sxs-lookup"><span data-stu-id="03fc8-331">Mitigating user risk events</span></span>
<span data-ttu-id="03fc8-332">Administratörer kan ange en användare risk säkerhet princip tooblock användaren vid inloggning beroende på hello risknivå.</span><span class="sxs-lookup"><span data-stu-id="03fc8-332">Administrators can set a user risk security policy tooblock users upon sign-in depending on hello risk level.</span></span>

<span data-ttu-id="03fc8-333">Blockera en inloggning:</span><span class="sxs-lookup"><span data-stu-id="03fc8-333">Blocking a sign-in:</span></span>

* <span data-ttu-id="03fc8-334">Förhindrar hello generering av nya användare riskhändelser för hello påverkade användare</span><span class="sxs-lookup"><span data-stu-id="03fc8-334">Prevents hello generation of new user risk events for hello affected user</span></span>
* <span data-ttu-id="03fc8-335">Låter administratörer toomanually reparera hello riskhändelser påverkar hello användarens identitet och återställa den tooa säkert tillstånd</span><span class="sxs-lookup"><span data-stu-id="03fc8-335">Enables administrators toomanually remediate hello risk events affecting hello user's identity and restore it tooa secure state</span></span>



## <a name="multi-factor-authentication-registration-policy"></a><span data-ttu-id="03fc8-336">Princip för registrering av multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="03fc8-336">Multi-factor authentication registration policy</span></span>
<span data-ttu-id="03fc8-337">Azure Multi-Factor authentication är en metod för att verifiera vem du är som kräver hello att mer än bara ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="03fc8-337">Azure multi-factor authentication is a method of verifying who you are that requires hello use of more than just a username and password.</span></span> <span data-ttu-id="03fc8-338">Det ger ett andra säkerhetslager säkerhet toouser inloggningar och transaktioner.</span><span class="sxs-lookup"><span data-stu-id="03fc8-338">It provides a second layer of security toouser sign-ins and transactions.</span></span>  
<span data-ttu-id="03fc8-339">Vi rekommenderar att du behöver Azure Multi-Factor authentication för användarinloggningar eftersom den:</span><span class="sxs-lookup"><span data-stu-id="03fc8-339">We recommend that you require Azure multi-factor authentication for user sign-ins because it:</span></span>

* <span data-ttu-id="03fc8-340">Ger stark autentisering med ett antal alternativ för enkel verifiering</span><span class="sxs-lookup"><span data-stu-id="03fc8-340">Delivers strong authentication with a range of easy verification options</span></span>
* <span data-ttu-id="03fc8-341">Spelar en viktig roll i att förbereda din organisation tooprotect och återställa från kontot kompromisser</span><span class="sxs-lookup"><span data-stu-id="03fc8-341">Plays a key role in preparing your organization tooprotect and recover from account compromises</span></span>

<span data-ttu-id="03fc8-342">![Användarprincip ridk](./media/active-directory-identityprotection/1019.png "ridk användarprincip")</span><span class="sxs-lookup"><span data-stu-id="03fc8-342">![User ridk policy](./media/active-directory-identityprotection/1019.png "User ridk policy")</span></span>

<span data-ttu-id="03fc8-343">Mer information finns i [vad är Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="03fc8-343">For more details, see [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>

<span data-ttu-id="03fc8-344">Azure AD Identity Protection hjälper dig att hantera hello lansering av multifaktorautentisering registrering genom att konfigurera en princip som gör att du kan:</span><span class="sxs-lookup"><span data-stu-id="03fc8-344">Azure AD Identity Protection helps you manage hello roll-out of multi-factor authentication registration by configuring a policy that enables you to:</span></span>

* <span data-ttu-id="03fc8-345">Ange hello användare och grupper hello-principen gäller för:</span><span class="sxs-lookup"><span data-stu-id="03fc8-345">Set hello users and groups hello policy applies to:</span></span>

    <span data-ttu-id="03fc8-346">![Principen för MFA](./media/active-directory-identityprotection/1020.png "MFA-principen")</span><span class="sxs-lookup"><span data-stu-id="03fc8-346">![MFA policy](./media/active-directory-identityprotection/1020.png "MFA policy")</span></span>
* <span data-ttu-id="03fc8-347">Ange hello kontroller toobe tillämpas när hello princip utlöser::</span><span class="sxs-lookup"><span data-stu-id="03fc8-347">Set hello controls toobe enforced when hello policy triggers::</span></span>  

    <span data-ttu-id="03fc8-348">![Principen för MFA](./media/active-directory-identityprotection/1021.png "MFA-principen")</span><span class="sxs-lookup"><span data-stu-id="03fc8-348">![MFA policy](./media/active-directory-identityprotection/1021.png "MFA policy")</span></span>
* <span data-ttu-id="03fc8-349">Växeln hello tillstånd för principen:</span><span class="sxs-lookup"><span data-stu-id="03fc8-349">Switch hello state of your policy:</span></span>

    <span data-ttu-id="03fc8-350">![Principen för MFA](./media/active-directory-identityprotection/403.png "MFA-principen")</span><span class="sxs-lookup"><span data-stu-id="03fc8-350">![MFA policy](./media/active-directory-identityprotection/403.png "MFA policy")</span></span>
* <span data-ttu-id="03fc8-351">Visa status för aktuella hello-registrering:</span><span class="sxs-lookup"><span data-stu-id="03fc8-351">View hello current registration status:</span></span>

    <span data-ttu-id="03fc8-352">![Principen för MFA](./media/active-directory-identityprotection/1022.png "MFA-principen")</span><span class="sxs-lookup"><span data-stu-id="03fc8-352">![MFA policy](./media/active-directory-identityprotection/1022.png "MFA policy")</span></span>

<span data-ttu-id="03fc8-353">En översikt över hello relaterade användarupplevelsen, se:</span><span class="sxs-lookup"><span data-stu-id="03fc8-353">For an overview of hello related user experience, see:</span></span>

* <span data-ttu-id="03fc8-354">[Multifaktorautentisering registrering flödet](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).</span><span class="sxs-lookup"><span data-stu-id="03fc8-354">[Multi-factor authentication registration flow](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).</span></span>  
* <span data-ttu-id="03fc8-355">[Logga in som inträffar med Azure AD Identity Protection](active-directory-identityprotection-flows.md).</span><span class="sxs-lookup"><span data-stu-id="03fc8-355">[Sign-in experiences with Azure AD Identity Protection](active-directory-identityprotection-flows.md).</span></span>  

<span data-ttu-id="03fc8-356">**tooopen hello relaterade konfigurationsdialogruta**:</span><span class="sxs-lookup"><span data-stu-id="03fc8-356">**tooopen hello related configuration dialog**:</span></span>

- <span data-ttu-id="03fc8-357">På hello **Azure AD Identity Protection** bladet i hello **konfigurera** klickar du på **multifaktorautentisering registrering**.</span><span class="sxs-lookup"><span data-stu-id="03fc8-357">On hello **Azure AD Identity Protection** blade, in hello **Configure** section, click **Multi-factor authentication registration**.</span></span>

    <span data-ttu-id="03fc8-358">![Principen för MFA](./media/active-directory-identityprotection/1019.png "MFA-principen")</span><span class="sxs-lookup"><span data-stu-id="03fc8-358">![MFA policy](./media/active-directory-identityprotection/1019.png "MFA policy")</span></span>

## <a name="next-steps"></a><span data-ttu-id="03fc8-359">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="03fc8-359">Next steps</span></span>
* [<span data-ttu-id="03fc8-360">Channel 9: Azure AD och Identity visa: Identity Protection Preview</span><span class="sxs-lookup"><span data-stu-id="03fc8-360">Channel 9: Azure AD and Identity Show: Identity Protection Preview</span></span>](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

* [<span data-ttu-id="03fc8-361">Aktivera Identitetsskydd för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="03fc8-361">Enabling Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-enable.md)

* [<span data-ttu-id="03fc8-362">Sårbarheter som identifieras av Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="03fc8-362">Vulnerabilities detected by Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-vulnerabilities.md)

* [<span data-ttu-id="03fc8-363">Azure Active Directory-riskhändelser</span><span class="sxs-lookup"><span data-stu-id="03fc8-363">Azure Active Directory risk events</span></span>](active-directory-identity-protection-risk-events.md)

* [<span data-ttu-id="03fc8-364">Azure Active Directory Identity Protection-aviseringar</span><span class="sxs-lookup"><span data-stu-id="03fc8-364">Azure Active Directory Identity Protection notifications</span></span>](active-directory-identityprotection-notifications.md)

* [<span data-ttu-id="03fc8-365">Azure Active Directory-identitetsskydd playbook</span><span class="sxs-lookup"><span data-stu-id="03fc8-365">Azure Active Directory Identity Protection playbook</span></span>](active-directory-identityprotection-playbook.md)

* [<span data-ttu-id="03fc8-366">Azure Active Directory Identity Protection – ordlista</span><span class="sxs-lookup"><span data-stu-id="03fc8-366">Azure Active Directory Identity Protection glossary</span></span>](active-directory-identityprotection-glossary.md)

* [<span data-ttu-id="03fc8-367">Logga in som inträffar med Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="03fc8-367">Sign-in experiences with Azure AD Identity Protection</span></span>](active-directory-identityprotection-flows.md)

* [<span data-ttu-id="03fc8-368">Azure Active Directory Identity Protection - hur toounblock användare</span><span class="sxs-lookup"><span data-stu-id="03fc8-368">Azure Active Directory Identity Protection - How toounblock users</span></span>](active-directory-identityprotection-unblock-howto.md)

* [<span data-ttu-id="03fc8-369">Kom igång med Azure Active Directory Identity Protection och Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="03fc8-369">Get started with Azure Active Directory Identity Protection and Microsoft Graph</span></span>](active-directory-identityprotection-graph-getting-started.md)
