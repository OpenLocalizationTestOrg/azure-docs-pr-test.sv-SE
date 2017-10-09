---
title: aaaReplicate Hyper-V virtuella datorer i VMM-moln tooAzure med Azure Site Recovery | Microsoft Docs
description: "Ger en översikt för att replikera virtuella Hyper-V-datorer i VMM-moln tooAzure hello Azure Site Recovery-tjänsten"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: d6f729a49cc86ea07bebc4d7266fd7b58b3998f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure-using-site-recovery-in-hello-azure-portal"></a><span data-ttu-id="c7c88-103">Replikera virtuella Hyper-V-datorer i VMM-moln tooAzure med Site Recovery i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c7c88-103">Replicate Hyper-V virtual machines in VMM clouds tooAzure using Site Recovery in hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c7c88-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c7c88-104">Azure portal</span></span>](site-recovery-vmm-to-azure.md)
> * [<span data-ttu-id="c7c88-105">Klassiska Azure</span><span class="sxs-lookup"><span data-stu-id="c7c88-105">Azure classic</span></span>](site-recovery-vmm-to-azure-classic.md)
> * [<span data-ttu-id="c7c88-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c7c88-106">PowerShell Resource Manager</span></span>](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [<span data-ttu-id="c7c88-107">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="c7c88-107">PowerShell classic</span></span>](site-recovery-deploy-with-powershell.md)


<span data-ttu-id="c7c88-108">Den här artikeln innehåller en översikt över hello steg krävs tooreplicate lokala Hyper-V virtuella datorer (VM) hanteras i System Center Virtual Machine Manager (VMM) moln tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c7c88-108">This article provides an overview of hello steps required tooreplicate on-premises Hyper-V virtual machines (VMs) managed in System Center Virtual Machine Manager (VMM) clouds tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="c7c88-109">När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="c7c88-109">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="step-1-review-hello-scenario-architecture"></a><span data-ttu-id="c7c88-110">Steg 1: Granska hello scenariots arkitektur</span><span class="sxs-lookup"><span data-stu-id="c7c88-110">Step 1: Review hello scenario architecture</span></span>

<span data-ttu-id="c7c88-111">Innan du börjar distributionen, granska hello scenariots arkitektur och se till att du förstår alla hello-komponenter måste toodeploy.</span><span class="sxs-lookup"><span data-stu-id="c7c88-111">Before you start deployment, review hello scenario architecture, and make sure that you understand all hello components you need toodeploy.</span></span>

<span data-ttu-id="c7c88-112">Gå för[steg 1: granska hello-arkitektur](vmm-to-azure-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="c7c88-112">Go too[Step 1: Review hello architecture](vmm-to-azure-walkthrough-architecture.md)</span></span>

## <a name="step-2-review-prerequisites-and-limitations"></a><span data-ttu-id="c7c88-113">Steg 2: Granska förutsättningar och begränsningar</span><span class="sxs-lookup"><span data-stu-id="c7c88-113">Step 2: Review prerequisites and limitations</span></span>

<span data-ttu-id="c7c88-114">Kontrollera att du förstår hello distributionskrav och begränsningar.</span><span class="sxs-lookup"><span data-stu-id="c7c88-114">Make sure that you understand hello deployment prerequisites and limitations.</span></span>

<span data-ttu-id="c7c88-115">**Krav för Azure**: du behöver ett Microsoft Azure-konto, Azure-nätverk och lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="c7c88-115">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
<span data-ttu-id="c7c88-116">**Lokal VMM-servrar och Hyper-V-värdar**: Kontrollera att VMM-servrar och Hyper-V-värdar är kompatibla och förberedda för distributionen av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="c7c88-116">**On-premises VMM servers and Hyper-V hosts**: Make sure that VMM servers and Hyper-V hosts are compliant and prepared for Site Recovery deployment.</span></span>
<span data-ttu-id="c7c88-117">**Replikerade virtuella datorer**: Kontrollera att virtuella datorer du vill tooreplicate uppfyller kraven för Azure.</span><span class="sxs-lookup"><span data-stu-id="c7c88-117">**Replicated VMs**: Check that VMs you want tooreplicate comply with Azure requirements.</span></span>

<span data-ttu-id="c7c88-118">Gå för[steg 2: Verifiera förutsättningar och begränsningar](vmm-to-azure-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="c7c88-118">Go too[Step 2: Verify prerequisites and limitations](vmm-to-azure-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="c7c88-119">Steg 3: Planera kapacitet</span><span class="sxs-lookup"><span data-stu-id="c7c88-119">Step 3: Plan capacity</span></span>

<span data-ttu-id="c7c88-120">Om du utför en fullständig distribution måste toofigure reda på vilka replikering resurser som du behöver.</span><span class="sxs-lookup"><span data-stu-id="c7c88-120">If you're doing a full deployment, you need toofigure out what replication resources you need.</span></span> <span data-ttu-id="c7c88-121">Det finns ett par av verktyg tillgängliga toohelp du göra detta.</span><span class="sxs-lookup"><span data-stu-id="c7c88-121">There are a couple of tools available toohelp you do this.</span></span> <span data-ttu-id="c7c88-122">Om du utför en snabb konfigurera tootest hello miljö kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="c7c88-122">If you're doing a quick set up tootest hello environment, you can skip this step.</span></span>

<span data-ttu-id="c7c88-123">Gå för[steg3: Planera kapacitet](vmm-to-azure-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="c7c88-123">Go too[Step 3: Plan capacity](vmm-to-azure-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="c7c88-124">Steg 4: Planera nätverk</span><span class="sxs-lookup"><span data-stu-id="c7c88-124">Step 4: Plan networking</span></span>

<span data-ttu-id="c7c88-125">Du måste toodo vissa nätverk planera tooensure som du kan konfigurera nätverksmappning när du distribuerar hello scenariot virtuella Azure-datorer kommer att anslutna tooAzure virtuella nätverk efter växling vid fel och som att de är tilldelade lämplig IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="c7c88-125">You need toodo some network planning tooensure that you can configure network mapping when you deploy hello scenario, that Azure VMs will be connected tooAzure virtual networks after failover occurs, and that that they're assigned appropriate IP addresses.</span></span>

<span data-ttu-id="c7c88-126">Gå för[steg 4: Planera nätverk](vmm-to-azure-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="c7c88-126">Go too[Step 4: Plan networking](vmm-to-azure-walkthrough-network.md)</span></span>


## <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="c7c88-127">Steg 5: Förbered Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="c7c88-127">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="c7c88-128">Skapa ett Azure-konto, nätverk och lagring.</span><span class="sxs-lookup"><span data-stu-id="c7c88-128">Set up an Azure account, networks, and storage.</span></span> <span data-ttu-id="c7c88-129">Du kan göra detta under distributionen, men vi rekommenderar att du gör detta innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="c7c88-129">You can do this during deployment, but we recommend you do this before you start.</span></span>

<span data-ttu-id="c7c88-130">Gå för[steg 5: Förbered Azure](vmm-to-azure-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="c7c88-130">Go too[Step 5: Prepare Azure](vmm-to-azure-walkthrough-prepare-azure.md)</span></span>

## <a name="step-6-prepare-vmm-and-hyper-v"></a><span data-ttu-id="c7c88-131">Steg 6: Förbereda VMM och Hyper-V</span><span class="sxs-lookup"><span data-stu-id="c7c88-131">Step 6: Prepare VMM and Hyper-V</span></span>

<span data-ttu-id="c7c88-132">Förbered hello lokal VMM-servrar och Hyper-V-värdar för distributionen av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="c7c88-132">Prepare hello on-premises VMM servers and Hyper-V hosts for Site Recovery deployment.</span></span>

<span data-ttu-id="c7c88-133">Gå för[steg 6: förbereda lokala servrar](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span><span class="sxs-lookup"><span data-stu-id="c7c88-133">Go too[Step 6: Prepare on-premises servers](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span></span>

## <a name="step-7-set-up-a-vault"></a><span data-ttu-id="c7c88-134">Steg 7: Konfigurera ett valv</span><span class="sxs-lookup"><span data-stu-id="c7c88-134">Step 7: Set up a vault</span></span>

<span data-ttu-id="c7c88-135">Skapa ett Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="c7c88-135">Set up a Recovery Services vault.</span></span> <span data-ttu-id="c7c88-136">hello valvet innehåller konfigurationsinställningar och styr replikeringen.</span><span class="sxs-lookup"><span data-stu-id="c7c88-136">hello vault contains configuration settings, and orchestrates replication.</span></span>

[<span data-ttu-id="c7c88-137">Steg 7: Konfigurera ett valv</span><span class="sxs-lookup"><span data-stu-id="c7c88-137">Step 7: Set up a vault</span></span>](vmm-to-azure-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a><span data-ttu-id="c7c88-138">Steg 8: Konfigurera inställningar för källa och mål</span><span class="sxs-lookup"><span data-stu-id="c7c88-138">Step 8: Configure source and target settings</span></span>

<span data-ttu-id="c7c88-139">Ställ in hello käll- och målplatserna för replikering.</span><span class="sxs-lookup"><span data-stu-id="c7c88-139">Set up hello source and target replication locations.</span></span> <span data-ttu-id="c7c88-140">Lägg till hello VMM server toohello valvet och ladda ned hello installationsfilerna för Site Recovery-komponenter.</span><span class="sxs-lookup"><span data-stu-id="c7c88-140">Add hello VMM server toohello vault, and download hello installation files for Site Recovery components.</span></span> <span data-ttu-id="c7c88-141">Kör installationsprogrammet för Azure Site Recovery Provider på hello VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="c7c88-141">Run Azure Site Recovery Provider setup on hello VMM server.</span></span> <span data-ttu-id="c7c88-142">Installationsprogrammet installerar hello providern på hello VMM-servern och registrerar hello-server i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="c7c88-142">Setup installs hello Provider on hello VMM server, and registers hello server in hello vault.</span></span> <span data-ttu-id="c7c88-143">Hello Microsoft Recovery Services-agenten installeras på varje Hyper-V-värd.</span><span class="sxs-lookup"><span data-stu-id="c7c88-143">You install hello Microsoft Recovery Services agent on each Hyper-V host.</span></span>

<span data-ttu-id="c7c88-144">Gå för[steg 8: Konfigurera inställningar för källa och mål](vmm-to-azure-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="c7c88-144">Go too[Step 8: Configure source and target settings](vmm-to-azure-walkthrough-source-target.md)</span></span>

## <a name="step-9-configure-network-mapping"></a><span data-ttu-id="c7c88-145">Steg 9: Konfigurera nätverksmappning</span><span class="sxs-lookup"><span data-stu-id="c7c88-145">Step 9: Configure network mapping</span></span>

<span data-ttu-id="c7c88-146">Mappa en lokal VMM VM-nätverk tooAzure virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="c7c88-146">Map on-premises VMM VM networks tooAzure virtual networks.</span></span> <span data-ttu-id="c7c88-147">Efter växling vid fel skapas virtuella Azure-datorer i hello Azure-nätverk som mappar toohello lokala Virtuella nätverk i vilka hello datakälla Hyper-V finns.</span><span class="sxs-lookup"><span data-stu-id="c7c88-147">After failover, Azure VMs are created in hello Azure network that maps toohello on-premises VM network in which hello source Hyper-V is located.</span></span>

<span data-ttu-id="c7c88-148">Gå för[steg 9: Konfigurera nätverksmappning](vmm-to-azure-walkthrough-network-mapping.md)</span><span class="sxs-lookup"><span data-stu-id="c7c88-148">Go too[Step 9: Configure network mapping](vmm-to-azure-walkthrough-network-mapping.md)</span></span>


## <a name="step-10-set-up-a-replication-policy"></a><span data-ttu-id="c7c88-149">Steg 10: Ställ in en replikeringsprincip</span><span class="sxs-lookup"><span data-stu-id="c7c88-149">Step 10: Set up a replication policy</span></span>

<span data-ttu-id="c7c88-150">Ange hur lokala virtuella datorer kommer att replikerade tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c7c88-150">Specify how on-premises VMs will be replicated tooAzure.</span></span>

<span data-ttu-id="c7c88-151">Gå för[steg 10: Konfigurera en replikeringsprincip](vmm-to-azure-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="c7c88-151">Go too[Step 10: Set up a replication policy](vmm-to-azure-walkthrough-replication.md)</span></span>


## <a name="step-11-enable-replication-for-vms"></a><span data-ttu-id="c7c88-152">Steg 11: Aktivera replikering för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="c7c88-152">Step 11: Enable replication for VMs</span></span>

<span data-ttu-id="c7c88-153">Välj hello virtuella datorer du vill tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="c7c88-153">Select hello VMs you want tooreplicate.</span></span> <span data-ttu-id="c7c88-154">Aktivera en virtuell dator för replikering utlösare hello inledande replikering tooAzure följt av en pågående deltareplikering.</span><span class="sxs-lookup"><span data-stu-id="c7c88-154">ENabling a VM for replication triggers hello initial replication tooAzure, followed by ongoing delta replication.</span></span>

<span data-ttu-id="c7c88-155">Gå för[steg 11: Aktivera replikering](vmm-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="c7c88-155">Go too[Step 11: Enable replication](vmm-to-azure-walkthrough-enable-replication.md)</span></span>


## <a name="step-12-run-a-test-failover"></a><span data-ttu-id="c7c88-156">Steg 12: Kör ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="c7c88-156">Step 12: Run a test failover</span></span>

<span data-ttu-id="c7c88-157">Kör ett test redundans toomake att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="c7c88-157">Run a test failover toomake sure everything's working as expected.</span></span>

<span data-ttu-id="c7c88-158">Gå för[steg 12: kör ett redundanstest](vmm-to-azure-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="c7c88-158">Go too[Step 12: Run a test failover](vmm-to-azure-walkthrough-test-failover.md)</span></span>


