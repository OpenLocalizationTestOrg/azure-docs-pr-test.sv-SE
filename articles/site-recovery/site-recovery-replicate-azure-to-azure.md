---
title: aaaReplicate program (Azure tooAzure) | Microsoft Docs
description: "Den här artikeln beskriver hur tooset duplicering av virtuella datorer som körs i en Azure-region för en annan region i Azure."
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 5/22/2017
ms.author: asgang
ms.openlocfilehash: fb190dac14419f892a1c6b45a3d991d8005e4bd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-virtual-machines-tooanother-azure-region"></a>Replikera virtuella datorer i Azure tooanother Azure-region



>[!NOTE]
>
> Site Recovery replikering för virtuella Azure-datorer är för närvarande under förhandsgranskning.

Den här artikeln beskriver hur tooset duplicering av virtuella datorer som körs i en Azure-region tooanother Azure-region.

## <a name="prerequisites"></a>Krav

* hello förutsätter att du redan vet om Site Recovery och Recovery Services-valvet. Du måste toohave en Recovery services-ventilen pre skapas.

    >[!NOTE]
    >
    > Det rekommenderas att du skapar hello Recovery services-ventilen i hello plats där du vill att din tooreplicate för virtuella datorer. Till exempel om din målplats 'centrala USA, skapa valv i 'Centrala USA'.

* Se till att du godkända hello begärda URL: er eller IP-adresser om du använder regler för Nätverkssäkerhetsgrupp grupper (NSG) eller brandväggen proxy toocontrol åtkomst toooutbound internet-anslutning på hello Azure virtuella datorer. Se för[nätverk riktlinjerna](./site-recovery-azure-to-azure-networking-guidance.md) för mer information.

* Om du har en ExpressRoute eller en VPN-anslutning mellan lokala och hello datakällplats i Azure, Följ [platsöverväganden för Azure tooon lokala ExpressRoute / VPN-konfiguration](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) dokumentet.

