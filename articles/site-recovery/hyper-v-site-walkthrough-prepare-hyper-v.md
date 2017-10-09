---
title: "aaaPrepare Hyper-V-värd (utan VMM för System Center) för replikering tooAzure | Microsoft Docs"
description: "Beskriver hur tooprepare Hyper-V som värd för replikering tooAzure med hjälp av Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0f204e24-8d78-4076-95c5-8137d1be9c01
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 714b229d5efbd66a9844bd09e36ac3f69919a6bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-prepare-hyper-v-hosts-for-replication-tooazure"></a><span data-ttu-id="a618d-103">Steg 6: Förbereda Hyper-V-värdar för replikering tooAzure</span><span class="sxs-lookup"><span data-stu-id="a618d-103">Step 6: Prepare Hyper-V hosts for replication tooAzure</span></span>

<span data-ttu-id="a618d-104">Använd hello instruktioner i den här artikeln tooprepare lokala Hyper-V-värdar toointeract med Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="a618d-104">Use hello instructions in this article tooprepare on-premises Hyper-V hosts toointeract with Azure Site Recovery.</span></span>

<span data-ttu-id="a618d-105">När du har läst den här artikeln efter eventuella kommentarer längst ned hello eller tekniska frågor om hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="a618d-105">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="prepare-hosts"></a><span data-ttu-id="a618d-106">Förbered värdar</span><span class="sxs-lookup"><span data-stu-id="a618d-106">Prepare hosts</span></span>

- <span data-ttu-id="a618d-107">Kontrollera att hello Hyper-V-värdar uppfyller hello [krav](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).</span><span class="sxs-lookup"><span data-stu-id="a618d-107">Make sure that hello Hyper-V hosts meet hello [prerequisites](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).</span></span>
- <span data-ttu-id="a618d-108">Kontrollera att hello värdarna kan komma åt hello krävs URL: er:</span><span class="sxs-lookup"><span data-stu-id="a618d-108">Make sure that hello hosts can access hello required URLs:</span></span>

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- <span data-ttu-id="a618d-109">Om du har IP-adressbaserade brandväggsregler, se till att de tillåter kommunikation tooAzure.</span><span class="sxs-lookup"><span data-stu-id="a618d-109">If you have IP address-based firewall rules, ensure they allow communication tooAzure.</span></span>
- <span data-ttu-id="a618d-110">Tillåt hello [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653), och hello HTTPS (443)-port.</span><span class="sxs-lookup"><span data-stu-id="a618d-110">Allow hello [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and hello HTTPS (443) port.</span></span>
- <span data-ttu-id="a618d-111">Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och Identity Management).</span><span class="sxs-lookup"><span data-stu-id="a618d-111">Allow IP address ranges for hello Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

<span data-ttu-id="a618d-112">Under distributionen av Site Recovery kan du lägga till Hyper-V-värdar som innehåller virtuella datorer du vill tooreplicate tooa Hyper-V-platsen.</span><span class="sxs-lookup"><span data-stu-id="a618d-112">During Site Recovery deployment, you add Hyper-V hosts that contain VMs you want tooreplicate tooa Hyper-V site.</span></span> <span data-ttu-id="a618d-113">hello Site Recovery-Provider och Recovery Services-agenten installeras på varje värd.</span><span class="sxs-lookup"><span data-stu-id="a618d-113">hello Site Recovery Provider, and Recovery Services agent are installed on each host.</span></span> <span data-ttu-id="a618d-114">hello Hyper-V-platsen har registrerats i hello Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="a618d-114">hello Hyper-V site is registered in hello Recovery Services vault.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a618d-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a618d-115">Next steps</span></span>

<span data-ttu-id="a618d-116">Gå för[steg 7: skapa ett valv](hyper-v-site-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="a618d-116">Go too[Step 7: Create a vault](hyper-v-site-walkthrough-create-vault.md)</span></span>

