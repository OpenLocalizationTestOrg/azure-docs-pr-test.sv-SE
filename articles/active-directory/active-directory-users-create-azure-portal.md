---
title: "aaaAdd nya användare tooAzure Active Directory | Microsoft Docs"
description: "Förklarar hur tooadd nya användare eller ändrar användarinformation i Azure Active Directory."
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
ms.openlocfilehash: c4a156ea31b81202bb0d0ac224afbfc3f1785532
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-new-users-tooazure-active-directory"></a><span data-ttu-id="ccf8a-103">Lägga till nya användare tooAzure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ccf8a-103">Add new users tooAzure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ccf8a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ccf8a-104">Azure portal</span></span>](active-directory-users-create-azure-portal.md)
> * [<span data-ttu-id="ccf8a-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="ccf8a-105">Azure classic portal</span></span>](active-directory-create-users.md)
>
>

<span data-ttu-id="ccf8a-106">Den här artikeln förklarar hur tooadd nya användare i din organisation i hello Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ccf8a-106">This article explains how tooadd new users in your organization in hello Azure Active Directory (Azure AD).</span></span> 

1. <span data-ttu-id="ccf8a-107">Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="ccf8a-107">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="ccf8a-108">Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="ccf8a-108">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Öppna användare och grupper](./media/active-directory-users-create-azure-portal/create-users-user-management.png)
3. <span data-ttu-id="ccf8a-110">På hello **användare och grupper** bladet väljer **alla användare**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ccf8a-110">On hello **Users and groups** blade, select **All users**, and then select **Add**.</span></span>

   ![Att välja hello tilläggskommando](./media/active-directory-users-create-azure-portal/create-users-add-command.png)
4. <span data-ttu-id="ccf8a-112">Ange information om hello användaren som **namn** och **användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="ccf8a-112">Enter details for hello user, such as **Name** and **User name**.</span></span> <span data-ttu-id="ccf8a-113">Hej domännamndelen för hello användarnamn måste antingen vara hello inledande domain name ”foo.onmicrosoft.com” standarddomännamnet eller verifierad, ofedererad domännamn, till exempel ”contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="ccf8a-113">hello domain name portion of hello user name must either be hello initial default domain name "foo.onmicrosoft.com" domain name, or a verified, non-federated domain name such as "contoso.com."</span></span>
5. <span data-ttu-id="ccf8a-114">Kopiera eller på annat sätt Obs hello genereras användarlösenord så att du kan ange den toohello användaren när processen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ccf8a-114">Copy or otherwise note hello generated user password so that you can provide it toohello user after this process is complete.</span></span>
6. <span data-ttu-id="ccf8a-115">Du kan också öppna och fylla i hello information i hello **profil** bladet, hello **grupper** bladet eller hello **Directory rollen** bladet för hello användare.</span><span class="sxs-lookup"><span data-stu-id="ccf8a-115">Optionally, you can open and fill out hello information in hello **Profile** blade, hello **Groups** blade, or hello **Directory role** blade for hello user.</span></span> <span data-ttu-id="ccf8a-116">Mer information om användar- och administratörsroller finns i [Tilldela administratörsroller i Azure AD](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="ccf8a-116">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>
7. <span data-ttu-id="ccf8a-117">På hello **användare** bladet väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="ccf8a-117">On hello **User** blade, select **Create**.</span></span>
8. <span data-ttu-id="ccf8a-118">Distribuera hello genererade lösenordet toohello ny användare på ett säkert sätt så att hello användare kan logga in.</span><span class="sxs-lookup"><span data-stu-id="ccf8a-118">Securely distribute hello generated password toohello new user so that hello user can sign in.</span></span>

### <a name="next-steps"></a><span data-ttu-id="ccf8a-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ccf8a-119">Next steps</span></span>
* [<span data-ttu-id="ccf8a-120">Lägg till en extern användare</span><span class="sxs-lookup"><span data-stu-id="ccf8a-120">Add an external user</span></span>](active-directory-users-create-external-azure-portal.md)
* [<span data-ttu-id="ccf8a-121">Återställa en användares lösenord i hello nya Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ccf8a-121">Reset a user's password in hello new Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
* [<span data-ttu-id="ccf8a-122">Ändra en användares arbetsinformation</span><span class="sxs-lookup"><span data-stu-id="ccf8a-122">Change a user's work information</span></span>](active-directory-users-work-info-azure-portal.md)
* [<span data-ttu-id="ccf8a-123">Hantera användarprofiler</span><span class="sxs-lookup"><span data-stu-id="ccf8a-123">Manage user profiles</span></span>](active-directory-users-profile-azure-portal.md)
* [<span data-ttu-id="ccf8a-124">Tar bort en användare i din Azure AD</span><span class="sxs-lookup"><span data-stu-id="ccf8a-124">Delete a user in your Azure AD</span></span>](active-directory-users-delete-user-azure-portal.md)
* [<span data-ttu-id="ccf8a-125">Tilldela en användarroll tooa i din Azure AD</span><span class="sxs-lookup"><span data-stu-id="ccf8a-125">Assign a user tooa role in your Azure AD</span></span>](active-directory-users-assign-role-azure-portal.md)
