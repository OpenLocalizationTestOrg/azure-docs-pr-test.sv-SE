---
title: "aaaAzure Teknisk referens för Active Directory villkorlig åtkomst | Microsoft Docs"
description: "Med villkorlig åtkomstkontroll kontrollerar hello särskilda villkor som du väljer när du autentiserar användaren hello och innan åtkomst toohello program i Azure Active Directory. När dessa villkor är uppfyllda, hello användare autentiseras och tillåtet åtkomst toohello program."
services: active-directory.
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: ee201405d1d17f130607a95bf455b60cd222dd0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a><span data-ttu-id="9d20d-104">Teknisk referens för Azure Active Directory villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="9d20d-104">Azure Active Directory Conditional Access technical reference</span></span>

## <a name="services-enabled-with-conditional-access"></a><span data-ttu-id="9d20d-105">Services aktiverat med villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="9d20d-105">Services enabled with conditional access</span></span>

<span data-ttu-id="9d20d-106">Regler för villkorlig åtkomst stöds mellan olika typer av Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="9d20d-106">Conditional Access rules are supported across various Azure AD application types.</span></span> <span data-ttu-id="9d20d-107">Listan innehåller:</span><span class="sxs-lookup"><span data-stu-id="9d20d-107">This list includes:</span></span>


* <span data-ttu-id="9d20d-108">Program som är registrerade med hello Azure Application Proxy</span><span class="sxs-lookup"><span data-stu-id="9d20d-108">Applications registered with hello Azure Application Proxy</span></span>
* <span data-ttu-id="9d20d-109">Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="9d20d-109">Azure Remote App</span></span>
* <span data-ttu-id="9d20d-110">Utvecklade branschspecifika företag och program med flera klienter registrerad med Azure AD</span><span class="sxs-lookup"><span data-stu-id="9d20d-110">Developed line of business and multi-tenant applications registered with Azure AD</span></span>
* <span data-ttu-id="9d20d-111">Dynamics CRM</span><span class="sxs-lookup"><span data-stu-id="9d20d-111">Dynamics CRM</span></span>
* <span data-ttu-id="9d20d-112">Federerade program från hello Azure AD application gallery</span><span class="sxs-lookup"><span data-stu-id="9d20d-112">Federated applications from hello Azure AD application gallery</span></span>
* <span data-ttu-id="9d20d-113">Microsoft Office 365 Yammer</span><span class="sxs-lookup"><span data-stu-id="9d20d-113">Microsoft Office 365 Yammer</span></span>
* <span data-ttu-id="9d20d-114">Microsoft Office 365 Exchange Online</span><span class="sxs-lookup"><span data-stu-id="9d20d-114">Microsoft Office 365 Exchange Online</span></span>
* <span data-ttu-id="9d20d-115">Microsoft Office 365 SharePoint Online (inklusive OneDrive för företag)</span><span class="sxs-lookup"><span data-stu-id="9d20d-115">Microsoft Office 365 SharePoint Online (includes OneDrive for Business)</span></span>
* <span data-ttu-id="9d20d-116">Microsoft Power BI</span><span class="sxs-lookup"><span data-stu-id="9d20d-116">Microsoft Power BI</span></span> 
* <span data-ttu-id="9d20d-117">Lösenord SSO-program från hello Azure AD application gallery</span><span class="sxs-lookup"><span data-stu-id="9d20d-117">Password SSO applications from hello Azure AD application gallery</span></span>
* <span data-ttu-id="9d20d-118">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="9d20d-118">Visual Studio Team Services</span></span>
* <span data-ttu-id="9d20d-119">Microsoft Teams</span><span class="sxs-lookup"><span data-stu-id="9d20d-119">Microsoft Teams</span></span>









## <a name="enable-access-rules"></a><span data-ttu-id="9d20d-120">Aktivera åtkomstregler</span><span class="sxs-lookup"><span data-stu-id="9d20d-120">Enable access rules</span></span>
<span data-ttu-id="9d20d-121">Varje regel kan aktiveras eller inaktiveras på en per program baser.</span><span class="sxs-lookup"><span data-stu-id="9d20d-121">Each rule can be enabled or disabled on a per application bases.</span></span> <span data-ttu-id="9d20d-122">När reglerna är **ON** ska vara aktiverad och framtvingas för användare med åtkomst till programmet hello.</span><span class="sxs-lookup"><span data-stu-id="9d20d-122">When rules are **ON** they will be enabled and enforced for users accessing hello application.</span></span> <span data-ttu-id="9d20d-123">När de är **OFF** de används inte och kommer inte påverkan hello användare loggar in upplevelse.</span><span class="sxs-lookup"><span data-stu-id="9d20d-123">When they are **OFF** they will not be used and will not impact hello users sign in experience.</span></span>

