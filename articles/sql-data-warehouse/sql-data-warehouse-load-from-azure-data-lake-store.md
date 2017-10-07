---
title: aaaLoad - Azure Data Lake Store tooSQL Data Warehouse | Microsoft Docs
description: "Lär dig hur toouse PolyBase externa tabeller tooload data från Azure Data Lake Store till Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 01/25/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: 50ef23b3eba5f58bc9974095f84140dc5c11fa4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-azure-data-lake-store-into-sql-data-warehouse"></a><span data-ttu-id="7f50d-103">Läs in data från Azure Data Lake Store till SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="7f50d-103">Load data from Azure Data Lake Store into SQL Data Warehouse</span></span>
<span data-ttu-id="7f50d-104">Det här dokumentet får du alla steg som du behöver tooload dina egna data från Azure Data Lake Store (ADLS) till SQL Data Warehouse med PolyBase.</span><span class="sxs-lookup"><span data-stu-id="7f50d-104">This document gives you all steps you  need tooload your own data from Azure Data Lake Store (ADLS) into SQL Data Warehouse using PolyBase.</span></span>
<span data-ttu-id="7f50d-105">När du arbetar kan toorun ad hoc-frågor via hello data som lagras i ADLS med hello externa tabeller som bästa praxis föreslår vi importera hello data till hello SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7f50d-105">While you are able toorun adhoc queries over hello data stored in ADLS using hello External Tables, as a best practice we suggest importing hello data into hello SQL Data Warehouse.</span></span>
<span data-ttu-id="7f50d-106">Tid uppskattning: 10 minuter under förutsättning att du har hello förutsättningar måste toocomplete.</span><span class="sxs-lookup"><span data-stu-id="7f50d-106">Time Estimate: 10 minutes assuming you have hello prerequisites need toocomplete.</span></span>
<span data-ttu-id="7f50d-107">I den här kursen får du lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="7f50d-107">In this tutorial you will learn how to:</span></span>

1. <span data-ttu-id="7f50d-108">Skapa extern databas objekt tooload från Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="7f50d-108">Create External Database objects tooload from Azure Data Lake Store.</span></span>
2. <span data-ttu-id="7f50d-109">Ansluta tooan Azure Data Lake Store-katalogen.</span><span class="sxs-lookup"><span data-stu-id="7f50d-109">Connect tooan Azure Data Lake Store Directory.</span></span>
3. <span data-ttu-id="7f50d-110">Läs in data till Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7f50d-110">Load data into Azure SQL Data Warehouse.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="7f50d-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="7f50d-111">Before you begin</span></span>
<span data-ttu-id="7f50d-112">toorun den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="7f50d-112">toorun this tutorial, you need:</span></span>

* <span data-ttu-id="7f50d-113">Azure Active Directory-program toouse för tjänst-till-tjänst-autentisering.</span><span class="sxs-lookup"><span data-stu-id="7f50d-113">Azure Active Directory Application toouse for Service-to-Service authentication.</span></span> <span data-ttu-id="7f50d-114">toocreate, Följ [Active directory-autentisering](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory)</span><span class="sxs-lookup"><span data-stu-id="7f50d-114">toocreate, follow [Active directory authentication](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory)</span></span>

>[!NOTE] 
> <span data-ttu-id="7f50d-115">Du måste hello klient-ID och nyckel OAuth2.0 Token Endpoint-värdet för din Active Directory-program tooconnect tooyour Azure Data Lake från SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7f50d-115">You need hello client ID, Key, and OAuth2.0 Token Endpoint Value of your Active Directory Application tooconnect tooyour Azure Data Lake from SQL Data Warehouse.</span></span> <span data-ttu-id="7f50d-116">Information om hur tooget dessa värden är i hello länken ovan.</span><span class="sxs-lookup"><span data-stu-id="7f50d-116">Details for how tooget these values are in hello link above.</span></span>
><span data-ttu-id="7f50d-117">Obs för registrering av Azure Active Directory App använder hello program-ID som hello klient-ID.</span><span class="sxs-lookup"><span data-stu-id="7f50d-117">Note for Azure Active Directory App Registration use hello 'Application ID' as hello Client ID.</span></span>

