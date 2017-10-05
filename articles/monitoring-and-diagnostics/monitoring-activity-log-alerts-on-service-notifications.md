---
title: "Varningar aktivitet loggen på tjänstmeddelanden | Microsoft Docs"
description: "Håll dig informerad via SMS, e-post eller webhook när Azure-tjänsten inträffar."
author: johnkemnetz
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
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: bf6a98fd7e7e11764bef174f9efd0635fa7efe9a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-activity-log-alerts-on-service-notifications"></a><span data-ttu-id="16ae6-103">Skapa aktivitet loggen aviseringar på tjänstmeddelanden</span><span class="sxs-lookup"><span data-stu-id="16ae6-103">Create activity log alerts on service notifications</span></span>
## <a name="overview"></a><span data-ttu-id="16ae6-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="16ae6-104">Overview</span></span>
<span data-ttu-id="16ae6-105">Den här artikeln visar hur du ställer in aktiviteten loggen aviseringar för meddelanden om hälsostatus med hjälp av Azure portal.</span><span class="sxs-lookup"><span data-stu-id="16ae6-105">This article shows you how to set up activity log alerts for service health notifications by using the Azure portal.</span></span>  

<span data-ttu-id="16ae6-106">Du kan ta emot en avisering när Azure skickar meddelanden om hälsostatus till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="16ae6-106">You can receive an alert when Azure sends service health notifications to your Azure subscription.</span></span> <span data-ttu-id="16ae6-107">Du kan konfigurera aviseringen baserat på:</span><span class="sxs-lookup"><span data-stu-id="16ae6-107">You can configure the alert based on:</span></span>

- <span data-ttu-id="16ae6-108">Klass av tjänstens hälsa meddelande (incident, underhåll, information, etc.).</span><span class="sxs-lookup"><span data-stu-id="16ae6-108">The class of service health notification (incident, maintenance, information, etc.).</span></span>
- <span data-ttu-id="16ae6-109">Tjänster som påverkas.</span><span class="sxs-lookup"><span data-stu-id="16ae6-109">The service(s) affected.</span></span>
- <span data-ttu-id="16ae6-110">Regionerna påverkas.</span><span class="sxs-lookup"><span data-stu-id="16ae6-110">The region(s) affected.</span></span>
- <span data-ttu-id="16ae6-111">Status för meddelande (aktiv kontra löst).</span><span class="sxs-lookup"><span data-stu-id="16ae6-111">The status of the notification (active vs. resolved).</span></span>
- <span data-ttu-id="16ae6-112">Nivå av meddelanden (information, varning, fel).</span><span class="sxs-lookup"><span data-stu-id="16ae6-112">The level of the notifications (informational, warning, error).</span></span>

<span data-ttu-id="16ae6-113">Du kan också konfigurera som aviseringen ska skickas till:</span><span class="sxs-lookup"><span data-stu-id="16ae6-113">You also can configure who the alert should be sent to:</span></span>

- <span data-ttu-id="16ae6-114">Välj en befintlig grupp.</span><span class="sxs-lookup"><span data-stu-id="16ae6-114">Select an existing action group.</span></span>
- <span data-ttu-id="16ae6-115">Skapa en ny grupp (som kan användas för framtida aviseringar).</span><span class="sxs-lookup"><span data-stu-id="16ae6-115">Create a new action group (that can be used for future alerts).</span></span>

