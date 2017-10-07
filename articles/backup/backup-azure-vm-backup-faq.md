---
title: "aaaAzure VM säkerhetskopiering vanliga frågor och svar | Microsoft Docs"
description: "Svar på toocommon frågor om: hur Virtuella Azure-säkerhetskopiering fungerar, begränsningar och vad som händer när ändringar toopolicy inträffar"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: "säkerhetskopiering av virtuella datorer i azure, återställning av virtuell dator i azure, säkerhetskopieringspolicy"
ms.assetid: c4cd7ff6-8206-45a3-adf5-787f64dbd7e1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/18/2017
ms.author: trinadhk;pullabhk;
ms.openlocfilehash: a1ad2cb3a379577a8c4258c8207ce75809e11a4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="questions-about-hello-azure-vm-backup-service"></a>Frågor om hello Azure VM Backup-tjänsten
Den här artikeln innehåller svar toocommon frågor toohelp du snabbt kan förstå hello Azure VM Backup komponenter. I vissa hello-svar finns länkar toohello artiklar som har omfattande information. Du kan också ställa frågor om hello Azure Backup-tjänsten i hello [diskussionsforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="configure-backup"></a>Konfigurera säkerhetskopiering
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Stöder Recovery Services-valv klassiska virtuella datorer eller Resource Manager-baserade virtuella datorer? <br/>
Recovery Services-valv stöder båda modellerna.  Du kan säkerhetskopiera en klassisk virtuell (som skapats i hello klassisk portal) eller en Resource Manager VM (som skapats i hello Azure-portalen) tooa Recovery Services-valvet.

### <a name="what-configurations-are-not-supported-by-azure-vm-backup-"></a>Vilka konfigurationer stöds inte av vid säkerhetskopiering av virtuella Azure-datorer?
Gå igenom avsnitten [Operativsystem som stöds](backup-azure-arm-vms-prepare.md#supported-operating-system-for-backup) och [Begränsningar för säkerhetskopiering](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm)

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>Varför kan jag inte se min virtuella dator i guiden Konfigurera säkerhetskopiering?
I guiden Konfigurera säkerhetskopiering visar Azure Backup endast virtuella datorer som är:
* Inte redan skyddad - kan du kontrollera status för hello-säkerhetskopiering av en virtuell dator genom att gå tooVM bladet och kontrollera status för säkerhetskopiering från menyn Inställningar av hello-bladet. Mer information om hur för[Kontrollera status för säkerhetskopiering av en virtuell dator](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-management-blade)
* Tillhör toosame region som virtuell dator

## <a name="backup"></a>Säkerhetskopiering
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>Kommer säkerhetskopiering på begäran att följa samma kvarhållningsschema som schemalagda säkerhetskopieringar?
Nej. Du måste toospecify hello kvarhållningsintervallet för en säkerhetskopiering på begäran. Som standard hålls den kvar under 30 dagar när den utlöses från portalen. 

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-toowork"></a>Jag nyligen har aktiverat Azure Disk Encryption på vissa virtuella datorer. Mina säkerhetskopior kommer att fortsätta toowork?
Du behöver toogive behörigheter för Azure Backup service tooaccess Key Vault. Du kan ange dessa behörigheter i PowerShell med hjälp av anvisningarna i avsnittet *Aktivera säkerhetskopiering* i [PowerShell](backup-azure-vms-automation.md)-dokumentationen.

### <a name="i-migrated-disks-of-a-vm-toomanaged-disks-will-my-backups-continue-toowork"></a>Jag migrerade diskar på en VM toomanaged diskar. Mina säkerhetskopior kommer att fortsätta toowork?
Ja, säkerhetskopieringen fungerar sömlöst och inga måste toore-Konfigurera säkerhetskopiering. 

## <a name="restore"></a>Återställ
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>Hur gör jag för att välja mellan diskåterställning och fullständig återställning av en virtuell dator?
Tänk på fullständig återställning av en virtuell Azure-dator som ett sätt att snabbt skapa ett alternativ för den återställda virtuella datorn. Återställa Virtuella alternativet ändras hello namnen på diskar, behållare som används av diskar, offentliga IP-adresser, nätverksgränssnittet namn för resurser som hämtar skapades som en del av skapa en virtuell dator är unika. Även läggs inte hello VM tooavailability uppsättningen. 

Använd återställningsdiskar för att göra följande:
* Anpassa hello VM som skapas från punkten i konfiguration av som ändrar hello storlek från konfigurationen för säkerhetskopiering
* Lägger till konfigurationer som inte är tillgänglig vid hello tid för säkerhetskopiering 
* Kontrollera hello namngivningskonvention för resurser som hämtar skapades
* Lägga till VM tooavailability
* Du har en konfiguration som kan uppnås endast med hjälp av PowerShell/en deklarativ malldefinition

## <a name="manage-vm-backups"></a>Hantera säkerhetskopior av virtuella datorer
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>Vad händer om jag ändrar en säkerhetskopieringspolicy på virtuella datorer?
När en ny princip tillämpas på virtuella datorer, följs schema och lagring av hello ny princip. Om kvarhållning utökas befintliga återställningspunkter markeras tookeep dem enligt ny princip. Om kvarhållning minskar de är markerade för rensning i hello nästa rensningsjobbet och kommer att tas bort. 
