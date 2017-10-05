---
title: Exportera en Azure SQL database till en BACPAC fil | Microsoft Docs
description: "Exportera en Azure SQL database till en BACPAC-fil med hjälp av Azure-portalen"
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
ms.openlocfilehash: faa567ec615a07da8633629fc98e3454c84a8f5f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="export-an-azure-sql-database-to-a-bacpac-file"></a><span data-ttu-id="b1218-103">Exportera en Azure SQL database till en BACPAC-fil</span><span class="sxs-lookup"><span data-stu-id="b1218-103">Export an Azure SQL database to a BACPAC file</span></span>

<span data-ttu-id="b1218-104">När du vill exportera en databas för arkivering eller för att flytta till en annan plattform du kan exportera databasschemat och data till en [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fil.</span><span class="sxs-lookup"><span data-stu-id="b1218-104">When you need to export a database for archiving or for moving to another platform, you can export the database schema and data to a [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) file.</span></span> <span data-ttu-id="b1218-105">En BACPAC-fil är en ZIP-fil med filnamnstillägget BACPAC som innehåller metadata och data från en SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="b1218-105">A BACPAC file is a ZIP file with an extension of BACPAC containing the metadata and data from a SQL Server database.</span></span> <span data-ttu-id="b1218-106">En BACPAC-fil kan lagras i Azure blob storage eller i lokal lagring på en lokal plats och senare importeras tillbaka till Azure SQL Database eller till en lokal SQL Server-installation.</span><span class="sxs-lookup"><span data-stu-id="b1218-106">A BACPAC file can be stored in Azure blob storage or in local storage in an on-premises location and later imported back into Azure SQL Database or into a SQL Server on-premises installation.</span></span> 

> [!IMPORTANT] 
> <span data-ttu-id="b1218-107">Azure SQL Database automatiserad Export har avslutats på 1 mars 2017.</span><span class="sxs-lookup"><span data-stu-id="b1218-107">Azure SQL Database Automated Export was retired on March 1, 2017.</span></span> <span data-ttu-id="b1218-108">Du kan använda [långsiktig lagring av säkerhetskopior](sql-database-long-term-retention.md
) eller [Azure Automation](https://github.com/Microsoft/azure-docs-pr/blob/2461f706f8fc1150e69312098640c0676206a531/articles/automation/automation-intro.md) regelbundet Arkivera SQL-databaser med hjälp av PowerShell enligt ett schema som du väljer.</span><span class="sxs-lookup"><span data-stu-id="b1218-108">You can use [long-term backup retention](sql-database-long-term-retention.md
) or [Azure Automation](https://github.com/Microsoft/azure-docs-pr/blob/2461f706f8fc1150e69312098640c0676206a531/articles/automation/automation-intro.md) to periodically archive SQL databases using PowerShell according to a schedule of your choice.</span></span> <span data-ttu-id="b1218-109">Ett exempel kan du hämta den [exempel PowerShell-skript](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) från Github.</span><span class="sxs-lookup"><span data-stu-id="b1218-109">For a sample, download the [sample PowerShell script](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export) from Github.</span></span>
>

## <a name="considerations-when-exporting-an-azure-sql-database"></a><span data-ttu-id="b1218-110">Att tänka på när du exporterar en Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="b1218-110">Considerations when exporting an Azure SQL database</span></span>

* <span data-ttu-id="b1218-111">För en export transaktionellt konsekvent, måste du kontrollera antingen att inga skrivåtgärder aktivitet pågår under exporten eller som du exporterar från en [transaktionellt konsekvent kopiera](sql-database-copy.md) för din Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b1218-111">For an export to be transactionally consistent, you must ensure either that no write activity is occurring during the export, or that you are exporting from a [transactionally consistent copy](sql-database-copy.md) of your Azure SQL database.</span></span>
* <span data-ttu-id="b1218-112">Om du exporterar till blob storage är den maximala storleken för en BACPAC fil 200 GB.</span><span class="sxs-lookup"><span data-stu-id="b1218-112">If you are exporting to blob storage, the maximum size of a BACPAC file is 200 GB.</span></span> <span data-ttu-id="b1218-113">Om du vill arkivera en större BACPAC fil att exportera till lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="b1218-113">To archive a larger BACPAC file, export to local storage.</span></span>
* <span data-ttu-id="b1218-114">Exportera en BACPAC-fil till Azure premium-lagring med hjälp av metoderna som beskrivs i den här artikeln stöds inte.</span><span class="sxs-lookup"><span data-stu-id="b1218-114">Exporting a BACPAC file to Azure premium storage using the methods discussed in this article is not supported.</span></span>
* <span data-ttu-id="b1218-115">Om exporten från Azure SQL Database överskrider 20 timmar, kan den avbrytas.</span><span class="sxs-lookup"><span data-stu-id="b1218-115">If the export operation from Azure SQL Database exceeds 20 hours, it may be canceled.</span></span> <span data-ttu-id="b1218-116">Om du vill öka prestandan under exporten kan du:</span><span class="sxs-lookup"><span data-stu-id="b1218-116">To increase performance during export, you can:</span></span>
  * <span data-ttu-id="b1218-117">Tillfälligt öka din servicenivå.</span><span class="sxs-lookup"><span data-stu-id="b1218-117">Temporarily increase your service level.</span></span>
  * <span data-ttu-id="b1218-118">Upphör alla läsa och skriva aktiviteten under exporten.</span><span class="sxs-lookup"><span data-stu-id="b1218-118">Cease all read and write activity during the export.</span></span>
  * <span data-ttu-id="b1218-119">Använd en [grupperat index](https://msdn.microsoft.com/library/ms190457.aspx) med icke-null-värden för alla stora tabeller.</span><span class="sxs-lookup"><span data-stu-id="b1218-119">Use a [clustered index](https://msdn.microsoft.com/library/ms190457.aspx) with non-null values on all large tables.</span></span> <span data-ttu-id="b1218-120">Utan grupperade index, kan en export misslyckas om det tar längre tid än 6 – 12 timmar.</span><span class="sxs-lookup"><span data-stu-id="b1218-120">Without clustered indexes, an export may fail if it takes longer than 6-12 hours.</span></span> <span data-ttu-id="b1218-121">Det beror på att tjänsten export måste utföra en tabellgenomsökning om du vill exportera hela tabellen.</span><span class="sxs-lookup"><span data-stu-id="b1218-121">This is because the export service needs to complete a table scan to try to export entire table.</span></span> <span data-ttu-id="b1218-122">Ett bra sätt att bestämma om dina tabeller är optimerade för export är att köra **DBCC SHOW_STATISTICS** och se till att den *RANGE_HI_KEY* inte är null och dess värde har bra distribution.</span><span class="sxs-lookup"><span data-stu-id="b1218-122">A good way to determine if your tables are optimized for export is to run **DBCC SHOW_STATISTICS** and make sure that the *RANGE_HI_KEY* is not null and its value has good distribution.</span></span> <span data-ttu-id="b1218-123">Mer information finns i [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).</span><span class="sxs-lookup"><span data-stu-id="b1218-123">For details, see [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="b1218-124">BACPACs är inte avsedda att användas för säkerhetskopiering och återställning.</span><span class="sxs-lookup"><span data-stu-id="b1218-124">BACPACs are not intended to be used for backup and restore operations.</span></span> <span data-ttu-id="b1218-125">Azure SQL Database skapar automatiskt säkerhetskopior för varje databas.</span><span class="sxs-lookup"><span data-stu-id="b1218-125">Azure SQL Database automatically creates backups for every user database.</span></span> <span data-ttu-id="b1218-126">Mer information finns i [översikt över verksamhetskontinuitet](sql-database-business-continuity.md) och [SQL databassäkerhetskopieringar](sql-database-automated-backups.md).</span><span class="sxs-lookup"><span data-stu-id="b1218-126">For details, see [Business Continuity Overview](sql-database-business-continuity.md) and [SQL Database backups](sql-database-automated-backups.md).</span></span>  
> 

## <a name="export-to-a-bacpac-file-using-the-azure-portal"></a><span data-ttu-id="b1218-127">Exportera till en BACPAC-fil med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="b1218-127">Export to a BACPAC file using the Azure portal</span></span>

<span data-ttu-id="b1218-128">Så här exporterar du en databas med hjälp av den [Azure-portalen](https://portal.azure.com), öppna sidan för din databas och klickar på **exportera** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="b1218-128">To export a database using the [Azure portal](https://portal.azure.com), open the page for your database and click **Export** on the toolbar.</span></span> <span data-ttu-id="b1218-129">Ange filnamnet BACPAC, ange Azure storage-konto och behållare för export och ange autentiseringsuppgifter för att ansluta till källdatabasen.</span><span class="sxs-lookup"><span data-stu-id="b1218-129">Specify the BACPAC filename, provide the Azure storage account and container for the export, and provide the credentials to connect to the source database.</span></span>  

![Databasexport](./media/sql-database-export/database-export.png)

<span data-ttu-id="b1218-131">Öppna sidan för den logiska servern som innehåller den databas som exporteras för att övervaka förloppet för exporten.</span><span class="sxs-lookup"><span data-stu-id="b1218-131">To monitor the progress of the export operation, open the page for the logical server containing the database being exported.</span></span> <span data-ttu-id="b1218-132">Rulla ned till **Operations** och klicka sedan på **Import/Export** historik.</span><span class="sxs-lookup"><span data-stu-id="b1218-132">Scroll down to **Operations** and then click **Import/Export** history.</span></span>

<span data-ttu-id="b1218-133">![Exportera tidigare](./media/sql-database-export/export-history.png)
![exportstatus historik](./media/sql-database-export/export-history2.png)</span><span class="sxs-lookup"><span data-stu-id="b1218-133">![export history](./media/sql-database-export/export-history.png)
![export history status](./media/sql-database-export/export-history2.png)</span></span>

## <a name="export-to-a-bacpac-file-using-the-sqlpackage-utility"></a><span data-ttu-id="b1218-134">Exportera till en BACPAC-fil med hjälp av verktyget SQLPackage</span><span class="sxs-lookup"><span data-stu-id="b1218-134">Export to a BACPAC file using the SQLPackage utility</span></span>

<span data-ttu-id="b1218-135">Så här exporterar du en SQL-databas med den [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) kommandoradsverktyget finns [exportera parametrar och egenskaper](https://msdn.microsoft.com/library/hh550080.aspx#Export Parameters and Properties).</span><span class="sxs-lookup"><span data-stu-id="b1218-135">To export a SQL database using the [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) command-line utility, see [Export parameters and properties](https://msdn.microsoft.com/library/hh550080.aspx#Export Parameters and Properties).</span></span> <span data-ttu-id="b1218-136">Verktyget SQLPackage levereras med de senaste versionerna av [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) och [SQL Server Data Tools för Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), eller så kan du hämta den senaste versionen av [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) direkt från Microsoft download center.</span><span class="sxs-lookup"><span data-stu-id="b1218-136">The SQLPackage utility ships with the latest versions of [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) and [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), or you can download the latest version of [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) directly from the Microsoft download center.</span></span>

<span data-ttu-id="b1218-137">Vi rekommenderar användning av verktyget SQLPackage för skalning och prestanda i de flesta produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="b1218-137">We recommend the use of the SQLPackage utility for scale and performance in most production environments.</span></span> <span data-ttu-id="b1218-138">En SQL Server Customer Advisory Team-blogg om migrering med BACPAC-filer finns i [Migrera från SQL Server till Azure SQL Database med BACPAC-filer](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (på engelska).</span><span class="sxs-lookup"><span data-stu-id="b1218-138">For a SQL Server Customer Advisory Team blog about migrating using BACPAC files, see [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>

<span data-ttu-id="b1218-139">Det här exemplet visar hur du exporterar en databas med hjälp av SqlPackage.exe med Active Directory Universal autentisering:</span><span class="sxs-lookup"><span data-stu-id="b1218-139">This example shows how to export a database using SqlPackage.exe with Active Directory Universal Authentication:</span></span>

```cmd
SqlPackage.exe /a:Export /tf:testExport.bacpac /scs:"Data Source=apptestserver.database.windows.net;Initial Catalog=MyDB;" /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="export-to-a-bacpac-file-using-sql-server-management-studio-ssms"></a><span data-ttu-id="b1218-140">Exportera till en BACPAC-fil med hjälp av SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="b1218-140">Export to a BACPAC file using SQL Server Management Studio (SSMS)</span></span>

<span data-ttu-id="b1218-141">De senaste versionerna av SQL Server Management Studio ger också en guide för att exportera en Azure SQL Database till en BACPAC-fil.</span><span class="sxs-lookup"><span data-stu-id="b1218-141">The newest versions of SQL Server Management Studio also provide a wizard to export an Azure SQL Database to a BACPAC file.</span></span> <span data-ttu-id="b1218-142">Finns det [exportera en Dataskiktsprogrammet](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application).</span><span class="sxs-lookup"><span data-stu-id="b1218-142">See the [Export a Data-tier Application](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application).</span></span>

## <a name="export-to-a-bacpac-file-using-powershell"></a><span data-ttu-id="b1218-143">Exportera till en BACPAC-fil med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="b1218-143">Export to a BACPAC file using PowerShell</span></span>

<span data-ttu-id="b1218-144">Använd den [ny AzureRmSqlDatabaseExport](/powershell/module/azurerm.sql/new-azurermsqldatabaseexport) för att skicka en begäran om export av databas till Azure SQL Database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b1218-144">Use the [New-AzureRmSqlDatabaseExport](/powershell/module/azurerm.sql/new-azurermsqldatabaseexport) cmdlet to submit an export database request to the Azure SQL Database service.</span></span> <span data-ttu-id="b1218-145">Exportåtgärden kan ta lite tid att slutföra beroende på storleken på databasen.</span><span class="sxs-lookup"><span data-stu-id="b1218-145">Depending on the size of your database, the export operation may take some time to complete.</span></span>

 ```powershell
 $exportRequest = New-AzureRmSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
   -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
   -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password
 ```

<span data-ttu-id="b1218-146">Om du vill kontrollera status för begäran för export av [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b1218-146">To check the status of the export request, use the [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet.</span></span> <span data-ttu-id="b1218-147">Kör detta omedelbart efter begäran vanligtvis returnerar **Status: InProgress**.</span><span class="sxs-lookup"><span data-stu-id="b1218-147">Running this immediately after the request usually returns **Status: InProgress**.</span></span> <span data-ttu-id="b1218-148">När du ser **Status: lyckades** exporten har slutförts.</span><span class="sxs-lookup"><span data-stu-id="b1218-148">When you see **Status: Succeeded** the export is complete.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b1218-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b1218-149">Next steps</span></span>

* <span data-ttu-id="b1218-150">Mer information om långsiktig säkerhetskopiering kvarhållning av en Azure SQL database-säkerhetskopia som ett alternativ till att exportera en databas för finns [långsiktig lagring av säkerhetskopior](sql-database-long-term-retention.md).</span><span class="sxs-lookup"><span data-stu-id="b1218-150">To learn about long-term backup retention of an Azure SQL database backup as an alternative to exported a database for archive purposes, see [Long-term backup retention](sql-database-long-term-retention.md).</span></span>
- <span data-ttu-id="b1218-151">En SQL Server Customer Advisory Team-blogg om migrering med BACPAC-filer finns i [Migrera från SQL Server till Azure SQL Database med BACPAC-filer](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (på engelska).</span><span class="sxs-lookup"><span data-stu-id="b1218-151">For a SQL Server Customer Advisory Team blog about migrating using BACPAC files, see [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>
* <span data-ttu-id="b1218-152">Läs om hur du importerar en BACPAC till en SQL Server-databas i [importera en BACPCAC till en SQL Server-databas](https://msdn.microsoft.com/library/hh710052.aspx).</span><span class="sxs-lookup"><span data-stu-id="b1218-152">To learn about importing a BACPAC to a SQL Server database, see [Import a BACPCAC to a SQL Server database](https://msdn.microsoft.com/library/hh710052.aspx).</span></span>
* <span data-ttu-id="b1218-153">Läs om hur du exporterar en BACPAC från en SQL Server-databas i [exportera en Dataskiktsprogrammet](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) och [migrera din första databas](sql-database-migrate-your-sql-server-database.md).</span><span class="sxs-lookup"><span data-stu-id="b1218-153">To learn about exporting a BACPAC from a SQL Server database, see [Export a Data-tier Application](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) and [Migrate your first database](sql-database-migrate-your-sql-server-database.md).</span></span>
* <span data-ttu-id="b1218-154">Om du exporterar från SQL Server som en prelude för migrering till Azure SQL Database, se [migrera en SQL Server-databas till Azure SQL Database](sql-database-cloud-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="b1218-154">If you are exporting from SQL Server as a prelude to migration to Azure SQL Database, see [Migrate a SQL Server database to Azure SQL Database](sql-database-cloud-migrate.md).</span></span>
