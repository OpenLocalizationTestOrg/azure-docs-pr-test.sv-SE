---
title: Inaktivera och ta bort en StorSimple-enhet | Microsoft Docs
description: "Beskriver hur du tar bort StorSimple-enhet från tjänsten genom att först inaktivera det och sedan ta bort den."
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 155cda38-c5ae-45dc-b7e8-6444494afc9e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c000a642aa088ac80cc7077453b87e9a47f96900
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deactivate-and-delete-a-storsimple-8000-series-device-via-storsimple-manager-service"></a><span data-ttu-id="b08e8-103">Inaktivera och ta bort en enhet med StorSimple 8000-serien via StorSimple Manager-tjänsten</span><span class="sxs-lookup"><span data-stu-id="b08e8-103">Deactivate and delete a StorSimple 8000 series device via StorSimple Manager service</span></span>
## <a name="overview"></a><span data-ttu-id="b08e8-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="b08e8-104">Overview</span></span>
<span data-ttu-id="b08e8-105">Du kanske vill vidta en StorSimple-enhet out-of-service (till exempel om du ersätter eller uppgradera din enhet eller om du inte längre använder StorSimple).</span><span class="sxs-lookup"><span data-stu-id="b08e8-105">You may wish to take a StorSimple device out of service (for example, if you are replacing or upgrading your device or if you are no longer using StorSimple).</span></span> <span data-ttu-id="b08e8-106">Om så är fallet kommer du behöva inaktivera enheten innan du kan ta bort den.</span><span class="sxs-lookup"><span data-stu-id="b08e8-106">If this is the case, you will need to deactivate the device before you can delete it.</span></span> <span data-ttu-id="b08e8-107">Inaktivera servrarna anslutningen mellan enheten och motsvarande StorSimple Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b08e8-107">Deactivating severs the connection between the device and the corresponding StorSimple Manager service.</span></span> <span data-ttu-id="b08e8-108">Den här självstudiekursen beskrivs hur du tar bort en StorSimple-enhet från tjänsten genom att först avaktivera den och sedan ta bort den.</span><span class="sxs-lookup"><span data-stu-id="b08e8-108">This tutorial explains how to remove a StorSimple device from service by first deactivating it and then deleting it.</span></span> 

<span data-ttu-id="b08e8-109">När du inaktiverar en enhet kan kommer alla data som lagras lokalt på enheten inte längre tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="b08e8-109">When you deactivate a device, any data that was stored locally on the device will no longer be accessible.</span></span> <span data-ttu-id="b08e8-110">Endast data som är kopplade till den enhet som lagrades i molnet kan återställas.</span><span class="sxs-lookup"><span data-stu-id="b08e8-110">Only the data associated with the device that was stored in the cloud can be recovered.</span></span>  

> [!WARNING]
> <span data-ttu-id="b08e8-111">Inaktivering är en PERMANENT åtgärd och kan inte ångras.</span><span class="sxs-lookup"><span data-stu-id="b08e8-111">Deactivation is a PERMANENT operation and cannot be undone.</span></span> <span data-ttu-id="b08e8-112">En inaktiverad enhet kan inte registreras med StorSimple Manager-tjänsten om den första återställs till fabriksinställningar.</span><span class="sxs-lookup"><span data-stu-id="b08e8-112">A deactivated device cannot be registered with the StorSimple Manager service unless it is first reset to the default factory settings.</span></span> 
> 
> <span data-ttu-id="b08e8-113">Fabriksåterställa processen tar bort alla data som lagras lokalt på din enhet.</span><span class="sxs-lookup"><span data-stu-id="b08e8-113">The factory reset process deletes all the data that was stored locally on your device.</span></span> <span data-ttu-id="b08e8-114">Det är därför viktigt att ta en ögonblicksbild av alla data i molnet innan du inaktiverar en enhet.</span><span class="sxs-lookup"><span data-stu-id="b08e8-114">Therefore, it is essential that you take a cloud snapshot of all your data before you deactivate a device.</span></span> <span data-ttu-id="b08e8-115">Detta kan du återställa alla data i ett senare skede.</span><span class="sxs-lookup"><span data-stu-id="b08e8-115">This will allow you to recover all the data at a later stage.</span></span>
> 
> 

<span data-ttu-id="b08e8-116">Den här självstudiekursen beskrivs hur du:</span><span class="sxs-lookup"><span data-stu-id="b08e8-116">This tutorial explains how to:</span></span>

