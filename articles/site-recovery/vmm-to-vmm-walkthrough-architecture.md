---
title: "Granska arkitekturen för Hyper-V-replikering till en sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över arkitekturen för replikering av lokala virtuella Hyper-V-datorer till en sekundär System Center VMM-plats med Azure Site Recovery."
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
ms.openlocfilehash: b78cd0d5a5395873afaddc8856004775f447e8ea
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="step-1-review-the-architecture-for-hyper-v-replication-to-a-secondary-site"></a><span data-ttu-id="6c903-103">Steg 1: Granska arkitekturen för Hyper-V-replikering till en sekundär plats</span><span class="sxs-lookup"><span data-stu-id="6c903-103">Step 1: Review the architecture for Hyper-V replication to a secondary site</span></span>

<span data-ttu-id="6c903-104">Den här artikeln beskriver de komponenter och processer som ingår i replikeringen av lokala virtuella Hyper-V-datorer i System Center Virtual Machine Manager-moln (VMM) till en sekundär VMM-plats med tjänsten [Azure Site Recovery](site-recovery-overview.md) på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6c903-104">This article describes the components and processes involved when replicating on-premises Hyper-V virtual machines (VMs) in System Center Virtual Machine Manager (VMM) clouds, to a secondary VMM site using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="6c903-105">Skriv dina kommentarer längst ned i den här artikeln eller i [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="6c903-105">Post any comments at the bottom of this article, or in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="architectural-components"></a><span data-ttu-id="6c903-106">Arkitekturkomponenter</span><span class="sxs-lookup"><span data-stu-id="6c903-106">Architectural components</span></span>

<span data-ttu-id="6c903-107">Du behöver följande för att replikera virtuella Hyper-V-datorer till en sekundär VMM-plats.</span><span class="sxs-lookup"><span data-stu-id="6c903-107">Here's what you need for replicating Hyper-V VMs to a secondary VMM site.</span></span>

<span data-ttu-id="6c903-108">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="6c903-108">**Component**</span></span> | <span data-ttu-id="6c903-109">**Plats**</span><span class="sxs-lookup"><span data-stu-id="6c903-109">**Location**</span></span> | <span data-ttu-id="6c903-110">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="6c903-110">**Details**</span></span>
--- | --- | ---
<span data-ttu-id="6c903-111">**Azure**</span><span class="sxs-lookup"><span data-stu-id="6c903-111">**Azure**</span></span> | <span data-ttu-id="6c903-112">Prenumeration på Azure.</span><span class="sxs-lookup"><span data-stu-id="6c903-112">Subscription in Azure.</span></span> | <span data-ttu-id="6c903-113">Du kan skapa ett Recovery Services-valv i Azure-prenumerationen för att dirigera och hantera replikeringen mellan VMM-platser.</span><span class="sxs-lookup"><span data-stu-id="6c903-113">You create a Recovery Services vault in the Azure subscription, to orchestrate and manage replication between VMM locations.</span></span>
<span data-ttu-id="6c903-114">**VMM-server**</span><span class="sxs-lookup"><span data-stu-id="6c903-114">**VMM server**</span></span> | <span data-ttu-id="6c903-115">Du behöver en primär och sekundär VMM-plats.</span><span class="sxs-lookup"><span data-stu-id="6c903-115">You need a VMM primary and secondary location.</span></span> | <span data-ttu-id="6c903-116">Vi rekommenderar att det finns en VMM-server på den primära platsen och en på den sekundära platsen</span><span class="sxs-lookup"><span data-stu-id="6c903-116">We recommend a VMM server in the primary site, and one in the secondary site</span></span> 
<span data-ttu-id="6c903-117">**Hyper-V-server**</span><span class="sxs-lookup"><span data-stu-id="6c903-117">**Hyper-V server**</span></span> |  <span data-ttu-id="6c903-118">En eller flera Hyper-V-värdservrar i de primära och sekundära VMM-molnen.</span><span class="sxs-lookup"><span data-stu-id="6c903-118">One or more Hyper-V host servers in the primary and secondary VMM clouds.</span></span> | <span data-ttu-id="6c903-119">Data replikeras mellan de primära och sekundära Hyper-V-värdservrarna via LAN eller VPN med hjälp av Kerberos eller certifikatautentisering.</span><span class="sxs-lookup"><span data-stu-id="6c903-119">Data is replicated between the primary and secondary Hyper-V host servers over the LAN or VPN, using Kerberos or certificate authentication.</span></span>  
<span data-ttu-id="6c903-120">**Virtuella Hyper-V-datorer**</span><span class="sxs-lookup"><span data-stu-id="6c903-120">**Hyper-V VMs**</span></span> | <span data-ttu-id="6c903-121">På Hyper-V-värdservern.</span><span class="sxs-lookup"><span data-stu-id="6c903-121">On Hyper-V host server.</span></span> | <span data-ttu-id="6c903-122">Källvärdservern måste ha minst en virtuell dator som du vill replikera.</span><span class="sxs-lookup"><span data-stu-id="6c903-122">The source host server should have at least one VM that you want to replicate.</span></span>

## <a name="replication-process"></a><span data-ttu-id="6c903-123">Replikeringsprocessen</span><span class="sxs-lookup"><span data-stu-id="6c903-123">Replication process</span></span>

