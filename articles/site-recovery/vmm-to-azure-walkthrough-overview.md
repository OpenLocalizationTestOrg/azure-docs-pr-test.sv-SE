---
title: Replikera virtuella Hyper-V-datorer i VMM-moln till Azure med Azure Site Recovery | Microsoft Docs
description: "Ger en översikt för att replikera virtuella Hyper-V-datorer i VMM-moln till Azure med Azure Site Recovery-tjänsten"
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
ms.openlocfilehash: af68d21184c137b2b0f1bb4c1afb9bf507e8332d
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-site-recovery-in-the-azure-portal"></a><span data-ttu-id="f30bd-103">Replikera virtuella Hyper-V-datorer i VMM-moln till Azure med hjälp av Site Recovery i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f30bd-103">Replicate Hyper-V virtual machines in VMM clouds to Azure using Site Recovery in the Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f30bd-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f30bd-104">Azure portal</span></span>](site-recovery-vmm-to-azure.md)
> * [<span data-ttu-id="f30bd-105">Klassiska Azure</span><span class="sxs-lookup"><span data-stu-id="f30bd-105">Azure classic</span></span>](site-recovery-vmm-to-azure-classic.md)
> * [<span data-ttu-id="f30bd-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f30bd-106">PowerShell Resource Manager</span></span>](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [<span data-ttu-id="f30bd-107">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="f30bd-107">PowerShell classic</span></span>](site-recovery-deploy-with-powershell.md)


<span data-ttu-id="f30bd-108">Den här artikeln innehåller en översikt över de steg som krävs för att replikera lokala Hyper-V virtuella datorer (VM) som hanteras i System Center Virtual Machine Manager (VMM)-moln till Azure, med hjälp av den [Azure Site Recovery](site-recovery-overview.md) tjänsten på den Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f30bd-108">This article provides an overview of the steps required to replicate on-premises Hyper-V virtual machines (VMs) managed in System Center Virtual Machine Manager (VMM) clouds to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="f30bd-109">När du har läst den här artikeln kan du lämna kommentarer längst ned på sidan eller på [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="f30bd-109">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="step-1-review-the-scenario-architecture"></a><span data-ttu-id="f30bd-110">Steg 1: Granska scenariots arkitektur</span><span class="sxs-lookup"><span data-stu-id="f30bd-110">Step 1: Review the scenario architecture</span></span>

<span data-ttu-id="f30bd-111">Innan du börjar distributionen bör du granska scenariots arkitektur och se till att du förstår de komponenter som du behöver distribuera.</span><span class="sxs-lookup"><span data-stu-id="f30bd-111">Before you start deployment, review the scenario architecture, and make sure that you understand all the components you need to deploy.</span></span>

<span data-ttu-id="f30bd-112">Gå till [steg 1: granska arkitekturen](vmm-to-azure-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="f30bd-112">Go to [Step 1: Review the architecture](vmm-to-azure-walkthrough-architecture.md)</span></span>

## <a name="step-2-review-prerequisites-and-limitations"></a><span data-ttu-id="f30bd-113">Steg 2: Granska förutsättningar och begränsningar</span><span class="sxs-lookup"><span data-stu-id="f30bd-113">Step 2: Review prerequisites and limitations</span></span>

<span data-ttu-id="f30bd-114">Kontrollera att du förstår distributionskrav och begränsningar.</span><span class="sxs-lookup"><span data-stu-id="f30bd-114">Make sure that you understand the deployment prerequisites and limitations.</span></span>

<span data-ttu-id="f30bd-115">**Krav för Azure**: du behöver ett Microsoft Azure-konto, Azure-nätverk och lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="f30bd-115">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
<span data-ttu-id="f30bd-116">**Lokal VMM-servrar och Hyper-V-värdar**: Kontrollera att VMM-servrar och Hyper-V-värdar är kompatibla och förberedda för distributionen av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="f30bd-116">**On-premises VMM servers and Hyper-V hosts**: Make sure that VMM servers and Hyper-V hosts are compliant and prepared for Site Recovery deployment.</span></span>
<span data-ttu-id="f30bd-117">**Replikerade virtuella datorer**: Kontrollera att virtuella datorer som du vill replikera uppfyller kraven för Azure.</span><span class="sxs-lookup"><span data-stu-id="f30bd-117">**Replicated VMs**: Check that VMs you want to replicate comply with Azure requirements.</span></span>

<span data-ttu-id="f30bd-118">Gå till [steg 2: Verifiera förutsättningar och begränsningar](vmm-to-azure-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="f30bd-118">Go to [Step 2: Verify prerequisites and limitations](vmm-to-azure-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="f30bd-119">Steg 3: Planera kapacitet</span><span class="sxs-lookup"><span data-stu-id="f30bd-119">Step 3: Plan capacity</span></span>

<span data-ttu-id="f30bd-120">Om du gör en fullständig distribution, måste du ta reda på vilka replikering resurser som du behöver.</span><span class="sxs-lookup"><span data-stu-id="f30bd-120">If you're doing a full deployment, you need to figure out what replication resources you need.</span></span> <span data-ttu-id="f30bd-121">Det finns några tillgängliga för att hjälpa dig med detta verktyg.</span><span class="sxs-lookup"><span data-stu-id="f30bd-121">There are a couple of tools available to help you do this.</span></span> <span data-ttu-id="f30bd-122">Om du utför en snabb som ställts in för att testa miljön kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="f30bd-122">If you're doing a quick set up to test the environment, you can skip this step.</span></span>

<span data-ttu-id="f30bd-123">Gå till [Steg3: Planera kapacitet](vmm-to-azure-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="f30bd-123">Go to [Step 3: Plan capacity](vmm-to-azure-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="f30bd-124">Steg 4: Planera nätverk</span><span class="sxs-lookup"><span data-stu-id="f30bd-124">Step 4: Plan networking</span></span>

<span data-ttu-id="f30bd-125">Du behöver göra några nätverk som planerar att se till att du kan konfigurera nätverksmappning när du distribuerar Scenarionamn som virtuella Azure-datorer ansluts till virtuella Azure-nätverk efter växling vid fel och som att de är tilldelade lämplig IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="f30bd-125">You need to do some network planning to ensure that you can configure network mapping when you deploy the scenario, that Azure VMs will be connected to Azure virtual networks after failover occurs, and that that they're assigned appropriate IP addresses.</span></span>

<span data-ttu-id="f30bd-126">Gå till [steg 4: Planera nätverk](vmm-to-azure-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="f30bd-126">Go to [Step 4: Plan networking](vmm-to-azure-walkthrough-network.md)</span></span>


## <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="f30bd-127">Steg 5: Förbered Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="f30bd-127">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="f30bd-128">Skapa ett Azure-konto, nätverk och lagring.</span><span class="sxs-lookup"><span data-stu-id="f30bd-128">Set up an Azure account, networks, and storage.</span></span> <span data-ttu-id="f30bd-129">Du kan göra detta under distributionen, men vi rekommenderar att du gör detta innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="f30bd-129">You can do this during deployment, but we recommend you do this before you start.</span></span>

<span data-ttu-id="f30bd-130">Gå till [steg 5: Förbered Azure](vmm-to-azure-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="f30bd-130">Go to [Step 5: Prepare Azure](vmm-to-azure-walkthrough-prepare-azure.md)</span></span>

## <a name="step-6-prepare-vmm-and-hyper-v"></a><span data-ttu-id="f30bd-131">Steg 6: Förbereda VMM och Hyper-V</span><span class="sxs-lookup"><span data-stu-id="f30bd-131">Step 6: Prepare VMM and Hyper-V</span></span>

<span data-ttu-id="f30bd-132">Förbered den lokala VMM-servrar och Hyper-V-värdar för distributionen av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="f30bd-132">Prepare the on-premises VMM servers and Hyper-V hosts for Site Recovery deployment.</span></span>

<span data-ttu-id="f30bd-133">Gå till [steg 6: förbereda lokala servrar](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span><span class="sxs-lookup"><span data-stu-id="f30bd-133">Go to [Step 6: Prepare on-premises servers](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span></span>

## <a name="step-7-set-up-a-vault"></a><span data-ttu-id="f30bd-134">Steg 7: Konfigurera ett valv</span><span class="sxs-lookup"><span data-stu-id="f30bd-134">Step 7: Set up a vault</span></span>

<span data-ttu-id="f30bd-135">Skapa ett Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="f30bd-135">Set up a Recovery Services vault.</span></span> <span data-ttu-id="f30bd-136">Valvet innehåller konfigurationsinställningar och styr replikeringen.</span><span class="sxs-lookup"><span data-stu-id="f30bd-136">The vault contains configuration settings, and orchestrates replication.</span></span>

[<span data-ttu-id="f30bd-137">Steg 7: Konfigurera ett valv</span><span class="sxs-lookup"><span data-stu-id="f30bd-137">Step 7: Set up a vault</span></span>](vmm-to-azure-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a><span data-ttu-id="f30bd-138">Steg 8: Konfigurera inställningar för källa och mål</span><span class="sxs-lookup"><span data-stu-id="f30bd-138">Step 8: Configure source and target settings</span></span>

<span data-ttu-id="f30bd-139">Konfigurera käll- och målplatserna för replikering.</span><span class="sxs-lookup"><span data-stu-id="f30bd-139">Set up the source and target replication locations.</span></span> <span data-ttu-id="f30bd-140">Lägg till VMM-servern i valvet och hämta installationsfilerna för Site Recovery-komponenter.</span><span class="sxs-lookup"><span data-stu-id="f30bd-140">Add the VMM server to the vault, and download the installation files for Site Recovery components.</span></span> <span data-ttu-id="f30bd-141">Kör installationsprogrammet för Azure Site Recovery-providern på VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="f30bd-141">Run Azure Site Recovery Provider setup on the VMM server.</span></span> <span data-ttu-id="f30bd-142">Installationsprogrammet installerar providern på VMM-servern och registrerar servern i valvet.</span><span class="sxs-lookup"><span data-stu-id="f30bd-142">Setup installs the Provider on the VMM server, and registers the server in the vault.</span></span> <span data-ttu-id="f30bd-143">Du installerar Microsoft Recovery Services-agenten på varje Hyper-V-värd.</span><span class="sxs-lookup"><span data-stu-id="f30bd-143">You install the Microsoft Recovery Services agent on each Hyper-V host.</span></span>

<span data-ttu-id="f30bd-144">Gå till [steg 8: Konfigurera inställningar för källa och mål](vmm-to-azure-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="f30bd-144">Go to [Step 8: Configure source and target settings](vmm-to-azure-walkthrough-source-target.md)</span></span>

## <a name="step-9-configure-network-mapping"></a><span data-ttu-id="f30bd-145">Steg 9: Konfigurera nätverksmappning</span><span class="sxs-lookup"><span data-stu-id="f30bd-145">Step 9: Configure network mapping</span></span>

<span data-ttu-id="f30bd-146">Mappa en lokal VMM VM-nätverk för virtuella Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="f30bd-146">Map on-premises VMM VM networks to Azure virtual networks.</span></span> <span data-ttu-id="f30bd-147">Efter växling vid fel skapas virtuella Azure-datorer i Azure-nätverk som mappar till det lokala nätverket som datakälla Hyper-V finns.</span><span class="sxs-lookup"><span data-stu-id="f30bd-147">After failover, Azure VMs are created in the Azure network that maps to the on-premises VM network in which the source Hyper-V is located.</span></span>

<span data-ttu-id="f30bd-148">Gå till [steg 9: Konfigurera nätverksmappning](vmm-to-azure-walkthrough-network-mapping.md)</span><span class="sxs-lookup"><span data-stu-id="f30bd-148">Go to [Step 9: Configure network mapping](vmm-to-azure-walkthrough-network-mapping.md)</span></span>


## <a name="step-10-set-up-a-replication-policy"></a><span data-ttu-id="f30bd-149">Steg 10: Ställ in en replikeringsprincip</span><span class="sxs-lookup"><span data-stu-id="f30bd-149">Step 10: Set up a replication policy</span></span>

<span data-ttu-id="f30bd-150">Ange hur lokala virtuella datorer kommer att replikeras till Azure.</span><span class="sxs-lookup"><span data-stu-id="f30bd-150">Specify how on-premises VMs will be replicated to Azure.</span></span>

<span data-ttu-id="f30bd-151">Gå till [steg 10: Konfigurera en replikeringsprincip](vmm-to-azure-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="f30bd-151">Go to [Step 10: Set up a replication policy](vmm-to-azure-walkthrough-replication.md)</span></span>


## <a name="step-11-enable-replication-for-vms"></a><span data-ttu-id="f30bd-152">Steg 11: Aktivera replikering för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="f30bd-152">Step 11: Enable replication for VMs</span></span>

<span data-ttu-id="f30bd-153">Välj de virtuella datorerna som du vill replikera.</span><span class="sxs-lookup"><span data-stu-id="f30bd-153">Select the VMs you want to replicate.</span></span> <span data-ttu-id="f30bd-154">Aktivera en virtuell dator för replikering utlöser den inledande replikeringen till Azure, följt av en pågående deltareplikering.</span><span class="sxs-lookup"><span data-stu-id="f30bd-154">ENabling a VM for replication triggers the initial replication to Azure, followed by ongoing delta replication.</span></span>

<span data-ttu-id="f30bd-155">Gå till [steg 11: Aktivera replikering](vmm-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="f30bd-155">Go to [Step 11: Enable replication](vmm-to-azure-walkthrough-enable-replication.md)</span></span>


## <a name="step-12-run-a-test-failover"></a><span data-ttu-id="f30bd-156">Steg 12: Kör ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="f30bd-156">Step 12: Run a test failover</span></span>

<span data-ttu-id="f30bd-157">Kör ett redundanstest för att kontrollera att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="f30bd-157">Run a test failover to make sure everything's working as expected.</span></span>

<span data-ttu-id="f30bd-158">Gå till [steg 12: kör ett redundanstest](vmm-to-azure-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="f30bd-158">Go to [Step 12: Run a test failover](vmm-to-azure-walkthrough-test-failover.md)</span></span>