## <a name="applying-rules-toospecific-users"></a><span data-ttu-id="9d20d-124">Tillämpa regler toospecific användare</span><span class="sxs-lookup"><span data-stu-id="9d20d-124">Applying rules toospecific users</span></span>
<span data-ttu-id="9d20d-125">Regler kan vara tillämpade toospecific uppsättningar med användare som är baserade på säkerhetsgruppen genom att ange **tillämpa på**.</span><span class="sxs-lookup"><span data-stu-id="9d20d-125">Rules can be applied toospecific sets of users based on security group by setting **Apply To**.</span></span> <span data-ttu-id="9d20d-126">**Tillämpa på** kan anges för**alla användare** eller **grupper**.</span><span class="sxs-lookup"><span data-stu-id="9d20d-126">**Apply To** can be set too**All Users** or **Groups**.</span></span> <span data-ttu-id="9d20d-127">När värdet för**alla användare** hello regler gäller tooany användare med åtkomst toohello program.</span><span class="sxs-lookup"><span data-stu-id="9d20d-127">When set too**All Users** hello rules will apply tooany user with access toohello application.</span></span> <span data-ttu-id="9d20d-128">Hej **grupper** alternativet tillåter specifika säkerhetsbehörigheter och distribution grupper toobe markerad regler tillämpas endast för dessa grupper.</span><span class="sxs-lookup"><span data-stu-id="9d20d-128">hello **Groups** option allows specific security and distribution groups toobe selected, rules will only be enforced for these groups.</span></span>

<span data-ttu-id="9d20d-129">När du distribuerar en regel, är det vanligt toofirst tillämpa det en begränsad uppsättning användare som är medlemmar i en pilot-grupper.</span><span class="sxs-lookup"><span data-stu-id="9d20d-129">When deploying a rule,  it is common toofirst apply it a limited set of users, that are members of a piloting groups.</span></span> <span data-ttu-id="9d20d-130">När fullständig hello regel kan användas för**alla användare**.</span><span class="sxs-lookup"><span data-stu-id="9d20d-130">Once complete hello rule can be applied too**All Users**.</span></span> <span data-ttu-id="9d20d-131">Detta innebär att hello regeln toobe tillämpas för alla användare i hello organisation.</span><span class="sxs-lookup"><span data-stu-id="9d20d-131">This will cause hello rule toobe enforced for all users in hello organization.</span></span>

<span data-ttu-id="9d20d-132">Välj grupper kan också undantas från principen med hjälp av hello **utom** alternativet.</span><span class="sxs-lookup"><span data-stu-id="9d20d-132">Select groups may also be exempted from policy using hello **Except** option.</span></span> <span data-ttu-id="9d20d-133">Alla medlemmar i dessa grupper kommer undantas även om de visas i en inkluderad grupp.</span><span class="sxs-lookup"><span data-stu-id="9d20d-133">Any members of these groups will be exempted even if they appear in an included group.</span></span>

## <a name="at-work-networks"></a><span data-ttu-id="9d20d-134">”På arbetet” nätverk</span><span class="sxs-lookup"><span data-stu-id="9d20d-134">“At work” networks</span></span>
<span data-ttu-id="9d20d-135">Regler för villkorlig åtkomst som använder en ”på arbetet”-nätverket som förlitar sig på tillförlitliga IP-adressintervall som har konfigurerats i Azure AD eller användning av hello ”i corpnet” anspråk från AD FS.</span><span class="sxs-lookup"><span data-stu-id="9d20d-135">Conditional access rules that use an “At work” network, rely on trusted IP address ranges that have been configured in Azure AD, or use of hello "inside corpnet" claim from AD FS.</span></span> <span data-ttu-id="9d20d-136">Bestämmelserna omfattar:</span><span class="sxs-lookup"><span data-stu-id="9d20d-136">These rules include:</span></span>

* <span data-ttu-id="9d20d-137">Kräv Multi-Factor authentication när de inte är på arbetet</span><span class="sxs-lookup"><span data-stu-id="9d20d-137">Require multi-factor authentication when not at work</span></span>
* <span data-ttu-id="9d20d-138">Blockera åtkomst när de inte är på arbetet</span><span class="sxs-lookup"><span data-stu-id="9d20d-138">Block access when not at work</span></span>

<span data-ttu-id="9d20d-139">Alternativ för att ange ”på arbetet” nätverk</span><span class="sxs-lookup"><span data-stu-id="9d20d-139">Options for specifiying “at work” networks</span></span>

