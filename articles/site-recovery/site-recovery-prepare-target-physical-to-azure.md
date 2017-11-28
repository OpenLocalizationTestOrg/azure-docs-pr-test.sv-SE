---
title: "Förbereda mål (fysisk till Azure) | Microsoft Docs"
description: "Den här artikeln beskriver hur du förbereder din Azure-miljön att starta replikering av fysiska servrar som kör Windows eller Linux till Azure."
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
ms.openlocfilehash: aa7a32ace8354f615a8b8cc137f6bdf48fbadf48
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-target-vmware-to-azure"></a><span data-ttu-id="53a59-103">Förbereda mål (VMware till Azure)</span><span class="sxs-lookup"><span data-stu-id="53a59-103">Prepare target (VMware to Azure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="53a59-104">VMware till Azure</span><span class="sxs-lookup"><span data-stu-id="53a59-104">VMware to Azure</span></span>](./site-recovery-prepare-target-vmware-to-azure.md)
> * [<span data-ttu-id="53a59-105">Fysiska till Azure</span><span class="sxs-lookup"><span data-stu-id="53a59-105">Physical to Azure</span></span>](./site-recovery-prepare-target-physical-to-azure.md)

<span data-ttu-id="53a59-106">Den här artikeln beskriver hur du förbereder din Azure-miljön att starta replikering av fysiska servrar (x 64) som kör Windows eller Linux till Azure.</span><span class="sxs-lookup"><span data-stu-id="53a59-106">This article describes how to prepare your Azure environment to start replicating physical servers (x64) running Windows or Linux into Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="53a59-107">Krav</span><span class="sxs-lookup"><span data-stu-id="53a59-107">Prerequisites</span></span>

<span data-ttu-id="53a59-108">Artikeln förutsätter följande:</span><span class="sxs-lookup"><span data-stu-id="53a59-108">The article assumes the following:</span></span>
- <span data-ttu-id="53a59-109">Du har skapat en Återställningstjänstvalvet för att skydda din fysiska servrar.</span><span class="sxs-lookup"><span data-stu-id="53a59-109">You have created a Recovery Services Vault to protect your physical servers.</span></span> <span data-ttu-id="53a59-110">Du kan skapa ett Recovery Services-valv från den [Azure-portalen](http://portal.azure.com "Azure-portalen").</span><span class="sxs-lookup"><span data-stu-id="53a59-110">You can create a Recovery Services Vault from the [Azure portal](http://portal.azure.com "Azure portal").</span></span>
- <span data-ttu-id="53a59-111">Du har [konfigurering av din lokala miljö](./site-recovery-set-up-physical-to-azure.md) att replikera fysiska servrar till Azure.</span><span class="sxs-lookup"><span data-stu-id="53a59-111">You have [setup your on-premises environment](./site-recovery-set-up-physical-to-azure.md) to replicate physical servers to Azure.</span></span>

## <a name="prepare-target"></a><span data-ttu-id="53a59-112">Förbereda mål</span><span class="sxs-lookup"><span data-stu-id="53a59-112">Prepare target</span></span>

<span data-ttu-id="53a59-113">När du har slutfört den **steg 1: Välj skyddsmål** och **steg 2: Förbered källa**, kommer du till **steg3: mål**</span><span class="sxs-lookup"><span data-stu-id="53a59-113">After completing the **Step 1:Select Protection goal** and **Step 2:Prepare Source**, you are taken to **Step 3: Target**</span></span>

![Förbereda mål](./media/site-recovery-prepare-target-physical-to-azure/prepare-target-physical-to-azure.png)

1. <span data-ttu-id="53a59-115">**Prenumerationen:** från nedrullningsbara menyn, Välj den prenumeration som du vill replikera dina fysiska servrar till.</span><span class="sxs-lookup"><span data-stu-id="53a59-115">**Subscription:** From the drop down menu, select the Subscription that you want to replicate your physical servers to.</span></span>
2. <span data-ttu-id="53a59-116">**Distributionsmodell:** Välj distributionsmodell (klassiska eller Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="53a59-116">**Deployment Model:** Select the deployment model (Classic or Resource Manager)</span></span>

<span data-ttu-id="53a59-117">Baserat på den valda distributionsmodellen körs en verifiering för att säkerställa att du har minst en kompatibel storage-konto och virtuella nätverk i målprenumerationen för replikering och redundans din fysiska servrar med.</span><span class="sxs-lookup"><span data-stu-id="53a59-117">Based on the chosen deployment model, a validation is run to ensure that you have at least one compatible storage account and virtual network in the target subscription to replicate and failover your physical servers to.</span></span>

<span data-ttu-id="53a59-118">Klicka på OK för att gå till nästa steg när validering slutföras.</span><span class="sxs-lookup"><span data-stu-id="53a59-118">Once the validations complete successfully, click OK to go to the next step.</span></span>

<span data-ttu-id="53a59-119">Om du inte har en kompatibel Resource Manager storage-konto eller ett virtuellt nätverk eller skulle vilja lägga till fler, kan du göra det genom att klicka på den **+ Lagringskonto** eller **+ nätverk** knappar överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="53a59-119">If you don't have a compatible Resource Manager storage account or virtual network, or would like to add more, you can do so by clicking the **+ Storage Account** or **+ Network** buttons on the top of the blade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="53a59-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53a59-120">Next steps</span></span>
<span data-ttu-id="53a59-121">[Konfigurera replikeringsinställningar](./site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="53a59-121">[Configure replication settings](./site-recovery-setup-replication-settings-vmware.md).</span></span>
