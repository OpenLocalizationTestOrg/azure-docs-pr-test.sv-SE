---
title: "aaaBest praxis för villkorlig åtkomst i Azure Active Directory | Microsoft Docs"
description: "Lär dig mer om saker du bör känna till och vad det är bör du undvika detta när du konfigurerar principer för villkorlig åtkomst."
services: active-directory
keywords: "villkorlig åtkomst tooapps, villkorlig åtkomst med Azure AD, säker åtkomst toocompany resurser, principer för villkorlig åtkomst"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4952f8746a2e583380b3bb99cfe2fbdae1c07b08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a><span data-ttu-id="dd9ad-104">Metodtips för villkorlig åtkomst i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dd9ad-104">Best practices for conditional access in Azure Active Directory</span></span>

<span data-ttu-id="dd9ad-105">Det här avsnittet ger information om saker du bör känna till och vad det är bör du undvika detta när du konfigurerar principer för villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-105">This topic provides you with information about things you should know and what it is you should avoid doing when configuring conditional access policies.</span></span> <span data-ttu-id="dd9ad-106">Innan du läser det här avsnittet bör du bekanta dig med hello begrepp och hello terminologi som beskrivs i [villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="dd9ad-106">Before reading this topic, you should familiarize yourself with hello concepts and hello terminology outlined in [Conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal.md)</span></span>

## <a name="what-you-should-know"></a><span data-ttu-id="dd9ad-107">Vad du bör känna till</span><span class="sxs-lookup"><span data-stu-id="dd9ad-107">What you should know</span></span>

### <a name="whats-required-toomake-a-policy-work"></a><span data-ttu-id="dd9ad-108">Vad har krävt toomake en princip arbete?</span><span class="sxs-lookup"><span data-stu-id="dd9ad-108">What’s required toomake a policy work?</span></span>

<span data-ttu-id="dd9ad-109">När du skapar en ny princip, finns det inga användare, grupper, appar eller åtkomstkontroller som valts.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-109">When you create a new policy, there are no users, groups, apps or access controls selected.</span></span>

![Molnappar](./media/active-directory-conditional-access-best-practices/02.png)


<span data-ttu-id="dd9ad-111">toomake principen fungerar måste du konfigurera hello följande:</span><span class="sxs-lookup"><span data-stu-id="dd9ad-111">toomake your policy work, you must configure hello following:</span></span>


|<span data-ttu-id="dd9ad-112">Vad</span><span class="sxs-lookup"><span data-stu-id="dd9ad-112">What</span></span>           | <span data-ttu-id="dd9ad-113">Hur</span><span class="sxs-lookup"><span data-stu-id="dd9ad-113">How</span></span>                                  | <span data-ttu-id="dd9ad-114">Varför</span><span class="sxs-lookup"><span data-stu-id="dd9ad-114">Why</span></span>|
|:--            | :--                                  | :-- |
|<span data-ttu-id="dd9ad-115">**Molnappar**</span><span class="sxs-lookup"><span data-stu-id="dd9ad-115">**Cloud apps**</span></span> |<span data-ttu-id="dd9ad-116">Du måste tooselect en eller flera appar.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-116">You need tooselect one or more apps.</span></span>  | <span data-ttu-id="dd9ad-117">hello målet för en princip för villkorlig åtkomst är tooenable du toofine-finjustera hur behöriga användare kan komma åt dina appar.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-117">hello goal of a conditional access policy is tooenable you toofine-tune how authorized users can access your apps.</span></span>|
| <span data-ttu-id="dd9ad-118">**Användare och grupper**</span><span class="sxs-lookup"><span data-stu-id="dd9ad-118">**Users and groups**</span></span> | <span data-ttu-id="dd9ad-119">Du behöver tooselect minst en användare eller grupp som är auktoriserade tooaccess hello molnappar som du har valt.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-119">You need tooselect at least one user or group that is authorized tooaccess hello cloud apps you have selected.</span></span> | <span data-ttu-id="dd9ad-120">En villkorlig åtkomstprincip som har ingen användare och grupper som har tilldelats, utlöses aldrig.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-120">A conditional access policy that has no users and groups assigned, is never triggered.</span></span> |
| <span data-ttu-id="dd9ad-121">**Åtkomstkontroll**</span><span class="sxs-lookup"><span data-stu-id="dd9ad-121">**Access controls**</span></span> | <span data-ttu-id="dd9ad-122">Du behöver tooselect minst en åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-122">You need tooselect at least one access control.</span></span> | <span data-ttu-id="dd9ad-123">Princip för processorn måste tooknow vilka toodo om villkoren är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-123">Your policy processor needs tooknow what toodo if your conditions are satisfied.</span></span>|


