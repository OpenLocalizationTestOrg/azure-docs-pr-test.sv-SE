---
title: "aaaGet igång med Autoskala i Azure | Microsoft Docs"
description: "Lär dig hur tooscale resurs i Azure."
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
ms.openlocfilehash: 6b3c3f4529018dcaf9691c538fec63dfbb3cea06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-autoscale-in-azure"></a><span data-ttu-id="bbabf-103">Kom igång med Autoskala i Azure</span><span class="sxs-lookup"><span data-stu-id="bbabf-103">Get started with Autoscale in Azure</span></span>
<span data-ttu-id="bbabf-104">Den här artikeln beskriver hur tooset in Autoskala inställningarna för din resurs i hello Microsoft Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bbabf-104">This article describes how tooset up your Autoscale settings for your resource in hello Microsoft Azure portal.</span></span>

<span data-ttu-id="bbabf-105">Azure övervakaren Autoskala gäller endast toovirtual machine-skalningsuppsättningar, cloud services, Azure App Service-planer och apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="bbabf-105">Azure Monitor Autoscale applies only toovirtual machine scale sets, cloud services, Azure App Service plans, and App Service environments.</span></span> 

## <a name="discover-hello-autoscale-settings-in-your-subscription"></a><span data-ttu-id="bbabf-106">Identifiera hello Autoskala inställningarna i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="bbabf-106">Discover hello Autoscale settings in your subscription</span></span>
<span data-ttu-id="bbabf-107">Du kan identifiera alla hello resurser som autoskalning kan tillämpas i Azure-Monitor.</span><span class="sxs-lookup"><span data-stu-id="bbabf-107">You can discover all hello resources for which Autoscale is applicable in Azure Monitor.</span></span> <span data-ttu-id="bbabf-108">Använd följande steg för en stegvis genomgång hello:</span><span class="sxs-lookup"><span data-stu-id="bbabf-108">Use hello following steps for a step-by-step walkthrough:</span></span>

1. <span data-ttu-id="bbabf-109">Öppna hello [Azure-portalen.][1]</span><span class="sxs-lookup"><span data-stu-id="bbabf-109">Open hello [Azure portal.][1]</span></span>
2. <span data-ttu-id="bbabf-110">Klicka på hello Azure ikonen i hello till vänster.</span><span class="sxs-lookup"><span data-stu-id="bbabf-110">Click hello Azure Monitor icon in hello left pane.</span></span>
  <span data-ttu-id="bbabf-111">![Öppna Azure Övervakare][2]</span><span class="sxs-lookup"><span data-stu-id="bbabf-111">![Open Azure Monitor][2]</span></span>
3. <span data-ttu-id="bbabf-112">Klicka på **Autoskala** tooview alla hello resurser för vilka Autoskala kan användas tillsammans med deras aktuella Autoskala status.</span><span class="sxs-lookup"><span data-stu-id="bbabf-112">Click **Autoscale** tooview all hello resources for which Autoscale is applicable, along with their current Autoscale status.</span></span>
  <span data-ttu-id="bbabf-113">![Identifiera Autoskala i Azure-Monitor][3]</span><span class="sxs-lookup"><span data-stu-id="bbabf-113">![Discover Autoscale in Azure Monitor][3]</span></span>

<span data-ttu-id="bbabf-114">Du kan använda hello filterfönstret hello översta tooscope ned hello listan tooselect resurser i en viss resursgrupp, specifika resurstyper eller en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="bbabf-114">You can use hello filter pane at hello top tooscope down hello list tooselect resources in a specific resource group, specific resource types, or a specific resource.</span></span>

<span data-ttu-id="bbabf-115">Du kommer märka hello aktuella standardinstansantalet och hello Autoskala status för varje resurs.</span><span class="sxs-lookup"><span data-stu-id="bbabf-115">For each resource, you will find hello current instance count and hello Autoscale status.</span></span> <span data-ttu-id="bbabf-116">hello Autoskala status kan vara:</span><span class="sxs-lookup"><span data-stu-id="bbabf-116">hello Autoscale status can be:</span></span>

