---
title: "aaaReplicate Hyper-V VMs tooa sekundär VMM-plats med Azure Site Recovery | Microsoft Docs"
description: "Översikt för att replikera virtuella Hyper-V-datorer tooa sekundär VMM-plats med hjälp av hello Azure-portalen."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 476ca82a-8f5c-4498-9dcf-e1011d60ed59
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: raynew
ms.openlocfilehash: 90e44bfc2237dfa7646fb2b7b1e533a7f6d83c50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site"></a><span data-ttu-id="e0478-103">Replikera virtuella Hyper-V-datorer i VMM-moln tooa sekundär VMM-plats</span><span class="sxs-lookup"><span data-stu-id="e0478-103">Replicate Hyper-V virtual machines in VMM clouds tooa secondary VMM site</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e0478-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e0478-104">Azure portal</span></span>](site-recovery-vmm-to-vmm.md)
> * [<span data-ttu-id="e0478-105">Klassisk portal</span><span class="sxs-lookup"><span data-stu-id="e0478-105">Classic portal</span></span>](site-recovery-vmm-to-vmm-classic.md)
> * [<span data-ttu-id="e0478-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e0478-106">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

<span data-ttu-id="e0478-107">Den här artikeln innehåller en översikt över hello steg som krävs tooreplicate lokala Hyper-V virtuella datorer (VM) som hanteras i System Center Virtual Machine Manager (VMM) moln, tooa sekundära VMM-platsen med hjälp av [Azure Site Recovery](site-recovery-overview.md)i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0478-107">This article provides an overview of hello steps required tooreplicate on-premises Hyper-V virtual machines (VMs) managed in System Center Virtual Machine Manager (VMM) clouds, tooa secondary VMM location, using [Azure Site Recovery](site-recovery-overview.md) in hello Azure portal.</span></span>

<span data-ttu-id="e0478-108">När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="e0478-108">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="step-1-review-hello-scenario-architecture"></a><span data-ttu-id="e0478-109">Steg 1: Granska hello scenariots arkitektur</span><span class="sxs-lookup"><span data-stu-id="e0478-109">Step 1: Review hello scenario architecture</span></span>

<span data-ttu-id="e0478-110">Innan du börjar distributionen, granska hello scenariots arkitektur och se till att du förstår alla hello-komponenter måste toodeploy.</span><span class="sxs-lookup"><span data-stu-id="e0478-110">Before you start deployment, review hello scenario architecture, and make sure that you understand all hello components you need toodeploy.</span></span>

<span data-ttu-id="e0478-111">Gå för[steg 1: granska hello arkitektur](vmm-to-vmm-walkthrough-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="e0478-111">Go too[Step 1: Review hello architecture](vmm-to-vmm-walkthrough-architecture.md).</span></span>

## <a name="step-2-review-prerequisites-and-limitations"></a><span data-ttu-id="e0478-112">Steg 2: Granska förutsättningar och begränsningar</span><span class="sxs-lookup"><span data-stu-id="e0478-112">Step 2: Review prerequisites and limitations</span></span>

<span data-ttu-id="e0478-113">Kontrollera att du förstår hello distributionskrav och begränsningar.</span><span class="sxs-lookup"><span data-stu-id="e0478-113">Make sure that you understand hello deployment prerequisites and limitations.</span></span>

<span data-ttu-id="e0478-114">**Krav för Azure**: du behöver ett Microsoft Azure-prenumeration och Azure Recovery Services valvet, tooorchestrate och hantera replikering.</span><span class="sxs-lookup"><span data-stu-id="e0478-114">**Azure prerequisites**: You need a Microsoft Azure subscription, and Azure Recovery Services vault, tooorchestrate and manage replication.</span></span>
<span data-ttu-id="e0478-115">**Lokal VMM-servrar och Hyper-V-värdar**: Kontrollera att VMM-servrar och Hyper-V-värdar är kompatibla och förberedda för distributionen av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="e0478-115">**On-premises VMM servers and Hyper-V hosts**: Make sure that VMM servers and Hyper-V hosts are compliant and prepared for Site Recovery deployment.</span></span>

<span data-ttu-id="e0478-116">Gå för[steg 2: Verifiera förutsättningar och begränsningar](vmm-to-vmm-walkthrough-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="e0478-116">Go too[Step 2: Verify prerequisites and limitations](vmm-to-vmm-walkthrough-prerequisites.md).</span></span>

## <a name="step-3-plan-networking"></a><span data-ttu-id="e0478-117">Steg 3: Planera nätverk</span><span class="sxs-lookup"><span data-stu-id="e0478-117">Step 3: Plan networking</span></span>

<span data-ttu-id="e0478-118">Du måste toodo vissa nätverk planera tooensure att du kan konfigurera nätverksmappning mellan VMM VM-nätverk när du distribuerar hello scenario.</span><span class="sxs-lookup"><span data-stu-id="e0478-118">You need toodo some network planning tooensure that you can configure network mapping between VMM VM networks when you deploy hello scenario.</span></span>

<span data-ttu-id="e0478-119">Gå för[steg3: Planera nätverk](vmm-to-vmm-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="e0478-119">Go too[Step 3: Plan networking](vmm-to-vmm-walkthrough-network.md).</span></span>


## <a name="step-4-prepare-vmm-and-hyper-v"></a><span data-ttu-id="e0478-120">Steg 4: Förbered VMM och Hyper-V</span><span class="sxs-lookup"><span data-stu-id="e0478-120">Step 4: Prepare VMM and Hyper-V</span></span>

<span data-ttu-id="e0478-121">Förbered hello VMM-servrar och Hyper-V-värdar för distributionen av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="e0478-121">Prepare hello VMM servers and Hyper-V hosts for Site Recovery deployment.</span></span>

<span data-ttu-id="e0478-122">Gå för[steg 4: förbereda lokala servrar](vmm-to-vmm-walkthrough-vmm-hyper-v.md).</span><span class="sxs-lookup"><span data-stu-id="e0478-122">Go too[Step 4: Prepare on-premises servers](vmm-to-vmm-walkthrough-vmm-hyper-v.md).</span></span>

## <a name="step-5-set-up-a-vault"></a><span data-ttu-id="e0478-123">Steg 5: Konfigurera ett valv</span><span class="sxs-lookup"><span data-stu-id="e0478-123">Step 5: Set up a vault</span></span>

<span data-ttu-id="e0478-124">Skapa ett Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="e0478-124">Set up a Recovery Services vault.</span></span> <span data-ttu-id="e0478-125">hello valvet innehåller konfigurationsinställningar och styr replikeringen.</span><span class="sxs-lookup"><span data-stu-id="e0478-125">hello vault contains configuration settings, and orchestrates replication.</span></span>

<span data-ttu-id="e0478-126">[Steg 5: Konfigurera ett valv](vmm-to-vmm-walkthrough-create-vault.md).</span><span class="sxs-lookup"><span data-stu-id="e0478-126">[Step 5: Set up a vault](vmm-to-vmm-walkthrough-create-vault.md).</span></span>

## <a name="step-6-set-up-source-and-target-settings"></a><span data-ttu-id="e0478-127">Steg 6: Konfigurera inställningar för källa och mål</span><span class="sxs-lookup"><span data-stu-id="e0478-127">Step 6: Set up source and target settings</span></span>

<span data-ttu-id="e0478-128">Ställ in hello käll- och replikering VMM-platser.</span><span class="sxs-lookup"><span data-stu-id="e0478-128">Set up hello source and target replication VMM locations.</span></span> <span data-ttu-id="e0478-129">Lägg till hello VMM-servrar toohello valvet och ladda ned hello installationsfilerna för Site Recovery-komponenter.</span><span class="sxs-lookup"><span data-stu-id="e0478-129">Add hello VMM servers toohello vault, and download hello installation files for Site Recovery components.</span></span> <span data-ttu-id="e0478-130">Kör installationsprogrammet för Azure Site Recovery Provider på hello VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="e0478-130">Run Azure Site Recovery Provider setup on hello VMM server.</span></span> <span data-ttu-id="e0478-131">Installationsprogrammet installerar hello providern på hello VMM-servern och registrerar hello-server i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="e0478-131">Setup installs hello Provider on hello VMM server, and registers hello server in hello vault.</span></span> <span data-ttu-id="e0478-132">Hello Microsoft Recovery Services-agenten installeras på varje Hyper-V-värd.</span><span class="sxs-lookup"><span data-stu-id="e0478-132">You install hello Microsoft Recovery Services agent on each Hyper-V host.</span></span>

<span data-ttu-id="e0478-133">Gå för[steg 6: skapa hello källa och mål för](vmm-to-vmm-walkthrough-source-target.md).</span><span class="sxs-lookup"><span data-stu-id="e0478-133">Go too[Step 6: Set up hello source and target settings](vmm-to-vmm-walkthrough-source-target.md).</span></span>

## <a name="step-7-configure-network-mapping"></a><span data-ttu-id="e0478-134">Steg 7: Konfigurera nätverksmappning</span><span class="sxs-lookup"><span data-stu-id="e0478-134">Step 7: Configure network mapping</span></span>

<span data-ttu-id="e0478-135">Mappa VMM VM-nätverk i hello käll- och målplatserna.</span><span class="sxs-lookup"><span data-stu-id="e0478-135">Map VMM VM networks in hello source and target locations.</span></span> <span data-ttu-id="e0478-136">Efter växling vid fel skapas virtuella datorer i hello målnätverket att maps toohello källnätverket vilka hello Hyper-V Virtuella källdatorn finns.</span><span class="sxs-lookup"><span data-stu-id="e0478-136">After failover, VMs are created in hello target network that maps toohello source VM network in which hello source Hyper-V VM is located.</span></span>

<span data-ttu-id="e0478-137">Gå för[steg 7: Konfigurera nätverksmappning](vmm-to-vmm-walkthrough-network-mapping.md).</span><span class="sxs-lookup"><span data-stu-id="e0478-137">Go too[Step 7: Configure network mapping](vmm-to-vmm-walkthrough-network-mapping.md).</span></span>


## <a name="step-8-set-up-a-replication-policy"></a><span data-ttu-id="e0478-138">Steg 8: Skapa en replikeringsprincip</span><span class="sxs-lookup"><span data-stu-id="e0478-138">Step 8: Set up a replication policy</span></span>

<span data-ttu-id="e0478-139">Ange hur virtuella datorer kommer att replikeras mellan VMM-platser.</span><span class="sxs-lookup"><span data-stu-id="e0478-139">Specify how  VMs will be replicated between VMM locations.</span></span>

<span data-ttu-id="e0478-140">Gå för[steg 8: skapa en replikeringsprincip](vmm-to-vmm-walkthrough-replication.md).</span><span class="sxs-lookup"><span data-stu-id="e0478-140">Go too[Step 8: Set up a replication policy](vmm-to-vmm-walkthrough-replication.md).</span></span>


## <a name="step-9-enable-replication-for-vms"></a><span data-ttu-id="e0478-141">Steg 9: Aktivera replikering för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="e0478-141">Step 9: Enable replication for VMs</span></span>

<span data-ttu-id="e0478-142">Välj hello virtuella datorer du vill tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="e0478-142">Select hello VMs you want tooreplicate.</span></span> <span data-ttu-id="e0478-143">Aktivera en virtuell dator för replikering utlösare hello inledande replikering toohello sekundär plats, följt av en pågående deltareplikering.</span><span class="sxs-lookup"><span data-stu-id="e0478-143">Enabling a VM for replication triggers hello initial replication toohello secondary site, followed by ongoing delta replication.</span></span>

<span data-ttu-id="e0478-144">Gå för[steg 9: Aktivera replikering](vmm-to-vmm-walkthrough-enable-replication.md).</span><span class="sxs-lookup"><span data-stu-id="e0478-144">Go too[Step 9: Enable replication](vmm-to-vmm-walkthrough-enable-replication.md).</span></span>


## <a name="step-10-run-a-test-failover"></a><span data-ttu-id="e0478-145">Steg 10: Köra ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="e0478-145">Step 10: Run a test failover</span></span>

<span data-ttu-id="e0478-146">Kör ett test redundans toomake att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="e0478-146">Run a test failover toomake sure everything's working as expected.</span></span>

<span data-ttu-id="e0478-147">Gå för[steg 10: köra ett redundanstest](vmm-to-vmm-walkthrough-test-failover.md).</span><span class="sxs-lookup"><span data-stu-id="e0478-147">Go too[Step 10: Run a test failover](vmm-to-vmm-walkthrough-test-failover.md).</span></span>
