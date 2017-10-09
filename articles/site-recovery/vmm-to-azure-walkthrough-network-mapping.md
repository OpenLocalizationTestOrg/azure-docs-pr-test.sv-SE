---
title: "aaaConfigure nätverksmappningen för att replikera virtuella Hyper-V-datorer i VMM-moln tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur tooconfigure nätverksmappning vid replikering av Hyper-V virtuella datorer i VMM moln tooAzure med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 081a9fdb0ffa4114099e9bcb9c1b1e43ad26ecbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-configure-network-mapping-for-hyper-v-replication-with-vmm-tooazure"></a><span data-ttu-id="f9e01-103">Steg 9: Konfigurera nätverksmappning för tooAzure för Hyper-V-replikering (med VMM)</span><span class="sxs-lookup"><span data-stu-id="f9e01-103">Step 9: Configure network mapping for Hyper-V replication (with VMM) tooAzure</span></span>

<span data-ttu-id="f9e01-104">När du har skapat hello [käll- och replikeringsinställningarna](vmm-to-azure-walkthrough-source-target.md), Använd den här artikeln tooconfigure nätverk mappning toomap mellan lokala nätverk för virtuell dator i VMM och Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="f9e01-104">After you set up hello [source and target replication settings](vmm-to-azure-walkthrough-source-target.md), use this article tooconfigure network mapping toomap between on-premises VMM VM networks, and Azure networks.</span></span>

<span data-ttu-id="f9e01-105">Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="f9e01-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="f9e01-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="f9e01-106">Before you start</span></span>

- <span data-ttu-id="f9e01-107">Lär dig mer om [nätverksmappning](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span><span class="sxs-lookup"><span data-stu-id="f9e01-107">Learn about [network mapping](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span></span>
- <span data-ttu-id="f9e01-108">[Förbereda VMM för nätverksmappning](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping).</span><span class="sxs-lookup"><span data-stu-id="f9e01-108">[Prepare VMM for network mapping](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping).</span></span> 
- <span data-ttu-id="f9e01-109">Kontrollera att virtuella datorer på hello VMM-servern är ansluten tooa Virtuellt datornätverk och att du har skapat minst ett virtuellt Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="f9e01-109">Verify that virtual machines on hello VMM server are connected tooa VM network, and that you've created at least one Azure virtual network.</span></span> <span data-ttu-id="f9e01-110">Flera Virtuella datornätverk kan vara mappade tooa enda Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="f9e01-110">Multiple VM networks can be mapped tooa single Azure network.</span></span>

## <a name="configure-mapping"></a><span data-ttu-id="f9e01-111">Konfigurera mappning</span><span class="sxs-lookup"><span data-stu-id="f9e01-111">Configure mapping</span></span>

<span data-ttu-id="f9e01-112">Konfigurera mappning på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f9e01-112">Configure mapping as follows:</span></span>

1. <span data-ttu-id="f9e01-113">I **Site Recovery-infrastruktur** > **nätverksmappningar** > **nätverksmappning**, klicka på hello **+ nätverksmappning**  ikon.</span><span class="sxs-lookup"><span data-stu-id="f9e01-113">In **Site Recovery Infrastructure** > **Network mappings** > **Network Mapping**, click hello **+Network Mapping** icon.</span></span>

    ![Nätverksmappning](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping1.png)
2. <span data-ttu-id="f9e01-115">I **Lägg till nätverksmappning**väljer hello VMM-källservern och **Azure** som hello mål.</span><span class="sxs-lookup"><span data-stu-id="f9e01-115">In **Add network mapping**, select hello source VMM server, and **Azure** as hello target.</span></span>
3. <span data-ttu-id="f9e01-116">Kontrollera hello prenumeration och hello distributionsmodell efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="f9e01-116">Verify hello subscription and hello deployment model after failover.</span></span>
4. <span data-ttu-id="f9e01-117">I **Källnätverk**väljer hello lokala VM-källnätverket önskade toomap hello listan som är associerade med hello VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="f9e01-117">In **Source network**, select hello source on-premises VM network you want toomap from hello list associated with hello VMM server.</span></span>
5. <span data-ttu-id="f9e01-118">I **målnätverket**, Välj hello Azure-nätverk i vilka replik virtuella Azure-datorer kommer att finnas när de skapas.</span><span class="sxs-lookup"><span data-stu-id="f9e01-118">In **Target network**, select hello Azure network in which replica Azure VMs will be located when they're created.</span></span> <span data-ttu-id="f9e01-119">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f9e01-119">Then click **OK**.</span></span>

    ![Nätverksmappning](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping2.png)

<span data-ttu-id="f9e01-121">Det här händer när nätverksmappningen börjar:</span><span class="sxs-lookup"><span data-stu-id="f9e01-121">Here's what happens when network mapping begins:</span></span>

* <span data-ttu-id="f9e01-122">Befintliga virtuella datorer i VM-källnätverket hello är anslutna toohello målnätverket när mappningen börjar.</span><span class="sxs-lookup"><span data-stu-id="f9e01-122">Existing VMs on hello source VM network are connected toohello target network when mapping begins.</span></span> <span data-ttu-id="f9e01-123">Nya virtuella datorer anslutna toohello källnätverket ansluts toohello mappade Azure-nätverket när replikeringen sker.</span><span class="sxs-lookup"><span data-stu-id="f9e01-123">New VMs connected toohello source VM network are connected toohello mapped Azure network when replication occurs.</span></span>
* <span data-ttu-id="f9e01-124">Om du ändrar en befintlig nätverksmappning ansluts virtuella replikdatorer med hello nya inställningar.</span><span class="sxs-lookup"><span data-stu-id="f9e01-124">If you modify an existing network mapping, replica virtual machines connect using hello new settings.</span></span>
* <span data-ttu-id="f9e01-125">Om hello målnätverket har flera undernät och ett av dessa undernät har hello samma namn som undernätet som hello källa virtual machine finns så hello replikerade virtuella datorn ansluter toothat målundernätverket efter en redundansväxling.</span><span class="sxs-lookup"><span data-stu-id="f9e01-125">If hello target network has multiple subnets, and one of those subnets has hello same name as subnet on which hello source virtual machine is located, then hello replica virtual machine connects toothat target subnet after failover.</span></span>
* <span data-ttu-id="f9e01-126">Om det inte finns något målundernät med ett matchande namn, ansluter hello virtuella toohello första undernätet i hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="f9e01-126">If there’s no target subnet with a matching name, hello virtual machine connects toohello first subnet in hello network.</span></span>



## <a name="next-steps"></a><span data-ttu-id="f9e01-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f9e01-127">Next steps</span></span>

<span data-ttu-id="f9e01-128">Gå för[steg 10: skapa en replikeringsprincip](vmm-to-azure-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="f9e01-128">Go too[Step 10: Create a replication policy](vmm-to-azure-walkthrough-replication.md)</span></span>
