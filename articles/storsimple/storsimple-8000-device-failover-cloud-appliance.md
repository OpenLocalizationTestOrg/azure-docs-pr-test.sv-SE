---
title: aaaStorSimple redundans, disaster recovery tooa StorSimple moln installation | Microsoft Docs
description: "Lär dig hur toofail över din StorSimple 8000-serien fysisk enhet tooa cloud installation."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: e8a0bca057024358e3a557fe85a42ddefea36cff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-tooyour-storsimple-cloud-appliance"></a><span data-ttu-id="c8228-103">Växla över tooyour StorSimple-enhet för molnet</span><span class="sxs-lookup"><span data-stu-id="c8228-103">Fail over tooyour StorSimple Cloud Appliance</span></span>

## <a name="overview"></a><span data-ttu-id="c8228-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="c8228-104">Overview</span></span>

<span data-ttu-id="c8228-105">Den här självstudiekursen beskriver hello steg krävs toofail via en StorSimple 8000-serien fysisk enhet tooa StorSimple-enhet för molnet om en katastrof.</span><span class="sxs-lookup"><span data-stu-id="c8228-105">This tutorial describes hello steps required toofail over a StorSimple 8000 series physical device tooa StorSimple Cloud Appliance if there is a disaster.</span></span> <span data-ttu-id="c8228-106">StorSimple använder hello redundans funktionen toomigrate data på enheten från en fysisk enhet som källa i hello datacenter tooa moln installation körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="c8228-106">StorSimple uses hello device failover feature toomigrate data from a source physical device in hello datacenter tooa cloud appliance running in Azure.</span></span> <span data-ttu-id="c8228-107">hello riktlinjerna i den här kursen gäller tooStorSimple 8000-serien fysiska enheter och molnet installationer med programvaruversioner uppdatering 3 och senare.</span><span class="sxs-lookup"><span data-stu-id="c8228-107">hello guidance in this tutorial applies tooStorSimple 8000 series physical devices and cloud appliances running software versions Update 3 and later.</span></span>

