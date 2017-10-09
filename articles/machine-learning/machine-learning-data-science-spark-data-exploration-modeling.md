---
title: "aaaData undersökning och modellering med Spark | Microsoft Docs"
description: "Showcases hello datagranskning och integrera affärsmodelleringsfunktioner av hello Spark MLlib toolkit på Azure."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b989b918-5ba5-4696-b8d0-76ae510a23f4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: cf5cee4575053f5954b08ca659dfc39c53798371
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-exploration-and-modeling-with-spark"></a>Datagranskning och modellering med Spark
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Den här genomgången använder HDInsight Spark toodo datagranskning och binär klassificering och regressionen modeling uppgifter på ett urval av hello NYC Taxitransport resa och färdavgiften 2013 dataset.  Den vägleder dig genom stegen för hello av hello [datavetenskap Process](http://aka.ms/datascienceprocess), slutpunkt-till-slutpunkt, med hjälp av ett HDInsight Spark-kluster för bearbetning och Azure-blobbar toostore hello data och hello modeller. hello processen utforskar visualizes data som tillkommit från ett Azure Storage Blob och förbereder hello data toobuild förutsägelsemodeller. Dessa modeller är versionen med hello Spark MLlib toolkit toodo binär klassificering och regressionen modeling uppgifter.

* Hej **binär klassificering** uppgift är toopredict huruvida ett tips betalat för hello resa. 
* Hej **regression** uppgift är toopredict hello mängden hello tips baserat på andra tips funktioner. 

hello modeller som vi använder inkluderar logistic och linjär regression, slumpmässiga skogar och toning ökat träd:

* [Linjär regression med SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) är en linjär regressionsmodell som använder en Stokastisk toning härstammar (SGD)-metod och skalning toopredict hello tips belopp betald för optimering och funktion. 
* [Logistic regression med LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) eller ”logit” regression är en regressionsmodell som kan användas när hello beroende variabel är kategoriska toodo dataklassificering. LBFGS är en kvasi Karlsson optimering algoritm som uppskattar hello Broyden – Fletcher – Goldfarb – Shanno (BFGS) algoritm med en begränsad mängd ledigt diskutrymme på datorn och som ofta används i machine learning.
* [Slumpmässiga skogar](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) är ensembler av beslutsträd.  De kombinera många decision trees tooreduce hello risken för overfitting. Slumpmässiga skogar används för regression och klassificering kan hantera kategoriska funktioner och kan utökas toohello multiklass-baserad klassificering inställningen. De kräver inte funktionen skalning och är kan toocapture icke-linjärt och funktionen interaktioner. Slumpmässiga skogar är en av hello mest framgångsrika maskininlärning modeller för klassificering och regression.
* [Toningen ökat träd](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) är ensembler av beslutsträd. GBTs train beslut träd upprepade gånger toominimize en förlust-funktion. GBTs för regression och klassificering kan hantera kategoriska funktioner, kräver inte funktionen skalning och att kan toocapture icke-linjärt och interaktioner. De kan också användas i en multiclass klassificering.

hello modellering steg innehåller också kod visar hur tootrain, utvärdera och spara varje typ av modellen. Python har använt toocode hello lösningen och tooshow hello relevanta områden.   

> [!NOTE]
> Även om hello Spark MLlib toolkit är utformad toowork på stora datamängder, används ett relativt litet exempel (cirka 30 Mb med 170K rader, om 0,1% hello ursprungliga NYC datauppsättningen) här i informationssyfte. hello Övning som anges här körs effektivt (i 10 minuter) på ett HDInsight-kluster med 2 arbetsnoderna. hello kan har samma kod, med mindre ändringar vara används tooprocess större-datamängder med lämpliga ändringar för cachelagring av data i minnet och ändra hello klusterstorleken.
> 
> 

## <a name="prerequisites"></a>Krav
Du behöver ett Azure-konto och en Spark 1.6 (eller Spark 2.0) HDInsight-kluster toocomplete den här genomgången. Se hello [översikt av datavetenskap med Spark på Azure HDInsight](machine-learning-data-science-spark-overview.md) anvisningar för hur toosatisfy dessa krav. Avsnittet innehåller också en beskrivning av hello NYC 2013 Taxi data används här och anvisningar om hur tooexecute kod från en Jupyter-anteckningsbok på hello Spark-kluster. 

## <a name="spark-clusters-and-notebooks"></a>Spark-kluster och bärbara datorer
Konfigurationsstegen och kod finns i den här genomgången för att använda ett HDInsight Spark 1.6. Men Jupyter-anteckningsböcker som har angetts för både HDInsight Spark 1.6 och 2.0 Spark-kluster. En beskrivning av hello anteckningsböcker och länkar toothem finns i hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) för hello GitHub-lagringsplats som innehåller dessa. Dessutom hello koden här hello länkade anteckningsböcker är generisk och bör fungera på Spark-kluster. Om du inte använder HDInsight Spark hello konfiguration och hantering av steg kan vara skiljer sig från vad som anges här. För enkelhetens skull följer hello länkar toohello Jupyter-anteckningsböcker för Spark 1.6 (toobe köras i hello pySpark-kerneln av hello Jupyter-anteckningsbok server) och Spark 2.0 (toobe kör i hello pySpark3 kernel av hello Jupyter-anteckningsbok server):

