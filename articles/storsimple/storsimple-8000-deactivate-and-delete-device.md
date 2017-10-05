---
title: Inaktivera och ta bort en StorSimple 8000-serieenhet | Microsoft Docs
description: "Beskriver hur du tar bort StorSimple-enhet från tjänsten genom att först inaktivera det och sedan ta bort den."
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
ms.openlocfilehash: 3c00867a29cf8343a57e74e2aabe3971ae6837af
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="deactivate-and-delete-a-storsimple-device"></a><span data-ttu-id="54b36-103">Inaktivera och ta bort en StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="54b36-103">Deactivate and delete a StorSimple device</span></span>

## <a name="overview"></a><span data-ttu-id="54b36-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="54b36-104">Overview</span></span>

<span data-ttu-id="54b36-105">Den här artikeln beskriver hur du inaktivera och ta bort en StorSimple-enhet som är ansluten till en StorSimple Device Manager-tjänst.</span><span class="sxs-lookup"><span data-stu-id="54b36-105">This article describes how to deactivate and delete a StorSimple device that is connected to a StorSimple Device Manager service.</span></span> <span data-ttu-id="54b36-106">Riktlinjerna i den här artikeln gäller endast StorSimple 8000-serien enheter, inklusive StorSimple moln installationer.</span><span class="sxs-lookup"><span data-stu-id="54b36-106">The guidance in this article applies only to StorSimple 8000 series devices including the StorSimple Cloud Appliances.</span></span> <span data-ttu-id="54b36-107">Om du använder en virtuell StorSimple-matris, går du till [inaktivera och ta bort en virtuell StorSimple-matris](storsimple-virtual-array-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="54b36-107">If you are using a StorSimple Virtual Array, then go to [Deactivate and delete a StorSimple Virtual Array](storsimple-virtual-array-deactivate-and-delete-device.md).</span></span>

<span data-ttu-id="54b36-108">Inaktivering av servrarna anslutningen mellan enheten och motsvarande StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="54b36-108">Deactivation severs the connection between the device and the corresponding StorSimple Device Manager service.</span></span> <span data-ttu-id="54b36-109">Du kanske vill vidta en StorSimple-enhet out-of-service (till exempel om du ersätter eller uppgradera din enhet eller om du inte längre använder StorSimple).</span><span class="sxs-lookup"><span data-stu-id="54b36-109">You may wish to take a StorSimple device out of service (for example, if you are replacing or upgrading your device or if you are no longer using StorSimple).</span></span> <span data-ttu-id="54b36-110">I så fall, måste du inaktivera enheten innan du kan ta bort den.</span><span class="sxs-lookup"><span data-stu-id="54b36-110">If so, you need to deactivate the device before you can delete it.</span></span>

<span data-ttu-id="54b36-111">När du inaktiverar en enhet är alla data som lagras lokalt på enheten inte längre tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="54b36-111">When you deactivate a device, any data that was stored locally on the device is no longer accessible.</span></span> <span data-ttu-id="54b36-112">Endast data som är kopplade till den enhet som lagrades i molnet kan återställas.</span><span class="sxs-lookup"><span data-stu-id="54b36-112">Only the data associated with the device that was stored in the cloud can be recovered.</span></span>

> [!WARNING]
> <span data-ttu-id="54b36-113">Inaktivering är en PERMANENT åtgärd och kan inte ångras.</span><span class="sxs-lookup"><span data-stu-id="54b36-113">Deactivation is a PERMANENT operation and cannot be undone.</span></span> <span data-ttu-id="54b36-114">En inaktiverad enhet kan inte registreras med tjänsten StorSimple Enhetshanteraren såvida inte den återställs till fabriksinställningarna.</span><span class="sxs-lookup"><span data-stu-id="54b36-114">A deactivated device cannot be registered with the StorSimple Device Manager service unless it is reset to factory defaults.</span></span>
>
> <span data-ttu-id="54b36-115">Fabriksåterställa processen tar bort alla data som lagras lokalt på din enhet.</span><span class="sxs-lookup"><span data-stu-id="54b36-115">The factory reset process deletes all the data that was stored locally on your device.</span></span> <span data-ttu-id="54b36-116">Därför måste du ta en ögonblicksbild av alla data i molnet innan du inaktiverar en enhet.</span><span class="sxs-lookup"><span data-stu-id="54b36-116">Therefore, you must take a cloud snapshot of all your data before you deactivate a device.</span></span> <span data-ttu-id="54b36-117">Den här ögonblicksbild i molnet kan du återställa alla data i ett senare skede.</span><span class="sxs-lookup"><span data-stu-id="54b36-117">This cloud snapshot allows you to recover all the data at a later stage.</span></span>

<span data-ttu-id="54b36-118">När du har läst den här självstudiekursen kommer du att kunna:</span><span class="sxs-lookup"><span data-stu-id="54b36-118">After reading this tutorial, you will be able to:</span></span>

* <span data-ttu-id="54b36-119">Inaktivera en enhet och ta bort data.</span><span class="sxs-lookup"><span data-stu-id="54b36-119">Deactivate a device and delete the data.</span></span>
* <span data-ttu-id="54b36-120">Inaktivera en enhet och behålla data.</span><span class="sxs-lookup"><span data-stu-id="54b36-120">Deactivate a device and retain the data.</span></span>

> [!NOTE]
> <span data-ttu-id="54b36-121">Innan du inaktiverar en fysiska StorSimple-enheten eller molnet installation, stoppa eller ta bort klienter och värdar som beror på enheten.</span><span class="sxs-lookup"><span data-stu-id="54b36-121">Before you deactivate a StorSimple physical device or cloud appliance, stop or delete clients and hosts that depend on that device.</span></span>


## <a name="deactivate-and-delete-data"></a><span data-ttu-id="54b36-122">Inaktivera och ta bort data</span><span class="sxs-lookup"><span data-stu-id="54b36-122">Deactivate and delete data</span></span>

<span data-ttu-id="54b36-123">Slutför följande steg om du är intresserad av att ta bort enheten fullständigt och inte vill behålla data på enheten.</span><span class="sxs-lookup"><span data-stu-id="54b36-123">If you are interested in deleting the device completely and do not want to retain the data on the device, then complete the following steps.</span></span>

#### <a name="to-deactivate-the-device-and-delete-the-data"></a><span data-ttu-id="54b36-124">Att inaktivera enheten och ta bort data</span><span class="sxs-lookup"><span data-stu-id="54b36-124">To deactivate the device and delete the data</span></span>

1. <span data-ttu-id="54b36-125">Innan du inaktiverar en enhet måste du ta bort alla volymen behållare (och volymer) som är kopplade till enheten.</span><span class="sxs-lookup"><span data-stu-id="54b36-125">Before you deactivate a device, you must delete all the volume containers (and the volumes) associated with the device.</span></span> <span data-ttu-id="54b36-126">Du kan ta bort volymbehållare endast när du tar bort tillhörande säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="54b36-126">You can delete volume containers only after you have deleted the associated backups.</span></span>

    > [!NOTE]
    > <span data-ttu-id="54b36-127">Se till att data från behållaren borttagna volymen tas bort från enheten innan du inaktiverar en fysiska StorSimple-enheten eller molnet utrustning.</span><span class="sxs-lookup"><span data-stu-id="54b36-127">Before you deactivate a StorSimple physical device or cloud appliance, ensure that the data from the deleted volume container is actually deleted from the device.</span></span> <span data-ttu-id="54b36-128">Du kan övervaka molnet förbrukning diagram och när du ser molnanvändning släpp på grund av de säkerhetskopior som du har tagit bort kan du fortsätta att inaktivera enheten.</span><span class="sxs-lookup"><span data-stu-id="54b36-128">You can monitor the cloud consumption charts and when you see the cloud usage drop because of the backups you have deleted, then you can proceed to deactivate the device.</span></span> <span data-ttu-id="54b36-129">Om du inaktiverar enheten innan den här nedrullningsbara sker data ensamma i lagringskontot och påförs kostnader.</span><span class="sxs-lookup"><span data-stu-id="54b36-129">If you deactivate the device before this drop occurs, the data is stranded in the storage account and accrues charges.</span></span>

2. <span data-ttu-id="54b36-130">Inaktivera enheten enligt följande:</span><span class="sxs-lookup"><span data-stu-id="54b36-130">Deactivate the device as follows:</span></span>
   
   1. <span data-ttu-id="54b36-131">Gå till StorSimple Device Manager-tjänsten och klicka på **Enheter**.</span><span class="sxs-lookup"><span data-stu-id="54b36-131">Go to your StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="54b36-132">I den **enheter** bladet Välj den enhet som du vill inaktivera, högerklicka och klicka sedan på **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="54b36-132">In the **Devices** blade, select the device that you wish to deactivate, right-click, and then click **Deactivate**.</span></span>

        ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. <span data-ttu-id="54b36-134">I den **inaktivera** bladet, typ enhetsnamnet för att bekräfta och klicka sedan på **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="54b36-134">In the **Deactivate** blade, type the device name to confirm and then click **Deactivate**.</span></span> <span data-ttu-id="54b36-135">Inaktivera processen startar och tar några minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="54b36-135">The deactivate process starts and takes a few minutes to complete.</span></span>

        ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)

