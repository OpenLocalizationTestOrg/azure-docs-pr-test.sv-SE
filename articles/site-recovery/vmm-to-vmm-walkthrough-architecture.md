---
title: "aaaReview hello arkitekturen för Hyper-V-replikering tooa sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över hello arkitektur för att replikera lokala virtuella Hyper-V-datorer tooa sekundär System Center VMM plats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 07099161-4cc7-4f32-8eb9-2a71bbf0750b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 0de4b4e8601116c73e6fd710597ce4e561884368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-hyper-v-replication-tooa-secondary-site"></a>Steg 1: Granska hello arkitekturen för Hyper-V-replikering tooa sekundär plats

Den här artikeln beskriver hello komponenter och processer som är inblandade vid replikering av lokala Hyper-V virtuella datorer (VM) i System Center Virtual Machine Manager (VMM) moln tooa sekundär VMM-plats med hjälp av hello [Azure Site Recovery](site-recovery-overview.md)tjänsten i hello Azure-portalen.

Bokför eventuella kommentarer längst ned hello av den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="architectural-components"></a>Arkitekturkomponenter

Det här är vad du behöver för att replikera virtuella Hyper-V-datorer tooa sekundär VMM-plats.

**Komponent** | **Plats** | **Detaljer**
--- | --- | ---
**Azure** | Prenumeration på Azure. | Du skapar ett Recovery Services-valv i hello Azure-prenumeration, tooorchestrate och hantera replikering mellan platser i VMM.
**VMM-server** | Du behöver en primär och sekundär VMM-plats. | Vi rekommenderar en VMM-servern i hello primär plats och en i hello sekundär plats 
**Hyper-V-server** |  En eller flera Hyper-V-värdservrar i hello primära och sekundära VMM-moln. | Data replikeras mellan hello primära och sekundära Hyper-V-värdservrarna via hello LAN eller VPN med hjälp av Kerberos eller certifikatautentisering.  
**Virtuella Hyper-V-datorer** | På Hyper-V-värdservern. | hello källa värdservern bör ha minst en virtuell dator som du vill tooreplicate.

## <a name="replication-process"></a>Replikeringsprocessen

1. Ställ in hello Azure-konto, skapa ett Recovery Services-valv och ange vad du vill tooreplicate.
2. Du kan konfigurera hello käll- och replikeringsinställningarna, till exempel installera hello Azure Site Recovery-providern på VMM-servrar och hello Microsoft Azure Recovery Services-agenten på varje Hyper-V-värd.
3. Du kan skapa en replikeringsprincip för hello källan VMM-moln. hello principen är tillämpade tooall virtuella datorer på värdar i hello molnet.
4. Du aktiverar replikering för varje VMM och inledande replikering av en virtuell dator sker i enlighet med hello-inställningar som du väljer.
5. Efter den första replikeringen börjar replikeringen av deltaändringar. Spårade ändringar för ett objekt lagras i en .hrl-fil.


![Lokala tooon lokala](./media/vmm-to-vmm-walkthrough-architecture/arch-onprem-onprem.png)

## <a name="failover-and-failback-process"></a>Processen för redundans och återställning efter fel

1. Du kan köra en planerad eller oplanerad [redundansväxling](site-recovery-failover.md) mellan lokala platser. Om du kör en planerad redundans och virtuella källdatorer stängs tooensure inga data går förlorade.
2. Du kan växla över en enskild dator eller skapa [återställningsplaner](site-recovery-create-recovery-plans.md) tooorchestrate växling vid fel på flera datorer.
4. Om du vill utföra en oplanerad växling tooa sekundär plats, efter hello redundans datorer hello sekundär plats har inte aktiverats för skydd eller replikering. Om du körde en planerad redundans, efter hello redundans skydda datorer i hello sekundär plats.
5. Sedan kan du spara hello redundans toostart använder hello arbetsbelastning från hello replikerade virtuella datorn.
6. När den primära platsen är tillgänglig igen kan initiera du omvänd replikering tooreplicate från hello sekundär plats toohello primära. Omvänd replikering ger hello virtuella datorer i ett skyddat läge, men hello sekundärt datacenter är fortfarande aktiva hello-plats.
7. toomake hello primära platsen till aktiv plats för hello igen, kan du starta en planerad redundansväxling från sekundär tooprimary, följt av en annan omvänd replikering.



## <a name="next-steps"></a>Nästa steg

Gå för[steg 2: granska hello förutsättningar och begränsningar](vmm-to-vmm-walkthrough-prerequisites.md).
