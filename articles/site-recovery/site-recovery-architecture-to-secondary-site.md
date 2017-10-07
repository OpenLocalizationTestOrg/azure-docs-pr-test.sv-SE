---
title: "aaaHow utför lokal dator replikering tooa sekundär lokal plats arbetet i Azure Site Recovery? | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över komponenter och arkitektur som används när replikera lokala virtuella datorer och fysiska servrar tooa sekundär plats med hello Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: raynew
ms.openlocfilehash: 097a3f43446fec69ed7f9e0b7f11e8d11f41cc6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-on-premises-machine-replication-tooa-secondary-site-work-in-site-recovery"></a>Hur datorn replikering tooa sekundär plats fungerar i Site Recovery i lokalt?

Den här artikeln beskriver hello komponenter och processer som är inblandade vid replikering av lokala virtuella datorer och fysiska servrar tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) service.

Du kan replikera hello följande tooa sekundär lokal plats:
- Lokala virtuella Hyper-V-datorer, virtuella Hyper-V-datorer på Hyper-V-kluster och fristående värdar som hanteras i System Center Virtual Machine Manager (VMM)-moln.
- Lokala virtuella VMware-datorer och fysiska Windows- och Linux-servrar. I det här scenariot hanteras replikeringen av Scout.

Bokför eventuella kommentarer längst ned hello av den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="replicate-hyper-v-vms-tooa-secondary-on-premises-site"></a>Replikera virtuella Hyper-V-datorer tooa sekundär lokal plats


### <a name="architectural-components"></a>Arkitekturkomponenter

Det här är vad du behöver för att replikera virtuella Hyper-V-datorer tooa sekundär plats.

**Komponent** | **Plats** | **Detaljer**
--- | --- | ---
**Azure** | Du behöver ett konto hos Microsoft. |
**VMM-server** | Vi rekommenderar en VMM-servern i hello primär plats och en i hello sekundär plats | Varje VMM-servern ska vara anslutna toohello internet.<br/><br/> Varje server bör ha minst ett privat VMM-moln, med hello Hyper-V kapaciteten för profilen.<br/><br/> Du installerar hello Azure Site Recovery Provider på hello VMM-servern. hello providern samordnar och styr replikeringen med hello Site Recovery-tjänsten via hello internet. Kommunikation mellan hello providern och Azure är säker och krypterad.
**Hyper-V-server** |  En eller flera Hyper-V-värdservrar i hello primära och sekundära VMM-moln.<br/><br/> Servrar som ska vara anslutna toohello internet.<br/><br/> Data replikeras mellan hello primära och sekundära Hyper-V-värdservrarna via hello LAN eller VPN med hjälp av Kerberos eller certifikatautentisering.  
**Virtuella Hyper-V-datorer** | Finns på hello källservern Hyper-V-värd. | hello källa värdservern bör ha minst en virtuell dator som du vill tooreplicate.

### <a name="replication-process"></a>Replikeringsprocessen

1. Du kan konfigurera hello Azure-konto.
2. Du skapar ett Replication Services-valv för Site Recovery och konfigurerar valvinställningar, inklusive:

    - hello replikeringskälla och mål (primära och sekundära platser).
    - Installation av hello Azure Site Recovery-providern och hello Microsoft Azure Recovery Services-agenten. hello providern är installerad på VMM-servrar och hello-agenten på varje Hyper-V-värd.
    - Du skapar en replikeringsprincip för VMM-källmolnet. hello principen är tillämpade tooall virtuella datorer på värdar i hello molnet.
    - Du aktiverar replikering för virtuella Hyper-V-datorer. Den inledande replikeringen sker i enlighet med hello replikeringsprincipens inställningar.
4. Dataändringar spåras och replikering av delta ändrar toobegins när hello replikeringen har slutförts. Spårade ändringar för ett objekt lagras i en .hrl-fil.
5. Du kör ett test redundans toomake att allt fungerar.

**Bild 1: VMM tooVMM replikering**

