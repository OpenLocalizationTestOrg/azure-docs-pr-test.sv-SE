---
title: "Kom igång aaaAzure SQL Data Warehouse - kursen | Microsoft Docs"
description: "Den här kursen lär du dig hur tooprovision och Läs in data till Azure SQL Data Warehouse. Du får också lära dig hello grunderna om skalning, pausa och justera."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: barbkess
ms.assetid: 52DFC191-E094-4B04-893F-B64D5828A900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: quickstart
ms.date: 01/26/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: edd2a21b0fe49ca8e9792c7c512310339a822c55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-sql-data-warehouse"></a>Komma igång med SQL Data Warehouse

Den här kursen visar hur tooprovision och Läs in data till Azure SQL Data Warehouse. Du får också lära dig hello grunderna om skalning, pausa och justera. När du är klar kan du vara redo tooquery och utforska dina data warehouse.

**Uppskattad tid toocomplete:** det här är en vägledning för slutpunkt till slutpunkt med exempelkod som tar cirka 30 minuter toocomplete när hello förutsättningar är uppfyllda. 

## <a name="prerequisites"></a>Krav

hello kursen förutsätter att du är bekant med grundläggande koncept för SQL Data Warehouse. En introduktion finns i [Vad är SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md) 

### <a name="sign-up-for-microsoft-azure"></a>Registrera dig för Microsoft Azure
Om du inte redan har ett Microsoft Azure-konto, måste toosign för en toouse den här tjänsten. Om du redan har ett konto kan du hoppa över det här steget. 

1. Navigera toohello konto sidor [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/)
2. Skapa ett kostnadsfritt Azure-konto eller köp ett konto.
3. Följ instruktionerna för hello

### <a name="install-appropriate-sql-client-drivers-and-tools"></a>Installera rätt verktyg och drivrutin för SQL-klienten

De flesta SQL-klientverktygen kan ansluta tooSQL Data Warehouse med hjälp av ADO.NET, JDBC eller ODBC. På grund av toohello stort antal T-SQL-funktioner som har stöd för SQL Data Warehouse, är vissa klientprogram inte helt kompatibel med SQL Data Warehouse.

Om du kör ett Windows-operativsystem rekommenderar vi att du använder antingen [Visual Studio] eller [SQL Server Management Studio].

[!INCLUDE [Create a new logical server](../../includes/sql-data-warehouse-create-logical-server.md)] 

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="create-a-sql-data-warehouse"></a>Skapa ett SQL Data Warehouse

En SQL Data Warehouse är en särskild typ av databas som är avsedd för MPP (massively parallel processing). hello databasen distribueras över flera noder och bearbetar frågor parallellt. SQL Data Warehouse har en control-noden som samordnar hello aktiviteter för alla hello-noder. själva hello noderna använda SQL-databas toomanage dina data.  

> [!NOTE]
> Att skapa ett SQL Data Warehouse kan resultera i en ny fakturerbar tjänst.  Mer information finns på [prissidan för SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).
>

### <a name="create-a-data-warehouse"></a>Skapa ett datalager

1. Logga in på hello [Azure-portalen](https://portal.azure.com).
2. Klicka på **Nytt** > **Databaser** > **SQL Data Warehouse**.

    ![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png) ![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)

3. Fyll i distributionsinformationen

    **Databasnamn**: Välj önskat namn. Om du har flera datalager, rekommenderar vi ditt namn som innehåller information om till exempel hello region, miljö, till exempel *mydw-westus-1-test*.

    **Prenumeration**: Din Azure-prenumeration

    **Resursgrupp**: Skapa en resursgrupp eller använd en befintlig resursgrupp.
    > [!NOTE]
    > Resursgrupper är användbara för resursadministration, till exempel för att styra åtkomstkontroll och mallbaserad distribution. Läs mer om Azure-resursgrupper och bästa praxis [här](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)

    **Källa**: Tom databas

    **Server**: Välj hello-server som du skapade i [krav].

    **Sorteringen**: lämna hello Standardsortering SQL_Latin1_General_CP1_CI_AS.

    **Välj prestanda**: Vi rekommenderar att börja med hello standard 400DWU.

4. Välj **PIN-kod toodashboard** ![tooDashboard PIN-kod](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png)

5. Sitta tillbaka och vänta på din data warehouse toodeploy! Det är normalt för den här processen tootake flera minuter. hello-portalen meddelar dig när dina data warehouse är klar toouse. 

## <a name="connect-toosql-data-warehouse"></a>Ansluta tooSQL Data Warehouse

Den här kursen använder SQL Server Management Studio (SSMS) tooconnect toohello-datalagret. Du kan ansluta tooSQL datalagret via anslutningarna stöds: ADO.NET, JDBC, ODBC- och PHP. Tänk på att funktionaliteten kan vara begränsad med verktyg som inte stöds av Microsoft.


### <a name="get-connection-information"></a>Hämta anslutningsinformation

tooconnect tooyour datalagret, behöver du tooconnect via hello logiska SQLServer som du skapade i [krav].

1. Välj ditt data warehouse hello instrumentpanel eller söker efter det i dina resurser.

    ![Instrumentpanel för SQL Data Warehouse](./media/sql-data-warehouse-get-started-tutorial/sql-dw-dashboard.png)

2. Hitta hello fullständigt namn för hello logiska SQLServer.

    ![Välj servernamn](./media/sql-data-warehouse-get-started-tutorial/select-server.png)

3. Öppna SSMS och använder object explorer tooconnect toothis server med administratörsautentiseringsuppgifter för hello server du skapade i [krav]

    ![Anslut med SSMS](./media/sql-data-warehouse-get-started-tutorial/ssms-connect.png)

Om allt går korrekt, bör du nu anslutet tooyour logisk SQL-server. Eftersom du har loggat in som hello-administratören kan ansluta du tooany databasen hos hello-servern, inklusive hello master-databasen. 

Det finns bara en server-administratörskontot och hello har de flesta behörigheter för alla användare. Var noga med att inte tooallow för många personer i din organisation tooknow hello administratörslösenord. 

Du kan också ha ett administratörskonto för Azure Active Directory. Vi tillhandahålla inte hello informationen här. Om du vill toolearn mer om hur du använder Azure Active Directory-autentisering, se [Azure AD authentication](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).

Därefter skapar vi ytterligare inloggningar och användare.


## <a name="create-a-database-user"></a>Skapa en databasanvändare

I det här steget skapar du en användare konto tooaccess ditt data warehouse. Vi också visar hur toogive som användaren hello möjlighet toorun frågor med stora mängder minne och processorresurser.

### <a name="notes-about-resource-classes-for-allocating-resources-tooqueries"></a>Information om resursklasser för att fördela resurser tooqueries

- tookeep dina data är säkra, Använd inte hello server admin toorun frågor på produktionsdatabaserna. Det har hello de flesta behörigheter för alla användare och använder den tooperform åtgärder på användardata placerar dina data i fara. Dessutom eftersom hello serveradministratören är avsedd tooperform hanteringsåtgärder, körs operations med bara en liten fördelningen av minne och processorresurser. 

- SQL Data Warehouse använder fördefinierade databasroller som kallas resursklasser, tooallocate olika mängder minne, CPU-resurser och samtidighet fack toousers. Varje användare kan tillhöra tooa small, medium, stora eller extra stor resursklassen. hello användarens resursklassen bestämmer hello resurser hello användaren har toorun frågor och läsa in åtgärder.

- För optimala datakomprimering måste hello användaren tooload med stor eller extra stor resursallokeringar. Läs mer om resursklasser [här](./sql-data-warehouse-develop-concurrency.md#resource-classes):

### <a name="create-an-account-that-can-control-a-database"></a>Skapa ett konto som kan styra en databas

Eftersom du är inloggad i hello-administratören ha behörigheter toocreate inloggningar och användare.

1. Med SSMS eller någon annan frågeklient öppnar du en ny fråga för **huvuddatabasen**.

    ![Ny fråga mot huvuddatabas](./media/sql-data-warehouse-get-started-tutorial/query-on-server.png)

    ![Ny fråga mot huvuddatabas1](./media/sql-data-warehouse-get-started-tutorial/query-on-master.png)

2. Kör det här kommandot T-SQL-toocreate i hello frågefönstret en inloggning med namnet MedRCLogin och användare med namnet LoadingUser. Den här inloggningen kan ansluta toohello logisk SQL-server.

    ```sql
    CREATE LOGIN MedRCLogin WITH PASSWORD = 'a123reallySTRONGpassword!';
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

3. Nu frågar hello *SQL Data Warehouse-databas*, skapa en databasanvändare baserat på hello inloggning som du skapade tooaccess och utföra åtgärder på hello-databasen.

    ```sql
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

4. Ge hello användare kontroll behörigheter toohello databasen kallas NYT. 

    ```sql
    GRANT CONTROL ON DATABASE::[NYT] tooLoadingUser;
    ```
    > [!NOTE]
    > Om databasnamnet är bindestreck vara säker på att toowrap den inom parentes! 
    >

### <a name="give-hello-user-medium-resource-allocations"></a>Ge hello användaren medelhög resursallokeringar

1. Kör den här T-SQL-kommandot toomake detta till en medlem av hello medelhög resursklassen, som kallas mediumrc. 

    ```sql
    EXEC sp_addrolemember 'mediumrc', 'LoadingUser';
    ```
    > [!NOTE]
    > Klicka på [här](sql-data-warehouse-develop-concurrency.md#resource-classes) toolearn mer om samtidighet och resursen! 
    >

2. Ansluta toohello logisk server med hello nya autentiseringsuppgifter

    ![Logga in med ny inloggning](./media/sql-data-warehouse-get-started-tutorial/new-login.png)


## <a name="load-data-from-azure-blob-storage"></a>Läsa in data från Azure Blob Storage

Du är nu redo tooload data till data warehouse. Det här steget visar hur tooload New York City taxi cab data från en offentlig Azure storage blob. 

- Ett vanligt sätt tooload data till SQL Data Warehouse är toofirst flytta hello data tooAzure blob-lagring och läsa in den i ditt data warehouse. toomake den enklare toounderstand hur tooload, har vi New York taxi cab-data som redan finns i en offentlig Azure storage blob. 

- För framtida bruk, toolearn hur tooget data-tooAzure blob-lagring eller tooload den direkt från din datakälla till SQL Data Warehouse finns hello [översikt över inläsning](sql-data-warehouse-overview-load.md).


### <a name="define-external-data"></a>Definiera externa data

1. Skapa en huvudnyckel. Du behöver bara toocreate en huvudnyckel en gång per databas. 

    ```sql
    CREATE MASTER KEY;
    ```

2. Definiera hello placering av hello Azure blob som innehåller data som hello taxi CAB-filen.  

    ```sql
    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
        TYPE = Hadoop,
        LOCATION = 'wasbs://2013@nytpublic.blob.core.windows.net/'
    );
    ```

3. Ange hello externt filformat

    Hej ```CREATE EXTERNAL FILE FORMAT``` kommandot är används toospecify formatet för filer som innehåller hello externa data. De innehåller text som avgränsas med ett eller flera tecken som kallas avgränsare. I demonstrationssyfte lagras hello taxi cab data som komprimerade data såväl som gzip komprimerade data.

    Kör följande T-SQL-kommandon toodefine två olika format: okomprimerade och komprimerade.

    ```sql
    CREATE EXTERNAL FILE FORMAT uncompressedcsv
    WITH (
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( 
            FIELD_TERMINATOR = ',',
            STRING_DELIMITER = '',
            DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        )
    );

    CREATE EXTERNAL FILE FORMAT compressedcsv
    WITH ( 
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( FIELD_TERMINATOR = '|',
            STRING_DELIMITER = '',
        DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        ),
        DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
    );
    ```

4.  Skapa ett schema för det externa filformatet. 

    ```sql
    CREATE SCHEMA ext;
    ```
5. Skapa hello externa tabeller. Dessa tabeller refererar till data som lagras i Azure Blob Storage. Kör följande T-SQL-kommandon toocreate hello flera externa tabeller som alla punkt toohello Azure blob vi definierats tidigare i vår extern datakälla.

```sql
    CREATE EXTERNAL TABLE [ext].[Date] 
    (
        [DateID] int NOT NULL,
        [Date] datetime NULL,
        [DateBKey] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DaySuffix] varchar(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeek] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfQuarter] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfYear] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfMonth] varchar(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Month] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Quarter] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [QuarterName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Year] char(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [YearName] char(7) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthYear] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MMYYYY] char(6) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FirstDayOfMonth] date NULL,
        [LastDayOfMonth] date NULL,
        [FirstDayOfQuarter] date NULL,
        [LastDayOfQuarter] date NULL,
        [FirstDayOfYear] date NULL,
        [LastDayOfYear] date NULL,
        [IsHolidayUSA] bit NULL,
        [IsWeekday] bit NULL,
        [HolidayUSA] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Date',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    );
    
    CREATE EXTERNAL TABLE [ext].[Geography]
    (
        [GeographyID] int NOT NULL,
        [ZipCodeBKey] varchar(10) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [County] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [City] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [State] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Country] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [ZipCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Geography',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0 
    );
        
    
    CREATE EXTERNAL TABLE [ext].[HackneyLicense]
    (
        [HackneyLicenseID] int NOT NULL,
        [HackneyLicenseBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HackneyLicenseCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'HackneyLicense',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    
    CREATE EXTERNAL TABLE [ext].[Medallion]
    (
        [MedallionID] int NOT NULL,
        [MedallionBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [MedallionCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Medallion',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    CREATE EXTERNAL TABLE [ext].[Time]
    (
        [TimeID] int NOT NULL,
        [TimeBKey] varchar(8) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HourNumber] tinyint NOT NULL,
        [MinuteNumber] tinyint NOT NULL,
        [SecondNumber] tinyint NOT NULL,
        [TimeInSecond] int NOT NULL,
        [HourlyBucket] varchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [DayTimeBucketGroupKey] int NOT NULL,
        [DayTimeBucket] varchar(100) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
    )
    WITH
    (
        LOCATION = 'Time',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    
    CREATE EXTERNAL TABLE [ext].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        LOCATION = 'Trip2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = compressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    CREATE EXTERNAL TABLE [ext].[Weather]
    (
        [DateID] int NOT NULL,
        [GeographyID] int NOT NULL,
        [PrecipitationInches] float NOT NULL,
        [AvgTemperatureFahrenheit] float NOT NULL
    )
    WITH
    (
        LOCATION = 'Weather2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
```

### <a name="import-hello-data-from-azure-blob-storage"></a>Importera hello data från Azure blob storage.

SQL Data Warehouse har stöd för en nyckelinstruktion som kallas CREATE TABLE AS SELECT (CTAS). Den här instruktionen skapas en ny tabell baserat på hello resultaten av en select-instruktion. hello ny tabell har hello samma kolumner och datatyper som hello resultaten av hello select-uttryck.  Här är en elegant sätt tooimport data från Azure blob storage till SQL Data Warehouse.

1. Kör det här skriptet tooimport dina data.

    ```sql
    CREATE TABLE [dbo].[Date]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Date]
    OPTION (LABEL = 'CTAS : Load [dbo].[Date]')
    ;
    
    CREATE TABLE [dbo].[Geography]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT * FROM [ext].[Geography]
    OPTION (LABEL = 'CTAS : Load [dbo].[Geography]')
    ;
    
    CREATE TABLE [dbo].[HackneyLicense]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[HackneyLicense]
    OPTION (LABEL = 'CTAS : Load [dbo].[HackneyLicense]')
    ;
    
    CREATE TABLE [dbo].[Medallion]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Medallion]
    OPTION (LABEL = 'CTAS : Load [dbo].[Medallion]')
    ;
    
    CREATE TABLE [dbo].[Time]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Time]
    OPTION (LABEL = 'CTAS : Load [dbo].[Time]')
    ;
    
    CREATE TABLE [dbo].[Weather]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Weather]
    OPTION (LABEL = 'CTAS : Load [dbo].[Weather]')
    ;
    
    CREATE TABLE [dbo].[Trip]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Trip]
    OPTION (LABEL = 'CTAS : Load [dbo].[Trip]')
    ;
    ```

2. Visa data som laddas.

   Du läser in flera GB data och komprimerar dem till högpresterande klustrade Columnstore-index. Kör följande fråga som använder en dynamisk vyer (av DMV: er) tooshow hello Hanteringsstatus hello belastningen hello. När du har startat hello frågan hämtar du en kaffe och en tilltugg medan SQL Data Warehouse stöder vissa frekventa lyft.
    
    ```sql
    SELECT
        r.command,
        s.request_id,
        r.status,
        count(distinct input_name) as nbr_files,
        sum(s.bytes_processed)/1024/1024/1024 as gb_processed
    FROM 
        sys.dm_pdw_exec_requests r
        INNER JOIN sys.dm_pdw_dms_external_work s
        ON r.request_id = s.request_id
    WHERE
        r.[label] = 'CTAS : Load [dbo].[Date]' OR
        r.[label] = 'CTAS : Load [dbo].[Geography]' OR
        r.[label] = 'CTAS : Load [dbo].[HackneyLicense]' OR
        r.[label] = 'CTAS : Load [dbo].[Medallion]' OR
        r.[label] = 'CTAS : Load [dbo].[Time]' OR
        r.[label] = 'CTAS : Load [dbo].[Weather]' OR
        r.[label] = 'CTAS : Load [dbo].[Trip]'
    GROUP BY
        r.command,
        s.request_id,
        r.status
    ORDER BY
        nbr_files desc, 
        gb_processed desc;
    ```

3. Visa alla systemfrågor.

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

4. Se hur dina data läses in i Azure SQL Data Warehouse.

    ![Visa inlästa data](./media/sql-data-warehouse-get-started-tutorial/see-data-loaded.png)


## <a name="improve-query-performance"></a>Förbättra prestanda för frågor

Det finns flera sätt tooimprove frågeprestanda och tooachieve hello snabba prestanda som SQL Data Warehouse är utformad tooprovide.  

### <a name="see-hello-effect-of-scaling-on-query-performance"></a>Se hello av skalning på frågeprestanda 

Enkelriktade tooimprove frågeprestanda är tooscale resurser genom att ändra hello DWU servicenivå för ditt informationslager. Varje tjänstenivå kostar mer, men du kan minska eller pausa resurser när som helst. 

I det här steget jämför du prestanda på två olika DWU-inställningar.

Först ska vi skala hello sizing ned too100 DWU så att vi kan få en uppfattning om hur en beräkningsnod kan utföra på en egen.

1. Gå toohello portal och välj ditt SQL Data Warehouse.

2. Välj skala hello SQL Data Warehouse-bladet. 

    ![Skala DW från portalen](./media/sql-data-warehouse-get-started-tutorial/scale-dw.png)

3. Skala hello prestanda liggande too100 DWU och klicka på Spara.

    ![Skala och spara](./media/sql-data-warehouse-get-started-tutorial/scale-and-save.png)

4. Vänta medan din skala åtgärden toofinish.

    > [!NOTE]
    > Frågor kan inte köras medan du ändrar hello skala. Skalning **stoppar** dina frågor som körs för närvarande. Du kan starta om dem när hello-åtgärden har slutförts.
    >
    
5. Göra en genomsökning åtgärd på hello resa data, välja hello översta miljoner poster för alla hello-kolumner. Om du snabbt är gärna toomove på du ledigt tooselect färre rader. Anteckna hello tid det tar toorun för den här åtgärden.

    ```sql
    SELECT TOP(1000000) * FROM dbo.[Trip]
    ```
6. Skala ditt informationslager tillbaka too400 DWU. Kom ihåg att varje 100 DWU är att lägga till en annan compute-nod tooyour Azure SQL Data Warehouse.

7. Kör hello frågan igen! Du bör märka en stor skillnad. 

    > [!NOTE]
    > Eftersom hello frågan returnerar stora mängder data, kanske hello bandbreddstillgänglighet av hello-dator som kör SSMS flaskhalsar. Det kan leda till att du inte ser några prestandaförbättringar!

> [!NOTE]
> Eftersom SQL Data Warehouse använder massiv parallell bearbetning (MPP). Frågor som skanna eller utföra analysfunktioner på miljoner rader upplevelse hello true kraften i Azure SQL Data Warehouse.
>

### <a name="see-hello-effect-of-statistics-on-query-performance"></a>Se hello av statistik på frågeprestanda

1. Köra en fråga som kopplar hello datumtabell med hello resa tabell

    ```sql
    SELECT TOP (1000000) 
        dt.[DayOfWeek],
        tr.[MedallionID],
        tr.[HackneyLicenseID],
        tr.[PickupTimeID],
        tr.[DropoffTimeID],
        tr.[PickupGeographyID],
        tr.[DropoffGeographyID],
        tr.[PickupLatitude],
        tr.[PickupLongitude],
        tr.[PickupLatLong],
        tr.[DropoffLatitude],
        tr.[DropoffLongitude],
        tr.[DropoffLatLong],
        tr.[PassengerCount],
        tr.[TripDurationSeconds],
        tr.[TripDistanceMiles],
        tr.[PaymentType],
        tr.[FareAmount],
        tr.[SurchargeAmount],
        tr.[TaxAmount],
        tr.[TipAmount],
        tr.[TollsAmount],
        tr.[TotalAmount]
    FROM [dbo].[Trip] as tr
        JOIN dbo.[Date] as dt
        ON  tr.DateID = dt.DateID
    ```

    Den här frågan tar ett tag eftersom SQL Data Warehouse har tooshuffle data innan den kan utföra hello koppling. Kopplingar har inte tooshuffle data om de är utformad toojoin data i hello samma sätt som den distribueras. Det är ett djupare ämne. 

2. Statistik gör skillnad. 
3. Kör den här instruktionen toocreate statistik på hello kopplingskolumner.

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

    > [!NOTE]
    > SQL DW hanterar inte statistik automatiskt åt dig. Statistik är viktigt för frågeprestanda och vi rekommenderar att du skapar och uppdaterar statistik.
    > 
    > **Du får hello utnyttja genom att låta statistik för kolumner som ingår i kopplingar, kolumner som används i hello där satsen och kolumner finns i GROUP BY.**
    >

3. Kör hello frågan från förutsättningarna igen och notera eventuella skillnader i prestanda. Hello skillnader i prestanda för frågor kommer inte att som drastiskt som skala upp, bör du se en påskynda. 

## <a name="next-steps"></a>Nästa steg

Du är nu redo tooquery och utforska. Se våra bästa praxis och tips.

Om du är klar utforska hello dag, se till att toopause din instans! I produktion, du får mycket stora besparingar av du pausar och skalning toomeet dina affärsbehov.

![Pausa](./media/sql-data-warehouse-get-started-tutorial/pause.png)

## <a name="useful-readings"></a>Bra information

[Concurrency and workload management][] (Hantering av samtidiga körningar och arbetsbelastningar)

[Metodtips för Azure SQL Data Warehouse][]

[Query Monitoring][] (Frågeövervakning)

[Top 10 Best Practices for Building a Large Scale Relational Data Warehouse][] (Tio rekommendationer för att skapa ett storskaligt relationsinformationslager)

[Migrera Data tooAzure SQL Data Warehouse][]

[Concurrency and workload management]: sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example (Hantering av samtidiga körningar och arbetsbelastningar)
[Metodtips för Azure SQL Data Warehouse]: sql-data-warehouse-best-practices.md#hash-distribute-large-tables
[Query Monitoring]: sql-data-warehouse-manage-monitor.md (Frågeövervakning)
[Top 10 Best Practices for Building a Large Scale Relational Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2013/09/16/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse/ (Tio rekommendationer för att skapa ett storskaligt relationsinformationslager)
[Migrera Data tooAzure SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/



[!INCLUDE [Additional Resources](../../includes/sql-data-warehouse-article-footer.md)]

<!-- Internal Links -->
[krav]: sql-data-warehouse-get-started-tutorial.md#prerequisites

<!--Other Web references-->
[Visual Studio]: https://www.visualstudio.com/
[SQL Server Management Studio]: https://msdn.microsoft.com/en-us/library/mt238290.aspx
