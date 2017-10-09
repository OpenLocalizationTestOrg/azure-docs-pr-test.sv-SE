---
title: "Azure Toolkit för IntelliJ: skapa Spark-program för ett HDInsight-kluster | Microsoft Docs"
description: "Använd hello Azure Toolkit för IntelliJ toodevelop Spark-program som skrivits i Scala och skicka dem tooan HDInsight Spark-kluster."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 73304272-6c8b-482e-af7c-cd25d95dab4d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: nitinme
ms.openlocfilehash: 22cce014bb848a54e198e77a50bf13448012310e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-intellij-toocreate-spark-applications-for-an-hdinsight-cluster"></a>Använd Azure Toolkit för IntelliJ toocreate Spark-program för ett HDInsight-kluster

Använd hello Azure Toolkit för IntelliJ plugin toodevelop Spark-program som skrivits i Scala och skicka dem tooan HDInsight Spark-kluster direkt från hello IntelliJ integrerad utvecklingsmiljö (IDE). Du kan använda hello plugin-program på flera sätt:

* Utveckla och skicka Scala Spark-program på ett HDInsight Spark-kluster.
* Åtkomst till resurserna i Azure HDInsight Spark-kluster.
* Utveckla och köra ett Scala Spark-program lokalt.

toocreate ditt projekt, visa hello [skapa Spark-program med hello Azure Toolkit för IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) video.

> [!IMPORTANT]
> Du kan använda den här plugin-programmet toocreate och skicka program bara för ett HDInsight Spark-kluster på Linux.
> 

## <a name="prerequisites"></a>Krav

