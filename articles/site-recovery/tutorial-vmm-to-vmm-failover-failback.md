---
title: "Växla över och misslyckas tillbaka Hyper-V-datorer som replikeras till ett sekundärt datacenter med Site Recovery | Microsoft Docs"
description: "Lär dig hur du växla över Hyper-V virtuella datorer till en sekundär lokal plats och växla tillbaka till primär plats med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 44a662fa-2e7a-4996-86df-fdd6d6f5dedf
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/16/2017
ms.author: raynew
ms.openlocfilehash: 8f139070de99c4249207d048d445e86dd41e9060
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-your-secondary-on-premises-site"></a>Växla över och misslyckas tillbaka Hyper-V virtuella datorer som replikeras till en sekundär lokal plats

Den [Azure Site Recovery](site-recovery-overview.md) tjänsten hanterar och samordnar replikering, redundans och återställning efter fel för lokala datorer och virtuella Azure-datorer (VM).

Den här självstudiekursen beskrivs hur du redundansväxlar en Hyper-V virtuell dator som hanteras i ett moln med System Center Virtual Machine Manager (VMM), till en sekundär plats för VMM. När du har redundansväxlats växlar du tillbaka till den lokala platsen när den är tillgänglig. I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Växla över en Hyper-V virtuell dator från en primär VMM-moln till en sekundär VMM-moln
> * Skydda igen från den sekundära platsen till den primära servern och återställas
> * Du kan också starta replikering från primär till sekundär igen

## <a name="overview"></a>Översikt

Redundans och återställning efter fel har tre steg:

1. **Växla över till sekundär plats**: Redundansväxla datorer från den primära platsen till sekundärt.
2. **Växla tillbaka från den sekundära platsen**: replikera virtuella datorer från sekundär till primär och kör en planerad redundans ska återställas.
3. Efter den planerade redundansväxlingen om du vill starta replikering från den primära platsen för sekundärt igen.


## <a name="fail-over-to-a-secondary-site"></a>Växla över till en sekundär plats

### <a name="failover-prerequisites"></a>Krav för redundans

Kontrollera att du har slutfört en [disaster recovery ökad](tutorial-dr-drill-secondary.md) och kontrollera att allt fungerar som förväntat.


### <a name="run-a-failover-from-primary-to-secondary"></a>Kör en redundansväxling från primär till sekundär

Du kan köra en vanlig eller planerad redundans för Hyper-V virtuella datorer.

- Använda en vanlig redundans för oväntade avbrott. När du kör den här redundansen Site Recovery skapar en virtuell dator på den sekundära platsen och används upp. Du kör redundansväxlingen mot en specifik återställningspunkt. Data kan gå förlorade beroende på den återställningspunkt som du använder.
- En planerad redundans kan användas för underhåll, eller under förväntade avbrott. Det här alternativet ger noll dataförlust. När en planerad redundans utlöses är virtuella källdatorer avstängda. Osynkroniserade data synkroniseras och växling vid fel utlöses. 
- 
Den här proceduren beskriver hur du kör en vanlig redundans.


1. I **inställningar** > **replikerade objekt** klickar du på den virtuella datorn > **redundans**.
2. I **redundans** väljer en **återställningspunkt** att växla över till. Du kan använda något av följande alternativ:
    - **Senaste** (standard): det här alternativet först bearbetar alla data som skickas till Site Recovery. De ger den lägsta RPO (mål för återställningspunkt) eftersom replikerade virtuella datorn skapas efter växling vid fel har alla data som har replikerats till Site Recovery när redundans utlöstes.
    - **Senaste bearbetas**: det här alternativet växlar den virtuella datorn till den senaste återställningspunkten som bearbetas av Site Recovery. Det här alternativet ger en låga RTO (mål), eftersom ingen tid läggs obearbetade data bearbetas.
    - **Senaste programkonsekventa**: det här alternativet växlar den virtuella datorn till den senaste programkonsekventa återställningspunkten bearbetas av Site Recovery. 