### <a name="spark-16-notebooks"></a>Spark 1.6 bärbara datorer

[pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): innehåller information om hur tooperform datagranskning modellering och bedömningen med flera olika algoritmer.

### <a name="spark-20-notebooks"></a>Spark 2.0 bärbara datorer
hello regression och klassificering uppgifter som implementeras med hjälp av ett 2.0 Spark-kluster finns i separata anteckningsböcker och hello klassificering anteckningsboken använder en annan datauppsättning:

- [Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): den här filen innehåller information om hur tooperform datagranskning modellering och bedömningen i Spark 2.0-kluster med hello NYC Taxi resa och avgiften-datamängd beskrivs [här](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data). Den här anteckningsboken kan vara en bra utgångspunkt för att snabbt utforska hello koden som vi har angetts för Spark 2.0. En mer detaljerad anteckningsbok analyserar hello NYC Taxi data, finns hello nästa anteckningsboken i den här listan. Se hello anteckningar efter den här listan som jämför dessa datorer. 
- [Spark2.0 pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): den här filen visar hur tooperform data wrangling (Spark SQL och dataframe operations), utforskning modellering och bedömningen med hello NYC Taxi resa och avgiften datauppsättning beskrivs [ här](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).
- [Spark2.0 pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): den här filen visar hur tooperform data wrangling (Spark SQL och dataframe operations), utforskning modellering och bedömningen med hello välkända flygbolag i tid avvikelse DataSet från 2011 och 2012. Vi integrerad hello flygbolag dataset med hello flygplats väder data (t.ex. vindhastigheten, temperatur, höjd etc.) tidigare toomodeling, så att dessa väder-funktioner kan ingå i hello modellen.

<!-- -->

> [!NOTE]
> hello flygbolag datamängden har lagts till toohello Spark 2.0 anteckningsböcker toobetter illustrera hello användningen av algoritmer för klassificering. Se följande länkar för information om flygbolag i tid avvikelse dataset och väder datamängd hello:

>- Flygbolag i tid avvikelse data: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)

>- Flygplats väder data: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/) 
> 
> 

<!-- -->

<!-- -->

