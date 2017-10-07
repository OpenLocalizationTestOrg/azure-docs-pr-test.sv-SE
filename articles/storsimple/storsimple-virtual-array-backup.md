---
title: "aaaMicrosoft Azure StorSimple virtuell matris säkerhetskopiering kursen | Microsoft Docs"
description: Beskriver hur tooback in virtuella StorSimple-matris delar och volymer.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: e3cdcd9e-33b1-424d-82aa-b369d934067e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7a015fd594f8f56c48fab149a2736be9dec2c24b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a><span data-ttu-id="fc38d-103">Säkerhetskopiera resurser eller volymer på din virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="fc38d-103">Back up shares or volumes on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="fc38d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="fc38d-104">Overview</span></span>

<span data-ttu-id="fc38d-105">hello virtuella StorSimple-matrisen är en hybrid cloud lokalt virtuell lagringsenhet som kan konfigureras som en filserver eller en iSCSI-server.</span><span class="sxs-lookup"><span data-stu-id="fc38d-105">hello StorSimple Virtual Array is a hybrid cloud storage on-premises virtual device that can be configured as a file server or an iSCSI server.</span></span> <span data-ttu-id="fc38d-106">hello virtuella matris kan hello användaren toocreate schemalagda och manuella säkerhetskopieringar av alla hello resurser eller volymer på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="fc38d-106">hello virtual array allows hello user toocreate scheduled and manual backups of all hello shares or volumes on hello device.</span></span> <span data-ttu-id="fc38d-107">När du har konfigurerats som en filserver, kan också objektnivååterställning.</span><span class="sxs-lookup"><span data-stu-id="fc38d-107">When configured as a file server, it also allows item-level recovery.</span></span> <span data-ttu-id="fc38d-108">Den här självstudiekursen beskrivs hur toocreate schemalagda och manuella säkerhetskopieringar och utföra objektnivååterställning toorestore borttagna filer på din virtuella matrisen.</span><span class="sxs-lookup"><span data-stu-id="fc38d-108">This tutorial describes how toocreate scheduled and manual backups and perform item-level recovery toorestore a deleted file on your virtual array.</span></span>

<span data-ttu-id="fc38d-109">Den här kursen gäller toohello StorSimple virtuell matriser endast.</span><span class="sxs-lookup"><span data-stu-id="fc38d-109">This tutorial applies toohello StorSimple Virtual Arrays only.</span></span> <span data-ttu-id="fc38d-110">Information om 8000-serien finns för[skapa en säkerhetskopia för enheten i 8000-serien](storsimple-manage-backup-policies-u2.md)</span><span class="sxs-lookup"><span data-stu-id="fc38d-110">For information on 8000 series, go too[Create a backup for 8000 series device](storsimple-manage-backup-policies-u2.md)</span></span>

## <a name="back-up-shares-and-volumes"></a><span data-ttu-id="fc38d-111">Säkerhetskopiera filresurser och volymer</span><span class="sxs-lookup"><span data-stu-id="fc38d-111">Back up shares and volumes</span></span>

<span data-ttu-id="fc38d-112">Säkerhetskopieringar ger tidpunkts-skydd, förbättrar återställningsmöjligheterna och minimera återställningstider för filresurser och volymer.</span><span class="sxs-lookup"><span data-stu-id="fc38d-112">Backups provide point-in-time protection, improve recoverability, and minimize restore times for shares and volumes.</span></span> <span data-ttu-id="fc38d-113">Du kan säkerhetskopiera en resurs eller en volym på din StorSimple-enhet på två sätt: **schemalagda** eller **manuell**.</span><span class="sxs-lookup"><span data-stu-id="fc38d-113">You can back up a share or volume on your StorSimple device in two ways: **Scheduled** or **Manual**.</span></span> <span data-ttu-id="fc38d-114">Hello metoderna beskrivs i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="fc38d-114">Each of hello methods is discussed in hello following sections.</span></span>

## <a name="change-hello-backup-start-time"></a><span data-ttu-id="fc38d-115">Ändra hello starttiden för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="fc38d-115">Change hello backup start time</span></span>

