---
title: "aaaKernels för Jupyter notebook i Spark-kluster i Azure HDInsight | Microsoft Docs"
description: "Läs mer om hello PySpark och PySpark3 Spark kärnor för Jupyter-anteckningsbok med Spark-kluster i Azure HDInsight."
keywords: "jupyter-anteckningsbok på spark jupyter spark"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 0719e503-ee6d-41ac-b37e-3d77db8b121b
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: nitinme
ms.openlocfilehash: 560c944fe850c5753ac9fa90550b804f0c47d14c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="kernels-for-jupyter-notebook-on-spark-clusters-in-azure-hdinsight"></a>Kärnor för Jupyter notebook i Spark-kluster i Azure HDInsight 

HDInsight Spark-kluster tillhandahåller kernlar som du kan använda med hello Jupyter notebook i Spark för att testa ditt program. En kernel är ett program som körs och tolkar din kod. hello tre kärnor är:

- **PySpark** - för appar som skrivits i Python2
- **PySpark3** - för appar som skrivits i Python3
- **Spark** - för program som skrivits i Scala

I den här artikeln får du lära dig hur toouse dessa kärnor och hello fördelarna med att använda dem.

## <a name="prerequisites"></a>Krav

* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-on-spark-hdinsight"></a>Skapa en Jupyter-anteckningsbok på Spark HDInsight