> [!NOTE]
hello Spark 2.0 anteckningsböcker på hello NYC taxi och flygbolag svarta fördröjning-datauppsättningar kan ta 10 minuter eller mer toorun (beroende på hello storleken på ditt HDI-kluster). hello första anteckningsboken i hello ovanför listan visar flera olika aspekter av hello datagranskning, visualisering och ML-modell utbildning i en bärbar dator som tar mindre tid toorun med ned provtagning NYC datamängd, i vilken hello taxi och avgiften filer har redan anslutits: [ Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) anteckningsboken tar en mycket kortare tid toofinish (2-3 minuter) och kan vara en bra utgångspunkt för snabbt utforska hello koden har vi föreskrivs Spark 2.0. 

<!-- -->

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<!-- -->

> [!NOTE]
hello beskrivningar nedan är relaterade toousing Spark 1.6. Spark 2.0 versioner, Använd hello anteckningsböcker beskrivs och länkarna ovan. 

<!-- -->

## <a name="setup-storage-locations-libraries-and-hello-preset-spark-context"></a>: Lagringsplatser, bibliotek och hello förinställning Spark-kontexten
Spark är kan tooread och skriva tooAzure Lagringsblob (även kallat WASB). Så något av dina befintliga data som lagras där kan bearbetas med Spark och hello resultatet lagras igen i WASB.

toosave modeller eller filer i WASB hello sökväg behöver toobe som angetts på rätt sätt. hello standard behållaren kopplade toohello Spark-kluster kan refereras med en sökväg som börjar med ”: wasb: / / /”. Andra platser refererar till ”wasb: / /”.

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Ange katalogsökvägar för lagringsplatser i WASB
hello följande kodexempel anger hello platsen för hello data toobe läsa och sparas hello sökväg för hello modellen lagring directory toowhich hello modellen utdata:

    # SET PATHS tooFILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    # SET hello MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT hello FINAL BACKSLASH IN hello PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a>Importera bibliotek
Konfigurera kräver också importera nödvändiga bibliotek. Ange spark kontext och importera nödvändiga bibliotek med hello följande kod:

    # IMPORT LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a>Förinställda Spark-kontexten och PySpark användbara
Hej PySpark-kernel som tillhandahålls med Jupyter-anteckningsböcker har en förinställd kontext. Så behöver du inte tooset hello Spark- eller Hive kontexter explicit innan du börjar arbeta med hello-program som du utvecklar. Dessa kontexter är tillgängliga för dig som standard. Dessa kontexter är:

* SC - Spark 
* sqlContext - för Hive

Hej PySpark-kerneln innehåller vissa fördefinierade ”användbara”, som är särskilda kommandon som kan anropas med %%. Det finns två kommandon som används i följande kodexempel.

* **%% lokala** anger att hello koden i efterföljande rader är toobe körs lokalt. Koden måste vara giltig Python-kod.
* **%% sql -o <variable name>**  kör en Hive-fråga mot hello sqlContext. Om hello -o parametern överförs hello resultatet av hello fråga sparas i hello %% lokala Python kontext som en Pandas DataFrame.

För mer information om hello kärnor för Jupyter-anteckningsböcker och hello fördefinierade ”magics” som de tillhandahåller, se [kernlar som är tillgängliga för Jupyter-anteckningsböcker med HDInsight Spark Linux-kluster i HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).

## <a name="data-ingestion-from-public-blob"></a>Datapåfyllning från offentliga blob
hello första steget i processen för hello datavetenskap är tooingest hello data toobe analyseras från källor där är finns i din data från kartläggning av naturresurser och modellering miljö. hello-miljö är Spark i den här genomgången. Det här avsnittet innehåller hello koden toocomplete en serie aktiviteter:

* mata in hello data exempel toobe modelleras
* Läs i hello inkommande dataset (som lagras som en TSV-fil)
* Formatera och ren hello data
* Skapa och cachelagra objekt (RDDs eller ramar) i minnet
* registrera den som en temp-tabell i SQL-kontext.

Här är hello koden för datapåfyllning.

    # INGEST DATA

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)

    # GET SCHEMA OF hello FILE FROM HEADER
    schema_string = taxi_train_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))


    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )


    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

