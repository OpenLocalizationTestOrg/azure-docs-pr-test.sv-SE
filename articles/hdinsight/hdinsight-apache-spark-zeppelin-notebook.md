---
title: "aaaUse Zeppelin-anteckningsböcker med Apache Spark-kluster i Azure HDInsight | Microsoft Docs"
description: "Stegvisa instruktioner för hur toouse Zeppelin-anteckningsböcker med Apache Spark-kluster i Azure HDInsight."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: df489d70-7788-4efa-a089-e5e5006421e2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 3ab479cfccc7fd38a9bf6a9fb4f5928beec8ff7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Använda Zeppelin-anteckningsböcker med Apache Spark-kluster i Azure HDInsight

HDInsight Spark-kluster innehåller Zeppelin-anteckningsböcker som du kan använda toorun Spark jobb. I den här artikeln lär du dig hur toouse hello Zeppelin anteckningsbok på ett HDInsight-kluster.

> [!NOTE]
> Zeppelin-anteckningsböcker är endast tillgängligt för Spark 1.6.3 i HDInsight 3.5 och Spark 2.1.0 i HDInsight 3,6.
>

**Krav:**

* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Starta en Zeppelin-anteckningsbok
1. Hello Spark-klusterbladet, klicka på **Klusterinstrumentpanel**, och klicka sedan på **Zeppelin anteckningsboken**. Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.
   
   > [!NOTE]
   > Du kan också nå hello Zeppelin anteckningsboken för klustret genom att öppna hello följande URL i webbläsaren. Ersätt **KLUSTERNAMN** med hello namnet på klustret:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`
   > 
   > 
2. Skapa en ny anteckningsbok. Hej rubrikfönstret klickar du på **anteckningsboken**, och klicka sedan på **Skapa ny anteckning**.
   
    ![Skapa en ny anteckningsbok Zeppelin](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "skapa en ny anteckningsbok Zeppelin")
   
    Ange ett namn för anteckningsboken hello och klicka sedan på **skapa anteckning**.
3. Kontrollera också att hello anteckningsboken sidhuvud visar status för anslutna. Den betecknas med en grön punkt i hello övre högra hörnet.
   
    ![Status för Zeppelin-anteckningsboken](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin anteckningsboken status")
4. Läs in exempeldata i en tillfällig tabell. När du skapar ett Spark-kluster i HDInsight, hello exempeldatafil **hvac.csv**, är kopierade toohello associerade lagringskontot under **\HdiSamples\SensorSampleData\hvac**.
   
    Klistra in följande kodutdrag hello i hello tomt stycke som skapas som standard i hello ny anteckningsbok.
   
        %livy.spark
        //hello above magic instructs Zeppelin toouse hello Livy Scala interpreter
   
        // Create an RDD using hello default Spark context, sc
        val hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
   
        // Map hello values in hello .csv file toohello schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
   
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
   
    Tryck på **SKIFT + RETUR** eller klicka på hello **spela upp** knapp för hello punkt toorun hello fragment. hello status på hello högra hörnet av hello punkt ska passera från klar, VÄNTANDE tooFINISHED KÖRS. hello utdata visas längst ned hello hello samma punkt. Det ser ut så hello följande hello skärmbild:
   
    ![Skapa en tillfällig tabell från rådata](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "skapa en tillfällig tabell från rådata")
   
    Du kan också ange en avdelning tooeach punkt. Hello högra hörnet och klicka på hello **inställningar** ikonen och klickar sedan på **Visa rubrik**.
5. Du kan nu köra Spark SQL-uttryck på hello **hvac** tabell. Klistra in följande fråga i ett nytt stycke hello. hello fråga hämtar hello byggnad ID och hello skillnaden mellan hello mål och faktiska temperaturer för varje bygger på ett visst datum. Tryck på **SKIFT + RETUR**.
   
        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
   
    Hej **% sql** instruktionen hello början visar hello anteckningsboken toouse hello Livius Scala tolken.
   
    hello visar följande skärmbild hello utdata.
   
    ![Köra Spark SQL-uttryck med hello anteckningsboken](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "köra Spark SQL-uttryck med hello anteckningsboken")
   
     Klicka på hello Visa alternativ (markerade i rektangel) tooswitch mellan olika representationer för hello samma utdata. Klicka på **inställningar** toochoose vilka consitutes hello nyckel och värden i hello utdata. Hej skärmdump ovan använder **buildingID** som hello nyckel och hello medelvärdet av **temp_diff** som hello-värde.
6. Du kan också köra Spark SQL-uttryck med hjälp av variabler i hello-frågan. Hej nästa fragment visar hur toodefine en variabel, **Temp**, i hello frågan med hello möjliga värden du tooquery med. När du först kör hello frågan fylls automatiskt en listruta med hello-värden som du har angett för hello-variabeln.
   
        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 
   
    Klistra in följande kodutdrag i en ny punkt och trycker på **SKIFT + RETUR**. hello visar följande skärmbild hello utdata.
   
    ![Köra Spark SQL-uttryck med hello anteckningsboken](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "köra Spark SQL-uttryck med hello anteckningsboken")
   
    För efterföljande frågor kan du välja ett nytt värde hello listrutan och kör hello frågan igen. Klicka på **inställningar** toochoose vilka consitutes hello nyckel och värden i hello utdata. Hej skärmdump ovan använder **buildingID** som hello nyckel hello medelvärde för **temp_diff** som hello värde och **targettemp** som hello grupp.
7. Starta om hello Livius tolken tooexit hello program. toodo det, öppnar tolkens inställningar genom att klicka på hello inloggad användarnamnet från hello övre högra hörnet och klicka sedan på **tolken**.
   
    ![Starta tolken](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive utdata")
8. Rulla tooLivy tolkens inställningar och klickar sedan på **starta om**.
   
    ![Starta om hello Livius intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "starta om hello Zeppelin intepreter")

## <a name="how-do-i-use-external-packages-with-hello-notebook"></a>Hur använder externa paket med hello bärbara?
Du kan konfigurera hello Zeppelin anteckningsboken i Apache Spark-kluster i HDInsight (Linux) toouse externa, community-bidragit paket som inte är inkluderade out-of-the-box i hello kluster. Du kan söka hello [Maven databasen](http://search.maven.org/) hello fullständig lista över paket som är tillgängliga. Du kan också hämta en lista över tillgängliga paket från andra källor. Till exempel en fullständig lista över community-bidragit paket finns på [Spark paket](http://spark-packages.org/).

I den här artikeln visas hur toouse hello [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) paket med Jupyter-anteckningsbok hello.

1. Öppna tolkens inställningar. Klicka på hello inloggad användarnamn hello övre högra hörnet och klicka sedan på **tolken**.
   
    ![Starta tolken](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive utdata")
2. Rulla tooLivy tolkens inställningar och klickar sedan på **redigera**.
   
    ![Ändra inställningarna för tolken](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "ändra tolkens inställningar")
3. Lägg till en ny nyckel kallas **livy.spark.jars.packages** och ange värdet i hello format `group:id:version`. Om du vill toouse hello [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) paketet, måste du ange hello värde hello nyckel för`com.databricks:spark-csv_2.10:1.4.0`.
   
    ![Ändra inställningarna för tolken](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "ändra tolkens inställningar")
   
    Klicka på **spara** och starta sedan om hello Livius tolken.
4. **Tips**: Om du vill toounderstand hur tooarrive på hello värdet för hello nyckeln som anges ovan här hur.
   
    a. Leta upp hello paketet i hello Maven-databasen. Den här självstudiekursen kommer vi använde [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Samla in hello värden för från hello databasen **GroupId**, **artefakt-ID**, och **Version**.
   
    ![Använda externa paket med Jupyter-anteckningsbok](./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "använda externa paket med Jupyter-anteckningsbok")
   
    c. Sammanfoga hello tre värden, avgränsade med kolon (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-hello-zeppelin-notebooks-saved"></a>Var finns hello Zeppelin-anteckningsböcker som sparats?
Hej Zeppelin-anteckningsböcker sparas toohello klustret headnodes. Så om du tar bort klustret hello hello anteckningsböcker tas bort även. Om du vill toopreserve dina anteckningsböcker för senare användning på andra kluster, måste du exportera dem när du har kört hello jobb. tooexport en bärbar dator, klicka på hello **exportera** ikon som visas i hello bilden nedan.

![Ladda ned anteckningsboken](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Download hello anteckningsboken")

Hello anteckningsboken sparas som en JSON-fil i din nedladdningsplatsen.

## <a name="livy-session-management"></a>Livius sessionshantering
När du kör hello kod punkt i anteckningsboken Zeppelin, skapas en ny session Livius i HDInsight Spark-kluster. Den här sessionen delas mellan alla Zeppelin-anteckningsböcker som du skapar. Om du av vissa orsak hello Livius sessionen har avslutats (klustret omstart osv.) kan du inte kan toorun jobb från hello Zeppelin bärbar dator.

I sådana fall måste du utföra följande steg innan du kan starta jobb som körs från en bärbar dator Zeppelin hello. 

1. Starta om hello Livius tolken från hello Zeppelin anteckningsbok. toodo det, öppnar tolkens inställningar genom att klicka på hello inloggad användarnamnet från hello övre högra hörnet och klicka sedan på **tolken**.
   
    ![Starta tolken](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive utdata")
2. Rulla tooLivy tolkens inställningar och klickar sedan på **starta om**.
   
    ![Starta om hello Livius intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "starta om hello Zeppelin intepreter")
3. Kör en cell kod från en befintlig Zeppelin-anteckningsbok. Detta skapar en ny session Livius i hello HDInsight-kluster.

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
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-appar](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md 







