---
title: aaaAzure Active Directory reporting | Microsoft Docs
description: "En allmän översikt över Azure Active Directory-rapportering."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c91813acbdc4b0bfcd164169b0b575accac227d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting"></a><span data-ttu-id="53864-103">Azure Active Directory-rapportering</span><span class="sxs-lookup"><span data-stu-id="53864-103">Azure Active Directory reporting</span></span>

<span data-ttu-id="53864-104">Med Azure Active Directory-rapportering kan du få insikter om din miljös prestanda.</span><span class="sxs-lookup"><span data-stu-id="53864-104">With Azure Active Directory reporting, you can gain insights into how your environment is doing.</span></span>  
<span data-ttu-id="53864-105">hello som data kan du:</span><span class="sxs-lookup"><span data-stu-id="53864-105">hello provided data enables you to:</span></span>

- <span data-ttu-id="53864-106">Avgör hur din app och dina tjänster används av dina användare</span><span class="sxs-lookup"><span data-stu-id="53864-106">Determine how your apps and services are utilized by your users</span></span>
- <span data-ttu-id="53864-107">Identifiera potentiella risker som påverkar hello hälsotillståndet för din miljö</span><span class="sxs-lookup"><span data-stu-id="53864-107">Detect potential risks affecting hello health of your environment</span></span>
- <span data-ttu-id="53864-108">Felsöka problem som hindrar användare från att uträtta sitt arbete</span><span class="sxs-lookup"><span data-stu-id="53864-108">Troubleshoot issues preventing your users from getting their work done</span></span>  

<span data-ttu-id="53864-109">hello reporting arkitektur förlitar sig på två huvudsakliga pelare:</span><span class="sxs-lookup"><span data-stu-id="53864-109">hello reporting architecture relies on two main pillars:</span></span>

- <span data-ttu-id="53864-110">Säkerhetsrapporter</span><span class="sxs-lookup"><span data-stu-id="53864-110">Security reports</span></span>
- <span data-ttu-id="53864-111">Aktivitetsrapporter</span><span class="sxs-lookup"><span data-stu-id="53864-111">Activity reports</span></span>

![Rapportering](./media/active-directory-reporting-azure-portal/01.png)



## <a name="security-reports"></a><span data-ttu-id="53864-113">Säkerhetsrapporter</span><span class="sxs-lookup"><span data-stu-id="53864-113">Security reports</span></span>

<span data-ttu-id="53864-114">hello säkerhetsrapporter i Azure Active Directory kan du tooprotect din organisations identiteter.</span><span class="sxs-lookup"><span data-stu-id="53864-114">hello security reports in Azure Active Directory help you tooprotect your organization's identities.</span></span>  
<span data-ttu-id="53864-115">Det finns två typer av säkerhetsrapporter i Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="53864-115">There are two types of security reports in Azure Active Directory:</span></span>

- <span data-ttu-id="53864-116">**Användare som har flaggats för risk** - från hello [användare som har flaggats för risk säkerhetsrapporten](active-directory-reporting-security-user-at-risk.md), du få en översikt över användarkonton som kan ha drabbats.</span><span class="sxs-lookup"><span data-stu-id="53864-116">**Users flagged for risk** - From hello [users flagged for risk security report](active-directory-reporting-security-user-at-risk.md), you get an overview of user accounts that might have been compromised.</span></span>

- <span data-ttu-id="53864-117">**Riskfyllda inloggningar** – med hello [riskfyllda inloggning säkerhetsrapporten](active-directory-reporting-security-risky-sign-ins.md), du får en indikator för inloggningsförsök som kan ha utförts av någon som är inte hello legitima ägaren till ett användarkonto.</span><span class="sxs-lookup"><span data-stu-id="53864-117">**Risky sign-ins** - With hello [risky sign-in security report](active-directory-reporting-security-risky-sign-ins.md), you get an indicator for sign-in attempts that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> 

<span data-ttu-id="53864-118">**Vilka Azure AD-licens behöver du tooaccess en säkerhetsrapporten?**</span><span class="sxs-lookup"><span data-stu-id="53864-118">**What Azure AD license do you need tooaccess a security report?**</span></span>  
<span data-ttu-id="53864-119">Alla utgåvor av Azure Active Directory ger rapporter över användare som har flaggats för risk och riskfyllda inloggningar.</span><span class="sxs-lookup"><span data-stu-id="53864-119">All editions of Azure Active Directory provide you with users flagged for risk and risky sign-ins reports.</span></span>  
<span data-ttu-id="53864-120">Hello nivå av rapportens granularitet varierar dock mellan hello versioner:</span><span class="sxs-lookup"><span data-stu-id="53864-120">However, hello level of report granularity varies between hello editions:</span></span> 

- <span data-ttu-id="53864-121">I hello **Azure Active Directory ledigt och grundläggande**, du redan ha en lista över användare som har flaggats för risk och riskfyllda inloggningar.</span><span class="sxs-lookup"><span data-stu-id="53864-121">In hello **Azure Active Directory Free and Basic editions**, you already get a list of users flagged for risk and risky sign-ins.</span></span> 

