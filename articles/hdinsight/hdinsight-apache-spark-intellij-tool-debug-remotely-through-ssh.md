---
title: "Azure Toolkit för IntelliJ: felsöka Spark-program via fjärranslutning via SSH | Microsoft Docs"
description: "Stegvisa instruktioner om hur toouse HDInsight-verktyg i Azure Toolkit för IntelliJ toodebug program via fjärranslutning på HDInsight-kluster via SSH"
keywords: "Felsöka via fjärranslutning intellij, fjärrfelsökning intellij, ssh, intellij, hdinsight, felsöka intellij, felsökning"
services: hdinsight
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 08/24/2017
ms.author: Jenny Jiang
ms.openlocfilehash: bf3ab9d04c2ff9fcb6bbbdeefb11f55a12fbd845
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a><span data-ttu-id="37e73-104">Felsöka Spark-program på ett HDInsight-kluster med Azure Toolkit för IntelliJ via SSH</span><span class="sxs-lookup"><span data-stu-id="37e73-104">Debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH</span></span>

<span data-ttu-id="37e73-105">Den här artikeln innehåller stegvisa anvisningar om hur toouse HDInsight-verktyg i Azure Toolkit för IntelliJ toodebug program via fjärranslutning på ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="37e73-105">This article provides step-by-step guidance on how toouse HDInsight Tools in Azure Toolkit for IntelliJ toodebug applications remotely on an HDInsight cluster.</span></span> <span data-ttu-id="37e73-106">toodebug ditt projekt, du kan också visa hello [felsöka HDInsight Spark-program med Azure Toolkit för IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) video.</span><span class="sxs-lookup"><span data-stu-id="37e73-106">toodebug your project, you can also view hello [Debug HDInsight Spark applications with Azure Toolkit for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) video.</span></span>

<span data-ttu-id="37e73-107">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="37e73-107">**Prerequisites**</span></span>

* <span data-ttu-id="37e73-108">**HDInsight-verktyg i Azure Toolkit för IntelliJ**.</span><span class="sxs-lookup"><span data-stu-id="37e73-108">**HDInsight Tools in Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="37e73-109">Det här verktyget ingår i Azure Toolkit för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="37e73-109">This tool is part of Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="37e73-110">Mer information finns i [installera Azure Toolkit för IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).</span><span class="sxs-lookup"><span data-stu-id="37e73-110">For more information, see [Install Azure Toolkit for IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).</span></span>
* <span data-ttu-id="37e73-111">**Azure Toolkit för IntelliJ**.</span><span class="sxs-lookup"><span data-stu-id="37e73-111">**Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="37e73-112">Använd den här toolkit toocreate Spark-program för ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="37e73-112">Use this toolkit toocreate Spark applications for an HDInsight cluster.</span></span> <span data-ttu-id="37e73-113">Mer information, följer du anvisningarna för hello i [Använd Azure Toolkit för IntelliJ toocreate Spark-program för ett HDInsight-kluster](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).</span><span class="sxs-lookup"><span data-stu-id="37e73-113">For more information, follow hello instructions in [Use Azure Toolkit for IntelliJ toocreate Spark applications for an HDInsight cluster](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).</span></span>
* <span data-ttu-id="37e73-114">**HDInsight SSH-tjänsten med hantering av användarnamn och lösenord**.</span><span class="sxs-lookup"><span data-stu-id="37e73-114">**HDInsight SSH service with username and password management**.</span></span> <span data-ttu-id="37e73-115">Mer information finns i [ansluta tooHDInsight (Hadoop) med hjälp av SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) och [använda SSH tunnlar tooaccess Ambari web UI, jobbhistorik, NameNode, Oozie och andra webb-användargränssnitt](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel).</span><span class="sxs-lookup"><span data-stu-id="37e73-115">For more information, see [Connect tooHDInsight (Hadoop) by using SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) and [Use SSH tunneling tooaccess Ambari web UI, JobHistory, NameNode, Oozie, and other web UIs](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel).</span></span> 
 

## <a name="create-a-spark-scala-application-and-configure-it-for-remote-debugging"></a><span data-ttu-id="37e73-116">Skapa ett Spark Scala-program och konfigurera den för fjärrfelsökning</span><span class="sxs-lookup"><span data-stu-id="37e73-116">Create a Spark Scala application and configure it for remote debugging</span></span>

