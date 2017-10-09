---
title: "aaaCreate Scala app toorun på Spark - kluster i Azure HDInsight | Microsoft Docs"
description: "Skapa ett Spark-program som skrivits i Scala med Apache Maven medan hello skapar system och en befintlig Maven archetype för Scala som tillhandahålls av IntelliJ IDEA."
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
ms.openlocfilehash: b25291b60921021486f55d78b4832a070a54d163
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-scala-maven-application-toorun-on-apache-spark-cluster-on-hdinsight"></a><span data-ttu-id="76efb-103">Skapa en Scala Maven programmet toorun på Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="76efb-103">Create a Scala Maven application toorun on Apache Spark cluster on HDInsight</span></span>

<span data-ttu-id="76efb-104">Lär dig hur toocreate ett Spark-program som skrivits i Scala med IntelliJ IDEA Maven.</span><span class="sxs-lookup"><span data-stu-id="76efb-104">Learn how toocreate a Spark application written in Scala using Maven with IntelliJ IDEA.</span></span> <span data-ttu-id="76efb-105">hello artikeln använder Apache Maven hello bygga system och börjar med en befintlig Maven archetype för Scala som tillhandahålls av IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="76efb-105">hello article uses Apache Maven as hello build system and starts with an existing Maven archetype for Scala provided by IntelliJ IDEA.</span></span>  <span data-ttu-id="76efb-106">Skapa en koppling i Scala i IntelliJ IDEA omfattar hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="76efb-106">Creating a Scala application in IntelliJ IDEA involves hello following steps:</span></span>

* <span data-ttu-id="76efb-107">Använd Maven som hello build-system.</span><span class="sxs-lookup"><span data-stu-id="76efb-107">Use Maven as hello build system.</span></span>
* <span data-ttu-id="76efb-108">Uppdatera projektet Object Model (POM) tooresolve Spark modulen filberoenden.</span><span class="sxs-lookup"><span data-stu-id="76efb-108">Update Project Object Model (POM) file tooresolve Spark module dependencies.</span></span>
* <span data-ttu-id="76efb-109">Skriva ditt program i Scala.</span><span class="sxs-lookup"><span data-stu-id="76efb-109">Write your application in Scala.</span></span>
* <span data-ttu-id="76efb-110">Generera en jar-fil som kan vara skickade tooHDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="76efb-110">Generate a jar file that can be submitted tooHDInsight Spark clusters.</span></span>
* <span data-ttu-id="76efb-111">Kör hello på Spark-kluster med Livy.</span><span class="sxs-lookup"><span data-stu-id="76efb-111">Run hello application on Spark cluster using Livy.</span></span>

