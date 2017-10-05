---
title: "Lägga till nya användare i Azure Active Directory | Microsoft Docs"
description: "Beskriver hur du lägger till nya användare eller ändrar användarinformation i Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 0a90c3c5-4e0e-43bd-a606-6ee00f163038
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand;jeffsta
ms.reviewer: jeffsta
ms.openlocfilehash: bfe0c556d94d50207a23d2e3984371fb602e9406
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-new-users-to-azure-active-directory"></a><span data-ttu-id="e5aef-103">Lägga till nya användare i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e5aef-103">Add new users to Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e5aef-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e5aef-104">Azure portal</span></span>](active-directory-users-create-azure-portal.md)
> * [<span data-ttu-id="e5aef-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="e5aef-105">Azure classic portal</span></span>](active-directory-create-users.md)
>
>

<span data-ttu-id="e5aef-106">Den här artikeln förklarar hur du lägger till nya användare i din organisation i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e5aef-106">This article explains how to add new users in your organization in the Azure Active Directory (Azure AD).</span></span> 

1. <span data-ttu-id="e5aef-107">Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="e5aef-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="e5aef-108">Välj **fler tjänster**, ange **användare och grupper** i textrutan och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="e5aef-108">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Öppna användare och grupper](./media/active-directory-users-create-azure-portal/create-users-user-management.png)
3. <span data-ttu-id="e5aef-110">På den **användare och grupper** bladet väljer **alla användare**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e5aef-110">On the **Users and groups** blade, select **All users**, and then select **Add**.</span></span>

   ![Att välja kommandot Lägg till](./media/active-directory-users-create-azure-portal/create-users-add-command.png)
4. <span data-ttu-id="e5aef-112">Ange information för användaren, till exempel **namn** och **användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="e5aef-112">Enter details for the user, such as **Name** and **User name**.</span></span> <span data-ttu-id="e5aef-113">Domännamndelen för användarnamn måste antingen vara inledande domain name ”foo.onmicrosoft.com” standarddomännamnet eller verifierad, ofedererad domännamn, till exempel ”contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="e5aef-113">The domain name portion of the user name must either be the initial default domain name "foo.onmicrosoft.com" domain name, or a verified, non-federated domain name such as "contoso.com."</span></span>
5. <span data-ttu-id="e5aef-114">Kopiera eller annars Observera genererade användarens lösenord så att du kan ange den till användaren när processen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="e5aef-114">Copy or otherwise note the generated user password so that you can provide it to the user after this process is complete.</span></span>
6. <span data-ttu-id="e5aef-115">Du kan också öppna och fylla i informationen i den **profil** bladet den **grupper** bladet eller **Directory rollen** bladet för användaren.</span><span class="sxs-lookup"><span data-stu-id="e5aef-115">Optionally, you can open and fill out the information in the **Profile** blade, the **Groups** blade, or the **Directory role** blade for the user.</span></span> <span data-ttu-id="e5aef-116">Mer information om användar- och administratörsroller finns i [Tilldela administratörsroller i Azure AD](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="e5aef-116">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>
7. <span data-ttu-id="e5aef-117">På den **användare** bladet väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="e5aef-117">On the **User** blade, select **Create**.</span></span>
8. <span data-ttu-id="e5aef-118">På ett säkert sätt distribuera genererade lösenordet till den nya användaren så att användaren kan logga in.</span><span class="sxs-lookup"><span data-stu-id="e5aef-118">Securely distribute the generated password to the new user so that the user can sign in.</span></span>

### <a name="next-steps"></a><span data-ttu-id="e5aef-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e5aef-119">Next steps</span></span>
* [<span data-ttu-id="e5aef-120">Lägg till en extern användare</span><span class="sxs-lookup"><span data-stu-id="e5aef-120">Add an external user</span></span>](active-directory-users-create-external-azure-portal.md)
* [<span data-ttu-id="e5aef-121">Återställa en användares lösenord i den nya Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e5aef-121">Reset a user's password in the new Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
* [<span data-ttu-id="e5aef-122">Ändra en användares arbetsinformation</span><span class="sxs-lookup"><span data-stu-id="e5aef-122">Change a user's work information</span></span>](active-directory-users-work-info-azure-portal.md)
* [<span data-ttu-id="e5aef-123">Hantera användarprofiler</span><span class="sxs-lookup"><span data-stu-id="e5aef-123">Manage user profiles</span></span>](active-directory-users-profile-azure-portal.md)
* [<span data-ttu-id="e5aef-124">Tar bort en användare i din Azure AD</span><span class="sxs-lookup"><span data-stu-id="e5aef-124">Delete a user in your Azure AD</span></span>](active-directory-users-delete-user-azure-portal.md)
* [<span data-ttu-id="e5aef-125">Tilldela en användare till en roll i din Azure AD</span><span class="sxs-lookup"><span data-stu-id="e5aef-125">Assign a user to a role in your Azure AD</span></span>](active-directory-users-assign-role-azure-portal.md)
