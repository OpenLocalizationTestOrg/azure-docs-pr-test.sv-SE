---
title: "aaaRun ett redundanstest för replikering av virtuella Azure-datorn med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello steg som du behöver för att köra ett redundanstest för virtuella datorer i Azure replikering tooanother Azure-region med hello Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: e15c1b0c-5d75-4fdf-acb0-e61def9e9339
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: c1f765aa94c59dd70b33317ebbcd04beb7977969
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-run-a-test-failover-for-azure-vm-replication"></a>Steg 6: Köra ett redundanstest för Azure VM-replikering

När du har aktiverat replikering för virtuell Azure-dator (VM), så hello i den här artikeln toorun testa redundans från en Azure-region tooanother, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

- När du är klar hello artikel bör du har verifierat med ett redundanstest att minst en virtuell Azure-dator kan växla över tooyour sekundära Azure-region. 
- Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

>[!NOTE]
>
> Azure VM-replikering är för närvarande under förhandsgranskning.


## <a name="before-you-start"></a>Innan du börjar

- Innan du kör ett redundanstest rekommenderar vi att du kontrollera hello VM egenskaper och gör eventuella ändringar som du behöver. Du kan komma åt hello VM egenskaper i **replikerade objekt**. Hej **Essentials** bladet visar information om inställningar för datorer och status.
- Vi rekommenderar att du använder ett separat nätverk för virtuell dator i Azure för hello testa redundans och inte hello nätverk (standard eller anpassade) som har ställts in för redundans för produktion.
- hello testa redundans växlar över virtuella Azure-datorer (och deras lagringsutrymmen) toohello sekundära Azure-region. Den replikeras inte beroende appar eller resurser. Om appar som körs på misslyckade över virtuella datorer är beroende av andra resurser, till exempel Active Directory eller DNS, måste tooreplicate dessa också, om de inte redan finns i din sekundär region. [Läs mer](site-recovery-test-failover-to-azure.md#prepare-active-directory-and-dns)
- Om du vill tooaccess replikerade virtuella datorer efter redundans från en lokal plats, du behöver för[förbereda tooconnect](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) toothese virtuella datorer.

## <a name="run-a-test-failover"></a>Köra ett redundanstest

1. I **inställningar** > **replikerade objekt**, klicka på hello VM **+ testa redundans** ikon. 

2. I **Redundanstest**, Välj en återställningspunkt punkt toouse hello växling vid fel:

    - **Senaste bearbetas**: misslyckas hello VM över toohello senaste återställningspunkten som bearbetades av hello Site Recovery-tjänsten. Hej, tidsstämpel visas. Med det här alternativet läggs ingen tid bearbetning av data, så att den ger en låga RTO (mål).
    - **Senaste programkonsekventa**: det här alternativet flyttas över alla virtuella datorer toohello senaste programkonsekventa återställningspunkten. Hej, tidsstämpel visas. 
    - **Anpassad**: Välj någon annan återställningspunkt.
 
3. Välj hello mål virtuella Azure-nätverket toowhich Azure virtuella datorer i hello sekundär region ansluts när hello växling vid fel.
4. toostart Hej växling vid fel, klicka på **OK**. tootrack utvecklas, klicka på hello VM tooopen dess egenskaper. Du kan också klicka på hello **Redundanstest** jobb i hello valvnamnet > **inställningar** > **jobb** > **Site Recovery-jobb**.
5. Efter hello växling vid fel är klar hello replik virtuella Azure-datorn visas i hello Azure-portalen > **virtuella datorer**. Kontrollera att hello VM hello rätt storlek, att den är ansluten toohello lämpligt nätverk och att den körs.
6. toodelete hello virtuella datorer som har skapats under hello redundanstestningen klickar du på **Rensa redundanstestet** på hello replikerade objekt eller hello återställningsplan. I **anteckningar**, registrera och spara observationer från redundanstestningen hello. 

[Lär dig mer](site-recovery-test-failover-to-azure.md) om redundanstestning.

## <a name="next-steps"></a>Nästa steg

Den här genomgången är klar när du har testat växling vid fel. Nu, lär du dig om att köra redundansväxlingar i produktion:

- [Lär dig mer](site-recovery-failover.md) om olika typer av växling vid fel, och hur toorun dem.
- Mer information om misslyckas över flera virtuella datorer [med hjälp av en återställningsplan](site-recovery-create-recovery-plans.md).
- Lär dig mer om [med återställningsplaner](site-recovery-create-recovery-plans.md).
- Lär dig mer om [skydda virtuella datorer i Azure](site-recovery-how-to-reprotect.md) efter växling vid fel.

