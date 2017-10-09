---
title: "aaaAzure SQL Data Warehouse säkerhetskopieringar - ögonblicksbilder, geo-redundant | Microsoft Docs"
description: "Läs mer om SQL Data Warehouse inbyggd databassäkerhetskopieringar som gör att du toorestore en återställningspunkt för Azure SQL Data Warehouse tooa eller en annan geografisk region."
services: sql-data-warehouse
documentationcenter: 
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: b5aff094-05b2-4578-acf3-ec456656febd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: 34659480485246f54a1490e185fc1b903fb2520d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse-backups"></a>SQL Data Warehouse-säkerhetskopieringar
SQL Data Warehouse erbjuder både lokala och geografiska säkerhetskopior som en del av dess data warehouse-säkerhetskopiering. Dessa inkluderar Azure Storage Blob ögonblicksbilder och geo-redundant lagring. Använd data warehouse säkerhetskopieringar toorestore data warehouse tooa återställningen peka i hello primära region eller återställa tooa olika geografiska region. Den här artikeln förklarar hello specifika säkerhetskopior i SQL Data Warehouse.

## <a name="what-is-a-data-warehouse-backup"></a>Vad är en säkerhetskopiering av data warehouse?
En säkerhetskopiering av data warehouse är hello data som du kan använda toorestore en viss tid för data warehouse tooa.  Eftersom SQL Data Warehouse är ett distribuerat system, består en data warehouse-säkerhetskopia av många filer som lagras i Azure BLOB. 

Säkerhetskopiorna av databasen är en viktig del av alla disaster recovery strategi för affärskontinuitet och eftersom de skyddar dina data från oavsiktliga skador eller tas bort. Mer information finns i [översikt över verksamhetskontinuitet](../sql-database/sql-database-business-continuity.md).

## <a name="data-redundancy"></a>Dataredundans
SQL Data Warehouse skyddar dina data genom att lagra data i lokalt redundant (LRS) Azure Premium-lagring. Funktionen Azure Storage lagrar flera synkrona kopior av hello data i hello lokala data transparent dataskydd för center tooguarantee om lokaliserade fel. Dataredundans garanterar att data kan överleva infrastruktur problem som till exempel diskfel. Dataredundans säkerställer kontinuitet för företag med en feltolerant och infrastruktur med hög tillgänglighet.

Mer information om toolearn:

