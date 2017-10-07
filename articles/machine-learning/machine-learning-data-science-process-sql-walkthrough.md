---
title: "aaaBuild och distribuera en maskininlärningsmodell som använder SQL Server på en virtuell dator i Azure | Microsoft Docs"
description: "Processen för avancerade analyser och teknik i åtgärd"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6066b083-262c-4453-a712-a5c05acc3df8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: fashah;bradsev
ms.openlocfilehash: 30ba9a9e3cf65f75015e13f9c7876dcbccc5bc47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action-using-sql-server"></a>hello Team av vetenskapliga data i praktiken: använder SQL Server
I den här kursen går igenom hello processen att skapa och distribuera en maskininlärningsmodell med hjälp av SQL Server och ett offentligt tillgängliga dataset--hello [NYC Taxi resor](http://www.andresmh.com/nyctaxitrips/) dataset. hello proceduren följer ett standarddata vetenskap arbetsflöde: infognings- och utforska hello data, utveckla funktioner toofacilitate learning och sedan skapa och distribuera en modell.

## <a name="dataset"></a>NYC Taxi resor Dataset-beskrivning
hello NYC Taxi resa data är cirka 20GB komprimerat CSV-filer (~ 48GB okomprimerade), som består av fler än 173 miljoner enskilda resor och hello priser för varje resa. Hello hämtning och Samlingsbibliotek plats och tid, anonymiserade hackare (drivrutin) licensnummer och medallion (taxi's unikt id) nummer innehåller varje resa-post. hello data omfattar alla resor hello år 2013 och finns i följande två datamängder för varje månad hello:

1. hello 'trip_data' CSV innehåller resa information, till exempel antal passagerare, hämtning och dropoff punkter, resa varaktighet och resa längd. Här följer några Exempelposter:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. hello 'trip_fare' CSV innehåller information om hello avgiften betalat för varje förflyttning, till exempel betalningssätt, avgiften belopp, tillägg och skatter, tips och vägtullar och hello totalbelopp betald. Här följer några Exempelposter:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

hello Unik nyckel toojoin resa\_data och resa\_avgiften består av hello fält: medallion hackare\_licensen och hämtning\_datetime.

## <a name="mltasks"></a>Exempel på förutsägelse uppgifter
Vi kommer att formulera tre förutsägelse problem utifrån hello *tips\_belopp*, nämligen:

1. Binär klassificering: förutsäga oavsett betalats ett tips för en resa i d.v.s. en *tips\_belopp* som är större än 0 är ett positivt exempel, när en *tips\_belopp* $0 är en negativt exempel.
2. Multiklass-baserad klassificering: toopredict hello mängd tips betalat för hello resa. Vi dela hello *tips\_belopp* i fem lagerplatser eller klasser:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. Regression uppgiften: toopredict hello tips betalats för en transport.  

## <a name="setup"></a>Ställa in hello Azure vetenskap datamiljö för avancerade analyser
Som du kan se hello [planera din miljö](machine-learning-data-science-plan-your-environment.md) guide, det finns flera alternativ toowork med hello NYC Taxi resor datamängd i Azure:

* Arbeta med hello data i Azure BLOB sedan modellen i Azure Machine Learning
* Läs in hello data till en SQL Server-databasen och sedan modellen i Azure Machine Learning

I den här kursen visar vi parallella massimport av hello data tooa SQL Server, datagranskning, funktionen tekniker och ned provtagning med hjälp av SQL Server Management Studio som använder IPython bärbar dator. [Exempel på skript](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) och [IPython anteckningsböcker](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) delas i GitHub. Exempel IPython anteckningsboken toowork med hello data i Azure BLOB är också tillgängligt i hello samma plats.

tooset konfigurera Azure datavetenskap miljön:

1. [Skapa ett lagringskonto](../storage/common/storage-create-storage-account.md)
2. [Skapa en arbetsyta för Azure Machine Learning](machine-learning-create-workspace.md)
3. [Etablera en virtuell dator med datavetenskap](machine-learning-data-science-setup-sql-server-virtual-machine.md), som innehåller en SQL-Server och en bärbar dator IPython-server.
   
   > [!NOTE]
   > hello exempelskript och IPython bärbara datorer kommer att hämtade tooyour datavetenskap virtuella datorn under hello installationsprocessen. När hello VM efterinstallationsskript är klar blir hello-exempel i den Virtuella datorns dokumentbibliotek:  
   > 
   > * Exempel skript:`C:\Users\<user_name>\Documents\Data Science Scripts`  
   > * Exempel IPython bärbara datorer:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
   >   där `<user_name>` är den Virtuella datorns Windows-inloggningsnamn. Vi kommer att referera toohello exempel mappar som **exempelskript** och **exempel IPython anteckningsböcker**.
   > 
   > 

Utifrån hello dataset storlek, datakällplats och hello valda Azure målmiljön det här scenariot är liknande för[scenariot \#5: stora datauppsättning i lokala filer, mål SQL Server i Azure VM](machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).

## <a name="getdata"></a>Hämta hello Data från offentliga källa
tooget hello [NYC Taxi resor](http://www.andresmh.com/nyctaxitrips/) dataset från dess offentlig plats, kan du använda någon av hello-metoder som beskrivs i [tooand flytta Data från Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello data tooyour ny virtuell dator.

toocopy hello data med hjälp av AzCopy:

1. Logga in tooyour virtuell dator (VM)
2. Skapa en ny katalog i hello VM datadisk (Obs: Använd inte hello diskutrymme som medföljer hello VM som en datadisk).
3. Kör hello följande kommandorad för Azcopy, ersätta < path_to_data_folder > med datamappen som skapats i (2) i ett kommandotolksfönster:
   
        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S
   
    När hello AzCopy är klar totalt av 24 zippade CSV-filer (12 för resa\_data och 12 för resa\_avgiften) måste vara i hello data-mappen.
4. Packa upp hello hämtade filer. Observera hello mappen där hello okomprimerade filerna finns. Den här mappen blir refererad tooas Hej < sökväg\_till\_data\_filer\>.

## <a name="dbload"></a>Massinläsning importera Data till SQL Server-databas
hello prestanda för inläsning/överföra stora mängder data tooan SQL-databasen och efterföljande frågor kan förbättras genom att använda *partitionerade tabeller och vyer*. I det här avsnittet kommer vi följer instruktionerna i hello [parallella Bulk Import med hjälp av SQL-Partition datatabeller](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) toocreate en ny databas och Läs in hello data till partitionerade tabeller parallellt.

1. När du är inloggad i tooyour VM starta **SQL Server Management Studio**.
2. Ansluta med Windows-autentisering.
   
    ![SSMS Anslut][12]
3. Om du har ännu inte ändrats hello SQL Server-autentiseringsläget och skapa en ny SQL-inloggning, öppna hello skriptfilen som heter **ändra\_auth.sql** i hello **exempelskript** mapp. Ändra hello standardanvändarnamn och lösenord. Klicka på **! Köra** i hello verktygsfältet toorun hello skript.
   
    ![Kör skript][13]
4. Kontrollera och/eller ändra hello SQL Server standard databasen och loggfilerna mappar tooensure som nyligen skapats databaser kommer att lagras i en datadisk. hello SQL Server-VM-avbildning som är optimerad för datawarehousing belastningar är förkonfigurerad med data och loggfilen diskar. Om den virtuella datorn inte innehöll en datadisk och du har lagt till nya virtuella hårddiskar under hello VM installationsprocessen, ändra hello standardmappar på följande sätt:
   
   * Högerklicka på hello SQL Server-namnet i hello vänster panel och klicka på **egenskaper**.
     
       ![Egenskaper för SQL Server][14]
   * Välj **databasinställningar** från hello **Välj en sida** listan toohello vänster.
   * Kontrollera och/eller ändra hello **databasen standardplatserna** toohello **datadisk** platser för ditt val. Detta är där nya databaser finns om skapats med hello standardinställningarna för platsen.
     
       ![Standardvärdet för SQL-databasen][15]  
5. toocreate en ny databas och en uppsättning filgrupper toohold Hej partitionerade tabeller, öppna hello exempelskript **skapa\_db\_default.sql**. hello skriptet skapar en ny databas med namnet **TaxiNYC** och 12 filgrupper hello standardplatsen för data. Varje filgrupp innehåller en månad resa\_data och resa\_färdavgiften data. Ändra hello databasens namn, om så önskas. Klicka på **! Köra** toorun hello skript.
6. Skapa sedan två partitionstabeller, en för hello resa\_data och en annan för hello resa\_avgiften. Öppna hello exempelskript **skapa\_partitionerade\_table.sql**, som:
   
   * Skapa en partition funktionen toosplit hello data per månad.
   * Skapa en partition scheme toomap varje månad data tooa annan filgrupp.
   * Skapa två partitionerade tabeller mappade toohello partitionsschema: **nyctaxi\_resa** innehåller hello resa\_data och **nyctaxi\_avgiften** innehåller hello resa \_färdavgiften data.
     
     Klicka på **! Köra** toorun hello skript och skapa hello partitionerade tabeller.
7. I hello **exempelskript** mapp, det finns två PowerShell-exempelskript tillhandahålls toodemonstrate parallella bulk import av data tooSQL Server-tabeller.
   
   * **BCP\_parallella\_generic.ps1** är ett Allmänt skript tooparallel bulk importera data till en tabell. Ändra det här skriptet tooset hello indata- och variabler som anges i hello kommentarer i hello skript.
   * **BCP\_parallella\_nyctaxi.ps1** är en förkonfigurerad version av hello Allmänt skript och kan vara används tootooload båda tabellerna för hello NYC Taxi resor data.  
8. Högerklicka på hello **bcp\_parallella\_nyctaxi.ps1** skriptets namn och klicka på **redigera** tooopen i PowerShell. Granska hello förinställningen variabler och ändra bl.a tooyour valda databasens namn, mappen inkommande data, loggen målmappen och sökvägar toohello exempelfiler format **nyctaxi_trip.xml** och **nyctaxi\_ fare.XML** (anges i hello **exempelskript** mapp).
   
    ![Massinläsning importera Data][16]
   
    Du kan också markera hello autentiseringsläge, standardvärdet är Windows-autentisering. Klicka på hello grön pil i hello verktygsfältet toorun. hello skriptet startas 24 import massåtgärder parallell 12 för varje partitionerade tabellen. Du kan övervaka hello importera förlopp genom att öppna hello SQL Server data standardmappen som ovan.
9. hello PowerShell-skript rapporter hello start- och sluttid. När alla Massredigera importen slutförts rapporteras hello sluttid. Kontrollera hello mål loggen mappen tooverify hello bulk import, som t.ex. inga fel rapporteras i hello målmappen för loggen.
10. Databasen är nu redo för undersökning, funktionen tekniker och andra åtgärder efter behov. Eftersom hello tabeller partitioneras enligt toohello **hämtning\_datetime** fältet frågor som omfattar **hämtning\_datetime** villkor i hello  **DÄR** satsen drar nytta av hello partitionsschema.
11. I **SQL Server Management Studio**, utforska hello tillhandahålls exempelskript **exempel\_queries.sql**. toorun någon hello exempelfrågor, markera hello fråga rader och klicka sedan på **! Köra** i hello-verktygsfältet.
12. hello NYC Taxi resor data har lästs in i två olika tabeller. tooimprove kopplingsåtgärder rekommenderas tooindex hello tabeller. Hej exempelskript **skapa\_partitionerade\_index.sql** skapar partitionerade index i hello sammansatta koppling nyckeln **medallion hackare\_licens och hämtning\_datetime**.

## <a name="dbexplore"></a>Datagranskning och funktionen tekniker i SQLServer
I det här avsnittet ska vi utföra data från kartläggning av naturresurser och funktionen generation genom att köra SQL-frågor direkt i hello **SQL Server Management Studio** använda hello SQL Server-databas som skapats tidigare. Ett exempelskript som heter **exempel\_queries.sql** har angetts i hello **exempelskript** mapp. Ändra hello skriptet toochange hello databasens namn, om det skiljer sig från standard hello: **TaxiNYC**.

I den här övningen ska du:

* Ansluta för**SQL Server Management Studio** med hjälp av antingen Windows-autentisering eller SQL-autentisering och hello SQL-inloggningsnamn och lösenord.
* Utforska data distributioner av ett fåtal fält i olika tidsfönster.
* Undersök data quality hello longitud och latitud fält.
* Generera binära och multiklass-baserad klassificeringsetiketter baserat på hello **tips\_belopp**.
* Generera funktioner och beräkning eller jämförelse resa avstånd.
* Koppla hello två tabeller och extrahera ett slumpmässigt prov som kommer att använda toobuild modeller.

När du är klar tooproceed tooAzure Machine Learning, kan du antingen:  

1. Spara hello slutliga SQL tooextract och exempel hello data och kopiera / klistra in hello fråga direkt till en [importera Data] [ import-data] modul i Azure Machine Learning, eller
2. Kvarstår hello provtagning och bakåtkompilerade data som du planerar toouse för modellen bygga i en ny databas tabell och använda hello ny tabell i hello [importera Data] [ import-data] modul i Azure Machine Learning.

I det här avsnittet sparar vi hello slutlig tooextract och exempel hello data. hello andra metoden visar hello [Datagranskning och funktionen Engineering i IPython anteckningsbok](#ipnb) avsnitt.

För en snabb kontroll av hello antalet rader och kolumner i hello fylls tabeller tidigare med hjälp av parallella massimport

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Undersökning: Resa distribution av medallion
Det här exemplet identifierar hello medallion (taxi numbers) med mer än 100 resor inom en viss tidsperiod. hello-frågan skulle dra nytta av hello partitionerad tabellåtkomst eftersom den är villkorad av hello partitionsschemat för **hämtning\_datetime**. Frågar hello fullständiga datauppsättningen blir också användning av hello partitionerad tabell och/eller index-sökning.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Undersökning: Resa distribution av medallion och hack_license
    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Data utvärdering: Verifiera poster med felaktigt longituden eller latituden
Det här exemplet undersöker om någon av hello longituden eller latituden fält antingen innehåller ett ogiltigt värde (radian grader ska vara mellan-90 och 90), eller ha (0, 0) koordinater.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Undersökning: Lutad jämfört med Inte lutad resor distribution
Det här exemplet hittar hello antalet turer som har lutad kontra lutad inte i en given tidpunkt tidsperiod (eller i hello fullständiga datauppsättningen om täcker hello fullständig år). Den här distributionen visar hello binära etikett distribution toobe senare används för binär klassificering modellering.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Undersökning: Tips klass-intervallet Distribution
Det här exemplet beräknar hello distribution av tips intervall i en given tidpunkt tidsperiod (eller i hello fullständiga datauppsättningen om täcker hello fullständig år). Detta är hello distribution av hello etikett klasser som ska användas senare för multiklass-baserad klassificering modellering.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>Undersökning: Beräkna och jämföra resa avstånd
Det här exemplet konverterar hello hämtning och Samlingsbibliotek longitud och latitud tooSQL geografi pekar beräknar hello resa avståndet med SQL geografi punkter skillnaden och returnerar ett slumpvist urval av hello resultat för jämförelse. hello exempel begränsar hello resultat toovalid samordnar bara använder hello kvalitet assessment datafrågor omfattas tidigare.

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>Funktionen teknikerna i SQL-frågor
hello etikett generation och geografi konvertering utforskning frågor kan också vara används toogenerate etiketter och funktioner genom att ta bort hello räknat del. Ytterligare funktionen engineering SQL-exempel finns i hello [Datagranskning och funktionen Engineering i IPython anteckningsbok](#ipnb) avsnitt. Det är effektivare toorun hello funktionen generation frågor på hello fullständiga datauppsättningen eller en stor del av den med hjälp av SQL-frågor som körs direkt på hello SQL Server-databasinstansen. hello frågor kan köras i **SQL Server Management Studio**, IPython bärbar dator eller alla verktyg/utvecklingsmiljö kan komma åt hello databas lokalt eller fjärranslutet.

#### <a name="preparing-data-for-model-building"></a>Förbereder Data för Modellskapandet
hello följande fråga kopplingar hello **nyctaxi\_resa** och **nyctaxi\_avgiften** tabeller, genererar en binär klassificering etikett **lutad**, flera klassen klassificering etikett **tips\_klassen**, och extraherar ett slumpmässigt prov 1% från hello fullständigt sammanfogade dataset. Den här frågan kan kopieras och klistras in direkt i hello [Azure Machine Learning Studio](https://studio.azureml.net) [importera Data] [ import-data] -modulen för direkt datapåfyllning från hello SQL Server-databas instans i Azure. hello frågan utesluter poster med fel (0, 0) koordinater.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="ipnb"></a>Datagranskning och funktionen tekniker i IPython anteckningsbok
I det här avsnittet ska vi utföra datagranskning och funktion som genereras med hjälp av Python- och SQL-frågor mot hello SQL Server-databas som skapats tidigare. En exempel IPython bärbar dator med namnet **machine-Learning-data-science-process-sql-story.ipynb** har angetts i hello **exempel IPython anteckningsböcker** mapp. Den här anteckningsboken finns också på [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).

hello rekommenderas sekvens när du arbetar med stordata är hello följande:

* Läs i ett litet antal hello data till en ram i minnet data.
* Utföra vissa visualiseringar och explorations med hello samplade data.
* Experiment med funktionen tekniker med hello exempeldata.
* För större datagranskning använder datamanipulering och funktionen tekniker Python tooissue SQL-frågor direkt mot hello SQL Server-databas i hello Azure VM.
* Bestäm hello exempel storlek toouse för skapande av Azure Machine Learning-modellen.

När klar tooproceed tooAzure Machine Learning, du kan antingen:  

1. Spara hello slutliga SQL tooextract och exempel hello data och kopiera / klistra in hello fråga direkt till en [importera Data] [ import-data] modul i Azure Machine Learning. Den här metoden visar hello [bygga modeller i Azure Machine Learning](#mlmodel) avsnitt.    
2. Bevara hello provtagning och bakåtkompilerade data som du planerar toouse för modellen bygga i en ny databastabell, och Använd sedan hello ny tabell hello [importera Data] [ import-data] modul.

hello följande är några datagranskning datavisualisering och funktionen engineering exempel. Fler exempel finns hello exempel SQL IPython anteckningsboken i hello **exempel IPython anteckningsböcker** mapp.

#### <a name="initialize-database-credentials"></a>Initiera Databasautentiseringsuppgifter
Initiera databasen anslutningsinställningarna i hello följande variabler:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Skapa databasanslutning
    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Rapporten antalet rader och kolumner i tabellen nyctaxi_trip
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* Totalt antal rader = 173179759  
* Totalt antal kolumner = 14

#### <a name="read-in-a-small-data-sample-from-hello-sql-server-database"></a>Läs i ett litet datasampel från hello SQL Server-databas
    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time tooread hello sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Tid tooread hello exempeltabell är 6.492000 sekunder  
Antal rader och kolumner hämtas = (84952, 21)

#### <a name="descriptive-statistics"></a>Beskrivande statistik
Nu är klar tooexplore hello provtagning data. Vi börjar med att titta på beskrivande statistik för hello **resa\_avstånd** (eller andra) nyckelfälten:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Visualiseringen: Exempel på ritytans
Vi titta på hello Låddiagram för hello resa avståndet toovisualize hello quantiles

    df1.boxplot(column='trip_distance',return_type='dict')

![Rita #1][1]

#### <a name="visualization-distribution-plot-example"></a>Visualiseringen: Distribution ritytans exempel
    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Rita #2][2]

#### <a name="visualization-bar-and-line-plots"></a>Visualisering: Fältet och rad områden
I det här exemplet vi bin hello resa avståndet i fem lagerplatser och visualisera hello diskretisering resultat.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Vi kan ritas hello ovan bin distribution i ett fält eller rad ritytans enligt nedan

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Rita #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Rita #4][4]

#### <a name="visualization-scatterplot-example"></a>Visualiseringen: Scatterplot exempel
Visar vi punktdiagram ritytans mellan **resa\_tid\_i\_sek** och **resa\_avstånd** toosee om det finns några korrelation

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Rita #6][6]

På liknande sätt kan vi Kontrollera hello förhållandet mellan **hastighet\_kod** och **resa\_avstånd**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Rita #8][8]

### <a name="sub-sampling-hello-data-in-sql"></a>Underordnade provtagning hello Data i SQL
När du förbereder data för modellen bygga i [Azure Machine Learning Studio](https://studio.azureml.net), kan du antingen välja på hello **SQL-frågan toouse direkt i hello importera Data modulen** eller spara hello utformad och provtagning data i en ny tabell, som du kan använda i hello [importera Data] [ import-data] modul med en enkel **Välj * FROM < din\_nya\_tabell\_namn >**.

I det här avsnittet ska vi skapa en ny tabell toohold hello provtagning och utformad data. Ett exempel på en direkt SQL-fråga för skapande av modellen som har angetts i hello [Datagranskning och funktionen Engineering i SQL Server](#dbexplore) avsnitt.

#### <a name="create-a-sample-table-and-populate-with-1-of-hello-joined-tables-drop-table-first-if-it-exists"></a>Skapa en exempeltabell och Fyll i med 1% av hello kopplade tabeller. Ta bort den första tabellen om den finns.
I det här avsnittet vi koppla hello tabeller **nyctaxi\_resa** och **nyctaxi\_avgiften**Extrahera ett slumpmässigt prov 1% och bevara hello provtagning data i ett nytt tabellnamn  **nyctaxi\_en\_procent**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>Datagranskning med hjälp av SQL-frågor i IPython anteckningsbok
I det här avsnittet förklarar vi data distributioner med hello 1% provtagning data som sparas i hello ny tabell som vi skapade ovan. Observera att liknande explorations kan utföras med hjälp av hello ursprungliga tabeller, vid behov kan **TABLESAMPLE** toolimit hello utforskning exempel eller genom att begränsa hello resulterar tooa angivna tidsperiod med hello **hämtning \_datetime** partitioner, enligt beskrivningen i hello [Datagranskning och funktionen Engineering i SQL Server](#dbexplore) avsnitt.

#### <a name="exploration-daily-distribution-of-trips"></a>Undersökning: Dagliga distribution av resor
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Undersökning: Resa distribution per medallion
    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>Funktionen Generation med hjälp av SQL-frågor i IPython anteckningsbok
I det här avsnittet kommer vi att generera nya etiketter och funktioner direkt med SQL-frågor, arbetar hello 1% exempeltabell vi skapade i hello föregående avsnitt.

#### <a name="label-generation-generate-class-labels"></a>Etikettgenerering: Skapa klassen etiketter
I följande exempel hello, skapar vi två uppsättningar med etiketter toouse för modellering:

1. Binär klassen etiketter **lutad** (förutsäga om ett tips ges)
2. Multiklass-baserad etiketter **tips\_klassen** (förutsäga hello tips bin eller intervall)
   
        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''
   
        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()
   
        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''
   
        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Funktionen tekniker: Antal funktioner för Kategoriska kolumner
Det här exemplet transformerar ett kategoriska fält i ett numeriskt fält genom att ersätta varje kategori med hello antal dess förekomster i hello data.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Funktionen tekniker: Bin funktioner för numeriska kolumner
Det här exemplet omvandlar en kontinuerlig numeriskt fält till förinställda kategori intervall, d.v.s. transformeringen numeriskt fält i ett kategoriska fält.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Funktionen tekniker: Extrahera plats funktioner från Decimal latitud/longitud
Det här exemplet uppdelad hello decimal representation av ett latitud och longitud fält till flera region fält i olika detaljnivå som, land, ort, staden, block och så vidare. Observera att hello nya geo-fält inte är mappad tooactual platser. Information om geocode kartläggning finns [Bing Maps REST Services](https://msdn.microsoft.com/library/ff701710.aspx).

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-hello-final-form-of-hello-featurized-table"></a>Kontrollera hello slutliga form av hello featurized tabell
    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Vi är nu redo tooproceed toomodel byggnad och distribution av modellen i [Azure Machine Learning](https://studio.azureml.net). hello data är redo för någon av hello förutsägelse problem som konstaterats tidigare, nämligen:

1. Binär klassificering: toopredict huruvida ett tips har betalat för en resa.
2. Multiklass-baserad klassificering: toopredict hello mängd tips betald, enligt toohello tidigare definierade klasserna.
3. Regression uppgiften: toopredict hello tips betalats för en transport.  

## <a name="mlmodel"></a>Skapa modeller i Azure Machine Learning
toobegin Hej modeling Övning, logga in tooyour Azure Machine Learning-arbetsytan. Om du inte har skapat machine learning-arbetsytan finns [skapa en arbetsyta för Azure Machine Learning](machine-learning-create-workspace.md).

1. tooget igång med Azure Machine Learning finns [vad är Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)
2. Logga in för[Azure Machine Learning Studio](https://studio.azureml.net).
3. startsidan för hello Studio innehåller en mängd information, videor, självstudier, länkar toohello moduler referens och andra resurser. Mer information om Azure Machine Learning finns hello [Azure Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).

En typisk träningsexperiment består av följande hello:

1. Skapa en **+ ny** experiment.
2. Hämta hello data tooAzure Machine Learning.
3. Förbearbeta, transformera och manipulera hello data efter behov.
4. Generera funktioner efter behov.
5. Dela upp hello data i utbildning / / verifieringstesterna datauppsättningar (eller ha separata datauppsättningar för varje).
6. Välj en eller flera maskininlärningsalgoritmer beroende på hello learning problemet toosolve. T.ex. binär klassificering, multiklass-baserad klassificering, regression.
7. Träna en eller flera modeller som använder hello utbildning dataset.
8. Poängsätta hello validering dataset med hello tränade modeller.
9. Utvärdera hello modeller toocompute hello relevanta mätvärden för hello learning problem.
10. Finjustera hello modeller och välj hello bästa modellen toodeploy.

I den här övningen har vi redan utforskade och utformad hello data i SQL Server och valt hello exempel storlek tooingest i Azure Machine Learning. toobuild en eller flera av hello förutsägelse modeller vi valt:

1. Hämta hello data tooAzure Machine Learning med hello [importera Data] [ import-data] modulen är tillgängliga i hello **Data ingående och utgående** avsnitt. Mer information finns i hello [importera Data] [ import-data] modulsida referens.
   
    ![Azure Machine Learning importera Data][17]
2. Välj **Azure SQL Database** som hello **datakällan** i hello **egenskaper** panelen.
3. Ange hello DNS-namn i hello **Databasservernamnet** fältet. Format:`tcp:<your_virtual_machine_DNS_name>,1433`
4. Ange hello **databasnamnet** i hello motsvarande fält.
5. Ange hello **användarnamn för SQL** i hello ** aqccount användarnamn för Server och hello lösenord i hello **serverlösenord**.
6. Kontrollera **acceptera alla servercertifikat** alternativet.
7. I hello **databasfrågan** redigera textområde, klistra in vilka extrakt hello nödvändigt databasfält (inklusive eventuella beräknade fält, till exempel hello etiketter) och ned exempel hello data toohello önskad provtagning hello-fråga.

Ett exempel på en binär klassificering experiment läsning av data direkt från hello SQL Server-databasen är i hello bilden nedan. Liknande experiment kan konstrueras för multiklass-baserad klassificering och regressionsproblem.

![Azure Machine Learning Train][10]

> [!IMPORTANT]
> I hello modellering extrahering av data och provtagning frågan exempel finns i föregående avsnitt **alla etiketter för hello tre modellering övningarna tas med i hello fråga**. Är ett viktigt steg i (obligatoriskt) i varje hello modeling övningarna för**undanta** hello onödiga etiketter för hello andra två problem och andra **mål minnesläckor**. För t.ex. Använd när du använder binär klassificering hello etikett **lutad** och utelämna hello fält **tips\_klassen**, **tips\_belopp**, och **totala\_belopp**. hello senare är målet minnesläckor eftersom de innebär hello tips betalda.
> 
> tooexclude onödiga kolumner och/eller målet minnesläckor, kan du använda hello [Välj kolumner i datauppsättning] [ select-columns] modul eller hello [redigera Metadata] [ edit-metadata]. Mer information finns i [Välj kolumner i datauppsättning] [ select-columns] och [redigera Metadata] [ edit-metadata] referera sidor.
> 
> 

## <a name="mldeploy"></a>Distribuera modeller i Azure Machine Learning
När modellen är klar, kan du enkelt distribuera det som en webbtjänst direkt från hello experiment. Mer information om hur du distribuerar Azure Machine Learning-webbtjänster finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).

toodeploy en ny webbtjänst, måste du:

1. Skapa ett bedömningsprofil experiment.
2. Distribuera hello-webbtjänsten.

toocreate poängsättning av ett experiment från en **avslutad** utbildning experiment, klickar du på **skapa BEDÖMNINGEN EXPERIMENTERA** i hello lägre Åtgärdsfältet.

![Azure bedömningen][18]

Azure Machine Learning försöker toocreate ett bedömningsprofil experiment baserat på hello komponenter i hello träningsexperiment. I synnerhet att:

1. Spara hello tränad modell och ta bort hello modellen utbildningsmoduler.
2. Identifiera en logisk **inkommande port** toorepresent hello förväntades indata schemat.
3. Identifiera en logisk **utgående port** toorepresent hello förväntade web service utdataschema.

När hello bedömningen experiment skapas, granska den och justera efter behov. En typisk justering är tooreplace hello inkommande dataset och/eller fråga med någon vilket utesluter etikett fält, eftersom dessa inte är tillgänglig när tjänsten hello kallas. Det är också en bra idé tooreduce hello storlek på hello indata dataset-och/eller tooa några poster, bara tillräckligt med tooindicate hello ingående schema. För hello utdataporten den gemensamma tooexclude alla inmatningsfält och bara innehålla hello **poängsatta etiketter** och **bedömas sannolikhet** i hello utdata med hello [Välj kolumner i datauppsättning ] [ select-columns] modul.

Ett exempel på en bedömningen experiment har hello bilden nedan. När klar toodeploy, klicka på hello **publicera WEBBTJÄNSTEN** knapp i hello lägre Åtgärdsfältet.

![Publicera Azure Machine Learning][11]

toorecap, i den här genomgången kursen har du skapat ett Azure datavetenskap miljö, arbetat med en stor offentliga datauppsättning alla hello sätt från data förvärv toomodel träning och distributionen av en Azure Machine Learning-webbtjänst.

### <a name="license-information"></a>Licensinformationen
Den här genomgången exempel och dess tillhörande skript och IPython notebook(s) delas av Microsoft under hello MIT-licens. Kontrollera filen LICENSE.txt för hello i hello directory hello exempelkoden på GitHub för mer information.

### <a name="references"></a>Referenser
• [Andrés Monroy NYC Taxi resor hämtningssidan](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC Taxitransport resa Data av Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi och Limousine kommissionens forskning och statistik](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
