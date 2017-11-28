---
title: "aaaRun ett redundanstest för replikering av virtuella Azure-datorn med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello steg som du behöver för att köra ett redundanstest för virtuella datorer i Azure replikering tooanother Azure-region med hello Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: e15c1b0c-5d75-4fdf-acb0-e61def9e9339
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: c1f765aa94c59dd70b33317ebbcd04beb7977969
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-run-a-test-failover-for-azure-vm-replication"></a><span data-ttu-id="fdc9c-103">Steg 6: Köra ett redundanstest för Azure VM-replikering</span><span class="sxs-lookup"><span data-stu-id="fdc9c-103">Step 6: Run a test failover for Azure VM replication</span></span>

<span data-ttu-id="fdc9c-104">När du har aktiverat replikering för virtuell Azure-dator (VM), så hello i den här artikeln toorun testa redundans från en Azure-region tooanother, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-104">After you've enabled replication for Azure virtual machine (VMs), follow hello steps in this article, toorun test failover from one Azure region tooanother, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

- <span data-ttu-id="fdc9c-105">När du är klar hello artikel bör du har verifierat med ett redundanstest att minst en virtuell Azure-dator kan växla över tooyour sekundära Azure-region.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-105">When you finish hello article, you should have verified with a test failover, that at least one Azure VM can fail over tooyour secondary Azure region.</span></span> 
- <span data-ttu-id="fdc9c-106">Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span><span class="sxs-lookup"><span data-stu-id="fdc9c-106">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span></span>

>[!NOTE]
>
> <span data-ttu-id="fdc9c-107">Azure VM-replikering är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-107">Azure VM replication is currently in preview.</span></span>


## <a name="before-you-start"></a><span data-ttu-id="fdc9c-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="fdc9c-108">Before you start</span></span>