* <span data-ttu-id="7f50d-118">SQL Server Management Studio eller SQL Server Data Tools, toodownload SSMS och anslutas finns [fråga SSMS](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-query-ssms)</span><span class="sxs-lookup"><span data-stu-id="7f50d-118">SQL Server Management Studio or SQL Server Data Tools, toodownload SSMS and connect see [Query SSMS](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-query-ssms)</span></span>

* <span data-ttu-id="7f50d-119">Ett Azure SQL Data Warehouse toocreate som följer: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision</span><span class="sxs-lookup"><span data-stu-id="7f50d-119">An Azure SQL Data Warehouse, toocreate one follow: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision</span></span>

* <span data-ttu-id="7f50d-120">Ett Azure Data Lake Store, med eller utan kryptering aktiverat.</span><span class="sxs-lookup"><span data-stu-id="7f50d-120">An Azure Data Lake Store, with or without encryption enabled.</span></span> <span data-ttu-id="7f50d-121">toocreate en Följ: https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal</span><span class="sxs-lookup"><span data-stu-id="7f50d-121">toocreate one follow: https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal</span></span>




## <a name="configure-hello-data-source"></a><span data-ttu-id="7f50d-122">Konfigurera hello-datakälla</span><span class="sxs-lookup"><span data-stu-id="7f50d-122">Configure hello data source</span></span>
<span data-ttu-id="7f50d-123">PolyBase använder sig av T-SQL externa objekt toodefine hello plats och attribut för hello externa data.</span><span class="sxs-lookup"><span data-stu-id="7f50d-123">PolyBase uses T-SQL external objects toodefine hello location and attributes of hello external data.</span></span> <span data-ttu-id="7f50d-124">hello externa objekt som lagras i SQL Data Warehouse och referens hello data th lagras externt.</span><span class="sxs-lookup"><span data-stu-id="7f50d-124">hello external objects are stored in SQL Data Warehouse and reference hello data th is stored externally.</span></span>


###  <a name="create-a-credential"></a><span data-ttu-id="7f50d-125">Skapa en autentiseringsuppgift</span><span class="sxs-lookup"><span data-stu-id="7f50d-125">Create a credential</span></span>
<span data-ttu-id="7f50d-126">tooaccess din Azure Data Lake lagra måste du toocreate en huvudnyckel för databasen tooencrypt dina autentiseringsuppgifter hemlighet som används i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="7f50d-126">tooaccess your Azure Data Lake Store, you will need toocreate a Database Master Key tooencrypt your credential secret used in hello next step.</span></span>
<span data-ttu-id="7f50d-127">Sedan kan du skapa en Databasbegränsade autentiseringsuppgift som lagrar hello service principal autentiseringsuppgifter i AAD.</span><span class="sxs-lookup"><span data-stu-id="7f50d-127">You then create a Database scoped credential, which stores hello service principal credentials set up in AAD.</span></span> <span data-ttu-id="7f50d-128">För de som har använt PolyBase tooconnect tooWindows Azure Storage-Blobbar, Observera att hello-autentiseringsuppgifter är syntaxen olika.</span><span class="sxs-lookup"><span data-stu-id="7f50d-128">For those of you who have used PolyBase tooconnect tooWindows Azure Storage Blobs, note that hello credential syntax is different.</span></span>
<span data-ttu-id="7f50d-129">tooconnect tooAzure Data Lake Store, måste du **första** skapa ett Azure Active Directory-program, skapa en snabbtangent och bevilja hello åtkomst toohello Azure Data Lake programresursen.</span><span class="sxs-lookup"><span data-stu-id="7f50d-129">tooconnect tooAzure Data Lake Store, you must **first** create an Azure Active Directory Application, create an access key, and grant hello application access toohello Azure Data Lake resource.</span></span> <span data-ttu-id="7f50d-130">Instrucitons tooperform de här stegen finns [här](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-authenticate-using-active-directory).</span><span class="sxs-lookup"><span data-stu-id="7f50d-130">Instrucitons tooperform these steps are located [here](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-authenticate-using-active-directory).</span></span>

