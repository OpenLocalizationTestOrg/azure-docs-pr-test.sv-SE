---
title: "Använd Azure Toolkit för IntelliJ med Hortonworks Sandbox | Microsoft Docs"
description: "Lär dig hur du använder HDInsight-verktyg i Azure Toolkit för IntelliJ med Hortonworks Sandbox."
keywords: "hadoop-verktygen hive-fråga, intellij, hortonworks sandbox, azure toolkit för intellij"
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: b587cc9b-a41a-49ac-998f-b54d6c0bdfe0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: c49f185db5a035f70a711bf309b973182d94a2b0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a><span data-ttu-id="81b1f-104">Använda HDInsight Tools för IntelliJ med Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="81b1f-104">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>

<span data-ttu-id="81b1f-105">Lär dig hur du använder HDInsight-verktyg för IntelliJ att utveckla program för Apache Scala och testa programmen på [Hortonworks Sandbox](http://hortonworks.com/products/sandbox/) körs på din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="81b1f-105">Learn how to use HDInsight Tools for IntelliJ to develop Apache Scala applications and then test the applications on [Hortonworks Sandbox](http://hortonworks.com/products/sandbox/) running on your workstation.</span></span> 

<span data-ttu-id="81b1f-106">[IntelliJ IDEA](https://www.jetbrains.com/idea/) är en Java-integrerad utvecklingsmiljö (IDE) för att utveckla programvara.</span><span class="sxs-lookup"><span data-stu-id="81b1f-106">[IntelliJ IDEA](https://www.jetbrains.com/idea/) is a Java-integrated development environment (IDE) for developing computer software.</span></span> <span data-ttu-id="81b1f-107">När du har utvecklats och testa dina program på Hortonworks Sandbox, kan du flytta dem till [Azure HDInsight](hdinsight-hadoop-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="81b1f-107">After you have developed and tested your applications on Hortonworks Sandbox, you can move them to [Azure HDInsight](hdinsight-hadoop-introduction.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81b1f-108">Krav</span><span class="sxs-lookup"><span data-stu-id="81b1f-108">Prerequisites</span></span>

<span data-ttu-id="81b1f-109">Innan du börjar den här vägledningen måste du ha:</span><span class="sxs-lookup"><span data-stu-id="81b1f-109">Before you begin this tutorial, you must have:</span></span>

- <span data-ttu-id="81b1f-110">Hortonworks Data Platform (HDP) 2.4 på Hortonworks Sandbox körs på din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="81b1f-110">Hortonworks Data Platform (HDP) 2.4 on Hortonworks Sandbox running on your local environment.</span></span> <span data-ttu-id="81b1f-111">Om du vill konfigurera HDP finns [komma igång med Hadoop-ekosystemet med en Hadoop sandbox på en virtuell dator](hdinsight-hadoop-emulator-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="81b1f-111">To configure HDP, see [Get started in the Hadoop ecosystem with a Hadoop sandbox on a virtual machine](hdinsight-hadoop-emulator-get-started.md).</span></span> 
    >[!NOTE]
    ><span data-ttu-id="81b1f-112">HDInsight-verktyg för IntelliJ har testats med HDP 2.4.</span><span class="sxs-lookup"><span data-stu-id="81b1f-112">HDInsight Tools for IntelliJ has been tested only with HDP 2.4.</span></span> <span data-ttu-id="81b1f-113">För att få HDP 2.4, expandera **Hortonworks Sandbox Arkiv** på den [Hortonworks Sandbox hämtar plats](http://hortonworks.com/downloads/#sandbox).</span><span class="sxs-lookup"><span data-stu-id="81b1f-113">To get HDP 2.4, expand **Hortonworks Sandbox Archive** on the [Hortonworks Sandbox downloads site](http://hortonworks.com/downloads/#sandbox).</span></span>

- <span data-ttu-id="81b1f-114">[Java Developer Kit (JDK) version 1,8 eller senare](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="81b1f-114">[Java Developer Kit (JDK) version 1.8 or later](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span> <span data-ttu-id="81b1f-115">JDK krävs av Azure-verktyget för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="81b1f-115">JDK is required by the Azure Toolkit for IntelliJ.</span></span>

- <span data-ttu-id="81b1f-116">[IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) med den [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) plugin-program och [Azure Toolkit för IntelliJ](../azure-toolkit-for-intellij.md) plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="81b1f-116">[IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) with the [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) plug-in and the [Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij.md) plug-in.</span></span> <span data-ttu-id="81b1f-117">HDInsight-verktyg för IntelliJ är tillgänglig som en del av Azure-verktygen för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="81b1f-117">HDInsight Tools for IntelliJ is available as a part of the Azure Toolkit for IntelliJ.</span></span> 

  <span data-ttu-id="81b1f-118">Om du vill installera plugin-program gör du följande:</span><span class="sxs-lookup"><span data-stu-id="81b1f-118">To install the plug-ins, do the following:</span></span>

  1. <span data-ttu-id="81b1f-119">Öppna IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="81b1f-119">Open IntelliJ IDEA.</span></span>
  2. <span data-ttu-id="81b1f-120">På den **Välkommen** väljer **konfigurera**, och välj sedan **plugin-program**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-120">On the **Welcome** screen, select **Configure**, and then select **Plugins**.</span></span>
  3. <span data-ttu-id="81b1f-121">Välj **installera JetBrains plugin** i det nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="81b1f-121">Select **Install JetBrains plugin** in the lower-left corner.</span></span>
  4. <span data-ttu-id="81b1f-122">Använda sökfunktionen för att söka efter **Scala**, och välj sedan **installera**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-122">Use the search function to search for **Scala**, and then select **Install**.</span></span>
  5. <span data-ttu-id="81b1f-123">Välj **starta om IntelliJ IDEA** att slutföra installationen.</span><span class="sxs-lookup"><span data-stu-id="81b1f-123">Select **Restart IntelliJ IDEA** to complete the installation.</span></span>
  6. <span data-ttu-id="81b1f-124">Upprepa steg 4 och 5 för att installera den **Azure Toolkit för IntelliJ**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-124">Repeat step 4 and 5 to install the **Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="81b1f-125">Mer information finns i [installerar Azure Toolkit för IntelliJ](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="81b1f-125">For more information, see [Install the Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>

## <a name="create-a-spark-scala-application"></a><span data-ttu-id="81b1f-126">Skapa ett Spark Scala-program</span><span class="sxs-lookup"><span data-stu-id="81b1f-126">Create a Spark Scala application</span></span>

<span data-ttu-id="81b1f-127">I det här avsnittet Skapa en Scala exempelprojektet med IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="81b1f-127">In this section, you create a sample Scala project by using IntelliJ IDEA.</span></span> <span data-ttu-id="81b1f-128">I nästa avsnitt länka du IntelliJ IDEA till Hortonworks Sandbox (emulator) innan du skickar projektet.</span><span class="sxs-lookup"><span data-stu-id="81b1f-128">In the next section, you link IntelliJ IDEA to the Hortonworks Sandbox (emulator) before you submit the project.</span></span>

1. <span data-ttu-id="81b1f-129">Öppna IntelliJ IDEA från din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="81b1f-129">Open IntelliJ IDEA from your workstation.</span></span> <span data-ttu-id="81b1f-130">I den **nytt projekt** dialogrutan Gör följande:</span><span class="sxs-lookup"><span data-stu-id="81b1f-130">In the **New Project** dialog box, do the following:</span></span>

   <span data-ttu-id="81b1f-131">a.</span><span class="sxs-lookup"><span data-stu-id="81b1f-131">a.</span></span> <span data-ttu-id="81b1f-132">Välj **HDInsight** > **Spark i HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-132">Select **HDInsight** > **Spark on HDInsight (Scala)**.</span></span>

   <span data-ttu-id="81b1f-133">b.</span><span class="sxs-lookup"><span data-stu-id="81b1f-133">b.</span></span> <span data-ttu-id="81b1f-134">I den **Build verktyget** väljer du något av följande enligt dina behov:</span><span class="sxs-lookup"><span data-stu-id="81b1f-134">In the **Build tool** list, select either of the following, according to your need:</span></span>

    * <span data-ttu-id="81b1f-135">**Maven**, Scala skapa projekt guiden support</span><span class="sxs-lookup"><span data-stu-id="81b1f-135">**Maven**, for Scala project-creation wizard support</span></span>
    * <span data-ttu-id="81b1f-136">**Segregerade Barlasttankar**, för att hantera beroenden och skapa för Scala-projekt</span><span class="sxs-lookup"><span data-stu-id="81b1f-136">**SBT**, for managing the dependencies and building for the Scala project</span></span>

   ![Dialogrutan Nytt projekt](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. <span data-ttu-id="81b1f-138">Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-138">Select **Next**.</span></span>

3. <span data-ttu-id="81b1f-139">I nästa **nytt projekt** dialogrutan Gör följande:</span><span class="sxs-lookup"><span data-stu-id="81b1f-139">In the next **New Project** dialog box, do the following:</span></span>

    ![Skapa IntelliJ Scala projektegenskaperna](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)

    <span data-ttu-id="81b1f-141">a.</span><span class="sxs-lookup"><span data-stu-id="81b1f-141">a.</span></span> <span data-ttu-id="81b1f-142">I den **projektnamn** ange ett projektnamn.</span><span class="sxs-lookup"><span data-stu-id="81b1f-142">In the **Project name** box, enter a project name.</span></span>

    <span data-ttu-id="81b1f-143">b.</span><span class="sxs-lookup"><span data-stu-id="81b1f-143">b.</span></span> <span data-ttu-id="81b1f-144">I den **projektet plats** ange en projektplats.</span><span class="sxs-lookup"><span data-stu-id="81b1f-144">In the **Project location** box, enter a project location.</span></span>

    <span data-ttu-id="81b1f-145">c.</span><span class="sxs-lookup"><span data-stu-id="81b1f-145">c.</span></span> <span data-ttu-id="81b1f-146">Bredvid den **projekt SDK** listrutan, Välj **ny**väljer **JDK**, och ange sedan mappen Java JDK 1,7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="81b1f-146">Next to the **Project SDK** drop-down list, select **New**, select **JDK**, and then specify the folder of Java JDK version 1.7 or later.</span></span> <span data-ttu-id="81b1f-147">Välj **Java 1.8** för 2.x Spark-kluster eller välj **Java 1.7** för 1.x Spark-klustret.</span><span class="sxs-lookup"><span data-stu-id="81b1f-147">Select **Java 1.8** for the Spark 2.x cluster, or select **Java 1.7** for the Spark 1.x cluster.</span></span> <span data-ttu-id="81b1f-148">Standardsökvägen är C:\Program Files\Java\jdk1.8.x_xxx.</span><span class="sxs-lookup"><span data-stu-id="81b1f-148">The default location is C:\Program Files\Java\jdk1.8.x_xxx.</span></span>

    <span data-ttu-id="81b1f-149">d.</span><span class="sxs-lookup"><span data-stu-id="81b1f-149">d.</span></span> <span data-ttu-id="81b1f-150">I den **Spark version** listrutan Scala guide för att skapa projektet integreras rätt version för Spark SDK och Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="81b1f-150">In the **Spark version** drop-down list, Scala project creation wizard integrates the proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="81b1f-151">Om den Spark-kluster av versionen är äldre än 2.0 väljer **Väck 1.x**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-151">If the Spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="81b1f-152">Annars väljer **Spark2.x**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-152">Otherwise, select **Spark2.x**.</span></span> <span data-ttu-id="81b1f-153">Det här exemplet använder Spark 1.6.2 (Scala 2.10.5).</span><span class="sxs-lookup"><span data-stu-id="81b1f-153">This example uses Spark 1.6.2 (Scala 2.10.5).</span></span> <span data-ttu-id="81b1f-154">Kontrollera att du använder databasen markeras Scala 2.10.x.</span><span class="sxs-lookup"><span data-stu-id="81b1f-154">Make sure that you are using the repository marked Scala 2.10.x.</span></span> <span data-ttu-id="81b1f-155">Använd inte databasen markeras Scala 2.11.x.</span><span class="sxs-lookup"><span data-stu-id="81b1f-155">Do not use the repository marked Scala 2.11.x.</span></span>

4. <span data-ttu-id="81b1f-156">Välj **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-156">Select **Finish**.</span></span>

5. <span data-ttu-id="81b1f-157">Om den **projekt** vyn inte är redan öppen genom att trycka på **Alt + 1** att öppna den.</span><span class="sxs-lookup"><span data-stu-id="81b1f-157">If the **Project** view is not already open, press **Alt+1** to open it.</span></span>

6. <span data-ttu-id="81b1f-158">I **Projektutforskaren**, expandera projektet och välj sedan **src**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-158">In **Project Explorer**, expand the project, and then select **src**.</span></span>

7. <span data-ttu-id="81b1f-159">Högerklicka på **src**, peka på **ny**, och välj sedan **Scala klassen**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-159">Right-click **src**, point to **New**, and then select **Scala class**.</span></span>

8. <span data-ttu-id="81b1f-160">I den **namn** ange ett namn i den **typ** väljer **objekt**, och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-160">In the **Name** box, enter a name, in the **Kind** box, select **Object**, and then select **OK**.</span></span>

    ![Fönstret Skapa nya Scala-klass](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. <span data-ttu-id="81b1f-162">Klistra in följande kod i filen .scala:</span><span class="sxs-lookup"><span data-stu-id="81b1f-162">In the .scala file, paste the following code:</span></span>

        import java.util.Random
        import org.apache.spark.{SparkConf, SparkContext}
        import org.apache.spark.SparkContext._

        /**
        * Usage: GroupByTest [numMappers] [numKVPairs] [valSize] [numReducers]
        */
        object GroupByTest {
            def main(args: Array[String]) {
                val sparkConf = new SparkConf().setAppName("GroupBy Test")
                var numMappers = 3
                var numKVPairs = 10
                var valSize = 10
                var numReducers = 2

                val sc = new SparkContext(sparkConf)

                val pairs1 = sc.parallelize(0 until numMappers, numMappers).flatMap { p =>
                val ranGen = new Random
                var arr1 = new Array[(Int, Array[Byte])](numKVPairs)
                for (i <- 0 until numKVPairs) {
                    val byteArr = new Array[Byte](valSize)
                    ranGen.nextBytes(byteArr)
                    arr1(i) = (ranGen.nextInt(Int.MaxValue), byteArr)
                }
                arr1
                }.cache
                // Enforce that everything has been calculated and in cache
                pairs1.count

                println(pairs1.groupByKey(numReducers).count)
            }
        }

10. <span data-ttu-id="81b1f-163">På den **skapa** väljer du **bygga projektet**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-163">On the **Build** menu, select **Build project**.</span></span> <span data-ttu-id="81b1f-164">Kontrollera att kompileringen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="81b1f-164">Make sure that the compilation has been completed successfully.</span></span>


## <a name="link-to-the-hortonworks-sandbox"></a><span data-ttu-id="81b1f-165">Länk till sandlådan Hortonworks</span><span class="sxs-lookup"><span data-stu-id="81b1f-165">Link to the Hortonworks Sandbox</span></span>

<span data-ttu-id="81b1f-166">Innan du kan länka till en Hortonworks Sandbox (emulator), måste du ha ett befintligt IntelliJ program.</span><span class="sxs-lookup"><span data-stu-id="81b1f-166">Before you can link to a Hortonworks Sandbox (emulator), you must have an existing IntelliJ application.</span></span>

<span data-ttu-id="81b1f-167">Om du vill länka till en emulator, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="81b1f-167">To link to an emulator, do the following:</span></span>

1. <span data-ttu-id="81b1f-168">Öppna projektet i IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="81b1f-168">Open the project in IntelliJ.</span></span>

2. <span data-ttu-id="81b1f-169">På den **visa** väljer du **verktyg Windows**, och välj sedan **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-169">On the **View** menu, select **Tools Windows**, and then select **Azure Explorer**.</span></span>

3. <span data-ttu-id="81b1f-170">Expandera **Azure**, högerklicka på **HDInsight**, och välj sedan **länka en Emulator**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-170">Expand **Azure**, right-click **HDInsight**, and then select **Link an Emulator**.</span></span>

4. <span data-ttu-id="81b1f-171">I den **länken en ny emulatorn** och ange lösenord för att du har konfigurerat för rotkontot i sandlådan Hortonworks ange värden som är samma som i följande skärmbild och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-171">In the **Link A New Emulator** window, enter the password that you've configured for the root account of the Hortonworks Sandbox, enter values similar to those in the following screenshot, and then select **OK**.</span></span> 

   ![Fönstret ”länka ett nytt emulatorn”](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. <span data-ttu-id="81b1f-173">Om du vill konfigurera emulatorn, Välj **Ja**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-173">To configure the emulator, select **Yes**.</span></span>

<span data-ttu-id="81b1f-174">När emulatorn är ansluten med emulatorn (Hortonworks Sandbox) i HDInsight-nod.</span><span class="sxs-lookup"><span data-stu-id="81b1f-174">When the emulator is connected successfully, the emulator (Hortonworks Sandbox) is listed in the HDInsight node.</span></span>

## <a name="submit-the-spark-scala-application-to-the-hortonworks-sandbox"></a><span data-ttu-id="81b1f-175">Skicka Spark Scala-programmet till Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="81b1f-175">Submit the Spark Scala application to the Hortonworks Sandbox</span></span>

<span data-ttu-id="81b1f-176">När du har länkat IntelliJ IDEA till emulatorn, kan du skicka projektet.</span><span class="sxs-lookup"><span data-stu-id="81b1f-176">After you have linked IntelliJ IDEA to the emulator, you can submit your project.</span></span>

<span data-ttu-id="81b1f-177">För att skicka ett projekt till en emulator, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="81b1f-177">To submit a project to an emulator, do the following:</span></span>

1. <span data-ttu-id="81b1f-178">I **Projektutforskaren**, högerklicka på projektet och välj sedan **skicka Spark-program till HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-178">In **Project Explorer**, right-click the project, and then select **Submit Spark application to HDInsight**.</span></span>

2. <span data-ttu-id="81b1f-179">Gör följande:</span><span class="sxs-lookup"><span data-stu-id="81b1f-179">Do the following:</span></span>

    <span data-ttu-id="81b1f-180">a.</span><span class="sxs-lookup"><span data-stu-id="81b1f-180">a.</span></span> <span data-ttu-id="81b1f-181">I den **Spark-kluster (endast Linux)** listrutan väljer du din lokala Hortonworks Sandbox.</span><span class="sxs-lookup"><span data-stu-id="81b1f-181">In the **Spark cluster (Linux only)** drop-down list, select your local Hortonworks Sandbox.</span></span>

    <span data-ttu-id="81b1f-182">b.</span><span class="sxs-lookup"><span data-stu-id="81b1f-182">b.</span></span> <span data-ttu-id="81b1f-183">I den **Main klassnamn** rutan, Välj eller ange det huvudsakliga klassnamnet.</span><span class="sxs-lookup"><span data-stu-id="81b1f-183">In the **Main class name** box, choose or enter the main class name.</span></span> <span data-ttu-id="81b1f-184">Den här kursen är namn **GroupByTest**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-184">For this tutorial, the name is **GroupByTest**.</span></span>

3. <span data-ttu-id="81b1f-185">Välj **skicka**.</span><span class="sxs-lookup"><span data-stu-id="81b1f-185">Select **Submit**.</span></span> <span data-ttu-id="81b1f-186">Jobbet skicka loggarna visas i fönstret Spark skicka verktyget.</span><span class="sxs-lookup"><span data-stu-id="81b1f-186">The job submission logs are shown in the Spark submission tool window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="81b1f-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81b1f-187">Next steps</span></span>

- <span data-ttu-id="81b1f-188">Om du vill lära dig skapa Spark-program för HDInsight med hjälp av HDInsight-verktyg för IntelliJ, gå till [använda HDInsight Tools i Azure Toolkit för IntelliJ till att skapa Spark-program för HDInsight Spark Linux-kluster](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="81b1f-188">To learn how to create Spark applications for HDInsight by using HDInsight Tools for IntelliJ, go to [Use HDInsight Tools in Azure Toolkit for IntelliJ to create Spark applications for HDInsight Spark Linux cluster](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>

- <span data-ttu-id="81b1f-189">Om du vill se en video av HDInsight-verktyg för IntelliJ, gå till [införa HDInsight-verktyg för IntelliJ för Spark-utveckling](https://www.youtube.com/watch?v=YTZzYVgut6c).</span><span class="sxs-lookup"><span data-stu-id="81b1f-189">To watch a video of HDInsight Tools for IntelliJ, go to [Introduce HDInsight Tools for IntelliJ for Spark Development](https://www.youtube.com/watch?v=YTZzYVgut6c).</span></span>

- <span data-ttu-id="81b1f-190">Om du vill lära dig att felsöka Spark-program med hjälp av verktyget via fjärranslutning på HDInsight via SSH, gå till [felsöka Spark-program på ett HDInsight-kluster med Azure Toolkit för IntelliJ via SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).</span><span class="sxs-lookup"><span data-stu-id="81b1f-190">To learn how to debug Spark applications by using the toolkit remotely on HDInsight through SSH, go to [Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).</span></span>

- <span data-ttu-id="81b1f-191">Om du vill lära dig att felsöka Spark-program med hjälp av verktyget via fjärranslutning på HDInsight via VPN, gå till [använda HDInsight Tools i Azure Toolkit för IntelliJ till att felsöka Spark-program på HDInsight Spark Linux-kluster](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).</span><span class="sxs-lookup"><span data-stu-id="81b1f-191">To learn how to debug Spark applications by using the toolkit remotely on HDInsight through VPN, go to [Use HDInsight Tools in Azure Toolkit for IntelliJ to debug Spark applications remotely on HDInsight Spark Linux cluster](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).</span></span>

- <span data-ttu-id="81b1f-192">Mer information om hur du använder HDInsight-verktyg för Eclipse för att skapa ett Spark-program, gå till [använder HDInsight-verktyg i Azure Toolkit för Eclipse att skapa Spark-program](hdinsight-apache-spark-eclipse-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="81b1f-192">To learn how to use HDInsight Tools for Eclipse to create a Spark application, go to [Use HDInsight Tools in Azure Toolkit for Eclipse to create Spark applications](hdinsight-apache-spark-eclipse-tool-plugin.md).</span></span>

- <span data-ttu-id="81b1f-193">Om du vill se en video av HDInsight-verktyg för Eclipse, gå till [HDInsight verktyg för Eclipse att skapa Spark-program](https://mix.office.com/watch/1rau2mopb6fha).</span><span class="sxs-lookup"><span data-stu-id="81b1f-193">To watch a video of HDInsight Tools for Eclipse, go to [Use HDInsight Tool for Eclipse to create Spark applications](https://mix.office.com/watch/1rau2mopb6fha).</span></span>
