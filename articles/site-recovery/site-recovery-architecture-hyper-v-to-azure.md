---
title: Hur fungerar Hyper-V-replikering till Azure i Azure Site Recovery? | Microsoft Docs
description: "Den här artikeln innehåller en översikt över komponenter och arkitektur som används för att replikera lokala virtuella datorer med Hyper-V till Azure med tjänsten Azure Site Recovery."
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
ms.date: 06/23/2017
ms.author: raynew
ms.openlocfilehash: 28f775afaf72b11eec0c22f755e4dbd6a485c895
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-does-hyper-v-replication-to-azure-work-in-site-recovery"></a>Hur fungerar Hyper-V-replikering till Azure i Site Recovery?


Den här artikeln beskriver de komponenter och processer som är inblandade vid replikering av lokala virtuella Hyper-V-datorer till Azure med hjälp av tjänsten [Azure Site Recovery](site-recovery-overview.md).

Site Recovery kan replikera virtuella Hyper-V-datorer i Hyper-V-kluster och fristående värdar som hanteras med eller utan System Center Virtual Machine Manager (VMM).

Skriv dina kommentarer längst ned i den här artikeln eller i [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="architectural-components"></a>Arkitekturkomponenter

Det finns ett antal komponenter ingår vid replikering av virtuella Hyper-V-datorer till Azure.

**Komponent** | **Plats** | **Detaljer**
--- | --- | ---
**Azure** | I Azure behöver du ett Microsoft Azure-konto, ett Azure-lagringskonto och ett Azure-nätverk. | Replikerade data lagras i lagringskontot och virtuella Azure-datorer skapas med replikerad data när redundansväxling uppstår från din lokala plats.<br/><br/> Virtuella Azure-datorer ansluter till det virtuella Azure-nätverket när de skapas.
**VMM-server** | Hyper-V-värdar finns i VMM-moln | Om Hyper-V-värdar hanteras i VMM-moln kan du registrera VMM-servern i Recovery Services-valvet.<br/><br/> På VMM-servern installerar du Site Recovery-providern för att samordna replikeringen med Azure.<br/><br/> Du behöver logiska nätverk och VM-nätverk för att konfigurera nätverksmappning. Ett nätverk för virtuella datorer bör vara kopplat till ett logiskt nätverk som är associerat med molnet.
**Hyper-V-värd** | Hyper-V-värdar och -kluster kan distribueras med eller utan VMM-servern. | Om det inte finns någon VMM-server installeras Site Recovery-providern på värden för att dirigera replikering med Site Recovery via internet. Om det finns en VMM-server installeras providern på den och inte på värden.<br/><br/> Recovery Services-agenten installeras på värden för att hantera datareplikering.<br/><br/> Kommunikation från både providern och agenten är säker och krypterad. Replikerade data i Azure-lagring krypteras också.
**Virtuella Hyper-V-datorer** | Du behöver en eller flera virtuella datorer på Hyper-V-värdservern. | Du behöver inte installera något på de virtuella datorerna.

Lär dig mer om kraven för distribution och krav för var och en av komponenterna i [supportmatrisen](site-recovery-support-matrix-to-azure.md).

**Bild 1: Replikering från Hyper-V-plats till Azure**

![Komponenter](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Bild 2: Replikering från Hyper-V i VMM-moln till Azure**

![Komponenter](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)


## <a name="replication-process"></a>Replikeringsprocessen

**Bild 3: Replikering och återställningsprocess för Hyper-V-replikering till Azure**

![arbetsflöde](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

### <a name="enable-protection"></a>Aktivera skydd

1. När du har aktiverat skydd för en virtuell Hyper-V-dator i Azure Portal eller lokalt startar **Aktivera skydd**.
2. Jobbet kontrollerar att datorn uppfyller kraven och anropar sedan metoden [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) som konfigurerar replikering med de inställningar som du har konfigurerat.
3. Jobbet startar den initiala replikeringen genom att aktivera metoden [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) för att initiera en fullständig VM-replikering och skicka de virtuella datorernas diskar till Azure.
4. Du kan övervaka jobbet på fliken **Jobs** (Jobb).
        ![Lista över jobb](media/site-recovery-hyper-v-azure-architecture/image1.png) ![Detaljnivå för Aktivera skydd](media/site-recovery-hyper-v-azure-architecture/image2.png)

### <a name="replicate-the-initial-data"></a>Replikera ursprungliga data

1. En [ögonblicksbild av en virtuell Hyper-V-dator](https://technet.microsoft.com/library/dd560637.aspx) tas när den initiala replikeringen utlöses.
2. Virtuella hårddiskar replikeras en i taget tills alla har kopierats till Azure. Det kan ta en stund, beroende på den virtuella datorns storlek och nätverkets bandbredd. Om du vill optimera nätverksanvändningen läser du [How to manage on-premises to Azure protection network bandwidth usage](https://support.microsoft.com/kb/3056159) (Hantera bandbreddsanvändning för skydd från lokal plats till Azure).
3. Om diskändringar inträffar under en pågående inledande replikering spåras dessa ändringar av spårningsverktyget för Hyper-V-replikering som Hyper-V-replikeringsloggar (.hrl). De här filerna finns i samma mapp som diskarna. Varje disk har en associerad HRL-fil som skickas till den sekundära lagringsplatsen.
4. Ögonblicksbilden och loggfilerna använder diskresurser när den inledande replikeringen pågår.
5. När den initiala replikeringen är klar tas VM-ögonblicksbilden bort. Diskförändringarna (delta) i loggen synkroniseras och sammanfogas till den överordnade disken.


### <a name="finalize-protection"></a>Slutför skyddet

1. När replikeringen har slutförts konfigurerar jobbet **Slutför skydd på den virtuella datorn** nätverksinställningar och andra inställningar som krävs efter replikeringen så att den virtuella datorn är skyddad.
    ![Slutför skyddet](media/site-recovery-hyper-v-azure-architecture/image3.png)
2. Om du replikerar till Azure kan du behöva justera inställningarna för den virtuella datorn så att den är redo för redundans. Nu kan du köra ett redundanstest för att kontrollera att allt fungerar som förväntat.

### <a name="replicate-the-delta"></a>Replikera delta

1. Efter den första replikeringen startar deltasynkroniseringen baserat på replikeringsinställningarna.
2. Spårningsverktyget för Hyper-V-replikering spårar ändringarna till en virtuell hårddisk som .hrl-filer. Varje disk som är konfigurerad för replikering har en associerad .hrl-fil. Loggen skickas till kundens lagringskonto när den inledande replikeringen är klar. När en logg överförs till Azure spåras ändringarna i den primära disken i en annan loggfil, i samma katalog.
3. Under initial och deltareplikering kan du övervaka den virtuella datorn i VM-vyn. [Läs mer](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="synchronize-replication"></a>Synkronisera replikering

1. Om deltareplikeringen misslyckas och om en fullständig replikering skulle bli för tids- och bandbreddskrävande så markeras en virtuell dator för omsynkronisering. Om HRL-filerna till exempel når 50 % av diskstorleken markeras den virtuella datorn för omsynkronisering.
2.  Vid en omsynkronisering minimeras mängden data som skickas genom att kontrollsummor beräknas för de virtuella käll- och måldatorerna varefter endast deltadata skickas. Omsynkronisering använder en segmenteringsalgoritm med fasta block där käll- och målfiler delas in i fasta segment. Kontrollsummor skapas för varje segment skapas och jämförs sedan för att avgöra vilka block från källan som ska tillämpas på målet.
3. När omsynkroniseringen är klar ska den normala deltareplikeringen återupptas. Omsynkroniseringen schemaläggs som standard för automatisk körning utanför kontorstid, men du kan synkronisera om en virtuell dator manuellt. Du kan till exempel återuppta omsynkroniseringen om ett nätverksavbrott eller något annat avbrott inträffar. Det gör du genom att välja den virtuella datorn i portalen > **Resynchronize** (Omsynkronisera).

    ![Manuell omsynkronisering](media/site-recovery-hyper-v-azure-architecture/image4.png)


### <a name="retry-logic"></a>Logik för omprövning

Om det uppstår ett replikeringsfel finns det en inbyggd funktion som gör ett nytt försök. Den här logiken kan delas in i två kategorier:

**Kategori** | **Detaljer**
--- | ---
**Oåterkalleliga fel** | Inga nya försök görs. Status för den virtuella datorn är **kritisk**, och administratören måste ingripa. Exempel på sådana fel är: trasig VHD-kedja, ogiltigt tillstånd för den virtuella replikdatorn, nätverksautentiseringsfel, autentiseringsfel, den virtuella datorn hittas inte (för fristående Hyper-V-servrar)
**Återkalleliga fel** | Återförsök sker varje replikeringsintervall, med ett exponentiellt undantagsläge som ökar återförsöksintervallet från det första försökets start med 1, 2, 4, 8 och 10 minuter. Om felet kvarstår försöker du var 30: e minut. Exempel: nätverksfel, för lite diskutrymme, för lite minne |



## <a name="failover-and-failback-process"></a>Processen för redundans och återställning efter fel

1. Du kan köra en planerad eller oplanerad [redundansväxling](site-recovery-failover.md) från lokala Hyper-V virtuella datorer till Azure. Om du kör en planerad redundansväxling stängs de virtuella källdatorerna av för att säkerställa att inga data går förlorade.
2. Du kan redundansväxla en enskild dator eller skapa [återställningsplaner](site-recovery-create-recovery-plans.md) för att samordna redundans för flera datorer.
4. När du har kört redundansen, ska du kunna se den skapade virtuella replikdatorn i Azure. Du kan tilldela en offentlig IP-adress till datorn om det behövs.
5. Du etablerar därefter redundansen att börja ha åtkomst till arbetsbelastningen från den virtuella Azure-replikdatorn.
6. När din primära lokala plats är tillgänglig igen, kan du [återställa dit](site-recovery-failback-from-azure-to-hyper-v.md). Du startar en planerad redundans från Azure till den primära platsen. För en planerad redundans kan du välja återställning efter fel till samma virtuella dator eller till en alternativ plats och synkronisera ändringarna mellan Azure och lokalt, så att ingen data går förlorad. När virtuella datorer skapas lokalt, etablerar du redundansen.




## <a name="next-steps"></a>Nästa steg

Granska [supportmatrisen](site-recovery-support-matrix-to-azure.md)
