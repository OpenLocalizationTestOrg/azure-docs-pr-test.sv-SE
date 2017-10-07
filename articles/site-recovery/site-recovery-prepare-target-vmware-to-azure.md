---
title: "Förbereda mål (VMware tooAzure) | Microsoft Docs"
description: "Den här artikeln beskriver hur tooprepare din Azure-miljön toostart replikera VMware tooAzure för virtuella datorer."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhemraj
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 5/31/2017
ms.author: bsiva
ms.openlocfilehash: 5975d3c122032f92f8df370ee74fa0c7012ebe2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-target-vmware-tooazure"></a>Förbereda mål (VMware tooAzure)
> [!div class="op_single_selector"]
> * [VMware tooAzure](./site-recovery-prepare-target-vmware-to-azure.md)
> * [Fysisk tooAzure](./site-recovery-prepare-target-physical-to-azure.md)

Den här artikeln beskriver hur tooprepare din Azure-miljön toostart replikera VMware tooAzure för virtuella datorer.

## <a name="prerequisites"></a>Krav

hello förutsätter hello följande:
- Du har skapat en Återställningstjänstvalvet tooprotect VMware-datorer. Du kan skapa ett Recovery Services-valv från hello [Azure-portalen](http://portal.azure.com "Azure-portalen").
- Du har [konfigurering av din lokala miljö](./site-recovery-set-up-vmware-to-azure.md) tooreplicate VMware virtuella datorer tooAzure.

## <a name="prepare-target"></a>Förbereda mål

När du har slutfört hello **steg 1: Välj skyddsmål** och **steg 2: Förbered källa**, tas du för**steg3: mål**

![Förbereda mål](./media/site-recovery-prepare-target-vmware-to-azure/prepare-target-vmware-to-azure.png)

1. **Prenumerationen:** från hello se en meny, Välj hello prenumeration som du vill tooreplicate dina virtuella datorer till.
2. **Distributionsmodell:** väljer hello distributionsmodell (klassiska eller Resource Manager)

Baserat på hello valt distributionsmodell utförs en verifiering tooensure som du har minst en kompatibel storage-konto och virtuella nätverk i hello mål prenumeration tooreplicate och växling vid fel den virtuella datorn till.

Klicka på OK toogo toohello nästa steg när hello verifieringar kan slutföras.

Om du inte har en kompatibel Resource Manager storage-konto eller ett virtuellt nätverk eller vill tooadd mer, kan du göra det genom att klicka på hello **+ Lagringskonto** eller **+ nätverk** knappar på hello överkant hello bladet.

## <a name="next-steps"></a>Nästa steg
[Konfigurera replikeringsinställningar](./site-recovery-setup-replication-settings-vmware.md).
