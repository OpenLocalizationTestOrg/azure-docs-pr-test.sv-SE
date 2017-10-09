---
title: "aaaPrepare Azure-resurser tooreplicate lokala virtuella VMware-datorer tooAzure med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Beskriver vad du behöver på plats i Azure innan du börjar replikera lokala virtuella VMware-datorer tooAzure med hjälp av Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 4e320d9b-8bb8-46bb-ba21-77c5d16748ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: ac72fff0593783add789408ecfeb1812d70108b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-prepare-azure-resources-for-vmware-replication-tooazure"></a><span data-ttu-id="7ff3b-103">Steg 5: Förbered Azure-resurser för tooAzure för VMWare-replikering</span><span class="sxs-lookup"><span data-stu-id="7ff3b-103">Step 5: Prepare Azure resources for VMWare replication tooAzure</span></span>


<span data-ttu-id="7ff3b-104">Använd hello instruktioner i den här artikeln tooprepare Azure resurser så att du kan replikera lokala datorer tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="7ff3b-104">Use hello instructions in this article tooprepare Azure resources so that you can replicate on-premises machines tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="7ff3b-105">Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="7ff3b-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="7ff3b-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="7ff3b-106">Before you start</span></span>

<span data-ttu-id="7ff3b-107">Kontrollera att du har läst hello [krav](vmware-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="7ff3b-107">Make sure you've read hello [prerequisites](vmware-walkthrough-prerequisites.md)</span></span>

## <a name="set-up-an-azure-account"></a><span data-ttu-id="7ff3b-108">Konfigurera ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="7ff3b-108">Set up an Azure account</span></span>

- <span data-ttu-id="7ff3b-109">Hämta en [Microsoft Azure-konto](http://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="7ff3b-109">Get a [Microsoft Azure account](http://azure.microsoft.com/).</span></span>
- <span data-ttu-id="7ff3b-110">Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7ff3b-110">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
- <span data-ttu-id="7ff3b-111">Kontrollera hello stöds regioner för Site Recovery Under geografisk tillgänglighet i [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="7ff3b-111">Check hello supported regions for Site Recovery, Under Geographic Availability in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="7ff3b-112">Lär dig mer om [priserna för Site Recovery](site-recovery-faq.md#pricing), och få hello [prisinformationen](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="7ff3b-112">Learn about [Site Recovery pricing](site-recovery-faq.md#pricing), and get hello [pricing details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>



## <a name="set-up-an-azure-network"></a><span data-ttu-id="7ff3b-113">Skapa ett Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="7ff3b-113">Set up an Azure network</span></span>

- <span data-ttu-id="7ff3b-114">Skapa ett Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="7ff3b-114">Set up an Azure network.</span></span> <span data-ttu-id="7ff3b-115">Virtuella Azure-datorer placeras i det här nätverket när de skapas efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="7ff3b-115">Azure VMs are placed in this network when they're created after failover.</span></span>
- <span data-ttu-id="7ff3b-116">Site Recovery hello Azure-portalen kan använda nätverk i [Resource Manager](../resource-manager-deployment-model.md), eller i klassiskt läge.</span><span class="sxs-lookup"><span data-stu-id="7ff3b-116">Site Recovery in hello Azure portal can use networks set up in [Resource Manager](../resource-manager-deployment-model.md), or in classic mode.</span></span>
- <span data-ttu-id="7ff3b-117">hello nätverket bör finnas i hello samma region som hello Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="7ff3b-117">hello network should be in hello same region as hello Recovery Services vault</span></span>
- <span data-ttu-id="7ff3b-118">Lär dig mer om [priser för virtuella nätverk](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="7ff3b-118">Learn about [virtual network pricing](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>
- <span data-ttu-id="7ff3b-119">Lär dig mer om [Virtuella Azure-anslutningen](site-recovery-network-design.md) efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="7ff3b-119">Learn more about [Azure VM connectivity](site-recovery-network-design.md) after failover.</span></span>


## <a name="set-up-an-azure-storage-account"></a><span data-ttu-id="7ff3b-120">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="7ff3b-120">Set up an Azure storage account</span></span>

- <span data-ttu-id="7ff3b-121">Site Recovery replikerar tooAzure lagring för lokala datorer.</span><span class="sxs-lookup"><span data-stu-id="7ff3b-121">Site Recovery replicates on-premises machines tooAzure storage.</span></span> <span data-ttu-id="7ff3b-122">Virtuella Azure-datorer skapas från hello lagring efter redundansväxlingen.</span><span class="sxs-lookup"><span data-stu-id="7ff3b-122">Azure VMs are created from hello storage after failover occurs.</span></span>
- <span data-ttu-id="7ff3b-123">Konfigurera en [Azure storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account) för replikerade data.</span><span class="sxs-lookup"><span data-stu-id="7ff3b-123">Set up an [Azure storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) for replicated data.</span></span>
- <span data-ttu-id="7ff3b-124">Site Recovery hello Azure-portalen kan använda storage-konton som skapas i Resource Manager eller i klassiskt läge.</span><span class="sxs-lookup"><span data-stu-id="7ff3b-124">Site Recovery in hello Azure portal can use storage accounts set up in Resource Manager, or in classic mode.</span></span>
- <span data-ttu-id="7ff3b-125">Hej lagringskontot kan vara standard eller [premium](../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="7ff3b-125">hello storage account can be standard or [premium](../storage/common/storage-premium-storage.md).</span></span>
- <span data-ttu-id="7ff3b-126">Om du konfigurerar ett premium-konto behöver du även en ytterligare standardkonto för loggdata.</span><span class="sxs-lookup"><span data-stu-id="7ff3b-126">If you set up a premium account, you will also need an additional standard account for log data.</span></span>


## <a name="next-steps"></a><span data-ttu-id="7ff3b-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7ff3b-127">Next steps</span></span>

<span data-ttu-id="7ff3b-128">Gå för[steg 6: förbereda VMware-resurser](vmware-walkthrough-prepare-vmware.md)</span><span class="sxs-lookup"><span data-stu-id="7ff3b-128">Go too[Step 6: Prepare VMware resources](vmware-walkthrough-prepare-vmware.md)</span></span>
