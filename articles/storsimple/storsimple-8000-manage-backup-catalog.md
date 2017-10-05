---
title: "Hantera katalogen för säkerhetskopieringar StorSimple | Microsoft Docs"
description: "Beskriver hur du använda sidan StorSimple Enhetshanteraren säkerhetskopieringskatalogen listan, Välj och ta bort säkerhetskopior."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: ac2577c6cd350d6d437d55e61ec73d954cb24893
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-your-backup-catalog"></a><span data-ttu-id="2deb2-103">Använda Enhetshanteraren för StorSimple-tjänsten för att hantera katalogen för säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="2deb2-103">Use the StorSimple Device Manager service to manage your backup catalog</span></span>
## <a name="overview"></a><span data-ttu-id="2deb2-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="2deb2-104">Overview</span></span>
<span data-ttu-id="2deb2-105">Tjänsten StorSimple Enhetshanteraren **säkerhetskopieringskatalogen** bladet visar alla säkerhetskopior som skapas vid manuell eller schemalagda säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="2deb2-105">The StorSimple Device Manager service **Backup Catalog** blade displays all the backup sets that are created when manual or scheduled backups are taken.</span></span> <span data-ttu-id="2deb2-106">Du kan använda den här sidan för att visa alla säkerhetskopior för en princip för säkerhetskopiering eller en volym, Välj eller ta bort säkerhetskopior eller använda en säkerhetskopia att återställa eller klona en volym.</span><span class="sxs-lookup"><span data-stu-id="2deb2-106">You can use this page to list all the backups for a backup policy or a volume, select or delete backups, or use a backup to restore or clone a volume.</span></span>

