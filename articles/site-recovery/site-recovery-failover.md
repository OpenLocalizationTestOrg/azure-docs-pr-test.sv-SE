---
title: Redundans i Site Recovery | Microsoft Docs
description: "Azure Site Recovery samordnar replikering, redundans och återställning av virtuella datorer och fysiska servrar. Läs mer om redundans till Azure eller ett sekundärt datacenter."
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
ms.openlocfilehash: ef586191f0b89dca89810644d45503fe42538635
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="failover-in-site-recovery"></a>Redundans i Site Recovery
Den här artikeln beskriver hur till redundans virtuella datorer och fysiska servrar som skyddas av Site Recovery.

## <a name="prerequisites"></a>Krav
1. Innan du gör en redundansväxling gör en [redundanstestningen](site-recovery-test-failover-to-azure.md) så att allt fungerar som förväntat.
1. [Förbered nätverket](site-recovery-network-design.md) på målplatsen innan du gör en redundansväxling.  


## <a name="run-a-failover"></a>Kör en redundansväxling
Den här proceduren beskriver hur du kör en redundansväxling för en [återställningsplanen](site-recovery-create-recovery-plans.md). Du kan också köra redundans för en enskild virtuell dator eller fysisk server från den **replikerade objekt** sidan


![Redundans](./media/site-recovery-failover/Failover.png)

1. Välj **Återställningsplaner** > *recoveryplan_name*. Klicka på **växling vid fel**
2. På den **redundans** väljer en **återställningspunkt** ska gå över till. Du kan använda något av följande alternativ:
    1.  **Senaste** (standard): det här alternativet först bearbetar alla data som har skickats till Site Recovery-tjänsten för att skapa en återställningspunkt för varje virtuell dator innan du redundansväxlar dem till den. Det här alternativet ger den lägsta RPO (mål för återställningspunkt) som den virtuella datorn skapas efter växling vid fel har alla data som har replikerats till Site Recovery-tjänsten när redundans utlöstes.
    1.  **Senaste bearbetas**: det här alternativet flyttas över alla virtuella datorer i återställningsplanen så att den senaste återställningspunkten som redan har behandlats av Site Recovery-tjänsten. När du gör testa redundans för en virtuell dator visas också tidsstämpeln för den senaste bearbetade återställningspunkten. Om du gör redundans för en återställningsplan går du till en enskild virtuell dator och titta på **senaste återställningspunkter** rutan för att hämta informationen. Ingen tid för att bearbeta obearbetade data, ger det här alternativet ett alternativ för låga RTO (mål) växling vid fel.
    1.  **Senaste programkonsekventa**: det här alternativet flyttas över alla virtuella datorer i återställningsplanen så att den senaste programkonsekvent återställningspunkt som redan har behandlats av Site Recovery-tjänsten. När du gör testa redundans för en virtuell dator visas också tidsstämpeln för den senaste programkonsekventa återställningspunkten. Om du gör redundans för en återställningsplan går du till en enskild virtuell dator och titta på **senaste återställningspunkter** rutan för att hämta informationen.
    1.  **Senaste multi-VM bearbetas**: det här alternativet är endast tillgängligt för återställningsplaner som har minst en virtuell dator med konsekvens för flera på. Virtuella datorer som är en del av en replikering redundansväxla till den senaste vanliga multi-VM konsekventa återställningspunkten punkt. Andra virtuella datorer växling till sina senaste bearbetade återställningspunkten.  
    1.  **Senaste multi-VM programkonsekventa**: det här alternativet är endast tillgängligt för återställningsplaner som har minst en virtuell dator med multi-VM konsekvenskontroll på. Virtuella datorer som är en del av en replikering redundansväxla till den senaste vanliga multi-VM programkonsekventa återställningspunkten punkt. Andra virtuella datorer redundans till sina senaste programkonsekventa återställningspunkten.
    1.  **Anpassad**: Om du gör testa redundans för en virtuell dator, så du kan använda det här alternativet ska gå över till en viss återställningspunkt.

    > [!NOTE]
    > Möjlighet att välja en återställningspunkt är endast tillgänglig när du redundansväxlar till Azure.
    >
    >


