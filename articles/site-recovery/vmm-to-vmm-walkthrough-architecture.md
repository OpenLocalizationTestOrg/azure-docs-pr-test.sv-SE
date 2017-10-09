---
title: "aaaReview hello arkitekturen för Hyper-V-replikering tooa sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över hello arkitektur för att replikera lokala virtuella Hyper-V-datorer tooa sekundär System Center VMM plats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 07099161-4cc7-4f32-8eb9-2a71bbf0750b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 0de4b4e8601116c73e6fd710597ce4e561884368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-hyper-v-replication-tooa-secondary-site"></a><span data-ttu-id="729c7-103">Steg 1: Granska hello arkitekturen för Hyper-V-replikering tooa sekundär plats</span><span class="sxs-lookup"><span data-stu-id="729c7-103">Step 1: Review hello architecture for Hyper-V replication tooa secondary site</span></span>

<span data-ttu-id="729c7-104">Den här artikeln beskriver hello komponenter och processer som är inblandade vid replikering av lokala Hyper-V virtuella datorer (VM) i System Center Virtual Machine Manager (VMM) moln tooa sekundär VMM-plats med hjälp av hello [Azure Site Recovery](site-recovery-overview.md)tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="729c7-104">This article describes hello components and processes involved when replicating on-premises Hyper-V virtual machines (VMs) in System Center Virtual Machine Manager (VMM) clouds, tooa secondary VMM site using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="729c7-105">Bokför eventuella kommentarer längst ned hello av den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="729c7-105">Post any comments at hello bottom of this article, or in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="architectural-components"></a><span data-ttu-id="729c7-106">Arkitekturkomponenter</span><span class="sxs-lookup"><span data-stu-id="729c7-106">Architectural components</span></span>

<span data-ttu-id="729c7-107">Det här är vad du behöver för att replikera virtuella Hyper-V-datorer tooa sekundär VMM-plats.</span><span class="sxs-lookup"><span data-stu-id="729c7-107">Here's what you need for replicating Hyper-V VMs tooa secondary VMM site.</span></span>

<span data-ttu-id="729c7-108">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="729c7-108">**Component**</span></span> | <span data-ttu-id="729c7-109">**Plats**</span><span class="sxs-lookup"><span data-stu-id="729c7-109">**Location**</span></span> | <span data-ttu-id="729c7-110">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="729c7-110">**Details**</span></span>
--- | --- | ---
<span data-ttu-id="729c7-111">**Azure**</span><span class="sxs-lookup"><span data-stu-id="729c7-111">**Azure**</span></span> | <span data-ttu-id="729c7-112">Prenumeration på Azure.</span><span class="sxs-lookup"><span data-stu-id="729c7-112">Subscription in Azure.</span></span> | <span data-ttu-id="729c7-113">Du skapar ett Recovery Services-valv i hello Azure-prenumeration, tooorchestrate och hantera replikering mellan platser i VMM.</span><span class="sxs-lookup"><span data-stu-id="729c7-113">You create a Recovery Services vault in hello Azure subscription, tooorchestrate and manage replication between VMM locations.</span></span>
<span data-ttu-id="729c7-114">**VMM-server**</span><span class="sxs-lookup"><span data-stu-id="729c7-114">**VMM server**</span></span> | <span data-ttu-id="729c7-115">Du behöver en primär och sekundär VMM-plats.</span><span class="sxs-lookup"><span data-stu-id="729c7-115">You need a VMM primary and secondary location.</span></span> | <span data-ttu-id="729c7-116">Vi rekommenderar en VMM-servern i hello primär plats och en i hello sekundär plats</span><span class="sxs-lookup"><span data-stu-id="729c7-116">We recommend a VMM server in hello primary site, and one in hello secondary site</span></span> 
<span data-ttu-id="729c7-117">**Hyper-V-server**</span><span class="sxs-lookup"><span data-stu-id="729c7-117">**Hyper-V server**</span></span> |  <span data-ttu-id="729c7-118">En eller flera Hyper-V-värdservrar i hello primära och sekundära VMM-moln.</span><span class="sxs-lookup"><span data-stu-id="729c7-118">One or more Hyper-V host servers in hello primary and secondary VMM clouds.</span></span> | <span data-ttu-id="729c7-119">Data replikeras mellan hello primära och sekundära Hyper-V-värdservrarna via hello LAN eller VPN med hjälp av Kerberos eller certifikatautentisering.</span><span class="sxs-lookup"><span data-stu-id="729c7-119">Data is replicated between hello primary and secondary Hyper-V host servers over hello LAN or VPN, using Kerberos or certificate authentication.</span></span>  
<span data-ttu-id="729c7-120">**Virtuella Hyper-V-datorer**</span><span class="sxs-lookup"><span data-stu-id="729c7-120">**Hyper-V VMs**</span></span> | <span data-ttu-id="729c7-121">På Hyper-V-värdservern.</span><span class="sxs-lookup"><span data-stu-id="729c7-121">On Hyper-V host server.</span></span> | <span data-ttu-id="729c7-122">hello källa värdservern bör ha minst en virtuell dator som du vill tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="729c7-122">hello source host server should have at least one VM that you want tooreplicate.</span></span>

## <a name="replication-process"></a><span data-ttu-id="729c7-123">Replikeringsprocessen</span><span class="sxs-lookup"><span data-stu-id="729c7-123">Replication process</span></span>

