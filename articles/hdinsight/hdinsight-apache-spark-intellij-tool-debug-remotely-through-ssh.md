---
title: "Azure Toolkit för IntelliJ: felsöka Spark-program via fjärranslutning via SSH | Microsoft Docs"
description: "Stegvisa anvisningar om hur du använder HDInsight-verktyg i Azure Toolkit för IntelliJ för att felsöka program på HDInsight-kluster via SSH"
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
ms.openlocfilehash: 19053e31d6eb097bc91a04ef9c6af5772aaa16da
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="debug-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a><span data-ttu-id="8f28b-104">Felsöka Spark-program på ett HDInsight-kluster med Azure Toolkit för IntelliJ via SSH</span><span class="sxs-lookup"><span data-stu-id="8f28b-104">Debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH</span></span>

<span data-ttu-id="8f28b-105">Den här artikeln innehåller stegvisa anvisningar om hur du använder HDInsight-verktyg i Azure Toolkit för IntelliJ för att felsöka program via fjärranslutning på ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8f28b-105">This article provides step-by-step guidance on how to use HDInsight Tools in Azure Toolkit for IntelliJ to debug applications remotely on an HDInsight cluster.</span></span> <span data-ttu-id="8f28b-106">Om du vill felsöka ditt projekt, du kan också visa den [felsöka HDInsight Spark-program med Azure Toolkit för IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) video.</span><span class="sxs-lookup"><span data-stu-id="8f28b-106">To debug your project, you can also view the [Debug HDInsight Spark applications with Azure Toolkit for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) video.</span></span>

<span data-ttu-id="8f28b-107">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="8f28b-107">**Prerequisites**</span></span>

* <span data-ttu-id="8f28b-108">**HDInsight-verktyg i Azure Toolkit för IntelliJ**.</span><span class="sxs-lookup"><span data-stu-id="8f28b-108">**HDInsight Tools in Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="8f28b-109">Det här verktyget ingår i Azure Toolkit för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="8f28b-109">This tool is part of Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="8f28b-110">Mer information finns i [installera Azure Toolkit för IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).</span><span class="sxs-lookup"><span data-stu-id="8f28b-110">For more information, see [Install Azure Toolkit for IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).</span></span>
* <span data-ttu-id="8f28b-111">**Azure Toolkit för IntelliJ**.</span><span class="sxs-lookup"><span data-stu-id="8f28b-111">**Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="8f28b-112">Använd dessa verktyg för att skapa Spark-program för ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8f28b-112">Use this toolkit to create Spark applications for an HDInsight cluster.</span></span> <span data-ttu-id="8f28b-113">Mer information, följ instruktionerna i [Använd Azure Toolkit för IntelliJ till att skapa Spark-program för ett HDInsight-kluster](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).</span><span class="sxs-lookup"><span data-stu-id="8f28b-113">For more information, follow the instructions in [Use Azure Toolkit for IntelliJ to create Spark applications for an HDInsight cluster](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).</span></span>
* <span data-ttu-id="8f28b-114">**HDInsight SSH-tjänsten med hantering av användarnamn och lösenord**.</span><span class="sxs-lookup"><span data-stu-id="8f28b-114">**HDInsight SSH service with username and password management**.</span></span> <span data-ttu-id="8f28b-115">Mer information finns i [Anslut till HDInsight (Hadoop) med hjälp av SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) och [använda SSH tunnlar för att komma åt Ambari web UI, jobbhistorik, NameNode, Oozie och andra webb-användargränssnitt](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel).</span><span class="sxs-lookup"><span data-stu-id="8f28b-115">For more information, see [Connect to HDInsight (Hadoop) by using SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) and [Use SSH tunneling to access Ambari web UI, JobHistory, NameNode, Oozie, and other web UIs](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel).</span></span> 
 

## <a name="create-a-spark-scala-application-and-configure-it-for-remote-debugging"></a><span data-ttu-id="8f28b-116">Skapa ett Spark Scala-program och konfigurera den för fjärrfelsökning</span><span class="sxs-lookup"><span data-stu-id="8f28b-116">Create a Spark Scala application and configure it for remote debugging</span></span>

