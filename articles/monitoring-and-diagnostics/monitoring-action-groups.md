---
title: "aaaCreate och hantera åtgärdsgrupper i hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur toocreate och hantera åtgärdsgrupper i hello Azure-portalen."
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
ms.openlocfilehash: 97e0b22bea7787fff6856f895a7e6256c177efd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-action-groups-in-hello-azure-portal"></a><span data-ttu-id="fa41e-103">Skapa och hantera åtgärdsgrupper i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fa41e-103">Create and manage action groups in hello Azure portal</span></span>
## <a name="overview"></a><span data-ttu-id="fa41e-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="fa41e-104">Overview</span></span> ##
<span data-ttu-id="fa41e-105">Den här artikeln beskrivs hur du toocreate och hantera åtgärdsgrupper i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fa41e-105">This article shows you how toocreate and manage action groups in hello Azure portal.</span></span>

<span data-ttu-id="fa41e-106">Du kan konfigurera en lista med åtgärder med åtgärdsgrupper.</span><span class="sxs-lookup"><span data-stu-id="fa41e-106">You can configure a list of actions with action groups.</span></span> <span data-ttu-id="fa41e-107">Dessa grupper kan sedan användas när du definierar aktiviteten loggen aviseringar.</span><span class="sxs-lookup"><span data-stu-id="fa41e-107">These groups then can be used when you define activity log alerts.</span></span> <span data-ttu-id="fa41e-108">Dessa grupper kan sedan återanvändas av varje aktivitet loggen aviseringen som du definierar, se till att hello samma åtgärder vidtas för varje gång hello loggen varning utlöses.</span><span class="sxs-lookup"><span data-stu-id="fa41e-108">These groups can then be reused by each activity log alert you define, ensuring that hello same actions are taken each time hello activity log alert is triggered.</span></span>

<span data-ttu-id="fa41e-109">En grupp kan ha upp too10 för varje åtgärdstyp av.</span><span class="sxs-lookup"><span data-stu-id="fa41e-109">An action group can have up too10 of each action type.</span></span> <span data-ttu-id="fa41e-110">Varje åtgärd består av hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="fa41e-110">Each action is made up of hello following properties:</span></span>

* <span data-ttu-id="fa41e-111">**Namnet**: en unik identifierare inom hello grupp.</span><span class="sxs-lookup"><span data-stu-id="fa41e-111">**Name**: A unique identifier within hello action group.</span></span>  
* <span data-ttu-id="fa41e-112">**Åtgärdstyp**: skicka ett SMS, skicka ett e-postmeddelande eller anropa en webhook.</span><span class="sxs-lookup"><span data-stu-id="fa41e-112">**Action type**: Send an SMS, send an email, or call a webhook.</span></span>  
* <span data-ttu-id="fa41e-113">**Information om**: hello motsvarande telefonnummer, e-postadress eller webhook URI.</span><span class="sxs-lookup"><span data-stu-id="fa41e-113">**Details**: hello corresponding phone number, email address, or webhook URI.</span></span>

