---
title: "Skapa och hantera åtgärdsgrupper i Azure portal | Microsoft Docs"
description: "Lär dig hur du skapar och hanterar åtgärdsgrupper i Azure-portalen."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: ancav
ms.openlocfilehash: ea15705bf02d9773507c6cb59f2da4c1dd0f9d77
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a><span data-ttu-id="6b59d-103">Skapa och hantera åtgärdsgrupper i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6b59d-103">Create and manage action groups in the Azure portal</span></span>
## <a name="overview"></a><span data-ttu-id="6b59d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="6b59d-104">Overview</span></span> ##
<span data-ttu-id="6b59d-105">Den här artikeln visar hur du skapar och hanterar åtgärdsgrupper i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6b59d-105">This article shows you how to create and manage action groups in the Azure portal.</span></span>

<span data-ttu-id="6b59d-106">Du kan konfigurera en lista med åtgärder med åtgärdsgrupper.</span><span class="sxs-lookup"><span data-stu-id="6b59d-106">You can configure a list of actions with action groups.</span></span> <span data-ttu-id="6b59d-107">Dessa grupper kan sedan användas när du definierar aktiviteten loggen aviseringar.</span><span class="sxs-lookup"><span data-stu-id="6b59d-107">These groups then can be used when you define activity log alerts.</span></span> <span data-ttu-id="6b59d-108">Dessa grupper kan sedan återanvändas av varje aktivitet loggen aviseringen som du definierar, se till att samma åtgärder vidtas för varje gång som aktiviteten loggen avisering utlöses.</span><span class="sxs-lookup"><span data-stu-id="6b59d-108">These groups can then be reused by each activity log alert you define, ensuring that the same actions are taken each time the activity log alert is triggered.</span></span>

<span data-ttu-id="6b59d-109">En grupp kan ha upp till 10 för varje åtgärdstyp av.</span><span class="sxs-lookup"><span data-stu-id="6b59d-109">An action group can have up to 10 of each action type.</span></span> <span data-ttu-id="6b59d-110">Varje åtgärd består av följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="6b59d-110">Each action is made up of the following properties:</span></span>

* <span data-ttu-id="6b59d-111">**Namnet**: en unik identifierare inom åtgärdsgruppen.</span><span class="sxs-lookup"><span data-stu-id="6b59d-111">**Name**: A unique identifier within the action group.</span></span>  
* <span data-ttu-id="6b59d-112">**Åtgärdstyp**: skicka ett SMS, skicka ett e-postmeddelande eller anropa en webhook.</span><span class="sxs-lookup"><span data-stu-id="6b59d-112">**Action type**: Send an SMS, send an email, or call a webhook.</span></span>  
* <span data-ttu-id="6b59d-113">**Information om**: motsvarande phone nummer, e-postadress eller webhook URI.</span><span class="sxs-lookup"><span data-stu-id="6b59d-113">**Details**: The corresponding phone number, email address, or webhook URI.</span></span>

