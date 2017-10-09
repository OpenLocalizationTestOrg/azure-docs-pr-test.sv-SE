---
title: "aaaEnable Hyper-V-replikering tooa sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur tooenable replikering för Hyper-V virtuella datorer replikeras tooa sekundär System Center VMM webbplats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d8782d14-9fef-4396-8912-ff945efc851b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: b4484e0118cb23f343187fe867d9795d30926baf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-enable-replication-tooa-secondary-site-for-hyper-v-vms"></a>Steg 9: Aktivera replikering tooa sekundär plats för Hyper-V virtuella datorer


När du har installerat en replikeringsprincip, Använd den här artikeln tooenable replikering tooa sekundär System Center Virtual Machine Manager (VMM) webbplatsen för lokala Hyper-V virtuella datorer (VM) med [Azure Site Recovery](site-recovery-overview.md).

När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="select-vms-tooreplicate"></a>Välj tooreplicate för virtuella datorer

1. Klicka på **Steg 2: Replikera program** > **Källa**. 

    ![Aktivera replikering](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication1.png)

2. I **källa**hello VMM-servern och välj hello molnet vilka hello Hyper-V-värdar som du vill tooreplicate finns. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication2.png)
3. I **mål**, kontrollera hello sekundär VMM-servern och molnet.
4. I **virtuella datorer**, Välj hello virtuella datorer du vill tooprotect hello-listan.

    ![Aktivera skydd för en virtuell dator](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication5.png)

Du kan följa förloppet för hello **Aktivera skydd** åtgärden i **jobb** > **Site Recovery-jobb**. Efter hello **Slutför skydd** jobbet är slutfört, hello inledande replikeringen är klar och hello virtuella datorn är redo för redundans.

Tänk på följande:

- Du kan också aktivera skydd för virtuella datorer i hello VMM-konsolen. Klicka på **Aktivera skydd** hello verktygsfältet i hello egenskaper för virtuell dator > **Azure Site Recovery** fliken.
- När du har aktiverat replikering, kan du visa egenskaperna för hello VM i **replikerade objekt**. På hello **Essentials** instrumentpanel, visas information om hello replikeringsprincip för hello VM och dess status. Klicka på **egenskaper** för mer information.

## <a name="onboard-existing-vms"></a>Publicera befintliga virtuella datorer

Om du har befintliga virtuella datorer i VMM som replikerar med Hyper-V-replikering, kan du publicera dem för Azure Site Recovery replikering på följande sätt:

1. Kontrollera att hello Hyper-V-server som är värd hello befintlig virtuell dator finns i hello primära molnet och den hello Hyper-V-server som värd för hello replikerade virtuella datorn finns i hello sekundära moln.
2. Kontrollera att en replikeringsprincip har konfigurerats för hello primära VMM-moln.
3. Aktivera replikering för hello primära virtuella datorn. Azure Site Recovery och VMM säkerställa att hello samma replikeringsvärd och den virtuella datorn har identifierats och Azure Site Recovery återanvänder och återupprätta replikering med hello angivna inställningar.


## <a name="next-steps"></a>Nästa steg

Gå för[steg 10: köra ett redundanstest](vmm-to-vmm-walkthrough-test-failover.md).
