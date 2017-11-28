---
title: "Hantera din StorSimple-säkerhetskopieringsprinciper | Microsoft Docs"
description: "Beskriver hur du kan använda StorSimple Manager-tjänsten för att skapa och hantera manuella säkerhetskopieringar, scheman för säkerhetskopiering och lagring av säkerhetskopior.."
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
ms.openlocfilehash: 5448247428ab96887470c6b53f7a9b3dcd9238f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-backup-policies-update-2"></a><span data-ttu-id="f558c-103">Använda StorSimple Manager-tjänsten för att hantera principer för säkerhetskopiering (uppdatering 2)</span><span class="sxs-lookup"><span data-stu-id="f558c-103">Use the StorSimple Manager service to manage backup policies (Update 2)</span></span>
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a><span data-ttu-id="f558c-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="f558c-104">Overview</span></span>
<span data-ttu-id="f558c-105">Den här självstudiekursen beskrivs hur du använder StorSimple Manager-tjänsten **Säkerhetskopieringsprinciper** sidan för att styra processer för säkerhetskopiering och lagring av säkerhetskopior för din StorSimple-volymer.</span><span class="sxs-lookup"><span data-stu-id="f558c-105">This tutorial explains how to use the StorSimple Manager service **Backup Policies** page to control backup processes and backup retention for your StorSimple volumes.</span></span> <span data-ttu-id="f558c-106">Det beskriver också hur du utför en manuell säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="f558c-106">It also describes how to complete a manual backup.</span></span>

<span data-ttu-id="f558c-107">När du säkerhetskopierar en volym kan du skapa en lokal ögonblicksbild eller en ögonblicksbild i molnet.</span><span class="sxs-lookup"><span data-stu-id="f558c-107">When you back up a volume, you can choose to create a local snapshot or a cloud snapshot.</span></span> <span data-ttu-id="f558c-108">Om du säkerhetskopierar en lokalt Fäst volym rekommenderar vi att du anger en ögonblicksbild i molnet.</span><span class="sxs-lookup"><span data-stu-id="f558c-108">If you are backing up a locally pinned volume, we recommend that you specify a cloud snapshot.</span></span> <span data-ttu-id="f558c-109">Med ett stort antal lokala ögonblicksbilder av en lokalt Fäst volym tillsammans med en datauppsättning som har mycket omsättning resulterar i en situation där du snabbt kan köra out-of-lokalt utrymme.</span><span class="sxs-lookup"><span data-stu-id="f558c-109">Taking a large number of local snapshots of a locally pinned volume coupled with a data set that has a lot of churn will result in a situation in which you could rapidly run out of local space.</span></span> <span data-ttu-id="f558c-110">Om du väljer att ta lokala ögonblicksbilder, rekommenderar vi att du ta färre dagliga ögonblicksbilder för att säkerhetskopiera tillståndet senaste behålla dem under en dag och ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="f558c-110">If you choose to take local snapshots, we recommend that you take fewer daily snapshots to back up the most recent state, retain them for a day, and then delete them.</span></span>

<span data-ttu-id="f558c-111">När du tar en ögonblicksbild i molnet för en lokalt Fäst volym måste kopiera du endast de ändrade data till molnet, där det är deduplicerad och komprimeras.</span><span class="sxs-lookup"><span data-stu-id="f558c-111">When you take a cloud snapshot of a locally pinned volume, you copy only the changed data to the cloud, where it is deduplicated and compressed.</span></span> 

## <a name="the-backup-policies-page"></a><span data-ttu-id="f558c-112">Sidan för principer för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="f558c-112">The Backup Policies page</span></span>
<span data-ttu-id="f558c-113">Den **Säkerhetskopieringsprinciper** sidan kan du hantera principer för säkerhetskopiering och schemalägga lokala och molnbaserade ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="f558c-113">The **Backup Policies** page allows you to manage backup policies and schedule local and cloud snapshots.</span></span> <span data-ttu-id="f558c-114">(Säkerhetskopieringsprinciper används för att konfigurera scheman för säkerhetskopiering och lagring av säkerhetskopior för en samling av volymer.) Principer för säkerhetskopiering gör att du kan ta en ögonblicksbild av flera volymer samtidigt.</span><span class="sxs-lookup"><span data-stu-id="f558c-114">(Backup policies are used to configure backup schedules and backup retention for a collection of volumes.) Backup policies enable you to take a snapshot of multiple volumes simultaneously.</span></span> <span data-ttu-id="f558c-115">Det innebär att säkerhetskopiorna som skapats av en princip för säkerhetskopiering är kraschkonsekvent kopior.</span><span class="sxs-lookup"><span data-stu-id="f558c-115">This means that the backups created by a backup policy will be crash-consistent copies.</span></span> <span data-ttu-id="f558c-116">Den **Säkerhetskopieringsprinciper** sidan listar principer för säkerhetskopiering, deras typer, associerade volymer, antal säkerhetskopior bevaras och att aktivera dessa principer.</span><span class="sxs-lookup"><span data-stu-id="f558c-116">The **Backup Policies** page lists the backup policies, their types, the associated volumes, the number of backups retained, and the option to enable these policies.</span></span>

