---
title: "Automatisk skala en tjänst i molnet i den klassiska portalen | Microsoft Docs"
description: "(klassisk) Lär dig hur du använder den klassiska portalen för att konfigurera regler för Automatisk skala för en cloud service-webbroll eller worker-rollen i Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: eb167d70-4eba-42a4-b157-d8d0688abf4b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 90d55bbac6e113d6add848ace67cf0749e26342b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-auto-scaling-for-a-cloud-service-in-the-classic-portal"></a><span data-ttu-id="ec6b3-103">Så här konfigurerar du automatisk skalning för en molntjänst i den klassiska portalen</span><span class="sxs-lookup"><span data-stu-id="ec6b3-103">How to configure auto scaling for a Cloud Service in the classic portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ec6b3-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ec6b3-104">Azure portal</span></span>](cloud-services-how-to-scale-portal.md)
> * [<span data-ttu-id="ec6b3-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="ec6b3-105">Azure classic portal</span></span>](cloud-services-how-to-scale.md)

<span data-ttu-id="ec6b3-106">Du kan konfigurera automatiska inställningar för din webbroll eller worker-rollen på sidan skala i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-106">On the Scale page of the Azure classic portal, you can configure automatic scale settings for your web role or worker role.</span></span> <span data-ttu-id="ec6b3-107">Du kan också konfigurera manuell skalning i stället för regelbaserade automatisk skalning.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-107">Alternatively, you can configure manual scaling instead of rules-based automatic scaling.</span></span>

> [!NOTE]
> <span data-ttu-id="ec6b3-108">Den här artikeln fokuserar på Molntjänsten webb- och arbetsroller roller.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-108">This article focuses on Cloud Service web and worker roles.</span></span> <span data-ttu-id="ec6b3-109">När du skapar en virtuell dator (klassisk) direkt finns det i en molntjänst.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-109">When you create a virtual machine (classic) directly, it is hosted in a cloud service.</span></span> <span data-ttu-id="ec6b3-110">En del av den här informationen gäller för dessa typer av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-110">Some of this information applies to these types of virtual machines.</span></span> <span data-ttu-id="ec6b3-111">Skala en tillgänglighetsuppsättning för virtuella datorer stängs bara dem och inaktivera skala regler som du konfigurerar.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-111">Scaling an availability set of virtual machines is just shutting them on and off based on the scale rules you configure.</span></span> <span data-ttu-id="ec6b3-112">Läs mer om virtuella datorer och tillgänglighetsuppsättningar [hantera tillgängligheten för virtuella datorer](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="ec6b3-112">For more information about Virtual Machines and availability sets, see [Manage the Availability of Virtual Machines](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="ec6b3-113">Innan du konfigurerar skalning för ditt program bör du tänka på följande information:</span><span class="sxs-lookup"><span data-stu-id="ec6b3-113">You should consider the following information before you configure scaling for your application:</span></span>

* <span data-ttu-id="ec6b3-114">Skalning påverkas av kärnor användning.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-114">Scaling is affected by core usage.</span></span>

    <span data-ttu-id="ec6b3-115">Större rollinstanser använda flera kärnor.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-115">Larger role instances use more cores.</span></span> <span data-ttu-id="ec6b3-116">Du kan skala ett program bara inom ramen för kärnor för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-116">You can scale an application only within the limit of cores for your subscription.</span></span> <span data-ttu-id="ec6b3-117">Anta till exempel har en begränsning på 20 kärnor för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-117">For example, say your subscription has a limit of 20 cores.</span></span> <span data-ttu-id="ec6b3-118">Om du kör ett program med två medelstora molntjänster (totalt 4 kärnor) kan du bara skala upp andra distributioner av molntjänster i din prenumeration av återstående 16 kärnor.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-118">If you run an application with two medium-sized cloud services (a total of 4 cores), you can only scale up other cloud service deployments in your subscription by the remaining 16 cores.</span></span> <span data-ttu-id="ec6b3-119">Mer information om storlekar finns [Molntjänststorlekar](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="ec6b3-119">For more information about sizes, see [Cloud Service Sizes](cloud-services-sizes-specs.md).</span></span>

* <span data-ttu-id="ec6b3-120">Du måste skapa en kö och koppla den till en roll innan du kan skala en applikation baserat på ett tröskelvärde för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-120">You must create a queue and associate it with a role before you can scale an application based on a message threshold.</span></span> <span data-ttu-id="ec6b3-121">Mer information finns i [hur du använder tjänsten Queue Storage](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="ec6b3-121">For more information, see [How to use the Queue Storage Service](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span>

* <span data-ttu-id="ec6b3-122">Du kan skala resurser som är länkade till Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-122">You can scale resources that are linked to your cloud service.</span></span> <span data-ttu-id="ec6b3-123">Mer information om hur du länkar resurser finns [så här: länka en resurs till en molntjänst](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="ec6b3-123">For more information about linking resources, see [How to: Link a resource to a cloud service](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).</span></span>

* <span data-ttu-id="ec6b3-124">Om du vill aktivera hög tillgänglighet för ditt program bör du kontrollera att den har distribuerats med två eller flera rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-124">To enable high availability of your application, you should ensure that it is deployed with two or more role instances.</span></span> <span data-ttu-id="ec6b3-125">Mer information finns i [serviceavtal](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="ec6b3-125">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

## <a name="schedule-scaling"></a><span data-ttu-id="ec6b3-126">Schemalägga skalning</span><span class="sxs-lookup"><span data-stu-id="ec6b3-126">Schedule scaling</span></span>
<span data-ttu-id="ec6b3-127">Som standard följer inte alla roller för ett visst schema.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-127">By default, all roles do not follow a specific schedule.</span></span> <span data-ttu-id="ec6b3-128">Därför kan dessa inställningar ändras gäller för alla tider och alla dagar året.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-128">Therefore, any settings changed apply to all times and all days throughout the year.</span></span> <span data-ttu-id="ec6b3-129">Om du vill kan konfigurera du manuell eller automatisk skalning av något av följande lägen:</span><span class="sxs-lookup"><span data-stu-id="ec6b3-129">If you want, you can setup manual or automatic scaling for one of the following modes:</span></span>

* <span data-ttu-id="ec6b3-130">Veckodagar</span><span class="sxs-lookup"><span data-stu-id="ec6b3-130">Weekdays</span></span>
* <span data-ttu-id="ec6b3-131">Helger</span><span class="sxs-lookup"><span data-stu-id="ec6b3-131">Weekends</span></span>
* <span data-ttu-id="ec6b3-132">Vecka kvällar</span><span class="sxs-lookup"><span data-stu-id="ec6b3-132">Week nights</span></span>
* <span data-ttu-id="ec6b3-133">Vecka mornings</span><span class="sxs-lookup"><span data-stu-id="ec6b3-133">Week mornings</span></span>
* <span data-ttu-id="ec6b3-134">Specifika datum</span><span class="sxs-lookup"><span data-stu-id="ec6b3-134">Specific dates</span></span>
* <span data-ttu-id="ec6b3-135">Specifika datumintervall</span><span class="sxs-lookup"><span data-stu-id="ec6b3-135">Specific date ranges</span></span>

<span data-ttu-id="ec6b3-136">Schema-inställningen har konfigurerats i den [klassiska Azure-portalen](https://manage.windowsazure.com/) på den</span><span class="sxs-lookup"><span data-stu-id="ec6b3-136">The schedule setting is configured in the [Azure classic portal](https://manage.windowsazure.com/) on the</span></span>  
<span data-ttu-id="ec6b3-137">**Molntjänster** > **\[Molntjänsten\]** > **skala**  >   **\[Produktion eller mellanlagring\]**  sidan.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-137">**Cloud Services** > **\[Your cloud service\]** > **Scale** > **\[Production or Staging\]** page.</span></span>

<span data-ttu-id="ec6b3-138">Klicka på den **ställer in schema gånger** knappen för varje roll som du vill ändra.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-138">Click the **set up schedule times** button for each role you want to change.</span></span>

![Molntjänsten automatisk skalning enligt ett schema][scale_schedules]

## <a name="manual-scale"></a><span data-ttu-id="ec6b3-140">Manuell skala</span><span class="sxs-lookup"><span data-stu-id="ec6b3-140">Manual scale</span></span>
<span data-ttu-id="ec6b3-141">På den **skala** kan du kan manuellt öka eller minska antalet instanser som körs i en molntjänst.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-141">On the **Scale** page, you can manually increase or decrease the number of running instances in a cloud service.</span></span> <span data-ttu-id="ec6b3-142">Den här inställningen konfigureras för varje schema som du har skapat eller hela tiden om du inte har skapat ett schema.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-142">This setting is configured for each schedule you've created or to all time if you have not created a schedule.</span></span>

1. <span data-ttu-id="ec6b3-143">I den [klassiska Azure-portalen](https://manage.windowsazure.com/), klickar du på **molntjänster**, och klicka sedan på namnet på Molntjänsten att öppna instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-143">In the [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click the name of the cloud service to open the dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="ec6b3-144">Om du inte hittar din molntjänst kan du behöva ändra från **produktion** till **mellanlagring** eller vice versa.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-144">If you don't see your cloud service, you may need to change from **Production** to **Staging** or vice versa.</span></span>

2. <span data-ttu-id="ec6b3-145">Klicka på **skala**.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-145">Click **Scale**.</span></span>
3. <span data-ttu-id="ec6b3-146">Välj det schema som du vill ändra skalningsalternativ för.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-146">Select the schedule you want to change scaling options for.</span></span> <span data-ttu-id="ec6b3-147">*Inte schemalagda tider* är standardvärdet om du har inga scheman som definierats.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-147">*No scheduled times* is the default if you have no schedules defined.</span></span>
4. <span data-ttu-id="ec6b3-148">Hitta de **skala efter mått** avsnittet och väljer **NONE**.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-148">Find the **Scale by metric** section and select **NONE**.</span></span> <span data-ttu-id="ec6b3-149">Den här inställningen är standard för alla roller.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-149">This setting is the default for all roles.</span></span>
5. <span data-ttu-id="ec6b3-150">Varje roll i Molntjänsten har en skjutreglaget för att ändra antalet instanser som ska användas.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-150">Each role in the cloud service has a slider for changing the number of instances to use.</span></span>
   
    ![Skala manuellt en rolltjänst för molnet][manual_scale]
   
    <span data-ttu-id="ec6b3-152">Om du behöver fler instanser kan du behöva ändra den [molnet storleken för den virtuella datorn](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="ec6b3-152">If you need more instances, you may need to change the [cloud service virtual machine size](cloud-services-sizes-specs.md).</span></span>
6. <span data-ttu-id="ec6b3-153">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-153">Click **Save**.</span></span>  
   <span data-ttu-id="ec6b3-154">Rollinstanser läggs till eller tas bort baserat på dina val.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-154">Role instances are added or removed based on your selections.</span></span>

> [!TIP]
> <span data-ttu-id="ec6b3-155">När du ser ![][tip_icon] flytta musen till den och du kan få hjälp om en specifik inställning har.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-155">Whenever you see ![][tip_icon] move your mouse to it and you can get help about what a specific setting does.</span></span>

## <a name="automatic-scale---cpu"></a><span data-ttu-id="ec6b3-156">Automatisk skalning - processor</span><span class="sxs-lookup"><span data-stu-id="ec6b3-156">Automatic scale - CPU</span></span>
<span data-ttu-id="ec6b3-157">Det här läget skalas om den genomsnittliga procentandelen av CPU-användningen går över eller under angivna tröskelvärden.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-157">This mode scales if the average percentage of CPU usage goes above or below specified thresholds.</span></span> <span data-ttu-id="ec6b3-158">Rollinstanser skapas eller tas bort när detta sker.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-158">Role instances are created or deleted when this happens.</span></span>

1. <span data-ttu-id="ec6b3-159">I den [klassiska Azure-portalen](https://manage.windowsazure.com/), klickar du på **molntjänster**, och klicka sedan på namnet på Molntjänsten att öppna instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-159">In the [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click the name of the cloud service to open the dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="ec6b3-160">Om du inte hittar din molntjänst kan du behöva ändra från **produktion** till **mellanlagring** eller vice versa.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-160">If you don't see your cloud service, you may need to change from **Production** to **Staging** or vice versa.</span></span>

2. <span data-ttu-id="ec6b3-161">Klicka på **skala**.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-161">Click **Scale**.</span></span>
3. <span data-ttu-id="ec6b3-162">Välj det schema som du vill ändra skalningsalternativ för.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-162">Select the schedule you want to change scaling options for.</span></span> <span data-ttu-id="ec6b3-163">*Inte schemalagda tider* är standardvärdet om du har inga scheman som definierats.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-163">*No scheduled times* is the default if you have no schedules defined.</span></span>
4. <span data-ttu-id="ec6b3-164">Hitta de **skala efter mått** avsnittet och väljer **CPU**.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-164">Find the **Scale by metric** section and select **CPU**.</span></span>
5. <span data-ttu-id="ec6b3-165">Du kan nu konfigurera en lägsta och högsta antal instanser av roller, mål CPU-användning (för att utlösa en skala upp) och hur många instanser som du vill skala upp eller ned genom.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-165">Now you can configure a minimum and maximum range of roles instances, the target CPU usage (to trigger a scale up), and how many instances to scale up and down by.</span></span>

![Skala en rolltjänst i molnet genom att cpu-belastning][cpu_scale]

> [!TIP]
> <span data-ttu-id="ec6b3-167">När du ser ![][tip_icon] flytta musen till den och du kan få hjälp om en specifik inställning har.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-167">Whenever you see ![][tip_icon] move your mouse to it and you can get help about what a specific setting does.</span></span>

## <a name="automatic-scale---queue"></a><span data-ttu-id="ec6b3-168">Automatisk skalning - kön</span><span class="sxs-lookup"><span data-stu-id="ec6b3-168">Automatic scale - Queue</span></span>
<span data-ttu-id="ec6b3-169">Det här läget skalas automatiskt om antalet meddelanden i en kö går över eller under ett angivet tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-169">This mode automatically scales if the number of messages in a queue goes above or below a specified threshold.</span></span> <span data-ttu-id="ec6b3-170">Rollinstanser skapas eller tas bort när detta sker.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-170">Role instances are created or deleted when this happens.</span></span>

1. <span data-ttu-id="ec6b3-171">I den [klassiska Azure-portalen](https://manage.windowsazure.com/), klickar du på **molntjänster**, och klicka sedan på namnet på Molntjänsten att öppna instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-171">In the [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click the name of the cloud service to open the dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="ec6b3-172">Om du inte hittar din molntjänst kan du behöva ändra från **produktion** till **mellanlagring** eller vice versa.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-172">If you don't see your cloud service, you may need to change from **Production** to **Staging** or vice versa.</span></span>

2. <span data-ttu-id="ec6b3-173">Klicka på **skala**.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-173">Click **Scale**.</span></span>
3. <span data-ttu-id="ec6b3-174">Hitta de **skala efter mått** avsnittet och väljer **KÖN**.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-174">Find the **Scale by metric** section and select **QUEUE**.</span></span>
4. <span data-ttu-id="ec6b3-175">Du kan nu konfigurera en lägsta och högsta antal roller instanser, kö- och antal meddelanden i kö ska behandlas för varje instans och hur många instanser att skala upp eller ned genom.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-175">Now you can configure a minimum and maximum range of roles instances, the queue and number of queue messages to process for each instance, and how many instances to scale up and down by.</span></span>

![Skala en rolltjänst för molntjänster genom en meddelandekö][queue_scale]

> [!TIP]
> <span data-ttu-id="ec6b3-177">När du ser ![][tip_icon] flytta musen till den och du kan få hjälp om en specifik inställning har.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-177">Whenever you see ![][tip_icon] move your mouse to it and you can get help about what a specific setting does.</span></span>

## <a name="scale-linked-resources"></a><span data-ttu-id="ec6b3-178">Skala länkade resurserna</span><span class="sxs-lookup"><span data-stu-id="ec6b3-178">Scale linked resources</span></span>
<span data-ttu-id="ec6b3-179">Ofta när du skalar en roll, är det bra att skala databasen som programmet använder också.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-179">Often when you scale a role, it's beneficial to scale the database that the application is using also.</span></span> <span data-ttu-id="ec6b3-180">Om du länka databasen till Molntjänsten, kan du komma åt skalning inställningarna för den här resursen genom att klicka på länken.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-180">If you link the database to the cloud service, you can access the scaling settings for that resource by clicking the appropriate link.</span></span>

1. <span data-ttu-id="ec6b3-181">I den [klassiska Azure-portalen](https://manage.windowsazure.com/), klickar du på **molntjänster**, och klicka sedan på namnet på Molntjänsten att öppna instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-181">In the [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click the name of the cloud service to open the dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="ec6b3-182">Om du inte hittar din molntjänst kan du behöva ändra från **produktion** till **mellanlagring** eller vice versa.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-182">If you don't see your cloud service, you may need to change from **Production** to **Staging** or vice versa.</span></span>

2. <span data-ttu-id="ec6b3-183">Klicka på **skala**.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-183">Click **Scale**.</span></span>
3. <span data-ttu-id="ec6b3-184">Hitta de **länkade resurser** avsnittet och klicka på **hantera skala för den här databasen**.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-184">Find the **linked resources** section and click **Manage scale for this database**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="ec6b3-185">Om du inte ser en **länkade resurser** avsnittet du förmodligen inte har några länkade resurser.</span><span class="sxs-lookup"><span data-stu-id="ec6b3-185">If you don't see a **linked resources** section, you probably do not have any linked resources.</span></span>

![][linked_resource]

[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
