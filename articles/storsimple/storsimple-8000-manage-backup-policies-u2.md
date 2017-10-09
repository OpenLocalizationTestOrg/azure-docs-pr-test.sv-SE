---
title: "aaaManage StorSimple 8000-serien säkerhetskopieringsprinciper | Microsoft Docs"
description: "Beskriver hur du kan använda hello StorSimple Enhetshanteraren service toocreate och hantera manuella säkerhetskopieringar, scheman för säkerhetskopiering och lagring av säkerhetskopior på en enhet för StorSimple 8000-serien."
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
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 7c56365abb6ba69d02008829ca6ae703d4632705
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-in-azure-portal-toomanage-backup-policies"></a><span data-ttu-id="cff2f-103">Använd hello StorSimple enheten Manager-tjänsten i Azure portal toomanage principer för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="cff2f-103">Use hello StorSimple Device Manager service in Azure portal toomanage backup policies</span></span>


## <a name="overview"></a><span data-ttu-id="cff2f-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="cff2f-104">Overview</span></span>

<span data-ttu-id="cff2f-105">Den här självstudiekursen beskrivs hur toouse hello StorSimple Enhetshanteraren service **säkerhetskopiera princip** bladet toocontrol säkerhetskopiering processer och lagring av säkerhetskopior för din StorSimple-volymer.</span><span class="sxs-lookup"><span data-stu-id="cff2f-105">This tutorial explains how toouse hello StorSimple Device Manager service **Backup policy** blade toocontrol backup processes and backup retention for your StorSimple volumes.</span></span> <span data-ttu-id="cff2f-106">Här beskrivs också hur toocomplete en manuell säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="cff2f-106">It also describes how toocomplete a manual backup.</span></span>

<span data-ttu-id="cff2f-107">När du säkerhetskopierar en volym kan du välja toocreate lokal ögonblicksbild eller en ögonblicksbild i molnet.</span><span class="sxs-lookup"><span data-stu-id="cff2f-107">When you back up a volume, you can choose toocreate a local snapshot or a cloud snapshot.</span></span> <span data-ttu-id="cff2f-108">Om du säkerhetskopierar en lokalt Fäst volym rekommenderar vi att du anger en ögonblicksbild i molnet.</span><span class="sxs-lookup"><span data-stu-id="cff2f-108">If you are backing up a locally pinned volume, we recommend that you specify a cloud snapshot.</span></span> <span data-ttu-id="cff2f-109">Med ett stort antal lokala ögonblicksbilder av en lokalt Fäst volym tillsammans med en datauppsättning som har mycket omsättning resulterar i en situation där du snabbt kan köra out-of-lokalt utrymme.</span><span class="sxs-lookup"><span data-stu-id="cff2f-109">Taking a large number of local snapshots of a locally pinned volume coupled with a data set that has a lot of churn will result in a situation in which you could rapidly run out of local space.</span></span> <span data-ttu-id="cff2f-110">Om du väljer tootake lokala ögonblicksbilder, rekommenderar vi att du tar färre dagliga ögonblicksbilder tooback hello senaste tillstånd behålla dem i en dag och ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="cff2f-110">If you choose tootake local snapshots, we recommend that you take fewer daily snapshots tooback up hello most recent state, retain them for a day, and then delete them.</span></span>

<span data-ttu-id="cff2f-111">När du tar en ögonblicksbild i molnet för en lokalt Fäst volym måste kopiera du endast hello ändras data toohello moln, där det är deduplicerad och komprimeras.</span><span class="sxs-lookup"><span data-stu-id="cff2f-111">When you take a cloud snapshot of a locally pinned volume, you copy only hello changed data toohello cloud, where it is deduplicated and compressed.</span></span>

## <a name="hello-backup-policy-blade"></a><span data-ttu-id="cff2f-112">Hej principbladet för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="cff2f-112">hello Backup policy blade</span></span>

<span data-ttu-id="cff2f-113">Hej **säkerhetskopiera princip** bladet för din StorSimple-enhet kan du toomanage principer för säkerhetskopiering och schema för lokala och molnögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="cff2f-113">hello **Backup policy** blade for your StorSimple device allows you toomanage backup policies and schedule local and cloud snapshots.</span></span> <span data-ttu-id="cff2f-114">Principer för säkerhetskopiering är används tooconfigure scheman för säkerhetskopiering och lagring av säkerhetskopior för en samling av volymer.</span><span class="sxs-lookup"><span data-stu-id="cff2f-114">Backup policies are used tooconfigure backup schedules and backup retention for a collection of volumes.</span></span> <span data-ttu-id="cff2f-115">Principer för säkerhetskopiering kan du tootake en ögonblicksbild av flera volymer samtidigt.</span><span class="sxs-lookup"><span data-stu-id="cff2f-115">Backup policies enable you tootake a snapshot of multiple volumes simultaneously.</span></span> <span data-ttu-id="cff2f-116">Det innebär att hello säkerhetskopior som har skapats av en princip för säkerhetskopiering är kraschkonsekvent kopior.</span><span class="sxs-lookup"><span data-stu-id="cff2f-116">This means that hello backups created by a backup policy will be crash-consistent copies.</span></span>

