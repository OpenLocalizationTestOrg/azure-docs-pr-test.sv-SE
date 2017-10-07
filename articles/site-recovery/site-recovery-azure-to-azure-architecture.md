---
title: "aaaHow stöder virtuella Azure-datorreplikering mellan Azure-regioner arbete i Azure Site Recovery?  | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över komponenter och arkitektur som används när du replikerar virtuella Azure-datorer mellan Azure-regioner med hjälp av hello Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/29/2017
ms.author: sujayt
ms.openlocfilehash: 01eda83e490821f8afc8a2973c18a19453a2e84a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-vm-replication-work-in-site-recovery"></a><span data-ttu-id="5e788-104">Hur fungerar Azure VM-replikering i Site Recovery?</span><span class="sxs-lookup"><span data-stu-id="5e788-104">How does Azure VM replication work in Site Recovery?</span></span>


<span data-ttu-id="5e788-105">Den här artikeln beskriver hello komponenter och processer som är involverad i replikera och återställa virtuella Azure-datorer (VM) från en region tooanother med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="5e788-105">This article describes hello components and processes involved in replicating and recovering Azure virtual machines (VMs) from one region tooanother by using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

>[!NOTE]
><span data-ttu-id="5e788-106">Azure VM-replikering med hello Site Recovery-tjänsten är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="5e788-106">Azure VM replication with hello Site Recovery service is currently in preview.</span></span>

<span data-ttu-id="5e788-107">Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="5e788-107">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="architectural-components"></a><span data-ttu-id="5e788-108">Arkitekturkomponenter</span><span class="sxs-lookup"><span data-stu-id="5e788-108">Architectural components</span></span>

<span data-ttu-id="5e788-109">hello följande diagram ger en övergripande bild av en virtuell dator i Azure-miljö i en viss region (i det här exemplet hello östra USA plats).</span><span class="sxs-lookup"><span data-stu-id="5e788-109">hello following diagram provides a high-level view of an Azure VM environment in a specific region (in this example, hello East US location).</span></span> <span data-ttu-id="5e788-110">I en miljö med virtuella Azure-datorn:</span><span class="sxs-lookup"><span data-stu-id="5e788-110">In an Azure VM environment:</span></span>
- <span data-ttu-id="5e788-111">Appar kan köras på virtuella datorer med diskar som är fördelade på storage-konton.</span><span class="sxs-lookup"><span data-stu-id="5e788-111">Apps can be running on VMs with disks spread across storage accounts.</span></span>
- <span data-ttu-id="5e788-112">hello virtuella datorer kan ingå i en eller flera undernät i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="5e788-112">hello VMs can be included in one or more subnets within a virtual network.</span></span>

