---
title: aaaReplicate Hyper-V VMs tooAzure med Azure Site Recovery | Microsoft Docs
description: "Beskriver hur tooorchestrate replikering, redundans och återställning av lokala Hyper-V VMs tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: efddd986-bc13-4a1d-932d-5484cdc7ad8d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: ab9cd14149ef32a416428d0f4327aa18b042e9c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-without-vmm-tooazure"></a>Replikera tooAzure för Hyper-V-virtuella datorer (utan VMM) 

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [Klassiska Azure](site-recovery-hyper-v-site-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

Den här artikeln innehåller en översikt över hello steg som krävs för tooreplicate lokala Hyper-V virtuella datorer tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) i hello Azure-portalen. I den här distributionen Hyper-V virtuella datorer hanteras inte av System Center Virtual Machine Manager (VMM).


När du har läst den här artikeln efter eventuella kommentarer längst ned hello eller tekniska frågor om hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="step-1-review-architecture-and-prerequisites"></a>Steg 1: Granska arkitektur och förutsättningar

Innan du börjar distributionen bör granska hello scenariots arkitektur och kontrollera att du förstår alla hello-komponenter som du behöver toodeploy

Gå för[steg 1: granska hello-arkitektur](hyper-v-site-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Steg 2: Granska krav

Kontrollera att du har hello krav för varje komponent i distributionen:

- **Krav för Azure**: du behöver ett Microsoft Azure-konto, Azure-nätverk och lagringskonton.
- **Lokal Hyper-V-krav**: Kontrollera att Hyper-V-värdar är förberedda för distributionen av Site Recovery.
- **Replikerade virtuella datorer**: virtuella datorer du vill tooreplicate måste toocomply med krav för Azure.

Gå för[steg 2: Verifiera förutsättningar och begränsningar](hyper-v-site-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Steg 3: Planera kapacitet

Om du gör en fullständig distribution måste toofigure reda på vilka replikering resurser som du behöver. Det finns ett par av verktyg tillgängliga toohelp du göra detta. Gå tooStep 2. Om du gör en snabb konfigurera tootest hello miljö kan du hoppa över det här steget.

Gå för[steg3: Planera kapacitet](hyper-v-site-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>Steg 4: Planera nätverk

Du måste toodo vissa nätverk planera tooensure att virtuella Azure-datorer är anslutna toonetworks efter växling vid fel och att de har hello rätt IP-adresser.

Gå för[steg 4: Planera nätverk](hyper-v-site-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>Steg 5: Förbered Azure-resurser

Konfigurera Azure nätverken och lagringsenheterna innan du börjar. Du kan göra detta under distributionen, men vi rekommenderar att du gör detta innan du börjar.

Gå för[steg 5: Förbered Azure](hyper-v-site-walkthrough-prepare-azure.md)


## <a name="step-6-prepare-hyper-v"></a>Steg 6: Förbereda Hyper-V

Kontrollera att Hyper-V-servrar uppfyller kraven för Site Recovery-distribution.

Gå för[steg 6: förbereda Hyper-V](hyper-v-site-walkthrough-prepare-hyper-v.md)

## <a name="step-7-set-up-a-vault"></a>Steg 7: Konfigurera ett valv

Du behöver tooset in tooorchestrate en Recovery Services-valvet och hantera replikering. När du ställer in hello valvet kan du ange vad du vill tooreplicate, och där du vill att tooreplicate den.

Gå för[steg 7: skapa ett valv](hyper-v-site-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>Steg 8: Konfigurera inställningar för källa och mål

Ställ in hello källa och mål som används för replikering. Konfigurera inställningar för datakälla innehåller lägger till Hyper-V-värdar tooa Hyper-V plats installerar hello Site Recovery-Provider och Recovery Services-agenten på varje Hyper-V-värd och registrera hello plats i hello Recovery Services-valvet.

Gå för[steg 8: Ställ in hello källa och mål](hyper-v-site-walkthrough-source-target.md)

## <a name="step-9-set-up-a-replication-policy"></a>Steg 9: Konfigurera en replikeringsprincip

Du ställa in en princip toospecify replikeringsinställningar för Hyper-V virtuella datorer i hello-valvet.

Gå för[steg 9: Konfigurera en replikeringsprincip](hyper-v-site-walkthrough-replication.md)


## <a name="step-10-enable-replication"></a>Steg 10: Aktivera replikering

När du har en replikeringsprincip för på plats när du har aktiverat utförs inledande replikering av hello VM.

Gå för[steg 10: Aktivera replikering](hyper-v-site-walkthrough-enable-replication.md)

## <a name="step-11-run-a-test-failover"></a>Steg 11: Kör ett redundanstest

När replikeringen har slutförts och deltareplikering körs, kan du köra en testa redundans toomake att allt fungerar som förväntat.

Gå för[steg 11: kör ett redundanstest](hyper-v-site-walkthrough-test-failover.md)
