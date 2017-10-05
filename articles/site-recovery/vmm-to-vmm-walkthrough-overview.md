---
title: "Replikera virtuella Hyper-V-datorer till en sekundär plats för VMM med Azure Site Recovery | Microsoft Docs"
description: "Översikt för att replikera virtuella Hyper-V-datorer till en sekundär VMM-plats med hjälp av Azure portal."
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
ms.openlocfilehash: b422dd2cf23426de2f154a553b38509082536309
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a><span data-ttu-id="9466a-103">Replikera virtuella Hyper-V-datorer i VMM-moln till en sekundär plats för VMM</span><span class="sxs-lookup"><span data-stu-id="9466a-103">Replicate Hyper-V virtual machines in VMM clouds to a secondary VMM site</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9466a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9466a-104">Azure portal</span></span>](site-recovery-vmm-to-vmm.md)
> * [<span data-ttu-id="9466a-105">Klassisk portal</span><span class="sxs-lookup"><span data-stu-id="9466a-105">Classic portal</span></span>](site-recovery-vmm-to-vmm-classic.md)
> * [<span data-ttu-id="9466a-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9466a-106">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

<span data-ttu-id="9466a-107">Den här artikeln innehåller en översikt över de steg som krävs för att replikera lokala Hyper-V virtuella datorer (VM) som hanteras i System Center Virtual Machine Manager (VMM) moln till en sekundär plats i VMM med [Azure Site Recovery](site-recovery-overview.md) i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9466a-107">This article provides an overview of the steps required to replicate on-premises Hyper-V virtual machines (VMs) managed in System Center Virtual Machine Manager (VMM) clouds, to a secondary VMM location, using [Azure Site Recovery](site-recovery-overview.md) in the Azure portal.</span></span>

<span data-ttu-id="9466a-108">När du har läst den här artikeln kan du lämna kommentarer längst ned på sidan eller på [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="9466a-108">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="step-1-review-the-scenario-architecture"></a><span data-ttu-id="9466a-109">Steg 1: Granska scenariots arkitektur</span><span class="sxs-lookup"><span data-stu-id="9466a-109">Step 1: Review the scenario architecture</span></span>

<span data-ttu-id="9466a-110">Innan du börjar distributionen bör du granska scenariots arkitektur och se till att du förstår de komponenter som du behöver distribuera.</span><span class="sxs-lookup"><span data-stu-id="9466a-110">Before you start deployment, review the scenario architecture, and make sure that you understand all the components you need to deploy.</span></span>

<span data-ttu-id="9466a-111">Gå till [steg 1: granska arkitekturen](vmm-to-vmm-walkthrough-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="9466a-111">Go to [Step 1: Review the architecture](vmm-to-vmm-walkthrough-architecture.md).</span></span>

## <a name="step-2-review-prerequisites-and-limitations"></a><span data-ttu-id="9466a-112">Steg 2: Granska förutsättningar och begränsningar</span><span class="sxs-lookup"><span data-stu-id="9466a-112">Step 2: Review prerequisites and limitations</span></span>

<span data-ttu-id="9466a-113">Kontrollera att du förstår distributionskrav och begränsningar.</span><span class="sxs-lookup"><span data-stu-id="9466a-113">Make sure that you understand the deployment prerequisites and limitations.</span></span>

<span data-ttu-id="9466a-114">**Krav för Azure**: du behöver ett Microsoft Azure-prenumeration och Azure Recovery Services-valvet, dirigera och hantera replikering.</span><span class="sxs-lookup"><span data-stu-id="9466a-114">**Azure prerequisites**: You need a Microsoft Azure subscription, and Azure Recovery Services vault, to orchestrate and manage replication.</span></span>
<span data-ttu-id="9466a-115">**Lokal VMM-servrar och Hyper-V-värdar**: Kontrollera att VMM-servrar och Hyper-V-värdar är kompatibla och förberedda för distributionen av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="9466a-115">**On-premises VMM servers and Hyper-V hosts**: Make sure that VMM servers and Hyper-V hosts are compliant and prepared for Site Recovery deployment.</span></span>

<span data-ttu-id="9466a-116">Gå till [steg 2: Verifiera förutsättningar och begränsningar](vmm-to-vmm-walkthrough-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="9466a-116">Go to [Step 2: Verify prerequisites and limitations](vmm-to-vmm-walkthrough-prerequisites.md).</span></span>

## <a name="step-3-plan-networking"></a><span data-ttu-id="9466a-117">Steg 3: Planera nätverk</span><span class="sxs-lookup"><span data-stu-id="9466a-117">Step 3: Plan networking</span></span>

<span data-ttu-id="9466a-118">Du behöver göra vissa nätverk som planerar att se till att du kan konfigurera nätverksmappning mellan VMM VM-nätverk när du distribuerar scenariot.</span><span class="sxs-lookup"><span data-stu-id="9466a-118">You need to do some network planning to ensure that you can configure network mapping between VMM VM networks when you deploy the scenario.</span></span>

<span data-ttu-id="9466a-119">Gå till [steg3: Planera nätverk](vmm-to-vmm-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="9466a-119">Go to [Step 3: Plan networking](vmm-to-vmm-walkthrough-network.md).</span></span>


## <a name="step-4-prepare-vmm-and-hyper-v"></a><span data-ttu-id="9466a-120">Steg 4: Förbered VMM och Hyper-V</span><span class="sxs-lookup"><span data-stu-id="9466a-120">Step 4: Prepare VMM and Hyper-V</span></span>

<span data-ttu-id="9466a-121">Förbereda VMM-servrar och Hyper-V-värdar för distributionen av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="9466a-121">Prepare the VMM servers and Hyper-V hosts for Site Recovery deployment.</span></span>

<span data-ttu-id="9466a-122">Gå till [steg 4: förbereda lokala servrar](vmm-to-vmm-walkthrough-vmm-hyper-v.md).</span><span class="sxs-lookup"><span data-stu-id="9466a-122">Go to [Step 4: Prepare on-premises servers](vmm-to-vmm-walkthrough-vmm-hyper-v.md).</span></span>

## <a name="step-5-set-up-a-vault"></a><span data-ttu-id="9466a-123">Steg 5: Konfigurera ett valv</span><span class="sxs-lookup"><span data-stu-id="9466a-123">Step 5: Set up a vault</span></span>

<span data-ttu-id="9466a-124">Skapa ett Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="9466a-124">Set up a Recovery Services vault.</span></span> <span data-ttu-id="9466a-125">Valvet innehåller konfigurationsinställningar och styr replikeringen.</span><span class="sxs-lookup"><span data-stu-id="9466a-125">The vault contains configuration settings, and orchestrates replication.</span></span>

<span data-ttu-id="9466a-126">[Steg 5: Konfigurera ett valv](vmm-to-vmm-walkthrough-create-vault.md).</span><span class="sxs-lookup"><span data-stu-id="9466a-126">[Step 5: Set up a vault](vmm-to-vmm-walkthrough-create-vault.md).</span></span>

## <a name="step-6-set-up-source-and-target-settings"></a><span data-ttu-id="9466a-127">Steg 6: Konfigurera inställningar för källa och mål</span><span class="sxs-lookup"><span data-stu-id="9466a-127">Step 6: Set up source and target settings</span></span>

<span data-ttu-id="9466a-128">Ställ in på käll- och replikering VMM platser.</span><span class="sxs-lookup"><span data-stu-id="9466a-128">Set up the source and target replication VMM locations.</span></span> <span data-ttu-id="9466a-129">Lägg till VMM-servrarna i valvet och hämta installationsfilerna för Site Recovery-komponenter.</span><span class="sxs-lookup"><span data-stu-id="9466a-129">Add the VMM servers to the vault, and download the installation files for Site Recovery components.</span></span> <span data-ttu-id="9466a-130">Kör installationsprogrammet för Azure Site Recovery-providern på VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="9466a-130">Run Azure Site Recovery Provider setup on the VMM server.</span></span> <span data-ttu-id="9466a-131">Installationsprogrammet installerar providern på VMM-servern och registrerar servern i valvet.</span><span class="sxs-lookup"><span data-stu-id="9466a-131">Setup installs the Provider on the VMM server, and registers the server in the vault.</span></span> <span data-ttu-id="9466a-132">Du installerar Microsoft Recovery Services-agenten på varje Hyper-V-värd.</span><span class="sxs-lookup"><span data-stu-id="9466a-132">You install the Microsoft Recovery Services agent on each Hyper-V host.</span></span>

<span data-ttu-id="9466a-133">Gå till [steg 6: Konfigurera inställningar för käll- och](vmm-to-vmm-walkthrough-source-target.md).</span><span class="sxs-lookup"><span data-stu-id="9466a-133">Go to [Step 6: Set up the source and target settings](vmm-to-vmm-walkthrough-source-target.md).</span></span>

## <a name="step-7-configure-network-mapping"></a><span data-ttu-id="9466a-134">Steg 7: Konfigurera nätverksmappning</span><span class="sxs-lookup"><span data-stu-id="9466a-134">Step 7: Configure network mapping</span></span>

<span data-ttu-id="9466a-135">Mappa VMM VM-nätverk i käll- och målplatserna.</span><span class="sxs-lookup"><span data-stu-id="9466a-135">Map VMM VM networks in the source and target locations.</span></span> <span data-ttu-id="9466a-136">Efter växling vid fel skapas virtuella datorer i målnätverket som mappar till VM-källnätverket där källan Hyper-V-dator finns.</span><span class="sxs-lookup"><span data-stu-id="9466a-136">After failover, VMs are created in the target network that maps to the source VM network in which the source Hyper-V VM is located.</span></span>

<span data-ttu-id="9466a-137">Gå till [steg 7: Konfigurera nätverksmappning](vmm-to-vmm-walkthrough-network-mapping.md).</span><span class="sxs-lookup"><span data-stu-id="9466a-137">Go to [Step 7: Configure network mapping](vmm-to-vmm-walkthrough-network-mapping.md).</span></span>


## <a name="step-8-set-up-a-replication-policy"></a><span data-ttu-id="9466a-138">Steg 8: Skapa en replikeringsprincip</span><span class="sxs-lookup"><span data-stu-id="9466a-138">Step 8: Set up a replication policy</span></span>

<span data-ttu-id="9466a-139">Ange hur virtuella datorer kommer att replikeras mellan VMM-platser.</span><span class="sxs-lookup"><span data-stu-id="9466a-139">Specify how  VMs will be replicated between VMM locations.</span></span>

<span data-ttu-id="9466a-140">Gå till [steg 8: skapa en replikeringsprincip](vmm-to-vmm-walkthrough-replication.md).</span><span class="sxs-lookup"><span data-stu-id="9466a-140">Go to [Step 8: Set up a replication policy](vmm-to-vmm-walkthrough-replication.md).</span></span>


## <a name="step-9-enable-replication-for-vms"></a><span data-ttu-id="9466a-141">Steg 9: Aktivera replikering för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="9466a-141">Step 9: Enable replication for VMs</span></span>

<span data-ttu-id="9466a-142">Välj de virtuella datorerna som du vill replikera.</span><span class="sxs-lookup"><span data-stu-id="9466a-142">Select the VMs you want to replicate.</span></span> <span data-ttu-id="9466a-143">Aktivera en virtuell dator för replikering utlöser den inledande replikeringen på den sekundära platsen, följt av en pågående deltareplikering.</span><span class="sxs-lookup"><span data-stu-id="9466a-143">Enabling a VM for replication triggers the initial replication to the secondary site, followed by ongoing delta replication.</span></span>

<span data-ttu-id="9466a-144">Gå till [steg 9: Aktivera replikering](vmm-to-vmm-walkthrough-enable-replication.md).</span><span class="sxs-lookup"><span data-stu-id="9466a-144">Go to [Step 9: Enable replication](vmm-to-vmm-walkthrough-enable-replication.md).</span></span>


## <a name="step-10-run-a-test-failover"></a><span data-ttu-id="9466a-145">Steg 10: Köra ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="9466a-145">Step 10: Run a test failover</span></span>

<span data-ttu-id="9466a-146">Kör ett redundanstest för att kontrollera att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="9466a-146">Run a test failover to make sure everything's working as expected.</span></span>

<span data-ttu-id="9466a-147">Gå till [steg 10: köra ett redundanstest](vmm-to-vmm-walkthrough-test-failover.md).</span><span class="sxs-lookup"><span data-stu-id="9466a-147">Go to [Step 10: Run a test failover](vmm-to-vmm-walkthrough-test-failover.md).</span></span>
