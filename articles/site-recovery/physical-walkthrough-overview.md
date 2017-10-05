---
title: Replikera fysiska lokala servrar till Azure med Azure Site Recovery | Microsoft Docs
description: "Innehåller en översikt över stegen för att replikera arbetsbelastningar som körs på lokala Windows-/ Linux fysiska servrar till Azure med Azure Site Recovery-tjänsten."
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
ms.openlocfilehash: 0a09b35e98dc0b2f5283c2a707a3a2b8ac9a39f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-physical-servers-to-azure-with-site-recovery"></a><span data-ttu-id="4d016-103">Replikera fysiska servrar till Azure med Site Recovery</span><span class="sxs-lookup"><span data-stu-id="4d016-103">Replicate physical servers to Azure with Site Recovery</span></span>

<span data-ttu-id="4d016-104">Den här artikeln innehåller en översikt över de steg som krävs för att replikera lokala Windows-/ Linux fysiska servrar till Azure med hjälp av den [Azure Site Recovery](site-recovery-overview.md) tjänsten i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4d016-104">This article provides an overview of the steps required to replicate on-premises Windows/Linux physical servers to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>


## <a name="step-1-review-architecture-and-prerequisites"></a><span data-ttu-id="4d016-105">Steg 1: Granska arkitektur och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="4d016-105">Step 1: Review architecture and prerequisites</span></span>

<span data-ttu-id="4d016-106">Innan du börjar distributionen bör granska scenariots arkitektur och kontrollera att du förstår de komponenter som du behöver konfigurera distributionen.</span><span class="sxs-lookup"><span data-stu-id="4d016-106">Before you start deployment, review the scenario architecture, and make sure you understand all the components you need to set up the deployment.</span></span>