3. <span data-ttu-id="54b36-137">Efter inaktiveringen, kan du ta bort enheten helt.</span><span class="sxs-lookup"><span data-stu-id="54b36-137">After deactivation, you can delete the device completely.</span></span> <span data-ttu-id="54b36-138">Om du tar bort en enhet tas den bort från listan över enheter som är anslutna till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="54b36-138">Deleting a device removes it from the list of devices connected to the service.</span></span> <span data-ttu-id="54b36-139">Tjänsten kan sedan inte längre hantera enheten tagits bort.</span><span class="sxs-lookup"><span data-stu-id="54b36-139">The service can then no longer manage the deleted device.</span></span> <span data-ttu-id="54b36-140">Använd följande steg för att ta bort enheten:</span><span class="sxs-lookup"><span data-stu-id="54b36-140">Use the following steps to delete the device:</span></span>
   
   1. <span data-ttu-id="54b36-141">Gå till StorSimple Device Manager-tjänsten och klicka på **Enheter**.</span><span class="sxs-lookup"><span data-stu-id="54b36-141">Go to your StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="54b36-142">I den **enheter** bladet välj inaktiverade enheten som du vill ta bort, högerklicka och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="54b36-142">In the **Devices** blade, select the deactivated device that you wish to delete, right-click, and then click **Delete**.</span></span>

        ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. <span data-ttu-id="54b36-144">I den **ta bort** bladet, typ enhetsnamnet för att bekräfta och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="54b36-144">In the **Delete** blade, type the device name to confirm and then click **Delete**.</span></span> <span data-ttu-id="54b36-145">Borttagningen tar några minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="54b36-145">The deletion takes a few minutes to complete.</span></span>

        ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. <span data-ttu-id="54b36-147">När borttagningen är klar har du ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="54b36-147">After the deletion is successfully complete, you are notified.</span></span> <span data-ttu-id="54b36-148">Listan över enheter uppdateras också borttagningen.</span><span class="sxs-lookup"><span data-stu-id="54b36-148">The device list also updates to reflect the deletion.</span></span>

