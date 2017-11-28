---
title: "Skapa Scala app att köras på Spark - kluster i Azure HDInsight | Microsoft Docs"
description: "Skapa ett Spark-program som skrivits i Scala med Apache Maven som build-system och en befintlig Maven archetype för Scala som tillhandahålls av IntelliJ IDEA."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b2467a40-a340-4b80-bb00-f2c3339db57b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: nitinme
ms.openlocfilehash: 95dba08744357f8800b05e3d4b892e3a363d5985
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-scala-maven-application-to-run-on-apache-spark-cluster-on-hdinsight"></a><span data-ttu-id="29408-103">Skapa en Scala Maven-program körs i Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="29408-103">Create a Scala Maven application to run on Apache Spark cluster on HDInsight</span></span>

<span data-ttu-id="29408-104">Lär dig hur du skapar ett Spark-program som skrivits i Scala med IntelliJ IDEA Maven.</span><span class="sxs-lookup"><span data-stu-id="29408-104">Learn how to create a Spark application written in Scala using Maven with IntelliJ IDEA.</span></span> <span data-ttu-id="29408-105">Artikeln använder Apache Maven som build-system och börjar med en befintlig Maven archetype för Scala som tillhandahålls av IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="29408-105">The article uses Apache Maven as the build system and starts with an existing Maven archetype for Scala provided by IntelliJ IDEA.</span></span>  <span data-ttu-id="29408-106">Skapa en koppling i Scala i IntelliJ IDEA omfattar följande steg:</span><span class="sxs-lookup"><span data-stu-id="29408-106">Creating a Scala application in IntelliJ IDEA involves the following steps:</span></span>

* <span data-ttu-id="29408-107">Använd Maven som build-system.</span><span class="sxs-lookup"><span data-stu-id="29408-107">Use Maven as the build system.</span></span>
* <span data-ttu-id="29408-108">Uppdatera projektet Object Model (POM)-filen för att lösa Spark modulen beroenden.</span><span class="sxs-lookup"><span data-stu-id="29408-108">Update Project Object Model (POM) file to resolve Spark module dependencies.</span></span>
* <span data-ttu-id="29408-109">Skriva ditt program i Scala.</span><span class="sxs-lookup"><span data-stu-id="29408-109">Write your application in Scala.</span></span>
* <span data-ttu-id="29408-110">Generera en jar-fil som kan skickas till HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="29408-110">Generate a jar file that can be submitted to HDInsight Spark clusters.</span></span>
* <span data-ttu-id="29408-111">Kör programmet på Spark-kluster med Livy.</span><span class="sxs-lookup"><span data-stu-id="29408-111">Run the application on Spark cluster using Livy.</span></span>

