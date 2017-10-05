---
title: Skapa aktivitet loggen aviseringar | Microsoft Docs
description: "Ett meddelande via SMS, webhook och e-post när vissa händelser inträffar i aktivitetsloggen."
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
ms.date: 08/03/2017
ms.author: johnkem
ms.openlocfilehash: 3885469ec0e1fcc31386dd0ad7fe6cb5d03ab28e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-activity-log-alerts"></a><span data-ttu-id="f16b8-103">Skapa aktivitet Logga varningar</span><span class="sxs-lookup"><span data-stu-id="f16b8-103">Create activity log alerts</span></span>

## <a name="overview"></a><span data-ttu-id="f16b8-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="f16b8-104">Overview</span></span>
<span data-ttu-id="f16b8-105">Aktiviteten loggen aviseringar är aviseringar som aktiverar när en ny aktivitet logga en händelse inträffar som matchar de villkor som anges i aviseringen.</span><span class="sxs-lookup"><span data-stu-id="f16b8-105">Activity log alerts are alerts that activate when a new activity log event occurs that matches the conditions specified in the alert.</span></span> <span data-ttu-id="f16b8-106">De är Azure-resurser, så att de kan skapas med hjälp av en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="f16b8-106">They are Azure resources, so they can be created by using an Azure Resource Manager template.</span></span> <span data-ttu-id="f16b8-107">De kan också skapa, uppdatera, eller tas bort i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f16b8-107">They also can be created, updated, or deleted in the Azure portal.</span></span> <span data-ttu-id="f16b8-108">Den här artikeln beskriver begrepp bakom aktivitet loggen aviseringar.</span><span class="sxs-lookup"><span data-stu-id="f16b8-108">This article introduces the concepts behind activity log alerts.</span></span> <span data-ttu-id="f16b8-109">Den sedan visar hur du använder Azure-portalen för att ställa in en avisering på aktiviteten logghändelser.</span><span class="sxs-lookup"><span data-stu-id="f16b8-109">It then shows you how to use the Azure portal to set up an alert on activity log events.</span></span>

<span data-ttu-id="f16b8-110">Normalt skapar du aktiviteten loggen aviseringar för att ta emot meddelanden när:</span><span class="sxs-lookup"><span data-stu-id="f16b8-110">Typically, you create activity log alerts to receive notifications when:</span></span>

* <span data-ttu-id="f16b8-111">Vissa ändringar sker för resurser i din Azure-prenumeration som omfattar ofta enskilda resursgrupper eller resurser.</span><span class="sxs-lookup"><span data-stu-id="f16b8-111">Specific changes occur on resources in your Azure subscription, often scoped to particular resource groups or resources.</span></span> <span data-ttu-id="f16b8-112">Du kanske vill meddelas när en virtuell dator i myProductionResourceGroup tas bort.</span><span class="sxs-lookup"><span data-stu-id="f16b8-112">For example, you might want to be notified when any virtual machine in myProductionResourceGroup is deleted.</span></span> <span data-ttu-id="f16b8-113">Eller så kanske du vill meddelas om några nya roller har tilldelats en användare i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f16b8-113">Or, you might want to be notified if any new roles are assigned to a user in your subscription.</span></span>
* <span data-ttu-id="f16b8-114">En service hälsa händelse inträffar.</span><span class="sxs-lookup"><span data-stu-id="f16b8-114">A service health event occurs.</span></span> <span data-ttu-id="f16b8-115">Tjänsten hälsa händelser innehåller meddelanden om incidenter och underhållshändelser som gäller för resurser i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f16b8-115">Service health events include notification of incidents and maintenance events that apply to resources in your subscription.</span></span>

<span data-ttu-id="f16b8-116">I båda fallen övervakar en aktivitet loggen avisering bara för händelser i prenumerationen som aviseringen skapades.</span><span class="sxs-lookup"><span data-stu-id="f16b8-116">In either case, an activity log alert monitors only for events in the subscription in which the alert is created.</span></span>

<span data-ttu-id="f16b8-117">Du kan konfigurera en aktivitet loggen avisering baserat på någon översta egenskap i JSON-objekt för en aktivitet händelselogg.</span><span class="sxs-lookup"><span data-stu-id="f16b8-117">You can configure an activity log alert based on any top-level property in the JSON object for an activity log event.</span></span> <span data-ttu-id="f16b8-118">Dock visar portalen de vanligaste alternativen:</span><span class="sxs-lookup"><span data-stu-id="f16b8-118">However, the portal shows the most common options:</span></span>

