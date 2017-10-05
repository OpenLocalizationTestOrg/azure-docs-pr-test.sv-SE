---
title: "StorSimple-redundans, katastrofåterställning till ett moln StorSimple-enhet | Microsoft Docs"
description: "Lär dig mer om att växla över StorSimple 8000-serien fysiska enheten till en enhet i molnet."
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
ms.openlocfilehash: ec8bebf2854e84a37e84b45564e80fc20b63d8d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="fail-over-to-your-storsimple-cloud-appliance"></a><span data-ttu-id="dcbcc-103">Växla över till molnet för StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="dcbcc-103">Fail over to your StorSimple Cloud Appliance</span></span>

## <a name="overview"></a><span data-ttu-id="dcbcc-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="dcbcc-104">Overview</span></span>

<span data-ttu-id="dcbcc-105">Den här självstudiekursen beskriver de steg som krävs för att växla över en StorSimple 8000-serien fysisk enhet till ett moln StorSimple-enhet om en katastrof.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-105">This tutorial describes the steps required to fail over a StorSimple 8000 series physical device to a StorSimple Cloud Appliance if there is a disaster.</span></span> <span data-ttu-id="dcbcc-106">StorSimple använder funktionen med redundans i enheten för att migrera data från en fysisk enhet som källa i datacenter till ett moln-installation som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-106">StorSimple uses the device failover feature to migrate data from a source physical device in the datacenter to a cloud appliance running in Azure.</span></span> <span data-ttu-id="dcbcc-107">Riktlinjerna i den här kursen gäller StorSimple 8000-serien fysiska enheter och molnet installationer med programvaruversioner uppdatering 3 och senare.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-107">The guidance in this tutorial applies to StorSimple 8000 series physical devices and cloud appliances running software versions Update 3 and later.</span></span>

