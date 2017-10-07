---
title: "aaaFailback i Azure Site Recovery för Hyper-v-datorer | Microsoft Docs"
description: "Azure Site Recovery samordnar hello replikering, redundans och återställning av virtuella datorer och fysiska servrar. Mer information om återställning efter fel från Azure tooon lokala datacenter."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 50cda9105de6b6fb23e4c62942fdaffc55c3efa4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="failback-in-site-recovery-for-hyper-v-virtual-machines"></a>Återställning efter fel i Site Recovery för Hyper-V virtuella datorer

Den här artikeln beskriver hur toofailback virtuella datorer som skyddas av Site Recovery.

## <a name="prerequisites"></a>Krav
1. Kontrollera att hello primär plats VMM-servern/Hyper-V-servern är ansluten.
2. Du bör ha utfört **genomför** på hello virtuella datorn.

## <a name="why-is-there-no-button-called-failback"></a>Varför finns det ingen knapp som kallas återställning efter fel?
På hello portal finns det ingen explicit gest kallas återställning efter fel. Återställning är ett steg där du kommer tillbaka toohello primär plats. Per definition är växling vid fel när du redundans hello virtuella datorer från primary(on-premises) plats toorecovery (Azure) och återställning när du redundans hello virtuella datorer från recovery tillbaka tooprimary.

När du påbörja en växling informerar hello bladet om hello riktning hello jobb. Om hello riktningen är från Azure tooOn lokala, är det en återställning efter fel.

## <a name="why-is-there-only-a-planned-failover-gesture-toofailback"></a>Varför är det bara en planerad redundans gest toofailback?
Azure är en miljö med hög tillgänglighet och virtuella datorer kommer alltid att vara tillgängliga. Återställning är en planerad aktivitet kan du välja tootake ett litet driftstopp så att hello arbetsbelastningar kan starta körs lokalt igen. Detta förväntar sig inga data går förlorade. Därför bara en planerad redundans gest är tillgänglig, som inaktiverar hello virtuella datorer i Azure, hämta hello senaste ändringarna och se till att det finns inga data går förlorade.

## <a name="initiate-failback"></a>Initiera återställning efter fel
Den replikerade virtuella datorer som inte skyddas av Site Recovery efter växling från hello primära toosecondary plats, och hello sekundär plats nu fungerar som hello aktiva plats. Följ dessa procedurer toofail tillbaka toohello ursprungliga primär plats. Den här proceduren beskriver hur toorun en planerad redundans för återställning av en plan. Du kan också köra hello redundans för en enskild virtuell dator på hello **virtuella datorer** fliken.

1. Välj **Återställningsplaner** > *recoveryplan_name*. Klicka på **redundans** > **planerad redundans**.
2. På hello ** bekräfta planerad redundans ** väljer hello käll- och målplatserna. Observera hello redundansriktning. Om hello redundansväxling från primär fungerade som förväntat och alla virtuella datorer finns i hello sekundär plats, detta är endast i informationssyfte.
3. Om du växlar tillbaka från Azure väljer inställningar i **datasynkronisering**:

   * **Synkronisera data innan redundans (synkronisera deltaändringar endast)**– det här alternativet minimerar avbrottstid för virtuella datorer som det synkroniserar utan att de stängs av. Det hello följande:
     * Fas 1: Tar en ögonblicksbild av hello virtuell dator i Azure och kopieras toohello lokala Hyper-V-värd. hello datorn fortsätter att köra i Azure.
     * Fas 2: Stängs hello virtuell dator i Azure så att inga nya ändringar sker det. hello slutgiltiga uppsättningen deltaändringar är överförda toohello lokal server och hello lokala virtuella datorn har startats.

    - **Synkronisera data under växling vid fel endast (fullständig hämtning)**– Använd det här alternativet om du har kört i Azure under lång tid. Det här alternativet är snabbare eftersom vi räknar med att de flesta av hello disken har ändrats och bör inte toospend tid i beräkningen av kontrollsumma. Den genomför en hämtning av hello disk. Det är också användbart när hello lokala virtuella datorn har tagits bort.

    >[!NOTE]
    >Vi rekommenderar att du använder det här alternativet om du har kört Azure på ett tag (en månad eller längre) eller hello lokala virtuella datorn har tagits bort. Det här alternativet utföra inte några beräkningar av kontrollsumma.
    >
    >




