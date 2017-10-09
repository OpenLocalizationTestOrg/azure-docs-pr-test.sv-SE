---
title: "aaaManage hello medlemmar för en grupp i Azure Active Directory | Microsoft Docs"
description: "Hur tooadd eller ta bort användare och enheter från en grupp i Azure Active Directory"
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
ms.openlocfilehash: 4cb16ee63828003da251423a04736f7174dd4896
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a><span data-ttu-id="d5f39-103">Hantera gruppmedlemskap för användare i din Azure Active Directory-klient</span><span class="sxs-lookup"><span data-stu-id="d5f39-103">Manage group membership for users in your Azure Active Directory tenant</span></span>
<span data-ttu-id="d5f39-104">Den här artikeln förklarar hur toomanage hello medlemmar för en grupp i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d5f39-104">This article explains how toomanage hello members for a group in Azure Active Directory (Azure AD).</span></span>

## <a name="how-do-i-find-hello-members-and-manage-them"></a><span data-ttu-id="d5f39-105">Hur gör hitta hello medlemmar och hantera dem?</span><span class="sxs-lookup"><span data-stu-id="d5f39-105">How do I find hello members and manage them?</span></span>
1. <span data-ttu-id="d5f39-106">Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="d5f39-106">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="d5f39-107">Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="d5f39-107">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Öppna användarhantering](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. <span data-ttu-id="d5f39-109">På hello **användare och grupper** bladet väljer **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="d5f39-109">On hello **Users and groups** blade, select **All groups**.</span></span>

   ![Öppna hello grupper bladet](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="d5f39-111">På hello **användare och grupper – alla grupper** bladet Välj en grupp.</span><span class="sxs-lookup"><span data-stu-id="d5f39-111">On hello **Users and groups - All groups** blade, select a group.</span></span>
5. <span data-ttu-id="d5f39-112">På hello **grupp - *groupname***  bladet väljer **medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="d5f39-112">On hello **Group - *groupname*** blade, select **Members**.</span></span>

   ![Öppna hello medlemmar bladet](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. <span data-ttu-id="d5f39-114">tooadd medlemmar toohello grupp på hello **grupp - medlemmar** bladet väljer **lägga till medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="d5f39-114">tooadd members toohello group, on hello **Group - Members** blade, select **Add Members**.</span></span>

   ![Lägga till medlemmar kommando](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. <span data-ttu-id="d5f39-116">På hello **medlemmar** bladet, Välj en eller flera användare eller enheter tooadd toohello gruppen och välj hello **Välj** knappen längst ned hello hello bladet tooadd dem toohello grupp.</span><span class="sxs-lookup"><span data-stu-id="d5f39-116">On hello **Members** blade, select one or more users or devices tooadd toohello group and select hello **Select** button at hello bottom of hello blade tooadd them toohello group.</span></span> <span data-ttu-id="d5f39-117">Hej **användaren** filtrerar hello visas baserat på matchning post tooany-tillhör ett namn för användaren eller enheten.</span><span class="sxs-lookup"><span data-stu-id="d5f39-117">hello **User** box filters hello display based on matching your entry tooany part of a user or device name.</span></span> <span data-ttu-id="d5f39-118">Inga jokertecken godkänns i rutan.</span><span class="sxs-lookup"><span data-stu-id="d5f39-118">No wildcard characters are accepted in that box.</span></span>
8. <span data-ttu-id="d5f39-119">tooremove medlemmar från hello gruppen på hello **grupp - medlemmar** bladet Välj en medlem.</span><span class="sxs-lookup"><span data-stu-id="d5f39-119">tooremove members from hello group, on hello **Group - Members** blade, select a member.</span></span>
9. <span data-ttu-id="d5f39-120">På hello ***membername*** bladet, Välj hello **ta bort** kommando och bekräfta valet hello i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="d5f39-120">On hello ***membername*** blade, select hello **Remove** command, and confirm your choice at hello prompt.</span></span>

   ![ta bort medlemmar kommandot](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. <span data-ttu-id="d5f39-122">När du har ändrat medlemmar för hello grupp, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="d5f39-122">When you finish changing members for hello group, select **Save**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="d5f39-123">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="d5f39-123">Additional information</span></span>
<span data-ttu-id="d5f39-124">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d5f39-124">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="d5f39-125">Se befintliga grupper</span><span class="sxs-lookup"><span data-stu-id="d5f39-125">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="d5f39-126">Skapa en ny grupp och lägga till medlemmar</span><span class="sxs-lookup"><span data-stu-id="d5f39-126">Create a new group and adding members</span></span>](active-directory-groups-create-azure-portal.md)
* [<span data-ttu-id="d5f39-127">Hantera inställningar för en grupp</span><span class="sxs-lookup"><span data-stu-id="d5f39-127">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="d5f39-128">Hantera medlemskap i en grupp</span><span class="sxs-lookup"><span data-stu-id="d5f39-128">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="d5f39-129">Hantera dynamiska regler för användare i en grupp</span><span class="sxs-lookup"><span data-stu-id="d5f39-129">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
