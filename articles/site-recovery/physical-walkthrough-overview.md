---
title: aaaReplicate fysiska lokala servrar tooAzure med Azure Site Recovery | Microsoft Docs
description: "Ger en översikt över hello steg för att replikera arbetsbelastningar som körs på lokala Windows-/ Linux fysiska servrar tooAzure med hello Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 20122f01-929a-4675-b85b-a9b99d2618bc
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: f801b9544072d4029ec06cc1abfd4ff370e852e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-physical-servers-tooazure-with-site-recovery"></a><span data-ttu-id="6d882-103">Replikera fysiska servrar tooAzure med Site Recovery</span><span class="sxs-lookup"><span data-stu-id="6d882-103">Replicate physical servers tooAzure with Site Recovery</span></span>

<span data-ttu-id="6d882-104">Den här artikeln innehåller en översikt över hello steg krävs tooreplicate lokala Windows-/ Linux fysiska servrar tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6d882-104">This article provides an overview of hello steps required tooreplicate on-premises Windows/Linux physical servers tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


## <a name="step-1-review-architecture-and-prerequisites"></a><span data-ttu-id="6d882-105">Steg 1: Granska arkitektur och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="6d882-105">Step 1: Review architecture and prerequisites</span></span>

<span data-ttu-id="6d882-106">Innan du börjar distributionen bör granska hello scenariots arkitektur och kontrollera att du förstår alla hello-komponenter som du behöver tooset upp hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="6d882-106">Before you start deployment, review hello scenario architecture, and make sure you understand all hello components you need tooset up hello deployment.</span></span>

