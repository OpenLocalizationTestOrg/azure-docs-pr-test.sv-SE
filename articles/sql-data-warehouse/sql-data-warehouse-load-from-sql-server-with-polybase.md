---
title: "aaaLoad data från SQL Server till Azure SQL Data Warehouse (PolyBase) | Microsoft Docs"
description: "Använder bcp tooexport data från SQL Server tooflat filer, AZCopy tooimport data tooAzure blobblagring och PolyBase tooingest hello data till Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 860c86e0-90f7-492c-9a84-1bdd3d1735cd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 1346fb016e0538a44426671bf4e29358cb24f7ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-polybase-in-sql-data-warehouse"></a><span data-ttu-id="7f939-103">Läs in data med PolyBase i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="7f939-103">Load data with PolyBase in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7f939-104">SSIS</span><span class="sxs-lookup"><span data-stu-id="7f939-104">SSIS</span></span>](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [<span data-ttu-id="7f939-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="7f939-105">PolyBase</span></span>](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [<span data-ttu-id="7f939-106">bcp</span><span class="sxs-lookup"><span data-stu-id="7f939-106">bcp</span></span>](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

<span data-ttu-id="7f939-107">Den här kursen visar hur tooload data till SQL Data Warehouse med hjälp av AzCopy och PolyBase.</span><span class="sxs-lookup"><span data-stu-id="7f939-107">This tutorial shows how tooload data into SQL Data Warehouse by using AzCopy and PolyBase.</span></span> <span data-ttu-id="7f939-108">När du är klar, kommer du att veta hur man:</span><span class="sxs-lookup"><span data-stu-id="7f939-108">When finished, you will know how to:</span></span>

* <span data-ttu-id="7f939-109">Använda AzCopy toocopy data tooAzure blob storage</span><span class="sxs-lookup"><span data-stu-id="7f939-109">Use AzCopy toocopy data tooAzure blob storage</span></span>
* <span data-ttu-id="7f939-110">Skapa databasobjekt toodefine hello data</span><span class="sxs-lookup"><span data-stu-id="7f939-110">Create database objects toodefine hello data</span></span>
* <span data-ttu-id="7f939-111">Kör en T-SQL-fråga tooload hello data</span><span class="sxs-lookup"><span data-stu-id="7f939-111">Run a T-SQL query tooload hello data</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-with-PolyBase-in-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="7f939-112">Krav</span><span class="sxs-lookup"><span data-stu-id="7f939-112">Prerequisites</span></span>
<span data-ttu-id="7f939-113">toostep igenom den här kursen behöver du</span><span class="sxs-lookup"><span data-stu-id="7f939-113">toostep through this tutorial, you need</span></span>

* <span data-ttu-id="7f939-114">En SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="7f939-114">A SQL Data Warehouse database.</span></span>
* <span data-ttu-id="7f939-115">Ett Azure-lagringskonto av typen standard lokalt redundant lagring (Standard-LRS), standard geo-redundant lagring (Standard-GRS) eller standard geo-redundant lagring med läsbehörighet (Standard-RAGRS).</span><span class="sxs-lookup"><span data-stu-id="7f939-115">An Azure storage account of type Standard Locally Redundant Storage (Standard-LRS), Standard Geo-Redundant Storage (Standard-GRS), or Standard Read-Access Geo-Redundant Storage (Standard-RAGRS).</span></span>
* <span data-ttu-id="7f939-116">Kommandoradsverktyget AzCopy.</span><span class="sxs-lookup"><span data-stu-id="7f939-116">AzCopy Command-Line Utility.</span></span> <span data-ttu-id="7f939-117">Hämta och installera hello [senaste versionen av AzCopy] [ latest version of AzCopy] som installeras med hello Microsoft Azure Storage-verktyg.</span><span class="sxs-lookup"><span data-stu-id="7f939-117">Download and install hello [latest version of AzCopy][latest version of AzCopy] which is installed with hello Microsoft Azure Storage Tools.</span></span>
  
    ![Azure Storage-verktyg](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)

## <a name="step-1-add-sample-data-tooazure-blob-storage"></a><span data-ttu-id="7f939-119">Steg 1: Lägg till exempel data tooAzure blob-lagring</span><span class="sxs-lookup"><span data-stu-id="7f939-119">Step 1: Add sample data tooAzure blob storage</span></span>
<span data-ttu-id="7f939-120">I ordning tooload data måste tooput exempeldata i en Azure-blobblagring.</span><span class="sxs-lookup"><span data-stu-id="7f939-120">In order tooload data, we need tooput some sample data into an Azure blob storage.</span></span> <span data-ttu-id="7f939-121">I det här steget fyller vi en Azure Storage-blob med exempeldata.</span><span class="sxs-lookup"><span data-stu-id="7f939-121">In this step we populate an Azure Storage blob with sample data.</span></span> <span data-ttu-id="7f939-122">Senare, ska vi använda PolyBase tooload exempeldatan till din SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="7f939-122">Later, we will use PolyBase tooload this sample data into your SQL Data Warehouse database.</span></span>

### <a name="a-prepare-a-sample-text-file"></a><span data-ttu-id="7f939-123">A.</span><span class="sxs-lookup"><span data-stu-id="7f939-123">A.</span></span> <span data-ttu-id="7f939-124">Förbered en exempeltextfil</span><span class="sxs-lookup"><span data-stu-id="7f939-124">Prepare a sample text file</span></span>
<span data-ttu-id="7f939-125">tooprepare en exempeltextfil:</span><span class="sxs-lookup"><span data-stu-id="7f939-125">tooprepare a sample text file:</span></span>

1. <span data-ttu-id="7f939-126">Öppna Anteckningar och kopiera hello följande rader med data i en ny fil.</span><span class="sxs-lookup"><span data-stu-id="7f939-126">Open Notepad and copy hello following lines of data into a new file.</span></span> <span data-ttu-id="7f939-127">Spara den här tooyour lokala temp-katalog som % temp%\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="7f939-127">Save this tooyour local temp directory as %temp%\DimDate2.txt.</span></span>

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="b-find-your-blob-service-endpoint"></a><span data-ttu-id="7f939-128">B.</span><span class="sxs-lookup"><span data-stu-id="7f939-128">B.</span></span> <span data-ttu-id="7f939-129">Hitta blobbtjänstens slutpunkt</span><span class="sxs-lookup"><span data-stu-id="7f939-129">Find your blob service endpoint</span></span>
<span data-ttu-id="7f939-130">toofind blobbtjänstens slutpunkt:</span><span class="sxs-lookup"><span data-stu-id="7f939-130">toofind your blob service endpoint:</span></span>

1. <span data-ttu-id="7f939-131">Välj hello Azure Portal **Bläddra** > **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="7f939-131">From hello Azure Portal select **Browse** > **Storage Accounts**.</span></span>
2. <span data-ttu-id="7f939-132">Klicka på hello storage-konto som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="7f939-132">Click hello storage account you want toouse.</span></span>
3. <span data-ttu-id="7f939-133">I hello lagring-kontoblad klickar du på Blobbar</span><span class="sxs-lookup"><span data-stu-id="7f939-133">In hello Storage account blade, click Blobs</span></span>
   
    ![Klicka på Blobbar](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)
4. <span data-ttu-id="7f939-135">Spara URL:en för blobbtjänstens slutpunkt för senare.</span><span class="sxs-lookup"><span data-stu-id="7f939-135">Save your blob service endpoint URL for later.</span></span>
   
    ![Blob-tjänstens slutpunkt](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a><span data-ttu-id="7f939-137">C.</span><span class="sxs-lookup"><span data-stu-id="7f939-137">C.</span></span> <span data-ttu-id="7f939-138">Hitta din Azure-lagringsnyckel</span><span class="sxs-lookup"><span data-stu-id="7f939-138">Find your Azure storage key</span></span>
<span data-ttu-id="7f939-139">toofind Azure-lagringsnyckel:</span><span class="sxs-lookup"><span data-stu-id="7f939-139">toofind your Azure storage key:</span></span>

1. <span data-ttu-id="7f939-140">Hello Azure Portal, Välj **Bläddra** > **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="7f939-140">From hello Azure Portal, select **Browse** > **Storage Accounts**.</span></span>
2. <span data-ttu-id="7f939-141">Klicka på hello storage-konto som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="7f939-141">Click on hello storage account you want toouse.</span></span>
3. <span data-ttu-id="7f939-142">Välj **Alla inställningar** > **Åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="7f939-142">Select **All settings** > **Access keys**.</span></span>
4. <span data-ttu-id="7f939-143">Klicka på hello kopiera rutan toocopy en access nycklar toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="7f939-143">Click hello copy box toocopy one of your access keys toohello clipboard.</span></span>
   
    ![Kopiera Azure-lagringsnyckel](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-hello-sample-file-tooazure-blob-storage"></a><span data-ttu-id="7f939-145">D.</span><span class="sxs-lookup"><span data-stu-id="7f939-145">D.</span></span> <span data-ttu-id="7f939-146">Kopiera hello exempel filen tooAzure blob-lagring</span><span class="sxs-lookup"><span data-stu-id="7f939-146">Copy hello sample file tooAzure blob storage</span></span>
<span data-ttu-id="7f939-147">toocopy datalagringen tooAzure blob:</span><span class="sxs-lookup"><span data-stu-id="7f939-147">toocopy your data tooAzure blob storage:</span></span>

1. <span data-ttu-id="7f939-148">Öppna en kommandotolk och ändra kataloger toohello installationskatalogen för AzCopy.</span><span class="sxs-lookup"><span data-stu-id="7f939-148">Open a command prompt, and change directories toohello AzCopy installation directory.</span></span> <span data-ttu-id="7f939-149">Det här kommandot ändrar toohello Standardinstallationskatalogen på en 64-bitars Windows-klient.</span><span class="sxs-lookup"><span data-stu-id="7f939-149">This command changes toohello default installation directory on a 64-bit Windows client.</span></span>
   
    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```
2. <span data-ttu-id="7f939-150">Kör följande kommando tooupload hello hello.</span><span class="sxs-lookup"><span data-stu-id="7f939-150">Run hello following command tooupload hello file.</span></span> <span data-ttu-id="7f939-151">Ange URL:en för blobbtjänstens slutpunkt för <blob service endpoint URL> och nyckeln för Azure-lagringskontot för <azure_storage_account_key>.</span><span class="sxs-lookup"><span data-stu-id="7f939-151">Specify your blob service endpoint URL for <blob service endpoint URL> and your Azure storage account key for <azure_storage_account_key>.</span></span>
   
    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

<span data-ttu-id="7f939-152">Se även [komma igång med kommandoradsverktyget Azcopy hello][latest version of AzCopy].</span><span class="sxs-lookup"><span data-stu-id="7f939-152">See also [Getting Started with hello AzCopy Command-Line Utility][latest version of AzCopy].</span></span>

### <a name="e-explore-your-blob-storage-container"></a><span data-ttu-id="7f939-153">E.</span><span class="sxs-lookup"><span data-stu-id="7f939-153">E.</span></span> <span data-ttu-id="7f939-154">Utforska din blobblagringsbehållare</span><span class="sxs-lookup"><span data-stu-id="7f939-154">Explore your blob storage container</span></span>
<span data-ttu-id="7f939-155">toosee hello filen du överförde tooblob lagring:</span><span class="sxs-lookup"><span data-stu-id="7f939-155">toosee hello file you uploaded tooblob storage:</span></span>

1. <span data-ttu-id="7f939-156">Gå tillbaka tooyour Blob-tjänsten bladet.</span><span class="sxs-lookup"><span data-stu-id="7f939-156">Go back tooyour Blob service blade.</span></span>
2. <span data-ttu-id="7f939-157">Under Behållare dubbelklickar du på **datacontainer**.</span><span class="sxs-lookup"><span data-stu-id="7f939-157">Under Containers, double-click **datacontainer**.</span></span>
3. <span data-ttu-id="7f939-158">tooexplore hello sökvägen tooyour data, klickar du på hello mapp **datedimension** och du ser den överförda filen **DimDate2.txt**.</span><span class="sxs-lookup"><span data-stu-id="7f939-158">tooexplore hello path tooyour data, click hello folder **datedimension** and you will see your uploaded file **DimDate2.txt**.</span></span>
4. <span data-ttu-id="7f939-159">tooview egenskaper, klicka på **DimDate2.txt**.</span><span class="sxs-lookup"><span data-stu-id="7f939-159">tooview properties, click **DimDate2.txt**.</span></span>
5. <span data-ttu-id="7f939-160">Observera att i egenskapsbladet för hello Blob, du kan hämta eller ta bort hello-filen.</span><span class="sxs-lookup"><span data-stu-id="7f939-160">Note that in hello Blob properties blade, you can download or delete hello file.</span></span>
   
    ![Visa Azure-lagringsblobb](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)

## <a name="step-2-create-an-external-table-for-hello-sample-data"></a><span data-ttu-id="7f939-162">Steg 2: Skapa en extern tabell för exempeldata hello</span><span class="sxs-lookup"><span data-stu-id="7f939-162">Step 2: Create an external table for hello sample data</span></span>
<span data-ttu-id="7f939-163">I det här avsnittet skapar vi en extern tabell som definierar hello exempeldata.</span><span class="sxs-lookup"><span data-stu-id="7f939-163">In this section we create an external table that defines hello sample data.</span></span>

<span data-ttu-id="7f939-164">PolyBase använder externa tabeller tooaccess data i Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="7f939-164">PolyBase uses external tables tooaccess data in Azure blob storage.</span></span> <span data-ttu-id="7f939-165">Eftersom hello data inte lagras inom SQL Data Warehouse, hanterar PolyBase autentisering toohello externa data med hjälp av en databas-omfattande autentisering.</span><span class="sxs-lookup"><span data-stu-id="7f939-165">Since hello data is not stored within SQL Data Warehouse, PolyBase handles authentication toohello external data by using a database-scoped credential.</span></span>

<span data-ttu-id="7f939-166">hello exemplet i det här steget använder de här Transact-SQL-instruktioner toocreate en extern tabell.</span><span class="sxs-lookup"><span data-stu-id="7f939-166">hello example in this step uses these Transact-SQL statements toocreate an external table.</span></span>

* <span data-ttu-id="7f939-167">[Skapa huvudnyckel (Transact-SQL)] [ Create Master Key (Transact-SQL)] tooencrypt hello hemligheten för din databas-omfattande autentisering.</span><span class="sxs-lookup"><span data-stu-id="7f939-167">[Create Master Key (Transact-SQL)][Create Master Key (Transact-SQL)] tooencrypt hello secret of your database scoped credential.</span></span>
* <span data-ttu-id="7f939-168">[Skapa Databasomfattande autentisering (Transact-SQL)] [ Create Database Scoped Credential (Transact-SQL)] toospecify autentiseringsinformation för ditt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7f939-168">[Create Database Scoped Credential (Transact-SQL)][Create Database Scoped Credential (Transact-SQL)] toospecify authentication information for your Azure storage account.</span></span>
* <span data-ttu-id="7f939-169">[Skapa extern datakälla (Transact-SQL)] [ Create External Data Source (Transact-SQL)] toospecify hello platsen för din Azure-blobblagring.</span><span class="sxs-lookup"><span data-stu-id="7f939-169">[Create External Data Source (Transact-SQL)][Create External Data Source (Transact-SQL)] toospecify hello location of your Azure blob storage.</span></span>
* <span data-ttu-id="7f939-170">[Skapa externt filformat (Transact-SQL)] [ Create External File Format (Transact-SQL)] toospecify hello formatet för dina data.</span><span class="sxs-lookup"><span data-stu-id="7f939-170">[Create External File Format (Transact-SQL)][Create External File Format (Transact-SQL)] toospecify hello format of your data.</span></span>
* <span data-ttu-id="7f939-171">[Skapa extern tabell (Transact-SQL)] [ Create External Table (Transact-SQL)] toospecify hello tabelldefinitionen och platsen för hello data.</span><span class="sxs-lookup"><span data-stu-id="7f939-171">[Create External Table (Transact-SQL)][Create External Table (Transact-SQL)] toospecify hello table definition and location of hello data.</span></span>

<span data-ttu-id="7f939-172">Kör den här frågan mot din SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="7f939-172">Run this query against your SQL Data Warehouse database.</span></span> <span data-ttu-id="7f939-173">Den skapar en extern tabell som heter DimDate2External i dbo-schemat, hello som pekar toohello Exempeldatan dimdate2.txt i hello Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="7f939-173">It will create an external table named DimDate2External in hello dbo schema that points toohello DimDate2.txt sample data in hello Azure blob storage.</span></span>

```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required tooencrypt hello credential secret in hello next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication tooAzure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs tooaccess data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide hello credential created in hello previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create hello external table
-- Specify column names and data types. This needs toomatch hello data in hello sample file.
-- LOCATION: Specify path toofile or directory that contains hello data (relative toohello blob container).
-- toopoint tooall files under hello blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on hello external table

SELECT count(*) FROM dbo.DimDate2External;

```


<span data-ttu-id="7f939-174">I SQL Server Object Explorer i Visual Studio, kan du se hello externa filformatet, extern datakälla och hello DimDate2External-tabellen.</span><span class="sxs-lookup"><span data-stu-id="7f939-174">In SQL Server Object Explorer in Visual Studio, you can see hello external file format, external data source, and hello DimDate2External table.</span></span>

![Visa extern tabell](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a><span data-ttu-id="7f939-176">Steg 3: Läs in data till SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="7f939-176">Step 3: Load data into SQL Data Warehouse</span></span>
<span data-ttu-id="7f939-177">När hello extern tabell har skapats kan du läsa hello data i en ny tabell eller infoga den i en befintlig tabell.</span><span class="sxs-lookup"><span data-stu-id="7f939-177">Once hello external table is created, you can either load hello data into a new table or insert it into an existing table.</span></span>

* <span data-ttu-id="7f939-178">tooload hello data i en ny tabell, kör hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] instruktionen.</span><span class="sxs-lookup"><span data-stu-id="7f939-178">tooload hello data into a new table, run hello [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] statement.</span></span> <span data-ttu-id="7f939-179">hello nya tabellen kommer att ha hello kolumnerna som namnges i frågan hello.</span><span class="sxs-lookup"><span data-stu-id="7f939-179">hello new table will have hello columns named in hello query.</span></span> <span data-ttu-id="7f939-180">hello datatyper hello kolumner matchar hello datatyper i hello externa tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="7f939-180">hello data types of hello columns will match hello data types in hello external table definition.</span></span>
* <span data-ttu-id="7f939-181">tooload hello data i en befintlig tabell använder hello [INSERT... Välj (Transact-SQL)] [ INSERT...SELECT (Transact-SQL)] instruktionen.</span><span class="sxs-lookup"><span data-stu-id="7f939-181">tooload hello data into an existing table, use hello [INSERT...SELECT (Transact-SQL)][INSERT...SELECT (Transact-SQL)] statement.</span></span>

```sql
-- Load hello data from Azure blob storage tooSQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a><span data-ttu-id="7f939-182">Steg 4: Skapa statistik på dina nyinlästa data</span><span class="sxs-lookup"><span data-stu-id="7f939-182">Step 4: Create statistics on your newly loaded data</span></span>
<span data-ttu-id="7f939-183">SQL Data Warehouse skapar och uppdaterar inte statistik automatiskt.</span><span class="sxs-lookup"><span data-stu-id="7f939-183">SQL Data Warehouse does not auto-create or auto-update statistics.</span></span> <span data-ttu-id="7f939-184">Därför tooachieve hög frågeprestanda, det är viktigt toocreate statistik för varje kolumn av varje tabell efter hello först hämta.</span><span class="sxs-lookup"><span data-stu-id="7f939-184">Therefore, tooachieve high query performance, it's important toocreate statistics on each column of each table after hello first load.</span></span> <span data-ttu-id="7f939-185">Det är också viktigt tooupdate statistik efter betydande förändringar i hello data.</span><span class="sxs-lookup"><span data-stu-id="7f939-185">It's also important tooupdate statistics after substantial changes in hello data.</span></span>

<span data-ttu-id="7f939-186">Det här exemplet skapar enkolumns-statistik på hello nya DimDate2-tabellen.</span><span class="sxs-lookup"><span data-stu-id="7f939-186">This example creates single-column statistics on hello new DimDate2 table.</span></span>

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

<span data-ttu-id="7f939-187">Det finns fler toolearn [statistik][Statistics].</span><span class="sxs-lookup"><span data-stu-id="7f939-187">toolearn more, see [Statistics][Statistics].</span></span>  

## <a name="next-steps"></a><span data-ttu-id="7f939-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7f939-188">Next steps</span></span>
<span data-ttu-id="7f939-189">Se hello [PolyBase-guiden] [ PolyBase guide] för ytterligare information som du bör känna till när du utvecklar en lösning som använder PolyBase.</span><span class="sxs-lookup"><span data-stu-id="7f939-189">See hello [PolyBase guide][PolyBase guide] for further information you should know as you develop a solution that uses PolyBase.</span></span>

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[PolyBase guide]: ./sql-data-warehouse-load-polybase-guide.md
[latest version of AzCopy]:../storage/common/storage-use-azcopy.md

<!--External references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/dn935026.aspx
[CREATE EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[CREATE TABLE AS SELECT (Transact-SQL)]:https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]:https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]:https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx
