---
title: "aaaReceive aktivitet Logga varningar på tjänstmeddelanden | Microsoft Docs"
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
ms.openlocfilehash: dd35e8f39d2a522efdae4dfed20779c992c1dd27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-activity-log-alerts-on-service-notifications"></a><span data-ttu-id="04c79-103">Skapa aktivitet loggen aviseringar på tjänstmeddelanden</span><span class="sxs-lookup"><span data-stu-id="04c79-103">Create activity log alerts on service notifications</span></span>
## <a name="overview"></a><span data-ttu-id="04c79-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="04c79-104">Overview</span></span>
<span data-ttu-id="04c79-105">Den här artikeln visar hur tooset in aktivitetsloggen aviseringar för meddelanden om hälsostatus med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="04c79-105">This article shows you how tooset up activity log alerts for service health notifications by using hello Azure portal.</span></span>  

<span data-ttu-id="04c79-106">Du kan ta emot en avisering när Azure skickar tjänsten hälsa meddelanden tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="04c79-106">You can receive an alert when Azure sends service health notifications tooyour Azure subscription.</span></span> <span data-ttu-id="04c79-107">Du kan konfigurera hello avisering baserat på:</span><span class="sxs-lookup"><span data-stu-id="04c79-107">You can configure hello alert based on:</span></span>

- <span data-ttu-id="04c79-108">hello klass av tjänstens hälsa meddelande (incident, underhåll, information, etc.).</span><span class="sxs-lookup"><span data-stu-id="04c79-108">hello class of service health notification (incident, maintenance, information, etc.).</span></span>
- <span data-ttu-id="04c79-109">hello tjänster påverkas.</span><span class="sxs-lookup"><span data-stu-id="04c79-109">hello service(s) affected.</span></span>
- <span data-ttu-id="04c79-110">Hej region(erna) påverkas.</span><span class="sxs-lookup"><span data-stu-id="04c79-110">hello region(s) affected.</span></span>
- <span data-ttu-id="04c79-111">hello status för hello-meddelande (aktiv kontra löst).</span><span class="sxs-lookup"><span data-stu-id="04c79-111">hello status of hello notification (active vs. resolved).</span></span>
- <span data-ttu-id="04c79-112">hello andelen hello meddelanden (information, varning, fel).</span><span class="sxs-lookup"><span data-stu-id="04c79-112">hello level of hello notifications (informational, warning, error).</span></span>

<span data-ttu-id="04c79-113">Du kan också konfigurera som hello avisering ska skickas till:</span><span class="sxs-lookup"><span data-stu-id="04c79-113">You also can configure who hello alert should be sent to:</span></span>

- <span data-ttu-id="04c79-114">Välj en befintlig grupp.</span><span class="sxs-lookup"><span data-stu-id="04c79-114">Select an existing action group.</span></span>
- <span data-ttu-id="04c79-115">Skapa en ny grupp (som kan användas för framtida aviseringar).</span><span class="sxs-lookup"><span data-stu-id="04c79-115">Create a new action group (that can be used for future alerts).</span></span>

