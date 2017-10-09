---
title: "aaaEnable replikering för Hyper-V virtuella datorer replikeras tooAzure (utan System Center VMM) med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooenable replikering tooAzure för Hyper-V virtuella datorer med hjälp av hello Azure Site Recovery-tjänsten"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: bd31ef01-69f1-4591-a519-e42510a6e2f4
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 1589cb7aa1fe954e075cb7bf1f4a4ec199ed3ec7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-enable-replication-for-hyper-v-vms-replicating-tooazure"></a>Steg 10: Aktivera replikering för Hyper-V virtuella datorer replikeras tooAzure


Den här artikeln beskriver hur tooenable replikering för den lokala Hyper-V virtuella datorer (som inte hanteras av System Center VMM) tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="before-you-start"></a>Innan du börjar

Innan du börjar bör du kontrollera att ditt Azure-konto har hello krävs [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikering av en ny virtuell dator tooAzure.

## <a name="exclude-disks-from-replication"></a>Undanta diskar från replikering

Alla diskar på en dator replikeras som standard. Du kan exkludera diskar från replikeringen. Till exempel kanske du inte vill tooreplicate diskar med tillfälliga data eller data som har uppdateras varje gång en dator eller programmet startas om (till exempel pagefile.sys eller SQL Server tempdb). [Läs mer](site-recovery-exclude-disk.md)


## <a name="replicate-vms"></a>Replikera virtuella datorer

Aktivera replikering för virtuella datorer på följande sätt:          

1. Klicka på **replikera program** > **källa**. När du har konfigurerat replikering för hello första gången du klickar på **+ replikera** tooenable replikering för ytterligare datorer.

    ![Aktivera replikering](./media/hyper-v-site-walkthrough-enable-replication/enable-replication.png)
2. I **källa**väljer hello Hyper-V-platsen. Klicka sedan på **OK**.
3. I **mål**hello valvet prenumerationen och välj hello failover-modell som du vill ha toouse i Azure (klassiska eller resource management) efter växling vid fel.
4. Välj hello storage-konto som du vill toouse. Om du inte har ett konto som du vill toouse, kan du [skapar du en](#set-up-an-azure-storage-account). Klicka sedan på **OK**.
5. Välj hello Azure nätverk och undernät toowhich virtuella Azure-datorer ansluter när de skapas växling vid fel.

    - tooapply hello inställningar tooall datorer aktiveras för replikering, Välj **konfigurera nu för valda datorer**.
    - Välj **konfigurera senare** tooselect hello Azure-nätverk per dator.
    - Om du inte har ett nätverk som du vill toouse, kan du [skapar du en](#set-up-an-azure-network). Välj ett undernät om det behövs. Klicka sedan på **OK**.

   ![Aktivera replikering](./media/hyper-v-site-walkthrough-enable-replication/enable-replication11.png)

6. I **virtuella datorer** > **Välj virtuella datorer**och på varje dator som du vill tooreplicate. Du kan bara välja datorer som stöder replikering. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/hyper-v-site-walkthrough-enable-replication/enable-replication5-for-exclude-disk.png)

7. I **egenskaper** > **konfigurera egenskaper**hello operativsystemet för hello valda virtuella datorer och välj hello OS-disken.
8. Kontrollera att hello Azure VM namn (mål) uppfyller [Azure virtuella datorns krav](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
9. Som standard markeras alla hello diskar på hello VM för replikering. Rensa diskar tooexclude dem.
10. Klicka på **OK** toosave ändringar. Du kan ange ytterligare egenskaper senare.

    ![Aktivera replikering](./media/hyper-v-site-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

11. I **replikeringsinställningarna** > **konfigurerar replikeringsinställningar**, Välj hello replikeringsprincip som du vill tooapply för hello skyddade virtuella datorer. Klicka sedan på **OK**. Du kan ändra hello replikeringsprincip i **replikeringsprinciper** > principnamn > **redigera inställningar för**. De ändringar du gör används både för datorer som redan replikeras och för nya datorer.


   ![Aktivera replikering](./media/hyper-v-site-walkthrough-enable-replication/enable-replication7.png)

Du kan följa förloppet för hello **Aktivera skydd** jobb **jobb** > **Site Recovery-jobb**. Efter hello **Slutför skydd** jobbet körs hello datorn är redo för redundans.


## <a name="next-steps"></a>Nästa steg


Gå för[steg 11: kör ett redundanstest](hyper-v-site-walkthrough-test-failover.md)
