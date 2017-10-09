---
title: "aaaManage din StorSimple-säkerhetskopieringsprinciper | Microsoft Docs"
description: "Beskriver hur du kan använda hello StorSimple Manager-tjänsten toocreate och hantera manuella säkerhetskopieringar, scheman för säkerhetskopiering och lagring av säkerhetskopior.."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: d1834bc8-d520-4463-82ae-3b32fabd021c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2016
ms.author: v-sharos
ms.openlocfilehash: 710cbe54d14031b4de43e9da292ed169085d5af9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-backup-policies"></a><span data-ttu-id="7cab8-103">Använd hello StorSimple Manager-tjänsten toomanage principer för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="7cab8-103">Use hello StorSimple Manager service toomanage backup policies</span></span>
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a><span data-ttu-id="7cab8-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="7cab8-104">Overview</span></span>
<span data-ttu-id="7cab8-105">Den här självstudiekursen beskrivs hur toouse hello StorSimple Manager-tjänsten **Säkerhetskopieringsprinciper** toocontrol processer för säkerhetskopiering och lagring av säkerhetskopior för din StorSimple-volymer.</span><span class="sxs-lookup"><span data-stu-id="7cab8-105">This tutorial explains how toouse hello StorSimple Manager service **Backup Policies** page toocontrol backup processes and backup retention for your StorSimple volumes.</span></span> <span data-ttu-id="7cab8-106">Här beskrivs också hur toocomplete en manuell säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="7cab8-106">It also describes how toocomplete a manual backup.</span></span>

<span data-ttu-id="7cab8-107">Hej **Säkerhetskopieringsprinciper** sidan kan du toomanage principer för säkerhetskopiering och schema för lokala och molnögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="7cab8-107">hello **Backup Policies** page allows you toomanage backup policies and schedule local and cloud snapshots.</span></span> <span data-ttu-id="7cab8-108">(Principer för säkerhetskopiering är används tooconfigure scheman för säkerhetskopiering och lagring av säkerhetskopior för en samling av volymer). Principer för säkerhetskopiering kan du tootake en ögonblicksbild av flera volymer samtidigt.</span><span class="sxs-lookup"><span data-stu-id="7cab8-108">(Backup policies are used tooconfigure backup schedules and backup retention for a collection of volumes.) Backup policies enable you tootake a snapshot of multiple volumes simultaneously.</span></span> <span data-ttu-id="7cab8-109">Det innebär att hello säkerhetskopior som har skapats av en princip för säkerhetskopiering är kraschkonsekvent kopior.</span><span class="sxs-lookup"><span data-stu-id="7cab8-109">This means that hello backups created by a backup policy will be crash-consistent copies.</span></span> <span data-ttu-id="7cab8-110">Den här sidan visar hello principer för säkerhetskopiering, typer, hello associerade volymer, hello antal säkerhetskopior bevaras och hello alternativet tooenable dessa principer.</span><span class="sxs-lookup"><span data-stu-id="7cab8-110">This page lists hello backup policies, their types, hello associated volumes, hello number of backups retained, and hello option tooenable these policies.</span></span>

<span data-ttu-id="7cab8-111">Hej **Säkerhetskopieringsprinciper** sidan kan du också toofilter hello befintliga principer för säkerhetskopiering av en eller flera av hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="7cab8-111">hello **Backup Policies** page also allows you toofilter hello existing backup policies by one or more of hello following fields:</span></span>

