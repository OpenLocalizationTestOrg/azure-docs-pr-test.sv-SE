---
title: "aaaPlan kapacitet och skalning för Virtuella Hyper-V-replikering (med VMM) tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Använd den här artikeln tooplan kapacitet och skala när replikera virtuella Hyper-V-datorer i VMM moln tooAzure med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 41c3c83e-6b1a-496a-8179-498c552ef0c7
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 9818ada9bb21f60ac00b3894696201b06630cb2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-hyper-v-with-vmm-tooazure-replication"></a><span data-ttu-id="ced55-103">Steg 3: Planera kapacitet och skalning för Hyper-V (med VMM) tooAzure replikering</span><span class="sxs-lookup"><span data-stu-id="ced55-103">Step 3: Plan capacity and scaling for Hyper-V (with VMM) tooAzure replication</span></span>

<span data-ttu-id="ced55-104">När du har granskat hello [kraven för distribution av](vmm-to-azure-walkthrough-prerequisites.md)kan använda den här artikeln toofigure planera för kapacitet och skala vid replikering av lokala virtuella Hyper-V-datorer i System Center Virtual Machine Manager (VMM) moln tooAzure med [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ced55-104">After you've reviewed hello [deployment prerequisites](vmm-to-azure-walkthrough-prerequisites.md), use this article toofigure out planning for capacity and scaling, when replicating on-premises Hyper-V VMs in System Center Virtual Machine Manager (VMM) clouds tooAzure, with [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="ced55-105">När du har läst den här artikeln efter eventuella kommentarer längst ned hello eller tekniska frågor om hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="ced55-105">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="how-do-i-start-capacity-planning"></a><span data-ttu-id="ced55-106">Hur börjar jag kapacitetsplanering?</span><span class="sxs-lookup"><span data-stu-id="ced55-106">How do I start capacity planning?</span></span>


<span data-ttu-id="ced55-107">Du samla in information om din miljö för replikering och planera kapacitet i hello data, tillsammans med hello överväganden markerat i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="ced55-107">You gather information about your replication environment, and then plan capacity using hello data, together with hello considerations highlighted in this article.</span></span>


## <a name="gather-information"></a><span data-ttu-id="ced55-108">Samla in information</span><span class="sxs-lookup"><span data-stu-id="ced55-108">Gather information</span></span>

1. <span data-ttu-id="ced55-109">Samla in information om replikeringsmiljön, inklusive virtuella datorer, diskar per virtuell dator och lagringsutrymme per disk.</span><span class="sxs-lookup"><span data-stu-id="ced55-109">Gather information about your replication environment, including VMs, disks per VMs, and storage per disk.</span></span>
2. <span data-ttu-id="ced55-110">Identifiera dina dagliga (omsättningen) förändringstakten för replikerade data.</span><span class="sxs-lookup"><span data-stu-id="ced55-110">Identify your daily change (churn) rate for replicated data.</span></span> <span data-ttu-id="ced55-111">Hämta hello [Hyper-V kapacitetsplanering verktyget](https://www.microsoft.com/download/details.aspx?id=39057) tooget hello förändringstakten.</span><span class="sxs-lookup"><span data-stu-id="ced55-111">Download hello [Hyper-V capacity planning tool](https://www.microsoft.com/download/details.aspx?id=39057) tooget hello change rate.</span></span> <span data-ttu-id="ced55-112">Vi rekommenderar att du kör det här verktyget över en vecka toocapture medelvärden.</span><span class="sxs-lookup"><span data-stu-id="ced55-112">We recommend you run this tool over a week toocapture averages.</span></span>
 

## <a name="figure-out-capacity"></a><span data-ttu-id="ced55-113">Ta reda på kapaciteten</span><span class="sxs-lookup"><span data-stu-id="ced55-113">Figure out capacity</span></span>

<span data-ttu-id="ced55-114">Baserat på hello information som du har samla du kör hello [Site Recovery Kapacitetsplaneringsverktyget för](http://aka.ms/asr-capacity-planner-excel) tooanalyze din källmiljö arbetsbelastningar, beräkna bandbreddsbehov och serverresurser för hello källplats och hello resurser (virtuella datorer och lagring osv), som du behöver i hello målplats.</span><span class="sxs-lookup"><span data-stu-id="ced55-114">Based on hello information you've gather, you run hello [Site Recovery Capacity Planner Tool](http://aka.ms/asr-capacity-planner-excel) tooanalyze your source environment and workloads, estimate bandwidth needs and server resources for hello source location, and hello resources (virtual machines and storage etc), that you need in hello target location.</span></span> <span data-ttu-id="ced55-115">Du kan köra verktyget hello på ett par olika lägen:</span><span class="sxs-lookup"><span data-stu-id="ced55-115">You can run hello tool in a couple of modes:</span></span>

- <span data-ttu-id="ced55-116">Snabb planering: kör hello-verktyget i det här läget tooget nätverks- och projektioner baserat på ett genomsnittligt antal virtuella datorer, diskar, lagring och förändringstakten.</span><span class="sxs-lookup"><span data-stu-id="ced55-116">Quick planning: Run hello tool in this mode tooget network and server projections based on an average number of VMs, disks, storage, and change rate.</span></span>
- <span data-ttu-id="ced55-117">Detaljerad planering: kör hello-verktyget i det här läget och ange information för varje arbetsbelastning på VM-nivå.</span><span class="sxs-lookup"><span data-stu-id="ced55-117">Detailed planning: Run hello tool in this mode, and provide details of each workload at VM level.</span></span> <span data-ttu-id="ced55-118">Analysera VM-kompatibilitet och får nätverks- och projektioner.</span><span class="sxs-lookup"><span data-stu-id="ced55-118">Analyze VM compatibility and get network and server projections.</span></span>

<span data-ttu-id="ced55-119">Kör nu hello verktyget:</span><span class="sxs-lookup"><span data-stu-id="ced55-119">Now run hello tool:</span></span>

1. <span data-ttu-id="ced55-120">Hämta hello [verktyget](http://aka.ms/asr-capacity-planner-excel)</span><span class="sxs-lookup"><span data-stu-id="ced55-120">Download hello [tool](http://aka.ms/asr-capacity-planner-excel)</span></span>
2. <span data-ttu-id="ced55-121">toorun hello snabb planner, Följ [instruktionerna](site-recovery-capacity-planner.md#run-the-quick-planner), och välj hello scenario **Hyper-V tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="ced55-121">toorun hello quick planner, follow [these instructions](site-recovery-capacity-planner.md#run-the-quick-planner), and select hello scenario **Hyper-V tooAzure**.</span></span>
3. <span data-ttu-id="ced55-122">toorun Hej detaljerad planner, Följ [instruktionerna](site-recovery-capacity-planner.md#run-the-detailed-planner), och välj hello scenario **Hyper-V tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="ced55-122">toorun hello detailed planner, follow [these instructions](site-recovery-capacity-planner.md#run-the-detailed-planner), and select hello scenario **Hyper-V tooAzure**.</span></span>

## <a name="control-network-bandwidth"></a><span data-ttu-id="ced55-123">Kontrollera nätverksbandbredd</span><span class="sxs-lookup"><span data-stu-id="ced55-123">Control network bandwidth</span></span>

<span data-ttu-id="ced55-124">När du har beräknade hello bandbredd som du behöver ha ett antal alternativ för att styra hello mängden bandbredd som används för replikering:</span><span class="sxs-lookup"><span data-stu-id="ced55-124">After you've calculated hello bandwidth you need, you have a couple of options for controlling hello amount of bandwidth used for replication:</span></span>

* <span data-ttu-id="ced55-125">**Begränsa bandbredden**: Hyper-V-trafik som replikeras tooAzure går igenom en specifik Hyper-V-värd.</span><span class="sxs-lookup"><span data-stu-id="ced55-125">**Throttle bandwidth**: Hyper-V traffic that replicates tooAzure goes through a specific Hyper-V host.</span></span> <span data-ttu-id="ced55-126">Du kan begränsa bandbredden på värdservern för hello.</span><span class="sxs-lookup"><span data-stu-id="ced55-126">You can throttle bandwidth on hello host server.</span></span>
* <span data-ttu-id="ced55-127">**Påverka bandbredd**: du kan påverka hello bandbredd som används för replikering med hjälp av några registernycklar.</span><span class="sxs-lookup"><span data-stu-id="ced55-127">**Influence bandwidth**: You can influence hello bandwidth used for replication using a couple of registry keys.</span></span>

### <a name="throttle-bandwidth"></a><span data-ttu-id="ced55-128">Begränsa bandbredden</span><span class="sxs-lookup"><span data-stu-id="ced55-128">Throttle bandwidth</span></span>
1. <span data-ttu-id="ced55-129">Öppna hello Microsoft Azure Backup MMC snapin-modulen på hello Hyper-V-värdservern.</span><span class="sxs-lookup"><span data-stu-id="ced55-129">Open hello Microsoft Azure Backup MMC snap-in on hello Hyper-V host server.</span></span> <span data-ttu-id="ced55-130">Som standard är en genväg till Microsoft Azure Backup finns på hello skrivbordet eller i C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span><span class="sxs-lookup"><span data-stu-id="ced55-130">By default a shortcut for Microsoft Azure Backup is available on hello desktop or in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span></span>
2. <span data-ttu-id="ced55-131">I snapin-modulen hello klickar du på **ändra egenskaper för**.</span><span class="sxs-lookup"><span data-stu-id="ced55-131">In hello snap-in click **Change Properties**.</span></span>
3. <span data-ttu-id="ced55-132">På hello **begränsning** väljer **aktivera Användningsbegränsning för internetbandbredd för säkerhetskopieringsåtgärder**, och ange hello gränserna för arbete och fritid timmar.</span><span class="sxs-lookup"><span data-stu-id="ced55-132">On hello **Throttling** tab select **Enable internet bandwidth usage throttling for backup operations**, and set hello limits for work and non-work hours.</span></span> <span data-ttu-id="ced55-133">Giltigt intervall är från 512 kbit/s too102 Mbit/s per sekund.</span><span class="sxs-lookup"><span data-stu-id="ced55-133">Valid ranges are from 512 Kbps too102 Mbps per second.</span></span>

    ![Begränsa bandbredden](./media/vmm-to-azure-walkthrough-capacity/throttle2.png)

<span data-ttu-id="ced55-135">Du kan också använda hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet tooset begränsning.</span><span class="sxs-lookup"><span data-stu-id="ced55-135">You can also use hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet tooset throttling.</span></span> <span data-ttu-id="ced55-136">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="ced55-136">Here's a sample:</span></span>

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

<span data-ttu-id="ced55-137">**Set-OBMachineSetting -NoThrottle** anger att ingen begränsning krävs.</span><span class="sxs-lookup"><span data-stu-id="ced55-137">**Set-OBMachineSetting -NoThrottle** indicates that no throttling is required.</span></span>

### <a name="influence-network-bandwidth"></a><span data-ttu-id="ced55-138">Påverka nätverkets bandbredd</span><span class="sxs-lookup"><span data-stu-id="ced55-138">Influence network bandwidth</span></span>
1. <span data-ttu-id="ced55-139">I registret hello navigera för**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.</span><span class="sxs-lookup"><span data-stu-id="ced55-139">In hello registry navigate too**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.</span></span>
   * <span data-ttu-id="ced55-140">tooinfluence hello bandbredd trafik på en replikering disk, ändra hello värdet hello **UploadThreadsPerVM**, eller skapa hello nyckeln om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="ced55-140">tooinfluence hello bandwidth traffic on a replicating disk, modify hello value hello **UploadThreadsPerVM**, or create hello key if it doesn't exist.</span></span>
   * <span data-ttu-id="ced55-141">tooinfluence hello bandbredd för redundanstrafik från Azure, ändra värdet för hello **DownloadThreadsPerVM**.</span><span class="sxs-lookup"><span data-stu-id="ced55-141">tooinfluence hello bandwidth for failback traffic from Azure, modify hello value **DownloadThreadsPerVM**.</span></span>
2. <span data-ttu-id="ced55-142">hello standardvärdet är 4.</span><span class="sxs-lookup"><span data-stu-id="ced55-142">hello default value is 4.</span></span> <span data-ttu-id="ced55-143">I ett ”överetablerat” nätverk bör registernycklarna ändras från hello standardvärden.</span><span class="sxs-lookup"><span data-stu-id="ced55-143">In an “overprovisioned” network, these registry keys should be changed from hello default values.</span></span> <span data-ttu-id="ced55-144">hello maximala är 32.</span><span class="sxs-lookup"><span data-stu-id="ced55-144">hello maximum is 32.</span></span> <span data-ttu-id="ced55-145">Övervaka trafik toooptimize hello värde.</span><span class="sxs-lookup"><span data-stu-id="ced55-145">Monitor traffic toooptimize hello value.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ced55-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ced55-146">Next steps</span></span>

<span data-ttu-id="ced55-147">Gå för[steg 4: Planera nätverk](vmm-to-azure-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="ced55-147">Go too[Step 4: Plan networking](vmm-to-azure-walkthrough-network.md).</span></span>
