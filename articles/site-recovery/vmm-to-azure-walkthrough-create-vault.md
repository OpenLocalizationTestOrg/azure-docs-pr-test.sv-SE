---
title: "aaaSet upp ett valv för Hyper-V-replikering (med System Center VMM) tooAzure med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooset upp ett valv för Hyper-V-replikering (med VMM) tooAzure med hjälp av Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: b3cd6f03-c33c-406d-91d4-5cba69f79abd
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: f2c90f3c8b0a48db1e57fefd9829d29cffff8d43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a>Steg 7: Konfigurera ett valv för Hyper-V-replikering

Den här artikeln beskriver hur tooset upp ett valv och ange vad du vill tooreplicate från din lokala plats, tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.


Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]



## <a name="select-a-protection-goal"></a>Välj en skyddsmål

Vad kan du välja tooreplicate, och där du vill tooreplicate till.

1. Klicka på **Recovery Services-valv** > valvet.
2. I hello resurs-menyn, klickar du på **Site Recovery** > **Förbered infrastrukturen** > **skyddsmål**.
3. I **skyddsmål**väljer **tooAzure** > **Ja, med Hyper-V**. Välj **Ja** tooconfirm du är nusing VMM. 

     ![Välja mål](./media/vmm-to-azure-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a>Nästa steg

Gå för[steg 8: Ställ in källa och mål](vmm-to-azure-walkthrough-source-target.md)
