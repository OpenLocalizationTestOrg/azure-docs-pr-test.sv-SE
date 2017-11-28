---
title: aaaManaging grupper i Azure Active Directory | Microsoft Docs
description: "Hur toocreate och hantera grupper toomanage Azure användare med Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d1f5451c-3807-423c-8bac-2822d27b893f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/24/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 9bee224655639740b3dd99983892b30c3c537aa0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-groups-in-azure-active-directory"></a><span data-ttu-id="f15bc-103">Hantera grupper i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f15bc-103">Managing groups in Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f15bc-104">Azure-portal</span><span class="sxs-lookup"><span data-stu-id="f15bc-104">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="f15bc-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="f15bc-105">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="f15bc-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f15bc-106">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="f15bc-107">En av hello funktionerna i Azure Active Directory (AD Azure) användarhantering är hello möjlighet toocreate grupper av användare.</span><span class="sxs-lookup"><span data-stu-id="f15bc-107">One of hello features of Azure Active Directory (Azure AD) user management is hello ability toocreate groups of users.</span></span> <span data-ttu-id="f15bc-108">Du kan använda en grupp tooperform hanteringsuppgifter, till exempel tilldela licenser eller behörigheter tooa många användare samtidigt.</span><span class="sxs-lookup"><span data-stu-id="f15bc-108">You use a group tooperform management tasks such as assigning licenses or permissions tooa number of users at once.</span></span> <span data-ttu-id="f15bc-109">Du kan också använda grupper tooassign behörighet till</span><span class="sxs-lookup"><span data-stu-id="f15bc-109">You can also use groups tooassign access permission to</span></span>

* <span data-ttu-id="f15bc-110">Resurser, till exempel objekt i hello directory</span><span class="sxs-lookup"><span data-stu-id="f15bc-110">Resources such as objects in hello directory</span></span>
* <span data-ttu-id="f15bc-111">Resurser externa toohello katalogen som SaaS-program, Azure-tjänster, SharePoint-webbplatser eller lokala resurser</span><span class="sxs-lookup"><span data-stu-id="f15bc-111">Resources external toohello directory such as SaaS applications, Azure services, SharePoint sites, or on-premises resources</span></span>