<span data-ttu-id="cff2f-117">hello säkerhetskopieringsprinciper tabular lista kan du toofilter hello befintliga principer för säkerhetskopiering av en eller flera av hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="cff2f-117">hello backup policies tabular listing also allows you toofilter hello existing backup policies by one or more of hello following fields:</span></span>

* <span data-ttu-id="cff2f-118">**Principnamn** – hello namnet som associeras med hello princip.</span><span class="sxs-lookup"><span data-stu-id="cff2f-118">**Policy name** – hello name associated with hello policy.</span></span> <span data-ttu-id="cff2f-119">hello olika typer av principer är:</span><span class="sxs-lookup"><span data-stu-id="cff2f-119">hello different types of policies include:</span></span>

  * <span data-ttu-id="cff2f-120">Schemalagda principer som skapas av hello användaren explicit.</span><span class="sxs-lookup"><span data-stu-id="cff2f-120">Scheduled policies, which are explicitly created by hello user.</span></span>
  * <span data-ttu-id="cff2f-121">Importera principer som ursprungligen skapades i hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="cff2f-121">Imported policies, which were originally created in hello StorSimple Snapshot Manager.</span></span> <span data-ttu-id="cff2f-122">Dessa har en tagg som beskriver hello StorSimple Snapshot Manager värden som hello principer har importerats från.</span><span class="sxs-lookup"><span data-stu-id="cff2f-122">These have a tag that describes hello StorSimple Snapshot Manager host that hello policies were imported from.</span></span>

  > [!NOTE]
  > <span data-ttu-id="cff2f-123">Automatisk eller standard principer för säkerhetskopiering är inte längre aktiverat när hello volymer skapas.</span><span class="sxs-lookup"><span data-stu-id="cff2f-123">Automatic or default backup policies are no longer enabled at hello time of volume creation.</span></span>

* <span data-ttu-id="cff2f-124">**Senaste lyckade säkerhetskopiering** – hello datum och tid för hello senaste lyckad säkerhetskopia som togs med den här principen.</span><span class="sxs-lookup"><span data-stu-id="cff2f-124">**Last successful backup** – hello date and time of hello last successful backup that was taken with this policy.</span></span>

* <span data-ttu-id="cff2f-125">**Nästa säkerhetskopiering** – hello datum och tid för hello nästa schemalagda säkerhetskopieringen som initieras av den här principen.</span><span class="sxs-lookup"><span data-stu-id="cff2f-125">**Next backup** – hello date and time of hello next scheduled backup that will be initiated by this policy.</span></span>

* <span data-ttu-id="cff2f-126">**Volymer** – hello volymer som är associerade med hello principen.</span><span class="sxs-lookup"><span data-stu-id="cff2f-126">**Volumes** – hello volumes associated with hello policy.</span></span> <span data-ttu-id="cff2f-127">Alla hello volymer som är associerade med en princip för säkerhetskopiering är grupperade när säkerhetskopieringar skapas.</span><span class="sxs-lookup"><span data-stu-id="cff2f-127">All hello volumes associated with a backup policy are grouped together when backups are created.</span></span>

* <span data-ttu-id="cff2f-128">**Scheman** – hello antalet scheman som är associerade med hello säkerhetskopieringsprincip.</span><span class="sxs-lookup"><span data-stu-id="cff2f-128">**Schedules** – hello number of schedules associated with hello backup policy.</span></span>

<span data-ttu-id="cff2f-129">hello vanliga åtgärder som du kan utföra för principer för säkerhetskopiering är:</span><span class="sxs-lookup"><span data-stu-id="cff2f-129">hello frequently used operations that you can perform for backup policies are:</span></span>

* <span data-ttu-id="cff2f-130">Lägga till en säkerhetskopieringspolicy</span><span class="sxs-lookup"><span data-stu-id="cff2f-130">Add a backup policy</span></span>
* <span data-ttu-id="cff2f-131">Lägg till eller ändra ett schema</span><span class="sxs-lookup"><span data-stu-id="cff2f-131">Add or modify a schedule</span></span>
* <span data-ttu-id="cff2f-132">Lägg till eller ta bort en volym</span><span class="sxs-lookup"><span data-stu-id="cff2f-132">Add or remove a volume</span></span>
* <span data-ttu-id="cff2f-133">Ta bort en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="cff2f-133">Delete a backup policy</span></span>
* <span data-ttu-id="cff2f-134">Gör en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="cff2f-134">Take a manual backup</span></span>

