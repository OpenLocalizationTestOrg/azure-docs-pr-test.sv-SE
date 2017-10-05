---
title: "Ställ in ett valv för Hyper-V-replikering (utan System Center VMM) till Azure med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattas de steg som du måste ställa in ett valv för Hyper-V-replikering till Azure med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a936b3e2-0dbe-47ac-a98e-5285d96dc786
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 8212ff011633c3a89d3310e828b6d5f1cda6ce3f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a><span data-ttu-id="6a174-103">Steg 7: Konfigurera ett valv för Hyper-V-replikering</span><span class="sxs-lookup"><span data-stu-id="6a174-103">Step 7: Set up a vault for Hyper-V replication</span></span>

<span data-ttu-id="6a174-104">Den här artikeln beskriver hur du ställer in ett valv och ange vad du vill replikera från den lokala platsen till Azure med hjälp av [Azure Site Recovery](site-recovery-overview.md) tjänsten i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6a174-104">This article describes how to set up a vault, and specify what you want to replicate from your on-premises location, to Azure using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>


<span data-ttu-id="6a174-105">Publicera kommentarer och frågor längst ned i den här artikeln eller i den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="6a174-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="6a174-106">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="6a174-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]



## <a name="select-a-protection-goal"></a><span data-ttu-id="6a174-107">Välj en skyddsmål</span><span class="sxs-lookup"><span data-stu-id="6a174-107">Select a protection goal</span></span>

<span data-ttu-id="6a174-108">Välj vad och vart du vill replikera.</span><span class="sxs-lookup"><span data-stu-id="6a174-108">Select what you want to replicate, and where you want to replicate to.</span></span>

1. <span data-ttu-id="6a174-109">Klicka på **Recovery Services-valv** > valvet.</span><span class="sxs-lookup"><span data-stu-id="6a174-109">Click **Recovery Services vaults** > vault.</span></span>
2. <span data-ttu-id="6a174-110">Resurs-menyn klickar du på **Site Recovery** > **Förbered infrastrukturen** > **skyddsmål**.</span><span class="sxs-lookup"><span data-stu-id="6a174-110">In the Resource Menu, click **Site Recovery** > **Prepare Infrastructure** > **Protection goal**.</span></span>
3. <span data-ttu-id="6a174-111">I **skyddsmål**väljer **till Azure** > **Ja, med Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="6a174-111">In **Protection goal**, select **To Azure** > **Yes, with Hyper-V**.</span></span> <span data-ttu-id="6a174-112">Välj **nr** att bekräfta att du inte använder VMM.</span><span class="sxs-lookup"><span data-stu-id="6a174-112">Select **No** to confirm you're not using VMM.</span></span> 

    ![Välja mål](./media/hyper-v-site-walkthrough-create-vault/choose-goals2.png)



## <a name="next-steps"></a><span data-ttu-id="6a174-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6a174-114">Next steps</span></span>

<span data-ttu-id="6a174-115">Gå till [steg 8: Ställ in källa och mål](hyper-v-site-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="6a174-115">Go to [Step 8: Set up source and target](hyper-v-site-walkthrough-source-target.md)</span></span>
