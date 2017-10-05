---
title: "Förbereda VMM för System Center för Hyper-V-replikering till Azure | Microsoft Docs"
description: "Beskriver hur du förbereder System Center VMM-servern för Hyper-V-replikering till Azure med Azure Site Recovery"
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
ms.openlocfilehash: ec118ed837dbf140083b3ae1e4ecd41c81562018
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="step-6-prepare-vmm-servers-and-hyper-v-hosts-for-hyper-v-replication-to-azure"></a><span data-ttu-id="12adf-103">Steg 6: Förbereda VMM-servrar och Hyper-V-värdar för Hyper-V-replikering till Azure</span><span class="sxs-lookup"><span data-stu-id="12adf-103">Step 6: Prepare VMM servers and Hyper-V hosts for Hyper-V replication to Azure</span></span>

<span data-ttu-id="12adf-104">När du har installerat [Azure komponenter](vmm-to-azure-walkthrough-prepare-azure.md) för distributionen, följ instruktionerna i den här artikeln förbereda lokal VMM-servrar och Hyper-V-värdar att interagera med Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="12adf-104">After setting up [Azure components](vmm-to-azure-walkthrough-prepare-azure.md) for the deployment, use the instructions in this article to prepare on-premises VMM servers and Hyper-V hosts to interact with Azure Site Recovery.</span></span>

<span data-ttu-id="12adf-105">När du har läst den här artikeln efter eventuella kommentarer längst ned eller tekniska frågor om den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="12adf-105">After reading this article, post any comments at the bottom, or ask technical questions on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="prepare-vmm-servers"></a><span data-ttu-id="12adf-106">Förbereda VMM-servrar</span><span class="sxs-lookup"><span data-stu-id="12adf-106">Prepare VMM servers</span></span>

- <span data-ttu-id="12adf-107">Du behöver minst en VMM-server som uppfyller kraven för stöd för Site Recovery replikering (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).</span><span class="sxs-lookup"><span data-stu-id="12adf-107">You need at least one VMM server that meet the support requirements for Site Recovery replication (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).</span></span>
- <span data-ttu-id="12adf-108">Kontrollera att du har förberett VMM-server för [nätverksmappning](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span><span class="sxs-lookup"><span data-stu-id="12adf-108">Make sure you've prepared the VMM server for [network mapping](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span></span>
- <span data-ttu-id="12adf-109">Kontrollera att VMM-servern kan komma åt dessa URL: er</span><span class="sxs-lookup"><span data-stu-id="12adf-109">Make sure that the VMM server can access these URLs</span></span>

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- <span data-ttu-id="12adf-110">Om du har IP-adressbaserade brandväggsregler ontrollerar du att de tillåter kommunikation till Azure.</span><span class="sxs-lookup"><span data-stu-id="12adf-110">If you have IP address-based firewall rules, ensure they allow communication to Azure.</span></span>
- <span data-ttu-id="12adf-111">Tillåt [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-intervall för Azures datacenter) och HTTPS-port 443.</span><span class="sxs-lookup"><span data-stu-id="12adf-111">Allow the [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and the HTTPS (443) port.</span></span>
- <span data-ttu-id="12adf-112">Tillåt IP-adressintervall för Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och identitetshantering).</span><span class="sxs-lookup"><span data-stu-id="12adf-112">Allow IP address ranges for the Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

<span data-ttu-id="12adf-113">Under distributionen av Site Recovery hämta Site Recovery-providern och installera den på varje VMM-server.</span><span class="sxs-lookup"><span data-stu-id="12adf-113">During Site Recovery deployment, you download the Site Recovery Provider and install it on each VMM server.</span></span> <span data-ttu-id="12adf-114">VMM-servern registreras i Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="12adf-114">The VMM server is registered in the Recovery Services vault.</span></span>




## <a name="next-steps"></a><span data-ttu-id="12adf-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12adf-115">Next steps</span></span>

<span data-ttu-id="12adf-116">Gå till [steg 7: skapa ett valv](vmm-to-azure-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="12adf-116">Go to [Step 7: Create a vault](vmm-to-azure-walkthrough-create-vault.md)</span></span>

