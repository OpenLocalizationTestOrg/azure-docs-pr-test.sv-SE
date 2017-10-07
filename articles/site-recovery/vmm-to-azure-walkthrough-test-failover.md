---
title: "aaaRun ett redundanstest för tooAzure för Hyper-V-replikering (med System Center VMM) | Microsoft Docs"
description: "Sammanfattar hello stegen för att köra ett redundanstest för Hyper-V virtuella datorer i VMM-moln, replikerar tooAzure hello Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 7b562a23-7ba7-48ee-905d-c22b4b5d6466
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/25/2017
ms.author: raynew
ms.openlocfilehash: fc60e536f2eeb6f95dde3d347f364f3bf8bfdf45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-11-run-a-test-failover-for-hyper-v-replication-with-vmm-tooazure"></a>Steg 11: Kör ett redundanstest för tooAzure för Hyper-V-replikering (med VMM)

När du [Aktivera replikering för virtuella datorer som Hyper-V](vmm-to-azure-walkthrough-enable-replication.md), Använd den här artikeln toorun ett redundanstest från lokala virtuella Hyper-V-datorer hanteras i System Center Virtual Machine Manager (VMM) moln tooAzure, med hjälp av hello [ Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Innan du börjar

Innan du kör ett redundanstest rekommenderar vi att verifiera hello VM-egenskaper och gör eventuella ändringar behöver du. Du kan komma åt hello VM egenskaper i **replikerade objekt**. Hej **Essentials** bladet visar information om inställningar för datorer och status.

## <a name="managed-disk-considerations"></a>Överväganden för hanterade diskar

[Hanterade diskar](../virtual-machines/windows/managed-disks-overview.md) förenkla Diskhantering för virtuella Azure-datorer genom att hantera hello storage-konton som är associerade med hello Virtuella diskar. 

- Hanterade diskar skapas och anslutna toohello VM bara när en tooAzure för växling vid fel inträffar. När du aktiverar skyddet replikerar data från lokala virtuella datorer toostorage konton.
- Hanterade diskar kan endast skapas för virtuella datorer som distribueras med hello Resource manager-distributionsmodellen.
- Återställning från Azure tooan lokal Hyper-V-miljön inte stöds för närvarande för datorer med hanterade diskar. Du bör endast ange **Använd hanterade diskar** för**Ja** om du utför en migrering endast (failover tooAzure utan återställning efter fel)
- Den här inställningen bara tillgänglighetsuppsättningar i resursgrupper som har **Använd hanterade diskar** aktiverat kan väljas. Virtuella datorer med hanterade diskar måste vara i tillgänglighetsuppsättningar med **Använd hanterade diskar** ställa in också**Ja**. Om hello inställningen inte är aktiverad för virtuella datorer kan bara tillgänglighetsuppsättningar resursgrupper utan hanterade diskar aktiverad markeras. [Läs mer](../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set).
- - Om hello storage-konto du använder för replikering har krypterats med Storage Service-kryptering, kan hanterade diskar inte skapas under växling vid fel. I det här fallet antingen inte aktivera användning av hanterade diskar, eller inaktivera skyddet för hello VM och återaktivera det toouse ett lagringskonto som inte har kryptering aktiverat. [Läs mer](../virtual-machines/windows/managed-disks-overview.md#managed-disks-and-encryption).

 
## <a name="network-considerations"></a>Nätverksöverväganden
    
- Du kan ange hello mål-IP-adress toobe används för hello Azure VM efter växling vid fel. Om du inte anger någon adress använder hello redundansväxlade datorn DHCP. Om du anger en adress som inte är tillgänglig under redundansväxlingen, misslyckas hello växling vid fel. hello samma mål-IP-adress kan användas för att testa redundans om adressen hello är tillgänglig i nätverket hello.
- hello antalet nätverkskort beror hello storleken du anger för hello för den virtuella måldatorn enligt följande:
    - Om hello antalet nätverkskort på hello källdatorn är mindre än eller lika med toohello antal nätverkskort som tillåts för hello måldatorn och sedan hello målet att ha hello samma antal kort som hello källa.
    - Om hello antalet nätverkskort för hello virtuella källdatorn överskrider hello tillåtna antal för hello målstorleken så hello högsta kommer att användas.
    - Till exempel om en källdator har två nätverkskort och hello måldatorn stöder fyra hello måldatorn att ha två kort. Om hello källdatorn har två nätverkskort men hello målstorleken endast stöder ett att hello måldatorn ha en enda nätverkskort.     
- Om hello VM har flera nätverkskort ansluts alla toohello samma nätverk.
- Om hello virtuella datorn har flera nätverkskort hello först visas i listan hello blir hello *standard* hello Azure virtuella nätverkskort.


## <a name="view-and-manage-vm-settings"></a>Visa och hantera inställningar för virtuell dator

Vi rekommenderar att du kontrollerar hello egenskaper för hello källdatorn innan du kör en redundansväxling.

1. I **skyddade objekt**, klickar du på **replikerade objekt**, och klicka på hello VM.

    ![Aktivera replikering](./media/vmm-to-azure-walkthrough-test-failover/test-failover1.png)
2. I hello **replikerade objekt** fönstret visas en sammanfattning av VM-information, hälsotillstånd och hello senaste tillgängliga återställningspunkter. Klicka på **egenskaper** tooview mer information.

    ![Aktivera replikering](./media/vmm-to-azure-walkthrough-test-failover/test-failover2.png)
3. I **beräknings- och nätverksinställningar**, kan du:
    - Ändra hello Azure VM-namn. hello-namnet måste uppfylla [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
    - Ange postredundans [resursgrupp].
    - Ange en Målstorlek för hello Azure VM
    - Välj en [tillgänglighetsuppsättning](../virtual-machines/windows/tutorial-availability-sets.md).
    - Ange om toouse [hanterade diskar](#managed-disk-considerations). Välj **Ja**, om du vill tooattach hanterade diskar tooyour datorn på tooAzure för migrering.
    - Visa eller ändra inställningar för nätverk, inklusive hello /-undernätet i vilka hello Azure VM kommer att finnas efter växling vid fel och hello IP-adress som ska tilldelas tooit.

    ![Aktivera replikering](./media/vmm-to-azure-walkthrough-test-failover/test-failover4.png)
4. I **diskar**, visas information om hello operativsystem och datadiskar på hello VM.


## <a name="run-a-test-failover"></a>Köra ett redundanstest

Kör nu en testa redundans toomake att allt fungerar som förväntat.

- Om du vill tooconnect tooAzure virtuella datorer med RDP efter en växling vid fel, [förbereda tooconnect](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - toofully test som du behöver toocopy Active Directory och DNS i din testmiljö. [Läs mer](site-recovery-active-directory.md#test-failover-considerations).
 - Fullständig information om redundanstestet [i den här artikeln](site-recovery-test-failover-to-azure.md) artikel.
 
 Kör nu en växling vid fel:

1. toofail över en enskild dator i **replikerade objekt**, klicka på hello VM > **+ testa redundans** ikon.
2. toofail över en återställning planera i **Återställningsplaner**, högerklicka på hello plan > **Redundanstest**. toocreate en återställningsplan [följer du dessa instruktioner](site-recovery-create-recovery-plans.md).
3. I **Redundanstest**väljer hello Azure-nätverk toowhich virtuella Azure-datorer ska ansluta till efter redundansväxlingen.
4. Klicka på **OK** toobegin hello redundans. Du kan följa förloppet genom att klicka på hello VM tooopen dess egenskaper eller på hello **Redundanstest** jobb i valvnamnet > **jobb** > **Site Recovery-jobb**.
5. När hello växling vid fel har slutförts kan du bör även kunna toosee hello replik Azure datorn visas i hello Azure-portalen > **virtuella datorer**. Kontrollera att hello VM är hello lämplig storlek som den är ansluten toohello lämpligt nätverk och att den körs.
6. Om du har förberett för anslutningar efter växling vid fel, bör du kunna tooconnect toohello Azure VM.
7. När du är klar klickar du på **Rensa redundanstestet** på hello återställningsplan. I **anteckningar** spela in och spara observationer från redundanstestningen hello. Detta tar bort hello virtuella datorer som har skapats under redundanstestningen.



## <a name="next-steps"></a>Nästa steg

- [Lär dig mer](site-recovery-failover.md) om olika typer av växling vid fel, och hur toorun dem.
- [Läs mer om återställning efter fel](site-recovery-failback-from-azure-to-hyper-v.md), toofail tillbaka och replikera virtuella datorer i Azure tillbaka toohello primär lokal VMM-moln.

