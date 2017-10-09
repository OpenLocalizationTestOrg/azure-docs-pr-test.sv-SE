---
title: "aaaCreate ett valv för Hyper-V-replikering tooa sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur toocreate ett valv vid replikering av Hyper-V VMs tooa sekundär System Center VMM webbplats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ff65dbfb-cb26-410e-ab48-76971625db08
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 96ee09cbf2376a5089b9efa09dc7ab3fb7d472cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-create-a-vault-for-hyper-v-replication-tooa-secondary-site"></a>Steg 5: Skapa ett valv för Hyper-V-replikering tooa sekundär plats

När du har förberett lokalt [System Center Virtual Machine Manager (VMM) servrar och Hyper-V-värdar/kluster](vmm-to-vmm-walkthrough-vmm-hyper-v.md) för Hyper-V replikering tooa sekundär plats med hjälp av [Azure Site Recovery](site-recovery-overview.md), kan du skapa en Recovery Services-valvet och välj hello replikering scenario.

När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a>Välj en skyddsmål

Välj vad du vill att tooreplicate och där du vill att tooreplicate till.

1. Klicka på **Site Recovery** > **steg 1: Förbered infrastrukturen** > **skyddsmål**.
2. Välj **toorecovery plats**, och välj **Ja, med Hyper-V**.
3. Välj **Ja** tooindicate som du använder VMM toomanage hello Hyper-V-värdar.
4. Välj **Ja** om du har en sekundär VMM-server. Om du distribuerar replikering mellan moln på en VMM-server, klickar du på **nr**. Klicka sedan på **OK**.

    ![Välja mål](./media/vmm-to-vmm-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a>Nästa steg

Gå för[steg 6: Konfigurera hello replikering käll- och](vmm-to-vmm-walkthrough-source-target.md).
