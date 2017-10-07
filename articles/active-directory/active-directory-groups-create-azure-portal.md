---
title: "aaaCreate en grupp för användare i Azure Active Directory | Microsoft Docs"
description: "Hur toocreate en grupp i Azure Active Directory och lägga till medlemmar toohello grupp"
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
ms.openlocfilehash: fc583a7f02ce50e7f3b2c8f97a9c032a3e2dc33a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a><span data-ttu-id="c4ba3-103">Skapa en grupp och lägga till medlemmar i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c4ba3-103">Create a group and add members in Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c4ba3-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c4ba3-104">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="c4ba3-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="c4ba3-105">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="c4ba3-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c4ba3-106">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="c4ba3-107">Den här artikeln förklarar hur toocreate och fylla i en ny grupp i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c4ba3-107">This article explains how toocreate and populate a new group in Azure Active Directory.</span></span> <span data-ttu-id="c4ba3-108">Använd en grupp tooperform hanteringsuppgifter, till exempel tilldela licenser eller behörigheter tooa antalet användare eller enheter på samma gång.</span><span class="sxs-lookup"><span data-stu-id="c4ba3-108">Use a group tooperform management tasks such as assigning licenses or permissions tooa number of users or devices at once.</span></span>

## <a name="how-do-i-create-a-group"></a><span data-ttu-id="c4ba3-109">Hur skapar jag en grupp?</span><span class="sxs-lookup"><span data-stu-id="c4ba3-109">How do I create a group?</span></span>
1. <span data-ttu-id="c4ba3-110">Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="c4ba3-110">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="c4ba3-111">Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="c4ba3-111">Select **More services**, enter **User and groups** in hello text box, and then select **Enter**.</span></span>

   ![Öppna användarhantering](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. <span data-ttu-id="c4ba3-113">På hello **användare och grupper** bladet väljer **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="c4ba3-113">On hello **Users and groups** blade, select **All groups**.</span></span>

   ![Öppna hello grupper bladet](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="c4ba3-115">På hello **användare och grupper – alla grupper** bladet, Välj hello **Lägg till** kommando.</span><span class="sxs-lookup"><span data-stu-id="c4ba3-115">On hello **Users and groups - All groups** blade, select hello **Add** command.</span></span>

   ![Att välja hello tilläggskommando](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. <span data-ttu-id="c4ba3-117">På hello **grupp** bladet Lägg till ett namn och beskrivning för hello grupp.</span><span class="sxs-lookup"><span data-stu-id="c4ba3-117">On hello **Group** blade, add a name and description for hello group.</span></span>
6. <span data-ttu-id="c4ba3-118">tooselect medlemmar tooadd toohello gruppen väljer **tilldelad** i hello **medlemskapstypen** och välj sedan **medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="c4ba3-118">tooselect members tooadd toohello group, select **Assigned** in hello **Membership type** box, and then select **Members**.</span></span> <span data-ttu-id="c4ba3-119">Mer information om hur toomanage hello medlemskap i en grupp dynamiskt finns [genom att använda attribut toocreate avancerade regler för gruppmedlemskap](active-directory-groups-dynamic-membership-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c4ba3-119">For more information about how toomanage hello membership of a group dynamically, see [Using attributes toocreate advanced rules for group membership](active-directory-groups-dynamic-membership-azure-portal.md).</span></span>

   ![Att välja medlemmar tooadd](./media/active-directory-groups-create-azure-portal/select-members.png)
7. <span data-ttu-id="c4ba3-121">På hello **medlemmar** bladet, Välj en eller flera användare eller enheter tooadd toohello gruppen och välj hello **Välj** knappen längst ned hello hello bladet tooadd dem toohello grupp.</span><span class="sxs-lookup"><span data-stu-id="c4ba3-121">On hello **Members** blade, select one or more users or devices tooadd toohello group and select hello **Select** button at hello bottom of hello blade tooadd them toohello group.</span></span> <span data-ttu-id="c4ba3-122">Hej **användaren** filtrerar hello visas baserat på matchning post tooany-tillhör ett namn för användaren eller enheten.</span><span class="sxs-lookup"><span data-stu-id="c4ba3-122">hello **User** box filters hello display based on matching your entry tooany part of a user or device name.</span></span> <span data-ttu-id="c4ba3-123">Inga jokertecken godkänns i rutan.</span><span class="sxs-lookup"><span data-stu-id="c4ba3-123">No wildcard characters are accepted in that box.</span></span>
8. <span data-ttu-id="c4ba3-124">När du har lagt till medlemmar toohello gruppen, Välj **skapa** på hello **grupp** bladet.</span><span class="sxs-lookup"><span data-stu-id="c4ba3-124">When you finish adding members toohello group, select **Create** on hello **Group** blade.</span></span>    

   ![Skapa grupp bekräftelse](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a><span data-ttu-id="c4ba3-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c4ba3-126">Next steps</span></span>
<span data-ttu-id="c4ba3-127">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c4ba3-127">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="c4ba3-128">Se befintliga grupper</span><span class="sxs-lookup"><span data-stu-id="c4ba3-128">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="c4ba3-129">Hantera inställningar för en grupp</span><span class="sxs-lookup"><span data-stu-id="c4ba3-129">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="c4ba3-130">Hantera medlemmar i en grupp</span><span class="sxs-lookup"><span data-stu-id="c4ba3-130">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="c4ba3-131">Hantera medlemskap i en grupp</span><span class="sxs-lookup"><span data-stu-id="c4ba3-131">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="c4ba3-132">Hantera dynamiska regler för användare i en grupp</span><span class="sxs-lookup"><span data-stu-id="c4ba3-132">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