* Ditt Azure-konto måste toohave vissa [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikering av en virtuell Azure-dator.

* Din Azure-prenumeration ska vara aktiverade toocreate virtuella datorer i hello målplats du toouse som DR region. Du kan kontakta support tooenable hello krävs kvoten.

## <a name="enable-replication-from-azure-site-recovery-vault"></a>Aktivera replikering från Azure Site Recovery-valvet
Vi kommer replikera virtuella datorer som körs i Azure-plats toohello för hello östra Asien, Sydostasien, plats för den här bilden. hello stegen är följande:

 Klicka på **+ replikera** i hello valvet tooenable replikering för hello virtuella datorer.

1. **Källa:** refererar den ursprungliga hello datorer i det här fallet toohello punkt **Azure**.

2. **Datakällplats:** är det hello Azure-region där du vill att tooprotect dina virtuella datorer. Den här bilden ska hello källplats östra Asien

3. **Distributionsmodell:** refererar toohello Azure distributionsmodell för hello källdatorer. Du kan välja antingen klassiska eller resource manager och datorer som tillhör toohello viss modell visas för skydd i hello nästa steg.

      >[!NOTE]
      >
      > Du kan endast replikera en klassisk virtuell dator och återställa den som en klassisk virtuell dator. Du kan inte återställa den som en virtuell dator i Resource Manager.

4. **Resursgrupp:** det är hello resurs grupp toowhich dina virtuella källdatorer tillhör. Alla hello virtuella datorer under hello valda resursgruppen visas för skydd i hello nästa steg.

    ![Aktivera replikering](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

I **virtuella datorer > Välj virtuella datorer**och på varje dator som du vill tooreplicate. Du kan bara välja datorer som stöder replikering. Klicka sedan på OK.
    ![Aktivera replikering](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)


Under inställningar kan du konfigurera egenskaper för mål

1. **Målplatsen:** det här är hello plats där källdata virtuella datorn kommer att replikeras. Beroende på din plats för valda datorer, tillhandahåller Site Recovery du hello lista över lämpliga målregioner.

    > [!TIP]
    > Det rekommenderas tookeep målplats samma från och med din recovery services-valvet.

2. **Målresursgruppen:** är det hello resurs grupp toowhich alla replikerade virtuella datorer ska tillhöra. Som standard skapar ASR en ny resursgrupp i hello målregionen med namn med suffixet ”asr”. Om resursgruppen som skapats av ASR redan finns, den av återanvändas. Du kan också välja toocustomize den enligt hello nedan.    
3. **Mål för virtuella nätverk:** som standard ASR skapas ett nytt virtuellt nätverk i hello målregionen med namn med suffixet ”asr”. Detta kommer att vara mappade tooyour källnätverket och kommer att användas för alla framtida skydd.

    > [!NOTE]
    > [Nätverk klientkontrollen](site-recovery-network-mapping-azure-to-azure.md) tooknow mer om nätverksmappning.

4. **Rikta Storage-konton:** skapar som standard ASR hello nya mål-lagringskontot frihandsbilden lagringskonfigurationen källa VM. Om lagringskonto som skapats av ASR redan finns, den av återanvändas.

5. **Cachelagra Storage-konton:** ASR måste extra lagring som kallas konto för cachelagring i hello källa region. Alla hello ändringar som sker på hello virtuella källdatorer spåras och skickas toocache storage-konto innan du replikerar de toohello målplatsen.

6. **Tillgänglighetsuppsättningen:** som standard ASR skapar en ny tillgänglighetsuppsättning i hello målregionen med namn med suffixet ”asr”. Om tillgänglighet som skapats av ASR redan finns kommer den av återanvändas.

7.  **Replikeringsprincip:** definierar hello inställningar för återställning punkt kvarhållning historik och app programkonsekvent ögonblicksbild frekvens. Som standard skapas ASR en ny replikeringsprincip med standardinställningarna för ”24 timmars för kvarhållningstid för återställningspunkten och” 60 minuters för app programkonsekvent ögonblicksbild frekvens.

    ![Aktivera replikering](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a>Anpassa target-resurser

Om du vill toochange hello standardvärden som används av ASR kan du ändra hello inställningar baserat på dina behov.

1. **Anpassa:** klickar du på den toochange hello standardvärden som används av ASR.

2. **Målresursgruppen:** du kan välja hello resursgrupp hello listan över alla resursgrupper för hello befintliga hello målplatsen inom hello prenumeration.

3. **Mål för virtuella nätverk:** du hittar hello lista över alla hello virtuellt nätverk i hello målplats.

4. **Tillgänglighetsuppsättningen:** du kan bara lägga till tillgänglighet anger inställningar toohello virtuella datorer som ingår i tillgänglighet i källan region.

5. **Mål Storage-konton:**

![Aktivera replikering](./media/site-recovery-replicate-azure-to-azure/customize.PNG) klickar du på **skapa målresurs** och aktivera replikering


När virtuella datorer är skyddade kan du hello status för virtuella datorer hälsa under **replikerade objekt**

>[!NOTE]
>Under hello kunde tid för inledande replikering det en risk för att status tar tid toorefresh och du inte kan se förloppet under en viss tid. Du kan klicka hello uppdatera ovanpå hello hello bladet tooget hello senaste status.
>

![Aktivera replikering](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)


## <a name="next-steps"></a>Nästa steg
- [Lär dig mer](site-recovery-test-failover-to-azure.md) om att köra ett redundanstest.
- [Lär dig mer](site-recovery-failover.md) om olika typer av växling vid fel, och hur toorun dem.
- Lär dig mer om [med återställningsplaner](site-recovery-create-recovery-plans.md) tooreduce Återställningstidsmål.
- Lär dig mer om [skydda virtuella datorer i Azure](site-recovery-how-to-reprotect.md) efter växling vid fel.
