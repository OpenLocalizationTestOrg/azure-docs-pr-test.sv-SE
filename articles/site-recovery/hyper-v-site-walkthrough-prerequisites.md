---
title: "aaaReview hello krav för Hyper-V tooAzure replikering (utan System Center VMM) med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hello förutsättningar för att konfigurera replikering, redundans och återställning av lokala datorer med Hyper-V tooAzure med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 7ef3fb46-52f5-4c8a-b1a1-658c2305762a
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 3eefc3b7e3982ec6c413c1db7f7784863f9c701d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-for-hyper-v-without-vmm-tooazure-replication"></a><span data-ttu-id="6ffb1-103">Steg 2: Granska hello krav för Hyper-V (utan VMM) tooAzure-replikering</span><span class="sxs-lookup"><span data-stu-id="6ffb1-103">Step 2: Review hello prerequisites for Hyper-V (without VMM) tooAzure replication</span></span>

<span data-ttu-id="6ffb1-104">hello kraven sammanfattas i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="6ffb1-104">hello prerequisites are summarized in hello table.</span></span>


<span data-ttu-id="6ffb1-105">**Krav**</span><span class="sxs-lookup"><span data-stu-id="6ffb1-105">**Prerequisite**</span></span> | <span data-ttu-id="6ffb1-106">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="6ffb1-106">**Details**</span></span> 
--- | --- 
<span data-ttu-id="6ffb1-107">**Azure**</span><span class="sxs-lookup"><span data-stu-id="6ffb1-107">**Azure**</span></span> | <span data-ttu-id="6ffb1-108">Lär dig om [Azure-krav](site-recovery-prereq.md#azure-requirements).</span><span class="sxs-lookup"><span data-stu-id="6ffb1-108">Learn about [Azure requirements](site-recovery-prereq.md#azure-requirements).</span></span>
<span data-ttu-id="6ffb1-109">**Lokala servrar**</span><span class="sxs-lookup"><span data-stu-id="6ffb1-109">**On-premises servers**</span></span> | <span data-ttu-id="6ffb1-110">[Lär dig mer](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm) om kraven för hello lokala Hyper-V-värdar.</span><span class="sxs-lookup"><span data-stu-id="6ffb1-110">[Learn more](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm) about requirements for hello on-premises Hyper-V hosts.</span></span>
<span data-ttu-id="6ffb1-111">**Lokala virtuella Hyper-V-datorer**</span><span class="sxs-lookup"><span data-stu-id="6ffb1-111">**On-premises Hyper-V VMs**</span></span> | <span data-ttu-id="6ffb1-112">Virtuella datorer du vill ska köra tooreplicate en [operativsystem som stöds](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), och överensstämma med [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="6ffb1-112">VMs you want tooreplicate should be running a [supported operating system](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), and conform with [Azure prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>
<span data-ttu-id="6ffb1-113">**Azure-URL: er**</span><span class="sxs-lookup"><span data-stu-id="6ffb1-113">**Azure URLs**</span></span> | <span data-ttu-id="6ffb1-114">Hyper-V-värdar behöver komma åt toothese URL: er:</span><span class="sxs-lookup"><span data-stu-id="6ffb1-114">Hyper-V hosts need access toothese URLs:</span></span><br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> <span data-ttu-id="6ffb1-115">Om du har IP-adressbaserade brandväggsregler, se till att de tillåter kommunikation tooAzure.</span><span class="sxs-lookup"><span data-stu-id="6ffb1-115">If you have IP address-based firewall rules, ensure they allow communication tooAzure.</span></span><br/></br> <span data-ttu-id="6ffb1-116">Tillåt hello [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653), och hello HTTPS (443)-port.</span><span class="sxs-lookup"><span data-stu-id="6ffb1-116">Allow hello [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and hello HTTPS (443) port.</span></span><br/></br> <span data-ttu-id="6ffb1-117">Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och Identity Management).</span><span class="sxs-lookup"><span data-stu-id="6ffb1-117">Allow IP address ranges for hello Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>



## <a name="next-steps"></a><span data-ttu-id="6ffb1-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6ffb1-118">Next steps</span></span>

- <span data-ttu-id="6ffb1-119">Om du utför en fullständig distribution går för[steg3: Planera kapacitet](hyper-v-site-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="6ffb1-119">If you're doing a full deployment, go too[Step 3: Plan capacity](hyper-v-site-walkthrough-capacity.md)</span></span>
- <span data-ttu-id="6ffb1-120">Om du utför en enkel testdistributionen gå för[steg 4: Planera nätverk](hyper-v-site-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="6ffb1-120">If you're doing a simple test deployment, go too[Step 4: Plan networking](hyper-v-site-walkthrough-network.md).</span></span>
