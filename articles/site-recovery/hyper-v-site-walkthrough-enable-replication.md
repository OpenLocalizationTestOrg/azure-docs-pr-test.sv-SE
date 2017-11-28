---
title: "Aktivera replikering för virtuella Hyper-V-datorer som replikerar till Azure (utan System Center VMM) med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattas de steg som du måste aktivera replikering till Azure för Hyper-V virtuella datorer med hjälp av Azure Site Recovery-tjänsten"
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
ms.openlocfilehash: aabe99dbd375b80e4a87ca7a067927008672b4ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="step-10-enable-replication-for-hyper-v-vms-replicating-to-azure"></a><span data-ttu-id="7c2d5-103">Steg 10: Aktivera replikering för virtuella Hyper-V-datorer som replikerar till Azure</span><span class="sxs-lookup"><span data-stu-id="7c2d5-103">Step 10: Enable replication for Hyper-V VMs replicating to Azure</span></span>


<span data-ttu-id="7c2d5-104">Den här artikeln beskriver hur du aktiverar replikering för lokala Hyper-V virtuella datorer (som inte hanteras av System Center VMM) till Azure med hjälp av den [Azure Site Recovery](site-recovery-overview.md) tjänsten i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-104">This article describes how to enable replication for on-premises Hyper-V virtual machines (not managed by System Center VMM) to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="7c2d5-105">Publicera kommentarer och frågor längst ned i den här artikeln eller i den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="7c2d5-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="before-you-start"></a><span data-ttu-id="7c2d5-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="7c2d5-106">Before you start</span></span>

<span data-ttu-id="7c2d5-107">Innan du börjar bör du kontrollera att ditt Azure-konto har de nödvändiga [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) att aktivera replikering för en ny virtuell dator till Azure.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-107">Before you start, ensure that your Azure user account has the required [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) to enable replication of a new virtual machine to Azure.</span></span>

## <a name="exclude-disks-from-replication"></a><span data-ttu-id="7c2d5-108">Undanta diskar från replikering</span><span class="sxs-lookup"><span data-stu-id="7c2d5-108">Exclude disks from replication</span></span>

<span data-ttu-id="7c2d5-109">Alla diskar på en dator replikeras som standard.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-109">By default all disks on a machine are replicated.</span></span> <span data-ttu-id="7c2d5-110">Du kan exkludera diskar från replikeringen.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-110">You can exclude disks from replication.</span></span> <span data-ttu-id="7c2d5-111">Till exempel kanske du inte vill replikera diskar med tillfälliga data eller data som har uppdateras varje gång en dator eller programmet startas om (till exempel pagefile.sys eller SQL Server tempdb).</span><span class="sxs-lookup"><span data-stu-id="7c2d5-111">For example you might not want to replicate disks with temporary data, or data that's refreshed each time a machine or application restarts (for example pagefile.sys or SQL Server tempdb).</span></span> [<span data-ttu-id="7c2d5-112">Läs mer</span><span class="sxs-lookup"><span data-stu-id="7c2d5-112">Learn more</span></span>](site-recovery-exclude-disk.md)


## <a name="replicate-vms"></a><span data-ttu-id="7c2d5-113">Replikera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="7c2d5-113">Replicate VMs</span></span>

<span data-ttu-id="7c2d5-114">Aktivera replikering för virtuella datorer på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7c2d5-114">Enable replication for VMs as follows:</span></span>          

1. <span data-ttu-id="7c2d5-115">Klicka på **replikera program** > **källa**.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-115">Click **Replicate application** > **Source**.</span></span> <span data-ttu-id="7c2d5-116">När du har konfigurerat replikering för första gången, kan du klicka på **+ replikera** att aktivera replikering för ytterligare datorer.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-116">After you've set up replication for the first time, you can click **+Replicate** to enable replication for additional machines.</span></span>

    ![Aktivera replikering](./media/hyper-v-site-walkthrough-enable-replication/enable-replication.png)
