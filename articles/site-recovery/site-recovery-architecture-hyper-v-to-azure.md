---
title: "aaaHow stöder Hyper-V-replikering tooAzure arbete i Azure Site Recovery? | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över komponenter och arkitektur som används när replikera lokala virtuella Hyper-V-datorer tooAzure med hello Azure Site Recovery-tjänsten."
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
ms.openlocfilehash: 3b64156307f37764a8315dec68cf297acf279530
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-hyper-v-replication-tooazure-work-in-site-recovery"></a>Hur fungerar tooAzure för Hyper-V-replikering i Site Recovery?


Den här artikeln beskriver hello komponenter och processer som är inblandade vid replikering av lokala Hyper-V virtuella datorer med hjälp av hello tooAzure [Azure Site Recovery](site-recovery-overview.md) service.

Site Recovery kan replikera virtuella Hyper-V-datorer i Hyper-V-kluster och fristående värdar som hanteras med eller utan System Center Virtual Machine Manager (VMM).

Bokför eventuella kommentarer längst ned hello av den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="architectural-components"></a>Arkitekturkomponenter

Det finns ett antal komponenter ingår när du replikerar virtuella Hyper-V-datorer tooAzure.

**Komponent** | **Plats** | **Detaljer**
--- | --- | ---
**Azure** | I Azure behöver du ett Microsoft Azure-konto, ett Azure-lagringskonto och ett Azure-nätverk. | Replikerade data lagras i hello storage-konto och virtuella Azure-datorer skapas med hello replikerade data när det uppstår redundans från din lokala plats.<br/><br/> hello Azure virtuella datorer ansluta toohello virtuella Azure-nätverket när de skapas.
**VMM-server** | Hyper-V-värdar finns i VMM-moln | Om Hyper-V-värdar hanteras i VMM-moln, registrera hello VMM-servern i hello Recovery Services-valvet.<br/><br/> På hello VMM-servern installerar du hello Site Recovery-providern tooorchestrate replikering med Azure.<br/><br/> Behöver du logiska och Virtuella datornätverk konfigurera tooconfigure nätverksmappning. Ett Virtuellt datornätverk ska vara länkade tooa logiskt nätverk som är kopplad till hello molnet.
**Hyper-V-värd** | Hyper-V-värdar och -kluster kan distribueras med eller utan VMM-servern. | Om det finns ingen VMM-server, hello hello Site Recovery-providern är installerad på hello värden tooorchestrate replikeringen med Site Recovery via internet. Om det finns en VMM-server, har hello providern installerats på den och inte på hello värden.<br/><br/> hello Recovery Services-agenten är installerad på hello värden toohandle replikering.<br/><br/> Kommunikation från både hello providern och hello-agenten är säker och krypterad. Replikerade data i Azure-lagring krypteras också.
**Virtuella Hyper-V-datorer** | Du behöver en eller flera virtuella datorer på Hyper-V-värdservern. | Behövs inga tooexplicitly som installerats på virtuella datorer.

Lär dig mer om kraven för distribution av hello och krav för var och en av komponenterna i hello [supportmatrisen](site-recovery-support-matrix-to-azure.md).

**Bild 1: Hyper-V tooAzure replikering**