1. <span data-ttu-id="37e73-117">Starta IntelliJ IDEA och sedan skapa ett projekt.</span><span class="sxs-lookup"><span data-stu-id="37e73-117">Start IntelliJ IDEA, and then create a project.</span></span> <span data-ttu-id="37e73-118">I hello **nytt projekt** dialogrutan rutan, hello följande:</span><span class="sxs-lookup"><span data-stu-id="37e73-118">In hello **New Project** dialog box, do hello following:</span></span>

   <span data-ttu-id="37e73-119">a.</span><span class="sxs-lookup"><span data-stu-id="37e73-119">a.</span></span> <span data-ttu-id="37e73-120">Välj **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="37e73-120">Select **HDInsight**.</span></span> 

   <span data-ttu-id="37e73-121">b.</span><span class="sxs-lookup"><span data-stu-id="37e73-121">b.</span></span> <span data-ttu-id="37e73-122">Välj en mall för Java eller Scala baserat på dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="37e73-122">Select a Java or Scala template based on your preference.</span></span> <span data-ttu-id="37e73-123">Välj mellan hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="37e73-123">Select between hello following options:</span></span>

      - <span data-ttu-id="37e73-124">**Spark i HDInsight (Scala)**</span><span class="sxs-lookup"><span data-stu-id="37e73-124">**Spark on HDInsight (Scala)**</span></span>

      - <span data-ttu-id="37e73-125">**Spark i HDInsight (Java)**</span><span class="sxs-lookup"><span data-stu-id="37e73-125">**Spark on HDInsight (Java)**</span></span>

      - <span data-ttu-id="37e73-126">**Spark i HDInsight-klustret kör exempel (Scala)**</span><span class="sxs-lookup"><span data-stu-id="37e73-126">**Spark on HDInsight Cluster Run Sample (Scala)**</span></span>

      <span data-ttu-id="37e73-127">Det här exemplet används en **Spark på HDInsight-klustret kör sampel (Scala)** mall.</span><span class="sxs-lookup"><span data-stu-id="37e73-127">This example uses a **Spark on HDInsight Cluster Run Sample (Scala)** template.</span></span>

   <span data-ttu-id="37e73-128">c.</span><span class="sxs-lookup"><span data-stu-id="37e73-128">c.</span></span> <span data-ttu-id="37e73-129">I hello **Build verktyget** väljer du antingen hello följande, enligt tooyour behov:</span><span class="sxs-lookup"><span data-stu-id="37e73-129">In hello **Build tool** list, select either of hello following, according tooyour need:</span></span>

      - <span data-ttu-id="37e73-130">**Maven**, Scala skapa projekt guiden support</span><span class="sxs-lookup"><span data-stu-id="37e73-130">**Maven**, for Scala project-creation wizard support</span></span>

      -  <span data-ttu-id="37e73-131">**Segregerade Barlasttankar**, för att hantera hello beroenden och bygga för hello Scala-projekt</span><span class="sxs-lookup"><span data-stu-id="37e73-131">**SBT**, for managing hello dependencies and building for hello Scala project</span></span> 

      ![Skapa ett debug-projekt](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-create-projectfor-debug-remotely.png)

   <span data-ttu-id="37e73-133">d.</span><span class="sxs-lookup"><span data-stu-id="37e73-133">d.</span></span> <span data-ttu-id="37e73-134">Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="37e73-134">Select **Next**.</span></span>     
 
3. <span data-ttu-id="37e73-135">I hello nästa **nytt projekt** fönstret hello följande:</span><span class="sxs-lookup"><span data-stu-id="37e73-135">In hello next **New Project** window, do hello following:</span></span>

   ![Välj hello Spark SDK](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-new-project.png)

   <span data-ttu-id="37e73-137">a.</span><span class="sxs-lookup"><span data-stu-id="37e73-137">a.</span></span> <span data-ttu-id="37e73-138">Ange ett projektnamn och projektets plats.</span><span class="sxs-lookup"><span data-stu-id="37e73-138">Enter a project name and project location.</span></span>

   <span data-ttu-id="37e73-139">b.</span><span class="sxs-lookup"><span data-stu-id="37e73-139">b.</span></span> <span data-ttu-id="37e73-140">I hello **projekt SDK** listrutan, Välj **Java 1.8** för **Väck 2.x** kluster eller välj **Java 1.7** för **Spark-1. x** klustret.</span><span class="sxs-lookup"><span data-stu-id="37e73-140">In hello **Project SDK** drop-down list, select **Java 1.8** for **Spark 2.x** cluster or select **Java 1.7** for **Spark 1.x** cluster.</span></span>

   <span data-ttu-id="37e73-141">c.</span><span class="sxs-lookup"><span data-stu-id="37e73-141">c.</span></span> <span data-ttu-id="37e73-142">I hello **Spark Version** listrutan hello Scala projektguide integrerar hello rätt version för Spark SDK och Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="37e73-142">In hello **Spark Version** drop-down list, hello Scala project creation wizard integrates hello correct version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="37e73-143">Om hello spark-kluster av version är äldre än 2.0 väljer **Väck 1.x**.</span><span class="sxs-lookup"><span data-stu-id="37e73-143">If hello spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="37e73-144">Annars väljer **Väck 2.x.**</span><span class="sxs-lookup"><span data-stu-id="37e73-144">Otherwise, select **Spark 2.x.**</span></span> <span data-ttu-id="37e73-145">Det här exemplet används **Spark punkt 2.0.2 (Scala 2.11.8)**.</span><span class="sxs-lookup"><span data-stu-id="37e73-145">This example uses **Spark 2.0.2 (Scala 2.11.8)**.</span></span>

   <span data-ttu-id="37e73-146">d.</span><span class="sxs-lookup"><span data-stu-id="37e73-146">d.</span></span> <span data-ttu-id="37e73-147">Välj **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="37e73-147">Select **Finish**.</span></span>

