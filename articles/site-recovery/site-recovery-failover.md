---
title: aaaFailover i Site Recovery | Microsoft Docs
description: "Azure Site Recovery samordnar hello replikering, redundans och återställning av virtuella datorer och fysiska servrar. Läs mer om tooAzure för växling vid fel eller ett sekundärt datacenter."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/04/2017
ms.author: pratshar
ms.openlocfilehash: 7cacea829d78bb7de2b2d67402291b472b10f023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="failover-in-site-recovery"></a>Redundans i Site Recovery
Den här artikeln beskriver hur toofailover virtuella datorer och fysiska servrar skyddas av Site Recovery.

## <a name="prerequisites"></a>Krav
1. Innan du gör en redundansväxling gör en [redundanstestningen](site-recovery-test-failover-to-azure.md) tooensure att allt fungerar som förväntat.
1. [Förbereda hello nätverket](site-recovery-network-design.md) på målplatsen innan du gör en redundansväxling.  


## <a name="run-a-failover"></a>Kör en redundansväxling
Den här proceduren beskriver hur toorun en växling vid fel för en [återställningsplanen](site-recovery-create-recovery-plans.md). Du kan också köra hello redundans för en enskild virtuell dator eller fysisk server från hello **replikerade objekt** sidan


![Redundans](./media/site-recovery-failover/Failover.png)

1. Välj **Återställningsplaner** > *recoveryplan_name*. Klicka på **växling vid fel**
2. På hello **redundans** väljer en **återställningspunkt** toofailover till. Du kan använda något av följande alternativ för hello:
    1.  **Senaste** (standard): det här alternativet först bearbetar alla hello-data som har skickats tooSite Recovery service toocreate en återställningspunkt för varje virtuell dator innan redundansväxlingen tooit. Det här alternativet får hello lägsta RPO (mål för återställningspunkt) som hello virtuell dator skapas efter växling vid fel har alla hello data som har replikerats tooSite Recovery-tjänsten när hello redundans utlöstes.
    1.  **Senaste bearbetas**: det här alternativet flyttas över alla virtuella datorer i hello recovery plan toohello senaste återställningspunkten som redan har behandlats av Site Recovery-tjänsten. När du gör testa redundans för en virtuell dator visas också tidsstämpeln för hello senaste bearbetade återställningspunkten. Om du gör redundans för en återställningsplan går tooindividual virtuell dator och titta på **senaste återställningspunkter** panelen tooget informationen. Ingen tid läggs tooprocess hello obearbetade data, det här alternativet ger ett alternativ för låga RTO (mål) växling vid fel.
    1.  **Senaste programkonsekventa**: det här alternativet flyttas över alla virtuella datorer i hello recovery plan toohello senaste programkonsekvent återställningspunkt som redan har behandlats av Site Recovery-tjänsten. När du gör testa redundans för en virtuell dator visas också tidsstämpeln för hello senaste programkonsekventa återställningspunkten. Om du gör redundans för en återställningsplan går tooindividual virtuell dator och titta på **senaste återställningspunkter** panelen tooget informationen.
    1.  **Senaste multi-VM bearbetas**: det här alternativet är endast tillgängligt för återställningsplaner som har minst en virtuell dator med konsekvens för flera på. Virtuella datorer som ingår i en grupp redundans toohello senaste vanliga multi-VM konsekvent replikeringsåterställning punkt. Andra virtuella datorer redundans tootheir senaste bearbetade återställningspunkten.  
    1.  **Senaste multi-VM programkonsekventa**: det här alternativet är endast tillgängligt för återställningsplaner som har minst en virtuell dator med multi-VM konsekvenskontroll på. Virtuella datorer som är en del av en replikering gruppera redundans toohello senaste vanliga multi-VM programkonsekvent återställningspunkt. Andra virtuella datorer redundans tootheir senaste programkonsekventa återställningspunkter.
    1.  **Anpassad**: Om du gör testa redundans för en virtuell dator, så du kan använda alternativet toofailover tooa viss återställningspunkten.

    > [!NOTE]
    > hello alternativet toochoose en återställningspunkt är endast tillgängligt när du misslyckas över tooAzure.
    >
    >


1. Om några av hello virtuella datorer i återställningsplanen hello har redundansväxlats i en tidigare körning och nu hello virtuella datorer är aktiva på både käll- och plats, kan du använda **ändra riktning** alternativet toodecide hello riktning i vilka hello växling vid fel ska inträffa.
1. Om du växlar över tooAzure och datakryptering är aktiverat för hello molnet (gäller endast när du har skyddat Hyper-v virtuella datorer från en VMM-Server) i **krypteringsnyckeln** väljer hello-certifikat som utfärdades när du Aktivera kryptering av data under installationen på hello VMM-servern.
1. Välj **Stäng datorn innan du påbörjar redundans** om du vill använda Site Recovery tooattempt toodo stängas av virtuella källdatorer innan hello växling vid fel. Redundans fortsätter även om avstängning misslyckas.  

    > [!NOTE]
    > Om Hyper-v virtuella datorer försöker det här alternativet också toosynchronize hello lokala data som inte har ännu skickats toohello service innan hello växling vid fel.
    >
    >

