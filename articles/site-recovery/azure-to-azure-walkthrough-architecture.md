---
title: "Granska arkitektur för replikering av virtuella Azure-datorer mellan Azure-regioner | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över komponenter och arkitektur som används när du replikerar virtuella Azure-datorer mellan Azure-regioner med hjälp av Azure Site Recovery-tjänsten."
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
ms.openlocfilehash: f471add4f4dee26482e2820fb06d010d6c0c0e88
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="step-1-review-the-architecture-for-azure-vm-replication-between-azure-regions"></a><span data-ttu-id="ecd6d-103">Steg 1: Granska arkitektur för Azure VM replikering mellan Azure-regioner</span><span class="sxs-lookup"><span data-stu-id="ecd6d-103">Step 1: Review the architecture for Azure VM replication between Azure regions</span></span>


<span data-ttu-id="ecd6d-104">När du har granskat den [översikt över stegen](azure-to-azure-walkthrough-overview.md) för den här distributionen den här artikeln för att förstå komponenterna och processer som används när replikering och återställa virtuella Azure-datorer (VM) från en Azure-region till en annan, med hjälp av [ Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ecd6d-104">After reviewing the [overview steps](azure-to-azure-walkthrough-overview.md) for this deployment, read this article to understand the components and processes used when replicating and recovering Azure virtual machines (VMs) from one Azure region to another, using [Azure Site Recovery](site-recovery-overview.md).</span></span>

- <span data-ttu-id="ecd6d-105">När du är klar artikeln bör du ha en förståelse för hur fungerar Azure VM replikering till en annan region.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-105">When you finish the article, you should have a clear understanding of how Azure VM replication to another region works.</span></span>
- <span data-ttu-id="ecd6d-106">Skriv dina kommentarer eller frågor längst ned i den här artikeln eller i [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="ecd6d-106">Post any comments at the bottom of this article, or ask questions in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

>[!NOTE]
><span data-ttu-id="ecd6d-107">Azure VM-replikeringen med Site Recovery-tjänsten är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-107">Azure VM replication with the Site Recovery service is currently in preview.</span></span>



## <a name="architectural-components"></a><span data-ttu-id="ecd6d-108">Arkitekturkomponenter</span><span class="sxs-lookup"><span data-stu-id="ecd6d-108">Architectural components</span></span>

<span data-ttu-id="ecd6d-109">Följande diagram ger en övergripande bild av en Azure VM-miljö i en viss region (i det här exemplet östra USA platsen).</span><span class="sxs-lookup"><span data-stu-id="ecd6d-109">The following diagram provides a high-level view of an Azure VM environment in a specific region (in this example, the East US location).</span></span> <span data-ttu-id="ecd6d-110">I en miljö med virtuella Azure-datorn:</span><span class="sxs-lookup"><span data-stu-id="ecd6d-110">In an Azure VM environment:</span></span>
- <span data-ttu-id="ecd6d-111">Appar kan köras på virtuella datorer med diskar som är fördelade på storage-konton.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-111">Apps can be running on VMs with disks spread across storage accounts.</span></span>
- <span data-ttu-id="ecd6d-112">De virtuella datorerna kan ingå i en eller flera undernät i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-112">The VMs can be included in one or more subnets within a virtual network.</span></span>

![kund-miljö](./media/azure-to-azure-walkthrough-architecture/source-environment.png)

## <a name="replication-process"></a><span data-ttu-id="ecd6d-114">Replikeringsprocessen</span><span class="sxs-lookup"><span data-stu-id="ecd6d-114">Replication process</span></span>

### <a name="step-1"></a><span data-ttu-id="ecd6d-115">Steg 1</span><span class="sxs-lookup"><span data-stu-id="ecd6d-115">Step 1</span></span>

<span data-ttu-id="ecd6d-116">När du aktiverar Azure VM-replikering i Azure-portalen, skapas automatiskt resurserna som visas i följande diagram och tabellen i mål-region.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-116">When you enable Azure VM replication in the Azure portal, the resources shown in the following diagram and table are automatically created in the target region.</span></span> <span data-ttu-id="ecd6d-117">Som standard skapas resurser baserat på inställningarna för datakälla region.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-117">By default, resources are created based on source region settings.</span></span> <span data-ttu-id="ecd6d-118">Du kan anpassa Målinställningar efter behov.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-118">You can customize the target settings as required.</span></span> <span data-ttu-id="ecd6d-119">[Läs mer](site-recovery-replicate-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="ecd6d-119">[Learn more](site-recovery-replicate-azure-to-azure.md).</span></span>

![Aktivera replikeringen steg 1](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-1.png)

<span data-ttu-id="ecd6d-121">**Resurs**</span><span class="sxs-lookup"><span data-stu-id="ecd6d-121">**Resource**</span></span> | <span data-ttu-id="ecd6d-122">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="ecd6d-122">**Details**</span></span>
--- | ---
<span data-ttu-id="ecd6d-123">**Målresursgruppen**</span><span class="sxs-lookup"><span data-stu-id="ecd6d-123">**Target resource group**</span></span> | <span data-ttu-id="ecd6d-124">Resursgruppen som tillhör replikerade virtuella datorer efter redundans.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-124">The resource group to which replicated VMs belong after failover.</span></span>
<span data-ttu-id="ecd6d-125">**Mål virtuellt nätverk**</span><span class="sxs-lookup"><span data-stu-id="ecd6d-125">**Target virtual network**</span></span> | <span data-ttu-id="ecd6d-126">Det virtuella nätverket som replikerade virtuella datorer finns efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-126">The virtual network in which replicated VMs are located after failover.</span></span> <span data-ttu-id="ecd6d-127">En nätverksmappning skapas mellan käll- och virtuella nätverk och vice versa.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-127">A network mapping is created between source and target virtual networks, and vice versa.</span></span>
<span data-ttu-id="ecd6d-128">**Cache-lagringskonton**</span><span class="sxs-lookup"><span data-stu-id="ecd6d-128">**Cache storage accounts**</span></span> | <span data-ttu-id="ecd6d-129">Innan ändringar på virtuella källdatorer replikeras till mål-lagringskontot de spåras och skickas till cachen storage-konto på målplatsen.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-129">Before changes on source VMs are replicated to the target storage account, they are tracked and sent to the cache storage account in the target location.</span></span> <span data-ttu-id="ecd6d-130">Detta säkerställer minimal inverkan på produktion appar som körs på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-130">This ensures minimal impact on production apps running on the VM.</span></span>
<span data-ttu-id="ecd6d-131">**Mål-lagringskonton**</span><span class="sxs-lookup"><span data-stu-id="ecd6d-131">**Target storage accounts**</span></span>  | <span data-ttu-id="ecd6d-132">Storage-konton på målplatsen data replikeras.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-132">Storage accounts in the target location to which the data is replicated.</span></span>
<span data-ttu-id="ecd6d-133">**Mål-tillgänglighetsuppsättningar**</span><span class="sxs-lookup"><span data-stu-id="ecd6d-133">**Target availability sets**</span></span>  | <span data-ttu-id="ecd6d-134">Tillgänglighetsuppsättningar i som de replikerade virtuella datorerna är placerade efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-134">Availability sets in which the replicated VMs are located after failover.</span></span>

### <a name="step-2"></a><span data-ttu-id="ecd6d-135">Steg 2</span><span class="sxs-lookup"><span data-stu-id="ecd6d-135">Step 2</span></span>

<span data-ttu-id="ecd6d-136">Eftersom replikering är aktiverat, installeras automatiskt Site Recovery-tillägget mobilitetstjänsten på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-136">As replication is enabled, the Site Recovery extension Mobility service is automatically installed on the VM.</span></span> <span data-ttu-id="ecd6d-137">Inträffar följande:</span><span class="sxs-lookup"><span data-stu-id="ecd6d-137">The following occurs:</span></span>

1. <span data-ttu-id="ecd6d-138">Den virtuella datorn har registrerats med Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-138">The VM is registered with Site Recovery.</span></span>

2. <span data-ttu-id="ecd6d-139">Kontinuerlig replikering har konfigurerats för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-139">Continuous replication is configured for the VM.</span></span> <span data-ttu-id="ecd6d-140">Skrivning av data på Virtuella diskar överförs kontinuerligt till lagringskontot cache på källplatsen.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-140">Data writes on the VM disks are continuously transferred to the cache storage account in the source location.</span></span>

   ![Aktivera replikeringen steg 2](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-2.png)

  
  <span data-ttu-id="ecd6d-142">Observera att Site Recovery aldrig ha inkommande anslutning till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-142">Note that Site Recovery never needs inbound connectivity to the VM.</span></span> <span data-ttu-id="ecd6d-143">Endast krävs utgående anslutning till Site Recovery-tjänsten URL-adresser/IP-adresser, Office 365 autentisering URL: er/IP-adresser och IP-adresser för cache storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-143">Only outbound connectivity to Site Recovery service URLs/IP addresses, Office 365 authentication URLs/IP addresses, and cache storage account IP addresses is needed.</span></span> 

## <a name="continuous-replication-process"></a><span data-ttu-id="ecd6d-144">Kontinuerlig replikeringsprocess</span><span class="sxs-lookup"><span data-stu-id="ecd6d-144">Continuous replication process</span></span>

<span data-ttu-id="ecd6d-145">När kontinuerlig replikering fungerar överföras Diskskrivningar omedelbart till cache-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-145">After continuous replication is working, disk writes are immediately transferred to the cache storage account.</span></span> <span data-ttu-id="ecd6d-146">Site Recovery bearbetar data och skickar det till mål-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-146">Site Recovery processes the data and sends it to the target storage account.</span></span> <span data-ttu-id="ecd6d-147">När data har bearbetats skapas återställningspunkter i mål-lagringskontot med några minuters mellanrum.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-147">After the data is processed, recovery points are generated in the target storage account every few minutes.</span></span>

## <a name="failover-process"></a><span data-ttu-id="ecd6d-148">Failover-processen</span><span class="sxs-lookup"><span data-stu-id="ecd6d-148">Failover process</span></span>

<span data-ttu-id="ecd6d-149">När du påbörja en växling virtuella datorer skapas i målresursgruppen, virtuella målnätverket mål undernät och mål-tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-149">When you initiate a failover, the VMs are created in the target resource group, target virtual network, target subnet, and the target availability set.</span></span> <span data-ttu-id="ecd6d-150">Du kan använda valfri återställningspunkt under en redundansväxling.</span><span class="sxs-lookup"><span data-stu-id="ecd6d-150">During a failover, you can use any recovery point.</span></span>

![Failover-processen](./media/azure-to-azure-walkthrough-architecture/failover.png)

## <a name="next-steps"></a><span data-ttu-id="ecd6d-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ecd6d-152">Next steps</span></span>

<span data-ttu-id="ecd6d-153">Gå till [steg 2: Verifiera förutsättningar och begränsningar](azure-to-azure-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="ecd6d-153">Go to [Step 2: Verify prerequisites and limitations](azure-to-azure-walkthrough-prerequisites.md)</span></span>
