---
title: "aaaEnable replikering för Hyper-V virtuella datorer replikeras tooAzure (utan System Center VMM) med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooenable replikering tooAzure för Hyper-V virtuella datorer med hjälp av hello Azure Site Recovery-tjänsten"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: bd31ef01-69f1-4591-a519-e42510a6e2f4
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 1589cb7aa1fe954e075cb7bf1f4a4ec199ed3ec7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-enable-replication-for-hyper-v-vms-replicating-tooazure"></a><span data-ttu-id="b474f-103">Steg 10: Aktivera replikering för Hyper-V virtuella datorer replikeras tooAzure</span><span class="sxs-lookup"><span data-stu-id="b474f-103">Step 10: Enable replication for Hyper-V VMs replicating tooAzure</span></span>


<span data-ttu-id="b474f-104">Den här artikeln beskriver hur tooenable replikering för den lokala Hyper-V virtuella datorer (som inte hanteras av System Center VMM) tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b474f-104">This article describes how tooenable replication for on-premises Hyper-V virtual machines (not managed by System Center VMM) tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="b474f-105">Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="b474f-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="before-you-start"></a><span data-ttu-id="b474f-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="b474f-106">Before you start</span></span>

<span data-ttu-id="b474f-107">Innan du börjar bör du kontrollera att ditt Azure-konto har hello krävs [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikering av en ny virtuell dator tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b474f-107">Before you start, ensure that your Azure user account has hello required [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replication of a new virtual machine tooAzure.</span></span>

## <a name="exclude-disks-from-replication"></a><span data-ttu-id="b474f-108">Undanta diskar från replikering</span><span class="sxs-lookup"><span data-stu-id="b474f-108">Exclude disks from replication</span></span>

<span data-ttu-id="b474f-109">Alla diskar på en dator replikeras som standard.</span><span class="sxs-lookup"><span data-stu-id="b474f-109">By default all disks on a machine are replicated.</span></span> <span data-ttu-id="b474f-110">Du kan exkludera diskar från replikeringen.</span><span class="sxs-lookup"><span data-stu-id="b474f-110">You can exclude disks from replication.</span></span> <span data-ttu-id="b474f-111">Till exempel kanske du inte vill tooreplicate diskar med tillfälliga data eller data som har uppdateras varje gång en dator eller programmet startas om (till exempel pagefile.sys eller SQL Server tempdb).</span><span class="sxs-lookup"><span data-stu-id="b474f-111">For example you might not want tooreplicate disks with temporary data, or data that's refreshed each time a machine or application restarts (for example pagefile.sys or SQL Server tempdb).</span></span> [<span data-ttu-id="b474f-112">Läs mer</span><span class="sxs-lookup"><span data-stu-id="b474f-112">Learn more</span></span>](site-recovery-exclude-disk.md)


## <a name="replicate-vms"></a><span data-ttu-id="b474f-113">Replikera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="b474f-113">Replicate VMs</span></span>

<span data-ttu-id="b474f-114">Aktivera replikering för virtuella datorer på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b474f-114">Enable replication for VMs as follows:</span></span>          

1. <span data-ttu-id="b474f-115">Klicka på **replikera program** > **källa**.</span><span class="sxs-lookup"><span data-stu-id="b474f-115">Click **Replicate application** > **Source**.</span></span> <span data-ttu-id="b474f-116">När du har konfigurerat replikering för hello första gången du klickar på **+ replikera** tooenable replikering för ytterligare datorer.</span><span class="sxs-lookup"><span data-stu-id="b474f-116">After you've set up replication for hello first time, you can click **+Replicate** tooenable replication for additional machines.</span></span>

    ![Aktivera replikering](./media/hyper-v-site-walkthrough-enable-replication/enable-replication.png)
2. <span data-ttu-id="b474f-118">I **källa**väljer hello Hyper-V-platsen.</span><span class="sxs-lookup"><span data-stu-id="b474f-118">In **Source**, select hello Hyper-V site.</span></span> <span data-ttu-id="b474f-119">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b474f-119">Then click **OK**.</span></span>
3. <span data-ttu-id="b474f-120">I **mål**hello valvet prenumerationen och välj hello failover-modell som du vill ha toouse i Azure (klassiska eller resource management) efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="b474f-120">In **Target**, select hello vault subscription, and hello failover model you want toouse in Azure (classic or resource management) after failover.</span></span>
4. <span data-ttu-id="b474f-121">Välj hello storage-konto som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="b474f-121">Select hello storage account you want toouse.</span></span> <span data-ttu-id="b474f-122">Om du inte har ett konto som du vill toouse, kan du [skapar du en](#set-up-an-azure-storage-account).</span><span class="sxs-lookup"><span data-stu-id="b474f-122">If you don't have an account you want toouse, you can [create one](#set-up-an-azure-storage-account).</span></span> <span data-ttu-id="b474f-123">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b474f-123">Then click **OK**.</span></span>
5. <span data-ttu-id="b474f-124">Välj hello Azure nätverk och undernät toowhich virtuella Azure-datorer ansluter när de skapas växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="b474f-124">Select hello Azure network and subnet toowhich Azure VMs will connect when they're created failover.</span></span>

    - <span data-ttu-id="b474f-125">tooapply hello inställningar tooall datorer aktiveras för replikering, Välj **konfigurera nu för valda datorer**.</span><span class="sxs-lookup"><span data-stu-id="b474f-125">tooapply hello network settings tooall machines you enable for replication, select **Configure now for selected machines**.</span></span>
    - <span data-ttu-id="b474f-126">Välj **konfigurera senare** tooselect hello Azure-nätverk per dator.</span><span class="sxs-lookup"><span data-stu-id="b474f-126">Select **Configure later** tooselect hello Azure network per machine.</span></span>
    - <span data-ttu-id="b474f-127">Om du inte har ett nätverk som du vill toouse, kan du [skapar du en](#set-up-an-azure-network).</span><span class="sxs-lookup"><span data-stu-id="b474f-127">If you don't have a network you want toouse, you can [create one](#set-up-an-azure-network).</span></span> <span data-ttu-id="b474f-128">Välj ett undernät om det behövs.</span><span class="sxs-lookup"><span data-stu-id="b474f-128">Select a subnet if applicable.</span></span> <span data-ttu-id="b474f-129">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b474f-129">Then click **OK**.</span></span>

   ![Aktivera replikering](./media/hyper-v-site-walkthrough-enable-replication/enable-replication11.png)

6. <span data-ttu-id="b474f-131">I **virtuella datorer** > **Välj virtuella datorer**och på varje dator som du vill tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="b474f-131">In **Virtual Machines** > **Select virtual machines**, click and select each machine you want tooreplicate.</span></span> <span data-ttu-id="b474f-132">Du kan bara välja datorer som stöder replikering.</span><span class="sxs-lookup"><span data-stu-id="b474f-132">You can only select machines for which replication can be enabled.</span></span> <span data-ttu-id="b474f-133">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b474f-133">Then click **OK**.</span></span>

    ![Aktivera replikering](./media/hyper-v-site-walkthrough-enable-replication/enable-replication5-for-exclude-disk.png)

7. <span data-ttu-id="b474f-135">I **egenskaper** > **konfigurera egenskaper**hello operativsystemet för hello valda virtuella datorer och välj hello OS-disken.</span><span class="sxs-lookup"><span data-stu-id="b474f-135">In **Properties** > **Configure properties**, select hello operating system for hello selected VMs, and hello OS disk.</span></span>
8. <span data-ttu-id="b474f-136">Kontrollera att hello Azure VM namn (mål) uppfyller [Azure virtuella datorns krav](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="b474f-136">Verify that hello Azure VM name (target name) complies with [Azure virtual machine requirements](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>
9. <span data-ttu-id="b474f-137">Som standard markeras alla hello diskar på hello VM för replikering.</span><span class="sxs-lookup"><span data-stu-id="b474f-137">By default all hello disks of hello VM are selected for replication.</span></span> <span data-ttu-id="b474f-138">Rensa diskar tooexclude dem.</span><span class="sxs-lookup"><span data-stu-id="b474f-138">Clear disks tooexclude them.</span></span>
10. <span data-ttu-id="b474f-139">Klicka på **OK** toosave ändringar.</span><span class="sxs-lookup"><span data-stu-id="b474f-139">Click **OK** toosave changes.</span></span> <span data-ttu-id="b474f-140">Du kan ange ytterligare egenskaper senare.</span><span class="sxs-lookup"><span data-stu-id="b474f-140">You can set additional properties later.</span></span>

    ![Aktivera replikering](./media/hyper-v-site-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

11. <span data-ttu-id="b474f-142">I **replikeringsinställningarna** > **konfigurerar replikeringsinställningar**, Välj hello replikeringsprincip som du vill tooapply för hello skyddade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b474f-142">In **Replication settings** > **Configure replication settings**, select hello replication policy you want tooapply for hello protected VMs.</span></span> <span data-ttu-id="b474f-143">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b474f-143">Then click **OK**.</span></span> <span data-ttu-id="b474f-144">Du kan ändra hello replikeringsprincip i **replikeringsprinciper** > principnamn > **redigera inställningar för**.</span><span class="sxs-lookup"><span data-stu-id="b474f-144">You can modify hello replication policy in **Replication policies** > policy-name > **Edit Settings**.</span></span> <span data-ttu-id="b474f-145">De ändringar du gör används både för datorer som redan replikeras och för nya datorer.</span><span class="sxs-lookup"><span data-stu-id="b474f-145">Changes you apply will be used for machines that are already replicating, and new machines.</span></span>


   ![Aktivera replikering](./media/hyper-v-site-walkthrough-enable-replication/enable-replication7.png)

<span data-ttu-id="b474f-147">Du kan följa förloppet för hello **Aktivera skydd** jobb **jobb** > **Site Recovery-jobb**.</span><span class="sxs-lookup"><span data-stu-id="b474f-147">You can track progress of hello **Enable Protection** job in **Jobs** > **Site Recovery jobs**.</span></span> <span data-ttu-id="b474f-148">Efter hello **Slutför skydd** jobbet körs hello datorn är redo för redundans.</span><span class="sxs-lookup"><span data-stu-id="b474f-148">After hello **Finalize Protection** job runs hello machine is ready for failover.</span></span>


## <a name="next-steps"></a><span data-ttu-id="b474f-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b474f-149">Next steps</span></span>


<span data-ttu-id="b474f-150">Gå för[steg 11: kör ett redundanstest](hyper-v-site-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="b474f-150">Go too[Step 11: Run a test failover](hyper-v-site-walkthrough-test-failover.md)</span></span>