1. Du kan följa redundansförloppet hello hello **jobb** sidan. Även om fel uppstår under en oplanerad redundans körs hello återställningsplan tills den är klar.
1. Validera hello virtuell dator genom att logga in tooit efter hello redundans. Om du vill använda toogo en annan återställningspunkt för hello virtuell dator och du kan använda **ändra återställningspunkt** alternativet.
1. När du är nöjd med hello redundansväxlade virtuella datorn, kan du **genomför** hello växling vid fel. Detta tar bort alla hello tillgängliga återställningspunkter med hello-tjänsten och **ändra återställningspunkt** alternativet kommer inte längre tillgänglig.

## <a name="planned-failover"></a>Planerad redundans
Förutom redundans, Hyper-V virtuella datorer som skyddas med Site Recovery även stöd **planerad redundans**. Detta är ett noll data går förlorade redundans alternativ. När en planerad redundansväxling initieras först hello källa virtuella datorer är avstängd, hello data har toobe synkroniseras synkroniseras och sedan en växling vid fel utlöses.

> [!NOTE]
> När du redundans Hyper-v virtuella datorer från en lokal plats tooanother lokal plats toocome tillbaka toohello primär lokal plats har du toofirst **replikera omvänt** hello virtuella tillbaka tooprimary plats och sedan Utlös en växling vid fel. Om hello primära virtuella datorn inte är tillgängligt sedan innan du startar för**replikera omvänt** du har toorestore hello virtuell dator från en säkerhetskopia.   
>
>

## <a name="failover-job"></a>Beställningsjobbet

![Redundans](./media/site-recovery-failover/FailoverJob.png)

När en växling vid fel utlöses omfattar följande steg:

1. Kravkontroll: det här steget säkerställer att alla krav som gäller för växling vid fel är uppfyllda.
1. Redundans: Det här steget bearbetar hello data och gör den redo så att en virtuell Azure-dator kan skapas ur den. Om du har valt **senaste** återställningspunkten det här steget skapar en återställningspunkt från hello data som har skickats toohello service.
1. Starta: Det här steget skapar en virtuell Azure-dator med hjälp av hello data bearbetas i hello föregående steg.

> [!WARNING]
> **Inte avbryta en pågående redundans**: innan redundans startas replikering för hello virtuella datorn har stoppats. Om du **Avbryt** ett jobb pågår, stoppar redundans men hello virtuella datorn startar inte tooreplicate. Att det går inte starta replikering igen.
>
>

## <a name="time-taken-for-failover-tooazure"></a>Tidsåtgång för växling vid fel tooAzure

I vissa fall kräver redundans för virtuella datorer ett extra steg som tar vanligtvis cirka 8 too10 minuter toocomplete. Dessa fall är följande:

* Med hjälp av mobilitetstjänsten version som är äldre än 9.8 virtuella VMware-datorer
* Fysiska servrar 
* VMware Linux virtuella datorer
* Hyper-V virtuella datorer som skyddas som fysiska servrar
* VMware-datorer där följande drivrutiner inte finns som startdrivrutiner 
    * storvsc 
    * VMBus 
    * storflt 
    * Intelide 
    * ATAPI
* Virtuella VMware-datorer som inte har DHCP-tjänsten aktiveras oavsett om de använder DHCP eller statiska IP-adresser

I alla hello andra fall detta mellanliggande steg krävs inte och hello tidsåtgång för hello redundans är betydligt lägre. 





## <a name="using-scripts-in-failover"></a>Med hjälp av skript i redundanskluster
Du kanske vill tooautomate vissa åtgärder medan du gör en redundansväxling. Du kan använda skript eller [Azure automation-runbooks](site-recovery-runbook-automation.md) i [återställningsplaner](site-recovery-create-recovery-plans.md) toodo som.

## <a name="other-considerations"></a>Andra överväganden
* **Enhetsbeteckning** – tooretain hello enhetsbeteckning på virtuella datorer efter redundans kan du ange hello **SAN-princip** hello virtuella datorn för**OnlineAll**. [Läs mer](https://support.microsoft.com/en-us/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure).



## <a name="next-steps"></a>Nästa steg
När du har redundansväxlade virtuella datorer och hello lokalt Datacenter är tillgänglig, bör du [ **skydda igen** ](site-recovery-how-to-reprotect.md) virtuella VMware-datorer tillbaka toohello lokalt datacenter.

Använd [ **planerad redundans** ](site-recovery-failback-from-azure-to-hyper-v.md) alternativet för**återställning** Hyper-v-datorer tillbaka tooon lokala från Azure.

Om du har inte över en Hyper-v virtuell dator tooanother lokala data center som hanteras av en VMM-servern och hello primära Datacenter är tillgänglig, Använd **omvänd replikering** alternativet toostart hello replikering tillbaka toohello primära datacenter.
