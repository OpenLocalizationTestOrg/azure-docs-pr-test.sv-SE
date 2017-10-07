---
title: "aaaLog Analytics datamodellen för Azure Backup"
description: "Den här artikeln handlar om logganalys data model detaljer för Azure Backup-data."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: dfd5c73d-0d34-4d48-959e-1936986f9fc0
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/24/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 04ac16e38b896851f60b1c4ffbea4343347ae32c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Log Analytics-datamodell för Azure Backup-data
Den här artikeln beskriver hello-datamodell som används för att överföra reporting data tooLog Analytics. Med den här datamodellen kan du skapa egna frågor, instrumentpaneler, och använda den i OMS. 

## <a name="using-azure-backup-data-model"></a>Med hjälp av Azure Backup-datamodell
Du kan använda hello följande fält som har angetts som en del av hello data model toocreate visuell information, anpassade frågor och instrumentpanel enligt dina krav.

### <a name="alert"></a>Varning
Den här tabellen innehåller information om relaterade aviseringsfält.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| AlertUniqueId_s |Text |Unikt Id för hello genereras aviseringen |
| AlertType_s |Text |Typ av hello genereras aviseringen, till exempel säkerhetskopiering |
| AlertStatus_s |Text |Status för hello aviseringen, till exempel Active |
| AlertOccurenceDateTime_s |Datum/tid |Datum och tid då aviseringen skapades |
| AlertSeverity_s |Text |Aviseringens allvarlighetsgrad hello, till exempel kritisk |
| EventName_s |Text |Det här fältet motsvarar namnet på den här händelsen, är det alltid AzureBackupCentralReport |
| BackupItemUniqueId_s |Text |Unikt Id för hello Säkerhetskopiera objekt toowhich aviseringen tillhör för|
| SchemaVersion_s |Text |Det här fältet anger nuvarande version av hello schemat, är det **V1** |
| State_s |Text |Aktuell status för hello avisering objekt, till exempel Active, tagits bort |
| BackupManagementType_s |Text |Providertyp för att utföra säkerhetskopieringen, till exempel IaaSVM FileFolder toowhich aviseringen tillhör för|
| OperationName |Text |Det här fältet motsvarar namnet på hello den aktuella åtgärden - avisering |
| Kategori |Text |Det här fältet motsvarar datakategori diagnostik pushas tooLog Analytics, är det AzureBackupReport |
| Resurs |Text |Detta är hello resurs som data samlas, den visar namn på Recovery Services-valvet |
| ProtectedServerUniqueId_s |Text |Unikt Id för hello skyddade toowhich som tillhör den här aviseringen för|
| VaultUniqueId_s |Text |Unikt Id för hello skyddade toowhich som tillhör den här aviseringen för|
| SourceSystem |Text |Källsystemet hello aktuella data – Azure |
| Resurs-ID |Text |Det här fältet representerar resurs-id som data samlas, den visar Recovery Services-valvet resurs-id |
| SubscriptionId |Text |Det här fältet motsvarar prenumerations-id för hello-resurs (RS valvet) som data samlas |
| ResourceGroup |Text |Det här fältet motsvarar resursgruppen hello-resurs (RS valvet) som data samlas |
| ResourceProvider |Text |Det här fältet motsvarar hello resursprovidern som data samlas - Microsoft.RecoveryServices |
| ResourceType |Text |Det här fältet motsvarar hello resurstyp som data samlas - valv |

