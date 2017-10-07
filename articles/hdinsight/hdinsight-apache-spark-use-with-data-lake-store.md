---
title: aaaUse Apache Spark tooanalyze data i Azure Data Lake Store | Microsoft Docs
description: "Köra Spark jobb tooanalyze data som lagras i Azure Data Lake Store"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1f174323-c17b-428c-903d-04f0e272784c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 3b7f628f7a8114d2ca6f3f9219ce107905f1c818
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hdinsight-spark-cluster-tooanalyze-data-in-data-lake-store"></a>Använda HDInsight Spark-kluster tooanalyze data i Data Lake Store

I den här kursen använder du Jupyter-anteckningsbok tillgänglig med HDInsight Spark-kluster toorun ett jobb som läser data från ett Data Lake Store-konto.

## <a name="prerequisites"></a>Krav

* Azure Data Lake Store-konto. Följ instruktionerna för hello på [Kom igång med Azure Data Lake Store med hjälp av hello Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md).

* Azure HDInsight Spark-kluster med Data Lake Store som lagring. Följ instruktionerna för hello på [skapar ett HDInsight-kluster med Data Lake Store med hjälp av Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

    
## <a name="prepare-hello-data"></a>Förbereda hello data

> [!NOTE]
> Du behöver inte tooperform det här steget om du har skapat hello HDInsight-kluster med Data Lake Store som standardlagring. hello klustret gruppskapande processer lägger till exempeldata i hello Data Lake Store-konto som du anger när du skapar hello klustret. Hoppa över toohello avsnittet [använda HDInsight Spark-kluster med Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).
>
>

Om du har skapat ett HDInsight-kluster med Data Lake Store som ytterligare lagringsutrymme och Azure Storage Blob som standardlagring, bör du först kopiera över vissa exempel data toohello Data Lake Store-konto. Du kan använda hello exempeldata från hello Azure Storage Blob som är associerade med hello HDInsight-kluster. Du kan använda hello [ADLCopy verktyget](http://aka.ms/downloadadlcopy) toodo så. Hämta och installera hello verktyget från hello länk.

1. Öppna en kommandotolk och navigera toohello directory där AdlCopy installeras normalt `%HOMEPATH%\Documents\adlcopy`.

2. Kör följande kommando toocopy hello en specifik blobb från hello källa behållaren tooa Data Lake Store:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Kopiera hello **HVAC.csv** exempeldata filen på **/HdiSamples/HdiSamples/SensorSampleData/hvac/** toohello Azure Data Lake Store-konto. hello kodstycke bör se ut som:

        AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

   > [!WARNING]
   > Kontrollera att du hello fil och sökvägar i hello rätt skiftläge.
   >
   >
3. Du kan ange tooenter hello autentiseringsuppgifter för hello Azure-prenumeration som du har ditt Data Lake Store-konto. En liknande toohello följande i utdata visas:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Copy Completed. 1 file copied.

    hello-datafil (**HVAC.csv**) kopieras under en mapp **/hvac** i hello Data Lake Store-konto.

## <a name="use-an-hdinsight-spark-cluster-with-data-lake-store"></a>Använda ett HDInsight Spark-kluster med Data Lake Store

1. Från hello [Azure Portal](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan). Du kan också navigera tooyour kluster under **Bläddra bland alla** > **HDInsight-kluster**.

2. Hello Spark-klusterbladet, klicka på **snabblänkar**, och sedan hello **Klusterinstrumentpanel** bladet, klickar du på **Jupyter-anteckningsbok**. Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.

   > [!NOTE]
   > Du kan också nå hello Jupyter Notebook för ditt kluster genom att öppna hello följande URL i webbläsaren. Ersätt **KLUSTERNAMN** med hello namnet på klustret:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. Skapa en ny anteckningsbok. Klicka på **Ny** och sedan på **PySpark**.

    ![Skapa en ny Jupyter-anteckningsbok](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Skapa en ny Jupyter-anteckningsbok")

4. Eftersom du har skapat anteckningsboken med hello PySpark-kerneln, behöver du inte toocreate några kontexter explicit. hello Spark och Hive-kontexterna skapas automatiskt för dig när du kör hello första kodcellen. Du kan starta genom att importera hello-typer som krävs för det här scenariot. toodo så klistra in hello följande kodfragment i en cell och tryck på **SKIFT + RETUR**.

        from pyspark.sql.types import *

    Varje gång du kör ett jobb i Jupyter fönsternamn din web webbläsaren visar en **(upptagen)** status tillsammans med hello anteckningsbokens titel. Du kan även se en fylld cirkel nästa toohello **PySpark** text i hello övre högra hörnet. När hello jobbet har slutförts ändras tooa tom cirkel.

     ![Status för ett Jupyter-anteckningsboksjobb](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Status för ett Jupyter-anteckningsboksjobb")

5. Läs in exempeldata i en tillfällig tabell med hello **HVAC.csv** filen som du kopierade toohello Data Lake Store-konto. Du kan komma åt hello data i hello Data Lake Store-konto med hjälp av hello följande URL-mönster.

    * Om du har Data Lake Store som standardlagring blir HVAC.csv på hello sökväg liknande toohello följande URL:

            adl://<data_lake_store_name>.azuredatalakestore.net/<cluster_root>/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

        Eller, du kan också använda ett kortare format, till exempel hello följande:

            adl:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

    * Om du har Data Lake Store som ytterligare lagringsutrymme, kommer att vara hello platsen där du har kopierat, t.ex HVAC.csv:

            adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

     I en tom cell klistra in hello följande kodexempel, ersätter **MYDATALAKESTORE** med Data Lake Store-kontonamnet och tryck på **SKIFT + RETUR**. Den här kodexemplet registrerar hello data till en tillfällig tabell som kallas **hvac**.

            # Load hello data. hello path below assumes Data Lake Store is default storage for hello Spark cluster
            hvacText = sc.textFile("adl://MYDATALAKESTORE.azuredatalakestore.net/cluster/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            # Create hello schema
            hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

            # Parse hello data in hvacText
            hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

            # Create a data frame
            hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

            # Register hello data fram as a table toorun queries against
            hvacdf.registerTempTable("hvac")

6. Eftersom du använder en PySpark-kerneln, du kan nu direkt köra en SQL-fråga på hello tillfällig tabell **hvac** som du nyss skapade med hjälp av hello `%%sql` Magiskt tal. Mer information om hello `%%sql` magic, samt andra användbara med hello PySpark-kerneln, finns [kernlar som är tillgängliga i Jupyter-anteckningsböcker med HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

7. När hello jobbet har slutförts, visas hello följande tabular utdata som standard.

      ![Tabellutdata från frågeresultat](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Tabellutdata från frågeresultat")

     Du kan också se hello resulterar i andra visualiseringar. Till exempel detta ett Områdesdiagram för samma utdata skulle se ut hello följande hello.

     ![Områdesdiagram över frågeresultat](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-area-output.png "Områdesdiagram över frågeresultat")

8. När du har kört programmet hello ska avstängning hello anteckningsboken toorelease hello resurser. toodo så från hello **filen** Klicka på menyn på hello anteckningsboken **Stäng och stoppa**. Den här stängs och Stäng hello bärbar dator.


## <a name="next-steps"></a>Nästa steg

* [Skapa en fristående Scala programmet toorun på Apache Spark-kluster](hdinsight-apache-spark-create-standalone-application.md)
* [Använda HDInsight Tools i Azure Toolkit för IntelliJ toocreate Spark-program för HDInsight Spark Linux-kluster](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Använda HDInsight Tools i Azure Toolkit för Eclipse toocreate Spark-program för HDInsight Spark Linux-kluster](hdinsight-apache-spark-eclipse-tool-plugin.md)