1. <span data-ttu-id="8f28b-117">Starta IntelliJ IDEA och sedan skapa ett projekt.</span><span class="sxs-lookup"><span data-stu-id="8f28b-117">Start IntelliJ IDEA, and then create a project.</span></span> <span data-ttu-id="8f28b-118">I den **nytt projekt** dialogrutan Gör följande:</span><span class="sxs-lookup"><span data-stu-id="8f28b-118">In the **New Project** dialog box, do the following:</span></span>

   <span data-ttu-id="8f28b-119">a.</span><span class="sxs-lookup"><span data-stu-id="8f28b-119">a.</span></span> <span data-ttu-id="8f28b-120">Välj **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="8f28b-120">Select **HDInsight**.</span></span> 

   <span data-ttu-id="8f28b-121">b.</span><span class="sxs-lookup"><span data-stu-id="8f28b-121">b.</span></span> <span data-ttu-id="8f28b-122">Välj en mall för Java eller Scala baserat på dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="8f28b-122">Select a Java or Scala template based on your preference.</span></span> <span data-ttu-id="8f28b-123">Välj mellan följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="8f28b-123">Select between the following options:</span></span>

      - <span data-ttu-id="8f28b-124">**Spark i HDInsight (Scala)**</span><span class="sxs-lookup"><span data-stu-id="8f28b-124">**Spark on HDInsight (Scala)**</span></span>

      - <span data-ttu-id="8f28b-125">**Spark i HDInsight (Java)**</span><span class="sxs-lookup"><span data-stu-id="8f28b-125">**Spark on HDInsight (Java)**</span></span>

      - <span data-ttu-id="8f28b-126">**Spark i HDInsight-klustret kör exempel (Scala)**</span><span class="sxs-lookup"><span data-stu-id="8f28b-126">**Spark on HDInsight Cluster Run Sample (Scala)**</span></span>

      <span data-ttu-id="8f28b-127">Det här exemplet används en **Spark på HDInsight-klustret kör sampel (Scala)** mall.</span><span class="sxs-lookup"><span data-stu-id="8f28b-127">This example uses a **Spark on HDInsight Cluster Run Sample (Scala)** template.</span></span>

   <span data-ttu-id="8f28b-128">c.</span><span class="sxs-lookup"><span data-stu-id="8f28b-128">c.</span></span> <span data-ttu-id="8f28b-129">I den **Build verktyget** väljer du något av följande enligt dina behov:</span><span class="sxs-lookup"><span data-stu-id="8f28b-129">In the **Build tool** list, select either of the following, according to your need:</span></span>

      - <span data-ttu-id="8f28b-130">**Maven**, Scala skapa projekt guiden support</span><span class="sxs-lookup"><span data-stu-id="8f28b-130">**Maven**, for Scala project-creation wizard support</span></span>

      -  <span data-ttu-id="8f28b-131">**Segregerade Barlasttankar**, för att hantera beroenden och skapa för Scala-projekt</span><span class="sxs-lookup"><span data-stu-id="8f28b-131">**SBT**, for managing the dependencies and building for the Scala project</span></span> 

      ![Skapa ett debug-projekt](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-create-projectfor-debug-remotely.png)

   <span data-ttu-id="8f28b-133">d.</span><span class="sxs-lookup"><span data-stu-id="8f28b-133">d.</span></span> <span data-ttu-id="8f28b-134">Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8f28b-134">Select **Next**.</span></span>     
 
