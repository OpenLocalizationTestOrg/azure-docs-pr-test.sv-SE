---
title: "aaaAuto skala en tjänst i molnet i hello klassiska portal | Microsoft Docs"
description: "(klassisk) Lär dig hur toouse hello klassiska portal tooconfigure automatiskt skala regler för en cloud service webbroll eller en worker-rollen i Azure."
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
ms.openlocfilehash: ddb5816d4d22192c6d2f51d7508e390779742078
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-auto-scaling-for-a-cloud-service-in-hello-classic-portal"></a><span data-ttu-id="393b8-103">Hur tooconfigure automatisk skalning för en tjänst i molnet i hello klassiska portalen</span><span class="sxs-lookup"><span data-stu-id="393b8-103">How tooconfigure auto scaling for a Cloud Service in hello classic portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="393b8-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="393b8-104">Azure portal</span></span>](cloud-services-how-to-scale-portal.md)
> * [<span data-ttu-id="393b8-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="393b8-105">Azure classic portal</span></span>](cloud-services-how-to-scale.md)

<span data-ttu-id="393b8-106">Du kan konfigurera automatiska inställningar för din webbroll eller arbetsrollen hello skala sida av hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="393b8-106">On hello Scale page of hello Azure classic portal, you can configure automatic scale settings for your web role or worker role.</span></span> <span data-ttu-id="393b8-107">Du kan också konfigurera manuell skalning i stället för regelbaserade automatisk skalning.</span><span class="sxs-lookup"><span data-stu-id="393b8-107">Alternatively, you can configure manual scaling instead of rules-based automatic scaling.</span></span>

> [!NOTE]
> <span data-ttu-id="393b8-108">Den här artikeln fokuserar på Molntjänsten webb- och arbetsroller roller.</span><span class="sxs-lookup"><span data-stu-id="393b8-108">This article focuses on Cloud Service web and worker roles.</span></span> <span data-ttu-id="393b8-109">När du skapar en virtuell dator (klassisk) direkt finns det i en molntjänst.</span><span class="sxs-lookup"><span data-stu-id="393b8-109">When you create a virtual machine (classic) directly, it is hosted in a cloud service.</span></span> <span data-ttu-id="393b8-110">En del av den här informationen gäller toothese typer av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="393b8-110">Some of this information applies toothese types of virtual machines.</span></span> <span data-ttu-id="393b8-111">Skala en tillgänglighetsuppsättning för virtuella datorer stängs bara dem och inaktivera baserat på hello skala regler som du konfigurerar.</span><span class="sxs-lookup"><span data-stu-id="393b8-111">Scaling an availability set of virtual machines is just shutting them on and off based on hello scale rules you configure.</span></span> <span data-ttu-id="393b8-112">Läs mer om virtuella datorer och tillgänglighetsuppsättningar [hantera hello tillgänglighet av virtuella datorer](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="393b8-112">For more information about Virtual Machines and availability sets, see [Manage hello Availability of Virtual Machines](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="393b8-113">Du bör hello följande information innan du konfigurerar skalning för programmet:</span><span class="sxs-lookup"><span data-stu-id="393b8-113">You should consider hello following information before you configure scaling for your application:</span></span>

* <span data-ttu-id="393b8-114">Skalning påverkas av kärnor användning.</span><span class="sxs-lookup"><span data-stu-id="393b8-114">Scaling is affected by core usage.</span></span>

    <span data-ttu-id="393b8-115">Större rollinstanser använda flera kärnor.</span><span class="sxs-lookup"><span data-stu-id="393b8-115">Larger role instances use more cores.</span></span> <span data-ttu-id="393b8-116">Du kan skala ett program bara inom hello kärnor för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="393b8-116">You can scale an application only within hello limit of cores for your subscription.</span></span> <span data-ttu-id="393b8-117">Anta till exempel har en begränsning på 20 kärnor för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="393b8-117">For example, say your subscription has a limit of 20 cores.</span></span> <span data-ttu-id="393b8-118">Om du kör ett program med två medelstora molntjänster (totalt 4 kärnor) kan du bara skala upp andra distributioner av molntjänster i din prenumeration av hello återstående 16 kärnor.</span><span class="sxs-lookup"><span data-stu-id="393b8-118">If you run an application with two medium-sized cloud services (a total of 4 cores), you can only scale up other cloud service deployments in your subscription by hello remaining 16 cores.</span></span> <span data-ttu-id="393b8-119">Mer information om storlekar finns [Molntjänststorlekar](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="393b8-119">For more information about sizes, see [Cloud Service Sizes](cloud-services-sizes-specs.md).</span></span>

* <span data-ttu-id="393b8-120">Du måste skapa en kö och koppla den till en roll innan du kan skala en applikation baserat på ett tröskelvärde för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="393b8-120">You must create a queue and associate it with a role before you can scale an application based on a message threshold.</span></span> <span data-ttu-id="393b8-121">Mer information finns i [hur toouse hello Queue Storage Service](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="393b8-121">For more information, see [How toouse hello Queue Storage Service](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span>

* <span data-ttu-id="393b8-122">Du kan skala resurser som är länkade tooyour Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="393b8-122">You can scale resources that are linked tooyour cloud service.</span></span> <span data-ttu-id="393b8-123">Mer information om hur du länkar resurser finns [så här: länka en molntjänst för resursen tooa](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="393b8-123">For more information about linking resources, see [How to: Link a resource tooa cloud service](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).</span></span>

* <span data-ttu-id="393b8-124">tooenable hög tillgänglighet för programmet, bör du kontrollera att den har distribuerats med två eller flera rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="393b8-124">tooenable high availability of your application, you should ensure that it is deployed with two or more role instances.</span></span> <span data-ttu-id="393b8-125">Mer information finns i [serviceavtal](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="393b8-125">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

## <a name="schedule-scaling"></a><span data-ttu-id="393b8-126">Schemalägga skalning</span><span class="sxs-lookup"><span data-stu-id="393b8-126">Schedule scaling</span></span>
<span data-ttu-id="393b8-127">Som standard följer inte alla roller för ett visst schema.</span><span class="sxs-lookup"><span data-stu-id="393b8-127">By default, all roles do not follow a specific schedule.</span></span> <span data-ttu-id="393b8-128">Därför gäller alla inställningar som ändrades tooall samt alla dagar över hello året.</span><span class="sxs-lookup"><span data-stu-id="393b8-128">Therefore, any settings changed apply tooall times and all days throughout hello year.</span></span> <span data-ttu-id="393b8-129">Om du vill kan konfigurera du manuell eller automatisk skalning av något av följande lägen hello:</span><span class="sxs-lookup"><span data-stu-id="393b8-129">If you want, you can setup manual or automatic scaling for one of hello following modes:</span></span>

* <span data-ttu-id="393b8-130">Veckodagar</span><span class="sxs-lookup"><span data-stu-id="393b8-130">Weekdays</span></span>
* <span data-ttu-id="393b8-131">Helger</span><span class="sxs-lookup"><span data-stu-id="393b8-131">Weekends</span></span>
* <span data-ttu-id="393b8-132">Vecka kvällar</span><span class="sxs-lookup"><span data-stu-id="393b8-132">Week nights</span></span>
* <span data-ttu-id="393b8-133">Vecka mornings</span><span class="sxs-lookup"><span data-stu-id="393b8-133">Week mornings</span></span>
* <span data-ttu-id="393b8-134">Specifika datum</span><span class="sxs-lookup"><span data-stu-id="393b8-134">Specific dates</span></span>
* <span data-ttu-id="393b8-135">Specifika datumintervall</span><span class="sxs-lookup"><span data-stu-id="393b8-135">Specific date ranges</span></span>

<span data-ttu-id="393b8-136">hello schema inställning konfigureras i hello [klassiska Azure-portalen](https://manage.windowsazure.com/) på hello</span><span class="sxs-lookup"><span data-stu-id="393b8-136">hello schedule setting is configured in hello [Azure classic portal](https://manage.windowsazure.com/) on hello</span></span>  
<span data-ttu-id="393b8-137">**Molntjänster** > **\[Molntjänsten\]** > **skala**  >   **\[Produktion eller mellanlagring\]**  sidan.</span><span class="sxs-lookup"><span data-stu-id="393b8-137">**Cloud Services** > **\[Your cloud service\]** > **Scale** > **\[Production or Staging\]** page.</span></span>

<span data-ttu-id="393b8-138">Klicka på hello **ställer in schema gånger** knappen för varje roll som du vill toochange.</span><span class="sxs-lookup"><span data-stu-id="393b8-138">Click hello **set up schedule times** button for each role you want toochange.</span></span>

![Molntjänsten automatisk skalning enligt ett schema][scale_schedules]

## <a name="manual-scale"></a><span data-ttu-id="393b8-140">Manuell skala</span><span class="sxs-lookup"><span data-stu-id="393b8-140">Manual scale</span></span>
<span data-ttu-id="393b8-141">På hello **skala** kan du manuellt öka eller minska hello antalet instanser som körs i en molntjänst.</span><span class="sxs-lookup"><span data-stu-id="393b8-141">On hello **Scale** page, you can manually increase or decrease hello number of running instances in a cloud service.</span></span> <span data-ttu-id="393b8-142">Den här inställningen har konfigurerats för varje schema som du har skapat eller tooall tid om du inte har skapat ett schema.</span><span class="sxs-lookup"><span data-stu-id="393b8-142">This setting is configured for each schedule you've created or tooall time if you have not created a schedule.</span></span>

1. <span data-ttu-id="393b8-143">I hello [klassiska Azure-portalen](https://manage.windowsazure.com/), klickar du på **molntjänster**, och klicka sedan på hello namnet på hello cloud service tooopen hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="393b8-143">In hello [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click hello name of hello cloud service tooopen hello dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="393b8-144">Om du inte hittar din molntjänst kan du behöva toochange från **produktion** för**mellanlagring** eller vice versa.</span><span class="sxs-lookup"><span data-stu-id="393b8-144">If you don't see your cloud service, you may need toochange from **Production** too**Staging** or vice versa.</span></span>

2. <span data-ttu-id="393b8-145">Klicka på **skala**.</span><span class="sxs-lookup"><span data-stu-id="393b8-145">Click **Scale**.</span></span>
3. <span data-ttu-id="393b8-146">Välj hello-schema som du vill toochange skalningsalternativ för.</span><span class="sxs-lookup"><span data-stu-id="393b8-146">Select hello schedule you want toochange scaling options for.</span></span> <span data-ttu-id="393b8-147">*Inte schemalagda tider* är hello standard om du har inga scheman som definierats.</span><span class="sxs-lookup"><span data-stu-id="393b8-147">*No scheduled times* is hello default if you have no schedules defined.</span></span>
4. <span data-ttu-id="393b8-148">Hitta hello **skala efter mått** avsnittet och väljer **NONE**.</span><span class="sxs-lookup"><span data-stu-id="393b8-148">Find hello **Scale by metric** section and select **NONE**.</span></span> <span data-ttu-id="393b8-149">Den här inställningen är hello standard för alla roller.</span><span class="sxs-lookup"><span data-stu-id="393b8-149">This setting is hello default for all roles.</span></span>
5. <span data-ttu-id="393b8-150">Varje roll i hello Molntjänsten har en skjutreglaget för att ändra hello antalet instanser toouse.</span><span class="sxs-lookup"><span data-stu-id="393b8-150">Each role in hello cloud service has a slider for changing hello number of instances toouse.</span></span>
   
    ![Skala manuellt en rolltjänst för molnet][manual_scale]
   
    <span data-ttu-id="393b8-152">Om du behöver fler instanser kan du behöva toochange hello [molnet storleken för den virtuella datorn](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="393b8-152">If you need more instances, you may need toochange hello [cloud service virtual machine size](cloud-services-sizes-specs.md).</span></span>
6. <span data-ttu-id="393b8-153">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="393b8-153">Click **Save**.</span></span>  
   <span data-ttu-id="393b8-154">Rollinstanser läggs till eller tas bort baserat på dina val.</span><span class="sxs-lookup"><span data-stu-id="393b8-154">Role instances are added or removed based on your selections.</span></span>

> [!TIP]
> <span data-ttu-id="393b8-155">När du ser ![][tip_icon] flytta musen-tooit och du kan få hjälp om en specifik inställning har.</span><span class="sxs-lookup"><span data-stu-id="393b8-155">Whenever you see ![][tip_icon] move your mouse tooit and you can get help about what a specific setting does.</span></span>

## <a name="automatic-scale---cpu"></a><span data-ttu-id="393b8-156">Automatisk skalning - processor</span><span class="sxs-lookup"><span data-stu-id="393b8-156">Automatic scale - CPU</span></span>
<span data-ttu-id="393b8-157">Det här läget skalas om hello genomsnittliga procentandelen av CPU-användningen går över eller under angivna tröskelvärden.</span><span class="sxs-lookup"><span data-stu-id="393b8-157">This mode scales if hello average percentage of CPU usage goes above or below specified thresholds.</span></span> <span data-ttu-id="393b8-158">Rollinstanser skapas eller tas bort när detta sker.</span><span class="sxs-lookup"><span data-stu-id="393b8-158">Role instances are created or deleted when this happens.</span></span>

1. <span data-ttu-id="393b8-159">I hello [klassiska Azure-portalen](https://manage.windowsazure.com/), klickar du på **molntjänster**, och klicka sedan på hello namnet på hello cloud service tooopen hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="393b8-159">In hello [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click hello name of hello cloud service tooopen hello dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="393b8-160">Om du inte hittar din molntjänst kan du behöva toochange från **produktion** för**mellanlagring** eller vice versa.</span><span class="sxs-lookup"><span data-stu-id="393b8-160">If you don't see your cloud service, you may need toochange from **Production** too**Staging** or vice versa.</span></span>

2. <span data-ttu-id="393b8-161">Klicka på **skala**.</span><span class="sxs-lookup"><span data-stu-id="393b8-161">Click **Scale**.</span></span>
3. <span data-ttu-id="393b8-162">Välj hello-schema som du vill toochange skalningsalternativ för.</span><span class="sxs-lookup"><span data-stu-id="393b8-162">Select hello schedule you want toochange scaling options for.</span></span> <span data-ttu-id="393b8-163">*Inte schemalagda tider* är hello standard om du har inga scheman som definierats.</span><span class="sxs-lookup"><span data-stu-id="393b8-163">*No scheduled times* is hello default if you have no schedules defined.</span></span>
4. <span data-ttu-id="393b8-164">Hitta hello **skala efter mått** avsnittet och väljer **CPU**.</span><span class="sxs-lookup"><span data-stu-id="393b8-164">Find hello **Scale by metric** section and select **CPU**.</span></span>
5. <span data-ttu-id="393b8-165">Du kan nu konfigurera en lägsta och högsta antal roller instanser hello mål CPU-användning (tootrigger en skalas upp) och hur många instanser tooscale uppåt och nedåt av.</span><span class="sxs-lookup"><span data-stu-id="393b8-165">Now you can configure a minimum and maximum range of roles instances, hello target CPU usage (tootrigger a scale up), and how many instances tooscale up and down by.</span></span>

![Skala en rolltjänst i molnet genom att cpu-belastning][cpu_scale]

> [!TIP]
> <span data-ttu-id="393b8-167">När du ser ![][tip_icon] flytta musen-tooit och du kan få hjälp om en specifik inställning har.</span><span class="sxs-lookup"><span data-stu-id="393b8-167">Whenever you see ![][tip_icon] move your mouse tooit and you can get help about what a specific setting does.</span></span>

## <a name="automatic-scale---queue"></a><span data-ttu-id="393b8-168">Automatisk skalning - kön</span><span class="sxs-lookup"><span data-stu-id="393b8-168">Automatic scale - Queue</span></span>
<span data-ttu-id="393b8-169">Det här läget skalas automatiskt om hello antal meddelanden i en kö går över eller under ett angivet tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="393b8-169">This mode automatically scales if hello number of messages in a queue goes above or below a specified threshold.</span></span> <span data-ttu-id="393b8-170">Rollinstanser skapas eller tas bort när detta sker.</span><span class="sxs-lookup"><span data-stu-id="393b8-170">Role instances are created or deleted when this happens.</span></span>

1. <span data-ttu-id="393b8-171">I hello [klassiska Azure-portalen](https://manage.windowsazure.com/), klickar du på **molntjänster**, och klicka sedan på hello namnet på hello cloud service tooopen hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="393b8-171">In hello [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click hello name of hello cloud service tooopen hello dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="393b8-172">Om du inte hittar din molntjänst kan du behöva toochange från **produktion** för**mellanlagring** eller vice versa.</span><span class="sxs-lookup"><span data-stu-id="393b8-172">If you don't see your cloud service, you may need toochange from **Production** too**Staging** or vice versa.</span></span>

2. <span data-ttu-id="393b8-173">Klicka på **skala**.</span><span class="sxs-lookup"><span data-stu-id="393b8-173">Click **Scale**.</span></span>
3. <span data-ttu-id="393b8-174">Hitta hello **skala efter mått** avsnittet och väljer **KÖN**.</span><span class="sxs-lookup"><span data-stu-id="393b8-174">Find hello **Scale by metric** section and select **QUEUE**.</span></span>
4. <span data-ttu-id="393b8-175">Du kan nu konfigurera en lägsta och högsta antal roller instanser hello kön och antalet kön meddelanden tooprocess för varje instans och hur många instanser tooscale uppåt och nedåt av.</span><span class="sxs-lookup"><span data-stu-id="393b8-175">Now you can configure a minimum and maximum range of roles instances, hello queue and number of queue messages tooprocess for each instance, and how many instances tooscale up and down by.</span></span>

![Skala en rolltjänst för molntjänster genom en meddelandekö][queue_scale]

> [!TIP]
> <span data-ttu-id="393b8-177">När du ser ![][tip_icon] flytta musen-tooit och du kan få hjälp om en specifik inställning har.</span><span class="sxs-lookup"><span data-stu-id="393b8-177">Whenever you see ![][tip_icon] move your mouse tooit and you can get help about what a specific setting does.</span></span>

## <a name="scale-linked-resources"></a><span data-ttu-id="393b8-178">Skala länkade resurserna</span><span class="sxs-lookup"><span data-stu-id="393b8-178">Scale linked resources</span></span>
<span data-ttu-id="393b8-179">Ofta när du skalar en roll är bra tooscale hello databas som programmet hello använder också.</span><span class="sxs-lookup"><span data-stu-id="393b8-179">Often when you scale a role, it's beneficial tooscale hello database that hello application is using also.</span></span> <span data-ttu-id="393b8-180">Du kan komma åt hello skalning inställningar för den här resursen genom att klicka på länken för hello om du länkar hello databasen toohello Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="393b8-180">If you link hello database toohello cloud service, you can access hello scaling settings for that resource by clicking hello appropriate link.</span></span>

1. <span data-ttu-id="393b8-181">I hello [klassiska Azure-portalen](https://manage.windowsazure.com/), klickar du på **molntjänster**, och klicka sedan på hello namnet på hello cloud service tooopen hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="393b8-181">In hello [Azure classic portal](https://manage.windowsazure.com/), click **Cloud Services**, and then click hello name of hello cloud service tooopen hello dashboard.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="393b8-182">Om du inte hittar din molntjänst kan du behöva toochange från **produktion** för**mellanlagring** eller vice versa.</span><span class="sxs-lookup"><span data-stu-id="393b8-182">If you don't see your cloud service, you may need toochange from **Production** too**Staging** or vice versa.</span></span>

2. <span data-ttu-id="393b8-183">Klicka på **skala**.</span><span class="sxs-lookup"><span data-stu-id="393b8-183">Click **Scale**.</span></span>
3. <span data-ttu-id="393b8-184">Hitta hello **länkade resurser** avsnittet och klicka på **hantera skala för den här databasen**.</span><span class="sxs-lookup"><span data-stu-id="393b8-184">Find hello **linked resources** section and click **Manage scale for this database**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="393b8-185">Om du inte ser en **länkade resurser** avsnittet du förmodligen inte har några länkade resurser.</span><span class="sxs-lookup"><span data-stu-id="393b8-185">If you don't see a **linked resources** section, you probably do not have any linked resources.</span></span>

![][linked_resource]

[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
