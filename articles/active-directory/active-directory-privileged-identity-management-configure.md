---
title: aaaConfigure Azure AD Privileged Identity Management | Microsoft Docs
description: "Ett avsnitt som förklarar vad Azure AD Privileged Identity Management är och hur toouse PIM tooimprove moln-säkerhet."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: c548ed2e-06e3-4eaf-a63d-0f02ee72da25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: dbe49fe4a0f6e5b46ed5a17fc7e8dcdacafe3846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-ad-privileged-identity-management"></a><span data-ttu-id="c1ef4-103">Vad är Azure AD Privileged Identity Management?</span><span class="sxs-lookup"><span data-stu-id="c1ef4-103">What is Azure AD Privileged Identity Management?</span></span>
<span data-ttu-id="c1ef4-104">Med Azure Active Directory (AD) Privileged Identity Management kan du hantera, kontrollera och övervaka åtkomst inom din organisation.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-104">With Azure Active Directory (AD) Privileged Identity Management, you can manage, control, and monitor access within your organization.</span></span> <span data-ttu-id="c1ef4-105">Detta inkluderar åtkomst tooresources i Azure AD och andra Microsoft online services som Office 365 eller Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-105">This includes access tooresources in Azure AD and other Microsoft online services like Office 365 or Microsoft Intune.</span></span>  

> [!NOTE]
> <span data-ttu-id="c1ef4-106">Privileged Identity Management är tillgängliga tooyour hela organisationen när du licensierar dina administratörer med hello Premium P2-versionen av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-106">Privileged Identity Management is available tooyour entire organization when you license your Administrators with hello Premium P2 edition of Azure Active Directory.</span></span> <span data-ttu-id="c1ef4-107">Mer information finns i [Azure Active Directory-versioner](active-directory-editions.md).</span><span class="sxs-lookup"><span data-stu-id="c1ef4-107">For more information, see [Azure Active Directory editions](active-directory-editions.md).</span></span>

<span data-ttu-id="c1ef4-108">Organisationer som vill toominimize hello antalet personer som har åtkomst till toosecure information och resurser för eftersom som minskar hello risken för en obehörig användare komma att åtkomsten.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-108">Organizations want toominimize hello number of people who have access toosecure information or resources, because that reduces hello chance of a malicious user getting that access.</span></span> <span data-ttu-id="c1ef4-109">Användare behöver dock fortfarande toocarry Privilegierade åtgärder i Azure, Office 365 eller SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-109">However, users still need toocarry out privileged operations in Azure, Office 365, or SaaS apps.</span></span> <span data-ttu-id="c1ef4-110">Organisationer som ger användare privilegierad åtkomst i Azure AD utan övervakning vad användarna gör med deras administratörsrättigheter.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-110">Organizations give users privileged access in Azure AD without monitoring what those users are doing with their admin privileges.</span></span> <span data-ttu-id="c1ef4-111">Azure AD Privileged Identity Management hjälper tooresolve denna risk.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-111">Azure AD Privileged Identity Management helps tooresolve this risk.</span></span>  

<span data-ttu-id="c1ef4-112">Azure AD Privileged Identity Management kan du:</span><span class="sxs-lookup"><span data-stu-id="c1ef4-112">Azure AD Privileged Identity Management helps you:</span></span>  

