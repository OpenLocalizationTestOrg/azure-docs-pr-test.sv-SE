---
title: "aaaReview hello arkitektur för replikering av virtuella Azure-datorer mellan Azure-regioner | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över komponenter och arkitektur som används när du replikerar virtuella Azure-datorer mellan Azure-regioner hello Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/12/2017
ms.author: raynew
ms.openlocfilehash: 4caab4e7a764040f317201d1345c40c73f836d81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-azure-vm-replication-between-azure-regions"></a><span data-ttu-id="ef522-103">Steg 1: Granska hello arkitektur för Azure VM replikering mellan Azure-regioner</span><span class="sxs-lookup"><span data-stu-id="ef522-103">Step 1: Review hello architecture for Azure VM replication between Azure regions</span></span>


<span data-ttu-id="ef522-104">När du har granskat hello [översikt över stegen](azure-to-azure-walkthrough-overview.md) läsa den här artikeln toounderstand hello komponenter och processer som används när replikering och återställa virtuella Azure-datorer (VM) från en Azure-region tooanother med för den här distributionen [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ef522-104">After reviewing hello [overview steps](azure-to-azure-walkthrough-overview.md) for this deployment, read this article toounderstand hello components and processes used when replicating and recovering Azure virtual machines (VMs) from one Azure region tooanother, using [Azure Site Recovery](site-recovery-overview.md).</span></span>

- <span data-ttu-id="ef522-105">Du bör ha en förståelse av hur Azure VM replikering tooanother region fungerar när du är klar hello artikel.</span><span class="sxs-lookup"><span data-stu-id="ef522-105">When you finish hello article, you should have a clear understanding of how Azure VM replication tooanother region works.</span></span>
- <span data-ttu-id="ef522-106">Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="ef522-106">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

>[!NOTE]
><span data-ttu-id="ef522-107">Azure VM-replikering med hello Site Recovery-tjänsten är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="ef522-107">Azure VM replication with hello Site Recovery service is currently in preview.</span></span>



## <a name="architectural-components"></a><span data-ttu-id="ef522-108">Arkitekturkomponenter</span><span class="sxs-lookup"><span data-stu-id="ef522-108">Architectural components</span></span>

<span data-ttu-id="ef522-109">hello följande diagram ger en övergripande bild av en virtuell dator i Azure-miljö i en viss region (i det här exemplet hello östra USA plats).</span><span class="sxs-lookup"><span data-stu-id="ef522-109">hello following diagram provides a high-level view of an Azure VM environment in a specific region (in this example, hello East US location).</span></span> <span data-ttu-id="ef522-110">I en miljö med virtuella Azure-datorn:</span><span class="sxs-lookup"><span data-stu-id="ef522-110">In an Azure VM environment:</span></span>
- <span data-ttu-id="ef522-111">Appar kan köras på virtuella datorer med diskar som är fördelade på storage-konton.</span><span class="sxs-lookup"><span data-stu-id="ef522-111">Apps can be running on VMs with disks spread across storage accounts.</span></span>
- <span data-ttu-id="ef522-112">hello virtuella datorer kan ingå i en eller flera undernät i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="ef522-112">hello VMs can be included in one or more subnets within a virtual network.</span></span>

![kund-miljö](./media/azure-to-azure-walkthrough-architecture/source-environment.png)

## <a name="replication-process"></a><span data-ttu-id="ef522-114">Replikeringsprocessen</span><span class="sxs-lookup"><span data-stu-id="ef522-114">Replication process</span></span>

### <a name="step-1"></a><span data-ttu-id="ef522-115">Steg 1</span><span class="sxs-lookup"><span data-stu-id="ef522-115">Step 1</span></span>

<span data-ttu-id="ef522-116">När du aktiverar Azure VM-replikering i hello Azure-portalen hello resurserna som visas i följande hello diagram och tabell skapas automatiskt i hello målregionen.</span><span class="sxs-lookup"><span data-stu-id="ef522-116">When you enable Azure VM replication in hello Azure portal, hello resources shown in hello following diagram and table are automatically created in hello target region.</span></span> <span data-ttu-id="ef522-117">Som standard skapas resurser baserat på inställningarna för datakälla region.</span><span class="sxs-lookup"><span data-stu-id="ef522-117">By default, resources are created based on source region settings.</span></span> <span data-ttu-id="ef522-118">Du kan anpassa hello Målinställningar efter behov.</span><span class="sxs-lookup"><span data-stu-id="ef522-118">You can customize hello target settings as required.</span></span> <span data-ttu-id="ef522-119">[Läs mer](site-recovery-replicate-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="ef522-119">[Learn more](site-recovery-replicate-azure-to-azure.md).</span></span>

![Aktivera replikeringen steg 1](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-1.png)

<span data-ttu-id="ef522-121">**Resurs**</span><span class="sxs-lookup"><span data-stu-id="ef522-121">**Resource**</span></span> | <span data-ttu-id="ef522-122">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="ef522-122">**Details**</span></span>
--- | ---
<span data-ttu-id="ef522-123">**Målresursgruppen**</span><span class="sxs-lookup"><span data-stu-id="ef522-123">**Target resource group**</span></span> | <span data-ttu-id="ef522-124">Hej resurs grupp toowhich replikerade virtuella datorer som tillhör efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="ef522-124">hello resource group toowhich replicated VMs belong after failover.</span></span>
<span data-ttu-id="ef522-125">**Mål virtuellt nätverk**</span><span class="sxs-lookup"><span data-stu-id="ef522-125">**Target virtual network**</span></span> | <span data-ttu-id="ef522-126">hello virtuellt nätverk där replikerade virtuella datorer finns efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="ef522-126">hello virtual network in which replicated VMs are located after failover.</span></span> <span data-ttu-id="ef522-127">En nätverksmappning skapas mellan käll- och virtuella nätverk och vice versa.</span><span class="sxs-lookup"><span data-stu-id="ef522-127">A network mapping is created between source and target virtual networks, and vice versa.</span></span>
<span data-ttu-id="ef522-128">**Cache-lagringskonton**</span><span class="sxs-lookup"><span data-stu-id="ef522-128">**Cache storage accounts**</span></span> | <span data-ttu-id="ef522-129">Innan ändringarna på källan är replikerats toohello mål-lagringskontot, de spåras och skickas toohello cache storage-konto i hello målplats.</span><span class="sxs-lookup"><span data-stu-id="ef522-129">Before changes on source VMs are replicated toohello target storage account, they are tracked and sent toohello cache storage account in hello target location.</span></span> <span data-ttu-id="ef522-130">Detta säkerställer minimal inverkan på produktion appar som körs på hello VM.</span><span class="sxs-lookup"><span data-stu-id="ef522-130">This ensures minimal impact on production apps running on hello VM.</span></span>
<span data-ttu-id="ef522-131">**Mål-lagringskonton**</span><span class="sxs-lookup"><span data-stu-id="ef522-131">**Target storage accounts**</span></span>  | <span data-ttu-id="ef522-132">Storage-konton i hello mål plats toowhich hello data replikeras.</span><span class="sxs-lookup"><span data-stu-id="ef522-132">Storage accounts in hello target location toowhich hello data is replicated.</span></span>
<span data-ttu-id="ef522-133">**Mål-tillgänglighetsuppsättningar**</span><span class="sxs-lookup"><span data-stu-id="ef522-133">**Target availability sets**</span></span>  | <span data-ttu-id="ef522-134">Tillgänglighetsuppsättningar i vilka hello replikerade virtuella datorer är placerade efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="ef522-134">Availability sets in which hello replicated VMs are located after failover.</span></span>

### <a name="step-2"></a><span data-ttu-id="ef522-135">Steg 2</span><span class="sxs-lookup"><span data-stu-id="ef522-135">Step 2</span></span>

<span data-ttu-id="ef522-136">Eftersom replikering är aktiverat, installeras automatiskt hello Site Recovery-tillägget mobilitetstjänsten på hello VM.</span><span class="sxs-lookup"><span data-stu-id="ef522-136">As replication is enabled, hello Site Recovery extension Mobility service is automatically installed on hello VM.</span></span> <span data-ttu-id="ef522-137">hello följande inträffar:</span><span class="sxs-lookup"><span data-stu-id="ef522-137">hello following occurs:</span></span>

1. <span data-ttu-id="ef522-138">hello VM har registrerats med Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="ef522-138">hello VM is registered with Site Recovery.</span></span>

2. <span data-ttu-id="ef522-139">Kontinuerlig replikering har konfigurerats för hello VM.</span><span class="sxs-lookup"><span data-stu-id="ef522-139">Continuous replication is configured for hello VM.</span></span> <span data-ttu-id="ef522-140">Data skrivs på hello Virtuella diskar som är kontinuerligt överförs toohello cache storage-konto i hello källplats.</span><span class="sxs-lookup"><span data-stu-id="ef522-140">Data writes on hello VM disks are continuously transferred toohello cache storage account in hello source location.</span></span>

   ![Aktivera replikeringen steg 2](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-2.png)

  
  <span data-ttu-id="ef522-142">Observera att Site Recovery aldrig måste inkommande anslutning toohello VM.</span><span class="sxs-lookup"><span data-stu-id="ef522-142">Note that Site Recovery never needs inbound connectivity toohello VM.</span></span> <span data-ttu-id="ef522-143">Endast utgående anslutning tooSite återställningstjänsten URL: er/IP-adresser, autentisering för Office 365-URL: er/IP-adresser och IP-adresser för cache storage-konto krävs.</span><span class="sxs-lookup"><span data-stu-id="ef522-143">Only outbound connectivity tooSite Recovery service URLs/IP addresses, Office 365 authentication URLs/IP addresses, and cache storage account IP addresses is needed.</span></span> 

## <a name="continuous-replication-process"></a><span data-ttu-id="ef522-144">Kontinuerlig replikeringsprocess</span><span class="sxs-lookup"><span data-stu-id="ef522-144">Continuous replication process</span></span>

<span data-ttu-id="ef522-145">När kontinuerlig replikering fungerar, överförs disk skrivningar är omedelbart toohello cache storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ef522-145">After continuous replication is working, disk writes are immediately transferred toohello cache storage account.</span></span> <span data-ttu-id="ef522-146">Site Recovery bearbetar hello data och skickar den toohello mål-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="ef522-146">Site Recovery processes hello data and sends it toohello target storage account.</span></span> <span data-ttu-id="ef522-147">När hello data bearbetas skapas återställningspunkter i hello mål-lagringskontot med några minuters mellanrum.</span><span class="sxs-lookup"><span data-stu-id="ef522-147">After hello data is processed, recovery points are generated in hello target storage account every few minutes.</span></span>

## <a name="failover-process"></a><span data-ttu-id="ef522-148">Failover-processen</span><span class="sxs-lookup"><span data-stu-id="ef522-148">Failover process</span></span>

<span data-ttu-id="ef522-149">När du påbörja en växling mål hello virtuella datorer skapas i hello målresursgruppen, virtuella målnätverket, mål-undernät och hello-tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="ef522-149">When you initiate a failover, hello VMs are created in hello target resource group, target virtual network, target subnet, and hello target availability set.</span></span> <span data-ttu-id="ef522-150">Du kan använda valfri återställningspunkt under en redundansväxling.</span><span class="sxs-lookup"><span data-stu-id="ef522-150">During a failover, you can use any recovery point.</span></span>

![Failover-processen](./media/azure-to-azure-walkthrough-architecture/failover.png)

## <a name="next-steps"></a><span data-ttu-id="ef522-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ef522-152">Next steps</span></span>

<span data-ttu-id="ef522-153">Gå för[steg 2: Verifiera förutsättningar och begränsningar](azure-to-azure-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="ef522-153">Go too[Step 2: Verify prerequisites and limitations](azure-to-azure-walkthrough-prerequisites.md)</span></span>