* Azure Premium-lagring finns [introduktion tooAzure Premium-lagring](../storage/common/storage-premium-storage.md).
* Lokalt Redundant lagring finns [Azure Storage-replikering](../storage/common/storage-redundancy.md#locally-redundant-storage).

## <a name="azure-storage-blob-snapshots"></a>Azure Storage Blob-ögonblicksbilder
Som en fördel med att använda Azure Premium Storage SQL Data Warehouse använder Azure Storage Blob ögonblicksbilder toobackup hello datalagret lokalt. Du kan återställa en återställningspunkt för data warehouse tooa ögonblicksbild. Ögonblicksbilder starta minst var åttonde timme och är tillgängliga i sju dagar.  

Mer information om toolearn:

* Azure blob-ögonblicksbilder finns [skapa en ögonblicksbild av blob](../storage/blobs/storage-blob-snapshots.md).

## <a name="geo-redundant-backups"></a>GEO-redundant säkerhetskopieringar
Var 24: e timme lagrar SQL Data Warehouse hello fullständig datalagret i standardlagring. hello fullständig datalagret skapas toomatch hello tid för senaste hello-ögonblicksbild. hello standardlagring tillhör tooa geo-redundant lagringskonto med läsbehörighet (RA-GRS). hello Azure Storage RA-GRS funktionen replikerar hello säkerhetskopior tooa [parad Datacenter](../best-practices-availability-paired-regions.md). Den här georeplikering gör att du kan återställa datalagret om du inte kommer åt hello ögonblicksbilder i din primära region. 

Den här funktionen är aktiverad som standard. Om du inte vill toouse geo-redundant säkerhetskopior, du kan [avanmälas] (https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicy?redirectedfrom=msdn). 

> [!NOTE]
> I Azure storage hello termen *replikering* refererar toocopying filer från en plats tooanother. SQL- *databasreplikering* refererar tookeeping toomultiple sekundära databaser synkroniseras med en primär databas. 
> 
> 

> [!NOTE]
> Du kan inte välja bort geo-redundant säkerhetskopior med DWU 9000 och DWU 18000. 
>
> 

Mer information om toolearn:

* GEO-redundant lagring finns [Azure Storage-replikering](../storage/common/storage-redundancy.md).
* RA-GRS lagring finns [geo-redundant lagring med läsbehörighet](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage).

## <a name="data-warehouse-backup-schedule-and-retention-period"></a>Schemat för säkerhetskopiering och kvarhållningsperiod datalager
SQL Data Warehouse skapar ögonblicksbilder på dina online-datalager som alla fyra tooeight timmar och behåller varje ögonblicksbild i sju dagar. Du kan återställa din onlinedatabasen tooone hello Återställningspunkter i hello senaste sju dagarna. 

toosee när hello senaste ögonblicksbilden startas kör frågan på ditt online SQL Data Warehouse. 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Om du behöver tooretain en ögonblicksbild av en längre än sju dagar kan återställa du en återställning punkt tooa Nytt datalager. När hello återställningen är klar startar SQL Data Warehouse skapar ögonblicksbilder på hello Nytt datalager. Om du inte gör ändringar toohello Nytt datalager hello ögonblicksbilder förblir tom och därför hello ögonblicksbild kostnaden är minimal. Du kan också pausa hello databasen tookeep SQL Data Warehouse från att skapa ögonblicksbilder.

### <a name="what-happens-toomy-backup-retention-while-my-data-warehouse-is-paused"></a>Vad händer lagring av säkerhetskopior toomy medan min datalagret pausas?
SQL Data Warehouse skapar inte ögonblicksbilder och upphör inte ögonblicksbilder medan ett informationslager har pausats. hello ögonblicksbild ålder ändras inte när hello-datalagret har pausats. Ögonblicksbild kvarhållning baseras på hello antalet dagar hello-datalagret är online, inte kalenderdagar.

Om en ögonblicksbild startas 1 oktober klockan 4 och hello-datalagret är pausad 3 oktober klockan 4 är hello ögonblicksbilden två dagar gammal. När hello datalagret går tillbaka online är hello ögonblicksbilden två dagar gammal. Om hello-datalagret är online 5 oktober klockan 4 hello ögonblicksbild är två dagar gammal och förblir i fem dagar.

När hello-datalagret är online igen återupptar nya ögonblicksbilder SQL Data Warehouse och upphör att gälla ögonblicksbilder när de har mer än sju dagar data.

### <a name="how-long-is-hello-retention-period-for-a-dropped-data-warehouse"></a>Hur lång tid är hello kvarhållningsperiod för en släppt datalagret?
När ett informationslager släpps hello datalagret och hello ögonblicksbilder sparas i sju dagar och sedan har tagits bort. Du kan återställa hello data warehouse tooany hello spara återställningspunkter.

> [!IMPORTANT]
> Om du tar bort en logisk SQL-serverinstans tas även bort alla databaser som tillhör instansen toohello och kan inte återställas. Du kan inte återställa en server som har tagits bort.
> 
> 

## <a name="data-warehouse-backup-costs"></a>Data warehouse säkerhetskopiering kostnader
hello totala kostnaden för ditt primära datalagret och sju dagar i Azure Blob-ögonblicksbilder är avrundade toohello närmsta TB. Om ditt data warehouse är 1,5 TB och hello ögonblicksbilder använder 100 GB, debiteras du till exempel för 2 TB data till Azure Premium Storage-priser. 

> [!NOTE]
> Varje ögonblicksbild är tom från början och växer när du gör ändringar toohello primära datalagret. Alla ögonblicksbilder ökar i storlek som hello data warehouse ändringar. Hello lagringskostnader för ögonblicksbilder växer därför bl.a toohello i ändringstakt.
> 
> 

Om du använder geo-redundant lagring, får du en separat lagring kostnad. du debiteras hello geo-redundant lagring med hello-geografiskt Redundant lagring med läsbehörighet (RA-GRS) normalpris.

Mer information om priser för SQL Data Warehouse finns [priser för SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="using-database-backups"></a>Med hjälp av databassäkerhetskopieringar
hello primär användning för säkerhetskopiering av SQL data warehouse är toorestore hello data warehouse tooone hello Återställningspunkter i hello Bevarandeperiod.  

* toorestore ett SQL data warehouse finns [återställer ett SQL data warehouse](sql-data-warehouse-restore-database-overview.md).

## <a name="related-topics"></a>Relaterade ämnen
### <a name="scenarios"></a>Scenarier
* En översikt över verksamhetskontinuitet, se [översikt över verksamhetskontinuitet](../sql-database/sql-database-business-continuity.md)

<!-- ### Tasks -->

* toorestore datalager, se [återställer ett SQL data warehouse](sql-data-warehouse-restore-database-overview.md)

<!-- ### Tutorials -->

