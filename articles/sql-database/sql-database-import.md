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
# <a name="import-a-bacpac-file-tooa-new-azure-sql-database"></a><span data-ttu-id="8e964-103">Importera en BACPAC filen tooa ny Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="8e964-103">Import a BACPAC file tooa new Azure SQL Database</span></span>

<span data-ttu-id="8e964-104">När du behöver tooimport en databas från ett arkiv eller när du migrerar från en annan plattform, kan du importera hello databasschemat och data från en [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fil.</span><span class="sxs-lookup"><span data-stu-id="8e964-104">When you need tooimport a database from an archive or when migrating from another platform, you can import hello database schema and data from a [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) file.</span></span> <span data-ttu-id="8e964-105">En BACPAC-fil är en ZIP-fil med filnamnstillägget BACPAC som innehåller hello metadata och data från en SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="8e964-105">A BACPAC file is a ZIP file with an extension of BACPAC containing hello metadata and data from a SQL Server database.</span></span> <span data-ttu-id="8e964-106">En BACPAC-fil kan importeras från Azure blob storage (endast standardlagring) eller från lokal lagring på en lokal plats.</span><span class="sxs-lookup"><span data-stu-id="8e964-106">A BACPAC file can be imported from Azure blob storage (standard storage only) or from local storage in an on-premises location.</span></span> <span data-ttu-id="8e964-107">toomaximize hello importera hastighet, rekommenderar vi att du anger en högre nivå och prestanda servicenivå, till exempel en P6 och sedan skala toodown efter behov efter hello importen är klar.</span><span class="sxs-lookup"><span data-stu-id="8e964-107">toomaximize hello import speed, we recommend that you specify a higher service tier and performance level, such as a P6, and then scale toodown as appropriate after hello import is successful.</span></span> <span data-ttu-id="8e964-108">Dessutom hello databasens kompatibilitetsnivå efter hello import baseras på hello kompatibilitetsnivå hello källdatabasen.</span><span class="sxs-lookup"><span data-stu-id="8e964-108">Also, hello database compatibility level after hello import is based on hello compatibility level of hello source database.</span></span> 

> [!IMPORTANT] 
> <span data-ttu-id="8e964-109">När du migrerar din databas tooAzure SQL-databas, väljer du toooperate hello databas på dess aktuella kompatibilitetsnivån (nivå 100 för hello AdventureWorks2008R2 databasen) eller på en högre nivå.</span><span class="sxs-lookup"><span data-stu-id="8e964-109">After you migrate your database tooAzure SQL Database, you can choose toooperate hello database at its current compatibility level (level 100 for hello AdventureWorks2008R2 database) or at a higher level.</span></span> <span data-ttu-id="8e964-110">Läs mer om hello effekter och alternativ för att driva en databas på en specifik kompatibilitetsnivå [ändra DATABASENS kompatibilitetsnivån](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span><span class="sxs-lookup"><span data-stu-id="8e964-110">For more information on hello implications and options for operating a database at a specific compatibility level, see [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span></span> <span data-ttu-id="8e964-111">Se även [ALTER OMFÅNG DATABASKONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) information om ytterligare databasnivå inställningar relaterade toocompatibility nivåer.</span><span class="sxs-lookup"><span data-stu-id="8e964-111">See also [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) for information about additional database-level settings related toocompatibility levels.</span></span>   >

> [!NOTE]
> <span data-ttu-id="8e964-112">tooimport en BACPAC tooa ny databas, måste du först skapa en logisk Azure SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="8e964-112">tooimport a BACPAC tooa new database, you must first create an Azure SQL Database logical server.</span></span> <span data-ttu-id="8e964-113">En självstudiekurs visar hur toomigrate en SQL Server-databas tooAzure SQL-databas med hjälp av SQLPackage, finns [migrera en SQL Server-databas](sql-database-migrate-your-sql-server-database.md)</span><span class="sxs-lookup"><span data-stu-id="8e964-113">For a tutorial showing you how toomigrate a SQL Server database tooAzure SQL Database using SQLPackage, see [Migrate a SQL Server Database](sql-database-migrate-your-sql-server-database.md)</span></span>
>

## <a name="import-from-a-bacpac-file-using-azure-portal"></a><span data-ttu-id="8e964-114">Importera från en BACPAC-fil med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="8e964-114">Import from a BACPAC file using Azure portal</span></span>

<span data-ttu-id="8e964-115">Den här artikeln innehåller anvisningar för att skapa en Azure SQL-databas från en BACPAC-fil som lagras i Azure blob storage med hjälp av hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8e964-115">This article provides directions for creating an Azure SQL database from a BACPAC file stored in Azure blob storage using hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8e964-116">Import med hjälp av hello Azure portal stöder importera en BACPAC-fil från Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="8e964-116">Import using hello Azure portal only supports importing a BACPAC file from Azure blob storage.</span></span>

<span data-ttu-id="8e964-117">en databas med hjälp av tooimport hello Azure-portalen, öppna hello sidan för din databas och klickar på **importera** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="8e964-117">tooimport a database using hello Azure portal, open hello page for your database and click **Import** on hello toolbar.</span></span> <span data-ttu-id="8e964-118">Ange hello storage-konto och en behållare och markera hello BACPAC tooimport.</span><span class="sxs-lookup"><span data-stu-id="8e964-118">Specify hello storage account and container, and select hello BACPAC file you want tooimport.</span></span> <span data-ttu-id="8e964-119">Välj hello storlek för hello ny databas (vanligtvis hello densamma som originalet) och ange autentiseringsuppgifter för hello mål SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8e964-119">Select hello size of hello new database (usually hello same as origin) and provide hello destination SQL Server credentials.</span></span>  

   ![Databasimport](./media/sql-database-import/import.png)

<span data-ttu-id="8e964-121">toomonitor hello fortskrider hello importåtgärden, öppna hello sidan för hello logisk server som innehåller hello databas som importeras.</span><span class="sxs-lookup"><span data-stu-id="8e964-121">toomonitor hello progress of hello import operation, open hello page for hello logical server containing hello database being imported.</span></span> <span data-ttu-id="8e964-122">Rulla nedåt för**Operations** och klicka sedan på **Import/Export** historik.</span><span class="sxs-lookup"><span data-stu-id="8e964-122">Scroll down too**Operations** and then click **Import/Export** history.</span></span>

### <a name="monitor-hello-progress-of-an-import-operation"></a><span data-ttu-id="8e964-123">Övervaka hello förloppet för en importåtgärd</span><span class="sxs-lookup"><span data-stu-id="8e964-123">Monitor hello progress of an import operation</span></span>

<span data-ttu-id="8e964-124">toomonitor hello fortskrider hello importåtgärden, öppna hello sidan för hello logisk server till vilken hello databasen importeras importeras.</span><span class="sxs-lookup"><span data-stu-id="8e964-124">toomonitor hello progress of hello import operation, open hello page for hello logical server into which hello database is being imported imported.</span></span> <span data-ttu-id="8e964-125">Rulla nedåt för**Operations** och klicka sedan på **Import/Export** historik.</span><span class="sxs-lookup"><span data-stu-id="8e964-125">Scroll down too**Operations** and then click **Import/Export** history.</span></span>
   
   <span data-ttu-id="8e964-126">![importera](./media/sql-database-import/import-history.png) ![importera status](./media/sql-database-import/import-status.png)</span><span class="sxs-lookup"><span data-stu-id="8e964-126">![import](./media/sql-database-import/import-history.png) ![import status](./media/sql-database-import/import-status.png)</span></span>

<span data-ttu-id="8e964-127">tooverify hello databasen är aktiv på hello-server klickar du på **SQL-databaser** och kontrollera hello ny databas är **Online**.</span><span class="sxs-lookup"><span data-stu-id="8e964-127">tooverify hello database is live on hello server, click **SQL databases** and verify hello new database is **Online**.</span></span>

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a><span data-ttu-id="8e964-128">Importera från en BACPAC-fil med SQLPackage</span><span class="sxs-lookup"><span data-stu-id="8e964-128">Import from a BACPAC file using SQLPackage</span></span>

<span data-ttu-id="8e964-129">tooimport en SQL-databas med hjälp av hello [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) kommandoradsverktyget finns [importera parametrar och egenskaper](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties).</span><span class="sxs-lookup"><span data-stu-id="8e964-129">tooimport a SQL database using hello [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) command-line utility, see [Import parameters and properties](https://msdn.microsoft.com/library/hh550080.aspx#Import Parameters and Properties).</span></span> <span data-ttu-id="8e964-130">Hej SQLPackage verktyget som levereras med hello senaste versionerna av [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) och [SQL Server Data Tools för Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), eller så kan du hämta hello senaste versionen av [ SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) direkt från hello Microsoft hämtningssidan.</span><span class="sxs-lookup"><span data-stu-id="8e964-130">hello SQLPackage utility ships with hello latest versions of [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) and [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), or you can download hello latest version of [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) directly from hello Microsoft download center.</span></span>

<span data-ttu-id="8e964-131">Vi rekommenderar hello användningen av hello SQLPackage verktyg för skalning och prestanda i de flesta produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="8e964-131">We recommend hello use of hello SQLPackage utility for scale and performance in most production environments.</span></span> <span data-ttu-id="8e964-132">Bloggen om att migrera med BACPAC filer finns i en SQL Server Customer Advisory Team [migrera från SQL Server-tooAzure SQL-databas med hjälp av BACPAC filer](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span><span class="sxs-lookup"><span data-stu-id="8e964-132">For a SQL Server Customer Advisory Team blog about migrating using BACPAC files, see [Migrating from SQL Server tooAzure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>

<span data-ttu-id="8e964-133">Se hello följande SQLPackage kommando för ett exempel på skript för hur tooimport hello **AdventureWorks2008R2** databasen från lokal lagring tooan Azure SQL Database logiska server, kallas **mynewserver20170403** i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="8e964-133">See hello following SQLPackage command for a script example for how tooimport hello **AdventureWorks2008R2** database from local storage tooan Azure SQL Database logical server, called **mynewserver20170403** in this example.</span></span> <span data-ttu-id="8e964-134">Det här skriptet visar hello skapandet av en ny databas kallas **myMigratedDatabase**, med en tjänstnivå för **Premium**, och ett Tjänstmål för **P6**.</span><span class="sxs-lookup"><span data-stu-id="8e964-134">This script shows hello creation of a new database called **myMigratedDatabase**, with a service tier of **Premium**, and a Service Objective of **P6**.</span></span> <span data-ttu-id="8e964-135">Ändra dessa värden som lämpliga tooyour miljö.</span><span class="sxs-lookup"><span data-stu-id="8e964-135">Change these values as appropriate tooyour environment.</span></span>

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

   ![sqlpackage import](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> <span data-ttu-id="8e964-137">En logisk Azure SQL Database-server avlyssnar port 1433.</span><span class="sxs-lookup"><span data-stu-id="8e964-137">An Azure SQL Database logical server listens on port 1433.</span></span> <span data-ttu-id="8e964-138">Om du försöker tooconnect tooan Azure SQL Database logiska server från en företagsbrandvägg måste den här porten öppnas i hello företagets brandvägg för du toosuccessfully ansluta.</span><span class="sxs-lookup"><span data-stu-id="8e964-138">If you are attempting tooconnect tooan Azure SQL Database logical server from within a corporate firewall, this port must be open in hello corporate firewall for you toosuccessfully connect.</span></span>
>

<span data-ttu-id="8e964-139">Det här exemplet illustrerar hur tooimport en databas med hjälp av SqlPackage.exe med Active Directory Universal autentisering:</span><span class="sxs-lookup"><span data-stu-id="8e964-139">This example shows how tooimport a database using SqlPackage.exe with Active Directory Universal Authentication:</span></span>

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a><span data-ttu-id="8e964-140">Importera från en BACPAC-fil med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="8e964-140">Import from a BACPAC file using PowerShell</span></span>

<span data-ttu-id="8e964-141">Använd hello [ny AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) cmdlet toosubmit en importera databasen begäran toohello Azure SQL Database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8e964-141">Use hello [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) cmdlet toosubmit an import database request toohello Azure SQL Database service.</span></span> <span data-ttu-id="8e964-142">Hello importen kan ta viss tid toocomplete beroende på hello storleken på databasen.</span><span class="sxs-lookup"><span data-stu-id="8e964-142">Depending on hello size of your database, hello import operation may take some time toocomplete.</span></span>

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

<span data-ttu-id="8e964-143">toocheck hello status för hello importera begäran använder hello [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="8e964-143">toocheck hello status of hello import request, use hello [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet.</span></span> <span data-ttu-id="8e964-144">Kör detta omedelbart efter hello begära vanligtvis returnerar **Status: InProgress**.</span><span class="sxs-lookup"><span data-stu-id="8e964-144">Running this immediately after hello request usually returns **Status: InProgress**.</span></span> <span data-ttu-id="8e964-145">När du ser **Status: lyckades** hello importen är klar.</span><span class="sxs-lookup"><span data-stu-id="8e964-145">When you see **Status: Succeeded** hello import is complete.</span></span>

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
<span data-ttu-id="8e964-146">Exempel på ett annat skript finns [importera en databas från en BACPAC fil](scripts/sql-database-import-from-bacpac-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="8e964-146">For another script example, see [Import a database from a BACPAC file](scripts/sql-database-import-from-bacpac-powershell.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8e964-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8e964-147">Next steps</span></span>
* <span data-ttu-id="8e964-148">toolearn hur tooconnect tooand fråga importerade SQL-databas, finns i [ansluta tooSQL databasen med SQL Server Management Studio och utföra en exempelfråga i T-SQL](sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="8e964-148">toolearn how tooconnect tooand query an imported SQL Database, see [Connect tooSQL Database with SQL Server Management Studio and perform a sample T-SQL query](sql-database-connect-query-ssms.md).</span></span>
* <span data-ttu-id="8e964-149">Bloggen om att migrera med BACPAC filer finns i en SQL Server Customer Advisory Team [migrera från SQL Server-tooAzure SQL-databas med hjälp av BACPAC filer](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span><span class="sxs-lookup"><span data-stu-id="8e964-149">For a SQL Server Customer Advisory Team blog about migrating using BACPAC files, see [Migrating from SQL Server tooAzure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>
* <span data-ttu-id="8e964-150">En beskrivning av hello hela SQL Server-databasen migreringsprocessen, inklusive rekommendationer, se [migrera en SQL-databas med SQL Server-databasen tooAzure](sql-database-cloud-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="8e964-150">For a discussion of hello entire SQL Server database migration process, including performance recommendations, see [Migrate a SQL Server database tooAzure SQL Database](sql-database-cloud-migrate.md).</span></span>



