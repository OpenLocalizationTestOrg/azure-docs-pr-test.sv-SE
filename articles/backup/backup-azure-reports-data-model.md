---
title: "aaaData modell för Azure Backup"
description: "Den här artikeln handlar om Power BI data modellinformation för Azure Backup-rapporter."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 0767c330-690d-474d-85a6-aa8ddc410bb2
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/26/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5e3e7ca13c7a3f007c206bd56b8753166a2c264b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-model-for-azure-backup-reports"></a>Datamodell för Azure Backup-rapporter
Den här artikeln beskriver hello Power BI-datamodell som används för att skapa rapporter för Azure Backup. Med den här datamodellen kan du filtrera befintliga rapporter baserat på relevanta fält och mer skapa allt egna rapporter med hjälp av tabeller och fält i hello modellen. 

## <a name="creating-new-reports-in-power-bi"></a>Skapa nya rapporter i Power BI
Powerbi ger anpassningsfunktionerna med hjälp av som du kan [skapa rapporter med hello datamodellen](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/).

## <a name="using-azure-backup-data-model"></a>Med hjälp av Azure Backup-datamodell
Du kan använda hello följande fält som har angetts som en del av hello data modellen toocreate rapporter och anpassa befintliga rapporter.

### <a name="alert"></a>Varning
Den här tabellen innehåller grundläggande fält och aggregeringar över olika aviseringar relaterade fält.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| #AlertsCreatedInPeriod |Heltal |Antalet aviseringar som skapats i den valda tidsperioden |
| % ActiveAlertsCreatedInPeriod |Procent |Procentandelen aktiva aviseringar under vald tidsperiod |
| % CriticalAlertsCreatedInPeriod |Procent |Andelen kritiska aviseringar tidsperioden |
| AlertOccurenceDate |Date |Datum då aviseringen skapades |
| AlertSeverity |Text |Aviseringens hello exempelvis kritisk allvarlighetsgrad |
| AlertStatus |Text |Status för hello avisering exempelvis Active |
| AlertType |Text |Typen av avisering hello genereras till exempel säkerhetskopiering |
| AlertUniqueId |Text |Unikt Id för hello genereras aviseringen |
| AsOnDateTime |Datum/tid |Senaste Uppdateringstid för hello valda rad |
| AvgResolutionTimeInMinsForAlertsCreatedInPeriod |Decimaltal |Genomsnittlig tid (i minuter) tooresolve avisering för vald tidsperiod |
| EntityState |Text |Aktuell status för hello avisering objektet exempelvis aktiv, tagits bort |

### <a name="backup-item"></a>Säkerhetskopiera objekt
Den här tabellen innehåller grundläggande fält och aggregeringar över olika säkerhetskopiering objektet-relaterade fält.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| #BackupItems |Heltal |Antal objekt som säkerhetskopiering |
| #UnprotectedBackupItems |Heltal |Antal Säkerhetskopiera objekt har stoppats för skydd eller har konfigurerats för säkerhetskopiering men säkerhetskopieringar som inte har startats|
| AsOnDateTime |Datum/tid |Senaste Uppdateringstid för hello valda rad |
| BackupItemFriendlyName |Text |Eget namn för Säkerhetskopiera objekt |
| BackupItemId |Text |Säkerhetskopiera objekt-ID |
| BackupItemName |Text |Namnet på Säkerhetskopiera objekt |
| BackupItemType |Text |Typ av säkerhetskopiering objekt till exempel Virtuella FileFolder |
| EntityState |Text |Aktuell status för hello Säkerhetskopiera objekt objektet exempelvis aktiv, tagits bort |
| LastBackupDateTime |Datum/tid |Tid för senaste säkerhetskopiering för det valda objektet för säkerhetskopiering |
| LastBackupState |Text |Tillståndet för den senaste säkerhetskopieringen för det valda säkerhetskopiering objektet exempelvis lyckades, misslyckades |
| LastSuccessfulBackupDateTime |Datum/tid |Tid för senaste lyckade säkerhetskopiering för det valda objektet för säkerhetskopiering |
| ProtectionState |Text |Skydd med dagens hello Säkerhetskopiera objekt till exempel skyddade, ProtectionStopped |

### <a name="calendar"></a>Kalender
Den här tabellen innehåller information om kalender-relaterade fält.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| Date |Date |Datum som valts för att filtrera data |
| DateKey |Text |Unik nyckel för varje datum-objekt |
| DayDiff |Decimaltal |Skillnad i dag för att filtrera data, till exempel, 0 anger data aktuell dag, -1 anger föregående en dags data, 0 och -1 anger data för aktuella och föregående dag  |
| Månad |Text |Månad hello år som valts för att filtrera data, månad börjar på första dag och slutar på 31 dagar |
| MonthDate | Date |Datum hello månad då månad slutar valts för att filtrera data |
| MonthDiff |Decimaltal |Skillnad i månad för att filtrera data, till exempel, 0 anger aktuella månadens uppgifter, -1 anger föregående månad data, 0 och -1 anger data för aktuella och föregående månad |
| Vecka |Text |Veckan har valts för att filtrera data, veckan börjar med söndag och slutar på lördag |
| WeekDate |Date |Datum i hello veckan när vecka avslutas valts för att filtrera data |
| WeekDiff |Decimaltal |Skillnad i veckan för filtrera data, till exempel, 0 anger aktuella veckan data, -1 anger data för föregående vecka, 0 och -1 anger data för aktuella och föregående vecka |
| År |Text |Kalenderåret som valts för att filtrera data |
| YearDate |Date |Datum i hello år när året slutar valts för att filtrera data |