<span data-ttu-id="04c79-116">toolearn mer om åtgärdsgrupper finns [skapa och hantera åtgärdsgrupper](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="04c79-116">toolearn more about action groups, see [Create and manage action groups](monitoring-action-groups.md).</span></span>

<span data-ttu-id="04c79-117">Mer information om hur tooconfigure service hälsa postmeddelanden aviseringar med hjälp av Azure Resource Manager-mallar finns [Resource Manager-mallar](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="04c79-117">For information on how tooconfigure service health notification alerts by using Azure Resource Manager templates, see [Resource Manager templates](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span></span>

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-hello-azure-portal"></a><span data-ttu-id="04c79-118">Skapa en avisering på ett meddelande som tjänsten hälsa för en ny grupp med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="04c79-118">Create an alert on a service health notification for a new action group by using hello Azure portal</span></span>
1. <span data-ttu-id="04c79-119">I hello [portal](https://portal.azure.com)väljer **övervakaren**.</span><span class="sxs-lookup"><span data-stu-id="04c79-119">In hello [portal](https://portal.azure.com), select **Monitor**.</span></span>

    ![Hej ”övervakningstjänsten”](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2. <span data-ttu-id="04c79-121">I hello **aktivitetsloggen** väljer **aviseringar**.</span><span class="sxs-lookup"><span data-stu-id="04c79-121">In hello **Activity log** section, select **Alerts**.</span></span>

    ![Hej ”” aviseringsfliken](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

3. <span data-ttu-id="04c79-123">Välj **Lägg till aktivitet loggen avisering**, och hello fält fylls i.</span><span class="sxs-lookup"><span data-stu-id="04c79-123">Select **Add activity log alert**, and fill in hello fields.</span></span>

    ![Hej ”Lägg till loggen varning” kommando](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

4. <span data-ttu-id="04c79-125">Ange ett namn i hello **aktivitet avisering loggnamn** rutan och ange en **beskrivning**.</span><span class="sxs-lookup"><span data-stu-id="04c79-125">Enter a name in hello **Activity log alert name** box, and provide a **Description**.</span></span>

    ![Hej ”Lägg till loggen varning” dialogrutan](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

5. <span data-ttu-id="04c79-127">Hej **prenumeration** rutan autofills med din aktuella prenumeration.</span><span class="sxs-lookup"><span data-stu-id="04c79-127">hello **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="04c79-128">Den här prenumerationen har använt toosave hello loggen varning.</span><span class="sxs-lookup"><span data-stu-id="04c79-128">This subscription is used toosave hello activity log alert.</span></span> <span data-ttu-id="04c79-129">hello avisering resursen är distribuerade toothis prenumeration och Övervakare händelser i hello aktivitetsloggen för den.</span><span class="sxs-lookup"><span data-stu-id="04c79-129">hello alert resource is deployed toothis subscription and monitors events in hello activity log for it.</span></span>

6. <span data-ttu-id="04c79-130">Välj hello **resursgruppen** i vilka hello avisering resursen har skapats.</span><span class="sxs-lookup"><span data-stu-id="04c79-130">Select hello **Resource group** in which hello alert resource is created.</span></span> <span data-ttu-id="04c79-131">Det är inte hello resursgruppen som övervakas av hello aviseringen.</span><span class="sxs-lookup"><span data-stu-id="04c79-131">This isn't hello resource group that's monitored by hello alert.</span></span> <span data-ttu-id="04c79-132">Det är hello resursgruppen där hello avisering resursen finns.</span><span class="sxs-lookup"><span data-stu-id="04c79-132">Instead, it's hello resource group where hello alert resource is located.</span></span>

7. <span data-ttu-id="04c79-133">I hello **händelsekategori** väljer **Hälsotjänst**.</span><span class="sxs-lookup"><span data-stu-id="04c79-133">In hello **Event category** box, select **Service Health**.</span></span> <span data-ttu-id="04c79-134">Alternativt, Välj hello **Service**, **Region**, **typen**, **Status**, och **nivå** för tjänsten meddelanden om hälsostatus som du vill tooreceive.</span><span class="sxs-lookup"><span data-stu-id="04c79-134">Optionally, select hello **Service**, **Region**, **Type**, **Status**, and **Level** of service health notifications that you want tooreceive.</span></span>

8. <span data-ttu-id="04c79-135">Under **avisering via**väljer hello **ny** åtgärdsknappen för gruppen.</span><span class="sxs-lookup"><span data-stu-id="04c79-135">Under **Alert via**, select hello **New** action group button.</span></span> <span data-ttu-id="04c79-136">Ange ett namn i hello **åtgärd gruppnamn** och ange ett namn i hello **kortnamnet** rutan.</span><span class="sxs-lookup"><span data-stu-id="04c79-136">Enter a name in hello **Action group name** box, and enter a name in hello **Short name** box.</span></span> <span data-ttu-id="04c79-137">hello kortnamnet refereras i hello-meddelanden som skickas när den här aviseringen utlöses.</span><span class="sxs-lookup"><span data-stu-id="04c79-137">hello short name is referenced in hello notifications that are sent when this alert fires.</span></span>

9. <span data-ttu-id="04c79-138">Definiera en lista över mottagare genom att tillhandahålla hello mottagare:</span><span class="sxs-lookup"><span data-stu-id="04c79-138">Define a list of receivers by providing hello receiver's:</span></span>

    <span data-ttu-id="04c79-139">a.</span><span class="sxs-lookup"><span data-stu-id="04c79-139">a.</span></span> <span data-ttu-id="04c79-140">**Namnet**: Ange hello mottagare, alias eller identifierare.</span><span class="sxs-lookup"><span data-stu-id="04c79-140">**Name**: Enter hello receiver’s name, alias, or identifier.</span></span>

    <span data-ttu-id="04c79-141">b.</span><span class="sxs-lookup"><span data-stu-id="04c79-141">b.</span></span> <span data-ttu-id="04c79-142">**Åtgärdstyp**: Välj SMS, e-post eller webhooken.</span><span class="sxs-lookup"><span data-stu-id="04c79-142">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="04c79-143">c.</span><span class="sxs-lookup"><span data-stu-id="04c79-143">c.</span></span> <span data-ttu-id="04c79-144">**Information om**: baserat på hello åtgärdstyp valt, ange ett telefonnummer, e-postadress eller webhook URI.</span><span class="sxs-lookup"><span data-stu-id="04c79-144">**Details**: Based on hello action type chosen, enter a phone number, email address, or webhook URI.</span></span>

10. <span data-ttu-id="04c79-145">Välj **OK** toocreate hello avisering.</span><span class="sxs-lookup"><span data-stu-id="04c79-145">Select **OK** toocreate hello alert.</span></span>

<span data-ttu-id="04c79-146">Inom några minuter hello aviseringen är aktiv och börjar tootrigger baserat på hello villkor som du angav när du skapar.</span><span class="sxs-lookup"><span data-stu-id="04c79-146">Within a few minutes, hello alert is active and begins tootrigger based on hello conditions you specified during creation.</span></span>

<span data-ttu-id="04c79-147">Mer information om hello webhook-schemat för aktiviteten loggen aviseringar finns [Webhooks för Azure aktiviteten Logga varningar](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="04c79-147">For information on hello webhook schema for activity log alerts, see [Webhooks for Azure activity log alerts](monitoring-activity-log-alerts-webhook.md).</span></span>

>[!NOTE]
><span data-ttu-id="04c79-148">hello åtgärdsgrupp som definierats i de här stegen är återanvändbara som en befintlig grupp för alla framtida definitioner för aviseringen.</span><span class="sxs-lookup"><span data-stu-id="04c79-148">hello action group defined in these steps is reusable as an existing action group for all future alert definitions.</span></span>
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-hello-azure-portal"></a><span data-ttu-id="04c79-149">Skapa en avisering på ett meddelande med tjänstens hälsa för en befintlig grupp med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="04c79-149">Create an alert on a service health notification for an existing action group by using hello Azure portal</span></span>

1. <span data-ttu-id="04c79-150">Följ steg 1 till 7 i hello föregående avsnitt toocreate health service-meddelande.</span><span class="sxs-lookup"><span data-stu-id="04c79-150">Follow steps 1 through 7 in hello previous section toocreate your service health notification.</span></span> 

2. <span data-ttu-id="04c79-151">Under **avisering via**väljer hello **befintliga** åtgärdsknappen för gruppen.</span><span class="sxs-lookup"><span data-stu-id="04c79-151">Under **Alert via**, select hello **Existing** action group button.</span></span> <span data-ttu-id="04c79-152">Välj grupp för hello lämplig åtgärd.</span><span class="sxs-lookup"><span data-stu-id="04c79-152">Select hello appropriate action group.</span></span>

3. <span data-ttu-id="04c79-153">Välj **OK** toocreate hello avisering.</span><span class="sxs-lookup"><span data-stu-id="04c79-153">Select **OK** toocreate hello alert.</span></span>

<span data-ttu-id="04c79-154">Inom några minuter hello aviseringen är aktiv och börjar tootrigger baserat på hello villkor som du angav när du skapar.</span><span class="sxs-lookup"><span data-stu-id="04c79-154">Within a few minutes, hello alert is active and begins tootrigger based on hello conditions you specified during creation.</span></span>

## <a name="manage-your-alerts"></a><span data-ttu-id="04c79-155">Hantera aviseringar</span><span class="sxs-lookup"><span data-stu-id="04c79-155">Manage your alerts</span></span>

<span data-ttu-id="04c79-156">När du skapar en avisering, är det visas i hello **aviseringar** avsnitt i hello **övervakaren** bladet.</span><span class="sxs-lookup"><span data-stu-id="04c79-156">After you create an alert, it's visible in hello **Alerts** section of hello **Monitor** blade.</span></span> <span data-ttu-id="04c79-157">Välj hello avisering du vill toomanage till:</span><span class="sxs-lookup"><span data-stu-id="04c79-157">Select hello alert you want toomanage to:</span></span>

* <span data-ttu-id="04c79-158">Redigera den.</span><span class="sxs-lookup"><span data-stu-id="04c79-158">Edit it.</span></span>
* <span data-ttu-id="04c79-159">Ta bort den.</span><span class="sxs-lookup"><span data-stu-id="04c79-159">Delete it.</span></span>
* <span data-ttu-id="04c79-160">Inaktivera eller aktivera den, om du vill stoppa tootemporarily eller återuppta tar emot meddelanden om hello avisering.</span><span class="sxs-lookup"><span data-stu-id="04c79-160">Disable or enable it, if you want tootemporarily stop or resume receiving notifications for hello alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="04c79-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="04c79-161">Next steps</span></span>
- <span data-ttu-id="04c79-162">Lär dig mer om [tjänsten meddelanden om hälsostatus](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="04c79-162">Learn about [service health notifications](monitoring-service-notifications.md).</span></span>
- <span data-ttu-id="04c79-163">Lär dig mer om [meddelande hastighetsbegränsning](monitoring-alerts-rate-limiting.md).</span><span class="sxs-lookup"><span data-stu-id="04c79-163">Learn about [notification rate limiting](monitoring-alerts-rate-limiting.md).</span></span>
- <span data-ttu-id="04c79-164">Granska hello [avisering webhook för aktivitetslogg](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="04c79-164">Review hello [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>
- <span data-ttu-id="04c79-165">Hämta en [översikt över aktivitet loggen aviseringar](monitoring-overview-alerts.md), och lära dig hur tooreceive aviseringar.</span><span class="sxs-lookup"><span data-stu-id="04c79-165">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how tooreceive alerts.</span></span> 
- <span data-ttu-id="04c79-166">Lär dig mer om [åtgärdsgrupper](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="04c79-166">Learn more about [action groups](monitoring-action-groups.md).</span></span>
