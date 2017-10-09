---
title: "aaaRestore Azure SQL-databas från en säkerhetskopia | Microsoft Docs"
description: "Läs mer om Point-in-Time-återställning, som gör att du tooroll tillbaka en tidigare punkt i Azure SQL Database tooa tidpunkt (upp too35 dagar)."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: monicar
ms.assetid: fd1d334d-a035-4a55-9446-d1cf750d9cf7
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: carlrab
ms.openlocfilehash: 84ecc306a2072445c10dbf1e9d7cf3d1b43860cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Återställa en Azure SQL-databas med automatisk databassäkerhetskopieringar
SQL-databas finns följande alternativ för databas återställning med hjälp av [automatisk säkerhetskopiering av databaser](sql-database-automated-backups.md) och [säkerhetskopieringar i långsiktig kvarhållning](sql-database-long-term-retention.md). Du kan återställa från en säkerhetskopia av databasen till:

* En ny databas på samma logiska server återställs hello tooa angetts punkt i tiden i hello Bevarandeperiod. 
* En databas på hello samma logiska server återställs toohello borttagningstid för en borttagen databas.
* En ny databas på en logisk server i en region återställas toohello punkt i hello senaste dagliga säkerhetskopior i georeplikerad blob storage (RA-GRS).

> [!IMPORTANT]
> Du kan inte skriva över en befintlig databas under återställning.
>

Du kan också använda [automatisk säkerhetskopiering av databaser](sql-database-automated-backups.md) toocreate en [databaskopieringen](sql-database-copy.md) på någon logisk server i en region. 

## <a name="recovery-time"></a>Tiden för återställning
Hej recovery tid toorestore en databas med automatisk databassäkerhetskopieringar påverkas av flera faktorer: 

* hello storleken på hello-databasen
* hello prestandanivå hello-databas
* hello antalet transaktionsloggar ingår
* hello mängden aktivitet som måste toobe spelas toorecover toohello återställningspunkt
* hello nätverksbandbredden är om hello återställer tooa annan region 
* hello antalet samtidiga återställning begäranden som bearbetas i hello målregionen. 
  
  För en stor och/eller mycket aktiv databas kan hello återställning ta flera timmar. Om det är långvariga avbrott i en region, är det möjligt att det finns ett stort antal geo-återställning begäranden som bearbetas av andra regioner. När det finns många begäranden, öka hello återställningstid för databaser i den regionen. De flesta databasen återställs fullständig inom 12 timmar.
  
Det finns ingen inbyggd funktion toodo bulk återställning. Hej [Azure SQL Database: fullständig serveråterställning](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) skript är ett exempel på ett sätt att utföra den här uppgiften.

> [!IMPORTANT]
> toorecover med hjälp av automatisk säkerhetskopiering, måste du vara medlem i hello SQL Server deltagarrollen hello prenumeration eller hello prenumerationsägaren. Du kan återställa med hello Azure-portalen, PowerShell eller hello REST API. Du kan inte använda Transact-SQL. 
> 

## <a name="point-in-time-restore"></a>Point-In-Time-återställning

