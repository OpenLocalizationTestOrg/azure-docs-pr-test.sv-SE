---
title: "Förbereda Hyper-V-värdar (utan VMM för System Center) för replikering till Azure | Microsoft Docs"
description: "Beskriver hur du förbereder Hyper-V-värdar för replikering till Azure med Azure Site Recovery"
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
ms.openlocfilehash: f9bcaa8e55be6e8fddaf88ebc3f18f5dbb2811e4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="step-6-prepare-hyper-v-hosts-for-replication-to-azure"></a><span data-ttu-id="1f080-103">Steg 6: Förbereda Hyper-V-värdar för replikering till Azure</span><span class="sxs-lookup"><span data-stu-id="1f080-103">Step 6: Prepare Hyper-V hosts for replication to Azure</span></span>

<span data-ttu-id="1f080-104">Använd instruktionerna i den här artikeln för att förbereda lokala Hyper-V-värdar att interagera med Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="1f080-104">Use the instructions in this article to prepare on-premises Hyper-V hosts to interact with Azure Site Recovery.</span></span>

<span data-ttu-id="1f080-105">När du har läst den här artikeln efter eventuella kommentarer längst ned eller tekniska frågor om den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="1f080-105">After reading this article, post any comments at the bottom, or ask technical questions on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="prepare-hosts"></a><span data-ttu-id="1f080-106">Förbered värdar</span><span class="sxs-lookup"><span data-stu-id="1f080-106">Prepare hosts</span></span>

- <span data-ttu-id="1f080-107">Kontrollera att Hyper-V-värdar uppfyller de [krav](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).</span><span class="sxs-lookup"><span data-stu-id="1f080-107">Make sure that the Hyper-V hosts meet the [prerequisites](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).</span></span>
- <span data-ttu-id="1f080-108">Kontrollera att värdarna kan komma åt de nödvändiga URL: er:</span><span class="sxs-lookup"><span data-stu-id="1f080-108">Make sure that the hosts can access the required URLs:</span></span>

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- <span data-ttu-id="1f080-109">Om du har IP-adressbaserade brandväggsregler ontrollerar du att de tillåter kommunikation till Azure.</span><span class="sxs-lookup"><span data-stu-id="1f080-109">If you have IP address-based firewall rules, ensure they allow communication to Azure.</span></span>
- <span data-ttu-id="1f080-110">Tillåt [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-intervall för Azures datacenter) och HTTPS-port 443.</span><span class="sxs-lookup"><span data-stu-id="1f080-110">Allow the [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and the HTTPS (443) port.</span></span>
- <span data-ttu-id="1f080-111">Tillåt IP-adressintervall för Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och identitetshantering).</span><span class="sxs-lookup"><span data-stu-id="1f080-111">Allow IP address ranges for the Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

<span data-ttu-id="1f080-112">Under distributionen av Site Recovery kan du lägga till Hyper-V-värdar som innehåller virtuella datorer som du vill replikera till en Hyper-V-plats.</span><span class="sxs-lookup"><span data-stu-id="1f080-112">During Site Recovery deployment, you add Hyper-V hosts that contain VMs you want to replicate to a Hyper-V site.</span></span> <span data-ttu-id="1f080-113">Site Recovery-Provider och Recovery Services-agenten installeras på varje värd.</span><span class="sxs-lookup"><span data-stu-id="1f080-113">The Site Recovery Provider, and Recovery Services agent are installed on each host.</span></span> <span data-ttu-id="1f080-114">Hyper-V-platsen har registrerats i Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="1f080-114">The Hyper-V site is registered in the Recovery Services vault.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f080-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1f080-115">Next steps</span></span>

<span data-ttu-id="1f080-116">Gå till [steg 7: skapa ett valv](hyper-v-site-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="1f080-116">Go to [Step 7: Create a vault](hyper-v-site-walkthrough-create-vault.md)</span></span>

