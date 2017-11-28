---
title: "aaaSecuring privilegierad åtkomst i Azure AD | Microsoft Docs"
description: "Ett avsnitt som förklarar hello närmar sig för att skydda privilegierad åtkomst i Azure, Azure Active Directory och Microsoft Online Services."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: mwahl
ms.assetid: 235a0ce9-1daf-4433-8f65-9c6afcd64d08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: kgremban
ms.custom: pim
ms.openlocfilehash: 694835dc5c41640673dbd996d44b0d1f217220de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securing-privileged-access-in-azure-ad"></a><span data-ttu-id="06a45-103">Skydda privilegierad åtkomst i Azure AD</span><span class="sxs-lookup"><span data-stu-id="06a45-103">Securing privileged access in Azure AD</span></span>
<span data-ttu-id="06a45-104">Skydda privilegierad åtkomst är ett viktigt första steg toohelp skydda företagets tillgångar i en modern organisation.</span><span class="sxs-lookup"><span data-stu-id="06a45-104">Securing privileged access is a critical first step toohelp protect business assets in a modern organization.</span></span> <span data-ttu-id="06a45-105">Privilegierade konton är konton som administrera och hantera IT-system.</span><span class="sxs-lookup"><span data-stu-id="06a45-105">Privileged accounts are accounts that administer and manage IT systems.</span></span> <span data-ttu-id="06a45-106">Cyber angripare rikta dessa konton toogain åtkomst tooan organisationens data och datorer.</span><span class="sxs-lookup"><span data-stu-id="06a45-106">Cyber-attackers target these accounts toogain access tooan organization’s data and systems.</span></span> <span data-ttu-id="06a45-107">toosecure privilegierad åtkomst, bör du isolera hello konton och system från hello risken att exponeras tooa angripare.</span><span class="sxs-lookup"><span data-stu-id="06a45-107">toosecure privileged access, you should isolate hello accounts and systems from hello risk of being exposed tooa malicious user.</span></span>

<span data-ttu-id="06a45-108">Fler användare har börjat tooget privilegierad åtkomst via molntjänster.</span><span class="sxs-lookup"><span data-stu-id="06a45-108">More users are starting tooget privileged access through cloud services.</span></span> <span data-ttu-id="06a45-109">Detta kan inkludera globala administratörer för Office 365, Azure-prenumerationsadministratörer och användare som har administratörsbehörighet i virtuella datorer eller på SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="06a45-109">This can include global administrators of Office365, Azure subscription administrators, and users who have administrative access in VMs or on SaaS apps.</span></span>

