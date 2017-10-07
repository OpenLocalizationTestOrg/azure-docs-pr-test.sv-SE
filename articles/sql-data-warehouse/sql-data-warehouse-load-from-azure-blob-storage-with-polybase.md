---
title: "aaaLoad från Azure blob tooAzure data warehouse | Microsoft Docs"
description: "Lär dig hur toouse PolyBase tooload data från Azure blob storage till SQL Data Warehouse. Läsa in några tabeller från offentliga data till hello Contoso Retail-datalagret schemat."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: faca0fe7-62e7-4e1f-a86f-032b4ffcb06e
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 4b4978ccefa4d55ff5c89fba84c5e705422ddbb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a><span data-ttu-id="87b51-104">Läs in data från Azure blob storage till SQL Data Warehouse (PolyBase)</span><span class="sxs-lookup"><span data-stu-id="87b51-104">Load data from Azure blob storage into SQL Data Warehouse (PolyBase)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="87b51-105">Data Factory</span><span class="sxs-lookup"><span data-stu-id="87b51-105">Data Factory</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [<span data-ttu-id="87b51-106">PolyBase</span><span class="sxs-lookup"><span data-stu-id="87b51-106">PolyBase</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

<span data-ttu-id="87b51-107">Använd PolyBase och T-SQL-kommandon tooload data från Azure blobblagring till Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="87b51-107">Use PolyBase and T-SQL commands tooload data from Azure blob storage into Azure SQL Data Warehouse.</span></span> 

<span data-ttu-id="87b51-108">tookeep enkel, den här självstudiekursen laddas två tabeller från en offentlig Azure Storage Blob till hello Contoso Retail-datalagret schemat.</span><span class="sxs-lookup"><span data-stu-id="87b51-108">tookeep it simple, this tutorial loads two tables from a public Azure Storage Blob into hello Contoso Retail Data Warehouse schema.</span></span> <span data-ttu-id="87b51-109">tooload hello fullständig datauppsättning, köra exemplet hello [belastningen hello fullständiga Retail-datalagret för Contoso] [ Load hello full Contoso Retail Data Warehouse] från hello exempel för Microsoft SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="87b51-109">tooload hello full data set, run hello example [Load hello full Contoso Retail Data Warehouse][Load hello full Contoso Retail Data Warehouse] from hello Microsoft SQL Server Samples repository.</span></span>

<span data-ttu-id="87b51-110">I den här självstudiekursen kommer du att:</span><span class="sxs-lookup"><span data-stu-id="87b51-110">In this tutorial you will:</span></span>

1. <span data-ttu-id="87b51-111">Konfigurera PolyBase tooload från Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="87b51-111">Configure PolyBase tooload from Azure blob storage</span></span>
2. <span data-ttu-id="87b51-112">Hämta offentliga data till databasen</span><span class="sxs-lookup"><span data-stu-id="87b51-112">Load public data into your database</span></span>
3. <span data-ttu-id="87b51-113">Utföra optimeringar när hello belastningen är klar.</span><span class="sxs-lookup"><span data-stu-id="87b51-113">Perform optimizations after hello load is finished.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="87b51-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="87b51-114">Before you begin</span></span>
<span data-ttu-id="87b51-115">toorun den här självstudiekursen kommer du behöver ett Azure-konto som redan har en SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="87b51-115">toorun this tutorial, you need an Azure account that already has a SQL Data Warehouse database.</span></span> <span data-ttu-id="87b51-116">Om du inte redan har det, se [skapa ett SQL Data Warehouse][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="87b51-116">If you don't already have this, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse].</span></span>

## <a name="1-configure-hello-data-source"></a><span data-ttu-id="87b51-117">1. Konfigurera hello-datakälla</span><span class="sxs-lookup"><span data-stu-id="87b51-117">1. Configure hello data source</span></span>
<span data-ttu-id="87b51-118">PolyBase använder sig av T-SQL externa objekt toodefine hello plats och attribut för hello externa data.</span><span class="sxs-lookup"><span data-stu-id="87b51-118">PolyBase uses T-SQL external objects toodefine hello location and attributes of hello external data.</span></span> <span data-ttu-id="87b51-119">hello externa objektdefinitioner lagras i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="87b51-119">hello external object definitions are stored in SQL Data Warehouse.</span></span> <span data-ttu-id="87b51-120">hello data lagras externt.</span><span class="sxs-lookup"><span data-stu-id="87b51-120">hello data itself is stored externally.</span></span>

### <a name="11-create-a-credential"></a><span data-ttu-id="87b51-121">1.1.</span><span class="sxs-lookup"><span data-stu-id="87b51-121">1.1.</span></span> <span data-ttu-id="87b51-122">Skapa en autentiseringsuppgift</span><span class="sxs-lookup"><span data-stu-id="87b51-122">Create a credential</span></span>
<span data-ttu-id="87b51-123">**Hoppa över detta steg** om du läser in hello Contoso offentliga data.</span><span class="sxs-lookup"><span data-stu-id="87b51-123">**Skip this step** if you are loading hello Contoso public data.</span></span> <span data-ttu-id="87b51-124">Behöver du inte skydda åtkomsten toohello offentliga data eftersom det redan finns tillgänglig tooanyone.</span><span class="sxs-lookup"><span data-stu-id="87b51-124">You don't need secure access toohello public data since it is already accessible tooanyone.</span></span>

<span data-ttu-id="87b51-125">**Hoppa inte över det här steget** om du använder den här kursen som en mall för att läsa in dina egna data.</span><span class="sxs-lookup"><span data-stu-id="87b51-125">**Don't skip this step** if you are using this tutorial as a template for loading your own data.</span></span> <span data-ttu-id="87b51-126">tooaccess data via en autentiseringsuppgift Använd hello följande skript toocreate en databas-omfattande autentiseringsuppgifter och sedan använda den när du definierar hello platsen för hello-datakälla.</span><span class="sxs-lookup"><span data-stu-id="87b51-126">tooaccess data through a credential, use hello following script toocreate a database-scoped credential, and then use it when defining hello location of hello data source.</span></span>

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
```

<span data-ttu-id="87b51-127">Hoppa över toostep 2.</span><span class="sxs-lookup"><span data-stu-id="87b51-127">Skip toostep 2.</span></span>

### <a name="12-create-hello-external-data-source"></a><span data-ttu-id="87b51-128">1.2.</span><span class="sxs-lookup"><span data-stu-id="87b51-128">1.2.</span></span> <span data-ttu-id="87b51-129">Skapa hello extern datakälla</span><span class="sxs-lookup"><span data-stu-id="87b51-129">Create hello external data source</span></span>
<span data-ttu-id="87b51-130">Använd den här [Skapa extern DATAKÄLLA] [ CREATE EXTERNAL DATA SOURCE] kommandot toostore hello plats hello data och hello typ av data.</span><span class="sxs-lookup"><span data-stu-id="87b51-130">Use this [CREATE EXTERNAL DATA SOURCE][CREATE EXTERNAL DATA SOURCE] command toostore hello location of hello data, and hello type of data.</span></span> 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [!IMPORTANT]
> <span data-ttu-id="87b51-131">Om du väljer toomake dina azure blob storage-behållare gemensamma, Kom ihåg att som hello dataägaren du kommer att debiteras för data utgång avgifter när data lämnar hello datacenter.</span><span class="sxs-lookup"><span data-stu-id="87b51-131">If you choose toomake your azure blob storage containers public, remember that as hello data owner you will be charged for data egress charges when data leaves hello data center.</span></span> 
> 
> 

## <a name="2-configure-data-format"></a><span data-ttu-id="87b51-132">2. Konfigurera dataformat</span><span class="sxs-lookup"><span data-stu-id="87b51-132">2. Configure data format</span></span>
<span data-ttu-id="87b51-133">hello data lagras i textfiler i Azure blob storage och varje fält avgränsas med en avgränsare.</span><span class="sxs-lookup"><span data-stu-id="87b51-133">hello data is stored in text files in Azure blob storage, and each field is separated with a delimiter.</span></span> <span data-ttu-id="87b51-134">Kör den här [skapa externt FILFORMAT] [ CREATE EXTERNAL FILE FORMAT] kommandot toospecify hello dataformat hello i hello textfiler.</span><span class="sxs-lookup"><span data-stu-id="87b51-134">Run this [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT] command toospecify hello format of hello data in hello text files.</span></span> <span data-ttu-id="87b51-135">hello Contoso data är okomprimerade pipe avgränsade.</span><span class="sxs-lookup"><span data-stu-id="87b51-135">hello Contoso data is uncompressed and pipe delimited.</span></span>

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-hello-external-tables"></a><span data-ttu-id="87b51-136">3. Skapa hello externa tabeller</span><span class="sxs-lookup"><span data-stu-id="87b51-136">3. Create hello external tables</span></span>
<span data-ttu-id="87b51-137">Nu när du har angett hello käll- och dataformat är klar toocreate hello externa tabeller.</span><span class="sxs-lookup"><span data-stu-id="87b51-137">Now that you have specified hello data source and file format, you are ready toocreate hello external tables.</span></span> 

### <a name="31-create-a-schema-for-hello-data"></a><span data-ttu-id="87b51-138">3.1.</span><span class="sxs-lookup"><span data-stu-id="87b51-138">3.1.</span></span> <span data-ttu-id="87b51-139">Skapa ett schema för hello data.</span><span class="sxs-lookup"><span data-stu-id="87b51-139">Create a schema for hello data.</span></span>
<span data-ttu-id="87b51-140">toocreate en plats toostore hello Contoso data i databasen, skapa ett schema.</span><span class="sxs-lookup"><span data-stu-id="87b51-140">toocreate a place toostore hello Contoso data in your database, create a schema.</span></span>

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-hello-external-tables"></a><span data-ttu-id="87b51-141">3.2.</span><span class="sxs-lookup"><span data-stu-id="87b51-141">3.2.</span></span> <span data-ttu-id="87b51-142">Skapa hello externa tabeller.</span><span class="sxs-lookup"><span data-stu-id="87b51-142">Create hello external tables.</span></span>
<span data-ttu-id="87b51-143">Kör det här skriptet toocreate hello DimProduct och FactOnlineSales externa tabeller.</span><span class="sxs-lookup"><span data-stu-id="87b51-143">Run this script toocreate hello DimProduct and FactOnlineSales external tables.</span></span> <span data-ttu-id="87b51-144">Alla vi gör här definiera kolumnnamn och datatyper och binda dem toohello plats och format för filer för hello Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="87b51-144">All we are doing here is defining column names and data types, and binding them toohello location and format of hello Azure blob storage files.</span></span> <span data-ttu-id="87b51-145">hello definition lagras i SQL Data Warehouse och hello data finns kvar i hello Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="87b51-145">hello definition is stored in SQL Data Warehouse and hello data is still in hello Azure Storage Blob.</span></span>

<span data-ttu-id="87b51-146">Hej **plats** parametern är hello mapp under hello rotmappen i hello Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="87b51-146">hello  **LOCATION** parameter is hello folder under hello root folder in hello Azure Storage Blob.</span></span> <span data-ttu-id="87b51-147">Varje tabell är i en annan mapp.</span><span class="sxs-lookup"><span data-stu-id="87b51-147">Each table is in a different folder.</span></span>

```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-hello-data"></a><span data-ttu-id="87b51-148">4. Läs in hello data</span><span class="sxs-lookup"><span data-stu-id="87b51-148">4. Load hello data</span></span>
<span data-ttu-id="87b51-149">Det finns olika sätt tooaccess externa data.</span><span class="sxs-lookup"><span data-stu-id="87b51-149">There's different ways tooaccess external data.</span></span>  <span data-ttu-id="87b51-150">Du kan fråga efter data direkt från hello extern tabell, Läs in hello data till den nya databastabeller eller Lägg till externa data tooexisting databastabeller.</span><span class="sxs-lookup"><span data-stu-id="87b51-150">You can query data directly from hello external table, load hello data into new database tables, or add external data tooexisting database tables.</span></span>  

### <a name="41-create-a-new-schema"></a><span data-ttu-id="87b51-151">4.1.</span><span class="sxs-lookup"><span data-stu-id="87b51-151">4.1.</span></span> <span data-ttu-id="87b51-152">Skapa ett nytt schema</span><span class="sxs-lookup"><span data-stu-id="87b51-152">Create a new schema</span></span>
<span data-ttu-id="87b51-153">CTAS skapar en ny tabell som innehåller data.</span><span class="sxs-lookup"><span data-stu-id="87b51-153">CTAS creates a new table that contains data.</span></span>  <span data-ttu-id="87b51-154">Skapa först ett schema för hello contoso data.</span><span class="sxs-lookup"><span data-stu-id="87b51-154">First, create a schema for hello contoso data.</span></span>

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-hello-data-into-new-tables"></a><span data-ttu-id="87b51-155">4.2.</span><span class="sxs-lookup"><span data-stu-id="87b51-155">4.2.</span></span> <span data-ttu-id="87b51-156">Läs in hello data till nya tabeller</span><span class="sxs-lookup"><span data-stu-id="87b51-156">Load hello data into new tables</span></span>
<span data-ttu-id="87b51-157">tooload data från Azure blob storage och spara den i en tabell i databasen, använder du hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] instruktionen.</span><span class="sxs-lookup"><span data-stu-id="87b51-157">tooload data from Azure blob storage and save it in a table inside of your database, use hello [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] statement.</span></span> <span data-ttu-id="87b51-158">Läser in med CTAS använder hello starkt typbestämd externa tabeller som du har bara created.tooload hello data till nya tabeller måste använda en [CTAS] [ CTAS] instruktionen per tabell.</span><span class="sxs-lookup"><span data-stu-id="87b51-158">Loading with CTAS leverages hello strongly typed external tables you have just created.tooload hello data into new tables, use one [CTAS][CTAS] statement per table.</span></span> 
 
<span data-ttu-id="87b51-159">CTAS skapar en ny tabell och fylls med hello resultaten av en select-instruktion.</span><span class="sxs-lookup"><span data-stu-id="87b51-159">CTAS creates a new table and populates it with hello results of a select statement.</span></span> <span data-ttu-id="87b51-160">CTAS definierar hello ny tabell toohave hello samma kolumner och datatyper som hello resultaten av hello select-uttryck.</span><span class="sxs-lookup"><span data-stu-id="87b51-160">CTAS defines hello new table toohave hello same columns and data types as hello results of hello select statement.</span></span> <span data-ttu-id="87b51-161">Om du väljer alla hello kolumner från en extern tabell kommer hello nya tabellen att en replik av hello kolumner och datatyper i hello extern tabell.</span><span class="sxs-lookup"><span data-stu-id="87b51-161">If you select all hello columns from an external table, hello new table will be a replica of hello columns and data types in hello external table.</span></span>

<span data-ttu-id="87b51-162">Det här exemplet skapa både hello dimension och hello faktatabell som hash-distribuerade tabeller.</span><span class="sxs-lookup"><span data-stu-id="87b51-162">In this example, we create both hello dimension and hello fact table as hash distributed tables.</span></span> 

```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-hello-load-progress"></a><span data-ttu-id="87b51-163">4.3 följa förloppet för hello belastning</span><span class="sxs-lookup"><span data-stu-id="87b51-163">4.3 Track hello load progress</span></span>
<span data-ttu-id="87b51-164">Du kan följa förloppet för hello din belastningen med dynamiska hanteringsvyer (av DMV: er).</span><span class="sxs-lookup"><span data-stu-id="87b51-164">You can track hello progress of your load using dynamic management views (DMVs).</span></span> 

```sql
-- toosee all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- toosee a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- tootrack bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a><span data-ttu-id="87b51-165">5. Optimera columnstore komprimering</span><span class="sxs-lookup"><span data-stu-id="87b51-165">5. Optimize columnstore compression</span></span>
<span data-ttu-id="87b51-166">Som standard lagrar SQL Data Warehouse hello tabellen som ett grupperat columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="87b51-166">By default, SQL Data Warehouse stores hello table as a clustered columnstore index.</span></span> <span data-ttu-id="87b51-167">När en belastning är klar, kan vissa av hello datarader inte komprimeras i hello columnstore.</span><span class="sxs-lookup"><span data-stu-id="87b51-167">After a load completes, some of hello data rows might not be compressed into hello columnstore.</span></span>  <span data-ttu-id="87b51-168">Det finns många olika orsaker till detta kan inträffa.</span><span class="sxs-lookup"><span data-stu-id="87b51-168">There's a variety of reasons why this can happen.</span></span> <span data-ttu-id="87b51-169">Det finns fler toolearn [hantera kolumnlagringsindex][manage columnstore indexes].</span><span class="sxs-lookup"><span data-stu-id="87b51-169">toolearn more, see [manage columnstore indexes][manage columnstore indexes].</span></span>

<span data-ttu-id="87b51-170">columnstore-komprimering efter en belastning och toooptimize frågeprestanda återskapa hello tabell tooforce hello columnstore-index toocompress alla hello rader.</span><span class="sxs-lookup"><span data-stu-id="87b51-170">toooptimize query performance and columnstore compression after a load, rebuild hello table tooforce hello columnstore index toocompress all hello rows.</span></span> 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

<span data-ttu-id="87b51-171">Mer information om hur du underhåller columnstore-index finns hello [hantera kolumnlagringsindex] [ manage columnstore indexes] artikel.</span><span class="sxs-lookup"><span data-stu-id="87b51-171">For more information on maintaining columnstore indexes, see hello [manage columnstore indexes][manage columnstore indexes] article.</span></span>

## <a name="6-optimize-statistics"></a><span data-ttu-id="87b51-172">6. Optimera statistik</span><span class="sxs-lookup"><span data-stu-id="87b51-172">6. Optimize statistics</span></span>
<span data-ttu-id="87b51-173">Det är bästa toocreate enkolumns-statistik omedelbart efter en belastning.</span><span class="sxs-lookup"><span data-stu-id="87b51-173">It is best toocreate single-column statistics immediately after a load.</span></span> <span data-ttu-id="87b51-174">Det finns vissa alternativ för statistik.</span><span class="sxs-lookup"><span data-stu-id="87b51-174">There are some choices for statistics.</span></span> <span data-ttu-id="87b51-175">Till exempel om du skapar enkolumns-statistik på alla kolumner kan det ta en lång tid toorebuild all statistik om hello.</span><span class="sxs-lookup"><span data-stu-id="87b51-175">For example, if you create single-column statistics on every column it might take a long time toorebuild all hello statistics.</span></span> <span data-ttu-id="87b51-176">Om du vet att vissa kolumner inte ska toobe i frågan predikat kan du hoppa över att skapa statistik på de kolumnerna.</span><span class="sxs-lookup"><span data-stu-id="87b51-176">If you know certain columns are not going toobe in query predicates, you can skip creating statistics on those columns.</span></span>

<span data-ttu-id="87b51-177">Om du väljer toocreate enkolumns-statistik på varje kolumn i varje tabell, kan du använda hello lagrade proceduren kodexemplet `prc_sqldw_create_stats` i hello [statistik] [ statistics] artikel.</span><span class="sxs-lookup"><span data-stu-id="87b51-177">If you decide toocreate single-column statistics on every column of every table, you can use hello stored procedure code sample `prc_sqldw_create_stats` in hello [statistics][statistics] article.</span></span>

<span data-ttu-id="87b51-178">hello följande exempel är en bra utgångspunkt för att skapa statistik.</span><span class="sxs-lookup"><span data-stu-id="87b51-178">hello following example is a good starting point for creating statistics.</span></span> <span data-ttu-id="87b51-179">Den skapar enkolumns-statistik på varje kolumn i dimensionstabellen hello och på varje anslutande kolumn i hello faktatabeller.</span><span class="sxs-lookup"><span data-stu-id="87b51-179">It creates single-column statistics on each column in hello dimension table, and on each joining column in hello fact tables.</span></span> <span data-ttu-id="87b51-180">Du kan alltid lägga till en eller flera kolumner statistik tooother fakta tabellkolumner vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="87b51-180">You can always add single or multi-column statistics tooother fact table columns later on.</span></span>

```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a><span data-ttu-id="87b51-181">Uppnå låsas upp!</span><span class="sxs-lookup"><span data-stu-id="87b51-181">Achievement unlocked!</span></span>
<span data-ttu-id="87b51-182">Offentliga data har lästs in Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="87b51-182">You have successfully loaded public data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="87b51-183">Bra jobbat!</span><span class="sxs-lookup"><span data-stu-id="87b51-183">Great job!</span></span>

<span data-ttu-id="87b51-184">Du kan nu starta frågar hello tabeller med hjälp av frågor som hello nedan:</span><span class="sxs-lookup"><span data-stu-id="87b51-184">You can now start querying hello tables using queries like hello following:</span></span>

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a><span data-ttu-id="87b51-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="87b51-185">Next steps</span></span>
<span data-ttu-id="87b51-186">tooload hello fullständig Contoso Retail Data Warehouse-data, använder hello skriptet i för fler utvecklingstips, se [översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="87b51-186">tooload hello full Contoso Retail Data Warehouse data, use hello script in For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[manage columnstore indexes]: sql-data-warehouse-tables-index.md
[Statistics]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[CREATE EXTERNAL DATA SOURCE]: https://msdn.microsoft.com/en-us/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/en-us/library/dn935026.aspx
[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Load hello full Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
