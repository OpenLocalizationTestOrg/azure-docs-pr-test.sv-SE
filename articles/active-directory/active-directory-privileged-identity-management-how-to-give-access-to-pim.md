---
title: "Ge åtkomst till Privileged Identity Management - Azure | Microsoft Docs"
description: "Lär dig mer om att lägga till roller för användare med Azure Active Directory Privileged Identity Management-tillägget så att de kan hantera PIM."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: d4c53b53-2b37-41e6-813c-96ec08a1c897
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: aeaefb484b29da6e89c2c3c650a79a881b3fa5b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="giving-access-to-manage-azure-ad-privileged-identity-management"></a><span data-ttu-id="f88f7-103">Ger tillgång till att hantera Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="f88f7-103">Giving access to manage Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="f88f7-104">Den globala administratören som aktiverar Azure AD Privileged Identity Management (PIM) för en organisation automatiskt få rolltilldelningar och åtkomst till PIM.</span><span class="sxs-lookup"><span data-stu-id="f88f7-104">The global administrator who enables Azure AD Privileged Identity Management (PIM) for an organization automatically get role assignments and access to PIM.</span></span> <span data-ttu-id="f88f7-105">Ingen annan får skrivåtkomst som standard, men även andra globala administratörer.</span><span class="sxs-lookup"><span data-stu-id="f88f7-105">No one else gets write access by default, though, including other global administrators.</span></span> <span data-ttu-id="f88f7-106">Andra globala administratörer, säkerhetsadministratörer och säkerhet läsare har skrivskyddad åtkomst till Azure AD PIM.</span><span class="sxs-lookup"><span data-stu-id="f88f7-106">Other global adminstrators, security administrators, and security readers have read-only access to Azure AD PIM.</span></span> <span data-ttu-id="f88f7-107">Om du vill ge åtkomst till PIM, kan den första användaren tilldela andra användare till den **administratör av Privilegierade roller** roll.</span><span class="sxs-lookup"><span data-stu-id="f88f7-107">To give access to PIM, the first user can assign others to the **Privileged role administrator** role.</span></span> <span data-ttu-id="f88f7-108">Den här tilldelningen måste göras i PIM sig själv och kan inte ändras via PowerShell eller andra portaler.</span><span class="sxs-lookup"><span data-stu-id="f88f7-108">This assignment must be done from within PIM itself, and cannot be changed via PowerShell or other portals.</span></span>

> [!NOTE]
> <span data-ttu-id="f88f7-109">Hantera Azure AD PIM kräver Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="f88f7-109">Managing Azure AD PIM requires Azure MFA.</span></span> <span data-ttu-id="f88f7-110">Eftersom Microsoft-konton inte kan registrera dig för Azure MFA, när en användare loggar in med ett Microsoft-konto kan inte komma åt Azure AD PIM.</span><span class="sxs-lookup"><span data-stu-id="f88f7-110">Since Microsoft accounts cannot register for Azure MFA, a user who signs in with a Microsoft account cannot access Azure AD PIM.</span></span>
> 
> 

<span data-ttu-id="f88f7-111">Kontrollera att det finns alltid minst två användare i en privilegierad roll administratörsroll om en användare är utelåst eller deras kontot har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="f88f7-111">Make sure there are always at least two users in a privileged role administrator role, in case one user is locked out or their account is deleted.</span></span>

