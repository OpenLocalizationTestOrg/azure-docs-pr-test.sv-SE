---
title: "aaaCreate ett valv för Hyper-V-replikering tooa sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur toocreate ett valv vid replikering av Hyper-V VMs tooa sekundär System Center VMM webbplats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ff65dbfb-cb26-410e-ab48-76971625db08
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 96ee09cbf2376a5089b9efa09dc7ab3fb7d472cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-create-a-vault-for-hyper-v-replication-tooa-secondary-site"></a><span data-ttu-id="bc26a-103">Steg 5: Skapa ett valv för Hyper-V-replikering tooa sekundär plats</span><span class="sxs-lookup"><span data-stu-id="bc26a-103">Step 5: Create a vault for Hyper-V replication tooa secondary site</span></span>

<span data-ttu-id="bc26a-104">När du har förberett lokalt [System Center Virtual Machine Manager (VMM) servrar och Hyper-V-värdar/kluster](vmm-to-vmm-walkthrough-vmm-hyper-v.md) för Hyper-V replikering tooa sekundär plats med hjälp av [Azure Site Recovery](site-recovery-overview.md), kan du skapa en Recovery Services-valvet och välj hello replikering scenario.</span><span class="sxs-lookup"><span data-stu-id="bc26a-104">After preparing on-premises [System Center Virtual Machine Manager (VMM) servers and Hyper-V hosts/clusters](vmm-to-vmm-walkthrough-vmm-hyper-v.md) for Hyper-V replication tooa secondary site using [Azure Site Recovery](site-recovery-overview.md), you can create a Recovery Services vault, and select hello replication scenario.</span></span>

<span data-ttu-id="bc26a-105">När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="bc26a-105">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="bc26a-106">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="bc26a-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a><span data-ttu-id="bc26a-107">Välj en skyddsmål</span><span class="sxs-lookup"><span data-stu-id="bc26a-107">Choose a protection goal</span></span>

<span data-ttu-id="bc26a-108">Välj vad du vill att tooreplicate och där du vill att tooreplicate till.</span><span class="sxs-lookup"><span data-stu-id="bc26a-108">Select what you want tooreplicate and where you want tooreplicate to.</span></span>

1. <span data-ttu-id="bc26a-109">Klicka på **Site Recovery** > **steg 1: Förbered infrastrukturen** > **skyddsmål**.</span><span class="sxs-lookup"><span data-stu-id="bc26a-109">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>
2. <span data-ttu-id="bc26a-110">Välj **toorecovery plats**, och välj **Ja, med Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="bc26a-110">Select **toorecovery site**, and select **Yes, with Hyper-V**.</span></span>
3. <span data-ttu-id="bc26a-111">Välj **Ja** tooindicate som du använder VMM toomanage hello Hyper-V-värdar.</span><span class="sxs-lookup"><span data-stu-id="bc26a-111">Select **Yes** tooindicate you're using VMM toomanage hello Hyper-V hosts.</span></span>
4. <span data-ttu-id="bc26a-112">Välj **Ja** om du har en sekundär VMM-server.</span><span class="sxs-lookup"><span data-stu-id="bc26a-112">Select **Yes** if you have a secondary VMM server.</span></span> <span data-ttu-id="bc26a-113">Om du distribuerar replikering mellan moln på en VMM-server, klickar du på **nr**.</span><span class="sxs-lookup"><span data-stu-id="bc26a-113">If you're deploying replication between clouds on a single VMM server, click **No**.</span></span> <span data-ttu-id="bc26a-114">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc26a-114">Then click **OK**.</span></span>

    ![Välja mål](./media/vmm-to-vmm-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a><span data-ttu-id="bc26a-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bc26a-116">Next steps</span></span>

<span data-ttu-id="bc26a-117">Gå för[steg 6: Konfigurera hello replikering käll- och](vmm-to-vmm-walkthrough-source-target.md).</span><span class="sxs-lookup"><span data-stu-id="bc26a-117">Go too[Step 6: Set up hello replication source and target](vmm-to-vmm-walkthrough-source-target.md).</span></span>
