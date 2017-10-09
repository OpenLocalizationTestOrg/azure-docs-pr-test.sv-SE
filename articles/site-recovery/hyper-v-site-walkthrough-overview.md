---
title: aaaReplicate Hyper-V VMs tooAzure med Azure Site Recovery | Microsoft Docs
description: "Beskriver hur tooorchestrate replikering, redundans och återställning av lokala Hyper-V VMs tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: efddd986-bc13-4a1d-932d-5484cdc7ad8d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: ab9cd14149ef32a416428d0f4327aa18b042e9c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-without-vmm-tooazure"></a><span data-ttu-id="0f536-103">Replikera tooAzure för Hyper-V-virtuella datorer (utan VMM)</span><span class="sxs-lookup"><span data-stu-id="0f536-103">Replicate Hyper-V virtual machines (without VMM) tooAzure</span></span> 

> [!div class="op_single_selector"]
> * [<span data-ttu-id="0f536-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0f536-104">Azure portal</span></span>](site-recovery-hyper-v-site-to-azure.md)
> * [<span data-ttu-id="0f536-105">Klassiska Azure</span><span class="sxs-lookup"><span data-stu-id="0f536-105">Azure classic</span></span>](site-recovery-hyper-v-site-to-azure-classic.md)
> * [<span data-ttu-id="0f536-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0f536-106">PowerShell - Resource Manager</span></span>](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

<span data-ttu-id="0f536-107">Den här artikeln innehåller en översikt över hello steg som krävs för tooreplicate lokala Hyper-V virtuella datorer tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0f536-107">This article provides an overview of hello steps required tooreplicate on-premises Hyper-V virtual machines tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) in hello Azure portal.</span></span> <span data-ttu-id="0f536-108">I den här distributionen Hyper-V virtuella datorer hanteras inte av System Center Virtual Machine Manager (VMM).</span><span class="sxs-lookup"><span data-stu-id="0f536-108">In this deployment Hyper-V VMs aren't managed by System Center Virtual Machine Manager (VMM).</span></span>


<span data-ttu-id="0f536-109">När du har läst den här artikeln efter eventuella kommentarer längst ned hello eller tekniska frågor om hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="0f536-109">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="step-1-review-architecture-and-prerequisites"></a><span data-ttu-id="0f536-110">Steg 1: Granska arkitektur och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="0f536-110">Step 1: Review architecture and prerequisites</span></span>

<span data-ttu-id="0f536-111">Innan du börjar distributionen bör granska hello scenariots arkitektur och kontrollera att du förstår alla hello-komponenter som du behöver toodeploy</span><span class="sxs-lookup"><span data-stu-id="0f536-111">Before you start deployment, review hello scenario architecture, and make sure you understand all hello components you need toodeploy</span></span>

<span data-ttu-id="0f536-112">Gå för[steg 1: granska hello-arkitektur](hyper-v-site-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="0f536-112">Go too[Step 1: Review hello architecture](hyper-v-site-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="0f536-113">Steg 2: Granska krav</span><span class="sxs-lookup"><span data-stu-id="0f536-113">Step 2: Review prerequisites</span></span>

<span data-ttu-id="0f536-114">Kontrollera att du har hello krav för varje komponent i distributionen:</span><span class="sxs-lookup"><span data-stu-id="0f536-114">Make sure you have hello prerequisites in place for each deployment component:</span></span>

- <span data-ttu-id="0f536-115">**Krav för Azure**: du behöver ett Microsoft Azure-konto, Azure-nätverk och lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="0f536-115">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
- <span data-ttu-id="0f536-116">**Lokal Hyper-V-krav**: Kontrollera att Hyper-V-värdar är förberedda för distributionen av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="0f536-116">**On-premises Hyper-V prerequisites**: Make sure Hyper-V hosts are prepared for Site Recovery deployment.</span></span>
- <span data-ttu-id="0f536-117">**Replikerade virtuella datorer**: virtuella datorer du vill tooreplicate måste toocomply med krav för Azure.</span><span class="sxs-lookup"><span data-stu-id="0f536-117">**Replicated VMs**: VMs you want tooreplicate need toocomply with Azure requirements.</span></span>

<span data-ttu-id="0f536-118">Gå för[steg 2: Verifiera förutsättningar och begränsningar](hyper-v-site-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="0f536-118">Go too[Step 2: Verify prerequisites and limitations](hyper-v-site-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="0f536-119">Steg 3: Planera kapacitet</span><span class="sxs-lookup"><span data-stu-id="0f536-119">Step 3: Plan capacity</span></span>

<span data-ttu-id="0f536-120">Om du gör en fullständig distribution måste toofigure reda på vilka replikering resurser som du behöver.</span><span class="sxs-lookup"><span data-stu-id="0f536-120">If you're doing a full deployment you need toofigure out what replication resources you need.</span></span> <span data-ttu-id="0f536-121">Det finns ett par av verktyg tillgängliga toohelp du göra detta.</span><span class="sxs-lookup"><span data-stu-id="0f536-121">There are a couple of tools available toohelp you do this.</span></span> <span data-ttu-id="0f536-122">Gå tooStep 2.</span><span class="sxs-lookup"><span data-stu-id="0f536-122">Go tooStep 2.</span></span> <span data-ttu-id="0f536-123">Om du gör en snabb konfigurera tootest hello miljö kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="0f536-123">If you're doing a quick set up tootest hello environment you can skip this step.</span></span>

<span data-ttu-id="0f536-124">Gå för[steg3: Planera kapacitet](hyper-v-site-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="0f536-124">Go too[Step 3: Plan capacity](hyper-v-site-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="0f536-125">Steg 4: Planera nätverk</span><span class="sxs-lookup"><span data-stu-id="0f536-125">Step 4: Plan networking</span></span>

<span data-ttu-id="0f536-126">Du måste toodo vissa nätverk planera tooensure att virtuella Azure-datorer är anslutna toonetworks efter växling vid fel och att de har hello rätt IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="0f536-126">You need toodo some network planning tooensure that Azure VMs are connected toonetworks after failover occurs, and  that that they have hello right IP addresses.</span></span>

<span data-ttu-id="0f536-127">Gå för[steg 4: Planera nätverk](hyper-v-site-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="0f536-127">Go too[Step 4: Plan networking](hyper-v-site-walkthrough-network.md)</span></span>

##  <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="0f536-128">Steg 5: Förbered Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="0f536-128">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="0f536-129">Konfigurera Azure nätverken och lagringsenheterna innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="0f536-129">Set up Azure networks and storage before you start.</span></span> <span data-ttu-id="0f536-130">Du kan göra detta under distributionen, men vi rekommenderar att du gör detta innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="0f536-130">You can do this during deployment, but we recommend you do this before you start.</span></span>

<span data-ttu-id="0f536-131">Gå för[steg 5: Förbered Azure](hyper-v-site-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="0f536-131">Go too[Step 5: Prepare Azure](hyper-v-site-walkthrough-prepare-azure.md)</span></span>


## <a name="step-6-prepare-hyper-v"></a><span data-ttu-id="0f536-132">Steg 6: Förbereda Hyper-V</span><span class="sxs-lookup"><span data-stu-id="0f536-132">Step 6: Prepare Hyper-V</span></span>

<span data-ttu-id="0f536-133">Kontrollera att Hyper-V-servrar uppfyller kraven för Site Recovery-distribution.</span><span class="sxs-lookup"><span data-stu-id="0f536-133">Make sure that Hyper-V servers meet Site Recovery deployment requirements.</span></span>

<span data-ttu-id="0f536-134">Gå för[steg 6: förbereda Hyper-V](hyper-v-site-walkthrough-prepare-hyper-v.md)</span><span class="sxs-lookup"><span data-stu-id="0f536-134">Go too[Step 6: Prepare Hyper-V](hyper-v-site-walkthrough-prepare-hyper-v.md)</span></span>

## <a name="step-7-set-up-a-vault"></a><span data-ttu-id="0f536-135">Steg 7: Konfigurera ett valv</span><span class="sxs-lookup"><span data-stu-id="0f536-135">Step 7: Set up a vault</span></span>

<span data-ttu-id="0f536-136">Du behöver tooset in tooorchestrate en Recovery Services-valvet och hantera replikering.</span><span class="sxs-lookup"><span data-stu-id="0f536-136">You need tooset up a Recovery Services vault tooorchestrate and manage replication.</span></span> <span data-ttu-id="0f536-137">När du ställer in hello valvet kan du ange vad du vill tooreplicate, och där du vill att tooreplicate den.</span><span class="sxs-lookup"><span data-stu-id="0f536-137">When you set up hello vault, you specify what you want tooreplicate, and where you want tooreplicate it to.</span></span>

<span data-ttu-id="0f536-138">Gå för[steg 7: skapa ett valv](hyper-v-site-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="0f536-138">Go too[Step 7: Create a vault](hyper-v-site-walkthrough-create-vault.md)</span></span>

## <a name="step-8-configure-source-and-target-settings"></a><span data-ttu-id="0f536-139">Steg 8: Konfigurera inställningar för källa och mål</span><span class="sxs-lookup"><span data-stu-id="0f536-139">Step 8: Configure source and target settings</span></span>

<span data-ttu-id="0f536-140">Ställ in hello källa och mål som används för replikering.</span><span class="sxs-lookup"><span data-stu-id="0f536-140">Set up hello source and target that's used for replication.</span></span> <span data-ttu-id="0f536-141">Konfigurera inställningar för datakälla innehåller lägger till Hyper-V-värdar tooa Hyper-V plats installerar hello Site Recovery-Provider och Recovery Services-agenten på varje Hyper-V-värd och registrera hello plats i hello Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="0f536-141">Setting up source settings includes adding Hyper-V hosts tooa Hyper-V site, installing hello Site Recovery Provider and Recovery Services agent on each Hyper-V host, and registering hello site in hello Recovery Services vault.</span></span>

<span data-ttu-id="0f536-142">Gå för[steg 8: Ställ in hello källa och mål](hyper-v-site-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="0f536-142">Go too[Step 8: Set up hello source and target](hyper-v-site-walkthrough-source-target.md)</span></span>

## <a name="step-9-set-up-a-replication-policy"></a><span data-ttu-id="0f536-143">Steg 9: Konfigurera en replikeringsprincip</span><span class="sxs-lookup"><span data-stu-id="0f536-143">Step 9: Set up a replication policy</span></span>

<span data-ttu-id="0f536-144">Du ställa in en princip toospecify replikeringsinställningar för Hyper-V virtuella datorer i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="0f536-144">You set up a policy toospecify replication settings for Hyper-V VMs in hello vault.</span></span>

<span data-ttu-id="0f536-145">Gå för[steg 9: Konfigurera en replikeringsprincip](hyper-v-site-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="0f536-145">Go too[Step 9: Set up a replication policy](hyper-v-site-walkthrough-replication.md)</span></span>


## <a name="step-10-enable-replication"></a><span data-ttu-id="0f536-146">Steg 10: Aktivera replikering</span><span class="sxs-lookup"><span data-stu-id="0f536-146">Step 10: Enable replication</span></span>

<span data-ttu-id="0f536-147">När du har en replikeringsprincip för på plats när du har aktiverat utförs inledande replikering av hello VM.</span><span class="sxs-lookup"><span data-stu-id="0f536-147">After you have a replication policy in place,  After enabling, initial replication of hello VM occurs.</span></span>

<span data-ttu-id="0f536-148">Gå för[steg 10: Aktivera replikering](hyper-v-site-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="0f536-148">Go too[Step 10: Enable replication](hyper-v-site-walkthrough-enable-replication.md)</span></span>

## <a name="step-11-run-a-test-failover"></a><span data-ttu-id="0f536-149">Steg 11: Kör ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="0f536-149">Step 11: Run a test failover</span></span>

<span data-ttu-id="0f536-150">När replikeringen har slutförts och deltareplikering körs, kan du köra en testa redundans toomake att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="0f536-150">After initial replication finishes, and delta replication is running, you can run a test failover toomake sure everything works as expected.</span></span>

<span data-ttu-id="0f536-151">Gå för[steg 11: kör ett redundanstest](hyper-v-site-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="0f536-151">Go too[Step 11: Run a test failover](hyper-v-site-walkthrough-test-failover.md)</span></span>