<span data-ttu-id="c8228-108">toolearn mer om enheten växling vid fel och hur den används toorecover en katastrofåterställning gå för[redundans och disaster recovery för StorSimple 8000-serien enheter](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="c8228-108">toolearn more about device failover and how it is used toorecover from a disaster, go too[Failover and disaster recovery for StorSimple 8000 series devices](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

<span data-ttu-id="c8228-109">toofail via en fysisk enhet tooanother fysiska StorSimple-enheten, gå för[växla över fysiska tooa StorSimple-enheten](storsimple-8000-device-failover-physical-device.md).</span><span class="sxs-lookup"><span data-stu-id="c8228-109">toofail over a StorSimple physical device tooanother physical device, go too[Fail over tooa StorSimple physical device](storsimple-8000-device-failover-physical-device.md).</span></span> <span data-ttu-id="c8228-110">toofail via en enhet tooitself gå för[växla över toohello samma fysiska StorSimple-enheten](storsimple-8000-device-failover-same-device.md).</span><span class="sxs-lookup"><span data-stu-id="c8228-110">toofail over a device tooitself, go too[Fail over toohello same StorSimple physical device](storsimple-8000-device-failover-same-device.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8228-111">Krav</span><span class="sxs-lookup"><span data-stu-id="c8228-111">Prerequisites</span></span>

- <span data-ttu-id="c8228-112">Se till att du har granskat hello överväganden för växling vid fel för enheten.</span><span class="sxs-lookup"><span data-stu-id="c8228-112">Ensure that you have reviewed hello considerations for device failover.</span></span> <span data-ttu-id="c8228-113">Mer information finns för[vanliga överväganden för enheten redundans](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="c8228-113">For more information, go too[Common considerations for device failover](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

- <span data-ttu-id="c8228-114">Du måste ha en StorSimple-molnet enhet skapas och konfigureras innan du kör den här proceduren.</span><span class="sxs-lookup"><span data-stu-id="c8228-114">You must have a StorSimple Cloud Appliance created and configured before running this procedure.</span></span> <span data-ttu-id="c8228-115">Om körs uppdaterar 3 programvaruversionen eller senare, Överväg att använda en 8020 moln anordning för hello DR.</span><span class="sxs-lookup"><span data-stu-id="c8228-115">If running   Update 3 software version or later, consider using an 8020 cloud appliance for hello DR.</span></span> <span data-ttu-id="c8228-116">modellen för hello 8020 har 64 TB och Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="c8228-116">hello 8020 model has 64 TB and uses Premium Storage.</span></span> <span data-ttu-id="c8228-117">Mer information finns för[distribuera och hantera en StorSimple-enhet för molnet](storsimple-8000-cloud-appliance-u2.md).</span><span class="sxs-lookup"><span data-stu-id="c8228-117">For more information, go too[Deploy and manage a StorSimple Cloud Appliance](storsimple-8000-cloud-appliance-u2.md).</span></span>

## <a name="steps-toofail-over-tooa-cloud-appliance"></a><span data-ttu-id="c8228-118">Steg toofail över tooa moln-enhet</span><span class="sxs-lookup"><span data-stu-id="c8228-118">Steps toofail over tooa cloud appliance</span></span>

<span data-ttu-id="c8228-119">Utföra hello följande steg toorestore hello enheten tooa mål StorSimple-enhet för molnet.</span><span class="sxs-lookup"><span data-stu-id="c8228-119">Perform hello following steps toorestore hello device tooa target StorSimple Cloud Appliance.</span></span>

1.  <span data-ttu-id="c8228-120">Kontrollera att hello volymbehållare som du vill ha toofail över har associerade molnögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="c8228-120">Verify that hello volume container you want toofail over has associated cloud snapshots.</span></span> <span data-ttu-id="c8228-121">Mer information finns för[Använd StorSimple Enhetshanteraren service toocreate säkerhetskopieringar](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="c8228-121">For more information, go too[Use StorSimple Device Manager service toocreate backups](storsimple-8000-manage-backup-policies-u2.md).</span></span>
2. <span data-ttu-id="c8228-122">Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="c8228-122">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="c8228-123">I hello **enheter** bladet, gå toohello lista över enheter som är anslutna med din tjänst.</span><span class="sxs-lookup"><span data-stu-id="c8228-123">In hello **Devices** blade, go toohello list of devices connected with your service.</span></span>
    <span data-ttu-id="c8228-124">![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)</span><span class="sxs-lookup"><span data-stu-id="c8228-124">![Select device](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)</span></span>
3. <span data-ttu-id="c8228-125">Välj och klicka på din källenhet.</span><span class="sxs-lookup"><span data-stu-id="c8228-125">Select and click your source device.</span></span> <span data-ttu-id="c8228-126">hello källenheten har hello volymbehållare som du vill ha toofail över.</span><span class="sxs-lookup"><span data-stu-id="c8228-126">hello source device has hello volume containers that you want toofail over.</span></span> <span data-ttu-id="c8228-127">Gå för**Inställningar > Volymbehållare**.</span><span class="sxs-lookup"><span data-stu-id="c8228-127">Go too**Settings > Volume Containers**.</span></span>

    ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev2.png)
    
4. <span data-ttu-id="c8228-129">Välj en volymbehållare som du vill att toofail över tooanother enhet.</span><span class="sxs-lookup"><span data-stu-id="c8228-129">Select a volume container that you would like toofail over tooanother device.</span></span> <span data-ttu-id="c8228-130">Klicka på hello volym behållaren toodisplay hello lista över volymer i den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="c8228-130">Click hello volume container toodisplay hello list of volumes within this container.</span></span> <span data-ttu-id="c8228-131">Välj en volym, högerklicka och klicka på **ta Offline** tootake hello volymen offline.</span><span class="sxs-lookup"><span data-stu-id="c8228-131">Select a volume, right-click, and click **Take Offline** tootake hello volume offline.</span></span>

    ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev5.png)

5. <span data-ttu-id="c8228-133">Upprepa proceduren för alla hello volymer i hello volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="c8228-133">Repeat this process for all hello volumes in hello volume container.</span></span>

     ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev7.png)

6. <span data-ttu-id="c8228-135">Hello Upprepa föregående steg för alla hello volymbehållare som toofail över tooanother enhet.</span><span class="sxs-lookup"><span data-stu-id="c8228-135">Repeat hello previous step for all hello volume containers you would like toofail over tooanother device.</span></span>

7. <span data-ttu-id="c8228-136">Gå tillbaka toohello **enheter** bladet.</span><span class="sxs-lookup"><span data-stu-id="c8228-136">Go back toohello **Devices** blade.</span></span> <span data-ttu-id="c8228-137">Hello kommandofältet klickar du på **växla över**.</span><span class="sxs-lookup"><span data-stu-id="c8228-137">From hello command bar, click **Fail over**.</span></span>

    ![Klicka på misslyckas över](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev8.png)
8. <span data-ttu-id="c8228-139">I hello **växla över** bladet utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c8228-139">In hello **Fail over** blade, perform hello following steps:</span></span>
   
    1. <span data-ttu-id="c8228-140">Klicka på **källa**.</span><span class="sxs-lookup"><span data-stu-id="c8228-140">Click **Source**.</span></span> <span data-ttu-id="c8228-141">Välj hello volym behållare toofail över.</span><span class="sxs-lookup"><span data-stu-id="c8228-141">Select hello volume containers toofail over.</span></span> <span data-ttu-id="c8228-142">**Endast hello volymbehållare med associerade molnögonblicksbilder och offline volymer visas.**</span><span class="sxs-lookup"><span data-stu-id="c8228-142">**Only hello volume containers with associated cloud snapshots and offline volumes are displayed.**</span></span>
        <span data-ttu-id="c8228-143">![Välj källa](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)</span><span class="sxs-lookup"><span data-stu-id="c8228-143">![Select source](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)</span></span>
    2. <span data-ttu-id="c8228-144">Klicka på **mål**.</span><span class="sxs-lookup"><span data-stu-id="c8228-144">Click **Target**.</span></span> <span data-ttu-id="c8228-145">Välj ett mål moln installation från hello nedrullningsbara listan över tillgängliga enheter.</span><span class="sxs-lookup"><span data-stu-id="c8228-145">Select a target cloud appliance from hello dropdown list of available devices.</span></span> <span data-ttu-id="c8228-146">**Endast hello-enheter som har tillräcklig kapacitet tooaccommodate källa volymbehållare visas i hello.**</span><span class="sxs-lookup"><span data-stu-id="c8228-146">**Only hello devices that have sufficient capacity tooaccommodate source volume containers are displayed in hello list.**</span></span>

        ![Välj mål](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev12.png)

    3. <span data-ttu-id="c8228-148">Granska inställningarna för hello växling vid fel under **sammanfattning** och välj hello kryssrutan som anger att hello volymer i valda volymbehållare är offline.</span><span class="sxs-lookup"><span data-stu-id="c8228-148">Review hello failover settings under **Summary** and select hello checkbox indicating that hello volumes in selected volume containers are offline.</span></span> 

        ![Granska inställningarna för växling vid fel](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev13.png)

9. <span data-ttu-id="c8228-150">Det skapas ett jobb för växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="c8228-150">A failover job is created.</span></span> <span data-ttu-id="c8228-151">toomonitor hello redundans jobbet, klicka på meddelandet om hello.</span><span class="sxs-lookup"><span data-stu-id="c8228-151">toomonitor hello failover job, click hello job notification.</span></span>

    ![Övervakningsjobb för växling vid fel](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

10. <span data-ttu-id="c8228-153">När hello växling vid fel är klar går du tillbaka toohello **enheter** bladet.</span><span class="sxs-lookup"><span data-stu-id="c8228-153">After hello failover is completed, go back toohello **Devices** blade.</span></span>

    1. <span data-ttu-id="c8228-154">Välj hello-enhet som används som mål för hello hello växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="c8228-154">Select hello device that was used as hello target for hello failover.</span></span>

       ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

    2. <span data-ttu-id="c8228-156">Klicka på **Volymbehållare**.</span><span class="sxs-lookup"><span data-stu-id="c8228-156">Click **Volume Containers**.</span></span> <span data-ttu-id="c8228-157">Alla hello volymbehållare, tillsammans med hello volymer från hello gamla enhet, ska visas.</span><span class="sxs-lookup"><span data-stu-id="c8228-157">All hello volume containers, along with hello volumes from hello old device, should be listed.</span></span>

       <span data-ttu-id="c8228-158">Om hello volymbehållare som du redundansväxlade har lokalt Fäst volymer, har volymerna redundansväxlats som nivåindelade volymer.</span><span class="sxs-lookup"><span data-stu-id="c8228-158">If hello volume container that you failed over has locally pinned volumes, those volumes are failed over as tiered volumes.</span></span> <span data-ttu-id="c8228-159">Lokalt fästa volymer stöds inte på en StorSimple-enhet för molnet.</span><span class="sxs-lookup"><span data-stu-id="c8228-159">Locally pinned volumes are not supported on a StorSimple Cloud Appliance.</span></span>

       ![View target volymbehållare](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev17.png)


## <a name="next-steps"></a><span data-ttu-id="c8228-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c8228-161">Next steps</span></span>

* <span data-ttu-id="c8228-162">När du har utfört en växling vid fel, kanske du måste för[inaktivera eller ta bort din StorSimple-enhet](storsimple-8000-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="c8228-162">After you have performed a failover, you may need too[deactivate or delete your StorSimple device](storsimple-8000-deactivate-and-delete-device.md).</span></span>

* <span data-ttu-id="c8228-163">Information om hur toouse hello StorSimple Device Manager service, gå för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="c8228-163">For information about how toouse hello StorSimple Device Manager service, go too[Use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

