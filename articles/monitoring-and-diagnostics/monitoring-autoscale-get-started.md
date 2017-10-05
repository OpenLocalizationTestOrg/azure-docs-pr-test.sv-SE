---
title: "Kom igång med Autoskala i Azure | Microsoft Docs"
description: "Lär dig mer om att skala din resurs i Azure."
author: rajram
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: rajram
ms.openlocfilehash: 68cb624b3ef4a77e7cfc949979e0b1949c2e5535
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-autoscale-in-azure"></a><span data-ttu-id="15cdd-103">Kom igång med Autoskala i Azure</span><span class="sxs-lookup"><span data-stu-id="15cdd-103">Get started with Autoscale in Azure</span></span>
<span data-ttu-id="15cdd-104">Den här artikeln beskriver hur du ställer in Autoskala inställningarna för din resurs i Microsoft Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="15cdd-104">This article describes how to set up your Autoscale settings for your resource in the Microsoft Azure portal.</span></span>

<span data-ttu-id="15cdd-105">Azure övervakaren Autoskala gäller bara för virtuella datorer, cloud services, Azure App Service-planer och apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="15cdd-105">Azure Monitor Autoscale applies only to virtual machine scale sets, cloud services, Azure App Service plans, and App Service environments.</span></span> 

## <a name="discover-the-autoscale-settings-in-your-subscription"></a><span data-ttu-id="15cdd-106">Identifiera Autoskala inställningarna i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="15cdd-106">Discover the Autoscale settings in your subscription</span></span>
<span data-ttu-id="15cdd-107">Du kan identifiera alla resurser som autoskalning kan tillämpas i Azure-Monitor.</span><span class="sxs-lookup"><span data-stu-id="15cdd-107">You can discover all the resources for which Autoscale is applicable in Azure Monitor.</span></span> <span data-ttu-id="15cdd-108">Använd följande steg för steg för steg:</span><span class="sxs-lookup"><span data-stu-id="15cdd-108">Use the following steps for a step-by-step walkthrough:</span></span>

1. <span data-ttu-id="15cdd-109">Öppna den [Azure-portalen.][1]</span><span class="sxs-lookup"><span data-stu-id="15cdd-109">Open the [Azure portal.][1]</span></span>
2. <span data-ttu-id="15cdd-110">Klicka på ikonen Azure i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="15cdd-110">Click the Azure Monitor icon in the left pane.</span></span>
  <span data-ttu-id="15cdd-111">![Öppna Azure Övervakare][2]</span><span class="sxs-lookup"><span data-stu-id="15cdd-111">![Open Azure Monitor][2]</span></span>
3. <span data-ttu-id="15cdd-112">Klicka på **Autoskala** att visa alla resurser som autoskalning kan tillämpas, tillsammans med deras aktuella Autoskala status.</span><span class="sxs-lookup"><span data-stu-id="15cdd-112">Click **Autoscale** to view all the resources for which Autoscale is applicable, along with their current Autoscale status.</span></span>
  <span data-ttu-id="15cdd-113">![Identifiera Autoskala i Azure-Monitor][3]</span><span class="sxs-lookup"><span data-stu-id="15cdd-113">![Discover Autoscale in Azure Monitor][3]</span></span>

<span data-ttu-id="15cdd-114">Du kan använda filterfönstret längst upp till scope nedåt i listan välja resurser i en viss resursgrupp, specifika resurstyper eller en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="15cdd-114">You can use the filter pane at the top to scope down the list to select resources in a specific resource group, specific resource types, or a specific resource.</span></span>

<span data-ttu-id="15cdd-115">Du kommer märka aktuella standardinstansantalet och Autoskala status för varje resurs.</span><span class="sxs-lookup"><span data-stu-id="15cdd-115">For each resource, you will find the current instance count and the Autoscale status.</span></span> <span data-ttu-id="15cdd-116">Autoskala status kan vara:</span><span class="sxs-lookup"><span data-stu-id="15cdd-116">The Autoscale status can be:</span></span>

