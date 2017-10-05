---
title: "Konfigurera nätverksmappning för att replikera virtuella Hyper-V-datorer i VMM-moln till Azure med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur du konfigurerar nätverksmappning vid replikering av Hyper-V virtuella datorer i VMM-moln till Azure med Azure Site Recovery"
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
ms.openlocfilehash: ed6f73d8baea5af0d2aa5f0ae885f305911ccc82
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="step-9-configure-network-mapping-for-hyper-v-replication-with-vmm-to-azure"></a><span data-ttu-id="875c9-103">Steg 9: Konfigurera nätverksmappning för Hyper-V-replikering (med VMM) till Azure</span><span class="sxs-lookup"><span data-stu-id="875c9-103">Step 9: Configure network mapping for Hyper-V replication (with VMM) to Azure</span></span>

<span data-ttu-id="875c9-104">När du har skapat den [käll- och replikeringsinställningarna](vmm-to-azure-walkthrough-source-target.md), Använd den här artikeln för att konfigurera nätverksmappning att mappa mellan lokala nätverk för virtuell dator i VMM och Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="875c9-104">After you set up the [source and target replication settings](vmm-to-azure-walkthrough-source-target.md), use this article to configure network mapping to map between on-premises VMM VM networks, and Azure networks.</span></span>

<span data-ttu-id="875c9-105">Publicera kommentarer och frågor längst ned i den här artikeln eller i den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="875c9-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="875c9-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="875c9-106">Before you start</span></span>

- <span data-ttu-id="875c9-107">Lär dig mer om [nätverksmappning](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span><span class="sxs-lookup"><span data-stu-id="875c9-107">Learn about [network mapping](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span></span>
- <span data-ttu-id="875c9-108">[Förbereda VMM för nätverksmappning](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping).</span><span class="sxs-lookup"><span data-stu-id="875c9-108">[Prepare VMM for network mapping](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping).</span></span> 
- <span data-ttu-id="875c9-109">Kontrollera att virtuella datorer på VMM-servern är anslutna till ett virtuellt datornätverk och att du har skapat minst ett virtuellt Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="875c9-109">Verify that virtual machines on the VMM server are connected to a VM network, and that you've created at least one Azure virtual network.</span></span> <span data-ttu-id="875c9-110">Flera virtuella datornätverk kan mappas till ett enda Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="875c9-110">Multiple VM networks can be mapped to a single Azure network.</span></span>

## <a name="configure-mapping"></a><span data-ttu-id="875c9-111">Konfigurera mappning</span><span class="sxs-lookup"><span data-stu-id="875c9-111">Configure mapping</span></span>

<span data-ttu-id="875c9-112">Konfigurera mappning på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="875c9-112">Configure mapping as follows:</span></span>

1. <span data-ttu-id="875c9-113">Under **Site Recovery-infrastruktur** > **Nätverksmappningar** > **Nätverksmappning** klickar du på ikonen **+Nätverksmappning**.</span><span class="sxs-lookup"><span data-stu-id="875c9-113">In **Site Recovery Infrastructure** > **Network mappings** > **Network Mapping**, click the **+Network Mapping** icon.</span></span>

    ![Nätverksmappning](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping1.png)
2. <span data-ttu-id="875c9-115">På **Lägg till nätverksmappning** väljer du VMM-källservern och **Azure** som mål.</span><span class="sxs-lookup"><span data-stu-id="875c9-115">In **Add network mapping**, select the source VMM server, and **Azure** as the target.</span></span>
3. <span data-ttu-id="875c9-116">Kontrollera prenumerationen och distributionsmodellen efter redundansväxling.</span><span class="sxs-lookup"><span data-stu-id="875c9-116">Verify the subscription and the deployment model after failover.</span></span>
4. <span data-ttu-id="875c9-117">I **Källnätverk** väljer du det lokala VM-källnätverk som du vill mappa från listan som är associerad med VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="875c9-117">In **Source network**, select the source on-premises VM network you want to map from the list associated with the VMM server.</span></span>
5. <span data-ttu-id="875c9-118">I **Målnätverk** väljer du det Azure-nätverk som de virtuella Azure-replikdatorerna ska anslutas till när de skapas.</span><span class="sxs-lookup"><span data-stu-id="875c9-118">In **Target network**, select the Azure network in which replica Azure VMs will be located when they're created.</span></span> <span data-ttu-id="875c9-119">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="875c9-119">Then click **OK**.</span></span>

    ![Nätverksmappning](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping2.png)

<span data-ttu-id="875c9-121">Det här händer när nätverksmappningen börjar:</span><span class="sxs-lookup"><span data-stu-id="875c9-121">Here's what happens when network mapping begins:</span></span>

* <span data-ttu-id="875c9-122">Befintliga virtuella datorerna i VM-källnätverket ansluts till målnätverket när mappningen börjar.</span><span class="sxs-lookup"><span data-stu-id="875c9-122">Existing VMs on the source VM network are connected to the target network when mapping begins.</span></span> <span data-ttu-id="875c9-123">Nya virtuella datorer som är anslutna till VM-källnätverket ansluts till det mappade Azure-nätverket när replikeringen sker.</span><span class="sxs-lookup"><span data-stu-id="875c9-123">New VMs connected to the source VM network are connected to the mapped Azure network when replication occurs.</span></span>
* <span data-ttu-id="875c9-124">Om du ändrar en befintlig nätverksmappning ansluts virtuella replikdatorer med hjälp av de nya inställningarna.</span><span class="sxs-lookup"><span data-stu-id="875c9-124">If you modify an existing network mapping, replica virtual machines connect using the new settings.</span></span>
* <span data-ttu-id="875c9-125">Om målnätverket har flera undernät och ett av dessa undernät har samma namn som undernätet där den virtuella källdatorn finns så ansluts den virtuella replikdatorn till det målundernätverket efter en redundansväxling.</span><span class="sxs-lookup"><span data-stu-id="875c9-125">If the target network has multiple subnets, and one of those subnets has the same name as subnet on which the source virtual machine is located, then the replica virtual machine connects to that target subnet after failover.</span></span>
* <span data-ttu-id="875c9-126">Om det inte finns något målundernät med ett matchande namn ansluts den virtuella datorn till det första undernätet i nätverket.</span><span class="sxs-lookup"><span data-stu-id="875c9-126">If there’s no target subnet with a matching name, the virtual machine connects to the first subnet in the network.</span></span>



## <a name="next-steps"></a><span data-ttu-id="875c9-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="875c9-127">Next steps</span></span>

<span data-ttu-id="875c9-128">Gå till [steg 10: skapa en replikeringsprincip](vmm-to-azure-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="875c9-128">Go to [Step 10: Create a replication policy](vmm-to-azure-walkthrough-replication.md)</span></span>