1. <span data-ttu-id="729c7-124">Ställ in hello Azure-konto, skapa ett Recovery Services-valv och ange vad du vill tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="729c7-124">You set up hello Azure account, create a Recovery Services vault, and specify what you want tooreplicate.</span></span>
2. <span data-ttu-id="729c7-125">Du kan konfigurera hello käll- och replikeringsinställningarna, till exempel installera hello Azure Site Recovery-providern på VMM-servrar och hello Microsoft Azure Recovery Services-agenten på varje Hyper-V-värd.</span><span class="sxs-lookup"><span data-stu-id="729c7-125">You configure hello source and target replication settings, which includes installing hello Azure Site Recovery Provider on VMM servers, and hello Microsoft Azure Recovery Services agent on each Hyper-V host.</span></span>
3. <span data-ttu-id="729c7-126">Du kan skapa en replikeringsprincip för hello källan VMM-moln.</span><span class="sxs-lookup"><span data-stu-id="729c7-126">You create a replication policy for hello source VMM cloud.</span></span> <span data-ttu-id="729c7-127">hello principen är tillämpade tooall virtuella datorer på värdar i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="729c7-127">hello policy is applied tooall VMs located on hosts in hello cloud.</span></span>
4. <span data-ttu-id="729c7-128">Du aktiverar replikering för varje VMM och inledande replikering av en virtuell dator sker i enlighet med hello-inställningar som du väljer.</span><span class="sxs-lookup"><span data-stu-id="729c7-128">You enable replication for each VMM, and initial replication of a VM occurs in accordance with hello settings you choose.</span></span>
5. <span data-ttu-id="729c7-129">Efter den första replikeringen börjar replikeringen av deltaändringar.</span><span class="sxs-lookup"><span data-stu-id="729c7-129">After initial replication, replication of delta changes begins.</span></span> <span data-ttu-id="729c7-130">Spårade ändringar för ett objekt lagras i en .hrl-fil.</span><span class="sxs-lookup"><span data-stu-id="729c7-130">Tracked changes for an item are held in a .hrl file.</span></span>


![Lokala tooon lokala](./media/vmm-to-vmm-walkthrough-architecture/arch-onprem-onprem.png)

## <a name="failover-and-failback-process"></a><span data-ttu-id="729c7-132">Processen för redundans och återställning efter fel</span><span class="sxs-lookup"><span data-stu-id="729c7-132">Failover and failback process</span></span>

1. <span data-ttu-id="729c7-133">Du kan köra en planerad eller oplanerad [redundansväxling](site-recovery-failover.md) mellan lokala platser.</span><span class="sxs-lookup"><span data-stu-id="729c7-133">You can run a planned or unplanned [failover](site-recovery-failover.md) between on-premises sites.</span></span> <span data-ttu-id="729c7-134">Om du kör en planerad redundans och virtuella källdatorer stängs tooensure inga data går förlorade.</span><span class="sxs-lookup"><span data-stu-id="729c7-134">If you run a planned failover, then source VMs are shut down tooensure no data loss.</span></span>
2. <span data-ttu-id="729c7-135">Du kan växla över en enskild dator eller skapa [återställningsplaner](site-recovery-create-recovery-plans.md) tooorchestrate växling vid fel på flera datorer.</span><span class="sxs-lookup"><span data-stu-id="729c7-135">You can fail over a single machine, or create [recovery plans](site-recovery-create-recovery-plans.md) tooorchestrate failover of multiple machines.</span></span>
4. <span data-ttu-id="729c7-136">Om du vill utföra en oplanerad växling tooa sekundär plats, efter hello redundans datorer hello sekundär plats har inte aktiverats för skydd eller replikering.</span><span class="sxs-lookup"><span data-stu-id="729c7-136">If you perform an unplanned failover tooa secondary site, after hello failover machines in hello secondary location aren't enabled for protection or replication.</span></span> <span data-ttu-id="729c7-137">Om du körde en planerad redundans, efter hello redundans skydda datorer i hello sekundär plats.</span><span class="sxs-lookup"><span data-stu-id="729c7-137">If you ran a planned failover, after hello failover, machines in hello secondary location are protected.</span></span>
5. <span data-ttu-id="729c7-138">Sedan kan du spara hello redundans toostart använder hello arbetsbelastning från hello replikerade virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="729c7-138">Then, you commit hello failover toostart accessing hello workload from hello replica VM.</span></span>
6. <span data-ttu-id="729c7-139">När den primära platsen är tillgänglig igen kan initiera du omvänd replikering tooreplicate från hello sekundär plats toohello primära.</span><span class="sxs-lookup"><span data-stu-id="729c7-139">When your primary site is available again, you initiate reverse replication tooreplicate from hello secondary site toohello primary.</span></span> <span data-ttu-id="729c7-140">Omvänd replikering ger hello virtuella datorer i ett skyddat läge, men hello sekundärt datacenter är fortfarande aktiva hello-plats.</span><span class="sxs-lookup"><span data-stu-id="729c7-140">Reverse replication brings hello virtual machines into a protected state, but hello secondary datacenter is still hello active location.</span></span>
7. <span data-ttu-id="729c7-141">toomake hello primära platsen till aktiv plats för hello igen, kan du starta en planerad redundansväxling från sekundär tooprimary, följt av en annan omvänd replikering.</span><span class="sxs-lookup"><span data-stu-id="729c7-141">toomake hello primary site into hello active location again, you initiate a planned failover from secondary tooprimary, followed by another reverse replication.</span></span>



## <a name="next-steps"></a><span data-ttu-id="729c7-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="729c7-142">Next steps</span></span>

<span data-ttu-id="729c7-143">Gå för[steg 2: granska hello förutsättningar och begränsningar](vmm-to-vmm-walkthrough-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="729c7-143">Go too[Step 2: Review hello prerequisites and limitations](vmm-to-vmm-walkthrough-prerequisites.md).</span></span>
