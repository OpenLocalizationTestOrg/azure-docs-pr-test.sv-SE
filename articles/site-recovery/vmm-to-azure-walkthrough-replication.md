---
title: "aaaSet upp en replikeringsprincip för Hyper-V-dator (med VMM) replikering tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur tooset upp en replikeringsprincip för Hyper-V-dator (med VMM) replikering tooAzure med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ee808b20-324b-4198-b831-edb65b95e8b7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: e1579fde559ca34eca19a01e740fec28a0df2f9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-set-up-a-replication-policy-for-hyper-v-vm-replication-with-vmm-tooazure"></a>Steg 10: Ställ in en replikeringsprincip för Hyper-V VM replikering (med VMM) tooAzure


När du har installerat [nätverksmappning](vmm-to-azure-walkthrough-network-mapping.md), Använd den här artikeln tooconfigure en replikeringsprincip T\tooreplicate lokala Hyper-V virtuella datorer som hanteras i System Center Virtual Machine Manager (VMM) moln tooAzure, med hjälp av hello [ Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="create-a-policy"></a>Skapa en princip

1. Klicka på toocreate en ny replikeringsprincip **Förbered infrastruktur** > **replikeringsinställningarna** > **+ skapa och koppla**.

    ![Nätverk](./media/vmm-to-azure-walkthrough-replication/gs-replication.png)
2. I **Princip för att skapa och koppla** anger du ett principnamn.
3. I **Kopieringsfrekvens**, ange hur ofta du vill tooreplicate deltadata efter hello första replikeringen (med 30 sekunders mellanrum, 5 eller 15 minuter).

    > [!NOTE]
    >  En 30 andra frekvens stöds inte vid replikering av toopremium lagring. hello begränsning bestäms av hello antalet ögonblicksbilder per blob (100) stöds av premium-lagring. [Läs mer](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. I **kvarhållningstid för återställningspunkten**, ange i timmar hur länge hello kvarhållningsperiod ska vara för varje återställningspunkt. Skyddade datorer kan återställas tooany punkt inom en period.
5. I **Appkompatibel ögonblicksbildsfrekvens** anger du hur ofta (1–12 timmar) återställningspunkter som innehåller programkonsekventa ögonblicksbilder ska skapas. Hyper-V använder två typer av ögonblicksbilder, en standardögonblicksbild som tillhandahåller en inkrementell ögonblicksbild av hello hela den virtuella datorn och en programkonsekvent ögonblicksbild som tar en tidpunkt i ögonblicksbild av hello programdata i hello virtuella datorn. Programkonsekventa ögonblicksbilder använda Volume Shadow Copy Service (VSS) tooensure som programmen är i ett konsekvent tillstånd när hello ögonblicksbilden tas. Observera att om du aktiverar programkonsekventa ögonblicksbilder påverkas hello prestanda för program som körs på virtuella källdatorer. Kontrollera att hello-värdet som du angett är mindre än hello antalet ytterligare återställningspunkter som du konfigurerar.
6. I **starttid för inledande replikering**, ange när toostart hello inledande replikering. hello replikeringen sker via internetbandbredden så du kanske vill tooschedule den utanför kontorstid.
7. I **kryptera data lagrade på Azure**, ange om tooencrypt vilande data i Azure-lagring. Klicka sedan på **OK**.

    ![Replikeringsprincip](./media/vmm-to-azure-walkthrough-replication/gs-replication2.png)
8. När du skapar en ny princip associeras den automatiskt med hello VMM-moln. Klicka på **OK**. Du kan associera ytterligare VMM-moln (och hello virtuella datorer i dem) med den här replikeringsprincipen i **replikering** > principnamn > **associera VMM-moln**.

    ![Replikeringsprincip](./media/vmm-to-azure-walkthrough-replication/policy-associate.png)



## <a name="next-steps"></a>Nästa steg

Gå för[steg 11: Aktivera replikering](vmm-to-azure-walkthrough-enable-replication.md)