```sql
-- A: Create a Database Master Key.
-- Only necessary if one does not already exist.
-- Required tooencrypt hello credential secret in hello next step.
-- For more information on Master Key: https://msdn.microsoft.com/en-us/library/ms174382.aspx?f=255&MSPPError=-2147217396

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Pass hello client id and OAuth 2.0 Token Endpoint taken from your Azure Active Directory Application
-- SECRET: Provide your AAD Application Service Principal key.
-- For more information on Create Database Scoped Credential: https://msdn.microsoft.com/en-us/library/mt270260.aspx

CREATE DATABASE SCOPED CREDENTIAL ADLCredential
WITH
    IDENTITY = '<client_id>@<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;

-- It should look something like this:
CREATE DATABASE SCOPED CREDENTIAL ADLCredential
WITH
    IDENTITY = '536540b4-4239-45fe-b9a3-629f97591c0c@https://login.microsoftonline.com/42f988bf-85f1-41af-91ab-2d2cd011da47/oauth2/token',
    SECRET = 'BjdIlmtKp4Fpyh9hIvr8HJlUida/seM5kQ3EpLAmeDI='
;
```


### <a name="create-hello-external-data-source"></a><span data-ttu-id="7f50d-131">Skapa hello extern datakälla</span><span class="sxs-lookup"><span data-stu-id="7f50d-131">Create hello external data source</span></span>
<span data-ttu-id="7f50d-132">Använd den här [Skapa extern DATAKÄLLA] [ CREATE EXTERNAL DATA SOURCE] kommandot toostore hello plats hello data och hello typ av data.</span><span class="sxs-lookup"><span data-stu-id="7f50d-132">Use this [CREATE EXTERNAL DATA SOURCE][CREATE EXTERNAL DATA SOURCE] command toostore hello location of hello data, and hello type of data.</span></span>
<span data-ttu-id="7f50d-133">Du kan hitta hello ADL URI i hello Azure-portalen och www.portal.azure.com.</span><span class="sxs-lookup"><span data-stu-id="7f50d-133">You can find hello ADL URI in hello Azure portal and www.portal.azure.com.</span></span>

```sql
-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs tooaccess data in Azure Data Lake Store.
-- LOCATION: Provide Azure Data Lake accountname and URI
-- CREDENTIAL: Provide hello credential created in hello previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (
    TYPE = HADOOP,
    LOCATION = 'adl://<AzureDataLake account_name>.azuredatalakestore.net',
    CREDENTIAL = ADLCredential
);
```



## <a name="configure-data-format"></a><span data-ttu-id="7f50d-134">Konfigurera dataformat</span><span class="sxs-lookup"><span data-stu-id="7f50d-134">Configure data format</span></span>
<span data-ttu-id="7f50d-135">tooimport hello data från ADLS, behöver du toospecify hello externa filformatet.</span><span class="sxs-lookup"><span data-stu-id="7f50d-135">tooimport hello data from ADLS, you need toospecify hello external file format.</span></span> <span data-ttu-id="7f50d-136">Det här kommandot har format-specifika alternativ toodescribe dina data.</span><span class="sxs-lookup"><span data-stu-id="7f50d-136">This command has format-specific options toodescribe your data.</span></span>
<span data-ttu-id="7f50d-137">Nedan visas ett exempel på ett vanligt förekommande format som är en pipe-avgränsad textfil.</span><span class="sxs-lookup"><span data-stu-id="7f50d-137">Below is an example of a commonly used file format that is a pipe-delimited text file.</span></span>
<span data-ttu-id="7f50d-138">Titta på vår T-SQL-dokumentation för en fullständig lista över [skapa externt FILFORMAT][CREATE EXTERNAL FILE FORMAT]</span><span class="sxs-lookup"><span data-stu-id="7f50d-138">Look at our T-SQL documentation for a complete list of [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT]</span></span>

