---
title: "StorSimple-redundans, återställning till en fysisk enhet för StorSimple 8000-serien | Microsoft Docs"
description: "Lär dig mer om att växla över din StorSimple 8000-serien fysisk enhet till en annan fysisk enhet."
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
ms.openlocfilehash: f3ac9545a341fc24ca12c9f2547805d6956cd98a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="fail-over-to-a-storsimple-8000-series-physical-device"></a><span data-ttu-id="9803b-103">Växla över till en fysisk enhet för StorSimple 8000-serien</span><span class="sxs-lookup"><span data-stu-id="9803b-103">Fail over to a StorSimple 8000 series physical device</span></span>

## <a name="overview"></a><span data-ttu-id="9803b-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="9803b-104">Overview</span></span>

<span data-ttu-id="9803b-105">Den här självstudiekursen beskriver de steg som krävs för att växla över en StorSimple 8000-serien fysisk enhet till en annan fysiska StorSimple-enheten om en katastrof.</span><span class="sxs-lookup"><span data-stu-id="9803b-105">This tutorial describes the steps required to fail over a StorSimple 8000 series physical device to another StorSimple physical device if there is a disaster.</span></span> <span data-ttu-id="9803b-106">StorSimple använder funktionen med redundans i enheten för att migrera data från en fysisk enhet som källa i datacentret till en annan fysisk enhet.</span><span class="sxs-lookup"><span data-stu-id="9803b-106">StorSimple uses the device failover feature to migrate data from a source physical device in the datacenter to another physical device.</span></span> <span data-ttu-id="9803b-107">Riktlinjerna i den här kursen gäller StorSimple 8000-serien fysiska enheter som kör programvaruversioner uppdatering 3 och senare.</span><span class="sxs-lookup"><span data-stu-id="9803b-107">The guidance in this tutorial applies to StorSimple 8000 series physical devices running software versions Update 3 and later.</span></span>

