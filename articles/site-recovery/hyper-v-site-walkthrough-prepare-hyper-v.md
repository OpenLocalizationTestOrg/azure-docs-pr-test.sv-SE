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
# <a name="step-6-prepare-hyper-v-hosts-for-replication-tooazure"></a>Steg 6: Förbereda Hyper-V-värdar för replikering tooAzure

Använd hello instruktioner i den här artikeln tooprepare lokala Hyper-V-värdar toointeract med Azure Site Recovery.

När du har läst den här artikeln efter eventuella kommentarer längst ned hello eller tekniska frågor om hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-hosts"></a>Förbered värdar

- Kontrollera att hello Hyper-V-värdar uppfyller hello [krav](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).
- Kontrollera att hello värdarna kan komma åt hello krävs URL: er:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- Om du har IP-adressbaserade brandväggsregler, se till att de tillåter kommunikation tooAzure.
- Tillåt hello [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653), och hello HTTPS (443)-port.
- Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och Identity Management).

Under distributionen av Site Recovery kan du lägga till Hyper-V-värdar som innehåller virtuella datorer du vill tooreplicate tooa Hyper-V-platsen. hello Site Recovery-Provider och Recovery Services-agenten installeras på varje värd. hello Hyper-V-platsen har registrerats i hello Recovery Services-valvet.

## <a name="next-steps"></a>Nästa steg

Gå för[steg 7: skapa ett valv](hyper-v-site-walkthrough-create-vault.md)

