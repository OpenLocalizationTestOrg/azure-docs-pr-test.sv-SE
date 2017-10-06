---
title: "aaaHow toogive åtkomst tooPrivileged Identity Management - Azure | Microsoft Docs"
description: "Lär dig hur tooadd roller toousers med hello Azure Active Directory Privileged Identity Management-tillägget så att de kan hantera PIM."
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
ms.openlocfilehash: 5d99589af4af766e430d7cecd743ace752f63768
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="giving-access-toomanage-azure-ad-privileged-identity-management"></a><span data-ttu-id="aa259-103">Ge åtkomst toomanage Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="aa259-103">Giving access toomanage Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="aa259-104">hello globala administratören som aktiverar Azure AD Privileged Identity Management (PIM) för en organisation automatiskt hämta rolltilldelningar och komma åt tooPIM.</span><span class="sxs-lookup"><span data-stu-id="aa259-104">hello global administrator who enables Azure AD Privileged Identity Management (PIM) for an organization automatically get role assignments and access tooPIM.</span></span> <span data-ttu-id="aa259-105">Ingen annan får skrivåtkomst som standard, men även andra globala administratörer.</span><span class="sxs-lookup"><span data-stu-id="aa259-105">No one else gets write access by default, though, including other global administrators.</span></span> <span data-ttu-id="aa259-106">Andra globala administratörer, säkerhetsadministratörer och säkerhet läsare har skrivskyddad åtkomst tooAzure AD PIM.</span><span class="sxs-lookup"><span data-stu-id="aa259-106">Other global adminstrators, security administrators, and security readers have read-only access tooAzure AD PIM.</span></span> <span data-ttu-id="aa259-107">toogive åtkomst tooPIM hello första användaren kan tilldela andra toohello **administratör av Privilegierade roller** roll.</span><span class="sxs-lookup"><span data-stu-id="aa259-107">toogive access tooPIM, hello first user can assign others toohello **Privileged role administrator** role.</span></span> <span data-ttu-id="aa259-108">Den här tilldelningen måste göras i PIM sig själv och kan inte ändras via PowerShell eller andra portaler.</span><span class="sxs-lookup"><span data-stu-id="aa259-108">This assignment must be done from within PIM itself, and cannot be changed via PowerShell or other portals.</span></span>

> [!NOTE]
> <span data-ttu-id="aa259-109">Hantera Azure AD PIM kräver Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="aa259-109">Managing Azure AD PIM requires Azure MFA.</span></span> <span data-ttu-id="aa259-110">Eftersom Microsoft-konton inte kan registrera dig för Azure MFA, när en användare loggar in med ett Microsoft-konto kan inte komma åt Azure AD PIM.</span><span class="sxs-lookup"><span data-stu-id="aa259-110">Since Microsoft accounts cannot register for Azure MFA, a user who signs in with a Microsoft account cannot access Azure AD PIM.</span></span>
> 
> 

<span data-ttu-id="aa259-111">Kontrollera att det finns alltid minst två användare i en privilegierad roll administratörsroll om en användare är utelåst eller deras kontot har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="aa259-111">Make sure there are always at least two users in a privileged role administrator role, in case one user is locked out or their account is deleted.</span></span>

