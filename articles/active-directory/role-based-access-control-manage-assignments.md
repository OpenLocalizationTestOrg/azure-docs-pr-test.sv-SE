---
title: "aaaView Azure-resurs åtkomst tilldelningar | Microsoft Docs"
description: "Visa och hantera alla hello rollbaserad åtkomstkontroll tilldelningar för alla användare eller grupp i hello Azure-portalen"
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
editor: jeffsta
ms.assetid: e6f9e657-8ee3-4eec-a21c-78fe1b52a005
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: andredm
ms.openlocfilehash: ec96c9d4b9e2cb4925949b8bac78767bd564c8ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-access-assignments-for-users-and-groups-in-hello-azure-portal"></a><span data-ttu-id="f7ccd-103">Visa åtkomst-tilldelningar för användare och grupper i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f7ccd-103">View access assignments for users and groups in hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f7ccd-104">Hantera åtkomst enligt användare eller grupp</span><span class="sxs-lookup"><span data-stu-id="f7ccd-104">Manage access by user or group</span></span>](role-based-access-control-manage-assignments.md)
> * [<span data-ttu-id="f7ccd-105">Hantera åtkomst enligt resurs</span><span class="sxs-lookup"><span data-stu-id="f7ccd-105">Manage access by resource</span></span>](role-based-access-control-configure.md)

<span data-ttu-id="f7ccd-106">Med rollbaserad åtkomstkontroll (RBAC) i hello Azure Active Directory (Azure AD), kan du hantera åtkomst tooyour Azure resurser.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-106">With role-based access control (RBAC) in hello Azure Active Directory (Azure AD), you can manage access tooyour Azure resources.</span></span> 

<span data-ttu-id="f7ccd-107">Åtkomst tilldelas med RBAC är detaljerad eftersom du kan begränsa hello behörigheter på två sätt:</span><span class="sxs-lookup"><span data-stu-id="f7ccd-107">Access assigned with RBAC is fine-grained because there are two ways you can restrict hello permissions:</span></span>

* <span data-ttu-id="f7ccd-108">**Omfattning:** RBAC rolltilldelningar är begränsade tooa specifika prenumerationen, resursgruppen eller resursen.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-108">**Scope:** RBAC role assignments are scoped tooa specific subscription, resource group, or resource.</span></span> <span data-ttu-id="f7ccd-109">En användare får åtkomst tooa enskild resurs kan inte komma åt andra resurser i hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-109">A user given access tooa single resource cannot access any other resources in hello same subscription.</span></span>
* <span data-ttu-id="f7ccd-110">**Roll:** inom hello omfattning hello tilldelning åtkomst minskar även ytterligare genom att tilldela en roll.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-110">**Role:** Within hello scope of hello assignment, access is narrowed even further by assigning a role.</span></span> <span data-ttu-id="f7ccd-111">Roller kan vara hög nivå som ägare eller specifika som virtuella läsare.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-111">Roles can be high-level, like owner, or specific, like virtual machine reader.</span></span>

<span data-ttu-id="f7ccd-112">Roller kan bara tilldelas från inom hello prenumeration, resursgrupp eller resurs som hello omfånget för hello tilldelning.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-112">Roles can only be assigned from within hello subscription, resource group, or resource that is hello scope for hello assignment.</span></span> <span data-ttu-id="f7ccd-113">Men du kan visa alla hello åtkomst tilldelningar för en viss användare eller grupp i en enda plats.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-113">But you can view all hello access assignments for a given user or group in a single place.</span></span> <span data-ttu-id="f7ccd-114">Du kan ha upp too2000 rolltilldelningar i varje prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-114">You can have up too2000 role assignments in each subscription.</span></span> 

