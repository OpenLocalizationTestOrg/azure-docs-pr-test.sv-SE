---
title: "aaaRun ett redundanstest för VMware replikering tooAzure | Microsoft Docs"
description: "Sammanfattar hello stegen för att köra ett redundanstest för VMwares virtuella datorer replikeras tooAzure hello Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a640e139-3a09-4ad8-8283-8fa92544f4c6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: bed934446f0c6940089bfae24a13de4fb1556a62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-12-run-a-test-failover-tooazure-for-vmware-vms"></a>Steg 12: Kör ett test redundans tooAzure för VMwares virtuella datorer

Den här artikeln beskriver hur toorun ett redundanstest från lokala VMware virtuella datorer tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Innan du börjar

Innan du kör ett redundanstest rekommenderar vi att verifiera hello VM-egenskaper och gör eventuella ändringar behöver du. Du kan komma åt hello VM egenskaper i **replikerade objekt**. Hej **Essentials** bladet visar information om inställningar för datorer och status.

## <a name="managed-disk-considerations"></a>Överväganden för hanterade diskar

[Hanterade diskar](../virtual-machines/windows/managed-disks-overview.md) förenkla Diskhantering för virtuella Azure-datorer genom att hantera hello storage-konton som är associerade med hello Virtuella diskar. 

