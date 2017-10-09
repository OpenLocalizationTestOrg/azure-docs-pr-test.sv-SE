---
title: "aaaHow tooReprotect från redundansväxling av virtuella Azure-datorer tillbaka tooprimary Azure-region | Microsoft Docs"
description: "Du kan använda Azure Site Recovery tooprotect hello datorerna i omvänd riktning efter växling för virtuella datorer från en Azure-region tooanother. Lär dig hello steg hur toodo skydda igen innan en växling vid fel igen."
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
ms.openlocfilehash: 991c7ee8f489e84c250230bf73f3e99015c5f051
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reprotect-from-failed-over-azure-region-back-tooprimary-region"></a>Det gick inte att skydda igen från via Azure-region tillbaka tooprimary region



>[!NOTE]
>
> Site Recovery replikering för virtuella Azure-datorer är för närvarande under förhandsgranskning.


## <a name="overview"></a>Översikt
När du [redundans](site-recovery-failover.md) hello virtuella datorer från en Azure-region tooanother hello virtuella datorer är oskyddad. Om du vill toobring dem tillbaka toohello primär region måste du toofirst skydda hello virtuella datorer och växling vid fel igen. Det finns ingen skillnad mellan hur redundans i en riktning eller andra. Bokför aktivera skyddet för hello virtuella datorer på samma sätt och det finns ingen skillnad mellan hello skydda igen efter växling vid fel eller efter återställning efter fel.
tooexplain hello arbetsflöden skyddar och tooavoid förvirring, vi använder hello primär plats hello skyddade datorer som Östasien region och hello återställningsplatsen hello datorer som Sydostasien region. Under växling vid fel, kommer du att redundans hello virtuella datorer toohello Sydostasien region. Innan du återställning efter fel måste tooreprotect hello virtuella datorer från Sydostasien tillbaka tooEast Asien. Den här artikeln beskriver hello steg om hur tooreprotect.

> [!WARNING]
> Om du har [slutfört en migrering](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration)flyttade hello virtuell tooanother datorresurs grupp- eller borttagna hello Azure-dator, det går inte att återställning efter.

När du skyddar är klar och replikerar hello skyddade virtuella datorer, kan du påbörja en växling på hello virtuella datorer toobring tillbaka dem tooEast Asien region.

Skicka kommentarer eller frågor hello slutet av den här artikeln eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Krav
1. hello virtuella datorer bör har utförts.
2. hello målplatsen – i det här fallet hello Östasien Azure-region ska vara tillgängliga och bör vara kan tooaccess/skapa nya resurser i den regionen.

## <a name="steps-tooreprotect"></a>Steg tooreprotect

Följande är en virtuell dator med hello standardvärden hello steg tooreprotect.

1. I **valvet** > **replikerade objekt**, högerklicka på hello virtuell dator som har växlas över och välj sedan **skydda igen**. Du kan också klicka på hello maskin- och välj **skydda igen** från hello kommandoknappar.

