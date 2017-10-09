---
title: "aaaSet upp ett valv för Azure VM repliction mellan regioner med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooset upp ett valv för Azure replikering mellan Azure-regioner med hjälp av Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 40472189-3d80-4963-b175-8bddcbc2f61f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: 9959c59c7ea57114763f13bf060404ddd267ba80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-set-up-a-vault-for-azure-tooazure-replication"></a><span data-ttu-id="f416b-103">Steg 4: Konfigurera ett valv för Azure tooAzure replikering</span><span class="sxs-lookup"><span data-stu-id="f416b-103">Step 4: Set up a vault for Azure tooAzure replication</span></span>

<span data-ttu-id="f416b-104">Efter [planera nätverk](azure-to-azure-walkthrough-network.md), Använd den här artikeln tooset upp ett valv för Azure virtuella datorer (VM) som replikerar tooanother Azure-region med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f416b-104">After [planning networks](azure-to-azure-walkthrough-network.md), use this article tooset up a vault, for Azure virtual machines (VMs) replicating tooanother Azure region, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

- <span data-ttu-id="f416b-105">När du är klar hello artikeln bör du ha ett Recovery Services-valv ställa in.</span><span class="sxs-lookup"><span data-stu-id="f416b-105">When you finish hello article, you should have a Recovery Services vault set up.</span></span>
- <span data-ttu-id="f416b-106">Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="f416b-106">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



>[!NOTE]
>
> <span data-ttu-id="f416b-107">Azure VM-replikering är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="f416b-107">Azure VM replication is currently in preview.</span></span>




## <a name="create-a-vault"></a><span data-ttu-id="f416b-108">Skapa ett valv</span><span class="sxs-lookup"><span data-stu-id="f416b-108">Create a vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> <span data-ttu-id="f416b-109">Vi rekommenderar att du skapar hello Recovery Services-valvet i hello plats där du vill att din tooreplicate för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f416b-109">We recommend that you create hello Recovery Services vault in hello location where you want your VMs tooreplicate.</span></span> <span data-ttu-id="f416b-110">Till exempel om din målplatsen är hello central oss, skapa hello valv i **centrala USA**.</span><span class="sxs-lookup"><span data-stu-id="f416b-110">For example, if your target location is hello central US, create hello vault in **Central US**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="f416b-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f416b-111">Next steps</span></span>

<span data-ttu-id="f416b-112">Gå för[steg 5: Aktivera replikering](azure-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="f416b-112">Go too[Step 5: Enable replication](azure-to-azure-walkthrough-enable-replication.md)</span></span>
