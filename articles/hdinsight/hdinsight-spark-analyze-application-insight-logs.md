---
title: aaaAnalyze Application Insights loggar med Spark - Azure HDInsight | Microsoft Docs
description: "Lär dig hur tooexport Application Insights loggar tooblob lagring och analysera hello loggar med Spark i HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 883beae6-9839-45b5-94f7-7eb0f4534ad5
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: 11ed8cf68dba8d5f9d6e4a65eba0d2b5a950cd00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-application-insights-telemetry-logs-with-spark-on-hdinsight"></a>Analysera Application Insights telemetri loggar med Spark i HDInsight

Lär dig hur toouse Väck på HDInsight tooanalyze programmet inblick telemetridata.

[Visual Studio Application Insights](../application-insights/app-insights-overview.md) är en analytics-tjänst som övervakar dina webbprogram. Telemetridata som genererats av Application Insights kan vara exporterade tooAzure lagring. När hello data finns i Azure Storage, HDInsight kan vara används tooanalyze den.

## <a name="prerequisites"></a>Krav

* Ett program som är konfigurerad toouse Application Insights.

* Om du är bekant med att skapa ett Linux-baserade HDInsight-kluster. Mer information finns i [skapa Spark i HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

  > [!IMPORTANT]
  > hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* En webbläsare.

hello har följande resurser använts i utvecklar och testar det här dokumentet:

* Application Insights telemetridata genererades med en [Node.js-webbapp konfigurerade toouse Application Insights](../application-insights/app-insights-nodejs.md).

* En Linux-baserade Spark i HDInsight-kluster av version 3.5 har använt tooanalyze hello data.

## <a name="architecture-and-planning"></a>Arkitektur och planering

hello följande diagram visar hello service-arkitektur för det här exemplet:

![diagram som visar data som flödar från Application Insights tooblob lagring och sedan bearbetas av Spark i HDInsight](./media/hdinsight-spark-analyze-application-insight-logs/appinsightshdinsight.png)

### <a name="azure-storage"></a>Azure Storage

Application Insights kan vara konfigurerade toocontinuously export telemetri information tooblobs. HDInsight kan sedan läsa data som lagras i hello blobbar. Det finns dock vissa krav som måste följas:

* **Plats**: om hello Storage-konto och HDInsight finns på olika platser, kan öka svarstid. Kostnad, ökar även som utgång avgifterna är tillämpade toodata flytta mellan regioner.

    > [!WARNING]
    > Med hjälp av ett Lagringskonto i en annan plats än HDInsight stöds inte.

* **BLOB-typen**: HDInsight stöder endast blockblobar. Application Insights standard toousing blockblobbar, så ska fungera som standard med HDInsight.

Information om att lägga till ytterligare lagringsutrymme tooan befintliga HDInsight-kluster finns i hello [lägga till ytterligare lagringskonton](hdinsight-hadoop-add-storage.md) dokumentet.

### <a name="data-schema"></a>Dataschemat

Application Insights ger [exportera datamodellen](../application-insights/app-insights-export-data-model.md) information för hello telemetri dataformat exporteras tooblobs. hello stegen i det här dokumentet använder Spark SQL toowork med hello data. Spark SQL kan automatiskt skapa ett schema för hello JSON datastruktur som loggats av Application Insights.

## <a name="export-telemetry-data"></a>Exportera telemetridata

Gör så hello i [konfigurera löpande Export](../application-insights/app-insights-export-telemetry.md) tooconfigure din Application Insights tooexport telemetri information tooan Azure storage blob.

## <a name="configure-hdinsight-tooaccess-hello-data"></a>Konfigurera HDInsight tooaccess hello data

Om du skapar ett HDInsight-kluster, kan du lägga till hello storage-konto när klustret skapas.

tooadd hello Azure Storage-konto tooan befintligt kluster, använda hello information i hello [lägga till ytterligare Lagringskonton](hdinsight-hadoop-add-storage.md) dokumentet.

## <a name="analyze-hello-data-pyspark"></a>Analysera hello: PySpark

1. Från hello [Azure-portalen](https://portal.azure.com), Välj ditt Spark på HDInsight-kluster. Från hello **snabblänkar** väljer **Klusterinstrumentpaneler**, och välj sedan **Jupyter-anteckningsbok** från hello klustret Dashboard__ bladet.

    ![Hej klusterinstrumentpaneler](./media/hdinsight-spark-analyze-application-insight-logs/clusterdashboards.png)

2. I hello övre högra hörnet av hello Jupyter-sidan, Välj **ny**, och sedan **PySpark**. En ny webbläsarflik som innehåller en Python-baserade Jupyter-anteckningsbok öppnas.

3. I första hello-fältet (kallas en **cell**) hello på sidan Ange hello följande text:

   ```python
   sc._jsc.hadoopConfiguration().set('mapreduce.input.fileinputformat.input.dir.recursive', 'true')
   ```

    Den här koden konfigurerar Spark toorecursively åtkomst hello efter katalogstrukturen hello indata. Application Insights telemetry är loggade tooa directory struktur liknande toohello `/{telemetry type}/YYYY-MM-DD/{##}/`.

4. Använd **SKIFT + RETUR** toorun hello kod. På vänster sida av hello cell hello en '\*' visas mellan hello hakparenteser tooindicate hello koden i den här cellen utförs. När den är klar, hello '\*' ändras tooa antal och liknande toohello följande text som visas under hello cellen utdata:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    pyspark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. En ny cell skapas under hello först en. Ange hello följande text i hello nya cellen. Ersätt `CONTAINER` och `STORAGEACCOUNT` med hello Azure storage-kontonamn och blob behållare som innehåller Application Insights-data.

   ```python
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Använd **SKIFT + RETUR** tooexecute cellen. Du ser ett resultat liknande toohello följande text:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Hej wasb sökväg som returneras beror hello var hello Application Insights telemetridata. Ändra hello `hdfs dfs -ls` rad i hello cell toouse hello wasb sökväg som returneras och sedan använda **SKIFT + RETUR** toorun hello cell igen. Den här gången ska hello resultaten visa hello kataloger som innehåller telemetridata.

   > [!NOTE]
   > För hello resten av hello stegen i det här avsnittet, hello `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` directory användes. Katalogstrukturen kan vara olika.

6. Ange hello följande kod i hello nästa cell: ersätta `WASB_PATH` med hello sökvägen hello föregående steg.

   ```python
   jsonFiles = sc.textFile('WASB_PATH')
   jsonData = sqlContext.read.json(jsonFiles)
   ```

    Den här koden skapar en dataframe från hello JSON-filer som exporterats av hello löpande export process. Använd **SKIFT + RETUR** toorun cellen.
7. Ange hello nästa cell, och kör hello följande tooview hello schemat Spark skapade för hello JSON-filer:

   ```python
   jsonData.printSchema()
   ```

    hello schema för varje typ av telemetri är olika. hello följande exempel är hello schema som genereras för webbegäranden (data som lagras i hello `Requests` underkatalog):

        root
        |-- context: struct (nullable = true)
        |    |-- application: struct (nullable = true)
        |    |    |-- version: string (nullable = true)
        |    |-- custom: struct (nullable = true)
        |    |    |-- dimensions: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |    |-- metrics: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- eventTime: string (nullable = true)
        |    |    |-- isSynthetic: boolean (nullable = true)
        |    |    |-- samplingRate: double (nullable = true)
        |    |    |-- syntheticSource: string (nullable = true)
        |    |-- device: struct (nullable = true)
        |    |    |-- browser: string (nullable = true)
        |    |    |-- browserVersion: string (nullable = true)
        |    |    |-- deviceModel: string (nullable = true)
        |    |    |-- deviceName: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- osVersion: string (nullable = true)
        |    |    |-- type: string (nullable = true)
        |    |-- location: struct (nullable = true)
        |    |    |-- city: string (nullable = true)
        |    |    |-- clientip: string (nullable = true)
        |    |    |-- continent: string (nullable = true)
        |    |    |-- country: string (nullable = true)
        |    |    |-- province: string (nullable = true)
        |    |-- operation: struct (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |-- session: struct (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- isFirst: boolean (nullable = true)
        |    |-- user: struct (nullable = true)
        |    |    |-- anonId: string (nullable = true)
        |    |    |-- isAuthenticated: boolean (nullable = true)
        |-- internal: struct (nullable = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- documentVersion: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |-- request: array (nullable = true)
        |    |-- element: struct (containsNull = true)
        |    |    |-- count: long (nullable = true)
        |    |    |-- durationMetric: struct (nullable = true)
        |    |    |    |-- count: double (nullable = true)
        |    |    |    |-- max: double (nullable = true)
        |    |    |    |-- min: double (nullable = true)
        |    |    |    |-- sampledValue: double (nullable = true)
        |    |    |    |-- stdDev: double (nullable = true)
        |    |    |    |-- value: double (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |    |-- responseCode: long (nullable = true)
        |    |    |-- success: boolean (nullable = true)
        |    |    |-- url: string (nullable = true)
        |    |    |-- urlData: struct (nullable = true)
        |    |    |    |-- base: string (nullable = true)
        |    |    |    |-- hashTag: string (nullable = true)
        |    |    |    |-- host: string (nullable = true)
        |    |    |    |-- protocol: string (nullable = true)
8. Använd hello följande tooregister hello dataframe som en temporär tabell och köra en fråga mot hello data:

   ```python
   jsonData.registerTempTable("requests")
   df = sqlContext.sql("select context.location.city from requests where context.location.city is not null")
   df.show()
   ```

    Den här frågan returnerar information om hello ort för hello översta 20 poster där context.location.city inte är null.

   > [!NOTE]
   > hello kontexten struktur finns all telemetri som loggats av Application Insights. hello stad element kan inte fyllas i loggarna. Använd hello schemat tooidentify andra element som du kan fråga som kan innehålla data för loggarna.

    Den här frågan returnerar information liknande toohello följande text:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="analyze-hello-data-scala"></a>Analysera hello: Scala

1. Från hello [Azure-portalen](https://portal.azure.com), Välj ditt Spark på HDInsight-kluster. Från hello **snabblänkar** väljer **Klusterinstrumentpaneler**, och välj sedan **Jupyter-anteckningsbok** från hello klustret Dashboard__ bladet.

    ![Hej klusterinstrumentpaneler](./media/hdinsight-spark-analyze-application-insight-logs/clusterdashboards.png)
2. I hello övre högra hörnet av hello Jupyter-sidan, Välj **ny**, och sedan **Scala**. En ny webbläsarflik som innehåller en Scala-baserade Jupyter-anteckningsbok visas.
3. I första hello-fältet (kallas en **cell**) hello på sidan Ange hello följande text:

   ```scala
   sc.hadoopConfiguration.set("mapreduce.input.fileinputformat.input.dir.recursive", "true")
   ```

    Den här koden konfigurerar Spark toorecursively åtkomst hello efter katalogstrukturen hello indata. Application Insights telemetri är inloggad tooa katalogstruktur liknande för`/{telemetry type}/YYYY-MM-DD/{##}/`.

4. Använd **SKIFT + RETUR** toorun hello kod. På vänster sida av hello cell hello en '\*' visas mellan hello hakparenteser tooindicate hello koden i den här cellen utförs. När den är klar, hello '\*' ändras tooa antal och liknande toohello följande text som visas under hello cellen utdata:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    spark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. En ny cell skapas under hello först en. Ange hello följande text i hello nya cellen. Ersätt `CONTAINER` och `STORAGEACCOUNT` hello Azure storage-kontonamn och blob behållare som innehåller Programinsikter för inloggning.

   ```scala
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Använd **SKIFT + RETUR** tooexecute cellen. Du ser ett resultat liknande toohello följande text:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Hej wasb sökväg som returneras beror hello var hello Application Insights telemetridata. Ändra hello `hdfs dfs -ls` rad i hello cell toouse hello wasb sökväg som returneras och sedan använda **SKIFT + RETUR** toorun hello cell igen. Den här gången ska hello resultaten visa hello kataloger som innehåller telemetridata.

   > [!NOTE]
   > För hello resten av hello stegen i det här avsnittet, hello `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` directory användes. Den här katalogen finns inte om inte telemetridata är för en webbapp.

6. Ange hello följande kod i hello nästa cell: ersätta `WASB\_PATH` med hello sökvägen hello föregående steg.

   ```scala
   var jsonFiles = sc.textFile('WASB_PATH')
   val sqlContext = new org.apache.spark.sql.SQLContext(sc)
   var jsonData = sqlContext.read.json(jsonFiles)
   ```

    Den här koden skapar en dataframe från hello JSON-filer som exporterats av hello löpande export process. Använd **SKIFT + RETUR** toorun cellen.

7. Ange hello nästa cell, och kör hello följande tooview hello schemat Spark skapade för hello JSON-filer:

   ```scala
   jsonData.printSchema
   ```

    hello schema för varje typ av telemetri är olika. hello följande exempel är hello schema som genereras för webbegäranden (data som lagras i hello `Requests` underkatalog):

        root
        |-- context: struct (nullable = true)
        |    |-- application: struct (nullable = true)
        |    |    |-- version: string (nullable = true)
        |    |-- custom: struct (nullable = true)
        |    |    |-- dimensions: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |    |-- metrics: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- eventTime: string (nullable = true)
        |    |    |-- isSynthetic: boolean (nullable = true)
        |    |    |-- samplingRate: double (nullable = true)
        |    |    |-- syntheticSource: string (nullable = true)
        |    |-- device: struct (nullable = true)
        |    |    |-- browser: string (nullable = true)
        |    |    |-- browserVersion: string (nullable = true)
        |    |    |-- deviceModel: string (nullable = true)
        |    |    |-- deviceName: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- osVersion: string (nullable = true)
        |    |    |-- type: string (nullable = true)
        |    |-- location: struct (nullable = true)
        |    |    |-- city: string (nullable = true)
        |    |    |-- clientip: string (nullable = true)
        |    |    |-- continent: string (nullable = true)
        |    |    |-- country: string (nullable = true)
        |    |    |-- province: string (nullable = true)
        |    |-- operation: struct (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |-- session: struct (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- isFirst: boolean (nullable = true)
        |    |-- user: struct (nullable = true)
        |    |    |-- anonId: string (nullable = true)
        |    |    |-- isAuthenticated: boolean (nullable = true)
        |-- internal: struct (nullable = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- documentVersion: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |-- request: array (nullable = true)
        |    |-- element: struct (containsNull = true)
        |    |    |-- count: long (nullable = true)
        |    |    |-- durationMetric: struct (nullable = true)
        |    |    |    |-- count: double (nullable = true)
        |    |    |    |-- max: double (nullable = true)
        |    |    |    |-- min: double (nullable = true)
        |    |    |    |-- sampledValue: double (nullable = true)
        |    |    |    |-- stdDev: double (nullable = true)
        |    |    |    |-- value: double (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |    |-- responseCode: long (nullable = true)
        |    |    |-- success: boolean (nullable = true)
        |    |    |-- url: string (nullable = true)
        |    |    |-- urlData: struct (nullable = true)
        |    |    |    |-- base: string (nullable = true)
        |    |    |    |-- hashTag: string (nullable = true)
        |    |    |    |-- host: string (nullable = true)
        |    |    |    |-- protocol: string (nullable = true)

8. Använd hello följande tooregister hello dataframe som en temporär tabell och köra en fråga mot hello data:

   ```scala
   jsonData.registerTempTable("requests")
   var city = sqlContext.sql("select context.location.city from requests where context.location.city is not null limit 10").show()
   ```

    Den här frågan returnerar information om hello ort för hello översta 20 poster där context.location.city inte är null.

   > [!NOTE]
   > hello kontexten struktur finns all telemetri som loggats av Application Insights. hello stad element kan inte fyllas i loggarna. Använd hello schemat tooidentify andra element som du kan fråga som kan innehålla data för loggarna.
   >
   >

    Den här frågan returnerar information liknande toohello följande text:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="next-steps"></a>Nästa steg

Fler exempel på Spark toowork med data och tjänster i Azure finns i hello följande dokument:

* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](hdinsight-apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning](hdinsight-apache-spark-eventhub-streaming.md)
* [Webbplatslogganalys med Spark i HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

Information om hur du skapar och kör Spark-program finns i hello följande dokument:

* [Skapa ett fristående program med hjälp av Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)