<span data-ttu-id="06a45-110">Microsoft rekommenderar att du följer den här vägledningen [skydda privilegierad åtkomst](https://technet.microsoft.com/library/mt631194.aspx).</span><span class="sxs-lookup"><span data-stu-id="06a45-110">Microsoft recommends you follow this roadmap on [Securing Privileged Access](https://technet.microsoft.com/library/mt631194.aspx).</span></span>

<span data-ttu-id="06a45-111">För kunder som använder Azure Active Directory, Office 365 eller andra Microsoft-tjänster och program, tillämpa dessa principer om användarkonton hanteras och har autentiserats av Active Directory eller i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="06a45-111">For customers using Azure Active Directory, Office 365, or other Microsoft services and applications, these principles apply whether user accounts are managed and authenticated by Active Directory or in Azure Active Directory.</span></span> <span data-ttu-id="06a45-112">hello följande avsnitt innehåller mer information om Azure AD-funktioner toosupport skydda privilegierad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="06a45-112">hello following sections provide more information on Azure AD features toosupport securing privileged access.</span></span>

## <a name="azure-multi-factor-authentication"></a><span data-ttu-id="06a45-113">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="06a45-113">Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="06a45-114">Du bör kräva tvåstegsverifiering innan du beviljar behörighet tooincrease hello säkerhet för autentisering av administratörer.</span><span class="sxs-lookup"><span data-stu-id="06a45-114">tooincrease hello security of administrator authentication, you should require two-step verification before granting privileges.</span></span> <span data-ttu-id="06a45-115">Tvåstegsverifiering är en metod för att verifiera vem du är som kräver hello använder mer än bara ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="06a45-115">Two-step verification is a method of verifying who you are that requires hello use of more than just a username and password.</span></span> <span data-ttu-id="06a45-116">Det ger ett andra säkerhetslager säkerhet toouser inloggningar och transaktioner.</span><span class="sxs-lookup"><span data-stu-id="06a45-116">It provides a second layer of security toouser sign-ins and transactions.</span></span>

<span data-ttu-id="06a45-117">Azure Multi-Factor Authentication (MFA) är Microsofts tvåstegsverifiering verifiering lösning som hjälper dig att skydda åtkomst toodata och program och uppfyller efterfrågan från användarna för en process för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="06a45-117">Azure Multi-Factor Authentication (MFA) is Microsoft's two-step verification solution, which helps safeguard access toodata and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="06a45-118">Den ger stark autentisering via en mängd alternativ för enkel verifiering inklusive:</span><span class="sxs-lookup"><span data-stu-id="06a45-118">It delivers strong authentication via a range of easy verification options including:</span></span>

- <span data-ttu-id="06a45-119">telefonsamtal</span><span class="sxs-lookup"><span data-stu-id="06a45-119">phone calls</span></span>
- <span data-ttu-id="06a45-120">Textmeddelanden</span><span class="sxs-lookup"><span data-stu-id="06a45-120">text messages</span></span>
- <span data-ttu-id="06a45-121">mobilapp-meddelanden</span><span class="sxs-lookup"><span data-stu-id="06a45-121">mobile app notifications</span></span>
- <span data-ttu-id="06a45-122">koder för mobilappar</span><span class="sxs-lookup"><span data-stu-id="06a45-122">mobile app verification codes</span></span>
- <span data-ttu-id="06a45-123">OATH-token från tredje part</span><span class="sxs-lookup"><span data-stu-id="06a45-123">third-party OATH tokens</span></span>

<span data-ttu-id="06a45-124">En översikt över hur Azure Multi-Factor Authentication fungerar finns i följande video hello:</span><span class="sxs-lookup"><span data-stu-id="06a45-124">For an overview of how Azure Multi-Factor Authentication works see hello following video:</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]

<span data-ttu-id="06a45-125">Mer information finns i [MFA för Office 365 och Azure MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).</span><span class="sxs-lookup"><span data-stu-id="06a45-125">For more information, see [MFA for Office 365 and MFA for Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).</span></span>

## <a name="time-bound-privileges"></a><span data-ttu-id="06a45-126">Tidsbundna privilegier</span><span class="sxs-lookup"><span data-stu-id="06a45-126">Time-bound privileges</span></span>
<span data-ttu-id="06a45-127">Vissa organisationer kan hitta de har för många användare i mycket Privilegierade roller.</span><span class="sxs-lookup"><span data-stu-id="06a45-127">Some organizations may find that they have too many users in highly privileged roles.</span></span> <span data-ttu-id="06a45-128">En användare kan ha lagts toohello roll för en viss aktivitet som toosign dig för en tjänst, men använder inte dessa behörigheter ofta efteråt.</span><span class="sxs-lookup"><span data-stu-id="06a45-128">A user might have been added toohello role for a particular activity, like toosign up for a service, but didn't use those privileges frequently afterward.</span></span>

<span data-ttu-id="06a45-129">toolower hello tidsperioden privilegier och öka din insyn i deras användning, begränsa användare tooonly tar på sina privilegier just-in-time ”(JIT) när de behöver tooperform en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="06a45-129">toolower hello exposure time of privileges and increase your visibility into their use, limit users tooonly taking on their privileges "just in time" (JIT), when they need tooperform a task.</span></span> <span data-ttu-id="06a45-130">Du kan använda för Azure Active Directory och Microsoft Online Services [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).</span><span class="sxs-lookup"><span data-stu-id="06a45-130">For Azure Active Directory and Microsoft Online Services, you can use [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).</span></span>

![PIM-instrumentpanelen][2]