- Ett Apache Spark-kluster i HDInsight Linux. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
- Oracle Java Development Kit. Du kan installera den från hello [Oracle webbplats](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
- IntelliJ IDEA. Den här artikeln använder version 2017.1. Du kan installera den från hello [JetBrains webbplats](https://www.jetbrains.com/idea/download/).

## <a name="install-azure-toolkit-for-intellij"></a>Installera Azure Toolkit för IntelliJ
Installationsanvisningar finns i [installera Azure Toolkit för IntelliJ](../azure-toolkit-for-intellij-installation.md).

## <a name="sign-in-tooyour-azure-subscription"></a>Logga in tooyour Azure-prenumeration

1. Starta hello IntelliJ IDE och öppna Utforskaren i Azure. På hello **visa** väljer du **verktyget Windows**, och välj sedan **Azure Explorer**.
       
   ![hello Azure Explorer länk](./media/hdinsight-apache-spark-intellij-tool-plugin/show-azure-explorer.png)

2. Högerklicka på hello **Azure** och sedan väljer **logga In**.

3. I hello **Azure logga In** dialogrutan **logga in**, och ange dina autentiseringsuppgifter för Azure.

    ![hello Azure logga In dialogrutan](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-2.png)

4. När du har loggat in hello **Välj prenumerationer** listar dialogrutan alla hello Azure-prenumerationer som är associerade med hello autentiseringsuppgifter. Välj hello **Välj** knappen.

    ![dialogrutan Välj prenumerationer för hello](./media/hdinsight-apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

5. På hello **Azure Explorer** fliken, expandera **HDInsight** tooview hello HDInsight Spark-kluster som finns i din prenumeration.
   
    ![HDInsight Spark-kluster i Azure Explorer](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-3.png)

6. tooview hello resurser (till exempel lagringskonton) som är associerade med hello kluster, kan du ytterligare expandera en nod i klustret-namn.
   
    ![En expanderad klusternamnet nod](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Kör en Spark Scala på ett HDInsight Spark-kluster

1. Starta IntelliJ IDEA och sedan skapa ett projekt. I hello **nytt projekt** dialogrutan rutan, hello följande: 

   a. Välj **HDInsight** > **Spark i HDInsight (Scala)**.

   b. I hello **Build verktyget** väljer du antingen hello följande, enligt tooyour behov:

      * **Maven**, Scala skapa projekt guiden support
      * **Segregerade Barlasttankar**, för att hantera hello beroenden och bygga för hello Scala-projekt

    ![dialogrutan Nytt projekt för hello](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

2. Välj **nästa**.

3. Hej Scala skapa projekt guiden identifierar automatiskt om du har installerat hello Scala plugin-programmet. Välj **installera**.

   ![Kontroll av Scala plugin-program](./media/hdinsight-apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. toodownload hello Scala plugin-program, Välj **OK**. Följ hello instruktioner toorestart IntelliJ. 

   ![dialogrutan för hello Scala plugin-programmet för installation](./media/hdinsight-apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. I hello **nytt projekt** fönstret hello följande:  

    ![Att välja hello Spark SDK](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-new-project.png)

   a. Ange ett namn och plats.

   b. I hello **projekt SDK** listrutan, Välj **Java 1.8** för 2.x hello Spark-kluster eller välj **Java 1.7** för 1.x hello Spark-kluster.

   c. I hello **Spark version** listrutan Scala guide för att skapa projektet integreras hello rätt version för Spark SDK och Scala SDK. Om hello Spark-kluster av version är äldre än 2.0 väljer **Väck 1.x**. Annars väljer **Spark2.x**. Det här exemplet används **Spark punkt 2.0.2 (Scala 2.11.8)**.

6. Välj **Slutför**.

7. hello Spark-projekt skapas automatiskt en artefakt. tooview hello artefakt hello följande:

   a. På hello **filen** väljer du **projektstruktur**.

   b. I hello **projektstruktur** dialogrutan **artefakter** tooview hello standard artefakt som har skapats. Du kan också skapa egna artefakt genom att välja hello plustecken (**+**).

      ![Artefakt info hello dialogrutan](./media/hdinsight-apache-spark-intellij-tool-plugin/default-artifact.png)
      
8. Lägg till källkoden programmet hello följande:

   a. Högerklicka i Projektutforskaren **src**, peka för**ny**, och välj sedan **Scala klassen**.
      
      ![Kommandon för att skapa en Scala-klass från Projektutforskaren](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   b. I hello **skapa nya Scala klass** dialogrutan, ange ett namn, väljer **objekt** i hello **typ** och välj sedan **OK**.
      
      ![Skapa ny Scala klass dialogruta](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   c. I hello **MyClusterApp.scala** fil, klistra in följande kod hello. hello koden läser hello data från HVAC.csv (finns i alla HDInsight Spark-kluster), hämtar hello rader som har endast en siffra i hello sjunde kolumnen i hello CSV-fil och skriver hello utdata för**/HVACOut** under hello standard lagringsbehållare som klustret hello.

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object MyClusterApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find hello rows that have only one digit in hello seventh column in hello CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasb:///HVACOut")
            }
    
        }

9. Kör hello på ett HDInsight Spark-kluster genom att göra hello följande:

   a. Högerklicka på hello projektnamn i Projektutforskaren, och välj sedan **skicka Spark-program tooHDInsight**.
      
      ![hello skicka Spark-program tooHDInsight kommando](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

   b. Du är tooenter ange dina autentiseringsuppgifter för Azure-prenumeration. I hello **Spark skicka** dialogrutan, ange följande värden hello och välj sedan **skicka**.
      
      * För **Väck kluster (endast Linux)**, Välj hello HDInsight Spark-kluster som du vill använda toorun ditt program.

      * Välj en artefakt hello IntelliJ projektet eller välj en från hello hårddisken.

      * I hello **Main klassnamn** rutan, Välj hello ellips (**...** ), Välj hello huvudsakliga klass i din programkod för datakälla och väljer sedan **OK**.

        ![dialogrutan för hello Välj Main-klass](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)

      * Eftersom hello programkod i det här exemplet inte kräver kommandoradsargument eller referera burkar eller filer, kan du lämna hello återstående rutor som är tom. När du har angett alla hello information bör hello dialogrutan likna följande bild hello.
        
        ![dialogrutan för hello Spark överföring](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

   c. Hej **Spark skicka** fliken längst hello hello fönstret ska börja visas hello pågår. Du kan också avbryta hello program genom att välja hello röd knapp i hello **Spark skicka** fönster.
      
      ![hello Spark skicka fönster](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      toolearn hur tooaccess hello jobbutdata, finns i hello ”åtkomst och hantera HDInsight Spark-kluster med hjälp av Azure Toolkit för IntelliJ” senare i den här artikeln.

## <a name="run-or-debug-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Köra eller felsöka ett Spark Scala-program på ett HDInsight Spark-kluster
Vi rekommenderar också ett annat sätt att skicka hello Spark programmet toohello-kluster. Du kan göra det genom att ange hello parametrar i hello **kör/Debug konfigurationer** IDE. Mer information finns i [felsöka Spark-program på ett HDInsight-kluster med Azure Toolkit för IntelliJ via SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Komma åt och hantera HDInsight Spark-kluster med hjälp av Azure Toolkit för IntelliJ
Du kan utföra olika åtgärder med hjälp av Azure Toolkit för IntelliJ.

### <a name="access-hello-job-view"></a>Åtkomst hello jobbet vy
1. I Azure Explorer expanderar **HDInsight**, expandera klusternamnet i hello Spark och välj sedan **jobb**.  

    ![Jobbet visa nod](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. I högra fönstret hello hello **Spark jobbet vyn** visar alla hello-program som kördes på hello klustret. Välj hello hello program som du vill toosee mer information.

    ![Programinformation](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)

3. toodisplay grundläggande körs jobbinformation hovra över hello jobbdiagram. tooview hello steg diagram och information som genereras av varje jobb markerar du en nod i hello jobbdiagram.

    ![Steget jobbinformation](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. tooview används ofta loggar som *drivrutinen Stderr*, *drivrutinen Stdout*, och *Directory Info*väljer hello **loggen** fliken.

    ![Logginformation](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)

5. Du kan också visa hello Spark historik UI och hello YARN-Användargränssnittet (på programnivå hello) genom att välja en länk hello överst i fönstret hello.

### <a name="access-hello-spark-history-server"></a>Historik fjärråtkomstserver hello Spark
1. I Azure Explorer expanderar **HDInsight**, högerklickar du på klusternamnet Spark och väljer sedan **öppna Spark historik UI**. 

2. När du uppmanas ange administratörsautentiseringsuppgifter för hello klustret som du angav när du ställer in hello kluster.

3. Du kan använda hello programmet namnet toolook för hello program just avslutats körs på hello Spark historik server instrumentpanelen. I hello föregående kod, ange hello programnamn med `val conf = new SparkConf().setAppName("MyClusterApp")`. Programnamnet Spark är därför **MyClusterApp**.

### <a name="start-hello-ambari-portal"></a>Starta hello Ambari-portalen
1. I Azure Explorer expanderar **HDInsight**, högerklickar du på klusternamnet Spark och väljer sedan **öppna klustret hanteringsportalen (Ambari)**. 

2. När du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret. Du har angett autentiseringsuppgifterna under hello klusterkonfigurationen.

### <a name="manage-azure-subscriptions"></a>Hantera Azure-prenumerationer
Som standard visar Azure Toolkit för IntelliJ hello Spark-kluster från alla dina Azure-prenumerationer. Om det behövs kan du ange hello prenumerationer som du vill tooaccess. 

1. Högerklicka i Azure Explorer hello **Azure** rot nod och välj sedan **hantera prenumerationer**. 

2. Hej, avmarkera hello kryssrutorna nästa toohello prenumerationer som du inte vill tooaccess och välj sedan **Stäng**. Du kan också välja **logga ut** om du vill toosign utanför din Azure-prenumeration.

## <a name="run-a-spark-scala-application-locally"></a>Köra ett program med Spark Scala lokalt
Du kan använda Azure Toolkit för IntelliJ toorun Spark Scala program lokalt på din arbetsstation. hello program vanligtvis inte behöver komma åt toocluster resurser, till exempel behållare för lagring, och du kan köra och testa dem lokalt.

### <a name="prerequisite"></a>Krav
När du kör hello lokalt Spark Scala-program på en Windows-dator, kan du få ett undantag som beskrivs i [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356). hello-undantag inträffar eftersom WinUtils.exe saknas i Windows. 

tooresolve det här felet [hämta hello körbara](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa plats som **C:\WinUtils\bin**. Lägg sedan till hello miljövariabeln **HADOOP_HOME**, och ange hello hello variabels värde för**C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>Kör ett lokalt Spark Scala-program
1. Starta IntelliJ IDEA och skapa ett projekt. 

2. I hello **nytt projekt** dialogrutan rutan, hello följande:
   
    a. Välj **HDInsight** > **Spark på HDInsight lokala kör sampel (Scala)**.

    b. I hello **Build verktyget** väljer du antingen hello följande, enligt tooyour behov:

      * **Maven**, Scala skapa projekt guiden support
      * **Segregerade Barlasttankar**, för att hantera hello beroenden och bygga för hello Scala-projekt

    ![dialogrutan Nytt projekt för hello](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run.png)

3. Välj **nästa**.
 
4. I nästa hello-fönstret hello följande:
   
    a. Ange ett namn och plats.

    b. I hello **projekt SDK** listrutan väljer du en Java-version som är senare än version 1.7.

    c. I hello **Spark Version** listrutan, Välj hello version av Scala som du vill toouse: Scala 2.11.x för Spark 2.0 eller Scala 2.10.x för Spark 1.6.

    ![dialogrutan Nytt projekt för hello](./media/hdinsight-apache-spark-intellij-tool-plugin/Create-local-project.PNG)

5. Välj **Slutför**.

6. hello mallen lägger till en exempelkod (**LogQuery**) under hello **src** mapp som du kan köra lokalt på datorn.
   
    ![Platsen för LogQuery](./media/hdinsight-apache-spark-intellij-tool-plugin/local-app.png)

7. Högerklicka på hello **LogQuery** program och välj sedan **kör 'LogQuery'**. På hello **kör** fliken hello längst ned i visas utdata som liknar hello följande:
   
   ![Spark programmet lokala kör resultat](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="convert-existing-intellij-idea-applications-toouse-azure-toolkit-for-intellij"></a>Konvertera befintliga IntelliJ IDEA program toouse Azure Toolkit för IntelliJ
Du kan konvertera hello befintliga Spark Scala program som du skapade i IntelliJ IDEA toobe kompatibla med Azure Toolkit för IntelliJ. Du kan sedan använda hello plugin toosubmit hello program tooan HDInsight Spark-kluster.

1. Öppna hello associerade .iml fil för ett befintligt Spark Scala program som har skapats via IntelliJ IDEA.

2. Hello roten nivån är en **modulen** hello följande element:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   Redigera hello elementet tooadd `UniqueKey="HDInsightTool"` så som hello **modulen** ser ut som hello nedan:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

3. Spara hello ändringar. Ditt program bör nu vara kompatibla med Azure Toolkit för IntelliJ. Du kan testa den genom att högerklicka på projektnamnet på hello i Projektutforskaren. hello popup-menyn har nu hello alternativet **skicka Spark-program tooHDInsight**.

## <a name="troubleshooting"></a>Felsökning

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a>Fel vid lokal körning: *Använd en större heapstorlek*
I Spark 1.6 om du använder en 32-bitars Java SDK under lokal körning kan uppstå hello följande fel:

    Exception in thread "main" java.lang.IllegalArgumentException: System memory 259522560 must be at least 4.718592E8. Please use a larger heap size.
        at org.apache.spark.memory.UnifiedMemoryManager$.getMaxMemory(UnifiedMemoryManager.scala:193)
        at org.apache.spark.memory.UnifiedMemoryManager$.apply(UnifiedMemoryManager.scala:175)
        at org.apache.spark.SparkEnv$.create(SparkEnv.scala:354)
        at org.apache.spark.SparkEnv$.createDriverEnv(SparkEnv.scala:193)
        at org.apache.spark.SparkContext.createSparkEnv(SparkContext.scala:288)
        at org.apache.spark.SparkContext.<init>(SparkContext.scala:457)
        at LogQuery$.main(LogQuery.scala:53)
        at LogQuery.main(LogQuery.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:606)
        at com.intellij.rt.execution.application.AppMain.main(AppMain.java:144)

Dessa fel inträffa eftersom hello heap-storlek inte är tillräckligt stor för Spark toorun. Spark kräver minst 471 MB. (Mer information finns i [SPARK 12081](https://issues.apache.org/jira/browse/SPARK-12081).) En enkel lösning är toouse en 64-bitars Java SDK. Du kan också ändra hello JVM i IntelliJ genom att lägga till hello följande alternativ:

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Lägga till alternativ toohello rutan ”alternativ för VM” i IntelliJ](./media/hdinsight-apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
toosubmit ett program tooAzure Data Lake Store, Välj **interaktiv** läge under hello Azure-inloggning. Om du väljer **automatisk** läge, du får ett felmeddelande.

![Interaktiv inloggning](./media/hdinsight-apache-spark-intellij-tool-plugin/interative-signin.png)

Nu ska löste vi problemet. Du kan välja ett Azure Data Lake klustret toosubmit ditt program med valfri metod för inloggning.

## <a name="feedback-and-known-issues"></a>Feedback och kända problem
För närvarande kan stöds visa Spark utdata direkt inte.

Om du har förslag eller feedback eller om du stöter på problem när du använder plugin-modulen, skicka e-post på hdivstool@microsoft.com.

## <a name="seealso"></a>Nästa steg
* [Översikt: Apache Spark i Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="demo"></a>Demo
* Skapa Scala projekt (video): [skapa Spark Scala-program](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Fjärråtkomst debug (video): [Använd Azure Toolkit för IntelliJ toodebug Spark-program på HDInsight-kluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Scenarier
* [Spark med BI: utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](hdinsight-apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: använda Spark i HDInsight tooanalyze skapa temperatur med HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Använda Spark i HDInsight toobuild strömmande realtidsprogram](hdinsight-apache-spark-eventhub-streaming.md)
* [Webbplatslogganalys med Spark i HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Skapa och köra program
* [Skapa ett fristående program med hjälp av Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Verktyg och tillägg
* [Använd Azure Toolkit för IntelliJ toodebug Spark-program via fjärranslutning via VPN](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använd Azure Toolkit för IntelliJ toodebug Spark-program via fjärranslutning via SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Använda HDInsight Tools för IntelliJ med Hortonworks Sandbox](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Använda HDInsight Tools i Azure Toolkit för Eclipse toocreate Spark-program](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Hantera resurser
* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)

