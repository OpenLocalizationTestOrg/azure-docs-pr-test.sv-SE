---
title: "aaaManage din StorSimple-säkerhetskopieringsprinciper | Microsoft Docs"
description: "Beskriver hur du kan använda hello StorSimple Manager-tjänsten toocreate och hantera manuella säkerhetskopieringar, scheman för säkerhetskopiering och lagring av säkerhetskopior.."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 4a2db707-bbfc-425c-bfeb-bc5c97781873
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2016
ms.author: v-sharos
ms.openlocfilehash: 7b01f29a8d8a096d9890c8406557021317b9baff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-backup-policies-update-2"></a><span data-ttu-id="2955c-103">Använd hello StorSimple Manager-tjänsten toomanage principer för säkerhetskopiering (uppdatering 2)</span><span class="sxs-lookup"><span data-stu-id="2955c-103">Use hello StorSimple Manager service toomanage backup policies (Update 2)</span></span>
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a><span data-ttu-id="2955c-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="2955c-104">Overview</span></span>
<span data-ttu-id="2955c-105">Den här självstudiekursen beskrivs hur toouse hello StorSimple Manager-tjänsten **Säkerhetskopieringsprinciper** toocontrol processer för säkerhetskopiering och lagring av säkerhetskopior för din StorSimple-volymer.</span><span class="sxs-lookup"><span data-stu-id="2955c-105">This tutorial explains how toouse hello StorSimple Manager service **Backup Policies** page toocontrol backup processes and backup retention for your StorSimple volumes.</span></span> <span data-ttu-id="2955c-106">Här beskrivs också hur toocomplete en manuell säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="2955c-106">It also describes how toocomplete a manual backup.</span></span>

<span data-ttu-id="2955c-107">När du säkerhetskopierar en volym kan du välja toocreate lokal ögonblicksbild eller en ögonblicksbild i molnet.</span><span class="sxs-lookup"><span data-stu-id="2955c-107">When you back up a volume, you can choose toocreate a local snapshot or a cloud snapshot.</span></span> <span data-ttu-id="2955c-108">Om du säkerhetskopierar en lokalt Fäst volym rekommenderar vi att du anger en ögonblicksbild i molnet.</span><span class="sxs-lookup"><span data-stu-id="2955c-108">If you are backing up a locally pinned volume, we recommend that you specify a cloud snapshot.</span></span> <span data-ttu-id="2955c-109">Med ett stort antal lokala ögonblicksbilder av en lokalt Fäst volym tillsammans med en datauppsättning som har mycket omsättning resulterar i en situation där du snabbt kan köra out-of-lokalt utrymme.</span><span class="sxs-lookup"><span data-stu-id="2955c-109">Taking a large number of local snapshots of a locally pinned volume coupled with a data set that has a lot of churn will result in a situation in which you could rapidly run out of local space.</span></span> <span data-ttu-id="2955c-110">Om du väljer tootake lokala ögonblicksbilder, rekommenderar vi att du tar färre dagliga ögonblicksbilder tooback hello senaste tillstånd behålla dem i en dag och ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="2955c-110">If you choose tootake local snapshots, we recommend that you take fewer daily snapshots tooback up hello most recent state, retain them for a day, and then delete them.</span></span>

<span data-ttu-id="2955c-111">När du tar en ögonblicksbild i molnet för en lokalt Fäst volym måste kopiera du endast hello ändras data toohello moln, där det är deduplicerad och komprimeras.</span><span class="sxs-lookup"><span data-stu-id="2955c-111">When you take a cloud snapshot of a locally pinned volume, you copy only hello changed data toohello cloud, where it is deduplicated and compressed.</span></span> 

## <a name="hello-backup-policies-page"></a><span data-ttu-id="2955c-112">Hej Säkerhetskopieringsprinciper sidan</span><span class="sxs-lookup"><span data-stu-id="2955c-112">hello Backup Policies page</span></span>
<span data-ttu-id="2955c-113">Hej **Säkerhetskopieringsprinciper** sidan kan du toomanage principer för säkerhetskopiering och schema för lokala och molnögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="2955c-113">hello **Backup Policies** page allows you toomanage backup policies and schedule local and cloud snapshots.</span></span> <span data-ttu-id="2955c-114">(Principer för säkerhetskopiering är används tooconfigure scheman för säkerhetskopiering och lagring av säkerhetskopior för en samling av volymer). Principer för säkerhetskopiering kan du tootake en ögonblicksbild av flera volymer samtidigt.</span><span class="sxs-lookup"><span data-stu-id="2955c-114">(Backup policies are used tooconfigure backup schedules and backup retention for a collection of volumes.) Backup policies enable you tootake a snapshot of multiple volumes simultaneously.</span></span> <span data-ttu-id="2955c-115">Det innebär att hello säkerhetskopior som har skapats av en princip för säkerhetskopiering är kraschkonsekvent kopior.</span><span class="sxs-lookup"><span data-stu-id="2955c-115">This means that hello backups created by a backup policy will be crash-consistent copies.</span></span> <span data-ttu-id="2955c-116">Hej **Säkerhetskopieringsprinciper** sidan visar hello principer för säkerhetskopiering, typer, hello associerade volymer, hello antal säkerhetskopior bevaras och hello alternativet tooenable dessa principer.</span><span class="sxs-lookup"><span data-stu-id="2955c-116">hello **Backup Policies** page lists hello backup policies, their types, hello associated volumes, hello number of backups retained, and hello option tooenable these policies.</span></span>