## <a name="attack-detection"></a><span data-ttu-id="06a45-132">Angreppsidentifiering</span><span class="sxs-lookup"><span data-stu-id="06a45-132">Attack detection</span></span>
<span data-ttu-id="06a45-133">[Azure Active Directory-identitetsskydd](../active-directory-identityprotection.md) ger en samlad vy riskhändelser och potentiella säkerhetsproblem som påverkar din organisations identiteter.</span><span class="sxs-lookup"><span data-stu-id="06a45-133">[Azure Active Directory Identity Protection](../active-directory-identityprotection.md) provides a consolidated view into risk events and potential vulnerabilities affecting your organization’s identities.</span></span> <span data-ttu-id="06a45-134">Baserat på riskhändelser identitetsskydd beräknar en användare risknivå för varje användare, vilket gör att du tooconfigure risk-baserade policys tooautomatically skydda hello identiteter i din organisation.</span><span class="sxs-lookup"><span data-stu-id="06a45-134">Based on risk events, Identity Protection calculates a user risk level for each user, enabling you tooconfigure risk-based policies tooautomatically protect hello identities of your organization.</span></span> <span data-ttu-id="06a45-135">Dessa principer tillsammans med andra kontroller för villkorlig åtkomst som tillhandahålls av Azure Active Directory och EMS, kan automatiskt blockerar hello användare eller erbjuda förslag som inkluderar lösenordsåterställning och multifaktorautentisering tvingande.</span><span class="sxs-lookup"><span data-stu-id="06a45-135">These policies, along with other conditional access controls provided by Azure Active Directory and EMS, can automatically block hello user or offer suggestions that include password resets and multi-factor authentication enforcement.</span></span>

![Azure AD Identity Protection][3]

## <a name="conditional-access"></a><span data-ttu-id="06a45-137">Villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="06a45-137">Conditional access</span></span>
<span data-ttu-id="06a45-138">Med villkorlig åtkomstkontroll kontrollerar hello särskilda villkor som du väljer när en användare autentiseras innan åtkomst tooan program i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="06a45-138">With conditional access control, Azure Active Directory checks hello specific conditions you choose when authenticating a user, before allowing access tooan application.</span></span> <span data-ttu-id="06a45-139">När dessa villkor är uppfyllda, hello användare autentiseras och tillåtet åtkomst toohello program.</span><span class="sxs-lookup"><span data-stu-id="06a45-139">Once those conditions are met, hello user is authenticated and allowed access toohello application.</span></span>

![Ange regler för villkorlig åtkomst med MFA][4]

## <a name="related-articles"></a><span data-ttu-id="06a45-141">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="06a45-141">Related articles</span></span>
* <span data-ttu-id="06a45-142">Aktivera [Azure Multi-Factor Authentication](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)</span><span class="sxs-lookup"><span data-stu-id="06a45-142">Enable [Azure Multi-Factor Authentication](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)</span></span>
* <span data-ttu-id="06a45-143">Aktivera [Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)</span><span class="sxs-lookup"><span data-stu-id="06a45-143">Enable [Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)</span></span>
* <span data-ttu-id="06a45-144">Aktivera [Azure AD Identity Protection](../active-directory-identityprotection.md)</span><span class="sxs-lookup"><span data-stu-id="06a45-144">Enable [Azure AD Identity Protection](../active-directory-identityprotection.md)</span></span>
* <span data-ttu-id="06a45-145">Aktivera [villkorlig åtkomstkontroll](../active-directory-conditional-access.md)</span><span class="sxs-lookup"><span data-stu-id="06a45-145">Enable [conditional access controls](../active-directory-conditional-access.md)</span></span>

<span data-ttu-id="06a45-146">Mer information om hur du skapar en plan för fullständig säkerhet avsnittet hello ”kundens ansvarsområden och översikt över” i hello [Microsoft Cloud Security för Enterprise-arkitekter](http://aka.ms/securecustomer) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="06a45-146">For more information on building a complete security roadmap, see hello “Customer responsibilities and roadmap” section of hello [Microsoft Cloud Security for Enterprise Architects](http://aka.ms/securecustomer) document.</span></span> <span data-ttu-id="06a45-147">Mer information om att engagera er tooassist för Microsoft-tjänster med något av dessa avsnitt, kontakta din Microsoft-representant eller besök vår [Cybersecurity lösningar sidan](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).</span><span class="sxs-lookup"><span data-stu-id="06a45-147">For more information on engaging Microsoft services tooassist with any of these topics, contact your Microsoft representative or visit our [Cybersecurity solutions page](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).</span></span>

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
