---
title: aaaReplicate fysiska lokala servrar tooAzure med Azure Site Recovery | Microsoft Docs
description: "Ger en översikt över hello steg för att replikera arbetsbelastningar som körs på lokala Windows-/ Linux fysiska servrar tooAzure med hello Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 20122f01-929a-4675-b85b-a9b99d2618bc
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: f801b9544072d4029ec06cc1abfd4ff370e852e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-physical-servers-tooazure-with-site-recovery"></a>Replikera fysiska servrar tooAzure med Site Recovery

Den här artikeln innehåller en översikt över hello steg krävs tooreplicate lokala Windows-/ Linux fysiska servrar tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.


## <a name="step-1-review-architecture-and-prerequisites"></a>Steg 1: Granska arkitektur och förutsättningar

Innan du börjar distributionen bör granska hello scenariots arkitektur och kontrollera att du förstår alla hello-komponenter som du behöver tooset upp hello-distribution.

Gå för[steg 1: granska hello-arkitektur](physical-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Steg 2: Granska krav

Kontrollera att du har hello krav för varje komponent i distributionen:

- **Krav för Azure**: du behöver ett Microsoft Azure-konto, Azure-nätverk och lagringskonton.
- **Site Recovery-komponenter på lokala**: du behöver en dator som körs lokalt Site Recovery-komponenter.
- **Replikerade datorer**: servrar som du vill tooreplicate måste toocomply med lokala och krav för Azure.

Gå för[steg 2: granska förutsättningar och begränsningar](physical-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Steg 3: Planera kapacitet

Om du gör en fullständig distribution måste toofigure reda på vilka replikering resurser som du behöver. Om du utför en snabb konfigurera tootest hello miljö kan du hoppa över det här steget.

Gå för[steg3: Planera kapacitet](physical-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>Steg 4: Planera nätverk

Du måste toodo vissa nätverk planera tooensure att virtuella Azure-datorer är anslutna toonetworks efter växling vid fel och att de har hello rätt IP-adresser.

Gå för[steg 4: Planera nätverk](physical-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>Steg 5: Förbered Azure-resurser

Konfigurera Azure nätverken och lagringsenheterna innan du börjar. 

Gå för[steg 5: Förbered Azure](physical-walkthrough-prepare-azure.md)


## <a name="step-6-set-up-a-vault"></a>Steg 6: Konfigurera ett valv

Du ställer in en tooorchestrate för Recovery Services-valvet och hantera replikering. När du ställer in hello valvet kan du ange vad du vill tooreplicate, och där du vill att tooreplicate den.

Gå för[steg 6: konfigurera ett valv](physical-walkthrough-create-vault.md)

## <a name="step-7-configure-source-and-target-settings"></a>Steg 7: Konfigurera inställningar för källa och mål

Konfigurera inställningar för hello källa och mål (Azure) plats. Inställningar för datakälla innehåller och kör installationsprogrammet för enhetlig tooinstall hello lokalt Site Recovery-komponenter.

Gå för[steg 7: Konfigurera hello källa och mål](physical-walkthrough-source-target.md)

## <a name="step-8-set-up-a-replication-policy"></a>Steg 8: Skapa en replikeringsprincip

Du konfigurerar en princip toospecify hur fysiska servrar som ska replikeras.

Gå för[steg 8: skapa en replikeringsprincip](physical-walkthrough-replication.md)

## <a name="step-9-install-hello-mobility-service"></a>Steg 9: Installera hello mobilitetstjänsten

Hej mobilitetstjänsten måste installeras på varje server som du vill tooreplicate. Det finns ett par sätt tooset hello tjänsten med push- eller pull-installation.

Gå för[steg 9: Installera hello mobilitetstjänsten](physical-walkthrough-install-mobility.md)

## <a name="step-10-enable-replication"></a>Steg 10: Aktivera replikering

När hello mobilitetstjänsten körs på en server, kan du aktivera replikering för den. När du har aktiverat inträffar inledande replikering av hello VM.

Gå för[steg 10: Aktivera replikering](physical-walkthrough-enable-replication.md)

## <a name="step-11-run-a-test-failover"></a>Steg 11: Kör ett redundanstest

När replikeringen har slutförts och deltareplikering körs, kan du köra en testa redundans toomake att allt fungerar som förväntat.

Gå för[steg 11: kör ett redundanstest](physical-walkthrough-test-failover.md)

