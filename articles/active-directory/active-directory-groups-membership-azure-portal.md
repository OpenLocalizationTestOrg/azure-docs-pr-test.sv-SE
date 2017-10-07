---
title: "aaaManage hello grupper som gruppen tillhör tooin Azure Active Directory | Microsoft Docs"
description: "Grupper kan innehålla andra grupper i Azure Active Directory. Här är hur toomanage dessa medlemskap."
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
ms.openlocfilehash: d0a0a1967084de0968e1e802559f9cdfd7ca6ae4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-toowhich-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a><span data-ttu-id="29c78-104">Hantera toowhich grupper som en grupp tillhör i Azure Active Directory-klient</span><span class="sxs-lookup"><span data-stu-id="29c78-104">Manage toowhich groups a group belongs in your Azure Active Directory tenant</span></span>
<span data-ttu-id="29c78-105">Grupper kan innehålla andra grupper i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="29c78-105">Groups can contain other groups in Azure Active Directory.</span></span> <span data-ttu-id="29c78-106">Här är hur toomanage dessa medlemskap.</span><span class="sxs-lookup"><span data-stu-id="29c78-106">Here's how toomanage those memberships.</span></span>

## <a name="how-do-i-find-hello-groups-my-group-is-a-member-of"></a><span data-ttu-id="29c78-107">Hur hittar hello grupper min grupp är medlem i?</span><span class="sxs-lookup"><span data-stu-id="29c78-107">How do I find hello groups my group is a member of?</span></span>
1. <span data-ttu-id="29c78-108">Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="29c78-108">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="29c78-109">Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="29c78-109">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Öppna användarhantering](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
3. <span data-ttu-id="29c78-111">På hello **användare och grupper** bladet väljer **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="29c78-111">On hello **Users and groups** blade, select **All groups**.</span></span>

   ![Öppna hello grupper bladet](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="29c78-113">På hello **användare och grupper – alla grupper** bladet Välj en grupp.</span><span class="sxs-lookup"><span data-stu-id="29c78-113">On hello **Users and groups - All groups** blade, select a group.</span></span>
5. <span data-ttu-id="29c78-114">På hello **grupp - *groupname***  bladet väljer **gruppmedlemskap**.</span><span class="sxs-lookup"><span data-stu-id="29c78-114">On hello **Group - *groupname*** blade, select **Group memberships**.</span></span>

   ![Öppna hello blad för medlemskap](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
6. <span data-ttu-id="29c78-116">tooadd din grupp som en medlem i en annan grupp på hello **Group - gruppmedlemskap** bladet, Välj hello **Lägg till** kommando.</span><span class="sxs-lookup"><span data-stu-id="29c78-116">tooadd your group as a member of another group, on hello **Group - Group memberships** blade, select hello **Add** command.</span></span>
7. <span data-ttu-id="29c78-117">Välj en grupp i hello **Välj grupp** bladet och välj sedan hello **Välj** knappen längst ned hello hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="29c78-117">Select a group from hello **Select Group** blade, and then select hello **Select** button at hello bottom of hello blade.</span></span> <span data-ttu-id="29c78-118">Du kan lägga till din grupp tooonly en grupp i taget.</span><span class="sxs-lookup"><span data-stu-id="29c78-118">You can add your group tooonly one group at a time.</span></span> <span data-ttu-id="29c78-119">Hej **användaren** filtrerar hello visas baserat på matchning post tooany-tillhör ett namn för användaren eller enheten.</span><span class="sxs-lookup"><span data-stu-id="29c78-119">hello **User** box filters hello display based on matching your entry tooany part of a user or device name.</span></span> <span data-ttu-id="29c78-120">Inga jokertecken godkänns i rutan.</span><span class="sxs-lookup"><span data-stu-id="29c78-120">No wildcard characters are accepted in that box.</span></span>

   ![Lägg till en gruppmedlemskap](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. <span data-ttu-id="29c78-122">tooremove din grupp som en medlem i en annan grupp på hello **Group - gruppmedlemskap** bladet Välj en grupp.</span><span class="sxs-lookup"><span data-stu-id="29c78-122">tooremove your group as a member of another group, on hello **Group - Group memberships** blade, select a group.</span></span>
9. <span data-ttu-id="29c78-123">På hello ***groupname*** bladet, Välj hello **ta bort** kommando och bekräfta valet hello i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="29c78-123">On hello ***groupname*** blade, select hello **Remove** command, and confirm your choice at hello prompt.</span></span>

   ![ta bort medlemskap kommandot](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. <span data-ttu-id="29c78-125">När du har ändrat medlemskap för gruppen, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="29c78-125">When you finish changing group memberships for your group, select **Save**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="29c78-126">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="29c78-126">Additional information</span></span>
<span data-ttu-id="29c78-127">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="29c78-127">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="29c78-128">Se befintliga grupper</span><span class="sxs-lookup"><span data-stu-id="29c78-128">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="29c78-129">Skapa en ny grupp och lägga till medlemmar</span><span class="sxs-lookup"><span data-stu-id="29c78-129">Create a new group and adding members</span></span>](active-directory-groups-create-azure-portal.md)
* [<span data-ttu-id="29c78-130">Hantera inställningar för en grupp</span><span class="sxs-lookup"><span data-stu-id="29c78-130">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="29c78-131">Hantera medlemmar i en grupp</span><span class="sxs-lookup"><span data-stu-id="29c78-131">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="29c78-132">Hantera dynamiska regler för användare i en grupp</span><span class="sxs-lookup"><span data-stu-id="29c78-132">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
