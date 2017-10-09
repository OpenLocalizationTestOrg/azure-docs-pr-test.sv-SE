---
title: "aaaRun ett redundanstest för Hyper-V VM replikering tooa sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur toorun ett redundanstest för Hyper-V VM replikering tooa sekundär System Center VMM webbplats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a3fd11ce-65a1-4308-8435-45cf954353ef
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: a60411541c2db001c573735bcb0d651f6618f295
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-run-a-test-failover-for-hyper-v-replication-tooa-secondary-site"></a><span data-ttu-id="55557-103">Steg 10: Köra ett redundanstest för Hyper-V-replikering tooa sekundär plats</span><span class="sxs-lookup"><span data-stu-id="55557-103">Step 10: Run a test failover for Hyper-V replication tooa secondary site</span></span>


<span data-ttu-id="55557-104">När du har aktiverat replikering för Hyper-V virtuella datorer (VM) med [Azure Site Recovery](site-recovery-overview.md), Använd den här artikeln toorun testa redundans.</span><span class="sxs-lookup"><span data-stu-id="55557-104">After you enable replication for Hyper-V virtual machines (VMs) with [Azure Site Recovery](site-recovery-overview.md), use this article toorun a test failover.</span></span> <span data-ttu-id="55557-105">Ett redundanstest kontrollerar att replikering fungerar utan att påverka produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="55557-105">A test failover verifies that replication is working, without impacting your production environment.</span></span> 


<span data-ttu-id="55557-106">När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="55557-106">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="before-you-start"></a><span data-ttu-id="55557-107">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="55557-107">Before you start</span></span>

