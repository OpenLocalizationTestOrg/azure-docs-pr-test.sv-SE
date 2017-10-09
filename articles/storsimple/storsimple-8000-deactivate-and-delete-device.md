---
title: aaaDeactivate och ta bort en StorSimple 8000-serieenhet | Microsoft Docs
description: "Beskriver hur tooremove StorSimple-enhet från tjänsten genom att först inaktivera det och sedan ta bort den."
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
ms.date: 05/23/2017
ms.author: alkohli
ms.openlocfilehash: 841ecd7f0fb5e425bf23e1fe0044faeab2af4b53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deactivate-and-delete-a-storsimple-device"></a><span data-ttu-id="60ef1-103">Inaktivera och ta bort en StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="60ef1-103">Deactivate and delete a StorSimple device</span></span>

## <a name="overview"></a><span data-ttu-id="60ef1-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="60ef1-104">Overview</span></span>

<span data-ttu-id="60ef1-105">Den här artikeln beskriver hur toodeactivate och ta bort en StorSimple-enhet som är anslutna tooa StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="60ef1-105">This article describes how toodeactivate and delete a StorSimple device that is connected tooa StorSimple Device Manager service.</span></span> <span data-ttu-id="60ef1-106">hello anvisningarna i den här artikeln gäller endast tooStorSimple 8000-serien enheter inklusive hello StorSimple moln apparater.</span><span class="sxs-lookup"><span data-stu-id="60ef1-106">hello guidance in this article applies only tooStorSimple 8000 series devices including hello StorSimple Cloud Appliances.</span></span> <span data-ttu-id="60ef1-107">Om du använder en virtuell StorSimple-matris går för[inaktivera och ta bort en virtuell StorSimple-matris](storsimple-virtual-array-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="60ef1-107">If you are using a StorSimple Virtual Array, then go too[Deactivate and delete a StorSimple Virtual Array](storsimple-virtual-array-deactivate-and-delete-device.md).</span></span>

<span data-ttu-id="60ef1-108">Inaktivering av servrarna hello anslutning mellan hello enhets- och hello motsvarande StorSimple Enhetshanteraren.</span><span class="sxs-lookup"><span data-stu-id="60ef1-108">Deactivation severs hello connection between hello device and hello corresponding StorSimple Device Manager service.</span></span> <span data-ttu-id="60ef1-109">Vill du kanske tootake StorSimple-enheten out-of-service (till exempel om du ersätter eller uppgradera din enhet eller om du inte längre använder StorSimple).</span><span class="sxs-lookup"><span data-stu-id="60ef1-109">You may wish tootake a StorSimple device out of service (for example, if you are replacing or upgrading your device or if you are no longer using StorSimple).</span></span> <span data-ttu-id="60ef1-110">I så fall måste toodeactivate hello enheten innan du kan ta bort den.</span><span class="sxs-lookup"><span data-stu-id="60ef1-110">If so, you need toodeactivate hello device before you can delete it.</span></span>

<span data-ttu-id="60ef1-111">När du inaktiverar en enhet är alla data som lagras lokalt på hello enheten inte längre tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="60ef1-111">When you deactivate a device, any data that was stored locally on hello device is no longer accessible.</span></span> <span data-ttu-id="60ef1-112">Endast hello data som är associerade med hello-enhet som lagrats i hello molnet kan återställas.</span><span class="sxs-lookup"><span data-stu-id="60ef1-112">Only hello data associated with hello device that was stored in hello cloud can be recovered.</span></span>

> [!WARNING]
> <span data-ttu-id="60ef1-113">Inaktivering är en PERMANENT åtgärd och kan inte ångras.</span><span class="sxs-lookup"><span data-stu-id="60ef1-113">Deactivation is a PERMANENT operation and cannot be undone.</span></span> <span data-ttu-id="60ef1-114">En inaktiverad enhet kan inte registreras med hello StorSimple enheten Manager-tjänsten om det inte att återställa toofactory standardvärden.</span><span class="sxs-lookup"><span data-stu-id="60ef1-114">A deactivated device cannot be registered with hello StorSimple Device Manager service unless it is reset toofactory defaults.</span></span>
>
> <span data-ttu-id="60ef1-115">Hej fabriksåterställa processen tar bort alla hello-data som lagras lokalt på din enhet.</span><span class="sxs-lookup"><span data-stu-id="60ef1-115">hello factory reset process deletes all hello data that was stored locally on your device.</span></span> <span data-ttu-id="60ef1-116">Därför måste du ta en ögonblicksbild av alla data i molnet innan du inaktiverar en enhet.</span><span class="sxs-lookup"><span data-stu-id="60ef1-116">Therefore, you must take a cloud snapshot of all your data before you deactivate a device.</span></span> <span data-ttu-id="60ef1-117">Den här ögonblicksbild i molnet kan du toorecover alla hello data i ett senare skede.</span><span class="sxs-lookup"><span data-stu-id="60ef1-117">This cloud snapshot allows you toorecover all hello data at a later stage.</span></span>

<span data-ttu-id="60ef1-118">När du har läst den här självstudiekursen kommer du att kunna:</span><span class="sxs-lookup"><span data-stu-id="60ef1-118">After reading this tutorial, you will be able to:</span></span>

* <span data-ttu-id="60ef1-119">Inaktivera en enhet och ta bort hello data.</span><span class="sxs-lookup"><span data-stu-id="60ef1-119">Deactivate a device and delete hello data.</span></span>
* <span data-ttu-id="60ef1-120">Inaktivera en enhet och behålla hello data.</span><span class="sxs-lookup"><span data-stu-id="60ef1-120">Deactivate a device and retain hello data.</span></span>

> [!NOTE]
> <span data-ttu-id="60ef1-121">Innan du inaktiverar en fysiska StorSimple-enheten eller molnet installation, stoppa eller ta bort klienter och värdar som beror på enheten.</span><span class="sxs-lookup"><span data-stu-id="60ef1-121">Before you deactivate a StorSimple physical device or cloud appliance, stop or delete clients and hosts that depend on that device.</span></span>


## <a name="deactivate-and-delete-data"></a><span data-ttu-id="60ef1-122">Inaktivera och ta bort data</span><span class="sxs-lookup"><span data-stu-id="60ef1-122">Deactivate and delete data</span></span>

<span data-ttu-id="60ef1-123">Om du är intresserad av att ta bort hello enheten fullständigt och inte vill att tooretain hello data på hello enhet, slutför du hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="60ef1-123">If you are interested in deleting hello device completely and do not want tooretain hello data on hello device, then complete hello following steps.</span></span>

#### <a name="toodeactivate-hello-device-and-delete-hello-data"></a><span data-ttu-id="60ef1-124">toodeactivate hello enheten och ta bort hello data</span><span class="sxs-lookup"><span data-stu-id="60ef1-124">toodeactivate hello device and delete hello data</span></span>

1. <span data-ttu-id="60ef1-125">Innan du inaktiverar en enhet måste du ta bort alla hello volym behållare (och hello volymer) som är associerade med hello enhet.</span><span class="sxs-lookup"><span data-stu-id="60ef1-125">Before you deactivate a device, you must delete all hello volume containers (and hello volumes) associated with hello device.</span></span> <span data-ttu-id="60ef1-126">Du kan ta bort volymbehållare bara när du har tagit bort hello associerade säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="60ef1-126">You can delete volume containers only after you have deleted hello associated backups.</span></span>

    > [!NOTE]
    > <span data-ttu-id="60ef1-127">Se till att hello data från hello bort volymbehållare tas bort från hello enheten innan du inaktiverar en fysiska StorSimple-enheten eller molnet utrustning.</span><span class="sxs-lookup"><span data-stu-id="60ef1-127">Before you deactivate a StorSimple physical device or cloud appliance, ensure that hello data from hello deleted volume container is actually deleted from hello device.</span></span> <span data-ttu-id="60ef1-128">Du kan övervaka hello molnet förbrukning diagram och när du ser hello molnanvändning släpp på grund av hello säkerhetskopior som du har tagit bort kan du fortsätta toodeactivate hello enhet.</span><span class="sxs-lookup"><span data-stu-id="60ef1-128">You can monitor hello cloud consumption charts and when you see hello cloud usage drop because of hello backups you have deleted, then you can proceed toodeactivate hello device.</span></span> <span data-ttu-id="60ef1-129">Om du inaktiverar hello enheten innan inträffar den här nedrullningsbara hello data ensamma i hello storage-konto och påförs kostnader.</span><span class="sxs-lookup"><span data-stu-id="60ef1-129">If you deactivate hello device before this drop occurs, hello data is stranded in hello storage account and accrues charges.</span></span>

2. <span data-ttu-id="60ef1-130">Inaktivera hello enhet enligt följande:</span><span class="sxs-lookup"><span data-stu-id="60ef1-130">Deactivate hello device as follows:</span></span>
   
   1. <span data-ttu-id="60ef1-131">Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="60ef1-131">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="60ef1-132">I hello **enheter** bladet, Välj hello-enhet som du vill toodeactivate, högerklicka och klicka sedan på **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="60ef1-132">In hello **Devices** blade, select hello device that you wish toodeactivate, right-click, and then click **Deactivate**.</span></span>

        ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. <span data-ttu-id="60ef1-134">I hello **inaktivera** bladet Skriv hello enheten namnet tooconfirm och klicka på **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="60ef1-134">In hello **Deactivate** blade, type hello device name tooconfirm and then click **Deactivate**.</span></span> <span data-ttu-id="60ef1-135">hello inaktivera startar och tar några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="60ef1-135">hello deactivate process starts and takes a few minutes toocomplete.</span></span>

        ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)

