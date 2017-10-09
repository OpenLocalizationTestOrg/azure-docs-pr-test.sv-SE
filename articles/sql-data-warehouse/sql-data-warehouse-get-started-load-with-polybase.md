---
title: "aaaPolyBase i SQL Data Warehouse självstudiekursen | Microsoft Docs"
description: "Lär dig vad PolyBase är och hur toouse det för informationslagerscenarier."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 0a0103b4-ddd6-4d1e-87be-4965d6e99f3f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 03/01/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: 3e680ec407c1d920dd59ea922b82c9208b5e9a84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-polybase-in-sql-data-warehouse"></a>Läs in data med PolyBase i SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)  
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [BCP](sql-data-warehouse-load-with-bcp.md)
> 
> 

Den här kursen visar hur tooload data till SQL Data Warehouse med hjälp av AzCopy och PolyBase. När du är klar, kommer du att veta hur man:

* Använda AzCopy toocopy data tooAzure blob storage
* Skapa databasobjekt toodefine hello data
* Kör en T-SQL-fråga tooload hello data

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-with-PolyBase-in-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a>Krav
toostep igenom den här kursen behöver du

* En SQL Data Warehouse-databas.
* Ett Azure-lagringskonto av typen standard lokalt redundant lagring (Standard-LRS), standard geo-redundant lagring (Standard-GRS) eller standard geo-redundant lagring med läsbehörighet (Standard-RAGRS).
* Kommandoradsverktyget AzCopy. Hämta och installera hello [senaste versionen av AzCopy] [ latest version of AzCopy] som installeras med hello Microsoft Azure Storage-verktyg.
  
    ![Azure Storage-verktyg](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)

## <a name="step-1-add-sample-data-tooazure-blob-storage"></a>Steg 1: Lägg till exempel data tooAzure blob-lagring
I ordning tooload data måste tooput exempeldata i en Azure-blobblagring. I det här steget fyller vi en Azure Storage-blob med exempeldata. Senare, ska vi använda PolyBase tooload exempeldatan till din SQL Data Warehouse-databas.

### <a name="a-prepare-a-sample-text-file"></a>A. Förbered en exempeltextfil
tooprepare en exempeltextfil:

1. Öppna Anteckningar och kopiera hello följande rader med data i en ny fil. Spara den här tooyour lokala temp-katalog som % temp%\DimDate2.txt.

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

### <a name="b-find-your-blob-service-endpoint"></a>B. Hitta blobbtjänstens slutpunkt
toofind blobbtjänstens slutpunkt:

1. Välj hello Azure Portal **Bläddra** > **Lagringskonton**.
2. Klicka på hello storage-konto som du vill toouse.
3. I hello lagring-kontoblad klickar du på Blobbar
   
    ![Klicka på Blobbar](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)
