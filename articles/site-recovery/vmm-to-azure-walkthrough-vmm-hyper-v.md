---
title: "aaaPrepare System Center VMM för Hyper-V-replikering tooAzure | Microsoft Docs"
description: "Beskriver hur tooprepare System Center VMM-servern för Hyper-V-replikering tooAzure, med hjälp av Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: afcd81ae-d192-4013-a0af-3dac45b3c7e9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 773b06afaf7d3eea1fe64f050bf3970943cf466a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-prepare-vmm-servers-and-hyper-v-hosts-for-hyper-v-replication-tooazure"></a><span data-ttu-id="52415-103">Steg 6: Förbereda VMM-servrar och Hyper-V-värdar för tooAzure för Hyper-V-replikering</span><span class="sxs-lookup"><span data-stu-id="52415-103">Step 6: Prepare VMM servers and Hyper-V hosts for Hyper-V replication tooAzure</span></span>

<span data-ttu-id="52415-104">När du har installerat [Azure komponenter](vmm-to-azure-walkthrough-prepare-azure.md) hello distribution kan använda hello instruktionerna i den här artikeln tooprepare lokal VMM-servrar och Hyper-V-värdar toointeract med Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="52415-104">After setting up [Azure components](vmm-to-azure-walkthrough-prepare-azure.md) for hello deployment, use hello instructions in this article tooprepare on-premises VMM servers and Hyper-V hosts toointeract with Azure Site Recovery.</span></span>

<span data-ttu-id="52415-105">När du har läst den här artikeln efter eventuella kommentarer längst ned hello eller tekniska frågor om hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="52415-105">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="prepare-vmm-servers"></a><span data-ttu-id="52415-106">Förbereda VMM-servrar</span><span class="sxs-lookup"><span data-stu-id="52415-106">Prepare VMM servers</span></span>

- <span data-ttu-id="52415-107">Du behöver minst en VMM-server som uppfyller hello stöd för Site Recovery replikering (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).</span><span class="sxs-lookup"><span data-stu-id="52415-107">You need at least one VMM server that meet hello support requirements for Site Recovery replication (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).</span></span>
- <span data-ttu-id="52415-108">Kontrollera att du har skapat hello VMM-server för [nätverksmappning](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span><span class="sxs-lookup"><span data-stu-id="52415-108">Make sure you've prepared hello VMM server for [network mapping](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span></span>
- <span data-ttu-id="52415-109">Se till att hello VMM-servern kan komma åt dessa URL: er</span><span class="sxs-lookup"><span data-stu-id="52415-109">Make sure that hello VMM server can access these URLs</span></span>

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- <span data-ttu-id="52415-110">Om du har IP-adressbaserade brandväggsregler, se till att de tillåter kommunikation tooAzure.</span><span class="sxs-lookup"><span data-stu-id="52415-110">If you have IP address-based firewall rules, ensure they allow communication tooAzure.</span></span>
- <span data-ttu-id="52415-111">Tillåt hello [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653), och hello HTTPS (443)-port.</span><span class="sxs-lookup"><span data-stu-id="52415-111">Allow hello [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and hello HTTPS (443) port.</span></span>
- <span data-ttu-id="52415-112">Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och Identity Management).</span><span class="sxs-lookup"><span data-stu-id="52415-112">Allow IP address ranges for hello Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

<span data-ttu-id="52415-113">Under distributionen av Site Recovery hämta hello Site Recovery-providern och installera den på varje VMM-server.</span><span class="sxs-lookup"><span data-stu-id="52415-113">During Site Recovery deployment, you download hello Site Recovery Provider and install it on each VMM server.</span></span> <span data-ttu-id="52415-114">hello VMM-servern registreras i hello Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="52415-114">hello VMM server is registered in hello Recovery Services vault.</span></span>




## <a name="next-steps"></a><span data-ttu-id="52415-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="52415-115">Next steps</span></span>

<span data-ttu-id="52415-116">Gå för[steg 7: skapa ett valv](vmm-to-azure-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="52415-116">Go too[Step 7: Create a vault](vmm-to-azure-walkthrough-create-vault.md)</span></span>

