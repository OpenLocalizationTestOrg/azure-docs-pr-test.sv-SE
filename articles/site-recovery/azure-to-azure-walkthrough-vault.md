---
title: "aaaSet upp ett valv för Azure VM repliction mellan regioner med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooset upp ett valv för Azure replikering mellan Azure-regioner med hjälp av Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 40472189-3d80-4963-b175-8bddcbc2f61f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: 9959c59c7ea57114763f13bf060404ddd267ba80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-set-up-a-vault-for-azure-tooazure-replication"></a>Steg 4: Konfigurera ett valv för Azure tooAzure replikering

Efter [planera nätverk](azure-to-azure-walkthrough-network.md), Använd den här artikeln tooset upp ett valv för Azure virtuella datorer (VM) som replikerar tooanother Azure-region med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

- När du är klar hello artikeln bör du ha ett Recovery Services-valv ställa in.
- Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



>[!NOTE]
>
> Azure VM-replikering är för närvarande under förhandsgranskning.




## <a name="create-a-vault"></a>Skapa ett valv

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> Vi rekommenderar att du skapar hello Recovery Services-valvet i hello plats där du vill att din tooreplicate för virtuella datorer. Till exempel om din målplatsen är hello central oss, skapa hello valv i **centrala USA**.


## <a name="next-steps"></a>Nästa steg

Gå för[steg 5: Aktivera replikering](azure-to-azure-walkthrough-enable-replication.md)
