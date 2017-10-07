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
# <a name="step-6-prepare-vmm-servers-and-hyper-v-hosts-for-hyper-v-replication-tooazure"></a>Steg 6: Förbereda VMM-servrar och Hyper-V-värdar för tooAzure för Hyper-V-replikering

När du har installerat [Azure komponenter](vmm-to-azure-walkthrough-prepare-azure.md) hello distribution kan använda hello instruktionerna i den här artikeln tooprepare lokal VMM-servrar och Hyper-V-värdar toointeract med Azure Site Recovery.

När du har läst den här artikeln efter eventuella kommentarer längst ned hello eller tekniska frågor om hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-vmm-servers"></a>Förbereda VMM-servrar

- Du behöver minst en VMM-server som uppfyller hello stöd för Site Recovery replikering (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
- Kontrollera att du har skapat hello VMM-server för [nätverksmappning](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).
- Se till att hello VMM-servern kan komma åt dessa URL: er

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- Om du har IP-adressbaserade brandväggsregler, se till att de tillåter kommunikation tooAzure.
- Tillåt hello [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653), och hello HTTPS (443)-port.
- Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och Identity Management).

Under distributionen av Site Recovery hämta hello Site Recovery-providern och installera den på varje VMM-server. hello VMM-servern registreras i hello Recovery Services-valvet.




## <a name="next-steps"></a>Nästa steg

Gå för[steg 7: skapa ett valv](vmm-to-azure-walkthrough-create-vault.md)