### <a name="backupitem"></a>BackupItem
Den här tabellen innehåller information om säkerhetskopiering fält som är relaterade objekt.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| EventName_s |Text |Det här fältet motsvarar namnet på den här händelsen, är det alltid AzureBackupCentralReport |  
| BackupItemUniqueId_s |Text |Unikt Id för hello Säkerhetskopiera objekt |
| BackupItemId_s |Text |Säkerhetskopiera objekt-ID |
| BackupItemName_s |Text |Namnet på Säkerhetskopiera objekt |
| BackupItemFriendlyName_s |Text |Eget namn för Säkerhetskopiera objekt |
| BackupItemType_s |Text |Typ av säkerhetskopiering objekt, till exempel Virtuella FileFolder |
| ProtectedServerName_s |Text |Namnet på den skyddade servern toowhich Säkerhetskopiera objekt tillhör för|
| ProtectionState_s |Text |Skydd med dagens hello Säkerhetskopiera objekt, till exempel skyddade, ProtectionStopped |
| SchemaVersion_s |Text |Det här fältet anger nuvarande version av hello schemat, är det **V1** |
| State_s |Text |Aktuell status för hello säkerhetskopieobjekt objekt, till exempel Active, tagits bort |
| BackupManagementType_s |Text |Providertyp för att utföra säkerhetskopieringen, till exempel IaaSVM FileFolder toowhich säkerhetskopiering på artikeln för|
| OperationName |Text |Det här fältet motsvarar namnet på hello den aktuella åtgärden - BackupItem |
| Kategori |Text |Det här fältet motsvarar datakategori diagnostik pushas tooLog Analytics, är det AzureBackupReport |
| Resurs |Text |Detta är hello resurs som data samlas, den visar namn på Recovery Services-valvet |
| SourceSystem |Text |Källsystemet hello aktuella data – Azure |
| Resurs-ID |Text |Det här fältet representerar resurs-id som data samlas, den visar Recovery Services-valvet resurs-id |
| SubscriptionId |Text |Det här fältet motsvarar prenumerations-id för hello-resurs (RS valvet) som data samlas |
| ResourceGroup |Text |Det här fältet motsvarar resursgruppen hello-resurs (RS valvet) som data samlas |
| ResourceProvider |Text |Det här fältet motsvarar hello resursprovidern som data samlas - Microsoft.RecoveryServices |
| ResourceType |Text |Det här fältet motsvarar hello resurstyp som data samlas - valv |

### <a name="backupitemassociation"></a>BackupItemAssociation
Den här tabellen innehåller information om säkerhetskopiering objektet associationer med olika enheter.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| EventName_s |Text |Det här fältet motsvarar namnet på den här händelsen, är det alltid AzureBackupCentralReport |  
| BackupItemUniqueId_s |Text |Unikt Id för hello Säkerhetskopiera objekt |
| SchemaVersion_s |Text |Det här fältet anger nuvarande version av hello schemat, är det **V1** |
| State_s |Text |Aktuell status för hello Säkerhetskopiera objekt kopplingsobjektet, till exempel Active, tagits bort |
| BackupManagementType_s |Text |Providertyp för att utföra säkerhetskopieringen, till exempel IaaSVM FileFolder toowhich säkerhetskopiering på artikeln för|
| OperationName |Text |Det här fältet motsvarar namnet på hello den aktuella åtgärden - BackupItemAssociation |
| Kategori |Text |Det här fältet motsvarar datakategori diagnostik pushas tooLog Analytics, är det AzureBackupReport |
| Resurs |Text |Detta är hello resurs som data samlas, den visar namn på Recovery Services-valvet |
| PolicyUniqueId_g |Text |Unikt Id tooidentify hello principen kan säkerhetskopiera objekt som är associerade för|
| ProtectedServerUniqueId_s |Text |Unikt Id för hello skyddad server toowhich säkerhetskopiering på artikeln för|
| VaultUniqueId_s |Text |Unikt Id för hello valvet toowhich säkerhetskopiering på artikeln för|
| SourceSystem |Text |Källsystemet hello aktuella data – Azure |
| Resurs-ID |Text |Det här fältet representerar resurs-id som data samlas, den visar Recovery Services-valvet resurs-id |
| SubscriptionId |Text |Det här fältet motsvarar prenumerations-id för hello-resurs (RS valvet) som data samlas |
| ResourceGroup |Text |Det här fältet motsvarar resursgruppen hello-resurs (RS valvet) som data samlas |
| ResourceProvider |Text |Det här fältet motsvarar hello resursprovidern som data samlas - Microsoft.RecoveryServices |
| ResourceType |Text |Det här fältet motsvarar hello resurstyp som data samlas - valv |

