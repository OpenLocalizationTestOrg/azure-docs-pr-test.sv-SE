---
title: "aaaDeactivate och ta bort en virtuell matris för Microsoft Azure StorSimple | Microsoft Docs"
description: "Beskriver hur tooremove StorSimple-enhet från tjänsten genom att först inaktivera det och sedan ta bort den."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: a929f5bc-03e2-4b01-b925-973db236f19f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: b1f3ddb5822d19965739777e238af19b507df984
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a><span data-ttu-id="97986-103">Inaktivera och ta bort en virtuell StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="97986-103">Deactivate and delete a StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="97986-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="97986-104">Overview</span></span>

<span data-ttu-id="97986-105">När du inaktiverar en virtuell StorSimple-matris bryta hello anslutning mellan hello enhets- och hello motsvarande StorSimple Enhetshanteraren.</span><span class="sxs-lookup"><span data-stu-id="97986-105">When you deactivate a StorSimple Virtual Array, you break hello connection between hello device and hello corresponding StorSimple Device Manager service.</span></span> <span data-ttu-id="97986-106">Den här självstudiekursen beskrivs hur du:</span><span class="sxs-lookup"><span data-stu-id="97986-106">This tutorial explains how to:</span></span>

* <span data-ttu-id="97986-107">Inaktivera en enhet</span><span class="sxs-lookup"><span data-stu-id="97986-107">Deactivate a device</span></span> 
* <span data-ttu-id="97986-108">Ta bort en inaktiverad enhet</span><span class="sxs-lookup"><span data-stu-id="97986-108">Delete a deactivated device</span></span>