**UTDATA:**

Tidsåtgång tooexecute ovanför cellen: 51.72 sekunder

## <a name="data-exploration--visualization"></a>Datagranskning & visualiseringen
När hello data har trätt i Spark, är hello nästa steg i processen för hello datavetenskap toogain djupare förståelse för hello data via undersökning och visualisering. I det här avsnittet undersöka vi hello taxi data med hjälp av SQL-frågor och ritytans hello target-variabler och potentiella funktioner för granskning. Mer specifikt Rita vi hello frekvensen av passagerare antal i taxi resor hello frekvensen av tips belopp och hur tips varierar beroende på belopp och typen.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-hello-sample-of-taxi-trips"></a>Rita ett histogram passagerare antal frekvenser i hello prov av taxi resor
Den här koden och efterföljande kodavsnitt använder SQL magiskt tooquery hello exempel och lokala magiskt tooplot hello data.

* **SQL-magic (`%%sql`)** hello HDInsight PySpark-kerneln stöder enkelt infogade HiveQL frågor mot hello sqlContext. hello (-o VARIABLE_NAME) argumentet kvarstår hello utdata från hello SQL-frågan som en Pandas DataFrame på hello Jupyter-servern. Det innebär att den är tillgänglig i hello lokalt läge.
* Hej  **`%%local` magic** är används toorun kod lokalt på hello Jupyter servern som hello headnode hello HDInsight-klustret. Normalt använder du `%%local` magiskt tillsammans med hello `%%sql` magiskt med parametern -o. hello -o parametern skulle bevara hello utdata från lokalt hello SQL-frågan och sedan %% lokala magic initierar hello nästa uppsättning kodfragment toorun lokalt mot hello utdata för hello SQL-frågor som sparas lokalt

hello utdata visualiseras automatiskt när du har kört hello kod.

Den här frågan hämtar hello resor av passagerare count. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST hello sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

Den här koden skapar en lokala data ram från hello frågeresultatet och visar hello data. Hej `%%local` magic skapar lokala data-ram, `sqlResults`, som kan användas för att rita upp med matplotlib. 

> [!NOTE]
> Den här PySpark-magic används flera gånger i den här genomgången. Om hello mängden data är stor, bör du prov toocreate en data-ram som får plats i lokalt minne.
> 
> 

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES. 
    # CLICK ON hello TYPE OF PLOT tooBE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Här är hello kod tooplot hello resor av passagerare antal

    # PLOT PASSENGER NUMBER VS. TRIP COUNTS
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**UTDATA:**