### <a name="job"></a>Jobb
Den här tabellen innehåller grundläggande fält och aggregeringar över olika projektspecifika fält.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| #JobsCreatedInPeriod |Heltal |Antalet jobb som skapats i hello tidsperiod |
| % FailuresForJobsCreatedInPeriod |Procent |Procentandel övergripande jobbet fel i hello tidsperiod |
| 80thPercentileDataTransferredInMBForBackupJobsCreatedInPeriod |Decimaltal |80 percentilvärdet för data som överförs i MB för **säkerhetskopiering** jobb som skapats i hello tidsperiod |
| AsOnDateTime |Datum/tid |Senaste Uppdateringstid för hello valda rad |
| AvgBackupDurationInMinsForJobsCreatedInPeriod |Decimaltal |Genomsnittlig tid i minuter för **slutförda säkerhetskopiering** jobb som skapats i den valda tidsperioden |
| AvgRestoreDurationInMinsForJobsCreatedInPeriod |Decimaltal |Genomsnittlig tid i minuter för **slutförd återställning** jobb som skapats i den valda tidsperioden |
| BackupStorageDestination |Text |Mål för säkerhetskopieringslagring exempelvis molnet Disk  |
| EntityState |Text |Aktuell status för hello jobbobjektet exempelvis aktiv, tagits bort |
| JobFailureCode |Text |Felkod sträng på grund av som skett jobbet misslyckades |
| JobOperation |Text |Åtgärden som jobbet körs exempelvis säkerhetskopiering, återställning, konfigurera säkerhetskopiering |
| JobStartDate |Date |Datum när jobbet har startats |
| JobStartTime |Tid |När jobbet har startats |
| JobStatus |Text |Status för hello klar jobb exempelvis slutfört, misslyckades |
| JobUniqueId |Text |Unikt Id tooidentify hello jobb |

### <a name="policy"></a>Princip
Den här tabellen innehåller grundläggande fält och aggregeringar över olika principrelaterade fält.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| #Policies |Heltal |Antal principer för säkerhetskopiering som finns i hello system |
| #PoliciesInUse |Heltal |Antal principer som för närvarande används för att konfigurera säkerhetskopiering |
| AsOnDateTime |Datum/tid |Senaste Uppdateringstid för hello valda rad |
| BackupDaysOfTheWeek |Text |Veckodagar hello när schemalagda säkerhetskopieringar |
| BackupFrequency |Text |Frekvens som säkerhetskopieringar körs exempelvis, varje dag, varje vecka |
| BackupTimes |Text |Datum och tid när säkerhetskopieringar är schemalagda |
| DailyRetentionDuration |Heltal |Totalt bevarande varaktighet i dagar för konfigurerade säkerhetskopieringar |
| DailyRetentionTimes |Text |Datum och tid när dagliga kvarhållning konfigurerades |
| EntityState |Text |Aktuell status för hello principobjektet exempelvis aktiv, tagits bort |
| MonthlyRetentionDaysOfTheMonth |Text |Datum för hello månad som månatliga kvarhållning |
| MonthlyRetentionDaysOfTheWeek |Text |Hello veckodagar som valts för månatliga kvarhållning |
| MonthlyRetentionDuration |Decimaltal |Totalt bevarande varaktighet i månader för konfigurerade säkerhetskopieringar |
| MonthlyRetentionFormat |Text |Typ av konfigurationen för kvarhållning av månatlig exempelvis varje dag för dag, varje vecka baserat på vecka baserat |
| MonthlyRetentionTimes |Text |Datum och tid när månatliga kvarhållning har konfigurerats |
| MonthlyRetentionWeeksOfTheMonth |Text |Veckor hello månad när månatliga kvarhållning är konfigurerad till exempel första, sista osv. |
| Principnamn |Text |Namnet på hello princip har definierats |
| PolicyUniqueId |Text |Unikt Id tooidentify hello princip |
| RetentionType |Text |Typ av bevarandeprincip till exempel, varje dag, vecka, månad, varje år |
| WeeklyRetentionDaysOfTheWeek |Text |Hello veckodagar som valts för kvarhållning av veckovis |
| WeeklyRetentionDuration |Decimaltal |Total varaktighet för varje vecka kvarhållning i veckor för konfigurerade säkerhetskopieringar |
| WeeklyRetentionTimes |Text |Datum och tid när varje vecka kvarhållning har konfigurerats |
| YearlyRetentionDaysOfTheMonth |Text |Datum för hello månad som årlig kvarhållning |
| YearlyRetentionDaysOfTheWeek |Text |Hello veckodagar som valts för kvarhållning av årlig |
| YearlyRetentionDuration |Decimaltal |Totalt bevarande varaktighet i år för konfigurerade säkerhetskopieringar |
| YearlyRetentionFormat |Text |Typ av konfigurationen för kvarhållning av årlig exempelvis varje dag för dag, varje vecka baserat på vecka baserat |
| YearlyRetentionMonthsOfTheYear |Text |Hello årets månader som har valts för kvarhållning av årlig |
| YearlyRetentionTimes |Text |Datum och tid när årlig kvarhållning har konfigurerats |
| YearlyRetentionWeeksOfTheMonth |Text |Veckor hello månad när årlig kvarhållning är konfigurerad till exempel första, sista osv. |

