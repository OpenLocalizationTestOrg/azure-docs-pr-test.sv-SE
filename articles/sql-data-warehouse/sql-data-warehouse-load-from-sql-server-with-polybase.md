---
title: "Läs in data från SQL Server till Azure SQL Data Warehouse (PolyBase) | Microsoft Docs"
description: "Använder sig av bcp för att exportera data från SQL Server till flat-filer, AZCopy för att importera data till Azure blobblagring och PolyBase för att mata in data i Azure SQL Data Warehouse."
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
ms.openlocfilehash: 966100094f98bae41bf90df500d005fa78b31ec3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="load-data-with-polybase-in-sql-data-warehouse"></a><span data-ttu-id="49f36-103">Läs in data med PolyBase i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="49f36-103">Load data with PolyBase in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="49f36-104">SSIS</span><span class="sxs-lookup"><span data-stu-id="49f36-104">SSIS</span></span>](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [<span data-ttu-id="49f36-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="49f36-105">PolyBase</span></span>](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [<span data-ttu-id="49f36-106">bcp</span><span class="sxs-lookup"><span data-stu-id="49f36-106">bcp</span></span>](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

<span data-ttu-id="49f36-107">De här självstudierna visar hur du läser in data i SQL Data Warehouse med hjälp av AzCopy och PolyBase.</span><span class="sxs-lookup"><span data-stu-id="49f36-107">This tutorial shows how to load data into SQL Data Warehouse by using AzCopy and PolyBase.</span></span> <span data-ttu-id="49f36-108">När du är klar, kommer du att veta hur man:</span><span class="sxs-lookup"><span data-stu-id="49f36-108">When finished, you will know how to:</span></span>

* <span data-ttu-id="49f36-109">Använder AzCopy för att kopiera data till Azure-blobblagring</span><span class="sxs-lookup"><span data-stu-id="49f36-109">Use AzCopy to copy data to Azure blob storage</span></span>
* <span data-ttu-id="49f36-110">Skapar databasobjekt för att definiera data</span><span class="sxs-lookup"><span data-stu-id="49f36-110">Create database objects to define the data</span></span>
* <span data-ttu-id="49f36-111">Kör en T-SQL-fråga för att läsa in data</span><span class="sxs-lookup"><span data-stu-id="49f36-111">Run a T-SQL query to load the data</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-with-PolyBase-in-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="49f36-112">Förutsättningar</span><span class="sxs-lookup"><span data-stu-id="49f36-112">Prerequisites</span></span>
<span data-ttu-id="49f36-113">För att gå igenom de här självstudierna behöver du</span><span class="sxs-lookup"><span data-stu-id="49f36-113">To step through this tutorial, you need</span></span>

* <span data-ttu-id="49f36-114">En SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="49f36-114">A SQL Data Warehouse database.</span></span>
* <span data-ttu-id="49f36-115">Ett Azure-lagringskonto av typen standard lokalt redundant lagring (Standard-LRS), standard geo-redundant lagring (Standard-GRS) eller standard geo-redundant lagring med läsbehörighet (Standard-RAGRS).</span><span class="sxs-lookup"><span data-stu-id="49f36-115">An Azure storage account of type Standard Locally Redundant Storage (Standard-LRS), Standard Geo-Redundant Storage (Standard-GRS), or Standard Read-Access Geo-Redundant Storage (Standard-RAGRS).</span></span>
* <span data-ttu-id="49f36-116">Kommandoradsverktyget AzCopy.</span><span class="sxs-lookup"><span data-stu-id="49f36-116">AzCopy Command-Line Utility.</span></span> <span data-ttu-id="49f36-117">Hämta och installera den [senaste versionen av AzCopy][latest version of AzCopy] som installeras med Microsoft Azure Storage-verktygen.</span><span class="sxs-lookup"><span data-stu-id="49f36-117">Download and install the [latest version of AzCopy][latest version of AzCopy] which is installed with the Microsoft Azure Storage Tools.</span></span>
  
    ![Azure Storage-verktyg](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)

## <a name="step-1-add-sample-data-to-azure-blob-storage"></a><span data-ttu-id="49f36-119">Steg 1: Lägg till exempeldata i Azure blobblagret</span><span class="sxs-lookup"><span data-stu-id="49f36-119">Step 1: Add sample data to Azure blob storage</span></span>
<span data-ttu-id="49f36-120">För att kunna läsa in data, behöver vi lägga exempeldata i ett Azure-blobblager.</span><span class="sxs-lookup"><span data-stu-id="49f36-120">In order to load data, we need to put some sample data into an Azure blob storage.</span></span> <span data-ttu-id="49f36-121">I det här steget fyller vi en Azure Storage-blob med exempeldata.</span><span class="sxs-lookup"><span data-stu-id="49f36-121">In this step we populate an Azure Storage blob with sample data.</span></span> <span data-ttu-id="49f36-122">Senare kommer vi att använda PolyBase för att läsa in exempeldatan till din SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="49f36-122">Later, we will use PolyBase to load this sample data into your SQL Data Warehouse database.</span></span>

### <a name="a-prepare-a-sample-text-file"></a><span data-ttu-id="49f36-123">A.</span><span class="sxs-lookup"><span data-stu-id="49f36-123">A.</span></span> <span data-ttu-id="49f36-124">Förbered en exempeltextfil</span><span class="sxs-lookup"><span data-stu-id="49f36-124">Prepare a sample text file</span></span>
<span data-ttu-id="49f36-125">Förbered en exempeltextfil:</span><span class="sxs-lookup"><span data-stu-id="49f36-125">To prepare a sample text file:</span></span>

1. <span data-ttu-id="49f36-126">Öppna anteckningar och kopiera följande datarader i en ny fil.</span><span class="sxs-lookup"><span data-stu-id="49f36-126">Open Notepad and copy the following lines of data into a new file.</span></span> <span data-ttu-id="49f36-127">Spara den på din lokala temp-katalog som %temp%\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="49f36-127">Save this to your local temp directory as %temp%\DimDate2.txt.</span></span>

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

### <a name="b-find-your-blob-service-endpoint"></a><span data-ttu-id="49f36-128">B.</span><span class="sxs-lookup"><span data-stu-id="49f36-128">B.</span></span> <span data-ttu-id="49f36-129">Hitta blobbtjänstens slutpunkt</span><span class="sxs-lookup"><span data-stu-id="49f36-129">Find your blob service endpoint</span></span>
<span data-ttu-id="49f36-130">Så här hittar du blobbtjänstens slutpunkt:</span><span class="sxs-lookup"><span data-stu-id="49f36-130">To find your blob service endpoint:</span></span>

1. <span data-ttu-id="49f36-131">I Azure Portal väljer du **Bläddra** > **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="49f36-131">From the Azure Portal select **Browse** > **Storage Accounts**.</span></span>
2. <span data-ttu-id="49f36-132">Klicka på det lagringskonto som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="49f36-132">Click the storage account you want to use.</span></span>
3. <span data-ttu-id="49f36-133">I bladet Storage-konton klickar du på Blobbar</span><span class="sxs-lookup"><span data-stu-id="49f36-133">In the Storage account blade, click Blobs</span></span>
   
    ![Klicka på Blobbar](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)
4. <span data-ttu-id="49f36-135">Spara URL:en för blobbtjänstens slutpunkt för senare.</span><span class="sxs-lookup"><span data-stu-id="49f36-135">Save your blob service endpoint URL for later.</span></span>
   
    ![Blob-tjänstens slutpunkt](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a><span data-ttu-id="49f36-137">C.</span><span class="sxs-lookup"><span data-stu-id="49f36-137">C.</span></span> <span data-ttu-id="49f36-138">Hitta din Azure-lagringsnyckel</span><span class="sxs-lookup"><span data-stu-id="49f36-138">Find your Azure storage key</span></span>
<span data-ttu-id="49f36-139">Hitta din Azure-lagringsnyckel:</span><span class="sxs-lookup"><span data-stu-id="49f36-139">To find your Azure storage key:</span></span>

1. <span data-ttu-id="49f36-140">I Azure Portal väljer du **Bläddra** > **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="49f36-140">From the Azure Portal, select **Browse** > **Storage Accounts**.</span></span>
2. <span data-ttu-id="49f36-141">Klicka på det lagringskonto som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="49f36-141">Click on the storage account you want to use.</span></span>
3. <span data-ttu-id="49f36-142">Välj **Alla inställningar** > **Åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="49f36-142">Select **All settings** > **Access keys**.</span></span>
4. <span data-ttu-id="49f36-143">Klicka på kopiera-rutan för att kopiera en av dina åtkomstnycklar till urklipp.</span><span class="sxs-lookup"><span data-stu-id="49f36-143">Click the copy box to copy one of your access keys to the clipboard.</span></span>
   
    ![Kopiera Azure-lagringsnyckel](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-the-sample-file-to-azure-blob-storage"></a><span data-ttu-id="49f36-145">D.</span><span class="sxs-lookup"><span data-stu-id="49f36-145">D.</span></span> <span data-ttu-id="49f36-146">Kopiera exempelfilen till Azure-blobblagring</span><span class="sxs-lookup"><span data-stu-id="49f36-146">Copy the sample file to Azure blob storage</span></span>
<span data-ttu-id="49f36-147">Kopiera dina data till Azure-blobblagring:</span><span class="sxs-lookup"><span data-stu-id="49f36-147">To copy your data to Azure blob storage:</span></span>

1. <span data-ttu-id="49f36-148">Öppna en kommandotolk och ändra katalog till installationskatalogen för AzCopy.</span><span class="sxs-lookup"><span data-stu-id="49f36-148">Open a command prompt, and change directories to the AzCopy installation directory.</span></span> <span data-ttu-id="49f36-149">Det här kommandot ändrar till standard-installationskatalogen på en 64-bitars Windows-klient.</span><span class="sxs-lookup"><span data-stu-id="49f36-149">This command changes to the default installation directory on a 64-bit Windows client.</span></span>
   
    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```
2. <span data-ttu-id="49f36-150">Kör följande kommando för att ladda upp filen.</span><span class="sxs-lookup"><span data-stu-id="49f36-150">Run the following command to upload the file.</span></span> <span data-ttu-id="49f36-151">Ange URL:en för blobbtjänstens slutpunkt för <blob service endpoint URL> och nyckeln för Azure-lagringskontot för <azure_storage_account_key>.</span><span class="sxs-lookup"><span data-stu-id="49f36-151">Specify your blob service endpoint URL for <blob service endpoint URL> and your Azure storage account key for <azure_storage_account_key>.</span></span>
   
    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

<span data-ttu-id="49f36-152">Mer information finns i [Kom igång med kommandoradsverktyget AzCopy][latest version of AzCopy].</span><span class="sxs-lookup"><span data-stu-id="49f36-152">See also [Getting Started with the AzCopy Command-Line Utility][latest version of AzCopy].</span></span>

### <a name="e-explore-your-blob-storage-container"></a><span data-ttu-id="49f36-153">E.</span><span class="sxs-lookup"><span data-stu-id="49f36-153">E.</span></span> <span data-ttu-id="49f36-154">Utforska din blobblagringsbehållare</span><span class="sxs-lookup"><span data-stu-id="49f36-154">Explore your blob storage container</span></span>
<span data-ttu-id="49f36-155">Om du vill se filen du laddade upp till blobblagring:</span><span class="sxs-lookup"><span data-stu-id="49f36-155">To see the file you uploaded to blob storage:</span></span>

1. <span data-ttu-id="49f36-156">Gå tillbaka till bladet Blob-tjänst.</span><span class="sxs-lookup"><span data-stu-id="49f36-156">Go back to your Blob service blade.</span></span>
2. <span data-ttu-id="49f36-157">Under Behållare dubbelklickar du på **datacontainer**.</span><span class="sxs-lookup"><span data-stu-id="49f36-157">Under Containers, double-click **datacontainer**.</span></span>
3. <span data-ttu-id="49f36-158">Om du vill följa sökvägen till dina data, klickar du på mappen **datedimension** för att se filen **DimDate2.txt** som du laddat upp.</span><span class="sxs-lookup"><span data-stu-id="49f36-158">To explore the path to your data, click the folder **datedimension** and you will see your uploaded file **DimDate2.txt**.</span></span>
4. <span data-ttu-id="49f36-159">Om du vill visa egenskaper, klickar du på **DimDate2.txt**.</span><span class="sxs-lookup"><span data-stu-id="49f36-159">To view properties, click **DimDate2.txt**.</span></span>
5. <span data-ttu-id="49f36-160">Observera att bladet för Blob-egenskaper låter dig hämta eller ta bort filen.</span><span class="sxs-lookup"><span data-stu-id="49f36-160">Note that in the Blob properties blade, you can download or delete the file.</span></span>
   
    ![Visa Azure-lagringsblobb](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)

## <a name="step-2-create-an-external-table-for-the-sample-data"></a><span data-ttu-id="49f36-162">Steg 2: Skapa en extern tabell för exempeldata</span><span class="sxs-lookup"><span data-stu-id="49f36-162">Step 2: Create an external table for the sample data</span></span>
<span data-ttu-id="49f36-163">I det här avsnittet ska vi skapa en extern tabell som definierar exempeldata.</span><span class="sxs-lookup"><span data-stu-id="49f36-163">In this section we create an external table that defines the sample data.</span></span>

<span data-ttu-id="49f36-164">PolyBase använder sig av externa tabeller för att komma åt data i Azure-blobblagring.</span><span class="sxs-lookup"><span data-stu-id="49f36-164">PolyBase uses external tables to access data in Azure blob storage.</span></span> <span data-ttu-id="49f36-165">Eftersom data inte lagras inom SQL Data Warehouse, hanterar PolyBase autentisering till externa data med hjälp av en databas-omfattande autentisering.</span><span class="sxs-lookup"><span data-stu-id="49f36-165">Since the data is not stored within SQL Data Warehouse, PolyBase handles authentication to the external data by using a database-scoped credential.</span></span>

<span data-ttu-id="49f36-166">Exemplet i det här steget använder de här Transact-SQL-uttrycken för att skapa en extern tabell.</span><span class="sxs-lookup"><span data-stu-id="49f36-166">The example in this step uses these Transact-SQL statements to create an external table.</span></span>

* <span data-ttu-id="49f36-167">[Skapa huvudnyckel (Transact-SQL)][Create Master Key (Transact-SQL)] för att kryptera hemligheten för din databasövergripande autentisering.</span><span class="sxs-lookup"><span data-stu-id="49f36-167">[Create Master Key (Transact-SQL)][Create Master Key (Transact-SQL)] to encrypt the secret of your database scoped credential.</span></span>
* <span data-ttu-id="49f36-168">[Skapa databasomfattande autentisering (Transact-SQL)][Create Database Scoped Credential (Transact-SQL)] och ange autentiseringsinformation för ditt Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="49f36-168">[Create Database Scoped Credential (Transact-SQL)][Create Database Scoped Credential (Transact-SQL)] to specify authentication information for your Azure storage account.</span></span>
* <span data-ttu-id="49f36-169">[Skapa extern datakälla (Transact-SQL)][Create External Data Source (Transact-SQL)] och ange platsen för din Azure-bloblagring.</span><span class="sxs-lookup"><span data-stu-id="49f36-169">[Create External Data Source (Transact-SQL)][Create External Data Source (Transact-SQL)] to specify the location of your Azure blob storage.</span></span>
* <span data-ttu-id="49f36-170">[Skapa externt filformat (Transact-SQL)][Create External File Format (Transact-SQL)] och ange formatet för dina data.</span><span class="sxs-lookup"><span data-stu-id="49f36-170">[Create External File Format (Transact-SQL)][Create External File Format (Transact-SQL)] to specify the format of your data.</span></span>
* <span data-ttu-id="49f36-171">[Skapa extern tabell (Transact-SQL)][Create External Table (Transact-SQL)] och ange tabelldefinitionen och platsen för dina data.</span><span class="sxs-lookup"><span data-stu-id="49f36-171">[Create External Table (Transact-SQL)][Create External Table (Transact-SQL)] to specify the table definition and location of the data.</span></span>

<span data-ttu-id="49f36-172">Kör den här frågan mot din SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="49f36-172">Run this query against your SQL Data Warehouse database.</span></span> <span data-ttu-id="49f36-173">Det skapar en extern tabell som heter DimDate2External i dbo-schemat, som pekar på exempeldatan DimDate2.txt i Azure-blobblagret.</span><span class="sxs-lookup"><span data-stu-id="49f36-173">It will create an external table named DimDate2External in the dbo schema that points to the DimDate2.txt sample data in the Azure blob storage.</span></span>

```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

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


-- E: Create the external table
-- Specify column names and data types. This needs to match the data in the sample file.
-- LOCATION: Specify path to file or directory that contains the data (relative to the blob container).
-- To point to all files under the blob container, use LOCATION='.'

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


-- Run a query on the external table

SELECT count(*) FROM dbo.DimDate2External;

```


<span data-ttu-id="49f36-174">I SQL Server Object Explorer i Visual Studio, kan du se det externa filformatet, den externa datakällan och DimDate2External-tabellen.</span><span class="sxs-lookup"><span data-stu-id="49f36-174">In SQL Server Object Explorer in Visual Studio, you can see the external file format, external data source, and the DimDate2External table.</span></span>

![Visa extern tabell](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a><span data-ttu-id="49f36-176">Steg 3: Läs in data till SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="49f36-176">Step 3: Load data into SQL Data Warehouse</span></span>
<span data-ttu-id="49f36-177">När den externa tabellen har skapats kan du antingen läsa in dina data till en ny tabell eller infoga dem i en befintlig tabell.</span><span class="sxs-lookup"><span data-stu-id="49f36-177">Once the external table is created, you can either load the data into a new table or insert it into an existing table.</span></span>

* <span data-ttu-id="49f36-178">Om du vill läsa in data till en ny tabell kör du uttrycket [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].</span><span class="sxs-lookup"><span data-stu-id="49f36-178">To load the data into a new table, run the [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] statement.</span></span> <span data-ttu-id="49f36-179">Den nya tabellen kommer att ha kolumnerna som namnges i frågan.</span><span class="sxs-lookup"><span data-stu-id="49f36-179">The new table will have the columns named in the query.</span></span> <span data-ttu-id="49f36-180">Datatyperna för kolumnerna kommer att matcha datatyperna i den externa tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="49f36-180">The data types of the columns will match the data types in the external table definition.</span></span>
* <span data-ttu-id="49f36-181">Om du vill läsa in data i en befintlig tabell använder du uttrycket [INSERT...SELECT (Transact-SQL)][INSERT...SELECT (Transact-SQL)].</span><span class="sxs-lookup"><span data-stu-id="49f36-181">To load the data into an existing table, use the [INSERT...SELECT (Transact-SQL)][INSERT...SELECT (Transact-SQL)] statement.</span></span>

```sql
-- Load the data from Azure blob storage to SQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a><span data-ttu-id="49f36-182">Steg 4: Skapa statistik på dina nyinlästa data</span><span class="sxs-lookup"><span data-stu-id="49f36-182">Step 4: Create statistics on your newly loaded data</span></span>
<span data-ttu-id="49f36-183">SQL Data Warehouse skapar och uppdaterar inte statistik automatiskt.</span><span class="sxs-lookup"><span data-stu-id="49f36-183">SQL Data Warehouse does not auto-create or auto-update statistics.</span></span> <span data-ttu-id="49f36-184">För att få en hög frågeprestanda är det därför viktigt att skapa statistik för varje kolumn av varje tabell efter den första inläsningen.</span><span class="sxs-lookup"><span data-stu-id="49f36-184">Therefore, to achieve high query performance, it's important to create statistics on each column of each table after the first load.</span></span> <span data-ttu-id="49f36-185">Det är också viktigt att uppdatera statistiken efter att det har skett betydande förändringar.</span><span class="sxs-lookup"><span data-stu-id="49f36-185">It's also important to update statistics after substantial changes in the data.</span></span>

<span data-ttu-id="49f36-186">Det här exemplet skapar enkolumns-statistik för den nya DimDate2-tabellen.</span><span class="sxs-lookup"><span data-stu-id="49f36-186">This example creates single-column statistics on the new DimDate2 table.</span></span>

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

<span data-ttu-id="49f36-187">Mer information finns i [Statistik][Statistics].</span><span class="sxs-lookup"><span data-stu-id="49f36-187">To learn more, see [Statistics][Statistics].</span></span>  

## <a name="next-steps"></a><span data-ttu-id="49f36-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49f36-188">Next steps</span></span>
<span data-ttu-id="49f36-189">Se [PolyBase-guiden][PolyBase guide] för ytterligare information som du bör känna till när du utvecklar en PolyBase-baserad lösning.</span><span class="sxs-lookup"><span data-stu-id="49f36-189">See the [PolyBase guide][PolyBase guide] for further information you should know as you develop a solution that uses PolyBase.</span></span>

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
