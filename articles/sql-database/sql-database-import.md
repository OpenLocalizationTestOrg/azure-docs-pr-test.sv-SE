---
title: aaaImport en BACPAC filen toocreate en Azure SQL database | Microsoft Docs
description: Skapa en newAzure SQL-databas genom att importera en BACPAC-fil.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: cf9a9631-56aa-4985-a565-1cacc297871d
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.date: 06/26/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 0d5fc93acf27b79502969fcd6199d11161915b19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="import-a-bacpac-file-tooa-new-azure-sql-database"></a>Importera en BACPAC filen tooa ny Azure SQL-databas

När du behöver tooimport en databas från ett arkiv eller när du migrerar från en annan plattform, kan du importera hello databasschemat och data från en [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fil. En BACPAC-fil är en ZIP-fil med filnamnstillägget BACPAC som innehåller hello metadata och data från en SQL Server-databas. En BACPAC-fil kan importeras från Azure blob storage (endast standardlagring) eller från lokal lagring på en lokal plats. toomaximize hello importera hastighet, rekommenderar vi att du anger en högre nivå och prestanda servicenivå, till exempel en P6 och sedan skala toodown efter behov efter hello importen är klar. Dessutom hello databasens kompatibilitetsnivå efter hello import baseras på hello kompatibilitetsnivå hello källdatabasen. 

> [!IMPORTANT] 
> När du migrerar din databas tooAzure SQL-databas, väljer du toooperate hello databas på dess aktuella kompatibilitetsnivån (nivå 100 för hello AdventureWorks2008R2 databasen) eller på en högre nivå. Läs mer om hello effekter och alternativ för att driva en databas på en specifik kompatibilitetsnivå [ändra DATABASENS kompatibilitetsnivån](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Se även [ALTER OMFÅNG DATABASKONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) information om ytterligare databasnivå inställningar relaterade toocompatibility nivåer.   >

> [!NOTE]
> tooimport en BACPAC tooa ny databas, måste du först skapa en logisk Azure SQL Database-server. En självstudiekurs visar hur toomigrate en SQL Server-databas tooAzure SQL-databas med hjälp av SQLPackage, finns [migrera en SQL Server-databas](sql-database-migrate-your-sql-server-database.md)
>

## <a name="import-from-a-bacpac-file-using-azure-portal"></a>Importera från en BACPAC-fil med hjälp av Azure portal

Den här artikeln innehåller anvisningar för att skapa en Azure SQL-databas från en BACPAC-fil som lagras i Azure blob storage med hjälp av hello [Azure-portalen](https://portal.azure.com). Import med hjälp av hello Azure portal stöder importera en BACPAC-fil från Azure blob storage.

en databas med hjälp av tooimport hello Azure-portalen, öppna hello sidan för din databas och klickar på **importera** hello i verktygsfältet. Ange hello storage-konto och en behållare och markera hello BACPAC tooimport. Välj hello storlek för hello ny databas (vanligtvis hello densamma som originalet) och ange autentiseringsuppgifter för hello mål SQL Server.  

   ![Databasimport](./media/sql-database-import/import.png)

toomonitor hello fortskrider hello importåtgärden, öppna hello sidan för hello logisk server som innehåller hello databas som importeras. Rulla nedåt för**Operations** och klicka sedan på **Import/Export** historik.

### <a name="monitor-hello-progress-of-an-import-operation"></a>Övervaka hello förloppet för en importåtgärd

toomonitor hello fortskrider hello importåtgärden, öppna hello sidan för hello logisk server till vilken hello databasen importeras importeras. Rulla nedåt för**Operations** och klicka sedan på **Import/Export** historik.
   
   ![importera](./media/sql-database-import/import-history.png) ![importera status](./media/sql-database-import/import-status.png)

tooverify hello databasen är aktiv på hello-server klickar du på **SQL-databaser** och kontrollera hello ny databas är **Online**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>Importera från en BACPAC-fil med SQLPackage

tooimport en SQL-databas med hjälp av hello [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) kommandoradsverktyget finns [importera parametrar och egenskaper](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties). Hej SQLPackage verktyget som levereras med hello senaste versionerna av [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) och [SQL Server Data Tools för Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), eller så kan du hämta hello senaste versionen av [ SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) direkt från hello Microsoft hämtningssidan.

Vi rekommenderar hello användningen av hello SQLPackage verktyg för skalning och prestanda i de flesta produktionsmiljöer. Bloggen om att migrera med BACPAC filer finns i en SQL Server Customer Advisory Team [migrera från SQL Server-tooAzure SQL-databas med hjälp av BACPAC filer](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Se hello följande SQLPackage kommando för ett exempel på skript för hur tooimport hello **AdventureWorks2008R2** databasen från lokal lagring tooan Azure SQL Database logiska server, kallas **mynewserver20170403** i det här exemplet. Det här skriptet visar hello skapandet av en ny databas kallas **myMigratedDatabase**, med en tjänstnivå för **Premium**, och ett Tjänstmål för **P6**. Ändra dessa värden som lämpliga tooyour miljö.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

   ![sqlpackage import](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> En logisk Azure SQL Database-server avlyssnar port 1433. Om du försöker tooconnect tooan Azure SQL Database logiska server från en företagsbrandvägg måste den här porten öppnas i hello företagets brandvägg för du toosuccessfully ansluta.
>

Det här exemplet illustrerar hur tooimport en databas med hjälp av SqlPackage.exe med Active Directory Universal autentisering:

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a>Importera från en BACPAC-fil med hjälp av PowerShell

Använd hello [ny AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) cmdlet toosubmit en importera databasen begäran toohello Azure SQL Database-tjänsten. Hello importen kan ta viss tid toocomplete beroende på hello storleken på databasen.

 ```powershell
 $importRequest = New-AzureRmSqlDatabaseImport -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName "MyImportSample" `
    -DatabaseMaxSizeBytes "262144000" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzureRmStorageAccountKey -ResourceGroupName "myResourceGroup" -StorageAccountName $storageaccountname).Value[0] `
    -StorageUri "http://$storageaccountname.blob.core.windows.net/importsample/sample.bacpac" `
    -Edition "Standard" `
    -ServiceObjectiveName "P6" `
    -AdministratorLogin "ServerAdmin" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String "ASecureP@assw0rd" -AsPlainText -Force)

 ```

toocheck hello status för hello importera begäran använder hello [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet. Kör detta omedelbart efter hello begära vanligtvis returnerar **Status: InProgress**. När du ser **Status: lyckades** hello importen är klar.

```powershell
$importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
[Console]::Write("Importing")
while ($importStatus.Status -eq "InProgress")
{
    $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$importStatus
```

> [!TIP]
Exempel på ett annat skript finns [importera en databas från en BACPAC fil](scripts/sql-database-import-from-bacpac-powershell.md).

## <a name="next-steps"></a>Nästa steg
* toolearn hur tooconnect tooand fråga importerade SQL-databas, finns i [ansluta tooSQL databasen med SQL Server Management Studio och utföra en exempelfråga i T-SQL](sql-database-connect-query-ssms.md).
* Bloggen om att migrera med BACPAC filer finns i en SQL Server Customer Advisory Team [migrera från SQL Server-tooAzure SQL-databas med hjälp av BACPAC filer](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* En beskrivning av hello hela SQL Server-databasen migreringsprocessen, inklusive rekommendationer, se [migrera en SQL-databas med SQL Server-databasen tooAzure](sql-database-cloud-migrate.md).