![kund-miljö](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

<span data-ttu-id="5e788-114">Lär dig mer om kraven för distribution av hello och kraven i hello [supportmatrisen](site-recovery-support-matrix-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="5e788-114">Learn about hello deployment prerequisites and requirements in hello [support matrix](site-recovery-support-matrix-azure-to-azure.md).</span></span>

## <a name="replication-process"></a><span data-ttu-id="5e788-115">Replikeringsprocessen</span><span class="sxs-lookup"><span data-stu-id="5e788-115">Replication process</span></span>

### <a name="step-1"></a><span data-ttu-id="5e788-116">Steg 1</span><span class="sxs-lookup"><span data-stu-id="5e788-116">Step 1</span></span>

<span data-ttu-id="5e788-117">När du aktiverar Azure VM-replikering i hello Azure-portalen hello resurserna som visas i följande hello diagram och tabell skapas automatiskt i hello målregionen.</span><span class="sxs-lookup"><span data-stu-id="5e788-117">When you enable Azure VM replication in hello Azure portal, hello resources shown in hello following diagram and table are automatically created in hello target region.</span></span> <span data-ttu-id="5e788-118">Som standard skapas resurser baserat på inställningarna för datakälla region.</span><span class="sxs-lookup"><span data-stu-id="5e788-118">By default, resources are created based on source region settings.</span></span> <span data-ttu-id="5e788-119">Du kan anpassa hello Målinställningar efter behov.</span><span class="sxs-lookup"><span data-stu-id="5e788-119">You can customize hello target settings as required.</span></span> <span data-ttu-id="5e788-120">[Läs mer](site-recovery-replicate-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="5e788-120">[Learn more](site-recovery-replicate-azure-to-azure.md).</span></span>

![Aktivera replikeringen steg 1](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-1.png)

<span data-ttu-id="5e788-122">**Resurs**</span><span class="sxs-lookup"><span data-stu-id="5e788-122">**Resource**</span></span> | <span data-ttu-id="5e788-123">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="5e788-123">**Details**</span></span>
--- | ---
<span data-ttu-id="5e788-124">**Målresursgruppen**</span><span class="sxs-lookup"><span data-stu-id="5e788-124">**Target resource group**</span></span> | <span data-ttu-id="5e788-125">Hej resurs grupp toowhich replikerade virtuella datorer som tillhör efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="5e788-125">hello resource group toowhich replicated VMs belong after failover.</span></span>
<span data-ttu-id="5e788-126">**Mål virtuellt nätverk**</span><span class="sxs-lookup"><span data-stu-id="5e788-126">**Target virtual network**</span></span> | <span data-ttu-id="5e788-127">hello virtuellt nätverk där replikerade virtuella datorer finns efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="5e788-127">hello virtual network in which replicated VMs are located after failover.</span></span> <span data-ttu-id="5e788-128">En nätverksmappning skapas mellan käll- och virtuella nätverk och vice versa.</span><span class="sxs-lookup"><span data-stu-id="5e788-128">A network mapping is created between source and target virtual networks, and vice versa.</span></span>
<span data-ttu-id="5e788-129">**Cache-lagringskonton**</span><span class="sxs-lookup"><span data-stu-id="5e788-129">**Cache storage accounts**</span></span> | <span data-ttu-id="5e788-130">Innan ändringarna på källan är replikerats toohello mål-lagringskontot, de spåras och skickas toohello cache storage-konto i hello målplats.</span><span class="sxs-lookup"><span data-stu-id="5e788-130">Before changes on source VMs are replicated toohello target storage account, they are tracked and sent toohello cache storage account in hello target location.</span></span> <span data-ttu-id="5e788-131">Detta säkerställer minimal inverkan på produktion appar som körs på hello VM.</span><span class="sxs-lookup"><span data-stu-id="5e788-131">This ensures minimal impact on production apps running on hello VM.</span></span>
<span data-ttu-id="5e788-132">**Mål-lagringskonton**</span><span class="sxs-lookup"><span data-stu-id="5e788-132">**Target storage accounts**</span></span>  | <span data-ttu-id="5e788-133">Storage-konton i hello mål plats toowhich hello data replikeras.</span><span class="sxs-lookup"><span data-stu-id="5e788-133">Storage accounts in hello target location toowhich hello data is replicated.</span></span>
<span data-ttu-id="5e788-134">**Mål-tillgänglighetsuppsättningar**</span><span class="sxs-lookup"><span data-stu-id="5e788-134">**Target availability sets**</span></span>  | <span data-ttu-id="5e788-135">Tillgänglighetsuppsättningar i vilka hello replikerade virtuella datorer är placerade efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="5e788-135">Availability sets in which hello replicated VMs are located after failover.</span></span>

### <a name="step-2"></a><span data-ttu-id="5e788-136">Steg 2</span><span class="sxs-lookup"><span data-stu-id="5e788-136">Step 2</span></span>

<span data-ttu-id="5e788-137">Eftersom replikering är aktiverat, installeras automatiskt hello Site Recovery-tillägget mobilitetstjänsten på hello VM.</span><span class="sxs-lookup"><span data-stu-id="5e788-137">As replication is enabled, hello Site Recovery extension Mobility service is automatically installed on hello VM.</span></span> <span data-ttu-id="5e788-138">hello följande inträffar:</span><span class="sxs-lookup"><span data-stu-id="5e788-138">hello following occurs:</span></span>

1. <span data-ttu-id="5e788-139">hello VM har registrerats med Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="5e788-139">hello VM is registered with Site Recovery.</span></span>

2. <span data-ttu-id="5e788-140">Kontinuerlig replikering har konfigurerats för hello VM.</span><span class="sxs-lookup"><span data-stu-id="5e788-140">Continuous replication is configured for hello VM.</span></span> <span data-ttu-id="5e788-141">Data skrivs på hello Virtuella diskar som är kontinuerligt överförs toohello cache storage-konto i hello källplats.</span><span class="sxs-lookup"><span data-stu-id="5e788-141">Data writes on hello VM disks are continuously transferred toohello cache storage account in hello source location.</span></span>

   ![Aktivera replikeringen steg 2](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-2.png)

   >[!IMPORTANT]
   > <span data-ttu-id="5e788-143">Site Recovery behöver aldrig inkommande anslutning toohello VM.</span><span class="sxs-lookup"><span data-stu-id="5e788-143">Site Recovery never needs inbound connectivity toohello VM.</span></span> <span data-ttu-id="5e788-144">hello VM behöver endast utgående anslutning tooSite återställning URL: er/IP-adresser, Office 365-autentisering URL: er/IP-adresser och IP-adresser för cache storage-konto.</span><span class="sxs-lookup"><span data-stu-id="5e788-144">hello VM needs only outbound connectivity tooSite Recovery service URLs/IP addresses, Office 365 authentication URLs/IP addresses, and cache storage account IP addresses.</span></span> <span data-ttu-id="5e788-145">Mer information finns i hello [nätverk vägledning för att replikera virtuella datorer i Azure](site-recovery-azure-to-azure-networking-guidance.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="5e788-145">For more information, see hello [Networking guidance for replicating Azure virtual machines](site-recovery-azure-to-azure-networking-guidance.md) article.</span></span>

## <a name="continuous-replication-process"></a><span data-ttu-id="5e788-146">Kontinuerlig replikeringsprocess</span><span class="sxs-lookup"><span data-stu-id="5e788-146">Continuous replication process</span></span>

<span data-ttu-id="5e788-147">När kontinuerlig replikering fungerar, överförs disk skrivningar är omedelbart toohello cache storage-konto.</span><span class="sxs-lookup"><span data-stu-id="5e788-147">After continuous replication is working, disk writes are immediately transferred toohello cache storage account.</span></span> <span data-ttu-id="5e788-148">Site Recovery bearbetar hello data och skickar den toohello mål-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="5e788-148">Site Recovery processes hello data and sends it toohello target storage account.</span></span> <span data-ttu-id="5e788-149">När hello data bearbetas skapas återställningspunkter i hello mål-lagringskontot med några minuters mellanrum.</span><span class="sxs-lookup"><span data-stu-id="5e788-149">After hello data is processed, recovery points are generated in hello target storage account every few minutes.</span></span>

## <a name="failover-process"></a><span data-ttu-id="5e788-150">Failover-processen</span><span class="sxs-lookup"><span data-stu-id="5e788-150">Failover process</span></span>

<span data-ttu-id="5e788-151">När du påbörja en växling mål hello virtuella datorer skapas i hello målresursgruppen, virtuella målnätverket, mål-undernät och hello-tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="5e788-151">When you initiate a failover, hello VMs are created in hello target resource group, target virtual network, target subnet, and hello target availability set.</span></span> <span data-ttu-id="5e788-152">Du kan använda valfri återställningspunkt under en redundansväxling.</span><span class="sxs-lookup"><span data-stu-id="5e788-152">During a failover, you can use any recovery point.</span></span>

![Failover-processen](./media/site-recovery-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a><span data-ttu-id="5e788-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5e788-154">Next steps</span></span>

- <span data-ttu-id="5e788-155">Lär dig mer om [nätverk](site-recovery-azure-to-azure-networking-guidance.md) för Azure VM-replikering.</span><span class="sxs-lookup"><span data-stu-id="5e788-155">Learn about [networking](site-recovery-azure-to-azure-networking-guidance.md) for Azure VM replication.</span></span>
- <span data-ttu-id="5e788-156">Följ en genomgång för[replikera virtuella datorer i Azure.](site-recovery-azure-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="5e788-156">Follow a walkthrough too[replicate Azure VMs.](site-recovery-azure-to-azure.md)</span></span>
