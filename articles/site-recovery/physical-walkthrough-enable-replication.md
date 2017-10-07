---
title: "aaaEnable replikering för fysiska servrar som replikerar tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooenable replikering tooAzure för fysiska servrar med hjälp av hello Azure Site Recovery-tjänsten"
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
ms.openlocfilehash: dde4b1463023d2ccefa498f72bb51e57d60ac0d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-enable-replication-for-physical-servers-tooazure"></a>Steg 10: Aktivera replikering för fysiska servrar tooAzure


Den här artikeln beskriver hur tooenable replikering för lokala Windows-/ Linux fysiska servrar tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Innan du börjar

- Servrarna måste ha hello [mobilitetstjänsten installeras](physical-walkthrough-install-mobility.md).
- Om en virtuell dator är förberedd för push-installation installerar hello mobilitetstjänsten automatiskt hello processervern när du aktiverar replikering.
- När du aktiverar en dator för replikering ditt Azure-konto måste specifika [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooensure du är kan toouse Azure storage och skapa virtuella Azure-datorer.
- När du lägger till eller ändra IP-adresser för servrar, kan det ta upp too15 minuter eller längre för ändringar tootake effekt och för dem tooappear hello-portalen.


## <a name="exclude-disks-from-replication"></a>Undanta diskar från replikering

Alla diskar på en dator replikeras som standard. Du kan exkludera diskar från replikeringen. Till exempel kanske du inte vill tooreplicate diskar med tillfälliga data eller data som har uppdateras varje gång en dator eller programmet startas om (till exempel pagefile.sys eller SQL Server tempdb). [Läs mer](site-recovery-exclude-disk.md)

## <a name="replicate-servers"></a>Replikera servrar

1. Klicka på **Steg 2: Replikera program** > **Källa**.
2. I **källa**väljer hello konfigurationsservern.
3. I **datorn typen**väljer **fysiska datorer**.
4. Välj hello processervern. Om du inte skapat några ytterligare servrar blir hello konfigurationsservern. Klicka sedan på **OK**.
5. I **mål**hello prenumerationen och välj hello resursgrupp som du vill toocreate hello redundansväxlade virtuella datorer. Välj hello distributionsmodell som du vill använda toouse i Azure (klassiska eller resource management), för hello redundansväxlade virtuella datorer.
6. Välj hello Azure storage-konto som du vill använda toouse för replikering av data. Om du inte vill toouse ett konto som du redan har konfigurerat, kan du skapa en ny.
7. Välj hello **Azure-nätverk** och **undernät** toowhich virtuella Azure-datorer ansluta efter växling vid fel. Välj **konfigurera nu för valda datorer** tooapply hello nätverket inställningen tooall datorer som du väljer för skydd. Välj **konfigurera senare** tooselect hello Azure-nätverk per dator. Om du inte vill toouse ett befintligt nätverk kan skapa du en.

    ![Aktivera replikering](./media/physical-walkthrough-enable-replication/targetsettings.png)

8. I **fysiska datorer**, klickar du på **+ fysisk dator** och ange hello **namn** och **IP-adress**. Välj operativsystem för hello hello datorn som du vill tooreplicate. Det tar några minuter tills datorer identifieras och visas i hello lista.
9. I **egenskaper** > **konfigurera egenskaper**väljer hello-konto som ska användas av hello processen server tooautomatically installera hello mobilitetstjänsten på hello-datorn.
10. Alla diskar replikeras som standard. Klicka på **alla diskar** och avmarkera alla diskar som du inte vill tooreplicate. Klicka sedan på **OK**. Du kan ange ytterligare egenskaper för Virtuella datorer senare.

    ![Aktivera replikering](./media/physical-walkthrough-enable-replication/enable-replication6.png)
11. I **replikeringsinställningarna** > **konfigurerar replikeringsinställningar**, kontrollera att rätt replikeringsprincip väljs hello. Om du ändrar en princip kommer ändringarna att tillämpade tooreplicating datorn och toonew datorer.
12. Aktivera **konsekvens för flera** om du vill toogather datorer i en replikeringsgrupp och ange ett namn för hello-gruppen. Klicka sedan på **OK**. Tänk på följande:

    * Datorer i replikeringsgrupper replikeras tillsammans, och har delat kraschkonsekvent och programkonsekventa återställningspunkter när de växlar över.
    * Vi rekommenderar att du samla fysiska servrar så att de speglar dina arbetsbelastningar. Aktivera konsekvens för flera kan påverka arbetsbelastningens prestanda och bör endast användas om datorer kör hello samma arbetsbelastning och du behöver enhetlighet.

13. Klicka på **Aktivera replikering**. Du kan följa förloppet för hello **Aktivera skydd** jobb **inställningar** > **jobb** > **Site Recovery-jobb**. Efter hello **Slutför skydd** jobbet körs hello datorn är redo för redundans.

## <a name="next-steps"></a>Nästa steg

Gå för[steg 11: kör ett redundanstest](physical-walkthrough-test-failover.md)