## <a name="add-a-backup-policy"></a><span data-ttu-id="cff2f-135">Lägga till en säkerhetskopieringspolicy</span><span class="sxs-lookup"><span data-stu-id="cff2f-135">Add a backup policy</span></span>

<span data-ttu-id="cff2f-136">Lägg till en princip för säkerhetskopiering tooautomatically schema säkerhetskopiorna.</span><span class="sxs-lookup"><span data-stu-id="cff2f-136">Add a backup policy tooautomatically schedule your backups.</span></span> <span data-ttu-id="cff2f-137">När du skapar en volym, det finns ingen standardprincip för säkerhetskopiering som är kopplad till volymen.</span><span class="sxs-lookup"><span data-stu-id="cff2f-137">When you first create a volume, there is no default backup policy associated with your volume.</span></span> <span data-ttu-id="cff2f-138">Du behöver tooadd och tilldela en säkerhetskopieringsprincip tooprotect volymdata.</span><span class="sxs-lookup"><span data-stu-id="cff2f-138">You need tooadd and assign a backup policy tooprotect volume data.</span></span>

<span data-ttu-id="cff2f-139">Utföra hello följa stegen i hello Azure portal tooadd en säkerhetskopieringsprincip för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="cff2f-139">Perform hello following steps in hello Azure portal tooadd a backup policy for your StorSimple device.</span></span> <span data-ttu-id="cff2f-140">När du har lagt till hello princip kan du definiera ett schema (se [Lägg till eller ändra ett schema](#add-or-modify-a-schedule)).</span><span class="sxs-lookup"><span data-stu-id="cff2f-140">After you add hello policy, you can define a schedule (see [Add or modify a schedule](#add-or-modify-a-schedule)).</span></span>

[!INCLUDE [storsimple-8000-add-backup-policy-u2](../../includes/storsimple-8000-add-backup-policy-u2.md)]

## <a name="add-or-modify-a-schedule"></a><span data-ttu-id="cff2f-141">Lägg till eller ändra ett schema</span><span class="sxs-lookup"><span data-stu-id="cff2f-141">Add or modify a schedule</span></span>

<span data-ttu-id="cff2f-142">Du kan lägga till eller ändra ett schema som är bifogade tooan säkerhetskopieringsprincip på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="cff2f-142">You can add or modify a schedule that is attached tooan existing backup policy on your StorSimple device.</span></span> <span data-ttu-id="cff2f-143">Utför följande steg i hello Azure portal tooadd hello eller ändra ett schema.</span><span class="sxs-lookup"><span data-stu-id="cff2f-143">Perform hello following steps in hello Azure portal tooadd or modify a schedule.</span></span>

[!INCLUDE [storsimple-8000-add-modify-backup-schedule](../../includes/storsimple-8000-add-modify-backup-schedule-u2.md)]


## <a name="add-or-remove-a-volume"></a><span data-ttu-id="cff2f-144">Lägg till eller ta bort en volym</span><span class="sxs-lookup"><span data-stu-id="cff2f-144">Add or remove a volume</span></span>

<span data-ttu-id="cff2f-145">Du kan lägga till eller ta bort en volym som tilldelats tooa princip för säkerhetskopiering på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="cff2f-145">You can add or remove a volume assigned tooa backup policy on your StorSimple device.</span></span> <span data-ttu-id="cff2f-146">Utför följande steg i hello Azure portal tooadd hello eller ta bort en volym.</span><span class="sxs-lookup"><span data-stu-id="cff2f-146">Perform hello following steps in hello Azure portal tooadd or remove a volume.</span></span>

[!INCLUDE [storsimple-8000-add-volume-backup-policy-u2](../../includes/storsimple-8000-add-remove-volume-backup-policy-u2.md)]


## <a name="delete-a-backup-policy"></a><span data-ttu-id="cff2f-147">Ta bort en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="cff2f-147">Delete a backup policy</span></span>

<span data-ttu-id="cff2f-148">Utföra hello följa stegen i hello Azure portal toodelete en princip för säkerhetskopiering på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="cff2f-148">Perform hello following steps in hello Azure portal toodelete a backup policy on your StorSimple device.</span></span>

[!INCLUDE [storsimple-8000-delete-backup-policy](../../includes/storsimple-8000-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a><span data-ttu-id="cff2f-149">Gör en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="cff2f-149">Take a manual backup</span></span>

<span data-ttu-id="cff2f-150">Utföra hello följa stegen i hello Azure portal toocreate en på-begäran (manuell) säkerhetskopiering för en enskild volym.</span><span class="sxs-lookup"><span data-stu-id="cff2f-150">Perform hello following steps in hello Azure portal toocreate an on-demand (manual) backup for a single volume.</span></span>

[!INCLUDE [storsimple-8000-create-manual-backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a><span data-ttu-id="cff2f-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cff2f-151">Next steps</span></span>

<span data-ttu-id="cff2f-152">Lär dig mer om [med hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="cff2f-152">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

