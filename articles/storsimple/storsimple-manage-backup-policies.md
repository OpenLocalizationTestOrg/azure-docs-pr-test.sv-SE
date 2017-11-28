---
title: "Hantera din StorSimple-säkerhetskopieringsprinciper | Microsoft Docs"
description: "Beskriver hur du kan använda StorSimple Manager-tjänsten för att skapa och hantera manuella säkerhetskopieringar, scheman för säkerhetskopiering och lagring av säkerhetskopior.."
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
ms.openlocfilehash: c1e9d5d0450bab5d371aafb40fd7c5920d39dfdb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-backup-policies"></a><span data-ttu-id="f5d1a-103">Använda StorSimple Manager-tjänsten för att hantera principer för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="f5d1a-103">Use the StorSimple Manager service to manage backup policies</span></span>
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a><span data-ttu-id="f5d1a-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="f5d1a-104">Overview</span></span>
<span data-ttu-id="f5d1a-105">Den här självstudiekursen beskrivs hur du använder StorSimple Manager-tjänsten **Säkerhetskopieringsprinciper** sidan för att styra processer för säkerhetskopiering och lagring av säkerhetskopior för din StorSimple-volymer.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-105">This tutorial explains how to use the StorSimple Manager service **Backup Policies** page to control backup processes and backup retention for your StorSimple volumes.</span></span> <span data-ttu-id="f5d1a-106">Det beskriver också hur du utför en manuell säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-106">It also describes how to complete a manual backup.</span></span>

<span data-ttu-id="f5d1a-107">Den **Säkerhetskopieringsprinciper** sidan kan du hantera principer för säkerhetskopiering och schemalägga lokala och molnbaserade ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-107">The **Backup Policies** page allows you to manage backup policies and schedule local and cloud snapshots.</span></span> <span data-ttu-id="f5d1a-108">(Säkerhetskopieringsprinciper används för att konfigurera scheman för säkerhetskopiering och lagring av säkerhetskopior för en samling av volymer.) Principer för säkerhetskopiering gör att du kan ta en ögonblicksbild av flera volymer samtidigt.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-108">(Backup policies are used to configure backup schedules and backup retention for a collection of volumes.) Backup policies enable you to take a snapshot of multiple volumes simultaneously.</span></span> <span data-ttu-id="f5d1a-109">Det innebär att säkerhetskopiorna som skapats av en princip för säkerhetskopiering är kraschkonsekvent kopior.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-109">This means that the backups created by a backup policy will be crash-consistent copies.</span></span> <span data-ttu-id="f5d1a-110">Den här sidan visar principer för säkerhetskopiering, deras typer, associerade volymer, antal säkerhetskopior bevaras och att aktivera dessa principer.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-110">This page lists the backup policies, their types, the associated volumes, the number of backups retained, and the option to enable these policies.</span></span>

<span data-ttu-id="f5d1a-111">Den **Säkerhetskopieringsprinciper** sidan kan du filtrera befintliga principer för säkerhetskopiering av en eller flera av följande fält:</span><span class="sxs-lookup"><span data-stu-id="f5d1a-111">The **Backup Policies** page also allows you to filter the existing backup policies by one or more of the following fields:</span></span>