<span data-ttu-id="16ae6-116">Läs mer om åtgärdsgrupper i [skapa och hantera åtgärdsgrupper](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="16ae6-116">To learn more about action groups, see [Create and manage action groups](monitoring-action-groups.md).</span></span>

<span data-ttu-id="16ae6-117">Information om hur du konfigurerar tjänsten hälsovarningar för meddelande med hjälp av Azure Resource Manager-mallar finns i [Resource Manager-mallar](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="16ae6-117">For information on how to configure service health notification alerts by using Azure Resource Manager templates, see [Resource Manager templates](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span></span>

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a><span data-ttu-id="16ae6-118">Skapa en avisering på ett meddelande som tjänsten hälsa för en ny grupp med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="16ae6-118">Create an alert on a service health notification for a new action group by using the Azure portal</span></span>
1. <span data-ttu-id="16ae6-119">I den [portal](https://portal.azure.com)väljer **övervakaren**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-119">In the [portal](https://portal.azure.com), select **Monitor**.</span></span>

    ![Tjänsten ”Övervakaren”](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2. <span data-ttu-id="16ae6-121">I den **aktivitetsloggen** väljer **aviseringar**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-121">In the **Activity log** section, select **Alerts**.</span></span>

    ![Fliken ”aviseringar”](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

3. <span data-ttu-id="16ae6-123">Välj **Lägg till aktivitet loggen avisering**, och Fyll i fälten.</span><span class="sxs-lookup"><span data-stu-id="16ae6-123">Select **Add activity log alert**, and fill in the fields.</span></span>

    ![Kommandot ”Lägg till aktivitet loggen avisering”](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

4. <span data-ttu-id="16ae6-125">Ange ett namn i den **aktivitet avisering loggnamn** rutan och ange en **beskrivning**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-125">Enter a name in the **Activity log alert name** box, and provide a **Description**.</span></span>

    ![Dialogrutan ”Lägg till loggen varning”](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

5. <span data-ttu-id="16ae6-127">Den **prenumeration** rutan autofills med din aktuella prenumeration.</span><span class="sxs-lookup"><span data-stu-id="16ae6-127">The **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="16ae6-128">Den här prenumerationen används för att spara aktiviteten loggen aviseringen.</span><span class="sxs-lookup"><span data-stu-id="16ae6-128">This subscription is used to save the activity log alert.</span></span> <span data-ttu-id="16ae6-129">Aviseringen resursen har distribuerats till den här prenumerationen och övervakar händelser i aktivitetsloggen för den.</span><span class="sxs-lookup"><span data-stu-id="16ae6-129">The alert resource is deployed to this subscription and monitors events in the activity log for it.</span></span>

6. <span data-ttu-id="16ae6-130">Välj den **resursgruppen** i aviseringen resursen har skapats.</span><span class="sxs-lookup"><span data-stu-id="16ae6-130">Select the **Resource group** in which the alert resource is created.</span></span> <span data-ttu-id="16ae6-131">Det är inte den resursgrupp som övervakas av aviseringen.</span><span class="sxs-lookup"><span data-stu-id="16ae6-131">This isn't the resource group that's monitored by the alert.</span></span> <span data-ttu-id="16ae6-132">Det är istället resursgruppen där aviseringen resursen finns.</span><span class="sxs-lookup"><span data-stu-id="16ae6-132">Instead, it's the resource group where the alert resource is located.</span></span>

7. <span data-ttu-id="16ae6-133">I den **händelsekategori** väljer **Hälsotjänst**.</span><span class="sxs-lookup"><span data-stu-id="16ae6-133">In the **Event category** box, select **Service Health**.</span></span> <span data-ttu-id="16ae6-134">Alternativt, Välj den **Service**, **Region**, **typen**, **Status**, och **nivå** av tjänstens hälsa meddelanden som du vill ta emot.</span><span class="sxs-lookup"><span data-stu-id="16ae6-134">Optionally, select the **Service**, **Region**, **Type**, **Status**, and **Level** of service health notifications that you want to receive.</span></span>

8. <span data-ttu-id="16ae6-135">Under **avisering via**, Välj den **ny** åtgärdsknappen för gruppen.</span><span class="sxs-lookup"><span data-stu-id="16ae6-135">Under **Alert via**, select the **New** action group button.</span></span> <span data-ttu-id="16ae6-136">Ange ett namn i den **åtgärd gruppnamn** och ange ett namn i den **kortnamnet** rutan.</span><span class="sxs-lookup"><span data-stu-id="16ae6-136">Enter a name in the **Action group name** box, and enter a name in the **Short name** box.</span></span> <span data-ttu-id="16ae6-137">Det korta namnet refereras i meddelanden som skickas när den här aviseringen utlöses.</span><span class="sxs-lookup"><span data-stu-id="16ae6-137">The short name is referenced in the notifications that are sent when this alert fires.</span></span>

9. <span data-ttu-id="16ae6-138">Definiera en lista över mottagare genom att tillhandahålla mottagarens:</span><span class="sxs-lookup"><span data-stu-id="16ae6-138">Define a list of receivers by providing the receiver's:</span></span>

    <span data-ttu-id="16ae6-139">a.</span><span class="sxs-lookup"><span data-stu-id="16ae6-139">a.</span></span> <span data-ttu-id="16ae6-140">**Namnet**: Ange mottagarens namn, alias eller identifierare.</span><span class="sxs-lookup"><span data-stu-id="16ae6-140">**Name**: Enter the receiver’s name, alias, or identifier.</span></span>

    <span data-ttu-id="16ae6-141">b.</span><span class="sxs-lookup"><span data-stu-id="16ae6-141">b.</span></span> <span data-ttu-id="16ae6-142">**Åtgärdstyp**: Välj SMS, e-post eller webhooken.</span><span class="sxs-lookup"><span data-stu-id="16ae6-142">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="16ae6-143">c.</span><span class="sxs-lookup"><span data-stu-id="16ae6-143">c.</span></span> <span data-ttu-id="16ae6-144">**Information om**: baserat på typen av valt, ange ett telefonnummer, e-postadress eller webhook URI.</span><span class="sxs-lookup"><span data-stu-id="16ae6-144">**Details**: Based on the action type chosen, enter a phone number, email address, or webhook URI.</span></span>

10. <span data-ttu-id="16ae6-145">Välj **OK** att skapa aviseringen.</span><span class="sxs-lookup"><span data-stu-id="16ae6-145">Select **OK** to create the alert.</span></span>

<span data-ttu-id="16ae6-146">Inom några minuter aviseringen är aktiv och börjar att utlösa baserat på de villkor som du angav när du skapar.</span><span class="sxs-lookup"><span data-stu-id="16ae6-146">Within a few minutes, the alert is active and begins to trigger based on the conditions you specified during creation.</span></span>

<span data-ttu-id="16ae6-147">Information om webhook-schemat för aktiviteten loggen aviseringar finns [Webhooks för Azure aktiviteten Logga varningar](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="16ae6-147">For information on the webhook schema for activity log alerts, see [Webhooks for Azure activity log alerts](monitoring-activity-log-alerts-webhook.md).</span></span>

>[!NOTE]
><span data-ttu-id="16ae6-148">Åtgärdsgruppen som definierats i de här stegen är återanvändbara som en befintlig grupp för alla framtida definitioner för aviseringen.</span><span class="sxs-lookup"><span data-stu-id="16ae6-148">The action group defined in these steps is reusable as an existing action group for all future alert definitions.</span></span>
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a><span data-ttu-id="16ae6-149">Skapa en avisering på ett meddelande med tjänstens hälsa för en befintlig grupp med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="16ae6-149">Create an alert on a service health notification for an existing action group by using the Azure portal</span></span>

1. <span data-ttu-id="16ae6-150">Följ steg 1 till 7 i föregående avsnitt för att skapa health service-meddelande.</span><span class="sxs-lookup"><span data-stu-id="16ae6-150">Follow steps 1 through 7 in the previous section to create your service health notification.</span></span> 

2. <span data-ttu-id="16ae6-151">Under **avisering via**, Välj den **befintliga** åtgärdsknappen för gruppen.</span><span class="sxs-lookup"><span data-stu-id="16ae6-151">Under **Alert via**, select the **Existing** action group button.</span></span> <span data-ttu-id="16ae6-152">Markera gruppen lämplig åtgärd.</span><span class="sxs-lookup"><span data-stu-id="16ae6-152">Select the appropriate action group.</span></span>

3. <span data-ttu-id="16ae6-153">Välj **OK** att skapa aviseringen.</span><span class="sxs-lookup"><span data-stu-id="16ae6-153">Select **OK** to create the alert.</span></span>

<span data-ttu-id="16ae6-154">Inom några minuter aviseringen är aktiv och börjar att utlösa baserat på de villkor som du angav när du skapar.</span><span class="sxs-lookup"><span data-stu-id="16ae6-154">Within a few minutes, the alert is active and begins to trigger based on the conditions you specified during creation.</span></span>

## <a name="manage-your-alerts"></a><span data-ttu-id="16ae6-155">Hantera aviseringar</span><span class="sxs-lookup"><span data-stu-id="16ae6-155">Manage your alerts</span></span>

<span data-ttu-id="16ae6-156">När du skapar en avisering, är det visas i den **aviseringar** avsnitt i den **övervakaren** bladet.</span><span class="sxs-lookup"><span data-stu-id="16ae6-156">After you create an alert, it's visible in the **Alerts** section of the **Monitor** blade.</span></span> <span data-ttu-id="16ae6-157">Väljer du den avisering du vill hantera att:</span><span class="sxs-lookup"><span data-stu-id="16ae6-157">Select the alert you want to manage to:</span></span>

* <span data-ttu-id="16ae6-158">Redigera den.</span><span class="sxs-lookup"><span data-stu-id="16ae6-158">Edit it.</span></span>
* <span data-ttu-id="16ae6-159">Ta bort den.</span><span class="sxs-lookup"><span data-stu-id="16ae6-159">Delete it.</span></span>
* <span data-ttu-id="16ae6-160">Inaktivera eller aktivera den, om du vill att tillfälligt stoppa eller återuppta tar emot meddelanden om aviseringen.</span><span class="sxs-lookup"><span data-stu-id="16ae6-160">Disable or enable it, if you want to temporarily stop or resume receiving notifications for the alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="16ae6-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16ae6-161">Next steps</span></span>
- <span data-ttu-id="16ae6-162">Lär dig mer om [tjänsten meddelanden om hälsostatus](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="16ae6-162">Learn about [service health notifications](monitoring-service-notifications.md).</span></span>
- <span data-ttu-id="16ae6-163">Lär dig mer om [meddelande hastighetsbegränsning](monitoring-alerts-rate-limiting.md).</span><span class="sxs-lookup"><span data-stu-id="16ae6-163">Learn about [notification rate limiting](monitoring-alerts-rate-limiting.md).</span></span>
- <span data-ttu-id="16ae6-164">Granska de [avisering webhook för aktivitetslogg](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="16ae6-164">Review the [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>
- <span data-ttu-id="16ae6-165">Hämta en [översikt över aktivitet loggen aviseringar](monitoring-overview-alerts.md), och lära dig hur du tar emot aviseringar.</span><span class="sxs-lookup"><span data-stu-id="16ae6-165">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how to receive alerts.</span></span> 
- <span data-ttu-id="16ae6-166">Lär dig mer om [åtgärdsgrupper](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="16ae6-166">Learn more about [action groups](monitoring-action-groups.md).</span></span>