- <span data-ttu-id="bbabf-117">**Inte konfigurerad**: du har inte aktiverat Autoskala ännu för den här resursen.</span><span class="sxs-lookup"><span data-stu-id="bbabf-117">**Not configured**: You have not enabled Autoscale yet for this resource.</span></span>
- <span data-ttu-id="bbabf-118">**Aktiverad**: du har aktiverat Autoskala för den här resursen.</span><span class="sxs-lookup"><span data-stu-id="bbabf-118">**Enabled**: You have enabled Autoscale for this resource.</span></span>
- <span data-ttu-id="bbabf-119">**Inaktiverad**: du har inaktiverat Autoskala för den här resursen.</span><span class="sxs-lookup"><span data-stu-id="bbabf-119">**Disabled**: You have disabled Autoscale for this resource.</span></span>

## <a name="create-your-first-autoscale-setting"></a><span data-ttu-id="bbabf-120">Skapa din första autoskalningsinställning</span><span class="sxs-lookup"><span data-stu-id="bbabf-120">Create your first Autoscale setting</span></span>

<span data-ttu-id="bbabf-121">Låt oss nu gå igenom en enkla steg för steg-toocreate din första autoskalningsinställning.</span><span class="sxs-lookup"><span data-stu-id="bbabf-121">Let's now go through a simple step-by-step walkthrough toocreate your first Autoscale setting.</span></span>

1. <span data-ttu-id="bbabf-122">Öppna hello **Autoskala** bladet i Azure-Monitor och välj en resurs som du vill tooscale.</span><span class="sxs-lookup"><span data-stu-id="bbabf-122">Open hello **Autoscale** blade in Azure Monitor and select a resource that you want tooscale.</span></span> <span data-ttu-id="bbabf-123">(hello följande använda en App Service-plan som är associerade med ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="bbabf-123">(hello following steps use an App Service plan associated with a web app.</span></span> <span data-ttu-id="bbabf-124">Du kan [skapa din första ASP.NET-webbapp i Azure på fem minuter.] [4])</span><span class="sxs-lookup"><span data-stu-id="bbabf-124">You can [create your first ASP.NET web app in Azure in 5 minutes.][4])</span></span>
2. <span data-ttu-id="bbabf-125">Observera att hello aktuella standardinstansantalet är 1.</span><span class="sxs-lookup"><span data-stu-id="bbabf-125">Note that hello current instance count is 1.</span></span> <span data-ttu-id="bbabf-126">Klicka på **aktivera Autoskala**.</span><span class="sxs-lookup"><span data-stu-id="bbabf-126">Click **Enable autoscale**.</span></span>
  <span data-ttu-id="bbabf-127">![Skalinställningen för ny webbapp][5]</span><span class="sxs-lookup"><span data-stu-id="bbabf-127">![Scale setting for new web app][5]</span></span>
3. <span data-ttu-id="bbabf-128">Ange ett namn för hello skala inställningen och klicka sedan på **lägga till en regel**.</span><span class="sxs-lookup"><span data-stu-id="bbabf-128">Provide a name for hello scale setting, and then click **Add a rule**.</span></span> <span data-ttu-id="bbabf-129">Observera hello regeln skalningsalternativ som öppnas som en kontext fönstret hello höger.</span><span class="sxs-lookup"><span data-stu-id="bbabf-129">Notice hello scale rule options that open as a context pane on hello right side.</span></span> <span data-ttu-id="bbabf-130">Som standard anger hello alternativet tooscale din instans räkna med 1 om hello CPU-procent av hello resurs överskrider 70 procent.</span><span class="sxs-lookup"><span data-stu-id="bbabf-130">By default, this sets hello option tooscale your instance count by 1 if hello CPU percentage of hello resource exceeds 70 percent.</span></span> <span data-ttu-id="bbabf-131">Lämna till sina standardvärden och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="bbabf-131">Leave it at its default values and click **Add**.</span></span>
  <span data-ttu-id="bbabf-132">![Skapa skalinställningen för en webbapp][6]</span><span class="sxs-lookup"><span data-stu-id="bbabf-132">![Create scale setting for a web app][6]</span></span>
