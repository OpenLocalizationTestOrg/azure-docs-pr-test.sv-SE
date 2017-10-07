---
title: "hello Team av vetenskapliga data i praktiken: använda SQL Data Warehouse | Microsoft Docs"
description: "Processen för avancerade analyser och teknik i åtgärd"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 88ba8e28-0bd7-49fe-8320-5dfa83b65724
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;hangzh;weig
ms.openlocfilehash: b1b6371583a023d32e33db59464cafd8c3b767d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action-using-sql-data-warehouse"></a>hello Team av vetenskapliga data i praktiken: använda SQL Data Warehouse
I den här självstudiekursen vi beskriver hur du skapar och distribuerar en maskininlärningsmodell med hjälp av SQL Data Warehouse (SQL DW) för en offentligt tillgängliga dataset--hello [NYC Taxi resor](http://www.andresmh.com/nyctaxitrips/) dataset. hello binär klassificering modellen konstrueras beräknar huruvida ett tips betalat för en resa och modeller för multiklass-baserad klassificering och regression beskrivs också som förutsäga hello distribution för hello tips belopp betald.

hello proceduren följer hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) arbetsflöde. Visar vi hur toosetup vetenskap datamiljö, hur tooload hello data till SQL DW och hur använda SQL DW eller en bärbar dator IPython tooexplore hello data och tekniker funktioner toomodel. Sedan visar vi hur toobuild och distribuera en modell med Azure Machine Learning.