<span data-ttu-id="fa41e-114">Mer information om hur toouse Azure Resource Manager-mallar tooconfigure åtgärdsgrupper, se [åtgärd grupp Resource Manager-mallar](monitoring-create-action-group-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="fa41e-114">For information on how toouse Azure Resource Manager templates tooconfigure action groups, see [Action group Resource Manager templates](monitoring-create-action-group-with-resource-manager-template.md).</span></span>

## <a name="create-an-action-group-by-using-hello-azure-portal"></a><span data-ttu-id="fa41e-115">Skapa en grupp med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fa41e-115">Create an action group by using hello Azure portal</span></span> ##
1. <span data-ttu-id="fa41e-116">I hello [portal](https://portal.azure.com)väljer **övervakaren**.</span><span class="sxs-lookup"><span data-stu-id="fa41e-116">In hello [portal](https://portal.azure.com), select **Monitor**.</span></span> <span data-ttu-id="fa41e-117">Hej **övervakaren** bladet konsoliderar alla dina övervakning inställningar och data i en vy.</span><span class="sxs-lookup"><span data-stu-id="fa41e-117">hello **Monitor** blade consolidates all your monitoring settings and data in one view.</span></span>

    ![Hej ”övervakningstjänsten”](./media/monitoring-action-groups/home-monitor.png)
2. <span data-ttu-id="fa41e-119">I hello **aktivitetsloggen** väljer **åtgärdsgrupper**.</span><span class="sxs-lookup"><span data-stu-id="fa41e-119">In hello **Activity log** section, select **Action groups**.</span></span>

    ![Hej ”åtgärdsgrupper” fliken](./media/monitoring-action-groups/action-groups-blade.png)
3. <span data-ttu-id="fa41e-121">Välj **Lägg till grupp**, och hello fält fylls i.</span><span class="sxs-lookup"><span data-stu-id="fa41e-121">Select **Add action group**, and fill in hello fields.</span></span>

    ![Hej ”Lägg till grupp” kommando](./media/monitoring-action-groups/add-action-group.png)
4. <span data-ttu-id="fa41e-123">Ange ett namn i hello **åtgärd gruppnamn** och ange ett namn i hello **kortnamnet** rutan.</span><span class="sxs-lookup"><span data-stu-id="fa41e-123">Enter a name in hello **Action group name** box, and enter a name in hello **Short name** box.</span></span> <span data-ttu-id="fa41e-124">hello kort namn används i stället för en fullständig åtgärd gruppnamn när meddelanden som skickas med den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="fa41e-124">hello short name is used in place of a full action group name when notifications are sent using this group.</span></span>

      ![dialogrutan hello Lägg till grupp ”](./media/monitoring-action-groups/action-group-define.png)

5. <span data-ttu-id="fa41e-126">Hej **prenumeration** rutan autofills med din aktuella prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fa41e-126">hello **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="fa41e-127">Den här prenumerationen är hello något i vilken åtgärd hello-grupp har sparats.</span><span class="sxs-lookup"><span data-stu-id="fa41e-127">This subscription is hello one in which hello action group is saved.</span></span>

6. <span data-ttu-id="fa41e-128">Välj hello **resursgruppen** där hello åtgärd spara gruppen.</span><span class="sxs-lookup"><span data-stu-id="fa41e-128">Select hello **Resource group** in which hello action group is saved.</span></span>

7. <span data-ttu-id="fa41e-129">Definiera en lista med åtgärder genom att ge varje åtgärd:</span><span class="sxs-lookup"><span data-stu-id="fa41e-129">Define a list of actions by providing each action's:</span></span>

    <span data-ttu-id="fa41e-130">a.</span><span class="sxs-lookup"><span data-stu-id="fa41e-130">a.</span></span> <span data-ttu-id="fa41e-131">**Namnet**: Ange en unik identifierare för den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="fa41e-131">**Name**: Enter a unique identifier for this action.</span></span>

    <span data-ttu-id="fa41e-132">b.</span><span class="sxs-lookup"><span data-stu-id="fa41e-132">b.</span></span> <span data-ttu-id="fa41e-133">**Åtgärdstyp**: Välj SMS, e-post eller webhooken.</span><span class="sxs-lookup"><span data-stu-id="fa41e-133">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="fa41e-134">c.</span><span class="sxs-lookup"><span data-stu-id="fa41e-134">c.</span></span> <span data-ttu-id="fa41e-135">**Information om**: baserat på hello åtgärdstyp, ange ett telefonnummer, e-postadress eller webhook URI.</span><span class="sxs-lookup"><span data-stu-id="fa41e-135">**Details**: Based on hello action type, enter a phone number, email address, or webhook URI.</span></span>

8. <span data-ttu-id="fa41e-136">Välj **OK** toocreate hello grupp.</span><span class="sxs-lookup"><span data-stu-id="fa41e-136">Select **OK** toocreate hello action group.</span></span>

## <a name="manage-your-action-groups"></a><span data-ttu-id="fa41e-137">Hantera åtgärdsgrupper</span><span class="sxs-lookup"><span data-stu-id="fa41e-137">Manage your action groups</span></span> ##
<span data-ttu-id="fa41e-138">När du har skapat en grupp, den är synlig i hello **åtgärdsgrupper** avsnitt i hello **övervakaren** bladet.</span><span class="sxs-lookup"><span data-stu-id="fa41e-138">After you create an action group, it's visible in hello **Action groups** section of hello **Monitor** blade.</span></span> <span data-ttu-id="fa41e-139">Välj hello åtgärd du vill toomanage till:</span><span class="sxs-lookup"><span data-stu-id="fa41e-139">Select hello action group you want toomanage to:</span></span>

* <span data-ttu-id="fa41e-140">Lägga till, redigera eller ta bort åtgärder.</span><span class="sxs-lookup"><span data-stu-id="fa41e-140">Add, edit, or remove actions.</span></span>
* <span data-ttu-id="fa41e-141">Ta bort hello grupp.</span><span class="sxs-lookup"><span data-stu-id="fa41e-141">Delete hello action group.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa41e-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fa41e-142">Next steps</span></span> ##
* <span data-ttu-id="fa41e-143">Lär dig mer om [SMS Varna beteende](monitoring-sms-alert-behavior.md).</span><span class="sxs-lookup"><span data-stu-id="fa41e-143">Learn more about [SMS alert behavior](monitoring-sms-alert-behavior.md).</span></span>  
* <span data-ttu-id="fa41e-144">Få en [förståelse av hello avisering webhook för aktivitetslogg](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="fa41e-144">Gain an [understanding of hello activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>  
* <span data-ttu-id="fa41e-145">Lär dig mer om [hastighetsbegränsning](monitoring-alerts-rate-limiting.md) aviseringar för.</span><span class="sxs-lookup"><span data-stu-id="fa41e-145">Learn more about [rate limiting](monitoring-alerts-rate-limiting.md) on alerts.</span></span> 
* <span data-ttu-id="fa41e-146">Hämta en [översikt över aktivitet loggen aviseringar](monitoring-overview-alerts.md), och lära dig hur tooreceive aviseringar.</span><span class="sxs-lookup"><span data-stu-id="fa41e-146">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how tooreceive alerts.</span></span>  
* <span data-ttu-id="fa41e-147">Lär dig hur för[konfigurera aviseringar när ett meddelande om tjänstens hälsa är bokförd](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="fa41e-147">Learn how too[configure alerts whenever a service health notification is posted](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
