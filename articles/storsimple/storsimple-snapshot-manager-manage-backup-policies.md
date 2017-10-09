---
title: "principer för säkerhetskopiering aaaStorSimple Snapshot Manager | Microsoft Docs"
description: "Beskriver hur toouse hello StorSimple Snapshot Manager MMC snapin-modulen toocreate och hantera hello säkerhetskopiering principer som styr schemalagda säkerhetskopieringar."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 04415d0b-42f0-4737-8afa-257fb2dbe5d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: c2ae75a8d0568090add6018da18de73eb56e6590
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-toocreate-and-manage-backup-policies"></a><span data-ttu-id="c2072-103">Använda StorSimple Snapshot Manager toocreate och hantera principer för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="c2072-103">Use StorSimple Snapshot Manager toocreate and manage backup policies</span></span>
## <a name="overview"></a><span data-ttu-id="c2072-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="c2072-104">Overview</span></span>
<span data-ttu-id="c2072-105">En princip för säkerhetskopiering skapar ett schema för att säkerhetskopiera volymdata lokalt eller i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="c2072-105">A backup policy creates a schedule for backing up volume data locally or in hello cloud.</span></span> <span data-ttu-id="c2072-106">När du skapar en princip för säkerhetskopiering måste ange du också en bevarandeprincip.</span><span class="sxs-lookup"><span data-stu-id="c2072-106">When you create a backup policy, you can also specify a retention policy.</span></span> <span data-ttu-id="c2072-107">(Du kan lagra maximalt 64 ögonblicksbilder.) Mer information om principer för säkerhetskopiering finns [säkerhetskopiera typer](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) i [StorSimple 8000-serien: en hybridmolnlösningen](storsimple-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c2072-107">(You can retain a maximum of 64 snapshots.) For more information about backup policies, see [Backup types](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) in [StorSimple 8000 series: a hybrid cloud solution](storsimple-overview.md).</span></span>

<span data-ttu-id="c2072-108">Den här självstudiekursen beskrivs hur du:</span><span class="sxs-lookup"><span data-stu-id="c2072-108">This tutorial explains how to:</span></span>

* <span data-ttu-id="c2072-109">Skapa en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="c2072-109">Create a backup policy</span></span>
* <span data-ttu-id="c2072-110">Redigera en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="c2072-110">Edit a backup policy</span></span>
* <span data-ttu-id="c2072-111">Ta bort en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="c2072-111">Delete a backup policy</span></span>

## <a name="create-a-backup-policy"></a><span data-ttu-id="c2072-112">Skapa en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="c2072-112">Create a backup policy</span></span>
<span data-ttu-id="c2072-113">Använd följande procedur toocreate en ny säkerhetskopieringsprincip hello.</span><span class="sxs-lookup"><span data-stu-id="c2072-113">Use hello following procedure toocreate a new backup policy.</span></span>

#### <a name="toocreate-a-backup-policy"></a><span data-ttu-id="c2072-114">toocreate en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="c2072-114">toocreate a backup policy</span></span>
1. <span data-ttu-id="c2072-115">Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="c2072-115">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="c2072-116">I hello **omfång** fönstret högerklickar du på **Säkerhetskopieringsprinciper**, och klicka på **skapa princip för säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="c2072-116">In hello **Scope** pane, right-click **Backup Policies**, and click **Create Backup Policy**.</span></span>

    ![Skapa en princip för säkerhetskopiering](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    <span data-ttu-id="c2072-118">Hej **skapa en princip** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="c2072-118">hello **Create a Policy** dialog box appears.</span></span>

    ![Skapa en princip – fliken Allmänt](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. <span data-ttu-id="c2072-120">På hello **allmänna** fliken, fullständig hello följande information:</span><span class="sxs-lookup"><span data-stu-id="c2072-120">On hello **General** tab, complete hello following information:</span></span>

   1. <span data-ttu-id="c2072-121">I hello **namnet** skriver du ett namn för hello principen.</span><span class="sxs-lookup"><span data-stu-id="c2072-121">In hello **Name** text box, type a name for hello policy.</span></span>
   2. <span data-ttu-id="c2072-122">I hello **volym grupp** textruta, hello-typnamn för hello volym gruppen som är associerad med hello principen.</span><span class="sxs-lookup"><span data-stu-id="c2072-122">In hello **Volume group** text box, type hello name of hello volume group associated with hello policy.</span></span>
   3. <span data-ttu-id="c2072-123">Välj antingen **lokal ögonblicksbild** eller **molnet ögonblicksbild**.</span><span class="sxs-lookup"><span data-stu-id="c2072-123">Select either **Local Snapshot** or **Cloud Snapshot**.</span></span>
   4. <span data-ttu-id="c2072-124">Välj hello antalet ögonblicksbilder tooretain.</span><span class="sxs-lookup"><span data-stu-id="c2072-124">Select hello number of snapshots tooretain.</span></span> <span data-ttu-id="c2072-125">Om du väljer **alla**, 64 ögonblicksbilder ska behållas (hello maximala).</span><span class="sxs-lookup"><span data-stu-id="c2072-125">If you select **All**, 64 snapshots will be retained (hello maximum).</span></span>
4. <span data-ttu-id="c2072-126">Klicka på hello **schema** fliken.</span><span class="sxs-lookup"><span data-stu-id="c2072-126">Click hello **Schedule** tab.</span></span>

    ![Skapa en princip - fliken schema](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. <span data-ttu-id="c2072-128">På hello **schema** fliken, fullständig hello följande information:</span><span class="sxs-lookup"><span data-stu-id="c2072-128">On hello **Schedule** tab, complete hello following information:</span></span>

   1. <span data-ttu-id="c2072-129">Klicka på hello **aktivera** kryssrutan tooschedule hello nästa säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="c2072-129">Click hello **Enable** check box tooschedule hello next backup.</span></span>
   2. <span data-ttu-id="c2072-130">Under **inställningar**väljer **en gång**, **dagliga**, **veckovisa**, eller **månatliga**.</span><span class="sxs-lookup"><span data-stu-id="c2072-130">Under **Settings**, select **One time**, **Daily**, **Weekly**, or **Monthly**.</span></span>
   3. <span data-ttu-id="c2072-131">I hello **starta** textrutan, klicka hello kalendern och välj ett startdatum.</span><span class="sxs-lookup"><span data-stu-id="c2072-131">In hello **Start** text box, click hello calendar icon and select a start date.</span></span>
   4. <span data-ttu-id="c2072-132">Under **avancerade inställningar**, kan du ange valfria Upprepa scheman och ett slutdatum.</span><span class="sxs-lookup"><span data-stu-id="c2072-132">Under **Advanced Settings**, you can set optional repeat schedules and an end date.</span></span>
   5. <span data-ttu-id="c2072-133">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c2072-133">Click **OK**.</span></span>

<span data-ttu-id="c2072-134">När du har skapat en princip för säkerhetskopiering hello följande information visas i hello **resultat** fönstret:</span><span class="sxs-lookup"><span data-stu-id="c2072-134">After you create a backup policy, hello following information appears in hello **Results** pane:</span></span>

* <span data-ttu-id="c2072-135">**Namnet** – hello namn på princip för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="c2072-135">**Name** – hello name of backup policy.</span></span>
* <span data-ttu-id="c2072-136">**Typen** – lokal ögonblicksbild eller ögonblicksbild i molnet.</span><span class="sxs-lookup"><span data-stu-id="c2072-136">**Type** – local snapshot or cloud snapshot.</span></span>
* <span data-ttu-id="c2072-137">**Volymen grupp** – hello volym gruppen som är associerad med hello principen.</span><span class="sxs-lookup"><span data-stu-id="c2072-137">**Volume Group** – hello volume group associated with hello policy.</span></span>
* <span data-ttu-id="c2072-138">**Kvarhållning** – hello antalet ögonblicksbilder behålls; hello högsta 64.</span><span class="sxs-lookup"><span data-stu-id="c2072-138">**Retention** – hello number of snapshots retained; hello maximum is 64.</span></span>
* <span data-ttu-id="c2072-139">**Skapa** – hello datum som principen skapades.</span><span class="sxs-lookup"><span data-stu-id="c2072-139">**Created** – hello date that this policy was created.</span></span>
* <span data-ttu-id="c2072-140">**Aktiverad** – om hello principen är för närvarande i praktiken: **True** anger att det är aktivt; **FALSKT** anger att det inte är aktiv.</span><span class="sxs-lookup"><span data-stu-id="c2072-140">**Enabled** – whether hello policy is currently in effect: **True** indicates that it is in effect; **False** indicates that it is not in effect.</span></span>

## <a name="edit-a-backup-policy"></a><span data-ttu-id="c2072-141">Redigera en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="c2072-141">Edit a backup policy</span></span>
<span data-ttu-id="c2072-142">Använd följande procedur tooedit en säkerhetskopieringsprincip hello.</span><span class="sxs-lookup"><span data-stu-id="c2072-142">Use hello following procedure tooedit an existing backup policy.</span></span>

#### <a name="tooedit-a-backup-policy"></a><span data-ttu-id="c2072-143">tooedit en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="c2072-143">tooedit a backup policy</span></span>
1. <span data-ttu-id="c2072-144">Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="c2072-144">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="c2072-145">I hello **omfång** rutan klickar du på hello **Säkerhetskopieringsprinciper** nod.</span><span class="sxs-lookup"><span data-stu-id="c2072-145">In hello **Scope** pane, click hello **Backup Policies** node.</span></span> <span data-ttu-id="c2072-146">Alla principer för säkerhetskopiering av hello visas i hello **resultat** fönstret.</span><span class="sxs-lookup"><span data-stu-id="c2072-146">All hello backup policies appear in hello **Results** pane.</span></span>
3. <span data-ttu-id="c2072-147">Högerklicka på hello princip du vill tooedit och klicka sedan på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="c2072-147">Right-click hello policy that you want tooedit, and then click **Edit**.</span></span>

    ![Redigera en princip för säkerhetskopiering](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. <span data-ttu-id="c2072-149">När hello **skapa en princip** visas, gör ändringarna och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c2072-149">When hello **Create a Policy** window appears, enter your changes, and then click **OK**.</span></span>

## <a name="delete-a-backup-policy"></a><span data-ttu-id="c2072-150">Ta bort en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="c2072-150">Delete a backup policy</span></span>
<span data-ttu-id="c2072-151">Använd hello följa proceduren toodelete en princip för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="c2072-151">Use hello following procedure toodelete a backup policy.</span></span>

#### <a name="toodelete-a-backup-policy"></a><span data-ttu-id="c2072-152">toodelete en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="c2072-152">toodelete a backup policy</span></span>
1. <span data-ttu-id="c2072-153">Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="c2072-153">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="c2072-154">I hello **omfång** rutan klickar du på hello **Säkerhetskopieringsprinciper** nod.</span><span class="sxs-lookup"><span data-stu-id="c2072-154">In hello **Scope** pane, click hello **Backup Policies** node.</span></span> <span data-ttu-id="c2072-155">Alla principer för säkerhetskopiering av hello visas i hello **resultat** fönstret.</span><span class="sxs-lookup"><span data-stu-id="c2072-155">All hello backup policies appear in hello **Results** pane.</span></span>
3. <span data-ttu-id="c2072-156">Högerklicka på hello säkerhetskopieringsprincip som du vill toodelete och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="c2072-156">Right-click hello backup policy that you want toodelete, and then click **Delete**.</span></span>
4. <span data-ttu-id="c2072-157">När hello-meddelande visas klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="c2072-157">When hello confirmation message appears, click **Yes**.</span></span>

    ![Princip för säkerhetskopiering bekräftelse på borttagning](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a><span data-ttu-id="c2072-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c2072-159">Next steps</span></span>
* <span data-ttu-id="c2072-160">Lär dig hur för[med StorSimple Snapshot Manager tooadminister din StorSimple-lösning](storsimple-snapshot-manager-admin.md).</span><span class="sxs-lookup"><span data-stu-id="c2072-160">Learn how too[use StorSimple Snapshot Manager tooadminister your StorSimple solution](storsimple-snapshot-manager-admin.md).</span></span>
* <span data-ttu-id="c2072-161">Lär dig hur för[använda StorSimple Snapshot Manager tooview och hantera säkerhetskopieringsjobb](storsimple-snapshot-manager-manage-backup-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="c2072-161">Learn how too[use StorSimple Snapshot Manager tooview and manage backup jobs](storsimple-snapshot-manager-manage-backup-jobs.md).</span></span>
