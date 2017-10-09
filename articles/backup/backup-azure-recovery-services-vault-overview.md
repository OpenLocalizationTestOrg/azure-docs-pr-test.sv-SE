---
title: aaaOverview av Recovery Services-valv | Microsoft Docs
description: "En översikt och jämförelse mellan Recovery Services-valv och Azure Backup-valv."
services: backup
documentationcenter: " "
author: markgalioto
manager: carmonm
ms.assetid: 38d4078b-ebc8-41ff-9bc8-47acf256dc80
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/15/2017
ms.author: markgal;arunak
ms.openlocfilehash: 77dd9ece7fe86429cc6f9a47a68b5150a1f4af71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="recovery-services-vaults-overview"></a>Översikt över Recovery Services-valv

Den här artikeln beskriver hello funktionerna i Recovery Services-valvet. Recovery Services-valvet är en enhet för lagring i Azure som innehåller data. hello data är vanligtvis kopior av data eller konfigurationsinformation för virtuella datorer (VM), arbetsbelastningar, servrar eller arbetsstationer. Recovery Services-valvet är hello Resource Manager version av en Backup-valvet. Microsoft rekommenderar att du toouse Recovery Services-valv och tooconvert några säkerhetskopieringsvalv tooRecovery Services-valv.

## <a name="what-is-a-recovery-services-vault"></a>Vad är ett Recovery Services-valv?

Recovery Services-valvet är en onlinelagring entitet i Azure används toohold data, till exempel säkerhetskopior, återställningspunkter och principer för säkerhetskopiering. Du kan använda återställningstjänster valv toohold säkerhetskopierade data för olika Azure-tjänster, till exempel virtuella IaaS-datorer (Linux eller Windows) och Azure SQL-databaser. Recovery Services valv stöd för System Center DPM, Windows Server, Azure Backup Server och mer. Recovery Services-valv gör det enkelt tooorganize dina säkerhetskopierade data och minimerar hanteringskostnader.

Du kan skapa så många Recovery Services-valv som helst inom en Azure-prenumeration.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Jämför Recovery Services-valv och säkerhetskopieringsvalv

Recovery Services-valv baseras på hello Azure Resource Manager-modellen i Azure-säkerhetskopieringsvalv bygger på hello Azure Service Manager-modellen. När du uppgraderar en Backup-valvet tooa Recovery Services-valvet bevaras hello säkerhetskopierade data under och efter hello uppgraderingsprocessen. Recovery Services-valv innehåller funktioner som är inte tillgängligt för säkerhetskopieringsvalv, som:

- **Förbättrad funktioner toohelp säker säkerhetskopieringsdata**: med Recovery Services-valv, Azure Backup ger säkerhet funktioner tooprotect moln säkerhetskopieringar. Dessa funktioner Se till att du kan skydda dina säkerhetskopieringar och på ett säkert sätt återställa data från molnet säkerhetskopior, även om produktions- och backup-servrar som har angripits. [Läs mer](backup-azure-security-feature.md)

