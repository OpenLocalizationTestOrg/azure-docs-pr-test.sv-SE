---
title: aaaUse Azure Backup tooreplace infrastrukturen band | Microsoft Docs
description: "Lär dig hur ger Azure Backup band-liknande semantik som gör att du toobackup och återställa data i Azure"
services: backup
documentationcenter: 
author: trinadhk
manager: vijayts
editor: 
ms.assetid: 2e1bb67d-986c-4437-8056-3a63169b4214
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 1/10/2017
ms.author: saurse;trinadhk;markgal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4c5b095d95d39267c54b1eed9427bda09658bb94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-your-long-term-storage-from-tape-toohello-azure-cloud"></a><span data-ttu-id="7e3fc-103">Flytta dina långsiktig lagring från band toohello Azure-molnet</span><span class="sxs-lookup"><span data-stu-id="7e3fc-103">Move your long-term storage from tape toohello Azure cloud</span></span>
<span data-ttu-id="7e3fc-104">Azure Backup och System Center Data Protection Manager-kunder kan:</span><span class="sxs-lookup"><span data-stu-id="7e3fc-104">Azure Backup and System Center Data Protection Manager customers can:</span></span>

* <span data-ttu-id="7e3fc-105">Säkerhetskopiera data i scheman som bäst passar hello organisationens behov.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-105">Back up data in schedules which best suit hello organizational needs.</span></span>
* <span data-ttu-id="7e3fc-106">Behåll hello säkerhetskopierade informationen under längre perioder</span><span class="sxs-lookup"><span data-stu-id="7e3fc-106">Retain hello backup data for longer periods</span></span>
* <span data-ttu-id="7e3fc-107">Se Azure som en del av deras långsiktig kvarhållning måste (istället för band).</span><span class="sxs-lookup"><span data-stu-id="7e3fc-107">Make Azure a part of their long-term retention needs (instead of tape).</span></span>

