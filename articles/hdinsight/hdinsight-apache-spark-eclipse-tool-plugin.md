---
title: "aaaAzure Toolkit för Eclipse - skapa Scala program för HDInsight Spark | Microsoft Docs"
description: "Använda HDInsight Tools i Azure Toolkit för Eclipse toodevelop Spark-program som skrivits i Scala och skicka dem tooan HDInsight Spark-kluster, direkt från hello Eclipse IDE."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f6c79550-5803-4e13-b541-e86c4abb420b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: nitinme
ms.openlocfilehash: 3ab70857c1e81f591a1c7e29bc1706ec4899ff58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-eclipse-toocreate-spark-applications-for-an-hdinsight-cluster"></a>Använd Azure Toolkit för Eclipse toocreate Spark-program för ett HDInsight-kluster

Använda HDInsight Tools i Azure Toolkit för Eclipse toodevelop Spark-program som skrivits i Scala och skicka dem tooan Azure HDInsight Spark-kluster, direkt från hello Eclipse IDE. Du kan använda hello HDInsight Tools-plugin-programmet på några olika sätt:

* toodevelop och skicka Scala Spark-program på ett HDInsight Spark-kluster
* tooaccess resurserna i Azure HDInsight Spark-kluster
* toodevelop och köra Scala Spark programmet lokalt

> [!IMPORTANT]
> Det här verktyget kan använda toocreate och skicka program bara för ett HDInsight Spark-kluster på Linux.
> 
> 

## <a name="prerequisites"></a>Krav

* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java Development Kit version 8, som används för hello Eclipse IDE runtime. Du kan ladda ned det från hello [Oracle webbplats](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* Eclipse IDE. Den här artikeln använder Eclipse Neon. Du kan installera den från hello [Eclipse webbplats](https://www.eclipse.org/downloads/).   
* Spark SDK. Du kan ladda ned det från [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).


## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-scala-plugin"></a>Installera HDInsight-verktyg i Azure Toolkit för Eclipse och Scala-plugin-programmet
### <a name="install-hdinsight-tools"></a>Installera HDInsight-verktyg
HDInsight-verktyg för Eclipse är tillgänglig som en del av Azure Toolkit för Eclipse. Installationsanvisningar finns i [installerar Azure Toolkit för Eclipse](../azure-toolkit-for-eclipse-installation.md).
### <a name="install-scala-plugin"></a>Installera Scala plugin-programmet
När du öppnar hello Intellij identifierar hello HDInsight Tools automatiskt om du har installerat Scala plugin-programmet eller inte. Klicka på **OK** toocontinue och följ hello instruktioner tooinstall av hello Eclipse Marketplace.

 ![Automatisk installation Scala plugin-program](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala.png)

## <a name="sign-in-tooyour-azure-subscription"></a>Logga in tooyour Azure-prenumeration
1. Starta hello Eclipse IDE och öppna Utforskaren i Azure. På hello **fönstret** -menyn klickar du på **visa**, och klicka sedan på **andra**. Expandera i hello dialogrutan som öppnas, **Azure**, klickar du på **Azure Explorer**, och klicka sedan på **OK**.

    ![Visa dialogrutan vy](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-1.png)
2. Högerklicka på hello **Azure** noden och klicka sedan på **logga in**.
3. I hello **Azure logga In** dialogrutan Välj autentiseringsmetod för hello, klickar du på **logga in**, och ange dina autentiseringsuppgifter för Azure.
   
    ![Azure logga In dialogrutan](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-2.png)
4. När du har loggat in hello **Välj prenumerationer** listar dialogrutan alla hello Azure-prenumerationer som är kopplade till hello autentiseringsuppgifter. Klicka på **Välj** tooclose hello dialogrutan.

    ![Dialogrutan för prenumerationer](./media/hdinsight-apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
5. På hello **Azure Explorer** fliken, expandera **HDInsight** toosee hello HDInsight Spark-kluster i din prenumeration.
   
    ![HDInsight Spark-kluster i Azure Explorer](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-3.png)
6. Ytterligare kan du expandera en namnet nod toosee hello klusterresurser (till exempel storage-konton) som är associerade med hello-kluster.
   
    ![Expandera en klusterresurser namn toosee](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-4.png)



## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a>Ställ in ett Spark Scala-projekt för ett HDInsight Spark-kluster

1. Hello Eclipse IDE arbetsytan klickar **filen**, klickar du på **ny**, och klicka sedan på **projekt**. 
2. Hello nytt projekt i guiden expanderar **HDInsight**väljer **Spark i HDInsight (Scala)**, och klicka sedan på **nästa**.

    ![Att välja hello Spark på HDInsight (Scala)-projekt](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
3. Hej Scala projekt skapa guiden automatiskt identifierar om du har installerat Scala plugin-programmet eller inte. Klicka på **OK** toocontinue hämtar hello Scala plugin-program och sedan följa hello instruktioner toorestart Eclipse.

    ![Kontrollera scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
4. I hello **nytt projekt för HDInsight Scala** dialogrutan hello följande värden, och klickar sedan på **nästa**:
   * Ange ett namn för hello-projekt.
   * I hello **JRE** område, se till att **använder en körningsmiljö JRE** har angetts för**JavaSE 1.7** eller senare.
   * Se till att Spark SDK är toohello platsen dit du hämtade hello SDK. hello länken toohello hämtningsplats ingår i hello [krav](#prerequisites) tidigare i den här artikeln. Du kan också hämta hello SDK från hello länk som ingår i hello dialogrutan.

    ![Dialogrutan Nytt projekt för HDInsight Scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
5.  Klicka på hello hello nästa i dialogrutan **bibliotek** fliken hålla hello standardvärden och klicka sedan på **Slutför**. 
   
    ![Biblioteksfliken](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-4.png)
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a>Skapa ett Scala program för ett HDInsight Spark-kluster

1. Expandera hello-projekt som du skapade tidigare i hello Eclipse IDE från paketet Explorer, högerklicka på **src**, peka för**ny**, och klicka sedan på **andra**.
2. I hello **Välj en guide** dialogrutan Expandera **Scala guider**, klickar du på **Scala objekt**, och klicka sedan på **nästa**.
   
    ![Välj en dialogruta för guiden](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
3. I hello **Skapa ny fil** dialogrutan Ange ett namn för hello-objektet och klicka sedan på **Slutför**.
   
    ![Skapa en ny fil dialogruta](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
4. Klistra in följande kod i hello textredigerare hello:
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        object MyClusterApp{
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find hello rows that have only one digit in hello seventh column in hello CSV
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACOut")
          }        
        }
5. Kör hello på ett HDInsight Spark-kluster:
   
   1. Högerklicka på projektnamnet hello från Utforskaren i paketet, och välj sedan **skicka Spark-program tooHDInsight**.        
   2. I hello **Spark skicka** dialogrutan hello följande värden, och klickar sedan på **skicka**:
      
      * För **klusternamnet**, Välj hello HDInsight Spark-kluster som du vill använda toorun ditt program.
      * Välj en artefakt hello Eclipse-projektet, eller välja en från en hårddisk. hello Standardvärdet beror på hello objekt du högerklickar på paketet Explorer.
      * I hello **Main klassnamn** dropdownlist skickas visas alla objektnamn från projektet valda. Välj eller ange en som du vill toorun. Om du väljer artefakt från hårddisk måste du ange huvudsakliga klassnamn själv. 
      * Eftersom hello programkod i det här exemplet inte kräver några kommandoradsargument eller referera burkar eller filer, kan du lämna hello återstående textrutor är tom.
        
       ![Dialogrutan Skicka Spark](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
   3. Hej **Spark skicka** fliken ska börja visas hello pågår. Du kan stoppa hello program genom att klicka hello red i hello **Spark skicka** fönster. Du kan också visa hello loggar för specifika programmet kör genom att klicka på hello Globikon (betecknas med hello blå ruta hello bild).
      
       ![Skicka Spark-fönster](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a>Komma åt och hantera HDInsight Spark-kluster med hjälp av HDInsight-verktyg i Azure Toolkit för Eclipse
Du kan utföra olika åtgärder med hjälp av HDInsight-verktyg, inklusive åtkomst till hello jobbutdata.

### <a name="access-hello-job-view"></a>Åtkomst hello jobbet vy
1. I Azure Explorer expanderar **HDInsight**, expandera klusternamnet i hello Spark och klicka sedan på **jobb**. 

    ![Jobbet visa nod](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. Klicka på hello **jobb** nod. Hej HDInsight Tools identifieras om du har installerat hello E (fx) clipse plugin-programmet eller inte. Klicka på **OK** toocontinue och följ hello instruktioner tooinstall hello Eclipse Marketplace och starta om Eclipse.

    ![Installera clipse E (fx)](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

3. Öppna hello jobbet från hello **jobb** nod. I högra fönstret hello hello **Spark jobbet vyn** visar alla hello-program som kördes på hello klustret. Klicka på hello namnet på hello program som du vill toosee mer information.

    ![Programinformation](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)
4. Om du hovrar på hello jobbdiagram visar grundläggande körs Jobbinformationen. Visas klickar på hello jobbdiagram hello steg diagram och information som genereras av varje jobb.

    ![Steget jobbinformation](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

5. Vanliga loggar, inklusive drivrutinen Stderr, drivrutinen Stdout och Directory information visas i hello **loggen** fliken.

    ![Logginformation](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)
6. Du kan också öppna hello Spark historik UI och hello YARN-Användargränssnittet (på programnivå hello) genom att klicka på hello respektive hyperlink hello överst i fönstret hello.

### <a name="access-hello-storage-container-for-hello-cluster"></a>Åtkomst hello lagringsbehållare som klustret hello
1. I Azure Explorer expanderar du hello **HDInsight** rot nod toosee en lista över HDInsight Spark-kluster som är tillgängliga.
2. Expandera hello toosee hello lagring klusternamnkontot och hello standardbehållare för lagring för hello-kluster.
   
    ![Storage-konto och standard lagringsbehållare](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-5.png)
3. Klicka på hello namnet på lagringsbehållaren som associerats med hello kluster. I hello högra rutan, dubbelklickar du på hello **HVACOut** mapp. Öppna en hello **del -** toosee hello utdata från programmet hello-filer.

### <a name="access-hello-spark-history-server"></a>Historik fjärråtkomstserver hello Spark
1. Högerklicka på klusternamnet Spark i Azure Explorer och markera **öppna Spark historik UI**. När du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret. Du måste ange dessa vid etablering hello klustret.
2. I instrumentpanelen för hello Spark historik server använder du hello programmet namnet toolook för hello program just avslutats körs. I hello föregående kod, ange hello programnamn med `val conf = new SparkConf().setAppName("MyClusterApp")`. Därför programnamnet Spark har **MyClusterApp**.

### <a name="start-hello-ambari-portal"></a>Starta hello Ambari-portalen
1. Högerklicka på klusternamnet Spark i Azure Explorer och markera **öppna klustret hanteringsportalen (Ambari)**. 
2. När du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret. Du måste ange dessa vid etablering hello klustret.

### <a name="manage-azure-subscriptions"></a>Hantera Azure-prenumerationer
Som standard visar HDInsight-verktyg i Azure Toolkit för Eclipse hello Spark-kluster från alla dina Azure-prenumerationer. Du kan ange hello prenumerationer som du vill tooaccess hello klustret om det behövs. 

1. Högerklicka i Azure Explorer hello **Azure** rot noden och klicka sedan på **hantera prenumerationer**. 
2. I dialogrutan för hello avmarkera hello för hello prenumeration som du inte vill tooaccess och klicka sedan på **Stäng**. Du kan också klicka på **logga ut** om du vill toosign utanför din Azure-prenumeration.

## <a name="run-a-spark-scala-application-locally"></a>Köra ett program med Spark Scala lokalt
Du kan använda HDInsight Tools i Azure Toolkit för Eclipse toorun Spark Scala program lokalt på din arbetsstation. Normalt programmen behöver inte komma åt toocluster resurser, till exempel en lagringsbehållare och du kan köra och testa dem lokalt.

### <a name="prerequisite"></a>Krav
När du kör hello lokalt Spark Scala-program på en Windows-dator, kan du få ett undantag som beskrivs i [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356). Det här undantaget inträffar eftersom **WinUtils.exe** saknas i Windows. 

tooresolve det här felet måste du [hämta hello körbara](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa plats som **C:\WinUtils\bin**. Sedan måste du lägga till hello miljövariabeln **HADOOP_HOME** och ange hello hello variabels värde för**C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>Kör ett lokalt Spark Scala-program
1. Starta Eclipse och skapa ett projekt. I hello **nytt projekt** dialogrutan att Hej följande alternativ och klickar sedan på **nästa**.
   
   * I hello vänster och välj **HDInsight**.
   * I hello högra rutan, Välj **Spark på HDInsight lokala kör sampel (Scala)**.

    ![Dialogrutan Nytt projekt](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
2. tooprovide hello projektinformation, Följ steg 3 till 6 från hello tidigare avsnittet [Ställ in ett Spark Scala-projekt för ett HDInsight Spark-kluster](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).
3. hello mallen lägger till en exempelkod (**LogQuery**) under hello **src** mapp som du kan köra lokalt på datorn.
   
    ![Platsen för LogQuery](./media/hdinsight-apache-spark-eclipse-tool-plugin/local-app.png)
4. Högerklicka på hello **LogQuery** program, peka för**kör som**, och klicka sedan på **1 Scala program**. Du kommer att se utdata som liknar detta i hello **konsolen** fliken längst ned hello:
   
   ![Spark programmet lokala kör resultat](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
toosubmit ett program tooAzure Data Lake Store, Välj **interaktiv** läge under hello Azure-inloggning. Om du väljer **automatisk** läge, du får ett felmeddelande.

![Interaktiv inloggning](./media/hdinsight-apache-spark-eclipse-tool-plugin/interactive-authentication.png)

Nu ska löste vi problemet. Du kan välja ett Azure Data Lake klustret toosubmit ditt program med valfri metod för inloggning.

## <a name="feedback-and-known-issues"></a>Feedback och kända problem
För närvarande kan stöds visa Spark utdata direkt inte.

Om du har förslag eller feedback eller om du stöter på problem när du använder det här verktyget känna sig fria toosend oss ett e-postmeddelande på hdivstool@microsoft.com.

## <a name="seealso"></a>Se även
* [Översikt: Apache Spark i Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenarier
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](hdinsight-apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid](hdinsight-apache-spark-eventhub-streaming.md)
* [Webbplatslogganalys med Spark i HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Skapa och köra program
* [Skapa ett fristående program med hjälp av Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Verktyg och tillägg
* [Använd Azure Toolkit för IntelliJ toocreate och skicka Spark Scala-program](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Använd Azure Toolkit för IntelliJ toodebug Spark-program via fjärranslutning via VPN](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använd Azure Toolkit för IntelliJ toodebug Spark-program via fjärranslutning via SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Använda HDInsight Tools för IntelliJ med Hortonworks Sandbox](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Hantera resurser
* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)