- <span data-ttu-id="55557-108">När du utlöser ett redundanstest kan ange du hello nätverket toowhich hello test replik virtuella datorer ska ansluta.</span><span class="sxs-lookup"><span data-stu-id="55557-108">When you're triggering a test failover, you can specify hello network toowhich hello test replica VMs will connect.</span></span> <span data-ttu-id="55557-109">[Lär dig mer](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery) om hello Nätverksalternativ.</span><span class="sxs-lookup"><span data-stu-id="55557-109">[Learn more](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery) about hello network options.</span></span>
- <span data-ttu-id="55557-110">Vi rekommenderar att du inte väljer ett nätverk som du valde under nätverksmappning.</span><span class="sxs-lookup"><span data-stu-id="55557-110">We recommend that you don't choose a network that you selected during network mapping.</span></span>
- <span data-ttu-id="55557-111">hello anvisningar i den här artikeln beskrivs hur toofail via en enda virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="55557-111">hello instructions in this article describe how toofail over a single VM.</span></span> <span data-ttu-id="55557-112">Läs mer om [skapa återställningsplaner](site-recovery-create-recovery-plans.md) om du vill ha toofail över flera virtuella datorer tillsammans.</span><span class="sxs-lookup"><span data-stu-id="55557-112">Read about [creating recovery plans](site-recovery-create-recovery-plans.md) if you want toofail over multiple VMs together.</span></span>
- <span data-ttu-id="55557-113">Läs mer om [begränsningar för redundanstestning](site-recovery-test-failover-vmm-to-vmm.md#things-to-note).</span><span class="sxs-lookup"><span data-stu-id="55557-113">Read about [limitations for test failovers](site-recovery-test-failover-vmm-to-vmm.md#things-to-note).</span></span>
- <span data-ttu-id="55557-114">hello IP-adress angavs tooa VM under testning av redundans är hello samma IP-adress som hello VM skulle få en planerad eller oplanerad växling vid fel (förutsatt att hello IP-adress är tillgänglig i nätverket hello).</span><span class="sxs-lookup"><span data-stu-id="55557-114">hello IP address given tooa VM during test failover is hello same IP address that hello VM would receive for a planned or unplanned failover (presuming that hello IP address is available in hello test failover network).</span></span> <span data-ttu-id="55557-115">Om hello IP-adressen är inte tillgänglig i nätverket hello får hello VM en annan IP-adress som är tillgänglig i nätverket hello.</span><span class="sxs-lookup"><span data-stu-id="55557-115">If hello IP address isn't available in hello test failover network, hello VM receives another IP address that is available in hello test failover network.</span></span>
- <span data-ttu-id="55557-116">Om du vill toodo ett redundanstest i produktionsnätverket för fullständig validatation för slutpunkt till slutpunkt anslutningen datorn, Lägg märke till att:</span><span class="sxs-lookup"><span data-stu-id="55557-116">If you do want toodo a test failover in your production network, for full validatation of end-to-end network connectivity machine, note that:</span></span>
    - <span data-ttu-id="55557-117">hello primära virtuella datorn måste stängas av när du gör hello testa redundans.</span><span class="sxs-lookup"><span data-stu-id="55557-117">hello primary VM should be shut down when you're doing hello test failover.</span></span> <span data-ttu-id="55557-118">Annars två virtuella datorer med samma identitet som ska köras i hello hello samma nätverk på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="55557-118">Otherwise, two VMs with hello same identity will be running in hello same network at hello same time.</span></span> 
    - <span data-ttu-id="55557-119">Om du ändrar tootest VMs, försvinner dessa ändringar när du rensar hello testa redundans.</span><span class="sxs-lookup"><span data-stu-id="55557-119">If you make changes tootest VMs, those changes are lost when you clean up hello test failover.</span></span> <span data-ttu-id="55557-120">De här ändringarna inte replikeras tillbaka toohello primära virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="55557-120">These changes aren't replicated back toohello primary VM.</span></span>
    - <span data-ttu-id="55557-121">Testa en leder toodowntime för din produktionsarbetsbelastningar för ett produktionsnätverk.</span><span class="sxs-lookup"><span data-stu-id="55557-121">Testing a a production network leads toodowntime for your production workloads.</span></span> <span data-ttu-id="55557-122">Be användare inte toouse hello app när hello disaster recovery ökad pågår.</span><span class="sxs-lookup"><span data-stu-id="55557-122">Ask users not toouse hello app when hello disaster recovery drill is in progress.</span></span>  


## <a name="run-a-test-failover-for-a-vm"></a><span data-ttu-id="55557-123">Köra ett redundanstest för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="55557-123">Run a test failover for a VM</span></span>

1. <span data-ttu-id="55557-124">toofail via en enda virtuell dator i **replikerade objekt**, klicka på hello VM > **Redundanstest**.</span><span class="sxs-lookup"><span data-stu-id="55557-124">toofail over a single VM, in **Replicated Items**, click hello VM > **Test Failover**.</span></span>
2. <span data-ttu-id="55557-125">I **Redundanstestning**, ange hur test virtuella datorer kommer att vara anslutna toonetworks efter hello testa redundans.</span><span class="sxs-lookup"><span data-stu-id="55557-125">In **Test Failover**, specify how test VMs will be connected toonetworks after hello test failover.</span></span> 
3. <span data-ttu-id="55557-126">Klicka på **OK** toobegin hello redundans.</span><span class="sxs-lookup"><span data-stu-id="55557-126">Click **OK** toobegin hello failover.</span></span> <span data-ttu-id="55557-127">Följa förloppet på hello **jobb** fliken.</span><span class="sxs-lookup"><span data-stu-id="55557-127">Track progress on hello **Jobs** tab.</span></span>
5. <span data-ttu-id="55557-128">När redundansväxlingen är klar kontrollerar du att hello test start för virtuella datorer har.</span><span class="sxs-lookup"><span data-stu-id="55557-128">After failover is complete, verify that hello test VMs start successfully.</span></span>
6. <span data-ttu-id="55557-129">När du är klar klickar du på **Rensa redundanstestet** på hello återställningsplan.</span><span class="sxs-lookup"><span data-stu-id="55557-129">When you're done, click **Cleanup test failover** on hello recovery plan.</span></span>
7. <span data-ttu-id="55557-130">I **anteckningar**, registrera och spara observationer från redundanstestningen hello.</span><span class="sxs-lookup"><span data-stu-id="55557-130">In **Notes**, record and save any observations associated with hello test failover.</span></span> <span data-ttu-id="55557-131">Det här steget tar bort hello virtuella datorer och nätverk som har skapats under redundanstestningen.</span><span class="sxs-lookup"><span data-stu-id="55557-131">This step deletes hello virtual machines and networks that were created during test failover.</span></span>


## <a name="next-steps"></a><span data-ttu-id="55557-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="55557-132">Next steps</span></span>

<span data-ttu-id="55557-133">När du har testat hello distribution, mer information om andra typer av [redundans](site-recovery-failover.md).</span><span class="sxs-lookup"><span data-stu-id="55557-133">After you've tested hello deployment, learn more about other types of [failover](site-recovery-failover.md).</span></span>