* <span data-ttu-id="b08e8-117">Inaktivera en enhet och ta bort data</span><span class="sxs-lookup"><span data-stu-id="b08e8-117">Deactivate a device and delete the data</span></span>
* <span data-ttu-id="b08e8-118">Inaktivera en enhet och behålla data</span><span class="sxs-lookup"><span data-stu-id="b08e8-118">Deactivate a device and retain the data</span></span>

<span data-ttu-id="b08e8-119">Här förklaras också hur inaktivera och ta bort fungerar på en virtuell StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="b08e8-119">It also explains how deactivation and deletion works on a StorSimple virtual device.</span></span>

> [!NOTE]
> <span data-ttu-id="b08e8-120">Se till att stoppa eller ta bort klienter och värdar som är beroende av enheten innan du inaktiverar en StorSimple fysisk eller virtuell enhet.</span><span class="sxs-lookup"><span data-stu-id="b08e8-120">Before you deactivate a StorSimple physical or virtual device, make sure to stop or delete clients and hosts that depend on that device.</span></span>
> 
> 

## <a name="deactivate-and-delete-data"></a><span data-ttu-id="b08e8-121">Inaktivera och ta bort data</span><span class="sxs-lookup"><span data-stu-id="b08e8-121">Deactivate and delete data</span></span>
<span data-ttu-id="b08e8-122">Slutför följande steg om du är intresserad av att ta bort enheten fullständigt och inte vill behålla data på enheten.</span><span class="sxs-lookup"><span data-stu-id="b08e8-122">If you are interested in deleting the device completely and do not want to retain the data on the device, then complete the following steps.</span></span>

#### <a name="to-deactivate-the-device-and-delete-the-data"></a><span data-ttu-id="b08e8-123">Att inaktivera enheten och ta bort data</span><span class="sxs-lookup"><span data-stu-id="b08e8-123">To deactivate the device and delete the data</span></span>
1. <span data-ttu-id="b08e8-124">Innan du inaktiverar en enhet, måste du ta bort alla volymen behållare (och volymer) som är kopplade till enheten.</span><span class="sxs-lookup"><span data-stu-id="b08e8-124">Prior to deactivating a device, you must delete all the volume containers (and the volumes) associated with the device.</span></span> <span data-ttu-id="b08e8-125">Du kan ta bort volymbehållare endast när du tar bort tillhörande säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="b08e8-125">You can delete volume containers only after you have deleted the associated backups.</span></span>
2. <span data-ttu-id="b08e8-126">Inaktivera enheten enligt följande:</span><span class="sxs-lookup"><span data-stu-id="b08e8-126">Deactivate the device as follows:</span></span>
   
   1. <span data-ttu-id="b08e8-127">På StorSimple Manager-tjänsten **enheter** markerar du den enhet som du vill inaktivera och längst ned på sidan klickar du på **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="b08e8-127">On the StorSimple Manager service **Devices** page, select the device that you wish to deactivate and, at the bottom of the page, click **Deactivate**.</span></span>
   2. <span data-ttu-id="b08e8-128">Ett bekräftelsemeddelande visas.</span><span class="sxs-lookup"><span data-stu-id="b08e8-128">A confirmation message will appear.</span></span> <span data-ttu-id="b08e8-129">Klicka på **Ja** att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="b08e8-129">Click **Yes** to continue.</span></span> <span data-ttu-id="b08e8-130">Inaktivera processen startar och ta några minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="b08e8-130">The deactivate process will start and take a few minutes to complete.</span></span>