![Högerklicka på tooreprotect](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Observera att hello riktningen för skydd, i hello bladet **Sydostasien tooEast Asien**, har redan valts.

![Skapa nytt blad](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotectblade.png)

3. Granska hello **resurs grupp, nätverk, lagring och tillgänglighet anger** information och klicka på OK. Om det finns några resurser som är markerade (nya), skapas de som en del av hello skydda igen.

Detta ska utlösa ett jobb skyddar jobb som kommer först att dirigera hello målplatsen (SEA i det här fallet) med hello senaste data och när som har slutförts, replikeras tillbaka hello går innan du redundans tooSoutheast Asien.

### <a name="reprotect-customization"></a>Skapa nytt anpassning
Om du vill toochoose hello extrahera konto eller hello lagringsnätverk under skydda igen, gör du det med hjälp av hello anpassa alternativet tillhandahålls på hello skyddar bladet.

![Anpassa alternativet](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

Du kan anpassa hello följande egenskaper för han virtuella måldatorn under skydda igen.

![Anpassa blad](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Egenskap |Anteckningar  |
|---------|---------|
|Målresursgruppen     | Du kan välja toochange hello målresursgruppen th virtuella datorn kommer att skapas. Eftersom hello skyddar, hello virtuella måldatorn tas bort, därför kan du välja en ny resursgrupp som du kan skapa hello VM efter växling vid fel         |
|Mål virtuellt nätverk     | Nätverket kan inte ändras under hello skydda igen. toochange hello nätverk, gör om hello nätverksmappning.         |
|Mål-Lagringskontot     | Du kan ändra hello lagringskonto toowhich hello virtuella datorn kommer att skapas efter växling vid fel.         |
|Cachelagring     | Du kan ange ett cache-lagringskonto som ska användas vid replikering. Om du går med hello standardinställningar, skapas ett nytt lagringskonto för cache, om den inte redan finns.         |
|Tillgänglighetsuppsättning     |Du kan välja en tillgänglighetsuppsättning för hello virtuella måldatorn i Sydostasien om hello virtuell dator i Östasien ingår i en tillgänglighetsuppsättning. Standardvärden hittar hello befintlig SEA tillgänglighetsuppsättning och försök toouse den. Vid anpassning, kan du ange en helt ny uppsättning AV.         |


### <a name="what-happens-during-reprotect"></a>Vad som händer under skydda igen?

Precis som när hello först aktivera skydd, är följande hello av som skapas om du använder hello standardvärden.
1. Ett cache-lagringskonto skapas i hello Östasien region.
2. Om hello mål-lagringskontot (hello ursprungliga lagringskontot för hello Sydostasien VM) inte finns, skapas en ny. hello heter hello Östasien virtuella datorns storage-konto med ”asr” suffixet.
3. Om hello måluppsättningen AV finns inte och hello standardvärden identifiera måste toocreate nya AV har angetts och kommer att skapas som en del av hello skyddar jobb. Om du har anpassat hello skyddar sedan hello som valts AV uppsättningen kommer att användas.
4.

hello följande är hello lista över steg som kan inträffar när du utlösa ett jobb som skyddar. Detta är i hello case hello målsidan virtuella datorn finns.

1. hello krävs av skapas som en del av skydda igen. Om det redan finns, och sedan återanvänds.
2. hello sida (Sydostasien) datorn stängs av, först om det körs.
3. hello mål på serversidan virtuella datorns disk kopieras av Azure Site Recovery till en behållare som en seed-blob.
4. hello mål på serversidan virtuell dator tas sedan bort.
5. hello seed blob används av virtuella tooreplicate för hello aktuella källan sida (Östasien). Detta säkerställer att endast går replikeras.
6. hello större ändringar mellan hello källdisken och hello seed blob synkroniseras. Det kan ta viss tid toocomplete.
7. När hello skyddar jobbet är slutfört, hello deltareplikering börjar som skapar en återställningspunkt enligt hello princip.

> [!NOTE]
> Du kan inte skydda på en återställning plan nivå. Du kan bara skydda igen på en per VM-nivå.

När hello skyddar lyckas, hello virtuella datorn går in i ett skyddat läge.

## <a name="next-steps"></a>Nästa steg

Efter hello virtuell dator har angett ett skyddat läge, kan du påbörja en växling. hello redundans avslutas hello virtuell dator i Östasien Azure-region och sedan skapa och starta hello Sydostasien region virtuell dator. Det finns därför ett mindre driftstopp för hello program. Välj så hello tid för växling vid fel när ditt program kan tolerera ett driftstopp. Vi rekommenderar att du först testar redundans hello virtuella toomake att det kommer korrekt innan du påbörjar en växling vid fel.

-   [Steg tooinitiate testa redundans för hello virtuell dator](site-recovery-test-failover-to-azure.md)

-   [Steg tooinitiate redundans för hello virtuell dator](site-recovery-failover.md)