<span data-ttu-id="f15bc-112">En resursägare kan dessutom också tilldela åtkomst tooa tooan Azure AD resursgruppen som ägs av någon annan.</span><span class="sxs-lookup"><span data-stu-id="f15bc-112">In addition, a resource owner can also assign access tooa resource tooan Azure AD group owned by someone else.</span></span> <span data-ttu-id="f15bc-113">Tilldelningen ger hello medlemmar av gruppen åtkomst toohello resursen.</span><span class="sxs-lookup"><span data-stu-id="f15bc-113">This assignment grants hello members of that group access toohello resource.</span></span> <span data-ttu-id="f15bc-114">Hello grupp hello ägare hanterar sedan medlemskap i gruppen hello.</span><span class="sxs-lookup"><span data-stu-id="f15bc-114">Then, hello owner of hello group manages membership in hello group.</span></span> <span data-ttu-id="f15bc-115">Effektivt, hello resurs ägare delegater toohello ägare till hello grupp hello behörighet tooassign användare tootheir resurs.</span><span class="sxs-lookup"><span data-stu-id="f15bc-115">Effectively, hello resource owner delegates toohello owner of hello group hello permission tooassign users tootheir resource.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f15bc-116">Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f15bc-116">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> <span data-ttu-id="f15bc-117">Hur toomanage grupper i administrationscentret för hello Azure AD, finns i [skapar en grupp och lägga till medlemmar i Azure Active Directory](active-directory-groups-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f15bc-117">For how toomanage groups in hello Azure AD admin center, see [Create a group and add members in Azure Active Directory](active-directory-groups-create-azure-portal.md).</span></span>

## <a name="how-do-i-create-a-group"></a><span data-ttu-id="f15bc-118">Hur skapar jag en grupp?</span><span class="sxs-lookup"><span data-stu-id="f15bc-118">How do I create a group?</span></span>
<span data-ttu-id="f15bc-119">Du kan skapa en grupp med en av följande hello beroende tjänster från hello toowhich din organisation prenumererar på:</span><span class="sxs-lookup"><span data-stu-id="f15bc-119">Depending on hello services toowhich your organization has subscribed, you can create a group using one of hello following:</span></span>

* <span data-ttu-id="f15bc-120">hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f15bc-120">hello Azure classic portal</span></span>
* <span data-ttu-id="f15bc-121">hello Office 365-kontoportalen</span><span class="sxs-lookup"><span data-stu-id="f15bc-121">hello Office 365 account portal</span></span>
* <span data-ttu-id="f15bc-122">hello Windows Intune-kontoportalen</span><span class="sxs-lookup"><span data-stu-id="f15bc-122">hello Windows Intune account portal</span></span>

<span data-ttu-id="f15bc-123">Vi kommer att beskriva uppgifter som utförs i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f15bc-123">We'll describe tasks as performed in hello Azure classic portal.</span></span> <span data-ttu-id="f15bc-124">Mer information om hur du använder Azure-portaler toomanage Azure AD-katalogen finns [administrera Azure AD-katalogen](active-directory-administer.md).</span><span class="sxs-lookup"><span data-stu-id="f15bc-124">For more information about using non-Azure portals toomanage your Azure AD directory, see [Administering your Azure AD directory](active-directory-administer.md).</span></span>

1. <span data-ttu-id="f15bc-125">I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och välj sedan hello namnet på hello katalog för organisationen.</span><span class="sxs-lookup"><span data-stu-id="f15bc-125">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select hello name of hello directory for your organization.</span></span>
2. <span data-ttu-id="f15bc-126">Välj hello **grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="f15bc-126">Select hello **Groups** tab.</span></span>
3. <span data-ttu-id="f15bc-127">Välj **Lägg till grupp**.</span><span class="sxs-lookup"><span data-stu-id="f15bc-127">Select **Add Group**.</span></span>
4. <span data-ttu-id="f15bc-128">I hello **Lägg till grupp** fönstret Ange hello namn och hello beskrivningen för en grupp.</span><span class="sxs-lookup"><span data-stu-id="f15bc-128">In hello **Add Group** window, specify hello name and hello description of a group.</span></span>

## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a><span data-ttu-id="f15bc-129">Hur lägger jag till eller tar bort enskilda användare i en säkerhetsgrupp?</span><span class="sxs-lookup"><span data-stu-id="f15bc-129">How do I add or remove individual users in a security group?</span></span>
<span data-ttu-id="f15bc-130">**tooadd en enskild användare tooa grupp**</span><span class="sxs-lookup"><span data-stu-id="f15bc-130">**tooadd an individual user tooa group**</span></span>

1. <span data-ttu-id="f15bc-131">I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och välj sedan hello namnet på hello katalog för organisationen.</span><span class="sxs-lookup"><span data-stu-id="f15bc-131">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select hello name of hello directory for your organization.</span></span>
2. <span data-ttu-id="f15bc-132">Välj hello **grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="f15bc-132">Select hello **Groups** tab.</span></span>
3. <span data-ttu-id="f15bc-133">Öppna hello grupp toowhich som du vill tooadd medlemmar.</span><span class="sxs-lookup"><span data-stu-id="f15bc-133">Open hello group toowhich you want tooadd members.</span></span> <span data-ttu-id="f15bc-134">Öppna hello **medlemmar** fliken hello markerad grupp om den inte redan visas.</span><span class="sxs-lookup"><span data-stu-id="f15bc-134">Open hello **Members** tab of hello selected group if it not already displaying.</span></span>
4. <span data-ttu-id="f15bc-135">Välj **Lägg till medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="f15bc-135">Select **Add Members**.</span></span>
5. <span data-ttu-id="f15bc-136">På hello **lägga till medlemmar** sida och välj hello namnet hello användare eller grupp som du vill tooadd som en medlem i den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="f15bc-136">On hello **Add Members** page, select hello name of hello user or a group that you want tooadd as a member of this group.</span></span> <span data-ttu-id="f15bc-137">Kontrollera att namnet läggs toohello **valda** fönstret.</span><span class="sxs-lookup"><span data-stu-id="f15bc-137">Make sure that this name is added toohello **Selected** pane.</span></span>

<span data-ttu-id="f15bc-138">**tooremove en enskild användare från en grupp**</span><span class="sxs-lookup"><span data-stu-id="f15bc-138">**tooremove an individual user from a group**</span></span>

1. <span data-ttu-id="f15bc-139">I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och välj sedan hello namnet på hello katalog för organisationen.</span><span class="sxs-lookup"><span data-stu-id="f15bc-139">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select hello name of hello directory for your organization.</span></span>
2. <span data-ttu-id="f15bc-140">Välj hello **grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="f15bc-140">Select hello **Groups** tab.</span></span>
3. <span data-ttu-id="f15bc-141">Öppna hello-grupp som du vill tooremove medlemmar.</span><span class="sxs-lookup"><span data-stu-id="f15bc-141">Open hello group from which you want tooremove members.</span></span>
4. <span data-ttu-id="f15bc-142">Välj hello **medlemmar** fliken, väljer hello namnet för hello medlem du vill tooremove från den här gruppen och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f15bc-142">Select hello **Members** tab, select hello name of hello member that you want tooremove from this group, and then click **Remove**.</span></span>
5. <span data-ttu-id="f15bc-143">Bekräfta som du vill tooremove den här medlemmen från gruppen hello hello i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="f15bc-143">Confirm at hello prompt that you want tooremove this member from hello group.</span></span>

## <a name="how-can-i-manage-hello-membership-of-a-group-dynamically"></a><span data-ttu-id="f15bc-144">Hur hanterar jag hello medlemskap i en grupp dynamiskt?</span><span class="sxs-lookup"><span data-stu-id="f15bc-144">How can I manage hello membership of a group dynamically?</span></span>
<span data-ttu-id="f15bc-145">I Azure AD kan du enkelt konfigurera en enkel regel toodetermine vilka användare som är toobe medlemmar i hello-gruppen.</span><span class="sxs-lookup"><span data-stu-id="f15bc-145">In Azure AD, you can very easily set up a simple rule toodetermine which users are toobe members of hello group.</span></span> <span data-ttu-id="f15bc-146">En enkel regel är en regel som bara gör en jämförelse.</span><span class="sxs-lookup"><span data-stu-id="f15bc-146">A simple rule is one that makes only a single comparison.</span></span> <span data-ttu-id="f15bc-147">Till exempel om en grupp har tilldelats tooa SaaS-program, kan du konfigurera en regel tooadd användare som har befattningen ”säljare”.</span><span class="sxs-lookup"><span data-stu-id="f15bc-147">For example, if a group is assigned tooa SaaS application, you can set up a rule tooadd users who have a job title of "Sales Rep."</span></span> <span data-ttu-id="f15bc-148">Den här regeln beviljar sedan åtkomst toothis SaaS programanvändare tooall med den befattningen i din katalog.</span><span class="sxs-lookup"><span data-stu-id="f15bc-148">This rule then grants access toothis SaaS application tooall users with that job title in your directory.</span></span>

<span data-ttu-id="f15bc-149">När alla attribut för en användare ändra hello utvärderas alla dynamisk gruppregler i en katalog toosee om ändring av hello-attribut för hello användare skulle leda till valfri grupp lägger till eller tar bort.</span><span class="sxs-lookup"><span data-stu-id="f15bc-149">When any attributes of a user change, hello system evaluates all dynamic group rules in a directory toosee if hello attribute change of hello user would trigger any group adds or removes.</span></span> <span data-ttu-id="f15bc-150">Om en användare uppfyller en regel för en grupp, läggs de som en medlem toothat grupp.</span><span class="sxs-lookup"><span data-stu-id="f15bc-150">If a user satisfies a rule on a group, they are added as a member toothat group.</span></span> <span data-ttu-id="f15bc-151">Om de inte längre uppfyller hello regel i en grupp som de är medlem i tas de bort som en medlemmar från gruppen.</span><span class="sxs-lookup"><span data-stu-id="f15bc-151">If they no longer satisfy hello rule of a group they are a member of, they are removed as a members from that group.</span></span>

> [!NOTE]
> <span data-ttu-id="f15bc-152">Du kan skapa en regel för dynamiskt medlemskap för säkerhetsgrupper eller Office 365-grupper.</span><span class="sxs-lookup"><span data-stu-id="f15bc-152">You can set up a rule for dynamic membership on security groups or Office 365 groups.</span></span> <span data-ttu-id="f15bc-153">Kapslade gruppmedlemskap stöds inte för närvarande för gruppbaserad tilldelning tooapplications.</span><span class="sxs-lookup"><span data-stu-id="f15bc-153">Nested group memberships aren't currently supported for group-based assignment tooapplications.</span></span>
>
> <span data-ttu-id="f15bc-154">Dynamiskt medlemskap för grupper kräver en toobe för Azure AD Premium-licens som tilldelats</span><span class="sxs-lookup"><span data-stu-id="f15bc-154">Dynamic memberships for groups require an Azure AD Premium license toobe assigned to</span></span>
>
> * <span data-ttu-id="f15bc-155">Hej administratör som hanterar hello regeln för en grupp</span><span class="sxs-lookup"><span data-stu-id="f15bc-155">hello administrator who manages hello rule on a group</span></span>
> * <span data-ttu-id="f15bc-156">Alla medlemmar i gruppen hello</span><span class="sxs-lookup"><span data-stu-id="f15bc-156">All members of hello group</span></span>
>
>

<span data-ttu-id="f15bc-157">**tooenable dynamiskt medlemskap för en grupp**</span><span class="sxs-lookup"><span data-stu-id="f15bc-157">**tooenable dynamic membership for a group**</span></span>

1. <span data-ttu-id="f15bc-158">I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och välj sedan hello namnet på hello katalog för organisationen.</span><span class="sxs-lookup"><span data-stu-id="f15bc-158">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select hello name of hello directory for your organization.</span></span>
2. <span data-ttu-id="f15bc-159">Välj hello **grupper** fliken och öppna hello-grupp som du vill tooedit.</span><span class="sxs-lookup"><span data-stu-id="f15bc-159">Select hello **Groups** tab, and open hello group you want tooedit.</span></span>
3. <span data-ttu-id="f15bc-160">Välj hello **konfigurera** fliken och ange sedan **Aktivera dynamiskt medlemskap** för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="f15bc-160">Select hello **Configure** tab, and then set **Enable Dynamic Memberships** too**Yes**.</span></span>
4. <span data-ttu-id="f15bc-161">Konfigurera en enkel regel för hello grupp toocontrol hur dynamiskt medlemskap för gruppen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f15bc-161">Set up a simple single rule for hello group toocontrol how dynamic membership for this group functions.</span></span> <span data-ttu-id="f15bc-162">Se till att hello **lägga till användare där** alternativet är markerat och välj sedan en användaregenskap hello listan (till exempel avdelning, befattning, etc.)</span><span class="sxs-lookup"><span data-stu-id="f15bc-162">Make sure hello **Add users where** option is selected, and then select a user property from hello list (for example, department, jobTitle, etc.),</span></span>
5. <span data-ttu-id="f15bc-163">Välj ett villkor (inte lika med, lika med, börjar inte med, börjar med, innehåller ej, innehåller, matchar inte, matchar).</span><span class="sxs-lookup"><span data-stu-id="f15bc-163">Next, select a condition (Not Equals, Equals, Not Starts With, Starts With, Not Contains, Contains, Not Match, Match).</span></span>
6. <span data-ttu-id="f15bc-164">Ange ett Jämförelsevärde för hello valda Användaregenskapen.</span><span class="sxs-lookup"><span data-stu-id="f15bc-164">Specify a comparison value for hello selected user property.</span></span>

<span data-ttu-id="f15bc-165">toolearn om hur toocreate *avancerade* regler (regler som kan innehålla flera jämförelser) för dynamiska gruppmedlemskap finns [genom att använda attribut toocreate avancerade regler](active-directory-accessmanagement-groups-with-advanced-rules.md).</span><span class="sxs-lookup"><span data-stu-id="f15bc-165">toolearn about how toocreate *advanced* rules (rules that can contain multiple comparisons) for dynamic group membership, see [Using attributes toocreate advanced rules](active-directory-accessmanagement-groups-with-advanced-rules.md).</span></span>

## <a name="additional-information"></a><span data-ttu-id="f15bc-166">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="f15bc-166">Additional information</span></span>
<span data-ttu-id="f15bc-167">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f15bc-167">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="f15bc-168">Hantera åtkomst tooresources med Azure Active Directory-grupper</span><span class="sxs-lookup"><span data-stu-id="f15bc-168">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="f15bc-169">Azure Active Directory-cmdletar för att konfigurera gruppinställningar</span><span class="sxs-lookup"><span data-stu-id="f15bc-169">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="f15bc-170">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f15bc-170">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="f15bc-171">Vad är Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f15bc-171">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="f15bc-172">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f15bc-172">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