- <span data-ttu-id="15cdd-117">**Inte konfigurerad**: du har inte aktiverat Autoskala ännu för den här resursen.</span><span class="sxs-lookup"><span data-stu-id="15cdd-117">**Not configured**: You have not enabled Autoscale yet for this resource.</span></span>
- <span data-ttu-id="15cdd-118">**Aktiverad**: du har aktiverat Autoskala för den här resursen.</span><span class="sxs-lookup"><span data-stu-id="15cdd-118">**Enabled**: You have enabled Autoscale for this resource.</span></span>
- <span data-ttu-id="15cdd-119">**Inaktiverad**: du har inaktiverat Autoskala för den här resursen.</span><span class="sxs-lookup"><span data-stu-id="15cdd-119">**Disabled**: You have disabled Autoscale for this resource.</span></span>

## <a name="create-your-first-autoscale-setting"></a><span data-ttu-id="15cdd-120">Skapa din första autoskalningsinställning</span><span class="sxs-lookup"><span data-stu-id="15cdd-120">Create your first Autoscale setting</span></span>

<span data-ttu-id="15cdd-121">Nu ska vi gå via en enkla steg för steg att skapa din första autoskalningsinställning.</span><span class="sxs-lookup"><span data-stu-id="15cdd-121">Let's now go through a simple step-by-step walkthrough to create your first Autoscale setting.</span></span>

1. <span data-ttu-id="15cdd-122">Öppna den **Autoskala** bladet i Azure-Monitor och välj en resurs som du vill skala.</span><span class="sxs-lookup"><span data-stu-id="15cdd-122">Open the **Autoscale** blade in Azure Monitor and select a resource that you want to scale.</span></span> <span data-ttu-id="15cdd-123">(Följande använda en App Service-plan som är associerade med ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="15cdd-123">(The following steps use an App Service plan associated with a web app.</span></span> <span data-ttu-id="15cdd-124">Du kan [skapa din första ASP.NET-webbapp i Azure på fem minuter.] [4])</span><span class="sxs-lookup"><span data-stu-id="15cdd-124">You can [create your first ASP.NET web app in Azure in 5 minutes.][4])</span></span>
2. <span data-ttu-id="15cdd-125">Observera att den aktuella standardinstansantalet är 1.</span><span class="sxs-lookup"><span data-stu-id="15cdd-125">Note that the current instance count is 1.</span></span> <span data-ttu-id="15cdd-126">Klicka på **aktivera Autoskala**.</span><span class="sxs-lookup"><span data-stu-id="15cdd-126">Click **Enable autoscale**.</span></span>
  <span data-ttu-id="15cdd-127">![Skalinställningen för ny webbapp][5]</span><span class="sxs-lookup"><span data-stu-id="15cdd-127">![Scale setting for new web app][5]</span></span>
3. <span data-ttu-id="15cdd-128">Ange ett namn för inställningen skala och klicka sedan på **lägga till en regel**.</span><span class="sxs-lookup"><span data-stu-id="15cdd-128">Provide a name for the scale setting, and then click **Add a rule**.</span></span> <span data-ttu-id="15cdd-129">Lägg märke till regelalternativ för skalan som öppnas som en kontext rutan till höger.</span><span class="sxs-lookup"><span data-stu-id="15cdd-129">Notice the scale rule options that open as a context pane on the right side.</span></span> <span data-ttu-id="15cdd-130">Som standard anger det här alternativet för att skala din instansantalet av 1 om CPU-procent av resursen överskrider 70 procent.</span><span class="sxs-lookup"><span data-stu-id="15cdd-130">By default, this sets the option to scale your instance count by 1 if the CPU percentage of the resource exceeds 70 percent.</span></span> <span data-ttu-id="15cdd-131">Lämna till sina standardvärden och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="15cdd-131">Leave it at its default values and click **Add**.</span></span>
  <span data-ttu-id="15cdd-132">![Skapa skalinställningen för en webbapp][6]</span><span class="sxs-lookup"><span data-stu-id="15cdd-132">![Create scale setting for a web app][6]</span></span>
