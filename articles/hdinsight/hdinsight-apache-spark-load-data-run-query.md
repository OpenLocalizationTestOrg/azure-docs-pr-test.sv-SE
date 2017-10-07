---
title: "aaaRun interaktiva frågor i ett Azure HDInsight Spark-kluster | Microsoft Docs"
description: "HDInsight Spark quickstart på hur toocreate ett Apache Spark-kluster i HDInsight."
keywords: "spark-snabbstart,interaktiv spark,interaktiv fråga,hdinsight spark,azure spark"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 3864eba50eb3828a9ecb657ded88080e1974585f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-interactive-queries-on-an-hdinsight-spark-cluster"></a>Köra interaktiva frågor på ett HDInsight Spark-kluster

I den här artikeln använder du en Jupyter-anteckningsbok toorun interaktiva Spark SQL-frågor mot Spark-kluster. Jupyter-anteckningsbok är ett webbaserat program som utökar hello konsolbaserad interaktiva upplevelse toohello Web. Mer information finns i [hello Jupyter-anteckningsbok](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).

Den här kursen kan du använda hello **PySpark** kernel i hello Jupyter-anteckningsbok toorun en interaktiva Spark SQL-fråga. Jupyter-anteckningsböcker på HDInsight-kluster stöder också två andra kernel - **PySpark3** och **Spark**. Mer information om hello kärnor och hello fördelarna med att använda **PySpark**, se [Använd Jupyter-anteckningsbok kärnor med Apache Spark-kluster i HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Krav

* **Ett Azure HDInsight Spark-kluster**. Instruktioner finns i [skapar ett Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-toorun-interactive-queries"></a>Skapa en Jupyter-anteckningsbok toorun interaktiva frågor

toorun frågor vi använder exempeldata är som standard tillgängligt i hello lagring som associerats med hello kluster. Du måste dock först hämta dessa data till Spark som en dataframe. När du har hello dataframe kan köra du frågor i det med hello Jupyter-anteckningsbok. I det här avsnittet titta på hur du:

* Registrera data exempeldata som ett Spark-dataframe.
* Köra frågor på hello dataframe.

1. Öppna hello [Azure-portalen](https://portal.azure.com/). Om du har valt toopin hello klusterinstrumentpanel toohello Klicka hello klustret panelen från hello instrumentpanelen toolaunch hello klusterbladet.

    Om du inte fäster hello klustret toohello instrumentpanelen från hello till vänster och klicka på **HDInsight-kluster**, och klicka sedan på hello-klustret som du skapade.

3. I **Snabblänkar** klickar du på **Klusterinstrumentpaneler** och sedan på **Jupyter Notebook**. Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.

   ![Öppna Jupyter-anteckningsbok toorun interaktiva Spark SQL-frågan](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "öppna Jupyter-anteckningsbok toorun interaktiva Spark SQL-fråga")

   > [!NOTE]
   > Du kan också komma åt hello Jupyter notebook för ditt kluster genom att öppna hello följande URL i webbläsaren. Ersätt **KLUSTERNAMN** med hello namnet på klustret:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Skapa en anteckningsbok. Klicka på **Ny** och sedan på **PySpark**.

   ![Skapa en Jupyter-anteckningsbok toorun interaktiva Spark SQL-fråga](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "skapa en Jupyter-anteckningsbok toorun interaktiva Spark SQL-fråga")

   En ny anteckningsbok skapas och öppnas med hello namnet Untitled(Untitled.pynb).

4. Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn om du vill.

    ![Ange ett namn för hello Jupter anteckningsboken toorun interaktiva Spark frågan från](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "ange ett namn för hello Jupter anteckningsboken toorun interaktiva Spark frågan från")

5. Klistra in hello följande kod i en tom cell och tryck sedan på **SKIFT + RETUR** toorun hello kod. hello kod importerar hello-typer som krävs för det här scenariot:

        from pyspark.sql.types import *

    Eftersom du har skapat anteckningsboken med hello PySpark-kerneln, behöver du inte toocreate några kontexter explicit. hello Spark och Hive-kontexterna skapas automatiskt för dig när du kör hello första kodcellen.

    ![Status för interaktiv Spark SQL-fråga](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status för interaktiv Spark SQL-fråga")

    Varje gång du kör en interaktiv fråga i Jupyter fönsternamn din web webbläsaren visar en **(upptagen)** status tillsammans med hello anteckningsbokens titel. Du också se en fylld cirkel nästa toohello **PySpark** text i hello övre högra hörnet. När hello jobbet har slutförts ändras tooa tom cirkel.

6. Låt oss se ut en ögonblicksbild av den innan du läser in hello data i ett Spark-kluster. hello exempeldata som används i den här kursen är tillgänglig som en CSV-fil på alla HDInsight Spark-kluster på **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**. hello data avbildar hello temperaturvariationer av en byggnad. Här följer hello första raderna i hello data.

    ![Ögonblicksbild av data för interaktiva Spark SQL-frågan](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "ögonblicksbild av data för interaktiva Spark SQL-fråga")

6. Skapa en dataframe och en tillfällig tabell (**hvac**) genom att köra hello följande kod. För den här självstudiekursen kommer skapa vi inte alla hello kolumner i hello tillfällig tabell som jämfört med toohello kolumner i CSV-hello rådata. 

        # Create an RDD from sample data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create a schema for our data
        Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')

        # Parse hello data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
        hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))
        
        # Infer hello schema and create a table       
        hvacTable = sqlContext.createDataFrame(hvac)
        hvacTable.registerTempTable('hvactemptable')
        dfw = DataFrameWriter(hvacTable)
        dfw.saveAsTable('hvac')

7. När hello tabellen har skapats kan köra interaktiva fråga på hello data, kan du använda hello följande kod.

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   Eftersom du använder en PySpark-kerneln, du kan nu direkt köra en interaktiv SQL-fråga på hello tillfällig tabell **hvac** som du skapade med hjälp av hello `%%sql` Magiskt tal. Mer information om hello `%%sql` magic och andra användbara med hello PySpark-kerneln, finns [kernlar som är tillgängliga i Jupyter-anteckningsböcker med HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

   hello följande tabular utdata visas som standard.

     ![Tabellutdata från interaktivt Spark-frågeresultat](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Tabellutdata från interaktivt Spark-frågeresultat")

    Du kan också se hello resulterar i andra visualiseringar. Till exempel detta ett Områdesdiagram för samma utdata skulle se ut hello följande hello.

    ![Områdesdiagram över interaktivt Spark-frågeresultat](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Områdesdiagram över interaktivt Spark-frågeresultat")

9. Stäng hello anteckningsboken toorelease hello klusterresurser när du har kört programmet hello. toodo så från hello **filen** Klicka på menyn på hello anteckningsboken **Stäng och stoppa**.

## <a name="next-step"></a>Nästa steg

I denna artikel du lärt dig hur toorun interaktiva frågor i Spark med Jupyter-anteckningsbok. Avancera toohello nästa artikel toosee hur hello data som du har registrerat i Spark hämtas till en BI analytics verktyg som till exempel Power BI och Tableau. 

> [!div class="nextstepaction"]
>[Spark BI med hjälp av verktyg för visualisering av data med Azure HDInsight](hdinsight-apache-spark-use-bi-tools.md)




