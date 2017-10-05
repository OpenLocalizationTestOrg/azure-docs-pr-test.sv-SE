---
title: "Förbereda mål (VMware till Azure) | Microsoft Docs"
description: "Den här artikeln beskriver hur du förbereder din Azure-miljön att starta replikera virtuella VMware-datorer till Azure."
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
ms.openlocfilehash: c84a775564769ddc796aa9d75add019ef1003175
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-target-vmware-to-azure"></a><span data-ttu-id="d27dd-103">Förbereda mål (VMware till Azure)</span><span class="sxs-lookup"><span data-stu-id="d27dd-103">Prepare target (VMware to Azure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d27dd-104">VMware till Azure</span><span class="sxs-lookup"><span data-stu-id="d27dd-104">VMware to Azure</span></span>](./site-recovery-prepare-target-vmware-to-azure.md)
> * [<span data-ttu-id="d27dd-105">Fysiska till Azure</span><span class="sxs-lookup"><span data-stu-id="d27dd-105">Physical to Azure</span></span>](./site-recovery-prepare-target-physical-to-azure.md)

<span data-ttu-id="d27dd-106">Den här artikeln beskriver hur du förbereder din Azure-miljön att starta replikera virtuella VMware-datorer till Azure.</span><span class="sxs-lookup"><span data-stu-id="d27dd-106">This article describes how to prepare your Azure environment to start replicating VMware virtual machines to Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d27dd-107">Krav</span><span class="sxs-lookup"><span data-stu-id="d27dd-107">Prerequisites</span></span>

<span data-ttu-id="d27dd-108">Artikeln förutsätter följande:</span><span class="sxs-lookup"><span data-stu-id="d27dd-108">The article assumes the following:</span></span>
- <span data-ttu-id="d27dd-109">Du har skapat ett Recovery Services-valv för att skydda din virtuella VMware-datorer.</span><span class="sxs-lookup"><span data-stu-id="d27dd-109">You have created a Recovery Services Vault to protect your VMware virtual machines.</span></span> <span data-ttu-id="d27dd-110">Du kan skapa ett Recovery Services-valv från den [Azure-portalen](http://portal.azure.com "Azure-portalen").</span><span class="sxs-lookup"><span data-stu-id="d27dd-110">You can create a Recovery Services Vault from the [Azure portal](http://portal.azure.com "Azure portal").</span></span>
- <span data-ttu-id="d27dd-111">Du har [konfigurering av din lokala miljö](./site-recovery-set-up-vmware-to-azure.md) att replikera virtuella VMware-datorer till Azure.</span><span class="sxs-lookup"><span data-stu-id="d27dd-111">You have [setup your on-premises environment](./site-recovery-set-up-vmware-to-azure.md) to replicate VMware virtual machines to Azure.</span></span>

## <a name="prepare-target"></a><span data-ttu-id="d27dd-112">Förbereda mål</span><span class="sxs-lookup"><span data-stu-id="d27dd-112">Prepare target</span></span>

<span data-ttu-id="d27dd-113">När du har slutfört den **steg 1: Välj skyddsmål** och **steg 2: Förbered källa**, kommer du till **steg3: mål**</span><span class="sxs-lookup"><span data-stu-id="d27dd-113">After completing the **Step 1:Select Protection goal** and **Step 2:Prepare Source**, you are taken to **Step 3: Target**</span></span>

![Förbereda mål](./media/site-recovery-prepare-target-vmware-to-azure/prepare-target-vmware-to-azure.png)

1. <span data-ttu-id="d27dd-115">**Prenumerationen:** från nedrullningsbara menyn, Välj den prenumeration som du vill replikera dina virtuella datorer till.</span><span class="sxs-lookup"><span data-stu-id="d27dd-115">**Subscription:** From the drop down menu, select the Subscription that you want to replicate your virtual machines to.</span></span>
2. <span data-ttu-id="d27dd-116">**Distributionsmodell:** Välj distributionsmodell (klassiska eller Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="d27dd-116">**Deployment Model:** Select the deployment model (Classic or Resource Manager)</span></span>

<span data-ttu-id="d27dd-117">Baserat på den valda distributionsmodellen utförs en verifiering för att säkerställa att du har minst en kompatibel storage-konto och virtuella nätverk i målprenumerationen att replikera och växling vid fel den virtuella datorn till.</span><span class="sxs-lookup"><span data-stu-id="d27dd-117">Based on the chosen deployment model, a validation is run to ensure that you have at least one compatible storage account and virtual network in the target subscription to replicate and failover your virtual machine to.</span></span>

<span data-ttu-id="d27dd-118">Klicka på OK för att gå till nästa steg när validering slutföras.</span><span class="sxs-lookup"><span data-stu-id="d27dd-118">Once the validations complete successfully, click OK to go to the next step.</span></span>

<span data-ttu-id="d27dd-119">Om du inte har en kompatibel Resource Manager storage-konto eller ett virtuellt nätverk eller skulle vilja lägga till fler, kan du göra det genom att klicka på den **+ Lagringskonto** eller **+ nätverk** knappar överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="d27dd-119">If you don't have a compatible Resource Manager storage account or virtual network, or would like to add more, you can do so by clicking the **+ Storage Account** or **+ Network** buttons on the top of the blade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d27dd-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d27dd-120">Next steps</span></span>
<span data-ttu-id="d27dd-121">[Konfigurera replikeringsinställningar](./site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="d27dd-121">[Configure replication settings](./site-recovery-setup-replication-settings-vmware.md).</span></span>
