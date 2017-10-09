---
title: "aaaEnable replikering för virtuella VMware-datorer replikeras tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooenable replikering tooAzure för VMwares virtuella datorer med hjälp av hello Azure Site Recovery-tjänsten"
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 519c5559-7032-4954-b8b5-f24f5242a954
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 490782bbbfa3dd92c626d3985c75d771df53d566
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-11-enable-replication-for-vmware-virtual-machines-tooazure"></a>Steg 11: Aktivera replikering för VMware tooAzure för virtuella datorer


Den här artikeln beskriver hur tooenable replikering för lokal VMware virtuella datorer tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Innan du börjar

- Virtuella VMware-datorer måste ha hello [mobilitetstjänsten installeras](vmware-walkthrough-install-mobility.md). – Om en virtuell dator är förberedd för push-installation installerar hello mobilitetstjänsten automatiskt hello processervern när du aktiverar replikering.
- Ditt Azure-konto måste specifika [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikering av en VM-tooAzure
- När du lägger till eller ändra virtuella datorer, kan det ta upp too15 minuter eller längre för ändringar tootake effekt och för dem tooappear hello-portalen.
- Du kan kontrollera hello senast identifierade tid för virtuella datorer i **Konfigurationsservrar** > **senaste kontakt på**.
- tooadd virtuella datorer utan att vänta på hello schemalagda identifiering, markera hello konfigurationsservern (inte på den), och klicka på **uppdatera**.



## <a name="exclude-disks-from-replication"></a>Undanta diskar från replikering

Alla diskar på en dator replikeras som standard. Du kan exkludera diskar från replikeringen. Till exempel kanske du inte vill tooreplicate diskar med tillfälliga data eller data som har uppdateras varje gång en dator eller programmet startas om (till exempel pagefile.sys eller SQL Server tempdb). [Läs mer](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a>Replikera virtuella datorer

Innan du börjar titta på en video Snabböversikt

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. Klicka på **Steg 2: Replikera program** > **Källa**.
2. I **källa**väljer hello konfigurationsservern.
3. I **datorn typen**väljer **virtuella datorer**.
4. I **vCenter/vSphere-Hypervisor**väljer hello vCenter-servern som hanterar hello vSphere-värd, eller hello värden.
5. Välj hello processervern. Om du inte skapat några ytterligare servrar blir hello konfigurationsservern. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/vmware-walkthrough-enable-replication/enable-replication2.png)

6. I **mål**hello prenumerationen och välj hello resursgrupp som du vill toocreate hello redundansväxlade virtuella datorer. Välj hello distributionsmodell som du vill använda toouse i Azure (klassiska eller resource management), för hello redundansväxlade virtuella datorer.


7. Välj hello Azure storage-konto som du vill använda toouse för replikering av data. Om du inte vill toouse ett konto som du redan har konfigurerat, kan du skapa en ny.

8. Välj hello Azure nätverk och undernät toowhich virtuella Azure-datorer ansluter när de skapas efter växling vid fel. Välj **konfigurera nu för valda datorer**, tooapply hello nätverket inställningen tooall datorer som du väljer för skydd. Välj **konfigurera senare** tooselect hello Azure-nätverk per dator. Om du inte vill toouse ett befintligt nätverk kan skapa du en.

    ![Aktivera replikering](./media/vmware-walkthrough-enable-replication/enable-rep3.png)
9. I **virtuella datorer** > **Välj virtuella datorer**och på varje dator som du vill tooreplicate. Du kan bara välja datorer som stöder replikering. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/vmware-walkthrough-enable-replication/enable-replication5.png)
10. I **egenskaper** > **konfigurera egenskaper**väljer hello-konto som ska användas av hello processen server tooautomatically installera hello mobilitetstjänsten på hello-datorn.
11. Alla diskar replikeras som standard. Klicka på **alla diskar** och avmarkera alla diskar som du inte vill tooreplicate. Klicka sedan på **OK**. Du kan ange ytterligare egenskaper för Virtuella datorer senare.

    ![Aktivera replikering](./media/vmware-walkthrough-enable-replication/enable-replication6.png)
11. I **replikeringsinställningarna** > **konfigurerar replikeringsinställningar**, kontrollera att rätt replikeringsprincip väljs hello. Om du ändrar en princip kommer ändringarna att tillämpade tooreplicating datorn och toonew datorer.
12. Aktivera **konsekvens för flera** om du vill toogather datorer i en replikeringsgrupp och ange ett namn för hello-gruppen. Klicka sedan på **OK**. Tänk på följande:

    * Datorer i replikeringsgrupper replikeras tillsammans, och har delat kraschkonsekvent och programkonsekventa återställningspunkter när de växlar över.
    * Vi rekommenderar att du samla in virtuella datorer och fysiska servrar så att de speglar dina arbetsbelastningar. Aktivera konsekvens för flera kan påverka arbetsbelastningens prestanda och bör endast användas om datorer kör hello samma arbetsbelastning och du behöver enhetlighet.

    ![Aktivera replikering](./media/vmware-walkthrough-enable-replication/enable-replication7.png)
13. Klicka på **Aktivera replikering**. Du kan följa förloppet för hello **Aktivera skydd** jobb **inställningar** > **jobb** > **Site Recovery-jobb**. Efter hello **Slutför skydd** jobbet körs hello datorn är redo för redundans.

## <a name="next-steps"></a>Nästa steg

Gå för[steg 12: kör ett redundanstest](vmware-walkthrough-test-failover.md)
