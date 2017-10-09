---
title: "aaaSet upp ett valv för VMware replikering tooAzure med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooset upp ett valv för VMware replikering tooAzure med hjälp av Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8bce940e-f19f-4418-8360-aee7b073519a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 8a7755a6c9a3f55f241c615e425285bc4b782493
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-a-vault-for-vmware-replication-tooazure"></a>Steg 7: Konfigurera ett valv för tooAzure för VMware-replikering


Den här artikeln beskriver hur tooset upp ett valv och ange vad du vill tooreplicate från din lokala plats, tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.


Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Välj en skyddsmål

Vad kan du välja tooreplicate, och där du vill tooreplicate till.

1. Klicka på **Recovery Services-valv** > valvet.
2. I hello resurs-menyn, klickar du på **Site Recovery** > **Förbered infrastrukturen** > **skyddsmål**.
3. I **skyddsmål**väljer **tooAzure** > **Ja, med VMware vSphere-Hypervisor**.



## <a name="next-steps"></a>Nästa steg

Gå för[steg 8: Ställ in källa och mål](vmware-walkthrough-source-target.md)