3. <span data-ttu-id="60ef1-137">Efter inaktiveringen, kan du ta bort hello enheten helt.</span><span class="sxs-lookup"><span data-stu-id="60ef1-137">After deactivation, you can delete hello device completely.</span></span> <span data-ttu-id="60ef1-138">Om du tar bort en enhet tas den bort från hello lista över enheter anslutna toohello.</span><span class="sxs-lookup"><span data-stu-id="60ef1-138">Deleting a device removes it from hello list of devices connected toohello service.</span></span> <span data-ttu-id="60ef1-139">hello-tjänsten kan sedan inte längre hantera hello bort enheten.</span><span class="sxs-lookup"><span data-stu-id="60ef1-139">hello service can then no longer manage hello deleted device.</span></span> <span data-ttu-id="60ef1-140">Använd hello följande steg toodelete hello enhet:</span><span class="sxs-lookup"><span data-stu-id="60ef1-140">Use hello following steps toodelete hello device:</span></span>
   
   1. <span data-ttu-id="60ef1-141">Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="60ef1-141">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="60ef1-142">I hello **enheter** bladet, Välj hello inaktiveras enhet som du vill toodelete, högerklicka och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="60ef1-142">In hello **Devices** blade, select hello deactivated device that you wish toodelete, right-click, and then click **Delete**.</span></span>

        ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. <span data-ttu-id="60ef1-144">I hello **ta bort** bladet Skriv hello enheten namnet tooconfirm och klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="60ef1-144">In hello **Delete** blade, type hello device name tooconfirm and then click **Delete**.</span></span> <span data-ttu-id="60ef1-145">hello borttagning tar några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="60ef1-145">hello deletion takes a few minutes toocomplete.</span></span>

        ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. <span data-ttu-id="60ef1-147">När hello borttagningen är klar har du ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="60ef1-147">After hello deletion is successfully complete, you are notified.</span></span> <span data-ttu-id="60ef1-148">hello enhetslistan uppdateras även tooreflect hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="60ef1-148">hello device list also updates tooreflect hello deletion.</span></span>

