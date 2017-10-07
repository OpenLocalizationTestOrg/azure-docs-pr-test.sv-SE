---
title: "Förbereda mål (VMware tooAzure) | Microsoft Docs"
description: "Den här artikeln beskriver hur tooprepare din Azure-miljön toostart replikera VMware tooAzure för virtuella datorer."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhemraj
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 5/31/2017
ms.author: bsiva
ms.openlocfilehash: 5975d3c122032f92f8df370ee74fa0c7012ebe2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-target-vmware-tooazure"></a><span data-ttu-id="3c85b-103">Förbereda mål (VMware tooAzure)</span><span class="sxs-lookup"><span data-stu-id="3c85b-103">Prepare target (VMware tooAzure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3c85b-104">VMware tooAzure</span><span class="sxs-lookup"><span data-stu-id="3c85b-104">VMware tooAzure</span></span>](./site-recovery-prepare-target-vmware-to-azure.md)
> * [<span data-ttu-id="3c85b-105">Fysisk tooAzure</span><span class="sxs-lookup"><span data-stu-id="3c85b-105">Physical tooAzure</span></span>](./site-recovery-prepare-target-physical-to-azure.md)

<span data-ttu-id="3c85b-106">Den här artikeln beskriver hur tooprepare din Azure-miljön toostart replikera VMware tooAzure för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3c85b-106">This article describes how tooprepare your Azure environment toostart replicating VMware virtual machines tooAzure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c85b-107">Krav</span><span class="sxs-lookup"><span data-stu-id="3c85b-107">Prerequisites</span></span>

<span data-ttu-id="3c85b-108">hello förutsätter hello följande:</span><span class="sxs-lookup"><span data-stu-id="3c85b-108">hello article assumes hello following:</span></span>
- <span data-ttu-id="3c85b-109">Du har skapat en Återställningstjänstvalvet tooprotect VMware-datorer.</span><span class="sxs-lookup"><span data-stu-id="3c85b-109">You have created a Recovery Services Vault tooprotect your VMware virtual machines.</span></span> <span data-ttu-id="3c85b-110">Du kan skapa ett Recovery Services-valv från hello [Azure-portalen](http://portal.azure.com "Azure-portalen").</span><span class="sxs-lookup"><span data-stu-id="3c85b-110">You can create a Recovery Services Vault from hello [Azure portal](http://portal.azure.com "Azure portal").</span></span>
- <span data-ttu-id="3c85b-111">Du har [konfigurering av din lokala miljö](./site-recovery-set-up-vmware-to-azure.md) tooreplicate VMware virtuella datorer tooAzure.</span><span class="sxs-lookup"><span data-stu-id="3c85b-111">You have [setup your on-premises environment](./site-recovery-set-up-vmware-to-azure.md) tooreplicate VMware virtual machines tooAzure.</span></span>

## <a name="prepare-target"></a><span data-ttu-id="3c85b-112">Förbereda mål</span><span class="sxs-lookup"><span data-stu-id="3c85b-112">Prepare target</span></span>

<span data-ttu-id="3c85b-113">När du har slutfört hello **steg 1: Välj skyddsmål** och **steg 2: Förbered källa**, tas du för**steg3: mål**</span><span class="sxs-lookup"><span data-stu-id="3c85b-113">After completing hello **Step 1:Select Protection goal** and **Step 2:Prepare Source**, you are taken too**Step 3: Target**</span></span>

![Förbereda mål](./media/site-recovery-prepare-target-vmware-to-azure/prepare-target-vmware-to-azure.png)

1. <span data-ttu-id="3c85b-115">**Prenumerationen:** från hello se en meny, Välj hello prenumeration som du vill tooreplicate dina virtuella datorer till.</span><span class="sxs-lookup"><span data-stu-id="3c85b-115">**Subscription:** From hello drop down menu, select hello Subscription that you want tooreplicate your virtual machines to.</span></span>
2. <span data-ttu-id="3c85b-116">**Distributionsmodell:** väljer hello distributionsmodell (klassiska eller Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="3c85b-116">**Deployment Model:** Select hello deployment model (Classic or Resource Manager)</span></span>

<span data-ttu-id="3c85b-117">Baserat på hello valt distributionsmodell utförs en verifiering tooensure som du har minst en kompatibel storage-konto och virtuella nätverk i hello mål prenumeration tooreplicate och växling vid fel den virtuella datorn till.</span><span class="sxs-lookup"><span data-stu-id="3c85b-117">Based on hello chosen deployment model, a validation is run tooensure that you have at least one compatible storage account and virtual network in hello target subscription tooreplicate and failover your virtual machine to.</span></span>

<span data-ttu-id="3c85b-118">Klicka på OK toogo toohello nästa steg när hello verifieringar kan slutföras.</span><span class="sxs-lookup"><span data-stu-id="3c85b-118">Once hello validations complete successfully, click OK toogo toohello next step.</span></span>

<span data-ttu-id="3c85b-119">Om du inte har en kompatibel Resource Manager storage-konto eller ett virtuellt nätverk eller vill tooadd mer, kan du göra det genom att klicka på hello **+ Lagringskonto** eller **+ nätverk** knappar på hello överkant hello bladet.</span><span class="sxs-lookup"><span data-stu-id="3c85b-119">If you don't have a compatible Resource Manager storage account or virtual network, or would like tooadd more, you can do so by clicking hello **+ Storage Account** or **+ Network** buttons on hello top of hello blade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c85b-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3c85b-120">Next steps</span></span>
<span data-ttu-id="3c85b-121">[Konfigurera replikeringsinställningar](./site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="3c85b-121">[Configure replication settings](./site-recovery-setup-replication-settings-vmware.md).</span></span>
