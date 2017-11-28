---
title: "Bästa praxis för villkorlig åtkomst i Azure Active Directory | Microsoft Docs"
description: "Lär dig mer om saker du bör känna till och vad det är bör du undvika detta när du konfigurerar principer för villkorlig åtkomst."
services: active-directory
keywords: "villkorlig åtkomst till appar, villkorlig åtkomst med Azure AD, säker åtkomst till företagets resurser, principer för villkorlig åtkomst"
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
ms.openlocfilehash: 3e524c116479c1af6eb6a601c9b57d27a697c5a2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a><span data-ttu-id="f2af8-104">Metodtips för villkorlig åtkomst i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f2af8-104">Best practices for conditional access in Azure Active Directory</span></span>

<span data-ttu-id="f2af8-105">Det här avsnittet ger information om saker du bör känna till och vad det är bör du undvika detta när du konfigurerar principer för villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f2af8-105">This topic provides you with information about things you should know and what it is you should avoid doing when configuring conditional access policies.</span></span> <span data-ttu-id="f2af8-106">Innan du läser det här avsnittet bör du bekanta dig med begrepp och termer som beskrivs i [villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="f2af8-106">Before reading this topic, you should familiarize yourself with the concepts and the terminology outlined in [Conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal.md)</span></span>

## <a name="what-you-should-know"></a><span data-ttu-id="f2af8-107">Vad du bör känna till</span><span class="sxs-lookup"><span data-stu-id="f2af8-107">What you should know</span></span>

### <a name="whats-required-to-make-a-policy-work"></a><span data-ttu-id="f2af8-108">Vad har krävs för att skapa en princip för att fungera?</span><span class="sxs-lookup"><span data-stu-id="f2af8-108">What’s required to make a policy work?</span></span>

<span data-ttu-id="f2af8-109">När du skapar en ny princip, finns det inga användare, grupper, appar eller åtkomstkontroller som valts.</span><span class="sxs-lookup"><span data-stu-id="f2af8-109">When you create a new policy, there are no users, groups, apps or access controls selected.</span></span>

![Molnappar](./media/active-directory-conditional-access-best-practices/02.png)


<span data-ttu-id="f2af8-111">Om du vill att din princip för att fungera, måste du konfigurera följande:</span><span class="sxs-lookup"><span data-stu-id="f2af8-111">To make your policy work, you must configure the following:</span></span>


|<span data-ttu-id="f2af8-112">Vad</span><span class="sxs-lookup"><span data-stu-id="f2af8-112">What</span></span>           | <span data-ttu-id="f2af8-113">Hur</span><span class="sxs-lookup"><span data-stu-id="f2af8-113">How</span></span>                                  | <span data-ttu-id="f2af8-114">Varför</span><span class="sxs-lookup"><span data-stu-id="f2af8-114">Why</span></span>|
|:--            | :--                                  | :-- |
|<span data-ttu-id="f2af8-115">**Molnappar**</span><span class="sxs-lookup"><span data-stu-id="f2af8-115">**Cloud apps**</span></span> |<span data-ttu-id="f2af8-116">Du måste välja en eller flera appar.</span><span class="sxs-lookup"><span data-stu-id="f2af8-116">You need to select one or more apps.</span></span>  | <span data-ttu-id="f2af8-117">Målet med en princip för villkorlig åtkomst är så att du kan finjustera hur behöriga användare kan komma åt dina appar.</span><span class="sxs-lookup"><span data-stu-id="f2af8-117">The goal of a conditional access policy is to enable you to fine-tune how authorized users can access your apps.</span></span>|
| <span data-ttu-id="f2af8-118">**Användare och grupper**</span><span class="sxs-lookup"><span data-stu-id="f2af8-118">**Users and groups**</span></span> | <span data-ttu-id="f2af8-119">Du måste välja minst en användare eller grupp som har behörighet att få åtkomst till molnappar som du har valt.</span><span class="sxs-lookup"><span data-stu-id="f2af8-119">You need to select at least one user or group that is authorized to access the cloud apps you have selected.</span></span> | <span data-ttu-id="f2af8-120">En villkorlig åtkomstprincip som har ingen användare och grupper som har tilldelats, utlöses aldrig.</span><span class="sxs-lookup"><span data-stu-id="f2af8-120">A conditional access policy that has no users and groups assigned, is never triggered.</span></span> |
| <span data-ttu-id="f2af8-121">**Åtkomstkontroll**</span><span class="sxs-lookup"><span data-stu-id="f2af8-121">**Access controls**</span></span> | <span data-ttu-id="f2af8-122">Du måste välja minst en åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="f2af8-122">You need to select at least one access control.</span></span> | <span data-ttu-id="f2af8-123">Princip för processorn behöver veta vad du gör om dina villkor är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="f2af8-123">Your policy processor needs to know what to do if your conditions are satisfied.</span></span>|