## <a name="deactivate-and-retain-data"></a><span data-ttu-id="60ef1-149">Inaktivera och behålla data</span><span class="sxs-lookup"><span data-stu-id="60ef1-149">Deactivate and retain data</span></span>

<span data-ttu-id="60ef1-150">Om du är intresserad av att ta bort hello enhet men vill tooretain hello data Slutför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="60ef1-150">If you are interested in deleting hello device but want tooretain hello data, then complete hello following steps:</span></span>

#### <a name="toodeactivate-a-device-and-retain-hello-data"></a><span data-ttu-id="60ef1-151">toodeactivate en enhet och spara hello data</span><span class="sxs-lookup"><span data-stu-id="60ef1-151">toodeactivate a device and retain hello data</span></span>
1. <span data-ttu-id="60ef1-152">Inaktivera hello enhet.</span><span class="sxs-lookup"><span data-stu-id="60ef1-152">Deactivate hello device.</span></span> <span data-ttu-id="60ef1-153">Alla hello volymbehållare och förblir hello ögonblicksbilder av hello enhet.</span><span class="sxs-lookup"><span data-stu-id="60ef1-153">All hello volume containers and hello snapshots of hello device remain.</span></span>
   
   1. <span data-ttu-id="60ef1-154">Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="60ef1-154">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="60ef1-155">I hello **enheter** bladet, Välj hello-enhet som du vill toodeactivate, högerklicka och klicka sedan på **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="60ef1-155">In hello **Devices** blade, select hello device that you wish toodeactivate, right-click, and then click **Deactivate**.</span></span>

         ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. <span data-ttu-id="60ef1-157">I hello **inaktivera** bladet Skriv hello enheten namnet tooconfirm och klicka på **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="60ef1-157">In hello **Deactivate** blade, type hello device name tooconfirm and then click **Deactivate**.</span></span> <span data-ttu-id="60ef1-158">hello inaktivera startar och tar några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="60ef1-158">hello deactivate process starts and takes a few minutes toocomplete.</span></span>

         ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)