- <span data-ttu-id="f16b8-119">**Kategori**: administrativa, tjänsten hälsa och Autoskala rekommendation.</span><span class="sxs-lookup"><span data-stu-id="f16b8-119">**Category**: Administrative, Service Health, Autoscale, and Recommendation.</span></span> <span data-ttu-id="f16b8-120">Mer information finns i [översikt över Azure aktivitetsloggen](./monitoring-overview-activity-logs.md#categories-in-the-activity-log).</span><span class="sxs-lookup"><span data-stu-id="f16b8-120">For more information, see [Overview of the Azure activity log](./monitoring-overview-activity-logs.md#categories-in-the-activity-log).</span></span> <span data-ttu-id="f16b8-121">Mer information om hälsa av tjänsten-händelser finns [varningar aktivitet loggen på tjänstmeddelanden](./monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="f16b8-121">To learn more about service health events, see [Receive activity log alerts on service notifications](./monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
- <span data-ttu-id="f16b8-122">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="f16b8-122">**Resource group**</span></span>
- <span data-ttu-id="f16b8-123">**Resurs**</span><span class="sxs-lookup"><span data-stu-id="f16b8-123">**Resource**</span></span>
- <span data-ttu-id="f16b8-124">**Resurstyp**</span><span class="sxs-lookup"><span data-stu-id="f16b8-124">**Resource type**</span></span>
- <span data-ttu-id="f16b8-125">**Åtgärdsnamnet**: The Resource Manager Role-Based åtkomstkontroll åtgärdsnamn.</span><span class="sxs-lookup"><span data-stu-id="f16b8-125">**Operation name**: The Resource Manager Role-Based Access Control operation name.</span></span>
- <span data-ttu-id="f16b8-126">**Nivå**: allvarlighetsgrad för händelse (utförlig information, varning, fel eller kritiskt).</span><span class="sxs-lookup"><span data-stu-id="f16b8-126">**Level**: The severity level of the event (Verbose, Informational, Warning, Error, or Critical).</span></span>
- <span data-ttu-id="f16b8-127">**Status för**: status för händelsen, startas normalt, misslyckades eller lyckades.</span><span class="sxs-lookup"><span data-stu-id="f16b8-127">**Status**: The status of the event, typically Started, Failed, or Succeeded.</span></span>
- <span data-ttu-id="f16b8-128">**Händelse som initieras av**: kallas även ”anroparen”.</span><span class="sxs-lookup"><span data-stu-id="f16b8-128">**Event initiated by**: Also known as the "caller."</span></span> <span data-ttu-id="f16b8-129">E-postadressen eller Azure Active Directory-identifieraren för användaren som utförde åtgärden.</span><span class="sxs-lookup"><span data-stu-id="f16b8-129">The email address or Azure Active Directory identifier of the user who performed the operation.</span></span>

>[!NOTE]
><span data-ttu-id="f16b8-130">Du måste ange minst två av de föregående villkoren i aviseringen, med en är kategorin.</span><span class="sxs-lookup"><span data-stu-id="f16b8-130">You must specify at least two of the preceding criteria in your alert, with one being the category.</span></span> <span data-ttu-id="f16b8-131">Du kan inte skapa en avisering som aktiveras varje gång en händelse skapas i aktivitetsloggarna.</span><span class="sxs-lookup"><span data-stu-id="f16b8-131">You may not create an alert that activates every time an event is created in the activity logs.</span></span>
>
>

<span data-ttu-id="f16b8-132">När en aktivitet loggen avisering aktiveras använder en grupp för att generera meddelanden eller åtgärder.</span><span class="sxs-lookup"><span data-stu-id="f16b8-132">When an activity log alert is activated, it uses an action group to generate actions or notifications.</span></span> <span data-ttu-id="f16b8-133">En grupp är en återanvändbar uppsättning notification mottagare, till exempel e-postadresser, webhook URL: er eller SMS telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="f16b8-133">An action group is a reusable set of notification receivers, such as email addresses, webhook URLs, or SMS phone numbers.</span></span> <span data-ttu-id="f16b8-134">Mottagarna kan refereras från flera aviseringar centralisera och gruppera dina aviseringskanaler.</span><span class="sxs-lookup"><span data-stu-id="f16b8-134">The receivers can be referenced from multiple alerts to centralize and group your notification channels.</span></span> <span data-ttu-id="f16b8-135">När du definierar aviseringen aktivitet loggen har du två alternativ.</span><span class="sxs-lookup"><span data-stu-id="f16b8-135">When you define your activity log alert, you have two options.</span></span> <span data-ttu-id="f16b8-136">Du kan:</span><span class="sxs-lookup"><span data-stu-id="f16b8-136">You can:</span></span>

* <span data-ttu-id="f16b8-137">Använd en befintlig grupp i din logg varning.</span><span class="sxs-lookup"><span data-stu-id="f16b8-137">Use an existing action group in your activity log alert.</span></span> 
* <span data-ttu-id="f16b8-138">Skapa en ny grupp.</span><span class="sxs-lookup"><span data-stu-id="f16b8-138">Create a new action group.</span></span> 

<span data-ttu-id="f16b8-139">Läs mer om åtgärdsgrupper i [skapa och hantera åtgärdsgrupper i Azure portal](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="f16b8-139">To learn more about action groups, see [Create and manage action groups in the Azure portal](monitoring-action-groups.md).</span></span>

<span data-ttu-id="f16b8-140">Läs mer om meddelanden om hälsostatus i [varningar aktivitet loggen på meddelanden om hälsostatus](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="f16b8-140">To learn more about service health notifications, see [Receive activity log alerts on service health notifications](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>

## <a name="create-an-alert-on-an-activity-log-event-with-a-new-action-group-by-using-the-azure-portal"></a><span data-ttu-id="f16b8-141">Skapa en avisering på en aktivitet logga en händelse med en ny grupp med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="f16b8-141">Create an alert on an activity log event with a new action group by using the Azure portal</span></span>
1. <span data-ttu-id="f16b8-142">I den [portal](https://portal.azure.com)väljer **övervakaren**.</span><span class="sxs-lookup"><span data-stu-id="f16b8-142">In the [portal](https://portal.azure.com), select **Monitor**.</span></span>

    ![Tjänsten ”Övervakaren”](./media/monitoring-activity-log-alerts/home-monitor.png)
2. <span data-ttu-id="f16b8-144">I den **aktivitetsloggen** väljer **aviseringar**.</span><span class="sxs-lookup"><span data-stu-id="f16b8-144">In the **Activity log** section, select **Alerts**.</span></span>

    ![Fliken ”aviseringar”](./media/monitoring-activity-log-alerts/alerts-blades.png)
3. <span data-ttu-id="f16b8-146">Välj **Lägg till aktivitet loggen avisering**, och Fyll i fälten.</span><span class="sxs-lookup"><span data-stu-id="f16b8-146">Select **Add activity log alert**, and fill in the fields.</span></span>

4. <span data-ttu-id="f16b8-147">Ange ett namn i den **aktivitet avisering loggnamn** och välj en **beskrivning**.</span><span class="sxs-lookup"><span data-stu-id="f16b8-147">Enter a name in the **Activity log alert name** box, and select a **Description**.</span></span>

    ![Kommandot ”Lägg till aktivitet loggen avisering”](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

5. <span data-ttu-id="f16b8-149">Den **prenumeration** rutan autofills med din aktuella prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f16b8-149">The **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="f16b8-150">Den här prenumerationen är den som åtgärdsgruppen sparas.</span><span class="sxs-lookup"><span data-stu-id="f16b8-150">This subscription is the one in which the action group is saved.</span></span> <span data-ttu-id="f16b8-151">Aviseringen resursen har distribuerats till den här prenumerationen och övervakar aktivitetshändelser från den.</span><span class="sxs-lookup"><span data-stu-id="f16b8-151">The alert resource is deployed to this subscription and monitors activity log events from it.</span></span>

    ![Dialogrutan ”Lägg till loggen varning”](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

6. <span data-ttu-id="f16b8-153">Välj den **resursgruppen** i aviseringen resursen har skapats.</span><span class="sxs-lookup"><span data-stu-id="f16b8-153">Select the **Resource group** in which the alert resource is created.</span></span> <span data-ttu-id="f16b8-154">Detta är inte den resursgrupp som övervakas av aviseringen.</span><span class="sxs-lookup"><span data-stu-id="f16b8-154">This is not the resource group that's monitored by the alert.</span></span> <span data-ttu-id="f16b8-155">Det är istället resursgruppen där aviseringen resursen finns.</span><span class="sxs-lookup"><span data-stu-id="f16b8-155">Instead, it's the resource group where the alert resource is located.</span></span>

7. <span data-ttu-id="f16b8-156">Du kan också välja en **händelsekategori** att ändra ytterligare filter som visas.</span><span class="sxs-lookup"><span data-stu-id="f16b8-156">Optionally, select an **Event category** to modify the additional filters that are shown.</span></span> <span data-ttu-id="f16b8-157">För administrativa händelser filtren är **resursgruppen**, **resurs**, **resurstypen**, **åtgärdsnamn**, **Nivå**, **Status**, och **händelse som initieras av**.</span><span class="sxs-lookup"><span data-stu-id="f16b8-157">For Administrative events, the filters include **Resource group**, **Resource**, **Resource type**, **Operation name**, **Level**, **Status**, and **Event initiated by**.</span></span> <span data-ttu-id="f16b8-158">Dessa värden identifiera vilka händelser som bör du övervaka den här aviseringen.</span><span class="sxs-lookup"><span data-stu-id="f16b8-158">These values identify which events this alert should monitor.</span></span>

    >[!NOTE]
    ><span data-ttu-id="f16b8-159">Du måste ange minst en av de föregående villkoren i aviseringen.</span><span class="sxs-lookup"><span data-stu-id="f16b8-159">You must specify at least one of the preceding criteria in your alert.</span></span> <span data-ttu-id="f16b8-160">Du kan inte skapa en avisering som aktiveras varje gång en händelse skapas i aktivitetsloggarna.</span><span class="sxs-lookup"><span data-stu-id="f16b8-160">You may not create an alert that activates every time an event is created in the activity logs.</span></span>
    >
    >

8. <span data-ttu-id="f16b8-161">Ange ett namn i den **åtgärd gruppnamn** och ange ett namn i den **kortnamnet** rutan.</span><span class="sxs-lookup"><span data-stu-id="f16b8-161">Enter a name in the **Action group name** box, and enter a name in the **Short name** box.</span></span> <span data-ttu-id="f16b8-162">Det korta namnet används i stället för en fullständig åtgärd gruppnamn när meddelanden som skickas med den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="f16b8-162">The short name is used in place of a full action group name when notifications are sent using this group.</span></span>

9.  <span data-ttu-id="f16b8-163">Definiera en lista med åtgärder genom att tillhandahålla åtgärden:</span><span class="sxs-lookup"><span data-stu-id="f16b8-163">Define a list of actions by providing the action’s:</span></span>

    <span data-ttu-id="f16b8-164">a.</span><span class="sxs-lookup"><span data-stu-id="f16b8-164">a.</span></span> <span data-ttu-id="f16b8-165">**Namnet**: Ange åtgärdens namn, alias eller identifierare.</span><span class="sxs-lookup"><span data-stu-id="f16b8-165">**Name**: Enter the action’s name, alias, or identifier.</span></span>

    <span data-ttu-id="f16b8-166">b.</span><span class="sxs-lookup"><span data-stu-id="f16b8-166">b.</span></span> <span data-ttu-id="f16b8-167">**Åtgärdstyp**: Välj SMS, e-post eller webhooken.</span><span class="sxs-lookup"><span data-stu-id="f16b8-167">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="f16b8-168">c.</span><span class="sxs-lookup"><span data-stu-id="f16b8-168">c.</span></span> <span data-ttu-id="f16b8-169">**Information om**: baserat på typen av, ange ett telefonnummer, e-postadress eller webhook URI.</span><span class="sxs-lookup"><span data-stu-id="f16b8-169">**Details**: Based on the action type, enter a phone number, email address, or webhook URI.</span></span>

10. <span data-ttu-id="f16b8-170">Välj **OK** att skapa aviseringen.</span><span class="sxs-lookup"><span data-stu-id="f16b8-170">Select **OK** to create the alert.</span></span>

<span data-ttu-id="f16b8-171">Aviseringen tar några minuter att sprida fullständigt och sedan aktiveras.</span><span class="sxs-lookup"><span data-stu-id="f16b8-171">The alert takes a few minutes to fully propagate and then become active.</span></span> <span data-ttu-id="f16b8-172">Utlöser nya händelser som matchar den avisering kriterier.</span><span class="sxs-lookup"><span data-stu-id="f16b8-172">It triggers when new events match the alert's criteria.</span></span>

<span data-ttu-id="f16b8-173">Mer information finns i [förstå webhook-schemat som används i loggen Aktivitetsaviseringar](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="f16b8-173">For more information, see [Understand the webhook schema used in activity log alerts](monitoring-activity-log-alerts-webhook.md).</span></span>

>[!NOTE]
><span data-ttu-id="f16b8-174">Åtgärdsgruppen som definierats i de här stegen är återanvändbara som en befintlig grupp för alla framtida definitioner för aviseringen.</span><span class="sxs-lookup"><span data-stu-id="f16b8-174">The action group defined in these steps is reusable as an existing action group for all future alert definitions.</span></span>
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-the-azure-portal"></a><span data-ttu-id="f16b8-175">Skapa en avisering på en aktivitet logga en händelse för en befintlig grupp med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="f16b8-175">Create an alert on an activity log event for an existing action group by using the Azure portal</span></span>
1. <span data-ttu-id="f16b8-176">Följ steg 1 till 7 i föregående avsnitt för att skapa aviseringen aktivitet loggen.</span><span class="sxs-lookup"><span data-stu-id="f16b8-176">Follow steps 1 through 7 in the previous section to create your activity log alert.</span></span>

2. <span data-ttu-id="f16b8-177">Under **meddela**, Välj den **befintliga** åtgärdsknappen för gruppen.</span><span class="sxs-lookup"><span data-stu-id="f16b8-177">Under **Notify via**, select the **Existing** action group button.</span></span> <span data-ttu-id="f16b8-178">Välj en befintlig grupp i listan.</span><span class="sxs-lookup"><span data-stu-id="f16b8-178">Select an existing action group from the list.</span></span>

3. <span data-ttu-id="f16b8-179">Välj **OK** att skapa aviseringen.</span><span class="sxs-lookup"><span data-stu-id="f16b8-179">Select **OK** to create the alert.</span></span>

<span data-ttu-id="f16b8-180">Aviseringen tar några minuter att sprida fullständigt och sedan aktiveras.</span><span class="sxs-lookup"><span data-stu-id="f16b8-180">The alert takes a few minutes to fully propagate and then become active.</span></span> <span data-ttu-id="f16b8-181">Utlöser nya händelser som matchar den avisering kriterier.</span><span class="sxs-lookup"><span data-stu-id="f16b8-181">It triggers when new events match the alert's criteria.</span></span>

## <a name="manage-your-alerts"></a><span data-ttu-id="f16b8-182">Hantera aviseringar</span><span class="sxs-lookup"><span data-stu-id="f16b8-182">Manage your alerts</span></span>

<span data-ttu-id="f16b8-183">När du skapar en avisering, är det visas i avsnittet aviseringar i övervakaren bladet.</span><span class="sxs-lookup"><span data-stu-id="f16b8-183">After you create an alert, it's visible in the Alerts section of the Monitor blade.</span></span> <span data-ttu-id="f16b8-184">Väljer du den avisering du vill hantera att:</span><span class="sxs-lookup"><span data-stu-id="f16b8-184">Select the alert you want to manage to:</span></span>

* <span data-ttu-id="f16b8-185">Redigera den.</span><span class="sxs-lookup"><span data-stu-id="f16b8-185">Edit it.</span></span>
* <span data-ttu-id="f16b8-186">Ta bort den.</span><span class="sxs-lookup"><span data-stu-id="f16b8-186">Delete it.</span></span>
* <span data-ttu-id="f16b8-187">Inaktivera eller aktivera den, om du vill att tillfälligt stoppa eller återuppta tar emot meddelanden om aviseringen.</span><span class="sxs-lookup"><span data-stu-id="f16b8-187">Disable or enable it, if you want to temporarily stop or resume receiving notifications for the alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f16b8-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f16b8-188">Next steps</span></span>
- <span data-ttu-id="f16b8-189">Hämta en [översikt över aviseringar](monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="f16b8-189">Get an [overview of alerts](monitoring-overview-alerts.md).</span></span>
- <span data-ttu-id="f16b8-190">Lär dig mer om [meddelande hastighetsbegränsning](monitoring-alerts-rate-limiting.md).</span><span class="sxs-lookup"><span data-stu-id="f16b8-190">Learn about [notification rate limiting](monitoring-alerts-rate-limiting.md).</span></span>
- <span data-ttu-id="f16b8-191">Granska de [avisering webhook för aktivitetslogg](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="f16b8-191">Review the [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>
- <span data-ttu-id="f16b8-192">Lär dig mer om [åtgärdsgrupper](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="f16b8-192">Learn more about [action groups](monitoring-action-groups.md).</span></span>  
- <span data-ttu-id="f16b8-193">Lär dig mer om [tjänsten meddelanden om hälsostatus](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="f16b8-193">Learn about [service health notifications](monitoring-service-notifications.md).</span></span>
- <span data-ttu-id="f16b8-194">Skapa en [loggen varning att övervaka alla Autoskala motorn åtgärder för din prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span><span class="sxs-lookup"><span data-stu-id="f16b8-194">Create an [activity log alert to monitor all autoscale engine operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span></span>
- <span data-ttu-id="f16b8-195">Skapa en [loggen varning att övervaka alla misslyckade Autoskala skala-/ skalbar åtgärder för din prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span><span class="sxs-lookup"><span data-stu-id="f16b8-195">Create an [activity log alert to monitor all failed autoscale scale-in/scale-out operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span></span>
