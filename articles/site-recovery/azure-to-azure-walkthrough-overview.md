---
title: Replikera virtuella Azure-datorer mellan Azure-regioner | Microsoft Docs
description: "Sammanfattas de steg som krävs för att replikera virtuella Azure-datorer mellan Azure-regioner med Azure Site Recovery-tjänsten i Azure-portalen"
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
ms.openlocfilehash: 9258613161a61e36b1d0c5796d5763c916d66859
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a><span data-ttu-id="0570f-103">Replikera virtuella Azure-datorer mellan regioner med Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="0570f-103">Replicate Azure VMs between regions with Azure Site Recovery</span></span>

><span data-ttu-id="0570f-104">Den här artikeln innehåller en översikt över de steg som krävs för att replikera virtuella Azure-datorer (VM) i en Azure-region till Azure virtuella datorer i en annan region.</span><span class="sxs-lookup"><span data-stu-id="0570f-104">This article provides an overview of the steps required to replicate Azure virtual machines (VMs) in one Azure region to Azure VMs in a different region.</span></span> 

>[!NOTE]
>
> <span data-ttu-id="0570f-105">Azure VM-replikering är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="0570f-105">Azure VM replication is currently in preview.</span></span>

<span data-ttu-id="0570f-106">Publicera kommentarer och frågor längst ned i den här artikeln eller på den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="0570f-106">Post comments and questions at the bottom of this article or on the [Azure Recovery Services forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="step-1-review-architecture"></a><span data-ttu-id="0570f-107">Steg 1: Granska arkitektur</span><span class="sxs-lookup"><span data-stu-id="0570f-107">Step 1: Review architecture</span></span>

<span data-ttu-id="0570f-108">Innan du börjar distributionen bör du granska scenariots arkitektur och komponenter som du behöver distribuera.</span><span class="sxs-lookup"><span data-stu-id="0570f-108">Before you start deployment, review the scenario architecture, and the components you need to deploy.</span></span>

<span data-ttu-id="0570f-109">Gå till [steg 1: granska arkitekturen](azure-to-azure-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="0570f-109">Go to [Step 1: Review the architecture](azure-to-azure-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="0570f-110">Steg 2: Granska krav</span><span class="sxs-lookup"><span data-stu-id="0570f-110">Step 2: Review prerequisites</span></span>

<span data-ttu-id="0570f-111">Kontrollera att du har krav för Azure på platser, inklusive en prenumeration, virtuella nätverk, storage-konton och VM-krav.</span><span class="sxs-lookup"><span data-stu-id="0570f-111">Check that you have the Azure prerequisites in places, including a subscription, virtual networks, storage accounts, and VM requirements.</span></span>

<span data-ttu-id="0570f-112">Gå till [steg 2: Verifiera förutsättningar och begränsningar](azure-to-azure-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="0570f-112">Go to [Step 2: Verify prerequisites and limitations](azure-to-azure-walkthrough-prerequisites.md)</span></span>


## <a name="step-3-plan-networking"></a><span data-ttu-id="0570f-113">Steg 3: Planera nätverk</span><span class="sxs-lookup"><span data-stu-id="0570f-113">Step 3: Plan networking</span></span>

<span data-ttu-id="0570f-114">Kontrollera att utgående anslutning har ställts in på Azure virtuella datorer du vill replikera och att anslutningar från lokala har ställts in.</span><span class="sxs-lookup"><span data-stu-id="0570f-114">Check that outbound connectivity is set up on Azure VMs you want to replicate, and that connections from on-premises are set up.</span></span>

<span data-ttu-id="0570f-115">Gå till [steg 4: Planera nätverk](azure-to-azure-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="0570f-115">Go to [Step 4: Plan networking](azure-to-azure-walkthrough-network.md)</span></span>



## <a name="step-4-create-a-vault"></a><span data-ttu-id="0570f-116">Steg 4: Skapa ett valv</span><span class="sxs-lookup"><span data-stu-id="0570f-116">Step 4: Create a vault</span></span> 

<span data-ttu-id="0570f-117">Du måste skapa ett Recovery Services-valv dirigera och hantera replikering och ange käll-region.</span><span class="sxs-lookup"><span data-stu-id="0570f-117">You need to set up a Recovery Services vault to orchestrate and manage replication, and specify the source region.</span></span>

<span data-ttu-id="0570f-118">Gå till [steg 4: skapa ett valv](azure-to-azure-walkthrough-vault.md)</span><span class="sxs-lookup"><span data-stu-id="0570f-118">Go to [Step 4: Create a vault](azure-to-azure-walkthrough-vault.md)</span></span>


## <a name="step-5-enable-replication"></a><span data-ttu-id="0570f-119">Steg 5: Aktivera replikering</span><span class="sxs-lookup"><span data-stu-id="0570f-119">Step 5: Enable replication</span></span>


<span data-ttu-id="0570f-120">Om du vill aktivera replikering konfigurerar Målinställningar för platsen, ställa in en replikeringsprincip och välj den virtuella Azure-datorer som du vill replikera.</span><span class="sxs-lookup"><span data-stu-id="0570f-120">To enable replication, you configure target location settings, set up a replication policy, and select the Azure VMs that you want to replicate.</span></span> <span data-ttu-id="0570f-121">När du har aktiverat inträffar första replikeringen av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0570f-121">After enabling, initial replication of the VM occurs.</span></span>

<span data-ttu-id="0570f-122">Gå till [steg 5: Aktivera replikering](azure-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="0570f-122">Go to [Step 5: Enable replication](azure-to-azure-walkthrough-enable-replication.md)</span></span>


## <a name="step-6-run-a-test-failover"></a><span data-ttu-id="0570f-123">Steg 6: Köra ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="0570f-123">Step 6: Run a test failover</span></span>

<span data-ttu-id="0570f-124">När replikeringen har slutförts och deltareplikering körs, kan du köra ett redundanstest för att kontrollera att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="0570f-124">After initial replication finishes, and delta replication is running, you can run a test failover to make sure everything works as expected.</span></span>

<span data-ttu-id="0570f-125">Gå till [steg 6: köra ett redundanstest](azure-to-azure-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="0570f-125">Go to [Step 6: Run a test failover](azure-to-azure-walkthrough-test-failover.md)</span></span>