> [!NOTE]
> <span data-ttu-id="76efb-112">HDInsight tillhandahåller även en IntelliJ IDEA plugin verktyget tooease hello process för att skapa och skicka program tooan HDInsight Spark-kluster på Linux.</span><span class="sxs-lookup"><span data-stu-id="76efb-112">HDInsight also provides an IntelliJ IDEA plugin tool tooease hello process of creating and submitting applications tooan HDInsight Spark cluster on Linux.</span></span> <span data-ttu-id="76efb-113">Mer information finns i [använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark-program](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="76efb-113">For more information, see [Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark applications](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="76efb-114">Krav</span><span class="sxs-lookup"><span data-stu-id="76efb-114">Prerequisites</span></span>

* <span data-ttu-id="76efb-115">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="76efb-115">An Azure subscription.</span></span> <span data-ttu-id="76efb-116">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="76efb-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="76efb-117">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="76efb-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="76efb-118">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="76efb-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="76efb-119">Oracle Java Development kit.</span><span class="sxs-lookup"><span data-stu-id="76efb-119">Oracle Java Development kit.</span></span> <span data-ttu-id="76efb-120">Du kan installera den från [här](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="76efb-120">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="76efb-121">En Java IDE.</span><span class="sxs-lookup"><span data-stu-id="76efb-121">A Java IDE.</span></span> <span data-ttu-id="76efb-122">Den här artikeln använder IntelliJ IDEA 15.0.1.</span><span class="sxs-lookup"><span data-stu-id="76efb-122">This article uses IntelliJ IDEA 15.0.1.</span></span> <span data-ttu-id="76efb-123">Du kan installera den från [här](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="76efb-123">You can install it from [here](https://www.jetbrains.com/idea/download/).</span></span>

## <a name="install-scala-plugin-for-intellij-idea"></a><span data-ttu-id="76efb-124">Installera Scala-plugin för IntelliJ IDEA</span><span class="sxs-lookup"><span data-stu-id="76efb-124">Install Scala plugin for IntelliJ IDEA</span></span>
<span data-ttu-id="76efb-125">Om installationen för IntelliJ IDEA tillfrågades inte inte för att aktivera Scala-plugin-programmet, starta IntelliJ IDEA och gå igenom följande steg tooinstall hello plugin-programmet hello:</span><span class="sxs-lookup"><span data-stu-id="76efb-125">If IntelliJ IDEA installation did not not prompt for enabling Scala plugin, launch IntelliJ IDEA and go through hello following steps tooinstall hello plugin:</span></span>

1. <span data-ttu-id="76efb-126">Starta IntelliJ IDEA och klicka på välkomstskärmen **konfigurera** och klicka sedan på **plugin-program**.</span><span class="sxs-lookup"><span data-stu-id="76efb-126">Start IntelliJ IDEA and from welcome screen click **Configure** and then click **Plugins**.</span></span>
   
    ![Aktivera scala plugin-program](./media/hdinsight-apache-spark-create-standalone-application/enable-scala-plugin.png)
2. <span data-ttu-id="76efb-128">I nästa hello-skärmen klickar du på **installera JetBrains plugin** från hello nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="76efb-128">In hello next screen, click **Install JetBrains plugin** from hello lower left corner.</span></span> <span data-ttu-id="76efb-129">I hello **Bläddra JetBrains plugin-program** dialogrutan som öppnas, söka efter Scala och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="76efb-129">In hello **Browse JetBrains Plugins** dialog box that opens, search for Scala and then click **Install**.</span></span>
   
    ![Installera scala plugin-programmet](./media/hdinsight-apache-spark-create-standalone-application/install-scala-plugin.png)
3. <span data-ttu-id="76efb-131">När hello plugin-programmet installeras, klickar du på hello **starta om IntelliJ IDEA knappen** toorestart hello IDE.</span><span class="sxs-lookup"><span data-stu-id="76efb-131">After hello plugin installs successfully, click hello **Restart IntelliJ IDEA button** toorestart hello IDE.</span></span>

## <a name="create-a-standalone-scala-project"></a><span data-ttu-id="76efb-132">Skapa ett fristående Scala-projekt</span><span class="sxs-lookup"><span data-stu-id="76efb-132">Create a standalone Scala project</span></span>
1. <span data-ttu-id="76efb-133">Öppna IntelliJ IDEA och skapa ett nytt projekt.</span><span class="sxs-lookup"><span data-stu-id="76efb-133">Launch IntelliJ IDEA and create a new project.</span></span> <span data-ttu-id="76efb-134">Kontrollera hello följande alternativ och klickar sedan på i hello dialogrutan Nytt projekt, **nästa**.</span><span class="sxs-lookup"><span data-stu-id="76efb-134">In hello new project dialog box, make hello following choices, and then click **Next**.</span></span>
   
    ![Skapa Maven-projekt](./media/hdinsight-apache-spark-create-standalone-application/create-maven-project.png)
   
   * <span data-ttu-id="76efb-136">Välj **Maven** som hello projekttypen.</span><span class="sxs-lookup"><span data-stu-id="76efb-136">Select **Maven** as hello project type.</span></span>
   * <span data-ttu-id="76efb-137">Ange en **projektet SDK**.</span><span class="sxs-lookup"><span data-stu-id="76efb-137">Specify a **Project SDK**.</span></span> <span data-ttu-id="76efb-138">Klicka på nytt och navigera toohello Java-installationskatalogen, vanligtvis `C:\Program Files\Java\jdk1.8.0_66`.</span><span class="sxs-lookup"><span data-stu-id="76efb-138">Click New and navigate toohello Java installation directory, typically `C:\Program Files\Java\jdk1.8.0_66`.</span></span>
   * <span data-ttu-id="76efb-139">Välj hello **skapa från archetype** alternativet.</span><span class="sxs-lookup"><span data-stu-id="76efb-139">Select hello **Create from archetype** option.</span></span>
   * <span data-ttu-id="76efb-140">Välj hello listan över archetypes **org.scala tools.archetypes:scala-archetype-enkel**.</span><span class="sxs-lookup"><span data-stu-id="76efb-140">From hello list of archetypes, select **org.scala-tools.archetypes:scala-archetype-simple**.</span></span> <span data-ttu-id="76efb-141">Detta skapar hello rätt katalogstrukturen och hämta hello krävs beroenden toowrite Scala standardprogrammet.</span><span class="sxs-lookup"><span data-stu-id="76efb-141">This will create hello right directory structure and download hello required default dependencies toowrite Scala program.</span></span>
2. <span data-ttu-id="76efb-142">Ange relevanta värden för **GroupId**, **artefakt-ID**, och **Version**.</span><span class="sxs-lookup"><span data-stu-id="76efb-142">Provide relevant values for **GroupId**, **ArtifactId**, and **Version**.</span></span> <span data-ttu-id="76efb-143">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="76efb-143">Click **Next**.</span></span>
3. <span data-ttu-id="76efb-144">Hello nästa i dialogrutan anger du Maven arbetskatalog och andra inställningar, accepterar hello standardinställningarna och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="76efb-144">In hello next dialog box, where you specify Maven home directory and other user settings, accept hello defaults and click **Next**.</span></span>
4. <span data-ttu-id="76efb-145">Hello senaste i dialogrutan Ange namn och plats och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="76efb-145">In hello last dialog box, specify a project name and location and then click **Finish**.</span></span>
5. <span data-ttu-id="76efb-146">Ta bort hello **MySpec.Scala** filen på **src\test\scala\com\microsoft\spark\example**.</span><span class="sxs-lookup"><span data-stu-id="76efb-146">Delete hello **MySpec.Scala** file at **src\test\scala\com\microsoft\spark\example**.</span></span> <span data-ttu-id="76efb-147">Du behöver inte detta för hello program.</span><span class="sxs-lookup"><span data-stu-id="76efb-147">You do not need this for hello application.</span></span>
6. <span data-ttu-id="76efb-148">Om det behövs kan du byta namn på hello käll- och filer som standard.</span><span class="sxs-lookup"><span data-stu-id="76efb-148">If required, rename hello default source and test files.</span></span> <span data-ttu-id="76efb-149">Från hello till vänster i hello IntelliJ IDEA och navigera för**src\main\scala\com.microsoft.spark.example**.</span><span class="sxs-lookup"><span data-stu-id="76efb-149">From hello left pane in hello IntelliJ IDEA, navigate too**src\main\scala\com.microsoft.spark.example**.</span></span> <span data-ttu-id="76efb-150">Högerklicka på **App.scala**, klickar du på **Refactor**, klicka på Byt namn på filen hello i dialogrutan Ange hello nytt namn för hello program och klicka sedan på **Refactor**.</span><span class="sxs-lookup"><span data-stu-id="76efb-150">Right-click **App.scala**, click **Refactor**, click Rename file, and in hello dialog box, provide hello new name for hello application and then click **Refactor**.</span></span>
   
    ![Byta namn på filer](./media/hdinsight-apache-spark-create-standalone-application/rename-scala-files.png)  
7. <span data-ttu-id="76efb-152">I hello efterföljande steg uppdaterar du hello pom.xml toodefine hello beroenden för hello Spark Scala-programmet.</span><span class="sxs-lookup"><span data-stu-id="76efb-152">In hello subsequent steps, you will update hello pom.xml toodefine hello dependencies for hello Spark Scala application.</span></span> <span data-ttu-id="76efb-153">För dessa beroenden toobe hämtas och lösas automatiskt, måste du konfigurera Maven därefter.</span><span class="sxs-lookup"><span data-stu-id="76efb-153">For those dependencies toobe downloaded and resolved automatically, you must configure Maven accordingly.</span></span>
   
    ![Konfigurera Maven vid automatisk hämtning](./media/hdinsight-apache-spark-create-standalone-application/configure-maven.png)
   
   1. <span data-ttu-id="76efb-155">Från hello **filen** -menyn klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="76efb-155">From hello **File** menu, click **Settings**.</span></span>
   2. <span data-ttu-id="76efb-156">I hello **inställningar** dialogrutan går för**bygga, körning och distribution** > **Build Tools** > **Maven**  >  **Importera**.</span><span class="sxs-lookup"><span data-stu-id="76efb-156">In hello **Settings** dialog box, navigate too**Build, Execution, Deployment** > **Build Tools** > **Maven** > **Importing**.</span></span>
   3. <span data-ttu-id="76efb-157">Välj alternativet för hello för**importera Maven-projekt automatiskt**.</span><span class="sxs-lookup"><span data-stu-id="76efb-157">Select hello option too**Import Maven projects automatically**.</span></span>
   4. <span data-ttu-id="76efb-158">Klicka på **tillämpa**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="76efb-158">Click **Apply**, and then click **OK**.</span></span>
8. <span data-ttu-id="76efb-159">Uppdatera hello Scala källa filen tooinclude programkoden.</span><span class="sxs-lookup"><span data-stu-id="76efb-159">Update hello Scala source file tooinclude your application code.</span></span> <span data-ttu-id="76efb-160">Öppna och ersätta befintliga exempelkod med följande kod hello hello och spara hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="76efb-160">Open and replace hello existing sample code with hello following code and save hello changes.</span></span> <span data-ttu-id="76efb-161">Den här koden läser hello data från hello HVAC.csv (finns i alla HDInsight Spark-kluster), hämtar hello rader som bara innehåller en siffra i hello sjätte kolumnen och skriver hello utdata för**/HVACOut** under hello standardlagring behållare för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="76efb-161">This code reads hello data from hello HVAC.csv (available on all HDInsight Spark clusters), retrieves hello rows that only have one digit in hello sixth column, and writes hello output too**/HVACOut** under hello default storage container for hello cluster.</span></span>
   
        package com.microsoft.spark.example
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        /**
          * Test IO toowasb
          */
        object WasbIOTest {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("WASBIOTest")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find hello rows which have only one digit in hello 7th column in hello CSV
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACout")
          }
        }
9. <span data-ttu-id="76efb-162">Uppdatera hello pom.xml.</span><span class="sxs-lookup"><span data-stu-id="76efb-162">Update hello pom.xml.</span></span>
   
   1. <span data-ttu-id="76efb-163">Inom `<project>\<properties>` lägga till hello följande:</span><span class="sxs-lookup"><span data-stu-id="76efb-163">Within `<project>\<properties>` add hello following:</span></span>
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   2. <span data-ttu-id="76efb-164">Inom `<project>\<dependencies>` lägga till hello följande:</span><span class="sxs-lookup"><span data-stu-id="76efb-164">Within `<project>\<dependencies>` add hello following:</span></span>
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      <span data-ttu-id="76efb-165">Spara ändringarna toopom.xml.</span><span class="sxs-lookup"><span data-stu-id="76efb-165">Save changes toopom.xml.</span></span>
10. <span data-ttu-id="76efb-166">Skapa hello .jar-fil.</span><span class="sxs-lookup"><span data-stu-id="76efb-166">Create hello .jar file.</span></span> <span data-ttu-id="76efb-167">IntelliJ IDEA gör det möjligt för skapa JAR som ett resultat av ett projekt.</span><span class="sxs-lookup"><span data-stu-id="76efb-167">IntelliJ IDEA enables creation of JAR as an artifact of a project.</span></span> <span data-ttu-id="76efb-168">Utför följande steg hello.</span><span class="sxs-lookup"><span data-stu-id="76efb-168">Perform hello following steps.</span></span>
    
    1. <span data-ttu-id="76efb-169">Från hello **filen** -menyn klickar du på **projektstruktur**.</span><span class="sxs-lookup"><span data-stu-id="76efb-169">From hello **File** menu, click **Project Structure**.</span></span>
    2. <span data-ttu-id="76efb-170">I hello **projektstruktur** dialogrutan klickar du på **artefakter** och klicka sedan på hello plus symbolen.</span><span class="sxs-lookup"><span data-stu-id="76efb-170">In hello **Project Structure** dialog box, click **Artifacts** and then click hello plus symbol.</span></span> <span data-ttu-id="76efb-171">Hello popup-menyn klickar du på **JAR**, och klicka sedan på **från moduler med beroenden**.</span><span class="sxs-lookup"><span data-stu-id="76efb-171">From hello pop-up dialog box, click **JAR**, and then click **From modules with dependencies**.</span></span>
       
        ![Skapa JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-1.png)
    3. <span data-ttu-id="76efb-173">I hello **skapa JAR från moduler** dialogrutan klickar du på ellipsknappen hello (![tre punkter](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) mot hello **Main klassen**.</span><span class="sxs-lookup"><span data-stu-id="76efb-173">In hello **Create JAR from Modules** dialog box, click hello ellipsis (![ellipsis](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) against hello **Main Class**.</span></span>
    4. <span data-ttu-id="76efb-174">I hello **Välj Main klassen** dialogrutan, Välj hello-klass som visas som standard och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="76efb-174">In hello **Select Main Class** dialog box, select hello class that appears by default and then click **OK**.</span></span>
       
        ![Skapa JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-2.png)
    5. <span data-ttu-id="76efb-176">I hello **skapa JAR från moduler** dialogrutan Kontrollera hello alternativet för**extrahera toohello mål JAR** är markerad och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="76efb-176">In hello **Create JAR from Modules** dialog box, make sure that hello option too**extract toohello target JAR** is selected, and then click **OK**.</span></span> <span data-ttu-id="76efb-177">Detta skapar en enda JAR med alla beroenden.</span><span class="sxs-lookup"><span data-stu-id="76efb-177">This creates a single JAR with all dependencies.</span></span>
       
        ![Skapa JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-3.png)
    6. <span data-ttu-id="76efb-179">hello utdata layout fliken listar alla hello burkar som ingår som en del av hello Maven-projekt.</span><span class="sxs-lookup"><span data-stu-id="76efb-179">hello output layout tab lists all hello jars that are included as part of hello Maven project.</span></span> <span data-ttu-id="76efb-180">Du kan välja och ta bort hello sådana där hello Scala program har ingen direkt beroende.</span><span class="sxs-lookup"><span data-stu-id="76efb-180">You can select and delete hello ones on which hello Scala application has no direct dependency.</span></span> <span data-ttu-id="76efb-181">För hello program skapar vi här, kan du ta bort alla utom hello senast (**SparkSimpleApp kompilera utdata**).</span><span class="sxs-lookup"><span data-stu-id="76efb-181">For hello application we are creating here, you can remove all but hello last one (**SparkSimpleApp compile output**).</span></span> <span data-ttu-id="76efb-182">Välj hello burkar toodelete och klicka sedan på hello **ta bort** ikon.</span><span class="sxs-lookup"><span data-stu-id="76efb-182">Select hello jars toodelete and then click hello **Delete** icon.</span></span>
       
        ![Skapa JAR](./media/hdinsight-apache-spark-create-standalone-application/delete-output-jars.png)
       
        <span data-ttu-id="76efb-184">Kontrollera att **bygger på Kontrollera** är markerad, vilket säkerställer att hello jar skapas varje gång hello projektet skapats eller uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="76efb-184">Make sure **Build on make** box is selected, which ensures that hello jar is created every time hello project is built or updated.</span></span> <span data-ttu-id="76efb-185">Klicka på **gäller** och sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="76efb-185">Click **Apply** and then **OK**.</span></span>
    7. <span data-ttu-id="76efb-186">Hello menyraden klickar du på **skapa**, och klicka sedan på **Se projektet**.</span><span class="sxs-lookup"><span data-stu-id="76efb-186">From hello menu bar, click **Build**, and then click **Make Project**.</span></span> <span data-ttu-id="76efb-187">Du kan också klicka på **skapa artefakter** toocreate hello jar.</span><span class="sxs-lookup"><span data-stu-id="76efb-187">You can also click **Build Artifacts** toocreate hello jar.</span></span> <span data-ttu-id="76efb-188">hello utdata jar skapas under **\out\artifacts**.</span><span class="sxs-lookup"><span data-stu-id="76efb-188">hello output jar is created under **\out\artifacts**.</span></span>
       
        ![Skapa JAR](./media/hdinsight-apache-spark-create-standalone-application/output.png)

## <a name="run-hello-application-on-hello-spark-cluster"></a><span data-ttu-id="76efb-190">Kör hello på hello Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="76efb-190">Run hello application on hello Spark cluster</span></span>
<span data-ttu-id="76efb-191">toorun hello programmet på hello klustret, måste du göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="76efb-191">toorun hello application on hello cluster, you must do hello following:</span></span>

* <span data-ttu-id="76efb-192">**Kopiera hello programmet jar toohello Azure storage blob** som associerats med hello kluster.</span><span class="sxs-lookup"><span data-stu-id="76efb-192">**Copy hello application jar toohello Azure storage blob** associated with hello cluster.</span></span> <span data-ttu-id="76efb-193">Du kan använda [ **AzCopy**](../storage/common/storage-use-azcopy.md), ett kommando rad toodo-verktyget så.</span><span class="sxs-lookup"><span data-stu-id="76efb-193">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command line utility, toodo so.</span></span> <span data-ttu-id="76efb-194">Det finns en mängd andra klienter samt att du kan använda tooupload data.</span><span class="sxs-lookup"><span data-stu-id="76efb-194">There are a lot of other clients as well that you can use tooupload data.</span></span> <span data-ttu-id="76efb-195">Du kan hitta mer om dem i [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="76efb-195">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>
* <span data-ttu-id="76efb-196">**Använd Livius toosubmit ett program-jobb via fjärranslutning** toohello Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="76efb-196">**Use Livy toosubmit an application job remotely** toohello Spark cluster.</span></span> <span data-ttu-id="76efb-197">Spark-kluster i HDInsight innehåller Livius som exponerar REST-slutpunkter tooremotely skicka Spark jobb.</span><span class="sxs-lookup"><span data-stu-id="76efb-197">Spark clusters on HDInsight includes Livy that exposes REST endpoints tooremotely submit Spark jobs.</span></span> <span data-ttu-id="76efb-198">Mer information finns i [skicka Spark jobb via fjärranslutning med Livy med Spark-kluster i HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="76efb-198">For more information, see [Submit Spark jobs remotely using Livy with Spark clusters on HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span></span>

## <a name="next-step"></a><span data-ttu-id="76efb-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="76efb-199">Next step</span></span>

<span data-ttu-id="76efb-200">I denna artikel du lärt dig hur toocreate ett Spark scala-program.</span><span class="sxs-lookup"><span data-stu-id="76efb-200">In this article you learned how toocreate a Spark scala application.</span></span> <span data-ttu-id="76efb-201">Avancerade toohello nästa artikel toolearn hur toorun det här programmet på ett HDInsight Spark-kluster med Livy.</span><span class="sxs-lookup"><span data-stu-id="76efb-201">Advance toohello next article toolearn how toorun this application on an HDInsight Spark cluster using Livy.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="76efb-202">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="76efb-202">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

