---
title: "aaaMap nätverk för Virtuella Hyper-V replikering tooa sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur toomap nätverk när du replikerar virtuella Hyper-V-datorer tooa sekundär VMM-plats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 461b7c1c-ef61-4005-aeec-2324e727a3d0
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: d4f621df4ce08ae055bc6809daea0b71b76754ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-map-networks-for-hyper-v-vm-replication-tooa-secondary-site"></a><span data-ttu-id="7fbf1-103">Steg 7: Mappa nätverk för Virtuella Hyper-V-replikering tooa sekundär plats</span><span class="sxs-lookup"><span data-stu-id="7fbf1-103">Step 7: Map networks for Hyper-V VM replication tooa secondary site</span></span>


<span data-ttu-id="7fbf1-104">När du har installerat [käll- och inställningar](vmm-to-vmm-walkthrough-source-target.md) för replikering av Hyper-V virtuella datorer (VM) tooa sekundär System Center Virtual Machine Manager (VMM) plats, använder du den här artikeln tooconfigure nätverksmappningen för Hyper-V virtuell dator (VM) replikering tooa sekundära platsen med [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7fbf1-104">After setting up [source and target settings](vmm-to-vmm-walkthrough-source-target.md) for replicating Hyper-V virtual machines (VMs) tooa secondary System Center Virtual Machine Manager (VMM) site, use this article tooconfigure network mapping for Hyper-V virtual machine (VM) replication tooa secondary site, using  [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="7fbf1-105">När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="7fbf1-105">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="before-you-start"></a><span data-ttu-id="7fbf1-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="7fbf1-106">Before you start</span></span>

- <span data-ttu-id="7fbf1-107">Lär dig mer om [nätverksmappning](vmm-to-vmm-walkthrough-network.md#network-mapping-overview) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="7fbf1-107">Learn about [network mapping](vmm-to-vmm-walkthrough-network.md#network-mapping-overview) before you start.</span></span>
- <span data-ttu-id="7fbf1-108">Kontrollera att virtuella datorer på VMM-servrar är anslutna tooa VM-nätverk.</span><span class="sxs-lookup"><span data-stu-id="7fbf1-108">Verify that virtual machines on VMM servers are connected tooa VM network.</span></span>

## <a name="configure-network-mapping"></a><span data-ttu-id="7fbf1-109">Konfigurera nätverksmappning</span><span class="sxs-lookup"><span data-stu-id="7fbf1-109">Configure network mapping</span></span>

1. <span data-ttu-id="7fbf1-110">I **nätverksmappning** > **nätverksmappningar**, klickar du på **+ nätverksmappning**.</span><span class="sxs-lookup"><span data-stu-id="7fbf1-110">In **Network Mapping** > **Network mappings**, click **+Network Mapping**.</span></span>
2. <span data-ttu-id="7fbf1-111">I **Lägg till nätverksmappning** väljer hello källa och mål VMM-servrar.</span><span class="sxs-lookup"><span data-stu-id="7fbf1-111">In **Add network mapping** tab, select hello source and target VMM servers.</span></span> <span data-ttu-id="7fbf1-112">hello Virtuella datornätverk kopplade till hello VMM-servrar har hämtats.</span><span class="sxs-lookup"><span data-stu-id="7fbf1-112">hello VM networks associated with hello VMM servers are retrieved.</span></span>
3. <span data-ttu-id="7fbf1-113">I **Källnätverk**väljer hello-nätverk som du vill toouse hello listan över Virtuella datornätverk kopplade till hello primära VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="7fbf1-113">In **Source network**, select hello network you want toouse from hello list of VM networks associated with hello primary VMM server.</span></span>
4. <span data-ttu-id="7fbf1-114">I **målnätverket**väljer hello-nätverk som du vill använda toouse på hello sekundär VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="7fbf1-114">In **Target network**, select hello network you want toouse on hello secondary VMM server.</span></span> <span data-ttu-id="7fbf1-115">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7fbf1-115">Then click **OK**.</span></span>

    ![Nätverksmappning](./media/vmm-to-vmm-walkthrough-network-mapping/network-mapping2.png)

<span data-ttu-id="7fbf1-117">Det här händer när nätverksmappningen börjar:</span><span class="sxs-lookup"><span data-stu-id="7fbf1-117">Here's what happens when network mapping begins:</span></span>

* <span data-ttu-id="7fbf1-118">Alla befintliga virtuella replikdatorer som motsvarar toohello källnätverket blir anslutna toohello mål VM-nätverk.</span><span class="sxs-lookup"><span data-stu-id="7fbf1-118">Any existing replica virtual machines that correspond toohello source VM network will be connected toohello target VM network.</span></span>
* <span data-ttu-id="7fbf1-119">Nya virtuella datorer som är anslutna toohello källnätverket blir anslutna toohello Målnätverk mappade efter replikeringen.</span><span class="sxs-lookup"><span data-stu-id="7fbf1-119">New virtual machines that are connected toohello source VM network will be connected toohello target mapped network after replication.</span></span>
* <span data-ttu-id="7fbf1-120">Om du ändrar en befintlig mappning med ett nytt nätverk ansluts virtuella replikdatorer med de nya inställningarna för hello.</span><span class="sxs-lookup"><span data-stu-id="7fbf1-120">If you modify an existing mapping with a new network, replica virtual machines will be connected using hello new settings.</span></span>
* <span data-ttu-id="7fbf1-121">Om hello målnätverket har flera undernät och ett av dessa undernät har hello samma namn som undernätet på vilka hello virtuella källdatorn finns så hello replikerade virtuella datorn kommer att vara anslutna toothat målundernätverket efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="7fbf1-121">If hello target network has multiple subnets and one of those subnets has hello same name as subnet on which hello source virtual machine is located, then hello replica virtual machine will be connected toothat target subnet after failover.</span></span> <span data-ttu-id="7fbf1-122">Om det inte finns något målundernät med ett matchande namn, att hello virtuella datorn anslutna toohello första undernätet i hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="7fbf1-122">If there’s no target subnet with a matching name, hello virtual machine will be connected toohello first subnet in hello network.</span></span>



## <a name="next-steps"></a><span data-ttu-id="7fbf1-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7fbf1-123">Next steps</span></span>

<span data-ttu-id="7fbf1-124">Gå för[steg 8: Konfigurera en replikeringsprincip](vmm-to-vmm-walkthrough-replication.md).</span><span class="sxs-lookup"><span data-stu-id="7fbf1-124">Go too[Step 8: Configure a replication policy](vmm-to-vmm-walkthrough-replication.md).</span></span>