3. <span data-ttu-id="b08e8-131">Efter inaktiveringen, kan du ta bort enheten helt.</span><span class="sxs-lookup"><span data-stu-id="b08e8-131">After deactivation, you can delete the device completely.</span></span> <span data-ttu-id="b08e8-132">Om du tar bort en enhet tas den bort från listan över enheter som är anslutna till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b08e8-132">Deleting a device removes it from the list of devices connected to the service.</span></span> <span data-ttu-id="b08e8-133">Tjänsten kan sedan inte längre hantera enheten tagits bort.</span><span class="sxs-lookup"><span data-stu-id="b08e8-133">The service can then no longer manage the deleted device.</span></span> <span data-ttu-id="b08e8-134">Använd följande steg för att ta bort enheten:</span><span class="sxs-lookup"><span data-stu-id="b08e8-134">Use the following steps to delete the device:</span></span>
   
   1. <span data-ttu-id="b08e8-135">På StorSimple Manager-tjänsten **enheter** väljer du en inaktiverad enhet som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="b08e8-135">On the StorSimple Manager service **Devices** page, select a deactivated device that you wish to delete.</span></span>
   2. <span data-ttu-id="b08e8-136">Klicka på längst ned på sidan **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="b08e8-136">On the bottom on the page, click **Delete**.</span></span>
   3. <span data-ttu-id="b08e8-137">Du uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="b08e8-137">You will be prompted for confirmation.</span></span> <span data-ttu-id="b08e8-138">Klicka på **Ja** att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="b08e8-138">Click **Yes** to continue.</span></span>
      
      <span data-ttu-id="b08e8-139">Det kan ta några minuter för enheten som ska tas bort.</span><span class="sxs-lookup"><span data-stu-id="b08e8-139">It may take a few minutes for the device to be deleted.</span></span>

## <a name="deactivate-and-retain-data"></a><span data-ttu-id="b08e8-140">Inaktivera och behålla data</span><span class="sxs-lookup"><span data-stu-id="b08e8-140">Deactivate and retain data</span></span>
<span data-ttu-id="b08e8-141">Om du är intresserad av att ta bort enheten, men du vill behålla data, gör du följande.</span><span class="sxs-lookup"><span data-stu-id="b08e8-141">If you are interested in deleting the device but want to retain the data, then complete the following steps.</span></span>

#### <a name="to-deactivate-a-device-and-retain-the-data"></a><span data-ttu-id="b08e8-142">Inaktivera en enhet och behålla data</span><span class="sxs-lookup"><span data-stu-id="b08e8-142">To deactivate a device and retain the data</span></span>
1. <span data-ttu-id="b08e8-143">Inaktivera enheten.</span><span class="sxs-lookup"><span data-stu-id="b08e8-143">Deactivate the device.</span></span> <span data-ttu-id="b08e8-144">Alla volymbehållare och ögonblicksbilder på enheten som finns kvar.</span><span class="sxs-lookup"><span data-stu-id="b08e8-144">All the volume containers and the snapshots of the device will remain.</span></span>
   
   1. <span data-ttu-id="b08e8-145">På StorSimple Manager-tjänsten **enheter** markerar du den enhet som du vill inaktivera och längst ned på sidan klickar du på **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="b08e8-145">On the StorSimple Manager service **Devices** page, select the device that you wish to deactivate and, at the bottom of the page, click **Deactivate**.</span></span>
   2. <span data-ttu-id="b08e8-146">Ett bekräftelsemeddelande visas.</span><span class="sxs-lookup"><span data-stu-id="b08e8-146">A confirmation message will appear.</span></span> <span data-ttu-id="b08e8-147">Klicka på **Ja** att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="b08e8-147">Click **Yes** to continue.</span></span> <span data-ttu-id="b08e8-148">Inaktivera processen startar och ta några minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="b08e8-148">The deactivate process will start and take a few minutes to complete.</span></span>
