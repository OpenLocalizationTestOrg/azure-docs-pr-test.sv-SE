---
title: aaaReplicate virtuella VMware-datorer tooAzure med Azure Site Recovery | Microsoft Docs
description: "En översikt över hello steg för att replikera arbetsbelastningar som körs på virtuella VMware-datorer tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 7104f67a3635b916048dcb61bca770c89f0c77fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-vmware-vms-tooazure-with-site-recovery"></a><span data-ttu-id="e3252-103">Replikera virtuella VMware-datorer tooAzure med Site Recovery</span><span class="sxs-lookup"><span data-stu-id="e3252-103">Replicate VMware VMs tooAzure with Site Recovery</span></span>

<span data-ttu-id="e3252-104">Den här artikeln innehåller en översikt över hello steg krävs tooreplicate lokal VMware virtuella datorer tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e3252-104">This article provides an overview of hello steps required tooreplicate on-premises VMware virtual machines tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


![Processen för distribution](./media/vmware-walkthrough-overview/vmware-to-azure-process.png)

<span data-ttu-id="e3252-106">**Bild 1: Process distributionssammanfattning**</span><span class="sxs-lookup"><span data-stu-id="e3252-106">**Figure 1: Deployment process summary**</span></span>

## <a name="step-1-review-architecture-and-prerequisites"></a><span data-ttu-id="e3252-107">Steg 1: Granska arkitektur och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="e3252-107">Step 1: Review architecture and prerequisites</span></span>

<span data-ttu-id="e3252-108">Innan du börjar distributionen bör granska hello scenariots arkitektur och kontrollera att du förstår alla hello-komponenter som du behöver toodeploy</span><span class="sxs-lookup"><span data-stu-id="e3252-108">Before you start deployment, review hello scenario architecture, and make sure you understand all hello components you need toodeploy</span></span>