> [!NOTE]
> <span data-ttu-id="fc38d-116">Schemalagda säkerhetskopieringar skapas i den här versionen av en standardprincip som körs varje dag vid en viss tidpunkt och säkerhetskopierar alla hello resurser eller volymer på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="fc38d-116">In this release, scheduled backups are created by a default policy that runs daily at a specified time and backs up all hello shares or volumes on hello device.</span></span> <span data-ttu-id="fc38d-117">Det är inte möjligt toocreate anpassade principer för schemalagda säkerhetskopieringar just nu.</span><span class="sxs-lookup"><span data-stu-id="fc38d-117">It is not possible toocreate custom policies for scheduled backups at this time.</span></span>


<span data-ttu-id="fc38d-118">Din virtuella StorSimple-matris har en standardprincip för säkerhetskopiering som börjar med en angiven tid på dagen (22:30) och säkerhetskopierar alla hello resurser eller volymer på hello enheten en gång om dagen.</span><span class="sxs-lookup"><span data-stu-id="fc38d-118">Your StorSimple Virtual Array has a default backup policy that starts at a specified time of day (22:30) and backs up all hello shares or volumes on hello device once a day.</span></span> <span data-ttu-id="fc38d-119">Du kan ändra hello tiden på vilka hello säkerhetskopiering startar, men hello frekvens och hello Kvarhållning (som anger hello antal säkerhetskopior tooretain) inte kan ändras.</span><span class="sxs-lookup"><span data-stu-id="fc38d-119">You can change hello time at which hello backup starts, but hello frequency and hello retention (which specifies hello number of backups tooretain) cannot be changed.</span></span> <span data-ttu-id="fc38d-120">Under dessa säkerhetskopior säkerhetskopieras hello hela virtuella enheten.</span><span class="sxs-lookup"><span data-stu-id="fc38d-120">During these backups, hello entire virtual device is backed up.</span></span> <span data-ttu-id="fc38d-121">Detta kan potentiellt påverka hello prestanda hos hello enheten och påverkar hello arbetsbelastningar som har distribuerats på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="fc38d-121">This could potentially impact hello performance of hello device and affect hello workloads deployed on hello device.</span></span> <span data-ttu-id="fc38d-122">Därför rekommenderar vi att du schemalägger dessa säkerhetskopior för låg belastning.</span><span class="sxs-lookup"><span data-stu-id="fc38d-122">Therefore, we recommend that you schedule these backups for off-peak hours.</span></span>

 <span data-ttu-id="fc38d-123">toochange hello standard säkerhetskopiering starttid, utför följande steg i hello hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="fc38d-123">toochange hello default backup start time, perform hello following steps in hello [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="toochange-hello-start-time-for-hello-default-backup-policy"></a><span data-ttu-id="fc38d-124">toochange hello starttid för hello standardprincip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="fc38d-124">toochange hello start time for hello default backup policy</span></span>

1. <span data-ttu-id="fc38d-125">Gå för**enheter**.</span><span class="sxs-lookup"><span data-stu-id="fc38d-125">Go too**Devices**.</span></span> <span data-ttu-id="fc38d-126">hello lista över enheter som registrerats StorSimple Device Manager-tjänsten kommer att visas.</span><span class="sxs-lookup"><span data-stu-id="fc38d-126">hello list of devices registered with your StorSimple Device Manager service will be displayed.</span></span> 
   
    ![Navigera toodevices](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. <span data-ttu-id="fc38d-128">Välj och klicka på enheten.</span><span class="sxs-lookup"><span data-stu-id="fc38d-128">Select and click your device.</span></span> <span data-ttu-id="fc38d-129">Hej **inställningar** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="fc38d-129">hello **Settings** blade will be displayed.</span></span> <span data-ttu-id="fc38d-130">Gå för**hantera > Säkerhetskopieringsprinciper**.</span><span class="sxs-lookup"><span data-stu-id="fc38d-130">Go too**Manage > Backup policies**.</span></span>
   
    ![Välj enhet](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. <span data-ttu-id="fc38d-132">I hello **Säkerhetskopieringsprinciper** bladet hello standard starttiden är 22:30.</span><span class="sxs-lookup"><span data-stu-id="fc38d-132">In hello **Backup policies** blade, hello default start time is 22:30.</span></span> <span data-ttu-id="fc38d-133">Du kan ange hello Ny starttid för hello dagsschema i enheten tidszon.</span><span class="sxs-lookup"><span data-stu-id="fc38d-133">You can specify hello new start time for hello daily schedule in device time zone.</span></span>
   
    ![Navigera toobackup principer](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. <span data-ttu-id="fc38d-135">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="fc38d-135">Click **Save**.</span></span>

### <a name="take-a-manual-backup"></a><span data-ttu-id="fc38d-136">Gör en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="fc38d-136">Take a manual backup</span></span>

<span data-ttu-id="fc38d-137">Dessutom tooscheduled säkerhetskopior, du kan ta en manuell säkerhetskopiering (på begäran) av data på enheten när som helst.</span><span class="sxs-lookup"><span data-stu-id="fc38d-137">In addition tooscheduled backups, you can take a manual (on-demand) backup of device data at any time.</span></span>

#### <a name="toocreate-a-manual-backup"></a><span data-ttu-id="fc38d-138">toocreate en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="fc38d-138">toocreate a manual backup</span></span>

1. <span data-ttu-id="fc38d-139">Gå för**enheter**.</span><span class="sxs-lookup"><span data-stu-id="fc38d-139">Go too**Devices**.</span></span> <span data-ttu-id="fc38d-140">Välj enhet och högerklicka på **...**  på hello längst till höger i hello markerade raden.</span><span class="sxs-lookup"><span data-stu-id="fc38d-140">Select your device and right-click **...** at hello far right in hello selected row.</span></span> <span data-ttu-id="fc38d-141">Hello snabbmenyn, Välj **ta säkerhetskopia**.</span><span class="sxs-lookup"><span data-stu-id="fc38d-141">From hello context menu, select **Take backup**.</span></span>
   
    ![Navigera tootake säkerhetskopiering](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. <span data-ttu-id="fc38d-143">I hello **ta säkerhetskopia** bladet, klickar du på **ta säkerhetskopia**.</span><span class="sxs-lookup"><span data-stu-id="fc38d-143">In hello **Take backup** blade, click **Take backup**.</span></span> <span data-ttu-id="fc38d-144">Detta ska säkerhetskopiera alla hello resurser på hello filserver eller alla hello volymer på iSCSI-servern.</span><span class="sxs-lookup"><span data-stu-id="fc38d-144">This will backup all hello shares on hello file server or all hello volumes on your iSCSI server.</span></span> 
   
    ![Starta Säkerhetskopiering](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    <span data-ttu-id="fc38d-146">En säkerhetskopiering på begäran startar och du ser att ett säkerhetskopieringsjobb har startats.</span><span class="sxs-lookup"><span data-stu-id="fc38d-146">An on-demand backup starts and you see that a backup job has started.</span></span>
   
    ![Starta Säkerhetskopiering](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    <span data-ttu-id="fc38d-148">När hello jobbet har slutförts, meddelas du igen.</span><span class="sxs-lookup"><span data-stu-id="fc38d-148">Once hello job has successfully completed, you are notified again.</span></span> <span data-ttu-id="fc38d-149">hello säkerhetskopieringen startas.</span><span class="sxs-lookup"><span data-stu-id="fc38d-149">hello backup process then starts.</span></span>
   
    ![Säkerhetskopieringsjobbet har skapats](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. <span data-ttu-id="fc38d-151">tootrack hello fortskrider hello säkerhetskopieringar och titta på hello jobbinformation Klicka hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="fc38d-151">tootrack hello progress of hello backups and look at hello job details, click hello notification.</span></span> <span data-ttu-id="fc38d-152">Du kommer för **Jobbdetaljer**.</span><span class="sxs-lookup"><span data-stu-id="fc38d-152">This takes you too **Job details**.</span></span>
   
     ![information om säkerhetskopieringsjobb](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. <span data-ttu-id="fc38d-154">När hello säkerhetskopieringen är klar, går för**Management > säkerhetskopieringskatalog**.</span><span class="sxs-lookup"><span data-stu-id="fc38d-154">After hello backup is complete, go too**Management > Backup catalog**.</span></span> <span data-ttu-id="fc38d-155">En moln-ögonblicksbild av alla hello resurser (eller volymer) visas på enheten.</span><span class="sxs-lookup"><span data-stu-id="fc38d-155">You will see a cloud snapshot of all hello shares (or volumes) on your device.</span></span>
   
    ![Slutförda säkerhetskopiering](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a><span data-ttu-id="fc38d-157">Visa befintliga säkerhetskopior</span><span class="sxs-lookup"><span data-stu-id="fc38d-157">View existing backups</span></span>
<span data-ttu-id="fc38d-158">tooview hello befintliga säkerhetskopior, utföra hello följa stegen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fc38d-158">tooview hello existing backups, perform hello following steps in hello Azure portal.</span></span>

#### <a name="tooview-existing-backups"></a><span data-ttu-id="fc38d-159">tooview befintliga säkerhetskopior</span><span class="sxs-lookup"><span data-stu-id="fc38d-159">tooview existing backups</span></span>

1. <span data-ttu-id="fc38d-160">Gå för**enheter** bladet.</span><span class="sxs-lookup"><span data-stu-id="fc38d-160">Go too**Devices** blade.</span></span> <span data-ttu-id="fc38d-161">Välj och klicka på enheten.</span><span class="sxs-lookup"><span data-stu-id="fc38d-161">Select and click your device.</span></span> <span data-ttu-id="fc38d-162">I hello **inställningar** bladet går för**Management > säkerhetskopieringskatalog**.</span><span class="sxs-lookup"><span data-stu-id="fc38d-162">In hello **Settings** blade, go too**Management > Backup Catalog**.</span></span>
   
    ![Navigera toobackup katalog](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. <span data-ttu-id="fc38d-164">Ange hello följande kriterier toobe som används vid filtrering:</span><span class="sxs-lookup"><span data-stu-id="fc38d-164">Specify hello following criteria toobe used for filtering:</span></span>
   
    - <span data-ttu-id="fc38d-165">**Tidsintervallet** – kan vara **senaste 1 timme**, **föregående 24 timmar**, **senaste 7 dagarna**, **de senaste 30 dagarna**, **senaste året** , och **anpassade datum**.</span><span class="sxs-lookup"><span data-stu-id="fc38d-165">**Time range** – can be **Past 1 hour**, **Past 24 hours**, **Past 7 days**, **Past 30 days**, **Past year**, and **Custom date**.</span></span>
    
    - <span data-ttu-id="fc38d-166">**Enheter** – Välj hello listan över filservrar eller iSCSI-servrar har registrerats med din StorSimple Device Manager-tjänst.</span><span class="sxs-lookup"><span data-stu-id="fc38d-166">**Devices** – select from hello list of file servers or iSCSI servers registered with your StorSimple Device Manager service.</span></span>
   
    - <span data-ttu-id="fc38d-167">**Initierade** – kan vara automatiskt **schemalagda** (genom en princip för säkerhetskopiering) eller **manuellt** initieras (av du).</span><span class="sxs-lookup"><span data-stu-id="fc38d-167">**Initiated** – can be automatically **Scheduled** (by a backup policy) or **Manually** initiated (by you).</span></span>
   
    ![Filtrera säkerhetskopieringar](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. <span data-ttu-id="fc38d-169">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="fc38d-169">Click **Apply**.</span></span> <span data-ttu-id="fc38d-170">hello filtrerad lista över säkerhetskopieringar visas i hello **säkerhetskopieringskatalog** bladet.</span><span class="sxs-lookup"><span data-stu-id="fc38d-170">hello filtered list of backups is displayed in hello **Backup catalog** blade.</span></span> <span data-ttu-id="fc38d-171">Obs endast 100 säkerhetskopiering elementen kan visas vid en given tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="fc38d-171">Note only 100 backup elements can be displayed at a given time.</span></span>
   
    ![Uppdaterade säkerhetskopieringskatalogen](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a><span data-ttu-id="fc38d-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fc38d-173">Next steps</span></span>

<span data-ttu-id="fc38d-174">Lär dig mer om [administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="fc38d-174">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