<span data-ttu-id="4d016-107">Gå till [steg 1: granska arkitekturen](physical-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="4d016-107">Go to [Step 1: Review the architecture](physical-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="4d016-108">Steg 2: Granska krav</span><span class="sxs-lookup"><span data-stu-id="4d016-108">Step 2: Review prerequisites</span></span>

<span data-ttu-id="4d016-109">Kontrollera att du har de nödvändiga förutsättningarna för varje komponent i distributionen:</span><span class="sxs-lookup"><span data-stu-id="4d016-109">Make sure you have the prerequisites in place for each deployment component:</span></span>

- <span data-ttu-id="4d016-110">**Krav för Azure**: du behöver ett Microsoft Azure-konto, Azure-nätverk och lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="4d016-110">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
- <span data-ttu-id="4d016-111">**Site Recovery-komponenter på lokala**: du behöver en dator som körs lokalt Site Recovery-komponenter.</span><span class="sxs-lookup"><span data-stu-id="4d016-111">**On-premises Site Recovery components**: You need a machine running on-premises Site Recovery components.</span></span>
- <span data-ttu-id="4d016-112">**Replikerade datorer**: servrar som du vill replikera måste uppfylla kraven för Azure och lokalt.</span><span class="sxs-lookup"><span data-stu-id="4d016-112">**Replicated machines**: Servers you want to replicate need to comply with on-premises and Azure requirements.</span></span>

<span data-ttu-id="4d016-113">Gå till [steg 2: granska förutsättningar och begränsningar](physical-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="4d016-113">Go to [Step 2: Review prerequisites and limitations](physical-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="4d016-114">Steg 3: Planera kapacitet</span><span class="sxs-lookup"><span data-stu-id="4d016-114">Step 3: Plan capacity</span></span>

<span data-ttu-id="4d016-115">Om du gör en fullständig distribution måste du ta reda på vilka replikering resurser som du behöver.</span><span class="sxs-lookup"><span data-stu-id="4d016-115">If you're doing a full deployment you need to figure out what replication resources you need.</span></span> <span data-ttu-id="4d016-116">Om du utför en snabb som ställts in för att testa miljön kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="4d016-116">If you're doing a quick set up to test the environment, you can skip this step.</span></span>

<span data-ttu-id="4d016-117">Gå till [Steg3: Planera kapacitet](physical-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="4d016-117">Go to [Step 3: Plan capacity](physical-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="4d016-118">Steg 4: Planera nätverk</span><span class="sxs-lookup"><span data-stu-id="4d016-118">Step 4: Plan networking</span></span>

<span data-ttu-id="4d016-119">Du behöver göra några nätverk som planerar att se till att virtuella Azure-datorer är anslutna till nätverk efter växling vid fel och att de har rätt IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="4d016-119">You need to do some network planning to ensure that Azure VMs are connected to networks after failover occurs, and  that that they have the right IP addresses.</span></span>

<span data-ttu-id="4d016-120">Gå till [steg 4: Planera nätverk](physical-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="4d016-120">Go to [Step 4: Plan networking](physical-walkthrough-network.md)</span></span>

##  <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="4d016-121">Steg 5: Förbered Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="4d016-121">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="4d016-122">Konfigurera Azure nätverken och lagringsenheterna innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="4d016-122">Set up Azure networks and storage before you start.</span></span> 

<span data-ttu-id="4d016-123">Gå till [steg 5: Förbered Azure](physical-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="4d016-123">Go to [Step 5: Prepare Azure](physical-walkthrough-prepare-azure.md)</span></span>


## <a name="step-6-set-up-a-vault"></a><span data-ttu-id="4d016-124">Steg 6: Konfigurera ett valv</span><span class="sxs-lookup"><span data-stu-id="4d016-124">Step 6: Set up a vault</span></span>

<span data-ttu-id="4d016-125">Du kan skapa ett Recovery Services-valv dirigera och hantera replikering.</span><span class="sxs-lookup"><span data-stu-id="4d016-125">You set up a Recovery Services vault to orchestrate and manage replication.</span></span> <span data-ttu-id="4d016-126">När du ställer in valvet ange vad du vill replikera och var du vill replikera den till.</span><span class="sxs-lookup"><span data-stu-id="4d016-126">When you set up the vault, you specify what you want to replicate, and where you want to replicate it to.</span></span>

<span data-ttu-id="4d016-127">Gå till [steg 6: konfigurera ett valv](physical-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="4d016-127">Go to [Step 6: Set up a vault](physical-walkthrough-create-vault.md)</span></span>

## <a name="step-7-configure-source-and-target-settings"></a><span data-ttu-id="4d016-128">Steg 7: Konfigurera inställningar för källa och mål</span><span class="sxs-lookup"><span data-stu-id="4d016-128">Step 7: Configure source and target settings</span></span>

<span data-ttu-id="4d016-129">Konfigurera inställningar för källa och mål (Azure) plats.</span><span class="sxs-lookup"><span data-stu-id="4d016-129">Configure settings for the source and target (Azure) site.</span></span> <span data-ttu-id="4d016-130">Inställningar för datakälla innehåller Unified hur du installerar lokalt Site Recovery-komponenter.</span><span class="sxs-lookup"><span data-stu-id="4d016-130">Source settings includes running Unified Setup to install the on-premises Site Recovery components.</span></span>

<span data-ttu-id="4d016-131">Gå till [steg 7: Konfigurera källa och mål](physical-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="4d016-131">Go to [Step 7: Set up the source and target](physical-walkthrough-source-target.md)</span></span>

## <a name="step-8-set-up-a-replication-policy"></a><span data-ttu-id="4d016-132">Steg 8: Skapa en replikeringsprincip</span><span class="sxs-lookup"><span data-stu-id="4d016-132">Step 8: Set up a replication policy</span></span>

<span data-ttu-id="4d016-133">Du konfigurerar en princip för att ange hur fysiska servrar som ska replikeras.</span><span class="sxs-lookup"><span data-stu-id="4d016-133">You set up a policy to specify how physical servers should replicate.</span></span>

<span data-ttu-id="4d016-134">Gå till [steg 8: skapa en replikeringsprincip](physical-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="4d016-134">Go to [Step 8: Set up a replication policy](physical-walkthrough-replication.md)</span></span>

## <a name="step-9-install-the-mobility-service"></a><span data-ttu-id="4d016-135">Steg 9: Installera mobilitetstjänsten</span><span class="sxs-lookup"><span data-stu-id="4d016-135">Step 9: Install the Mobility service</span></span>

<span data-ttu-id="4d016-136">Mobilitetstjänsten måste installeras på varje server som du vill replikera.</span><span class="sxs-lookup"><span data-stu-id="4d016-136">The Mobility service must be installed on each server you want to replicate.</span></span> <span data-ttu-id="4d016-137">Det finns några olika sätt att konfigurera tjänsten med push- eller pull-installation.</span><span class="sxs-lookup"><span data-stu-id="4d016-137">There are a few ways to set up the service, with push or pull installation.</span></span>

<span data-ttu-id="4d016-138">Gå till [steg 9: Installera mobilitetstjänsten](physical-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="4d016-138">Go to [Step 9: Install the Mobility service](physical-walkthrough-install-mobility.md)</span></span>

## <a name="step-10-enable-replication"></a><span data-ttu-id="4d016-139">Steg 10: Aktivera replikering</span><span class="sxs-lookup"><span data-stu-id="4d016-139">Step 10: Enable replication</span></span>

<span data-ttu-id="4d016-140">När mobilitetstjänsten körs på en server, kan du aktivera replikering för den.</span><span class="sxs-lookup"><span data-stu-id="4d016-140">After the Mobility service is running on a server, you can enable replication for it.</span></span> <span data-ttu-id="4d016-141">När du har aktiverat inträffar första replikeringen av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4d016-141">After enabling, initial replication of the VM occurs.</span></span>

<span data-ttu-id="4d016-142">Gå till [steg 10: Aktivera replikering](physical-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="4d016-142">Go to [Step 10: Enable replication](physical-walkthrough-enable-replication.md)</span></span>

## <a name="step-11-run-a-test-failover"></a><span data-ttu-id="4d016-143">Steg 11: Kör ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="4d016-143">Step 11: Run a test failover</span></span>

<span data-ttu-id="4d016-144">När replikeringen har slutförts och deltareplikering körs, kan du köra ett redundanstest för att kontrollera att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="4d016-144">After initial replication finishes and delta replication is running, you can run a test failover to make sure everything works as expected.</span></span>

<span data-ttu-id="4d016-145">Gå till [steg 11: kör ett redundanstest](physical-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="4d016-145">Go to [Step 11: Run a test failover](physical-walkthrough-test-failover.md)</span></span>

