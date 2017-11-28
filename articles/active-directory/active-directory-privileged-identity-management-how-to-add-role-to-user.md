---
title: "Lägga till eller ta bort en användarroll | Microsoft Docs"
description: "Lär dig mer om att lägga till roller för privilegierade identiteter med Azure Active Directory Privileged Identity Management-programmet."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 6a47ced8-cf34-4ce8-bea2-e4fc548cfe22
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim;oldportal;it-pro;
ms.openlocfilehash: 3ac07bb7b070f44595c099a454b3d0dbc66126c9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a><span data-ttu-id="f352b-103">Azure AD Privileged Identity Management, lägg till eller ta bort en användarroll</span><span class="sxs-lookup"><span data-stu-id="f352b-103">Azure AD Privileged Identity Management: How to add or remove a user role</span></span>
<span data-ttu-id="f352b-104">Med Azure Active Directory (AD), en global administratör (eller företagets administratör) kan uppdatera som användare kan **permanent** tilldelade roller i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f352b-104">With Azure Active Directory (AD), a global administrator (or company administrator) can update which users are **permanently** assigned to roles in Azure AD.</span></span> <span data-ttu-id="f352b-105">Detta görs med PowerShell-cmdlets som `Add-MsolRoleMember` och `Remove-MsolRoleMember`.</span><span class="sxs-lookup"><span data-stu-id="f352b-105">This is done with PowerShell cmdlets like `Add-MsolRoleMember` and `Remove-MsolRoleMember`.</span></span> <span data-ttu-id="f352b-106">De kan också använda den klassiska Azure-portalen enligt beskrivningen i [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="f352b-106">Or they can use the Azure classic portal as described in [assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles.md).</span></span>

<span data-ttu-id="f352b-107">Programmet Azure AD Privileged Identity Management administratörer Privilegierade roller kan göra permanenta rolltilldelningar samt.</span><span class="sxs-lookup"><span data-stu-id="f352b-107">The Azure AD Privileged Identity Management application allows privileged role administrators to make permanent role assignments, as well.</span></span> <span data-ttu-id="f352b-108">Dessutom Privilegierade rollen administratörer kan utföra **berättigade** för administratörsroller.</span><span class="sxs-lookup"><span data-stu-id="f352b-108">Additionally, privileged role administrators can make users **eligible** for admin roles.</span></span> <span data-ttu-id="f352b-109">En berättigad administratör kan aktivera rollen när de behöver den och sedan deras behörigheter ut när de är klar.</span><span class="sxs-lookup"><span data-stu-id="f352b-109">An eligible admin can activate the role when they need it, and then their permissions expire once they're done.</span></span>

## <a name="manage-roles-with-pim-in-the-azure-portal"></a><span data-ttu-id="f352b-110">Hantera roller med PIM i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f352b-110">Manage roles with PIM in the Azure portal</span></span>
<span data-ttu-id="f352b-111">I din organisation, kan du tilldela användare för olika administrativa roller i Azure AD och Office 365 och andra Microsoft-tjänster och program.</span><span class="sxs-lookup"><span data-stu-id="f352b-111">In your organization, you can assign users to different administrative roles in Azure AD, Office 365, and other Microsoft services and applications.</span></span>  <span data-ttu-id="f352b-112">Mer information om tillgängliga roller kan hittas på [roller i Azure AD PIM](active-directory-privileged-identity-management-roles.md).</span><span class="sxs-lookup"><span data-stu-id="f352b-112">More details on the available roles can be found at [Roles in Azure AD PIM](active-directory-privileged-identity-management-roles.md).</span></span>

<span data-ttu-id="f352b-113">Om du vill lägga till eller ta bort en användare i en roll med hjälp av Privileged Identity Management, visa PIM-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="f352b-113">To add or remove a user in a role using Privileged Identity Management, bring up the PIM dashboard.</span></span> <span data-ttu-id="f352b-114">Klicka sedan på den **användare i administratörsroller** knappen eller välj en specifik roll (till exempel Global administratör) från tabellen roller.</span><span class="sxs-lookup"><span data-stu-id="f352b-114">Then either click the **Users in Admin Roles** button, or select a specific role (such as Global Administrator) from the roles table.</span></span>

