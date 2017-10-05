---
title: Hur fungerar Azure replikeringen mellan Azure-regioner i Azure Site Recovery?  | Microsoft Docs
description: "Den här artikeln innehåller en översikt över komponenter och arkitektur som används när du replikerar virtuella Azure-datorer mellan Azure-regioner med hjälp av Azure Site Recovery-tjänsten."
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
ms.openlocfilehash: ec397eaeda963f257d1bd996f1f57189bcde17ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-does-azure-vm-replication-work-in-site-recovery"></a><span data-ttu-id="9b28a-104">Hur fungerar Azure VM-replikering i Site Recovery?</span><span class="sxs-lookup"><span data-stu-id="9b28a-104">How does Azure VM replication work in Site Recovery?</span></span>


<span data-ttu-id="9b28a-105">Den här artikeln beskrivs de komponenter och processer som är involverad i replikera och återställa virtuella Azure-datorer (VM) från en region till ett annat med hjälp av den [Azure Site Recovery](site-recovery-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="9b28a-105">This article describes the components and processes involved in replicating and recovering Azure virtual machines (VMs) from one region to another by using the [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

>[!NOTE]
><span data-ttu-id="9b28a-106">Azure VM-replikeringen med Site Recovery-tjänsten är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="9b28a-106">Azure VM replication with the Site Recovery service is currently in preview.</span></span>

<span data-ttu-id="9b28a-107">Skriv dina kommentarer eller frågor längst ned i den här artikeln eller i [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="9b28a-107">Post any comments at the bottom of this article, or ask questions in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="architectural-components"></a><span data-ttu-id="9b28a-108">Arkitekturkomponenter</span><span class="sxs-lookup"><span data-stu-id="9b28a-108">Architectural components</span></span>

<span data-ttu-id="9b28a-109">Följande diagram ger en övergripande bild av en Azure VM-miljö i en viss region (i det här exemplet östra USA platsen).</span><span class="sxs-lookup"><span data-stu-id="9b28a-109">The following diagram provides a high-level view of an Azure VM environment in a specific region (in this example, the East US location).</span></span> <span data-ttu-id="9b28a-110">I en miljö med virtuella Azure-datorn:</span><span class="sxs-lookup"><span data-stu-id="9b28a-110">In an Azure VM environment:</span></span>
- <span data-ttu-id="9b28a-111">Appar kan köras på virtuella datorer med diskar som är fördelade på storage-konton.</span><span class="sxs-lookup"><span data-stu-id="9b28a-111">Apps can be running on VMs with disks spread across storage accounts.</span></span>
- <span data-ttu-id="9b28a-112">De virtuella datorerna kan ingå i en eller flera undernät i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="9b28a-112">The VMs can be included in one or more subnets within a virtual network.</span></span>

![kund-miljö](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

<span data-ttu-id="9b28a-114">Lär dig mer om kraven för distribution och kraven i de [supportmatrisen](site-recovery-support-matrix-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="9b28a-114">Learn about the deployment prerequisites and requirements in the [support matrix](site-recovery-support-matrix-azure-to-azure.md).</span></span>

## <a name="replication-process"></a><span data-ttu-id="9b28a-115">Replikeringsprocessen</span><span class="sxs-lookup"><span data-stu-id="9b28a-115">Replication process</span></span>

### <a name="step-1"></a><span data-ttu-id="9b28a-116">Steg 1</span><span class="sxs-lookup"><span data-stu-id="9b28a-116">Step 1</span></span>

<span data-ttu-id="9b28a-117">När du aktiverar Azure VM-replikering i Azure-portalen, skapas automatiskt resurserna som visas i följande diagram och tabellen i mål-region.</span><span class="sxs-lookup"><span data-stu-id="9b28a-117">When you enable Azure VM replication in the Azure portal, the resources shown in the following diagram and table are automatically created in the target region.</span></span> <span data-ttu-id="9b28a-118">Som standard skapas resurser baserat på inställningarna för datakälla region.</span><span class="sxs-lookup"><span data-stu-id="9b28a-118">By default, resources are created based on source region settings.</span></span> <span data-ttu-id="9b28a-119">Du kan anpassa Målinställningar efter behov.</span><span class="sxs-lookup"><span data-stu-id="9b28a-119">You can customize the target settings as required.</span></span> <span data-ttu-id="9b28a-120">[Läs mer](site-recovery-replicate-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="9b28a-120">[Learn more](site-recovery-replicate-azure-to-azure.md).</span></span>

![Aktivera replikeringen steg 1](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-1.png)

<span data-ttu-id="9b28a-122">**Resurs**</span><span class="sxs-lookup"><span data-stu-id="9b28a-122">**Resource**</span></span> | <span data-ttu-id="9b28a-123">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="9b28a-123">**Details**</span></span>
--- | ---
<span data-ttu-id="9b28a-124">**Målresursgruppen**</span><span class="sxs-lookup"><span data-stu-id="9b28a-124">**Target resource group**</span></span> | <span data-ttu-id="9b28a-125">Resursgruppen som tillhör replikerade virtuella datorer efter redundans.</span><span class="sxs-lookup"><span data-stu-id="9b28a-125">The resource group to which replicated VMs belong after failover.</span></span>
<span data-ttu-id="9b28a-126">**Mål virtuellt nätverk**</span><span class="sxs-lookup"><span data-stu-id="9b28a-126">**Target virtual network**</span></span> | <span data-ttu-id="9b28a-127">Det virtuella nätverket som replikerade virtuella datorer finns efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="9b28a-127">The virtual network in which replicated VMs are located after failover.</span></span> <span data-ttu-id="9b28a-128">En nätverksmappning skapas mellan käll- och virtuella nätverk och vice versa.</span><span class="sxs-lookup"><span data-stu-id="9b28a-128">A network mapping is created between source and target virtual networks, and vice versa.</span></span>
<span data-ttu-id="9b28a-129">**Cache-lagringskonton**</span><span class="sxs-lookup"><span data-stu-id="9b28a-129">**Cache storage accounts**</span></span> | <span data-ttu-id="9b28a-130">Innan ändringar på virtuella källdatorer replikeras till mål-lagringskontot de spåras och skickas till cachen storage-konto på målplatsen.</span><span class="sxs-lookup"><span data-stu-id="9b28a-130">Before changes on source VMs are replicated to the target storage account, they are tracked and sent to the cache storage account in the target location.</span></span> <span data-ttu-id="9b28a-131">Detta säkerställer minimal inverkan på produktion appar som körs på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9b28a-131">This ensures minimal impact on production apps running on the VM.</span></span>
<span data-ttu-id="9b28a-132">**Mål-lagringskonton**</span><span class="sxs-lookup"><span data-stu-id="9b28a-132">**Target storage accounts**</span></span>  | <span data-ttu-id="9b28a-133">Storage-konton på målplatsen data replikeras.</span><span class="sxs-lookup"><span data-stu-id="9b28a-133">Storage accounts in the target location to which the data is replicated.</span></span>
<span data-ttu-id="9b28a-134">**Mål-tillgänglighetsuppsättningar**</span><span class="sxs-lookup"><span data-stu-id="9b28a-134">**Target availability sets**</span></span>  | <span data-ttu-id="9b28a-135">Tillgänglighetsuppsättningar i som de replikerade virtuella datorerna är placerade efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="9b28a-135">Availability sets in which the replicated VMs are located after failover.</span></span>

### <a name="step-2"></a><span data-ttu-id="9b28a-136">Steg 2</span><span class="sxs-lookup"><span data-stu-id="9b28a-136">Step 2</span></span>

<span data-ttu-id="9b28a-137">Eftersom replikering är aktiverat, installeras automatiskt Site Recovery-tillägget mobilitetstjänsten på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9b28a-137">As replication is enabled, the Site Recovery extension Mobility service is automatically installed on the VM.</span></span> <span data-ttu-id="9b28a-138">Inträffar följande:</span><span class="sxs-lookup"><span data-stu-id="9b28a-138">The following occurs:</span></span>

1. <span data-ttu-id="9b28a-139">Den virtuella datorn har registrerats med Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="9b28a-139">The VM is registered with Site Recovery.</span></span>

2. <span data-ttu-id="9b28a-140">Kontinuerlig replikering har konfigurerats för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9b28a-140">Continuous replication is configured for the VM.</span></span> <span data-ttu-id="9b28a-141">Skrivning av data på Virtuella diskar överförs kontinuerligt till lagringskontot cache på källplatsen.</span><span class="sxs-lookup"><span data-stu-id="9b28a-141">Data writes on the VM disks are continuously transferred to the cache storage account in the source location.</span></span>

   ![Aktivera replikeringen steg 2](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-2.png)

   >[!IMPORTANT]
   > <span data-ttu-id="9b28a-143">Site Recovery behöver aldrig inkommande anslutning till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9b28a-143">Site Recovery never needs inbound connectivity to the VM.</span></span> <span data-ttu-id="9b28a-144">Den virtuella datorn måste bara utgående anslutning till Site Recovery-tjänsten URL-adresser/IP-adresser, Office 365 autentisering URL: er/IP-adresser och IP-adresser för cache storage-konto.</span><span class="sxs-lookup"><span data-stu-id="9b28a-144">The VM needs only outbound connectivity to Site Recovery service URLs/IP addresses, Office 365 authentication URLs/IP addresses, and cache storage account IP addresses.</span></span> <span data-ttu-id="9b28a-145">Mer information finns i [nätverk vägledning för att replikera virtuella datorer i Azure](site-recovery-azure-to-azure-networking-guidance.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="9b28a-145">For more information, see the [Networking guidance for replicating Azure virtual machines](site-recovery-azure-to-azure-networking-guidance.md) article.</span></span>

## <a name="continuous-replication-process"></a><span data-ttu-id="9b28a-146">Kontinuerlig replikeringsprocess</span><span class="sxs-lookup"><span data-stu-id="9b28a-146">Continuous replication process</span></span>

<span data-ttu-id="9b28a-147">När kontinuerlig replikering fungerar överföras Diskskrivningar omedelbart till cache-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="9b28a-147">After continuous replication is working, disk writes are immediately transferred to the cache storage account.</span></span> <span data-ttu-id="9b28a-148">Site Recovery bearbetar data och skickar det till mål-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="9b28a-148">Site Recovery processes the data and sends it to the target storage account.</span></span> <span data-ttu-id="9b28a-149">När data har bearbetats skapas återställningspunkter i mål-lagringskontot med några minuters mellanrum.</span><span class="sxs-lookup"><span data-stu-id="9b28a-149">After the data is processed, recovery points are generated in the target storage account every few minutes.</span></span>

## <a name="failover-process"></a><span data-ttu-id="9b28a-150">Failover-processen</span><span class="sxs-lookup"><span data-stu-id="9b28a-150">Failover process</span></span>

<span data-ttu-id="9b28a-151">När du påbörja en växling virtuella datorer skapas i målresursgruppen, virtuella målnätverket mål undernät och mål-tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="9b28a-151">When you initiate a failover, the VMs are created in the target resource group, target virtual network, target subnet, and the target availability set.</span></span> <span data-ttu-id="9b28a-152">Du kan använda valfri återställningspunkt under en redundansväxling.</span><span class="sxs-lookup"><span data-stu-id="9b28a-152">During a failover, you can use any recovery point.</span></span>

![Failover-processen](./media/site-recovery-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a><span data-ttu-id="9b28a-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9b28a-154">Next steps</span></span>

- <span data-ttu-id="9b28a-155">Lär dig mer om [nätverk](site-recovery-azure-to-azure-networking-guidance.md) för Azure VM-replikering.</span><span class="sxs-lookup"><span data-stu-id="9b28a-155">Learn about [networking](site-recovery-azure-to-azure-networking-guidance.md) for Azure VM replication.</span></span>
- <span data-ttu-id="9b28a-156">Följ en genomgång till [replikera virtuella datorer i Azure.](site-recovery-azure-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="9b28a-156">Follow a walkthrough to [replicate Azure VMs.](site-recovery-azure-to-azure.md)</span></span>