<span data-ttu-id="9803b-108">Mer information om enheten växling vid fel och hur den används för att återställa från en katastrof, gå till [redundans och disaster recovery för StorSimple 8000-serien enheter](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="9803b-108">To learn more about device failover and how it is used to recover from a disaster, go to [Failover and disaster recovery for StorSimple 8000 series devices](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

<span data-ttu-id="9803b-109">Om du vill växla över en fysiska StorSimple-enheten till ett moln StorSimple-enhet, gå till [växla över till en StorSimple-enhet för molnet](storsimple-8000-device-failover-cloud-appliance.md).</span><span class="sxs-lookup"><span data-stu-id="9803b-109">To fail over a StorSimple physical device to a StorSimple Cloud Appliance, go to [Fail over to a StorSimple Cloud Appliance](storsimple-8000-device-failover-cloud-appliance.md).</span></span> <span data-ttu-id="9803b-110">Om du vill växla över en fysisk enhet till sig själv, gå till [växla över till samma fysiska StorSimple-enheten](storsimple-8000-device-failover-same-device.md).</span><span class="sxs-lookup"><span data-stu-id="9803b-110">To fail over a physical device to itself, go to [Fail over to the same StorSimple physical device](storsimple-8000-device-failover-same-device.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="9803b-111">Krav</span><span class="sxs-lookup"><span data-stu-id="9803b-111">Prerequisites</span></span>

- <span data-ttu-id="9803b-112">Se till att du har granskat överväganden för växling vid fel för enheten.</span><span class="sxs-lookup"><span data-stu-id="9803b-112">Ensure that you have reviewed the considerations for device failover.</span></span> <span data-ttu-id="9803b-113">Mer information finns på [vanliga överväganden för enheten redundans](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="9803b-113">For more information, go to [Common considerations for device failover](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

- <span data-ttu-id="9803b-114">Du måste ha en StorSimple 8000-serien fysisk enhet som distribuerats i datacentret.</span><span class="sxs-lookup"><span data-stu-id="9803b-114">You must have a StorSimple 8000 series physical device deployed in the datacenter.</span></span> <span data-ttu-id="9803b-115">Enheten måste köra Update 3 eller senare programvaruversion.</span><span class="sxs-lookup"><span data-stu-id="9803b-115">The device must run Update 3 or later software version.</span></span> <span data-ttu-id="9803b-116">Mer information finns på [distribuera din lokala StorSimple-enhet](storsimple-8000-deployment-walkthrough-u2.md).</span><span class="sxs-lookup"><span data-stu-id="9803b-116">For more information, go to [Deploy your on-premises StorSimple device](storsimple-8000-deployment-walkthrough-u2.md).</span></span>


## <a name="steps-to-fail-over-to-a-physical-device"></a><span data-ttu-id="9803b-117">Steg för att växla över till en fysisk enhet</span><span class="sxs-lookup"><span data-stu-id="9803b-117">Steps to fail over to a physical device</span></span>

<span data-ttu-id="9803b-118">Utför följande steg för att återställa enheten till en fysisk enhet som mål.</span><span class="sxs-lookup"><span data-stu-id="9803b-118">Perform the following steps to restore your device to a target physical device.</span></span>

1. <span data-ttu-id="9803b-119">Kontrollera att volymbehållaren som du vill växla över associeras molnögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="9803b-119">Verify that the volume container you want to fail over has associated cloud snapshots.</span></span> <span data-ttu-id="9803b-120">Mer information finns på [Använd Enhetshanteraren för StorSimple-tjänsten för att skapa säkerhetskopior](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="9803b-120">For more information, go to [Use StorSimple Device Manager service to create backups](storsimple-8000-manage-backup-policies-u2.md).</span></span>
2. <span data-ttu-id="9803b-121">Gå till din StorSimple-Enhetshanteraren och klicka sedan på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="9803b-121">Go to your StorSimple Device Manager and then click **Devices**.</span></span> <span data-ttu-id="9803b-122">I den **enheter** bladet, gå till listan över enheter som är anslutna med din tjänst.</span><span class="sxs-lookup"><span data-stu-id="9803b-122">In the **Devices** blade, go to the list of devices connected with your service.</span></span>
    <span data-ttu-id="9803b-123">![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)</span><span class="sxs-lookup"><span data-stu-id="9803b-123">![Select device](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)</span></span>
3. <span data-ttu-id="9803b-124">Välj och klicka på din källenhet.</span><span class="sxs-lookup"><span data-stu-id="9803b-124">Select and click your source device.</span></span> <span data-ttu-id="9803b-125">På källenheten har volymbehållarna som du vill växla över.</span><span class="sxs-lookup"><span data-stu-id="9803b-125">The source device has the volume containers that you want to fail over.</span></span> <span data-ttu-id="9803b-126">Gå till **Inställningar > Volymbehållare**.</span><span class="sxs-lookup"><span data-stu-id="9803b-126">Go to **Settings > Volume Containers**.</span></span>
4. <span data-ttu-id="9803b-127">Välj en volymbehållare som du vill växla över till en annan enhet.</span><span class="sxs-lookup"><span data-stu-id="9803b-127">Select a volume container that you would like to fail over to another device.</span></span> <span data-ttu-id="9803b-128">Klicka på volymbehållare om du vill visa en lista över volymer i den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="9803b-128">Click the volume container to display the list of volumes within this container.</span></span> <span data-ttu-id="9803b-129">Välj en volym, högerklicka och klicka på **ta Offline** att kopplas ifrån volymen.</span><span class="sxs-lookup"><span data-stu-id="9803b-129">Select a volume, right-click, and click **Take Offline** to take the volume offline.</span></span> <span data-ttu-id="9803b-130">Upprepa proceduren för alla volymer i volymbehållaren.</span><span class="sxs-lookup"><span data-stu-id="9803b-130">Repeat this process for all the volumes in the volume container.</span></span>
5. <span data-ttu-id="9803b-131">Upprepa det föregående steget för alla volymbehållare som du vill växla över till en annan enhet.</span><span class="sxs-lookup"><span data-stu-id="9803b-131">Repeat the previous step for all the volume containers you would like to fail over to another device.</span></span>
6. <span data-ttu-id="9803b-132">Gå tillbaka till den **enheter** bladet.</span><span class="sxs-lookup"><span data-stu-id="9803b-132">Go back to the **Devices** blade.</span></span> <span data-ttu-id="9803b-133">Klicka på kommandofältet **växla över**.</span><span class="sxs-lookup"><span data-stu-id="9803b-133">From the command bar, click **Fail over**.</span></span>
    <span data-ttu-id="9803b-134">![Klicka på misslyckas över](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev2.png)</span><span class="sxs-lookup"><span data-stu-id="9803b-134">![Click fail over](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev2.png)</span></span>
    
7. <span data-ttu-id="9803b-135">I den **växla över** bladet utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="9803b-135">In the **Fail over** blade, perform the following steps:</span></span>
   
   1. <span data-ttu-id="9803b-136">Klicka på **källa**.</span><span class="sxs-lookup"><span data-stu-id="9803b-136">Click **Source**.</span></span> <span data-ttu-id="9803b-137">Volymbehållare med volymer som är kopplade till molnögonblicksbilder visas.</span><span class="sxs-lookup"><span data-stu-id="9803b-137">The volume containers with volumes associated with cloud snapshots are displayed.</span></span> <span data-ttu-id="9803b-138">De behållare som visas är berättigade för redundans.</span><span class="sxs-lookup"><span data-stu-id="9803b-138">Only the containers displayed are eligible for failover.</span></span> <span data-ttu-id="9803b-139">Välj volymbehållarna som du vill växla över i listan över volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="9803b-139">In the list of volume containers, select the volume containers you would like to fail over.</span></span> <span data-ttu-id="9803b-140">**Volymbehållare med associerade molnögonblicksbilder och offline volymer visas.**</span><span class="sxs-lookup"><span data-stu-id="9803b-140">**Only the volume containers with associated cloud snapshots and offline volumes are displayed.**</span></span>

       ![Välj källa](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev5.png)
   2. <span data-ttu-id="9803b-142">Klicka på **mål**.</span><span class="sxs-lookup"><span data-stu-id="9803b-142">Click **Target**.</span></span> <span data-ttu-id="9803b-143">Välj en målenhet för volymbehållarna valde i föregående steg, från listan över tillgängliga enheter.</span><span class="sxs-lookup"><span data-stu-id="9803b-143">For the volume containers selected in the previous step, select a target device from the drop-down list of available devices.</span></span> <span data-ttu-id="9803b-144">Endast de enheter som har tillräckligt med kapacitet för källa volymbehållare visas i listan.</span><span class="sxs-lookup"><span data-stu-id="9803b-144">Only the devices that have sufficient capacity to accommodate source volume containers are displayed in the list.</span></span>

        ![Välj mål](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev6.png)

   3. <span data-ttu-id="9803b-146">Slutligen granska alla inställningar för växling vid fel under **sammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="9803b-146">Finally, review all the failover settings under **Summary**.</span></span> <span data-ttu-id="9803b-147">När du har granskat inställningarna, markerar du kryssrutan som anger att volymerna i valda volymbehållare är offline.</span><span class="sxs-lookup"><span data-stu-id="9803b-147">After you have reviewed the settings, select the checkbox indicating that the volumes in selected volume containers are offline.</span></span> <span data-ttu-id="9803b-148">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9803b-148">Click **OK**.</span></span>

       ![Granska inställningarna för växling vid fel](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev8.png)
  
8. <span data-ttu-id="9803b-150">StorSimple skapar en växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="9803b-150">StorSimple creates a failover job.</span></span> <span data-ttu-id="9803b-151">Klicka på meddelandet för jobbet att övervaka beställningsjobbet via den **jobb** bladet.</span><span class="sxs-lookup"><span data-stu-id="9803b-151">Click the job notification to monitor the failover job via the **Jobs** blade.</span></span>

    <span data-ttu-id="9803b-152">Om volymbehållaren som du redundansväxlade har lokala volymer kan se du enskilda återställningsjobb för varje lokal volym (inte för nivåindelade volymer) i behållaren.</span><span class="sxs-lookup"><span data-stu-id="9803b-152">If the volume container that you failed over has local volumes, then you see individual restore jobs for each local volume (not for tiered volumes) in the container.</span></span> <span data-ttu-id="9803b-153">Dessa jobb kan ta tid för att slutföra återställningen.</span><span class="sxs-lookup"><span data-stu-id="9803b-153">These restore jobs may take quite some time to complete.</span></span> <span data-ttu-id="9803b-154">Det är troligt att beställningsjobbet kan slutföra tidigare.</span><span class="sxs-lookup"><span data-stu-id="9803b-154">It is likely that the failover job may complete earlier.</span></span> <span data-ttu-id="9803b-155">Volymerna har lokala garantier endast när återställningsjobb har slutförts.</span><span class="sxs-lookup"><span data-stu-id="9803b-155">These volumes will have local guarantees only after the restore jobs are complete.</span></span>

    ![Övervakningsjobb för växling vid fel](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

9. <span data-ttu-id="9803b-157">När redundansväxlingen är klar går du till den **enheter** bladet.</span><span class="sxs-lookup"><span data-stu-id="9803b-157">After the failover is completed, go to the **Devices** blade.</span></span>
   
   1. <span data-ttu-id="9803b-158">Välj den enhet som användes som målenhet för failover-processen.</span><span class="sxs-lookup"><span data-stu-id="9803b-158">Select the device that was used as the target device for the failover process.</span></span>

       ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

   2. <span data-ttu-id="9803b-160">Gå till den **Volymbehållare** bladet.</span><span class="sxs-lookup"><span data-stu-id="9803b-160">Go to the **Volume Containers** blade.</span></span> <span data-ttu-id="9803b-161">Alla volymbehållare tillsammans med volymer från den gamla enheten ska visas.</span><span class="sxs-lookup"><span data-stu-id="9803b-161">All the volume containers, along with the volumes from the old device, should be listed.</span></span>

       ![View target volymbehållare](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev16.png)


## <a name="next-steps"></a><span data-ttu-id="9803b-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9803b-163">Next steps</span></span>

* <span data-ttu-id="9803b-164">När du har utfört en växling vid fel, kan du behöva [inaktivera eller ta bort din StorSimple-enhet](storsimple-8000-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="9803b-164">After you have performed a failover, you may need to [deactivate or delete your StorSimple device](storsimple-8000-deactivate-and-delete-device.md).</span></span>
* <span data-ttu-id="9803b-165">För information om hur du använder tjänsten StorSimple Enhetshanteraren, gå till [använda Enhetshanteraren för StorSimple-tjänsten för att administrera din StorSimple-enhet](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="9803b-165">For information about how to use the StorSimple Device Manager service, go to [Use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