<span data-ttu-id="dd9ad-124">I tillägg toothese grundläggande krav, i de flesta fall bör du också konfigurera ett villkor.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-124">In addition toothese basic requirements, in many cases, you should also configure a condition.</span></span> <span data-ttu-id="dd9ad-125">När en princip fungerar också utan ett konfigurerat villkor, är villkor hello intresseväckande faktor för att finjustera åtkomst tooyour appar.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-125">While a policy would also work without a configured condition, conditions are hello driving factor for fine-tuning access tooyour apps.</span></span>


![Molnappar](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a><span data-ttu-id="dd9ad-127">Hur utvärderas tilldelningar?</span><span class="sxs-lookup"><span data-stu-id="dd9ad-127">How are assignments evaluated?</span></span>

<span data-ttu-id="dd9ad-128">Alla tilldelningar som är logiskt **and**.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-128">All assignments are logically **ANDed**.</span></span> <span data-ttu-id="dd9ad-129">Om du har konfigurerat, mer än en tilldelning tootrigger en princip, kommer alla tilldelningar måste vara uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-129">If you have more than one assignment configured, tootrigger a policy, all assignments must be satisfied.</span></span>  

<span data-ttu-id="dd9ad-130">Om du behöver tooconfigure ett plats-villkor som gäller tooall anslutningar från utanför företagets nätverk, kan du göra detta genom att:</span><span class="sxs-lookup"><span data-stu-id="dd9ad-130">If you need tooconfigure a location condition that applies tooall connections made from outside your organization's network, you can accomplish this by:</span></span>

- <span data-ttu-id="dd9ad-131">Inklusive **alla platser**</span><span class="sxs-lookup"><span data-stu-id="dd9ad-131">Including **All locations**</span></span>
- <span data-ttu-id="dd9ad-132">Förutom **alla betrodda IP-adresser**</span><span class="sxs-lookup"><span data-stu-id="dd9ad-132">Excluding **All trusted IPs**</span></span>

### <a name="what-happens-if-you-have-policies-in-hello-azure-classic-portal-and-azure-portal-configured"></a><span data-ttu-id="dd9ad-133">Vad händer om du har principer i hello klassiska Azure-portalen och Azure-portalen konfigurerad?</span><span class="sxs-lookup"><span data-stu-id="dd9ad-133">What happens if you have policies in hello Azure classic portal and Azure portal configured?</span></span>  
<span data-ttu-id="dd9ad-134">Båda principerna tillämpas av Azure Active Directory och hello användaren får åtkomst till endast när alla krav är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-134">Both policies are enforced by Azure Active Directory and hello user gets access only when all requirements are met.</span></span>

### <a name="what-happens-if-you-have-policies-in-hello-intune-silverlight-portal-and-hello-azure-portal"></a><span data-ttu-id="dd9ad-135">Vad händer om du har principer i hello Intune Silverlight-portalen och hello Azure Portal?</span><span class="sxs-lookup"><span data-stu-id="dd9ad-135">What happens if you have policies in hello Intune Silverlight portal and hello Azure Portal?</span></span>
<span data-ttu-id="dd9ad-136">Båda principerna tillämpas av Azure Active Directory och hello användaren får åtkomst till endast när alla krav är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-136">Both policies are enforced by Azure Active Directory and hello user gets access only when all requirements are met.</span></span>

### <a name="what-happens-if-i-have-multiple-policies-for-hello-same-user-configured"></a><span data-ttu-id="dd9ad-137">Vad händer om jag har flera principer för hello som konfigurerats för samma användare?</span><span class="sxs-lookup"><span data-stu-id="dd9ad-137">What happens if I have multiple policies for hello same user configured?</span></span>  
<span data-ttu-id="dd9ad-138">För varje inloggning, Azure Active Directory utvärderar alla principer och garanterar att alla krav är uppfyllda innan du beviljar åtkomst toohello användare.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-138">For every sign-in, Azure Active Directory evaluates all policies and ensures that all requirements are met before granted access toohello user.</span></span>


### <a name="does-conditional-access-work-with-exchange-activesync"></a><span data-ttu-id="dd9ad-139">Fungerar villkorlig åtkomst med Exchange ActiveSync</span><span class="sxs-lookup"><span data-stu-id="dd9ad-139">Does conditional access work with Exchange ActiveSync?</span></span>

<span data-ttu-id="dd9ad-140">Ja, du kan använda Exchange ActiveSync i en princip för villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-140">Yes, you can use Exchange ActiveSync in a conditional access policy.</span></span>


## <a name="what-you-should-avoid-doing"></a><span data-ttu-id="dd9ad-141">Vad du bör undvika att göra</span><span class="sxs-lookup"><span data-stu-id="dd9ad-141">What you should avoid doing</span></span>

<span data-ttu-id="dd9ad-142">hello villkorlig åtkomst framework ger dig en utmärkt konfigurationsflexibilitet.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-142">hello conditional access framework provides you with a great configuration flexibility.</span></span> <span data-ttu-id="dd9ad-143">Stor flexibilitet också innebär dock att du noggrant läser varje tidigare tooreleasing princip för configuration den tooavoid oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-143">However, great flexibility  also means that you should carefully review each configuration policy prior tooreleasing it tooavoid undesirable results.</span></span> <span data-ttu-id="dd9ad-144">I den här kontexten bör du bara särskild uppmärksamhet tooassignments som påverkar fullständiga **alla användare / grupper / molnappar**.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-144">In this context, you should pay special attention tooassignments affecting complete sets such as **all users / groups / cloud apps**.</span></span>

<span data-ttu-id="dd9ad-145">I din miljö bör du undvika hello följande konfigurationer:</span><span class="sxs-lookup"><span data-stu-id="dd9ad-145">In your environment, you should avoid hello following configurations:</span></span>


<span data-ttu-id="dd9ad-146">**För alla användare alla molnappar:**</span><span class="sxs-lookup"><span data-stu-id="dd9ad-146">**For all users, all cloud apps:**</span></span>

- <span data-ttu-id="dd9ad-147">**Blockera åtkomst** -den här konfigurationen blockerar hela organisationen, som definitivt inte är en bra idé.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-147">**Block access** - This configuration blocks your entire organization, which is definitely not a good idea.</span></span>

- <span data-ttu-id="dd9ad-148">**Kräver en kompatibel enhet** – för användare som inte har registrerat sina enheter än den här principen blockerar all åtkomst, inklusive åtkomst toohello Intune-portalen.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-148">**Require compliant device** - For users that don't have enrolled their devices yet, this policy blocks all access including access toohello Intune portal.</span></span> <span data-ttu-id="dd9ad-149">Om du är administratör saknar en registrerad enhet blockeras den här principen från att få tillbaka till hello Azure portal toochange hello principen.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-149">If you are an administrator without an enrolled device, this policy blocks you from getting back into hello Azure portal toochange hello policy.</span></span>

- <span data-ttu-id="dd9ad-150">**Kräva domänanslutning** – den här principen blockera åtkomst har också hello potentiella tooblock åtkomst för alla användare i organisationen om du inte har en domänansluten enhet ännu.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-150">**Require domain join** - This policy block access has also hello potential tooblock access for all users in your organization if you don't have a domain-joined device yet.</span></span>


<span data-ttu-id="dd9ad-151">**För alla användare, alla molnappar, alla enhetsplattformar:**</span><span class="sxs-lookup"><span data-stu-id="dd9ad-151">**For all users, all cloud apps, all device platforms:**</span></span>

- <span data-ttu-id="dd9ad-152">**Blockera åtkomst** -den här konfigurationen blockerar hela organisationen, som definitivt inte är en bra idé.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-152">**Block access** - This configuration blocks your entire organization, which is definitely not a good idea.</span></span>


## <a name="common-scenarios"></a><span data-ttu-id="dd9ad-153">Vanliga scenarier</span><span class="sxs-lookup"><span data-stu-id="dd9ad-153">Common scenarios</span></span>

### <a name="requiring-multi-factor-authentication-for-apps"></a><span data-ttu-id="dd9ad-154">Att kräva multifaktorautentisering för appar</span><span class="sxs-lookup"><span data-stu-id="dd9ad-154">Requiring multi-factor authentication for apps</span></span>

<span data-ttu-id="dd9ad-155">Många miljöer har appar som kräver en högre nivå av skydd än hello andra.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-155">Many environments have apps requiring a higher level of protection than hello others.</span></span>
<span data-ttu-id="dd9ad-156">Detta är till exempel hello fallet för appar som har åtkomst till toosensitive data.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-156">This is, for example, hello case for apps that have access toosensitive data.</span></span>
<span data-ttu-id="dd9ad-157">Om du vill tooadd ett extra lager av skydd toothese appar kan konfigurera du en princip för villkorlig åtkomst som kräver multifaktorautentisering när användarna kommer åt dessa appar.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-157">If you want tooadd another layer of protection toothese apps, you can configure a conditional access policy that requires multi-factor authentication when users are accessing these apps.</span></span>


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a><span data-ttu-id="dd9ad-158">Kräver multifaktorautentisering för åtkomst från nätverk som inte är betrodda</span><span class="sxs-lookup"><span data-stu-id="dd9ad-158">Requiring multi-factor authentication for access from networks that are not trusted</span></span>

<span data-ttu-id="dd9ad-159">Det här scenariot är liknande toohello föregående scenario eftersom den lägger till ett krav för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-159">This scenario is similar toohello previous scenario because it adds a requirement for multi-factor authentication.</span></span>
<span data-ttu-id="dd9ad-160">Hello största skillnaden är dock hello villkor för det här kravet.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-160">However, hello main difference is hello condition for this requirement.</span></span>  
<span data-ttu-id="dd9ad-161">Medan hello fokus på hello föregående scenario var i appar med åtkomst till toosensitve data, fokuserar hello i det här scenariot på betrodda platser.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-161">While hello focus of hello previous scenario was on apps with access toosensitve data, hello focus of this scenario is on trusted locations.</span></span>  
<span data-ttu-id="dd9ad-162">Du kan med andra ord ha ett krav för multifaktorautentisering om en app öppnas av en användare från ett nätverk som du inte litar på.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-162">In other words, you might have a requirement for multi-factor authentication if an app is accessed by a user from a network you don't trust.</span></span>


### <a name="only-trusted-devices-can-access-office-365-services"></a><span data-ttu-id="dd9ad-163">Endast betrodda enheter kan komma åt Office 365-tjänster</span><span class="sxs-lookup"><span data-stu-id="dd9ad-163">Only trusted devices can access Office 365 services</span></span>

<span data-ttu-id="dd9ad-164">Om du använder Intune i din miljö kan du direkt börja använda hello villkorlig åtkomst för gränssnittet i hello Azure-konsolen.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-164">If you are using Intune in your environment, you can immediately start using hello conditional access policy interface in hello Azure console.</span></span>

<span data-ttu-id="dd9ad-165">Många Intune-kunder använder villkorlig åtkomst tooensure att endast betrodda enheter kan komma åt Office 365-tjänster.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-165">Many Intune customers are using conditional access tooensure that only trusted devices can access Office 365 services.</span></span> <span data-ttu-id="dd9ad-166">Detta innebär att mobila enheter har registrerats med Intune och uppfyller krav på efterlevnad och att Windows-datorer är anslutna tooan lokal domän.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-166">This means that mobile devices are enrolled with Intune and meet compliance policy requirements, and that Windows PCs are joined tooan on-premises domain.</span></span> <span data-ttu-id="dd9ad-167">En viktiga förbättringar är att du inte har tooset hello samma princip för varje hello Office 365-tjänster.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-167">A key improvement is that you do not have tooset hello same policy for each of hello Office 365 services.</span></span>  <span data-ttu-id="dd9ad-168">När du skapar en ny princip, konfigurera hello molnet appar tooinclude varje hello O365-appar som du vill tooprotect med med villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="dd9ad-168">When you create a new policy, configure hello Cloud apps tooinclude each of hello O365 apps that you wish tooprotect with  with Conditional Access.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd9ad-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd9ad-169">Next steps</span></span>

<span data-ttu-id="dd9ad-170">Om du vill tooknow hur tooconfigure en princip för villkorlig åtkomst, se [Kom igång med villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="dd9ad-170">If you want tooknow how tooconfigure a conditional access policy, see [Get started with conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).</span></span>