3. <span data-ttu-id="8f28b-135">I nästa **nytt projekt** fönster, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="8f28b-135">In the next **New Project** window, do the following:</span></span>

   ![Välj Spark SDK](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-new-project.png)

   <span data-ttu-id="8f28b-137">a.</span><span class="sxs-lookup"><span data-stu-id="8f28b-137">a.</span></span> <span data-ttu-id="8f28b-138">Ange ett projektnamn och projektets plats.</span><span class="sxs-lookup"><span data-stu-id="8f28b-138">Enter a project name and project location.</span></span>

   <span data-ttu-id="8f28b-139">b.</span><span class="sxs-lookup"><span data-stu-id="8f28b-139">b.</span></span> <span data-ttu-id="8f28b-140">I den **projekt SDK** listrutan, Välj **Java 1.8** för **Väck 2.x** kluster eller välj **Java 1.7** för **Väck 1.x**  klustret.</span><span class="sxs-lookup"><span data-stu-id="8f28b-140">In the **Project SDK** drop-down list, select **Java 1.8** for **Spark 2.x** cluster or select **Java 1.7** for **Spark 1.x** cluster.</span></span>

   <span data-ttu-id="8f28b-141">c.</span><span class="sxs-lookup"><span data-stu-id="8f28b-141">c.</span></span> <span data-ttu-id="8f28b-142">I den **Spark Version** listrutan Scala projektguiden integrerar rätt version för Spark SDK och Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="8f28b-142">In the **Spark Version** drop-down list, the Scala project creation wizard integrates the correct version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="8f28b-143">Om den spark-kluster av versionen är äldre än 2.0 väljer **Väck 1.x**.</span><span class="sxs-lookup"><span data-stu-id="8f28b-143">If the spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="8f28b-144">Annars väljer **Väck 2.x.**</span><span class="sxs-lookup"><span data-stu-id="8f28b-144">Otherwise, select **Spark 2.x.**</span></span> <span data-ttu-id="8f28b-145">Det här exemplet används **Spark punkt 2.0.2 (Scala 2.11.8)**.</span><span class="sxs-lookup"><span data-stu-id="8f28b-145">This example uses **Spark 2.0.2 (Scala 2.11.8)**.</span></span>

   <span data-ttu-id="8f28b-146">d.</span><span class="sxs-lookup"><span data-stu-id="8f28b-146">d.</span></span> <span data-ttu-id="8f28b-147">Välj **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="8f28b-147">Select **Finish**.</span></span>

4. <span data-ttu-id="8f28b-148">Välj **src** > **huvudsakliga** > **scala** att öppna din kod i projektet.</span><span class="sxs-lookup"><span data-stu-id="8f28b-148">Select **src** > **main** > **scala** to open your code in the project.</span></span> <span data-ttu-id="8f28b-149">Det här exemplet används den **SparkCore_wasbloTest** skript.</span><span class="sxs-lookup"><span data-stu-id="8f28b-149">This example uses the **SparkCore_wasbloTest** script.</span></span>

5. <span data-ttu-id="8f28b-150">Att få åtkomst till den **redigera konfigurationer** -menyn väljer du ikonen i det övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="8f28b-150">To access the **Edit Configurations** menu, select the icon in the upper-right corner.</span></span> <span data-ttu-id="8f28b-151">Du kan skapa eller redigera konfigurationer för fjärrfelsökning från den här menyn.</span><span class="sxs-lookup"><span data-stu-id="8f28b-151">From this menu, you can create or edit the configurations for remote debugging.</span></span>

   ![Redigera konfigurationer](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-edit-configurations.png) 

6. <span data-ttu-id="8f28b-153">I den **kör/Debug konfigurationer** dialogrutan Välj på plustecknet (**+**).</span><span class="sxs-lookup"><span data-stu-id="8f28b-153">In the **Run/Debug Configurations** dialog box, select the plus sign (**+**).</span></span> <span data-ttu-id="8f28b-154">Välj sedan den **skicka Spark jobbet** alternativet.</span><span class="sxs-lookup"><span data-stu-id="8f28b-154">Then select the **Submit Spark Job** option.</span></span>

   ![Lägg till ny konfiguration](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-add-new-Configuration.png)
7. <span data-ttu-id="8f28b-156">Ange information för **namn**, **Spark-kluster**, och **Main klassnamn**.</span><span class="sxs-lookup"><span data-stu-id="8f28b-156">Enter information for **Name**, **Spark cluster**, and **Main class name**.</span></span> <span data-ttu-id="8f28b-157">Välj sedan **avancerad konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="8f28b-157">Then select **Advanced configuration**.</span></span> 

   ![Kör debug-konfigurationer](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-run-debug-configurations.png)