1. Om några av de virtuella datorerna i återställningsplanen har redundansväxlats i en tidigare körning och nu virtuella datorer är aktiva på både käll- och plats, kan du använda **ändra riktning** alternativet för att avgöra i vilken riktning på växling vid fel ska inträffa.
1. Om du växlar över till Azure och datakryptering är aktiverat för molnet (gäller endast när du har skyddat Hyper-v virtuella datorer från en VMM-Server) i **krypteringsnyckeln** Välj det certifikat som utfärdades när du har aktiverat datakryptering under installationen på VMM-servern.
1. Välj **Stäng datorn innan du påbörjar redundans** om du vill använda Site Recovery att göra en avstängning av virtuella källdatorer innan växling vid fel. Redundans fortsätter även om avstängning misslyckas.  

    > [!NOTE]
    > Om Hyper-v virtuella datorer försöker det här alternativet också synkroniserar lokala data som inte har ännu har skickats till tjänsten innan växling vid fel.
    >
    >

1. Du kan följa förloppet för växling vid fel på den **jobb** sidan. Även om fel uppstår under en oplanerad redundans körs återställningsplanen tills den är klar.
1. Verifiera den virtuella datorn efter växling vid fel, genom att logga in till den. Om du vill gå en annan återställningspunkt för den virtuella datorn så att du kan använda **ändra återställningspunkt** alternativet.
1. När du är nöjd med den redundansväxlade virtuella datorn kan du **genomför** växling vid fel. Detta tar bort alla återställningspunkter som är tillgängliga med tjänsten och **ändra återställningspunkt** alternativet kommer inte längre tillgänglig.

## <a name="planned-failover"></a>Planerad redundans
Förutom redundans, Hyper-V virtuella datorer som skyddas med Site Recovery även stöd **planerad redundans**. Detta är ett noll data går förlorade redundans alternativ. När en planerad redundansväxling initieras först virtuella källdatorer är avstängd, synkroniseras data som ska synkroniseras och sedan en växling vid fel utlöses.

> [!NOTE]
> När du redundans Hyper-v virtuella datorer från en lokal plats till en annan lokal plats för att gå tillbaka till webbplatsen för primär lokal måste du första **replikera omvänt** den virtuella datorn tillbaka till primär plats och sedan Utlös en växling vid fel. Om den primära virtuella datorn inte är tillgängliga innan från att **replikera omvänt** du måste återställa den virtuella datorn från en säkerhetskopia.   
>
>

## <a name="failover-job"></a>Beställningsjobbet

![Redundans](./media/site-recovery-failover/FailoverJob.png)

När en växling vid fel utlöses omfattar följande steg:

1. Kravkontroll: det här steget säkerställer att alla krav som gäller för växling vid fel är uppfyllda.
1. Redundans: Det här steget bearbetar data och gör den redo så att en virtuell Azure-dator kan skapas ur den. Om du har valt **senaste** återställningspunkten det här steget skapar en återställningspunkt från data som har skickats till tjänsten.
1. Starta: Det här steget skapar en virtuell Azure-dator med hjälp av data som bearbetas i föregående steg.

> [!WARNING]
> **Inte avbryta en pågående redundans**: innan redundans startas replikeringen för den virtuella datorn har stoppats. Om du **Avbryt** ett jobb pågår, redundans stoppas, men den virtuella datorn startar inte replikeras. Att det går inte starta replikering igen.
>
>

## <a name="time-taken-for-failover-to-azure"></a>Tidsåtgång för redundans till Azure

I vissa fall kräver redundans för virtuella datorer ett extra steg som tar vanligtvis cirka 8 till 10 minuter för att slutföra. Dessa fall är följande:

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

I andra fall detta mellanliggande steg krävs inte och den tid det tar för växling vid fel är betydligt lägre. 





## <a name="using-scripts-in-failover"></a>Med hjälp av skript i redundanskluster
Du kanske vill automatisera vissa åtgärder, medan en växling vid fel. Du kan använda skript eller [Azure automation-runbooks](site-recovery-runbook-automation.md) i [återställningsplaner](site-recovery-create-recovery-plans.md) du gör.

## <a name="other-considerations"></a>Andra överväganden
* **Enhetsbeteckning** – att behålla enhetsbeteckning på virtuella datorer efter redundans som du kan ange den **SAN-princip** för den virtuella datorn till **OnlineAll**. [Läs mer](https://support.microsoft.com/en-us/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure).



## <a name="next-steps"></a>Nästa steg
När du redundansväxlade virtuella datorer och lokala Datacenter är tillgängligt, bör du [ **skydda igen** ](site-recovery-how-to-reprotect.md) virtuella VMware-datorer tillbaka till lokala datacenter.

Använd [ **planerad redundans** ](site-recovery-failback-from-azure-to-hyper-v.md) att **återställning** Hyper-v virtuella datorer till lokala från Azure.

Om du har inte över en Hyper-v virtuell dator till en annan lokal datacenter som hanteras av en VMM-server och primära Datacenter är tillgänglig, sedan **omvänd replikering** alternativet för att starta replikering tillbaka till primära data Center.