<span data-ttu-id="f558c-117">Den **Säkerhetskopieringsprinciper** sidan kan du filtrera befintliga principer för säkerhetskopiering av en eller flera av följande fält:</span><span class="sxs-lookup"><span data-stu-id="f558c-117">The **Backup Policies** page also allows you to filter the existing backup policies by one or more of the following fields:</span></span>

* <span data-ttu-id="f558c-118">**Principnamn** – namnet som associeras med principen.</span><span class="sxs-lookup"><span data-stu-id="f558c-118">**Policy name** – The name associated with the policy.</span></span> <span data-ttu-id="f558c-119">Olika typer av principer är:</span><span class="sxs-lookup"><span data-stu-id="f558c-119">The different types of policies include:</span></span>
  
  * <span data-ttu-id="f558c-120">Schemalagda principer som uttryckligen har skapats av användaren.</span><span class="sxs-lookup"><span data-stu-id="f558c-120">Scheduled policies, which are explicitly created by the user.</span></span>
  * <span data-ttu-id="f558c-121">Automatisk principer som skapas när standard säkerhetskopieringen för det här alternativet om volymen har aktiverats på volymen skapades.</span><span class="sxs-lookup"><span data-stu-id="f558c-121">Automatic policies, which are created when the default backup for this volume option was enabled at the time of volume creation.</span></span> <span data-ttu-id="f558c-122">Dessa principer är namngivna som *VolumeName*_standardsökmappen där *VolumeName* refererar till namnet på StorSimple-volym som konfigurerats av användaren i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f558c-122">These policies are named as *VolumeName*_Default where *VolumeName* refers to the name of the StorSimple volume configured by the user in the Azure classic portal.</span></span> <span data-ttu-id="f558c-123">Automatisk principerna resultera i dagliga molnögonblicksbilder början på 22:30 tid.</span><span class="sxs-lookup"><span data-stu-id="f558c-123">The automatic policies result in daily cloud snapshots beginning at 22:30 device time.</span></span>
  * <span data-ttu-id="f558c-124">Importerade principer som ursprungligen skapades i StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="f558c-124">Imported policies, which were originally created in the StorSimple Snapshot Manager.</span></span> <span data-ttu-id="f558c-125">Dessa har en tagg som beskriver StorSimple Snapshot Manager värden som principerna som har importerats från.</span><span class="sxs-lookup"><span data-stu-id="f558c-125">These have a tag that describes the StorSimple Snapshot Manager host that the policies were imported from.</span></span>
* <span data-ttu-id="f558c-126">**Volymer** – de volymer som är associerade med principen.</span><span class="sxs-lookup"><span data-stu-id="f558c-126">**Volumes** – The volumes associated with the policy.</span></span> <span data-ttu-id="f558c-127">Alla volymer som är associerade med en princip för säkerhetskopiering är grupperade när säkerhetskopieringar skapas.</span><span class="sxs-lookup"><span data-stu-id="f558c-127">All the volumes associated with a backup policy are grouped together when backups are created.</span></span>
* <span data-ttu-id="f558c-128">**Senaste lyckade säkerhetskopiering** – datum och tid för senaste lyckade säkerhetskopian som skapades med den här principen.</span><span class="sxs-lookup"><span data-stu-id="f558c-128">**Last successful backup** – The date and time of the last successful backup that was taken with this policy.</span></span>
* <span data-ttu-id="f558c-129">**Nästa säkerhetskopiering** – datum och tid för nästa schemalagda säkerhetskopian som initieras av den här principen.</span><span class="sxs-lookup"><span data-stu-id="f558c-129">**Next backup** – The date and time of the next scheduled backup that will be initiated by this policy.</span></span>
* <span data-ttu-id="f558c-130">**Scheman** – antalet scheman som är associerade med principen för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="f558c-130">**Schedules** – The number of schedules associated with the backup policy.</span></span>

<span data-ttu-id="f558c-131">Vanliga åtgärder som du kan utföra från den här sidan är:</span><span class="sxs-lookup"><span data-stu-id="f558c-131">The frequently used operations that you can perform from this page are:</span></span>

* <span data-ttu-id="f558c-132">Lägga till en säkerhetskopieringspolicy</span><span class="sxs-lookup"><span data-stu-id="f558c-132">Add a backup policy</span></span> 
* <span data-ttu-id="f558c-133">Lägg till eller ändra ett schema</span><span class="sxs-lookup"><span data-stu-id="f558c-133">Add or modify a schedule</span></span> 
* <span data-ttu-id="f558c-134">Ta bort en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="f558c-134">Delete a backup policy</span></span> 
* <span data-ttu-id="f558c-135">Gör en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="f558c-135">Take a manual backup</span></span> 
* <span data-ttu-id="f558c-136">Skapa en anpassad princip för säkerhetskopiering med flera volymer och scheman</span><span class="sxs-lookup"><span data-stu-id="f558c-136">Create a custom backup policy with multiple volumes and schedules</span></span> 

