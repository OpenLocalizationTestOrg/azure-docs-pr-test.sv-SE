---
title: "aaaSet upp en replikeringsprincip för Hyper-V VM replikering (utan VMM för System Center) tooAzure med Azure Site Recovery | Microsoft Docs"
description: Sammanfattar hello stegen tooset upp en replikeringsprincip vid replikering av Hyper-V VMs tooAzure lagring
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 1777e0eb-accb-42b5-a747-11272e131a52
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 4bd7161f4a0f015da0ecf595fbc6861ede5d68b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-set-up-a-replication-policy-for-hyper-v-vm-replication-tooazure"></a>Steg 9: Konfigurera en replikeringsprincip för Hyper-V VM replikering tooAzure

Den här artikeln beskriver hur tooset upp en replikeringsprincip när du replikerar virtuella Hyper-V-datorer tooAzure (utan System Center VMM) med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.


Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="about-snapshots"></a>Om ögonblicksbilder

Hyper-V använder två typer av ögonblicksbilder, en standardögonblicksbild som tillhandahåller en inkrementell ögonblicksbild av hello hela den virtuella datorn och en programkonsekvent ögonblicksbild som tar en tidpunkt i ögonblicksbild av hello programdata i hello virtuella datorn.
    - Programkonsekventa ögonblicksbilder använda Volume Shadow Copy Service (VSS) tooensure som programmen är i ett konsekvent tillstånd när hello ögonblicksbilden tas.
    - Om du aktiverar programkonsekventa ögonblicksbilder så påverkar hello prestanda för program som körs på virtuella källdatorer. Kontrollera att hello-värdet som du angett är mindre än hello antalet ytterligare återställningspunkter som du konfigurerar.

## <a name="set-up-a-replication-policy"></a>Konfigurera en princip för lösenordsreplikering

1. Klicka på toocreate en ny replikeringsprincip **Förbered infrastruktur** > **replikeringsinställningarna** > **+ skapa och koppla**.

    ![Nätverk](./media/hyper-v-site-walkthrough-replication/gs-replication.png)
2. I **Princip för att skapa och koppla** anger du ett principnamn.
3. I **Kopieringsfrekvens**, ange hur ofta du vill tooreplicate deltadata efter hello första replikeringen (med 30 sekunders mellanrum, 5 eller 15 minuter).

    > [!NOTE]
    > En 30 andra frekvens stöds inte vid replikering av toopremium lagring. hello begränsning bestäms av hello antalet ögonblicksbilder per blob (100) stöds av premium-lagring. [Läs mer](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).

4. I **kvarhållningstid för återställningspunkten**, ange i hur länge timmar hello kvarhållningsperiod är för varje återställningspunkt. Virtuella datorer kan återställas tooany punkt inom en period.
5. I **frekvens av programkonsekventa ögonblicksbilder**, ange hur ofta (1 – 12 timmar) återställningspunkter som innehåller programkonsekventa ögonblicksbilder skapas.
6. I **starttid för inledande replikering**, ange när toostart hello inledande replikering. hello replikeringen sker via internetbandbredden så du kanske vill tooschedule den utanför kontorstid. Klicka sedan på **OK**.

    ![Replikeringsprincip](./media/hyper-v-site-walkthrough-replication/gs-replication2.png)

När du skapar en ny princip associeras den automatiskt med hello Hyper-V-platsen. Du kan associera en Hyper-V-platsen (och hello virtuella datorer i den) med flera replikeringsprinciper i **replikering** > principnamn > **associera Hyper-V-platsen**.



## <a name="next-steps"></a>Nästa steg

Gå för[steg 10: Aktivera replikering](hyper-v-site-walkthrough-enable-replication.md)
