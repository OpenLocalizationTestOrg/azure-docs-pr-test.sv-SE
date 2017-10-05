---
title: "Skapa ett valv för Hyper-V-replikering till en sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur du skapar ett valv vid replikering av Hyper-V virtuella datorer till en sekundär plats för System Center VMM med Azure Site Recovery."
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
ms.openlocfilehash: 28cfcf12b2e369f96664c163c0b6f2aa8a6ddcb9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="step-5-create-a-vault-for-hyper-v-replication-to-a-secondary-site"></a><span data-ttu-id="36e43-103">Steg 5: Skapa ett valv för Hyper-V-replikering till en sekundär plats</span><span class="sxs-lookup"><span data-stu-id="36e43-103">Step 5: Create a vault for Hyper-V replication to a secondary site</span></span>

<span data-ttu-id="36e43-104">När du har förberett lokalt [System Center Virtual Machine Manager (VMM) servrar och Hyper-V-värdar/kluster](vmm-to-vmm-walkthrough-vmm-hyper-v.md) för Hyper-V-replikering till en sekundär plats med hjälp av [Azure Site Recovery](site-recovery-overview.md), du kan skapa ett Recovery Services-valvet och välj scenario för replikering.</span><span class="sxs-lookup"><span data-stu-id="36e43-104">After preparing on-premises [System Center Virtual Machine Manager (VMM) servers and Hyper-V hosts/clusters](vmm-to-vmm-walkthrough-vmm-hyper-v.md) for Hyper-V replication to a secondary site using [Azure Site Recovery](site-recovery-overview.md), you can create a Recovery Services vault, and select the replication scenario.</span></span>

<span data-ttu-id="36e43-105">När du har läst den här artikeln kan du lämna kommentarer längst ned på sidan eller på [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="36e43-105">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="36e43-106">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="36e43-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a><span data-ttu-id="36e43-107">Välj en skyddsmål</span><span class="sxs-lookup"><span data-stu-id="36e43-107">Choose a protection goal</span></span>

<span data-ttu-id="36e43-108">Välj vad och vart du vill replikera.</span><span class="sxs-lookup"><span data-stu-id="36e43-108">Select what you want to replicate and where you want to replicate to.</span></span>

1. <span data-ttu-id="36e43-109">Klicka på **Site Recovery** > **steg 1: Förbered infrastrukturen** > **skyddsmål**.</span><span class="sxs-lookup"><span data-stu-id="36e43-109">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>
2. <span data-ttu-id="36e43-110">Välj **till återställningsplatsen**, och välj **Ja, med Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="36e43-110">Select **To recovery site**, and select **Yes, with Hyper-V**.</span></span>
3. <span data-ttu-id="36e43-111">Välj **Ja** att indikera att du använder VMM för att hantera Hyper-V-värdar.</span><span class="sxs-lookup"><span data-stu-id="36e43-111">Select **Yes** to indicate you're using VMM to manage the Hyper-V hosts.</span></span>
4. <span data-ttu-id="36e43-112">Välj **Ja** om du har en sekundär VMM-server.</span><span class="sxs-lookup"><span data-stu-id="36e43-112">Select **Yes** if you have a secondary VMM server.</span></span> <span data-ttu-id="36e43-113">Om du distribuerar replikering mellan moln på en VMM-server, klickar du på **nr**.</span><span class="sxs-lookup"><span data-stu-id="36e43-113">If you're deploying replication between clouds on a single VMM server, click **No**.</span></span> <span data-ttu-id="36e43-114">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="36e43-114">Then click **OK**.</span></span>

    ![Välja mål](./media/vmm-to-vmm-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a><span data-ttu-id="36e43-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="36e43-116">Next steps</span></span>

<span data-ttu-id="36e43-117">Gå till [steg 6: Konfigurera replikering käll- och](vmm-to-vmm-walkthrough-source-target.md).</span><span class="sxs-lookup"><span data-stu-id="36e43-117">Go to [Step 6: Set up the replication source and target](vmm-to-vmm-walkthrough-source-target.md).</span></span>