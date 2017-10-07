---
title: "aaaStorSimple redundans, katastrofåterställning för enheter i 8000-serien | Microsoft Docs"
description: "Lär dig hur toofail över din StorSimple-enhet toohello samma enhet."
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
ms.date: 06/23/2017
ms.author: alkohli
ms.openlocfilehash: b0b4216c7af6745ff68b85ca3d655691b43b4334
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-your-storsimple-physical-device-toosame-device"></a><span data-ttu-id="36b50-103">Växla över StorSimple-enheten fysisk enhet toosame</span><span class="sxs-lookup"><span data-stu-id="36b50-103">Fail over your StorSimple physical device toosame device</span></span>

## <a name="overview"></a><span data-ttu-id="36b50-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="36b50-104">Overview</span></span>

<span data-ttu-id="36b50-105">Den här självstudiekursen beskriver hello steg krävs toofail via en StorSimple 8000-serien fysisk enhet tooitself om en katastrof.</span><span class="sxs-lookup"><span data-stu-id="36b50-105">This tutorial describes hello steps required toofail over a StorSimple 8000 series physical device tooitself if there is a disaster.</span></span> <span data-ttu-id="36b50-106">StorSimple använder hello redundans funktionen toomigrate data på enheten från en fysisk enhet som källa i hello datacenter tooanother fysisk enhet.</span><span class="sxs-lookup"><span data-stu-id="36b50-106">StorSimple uses hello device failover feature toomigrate data from a source physical device in hello datacenter tooanother physical device.</span></span> <span data-ttu-id="36b50-107">hello anvisningarna i den här kursen gäller tooStorSimple 8000-serien fysiska enheter som kör programvaruversioner uppdatering 3 och senare.</span><span class="sxs-lookup"><span data-stu-id="36b50-107">hello guidance in this tutorial applies tooStorSimple 8000 series physical devices running software versions Update 3 and later.</span></span>