<span data-ttu-id="dcbcc-108">Mer information om enheten växling vid fel och hur den används för att återställa från en katastrof, gå till [redundans och disaster recovery för StorSimple 8000-serien enheter](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="dcbcc-108">To learn more about device failover and how it is used to recover from a disaster, go to [Failover and disaster recovery for StorSimple 8000 series devices](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

<span data-ttu-id="dcbcc-109">Om du vill växla över en fysiska StorSimple-enheten till en annan fysisk enhet, gå till [växla över till en fysisk enhet i StorSimple](storsimple-8000-device-failover-physical-device.md).</span><span class="sxs-lookup"><span data-stu-id="dcbcc-109">To fail over a StorSimple physical device to another physical device, go to [Fail over to a StorSimple physical device](storsimple-8000-device-failover-physical-device.md).</span></span> <span data-ttu-id="dcbcc-110">Om du vill växla en enhet till sig själv, gå till [växla över till samma fysiska StorSimple-enheten](storsimple-8000-device-failover-same-device.md).</span><span class="sxs-lookup"><span data-stu-id="dcbcc-110">To fail over a device to itself, go to [Fail over to the same StorSimple physical device](storsimple-8000-device-failover-same-device.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dcbcc-111">Krav</span><span class="sxs-lookup"><span data-stu-id="dcbcc-111">Prerequisites</span></span>

- <span data-ttu-id="dcbcc-112">Se till att du har granskat överväganden för växling vid fel för enheten.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-112">Ensure that you have reviewed the considerations for device failover.</span></span> <span data-ttu-id="dcbcc-113">Mer information finns på [vanliga överväganden för enheten redundans](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="dcbcc-113">For more information, go to [Common considerations for device failover](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

- <span data-ttu-id="dcbcc-114">Du måste ha en StorSimple-molnet enhet skapas och konfigureras innan du kör den här proceduren.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-114">You must have a StorSimple Cloud Appliance created and configured before running this procedure.</span></span> <span data-ttu-id="dcbcc-115">Om körs uppdaterar 3 programvaruversionen eller senare, Överväg att använda en 8020 moln anordning för DR.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-115">If running   Update 3 software version or later, consider using an 8020 cloud appliance for the DR.</span></span> <span data-ttu-id="dcbcc-116">Modellen 8020 har 64 TB och Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-116">The 8020 model has 64 TB and uses Premium Storage.</span></span> <span data-ttu-id="dcbcc-117">Mer information finns på [distribuera och hantera en StorSimple-enhet för molnet](storsimple-8000-cloud-appliance-u2.md).</span><span class="sxs-lookup"><span data-stu-id="dcbcc-117">For more information, go to [Deploy and manage a StorSimple Cloud Appliance](storsimple-8000-cloud-appliance-u2.md).</span></span>

## <a name="steps-to-fail-over-to-a-cloud-appliance"></a><span data-ttu-id="dcbcc-118">Steg för att växla över till en moln-installation</span><span class="sxs-lookup"><span data-stu-id="dcbcc-118">Steps to fail over to a cloud appliance</span></span>

<span data-ttu-id="dcbcc-119">Utför följande steg för att återställa enheten till ett mål StorSimple-enhet för molnet.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-119">Perform the following steps to restore the device to a target StorSimple Cloud Appliance.</span></span>

1.  <span data-ttu-id="dcbcc-120">Kontrollera att volymbehållaren som du vill växla över associeras molnögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-120">Verify that the volume container you want to fail over has associated cloud snapshots.</span></span> <span data-ttu-id="dcbcc-121">Mer information finns på [Använd Enhetshanteraren för StorSimple-tjänsten för att skapa säkerhetskopior](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="dcbcc-121">For more information, go to [Use StorSimple Device Manager service to create backups](storsimple-8000-manage-backup-policies-u2.md).</span></span>
2. <span data-ttu-id="dcbcc-122">Gå till StorSimple Device Manager-tjänsten och klicka på **Enheter**.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-122">Go to your StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="dcbcc-123">I den **enheter** bladet, gå till listan över enheter som är anslutna med din tjänst.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-123">In the **Devices** blade, go to the list of devices connected with your service.</span></span>
    <span data-ttu-id="dcbcc-124">![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)</span><span class="sxs-lookup"><span data-stu-id="dcbcc-124">![Select device](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)</span></span>
3. <span data-ttu-id="dcbcc-125">Välj och klicka på din källenhet.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-125">Select and click your source device.</span></span> <span data-ttu-id="dcbcc-126">På källenheten har volymbehållarna som du vill växla över.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-126">The source device has the volume containers that you want to fail over.</span></span> <span data-ttu-id="dcbcc-127">Gå till **Inställningar > Volymbehållare**.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-127">Go to **Settings > Volume Containers**.</span></span>

    ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev2.png)
    
4. <span data-ttu-id="dcbcc-129">Välj en volymbehållare som du vill växla över till en annan enhet.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-129">Select a volume container that you would like to fail over to another device.</span></span> <span data-ttu-id="dcbcc-130">Klicka på volymbehållare om du vill visa en lista över volymer i den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-130">Click the volume container to display the list of volumes within this container.</span></span> <span data-ttu-id="dcbcc-131">Välj en volym, högerklicka och klicka på **ta Offline** att kopplas ifrån volymen.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-131">Select a volume, right-click, and click **Take Offline** to take the volume offline.</span></span>

    ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev5.png)

5. <span data-ttu-id="dcbcc-133">Upprepa proceduren för alla volymer i volymbehållaren.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-133">Repeat this process for all the volumes in the volume container.</span></span>

     ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev7.png)

6. <span data-ttu-id="dcbcc-135">Upprepa det föregående steget för alla volymbehållare som du vill växla över till en annan enhet.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-135">Repeat the previous step for all the volume containers you would like to fail over to another device.</span></span>

7. <span data-ttu-id="dcbcc-136">Gå tillbaka till den **enheter** bladet.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-136">Go back to the **Devices** blade.</span></span> <span data-ttu-id="dcbcc-137">Klicka på kommandofältet **växla över**.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-137">From the command bar, click **Fail over**.</span></span>

    ![Klicka på misslyckas över](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev8.png)
