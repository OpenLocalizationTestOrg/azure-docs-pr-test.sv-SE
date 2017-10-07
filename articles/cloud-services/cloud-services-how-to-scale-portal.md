---
title: "aaaAuto skala en tjänst i molnet i hello portal | Microsoft Docs"
description: "Lär dig hur toouse hello portal tooconfigure automatiskt skala regler för en cloud service webbroll eller en worker-rollen i Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 701d4404-5cc0-454b-999c-feb94c1685c0
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 265f4c8ec5e1ec2f85585df25f18cd0d0c9946a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-auto-scaling-for-a-cloud-service-in-hello-portal"></a><span data-ttu-id="59423-103">Hur tooconfigure automatisk skalning för en tjänst i molnet i hello-portalen</span><span class="sxs-lookup"><span data-stu-id="59423-103">How tooconfigure auto scaling for a Cloud Service in hello portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="59423-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="59423-104">Azure portal</span></span>](cloud-services-how-to-scale-portal.md)
> * [<span data-ttu-id="59423-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="59423-105">Azure classic portal</span></span>](cloud-services-how-to-scale.md)

<span data-ttu-id="59423-106">Villkor kan anges för en cloud service-arbetsroll som utlöser en skala in eller ut igen.</span><span class="sxs-lookup"><span data-stu-id="59423-106">Conditions can be set for a cloud service worker role that trigger a scale in or out operation.</span></span> <span data-ttu-id="59423-107">hello villkor för hello roll kan baseras på hello CPU, disk eller nätverksbelastningen hello-rollen.</span><span class="sxs-lookup"><span data-stu-id="59423-107">hello conditions for hello role can be based on hello CPU, disk, or network load of hello role.</span></span> <span data-ttu-id="59423-108">Du kan också ange ett villkor baserat på ett meddelande kö eller hello mått om vissa andra Azure-resurs som är associerade med prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="59423-108">You can also set a condition based on a message queue or hello metric of some other Azure resource associated with your subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="59423-109">Den här artikeln fokuserar på Molntjänsten webb- och arbetsroller roller.</span><span class="sxs-lookup"><span data-stu-id="59423-109">This article focuses on Cloud Service web and worker roles.</span></span> <span data-ttu-id="59423-110">När du skapar en virtuell dator (klassisk) direkt finns det i en molntjänst.</span><span class="sxs-lookup"><span data-stu-id="59423-110">When you create a virtual machine (classic) directly, it is hosted in a cloud service.</span></span> <span data-ttu-id="59423-111">Du kan skala en vanliga virtuell dator genom att associera den med en [tillgänglighetsuppsättning](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) och aktivera dem manuellt eller inaktivera.</span><span class="sxs-lookup"><span data-stu-id="59423-111">You can scale a standard virtual machine by associating it with an [availability set](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) and manually turn them on or off.</span></span>

## <a name="considerations"></a><span data-ttu-id="59423-112">Överväganden</span><span class="sxs-lookup"><span data-stu-id="59423-112">Considerations</span></span>
<span data-ttu-id="59423-113">Du bör hello följande information innan du konfigurerar skalning för programmet:</span><span class="sxs-lookup"><span data-stu-id="59423-113">You should consider hello following information before you configure scaling for your application:</span></span>

* <span data-ttu-id="59423-114">Skalning påverkas av kärnor användning.</span><span class="sxs-lookup"><span data-stu-id="59423-114">Scaling is affected by core usage.</span></span>

    <span data-ttu-id="59423-115">Större rollinstanser använda flera kärnor.</span><span class="sxs-lookup"><span data-stu-id="59423-115">Larger role instances use more cores.</span></span> <span data-ttu-id="59423-116">Du kan skala ett program bara inom hello kärnor för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="59423-116">You can scale an application only within hello limit of cores for your subscription.</span></span> <span data-ttu-id="59423-117">Anta till exempel har en begränsning på 20 kärnor för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="59423-117">For example, say your subscription has a limit of 20 cores.</span></span> <span data-ttu-id="59423-118">Om du kör ett program med två medelstora molntjänster (totalt 4 kärnor) kan du bara skala upp andra distributioner av molntjänster i din prenumeration av hello återstående 16 kärnor.</span><span class="sxs-lookup"><span data-stu-id="59423-118">If you run an application with two medium-sized cloud services (a total of 4 cores), you can only scale up other cloud service deployments in your subscription by hello remaining 16 cores.</span></span> <span data-ttu-id="59423-119">Mer information om storlekar finns [Molntjänststorlekar](cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="59423-119">For more information about sizes, see [Cloud Service Sizes](cloud-services-sizes-specs.md).</span></span>

* <span data-ttu-id="59423-120">Du kan skala baserat på ett tröskelvärde för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="59423-120">You can scale based on a queue message threshold.</span></span> <span data-ttu-id="59423-121">Mer information om hur toouse köer, se [hur toouse hello Queue Storage Service](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="59423-121">For more information about how toouse queues, see [How toouse hello Queue Storage Service](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span>

* <span data-ttu-id="59423-122">Du kan även skala andra resurser som är associerade med prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="59423-122">You can also scale other resources associated with your subscription.</span></span>

* <span data-ttu-id="59423-123">tooenable hög tillgänglighet för programmet, bör du kontrollera att den har distribuerats med två eller flera rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="59423-123">tooenable high availability of your application, you should ensure that it is deployed with two or more role instances.</span></span> <span data-ttu-id="59423-124">Mer information finns i [serviceavtal](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="59423-124">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>


## <a name="where-scale-is-located"></a><span data-ttu-id="59423-125">Där skala finns</span><span class="sxs-lookup"><span data-stu-id="59423-125">Where scale is located</span></span>
<span data-ttu-id="59423-126">Du bör ha hello cloud service bladet visas när du har valt Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="59423-126">After you select your cloud service, you should have hello cloud service blade visible.</span></span>

1. <span data-ttu-id="59423-127">På hello cloud service bladet på hello **roller och instanser** sida vid sida och välj hello namnet hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="59423-127">On hello cloud service blade, on hello **Roles and Instances** tile, select hello name of hello cloud service.</span></span>   
   <span data-ttu-id="59423-128">**VIKTIGA**: gör att tooclick hello cloud service rollen, inte hello rollinstans som är lägre än hello roll.</span><span class="sxs-lookup"><span data-stu-id="59423-128">**IMPORTANT**: Make sure tooclick hello cloud service role, not hello role instance that is below hello role.</span></span>

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)
2. <span data-ttu-id="59423-129">Välj hello **skala** panelen.</span><span class="sxs-lookup"><span data-stu-id="59423-129">Select hello **scale** tile.</span></span>

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a><span data-ttu-id="59423-130">Automatisk skalning</span><span class="sxs-lookup"><span data-stu-id="59423-130">Automatic scale</span></span>
<span data-ttu-id="59423-131">Du kan konfigurera inställningar för en roll med antingen två lägen **manuell** eller **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="59423-131">You can configure scale settings for a role with either two modes **manual** or **automatic**.</span></span> <span data-ttu-id="59423-132">Manuell som förväntat, du anger hello absolut antal instanser.</span><span class="sxs-lookup"><span data-stu-id="59423-132">Manual is as you would expect, you set hello absolute count of instances.</span></span> <span data-ttu-id="59423-133">Automatisk kan du dock tooset regler som styr hur och med hur mycket du ska skalas.</span><span class="sxs-lookup"><span data-stu-id="59423-133">Automatic however allows you tooset rules that govern how and by how much you should scale.</span></span>

<span data-ttu-id="59423-134">Ange hello **skala genom** alternativet för**schema och prestanda regler**.</span><span class="sxs-lookup"><span data-stu-id="59423-134">Set hello **Scale by** option too**schedule and performance rules**.</span></span>

![Cloud services skalinställningar med profil och regel](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. <span data-ttu-id="59423-136">En befintlig profil.</span><span class="sxs-lookup"><span data-stu-id="59423-136">An existing profile.</span></span>
2. <span data-ttu-id="59423-137">Lägga till en regel för hello överordnade profilen.</span><span class="sxs-lookup"><span data-stu-id="59423-137">Add a rule for hello parent profile.</span></span>
3. <span data-ttu-id="59423-138">Lägg till en annan profil.</span><span class="sxs-lookup"><span data-stu-id="59423-138">Add another profile.</span></span>

<span data-ttu-id="59423-139">Välj **lägga till profilen**.</span><span class="sxs-lookup"><span data-stu-id="59423-139">Select **Add Profile**.</span></span> <span data-ttu-id="59423-140">hello profil avgör vilket läge du vill använda toouse för hello skala: **alltid**, **återkommande**, **fast datum**.</span><span class="sxs-lookup"><span data-stu-id="59423-140">hello profile determines which mode you want toouse for hello scale: **always**, **recurrence**, **fixed date**.</span></span>

<span data-ttu-id="59423-141">När du har konfigurerat hello profil och regler, Välj hello **spara** ikonen överst hello.</span><span class="sxs-lookup"><span data-stu-id="59423-141">After you have configured hello profile and rules, select hello **Save** icon at hello top.</span></span>

#### <a name="profile"></a><span data-ttu-id="59423-142">Profil</span><span class="sxs-lookup"><span data-stu-id="59423-142">Profile</span></span>
<span data-ttu-id="59423-143">hello-profil anger minsta och största instanser för hello skala och även när det här intervallet för skalan är aktiv.</span><span class="sxs-lookup"><span data-stu-id="59423-143">hello profile sets minimum and maximum instances for hello scale, and also when this scale range is active.</span></span>

* <span data-ttu-id="59423-144">**Alltid**</span><span class="sxs-lookup"><span data-stu-id="59423-144">**Always**</span></span>

    <span data-ttu-id="59423-145">Alltid ha detta antal instanser som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="59423-145">Always keep this range of instances available.</span></span>  

    ![Molntjänst som alltid skala](./media/cloud-services-how-to-scale-portal/select-always.png)
* <span data-ttu-id="59423-147">**Upprepning**</span><span class="sxs-lookup"><span data-stu-id="59423-147">**Recurrence**</span></span>

    <span data-ttu-id="59423-148">Välj en uppsättning dagarnas hello vecka tooscale.</span><span class="sxs-lookup"><span data-stu-id="59423-148">Choose a set of days of hello week tooscale.</span></span>

    ![Tjänsten molnskala med ett återkommande schema](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
* <span data-ttu-id="59423-150">**Fast datum**</span><span class="sxs-lookup"><span data-stu-id="59423-150">**Fixed Date**</span></span>

    <span data-ttu-id="59423-151">En fast datum intervallet tooscale hello roll.</span><span class="sxs-lookup"><span data-stu-id="59423-151">A fixed date range tooscale hello role.</span></span>

    ![Tjänsten molnskala med ett fast datum](./media/cloud-services-how-to-scale-portal/select-fixed.png)

<span data-ttu-id="59423-153">När du har konfigurerat hello profil, Välj hello **OK** knappen längst ned hello hello-profilbladet.</span><span class="sxs-lookup"><span data-stu-id="59423-153">After you have configured hello profile, select hello **OK** button at hello bottom of hello profile blade.</span></span>

#### <a name="rule"></a><span data-ttu-id="59423-154">Regel</span><span class="sxs-lookup"><span data-stu-id="59423-154">Rule</span></span>
<span data-ttu-id="59423-155">Regler läggs tooa profil som representerar ett villkor som utlöser hello skala.</span><span class="sxs-lookup"><span data-stu-id="59423-155">Rules are added tooa profile and represent a condition that triggers hello scale.</span></span>

<span data-ttu-id="59423-156">hello regeln utlösare är baserad på ett mått för hello Molntjänsten (CPU-användning, diskaktivitet eller nätverksaktivitet) toowhich som du kan lägga till en villkorlig värde.</span><span class="sxs-lookup"><span data-stu-id="59423-156">hello rule trigger is based on a metric of hello cloud service (CPU usage, disk activity, or network activity) toowhich you can add a conditional value.</span></span> <span data-ttu-id="59423-157">Du kan dessutom ha hello utlösning, baserat på ett meddelande kö eller hello mått om vissa andra Azure-resurs som är associerade med prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="59423-157">Additionally you can have hello trigger based on a message queue or hello metric of some other Azure resource associated with your subscription.</span></span>

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

<span data-ttu-id="59423-158">När du har konfigurerat hello regeln, Välj hello **OK** knappen längst ned hello hello regeln bladet.</span><span class="sxs-lookup"><span data-stu-id="59423-158">After you have configured hello rule, select hello **OK** button at hello bottom of hello rule blade.</span></span>

## <a name="back-toomanual-scale"></a><span data-ttu-id="59423-159">Säkerhetskopiera toomanual skala</span><span class="sxs-lookup"><span data-stu-id="59423-159">Back toomanual scale</span></span>
<span data-ttu-id="59423-160">Navigera toohello [skala inställningar](#where-scale-is-located) och ange hello **skala genom** alternativet för**instansantal som anges manuellt**.</span><span class="sxs-lookup"><span data-stu-id="59423-160">Navigate toohello [scale settings](#where-scale-is-located) and set hello **Scale by** option too**an instance count that I enter manually**.</span></span>

![Cloud services skalinställningar med profil och regel](./media/cloud-services-how-to-scale-portal/manual-basics.png)

<span data-ttu-id="59423-162">Den här inställningen tar bort automatisk skalning från hello-rollen och kan du ange hello instanser direkt.</span><span class="sxs-lookup"><span data-stu-id="59423-162">This setting removes automated scaling from hello role and then you can set hello instance count directly.</span></span>

1. <span data-ttu-id="59423-163">alternativ för hello skala (manuell eller automatisk).</span><span class="sxs-lookup"><span data-stu-id="59423-163">hello scale (manual or automated) option.</span></span>
2. <span data-ttu-id="59423-164">En roll instans skjutreglaget tooset hello instanser tooscale till.</span><span class="sxs-lookup"><span data-stu-id="59423-164">A role instance slider tooset hello instances tooscale to.</span></span>
3. <span data-ttu-id="59423-165">Instanser av hello roll tooscale till.</span><span class="sxs-lookup"><span data-stu-id="59423-165">Instances of hello role tooscale to.</span></span>

<span data-ttu-id="59423-166">När du har konfigurerat hello skalinställningar, Välj hello **spara** ikonen överst hello.</span><span class="sxs-lookup"><span data-stu-id="59423-166">After you have configured hello scale settings, select hello **Save** icon at hello top.</span></span>
