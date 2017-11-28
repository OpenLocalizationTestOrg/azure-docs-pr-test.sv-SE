---
title: "Planera kapacitet och skalning för Virtuella Hyper-V-replikering (med VMM) till Azure med Azure Site Recovery | Microsoft Docs"
description: "Använd den här artikeln för att planera kapaciteten och skala vid replikering av Hyper-V virtuella datorer i VMM-moln till Azure med Azure Site Recovery"
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
ms.openlocfilehash: ab1dc21a829140f8cd2e57837d83a05b0d71bcdf
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-hyper-v-with-vmm-to-azure-replication"></a><span data-ttu-id="68cf0-103">Steg 3: Planera kapacitet och skalning för Hyper-V (med VMM) till Azure-replikering</span><span class="sxs-lookup"><span data-stu-id="68cf0-103">Step 3: Plan capacity and scaling for Hyper-V (with VMM) to Azure replication</span></span>

<span data-ttu-id="68cf0-104">När du har granskat den [kraven för distribution av](vmm-to-azure-walkthrough-prerequisites.md), använda den här artikeln för att ta reda på planera för kapacitet och skala vid replikering av lokala Hyper-V virtuella datorer i System Center Virtual Machine Manager (VMM)-moln till Azure med [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="68cf0-104">After you've reviewed the [deployment prerequisites](vmm-to-azure-walkthrough-prerequisites.md), use this article to figure out planning for capacity and scaling, when replicating on-premises Hyper-V VMs in System Center Virtual Machine Manager (VMM) clouds to Azure, with [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="68cf0-105">När du har läst den här artikeln efter eventuella kommentarer längst ned eller tekniska frågor om den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="68cf0-105">After reading this article, post any comments at the bottom, or ask technical questions on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="how-do-i-start-capacity-planning"></a><span data-ttu-id="68cf0-106">Hur börjar jag kapacitetsplanering?</span><span class="sxs-lookup"><span data-stu-id="68cf0-106">How do I start capacity planning?</span></span>


<span data-ttu-id="68cf0-107">Du samla in information om replikeringsmiljön och planera kapacitet med hjälp av data, tillsammans med överväganden markerat i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="68cf0-107">You gather information about your replication environment, and then plan capacity using the data, together with the considerations highlighted in this article.</span></span>


## <a name="gather-information"></a><span data-ttu-id="68cf0-108">Samla in information</span><span class="sxs-lookup"><span data-stu-id="68cf0-108">Gather information</span></span>

1. <span data-ttu-id="68cf0-109">Samla in information om replikeringsmiljön, inklusive virtuella datorer, diskar per virtuell dator och lagringsutrymme per disk.</span><span class="sxs-lookup"><span data-stu-id="68cf0-109">Gather information about your replication environment, including VMs, disks per VMs, and storage per disk.</span></span>
2. <span data-ttu-id="68cf0-110">Identifiera dina dagliga (omsättningen) förändringstakten för replikerade data.</span><span class="sxs-lookup"><span data-stu-id="68cf0-110">Identify your daily change (churn) rate for replicated data.</span></span> <span data-ttu-id="68cf0-111">Hämta den [Hyper-V kapacitetsplanering verktyget](https://www.microsoft.com/download/details.aspx?id=39057) få förändringstakten.</span><span class="sxs-lookup"><span data-stu-id="68cf0-111">Download the [Hyper-V capacity planning tool](https://www.microsoft.com/download/details.aspx?id=39057) to get the change rate.</span></span> <span data-ttu-id="68cf0-112">Vi rekommenderar att du kör det här verktyget över en vecka att avbilda medelvärden.</span><span class="sxs-lookup"><span data-stu-id="68cf0-112">We recommend you run this tool over a week to capture averages.</span></span>
 

## <a name="figure-out-capacity"></a><span data-ttu-id="68cf0-113">Ta reda på kapaciteten</span><span class="sxs-lookup"><span data-stu-id="68cf0-113">Figure out capacity</span></span>

<span data-ttu-id="68cf0-114">Utifrån den information som du har samla kan du köra den [Site Recovery Kapacitetsplaneringsverktyget för](http://aka.ms/asr-capacity-planner-excel) beräkna bandbreddsbehov och serverresurser för källplatsen och resurser (virtuella datorer och lagring osv), som du behöver på målplatsen för att analysera dina källmiljön och arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="68cf0-114">Based on the information you've gather, you run the [Site Recovery Capacity Planner Tool](http://aka.ms/asr-capacity-planner-excel) to analyze your source environment and workloads, estimate bandwidth needs and server resources for the source location, and the resources (virtual machines and storage etc), that you need in the target location.</span></span> <span data-ttu-id="68cf0-115">Du kan köra verktyget på ett par olika lägen:</span><span class="sxs-lookup"><span data-stu-id="68cf0-115">You can run the tool in a couple of modes:</span></span>

- <span data-ttu-id="68cf0-116">Snabb planering: kör verktyget i det här läget för att hämta nätverks- och projektioner baserat på ett genomsnittligt antal virtuella datorer, diskar, lagring och förändringstakten.</span><span class="sxs-lookup"><span data-stu-id="68cf0-116">Quick planning: Run the tool in this mode to get network and server projections based on an average number of VMs, disks, storage, and change rate.</span></span>
- <span data-ttu-id="68cf0-117">Detaljerad planering: köra verktyget i det här läget och ger information för varje arbetsbelastning på VM-nivå.</span><span class="sxs-lookup"><span data-stu-id="68cf0-117">Detailed planning: Run the tool in this mode, and provide details of each workload at VM level.</span></span> <span data-ttu-id="68cf0-118">Analysera VM-kompatibilitet och får nätverks- och projektioner.</span><span class="sxs-lookup"><span data-stu-id="68cf0-118">Analyze VM compatibility and get network and server projections.</span></span>

<span data-ttu-id="68cf0-119">Nu köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="68cf0-119">Now run the tool:</span></span>

1. <span data-ttu-id="68cf0-120">Hämta den [verktyget](http://aka.ms/asr-capacity-planner-excel)</span><span class="sxs-lookup"><span data-stu-id="68cf0-120">Download the [tool](http://aka.ms/asr-capacity-planner-excel)</span></span>
2. <span data-ttu-id="68cf0-121">Så här kör du snabbt planner [instruktionerna](site-recovery-capacity-planner.md#run-the-quick-planner), och välj scenariot **Hyper-V till Azure**.</span><span class="sxs-lookup"><span data-stu-id="68cf0-121">To run the quick planner, follow [these instructions](site-recovery-capacity-planner.md#run-the-quick-planner), and select the scenario **Hyper-V to Azure**.</span></span>
3. <span data-ttu-id="68cf0-122">Så här kör du detaljerad planner [instruktionerna](site-recovery-capacity-planner.md#run-the-detailed-planner), och välj scenariot **Hyper-V till Azure**.</span><span class="sxs-lookup"><span data-stu-id="68cf0-122">To run the detailed planner, follow [these instructions](site-recovery-capacity-planner.md#run-the-detailed-planner), and select the scenario **Hyper-V to Azure**.</span></span>

## <a name="control-network-bandwidth"></a><span data-ttu-id="68cf0-123">Kontrollera nätverksbandbredd</span><span class="sxs-lookup"><span data-stu-id="68cf0-123">Control network bandwidth</span></span>

<span data-ttu-id="68cf0-124">När du har beräknat bandbredd som du behöver ha ett antal alternativ för att styra hur mycket bandbredd som används för replikering:</span><span class="sxs-lookup"><span data-stu-id="68cf0-124">After you've calculated the bandwidth you need, you have a couple of options for controlling the amount of bandwidth used for replication:</span></span>

* <span data-ttu-id="68cf0-125">**Begränsa bandbredden**: Hyper-V-trafik som replikeras till Azure går igenom en specifik Hyper-V-värd.</span><span class="sxs-lookup"><span data-stu-id="68cf0-125">**Throttle bandwidth**: Hyper-V traffic that replicates to Azure goes through a specific Hyper-V host.</span></span> <span data-ttu-id="68cf0-126">Du kan begränsa bandbredden på värdservern.</span><span class="sxs-lookup"><span data-stu-id="68cf0-126">You can throttle bandwidth on the host server.</span></span>
* <span data-ttu-id="68cf0-127">**Påverka bandbredd**: du kan påverka den bandbredd som används för replikering med hjälp av några registernycklar.</span><span class="sxs-lookup"><span data-stu-id="68cf0-127">**Influence bandwidth**: You can influence the bandwidth used for replication using a couple of registry keys.</span></span>

### <a name="throttle-bandwidth"></a><span data-ttu-id="68cf0-128">Begränsa bandbredden</span><span class="sxs-lookup"><span data-stu-id="68cf0-128">Throttle bandwidth</span></span>
1. <span data-ttu-id="68cf0-129">Öppna snapin-modulen Microsoft Azure Backup MMC på Hyper-V-värdservern.</span><span class="sxs-lookup"><span data-stu-id="68cf0-129">Open the Microsoft Azure Backup MMC snap-in on the Hyper-V host server.</span></span> <span data-ttu-id="68cf0-130">Som standard finns det en genväg till Microsoft Azure Backup på skrivbordet eller i C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span><span class="sxs-lookup"><span data-stu-id="68cf0-130">By default a shortcut for Microsoft Azure Backup is available on the desktop or in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span></span>
2. <span data-ttu-id="68cf0-131">Klicka på **Ändra egenskaper** i snapin-modulen.</span><span class="sxs-lookup"><span data-stu-id="68cf0-131">In the snap-in click **Change Properties**.</span></span>
3. <span data-ttu-id="68cf0-132">På fliken **Begränsning** väljer du **Aktivera användningsbegränsning för Internetbandbredd för säkerhetskopieringsåtgärder** och ange begränsningarna för arbetstid och övrig tid.</span><span class="sxs-lookup"><span data-stu-id="68cf0-132">On the **Throttling** tab select **Enable internet bandwidth usage throttling for backup operations**, and set the limits for work and non-work hours.</span></span> <span data-ttu-id="68cf0-133">Giltiga intervall är från 512 kbit/s till 102 Mbit/s.</span><span class="sxs-lookup"><span data-stu-id="68cf0-133">Valid ranges are from 512 Kbps to 102 Mbps per second.</span></span>

    ![Begränsa bandbredden](./media/vmm-to-azure-walkthrough-capacity/throttle2.png)

<span data-ttu-id="68cf0-135">Du kan också ange begränsningar med hjälp av cmdleten [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx).</span><span class="sxs-lookup"><span data-stu-id="68cf0-135">You can also use the [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet to set throttling.</span></span> <span data-ttu-id="68cf0-136">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="68cf0-136">Here's a sample:</span></span>

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

<span data-ttu-id="68cf0-137">**Set-OBMachineSetting -NoThrottle** anger att ingen begränsning krävs.</span><span class="sxs-lookup"><span data-stu-id="68cf0-137">**Set-OBMachineSetting -NoThrottle** indicates that no throttling is required.</span></span>

### <a name="influence-network-bandwidth"></a><span data-ttu-id="68cf0-138">Påverka nätverkets bandbredd</span><span class="sxs-lookup"><span data-stu-id="68cf0-138">Influence network bandwidth</span></span>
1. <span data-ttu-id="68cf0-139">Gå till **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication** i registret.</span><span class="sxs-lookup"><span data-stu-id="68cf0-139">In the registry navigate to **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.</span></span>
   * <span data-ttu-id="68cf0-140">Om du vill påverka bandbredd trafiken på en replikering disk ändrar du värdet i **UploadThreadsPerVM**, eller skapa nyckeln om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="68cf0-140">To influence the bandwidth traffic on a replicating disk, modify the value the **UploadThreadsPerVM**, or create the key if it doesn't exist.</span></span>
   * <span data-ttu-id="68cf0-141">Ändra värdet för att påverka bandbredden för redundanstrafik från Azure **DownloadThreadsPerVM**.</span><span class="sxs-lookup"><span data-stu-id="68cf0-141">To influence the bandwidth for failback traffic from Azure, modify the value **DownloadThreadsPerVM**.</span></span>
2. <span data-ttu-id="68cf0-142">Standardvärdet är 4.</span><span class="sxs-lookup"><span data-stu-id="68cf0-142">The default value is 4.</span></span> <span data-ttu-id="68cf0-143">I ett ”överetablerat” nätverk bör du ändra registernycklarnas standardvärden.</span><span class="sxs-lookup"><span data-stu-id="68cf0-143">In an “overprovisioned” network, these registry keys should be changed from the default values.</span></span> <span data-ttu-id="68cf0-144">Det högsta antalet är 32.</span><span class="sxs-lookup"><span data-stu-id="68cf0-144">The maximum is 32.</span></span> <span data-ttu-id="68cf0-145">Övervaka trafiken för att optimera värdet.</span><span class="sxs-lookup"><span data-stu-id="68cf0-145">Monitor traffic to optimize the value.</span></span>

## <a name="next-steps"></a><span data-ttu-id="68cf0-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="68cf0-146">Next steps</span></span>

<span data-ttu-id="68cf0-147">Gå till [steg 4: Planera nätverk](vmm-to-azure-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="68cf0-147">Go to [Step 4: Plan networking](vmm-to-azure-walkthrough-network.md).</span></span>
