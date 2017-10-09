---
title: "aaaHow tooadd eller ta bort en användarroll | Microsoft Docs"
description: "Lär dig hur hello tooadd roller tooprivileged identiteter med Azure Active Directory Privileged Identity Management-program."
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
ms.openlocfilehash: f84639757dd76061ea12ed6ea7ec9e62ad942109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-privileged-identity-management-how-tooadd-or-remove-a-user-role"></a><span data-ttu-id="34005-103">Azure AD Privileged Identity Management: Hur tooadd eller ta bort en användarroll</span><span class="sxs-lookup"><span data-stu-id="34005-103">Azure AD Privileged Identity Management: How tooadd or remove a user role</span></span>
<span data-ttu-id="34005-104">Med Azure Active Directory (AD), en global administratör (eller företagets administratör) kan uppdatera som användare kan **permanent** tilldelade tooroles i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="34005-104">With Azure Active Directory (AD), a global administrator (or company administrator) can update which users are **permanently** assigned tooroles in Azure AD.</span></span> <span data-ttu-id="34005-105">Detta görs med PowerShell-cmdlets som `Add-MsolRoleMember` och `Remove-MsolRoleMember`.</span><span class="sxs-lookup"><span data-stu-id="34005-105">This is done with PowerShell cmdlets like `Add-MsolRoleMember` and `Remove-MsolRoleMember`.</span></span> <span data-ttu-id="34005-106">De kan också använda hello klassiska Azure-portalen enligt beskrivningen i [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="34005-106">Or they can use hello Azure classic portal as described in [assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles.md).</span></span>

<span data-ttu-id="34005-107">hello programmet Azure AD Privileged Identity Management administratörer Privilegierade roller toomake permanent rolltilldelningar samt.</span><span class="sxs-lookup"><span data-stu-id="34005-107">hello Azure AD Privileged Identity Management application allows privileged role administrators toomake permanent role assignments, as well.</span></span> <span data-ttu-id="34005-108">Dessutom Privilegierade rollen administratörer kan utföra **berättigade** för administratörsroller.</span><span class="sxs-lookup"><span data-stu-id="34005-108">Additionally, privileged role administrators can make users **eligible** for admin roles.</span></span> <span data-ttu-id="34005-109">En berättigad administratör kan aktivera hello roll när det behövs och sedan deras behörigheter ut när de är klar.</span><span class="sxs-lookup"><span data-stu-id="34005-109">An eligible admin can activate hello role when they need it, and then their permissions expire once they're done.</span></span>

## <a name="manage-roles-with-pim-in-hello-azure-portal"></a><span data-ttu-id="34005-110">Hantera roller med PIM i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="34005-110">Manage roles with PIM in hello Azure portal</span></span>
<span data-ttu-id="34005-111">I din organisation, kan du tilldela användare toodifferent administrativa roller i Azure AD och Office 365 och andra Microsoft-tjänster och program.</span><span class="sxs-lookup"><span data-stu-id="34005-111">In your organization, you can assign users toodifferent administrative roles in Azure AD, Office 365, and other Microsoft services and applications.</span></span>  <span data-ttu-id="34005-112">Mer information om hello tillgängliga roller kan hittas på [roller i Azure AD PIM](active-directory-privileged-identity-management-roles.md).</span><span class="sxs-lookup"><span data-stu-id="34005-112">More details on hello available roles can be found at [Roles in Azure AD PIM](active-directory-privileged-identity-management-roles.md).</span></span>

<span data-ttu-id="34005-113">tooadd eller ta bort en användare i en roll med hjälp av Privileged Identity Management registreringspunkter hello PIM-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="34005-113">tooadd or remove a user in a role using Privileged Identity Management, bring up hello PIM dashboard.</span></span> <span data-ttu-id="34005-114">Sedan antingen på hello **användare i administratörsroller** knappen eller väljer en viss roll (till exempel Global administratör) hello roller tabell.</span><span class="sxs-lookup"><span data-stu-id="34005-114">Then either click hello **Users in Admin Roles** button, or select a specific role (such as Global Administrator) from hello roles table.</span></span>

> [!NOTE]
> <span data-ttu-id="34005-115">Om du inte har aktiverat PIM i hello Azure-portalen ännu går för[Kom igång med Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="34005-115">If you haven't enabled PIM in hello Azure portal yet, go too[Get started with Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) for details.</span></span>

<span data-ttu-id="34005-116">Om du vill toogive till en annan användare åtkomst tooPIM hello roller som PIM kräver hello användaren toohave beskrivs ytterligare i själva [hur toogive åt tooPIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span><span class="sxs-lookup"><span data-stu-id="34005-116">If you want toogive another user access tooPIM itself, hello roles which PIM requires hello user toohave are described further in [how toogive access tooPIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span></span>

## <a name="add-a-user-tooa-role"></a><span data-ttu-id="34005-117">Lägg till en användarroll för tooa</span><span class="sxs-lookup"><span data-stu-id="34005-117">Add a user tooa role</span></span>
1. <span data-ttu-id="34005-118">I hello [Azure-portalen](https://portal.azure.com/)väljer hello **Azure AD Privileged Identity Management** rutan på hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="34005-118">In hello [Azure portal](https://portal.azure.com/), select hello **Azure AD Privileged Identity Management** tile on hello dashboard.</span></span>
2. <span data-ttu-id="34005-119">Välj **hantera Privilegierade roller**.</span><span class="sxs-lookup"><span data-stu-id="34005-119">Select **Manage privileged roles**.</span></span>
3. <span data-ttu-id="34005-120">I hello **Rollsammanfattning** tabell, Välj hello roll som du vill använda toomanage.</span><span class="sxs-lookup"><span data-stu-id="34005-120">In hello **Role summary** table, select hello role you want toomanage.</span></span>
4. <span data-ttu-id="34005-121">Hello rollen bladet välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="34005-121">In hello role blade, select **Add**.</span></span>
5. <span data-ttu-id="34005-122">Klicka på **Välj användare** och Sök efter hello användare på hello **Välj användare** bladet.</span><span class="sxs-lookup"><span data-stu-id="34005-122">Click **Select users** and search for hello user on hello **Select users** blade.</span></span>  
6. <span data-ttu-id="34005-123">Välj hello användaren hello sökresultatet och klicka på **klar**.</span><span class="sxs-lookup"><span data-stu-id="34005-123">Select hello user from hello search results list, and click **Done**.</span></span>
7. <span data-ttu-id="34005-124">Klicka på **OK** toosave valet.</span><span class="sxs-lookup"><span data-stu-id="34005-124">Click **OK** toosave your selection.</span></span> <span data-ttu-id="34005-125">hello användare som du har valt visas i hello lista som är berättigade till hello roll.</span><span class="sxs-lookup"><span data-stu-id="34005-125">hello user you have selected will appear in hello list as eligible for hello role.</span></span>

> [!NOTE]
> <span data-ttu-id="34005-126">Nya användare i en roll är bara tillgängliga för hello roll som standard.</span><span class="sxs-lookup"><span data-stu-id="34005-126">New users in a role are only eligible for hello role by default.</span></span> <span data-ttu-id="34005-127">Om du vill toomake hello rollen permanenta, klickar du på hello användare i hello listan.</span><span class="sxs-lookup"><span data-stu-id="34005-127">If you want toomake hello role permanent, click hello user in hello list.</span></span> <span data-ttu-id="34005-128">hello användarens information visas i ett nytt blad.</span><span class="sxs-lookup"><span data-stu-id="34005-128">hello user's information will appear in a new blade.</span></span> <span data-ttu-id="34005-129">Välj **Se behörighet** hello användaren information-menyn.</span><span class="sxs-lookup"><span data-stu-id="34005-129">Select **Make perm** in hello user information menu.</span></span>  
> <span data-ttu-id="34005-130">Om en användare kan registrera dig för Azure Multi-Factor Authentication (MFA) eller använder ett Microsoft-konto (vanligtvis @outlook.com), behöver du toomake dem permanent i deras roller.</span><span class="sxs-lookup"><span data-stu-id="34005-130">If a user cannot register for Azure Multi-Factor Authentication (MFA), or is using a Microsoft account (usually @outlook.com), you need toomake them permanent in all their roles.</span></span> <span data-ttu-id="34005-131">Berättigad administratörer uppmanas tooregister MFA vid aktivering.</span><span class="sxs-lookup"><span data-stu-id="34005-131">Eligible admins are asked tooregister for MFA during activation.</span></span>

<span data-ttu-id="34005-132">Nu när hello användaren är berättigad till en roll, meddela dem om att de kan aktivera den enligt toohello instruktionerna i [hur tooactivate eller inaktivera en roll](active-directory-privileged-identity-management-how-to-activate-role.md).</span><span class="sxs-lookup"><span data-stu-id="34005-132">Now that hello user is eligible for a role, let them know that they can activate it according toohello instructions in [How tooactivate or deactivate a role](active-directory-privileged-identity-management-how-to-activate-role.md).</span></span>

## <a name="remove-a-user-from-a-role"></a><span data-ttu-id="34005-133">Ta bort en användare från en roll</span><span class="sxs-lookup"><span data-stu-id="34005-133">Remove a user from a role</span></span>
<span data-ttu-id="34005-134">Du kan ta bort användare från berättigade rolltilldelningar, men kontrollera att det finns alltid minst en användare som är en permanent global administratör.</span><span class="sxs-lookup"><span data-stu-id="34005-134">You can remove users from eligible role assignments, but make sure there is always at least one user who is a permanent global administrator.</span></span>

<span data-ttu-id="34005-135">Följ dessa steg tooremove en viss användare från en roll:</span><span class="sxs-lookup"><span data-stu-id="34005-135">Follow these steps tooremove a specific user from a role:</span></span>

1. <span data-ttu-id="34005-136">Navigera toohello roll i hello rollen listan genom att välja en roll i hello Azure AD PIM instrumentpanelen eller genom att klicka på hello **användare i administratörsroller** knappen.</span><span class="sxs-lookup"><span data-stu-id="34005-136">Navigate toohello role in hello role list either by selecting a role in hello Azure AD PIM dashboard or by clicking on hello **Users in Admin Roles** button.</span></span>
2. <span data-ttu-id="34005-137">Klicka på hello användare i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="34005-137">Click on hello user in hello user list.</span></span>
3. <span data-ttu-id="34005-138">Klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="34005-138">Click **Remove**.</span></span> <span data-ttu-id="34005-139">Ett meddelande som ber dig tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="34005-139">A message will ask you tooconfirm.</span></span>
4. <span data-ttu-id="34005-140">Klicka på **Ja** tooremove hello roll från hello användare.</span><span class="sxs-lookup"><span data-stu-id="34005-140">Click **Yes** tooremove hello role from hello user.</span></span>

<span data-ttu-id="34005-141">Om du inte vet vilka användarna måste fortfarande sina rolltilldelningar sedan kan du [startar en åtkomst-granskning för hello rollen](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="34005-141">If you're not sure which users still need their role assignments, then you can [start an access review for hello role](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="34005-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="34005-142">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