4. <span data-ttu-id="37e73-148">Välj **src** > **huvudsakliga** > **scala** tooopen koden i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="37e73-148">Select **src** > **main** > **scala** tooopen your code in hello project.</span></span> <span data-ttu-id="37e73-149">Det här exemplet används hello **SparkCore_wasbloTest** skript.</span><span class="sxs-lookup"><span data-stu-id="37e73-149">This example uses hello **SparkCore_wasbloTest** script.</span></span>

5. <span data-ttu-id="37e73-150">tooaccess hello **redigera konfigurationer** -menyn, Välj hello-ikonen i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="37e73-150">tooaccess hello **Edit Configurations** menu, select hello icon in hello upper-right corner.</span></span> <span data-ttu-id="37e73-151">Du kan skapa eller redigera hello konfigurationer för fjärrfelsökning från den här menyn.</span><span class="sxs-lookup"><span data-stu-id="37e73-151">From this menu, you can create or edit hello configurations for remote debugging.</span></span>

   ![Redigera konfigurationer](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-edit-configurations.png) 

6. <span data-ttu-id="37e73-153">I hello **kör/Debug konfigurationer** dialogrutan, Välj hello plustecken (**+**).</span><span class="sxs-lookup"><span data-stu-id="37e73-153">In hello **Run/Debug Configurations** dialog box, select hello plus sign (**+**).</span></span> <span data-ttu-id="37e73-154">Välj hello **skicka Spark jobbet** alternativet.</span><span class="sxs-lookup"><span data-stu-id="37e73-154">Then select hello **Submit Spark Job** option.</span></span>

   ![Lägg till ny konfiguration](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-add-new-Configuration.png)
7. <span data-ttu-id="37e73-156">Ange information för **namn**, **Spark-kluster**, och **Main klassnamn**.</span><span class="sxs-lookup"><span data-stu-id="37e73-156">Enter information for **Name**, **Spark cluster**, and **Main class name**.</span></span> <span data-ttu-id="37e73-157">Välj sedan **avancerad konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="37e73-157">Then select **Advanced configuration**.</span></span> 

   ![Kör debug-konfigurationer](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-run-debug-configurations.png)

8. <span data-ttu-id="37e73-159">I hello **Spark skicka avancerad konfiguration** dialogrutan **aktivera Spark remote debug**.</span><span class="sxs-lookup"><span data-stu-id="37e73-159">In hello **Spark Submission Advanced Configuration** dialog box, select **Enable Spark remote debug**.</span></span> <span data-ttu-id="37e73-160">Ange hello SSH-användarnamn och ange ett lösenord eller Använd en fil för privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="37e73-160">Enter hello SSH username, and then enter a password or use a private key file.</span></span> <span data-ttu-id="37e73-161">toosave hello konfiguration, Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="37e73-161">toosave hello configuration, select **OK**.</span></span>

   ![Aktivera remote Spark-felsökning](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-enable-spark-remote-debug.png)

9. <span data-ttu-id="37e73-163">hello konfiguration sparas nu med hello namn du angett.</span><span class="sxs-lookup"><span data-stu-id="37e73-163">hello configuration is now saved with hello name you provided.</span></span> <span data-ttu-id="37e73-164">tooview hello konfigurationsinformation, Välj hello Konfigurationsnamnet.</span><span class="sxs-lookup"><span data-stu-id="37e73-164">tooview hello configuration details, select hello configuration name.</span></span> <span data-ttu-id="37e73-165">Välj toomake ändringar **redigera konfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="37e73-165">toomake changes, select **Edit Configurations**.</span></span> 