## <a name="deactivate-and-retain-data"></a><span data-ttu-id="54b36-149">Inaktivera och behålla data</span><span class="sxs-lookup"><span data-stu-id="54b36-149">Deactivate and retain data</span></span>

<span data-ttu-id="54b36-150">Om du är intresserad av att ta bort enheten, men du vill behålla data, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="54b36-150">If you are interested in deleting the device but want to retain the data, then complete the following steps:</span></span>

#### <a name="to-deactivate-a-device-and-retain-the-data"></a><span data-ttu-id="54b36-151">Inaktivera en enhet och behålla data</span><span class="sxs-lookup"><span data-stu-id="54b36-151">To deactivate a device and retain the data</span></span>
1. <span data-ttu-id="54b36-152">Inaktivera enheten.</span><span class="sxs-lookup"><span data-stu-id="54b36-152">Deactivate the device.</span></span> <span data-ttu-id="54b36-153">Alla volymbehållare och ögonblicksbilder av enheten finns kvar.</span><span class="sxs-lookup"><span data-stu-id="54b36-153">All the volume containers and the snapshots of the device remain.</span></span>
   
   1. <span data-ttu-id="54b36-154">Gå till StorSimple Device Manager-tjänsten och klicka på **Enheter**.</span><span class="sxs-lookup"><span data-stu-id="54b36-154">Go to your StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="54b36-155">I den **enheter** bladet Välj den enhet som du vill inaktivera, högerklicka och klicka sedan på **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="54b36-155">In the **Devices** blade, select the device that you wish to deactivate, right-click, and then click **Deactivate**.</span></span>

         ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. <span data-ttu-id="54b36-157">I den **inaktivera** bladet, typ enhetsnamnet för att bekräfta och klicka sedan på **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="54b36-157">In the **Deactivate** blade, type the device name to confirm and then click **Deactivate**.</span></span> <span data-ttu-id="54b36-158">Inaktivera processen startar och tar några minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="54b36-158">The deactivate process starts and takes a few minutes to complete.</span></span>

         ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)
