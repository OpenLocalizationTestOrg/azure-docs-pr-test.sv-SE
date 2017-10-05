---
title: "Teknisk referens för Azure Active Directory villkorlig åtkomst | Microsoft Docs"
description: "Med villkorlig åtkomstkontroll kontrollerar de särskilda villkor som du väljer när du autentiserar användaren och innan du tillåter åtkomst till programmet i Azure Active Directory. När dessa villkor är uppfyllda, autentiserade användaren och få tillgång till programmet."
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
ms.openlocfilehash: ca16a5399f94fd1ab267e0798cade3fd83f75b13
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a><span data-ttu-id="7e20c-104">Teknisk referens för Azure Active Directory villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="7e20c-104">Azure Active Directory Conditional Access technical reference</span></span>

## <a name="services-enabled-with-conditional-access"></a><span data-ttu-id="7e20c-105">Services aktiverat med villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="7e20c-105">Services enabled with conditional access</span></span>

<span data-ttu-id="7e20c-106">Regler för villkorlig åtkomst stöds mellan olika typer av Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="7e20c-106">Conditional Access rules are supported across various Azure AD application types.</span></span> <span data-ttu-id="7e20c-107">Listan innehåller:</span><span class="sxs-lookup"><span data-stu-id="7e20c-107">This list includes:</span></span>


* <span data-ttu-id="7e20c-108">Program som är registrerade med Azure Application Proxy</span><span class="sxs-lookup"><span data-stu-id="7e20c-108">Applications registered with the Azure Application Proxy</span></span>
* <span data-ttu-id="7e20c-109">Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="7e20c-109">Azure Remote App</span></span>
* <span data-ttu-id="7e20c-110">Utvecklade branschspecifika företag och program med flera klienter registrerad med Azure AD</span><span class="sxs-lookup"><span data-stu-id="7e20c-110">Developed line of business and multi-tenant applications registered with Azure AD</span></span>
* <span data-ttu-id="7e20c-111">Dynamics CRM</span><span class="sxs-lookup"><span data-stu-id="7e20c-111">Dynamics CRM</span></span>
* <span data-ttu-id="7e20c-112">Federerade program från Azure AD application gallery</span><span class="sxs-lookup"><span data-stu-id="7e20c-112">Federated applications from the Azure AD application gallery</span></span>
* <span data-ttu-id="7e20c-113">Microsoft Office 365 Yammer</span><span class="sxs-lookup"><span data-stu-id="7e20c-113">Microsoft Office 365 Yammer</span></span>
* <span data-ttu-id="7e20c-114">Microsoft Office 365 Exchange Online</span><span class="sxs-lookup"><span data-stu-id="7e20c-114">Microsoft Office 365 Exchange Online</span></span>
* <span data-ttu-id="7e20c-115">Microsoft Office 365 SharePoint Online (inklusive OneDrive för företag)</span><span class="sxs-lookup"><span data-stu-id="7e20c-115">Microsoft Office 365 SharePoint Online (includes OneDrive for Business)</span></span>
* <span data-ttu-id="7e20c-116">Microsoft Power BI</span><span class="sxs-lookup"><span data-stu-id="7e20c-116">Microsoft Power BI</span></span> 
* <span data-ttu-id="7e20c-117">Lösenord SSO-program från Azure AD application gallery</span><span class="sxs-lookup"><span data-stu-id="7e20c-117">Password SSO applications from the Azure AD application gallery</span></span>
* <span data-ttu-id="7e20c-118">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="7e20c-118">Visual Studio Team Services</span></span>
* <span data-ttu-id="7e20c-119">Microsoft Teams</span><span class="sxs-lookup"><span data-stu-id="7e20c-119">Microsoft Teams</span></span>









## <a name="enable-access-rules"></a><span data-ttu-id="7e20c-120">Aktivera åtkomstregler</span><span class="sxs-lookup"><span data-stu-id="7e20c-120">Enable access rules</span></span>
<span data-ttu-id="7e20c-121">Varje regel kan aktiveras eller inaktiveras på en per program baser.</span><span class="sxs-lookup"><span data-stu-id="7e20c-121">Each rule can be enabled or disabled on a per application bases.</span></span> <span data-ttu-id="7e20c-122">När reglerna är **ON** ska vara aktiverad och framtvingas för användare med åtkomst till programmet.</span><span class="sxs-lookup"><span data-stu-id="7e20c-122">When rules are **ON** they will be enabled and enforced for users accessing the application.</span></span> <span data-ttu-id="7e20c-123">När de är **OFF** de används inte och kommer inte att påverka användare inloggningen.</span><span class="sxs-lookup"><span data-stu-id="7e20c-123">When they are **OFF** they will not be used and will not impact the users sign in experience.</span></span>

