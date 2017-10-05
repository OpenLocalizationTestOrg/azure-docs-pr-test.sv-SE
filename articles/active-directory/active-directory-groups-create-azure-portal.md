---
title: "Skapa en grupp för användare i Azure Active Directory | Microsoft Docs"
description: "Hur du skapar en grupp i Azure Active Directory och lägga till medlemmar i gruppen"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6d3d37761a9fdf9bd9801396d45f2fcd47efb0be
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a><span data-ttu-id="6d1a2-103">Skapa en grupp och lägga till medlemmar i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6d1a2-103">Create a group and add members in Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6d1a2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6d1a2-104">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="6d1a2-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="6d1a2-105">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="6d1a2-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6d1a2-106">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="6d1a2-107">Den här artikeln förklarar hur du skapar och fylla i en ny grupp i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6d1a2-107">This article explains how to create and populate a new group in Azure Active Directory.</span></span> <span data-ttu-id="6d1a2-108">Använda en grupp för att utföra hanteringsuppgifter, till exempel tilldela licenser eller behörigheter till ett antal användare eller enheter på samma gång.</span><span class="sxs-lookup"><span data-stu-id="6d1a2-108">Use a group to perform management tasks such as assigning licenses or permissions to a number of users or devices at once.</span></span>

## <a name="how-do-i-create-a-group"></a><span data-ttu-id="6d1a2-109">Hur skapar jag en grupp?</span><span class="sxs-lookup"><span data-stu-id="6d1a2-109">How do I create a group?</span></span>
1. <span data-ttu-id="6d1a2-110">Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="6d1a2-110">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="6d1a2-111">Välj **fler tjänster**, ange **användare och grupper** i textrutan och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="6d1a2-111">Select **More services**, enter **User and groups** in the text box, and then select **Enter**.</span></span>

   ![Öppna användarhantering](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. <span data-ttu-id="6d1a2-113">På den **användare och grupper** bladet väljer **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="6d1a2-113">On the **Users and groups** blade, select **All groups**.</span></span>

   ![Öppna bladet grupper](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="6d1a2-115">På den **användare och grupper – alla grupper** bladet väljer den **Lägg till** kommando.</span><span class="sxs-lookup"><span data-stu-id="6d1a2-115">On the **Users and groups - All groups** blade, select the **Add** command.</span></span>

   ![Att välja kommandot Lägg till](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. <span data-ttu-id="6d1a2-117">På den **grupp** bladet Lägg till ett namn och en beskrivning för gruppen.</span><span class="sxs-lookup"><span data-stu-id="6d1a2-117">On the **Group** blade, add a name and description for the group.</span></span>
6. <span data-ttu-id="6d1a2-118">Om du vill välja medlemmar som ska läggas till i gruppen **tilldelad** i den **medlemskapstypen** och välj sedan **medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="6d1a2-118">To select members to add to the group, select **Assigned** in the **Membership type** box, and then select **Members**.</span></span> <span data-ttu-id="6d1a2-119">Mer information om hur du hanterar medlemskap i en grupp dynamiskt finns [använda attribut för att skapa avancerade regler för gruppmedlemskap](active-directory-groups-dynamic-membership-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6d1a2-119">For more information about how to manage the membership of a group dynamically, see [Using attributes to create advanced rules for group membership](active-directory-groups-dynamic-membership-azure-portal.md).</span></span>

   ![Att välja medlemmar för att lägga till](./media/active-directory-groups-create-azure-portal/select-members.png)
7. <span data-ttu-id="6d1a2-121">På den **medlemmar** bladet, Välj en eller flera användare eller enheter att lägga till i gruppen och välj den **Välj** längst ned på bladet för att lägga till dem i gruppen.</span><span class="sxs-lookup"><span data-stu-id="6d1a2-121">On the **Members** blade, select one or more users or devices to add to the group and select the **Select** button at the bottom of the blade to add them to the group.</span></span> <span data-ttu-id="6d1a2-122">Den **användaren** rutan filtrerar baserat på matchning inmatningen till alla delar av namnet på en användare eller enhet.</span><span class="sxs-lookup"><span data-stu-id="6d1a2-122">The **User** box filters the display based on matching your entry to any part of a user or device name.</span></span> <span data-ttu-id="6d1a2-123">Inga jokertecken godkänns i rutan.</span><span class="sxs-lookup"><span data-stu-id="6d1a2-123">No wildcard characters are accepted in that box.</span></span>
8. <span data-ttu-id="6d1a2-124">När du är klar med att lägga till medlemmar i gruppen, Välj **skapa** på den **grupp** bladet.</span><span class="sxs-lookup"><span data-stu-id="6d1a2-124">When you finish adding members to the group, select **Create** on the **Group** blade.</span></span>    

   ![Skapa grupp bekräftelse](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a><span data-ttu-id="6d1a2-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6d1a2-126">Next steps</span></span>
<span data-ttu-id="6d1a2-127">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6d1a2-127">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="6d1a2-128">Se befintliga grupper</span><span class="sxs-lookup"><span data-stu-id="6d1a2-128">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="6d1a2-129">Hantera inställningar för en grupp</span><span class="sxs-lookup"><span data-stu-id="6d1a2-129">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="6d1a2-130">Hantera medlemmar i en grupp</span><span class="sxs-lookup"><span data-stu-id="6d1a2-130">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="6d1a2-131">Hantera medlemskap i en grupp</span><span class="sxs-lookup"><span data-stu-id="6d1a2-131">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="6d1a2-132">Hantera dynamiska regler för användare i en grupp</span><span class="sxs-lookup"><span data-stu-id="6d1a2-132">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