## <a name="give-another-user-access-toomanage-pim"></a><span data-ttu-id="aa259-112">Ge en annan användare åtkomst toomanage PIM</span><span class="sxs-lookup"><span data-stu-id="aa259-112">Give another user access toomanage PIM</span></span>
1. <span data-ttu-id="aa259-113">Logga in toohello [Azure-portalen](https://portal.azure.com/) och välj hello **Azure AD Privileged Identity Management** app på hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="aa259-113">Sign in toohello [Azure portal](https://portal.azure.com/) and select hello **Azure AD Privileged Identity Management** app on hello dashboard.</span></span>
2. <span data-ttu-id="aa259-114">Välj **hantera Privilegierade roller** > **administratör av Privilegierade roller** > **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="aa259-114">Select **Manage privileged roles** > **Privileged role administrator** > **Add**.</span></span>
   
    ![Lägg till Privilegierade rollen administratörer – skärmbild][1]
3. <span data-ttu-id="aa259-116">Steg 1 har redan slutförts på hello Lägg till hanterade användare bladet.</span><span class="sxs-lookup"><span data-stu-id="aa259-116">On hello Add managed users blade, step 1 is already complete.</span></span> <span data-ttu-id="aa259-117">Välj steg 2, **Välj användare** och Sök hello användare du vill ha tooadd.</span><span class="sxs-lookup"><span data-stu-id="aa259-117">Select step 2, **Select users** and search for hello user you want tooadd.</span></span>
   
    ![Välj användare – skärmbild][2]
4. <span data-ttu-id="aa259-119">Välj hello användare hello sökresultaten och på **klar**.</span><span class="sxs-lookup"><span data-stu-id="aa259-119">Select hello user from hello search results, and click **Done**.</span></span>
5. <span data-ttu-id="aa259-120">Klicka på **OK** toosave valet.</span><span class="sxs-lookup"><span data-stu-id="aa259-120">Click **OK** toosave your selection.</span></span> <span data-ttu-id="aa259-121">hello-användare som du har valt visas i hello listan över Privilegierade rollen administratörer.</span><span class="sxs-lookup"><span data-stu-id="aa259-121">hello user you have selected will appear in hello list of Privileged role administrators.</span></span>
   
   * <span data-ttu-id="aa259-122">När du tilldelar en ny roll toosomeone konfigureras de automatiskt som berättigade tooactivate hello roll.</span><span class="sxs-lookup"><span data-stu-id="aa259-122">Whenever you assign a new role toosomeone, they are automatically set up as eligible tooactivate hello role.</span></span> <span data-ttu-id="aa259-123">Om du vill toomake dem permanent i hello rollen klickar du på hello användare i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="aa259-123">If you want toomake them permanent in hello role, click hello user in hello list.</span></span> <span data-ttu-id="aa259-124">Välj **Se behörighet** hello användaren information-menyn.</span><span class="sxs-lookup"><span data-stu-id="aa259-124">Select **make perm** in hello user information menu.</span></span>
6. <span data-ttu-id="aa259-125">Skicka hello användaren en länk för[komma igång med Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="aa259-125">Send hello user a link too[Getting started with Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).</span></span>

## <a name="remove-another-users-access-rights-for-managing-pim"></a><span data-ttu-id="aa259-126">Ta bort en annan användare behörighet som krävs för att hantera PIM</span><span class="sxs-lookup"><span data-stu-id="aa259-126">Remove another user's access rights for managing PIM</span></span>
<span data-ttu-id="aa259-127">Innan du tar bort någon från hello Privilegierade roller administratörsroll du alltid kontrollera kommer fortfarande att två användare som är tilldelade tooit.</span><span class="sxs-lookup"><span data-stu-id="aa259-127">Before you remove someone from hello privileged role administrator role, always make sure there will still be two users assigned tooit.</span></span>

1. <span data-ttu-id="aa259-128">I hello PIM-instrumentpanelen, klickar du på hello rollen **administratör av Privilegierade roller**.</span><span class="sxs-lookup"><span data-stu-id="aa259-128">In hello PIM dashboard, click on hello role **Privileged role administrator**.</span></span>  <span data-ttu-id="aa259-129">hello lista över användare för närvarande i rollen visas.</span><span class="sxs-lookup"><span data-stu-id="aa259-129">hello list of users currently in that role will be displayed.</span></span>
2. <span data-ttu-id="aa259-130">Klicka på hello användare i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="aa259-130">Click on hello user in hello user list.</span></span>
3. <span data-ttu-id="aa259-131">Klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="aa259-131">Click on **Remove**.</span></span>  <span data-ttu-id="aa259-132">Visas ett bekräftelsemeddelande.</span><span class="sxs-lookup"><span data-stu-id="aa259-132">You are presented with a confirmation message.</span></span>
4. <span data-ttu-id="aa259-133">Klicka på **Ja** tooremove hello användaren från hello roll.</span><span class="sxs-lookup"><span data-stu-id="aa259-133">Click **Yes** tooremove hello user from hello role.</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="aa259-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aa259-134">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