<span data-ttu-id="6b59d-114">Information om hur du använder Azure Resource Manager-mallar för att konfigurera åtgärdsgrupper finns [åtgärd grupp Resource Manager-mallar](monitoring-create-action-group-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="6b59d-114">For information on how to use Azure Resource Manager templates to configure action groups, see [Action group Resource Manager templates](monitoring-create-action-group-with-resource-manager-template.md).</span></span>

## <a name="create-an-action-group-by-using-the-azure-portal"></a><span data-ttu-id="6b59d-115">Skapa en grupp med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="6b59d-115">Create an action group by using the Azure portal</span></span> ##
1. <span data-ttu-id="6b59d-116">I den [portal](https://portal.azure.com)väljer **övervakaren**.</span><span class="sxs-lookup"><span data-stu-id="6b59d-116">In the [portal](https://portal.azure.com), select **Monitor**.</span></span> <span data-ttu-id="6b59d-117">Den **övervakaren** bladet konsoliderar alla dina övervakning inställningar och data i en vy.</span><span class="sxs-lookup"><span data-stu-id="6b59d-117">The **Monitor** blade consolidates all your monitoring settings and data in one view.</span></span>

    ![Tjänsten ”Övervakaren”](./media/monitoring-action-groups/home-monitor.png)
2. <span data-ttu-id="6b59d-119">I den **aktivitetsloggen** väljer **åtgärdsgrupper**.</span><span class="sxs-lookup"><span data-stu-id="6b59d-119">In the **Activity log** section, select **Action groups**.</span></span>

    ![Fliken ”åtgärdsgrupper”](./media/monitoring-action-groups/action-groups-blade.png)
3. <span data-ttu-id="6b59d-121">Välj **Lägg till grupp**, och Fyll i fälten.</span><span class="sxs-lookup"><span data-stu-id="6b59d-121">Select **Add action group**, and fill in the fields.</span></span>

    ![Kommandot ”Lägg till grupp”](./media/monitoring-action-groups/add-action-group.png)
4. <span data-ttu-id="6b59d-123">Ange ett namn i den **åtgärd gruppnamn** och ange ett namn i den **kortnamnet** rutan.</span><span class="sxs-lookup"><span data-stu-id="6b59d-123">Enter a name in the **Action group name** box, and enter a name in the **Short name** box.</span></span> <span data-ttu-id="6b59d-124">Det korta namnet används i stället för en fullständig åtgärd gruppnamn när meddelanden som skickas med den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="6b59d-124">The short name is used in place of a full action group name when notifications are sent using this group.</span></span>

      ![Dialogrutan Lägg till grupp för åtgärden ”](./media/monitoring-action-groups/action-group-define.png)

5. <span data-ttu-id="6b59d-126">Den **prenumeration** rutan autofills med din aktuella prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6b59d-126">The **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="6b59d-127">Den här prenumerationen är den som åtgärdsgruppen sparas.</span><span class="sxs-lookup"><span data-stu-id="6b59d-127">This subscription is the one in which the action group is saved.</span></span>

6. <span data-ttu-id="6b59d-128">Välj den **resursgruppen** i åtgärdsgruppen har sparats.</span><span class="sxs-lookup"><span data-stu-id="6b59d-128">Select the **Resource group** in which the action group is saved.</span></span>

7. <span data-ttu-id="6b59d-129">Definiera en lista med åtgärder genom att ge varje åtgärd:</span><span class="sxs-lookup"><span data-stu-id="6b59d-129">Define a list of actions by providing each action's:</span></span>

    <span data-ttu-id="6b59d-130">a.</span><span class="sxs-lookup"><span data-stu-id="6b59d-130">a.</span></span> <span data-ttu-id="6b59d-131">**Namnet**: Ange en unik identifierare för den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="6b59d-131">**Name**: Enter a unique identifier for this action.</span></span>

    <span data-ttu-id="6b59d-132">b.</span><span class="sxs-lookup"><span data-stu-id="6b59d-132">b.</span></span> <span data-ttu-id="6b59d-133">**Åtgärdstyp**: Välj SMS, e-post eller webhooken.</span><span class="sxs-lookup"><span data-stu-id="6b59d-133">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="6b59d-134">c.</span><span class="sxs-lookup"><span data-stu-id="6b59d-134">c.</span></span> <span data-ttu-id="6b59d-135">**Information om**: baserat på typen av, ange ett telefonnummer, e-postadress eller webhook URI.</span><span class="sxs-lookup"><span data-stu-id="6b59d-135">**Details**: Based on the action type, enter a phone number, email address, or webhook URI.</span></span>

8. <span data-ttu-id="6b59d-136">Välj **OK** att skapa åtgärdsgruppen.</span><span class="sxs-lookup"><span data-stu-id="6b59d-136">Select **OK** to create the action group.</span></span>

## <a name="manage-your-action-groups"></a><span data-ttu-id="6b59d-137">Hantera åtgärdsgrupper</span><span class="sxs-lookup"><span data-stu-id="6b59d-137">Manage your action groups</span></span> ##
<span data-ttu-id="6b59d-138">När du skapar en grupp, är det visas i den **åtgärdsgrupper** avsnitt i den **övervakaren** bladet.</span><span class="sxs-lookup"><span data-stu-id="6b59d-138">After you create an action group, it's visible in the **Action groups** section of the **Monitor** blade.</span></span> <span data-ttu-id="6b59d-139">Välj den grupp du vill hantera att:</span><span class="sxs-lookup"><span data-stu-id="6b59d-139">Select the action group you want to manage to:</span></span>

* <span data-ttu-id="6b59d-140">Lägga till, redigera eller ta bort åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6b59d-140">Add, edit, or remove actions.</span></span>
* <span data-ttu-id="6b59d-141">Ta bort åtgärdsgruppen.</span><span class="sxs-lookup"><span data-stu-id="6b59d-141">Delete the action group.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b59d-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6b59d-142">Next steps</span></span> ##
* <span data-ttu-id="6b59d-143">Lär dig mer om [SMS Varna beteende](monitoring-sms-alert-behavior.md).</span><span class="sxs-lookup"><span data-stu-id="6b59d-143">Learn more about [SMS alert behavior](monitoring-sms-alert-behavior.md).</span></span>  
* <span data-ttu-id="6b59d-144">Få en [förståelse av aviseringen webhook för aktivitetslogg](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="6b59d-144">Gain an [understanding of the activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>  
* <span data-ttu-id="6b59d-145">Lär dig mer om [hastighetsbegränsning](monitoring-alerts-rate-limiting.md) aviseringar för.</span><span class="sxs-lookup"><span data-stu-id="6b59d-145">Learn more about [rate limiting](monitoring-alerts-rate-limiting.md) on alerts.</span></span> 
* <span data-ttu-id="6b59d-146">Hämta en [översikt över aktivitet loggen aviseringar](monitoring-overview-alerts.md), och lära dig hur du tar emot aviseringar.</span><span class="sxs-lookup"><span data-stu-id="6b59d-146">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how to receive alerts.</span></span>  
* <span data-ttu-id="6b59d-147">Lär dig hur du [konfigurera aviseringar när ett meddelande om tjänstens hälsa är bokförd](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="6b59d-147">Learn how to [configure alerts whenever a service health notification is posted](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