## <a name="applying-rules-to-specific-users"></a><span data-ttu-id="7e20c-124">Tillämpa regler för specifika användare</span><span class="sxs-lookup"><span data-stu-id="7e20c-124">Applying rules to specific users</span></span>
<span data-ttu-id="7e20c-125">Regler kan tillämpas på vissa uppsättningar användare baserat på säkerhetsgruppen genom att ange **tillämpa på**.</span><span class="sxs-lookup"><span data-stu-id="7e20c-125">Rules can be applied to specific sets of users based on security group by setting **Apply To**.</span></span> <span data-ttu-id="7e20c-126">**Tillämpa på** kan anges till **alla användare** eller **grupper**.</span><span class="sxs-lookup"><span data-stu-id="7e20c-126">**Apply To** can be set to **All Users** or **Groups**.</span></span> <span data-ttu-id="7e20c-127">Om värdet är **alla användare** regler gäller för alla användare med åtkomst till programmet.</span><span class="sxs-lookup"><span data-stu-id="7e20c-127">When set to **All Users** the rules will apply to any user with access to the application.</span></span> <span data-ttu-id="7e20c-128">Den **grupper** alternativet tillåter specifika säkerhets- och distributionsgrupper väljas, regler tillämpas endast för dessa grupper.</span><span class="sxs-lookup"><span data-stu-id="7e20c-128">The **Groups** option allows specific security and distribution groups to be selected, rules will only be enforced for these groups.</span></span>

<span data-ttu-id="7e20c-129">När du distribuerar en regel, är det vanligt att först använda den en begränsad uppsättning användare som är medlemmar i en pilot-grupper.</span><span class="sxs-lookup"><span data-stu-id="7e20c-129">When deploying a rule,  it is common to first apply it a limited set of users, that are members of a piloting groups.</span></span> <span data-ttu-id="7e20c-130">När du är klar regeln kan tillämpas på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7e20c-130">Once complete the rule can be applied to **All Users**.</span></span> <span data-ttu-id="7e20c-131">Detta innebär att regeln ska gälla för alla användare i organisationen.</span><span class="sxs-lookup"><span data-stu-id="7e20c-131">This will cause the rule to be enforced for all users in the organization.</span></span>

<span data-ttu-id="7e20c-132">Välj grupper kan också undantas från principen med den **utom** alternativet.</span><span class="sxs-lookup"><span data-stu-id="7e20c-132">Select groups may also be exempted from policy using the **Except** option.</span></span> <span data-ttu-id="7e20c-133">Alla medlemmar i dessa grupper kommer undantas även om de visas i en inkluderad grupp.</span><span class="sxs-lookup"><span data-stu-id="7e20c-133">Any members of these groups will be exempted even if they appear in an included group.</span></span>

## <a name="at-work-networks"></a><span data-ttu-id="7e20c-134">”På arbetet” nätverk</span><span class="sxs-lookup"><span data-stu-id="7e20c-134">“At work” networks</span></span>
<span data-ttu-id="7e20c-135">Regler för villkorlig åtkomst som använder en ”på arbetet”-nätverket som förlitar sig på tillförlitliga IP-adressintervall som har konfigurerats i Azure AD eller använda anspråkets ”i corpnet” från AD FS.</span><span class="sxs-lookup"><span data-stu-id="7e20c-135">Conditional access rules that use an “At work” network, rely on trusted IP address ranges that have been configured in Azure AD, or use of the "inside corpnet" claim from AD FS.</span></span> <span data-ttu-id="7e20c-136">Bestämmelserna omfattar:</span><span class="sxs-lookup"><span data-stu-id="7e20c-136">These rules include:</span></span>

* <span data-ttu-id="7e20c-137">Kräv Multi-Factor authentication när de inte är på arbetet</span><span class="sxs-lookup"><span data-stu-id="7e20c-137">Require multi-factor authentication when not at work</span></span>
* <span data-ttu-id="7e20c-138">Blockera åtkomst när de inte är på arbetet</span><span class="sxs-lookup"><span data-stu-id="7e20c-138">Block access when not at work</span></span>

<span data-ttu-id="7e20c-139">Alternativ för att ange ”på arbetet” nätverk</span><span class="sxs-lookup"><span data-stu-id="7e20c-139">Options for specifiying “at work” networks</span></span>

