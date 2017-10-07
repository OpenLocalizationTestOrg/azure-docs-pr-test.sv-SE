---
title: "aaaHow stöder Hyper-V-replikering tooAzure arbete i Site Recovery? | Microsoft Docs"
description: "I den här artikeln får du en översikt över hur Hyper-V-replikering fungerar i Azure Site Recovery"
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
redirect_url: site-recovery-architecture-hyper-v-to-azure
ms.openlocfilehash: e982806b4d6cdec2f71f82d8c73c17cc50ad3c33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-hyper-v-replication-tooazure-work"></a>Hur fungerar Hyper-V-replikering tooAzure?

Läs den här artikeln toounderstand hello arkitektur och arbetsflöden för Hyper-V-replikering tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) service.

Bokför eventuella kommentarer längst ned hello av den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Du kan replikera hello följande tooAzure:
- **Hyper-V med VMM**: Virtuella datorer som finns på en lokal Hyper-V-värd hanteras i System Center Virtual Machine Manager (VMM)-moln. Värdar kan köras på valfritt [operativsystem som stöds](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). Du kan replikera virtuella Hyper-V-datorer som körs på ett gästoperativsystem [som stöds av Hyper-V och Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).
- **Hyper-V utan VMM**: Lokala virtuella datorer som finns på Hyper-V-värdar som inte hanteras i VMM-moln. Värdar kan köra någon av hello [operativsystem](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions). Du kan replikera virtuella Hyper-V-datorer som körs på ett gästoperativsystem [som stöds av Hyper-V och Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).


## <a name="architectural-components"></a>Arkitekturkomponenter

**Område** | **Komponent** | **Detaljer**
--- | --- | ---
**Azure** | I Azure behöver du ett Microsoft Azure-konto, ett Azure-lagringskonto och ett Azure-nätverk. | Lagring och nätverk kan vara Resource Manager-baserade eller klassiska konton.<br/><br/> Replikerade data lagras i hello storage-konto och virtuella Azure-datorer skapas med hello replikerade data när det uppstår redundans från din lokala plats.<br/><br/> hello Azure virtuella datorer ansluta toohello virtuella Azure-nätverket när de skapas.
**VMM-server** | Hyper-V-värdar i VMM-moln | Om Hyper-V-värdar hanteras i VMM-moln, registrera hello VMM-servern i hello Recovery Services-valvet.<br/><br/> På hello VMM-servern installerar du hello Site Recovery-providern tooorchestrate replikering med Azure.<br/><br/> Behöver du logiska och Virtuella datornätverk konfigurera tooconfigure nätverksmappning. Ett Virtuellt datornätverk ska vara länkade tooa logiskt nätverk som är kopplad till hello molnet.
**Hyper-V-värd** | Hyper-V-servrar kan distribueras med eller utan VMM-servern. | Om det finns ingen VMM-server, hello hello Site Recovery-providern är installerad på hello värden tooorchestrate replikeringen med Site Recovery via internet. Om det finns en VMM-server, har hello providern installerats på den och inte på hello värden.<br/><br/> hello Recovery Services-agenten är installerad på hello värden toohandle replikering.<br/><br/> Kommunikation från både hello providern och hello-agenten är säker och krypterad. Replikerade data i Azure-lagring krypteras också.
**Virtuella Hyper-V-datorer** | Du behöver en eller flera virtuella datorer på hello Hyper-V-värdservern. | Behövs inga tooexplicitly som installerats på virtuella datorer

## <a name="deployment-steps"></a>Distributionssteg