4. <span data-ttu-id="bbabf-133">Nu har du skapat din första skalningsregeln.</span><span class="sxs-lookup"><span data-stu-id="bbabf-133">You've now created your first scale rule.</span></span> <span data-ttu-id="bbabf-134">Observera att hello UX rekommenderar bästa praxis och att ”bör toohave minst en skalan i regeln”.</span><span class="sxs-lookup"><span data-stu-id="bbabf-134">Note that hello UX recommends best practices and states that "It is recommended toohave at least one scale in rule."</span></span> <span data-ttu-id="bbabf-135">toodo så:</span><span class="sxs-lookup"><span data-stu-id="bbabf-135">toodo so:</span></span>
  
    <span data-ttu-id="bbabf-136">a.</span><span class="sxs-lookup"><span data-stu-id="bbabf-136">a.</span></span> <span data-ttu-id="bbabf-137">Klicka på **lägga till en regel**.</span><span class="sxs-lookup"><span data-stu-id="bbabf-137">Click **Add a rule**.</span></span> 

    <span data-ttu-id="bbabf-138">b.</span><span class="sxs-lookup"><span data-stu-id="bbabf-138">b.</span></span> <span data-ttu-id="bbabf-139">Ange **operatorn** för**mindre än**.</span><span class="sxs-lookup"><span data-stu-id="bbabf-139">Set **Operator** too**Less than**.</span></span>

    <span data-ttu-id="bbabf-140">c.</span><span class="sxs-lookup"><span data-stu-id="bbabf-140">c.</span></span> <span data-ttu-id="bbabf-141">Ange **tröskelvärdet** för**20**.</span><span class="sxs-lookup"><span data-stu-id="bbabf-141">Set **Threshold** too**20**.</span></span>

    <span data-ttu-id="bbabf-142">d.</span><span class="sxs-lookup"><span data-stu-id="bbabf-142">d.</span></span> <span data-ttu-id="bbabf-143">Ange **åtgärden** för**minska antalet av**.</span><span class="sxs-lookup"><span data-stu-id="bbabf-143">Set **Operation** too**Decrease count by**.</span></span>

   <span data-ttu-id="bbabf-144">Du bör nu ha en skalinställningen att skala ut/skalar i baserat på CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="bbabf-144">You should now have a scale setting that scales out/scales in based on CPU usage.</span></span>
   <span data-ttu-id="bbabf-145">![Skala baserat på CPU][8]</span><span class="sxs-lookup"><span data-stu-id="bbabf-145">![Scale based on CPU][8]</span></span>
5. <span data-ttu-id="bbabf-146">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="bbabf-146">Click **Save**.</span></span>

<span data-ttu-id="bbabf-147">Grattis!</span><span class="sxs-lookup"><span data-stu-id="bbabf-147">Congratulations!</span></span> <span data-ttu-id="bbabf-148">Du har nu skapat din första skala inställningen tooautoscale ditt webbprogram baserat på CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="bbabf-148">You've now successfully created your first scale setting tooautoscale your web app based on CPU usage.</span></span>

> [!NOTE] 
> <span data-ttu-id="bbabf-149">hello likadant är tillämpliga tooget igång med en virtuell dator skala rolltjänst för uppsättningen eller molnet.</span><span class="sxs-lookup"><span data-stu-id="bbabf-149">hello same steps are applicable tooget started with a virtual machine scale set or cloud service role.</span></span>

## <a name="other-considerations"></a><span data-ttu-id="bbabf-150">Andra överväganden</span><span class="sxs-lookup"><span data-stu-id="bbabf-150">Other considerations</span></span>
### <a name="scale-based-on-a-schedule"></a><span data-ttu-id="bbabf-151">Skala enligt ett schema</span><span class="sxs-lookup"><span data-stu-id="bbabf-151">Scale based on a schedule</span></span>
<span data-ttu-id="bbabf-152">Dessutom tooscale baserat på CPU, du kan ange nivå på olika sätt för särskilda dagar i veckan hello.</span><span class="sxs-lookup"><span data-stu-id="bbabf-152">In addition tooscale based on CPU, you can set your scale differently for specific days of hello week.</span></span>