4. Om datakryptering är aktiverat för hello moln i **krypteringsnyckeln** väljer hello-certifikat som utfärdades när du har aktiverat datakryptering under providerinstallationen på hello VMM-servern.
5. Initiera hello växling vid fel. Du kan följa redundansförloppet hello hello **jobb** fliken.
6. Om du har valt alternativet hello toosynchronize hello data innan hello redundans, en gång hello inledande datasynkronisering är klar och du är redo tooshut ned hello virtuella datorer i Azure, klickar du på **jobb** jobbnamn för planerad redundans **Slutför redundans**. Detta avslutar hello Azure dator, överföringar hello senaste ändras toohello lokala virtuella datorn och startar hello VM lokala.
7. Du kan nu logga till hello virtuella toovalidate är tillgängliga som förväntat.
8. hello virtuella datorn är i en bekräftelse av väntande tillstånd. Klicka på **genomför** toocommit hello redundans.
9. Nu i ordning toocomplete hello återställning klickar du på **omvänd replikera** toostart skyddar hello virtuell dator i hello primär plats.

## <a name="failback-tooan-alternate-location"></a>Återställning efter fel tooan alternativ plats
Om du har distribuerat skydd mellan en [Hyper-V-platsen och Azure](site-recovery-hyper-v-site-to-azure.md) du har tooability toofailback från Azure tooan alternativ lokal plats. Detta är användbart om du behöver tooset upp ny lokal maskinvara. Här är hur du gör.

1. Om du ställer in ny maskinvara installera Windows Server 2012 R2 och hello Hyper-V-rollen på hello-servern.
2. Skapa en virtuell nätverksswitch med hello samma namn som användes på hello ursprungliga servern.
3. Välj **skyddade objekt** -> **Skyddsgrupp**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> du vill toofail tillbaka och väljer **planerad Redundans**.
4. I **bekräfta planerad redundans** Välj **skapa lokala virtuella datorn om det inte finns**.
5. I **värdnamn** Välj hello nya Hyper-V-värdservern som du vill tooplace hello virtuella datorn.
6. I datasynkronisering rekommenderar vi du väljer alternativet hello **synkronisera hello data innan hello redundans**. Detta minimerar avbrottstid för virtuella datorer som det synkroniserar utan att de stängs av. Det hello följande:

   * Fas 1: Tar en ögonblicksbild av hello virtuell dator i Azure och kopieras toohello lokala Hyper-V-värd. hello datorn fortsätter att köra i Azure.
   * Fas 2: Stängs hello virtuell dator i Azure så att inga nya ändringar sker det. hello slutgiltiga uppsättningen med ändringarna är överförda toohello lokal server och hello lokala virtuella datorn har startats.
7. Klicka på hello markering toobegin hello växling vid fel (återställning).
8. När hello inledande synkroniseringen är klar och du är redo tooshut ned hello virtuell dator i Azure, klickar du på **jobb** > <planned failover job> > **slutföra växling vid fel**. Detta avslutar hello Azure dator, överföringar hello senaste ändringar toohello lokala virtuella datorn och startar den.
9. Du kan logga in på hello lokala virtuella tooverify allt fungerar som förväntat. Klicka på **genomför** toofinish hello redundans.
10. Klicka på **omvänd replikera** toostart skyddar hello lokala virtuella datorn.

    > [!NOTE]
    > Om du avbryter hello återställningsjobb när den är i datasynkronisering steg hello lokala virtuella datorn kommer att vara i ett skadat tillstånd. Detta beror på att synkronisering kopierar hello senaste data från Azure VM diskar toohello lokala diskar och tills hello synkroniseringen har slutförts hello diskdata kanske inte i ett konsekvent tillstånd. Om hello lokala VM startas när synkronisering har avbrutits, kan den inte startas. Ny utlösare redundans toocomplete hello datasynkronisering.
    >
    >



## <a name="next-steps"></a>Nästa steg

När du har slutfört hello återställningsjobb **genomför** hello virtuell dator. Commit hello virtuella Azure-datorn och dess diskar tas bort och förbereder hello VM toobe skyddad igen.

Efter **genomför**, kan du initiera hello *omvänd replikera*. Skydda hello virtuell dator från lokal tillbaka tooAzure startas. Observera att detta kommer att enbart replikera hello ändringar eftersom hello VM har inaktiverats i Azure och därför skickar differentiell endast ändringar.