4. <span data-ttu-id="15cdd-133">Nu har du skapat din första skalningsregeln.</span><span class="sxs-lookup"><span data-stu-id="15cdd-133">You've now created your first scale rule.</span></span> <span data-ttu-id="15cdd-134">Observera att UX rekommenderar metodtips och säger att ”bör ha minst en skala i regeln”.</span><span class="sxs-lookup"><span data-stu-id="15cdd-134">Note that the UX recommends best practices and states that "It is recommended to have at least one scale in rule."</span></span> <span data-ttu-id="15cdd-135">Gör så här:</span><span class="sxs-lookup"><span data-stu-id="15cdd-135">To do so:</span></span>
  
    <span data-ttu-id="15cdd-136">a.</span><span class="sxs-lookup"><span data-stu-id="15cdd-136">a.</span></span> <span data-ttu-id="15cdd-137">Klicka på **lägga till en regel**.</span><span class="sxs-lookup"><span data-stu-id="15cdd-137">Click **Add a rule**.</span></span> 

    <span data-ttu-id="15cdd-138">b.</span><span class="sxs-lookup"><span data-stu-id="15cdd-138">b.</span></span> <span data-ttu-id="15cdd-139">Ange **operatorn** till **mindre än**.</span><span class="sxs-lookup"><span data-stu-id="15cdd-139">Set **Operator** to **Less than**.</span></span>

    <span data-ttu-id="15cdd-140">c.</span><span class="sxs-lookup"><span data-stu-id="15cdd-140">c.</span></span> <span data-ttu-id="15cdd-141">Ange **tröskelvärdet** till **20**.</span><span class="sxs-lookup"><span data-stu-id="15cdd-141">Set **Threshold** to **20**.</span></span>

    <span data-ttu-id="15cdd-142">d.</span><span class="sxs-lookup"><span data-stu-id="15cdd-142">d.</span></span> <span data-ttu-id="15cdd-143">Ange **åtgärden** till **minska antalet av**.</span><span class="sxs-lookup"><span data-stu-id="15cdd-143">Set **Operation** to **Decrease count by**.</span></span>

   <span data-ttu-id="15cdd-144">Du bör nu ha en skalinställningen att skala ut/skalar i baserat på CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="15cdd-144">You should now have a scale setting that scales out/scales in based on CPU usage.</span></span>
   <span data-ttu-id="15cdd-145">![Skala baserat på CPU][8]</span><span class="sxs-lookup"><span data-stu-id="15cdd-145">![Scale based on CPU][8]</span></span>
5. <span data-ttu-id="15cdd-146">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="15cdd-146">Click **Save**.</span></span>

<span data-ttu-id="15cdd-147">Grattis!</span><span class="sxs-lookup"><span data-stu-id="15cdd-147">Congratulations!</span></span> <span data-ttu-id="15cdd-148">Du har nu skapat din första skalinställningen Autoskala ditt webbprogram baserat på CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="15cdd-148">You've now successfully created your first scale setting to autoscale your web app based on CPU usage.</span></span>

> [!NOTE] 
> <span data-ttu-id="15cdd-149">Samma steg kan användas för att komma igång med en skaluppsättning för virtuell dator eller moln rolltjänst.</span><span class="sxs-lookup"><span data-stu-id="15cdd-149">The same steps are applicable to get started with a virtual machine scale set or cloud service role.</span></span>

## <a name="other-considerations"></a><span data-ttu-id="15cdd-150">Andra överväganden</span><span class="sxs-lookup"><span data-stu-id="15cdd-150">Other considerations</span></span>
### <a name="scale-based-on-a-schedule"></a><span data-ttu-id="15cdd-151">Skala enligt ett schema</span><span class="sxs-lookup"><span data-stu-id="15cdd-151">Scale based on a schedule</span></span>
<span data-ttu-id="15cdd-152">Förutom att skala baserat på CPU, kan du ange nivå på olika sätt för särskilda dagar i veckan.</span><span class="sxs-lookup"><span data-stu-id="15cdd-152">In addition to scale based on CPU, you can set your scale differently for specific days of the week.</span></span>

1. <span data-ttu-id="15cdd-153">Klicka på **Lägg till ett villkor för skala**.</span><span class="sxs-lookup"><span data-stu-id="15cdd-153">Click **Add a scale condition**.</span></span>
2. <span data-ttu-id="15cdd-154">Skala-läge och regler är densamma som standard villkor.</span><span class="sxs-lookup"><span data-stu-id="15cdd-154">Setting the scale mode and the rules is the same as the default condition.</span></span>
3. <span data-ttu-id="15cdd-155">Välj **Upprepa särskilda dagar** för schemat.</span><span class="sxs-lookup"><span data-stu-id="15cdd-155">Select **Repeat specific days** for the schedule.</span></span>
4. <span data-ttu-id="15cdd-156">Välj dagarna och Starttid/Sluttid för när villkoret skala tillämpas.</span><span class="sxs-lookup"><span data-stu-id="15cdd-156">Select the days and the start/end time for when the scale condition should be applied.</span></span>