![Lokala tooon lokala](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback-process"></a>Processen för redundans och återställning efter fel

1. Du kan köra en planerad eller oplanerad [redundansväxling](site-recovery-failover.md) mellan lokala platser. Om du kör en planerad redundans och virtuella källdatorer stängs tooensure inga data går förlorade.
2. Du kan växla över en enskild dator eller skapa [återställningsplaner](site-recovery-create-recovery-plans.md) tooorchestrate växling vid fel på flera datorer.
4. Om du vill utföra en oplanerad växling tooa sekundär plats, efter hello redundans datorer hello sekundär plats har inte aktiverats för skydd eller replikering. Om du körde en planerad redundans, efter hello redundans skydda datorer i hello sekundär plats.
5. Sedan kan du spara hello redundans toostart använder hello arbetsbelastning från hello replikerade virtuella datorn.
6. När den primära platsen är tillgänglig igen kan initiera du omvänd replikering tooreplicate från hello sekundär plats toohello primära. Omvänd replikering ger hello virtuella datorer i ett skyddat läge, men hello sekundärt datacenter är fortfarande aktiva hello-plats.
7. toomake hello primära platsen till aktiv plats för hello igen, kan du starta en planerad redundansväxling från sekundär tooprimary, följt av en annan omvänd replikering.




## <a name="replicate-vmware-vmsphysical-servers-tooa-secondary-site"></a>Replikera VMware virtuella datorer/fysiska servrar tooa sekundär plats

Du kan replikera virtuella VMware-datorer eller fysiska servrar tooa sekundär plats med InMage Scout med hjälp av komponenterna arkitektur:


### <a name="architectural-components"></a>Arkitekturkomponenter

**Komponent** | **Plats** | **Detaljer**
--- | --- | ---
**Azure** | InMage Scout. | tooobtain InMage Scout som du behöver en Azure-prenumeration.<br/><br/> När du har skapat ett Recovery Services-valv du ned InMage Scout och installerar hello senaste uppdateringar tooset upp hello-distribution.
**Processervern** | Finns på primär plats | Du kan distribuera hello processen server toohandle cachelagring, komprimering och dataoptimering.<br/><br/> Den hanterar också push-installation av hello Unified Agent toomachines som du vill tooprotect.
**Konfigurationsserver** | Finns på sekundär plats | hello konfigurationsservern hanterar, konfigurera och övervaka distributionen, antingen med hjälp av hanteringswebbplatsen hello eller hello vContinuum-konsolen.
**vContinuum-server** | Valfri. Installerad i hello samma plats som hello konfigurationsservern. | Den innehåller en konsol för att hantera och övervaka din skyddade miljö.
**Huvudmålservern** | Finns i hello sekundär plats | Hej huvudmålservern innehåller replikerade data. Den tar emot data från hello processervern, skapar en replikdator på hello sekundär plats, och innehåller hello datakvarhållningspunkterna.<br/><br/> hello många huvudmålservrar du behöver beror på hello antalet datorer som du skyddar.<br/><br/> Om du vill toofail tillbaka toohello primär plats, måste en huvudmålserver det för. hello installeras Unified Agent på den här servern.
**VMware ESX/ESXi och vCenter-server** |  Virtuella datorer finns på ESX-/ESXi-värdar. Värdar hanteras med en vCenter-server | Du behöver en VMware infrastructure tooreplicate virtuella VMware-datorer.
**Virtuella datorer/fysiska servrar** |  Unified Agent installeras på virtuella VMware-datorer och fysiska servrar som du vill tooreplicate. | hello agenten fungerar som en kommunikationsprovider mellan alla hello-komponenter.


### <a name="replication-process"></a>Replikeringsprocessen

1. Du ställer in hello komponentservrarna på varje plats (konfiguration, process, huvudmålserver) och installera hello Unified Agent på datorer som du vill tooreplicate.
2. Efter den första replikeringen skickar hello-agenten på varje dator delta replikering ändringar toohello processervern.
3. Hej processervern optimerar hello data och överför toohello huvudmålservern på hello sekundär plats. hello konfigurationsservern hanterar replikeringen hello.

**Bild 2: VMware tooVMware-replikering**

![VMware tooVMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="next-steps"></a>Nästa steg

Granska hello [supportmatrisen](site-recovery-support-matrix-to-sec-site.md)
