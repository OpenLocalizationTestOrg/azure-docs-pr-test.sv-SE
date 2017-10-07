---
title: aaaCreate aktivitet loggen aviseringar | Microsoft Docs
description: "Ett meddelande via SMS, webhook och e-post när vissa händelser inträffar i hello aktivitetsloggen."
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
ms.openlocfilehash: ba0716cc12a0b3a0024ee5562a025f3f153f8982
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-activity-log-alerts"></a><span data-ttu-id="12d1c-103">Skapa aktivitet Logga varningar</span><span class="sxs-lookup"><span data-stu-id="12d1c-103">Create activity log alerts</span></span>

## <a name="overview"></a><span data-ttu-id="12d1c-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="12d1c-104">Overview</span></span>
<span data-ttu-id="12d1c-105">Aktiviteten loggen aviseringar är aviseringar som aktiverar när en ny aktivitet logga en händelse inträffar som matchar hello villkor som anges i hello avisering.</span><span class="sxs-lookup"><span data-stu-id="12d1c-105">Activity log alerts are alerts that activate when a new activity log event occurs that matches hello conditions specified in hello alert.</span></span> <span data-ttu-id="12d1c-106">De är Azure-resurser, så att de kan skapas med hjälp av en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="12d1c-106">They are Azure resources, so they can be created by using an Azure Resource Manager template.</span></span> <span data-ttu-id="12d1c-107">De kan också skapa, uppdatera, eller tas bort i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="12d1c-107">They also can be created, updated, or deleted in hello Azure portal.</span></span> <span data-ttu-id="12d1c-108">Den här artikeln introducerar hello koncepten bakom aktivitet loggen aviseringar.</span><span class="sxs-lookup"><span data-stu-id="12d1c-108">This article introduces hello concepts behind activity log alerts.</span></span> <span data-ttu-id="12d1c-109">Den sedan visar hur toouse hello Azure portal tooset upp en avisering på aktiviteten logghändelser.</span><span class="sxs-lookup"><span data-stu-id="12d1c-109">It then shows you how toouse hello Azure portal tooset up an alert on activity log events.</span></span>

<span data-ttu-id="12d1c-110">Normalt skapar du aktiviteten loggen aviseringar tooreceive meddelanden när:</span><span class="sxs-lookup"><span data-stu-id="12d1c-110">Typically, you create activity log alerts tooreceive notifications when:</span></span>

* <span data-ttu-id="12d1c-111">Vissa ändringar sker på resurser i Azure-prenumeration, ofta begränsade tooparticular resursgrupper eller resurser.</span><span class="sxs-lookup"><span data-stu-id="12d1c-111">Specific changes occur on resources in your Azure subscription, often scoped tooparticular resource groups or resources.</span></span> <span data-ttu-id="12d1c-112">Exempelvis kanske du vill toobe meddelas när en virtuell dator i myProductionResourceGroup tas bort.</span><span class="sxs-lookup"><span data-stu-id="12d1c-112">For example, you might want toobe notified when any virtual machine in myProductionResourceGroup is deleted.</span></span> <span data-ttu-id="12d1c-113">Eller så kanske du vill toobe meddelas om några nya roller har tilldelats tooa användare i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="12d1c-113">Or, you might want toobe notified if any new roles are assigned tooa user in your subscription.</span></span>
* <span data-ttu-id="12d1c-114">En service hälsa händelse inträffar.</span><span class="sxs-lookup"><span data-stu-id="12d1c-114">A service health event occurs.</span></span> <span data-ttu-id="12d1c-115">Tjänstens hälsa händelser innehåller meddelanden om incidenter och underhållshändelser som gäller tooresources i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="12d1c-115">Service health events include notification of incidents and maintenance events that apply tooresources in your subscription.</span></span>

<span data-ttu-id="12d1c-116">I båda fallen övervakar en aktivitet loggen avisering bara för händelser i hello prenumeration i vilka hello avisering skapas.</span><span class="sxs-lookup"><span data-stu-id="12d1c-116">In either case, an activity log alert monitors only for events in hello subscription in which hello alert is created.</span></span>

