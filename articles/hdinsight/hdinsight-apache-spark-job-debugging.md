---
title: "aaaDebug Apache Spark jobb som körs på Azure HDInsight | Microsoft Docs"
description: "Använd YARN-Användargränssnittet, Spark UI och Spark tidigare server tootrack och felsöka jobb som körs på ett Spark-kluster i Azure HDInsight"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 59af05a7-2bd9-44b0-b55f-2438d294198b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 33d352a5773920735aa4e5e8532b78122f381377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Felsöka Apache Spark-jobb som körs på Azure HDInsight

I den här artikeln du lära dig hur tootrack och felsökningsloggar Väck jobb som körs på HDInsight-kluster med hello YARN-Användargränssnittet, Spark-Gränssnittet och hello Spark historik Server. För den här artikeln har vi startar ett Spark-jobb med hjälp av en bärbar dator som är tillgängliga med hello Spark-kluster **Maskininlärning: förutsägbar analys på mat inspektion data med hjälp av MLLib**. Du kan använda hello steg nedan tootrack ett program som du skickas med någon annan metod, till exempel **spark-skicka**.

## <a name="prerequisites"></a>Krav
Du måste ha hello följande:

* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Du bör ha startat körs hello anteckningsboken  **[Maskininlärning: förutsägbar analys på mat inspektion data med hjälp av MLLib](hdinsight-apache-spark-machine-learning-mllib-ipython.md)**. Anvisningar för hur toorun anteckningsboken Följ hello länk.  

## <a name="track-an-application-in-hello-yarn-ui"></a>Spåra ett program i hello YARN-Användargränssnittet
1. Starta hello YARN-Användargränssnittet. Hello kluster-bladet, klickar du på **Klusterinstrumentpanel**, och klicka sedan på **YARN**.
   
    ![Starta YARN-Användargränssnittet](./media/hdinsight-apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]
   > Alternativt kan du starta hello YARN-Användargränssnittet från hello Ambari UI. toolaunch hello Ambari UI från hello kluster-bladet, klickar du på **Klusterinstrumentpanel**, och klicka sedan på **instrumentpanelen för HDInsight-klustret**. Hello Ambari UI, klicka på **YARN**, klickar du på **snabblänkar**, klicka på hello active resource manager och klicka sedan på **ResourceManager UI**.    
   > 
   > 
2. Eftersom du startade hello Spark jobb med hjälp av Jupyter-anteckningsböcker hello program har hello namn **remotesparkmagics** (detta är hello namn för alla program som startas från hello bärbara datorer). Klicka på hello program-ID mot hello programmet namnet tooget mer information om hello jobb. Detta startar hello programvy.
   
    ![Hitta Spark-program-ID](./media/hdinsight-apache-spark-job-debugging/find-application-id.png)
   
    Sådana program som startas från hello Jupyter-anteckningsböcker hello status är alltid **kör** tills du avslutar hello bärbar dator.
