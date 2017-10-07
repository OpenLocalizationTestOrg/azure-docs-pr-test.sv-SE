---
title: aaaReplicate Azure virtuella datorer mellan Azure-regioner | Microsoft Docs
description: "Sammanfattar hello steg krävs tooreplicate Azure virtuella datorer mellan Azure-regioner med hello Azure Site Recovery-tjänsten i hello Azure-portalen"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 6dd36239-4363-4538-bf80-a18e71b8ec67
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: f4ac386f040945131f8a10f02143437f4441e298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a>Replikera virtuella Azure-datorer mellan regioner med Azure Site Recovery

>Den här artikeln innehåller en översikt över hello steg krävs tooreplicate Azure virtuella datorer (VM) i en Azure-region tooAzure virtuella datorer i en annan region. 

>[!NOTE]
>
> Azure VM-replikering är för närvarande under förhandsgranskning.

Publicera kommentarer och frågor längst ned hello av den här artikeln eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="step-1-review-architecture"></a>Steg 1: Granska arkitektur

Innan du börjar distributionen bör du granska hello scenariots arkitektur och hello-komponenter som du behöver toodeploy.

Gå för[steg 1: granska hello-arkitektur](azure-to-azure-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Steg 2: Granska krav

Kontrollera att du har hello Azure-kraven på platser, inklusive en prenumeration, virtuella nätverk, storage-konton och VM-krav.

Gå för[steg 2: Verifiera förutsättningar och begränsningar](azure-to-azure-walkthrough-prerequisites.md)


## <a name="step-3-plan-networking"></a>Steg 3: Planera nätverk

Kontrollera att utgående anslutning har ställts in på Azure virtuella datorer du vill tooreplicate, och att anslutningar från lokala har ställts in.

Gå för[steg 4: Planera nätverk](azure-to-azure-walkthrough-network.md)



## <a name="step-4-create-a-vault"></a>Steg 4: Skapa ett valv 

Du behöver tooset upp en Recovery Services-valvet tooorchestrate hantera replikering och ange hello källa region.

Gå för[steg 4: skapa ett valv](azure-to-azure-walkthrough-vault.md)


## <a name="step-5-enable-replication"></a>Steg 5: Aktivera replikering


tooenable replikering måste du konfigurera platsen Målinställningar, ställer in en replikeringsprincip och väljer hello Azure virtuella datorer som du vill tooreplicate. När du har aktiverat inträffar inledande replikering av hello VM.

Gå för[steg 5: Aktivera replikering](azure-to-azure-walkthrough-enable-replication.md)


## <a name="step-6-run-a-test-failover"></a>Steg 6: Köra ett redundanstest

När replikeringen har slutförts och deltareplikering körs, kan du köra en testa redundans toomake att allt fungerar som förväntat.

Gå för[steg 6: köra ett redundanstest](azure-to-azure-walkthrough-test-failover.md)


