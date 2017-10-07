---
title: "aaaEnable replikering tooAzure för Hyper-V virtuella datorer i VMM-moln med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur tooenable replikering tooAzure för Hyper-V virtuella datorer i VMM moln med hello Azure Site Recovery-tjänsten"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 89a2c4fc-7e03-4a86-a2c0-52831ccebc1a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 1a39bf65490c59a89a5e891184cadfacc2adee6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-11-enable-replication-tooazure-for-hyper-v-vms-in-vmm-clouds"></a>Steg 11: Aktivera replikering tooAzure för Hyper-V virtuella datorer i VMM-moln

När du har konfigurerat en replikeringsprincip använder den här artikeln tooenable replikering för den lokala Hyper-V virtuella datorer (VM) som hanteras i System Center Virtual Machine Manager (VMM) moln), tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md)tjänsten i hello Azure-portalen.

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Innan du börjar

Kontrollera att ditt Azure-konto har rätt hello [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)toocreate virtuella Azure-datorer. [Lär dig mer](../active-directory/role-based-access-built-in-roles.md) om rollbaserad åtkomstkontroll i Azure.

## <a name="exclude-disks-from-replication"></a>Undanta diskar från replikering

Alla diskar på en dator replikeras som standard. Du kan exkludera diskar från replikeringen. Till exempel kanske du inte vill tooreplicate diskar med tillfälliga data eller data som har uppdateras varje gång en dator eller programmet startas om (till exempel pagefile.sys eller SQL Server tempdb). [Läs mer](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a>Replikera virtuella datorer

Aktivera replikering för virtuella datorer på följande sätt:  

1. Klicka på **Steg 2: Replikera program** > **Källa**. När du har aktiverat replikering för hello första gången, klickar du på **+ replikera** i hello valvet tooenable replikering för ytterligare datorer.

    ![Aktivera replikering](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication1.png)
2. I hello **källa** bladet välj hello VMM-servern och hello moln i vilka hello Hyper-V-värdarna finns. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication-source.png)
3. I **mål**hello prenumeration, postredundans distributionsmodell och välj hello storage-konto som du använder för replikerade data.

    ![Aktivera replikering](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication-target.png)
4. Välj hello storage-konto som du vill toouse. Om du vill toouse ett annat lagringskonto än de som du har kan du [skapar du en](#set-up-an-azure-storage-account). Om du använder ett premium-lagringskonto för replikerade data måste tooselect en ytterligare standardlagring konto toostore replikeringsloggar som samlar in löpande ändringar tooon lokala data.toocreate ett lagringskonto med hjälp av hello Resource Manager-modellen Klicka på **Skapa nytt**. Om du vill toocreate ett lagringskonto med hjälp av hello klassiska modellen gör det [i hello Azure-portalen](../storage/common/storage-create-storage-account.md). Klicka sedan på **OK**.
5. Välj hello Azure nätverk och undernät toowhich virtuella Azure-datorer ansluter när de skapas efter växling vid fel. Välj **konfigurera nu för valda datorer**, tooapply hello nätverket inställningen tooall datorer som du väljer för skydd. Välj **konfigurera senare**, tooselect hello Azure-nätverk per dator. Om du vill toouse ett annat nätverk än de som du har kan du [skapar du en](#set-up-an-azure-network). ett nätverk som använder toocreate hello Resource Manager modell klickar du på **Skapa nytt**. Om du vill toocreate ett nätverk med hello klassiska modellen gör det [i hello Azure-portalen](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Välj ett undernät om det behövs. Klicka sedan på **OK**.
6. I **virtuella datorer** > **Välj virtuella datorer**och på varje dator som du vill tooreplicate. Du kan bara välja datorer som stöder replikering. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication5.png)

7. I **egenskaper** > **konfigurera egenskaper**hello operativsystemet för hello valda virtuella datorer och välj hello OS-disken.

    - Kontrollera att hello Azure VM namn (mål) uppfyller [Azure virtuella datorns krav](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).   
    - Som standard markeras alla hello diskar på hello VM för replikering, men du kan rensa diskar tooexclude dem.

        - Du kanske vill tooexclude diskar tooreduce replikering bandbredd. Exempelvis kan du inte vill tooreplicate diskar med tillfälliga data eller data som har uppdateras varje gång en dator eller appar startas om (till exempel pagefile.sys eller Microsoft SQL Server tempdb). Du kan utesluta hello disk från replikering av unselecting hello-disk.
        - Endast standarddiskar kan undantas. Du kan inte undanta OS-diskar.
        - Vi rekommenderar att du inte undantar dynamiska diskar. Site Recovery kan inte identifiera om en virtuell hårddisk är en standarddisk eller dynamisk disk på den virtuella gästdatorn. Om alla diskar för beroende dynamisk volym inte uteslutas visar hello skyddade dynamisk disk som en disk som misslyckats när hello Virtuella växlar och hello data på disken inte är tillgängliga.
        - När replikering har aktiverats kan du inte lägga till eller ta bort diskar för replikering. Om du vill tooadd eller utesluta en disk, du behöver toodisable skydd för hello VM och aktivera det igen.
        - Diskar som du skapar manuellt i Azure växlas inte tillbaka igen. Om du misslyckas under tre diskar och skapa två direkt i Azure VM misslyckades exempelvis endast tre diskar för hello som har redundansväxling tillbaka från Azure tooHyper-V. Du kan inte innehålla diskar som skapats manuellt i återställning efter fel, eller i omvända replikeringen från Hyper-V tooAzure.
        - Om du undantar en disk som behövs för ett program toooperate efter redundans tooAzure måste toocreate det manuellt i Azure, så att hello replikeras program kan köras. Alternativt kan du integrera Azure automation i en återställningsplan, toocreate hello disken under växling vid fel för hello-datorn.

    Klicka på **OK** toosave ändringar. Du kan ange ytterligare egenskaper senare.

    ![Aktivera replikering](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

8. I **replikeringsinställningarna** > **konfigurerar replikeringsinställningar**, Välj hello replikeringsprincip som du vill tooapply för hello skyddade virtuella datorer. Klicka sedan på **OK**. Du kan ändra hello replikeringsprincip i **replikeringsprinciper** > principnamn > **redigera inställningar för**. De ändringar du gör används både för datorer som redan replikeras och för nya datorer.

   ![Aktivera replikering](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication7.png)

Du kan följa förloppet för hello **Aktivera skydd** jobb **jobb** > **Site Recovery-jobb**. Efter hello **Slutför skydd** jobbet körs, hello datorn är redo för redundans.



## <a name="next-steps"></a>Nästa steg

Gå för[steg 12: kör ett redundanstest](vmm-to-azure-walkthrough-test-failover.md)