1. <span data-ttu-id="6c903-124">Du konfigurerar Azure-kontot, skapar ett Recovery Services-valv och anger vad du vill replikera.</span><span class="sxs-lookup"><span data-stu-id="6c903-124">You set up the Azure account, create a Recovery Services vault, and specify what you want to replicate.</span></span>
2. <span data-ttu-id="6c903-125">Du konfigurerar replikeringsinställningarna för källan och målet. Som en del i det här steget installerar du Azure Site Recovery-providern på VMM-servrarna och Microsoft Azure Recovery Services-agenten på varje Hyper-V-värd.</span><span class="sxs-lookup"><span data-stu-id="6c903-125">You configure the source and target replication settings, which includes installing the Azure Site Recovery Provider on VMM servers, and the Microsoft Azure Recovery Services agent on each Hyper-V host.</span></span>
3. <span data-ttu-id="6c903-126">Du skapar en replikeringsprincip för VMM-källmolnet.</span><span class="sxs-lookup"><span data-stu-id="6c903-126">You create a replication policy for the source VMM cloud.</span></span> <span data-ttu-id="6c903-127">Principen tillämpas på alla virtuella datorer som befinner sig på värdar i molnet.</span><span class="sxs-lookup"><span data-stu-id="6c903-127">The policy is applied to all VMs located on hosts in the cloud.</span></span>
4. <span data-ttu-id="6c903-128">Du aktiverar replikering för varje VMM. Den första replikeringen av en virtuell dator sker baserat på de inställningar du väljer.</span><span class="sxs-lookup"><span data-stu-id="6c903-128">You enable replication for each VMM, and initial replication of a VM occurs in accordance with the settings you choose.</span></span>
5. <span data-ttu-id="6c903-129">Efter den första replikeringen börjar replikeringen av deltaändringar.</span><span class="sxs-lookup"><span data-stu-id="6c903-129">After initial replication, replication of delta changes begins.</span></span> <span data-ttu-id="6c903-130">Spårade ändringar för ett objekt lagras i en .hrl-fil.</span><span class="sxs-lookup"><span data-stu-id="6c903-130">Tracked changes for an item are held in a .hrl file.</span></span>


![Lokal till lokal](./media/vmm-to-vmm-walkthrough-architecture/arch-onprem-onprem.png)

## <a name="failover-and-failback-process"></a><span data-ttu-id="6c903-132">Processen för redundans och återställning efter fel</span><span class="sxs-lookup"><span data-stu-id="6c903-132">Failover and failback process</span></span>

1. <span data-ttu-id="6c903-133">Du kan köra en planerad eller oplanerad [redundansväxling](site-recovery-failover.md) mellan lokala platser.</span><span class="sxs-lookup"><span data-stu-id="6c903-133">You can run a planned or unplanned [failover](site-recovery-failover.md) between on-premises sites.</span></span> <span data-ttu-id="6c903-134">Om du kör en planerad redundansväxling stängs de virtuella källdatorerna av för att säkerställa att inga data går förlorade.</span><span class="sxs-lookup"><span data-stu-id="6c903-134">If you run a planned failover, then source VMs are shut down to ensure no data loss.</span></span>
2. <span data-ttu-id="6c903-135">Du kan redundansväxla en enskild dator eller skapa [återställningsplaner](site-recovery-create-recovery-plans.md) för att samordna redundans för flera datorer.</span><span class="sxs-lookup"><span data-stu-id="6c903-135">You can fail over a single machine, or create [recovery plans](site-recovery-create-recovery-plans.md) to orchestrate failover of multiple machines.</span></span>
4. <span data-ttu-id="6c903-136">Om du utför en oplanerad redundansväxling till en sekundär plats efter att redundansdatorerna på den sekundära platsen inte aktiverats för skydd eller replikering.</span><span class="sxs-lookup"><span data-stu-id="6c903-136">If you perform an unplanned failover to a secondary site, after the failover machines in the secondary location aren't enabled for protection or replication.</span></span> <span data-ttu-id="6c903-137">Om du utför en planerad redundans skyddas datorerna på den sekundära platsen efter redundansen.</span><span class="sxs-lookup"><span data-stu-id="6c903-137">If you ran a planned failover, after the failover, machines in the secondary location are protected.</span></span>
5. <span data-ttu-id="6c903-138">Sedan kan du etablera redundansen för att få åtkomst till arbetsbelastningen från den virtuella replikdatorn.</span><span class="sxs-lookup"><span data-stu-id="6c903-138">Then, you commit the failover to start accessing the workload from the replica VM.</span></span>
6. <span data-ttu-id="6c903-139">När din primära plats är tillgänglig igen, kan du initiera omvänd replikering för att replikera från den sekundära platsen till den primära.</span><span class="sxs-lookup"><span data-stu-id="6c903-139">When your primary site is available again, you initiate reverse replication to replicate from the secondary site to the primary.</span></span> <span data-ttu-id="6c903-140">Omvänd replikering skyddar de virtuella datorerna, men det sekundära datacentret är fortfarande den aktiva platsen.</span><span class="sxs-lookup"><span data-stu-id="6c903-140">Reverse replication brings the virtual machines into a protected state, but the secondary datacenter is still the active location.</span></span>
7. <span data-ttu-id="6c903-141">För att göra den primära platsen till den aktiva platsen igen, initierar du en planerad redundans från sekundär till primär, följt av en till omvänd replikering.</span><span class="sxs-lookup"><span data-stu-id="6c903-141">To make the primary site into the active location again, you initiate a planned failover from secondary to primary, followed by another reverse replication.</span></span>



## <a name="next-steps"></a><span data-ttu-id="6c903-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c903-142">Next steps</span></span>

<span data-ttu-id="6c903-143">Gå till [Step 2: Granska kraven och begränsningarna](vmm-to-vmm-walkthrough-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="6c903-143">Go to [Step 2: Review the prerequisites and limitations](vmm-to-vmm-walkthrough-prerequisites.md).</span></span>
