---
title: "Ställ in ett valv för fysisk serverreplikering till Azure med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattas de steg som du behöver konfigurera ett valv för att replikera fysiska servrar till Azure med Azure Site Recovery"
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
ms.openlocfilehash: deb5ad0495edc969b374795eeb2698326dd4ff4d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="step-6-set-up-a-vault-for-physical-server-replication-to-azure"></a><span data-ttu-id="04420-103">Steg 6: Konfigurera ett valv för fysisk serverreplikering till Azure</span><span class="sxs-lookup"><span data-stu-id="04420-103">Step 6: Set up a vault for physical server replication to Azure</span></span>


<span data-ttu-id="04420-104">Den här artikeln beskriver hur du ställer in ett valv.</span><span class="sxs-lookup"><span data-stu-id="04420-104">This article describes how to set up a vault.</span></span> <span data-ttu-id="04420-105">Du skapar valvet och anger du vill replikera från den lokala platsen till Azure med hjälp av den [Azure Site Recovery](site-recovery-overview.md) tjänsten i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="04420-105">You create the vault, and specify what you want to replicate from your on-premises location to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>


<span data-ttu-id="04420-106">Publicera kommentarer och frågor längst ned i den här artikeln eller i den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="04420-106">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="04420-107">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="04420-107">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a><span data-ttu-id="04420-108">Välj en skyddsmål</span><span class="sxs-lookup"><span data-stu-id="04420-108">Select a protection goal</span></span>

<span data-ttu-id="04420-109">Välj vad och vart du vill replikera.</span><span class="sxs-lookup"><span data-stu-id="04420-109">Select what you want to replicate, and where you want to replicate to.</span></span>

1. <span data-ttu-id="04420-110">Klicka på **Recovery Services-valv** > valvet.</span><span class="sxs-lookup"><span data-stu-id="04420-110">Click **Recovery Services vaults** > vault.</span></span>
2. <span data-ttu-id="04420-111">Resurs-menyn klickar du på **Site Recovery** > **Förbered infrastrukturen** > **skyddsmål**.</span><span class="sxs-lookup"><span data-stu-id="04420-111">In the Resource Menu, click **Site Recovery** > **Prepare Infrastructure** > **Protection goal**.</span></span>
3. <span data-ttu-id="04420-112">I **skyddsmål**väljer **till Azure** > **inte virtualiserade/andra**.</span><span class="sxs-lookup"><span data-stu-id="04420-112">In **Protection goal**, select **To Azure** > **Not virtualized/Other**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="04420-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="04420-113">Next steps</span></span>

<span data-ttu-id="04420-114">Gå till [steg 7: Konfigurera källa och mål](physical-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="04420-114">Go to [Step 7: Set up source and target](physical-walkthrough-source-target.md)</span></span>