## <a name="give-another-user-access-to-manage-pim"></a><span data-ttu-id="f88f7-112">Ge en annan användare tillgång till att hantera PIM</span><span class="sxs-lookup"><span data-stu-id="f88f7-112">Give another user access to manage PIM</span></span>
1. <span data-ttu-id="f88f7-113">Logga in på den [Azure-portalen](https://portal.azure.com/) och välj den **Azure AD Privileged Identity Management** app på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="f88f7-113">Sign in to the [Azure portal](https://portal.azure.com/) and select the **Azure AD Privileged Identity Management** app on the dashboard.</span></span>
2. <span data-ttu-id="f88f7-114">Välj **hantera Privilegierade roller** > **administratör av Privilegierade roller** > **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f88f7-114">Select **Manage privileged roles** > **Privileged role administrator** > **Add**.</span></span>
   
    ![Lägg till Privilegierade rollen administratörer – skärmbild][1]
3. <span data-ttu-id="f88f7-116">Steg 1 har redan slutförts på bladet Lägg till hanterade användare.</span><span class="sxs-lookup"><span data-stu-id="f88f7-116">On the Add managed users blade, step 1 is already complete.</span></span> <span data-ttu-id="f88f7-117">Välj steg 2, **Välj användare** och Sök efter den användare som du vill lägga till.</span><span class="sxs-lookup"><span data-stu-id="f88f7-117">Select step 2, **Select users** and search for the user you want to add.</span></span>
   
    ![Välj användare – skärmbild][2]
4. <span data-ttu-id="f88f7-119">Välj användaren i sökresultaten och på **klar**.</span><span class="sxs-lookup"><span data-stu-id="f88f7-119">Select the user from the search results, and click **Done**.</span></span>
5. <span data-ttu-id="f88f7-120">Klicka på **OK** att spara ditt val.</span><span class="sxs-lookup"><span data-stu-id="f88f7-120">Click **OK** to save your selection.</span></span> <span data-ttu-id="f88f7-121">Användare som du har valt visas i listan över Privilegierade rollen administratörer.</span><span class="sxs-lookup"><span data-stu-id="f88f7-121">The user you have selected will appear in the list of Privileged role administrators.</span></span>
   
   * <span data-ttu-id="f88f7-122">När du tilldelar en ny roll till någon konfigureras de automatiskt som kvalificerade att aktivera rollen.</span><span class="sxs-lookup"><span data-stu-id="f88f7-122">Whenever you assign a new role to someone, they are automatically set up as eligible to activate the role.</span></span> <span data-ttu-id="f88f7-123">Klicka på användare i listan om du vill göra dem permanent i rollen.</span><span class="sxs-lookup"><span data-stu-id="f88f7-123">If you want to make them permanent in the role, click the user in the list.</span></span> <span data-ttu-id="f88f7-124">Välj **Se behörighet** i menyn användaren information.</span><span class="sxs-lookup"><span data-stu-id="f88f7-124">Select **make perm** in the user information menu.</span></span>
6. <span data-ttu-id="f88f7-125">Skicka till användarna en länk till [komma igång med Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="f88f7-125">Send the user a link to [Getting started with Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).</span></span>

## <a name="remove-another-users-access-rights-for-managing-pim"></a><span data-ttu-id="f88f7-126">Ta bort en annan användare behörighet som krävs för att hantera PIM</span><span class="sxs-lookup"><span data-stu-id="f88f7-126">Remove another user's access rights for managing PIM</span></span>
<span data-ttu-id="f88f7-127">Innan du tar bort någon från en administratör för privilegierade roller du alltid kontrollera kommer fortfarande att två användare som är tilldelade till den.</span><span class="sxs-lookup"><span data-stu-id="f88f7-127">Before you remove someone from the privileged role administrator role, always make sure there will still be two users assigned to it.</span></span>

1. <span data-ttu-id="f88f7-128">I PIM-instrumentpanelen, klickar du på rollen **administratör av Privilegierade roller**.</span><span class="sxs-lookup"><span data-stu-id="f88f7-128">In the PIM dashboard, click on the role **Privileged role administrator**.</span></span>  <span data-ttu-id="f88f7-129">Listan över användare för närvarande i rollen visas.</span><span class="sxs-lookup"><span data-stu-id="f88f7-129">The list of users currently in that role will be displayed.</span></span>
2. <span data-ttu-id="f88f7-130">Klicka på användare i användarlistan.</span><span class="sxs-lookup"><span data-stu-id="f88f7-130">Click on the user in the user list.</span></span>
3. <span data-ttu-id="f88f7-131">Klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f88f7-131">Click on **Remove**.</span></span>  <span data-ttu-id="f88f7-132">Visas ett bekräftelsemeddelande.</span><span class="sxs-lookup"><span data-stu-id="f88f7-132">You are presented with a confirmation message.</span></span>
4. <span data-ttu-id="f88f7-133">Klicka på **Ja** att ta bort användaren från rollen.</span><span class="sxs-lookup"><span data-stu-id="f88f7-133">Click **Yes** to remove the user from the role.</span></span>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="f88f7-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f88f7-134">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
