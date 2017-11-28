---
title: Konfigurera Azure AD Privileged Identity Management | Microsoft Docs
description: "Ett avsnitt som förklarar vad Azure AD Privileged Identity Management är och hur du förbättrar säkerheten för molnet med hjälp av PIM."
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
ms.openlocfilehash: eb7059368cb80be7dd625f9dc6ad2aab1bad709a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-azure-ad-privileged-identity-management"></a><span data-ttu-id="9a6af-103">Vad är Azure AD Privileged Identity Management?</span><span class="sxs-lookup"><span data-stu-id="9a6af-103">What is Azure AD Privileged Identity Management?</span></span>
<span data-ttu-id="9a6af-104">Med Azure Active Directory (AD) Privileged Identity Management kan du hantera, kontrollera och övervaka åtkomst inom din organisation.</span><span class="sxs-lookup"><span data-stu-id="9a6af-104">With Azure Active Directory (AD) Privileged Identity Management, you can manage, control, and monitor access within your organization.</span></span> <span data-ttu-id="9a6af-105">Detta inkluderar åtkomst till resurser i Azure AD och andra Microsoft online-tjänster som Office 365 eller Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="9a6af-105">This includes access to resources in Azure AD and other Microsoft online services like Office 365 or Microsoft Intune.</span></span>  

> [!NOTE]
> <span data-ttu-id="9a6af-106">Privileged Identity Management är tillgänglig för hela organisationen när du licensierar dina administratörer med Premium P2-versionen av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9a6af-106">Privileged Identity Management is available to your entire organization when you license your Administrators with the Premium P2 edition of Azure Active Directory.</span></span> <span data-ttu-id="9a6af-107">Mer information finns i [Azure Active Directory-versioner](active-directory-editions.md).</span><span class="sxs-lookup"><span data-stu-id="9a6af-107">For more information, see [Azure Active Directory editions](active-directory-editions.md).</span></span>

<span data-ttu-id="9a6af-108">Organisationer som vill minimera antalet personer som har åtkomst till säker information eller resurser, eftersom som minskar risken för en obehörig användare komma att åtkomsten.</span><span class="sxs-lookup"><span data-stu-id="9a6af-108">Organizations want to minimize the number of people who have access to secure information or resources, because that reduces the chance of a malicious user getting that access.</span></span> <span data-ttu-id="9a6af-109">Användare behöver dock fortfarande att utföra Privilegierade åtgärder i Azure, Office 365 eller SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="9a6af-109">However, users still need to carry out privileged operations in Azure, Office 365, or SaaS apps.</span></span> <span data-ttu-id="9a6af-110">Organisationer som ger användare privilegierad åtkomst i Azure AD utan övervakning vad användarna gör med deras administratörsrättigheter.</span><span class="sxs-lookup"><span data-stu-id="9a6af-110">Organizations give users privileged access in Azure AD without monitoring what those users are doing with their admin privileges.</span></span> <span data-ttu-id="9a6af-111">Azure AD Privileged Identity Management kan lösa denna risk.</span><span class="sxs-lookup"><span data-stu-id="9a6af-111">Azure AD Privileged Identity Management helps to resolve this risk.</span></span>  

<span data-ttu-id="9a6af-112">Azure AD Privileged Identity Management kan du:</span><span class="sxs-lookup"><span data-stu-id="9a6af-112">Azure AD Privileged Identity Management helps you:</span></span>  

