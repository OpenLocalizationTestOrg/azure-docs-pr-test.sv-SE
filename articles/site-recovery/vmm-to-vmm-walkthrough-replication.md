---
title: "aaaSet upp en replikeringsprincip för Hyper-V-replikering tooa sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur tooset in en princip för Hyper-V VM replikering tooa sekundära VMM webbplats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5d9b79cf-89f2-4af9-ac8e-3a32ad8c6c4d
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 6d008e3bb3fa0b666e91678cf6de3693dd712ae3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-a-replication-policy"></a>Steg 8: Skapa en replikeringsprincip

När du har konfigurerat [nätverksmappning](vmm-to-vmm-walkthrough-network-mapping.md), använder den här artikeln tooset upp en replikeringsprincip för Hyper-V virtuell dator (VM) replikering tooa sekundär plats med hjälp av [Azure Site Recovery](site-recovery-overview.md).

När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Innan du börjar

- När du skapar en replikeringsprincip måste alla värdar med hjälp av hello princip ha hello samma operativsystem. hello VMM-moln kan innehålla Hyper-V-värdar som kör olika versioner av Windows Server, men i detta fall måste flera replikeringsprinciper.
- Du kan utföra hello första replikeringen offline.

## <a name="configure-replication-settings"></a>Konfigurera replikeringsinställningar

1. Klicka på toocreate en ny replikeringsprincip **Förbered infrastruktur** > **replikeringsinställningarna** > **+ skapa och koppla**.

    ![Nätverk](./media/vmm-to-vmm-walkthrough-replication/gs-replication.png)
2. I **Princip för att skapa och koppla** anger du ett principnamn. hello källa och mål-typen ska vara **Hyper-V**.
3. I **Hyper-V-värdversion**, Välj vilket operativsystem som körs på hello värden.
4. I **autentiseringstyp** och **autentiseringsport**, ange hur trafik autentiseras mellan hello primära och återställning Hyper-V-värdservrar. Välj **certifikat** om du inte har en fungerande Kerberos-miljö. Azure Site Recovery kommer automatiskt att konfigurera certifikat för HTTPS-autentisering. Du behöver inte toodo något manuellt. Som standard öppnas port 8083 och 8084 (för certifikat) i hello Windows-brandväggen på hello Hyper-V-värdservrar. Om du väljer **Kerberos**, en Kerberos-biljett används för ömsesidig autentisering av hello värdservrar. Observera att den här inställningen gäller endast för Hyper-V-värdservrar som körs på Windows Server 2012 R2.
5. I **Kopieringsfrekvens**, ange hur ofta du vill tooreplicate deltadata efter hello första replikeringen (med 30 sekunders mellanrum, 5 eller 15 minuter).
6. I **kvarhållningstid för återställningspunkten**, ange i timmar hur länge hello kvarhållningsperiod ska vara för varje återställningspunkt. Skyddade datorer kan återställas tooany punkt inom en period.
7. I **frekvens av programkonsekventa ögonblicksbilder**, ange hur ofta (1 – 12 timmar) återställningspunkter som innehåller programkonsekventa ögonblicksbilder skapas. Hyper-V använder två typer av ögonblicksbilder, en standardögonblicksbild som tillhandahåller en inkrementell ögonblicksbild av hello hela den virtuella datorn och en programkonsekvent ögonblicksbild som tar en tidpunkt i ögonblicksbild av hello programdata i hello virtuella datorn. Programkonsekventa ögonblicksbilder använda Volume Shadow Copy Service (VSS) tooensure som programmen är i ett konsekvent tillstånd när hello ögonblicksbilden tas. Om du aktiverar programkonsekventa ögonblicksbilder så påverkar hello prestanda för program som körs på virtuella källdatorer. Kontrollera att hello-värdet som du angett är mindre än hello antalet ytterligare återställningspunkter som du konfigurerar.
8. I **dataöverföring komprimering**, ange om replikerade data som överförs ska komprimeras.
9. Välj **ta bort replikering VM**, toospecify som hello replikerade virtuella datorn ska tas bort om du inaktiverar skydd för Virtuella hello källdatorn. Om du aktiverar den här inställningen när du inaktiverar skyddet för källan hello VM det tas bort från hello Site Recovery-konsolen, Site Recovery-inställningarna för hello VMM tas bort från hello VMM-konsolen och hello repliken tas bort.
10. I **inledande Replikeringsmetod**, om du replikerar hello nätverket, ange om toostart hello replikeringen eller schemalägga den. toosave nätverksbandbredd, kanske du vill tooschedule den utanför kontorstid. Klicka sedan på **OK**.

     ![Replikeringsprincip](./media/vmm-to-vmm-walkthrough-replication/gs-replication2.png)
11. När du skapar en ny princip associeras den automatiskt med hello VMM-moln. I **replikeringsprincipen**, klickar du på **OK**. Du kan associera ytterligare VMM-moln (och hello virtuella datorer i dem) med den här replikeringsprincipen i **replikering** > principnamn > **associera VMM-moln**.

     ![Replikeringsprincip](./media/vmm-to-vmm-walkthrough-replication/policy-associate.png)



## <a name="prepare-for-offline-initial-replication"></a>Förbereda för inledande offlinereplikering

Du kan göra offlinereplikering för hello första Datakopieringen. Du kan förbereda på följande sätt:

* På hello källservern, kan du ange en sökväg från vilka hello export av data sker. Tilldela fullständig behörighet för NTFS och dela behörigheter toohello VMM-tjänsten på hello exportsökvägen. På målservern för hello anger du en sökväg där hello dataimport sker. Tilldela hello samma behörigheter på den här importsökvägen.
* Om hello importera eller exportera sökvägen är delad, tilldela gruppmedlemskap för administratörer, Privilegierade användare, Skrivaransvariga eller serveransvarig för hello VMM-tjänstkontot på hello fjärrdator på vilken hello delade finns.
* Om du använder Kör som-konton tooadd värdar på hello importera och exportera sökvägar, tilldela Läs och skriva toohello kör som-konton i VMM.
* Hej importera och exportera resurser bör inte finnas på en dator som används som en Hyper-V-värdserver eftersom loopback konfigurationen inte stöds av Hyper-V.
* I Active Directory på varje Hyper-V-värdserver som innehåller virtuella datorer du vill tooprotect, aktivera och konfigurera begränsad delegering tootrust hello fjärranslutna datorer på vilka hello import och export sökvägar finns på följande sätt:
  1. Öppna på hello domänkontrollant **Active Directory-användare och datorer**.
  2. I konsolträdet hello klickar du på **DomainName** > **datorer**.
  3. Högerklicka på hello Hyper-V server värdnamn > **egenskaper**.
  4. På hello **delegering** klickar du på **förtroende för den här datorn för delegering toospecified services**.
  5. Klicka på **Använd valfritt autentiseringsprotokoll**.
  6. Klicka på **lägga till** > **användare och datorer**.
  7. Hello-typnamn för hello-dator som är värd för hello exportsökvägen > **OK**. Håll ned CTRL-tangenten hello hello listan över tillgängliga tjänster, och klicka på **cifs** > **OK**. Upprepa för hello namnet på datorn som hello värdar hello importera sökvägen. Upprepa efter behov för ytterligare Hyper-V-värdservrar.



## <a name="next-steps"></a>Nästa steg

Gå för[steg 9: Aktivera replikering](vmm-to-vmm-walkthrough-enable-replication.md).