- <span data-ttu-id="53864-122">Hej **Azure Active Directory Premium 1** edition utökar den här modellen genom att du även tooexamine vissa hello underliggande riskhändelser som har identifierats för varje rapport.</span><span class="sxs-lookup"><span data-stu-id="53864-122">hello **Azure Active Directory Premium 1** edition extends this model by also enabling you tooexamine some of hello underlying risk events that have been detected for each report.</span></span> 

- <span data-ttu-id="53864-123">Hej **Azure Active Directory Premium 2** versionen ger dig med hello mest detaljerad information om hello underliggande riskhändelser och du kan också tooconfigure säkerhetsprinciper som automatiskt svarar tooconfigured risknivåer.</span><span class="sxs-lookup"><span data-stu-id="53864-123">hello **Azure Active Directory Premium 2** edition provides you with hello most detailed information about hello underlying risk events and it also enables you tooconfigure security policies that automatically respond tooconfigured risk levels.</span></span>


## <a name="activity-reports"></a><span data-ttu-id="53864-124">Aktivitetsrapporter</span><span class="sxs-lookup"><span data-stu-id="53864-124">Activity reports</span></span>

<span data-ttu-id="53864-125">Det finns två typer av aktivitetsrapporter i Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="53864-125">There are two types of activity reports in Azure Active Directory:</span></span>

- <span data-ttu-id="53864-126">**Granskningsloggar** - hello [granskningsloggar aktivitetsrapport](active-directory-reporting-activity-audit-logs.md) ger dig åtkomst toohello historik för varje aktivitet i din klient.</span><span class="sxs-lookup"><span data-stu-id="53864-126">**Audit logs** - hello [audit logs activity report](active-directory-reporting-activity-audit-logs.md) provides you with access toohello history of every task performed in your tenant.</span></span>

- <span data-ttu-id="53864-127">**Inloggningar** – med hello [inloggningar aktivitetsrapport](active-directory-reporting-activity-sign-ins.md), du kan bestämma vem som har genomfört hello uppgifter som rapporterats av hello kontrollrapport loggar.</span><span class="sxs-lookup"><span data-stu-id="53864-127">**Sign-ins** -  With hello [sign-ins activity report](active-directory-reporting-activity-sign-ins.md), you can determine, who has performed hello tasks reported by hello audit logs report.</span></span>



<span data-ttu-id="53864-128">Hej **granskningsloggar rapporten** ger dig poster av systemaktiviteter för kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="53864-128">hello **audit logs report** provides you with records of system activities for compliance.</span></span>
<span data-ttu-id="53864-129">Bland annat tillhandahålls hello data kan du tooaddress vanliga scenarier som:</span><span class="sxs-lookup"><span data-stu-id="53864-129">Amongst others, hello provided data enables you tooaddress common scenarios such as:</span></span>

- <span data-ttu-id="53864-130">Någon i min klient fick åtkomst tooan administratörsgrupp.</span><span class="sxs-lookup"><span data-stu-id="53864-130">Someone in my tenant got access tooan admin group.</span></span> <span data-ttu-id="53864-131">Vem som gav användaren åtkomst?</span><span class="sxs-lookup"><span data-stu-id="53864-131">Who gave them access?</span></span> 

- <span data-ttu-id="53864-132">Jag vill tooknow hello lista över användare som loggar in på en viss app sedan jag nyligen publicerats så hello app och vill tooknow om det fungerar bra</span><span class="sxs-lookup"><span data-stu-id="53864-132">I want tooknow hello list of users signing into a specific app since I recently onboarded hello app and want tooknow if it’s doing well</span></span>

- <span data-ttu-id="53864-133">Jag vill tooknow hur många lösenord återställer sker i min klient</span><span class="sxs-lookup"><span data-stu-id="53864-133">I want tooknow how many password resets are happening in my tenant</span></span>


<span data-ttu-id="53864-134">**Vilka Azure AD-licens behöver du tooaccess hello granska loggarna rapporten?**</span><span class="sxs-lookup"><span data-stu-id="53864-134">**What Azure AD license do you need tooaccess hello audit logs report?**</span></span>  
<span data-ttu-id="53864-135">hello granska loggarna rapporten är tillgängligt för funktioner som du har licenser.</span><span class="sxs-lookup"><span data-stu-id="53864-135">hello audit logs report is available for features for which you have licenses.</span></span> <span data-ttu-id="53864-136">Om du har en licens för en specifik funktion kan ha du också åtkomst toohello granska informationen i felloggen för den.</span><span class="sxs-lookup"><span data-stu-id="53864-136">If you have a license for a specific feature, you also have access toohello audit log information for it.</span></span>

