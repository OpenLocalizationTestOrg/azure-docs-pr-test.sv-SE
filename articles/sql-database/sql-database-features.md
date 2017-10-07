---
title: "aaaAzure översikt över funktioner i SQL-databasen | Microsoft Docs"
description: "Den här sidan innehåller en översikt över logiska hello Azure SQL Database-servrar och databaser och innehåller en funktion stöd matrix med länkar varje funktion i listan."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d1a46fa4-53d2-4d25-a0a7-92e8f9d70828
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 08/25/2017
ms.author: carlrab
ms.openlocfilehash: 463c88edcd38eabbc768cfb701bc74461836aa36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-features"></a>Azure SQL Database-funktioner

Azure SQL Database delar en gemensam kodbas med SQL Server och stöder de flesta av hello på hello databasnivå samma funktioner. hello större funktionsskillnader mellan Azure SQL Database och SQL Server finns på instansnivå hello. 

Vi fortsätta tooadd funktioner tooAzure SQL-databas. Så vi rekommenderar att du toovisit vår tjänst uppdateringar för webbsidan för Azure och toouse dess filter:

* Filtrerade toohello [SQL Database-tjänsten](https://azure.microsoft.com/updates/?service=sql-database).
* Filtrerade tooGeneral tillgänglighet [(GA) meddelanden](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) för SQL Database-funktioner.

## <a name="sql-server-and-sql-database-feature-support"></a>Stöder SQL Server och SQL-databas

hello nedan visas hello viktiga funktioner i SQL Server och ger information om huruvida varje viss funktion stöds och en länk toomore information om hello-funktionen. Transact-SQL skillnader tooconsider när du migrerar en befintlig databaslösning, se [lösa Transact-SQL-skillnader under migreringen tooSQL databasen](sql-database-transact-sql-information.md).


| **SQL Server-funktionen** | **Stöds i Azure SQL-databas** | 
| --- | --- |  
| [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Ja - finns [certifikatarkivet](sql-database-always-encrypted.md) och [Key vault](sql-database-always-encrypted-azure-key-vault.md)|
| [AlwaysOn-Tillgänglighetsgrupper](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | Finns inte - [redundans grupper och aktiv geo-replikering](sql-database-geo-replication-overview.md) |
| [Ansluta en databas](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Nej |
| [Programroller](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Ja |
| [BACPAC fil (exportera)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Ja - finns [SQL Database-export](sql-database-export.md) |
| [BACPAC fil (importera)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Ja - finns [Importera SQL-databas](sql-database-import.md) |
| [BACKUP-kommandot](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Finns inte - [automatisk säkerhetskopiering](sql-database-automated-backups.md) |
| [Inbyggda funktioner](https://docs.microsoft.com/sql/t-sql/functions/functions) | De flesta - finns enskilda funktioner |
| [Registrering av ändringsdata](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Nej |
| [Spårning av ändringar](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Ja |
| [Instruktioner för sortering](https://docs.microsoft.com/sql/t-sql/statements/collations) | Ja |
| [Columnstore-index](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Ja - [endast Premium edition](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |
| [CLR (CLR)](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Nej |
| [Inneslutna databaser](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Ja |
| [Inneslutna användare](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Ja |
| [Kontroll av flödet nyckelord](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Ja |
| [Mellan databasfrågor](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/cross-database-queries) | Delvis – se [Elastiska frågor](sql-database-elastic-query-overview.md) |
| [Markörer](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Ja | 
| [Datakomprimering](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Ja |
| [Database mail](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Nej |
| [Databasspegling](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Nej |
| [Konfigurationsinställningar för databasen](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Ja |
| [Data Quality Services DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Nej |
| [Databasögonblicksbilder](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Nej |
| [Datatyper](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Ja |  
| [DBCC-instruktioner](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | De flesta - finns enskilda uttryck |
| [DDL-instruktioner](https://docs.microsoft.com/sql/t-sql/statements/statements) | De flesta - finns enskilda uttryck
| [DDL-utlösare](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Endast databas |
| [Distribuerade transaktioner - MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Finns inte - [elastisk transaktioner](sql-database-elastic-transactions-overview.md) |
| [DML-instruktioner](https://docs.microsoft.com/sql/t-sql/queries/queries) | De flesta - finns enskilda uttryck |
| [DML-utlösare](https://docs.microsoft.com/sql/relational-databases/triggers/dml-triggers) |
| [DMV](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Vissa - se enskilda av DMV: er |
| [Händelseaviseringar](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | Finns inte - [aviseringar](sql-database-insights-alerts-portal.md) |
| [Uttryck](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Ja |
| [Utökade händelser](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Vissa - se enskilda händelser |
| [Utökade lagrade procedurer](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Nej |
| [Filer och filgrupper](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Endast primära filgruppen |
| [FileStream](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Nej |
| [Fulltextsökning](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) | Tredjeparts-funnits stöds inte |
| [Funktioner](https://docs.microsoft.com/sql/t-sql/functions/functions) | De flesta - finns enskilda funktioner |
| [Diagrammet bearbetning](/sql/relational-databases/graphs/sql-graph-overview) | Ja |
| [Minnesintern optimering](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Ja - [endast Premium edition](sql-database-in-memory.md) |
| [Stöd för JSON-data](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | Ja |
| [Språkelement](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | De flesta - finns i enskilda element |  
| [Länkade servrar](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Finns inte - [elastisk fråga](sql-database-elastic-query-horizontal-partitioning.md) |
| [Loggöverföring](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | Finns inte - [redundans grupper och aktiv geo-replikering](sql-database-geo-replication-overview.md) |
| [Master Data Services (MDS)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Nej |
| [Minimal loggning i massimport](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Nej |
| [Ändra systemdata](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Nej |
| [Indexåtgärder online](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Transaktionsstorlek som stöds - begränsas av tjänstnivå |
| [Operatörer](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | De flesta - finns i enskilda operatorer |
| [Återställning vid tidpunkt databas](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Ja - finns [återställning av SQL-databasen](sql-database-recovery-using-backups.md#point-in-time-restore) |
| [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Nej |
| [Principbaserad hantering](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Nej |
| [Predikat](https://docs.microsoft.com/sql/t-sql/queries/predicates) | De flesta - finns enskilda Predikat |
| [R-tjänster](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Nej |
| [Resursstyrningen](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Nej |
| [ÅTERSTÄLLA rapporter](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Nej | 
| [Återställa databasen från en säkerhetskopia](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | Se från inbyggda säkerhetskopieringar endast - [återställning av SQL-databasen](sql-database-recovery-using-backups.md) |
| [Säkerhet på radnivå](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Ja |
| [Semantiska sökning](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Nej |
| [Sekvensnummer](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Ja |
| [Service Broker](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Nej |
| [Server-konfigurationsinställningar](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Nej |
| [Set-instruktioner](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | De flesta - finns enskilda uttryck 
| [Rumslig](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Ja |
| [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Nummer Se [elastiska jobb](sql-database-elastic-jobs-getting-started.md) |
| [SQL Server Analysis Services (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Finns inte - [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| [SQL Server-granskning](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | Finns inte - [SQL Database auditing](sql-database-auditing.md) |
| [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Finns inte - [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) |
| [SQL Server PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Ja |
| SQL Server-profiler | [Stöds](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Finns inte - [utökade händelser](sql-database-xevent-db-diff-from-svr.md) |
| [SQL Server-replikering](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Prenumerant för transaktions-och ögonblicksbildsreplikering endast](sql-database-cloud-migrate.md) |
| [SQLServer Reporting Services (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Nej |
| [Lagrade procedurer](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Ja |
| [Systemfunktioner lagras](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Vissa - se enskilda funktioner |
| [Systemets lagrade procedurer](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Vissa - se enskilda lagrade procedurer |
| [Systemtabeller](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Vissa - se enskilda tabeller |
| [System katalogvyer](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Vissa - se enskilda vyer |
| [Table partitioning](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) (Tabellpartitionering) | Ja - endast primära filgruppen |
| [Temporary tables](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#temporary-tables) (Temporära tabeller) | Lokala och databas-omfattande globala temporära tabeller endast |
| [Temporala tabeller](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | Ja |
| [Transaktioner](https://docs.microsoft.com/sql/t-sql/language-elements/transactions-transact-sql) | Nej |
| [Variabler](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Ja | 
| [Transparent datakryptering (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Ja |
| [Windows Server Failover-kluster](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | Finns inte - [redundans grupper och aktiv geo-replikering](sql-database-geo-replication-overview.md) |
| [XML-index](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Ja |

## <a name="next-steps"></a>Nästa steg

- Information om hello Azure SQL Database-tjänsten finns [vad är SQL Database?](sql-database-technical-overview.md)
- Information om stöd för Transact-SQL och skillnader finns [lösa Transact-SQL-skillnader under migreringen tooSQL databasen](sql-database-transact-sql-information.md).