<span data-ttu-id="f7ccd-115">Hämta mer information om hur för[använda rollen tilldelningar toomanage åtkomst tooyour Azure-prenumerationsresurser](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="f7ccd-115">Get more information about how too[Use role assignments toomanage access tooyour Azure subscription resources](role-based-access-control-configure.md).</span></span>

## <a name="view-access-assignments"></a><span data-ttu-id="f7ccd-116">Visa åtkomst tilldelningar</span><span class="sxs-lookup"><span data-stu-id="f7ccd-116">View access assignments</span></span>
<span data-ttu-id="f7ccd-117">toolook in hello åtkomst tilldelningar för en enskild användare eller grupp, startar i Azure Active Directory i hello [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f7ccd-117">toolook up hello access assignments for a single user or group, start in Azure Active Directory in hello [Azure portal](http://portal.azure.com).</span></span>

1. <span data-ttu-id="f7ccd-118">Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-118">Select **Azure Active Directory**.</span></span> <span data-ttu-id="f7ccd-119">Om det här alternativet inte visas i listan navigering väljer **fler tjänster** och bläddrar sedan nedåt toofind **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-119">If this option is not visible on your navigation list, select **More Services** and then scroll down toofind **Azure Active Directory**.</span></span>
2. <span data-ttu-id="f7ccd-120">Välj **användare och grupper**, och sedan antingen **alla användare** eller **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-120">Select **Users and Groups**, and then either **All users** or **All groups**.</span></span> <span data-ttu-id="f7ccd-121">I det här exemplet fokuserar vi på enskilda användare.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-121">For this example, we focus on individual users.</span></span>
    <span data-ttu-id="f7ccd-122">![Hantera användare och grupper i Azure Active Directory – skärmbild](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)</span><span class="sxs-lookup"><span data-stu-id="f7ccd-122">![Manage users and groups in Azure Active Directory - screenshot](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)</span></span>
3. <span data-ttu-id="f7ccd-123">Sök efter hello användare efter namn eller användarnamn.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-123">Search for hello user by name or username.</span></span>
4. <span data-ttu-id="f7ccd-124">Välj **Azure-resurser** hello användaren bladet.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-124">Select **Azure resources** on hello user blade.</span></span> <span data-ttu-id="f7ccd-125">Alla hello åtkomst tilldelningar för användaren visas.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-125">All hello access assignments for that user appear.</span></span>

### <a name="read-permissions-tooview-assignments"></a><span data-ttu-id="f7ccd-126">Läsbehörighet tooview tilldelningar</span><span class="sxs-lookup"><span data-stu-id="f7ccd-126">Read permissions tooview assignments</span></span>
<span data-ttu-id="f7ccd-127">Den här sidan visar endast hello åtkomst tilldelningar att du har behörighet tooread.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-127">This page only shows hello access assignments that you have permission tooread.</span></span> <span data-ttu-id="f7ccd-128">Till exempel har läsbehörighet toosubscription A och gå toohello Azure-resurser bladet toocheck tilldelningar för en användare.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-128">For example, you have read access toosubscription A and go toohello Azure resources blade toocheck a user's assignments.</span></span> <span data-ttu-id="f7ccd-129">Du kan se sin åtkomst tilldelningar för prenumerationen A, men inte kan se att hon också har åtkomst tilldelningar för prenumerationen B.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-129">You can see her access assignments for subscription A, but can't see that she also has access assignments on subscription B.</span></span>

## <a name="delete-access-assignments"></a><span data-ttu-id="f7ccd-130">Ta bort åtkomst tilldelningar</span><span class="sxs-lookup"><span data-stu-id="f7ccd-130">Delete access assignments</span></span>
<span data-ttu-id="f7ccd-131">Från det här bladet kan du ta bort åtkomst tilldelningar som har tilldelats direkt tooa användare eller grupp.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-131">From this blade, you can delete access assignments that were assigned directly tooa user or group.</span></span> <span data-ttu-id="f7ccd-132">Om hello åtkomst tilldelning ärvdes från en överordnad grupp, måste toogo toohello resurs eller en prenumeration och hantera det hello tilldelning.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-132">If hello access assignment was inherited from a parent group, you need toogo toohello resource or subscription and manage hello assignment there.</span></span>

1. <span data-ttu-id="f7ccd-133">Hello lista över alla hello åtkomst tilldelningar för en användare eller grupp, Välj hello en du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-133">From hello list of all hello access assignments for a user or group, select hello one you want toodelete.</span></span>
2. <span data-ttu-id="f7ccd-134">Välj **ta bort** och sedan **Ja** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="f7ccd-134">Select **Remove** and then **Yes** tooconfirm.</span></span>
    <span data-ttu-id="f7ccd-135">![Ta bort åtkomst tilldelning – skärmbild](./media/role-based-access-control-manage-assignments/delete_assignment.png)</span><span class="sxs-lookup"><span data-stu-id="f7ccd-135">![Remove access assignment - screenshot](./media/role-based-access-control-manage-assignments/delete_assignment.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="f7ccd-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f7ccd-136">Next steps</span></span>

* <span data-ttu-id="f7ccd-137">Kom igång med rollbaserad åtkomstkontroll för[använda rollen tilldelningar toomanage åtkomst tooyour Azure-prenumerationsresurser](role-based-access-control-configure.md)</span><span class="sxs-lookup"><span data-stu-id="f7ccd-137">Get started with Role-Based Access Control too[Use role assignments toomanage access tooyour Azure subscription resources](role-based-access-control-configure.md)</span></span>
* <span data-ttu-id="f7ccd-138">Se hello [inbyggda RBAC-roller](role-based-access-built-in-roles.md)</span><span class="sxs-lookup"><span data-stu-id="f7ccd-138">See hello [RBAC built-in roles](role-based-access-built-in-roles.md)</span></span>