```sql
-- D: Create an external file format
-- FIELD_TERMINATOR: Marks hello end of each field (column) in a delimited text file
-- STRING_DELIMITER: Specifies hello field terminator for data of type string in hello text-delimited file.
-- DATE_FORMAT: Specifies a custom format for all date and time data that might appear in a delimited text file.
-- Use_Type_Default: Store all Missing values as NULL

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

## <a name="create-hello-external-tables"></a><span data-ttu-id="7f50d-139">Skapa hello externa tabeller</span><span class="sxs-lookup"><span data-stu-id="7f50d-139">Create hello external tables</span></span>
<span data-ttu-id="7f50d-140">Nu när du har angett hello käll- och dataformat är klar toocreate hello externa tabeller.</span><span class="sxs-lookup"><span data-stu-id="7f50d-140">Now that you have specified hello data source and file format, you are ready toocreate hello external tables.</span></span> <span data-ttu-id="7f50d-141">Externa tabeller är hur du interagerar med externa data.</span><span class="sxs-lookup"><span data-stu-id="7f50d-141">External tables are how you interact with external data.</span></span> <span data-ttu-id="7f50d-142">PolyBase använder rekursiv directory traversal tooread alla filer i alla underkataloger hello-katalog som anges i hello platsparametern.</span><span class="sxs-lookup"><span data-stu-id="7f50d-142">PolyBase uses recursive directory traversal tooread all files in all subdirectories of hello directory specified in hello location parameter.</span></span> <span data-ttu-id="7f50d-143">Hello följande exempel visas också hur toocreate hello objekt.</span><span class="sxs-lookup"><span data-stu-id="7f50d-143">Also, hello following example shows how toocreate hello object.</span></span> <span data-ttu-id="7f50d-144">Du måste toocustomize hello instruktionen toowork med hello data som du har i ADLS.</span><span class="sxs-lookup"><span data-stu-id="7f50d-144">You need toocustomize hello statement toowork with hello data you have in ADLS.</span></span>

```sql
-- D: Create an External Table
-- LOCATION: Folder under hello ADLS root folder.
-- DATA_SOURCE: Specifies which Data Source Object toouse.
-- FILE_FORMAT: Specifies which File Format Object toouse
-- REJECT_TYPE: Specifies how you want toodeal with rejected rows. Either Value or percentage of hello total
-- REJECT_VALUE: Sets hello Reject value based on hello reject type.

