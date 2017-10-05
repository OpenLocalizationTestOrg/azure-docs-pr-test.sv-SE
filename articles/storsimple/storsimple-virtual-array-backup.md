---
title: "Microsoft Azure StorSimple virtuell matris säkerhetskopiering kursen | Microsoft Docs"
description: "Beskriver hur du säkerhetskopierar virtuella StorSimple-matris filresurser och volymer."
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
ms.openlocfilehash: c926f0c80ce56cac3106ad97ec3ec2e18a8e2cc6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a><span data-ttu-id="e6ce6-103">Säkerhetskopiera resurser eller volymer på din virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="e6ce6-103">Back up shares or volumes on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="e6ce6-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e6ce6-104">Overview</span></span>

<span data-ttu-id="e6ce6-105">Den virtuella StorSimple-matrisen är en hybrid cloud lokalt virtuell lagringsenhet som kan konfigureras som en filserver eller en iSCSI-server.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-105">The StorSimple Virtual Array is a hybrid cloud storage on-premises virtual device that can be configured as a file server or an iSCSI server.</span></span> <span data-ttu-id="e6ce6-106">Virtuella matrisen tillåter användare att skapa schemalagda säkerhetskopieringar och manuella säkerhetskopieringar för alla resurser eller volymer på enheten.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-106">The virtual array allows the user to create scheduled and manual backups of all the shares or volumes on the device.</span></span> <span data-ttu-id="e6ce6-107">När du har konfigurerats som en filserver, kan också objektnivååterställning.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-107">When configured as a file server, it also allows item-level recovery.</span></span> <span data-ttu-id="e6ce6-108">Den här självstudiekursen beskrivs hur du skapar schemalagda säkerhetskopieringar och manuella säkerhetskopieringar och utföra återställning på objektnivå om du vill återställa en borttagen fil på din virtuella matrisen.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-108">This tutorial describes how to create scheduled and manual backups and perform item-level recovery to restore a deleted file on your virtual array.</span></span>

<span data-ttu-id="e6ce6-109">Den här kursen gäller StorSimple virtuell matriserna endast.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-109">This tutorial applies to the StorSimple Virtual Arrays only.</span></span> <span data-ttu-id="e6ce6-110">För information om 8000-serien, gå till [skapa en säkerhetskopia för enheten i 8000-serien](storsimple-manage-backup-policies-u2.md)</span><span class="sxs-lookup"><span data-stu-id="e6ce6-110">For information on 8000 series, go to [Create a backup for 8000 series device](storsimple-manage-backup-policies-u2.md)</span></span>

## <a name="back-up-shares-and-volumes"></a><span data-ttu-id="e6ce6-111">Säkerhetskopiera filresurser och volymer</span><span class="sxs-lookup"><span data-stu-id="e6ce6-111">Back up shares and volumes</span></span>

<span data-ttu-id="e6ce6-112">Säkerhetskopieringar ger tidpunkts-skydd, förbättrar återställningsmöjligheterna och minimera återställningstider för filresurser och volymer.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-112">Backups provide point-in-time protection, improve recoverability, and minimize restore times for shares and volumes.</span></span> <span data-ttu-id="e6ce6-113">Du kan säkerhetskopiera en resurs eller en volym på din StorSimple-enhet på två sätt: **schemalagda** eller **manuell**.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-113">You can back up a share or volume on your StorSimple device in two ways: **Scheduled** or **Manual**.</span></span> <span data-ttu-id="e6ce6-114">Var och en av metoderna som beskrivs i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-114">Each of the methods is discussed in the following sections.</span></span>

## <a name="change-the-backup-start-time"></a><span data-ttu-id="e6ce6-115">Ändra starttiden för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="e6ce6-115">Change the backup start time</span></span>

> [!NOTE]
> <span data-ttu-id="e6ce6-116">Schemalagda säkerhetskopieringar skapas i den här versionen av en standardprincip som körs varje dag vid en viss tidpunkt och säkerhetskopierar alla resurser eller volymer på enheten.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-116">In this release, scheduled backups are created by a default policy that runs daily at a specified time and backs up all the shares or volumes on the device.</span></span> <span data-ttu-id="e6ce6-117">Det går inte att skapa anpassade principer för schemalagda säkerhetskopieringar just nu.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-117">It is not possible to create custom policies for scheduled backups at this time.</span></span>


