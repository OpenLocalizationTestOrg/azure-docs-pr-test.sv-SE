---
title: "Förbered Azure-resurser att replikera lokala fysiska servrar till Azure med Azure Site Recovery | Microsoft Docs"
description: "Beskriver vad du behöver på plats i Azure innan du börjar lokala servrar som replikerar till Azure med Azure Site Recovery-tjänsten"
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
ms.date: 06/25/2017
ms.author: raynew
ms.openlocfilehash: b7411fa6aba04ffd34f3f4bd03e706ca75afc9c8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-physical-server-replication-to-azure"></a><span data-ttu-id="9e44f-103">Steg 5: Förbered Azure-resurser för fysisk serverreplikering till Azure</span><span class="sxs-lookup"><span data-stu-id="9e44f-103">Step 5: Prepare Azure resources for physical server replication to Azure</span></span>


<span data-ttu-id="9e44f-104">Följ instruktionerna i den här artikeln för att förbereda Azure-resurser så att du kan replikera lokala servrar till Azure med hjälp av [Azure Site Recovery](site-recovery-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="9e44f-104">Use the instructions in this article to prepare Azure resources so that you can replicate on-premises servers to Azure using the [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="9e44f-105">Publicera kommentarer och frågor längst ned i den här artikeln eller i den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="9e44f-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="9e44f-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="9e44f-106">Before you start</span></span>

<span data-ttu-id="9e44f-107">Kontrollera att du har läst den [krav](physical-walkthrough-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="9e44f-107">Make sure you've read the [prerequisites](physical-walkthrough-prerequisites.md).</span></span>

## <a name="set-up-an-azure-account"></a><span data-ttu-id="9e44f-108">Konfigurera ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="9e44f-108">Set up an Azure account</span></span>

- <span data-ttu-id="9e44f-109">Hämta en [Microsoft Azure-konto](http://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="9e44f-109">Get a [Microsoft Azure account](http://azure.microsoft.com/).</span></span>
- <span data-ttu-id="9e44f-110">Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9e44f-110">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
- <span data-ttu-id="9e44f-111">Kontrollera regionerna som stöds för Site Recovery under **geografisk tillgänglighet** i [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="9e44f-111">Check the supported regions for Site Recovery, under **Geographic Availability** in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="9e44f-112">Lär dig mer om [priserna för Site Recovery](site-recovery-faq.md#pricing), och få de [prisinformationen](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="9e44f-112">Learn about [Site Recovery pricing](site-recovery-faq.md#pricing), and get the [pricing details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>



## <a name="set-up-an-azure-network"></a><span data-ttu-id="9e44f-113">Skapa ett Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="9e44f-113">Set up an Azure network</span></span>

- <span data-ttu-id="9e44f-114">Skapa ett Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="9e44f-114">Set up an Azure network.</span></span> <span data-ttu-id="9e44f-115">Virtuella Azure-datorer placeras i det här nätverket när de skapas efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="9e44f-115">Azure VMs are placed in this network when they're created after failover.</span></span>
- <span data-ttu-id="9e44f-116">Site Recovery på Azure-portalen kan använda nätverk i [Resource Manager](../resource-manager-deployment-model.md), eller i klassiskt läge.</span><span class="sxs-lookup"><span data-stu-id="9e44f-116">Site Recovery in the Azure portal can use networks set up in [Resource Manager](../resource-manager-deployment-model.md), or in classic mode.</span></span>
- <span data-ttu-id="9e44f-117">Nätverket måste finnas på samma region som Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="9e44f-117">The network should be in the same region as the Recovery Services vault.</span></span>
- <span data-ttu-id="9e44f-118">Lär dig mer om [priser för virtuella nätverk](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="9e44f-118">Learn about [virtual network pricing](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>
- <span data-ttu-id="9e44f-119">Lär dig mer om [Virtuella Azure-anslutningen](physical-walkthrough-network.md) efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="9e44f-119">Learn more about [Azure VM connectivity](physical-walkthrough-network.md) after failover.</span></span>


## <a name="set-up-an-azure-storage-account"></a><span data-ttu-id="9e44f-120">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="9e44f-120">Set up an Azure storage account</span></span>

- <span data-ttu-id="9e44f-121">Site Recovery replikerar lokala servrar till Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="9e44f-121">Site Recovery replicates on-premises servers to Azure storage.</span></span> <span data-ttu-id="9e44f-122">Virtuella Azure-datorer skapas från lagringen efter redundansväxlingen.</span><span class="sxs-lookup"><span data-stu-id="9e44f-122">Azure VMs are created from the storage after failover occurs.</span></span>
- <span data-ttu-id="9e44f-123">Konfigurera en [Azure storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account) för replikerade data.</span><span class="sxs-lookup"><span data-stu-id="9e44f-123">Set up an [Azure storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) for replicated data.</span></span>
- <span data-ttu-id="9e44f-124">Site Recovery på Azure-portalen kan använda storage-konton som skapas i Resource Manager eller i klassiskt läge.</span><span class="sxs-lookup"><span data-stu-id="9e44f-124">Site Recovery in the Azure portal can use storage accounts set up in Resource Manager, or in classic mode.</span></span>
- <span data-ttu-id="9e44f-125">Lagringskontot kan vara standard eller [premium](../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="9e44f-125">The storage account can be standard or [premium](../storage/common/storage-premium-storage.md).</span></span>
- <span data-ttu-id="9e44f-126">Om du konfigurerar ett premium-konto behöver du även en ytterligare standardkonto för loggdata.</span><span class="sxs-lookup"><span data-stu-id="9e44f-126">If you set up a premium account, you will also need an additional standard account for log data.</span></span>


## <a name="next-steps"></a><span data-ttu-id="9e44f-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9e44f-127">Next steps</span></span>

<span data-ttu-id="9e44f-128">Gå till [steg 6: konfigurera ett valv](physical-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="9e44f-128">Go to [Step 6: Set up a vault](physical-walkthrough-create-vault.md)</span></span>