<span data-ttu-id="e3252-109">Gå för[steg 1: granska hello-arkitektur](vmware-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="e3252-109">Go too[Step 1: Review hello architecture](vmware-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="e3252-110">Steg 2: Granska krav</span><span class="sxs-lookup"><span data-stu-id="e3252-110">Step 2: Review prerequisites</span></span>

<span data-ttu-id="e3252-111">Kontrollera att du har hello krav för varje komponent i distributionen:</span><span class="sxs-lookup"><span data-stu-id="e3252-111">Make sure you have hello prerequisites in place for each deployment component:</span></span>

- <span data-ttu-id="e3252-112">**Krav för Azure**: du behöver ett Microsoft Azure-konto, Azure-nätverk och lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="e3252-112">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
- <span data-ttu-id="e3252-113">**Site Recovery-komponenter på lokala**: du behöver en dator som körs lokalt Site Recovery-komponenter.</span><span class="sxs-lookup"><span data-stu-id="e3252-113">**On-premises Site Recovery components**: You need a machine running on-premises Site Recovery components.</span></span>
- <span data-ttu-id="e3252-114">**Lokal VMware krav**: du måste tooset konton så att Site Recovery kan komma åt VMware-servrar och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e3252-114">**On-premises VMware prerequisites**: You need tooset up accounts so that Site Recovery can access VMware servers and VMs.</span></span>
- <span data-ttu-id="e3252-115">**Replikerade virtuella datorer**: virtuella datorer du vill tooreplicate måste toocomply med krav för Azure och har hello mobilitetstjänsten installeras.</span><span class="sxs-lookup"><span data-stu-id="e3252-115">**Replicated VMs**: VMs you want tooreplicate need toocomply with Azure requirements, and have hello Mobility service component installed.</span></span>

<span data-ttu-id="e3252-116">Gå för[steg 2: granska förutsättningar och begränsningar](vmware-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="e3252-116">Go too[Step 2: Review prerequisites and limitations](vmware-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="e3252-117">Steg 3: Planera kapacitet</span><span class="sxs-lookup"><span data-stu-id="e3252-117">Step 3: Plan capacity</span></span>

<span data-ttu-id="e3252-118">Om du gör en fullständig distribution måste toofigure reda på vilka replikering resurser som du behöver.</span><span class="sxs-lookup"><span data-stu-id="e3252-118">If you're doing a full deployment you need toofigure out what replication resources you need.</span></span> <span data-ttu-id="e3252-119">Det finns ett par av verktyg tillgängliga toohelp du göra detta.</span><span class="sxs-lookup"><span data-stu-id="e3252-119">There are a couple of tools available toohelp you do this.</span></span> <span data-ttu-id="e3252-120">Gå tooStep 2.</span><span class="sxs-lookup"><span data-stu-id="e3252-120">Go tooStep 2.</span></span> <span data-ttu-id="e3252-121">Om du gör en snabb konfigurera tootest hello miljö kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="e3252-121">If you're doing a quick set up tootest hello environment you can skip this step.</span></span>

<span data-ttu-id="e3252-122">Gå för[steg3: Planera kapacitet](vmware-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="e3252-122">Go too[Step 3: Plan capacity](vmware-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="e3252-123">Steg 4: Planera nätverk</span><span class="sxs-lookup"><span data-stu-id="e3252-123">Step 4: Plan networking</span></span>

<span data-ttu-id="e3252-124">Du måste toodo vissa nätverk planera tooensure att virtuella Azure-datorer är anslutna toonetworks efter växling vid fel och att de har hello rätt IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="e3252-124">You need toodo some network planning tooensure that Azure VMs are connected toonetworks after failover occurs, and  that that they have hello right IP addresses.</span></span>

<span data-ttu-id="e3252-125">Gå för[steg 4: Planera nätverk](vmware-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="e3252-125">Go too[Step 4: Plan networking](vmware-walkthrough-network.md)</span></span>

##  <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="e3252-126">Steg 5: Förbered Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="e3252-126">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="e3252-127">Konfigurera Azure nätverken och lagringsenheterna innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="e3252-127">Set up Azure networks and storage before you start.</span></span> <span data-ttu-id="e3252-128">Du kan göra detta under distributionen, men vi rekommenderar att du gör detta innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="e3252-128">You can do this during deployment, but we recommend you do this before you start.</span></span>

<span data-ttu-id="e3252-129">Gå för[steg 5: Förbered Azure](vmware-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="e3252-129">Go too[Step 5: Prepare Azure](vmware-walkthrough-prepare-azure.md)</span></span>


## <a name="step-6-prepare-vmware"></a><span data-ttu-id="e3252-130">Steg 6: Förbereda VMware</span><span class="sxs-lookup"><span data-stu-id="e3252-130">Step 6: Prepare VMware</span></span>

<span data-ttu-id="e3252-131">Du behöver tooset konton som Site Recovery för att:</span><span class="sxs-lookup"><span data-stu-id="e3252-131">You need tooset up accounts that Site Recovery will use to:</span></span>

- <span data-ttu-id="e3252-132">Åtkomst VMware virtualisering servrar tooautomatically identifiera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e3252-132">Access VMware virtualization servers tooautomatically detect VMs.</span></span>
- <span data-ttu-id="e3252-133">Åtkomst till virtuella datorer tooinstall hello mobilitetstjänsten.</span><span class="sxs-lookup"><span data-stu-id="e3252-133">Access VMs tooinstall hello Mobility service.</span></span> <span data-ttu-id="e3252-134">Varje virtuell dator som du vill tooreplicate måste ha hello mobilitetstjänstagenten installerad innan du kan aktivera replikering för den.</span><span class="sxs-lookup"><span data-stu-id="e3252-134">Each VM you want tooreplicate must have hello Mobility service agent installed before you can enable replication for it.</span></span>

<span data-ttu-id="e3252-135">Gå för[steg 6: förbereda VMware](vmware-walkthrough-prepare-vmware.md)</span><span class="sxs-lookup"><span data-stu-id="e3252-135">Go too[Step 6: Prepare VMware](vmware-walkthrough-prepare-vmware.md)</span></span>

## <a name="step-7-set-up-a-vault"></a><span data-ttu-id="e3252-136">Steg 7: Konfigurera ett valv</span><span class="sxs-lookup"><span data-stu-id="e3252-136">Step 7: Set up a vault</span></span>

<span data-ttu-id="e3252-137">Du behöver tooset in tooorchestrate en Recovery Services-valvet och hantera replikering.</span><span class="sxs-lookup"><span data-stu-id="e3252-137">You need tooset up a Recovery Services vault tooorchestrate and manage replication.</span></span> <span data-ttu-id="e3252-138">När du ställer in hello valvet kan du ange vad du vill tooreplicate, och där du vill att tooreplicate den.</span><span class="sxs-lookup"><span data-stu-id="e3252-138">When you set up hello vault, you specify what you want tooreplicate, and where you want tooreplicate it to.</span></span>

<span data-ttu-id="e3252-139">Gå för[steg 7: konfigurera ett valv](vmware-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="e3252-139">Go too[Step 7: Set up a vault](vmware-walkthrough-create-vault.md)</span></span>

## <a name="step-8-configure-source-and-target-settings"></a><span data-ttu-id="e3252-140">Steg 8: Konfigurera inställningar för källa och mål</span><span class="sxs-lookup"><span data-stu-id="e3252-140">Step 8: Configure source and target settings</span></span>

<span data-ttu-id="e3252-141">Ställ in hello källa och mål som används för replikering.</span><span class="sxs-lookup"><span data-stu-id="e3252-141">Set up hello source and target that's used for replication.</span></span> <span data-ttu-id="e3252-142">Konfigurera inställningar för datakälla innehåller och kör installationsprogrammet för enhetlig tooinstall hello lokalt Site Recovery-komponenter.</span><span class="sxs-lookup"><span data-stu-id="e3252-142">Setting up source settings includes running Unified Setup tooinstall hello on-premises Site Recovery components.</span></span>

<span data-ttu-id="e3252-143">Gå för[steg 8: Ställ in hello källa och mål](vmware-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="e3252-143">Go too[Step 8: Set up hello source and target](vmware-walkthrough-source-target.md)</span></span>

## <a name="step-9-set-up-a-replication-policy"></a><span data-ttu-id="e3252-144">Steg 9: Konfigurera en replikeringsprincip</span><span class="sxs-lookup"><span data-stu-id="e3252-144">Step 9: Set up a replication policy</span></span>

<span data-ttu-id="e3252-145">Du ställa in en princip toospecify replikeringsinställningarna för virtuella VMware-datorer i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="e3252-145">You set up a policy toospecify replication settings for VMware VMs in hello vault.</span></span>

<span data-ttu-id="e3252-146">Gå för[steg 9: Konfigurera en replikeringsprincip](vmware-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="e3252-146">Go too[Step 9: Set up a replication policy](vmware-walkthrough-replication.md)</span></span>

## <a name="step-10-install-hello-mobility-service"></a><span data-ttu-id="e3252-147">Steg 10: Installera hello mobilitetstjänsten</span><span class="sxs-lookup"><span data-stu-id="e3252-147">Step 10: Install hello Mobility service</span></span>

<span data-ttu-id="e3252-148">Hej mobilitetstjänsten måste installeras på varje virtuell dator som du vill tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="e3252-148">hello Mobility service must be installed on each VM you want tooreplicate.</span></span> <span data-ttu-id="e3252-149">Det finns ett par sätt tooset hello tjänsten med push- eller pull-installation.</span><span class="sxs-lookup"><span data-stu-id="e3252-149">There are a few ways tooset up hello service with push or pull installation.</span></span>

<span data-ttu-id="e3252-150">Gå för[steg 10: Installera hello mobilitetstjänsten](vmware-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="e3252-150">Go too[Step 10: Install hello Mobility service](vmware-walkthrough-install-mobility.md)</span></span>

## <a name="step-11-enable-replication"></a><span data-ttu-id="e3252-151">Steg 11: Aktivera replikering</span><span class="sxs-lookup"><span data-stu-id="e3252-151">Step 11: Enable replication</span></span>

<span data-ttu-id="e3252-152">När hello mobilitetstjänsten körs på en virtuell dator kan du aktivera replikering för den.</span><span class="sxs-lookup"><span data-stu-id="e3252-152">After hello Mobility service is running on a VM you can enable replication for it.</span></span> <span data-ttu-id="e3252-153">När du har aktiverat inträffar inledande replikering av hello VM.</span><span class="sxs-lookup"><span data-stu-id="e3252-153">After enabling, initial replication of hello VM occurs.</span></span>

<span data-ttu-id="e3252-154">Gå för[steg 11: Aktivera replikering](vmware-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="e3252-154">Go too[Step 11: Enable replication](vmware-walkthrough-enable-replication.md)</span></span>

## <a name="step-12-run-a-test-failover"></a><span data-ttu-id="e3252-155">Steg 12: Kör ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="e3252-155">Step 12: Run a test failover</span></span>

<span data-ttu-id="e3252-156">När replikeringen har slutförts och deltareplikering körs, kan du köra en testa redundans toomake att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="e3252-156">After initial replication finishes, and delta replication is running, you can run a test failover toomake sure everything works as expected.</span></span>

<span data-ttu-id="e3252-157">Gå för[steg 12: kör ett redundanstest](vmware-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="e3252-157">Go too[Step 12: Run a test failover](vmware-walkthrough-test-failover.md)</span></span>
