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
# <a name="step-2-review-hello-prerequisites-for-hyper-v-without-vmm-tooazure-replication"></a>Steg 2: Granska hello krav för Hyper-V (utan VMM) tooAzure-replikering

hello kraven sammanfattas i hello tabell.


**Krav** | **Detaljer** 
--- | --- 
**Azure** | Lär dig om [Azure-krav](site-recovery-prereq.md#azure-requirements).
**Lokala servrar** | [Lär dig mer](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm) om kraven för hello lokala Hyper-V-värdar.
**Lokala virtuella Hyper-V-datorer** | Virtuella datorer du vill ska köra tooreplicate en [operativsystem som stöds](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), och överensstämma med [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Azure-URL: er** | Hyper-V-värdar behöver komma åt toothese URL: er:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Om du har IP-adressbaserade brandväggsregler, se till att de tillåter kommunikation tooAzure.<br/></br> Tillåt hello [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653), och hello HTTPS (443)-port.<br/></br> Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och Identity Management).



## <a name="next-steps"></a>Nästa steg

- Om du utför en fullständig distribution går för[steg3: Planera kapacitet](hyper-v-site-walkthrough-capacity.md)
- Om du utför en enkel testdistributionen gå för[steg 4: Planera nätverk](hyper-v-site-walkthrough-network.md).
