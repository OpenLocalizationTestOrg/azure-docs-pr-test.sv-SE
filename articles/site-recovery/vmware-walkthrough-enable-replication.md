---
title: "aaaEnable replikering för virtuella VMware-datorer replikeras tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooenable replikering tooAzure för VMwares virtuella datorer med hjälp av hello Azure Site Recovery-tjänsten"
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 519c5559-7032-4954-b8b5-f24f5242a954
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 490782bbbfa3dd92c626d3985c75d771df53d566
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-11-enable-replication-for-vmware-virtual-machines-tooazure"></a><span data-ttu-id="35452-103">Steg 11: Aktivera replikering för VMware tooAzure för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="35452-103">Step 11: Enable replication for VMware virtual machines tooAzure</span></span>


<span data-ttu-id="35452-104">Den här artikeln beskriver hur tooenable replikering för lokal VMware virtuella datorer tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="35452-104">This article describes how tooenable replication for on-premises VMware virtual machines tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="35452-105">Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="35452-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="before-you-start"></a><span data-ttu-id="35452-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="35452-106">Before you start</span></span>

- <span data-ttu-id="35452-107">Virtuella VMware-datorer måste ha hello [mobilitetstjänsten installeras](vmware-walkthrough-install-mobility.md).</span><span class="sxs-lookup"><span data-stu-id="35452-107">VMware VMs must have hello [Mobility service component installed](vmware-walkthrough-install-mobility.md).</span></span> <span data-ttu-id="35452-108">– Om en virtuell dator är förberedd för push-installation installerar hello mobilitetstjänsten automatiskt hello processervern när du aktiverar replikering.</span><span class="sxs-lookup"><span data-stu-id="35452-108">- If a VM is prepared for push installation, hello process server automatically installs hello Mobility service when you enable replication.</span></span>
- <span data-ttu-id="35452-109">Ditt Azure-konto måste specifika [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikering av en VM-tooAzure</span><span class="sxs-lookup"><span data-stu-id="35452-109">Your Azure user account needs specific [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replication of a VM tooAzure</span></span>
- <span data-ttu-id="35452-110">När du lägger till eller ändra virtuella datorer, kan det ta upp too15 minuter eller längre för ändringar tootake effekt och för dem tooappear hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="35452-110">When you add or modify VMs, it can take up too15 minutes or longer for changes tootake effect, and for them tooappear in hello portal.</span></span>
- <span data-ttu-id="35452-111">Du kan kontrollera hello senast identifierade tid för virtuella datorer i **Konfigurationsservrar** > **senaste kontakt på**.</span><span class="sxs-lookup"><span data-stu-id="35452-111">You can check hello last discovered time for VMs in **Configuration Servers** > **Last Contact At**.</span></span>
- <span data-ttu-id="35452-112">tooadd virtuella datorer utan att vänta på hello schemalagda identifiering, markera hello konfigurationsservern (inte på den), och klicka på **uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="35452-112">tooadd VMs without waiting for hello scheduled discovery, highlight hello configuration server (don’t click it), and click **Refresh**.</span></span>



## <a name="exclude-disks-from-replication"></a><span data-ttu-id="35452-113">Undanta diskar från replikering</span><span class="sxs-lookup"><span data-stu-id="35452-113">Exclude disks from replication</span></span>

<span data-ttu-id="35452-114">Alla diskar på en dator replikeras som standard.</span><span class="sxs-lookup"><span data-stu-id="35452-114">By default all disks on a machine are replicated.</span></span> <span data-ttu-id="35452-115">Du kan exkludera diskar från replikeringen.</span><span class="sxs-lookup"><span data-stu-id="35452-115">You can exclude disks from replication.</span></span> <span data-ttu-id="35452-116">Till exempel kanske du inte vill tooreplicate diskar med tillfälliga data eller data som har uppdateras varje gång en dator eller programmet startas om (till exempel pagefile.sys eller SQL Server tempdb).</span><span class="sxs-lookup"><span data-stu-id="35452-116">For example you might not want tooreplicate disks with temporary data, or data that's refreshed each time a machine or application restarts (for example pagefile.sys or SQL Server tempdb).</span></span> [<span data-ttu-id="35452-117">Läs mer</span><span class="sxs-lookup"><span data-stu-id="35452-117">Learn more</span></span>](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a><span data-ttu-id="35452-118">Replikera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="35452-118">Replicate VMs</span></span>

<span data-ttu-id="35452-119">Innan du börjar titta på en video Snabböversikt</span><span class="sxs-lookup"><span data-stu-id="35452-119">Before you start, watch a quick video overview</span></span>

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. <span data-ttu-id="35452-120">Klicka på **Steg 2: Replikera program** > **Källa**.</span><span class="sxs-lookup"><span data-stu-id="35452-120">Click **Step 2: Replicate application** > **Source**.</span></span>
2. <span data-ttu-id="35452-121">I **källa**väljer hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="35452-121">In **Source**, select hello configuration server.</span></span>
3. <span data-ttu-id="35452-122">I **datorn typen**väljer **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="35452-122">In **Machine type**, select **Virtual Machines**.</span></span>
4. <span data-ttu-id="35452-123">I **vCenter/vSphere-Hypervisor**väljer hello vCenter-servern som hanterar hello vSphere-värd, eller hello värden.</span><span class="sxs-lookup"><span data-stu-id="35452-123">In **vCenter/vSphere Hypervisor**, select hello vCenter server that manages hello vSphere host, or select hello host.</span></span>
5. <span data-ttu-id="35452-124">Välj hello processervern.</span><span class="sxs-lookup"><span data-stu-id="35452-124">Select hello process server.</span></span> <span data-ttu-id="35452-125">Om du inte skapat några ytterligare servrar blir hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="35452-125">If you haven't created any additional process servers this will be hello configuration server.</span></span> <span data-ttu-id="35452-126">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="35452-126">Then click **OK**.</span></span>

    ![Aktivera replikering](./media/vmware-walkthrough-enable-replication/enable-replication2.png)

6. <span data-ttu-id="35452-128">I **mål**hello prenumerationen och välj hello resursgrupp som du vill toocreate hello redundansväxlade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="35452-128">In **Target**, select hello subscription and hello resource group in which you want toocreate hello failed over VMs.</span></span> <span data-ttu-id="35452-129">Välj hello distributionsmodell som du vill använda toouse i Azure (klassiska eller resource management), för hello redundansväxlade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="35452-129">Choose hello deployment model that you want toouse in Azure (classic or resource management), for hello failed over VMs.</span></span>


7. <span data-ttu-id="35452-130">Välj hello Azure storage-konto som du vill använda toouse för replikering av data.</span><span class="sxs-lookup"><span data-stu-id="35452-130">Select hello Azure storage account you want toouse for replicating data.</span></span> <span data-ttu-id="35452-131">Om du inte vill toouse ett konto som du redan har konfigurerat, kan du skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="35452-131">If you don't want toouse an account you've already set up, you can create a new one.</span></span>

8. <span data-ttu-id="35452-132">Välj hello Azure nätverk och undernät toowhich virtuella Azure-datorer ansluter när de skapas efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="35452-132">Select hello Azure network and subnet toowhich Azure VMs will connect, when they're created after failover.</span></span> <span data-ttu-id="35452-133">Välj **konfigurera nu för valda datorer**, tooapply hello nätverket inställningen tooall datorer som du väljer för skydd.</span><span class="sxs-lookup"><span data-stu-id="35452-133">Select **Configure now for selected machines**, tooapply hello network setting tooall machines you select for protection.</span></span> <span data-ttu-id="35452-134">Välj **konfigurera senare** tooselect hello Azure-nätverk per dator.</span><span class="sxs-lookup"><span data-stu-id="35452-134">Select **Configure later** tooselect hello Azure network per machine.</span></span> <span data-ttu-id="35452-135">Om du inte vill toouse ett befintligt nätverk kan skapa du en.</span><span class="sxs-lookup"><span data-stu-id="35452-135">If you don't want toouse an existing network, you can create one.</span></span>

    ![Aktivera replikering](./media/vmware-walkthrough-enable-replication/enable-rep3.png)
9. <span data-ttu-id="35452-137">I **virtuella datorer** > **Välj virtuella datorer**och på varje dator som du vill tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="35452-137">In **Virtual Machines** > **Select virtual machines**, click and select each machine you want tooreplicate.</span></span> <span data-ttu-id="35452-138">Du kan bara välja datorer som stöder replikering.</span><span class="sxs-lookup"><span data-stu-id="35452-138">You can only select machines for which replication can be enabled.</span></span> <span data-ttu-id="35452-139">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="35452-139">Then click **OK**.</span></span>

    ![Aktivera replikering](./media/vmware-walkthrough-enable-replication/enable-replication5.png)
10. <span data-ttu-id="35452-141">I **egenskaper** > **konfigurera egenskaper**väljer hello-konto som ska användas av hello processen server tooautomatically installera hello mobilitetstjänsten på hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="35452-141">In **Properties** > **Configure properties**, select hello account that will be used by hello process server tooautomatically install hello Mobility service on hello machine.</span></span>
11. <span data-ttu-id="35452-142">Alla diskar replikeras som standard.</span><span class="sxs-lookup"><span data-stu-id="35452-142">By default all disks are replicated.</span></span> <span data-ttu-id="35452-143">Klicka på **alla diskar** och avmarkera alla diskar som du inte vill tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="35452-143">Click **All Disks** and clear any disks you don't want tooreplicate.</span></span> <span data-ttu-id="35452-144">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="35452-144">Then click **OK**.</span></span> <span data-ttu-id="35452-145">Du kan ange ytterligare egenskaper för Virtuella datorer senare.</span><span class="sxs-lookup"><span data-stu-id="35452-145">You can set additional VM properties later.</span></span>

    ![Aktivera replikering](./media/vmware-walkthrough-enable-replication/enable-replication6.png)
11. <span data-ttu-id="35452-147">I **replikeringsinställningarna** > **konfigurerar replikeringsinställningar**, kontrollera att rätt replikeringsprincip väljs hello.</span><span class="sxs-lookup"><span data-stu-id="35452-147">In **Replication settings** > **Configure replication settings**, verify that hello correct replication policy is selected.</span></span> <span data-ttu-id="35452-148">Om du ändrar en princip kommer ändringarna att tillämpade tooreplicating datorn och toonew datorer.</span><span class="sxs-lookup"><span data-stu-id="35452-148">If you modify a policy, changes will be applied tooreplicating machine, and toonew machines.</span></span>
12. <span data-ttu-id="35452-149">Aktivera **konsekvens för flera** om du vill toogather datorer i en replikeringsgrupp och ange ett namn för hello-gruppen.</span><span class="sxs-lookup"><span data-stu-id="35452-149">Enable **Multi-VM consistency** if you want toogather machines into a replication group, and specify a name for hello group.</span></span> <span data-ttu-id="35452-150">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="35452-150">Then click **OK**.</span></span> <span data-ttu-id="35452-151">Tänk på följande:</span><span class="sxs-lookup"><span data-stu-id="35452-151">Note that:</span></span>

    * <span data-ttu-id="35452-152">Datorer i replikeringsgrupper replikeras tillsammans, och har delat kraschkonsekvent och programkonsekventa återställningspunkter när de växlar över.</span><span class="sxs-lookup"><span data-stu-id="35452-152">Machines in replication groups replicate together, and have shared crash-consistent and app-consistent recovery points when they fail over.</span></span>
    * <span data-ttu-id="35452-153">Vi rekommenderar att du samla in virtuella datorer och fysiska servrar så att de speglar dina arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="35452-153">We recommend that you gather VMs and physical servers together so that they mirror your workloads.</span></span> <span data-ttu-id="35452-154">Aktivera konsekvens för flera kan påverka arbetsbelastningens prestanda och bör endast användas om datorer kör hello samma arbetsbelastning och du behöver enhetlighet.</span><span class="sxs-lookup"><span data-stu-id="35452-154">Enabling multi-VM consistency can impact workload performance, and should only be used if machines are running hello same workload and you need consistency.</span></span>

    ![Aktivera replikering](./media/vmware-walkthrough-enable-replication/enable-replication7.png)
13. <span data-ttu-id="35452-156">Klicka på **Aktivera replikering**.</span><span class="sxs-lookup"><span data-stu-id="35452-156">Click **Enable Replication**.</span></span> <span data-ttu-id="35452-157">Du kan följa förloppet för hello **Aktivera skydd** jobb **inställningar** > **jobb** > **Site Recovery-jobb**.</span><span class="sxs-lookup"><span data-stu-id="35452-157">You can track progress of hello **Enable Protection** job in **Settings** > **Jobs** > **Site Recovery Jobs**.</span></span> <span data-ttu-id="35452-158">Efter hello **Slutför skydd** jobbet körs hello datorn är redo för redundans.</span><span class="sxs-lookup"><span data-stu-id="35452-158">After hello **Finalize Protection** job runs hello machine is ready for failover.</span></span>

## <a name="next-steps"></a><span data-ttu-id="35452-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="35452-159">Next steps</span></span>

<span data-ttu-id="35452-160">Gå för[steg 12: kör ett redundanstest](vmware-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="35452-160">Go too[Step 12: Run a test failover](vmware-walkthrough-test-failover.md)</span></span>