* <span data-ttu-id="9a6af-113">Se vilka användare som är administratörer för Azure AD</span><span class="sxs-lookup"><span data-stu-id="9a6af-113">See which users are Azure AD administrators</span></span>
* <span data-ttu-id="9a6af-114">Aktivera på begäran, just-in-time ”administrativ åtkomst till Microsofts onlinetjänster som Office 365 och Intune</span><span class="sxs-lookup"><span data-stu-id="9a6af-114">Enable on-demand, "just in time" administrative access to Microsoft Online Services like Office 365 and Intune</span></span>
* <span data-ttu-id="9a6af-115">Hämta rapporter om administratören åtkomsthistorik och ändringar i administratörstilldelningar</span><span class="sxs-lookup"><span data-stu-id="9a6af-115">Get reports about administrator access history and changes in administrator assignments</span></span>
* <span data-ttu-id="9a6af-116">Få meddelanden om åtkomst till en privilegierad roll</span><span class="sxs-lookup"><span data-stu-id="9a6af-116">Get alerts about access to a privileged role</span></span>
* <span data-ttu-id="9a6af-117">Kräv godkännande för att aktivera (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="9a6af-117">Require approval to activate (Preview)</span></span>

<span data-ttu-id="9a6af-118">Azure AD Privileged Identity Management kan hantera inbyggt Azure AD organisationens roller, inklusive (men inte begränsat till):</span><span class="sxs-lookup"><span data-stu-id="9a6af-118">Azure AD Privileged Identity Management can manage the built-in Azure AD organizational roles, including (but not limited to):</span></span>  

* <span data-ttu-id="9a6af-119">Global administratör</span><span class="sxs-lookup"><span data-stu-id="9a6af-119">Global Administrator</span></span>
* <span data-ttu-id="9a6af-120">Faktureringsadministratör</span><span class="sxs-lookup"><span data-stu-id="9a6af-120">Billing Administrator</span></span>
* <span data-ttu-id="9a6af-121">Tjänstadministratör</span><span class="sxs-lookup"><span data-stu-id="9a6af-121">Service Administrator</span></span>  
* <span data-ttu-id="9a6af-122">Användare med rollen</span><span class="sxs-lookup"><span data-stu-id="9a6af-122">User Administrator</span></span>
* <span data-ttu-id="9a6af-123">Lösenordsadministratör</span><span class="sxs-lookup"><span data-stu-id="9a6af-123">Password Administrator</span></span>

## <a name="just-in-time-administrator-access"></a><span data-ttu-id="9a6af-124">Precis i tid administratörsåtkomst</span><span class="sxs-lookup"><span data-stu-id="9a6af-124">Just in time administrator access</span></span>
<span data-ttu-id="9a6af-125">Tidigare kan du tilldela en användare till en administratörsroll via den klassiska Azure-portalen eller Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9a6af-125">Historically, you could assign a user to an admin role through the Azure classic portal or Windows PowerShell.</span></span> <span data-ttu-id="9a6af-126">Därför blir en **permanent admin**alltid är aktiva i den tilldelade rollen.</span><span class="sxs-lookup"><span data-stu-id="9a6af-126">As a result, that user becomes a **permanent admin**, always active in the assigned role.</span></span> <span data-ttu-id="9a6af-127">Azure AD Privileged Identity Management introducerar konceptet för en **berättigade admin**.</span><span class="sxs-lookup"><span data-stu-id="9a6af-127">Azure AD Privileged Identity Management introduces the concept of an **eligible admin**.</span></span> <span data-ttu-id="9a6af-128">Berättigad administratörer ska användare som behöver privilegierad åtkomst då och då, men inte varje dag.</span><span class="sxs-lookup"><span data-stu-id="9a6af-128">Eligible admins should be users that need privileged access now and then, but not every day.</span></span> <span data-ttu-id="9a6af-129">Rollen är inaktiv tills användaren behöver åtkomst, och sedan de slutföra aktiveringsprocessen och bli administratör active för en förinställd tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="9a6af-129">The role is inactive until the user needs access, then they complete an activation process and become an active admin for a predetermined amount of time.</span></span>

## <a name="enable-privileged-identity-management-for-your-directory"></a><span data-ttu-id="9a6af-130">Aktivera Privileged Identity Management för din katalog</span><span class="sxs-lookup"><span data-stu-id="9a6af-130">Enable Privileged Identity Management for your directory</span></span>
<span data-ttu-id="9a6af-131">Du kan börja använda Azure AD Privileged Identity Management i den [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="9a6af-131">You can start using Azure AD Privileged Identity Management in the [Azure portal](https://portal.azure.com/).</span></span>

> [!NOTE]
> <span data-ttu-id="9a6af-132">Du måste vara en global administratör med ett organisationskonto (till exempel @yourdomain.com), inte ett Microsoft-konto (till exempel @outlook.com), för att aktivera Azure AD Privileged Identity Management för en katalog.</span><span class="sxs-lookup"><span data-stu-id="9a6af-132">You must be a global administrator with an organizational account (for example, @yourdomain.com), not a Microsoft account (for example, @outlook.com), to enable Azure AD Privileged Identity Management for a directory.</span></span>

1. <span data-ttu-id="9a6af-133">Logga in på [Azure-portalen](https://portal.azure.com/) som global administratör för din katalog.</span><span class="sxs-lookup"><span data-stu-id="9a6af-133">Sign in to the [Azure portal](https://portal.azure.com/) as a global administrator of your directory.</span></span>
2. <span data-ttu-id="9a6af-134">Om din organisation har mer än en katalog väljer du ditt användarnamn längst upp till höger på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9a6af-134">If your organization has more than one directory, select your username in the upper right-hand corner of the Azure portal.</span></span> <span data-ttu-id="9a6af-135">Välj den katalog där du ska använda Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="9a6af-135">Select the directory where you will use Azure AD Privileged Identity Management.</span></span>
3. <span data-ttu-id="9a6af-136">Välj **Fler tjänster** och använd textrutan Filter för att söka efter **Azure AD Privileged Identity Management**.</span><span class="sxs-lookup"><span data-stu-id="9a6af-136">Select **More services** and use the Filter textbox to search for **Azure AD Privileged Identity Management**.</span></span>
4. <span data-ttu-id="9a6af-137">Markera **Fäst på instrumentpanelen** och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9a6af-137">Check **Pin to dashboard** and then click **Create**.</span></span> <span data-ttu-id="9a6af-138">Privileged Identity Management-programmet öppnas.</span><span class="sxs-lookup"><span data-stu-id="9a6af-138">The Privileged Identity Management application opens.</span></span>

<span data-ttu-id="9a6af-139">Om du är den första som använder Azure AD Privileged Identity Management i din katalog kommer [säkerhetsguiden](active-directory-privileged-identity-management-security-wizard.md) att vägleda dig genom den första tilldelningen.</span><span class="sxs-lookup"><span data-stu-id="9a6af-139">If you're the first person to use Azure AD Privileged Identity Management in your directory, then the [security wizard](active-directory-privileged-identity-management-security-wizard.md) walks you through the initial assignment experience.</span></span> <span data-ttu-id="9a6af-140">Därefter blir du automatiskt först **säkerhetsadministratör** och **administratör av Privilegierade roller** av katalogen.</span><span class="sxs-lookup"><span data-stu-id="9a6af-140">After that you automatically become the first **Security administrator** and **Privileged role administrator** of the directory.</span></span>

<span data-ttu-id="9a6af-141">Endast en administratör av Privilegierade roller kan hantera åtkomst till andra administratörer.</span><span class="sxs-lookup"><span data-stu-id="9a6af-141">Only a privileged role administrator can manage access for other administrators.</span></span> <span data-ttu-id="9a6af-142">Du kan [ge andra användare möjlighet att hantera i PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span><span class="sxs-lookup"><span data-stu-id="9a6af-142">You can [give other users the ability to manage in PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span></span>

## <a name="privileged-identity-management-admin-dashboard"></a><span data-ttu-id="9a6af-143">Privileged Identity Management admin-instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="9a6af-143">Privileged Identity Management admin dashboard</span></span>
<span data-ttu-id="9a6af-144">Azure AD Privileged Identity Manager innehåller en admin-instrumentpanel som innehåller viktig information såsom:</span><span class="sxs-lookup"><span data-stu-id="9a6af-144">Azure AD Privileged Identity Manager provides an admin dashboard that gives you important information such as:</span></span>

* <span data-ttu-id="9a6af-145">Aviseringar som påpekas möjligheter att öka säkerheten</span><span class="sxs-lookup"><span data-stu-id="9a6af-145">Alerts that point out opportunities to improve security</span></span>
* <span data-ttu-id="9a6af-146">Antalet användare som har tilldelats varje privilegierad roll</span><span class="sxs-lookup"><span data-stu-id="9a6af-146">The number of users who are assigned to each privileged role</span></span>  
* <span data-ttu-id="9a6af-147">Antalet berättigade och permanenta administratörer</span><span class="sxs-lookup"><span data-stu-id="9a6af-147">The number of eligible and permanent admins</span></span>
* <span data-ttu-id="9a6af-148">Ett diagram över aktiveringar av Privilegierade roller i din katalog</span><span class="sxs-lookup"><span data-stu-id="9a6af-148">A graph of privileged role activations in your directory</span></span>

![PIM instrumentpanelen – skärmbild][2]

## <a name="privileged-role-management"></a><span data-ttu-id="9a6af-150">Hantering av Privilegierade roller</span><span class="sxs-lookup"><span data-stu-id="9a6af-150">Privileged role management</span></span>
<span data-ttu-id="9a6af-151">Med Azure AD Privileged Identity Management kan du hantera administratörer genom att lägga till eller ta bort permanent eller berättigade administratörer för varje roll.</span><span class="sxs-lookup"><span data-stu-id="9a6af-151">With Azure AD Privileged Identity Management, you can manage the administrators by adding or removing permanent or eligible administrators to each role.</span></span>

![PIM Lägg till/ta bort systemadministratörer – skärmbild][3]

## <a name="configure-the-role-activation-settings"></a><span data-ttu-id="9a6af-153">Konfigurera inställningar för roll-aktivering</span><span class="sxs-lookup"><span data-stu-id="9a6af-153">Configure the role activation settings</span></span>
<span data-ttu-id="9a6af-154">Med hjälp av den [rollinställningar](active-directory-privileged-identity-management-how-to-change-default-settings.md) du kan konfigurera egenskaper för aktivering berättigade roll, inklusive:</span><span class="sxs-lookup"><span data-stu-id="9a6af-154">Using the [role settings](active-directory-privileged-identity-management-how-to-change-default-settings.md) you can configure the eligible role activation properties including:</span></span>

* <span data-ttu-id="9a6af-155">Varaktighet för aktiveringsperioden roll</span><span class="sxs-lookup"><span data-stu-id="9a6af-155">The duration of the role activation period</span></span>
* <span data-ttu-id="9a6af-156">Rollaktiveringsmeddelande</span><span class="sxs-lookup"><span data-stu-id="9a6af-156">The role activation notification</span></span>
* <span data-ttu-id="9a6af-157">Informationen om en användare måste ange under aktiveringen roll</span><span class="sxs-lookup"><span data-stu-id="9a6af-157">The information a user needs to provide during the role activation process</span></span>
* <span data-ttu-id="9a6af-158">Biljett eller incident automatiskt</span><span class="sxs-lookup"><span data-stu-id="9a6af-158">Service ticket or incident number</span></span>
* [<span data-ttu-id="9a6af-159">Krav för godkännande arbetsflöde - förhandsgranskning</span><span class="sxs-lookup"><span data-stu-id="9a6af-159">Approval workflow requirements - Preview</span></span>](./privileged-identity-management/azure-ad-pim-approval-workflow.md)

![PIM inställningar - aktivering av administratör – skärmbild][4]

<span data-ttu-id="9a6af-161">Observera att i bild knapparna för **Multifaktorautentisering** har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="9a6af-161">Note that in the image, the buttons for **Multi-Factor Authentication** are disabled.</span></span> <span data-ttu-id="9a6af-162">För vissa, mycket Privilegierade roller, vi kräver MFA för förhöjd skydd.</span><span class="sxs-lookup"><span data-stu-id="9a6af-162">For certain, highly privileged roles, we require MFA for heightened protection.</span></span>

## <a name="role-activation"></a><span data-ttu-id="9a6af-163">Rollaktivering</span><span class="sxs-lookup"><span data-stu-id="9a6af-163">Role activation</span></span>
<span data-ttu-id="9a6af-164">Att [aktivera en roll](active-directory-privileged-identity-management-how-to-activate-role.md), administratör berättigade begär en Tidsbundna ”aktivering” för rollen.</span><span class="sxs-lookup"><span data-stu-id="9a6af-164">To [activate a role](active-directory-privileged-identity-management-how-to-activate-role.md), an eligible admin requests a time-bound "activation" for the role.</span></span> <span data-ttu-id="9a6af-165">Aktiveringen kan begäras med hjälp av den **aktivera min roll** alternativ i Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="9a6af-165">The activation can be requested using the **Activate my role** option in Azure AD Privileged Identity Management.</span></span>

<span data-ttu-id="9a6af-166">En administratör som vill aktivera en roll måste initiera Azure AD Privileged Identity Management i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9a6af-166">An admin who wants to activate a role needs to initialize Azure AD Privileged Identity Management in the Azure portal.</span></span>

<span data-ttu-id="9a6af-167">Rollaktivering kan anpassas.</span><span class="sxs-lookup"><span data-stu-id="9a6af-167">Role activation is customizable.</span></span> <span data-ttu-id="9a6af-168">I PIM-inställningar kan du ange längden på aktivering och vad information som administratören behöver för att aktivera rollen.</span><span class="sxs-lookup"><span data-stu-id="9a6af-168">In the PIM settings, you can determine the length of the activation and what information the admin needs to provide to activate the role.</span></span>

![PIM administratör begäran rollaktivering – skärmbild][5]

## <a name="review-role-activity"></a><span data-ttu-id="9a6af-170">Granska rollen aktiviteten</span><span class="sxs-lookup"><span data-stu-id="9a6af-170">Review role activity</span></span>
<span data-ttu-id="9a6af-171">Det finns två sätt att spåra hur dina anställda och administratörer använder Privilegierade roller.</span><span class="sxs-lookup"><span data-stu-id="9a6af-171">There are two ways to track how your employees and admins are using privileged roles.</span></span> <span data-ttu-id="9a6af-172">Med hjälp av alternativet först [Directory roller granskningshistorik](active-directory-privileged-identity-management-how-to-use-audit-log.md).</span><span class="sxs-lookup"><span data-stu-id="9a6af-172">The first option is using [Directory Roles audit history](active-directory-privileged-identity-management-how-to-use-audit-log.md).</span></span> <span data-ttu-id="9a6af-173">Granskningshistoriken loggar spåra ändringar i Privilegierade rolltilldelningar och historik för aktivering av rollen.</span><span class="sxs-lookup"><span data-stu-id="9a6af-173">The audit history logs track changes in privileged role assignments and role activation history.</span></span>

![Historik för aktivering av PIM - skärmbild][6]

<span data-ttu-id="9a6af-175">Det andra alternativet är att konfigurera vanliga [åtkomst till granskningar](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="9a6af-175">The second option is to set up regular [access reviews](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span> <span data-ttu-id="9a6af-176">Rapporterna åtkomst kan utföras av och tilldelade granskare (till exempel ett team manager) eller anställda kan granska sig själva.</span><span class="sxs-lookup"><span data-stu-id="9a6af-176">These access reviews can be performed by and assigned reviewer (like a team manager) or the employees can review themselves.</span></span> <span data-ttu-id="9a6af-177">Detta är det bästa sättet att övervaka som kräver fortfarande åtkomst och som inte längre finns.</span><span class="sxs-lookup"><span data-stu-id="9a6af-177">This is the best way to monitor who still requires access, and who no longer does.</span></span>

## <a name="azure-ad-pim-at-subscription-expiration"></a><span data-ttu-id="9a6af-178">Azure AD PIM på prenumerationen upphör att gälla</span><span class="sxs-lookup"><span data-stu-id="9a6af-178">Azure AD PIM at subscription expiration</span></span>
<span data-ttu-id="9a6af-179">Innan du når allmän tillgänglighet Azure AD PIM var i förhandsversionen och det fanns ingen licens kontroller för en klient att förhandsgranska Azure AD PIM.</span><span class="sxs-lookup"><span data-stu-id="9a6af-179">Prior to reaching general availability Azure AD PIM was in preview and there were no license checks for a tenant to preview Azure AD PIM.</span></span>  <span data-ttu-id="9a6af-180">Nu när Azure AD PIM har nått allmän tillgänglighet, måste utvärderingsversion eller betald licenser tilldelas till administratörer för klienten att fortsätta använda PIM.</span><span class="sxs-lookup"><span data-stu-id="9a6af-180">Now that Azure AD PIM has reached general availability, trial or paid licenses must be assigned to the administrators of the tenant to continue using PIM.</span></span>  <span data-ttu-id="9a6af-181">Om din organisation inte köpa Azure AD Premium P2 eller din utvärderingsversion upphör att gälla, vara främst alla Azure AD PIM-funktioner inte längre tillgängliga i din klient.</span><span class="sxs-lookup"><span data-stu-id="9a6af-181">If your organization does not purchase Azure AD Premium P2 or your trial expires, mostly all of the Azure AD PIM features will no longer be available in your tenant.</span></span>  <span data-ttu-id="9a6af-182">Du kan läsa mer i den [krav för Azure AD PIM-prenumeration](./privileged-identity-management/subscription-requirements.md)</span><span class="sxs-lookup"><span data-stu-id="9a6af-182">You can read more in the [Azure AD PIM subscription requirements](./privileged-identity-management/subscription-requirements.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a6af-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9a6af-183">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Admin_Overview.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_Settings_w_Approval_Disabled.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