8. <span data-ttu-id="8f28b-159">I den **Spark skicka avancerad konfiguration** dialogrutan **aktivera Spark remote debug**.</span><span class="sxs-lookup"><span data-stu-id="8f28b-159">In the **Spark Submission Advanced Configuration** dialog box, select **Enable Spark remote debug**.</span></span> <span data-ttu-id="8f28b-160">Ange SSH-användarnamn och ange ett lösenord eller Använd en fil för privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="8f28b-160">Enter the SSH username, and then enter a password or use a private key file.</span></span> <span data-ttu-id="8f28b-161">Välj för att spara konfigurationen **OK**.</span><span class="sxs-lookup"><span data-stu-id="8f28b-161">To save the configuration, select **OK**.</span></span>

   ![Aktivera remote Spark-felsökning](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-enable-spark-remote-debug.png)

9. <span data-ttu-id="8f28b-163">Konfiguration sparas nu med det namn du angett.</span><span class="sxs-lookup"><span data-stu-id="8f28b-163">The configuration is now saved with the name you provided.</span></span> <span data-ttu-id="8f28b-164">Välj namnet på konfigurationens om du vill visa konfigurationsinformationen.</span><span class="sxs-lookup"><span data-stu-id="8f28b-164">To view the configuration details, select the configuration name.</span></span> <span data-ttu-id="8f28b-165">Om du vill göra ändringar, Välj **redigera konfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="8f28b-165">To make changes, select **Edit Configurations**.</span></span> 

10. <span data-ttu-id="8f28b-166">När du har slutfört konfigurationsinställningarna för som kan du köra projektet mot fjärrklustret eller utföra fjärrfelsökning.</span><span class="sxs-lookup"><span data-stu-id="8f28b-166">After you complete the configurations settings, you can run the project against the remote cluster or perform remote debugging.</span></span>

## <a name="learn-how-to-perform-remote-debugging"></a><span data-ttu-id="8f28b-167">Lär dig hur du utför fjärrfelsökning</span><span class="sxs-lookup"><span data-stu-id="8f28b-167">Learn how to perform remote debugging</span></span>
### <a name="scenario-1-perform-remote-run"></a><span data-ttu-id="8f28b-168">Scenario 1: Utföra remote kör</span><span class="sxs-lookup"><span data-stu-id="8f28b-168">Scenario 1: Perform remote run</span></span>

<span data-ttu-id="8f28b-169">I det här avsnittet hur vi du felsöker drivrutiner och executors.</span><span class="sxs-lookup"><span data-stu-id="8f28b-169">In this section, we show you how to debug drivers and executors.</span></span>

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
        /** Tracks the total query count and number of aggregate bytes for a particular group. */
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


1. <span data-ttu-id="8f28b-170">Ställ in senaste punkter och välj sedan den **felsöka** ikon.</span><span class="sxs-lookup"><span data-stu-id="8f28b-170">Set up breaking points, and then select the **Debug** icon.</span></span>

   ![Välj debug-ikon](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-icon.png)

2. <span data-ttu-id="8f28b-172">När programkörningen når den senaste punkten, visas en **drivrutinen** fliken och två **utföraren** flikarna i den **felsökare** fönstret.</span><span class="sxs-lookup"><span data-stu-id="8f28b-172">When the program execution reaches the breaking point, you see a **Driver** tab and two **Executor** tabs in the **Debugger** pane.</span></span> <span data-ttu-id="8f28b-173">Välj den **återuppta programmet** ikon för att fortsätta att köra kod, som sedan når nästa brytpunkt och fokuserar på motsvarande **utföraren** fliken. Du kan granska körningsloggar på motsvarande **konsolen** fliken.</span><span class="sxs-lookup"><span data-stu-id="8f28b-173">Select the **Resume Program** icon to continue running the code, which then reaches the next breakpoint and focuses on the corresponding **Executor** tab. You can review the execution logs on the corresponding **Console** tab.</span></span>

   ![Fliken felsökning](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debugger-tab.png)

