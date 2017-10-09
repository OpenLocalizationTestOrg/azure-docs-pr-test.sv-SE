---
title: aaaReplicate virtuella VMware-datorer tooAzure med Azure Site Recovery | Microsoft Docs
description: "En översikt över hello steg för att replikera arbetsbelastningar som körs på virtuella VMware-datorer tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 7104f67a3635b916048dcb61bca770c89f0c77fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-vmware-vms-tooazure-with-site-recovery"></a>Replikera virtuella VMware-datorer tooAzure med Site Recovery

Den här artikeln innehåller en översikt över hello steg krävs tooreplicate lokal VMware virtuella datorer tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.


![Processen för distribution](./media/vmware-walkthrough-overview/vmware-to-azure-process.png)

**Bild 1: Process distributionssammanfattning**

## <a name="step-1-review-architecture-and-prerequisites"></a>Steg 1: Granska arkitektur och förutsättningar

Innan du börjar distributionen bör granska hello scenariots arkitektur och kontrollera att du förstår alla hello-komponenter som du behöver toodeploy

Gå för[steg 1: granska hello-arkitektur](vmware-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Steg 2: Granska krav

Kontrollera att du har hello krav för varje komponent i distributionen:

- **Krav för Azure**: du behöver ett Microsoft Azure-konto, Azure-nätverk och lagringskonton.
- **Site Recovery-komponenter på lokala**: du behöver en dator som körs lokalt Site Recovery-komponenter.
- **Lokal VMware krav**: du måste tooset konton så att Site Recovery kan komma åt VMware-servrar och virtuella datorer.
- **Replikerade virtuella datorer**: virtuella datorer du vill tooreplicate måste toocomply med krav för Azure och har hello mobilitetstjänsten installeras.

Gå för[steg 2: granska förutsättningar och begränsningar](vmware-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Steg 3: Planera kapacitet

Om du gör en fullständig distribution måste toofigure reda på vilka replikering resurser som du behöver. Det finns ett par av verktyg tillgängliga toohelp du göra detta. Gå tooStep 2. Om du gör en snabb konfigurera tootest hello miljö kan du hoppa över det här steget.

Gå för[steg3: Planera kapacitet](vmware-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>Steg 4: Planera nätverk

Du måste toodo vissa nätverk planera tooensure att virtuella Azure-datorer är anslutna toonetworks efter växling vid fel och att de har hello rätt IP-adresser.

Gå för[steg 4: Planera nätverk](vmware-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>Steg 5: Förbered Azure-resurser

Konfigurera Azure nätverken och lagringsenheterna innan du börjar. Du kan göra detta under distributionen, men vi rekommenderar att du gör detta innan du börjar.

Gå för[steg 5: Förbered Azure](vmware-walkthrough-prepare-azure.md)


## <a name="step-6-prepare-vmware"></a>Steg 6: Förbereda VMware

Du behöver tooset konton som Site Recovery för att:

- Åtkomst VMware virtualisering servrar tooautomatically identifiera virtuella datorer.
- Åtkomst till virtuella datorer tooinstall hello mobilitetstjänsten. Varje virtuell dator som du vill tooreplicate måste ha hello mobilitetstjänstagenten installerad innan du kan aktivera replikering för den.

Gå för[steg 6: förbereda VMware](vmware-walkthrough-prepare-vmware.md)

## <a name="step-7-set-up-a-vault"></a>Steg 7: Konfigurera ett valv

Du behöver tooset in tooorchestrate en Recovery Services-valvet och hantera replikering. När du ställer in hello valvet kan du ange vad du vill tooreplicate, och där du vill att tooreplicate den.

Gå för[steg 7: konfigurera ett valv](vmware-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>Steg 8: Konfigurera inställningar för källa och mål

Ställ in hello källa och mål som används för replikering. Konfigurera inställningar för datakälla innehåller och kör installationsprogrammet för enhetlig tooinstall hello lokalt Site Recovery-komponenter.

Gå för[steg 8: Ställ in hello källa och mål](vmware-walkthrough-source-target.md)

## <a name="step-9-set-up-a-replication-policy"></a>Steg 9: Konfigurera en replikeringsprincip

Du ställa in en princip toospecify replikeringsinställningarna för virtuella VMware-datorer i hello-valvet.

Gå för[steg 9: Konfigurera en replikeringsprincip](vmware-walkthrough-replication.md)

## <a name="step-10-install-hello-mobility-service"></a>Steg 10: Installera hello mobilitetstjänsten

Hej mobilitetstjänsten måste installeras på varje virtuell dator som du vill tooreplicate. Det finns ett par sätt tooset hello tjänsten med push- eller pull-installation.

Gå för[steg 10: Installera hello mobilitetstjänsten](vmware-walkthrough-install-mobility.md)

## <a name="step-11-enable-replication"></a>Steg 11: Aktivera replikering

När hello mobilitetstjänsten körs på en virtuell dator kan du aktivera replikering för den. När du har aktiverat inträffar inledande replikering av hello VM.

Gå för[steg 11: Aktivera replikering](vmware-walkthrough-enable-replication.md)

## <a name="step-12-run-a-test-failover"></a>Steg 12: Kör ett redundanstest

När replikeringen har slutförts och deltareplikering körs, kan du köra en testa redundans toomake att allt fungerar som förväntat.

Gå för[steg 12: kör ett redundanstest](vmware-walkthrough-test-failover.md)