2. <span data-ttu-id="7c2d5-118">I **källa**, välj platsen för Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-118">In **Source**, select the Hyper-V site.</span></span> <span data-ttu-id="7c2d5-119">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-119">Then click **OK**.</span></span>
3. <span data-ttu-id="7c2d5-120">I **mål**, Välj prenumerationen som valvet och växling vid fel modell du vill använda i Azure (klassiska eller resource management) efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-120">In **Target**, select the vault subscription, and the failover model you want to use in Azure (classic or resource management) after failover.</span></span>
4. <span data-ttu-id="7c2d5-121">Välj lagringskontot som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-121">Select the storage account you want to use.</span></span> <span data-ttu-id="7c2d5-122">Om du inte har ett konto som du vill använda, kan du [skapar du en](#set-up-an-azure-storage-account).</span><span class="sxs-lookup"><span data-stu-id="7c2d5-122">If you don't have an account you want to use, you can [create one](#set-up-an-azure-storage-account).</span></span> <span data-ttu-id="7c2d5-123">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-123">Then click **OK**.</span></span>
5. <span data-ttu-id="7c2d5-124">Välj Azure-nätverk och undernät som virtuella Azure-datorer ska ansluta till när de skapas växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-124">Select the Azure network and subnet to which Azure VMs will connect when they're created failover.</span></span>

    - <span data-ttu-id="7c2d5-125">Markera för att tillämpa nätverksinställningarna på alla datorer som du aktiverar för replikering **konfigurera nu för valda datorer**.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-125">To apply the network settings to all machines you enable for replication, select **Configure now for selected machines**.</span></span>
    - <span data-ttu-id="7c2d5-126">Välj **Konfigurera senare** om du vill välja Azure-nätverket för varje dator.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-126">Select **Configure later** to select the Azure network per machine.</span></span>
    - <span data-ttu-id="7c2d5-127">Om du inte har ett nätverk som du vill använda, kan du [skapar du en](#set-up-an-azure-network).</span><span class="sxs-lookup"><span data-stu-id="7c2d5-127">If you don't have a network you want to use, you can [create one](#set-up-an-azure-network).</span></span> <span data-ttu-id="7c2d5-128">Välj ett undernät om det behövs.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-128">Select a subnet if applicable.</span></span> <span data-ttu-id="7c2d5-129">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-129">Then click **OK**.</span></span>

   ![Aktivera replikering](./media/hyper-v-site-walkthrough-enable-replication/enable-replication11.png)

6. <span data-ttu-id="7c2d5-131">I **Virtual Machines** > **Välj virtuella datorer** klickar du på och väljer de datorer som du vill replikera.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-131">In **Virtual Machines** > **Select virtual machines**, click and select each machine you want to replicate.</span></span> <span data-ttu-id="7c2d5-132">Du kan bara välja datorer som stöder replikering.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-132">You can only select machines for which replication can be enabled.</span></span> <span data-ttu-id="7c2d5-133">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-133">Then click **OK**.</span></span>

    ![Aktivera replikering](./media/hyper-v-site-walkthrough-enable-replication/enable-replication5-for-exclude-disk.png)

7. <span data-ttu-id="7c2d5-135">I **Egenskaper** > **Konfigurera egenskaper** väljer du operativsystemet för de valda virtuella datorerna och operativsystemdisken.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-135">In **Properties** > **Configure properties**, select the operating system for the selected VMs, and the OS disk.</span></span>
8. <span data-ttu-id="7c2d5-136">Kontrollera att namnet på den virtuella datorn i Azure (målnamnet) uppfyller [kraven för virtuella datorer i Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="7c2d5-136">Verify that the Azure VM name (target name) complies with [Azure virtual machine requirements](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>
9. <span data-ttu-id="7c2d5-137">Som standard markeras alla diskar på den virtuella datorn för replikering.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-137">By default all the disks of the VM are selected for replication.</span></span> <span data-ttu-id="7c2d5-138">Rensa diskar för att utesluta dem.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-138">Clear disks to exclude them.</span></span>
10. <span data-ttu-id="7c2d5-139">Spara ändringarna genom att klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-139">Click **OK** to save changes.</span></span> <span data-ttu-id="7c2d5-140">Du kan ange ytterligare egenskaper senare.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-140">You can set additional properties later.</span></span>

    ![Aktivera replikering](./media/hyper-v-site-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

11. <span data-ttu-id="7c2d5-142">I **Replikeringsinställningar** > **Konfigurera replikeringsinställningar** väljer du den replikeringsprincip som du vill använda för de skyddade virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-142">In **Replication settings** > **Configure replication settings**, select the replication policy you want to apply for the protected VMs.</span></span> <span data-ttu-id="7c2d5-143">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-143">Then click **OK**.</span></span> <span data-ttu-id="7c2d5-144">Du kan ändra replikeringsprincipen i **replikeringsprinciper** > principnamn > **redigera inställningar för**.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-144">You can modify the replication policy in **Replication policies** > policy-name > **Edit Settings**.</span></span> <span data-ttu-id="7c2d5-145">De ändringar du gör används både för datorer som redan replikeras och för nya datorer.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-145">Changes you apply will be used for machines that are already replicating, and new machines.</span></span>


   ![Aktivera replikering](./media/hyper-v-site-walkthrough-enable-replication/enable-replication7.png)

<span data-ttu-id="7c2d5-147">Du kan följa förloppet för jobbet **Aktivera skydd** under **Jobb** > **Site Recovery-jobb**.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-147">You can track progress of the **Enable Protection** job in **Jobs** > **Site Recovery jobs**.</span></span> <span data-ttu-id="7c2d5-148">När jobbet **Slutför skydd** har körts är datorn redo för redundans.</span><span class="sxs-lookup"><span data-stu-id="7c2d5-148">After the **Finalize Protection** job runs the machine is ready for failover.</span></span>


## <a name="next-steps"></a><span data-ttu-id="7c2d5-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7c2d5-149">Next steps</span></span>


<span data-ttu-id="7c2d5-150">Gå till [steg 11: kör ett redundanstest](hyper-v-site-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="7c2d5-150">Go to [Step 11: Run a test failover](hyper-v-site-walkthrough-test-failover.md)</span></span>