<span data-ttu-id="36b50-108">toolearn mer om enheten växling vid fel och hur den används toorecover en katastrofåterställning gå för[redundans och disaster recovery för StorSimple 8000-serien enheter](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="36b50-108">toolearn more about device failover and how it is used toorecover from a disaster, go too[Failover and disaster recovery for StorSimple 8000 series devices](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

<span data-ttu-id="36b50-109">toofail via en fysisk enhet tooanother fysisk enhet, gå för[växla över toohello samma fysiska StorSimple-enheten](storsimple-8000-device-failover-physical-device.md).</span><span class="sxs-lookup"><span data-stu-id="36b50-109">toofail over a physical device tooanother physical device, go too[Fail over toohello same StorSimple physical device](storsimple-8000-device-failover-physical-device.md).</span></span> <span data-ttu-id="36b50-110">toofail över en StorSimple fysisk enhet tooa StorSimple moln-enhet, gå för[växla över tooa StorSimple moln installation](storsimple-8000-device-failover-cloud-appliance.md).</span><span class="sxs-lookup"><span data-stu-id="36b50-110">toofail over a StorSimple physical device tooa StorSimple Cloud Appliance, go too[Fail over tooa StorSimple Cloud Appliance](storsimple-8000-device-failover-cloud-appliance.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="36b50-111">Krav</span><span class="sxs-lookup"><span data-stu-id="36b50-111">Prerequisites</span></span>

- <span data-ttu-id="36b50-112">Se till att du har granskat hello överväganden för växling vid fel för enheten.</span><span class="sxs-lookup"><span data-stu-id="36b50-112">Ensure that you have reviewed hello considerations for device failover.</span></span> <span data-ttu-id="36b50-113">Mer information finns för[vanliga överväganden för enheten redundans](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="36b50-113">For more information, go too[Common considerations for device failover](storsimple-8000-device-failover-disaster-recovery.md).</span></span>


## <a name="steps-toofail-over-toohello-same-device"></a><span data-ttu-id="36b50-114">Steg toofail över toohello samma enhet</span><span class="sxs-lookup"><span data-stu-id="36b50-114">Steps toofail over toohello same device</span></span>

<span data-ttu-id="36b50-115">Utför följande steg om du behöver toofail över toohello hello samma enhet.</span><span class="sxs-lookup"><span data-stu-id="36b50-115">Perform hello following steps if you need toofail over toohello same device.</span></span>

1. <span data-ttu-id="36b50-116">Skapa molnögonblicksbilder av alla hello volymer i enheten.</span><span class="sxs-lookup"><span data-stu-id="36b50-116">Take cloud snapshots of all hello volumes in your device.</span></span> <span data-ttu-id="36b50-117">Mer information finns för[Använd StorSimple Enhetshanteraren service toocreate säkerhetskopieringar](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="36b50-117">For more information, go too[Use StorSimple Device Manager service toocreate backups](storsimple-8000-manage-backup-policies-u2.md).</span></span>
2. <span data-ttu-id="36b50-118">Återställa din enhet toofactory standardvärden.</span><span class="sxs-lookup"><span data-stu-id="36b50-118">Reset your device toofactory defaults.</span></span> <span data-ttu-id="36b50-119">Följ hello detaljerade instruktioner i [hur tooreset toofactory en StorSimple-enheten standardinställningar](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span><span class="sxs-lookup"><span data-stu-id="36b50-119">Follow hello detailed instructions in [how tooreset a StorSimple device toofactory default settings](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span></span>
3. <span data-ttu-id="36b50-120">Gå toohello StorSimple enheten Manager-tjänsten och välj sedan **enheter**.</span><span class="sxs-lookup"><span data-stu-id="36b50-120">Go toohello StorSimple Device Manager service and then select **Devices**.</span></span> <span data-ttu-id="36b50-121">I hello **enheter** bladet hello gamla enheten ska visa **Offline**.</span><span class="sxs-lookup"><span data-stu-id="36b50-121">In hello **Devices** blade, hello old device should show as **Offline**.</span></span>

    ![Källenheten offline](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev2.png)

4. <span data-ttu-id="36b50-123">Konfigurera din enhet och registrera den igen med din StorSimple Device Manager-tjänst.</span><span class="sxs-lookup"><span data-stu-id="36b50-123">Configure your device and register it again with your StorSimple Device Manager service.</span></span> <span data-ttu-id="36b50-124">hello nyregistrerade enheten ska visa **klar tooset in**.</span><span class="sxs-lookup"><span data-stu-id="36b50-124">hello newly registered device should show as **Ready tooset up**.</span></span> <span data-ttu-id="36b50-125">hello enhetsnamnet för hello ny enhet är hello samma som hello gamla enheten men läggas till med numeriska tooindicate hello enheten var Återställ toofactory standard och registreras igen.</span><span class="sxs-lookup"><span data-stu-id="36b50-125">hello device name for hello new device is hello same as hello old device but appended with a numeral tooindicate that hello device was reset toofactory default and registered again.</span></span>

    ![Nyligen registrerade enheten redo tooset in](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev3.png)
5. <span data-ttu-id="36b50-127">Slutför hello Enhetsinställningar för nya hello-enhet.</span><span class="sxs-lookup"><span data-stu-id="36b50-127">For hello new device, complete hello device setup.</span></span> <span data-ttu-id="36b50-128">Mer information finns för[steg 4: Slutför minimala Enhetsinstallationen](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup).</span><span class="sxs-lookup"><span data-stu-id="36b50-128">For more information, go too[Step 4: Complete minimum device setup](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup).</span></span> <span data-ttu-id="36b50-129">På hello **enheter** bladet hello hello enhet ändras status för**Online**.</span><span class="sxs-lookup"><span data-stu-id="36b50-129">On hello **Devices** blade, hello status of hello device changes too**Online**.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="36b50-130">**Slutför hello minimikonfiguration först eller din DR kan misslyckas.**</span><span class="sxs-lookup"><span data-stu-id="36b50-130">**Complete hello minimum configuration first, or your DR may fail.**</span></span>

    ![Nyligen registrerade enhet online](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev7.png)

6. <span data-ttu-id="36b50-132">Välj hello gamla enhet (offline status) och från hello kommandofältet på **växla över**.</span><span class="sxs-lookup"><span data-stu-id="36b50-132">Select hello old device (status offline) and from hello command bar, click **Fail over**.</span></span> <span data-ttu-id="36b50-133">I hello **växla över** bladet välj gamla enhet som hello källa och ange hello målenhet som hello nyligen registrerad enhet.</span><span class="sxs-lookup"><span data-stu-id="36b50-133">In hello **Fail over** blade, select old device as hello source and specify hello target device as hello newly registered device.</span></span>

    ![Sammanfattning för växling vid fel](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev11.png)

    <span data-ttu-id="36b50-135">Detaljerade anvisningar finns för[växla över tooanother fysisk enhet](#fail-over-to-another-physical-device).</span><span class="sxs-lookup"><span data-stu-id="36b50-135">For detailed instructions, refer too[Fail over tooanother physical device](#fail-over-to-another-physical-device).</span></span>

7. <span data-ttu-id="36b50-136">En enhet Återställningsjobbet har skapats att du kan övervaka från hello **jobb** bladet.</span><span class="sxs-lookup"><span data-stu-id="36b50-136">A device restore job is created that you can monitor from hello **Jobs** blade.</span></span>

8. <span data-ttu-id="36b50-137">När hello jobbet har slutförts, komma åt hello ny enhet och gå toohello **volymbehållare** bladet.</span><span class="sxs-lookup"><span data-stu-id="36b50-137">After hello job has successfully completed, access hello new device and navigate toohello **Volume containers** blade.</span></span> <span data-ttu-id="36b50-138">Kontrollera att alla hello volymbehållarna från hello gamla enheten har migrerats toohello ny enhet.</span><span class="sxs-lookup"><span data-stu-id="36b50-138">Verify that all hello volume containers from hello old device have migrated toohello new device.</span></span>

   ![Volymbehållare migreras](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev13.png)

9. <span data-ttu-id="36b50-140">När hello redundansväxlingen är klar kan du inaktivera och ta bort gamla hello-enhet från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="36b50-140">After hello failover is complete, you can deactivate and delete hello old device from hello portal.</span></span> <span data-ttu-id="36b50-141">Välj hello gamla (offline), högerklicka på enheten och välj sedan **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="36b50-141">Select hello old device (offline), right-click, and then select **Deactivate**.</span></span> <span data-ttu-id="36b50-142">Hello status för hello enheten uppdateras när hello enheten inaktiveras.</span><span class="sxs-lookup"><span data-stu-id="36b50-142">After hello device is deactivated, hello status of hello device is updated.</span></span>

     ![Källenheten inaktiveras](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev14.png)

10. <span data-ttu-id="36b50-144">Välj hello inaktiveras enheten, högerklicka och välj sedan **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="36b50-144">Select hello deactivated device, right-click, and then select **Delete**.</span></span> <span data-ttu-id="36b50-145">Detta tar bort hello enheten från hello lista över enheter.</span><span class="sxs-lookup"><span data-stu-id="36b50-145">This deletes hello device from hello list of devices.</span></span>

    ![Källenheten har tagits bort](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev15.png)



## <a name="next-steps"></a><span data-ttu-id="36b50-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="36b50-147">Next steps</span></span>

* <span data-ttu-id="36b50-148">När du har utfört en växling vid fel, kanske du måste för[inaktivera eller ta bort din StorSimple-enhet](storsimple-8000-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="36b50-148">After you have performed a failover, you may need too[deactivate or delete your StorSimple device](storsimple-8000-deactivate-and-delete-device.md).</span></span>
* <span data-ttu-id="36b50-149">Information om hur toouse hello StorSimple Device Manager service, gå för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="36b50-149">For information about how toouse hello StorSimple Device Manager service, go too[Use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

