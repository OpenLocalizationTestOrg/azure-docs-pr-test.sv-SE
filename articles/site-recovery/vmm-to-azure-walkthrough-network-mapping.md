---
title: "aaaConfigure nätverksmappningen för att replikera virtuella Hyper-V-datorer i VMM-moln tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur tooconfigure nätverksmappning vid replikering av Hyper-V virtuella datorer i VMM moln tooAzure med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 081a9fdb0ffa4114099e9bcb9c1b1e43ad26ecbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-configure-network-mapping-for-hyper-v-replication-with-vmm-tooazure"></a>Steg 9: Konfigurera nätverksmappning för tooAzure för Hyper-V-replikering (med VMM)

När du har skapat hello [käll- och replikeringsinställningarna](vmm-to-azure-walkthrough-source-target.md), Använd den här artikeln tooconfigure nätverk mappning toomap mellan lokala nätverk för virtuell dator i VMM och Azure-nätverk.

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Innan du börjar

- Lär dig mer om [nätverksmappning](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).
- [Förbereda VMM för nätverksmappning](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping). 
- Kontrollera att virtuella datorer på hello VMM-servern är ansluten tooa Virtuellt datornätverk och att du har skapat minst ett virtuellt Azure-nätverk. Flera Virtuella datornätverk kan vara mappade tooa enda Azure-nätverk.

## <a name="configure-mapping"></a>Konfigurera mappning

Konfigurera mappning på följande sätt:

1. I **Site Recovery-infrastruktur** > **nätverksmappningar** > **nätverksmappning**, klicka på hello **+ nätverksmappning**  ikon.

    ![Nätverksmappning](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping1.png)
2. I **Lägg till nätverksmappning**väljer hello VMM-källservern och **Azure** som hello mål.
3. Kontrollera hello prenumeration och hello distributionsmodell efter växling vid fel.
4. I **Källnätverk**väljer hello lokala VM-källnätverket önskade toomap hello listan som är associerade med hello VMM-servern.
5. I **målnätverket**, Välj hello Azure-nätverk i vilka replik virtuella Azure-datorer kommer att finnas när de skapas. Klicka sedan på **OK**.

    ![Nätverksmappning](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping2.png)

Det här händer när nätverksmappningen börjar:

* Befintliga virtuella datorer i VM-källnätverket hello är anslutna toohello målnätverket när mappningen börjar. Nya virtuella datorer anslutna toohello källnätverket ansluts toohello mappade Azure-nätverket när replikeringen sker.
* Om du ändrar en befintlig nätverksmappning ansluts virtuella replikdatorer med hello nya inställningar.
* Om hello målnätverket har flera undernät och ett av dessa undernät har hello samma namn som undernätet som hello källa virtual machine finns så hello replikerade virtuella datorn ansluter toothat målundernätverket efter en redundansväxling.
* Om det inte finns något målundernät med ett matchande namn, ansluter hello virtuella toohello första undernätet i hello nätverk.



## <a name="next-steps"></a>Nästa steg

Gå för[steg 10: skapa en replikeringsprincip](vmm-to-azure-walkthrough-replication.md)