![Resa frekvens av passagerare antal](./media/machine-learning-data-science-spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

Du kan välja bland flera olika typer av grafik (register, cirkeldiagram, rad, område eller fältet) med hjälp av hello **typen** menyn knapparna i hello anteckningsbok. hello-fältet ritytans visas här.

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Rita histogrammet tips belopp och hur mycket tips varierar efter passagerare antal och avgiften belopp.
Använda en SQL-fråga toosample data.

    #PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE

    # HIVEQL QUERY AGAINST hello sqlContext
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped 
    FROM taxi_train 
    WHERE passenger_count > 0 
    AND passenger_count < 7 
    AND fare_amount > 0 
    AND fare_amount < 200 
    AND payment_type in ('CSH', 'CRD') 
    AND tip_amount > 0 
    AND tip_amount < 25


Den här koden cellen använder hello SQL-frågan toocreate tre områden hello data.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # HISTOGRAM OF TIP AMOUNTS AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 100, -2, 20])
    plt.show()


**UTDATA:** 

![Fördelning av tips](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-distribution.png)

![Tips belopp av passagerare antal](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tips belopp med avgiften belopp](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Funktionen teknik, omvandling och data förberedelse för modellering
Det här avsnittet beskriver och hello koden för procedurer används tooprepare data för användning i ML-modell. Den visar hur toodo hello följande uppgifter:

* Skapa en ny funktion med diskretisering timmar till trafik tid buckets
* Index och koda kategoriska funktioner
* Skapa märkt point-objekt för indata till ML-funktioner
* Skapa ett slumpmässigt underordnade dataurval hello och dela upp den i träning och testning uppsättningar
* Funktionen skalning
* Cacheobjekt i minnet

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Skapa en ny funktion med diskretisering timmar till trafik tid buckets
Den här koden visar hur toocreate en ny funktion av diskretisering timmar till trafik tid tidsgrupper och sedan hur toocache hello resulterande data ram i minnet. Om flexibel distribuerade datauppsättningar (RDDs) och ramar data används flera gånger, leder cachelagring tooimproved körningstider. Därför cachelagra vi RDDs och data ramar i flera steg under hello genomgången. 

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # hello .COUNT() GOES THROUGH hello ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES hello COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**UTDATA:** 

126050

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a>Index och koda kategoriska funktioner för indata i modeling funktioner
Det här avsnittet visas hur tooindex eller koda kategoriska funktioner för indata i hello modeling funktioner. Hej modellering och förutsäga funktioner för MLlib kräver funktioner med kategoriska indata toobe indexerade eller kodade tidigare toouse. Beroende på hello modellen måste tooindex eller koda dem på olika sätt:  

* **Trädet-baserade modellering** kräver kategorier toobe kodad som värden (funktion med tre kategorier kan till exempel vara kodad med 0, 1, 2). Detta tillhandahålls av Mllib's [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) funktion. Den här funktionen kodar en strängkolumn för etiketter tooa kolumnen av etikett index som sorteras efter etikett frekvenser. Även om indexeras med numeriska värden för indata- och datahantering hello trädet-baserade algoritmer vara angivna tootreat dem på lämpligt sätt som kategorier. 
* **Logistic och linjär Regression modeller** kräver en hot kodning, till exempel en funktion med tre kategorier kan expanderas till tre funktionen kolumner med varje som innehåller 0 eller 1 beroende på hello kategori av en observationsintervallet. MLlib ger [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) fungerar toodo ett hot kodning. Den här kodaren mappar en kolumn med etiketten index tooa kolumn i binär angreppsmetoderna med högst ett ett-värde. Den här kodningen kan algoritmer som räknar numeriska värden funktioner, till exempel logistic regression toobe tillämpas toocategorical funktioner.

Här är hello kod tooindex och koda kategoriska funktioner:

    # INDEX AND ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES    
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer

    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is hello cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**UTDATA:**

Tidsåtgång tooexecute ovanför cellen: 1,28 sekunder

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Skapa märkt point-objekt för indata till ML-funktioner
Det här avsnittet innehåller kod som visar hur tooindex kategoriska text datatypen som en etikett punkt data och koda den så att det kan vara används tootrain och testa MLlib logistic regression och andra klassificering modeller. Märkt point-objekt är flexibel distribuerade datauppsättningar (RDD) formaterad på ett sätt som krävs av de flesta ML algoritmer i MLlib som indata. En [etikett punkt](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) är en lokal vector kompakta eller utspridd, som är associerade med en etikett/svar.  

Det här avsnittet innehåller kod som visar hur tooindex kategoriska textdata som en [etikett punkt](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) datatypen och koda den så att det kan vara används tootrain och testa MLlib logistic regression och andra klassificering modeller. Märkt point-objekt är flexibel distribuerade datauppsättningar (RDD) som består av en etikett (mål/svar-variabel) och funktionen vector. Det här formatet krävs av många ML algoritmer i MLlib som indata.

Här är hello kod tooindex och koda textfunktioner för binär klassificering.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Här är hello kod tooencode och index kategoriska text-funktioner för linjär regressionsanalys.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])

        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-hello-data-and-split-it-into-training-and-testing-sets"></a>Skapa ett slumpmässigt underordnade dataurval hello och dela upp den i träning och testning uppsättningar
