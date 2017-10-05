---
title: "Tilldela administratörsroller i Azure Active Directory användare | Microsoft Docs"
description: "Beskriver hur du ändrar administrativa användarinformation i Azure Active Directory"
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
ms.openlocfilehash: bfadf133154488f9827cfbeaa98ddb0eb84b52f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory"></a><span data-ttu-id="330a8-103">Tilldela administratörsroller i Azure Active Directory en användare</span><span class="sxs-lookup"><span data-stu-id="330a8-103">Assign a user to administrator roles in Azure Active Directory</span></span>
<span data-ttu-id="330a8-104">Den här artikeln beskriver hur du tilldelar en administrativ roll till en användare i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="330a8-104">This article explains how to assign an administrative role to a user in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="330a8-105">Information om att lägga till nya användare i din organisation finns i [lägga till nya användare i Azure Active Directory](active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="330a8-105">For information about adding new users in your organization, see [Add new users to Azure Active Directory](active-directory-users-create-azure-portal.md).</span></span> <span data-ttu-id="330a8-106">Tillagda användare har inte administratörsbehörighet som standard, men du kan tilldela roller till dem när som helst.</span><span class="sxs-lookup"><span data-stu-id="330a8-106">Added users don't have administrator permissions by default, but you can assign roles to them at any time.</span></span>

## <a name="assign-a-role-to-a-user"></a><span data-ttu-id="330a8-107">Tilldela en roll till en användare</span><span class="sxs-lookup"><span data-stu-id="330a8-107">Assign a role to a user</span></span>
1. <span data-ttu-id="330a8-108">Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="330a8-108">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="330a8-109">Välj **fler tjänster**, ange **användare och grupper** i textrutan och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="330a8-109">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Öppna användarhantering](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)
3. <span data-ttu-id="330a8-111">På den **användare och grupper** bladet väljer **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="330a8-111">On the **Users and groups** blade, select **All users**.</span></span>

   ![Öppna bladet för alla användare](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)
4. <span data-ttu-id="330a8-113">På den **användare och grupper – alla användare** bladet Välj en användare i listan.</span><span class="sxs-lookup"><span data-stu-id="330a8-113">On the **Users and groups - All users** blade, select a user from the list.</span></span>
5. <span data-ttu-id="330a8-114">På bladet för den valda användaren väljer **Directory rollen**, och tilldelar sedan användaren till en roll från den **Directory rollen** lista.</span><span class="sxs-lookup"><span data-stu-id="330a8-114">On the blade for the selected user, select **Directory role**, and then assign the user to a role from the **Directory role** list.</span></span> <span data-ttu-id="330a8-115">Mer information om användar- och administratörsroller finns i [Tilldela administratörsroller i Azure AD](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="330a8-115">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>

      ![Tilldela en användare till en roll](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)
6. <span data-ttu-id="330a8-117">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="330a8-117">Select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="330a8-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="330a8-118">Next steps</span></span>
* [<span data-ttu-id="330a8-119">Lägga till en användare</span><span class="sxs-lookup"><span data-stu-id="330a8-119">Add a user</span></span>](active-directory-users-create-azure-portal.md)
* [<span data-ttu-id="330a8-120">Återställa en användares lösenord i den nya Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="330a8-120">Reset a user's password in the new Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
* [<span data-ttu-id="330a8-121">Ändra en användares arbetsinformation</span><span class="sxs-lookup"><span data-stu-id="330a8-121">Change a user's work information</span></span>](active-directory-users-work-info-azure-portal.md)
* [<span data-ttu-id="330a8-122">Hantera användarprofiler</span><span class="sxs-lookup"><span data-stu-id="330a8-122">Manage user profiles</span></span>](active-directory-users-profile-azure-portal.md)
* [<span data-ttu-id="330a8-123">Tar bort en användare i din Azure AD</span><span class="sxs-lookup"><span data-stu-id="330a8-123">Delete a user in your Azure AD</span></span>](active-directory-users-delete-user-azure-portal.md)