- När du aktiverar skyddet för en virtuell dator replikeras VM-data tooa storage-konto. Hanterade diskar skapas och anslutna toohello VM endast när växling vid fel.
- Hanterade diskar kan endast skapas för virtuella datorer som distribueras med hjälp av hello Resource Manager-modellen.  
- Den här inställningen bara tillgänglighetsuppsättningar i resursgrupper som har **Använd hanterade diskar** aktiverat kan väljas. Virtuella datorer med hanterade diskar måste vara i tillgänglighetsuppsättningar med **Använd hanterade diskar** ställa in också**Ja**. Om hello inställningen inte är aktiverad för virtuella datorer kan bara tillgänglighetsuppsättningar resursgrupper utan hanterade diskar aktiverad markeras.
- [Lär dig mer](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set) om hanterade diskar och tillgänglighet anger.
- Om hello storage-konto du använder för replikering har krypterats med Storage Service-kryptering, kan hanterade diskar inte skapas under växling vid fel. I det här fallet antingen inte aktivera användning av hanterade diskar, eller inaktivera skyddet för hello VM och återaktivera det toouse ett lagringskonto som inte har kryptering aktiverat. [Läs mer](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="network-considerations"></a>Nätverksöverväganden

Du kan ange IP-adressen för hello mål för en Azure VM som skapades efter redundansväxlingen.

- Om du inte anger någon adress använder hello redundansväxlade datorn DHCP.
- Om du anger en adress som inte är tillgänglig under växling vid fel, fungerar inte växling vid fel.
- hello samma mål-IP-adress kan användas för att testa redundans om adressen hello är tillgänglig i nätverket hello.
- hello antalet nätverkskort beror hello storleken du anger för hello virtuella måldatorn:

     - Om hello antalet nätverkskort på källdatorn hello är hello samma som eller mindre än hello antalet nätverkskort som tillåts för hello måldatorn sedan hello målet att ha hello samma antal kort som hello källa.
     - Om hello antalet nätverkskort för hello virtuella källdatorn överskrider hello antal för hello Målstorlek hello högsta ska användas.
     - Om en källdator har två nätverkskort och hello måldatorn stöder fyra, kommer hello måldatorn ha två nätverkskort. Om hello källdatorn har två nätverkskort men hello målstorleken endast stöder ett att hello måldatorn ha en enda nätverkskort.     
   - Om hello virtuella datorn har flera nätverkskort ansluts alla toohello samma nätverk.
   - Om hello virtuella datorn har flera nätverkskort hello först visas i listan hello blir hello *standard* hello Azure virtuella nätverkskort.
 - [Lär dig mer](vmware-walkthrough-network.md) om IP-adresser.



## <a name="view-and-modify-vm-settings"></a>Visa och ändra inställningarna för VM

Vi rekommenderar att du kontrollerar hello egenskaper för hello källdatorn innan du kör en redundansväxling.

1. I **skyddade objekt**, klickar du på **replikerade objekt**, och klicka på hello VM.
2. I hello **replikerade objekt** fönstret visas en sammanfattning av VM-information, hälsotillstånd och hello senaste tillgängliga återställningspunkter. Klicka på **egenskaper** tooview mer information.
3. I **beräknings- och nätverksinställningar**, kan du:
    - Ändra hello Azure VM-namn. hello-namnet måste uppfylla [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
    - Ange postredundans [resursgrupp].
    - Ange en Målstorlek för hello Azure VM
    - Välj en [tillgänglighetsuppsättning](../virtual-machines/windows/tutorial-availability-sets.md).
    - Ange om toouse [hanterade diskar](#managed-disk-considerations). Välj **Ja**, om du vill tooattach hanterade diskar tooyour datorn på tooAzure för migrering.
    - Visa eller ändra inställningar för nätverk, inklusive hello /-undernätet i vilka hello Azure VM kommer att finnas efter växling vid fel och hello IP-adress som ska tilldelas tooit.
4. I **diskar**, visas information om hello operativsystem och datadiskar på hello VM.

## <a name="run-a-test-failover"></a>Köra ett redundanstest

När du har konfigurerat allt, kör du en testa redundans toomake att allt fungerar som förväntat.

- Om du vill tooconnect tooAzure virtuella datorer med RDP efter en växling vid fel, [förbereda tooconnect](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - toofully test som du behöver toocopy Active Directory och DNS i din testmiljö. [Läs mer](site-recovery-active-directory.md#test-failover-considerations).
 - Fullständig information om redundanstestet [i den här artikeln](site-recovery-test-failover-to-azure.md) artikel.
- Få en snabb överblick av video innan du börjar:


     >[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video4-Recovery-Plan-DR-Drill-and-Failover/player]


Kör nu en växling vid fel:

1. toofail över en enskild dator i **inställningar** > **replikerade objekt**, klicka på hello VM > **+ testa redundans** ikon.

    ![Testa redundans](./media/vmware-walkthrough-test-failover/test-failover.png)

2. toofail via en återställning planera i **inställningar** > **Återställningsplaner**, högerklicka på hello plan > **Redundanstestningen**. toocreate en återställningsplan [följer du dessa instruktioner](site-recovery-create-recovery-plans.md).  

3. I **Redundanstest**väljer hello Azure-nätverk toowhich virtuella Azure-datorer ska ansluta till efter redundansväxlingen.

4. Klicka på **OK** toobegin hello redundans. Du kan följa förloppet genom att klicka på hello VM tooopen dess egenskaper eller på hello **Redundanstest** jobb i valvnamnet > **inställningar** > **jobb**  >  **Site Recovery-jobb**.

5. När hello växling vid fel har slutförts kan du bör även kunna toosee hello replik Azure datorn visas i hello Azure-portalen > **virtuella datorer**. Kontrollera att hello VM är hello lämplig storlek som den är ansluten toohello lämpligt nätverk och att den körs.

6. Om du har förberett för anslutningar efter växling vid fel, bör du kunna tooconnect toohello Azure VM.

7. När du är klar klickar du på **Rensa redundanstestet** på hello återställningsplan. I **anteckningar**, registrera och spara observationer från redundanstestningen hello. Detta tar bort hello virtuella datorer som har skapats under redundanstestningen.



## <a name="next-steps"></a>Nästa steg

- [Lär dig mer](site-recovery-failover.md) om olika typer av växling vid fel, och hur toorun dem.
- Om du migrerar datorer i stället för att replikera och misslyckas igen, [mer](site-recovery-migrate-to-azure.md#migrate-on-premises-vms-and-physical-servers).
- [Läs mer om återställning efter fel](site-recovery-failback-azure-to-vmware.md), toofail tillbaka och replikera virtuella datorer i Azure tillbaka toohello primära lokal plats.