<span data-ttu-id="97986-109">hello informationen i den här artikeln gäller tooStorSimple virtuella matriser.</span><span class="sxs-lookup"><span data-stu-id="97986-109">hello information in this article applies tooStorSimple Virtual Arrays only.</span></span> <span data-ttu-id="97986-110">Information i 8000-serien finns toohow för[inaktivera eller ta bort en enhet](storsimple-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="97986-110">For information on 8000 series, go toohow too[deactivate or delete a device](storsimple-deactivate-and-delete-device.md).</span></span>

## <a name="when-toodeactivate"></a><span data-ttu-id="97986-111">När toodeactivate?</span><span class="sxs-lookup"><span data-stu-id="97986-111">When toodeactivate?</span></span>

<span data-ttu-id="97986-112">Inaktivering är en PERMANENT åtgärd och kan inte ångras.</span><span class="sxs-lookup"><span data-stu-id="97986-112">Deactivation is a PERMANENT operation and cannot be undone.</span></span> <span data-ttu-id="97986-113">Du kan inte registrera en inaktiverad enhet med hello StorSimple Enhetshanteraren tjänsten igen.</span><span class="sxs-lookup"><span data-stu-id="97986-113">You cannot register a deactivated device with hello StorSimple Device Manager service again.</span></span> <span data-ttu-id="97986-114">Du kanske behöver toodeactivate och ta bort en virtuell StorSimple-matris på hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="97986-114">You may need toodeactivate and delete a StorSimple Virtual Array in hello following scenarios:</span></span>

* <span data-ttu-id="97986-115">**Planerad redundans** : enheten är online och du planerar toofail över din enhet.</span><span class="sxs-lookup"><span data-stu-id="97986-115">**Planned failover** : Your device is online and you plan toofail over your device.</span></span> <span data-ttu-id="97986-116">Om du planerar tooupgrade tooa större enhet, eventuellt toofail över din enhet.</span><span class="sxs-lookup"><span data-stu-id="97986-116">If you are planning tooupgrade tooa larger device, you may need toofail over your device.</span></span> <span data-ttu-id="97986-117">När hello data äganderätten överförs och hello redundansväxlingen är klar, tas hello källenheten bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="97986-117">After hello data ownership is transferred and hello failover is complete, hello source device is automatically deleted.</span></span>
* <span data-ttu-id="97986-118">**Oplanerad växling** : enheten är offline och du behöver toofail över hello enhet.</span><span class="sxs-lookup"><span data-stu-id="97986-118">**Unplanned failover** : Your device is offline and you need toofail over hello device.</span></span> <span data-ttu-id="97986-119">Det här scenariot kan uppstå under en katastrofåterställning när det finns ett avbrott i hello datacenter och den primära enheten är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="97986-119">This scenario may occur during a disaster when there is an outage in hello datacenter and your primary device is down.</span></span> <span data-ttu-id="97986-120">Du kan planera toofail över hello enheten tooa sekundär enhet.</span><span class="sxs-lookup"><span data-stu-id="97986-120">You plan toofail over hello device tooa secondary device.</span></span> <span data-ttu-id="97986-121">När hello data äganderätten överförs och hello redundansväxlingen är klar, tas hello källenheten bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="97986-121">After hello data ownership is transferred and hello failover is complete, hello source device is automatically deleted.</span></span>
* <span data-ttu-id="97986-122">**Inaktivera** : du vill toodecommission hello enhet.</span><span class="sxs-lookup"><span data-stu-id="97986-122">**Decommission** : You want toodecommission hello device.</span></span> <span data-ttu-id="97986-123">Detta kräver toofirst inaktivera hello enhet och ta bort den.</span><span class="sxs-lookup"><span data-stu-id="97986-123">This requires you toofirst deactivate hello device and then delete it.</span></span> <span data-ttu-id="97986-124">När du inaktiverar en enhet kan du inte längre komma åt alla data som lagras lokalt.</span><span class="sxs-lookup"><span data-stu-id="97986-124">When you deactivate a device, you can no longer access any data that is stored locally.</span></span> <span data-ttu-id="97986-125">Du kan endast åtkomst och Återställ hello data som lagras i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="97986-125">You can only access and recover hello data stored in hello cloud.</span></span> <span data-ttu-id="97986-126">Om du planerar tookeep hello enhetsdata efter inaktiveringen, bör du ta en ögonblicksbild av alla data i molnet innan du inaktiverar en enhet.</span><span class="sxs-lookup"><span data-stu-id="97986-126">If you plan tookeep hello device data after deactivation, then you should take a cloud snapshot of all your data before you deactivate a device.</span></span> <span data-ttu-id="97986-127">Den här ögonblicksbild i molnet kan du toorecover alla hello data i ett senare skede.</span><span class="sxs-lookup"><span data-stu-id="97986-127">This cloud snapshot allows you toorecover all hello data at a later stage.</span></span>

## <a name="deactivate-a-device"></a><span data-ttu-id="97986-128">Inaktivera en enhet</span><span class="sxs-lookup"><span data-stu-id="97986-128">Deactivate a device</span></span>

<span data-ttu-id="97986-129">toodeactivate enheten, utföra hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="97986-129">toodeactivate your device, perform hello following steps.</span></span>

#### <a name="toodeactivate-hello-device"></a><span data-ttu-id="97986-130">toodeactivate hello enhet</span><span class="sxs-lookup"><span data-stu-id="97986-130">toodeactivate hello device</span></span>

1. <span data-ttu-id="97986-131">Gå för i din tjänst**Management > enheter**.</span><span class="sxs-lookup"><span data-stu-id="97986-131">In your service, go too**Management > Devices**.</span></span> <span data-ttu-id="97986-132">I hello **enheter** bladet, klickar du på och väljer hello-enhet som du vill toodeactivate.</span><span class="sxs-lookup"><span data-stu-id="97986-132">In hello **Devices** blade, click and select hello device that you wish toodeactivate.</span></span>
   
    ![Välj enhet toodeactivate](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete7.png)
2. <span data-ttu-id="97986-134">I din **enheten instrumentpanelen** bladet, klickar du på **... Flera** och välj hello listan **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="97986-134">In your **Device dashboard** blade, click **… More** and from hello list, select **Deactivate**.</span></span>
   
    ![Klicka på Inaktivera](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete8.png)
3. <span data-ttu-id="97986-136">I hello **inaktivera** bladet, typen hello enhetsnamnet och klicka sedan på **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="97986-136">In hello **Deactivate** blade, type hello device name and then click **Deactivate**.</span></span> 
   
    ![Bekräfta inaktivera](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete1.png)
   
    <span data-ttu-id="97986-138">hello inaktivera startar och tar några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="97986-138">hello deactivate process starts and takes a few minutes toocomplete.</span></span>
   
    ![Inaktivera pågår](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete2.png)
4. <span data-ttu-id="97986-140">Efter inaktiveringen uppdaterar hello lista över enheter.</span><span class="sxs-lookup"><span data-stu-id="97986-140">After deactivation, hello list of devices refreshes.</span></span>
   
    ![Inaktivera klar](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete3.png)
   
    <span data-ttu-id="97986-142">Nu kan du ta bort den här enheten.</span><span class="sxs-lookup"><span data-stu-id="97986-142">You can now delete this device.</span></span>

## <a name="delete-hello-device"></a><span data-ttu-id="97986-143">Ta bort hello-enhet</span><span class="sxs-lookup"><span data-stu-id="97986-143">Delete hello device</span></span>

<span data-ttu-id="97986-144">En enhet har toobe första inaktiverade toodelete den.</span><span class="sxs-lookup"><span data-stu-id="97986-144">A device has toobe first deactivated toodelete it.</span></span> <span data-ttu-id="97986-145">Om du tar bort en enhet tas den bort från hello lista över enheter anslutna toohello.</span><span class="sxs-lookup"><span data-stu-id="97986-145">Deleting a device removes it from hello list of devices connected toohello service.</span></span> <span data-ttu-id="97986-146">hello-tjänsten kan sedan inte längre hantera hello bort enheten.</span><span class="sxs-lookup"><span data-stu-id="97986-146">hello service can then no longer manage hello deleted device.</span></span> <span data-ttu-id="97986-147">hello data som är associerade med hello enhet men finns kvar i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="97986-147">hello data associated with hello device however remains in hello cloud.</span></span> <span data-ttu-id="97986-148">Dessa data sedan påförs kostnader.</span><span class="sxs-lookup"><span data-stu-id="97986-148">This data then accrues charges.</span></span>

<span data-ttu-id="97986-149">toodelete hello enhet, utför följande steg hello.</span><span class="sxs-lookup"><span data-stu-id="97986-149">toodelete hello device, perform hello following steps.</span></span>

#### <a name="toodelete-hello-device"></a><span data-ttu-id="97986-150">toodelete hello enhet</span><span class="sxs-lookup"><span data-stu-id="97986-150">toodelete hello device</span></span>

1. <span data-ttu-id="97986-151">I din StorSimple Enhetshanteraren, gå för**Management > enheter**.</span><span class="sxs-lookup"><span data-stu-id="97986-151">In your StorSimple Device Manager, go too**Management > Devices**.</span></span> <span data-ttu-id="97986-152">I hello **enheter** bladet Välj en inaktiverad enhet som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="97986-152">In hello **Devices** blade, select a deactivated device that you wish toodelete.</span></span>
2. <span data-ttu-id="97986-153">I hello **enheten instrumentpanelen** bladet, klickar du på **... Flera** och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="97986-153">In hello **Device dashboard** blade, click **… More** and then click **Delete**.</span></span>
   
   ![Välj enhet toodelete](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete4.png)
3. <span data-ttu-id="97986-155">I hello **ta bort** bladet, ange hello namnet på din enhet tooconfirm hello borttagning och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="97986-155">In hello **Delete** blade, type hello name of your device tooconfirm hello deletion and then click **Delete**.</span></span> <span data-ttu-id="97986-156">Ta bort hello enheten tar inte bort hello molndata som är associerade med hello enhet.</span><span class="sxs-lookup"><span data-stu-id="97986-156">Deleting hello device does not delete hello cloud data associated with hello device.</span></span> 
   
   ![Bekräfta borttagning](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete5.png) 
4. <span data-ttu-id="97986-158">hello borttagning startar och tar några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="97986-158">hello deletion starts and takes a few minutes toocomplete.</span></span>
   
   ![Ta bort pågår](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete6.png)
   
    <span data-ttu-id="97986-160">När hello enheten har tagits bort kan se du hello uppdatera listan över enheter.</span><span class="sxs-lookup"><span data-stu-id="97986-160">After hello device is deleted, you can view hello updated list of devices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="97986-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="97986-161">Next steps</span></span>

* <span data-ttu-id="97986-162">Mer information om hur toofail över, gå för[redundans och återställning vid återställning av din virtuella StorSimple-matris](storsimple-virtual-array-failover-dr.md).</span><span class="sxs-lookup"><span data-stu-id="97986-162">For information on how toofail over, go too[Failover and disaster recovery of your StorSimple Virtual Array](storsimple-virtual-array-failover-dr.md).</span></span>

* <span data-ttu-id="97986-163">toolearn mer om hur toouse hello StorSimple Enhetshanteraren service gå för[Använd hello StorSimple Enhetshanteraren service tooadminister din virtuella StorSimple-matris](storsimple-virtual-array-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="97986-163">toolearn more about how toouse hello StorSimple Device Manager service, go too[Use hello StorSimple Device Manager service tooadminister your StorSimple Virtual Array](storsimple-virtual-array-manager-service-administration.md).</span></span> 