3. Krypteringsnyckeln är inte relevant i det här scenariot.
4. Välj **Stäng datorn innan du påbörjar redundans** om du vill använda Site Recovery att göra en avstängning av virtuella källdatorer innan växling vid fel. Site Recovery försöker synkronisera lokala data som ännu inte har skickats till den sekundära platsen innan växling vid fel. Observera att redundansväxlingen fortsätter även om avstängning misslyckas. Du kan följa förloppet för växling vid fel på den **jobb** sidan.
5. Nu bör du kunna se den virtuella datorn i sekundära VMM-molnet.
6. När du har kontrollerat VM, **genomför** växling vid fel. Detta tar bort alla tillgängliga återställningspunkter.

> [!WARNING]
> **Inte avbryta en växling pågår**: innan redundans startas VM replikeringen har stoppats. Om du avbryter en växling pågår, stoppar för växling vid fel, men den virtuella datorn inte kommer att replikeras igen.  


## <a name="reprotect-and-fail-back-from-secondary-to-primary"></a>Skydda igen och växla från sekundär primär

### <a name="prerequisites-for-failback"></a>Krav för återställning efter fel

Kontrollera att de primära och sekundära VMM-servrarna är anslutna till Site Recovery för att slutföra återställning efter fel.


### <a name="reprotect-and-fail-back"></a>Skydda igen och återställas

Starta replikering från den sekundära platsen till den primära servern och växla tillbaka till den primära platsen. När virtuella datorer som körs på den primära platsen igen, kan du replikera dem till den sekundära platsen igen.  

1. I **inställningar** > **replikerade objekt** klickar du på den virtuella datorn och aktivera **omvänd replikera**. Den virtuella datorn börjar replikeras tillbaka till den primära platsen.
2. Klicka på den virtuella datorn > **planerad redundans**.
3. I **bekräfta planerad redundans**kontrollerar redundansriktning (från sekundär VMM-moln) och välj käll- och målplatserna. 
4. I **datasynkronisering**, ange hur du vill synkronisera:
    - **Synkronisera data innan redundans (synkronisera endast deltaändringar)**– det här alternativet minimerar avbrottstid för virtuell dator eftersom det synkroniserar utan att stänga av den virtuella datorn. Här är syfte:
        - Tar en ögonblicksbild av replikerade virtuella datorn och kopierar den till den primära Hyper-V-värden. Repliken VM körs.
        - Stänger replikerade virtuella datorn, så att inga nya ändringar sker det. Den slutgiltiga uppsättningen deltaändringar överförs till den primära platsen och den virtuella datorn i den primära platsen har startats.
    - **Synkronisera data under växling vid fel endast (fullständig hämtning)**– Använd det här alternativet om du har kört på den sekundära platsen för lång tid. Det här alternativet är snabbare, eftersom vi räknar med flera disk ändringar och inte ägna tid åt kontrollsumma beräkningar. Det här alternativet utför en disk hämtning. Det är också användbart när den primära virtuella datorn har tagits bort.
5. Krypteringsnyckeln är inte relevant i det här scenariot.
6. Påbörja redundans. Du kan följa förloppet för växling vid fel på den **jobb** fliken.
7. Om du har valt för att synkronisera data innan växling vid fel, efter den inledande datasynkroniseringen är klar och du är redo att stänga av replikerade virtuella datorn på den sekundära platsen och klickar på **jobb** > planerad redundans jobbnamn >  **Slutför Redundansväxlingen**. Detta stängs av den sekundära virtuella datorn, överförs de senaste ändringarna till den primära platsen och startar den primära virtuella datorn.
8. Kontrollera att den virtuella datorn är tillgänglig i det primära molnet i VMM.
9. Den primära virtuella datorn är nu i ett bekräfta väntande tillstånd. Klicka på **Commit**, för att genomföra redundans.
10. Om du vill starta replikera den primära virtuella datorn tillbaka till den sekundära platsen igen genom att aktivera **omvänd replikera**.


> [!NOTE]
> Omvänd replikering replikeras endast ändringar som har gjorts sedan repliken VM har varit avstängd och skickas endast deltaändringar.

