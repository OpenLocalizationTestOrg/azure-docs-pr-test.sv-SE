---
title: aaaExport en Azure SQL database tooa BACPAC fil | Microsoft Docs
description: Exportera en Azure SQL database tooa BACPAC-fil med hello Azure-portalen
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 41d63a97-37db-4e40-b652-77c2fd1c09b7
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.date: 06/15/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: cb3b4227318e0fd2114529c86c9792615fe7fd1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="export-an-azure-sql-database-tooa-bacpac-file"></a>Exportera en Azure SQL database tooa BACPAC fil

När du behöver tooexport en databas för arkivering eller för glidande tooanother plattform, kan du exportera hello databasen schema- och tooa [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fil. En BACPAC-fil är en ZIP-fil med filnamnstillägget BACPAC som innehåller hello metadata och data från en SQL Server-databas. En BACPAC-fil kan lagras i Azure blob storage eller i lokal lagring på en lokal plats och senare importeras tillbaka till Azure SQL Database eller till en lokal SQL Server-installation. 

> [!IMPORTANT] 
> Azure SQL Database automatiserad Export har avslutats på 1 mars 2017. Du kan använda [långsiktig lagring av säkerhetskopior](sql-database-long-term-retention.md
) eller [Azure Automation](https://github.com/Microsoft/azure-docs-pr/blob/2461f706f8fc1150e69312098640c0676206a531/articles/automation/automation-intro.md) tooperiodically Arkiv SQL-databaser med hjälp av PowerShell enligt tooa schema du väljer. Hämta en exempelfil hello [exempel PowerShell-skript](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) från Github.
>

## <a name="considerations-when-exporting-an-azure-sql-database"></a>Att tänka på när du exporterar en Azure SQL database

* För en export toobe transaktionellt konsekvent, måste du kontrollera antingen att ingen skrivning aktivitet sker under exporten hello eller att du exporterar från en [transaktionellt konsekvent kopiera](sql-database-copy.md) för din Azure SQL-databas.
* Om du exporterar tooblob lagring är hello maximal storlek för en BACPAC-fil 200 GB. tooarchive en större BACPAC fil exportera toolocal lagring.
* Exportera en BACPAC filen tooAzure premium-lagring med hjälp av hello metoderna som beskrivs i den här artikeln stöds inte.
* Om hello exportåtgärden från Azure SQL Database överskrider 20 timmar, kan den avbrytas. tooincrease prestanda under export, kan du:
  * Tillfälligt öka din servicenivå.
  * Upphör alla läsa och skriva aktiviteten under hello exporten.
  * Använd en [grupperat index](https://msdn.microsoft.com/library/ms190457.aspx) med icke-null-värden för alla stora tabeller. Utan grupperade index, kan en export misslyckas om det tar längre tid än 6 – 12 timmar. Det beror på att hello export service måste toocomplete en genomsökningen tootry tooexport hela tabellen. Ett bra sätt toodetermine om dina tabeller som är optimerade för export är toorun **DBCC SHOW_STATISTICS** och se till att hello *RANGE_HI_KEY* inte är null och dess värde har bra distribution. Mer information finns i [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [!NOTE]
> BACPACs är inte avsedda toobe som används för säkerhetskopiering och återställning. Azure SQL Database skapar automatiskt säkerhetskopior för varje databas. Mer information finns i [översikt över verksamhetskontinuitet](sql-database-business-continuity.md) och [SQL databassäkerhetskopieringar](sql-database-automated-backups.md).  
> 

## <a name="export-tooa-bacpac-file-using-hello-azure-portal"></a>Exportera tooa BACPAC-fil med hello Azure-portalen

en databas med hjälp av tooexport hello [Azure-portalen](https://portal.azure.com), öppna hello-sida för din databas och klickar på **exportera** hello i verktygsfältet. Ange hello BACPAC filnamn Anger hello Azure storage-konto och en behållare för hello export och ange hello autentiseringsuppgifter tooconnect toohello källdatabas.  

![Databasexport](./media/sql-database-export/database-export.png)

toomonitor hello fortskrider hello exportåtgärden, öppna hello sidan för hello logisk server som innehåller hello databas som exporteras. Rulla nedåt för**Operations** och klicka sedan på **Import/Export** historik.

![Exportera tidigare](./media/sql-database-export/export-history.png)
![exportstatus historik](./media/sql-database-export/export-history2.png)

## <a name="export-tooa-bacpac-file-using-hello-sqlpackage-utility"></a>Exportera tooa BACPAC fil med hello SQLPackage-verktyget

tooexport en SQL-databas med hjälp av hello [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) kommandoradsverktyget finns [exportera parametrar och egenskaper](https://msdn.microsoft.com/library/hh550080.aspx#Export Parameters and Properties). Hej SQLPackage verktyget som levereras med hello senaste versionerna av [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) och [SQL Server Data Tools för Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), eller så kan du hämta hello senaste versionen av [ SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) direkt från hello Microsoft hämtningssidan.

Vi rekommenderar hello användningen av hello SQLPackage verktyg för skalning och prestanda i de flesta produktionsmiljöer. Bloggen om att migrera med BACPAC filer finns i en SQL Server Customer Advisory Team [migrera från SQL Server-tooAzure SQL-databas med hjälp av BACPAC filer](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Det här exemplet illustrerar hur tooexport en databas med hjälp av SqlPackage.exe med Active Directory Universal autentisering:

```cmd
SqlPackage.exe /a:Export /tf:testExport.bacpac /scs:"Data Source=apptestserver.database.windows.net;Initial Catalog=MyDB;" /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="export-tooa-bacpac-file-using-sql-server-management-studio-ssms"></a>Exportera tooa BACPAC-fil med SQL Server Management Studio (SSMS)

hello senaste versioner av SQL Server Management Studio ger också en guiden tooexport en Azure SQL Database tooa BACPAC-fil. Se hello [exportera en Dataskiktsprogrammet](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application).

## <a name="export-tooa-bacpac-file-using-powershell"></a>Tooa BACPAC exportfilen med hjälp av PowerShell

Använd hello [ny AzureRmSqlDatabaseExport](/powershell/module/azurerm.sql/new-azurermsqldatabaseexport) cmdlet toosubmit en export databasen begäran toohello Azure SQL Database-tjänsten. Hello exportåtgärden kan ta viss tid toocomplete beroende på hello storleken på databasen.

 ```powershell
 $exportRequest = New-AzureRmSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
   -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
   -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password
 ```

toocheck hello status för hello begäran export använder hello [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet. Kör detta omedelbart efter hello begära vanligtvis returnerar **Status: InProgress**. När du ser **Status: lyckades** hello exporten har slutförts.

```powershell
$exportStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
[Console]::Write("Exporting")
while ($exportStatus.Status -eq "InProgress")
{
    $exportStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$exportStatus
```

## <a name="next-steps"></a>Nästa steg

* toolearn om långsiktig säkerhetskopiering kvarhållning av en Azure SQL databassäkerhetskopia som ett alternativt tooexported en databas för arkivering, se [långsiktig lagring av säkerhetskopior](sql-database-long-term-retention.md).
- Bloggen om att migrera med BACPAC filer finns i en SQL Server Customer Advisory Team [migrera från SQL Server-tooAzure SQL-databas med hjälp av BACPAC filer](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* toolearn om hur du importerar en BACPAC tooa SQL Server-databas finns [Importera SQL Server-databasen BACPCAC tooa](https://msdn.microsoft.com/library/hh710052.aspx).
* toolearn exportera en BACPAC från en SQL Server-databas finns [exportera en Dataskiktsprogrammet](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) och [migrera din första databas](sql-database-migrate-your-sql-server-database.md).
* Om du exporterar från SQL Server som en SQL-databas med prelude toomigration tooAzure [migrera en SQL-databas med SQL Server-databasen tooAzure](sql-database-cloud-migrate.md).