<span data-ttu-id="12d1c-117">Du kan konfigurera en aktivitet loggen avisering baserat på någon översta egenskap i hello JSON-objekt för en aktivitet händelselogg.</span><span class="sxs-lookup"><span data-stu-id="12d1c-117">You can configure an activity log alert based on any top-level property in hello JSON object for an activity log event.</span></span> <span data-ttu-id="12d1c-118">Dock visar hello portal hello de vanligaste alternativen:</span><span class="sxs-lookup"><span data-stu-id="12d1c-118">However, hello portal shows hello most common options:</span></span>

- <span data-ttu-id="12d1c-119">**Kategori**: administrativa, tjänsten hälsa och Autoskala rekommendation.</span><span class="sxs-lookup"><span data-stu-id="12d1c-119">**Category**: Administrative, Service Health, Autoscale, and Recommendation.</span></span> <span data-ttu-id="12d1c-120">Mer information finns i [översikt över hello Azure-aktivitetsloggen](./monitoring-overview-activity-logs.md#categories-in-the-activity-log).</span><span class="sxs-lookup"><span data-stu-id="12d1c-120">For more information, see [Overview of hello Azure activity log](./monitoring-overview-activity-logs.md#categories-in-the-activity-log).</span></span> <span data-ttu-id="12d1c-121">toolearn mer om tjänstens hälsa händelser, se [varningar aktivitet loggen på tjänstmeddelanden](./monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="12d1c-121">toolearn more about service health events, see [Receive activity log alerts on service notifications](./monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
- <span data-ttu-id="12d1c-122">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="12d1c-122">**Resource group**</span></span>
- <span data-ttu-id="12d1c-123">**Resurs**</span><span class="sxs-lookup"><span data-stu-id="12d1c-123">**Resource**</span></span>
- <span data-ttu-id="12d1c-124">**Resurstyp**</span><span class="sxs-lookup"><span data-stu-id="12d1c-124">**Resource type**</span></span>
- <span data-ttu-id="12d1c-125">**Åtgärdsnamnet**: hello åtgärdsnamn för rollbaserad åtkomstkontroll i Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="12d1c-125">**Operation name**: hello Resource Manager Role-Based Access Control operation name.</span></span>
- <span data-ttu-id="12d1c-126">**Nivå**: hello allvarlighetsgraden hello-händelse (utförlig information, varning, fel eller kritiskt).</span><span class="sxs-lookup"><span data-stu-id="12d1c-126">**Level**: hello severity level of hello event (Verbose, Informational, Warning, Error, or Critical).</span></span>
- <span data-ttu-id="12d1c-127">**Status för**: hello status för hello händelse, startas normalt, misslyckades eller lyckades.</span><span class="sxs-lookup"><span data-stu-id="12d1c-127">**Status**: hello status of hello event, typically Started, Failed, or Succeeded.</span></span>
- <span data-ttu-id="12d1c-128">**Händelse som initieras av**: kallas även hello ”anroparen”.</span><span class="sxs-lookup"><span data-stu-id="12d1c-128">**Event initiated by**: Also known as hello "caller."</span></span> <span data-ttu-id="12d1c-129">hello e-postadress eller Azure Active Directory identifierare för hello-användaren som utförde åtgärden hello.</span><span class="sxs-lookup"><span data-stu-id="12d1c-129">hello email address or Azure Active Directory identifier of hello user who performed hello operation.</span></span>

>[!NOTE]
><span data-ttu-id="12d1c-130">Du måste ange minst två av hello före kriterier i aviseringen, med en som hello kategori.</span><span class="sxs-lookup"><span data-stu-id="12d1c-130">You must specify at least two of hello preceding criteria in your alert, with one being hello category.</span></span> <span data-ttu-id="12d1c-131">Du kan inte skapa en avisering som aktiveras varje gång en händelse skapas i hello aktivitetsloggar.</span><span class="sxs-lookup"><span data-stu-id="12d1c-131">You may not create an alert that activates every time an event is created in hello activity logs.</span></span>
>
>

<span data-ttu-id="12d1c-132">När en aktivitet loggen avisering aktiveras den använder en åtgärd gruppera toogenerate åtgärder eller meddelanden.</span><span class="sxs-lookup"><span data-stu-id="12d1c-132">When an activity log alert is activated, it uses an action group toogenerate actions or notifications.</span></span> <span data-ttu-id="12d1c-133">En grupp är en återanvändbar uppsättning notification mottagare, till exempel e-postadresser, webhook URL: er eller SMS telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="12d1c-133">An action group is a reusable set of notification receivers, such as email addresses, webhook URLs, or SMS phone numbers.</span></span> <span data-ttu-id="12d1c-134">hello mottagare kan refereras från flera aviseringar toocentralize och gruppera dina aviseringskanaler.</span><span class="sxs-lookup"><span data-stu-id="12d1c-134">hello receivers can be referenced from multiple alerts toocentralize and group your notification channels.</span></span> <span data-ttu-id="12d1c-135">När du definierar aviseringen aktivitet loggen har du två alternativ.</span><span class="sxs-lookup"><span data-stu-id="12d1c-135">When you define your activity log alert, you have two options.</span></span> <span data-ttu-id="12d1c-136">Du kan:</span><span class="sxs-lookup"><span data-stu-id="12d1c-136">You can:</span></span>

* <span data-ttu-id="12d1c-137">Använd en befintlig grupp i din logg varning.</span><span class="sxs-lookup"><span data-stu-id="12d1c-137">Use an existing action group in your activity log alert.</span></span> 
* <span data-ttu-id="12d1c-138">Skapa en ny grupp.</span><span class="sxs-lookup"><span data-stu-id="12d1c-138">Create a new action group.</span></span> 

<span data-ttu-id="12d1c-139">toolearn mer om åtgärdsgrupper finns [skapa och hantera åtgärdsgrupper i hello Azure-portalen](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="12d1c-139">toolearn more about action groups, see [Create and manage action groups in hello Azure portal](monitoring-action-groups.md).</span></span>

<span data-ttu-id="12d1c-140">toolearn mer om hälsa tjänstmeddelanden, se [varningar aktivitet loggen på meddelanden om hälsostatus](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="12d1c-140">toolearn more about service health notifications, see [Receive activity log alerts on service health notifications](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>

## <a name="create-an-alert-on-an-activity-log-event-with-a-new-action-group-by-using-hello-azure-portal"></a><span data-ttu-id="12d1c-141">Skapa en avisering på en aktivitet logga en händelse med en ny grupp med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="12d1c-141">Create an alert on an activity log event with a new action group by using hello Azure portal</span></span>
1. <span data-ttu-id="12d1c-142">I hello [portal](https://portal.azure.com)väljer **övervakaren**.</span><span class="sxs-lookup"><span data-stu-id="12d1c-142">In hello [portal](https://portal.azure.com), select **Monitor**.</span></span>

    ![Hej ”övervakningstjänsten”](./media/monitoring-activity-log-alerts/home-monitor.png)
2. <span data-ttu-id="12d1c-144">I hello **aktivitetsloggen** väljer **aviseringar**.</span><span class="sxs-lookup"><span data-stu-id="12d1c-144">In hello **Activity log** section, select **Alerts**.</span></span>

    ![Hej ”” aviseringsfliken](./media/monitoring-activity-log-alerts/alerts-blades.png)
3. <span data-ttu-id="12d1c-146">Välj **Lägg till aktivitet loggen avisering**, och hello fält fylls i.</span><span class="sxs-lookup"><span data-stu-id="12d1c-146">Select **Add activity log alert**, and fill in hello fields.</span></span>

4. <span data-ttu-id="12d1c-147">Ange ett namn i hello **aktivitet avisering loggnamn** och välj en **beskrivning**.</span><span class="sxs-lookup"><span data-stu-id="12d1c-147">Enter a name in hello **Activity log alert name** box, and select a **Description**.</span></span>

    ![Hej ”Lägg till loggen varning” kommando](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

5. <span data-ttu-id="12d1c-149">Hej **prenumeration** rutan autofills med din aktuella prenumeration.</span><span class="sxs-lookup"><span data-stu-id="12d1c-149">hello **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="12d1c-150">Den här prenumerationen är hello något i vilken åtgärd hello-grupp har sparats.</span><span class="sxs-lookup"><span data-stu-id="12d1c-150">This subscription is hello one in which hello action group is saved.</span></span> <span data-ttu-id="12d1c-151">hello avisering resursen är distribuerade toothis prenumeration och Övervakare aktivitet logghändelser från den.</span><span class="sxs-lookup"><span data-stu-id="12d1c-151">hello alert resource is deployed toothis subscription and monitors activity log events from it.</span></span>

    ![Hej ”Lägg till loggen varning” dialogrutan](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

6. <span data-ttu-id="12d1c-153">Välj hello **resursgruppen** i vilka hello avisering resursen har skapats.</span><span class="sxs-lookup"><span data-stu-id="12d1c-153">Select hello **Resource group** in which hello alert resource is created.</span></span> <span data-ttu-id="12d1c-154">Detta är inte hello resursgruppen som övervakas av hello aviseringen.</span><span class="sxs-lookup"><span data-stu-id="12d1c-154">This is not hello resource group that's monitored by hello alert.</span></span> <span data-ttu-id="12d1c-155">Det är hello resursgruppen där hello avisering resursen finns.</span><span class="sxs-lookup"><span data-stu-id="12d1c-155">Instead, it's hello resource group where hello alert resource is located.</span></span>

7. <span data-ttu-id="12d1c-156">Du kan också välja en **händelsekategori** toomodify hello ytterligare filter som visas.</span><span class="sxs-lookup"><span data-stu-id="12d1c-156">Optionally, select an **Event category** toomodify hello additional filters that are shown.</span></span> <span data-ttu-id="12d1c-157">För administrativa händelser hello filtren är **resursgruppen**, **resurs**, **resurstypen**, **åtgärdsnamn**, **Nivå**, **Status**, och **händelse som initieras av**.</span><span class="sxs-lookup"><span data-stu-id="12d1c-157">For Administrative events, hello filters include **Resource group**, **Resource**, **Resource type**, **Operation name**, **Level**, **Status**, and **Event initiated by**.</span></span> <span data-ttu-id="12d1c-158">Dessa värden identifiera vilka händelser som bör du övervaka den här aviseringen.</span><span class="sxs-lookup"><span data-stu-id="12d1c-158">These values identify which events this alert should monitor.</span></span>

    >[!NOTE]
    ><span data-ttu-id="12d1c-159">Du måste ange minst en av hello före kriterier i aviseringen.</span><span class="sxs-lookup"><span data-stu-id="12d1c-159">You must specify at least one of hello preceding criteria in your alert.</span></span> <span data-ttu-id="12d1c-160">Du kan inte skapa en avisering som aktiveras varje gång en händelse skapas i hello aktivitetsloggar.</span><span class="sxs-lookup"><span data-stu-id="12d1c-160">You may not create an alert that activates every time an event is created in hello activity logs.</span></span>
    >
    >

8. <span data-ttu-id="12d1c-161">Ange ett namn i hello **åtgärd gruppnamn** och ange ett namn i hello **kortnamnet** rutan.</span><span class="sxs-lookup"><span data-stu-id="12d1c-161">Enter a name in hello **Action group name** box, and enter a name in hello **Short name** box.</span></span> <span data-ttu-id="12d1c-162">hello kort namn används i stället för en fullständig åtgärd gruppnamn när meddelanden som skickas med den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="12d1c-162">hello short name is used in place of a full action group name when notifications are sent using this group.</span></span>

9.  <span data-ttu-id="12d1c-163">Definiera en lista med åtgärder genom att tillhandahålla hello åtgärd:</span><span class="sxs-lookup"><span data-stu-id="12d1c-163">Define a list of actions by providing hello action’s:</span></span>

    <span data-ttu-id="12d1c-164">a.</span><span class="sxs-lookup"><span data-stu-id="12d1c-164">a.</span></span> <span data-ttu-id="12d1c-165">**Namnet**: Ange hello åtgärdens namn, alias eller identifierare.</span><span class="sxs-lookup"><span data-stu-id="12d1c-165">**Name**: Enter hello action’s name, alias, or identifier.</span></span>

    <span data-ttu-id="12d1c-166">b.</span><span class="sxs-lookup"><span data-stu-id="12d1c-166">b.</span></span> <span data-ttu-id="12d1c-167">**Åtgärdstyp**: Välj SMS, e-post eller webhooken.</span><span class="sxs-lookup"><span data-stu-id="12d1c-167">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="12d1c-168">c.</span><span class="sxs-lookup"><span data-stu-id="12d1c-168">c.</span></span> <span data-ttu-id="12d1c-169">**Information om**: baserat på hello åtgärdstyp, ange ett telefonnummer, e-postadress eller webhook URI.</span><span class="sxs-lookup"><span data-stu-id="12d1c-169">**Details**: Based on hello action type, enter a phone number, email address, or webhook URI.</span></span>

10. <span data-ttu-id="12d1c-170">Välj **OK** toocreate hello avisering.</span><span class="sxs-lookup"><span data-stu-id="12d1c-170">Select **OK** toocreate hello alert.</span></span>

<span data-ttu-id="12d1c-171">hello avisering tar några minuter toofully sprida och sedan aktiveras.</span><span class="sxs-lookup"><span data-stu-id="12d1c-171">hello alert takes a few minutes toofully propagate and then become active.</span></span> <span data-ttu-id="12d1c-172">Den startar när nya händelser hello avisering matchningsvillkor.</span><span class="sxs-lookup"><span data-stu-id="12d1c-172">It triggers when new events match hello alert's criteria.</span></span>

<span data-ttu-id="12d1c-173">Mer information finns i [förstå hello webhook schema som används i aktiviteten Logga varningar](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="12d1c-173">For more information, see [Understand hello webhook schema used in activity log alerts](monitoring-activity-log-alerts-webhook.md).</span></span>

>[!NOTE]
><span data-ttu-id="12d1c-174">hello åtgärdsgrupp som definierats i de här stegen är återanvändbara som en befintlig grupp för alla framtida definitioner för aviseringen.</span><span class="sxs-lookup"><span data-stu-id="12d1c-174">hello action group defined in these steps is reusable as an existing action group for all future alert definitions.</span></span>
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-hello-azure-portal"></a><span data-ttu-id="12d1c-175">Skapa en avisering på en aktivitet logga en händelse för en befintlig grupp med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="12d1c-175">Create an alert on an activity log event for an existing action group by using hello Azure portal</span></span>
1. <span data-ttu-id="12d1c-176">Följ steg 1 till 7 i hello föregående avsnitt toocreate aktivitet loggen aviseringen.</span><span class="sxs-lookup"><span data-stu-id="12d1c-176">Follow steps 1 through 7 in hello previous section toocreate your activity log alert.</span></span>

2. <span data-ttu-id="12d1c-177">Under **meddela**väljer hello **befintliga** åtgärdsknappen för gruppen.</span><span class="sxs-lookup"><span data-stu-id="12d1c-177">Under **Notify via**, select hello **Existing** action group button.</span></span> <span data-ttu-id="12d1c-178">Välj en befintlig grupp hello-listan.</span><span class="sxs-lookup"><span data-stu-id="12d1c-178">Select an existing action group from hello list.</span></span>

3. <span data-ttu-id="12d1c-179">Välj **OK** toocreate hello avisering.</span><span class="sxs-lookup"><span data-stu-id="12d1c-179">Select **OK** toocreate hello alert.</span></span>

<span data-ttu-id="12d1c-180">hello avisering tar några minuter toofully sprida och sedan aktiveras.</span><span class="sxs-lookup"><span data-stu-id="12d1c-180">hello alert takes a few minutes toofully propagate and then become active.</span></span> <span data-ttu-id="12d1c-181">Den startar när nya händelser hello avisering matchningsvillkor.</span><span class="sxs-lookup"><span data-stu-id="12d1c-181">It triggers when new events match hello alert's criteria.</span></span>

## <a name="manage-your-alerts"></a><span data-ttu-id="12d1c-182">Hantera aviseringar</span><span class="sxs-lookup"><span data-stu-id="12d1c-182">Manage your alerts</span></span>

<span data-ttu-id="12d1c-183">När du skapar en avisering är det synliga under hello aviseringar i hello övervakaren bladet.</span><span class="sxs-lookup"><span data-stu-id="12d1c-183">After you create an alert, it's visible in hello Alerts section of hello Monitor blade.</span></span> <span data-ttu-id="12d1c-184">Välj hello avisering du vill toomanage till:</span><span class="sxs-lookup"><span data-stu-id="12d1c-184">Select hello alert you want toomanage to:</span></span>

* <span data-ttu-id="12d1c-185">Redigera den.</span><span class="sxs-lookup"><span data-stu-id="12d1c-185">Edit it.</span></span>
* <span data-ttu-id="12d1c-186">Ta bort den.</span><span class="sxs-lookup"><span data-stu-id="12d1c-186">Delete it.</span></span>
* <span data-ttu-id="12d1c-187">Inaktivera eller aktivera den, om du vill stoppa tootemporarily eller återuppta tar emot meddelanden om hello avisering.</span><span class="sxs-lookup"><span data-stu-id="12d1c-187">Disable or enable it, if you want tootemporarily stop or resume receiving notifications for hello alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12d1c-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12d1c-188">Next steps</span></span>
- <span data-ttu-id="12d1c-189">Hämta en [översikt över aviseringar](monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="12d1c-189">Get an [overview of alerts](monitoring-overview-alerts.md).</span></span>
- <span data-ttu-id="12d1c-190">Lär dig mer om [meddelande hastighetsbegränsning](monitoring-alerts-rate-limiting.md).</span><span class="sxs-lookup"><span data-stu-id="12d1c-190">Learn about [notification rate limiting](monitoring-alerts-rate-limiting.md).</span></span>
- <span data-ttu-id="12d1c-191">Granska hello [avisering webhook för aktivitetslogg](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="12d1c-191">Review hello [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>
- <span data-ttu-id="12d1c-192">Lär dig mer om [åtgärdsgrupper](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="12d1c-192">Learn more about [action groups](monitoring-action-groups.md).</span></span>  
- <span data-ttu-id="12d1c-193">Lär dig mer om [tjänsten meddelanden om hälsostatus](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="12d1c-193">Learn about [service health notifications](monitoring-service-notifications.md).</span></span>
- <span data-ttu-id="12d1c-194">Skapa en [aktivitet logga avisering toomonitor alla Autoskala motorn åtgärder för din prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span><span class="sxs-lookup"><span data-stu-id="12d1c-194">Create an [activity log alert toomonitor all autoscale engine operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span></span>
- <span data-ttu-id="12d1c-195">Skapa en [aktivitet logga avisering toomonitor alla misslyckade Autoskala skala-/ skalbar åtgärder för din prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span><span class="sxs-lookup"><span data-stu-id="12d1c-195">Create an [activity log alert toomonitor all failed autoscale scale-in/scale-out operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span></span>