2. <span data-ttu-id="b08e8-149">Nu kan du växla över volymbehållarna och associerade ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="b08e8-149">You can now fail over the volume containers and the associated snapshots.</span></span> <span data-ttu-id="b08e8-150">Mer information om procedurer går du till [redundans och disaster recovery för din StorSimple-enhet](storsimple-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="b08e8-150">For procedures, go to [Failover and disaster recovery for your StorSimple device](storsimple-device-failover-disaster-recovery.md).</span></span>
3. <span data-ttu-id="b08e8-151">Efter inaktivering och växling vid fel, kan du ta bort enheten helt.</span><span class="sxs-lookup"><span data-stu-id="b08e8-151">After deactivation and failover, you can delete the device completely.</span></span> <span data-ttu-id="b08e8-152">Om du tar bort en enhet tas den bort från listan över enheter som är anslutna till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b08e8-152">Deleting a device removes it from the list of devices connected to the service.</span></span> <span data-ttu-id="b08e8-153">Tjänsten kan sedan inte längre hantera enheten tagits bort.</span><span class="sxs-lookup"><span data-stu-id="b08e8-153">The service can then no longer manage the deleted device.</span></span> <span data-ttu-id="b08e8-154">Utför följande steg för att ta bort enheten:</span><span class="sxs-lookup"><span data-stu-id="b08e8-154">Complete the following steps to delete the device:</span></span>
   
   1. <span data-ttu-id="b08e8-155">På StorSimple Manager-tjänsten **enheter** väljer du en inaktiverad enhet som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="b08e8-155">On the StorSimple Manager service **Devices** page, select a deactivated device that you wish to delete.</span></span>
   2. <span data-ttu-id="b08e8-156">Klicka på längst ned på sidan **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="b08e8-156">On the bottom on the page, click **Delete**.</span></span>
   3. <span data-ttu-id="b08e8-157">Du uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="b08e8-157">You will be prompted for confirmation.</span></span> <span data-ttu-id="b08e8-158">Klicka på **Ja** att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="b08e8-158">Click **Yes** to continue.</span></span>
      
      <span data-ttu-id="b08e8-159">Det kan ta några minuter för enheten som ska tas bort.</span><span class="sxs-lookup"><span data-stu-id="b08e8-159">It may take a few minutes for the device to be deleted.</span></span>

## <a name="deactivate-and-delete-a-virtual-device"></a><span data-ttu-id="b08e8-160">Inaktivera och ta bort en virtuell enhet</span><span class="sxs-lookup"><span data-stu-id="b08e8-160">Deactivate and delete a virtual device</span></span>
<span data-ttu-id="b08e8-161">För en virtuell StorSimple-enhet inaktiveringen tar du bort den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b08e8-161">For a StorSimple virtual device, deactivation deallocates the virtual machine.</span></span> <span data-ttu-id="b08e8-162">Du kan sedan ta bort den virtuella datorn och de resurser som skapades när den etablerades.</span><span class="sxs-lookup"><span data-stu-id="b08e8-162">You can then delete the virtual machine and the resources created when it was provisioned.</span></span> <span data-ttu-id="b08e8-163">När den virtuella enheten inaktiveras kan den inte återställas till sitt tidigare tillstånd.</span><span class="sxs-lookup"><span data-stu-id="b08e8-163">After the virtual device is deactivated, it cannot be restored to its previous state.</span></span> 

<span data-ttu-id="b08e8-164">Inaktivera resulterar i följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="b08e8-164">Deactivation results in the following actions:</span></span>

* <span data-ttu-id="b08e8-165">Den virtuella StorSimple-enheten tas bort.</span><span class="sxs-lookup"><span data-stu-id="b08e8-165">The StorSimple virtual device is removed.</span></span>
* <span data-ttu-id="b08e8-166">OSDisk och Datadiskar som skapats för den virtuella StorSimple-enheten tas bort.</span><span class="sxs-lookup"><span data-stu-id="b08e8-166">The OSDisk and Data Disks created for the StorSimple virtual device are removed.</span></span>
* <span data-ttu-id="b08e8-167">Värdbaserad tjänst och virtuella nätverk som skapades under etableringen bevaras.</span><span class="sxs-lookup"><span data-stu-id="b08e8-167">The Hosted Service and Virtual Network that were created during provisioning are retained.</span></span> <span data-ttu-id="b08e8-168">Om du inte använder dessa enheter, bör du ta bort dem manuellt.</span><span class="sxs-lookup"><span data-stu-id="b08e8-168">If you are not using these entities, you should delete them manually.</span></span>
* <span data-ttu-id="b08e8-169">Molnögonblicksbilder som skapats av den virtuella enheten StorSimple finns kvar.</span><span class="sxs-lookup"><span data-stu-id="b08e8-169">Cloud snapshots created by the StorSimple virtual device are retained.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b08e8-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b08e8-170">Next steps</span></span>
* <span data-ttu-id="b08e8-171">Om du vill återställa inaktiverade enheten till fabriksinställningarna, gå till [återställa enheten till fabriksinställningarna](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span><span class="sxs-lookup"><span data-stu-id="b08e8-171">To restore the deactivated device to factory defaults, go to [Reset the device to factory default settings](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span></span>
* <span data-ttu-id="b08e8-172">För teknisk support [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="b08e8-172">For technical assistance, [contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>
* <span data-ttu-id="b08e8-173">Mer information om hur du använder StorSimple Manager-tjänsten går du till [använda StorSimple Manager-tjänsten för att administrera din StorSimple-enhet](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="b08e8-173">To learn more about how to use the StorSimple Manager service, go to [Use the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span> 