### <a name="job"></a>Jobb
Den här tabellen innehåller information om projektspecifika fält.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| EventName_s |Text |Det här fältet motsvarar namnet på den här händelsen, är det alltid AzureBackupCentralReport |
| BackupItemUniqueId_s |Text |Unikt Id för hello Säkerhetskopiera objekt toowhich jobbet tillhör för|
| SchemaVersion_s |Text |Det här fältet anger nuvarande version av hello schemat, är det **V1** |
| State_s |Text |Aktuell status för hello jobbobjektet, till exempel Active, tagits bort |
| BackupManagementType_s |Text |Providertyp för att utföra säkerhetskopieringen, till exempel IaaSVM FileFolder toowhich jobbet tillhör för|
| OperationName |Text |Det här fältet motsvarar namnet på den aktuella åtgärden för hello - jobb |
| Kategori |Text |Det här fältet motsvarar datakategori diagnostik pushas tooLog Analytics, är det AzureBackupReport |
| Resurs |Text |Detta är hello resurs som data samlas, den visar namn på Recovery Services-valvet |
| ProtectedServerUniqueId_s |Text |Unikt Id för hello skyddade toowhich jobbet tillhör för|
| VaultUniqueId_s |Text |Unikt Id för hello skyddade toowhich jobbet tillhör för|
| JobOperation_s |Text |Åtgärden som jobbet körs exempelvis säkerhetskopiering, återställning, konfigurera säkerhetskopiering |
| JobStatus_s |Text |Status för hello klar jobb, till exempel slutförd, misslyckades |
| JobFailureCode_s |Text |Felkod sträng på grund av som skett jobbet misslyckades |
| JobStartDateTime_s |Datum/tid |Datum och tid då jobbet startade körs |
| BackupStorageDestination_s |Text |Mål för lagring för säkerhetskopiering, till exempel moln, Disk  |
| JobDurationInSecs_s | Tal |Totalt antal jobb varaktighet i sekunder |
| DataTransferredInMB_s | Tal |Data som överförs i MB för det här jobbet|
| JobUniqueId_g |Text |Unikt Id tooidentify hello jobb |
| SourceSystem |Text |Källsystemet hello aktuella data – Azure |
| Resurs-ID |Text |Det här fältet representerar resurs-id som data samlas, den visar Recovery Services-valvet resurs-id |
| SubscriptionId |Text |Det här fältet motsvarar prenumerations-id för hello-resurs (RS valvet) som data samlas |
| ResourceGroup |Text |Det här fältet motsvarar resursgruppen hello-resurs (RS valvet) som data samlas |
| ResourceProvider |Text |Det här fältet motsvarar hello resursprovidern som data samlas - Microsoft.RecoveryServices |
| ResourceType |Text |Det här fältet motsvarar hello resurstyp som data samlas - valv |

