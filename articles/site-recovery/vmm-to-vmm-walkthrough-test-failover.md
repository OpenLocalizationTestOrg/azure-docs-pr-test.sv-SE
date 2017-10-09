---
title: "aaaRun ett redundanstest för Hyper-V VM replikering tooa sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur toorun ett redundanstest för Hyper-V VM replikering tooa sekundär System Center VMM webbplats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a3fd11ce-65a1-4308-8435-45cf954353ef
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: a60411541c2db001c573735bcb0d651f6618f295
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-run-a-test-failover-for-hyper-v-replication-tooa-secondary-site"></a>Steg 10: Köra ett redundanstest för Hyper-V-replikering tooa sekundär plats


När du har aktiverat replikering för Hyper-V virtuella datorer (VM) med [Azure Site Recovery](site-recovery-overview.md), Använd den här artikeln toorun testa redundans. Ett redundanstest kontrollerar att replikering fungerar utan att påverka produktionsmiljön. 


När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Innan du börjar

- När du utlöser ett redundanstest kan ange du hello nätverket toowhich hello test replik virtuella datorer ska ansluta. [Lär dig mer](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery) om hello Nätverksalternativ.
- Vi rekommenderar att du inte väljer ett nätverk som du valde under nätverksmappning.
- hello anvisningar i den här artikeln beskrivs hur toofail via en enda virtuell dator. Läs mer om [skapa återställningsplaner](site-recovery-create-recovery-plans.md) om du vill ha toofail över flera virtuella datorer tillsammans.
- Läs mer om [begränsningar för redundanstestning](site-recovery-test-failover-vmm-to-vmm.md#things-to-note).
- hello IP-adress angavs tooa VM under testning av redundans är hello samma IP-adress som hello VM skulle få en planerad eller oplanerad växling vid fel (förutsatt att hello IP-adress är tillgänglig i nätverket hello). Om hello IP-adressen är inte tillgänglig i nätverket hello får hello VM en annan IP-adress som är tillgänglig i nätverket hello.
- Om du vill toodo ett redundanstest i produktionsnätverket för fullständig validatation för slutpunkt till slutpunkt anslutningen datorn, Lägg märke till att:
    - hello primära virtuella datorn måste stängas av när du gör hello testa redundans. Annars två virtuella datorer med samma identitet som ska köras i hello hello samma nätverk på hello samtidigt. 
    - Om du ändrar tootest VMs, försvinner dessa ändringar när du rensar hello testa redundans. De här ändringarna inte replikeras tillbaka toohello primära virtuella datorn.
    - Testa en leder toodowntime för din produktionsarbetsbelastningar för ett produktionsnätverk. Be användare inte toouse hello app när hello disaster recovery ökad pågår.  


## <a name="run-a-test-failover-for-a-vm"></a>Köra ett redundanstest för en virtuell dator

1. toofail via en enda virtuell dator i **replikerade objekt**, klicka på hello VM > **Redundanstest**.
2. I **Redundanstestning**, ange hur test virtuella datorer kommer att vara anslutna toonetworks efter hello testa redundans. 
3. Klicka på **OK** toobegin hello redundans. Följa förloppet på hello **jobb** fliken.
5. När redundansväxlingen är klar kontrollerar du att hello test start för virtuella datorer har.
6. När du är klar klickar du på **Rensa redundanstestet** på hello återställningsplan.
7. I **anteckningar**, registrera och spara observationer från redundanstestningen hello. Det här steget tar bort hello virtuella datorer och nätverk som har skapats under redundanstestningen.


## <a name="next-steps"></a>Nästa steg

När du har testat hello distribution, mer information om andra typer av [redundans](site-recovery-failover.md).
