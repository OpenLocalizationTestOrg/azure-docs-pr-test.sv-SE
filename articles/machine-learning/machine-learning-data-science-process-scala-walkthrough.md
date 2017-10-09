---
title: "aaaData vetenskap med hjälp av Scala och Spark på Azure | Microsoft Docs"
description: "Hur hello toouse Scala för övervakade maskininlärning uppgifter med Spark skalbara MLlib och Spark ML paket i ett Azure HDInsight Spark-kluster."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: a7c97153-583e-48fe-b301-365123db3780
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;deguhath
ms.openlocfilehash: e32ebd0b91417183fe48ee10ebc7929fd9605762
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-science-using-scala-and-spark-on-azure"></a>Datavetenskap med Scala och Spark på Azure
Den här artikeln visar hur toouse Scala för övervakade machine learning aktiviteter med hello Spark skalbara MLlib och Spark ML-paket i ett Azure HDInsight Spark-kluster. Den vägleder dig genom hello aktiviteter som utgör hello [datavetenskap processen](http://aka.ms/datascienceprocess): datapåfyllning och undersökning, visualisering, funktionen tekniker, modellering och förbrukning av modellen. hello modeller i hello artikeln omfattar logistic och linjär regression, slumpmässiga skogar och ökat toningen träd (GBTs), dessutom tootwo gemensamma övervakad machine learning uppgifter:

* Regression problem: förutsägelse av hello-tips ($) för en taxi resa
* Binär klassificering: förutsägelser av tips eller inget tips (1/0) för en taxi resa

hello modellera processen kräver träning och utvärdering på en datauppsättning test och relevanta noggrannhet mått. I den här artikeln får du veta hur toostore dessa modeller i Azure Blob storage och hur tooscore och utvärdera deras förutsägbar prestanda. Den här artikeln omfattar också hello mer avancerade alternativ för hur toooptimize modeller med omfattande korsvalidering och hyper-parametern. hello-data som används är ett exempel på hello 2013 NYC taxi resa och avgiften datauppsättning finns på GitHub.

[Scala](http://www.scala-lang.org/), ett språk baserat på hello Java virtual machine integrerar Språkbegrepp för objektorienterad och fungerar. Det är ett skalbart språk som är bra toodistributed bearbetning i hello molnet och körs på Azure Spark-kluster.

[Spark](http://spark.apache.org/) ett ramverk för parallell bearbetning av öppen källkod som stöder i minnet bearbetar tooboost hello prestandan för stordata analytics program. Hej bearbetningsmotorn i Spark är byggd för hastighet, enkel användning och avancerade analyser. Sparks funktioner för beräkning för distribuerade i minnet gör det ett bra alternativ för iterativa algoritmer i machine learning och grafberäkningar. Hej [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) paketet ger en enhetlig uppsättning övergripande API: er som bygger på data ramar som kan hjälpa dig att skapa och finjustera praktiska maskininlärning pipelines. [MLlib](http://spark.apache.org/mllib/) är Sparks skalbara machine learning-biblioteket, som ger funktioner för modellering toothis distribuerad miljö.

[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) är hello Azure-baserad erbjuds med öppen källkod Spark. Den omfattar också stöd för Jupyter Scala-anteckningsböcker på hello Spark-kluster och kan köra Spark SQL interaktiva frågor tootransform filtrera och visualisera data som lagras i Azure Blob storage. Hej Scala kodfragment i den här artikeln som tillhandahåller hello lösningar och visa hello relevanta områden toovisualize hello data körs i Jupyter-anteckningsböcker som är installerad på hello Spark-kluster. hello modellering stegen i följande avsnitt har kod som visar dig hur tootrain, utvärdera, spara och använda varje typ av modellen.

hello konfigurationsstegen och kod i den här artikeln gäller för Azure HDInsight 3.4 Spark 1.6. Dock hello koden i den här artikeln och hello [Scala Jupyter-anteckningsbok](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) är allmänna och bör fungera på Spark-kluster. Hej konfiguration och hantering av steg kan vara skiljer sig från vad som anges i den här artikeln om du inte använder HDInsight Spark.

> [!NOTE]
> Ett avsnitt som visar hur toouse Python snarare än Scala toocomplete aktiviteter för en datavetenskap slutpunkt till slutpunkt-processen, se [datavetenskap med Spark på Azure HDInsight](machine-learning-data-science-spark-overview.md).
> 
> 

## <a name="prerequisites"></a>Krav
* Du måste ha en Azure-prenumeration. Om du inte redan har en, [hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Du behöver ett Azure HDInsight 3.4 Spark 1.6 klustret toocomplete hello följande procedurer. toocreate ett kluster, se hello-instruktioner i [komma igång: skapa Apache Spark på Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Ange typ av hello kluster och version för hello **Välj typ av kluster** menyn.

![HDInsight klusterkonfiguration för typen](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)

> [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
> 
> 

En beskrivning av hello NYC taxi resa data och anvisningar om hur tooexecute kod från en Jupyter-anteckningsbok på hello Spark-kluster finns i hello relevanta avsnitt i [översikt av datavetenskap med Spark på Azure HDInsight](machine-learning-data-science-spark-overview.md).  

## <a name="execute-scala-code-from-a-jupyter-notebook-on-hello-spark-cluster"></a>Köra Scala kod från en Jupyter-anteckningsbok på hello Spark-kluster
Du kan starta en Jupyter-anteckningsbok från hello Azure-portalen. Hitta hello Spark-kluster på instrumentpanelen och klicka sedan på tooenter hello hanteringssidan för klustret. Klicka sedan på **Klusterinstrumentpaneler**, och klicka sedan på **Jupyter-anteckningsbok** tooopen hello bärbar dator som är associerade med hello Spark-kluster.

![Instrumentpanelen klustret och Jupyter-anteckningsböcker](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

Du kan också komma åt Jupyter-anteckningsböcker med https://&lt;clustername&gt;.azurehdinsight.net/jupyter. Ersätt *klusternamn* med hello namnet på klustret. Du behöver hello lösenord för din administratör konto tooaccess hello Jupyter-anteckningsböcker.

![Gå tooJupyter bärbara datorer med hjälp av hello klustrets namn](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

Välj **Scala** toosee en katalog som är några exempel på färdigförpackade bärbara datorer att använda hello PySpark-API. Hej utforskning modellering och beräkning med Scala.ipynb anteckningsbok med hello kodexempel för den här sviten Spark avsnitt är tillgängliga på [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).

Du kan ladda upp hello anteckningsboken direkt från GitHub toohello Jupyter-anteckningsbok server på Spark-kluster. Klicka på startsidan Jupyter hello **överför** knappen. Klistra in hello GitHub (rådata innehåll) URL för hello Scala anteckningsboken i hello Utforskaren och klicka sedan på **öppna**. Hej Scala anteckningsboken finns på hello följande URL:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Konfigurera: Förinställda Spark och Hive-kontexterna, Spark användbara och Spark-bibliotek
### <a name="preset-spark-and-hive-contexts"></a>Förinställningen Spark och Hive-kontexterna
    # SET hello START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


hello Spark kernlar som tillhandahålls med Jupyter-anteckningsböcker har förinställda sammanhang. Du behöver inte tooexplicitly set hello Spark- eller Hive kontexter innan du börjar arbeta med hello-program som du utvecklar. hello förinställda kontexter är:

* `sc`för SparkContext
* `sqlContext`för HiveContext

### <a name="spark-magics"></a>Spark användbara
hello Spark kernel innehåller vissa fördefinierade ”användbara”, som är särskilda kommandon som kan anropas med `%%`. Två av dessa kommandon som används i följande kodexempel hello.

* `%%local`Anger att hello koden i efterföljande rader körs lokalt. hello-koden måste vara giltig Scala-kod.
* `%%sql -o <variable name>`Kör en Hive-fråga mot `sqlContext`. Om hello `-o` -parameter har skickats, hello resultatet av hello fråga sparas i hello `%%local` Scala kontext som en Spark data ram.

För mer information om hello kärnor för Jupyter-anteckningsböcker och deras fördefinierade ”magics” som du anropar med `%%` (till exempel `%%local`), se [kernlar som är tillgängliga för Jupyter-anteckningsböcker med HDInsight Spark Linux-kluster på HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).

### <a name="import-libraries"></a>Importera bibliotek
Importera hello Spark, MLlib och andra bibliotek som du behöver genom att använda hello följande kod.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Datainhämtning
hello första steget i hello datavetenskap processen är tooingest hello data som du vill tooanalyze. Du kan aktivera hello data från externa källor eller system där den finns i din data från kartläggning av naturresurser och modellering miljö. I den här artikeln är hello data du mata in en domänansluten 0,1% exempel hello taxi resa och avgiften filen (som lagras som en TSV-fil). hello data från kartläggning av naturresurser och modellering miljö är Spark. Det här avsnittet innehåller följande rad uppgifter hello kod toocomplete hello:

1. Ange katalogsökvägar för lagring av data och modell.
2. Läs i hello inkommande datamängd (som lagras som en TSV-fil).
3. Definiera ett schema för hello data och ren hello data.
4. Skapa en rensade data ram och cachelagra i minnet.
5. Registrera hello data som en tillfällig tabell i SQLContext.
6. Fråga hello tabellen och importera hello resultaten till en data-ram.

### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Ange katalogsökvägar för lagringsplatser i Azure Blob storage
Spark kan läsa och skriva tooAzure Blob storage. Du kan använda Spark tooprocess någon av dina befintliga data och lagra om hello resultat i Blob storage.

toosave modeller eller filer i Blob storage, behöver du tooproperly ange hello sökväg. Referens hello standardbehållaren kopplad toohello Spark-kluster med hjälp av en sökväg som börjar med `wasb:///`. Referera till andra platser med hjälp av `wasb://`.

hello anger följande kodexempel hello plats hello indata toobe läsa och hello sökvägen tooBlob lagring som är bifogade toohello Spark-kluster där hello modellen ska sparas.

    # SET PATHS tooDATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET hello MODEL STORAGE DIRECTORY PATH
    # NOTE THAT hello FINAL BACKSLASH IN hello PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-toohello-schema"></a>Importera data, skapa en RDD och definierar data enligt toohello schema
    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE hello SCHEMA BASED ON hello HEADER OF hello FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING toohello SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE hello CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER hello DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**Utdata:**

Tid toorun hello cell: 8 sekunder.

### <a name="query-hello-table-and-import-results-in-a-data-frame"></a>Fråga hello tabellen och importera resulterar i en data-ram
Nästa hello frågetabellen för avgiften, resande och tips data. Filtrera bort skadad och öar data. och Skriv ut flera rader.

    # QUERY hello DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY hello TOP THREE ROWS
    sqlResultsDF.show(3)

**Utdata:**

| fare_amount | passenger_count | tip_amount | lutad |
| --- | --- | --- | --- |
|        13.5 |1.0 |2.9 |1.0 |
|        16.0 |2.0 |3.4 |1.0 |
|        10.5 |2.0 |1.0 |1.0 |

## <a name="data-exploration-and-visualization"></a>Datagranskning och visualisering
När du sätta hello data i Spark är hello nästa steg i hello datavetenskap processen toogain en djupare förståelse för hello data via undersökning och visualisering. I det här avsnittet kan undersöka du hello taxi data med hjälp av SQL-frågor. Sedan hello hello importresultaten till en data ram tooplot target-variabler och potentiella funktioner för granskning med funktionen hello automatiskt visualisering av Jupyter.

### <a name="use-local-and-sql-magic-tooplot-data"></a>Använda lokala och SQL magiskt tooplot data
Hello utdata från alla kodstycke som du kör från en Jupyter-anteckningsbok är tillgängliga i hello kontexten för hello-session som är kvar på hello arbetarnoder som standard. Om du vill toosave en resa toohello arbetarnoder för varje beräkning och om alla hello data som du behöver för din beräkning är tillgängliga lokalt på hello Jupyter-servernoden (vilket är hello huvudnod), kan du använda hello `%%local` magiska toorun hello kod fragment på hello Jupyter-servern.

* **SQL-magic** (`%%sql`). Hej HDInsight Spark kernel stöder enkelt infogade HiveQL frågor mot SQLContext. hello (`-o VARIABLE_NAME`) argumentet kvarstår hello utdata från hello SQL-frågan som en Pandas data ram på hello Jupyter-servern. Det innebär att den ska vara tillgänglig i hello lokalt läge.
* `%%local`**magic**. Hej `%%local` magic körs hello kod lokalt på hello Jupyter servern som hello huvudnod hello HDInsight-kluster. Normalt använder du `%%local` magiskt tillsammans med hello `%%sql` magiskt med hello `-o` parameter. Hej `-o` parametern skulle bevara hello utdata från hello SQL-frågan lokalt, och sedan `%%local` magic initierar hello nästa uppsättning kodfragment toorun lokalt mot hello utdata för hello SQL-frågor som sparas lokalt.

### <a name="query-hello-data-by-using-sql"></a>Fråga hello data med hjälp av SQL
Den här frågan hämtar hello taxi resor av avgiften, passagerare antal, och tips belopp.

    # RUN hello SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

I följande kod hello, hello `%%local` magic skapas en lokala data, sqlResults. Du kan använda sqlResults tooplot med hjälp av matplotlib.

> [!TIP]
> Lokala magic används flera gånger i den här artikeln. Om din datauppsättning är stor, ta prov toocreate en data-ram som får plats i lokalt minne.
> 
> 

### <a name="plot-hello-data"></a>Rita hello data
Du kan rita med hjälp av Python-kod när hello dataramen är i lokala kontexten som en Pandas data ram.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES.
    # CLICK hello TYPE OF PLOT tooGENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 hello Spark kernel visualizes automatiskt hello utdata för SQL (HiveQL)-frågor när du har kört hello kod. Du kan välja mellan flera olika typer av visualiseringar:

* Tabell
* Cirkeldiagram
* Rad
* Område
* Fältet

Här är hello kod tooplot hello data:

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**Utdata:**

![Tips belopp histogram](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Tips belopp av passagerare antal](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Tips belopp med avgiften belopp](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)

## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>Skapa funktioner och transformera funktioner och Förbered dig för indata i modeling funktioner
Trädet-baserade modelleringsfunktioner från Spark ML och MLlib har du tooprepare mål och funktioner med hjälp av en mängd olika tekniker, till exempel diskretisering, indexering, en hot kodning och vectorization. Här följer hello procedurer toofollow i det här avsnittet:

1. Skapa en ny funktion av **diskretisering** timmar till trafik tid buckets.
2. Tillämpa **indexering och en hot kodning** toocategorical funktioner.
3. **Exempel och dela hello datauppsättning** till bråk träning och testning.
4. **Ange variabeln utbildning och funktioner**, och sedan skapa indexerade eller ett hot kodade utbildning och testar inkommande märkt punkt flexibel distribueras datauppsättningar (RDDs) eller ramar.
5. Automatiskt **kategorisera och vectorize funktioner och mål** toouse som indata för machine learning-modeller.

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Skapa en ny funktion med diskretisering timmar till trafik tid buckets
Den här koden visar hur toocreate en ny funktion av diskretisering timmar till trafik tid tidsgrupper och hur toocache hello resulterande data ram i minnet. Om RDDs och data används flera gånger, leder cachelagring tooimproved körningstider. Du måste därför cachelagra RDDs och data i flera steg under hello följande procedurer.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE hello DATA FRAME IN MEMORY AND MATERIALIZE hello DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Indexering och en hot kodning av kategoriska funktioner
Hej modellering och förutsäga funktioner för MLlib kräver funktioner med kategoriska indata toobe indexerade eller kodade tidigare toouse. Det här avsnittet beskrivs hur du tooindex eller koda kategoriska funktioner för indata i hello modeling funktioner.

Du måste tooindex eller koda modeller på olika sätt beroende på hello modellen. Till exempel kräva logistic och linjär regression modeller ett hot kodning. Till exempel kan en funktion med tre kategorier expanderas i tre kolumner i funktionen. Varje kolumn innehåller 0 eller 1 beroende på hello kategori av en observationsintervallet. MLlib ger hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funktionen för en hot kodning. Den här kodaren mappar en kolumn med etiketten index tooa kolumn i binär angreppsmetoderna med högst ett ett-värde. Med den här kodningen vara algoritmer som numeriska värden funktioner, till exempel logistic regression förväntas tillämpade toocategorical funktioner.

Här kan du omvandla bara fyra variabler tooshow exempel som är strängar. Du kan också indexera andra variabler, till exempel veckodag representeras av numeriska värden som kategoriska variabler.

Använd för indexering, `StringIndexer()`, och för ett hot encoding använder `OneHotEncoder()` funktioner från MLlib. Här är hello kod tooindex och koda kategoriska funktioner:

    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**Utdata:**

Tid toorun hello cell: 4 sekunder.

### <a name="sample-and-split-hello-data-set-into-training-and-test-fractions"></a>Exempel och dela hello datauppsättning till bråk träning och testning
Den här koden skapar ett slumpmässigt dataurval hello (i det här exemplet 25%). Även om sampling inte krävs för det här exemplet på grund av toohello storlek på hello datauppsättning, hello artikeln lär du dig hur du kan prova så att du vet hur toouse den egna problem vid behov. När prover är stort, kan detta spara mycket tid medan du träna modeller. Dela sedan hello exempel i en utbildning (75% i det här exemplet) och en testnings-del (i det här exemplet 25%) toouse i klassificering och regressionen modeling.

Lägg till ett slumptal (mellan 0 och 1) tooeach rad (i en kolumn ”SLUMP”) som kan vara används tooselect korsvalidering vikningar vid träning.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT hello SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**Utdata:**

Tid toorun hello cell: 2 sekunder.

### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Ange utbildning variabeln och funktioner, och skapa indexerade eller ett hot kodade träning och testning indata med etiketten punkt RDDs eller data ramar
Det här avsnittet innehåller kod som visar hur tooindex kategoriska textdata som en etikett peka datatyp och koda den så att du kan använda tootrain och testa MLlib logistic regression och andra klassificering modeller. Märkt point-objekt är RDDs som är formaterad på ett sätt som krävs av de flesta maskininlärningsalgoritmer i MLlib som indata. En [etikett punkt](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) är en lokal vector kompakta eller utspridd, som är associerade med en etikett/svar.

I den här koden anger du hello (beroende) målvariabel och hello funktioner toouse tootrain modeller. Sedan kan skapa du indexerade eller ett hot kodade träning och testning indata med etiketten punkt RDDs eller data ramar.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY hello TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE tooTRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE tooTRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**Utdata:**

Tid toorun hello cell: 4 sekunder.

### <a name="automatically-categorize-and-vectorize-features-and-targets-toouse-as-inputs-for-machine-learning-models"></a>Kategorisera och vectorize funktioner och mål toouse som indata för machine learning-modeller automatiskt
Använda Spark ML toocategorize hello mål och funktioner toouse i trädet-baserade modelleringsfunktioner. hello koden utför två aktiviteter:

* Skapar ett binärt mål för klassificering genom att tilldela värdet 0 eller 1 tooeach datapunkt mellan 0 och 1 med hjälp av ett tröskelvärde för 0,5.
* Automatiskt kategoriserar funktioner. Om hello antalet distinkta numeriska värden för alla funktioner som är mindre än 32, kategoriseras den funktionen.

Här är hello koden för dessa två aktiviteter.

    # CATEGORIZE FEATURES AND BINARIZE hello TARGET FOR hello BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR hello REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>Binär klassificering modell: förutsäga om tipsrutor ska användas för betalning
I det här avsnittet kan du skapa tre olika typer av binär klassificering modeller toopredict huruvida ett tips betald:

* En **logistic regressionsmodell** med hjälp av hello Spark ML `LogisticRegression()` funktion
* En **slumpmässiga klassificering Skogsmodell** med hjälp av hello Spark ML `RandomForestClassifier()` funktion
* En **toning den trädet klassificering modellen** med hjälp av hello MLlib `GradientBoostedTrees()` funktion

### <a name="create-a-logistic-regression-model"></a>Skapa en logistic regressionsmodell
Skapa sedan en logistic regressionsmodell med hjälp av hello Spark ML `LogisticRegression()` funktion. Du skapar hello modell skapa koden i ett antal steg:

1. **Hej träningsmodell** data med en enda parameter.
2. **Utvärdera hello modellen** på en datauppsättning med test.
3. **Spara hello modell** i Blob storage för framtida användning.
4. **Hej poängmodell** mot testdata.
5. **Rita hello resultat** med operativsystemet kurvor egenskap (ROC).

Här är hello koden för dessa procedurer:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON hello TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` tooCOMPUTE hello TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE hello MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Läsa in poängsätta och spara hello resultat.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD hello SAVED MODEL AND SCORE hello TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE hello MODEL ON hello TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` tooCOMPUTE hello TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello ROC RESULTS
    println("ROC on test data = " + ROC)


**Utdata:**

ROC på testdata = 0.9827381497557599

Använda Python på lokala Pandas data ramar tooplot hello ROC-kurvan.

    # QUERY hello RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT hello ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT hello ROC CURVE
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


**Utdata:**

![Tips eller några tips ROC-kurvan](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)

### <a name="create-a-random-forest-classification-model"></a>Skapa en modell för klassificering av slumpmässiga skog
Skapa sedan en klassificering för slumpmässiga Skogsmodell med hello Spark ML `RandomForestClassifier()` fungera och sedan utvärdera hello modell på testdata.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE hello RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT hello MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE hello MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**Utdata:**

ROC på testdata = 0.9847103571552683

### <a name="create-a-gbt-classification-model"></a>Skapa en modell för klassificering av GBT
Skapa sedan en modell för klassificering av GBT med hjälp av Mllib's `GradientBoostedTrees()` fungera och sedan utvärdera hello modell på testdata.

    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE hello GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN hello MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE hello MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE hello MODEL ON TEST INSTANCES AND hello COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS tooEVALUATE hello MODEL ON hello TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**Utdata:**

Området under ROC-kurvan: 0.9846895479241554

## <a name="regression-model-predict-tip-amount"></a>Regressionsmodell: förutsäga tips belopp
I det här avsnittet kan du skapa två typer av regression modeller toopredict hello tips:

* En **reglerats linjär regressionsmodell** med hjälp av hello Spark ML `LinearRegression()` funktion. Du måste spara hello modell och utvärdera hello modell på testdata.
* En **toningen förstärkning trädet regressionsmodell** med hjälp av hello Spark ML `GBTRegressor()` funktion.

### <a name="create-a-regularized-linear-regression-model"></a>Skapa en reglerats linjär regressionsmodell
    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING hello SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT hello MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE hello MODEL OVER hello TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE hello MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT hello COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE hello MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE hello MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**Utdata:**

Tid toorun hello cell: 13 sekunder.

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE hello MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE hello MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello RESULTS
    println("R-sqr on test data = " + r2)


**Utdata:**

R-sqr på testdata = 0.5960320470835743

Nästa fråga hello test resulterar som en data- och Använd AutoVizWidget och matplotlib toovisualize den.

    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES
    # CLICK hello TYPE OF PLOT tooGENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

hello kod skapas en lokala data från hello frågeresultatet och visar hello data. Hej `%%local` magic skapar en lokala data ram `sqlResults`, som du kan använda tooplot med matplotlib.

> [!NOTE]
> Den här Spark magic används flera gånger i den här artikeln. Om hello mängden data är stor, bör du prov toocreate en data-ram som får plats i lokalt minne.
> 
> 

Skapa områden med Python matplotlib.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT hello RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**Utdata:**

![Tips belopp: faktiska kontra förväntade](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)

### <a name="create-a-gbt-regression-model"></a>Skapa en GBT regressionsmodell
Skapa en GBT regressionsmodell med hjälp av hello Spark ML `GBTRegressor()` fungera och sedan utvärdera hello modell på testdata.

[Ökat toningen träd](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) är ensembler av beslutsträd. GBTs train beslut träd upprepade gånger toominimize en förlust-funktion. Du kan använda GBTs för regression och klassificering. De kan hantera kategoriska funktioner kräver inte funktionen skalning och kan avbilda nonlinearities och funktionen interaktioner. Du kan också använda dem i en multiclass klassificering.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello RESULTS
    println("Test R-sqr is: " + Test_R2);


**Utdata:**

Testa R-sqr är: 0.7655383534596654

## <a name="advanced-modeling-utilities-for-optimization"></a>Avancerade modellering verktyg för optimering
I det här avsnittet använder du machine learning verktyg som utvecklare använder ofta för optimering av modellen. Mer specifikt kan du optimera machine learning-modeller tre olika sätt med hjälp av parametern omfattande och korsvalidering:

* Dela hello data i tåg och validering uppsättningar, optimera hello modellen med hjälp av hyper-parametern omfattande på en träningsmängden och utvärdera en verifiering mängd (linjär regression)
* Optimera hello modellen med hjälp av korsvalidering och hyper-parametern omfattande med funktionen Spark ML CrossValidator (binär klassificering)
* Optimera hello modellen med hjälp av anpassad kod för korsvalidering och parametern omfattande toouse alla maskininlärning funktion och parametern uppsättning (linjär regression)

**Korsvalidering** är en teknik som utvärderar hur väl en modell med en känd uppsättning data kommer att generalisera toopredict hello funktioner för datauppsättningar som den inte har tränats. hello är allmän uppfattning bakom den här metoden att en modell tränas på en datauppsättning med kända data, och sedan hello korrektheten i dess förutsägelser kontrolleras mot en oberoende datamängd. En vanlig tillämpning är toodivide en datamängd i *k*-vikningar och träna hello modell i ett resursallokering sätt på alla utom en av hello vikningar.

**Hyper-parametern optimering** hello problem med att välja en uppsättning hyper-parametrar för en inlärningsalgoritm normalt med hello målet av optimering av ett mått på hello algoritmen prestanda på en fristående uppsättning data. Hyper-parametern är ett värde som du måste ange utanför hello modellen utbildning procedur. Antaganden om hyper-parametervärden kan påverka hello flexibilitet och korrektheten i hello modellen. Beslutsträd har hyper-parametrar, till exempel som hello önskad djup och många blad i hello-trädet. Du måste ange en felklassificering påföljder term för en support vector machine (SVM).

Ett vanligt sätt tooperform hyper parametern optimering är toouse en rutnätet sökning, kallas även en **parametern Svep**. I ett rutnät sökning utförs en fullständig sökning via hello värdena för en angiven delmängd hello hyper parametern utrymme för en inlärningsalgoritm. Korsvalidering kan tillhandahålla en prestanda mått toosort ut hello bästa möjliga resultat som produceras av hello rutnätet Sök algoritm. Om du använder korsvalidering hyper parametern omfattande, kan du hjälpa gränsen problem angående overfitting en tootraining modelldata. Det här sättet hello modellen behåller hello kapacitet tooapply toohello allmän uppsättning data från vilken hello träningsdata har extraherats.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>Optimera en linjär regressionsmodell med hyper-parametern omfattande
Därefter dela in data i tåg och validering uppsättningar, Använd hyper-parametern omfattande på en utbildning ange toooptimize hello modellen och utvärdera en verifiering mängd (linjär regression).

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE hello ESTIMATOR FUNCTION: `hello LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE hello PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE hello PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN hello TRAINING SET), AND THEN hello SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN hello TRAIN VALIDATION SPLIT AND CHOOSE hello BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON hello TEST DATA BY USING hello MODEL WITH hello COMBINATION OF PARAMETERS THAT PERFORMS hello BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**Utdata:**

Testa R-sqr är: 0.6226484708501209

### <a name="optimize-hello-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>Optimera hello binär klassificering modellen med hjälp av korsvalidering och hyper-parametern omfattande
Det här avsnittet visar hur toooptimize en binär klassificering modellen med hjälp av omfattande korsvalidering och hyper-parametern. Här används hello Spark ML `CrossValidator` funktion.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS tooUSE WITH hello TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE hello ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE hello PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY hello NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE hello TRAIN/TEST VALIDATION SPLIT (75% IN hello TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN hello TRAIN VALIDATION SPLIT AND CHOOSE hello BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON hello TEST DATA BY USING hello MODEL WITH hello COMBINATION OF PARAMETERS THAT PERFORMS hello BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE hello TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**Utdata:**

Tid toorun hello cell: 33 sekunder.

### <a name="optimize-hello-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>Optimera hello linjär regressionsmodell med hjälp av anpassade korsvalidering och parametern omfattande kod
Sedan optimera hello modellen med hjälp av anpassade kod och identifiera hello bästa modellparametrarna med hjälp av hello kriterium högsta noggrannhet. Skapa sedan hello sista modellen, utvärdera hello modell på testdata och spara hello modell i Blob storage. Slutligen ladda hello modell poängsätta testdata och utvärdera noggrannhet.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE hello PARAMETER GRID AND hello NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY hello NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND hello PARAMETER GRID tooGET AND IDENTIFY hello BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 too(nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 too(numModels-1)) {
            for (nParams <- 0 too(numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET hello BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 too(numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE hello BEST MODEL WITH hello BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE hello BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON hello TRAINING SET WITH hello BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


    # LOAD hello MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST hello MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**Utdata:**

Tid toorun hello cell: 61 sekunder.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>Använda Spark-inbyggda machine learning-modeller automatiskt med Scala
En översikt över ämnen som vägleder dig genom hello aktiviteter som utgör hello datavetenskap processen i Azure finns [Team datavetenskap Process](http://aka.ms/datascienceprocess).

[Teamet datavetenskap Process genomgång](data-science-process-walkthroughs.md) beskriver andra slutpunkt till slutpunkt-genomgång som visar hello stegen i hello Team datavetenskap Process för specifika scenarier. hello genomgång också illustrerar hur toocombine molnet och lokala verktyg och tjänster i ett arbetsflöde eller pipeline toocreate ett intelligent program.

[Poängsätta Spark-inbyggda machine learning-modeller](machine-learning-data-science-spark-model-consumption.md) visar hur toouse Scala kod tooautomatically läsa in och poängsätta nya datauppsättningar med machine learning-modeller inbyggd Spark och sparas i Azure Blob storage. Du kan följa hello anvisningarna det och ersätter bara hello Python-kod med Scala kod i den här artikeln för automatisk användning.