### <a name="policy"></a>Princip
Den här tabellen innehåller information om principen-relaterade fält.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| EventName_s |Text |Det här fältet motsvarar namnet på den här händelsen, är det alltid AzureBackupCentralReport |
| SchemaVersion_s |Text |Det här fältet anger nuvarande version av hello schemat, är det **V1** |
| State_s |Text |Aktuell status för hello principobjektet, till exempel Active, tagits bort |
| BackupManagementType_s |Text |Providertyp för att utföra säkerhetskopieringen, till exempel IaaSVM FileFolder toowhich som tillhör den här principen för|
| OperationName |Text |Det här fältet motsvarar namnet på hello den aktuella åtgärden - princip |
| Kategori |Text |Det här fältet motsvarar datakategori diagnostik pushas tooLog Analytics, är det AzureBackupReport |
| Resurs |Text |Detta är hello resurs som data samlas, den visar namn på Recovery Services-valvet |
| PolicyUniqueId_g |Text |Unikt Id tooidentify hello princip |
| PolicyName_s |Text |Namnet på hello princip har definierats |
| BackupFrequency_s |Text |Frekvens som säkerhetskopieringar körs, till exempel, varje dag, varje vecka |
| BackupTimes_s |Text |Datum och tid när säkerhetskopieringar är schemalagda |
| BackupDaysOfTheWeek_s |Text |Veckodagar hello när schemalagda säkerhetskopieringar |
| RetentionDuration_s |Heltal |Kvarhållning varaktighet för konfigurerade säkerhetskopieringar |
| DailyRetentionDuration_s |Heltal |Totalt bevarande varaktighet i dagar för konfigurerade säkerhetskopieringar |
| DailyRetentionTimes_s |Text |Datum och tid när dagliga kvarhållning konfigurerades |
| WeeklyRetentionDuration_s |Decimaltal |Total varaktighet för varje vecka kvarhållning i veckor för konfigurerade säkerhetskopieringar |
| WeeklyRetentionTimes_s |Text |Datum och tid när varje vecka kvarhållning har konfigurerats |
| WeeklyRetentionDaysOfTheWeek_s |Text |Hello veckodagar som valts för kvarhållning av veckovis |
| MonthlyRetentionDuration_s |Decimaltal |Totalt bevarande varaktighet i månader för konfigurerade säkerhetskopieringar |
| MonthlyRetentionTimes_s |Text |Datum och tid när månatliga kvarhållning har konfigurerats |
| MonthlyRetentionFormat_s |Text |Typ av konfigurationen för månatliga kvarhållning, till exempel varje dag för dag, varje vecka baserat på vecka baserat |
| MonthlyRetentionDaysOfTheWeek_s |Text |Hello veckodagar som valts för månatliga kvarhållning |
| MonthlyRetentionWeeksOfTheMonth_s |Text |Veckor hello månaden när månatliga kvarhållning har konfigurerats, till exempel första, sista osv. |
| YearlyRetentionDuration_s |Decimaltal |Totalt bevarande varaktighet i år för konfigurerade säkerhetskopieringar |
| YearlyRetentionTimes_s |Text |Datum och tid när årlig kvarhållning har konfigurerats |
| YearlyRetentionMonthsOfTheYear_s |Text |Hello årets månader som har valts för kvarhållning av årlig |
| YearlyRetentionFormat_s |Text |Typ av konfigurationen för årlig kvarhållning, till exempel varje dag för dag, varje vecka baserat på vecka baserat |
| YearlyRetentionDaysOfTheMonth_s |Text |Datum för hello månad som årlig kvarhållning |
| SourceSystem |Text |Källsystemet hello aktuella data – Azure |
| Resurs-ID |Text |Det här fältet representerar resurs-id som data samlas, den visar Recovery Services-valvet resurs-id |
| SubscriptionId |Text |Det här fältet motsvarar prenumerations-id för hello-resurs (RS valvet) som data samlas |
| ResourceGroup |Text |Det här fältet motsvarar resursgruppen hello-resurs (RS valvet) som data samlas |
| ResourceProvider |Text |Det här fältet motsvarar hello resursprovidern som data samlas - Microsoft.RecoveryServices |
| ResourceType |Text |Det här fältet motsvarar hello resurstyp som data samlas - valv |

### <a name="policyassociation"></a>PolicyAssociation
Den här tabellen innehåller information om principkopplingar med olika enheter.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| EventName_s |Text |Det här fältet motsvarar namnet på den här händelsen, är det alltid AzureBackupCentralReport |
| SchemaVersion_s |Text |Det här fältet anger nuvarande version av hello schemat, är det **V1** |
| State_s |Text |Aktuell status för hello principobjektet, till exempel Active, tagits bort |
| BackupManagementType_s |Text |Providertyp för att utföra säkerhetskopieringen exempelvis IaaSVM FileFolder toowhich som tillhör den här principen för|
| OperationName |Text |Det här fältet motsvarar namnet på hello den aktuella åtgärden - PolicyAssociation |
| Kategori |Text |Det här fältet motsvarar datakategori diagnostik pushas tooLog Analytics, är det AzureBackupReport |
| Resurs |Text |Detta är hello resurs som data samlas, den visar namn på Recovery Services-valvet |
| PolicyUniqueId_g |Text |Unikt Id tooidentify hello princip |
| VaultUniqueId_s |Text |Unikt Id för hello valvet toowhich principen tillhör för|
| SourceSystem |Text |Källsystemet hello aktuella data – Azure |
| Resurs-ID |Text |Det här fältet representerar resurs-id som data samlas, den visar Recovery Services-valvet resurs-id |
| SubscriptionId |Text |Det här fältet motsvarar prenumerations-id för hello-resurs (RS valvet) som data samlas |
| ResourceGroup |Text |Det här fältet motsvarar resursgruppen hello-resurs (RS valvet) som data samlas |
| ResourceProvider |Text |Det här fältet motsvarar hello resursprovidern som data samlas - Microsoft.RecoveryServices |
| ResourceType |Text |Det här fältet motsvarar hello resurstyp som data samlas - valv |