<span data-ttu-id="2955c-117">Hej **Säkerhetskopieringsprinciper** sidan kan du också toofilter hello befintliga principer för säkerhetskopiering av en eller flera av hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="2955c-117">hello **Backup Policies** page also allows you toofilter hello existing backup policies by one or more of hello following fields:</span></span>

* <span data-ttu-id="2955c-118">**Principnamn** – hello namnet som associeras med hello princip.</span><span class="sxs-lookup"><span data-stu-id="2955c-118">**Policy name** – hello name associated with hello policy.</span></span> <span data-ttu-id="2955c-119">hello olika typer av principer är:</span><span class="sxs-lookup"><span data-stu-id="2955c-119">hello different types of policies include:</span></span>
  
  * <span data-ttu-id="2955c-120">Schemalagda principer som skapas av hello användaren explicit.</span><span class="sxs-lookup"><span data-stu-id="2955c-120">Scheduled policies, which are explicitly created by hello user.</span></span>
  * <span data-ttu-id="2955c-121">Automatisk principer som skapas när hello standard säkerhetskopiering för det här alternativet om volymen har aktiverat när hello volymer skapas.</span><span class="sxs-lookup"><span data-stu-id="2955c-121">Automatic policies, which are created when hello default backup for this volume option was enabled at hello time of volume creation.</span></span> <span data-ttu-id="2955c-122">Dessa principer är namngivna som *VolumeName*_standardsökmappen där *VolumeName* refererar toohello namnet på hello StorSimple-volym som konfigurerats av hello användare i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2955c-122">These policies are named as *VolumeName*_Default where *VolumeName* refers toohello name of hello StorSimple volume configured by hello user in hello Azure classic portal.</span></span> <span data-ttu-id="2955c-123">hello automatisk principer resultera i dagliga molnögonblicksbilder början på 22:30 tid.</span><span class="sxs-lookup"><span data-stu-id="2955c-123">hello automatic policies result in daily cloud snapshots beginning at 22:30 device time.</span></span>
  * <span data-ttu-id="2955c-124">Importera principer som ursprungligen skapades i hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="2955c-124">Imported policies, which were originally created in hello StorSimple Snapshot Manager.</span></span> <span data-ttu-id="2955c-125">Dessa har en tagg som beskriver hello StorSimple Snapshot Manager värden som hello principer har importerats från.</span><span class="sxs-lookup"><span data-stu-id="2955c-125">These have a tag that describes hello StorSimple Snapshot Manager host that hello policies were imported from.</span></span>
* <span data-ttu-id="2955c-126">**Volymer** – hello volymer som är associerade med hello principen.</span><span class="sxs-lookup"><span data-stu-id="2955c-126">**Volumes** – hello volumes associated with hello policy.</span></span> <span data-ttu-id="2955c-127">Alla hello volymer som är associerade med en princip för säkerhetskopiering är grupperade när säkerhetskopieringar skapas.</span><span class="sxs-lookup"><span data-stu-id="2955c-127">All hello volumes associated with a backup policy are grouped together when backups are created.</span></span>
* <span data-ttu-id="2955c-128">**Senaste lyckade säkerhetskopiering** – hello datum och tid för hello senaste lyckad säkerhetskopia som togs med den här principen.</span><span class="sxs-lookup"><span data-stu-id="2955c-128">**Last successful backup** – hello date and time of hello last successful backup that was taken with this policy.</span></span>
* <span data-ttu-id="2955c-129">**Nästa säkerhetskopiering** – hello datum och tid för hello nästa schemalagda säkerhetskopieringen som initieras av den här principen.</span><span class="sxs-lookup"><span data-stu-id="2955c-129">**Next backup** – hello date and time of hello next scheduled backup that will be initiated by this policy.</span></span>
* <span data-ttu-id="2955c-130">**Scheman** – hello antalet scheman som är associerade med hello säkerhetskopieringsprincip.</span><span class="sxs-lookup"><span data-stu-id="2955c-130">**Schedules** – hello number of schedules associated with hello backup policy.</span></span>

<span data-ttu-id="2955c-131">hello vanliga åtgärder som du kan utföra den här sidan är:</span><span class="sxs-lookup"><span data-stu-id="2955c-131">hello frequently used operations that you can perform from this page are:</span></span>