1. Från hello [Azure-portalen](https://portal.azure.com/), öppna ditt kluster.  Se [listan och visa](hdinsight-administer-use-portal-linux.md#list-and-show-clusters) hello-instruktioner. hello kluster öppnas i ett nytt portalblad.

2. Från hello **snabblänkar** klickar du på **kluster instrumentpaneler** tooopen hello **kluster instrumentpaneler** bladet.  Om du inte ser **snabblänkar**, klickar du på **översikt** hello vänstra menyn hello-bladet.

    ![Jupyter-anteckningsbok på Spark](./media/hdinsight-apache-spark-jupyter-notebook-kernels/hdinsight-jupyter-notebook-on-spark.png "Jupyter notebook i Spark") 

3. Klicka på **Jupyter-anteckningsbok**. Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.
   
   > [!NOTE]
   > Du kan också nå hello Jupyter notebook i Spark-kluster genom att öppna hello följande URL i webbläsaren. Ersätt **KLUSTERNAMN** med hello namnet på klustret:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 

3. Klicka på **ny**, och klicka sedan på antingen **Pyspark**, **PySpark3**, eller **Spark** toocreate en bärbar dator. Använd hello Spark kernel för Scala program, PySpark-kerneln för Python2 program och PySpark3 kernel för Python3 program.
   
    ![Kärnor för Jupyter notebook i Spark](./media/hdinsight-apache-spark-jupyter-notebook-kernels/kernel-jupyter-notebook-on-spark.png "kärnor för Jupyter notebook i Spark") 

4. En bärbar dator öppnas med hello kernel du valt.

## <a name="benefits-of-using-hello-kernels"></a>Fördelarna med att använda hello kärnor

Här är några fördelar med att använda hello nya kärnor med Jupyter notebook i Spark HDInsight-kluster.

- **Förinställningen kontexter**. Med **PySpark**, **PySpark3**, eller hello **Spark** kärnor, behöver du inte tooset hello Spark- eller Hive kontexter explicit innan du börjar arbeta med dina program. Dessa är tillgängliga som standard. Dessa kontexter är:
   
   * **SC** - Spark-kontexten
   * **sqlContext** - Hive-kontexten

    Så att du inte har toorun uttryck som hello följande tooset hello kontexter:

        SC = SparkContext('yarn-client') sqlContext = HiveContext(sc)

    I stället kan du kan använda direkt hello förinställningen kontexter i ditt program.

- **Cell användbara**. Hej PySpark-kerneln innehåller vissa fördefinierade ”användbara”, som är särskilda kommandon som kan anropas med `%%` (till exempel `%%MAGIC` <args>). hello magiskt kommandot måste vara hello första ordet i en cell i koden och tillåter flera rader med innehåll. hello magiskt word ska hello första ordet i hello cell. Lägger till något innan hello magic, även kommentarer orsakar ett fel.     Mer information om användbara finns [här](http://ipython.readthedocs.org/en/stable/interactive/magics.html).
   
    hello visar följande tabell hello olika användbara via hello kärnor.

   | Magiskt tal | Exempel | Beskrivning |
   | --- | --- | --- |
   | Hjälp |`%%help` |Genererar en tabell med alla tillgängliga hello-användbara med exempel och beskrivning |
   | Info |`%%info` |Utdata sessionsinformation för hello aktuella Livius slutpunkt |
   | Konfigurera |`%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} |Konfigurerar hello parametrar för att skapa en session. Hej flaggan force (-f) är obligatorisk om en session redan har skapats, vilket säkerställer att hello-session är släppas och återskapas. Titta på [Liviuss POST /sessions brödtext i begäran](https://github.com/cloudera/livy#request-body) en lista över giltiga parametrar. Parametrar måste överföras i som en JSON-sträng och måste vara hello nästa rad efter hello magic enligt hello exempel kolumn. |
   | SQL |`%%sql -o <variable name>`<br> `SHOW TABLES` |Kör en Hive-fråga mot hello sqlContext. Om hello `-o` -parameter har skickats, hello resultatet av hello fråga sparas i hello %% lokala Python kontext som en [Pandas](http://pandas.pydata.org/) dataframe. |
   | lokala |`%%local`<br>`a=1` |Alla hello koden i efterföljande rader körs lokalt. Koden måste vara giltig Python2 kod även oavsett hello kernel som du använder. Så även om du har valt **PySpark3** eller **Spark** kärnor när du skapar hello bärbar dator, om du använder hello `%%local` magiskt i en cell, cellen får bara ha giltig Python2 kod... |
   | loggar |`%%logs` |Utdata hello loggar för hello Livius sessionen. |
   | ta bort |`%%delete -f -s <session number>` |Tar bort en viss session hello aktuella Livius slutpunkt. Observera att du inte kan ta bort hello-session som har initierats för hello kernel sig själv. |
   | Rensa |`%%cleanup -f` |Tar bort alla hello-sessioner för hello aktuella Livius-slutpunkten, inklusive anteckningsbokens session. hello kraft flaggan -f är obligatoriskt. |

   > [!NOTE]
   > Dessutom toohello användbara lagts till av hello PySpark-kerneln, du kan också använda hello [inbyggda IPython användbara](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics), inklusive `%%sh`. Du kan använda hello `%%sh` magiska toorun skript och kodblock på hello klustret headnode.
   >
   >
2. **Automatisk visualiseringen**. Hej **Pyspark** kernel automatiskt visualizes hello utdata från Hive och SQL-frågor. Du kan välja mellan flera olika typer av visualiseringar inklusive tabell, cirkeldiagram, rad, område, fältet.

## <a name="parameters-supported-with-hello-sql-magic"></a>Parametrar som stöds med hello %% Magiskt tal för sql
Hej `%%sql` magic stöder olika parametrar som du kan använda toocontrol hello typ av utdata som visas när du kör frågor. hello i den följande tabellen listas hello utdata.

| Parameter | Exempel | Beskrivning |
| --- | --- | --- |
| -o |`-o <VARIABLE NAME>` |Använd den här parametern toopersist hello resultatet av hello-fråga i hello %% lokala Python kontext som en [Pandas](http://pandas.pydata.org/) dataframe. hello heter hello dataframe variabeln hello variabelnamn som du anger. |
| -q |`-q` |Använd den här tooturn av visualiseringar för hello cell. Om du inte vill tooauto-visualisera hello innehållet i en cell och bara vill toocapture sedan använda den som en dataframe `-q -o <VARIABLE>`. Om du vill tooturn av visualiseringar utan fånga hello resultat (till exempel för att köra en SQL-fråga som en `CREATE TABLE` instruktionen), Använd `-q` utan att ange en `-o` argumentet. |
| -m |`-m <METHOD>` |Där **metoden** är antingen **ta** eller **exempel** (standard är **ta**). Om hello-metoden är **ta**, hello kernel hämtar element från hello överkant hello resultatet som anges av MAXROWS (beskrivs senare i den här tabellen). Om hello-metoden är **exempel**, hello kernel slumpmässigt exempel element i hello datauppsättning enligt för`-r` parametern beskrivs nedan i den här tabellen. |
| -r |`-r <FRACTION>` |Här **bråk** är ett flyttal mellan 0,0 och 1,0. Om hello exempelmetoden för hello SQL-frågan är `sample`, och sedan hello kernel slumpmässigt exempel hello angivna bråkdel av hello element i hello resultatmängden du. Om du kör en SQL-fråga med hello argument exempelvis `-m sample -r 0.01`, och sedan 1% av hello resultatet rader samplas slumpmässigt. |
| -n |`-n <MAXROWS>` |**MAXROWS** är ett heltal. hello kernel begränsar hello antalet rader som utdata för**MAXROWS**. Om **MAXROWS** är ett negativt tal som **-1**, och sedan hello antalet rader i resultatmängden hello inte begränsas. |

**Exempel:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2
    SELECT * FROM hivesampletable

hello instruktionen ovan hello följande:

* Markera alla poster från **hivesampletable**.
* Eftersom vi använder - q, inaktiverar automatisk visualiseringen.
* Eftersom vi använder `-m sample -r 0.1 -n 500` den slumpmässigt exempel 10% av hello rader i hello hivesampletable och gränser hello hello rader med resultatmängd too500 storlek.
* Slutligen, eftersom vi använde `-o query2` sparas även hello utdata till en dataframe kallas **fråga2**.

## <a name="considerations-while-using-hello-new-kernels"></a>Att tänka på när du använder hello nya kärnor

Oavsett vilken kernel som du använder förbrukar lämnar hello-datorer som kör hello klusterresurser.  Med dessa kärnor eftersom hello kontexter är förinställda bara avslutas hello anteckningsböcker inte avsluta hello kontext och därför hello klusterresurser fortsätta toobe används. Är en bra idé toouse hello **Stäng och stoppa** alternativet från hello anteckningsbok **filen** menyn när du är klar med hello bärbar dator, vilket avslutar hello kontext och sedan avslutar hello bärbar dator.     

## <a name="show-me-some-examples"></a>Visa exempel

När du öppnar en Jupyter-anteckningsbok kan se du två mappar på rotnivå hello.

* Hej **PySpark** mappen innehåller exempel bärbara datorer att använda hello nya **Python** kernel.
* Hej **Scala** mappen innehåller exempel bärbara datorer att använda hello nya **Spark** kernel.

Du kan öppna hello **00 - [läsa mig första] Spark Magic Kernel funktioner** anteckningsboken från hello **PySpark** eller **Spark** mappen toolearn om hello olika användbara. Du kan också använda hello andra exempel anteckningsböcker som är tillgängliga under hello två mappar toolearn hur tooachieve olika scenarier med Jupyter-anteckningsböcker med HDInsight Spark-kluster.

## <a name="where-are-hello-notebooks-stored"></a>Var lagras hello bärbara datorer?

Jupyter-anteckningsböcker sparas toohello storage-konto som är associerade med hello kluster under hello **/HdiNotebooks** mapp.  Bärbara datorer, textfiler och mappar som du skapar från i Jupyter kan nås från hello storage-konto.  Till exempel om du använder Jupyter toocreate en mapp **MinMapp** och en bärbar dator **myfolder/mynotebook.ipynb**, du kan komma åt den bärbara på `/HdiNotebooks/myfolder/mynotebook.ipynb` inom hello storage-konto.  hello omvänd gäller även, det vill säga om du överför en bärbar dator direkt tooyour storage-konto på `/HdiNotebooks/mynotebook1.ipynb`, hello bärbara datorer som är synliga från Jupyter samt.  Bärbara datorer är kvar i hello storage-konto även efter hello kluster har tagits bort.

Hej hur anteckningsböcker sparas toohello storage-konto är kompatibelt med HDFS. Så, om du SSH till hello-kluster som du kan använda filen management kommandon som visas i följande fragment hello:

    hdfs dfs -ls /HdiNotebooks                               # List everything at hello root directory – everything in this directory is visible tooJupyter from hello home page
    hdfs dfs –copyToLocal /HdiNotebooks                    # Download hello contents of hello HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb toohello root folder so it’s visible from Jupyter


Om det finns problem med att komma åt hello storage-konto för hello kluster, hello anteckningsböcker sparas också på hello headnode `/var/lib/jupyter`.

## <a name="supported-browser"></a>Webbläsare som stöds

Jupyter-anteckningsböcker på HDInsight Spark-kluster stöds bara på Google Chrome.

## <a name="feedback"></a>Feedback
hello nya kärnor i utvecklingen av steg och kommer mogna över tid. Detta kan också innebära att API: er kan ändra som dessa kärnor mogna. Vi gärna feedback som du har när du använder dessa nya kärnor. Detta är användbart i shaping hello slutversionen av dessa kärnor. Du kan lämna kommentarer/feedback under hello **kommentarer** avsnittet längst ned hello i den här artikeln.

## <a name="seealso"></a>Se även
* [Översikt: Apache Spark i Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenarier
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](hdinsight-apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid](hdinsight-apache-spark-eventhub-streaming.md)
* [Webbplatslogganalys med Spark i HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Skapa och köra program
* [Skapa ett fristående program med hjälp av Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Verktyg och tillägg
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-program](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Använda externa paket med Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)