1. <span data-ttu-id="bbabf-153">Klicka på **Lägg till ett villkor för skala**.</span><span class="sxs-lookup"><span data-stu-id="bbabf-153">Click **Add a scale condition**.</span></span>
2. <span data-ttu-id="bbabf-154">Ange hello skala läge och hello regler är samma hello som hello standardtillståndet.</span><span class="sxs-lookup"><span data-stu-id="bbabf-154">Setting hello scale mode and hello rules is hello same as hello default condition.</span></span>
3. <span data-ttu-id="bbabf-155">Välj **Upprepa särskilda dagar** för hello schema.</span><span class="sxs-lookup"><span data-stu-id="bbabf-155">Select **Repeat specific days** for hello schedule.</span></span>
4. <span data-ttu-id="bbabf-156">Välj hello dagar och hello Starttid/Sluttid för när hello skala villkor ska tillämpas.</span><span class="sxs-lookup"><span data-stu-id="bbabf-156">Select hello days and hello start/end time for when hello scale condition should be applied.</span></span>

![Skala villkor baserat på schema][9]
### <a name="scale-differently-on-specific-dates"></a><span data-ttu-id="bbabf-158">Skala annorlunda på specifika datum</span><span class="sxs-lookup"><span data-stu-id="bbabf-158">Scale differently on specific dates</span></span>
<span data-ttu-id="bbabf-159">Dessutom tooscale baserat på CPU, du kan ange nivå på olika sätt för specifika datum.</span><span class="sxs-lookup"><span data-stu-id="bbabf-159">In addition tooscale based on CPU, you can set your scale differently for specific dates.</span></span>

1. <span data-ttu-id="bbabf-160">Klicka på **Lägg till ett villkor för skala**.</span><span class="sxs-lookup"><span data-stu-id="bbabf-160">Click **Add a scale condition**.</span></span>
2. <span data-ttu-id="bbabf-161">Ange hello skala läge och hello regler är samma hello som hello standardtillståndet.</span><span class="sxs-lookup"><span data-stu-id="bbabf-161">Setting hello scale mode and hello rules is hello same as hello default condition.</span></span>
3. <span data-ttu-id="bbabf-162">Välj **ange start-/ slutdatum** för hello schema.</span><span class="sxs-lookup"><span data-stu-id="bbabf-162">Select **Specify start/end dates** for hello schedule.</span></span>
4. <span data-ttu-id="bbabf-163">Välj hello start/slutdatum och hello Starttid/Sluttid för när hello skala villkor ska tillämpas.</span><span class="sxs-lookup"><span data-stu-id="bbabf-163">Select hello start/end dates and hello start/end time for when hello scale condition should be applied.</span></span>

![Skala villkor baserat på datum][10]

### <a name="view-hello-scale-history-of-your-resource"></a><span data-ttu-id="bbabf-165">Visa historiken för hello skala för din resurs</span><span class="sxs-lookup"><span data-stu-id="bbabf-165">View hello scale history of your resource</span></span>
<span data-ttu-id="bbabf-166">När din resurs skalas upp eller ned loggas en händelse i aktivitetsloggen för hello.</span><span class="sxs-lookup"><span data-stu-id="bbabf-166">Whenever your resource is scaled up or down, an event is logged in hello activity log.</span></span> <span data-ttu-id="bbabf-167">Du kan visa hello skala historiken för din resurs för hello senaste 24 timmarna genom att växla toohello **kör historik** fliken.</span><span class="sxs-lookup"><span data-stu-id="bbabf-167">You can view hello scale history of your resource for hello past 24 hours by switching toohello **Run history** tab.</span></span>

![Kör historik][11]

<span data-ttu-id="bbabf-169">Om du vill tooview hello fullständig skala historik (för upp too90 dagar), Välj **Klicka här toosee mer**.</span><span class="sxs-lookup"><span data-stu-id="bbabf-169">If you want tooview hello complete scale history (for up too90 days), select **Click here toosee more details**.</span></span> <span data-ttu-id="bbabf-170">hello aktivitetsloggen öppnas med Autoskala markerat för din resurs och kategori.</span><span class="sxs-lookup"><span data-stu-id="bbabf-170">hello activity log opens, with Autoscale pre-selected for your resource and category.</span></span>