## <a name="dataset"></a>hello NYC Taxi resor dataset
hello NYC Taxi resa data består av cirka 20GB komprimerat CSV-filer (~ 48GB okomprimerade), registrera mer än 173 miljoner enskilda resor och hello priser för varje resa. Varje resa post omfattar hello hämtning och Samlingsbibliotek platser och gånger anonym hacka () körkortsnummer och hello medallion (taxi's unikt id) nummer. hello data omfattar alla resor hello år 2013 och finns i följande två datamängder för varje månad hello:

1. Hej **trip_data.csv** filen innehåller resa information, till exempel antal passagerare, hämtning och dropoff, resa varaktighet och resa längd. Här följer några Exempelposter:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. Hej **trip_fare.csv** filen innehåller information om hello avgiften betalat för varje förflyttning, till exempel betalningssätt, avgiften belopp, tillägg och skatter, tips och vägtullar och hello totalbelopp betald. Här följer några Exempelposter:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Hej **Unik nyckel** används toojoin resa\_data och resa\_avgiften består av följande tre fält hello:

* medallion,
* hacka\_licens och
* hämtning\_datetime.

## <a name="mltasks"></a>Åtgärda tre typer av förutsägelse uppgifter
Vi formulera tre förutsägelse problem utifrån hello *tips\_belopp* tooillustrate tre typer av modeling uppgifter:

1. **Binär klassificering**: toopredict om huruvida ett tips har betalat för en resa, d.v.s. en *tips\_belopp* som är större än 0 är ett positivt exempel, när en *tips\_belopp* $0 är ett exempel på negativt.
2. **Multiklass-baserad klassificering**: toopredict hello mängd tips betalat för hello resa. Vi dela hello *tips\_belopp* i fem lagerplatser eller klasser:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. **Regression uppgiften**: toopredict hello tips betalats för en transport.  

## <a name="setup"></a>Konfigurera hello Azure datavetenskap miljö för avancerade analyser
tooset konfigurera Azure datavetenskap miljön, Följ dessa steg.

**Skapa din egen Azure blob storage-konto**

* När du konfigurerar din egen Azure-blobblagring Välj geoplats för din Azure-blobblagring i eller så mycket som möjligt för**södra centrala USA**, vilket är där hello NYC Taxi data lagras. hello data kopieras med hjälp av AzCopy från hello offentliga behållare tooa blobblagringsbehållare i ditt eget lagringskonto. hello närmare din Azure blob storage är tooSouth centrala USA, hello snabbare (steg 4) kommer att slutföra uppgiften.
* toocreate din egen Azure storage-konto, följ hello stegen som beskrivs i [om Azure storage-konton](../storage/common/storage-create-storage-account.md). Vara säker på att toomake not hello värden för följande lagringskontouppgifter som de behövs längre fram i den här genomgången.
  
  * **Lagringskontonamnet**
  * **Lagringskontonyckel**
  * **Behållarens namn** (som du vill hello data toobe lagras i hello Azure blob storage)

**Etablera din Azure SQL DW-instans.**
Följ hello dokumentation på [skapa ett SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) tooprovision en instans av SQL Data Warehouse. Kontrollera att du gör här ska på hello följande SQL Data Warehouse-autentiseringsuppgifter som ska användas i senare steg.

* **Servernamnet**: <server Name>. database.windows.net
* **Namn på SQLDW (Database)**
* **Användarnamn**
* **Lösenord**

**Installera Visual Studio och SQL Server Data Tools.** Instruktioner finns i [installera Visual Studio 2015 och SSDT (SQL Server Data Tools) för SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Ansluta tooyour Azure SQL DW med Visual Studio.** Instruktioner finns i steg 1 och 2 i [ansluta tooAzure SQL Data Warehouse med Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).

> [!NOTE]
> Kör hello följande SQL-frågan på hello databasen du skapade i ditt SQL Data Warehouse (i stället för hello frågan som angavs i steg 3 i hello ansluta avsnittet) för**skapa en huvudnyckel**.
> 
> 

    BEGIN TRY
           --Try toocreate hello master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If hello master key exists, do nothing
    END CATCH;

**Skapa en Azure Machine Learning-arbetsytan under din Azure-prenumeration.** Instruktioner finns i [skapa en arbetsyta för Azure Machine Learning](machine-learning-create-workspace.md).

## <a name="getdata"></a>Läs in hello data till SQL Data Warehouse
Öppna en Windows PowerShell-Kommandotolken. Kör hello följande PowerShell-kommandon till toodownload hello exempel SQL skriptfiler som vi dela med dig på GitHub tooa lokal katalog som du anger med hello parametern *- DestDir*. Du kan ändra hello värdet för parametern *- DestDir* tooany lokal katalog. Om *- DestDir* finns inte, kommer att skapas av hello PowerShell-skript.

> [!NOTE]
> Du kan behöva för**kör som administratör** när du kör följande PowerShell-skript om hello din *DestDir* directory behöver administratören behörighet toocreate eller toowrite tooit.
> 
> 

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

När du har körts, ändras den aktuella arbetskatalogen för*- DestDir*. Du bör kunna toosee skärmen nedan:

![][19]

I din *- DestDir*, köra följande PowerShell-skript i administratörsläge hello:

    ./SQLDW_Data_Import.ps1

När hello PowerShell-skript körs för hello första gången blir du ombedd tooinput hello information från din Azure SQL DW och Azure blob storage-konto. När det här PowerShell-skriptet har slutförts kommer körs första gången, hello autentiseringsuppgifter för hello du indata har skrivits tooa konfigurationsfilen SQLDW.conf i hello finns arbetskatalogen. hello har framtida körning av PowerShell-skriptfil hello alternativet tooread alla nödvändiga parametrar från konfigurationsfilen. Om du behöver toochange vissa parametrar, kan du välja tooinput hello parametrar på hello-skärmen vid uppmaning om att ta bort den här konfigurationsfilen och mata in hello parametervärdena som efterfrågas eller toochange hello parametervärden genom att redigera hello SQLDW.conf filen i din *- DestDir* directory.

> [!NOTE]
> I ordning tooavoid schema namnet står i konflikt med de som redan finns i din Azure SQL DW vid läsning av parametrar direkt från hello SQLDW.conf fil, läggs ett 3-siffriga slumptal toohello schemanamnet från hello SQLDW.conf fil som hello schemat för standardnamnet för varje körning. hello PowerShell-skript kan efterfrågas ett schemanamn: hello namn kan anges efter användaren gottfinnande.
> 
> 

Detta **PowerShell-skript** filen Slutför hello följande uppgifter:

* **Hämtar och installerar AzCopy**om AzCopy inte redan är installerad
  
        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
               Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }
* **Kopierar data tooyour privata blob storage-konto** från hello offentlig blob med AzCopy
  
        Write-Host "AzCopy is copying data from public blob tooyo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account tooverify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob tooyour storage account) takes $total_seconds seconds." -ForegroundColor "Green"
* **Läser in data med Polybase (genom att köra LoadDataToSQLDW.sql) tooyour Azure SQL DW** från privata blob storage-konto med hello följande kommandon.
  
  * Skapa ett schema
    
          EXEC (''CREATE SCHEMA {schemaname};'');
  * Skapa en databasbegränsade autentiseringsuppgift
    
          CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
          WITH IDENTITY = ''asbkey'' ,
          Secret = ''{StorageAccountKey}''
  * Skapa en extern datakälla för ett Azure storage-blob
    
          CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;
    
          CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;
  * Skapa ett externt filformat för en csv-fil. Fälten avgränsas med hello vertikalstrecket att data är okomprimerade.
    
          CREATE EXTERNAL FILE FORMAT {csv_file_format}
          WITH
          (   
              FORMAT_TYPE = DELIMITEDTEXT,
              FORMAT_OPTIONS  
              (
                  FIELD_TERMINATOR ='','',
                  USE_TYPE_DEFAULT = TRUE
              )
          )
          ;
  * Skapa extern avgiften och resa tabeller för NYC taxi dataset i Azure blob storage.
    
          CREATE EXTERNAL TABLE {external_nyctaxi_fare}
          (
              medallion varchar(50) not null,
              hack_license varchar(50) not null,
              vendor_id char(3),
              pickup_datetime datetime not null,
              payment_type char(3),
              fare_amount float,
              surcharge float,
              mta_tax float,
              tip_amount float,
              tolls_amount float,
              total_amount float
          )
          with (
              LOCATION    = ''/nyctaxifare/'',
              DATA_SOURCE = {nyctaxi_fare_storage},
              FILE_FORMAT = {csv_file_format},
              REJECT_TYPE = VALUE,
              REJECT_VALUE = 12     
          )  

            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                   medallion varchar(50) not null,
                   hack_license varchar(50)  not null,
                   vendor_id char(3),
                   rate_code char(3),
                   store_and_fwd_flag char(3),
                   pickup_datetime datetime  not null,
                   dropoff_datetime datetime,
                   passenger_count int,
                   trip_time_in_secs bigint,
                   trip_distance float,
                   pickup_longitude varchar(30),
                   pickup_latitude varchar(30),
                   dropoff_longitude varchar(30),
                   dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - Läs in data från externa tabeller i Azure blob storage tooSQL Data Warehouse

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - Skapa data exempeltabell (NYCTaxi_Sample) och infoga data tooit från att välja SQL-frågor för hello resa och avgiften tabeller. (Vissa steg i den här genomgången måste toouse denna exempeltabell.)

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

hello geografisk plats för dina lagringskonton påverkar inläsningstiden.

> [!NOTE]
> Beroende på hello geografiska placeringen för privata blob storage-konto, hello processen att kopiera data från en offentlig blob tooyour privata storage-konto kan tar ungefär 15 minuter eller ännu längre och hello bearbeta inläsning av data från ditt lagringskonto tooyour Azure SQL DW ta 20 minuter eller längre.  
> 
> 

Du måste toodecide vad gör om du har dubbla källa och målfiler.

> [!NOTE]
> Om hello CSV-filer toobe kopieras från hello offentlig blob storage tooyour privata blob storage-konto redan finns i ditt privata blob storage-konto, AzCopy tillfrågas du om du vill att toooverwrite dem. Om du inte vill toooverwrite dem, inkommande  **n**  när du tillfrågas. Om du vill toooverwrite **alla** av dem, ange **en** när du tillfrågas. Du kan också ange **y** toooverwrite CSV-filer separat.
> 
> 

![Rita #21][21]

Du kan använda dina egna data. Om data finns i den lokala datorn i ditt program i verkligheten kan använda du fortfarande AzCopy tooupload lokala data tooyour privat Azure-blobblagring. Behöver du bara toochange hello **källa** plats, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, i hello AzCopy-kommandot för hello PowerShell-skript fil toohello lokala katalog som innehåller dina data.

> [!TIP]
> Om data finns redan i din privata Azure blob storage i verkligheten programmet kan du hoppa över hello AzCopy steg i hello PowerShell-skript och direkt överföra hello data tooAzure SQL DW. Detta kräver ytterligare redigerar av hello skriptet tootailor det toohello formatet för dina data.
> 
> 

Detta Powershell-skript ansluts också hello Azure SQL DW information till hello utforskning exempel datafiler SQLDW_Explorations.sql, SQLDW_Explorations.ipynb och SQLDW_Explorations_Scripts.py så att dessa tre filer är klar toobe försökte omedelbart När hello PowerShell-skriptet har slutförts.

När du har körts, visas skärmen som nedan:

![][20]

## <a name="dbexplore"></a>Datagranskning och funktionen teknikerna i Azure SQL Data Warehouse
I det här avsnittet vi utföra data från kartläggning av naturresurser och funktionen generation genom att köra SQL-frågor mot Azure SQL DW direkt med **Visual Studio Data Tools**. Alla SQL-frågor som används i det här avsnittet hittar du i hello exempelskript med namnet *SQLDW_Explorations.sql*. Den här filen har redan använts hämtade tooyour lokal katalog med hello PowerShell-skript. Du kan också hämta det från [GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql). Men hello-filen i GitHub har inte hello Azure SQL DW information inkopplad.

Ansluta tooyour Azure SQL DW Visual Studio med hello SQL DW-inloggningsnamnet och lösenordet och öppna hello **SQL Object Explorer** tooconfirm hello-databasen och tabeller har importerats. Hämta hello *SQLDW_Explorations.sql* fil.

> [!NOTE]
> tooopen en Parallel Data Warehouse (PDW) frågeredigeraren använda hello **ny fråga** kommandot när din PDW har valts i hello **SQL Object Explorer**. hello SQL-frågan standardredigeraren stöds inte av PDW.
> 
> 

Här följer hello typ av data från kartläggning av naturresurser och funktionen generation uppgifter som utförs i det här avsnittet:

* Utforska data distributioner av ett fåtal fält i olika tidsfönster.
* Undersök data quality hello longitud och latitud fält.
* Generera binära och multiklass-baserad klassificeringsetiketter baserat på hello **tips\_belopp**.
* Generera funktioner och beräkning eller jämförelse resa avstånd.
* Koppla hello två tabeller och extrahera ett slumpmässigt prov som kommer att använda toobuild modeller.

### <a name="data-import-verification"></a>Importera dataverifieringen
De här frågorna ger en snabb kontroll av hello antalet rader och kolumner i hello tabeller fylls tidigare med Polybases parallella bulk importera

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Utdata:** du bör få 173,179,759 rader och 14 kolumner.

### <a name="exploration-trip-distribution-by-medallion"></a>Undersökning: Resa distribution av medallion
Den här exempelfråga identifierar hello medallions (taxi numbers) som slutförda mer än 100 resor inom en angiven tidsperiod. hello-frågan skulle dra nytta av hello partitionerad tabellåtkomst eftersom den är villkorad av hello partitionsschemat för **hämtning\_datetime**. Frågar hello fullständiga datauppsättningen blir också användning av hello partitionerad tabell och/eller index-sökning.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Utdata:** hello frågan ska returnera en tabell med rader att ange hello 13,369 medallions (taxibilar) och hello antalet resa slutförts av dem i 2013. hello sista kolumnen innehåller hello antal hello antalet turer som slutförts.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Undersökning: Resa distribution av medallion och hack_license
Det här exemplet identifierar hello medallions (taxi numbers) och hack_license siffror (drivrutiner) som slutförda mer än 100 resor inom en angiven tidsperiod.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Utdata:** hello frågan ska returnera en tabell med 13,369 rader att ange hello 13,369 car och drivrutin ID: N som har slutfört mer som 100 resor i 2013. hello sista kolumnen innehåller hello antal hello antalet turer som slutförts.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Data utvärdering: Verifiera poster med felaktigt longituden eller latituden
Det här exemplet undersöker om någon av hello longituden eller latituden fält antingen innehåller ett ogiltigt värde (radian grader ska vara mellan-90 och 90), eller ha (0, 0) koordinater.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Utdata:** hello frågan returnerar 837,467 resor som har ogiltiga longituden eller latituden fält.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Undersökning: Lutad kontra inte lutad resor distribution
Det här exemplet hittar hello antalet turer som har lutad kontra hello tal som inte har lutad i en angiven tidsperiod (eller hello fullständiga datauppsättningen om täcker hello fullständig år som det ställs in här). Den här distributionen visar hello binära etikett distribution toobe senare används för binär klassificering modellering.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Utdata:** hello frågan ska returnera hello följande tips frekvenser för hello år 2013: 90,447,622 lutad och 82,264,709 inte lutad.

### <a name="exploration-tip-classrange-distribution"></a>Undersökning: Tips klass-intervallet distribution
Det här exemplet beräknar hello distribution av tips intervall i en given tidpunkt tidsperiod (eller i hello fullständiga datauppsättningen om täcker hello fullständig år). Detta är hello distribution av hello etikett klasser som ska användas senare för multiklass-baserad klassificering modellering.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**Utdata:**

| tip_class | tip_freq |
| --- | --- |
| 1 |82230915 |
| 2 |6198803 |
| 3 |1932223 |
| 0 |82264625 |
| 4 |85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Undersökning: Beräkna och jämföra resa avstånd
Det här exemplet konverterar hello hämtning och Samlingsbibliotek longitud och latitud tooSQL geografi pekar beräknar hello resa avståndet med SQL geografi punkter skillnaden och returnerar ett slumpvist urval av hello resultat för jämförelse. hello exempel begränsar hello resultat toovalid samordnar bara använder hello kvalitet assessment datafrågor omfattas tidigare.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function toocalculate hello direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert tooradians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert toomiles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>Funktionen tekniker med hjälp av SQL-funktioner
SQL-funktioner kan ibland vara ett effektivt alternativ för funktionen tekniker. Vi har definierat ett SQL-funktionen toocalculate hello direkt avstånd mellan hello hämtning och dropoff platser i den här genomgången. Du kan köra följande SQL-skript i hello **Visual Studio Data Tools**.

Här är hello SQL-skript som definierar hello avståndet funktion.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate hello direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert tooradians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert toomiles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

Här är ett exempel toocall denna funktion toogenerate funktioner i SQL-frågan:

    -- Sample query toocall hello function toocreate features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Utdata:** den här frågan genererar en tabell (med 2,803,538 rader) med hämtning och dropoff latituderna och longitudes och hello motsvarande direkt avstånd i mil. Här följer hello resultat för första 3 rader:

|  | pickup_latitude | pickup_longitude | dropoff_latitude | dropoff_longitude | DirectDistance |
| --- | --- | --- | --- | --- | --- |
| 1 |40.731804 |-74.001083 |40.736622 |-73.988953 |.7169601222 |
| 2 |40.715794 |-74,010635 |40.725338 |-74.00399 |.7448343721 |
| 3 |40.761456 |-73.999886 |40.766544 |-73.988228 |0.7037227967 |

### <a name="prepare-data-for-model-building"></a>Förbered data för modellskapandet
hello följande fråga kopplingar hello **nyctaxi\_resa** och **nyctaxi\_avgiften** tabeller, genererar en binär klassificering etikett **lutad**, flera klassen klassificering etikett **tips\_klassen**, och extraherar ett sampel från hello fullständigt sammanfogade dataset. hello provtagning görs genom att hämta en delmängd av hello resor baserat på pickup tid.  Den här frågan kan kopieras och klistras in direkt i hello [Azure Machine Learning Studio](https://studio.azureml.net) [importera Data] [ import-data] -modulen för direkt datapåfyllning från hello SQL-databasinstans i Azure. hello frågan utesluter poster med fel (0, 0) koordinater.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

När du är klar tooproceed tooAzure Machine Learning, kan du antingen:  

1. Spara hello slutliga SQL tooextract och exempel hello data och kopiera / klistra in hello fråga direkt till en [importera Data] [ import-data] modul i Azure Machine Learning, eller
2. Kvarstår hello provtagning och bakåtkompilerade data som du planerar toouse för modellen bygga i en ny SQL DW tabell och använda hello ny tabell i hello [importera Data] [ import-data] modul i Azure Machine Learning. hello PowerShell-skript i tidigare steg har gjort det åt dig. Du kan läsa direkt från den här tabellen i hello importera Data modul.

## <a name="ipnb"></a>Datagranskning och funktionen tekniker i IPython anteckningsbok
Vi kommer att utföra datagranskning och funktionen generation använder båda Python och SQL-frågor mot hello SQL DW skapade tidigare i det här avsnittet. En exempel IPython bärbar dator med namnet **SQLDW_Explorations.ipynb** och en Python-skriptfil **SQLDW_Explorations_Scripts.py** har hämtat tooyour lokal katalog. De är också tillgängliga på [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). Dessa två filer är identiska i Python-skript. hello Python skriptfilen tillhandahålls tooyou om du inte har en bärbar dator IPython-server. Dessa två exempel Python filer är utformade under **Python 2.7**.

hello Azure SQL DW-information som behövs i hello prov IPython anteckningsboken och hello Python skriptet filen hämtade tooyour lokala datorn har anslutits med hello PowerShell-skript tidigare. De är körbara utan några ändringar.

Om du redan har installerat en AzureML-arbetsyta, du direkt överför hello exempel IPython anteckningsboken toohello AzureML IPython anteckningsboken service och starta körs. Här följer hello steg tooupload tooAzureML IPython anteckningsboken tjänsten:

1. Logga in tooyour AzureML arbetsytan, klickar du på ”Studio” hello överst och klicka på ”bärbara datorer” hello vänster på webbsidan hello.
   
    ![Rita #22][22]
2. Klicka på ”ny” i hello nedre vänstra hörnet på webbsidan hello och välj ”Python 2”. Sedan anger en bärbar dator toohello namn och klickar på hello markerat toocreate hello nya tomma IPython bärbar dator.
   
    ![Rita #23][23]
3. Klicka på symbolen för hello ”Jupyter” på hello översta till vänster i hello ny IPython anteckningsbok.
   
    ![Rita #24][24]
4. Dra och släpp hello exempel IPython anteckningsboken toohello **träd** i din AzureML IPython anteckningsboken tjänsten och klickar på **överför**. Hello exempel IPython anteckningsboken kommer sedan att överförda toohello AzureML IPython anteckningsboken service.
   
    ![Rita #25][25]

I ordning toorun hello exempel IPython bärbar dator eller Hej Python skriptfilen, hello följande Python-paket som behövs. Om du använder hello AzureML IPython anteckningsboken service har paketen installerats före.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

hello rekommenderas sekvens när du skapar avancerade analytiska lösningar på AzureML med stora mängder data är hello följande:

* Läs i ett litet antal hello data till en ram i minnet data.
* Utföra vissa visualiseringar och explorations med hello samplade data.
* Experiment med funktionen tekniker med hello exempeldata.
* För större datagranskning datamanipulering och funktionen konstruktion, kan du använda Python tooissue SQL-frågor direkt mot hello SQL DW.
* Bestäm hello exempel storlek toobe lämpar sig för Azure Machine Learning modellskapandet.

hello följande är några datagranskning datavisualisering exempel, och funktionen engineering. Mer data explorations kan hittas i hello exemplet IPython anteckningsboken och hello Python exempelfilen.

### <a name="initialize-database-credentials"></a>Initiera Databasautentiseringsuppgifter
Initiera databasen anslutningsinställningarna i hello följande variabler:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Skapa databasanslutning
Här är hello anslutningssträngen som skapar hello toohello databas.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Rapporten antalet rader och kolumner i tabellen < nyctaxi_trip >
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* Totalt antal rader = 173179759  
* Totalt antal kolumner = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Rapporten antalet rader och kolumner i tabellen < nyctaxi_fare >
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* Totalt antal rader = 173179759  
* Totalt antal kolumner = 11

### <a name="read-in-a-small-data-sample-from-hello-sql-data-warehouse-database"></a>Läs i ett litet datasampel från hello SQL Data Warehouse-databas
    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time tooread hello sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Tid tooread hello exempeltabell är 14.096495 sekunder.  
Antal rader och kolumner hämtas = (1000, 21).

### <a name="descriptive-statistics"></a>Beskrivande statistik
Nu är du redo tooexplore hello provtagning data. Vi börjar med att titta på vissa beskrivande statistik för hello **resa\_avstånd** (eller andra fält som du väljer toospecify).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Visualiseringen: Exempel på ritytans
Vi titta på hello Låddiagram för hello resa avståndet toovisualize hello quantiles.

    df1.boxplot(column='trip_distance',return_type='dict')

![Rita #1][1]

### <a name="visualization-distribution-plot-example"></a>Visualiseringen: Distribution ritytans exempel
Områden som visualiserar hello distribution och ett histogram för hello prov resa avstånd.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Rita #2][2]

### <a name="visualization-bar-and-line-plots"></a>Visualiseringen: Menyraden och rad områden
I det här exemplet vi bin hello resa avståndet i fem lagerplatser och visualisera hello diskretisering resultat.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Vi kan ritas hello ovan bin distribution i ett fält eller rad ritytans med:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Rita #3][3]

och

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Rita #4][4]

### <a name="visualization-scatterplot-examples"></a>Visualiseringen: Scatterplot exempel
Visar vi punktdiagram ritytans mellan **resa\_tid\_i\_sek** och **resa\_avstånd** toosee om det finns några korrelation

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Rita #6][6]

På liknande sätt kan vi Kontrollera hello förhållandet mellan **hastighet\_kod** och **resa\_avstånd**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Rita #8][8]

### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Datagranskning på samplade data med hjälp av SQL-frågor i IPython anteckningsbok
I det här avsnittet förklarar vi data distributioner med hello provtagning data som sparas i hello ny tabell som vi skapade ovan. Observera att liknande explorations kan utföras med hjälp av hello ursprungliga tabeller.

#### <a name="exploration-report-number-of-rows-and-columns-in-hello-sampled-table"></a>Undersökning: Rapporten antalet rader och kolumner i hello provtagning tabell
    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Undersökning: Lutad ej utlöses Distribution
    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Undersökning: Tips klassen distribution
    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-hello-tip-distribution-by-class"></a>Undersökning: Rita hello tips distribution av klassen
    tip_class_dist['tip_freq'].plot(kind='bar')

![Rita #26][26]

#### <a name="exploration-daily-distribution-of-trips"></a>Undersökning: Dagliga distribution av resor
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Undersökning: Resa distribution per medallion
    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Undersökning: Resa distribution av medallion och hackare licens
    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Undersökning: Fördelning av resa
    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Undersökning: Resa avståndet distribution
    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Undersökning: Betalning typen distribution
    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-hello-final-form-of-hello-featurized-table"></a>Kontrollera hello slutliga form av hello featurized tabell
    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Bygga modeller i Azure Machine Learning
Vi är nu redo tooproceed toomodel byggnad och distribution av modellen i [Azure Machine Learning](https://studio.azureml.net). hello data är klar toobe som används i någon av hello förutsägelse problem som konstaterats tidigare, nämligen:

1. **Binär klassificering**: toopredict om huruvida ett tips har betalat för en resa.
2. **Multiklass-baserad klassificering**: toopredict hello mängd tips betald, enligt toohello tidigare definierade klasserna.
3. **Regression uppgiften**: toopredict hello tips betalats för en transport.  

toobegin Hej modeling Övning, logga in tooyour **Azure Machine Learning** arbetsytan. Om du inte har skapat machine learning-arbetsytan finns [skapa en arbetsyta för Azure ML](machine-learning-create-workspace.md).

1. tooget igång med Azure Machine Learning finns [vad är Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)
2. Logga in för[Azure Machine Learning Studio](https://studio.azureml.net).
3. startsidan för hello Studio innehåller en mängd information, videor, självstudier, länkar toohello moduler referens och andra resurser. Mer information om Azure Machine Learning finns hello [Azure Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).

En typisk träningsexperiment består av hello följande steg:

1. Skapa en **+ ny** experiment.
2. Hämta hello data till Azure ML.
3. Förbearbeta, transformera och manipulera hello data efter behov.
4. Generera funktioner efter behov.
5. Dela upp hello data i utbildning / / verifieringstesterna datauppsättningar (eller ha separata datauppsättningar för varje).
6. Välj en eller flera maskininlärningsalgoritmer beroende på hello learning problemet toosolve. T.ex. binär klassificering, multiklass-baserad klassificering, regression.
7. Träna en eller flera modeller som använder hello utbildning dataset.
8. Poängsätta hello validering dataset med hello tränade modeller.
9. Utvärdera hello modeller toocompute hello relevanta mätvärden för hello learning problem.
10. Finjustera hello modeller och välj hello bästa modellen toodeploy.

I den här övningen har vi redan utforskade och utformad hello data i SQL Data Warehouse och valt hello exempel storlek tooingest i Azure ML. Här är hello proceduren toobuild en eller flera av hello förutsägelse modeller:

1. Hämta hello data till Azure ML med hello [importera Data] [ import-data] modulen är tillgängliga i hello **Data ingående och utgående** avsnitt. Mer information finns i hello [importera Data] [ import-data] modulsida referens.
   
    ![Azure ML-importera Data][17]
2. Välj **Azure SQL Database** som hello **datakällan** i hello **egenskaper** panelen.
3. Ange hello DNS-namn i hello **Databasservernamnet** fältet. Format:`tcp:<your_virtual_machine_DNS_name>,1433`
4. Ange hello **databasnamnet** i hello motsvarande fält.
5. Ange hello *användarnamn för SQL* i hello **Server användarkontonamnet**, och hello *lösenord* i hello **serverlösenord**.
6. Kontrollera hello **acceptera alla servercertifikat** alternativet.
7. I hello **databasfrågan** redigera textområde, klistra in vilka extrakt hello nödvändigt databasfält (inklusive eventuella beräknade fält, till exempel hello etiketter) och ned exempel hello data toohello önskad provtagning hello-fråga.

Ett exempel på en binär klassificering experiment läsning av data direkt från hello SQL Data Warehouse-databas är i hello figuren nedan (Kom ihåg tooreplace hello namnen nyctaxi_trip och nyctaxi_fare av hello schemat namn och hello tabellnamn som du använde i din genomgång). Liknande experiment kan konstrueras för multiklass-baserad klassificering och regressionsproblem.

![Azure ML Train][10]

> [!IMPORTANT]
> I hello modellering extrahering av data och provtagning frågan exempel finns i föregående avsnitt **alla etiketter för hello tre modellering övningarna tas med i hello fråga**. Är ett viktigt steg i (obligatoriskt) i varje hello modeling övningarna för**undanta** hello onödiga etiketter för hello andra två problem och andra **mål minnesläckor**. Till exempel när du använder binär klassificering, använda hello etikett **lutad** och utelämna hello fält **tips\_klassen**, **tips\_belopp**, och **totala\_belopp**. hello senare är målet minnesläckor eftersom de innebär hello tips betalda.
> 
> tooexclude alla onödiga kolumner eller mål minnesläckor du kan använda hello [Välj kolumner i datauppsättning] [ select-columns] modul eller hello [redigera Metadata] [ edit-metadata]. Mer information finns i [Välj kolumner i datauppsättning] [ select-columns] och [redigera Metadata] [ edit-metadata] referera sidor.
> 
> 

## <a name="mldeploy"></a>Distribuera modeller i Azure Machine Learning
När modellen är klar, kan du enkelt distribuera det som en webbtjänst direkt från hello experiment. Mer information om hur du distribuerar Azure ML web services finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).

toodeploy en ny webbtjänst, måste du:

1. Skapa ett bedömningsprofil experiment.
2. Distribuera hello-webbtjänsten.

toocreate poängsättning av ett experiment från en **avslutad** utbildning experiment, klickar du på **skapa BEDÖMNINGEN EXPERIMENTERA** i hello lägre Åtgärdsfältet.

![Azure bedömningen][18]

Azure Machine Learning försöker toocreate ett bedömningsprofil experiment baserat på hello komponenter i hello träningsexperiment. I synnerhet att:

1. Spara hello tränad modell och ta bort hello modellen utbildningsmoduler.
2. Identifiera en logisk **inkommande port** toorepresent hello förväntades indata schemat.
3. Identifiera en logisk **utgående port** toorepresent hello förväntade web service utdataschema.

När hello bedömningen experiment skapas, granska den och göra justera efter behov. En typisk justering är tooreplace hello inkommande dataset och/eller fråga med någon vilket utesluter etikett fält, eftersom dessa inte är tillgänglig när tjänsten hello kallas. Det är också en bra idé tooreduce hello storlek på hello indata dataset-och/eller tooa några poster, bara tillräckligt med tooindicate hello ingående schema. För hello utdataporten den gemensamma tooexclude alla inmatningsfält och bara innehålla hello **poängsatta etiketter** och **bedömas sannolikhet** i hello utdata med hello [Välj kolumner i datauppsättning ] [ select-columns] modul.

Ett exempel bedömningen experiment finns i hello bilden nedan. När klar toodeploy, klicka på hello **publicera WEBBTJÄNSTEN** knapp i hello lägre Åtgärdsfältet.

![Azure ML publicera][11]

## <a name="summary"></a>Sammanfattning
toorecap vad vi har gjort i den här genomgången självstudiekursen, du har skapat ett Azure datavetenskap miljö, arbetat med en stor offentliga datauppsättning tar genom hello Team vetenskap av data, alla hello sätt från data förvärv toomodel utbildning och sedan toohello distribution av en Azure Machine Learning-webbtjänst.

### <a name="license-information"></a>Licensinformationen
Den här genomgången exempel och dess tillhörande skript och IPython notebook(s) delas av Microsoft under hello MIT-licens. Kontrollera filen LICENSE.txt för hello i hello directory hello exempelkoden på GitHub för mer information.

## <a name="references"></a>Referenser
• [Andrés Monroy NYC Taxi resor hämtningssidan](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC Taxitransport resa Data av Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi och Limousine kommissionens forskning och statistik](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