### <a name="scenario-2-perform-remote-debugging-and-bug-fixing"></a><span data-ttu-id="8f28b-175">Scenario 2: Utföra fjärrfelsökning och åtgärda fel</span><span class="sxs-lookup"><span data-stu-id="8f28b-175">Scenario 2: Perform remote debugging and bug fixing</span></span>
<span data-ttu-id="8f28b-176">I det här avsnittet hur vi du uppdatera dynamiskt variabelvärdet med IntelliJ felsökning kapaciteten för en enkel lösning.</span><span class="sxs-lookup"><span data-stu-id="8f28b-176">In this section, we show you how to dynamically update the variable value by using the IntelliJ debugging capability for a simple fix.</span></span> <span data-ttu-id="8f28b-177">I följande kodexempel genereras ett undantag eftersom filen redan finns.</span><span class="sxs-lookup"><span data-stu-id="8f28b-177">In the following code example, an exception is thrown because the target file already exists.</span></span>
  
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext

        object SparkCore_WasbIOTest {
          def main(arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkCore_WasbIOTest")
            val sc = new SparkContext(conf)
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            // Find the rows that have only one digit in the sixth column.
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


#### <a name="to-perform-remote-debugging-and-bug-fixing"></a><span data-ttu-id="8f28b-178">Att utföra fjärrfelsökning och åtgärda fel</span><span class="sxs-lookup"><span data-stu-id="8f28b-178">To perform remote debugging and bug fixing</span></span>
1. <span data-ttu-id="8f28b-179">Ställa in två bryta punkter och välj sedan den **felsöka** ikon för att starta felsökning fjärrprocessen.</span><span class="sxs-lookup"><span data-stu-id="8f28b-179">Set up two breaking points, and then select the **Debug** icon to start the remote debugging process.</span></span>

2. <span data-ttu-id="8f28b-180">Koden stannar vid den första platsen för senaste och parametern och variabel information visas i den **variabler** fönstret.</span><span class="sxs-lookup"><span data-stu-id="8f28b-180">The code stops at the first breaking point, and the parameter and variable information are shown in the **Variables** pane.</span></span> 

3. <span data-ttu-id="8f28b-181">Välj den **återuppta programmet** ikon för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="8f28b-181">Select the **Resume Program** icon to continue.</span></span> <span data-ttu-id="8f28b-182">Koden stannar vid den andra punkten.</span><span class="sxs-lookup"><span data-stu-id="8f28b-182">The code stops at the second point.</span></span> <span data-ttu-id="8f28b-183">Det är undantagsfel som förväntat.</span><span class="sxs-lookup"><span data-stu-id="8f28b-183">The exception is caught as expected.</span></span>

  ![Utlösa fel](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-throw-error.png) 

4. <span data-ttu-id="8f28b-185">Välj den **återuppta programmet** ikonen igen.</span><span class="sxs-lookup"><span data-stu-id="8f28b-185">Select the **Resume Program** icon again.</span></span> <span data-ttu-id="8f28b-186">Den **HDInsight Spark skicka** fönstret visas felet ”jobbet körningen misslyckades”.</span><span class="sxs-lookup"><span data-stu-id="8f28b-186">The **HDInsight Spark Submission** window displays a "job run failed" error.</span></span>

  ![Skicka fel](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-error-submission.png) 

5. <span data-ttu-id="8f28b-188">Om du vill uppdatera dynamiskt variabelns värde med hjälp av IntelliJ felsökning kapaciteten, Välj **felsöka** igen.</span><span class="sxs-lookup"><span data-stu-id="8f28b-188">To dynamically update the variable value by using the IntelliJ debugging capability, select **Debug** again.</span></span> <span data-ttu-id="8f28b-189">Den **variabler** fönstret visas igen.</span><span class="sxs-lookup"><span data-stu-id="8f28b-189">The **Variables** pane appears again.</span></span> 

6. <span data-ttu-id="8f28b-190">Högerklicka på målet på den **felsöka** och välj sedan **ange värde**.</span><span class="sxs-lookup"><span data-stu-id="8f28b-190">Right-click the target on the **Debug** tab, and then select **Set Value**.</span></span> <span data-ttu-id="8f28b-191">Ange sedan ett nytt värde för variabeln.</span><span class="sxs-lookup"><span data-stu-id="8f28b-191">Next, enter a new value for the variable.</span></span> <span data-ttu-id="8f28b-192">Välj sedan **RETUR** att spara värdet.</span><span class="sxs-lookup"><span data-stu-id="8f28b-192">Then select **Enter** to save the value.</span></span> 

  ![Ange värde](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-set-value.png) 

7. <span data-ttu-id="8f28b-194">Välj den **återuppta programmet** ikon för att fortsätta att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="8f28b-194">Select the **Resume Program** icon to continue to run the program.</span></span> <span data-ttu-id="8f28b-195">Nu är har inga undantag inträffat.</span><span class="sxs-lookup"><span data-stu-id="8f28b-195">This time, no exception is caught.</span></span> <span data-ttu-id="8f28b-196">Du kan se att projektet har körts utan undantag.</span><span class="sxs-lookup"><span data-stu-id="8f28b-196">You can see that the project runs successfully without any exceptions.</span></span>

  ![Felsöka utan undantag](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-without-exception.png)

## <span data-ttu-id="8f28b-198"><a name="seealso"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f28b-198"><a name="seealso"></a>Next steps</span></span>
* [<span data-ttu-id="8f28b-199">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f28b-199">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="8f28b-200">Demo</span><span class="sxs-lookup"><span data-stu-id="8f28b-200">Demo</span></span>
* <span data-ttu-id="8f28b-201">Skapa Scala projekt (video): [skapa Spark Scala-program](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="8f28b-201">Create Scala project (video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="8f28b-202">Fjärråtkomst debug (video): [Använd Azure Toolkit för IntelliJ till att felsöka Spark-program på ett HDInsight-kluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="8f28b-202">Remote debug (video): [Use Azure Toolkit for IntelliJ to debug Spark applications remotely on an HDInsight cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="8f28b-203">Scenarier</span><span class="sxs-lookup"><span data-stu-id="8f28b-203">Scenarios</span></span>
* [<span data-ttu-id="8f28b-204">Spark med BI: utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="8f28b-204">Spark with BI: Perform interactive data analysis by using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="8f28b-205">Spark med Machine Learning: använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="8f28b-205">Spark with Machine Learning: Use Spark in HDInsight to analyze building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="8f28b-206">Spark med Machine Learning: Använda Spark i HDInsight för att förutsäga resultatet av en livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="8f28b-206">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="8f28b-207">Spark Streaming: Använda Spark i HDInsight för att skapa realtid strömmade program</span><span class="sxs-lookup"><span data-stu-id="8f28b-207">Spark Streaming: Use Spark in HDInsight to build real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="8f28b-208">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f28b-208">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="8f28b-209">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="8f28b-209">Create and run applications</span></span>
* [<span data-ttu-id="8f28b-210">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="8f28b-210">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="8f28b-211">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="8f28b-211">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="8f28b-212">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="8f28b-212">Tools and extensions</span></span>
* [<span data-ttu-id="8f28b-213">Använda Azure Toolkit för IntelliJ för att skapa Spark-program för ett HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="8f28b-213">Use Azure Toolkit for IntelliJ to create Spark applications for an HDInsight cluster</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="8f28b-214">Använda Azure Toolkit för IntelliJ för att felsöka Spark-program via fjärranslutning via VPN</span><span class="sxs-lookup"><span data-stu-id="8f28b-214">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="8f28b-215">Använda HDInsight Tools för IntelliJ med Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="8f28b-215">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="8f28b-216">Använda HDInsight-verktyg i Azure Toolkit för Eclipse för att skapa Spark-program</span><span class="sxs-lookup"><span data-stu-id="8f28b-216">Use HDInsight Tools in Azure Toolkit for Eclipse to create Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="8f28b-217">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f28b-217">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="8f28b-218">Kernlar som är tillgängliga för Jupyter notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f28b-218">Kernels available for Jupyter notebook in the Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="8f28b-219">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="8f28b-219">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="8f28b-220">Installera Jupyter på datorn och ansluta till ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="8f28b-220">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="8f28b-221">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="8f28b-221">Manage resources</span></span>
* [<span data-ttu-id="8f28b-222">Hantera resurser för Apache Spark-klustret i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f28b-222">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="8f28b-223">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f28b-223">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
