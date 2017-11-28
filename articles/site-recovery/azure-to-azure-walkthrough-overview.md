---
title: aaaReplicate Azure virtuella datorer mellan Azure-regioner | Microsoft Docs
description: "Sammanfattar hello steg krävs tooreplicate Azure virtuella datorer mellan Azure-regioner med hello Azure Site Recovery-tjänsten i hello Azure-portalen"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 6dd36239-4363-4538-bf80-a18e71b8ec67
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: f4ac386f040945131f8a10f02143437f4441e298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a><span data-ttu-id="3573b-103">Replikera virtuella Azure-datorer mellan regioner med Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="3573b-103">Replicate Azure VMs between regions with Azure Site Recovery</span></span>

><span data-ttu-id="3573b-104">Den här artikeln innehåller en översikt över hello steg krävs tooreplicate Azure virtuella datorer (VM) i en Azure-region tooAzure virtuella datorer i en annan region.</span><span class="sxs-lookup"><span data-stu-id="3573b-104">This article provides an overview of hello steps required tooreplicate Azure virtual machines (VMs) in one Azure region tooAzure VMs in a different region.</span></span> 

>[!NOTE]
>
> <span data-ttu-id="3573b-105">Azure VM-replikering är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="3573b-105">Azure VM replication is currently in preview.</span></span>

<span data-ttu-id="3573b-106">Publicera kommentarer och frågor längst ned hello av den här artikeln eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="3573b-106">Post comments and questions at hello bottom of this article or on hello [Azure Recovery Services forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="step-1-review-architecture"></a><span data-ttu-id="3573b-107">Steg 1: Granska arkitektur</span><span class="sxs-lookup"><span data-stu-id="3573b-107">Step 1: Review architecture</span></span>

<span data-ttu-id="3573b-108">Innan du börjar distributionen bör du granska hello scenariots arkitektur och hello-komponenter som du behöver toodeploy.</span><span class="sxs-lookup"><span data-stu-id="3573b-108">Before you start deployment, review hello scenario architecture, and hello components you need toodeploy.</span></span>

<span data-ttu-id="3573b-109">Gå för[steg 1: granska hello-arkitektur](azure-to-azure-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="3573b-109">Go too[Step 1: Review hello architecture](azure-to-azure-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="3573b-110">Steg 2: Granska krav</span><span class="sxs-lookup"><span data-stu-id="3573b-110">Step 2: Review prerequisites</span></span>

<span data-ttu-id="3573b-111">Kontrollera att du har hello Azure-kraven på platser, inklusive en prenumeration, virtuella nätverk, storage-konton och VM-krav.</span><span class="sxs-lookup"><span data-stu-id="3573b-111">Check that you have hello Azure prerequisites in places, including a subscription, virtual networks, storage accounts, and VM requirements.</span></span>

<span data-ttu-id="3573b-112">Gå för[steg 2: Verifiera förutsättningar och begränsningar](azure-to-azure-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="3573b-112">Go too[Step 2: Verify prerequisites and limitations](azure-to-azure-walkthrough-prerequisites.md)</span></span>


## <a name="step-3-plan-networking"></a><span data-ttu-id="3573b-113">Steg 3: Planera nätverk</span><span class="sxs-lookup"><span data-stu-id="3573b-113">Step 3: Plan networking</span></span>

<span data-ttu-id="3573b-114">Kontrollera att utgående anslutning har ställts in på Azure virtuella datorer du vill tooreplicate, och att anslutningar från lokala har ställts in.</span><span class="sxs-lookup"><span data-stu-id="3573b-114">Check that outbound connectivity is set up on Azure VMs you want tooreplicate, and that connections from on-premises are set up.</span></span>

<span data-ttu-id="3573b-115">Gå för[steg 4: Planera nätverk](azure-to-azure-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="3573b-115">Go too[Step 4: Plan networking](azure-to-azure-walkthrough-network.md)</span></span>



## <a name="step-4-create-a-vault"></a><span data-ttu-id="3573b-116">Steg 4: Skapa ett valv</span><span class="sxs-lookup"><span data-stu-id="3573b-116">Step 4: Create a vault</span></span> 

<span data-ttu-id="3573b-117">Du behöver tooset upp en Recovery Services-valvet tooorchestrate hantera replikering och ange hello källa region.</span><span class="sxs-lookup"><span data-stu-id="3573b-117">You need tooset up a Recovery Services vault tooorchestrate and manage replication, and specify hello source region.</span></span>

<span data-ttu-id="3573b-118">Gå för[steg 4: skapa ett valv](azure-to-azure-walkthrough-vault.md)</span><span class="sxs-lookup"><span data-stu-id="3573b-118">Go too[Step 4: Create a vault](azure-to-azure-walkthrough-vault.md)</span></span>


## <a name="step-5-enable-replication"></a><span data-ttu-id="3573b-119">Steg 5: Aktivera replikering</span><span class="sxs-lookup"><span data-stu-id="3573b-119">Step 5: Enable replication</span></span>


<span data-ttu-id="3573b-120">tooenable replikering måste du konfigurera platsen Målinställningar, ställer in en replikeringsprincip och väljer hello Azure virtuella datorer som du vill tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="3573b-120">tooenable replication, you configure target location settings, set up a replication policy, and select hello Azure VMs that you want tooreplicate.</span></span> <span data-ttu-id="3573b-121">När du har aktiverat inträffar inledande replikering av hello VM.</span><span class="sxs-lookup"><span data-stu-id="3573b-121">After enabling, initial replication of hello VM occurs.</span></span>

<span data-ttu-id="3573b-122">Gå för[steg 5: Aktivera replikering](azure-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="3573b-122">Go too[Step 5: Enable replication](azure-to-azure-walkthrough-enable-replication.md)</span></span>


## <a name="step-6-run-a-test-failover"></a><span data-ttu-id="3573b-123">Steg 6: Köra ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="3573b-123">Step 6: Run a test failover</span></span>

<span data-ttu-id="3573b-124">När replikeringen har slutförts och deltareplikering körs, kan du köra en testa redundans toomake att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="3573b-124">After initial replication finishes, and delta replication is running, you can run a test failover toomake sure everything works as expected.</span></span>

<span data-ttu-id="3573b-125">Gå för[steg 6: köra ett redundanstest](azure-to-azure-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="3573b-125">Go too[Step 6: Run a test failover](azure-to-azure-walkthrough-test-failover.md)</span></span>


