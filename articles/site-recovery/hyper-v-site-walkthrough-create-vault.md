---
title: "aaaSet upp ett valv för Hyper-V-replikering (utan VMM för System Center) tooAzure med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooset upp ett valv för Hyper-V-replikering tooAzure med hjälp av Azure Site Recovery"
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
ms.openlocfilehash: e3ef8758faab36d19d0968d98a23105bed7830f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a><span data-ttu-id="ca295-103">Steg 7: Konfigurera ett valv för Hyper-V-replikering</span><span class="sxs-lookup"><span data-stu-id="ca295-103">Step 7: Set up a vault for Hyper-V replication</span></span>

<span data-ttu-id="ca295-104">Den här artikeln beskriver hur tooset upp ett valv och ange vad du vill tooreplicate från din lokala plats, tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ca295-104">This article describes how tooset up a vault, and specify what you want tooreplicate from your on-premises location, tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="ca295-105">Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="ca295-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="ca295-106">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="ca295-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]



## <a name="select-a-protection-goal"></a><span data-ttu-id="ca295-107">Välj en skyddsmål</span><span class="sxs-lookup"><span data-stu-id="ca295-107">Select a protection goal</span></span>

<span data-ttu-id="ca295-108">Vad kan du välja tooreplicate, och där du vill tooreplicate till.</span><span class="sxs-lookup"><span data-stu-id="ca295-108">Select what you want tooreplicate, and where you want tooreplicate to.</span></span>

1. <span data-ttu-id="ca295-109">Klicka på **Recovery Services-valv** > valvet.</span><span class="sxs-lookup"><span data-stu-id="ca295-109">Click **Recovery Services vaults** > vault.</span></span>
2. <span data-ttu-id="ca295-110">I hello resurs-menyn, klickar du på **Site Recovery** > **Förbered infrastrukturen** > **skyddsmål**.</span><span class="sxs-lookup"><span data-stu-id="ca295-110">In hello Resource Menu, click **Site Recovery** > **Prepare Infrastructure** > **Protection goal**.</span></span>
3. <span data-ttu-id="ca295-111">I **skyddsmål**väljer **tooAzure** > **Ja, med Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="ca295-111">In **Protection goal**, select **tooAzure** > **Yes, with Hyper-V**.</span></span> <span data-ttu-id="ca295-112">Välj **nr** tooconfirm som du inte använder VMM.</span><span class="sxs-lookup"><span data-stu-id="ca295-112">Select **No** tooconfirm you're not using VMM.</span></span> 

    ![Välja mål](./media/hyper-v-site-walkthrough-create-vault/choose-goals2.png)



## <a name="next-steps"></a><span data-ttu-id="ca295-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ca295-114">Next steps</span></span>

<span data-ttu-id="ca295-115">Gå för[steg 8: Ställ in källa och mål](hyper-v-site-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="ca295-115">Go too[Step 8: Set up source and target](hyper-v-site-walkthrough-source-target.md)</span></span>