-- DimProduct
CREATE EXTERNAL TABLE [dbo].[DimProduct_external] (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL
)
WITH
(
    LOCATION='/DimProduct/'
,   DATA_SOURCE = AzureDataLakeStore
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

```

## <a name="external-table-considerations"></a><span data-ttu-id="7f50d-145">Överväganden för extern tabell</span><span class="sxs-lookup"><span data-stu-id="7f50d-145">External Table Considerations</span></span>
<span data-ttu-id="7f50d-146">Är enkelt att skapa en extern tabell, men det finns några olika delarna som behöver toobe beskrivs.</span><span class="sxs-lookup"><span data-stu-id="7f50d-146">Creating an external table is easy, but there are some nuances that need toobe discussed.</span></span>

<span data-ttu-id="7f50d-147">Läser in data med PolyBase typkontroll strikt.</span><span class="sxs-lookup"><span data-stu-id="7f50d-147">Loading data with PolyBase is strongly typed.</span></span> <span data-ttu-id="7f50d-148">Detta innebär att varje datarad hello som inhämtas måste uppfylla hello tabellschemadefinitionen.</span><span class="sxs-lookup"><span data-stu-id="7f50d-148">This means that each row of hello data being ingested must satisfy hello table schema definition.</span></span>
<span data-ttu-id="7f50d-149">Om en viss rad som inte matchar hello schemadefinition avvisas hello raden från hello belastningen.</span><span class="sxs-lookup"><span data-stu-id="7f50d-149">If a given row does not match hello schema definition, hello row is rejected from hello load.</span></span>

<span data-ttu-id="7f50d-150">Hej REJECT_TYPE och REJECT_VALUE alternativen kan du toodefine hur många rader eller vilken procentandel av hello data måste finnas i hello slutliga tabellen.</span><span class="sxs-lookup"><span data-stu-id="7f50d-150">hello REJECT_TYPE and REJECT_VALUE options allow you toodefine how many rows or what percentage of hello data must be present in hello final table.</span></span>
<span data-ttu-id="7f50d-151">Om hello avvisa värdet uppnås under belastning misslyckas hello belastningen.</span><span class="sxs-lookup"><span data-stu-id="7f50d-151">During load, if hello reject value is reached, hello load fails.</span></span> <span data-ttu-id="7f50d-152">hello beror vanligaste Avvisade rader på en felaktig schemamatchning definition.</span><span class="sxs-lookup"><span data-stu-id="7f50d-152">hello most common cause of rejected rows is a schema definition mismatch.</span></span>
<span data-ttu-id="7f50d-153">Till exempel om en kolumn anges felaktigt hello schemat för int när hello data i hello-filen är en sträng, misslyckas tooload i varje rad.</span><span class="sxs-lookup"><span data-stu-id="7f50d-153">For example, if a column is incorrectly given hello schema of int when hello data in hello file is a string, every row will fail tooload.</span></span>

<span data-ttu-id="7f50d-154">hello plats anger hello översta katalog som du vill tooread data från.</span><span class="sxs-lookup"><span data-stu-id="7f50d-154">hello Location specifies hello topmost directory that you want tooread data from.</span></span>
<span data-ttu-id="7f50d-155">I det här fallet om det fanns skulle underkataloger på /DimProduct/ PolyBase importera alla hello data inom hello underkataloger.</span><span class="sxs-lookup"><span data-stu-id="7f50d-155">In this case, if there were subdirectories under /DimProduct/ PolyBase would import all hello data within hello subdirectories.</span></span>

## <a name="load-hello-data"></a><span data-ttu-id="7f50d-156">Läs in hello data</span><span class="sxs-lookup"><span data-stu-id="7f50d-156">Load hello data</span></span>
<span data-ttu-id="7f50d-157">tooload data från Azure Data Lake Store använder hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] instruktionen.</span><span class="sxs-lookup"><span data-stu-id="7f50d-157">tooload data from Azure Data Lake Store use hello [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] statement.</span></span> <span data-ttu-id="7f50d-158">Läser in med CTAS typkontroll använder hello strikt extern tabell som du har skapat.</span><span class="sxs-lookup"><span data-stu-id="7f50d-158">Loading with CTAS uses hello strongly typed external table you have created.</span></span>

<span data-ttu-id="7f50d-159">CTAS skapar en ny tabell och fylls med hello resultaten av en select-instruktion.</span><span class="sxs-lookup"><span data-stu-id="7f50d-159">CTAS creates a new table and populates it with hello results of a select statement.</span></span> <span data-ttu-id="7f50d-160">CTAS definierar hello ny tabell toohave hello samma kolumner och datatyper som hello resultaten av hello select-uttryck.</span><span class="sxs-lookup"><span data-stu-id="7f50d-160">CTAS defines hello new table toohave hello same columns and data types as hello results of hello select statement.</span></span> <span data-ttu-id="7f50d-161">Om du väljer alla hello kolumner från en extern tabell är hello ny tabell en replik av hello kolumner och datatyper i hello extern tabell.</span><span class="sxs-lookup"><span data-stu-id="7f50d-161">If you select all hello columns from an external table, hello new table is a replica of hello columns and data types in hello external table.</span></span>

<span data-ttu-id="7f50d-162">I det här exemplet skapar vi en distribuerad hash-tabell som kallas DimProduct från våra extern tabell DimProduct_external.</span><span class="sxs-lookup"><span data-stu-id="7f50d-162">In this example, we are creating a hash distributed table called DimProduct from our External Table DimProduct_external.</span></span>

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```


## <a name="optimize-columnstore-compression"></a><span data-ttu-id="7f50d-163">Optimera columnstore komprimering</span><span class="sxs-lookup"><span data-stu-id="7f50d-163">Optimize columnstore compression</span></span>
<span data-ttu-id="7f50d-164">Som standard lagrar SQL Data Warehouse hello tabellen som ett grupperat columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="7f50d-164">By default, SQL Data Warehouse stores hello table as a clustered columnstore index.</span></span> <span data-ttu-id="7f50d-165">När en belastning är klar, kan vissa av hello datarader inte komprimeras i hello columnstore.</span><span class="sxs-lookup"><span data-stu-id="7f50d-165">After a load completes, some of hello data rows might not be compressed into hello columnstore.</span></span>  <span data-ttu-id="7f50d-166">Det finns många olika orsaker till detta kan inträffa.</span><span class="sxs-lookup"><span data-stu-id="7f50d-166">There's a variety of reasons why this can happen.</span></span> <span data-ttu-id="7f50d-167">Det finns fler toolearn [hantera kolumnlagringsindex][manage columnstore indexes].</span><span class="sxs-lookup"><span data-stu-id="7f50d-167">toolearn more, see [manage columnstore indexes][manage columnstore indexes].</span></span>

<span data-ttu-id="7f50d-168">columnstore-komprimering efter en belastning och toooptimize frågeprestanda återskapa hello tabell tooforce hello columnstore-index toocompress alla hello rader.</span><span class="sxs-lookup"><span data-stu-id="7f50d-168">toooptimize query performance and columnstore compression after a load, rebuild hello table tooforce hello columnstore index toocompress all hello rows.</span></span>

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

<span data-ttu-id="7f50d-169">Mer information om hur du underhåller columnstore-index finns hello [hantera kolumnlagringsindex] [ manage columnstore indexes] artikel.</span><span class="sxs-lookup"><span data-stu-id="7f50d-169">For more information on maintaining columnstore indexes, see hello [manage columnstore indexes][manage columnstore indexes] article.</span></span>

## <a name="optimize-statistics"></a><span data-ttu-id="7f50d-170">Optimera statistik</span><span class="sxs-lookup"><span data-stu-id="7f50d-170">Optimize statistics</span></span>
<span data-ttu-id="7f50d-171">Det är bästa toocreate enkolumns-statistik omedelbart efter en belastning.</span><span class="sxs-lookup"><span data-stu-id="7f50d-171">It is best toocreate single-column statistics immediately after a load.</span></span> <span data-ttu-id="7f50d-172">Det finns vissa alternativ för statistik.</span><span class="sxs-lookup"><span data-stu-id="7f50d-172">There are some choices for statistics.</span></span> <span data-ttu-id="7f50d-173">Till exempel om du skapar enkolumns-statistik på alla kolumner kan det ta en lång tid toorebuild all statistik om hello.</span><span class="sxs-lookup"><span data-stu-id="7f50d-173">For example, if you create single-column statistics on every column it might take a long time toorebuild all hello statistics.</span></span> <span data-ttu-id="7f50d-174">Om du vet att vissa kolumner inte ska toobe i frågan predikat kan du hoppa över att skapa statistik på de kolumnerna.</span><span class="sxs-lookup"><span data-stu-id="7f50d-174">If you know certain columns are not going toobe in query predicates, you can skip creating statistics on those columns.</span></span>

<span data-ttu-id="7f50d-175">Om du väljer toocreate enkolumns-statistik på varje kolumn i varje tabell, kan du använda hello lagrade proceduren kodexemplet `prc_sqldw_create_stats` i hello [statistik] [ statistics] artikel.</span><span class="sxs-lookup"><span data-stu-id="7f50d-175">If you decide toocreate single-column statistics on every column of every table, you can use hello stored procedure code sample `prc_sqldw_create_stats` in hello [statistics][statistics] article.</span></span>

<span data-ttu-id="7f50d-176">hello följande exempel är en bra utgångspunkt för att skapa statistik.</span><span class="sxs-lookup"><span data-stu-id="7f50d-176">hello following example is a good starting point for creating statistics.</span></span> <span data-ttu-id="7f50d-177">Den skapar enkolumns-statistik på varje kolumn i dimensionstabellen hello och på varje anslutande kolumn i hello faktatabeller.</span><span class="sxs-lookup"><span data-stu-id="7f50d-177">It creates single-column statistics on each column in hello dimension table, and on each joining column in hello fact tables.</span></span> <span data-ttu-id="7f50d-178">Du kan alltid lägga till en eller flera kolumner statistik tooother fakta tabellkolumner vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="7f50d-178">You can always add single or multi-column statistics tooother fact table columns later on.</span></span>


## <a name="achievement-unlocked"></a><span data-ttu-id="7f50d-179">Uppnå låsas upp!</span><span class="sxs-lookup"><span data-stu-id="7f50d-179">Achievement unlocked!</span></span>
<span data-ttu-id="7f50d-180">Data har lästs in Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7f50d-180">You have successfully loaded data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="7f50d-181">Bra jobbat!</span><span class="sxs-lookup"><span data-stu-id="7f50d-181">Great job!</span></span>

##<a name="next-steps"></a><span data-ttu-id="7f50d-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7f50d-182">Next Steps</span></span>
<span data-ttu-id="7f50d-183">Data läses in är hello första steg toodeveloping en data warehouse-lösning med hjälp av SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7f50d-183">Loading data is hello first step toodeveloping a data warehouse solution using SQL Data Warehouse.</span></span> <span data-ttu-id="7f50d-184">Kolla in våra development resurser på [tabeller](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-overview) och [T-SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-loops.md).</span><span class="sxs-lookup"><span data-stu-id="7f50d-184">Check out our development resources on [Tables](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-overview) and [T-SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-loops.md).</span></span>


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
[CREATE EXTERNAL DATA SOURCE]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Load hello full Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