2. <span data-ttu-id="54b36-160">Nu kan du växla över volymbehållarna och associerade ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="54b36-160">You can now fail over the volume containers and the associated snapshots.</span></span> <span data-ttu-id="54b36-161">Mer information om procedurer går du till [redundans och disaster recovery för din StorSimple-enhet](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="54b36-161">For procedures, go to [Failover and disaster recovery for your StorSimple device](storsimple-8000-device-failover-disaster-recovery.md).</span></span>
3. <span data-ttu-id="54b36-162">Efter inaktivering och växling vid fel, kan du ta bort enheten helt.</span><span class="sxs-lookup"><span data-stu-id="54b36-162">After deactivation and failover, you can delete the device completely.</span></span> <span data-ttu-id="54b36-163">Om du tar bort en enhet tas den bort från listan över enheter som är anslutna till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="54b36-163">Deleting a device removes it from the list of devices connected to the service.</span></span> <span data-ttu-id="54b36-164">Tjänsten kan sedan inte längre hantera enheten tagits bort.</span><span class="sxs-lookup"><span data-stu-id="54b36-164">The service can then no longer manage the deleted device.</span></span> <span data-ttu-id="54b36-165">Om du vill ta bort enheten, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="54b36-165">To delete the device, complete the following steps:</span></span>
   
   1. <span data-ttu-id="54b36-166">Gå till StorSimple Device Manager-tjänsten och klicka på **Enheter**.</span><span class="sxs-lookup"><span data-stu-id="54b36-166">Go to your StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="54b36-167">I den **enheter** bladet välj inaktiverade enheten som du vill ta bort, högerklicka och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="54b36-167">In the **Devices** blade, select the deactivated device that you wish to delete, right-click, and then click **Delete**.</span></span>

       ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. <span data-ttu-id="54b36-169">I den **ta bort** bladet, typ enhetsnamnet för att bekräfta och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="54b36-169">In the **Delete** blade, type the device name to confirm and then click **Delete**.</span></span> <span data-ttu-id="54b36-170">Borttagningen tar några minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="54b36-170">The deletion takes a few minutes to complete.</span></span>

       ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. <span data-ttu-id="54b36-172">När borttagningen är klar har du ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="54b36-172">After the deletion is successfully complete, you are notified.</span></span> <span data-ttu-id="54b36-173">Listan över enheter uppdateras också borttagningen.</span><span class="sxs-lookup"><span data-stu-id="54b36-173">The device list also updates to reflect the deletion.</span></span>

     
## <a name="deactivate-and-delete-a-cloud-appliance"></a><span data-ttu-id="54b36-174">Inaktivera och ta bort en moln-installation</span><span class="sxs-lookup"><span data-stu-id="54b36-174">Deactivate and delete a cloud appliance</span></span>

<span data-ttu-id="54b36-175">För en för StorSimple molnet, inaktivering från portalen tar bort och tar bort den virtuella datorn och de resurser som skapades när den etablerades.</span><span class="sxs-lookup"><span data-stu-id="54b36-175">For a StorSimple Cloud Appliance, deactivation from the portal deallocates and deletes the virtual machine, and the resources created when it was provisioned.</span></span> <span data-ttu-id="54b36-176">När molninstallationen har inaktiverats kan den inte återställas till sitt tidigare tillstånd.</span><span class="sxs-lookup"><span data-stu-id="54b36-176">After the cloud appliance is deactivated, it cannot be restored to its previous state.</span></span>

![Inaktivera molnet StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate7.png)

<span data-ttu-id="54b36-178">Inaktivera resulterar i följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="54b36-178">Deactivation results in the following actions:</span></span>

* <span data-ttu-id="54b36-179">StorSimple moln enheten tas bort från tjänsten.</span><span class="sxs-lookup"><span data-stu-id="54b36-179">The StorSimple Cloud Appliance is removed from the service.</span></span>
* <span data-ttu-id="54b36-180">Den virtuella datorn för StorSimple moln enheten tas bort.</span><span class="sxs-lookup"><span data-stu-id="54b36-180">The virtual machine for the StorSimple Cloud Appliance is deleted.</span></span>
* <span data-ttu-id="54b36-181">Operativsystemdisken och datadiskar som skapats för StorSimple moln enhet tas bort.</span><span class="sxs-lookup"><span data-stu-id="54b36-181">The OS disk and data disks created for the StorSimple Cloud Appliance are removed.</span></span>
* <span data-ttu-id="54b36-182">Den värdbaserade tjänsten och virtuella nätverk som skapades under etableringen bevaras.</span><span class="sxs-lookup"><span data-stu-id="54b36-182">The hosted service and Virtual Network that were created during provisioning are retained.</span></span> <span data-ttu-id="54b36-183">Om du inte använder dessa enheter, bör du ta bort dem manuellt.</span><span class="sxs-lookup"><span data-stu-id="54b36-183">If you are not using these entities, you should delete them manually.</span></span>
* <span data-ttu-id="54b36-184">Molnögonblicksbilder som skapats av StorSimple moln enhet bevaras.</span><span class="sxs-lookup"><span data-stu-id="54b36-184">Cloud snapshots created by the StorSimple Cloud Appliance are retained.</span></span>

<span data-ttu-id="54b36-185">När molnet enheten har inaktiverats kan du ta bort den från listan över enheter.</span><span class="sxs-lookup"><span data-stu-id="54b36-185">After the cloud appliance is deactivated, you can delete it from the list of devices.</span></span> <span data-ttu-id="54b36-186">Välj inaktiverad enhet, högerklicka och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="54b36-186">Select the deactivated device, right-click, and then click **Delete**.</span></span> <span data-ttu-id="54b36-187">StorSimple meddelas du när enheten har tagits bort och listan över enheter uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="54b36-187">StorSimple notifies you once the device is deleted and the list of devices updates.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54b36-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="54b36-188">Next steps</span></span>

* <span data-ttu-id="54b36-189">Om du vill återställa inaktiverade enheten till fabriksinställningarna, gå till [återställa enheten till fabriksinställningarna](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span><span class="sxs-lookup"><span data-stu-id="54b36-189">To restore the deactivated device to factory defaults, go to [Reset the device to factory default settings](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span></span>
* <span data-ttu-id="54b36-190">För teknisk support [kontaktar Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="54b36-190">For technical assistance, [contact Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span></span>
* <span data-ttu-id="54b36-191">Mer information om hur du använder tjänsten StorSimple Enhetshanteraren, gå till [använda Enhetshanteraren för StorSimple-tjänsten för att administrera din StorSimple-enhet](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="54b36-191">To learn more about how to use the StorSimple Device Manager service, go to [Use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