![Skala villkor baserat på schema][9]
### <a name="scale-differently-on-specific-dates"></a><span data-ttu-id="15cdd-158">Skala annorlunda på specifika datum</span><span class="sxs-lookup"><span data-stu-id="15cdd-158">Scale differently on specific dates</span></span>
<span data-ttu-id="15cdd-159">Förutom att skala baserat på CPU, kan du ange nivå på olika sätt för specifika datum.</span><span class="sxs-lookup"><span data-stu-id="15cdd-159">In addition to scale based on CPU, you can set your scale differently for specific dates.</span></span>

1. <span data-ttu-id="15cdd-160">Klicka på **Lägg till ett villkor för skala**.</span><span class="sxs-lookup"><span data-stu-id="15cdd-160">Click **Add a scale condition**.</span></span>
2. <span data-ttu-id="15cdd-161">Skala-läge och regler är densamma som standard villkor.</span><span class="sxs-lookup"><span data-stu-id="15cdd-161">Setting the scale mode and the rules is the same as the default condition.</span></span>
3. <span data-ttu-id="15cdd-162">Välj **ange start-/ slutdatum** för schemat.</span><span class="sxs-lookup"><span data-stu-id="15cdd-162">Select **Specify start/end dates** for the schedule.</span></span>
4. <span data-ttu-id="15cdd-163">Välj start-/ slutdatum och Starttid/Sluttid för när villkoret skala tillämpas.</span><span class="sxs-lookup"><span data-stu-id="15cdd-163">Select the start/end dates and the start/end time for when the scale condition should be applied.</span></span>

![Skala villkor baserat på datum][10]

### <a name="view-the-scale-history-of-your-resource"></a><span data-ttu-id="15cdd-165">Visa historiken skala för din resurs</span><span class="sxs-lookup"><span data-stu-id="15cdd-165">View the scale history of your resource</span></span>
<span data-ttu-id="15cdd-166">När din resurs skalas upp eller ned loggas en händelse i aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="15cdd-166">Whenever your resource is scaled up or down, an event is logged in the activity log.</span></span> <span data-ttu-id="15cdd-167">Du kan visa historiken skala för din resurs för de senaste 24 timmarna genom att växla till den **kör historik** fliken.</span><span class="sxs-lookup"><span data-stu-id="15cdd-167">You can view the scale history of your resource for the past 24 hours by switching to the **Run history** tab.</span></span>

![Kör historik][11]

<span data-ttu-id="15cdd-169">Om du vill visa historiken för fullständig skala (för upp till 90 dagar) Välj **Klicka här om du vill se mer information**.</span><span class="sxs-lookup"><span data-stu-id="15cdd-169">If you want to view the complete scale history (for up to 90 days), select **Click here to see more details**.</span></span> <span data-ttu-id="15cdd-170">Aktivitetsloggen öppnas med Autoskala markerat för din resurs och kategori.</span><span class="sxs-lookup"><span data-stu-id="15cdd-170">The activity log opens, with Autoscale pre-selected for your resource and category.</span></span>

### <a name="view-the-scale-definition-of-your-resource"></a><span data-ttu-id="15cdd-171">Visa skala definition av din resurs</span><span class="sxs-lookup"><span data-stu-id="15cdd-171">View the scale definition of your resource</span></span>
<span data-ttu-id="15cdd-172">Autoskala är en Azure Resource Manager-resurs.</span><span class="sxs-lookup"><span data-stu-id="15cdd-172">Autoscale is an Azure Resource Manager resource.</span></span> <span data-ttu-id="15cdd-173">Du kan visa skala-definition i JSON genom att växla till den **JSON** fliken.</span><span class="sxs-lookup"><span data-stu-id="15cdd-173">You can view the scale definition in JSON by switching to the **JSON** tab.</span></span>

