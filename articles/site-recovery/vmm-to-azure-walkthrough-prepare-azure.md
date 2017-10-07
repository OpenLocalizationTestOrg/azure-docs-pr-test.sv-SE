---
title: "aaaPrepare Azure-resurser tooreplicate Hyper-V virtuella datorer (med System Center VMM) tooAzure med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Beskriver vad du behöver på plats i Azure innan du börjar replikera virtuella Hyper-V-datorer (med VMM) tooAzure med hjälp av Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 1568bdc3-e767-477b-b040-f13699ab5644
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 86bfbab7722fe5bd5b93b92e398d1d441505d3b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-with-vmm-tooazure"></a><span data-ttu-id="6afdb-103">Steg 5: Förbered Azure-resurser för tooAzure för Hyper-V-replikering (med VMM)</span><span class="sxs-lookup"><span data-stu-id="6afdb-103">Step 5: Prepare Azure resources for Hyper-V replication (with VMM) tooAzure</span></span>

<span data-ttu-id="6afdb-104">När du har verifierat [krav på](vmm-to-azure-walkthrough-network.md), Använd hello instruktioner i den här artikeln tooprepare Azure resurser så att du kan replikera lokala Hyper-V virtuella datorer i System Center Virtual Machine Manager (VMM) moln tooAzure använder Hej [Azure Site Recovery](site-recovery-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="6afdb-104">After verifying [network requirements](vmm-to-azure-walkthrough-network.md), use hello instructions in this article tooprepare Azure resources so that you can replicate on-premises Hyper-V VMs in System Center Virtual Machine Manager (VMM) clouds tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="6afdb-105">När du har läst den här artikeln efter eventuella kommentarer längst ned hello eller tekniska frågor om hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="6afdb-105">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-an-azure-account"></a><span data-ttu-id="6afdb-106">Konfigurera ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="6afdb-106">Set up an Azure account</span></span>

- <span data-ttu-id="6afdb-107">Hämta en [Microsoft Azure-konto](http://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="6afdb-107">Get a [Microsoft Azure account](http://azure.microsoft.com/).</span></span>
- <span data-ttu-id="6afdb-108">Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6afdb-108">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
- <span data-ttu-id="6afdb-109">Kontrollera hello stöds regioner för Site Recovery Under geografisk tillgänglighet i [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="6afdb-109">Check hello supported regions for Site Recovery, Under Geographic Availability in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="6afdb-110">Lär dig mer om [priserna för Site Recovery](site-recovery-faq.md#pricing), och få hello [prisinformationen](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="6afdb-110">Learn about [Site Recovery pricing](site-recovery-faq.md#pricing), and get hello [pricing details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="6afdb-111">Kontrollera att ditt Azure-konto har rätt hello [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)toocreate virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="6afdb-111">Make sure your Azure account has hello correct [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)toocreate Azure VMs.</span></span> <span data-ttu-id="6afdb-112">[Lär dig mer](../active-directory/role-based-access-built-in-roles.md) om rollbaserad åtkomstkontroll i Azure.</span><span class="sxs-lookup"><span data-stu-id="6afdb-112">[Learn more](../active-directory/role-based-access-built-in-roles.md) about Azure role-based access control.</span></span>


## <a name="set-up-an-azure-network"></a><span data-ttu-id="6afdb-113">Skapa ett Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="6afdb-113">Set up an Azure network</span></span>

- <span data-ttu-id="6afdb-114">Konfigurera en [Azure-nätverk](../virtual-network/virtual-network-get-started-vnet-subnet.md).</span><span class="sxs-lookup"><span data-stu-id="6afdb-114">Set up an [Azure network](../virtual-network/virtual-network-get-started-vnet-subnet.md).</span></span> <span data-ttu-id="6afdb-115">Virtuella Azure-datorer placeras i det här nätverket när de skapas efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="6afdb-115">Azure VMs are placed in this network when they're created after failover.</span></span>
- <span data-ttu-id="6afdb-116">hello nätverket bör finnas i hello samma region som hello Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="6afdb-116">hello network should be in hello same region as hello Recovery Services vault</span></span>
- <span data-ttu-id="6afdb-117">Site Recovery hello Azure-portalen kan använda nätverk i [Resource Manager](../resource-manager-deployment-model.md), eller i klassiskt läge.</span><span class="sxs-lookup"><span data-stu-id="6afdb-117">Site Recovery in hello Azure portal can use networks set up in [Resource Manager](../resource-manager-deployment-model.md), or in classic mode.</span></span>
- <span data-ttu-id="6afdb-118">Vi rekommenderar att du konfigurerar ett nätverk innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="6afdb-118">We recommend you set up a network before you begin.</span></span> <span data-ttu-id="6afdb-119">Om du inte behöver du toodo under distributionen av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="6afdb-119">If you don't, you need toodo it during Site Recovery deployment.</span></span>
- <span data-ttu-id="6afdb-120">Lär dig mer om [priser för virtuella nätverk](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="6afdb-120">Learn about [virtual network pricing](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>


## <a name="set-up-an-azure-storage-account"></a><span data-ttu-id="6afdb-121">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="6afdb-121">Set up an Azure storage account</span></span>

- <span data-ttu-id="6afdb-122">Site Recovery replikerar tooAzure lagring för lokala datorer.</span><span class="sxs-lookup"><span data-stu-id="6afdb-122">Site Recovery replicates on-premises machines tooAzure storage.</span></span> <span data-ttu-id="6afdb-123">Virtuella Azure-datorer skapas från hello lagring efter redundansväxlingen.</span><span class="sxs-lookup"><span data-stu-id="6afdb-123">Azure VMs are created from hello storage after failover occurs.</span></span>
- <span data-ttu-id="6afdb-124">Konfigurera standard/premium [Azure storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account) toohold data replikeras tooAzure.</span><span class="sxs-lookup"><span data-stu-id="6afdb-124">Set up a standard/premium [Azure storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) toohold data replicated tooAzure.</span></span>
- <span data-ttu-id="6afdb-125">[Premium-lagring](../storage/common/storage-premium-storage.md) används vanligtvis för virtuella datorer som behöver en konsekvent höga i/o-prestanda och låg latens toohost-i/o-intensiv arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="6afdb-125">[Premium storage](../storage/common/storage-premium-storage.md) is typically used for virtual machines that need a consistently high IO performance, and low latency toohost IO intensive workloads.</span></span>
- <span data-ttu-id="6afdb-126">Om du vill toouse en premium konto toostore replikerade data måste du även en standardlagring konto toostore replikeringsloggar som avbilda pågående ändringar tooon lokala data.</span><span class="sxs-lookup"><span data-stu-id="6afdb-126">If you want toouse a premium account toostore replicated data, you also need a standard storage account toostore replication logs that capture ongoing changes tooon-premises data.</span></span>
- <span data-ttu-id="6afdb-127">Beroende på hello resursmodell du vill att toouse för redundansväxlade virtuella Azure-datorer kan du konfigurera ett konto i [Resource Manager-läget](../storage/common/storage-create-storage-account.md), eller [klassiskt läge](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="6afdb-127">Depending on hello resource model you want toouse for failed over Azure VMs, you set up an account in [Resource Manager mode](../storage/common/storage-create-storage-account.md), or [classic mode](../storage/common/storage-create-storage-account.md).</span></span>
- <span data-ttu-id="6afdb-128">Vi rekommenderar att du ställer in ett storage-konto innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="6afdb-128">We recommend that you set up a storage account before you begin.</span></span> <span data-ttu-id="6afdb-129">Om du inte behöver toodo under distributionen av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="6afdb-129">If you don't you need toodo it during Site Recovery deployment.</span></span> <span data-ttu-id="6afdb-130">hello konton måste finnas i hello samma region som hello Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="6afdb-130">hello accounts must be in hello same region as hello Recovery Services vault.</span></span>
- <span data-ttu-id="6afdb-131">Du kan inte flytta storage-konton används av Site Recovery över resursgrupper i hello samma prenumeration, eller mellan olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="6afdb-131">You can't move storage accounts used by Site Recovery across resource groups within hello same subscription, or across different subscriptions.</span></span>


## <a name="next-steps"></a><span data-ttu-id="6afdb-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6afdb-132">Next steps</span></span>

<span data-ttu-id="6afdb-133">Gå för[steg 6: Förbered VMM](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span><span class="sxs-lookup"><span data-stu-id="6afdb-133">Go too[Step 6: Prepare VMM](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span></span>