* <span data-ttu-id="7cab8-112">**Principnamn** – hello namnet som associeras med hello princip.</span><span class="sxs-lookup"><span data-stu-id="7cab8-112">**Policy name** – hello name associated with hello policy.</span></span> <span data-ttu-id="7cab8-113">hello olika typer av principer är:</span><span class="sxs-lookup"><span data-stu-id="7cab8-113">hello different types of policies include:</span></span>
  
  * <span data-ttu-id="7cab8-114">Schemalagda principer som skapas av hello användaren explicit.</span><span class="sxs-lookup"><span data-stu-id="7cab8-114">Scheduled policies, which are explicitly created by hello user.</span></span>
  * <span data-ttu-id="7cab8-115">Automatisk principer som skapas när hello standard säkerhetskopiering för det här alternativet om volymen har aktiverat när hello volymer skapas.</span><span class="sxs-lookup"><span data-stu-id="7cab8-115">Automatic policies, which are created when hello default backup for this volume option was enabled at hello time of volume creation.</span></span> <span data-ttu-id="7cab8-116">Dessa principer är namngivna som VolumeName_Default där volymnamn refererar toohello namnet på hello StorSimple-volym som konfigurerats av hello användare i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7cab8-116">These policies are named as VolumeName_Default where Volume name refers toohello name of hello StorSimple volume configured by hello user in hello Azure classic portal.</span></span> <span data-ttu-id="7cab8-117">hello automatisk principer resultera i dagliga molnögonblicksbilder början på 22:30 tid.</span><span class="sxs-lookup"><span data-stu-id="7cab8-117">hello automatic policies result in daily cloud snapshots beginning at 22:30 device time.</span></span>
  * <span data-ttu-id="7cab8-118">Importera principer som ursprungligen skapades i hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="7cab8-118">Imported policies, which were originally created in hello StorSimple Snapshot Manager.</span></span> <span data-ttu-id="7cab8-119">Dessa har en tagg som beskriver hello StorSimple Snapshot Manager värden som hello principer har importerats från.</span><span class="sxs-lookup"><span data-stu-id="7cab8-119">These have a tag that describes hello StorSimple Snapshot Manager host that hello policies were imported from.</span></span>
* <span data-ttu-id="7cab8-120">**Volymer** – hello volymer som är associerade med hello principen.</span><span class="sxs-lookup"><span data-stu-id="7cab8-120">**Volumes** – hello volumes associated with hello policy.</span></span> <span data-ttu-id="7cab8-121">Alla hello volymer som är associerade med en princip för säkerhetskopiering är grupperade när säkerhetskopieringar skapas.</span><span class="sxs-lookup"><span data-stu-id="7cab8-121">All hello volumes associated with a backup policy are grouped together when backups are created.</span></span>
* <span data-ttu-id="7cab8-122">**Senaste lyckade säkerhetskopiering** – hello datum och tid för hello senaste lyckad säkerhetskopia som togs med den här principen.</span><span class="sxs-lookup"><span data-stu-id="7cab8-122">**Last successful backup** – hello date and time of hello last successful backup that was taken with this policy.</span></span>
* <span data-ttu-id="7cab8-123">**Nästa säkerhetskopiering** – hello datum och tid för hello nästa schemalagda säkerhetskopieringen som initieras av den här principen.</span><span class="sxs-lookup"><span data-stu-id="7cab8-123">**Next backup** – hello date and time of hello next scheduled backup that will be initiated by this policy.</span></span>
* <span data-ttu-id="7cab8-124">**Scheman** – hello antalet scheman som är associerade med hello säkerhetskopieringsprincip.</span><span class="sxs-lookup"><span data-stu-id="7cab8-124">**Schedules** – hello number of schedules associated with hello backup policy.</span></span>

<span data-ttu-id="7cab8-125">hello vanliga åtgärder som du kan utföra den här sidan är:</span><span class="sxs-lookup"><span data-stu-id="7cab8-125">hello frequently used operations that you can perform from this page are:</span></span>

* <span data-ttu-id="7cab8-126">Lägga till en säkerhetskopieringspolicy</span><span class="sxs-lookup"><span data-stu-id="7cab8-126">Add a backup policy</span></span> 
* <span data-ttu-id="7cab8-127">Lägg till eller ändra ett schema</span><span class="sxs-lookup"><span data-stu-id="7cab8-127">Add or modify a schedule</span></span> 
* <span data-ttu-id="7cab8-128">Ta bort en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="7cab8-128">Delete a backup policy</span></span> 
* <span data-ttu-id="7cab8-129">Gör en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="7cab8-129">Take a manual backup</span></span> 
* <span data-ttu-id="7cab8-130">Skapa en anpassad princip för säkerhetskopiering med flera volymer och scheman</span><span class="sxs-lookup"><span data-stu-id="7cab8-130">Create a custom backup policy with multiple volumes and schedules</span></span> 