1. **Azure**: du ställer in hello Azure komponenter. Vi rekommenderar att du ställer in konton för lagring och nätverk innan du påbörjar Site Recovery-distributionen.
2. **Valv**: Du skapar ett Recovery Services-valv för Site Recovery och konfigurerar valvinställningarna, däribland inställningar för källa och mål, konfiguration av en replikeringsprincip och aktivering av replikering.
3. **Källa och mål**:
    - **Hyper-V-värdar i VMM-moln**: som en del av att ange inställningar för datakälla kan du hämta och installera hello Azure Site Recovery Provider på hello VMM-servern och hello Azure Recovery Services-agenten på varje Hyper-V-värd. hello datakälla kommer att hello VMM-servern. hello målet är Azure.
    - Hyper-V-värdar utan VMM: när du anger inställningar för datakälla kan du hämta och installera hello providern och agenten på varje Hyper-V-värd. Under distributionen samla hello värdar i ett Hyper-V-platsen och ange platsen som hello källa. hello målet är Azure.

    ![VMM-Hyper-V-replikering tooAzure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png) ![tooAzure för Hyper-V plats-replikering](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


4. **Replikeringsprincip**: du kan skapa en replikeringsprincip för hello Hyper-V-plats eller VMM-moln. hello principen är tillämpade tooall virtuella datorer på värdar i hello plats eller molnet.
5. **Aktivera replikering**: Du aktiverar replikering för virtuella Hyper-V-datorer. Den inledande replikeringen sker i enlighet med hello replikeringsprincipens inställningar. Dataändringar spåras och replikering av delta ändringar tooAzure börjar när hello replikeringen har slutförts. Spårade ändringar för ett objekt lagras i en .hrl-fil.
6. **Testa redundans**: du kör ett test redundans toomake att allt fungerar som förväntat.

Lär dig mer om distribution:
- [Komma igång med Hyper-V VM replikering tooAzure - med VMM](site-recovery-vmm-to-azure.md)
- [Komma igång med Hyper-V VM replikering tooAzure - utan VMM](site-recovery-hyper-v-site-to-azure.md)

## <a name="hyper-v-replication-workflow"></a>Arbetsflöde för Hyper-V-replikering

### <a name="enable-protection"></a>Aktivera skydd

1. När du aktiverar skyddet för en Hyper-V virtuell dator i hello Azure-portalen eller lokalt, hello **Aktivera skydd** startar.
2. hello jobbet kontrollerar att hello-datorn uppfyller kraven, innan du anropar hello [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), tooset konfigurerar replikering med hello-inställningar som du har konfigurerat.
3. hello jobbet startar den inledande replikeringen genom att anropa hello [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) metod, tooinitialize en fullständig replikering av Virtuella och skicka hello VM virtuella diskar tooAzure.
4. Du kan övervaka hello jobb i hello **jobb** fliken.      ![Lista över jobb](media/site-recovery-hyper-v-azure-architecture/image1.png) ![Detaljnivå för Aktivera skydd](media/site-recovery-hyper-v-azure-architecture/image2.png)

### <a name="initial-replication"></a>Inledande replikering

1. En [ögonblicksbild av en virtuell Hyper-V-dator](https://technet.microsoft.com/library/dd560637.aspx) tas när den initiala replikeringen utlöses.
2. Virtuella hårddiskar är replikeras en i taget tills de är alla kopierade tooAzure. Det kan ta en stund, beroende på hello VM-storlek och nätverkets bandbredd. toooptimize användningen av nätverket, se [hur toomanage lokalt tooAzure skydd av nätverksbandbredden](https://support.microsoft.com/kb/3056159).
3. Om det inträffar under den inledande replikeringen pågår, spårar hello Hyper-V-replikering Diskändringar ändringarna som Hyper-V-Replikeringsloggar (.hrl). Dessa filer finns i hello samma mapp som hello diskar. Varje disk har en associerad hrl-fil som ska skickas toosecondary lagring.
4. hello ögonblicksbilden och loggfilerna använder diskresurser när den inledande replikeringen pågår.
5. Hello VM-ögonblicksbild tas bort när hello den inledande replikeringen är klar. Diskförändringarna i hello loggen är synkroniserade och sammanfogade toohello överordnad disk.


### <a name="finalize-protection"></a>Slutför skyddet

1. När hello replikeringen har slutförts, hello **Slutför skydd på den virtuella datorn hello** jobbet konfigurerar nätverks- och andra inställningar för efter replikeringen så att hello virtuella datorn är skyddad.
    ![Slutför skyddet](media/site-recovery-hyper-v-azure-architecture/image3.png)
2. Om du replikerar tooAzure behöva du tootweak hello inställningar för hello virtuella datorn så att den är redo för redundans. Du kan nu köra ett test redundans toocheck allt fungerar som förväntat.

### <a name="delta-replication"></a>Deltareplikering

1. Efter den första replikeringen för hello startar i enlighet med inställningar för replikering.
2. hello Hyper-V-replikering Diskändringar spårar hello ändringar tooa virtuell hårddisk som hrl-filerna. Varje disk som är konfigurerad för replikering har en associerad .hrl-fil. Den här loggen skickas toohello kundens lagringskonto när den inledande replikeringen är klar. När en logg i överföringen tooAzure spåras hello ändringar i hello primär disk i en loggfil i hello samma katalog.
3. Du kan övervaka hello VM i hello VM-vyn under inledande och delta. [Läs mer](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="replication-synchronization"></a>Replikeringssynkronisering

1. Om deltareplikeringen misslyckas och om en fullständig replikering skulle bli för tids- och bandbreddskrävande så markeras en virtuell dator för omsynkronisering. Till exempel om hello hrl-filerna når 50% av diskstorleken hello, markeras sedan hello VM för omsynkronisering.
2.  Omsynkroniseringen minimerar hello mängden data som skickas genom att beräkna kontrollsummor för hello käll- och virtuella datorer och skicka endast hello delta-data. Omsynkronisering använder en segmenteringsalgoritm med fasta block där käll- och målfiler delas in i fasta segment. Kontrollsummor för varje segment genereras och sedan jämförs toodetermine som blockerar från hello källa måste toobe tillämpade toohello målet.
3. När omsynkroniseringen är klar ska den normala deltareplikeringen återupptas. Omsynkroniseringen är som standard schemalagda toorun automatiskt utanför kontorstid, men du kan synkronisera om en virtuell dator manuellt. Du kan till exempel återuppta omsynkroniseringen om ett nätverksavbrott eller något annat avbrott inträffar. toodo detta, Välj hello VM i hello portal > **synkronisera om**.

    ![Manuell omsynkronisering](media/site-recovery-hyper-v-azure-architecture/image4.png)


### <a name="retries"></a>Antal försök

Om det uppstår ett replikeringsfel finns det en inbyggd funktion som gör ett nytt försök. Den här logiken kan delas in i två kategorier:

**Kategori** | **Detaljer**
--- | ---
**Oåterkalleliga fel** | Inga nya försök görs. Status för den virtuella datorn är **kritisk**, och administratören måste ingripa. Exempel på dessa fel är: skadad VHD-enhet. Ogiltigt tillstånd för hello replikerade virtuella datorn; Nätverk autentiseringsfel: auktorisering fel. VM hittades inte-fel (för fristående Hyper-V-servrar)
**Återkalleliga fel** | Försök inträffa varje intervall för replikering med hjälp av en exponentiell inte som ökar hello återförsöksintervall från hello start av hello första försöket genom 1, 2, 4, 8 och 10 minuter. Om felet kvarstår försöker du var 30: e minut. Exempel: nätverksfel, för lite diskutrymme, för lite minne |

## <a name="protection-and-recovery-lifecycle"></a>Livscykel för skydd och återställning

![arbetsflöde](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Nästa steg

- [Kontrollera förhandskrav för distribution](site-recovery-prereq.md)
- Felsök med:
    - [Övervaka och felsöka skydd](site-recovery-monitoring-and-troubleshooting.md)
    - [Hjälp från Microsoft Support](site-recovery-monitoring-and-troubleshooting.md#reach-out-for-microsoft-support)
    - [Vanliga fel och lösningar](site-recovery-monitoring-and-troubleshooting.md#common-azure-site-recovery-errors-and-their-resolutions)