### <a name="protectedserver"></a>ProtectedServer
Den här tabellen innehåller information om skyddade server-relaterade fält.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| EventName_s |Text |Det här fältet motsvarar namnet på den här händelsen, är det alltid AzureBackupCentralReport |
| ProtectedServerName_s |Text |Namnet på skyddad server |
| SchemaVersion_s |Text |Det här fältet anger nuvarande version av hello schemat, är det **V1** |
| State_s |Text |Aktuell status för hello skyddad server-objekt, till exempel Active, tagits bort |
| BackupManagementType_s |Text |Providertyp för att utföra säkerhetskopieringen exempelvis IaaSVM FileFolder toowhich den här skyddade servern tillhör för|
| OperationName |Text |Det här fältet motsvarar namnet på hello den aktuella åtgärden - ProtectedServer |
| Kategori |Text |Det här fältet motsvarar datakategori diagnostik pushas tooLog Analytics, är det AzureBackupReport |
| Resurs |Text |Detta är hello resurs som data samlas, den visar namn på Recovery Services-valvet |
| ProtectedServerUniqueId_s |Text |Unikt Id för hello skyddad server |
| RegisteredContainerId_s |Text |ID: t för behållaren som registrerats för säkerhetskopiering |
| ProtectedServerType_s |Text |Typ av skyddade server säkerhetskopieras till exempel Windows |
| ProtectedServerFriendlyName_s |Text |Eget namn på skyddad server |
| AzureBackupAgentVersion_s |Text |Versionsnumret för agenten säkerhetskopiering Version |
| SourceSystem |Text |Källsystemet hello aktuella data – Azure |
| Resurs-ID |Text |Det här fältet representerar resurs-id som data samlas, den visar Recovery Services-valvet resurs-id |
| SubscriptionId |Text |Det här fältet motsvarar prenumerations-id för hello-resurs (RS valvet) som data samlas |
| ResourceGroup |Text |Det här fältet motsvarar resursgruppen hello-resurs (RS valvet) som data samlas |
| ResourceProvider |Text |Det här fältet motsvarar hello resursprovidern som data samlas - Microsoft.RecoveryServices |
| ResourceType |Text |Det här fältet motsvarar hello resurstyp som data samlas - valv |

### <a name="protectedserverassociation"></a>ProtectedServerAssociation
Den här tabellen innehåller information om skyddade servern associationer till andra enheter.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| EventName_s |Text |Det här fältet motsvarar namnet på den här händelsen, är det alltid AzureBackupCentralReport |
| SchemaVersion_s |Text |Det här fältet anger nuvarande version av hello schemat, är det **V1** |
| State_s |Text |Aktuell status för hello skyddad server kopplingsobjektet, till exempel Active, tagits bort |
| BackupManagementType_s |Text |Providertyp för att utföra säkerhetskopieringen, till exempel IaaSVM FileFolder toowhich den här skyddade servern tillhör för|
| OperationName |Text |Det här fältet motsvarar namnet på hello den aktuella åtgärden - ProtectedServerAssociation |
| Kategori |Text |Det här fältet motsvarar datakategori diagnostik pushas tooLog Analytics, är det AzureBackupReport |
| Resurs |Text |Detta är hello resurs som data samlas, den visar namn på Recovery Services-valvet |
| ProtectedServerUniqueId_s |Text |Unikt Id för hello skyddad server |
| VaultUniqueId_s |Text |Unikt Id för hello valvet toowhich den här skyddade servern tillhör för|
| SourceSystem |Text |Källsystemet hello aktuella data – Azure |
| Resurs-ID |Text |Det här fältet representerar resurs-id som data samlas, den visar Recovery Services-valvet resurs-id |
| SubscriptionId |Text |Det här fältet motsvarar prenumerations-id för hello-resurs (RS valvet) som data samlas |
| ResourceGroup |Text |Det här fältet motsvarar resursgruppen hello-resurs (RS valvet) som data samlas |
| ResourceProvider |Text |Det här fältet motsvarar hello resursprovidern som data samlas - Microsoft.RecoveryServices |
| ResourceType |Text |Det här fältet motsvarar hello resurstyp som data samlas - valv |