## <a name="add-a-backup-policy"></a><span data-ttu-id="7cab8-131">Lägga till en säkerhetskopieringspolicy</span><span class="sxs-lookup"><span data-stu-id="7cab8-131">Add a backup policy</span></span>
<span data-ttu-id="7cab8-132">Lägg till en princip för säkerhetskopiering tooautomatically schema säkerhetskopiorna.</span><span class="sxs-lookup"><span data-stu-id="7cab8-132">Add a backup policy tooautomatically schedule your backups.</span></span> <span data-ttu-id="7cab8-133">Utföra hello följa stegen i hello Azure klassiska portal tooadd en säkerhetskopieringsprincip för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="7cab8-133">Perform hello following steps in hello Azure classic portal tooadd a backup policy for your StorSimple device.</span></span> <span data-ttu-id="7cab8-134">När du har lagt till hello princip kan du definiera ett schema (se [Lägg till eller ändra ett schema](#add-or-modify-a-schedule)).</span><span class="sxs-lookup"><span data-stu-id="7cab8-134">After you add hello policy, you can define a schedule (see [Add or modify a schedule](#add-or-modify-a-schedule)).</span></span>

[!INCLUDE [storsimple-add-backup-policy](../../includes/storsimple-add-backup-policy.md)]

<span data-ttu-id="7cab8-135">![Video tillgänglig](./media/storsimple-manage-backup-policies/Video_icon.png) **Video tillgänglig**</span><span class="sxs-lookup"><span data-stu-id="7cab8-135">![Video available](./media/storsimple-manage-backup-policies/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="7cab8-136">toowatch en video som visar hur toocreate en lokal eller molnet säkerhetskopiera princip, klickar du på [här](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span><span class="sxs-lookup"><span data-stu-id="7cab8-136">toowatch a video that demonstrates how toocreate a local or cloud backup policy, click [here](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span></span>

## <a name="add-or-modify-a-schedule"></a><span data-ttu-id="7cab8-137">Lägg till eller ändra ett schema</span><span class="sxs-lookup"><span data-stu-id="7cab8-137">Add or modify a schedule</span></span>
<span data-ttu-id="7cab8-138">Du kan lägga till eller ändra ett schema som är bifogade tooan säkerhetskopieringsprincip på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="7cab8-138">You can add or modify a schedule that is attached tooan existing backup policy on your StorSimple device.</span></span> <span data-ttu-id="7cab8-139">Utför följande steg i hello Azure klassiska portal tooadd hello eller ändra ett schema.</span><span class="sxs-lookup"><span data-stu-id="7cab8-139">Perform hello following steps in hello Azure classic portal tooadd or modify a schedule.</span></span>

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule.md)]

## <a name="delete-a-backup-policy"></a><span data-ttu-id="7cab8-140">Ta bort en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="7cab8-140">Delete a backup policy</span></span>
<span data-ttu-id="7cab8-141">Utföra hello följa stegen i hello Azure klassiska portal toodelete en princip för säkerhetskopiering på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="7cab8-141">Perform hello following steps in hello Azure classic portal toodelete a backup policy on your StorSimple device.</span></span>

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a><span data-ttu-id="7cab8-142">Gör en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="7cab8-142">Take a manual backup</span></span>
<span data-ttu-id="7cab8-143">Utföra hello följa stegen i hello Azure klassiska portal toocreate en på-begäran (manuell) säkerhetskopiering för en enskild volym.</span><span class="sxs-lookup"><span data-stu-id="7cab8-143">Perform hello following steps in hello Azure classic portal toocreate an on-demand (manual) backup for a single volume.</span></span>

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a><span data-ttu-id="7cab8-144">Skapa en anpassad princip för säkerhetskopiering med flera volymer och scheman</span><span class="sxs-lookup"><span data-stu-id="7cab8-144">Create a custom backup policy with multiple volumes and schedules</span></span>
<span data-ttu-id="7cab8-145">Utföra hello följa stegen i hello Azure klassiska portal toocreate en anpassad princip för säkerhetskopiering som har flera volymer och scheman.</span><span class="sxs-lookup"><span data-stu-id="7cab8-145">Perform hello following steps in hello Azure classic portal toocreate a custom backup policy that has multiple volumes and schedules.</span></span>

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy.md)]

## <a name="next-steps"></a><span data-ttu-id="7cab8-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7cab8-146">Next steps</span></span>
<span data-ttu-id="7cab8-147">Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="7cab8-147">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

