---
title: "aaaReview hello arkitektur för fysisk server replication tooAzure med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över komponenter och arkitektur som används vid replikering av lokala fysiska servrar tooAzure med hello Azure Site Recovery-tjänsten"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 09c9b136-35f5-465e-8d0f-f4c5d5d9f880
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 4f334c181fa6f0821adf978ebe9dbd7d36a3c753
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-physical-server-replication-tooazure"></a>Steg 1: Granska hello arkitektur för fysisk server replication tooAzure

Den här artikeln beskriver hello komponenter och processer som används när du replikera lokala Windows-/ Linux fysiska servrar tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) service.

Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="architectural-components"></a>Arkitekturkomponenter

hello tabell sammanfattas hello-komponenter som du behöver.

**Komponent** | **Krav** | **Detaljer**
--- | --- | ---
**Azure** | Du behöver ett Azure-konto, ett Azure storage-konto och ett Azure-nätverk. | Replikerade data lagras i hello storage-konto och virtuella Azure-datorer skapas med hello replikerade data vid en redundansväxling. Virtuella Azure-datorer ansluta toohello virtuella Azure-nätverket när de skapas.
**Konfigurationsserver** | En enda lokal hanteringsserver (fysisk server eller VMware VM) som kör alla hello lokala Site Recovery-komponenter. Dessa inkluderar en konfigurationsservern, processervern, huvudmålservern. | Hej konfigurationsserverkomponent samordnar kommunikationen mellan lokala och Azure och hanterar datareplikering.
 **Processervern**  | Installeras som standard på hello konfigurationsservern. | Fungerar som en replikeringsgateway. Tar emot replikeringsdata, optimerar dem med cachelagring, komprimering och kryptering och skickar det tooAzure lagring.<br/><br/> Hej processervern hanterar också push-installation av hello Mobility service tooprotected datorer.<br/><br/> Du kan lägga till ytterligare separata dedikerade servrar, toohandle öka mängder replikeringstrafik.
 **Huvudmålservern** | Installeras som standard på hello lokalt konfigurationsservern. | Hanterar replikeringsdata vid återställning efter fel från Azure.<br/><br/> Om trafikvolymer för återställning efter fel är stora, kan du distribuera en separat huvudmålserver för återställning efter fel.
**Replikerade servrar** | Hej mobilitetstjänsten installeras på varje Windows-/ Linux-server du vill tooreplicate. Den kan installeras manuellt på varje dator eller med en push-installation från hello processervern.
**Återställning efter fel** | Infrastruktur krävs för återställning av en VMware. Du kan inte växla tillbaka tooa fysisk server.


**Bild 1: Fysiska tooAzure komponenter**

![Komponenter](./media/physical-walkthrough-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Replikeringsprocessen

1. Du kan konfigurera hello-distribution, inklusive lokalt och Azure-komponenter. I hello Recovery Services-valvet anger du hello replikeringskälla och mål, ställa in hello konfigurationsservern och skapa en replikeringsprincip, distribuera hello mobilitetstjänsten, aktivera replikering och köra ett redundanstest.
2.  Replikera datorer enligt hello replikeringsprinciper och en inledande kopia av hello data är replikerade tooAzure lagring.
4. Replikering av delta ändringar tooAzure börjar när replikeringen har slutförts. Spårade ändringar för en dator lagras i en .hrl-fil.
    - Replikera datorer kommunicerar med hello konfigurationsservern på port 443 för HTTPS inkommande för replikeringshantering av.
    - Replikera datorer skickar replikering data toohello processervern på port HTTPS 9443 inkommande (kan ändras).
    - hello konfigurationsservern samordnar replikeringshantering med Azure via port 443 för HTTPS utgående.
    - hello processervern tar emot data från källdatorer, optimerar och krypteras det och skickar det tooAzure lagring via port 443 utgående.
    - Om du aktiverar konsekvens för flera kommunicerar datorer i hello replikeringsgrupp med varandra via port 20004. Multi-VM används om du grupperar flera datorer i replikeringsgrupper som delar kraschkonsekventa och app-konsekventa återställningspunkter när de växlar över vid fel. Detta är användbart om datorerna kör hello samma arbetsbelastning och måste toobe konsekvent.
5. Trafiken är replikerade tooAzure lagring offentliga slutpunkter över hello internet. Du kan även använda Azure ExpressRoute [offentlig peering](../expressroute/expressroute-circuit-peerings.md#public-peering). Replikera trafik via ett plats-till-plats-VPN från en lokal plats tooAzure stöds inte.

**Bild 2: Fysiska tooAzure replikering**

![Optimerad](./media/physical-walkthrough-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Processen för redundans och återställning efter fel

När du har kontrollerat att redundanstestet fungerar som förväntat kan du köra oplanerade växlingar tooAzure som krävs. När du kör en växling vid fel, skapas virtuella Azure-datorer från replikerade data. Sedan, när din primära lokal plats är tillgänglig igen, kan du växla tillbaka. Tänk på följande:

- Planerad redundans stöds inte.
- Du kan växla över en enskild dator eller skapa [återställningsplaner](site-recovery-create-recovery-plans.md), toofail över flera datorer tillsammans.
- Du genomför en växling vid fel toostart använder hello arbetsbelastning från hello repliken Azure VM.
- När hello primära platsen är tillgänglig igen kan du replikera hello datorn från hello sekundära toohello primära platsen. Kör sedan en oplanerad redundans från hello sekundär plats. När du genomför den här redundansen data kommer att tillbaka lokalt och du behöver tooenable replikering tooAzure igen.

Återställning av komponenter är:

- **Tillfällig processerver i Azure**: du behöver tooset in en Azure VM tooact som en processerver, toohandle replikering från Azure. Du kan ta bort den här virtuella datorn när återställningen är klar.
- **VPN-anslutning**: du behöver en VPN-anslutning (eller Azure ExpressRoute) från hello Azure toohello lokal nätverksplats.
- **Separat lokal huvudmålserver**: hello lokala huvudmålservern (installeras som standard på hello konfigurationsservern) hanterar återställning efter fel. Om du växlar tillbaka stora volymer trafik bör du ställa in en separat lokal huvudmålserver för detta ändamål.
- **Princip för återställning efter fel**: du behöver en princip för återställning efter fel. Denna skapas automatiskt när du skapade din replikeringsprincip.
- **VMware-infrastrukturen**: du måste växla tillbaka tooan lokal VMware VM. Det innebär du behöver en lokal VMware-infrastruktur, även om du replikerar lokala fysiska servrar tooAzure.

**Bild 3: Fysisk server återställning efter fel**

![Återställning efter fel](./media/physical-walkthrough-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Nästa steg

Gå för[steg 2: Verifiera förutsättningar och begränsningar](physical-walkthrough-prerequisites.md)