<span data-ttu-id="f2af8-124">Förutom de grundläggande kraven bör i många fall kan du också konfigurera ett villkor.</span><span class="sxs-lookup"><span data-stu-id="f2af8-124">In addition to these basic requirements, in many cases, you should also configure a condition.</span></span> <span data-ttu-id="f2af8-125">När en princip fungerar också utan ett konfigurerat villkor, är villkor drivande faktor för att finjustera åtkomst till dina appar.</span><span class="sxs-lookup"><span data-stu-id="f2af8-125">While a policy would also work without a configured condition, conditions are the driving factor for fine-tuning access to your apps.</span></span>


![Molnappar](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a><span data-ttu-id="f2af8-127">Hur utvärderas tilldelningar?</span><span class="sxs-lookup"><span data-stu-id="f2af8-127">How are assignments evaluated?</span></span>

<span data-ttu-id="f2af8-128">Alla tilldelningar som är logiskt **and**.</span><span class="sxs-lookup"><span data-stu-id="f2af8-128">All assignments are logically **ANDed**.</span></span> <span data-ttu-id="f2af8-129">Om du har mer än en tilldelning som konfigurerats för att utlösa en princip, måste alla tilldelningar vara uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="f2af8-129">If you have more than one assignment configured, to trigger a policy, all assignments must be satisfied.</span></span>  

<span data-ttu-id="f2af8-130">Om du behöver konfigurera en plats-villkor som gäller för alla anslutningar från utanför företagets nätverk åstadkommer detta genom att:</span><span class="sxs-lookup"><span data-stu-id="f2af8-130">If you need to configure a location condition that applies to all connections made from outside your organization's network, you can accomplish this by:</span></span>

- <span data-ttu-id="f2af8-131">Inklusive **alla platser**</span><span class="sxs-lookup"><span data-stu-id="f2af8-131">Including **All locations**</span></span>
- <span data-ttu-id="f2af8-132">Förutom **alla betrodda IP-adresser**</span><span class="sxs-lookup"><span data-stu-id="f2af8-132">Excluding **All trusted IPs**</span></span>

### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a><span data-ttu-id="f2af8-133">Vad händer om du har principer i den klassiska Azure-portalen och Azure-portalen konfigurerad?</span><span class="sxs-lookup"><span data-stu-id="f2af8-133">What happens if you have policies in the Azure classic portal and Azure portal configured?</span></span>  
<span data-ttu-id="f2af8-134">Båda principerna tillämpas av Azure Active Directory och användaren får åtkomst till endast när alla krav är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="f2af8-134">Both policies are enforced by Azure Active Directory and the user gets access only when all requirements are met.</span></span>

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a><span data-ttu-id="f2af8-135">Vad händer om du har principer i Intune Silverlight-portalen och Azure Portal?</span><span class="sxs-lookup"><span data-stu-id="f2af8-135">What happens if you have policies in the Intune Silverlight portal and the Azure Portal?</span></span>
<span data-ttu-id="f2af8-136">Båda principerna tillämpas av Azure Active Directory och användaren får åtkomst till endast när alla krav är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="f2af8-136">Both policies are enforced by Azure Active Directory and the user gets access only when all requirements are met.</span></span>

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a><span data-ttu-id="f2af8-137">Vad händer om jag har flera principer för samma användare konfigurerade?</span><span class="sxs-lookup"><span data-stu-id="f2af8-137">What happens if I have multiple policies for the same user configured?</span></span>  
<span data-ttu-id="f2af8-138">För varje inloggning, Azure Active Directory utvärderar alla principer och garanterar att alla krav är uppfyllda innan du beviljar åtkomst till användaren.</span><span class="sxs-lookup"><span data-stu-id="f2af8-138">For every sign-in, Azure Active Directory evaluates all policies and ensures that all requirements are met before granted access to the user.</span></span>


### <a name="does-conditional-access-work-with-exchange-activesync"></a><span data-ttu-id="f2af8-139">Fungerar villkorlig åtkomst med Exchange ActiveSync</span><span class="sxs-lookup"><span data-stu-id="f2af8-139">Does conditional access work with Exchange ActiveSync?</span></span>

<span data-ttu-id="f2af8-140">Ja, du kan använda Exchange ActiveSync i en princip för villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f2af8-140">Yes, you can use Exchange ActiveSync in a conditional access policy.</span></span>


## <a name="what-you-should-avoid-doing"></a><span data-ttu-id="f2af8-141">Vad du bör undvika att göra</span><span class="sxs-lookup"><span data-stu-id="f2af8-141">What you should avoid doing</span></span>

<span data-ttu-id="f2af8-142">Villkorlig åtkomst framework ger dig en utmärkt konfigurationsflexibilitet.</span><span class="sxs-lookup"><span data-stu-id="f2af8-142">The conditional access framework provides you with a great configuration flexibility.</span></span> <span data-ttu-id="f2af8-143">Stor flexibilitet innebär dock också att du noggrant läser varje princip för konfiguration innan du släpper den för att undvika oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="f2af8-143">However, great flexibility  also means that you should carefully review each configuration policy prior to releasing it to avoid undesirable results.</span></span> <span data-ttu-id="f2af8-144">I den här kontexten bör du vara särskilt uppmärksam på tilldelningar som påverkar fullständiga **alla användare / grupper / molnappar**.</span><span class="sxs-lookup"><span data-stu-id="f2af8-144">In this context, you should pay special attention to assignments affecting complete sets such as **all users / groups / cloud apps**.</span></span>

<span data-ttu-id="f2af8-145">I din miljö bör du undvika följande konfigurationer:</span><span class="sxs-lookup"><span data-stu-id="f2af8-145">In your environment, you should avoid the following configurations:</span></span>


<span data-ttu-id="f2af8-146">**För alla användare alla molnappar:**</span><span class="sxs-lookup"><span data-stu-id="f2af8-146">**For all users, all cloud apps:**</span></span>

- <span data-ttu-id="f2af8-147">**Blockera åtkomst** -den här konfigurationen blockerar hela organisationen, som definitivt inte är en bra idé.</span><span class="sxs-lookup"><span data-stu-id="f2af8-147">**Block access** - This configuration blocks your entire organization, which is definitely not a good idea.</span></span>

- <span data-ttu-id="f2af8-148">**Kräver en kompatibel enhet** – för användare som inte har registrerat sina enheter än den här principen blockerar all åtkomst, inklusive åtkomst till Intune-portalen.</span><span class="sxs-lookup"><span data-stu-id="f2af8-148">**Require compliant device** - For users that don't have enrolled their devices yet, this policy blocks all access including access to the Intune portal.</span></span> <span data-ttu-id="f2af8-149">Om du är administratör saknar en registrerad enhet blockerar du från att få tillbaka till Azure portal för att ändra principen i den här principen.</span><span class="sxs-lookup"><span data-stu-id="f2af8-149">If you are an administrator without an enrolled device, this policy blocks you from getting back into the Azure portal to change the policy.</span></span>

- <span data-ttu-id="f2af8-150">**Kräva domänanslutning** – den här principen blockera åtkomst har också möjlighet att blockera åtkomst för alla användare i organisationen om du inte har en domänansluten enhet ännu.</span><span class="sxs-lookup"><span data-stu-id="f2af8-150">**Require domain join** - This policy block access has also the potential to block access for all users in your organization if you don't have a domain-joined device yet.</span></span>


<span data-ttu-id="f2af8-151">**För alla användare, alla molnappar, alla enhetsplattformar:**</span><span class="sxs-lookup"><span data-stu-id="f2af8-151">**For all users, all cloud apps, all device platforms:**</span></span>

- <span data-ttu-id="f2af8-152">**Blockera åtkomst** -den här konfigurationen blockerar hela organisationen, som definitivt inte är en bra idé.</span><span class="sxs-lookup"><span data-stu-id="f2af8-152">**Block access** - This configuration blocks your entire organization, which is definitely not a good idea.</span></span>


## <a name="common-scenarios"></a><span data-ttu-id="f2af8-153">Vanliga scenarier</span><span class="sxs-lookup"><span data-stu-id="f2af8-153">Common scenarios</span></span>

### <a name="requiring-multi-factor-authentication-for-apps"></a><span data-ttu-id="f2af8-154">Att kräva multifaktorautentisering för appar</span><span class="sxs-lookup"><span data-stu-id="f2af8-154">Requiring multi-factor authentication for apps</span></span>

<span data-ttu-id="f2af8-155">Många miljöer har appar som kräver en högre nivå av skydd än den andra.</span><span class="sxs-lookup"><span data-stu-id="f2af8-155">Many environments have apps requiring a higher level of protection than the others.</span></span>
<span data-ttu-id="f2af8-156">Detta är exempelvis fallet för appar som har åtkomst till känslig information.</span><span class="sxs-lookup"><span data-stu-id="f2af8-156">This is, for example, the case for apps that have access to sensitive data.</span></span>
<span data-ttu-id="f2af8-157">Om du vill lägga till ett extra skyddslager för dessa appar kan du konfigurera en princip för villkorlig åtkomst som kräver multifaktorautentisering när användarna kommer åt dessa appar.</span><span class="sxs-lookup"><span data-stu-id="f2af8-157">If you want to add another layer of protection to these apps, you can configure a conditional access policy that requires multi-factor authentication when users are accessing these apps.</span></span>


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a><span data-ttu-id="f2af8-158">Kräver multifaktorautentisering för åtkomst från nätverk som inte är betrodda</span><span class="sxs-lookup"><span data-stu-id="f2af8-158">Requiring multi-factor authentication for access from networks that are not trusted</span></span>

<span data-ttu-id="f2af8-159">Det här scenariot liknar föregående scenario eftersom den lägger till ett krav för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="f2af8-159">This scenario is similar to the previous scenario because it adds a requirement for multi-factor authentication.</span></span>
<span data-ttu-id="f2af8-160">Den största skillnaden är dock villkoret för det här kravet.</span><span class="sxs-lookup"><span data-stu-id="f2af8-160">However, the main difference is the condition for this requirement.</span></span>  
<span data-ttu-id="f2af8-161">Medan fokus i det föregående scenariot var i appar med åtkomst till sensitve data, fokuserar i det här scenariot på betrodda platser.</span><span class="sxs-lookup"><span data-stu-id="f2af8-161">While the focus of the previous scenario was on apps with access to sensitve data, the focus of this scenario is on trusted locations.</span></span>  
<span data-ttu-id="f2af8-162">Du kan med andra ord ha ett krav för multifaktorautentisering om en app öppnas av en användare från ett nätverk som du inte litar på.</span><span class="sxs-lookup"><span data-stu-id="f2af8-162">In other words, you might have a requirement for multi-factor authentication if an app is accessed by a user from a network you don't trust.</span></span>


### <a name="only-trusted-devices-can-access-office-365-services"></a><span data-ttu-id="f2af8-163">Endast betrodda enheter kan komma åt Office 365-tjänster</span><span class="sxs-lookup"><span data-stu-id="f2af8-163">Only trusted devices can access Office 365 services</span></span>

<span data-ttu-id="f2af8-164">Om du använder Intune i din miljö kan du direkt börja använda gränssnittet för princip för villkorlig åtkomst i Azure-konsolen.</span><span class="sxs-lookup"><span data-stu-id="f2af8-164">If you are using Intune in your environment, you can immediately start using the conditional access policy interface in the Azure console.</span></span>

<span data-ttu-id="f2af8-165">Många Intune-kunder använder villkorlig åtkomst för att se till att endast betrodda enheter kan komma åt Office 365-tjänster.</span><span class="sxs-lookup"><span data-stu-id="f2af8-165">Many Intune customers are using conditional access to ensure that only trusted devices can access Office 365 services.</span></span> <span data-ttu-id="f2af8-166">Detta innebär att mobila enheter har registrerats med Intune och uppfyller krav på efterlevnad och att Windows-datorer är anslutna till en lokal domän.</span><span class="sxs-lookup"><span data-stu-id="f2af8-166">This means that mobile devices are enrolled with Intune and meet compliance policy requirements, and that Windows PCs are joined to an on-premises domain.</span></span> <span data-ttu-id="f2af8-167">En viktiga förbättringar är att du inte behöver ange samma princip för varje Office 365-tjänster.</span><span class="sxs-lookup"><span data-stu-id="f2af8-167">A key improvement is that you do not have to set the same policy for each of the Office 365 services.</span></span>  <span data-ttu-id="f2af8-168">När du skapar en ny princip, konfigurera molnappar för att inkludera varje O365-appar som du vill skydda med med villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f2af8-168">When you create a new policy, configure the Cloud apps to include each of the O365 apps that you wish to protect with  with Conditional Access.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f2af8-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f2af8-169">Next steps</span></span>

<span data-ttu-id="f2af8-170">Om du vill veta hur du konfigurerar en princip för villkorlig åtkomst finns [Kom igång med villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f2af8-170">If you want to know how to configure a conditional access policy, see [Get started with conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).</span></span>
