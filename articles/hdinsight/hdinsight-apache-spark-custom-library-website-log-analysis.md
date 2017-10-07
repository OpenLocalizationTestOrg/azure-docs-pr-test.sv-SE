---
title: aaaAnalyze webbplatsloggar med Python-bibliotek i Spark - Azure | Microsoft Docs
description: "Den här anteckningsboken visar hur tooanalyze logga data med hjälp av en anpassad bibliotek med Spark på Azure HDInsight."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8c61c70f-fe7f-4f0f-a4ab-0cccee5668c9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 29e4308b2a359aee6d69494a98307d4da07f7909
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-website-logs-using-a-custom-python-library-with-spark-cluster-on-hdinsight"></a>Analysera webbplatsloggar med hjälp av en anpassad Python-bibliotek med Spark-kluster i HDInsight

Den här anteckningsboken visar hur tooanalyze logga data med hjälp av en anpassad bibliotek med Spark i HDInsight. hello anpassade bibliotek som vi använder är en Python-bibliotek kallas **iislogparser.py**.

> [!TIP]
> Den här kursen är också tillgängliga som en Jupyter-anteckningsbok på ett kluster med Spark (Linux) som du skapar i HDInsight. hello anteckningsboken upplevelse kan du köra hello Python kodavsnitt från hello anteckningsbok sig själv. tooperform hello vägledningen i en bärbar dator, skapa ett Spark-kluster, starta en Jupyter-anteckningsbok (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), och kör sedan hello anteckningsboken **analysera loggar med Spark med hjälp av en anpassad library.ipynb** under hello  **PySpark** mapp.
>
>

**Krav:**

Du måste ha hello följande:

* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Spara rådata som en RDD
I det här avsnittet ska vi använda hello [Jupyter](https://jupyter.org) bärbar dator som är associerad med ett Apache Spark-kluster i HDInsight toorun jobb som bearbetar dina rådata exempeldata och spara den som en Hive-tabell. hello exempeldata är en CSV-fil (hvac.csv) tillgängliga på alla kluster som standard.

När du har sparat dina data som en Hive-tabell i nästa avsnitt om hello ansluter vi toohello Hive-tabell med BI-verktyg som Power BI och Tableau.

1. Från hello [Azure-portalen](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan). Du kan också navigera tooyour kluster under **Bläddra bland alla** > **HDInsight-kluster**.   
2. Hello Spark-klusterbladet, klicka på **Klusterinstrumentpanel**, och klicka sedan på **Jupyter-anteckningsbok**. Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.

   > [!NOTE]
   > Du kan också nå hello Jupyter Notebook för ditt kluster genom att öppna hello följande URL i webbläsaren. Ersätt **KLUSTERNAMN** med hello namnet på klustret:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Skapa en ny anteckningsbok. Klicka på **Ny** och sedan på **PySpark**.

    ![Skapa en ny Jupyter-anteckningsbok](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-create-jupyter-notebook.png "Skapa en ny Jupyter-anteckningsbok")
4. En ny anteckningsbok skapas och öppnas med hello namnet Untitled.pynb. Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn.

    ![Ange ett namn för anteckningsboken hello](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-name-jupyter-notebook.png "ange ett namn för anteckningsboken hello")
5. Eftersom du har skapat anteckningsboken med hello PySpark-kerneln, behöver du inte toocreate några kontexter explicit. hello Spark och Hive-kontexterna skapas automatiskt för dig när du kör hello första kodcellen. Du kan starta genom att importera hello-typer som krävs för det här scenariot. Klistra in hello följande fragment i en tom cell och tryck sedan på **SKIFT + RETUR**.

        from pyspark.sql import Row
        from pyspark.sql.types import *


1. Skapa en RDD via hello loggen exempeldata redan finns på hello kluster. Du kan komma åt hello data i hello standardkontot för lagring som associerats med hello kluster på **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.

        logs = sc.textFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


1. Hämta en exempellogg ange tooverify hello tidigare steget har slutförts.

        logs.take(5)

    Du bör se en utdata liknande toohello följande:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Analysera loggdata som använder en anpassad Python-bibliotek
1. Hello första par raderna innehåller hello rubrikinformation i hello utdata ovan, och varje återstående rad matchar hello schemat som beskrivs i den rubriken. Parsning av dessa loggar kan vara komplicerat. Så vi använder en anpassad Python-bibliotek (**iislogparser.py**) som gör att parsning av dessa loggar som är mycket enklare. Det här biblioteket är som standard ingår i Spark-kluster i HDInsight på **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.

    Det här biblioteket är dock inte i hello `PYTHONPATH` så att vi inte kan använda den med hjälp av en import-sats som `import iislogparser`. toouse det här biblioteket vi måste distribuera den tooall hello arbetsnoderna. Kör hello följande kodavsnitt.

        sc.addPyFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


1. `iislogparser`innehåller en funktion `parse_log_line` som returnerar `None` om en logg är en rubrikrad, och returnerar en instans av hello `LogLine` klassen om det uppstår en rad i loggfilen. Använd hello `LogLine` klassen tooextract endast hello loggen rader från hello RDD:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()
2. Hämta ett par extraherade loggen rader tooverify som hello steg har slutförts.

       logLines.take(2)

   hello utdata ska vara liknande toohello följande:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]