### <a name="protected-server"></a>Skyddad Server
Den här tabellen innehåller grundläggande fält och aggregeringar över olika skyddade server-relaterade fält.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| #ProtectedServers |Heltal |Antal skyddade servrar |
| AsOnDateTime |Datum/tid |Senaste Uppdateringstid för hello valda rad |
| AzureBackupAgentOSType |Text |OS-typen för Azure Backup-agenten |
| AzureBackupAgentOSVersion |Text |OS-versionen av Azure Backup-agenten |
| AzureBackupAgentUpdateDate |Text |Datum när agenten Backup-agenten uppdaterades |
| AzureBackupAgentVersion |Text |Versionsnumret för agenten säkerhetskopiering Version |
| BackupManagementType |Text |Providertyp för att utföra säkerhetskopieringen exempelvis IaaSVM FileFolder |
| EntityState |Text |Aktuell status för hello skyddade serverobjekt exempelvis aktiv, tagits bort |
| ProtectedServerFriendlyName |Text |Eget namn på skyddad server |
| ProtectedServerName |Text |Namnet på skyddad server |
| ProtectedServerType |Text |Typ av skyddade server säkerhetskopieras till exempel IaaSVMContainer |
| ProtectedServerName |Text |Namnet på den skyddade servern toowhich Säkerhetskopiera objekt tillhör |
| RegisteredContainerId |Text |ID: t för behållaren som registrerats för säkerhetskopiering |

### <a name="storage"></a>Lagring
Den här tabellen innehåller grundläggande fält och aggregeringar över olika lagringsrelaterade fält.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| #ProtectedInstances |Decimaltal |Antalet skyddade instanser som används för att beräkna klientdel lagring i fakturering, beräknad baserat på senaste värdet i vald tid |
| AsOnDateTime |Datum/tid |Senaste Uppdateringstid för hello valda rad |
| CloudStorageInMB |Decimaltal |Säkerhetskopiering molnlagring som används av säkerhetskopior, beräknas baserat på senaste värdet i vald tid |
| EntityState |Text |Aktuell status för hello objektet exempelvis aktiv, tagits bort |
| LastUpdatedDate |Date |Datum då markerade raden senast uppdaterades |

### <a name="time"></a>Tid
Den här tabellen innehåller information om tidsrelaterade fält.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| Timme |Tid |Timme på dagen hello till exempel 1:00:00 PM |
| HourNumber |Decimaltal |Timme nummer i hello dag exempelvis 13,00 |
| Minut |Decimaltal |Hello minut |
| PeriodOfTheDay |Text |Period tidslucka hello dag exempelvis 12 3 AM |
| Tid |Tid |Den tid på dagen hello exempelvis 12:00:01: 00 |
| TimeKey |Text |Värdet för nyckeln toorepresent tid |

### <a name="vault"></a>Valv
Den här tabellen innehåller grundläggande fält och aggregeringar över olika valvet-relaterade fält.

| Fält | Datatyp | Beskrivning |
| --- | --- | --- |
| #Vaults |Heltal |Antal valv |
| AsOnDateTime |Datum/tid |Senaste Uppdateringstid för hello valda rad |
| AzureDataCenter |Text |Datacenter där valvet finns |
| EntityState |Text |Aktuell status för hello valvet objektet exempelvis aktiv, tagits bort |
| StorageReplicationType |Text |Typ av hello valvet exempelvis GeoRedundant storage-replikering |
| SubscriptionId |Text |Prenumerations-Id för hello kund som har markerats för att generera rapporter |
| VaultName |Text |Namnet på hello valvet |
| VaultTags |Text |Taggarna associerade toohello valvet |

## <a name="next-steps"></a>Nästa steg
När du granskar hello-datamodell för att skapa rapporter för Azure Backup finns hello följande artiklar för mer information om att skapa och visa rapporter i Power BI.

* [Skapa rapporter i Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
* [Filtrera rapporter i Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