4. Spara URL:en för blobbtjänstens slutpunkt för senare.
   
    ![Blob-tjänstens slutpunkt](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a>C. Hitta din Azure-lagringsnyckel
toofind Azure-lagringsnyckel:

1. Hello Azure Portal, Välj **Bläddra** > **Lagringskonton**.
2. Klicka på hello storage-konto som du vill toouse.
3. Välj **Alla inställningar** > **Åtkomstnycklar**.
4. Klicka på hello kopiera rutan toocopy en access nycklar toohello Urklipp.
   
    ![Kopiera Azure-lagringsnyckel](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-hello-sample-file-tooazure-blob-storage"></a>D. Kopiera hello exempel filen tooAzure blob-lagring
toocopy datalagringen tooAzure blob:

1. Öppna en kommandotolk och ändra kataloger toohello installationskatalogen för AzCopy. Det här kommandot ändrar toohello Standardinstallationskatalogen på en 64-bitars Windows-klient.
   
    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```
2. Kör följande kommando tooupload hello hello. Ange URL:en för blobbtjänstens slutpunkt för <blob service endpoint URL> och nyckeln för Azure-lagringskontot för <azure_storage_account_key>.
   
    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

Se även [komma igång med kommandoradsverktyget Azcopy hello][Getting Started with hello AzCopy Command-Line Utility].

### <a name="e-explore-your-blob-storage-container"></a>E. Utforska din blobblagringsbehållare
toosee hello filen du överförde tooblob lagring:

1. Gå tillbaka tooyour Blob-tjänsten bladet.
2. Under Behållare dubbelklickar du på **datacontainer**.
3. tooexplore hello sökvägen tooyour data, klickar du på hello mapp **datedimension** och du ser den överförda filen **DimDate2.txt**.
4. tooview egenskaper, klicka på **DimDate2.txt**.
5. Observera att i egenskapsbladet för hello Blob, du kan hämta eller ta bort hello-filen.
   
    ![Visa Azure-lagringsblobb](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)

## <a name="step-2-create-an-external-table-for-hello-sample-data"></a>Steg 2: Skapa en extern tabell för exempeldata hello
I det här avsnittet skapar vi en extern tabell som definierar hello exempeldata.

PolyBase använder externa tabeller tooaccess data i Azure blob storage. Eftersom hello data inte lagras inom SQL Data Warehouse, hanterar PolyBase autentisering toohello externa data med hjälp av en databas-omfattande autentisering.

hello exemplet i det här steget använder de här Transact-SQL-instruktioner toocreate en extern tabell.

* [Skapa huvudnyckel (Transact-SQL)] [ Create Master Key (Transact-SQL)] tooencrypt hello hemligheten för din databas-omfattande autentisering.
* [Skapa Databasomfattande autentisering (Transact-SQL)] [ Create Database Scoped Credential (Transact-SQL)] toospecify autentiseringsinformation för ditt Azure storage-konto.
* [Skapa extern datakälla (Transact-SQL)] [ Create External Data Source (Transact-SQL)] toospecify hello platsen för din Azure-blobblagring.
* [Skapa externt filformat (Transact-SQL)] [ Create External File Format (Transact-SQL)] toospecify hello formatet för dina data.
* [Skapa extern tabell (Transact-SQL)] [ Create External Table (Transact-SQL)] toospecify hello tabelldefinitionen och platsen för hello data.

Kör den här frågan mot din SQL Data Warehouse-databas. Den skapar en extern tabell som heter DimDate2External i dbo-schemat, hello som pekar toohello Exempeldatan dimdate2.txt i hello Azure blob storage.

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


I SQL Server Object Explorer i Visual Studio, kan du se hello externa filformatet, extern datakälla och hello DimDate2External-tabellen.

![Visa extern tabell](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a>Steg 3: Läs in data till SQL Data Warehouse
När hello extern tabell har skapats kan du läsa hello data i en ny tabell eller infoga den i en befintlig tabell.

* tooload hello data i en ny tabell, kör hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] instruktionen. hello nya tabellen kommer att ha hello kolumnerna som namnges i frågan hello. hello datatyper hello kolumner matchar hello datatyper i hello externa tabelldefinitionen.
* tooload hello data i en befintlig tabell använder hello [INSERT... Välj (Transact-SQL)] [ INSERT...SELECT (Transact-SQL)] instruktionen.

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

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Steg 4: Skapa statistik på dina nyinlästa data
SQL Data Warehouse skapar och uppdaterar inte statistik automatiskt. Därför tooachieve hög frågeprestanda, det är viktigt toocreate statistik för varje kolumn av varje tabell efter hello först hämta. Det är också viktigt tooupdate statistik efter betydande förändringar i hello data.

Det här exemplet skapar enkolumns-statistik på hello nya DimDate2-tabellen.

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

Det finns fler toolearn [statistik][Statistics].  

## <a name="next-steps"></a>Nästa steg
Se hello [PolyBase-guiden] [ PolyBase guide] för ytterligare information som du bör känna till när du utvecklar en lösning som använder PolyBase.

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[PolyBase guide]: ./sql-data-warehouse-load-polybase-guide.md
[Getting Started with hello AzCopy Command-Line Utility]:../storage/common/storage-use-azcopy.md
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
