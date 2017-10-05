---
title: "Aktivera replikering för virtuella Azure-datorer till en annan Azure-region med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattas de steg som du måste aktivera replikering till en annan Azure-region för virtuella Azure-datorer med hjälp av Azure Site Recovery-tjänsten"
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
ms.openlocfilehash: f426ba4341e61d3c7da820d7d5097b217e94ca0e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="step-5-enable-replication-for-azure-vms"></a>Steg 5: Aktivera replikering för virtuella Azure-datorer


När du har installerat en [Recovery Services-valvet](azure-to-azure-walkthrough-vault.md), Använd den här artikeln för att aktivera replikering av virtuella datorer (VM) till en annan Azure-regionen med [Azure Site Recovery](site-recovery-overview.md). Om du vill aktivera replikering, konfigurera inställningar för källa och mål, kontrollera replikeringsprinciper och välj virtuella datorer du vill replikera.

- När du är klar i artikeln ditt virtuella Azure-datorer ska replikeras till den sekundära regionen för Azure.
- Skicka kommentarer längst ned i den här artikeln eller ställa frågor i den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

>[!NOTE]
>
> Azure VM-replikering är för närvarande under förhandsgranskning.


## <a name="select-the-source"></a>Välj källa 

1. Klicka på valvnamnet i Recovery Services-valv > **+ replikera**.
2. I **källa**väljer **Azure - PREVIEW**.
2. I **datakällplats**, väljer du den Azure-region där din virtuella dator körs för närvarande.
3. Välj den **virtuella Azure-datorn distributionsmodell** för virtuella datorer: **Resource Manager** eller **klassiska**.
4. Välj den **källresursgruppen** för hanteraren för virtuella datorer, eller **Molntjänsten** för klassiska virtuella datorer.
5. Spara inställningarna genom att klicka på **OK**.

    ![Konfigurera källan](./media/azure-to-azure-walkthrough-enable-replication/source.png)

## <a name="select-the-vms"></a>Välj de virtuella datorerna

Site Recovery hämtar en lista över de virtuella datorerna som är associerad med tjänsten prenumeration och resurs grupp eller ett moln.

1. I **virtuella datorer**, Välj de virtuella datorerna som du vill replikera.
2. Klicka på **OK**.

    ![Välj virtuella datorer](./media/azure-to-azure-walkthrough-enable-replication/vms.png)


## <a name="configure-settings"></a>Konfigurera inställningar

Site Recovery tillhandahåller standardinställningar för målregionen (baserat på inställningarna för datakälla region) och replikeringsprinciper:

   - **Målplatsen**: målregionen som du vill använda för katastrofåterställning. Vi rekommenderar att målplatsen matchar platsen för Site Recovery-valvet.
   - **Målresursgruppen**: resursgruppen som virtuella Azure-datorer i målet region ska tillhöra efter växling vid fel. Som standard skapar Site Recovery en ny resursgrupp i målregionen med ett ”asr” suffix. 
   - **Virtuella målnätverket**: nätverk i vilka Azure virtuella datorer i målet region kommer att finnas efter växling vid fel. Som standard skapar Site Recovery ett nytt virtuellt nätverk (och undernät) i området mål med en ”asr” suffix. Det här nätverket är mappad till nätverket källa. Observera att du kan tilldela en specifik IP-adress efter växling vid fel på en virtuell dator om du vill behålla samma IP-adress i käll- och målplatserna. 
   - **Cachelagra lagringskonton**: Site Recovery använder ett lagringskonto i regionen källa. Ändringar på virtuella källdatorer skickas till det här kontot innan replikeringen till målplatsen. 
   - **Rikta lagringskonton**: som standard skapar Site Recovery ett nytt lagringskonto i målregionen, för spegling av källan VM storage-konto.
   -  **Rikta tillgänglighetsuppsättningar**: som standard skapar en ny tillgänglighetsuppsättning i målregionen med suffixet ”asr” Site Recovery. 
   - **Namn på**: principnamn.
   - **Kvarhållningstid för återställningspunkten**: som standard sparas Site Recovery återställningspunkter under 24 timmar. Du kan konfigurera ett värde mellan 1 och 72 timmar.
   - **Frekvens av programkonsekventa ögonblicksbilder**: som standard Site Recovery tar en ögonblicksbild av programkonsekventa var fjärde timme. Du kan konfigurera ett värde mellan 1 och 12 timmar. Data replikeras kontinuerligt:
    - Kraschkonsekvent återställningspunkter upprätthålla konsekventa data write-ordning när de skapas. Den här typen av återställningspunkt räcker vanligtvis om din app är utformat för att återställa från en krasch utan inkonsekventa data
    - Kraschkonsekvent återställningspunkter skapas med några minuters mellanrum. Med dessa återställningspunkter att växla över och återställa din virtuella dator innehåller en återställning punkt mål (RPO) i minuter.
    - Programkonsekventa återställningspunkter (förutom skrivåtgärder ordning konsekvenskontroll) kontrollera att appar körs slutföra alla åtgärder och rensa buffertar till disk (programmet offline stegvis). Vi rekommenderar att du använder dessa återställningspunkter för databas-appar som SQL Server, Oracle och Exchange.
        
    ![Konfigurera inställningar](./media/azure-to-azure-walkthrough-enable-replication/settings.png)


### <a name="modify-settings"></a>Ändra inställningar

Om du vill ändra inställningar för mål- och replikering, gör du följande:

1. Om du vill visa eller ändra Målinställningar, klickar du på **inställningar**.
2. Om du vill åsidosätta standardinställningarna för mål klickar du på **anpassa**. Du kan ange en target-resursgruppen, virtuella nätverk, tillgänglighetsuppsättning och mål-lagringskontot. Du kan bara lägga till tillgänglighetsuppsättningar om virtuella datorer som är en del av en uppsättning i käll-region.

    ![Konfigurera inställningar](./media/azure-to-azure-walkthrough-enable-replication/customize-target.png)

3. Om du vill åsidosätta replikeringsinställningar för återställningspunkter och programkonsekventa ögonblicksbilder klickar du på **anpassa** bredvid **Replikeringsprincipen**.
 
    ![Konfigurera inställningar](./media/azure-to-azure-walkthrough-enable-replication/customize-policy.png)

4. Starta etablering target-resurser, klicka på **skapa målresurser**. Etablering tar en minut. Stäng inte bladet under etableringen eller måste du börja om från början.




## <a name="enable-replication"></a>Aktivera replikering

1. I **inställningar**, klickar du på **Aktivera replikering**. Detta gör det möjligt för inledande replikering av de virtuella datorerna som du har valt. Status för den inledande replikeringen kan ta lite tid att uppdatera. Klicka på **uppdatera** att hämta senaste status.

2. Du kan följa förloppet för den **Aktivera skydd** jobb **inställningar** > **jobb** > **Site Recovery-jobb**.

3. I **inställningar** > **replikerade objekt**, du kan visa status för virtuella datorer och den inledande replikeringen pågår. Klicka på den virtuella datorn och öka detaljnivån till dess inställningar.



## <a name="next-steps"></a>Nästa steg

Gå till [steg 6: köra ett redundanstest](azure-to-azure-walkthrough-test-failover.md)
