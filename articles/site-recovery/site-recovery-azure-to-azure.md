---
title: "Replikera virtuella Azure-datorer mellan Azure-regioner för katastrofåterställningskrav: Azure tooAzure | Microsoft Docs"
description: "Sammanfattar hello stegen tooreplicate Azure virtuella datorer mellan Azure-regioner (Azure till Azure) med hello Azure Site Recovery-tjänsten för disaster recovery behov."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: raynew
ms.openlocfilehash: c4c425af260a9bcc3dd4dcc13da26109e20f03bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a>Replikera virtuella Azure-datorer mellan regioner med Azure Site Recovery

>[!NOTE]
>
> Azure Site Recovery replikering för virtuella Azure-datorer (VM) är för närvarande under förhandsgranskning.

Den här artikeln beskriver hur tooreplicate Azure virtuella datorer mellan Azure-regioner med hjälp av hello [Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

Publicera kommentarer och frågor längst ned hello av den här artikeln eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="disaster-recovery-in-azure"></a>Katastrofåterställning i Azure

Inbyggda Azure-infrastrukturen-funktioner bidrar tooa robust och flexibel tillgängligheten för arbetsbelastningar som körs på virtuella Azure-datorer. Det finns många orsaker till varför du behöver tooplan för katastrofåterställning mellan Azure-regioner själv:

- Du behöver toomeet efterlevnadsriktlinjer för specifika appar och arbetsbelastningar som kräver en affärskontinuitet och haveriberedskap (BCDR).
- Du vill hello möjlighet tooprotect och återställa virtuella Azure-datorer baserat på din affärsbeslut och inte bara baserat på inbyggda funktioner för Azure.
- Du behöver tootest redundans och återställning enligt dina behov som företag och efterlevnad, utan inverkan på produktion.
- Du behöver toofail över toohello recovery region i en katastrof hello-händelse och växla tillbaka toohello ursprungliga källan region sömlöst.

Använda Site Recovery för Virtuella Azure-Azure replikering toohelp du gör dessa uppgifter.


## <a name="why-use-site-recovery"></a>Varför ska jag använda Site Recovery?      

Site Recovery tillhandahåller ett enkelt sätt tooreplicate Azure virtuella datorer mellan regioner:

- **Automatisk distribution**. Till skillnad från en aktiv-aktiv replikeringsmodell behövs det ingen för ett dyrt och komplexa infrastrukturen i hello sekundär region. När du aktiverar replikering skapar Site Recovery automatiskt hello krävs resurser i hello målregionen baserat på inställningarna för datakälla region.
- **Kontrollera regioner**. Du kan replikera från en region tooany region inom en kontinent med Site Recovery. Jämför detta med läsbehörighet geo-redundant lagring, som replikerar asynkront mellan standard [länkas regioner](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) endast. Geo-redundant lagring med läsbehörighet tillhandahåller skrivskyddad åtkomst toohello data i hello målregionen.
- **Automatisk replikering**. Site Recovery tillhandahåller automatiserad kontinuerlig replikering. Redundans och återställning kan aktiveras med en enda klickning.
- **Återställningstidsmålet och Återställningspunktmål**. Site Recovery drar nytta av hello Azure nätverksinfrastrukturen som kopplar samman regioner tookeep RTO och Återställningspunktmål mycket låg.
- **Testa**. Du kan köra katastrofåterställning flyttar med på begäran redundanstestningar, vid behov utan att påverka dina produktionsarbetsbelastningar eller pågående replikering.
- **Återställningsplaner**. Du kan använda återställning planer tooorchestrate redundans och återställning efter fel för hello hela program som körs på flera virtuella datorer. hello återställning plan för har omfattande förstklassigt integrering med Azure automation-runbooks.


## <a name="deployment-summary"></a>Översikt över distribution

Här följer en sammanfattning av vad du behöver toodo tooset duplicering av virtuella datorer mellan Azure-regioner:

1. Skapa ett Recovery Services-valv. hello valvet innehåller konfigurationsinställningar och styr replikeringen.
2. Aktivera replikering för hello Azure virtuella datorer.
3. Kör ett test redundans toomake till att allt fungerar som förväntat.

>[!IMPORTANT]
>
> Du kan kontrollera hello [stöd matrix för replikering av Azure Virtuella](./site-recovery-support-matrix-azure-to-azure.md).

>[!IMPORTANT]
>
> Information om hur tooconfigure hello krävs utgående nätverksanslutningen för virtuella datorer i Azure för Site Recovery replikering finns hello [nätverk riktlinjerna](./site-recovery-azure-to-azure-networking-guidance.md).

### <a name="before-you-start"></a>Innan du börjar

* Ditt Azure-konto måste toohave vissa [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikering av en virtuell Azure-dator.
* Din Azure-prenumeration ska vara aktiverade toocreate virtuella datorer i hello målplats som du vill toouse som hello disaster recovery region. Kontakta supporten tooenable hello krävs kvoten.

## <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> Vi rekommenderar att du skapar hello Recovery Services-valvet i hello plats där du vill att din tooreplicate för virtuella datorer. Till exempel om din målplatsen är hello central oss, skapa hello valv i **centrala USA**.

## <a name="enable-replication"></a>Aktivera replikering

I **Recovery Services-valv**, klicka på valvnamnet för hello. Klicka på hello i hello valvet, **+ replikera** knappen.

### <a name="step-1-configure-hello-source"></a>Steg 1. Konfigurera hello källa
1. I **källa**väljer **Azure - PREVIEW**.
2. I **datakällplats**väljer hello källa Azure-region där din virtuella dator körs för närvarande.
3. Välj hello distributionsmodell för din virtuella datorer: **Resource Manager** eller **klassiska**.
4. Välj hello **källresursgruppen** för hanteraren för virtuella datorer eller **Molntjänsten** för klassiska virtuella datorer.

    ![Konfigurera hello källa](./media/site-recovery-azure-to-azure/source.png)

### <a name="step-2-select-virtual-machines"></a>Steg 2. Välj virtuella datorer

* Välj hello virtuella datorer du vill tooreplicate och klicka sedan på **OK**.

    ![Välj virtuella datorer](./media/site-recovery-azure-to-azure/vms.png)

### <a name="step-3-configure-settings"></a>Steg 3. Konfigurera inställningar

1. toooverride hello standard målet inställningar och ange hello inställningar du väljer, klickar du på **anpassa**. Mer information finns i [anpassa målresurser](site-recovery-replicate-azure-to-azure.md##customize-target-resources).

    ![Konfigurera inställningar](./media/site-recovery-azure-to-azure/settings.png)


2. Som standard skapar Site Recovery en replikeringsprincip som tar programkonsekventa ögonblicksbilder var fjärde timme och behåller återställningspunkter under 24 timmar. toocreate en princip med olika inställningar, klicka på **anpassa** nästa för**Replikeringsprincipen**.

    ![Anpassa principen](./media/site-recovery-azure-to-azure/customize-policy.png)

3. toostart etablering hello target-resurser, klickar du på **skapa målresurser**. Etablering tar en minut. Stäng inte hello bladet under etableringen, eller så behöver toostart över.

4. tootrigger replikering av hello valt VM, klicka på **Aktivera replikering**.

5. Du kan följa förloppet för hello **Skyddsaktivering** jobb **inställningar** > **jobb** > **Site Recovery-jobb**.

6. I **inställningar** > **replikerade objekt**, kan du visa hello status för virtuella datorer och hello inledande replikering pågår. Klicka på hello VM toodrill ned till dess inställningar.

## <a name="run-a-test-failover"></a>Köra ett redundanstest

När du har skapat allt kör ett test redundans toomake att allt fungerar som förväntat:

1. toofail över en enskild dator i **inställningar** > **replikerade objekt**, klicka på hello VM **+ testa redundans** ikon.

2. toofail via en återställning planera i **inställningar** > **Återställningsplaner**, högerklicka på hello plan **Redundanstestningen**. toocreate en återställningsplan [följer du dessa instruktioner](site-recovery-create-recovery-plans.md). 

3. I **Redundanstest**väljer hello mål virtuella Azure-nätverket toowhich virtuella Azure-datorer är anslutna när hello växling vid fel.

4. toostart Hej växling vid fel, klicka på **OK**. tootrack utvecklas, klicka på hello VM tooopen dess egenskaper. Du kan också klicka på hello **Redundanstest** jobb i hello valvnamnet > **inställningar** > **jobb** > **Site Recovery-jobb**.

5. Efter hello växling vid fel är klar hello replik Azure datorn visas i hello Azure-portalen > **virtuella datorer**. Kontrollera att hello VM hello rätt storlek, att den är ansluten toohello lämpligt nätverk och att den körs.

6. toodelete hello virtuella datorer som har skapats under hello redundanstestningen klickar du på **Rensa redundanstestet** på hello replikerade objekt eller hello återställningsplan. I **anteckningar**, registrera och spara observationer från redundanstestningen hello. 

[Lär dig mer](site-recovery-test-failover-to-azure.md) om redundanstestning.


## <a name="next-steps"></a>Nästa steg

När du testa hello distribution:

- [Lär dig mer](site-recovery-failover.md) om olika typer av redundansväxlingar och hur toorun dem.
- Lär dig mer om [med återställningsplaner](site-recovery-create-recovery-plans.md) tooreduce Återställningstidsmål.
