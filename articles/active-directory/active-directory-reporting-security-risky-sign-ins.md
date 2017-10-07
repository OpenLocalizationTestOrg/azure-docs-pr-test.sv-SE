---
title: aaaRisky inloggningar rapporten i hello Azure Active Directory-portalen | Microsoft Docs
description: "Lär dig mer om hello riskfyllda inloggningar rapporten i hello Azure Active Directory-portalen"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d8df5cafea6b38f3e364c24a6aff599abe088e88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="risky-sign-ins-report-in-hello-azure-active-directory-portal"></a><span data-ttu-id="da91d-103">Riskfyllda inloggningar rapporten i hello Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="da91d-103">Risky sign-ins report in hello Azure Active Directory portal</span></span>

<span data-ttu-id="da91d-104">Med hello säkerhetsrapporter i Azure Active Directory (Azure AD) kan du få insikter om hello sannolikheten för avslöjade användarkonton i din miljö.</span><span class="sxs-lookup"><span data-stu-id="da91d-104">With hello security reports in Azure Active Directory (Azure AD) you can gain insights into hello probability of compromised user accounts in your environment.</span></span> 

<span data-ttu-id="da91d-105">Azure AD identifierar misstänkt åtgärder som är relaterade tooyour användarkonton.</span><span class="sxs-lookup"><span data-stu-id="da91d-105">Azure AD detects suspicious actions that are related tooyour user accounts.</span></span> <span data-ttu-id="da91d-106">För varje identifierad åtgärd skapas en post med namnet *riskhändelse*.</span><span class="sxs-lookup"><span data-stu-id="da91d-106">For each detected action, a record called *risk event* is created.</span></span> <span data-ttu-id="da91d-107">Mer information finns i avsnittet om [Azure Active Directory-riskhändelser](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="da91d-107">For more details, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span> 

<span data-ttu-id="da91d-108">hello upptäckt riskhändelser finns används toocalculate:</span><span class="sxs-lookup"><span data-stu-id="da91d-108">hello detected risk events are used toocalculate:</span></span>

- <span data-ttu-id="da91d-109">**Riskfyllda inloggningar** -riskfyllda loggar in är en indikator för en inloggning försök som kan ha utförts av någon som inte är hello legitima ägare för ett användarkonto.</span><span class="sxs-lookup"><span data-stu-id="da91d-109">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> <span data-ttu-id="da91d-110">Mer information finns i avsnittet om [riskfyllda inloggningar](active-directory-identityprotection.md#risky-sign-ins).</span><span class="sxs-lookup"><span data-stu-id="da91d-110">For more details, see [Risky sign-ins](active-directory-identityprotection.md#risky-sign-ins).</span></span> 

- <span data-ttu-id="da91d-111">**Användare som har flaggats för risk** – En användare som har flaggats för risk indikerar att ett användarkonto kan ha komprometterats.</span><span class="sxs-lookup"><span data-stu-id="da91d-111">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="da91d-112">Mer information finns i avsnittet om [användare som har flaggats för risk](active-directory-identityprotection.md#users-flagged-for-risk).</span><span class="sxs-lookup"><span data-stu-id="da91d-112">For more details, see [Users flagged for risk](active-directory-identityprotection.md#users-flagged-for-risk).</span></span>  

<span data-ttu-id="da91d-113">I [hello Azure-portalen](https://portal.azure.com), du kan hitta hello säkerhet rapporter om hello **Azure Active Directory** bladet i hello **säkerhet** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="da91d-113">In [hello Azure portal](https://portal.azure.com), you can find hello security reports on hello **Azure Active Directory** blade in hello **Security** section.</span></span> 

![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/10.png)


## <a name="what-azure-ad-license-do-you-need-tooaccess-a-security-report"></a><span data-ttu-id="da91d-115">Vilka Azure AD-licens behöver du tooaccess en säkerhetsrapporten?</span><span class="sxs-lookup"><span data-stu-id="da91d-115">What Azure AD license do you need tooaccess a security report?</span></span>  

<span data-ttu-id="da91d-116">Alla utgåvor av Azure Active Directory ger rapporter över riskfyllda inloggningar.</span><span class="sxs-lookup"><span data-stu-id="da91d-116">All editions of Azure Active Directory provide you with risky sign-ins reports.</span></span>  
<span data-ttu-id="da91d-117">Hello nivå av rapportens granularitet varierar dock mellan hello versioner:</span><span class="sxs-lookup"><span data-stu-id="da91d-117">However, hello level of report granularity varies between hello editions:</span></span> 

- <span data-ttu-id="da91d-118">I hello **Azure Active Directory ledigt och grundläggande**, du redan ha en lista över riskfyllda inloggningar.</span><span class="sxs-lookup"><span data-stu-id="da91d-118">In hello **Azure Active Directory Free and Basic editions**, you already get a list of risky sign-ins.</span></span> 

- <span data-ttu-id="da91d-119">Hej **Azure Active Directory Premium 1** edition utökar den här modellen genom att du även tooexamine vissa hello underliggande riskhändelser som har identifierats för varje rapport.</span><span class="sxs-lookup"><span data-stu-id="da91d-119">hello **Azure Active Directory Premium 1** edition extends this model by also enabling you tooexamine some of hello underlying risk events that have been detected for each report.</span></span> 

- <span data-ttu-id="da91d-120">Hej **Azure Active Directory Premium 2** versionen ger dig med hello mest detaljerad information om alla underliggande riskhändelser och du kan också tooconfigure säkerhetsprinciper som automatiskt svarar tooconfigured risknivåer.</span><span class="sxs-lookup"><span data-stu-id="da91d-120">hello **Azure Active Directory Premium 2** edition provides you with hello most detailed information about all underlying risk events and it also enables you tooconfigure security policies that automatically respond tooconfigured risk levels.</span></span>



## <a name="azure-active-directory-free-and-basic-edition"></a><span data-ttu-id="da91d-121">Azure Active Directory kostnadsfri och grundläggande utgåva</span><span class="sxs-lookup"><span data-stu-id="da91d-121">Azure Active Directory free and basic edition</span></span>

<span data-ttu-id="da91d-122">hello Azure Active Directory ledigt och Basic ger dig en lista över riskfyllda inloggningar som har identifierats för dina användare.</span><span class="sxs-lookup"><span data-stu-id="da91d-122">hello Azure Active Directory free and basic editions provide you with a list of risky sign-ins that have been detected for your users.</span></span> <span data-ttu-id="da91d-123">Den här rapporten innehåller:</span><span class="sxs-lookup"><span data-stu-id="da91d-123">This report lists:</span></span>

- <span data-ttu-id="da91d-124">**Användaren** - hello namn på hello-användaren som användes under hello logga in igen</span><span class="sxs-lookup"><span data-stu-id="da91d-124">**User** - hello name of hello user that was used during hello sign-in operation</span></span>
- <span data-ttu-id="da91d-125">**IP** -hello IP-adressen för hello-enhet som tidigare har använt tooconnect tooAzure Active Directory</span><span class="sxs-lookup"><span data-stu-id="da91d-125">**IP** - hello IP address of hello device that was used tooconnect tooAzure Active Directory</span></span>
- <span data-ttu-id="da91d-126">**Plats** -hello plats används tooconnect tooAzure Active Directory</span><span class="sxs-lookup"><span data-stu-id="da91d-126">**Location** - hello location used tooconnect tooAzure Active Directory</span></span>
- <span data-ttu-id="da91d-127">**Inloggningstid** -hello tid när hello-inloggning har utförts</span><span class="sxs-lookup"><span data-stu-id="da91d-127">**Sign-in time** - hello time when hello sign-in was performed</span></span>
- <span data-ttu-id="da91d-128">**Status för** -hello status för hello-inloggning</span><span class="sxs-lookup"><span data-stu-id="da91d-128">**Status** - hello status of hello sign-in</span></span>


![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/01.png)

<span data-ttu-id="da91d-130">Du kan ge feedback tooAzure Active Directory i form av hello följande åtgärder baserat på din undersökning av hello riskfyllda inloggning:</span><span class="sxs-lookup"><span data-stu-id="da91d-130">Based on your investigation of hello risky sign-in, you can provide feedback tooAzure Active Directory in form of hello following actions:</span></span>

- <span data-ttu-id="da91d-131">Lös</span><span class="sxs-lookup"><span data-stu-id="da91d-131">Resolve</span></span>
- <span data-ttu-id="da91d-132">Markera som falskt positivt resultat</span><span class="sxs-lookup"><span data-stu-id="da91d-132">Mark as false positive</span></span>
- <span data-ttu-id="da91d-133">Ignorera</span><span class="sxs-lookup"><span data-stu-id="da91d-133">Ignore</span></span>
- <span data-ttu-id="da91d-134">Återaktivera</span><span class="sxs-lookup"><span data-stu-id="da91d-134">Reactivate</span></span>

![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/21.png)

<span data-ttu-id="da91d-136">Mer information finns i [Stänga riskhändelser manuellt](active-directory-identityprotection.md#closing-risk-events-manually).</span><span class="sxs-lookup"><span data-stu-id="da91d-136">For more details, see [Closing risk events manually](active-directory-identityprotection.md#closing-risk-events-manually).</span></span>

<span data-ttu-id="da91d-137">Rapporten tillhandahåller ett alternativ för att:</span><span class="sxs-lookup"><span data-stu-id="da91d-137">This report provides you with an option to:</span></span>

- <span data-ttu-id="da91d-138">Söka resurser</span><span class="sxs-lookup"><span data-stu-id="da91d-138">Searh resources</span></span>
- <span data-ttu-id="da91d-139">Hämta hello rapportdata</span><span class="sxs-lookup"><span data-stu-id="da91d-139">Download hello report data</span></span>


![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/93.png)


## <a name="azure-active-directory-premium-editions"></a><span data-ttu-id="da91d-141">Azure Active Directory Premium-versioner</span><span class="sxs-lookup"><span data-stu-id="da91d-141">Azure Active Directory premium editions</span></span>

<span data-ttu-id="da91d-142">hello riskfyllda inloggningar rapporten i hello Azure Active Directory premium-versioner ger dig:</span><span class="sxs-lookup"><span data-stu-id="da91d-142">hello risky sign-ins report in hello Azure Active Directory premium editions provides you with:</span></span>

- <span data-ttu-id="da91d-143">Information om hello samman [riskerar händelsetyper](active-directory-identity-protection-risk-events.md) som har identifierats</span><span class="sxs-lookup"><span data-stu-id="da91d-143">Aggregated information about hello [risk event types](active-directory-identity-protection-risk-events.md) that have been detected</span></span>

- <span data-ttu-id="da91d-144">En alternativ toodownload hello-rapport</span><span class="sxs-lookup"><span data-stu-id="da91d-144">An option toodownload hello report</span></span>


![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/456.png)


<span data-ttu-id="da91d-146">När du väljer en riskhändelse kan få en detaljerad rapportvy för den här riskhändelsen som gör att du kan göra följande:</span><span class="sxs-lookup"><span data-stu-id="da91d-146">When you select a risk event, you get a detailed report view for this risk event that enables you to:</span></span>

- <span data-ttu-id="da91d-147">En alternativ tooconfigure en [användarprincip risk reparation](active-directory-identityprotection.md#user-risk-security-policy)</span><span class="sxs-lookup"><span data-stu-id="da91d-147">An option tooconfigure a [user risk remediation policy](active-directory-identityprotection.md#user-risk-security-policy)</span></span>  

- <span data-ttu-id="da91d-148">Granska hello identifiering tidslinje för hello risk händelse</span><span class="sxs-lookup"><span data-stu-id="da91d-148">Review hello detection timeline for hello risk event</span></span>  

- <span data-ttu-id="da91d-149">Granska en lista över användare för vilka den här riskhändelsen har upptäckts</span><span class="sxs-lookup"><span data-stu-id="da91d-149">Review a list of users for which this risk event has been detected</span></span>

- <span data-ttu-id="da91d-150">[Stänga riskhändelser manuellt](active-directory-identityprotection.md#closing-risk-events-manually) eller återaktivera en manuellt stängd riskhändelse.</span><span class="sxs-lookup"><span data-stu-id="da91d-150">[Manually close risk events](active-directory-identityprotection.md#closing-risk-events-manually) or reactivate a manually closed risk event.</span></span> 


![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/457.png)

<span data-ttu-id="da91d-152">När du väljer en användare får du en detaljerad rapportvy för den här användaren som du kan använda för att göra följande:</span><span class="sxs-lookup"><span data-stu-id="da91d-152">When you select a user, you get a detailed report view for this user that enables you to:</span></span>

- <span data-ttu-id="da91d-153">Öppna hello visa alla inloggningar</span><span class="sxs-lookup"><span data-stu-id="da91d-153">Open hello All sign-ins view</span></span>

- <span data-ttu-id="da91d-154">Återställa hello användares lösenord</span><span class="sxs-lookup"><span data-stu-id="da91d-154">Reset hello user's password</span></span>

- <span data-ttu-id="da91d-155">Ignorera alla händelser</span><span class="sxs-lookup"><span data-stu-id="da91d-155">Dismiss all events</span></span>

- <span data-ttu-id="da91d-156">Undersök rapporterade riskhändelser för hello användare.</span><span class="sxs-lookup"><span data-stu-id="da91d-156">Investigate reported risk events for hello user.</span></span> 


![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/324.png)


<span data-ttu-id="da91d-158">tooinvestigate en risk-händelse, välja en hello lista.</span><span class="sxs-lookup"><span data-stu-id="da91d-158">tooinvestigate a risk event, select one from hello list.</span></span>  
<span data-ttu-id="da91d-159">Då öppnas hello **information** bladet för den här händelsen för risk.</span><span class="sxs-lookup"><span data-stu-id="da91d-159">This opens hello **Details** blade for this risk event.</span></span> <span data-ttu-id="da91d-160">På hello **information** bladet du har hello alternativet tooeither [stänga manuellt en risk händelse](active-directory-identityprotection.md#closing-risk-events-manually) eller återaktivera en stängd manuellt risk-händelse.</span><span class="sxs-lookup"><span data-stu-id="da91d-160">On hello **Details** blade, you have hello option tooeither [manually close a risk event](active-directory-identityprotection.md#closing-risk-events-manually) or reactivate a manually closed risk event.</span></span> 


![Riskfyllda inloggningar](./media/active-directory-reporting-security-risky-sign-ins/325.png)





## <a name="next-steps"></a><span data-ttu-id="da91d-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="da91d-162">Next steps</span></span>

- <span data-ttu-id="da91d-163">Mer information om Azure Active Directory Identity Protection finns i [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="da91d-163">For more information about Azure Active Directory Identity Protection, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>