> [!NOTE]
> <span data-ttu-id="f352b-115">Om du inte har aktiverat PIM i Azure portal ännu, går du till [Kom igång med Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="f352b-115">If you haven't enabled PIM in the Azure portal yet, go to [Get started with Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) for details.</span></span>

<span data-ttu-id="f352b-116">Om du vill ge en annan användare tillgång till PIM själva roller som PIM kräver att användaren har beskrivs ytterligare i [ge åtkomst till PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span><span class="sxs-lookup"><span data-stu-id="f352b-116">If you want to give another user access to PIM itself, the roles which PIM requires the user to have are described further in [how to give access to PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span></span>

## <a name="add-a-user-to-a-role"></a><span data-ttu-id="f352b-117">Lägga till en användare till en roll</span><span class="sxs-lookup"><span data-stu-id="f352b-117">Add a user to a role</span></span>
1. <span data-ttu-id="f352b-118">I den [Azure-portalen](https://portal.azure.com/), Välj den **Azure AD Privileged Identity Management** panelen på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="f352b-118">In the [Azure portal](https://portal.azure.com/), select the **Azure AD Privileged Identity Management** tile on the dashboard.</span></span>
2. <span data-ttu-id="f352b-119">Välj **hantera Privilegierade roller**.</span><span class="sxs-lookup"><span data-stu-id="f352b-119">Select **Manage privileged roles**.</span></span>
3. <span data-ttu-id="f352b-120">I den **Rollsammanfattning** tabell, väljer du den roll som du vill hantera.</span><span class="sxs-lookup"><span data-stu-id="f352b-120">In the **Role summary** table, select the role you want to manage.</span></span>
4. <span data-ttu-id="f352b-121">I bladet roll väljer **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f352b-121">In the role blade, select **Add**.</span></span>
5. <span data-ttu-id="f352b-122">Klicka på **Välj användare** och Sök efter användare på den **Välj användare** bladet.</span><span class="sxs-lookup"><span data-stu-id="f352b-122">Click **Select users** and search for the user on the **Select users** blade.</span></span>  
6. <span data-ttu-id="f352b-123">Välj användaren från listan över sökresultat och klicka på **klar**.</span><span class="sxs-lookup"><span data-stu-id="f352b-123">Select the user from the search results list, and click **Done**.</span></span>
7. <span data-ttu-id="f352b-124">Klicka på **OK** att spara ditt val.</span><span class="sxs-lookup"><span data-stu-id="f352b-124">Click **OK** to save your selection.</span></span> <span data-ttu-id="f352b-125">Användare som du har valt visas i listan som är berättigade till rollen.</span><span class="sxs-lookup"><span data-stu-id="f352b-125">The user you have selected will appear in the list as eligible for the role.</span></span>

> [!NOTE]
> <span data-ttu-id="f352b-126">Nya användare i en roll är bara tillgängliga för rollen som standard.</span><span class="sxs-lookup"><span data-stu-id="f352b-126">New users in a role are only eligible for the role by default.</span></span> <span data-ttu-id="f352b-127">Klicka på användare i listan om du vill att rollen permanent.</span><span class="sxs-lookup"><span data-stu-id="f352b-127">If you want to make the role permanent, click the user in the list.</span></span> <span data-ttu-id="f352b-128">Användarens information visas i ett nytt blad.</span><span class="sxs-lookup"><span data-stu-id="f352b-128">The user's information will appear in a new blade.</span></span> <span data-ttu-id="f352b-129">Välj **Se behörighet** i menyn användaren information.</span><span class="sxs-lookup"><span data-stu-id="f352b-129">Select **Make perm** in the user information menu.</span></span>  
> <span data-ttu-id="f352b-130">Om en användare kan registrera dig för Azure Multi-Factor Authentication (MFA) eller använder ett Microsoft-konto (vanligtvis @outlook.com), måste du se dem permanent i deras roller.</span><span class="sxs-lookup"><span data-stu-id="f352b-130">If a user cannot register for Azure Multi-Factor Authentication (MFA), or is using a Microsoft account (usually @outlook.com), you need to make them permanent in all their roles.</span></span> <span data-ttu-id="f352b-131">Berättigad administratörer uppmanas att registrera sig för MFA vid aktivering.</span><span class="sxs-lookup"><span data-stu-id="f352b-131">Eligible admins are asked to register for MFA during activation.</span></span>

<span data-ttu-id="f352b-132">Nu när användaren är berättigad till en roll, meddela dem om att de kan aktivera enligt anvisningarna i [så här aktiverar eller inaktiverar du en roll](active-directory-privileged-identity-management-how-to-activate-role.md).</span><span class="sxs-lookup"><span data-stu-id="f352b-132">Now that the user is eligible for a role, let them know that they can activate it according to the instructions in [How to activate or deactivate a role](active-directory-privileged-identity-management-how-to-activate-role.md).</span></span>

## <a name="remove-a-user-from-a-role"></a><span data-ttu-id="f352b-133">Ta bort en användare från en roll</span><span class="sxs-lookup"><span data-stu-id="f352b-133">Remove a user from a role</span></span>
<span data-ttu-id="f352b-134">Du kan ta bort användare från berättigade rolltilldelningar, men kontrollera att det finns alltid minst en användare som är en permanent global administratör.</span><span class="sxs-lookup"><span data-stu-id="f352b-134">You can remove users from eligible role assignments, but make sure there is always at least one user who is a permanent global administrator.</span></span>

<span data-ttu-id="f352b-135">Följ dessa steg om du vill ta bort en viss användare från en roll:</span><span class="sxs-lookup"><span data-stu-id="f352b-135">Follow these steps to remove a specific user from a role:</span></span>

1. <span data-ttu-id="f352b-136">Navigera till rollen i listan roll genom att välja en roll i Azure AD PIM-instrumentpanelen eller genom att klicka på den **användare i administratörsroller** knappen.</span><span class="sxs-lookup"><span data-stu-id="f352b-136">Navigate to the role in the role list either by selecting a role in the Azure AD PIM dashboard or by clicking on the **Users in Admin Roles** button.</span></span>
2. <span data-ttu-id="f352b-137">Klicka på användare i användarlistan.</span><span class="sxs-lookup"><span data-stu-id="f352b-137">Click on the user in the user list.</span></span>
3. <span data-ttu-id="f352b-138">Klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f352b-138">Click **Remove**.</span></span> <span data-ttu-id="f352b-139">Ett meddelande som ber dig att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="f352b-139">A message will ask you to confirm.</span></span>
4. <span data-ttu-id="f352b-140">Klicka på **Ja** att ta bort rollen från användaren.</span><span class="sxs-lookup"><span data-stu-id="f352b-140">Click **Yes** to remove the role from the user.</span></span>

<span data-ttu-id="f352b-141">Om du inte vet vilka användarna måste fortfarande sina rolltilldelningar sedan kan du [startar en åtkomst-granskning för rollen](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="f352b-141">If you're not sure which users still need their role assignments, then you can [start an access review for the role](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f352b-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f352b-142">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

