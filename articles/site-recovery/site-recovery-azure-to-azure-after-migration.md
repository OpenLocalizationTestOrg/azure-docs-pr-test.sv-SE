---
title: "aaaPrepare datorer tooset in katastrofåterställning mellan Azure-regioner efter migrering tooAzure genom att använda Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur tooprepare datorer tooset in katastrofåterställning mellan Azure-regioner efter migrering tooAzure med hjälp av Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: ponatara
ms.openlocfilehash: b6274e3df210c1d8a7b8289cc85868ee6414e523
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-tooanother-region-after-migration-tooazure-by-using-azure-site-recovery"></a>Replikera virtuella datorer i Azure tooanother region efter migrering tooAzure med hjälp av Azure Site Recovery

>[!NOTE]
> Azure Site Recovery replikering för virtuella Azure-datorer (VM) är för närvarande under förhandsgranskning.

## <a name="overview"></a>Översikt

Den här artikeln hjälper dig att förbereda virtuella Azure-datorer för replikering mellan två Azure-regioner när dessa datorer har migrerats från en lokal miljö tooAzure med hjälp av Azure Site Recovery.

## <a name="disaster-recovery-and-compliance"></a>Katastrofåterställning och efterlevnad
Idag Flyttar fler och fler företag sina arbetsbelastningar tooAzure. Med företag som flyttar verksamhetskritiska lokalt produktion är arbetsbelastningar tooAzure, ställa in katastrofåterställning för dessa arbetsbelastningar obligatoriskt för efterlevnad och toosafeguard mot eventuella avbrott i en Azure-region.

## <a name="steps-for-preparing-migrated-machines-for-replication"></a>Steg för att förbereda migrerade datorer för replikering
tooprepare migrerade datorer för att konfigurera replikering tooanother Azure-region:

1. Slutföra migreringen.
2. Installera hello Azure-agenten om det behövs.
3. Ta bort hello mobilitetstjänsten.  
4. Starta om hello VM.

Dessa steg beskrivs i detalj i följande avsnitt hello.

### <a name="step-1-migrate-workloads-running-on-hyper-v-vms-vmware-vms-and-physical-servers-toorun-on-azure-vms"></a>Steg 1: Migrera arbetsbelastningar som körs på Hyper-V virtuella datorer, virtuella VMware-datorer och fysiska servrar toorun på Azure Virtual Machines

tooset upp replikering och migrera dina lokala Hyper-V, VMware och fysiska arbetsbelastningar tooAzure Följ hello stegen i hello [migrera Azure IaaS-virtuella datorer mellan Azure-regioner med Azure Site Recovery](site-recovery-migrate-to-azure.md) artikel. 

Efter migreringen kan du inte behöver toocommit eller ta bort en växling vid fel. I stället väljer hello **slutföra migreringen** alternativet för varje dator som du vill toomigrate:
1. I **replikerade objekt**och högerklicka på hello VM, klicka på **slutföra migreringen**. Klicka på **OK** toocomplete hello steg. Du kan följa förloppet i hello VM egenskaper genom att övervaka hello fullständig migreringsjobb i **Site Recovery-jobb**.
2. Hej **slutföra migreringen** åtgärden har slutförts hello migreringsprocessen, tar du bort replikeringen av hello datorn och stoppar Site Recovery-faktureringen för hello datorn.

   ![fullständig migrering](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

### <a name="step-2-install-hello-azure-vm-agent-on-hello-virtual-machine"></a>Steg 2: Installera hello Azure VM-agenten på hello virtuell dator
hello Azure [VM-agenten](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) måste vara installerad på hello virtuell dator för hello Site Recovery tillägget toowork och toohelp skydda hello VM.

>[!IMPORTANT]
>Från och med version 9.7.0.0, på Windows-datorer installeras hello Mobilitetstjänstens installationsprogram också hello senaste tillgängliga Virtuella Azure-agenten. Om migrering uppfyller hello virtuella installationen av nödvändiga för att använda alla VM-tillägg, inklusive hello Site Recovery-tillägget. hello Azure VM-agenten måste toobe manuellt installerade endast om hello mobilitetstjänsten installeras på hello migrerad dator är version 9,6 eller tidigare.

hello innehåller följande tabell ytterligare information om hur du installerar hello VM-agenten och verifiera att den har installerats:

| **Åtgärd** | **Windows** | **Linux** |
| --- | --- | --- |
| Installera hello VM-agent |Hämta och installera hello [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Du måste administratören privilegier toocomplete hello installation. |Installera hello senaste [Linux-agenten](../virtual-machines/linux/agent-user-guide.md). Du måste administratören privilegier toocomplete hello installation. Vi rekommenderar att du installerar hello agent från databasen för din distribution. Vi *rekommenderar inte* installera hello Linux VM-agenten direkt från GitHub.  |
| Verifiera hello VM agentinstallation |1. Bläddra toohello C:\WindowsAzure\Packages mapp i hello Azure VM. Du bör se hello WaAppAgent.exe fil. <br>2. Högerklicka på filen hello finns för**egenskaper**, och välj sedan hello **information** fliken hello **produktversionen** fältet måste innehålla 2.6.1198.718 eller högre. |Saknas |


### <a name="step-3-remove-hello-mobility-service-from-hello-migrated-virtual-machine"></a>Steg 3: Ta bort hello mobilitetstjänsten från hello migrerad virtuell dator

Om du har migrerat dina lokala VMware-datorer eller fysiska servrar på Windows-/ Linux, måste toomanually ta bort eller avinstallera hello mobilitetstjänsten från hello migrerade virtuella datorn.

>[!IMPORTANT]
>Det här steget krävs inte för migrerade tooAzure för Hyper-V virtuella datorer.

#### <a name="uninstall-hello-mobility-service-on-a-windows-server-vm"></a>Avinstallera hello mobilitetstjänsten på en Windows Server-VM
Använd någon av hello följande metoder toouninstall hello mobilitetstjänsten på en Windows Server-dator.

##### <a name="uninstall-by-using-hello-windows-ui"></a>Avinstallera med hello användargränssnitt för Windows
1. I hello Kontrollpanelen, Välj **program**.
2. Välj **Microsoft Azure Site Recovery Mobility tjänsten eller Huvudtjänsten målservern**, och välj sedan **avinstallera**.

##### <a name="uninstall-at-a-command-prompt"></a>Avinstallera i en kommandotolk
1. Öppna ett kommandotolksfönster som administratör.
2. toouninstall Hej mobilitetstjänsten, kör hello följande kommando:

   ```
   MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
   ```

#### <a name="uninstall-hello-mobility-service-on-a-linux-computer"></a>Avinstallera hello mobilitetstjänsten på en Linux-dator
1. Logga in på Linux-servern som en **rot** användare.
2. Gå för/användare/lokal/ASR i en terminal.
3. toouninstall Hej mobilitetstjänsten, kör hello följande kommando:

   ```
   uninstall.sh -Y
   ```

### <a name="step-4-restart-hello-vm"></a>Steg 4: Starta om hello VM

När du har avinstallerat hello mobilitetstjänsten omstart hello VM innan du konfigurera replikering tooanother Azure-region.


## <a name="next-steps"></a>Nästa steg
- Börja skydda dina arbetsbelastningar av [replikering av Azure virtuella datorer](site-recovery-azure-to-azure.md).
- Lär dig mer om [nätverk vägledning för att replikera virtuella datorer i Azure](site-recovery-azure-to-azure-networking-guidance.md).
