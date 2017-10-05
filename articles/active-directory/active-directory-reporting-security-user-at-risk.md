---
title: "Användare som flaggats i säkerhetsrapporten i Azure Active Directory | Microsoft Docs"
description: "Lär dig mer om användare som flaggats i säkerhetsrapporten i Azure Active Directory-portalen"
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
ms.openlocfilehash: 04f15384a7cd0fa03300acdf159d371569ecf9fc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="users-flagged-for-risk-security-report-in-the-azure-active-directory-portal"></a><span data-ttu-id="dc68d-103">Användare som flaggats i säkerhetsrapporten i Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="dc68d-103">Users flagged for risk security report in the Azure Active Directory portal</span></span>

<span data-ttu-id="dc68d-104">Med hjälp av säkerhetsrapporterna i Azure Active Directory (Azure AD) kan du bedöma risken för att användarkonton i din miljö har komprometterats.</span><span class="sxs-lookup"><span data-stu-id="dc68d-104">With the security reports in the Azure Active Directory (Azure AD), you can gain insights into the probability of compromised user accounts in your environment.</span></span> 

<span data-ttu-id="dc68d-105">Azure Active Directory identifierar misstänkta åtgärder relaterade till dina användarkonton.</span><span class="sxs-lookup"><span data-stu-id="dc68d-105">Azure Active Directory detects suspicious actions that are related to your user accounts.</span></span> <span data-ttu-id="dc68d-106">För varje identifierad åtgärd skapas en post med namnet *riskhändelse*.</span><span class="sxs-lookup"><span data-stu-id="dc68d-106">For each detected action, a record called *risk event* is created.</span></span> <span data-ttu-id="dc68d-107">Mer information finns i avsnittet om [Azure Active Directory-riskhändelser](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="dc68d-107">For more information, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span> 

<span data-ttu-id="dc68d-108">De identifierade riskhändelserna används för att beräkna:</span><span class="sxs-lookup"><span data-stu-id="dc68d-108">The detected risk events are used to calculate:</span></span>

- <span data-ttu-id="dc68d-109">**Riskfyllda inloggningar** – En riskfylld inloggning indikerar ett potentiellt inloggningsförsök av någon annan än användarkontots ägare.</span><span class="sxs-lookup"><span data-stu-id="dc68d-109">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not the legitimate owner of a user account.</span></span> <span data-ttu-id="dc68d-110">Mer information finns i avsnittet om [riskfyllda inloggningar](active-directory-identityprotection.md#risky-sign-ins).</span><span class="sxs-lookup"><span data-stu-id="dc68d-110">For more information, see [Risky sign-ins](active-directory-identityprotection.md#risky-sign-ins).</span></span> 

- <span data-ttu-id="dc68d-111">**Användare som har flaggats för risk** – En användare som har flaggats för risk indikerar att ett användarkonto kan ha komprometterats.</span><span class="sxs-lookup"><span data-stu-id="dc68d-111">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="dc68d-112">Mer information finns i avsnittet om [användare som har flaggats för risk](active-directory-identityprotection.md#users-flagged-for-risk).</span><span class="sxs-lookup"><span data-stu-id="dc68d-112">For more information, see [Users flagged for risk](active-directory-identityprotection.md#users-flagged-for-risk).</span></span>  

<span data-ttu-id="dc68d-113">I Azure-portalen hittar du säkerhetsrapporter på bladet **Azure Active Directory** i avsnittet **Säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="dc68d-113">In the Azure portal, you can find the security reports on the **Azure Active Directory** blade in the **Security** section.</span></span>  

![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/10.png)



## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a><span data-ttu-id="dc68d-115">Vilken Azure AD-licens behöver du för att komma åt en säkerhetsrapport?</span><span class="sxs-lookup"><span data-stu-id="dc68d-115">What Azure AD license do you need to access a security report?</span></span>  

<span data-ttu-id="dc68d-116">Alla utgåvor av Azure Active Directory ger rapporter över användare som har flaggats för risk.</span><span class="sxs-lookup"><span data-stu-id="dc68d-116">All editions of Azure Active Directory provide you with users flagged for risk reports.</span></span>  
<span data-ttu-id="dc68d-117">Nivån av rapportens detaljrikedom varierar dock mellan versionerna:</span><span class="sxs-lookup"><span data-stu-id="dc68d-117">However, the level of report granularity varies between the editions:</span></span> 

- <span data-ttu-id="dc68d-118">I **versionerna Azure Active Directory Free och Basic** finns redan en lista över användare som har flaggats för risk.</span><span class="sxs-lookup"><span data-stu-id="dc68d-118">In the **Azure Active Directory Free and Basic editions**, you already get a list of users flagged for risk.</span></span> 

- <span data-ttu-id="dc68d-119">Utgåvan **Azure Active Directory Premium 1** har en utökad modell där du även kan utforska några av de underliggande riskhändelser som har identifierats för varje rapport.</span><span class="sxs-lookup"><span data-stu-id="dc68d-119">The **Azure Active Directory Premium 1** edition extends this model by also enabling you to examine some of the underlying risk events that have been detected for each report.</span></span> 

- <span data-ttu-id="dc68d-120">Utgåvan **Azure Active Directory Premium 2** ger den mest detaljerade informationen om alla underliggande riskhändelser och du kan konfigurera säkerhetsprinciper som automatiskt svarar på konfigurerade risknivåer.</span><span class="sxs-lookup"><span data-stu-id="dc68d-120">The **Azure Active Directory Premium 2** edition provides you with the most detailed information about all underlying risk events and enables you to configure security policies that automatically respond to configured risk levels.</span></span>



## <a name="azure-active-directory-free-and-basic-edition"></a><span data-ttu-id="dc68d-121">Azure Active Directory kostnadsfri och grundläggande utgåva</span><span class="sxs-lookup"><span data-stu-id="dc68d-121">Azure Active Directory free and basic edition</span></span>

<span data-ttu-id="dc68d-122">Rapporten om användare som flaggats för risk i den kostnadsfria och grundläggande versionen av Azure Active Directory tillhandahåller en lista över användarkonton som kan ha komprometterats.</span><span class="sxs-lookup"><span data-stu-id="dc68d-122">The users flagged for risk report in the Azure Active Directory free and basic editions provides you with a list of user accounts that may have been compromised.</span></span> 


![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/03.png)

<span data-ttu-id="dc68d-124">Om du klickar på en användare öppnas tillhörande blad med användardata.</span><span class="sxs-lookup"><span data-stu-id="dc68d-124">Selecting a user opens the related user data blade.</span></span>
<span data-ttu-id="dc68d-125">För användare i farozonen går du igenom användarens inloggningshistorik och återställer lösenordet om det behövs.</span><span class="sxs-lookup"><span data-stu-id="dc68d-125">For users that are at risk, you can review the user’s sign-in history and reset the password if necessary.</span></span>

![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/46.png)


<span data-ttu-id="dc68d-127">Dialogrutan tillhandahåller ett alternativ för att:</span><span class="sxs-lookup"><span data-stu-id="dc68d-127">This dialog provides you with an option to:</span></span>

- <span data-ttu-id="dc68d-128">Ladda ned rapport</span><span class="sxs-lookup"><span data-stu-id="dc68d-128">Download the report</span></span>

- <span data-ttu-id="dc68d-129">Söka efter användare</span><span class="sxs-lookup"><span data-stu-id="dc68d-129">Search users</span></span>

![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/16.png)


## <a name="azure-active-directory-premium-editions"></a><span data-ttu-id="dc68d-131">Azure Active Directory Premium-versioner</span><span class="sxs-lookup"><span data-stu-id="dc68d-131">Azure Active Directory premium editions</span></span>

<span data-ttu-id="dc68d-132">Rapporten om användare som flaggats för risk i Azure Active Directory Premium-versionerna innehåller följande:</span><span class="sxs-lookup"><span data-stu-id="dc68d-132">The users flagged for risk report in the Azure Active Directory premium editions provides you with:</span></span>

- <span data-ttu-id="dc68d-133">En [lista över användarkonton](active-directory-identityprotection.md#users-flagged-for-risk) som kan ha drabbats</span><span class="sxs-lookup"><span data-stu-id="dc68d-133">A [list of user accounts](active-directory-identityprotection.md#users-flagged-for-risk) that may have been compromised</span></span> 

- <span data-ttu-id="dc68d-134">Sammanställd information om de [riskhändelsetyper](active-directory-identity-protection-risk-events.md) som har identifierats</span><span class="sxs-lookup"><span data-stu-id="dc68d-134">Aggregated information about the [risk event types](active-directory-identity-protection-risk-events.md) that have been detected</span></span>

- <span data-ttu-id="dc68d-135">Ett alternativ för att ladda ned rapporten</span><span class="sxs-lookup"><span data-stu-id="dc68d-135">An option to download the report</span></span>

- <span data-ttu-id="dc68d-136">Ett alternativ för att konfigurera en [princip för att åtgärda användarrisker](active-directory-identityprotection.md#user-risk-security-policy)</span><span class="sxs-lookup"><span data-stu-id="dc68d-136">An option to configure a [user risk remediation policy](active-directory-identityprotection.md#user-risk-security-policy)</span></span>  


![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/71.png)

<span data-ttu-id="dc68d-138">När du väljer en användare får du en detaljerad rapportvy för den här användaren som du kan använda för att göra följande:</span><span class="sxs-lookup"><span data-stu-id="dc68d-138">When you select a user, you get a detailed report view for this user that enables you to:</span></span>

- <span data-ttu-id="dc68d-139">Öppna vyn Alla inloggningar</span><span class="sxs-lookup"><span data-stu-id="dc68d-139">Open the All sign-ins view</span></span>

- <span data-ttu-id="dc68d-140">Återställ användarens lösenord</span><span class="sxs-lookup"><span data-stu-id="dc68d-140">Reset the user's password</span></span>

- <span data-ttu-id="dc68d-141">Ignorera alla händelser</span><span class="sxs-lookup"><span data-stu-id="dc68d-141">Dismiss all events</span></span>

- <span data-ttu-id="dc68d-142">Undersök rapporterade riskhändelser för användaren.</span><span class="sxs-lookup"><span data-stu-id="dc68d-142">Investigate reported risk events for the user.</span></span> 


![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/324.png)


<span data-ttu-id="dc68d-144">Om du vill undersöka en riskhändelse, markerar du en på listan för att öppna bladet med **Information** om den riskhändelsen.</span><span class="sxs-lookup"><span data-stu-id="dc68d-144">To investigate a risk event, select one from the list to open the **Details** blade for this risk event.</span></span> <span data-ttu-id="dc68d-145">På bladet **Information** har du möjlighet att antingen [stänga en riskhändelse manuellt](active-directory-identityprotection.md#closing-risk-events-manually) eller återaktivera en manuellt stängd riskhändelse.</span><span class="sxs-lookup"><span data-stu-id="dc68d-145">On the **Details** blade, you have the option to either [manually close a risk event](active-directory-identityprotection.md#closing-risk-events-manually) or reactivate a manually closed risk event.</span></span> 


![Riskfyllda inloggningar](./media/active-directory-reporting-security-user-at-risk/325.png)



## <a name="next-steps"></a><span data-ttu-id="dc68d-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dc68d-147">Next steps</span></span>

- <span data-ttu-id="dc68d-148">Mer information om Azure Active Directory Identity Protection finns i [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="dc68d-148">For more information about Azure Active Directory Identity Protection, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>

