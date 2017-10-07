---
title: aaaStorSimple redundans, disaster recovery tooa StorSimple 8000-serien fysisk enhet | Microsoft Docs
description: "Lär dig hur toofail över StorSimple 8000-serien fysisk enhet tooanother fysiska enheten."
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
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: 29d2576a96c446ff5ffcd98dcd0f5a07f1ab08ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-tooa-storsimple-8000-series-physical-device"></a><span data-ttu-id="9a680-103">Växla över tooa StorSimple 8000-serien fysisk enhet</span><span class="sxs-lookup"><span data-stu-id="9a680-103">Fail over tooa StorSimple 8000 series physical device</span></span>

## <a name="overview"></a><span data-ttu-id="9a680-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="9a680-104">Overview</span></span>

<span data-ttu-id="9a680-105">Den här självstudiekursen beskriver hello steg krävs toofail via en StorSimple 8000-serien fysisk enhet tooanother fysiska StorSimple-enheten om en katastrof.</span><span class="sxs-lookup"><span data-stu-id="9a680-105">This tutorial describes hello steps required toofail over a StorSimple 8000 series physical device tooanother StorSimple physical device if there is a disaster.</span></span> <span data-ttu-id="9a680-106">StorSimple använder hello redundans funktionen toomigrate data på enheten från en fysisk enhet som källa i hello datacenter tooanother fysisk enhet.</span><span class="sxs-lookup"><span data-stu-id="9a680-106">StorSimple uses hello device failover feature toomigrate data from a source physical device in hello datacenter tooanother physical device.</span></span> <span data-ttu-id="9a680-107">hello anvisningarna i den här kursen gäller tooStorSimple 8000-serien fysiska enheter som kör programvaruversioner uppdatering 3 och senare.</span><span class="sxs-lookup"><span data-stu-id="9a680-107">hello guidance in this tutorial applies tooStorSimple 8000 series physical devices running software versions Update 3 and later.</span></span>