* <span data-ttu-id="c1ef4-113">Se vilka användare som är administratörer för Azure AD</span><span class="sxs-lookup"><span data-stu-id="c1ef4-113">See which users are Azure AD administrators</span></span>
* <span data-ttu-id="c1ef4-114">Aktivera på begäran, just-in-time ”administrativ åtkomst tooMicrosoft onlinetjänster som Office 365 och Intune</span><span class="sxs-lookup"><span data-stu-id="c1ef4-114">Enable on-demand, "just in time" administrative access tooMicrosoft Online Services like Office 365 and Intune</span></span>
* <span data-ttu-id="c1ef4-115">Hämta rapporter om administratören åtkomsthistorik och ändringar i administratörstilldelningar</span><span class="sxs-lookup"><span data-stu-id="c1ef4-115">Get reports about administrator access history and changes in administrator assignments</span></span>
* <span data-ttu-id="c1ef4-116">Få meddelanden om åtkomst tooa Privilegierade roller</span><span class="sxs-lookup"><span data-stu-id="c1ef4-116">Get alerts about access tooa privileged role</span></span>
* <span data-ttu-id="c1ef4-117">Kräv godkännande tooactivate (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="c1ef4-117">Require approval tooactivate (Preview)</span></span>

<span data-ttu-id="c1ef4-118">Azure AD Privileged Identity Management kan hantera hello inbyggda Azure AD organisationens roller, inklusive (men inte begränsat till):</span><span class="sxs-lookup"><span data-stu-id="c1ef4-118">Azure AD Privileged Identity Management can manage hello built-in Azure AD organizational roles, including (but not limited to):</span></span>  

* <span data-ttu-id="c1ef4-119">Global administratör</span><span class="sxs-lookup"><span data-stu-id="c1ef4-119">Global Administrator</span></span>
* <span data-ttu-id="c1ef4-120">Faktureringsadministratör</span><span class="sxs-lookup"><span data-stu-id="c1ef4-120">Billing Administrator</span></span>
* <span data-ttu-id="c1ef4-121">Tjänstadministratör</span><span class="sxs-lookup"><span data-stu-id="c1ef4-121">Service Administrator</span></span>  
* <span data-ttu-id="c1ef4-122">Användare med rollen</span><span class="sxs-lookup"><span data-stu-id="c1ef4-122">User Administrator</span></span>
* <span data-ttu-id="c1ef4-123">Lösenordsadministratör</span><span class="sxs-lookup"><span data-stu-id="c1ef4-123">Password Administrator</span></span>

## <a name="just-in-time-administrator-access"></a><span data-ttu-id="c1ef4-124">Precis i tid administratörsåtkomst</span><span class="sxs-lookup"><span data-stu-id="c1ef4-124">Just in time administrator access</span></span>
<span data-ttu-id="c1ef4-125">Tidigare kan du tilldela en tooan admin användarroll via hello klassiska Azure-portalen eller Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-125">Historically, you could assign a user tooan admin role through hello Azure classic portal or Windows PowerShell.</span></span> <span data-ttu-id="c1ef4-126">Därför blir en **permanent admin**alltid är aktiva i hello som har tilldelats rollen.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-126">As a result, that user becomes a **permanent admin**, always active in hello assigned role.</span></span> <span data-ttu-id="c1ef4-127">Azure AD Privileged Identity Management introducerar hello begreppet en **berättigade admin**. Berättigad administratörer ska användare som behöver privilegierad åtkomst då och då, men inte varje dag.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-127">Azure AD Privileged Identity Management introduces hello concept of an **eligible admin**. Eligible admins should be users that need privileged access now and then, but not every day.</span></span> <span data-ttu-id="c1ef4-128">hello-rollen är inaktiv tills hello användare behöver åtkomst, och sedan de slutföra aktiveringsprocessen och bli administratör active för en förinställd tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-128">hello role is inactive until hello user needs access, then they complete an activation process and become an active admin for a predetermined amount of time.</span></span>

## <a name="enable-privileged-identity-management-for-your-directory"></a><span data-ttu-id="c1ef4-129">Aktivera Privileged Identity Management för din katalog</span><span class="sxs-lookup"><span data-stu-id="c1ef4-129">Enable Privileged Identity Management for your directory</span></span>
<span data-ttu-id="c1ef4-130">Du kan börja använda Azure AD Privileged Identity Management i hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c1ef4-130">You can start using Azure AD Privileged Identity Management in hello [Azure portal](https://portal.azure.com/).</span></span>

> [!NOTE]
> <span data-ttu-id="c1ef4-131">Du måste vara en global administratör med ett organisationskonto (till exempel @yourdomain.com), inte ett Microsoft-konto (till exempel @outlook.com), tooenable Azure AD Privileged Identity Management för en katalog.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-131">You must be a global administrator with an organizational account (for example, @yourdomain.com), not a Microsoft account (for example, @outlook.com), tooenable Azure AD Privileged Identity Management for a directory.</span></span>

1. <span data-ttu-id="c1ef4-132">Logga in toohello [Azure-portalen](https://portal.azure.com/) som global administratör i din katalog.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-132">Sign in toohello [Azure portal](https://portal.azure.com/) as a global administrator of your directory.</span></span>
2. <span data-ttu-id="c1ef4-133">Om din organisation har mer än en katalog, väljer du ditt användarnamn i hello övre högra hörnet av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-133">If your organization has more than one directory, select your username in hello upper right-hand corner of hello Azure portal.</span></span> <span data-ttu-id="c1ef4-134">Välj hello katalog där du ska använda Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-134">Select hello directory where you will use Azure AD Privileged Identity Management.</span></span>
3. <span data-ttu-id="c1ef4-135">Välj **fler tjänster** och använda hello Filter textruta toosearch för **Azure AD Privileged Identity Management**.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-135">Select **More services** and use hello Filter textbox toosearch for **Azure AD Privileged Identity Management**.</span></span>
4. <span data-ttu-id="c1ef4-136">Kontrollera **PIN-kod toodashboard** och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-136">Check **Pin toodashboard** and then click **Create**.</span></span> <span data-ttu-id="c1ef4-137">hello programmet Privileged Identity Management öppnas.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-137">hello Privileged Identity Management application opens.</span></span>

<span data-ttu-id="c1ef4-138">Om du använder hello första personen toouse Azure AD Privileged Identity Management i din katalog, sedan hello [säkerhetsguiden](active-directory-privileged-identity-management-security-wizard.md) vägleder dig genom hello första tilldelningsupplevelse.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-138">If you're hello first person toouse Azure AD Privileged Identity Management in your directory, then hello [security wizard](active-directory-privileged-identity-management-security-wizard.md) walks you through hello initial assignment experience.</span></span> <span data-ttu-id="c1ef4-139">Därefter blir du automatiskt hello först **säkerhetsadministratör** och **administratör av Privilegierade roller** för hello katalog.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-139">After that you automatically become hello first **Security administrator** and **Privileged role administrator** of hello directory.</span></span>

<span data-ttu-id="c1ef4-140">Endast en administratör av Privilegierade roller kan hantera åtkomst till andra administratörer.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-140">Only a privileged role administrator can manage access for other administrators.</span></span> <span data-ttu-id="c1ef4-141">Du kan [ge andra användare hello möjlighet toomanage i PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span><span class="sxs-lookup"><span data-stu-id="c1ef4-141">You can [give other users hello ability toomanage in PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span></span>

## <a name="privileged-identity-management-admin-dashboard"></a><span data-ttu-id="c1ef4-142">Privileged Identity Management admin-instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="c1ef4-142">Privileged Identity Management admin dashboard</span></span>
<span data-ttu-id="c1ef4-143">Azure AD Privileged Identity Manager innehåller en admin-instrumentpanel som innehåller viktig information såsom:</span><span class="sxs-lookup"><span data-stu-id="c1ef4-143">Azure AD Privileged Identity Manager provides an admin dashboard that gives you important information such as:</span></span>

* <span data-ttu-id="c1ef4-144">Aviseringar som påpekas affärsmöjligheter tooimprove säkerhet</span><span class="sxs-lookup"><span data-stu-id="c1ef4-144">Alerts that point out opportunities tooimprove security</span></span>
* <span data-ttu-id="c1ef4-145">hello antalet användare som är tilldelade tooeach Privilegierade roller</span><span class="sxs-lookup"><span data-stu-id="c1ef4-145">hello number of users who are assigned tooeach privileged role</span></span>  
* <span data-ttu-id="c1ef4-146">hello antalet berättigade och permanenta administratörer</span><span class="sxs-lookup"><span data-stu-id="c1ef4-146">hello number of eligible and permanent admins</span></span>
* <span data-ttu-id="c1ef4-147">Ett diagram över aktiveringar av Privilegierade roller i din katalog</span><span class="sxs-lookup"><span data-stu-id="c1ef4-147">A graph of privileged role activations in your directory</span></span>

![PIM instrumentpanelen – skärmbild][2]

## <a name="privileged-role-management"></a><span data-ttu-id="c1ef4-149">Hantering av Privilegierade roller</span><span class="sxs-lookup"><span data-stu-id="c1ef4-149">Privileged role management</span></span>
<span data-ttu-id="c1ef4-150">Med Azure AD Privileged Identity Management kan du hantera hello administratörer genom att lägga till eller ta bort permanent eller är tillämpliga tooeach administratörsrollen.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-150">With Azure AD Privileged Identity Management, you can manage hello administrators by adding or removing permanent or eligible administrators tooeach role.</span></span>

![PIM Lägg till/ta bort systemadministratörer – skärmbild][3]

## <a name="configure-hello-role-activation-settings"></a><span data-ttu-id="c1ef4-152">Konfigurera hello produktaktivering rollinställningar</span><span class="sxs-lookup"><span data-stu-id="c1ef4-152">Configure hello role activation settings</span></span>
<span data-ttu-id="c1ef4-153">Med hjälp av hello [rollinställningar](active-directory-privileged-identity-management-how-to-change-default-settings.md) du kan konfigurera hello berättigade aktivering rollegenskaper inklusive:</span><span class="sxs-lookup"><span data-stu-id="c1ef4-153">Using hello [role settings](active-directory-privileged-identity-management-how-to-change-default-settings.md) you can configure hello eligible role activation properties including:</span></span>

* <span data-ttu-id="c1ef4-154">hello rollen aktiveringsperioden hello varaktighet</span><span class="sxs-lookup"><span data-stu-id="c1ef4-154">hello duration of hello role activation period</span></span>
* <span data-ttu-id="c1ef4-155">Hej rollaktiveringsmeddelande</span><span class="sxs-lookup"><span data-stu-id="c1ef4-155">hello role activation notification</span></span>
* <span data-ttu-id="c1ef4-156">hello information en användare måste tooprovide under aktiveringen för hello roll</span><span class="sxs-lookup"><span data-stu-id="c1ef4-156">hello information a user needs tooprovide during hello role activation process</span></span>
* <span data-ttu-id="c1ef4-157">Biljett eller incident automatiskt</span><span class="sxs-lookup"><span data-stu-id="c1ef4-157">Service ticket or incident number</span></span>
* [<span data-ttu-id="c1ef4-158">Krav för godkännande arbetsflöde - förhandsgranskning</span><span class="sxs-lookup"><span data-stu-id="c1ef4-158">Approval workflow requirements - Preview</span></span>](./privileged-identity-management/azure-ad-pim-approval-workflow.md)

![PIM inställningar - aktivering av administratör – skärmbild][4]

<span data-ttu-id="c1ef4-160">Observera att hello hello bild knappar för **Multifaktorautentisering** har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-160">Note that in hello image, hello buttons for **Multi-Factor Authentication** are disabled.</span></span> <span data-ttu-id="c1ef4-161">För vissa, mycket Privilegierade roller, vi kräver MFA för förhöjd skydd.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-161">For certain, highly privileged roles, we require MFA for heightened protection.</span></span>

## <a name="role-activation"></a><span data-ttu-id="c1ef4-162">Rollaktivering</span><span class="sxs-lookup"><span data-stu-id="c1ef4-162">Role activation</span></span>
<span data-ttu-id="c1ef4-163">för[aktivera en roll](active-directory-privileged-identity-management-how-to-activate-role.md), administratör berättigade begär en Tidsbundna ”aktivering” för hello roll.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-163">too[activate a role](active-directory-privileged-identity-management-how-to-activate-role.md), an eligible admin requests a time-bound "activation" for hello role.</span></span> <span data-ttu-id="c1ef4-164">hello aktiveringen kan begäras med hello **aktivera min roll** alternativ i Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-164">hello activation can be requested using hello **Activate my role** option in Azure AD Privileged Identity Management.</span></span>

<span data-ttu-id="c1ef4-165">En administratör som vill ha tooactivate en roll måste tooinitialize Azure AD Privileged Identity Management i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-165">An admin who wants tooactivate a role needs tooinitialize Azure AD Privileged Identity Management in hello Azure portal.</span></span>

<span data-ttu-id="c1ef4-166">Rollaktivering kan anpassas.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-166">Role activation is customizable.</span></span> <span data-ttu-id="c1ef4-167">I hello PIM-inställningar, kan du ange hello hello aktivering och vilken information Hej administratör måste tooprovide tooactivate hello roll.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-167">In hello PIM settings, you can determine hello length of hello activation and what information hello admin needs tooprovide tooactivate hello role.</span></span>

![PIM administratör begäran rollaktivering – skärmbild][5]

## <a name="review-role-activity"></a><span data-ttu-id="c1ef4-169">Granska rollen aktiviteten</span><span class="sxs-lookup"><span data-stu-id="c1ef4-169">Review role activity</span></span>
<span data-ttu-id="c1ef4-170">Det finns två sätt tootrack hur dina anställda och administratörer använder Privilegierade roller.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-170">There are two ways tootrack how your employees and admins are using privileged roles.</span></span> <span data-ttu-id="c1ef4-171">med hjälp av hello första alternativet [Directory roller granskningshistorik](active-directory-privileged-identity-management-how-to-use-audit-log.md).</span><span class="sxs-lookup"><span data-stu-id="c1ef4-171">hello first option is using [Directory Roles audit history](active-directory-privileged-identity-management-how-to-use-audit-log.md).</span></span> <span data-ttu-id="c1ef4-172">hello granskningshistorik loggar spåra ändringar i Privilegierade rolltilldelningar och historik för aktivering av rollen.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-172">hello audit history logs track changes in privileged role assignments and role activation history.</span></span>

![Historik för aktivering av PIM - skärmbild][6]

<span data-ttu-id="c1ef4-174">hello andra alternativ är tooset upp vanliga [åtkomst till granskningar](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="c1ef4-174">hello second option is tooset up regular [access reviews](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span> <span data-ttu-id="c1ef4-175">Rapporterna åtkomst kan utföras av och tilldelade granskare (till exempel ett team manager) eller hello anställda kan granska sig själva.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-175">These access reviews can be performed by and assigned reviewer (like a team manager) or hello employees can review themselves.</span></span> <span data-ttu-id="c1ef4-176">Detta är hello bästa sätt toomonitor som kräver fortfarande åtkomst och som inte längre.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-176">This is hello best way toomonitor who still requires access, and who no longer does.</span></span>

## <a name="azure-ad-pim-at-subscription-expiration"></a><span data-ttu-id="c1ef4-177">Azure AD PIM på prenumerationen upphör att gälla</span><span class="sxs-lookup"><span data-stu-id="c1ef4-177">Azure AD PIM at subscription expiration</span></span>
<span data-ttu-id="c1ef4-178">Tidigare tooreaching allmän tillgänglighet Azure AD PIM var i förhandsversionen och det fanns ingen licens söker efter klient-toopreview Azure AD PIM.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-178">Prior tooreaching general availability Azure AD PIM was in preview and there were no license checks for a tenant toopreview Azure AD PIM.</span></span>  <span data-ttu-id="c1ef4-179">Nu när Azure AD PIM har nått allmän tillgänglighet, tilldelas utvärderingsversion eller betald licenser toohello administratörer av hello klient toocontinue med PIM.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-179">Now that Azure AD PIM has reached general availability, trial or paid licenses must be assigned toohello administrators of hello tenant toocontinue using PIM.</span></span>  <span data-ttu-id="c1ef4-180">Om din organisation inte köpa Azure AD Premium P2 eller din utvärderingsversion upphör att gälla, vara främst alla hello Azure AD PIM-funktioner inte längre tillgängliga i din klient.</span><span class="sxs-lookup"><span data-stu-id="c1ef4-180">If your organization does not purchase Azure AD Premium P2 or your trial expires, mostly all of hello Azure AD PIM features will no longer be available in your tenant.</span></span>  <span data-ttu-id="c1ef4-181">Du kan läsa mer i hello [krav för Azure AD PIM-prenumeration](./privileged-identity-management/subscription-requirements.md)</span><span class="sxs-lookup"><span data-stu-id="c1ef4-181">You can read more in hello [Azure AD PIM subscription requirements](./privileged-identity-management/subscription-requirements.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1ef4-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c1ef4-182">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Admin_Overview.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_Settings_w_Approval_Disabled.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
