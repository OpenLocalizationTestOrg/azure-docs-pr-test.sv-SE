---
title: aaaHow hanterar VMware replikering tooAzure arbete i Azure Site Recovery? | Microsoft Docs
description: "Den här artikeln innehåller en översikt över komponenter och arkitektur som används när replikera lokala virtuella VMware-datorer och fysiska servrar tooAzure med hello Azure Site Recovery-tjänsten"
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
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: vmware-walkthrough-architecture
ms.openlocfilehash: f0fb834f8b251640f97e4d0163b2b9e54de691e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-vmware-replication-tooazure-work-in-site-recovery"></a>Hur fungerar VMware replikering tooAzure i Site Recovery?

Den här artikeln beskriver hello komponenter och processer som är inblandade vid replikering av lokala VMware virtuella datorer och Windows-/ Linux fysiska servrar, tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) service.

När du replikerar fysiska, lokala servrar tooAzure hello replikering använder även samma komponenter och processer som VMware VM replikering med dessa skillnader:

- Du kan använda en fysisk server för hello konfigurationsservern och i stället för en VMware VM.
- Du behöver en lokal VMware-infrastruktur för återställning efter fel. Du kan inte växla tillbaka tooa fysisk dator.

Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="architectural-components"></a>Arkitekturkomponenter

Det finns ett antal komponenter ingår vid replikering av virtuella VMware-datorer och fysiska servrar tooAzure.

**Komponent** | **Plats** | **Detaljer**
--- | --- | ---
**Azure** | I Azure behöver du ett Azure-konto, ett Azure-lagringskonto och ett Azure-nätverk. | Replikerade data lagras i hello storage-konto och virtuella Azure-datorer skapas med hello replikerade data när det uppstår redundans från din lokala plats. hello Azure virtuella datorer ansluta toohello virtuella Azure-nätverket när de skapas.
**Konfigurationsserver** | En enda lokal hanteringsserver (VMWare VM) som kör alla hello lokala komponenter som behövs för hello-distribution, inklusive hello konfigurationsservern, processervern och huvudmålservern | Hej konfigurationsserverkomponent samordnar kommunikationen mellan lokala och Azure och hanterar datareplikering.
 **Processerver**:  | Installeras som standard på hello konfigurationsservern. | Fungerar som en replikeringsgateway. Tar emot replikeringsdata, optimerar dem med cachelagring, komprimering och kryptering och skickar det tooAzure lagring.<br/><br/> Hej processervern också hanterar push-installation av hello Mobility service tooprotected datorer och utför automatisk identifiering av virtuella VMware-datorer.<br/><br/> Eftersom distributionen växer kan du lägga till ytterligare separata dedikerade processer servrar toohandle öka mängder replikeringstrafik.
 **Huvudmålservern** | Installeras som standard på hello lokalt konfigurationsservern. | Hanterar replikeringsdata vid återställning efter fel från Azure.<br/><br/> Om trafikvolymer för återställning efter fel är stora, kan du distribuera en separat huvudmålserver för återställning efter fel.
**VMware-servrar** | Virtuella VMware-datorer finns på vSphere ESXi servrar och vi rekommenderar en vCenter server toomanage hello-värdar. | Du lägger till VMware-servrar tooyour Recovery Services-valvet.
**Replikerade datorer** | Hej mobilitetstjänsten installeras på varje VMware VM som du vill tooreplicate. Den kan installeras manuellt på varje dator eller med en push-installation från hello processervern.

Lär dig mer om kraven för distribution av hello och krav för var och en av komponenterna i hello [supportmatrisen](site-recovery-support-matrix-to-azure.md).

**Bild 1: VMware tooAzure komponenter**

![Komponenter](./media/site-recovery-components/arch-enhanced.png)

## <a name="replication-process"></a>Replikeringsprocessen

