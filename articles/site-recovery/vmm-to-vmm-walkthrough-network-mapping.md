---
title: "aaaMap nätverk för Virtuella Hyper-V replikering tooa sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur toomap nätverk när du replikerar virtuella Hyper-V-datorer tooa sekundär VMM-plats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 461b7c1c-ef61-4005-aeec-2324e727a3d0
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: d4f621df4ce08ae055bc6809daea0b71b76754ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-map-networks-for-hyper-v-vm-replication-tooa-secondary-site"></a>Steg 7: Mappa nätverk för Virtuella Hyper-V-replikering tooa sekundär plats


När du har installerat [käll- och inställningar](vmm-to-vmm-walkthrough-source-target.md) för replikering av Hyper-V virtuella datorer (VM) tooa sekundär System Center Virtual Machine Manager (VMM) plats, använder du den här artikeln tooconfigure nätverksmappningen för Hyper-V virtuell dator (VM) replikering tooa sekundära platsen med [Azure Site Recovery](site-recovery-overview.md).

När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Innan du börjar

- Lär dig mer om [nätverksmappning](vmm-to-vmm-walkthrough-network.md#network-mapping-overview) innan du börjar.
- Kontrollera att virtuella datorer på VMM-servrar är anslutna tooa VM-nätverk.

## <a name="configure-network-mapping"></a>Konfigurera nätverksmappning

1. I **nätverksmappning** > **nätverksmappningar**, klickar du på **+ nätverksmappning**.
2. I **Lägg till nätverksmappning** väljer hello källa och mål VMM-servrar. hello Virtuella datornätverk kopplade till hello VMM-servrar har hämtats.
3. I **Källnätverk**väljer hello-nätverk som du vill toouse hello listan över Virtuella datornätverk kopplade till hello primära VMM-servern.
4. I **målnätverket**väljer hello-nätverk som du vill använda toouse på hello sekundär VMM-servern. Klicka sedan på **OK**.

    ![Nätverksmappning](./media/vmm-to-vmm-walkthrough-network-mapping/network-mapping2.png)

Det här händer när nätverksmappningen börjar:

* Alla befintliga virtuella replikdatorer som motsvarar toohello källnätverket blir anslutna toohello mål VM-nätverk.
* Nya virtuella datorer som är anslutna toohello källnätverket blir anslutna toohello Målnätverk mappade efter replikeringen.
* Om du ändrar en befintlig mappning med ett nytt nätverk ansluts virtuella replikdatorer med de nya inställningarna för hello.
* Om hello målnätverket har flera undernät och ett av dessa undernät har hello samma namn som undernätet på vilka hello virtuella källdatorn finns så hello replikerade virtuella datorn kommer att vara anslutna toothat målundernätverket efter växling vid fel. Om det inte finns något målundernät med ett matchande namn, att hello virtuella datorn anslutna toohello första undernätet i hello nätverk.



## <a name="next-steps"></a>Nästa steg

Gå för[steg 8: Konfigurera en replikeringsprincip](vmm-to-vmm-walkthrough-replication.md).