3. Hej `LogLine` klass, i sin tur har vissa användbara metoder som `is_error()`, som returnerar om en loggpost har en felkod. Använd den här toocompute hello antalet fel på hello extraherade loggen rader och logga sedan alla hello fel tooa annan fil.

       errors = logLines.filter(lambda p: p.is_error())
       numLines = logLines.count()
       numErrors = errors.count()
       print 'There are', numErrors, 'errors and', numLines, 'log entries'
       errors.map(lambda p: str(p)).saveAsTextFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

   Du bör se utdata som liknar hello följande:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       There are 30 errors and 646 log entries
4. Du kan också använda **Matplotlib** tooconstruct en visualisering av hello data. Om du vill tooisolate hello orsaken till begäranden som körs under lång tid kan kanske du vill toofind hello filer som tar hello de flesta tid tooserve i genomsnitt.
   hello kodfragmentet nedan hämtar hello översta 25 resurser som tog tooserve för de flesta tid en begäran.

       def avgTimeTakenByKey(rdd):
           return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                   lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                   lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                     .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))

       avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

   Du bör se utdata som liknar hello följande:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [(u'/blogposts/mvc4/step13.png', 197.5),
        (u'/blogposts/mvc2/step10.jpg', 179.5),
        (u'/blogposts/extractusercontrol/step5.png', 170.0),
        (u'/blogposts/mvc4/step8.png', 159.0),
        (u'/blogposts/mvcrouting/step22.jpg', 155.0),
        (u'/blogposts/mvcrouting/step3.jpg', 152.0),
        (u'/blogposts/linqsproc1/step16.jpg', 138.75),
        (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
        (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
        (u'/blogposts/nested/step2.jpg', 126.0),
        (u'/blogposts/adminpack/step1.png', 124.0),
        (u'/BlogPosts/datalistpaging/step2.png', 118.0),
        (u'/blogposts/mvc4/step35.png', 117.0),
        (u'/blogposts/mvcrouting/step2.jpg', 116.5),
        (u'/blogposts/aboutme/basketball.jpg', 109.0),
        (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
        (u'/blogposts/mvc4/step12.png', 106.0),
        (u'/blogposts/linq8/step0.jpg', 105.5),
        (u'/blogposts/mvc2/step18.jpg', 104.0),
        (u'/blogposts/mvc2/step11.jpg', 104.0),
        (u'/blogposts/mvcrouting/step1.jpg', 104.0),
        (u'/blogposts/extractusercontrol/step1.png', 103.0),
        (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
        (u'/blogposts/mvcrouting/step21.jpg', 101.0),
        (u'/blogposts/mvc4/step1.png', 98.0)]
5. Du kan också visa denna information i form av hello på ritytan. Som ett första steg toocreate ritning, låt oss först skapa en tillfällig tabell **AverageTime**. hello tabell grupper hello loggar som tid toosee om det inte finns några ovanliga svarstidsspikar vid en viss tidpunkt.

       avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
       schema = StructType([StructField('Minutes', IntegerType(), True),
                            StructField('Time', FloatType(), True)])

       avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
       avgTimeTakenByMinuteDF.registerTempTable('AverageTime')
6. Sedan kan du köra följande SQL-frågan tooget hello alla hello poster i hello **AverageTime** tabell.

       %%sql -o averagetime
       SELECT * FROM AverageTime

   Hej `%%sql` magic följt av `-o averagetime` säkerställer att hello utdata från frågan hello sparas lokalt på hello Jupyter-servern (vanligtvis hello headnode hello klustret). hello utdata sparas som en [Pandas](http://pandas.pydata.org/) dataframe med hello angett namn **averagetime**.

   Du bör se utdata som liknar hello följande:

   ![SQL-frågan](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-jupyter-sql-qyery-output.png "SQL frågeresultatet")

   Mer information om hello `%%sql` magiskt, se [parametrar stöds med hello %% sql magic](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).
7. Du kan nu använda Matplotlib, ett bibliotek används tooconstruct visualisering av data, toocreate ritning. Eftersom hello ritytans måste skapas från hello lokalt sparade **averagetime** dataframe, hello kodstycke måste börja med hello `%%local` Magiskt tal. Detta säkerställer att hello kod körs lokalt på hello Jupyter-servern.

       %%local
       %matplotlib inline
       import matplotlib.pyplot as plt

       plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
       plt.xlabel('Time (min)')
       plt.ylabel('Average time taken for request (ms)')

   Du bör se utdata som liknar hello följande:

   ![Matplotlib utdata](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-apache-spark-web-log-analysis-plot.png "Matplotlib utdata")
8. När du har kört programmet hello ska avstängning hello anteckningsboken toorelease hello resurser. toodo så från hello **filen** Klicka på menyn på hello anteckningsboken **Stäng och stoppa**. Den här stängs och Stäng hello bärbar dator.

## <a name="seealso"></a>Se även
* [Översikt: Apache Spark i Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenarier
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](hdinsight-apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid](hdinsight-apache-spark-eventhub-streaming.md)

### <a name="create-and-run-applications"></a>Skapa och köra program
* [Skapa ett fristående program med hjälp av Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Verktyg och tillägg
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-program](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)