Den här koden skapar ett slumpmässigt dataurval hello (25% används här). Även om det inte krävs för det här exemplet på grund av toohello storlek på hello dataset, visar vi hur du kan prova här så att du vet hur toouse den för din egen problem vid behov. När prover är stort, kan detta spara mycket tid när utbildning modeller. Nästa dela vi hello exempel i en utbildning (här 75%) och en tester del (här 25%) toouse i klassificering och regressionen modeling.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.sql.functions import rand

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)

    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS (FOR USE LATER IN AN ADVANCED TOPIC)
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**UTDATA:**

Tidsåtgång tooexecute ovanför cellen: 0,24 sekunder

### <a name="feature-scaling"></a>Funktionen skalning
Funktionen skalning, även kallat databasnormalisering garanterar att funktioner med brett erläggas värden är inte den angivna överdriven väg i mål hello-funktionen. hello koden för funktionen skalning använder hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) tooscale hello funktioner toounit varians. Den tillhandahålls av MLlib för användning i linjär regression med Stokastisk toning härstammar (SGD), en populär algoritm för att träna en mängd andra maskininlärning modeller som reglerats regressioner eller support vector datorer (SVM).

> [!NOTE]
> Vi har hittat hello LinearRegressionWithSGD algoritmen toobe känsliga toofeature skalning.
> 
> 

Här är hello kod tooscale variabler för användning med hello reglerats linjär SGD algoritm.

    # FEATURE SCALING

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils

    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**UTDATA:**

Tidsåtgång tooexecute ovanför cellen: 13.17 sekunder

### <a name="cache-objects-in-memory"></a>Cacheobjekt i minnet
hello tidsåtgång för träning och testning av ML algoritmer kan minskas med cachelagring hello indata ram objekt som används för klassificering, regression, och skalas funktioner.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()

    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**UTDATA:** 

Tidsåtgång tooexecute ovanför cellen: 0,15 sekunder

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Förutsäga huruvida ett tips är betald med binär klassificering modeller
Det här avsnittet visas hur användning tre modeller för hello binär klassificering uppgiften att förutsäga ett tips betalat för en taxi resa eller inte. hello modeller som visas är:

* Reglerats logistic regression 
* Slumpmässiga Skogsmodell
* Toning träd

Varje modell skapa avsnitt med exempelkod delas upp i steg: 

1. **Modellen utbildning** data med en enda parameter
2. **Modellen utvärdering** på en datauppsättning med test
3. **Spara modellen** i blob för framtida användning

### <a name="classification-using-logistic-regression"></a>Klassificering med logistic regression
hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en logistic regressionsmodell med [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) som beräknar huruvida ett tips är betalat för en resa i hello NYC taxi resa och avgiften dataset.

**Hej logistic regression träningsmodell med KA och hyperparameter omfattande**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics


    # CREATE MODEL WITH ONE SET OF PARAMETERS
    logitModel = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, iterations=20, initialWeights=None, 
                                                   regParam=0.01, regType='l2', intercept=True, corrections=10, 
                                                   tolerance=0.0001, validateData=True, numClasses=2)

    # PRINT COEFFICIENTS AND INTERCEPT OF hello MODEL
    # NOTE: There are 20 coefficient terms for hello 10 features, 
    #       and hello different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitModel.weights))
    print("Intercept: " + str(logitModel.intercept))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**UTDATA:** 

Koefficienter: [0.0082065285375,-0.0223675576104,-0.0183812028036, -3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Skärningspunkt:-0.0111216486893

Tidsåtgång tooexecute ovanför cellen: 14.43 sekunder

**Utvärdera hello binär klassificering modellen med standard**

    #EVALUATE LOGISTIC REGRESSION MODEL WITH LBFGS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # PREDICT ON TEST DATA WITH MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitModel.predict(lp.features)), lp.label))

    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)


    ## SAVE MODEL WITH DATE-STAMP
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    logitModel.save(sc, dirfilename);

    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitModel.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

