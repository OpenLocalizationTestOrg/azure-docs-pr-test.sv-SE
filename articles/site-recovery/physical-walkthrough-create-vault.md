---
title: "aaaSet upp ett valv för fysisk server replication tooAzure med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooset in valvet tooreplicate fysiska servrar tooAzure med hjälp av Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99f2605-f417-4995-be77-5323136b814f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 988928e3ece31116823f132cc39223fe44443468
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-set-up-a-vault-for-physical-server-replication-tooazure"></a>Steg 6: Konfigurera ett valv för fysisk server replication tooAzure


Den här artikeln beskriver hur tooset upp ett valv. Du skapar hello valvet och ange vad du vill tooreplicate från din lokala plats-tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.


Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Välj en skyddsmål

Vad kan du välja tooreplicate, och där du vill tooreplicate till.

1. Klicka på **Recovery Services-valv** > valvet.
2. I hello resurs-menyn, klickar du på **Site Recovery** > **Förbered infrastrukturen** > **skyddsmål**.
3. I **skyddsmål**väljer **tooAzure** > **inte virtualiserade/andra**.


## <a name="next-steps"></a>Nästa steg

Gå för[steg 7: Konfigurera källa och mål](physical-walkthrough-source-target.md)