<span data-ttu-id="6d882-107">Gå för[steg 1: granska hello-arkitektur](physical-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="6d882-107">Go too[Step 1: Review hello architecture](physical-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="6d882-108">Steg 2: Granska krav</span><span class="sxs-lookup"><span data-stu-id="6d882-108">Step 2: Review prerequisites</span></span>

<span data-ttu-id="6d882-109">Kontrollera att du har hello krav för varje komponent i distributionen:</span><span class="sxs-lookup"><span data-stu-id="6d882-109">Make sure you have hello prerequisites in place for each deployment component:</span></span>

- <span data-ttu-id="6d882-110">**Krav för Azure**: du behöver ett Microsoft Azure-konto, Azure-nätverk och lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="6d882-110">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
- <span data-ttu-id="6d882-111">**Site Recovery-komponenter på lokala**: du behöver en dator som körs lokalt Site Recovery-komponenter.</span><span class="sxs-lookup"><span data-stu-id="6d882-111">**On-premises Site Recovery components**: You need a machine running on-premises Site Recovery components.</span></span>
- <span data-ttu-id="6d882-112">**Replikerade datorer**: servrar som du vill tooreplicate måste toocomply med lokala och krav för Azure.</span><span class="sxs-lookup"><span data-stu-id="6d882-112">**Replicated machines**: Servers you want tooreplicate need toocomply with on-premises and Azure requirements.</span></span>

<span data-ttu-id="6d882-113">Gå för[steg 2: granska förutsättningar och begränsningar](physical-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="6d882-113">Go too[Step 2: Review prerequisites and limitations](physical-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="6d882-114">Steg 3: Planera kapacitet</span><span class="sxs-lookup"><span data-stu-id="6d882-114">Step 3: Plan capacity</span></span>

<span data-ttu-id="6d882-115">Om du gör en fullständig distribution måste toofigure reda på vilka replikering resurser som du behöver.</span><span class="sxs-lookup"><span data-stu-id="6d882-115">If you're doing a full deployment you need toofigure out what replication resources you need.</span></span> <span data-ttu-id="6d882-116">Om du utför en snabb konfigurera tootest hello miljö kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="6d882-116">If you're doing a quick set up tootest hello environment, you can skip this step.</span></span>

<span data-ttu-id="6d882-117">Gå för[steg3: Planera kapacitet](physical-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="6d882-117">Go too[Step 3: Plan capacity](physical-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="6d882-118">Steg 4: Planera nätverk</span><span class="sxs-lookup"><span data-stu-id="6d882-118">Step 4: Plan networking</span></span>

<span data-ttu-id="6d882-119">Du måste toodo vissa nätverk planera tooensure att virtuella Azure-datorer är anslutna toonetworks efter växling vid fel och att de har hello rätt IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="6d882-119">You need toodo some network planning tooensure that Azure VMs are connected toonetworks after failover occurs, and  that that they have hello right IP addresses.</span></span>

<span data-ttu-id="6d882-120">Gå för[steg 4: Planera nätverk](physical-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="6d882-120">Go too[Step 4: Plan networking](physical-walkthrough-network.md)</span></span>

##  <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="6d882-121">Steg 5: Förbered Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="6d882-121">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="6d882-122">Konfigurera Azure nätverken och lagringsenheterna innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="6d882-122">Set up Azure networks and storage before you start.</span></span> 

<span data-ttu-id="6d882-123">Gå för[steg 5: Förbered Azure](physical-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="6d882-123">Go too[Step 5: Prepare Azure](physical-walkthrough-prepare-azure.md)</span></span>


## <a name="step-6-set-up-a-vault"></a><span data-ttu-id="6d882-124">Steg 6: Konfigurera ett valv</span><span class="sxs-lookup"><span data-stu-id="6d882-124">Step 6: Set up a vault</span></span>

<span data-ttu-id="6d882-125">Du ställer in en tooorchestrate för Recovery Services-valvet och hantera replikering.</span><span class="sxs-lookup"><span data-stu-id="6d882-125">You set up a Recovery Services vault tooorchestrate and manage replication.</span></span> <span data-ttu-id="6d882-126">När du ställer in hello valvet kan du ange vad du vill tooreplicate, och där du vill att tooreplicate den.</span><span class="sxs-lookup"><span data-stu-id="6d882-126">When you set up hello vault, you specify what you want tooreplicate, and where you want tooreplicate it to.</span></span>

<span data-ttu-id="6d882-127">Gå för[steg 6: konfigurera ett valv](physical-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="6d882-127">Go too[Step 6: Set up a vault](physical-walkthrough-create-vault.md)</span></span>

## <a name="step-7-configure-source-and-target-settings"></a><span data-ttu-id="6d882-128">Steg 7: Konfigurera inställningar för källa och mål</span><span class="sxs-lookup"><span data-stu-id="6d882-128">Step 7: Configure source and target settings</span></span>

<span data-ttu-id="6d882-129">Konfigurera inställningar för hello källa och mål (Azure) plats.</span><span class="sxs-lookup"><span data-stu-id="6d882-129">Configure settings for hello source and target (Azure) site.</span></span> <span data-ttu-id="6d882-130">Inställningar för datakälla innehåller och kör installationsprogrammet för enhetlig tooinstall hello lokalt Site Recovery-komponenter.</span><span class="sxs-lookup"><span data-stu-id="6d882-130">Source settings includes running Unified Setup tooinstall hello on-premises Site Recovery components.</span></span>

<span data-ttu-id="6d882-131">Gå för[steg 7: Konfigurera hello källa och mål](physical-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="6d882-131">Go too[Step 7: Set up hello source and target](physical-walkthrough-source-target.md)</span></span>

## <a name="step-8-set-up-a-replication-policy"></a><span data-ttu-id="6d882-132">Steg 8: Skapa en replikeringsprincip</span><span class="sxs-lookup"><span data-stu-id="6d882-132">Step 8: Set up a replication policy</span></span>

<span data-ttu-id="6d882-133">Du konfigurerar en princip toospecify hur fysiska servrar som ska replikeras.</span><span class="sxs-lookup"><span data-stu-id="6d882-133">You set up a policy toospecify how physical servers should replicate.</span></span>

<span data-ttu-id="6d882-134">Gå för[steg 8: skapa en replikeringsprincip](physical-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="6d882-134">Go too[Step 8: Set up a replication policy](physical-walkthrough-replication.md)</span></span>

## <a name="step-9-install-hello-mobility-service"></a><span data-ttu-id="6d882-135">Steg 9: Installera hello mobilitetstjänsten</span><span class="sxs-lookup"><span data-stu-id="6d882-135">Step 9: Install hello Mobility service</span></span>

<span data-ttu-id="6d882-136">Hej mobilitetstjänsten måste installeras på varje server som du vill tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="6d882-136">hello Mobility service must be installed on each server you want tooreplicate.</span></span> <span data-ttu-id="6d882-137">Det finns ett par sätt tooset hello tjänsten med push- eller pull-installation.</span><span class="sxs-lookup"><span data-stu-id="6d882-137">There are a few ways tooset up hello service, with push or pull installation.</span></span>

<span data-ttu-id="6d882-138">Gå för[steg 9: Installera hello mobilitetstjänsten](physical-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="6d882-138">Go too[Step 9: Install hello Mobility service](physical-walkthrough-install-mobility.md)</span></span>

## <a name="step-10-enable-replication"></a><span data-ttu-id="6d882-139">Steg 10: Aktivera replikering</span><span class="sxs-lookup"><span data-stu-id="6d882-139">Step 10: Enable replication</span></span>

<span data-ttu-id="6d882-140">När hello mobilitetstjänsten körs på en server, kan du aktivera replikering för den.</span><span class="sxs-lookup"><span data-stu-id="6d882-140">After hello Mobility service is running on a server, you can enable replication for it.</span></span> <span data-ttu-id="6d882-141">När du har aktiverat inträffar inledande replikering av hello VM.</span><span class="sxs-lookup"><span data-stu-id="6d882-141">After enabling, initial replication of hello VM occurs.</span></span>

<span data-ttu-id="6d882-142">Gå för[steg 10: Aktivera replikering](physical-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="6d882-142">Go too[Step 10: Enable replication](physical-walkthrough-enable-replication.md)</span></span>

## <a name="step-11-run-a-test-failover"></a><span data-ttu-id="6d882-143">Steg 11: Kör ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="6d882-143">Step 11: Run a test failover</span></span>

<span data-ttu-id="6d882-144">När replikeringen har slutförts och deltareplikering körs, kan du köra en testa redundans toomake att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="6d882-144">After initial replication finishes and delta replication is running, you can run a test failover toomake sure everything works as expected.</span></span>

<span data-ttu-id="6d882-145">Gå för[steg 11: kör ett redundanstest](physical-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="6d882-145">Go too[Step 11: Run a test failover](physical-walkthrough-test-failover.md)</span></span>

