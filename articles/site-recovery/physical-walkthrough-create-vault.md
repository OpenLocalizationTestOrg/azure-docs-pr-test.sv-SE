---
title: "aaaSet upp ett valv för fysisk server replication tooAzure med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooset in valvet tooreplicate fysiska servrar tooAzure med hjälp av Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99f2605-f417-4995-be77-5323136b814f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 988928e3ece31116823f132cc39223fe44443468
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-set-up-a-vault-for-physical-server-replication-tooazure"></a><span data-ttu-id="e60e3-103">Steg 6: Konfigurera ett valv för fysisk server replication tooAzure</span><span class="sxs-lookup"><span data-stu-id="e60e3-103">Step 6: Set up a vault for physical server replication tooAzure</span></span>


<span data-ttu-id="e60e3-104">Den här artikeln beskriver hur tooset upp ett valv.</span><span class="sxs-lookup"><span data-stu-id="e60e3-104">This article describes how tooset up a vault.</span></span> <span data-ttu-id="e60e3-105">Du skapar hello valvet och ange vad du vill tooreplicate från din lokala plats-tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e60e3-105">You create hello vault, and specify what you want tooreplicate from your on-premises location tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="e60e3-106">Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="e60e3-106">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="e60e3-107">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="e60e3-107">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a><span data-ttu-id="e60e3-108">Välj en skyddsmål</span><span class="sxs-lookup"><span data-stu-id="e60e3-108">Select a protection goal</span></span>

<span data-ttu-id="e60e3-109">Vad kan du välja tooreplicate, och där du vill tooreplicate till.</span><span class="sxs-lookup"><span data-stu-id="e60e3-109">Select what you want tooreplicate, and where you want tooreplicate to.</span></span>

1. <span data-ttu-id="e60e3-110">Klicka på **Recovery Services-valv** > valvet.</span><span class="sxs-lookup"><span data-stu-id="e60e3-110">Click **Recovery Services vaults** > vault.</span></span>
2. <span data-ttu-id="e60e3-111">I hello resurs-menyn, klickar du på **Site Recovery** > **Förbered infrastrukturen** > **skyddsmål**.</span><span class="sxs-lookup"><span data-stu-id="e60e3-111">In hello Resource Menu, click **Site Recovery** > **Prepare Infrastructure** > **Protection goal**.</span></span>
3. <span data-ttu-id="e60e3-112">I **skyddsmål**väljer **tooAzure** > **inte virtualiserade/andra**.</span><span class="sxs-lookup"><span data-stu-id="e60e3-112">In **Protection goal**, select **tooAzure** > **Not virtualized/Other**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="e60e3-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e60e3-113">Next steps</span></span>

<span data-ttu-id="e60e3-114">Gå för[steg 7: Konfigurera källa och mål](physical-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="e60e3-114">Go too[Step 7: Set up source and target](physical-walkthrough-source-target.md)</span></span>