Du kan återställa en befintlig databas tooan som tidigare punkt i tiden som en ny databas på hello samma logiska server använder hello Azure-portalen [PowerShell](https://docs.microsoft.com/en-us/powershell/module/azurerm.sql/restore-azurermsqldatabase), eller hello [REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [!TIP]
> För en PowerShell-exempelskript visar hur tooperform en point-in-time-återställning av en databas, se [återställa en SQL-databas med hjälp av PowerShell](scripts/sql-database-restore-database-powershell.md).
>

hello-databasen kan vara återställda tooany tjänstnivån eller prestanda nivå, och som en enskild databas eller i en elastisk pool. Kontrollera att du har tillräckligt med resurser på hello logisk server eller hello elastisk pool toowhich återställs hello-databasen. När borttagningen är klar är hello återställa databasen en normal, fullständigt tillgänglig online-databas. hello återställs databasen debiteras med normal priser baserat på dess servicenivå för tjänstnivå och prestandanivå. Du inte betalar avgifter tills hello databasen återställningen är slutförd.

Du återställer en databas tooan vanligtvis tidigare peka för återställning. När du gör det kan du hantera återställd databas som en ersättning för hello originaldatabasen hello eller använda den tooretrieve data från och uppdatera sedan hello originaldatabasen. 

* ***Databasen ersättning:*** om hello återställa databasen är avsedd som en ersättning för hello originaldatabasen, bör du kontrollera hello prestandanivå och/eller tjänstnivå är lämpliga och skala hello databasen om det behövs. Du kan byta namn på hello originaldatabasen och sedan ge hello återställts hello ursprungliga databasnamnet med hello ALTER DATABASE-kommandot i T-SQL. 
* ***Återställning av data:*** om du planerar tooretrieve data från hello återställs databasen toorecover från en användare eller ett program fel du behöver toowrite och köra hello nödvändiga data recovery skript tooextract data från hello återställs databasen toohello originaldatabasen. Även om hello återställningen kan ta en lång tid toocomplete, visas hello återställer databasen i hello databaslistan i hela hello återställningsprocessen. Om du tar bort hello databasen under en återställning av hello hello återställningen avbryts och du debiteras inte för hello-databas som hello återställningen inte slutfördes. 

### <a name="azure-portal"></a>Azure Portal

toorecover tooa punkten i gång med hjälp av hello Azure-portalen, öppna hello sidan för din databas och klickar på **återställa** hello i verktygsfältet.

![punkt i tiden återställning](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

## <a name="deleted-database-restore"></a>Återställning av databasen borttagen
Du kan återställa en borttagen databas toohello borttagningstid för en borttagen databas på hello samma logiska server med hjälp av hello Azure-portalen [PowerShell](https://docs.microsoft.com/en-us/powershell/module/azurerm.sql/restore-azurermsqldatabase), eller hello [REST (createMode = återställer)](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [!TIP]
> Ett exempel PowerShell-skript visar hur toorestore en borttagen databas finns [återställa en SQL-databas med hjälp av PowerShell](scripts/sql-database-restore-database-powershell.md).
>

> [!IMPORTANT]
> Om du tar bort en Azure SQL Database server-instansen tas även bort alla databaser och kan inte återställas. Det finns inget stöd för att återställa en borttagen server.
> 

### <a name="azure-portal"></a>Azure Portal

toorecover en borttagna databasen under dess [kvarhållningsperioden](sql-database-service-tiers.md) på hello Azure-portalen, öppna hello sidan för din server och i hello Operations området klickar du på **bort databaser**.

![ta bort-databasen-restore-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)


![ta bort-databasen-restore-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

## <a name="geo-restore"></a>Geo-återställning
Du kan återställa en SQL-databas på en server i Azure-region från hello senaste georeplikerad fullständig och differentiell säkerhetskopiering. GEO-återställning använder geo-redundant säkerhetskopia som källa och kan vara används toorecover en databas även om hello databas eller datacenter inte är tillgänglig på grund av tooan avbrott. 

GEO-återställning är hello standardalternativet när databasen är inte tillgänglig på grund av en incident i hello region där hello-databasen finns. Om en storskalig incident i en region resulterar i att programmets databasen återställer du en databas från hello georeplikerad säkerhetskopieringar tooa server i en annan region. Det uppstår en fördröjning mellan när en differentiell säkerhetskopiering utförs och när den är georeplikerad tooan Azure blob i en annan region. Fördröjningen kan vara upp tooan timme, så om en olycka inträffar, det kan vara upp tooone timme data går förlorade. hello följande bild visar återställning av databasen hello från hello senaste tillgängliga säkerhetskopian i en annan region.

![GEO-återställning](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Ett exempel PowerShell-skript visar hur tooperform en geo-återställning, se [återställa en SQL-databas med hjälp av PowerShell](scripts/sql-database-restore-database-powershell.md).
> 

Point-in-time-återställning på en geo sekundär stöds inte för närvarande. Point-in-time-återställning kan göras endast på en primär databas. Detaljerad information om hur du använder geo-återställning toorecover efter ett avbrott finns [återställa från avbrott](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Återställning från säkerhetskopior är hello mest grundläggande av hello disaster recovery-lösningar som är tillgängliga i SQL-databas med hello längsta Återställningspunktmål och uppskattning Recovery tid (Infoga). Lösningar med hjälp av grundläggande databaser, är geo-återställning ofta en rimlig DR-lösning med en Infoga på 12 timmar. Lösningar med hjälp av större Standard eller Premium-databaser som kräver kortare återställningstiden, bör du använda [aktiv geo-replikering](sql-database-geo-replication-overview.md). Aktiv geo-replikering har en mycket lägre Återställningspunktmål och infoga eftersom det endast kräver att du påbörja en växling vid fel tooa kontinuerligt replikeras sekundär. Läs mer på företag contiuity alternativ [av företagskontinuitet](sql-database-business-continuity.md).
> 

### <a name="azure-portal"></a>Azure Portal

toogeo Återställ en databas under dess [kvarhållningsperioden](sql-database-service-tiers.md) använder hello Azure-portalen, öppna hello SQL-databaser sida och klicka sedan på **Lägg till**. I hello **Välj källa** textruta, Välj **säkerhetskopiering**. Ange hello säkerhetskopiering från vilken tooperform hello recovery i hello region och hello server du väljer. 

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Programmässigt utföra återställning med hjälp av automatisk säkerhetskopiering
Som vi nämnt tidigare, dessutom toohello Azure-portalen databasåterställning kan utföras via programmering med hjälp av Azure PowerShell eller hello REST API. hello följande tabeller beskrivs hello uppsättning kommandon som är tillgängliga.

### <a name="powershell"></a>PowerShell
| Cmdlet | Beskrivning |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Hämtar en eller flera databaser. |
| [Get-AzureRMSqlDeletedDatabaseBackup](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | Hämtar en borttagen databas som du kan återställa. |
| [Get-AzureRmSqlDatabaseGeoBackup](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) |Hämtar en geo-redundant säkerhetskopia av en databas. |
| [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) |Återställer en SQL-databas. |
|  | |

### <a name="rest-api"></a>REST API
| API | Beskrivning |
| --- | --- |
| [REST (createMode = återställning)](https://msdn.microsoft.com/library/azure/mt163685.aspx) |Återställer en databas |
| [Hämta skapa eller uppdatera databasens Status](https://msdn.microsoft.com/library/azure/mt643934.aspx) |Returnerar hello status under en återställning |
|  | |

## <a name="summary"></a>Sammanfattning
Automatisk säkerhetskopiering skydda dina databaser från användar- och programfel, oavsiktliga databasadministratörer att ta bort och långvariga avbrott. Den här inbyggda funktionen är tillgänglig för alla servicenivåer och prestandanivåer. 

## <a name="next-steps"></a>Nästa steg
* En översikt över verksamhetskontinuitet och scenarier finns [översikt över verksamhetskontinuitet](sql-database-business-continuity.md)
* toolearn om Azure SQL Database automatisk säkerhetskopiering finns [SQL-databas automatisk säkerhetskopiering](sql-database-automated-backups.md)
* toolearn om långsiktig säkerhetskopiering kvarhållning finns [långsiktig lagring av säkerhetskopior.](sql-database-long-term-retention.md)
* tooconfigure, hantera och återställa från långsiktig kvarhållning av automatiska säkerhetskopieringar i ett Azure Recovery Services-valv med hello Azure portal, se [konfigurera och använda långsiktig säkerhetskopiering kvarhållning](sql-database-long-term-backup-retention-configure.md). 
* toolearn om snabbare återställningsalternativ finns [aktiv geo-replikering](sql-database-geo-replication-overview.md)  