![Skala definition][12]

<span data-ttu-id="15cdd-175">Du kan göra ändringar i JSON direkt, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="15cdd-175">You can make changes in JSON directly, if required.</span></span> <span data-ttu-id="15cdd-176">Dessa ändringar visas när du har sparat.</span><span class="sxs-lookup"><span data-stu-id="15cdd-176">These changes will be reflected after you save them.</span></span>

### <a name="disable-autoscale-and-manually-scale-your-instances"></a><span data-ttu-id="15cdd-177">Inaktivera Autoskala och skala dina instanser manuellt</span><span class="sxs-lookup"><span data-stu-id="15cdd-177">Disable Autoscale and manually scale your instances</span></span>
<span data-ttu-id="15cdd-178">Det kan finnas tillfällen när du vill inaktivera din aktuella skalinställningen och skala resursen manuellt.</span><span class="sxs-lookup"><span data-stu-id="15cdd-178">There might be times when you want to disable your current scale setting and manually scale your resource.</span></span>

<span data-ttu-id="15cdd-179">Klicka på den **inaktivera Autoskala** längst upp.</span><span class="sxs-lookup"><span data-stu-id="15cdd-179">Click the **Disable autoscale** button at the top.</span></span>
<span data-ttu-id="15cdd-180">![Inaktivera Autoskala][13]</span><span class="sxs-lookup"><span data-stu-id="15cdd-180">![Disable Autoscale][13]</span></span>

> [!NOTE] 
> <span data-ttu-id="15cdd-181">Det här alternativet inaktiverar konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="15cdd-181">This option disables your configuration.</span></span> <span data-ttu-id="15cdd-182">Men kan du få tillgång till den när du har aktiverat Autoskala igen.</span><span class="sxs-lookup"><span data-stu-id="15cdd-182">However, you can get back to it after you enable Autoscale again.</span></span> 

<span data-ttu-id="15cdd-183">Du kan nu ange antalet instanser som du vill skala till manuellt.</span><span class="sxs-lookup"><span data-stu-id="15cdd-183">You can now set the number of instances that you want to scale to manually.</span></span>

![Ange manuell skala][14]

<span data-ttu-id="15cdd-185">Du kan alltid gå tillbaka till Autoskala genom att klicka på **aktivera Autoskala** och sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="15cdd-185">You can always return to Autoscale by clicking **Enable autoscale** and then **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="15cdd-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="15cdd-186">Next steps</span></span>
- [<span data-ttu-id="15cdd-187">Skapa en aktivitet loggen aviseringen om du vill övervaka alla Autoskala motorn åtgärder för din prenumeration</span><span class="sxs-lookup"><span data-stu-id="15cdd-187">Create an Activity Log Alert to monitor all Autoscale engine operations on your subscription</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [<span data-ttu-id="15cdd-188">Skapa en aktivitet loggen aviseringen om du vill övervaka alla misslyckade Autoskala skala-/ skalbar åtgärder för din prenumeration</span><span class="sxs-lookup"><span data-stu-id="15cdd-188">Create an Activity Log Alert to monitor all failed Autoscale scale-in/scale-out operations on your subscription</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

<!--Reference-->
[1]:https://portal.azure.com
[2]: ./media/monitoring-autoscale-get-started/azure-monitor-launch.png
[3]: ./media/monitoring-autoscale-get-started/discover-autoscale-azure-monitor.png
[4]: https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-get-started-dotnet
[5]: ./media/monitoring-autoscale-get-started/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-get-started/create-scale-setting-web-app.png
[7]: ./media/monitoring-autoscale-get-started/scale-in-recommendation.png
[8]: ./media/monitoring-autoscale-get-started/scale-based-on-cpu.png
[9]: ./media/monitoring-autoscale-get-started/scale-condition-schedule.png
[10]: ./media/monitoring-autoscale-get-started/scale-condition-dates.png
[11]: ./media/monitoring-autoscale-get-started/scale-history.png
[12]: ./media/monitoring-autoscale-get-started/scale-definition-json.png
[13]: ./media/monitoring-autoscale-get-started/disable-autoscale.png
[14]: ./media/monitoring-autoscale-get-started/set-manualscale.png