### <a name="view-hello-scale-definition-of-your-resource"></a><span data-ttu-id="bbabf-171">Visa hello skala definition av din resurs</span><span class="sxs-lookup"><span data-stu-id="bbabf-171">View hello scale definition of your resource</span></span>
<span data-ttu-id="bbabf-172">Autoskala är en Azure Resource Manager-resurs.</span><span class="sxs-lookup"><span data-stu-id="bbabf-172">Autoscale is an Azure Resource Manager resource.</span></span> <span data-ttu-id="bbabf-173">Du kan visa hello skala definition i JSON genom att växla toohello **JSON** fliken.</span><span class="sxs-lookup"><span data-stu-id="bbabf-173">You can view hello scale definition in JSON by switching toohello **JSON** tab.</span></span>

![Skala definition][12]

<span data-ttu-id="bbabf-175">Du kan göra ändringar i JSON direkt, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="bbabf-175">You can make changes in JSON directly, if required.</span></span> <span data-ttu-id="bbabf-176">Dessa ändringar visas när du har sparat.</span><span class="sxs-lookup"><span data-stu-id="bbabf-176">These changes will be reflected after you save them.</span></span>

### <a name="disable-autoscale-and-manually-scale-your-instances"></a><span data-ttu-id="bbabf-177">Inaktivera Autoskala och skala dina instanser manuellt</span><span class="sxs-lookup"><span data-stu-id="bbabf-177">Disable Autoscale and manually scale your instances</span></span>
<span data-ttu-id="bbabf-178">Det kan finnas tillfällen när du vill toodisable din aktuella skalinställningen och skala resursen manuellt.</span><span class="sxs-lookup"><span data-stu-id="bbabf-178">There might be times when you want toodisable your current scale setting and manually scale your resource.</span></span>

<span data-ttu-id="bbabf-179">Klicka på hello **inaktivera Autoskala** knappen hello överst.</span><span class="sxs-lookup"><span data-stu-id="bbabf-179">Click hello **Disable autoscale** button at hello top.</span></span>
<span data-ttu-id="bbabf-180">![Inaktivera Autoskala][13]</span><span class="sxs-lookup"><span data-stu-id="bbabf-180">![Disable Autoscale][13]</span></span>

> [!NOTE] 
> <span data-ttu-id="bbabf-181">Det här alternativet inaktiverar konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="bbabf-181">This option disables your configuration.</span></span> <span data-ttu-id="bbabf-182">Men kan du hämta tillbaka tooit när du har aktiverat Autoskala igen.</span><span class="sxs-lookup"><span data-stu-id="bbabf-182">However, you can get back tooit after you enable Autoscale again.</span></span> 

<span data-ttu-id="bbabf-183">Du kan nu ange hello antalet instanser som du vill tooscale toomanually.</span><span class="sxs-lookup"><span data-stu-id="bbabf-183">You can now set hello number of instances that you want tooscale toomanually.</span></span>

![Ange manuell skala][14]

<span data-ttu-id="bbabf-185">Du kan alltid gå tillbaka tooAutoscale genom att klicka på **aktivera Autoskala** och sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="bbabf-185">You can always return tooAutoscale by clicking **Enable autoscale** and then **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bbabf-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bbabf-186">Next steps</span></span>
- [<span data-ttu-id="bbabf-187">Skapa en aktivitet loggen avisering toomonitor alla Autoskala motorn åtgärder för din prenumeration</span><span class="sxs-lookup"><span data-stu-id="bbabf-187">Create an Activity Log Alert toomonitor all Autoscale engine operations on your subscription</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [<span data-ttu-id="bbabf-188">Skapa en aktivitet loggen avisering toomonitor alla misslyckade Autoskala skala-/ skalbar åtgärder för din prenumeration</span><span class="sxs-lookup"><span data-stu-id="bbabf-188">Create an Activity Log Alert toomonitor all failed Autoscale scale-in/scale-out operations on your subscription</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

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

