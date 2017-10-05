---
title: "Hantera grupper som gruppen tillhör i Azure Active Directory | Microsoft Docs"
description: "Grupper kan innehålla andra grupper i Azure Active Directory. Här är hur du hanterar dessa medlemskap."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e785c2d0-7724-47d4-a56e-c58280c08a14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 08e04a6590176c4084ca47b4bd6cbb22500eca2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-to-which-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a><span data-ttu-id="35347-104">Hantera vilka grupper som en grupp tillhör i Azure Active Directory-klient</span><span class="sxs-lookup"><span data-stu-id="35347-104">Manage to which groups a group belongs in your Azure Active Directory tenant</span></span>
<span data-ttu-id="35347-105">Grupper kan innehålla andra grupper i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="35347-105">Groups can contain other groups in Azure Active Directory.</span></span> <span data-ttu-id="35347-106">Här är hur du hanterar dessa medlemskap.</span><span class="sxs-lookup"><span data-stu-id="35347-106">Here's how to manage those memberships.</span></span>

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a><span data-ttu-id="35347-107">Hur hittar jag min grupp är medlem i grupperna?</span><span class="sxs-lookup"><span data-stu-id="35347-107">How do I find the groups my group is a member of?</span></span>
1. <span data-ttu-id="35347-108">Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="35347-108">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="35347-109">Välj **fler tjänster**, ange **användare och grupper** i textrutan och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="35347-109">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Öppna användarhantering](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
3. <span data-ttu-id="35347-111">På den **användare och grupper** bladet väljer **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="35347-111">On the **Users and groups** blade, select **All groups**.</span></span>

   ![Öppna bladet grupper](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="35347-113">På den **användare och grupper – alla grupper** bladet Välj en grupp.</span><span class="sxs-lookup"><span data-stu-id="35347-113">On the **Users and groups - All groups** blade, select a group.</span></span>
5. <span data-ttu-id="35347-114">På den **grupp - *groupname***  bladet väljer **gruppmedlemskap**.</span><span class="sxs-lookup"><span data-stu-id="35347-114">On the **Group - *groupname*** blade, select **Group memberships**.</span></span>

   ![Öppna bladet för medlemskap](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
6. <span data-ttu-id="35347-116">Att lägga till gruppen som en medlem i en annan grupp på den **Group - gruppmedlemskap** bladet väljer den **Lägg till** kommando.</span><span class="sxs-lookup"><span data-stu-id="35347-116">To add your group as a member of another group, on the **Group - Group memberships** blade, select the **Add** command.</span></span>
7. <span data-ttu-id="35347-117">Väljer du en grupp i **Välj grupp** bladet och välj sedan den **Välj** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="35347-117">Select a group from the **Select Group** blade, and then select the **Select** button at the bottom of the blade.</span></span> <span data-ttu-id="35347-118">Du kan lägga till din grupp bara en grupp i taget.</span><span class="sxs-lookup"><span data-stu-id="35347-118">You can add your group to only one group at a time.</span></span> <span data-ttu-id="35347-119">Den **användaren** rutan filtrerar baserat på matchning inmatningen till alla delar av namnet på en användare eller enhet.</span><span class="sxs-lookup"><span data-stu-id="35347-119">The **User** box filters the display based on matching your entry to any part of a user or device name.</span></span> <span data-ttu-id="35347-120">Inga jokertecken godkänns i rutan.</span><span class="sxs-lookup"><span data-stu-id="35347-120">No wildcard characters are accepted in that box.</span></span>

   ![Lägg till en gruppmedlemskap](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. <span data-ttu-id="35347-122">Ta bort gruppen som en medlem i en annan grupp på den **Group - gruppmedlemskap** bladet Välj en grupp.</span><span class="sxs-lookup"><span data-stu-id="35347-122">To remove your group as a member of another group, on the **Group - Group memberships** blade, select a group.</span></span>
9. <span data-ttu-id="35347-123">På den ***groupname*** bladet väljer den **ta bort** kommando och bekräfta valet i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="35347-123">On the ***groupname*** blade, select the **Remove** command, and confirm your choice at the prompt.</span></span>

   ![ta bort medlemskap kommandot](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. <span data-ttu-id="35347-125">När du har ändrat medlemskap för gruppen, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="35347-125">When you finish changing group memberships for your group, select **Save**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="35347-126">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="35347-126">Additional information</span></span>
<span data-ttu-id="35347-127">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="35347-127">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="35347-128">Se befintliga grupper</span><span class="sxs-lookup"><span data-stu-id="35347-128">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="35347-129">Skapa en ny grupp och lägga till medlemmar</span><span class="sxs-lookup"><span data-stu-id="35347-129">Create a new group and adding members</span></span>](active-directory-groups-create-azure-portal.md)
* [<span data-ttu-id="35347-130">Hantera inställningar för en grupp</span><span class="sxs-lookup"><span data-stu-id="35347-130">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="35347-131">Hantera medlemmar i en grupp</span><span class="sxs-lookup"><span data-stu-id="35347-131">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="35347-132">Hantera dynamiska regler för användare i en grupp</span><span class="sxs-lookup"><span data-stu-id="35347-132">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