<span data-ttu-id="e6ce6-118">Din virtuella StorSimple-matris har en standardprincip för säkerhetskopiering som börjar med en angiven tid på dagen (22:30) och säkerhetskopierar alla resurser eller volymer på enheten en gång om dagen.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-118">Your StorSimple Virtual Array has a default backup policy that starts at a specified time of day (22:30) and backs up all the shares or volumes on the device once a day.</span></span> <span data-ttu-id="e6ce6-119">Du kan ändra den tid då säkerhetskopieringen startar, men frekvensen och kvarhållning (som anger hur många säkerhetskopior av ska behålla) inte kan ändras.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-119">You can change the time at which the backup starts, but the frequency and the retention (which specifies the number of backups to retain) cannot be changed.</span></span> <span data-ttu-id="e6ce6-120">Hela virtuella enheten har säkerhetskopierats under dessa säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-120">During these backups, the entire virtual device is backed up.</span></span> <span data-ttu-id="e6ce6-121">Detta kan potentiellt påverka prestandan för enheten och påverkar arbetsbelastningar som har distribuerats på enheten.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-121">This could potentially impact the performance of the device and affect the workloads deployed on the device.</span></span> <span data-ttu-id="e6ce6-122">Därför rekommenderar vi att du schemalägger dessa säkerhetskopior för låg belastning.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-122">Therefore, we recommend that you schedule these backups for off-peak hours.</span></span>

 <span data-ttu-id="e6ce6-123">Om du vill ändra standard säkerhetskopiering starttiden, utför följande steg i den [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e6ce6-123">To change the default backup start time, perform the following steps in the [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a><span data-ttu-id="e6ce6-124">Ändra starttiden för standardprincip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="e6ce6-124">To change the start time for the default backup policy</span></span>

1. <span data-ttu-id="e6ce6-125">Gå till **enheter**.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-125">Go to **Devices**.</span></span> <span data-ttu-id="e6ce6-126">Lista över enheter som registrerats StorSimple Device Manager-tjänsten ska visas.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-126">The list of devices registered with your StorSimple Device Manager service will be displayed.</span></span> 
   
    ![Navigera till enheter](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. <span data-ttu-id="e6ce6-128">Välj och klicka på enheten.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-128">Select and click your device.</span></span> <span data-ttu-id="e6ce6-129">Den **inställningar** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-129">The **Settings** blade will be displayed.</span></span> <span data-ttu-id="e6ce6-130">Gå till **hantera > Säkerhetskopieringsprinciper**.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-130">Go to **Manage > Backup policies**.</span></span>
   
    ![Välj enhet](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. <span data-ttu-id="e6ce6-132">I den **Säkerhetskopieringsprinciper** bladet starttiden som standard är 22:30.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-132">In the **Backup policies** blade, the default start time is 22:30.</span></span> <span data-ttu-id="e6ce6-133">Du kan ange nya starttiden för schemat i enheten tidszon.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-133">You can specify the new start time for the daily schedule in device time zone.</span></span>
   
    ![Navigera till principer för säkerhetskopiering](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. <span data-ttu-id="e6ce6-135">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-135">Click **Save**.</span></span>

### <a name="take-a-manual-backup"></a><span data-ttu-id="e6ce6-136">Gör en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="e6ce6-136">Take a manual backup</span></span>

<span data-ttu-id="e6ce6-137">Förutom schemalagda säkerhetskopieringar, kan du utföra en manuell (på begäran) säkerhetskopiering av data på enheten när som helst.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-137">In addition to scheduled backups, you can take a manual (on-demand) backup of device data at any time.</span></span>

#### <a name="to-create-a-manual-backup"></a><span data-ttu-id="e6ce6-138">Så här skapar du en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="e6ce6-138">To create a manual backup</span></span>

1. <span data-ttu-id="e6ce6-139">Gå till **enheter**.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-139">Go to **Devices**.</span></span> <span data-ttu-id="e6ce6-140">Välj enhet och högerklicka på **...**  längst till höger i den markerade raden.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-140">Select your device and right-click **...** at the far right in the selected row.</span></span> <span data-ttu-id="e6ce6-141">På snabbmenyn Välj **ta säkerhetskopia**.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-141">From the context menu, select **Take backup**.</span></span>
   
    ![Navigera för att göra säkerhetskopieringar](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. <span data-ttu-id="e6ce6-143">I den **ta säkerhetskopia** bladet, klickar du på **ta säkerhetskopia**.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-143">In the **Take backup** blade, click **Take backup**.</span></span> <span data-ttu-id="e6ce6-144">Detta ska säkerhetskopiera alla filresurser på filservern eller alla volymer på iSCSI-servern.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-144">This will backup all the shares on the file server or all the volumes on your iSCSI server.</span></span> 
   
    ![Starta Säkerhetskopiering](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    <span data-ttu-id="e6ce6-146">En säkerhetskopiering på begäran startar och du ser att ett säkerhetskopieringsjobb har startats.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-146">An on-demand backup starts and you see that a backup job has started.</span></span>
   
    ![Starta Säkerhetskopiering](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    <span data-ttu-id="e6ce6-148">När jobbet har slutförts, meddelas du igen.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-148">Once the job has successfully completed, you are notified again.</span></span> <span data-ttu-id="e6ce6-149">Säkerhetskopieringen startas.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-149">The backup process then starts.</span></span>
   
    ![Säkerhetskopieringsjobbet har skapats](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. <span data-ttu-id="e6ce6-151">Klicka på meddelandet för att följa förloppet för säkerhetskopieringar och granska Jobbinformationen.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-151">To track the progress of the backups and look at the job details, click the notification.</span></span> <span data-ttu-id="e6ce6-152">Då kommer du att **Jobbdetaljer**.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-152">This takes you to  **Job details**.</span></span>
   
     ![information om säkerhetskopieringsjobb](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. <span data-ttu-id="e6ce6-154">När säkerhetskopieringen är klar, går du till **Management > säkerhetskopieringskatalog**.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-154">After the backup is complete, go to **Management > Backup catalog**.</span></span> <span data-ttu-id="e6ce6-155">En ögonblicksbild i molnet för alla resurser (eller volymer) visas på enheten.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-155">You will see a cloud snapshot of all the shares (or volumes) on your device.</span></span>
   
    ![Slutförda säkerhetskopiering](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a><span data-ttu-id="e6ce6-157">Visa befintliga säkerhetskopior</span><span class="sxs-lookup"><span data-stu-id="e6ce6-157">View existing backups</span></span>
<span data-ttu-id="e6ce6-158">Utför följande steg om du vill visa befintliga säkerhetskopior i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-158">To view the existing backups, perform the following steps in the Azure portal.</span></span>

#### <a name="to-view-existing-backups"></a><span data-ttu-id="e6ce6-159">Visa befintliga säkerhetskopior</span><span class="sxs-lookup"><span data-stu-id="e6ce6-159">To view existing backups</span></span>

1. <span data-ttu-id="e6ce6-160">Gå till **enheter** bladet.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-160">Go to **Devices** blade.</span></span> <span data-ttu-id="e6ce6-161">Välj och klicka på enheten.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-161">Select and click your device.</span></span> <span data-ttu-id="e6ce6-162">I den **inställningar** gå till bladet **Management > säkerhetskopieringskatalog**.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-162">In the **Settings** blade, go to **Management > Backup Catalog**.</span></span>
   
    ![Gå till katalogen för säkerhetskopiering](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. <span data-ttu-id="e6ce6-164">Ange följande kriterier som ska användas för att filtrera:</span><span class="sxs-lookup"><span data-stu-id="e6ce6-164">Specify the following criteria to be used for filtering:</span></span>
   
    - <span data-ttu-id="e6ce6-165">**Tidsintervallet** – kan vara **senaste 1 timme**, **föregående 24 timmar**, **senaste 7 dagarna**, **de senaste 30 dagarna**, **senaste året** , och **anpassade datum**.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-165">**Time range** – can be **Past 1 hour**, **Past 24 hours**, **Past 7 days**, **Past 30 days**, **Past year**, and **Custom date**.</span></span>
    
    - <span data-ttu-id="e6ce6-166">**Enheter** – Välj i listan över filservrar eller iSCSI-servrar har registrerats med din StorSimple Device Manager-tjänst.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-166">**Devices** – select from the list of file servers or iSCSI servers registered with your StorSimple Device Manager service.</span></span>
   
    - <span data-ttu-id="e6ce6-167">**Initierade** – kan vara automatiskt **schemalagda** (genom en princip för säkerhetskopiering) eller **manuellt** initieras (av du).</span><span class="sxs-lookup"><span data-stu-id="e6ce6-167">**Initiated** – can be automatically **Scheduled** (by a backup policy) or **Manually** initiated (by you).</span></span>
   
    ![Filtrera säkerhetskopieringar](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. <span data-ttu-id="e6ce6-169">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-169">Click **Apply**.</span></span> <span data-ttu-id="e6ce6-170">Filtrerad lista över säkerhetskopieringar visas i den **säkerhetskopieringskatalog** bladet.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-170">The filtered list of backups is displayed in the **Backup catalog** blade.</span></span> <span data-ttu-id="e6ce6-171">Obs endast 100 säkerhetskopiering elementen kan visas vid en given tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="e6ce6-171">Note only 100 backup elements can be displayed at a given time.</span></span>
   
    ![Uppdaterade säkerhetskopieringskatalogen](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a><span data-ttu-id="e6ce6-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6ce6-173">Next steps</span></span>

<span data-ttu-id="e6ce6-174">Lär dig mer om [administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="e6ce6-174">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

