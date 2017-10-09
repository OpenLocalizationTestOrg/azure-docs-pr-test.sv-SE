---
title: "aaaReplicate Hyper-V VMs tooa sekundär VMM-plats med Azure Site Recovery | Microsoft Docs"
description: "Översikt för att replikera virtuella Hyper-V-datorer tooa sekundär VMM-plats med hjälp av hello Azure-portalen."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 476ca82a-8f5c-4498-9dcf-e1011d60ed59
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: raynew
ms.openlocfilehash: 90e44bfc2237dfa7646fb2b7b1e533a7f6d83c50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site"></a>Replikera virtuella Hyper-V-datorer i VMM-moln tooa sekundär VMM-plats

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Klassisk portal](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Den här artikeln innehåller en översikt över hello steg som krävs tooreplicate lokala Hyper-V virtuella datorer (VM) som hanteras i System Center Virtual Machine Manager (VMM) moln, tooa sekundära VMM-platsen med hjälp av [Azure Site Recovery](site-recovery-overview.md)i hello Azure-portalen.

När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="step-1-review-hello-scenario-architecture"></a>Steg 1: Granska hello scenariots arkitektur

Innan du börjar distributionen, granska hello scenariots arkitektur och se till att du förstår alla hello-komponenter måste toodeploy.

Gå för[steg 1: granska hello arkitektur](vmm-to-vmm-walkthrough-architecture.md).

## <a name="step-2-review-prerequisites-and-limitations"></a>Steg 2: Granska förutsättningar och begränsningar

Kontrollera att du förstår hello distributionskrav och begränsningar.

**Krav för Azure**: du behöver ett Microsoft Azure-prenumeration och Azure Recovery Services valvet, tooorchestrate och hantera replikering.
**Lokal VMM-servrar och Hyper-V-värdar**: Kontrollera att VMM-servrar och Hyper-V-värdar är kompatibla och förberedda för distributionen av Site Recovery.

Gå för[steg 2: Verifiera förutsättningar och begränsningar](vmm-to-vmm-walkthrough-prerequisites.md).

## <a name="step-3-plan-networking"></a>Steg 3: Planera nätverk

Du måste toodo vissa nätverk planera tooensure att du kan konfigurera nätverksmappning mellan VMM VM-nätverk när du distribuerar hello scenario.

Gå för[steg3: Planera nätverk](vmm-to-vmm-walkthrough-network.md).


## <a name="step-4-prepare-vmm-and-hyper-v"></a>Steg 4: Förbered VMM och Hyper-V

Förbered hello VMM-servrar och Hyper-V-värdar för distributionen av Site Recovery.

Gå för[steg 4: förbereda lokala servrar](vmm-to-vmm-walkthrough-vmm-hyper-v.md).

## <a name="step-5-set-up-a-vault"></a>Steg 5: Konfigurera ett valv

Skapa ett Recovery Services-valvet. hello valvet innehåller konfigurationsinställningar och styr replikeringen.

[Steg 5: Konfigurera ett valv](vmm-to-vmm-walkthrough-create-vault.md).

## <a name="step-6-set-up-source-and-target-settings"></a>Steg 6: Konfigurera inställningar för källa och mål

Ställ in hello käll- och replikering VMM-platser. Lägg till hello VMM-servrar toohello valvet och ladda ned hello installationsfilerna för Site Recovery-komponenter. Kör installationsprogrammet för Azure Site Recovery Provider på hello VMM-servern. Installationsprogrammet installerar hello providern på hello VMM-servern och registrerar hello-server i hello-valvet. Hello Microsoft Recovery Services-agenten installeras på varje Hyper-V-värd.

Gå för[steg 6: skapa hello källa och mål för](vmm-to-vmm-walkthrough-source-target.md).

## <a name="step-7-configure-network-mapping"></a>Steg 7: Konfigurera nätverksmappning

Mappa VMM VM-nätverk i hello käll- och målplatserna. Efter växling vid fel skapas virtuella datorer i hello målnätverket att maps toohello källnätverket vilka hello Hyper-V Virtuella källdatorn finns.

Gå för[steg 7: Konfigurera nätverksmappning](vmm-to-vmm-walkthrough-network-mapping.md).


## <a name="step-8-set-up-a-replication-policy"></a>Steg 8: Skapa en replikeringsprincip

Ange hur virtuella datorer kommer att replikeras mellan VMM-platser.

Gå för[steg 8: skapa en replikeringsprincip](vmm-to-vmm-walkthrough-replication.md).


## <a name="step-9-enable-replication-for-vms"></a>Steg 9: Aktivera replikering för virtuella datorer

Välj hello virtuella datorer du vill tooreplicate. Aktivera en virtuell dator för replikering utlösare hello inledande replikering toohello sekundär plats, följt av en pågående deltareplikering.

Gå för[steg 9: Aktivera replikering](vmm-to-vmm-walkthrough-enable-replication.md).


## <a name="step-10-run-a-test-failover"></a>Steg 10: Köra ett redundanstest

Kör ett test redundans toomake att allt fungerar som förväntat.

Gå för[steg 10: köra ett redundanstest](vmm-to-vmm-walkthrough-test-failover.md).
