---
title: aaaHow fungerar Site Recovery arbete? | Microsoft Docs
description: "Den här artikeln innehåller en översikt över arkitekturen i Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-azure-to-azure-architecture
ms.openlocfilehash: ff1580d0fe294148dc0c621728491e6119c3048a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-site-recovery-work-for-on-premises-infrastructure"></a>Hur fungerar Azure Site Recovery för lokal infrastruktur?

> [!div class="op_single_selector"]
> * [Replikera virtuella Azure-datorer](site-recovery-azure-to-azure-architecture.md)
> * [Replikera lokala virtuella datorer på plats](site-recovery-components.md)

Den här artikeln beskriver underliggande arkitekturen för hello [Azure Site Recovery](site-recovery-overview.md) tjänsten och hello-komponenter som gör det fungerar för att replikera arbetsbelastningar från lokala tooAzure.

Bokför eventuella kommentarer längst ned hello av den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="replicate-tooazure"></a>Replikera tooAzure

Du kan replikera och skydda hello följande på lokal infrastruktur tooAzure:

- **VMware**: Lokala virtuella VMware-datorer som körs på en [värd som stöds](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). Du kan replikera virtuella VMware-datorer som kör [operativsystem som stöds](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)
- **Hyper-V**: Lokala virtuella Hyper-V-datorer som körs på [värdar som stöds](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
- **Fysiska datorer**: Lokala fysiska servrar som kör Windows eller Linux på [operativsystem som stöds](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions). Du kan replikera virtuella Hyper-V-datorer som körs på ett gästoperativsystem [som stöds av Hyper-V och Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).

## <a name="vmware-tooazure"></a>VMware tooAzure

Det här är vad du behöver för att replikera virtuella VMware-datorer tooAzure.

Område | Komponent | Information
--- | --- | ---
**Konfigurationsserver** | En enda hanteringsserver (virtuell VMWare-dator) kör alla lokala komponenter – konfigurationsserver, processerver, huvudmålserver | hello konfigurationsservern samordnar kommunikationen mellan lokala och Azure och hanterar datareplikering.
 **Processerver**:  | Installeras som standard på hello konfigurationsservern. | Fungerar som en replikeringsgateway. Tar emot replikeringsdata, optimerar dem med cachelagring, komprimering och kryptering och skickar det tooAzure lagring.<br/><br/> Hej processervern också hanterar push-installation av hello Mobility service tooprotected datorer och utför automatisk identifiering av virtuella VMware-datorer.<br/><br/> Eftersom distributionen växer kan du lägga till ytterligare separata dedikerade processer servrar toohandle öka mängder replikeringstrafik.
 **Huvudmålservern** | Installeras som standard på hello lokalt konfigurationsservern. | Hanterar replikeringsdata vid återställning efter fel från Azure.<br/><br/> Om trafikvolymer för återställning efter fel är stora, kan du distribuera en separat huvudmålserver för återställning efter fel.
**VMware-servrar** | Virtuella VMware-datorer finns på vSphere ESXi servrar och vi rekommenderar en vCenter server toomanage hello-värdar. | Du lägger till VMware-servrar tooyour Recovery Services-valvet.<br/><br/>
**Replikerade datorer** | Hej mobilitetstjänsten installeras på varje VMware VM som du vill tooreplicate. Den kan installeras manuellt på varje dator eller med en push-installation från hello processervern.| -

**Bild 1: VMware tooAzure komponenter**

![Komponenter](./media/site-recovery-components/arch-enhanced.png)

### <a name="replication-process"></a>Replikeringsprocessen

1. Du kan konfigurera hello-distribution, inklusive Azure komponenter och Recovery Services-valvet. I hello valvet anger du hello replikeringskälla och mål, ställa in hello konfigurationsservern och lägga till VMware-servrar, skapa en replikeringsprincip, distribuera hello mobilitetstjänsten, aktivera replikering och köra ett redundanstest.
2.  Datorer starta replikeringen i enlighet med hello replikeringsprinciper och en inledande kopia av hello data är replikerade tooAzure lagring.
4. Replikering av delta ändringar tooAzure börjar när hello replikeringen har slutförts. Spårade ändringar för en dator lagras i en .hrl-fil.
    - Replikera datorer kommunicerar med hello konfigurationsservern på port 443 för HTTPS inkommande för replikeringshantering av.
    - Replikera datorer skickar replikering data toohello processervern på port HTTPS 9443 inkommande (kan konfigureras).
    - hello konfigurationsservern samordnar replikeringshantering med Azure via port 443 för HTTPS utgående.
    - hello processervern tar emot data från källdatorer, optimerar och krypteras det och skickar det tooAzure lagring via port 443 utgående.
    - Om du aktiverar konsekvens för flera kommunicerar datorer i hello replikeringsgrupp med varandra via port 20004. Multi-VM används om du grupperar flera datorer i replikeringsgrupper som delar kraschkonsekventa och app-konsekventa återställningspunkter när de växlar över vid fel. Detta är användbart om datorerna kör hello samma arbetsbelastning och måste toobe konsekvent.
5. Trafiken är replikerade tooAzure lagring offentliga slutpunkter över hello internet. Du kan även använda Azure ExpressRoute [offentlig peering](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-circuit-peerings#public-peering). Replikera trafik via ett plats-till-plats-VPN från en lokal plats tooAzure stöds inte.

**Bild 2: VMware tooAzure-replikering**

![Optimerad](./media/site-recovery-components/v2a-architecture-henry.png)

### <a name="failover-and-failback"></a>Redundans och återställning efter fel

1. När du har kontrollerat att redundanstestet fungerar som förväntat kan du köra oplanerade växlingar tooAzure som krävs. Planerad redundans stöds inte.
2. Du kan växla över en enskild dator eller skapa [återställningsplaner](site-recovery-create-recovery-plans.md), toofail över flera virtuella datorer.
3. När du kör en redundans skapas virtuella replikdatorer i Azure. Du genomför en växling vid fel toostart använder hello arbetsbelastning från hello repliken Azure VM.
4. När din primära lokala plats är tillgänglig igen, kan du återställa dit. Du ställer in en infrastruktur för återställning efter fel, starta replikering hello datorn från hello sekundär plats toohello primära och köra oplanerad växling från hello sekundär plats. När du genomför den här redundansen data kommer att tillbaka lokalt och du behöver tooenable replikering tooAzure igen. [Läs mer](site-recovery-failback-azure-to-vmware.md)

**Bild 3: VMware/fysisk återställning efter fel**

![Återställning efter fel](./media/site-recovery-components/enhanced-failback.png)

## <a name="physical-tooazure"></a>Fysisk tooAzure

När du replikerar fysiska, lokala servrar tooAzure replikering använder också hello komponenter och processer som [VMware tooAzure](#vmware-replication-to-azure), men Observera skillnaderna:

- Du kan använda en fysisk server för hello konfigurationsservern och i stället för en VMware VM
- Du behöver en lokal VMware-infrastruktur för återställning efter fel. Du kan inte växla tillbaka tooa fysisk dator.

## <a name="hyper-v-tooazure"></a>Hyper-V tooAzure

### <a name="replication-process"></a>Replikeringsprocessen

1. Du ställa in hello Azure komponenter. Vi rekommenderar att du ställer in konton för lagring och nätverk innan du påbörjar Site Recovery-distributionen.
2. Du skapar ett Replication Services-valv för Site Recovery och konfigurerar valvinställningar, inklusive:
    - Inställningar för källa och mål. Om du inte hanterar Hyper-V-värdar i ett VMM-moln, för hello mål du skapa en behållare för Hyper-V-platsen och lägga till Hyper-V-värdar tooit. Om Hyper-V-värdar hanteras i VMM, är hello källan hello VMM-moln. hello målet är Azure.
    - Installation av hello Azure Site Recovery-providern och hello Microsoft Azure Recovery Services-agenten. Om du har VMM hello-providern kommer att installeras på den och hello-agenten på varje Hyper-V-värd. Om du inte har VMM är både hello providern och agenten installerade på varje värd.
    - Du kan skapa en replikeringsprincip för hello Hyper-V-platsen eller VMM-moln. hello principen är tillämpade tooall virtuella datorer på värdar i hello plats eller molnet.
    - Du aktiverar replikering för virtuella Hyper-V-datorer. Den inledande replikeringen sker i enlighet med hello replikeringsprincipens inställningar.
4. Dataändringar spåras och replikering av delta ändringar tooAzure börjar när hello replikeringen har slutförts. Spårade ändringar för ett objekt lagras i en .hrl-fil.
5. Du kör ett test redundans toomake att allt fungerar.

### <a name="failover-and-failback-process"></a>Processen för redundans och återställning efter fel

1. Du kan köra en planerad eller oplanerad [redundans](site-recovery-failover.md) från lokala virtuella Hyper-V-datorer tooAzure. Om du kör en planerad redundans och virtuella källdatorer stängs tooensure inga data går förlorade.
2. Du kan växla över en enskild dator eller skapa [återställningsplaner](site-recovery-create-recovery-plans.md) tooorchestrate växling vid fel på flera datorer.
4. När du har kört hello redundans ska kunna toosee hello skapade Replikdatorerna i Azure. Du kan tilldela en offentlig IP-adress toohello VM om det behövs.
5. Du genomför sedan hello redundans toostart åt hello arbetsbelastning från hello replik Azure VM.
6. När din primära lokala plats är tillgänglig igen, kan du [återställa dit](site-recovery-failback-from-azure-to-hyper-v.md). Startar du en planerad redundans från Azure toohello primära platsen. För planerad redundans kan du ändrar väljer toofailback toohello samma VM eller tooan alternativ plats och synkronisera utseende mellan Azure och lokalt, tooensure inga data går förlorade. När virtuella datorer skapas lokalt, genomför hello växling vid fel.

**Bild 4: Hyper-V tooAzure replikering**

![Komponenter](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Bild 5: Hyper-V i VMM-moln tooAzure replikering**

![Komponenter](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)


## <a name="replicate-tooa-secondary-site"></a>Replikera tooa sekundär plats

Du kan replikera hello följande tooyour sekundär plats:

- **VMware**: Lokala virtuella VMware-datorer som körs på en [värd som stöds](site-recovery-support-matrix-to-sec-site.md#on-premises-servers). Du kan replikera virtuella VMware-datorer som kör [operativsystem som stöds](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions)
- **Fysiska datorer**: Lokala fysiska servrar som kör Windows eller Linux på [operativsystem som stöds](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions).
- **Hyper-V**: Lokala virtuella Hyper-V-datorer som körs på [Hyper-V-värdar som stöds](site-recovery-support-matrix-to-sec-site.md#on-premises-servers) och hanteras i VMM-moln. [värdar som stöds](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). Du kan replikera virtuella Hyper-V-datorer som körs på ett gästoperativsystem [som stöds av Hyper-V och Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).


## <a name="vmwarephysical-tooa-secondary-site"></a>VMware/fysiska tooa sekundär plats

Du kan replikera virtuella VMware-datorer eller fysiska servrar tooa sekundär plats med InMage Scout.

### <a name="components"></a>Komponenter

**Område** | **Komponent** | **Detaljer**
--- | --- | ---
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

**Bild 6: VMware tooVMware-replikering**

![VMware tooVMware](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="hyper-v-tooa-secondary-site"></a>Hyper-V tooa sekundär plats

Det här är vad du behöver för att replikera virtuella Hyper-V-datorer tooa sekundär plats.


**Område** | **Komponent** | **Detaljer**
--- | --- | ---
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

**Bild 7: VMM tooVMM replikering**

![Lokala tooon lokala](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback"></a>Redundans och återställning efter fel

1. Du kan köra en planerad eller oplanerad [redundansväxling](site-recovery-failover.md) mellan lokala platser. Om du kör en planerad redundans och virtuella källdatorer stängs tooensure inga data går förlorade.
2. Du kan växla över en enskild dator eller skapa [återställningsplaner](site-recovery-create-recovery-plans.md) tooorchestrate växling vid fel på flera datorer.
4. Om du vill utföra en oplanerad växling tooa sekundär plats, efter hello redundans datorer hello sekundär plats har inte aktiverats för skydd eller replikering. Om du körde en planerad redundans, efter hello redundans skydda datorer i hello sekundär plats.
5. Sedan kan du spara hello redundans toostart använder hello arbetsbelastning från hello replikerade virtuella datorn.
6. När den primära platsen är tillgänglig igen kan initiera du omvänd replikering tooreplicate från hello sekundär plats toohello primära. Omvänd replikering ger hello virtuella datorer i ett skyddat läge, men hello sekundärt datacenter är fortfarande aktiva hello-plats.
7. toomake hello primära platsen till aktiv plats för hello igen, kan du starta en planerad redundansväxling från sekundär tooprimary, följt av en annan omvänd replikering.


## <a name="next-steps"></a>Nästa steg

- [Lär dig mer](site-recovery-hyper-v-azure-architecture.md) om arbetsflödet för hello Hyper-V-replikering.
- [Kontrollera krav](site-recovery-prereq.md)
