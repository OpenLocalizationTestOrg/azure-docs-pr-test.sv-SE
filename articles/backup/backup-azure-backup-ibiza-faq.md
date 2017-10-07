---
title: "aaaRecovery Services valvet vanliga frågor och svar | Microsoft Docs"
description: "Den här versionen av hello vanliga frågor och svar stöder hello offentliga förhandsversionen av hello Azure Backup-tjänsten. Svaren toofrequently frågor och svar om hello backup-agenten, säkerhetskopiering och kvarhållning, återställning, säkerhet och andra vanliga frågor om hello lösning för säkerhetskopiering till Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "lösning för säkerhetskopiering; säkerhetskopieringstjänst"
ms.assetid: 5f55b500-1ee9-4f64-9306-02d6f7a8eded
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/21/2016
ms.author: markgal;trinadhk;
ms.openlocfilehash: 882b2e67ed424dc9f3681a8870e6b4c7b4cdcaec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="recovery-services-vault---faq"></a>Recovery Services-valv – vanliga frågor och svar
Den här artikeln innehåller information specifik tooRecovery Services-valvet och den kompletterar hello [Azure Backup FAQ](backup-azure-backup-faq.md). hello Azure Backup FAQ innehåller hello fullständig uppsättning av frågor och svar om hello Azure Backup-tjänsten.  

Du kan ställa frågor om Azure Backup i hello Disqus-delen av den här artikeln eller en relaterad artikel. Du kan också ställa frågor om hello Azure Backup-tjänsten i hello [diskussionsforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Recovery Services-valv är baserade på resurshanteraren. Stöds säkerhetskopieringsvalv (klassiskt läge) fortfarande? <br/>
Ja, Backup-valv stöds fortfarande. Skapa säkerhetskopieringsvalv i hello [klassisk portal](https://manage.windowsazure.com). Skapa Recovery Services-valv i hello [Azure-portalen](https://portal.azure.com). Men vi rekommenderar starkt du toocreate återställningstjänstvalvet som alla framtida förbättringar kan endast tillgänglig i Recovery Services-valvet.

## <a name="can-i-migrate-a-backup-vault-tooa-recovery-services-vault-br"></a>Kan jag migrera en Backup-valvet tooa Recovery Services-valvet? <br/>
Tyvärr inte, just nu du kan inte migrera hello innehållet i en Backup-valvet tooa Recovery Services-valvet. Vi arbetar för att lägga till den här funktionen, men den är inte tillgänglig som en del av den offentliga förhandsversionen.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Stöder Recovery Services-valv klassiska virtuella datorer eller Resource Manager-baserade virtuella datorer? <br/>
Recovery Services-valv stöder båda modellerna.  Du kan säkerhetskopiera en virtuell dator som skapats i hello klassisk portal (som virtuella datorer i klassiskt läge) eller en virtuell dator skapas i hello Azure-portalen (som är Resource Manager-baserat) tooa Recovery Services-valvet.

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-toomigrate-my-vms-from-classic-mode-tooresource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>Jag har säkerhetskopierat mina klassiska virtuella datorer i säkerhetskopieringsvalvet. Nu vill jag toomigrate min virtuella datorer från klassiskt läge tooResource Manager-läget.  Hur kan jag säkerhetskopiera dem i Recovery Services-valv?
Säkerhetskopior av klassiska virtuella datorer i säkerhetskopieringsvalvet migrera inte automatiskt toorecovery services-valvet när du migrerar hello virtuella datorer från klassiska tooResource Manager-läget. Följ dessa steg för att migrera säkerhetskopior för virtuella datorer:

1. I säkerhetskopieringsvalvet, gå för**skyddade objekt** fliken och markera hello VM. Klicka på [Stoppa skydd](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Lämna alternativet *Ta bort associerade säkerhetskopieringsdata* **avmarkerat**.
2. I hello [Azure-portalen](https://portal.azure.com), gå toohello **tillägg** menyn för hello VM och avinstallera hello **VMSnapshot/VMSnapshotLinux** tillägg.
3. Migrera hello virtuell dator från klassiskt läge tooResource Manager läge. Kontrollera att lagrings- och motsvarande toovirtual datorn finns också migrerade tooResource Manager-läget.
4. Skapa ett recovery services-valv och konfigurera säkerhetskopiering på hello migrerade virtuella datorn med hjälp av **säkerhetskopiering** åtgärd ovanpå valvet instrumentpanelen. Mer information om hur för[Aktivera säkerhetskopiering i recovery services-valvet](backup-azure-vms-first-look-arm.md)
