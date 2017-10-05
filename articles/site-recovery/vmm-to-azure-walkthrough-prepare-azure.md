---
title: "Förbered Azure resurser för att replikera virtuella Hyper-V-datorer (med System Center VMM) till Azure med Azure Site Recovery | Microsoft Docs"
description: "Beskriver vad du behöver på plats i Azure innan du börjar replikera virtuella Hyper-V-datorer (med VMM) till Azure med Azure Site Recovery"
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
ms.openlocfilehash: 63b005f37ab5e15e8a1b4645446d65f1529f1bbd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-with-vmm-to-azure"></a><span data-ttu-id="ba5c3-103">Steg 5: Förbered Azure-resurser för Hyper-V-replikering (med VMM) till Azure</span><span class="sxs-lookup"><span data-stu-id="ba5c3-103">Step 5: Prepare Azure resources for Hyper-V replication (with VMM) to Azure</span></span>

<span data-ttu-id="ba5c3-104">När du har verifierat [krav på](vmm-to-azure-walkthrough-network.md), följ instruktionerna i den här artikeln för att förbereda Azure-resurser så att du kan replikera lokala Hyper-V virtuella datorer i System Center Virtual Machine Manager (VMM)-moln till Azure, med [ Azure Site Recovery](site-recovery-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-104">After verifying [network requirements](vmm-to-azure-walkthrough-network.md), use the instructions in this article to prepare Azure resources so that you can replicate on-premises Hyper-V VMs in System Center Virtual Machine Manager (VMM) clouds to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="ba5c3-105">När du har läst den här artikeln efter eventuella kommentarer längst ned eller tekniska frågor om den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="ba5c3-105">After reading this article, post any comments at the bottom, or ask technical questions on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-an-azure-account"></a><span data-ttu-id="ba5c3-106">Konfigurera ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="ba5c3-106">Set up an Azure account</span></span>

- <span data-ttu-id="ba5c3-107">Hämta en [Microsoft Azure-konto](http://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="ba5c3-107">Get a [Microsoft Azure account](http://azure.microsoft.com/).</span></span>
- <span data-ttu-id="ba5c3-108">Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ba5c3-108">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
- <span data-ttu-id="ba5c3-109">Kontrollera regionerna som stöds för Site Recovery Under geografisk tillgänglighet i [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="ba5c3-109">Check the supported regions for Site Recovery, Under Geographic Availability in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="ba5c3-110">Lär dig mer om [priserna för Site Recovery](site-recovery-faq.md#pricing), och få de [prisinformationen](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="ba5c3-110">Learn about [Site Recovery pricing](site-recovery-faq.md#pricing), and get the [pricing details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="ba5c3-111">Kontrollera att kontot har rätt [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)att skapa virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-111">Make sure your Azure account has the correct [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)to create Azure VMs.</span></span> <span data-ttu-id="ba5c3-112">[Lär dig mer](../active-directory/role-based-access-built-in-roles.md) om rollbaserad åtkomstkontroll i Azure.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-112">[Learn more](../active-directory/role-based-access-built-in-roles.md) about Azure role-based access control.</span></span>


## <a name="set-up-an-azure-network"></a><span data-ttu-id="ba5c3-113">Skapa ett Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="ba5c3-113">Set up an Azure network</span></span>

- <span data-ttu-id="ba5c3-114">Konfigurera en [Azure-nätverk](../virtual-network/virtual-network-get-started-vnet-subnet.md).</span><span class="sxs-lookup"><span data-stu-id="ba5c3-114">Set up an [Azure network](../virtual-network/virtual-network-get-started-vnet-subnet.md).</span></span> <span data-ttu-id="ba5c3-115">Virtuella Azure-datorer placeras i det här nätverket när de skapas efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-115">Azure VMs are placed in this network when they're created after failover.</span></span>
- <span data-ttu-id="ba5c3-116">Nätverket måste finnas i samma region som Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="ba5c3-116">The network should be in the same region as the Recovery Services vault</span></span>
- <span data-ttu-id="ba5c3-117">Site Recovery på Azure-portalen kan använda nätverk i [Resource Manager](../resource-manager-deployment-model.md), eller i klassiskt läge.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-117">Site Recovery in the Azure portal can use networks set up in [Resource Manager](../resource-manager-deployment-model.md), or in classic mode.</span></span>
- <span data-ttu-id="ba5c3-118">Vi rekommenderar att du konfigurerar ett nätverk innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-118">We recommend you set up a network before you begin.</span></span> <span data-ttu-id="ba5c3-119">Om du inte gör det måste du göra det under distributionen av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-119">If you don't, you need to do it during Site Recovery deployment.</span></span>
- <span data-ttu-id="ba5c3-120">Lär dig mer om [priser för virtuella nätverk](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="ba5c3-120">Learn about [virtual network pricing](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>


## <a name="set-up-an-azure-storage-account"></a><span data-ttu-id="ba5c3-121">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="ba5c3-121">Set up an Azure storage account</span></span>

- <span data-ttu-id="ba5c3-122">Site Recovery replikerar lokala datorer till Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-122">Site Recovery replicates on-premises machines to Azure storage.</span></span> <span data-ttu-id="ba5c3-123">Virtuella Azure-datorer skapas från lagringen efter redundansväxlingen.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-123">Azure VMs are created from the storage after failover occurs.</span></span>
- <span data-ttu-id="ba5c3-124">Konfigurera standard/premium [Azure storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account) att lagra data som replikeras till Azure.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-124">Set up a standard/premium [Azure storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) to hold data replicated to Azure.</span></span>
- <span data-ttu-id="ba5c3-125">[Premium-lagring](../storage/common/storage-premium-storage.md) används vanligtvis för virtuella datorer som behöver en konsekvent höga i/o-prestanda och låg fördröjning till värd-i/o-intensiv arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-125">[Premium storage](../storage/common/storage-premium-storage.md) is typically used for virtual machines that need a consistently high IO performance, and low latency to host IO intensive workloads.</span></span>
- <span data-ttu-id="ba5c3-126">Om du vill använda ett Premium Storage-konto för replikerade data konfigurerar du ytterligare ett standardlagringskonto för att lagra replikeringsloggar som samlar in löpande ändringar i lokala data.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-126">If you want to use a premium account to store replicated data, you also need a standard storage account to store replication logs that capture ongoing changes to on-premises data.</span></span>
- <span data-ttu-id="ba5c3-127">Beroende på vilken resursmodell som du vill använda för redundansväxlade virtuella Azure-datorer kan du konfigurera ett konto i [Resource Manager-läget](../storage/common/storage-create-storage-account.md), eller [klassiskt läge](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="ba5c3-127">Depending on the resource model you want to use for failed over Azure VMs, you set up an account in [Resource Manager mode](../storage/common/storage-create-storage-account.md), or [classic mode](../storage/common/storage-create-storage-account.md).</span></span>
- <span data-ttu-id="ba5c3-128">Vi rekommenderar att du ställer in ett storage-konto innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-128">We recommend that you set up a storage account before you begin.</span></span> <span data-ttu-id="ba5c3-129">Om du inte behöver göra det under distributionen av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-129">If you don't you need to do it during Site Recovery deployment.</span></span> <span data-ttu-id="ba5c3-130">Konton måste vara i samma region som Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-130">The accounts must be in the same region as the Recovery Services vault.</span></span>
- <span data-ttu-id="ba5c3-131">Du kan inte flytta storage-konton som används av Site Recovery i resursgrupper inom samma prenumeration, eller olika prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="ba5c3-131">You can't move storage accounts used by Site Recovery across resource groups within the same subscription, or across different subscriptions.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ba5c3-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ba5c3-132">Next steps</span></span>

<span data-ttu-id="ba5c3-133">Gå till [steg 6: förbereda VMM](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span><span class="sxs-lookup"><span data-stu-id="ba5c3-133">Go to [Step 6: Prepare VMM](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span></span>