**UTDATA:** 

Området under PR = 0.985297691373

Området under ROC = 0.983714670256

Sammanfattande statistik

Precision = 0.984304060189

Återkalla = 0.984304060189

F1 Poängsätta = 0.984304060189

Tidsåtgång tooexecute ovanför cellen: 57.61 sekunder

**Rita hello ROC-kurvan.**

Hej *predictionAndLabelsDF* registreras som en tabell, *tmp_results*, i hello föregående cell. *tmp_results* kan använda toodo frågor och spara resultatet i hello sqlResults data-ram för att rita upp. Här är hello kod.

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Här är hello kod toomake förutsägelser och ritytans hello ROC-kurvan.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    # MAKE PREDICTIONS
    predictions_pddf = test_predictions.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVE
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**UTDATA:**

![Logistic regression ROC curve.png](./media/machine-learning-data-science-spark-data-exploration-modeling/logistic-regression-roc-curve.png)

### <a name="random-forest-classification"></a>Slumpmässiga skog klassificering
hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en slumpmässig Skogsmodell som beräknar huruvida ett tips är betalat för en resa i hello NYC taxi resa och avgiften dataset.

    #PREDICT WHETHER A TIP IS PAID OR NOT USING RANDOM FOREST

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT tooPRINT TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**UTDATA:**

Området under ROC = 0.985297691373

Tidsåtgång tooexecute ovanför cellen: 31.09 sekunder

### <a name="gradient-boosting-trees-classification"></a>Toning den träd klassificering
hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en toning den träd modell som beräknar huruvida ett tips är betalat för en resa i hello NYC taxi resa och avgiften dataset.

    #PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
    ## UNCOMMENT IF YOU WANT tooPRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;

    gbtModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**UTDATA:**

Området under ROC = 0.985297691373

Tidsåtgång tooexecute ovanför cellen: 19.76 sekunder

## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a>Förutsäga tips belopp för taxi resor med regression modeller
Det här avsnittet visas hur användning tre modeller för hello regression uppgiften att förutsäga hello mängden hello tips för en taxi färd baserat på andra tips funktioner. hello modeller som visas är:

* Reglerats linjär regression
* Slumpmässiga skog
* Toning träd

Dessa modeller beskrivs i hello introduktion. Varje modell skapa avsnitt med exempelkod delas upp i steg: 

1. **Modellen utbildning** data med en enda parameter
2. **Modellen utvärdering** på en datauppsättning med test
3. **Spara modellen** i blob för framtida användning

### <a name="linear-regression-with-sgd"></a>Linjär regression med SGD
hello kod i det här avsnittet visar hur skalas toouse funktioner tootrain en linjär regression som använder stokastisk toning härstammar (SGD) för optimering, och hur tooscore, utvärdera och spara hello modellen i Azure Blob Storage (WASB).

> [!TIP]
> Det kan finnas problem med hello konvergens LinearRegressionWithSGD modeller i vår erfarenhet och parametrar måste toobe ändras/optimerad noggrant för att erhålla en giltig modell. Skalning av variabler avsevärt hjälper med konvergens. 
> 
> 

    #PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats

    # USE SCALED FEATURES tooTRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF hello MODEL
    # NOTE: There are 20 coefficient terms for hello 10 features, 
    #       and hello different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))

    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)

    # PRINT TEST METRICS
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL WITH DATE-STAMP IN hello DEFAULT BLOB FOR hello CLUSTER
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;

    linearModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**UTDATA:**

Koefficienter: [0.00457675809917,-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,-0.00990211159703,-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]

Fånga: 0.853872718283

RMSE = 1.24190115863

R-sqr = 0.608017146081

Tidsåtgång tooexecute ovanför cellen: 58,42 sekunder