<span data-ttu-id="7e3fc-108">Den här artikeln förklarar hur kunder kan aktivera principer för säkerhetskopiering och kvarhållning.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-108">This article explains how customers can enable backup and retention policies.</span></span> <span data-ttu-id="7e3fc-109">Kunder som använder band tooaddress lång-sikt-samtidigt måste nu har ett kraftfullt och genomförbart alternativ med hello tillgänglighet för den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-109">Customers who use tapes tooaddress their long-term-retention needs now have a powerful and viable alternative with hello availability of this feature.</span></span> <span data-ttu-id="7e3fc-110">hello funktionen är aktiverad i hello senaste versionen av hello Azure Backup (som är tillgänglig [här](http://aka.ms/azurebackup_agent)).</span><span class="sxs-lookup"><span data-stu-id="7e3fc-110">hello feature is enabled in hello latest release of hello Azure Backup (which is available [here](http://aka.ms/azurebackup_agent)).</span></span> <span data-ttu-id="7e3fc-111">System Center DPM-kunder måste uppdatera till, åtminstone, DPM 2012 R2 UR5 innan du använder DPM med hello Azure Backup-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-111">System Center DPM customers must update to, at least, DPM 2012 R2 UR5 before using DPM with hello Azure Backup service.</span></span>

## <a name="what-is-hello-backup-schedule"></a><span data-ttu-id="7e3fc-112">Vad är hello schema för säkerhetskopiering?</span><span class="sxs-lookup"><span data-stu-id="7e3fc-112">What is hello Backup Schedule?</span></span>
<span data-ttu-id="7e3fc-113">schemat för säkerhetskopiering av hello anger hello frekvens av hello säkerhetskopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-113">hello backup schedule indicates hello frequency of hello backup operation.</span></span> <span data-ttu-id="7e3fc-114">Till exempel ange hello inställningarna i följande skärmbild hello säkerhetskopieringar tas dagligen vid 18: 00 och vid midnatt.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-114">For example, hello settings in hello following screen indicate that backups are taken daily at 6pm and at midnight.</span></span>

![Dagsschema](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

<span data-ttu-id="7e3fc-116">Kunder kan även schemalägga en säkerhetskopiering varje vecka.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-116">Customers can also schedule a weekly backup.</span></span> <span data-ttu-id="7e3fc-117">Till exempel indikera hello inställningarna i följande skärmbild hello att säkerhetskopieringar vidtas för varje alternativ söndag & onsdag på 9:30 och 1:00:00.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-117">For example, hello settings in hello following screen indicate that backups are taken every alternate Sunday & Wednesday at 9:30AM and 1:00AM.</span></span>

![Veckoschema](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-hello-retention-policy"></a><span data-ttu-id="7e3fc-119">Vad är hello bevarandeprincip?</span><span class="sxs-lookup"><span data-stu-id="7e3fc-119">What is hello Retention Policy?</span></span>
<span data-ttu-id="7e3fc-120">hello bevarandeprincip anger hello varaktighet som hello säkerhetskopiering måste lagras.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-120">hello retention policy specifies hello duration for which hello backup must be stored.</span></span> <span data-ttu-id="7e3fc-121">Kunder kan i stället för att bara ange en ”platt policy” för alla återställningspunkter för säkerhetskopiering, ange olika bevarandeprinciper baserat på när hello säkerhetskopia görs.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-121">Rather than just specifying a “flat policy” for all backup points, customers can specify different retention policies based on when hello backup is taken.</span></span> <span data-ttu-id="7e3fc-122">Till exempel bevaras hello säkerhetskopieringspunkt tas dagligen, som fungerar som en operativa återställningspunkt i 90 dagar.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-122">For example, hello backup point taken daily, which serves as an operational recovery point, is preserved for 90 days.</span></span> <span data-ttu-id="7e3fc-123">Hej säkerhetskopieringspunkt vidtas hello slutet av varje kvartal granskningssyfte bevaras under en längre period.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-123">hello backup point taken at hello end of each quarter for audit purposes is preserved for a longer duration.</span></span>

![Bevarandeprincip](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

<span data-ttu-id="7e3fc-125">hello Totalt antal ”bevarandepunkter” anges i den här principen är 90 (dagliga poäng) + 40 (en varje kvartal för 10 år) = 130.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-125">hello total number of “retention points” specified in this policy is 90 (daily points) + 40 (one each quarter for 10 years) = 130.</span></span>

## <a name="example--putting-both-together"></a><span data-ttu-id="7e3fc-126">Exempel – kombinera båda</span><span class="sxs-lookup"><span data-stu-id="7e3fc-126">Example – Putting both together</span></span>
![Exempel skärmen](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. <span data-ttu-id="7e3fc-128">**Dagliga bevarandeprincip**: säkerhetskopieringar tas dagligen sparas i sju dagar.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-128">**Daily retention policy**: Backups taken daily are stored for seven days.</span></span>
2. <span data-ttu-id="7e3fc-129">**Varje vecka bevarandeprincip**: säkerhetskopior som gjorts varje dag vid midnatt och 18: 00 lördag bevaras i fyra veckor</span><span class="sxs-lookup"><span data-stu-id="7e3fc-129">**Weekly retention policy**: Backups taken every day at midnight and 6PM Saturday are preserved for four weeks</span></span>
3. <span data-ttu-id="7e3fc-130">**Månatliga bevarandeprincip**: bevaras säkerhetskopior som har gjorts vid midnatt och 18: 00 på hello senaste lördag i varje månad i 12 månader</span><span class="sxs-lookup"><span data-stu-id="7e3fc-130">**Monthly retention policy**: Backups taken at midnight and 6pm on hello last Saturday of each month are preserved for 12 months</span></span>
4. <span data-ttu-id="7e3fc-131">**Årlig bevarandeprincip**: säkerhetskopior som har gjorts vid midnatt på hello senaste lördag varje mars bevaras för 10 år</span><span class="sxs-lookup"><span data-stu-id="7e3fc-131">**Yearly retention policy**: Backups taken at midnight on hello last Saturday of every March are preserved for 10 years</span></span>

<span data-ttu-id="7e3fc-132">Totalt antal ”bevarandepunkter” hello (poäng som en kund kan återställa data) i hello föregående diagram beräknas enligt följande:</span><span class="sxs-lookup"><span data-stu-id="7e3fc-132">hello total number of “retention points” (points from which a customer can restore data) in hello preceding diagram is computed as follows:</span></span>

* <span data-ttu-id="7e3fc-133">två hanteringsplatser per dag för sju dagar = 14 återställningspunkter</span><span class="sxs-lookup"><span data-stu-id="7e3fc-133">two points per day for seven days = 14 recovery points</span></span>
* <span data-ttu-id="7e3fc-134">två hanteringsplatser per vecka för fyra veckor = 8 återställningspunkter</span><span class="sxs-lookup"><span data-stu-id="7e3fc-134">two points per week for four weeks = 8 recovery points</span></span>
* <span data-ttu-id="7e3fc-135">två hanteringsplatser per månad i 12 månader = 24 återställningspunkter</span><span class="sxs-lookup"><span data-stu-id="7e3fc-135">two points per month for 12 months = 24 recovery points</span></span>
* <span data-ttu-id="7e3fc-136">en punkt per år och 10 år = 10 recovery pekar</span><span class="sxs-lookup"><span data-stu-id="7e3fc-136">one point per year per 10 years = 10 recovery points</span></span>

<span data-ttu-id="7e3fc-137">hello Totalt antal återställningspunkter är 56.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-137">hello total number of recovery points is 56.</span></span>

> [!NOTE]
> <span data-ttu-id="7e3fc-138">Azure-säkerhetskopiering har inte en begränsning på antalet återställningspunkter.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-138">Azure backup doesn't have a restriction on number of recovery points.</span></span>
>
>

## <a name="advanced-configuration"></a><span data-ttu-id="7e3fc-139">Avancerad konfiguration</span><span class="sxs-lookup"><span data-stu-id="7e3fc-139">Advanced configuration</span></span>
<span data-ttu-id="7e3fc-140">Genom att klicka på **ändra** i hello föregående skärm, kunder har större flexibilitet att ange scheman.</span><span class="sxs-lookup"><span data-stu-id="7e3fc-140">By clicking **Modify** in hello preceding screen, customers have further flexibility in specifying retention schedules.</span></span>

![Ändra](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a><span data-ttu-id="7e3fc-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e3fc-142">Next Steps</span></span>
<span data-ttu-id="7e3fc-143">Mer information om Azure Backup finns:</span><span class="sxs-lookup"><span data-stu-id="7e3fc-143">For more information about Azure Backup, see:</span></span>

* [<span data-ttu-id="7e3fc-144">Introduktion tooAzure säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="7e3fc-144">Introduction tooAzure Backup</span></span>](backup-introduction-to-azure-backup.md)
* [<span data-ttu-id="7e3fc-145">Prova Azure-säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="7e3fc-145">Try Azure Backup</span></span>](backup-try-azure-backup-in-10-mins.md)