1. Du kan konfigurera hello-distribution, inklusive Azure komponenter och Recovery Services-valvet. I hello valvet anger du hello replikeringskälla och mål, ställa in hello konfigurationsservern och lägga till VMware-servrar, skapa en replikeringsprincip, distribuera hello mobilitetstjänsten, aktivera replikering och köra ett redundanstest.
2.  Datorer starta replikeringen i enlighet med hello replikeringsprinciper och en inledande kopia av hello data är replikerade tooAzure lagring.
4. Replikering av delta ändringar tooAzure börjar när hello replikeringen har slutförts. Spårade ändringar för en dator lagras i en .hrl-fil.
    - Replikera datorer kommunicerar med hello konfigurationsservern på port 443 för HTTPS inkommande för replikeringshantering av.
    - Replikera datorer skickar replikering data toohello processervern på port HTTPS 9443 inkommande (kan konfigureras).
    - hello konfigurationsservern samordnar replikeringshantering med Azure via port 443 för HTTPS utgående.
    - hello processervern tar emot data från källdatorer, optimerar och krypteras det och skickar det tooAzure lagring via port 443 utgående.
    - Om du aktiverar konsekvens för flera kommunicerar datorer i hello replikeringsgrupp med varandra via port 20004. Multi-VM används om du grupperar flera datorer i replikeringsgrupper som delar kraschkonsekventa och app-konsekventa återställningspunkter när de växlar över vid fel. Detta är användbart om datorerna kör hello samma arbetsbelastning och måste toobe konsekvent.
5. Trafiken är replikerade tooAzure lagring offentliga slutpunkter över hello internet. Du kan även använda Azure ExpressRoute [offentlig peering](../expressroute/expressroute-circuit-peerings.md#public-peering). Replikera trafik via ett plats-till-plats-VPN från en lokal plats tooAzure stöds inte.

**Bild 2: VMware tooAzure-replikering**

![Optimerad](./media/site-recovery-components/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Processen för redundans och återställning efter fel

1. När du har kontrollerat att redundanstestet fungerar som förväntat kan du köra oplanerade växlingar tooAzure som krävs. Planerad redundans stöds inte.
2. Du kan växla över en enskild dator eller skapa [återställningsplaner](site-recovery-create-recovery-plans.md), toofail över flera virtuella datorer.
3. När du kör en redundans skapas virtuella replikdatorer i Azure. Du genomför en växling vid fel toostart använder hello arbetsbelastning från hello repliken Azure VM.
4. När din primära lokala plats är tillgänglig igen, kan du återställa dit. Du ställer in en infrastruktur för återställning efter fel, starta replikering hello datorn från hello sekundär plats toohello primära och köra oplanerad växling från hello sekundär plats. När du genomför den här redundansen data kommer att tillbaka lokalt och du behöver tooenable replikering tooAzure igen. [Läs mer](site-recovery-failback-azure-to-vmware.md)

Det finns några återställningskrav:


- **Tillfällig processerver i Azure**: Om du vill ha toofail tillbaka från Azure efter växling vid fel måste tooset upp en Azure VM som konfigurerats som en processerver toohandle replikering från Azure. Du kan ta bort den här virtuella datorn när återställningen är klar.
- **VPN-anslutning**: för återställning efter fel behöver du en VPN-anslutning (eller Azure ExpressRoute) Konfigurera från hello Azure toohello lokal nätverksplats.
- **Separat lokal huvudmålserver**: hello lokala huvudmålservern hanterar återställning efter fel. Hej huvudmålservern installeras som standard på hello hanteringsservern, men om du växlar tillbaka större volymer trafik bör du ställa in en separat lokal huvudmålserver för detta ändamål.
- **Princip för återställning efter fel**: tooreplicate tillbaka tooyour lokal plats måste du en princip för återställning efter fel. Denna skapas automatiskt när du skapade din replikeringsprincip.
- **VMware-infrastrukturen**: du måste växla tillbaka tooan lokal VMware VM. Det innebär du behöver en lokal VMware-infrastruktur, även om du replikerar lokala fysiska servrar tooAzure.

**Bild 3: VMware/fysisk återställning efter fel**

![Återställning efter fel](./media/site-recovery-components/enhanced-failback.png)


## <a name="next-steps"></a>Nästa steg

Granska hello [supportmatrisen](site-recovery-support-matrix-to-azure.md)
