---
title: aaaMigrate Azure IaaS-VM mellan Azure-regioner | Microsoft Docs
description: "Använd Azure Site Recovery toomigrate Azure IaaS-virtuella datorer från en Azure-region tooanother."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8a29e0d9-0010-4739-972f-02b8bdf360f6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: c84dc77716b8d19969eab60707ed1332ca39b893
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a><span data-ttu-id="3e0ff-103">Migrera Azure IaaS-virtuella datorer mellan Azure-regioner med Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="3e0ff-103">Migrate Azure IaaS virtual machines between Azure regions with Azure Site Recovery</span></span>
## <a name="overview"></a><span data-ttu-id="3e0ff-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="3e0ff-104">Overview</span></span>
<span data-ttu-id="3e0ff-105">Välkommen tooAzure Site Recovery!</span><span class="sxs-lookup"><span data-stu-id="3e0ff-105">Welcome tooAzure Site Recovery!</span></span> <span data-ttu-id="3e0ff-106">Använd den här artikeln om du vill toomigrate Azure virtuella datorer mellan Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="3e0ff-106">Use this article if you want toomigrate Azure VMs between Azure regions.</span></span> <span data-ttu-id="3e0ff-107">Observera att innan du börjar:</span><span class="sxs-lookup"><span data-stu-id="3e0ff-107">Before you start, note that:</span></span>

* <span data-ttu-id="3e0ff-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="3e0ff-108">Azure has two different deployment models for creating and working with resources: Azure Resource Manager and classic.</span></span> <span data-ttu-id="3e0ff-109">Azure har också två portaler – hello klassiska Azure-portalen som stöder hello klassiska distributionsmodellen och hello Azure-portalen med stöd för båda distributionsmodellerna.</span><span class="sxs-lookup"><span data-stu-id="3e0ff-109">Azure also has two portals – hello Azure classic portal that supports hello classic deployment model, and hello Azure portal with support for both deployment models.</span></span> <span data-ttu-id="3e0ff-110">hello grundläggande anvisningar hello för migrering är samma om du konfigurerar Site Recovery i Resource Manager eller i klassiskt.</span><span class="sxs-lookup"><span data-stu-id="3e0ff-110">hello basic steps for migration are hello same whether you're configuring Site Recovery in Resource Manager or in classic.</span></span> <span data-ttu-id="3e0ff-111">Men hello UI anvisningar och skärmdumpar i den här artikeln är relevanta för hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3e0ff-111">However hello UI instructions and screenshots in this article are relevant for hello Azure portal.</span></span>
* <span data-ttu-id="3e0ff-112">**Du kan för närvarande endast migrera från en region tooanother. Du kan växla över virtuella datorer från en Azure-region tooanother, men du kan inte återställas dem igen.**</span><span class="sxs-lookup"><span data-stu-id="3e0ff-112">**Currently you can only migrate from one region tooanother. You can fail over VMs from one Azure region tooanother, but you can't fail them back again.**</span></span>

<span data-ttu-id="3e0ff-113">Skriv dina kommentarer eller frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="3e0ff-113">Post any comments or questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3e0ff-114">Krav</span><span class="sxs-lookup"><span data-stu-id="3e0ff-114">Prerequisites</span></span>
<span data-ttu-id="3e0ff-115">Här är vad du behöver för den här distributionen:</span><span class="sxs-lookup"><span data-stu-id="3e0ff-115">Here's what you need for this deployment:</span></span>

* <span data-ttu-id="3e0ff-116">**IaaS-virtuella datorer**: hello virtuella datorer du vill toomigrate.</span><span class="sxs-lookup"><span data-stu-id="3e0ff-116">**IaaS virtual machines**: hello VMs you want toomigrate.</span></span> <span data-ttu-id="3e0ff-117">Du kan migrera dessa virtuella datorer genom att behandla dem som fysiska datorer.</span><span class="sxs-lookup"><span data-stu-id="3e0ff-117">You migrate these VMs by treating them as physical machines.</span></span>

## <a name="deployment-steps"></a><span data-ttu-id="3e0ff-118">Distributionssteg</span><span class="sxs-lookup"><span data-stu-id="3e0ff-118">Deployment steps</span></span>
<span data-ttu-id="3e0ff-119">Det här avsnittet beskrivs hello distributionsstegen i hello nya Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3e0ff-119">This section describes hello deployment steps in hello new Azure portal.</span></span>

1. <span data-ttu-id="3e0ff-120">[Skapa ett valv](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="3e0ff-120">[Create a vault](site-recovery-vmware-to-azure.md).</span></span>
2. <span data-ttu-id="3e0ff-121">[Aktivera replikering](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="3e0ff-121">[Enable replication](site-recovery-vmware-to-azure.md).</span></span> <span data-ttu-id="3e0ff-122">Aktivera replikering för hello VMs toomigrate, och välj Azure som källa.</span><span class="sxs-lookup"><span data-stu-id="3e0ff-122">Enable replication for hello VMs you want toomigrate, and choose Azure as source.</span></span> 
3. <span data-ttu-id="3e0ff-123">[Kör en oplanerad redundansväxling](site-recovery-failover.md).</span><span class="sxs-lookup"><span data-stu-id="3e0ff-123">[ Run an unplanned failover](site-recovery-failover.md).</span></span> <span data-ttu-id="3e0ff-124">När den inledande replikeringen är klar kan köra du en oplanerad redundans från en Azure-region tooanother.</span><span class="sxs-lookup"><span data-stu-id="3e0ff-124">After initial replication is complete, you can run an unplanned failover from one Azure region tooanother.</span></span> <span data-ttu-id="3e0ff-125">Om du vill kan du skapa en återställningsplan och kör en oplanerad redundans toomigrate flera virtuella datorer mellan regioner.</span><span class="sxs-lookup"><span data-stu-id="3e0ff-125">Optionally, you can create a recovery plan and run an unplanned failover, toomigrate multiple virtual machines between regions.</span></span> <span data-ttu-id="3e0ff-126">[Lär dig mer](site-recovery-create-recovery-plans.md) om återställningsplaner.</span><span class="sxs-lookup"><span data-stu-id="3e0ff-126">[Learn more](site-recovery-create-recovery-plans.md) about recovery plans.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e0ff-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e0ff-127">Next steps</span></span>
<span data-ttu-id="3e0ff-128">Mer information om andra replikeringarna i [vad är Azure Site Recovery?](site-recovery-overview.md)</span><span class="sxs-lookup"><span data-stu-id="3e0ff-128">Learn more about other replication scenarios in [What is Azure Site Recovery?](site-recovery-overview.md)</span></span>