### <a name="storage"></a>Lagring
Den här tabellen innehåller information om lagringsrelaterade fält.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| CloudStorageInBytes_s |Decimaltal |Säkerhetskopiering molnlagring som används av säkerhetskopior, beräknas baserat på senaste värde |
| ProtectedInstances_s |Decimaltal |Antalet skyddade instanser som används för att beräkna klientdel lagring i fakturering, beräknad baserat på senaste värdet |
| EventName_s |Text |Det här fältet motsvarar namnet på den här händelsen, är det alltid AzureBackupCentralReport |
| SchemaVersion_s |Text |Det här fältet anger nuvarande version av hello schemat, är det **V1** |
| State_s |Text |Aktuell status för hello lagringsobjekt, till exempel Active, tagits bort |
| BackupManagementType_s |Text |Providertyp för att utföra säkerhetskopieringen, till exempel IaaSVM FileFolder toowhich lagringen för tillhör|
| OperationName |Text |Det här fältet motsvarar namnet på hello den aktuella åtgärden - lagring |
| Kategori |Text |Det här fältet motsvarar datakategori diagnostik pushas tooLog Analytics, är det AzureBackupReport |
| Resurs |Text |Detta är hello resurs som data samlas, den visar namn på Recovery Services-valvet |
| ProtectedServerUniqueId_s |Text |Unikt Id för hello skyddad server för vilket lagring beräknas |
| VaultUniqueId_s |Text |Unikt Id för hello vault för lagring beräknas |
| SourceSystem |Text |Källsystemet hello aktuella data – Azure |
| Resurs-ID |Text |Det här fältet representerar resurs-id som data samlas, den visar Recovery Services-valvet resurs-id |
| SubscriptionId |Text |Det här fältet motsvarar prenumerations-id för hello-resurs (RS valvet) som data samlas |
| ResourceGroup |Text |Det här fältet motsvarar resursgruppen hello-resurs (RS valvet) som data samlas |
| ResourceProvider |Text |Det här fältet motsvarar hello resursprovidern som data samlas - Microsoft.RecoveryServices |
| ResourceType |Text |Det här fältet representse typ av hello resurs som data samlas - valv |

### <a name="vault"></a>Valv
Den här tabellen innehåller information om valvet-relaterade fält.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| EventName_s |Text |Det här fältet motsvarar namnet på den här händelsen, är det alltid AzureBackupCentralReport |
| SchemaVersion_s |Text |Det här fältet anger nuvarande version av hello schemat, är det **V1** |
| State_s |Text |Aktuell status för hello valvet objekt, till exempel Active, tagits bort |
| OperationName |Text |Det här fältet motsvarar namnet på hello den aktuella åtgärden - valvet |
| Kategori |Text |Det här fältet motsvarar datakategori diagnostik pushas tooLog Analytics, är det AzureBackupReport |
| Resurs |Text |Detta är hello resurs som data samlas, den visar namn på Recovery Services-valvet |
| VaultUniqueId_s |Text |Unikt Id för hello valvet |
| VaultName_s |Text |Namnet på hello valvet |
| AzureDataCenter_s |Text |Datacenter där valvet finns |
| StorageReplicationType_s |Text |Typ av hello valvet, till exempel GeoRedundant storage-replikering |
| SourceSystem |Text |Källsystemet hello aktuella data – Azure |
| Resurs-ID |Text |Det här fältet representerar resurs-id som data samlas, den visar Recovery Services-valvet resurs-id |
| SubscriptionId |Text |Det här fältet motsvarar prenumerations-id för hello-resurs (RS valvet) som data samlas |
| ResourceGroup |Text |Det här fältet motsvarar resursgruppen hello-resurs (RS valvet) som data samlas |
| ResourceProvider |Text |Det här fältet motsvarar hello resursprovidern som data samlas - Microsoft.RecoveryServices |
| ResourceType |Text |Det här fältet motsvarar hello resurstyp som data samlas - valv |

## <a name="next-steps"></a>Nästa steg
När du granskar hello-datamodell för att skapa rapporter för Azure Backup kan du börja [skapar instrumentpanelen](../log-analytics/log-analytics-dashboards.md) i logganalys och OMS.