2. <span data-ttu-id="60ef1-160">Nu kan du växla över hello volymbehållare och hello associerade ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="60ef1-160">You can now fail over hello volume containers and hello associated snapshots.</span></span> <span data-ttu-id="60ef1-161">Procedurer finns för[redundans och disaster recovery för din StorSimple-enhet](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="60ef1-161">For procedures, go too[Failover and disaster recovery for your StorSimple device](storsimple-8000-device-failover-disaster-recovery.md).</span></span>
3. <span data-ttu-id="60ef1-162">Efter inaktivering och växling vid fel, kan du ta bort hello enhet helt.</span><span class="sxs-lookup"><span data-stu-id="60ef1-162">After deactivation and failover, you can delete hello device completely.</span></span> <span data-ttu-id="60ef1-163">Om du tar bort en enhet tas den bort från hello lista över enheter anslutna toohello.</span><span class="sxs-lookup"><span data-stu-id="60ef1-163">Deleting a device removes it from hello list of devices connected toohello service.</span></span> <span data-ttu-id="60ef1-164">hello-tjänsten kan sedan inte längre hantera hello bort enheten.</span><span class="sxs-lookup"><span data-stu-id="60ef1-164">hello service can then no longer manage hello deleted device.</span></span> <span data-ttu-id="60ef1-165">toodelete hello enhet, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="60ef1-165">toodelete hello device, complete hello following steps:</span></span>
   
   1. <span data-ttu-id="60ef1-166">Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="60ef1-166">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="60ef1-167">I hello **enheter** bladet, Välj hello inaktiveras enhet som du vill toodelete, högerklicka och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="60ef1-167">In hello **Devices** blade, select hello deactivated device that you wish toodelete, right-click, and then click **Delete**.</span></span>

       ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. <span data-ttu-id="60ef1-169">I hello **ta bort** bladet Skriv hello enheten namnet tooconfirm och klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="60ef1-169">In hello **Delete** blade, type hello device name tooconfirm and then click **Delete**.</span></span> <span data-ttu-id="60ef1-170">hello borttagning tar några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="60ef1-170">hello deletion takes a few minutes toocomplete.</span></span>

       ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. <span data-ttu-id="60ef1-172">När hello borttagningen är klar har du ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="60ef1-172">After hello deletion is successfully complete, you are notified.</span></span> <span data-ttu-id="60ef1-173">hello enhetslistan uppdateras även tooreflect hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="60ef1-173">hello device list also updates tooreflect hello deletion.</span></span>

     
## <a name="deactivate-and-delete-a-cloud-appliance"></a><span data-ttu-id="60ef1-174">Inaktivera och ta bort en moln-installation</span><span class="sxs-lookup"><span data-stu-id="60ef1-174">Deactivate and delete a cloud appliance</span></span>

<span data-ttu-id="60ef1-175">För en för StorSimple molnet, inaktivering från hello portal tar bort och tar bort hello virtuell dator och hello resurser som skapades när den etablerades.</span><span class="sxs-lookup"><span data-stu-id="60ef1-175">For a StorSimple Cloud Appliance, deactivation from hello portal deallocates and deletes hello virtual machine, and hello resources created when it was provisioned.</span></span> <span data-ttu-id="60ef1-176">När hello molnet installation inaktiveras, kan den inte återställas tooits tidigare tillstånd.</span><span class="sxs-lookup"><span data-stu-id="60ef1-176">After hello cloud appliance is deactivated, it cannot be restored tooits previous state.</span></span>

![Inaktivera molnet StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate7.png)

<span data-ttu-id="60ef1-178">Inaktivera resulterar i hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="60ef1-178">Deactivation results in hello following actions:</span></span>

* <span data-ttu-id="60ef1-179">hello StorSimple moln enheten tas bort från hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="60ef1-179">hello StorSimple Cloud Appliance is removed from hello service.</span></span>
* <span data-ttu-id="60ef1-180">hello virtuell dator för hello StorSimple moln enheten tas bort.</span><span class="sxs-lookup"><span data-stu-id="60ef1-180">hello virtual machine for hello StorSimple Cloud Appliance is deleted.</span></span>
* <span data-ttu-id="60ef1-181">hello operativsystemdisken och datadiskar som skapats för hello StorSimple moln installation tas bort.</span><span class="sxs-lookup"><span data-stu-id="60ef1-181">hello OS disk and data disks created for hello StorSimple Cloud Appliance are removed.</span></span>
* <span data-ttu-id="60ef1-182">hello värdbaserade tjänsten och virtuella nätverk som skapades under etableringen bevaras.</span><span class="sxs-lookup"><span data-stu-id="60ef1-182">hello hosted service and Virtual Network that were created during provisioning are retained.</span></span> <span data-ttu-id="60ef1-183">Om du inte använder dessa enheter, bör du ta bort dem manuellt.</span><span class="sxs-lookup"><span data-stu-id="60ef1-183">If you are not using these entities, you should delete them manually.</span></span>
* <span data-ttu-id="60ef1-184">Molnögonblicksbilder som skapats av hello StorSimple-enhet för molnet bevaras.</span><span class="sxs-lookup"><span data-stu-id="60ef1-184">Cloud snapshots created by hello StorSimple Cloud Appliance are retained.</span></span>

<span data-ttu-id="60ef1-185">När hello molnet enheten har inaktiverats kan du ta bort den från hello lista över enheter.</span><span class="sxs-lookup"><span data-stu-id="60ef1-185">After hello cloud appliance is deactivated, you can delete it from hello list of devices.</span></span> <span data-ttu-id="60ef1-186">Välj hello inaktiveras enheten, högerklicka och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="60ef1-186">Select hello deactivated device, right-click, and then click **Delete**.</span></span> <span data-ttu-id="60ef1-187">StorSimple meddelar dig när hello enheten tas bort och hello listan över enheter uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="60ef1-187">StorSimple notifies you once hello device is deleted and hello list of devices updates.</span></span>

## <a name="next-steps"></a><span data-ttu-id="60ef1-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="60ef1-188">Next steps</span></span>

* <span data-ttu-id="60ef1-189">toorestore hello inaktiverad enhet toofactory standardinställningar finns för[återställer standardinställningarna för hello enheten toofactory](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span><span class="sxs-lookup"><span data-stu-id="60ef1-189">toorestore hello deactivated device toofactory defaults, go too[Reset hello device toofactory default settings](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span></span>
* <span data-ttu-id="60ef1-190">För teknisk support [kontaktar Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="60ef1-190">For technical assistance, [contact Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span></span>
* <span data-ttu-id="60ef1-191">toolearn mer om hur toouse hello StorSimple Enhetshanteraren service gå för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="60ef1-191">toolearn more about how toouse hello StorSimple Device Manager service, go too[Use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