<span data-ttu-id="2deb2-107">Den här självstudiekursen beskrivs hur du väljer, och ta bort en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="2deb2-107">This tutorial explains how to list, select, and delete a backup set.</span></span> <span data-ttu-id="2deb2-108">Mer information om hur du återställer din enhet från säkerhetskopia, gå till [återställa enheten från en säkerhetskopia](storsimple-8000-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="2deb2-108">To learn how to restore your device from backup, go to [Restore your device from a backup set](storsimple-8000-restore-from-backup-set-u2.md).</span></span> <span data-ttu-id="2deb2-109">Information om hur du klona en volym, gå till [klona en StorSimple-volym](storsimple-8000-clone-volume-u2.md).</span><span class="sxs-lookup"><span data-stu-id="2deb2-109">To learn how to clone a volume, go to [Clone a StorSimple volume](storsimple-8000-clone-volume-u2.md).</span></span>

![Säkerhetskopieringskatalogen](./media/storsimple-8000-manage-backup-catalog/bucatalog.png) 

<span data-ttu-id="2deb2-111">Den **säkerhetskopieringskatalogen** bladet innehåller en fråga för att begränsa säkerhetskopian uppsättning val.</span><span class="sxs-lookup"><span data-stu-id="2deb2-111">The **Backup Catalog** blade provides a query to narrow your backup set selection.</span></span> <span data-ttu-id="2deb2-112">Du kan filtrera de säkerhetskopior som hämtas, baserat på följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="2deb2-112">You can filter the backup sets that are retrieved, based on the following parameters:</span></span>

* <span data-ttu-id="2deb2-113">**Enheten** – enheten där säkerhetskopian skapades.</span><span class="sxs-lookup"><span data-stu-id="2deb2-113">**Device** – The device on which the backup set was created.</span></span>
* <span data-ttu-id="2deb2-114">**Princip för säkerhetskopiering eller volym** – den princip för säkerhetskopiering eller en volym som är associerade med den här säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="2deb2-114">**Backup policy or Volume** – The backup policy or volume associated with this backup set.</span></span>
* <span data-ttu-id="2deb2-115">**Från och till** – intervallet datum och tid när säkerhetskopian skapades.</span><span class="sxs-lookup"><span data-stu-id="2deb2-115">**From and To** – The date and time range when the backup set was created.</span></span>

<span data-ttu-id="2deb2-116">Filtrerade säkerhetskopiorna visas sedan som en tabell baserat på följande attribut:</span><span class="sxs-lookup"><span data-stu-id="2deb2-116">The filtered backup sets are then tabulated based on the following attributes:</span></span>

* <span data-ttu-id="2deb2-117">**Namnet** – namnet på den princip för säkerhetskopiering eller en volym som är associerade med säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="2deb2-117">**Name** – The name of the backup policy or volume associated with the backup set.</span></span>
* <span data-ttu-id="2deb2-118">**Storlek** – den verkliga storleken för säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="2deb2-118">**Size** – The actual size of the backup set.</span></span>
* <span data-ttu-id="2deb2-119">**Skapad den** – datum och tid då säkerhetskopieringarna skapades.</span><span class="sxs-lookup"><span data-stu-id="2deb2-119">**Created On** – The date and time when the backups were created.</span></span> 
* <span data-ttu-id="2deb2-120">**Typen** – säkerhetskopior kan lokala ögonblicksbilder eller molnbaserade ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="2deb2-120">**Type** – Backup sets can be local snapshots or cloud snapshots.</span></span> <span data-ttu-id="2deb2-121">En lokal ögonblicksbild är en säkerhetskopia av din volymdata som lagras lokalt på enheten, medan en ögonblicksbild i molnet som refererar till säkerhetskopian av volymens data som finns i molnet.</span><span class="sxs-lookup"><span data-stu-id="2deb2-121">A local snapshot is a backup of all your volume data stored locally on the device, whereas a cloud snapshot refers to the backup of volume data residing in the cloud.</span></span> <span data-ttu-id="2deb2-122">Lokala ögonblicksbilder ger snabbare åtkomst medan molnögonblicksbilder väljs för dataåterhämtning.</span><span class="sxs-lookup"><span data-stu-id="2deb2-122">Local snapshots provide faster access, whereas cloud snapshots are chosen for data resiliency.</span></span>
* <span data-ttu-id="2deb2-123">**Initierades av** – säkerhetskopieringar kan initieras automatiskt av ett schema eller manuellt av en användare.</span><span class="sxs-lookup"><span data-stu-id="2deb2-123">**Initiated By** – The backups can be initiated automatically by a schedule or manually by a user.</span></span> <span data-ttu-id="2deb2-124">Du kan använda en princip för säkerhetskopiering för att schemalägga säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="2deb2-124">You can use a backup policy to schedule backups.</span></span> <span data-ttu-id="2deb2-125">Du kan också använda den **ta säkerhetskopia** kan ta en manuell säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="2deb2-125">Alternatively, you can use the **Take backup** option to take a manual backup.</span></span>

## <a name="list-backup-sets-for-a-backup-policy"></a><span data-ttu-id="2deb2-126">Lista över säkerhetskopior för en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="2deb2-126">List backup sets for a backup policy</span></span>
<span data-ttu-id="2deb2-127">Utför följande steg om du vill visa alla säkerhetskopior för en princip för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="2deb2-127">Complete the following steps to list all the backups for a backup policy.</span></span>

#### <a name="to-list-backup-sets"></a><span data-ttu-id="2deb2-128">Att lista säkerhetskopior</span><span class="sxs-lookup"><span data-stu-id="2deb2-128">To list backup sets</span></span>
1. <span data-ttu-id="2deb2-129">Gå till Enhetshanteraren din StorSimple-tjänsten och klicka på **säkerhetskopieringskatalog**.</span><span class="sxs-lookup"><span data-stu-id="2deb2-129">Go to your StorSimple Device Manager service and click **Backup catalog**.</span></span>

2. <span data-ttu-id="2deb2-130">Filtrera valen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2deb2-130">Filter the selections as follows:</span></span>
   
   1. <span data-ttu-id="2deb2-131">Ange tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="2deb2-131">Specify the time range.</span></span>
   2. <span data-ttu-id="2deb2-132">Välj lämplig enhet.</span><span class="sxs-lookup"><span data-stu-id="2deb2-132">Select the appropriate device.</span></span>
   3. <span data-ttu-id="2deb2-133">Filtrera efter **säkerhetskopiera princip** att visa motsvarande säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="2deb2-133">Filter by **Backup policy** to view the corresponding the backups.</span></span>
   3. <span data-ttu-id="2deb2-134">I listrutan säkerhetskopieringsprincip, Välj **alla** visa alla säkerhetskopior på den valda enheten.</span><span class="sxs-lookup"><span data-stu-id="2deb2-134">From the backup policy dropdown list, choose **All** to view all the backups on the selected device.</span></span>
   4. <span data-ttu-id="2deb2-135">Klicka på **tillämpa** att köra frågan.</span><span class="sxs-lookup"><span data-stu-id="2deb2-135">Click **Apply** to execute this query.</span></span>
      
      <span data-ttu-id="2deb2-136">De säkerhetskopior som är associerade med den valda säkerhetskopieringsprincipen ska visas i listan över säkerhetskopieringsuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="2deb2-136">The backups associated with the selected backup policy should appear in the list of backup sets.</span></span>

      ![Gå till katalogen för säkerhetskopiering](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

## <a name="select-a-backup-set"></a><span data-ttu-id="2deb2-138">Välj en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="2deb2-138">Select a backup set</span></span>
<span data-ttu-id="2deb2-139">Utför följande steg för att välja en säkerhetskopia för en volym eller en princip för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="2deb2-139">Complete the following steps to select a backup set for a volume or backup policy.</span></span>

#### <a name="to-select-a-backup-set"></a><span data-ttu-id="2deb2-140">Att välja en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="2deb2-140">To select a backup set</span></span>
1. <span data-ttu-id="2deb2-141">Gå till Enhetshanteraren din StorSimple-tjänsten och klicka på **säkerhetskopieringskatalog**.</span><span class="sxs-lookup"><span data-stu-id="2deb2-141">Go to your StorSimple Device Manager service and click **Backup catalog**.</span></span>
2. <span data-ttu-id="2deb2-142">Filtrera valen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2deb2-142">Filter the selections as follows:</span></span>
   
   1. <span data-ttu-id="2deb2-143">Ange tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="2deb2-143">Specify the time range.</span></span> 
   2. <span data-ttu-id="2deb2-144">Välj lämplig enhet.</span><span class="sxs-lookup"><span data-stu-id="2deb2-144">Select the appropriate device.</span></span> 
   3. <span data-ttu-id="2deb2-145">Filtrera efter volym eller säkerhetskopiering princip för säkerhetskopiering som du vill välja.</span><span class="sxs-lookup"><span data-stu-id="2deb2-145">Filter by volume or backup policy for the backup that you wish to select.</span></span>
   4. <span data-ttu-id="2deb2-146">Klicka på **tillämpa** att köra frågan.</span><span class="sxs-lookup"><span data-stu-id="2deb2-146">Click **Apply** to execute this query.</span></span>
      
      <span data-ttu-id="2deb2-147">Säkerhetskopiorna som är associerade med den valda volymen eller princip för säkerhetskopiering ska visas i listan över säkerhetskopieringsuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="2deb2-147">The backups associated with the selected volume or backup policy should appear in the list of backup sets.</span></span>

      ![Gå till katalogen för säkerhetskopiering](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. <span data-ttu-id="2deb2-149">Markera och utöka en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="2deb2-149">Select and expand a backup set.</span></span> <span data-ttu-id="2deb2-150">Du kan nu se de säkerhetskopior som är fördelade på de volymer som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="2deb2-150">You can now see the backup sets broken down by the volumes that it contains.</span></span> <span data-ttu-id="2deb2-151">Den **återställa** och **ta bort** alternativ är tillgängliga via snabbmenyn (högerklicka) för säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="2deb2-151">The **Restore** and **Delete** options are available via the context menu (right-click) for the backup set.</span></span> <span data-ttu-id="2deb2-152">Du kan utföra någon av dessa åtgärder på den säkerhetskopia som du har valt.</span><span class="sxs-lookup"><span data-stu-id="2deb2-152">You can perform either of these actions on the backup set that you selected.</span></span>

    ![Gå till katalogen för säkerhetskopiering](./media/storsimple-8000-manage-backup-catalog/bucatalog2.png)

## <a name="delete-a-backup-set"></a><span data-ttu-id="2deb2-154">Ta bort en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="2deb2-154">Delete a backup set</span></span>
<span data-ttu-id="2deb2-155">Ta bort en säkerhetskopiering när du inte längre vill behålla data som är associerade med den.</span><span class="sxs-lookup"><span data-stu-id="2deb2-155">Delete a backup when you no longer wish to retain the data associated with it.</span></span> <span data-ttu-id="2deb2-156">Utför följande steg för att ta bort en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="2deb2-156">Perform the following steps to delete a backup set.</span></span>

#### <a name="to-delete-a-backup-set"></a><span data-ttu-id="2deb2-157">Ta bort en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="2deb2-157">To delete a backup set</span></span>
 <span data-ttu-id="2deb2-158">Gå till Enhetshanteraren din StorSimple-tjänsten och klicka på **säkerhetskopieringskatalog**.</span><span class="sxs-lookup"><span data-stu-id="2deb2-158">Go to your StorSimple Device Manager service and click **Backup catalog**.</span></span>
2. <span data-ttu-id="2deb2-159">Filtrera valen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2deb2-159">Filter the selections as follows:</span></span>
   
   1. <span data-ttu-id="2deb2-160">Ange tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="2deb2-160">Specify the time range.</span></span> 
   2. <span data-ttu-id="2deb2-161">Välj lämplig enhet.</span><span class="sxs-lookup"><span data-stu-id="2deb2-161">Select the appropriate device.</span></span> 
   3. <span data-ttu-id="2deb2-162">Filtrera efter volym eller säkerhetskopiering princip för säkerhetskopiering som du vill välja.</span><span class="sxs-lookup"><span data-stu-id="2deb2-162">Filter by volume or backup policy for the backup that you wish to select.</span></span>
   4. <span data-ttu-id="2deb2-163">Klicka på **tillämpa** att köra frågan.</span><span class="sxs-lookup"><span data-stu-id="2deb2-163">Click **Apply** to execute this query.</span></span>
      
      <span data-ttu-id="2deb2-164">Säkerhetskopiorna som är associerade med den valda volymen eller princip för säkerhetskopiering ska visas i listan över säkerhetskopieringsuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="2deb2-164">The backups associated with the selected volume or backup policy should appear in the list of backup sets.</span></span>

      ![Gå till katalogen för säkerhetskopiering](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. <span data-ttu-id="2deb2-166">Markera och utöka en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="2deb2-166">Select and expand a backup set.</span></span> <span data-ttu-id="2deb2-167">Du kan nu se de säkerhetskopior som är fördelade på de volymer som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="2deb2-167">You can now see the backup sets broken down by the volumes that it contains.</span></span> <span data-ttu-id="2deb2-168">Den **återställa** och **ta bort** alternativ är tillgängliga via snabbmenyn (högerklicka) för säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="2deb2-168">The **Restore** and **Delete** options are available via the context menu (right-click) for the backup set.</span></span> <span data-ttu-id="2deb2-169">Högerklicka på den valda säkerhetskopian och på snabbmenyn Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="2deb2-169">Right-click the selected backup set and from the context menu, select **Delete**.</span></span>

    ![Gå till katalogen för säkerhetskopiering](./media/storsimple-8000-manage-backup-catalog/bucatalog3.png)

4. <span data-ttu-id="2deb2-171">När du uppmanas att bekräfta, granska informationen som visas och klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="2deb2-171">When prompted for confirmation, review the displayed information and click **Delete**.</span></span> <span data-ttu-id="2deb2-172">Den valda säkerhetskopian tas bort permanent.</span><span class="sxs-lookup"><span data-stu-id="2deb2-172">The selected backup is deleted permanently.</span></span>

    ![Gå till katalogen för säkerhetskopiering](./media/storsimple-8000-manage-backup-catalog/bucatalog4.png)  

5. <span data-ttu-id="2deb2-174">Du meddelas när borttagningen är pågående och när den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2deb2-174">You will be notified when the deletion is in progress and when it has successfully finished.</span></span> <span data-ttu-id="2deb2-175">Uppdatera frågan på den här sidan när borttagningen är klar.</span><span class="sxs-lookup"><span data-stu-id="2deb2-175">After the deletion is done, refresh the query on this page.</span></span> <span data-ttu-id="2deb2-176">Borttagna säkerhetskopian visas inte längre i listan över säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="2deb2-176">The deleted backup set will no longer appear in the list of backup sets.</span></span>

    ![Gå till katalogen för säkerhetskopiering](./media/storsimple-8000-manage-backup-catalog/bucatalog7.png)

## <a name="next-steps"></a><span data-ttu-id="2deb2-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2deb2-178">Next steps</span></span>
* <span data-ttu-id="2deb2-179">Lär dig hur du [använder Säkerhetskopiering katalogen för att återställa enheten från en säkerhetskopia](storsimple-8000-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="2deb2-179">Learn how to [use the backup catalog to restore your device from a backup set](storsimple-8000-restore-from-backup-set-u2.md).</span></span>
* <span data-ttu-id="2deb2-180">Lär dig hur du [använda Enhetshanteraren för StorSimple-tjänsten för att administrera din StorSimple-enhet](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="2deb2-180">Learn how to [use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