10. <span data-ttu-id="37e73-166">När du har slutfört hello konfigurationsinställningar kan du köra hello project mot hello fjärrkluster eller utföra fjärrfelsökning.</span><span class="sxs-lookup"><span data-stu-id="37e73-166">After you complete hello configurations settings, you can run hello project against hello remote cluster or perform remote debugging.</span></span>

## <a name="learn-how-tooperform-remote-debugging"></a><span data-ttu-id="37e73-167">Lär dig hur tooperform fjärrfelsökning</span><span class="sxs-lookup"><span data-stu-id="37e73-167">Learn how tooperform remote debugging</span></span>
### <a name="scenario-1-perform-remote-run"></a><span data-ttu-id="37e73-168">Scenario 1: Utföra remote kör</span><span class="sxs-lookup"><span data-stu-id="37e73-168">Scenario 1: Perform remote run</span></span>

<span data-ttu-id="37e73-169">I det här avsnittet visar vi dig hur toodebug drivrutiner och executors.</span><span class="sxs-lookup"><span data-stu-id="37e73-169">In this section, we show you how toodebug drivers and executors.</span></span>

    import org.apache.spark.{SparkConf, SparkContext}

    object LogQuery {
      val exampleApacheLogs = List(
        """10.10.10.10 - "FRED" [18/Jan/2013:17:56:07 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 315 "http://referall.com/" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.350 "-" - "" 265 923 934 ""
          | 62.24.11.25 images.com 1358492167 - Whatup""".stripMargin.lines.mkString,
        """10.10.10.10 - "FRED" [18/Jan/2013:18:02:37 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 306 "http:/referall.com" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.352 "-" - "" 256 977 988 ""
          | 0 73.23.2.15 images.com 1358492557 - Whatup""".stripMargin.lines.mkString
      )
      def main(args: Array[String]) {
        val sparkconf = new SparkConf().setAppName("Log Query")
        val sc = new SparkContext(sparkconf)
        val dataSet = sc.parallelize(exampleApacheLogs)
        // scalastyle:off
        val apacheLogRegex =
          """^([\d.]+) (\S+) (\S+) \[([\w\d:/]+\s[+\-]\d{4})\] "(.+?)" (\d{3}) ([\d\-]+) "([^"]+)" "([^"]+)".*""".r
        // scalastyle:on
        /** Tracks hello total query count and number of aggregate bytes for a particular group. */
        class Stats(val count: Int, val numBytes: Int) extends Serializable {
          def merge(other: Stats): Stats = new Stats(count + other.count, numBytes + other.numBytes)
          override def toString: String = "bytes=%s\tn=%s".format(numBytes, count)
        }
        def extractKey(line: String): (String, String, String) = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              if (user != "\"-\"") (ip, user, query)
              else (null, null, null)
            case _ => (null, null, null)
          }
        }
        def extractStats(line: String): Stats = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              new Stats(1, bytes.toInt)
            case _ => new Stats(1, 0)
          }
        }
        
        dataSet.map(line => (extractKey(line), extractStats(line)))
          .reduceByKey((a, b) => a.merge(b))
          .collect().foreach{
          case (user, query) => println("%s\t%s".format(user, query))}

        sc.stop()
      }
    }


1. <span data-ttu-id="37e73-170">Ställ in senaste punkter och välj sedan hello **felsöka** ikon.</span><span class="sxs-lookup"><span data-stu-id="37e73-170">Set up breaking points, and then select hello **Debug** icon.</span></span>

   ![Välj hello debug-ikon](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-icon.png)

2. <span data-ttu-id="37e73-172">När hello programkörningen når hello bryta punkt, visas en **drivrutinen** fliken och två **utföraren** flikar i hello **felsökare** fönstret.</span><span class="sxs-lookup"><span data-stu-id="37e73-172">When hello program execution reaches hello breaking point, you see a **Driver** tab and two **Executor** tabs in hello **Debugger** pane.</span></span> <span data-ttu-id="37e73-173">Välj hello **återuppta programmet** ikonen toocontinue kör hello kod, som sedan når hello nästa brytpunkt och fokuserar på hello motsvarande **utföraren** fliken. Du kan granska hello körningsloggar i hello motsvarande **konsolen** fliken.</span><span class="sxs-lookup"><span data-stu-id="37e73-173">Select hello **Resume Program** icon toocontinue running hello code, which then reaches hello next breakpoint and focuses on hello corresponding **Executor** tab. You can review hello execution logs on hello corresponding **Console** tab.</span></span>

   ![Fliken felsökning](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debugger-tab.png)

