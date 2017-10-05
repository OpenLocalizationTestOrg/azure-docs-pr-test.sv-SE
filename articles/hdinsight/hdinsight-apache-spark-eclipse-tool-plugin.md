---
title: "Azure Toolkit för Eclipse - skapa Scala program för HDInsight Spark | Microsoft Docs"
description: "Använda HDInsight Tools i Azure Toolkit för Eclipse för att utveckla Spark-program som skrivits i Scala och skicka dem till ett HDInsight Spark-kluster från dem Eclipse IDE."
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
ms.openlocfilehash: 4bcb1987a62c0b7f4965e6fd257315e820004238
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-toolkit-for-eclipse-to-create-spark-applications-for-an-hdinsight-cluster"></a>Använda Azure Toolkit för Eclipse för att skapa Spark-program för ett HDInsight-kluster

Använda HDInsight Tools i Azure Toolkit för Eclipse för att utveckla Spark-program som skrivits i Scala och skicka dem till ett Azure HDInsight Spark-kluster direkt från Eclipse IDE. Du kan använda HDInsight Tools-plugin-programmet på några olika sätt:

* Att utveckla och skicka Scala Spark-program på ett HDInsight Spark-kluster
* Åtkomst till resurserna i Azure HDInsight Spark-kluster
* Att utveckla och köra ett Scala Spark-program lokalt

> [!IMPORTANT]
> Det här verktyget kan användas för att skapa och skicka program bara för ett HDInsight Spark-kluster på Linux.
> 
> 

## <a name="prerequisites"></a>Krav

* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java Development Kit version 8, som används för Eclipse IDE-körningsmiljön. Du kan ladda ned det från den [Oracle webbplats](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* Eclipse IDE. Den här artikeln använder Eclipse Neon. Du kan installera det från den [Eclipse webbplats](https://www.eclipse.org/downloads/).   
* Spark SDK. Du kan ladda ned det från [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).


## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-scala-plugin"></a>Installera HDInsight-verktyg i Azure Toolkit för Eclipse och Scala-plugin-programmet
### <a name="install-hdinsight-tools"></a>Installera HDInsight-verktyg
HDInsight-verktyg för Eclipse är tillgänglig som en del av Azure Toolkit för Eclipse. Installationsanvisningar finns i [installerar Azure Toolkit för Eclipse](../azure-toolkit-for-eclipse-installation.md).
### <a name="install-scala-plugin"></a>Installera Scala plugin-programmet
När du öppnar Intellij identifierar HDInsight Tools automatiskt om du har installerat Scala plugin-programmet eller inte. Klicka på **OK** och följ instruktionerna för att installera genom Eclipse Marketplace.

 ![Automatisk installation Scala plugin-program](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala.png)

## <a name="sign-in-to-your-azure-subscription"></a>Logga in till din Azure-prenumeration
1. Starta Eclipse IDE och öppna Utforskaren i Azure. På den **fönstret** -menyn klickar du på **visa**, och klicka sedan på **andra**. I dialogrutan som öppnas, expandera **Azure**, klickar du på **Azure Explorer**, och klicka sedan på **OK**.

    ![Visa dialogrutan vy](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-1.png)
2. Högerklicka på den **Azure** noden och klicka sedan på **logga in**.
3. I den **Azure logga In** dialogrutan Välj autentiseringsmetod, klickar du på **logga in**, och ange dina autentiseringsuppgifter för Azure.
   
    ![Azure logga In dialogrutan](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-2.png)
4. När du har loggat in, den **Välj prenumerationer** i dialogrutan visas alla de Azure-prenumerationer som är associerade med autentiseringsuppgifter. Klicka på **Välj** att stänga dialogrutan.

    ![Dialogrutan för prenumerationer](./media/hdinsight-apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
5. På den **Azure Explorer** fliken, expandera **HDInsight** att se HDInsight Spark-kluster i din prenumeration.
   
    ![HDInsight Spark-kluster i Azure Explorer](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-3.png)
6. Ytterligare kan du expandera en nod i namn om du vill se de resurser (till exempel storage-konton) som associeras med klustret.
   
    ![Expandera en klusternamnet finns resurser](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-4.png)



## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a>Ställ in ett Spark Scala-projekt för ett HDInsight Spark-kluster

1. I arbetsytan Eclipse IDE klickar du på **filen**, klickar du på **ny**, och klicka sedan på **projekt**. 
2. Expandera i guiden Nytt projekt **HDInsight**väljer **Spark i HDInsight (Scala)**, och klicka sedan på **nästa**.

    ![Att välja Spark i HDInsight (Scala)-projekt](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
3. Scala projekt skapa guiden automatiskt identifierar om du har installerat Scala plugin-programmet eller inte. Klicka på **OK** om du vill fortsätta att ladda ned Scala plugin-programmet, följ instruktionerna för att starta om Eclipse.

    ![Kontrollera scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
4. I den **nytt projekt för HDInsight Scala** dialogrutan, ange följande värden och klicka sedan på **nästa**:
   * Ange ett namn för projektet.
   * I den **JRE** område, se till att **använder en körningsmiljö JRE** är inställd på **JavaSE 1.7** eller senare.
   * Kontrollera att Spark SDK anges till den plats där du sparade SDK. Länk till nedladdningsplatsen ingår i den [krav](#prerequisites) tidigare i den här artikeln. Du kan också hämta SDK från länken som ingår i dialogrutan.

    ![Dialogrutan Nytt projekt för HDInsight Scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
5.  I dialogrutan nästa klickar du på den **bibliotek** fliken Behåll standardvärdena och klicka sedan på **Slutför**. 
   
    ![Biblioteksfliken](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-4.png)
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a>Skapa ett Scala program för ett HDInsight Spark-kluster

1. I Eclipse-IDE från paketet Explorer, expandera projektet som du skapade tidigare, högerklicka på **src**, peka på **ny**, och klicka sedan på **andra**.
2. I den **Välj en guide** dialogrutan Expandera **Scala guider**, klickar du på **Scala objekt**, och klicka sedan på **nästa**.
   
    ![Välj en dialogruta för guiden](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
3. I den **Skapa ny fil** dialogrutan, ange ett namn för objektet och klicka sedan på **Slutför**.
   
    ![Skapa en ny fil dialogruta](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
4. Klistra in följande kod i textredigeraren:
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        object MyClusterApp{
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows that have only one digit in the seventh column in the CSV
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACOut")
          }        
        }
5. Kör programmet på ett HDInsight Spark-kluster:
   
   1. Högerklicka på projektnamnet paketet Explorer och markera sedan **skicka Spark-program till HDInsight**.        
   2. I den **Spark skicka** dialogrutan, ange följande värden och klicka sedan på **skicka**:
      
      * För **klusternamnet**, Välj HDInsight Spark-kluster som du vill köra programmet.
      * Välj en artefakt Eclipse-projektet, eller välja en från en hårddisk. Standardvärdet beror på det objekt du högerklickar på paketet Explorer.
      * I den **Main klassnamn** dropdownlist skickas visas alla objektnamn från projektet valda. Välj eller ange en som du vill köra. Om du väljer artefakt från hårddisk måste du ange huvudsakliga klassnamn själv. 
      * Eftersom programkoden i det här exemplet inte kräver några kommandoradsargument eller referera burkar eller filer, kan du lämna textrutorna tomt.
        
       ![Dialogrutan Skicka Spark](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
   3. Den **Spark skicka** fliken ska börja visas förloppet. Du kan stoppa programmet genom att klicka på red i den **Spark skicka** fönster. Du kan också visa loggarna för specifika programmet kör genom att klicka på ikonen (betecknas med den blå rutan i bilden).
      
       ![Skicka Spark-fönster](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a>Komma åt och hantera HDInsight Spark-kluster med hjälp av HDInsight-verktyg i Azure Toolkit för Eclipse
Du kan utföra olika åtgärder med hjälp av HDInsight-verktyg, inklusive åtkomst till jobbutdata.

### <a name="access-the-job-view"></a>Åtkomst till vyn jobb
1. I Azure Explorer expanderar **HDInsight**, expandera klusternamnet Spark och klicka sedan på **jobb**. 

    ![Jobbet visa nod](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. Klicka på den **jobb** nod. HDInsight Tools identifieras om du har installerat E (fx) clipse plugin-programmet eller inte. Klicka på **OK** och följ instruktionerna för att installera Eclipse Marketplace och starta om Eclipse.

    ![Installera clipse E (fx)](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

3. Öppna vyn jobb från de **jobb** nod. I den högra rutan i **Spark jobbet vyn** visar alla program som kördes på klustret. Klicka på namnet på programmet som du vill se mer information.

    ![Programinformation](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)
4. Om du hovrar på jobbet diagrammet visar grundläggande körs Jobbinformationen. Visas klickar på jobbdiagram steg diagram och information som genereras av varje jobb.

    ![Steget jobbinformation](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

5. Vanliga loggar, inklusive drivrutinen Stderr, drivrutinen Stdout och Directory information visas i den **loggen** fliken.

    ![Logginformation](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)
6. Du kan också öppna Spark historik UI och YARN-Användargränssnittet (på programnivå) genom att klicka på respektive hyperlänken längst upp i fönstret.

### <a name="access-the-storage-container-for-the-cluster"></a>Åtkomst till vilken lagringsbehållare som klustret
1. I Azure Explorer expanderar den **HDInsight** rotnoden att se en lista över HDInsight Spark-kluster som är tillgängliga.
2. Expandera klusternamnet finns i lagringskontot och standardbehållare för lagring för klustret.
   
    ![Storage-konto och standard lagringsbehållare](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-5.png)
3. Klicka på lagring behållarens namn som är associerade med klustret. I den högra rutan, dubbelklickar du på den **HVACOut** mapp. Öppna en av de **del -** filer för att se utdata från programmet.

### <a name="access-the-spark-history-server"></a>Åtkomst till servern för Spark-historik
1. Högerklicka på klusternamnet Spark i Azure Explorer och markera **öppna Spark historik UI**. När du uppmanas ange administratörsautentiseringsuppgifterna för klustret. Du måste ange dessa vid etablering av klustret.
2. I instrumentpanelen Spark historik server använder du namnet på programmet för att söka efter programmet just avslutats körs. I föregående kod, ange namnet på programmet med `val conf = new SparkConf().setAppName("MyClusterApp")`. Därför programnamnet Spark har **MyClusterApp**.

### <a name="start-the-ambari-portal"></a>Starta Ambari-portal
1. Högerklicka på klusternamnet Spark i Azure Explorer och markera **öppna klustret hanteringsportalen (Ambari)**. 
2. När du uppmanas ange administratörsautentiseringsuppgifterna för klustret. Du måste ange dessa vid etablering av klustret.

### <a name="manage-azure-subscriptions"></a>Hantera Azure-prenumerationer
Som standard visar HDInsight-verktyg i Azure Toolkit för Eclipse Spark-kluster från alla dina Azure-prenumerationer. Om det behövs kan du ange prenumerationer som du vill ha åtkomst till klustret. 

1. I Azure Explorer högerklickar du på den **Azure** rot noden och klicka sedan på **hantera prenumerationer**. 
2. Avmarkera kryssrutorna för den prenumeration som du inte vill få åtkomst till och klicka sedan på i dialogrutan **Stäng**. Du kan också klicka på **logga ut** om du vill logga ut från din Azure-prenumeration.

## <a name="run-a-spark-scala-application-locally"></a>Köra ett program med Spark Scala lokalt
Du kan använda HDInsight Tools i Azure Toolkit för Eclipse för att köra Spark Scala program lokalt på din arbetsstation. Normalt programmen behöver inte åtkomst till resurser i klustret som en lagringsbehållare och du kan köra och testa dem lokalt.

### <a name="prerequisite"></a>Krav
När du kör programmet lokalt Spark Scala på en Windows-dator, kan du få ett undantag som beskrivs i [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356). Det här undantaget inträffar eftersom **WinUtils.exe** saknas i Windows. 

För att lösa det här felet måste du [ladda ned den körbara filen](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) till en plats som **C:\WinUtils\bin**. Sedan måste du lägga till miljövariabeln **HADOOP_HOME** och ange värdet för variabeln för att **C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>Kör ett lokalt Spark Scala-program
1. Starta Eclipse och skapa ett projekt. I den **nytt projekt** dialogrutan följande alternativ och klickar sedan på **nästa**.
   
   * I den vänstra rutan, Välj **HDInsight**.
   * I den högra rutan, Välj **Spark på HDInsight lokala kör sampel (Scala)**.

    ![Dialogrutan Nytt projekt](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
2. Följ steg 3 till 6 för att ge projektinformationen, från det tidigare avsnittet [Ställ in ett Spark Scala-projekt för ett HDInsight Spark-kluster](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).
3. Mallen lägger till en exempelkod (**LogQuery**) under den **src** mapp som du kan köra lokalt på datorn.
   
    ![Platsen för LogQuery](./media/hdinsight-apache-spark-eclipse-tool-plugin/local-app.png)
4. Högerklicka på den **LogQuery** program, peka på **kör som**, och klicka sedan på **1 Scala program**. Du kommer att se utdata som liknar detta i den **konsolen** längst ned:
   
   ![Spark programmet lokala kör resultat](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
För att skicka ett program till Azure Data Lake Store, Välj **interaktiv** läge under processen för Azure-inloggning. Om du väljer **automatisk** läge, du får ett felmeddelande.

![Interaktiv inloggning](./media/hdinsight-apache-spark-eclipse-tool-plugin/interactive-authentication.png)

Nu ska löste vi problemet. Du kan välja ett Azure Data Lake-kluster att skicka ditt program med valfri metod för inloggning.

## <a name="feedback-and-known-issues"></a>Feedback och kända problem
För närvarande kan stöds visa Spark utdata direkt inte.

Om du har förslag eller feedback eller om du stöter på problem när du använder det här verktyget gärna skicka ett e-postmeddelande på hdivstool@microsoft.com.

## <a name="seealso"></a>Se även
* [Översikt: Apache Spark i Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenarier
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](hdinsight-apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för att förutsäga resultatet av en livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid](hdinsight-apache-spark-eventhub-streaming.md)
* [Webbplatslogganalys med Spark i HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Skapa och köra program
* [Skapa ett fristående program med hjälp av Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Verktyg och tillägg
* [Använda Azure Toolkit för IntelliJ för att skapa och skicka Spark Scala-program](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Använda Azure Toolkit för IntelliJ för att felsöka Spark-program via fjärranslutning via VPN](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använda Azure Toolkit för IntelliJ för att felsöka Spark-program via fjärranslutning via SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Använda HDInsight Tools för IntelliJ med Hortonworks Sandbox](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta till ett HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Hantera resurser
* [Hantera resurser för Apache Spark-klustret i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)