> [!NOTE]
> <span data-ttu-id="29408-112">HDInsight tillhandahåller även en IntelliJ IDEA plugin-verktyget för att underlätta processen med att skapa och skicka ett HDInsight Spark-kluster på Linux-program.</span><span class="sxs-lookup"><span data-stu-id="29408-112">HDInsight also provides an IntelliJ IDEA plugin tool to ease the process of creating and submitting applications to an HDInsight Spark cluster on Linux.</span></span> <span data-ttu-id="29408-113">Mer information finns i [använda HDInsight Tools-Plugin för IntelliJ IDEA till att skapa och skicka Spark-program](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="29408-113">For more information, see [Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark applications](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="29408-114">Krav</span><span class="sxs-lookup"><span data-stu-id="29408-114">Prerequisites</span></span>

* <span data-ttu-id="29408-115">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="29408-115">An Azure subscription.</span></span> <span data-ttu-id="29408-116">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="29408-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="29408-117">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="29408-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="29408-118">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="29408-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="29408-119">Oracle Java Development kit.</span><span class="sxs-lookup"><span data-stu-id="29408-119">Oracle Java Development kit.</span></span> <span data-ttu-id="29408-120">Du kan installera den från [här](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="29408-120">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="29408-121">En Java IDE.</span><span class="sxs-lookup"><span data-stu-id="29408-121">A Java IDE.</span></span> <span data-ttu-id="29408-122">Den här artikeln använder IntelliJ IDEA 15.0.1.</span><span class="sxs-lookup"><span data-stu-id="29408-122">This article uses IntelliJ IDEA 15.0.1.</span></span> <span data-ttu-id="29408-123">Du kan installera den från [här](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="29408-123">You can install it from [here](https://www.jetbrains.com/idea/download/).</span></span>

## <a name="install-scala-plugin-for-intellij-idea"></a><span data-ttu-id="29408-124">Installera Scala-plugin för IntelliJ IDEA</span><span class="sxs-lookup"><span data-stu-id="29408-124">Install Scala plugin for IntelliJ IDEA</span></span>
<span data-ttu-id="29408-125">Om installationen för IntelliJ IDEA tillfrågades inte inte för att aktivera Scala-plugin-programmet, starta IntelliJ IDEA och gå igenom följande steg för att installera plugin-programmet:</span><span class="sxs-lookup"><span data-stu-id="29408-125">If IntelliJ IDEA installation did not not prompt for enabling Scala plugin, launch IntelliJ IDEA and go through the following steps to install the plugin:</span></span>

1. <span data-ttu-id="29408-126">Starta IntelliJ IDEA och klicka på välkomstskärmen **konfigurera** och klicka sedan på **plugin-program**.</span><span class="sxs-lookup"><span data-stu-id="29408-126">Start IntelliJ IDEA and from welcome screen click **Configure** and then click **Plugins**.</span></span>
   
    ![Aktivera scala plugin-program](./media/hdinsight-apache-spark-create-standalone-application/enable-scala-plugin.png)
2. <span data-ttu-id="29408-128">Klicka på nästa skärm **installera JetBrains plugin** från det nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="29408-128">In the next screen, click **Install JetBrains plugin** from the lower left corner.</span></span> <span data-ttu-id="29408-129">I den **Bläddra JetBrains plugin-program** dialogrutan som öppnas, söka efter Scala och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="29408-129">In the **Browse JetBrains Plugins** dialog box that opens, search for Scala and then click **Install**.</span></span>
   
    ![Installera scala plugin-programmet](./media/hdinsight-apache-spark-create-standalone-application/install-scala-plugin.png)
3. <span data-ttu-id="29408-131">När plugin-programmet installeras, klickar du på den **starta om IntelliJ IDEA knappen** att starta om IDE.</span><span class="sxs-lookup"><span data-stu-id="29408-131">After the plugin installs successfully, click the **Restart IntelliJ IDEA button** to restart the IDE.</span></span>

## <a name="create-a-standalone-scala-project"></a><span data-ttu-id="29408-132">Skapa ett fristående Scala-projekt</span><span class="sxs-lookup"><span data-stu-id="29408-132">Create a standalone Scala project</span></span>
1. <span data-ttu-id="29408-133">Öppna IntelliJ IDEA och skapa ett nytt projekt.</span><span class="sxs-lookup"><span data-stu-id="29408-133">Launch IntelliJ IDEA and create a new project.</span></span> <span data-ttu-id="29408-134">Följande alternativ i dialogrutan Nytt projekt och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="29408-134">In the new project dialog box, make the following choices, and then click **Next**.</span></span>
   
    ![Skapa Maven-projekt](./media/hdinsight-apache-spark-create-standalone-application/create-maven-project.png)
   
   * <span data-ttu-id="29408-136">Välj **Maven** som projekttypen.</span><span class="sxs-lookup"><span data-stu-id="29408-136">Select **Maven** as the project type.</span></span>
   * <span data-ttu-id="29408-137">Ange en **projektet SDK**.</span><span class="sxs-lookup"><span data-stu-id="29408-137">Specify a **Project SDK**.</span></span> <span data-ttu-id="29408-138">Klicka på nytt och navigera till installationskatalogen Java vanligtvis `C:\Program Files\Java\jdk1.8.0_66`.</span><span class="sxs-lookup"><span data-stu-id="29408-138">Click New and navigate to the Java installation directory, typically `C:\Program Files\Java\jdk1.8.0_66`.</span></span>
   * <span data-ttu-id="29408-139">Välj den **skapa från archetype** alternativet.</span><span class="sxs-lookup"><span data-stu-id="29408-139">Select the **Create from archetype** option.</span></span>
   * <span data-ttu-id="29408-140">Välj i listan över archetypes **org.scala tools.archetypes:scala-archetype-enkel**.</span><span class="sxs-lookup"><span data-stu-id="29408-140">From the list of archetypes, select **org.scala-tools.archetypes:scala-archetype-simple**.</span></span> <span data-ttu-id="29408-141">Detta skapar rätt katalogstrukturen och hämta nödvändig beroenden för att skriva Scala program.</span><span class="sxs-lookup"><span data-stu-id="29408-141">This will create the right directory structure and download the required default dependencies to write Scala program.</span></span>
2. <span data-ttu-id="29408-142">Ange relevanta värden för **GroupId**, **artefakt-ID**, och **Version**.</span><span class="sxs-lookup"><span data-stu-id="29408-142">Provide relevant values for **GroupId**, **ArtifactId**, and **Version**.</span></span> <span data-ttu-id="29408-143">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="29408-143">Click **Next**.</span></span>
3. <span data-ttu-id="29408-144">I nästa dialogrutan där du anger Maven arbetskatalog och andra inställningar, acceptera standardinställningarna och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="29408-144">In the next dialog box, where you specify Maven home directory and other user settings, accept the defaults and click **Next**.</span></span>
4. <span data-ttu-id="29408-145">I den sista dialogrutan, ange ett namn och en plats och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="29408-145">In the last dialog box, specify a project name and location and then click **Finish**.</span></span>
5. <span data-ttu-id="29408-146">Ta bort den **MySpec.Scala** filen på **src\test\scala\com\microsoft\spark\example**.</span><span class="sxs-lookup"><span data-stu-id="29408-146">Delete the **MySpec.Scala** file at **src\test\scala\com\microsoft\spark\example**.</span></span> <span data-ttu-id="29408-147">Du behöver inte detta för programmet.</span><span class="sxs-lookup"><span data-stu-id="29408-147">You do not need this for the application.</span></span>
6. <span data-ttu-id="29408-148">Om det behövs, Byt namn på standard käll- och test.</span><span class="sxs-lookup"><span data-stu-id="29408-148">If required, rename the default source and test files.</span></span> <span data-ttu-id="29408-149">Gå till i den vänstra rutan i IntelliJ IDEA **src\main\scala\com.microsoft.spark.example**.</span><span class="sxs-lookup"><span data-stu-id="29408-149">From the left pane in the IntelliJ IDEA, navigate to **src\main\scala\com.microsoft.spark.example**.</span></span> <span data-ttu-id="29408-150">Högerklicka på **App.scala**, klickar du på **Refactor**, klicka på Byt namn på filen i dialogrutan, ange det nya namnet för programmet och klicka sedan på **Refactor**.</span><span class="sxs-lookup"><span data-stu-id="29408-150">Right-click **App.scala**, click **Refactor**, click Rename file, and in the dialog box, provide the new name for the application and then click **Refactor**.</span></span>
   
    ![Byta namn på filer](./media/hdinsight-apache-spark-create-standalone-application/rename-scala-files.png)  
7. <span data-ttu-id="29408-152">I efterföljande steg uppdateras pom.xml om du vill definiera beroenden för Spark Scala-programmet.</span><span class="sxs-lookup"><span data-stu-id="29408-152">In the subsequent steps, you will update the pom.xml to define the dependencies for the Spark Scala application.</span></span> <span data-ttu-id="29408-153">Dessa beroenden hämtas och lösas automatiskt, måste du konfigurera Maven därefter.</span><span class="sxs-lookup"><span data-stu-id="29408-153">For those dependencies to be downloaded and resolved automatically, you must configure Maven accordingly.</span></span>
   
    ![Konfigurera Maven vid automatisk hämtning](./media/hdinsight-apache-spark-create-standalone-application/configure-maven.png)
   
   1. <span data-ttu-id="29408-155">Från den **filen** -menyn klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="29408-155">From the **File** menu, click **Settings**.</span></span>
   2. <span data-ttu-id="29408-156">I den **inställningar** dialogrutan går du till **bygga, körning och distribution** > **Build Tools** > **Maven**  >  **Importera**.</span><span class="sxs-lookup"><span data-stu-id="29408-156">In the **Settings** dialog box, navigate to **Build, Execution, Deployment** > **Build Tools** > **Maven** > **Importing**.</span></span>
   3. <span data-ttu-id="29408-157">Välj alternativet för att **importera Maven-projekt automatiskt**.</span><span class="sxs-lookup"><span data-stu-id="29408-157">Select the option to **Import Maven projects automatically**.</span></span>
   4. <span data-ttu-id="29408-158">Klicka på **tillämpa**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="29408-158">Click **Apply**, and then click **OK**.</span></span>
8. <span data-ttu-id="29408-159">Uppdatera källfilen Scala om du vill inkludera programkoden.</span><span class="sxs-lookup"><span data-stu-id="29408-159">Update the Scala source file to include your application code.</span></span> <span data-ttu-id="29408-160">Öppna och Ersätt den befintliga exempelkoden med följande kod och spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="29408-160">Open and replace the existing sample code with the following code and save the changes.</span></span> <span data-ttu-id="29408-161">Den här koden läser data från HVAC.csv (finns i alla HDInsight Spark-kluster), hämtar de rader som bara innehåller en siffra i sjätte kolumnen och skriver utdata till **/HVACOut** under standardbehållare för lagring för det kluster.</span><span class="sxs-lookup"><span data-stu-id="29408-161">This code reads the data from the HVAC.csv (available on all HDInsight Spark clusters), retrieves the rows that only have one digit in the sixth column, and writes the output to **/HVACOut** under the default storage container for the cluster.</span></span>
   
        package com.microsoft.spark.example
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        /**
          * Test IO to wasb
          */
        object WasbIOTest {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("WASBIOTest")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows which have only one digit in the 7th column in the CSV
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACout")
          }
        }
9. <span data-ttu-id="29408-162">Uppdatera pom.xml.</span><span class="sxs-lookup"><span data-stu-id="29408-162">Update the pom.xml.</span></span>
   
   1. <span data-ttu-id="29408-163">Inom `<project>\<properties>` Lägg till följande:</span><span class="sxs-lookup"><span data-stu-id="29408-163">Within `<project>\<properties>` add the following:</span></span>
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   2. <span data-ttu-id="29408-164">Inom `<project>\<dependencies>` Lägg till följande:</span><span class="sxs-lookup"><span data-stu-id="29408-164">Within `<project>\<dependencies>` add the following:</span></span>
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      <span data-ttu-id="29408-165">Spara ändringar i pom.xml.</span><span class="sxs-lookup"><span data-stu-id="29408-165">Save changes to pom.xml.</span></span>
10. <span data-ttu-id="29408-166">Skapa .jar-fil.</span><span class="sxs-lookup"><span data-stu-id="29408-166">Create the .jar file.</span></span> <span data-ttu-id="29408-167">IntelliJ IDEA gör det möjligt för skapa JAR som ett resultat av ett projekt.</span><span class="sxs-lookup"><span data-stu-id="29408-167">IntelliJ IDEA enables creation of JAR as an artifact of a project.</span></span> <span data-ttu-id="29408-168">Utför följande steg.</span><span class="sxs-lookup"><span data-stu-id="29408-168">Perform the following steps.</span></span>
    
    1. <span data-ttu-id="29408-169">Från den **filen** -menyn klickar du på **projektstruktur**.</span><span class="sxs-lookup"><span data-stu-id="29408-169">From the **File** menu, click **Project Structure**.</span></span>
    2. <span data-ttu-id="29408-170">I den **projektstruktur** dialogrutan klickar du på **artefakter** och sedan klicka på plustecknet.</span><span class="sxs-lookup"><span data-stu-id="29408-170">In the **Project Structure** dialog box, click **Artifacts** and then click the plus symbol.</span></span> <span data-ttu-id="29408-171">Klicka på dialogrutan popup- **JAR**, och klicka sedan på **från moduler med beroenden**.</span><span class="sxs-lookup"><span data-stu-id="29408-171">From the pop-up dialog box, click **JAR**, and then click **From modules with dependencies**.</span></span>
       
        ![Skapa JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-1.png)
    3. <span data-ttu-id="29408-173">I den **skapa JAR från moduler** dialogrutan klickar du på ellipsknappen (![tre punkter](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) mot den **Main klassen**.</span><span class="sxs-lookup"><span data-stu-id="29408-173">In the **Create JAR from Modules** dialog box, click the ellipsis (![ellipsis](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) against the **Main Class**.</span></span>
    4. <span data-ttu-id="29408-174">I den **Välj Main klassen** dialogrutan, Välj den klass som visas som standard och klicka sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="29408-174">In the **Select Main Class** dialog box, select the class that appears by default and then click **OK**.</span></span>
       
        ![Skapa JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-2.png)
    5. <span data-ttu-id="29408-176">I den **skapa JAR från moduler** dialogrutan rutan, kontrollera att alternativet att **extrahera till målet JAR** är markerad och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="29408-176">In the **Create JAR from Modules** dialog box, make sure that the option to **extract to the target JAR** is selected, and then click **OK**.</span></span> <span data-ttu-id="29408-177">Detta skapar en enda JAR med alla beroenden.</span><span class="sxs-lookup"><span data-stu-id="29408-177">This creates a single JAR with all dependencies.</span></span>
       
        ![Skapa JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-3.png)
    6. <span data-ttu-id="29408-179">Fliken layout utdata visar alla burkar som ingår som en del av Maven-projekt.</span><span class="sxs-lookup"><span data-stu-id="29408-179">The output layout tab lists all the jars that are included as part of the Maven project.</span></span> <span data-ttu-id="29408-180">Du kan välja och ta bort de där programmet Scala har ingen direkt beroende.</span><span class="sxs-lookup"><span data-stu-id="29408-180">You can select and delete the ones on which the Scala application has no direct dependency.</span></span> <span data-ttu-id="29408-181">För program som skapar vi här, kan du ta bort alla utom det senaste (**SparkSimpleApp kompilera utdata**).</span><span class="sxs-lookup"><span data-stu-id="29408-181">For the application we are creating here, you can remove all but the last one (**SparkSimpleApp compile output**).</span></span> <span data-ttu-id="29408-182">Välj burkar att ta bort och klicka sedan på den **ta bort** ikon.</span><span class="sxs-lookup"><span data-stu-id="29408-182">Select the jars to delete and then click the **Delete** icon.</span></span>
       
        ![Skapa JAR](./media/hdinsight-apache-spark-create-standalone-application/delete-output-jars.png)
       
        <span data-ttu-id="29408-184">Kontrollera att **bygger på Kontrollera** är markerad, vilket säkerställer att jar skapas varje gång projektet skapats eller uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="29408-184">Make sure **Build on make** box is selected, which ensures that the jar is created every time the project is built or updated.</span></span> <span data-ttu-id="29408-185">Klicka på **gäller** och sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="29408-185">Click **Apply** and then **OK**.</span></span>
    7. <span data-ttu-id="29408-186">På menyraden klickar du på **skapa**, och klicka sedan på **Se projektet**.</span><span class="sxs-lookup"><span data-stu-id="29408-186">From the menu bar, click **Build**, and then click **Make Project**.</span></span> <span data-ttu-id="29408-187">Du kan också klicka på **skapa artefakter** att skapa jar.</span><span class="sxs-lookup"><span data-stu-id="29408-187">You can also click **Build Artifacts** to create the jar.</span></span> <span data-ttu-id="29408-188">Utdata jar skapas under **\out\artifacts**.</span><span class="sxs-lookup"><span data-stu-id="29408-188">The output jar is created under **\out\artifacts**.</span></span>
       
        ![Skapa JAR](./media/hdinsight-apache-spark-create-standalone-application/output.png)

## <a name="run-the-application-on-the-spark-cluster"></a><span data-ttu-id="29408-190">Köra programmet på Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="29408-190">Run the application on the Spark cluster</span></span>
<span data-ttu-id="29408-191">Om du vill köra programmet på klustret, måste du göra följande:</span><span class="sxs-lookup"><span data-stu-id="29408-191">To run the application on the cluster, you must do the following:</span></span>

* <span data-ttu-id="29408-192">**Kopiera jar för programmet till Azure storage-blob** associeras med klustret.</span><span class="sxs-lookup"><span data-stu-id="29408-192">**Copy the application jar to the Azure storage blob** associated with the cluster.</span></span> <span data-ttu-id="29408-193">Du kan använda [ **AzCopy**](../storage/common/storage-use-azcopy.md), ett kommandoradsverktyg, gör.</span><span class="sxs-lookup"><span data-stu-id="29408-193">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command line utility, to do so.</span></span> <span data-ttu-id="29408-194">Det finns en mängd andra klienter samt som du kan använda för att överföra data.</span><span class="sxs-lookup"><span data-stu-id="29408-194">There are a lot of other clients as well that you can use to upload data.</span></span> <span data-ttu-id="29408-195">Du kan hitta mer om dem i [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="29408-195">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>
* <span data-ttu-id="29408-196">**Använda Livius för att skicka ett program-jobb via fjärranslutning** till Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="29408-196">**Use Livy to submit an application job remotely** to the Spark cluster.</span></span> <span data-ttu-id="29408-197">Spark-kluster i HDInsight innehåller Livius som exponerar REST-slutpunkter för att skicka Spark jobb via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="29408-197">Spark clusters on HDInsight includes Livy that exposes REST endpoints to remotely submit Spark jobs.</span></span> <span data-ttu-id="29408-198">Mer information finns i [skicka Spark jobb via fjärranslutning med Livy med Spark-kluster i HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="29408-198">For more information, see [Submit Spark jobs remotely using Livy with Spark clusters on HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span></span>

## <a name="next-step"></a><span data-ttu-id="29408-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29408-199">Next step</span></span>

<span data-ttu-id="29408-200">Du lärt dig hur du skapar ett Spark scala-program i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="29408-200">In this article you learned how to create a Spark scala application.</span></span> <span data-ttu-id="29408-201">Gå vidare till nästa artikeln information om hur du kör det här programmet på ett HDInsight Spark-kluster med Livy.</span><span class="sxs-lookup"><span data-stu-id="29408-201">Advance to the next article to learn how to run this application on an HDInsight Spark cluster using Livy.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="29408-202">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="29408-202">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