### <a name="scenario-2-perform-remote-debugging-and-bug-fixing"></a><span data-ttu-id="37e73-175">Scenario 2: Utföra fjärrfelsökning och åtgärda fel</span><span class="sxs-lookup"><span data-stu-id="37e73-175">Scenario 2: Perform remote debugging and bug fixing</span></span>
<span data-ttu-id="37e73-176">I det här avsnittet visar vi hur toodynamically uppdatering hello variabelvärde med hjälp av hello IntelliJ felsökning möjligheten för en enkel lösning.</span><span class="sxs-lookup"><span data-stu-id="37e73-176">In this section, we show you how toodynamically update hello variable value by using hello IntelliJ debugging capability for a simple fix.</span></span> <span data-ttu-id="37e73-177">I följande kodexempel hello, genereras ett undantag eftersom hello målfilen finns redan.</span><span class="sxs-lookup"><span data-stu-id="37e73-177">In hello following code example, an exception is thrown because hello target file already exists.</span></span>
  
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext

        object SparkCore_WasbIOTest {
          def main(arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkCore_WasbIOTest")
            val sc = new SparkContext(conf)
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            // Find hello rows that have only one digit in hello sixth column.
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)

            try {
              var target = "wasb:///HVACout2_testdebug1";
              rdd1.saveAsTextFile(target);
            } catch {
              case ex: Exception => {
                throw ex;
              }
            }
          }
        }


#### <a name="tooperform-remote-debugging-and-bug-fixing"></a><span data-ttu-id="37e73-178">tooperform fjärrfelsökning och åtgärda fel</span><span class="sxs-lookup"><span data-stu-id="37e73-178">tooperform remote debugging and bug fixing</span></span>
1. <span data-ttu-id="37e73-179">Ställa in två bryta punkter och välj sedan hello **felsöka** ikonen toostart hello fjärrfelsökning processen.</span><span class="sxs-lookup"><span data-stu-id="37e73-179">Set up two breaking points, and then select hello **Debug** icon toostart hello remote debugging process.</span></span>

2. <span data-ttu-id="37e73-180">hello kod slutar vid hello första bryta och hello-parametern och variabel information visas i hello **variabler** fönstret.</span><span class="sxs-lookup"><span data-stu-id="37e73-180">hello code stops at hello first breaking point, and hello parameter and variable information are shown in hello **Variables** pane.</span></span> 

3. <span data-ttu-id="37e73-181">Välj hello **återuppta programmet** ikonen toocontinue.</span><span class="sxs-lookup"><span data-stu-id="37e73-181">Select hello **Resume Program** icon toocontinue.</span></span> <span data-ttu-id="37e73-182">hello kod stannar vid hello andra punkten.</span><span class="sxs-lookup"><span data-stu-id="37e73-182">hello code stops at hello second point.</span></span> <span data-ttu-id="37e73-183">hello är undantagsfel som förväntat.</span><span class="sxs-lookup"><span data-stu-id="37e73-183">hello exception is caught as expected.</span></span>

  ![Utlösa fel](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-throw-error.png) 

4. <span data-ttu-id="37e73-185">Välj hello **återuppta programmet** ikonen igen.</span><span class="sxs-lookup"><span data-stu-id="37e73-185">Select hello **Resume Program** icon again.</span></span> <span data-ttu-id="37e73-186">Hej **HDInsight Spark skicka** fönstret visas felet ”jobbet körningen misslyckades”.</span><span class="sxs-lookup"><span data-stu-id="37e73-186">hello **HDInsight Spark Submission** window displays a "job run failed" error.</span></span>

  ![Skicka fel](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-error-submission.png) 

5. <span data-ttu-id="37e73-188">toodynamically uppdatering hello variabelvärde med hjälp av hello IntelliJ felsökning funktion, Välj **felsöka** igen.</span><span class="sxs-lookup"><span data-stu-id="37e73-188">toodynamically update hello variable value by using hello IntelliJ debugging capability, select **Debug** again.</span></span> <span data-ttu-id="37e73-189">Hej **variabler** fönstret visas igen.</span><span class="sxs-lookup"><span data-stu-id="37e73-189">hello **Variables** pane appears again.</span></span> 

