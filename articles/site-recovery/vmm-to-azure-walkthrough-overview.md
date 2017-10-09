---
title: aaaReplicate Hyper-V virtuella datorer i VMM-moln tooAzure med Azure Site Recovery | Microsoft Docs
description: "Ger en översikt för att replikera virtuella Hyper-V-datorer i VMM-moln tooAzure hello Azure Site Recovery-tjänsten"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: d6f729a49cc86ea07bebc4d7266fd7b58b3998f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure-using-site-recovery-in-hello-azure-portal"></a>Replikera virtuella Hyper-V-datorer i VMM-moln tooAzure med Site Recovery i hello Azure-portalen
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [Klassiska Azure](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [PowerShell – Klassisk](site-recovery-deploy-with-powershell.md)


Den här artikeln innehåller en översikt över hello steg krävs tooreplicate lokala Hyper-V virtuella datorer (VM) hanteras i System Center Virtual Machine Manager (VMM) moln tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="step-1-review-hello-scenario-architecture"></a>Steg 1: Granska hello scenariots arkitektur

Innan du börjar distributionen, granska hello scenariots arkitektur och se till att du förstår alla hello-komponenter måste toodeploy.

Gå för[steg 1: granska hello-arkitektur](vmm-to-azure-walkthrough-architecture.md)

## <a name="step-2-review-prerequisites-and-limitations"></a>Steg 2: Granska förutsättningar och begränsningar

Kontrollera att du förstår hello distributionskrav och begränsningar.

**Krav för Azure**: du behöver ett Microsoft Azure-konto, Azure-nätverk och lagringskonton.
**Lokal VMM-servrar och Hyper-V-värdar**: Kontrollera att VMM-servrar och Hyper-V-värdar är kompatibla och förberedda för distributionen av Site Recovery.
**Replikerade virtuella datorer**: Kontrollera att virtuella datorer du vill tooreplicate uppfyller kraven för Azure.

Gå för[steg 2: Verifiera förutsättningar och begränsningar](vmm-to-azure-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Steg 3: Planera kapacitet

Om du utför en fullständig distribution måste toofigure reda på vilka replikering resurser som du behöver. Det finns ett par av verktyg tillgängliga toohelp du göra detta. Om du utför en snabb konfigurera tootest hello miljö kan du hoppa över det här steget.

Gå för[steg3: Planera kapacitet](vmm-to-azure-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>Steg 4: Planera nätverk

Du måste toodo vissa nätverk planera tooensure som du kan konfigurera nätverksmappning när du distribuerar hello scenariot virtuella Azure-datorer kommer att anslutna tooAzure virtuella nätverk efter växling vid fel och som att de är tilldelade lämplig IP-adresser.

Gå för[steg 4: Planera nätverk](vmm-to-azure-walkthrough-network.md)


## <a name="step-5-prepare-azure-resources"></a>Steg 5: Förbered Azure-resurser

Skapa ett Azure-konto, nätverk och lagring. Du kan göra detta under distributionen, men vi rekommenderar att du gör detta innan du börjar.

Gå för[steg 5: Förbered Azure](vmm-to-azure-walkthrough-prepare-azure.md)

## <a name="step-6-prepare-vmm-and-hyper-v"></a>Steg 6: Förbereda VMM och Hyper-V

Förbered hello lokal VMM-servrar och Hyper-V-värdar för distributionen av Site Recovery.

Gå för[steg 6: förbereda lokala servrar](vmm-to-azure-walkthrough-vmm-hyper-v.md)

## <a name="step-7-set-up-a-vault"></a>Steg 7: Konfigurera ett valv

Skapa ett Recovery Services-valvet. hello valvet innehåller konfigurationsinställningar och styr replikeringen.

[Steg 7: Konfigurera ett valv](vmm-to-azure-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>Steg 8: Konfigurera inställningar för källa och mål

Ställ in hello käll- och målplatserna för replikering. Lägg till hello VMM server toohello valvet och ladda ned hello installationsfilerna för Site Recovery-komponenter. Kör installationsprogrammet för Azure Site Recovery Provider på hello VMM-servern. Installationsprogrammet installerar hello providern på hello VMM-servern och registrerar hello-server i hello-valvet. Hello Microsoft Recovery Services-agenten installeras på varje Hyper-V-värd.

Gå för[steg 8: Konfigurera inställningar för källa och mål](vmm-to-azure-walkthrough-source-target.md)

## <a name="step-9-configure-network-mapping"></a>Steg 9: Konfigurera nätverksmappning

Mappa en lokal VMM VM-nätverk tooAzure virtuella nätverk. Efter växling vid fel skapas virtuella Azure-datorer i hello Azure-nätverk som mappar toohello lokala Virtuella nätverk i vilka hello datakälla Hyper-V finns.

Gå för[steg 9: Konfigurera nätverksmappning](vmm-to-azure-walkthrough-network-mapping.md)


## <a name="step-10-set-up-a-replication-policy"></a>Steg 10: Ställ in en replikeringsprincip

Ange hur lokala virtuella datorer kommer att replikerade tooAzure.

Gå för[steg 10: Konfigurera en replikeringsprincip](vmm-to-azure-walkthrough-replication.md)


## <a name="step-11-enable-replication-for-vms"></a>Steg 11: Aktivera replikering för virtuella datorer

Välj hello virtuella datorer du vill tooreplicate. Aktivera en virtuell dator för replikering utlösare hello inledande replikering tooAzure följt av en pågående deltareplikering.

Gå för[steg 11: Aktivera replikering](vmm-to-azure-walkthrough-enable-replication.md)


## <a name="step-12-run-a-test-failover"></a>Steg 12: Kör ett redundanstest

Kör ett test redundans toomake att allt fungerar som förväntat.

Gå för[steg 12: kör ett redundanstest](vmm-to-azure-walkthrough-test-failover.md)