### <a name="random-forest-regression"></a>Slumpmässiga skog regression
hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en slumpmässig skog regression som beräknar tips belopp för hello NYC taxi resa data.

    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics


    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    ## UN-COMMENT IF YOU WANT tooPRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    ## PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**UTDATA:**

RMSE = 0.891209218139

R-sqr = 0.759661334921

Tidsåtgång tooexecute ovanför cellen: 49.21 sekunder

### <a name="gradient-boosting-trees-regression"></a>Toning den träd regression
hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en toning den träd modell som beräknar tips belopp för hello NYC taxi resa data.

** Träna och utvärdera **

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils

    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)

    ## EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)

    # CONVER RESULTS tooDF AND REGISER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**UTDATA:**

RMSE = 0.908473148639

R-sqr = 0.753835096681

Tidsåtgång tooexecute ovanför cellen: 34.52 sekunder

**Rita**

*tmp_results* registreras som en Hive-tabell i hello föregående cell. Resultat från hello tabellen är resultatet i hello *sqlResults* data ram för att rita upp. Här är hello kod

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

Här är hello kod tooplot hello data med hjälp av hello Jupyter-server.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    import numpy as np

    # PLOT 
    ax = test_predictions_pddf.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(test_predictions_pddf['_1'], test_predictions_pddf['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(test_predictions_pddf['_1'], fit[0] * test_predictions_pddf['_1'] + fit[1], color='magenta')
    plt.axis([-1, 20, -1, 20])
    plt.show(ax)


**UTDATA:**

![Aktuella-vs-förutsade-tips-belopp](./media/machine-learning-data-science-spark-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="clean-up-objects-from-memory"></a>Rensa objekt från minnet
Använd `unpersist()` toodelete objekt som cachelagrats i minnet.

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()

    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


## <a name="record-storage-locations-of-hello-models-for-consumption-and-scoring"></a>Posten lagringsplatser för hello modeller för användnings- och poängsättning
tooconsume och poäng en oberoende datauppsättning beskrivs i hello [poängsätta och utvärdera Spark-inbyggda machine learning-modeller](machine-learning-data-science-spark-model-consumption.md) avsnittet, behöver du toocopy och klistra in filnamn som innehåller hello sparade modeller som skapas här i hello Förbrukning Jupyter-anteckningsbok. Här är hello kod tooprint ut hello sökvägar toomodel filer du behöver det.

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**UTDATA**

logisticRegFileLoc = modelDir + ”LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568”

linearRegFileLoc = modelDir + ”LinearRegressionWithSGD_2016-05-0317_05_21.577773”

randomForestClassificationFileLoc = modelDir + ”RandomForestClassification_2016-05-0317_04_11.950206”

randomForestRegFileLoc = modelDir + ”RandomForestRegression_2016-05-0317_06_08.723736”

BoostedTreeClassificationFileLoc = modelDir + ”GradientBoostingTreeClassification_2016-05-0317_04_36.346583”

BoostedTreeRegressionFileLoc = modelDir + ”GradientBoostingTreeRegression_2016-05-0317_06_51.737282”

## <a name="whats-next"></a>Nästa steg
Nu när du har skapat regression och klassificering modeller med hello Spark MlLib, är du redo toolearn hur tooscore och utvärdera dessa modeller. hello avancerade datagranskning och modellering anteckningsboken dyker ned djupare i inklusive korsvalidering, hyper-parametern omfattande och modellerar utvärdering. 

**Modellen förbrukning:** toolearn hur tooscore och utvärdera hello klassificering och regression modeller som skapats i det här avsnittet, finns [poängsätta och utvärdera Spark-inbyggda machine learning-modeller](machine-learning-data-science-spark-model-consumption.md).

**Korsvalidering och hyperparameter omfattande**: se [avancerade datagranskning och modellering med Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) på hur modeller kan vara tränas med omfattande korsvalidering och hyper-parameter

