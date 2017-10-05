---
title: Hantera grupper i Azure Active Directory | Microsoft Docs
description: "Så här skapar och hanterar du grupper för att hantera Azure med Azure Active Directory-användare."
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
ms.openlocfilehash: 2cc2b63312b331a19c61cd7b59a4cac78edf32e6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="managing-groups-in-azure-active-directory"></a><span data-ttu-id="08190-103">Hantera grupper i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="08190-103">Managing groups in Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="08190-104">Azure-portal</span><span class="sxs-lookup"><span data-stu-id="08190-104">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="08190-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="08190-105">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="08190-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="08190-106">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="08190-107">En av användarhanteringsfunktionerna i Azure Active Directory (AD Azure) är möjligheten att skapa grupper av användare.</span><span class="sxs-lookup"><span data-stu-id="08190-107">One of the features of Azure Active Directory (Azure AD) user management is the ability to create groups of users.</span></span> <span data-ttu-id="08190-108">Du kan använda en grupp för att utföra hanteringsuppgifter som att tilldela licenser eller behörigheter till flera användare på en gång.</span><span class="sxs-lookup"><span data-stu-id="08190-108">You use a group to perform management tasks such as assigning licenses or permissions to a number of users at once.</span></span> <span data-ttu-id="08190-109">Du kan också använda grupper för att ge behörighet till</span><span class="sxs-lookup"><span data-stu-id="08190-109">You can also use groups to assign access permission to</span></span>

* <span data-ttu-id="08190-110">resurser, till exempel objekt i katalogen,</span><span class="sxs-lookup"><span data-stu-id="08190-110">Resources such as objects in the directory</span></span>
* <span data-ttu-id="08190-111">Resurser som är externa för katalogen, t.ex. SaaS-program, Azure-tjänster, SharePoint-webbplatser eller lokala resurser</span><span class="sxs-lookup"><span data-stu-id="08190-111">Resources external to the directory such as SaaS applications, Azure services, SharePoint sites, or on-premises resources</span></span>

