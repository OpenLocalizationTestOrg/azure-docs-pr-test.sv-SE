---
title: "aaaEnable replikering för virtuella datorer i Azure tooanother Azure-region med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooenable replikering tooanother Azure-region för virtuella Azure-datorer med hjälp av hello Azure Site Recovery-tjänsten"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a309644f-d36b-4188-bba7-ad45a2d9bede
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 8/01/2017
ms.author: raynew
ms.openlocfilehash: 2fa03db45a18ccb8b9f31ed05589be0dd6d5f031
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-enable-replication-for-azure-vms"></a>Steg 5: Aktivera replikering för virtuella Azure-datorer


När du har installerat en [Recovery Services-valvet](azure-to-azure-walkthrough-vault.md), använda den här artikeln tooenable replikering av virtuella datorer (VM), tooanother Azure-region med [Azure Site Recovery](site-recovery-overview.md). tooenable replikering, konfigurera inställningar för källa och mål, kontrollera hello replikeringsprinciper och välj virtuella datorer du vill tooreplicate.

- När du har virtuella datorerna i Azure-hello artikeln bör replikerar toohello sekundära Azure-region.
- Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

>[!NOTE]
>
> Azure VM-replikering är för närvarande under förhandsgranskning.


## <a name="select-hello-source"></a>Välj hello källa 

1. Klicka på valvnamnet för hello i Recovery Services-valv > **+ replikera**.
2. I **källa**väljer **Azure - PREVIEW**.
2. I **datakällplats**väljer hello källa Azure-region där din virtuella dator körs för närvarande.
3. Välj hello **virtuella Azure-datorn distributionsmodell** för virtuella datorer: **Resource Manager** eller **klassiska**.
4. Välj hello **källresursgruppen** för hanteraren för virtuella datorer, eller **Molntjänsten** för klassiska virtuella datorer.
5. Klicka på **OK** toosave hello inställningar.

    ![Konfigurera hello källa](./media/azure-to-azure-walkthrough-enable-replication/source.png)

## <a name="select-hello-vms"></a>Välj hello virtuella datorer

Site Recovery hämtar en lista över hello virtuella datorer som är associerade med hello prenumeration och resurs grupp eller ett moln-tjänsten.

1. I **virtuella datorer**, Välj hello virtuella datorer du vill tooreplicate.
2. Klicka på **OK**.

    ![Välj virtuella datorer](./media/azure-to-azure-walkthrough-enable-replication/vms.png)


## <a name="configure-settings"></a>Konfigurera inställningar

Site Recovery tillhandahåller standardinställningarna för hello målregionen (baserat på inställningarna för hello datakälla region), och hello replikeringsprincip:

   - **Målplatsen**: hello målregionen som du vill använda toouse för katastrofåterställning. Vi rekommenderar att hello målplats matchar hello platsen för hello Site Recovery-valvet.
   - **Målresursgruppen**: resurs grupp toowhich Azure virtuella datorer i hello målregionen hör efter växling vid fel. Som standard skapar Site Recovery en ny resursgrupp i hello målregionen med ett ”asr” suffix. 
   - **Virtuella målnätverket**: hello nätverk i vilka Azure virtuella datorer i hello målregionen kommer att finnas efter växling vid fel. Som standard skapar Site Recovery ett nytt virtuellt nätverk (och undernät) i hello målregionen med ett ”asr” suffix. Det här nätverket är mappade tooyour Källnätverk. Observera att du kan tilldela en specifik IP-adress efter växling vid fel på en virtuell dator, om du behöver tooretain hello samma IP-adressen i hello käll- och målplatserna. 
   - **Cachelagra lagringskonton**: Site Recovery använder ett lagringskonto i hello källa region. Ändringar på virtuella källdatorer skickas toothis konto innan replikeringen toohello målplats. 
   - **Rikta lagringskonton**: som standard skapar Site Recovery ett nytt lagringskonto i hello målregionen, toomirror hello källa VM storage-konto.
   -  **Rikta tillgänglighetsuppsättningar**: som standard skapar en ny tillgänglighetsuppsättning i hello målregionen med hello ”asr” suffix Site Recovery. 
   - **Namn på**: principnamn.
   - **Kvarhållningstid för återställningspunkten**: som standard sparas Site Recovery återställningspunkter under 24 timmar. Du kan konfigurera ett värde mellan 1 och 72 timmar.
   - **Frekvens av programkonsekventa ögonblicksbilder**: som standard Site Recovery tar en ögonblicksbild av programkonsekventa var fjärde timme. Du kan konfigurera ett värde mellan 1 och 12 timmar. Data replikeras kontinuerligt:
    - Kraschkonsekvent återställningspunkter upprätthålla konsekventa data write-ordning när de skapas. Den här typen av återställningspunkt räcker vanligtvis om din app är utformad toorecover från en krasch utan inkonsekventa data
    - Kraschkonsekvent återställningspunkter skapas med några minuters mellanrum. Med dessa recovery punkter toofail över och Återställ din virtuella dator innehåller hello efter minuter en återställning punkt mål (RPO).
    - Programkonsekventa återställningspunkter (i tillägget toowrite ordning konsekvenskontroll) kontrollera att appar körs slutföra alla åtgärder och rensa buffertar toodisk (programmet offline stegvis). Vi rekommenderar att du använder dessa återställningspunkter för databas-appar som SQL Server, Oracle och Exchange.
        
    ![Konfigurera inställningar](./media/azure-to-azure-walkthrough-enable-replication/settings.png)


### <a name="modify-settings"></a>Ändra inställningar

Om du vill toomodify mål och replikering principinställningar hello följande:

1. tooview eller ändra inställningarna för målet, klickar på **inställningar**.
2. toooverride hello mål standardinställningarna klickar du på **anpassa**. Du kan ange en target-resursgruppen, virtuella nätverk, tillgänglighetsuppsättning och mål-lagringskontot. Du kan bara lägga till tillgänglighetsuppsättningar om virtuella datorer som är en del av en uppsättning i hello källa region.

    ![Konfigurera inställningar](./media/azure-to-azure-walkthrough-enable-replication/customize-target.png)

3. toooverride replikeringsinställningar för återställningspunkter och programkonsekventa ögonblicksbilder klickar du på **anpassa** nästa för**Replikeringsprincipen**.
 
    ![Konfigurera inställningar](./media/azure-to-azure-walkthrough-enable-replication/customize-policy.png)

4. toostart etablering hello target-resurser, klickar du på **skapa målresurser**. Etablering tar en minut. Stäng inte hello bladet under etableringen eller du har toostart över.




## <a name="enable-replication"></a>Aktivera replikering

1. I **inställningar**, klickar du på **Aktivera replikering**. Detta gör det möjligt för inledande replikering av hello virtuella datorer som du har valt. Status för den inledande replikeringen kan ta viss tid toorefresh. Klicka på **uppdatera** tooget hello senaste status.

2. Du kan följa förloppet för hello **Skyddsaktivering** jobb **inställningar** > **jobb** > **Site Recovery-jobb**.

3. I **inställningar** > **replikerade objekt**, kan du visa hello status för virtuella datorer och hello inledande replikering pågår. Klicka på hello VM toodrill ned till dess inställningar.



## <a name="next-steps"></a>Nästa steg

Gå för[steg 6: köra ett redundanstest](azure-to-azure-walkthrough-test-failover.md)