- **Central övervakning av din IT-miljö för hybrid**: med Recovery Services-valv kan du övervaka inte bara din [Azure IaaS-VM](backup-azure-manage-vms.md) utan även din [lokala tillgångar](backup-azure-manage-windows-server.md#manage-backup-items) från en central portalen. [Läs mer](http://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Rollbaserad åtkomstkontroll (RBAC)**: RBAC ger detaljerade management åtkomstkontroll i Azure. [Azure tillhandahåller olika inbyggda roller](../active-directory/role-based-access-built-in-roles.md), och Azure Backup har tre [inbyggda roller toomanage återställningspunkter](backup-rbac-rs-vault.md). Recovery Services-valv är kompatibla med RBAC som begränsar säkerhetskopia och återställa åtkomst toohello definierad uppsättning användarroller. [Läs mer](backup-rbac-rs-vault.md)

- **Skydda alla konfigurationer av virtuella datorer i Azure**: Recovery Services-valv skydda Resource Manager-baserade virtuella datorer inklusive Premiumdiskar, hanterade diskar och krypterade virtuella datorer. Uppgradera en Backup-valvet tooa valvet Recovery Services ger du hello möjlighet tooupgrade din Service Manager-baserade virtuella datorer tooResource Manager-baserade virtuella datorer. Du kan behålla dina återställningspunkter för Service Manager-baserade Virtuella och konfigurera skydd för hello uppgraderas (Resource Manager-aktiverat) virtuella datorer under uppgraderingen hello-valvet. [Läs mer](http://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Omedelbar återställning för IaaS-VM**: med Recovery Services-valv, kan du återställa filer och mappar från en IaaS-VM utan att återställa hello hela VM, vilket möjliggör snabbare återställning. Omedelbar återställning för IaaS-VM är tillgänglig för både Windows- och Linux virtuella datorer. [Läs mer](http://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)

## <a name="managing-your-recovery-services-vaults-in-hello-portal"></a>Hantera din Recovery Services-valv i hello-portalen
Skapande och hantering av Recovery Services-valv i hello Azure-portalen är enkelt eftersom hello Backup-tjänsten är integrerad i hello Azure inställningsbladet. Den här integreringen innebär att du kan skapa eller hantera ett Recovery Services-valv *hello gäller hello Måltjänsten*. Till exempel tooview hello Återställningspunkter för en virtuell dator, markerar du den och klicka på **säkerhetskopiering** hello inställningar-bladet. hello säkerhetskopierad information specifik toothat VM visas. I följande exempel hello **ContosoVM** är hello hello virtuella datorns namn. **ContosoVM demovault** är hello namnet på hello Recovery Services-valvet. Du behöver inte tooremember hello namnet på hello Recovery Services-valv som lagrar hello Återställningspunkter, du kan komma åt den här informationen från hello virtuell dator.  

![Recovery services-valvet information VM](./media/backup-azure-recovery-services-vault-overview/rs-vault-in-context.png)

Om flera servrar är skyddade med hello samma Recovery Services-valvet, kan det vara flera logiska toolook på hello Recovery Services-valvet. Du kan söka efter alla Recovery Services-valv i hello prenumerationen och väljer en från hello lista.

hello innehåller följande avsnitt länkar tooarticles som förklarar hur toouse en återställningstjänster valvet i varje typ av aktivitet.

### <a name="back-up-data"></a>Säkerhetskopiera data
- [Säkerhetskopiera en Azure VM](backup-azure-vms-first-look-arm.md)
- [Säkerhetskopiera Windows Server eller Windows-arbetsstation](backup-try-azure-backup-in-10-mins.md)
- [Säkerhetskopiera DPM-arbetsbelastningar tooAzure](backup-azure-dpm-introduction.md)
- [Förbereda tooback in arbetsbelastningar med Azure Backup Server](backup-azure-microsoft-azure-backup.md)

### <a name="manage-recovery-points"></a>Hantera återställningspunkter
- [Hantera Virtuella Azure-säkerhetskopiering](backup-azure-manage-vms.md)
- [Hantera filer och mappar](backup-azure-manage-windows-server.md)

### <a name="restore-data-from-hello-vault"></a>Återställa data från hello-valvet
- [Återställa enskilda filer från en Azure VM](backup-azure-restore-files-from-vm.md)
- [Återställa en virtuell dator i Azure](backup-azure-arm-restore-vms.md)

### <a name="secure-hello-vault"></a>Säker hello valvet
- [Skydda säkerhetskopierade data från molnet i Recovery Services-valv](backup-azure-security-feature.md)



## <a name="next-steps"></a>Nästa steg
Använd följande artikel för hello:</br>
[Säkerhetskopiera en IaaS VM](backup-azure-arm-vms-prepare.md)</br>
[Säkerhetskopiera en Azure Backup-Server](backup-azure-microsoft-azure-backup.md)</br>
[Säkerhetskopiera Windows Server](backup-configure-vault.md)
