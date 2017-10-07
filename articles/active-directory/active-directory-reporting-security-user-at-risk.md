---
title: "aaaUsers flaggats för risk säkerhetsrapporten hello Azure Active Directory-portalen | Microsoft Docs"
description: "Lär dig mer om hello användare som flaggats för risk säkerhetsrapporten hello Azure Active Directory-portalen"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5077cd61d6119745a85ed712623904633a151331
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="users-flagged-for-risk-security-report-in-hello-azure-active-directory-portal"></a><span data-ttu-id="e4ba6-103">Användare som har flaggats för risk säkerhetsrapporten hello Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="e4ba6-103">Users flagged for risk security report in hello Azure Active Directory portal</span></span>

<span data-ttu-id="e4ba6-104">Med hello säkerhetsrapporter i hello Azure Active Directory (Azure AD), kan du få insikter om hello sannolikheten för avslöjade användarkonton i din miljö.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-104">With hello security reports in hello Azure Active Directory (Azure AD), you can gain insights into hello probability of compromised user accounts in your environment.</span></span> 

<span data-ttu-id="e4ba6-105">Azure Active Directory identifierar misstänkt åtgärder som är relaterade tooyour användarkonton.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-105">Azure Active Directory detects suspicious actions that are related tooyour user accounts.</span></span> <span data-ttu-id="e4ba6-106">För varje identifierad åtgärd skapas en post med namnet *riskhändelse*.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-106">For each detected action, a record called *risk event* is created.</span></span> <span data-ttu-id="e4ba6-107">Mer information finns i avsnittet om [Azure Active Directory-riskhändelser](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="e4ba6-107">For more information, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span> 

<span data-ttu-id="e4ba6-108">hello upptäckt riskhändelser finns används toocalculate:</span><span class="sxs-lookup"><span data-stu-id="e4ba6-108">hello detected risk events are used toocalculate:</span></span>

- <span data-ttu-id="e4ba6-109">**Riskfyllda inloggningar** -riskfyllda loggar in är en indikator för en inloggning försök som kan ha utförts av någon som inte är hello legitima ägare för ett användarkonto.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-109">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> <span data-ttu-id="e4ba6-110">Mer information finns i avsnittet om [riskfyllda inloggningar](active-directory-identityprotection.md#risky-sign-ins).</span><span class="sxs-lookup"><span data-stu-id="e4ba6-110">For more information, see [Risky sign-ins](active-directory-identityprotection.md#risky-sign-ins).</span></span> 

- <span data-ttu-id="e4ba6-111">**Användare som har flaggats för risk** – En användare som har flaggats för risk indikerar att ett användarkonto kan ha komprometterats.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-111">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="e4ba6-112">Mer information finns i avsnittet om [användare som har flaggats för risk](active-directory-identityprotection.md#users-flagged-for-risk).</span><span class="sxs-lookup"><span data-stu-id="e4ba6-112">For more information, see [Users flagged for risk](active-directory-identityprotection.md#users-flagged-for-risk).</span></span>  

<span data-ttu-id="e4ba6-113">I hello Azure-portalen, du kan hitta hello säkerhet rapporter om hello **Azure Active Directory** bladet i hello **säkerhet** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-113">In hello Azure portal, you can find hello security reports on hello **Azure Active Directory** blade in hello **Security** section.</span></span>  

![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/10.png)



## <a name="what-azure-ad-license-do-you-need-tooaccess-a-security-report"></a><span data-ttu-id="e4ba6-115">Vilka Azure AD-licens behöver du tooaccess en säkerhetsrapporten?</span><span class="sxs-lookup"><span data-stu-id="e4ba6-115">What Azure AD license do you need tooaccess a security report?</span></span>  

<span data-ttu-id="e4ba6-116">Alla utgåvor av Azure Active Directory ger rapporter över användare som har flaggats för risk.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-116">All editions of Azure Active Directory provide you with users flagged for risk reports.</span></span>  
<span data-ttu-id="e4ba6-117">Hello nivå av rapportens granularitet varierar dock mellan hello versioner:</span><span class="sxs-lookup"><span data-stu-id="e4ba6-117">However, hello level of report granularity varies between hello editions:</span></span> 

- <span data-ttu-id="e4ba6-118">I hello **Azure Active Directory ledigt och grundläggande**, du redan ha en lista över användare som har flaggats för risk.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-118">In hello **Azure Active Directory Free and Basic editions**, you already get a list of users flagged for risk.</span></span> 

- <span data-ttu-id="e4ba6-119">Hej **Azure Active Directory Premium 1** edition utökar den här modellen genom att du även tooexamine vissa hello underliggande riskhändelser som har identifierats för varje rapport.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-119">hello **Azure Active Directory Premium 1** edition extends this model by also enabling you tooexamine some of hello underlying risk events that have been detected for each report.</span></span> 

- <span data-ttu-id="e4ba6-120">Hej **Azure Active Directory Premium 2** -versionen tillhandahåller hello mest detaljerad information om alla underliggande riskhändelser och gör att du tooconfigure säkerhetsprinciper som automatiskt svarar tooconfigured risk nivåer.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-120">hello **Azure Active Directory Premium 2** edition provides you with hello most detailed information about all underlying risk events and enables you tooconfigure security policies that automatically respond tooconfigured risk levels.</span></span>



## <a name="azure-active-directory-free-and-basic-edition"></a><span data-ttu-id="e4ba6-121">Azure Active Directory kostnadsfri och grundläggande utgåva</span><span class="sxs-lookup"><span data-stu-id="e4ba6-121">Azure Active Directory free and basic edition</span></span>

<span data-ttu-id="e4ba6-122">hello-användare som flaggats för risk rapporten i hello Azure Active Directory ledigt grundläggande utgåvor och ger dig en lista över användarkonton som komprometterats.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-122">hello users flagged for risk report in hello Azure Active Directory free and basic editions provides you with a list of user accounts that may have been compromised.</span></span> 


![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/03.png)

<span data-ttu-id="e4ba6-124">När du väljer en användare öppnar hello relaterade användaren data blad.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-124">Selecting a user opens hello related user data blade.</span></span>
<span data-ttu-id="e4ba6-125">Du kan granska hello användarens inloggning historik och återställa hello lösenord om det behövs för användare som är i fara.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-125">For users that are at risk, you can review hello user’s sign-in history and reset hello password if necessary.</span></span>

![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/46.png)


<span data-ttu-id="e4ba6-127">Dialogrutan tillhandahåller ett alternativ för att:</span><span class="sxs-lookup"><span data-stu-id="e4ba6-127">This dialog provides you with an option to:</span></span>

- <span data-ttu-id="e4ba6-128">Hämta hello-rapport</span><span class="sxs-lookup"><span data-stu-id="e4ba6-128">Download hello report</span></span>

- <span data-ttu-id="e4ba6-129">Söka efter användare</span><span class="sxs-lookup"><span data-stu-id="e4ba6-129">Search users</span></span>

![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/16.png)


## <a name="azure-active-directory-premium-editions"></a><span data-ttu-id="e4ba6-131">Azure Active Directory Premium-versioner</span><span class="sxs-lookup"><span data-stu-id="e4ba6-131">Azure Active Directory premium editions</span></span>

<span data-ttu-id="e4ba6-132">hello-användare som flaggats för risk rapporten i hello Azure Active Directory premium-versioner ger dig:</span><span class="sxs-lookup"><span data-stu-id="e4ba6-132">hello users flagged for risk report in hello Azure Active Directory premium editions provides you with:</span></span>

- <span data-ttu-id="e4ba6-133">En [lista över användarkonton](active-directory-identityprotection.md#users-flagged-for-risk) som kan ha drabbats</span><span class="sxs-lookup"><span data-stu-id="e4ba6-133">A [list of user accounts](active-directory-identityprotection.md#users-flagged-for-risk) that may have been compromised</span></span> 

- <span data-ttu-id="e4ba6-134">Information om hello samman [riskerar händelsetyper](active-directory-identity-protection-risk-events.md) som har identifierats</span><span class="sxs-lookup"><span data-stu-id="e4ba6-134">Aggregated information about hello [risk event types](active-directory-identity-protection-risk-events.md) that have been detected</span></span>

- <span data-ttu-id="e4ba6-135">En alternativ toodownload hello-rapport</span><span class="sxs-lookup"><span data-stu-id="e4ba6-135">An option toodownload hello report</span></span>

- <span data-ttu-id="e4ba6-136">En alternativ tooconfigure en [användarprincip risk reparation](active-directory-identityprotection.md#user-risk-security-policy)</span><span class="sxs-lookup"><span data-stu-id="e4ba6-136">An option tooconfigure a [user risk remediation policy](active-directory-identityprotection.md#user-risk-security-policy)</span></span>  


![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/71.png)

<span data-ttu-id="e4ba6-138">När du väljer en användare får du en detaljerad rapportvy för den här användaren som du kan använda för att göra följande:</span><span class="sxs-lookup"><span data-stu-id="e4ba6-138">When you select a user, you get a detailed report view for this user that enables you to:</span></span>

- <span data-ttu-id="e4ba6-139">Öppna hello visa alla inloggningar</span><span class="sxs-lookup"><span data-stu-id="e4ba6-139">Open hello All sign-ins view</span></span>

- <span data-ttu-id="e4ba6-140">Återställa hello användares lösenord</span><span class="sxs-lookup"><span data-stu-id="e4ba6-140">Reset hello user's password</span></span>

- <span data-ttu-id="e4ba6-141">Ignorera alla händelser</span><span class="sxs-lookup"><span data-stu-id="e4ba6-141">Dismiss all events</span></span>

- <span data-ttu-id="e4ba6-142">Undersök rapporterade riskhändelser för hello användare.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-142">Investigate reported risk events for hello user.</span></span> 


![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/324.png)


<span data-ttu-id="e4ba6-144">tooinvestigate en risk-händelse, välja en hello listan tooopen hello **information** bladet för den här händelsen för risk.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-144">tooinvestigate a risk event, select one from hello list tooopen hello **Details** blade for this risk event.</span></span> <span data-ttu-id="e4ba6-145">På hello **information** bladet du har hello alternativet tooeither [stänga manuellt en risk händelse](active-directory-identityprotection.md#closing-risk-events-manually) eller återaktivera en stängd manuellt risk-händelse.</span><span class="sxs-lookup"><span data-stu-id="e4ba6-145">On hello **Details** blade, you have hello option tooeither [manually close a risk event](active-directory-identityprotection.md#closing-risk-events-manually) or reactivate a manually closed risk event.</span></span> 


![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/325.png)



## <a name="next-steps"></a><span data-ttu-id="e4ba6-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e4ba6-147">Next steps</span></span>

- <span data-ttu-id="e4ba6-148">Mer information om Azure Active Directory Identity Protection finns i [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="e4ba6-148">For more information about Azure Active Directory Identity Protection, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>

