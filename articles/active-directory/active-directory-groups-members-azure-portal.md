---
title: "Hantera medlemmar för en grupp i Azure Active Directory | Microsoft Docs"
description: "Lägga till eller ta bort användare och enheter från en grupp i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d399a97d-fd2a-4b2d-b73d-0975db83f41b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 044e88f95712e1cc5b5532f5492c78d711a8d858
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a><span data-ttu-id="6f609-103">Hantera gruppmedlemskap för användare i din Azure Active Directory-klient</span><span class="sxs-lookup"><span data-stu-id="6f609-103">Manage group membership for users in your Azure Active Directory tenant</span></span>
<span data-ttu-id="6f609-104">Den här artikeln förklarar hur du hanterar medlemmar för en grupp i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6f609-104">This article explains how to manage the members for a group in Azure Active Directory (Azure AD).</span></span>

## <a name="how-do-i-find-the-members-and-manage-them"></a><span data-ttu-id="6f609-105">Hur gör hitta medlemmarna och hantera dem?</span><span class="sxs-lookup"><span data-stu-id="6f609-105">How do I find the members and manage them?</span></span>
1. <span data-ttu-id="6f609-106">Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="6f609-106">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="6f609-107">Välj **fler tjänster**, ange **användare och grupper** i textrutan och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="6f609-107">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Öppna användarhantering](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. <span data-ttu-id="6f609-109">På den **användare och grupper** bladet väljer **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="6f609-109">On the **Users and groups** blade, select **All groups**.</span></span>

   ![Öppna bladet grupper](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="6f609-111">På den **användare och grupper – alla grupper** bladet Välj en grupp.</span><span class="sxs-lookup"><span data-stu-id="6f609-111">On the **Users and groups - All groups** blade, select a group.</span></span>
5. <span data-ttu-id="6f609-112">På den **grupp - *groupname***  bladet väljer **medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="6f609-112">On the **Group - *groupname*** blade, select **Members**.</span></span>

   ![Öppna bladet medlemmar](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. <span data-ttu-id="6f609-114">Lägga till medlemmar i gruppen på den **grupp - medlemmar** bladet väljer **lägga till medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="6f609-114">To add members to the group, on the **Group - Members** blade, select **Add Members**.</span></span>

   ![Lägga till medlemmar kommando](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. <span data-ttu-id="6f609-116">På den **medlemmar** bladet, Välj en eller flera användare eller enheter att lägga till i gruppen och välj den **Välj** längst ned på bladet för att lägga till dem i gruppen.</span><span class="sxs-lookup"><span data-stu-id="6f609-116">On the **Members** blade, select one or more users or devices to add to the group and select the **Select** button at the bottom of the blade to add them to the group.</span></span> <span data-ttu-id="6f609-117">Den **användaren** rutan filtrerar baserat på matchning inmatningen till alla delar av namnet på en användare eller enhet.</span><span class="sxs-lookup"><span data-stu-id="6f609-117">The **User** box filters the display based on matching your entry to any part of a user or device name.</span></span> <span data-ttu-id="6f609-118">Inga jokertecken godkänns i rutan.</span><span class="sxs-lookup"><span data-stu-id="6f609-118">No wildcard characters are accepted in that box.</span></span>
8. <span data-ttu-id="6f609-119">Ta bort medlemmar från gruppen på den **grupp - medlemmar** bladet Välj en medlem.</span><span class="sxs-lookup"><span data-stu-id="6f609-119">To remove members from the group, on the **Group - Members** blade, select a member.</span></span>
9. <span data-ttu-id="6f609-120">På den ***membername*** bladet väljer den **ta bort** kommando och bekräfta valet i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="6f609-120">On the ***membername*** blade, select the **Remove** command, and confirm your choice at the prompt.</span></span>

   ![ta bort medlemmar kommandot](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. <span data-ttu-id="6f609-122">När du har ändrat medlemmar för gruppen, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="6f609-122">When you finish changing members for the group, select **Save**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="6f609-123">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="6f609-123">Additional information</span></span>
<span data-ttu-id="6f609-124">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6f609-124">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="6f609-125">Se befintliga grupper</span><span class="sxs-lookup"><span data-stu-id="6f609-125">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="6f609-126">Skapa en ny grupp och lägga till medlemmar</span><span class="sxs-lookup"><span data-stu-id="6f609-126">Create a new group and adding members</span></span>](active-directory-groups-create-azure-portal.md)
* [<span data-ttu-id="6f609-127">Hantera inställningar för en grupp</span><span class="sxs-lookup"><span data-stu-id="6f609-127">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="6f609-128">Hantera medlemskap i en grupp</span><span class="sxs-lookup"><span data-stu-id="6f609-128">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="6f609-129">Hantera dynamiska regler för användare i en grupp</span><span class="sxs-lookup"><span data-stu-id="6f609-129">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