![Komponenter](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Bild 2: Hyper-V i VMM-moln tooAzure replikering**

![Komponenter](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)


## <a name="replication-process"></a>Replikeringsprocessen

**Bild 3: Replikering och återställning process för tooAzure för Hyper-V-replikering**

![arbetsflöde](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

### <a name="enable-protection"></a>Aktivera skydd

1. När du aktiverar skyddet för en Hyper-V virtuell dator i hello Azure-portalen eller lokalt, hello **Aktivera skydd** startar.
2. hello jobbet kontrollerar att hello-datorn uppfyller kraven, innan du anropar hello [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), tooset konfigurerar replikering med hello-inställningar som du har konfigurerat.
3. hello jobbet startar den inledande replikeringen genom att anropa hello [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) metod, tooinitialize en fullständig replikering av Virtuella och skicka hello VM virtuella diskar tooAzure.
4. Du kan övervaka hello jobb i hello **jobb** fliken.      ![Lista över jobb](media/site-recovery-hyper-v-azure-architecture/image1.png) ![Detaljnivå för Aktivera skydd](media/site-recovery-hyper-v-azure-architecture/image2.png)

### <a name="replicate-hello-initial-data"></a>Replikera hello inledande data

1. En [ögonblicksbild av en virtuell Hyper-V-dator](https://technet.microsoft.com/library/dd560637.aspx) tas när den initiala replikeringen utlöses.
2. Virtuella hårddiskar är replikeras en i taget tills de är alla kopierade tooAzure. Det kan ta en stund, beroende på hello VM-storlek och nätverkets bandbredd. toooptimize användningen av nätverket, se [hur toomanage lokalt tooAzure skydd av nätverksbandbredden](https://support.microsoft.com/kb/3056159).
3. Om det inträffar under den inledande replikeringen pågår, spårar hello Hyper-V-replikering Diskändringar ändringarna som Hyper-V-Replikeringsloggar (.hrl). Dessa filer finns i hello samma mapp som hello diskar. Varje disk har en associerad hrl-fil som ska skickas toosecondary lagring.
4. hello ögonblicksbilden och loggfilerna använder diskresurser när den inledande replikeringen pågår.
5. Hello VM-ögonblicksbild tas bort när hello den inledande replikeringen är klar. Diskförändringarna i hello loggen är synkroniserade och sammanfogade toohello överordnad disk.


### <a name="finalize-protection"></a>Slutför skyddet

1. När hello replikeringen har slutförts, hello **Slutför skydd på den virtuella datorn hello** jobbet konfigurerar nätverks- och andra inställningar för efter replikeringen så att hello virtuella datorn är skyddad.
    ![Slutför skyddet](media/site-recovery-hyper-v-azure-architecture/image3.png)
2. Om du replikerar tooAzure behöva du tootweak hello inställningar för hello virtuella datorn så att den är redo för redundans. Du kan nu köra ett test redundans toocheck allt fungerar som förväntat.

### <a name="replicate-hello-delta"></a>Replikera hello delta

1. Efter den första replikeringen för hello startar i enlighet med inställningar för replikering.
2. hello Hyper-V-replikering Diskändringar spårar hello ändringar tooa virtuell hårddisk som hrl-filerna. Varje disk som är konfigurerad för replikering har en associerad .hrl-fil. Den här loggen skickas toohello kundens lagringskonto när den inledande replikeringen är klar. När en logg i överföringen tooAzure spåras hello ändringar i hello primär disk i en loggfil i hello samma katalog.
3. Du kan övervaka hello VM i hello VM-vyn under inledande och delta. [Läs mer](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="synchronize-replication"></a>Synkronisera replikering

1. Om deltareplikeringen misslyckas och om en fullständig replikering skulle bli för tids- och bandbreddskrävande så markeras en virtuell dator för omsynkronisering. Till exempel om hello hrl-filerna når 50% av diskstorleken hello, markeras sedan hello VM för omsynkronisering.
2.  Omsynkroniseringen minimerar hello mängden data som skickas genom att beräkna kontrollsummor för hello käll- och virtuella datorer och skicka endast hello delta-data. Omsynkronisering använder en segmenteringsalgoritm med fasta block där käll- och målfiler delas in i fasta segment. Kontrollsummor för varje segment genereras och sedan jämförs toodetermine som blockerar från hello källa måste toobe tillämpade toohello målet.
3. När omsynkroniseringen är klar ska den normala deltareplikeringen återupptas. Omsynkroniseringen är som standard schemalagda toorun automatiskt utanför kontorstid, men du kan synkronisera om en virtuell dator manuellt. Du kan till exempel återuppta omsynkroniseringen om ett nätverksavbrott eller något annat avbrott inträffar. toodo detta, Välj hello VM i hello portal > **synkronisera om**.

    ![Manuell omsynkronisering](media/site-recovery-hyper-v-azure-architecture/image4.png)


### <a name="retry-logic"></a>Logik för omprövning

Om det uppstår ett replikeringsfel finns det en inbyggd funktion som gör ett nytt försök. Den här logiken kan delas in i två kategorier:

**Kategori** | **Detaljer**
--- | ---
**Oåterkalleliga fel** | Inga nya försök görs. Status för den virtuella datorn är **kritisk**, och administratören måste ingripa. Exempel på dessa fel är: skadad VHD-enhet. Ogiltigt tillstånd för hello replikerade virtuella datorn; Nätverk autentiseringsfel: auktorisering fel. VM hittades inte-fel (för fristående Hyper-V-servrar)
**Återkalleliga fel** | Försök inträffa varje intervall för replikering med hjälp av en exponentiell inte som ökar hello återförsöksintervall från hello start av hello första försöket genom 1, 2, 4, 8 och 10 minuter. Om felet kvarstår försöker du var 30: e minut. Exempel: nätverksfel, för lite diskutrymme, för lite minne |



## <a name="failover-and-failback-process"></a>Processen för redundans och återställning efter fel

1. Du kan köra en planerad eller oplanerad [redundans](site-recovery-failover.md) från lokala virtuella Hyper-V-datorer tooAzure. Om du kör en planerad redundans och virtuella källdatorer stängs tooensure inga data går förlorade.
2. Du kan växla över en enskild dator eller skapa [återställningsplaner](site-recovery-create-recovery-plans.md) tooorchestrate växling vid fel på flera datorer.
4. När du har kört hello redundans ska kunna toosee hello skapade Replikdatorerna i Azure. Du kan tilldela en offentlig IP-adress toohello VM om det behövs.
5. Du genomför sedan hello redundans toostart åt hello arbetsbelastning från hello replik Azure VM.
6. När din primära lokala plats är tillgänglig igen, kan du [återställa dit](site-recovery-failback-from-azure-to-hyper-v.md). Startar du en planerad redundans från Azure toohello primära platsen. För planerad redundans kan du ändrar väljer toofailback toohello samma VM eller tooan alternativ plats och synkronisera utseende mellan Azure och lokalt, tooensure inga data går förlorade. När virtuella datorer skapas lokalt, genomför hello växling vid fel.




## <a name="next-steps"></a>Nästa steg

Granska hello [supportmatrisen](site-recovery-support-matrix-to-azure.md)