1. <span data-ttu-id="9d20d-140">Konfigurera betrodda IP-adressintervall i hello [multifaktorautentisering konfigurationssidan](../multi-factor-authentication/multi-factor-authentication-whats-next.md).</span><span class="sxs-lookup"><span data-stu-id="9d20d-140">Configure trusted IP address ranges in hello [multi-factor authentication configuration page](../multi-factor-authentication/multi-factor-authentication-whats-next.md).</span></span> <span data-ttu-id="9d20d-141">Princip för villkorlig åtkomst använder hello konfigurerade intervallen för varje begäran och token utfärdande tooevaluate reglerna för autentisering.</span><span class="sxs-lookup"><span data-stu-id="9d20d-141">Conditional Access policy will use hello configured ranges on each authentication request and token issuance tooevaluate rules.</span></span> 
2. <span data-ttu-id="9d20d-142">Konfigurera användningen av hello inuti corpnet anspråk, det här alternativet kan användas med externa kataloger, med hjälp av AD FS.</span><span class="sxs-lookup"><span data-stu-id="9d20d-142">Configure use of hello inside corpnet claim, this option can be used with federated directories, using AD FS.</span></span> <span data-ttu-id="9d20d-143">toolearn mer om hello inuti corpnet anspråk, se [Tusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span><span class="sxs-lookup"><span data-stu-id="9d20d-143">toolearn more about hello inside corpnet claims, see [Tusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span></span>


## <a name="rules-based-on-application-sensitivity"></a><span data-ttu-id="9d20d-144">Regler baserat på program känslighet</span><span class="sxs-lookup"><span data-stu-id="9d20d-144">Rules based on application sensitivity</span></span>
<span data-ttu-id="9d20d-145">Regler är konfigurerade per program så att hello värdefulla tjänster toobe skyddas utan att påverka tooother tjänster.</span><span class="sxs-lookup"><span data-stu-id="9d20d-145">Rules are configured per application allowing hello high value services toobe secured without impacting access tooother services.</span></span> <span data-ttu-id="9d20d-146">Regler för villkorlig åtkomst kan konfigureras på hello **konfigurera** fliken av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="9d20d-146">Conditional access rules can be configured on hello  **Configure** tab of hello application.</span></span> 

<span data-ttu-id="9d20d-147">Regler som för närvarande finns:</span><span class="sxs-lookup"><span data-stu-id="9d20d-147">Rules currently offered:</span></span>

* <span data-ttu-id="9d20d-148">**Kräv Multi-Factor authentication**</span><span class="sxs-lookup"><span data-stu-id="9d20d-148">**Require multi-factor authentication**</span></span>
  
  * <span data-ttu-id="9d20d-149">Alla användare att den här principen är tillämpade toowill vara nödvändiga tooauthenticate via multifaktorautentisering minst en gång.</span><span class="sxs-lookup"><span data-stu-id="9d20d-149">All users that this policy is applied toowill be required tooauthenticate via multi-factor authentication at least once.</span></span>
* <span data-ttu-id="9d20d-150">**Kräv Multi-Factor authentication när de inte är på arbetet**</span><span class="sxs-lookup"><span data-stu-id="9d20d-150">**Require multi-factor authentication when not at work**</span></span>
  
  * <span data-ttu-id="9d20d-151">Om principen gäller kommer alla användare att nödvändiga toohave utförs multifaktorautentisering minst en gång om de komma åt hello-tjänsten från en annan plats fungerar.</span><span class="sxs-lookup"><span data-stu-id="9d20d-151">If this policy is applied, all users will be required toohave performed multi-factor authentication at least once if they access hello service from a non-work remote location.</span></span> <span data-ttu-id="9d20d-152">Om de flyttar från en plats för arbete tooremote vara de nödvändiga tooperform flerfunktionsautentisering när hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9d20d-152">If they move from a work tooremote location, they will be required tooperform multifactor authentication when accessing hello service.</span></span>
* <span data-ttu-id="9d20d-153">**Blockera åtkomst när de inte är på arbetet**</span><span class="sxs-lookup"><span data-stu-id="9d20d-153">**Block access when not at work**</span></span> 
  
  * <span data-ttu-id="9d20d-154">När användarna flyttar från arbete tooa fjärrplats, blockeras de om hello ”blockera åtkomst när de inte är på arbetet” principen är tillämpade toothem.</span><span class="sxs-lookup"><span data-stu-id="9d20d-154">When users move from work tooa remote location, they will be blocked if hello "Block access when not at work" policy is applied toothem.</span></span>  <span data-ttu-id="9d20d-155">De tillåts nytt åtkomst när de är på en plats för arbetet.</span><span class="sxs-lookup"><span data-stu-id="9d20d-155">They will be re-allowed access when at a work location.</span></span>

## <a name="related-topics"></a><span data-ttu-id="9d20d-156">Relaterade ämnen</span><span class="sxs-lookup"><span data-stu-id="9d20d-156">Related topics</span></span>
* [<span data-ttu-id="9d20d-157">Skydda åtkomsten tooOffice 365 och andra appar anslutna tooAzure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9d20d-157">Securing access tooOffice 365 and other apps connected tooAzure Active Directory</span></span>](active-directory-conditional-access.md)
* [<span data-ttu-id="9d20d-158">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9d20d-158">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)

