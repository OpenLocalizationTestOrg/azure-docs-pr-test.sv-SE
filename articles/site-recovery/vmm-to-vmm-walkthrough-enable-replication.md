---
title: "aaaEnable Hyper-V-replikering tooa sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur tooenable replikering för Hyper-V virtuella datorer replikeras tooa sekundär System Center VMM webbplats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d8782d14-9fef-4396-8912-ff945efc851b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: b4484e0118cb23f343187fe867d9795d30926baf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-enable-replication-tooa-secondary-site-for-hyper-v-vms"></a><span data-ttu-id="1eef8-103">Steg 9: Aktivera replikering tooa sekundär plats för Hyper-V virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="1eef8-103">Step 9: Enable replication tooa secondary site for Hyper-V VMs</span></span>


<span data-ttu-id="1eef8-104">När du har installerat en replikeringsprincip, Använd den här artikeln tooenable replikering tooa sekundär System Center Virtual Machine Manager (VMM) webbplatsen för lokala Hyper-V virtuella datorer (VM) med [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1eef8-104">After setting up a replication policy, use this article tooenable replication tooa secondary System Center Virtual Machine Manager (VMM) site for on-premises Hyper-V virtual machines (VM), using [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="1eef8-105">När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="1eef8-105">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="select-vms-tooreplicate"></a><span data-ttu-id="1eef8-106">Välj tooreplicate för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="1eef8-106">Select VMs tooreplicate</span></span>

1. <span data-ttu-id="1eef8-107">Klicka på **Steg 2: Replikera program** > **Källa**.</span><span class="sxs-lookup"><span data-stu-id="1eef8-107">Click **Step 2: Replicate application** > **Source**.</span></span> 

    ![Aktivera replikering](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication1.png)

2. <span data-ttu-id="1eef8-109">I **källa**hello VMM-servern och välj hello molnet vilka hello Hyper-V-värdar som du vill tooreplicate finns.</span><span class="sxs-lookup"><span data-stu-id="1eef8-109">In **Source**, select hello VMM server, and hello cloud in which hello Hyper-V hosts you want tooreplicate are located.</span></span> <span data-ttu-id="1eef8-110">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1eef8-110">Then click **OK**.</span></span>

    ![Aktivera replikering](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication2.png)
3. <span data-ttu-id="1eef8-112">I **mål**, kontrollera hello sekundär VMM-servern och molnet.</span><span class="sxs-lookup"><span data-stu-id="1eef8-112">In **Target**, verify hello secondary VMM server and cloud.</span></span>
4. <span data-ttu-id="1eef8-113">I **virtuella datorer**, Välj hello virtuella datorer du vill tooprotect hello-listan.</span><span class="sxs-lookup"><span data-stu-id="1eef8-113">In **Virtual machines**, select hello VMs you want tooprotect from hello list.</span></span>

    ![Aktivera skydd för en virtuell dator](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication5.png)

<span data-ttu-id="1eef8-115">Du kan följa förloppet för hello **Aktivera skydd** åtgärden i **jobb** > **Site Recovery-jobb**.</span><span class="sxs-lookup"><span data-stu-id="1eef8-115">You can track progress of hello **Enable Protection** action in **Jobs** > **Site Recovery jobs**.</span></span> <span data-ttu-id="1eef8-116">Efter hello **Slutför skydd** jobbet är slutfört, hello inledande replikeringen är klar och hello virtuella datorn är redo för redundans.</span><span class="sxs-lookup"><span data-stu-id="1eef8-116">After hello **Finalize Protection** job completes, hello initial replication is complete, and hello virtual machine is ready for failover.</span></span>

<span data-ttu-id="1eef8-117">Tänk på följande:</span><span class="sxs-lookup"><span data-stu-id="1eef8-117">Note that:</span></span>

- <span data-ttu-id="1eef8-118">Du kan också aktivera skydd för virtuella datorer i hello VMM-konsolen.</span><span class="sxs-lookup"><span data-stu-id="1eef8-118">You can also enable protection for virtual machines in hello VMM console.</span></span> <span data-ttu-id="1eef8-119">Klicka på **Aktivera skydd** hello verktygsfältet i hello egenskaper för virtuell dator > **Azure Site Recovery** fliken.</span><span class="sxs-lookup"><span data-stu-id="1eef8-119">Click **Enable Protection** on hello toolbar in hello virtual machine properties > **Azure Site Recovery** tab.</span></span>
- <span data-ttu-id="1eef8-120">När du har aktiverat replikering, kan du visa egenskaperna för hello VM i **replikerade objekt**.</span><span class="sxs-lookup"><span data-stu-id="1eef8-120">After you've enabled replication, you can view properties for hello VM in **Replicated Items**.</span></span> <span data-ttu-id="1eef8-121">På hello **Essentials** instrumentpanel, visas information om hello replikeringsprincip för hello VM och dess status.</span><span class="sxs-lookup"><span data-stu-id="1eef8-121">On hello **Essentials** dashboard, you can see information about hello replication policy for hello VM and its status.</span></span> <span data-ttu-id="1eef8-122">Klicka på **egenskaper** för mer information.</span><span class="sxs-lookup"><span data-stu-id="1eef8-122">Click **Properties** for more details.</span></span>

## <a name="onboard-existing-vms"></a><span data-ttu-id="1eef8-123">Publicera befintliga virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="1eef8-123">Onboard existing VMs</span></span>

<span data-ttu-id="1eef8-124">Om du har befintliga virtuella datorer i VMM som replikerar med Hyper-V-replikering, kan du publicera dem för Azure Site Recovery replikering på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1eef8-124">If you have existing virtual machines in VMM that are replicating with Hyper-V Replica, you can onboard them for Azure Site Recovery replication as follows:</span></span>

1. <span data-ttu-id="1eef8-125">Kontrollera att hello Hyper-V-server som är värd hello befintlig virtuell dator finns i hello primära molnet och den hello Hyper-V-server som värd för hello replikerade virtuella datorn finns i hello sekundära moln.</span><span class="sxs-lookup"><span data-stu-id="1eef8-125">Ensure that hello Hyper-V server hosting hello existing VM is located in hello primary cloud, and that hello Hyper-V server hosting hello replica virtual machine is located in hello secondary cloud.</span></span>
2. <span data-ttu-id="1eef8-126">Kontrollera att en replikeringsprincip har konfigurerats för hello primära VMM-moln.</span><span class="sxs-lookup"><span data-stu-id="1eef8-126">Make sure a replication policy is configured for hello primary VMM cloud.</span></span>
3. <span data-ttu-id="1eef8-127">Aktivera replikering för hello primära virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1eef8-127">Enable replication for hello primary virtual machine.</span></span> <span data-ttu-id="1eef8-128">Azure Site Recovery och VMM säkerställa att hello samma replikeringsvärd och den virtuella datorn har identifierats och Azure Site Recovery återanvänder och återupprätta replikering med hello angivna inställningar.</span><span class="sxs-lookup"><span data-stu-id="1eef8-128">Azure Site Recovery and VMM ensure that hello same replica host and virtual machine is detected, and Azure Site Recovery will reuse and reestablish replication using hello specified settings.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1eef8-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1eef8-129">Next steps</span></span>

<span data-ttu-id="1eef8-130">Gå för[steg 10: köra ett redundanstest](vmm-to-vmm-walkthrough-test-failover.md).</span><span class="sxs-lookup"><span data-stu-id="1eef8-130">Go too[Step 10: Run a test failover](vmm-to-vmm-walkthrough-test-failover.md).</span></span>