* <span data-ttu-id="f5d1a-112">**Principnamn** – namnet som associeras med principen.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-112">**Policy name** – The name associated with the policy.</span></span> <span data-ttu-id="f5d1a-113">Olika typer av principer är:</span><span class="sxs-lookup"><span data-stu-id="f5d1a-113">The different types of policies include:</span></span>
  
  * <span data-ttu-id="f5d1a-114">Schemalagda principer som uttryckligen har skapats av användaren.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-114">Scheduled policies, which are explicitly created by the user.</span></span>
  * <span data-ttu-id="f5d1a-115">Automatisk principer som skapas när standard säkerhetskopieringen för det här alternativet om volymen har aktiverats på volymen skapades.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-115">Automatic policies, which are created when the default backup for this volume option was enabled at the time of volume creation.</span></span> <span data-ttu-id="f5d1a-116">Dessa principer är namngivna som VolumeName_Default där volymnamn refererar till namnet på StorSimple-volym som konfigurerats av användaren i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-116">These policies are named as VolumeName_Default where Volume name refers to the name of the StorSimple volume configured by the user in the Azure classic portal.</span></span> <span data-ttu-id="f5d1a-117">Automatisk principerna resultera i dagliga molnögonblicksbilder början på 22:30 tid.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-117">The automatic policies result in daily cloud snapshots beginning at 22:30 device time.</span></span>
  * <span data-ttu-id="f5d1a-118">Importerade principer som ursprungligen skapades i StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-118">Imported policies, which were originally created in the StorSimple Snapshot Manager.</span></span> <span data-ttu-id="f5d1a-119">Dessa har en tagg som beskriver StorSimple Snapshot Manager värden som principerna som har importerats från.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-119">These have a tag that describes the StorSimple Snapshot Manager host that the policies were imported from.</span></span>
* <span data-ttu-id="f5d1a-120">**Volymer** – de volymer som är associerade med principen.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-120">**Volumes** – The volumes associated with the policy.</span></span> <span data-ttu-id="f5d1a-121">Alla volymer som är associerade med en princip för säkerhetskopiering är grupperade när säkerhetskopieringar skapas.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-121">All the volumes associated with a backup policy are grouped together when backups are created.</span></span>
* <span data-ttu-id="f5d1a-122">**Senaste lyckade säkerhetskopiering** – datum och tid för senaste lyckade säkerhetskopian som skapades med den här principen.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-122">**Last successful backup** – The date and time of the last successful backup that was taken with this policy.</span></span>
* <span data-ttu-id="f5d1a-123">**Nästa säkerhetskopiering** – datum och tid för nästa schemalagda säkerhetskopian som initieras av den här principen.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-123">**Next backup** – The date and time of the next scheduled backup that will be initiated by this policy.</span></span>
* <span data-ttu-id="f5d1a-124">**Scheman** – antalet scheman som är associerade med principen för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-124">**Schedules** – The number of schedules associated with the backup policy.</span></span>

<span data-ttu-id="f5d1a-125">Vanliga åtgärder som du kan utföra från den här sidan är:</span><span class="sxs-lookup"><span data-stu-id="f5d1a-125">The frequently used operations that you can perform from this page are:</span></span>

* <span data-ttu-id="f5d1a-126">Lägga till en säkerhetskopieringspolicy</span><span class="sxs-lookup"><span data-stu-id="f5d1a-126">Add a backup policy</span></span> 
* <span data-ttu-id="f5d1a-127">Lägg till eller ändra ett schema</span><span class="sxs-lookup"><span data-stu-id="f5d1a-127">Add or modify a schedule</span></span> 
* <span data-ttu-id="f5d1a-128">Ta bort en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="f5d1a-128">Delete a backup policy</span></span> 
* <span data-ttu-id="f5d1a-129">Gör en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="f5d1a-129">Take a manual backup</span></span> 
* <span data-ttu-id="f5d1a-130">Skapa en anpassad princip för säkerhetskopiering med flera volymer och scheman</span><span class="sxs-lookup"><span data-stu-id="f5d1a-130">Create a custom backup policy with multiple volumes and schedules</span></span> 