3. Hello programmet vyn detaljnivån du ytterligare toofind ut hello behållare som är associerade med programmet hello och hello-loggar (stdout/stderr). Du kan också starta hello Spark-Användargränssnittet genom att klicka på Länka motsvarande toohello hello **spårning URL**, enligt nedan. 
   
    ![Hämta loggar för behållaren](./media/hdinsight-apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-hello-spark-ui"></a>Spåra ett program i hello Spark UI
I hello Spark UI, kan du gå till hello Spark jobb som skapas av hello-program som du startade tidigare.

1. toolaunch hello Spark-Gränssnittet från hello programvy Klicka hello länken mot hello **spårning URL**som visas i hello skärmdump ovan. Du kan se alla hello Spark jobb som startats av hello-program som körs i hello Jupyter-anteckningsbok.
   
    ![Visa Spark jobb](./media/hdinsight-apache-spark-job-debugging/view-spark-jobs.png)
2. Klicka på hello **Executors** fliken information om toosee bearbetning och lagring för varje utförare. Du kan också hämta hello anropsstacken genom att klicka på hello **tråd Dump** länk.
   
    ![Visa Spark executors](./media/hdinsight-apache-spark-job-debugging/view-spark-executors.png)
3. Klicka på hello **steg** fliken toosee hello steg som är associerade med programmet hello.
   
    ![Visa Spark faser](./media/hdinsight-apache-spark-job-debugging/view-spark-stages.png)
   
    Varje steg kan ha flera uppgifter som du kan visa statistik för körning, som visas nedan.
   
    ![Visa Spark faser](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-details.png) 
4. Du kan starta DAG visualiseringen från hello steg på sidan. Expandera hello **DAG visualiseringen** länka hello överst i hello sida som visas nedan.
   
    ![Visa Spark steg DAG visualiseringen](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    Representerar hello olika faser i programmet hello DAG eller direkt Aclyic diagram. Varje ruta i hello graph representerar en Spark-åtgärd som anropas från hello program.
5. Du kan starta hello tidslinjevyn för programmet från hello steg på sidan. Expandera hello **händelse tidslinjen** länka hello överst i hello sida som visas nedan.
   
    ![Visa Spark steg händelse tidslinjen](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    Detta visar hello Spark händelser i hello form av en tidslinje. hello tidslinjevyn är tillgänglig på tre nivåer över jobb inom ett jobb, och ett steg. hello bilden ovan avbildar hello tidslinjevyn för ett visst stadium.
   
   > [!TIP]
   > Om du väljer hello **aktivera** kryssrutan, du kan rulla åt vänster och höger över hello tidslinjevyn.
   > 
   > 
6. Andra flikar i hello Spark Användargränssnittet innehåller användbar information om samt hello Spark-instans.
   
   * Fliken lagring – om programmet skapar en RDDs hittar du information om de på fliken för hello lagring.
   * Fliken miljö - den här fliken ger mycket användbar information om din Spark-instans, till exempel hello 
     * Scala version
     * Händelseloggen katalog som är associerad med hello-kluster
     * Antal kärnor utförare för hello program
     * O.s.v.

## <a name="find-information-about-completed-jobs-using-hello-spark-history-server"></a>Hitta information om slutförda jobb med hjälp av hello Spark historik Server
När ett jobb har slutförts beständig hello information om hello jobb i hello Spark historik Server.

1. toolaunch hello Spark historik Server från hello kluster-bladet, klickar du på **Klusterinstrumentpanel**, och klicka sedan på **Spark historik Server**.
   
    ![Starta Spark historik Server](./media/hdinsight-apache-spark-job-debugging/launch-spark-history-server.png)
   
   > [!TIP]
   > Alternativt kan du starta hello Spark historik Server UI från hello Ambari UI. toolaunch hello Ambari UI från hello kluster-bladet, klickar du på **Klusterinstrumentpanel**, och klicka sedan på **instrumentpanelen för HDInsight-klustret**. Hello Ambari UI, klicka på **Spark**, klickar du på **snabblänkar**, och klicka sedan på **Spark historik Server UI**.
   > 
   > 
2. Alla hello slutförts program i listan visas. Klicka på ett program-ID toodrill nedåt till ett program för mer information.
   
    ![Starta Spark historik Server](./media/hdinsight-apache-spark-job-debugging/view-completed-applications.png)

## <a name="see-also"></a>Se även
*  [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

### <a name="for-data-analysts"></a>För dataanalytiker

* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Webbplatslogganalys med Spark i HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [Application Insight-telemetridataanalys i HDInsight](hdinsight-spark-analyze-application-insight-logs.md)
* [Använda Caffe för distribuerade djup learning på Azure HDInsight Spark](hdinsight-deep-learning-caffe-spark.md)

### <a name="for-spark-developers"></a>För Spark-utvecklare

* [Skapa ett fristående program med hjälp av Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-program](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid](hdinsight-apache-spark-eventhub-streaming.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)