<span data-ttu-id="53864-137">Mer information finns i **jämföra allmänt tillgängliga funktioner i hello lediga, Basic eller Premium Edition** i [Azure Active Directory-funktioner som](https://www.microsoft.com/cloud-platform/azure-active-directory-features).</span><span class="sxs-lookup"><span data-stu-id="53864-137">For more details, see **Comparing generally available features of hello Free, Basic, and Premium editions** in [Azure Active Directory features and capabilities](https://www.microsoft.com/cloud-platform/azure-active-directory-features).</span></span>   



<span data-ttu-id="53864-138">Hej **inloggningar aktivitetsrapport** aktiverar tootoofind svar tooquestions som:</span><span class="sxs-lookup"><span data-stu-id="53864-138">hello **sign-ins activity report** enables tootoofind answers tooquestions such as:</span></span>

- <span data-ttu-id="53864-139">Vad är hello inloggning mönstret för en användare?</span><span class="sxs-lookup"><span data-stu-id="53864-139">What is hello sign-in pattern of a user?</span></span>
- <span data-ttu-id="53864-140">Hur många användare har en användare loggat in under en vecka?</span><span class="sxs-lookup"><span data-stu-id="53864-140">How many users have users signed in over a week?</span></span>
- <span data-ttu-id="53864-141">Vad är hello statusen för dessa inloggningar?</span><span class="sxs-lookup"><span data-stu-id="53864-141">What’s hello status of these sign-ins?</span></span>


<span data-ttu-id="53864-142">**Vilka Azure AD-licens gör du behöver tooaccess hello inloggningar Aktivitetsrapport?**</span><span class="sxs-lookup"><span data-stu-id="53864-142">**What Azure AD license do you need tooaccess hello sign-ins activity report?**</span></span>  
<span data-ttu-id="53864-143">tooaccess Hej inloggningar aktivitetsrapport, din klient måste ha en Azure AD Premium-licens som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="53864-143">tooaccess hello sign-ins activity report, your tenant must have an Azure AD Premium license associated with it.</span></span>


## <a name="programmatic-access"></a><span data-ttu-id="53864-144">Programmässig åtkomst</span><span class="sxs-lookup"><span data-stu-id="53864-144">Programmatic access</span></span>

<span data-ttu-id="53864-145">I användargränssnittet för tillägg toohello Azure Active Directory reporting ger dig också med [Programmeringsåtkomst](active-directory-reporting-api-getting-started-azure-portal.md) toohello rapportdata.</span><span class="sxs-lookup"><span data-stu-id="53864-145">In addition toohello user interface, Azure Active Directory reporting also provides you with [programmatic access](active-directory-reporting-api-getting-started-azure-portal.md) toohello reporting data.</span></span> <span data-ttu-id="53864-146">hello data för de här rapporterna kan vara användbar tooyour program, till exempel SIEM-system, granskning och business intelligence-verktyg.</span><span class="sxs-lookup"><span data-stu-id="53864-146">hello data of these reports can be very useful tooyour applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="53864-147">hello Azure AD reporting API: er ger Programmeringsåtkomst toohello data via en uppsättning REST-baserad API: er.</span><span class="sxs-lookup"><span data-stu-id="53864-147">hello Azure AD reporting APIs provide programmatic access toohello data through a set of REST-based APIs.</span></span> <span data-ttu-id="53864-148">Du kan anropa API: erna från en mängd olika programmeringsspråk och verktyg.</span><span class="sxs-lookup"><span data-stu-id="53864-148">You can call these APIs from a variety of programming languages and tools.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="53864-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53864-149">Next steps</span></span>

<span data-ttu-id="53864-150">Om du vill tooknow mer om hello olika rapporttyper i Azure Active Directory, se:</span><span class="sxs-lookup"><span data-stu-id="53864-150">If you want tooknow more about hello various report types in Azure Active Directory, see:</span></span>

- [<span data-ttu-id="53864-151">Användare som har flaggats för risk</span><span class="sxs-lookup"><span data-stu-id="53864-151">Users flagged for risk report</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="53864-152">Rapport över riskfyllda inloggningar</span><span class="sxs-lookup"><span data-stu-id="53864-152">Risky sign-ins report</span></span>](active-directory-reporting-security-risky-sign-ins.md)
- [<span data-ttu-id="53864-153">Granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="53864-153">Audit logs report</span></span>](active-directory-reporting-activity-audit-logs.md)
- [<span data-ttu-id="53864-154">Inloggningsrapport</span><span class="sxs-lookup"><span data-stu-id="53864-154">Sign-ins logs report</span></span>](active-directory-reporting-activity-sign-ins.md)

<span data-ttu-id="53864-155">Om du vill tooknow mer om att komma åt hello hello reporting data med hjälp av hello reporting API, se:</span><span class="sxs-lookup"><span data-stu-id="53864-155">If you want tooknow more about accessing hello hello reporting data using hello reporting API, see:</span></span> 

- [<span data-ttu-id="53864-156">Komma igång med hello Azure Active Directory reporting API</span><span class="sxs-lookup"><span data-stu-id="53864-156">Getting started with hello Azure Active Directory reporting API</span></span>](active-directory-reporting-api-getting-started-azure-portal.md)


<!--Image references-->
[1]: ./media/active-directory-reporting-azure-portal/ic195031.png