- <span data-ttu-id="fdc9c-109">Innan du kör ett redundanstest rekommenderar vi att du kontrollera hello VM egenskaper och gör eventuella ändringar som du behöver.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-109">Before you run a test failover, we recommend that you verify hello VM properties, and make any changes you need to.</span></span> <span data-ttu-id="fdc9c-110">Du kan komma åt hello VM egenskaper i **replikerade objekt**.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-110">You can access hello VM properties in **Replicated items**.</span></span> <span data-ttu-id="fdc9c-111">Hej **Essentials** bladet visar information om inställningar för datorer och status.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-111">hello **Essentials** blade shows information about machines settings and status.</span></span>
- <span data-ttu-id="fdc9c-112">Vi rekommenderar att du använder ett separat nätverk för virtuell dator i Azure för hello testa redundans och inte hello nätverk (standard eller anpassade) som har ställts in för redundans för produktion.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-112">We recommend you use a separate Azure VM network for hello test failover, and not hello network (default or customized) that was set up for production failover.</span></span>
- <span data-ttu-id="fdc9c-113">hello testa redundans växlar över virtuella Azure-datorer (och deras lagringsutrymmen) toohello sekundära Azure-region.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-113">hello test failover fails over Azure VMs (and their storage) toohello secondary Azure region.</span></span> <span data-ttu-id="fdc9c-114">Den replikeras inte beroende appar eller resurser.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-114">It doesn't replicate any dependent apps or resources.</span></span> <span data-ttu-id="fdc9c-115">Om appar som körs på misslyckade över virtuella datorer är beroende av andra resurser, till exempel Active Directory eller DNS, måste tooreplicate dessa också, om de inte redan finns i din sekundär region.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-115">If apps running on failed over VMs are dependent on other resources, such as Active Directory or DNS, you need tooreplicate these too, if they're not already available in your secondary region.</span></span> [<span data-ttu-id="fdc9c-116">Läs mer</span><span class="sxs-lookup"><span data-stu-id="fdc9c-116">Learn more</span></span>](site-recovery-test-failover-to-azure.md#prepare-active-directory-and-dns)
- <span data-ttu-id="fdc9c-117">Om du vill tooaccess replikerade virtuella datorer efter redundans från en lokal plats, du behöver för[förbereda tooconnect](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) toothese virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-117">If you want tooaccess replicated VMs after failover from an on-premises site, you need too[prepare tooconnect](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) toothese VMs.</span></span>

## <a name="run-a-test-failover"></a><span data-ttu-id="fdc9c-118">Köra ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="fdc9c-118">Run a test failover</span></span>

1. <span data-ttu-id="fdc9c-119">I **inställningar** > **replikerade objekt**, klicka på hello VM **+ testa redundans** ikon.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-119">In **Settings** > **Replicated Items**, click hello VM **+Test Failover** icon.</span></span> 

2. <span data-ttu-id="fdc9c-120">I **Redundanstest**, Välj en återställningspunkt punkt toouse hello växling vid fel:</span><span class="sxs-lookup"><span data-stu-id="fdc9c-120">In **Test Failover**, Select a recovery point toouse for hello failover:</span></span>

    - <span data-ttu-id="fdc9c-121">**Senaste bearbetas**: misslyckas hello VM över toohello senaste återställningspunkten som bearbetades av hello Site Recovery-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-121">**Latest processed**: Fails hello VM over toohello latest recovery point that was processed by hello Site Recovery service.</span></span> <span data-ttu-id="fdc9c-122">Hej, tidsstämpel visas.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-122">hello time stamp is shown.</span></span> <span data-ttu-id="fdc9c-123">Med det här alternativet läggs ingen tid bearbetning av data, så att den ger en låga RTO (mål).</span><span class="sxs-lookup"><span data-stu-id="fdc9c-123">With this option, no time is spent processing data, so it provides a low RTO (Recovery Time Objective).</span></span>
    - <span data-ttu-id="fdc9c-124">**Senaste programkonsekventa**: det här alternativet flyttas över alla virtuella datorer toohello senaste programkonsekventa återställningspunkten.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-124">**Latest app-consistent**: This option fails over all VMs toohello latest app-consistent recovery point.</span></span> <span data-ttu-id="fdc9c-125">Hej, tidsstämpel visas.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-125">hello time stamp is shown.</span></span> 
    - <span data-ttu-id="fdc9c-126">**Anpassad**: Välj någon annan återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-126">**Custom**: Select any recovery point.</span></span>
 
3. <span data-ttu-id="fdc9c-127">Välj hello mål virtuella Azure-nätverket toowhich Azure virtuella datorer i hello sekundär region ansluts när hello växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-127">Select hello target Azure virtual network toowhich Azure VMs in hello secondary region will be connected, after hello failover occurs.</span></span>
4. <span data-ttu-id="fdc9c-128">toostart Hej växling vid fel, klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-128">toostart hello failover, click **OK**.</span></span> <span data-ttu-id="fdc9c-129">tootrack utvecklas, klicka på hello VM tooopen dess egenskaper.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-129">tootrack progress, click hello VM tooopen its properties.</span></span> <span data-ttu-id="fdc9c-130">Du kan också klicka på hello **Redundanstest** jobb i hello valvnamnet > **inställningar** > **jobb** > **Site Recovery-jobb**.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-130">Or, you can click hello **Test Failover** job in hello vault name > **Settings** > **Jobs** > **Site Recovery jobs**.</span></span>
5. <span data-ttu-id="fdc9c-131">Efter hello växling vid fel är klar hello replik virtuella Azure-datorn visas i hello Azure-portalen > **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-131">After hello failover finishes, hello replica Azure VM appears in hello Azure portal > **Virtual Machines**.</span></span> <span data-ttu-id="fdc9c-132">Kontrollera att hello VM hello rätt storlek, att den är ansluten toohello lämpligt nätverk och att den körs.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-132">Make sure that hello VM is hello appropriate size, that it's connected toohello appropriate network, and that it's running.</span></span>
6. <span data-ttu-id="fdc9c-133">toodelete hello virtuella datorer som har skapats under hello redundanstestningen klickar du på **Rensa redundanstestet** på hello replikerade objekt eller hello återställningsplan.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-133">toodelete hello VMs that were created during hello test failover, click **Cleanup test failover** on hello replicated item or hello recovery plan.</span></span> <span data-ttu-id="fdc9c-134">I **anteckningar**, registrera och spara observationer från redundanstestningen hello.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-134">In **Notes**, record and save any observations associated with hello test failover.</span></span> 

<span data-ttu-id="fdc9c-135">[Lär dig mer](site-recovery-test-failover-to-azure.md) om redundanstestning.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-135">[Learn more](site-recovery-test-failover-to-azure.md) about test failovers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fdc9c-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fdc9c-136">Next steps</span></span>

<span data-ttu-id="fdc9c-137">Den här genomgången är klar när du har testat växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-137">After you've tested failover, this walkthrough is complete.</span></span> <span data-ttu-id="fdc9c-138">Nu, lär du dig om att köra redundansväxlingar i produktion:</span><span class="sxs-lookup"><span data-stu-id="fdc9c-138">Now, learn about running failovers in production:</span></span>

- <span data-ttu-id="fdc9c-139">[Lär dig mer](site-recovery-failover.md) om olika typer av växling vid fel, och hur toorun dem.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-139">[Learn more](site-recovery-failover.md) about different types of failovers, and how toorun them.</span></span>
- <span data-ttu-id="fdc9c-140">Mer information om misslyckas över flera virtuella datorer [med hjälp av en återställningsplan](site-recovery-create-recovery-plans.md).</span><span class="sxs-lookup"><span data-stu-id="fdc9c-140">Learn more about failing over multiple VMs [using a recovery plan](site-recovery-create-recovery-plans.md).</span></span>
- <span data-ttu-id="fdc9c-141">Lär dig mer om [med återställningsplaner](site-recovery-create-recovery-plans.md).</span><span class="sxs-lookup"><span data-stu-id="fdc9c-141">Learn more about [using recovery plans](site-recovery-create-recovery-plans.md).</span></span>
- <span data-ttu-id="fdc9c-142">Lär dig mer om [skydda virtuella datorer i Azure](site-recovery-how-to-reprotect.md) efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="fdc9c-142">Learn more about [reprotecting Azure  VMs](site-recovery-how-to-reprotect.md) after failover.</span></span>