<span data-ttu-id="08190-112">En resursägare kan dessutom ge en Azure AD-grupp som ägs av någon annan åtkomst till en resurs.</span><span class="sxs-lookup"><span data-stu-id="08190-112">In addition, a resource owner can also assign access to a resource to an Azure AD group owned by someone else.</span></span> <span data-ttu-id="08190-113">Denna tilldelning ger medlemmarna i den aktuella gruppen åtkomst till resursen.</span><span class="sxs-lookup"><span data-stu-id="08190-113">This assignment grants the members of that group access to the resource.</span></span> <span data-ttu-id="08190-114">Gruppens ägare hanterar sedan medlemskap i gruppen.</span><span class="sxs-lookup"><span data-stu-id="08190-114">Then, the owner of the group manages membership in the group.</span></span> <span data-ttu-id="08190-115">I själva verket delegerar resursägaren behörighet till gruppens ägare att tilldela användare till resursen.</span><span class="sxs-lookup"><span data-stu-id="08190-115">Effectively, the resource owner delegates to the owner of the group the permission to assign users to their resource.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="08190-116">Microsoft rekommenderar att du hanterar Azure AD via [Azure AD administratörscenter](https://aad.portal.azure.com) på Azure Portal istället för via den klassiska Azure-portalen som nämns i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="08190-116">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> <span data-ttu-id="08190-117">Information om hur du hanterar grupper i Azure AD administratörscenter finns i [Create a group and add members in Azure Active Directory](active-directory-groups-create-azure-portal.md) (Skapa en grupp och lägg till medlemmar i Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="08190-117">For how to manage groups in the Azure AD admin center, see [Create a group and add members in Azure Active Directory](active-directory-groups-create-azure-portal.md).</span></span>

## <a name="how-do-i-create-a-group"></a><span data-ttu-id="08190-118">Hur skapar jag en grupp?</span><span class="sxs-lookup"><span data-stu-id="08190-118">How do I create a group?</span></span>
<span data-ttu-id="08190-119">Beroende på vilka tjänster som din organisation prenumererar på, kan du skapa en grupp med något av följande:</span><span class="sxs-lookup"><span data-stu-id="08190-119">Depending on the services to which your organization has subscribed, you can create a group using one of the following:</span></span>

* <span data-ttu-id="08190-120">den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="08190-120">the Azure classic portal</span></span>
* <span data-ttu-id="08190-121">Office 365-kontoportalen</span><span class="sxs-lookup"><span data-stu-id="08190-121">the Office 365 account portal</span></span>
* <span data-ttu-id="08190-122">Windows Intune-kontoportalen</span><span class="sxs-lookup"><span data-stu-id="08190-122">the Windows Intune account portal</span></span>

<span data-ttu-id="08190-123">Vi kommer att beskriva uppgifter som de genomförs på den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="08190-123">We'll describe tasks as performed in the Azure classic portal.</span></span> <span data-ttu-id="08190-124">Mer information om hur du hanterar din Azure AD-katalog med hjälp av andra portaler än Azure-portalen finns i [Administrera Azure AD-katalogen](active-directory-administer.md).</span><span class="sxs-lookup"><span data-stu-id="08190-124">For more information about using non-Azure portals to manage your Azure AD directory, see [Administering your Azure AD directory](active-directory-administer.md).</span></span>

1. <span data-ttu-id="08190-125">På [den klassiska Azure-portalen](https://manage.windowsazure.com) väljer du först **Active Directory** och sedan namnet på din organisations katalog.</span><span class="sxs-lookup"><span data-stu-id="08190-125">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select the name of the directory for your organization.</span></span>
2. <span data-ttu-id="08190-126">Välj fliken **Grupper**.</span><span class="sxs-lookup"><span data-stu-id="08190-126">Select the **Groups** tab.</span></span>
3. <span data-ttu-id="08190-127">Välj **Lägg till grupp**.</span><span class="sxs-lookup"><span data-stu-id="08190-127">Select **Add Group**.</span></span>
4. <span data-ttu-id="08190-128">I fönstret **Lägg till grupp** anger du namnet och beskrivningen för en grupp.</span><span class="sxs-lookup"><span data-stu-id="08190-128">In the **Add Group** window, specify the name and the description of a group.</span></span>

## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a><span data-ttu-id="08190-129">Hur lägger jag till eller tar bort enskilda användare i en säkerhetsgrupp?</span><span class="sxs-lookup"><span data-stu-id="08190-129">How do I add or remove individual users in a security group?</span></span>
<span data-ttu-id="08190-130">**Så här lägger du till en enskild användare till en grupp**</span><span class="sxs-lookup"><span data-stu-id="08190-130">**To add an individual user to a group**</span></span>

1. <span data-ttu-id="08190-131">På [den klassiska Azure-portalen](https://manage.windowsazure.com) väljer du först **Active Directory** och sedan namnet på din organisations katalog.</span><span class="sxs-lookup"><span data-stu-id="08190-131">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select the name of the directory for your organization.</span></span>
2. <span data-ttu-id="08190-132">Välj fliken **Grupper**.</span><span class="sxs-lookup"><span data-stu-id="08190-132">Select the **Groups** tab.</span></span>
3. <span data-ttu-id="08190-133">Öppna den grupp som du vill lägga till medlemmar i.</span><span class="sxs-lookup"><span data-stu-id="08190-133">Open the group to which you want to add members.</span></span> <span data-ttu-id="08190-134">Öppna fliken **Medlemmar** för den valda gruppen, såvida den inte redan visas.</span><span class="sxs-lookup"><span data-stu-id="08190-134">Open the **Members** tab of the selected group if it not already displaying.</span></span>
4. <span data-ttu-id="08190-135">Välj **Lägg till medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="08190-135">Select **Add Members**.</span></span>
5. <span data-ttu-id="08190-136">Välj namnet på den användare eller grupp som du vill lägga till som en medlem i den här gruppen på sidan **Lägg till medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="08190-136">On the **Add Members** page, select the name of the user or a group that you want to add as a member of this group.</span></span> <span data-ttu-id="08190-137">Kontrollera att det här namnet verkligen läggs till i fönstret **Valda**.</span><span class="sxs-lookup"><span data-stu-id="08190-137">Make sure that this name is added to the **Selected** pane.</span></span>

<span data-ttu-id="08190-138">**Så här tar du bort en användare från en grupp**</span><span class="sxs-lookup"><span data-stu-id="08190-138">**To remove an individual user from a group**</span></span>

1. <span data-ttu-id="08190-139">På [den klassiska Azure-portalen](https://manage.windowsazure.com) väljer du först **Active Directory** och sedan namnet på din organisations katalog.</span><span class="sxs-lookup"><span data-stu-id="08190-139">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select the name of the directory for your organization.</span></span>
2. <span data-ttu-id="08190-140">Välj fliken **Grupper**.</span><span class="sxs-lookup"><span data-stu-id="08190-140">Select the **Groups** tab.</span></span>
3. <span data-ttu-id="08190-141">Öppna den grupp som du vill ta bort medlemmar från.</span><span class="sxs-lookup"><span data-stu-id="08190-141">Open the group from which you want to remove members.</span></span>
4. <span data-ttu-id="08190-142">Välj fliken **Medlemmar**, välj namnet på medlemmen som du vill ta bort från den här gruppen och klicka sedan på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="08190-142">Select the **Members** tab, select the name of the member that you want to remove from this group, and then click **Remove**.</span></span>
5. <span data-ttu-id="08190-143">Bekräfta att du vill ta bort medlemmen från gruppen när du uppmanas att göra det.</span><span class="sxs-lookup"><span data-stu-id="08190-143">Confirm at the prompt that you want to remove this member from the group.</span></span>

## <a name="how-can-i-manage-the-membership-of-a-group-dynamically"></a><span data-ttu-id="08190-144">Hur kan jag hantera medlemskapet för en grupp dynamiskt?</span><span class="sxs-lookup"><span data-stu-id="08190-144">How can I manage the membership of a group dynamically?</span></span>
<span data-ttu-id="08190-145">I Azure AD kan du enkelt skapa en enkel regel för att avgöra vilka användare som ska vara medlemmar i gruppen.</span><span class="sxs-lookup"><span data-stu-id="08190-145">In Azure AD, you can very easily set up a simple rule to determine which users are to be members of the group.</span></span> <span data-ttu-id="08190-146">En enkel regel är en regel som bara gör en jämförelse.</span><span class="sxs-lookup"><span data-stu-id="08190-146">A simple rule is one that makes only a single comparison.</span></span> <span data-ttu-id="08190-147">Om ett SaaS-program t.ex. har tilldelats en grupp, kan du konfigurera en regel som lägger till användare som har befattningen Försäljare.</span><span class="sxs-lookup"><span data-stu-id="08190-147">For example, if a group is assigned to a SaaS application, you can set up a rule to add users who have a job title of "Sales Rep."</span></span> <span data-ttu-id="08190-148">Den här regeln beviljar sedan åtkomst till det här SaaS-programmet för alla användare med denna befattning i katalogen.</span><span class="sxs-lookup"><span data-stu-id="08190-148">This rule then grants access to this SaaS application to all users with that job title in your directory.</span></span>

<span data-ttu-id="08190-149">När ett attribut för en användare ändras utvärderar systemet alla dynamiska gruppregler i en katalog för att se om attributändringen ska utlösa grupptillägg eller gruppborttagningar.</span><span class="sxs-lookup"><span data-stu-id="08190-149">When any attributes of a user change, the system evaluates all dynamic group rules in a directory to see if the attribute change of the user would trigger any group adds or removes.</span></span> <span data-ttu-id="08190-150">Om användaren uppfyller en regel i en grupp läggs användaren till som medlem i gruppen.</span><span class="sxs-lookup"><span data-stu-id="08190-150">If a user satisfies a rule on a group, they are added as a member to that group.</span></span> <span data-ttu-id="08190-151">Om användaren inte längre uppfyller regeln i en grupp som han eller hon är medlem i tas användaren bort som medlem i gruppen.</span><span class="sxs-lookup"><span data-stu-id="08190-151">If they no longer satisfy the rule of a group they are a member of, they are removed as a members from that group.</span></span>

> [!NOTE]
> <span data-ttu-id="08190-152">Du kan skapa en regel för dynamiskt medlemskap för säkerhetsgrupper eller Office 365-grupper.</span><span class="sxs-lookup"><span data-stu-id="08190-152">You can set up a rule for dynamic membership on security groups or Office 365 groups.</span></span> <span data-ttu-id="08190-153">Kapslade gruppmedlemskap stöds för närvarande inte för gruppbaserad tilldelning till program.</span><span class="sxs-lookup"><span data-stu-id="08190-153">Nested group memberships aren't currently supported for group-based assignment to applications.</span></span>
>
> <span data-ttu-id="08190-154">Dynamiskt medlemskap för grupper kräver en Azure AD Premium-licens som tilldelas till</span><span class="sxs-lookup"><span data-stu-id="08190-154">Dynamic memberships for groups require an Azure AD Premium license to be assigned to</span></span>
>
> * <span data-ttu-id="08190-155">administratören som hanterar regeln för en grupp</span><span class="sxs-lookup"><span data-stu-id="08190-155">The administrator who manages the rule on a group</span></span>
> * <span data-ttu-id="08190-156">Alla medlemmar i gruppen</span><span class="sxs-lookup"><span data-stu-id="08190-156">All members of the group</span></span>
>
>

<span data-ttu-id="08190-157">**Så här aktiverar du dynamiskt medlemskap för en grupp**</span><span class="sxs-lookup"><span data-stu-id="08190-157">**To enable dynamic membership for a group**</span></span>

1. <span data-ttu-id="08190-158">På [den klassiska Azure-portalen](https://manage.windowsazure.com) väljer du först **Active Directory** och sedan namnet på din organisations katalog.</span><span class="sxs-lookup"><span data-stu-id="08190-158">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select the name of the directory for your organization.</span></span>
2. <span data-ttu-id="08190-159">Välj fliken **Grupper** och öppna den grupp som du vill redigera.</span><span class="sxs-lookup"><span data-stu-id="08190-159">Select the **Groups** tab, and open the group you want to edit.</span></span>
3. <span data-ttu-id="08190-160">Välj fliken **Konfigurera** och ändra **Aktivera dynamiskt medlemskap** till **Ja**.</span><span class="sxs-lookup"><span data-stu-id="08190-160">Select the **Configure** tab, and then set **Enable Dynamic Memberships** to **Yes**.</span></span>
4. <span data-ttu-id="08190-161">Skapa en enkel regel för gruppen som avgör hur dynamiskt medlemskap för den här gruppen ska fungera.</span><span class="sxs-lookup"><span data-stu-id="08190-161">Set up a simple single rule for the group to control how dynamic membership for this group functions.</span></span> <span data-ttu-id="08190-162">Kontrollera att alternativet **Lägg till användare där** är valt och välj sedan en användaregenskap i listan (till exempel avdelning, befattning osv.)</span><span class="sxs-lookup"><span data-stu-id="08190-162">Make sure the **Add users where** option is selected, and then select a user property from the list (for example, department, jobTitle, etc.),</span></span>
5. <span data-ttu-id="08190-163">Välj ett villkor (inte lika med, lika med, börjar inte med, börjar med, innehåller ej, innehåller, matchar inte, matchar).</span><span class="sxs-lookup"><span data-stu-id="08190-163">Next, select a condition (Not Equals, Equals, Not Starts With, Starts With, Not Contains, Contains, Not Match, Match).</span></span>
6. <span data-ttu-id="08190-164">Ange ett jämförelsevärde för den valda användaregenskapen.</span><span class="sxs-lookup"><span data-stu-id="08190-164">Specify a comparison value for the selected user property.</span></span>

<span data-ttu-id="08190-165">Mer information om hur du skapar *avancerade* regler (regler som kan innehålla flera jämförelser) för dynamiska gruppmedlemskap finns i [Använda attribut för att skapa avancerade regler](active-directory-accessmanagement-groups-with-advanced-rules.md).</span><span class="sxs-lookup"><span data-stu-id="08190-165">To learn about how to create *advanced* rules (rules that can contain multiple comparisons) for dynamic group membership, see [Using attributes to create advanced rules](active-directory-accessmanagement-groups-with-advanced-rules.md).</span></span>

## <a name="additional-information"></a><span data-ttu-id="08190-166">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="08190-166">Additional information</span></span>
<span data-ttu-id="08190-167">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="08190-167">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="08190-168">Hantera åtkomst till resurser med Azure Active Directory-grupper</span><span class="sxs-lookup"><span data-stu-id="08190-168">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="08190-169">Azure Active Directory-cmdletar för att konfigurera gruppinställningar</span><span class="sxs-lookup"><span data-stu-id="08190-169">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="08190-170">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="08190-170">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="08190-171">Vad är Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="08190-171">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="08190-172">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="08190-172">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