* <span data-ttu-id="2955c-132">Lägga till en säkerhetskopieringspolicy</span><span class="sxs-lookup"><span data-stu-id="2955c-132">Add a backup policy</span></span> 
* <span data-ttu-id="2955c-133">Lägg till eller ändra ett schema</span><span class="sxs-lookup"><span data-stu-id="2955c-133">Add or modify a schedule</span></span> 
* <span data-ttu-id="2955c-134">Ta bort en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="2955c-134">Delete a backup policy</span></span> 
* <span data-ttu-id="2955c-135">Gör en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="2955c-135">Take a manual backup</span></span> 
* <span data-ttu-id="2955c-136">Skapa en anpassad princip för säkerhetskopiering med flera volymer och scheman</span><span class="sxs-lookup"><span data-stu-id="2955c-136">Create a custom backup policy with multiple volumes and schedules</span></span> 

## <a name="add-a-backup-policy"></a><span data-ttu-id="2955c-137">Lägga till en säkerhetskopieringspolicy</span><span class="sxs-lookup"><span data-stu-id="2955c-137">Add a backup policy</span></span>
<span data-ttu-id="2955c-138">Lägg till en princip för säkerhetskopiering tooautomatically schema säkerhetskopiorna.</span><span class="sxs-lookup"><span data-stu-id="2955c-138">Add a backup policy tooautomatically schedule your backups.</span></span> <span data-ttu-id="2955c-139">Utföra hello följa stegen i hello Azure klassiska portal tooadd en säkerhetskopieringsprincip för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="2955c-139">Perform hello following steps in hello Azure classic portal tooadd a backup policy for your StorSimple device.</span></span> <span data-ttu-id="2955c-140">När du har lagt till hello princip kan du definiera ett schema (se [Lägg till eller ändra ett schema](#add-or-modify-a-schedule)).</span><span class="sxs-lookup"><span data-stu-id="2955c-140">After you add hello policy, you can define a schedule (see [Add or modify a schedule](#add-or-modify-a-schedule)).</span></span>

[!INCLUDE [storsimple-add-backup-policy-u2](../../includes/storsimple-add-backup-policy-u2.md)]

<span data-ttu-id="2955c-141">![Video tillgänglig](./media/storsimple-manage-backup-policies-u2/Video_icon.png) **Video tillgänglig**</span><span class="sxs-lookup"><span data-stu-id="2955c-141">![Video available](./media/storsimple-manage-backup-policies-u2/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="2955c-142">toowatch en video som visar hur toocreate en lokal eller molnet säkerhetskopiera princip, klickar du på [här](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span><span class="sxs-lookup"><span data-stu-id="2955c-142">toowatch a video that demonstrates how toocreate a local or cloud backup policy, click [here](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span></span>

## <a name="add-or-modify-a-schedule"></a><span data-ttu-id="2955c-143">Lägg till eller ändra ett schema</span><span class="sxs-lookup"><span data-stu-id="2955c-143">Add or modify a schedule</span></span>
<span data-ttu-id="2955c-144">Du kan lägga till eller ändra ett schema som är bifogade tooan säkerhetskopieringsprincip på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="2955c-144">You can add or modify a schedule that is attached tooan existing backup policy on your StorSimple device.</span></span> <span data-ttu-id="2955c-145">Utför följande steg i hello Azure klassiska portal tooadd hello eller ändra ett schema.</span><span class="sxs-lookup"><span data-stu-id="2955c-145">Perform hello following steps in hello Azure classic portal tooadd or modify a schedule.</span></span>

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule-u2.md)]

## <a name="delete-a-backup-policy"></a><span data-ttu-id="2955c-146">Ta bort en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="2955c-146">Delete a backup policy</span></span>
<span data-ttu-id="2955c-147">Utföra hello följa stegen i hello Azure klassiska portal toodelete en princip för säkerhetskopiering på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="2955c-147">Perform hello following steps in hello Azure classic portal toodelete a backup policy on your StorSimple device.</span></span>

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a><span data-ttu-id="2955c-148">Gör en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="2955c-148">Take a manual backup</span></span>
<span data-ttu-id="2955c-149">Utföra hello följa stegen i hello Azure klassiska portal toocreate en på-begäran (manuell) säkerhetskopiering för en enskild volym.</span><span class="sxs-lookup"><span data-stu-id="2955c-149">Perform hello following steps in hello Azure classic portal toocreate an on-demand (manual) backup for a single volume.</span></span>

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a><span data-ttu-id="2955c-150">Skapa en anpassad princip för säkerhetskopiering med flera volymer och scheman</span><span class="sxs-lookup"><span data-stu-id="2955c-150">Create a custom backup policy with multiple volumes and schedules</span></span>
<span data-ttu-id="2955c-151">Utföra hello följa stegen i hello Azure klassiska portal toocreate en anpassad princip för säkerhetskopiering som har flera volymer och scheman.</span><span class="sxs-lookup"><span data-stu-id="2955c-151">Perform hello following steps in hello Azure classic portal toocreate a custom backup policy that has multiple volumes and schedules.</span></span>

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy-u2.md)]

## <a name="next-steps"></a><span data-ttu-id="2955c-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2955c-152">Next steps</span></span>
<span data-ttu-id="2955c-153">Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="2955c-153">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

