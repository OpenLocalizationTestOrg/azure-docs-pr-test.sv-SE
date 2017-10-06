---
title: "aaaAssign en användare tooadministrator roller i Azure Active Directory | Microsoft Docs"
description: "Beskriver hur användaren toochange administrativ information i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a1ca1a53-50d8-4bf0-ae8f-73fa1253e2d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.openlocfilehash: 8bb6711c2570843962fe075b6026542a4bca525f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-a-user-tooadministrator-roles-in-azure-active-directory"></a><span data-ttu-id="8766e-103">Tilldela en användare tooadministrator roller i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8766e-103">Assign a user tooadministrator roles in Azure Active Directory</span></span>
<span data-ttu-id="8766e-104">Den här artikeln förklarar hur tooassign en administrativ roll tooa användare i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8766e-104">This article explains how tooassign an administrative role tooa user in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="8766e-105">Information om att lägga till nya användare i din organisation finns i [lägga till nya användare tooAzure Active Directory](active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8766e-105">For information about adding new users in your organization, see [Add new users tooAzure Active Directory](active-directory-users-create-azure-portal.md).</span></span> <span data-ttu-id="8766e-106">Tillagda användare har inte administratörsbehörighet som standard, men du kan tilldela roller toothem när som helst.</span><span class="sxs-lookup"><span data-stu-id="8766e-106">Added users don't have administrator permissions by default, but you can assign roles toothem at any time.</span></span>

## <a name="assign-a-role-tooa-user"></a><span data-ttu-id="8766e-107">Tilldela en roll tooa användare</span><span class="sxs-lookup"><span data-stu-id="8766e-107">Assign a role tooa user</span></span>
1. <span data-ttu-id="8766e-108">Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="8766e-108">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="8766e-109">Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="8766e-109">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Öppna användarhantering](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)
3. <span data-ttu-id="8766e-111">På hello **användare och grupper** bladet väljer **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="8766e-111">On hello **Users and groups** blade, select **All users**.</span></span>

   ![Öppna hello bladet för alla användare](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)
4. <span data-ttu-id="8766e-113">På hello **användare och grupper – alla användare** bladet Välj en användare hello listan.</span><span class="sxs-lookup"><span data-stu-id="8766e-113">On hello **Users and groups - All users** blade, select a user from hello list.</span></span>
5. <span data-ttu-id="8766e-114">På hello bladet för valda hello-användaren väljer **Directory rollen**, och tilldela sedan hello användarrollen tooa hello **Directory rollen** lista.</span><span class="sxs-lookup"><span data-stu-id="8766e-114">On hello blade for hello selected user, select **Directory role**, and then assign hello user tooa role from hello **Directory role** list.</span></span> <span data-ttu-id="8766e-115">Mer information om användar- och administratörsroller finns i [Tilldela administratörsroller i Azure AD](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="8766e-115">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>

      ![Tilldela en användarroll för tooa](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)
6. <span data-ttu-id="8766e-117">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8766e-117">Select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8766e-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8766e-118">Next steps</span></span>
* [<span data-ttu-id="8766e-119">Lägga till en användare</span><span class="sxs-lookup"><span data-stu-id="8766e-119">Add a user</span></span>](active-directory-users-create-azure-portal.md)
* [<span data-ttu-id="8766e-120">Återställa en användares lösenord i hello nya Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8766e-120">Reset a user's password in hello new Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
* [<span data-ttu-id="8766e-121">Ändra en användares arbetsinformation</span><span class="sxs-lookup"><span data-stu-id="8766e-121">Change a user's work information</span></span>](active-directory-users-work-info-azure-portal.md)
* [<span data-ttu-id="8766e-122">Hantera användarprofiler</span><span class="sxs-lookup"><span data-stu-id="8766e-122">Manage user profiles</span></span>](active-directory-users-profile-azure-portal.md)
* [<span data-ttu-id="8766e-123">Tar bort en användare i din Azure AD</span><span class="sxs-lookup"><span data-stu-id="8766e-123">Delete a user in your Azure AD</span></span>](active-directory-users-delete-user-azure-portal.md)