<span data-ttu-id="9a680-108">toolearn mer om enheten växling vid fel och hur den används toorecover en katastrofåterställning gå för[redundans och disaster recovery för StorSimple 8000-serien enheter](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="9a680-108">toolearn more about device failover and how it is used toorecover from a disaster, go too[Failover and disaster recovery for StorSimple 8000 series devices](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

<span data-ttu-id="9a680-109">toofail över en StorSimple fysisk enhet tooa StorSimple moln-enhet, gå för[växla över tooa StorSimple moln installation](storsimple-8000-device-failover-cloud-appliance.md).</span><span class="sxs-lookup"><span data-stu-id="9a680-109">toofail over a StorSimple physical device tooa StorSimple Cloud Appliance, go too[Fail over tooa StorSimple Cloud Appliance](storsimple-8000-device-failover-cloud-appliance.md).</span></span> <span data-ttu-id="9a680-110">toofail via en fysisk enhet tooitself gå för[växla över toohello samma fysiska StorSimple-enheten](storsimple-8000-device-failover-same-device.md).</span><span class="sxs-lookup"><span data-stu-id="9a680-110">toofail over a physical device tooitself, go too[Fail over toohello same StorSimple physical device](storsimple-8000-device-failover-same-device.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="9a680-111">Krav</span><span class="sxs-lookup"><span data-stu-id="9a680-111">Prerequisites</span></span>

- <span data-ttu-id="9a680-112">Se till att du har granskat hello överväganden för växling vid fel för enheten.</span><span class="sxs-lookup"><span data-stu-id="9a680-112">Ensure that you have reviewed hello considerations for device failover.</span></span> <span data-ttu-id="9a680-113">Mer information finns för[vanliga överväganden för enheten redundans](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="9a680-113">For more information, go too[Common considerations for device failover](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

- <span data-ttu-id="9a680-114">Du måste ha en StorSimple 8000-serien fysisk enhet som distribuerats i hello datacenter.</span><span class="sxs-lookup"><span data-stu-id="9a680-114">You must have a StorSimple 8000 series physical device deployed in hello datacenter.</span></span> <span data-ttu-id="9a680-115">hello enheten måste köra Update 3 eller senare programvaruversion.</span><span class="sxs-lookup"><span data-stu-id="9a680-115">hello device must run Update 3 or later software version.</span></span> <span data-ttu-id="9a680-116">Mer information finns för[distribuera din lokala StorSimple-enhet](storsimple-8000-deployment-walkthrough-u2.md).</span><span class="sxs-lookup"><span data-stu-id="9a680-116">For more information, go too[Deploy your on-premises StorSimple device](storsimple-8000-deployment-walkthrough-u2.md).</span></span>


## <a name="steps-toofail-over-tooa-physical-device"></a><span data-ttu-id="9a680-117">Steg toofail över tooa fysisk enhet</span><span class="sxs-lookup"><span data-stu-id="9a680-117">Steps toofail over tooa physical device</span></span>

<span data-ttu-id="9a680-118">Utför följande steg toorestore hello din enhet tooa fysiska målenhet.</span><span class="sxs-lookup"><span data-stu-id="9a680-118">Perform hello following steps toorestore your device tooa target physical device.</span></span>

1. <span data-ttu-id="9a680-119">Kontrollera att hello volymbehållare som du vill ha toofail över har associerade molnögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="9a680-119">Verify that hello volume container you want toofail over has associated cloud snapshots.</span></span> <span data-ttu-id="9a680-120">Mer information finns för[Använd StorSimple Enhetshanteraren service toocreate säkerhetskopieringar](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="9a680-120">For more information, go too[Use StorSimple Device Manager service toocreate backups](storsimple-8000-manage-backup-policies-u2.md).</span></span>
2. <span data-ttu-id="9a680-121">Gå tooyour StorSimple Enhetshanteraren och klicka sedan på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="9a680-121">Go tooyour StorSimple Device Manager and then click **Devices**.</span></span> <span data-ttu-id="9a680-122">I hello **enheter** bladet, gå toohello lista över enheter som är anslutna med din tjänst.</span><span class="sxs-lookup"><span data-stu-id="9a680-122">In hello **Devices** blade, go toohello list of devices connected with your service.</span></span>
    <span data-ttu-id="9a680-123">![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)</span><span class="sxs-lookup"><span data-stu-id="9a680-123">![Select device](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)</span></span>
3. <span data-ttu-id="9a680-124">Välj och klicka på din källenhet.</span><span class="sxs-lookup"><span data-stu-id="9a680-124">Select and click your source device.</span></span> <span data-ttu-id="9a680-125">hello källenheten har hello volymbehållare som du vill ha toofail över.</span><span class="sxs-lookup"><span data-stu-id="9a680-125">hello source device has hello volume containers that you want toofail over.</span></span> <span data-ttu-id="9a680-126">Gå för**Inställningar > Volymbehållare**.</span><span class="sxs-lookup"><span data-stu-id="9a680-126">Go too**Settings > Volume Containers**.</span></span>
4. <span data-ttu-id="9a680-127">Välj en volymbehållare som du vill att toofail över tooanother enhet.</span><span class="sxs-lookup"><span data-stu-id="9a680-127">Select a volume container that you would like toofail over tooanother device.</span></span> <span data-ttu-id="9a680-128">Klicka på hello volym behållaren toodisplay hello lista över volymer i den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="9a680-128">Click hello volume container toodisplay hello list of volumes within this container.</span></span> <span data-ttu-id="9a680-129">Välj en volym, högerklicka och klicka på **ta Offline** tootake hello volymen offline.</span><span class="sxs-lookup"><span data-stu-id="9a680-129">Select a volume, right-click, and click **Take Offline** tootake hello volume offline.</span></span> <span data-ttu-id="9a680-130">Upprepa proceduren för alla hello volymer i hello volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="9a680-130">Repeat this process for all hello volumes in hello volume container.</span></span>
5. <span data-ttu-id="9a680-131">Hello Upprepa föregående steg för alla hello volymbehållare som toofail över tooanother enhet.</span><span class="sxs-lookup"><span data-stu-id="9a680-131">Repeat hello previous step for all hello volume containers you would like toofail over tooanother device.</span></span>
6. <span data-ttu-id="9a680-132">Gå tillbaka toohello **enheter** bladet.</span><span class="sxs-lookup"><span data-stu-id="9a680-132">Go back toohello **Devices** blade.</span></span> <span data-ttu-id="9a680-133">Hello kommandofältet klickar du på **växla över**.</span><span class="sxs-lookup"><span data-stu-id="9a680-133">From hello command bar, click **Fail over**.</span></span>
    <span data-ttu-id="9a680-134">![Klicka på misslyckas över](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev2.png)</span><span class="sxs-lookup"><span data-stu-id="9a680-134">![Click fail over](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev2.png)</span></span>
    
7. <span data-ttu-id="9a680-135">I hello **växla över** bladet utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9a680-135">In hello **Fail over** blade, perform hello following steps:</span></span>
   
   1. <span data-ttu-id="9a680-136">Klicka på **källa**.</span><span class="sxs-lookup"><span data-stu-id="9a680-136">Click **Source**.</span></span> <span data-ttu-id="9a680-137">Hej volymbehållare med volymer som är kopplade till molnögonblicksbilder visas.</span><span class="sxs-lookup"><span data-stu-id="9a680-137">hello volume containers with volumes associated with cloud snapshots are displayed.</span></span> <span data-ttu-id="9a680-138">Endast hello behållare som visas är berättigade för redundans.</span><span class="sxs-lookup"><span data-stu-id="9a680-138">Only hello containers displayed are eligible for failover.</span></span> <span data-ttu-id="9a680-139">Välj hello volymbehållare som toofail över i hello lista över volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="9a680-139">In hello list of volume containers, select hello volume containers you would like toofail over.</span></span> <span data-ttu-id="9a680-140">**Endast hello volymbehållare med associerade molnögonblicksbilder och offline volymer visas.**</span><span class="sxs-lookup"><span data-stu-id="9a680-140">**Only hello volume containers with associated cloud snapshots and offline volumes are displayed.**</span></span>

       ![Välj källa](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev5.png)
   2. <span data-ttu-id="9a680-142">Klicka på **mål**.</span><span class="sxs-lookup"><span data-stu-id="9a680-142">Click **Target**.</span></span> <span data-ttu-id="9a680-143">Välj en målenhet för hello volymbehållare valde i föregående steg i hello, hello nedrullningsbara listan över tillgängliga enheter.</span><span class="sxs-lookup"><span data-stu-id="9a680-143">For hello volume containers selected in hello previous step, select a target device from hello drop-down list of available devices.</span></span> <span data-ttu-id="9a680-144">Endast hello-enheter som har tillräcklig kapacitet tooaccommodate källa volymbehållare visas i hello.</span><span class="sxs-lookup"><span data-stu-id="9a680-144">Only hello devices that have sufficient capacity tooaccommodate source volume containers are displayed in hello list.</span></span>

        ![Välj mål](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev6.png)

   3. <span data-ttu-id="9a680-146">Slutligen granska alla hello växling vid fel inställningar under **sammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="9a680-146">Finally, review all hello failover settings under **Summary**.</span></span> <span data-ttu-id="9a680-147">När du har granskat hello inställningar, Välj hello kryssrutan som anger att hello volymer i valda volymbehållare är offline.</span><span class="sxs-lookup"><span data-stu-id="9a680-147">After you have reviewed hello settings, select hello checkbox indicating that hello volumes in selected volume containers are offline.</span></span> <span data-ttu-id="9a680-148">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9a680-148">Click **OK**.</span></span>

       ![Granska inställningarna för växling vid fel](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev8.png)
  
8. <span data-ttu-id="9a680-150">StorSimple skapar en växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="9a680-150">StorSimple creates a failover job.</span></span> <span data-ttu-id="9a680-151">Klicka på hello jobbet meddelande toomonitor hello redundans jobb via hello **jobb** bladet.</span><span class="sxs-lookup"><span data-stu-id="9a680-151">Click hello job notification toomonitor hello failover job via hello **Jobs** blade.</span></span>

    <span data-ttu-id="9a680-152">Om hello volymbehållare som du redundansväxlade har lokala volymer, ser du enskilda återställningsjobb för varje lokal volym (inte för nivåindelade volymer) i hello behållaren.</span><span class="sxs-lookup"><span data-stu-id="9a680-152">If hello volume container that you failed over has local volumes, then you see individual restore jobs for each local volume (not for tiered volumes) in hello container.</span></span> <span data-ttu-id="9a680-153">Dessa återställningspunkter jobb kan ta ganska tid toocomplete.</span><span class="sxs-lookup"><span data-stu-id="9a680-153">These restore jobs may take quite some time toocomplete.</span></span> <span data-ttu-id="9a680-154">Det är troligt att hello beställningsjobbet kan slutföra tidigare.</span><span class="sxs-lookup"><span data-stu-id="9a680-154">It is likely that hello failover job may complete earlier.</span></span> <span data-ttu-id="9a680-155">Volymerna har lokala garantier endast när hello återställningsjobb har slutförts.</span><span class="sxs-lookup"><span data-stu-id="9a680-155">These volumes will have local guarantees only after hello restore jobs are complete.</span></span>

    ![Övervakningsjobb för växling vid fel](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

9. <span data-ttu-id="9a680-157">När hello växling vid fel har slutförts går toohello **enheter** bladet.</span><span class="sxs-lookup"><span data-stu-id="9a680-157">After hello failover is completed, go toohello **Devices** blade.</span></span>
   
   1. <span data-ttu-id="9a680-158">Välj hello-enhet som har använts som hello målenhet för hello failover-processen.</span><span class="sxs-lookup"><span data-stu-id="9a680-158">Select hello device that was used as hello target device for hello failover process.</span></span>

       ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

   2. <span data-ttu-id="9a680-160">Gå toohello **Volymbehållare** bladet.</span><span class="sxs-lookup"><span data-stu-id="9a680-160">Go toohello **Volume Containers** blade.</span></span> <span data-ttu-id="9a680-161">Alla hello volymbehållare, tillsammans med hello volymer från hello gamla enhet, ska visas.</span><span class="sxs-lookup"><span data-stu-id="9a680-161">All hello volume containers, along with hello volumes from hello old device, should be listed.</span></span>

       ![View target volymbehållare](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev16.png)


## <a name="next-steps"></a><span data-ttu-id="9a680-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9a680-163">Next steps</span></span>

* <span data-ttu-id="9a680-164">När du har utfört en växling vid fel, kanske du måste för[inaktivera eller ta bort din StorSimple-enhet](storsimple-8000-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="9a680-164">After you have performed a failover, you may need too[deactivate or delete your StorSimple device](storsimple-8000-deactivate-and-delete-device.md).</span></span>
* <span data-ttu-id="9a680-165">Information om hur toouse hello StorSimple Device Manager service, gå för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="9a680-165">For information about how toouse hello StorSimple Device Manager service, go too[Use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