8. <span data-ttu-id="dcbcc-139">I den **växla över** bladet utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="dcbcc-139">In the **Fail over** blade, perform the following steps:</span></span>
   
    1. <span data-ttu-id="dcbcc-140">Klicka på **källa**.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-140">Click **Source**.</span></span> <span data-ttu-id="dcbcc-141">Välj volymbehållare att växla över.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-141">Select the volume containers to fail over.</span></span> <span data-ttu-id="dcbcc-142">**Volymbehållare med associerade molnögonblicksbilder och offline volymer visas.**</span><span class="sxs-lookup"><span data-stu-id="dcbcc-142">**Only the volume containers with associated cloud snapshots and offline volumes are displayed.**</span></span>
        <span data-ttu-id="dcbcc-143">![Välj källa](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)</span><span class="sxs-lookup"><span data-stu-id="dcbcc-143">![Select source](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)</span></span>
    2. <span data-ttu-id="dcbcc-144">Klicka på **mål**.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-144">Click **Target**.</span></span> <span data-ttu-id="dcbcc-145">Välj ett mål moln installation från den nedrullningsbara listan över tillgängliga enheter.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-145">Select a target cloud appliance from the dropdown list of available devices.</span></span> <span data-ttu-id="dcbcc-146">**Endast de enheter som har tillräckligt med kapacitet för källa volymbehållare visas i listan.**</span><span class="sxs-lookup"><span data-stu-id="dcbcc-146">**Only the devices that have sufficient capacity to accommodate source volume containers are displayed in the list.**</span></span>

        ![Välj mål](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev12.png)

    3. <span data-ttu-id="dcbcc-148">Granska inställningarna för växling vid fel under **sammanfattning** och markera kryssrutan som anger att volymerna i valda volymbehållare är offline.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-148">Review the failover settings under **Summary** and select the checkbox indicating that the volumes in selected volume containers are offline.</span></span> 

        ![Granska inställningarna för växling vid fel](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev13.png)

9. <span data-ttu-id="dcbcc-150">Det skapas ett jobb för växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-150">A failover job is created.</span></span> <span data-ttu-id="dcbcc-151">Klicka på meddelandet för jobbet om du vill övervaka beställningsjobbet.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-151">To monitor the failover job, click the job notification.</span></span>

    ![Övervakningsjobb för växling vid fel](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

10. <span data-ttu-id="dcbcc-153">När redundansväxlingen är klar går du tillbaka till den **enheter** bladet.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-153">After the failover is completed, go back to the **Devices** blade.</span></span>

    1. <span data-ttu-id="dcbcc-154">Välj den enhet som används som mål för redundans.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-154">Select the device that was used as the target for the failover.</span></span>

       ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

    2. <span data-ttu-id="dcbcc-156">Klicka på **Volymbehållare**.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-156">Click **Volume Containers**.</span></span> <span data-ttu-id="dcbcc-157">Alla volymbehållare tillsammans med volymer från den gamla enheten ska visas.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-157">All the volume containers, along with the volumes from the old device, should be listed.</span></span>

       <span data-ttu-id="dcbcc-158">Om volymbehållaren som du redundansväxlade har lokalt Fäst volymer, har volymerna redundansväxlats som nivåindelade volymer.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-158">If the volume container that you failed over has locally pinned volumes, those volumes are failed over as tiered volumes.</span></span> <span data-ttu-id="dcbcc-159">Lokalt fästa volymer stöds inte på en StorSimple-enhet för molnet.</span><span class="sxs-lookup"><span data-stu-id="dcbcc-159">Locally pinned volumes are not supported on a StorSimple Cloud Appliance.</span></span>

       ![View target volymbehållare](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev17.png)


## <a name="next-steps"></a><span data-ttu-id="dcbcc-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dcbcc-161">Next steps</span></span>

* <span data-ttu-id="dcbcc-162">När du har utfört en växling vid fel, kan du behöva [inaktivera eller ta bort din StorSimple-enhet](storsimple-8000-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="dcbcc-162">After you have performed a failover, you may need to [deactivate or delete your StorSimple device](storsimple-8000-deactivate-and-delete-device.md).</span></span>

* <span data-ttu-id="dcbcc-163">För information om hur du använder tjänsten StorSimple Enhetshanteraren, gå till [använda Enhetshanteraren för StorSimple-tjänsten för att administrera din StorSimple-enhet](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="dcbcc-163">For information about how to use the StorSimple Device Manager service, go to [Use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

