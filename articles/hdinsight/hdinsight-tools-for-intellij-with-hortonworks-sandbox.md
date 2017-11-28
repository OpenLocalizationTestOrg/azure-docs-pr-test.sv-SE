---
title: "aaaUse Azure Toolkit för IntelliJ med Hortonworks Sandbox | Microsoft Docs"
description: "Lär dig hur toouse HDInsight Tools i Azure Toolkit för IntelliJ med Hortonworks Sandbox."
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
ms.openlocfilehash: 2bf97068a9cec99fcc7b4ff9469b91d8cbe2a8d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a><span data-ttu-id="eaa3a-104">Använda HDInsight Tools för IntelliJ med Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="eaa3a-104">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>

<span data-ttu-id="eaa3a-105">Lär dig hur toouse HDInsight-verktyg för IntelliJ toodevelop Apache Scala program och testa hello program på [Hortonworks Sandbox](http://hortonworks.com/products/sandbox/) körs på din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-105">Learn how toouse HDInsight Tools for IntelliJ toodevelop Apache Scala applications and then test hello applications on [Hortonworks Sandbox](http://hortonworks.com/products/sandbox/) running on your workstation.</span></span> 

<span data-ttu-id="eaa3a-106">[IntelliJ IDEA](https://www.jetbrains.com/idea/) är en Java-integrerad utvecklingsmiljö (IDE) för att utveckla programvara.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-106">[IntelliJ IDEA](https://www.jetbrains.com/idea/) is a Java-integrated development environment (IDE) for developing computer software.</span></span> <span data-ttu-id="eaa3a-107">När du har utvecklats och testa dina program på Hortonworks Sandbox kan du flytta dem för[Azure HDInsight](hdinsight-hadoop-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="eaa3a-107">After you have developed and tested your applications on Hortonworks Sandbox, you can move them too[Azure HDInsight](hdinsight-hadoop-introduction.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eaa3a-108">Krav</span><span class="sxs-lookup"><span data-stu-id="eaa3a-108">Prerequisites</span></span>

<span data-ttu-id="eaa3a-109">Innan du börjar den här vägledningen måste du ha:</span><span class="sxs-lookup"><span data-stu-id="eaa3a-109">Before you begin this tutorial, you must have:</span></span>

- <span data-ttu-id="eaa3a-110">Hortonworks Data Platform (HDP) 2.4 på Hortonworks Sandbox körs på din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-110">Hortonworks Data Platform (HDP) 2.4 on Hortonworks Sandbox running on your local environment.</span></span> <span data-ttu-id="eaa3a-111">tooconfigure HDP, se [komma igång i hello Hadoop-ekosystemet med en Hadoop sandbox på en virtuell dator](hdinsight-hadoop-emulator-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="eaa3a-111">tooconfigure HDP, see [Get started in hello Hadoop ecosystem with a Hadoop sandbox on a virtual machine](hdinsight-hadoop-emulator-get-started.md).</span></span> 
    >[!NOTE]
    ><span data-ttu-id="eaa3a-112">HDInsight-verktyg för IntelliJ har testats med HDP 2.4.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-112">HDInsight Tools for IntelliJ has been tested only with HDP 2.4.</span></span> <span data-ttu-id="eaa3a-113">Expandera tooget HDP 2.4 **Hortonworks Sandbox Arkiv** på hello [Hortonworks Sandbox hämtar plats](http://hortonworks.com/downloads/#sandbox).</span><span class="sxs-lookup"><span data-stu-id="eaa3a-113">tooget HDP 2.4, expand **Hortonworks Sandbox Archive** on hello [Hortonworks Sandbox downloads site](http://hortonworks.com/downloads/#sandbox).</span></span>

- <span data-ttu-id="eaa3a-114">[Java Developer Kit (JDK) version 1,8 eller senare](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="eaa3a-114">[Java Developer Kit (JDK) version 1.8 or later](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span> <span data-ttu-id="eaa3a-115">JDK krävs av hello Azure Toolkit för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-115">JDK is required by hello Azure Toolkit for IntelliJ.</span></span>

- <span data-ttu-id="eaa3a-116">[IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) med hello [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) plugin-program och hello [Azure Toolkit för IntelliJ](../azure-toolkit-for-intellij.md) plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-116">[IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) with hello [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) plug-in and hello [Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij.md) plug-in.</span></span> <span data-ttu-id="eaa3a-117">HDInsight-verktyg för IntelliJ är tillgänglig som en del av hello Azure Toolkit för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-117">HDInsight Tools for IntelliJ is available as a part of hello Azure Toolkit for IntelliJ.</span></span> 

  <span data-ttu-id="eaa3a-118">tooinstall hello plugin-program, hello följande:</span><span class="sxs-lookup"><span data-stu-id="eaa3a-118">tooinstall hello plug-ins, do hello following:</span></span>

  1. <span data-ttu-id="eaa3a-119">Öppna IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-119">Open IntelliJ IDEA.</span></span>
  2. <span data-ttu-id="eaa3a-120">På hello **Välkommen** väljer **konfigurera**, och välj sedan **plugin-program**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-120">On hello **Welcome** screen, select **Configure**, and then select **Plugins**.</span></span>
  3. <span data-ttu-id="eaa3a-121">Välj **installera JetBrains plugin** i hello nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-121">Select **Install JetBrains plugin** in hello lower-left corner.</span></span>
  4. <span data-ttu-id="eaa3a-122">Använd hello Sök funktionen toosearch för **Scala**, och välj sedan **installera**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-122">Use hello search function toosearch for **Scala**, and then select **Install**.</span></span>
  5. <span data-ttu-id="eaa3a-123">Välj **starta om IntelliJ IDEA** toocomplete hello installation.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-123">Select **Restart IntelliJ IDEA** toocomplete hello installation.</span></span>
  6. <span data-ttu-id="eaa3a-124">Upprepa steg 4 och 5 tooinstall hello **Azure Toolkit för IntelliJ**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-124">Repeat step 4 and 5 tooinstall hello **Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="eaa3a-125">Mer information finns i [installera hello Azure Toolkit för IntelliJ](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="eaa3a-125">For more information, see [Install hello Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>

## <a name="create-a-spark-scala-application"></a><span data-ttu-id="eaa3a-126">Skapa ett Spark Scala-program</span><span class="sxs-lookup"><span data-stu-id="eaa3a-126">Create a Spark Scala application</span></span>

<span data-ttu-id="eaa3a-127">I det här avsnittet Skapa en Scala exempelprojektet med IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-127">In this section, you create a sample Scala project by using IntelliJ IDEA.</span></span> <span data-ttu-id="eaa3a-128">I nästa avsnitt om hello länka IntelliJ IDEA toohello Hortonworks Sandbox (emulator) innan du skickar hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-128">In hello next section, you link IntelliJ IDEA toohello Hortonworks Sandbox (emulator) before you submit hello project.</span></span>

1. <span data-ttu-id="eaa3a-129">Öppna IntelliJ IDEA från din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-129">Open IntelliJ IDEA from your workstation.</span></span> <span data-ttu-id="eaa3a-130">I hello **nytt projekt** dialogrutan rutan, hello följande:</span><span class="sxs-lookup"><span data-stu-id="eaa3a-130">In hello **New Project** dialog box, do hello following:</span></span>

   <span data-ttu-id="eaa3a-131">a.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-131">a.</span></span> <span data-ttu-id="eaa3a-132">Välj **HDInsight** > **Spark i HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-132">Select **HDInsight** > **Spark on HDInsight (Scala)**.</span></span>

   <span data-ttu-id="eaa3a-133">b.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-133">b.</span></span> <span data-ttu-id="eaa3a-134">I hello **Build verktyget** väljer du antingen hello följande, enligt tooyour behov:</span><span class="sxs-lookup"><span data-stu-id="eaa3a-134">In hello **Build tool** list, select either of hello following, according tooyour need:</span></span>

    * <span data-ttu-id="eaa3a-135">**Maven**, Scala skapa projekt guiden support</span><span class="sxs-lookup"><span data-stu-id="eaa3a-135">**Maven**, for Scala project-creation wizard support</span></span>
    * <span data-ttu-id="eaa3a-136">**Segregerade Barlasttankar**, för att hantera hello beroenden och bygga för hello Scala-projekt</span><span class="sxs-lookup"><span data-stu-id="eaa3a-136">**SBT**, for managing hello dependencies and building for hello Scala project</span></span>

   ![dialogrutan Nytt projekt för hello](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. <span data-ttu-id="eaa3a-138">Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-138">Select **Next**.</span></span>

3. <span data-ttu-id="eaa3a-139">I hello nästa **nytt projekt** dialogrutan rutan, hello följande:</span><span class="sxs-lookup"><span data-stu-id="eaa3a-139">In hello next **New Project** dialog box, do hello following:</span></span>

    ![Skapa IntelliJ Scala projektegenskaperna](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)

    <span data-ttu-id="eaa3a-141">a.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-141">a.</span></span> <span data-ttu-id="eaa3a-142">I hello **projektnamn** ange ett projektnamn.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-142">In hello **Project name** box, enter a project name.</span></span>

    <span data-ttu-id="eaa3a-143">b.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-143">b.</span></span> <span data-ttu-id="eaa3a-144">I hello **projektet plats** ange en projektplats.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-144">In hello **Project location** box, enter a project location.</span></span>

    <span data-ttu-id="eaa3a-145">c.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-145">c.</span></span> <span data-ttu-id="eaa3a-146">Nästa toohello **projekt SDK** listrutan, Välj **ny**väljer **JDK**, och sedan ange hello mapp för Java JDK 1,7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-146">Next toohello **Project SDK** drop-down list, select **New**, select **JDK**, and then specify hello folder of Java JDK version 1.7 or later.</span></span> <span data-ttu-id="eaa3a-147">Välj **Java 1.8** för 2.x hello Spark-kluster eller välj **Java 1.7** för 1.x hello Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-147">Select **Java 1.8** for hello Spark 2.x cluster, or select **Java 1.7** for hello Spark 1.x cluster.</span></span> <span data-ttu-id="eaa3a-148">hello standardsökvägen är C:\Program Files\Java\jdk1.8.x_xxx.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-148">hello default location is C:\Program Files\Java\jdk1.8.x_xxx.</span></span>

    <span data-ttu-id="eaa3a-149">d.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-149">d.</span></span> <span data-ttu-id="eaa3a-150">I hello **Spark version** listrutan Scala guide för att skapa projektet integreras hello rätt version för Spark SDK och Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-150">In hello **Spark version** drop-down list, Scala project creation wizard integrates hello proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="eaa3a-151">Om hello Spark-kluster av version är äldre än 2.0 väljer **Väck 1.x**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-151">If hello Spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="eaa3a-152">Annars väljer **Spark2.x**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-152">Otherwise, select **Spark2.x**.</span></span> <span data-ttu-id="eaa3a-153">Det här exemplet använder Spark 1.6.2 (Scala 2.10.5).</span><span class="sxs-lookup"><span data-stu-id="eaa3a-153">This example uses Spark 1.6.2 (Scala 2.10.5).</span></span> <span data-ttu-id="eaa3a-154">Kontrollera att du använder hello databasen markeras Scala 2.10.x.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-154">Make sure that you are using hello repository marked Scala 2.10.x.</span></span> <span data-ttu-id="eaa3a-155">Använd inte hello databasen markeras Scala 2.11.x.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-155">Do not use hello repository marked Scala 2.11.x.</span></span>

4. <span data-ttu-id="eaa3a-156">Välj **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-156">Select **Finish**.</span></span>

5. <span data-ttu-id="eaa3a-157">Om hello **projekt** vyn inte är redan öppen genom att trycka på **Alt + 1** tooopen den.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-157">If hello **Project** view is not already open, press **Alt+1** tooopen it.</span></span>

6. <span data-ttu-id="eaa3a-158">I **Projektutforskaren**, expandera hello projektet och välj sedan **src**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-158">In **Project Explorer**, expand hello project, and then select **src**.</span></span>

7. <span data-ttu-id="eaa3a-159">Högerklicka på **src**, peka för**ny**, och välj sedan **Scala klassen**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-159">Right-click **src**, point too**New**, and then select **Scala class**.</span></span>

8. <span data-ttu-id="eaa3a-160">I hello **namn** ange ett namn i hello **typ** väljer **objekt**, och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-160">In hello **Name** box, enter a name, in hello **Kind** box, select **Object**, and then select **OK**.</span></span>

    ![hello Skapa ny Scala klass fönster](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. <span data-ttu-id="eaa3a-162">Klistra in följande kod hello i hello .scala filen:</span><span class="sxs-lookup"><span data-stu-id="eaa3a-162">In hello .scala file, paste hello following code:</span></span>

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

10. <span data-ttu-id="eaa3a-163">På hello **skapa** väljer du **bygga projektet**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-163">On hello **Build** menu, select **Build project**.</span></span> <span data-ttu-id="eaa3a-164">Kontrollera att hello kompilering har slutförts.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-164">Make sure that hello compilation has been completed successfully.</span></span>


## <a name="link-toohello-hortonworks-sandbox"></a><span data-ttu-id="eaa3a-165">Länka toohello Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="eaa3a-165">Link toohello Hortonworks Sandbox</span></span>

<span data-ttu-id="eaa3a-166">Innan du kan länka tooa Hortonworks Sandbox (emulator), måste du ha ett befintligt IntelliJ program.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-166">Before you can link tooa Hortonworks Sandbox (emulator), you must have an existing IntelliJ application.</span></span>

<span data-ttu-id="eaa3a-167">toolink tooan emulatorn hello följande:</span><span class="sxs-lookup"><span data-stu-id="eaa3a-167">toolink tooan emulator, do hello following:</span></span>

1. <span data-ttu-id="eaa3a-168">Öppna hello projekt i IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-168">Open hello project in IntelliJ.</span></span>

2. <span data-ttu-id="eaa3a-169">På hello **visa** väljer du **verktyg Windows**, och välj sedan **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-169">On hello **View** menu, select **Tools Windows**, and then select **Azure Explorer**.</span></span>

3. <span data-ttu-id="eaa3a-170">Expandera **Azure**, högerklicka på **HDInsight**, och välj sedan **länka en Emulator**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-170">Expand **Azure**, right-click **HDInsight**, and then select **Link an Emulator**.</span></span>

4. <span data-ttu-id="eaa3a-171">I hello **länken en ny emulatorn** fönstret Ange hello lösenord att du har konfigurerats för hello rotkontot av hello Hortonworks Sandbox ange värden liknande toothose i följande skärmbild hello och välj sedan **OK** .</span><span class="sxs-lookup"><span data-stu-id="eaa3a-171">In hello **Link A New Emulator** window, enter hello password that you've configured for hello root account of hello Hortonworks Sandbox, enter values similar toothose in hello following screenshot, and then select **OK**.</span></span> 

   ![Hej ”länka ett nytt emulatorn” fönster](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. <span data-ttu-id="eaa3a-173">tooconfigure hello-emulatorn väljer **Ja**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-173">tooconfigure hello emulator, select **Yes**.</span></span>

<span data-ttu-id="eaa3a-174">När hello-emulatorn är ansluten med hello-emulatorn (Hortonworks Sandbox) i hello HDInsight nod.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-174">When hello emulator is connected successfully, hello emulator (Hortonworks Sandbox) is listed in hello HDInsight node.</span></span>

## <a name="submit-hello-spark-scala-application-toohello-hortonworks-sandbox"></a><span data-ttu-id="eaa3a-175">Skicka hello Spark Scala programmet toohello Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="eaa3a-175">Submit hello Spark Scala application toohello Hortonworks Sandbox</span></span>

<span data-ttu-id="eaa3a-176">När du har länkat IntelliJ IDEA toohello emulator, kan du skicka projektet.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-176">After you have linked IntelliJ IDEA toohello emulator, you can submit your project.</span></span>

<span data-ttu-id="eaa3a-177">toosubmit en tooan emulator projekt hello följande:</span><span class="sxs-lookup"><span data-stu-id="eaa3a-177">toosubmit a project tooan emulator, do hello following:</span></span>

1. <span data-ttu-id="eaa3a-178">I **Projektutforskaren**, högerklicka på hello-projektet och välj sedan **skicka Spark programmet tooHDInsight**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-178">In **Project Explorer**, right-click hello project, and then select **Submit Spark application tooHDInsight**.</span></span>

2. <span data-ttu-id="eaa3a-179">Hej du följande:</span><span class="sxs-lookup"><span data-stu-id="eaa3a-179">Do hello following:</span></span>

    <span data-ttu-id="eaa3a-180">a.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-180">a.</span></span> <span data-ttu-id="eaa3a-181">I hello **Spark-kluster (endast Linux)** listrutan väljer du din lokala Hortonworks Sandbox.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-181">In hello **Spark cluster (Linux only)** drop-down list, select your local Hortonworks Sandbox.</span></span>

    <span data-ttu-id="eaa3a-182">b.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-182">b.</span></span> <span data-ttu-id="eaa3a-183">I hello **Main klassnamn** rutan, Välj eller ange hello huvudsakliga klassnamn.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-183">In hello **Main class name** box, choose or enter hello main class name.</span></span> <span data-ttu-id="eaa3a-184">Den här kursen är hello namn **GroupByTest**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-184">For this tutorial, hello name is **GroupByTest**.</span></span>

3. <span data-ttu-id="eaa3a-185">Välj **skicka**.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-185">Select **Submit**.</span></span> <span data-ttu-id="eaa3a-186">hello jobbet skicka loggarna visas i hello Spark skicka verktygsfönster.</span><span class="sxs-lookup"><span data-stu-id="eaa3a-186">hello job submission logs are shown in hello Spark submission tool window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eaa3a-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eaa3a-187">Next steps</span></span>

- <span data-ttu-id="eaa3a-188">hur toocreate Spark-program för HDInsight med hjälp av HDInsight-verktyg för IntelliJ, gå för toolearn[använda HDInsight Tools i Azure Toolkit för IntelliJ toocreate Spark-program för HDInsight Spark Linux-kluster](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="eaa3a-188">toolearn how toocreate Spark applications for HDInsight by using HDInsight Tools for IntelliJ, go too[Use HDInsight Tools in Azure Toolkit for IntelliJ toocreate Spark applications for HDInsight Spark Linux cluster](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>

- <span data-ttu-id="eaa3a-189">toowatch en video med HDInsight-verktyg för IntelliJ, gå för[införa HDInsight-verktyg för IntelliJ för Spark-utveckling](https://www.youtube.com/watch?v=YTZzYVgut6c).</span><span class="sxs-lookup"><span data-stu-id="eaa3a-189">toowatch a video of HDInsight Tools for IntelliJ, go too[Introduce HDInsight Tools for IntelliJ for Spark Development](https://www.youtube.com/watch?v=YTZzYVgut6c).</span></span>

- <span data-ttu-id="eaa3a-190">toolearn hur toodebug Spark-program med hjälp av hello toolkit via fjärranslutning på HDInsight via SSH för Gå[felsöka Spark-program på ett HDInsight-kluster med Azure Toolkit för IntelliJ via SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).</span><span class="sxs-lookup"><span data-stu-id="eaa3a-190">toolearn how toodebug Spark applications by using hello toolkit remotely on HDInsight through SSH, go too[Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).</span></span>

- <span data-ttu-id="eaa3a-191">toolearn hur toodebug Spark-program med hjälp av hello toolkit via fjärranslutning på HDInsight via VPN, gå för[använda HDInsight Tools i Azure Toolkit för IntelliJ toodebug Spark-program på HDInsight Spark Linux-kluster](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).</span><span class="sxs-lookup"><span data-stu-id="eaa3a-191">toolearn how toodebug Spark applications by using hello toolkit remotely on HDInsight through VPN, go too[Use HDInsight Tools in Azure Toolkit for IntelliJ toodebug Spark applications remotely on HDInsight Spark Linux cluster](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).</span></span>

- <span data-ttu-id="eaa3a-192">hur toouse HDInsight-verktyg för Eclipse toocreate ett Spark-program går för toolearn[använda HDInsight Tools i Azure Toolkit för Eclipse toocreate Spark-program](hdinsight-apache-spark-eclipse-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="eaa3a-192">toolearn how toouse HDInsight Tools for Eclipse toocreate a Spark application, go too[Use HDInsight Tools in Azure Toolkit for Eclipse toocreate Spark applications](hdinsight-apache-spark-eclipse-tool-plugin.md).</span></span>

- <span data-ttu-id="eaa3a-193">toowatch en video med HDInsight-verktyg för Eclipse gå för[HDInsight verktyg för Eclipse toocreate Spark-program](https://mix.office.com/watch/1rau2mopb6fha).</span><span class="sxs-lookup"><span data-stu-id="eaa3a-193">toowatch a video of HDInsight Tools for Eclipse, go too[Use HDInsight Tool for Eclipse toocreate Spark applications](https://mix.office.com/watch/1rau2mopb6fha).</span></span>