1. <span data-ttu-id="7e20c-140">Konfigurera betrodda IP-adressintervall i den [multifaktorautentisering konfigurationssidan](../multi-factor-authentication/multi-factor-authentication-whats-next.md).</span><span class="sxs-lookup"><span data-stu-id="7e20c-140">Configure trusted IP address ranges in the [multi-factor authentication configuration page](../multi-factor-authentication/multi-factor-authentication-whats-next.md).</span></span> <span data-ttu-id="7e20c-141">Princip för villkorlig åtkomst använder de konfigurerade intervallen för varje autentiseringsbegäran och utfärdande för att utvärdera regler.</span><span class="sxs-lookup"><span data-stu-id="7e20c-141">Conditional Access policy will use the configured ranges on each authentication request and token issuance to evaluate rules.</span></span> 
2. <span data-ttu-id="7e20c-142">Konfigurera användning av inre corpnet begära det här alternativet kan användas med externa kataloger, med hjälp av AD FS.</span><span class="sxs-lookup"><span data-stu-id="7e20c-142">Configure use of the inside corpnet claim, this option can be used with federated directories, using AD FS.</span></span> <span data-ttu-id="7e20c-143">Mer information om inre corpnet anspråk, se [Tusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span><span class="sxs-lookup"><span data-stu-id="7e20c-143">To learn more about the inside corpnet claims, see [Tusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span></span>


## <a name="rules-based-on-application-sensitivity"></a><span data-ttu-id="7e20c-144">Regler baserat på program känslighet</span><span class="sxs-lookup"><span data-stu-id="7e20c-144">Rules based on application sensitivity</span></span>
<span data-ttu-id="7e20c-145">Regler är konfigurerade per program så att tjänsterna för högt värde som ska skyddas utan att påverka åtkomst till andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="7e20c-145">Rules are configured per application allowing the high value services to be secured without impacting access to other services.</span></span> <span data-ttu-id="7e20c-146">Regler för villkorlig åtkomst kan konfigureras på den **konfigurera** för programmet.</span><span class="sxs-lookup"><span data-stu-id="7e20c-146">Conditional access rules can be configured on the  **Configure** tab of the application.</span></span> 

<span data-ttu-id="7e20c-147">Regler som för närvarande finns:</span><span class="sxs-lookup"><span data-stu-id="7e20c-147">Rules currently offered:</span></span>

* <span data-ttu-id="7e20c-148">**Kräv Multi-Factor authentication**</span><span class="sxs-lookup"><span data-stu-id="7e20c-148">**Require multi-factor authentication**</span></span>
  
  * <span data-ttu-id="7e20c-149">Alla användare som principen tillämpas på måste utföras för att autentisera via multifaktorautentisering minst en gång.</span><span class="sxs-lookup"><span data-stu-id="7e20c-149">All users that this policy is applied to will be required to authenticate via multi-factor authentication at least once.</span></span>
* <span data-ttu-id="7e20c-150">**Kräv Multi-Factor authentication när de inte är på arbetet**</span><span class="sxs-lookup"><span data-stu-id="7e20c-150">**Require multi-factor authentication when not at work**</span></span>
  
  * <span data-ttu-id="7e20c-151">Om principen gäller kommer alla användare att behöva har utfört multifaktorautentisering minst en gång om de har åtkomst till tjänsten från en annan plats.</span><span class="sxs-lookup"><span data-stu-id="7e20c-151">If this policy is applied, all users will be required to have performed multi-factor authentication at least once if they access the service from a non-work remote location.</span></span> <span data-ttu-id="7e20c-152">Om de flyttar från ett verk till fjärrplatsen behöver de utföra multifaktorautentisering vid åtkomst till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7e20c-152">If they move from a work to remote location, they will be required to perform multifactor authentication when accessing the service.</span></span>
* <span data-ttu-id="7e20c-153">**Blockera åtkomst när de inte är på arbetet**</span><span class="sxs-lookup"><span data-stu-id="7e20c-153">**Block access when not at work**</span></span> 
  
  * <span data-ttu-id="7e20c-154">När användarna flyttar från arbetet till en fjärrplats, blockeras de om ”blockera åtkomst när de inte är på arbetet”-principen gäller för dem.</span><span class="sxs-lookup"><span data-stu-id="7e20c-154">When users move from work to a remote location, they will be blocked if the "Block access when not at work" policy is applied to them.</span></span>  <span data-ttu-id="7e20c-155">De tillåts nytt åtkomst när de är på en plats för arbetet.</span><span class="sxs-lookup"><span data-stu-id="7e20c-155">They will be re-allowed access when at a work location.</span></span>

## <a name="related-topics"></a><span data-ttu-id="7e20c-156">Relaterade ämnen</span><span class="sxs-lookup"><span data-stu-id="7e20c-156">Related topics</span></span>
* [<span data-ttu-id="7e20c-157">Skydda åtkomst till Office 365 och andra appar ansluten till Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7e20c-157">Securing access to Office 365 and other apps connected to Azure Active Directory</span></span>](active-directory-conditional-access.md)
* [<span data-ttu-id="7e20c-158">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7e20c-158">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)