6. <span data-ttu-id="37e73-190">Högerklicka på hello mål på hello **felsöka** och välj sedan **ange värde**.</span><span class="sxs-lookup"><span data-stu-id="37e73-190">Right-click hello target on hello **Debug** tab, and then select **Set Value**.</span></span> <span data-ttu-id="37e73-191">Ange sedan ett nytt värde för hello-variabeln.</span><span class="sxs-lookup"><span data-stu-id="37e73-191">Next, enter a new value for hello variable.</span></span> <span data-ttu-id="37e73-192">Välj sedan **RETUR** toosave hello värde.</span><span class="sxs-lookup"><span data-stu-id="37e73-192">Then select **Enter** toosave hello value.</span></span> 

  ![Ange värde](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-set-value.png) 

7. <span data-ttu-id="37e73-194">Välj hello **återuppta programmet** ikonen toocontinue toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="37e73-194">Select hello **Resume Program** icon toocontinue toorun hello program.</span></span> <span data-ttu-id="37e73-195">Nu är har inga undantag inträffat.</span><span class="sxs-lookup"><span data-stu-id="37e73-195">This time, no exception is caught.</span></span> <span data-ttu-id="37e73-196">Du kan se hello projektet har körts utan undantag.</span><span class="sxs-lookup"><span data-stu-id="37e73-196">You can see that hello project runs successfully without any exceptions.</span></span>

  ![Felsöka utan undantag](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-without-exception.png)

## <span data-ttu-id="37e73-198"><a name="seealso"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="37e73-198"><a name="seealso"></a>Next steps</span></span>
* [<span data-ttu-id="37e73-199">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="37e73-199">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="37e73-200">Demo</span><span class="sxs-lookup"><span data-stu-id="37e73-200">Demo</span></span>
* <span data-ttu-id="37e73-201">Skapa Scala projekt (video): [skapa Spark Scala-program](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="37e73-201">Create Scala project (video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="37e73-202">Fjärråtkomst debug (video): [Använd Azure Toolkit för IntelliJ toodebug Spark-program på ett HDInsight-kluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="37e73-202">Remote debug (video): [Use Azure Toolkit for IntelliJ toodebug Spark applications remotely on an HDInsight cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="37e73-203">Scenarier</span><span class="sxs-lookup"><span data-stu-id="37e73-203">Scenarios</span></span>
* [<span data-ttu-id="37e73-204">Spark med BI: utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="37e73-204">Spark with BI: Perform interactive data analysis by using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="37e73-205">Spark med Machine Learning: använda Spark i HDInsight tooanalyze skapa temperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="37e73-205">Spark with Machine Learning: Use Spark in HDInsight tooanalyze building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="37e73-206">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="37e73-206">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="37e73-207">Spark Streaming: Använda Spark i HDInsight toobuild strömmande realtidsprogram</span><span class="sxs-lookup"><span data-stu-id="37e73-207">Spark Streaming: Use Spark in HDInsight toobuild real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="37e73-208">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="37e73-208">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="37e73-209">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="37e73-209">Create and run applications</span></span>
* [<span data-ttu-id="37e73-210">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="37e73-210">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="37e73-211">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="37e73-211">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="37e73-212">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="37e73-212">Tools and extensions</span></span>
* [<span data-ttu-id="37e73-213">Använd Azure Toolkit för IntelliJ toocreate Spark-program för ett HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="37e73-213">Use Azure Toolkit for IntelliJ toocreate Spark applications for an HDInsight cluster</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="37e73-214">Använd Azure Toolkit för IntelliJ toodebug Spark-program via fjärranslutning via VPN</span><span class="sxs-lookup"><span data-stu-id="37e73-214">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="37e73-215">Använda HDInsight Tools för IntelliJ med Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="37e73-215">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="37e73-216">Använda HDInsight Tools i Azure Toolkit för Eclipse toocreate Spark-program</span><span class="sxs-lookup"><span data-stu-id="37e73-216">Use HDInsight Tools in Azure Toolkit for Eclipse toocreate Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="37e73-217">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="37e73-217">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="37e73-218">Kernlar som är tillgängliga för Jupyter notebook i hello Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="37e73-218">Kernels available for Jupyter notebook in hello Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="37e73-219">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="37e73-219">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="37e73-220">Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="37e73-220">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="37e73-221">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="37e73-221">Manage resources</span></span>
* [<span data-ttu-id="37e73-222">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="37e73-222">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="37e73-223">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="37e73-223">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