## <a name="add-a-backup-policy"></a><span data-ttu-id="f5d1a-131">Lägga till en säkerhetskopieringspolicy</span><span class="sxs-lookup"><span data-stu-id="f5d1a-131">Add a backup policy</span></span>
<span data-ttu-id="f5d1a-132">Lägga till en princip för säkerhetskopiering för att automatiskt schemalägga säkerhetskopiorna.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-132">Add a backup policy to automatically schedule your backups.</span></span> <span data-ttu-id="f5d1a-133">Utför följande steg i den klassiska Azure portalen för att lägga till en princip för säkerhetskopiering för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-133">Perform the following steps in the Azure classic portal to add a backup policy for your StorSimple device.</span></span> <span data-ttu-id="f5d1a-134">När du lägger till principen, kan du definiera ett schema (se [Lägg till eller ändra ett schema](#add-or-modify-a-schedule)).</span><span class="sxs-lookup"><span data-stu-id="f5d1a-134">After you add the policy, you can define a schedule (see [Add or modify a schedule](#add-or-modify-a-schedule)).</span></span>

[!INCLUDE [storsimple-add-backup-policy](../../includes/storsimple-add-backup-policy.md)]

<span data-ttu-id="f5d1a-135">![Video tillgänglig](./media/storsimple-manage-backup-policies/Video_icon.png) **Video tillgänglig**</span><span class="sxs-lookup"><span data-stu-id="f5d1a-135">![Video available](./media/storsimple-manage-backup-policies/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="f5d1a-136">Om du vill se en video som visar hur du skapar en lokal eller molnet säkerhetskopieringsprincip, klickar du på [här](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span><span class="sxs-lookup"><span data-stu-id="f5d1a-136">To watch a video that demonstrates how to create a local or cloud backup policy, click [here](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span></span>

## <a name="add-or-modify-a-schedule"></a><span data-ttu-id="f5d1a-137">Lägg till eller ändra ett schema</span><span class="sxs-lookup"><span data-stu-id="f5d1a-137">Add or modify a schedule</span></span>
<span data-ttu-id="f5d1a-138">Du kan lägga till eller ändra ett schema som är kopplad till en säkerhetskopieringsprincip på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-138">You can add or modify a schedule that is attached to an existing backup policy on your StorSimple device.</span></span> <span data-ttu-id="f5d1a-139">Utför följande steg i den klassiska Azure portalen för att lägga till eller ändra ett schema.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-139">Perform the following steps in the Azure classic portal to add or modify a schedule.</span></span>

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule.md)]

## <a name="delete-a-backup-policy"></a><span data-ttu-id="f5d1a-140">Ta bort en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="f5d1a-140">Delete a backup policy</span></span>
<span data-ttu-id="f5d1a-141">Utför följande steg i den klassiska Azure portalen för att ta bort en princip för säkerhetskopiering på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-141">Perform the following steps in the Azure classic portal to delete a backup policy on your StorSimple device.</span></span>

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a><span data-ttu-id="f5d1a-142">Gör en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="f5d1a-142">Take a manual backup</span></span>
<span data-ttu-id="f5d1a-143">Utför följande steg i den klassiska Azure portalen för att skapa en (manuell) säkerhetskopiering på begäran för en enskild volym.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-143">Perform the following steps in the Azure classic portal to create an on-demand (manual) backup for a single volume.</span></span>

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a><span data-ttu-id="f5d1a-144">Skapa en anpassad princip för säkerhetskopiering med flera volymer och scheman</span><span class="sxs-lookup"><span data-stu-id="f5d1a-144">Create a custom backup policy with multiple volumes and schedules</span></span>
<span data-ttu-id="f5d1a-145">Utför följande steg i den klassiska Azure portalen för att skapa en anpassad princip för säkerhetskopiering som har flera volymer och scheman.</span><span class="sxs-lookup"><span data-stu-id="f5d1a-145">Perform the following steps in the Azure classic portal to create a custom backup policy that has multiple volumes and schedules.</span></span>

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy.md)]

## <a name="next-steps"></a><span data-ttu-id="f5d1a-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f5d1a-146">Next steps</span></span>
<span data-ttu-id="f5d1a-147">Lär dig mer om [använda StorSimple Manager-tjänsten för att administrera din StorSimple-enhet](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="f5d1a-147">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