## <a name="add-a-backup-policy"></a><span data-ttu-id="f558c-137">Lägga till en säkerhetskopieringspolicy</span><span class="sxs-lookup"><span data-stu-id="f558c-137">Add a backup policy</span></span>
<span data-ttu-id="f558c-138">Lägga till en princip för säkerhetskopiering för att automatiskt schemalägga säkerhetskopiorna.</span><span class="sxs-lookup"><span data-stu-id="f558c-138">Add a backup policy to automatically schedule your backups.</span></span> <span data-ttu-id="f558c-139">Utför följande steg i den klassiska Azure portalen för att lägga till en princip för säkerhetskopiering för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="f558c-139">Perform the following steps in the Azure classic portal to add a backup policy for your StorSimple device.</span></span> <span data-ttu-id="f558c-140">När du lägger till principen, kan du definiera ett schema (se [Lägg till eller ändra ett schema](#add-or-modify-a-schedule)).</span><span class="sxs-lookup"><span data-stu-id="f558c-140">After you add the policy, you can define a schedule (see [Add or modify a schedule](#add-or-modify-a-schedule)).</span></span>

[!INCLUDE [storsimple-add-backup-policy-u2](../../includes/storsimple-add-backup-policy-u2.md)]

<span data-ttu-id="f558c-141">![Video tillgänglig](./media/storsimple-manage-backup-policies-u2/Video_icon.png) **Video tillgänglig**</span><span class="sxs-lookup"><span data-stu-id="f558c-141">![Video available](./media/storsimple-manage-backup-policies-u2/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="f558c-142">Om du vill se en video som visar hur du skapar en lokal eller molnet säkerhetskopieringsprincip, klickar du på [här](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span><span class="sxs-lookup"><span data-stu-id="f558c-142">To watch a video that demonstrates how to create a local or cloud backup policy, click [here](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span></span>

## <a name="add-or-modify-a-schedule"></a><span data-ttu-id="f558c-143">Lägg till eller ändra ett schema</span><span class="sxs-lookup"><span data-stu-id="f558c-143">Add or modify a schedule</span></span>
<span data-ttu-id="f558c-144">Du kan lägga till eller ändra ett schema som är kopplad till en säkerhetskopieringsprincip på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="f558c-144">You can add or modify a schedule that is attached to an existing backup policy on your StorSimple device.</span></span> <span data-ttu-id="f558c-145">Utför följande steg i den klassiska Azure portalen för att lägga till eller ändra ett schema.</span><span class="sxs-lookup"><span data-stu-id="f558c-145">Perform the following steps in the Azure classic portal to add or modify a schedule.</span></span>

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule-u2.md)]

## <a name="delete-a-backup-policy"></a><span data-ttu-id="f558c-146">Ta bort en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="f558c-146">Delete a backup policy</span></span>
<span data-ttu-id="f558c-147">Utför följande steg i den klassiska Azure portalen för att ta bort en princip för säkerhetskopiering på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="f558c-147">Perform the following steps in the Azure classic portal to delete a backup policy on your StorSimple device.</span></span>

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a><span data-ttu-id="f558c-148">Gör en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="f558c-148">Take a manual backup</span></span>
<span data-ttu-id="f558c-149">Utför följande steg i den klassiska Azure portalen för att skapa en (manuell) säkerhetskopiering på begäran för en enskild volym.</span><span class="sxs-lookup"><span data-stu-id="f558c-149">Perform the following steps in the Azure classic portal to create an on-demand (manual) backup for a single volume.</span></span>

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a><span data-ttu-id="f558c-150">Skapa en anpassad princip för säkerhetskopiering med flera volymer och scheman</span><span class="sxs-lookup"><span data-stu-id="f558c-150">Create a custom backup policy with multiple volumes and schedules</span></span>
<span data-ttu-id="f558c-151">Utför följande steg i den klassiska Azure portalen för att skapa en anpassad princip för säkerhetskopiering som har flera volymer och scheman.</span><span class="sxs-lookup"><span data-stu-id="f558c-151">Perform the following steps in the Azure classic portal to create a custom backup policy that has multiple volumes and schedules.</span></span>

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy-u2.md)]

## <a name="next-steps"></a><span data-ttu-id="f558c-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f558c-152">Next steps</span></span>
<span data-ttu-id="f558c-153">Lär dig mer om [använda StorSimple Manager-tjänsten för att administrera din StorSimple-enhet](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="f558c-153">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

