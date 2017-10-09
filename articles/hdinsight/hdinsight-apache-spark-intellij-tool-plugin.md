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
# <a name="use-azure-toolkit-for-intellij-toocreate-spark-applications-for-an-hdinsight-cluster"></a><span data-ttu-id="87143-103">Använd Azure Toolkit för IntelliJ toocreate Spark-program för ett HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="87143-103">Use Azure Toolkit for IntelliJ toocreate Spark applications for an HDInsight cluster</span></span>

<span data-ttu-id="87143-104">Använd hello Azure Toolkit för IntelliJ plugin toodevelop Spark-program som skrivits i Scala och skicka dem tooan HDInsight Spark-kluster direkt från hello IntelliJ integrerad utvecklingsmiljö (IDE).</span><span class="sxs-lookup"><span data-stu-id="87143-104">Use hello Azure Toolkit for IntelliJ plug-in toodevelop Spark applications written in Scala, and then submit them tooan HDInsight Spark cluster directly from hello IntelliJ integrated development environment (IDE).</span></span> <span data-ttu-id="87143-105">Du kan använda hello plugin-program på flera sätt:</span><span class="sxs-lookup"><span data-stu-id="87143-105">You can use hello plug-in in a few ways:</span></span>

* <span data-ttu-id="87143-106">Utveckla och skicka Scala Spark-program på ett HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="87143-106">Develop and submit a Scala Spark application on an HDInsight Spark cluster.</span></span>
* <span data-ttu-id="87143-107">Åtkomst till resurserna i Azure HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="87143-107">Access your Azure HDInsight Spark cluster resources.</span></span>
* <span data-ttu-id="87143-108">Utveckla och köra ett Scala Spark-program lokalt.</span><span class="sxs-lookup"><span data-stu-id="87143-108">Develop and run a Scala Spark application locally.</span></span>

<span data-ttu-id="87143-109">toocreate ditt projekt, visa hello [skapa Spark-program med hello Azure Toolkit för IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) video.</span><span class="sxs-lookup"><span data-stu-id="87143-109">toocreate your project, view hello [Create Spark Applications with hello Azure Toolkit for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) video.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="87143-110">Du kan använda den här plugin-programmet toocreate och skicka program bara för ett HDInsight Spark-kluster på Linux.</span><span class="sxs-lookup"><span data-stu-id="87143-110">You can use this plug-in toocreate and submit applications only for an HDInsight Spark cluster on Linux.</span></span>
> 

## <a name="prerequisites"></a><span data-ttu-id="87143-111">Krav</span><span class="sxs-lookup"><span data-stu-id="87143-111">Prerequisites</span></span>

- <span data-ttu-id="87143-112">Ett Apache Spark-kluster i HDInsight Linux.</span><span class="sxs-lookup"><span data-stu-id="87143-112">An Apache Spark cluster on HDInsight Linux.</span></span> <span data-ttu-id="87143-113">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="87143-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
- <span data-ttu-id="87143-114">Oracle Java Development Kit.</span><span class="sxs-lookup"><span data-stu-id="87143-114">Oracle Java Development Kit.</span></span> <span data-ttu-id="87143-115">Du kan installera den från hello [Oracle webbplats](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="87143-115">You can install it from hello [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
- <span data-ttu-id="87143-116">IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="87143-116">IntelliJ IDEA.</span></span> <span data-ttu-id="87143-117">Den här artikeln använder version 2017.1.</span><span class="sxs-lookup"><span data-stu-id="87143-117">This article uses version 2017.1.</span></span> <span data-ttu-id="87143-118">Du kan installera den från hello [JetBrains webbplats](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="87143-118">You can install it from hello [JetBrains website](https://www.jetbrains.com/idea/download/).</span></span>

## <a name="install-azure-toolkit-for-intellij"></a><span data-ttu-id="87143-119">Installera Azure Toolkit för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="87143-119">Install Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="87143-120">Installationsanvisningar finns i [installera Azure Toolkit för IntelliJ](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="87143-120">For installation instructions, see [Install Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>

## <a name="sign-in-tooyour-azure-subscription"></a><span data-ttu-id="87143-121">Logga in tooyour Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="87143-121">Sign in tooyour Azure subscription</span></span>

1. <span data-ttu-id="87143-122">Starta hello IntelliJ IDE och öppna Utforskaren i Azure.</span><span class="sxs-lookup"><span data-stu-id="87143-122">Start hello IntelliJ IDE, and open Azure Explorer.</span></span> <span data-ttu-id="87143-123">På hello **visa** väljer du **verktyget Windows**, och välj sedan **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="87143-123">On hello **View** menu, select **Tool Windows**, and then select **Azure Explorer**.</span></span>
       
   ![hello Azure Explorer länk](./media/hdinsight-apache-spark-intellij-tool-plugin/show-azure-explorer.png)

2. <span data-ttu-id="87143-125">Högerklicka på hello **Azure** och sedan väljer **logga In**.</span><span class="sxs-lookup"><span data-stu-id="87143-125">Right-click hello **Azure** node, and then select **Sign In**.</span></span>

3. <span data-ttu-id="87143-126">I hello **Azure logga In** dialogrutan **logga in**, och ange dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="87143-126">In hello **Azure Sign In** dialog box, select **Sign in**, and then enter your Azure credentials.</span></span>

    ![hello Azure logga In dialogrutan](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-2.png)

4. <span data-ttu-id="87143-128">När du har loggat in hello **Välj prenumerationer** listar dialogrutan alla hello Azure-prenumerationer som är associerade med hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="87143-128">After you're signed in, hello **Select Subscriptions** dialog box lists all hello Azure subscriptions that are associated with hello credentials.</span></span> <span data-ttu-id="87143-129">Välj hello **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="87143-129">Select hello **Select** button.</span></span>

    ![dialogrutan Välj prenumerationer för hello](./media/hdinsight-apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

5. <span data-ttu-id="87143-131">På hello **Azure Explorer** fliken, expandera **HDInsight** tooview hello HDInsight Spark-kluster som finns i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="87143-131">On hello **Azure Explorer** tab, expand **HDInsight** tooview hello HDInsight Spark clusters that are in your subscription.</span></span>
   
    ![HDInsight Spark-kluster i Azure Explorer](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-3.png)

6. <span data-ttu-id="87143-133">tooview hello resurser (till exempel lagringskonton) som är associerade med hello kluster, kan du ytterligare expandera en nod i klustret-namn.</span><span class="sxs-lookup"><span data-stu-id="87143-133">tooview hello resources (for example, storage accounts) that are associated with hello cluster, you can further expand a cluster-name node.</span></span>
   
    ![En expanderad klusternamnet nod](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="87143-135">Kör en Spark Scala på ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="87143-135">Run a Spark Scala application on an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="87143-136">Starta IntelliJ IDEA och sedan skapa ett projekt.</span><span class="sxs-lookup"><span data-stu-id="87143-136">Start IntelliJ IDEA, and then create a project.</span></span> <span data-ttu-id="87143-137">I hello **nytt projekt** dialogrutan rutan, hello följande:</span><span class="sxs-lookup"><span data-stu-id="87143-137">In hello **New Project** dialog box, do hello following:</span></span> 

   <span data-ttu-id="87143-138">a.</span><span class="sxs-lookup"><span data-stu-id="87143-138">a.</span></span> <span data-ttu-id="87143-139">Välj **HDInsight** > **Spark i HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="87143-139">Select **HDInsight** > **Spark on HDInsight (Scala)**.</span></span>

   <span data-ttu-id="87143-140">b.</span><span class="sxs-lookup"><span data-stu-id="87143-140">b.</span></span> <span data-ttu-id="87143-141">I hello **Build verktyget** väljer du antingen hello följande, enligt tooyour behov:</span><span class="sxs-lookup"><span data-stu-id="87143-141">In hello **Build tool** list, select either of hello following, according tooyour need:</span></span>

      * <span data-ttu-id="87143-142">**Maven**, Scala skapa projekt guiden support</span><span class="sxs-lookup"><span data-stu-id="87143-142">**Maven**, for Scala project-creation wizard support</span></span>
      * <span data-ttu-id="87143-143">**Segregerade Barlasttankar**, för att hantera hello beroenden och bygga för hello Scala-projekt</span><span class="sxs-lookup"><span data-stu-id="87143-143">**SBT**, for managing hello dependencies and building for hello Scala project</span></span>

    ![dialogrutan Nytt projekt för hello](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

2. <span data-ttu-id="87143-145">Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="87143-145">Select **Next**.</span></span>

3. <span data-ttu-id="87143-146">Hej Scala skapa projekt guiden identifierar automatiskt om du har installerat hello Scala plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="87143-146">hello Scala project-creation wizard automatically detects whether you've installed hello Scala plug-in.</span></span> <span data-ttu-id="87143-147">Välj **installera**.</span><span class="sxs-lookup"><span data-stu-id="87143-147">Select **Install**.</span></span>

   ![Kontroll av Scala plugin-program](./media/hdinsight-apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. <span data-ttu-id="87143-149">toodownload hello Scala plugin-program, Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="87143-149">toodownload hello Scala plug-in, select **OK**.</span></span> <span data-ttu-id="87143-150">Följ hello instruktioner toorestart IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="87143-150">Follow hello instructions toorestart IntelliJ.</span></span> 

   ![dialogrutan för hello Scala plugin-programmet för installation](./media/hdinsight-apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. <span data-ttu-id="87143-152">I hello **nytt projekt** fönstret hello följande:</span><span class="sxs-lookup"><span data-stu-id="87143-152">In hello **New Project** window, do hello following:</span></span>  

    ![Att välja hello Spark SDK](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-new-project.png)

   <span data-ttu-id="87143-154">a.</span><span class="sxs-lookup"><span data-stu-id="87143-154">a.</span></span> <span data-ttu-id="87143-155">Ange ett namn och plats.</span><span class="sxs-lookup"><span data-stu-id="87143-155">Enter a project name and location.</span></span>

   <span data-ttu-id="87143-156">b.</span><span class="sxs-lookup"><span data-stu-id="87143-156">b.</span></span> <span data-ttu-id="87143-157">I hello **projekt SDK** listrutan, Välj **Java 1.8** för 2.x hello Spark-kluster eller välj **Java 1.7** för 1.x hello Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="87143-157">In hello **Project SDK** drop-down list, select **Java 1.8** for hello Spark 2.x cluster, or select **Java 1.7** for hello Spark 1.x cluster.</span></span>

   <span data-ttu-id="87143-158">c.</span><span class="sxs-lookup"><span data-stu-id="87143-158">c.</span></span> <span data-ttu-id="87143-159">I hello **Spark version** listrutan Scala guide för att skapa projektet integreras hello rätt version för Spark SDK och Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="87143-159">In hello **Spark version** drop-down list, Scala project creation wizard integrates hello proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="87143-160">Om hello Spark-kluster av version är äldre än 2.0 väljer **Väck 1.x**.</span><span class="sxs-lookup"><span data-stu-id="87143-160">If hello Spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="87143-161">Annars väljer **Spark2.x**.</span><span class="sxs-lookup"><span data-stu-id="87143-161">Otherwise, select **Spark2.x**.</span></span> <span data-ttu-id="87143-162">Det här exemplet används **Spark punkt 2.0.2 (Scala 2.11.8)**.</span><span class="sxs-lookup"><span data-stu-id="87143-162">This example uses **Spark 2.0.2 (Scala 2.11.8)**.</span></span>

6. <span data-ttu-id="87143-163">Välj **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="87143-163">Select **Finish**.</span></span>

7. <span data-ttu-id="87143-164">hello Spark-projekt skapas automatiskt en artefakt.</span><span class="sxs-lookup"><span data-stu-id="87143-164">hello Spark project automatically creates an artifact for you.</span></span> <span data-ttu-id="87143-165">tooview hello artefakt hello följande:</span><span class="sxs-lookup"><span data-stu-id="87143-165">tooview hello artifact, do hello following:</span></span>

   <span data-ttu-id="87143-166">a.</span><span class="sxs-lookup"><span data-stu-id="87143-166">a.</span></span> <span data-ttu-id="87143-167">På hello **filen** väljer du **projektstruktur**.</span><span class="sxs-lookup"><span data-stu-id="87143-167">On hello **File** menu, select **Project Structure**.</span></span>

   <span data-ttu-id="87143-168">b.</span><span class="sxs-lookup"><span data-stu-id="87143-168">b.</span></span> <span data-ttu-id="87143-169">I hello **projektstruktur** dialogrutan **artefakter** tooview hello standard artefakt som har skapats.</span><span class="sxs-lookup"><span data-stu-id="87143-169">In hello **Project Structure** dialog box, select **Artifacts** tooview hello default artifact that is created.</span></span> <span data-ttu-id="87143-170">Du kan också skapa egna artefakt genom att välja hello plustecken (**+**).</span><span class="sxs-lookup"><span data-stu-id="87143-170">You can also create your own artifact by selecting hello plus sign (**+**).</span></span>

      ![Artefakt info hello dialogrutan](./media/hdinsight-apache-spark-intellij-tool-plugin/default-artifact.png)
      
8. <span data-ttu-id="87143-172">Lägg till källkoden programmet hello följande:</span><span class="sxs-lookup"><span data-stu-id="87143-172">Add your application source code by doing hello following:</span></span>

   <span data-ttu-id="87143-173">a.</span><span class="sxs-lookup"><span data-stu-id="87143-173">a.</span></span> <span data-ttu-id="87143-174">Högerklicka i Projektutforskaren **src**, peka för**ny**, och välj sedan **Scala klassen**.</span><span class="sxs-lookup"><span data-stu-id="87143-174">In Project Explorer, right-click **src**, point too**New**, and then select **Scala Class**.</span></span>
      
      ![Kommandon för att skapa en Scala-klass från Projektutforskaren](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   <span data-ttu-id="87143-176">b.</span><span class="sxs-lookup"><span data-stu-id="87143-176">b.</span></span> <span data-ttu-id="87143-177">I hello **skapa nya Scala klass** dialogrutan, ange ett namn, väljer **objekt** i hello **typ** och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="87143-177">In hello **Create New Scala Class** dialog box, provide a name, select **Object** in hello **Kind** box, and then select **OK**.</span></span>
      
      ![Skapa ny Scala klass dialogruta](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   <span data-ttu-id="87143-179">c.</span><span class="sxs-lookup"><span data-stu-id="87143-179">c.</span></span> <span data-ttu-id="87143-180">I hello **MyClusterApp.scala** fil, klistra in följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="87143-180">In hello **MyClusterApp.scala** file, paste hello following code.</span></span> <span data-ttu-id="87143-181">hello koden läser hello data från HVAC.csv (finns i alla HDInsight Spark-kluster), hämtar hello rader som har endast en siffra i hello sjunde kolumnen i hello CSV-fil och skriver hello utdata för**/HVACOut** under hello standard lagringsbehållare som klustret hello.</span><span class="sxs-lookup"><span data-stu-id="87143-181">hello code reads hello data from HVAC.csv (available on all HDInsight Spark clusters), retrieves hello rows that have only one digit in hello seventh column in hello CSV file, and writes hello output too**/HVACOut** under hello default storage container for hello cluster.</span></span>

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

9. <span data-ttu-id="87143-182">Kör hello på ett HDInsight Spark-kluster genom att göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="87143-182">Run hello application on an HDInsight Spark cluster by doing hello following:</span></span>

   <span data-ttu-id="87143-183">a.</span><span class="sxs-lookup"><span data-stu-id="87143-183">a.</span></span> <span data-ttu-id="87143-184">Högerklicka på hello projektnamn i Projektutforskaren, och välj sedan **skicka Spark-program tooHDInsight**.</span><span class="sxs-lookup"><span data-stu-id="87143-184">In Project Explorer, right-click hello project name, and then select **Submit Spark Application tooHDInsight**.</span></span>
      
      ![hello skicka Spark-program tooHDInsight kommando](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

   <span data-ttu-id="87143-186">b.</span><span class="sxs-lookup"><span data-stu-id="87143-186">b.</span></span> <span data-ttu-id="87143-187">Du är tooenter ange dina autentiseringsuppgifter för Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="87143-187">You are prompted tooenter your Azure subscription credentials.</span></span> <span data-ttu-id="87143-188">I hello **Spark skicka** dialogrutan, ange följande värden hello och välj sedan **skicka**.</span><span class="sxs-lookup"><span data-stu-id="87143-188">In hello **Spark Submission** dialog box, provide hello following values, and then select **Submit**.</span></span>
      
      * <span data-ttu-id="87143-189">För **Väck kluster (endast Linux)**, Välj hello HDInsight Spark-kluster som du vill använda toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="87143-189">For **Spark clusters (Linux only)**, select hello HDInsight Spark cluster on which you want toorun your application.</span></span>

      * <span data-ttu-id="87143-190">Välj en artefakt hello IntelliJ projektet eller välj en från hello hårddisken.</span><span class="sxs-lookup"><span data-stu-id="87143-190">Select an artifact from hello IntelliJ project, or select one from hello hard drive.</span></span>

      * <span data-ttu-id="87143-191">I hello **Main klassnamn** rutan, Välj hello ellips (**...** ), Välj hello huvudsakliga klass i din programkod för datakälla och väljer sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="87143-191">In hello **Main class name** box, select hello ellipsis (**...**), select hello main class in your application source code, and then select **OK**.</span></span>

        ![dialogrutan för hello Välj Main-klass](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)

      * <span data-ttu-id="87143-193">Eftersom hello programkod i det här exemplet inte kräver kommandoradsargument eller referera burkar eller filer, kan du lämna hello återstående rutor som är tom.</span><span class="sxs-lookup"><span data-stu-id="87143-193">Because hello application code in this example does not require command-line arguments or reference JARs or files, you can leave hello remaining boxes empty.</span></span> <span data-ttu-id="87143-194">När du har angett alla hello information bör hello dialogrutan likna följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="87143-194">After you provide all hello information, hello dialog box should resemble hello following image.</span></span>
        
        ![dialogrutan för hello Spark överföring](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

   <span data-ttu-id="87143-196">c.</span><span class="sxs-lookup"><span data-stu-id="87143-196">c.</span></span> <span data-ttu-id="87143-197">Hej **Spark skicka** fliken längst hello hello fönstret ska börja visas hello pågår.</span><span class="sxs-lookup"><span data-stu-id="87143-197">hello **Spark Submission** tab at hello bottom of hello window should start displaying hello progress.</span></span> <span data-ttu-id="87143-198">Du kan också avbryta hello program genom att välja hello röd knapp i hello **Spark skicka** fönster.</span><span class="sxs-lookup"><span data-stu-id="87143-198">You can also stop hello application by selecting hello red button in hello **Spark Submission** window.</span></span>
      
      ![hello Spark skicka fönster](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      <span data-ttu-id="87143-200">toolearn hur tooaccess hello jobbutdata, finns i hello ”åtkomst och hantera HDInsight Spark-kluster med hjälp av Azure Toolkit för IntelliJ” senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="87143-200">toolearn how tooaccess hello job output, see hello "Access and manage HDInsight Spark clusters by using Azure Toolkit for IntelliJ" section later in this article.</span></span>

## <a name="run-or-debug-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="87143-201">Köra eller felsöka ett Spark Scala-program på ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="87143-201">Run or debug a Spark Scala application on an HDInsight Spark cluster</span></span>
<span data-ttu-id="87143-202">Vi rekommenderar också ett annat sätt att skicka hello Spark programmet toohello-kluster.</span><span class="sxs-lookup"><span data-stu-id="87143-202">We also recommend another way of submitting hello Spark application toohello cluster.</span></span> <span data-ttu-id="87143-203">Du kan göra det genom att ange hello parametrar i hello **kör/Debug konfigurationer** IDE.</span><span class="sxs-lookup"><span data-stu-id="87143-203">You can do so by setting hello parameters in hello **Run/Debug configurations** IDE.</span></span> <span data-ttu-id="87143-204">Mer information finns i [felsöka Spark-program på ett HDInsight-kluster med Azure Toolkit för IntelliJ via SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span><span class="sxs-lookup"><span data-stu-id="87143-204">For more information, see [Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span></span>

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a><span data-ttu-id="87143-205">Komma åt och hantera HDInsight Spark-kluster med hjälp av Azure Toolkit för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="87143-205">Access and manage HDInsight Spark clusters by using Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="87143-206">Du kan utföra olika åtgärder med hjälp av Azure Toolkit för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="87143-206">You can perform various operations by using Azure Toolkit for IntelliJ.</span></span>

### <a name="access-hello-job-view"></a><span data-ttu-id="87143-207">Åtkomst hello jobbet vy</span><span class="sxs-lookup"><span data-stu-id="87143-207">Access hello job view</span></span>
1. <span data-ttu-id="87143-208">I Azure Explorer expanderar **HDInsight**, expandera klusternamnet i hello Spark och välj sedan **jobb**.</span><span class="sxs-lookup"><span data-stu-id="87143-208">In Azure Explorer, expand **HDInsight**, expand hello Spark cluster name, and then select **Jobs**.</span></span>  

    ![Jobbet visa nod](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. <span data-ttu-id="87143-210">I högra fönstret hello hello **Spark jobbet vyn** visar alla hello-program som kördes på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="87143-210">In hello right pane, hello **Spark Job View** tab displays all hello applications that were run on hello cluster.</span></span> <span data-ttu-id="87143-211">Välj hello hello program som du vill toosee mer information.</span><span class="sxs-lookup"><span data-stu-id="87143-211">Select hello name of hello application for which you want toosee more details.</span></span>

    ![Programinformation](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)

3. <span data-ttu-id="87143-213">toodisplay grundläggande körs jobbinformation hovra över hello jobbdiagram.</span><span class="sxs-lookup"><span data-stu-id="87143-213">toodisplay basic running job information, hover over hello job graph.</span></span> <span data-ttu-id="87143-214">tooview hello steg diagram och information som genereras av varje jobb markerar du en nod i hello jobbdiagram.</span><span class="sxs-lookup"><span data-stu-id="87143-214">tooview hello stages graph and information that every job generates, select a node on hello job graph.</span></span>

    ![Steget jobbinformation](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. <span data-ttu-id="87143-216">tooview används ofta loggar som *drivrutinen Stderr*, *drivrutinen Stdout*, och *Directory Info*väljer hello **loggen** fliken.</span><span class="sxs-lookup"><span data-stu-id="87143-216">tooview frequently used logs, such as *Driver Stderr*, *Driver Stdout*, and *Directory Info*, select hello **Log** tab.</span></span>

    ![Logginformation](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)

5. <span data-ttu-id="87143-218">Du kan också visa hello Spark historik UI och hello YARN-Användargränssnittet (på programnivå hello) genom att välja en länk hello överst i fönstret hello.</span><span class="sxs-lookup"><span data-stu-id="87143-218">You can also view hello Spark history UI and hello YARN UI (at hello application level) by selecting a link at hello top of hello window.</span></span>

### <a name="access-hello-spark-history-server"></a><span data-ttu-id="87143-219">Historik fjärråtkomstserver hello Spark</span><span class="sxs-lookup"><span data-stu-id="87143-219">Access hello Spark history server</span></span>
1. <span data-ttu-id="87143-220">I Azure Explorer expanderar **HDInsight**, högerklickar du på klusternamnet Spark och väljer sedan **öppna Spark historik UI**.</span><span class="sxs-lookup"><span data-stu-id="87143-220">In Azure Explorer, expand **HDInsight**, right-click your Spark cluster name, and then select **Open Spark History UI**.</span></span> 

2. <span data-ttu-id="87143-221">När du uppmanas ange administratörsautentiseringsuppgifter för hello klustret som du angav när du ställer in hello kluster.</span><span class="sxs-lookup"><span data-stu-id="87143-221">When you're prompted, enter hello cluster's admin credentials, which you specified when you set up hello cluster.</span></span>

3. <span data-ttu-id="87143-222">Du kan använda hello programmet namnet toolook för hello program just avslutats körs på hello Spark historik server instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="87143-222">On hello Spark history server dashboard, you can use hello application name toolook for hello application that you just finished running.</span></span> <span data-ttu-id="87143-223">I hello föregående kod, ange hello programnamn med `val conf = new SparkConf().setAppName("MyClusterApp")`.</span><span class="sxs-lookup"><span data-stu-id="87143-223">In hello preceding code, you set hello application name by using `val conf = new SparkConf().setAppName("MyClusterApp")`.</span></span> <span data-ttu-id="87143-224">Programnamnet Spark är därför **MyClusterApp**.</span><span class="sxs-lookup"><span data-stu-id="87143-224">Therefore, your Spark application name is **MyClusterApp**.</span></span>

### <a name="start-hello-ambari-portal"></a><span data-ttu-id="87143-225">Starta hello Ambari-portalen</span><span class="sxs-lookup"><span data-stu-id="87143-225">Start hello Ambari portal</span></span>
1. <span data-ttu-id="87143-226">I Azure Explorer expanderar **HDInsight**, högerklickar du på klusternamnet Spark och väljer sedan **öppna klustret hanteringsportalen (Ambari)**.</span><span class="sxs-lookup"><span data-stu-id="87143-226">In Azure Explorer, expand **HDInsight**, right-click your Spark cluster name, and then select **Open Cluster Management Portal (Ambari)**.</span></span> 

2. <span data-ttu-id="87143-227">När du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="87143-227">When you're prompted, enter hello admin credentials for hello cluster.</span></span> <span data-ttu-id="87143-228">Du har angett autentiseringsuppgifterna under hello klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="87143-228">You specified these credentials during hello cluster setup process.</span></span>

### <a name="manage-azure-subscriptions"></a><span data-ttu-id="87143-229">Hantera Azure-prenumerationer</span><span class="sxs-lookup"><span data-stu-id="87143-229">Manage Azure subscriptions</span></span>
<span data-ttu-id="87143-230">Som standard visar Azure Toolkit för IntelliJ hello Spark-kluster från alla dina Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="87143-230">By default, Azure Toolkit for IntelliJ lists hello Spark clusters from all your Azure subscriptions.</span></span> <span data-ttu-id="87143-231">Om det behövs kan du ange hello prenumerationer som du vill tooaccess.</span><span class="sxs-lookup"><span data-stu-id="87143-231">If necessary, you can specify hello subscriptions that you want tooaccess.</span></span> 

1. <span data-ttu-id="87143-232">Högerklicka i Azure Explorer hello **Azure** rot nod och välj sedan **hantera prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="87143-232">In Azure Explorer, right-click hello **Azure** root node, and then select **Manage Subscriptions**.</span></span> 

2. <span data-ttu-id="87143-233">Hej, avmarkera hello kryssrutorna nästa toohello prenumerationer som du inte vill tooaccess och välj sedan **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="87143-233">In hello dialog box, clear hello check boxes next toohello subscriptions that you don't want tooaccess, and then select **Close**.</span></span> <span data-ttu-id="87143-234">Du kan också välja **logga ut** om du vill toosign utanför din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="87143-234">You can also select **Sign Out** if you want toosign out of your Azure subscription.</span></span>

## <a name="run-a-spark-scala-application-locally"></a><span data-ttu-id="87143-235">Köra ett program med Spark Scala lokalt</span><span class="sxs-lookup"><span data-stu-id="87143-235">Run a Spark Scala application locally</span></span>
<span data-ttu-id="87143-236">Du kan använda Azure Toolkit för IntelliJ toorun Spark Scala program lokalt på din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="87143-236">You can use Azure Toolkit for IntelliJ toorun Spark Scala applications locally on your workstation.</span></span> <span data-ttu-id="87143-237">hello program vanligtvis inte behöver komma åt toocluster resurser, till exempel behållare för lagring, och du kan köra och testa dem lokalt.</span><span class="sxs-lookup"><span data-stu-id="87143-237">hello applications usually don't need access toocluster resources, such as storage containers, and you can run and test them locally.</span></span>

### <a name="prerequisite"></a><span data-ttu-id="87143-238">Krav</span><span class="sxs-lookup"><span data-stu-id="87143-238">Prerequisite</span></span>
<span data-ttu-id="87143-239">När du kör hello lokalt Spark Scala-program på en Windows-dator, kan du få ett undantag som beskrivs i [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356).</span><span class="sxs-lookup"><span data-stu-id="87143-239">While you're running hello local Spark Scala application on a Windows computer, you might get an exception, as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span></span> <span data-ttu-id="87143-240">hello-undantag inträffar eftersom WinUtils.exe saknas i Windows.</span><span class="sxs-lookup"><span data-stu-id="87143-240">hello exception occurs because WinUtils.exe is missing on Windows.</span></span> 

<span data-ttu-id="87143-241">tooresolve det här felet [hämta hello körbara](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa plats som **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="87143-241">tooresolve this error, [download hello executable](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa location such as **C:\WinUtils\bin**.</span></span> <span data-ttu-id="87143-242">Lägg sedan till hello miljövariabeln **HADOOP_HOME**, och ange hello hello variabels värde för**C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="87143-242">Then, add hello environment variable **HADOOP_HOME**, and set hello value of hello variable too**C\WinUtils**.</span></span>

### <a name="run-a-local-spark-scala-application"></a><span data-ttu-id="87143-243">Kör ett lokalt Spark Scala-program</span><span class="sxs-lookup"><span data-stu-id="87143-243">Run a local Spark Scala application</span></span>
1. <span data-ttu-id="87143-244">Starta IntelliJ IDEA och skapa ett projekt.</span><span class="sxs-lookup"><span data-stu-id="87143-244">Start IntelliJ IDEA, and create a project.</span></span> 

2. <span data-ttu-id="87143-245">I hello **nytt projekt** dialogrutan rutan, hello följande:</span><span class="sxs-lookup"><span data-stu-id="87143-245">In hello **New Project** dialog box, do hello following:</span></span>
   
    <span data-ttu-id="87143-246">a.</span><span class="sxs-lookup"><span data-stu-id="87143-246">a.</span></span> <span data-ttu-id="87143-247">Välj **HDInsight** > **Spark på HDInsight lokala kör sampel (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="87143-247">Select **HDInsight** > **Spark on HDInsight Local Run Sample (Scala)**.</span></span>

    <span data-ttu-id="87143-248">b.</span><span class="sxs-lookup"><span data-stu-id="87143-248">b.</span></span> <span data-ttu-id="87143-249">I hello **Build verktyget** väljer du antingen hello följande, enligt tooyour behov:</span><span class="sxs-lookup"><span data-stu-id="87143-249">In hello **Build tool** list, select either of hello following, according tooyour need:</span></span>

      * <span data-ttu-id="87143-250">**Maven**, Scala skapa projekt guiden support</span><span class="sxs-lookup"><span data-stu-id="87143-250">**Maven**, for Scala project-creation wizard support</span></span>
      * <span data-ttu-id="87143-251">**Segregerade Barlasttankar**, för att hantera hello beroenden och bygga för hello Scala-projekt</span><span class="sxs-lookup"><span data-stu-id="87143-251">**SBT**, for managing hello dependencies and building for hello Scala project</span></span>

    ![dialogrutan Nytt projekt för hello](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run.png)

3. <span data-ttu-id="87143-253">Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="87143-253">Select **Next**.</span></span>
 
4. <span data-ttu-id="87143-254">I nästa hello-fönstret hello följande:</span><span class="sxs-lookup"><span data-stu-id="87143-254">In hello next window, do hello following:</span></span>
   
    <span data-ttu-id="87143-255">a.</span><span class="sxs-lookup"><span data-stu-id="87143-255">a.</span></span> <span data-ttu-id="87143-256">Ange ett namn och plats.</span><span class="sxs-lookup"><span data-stu-id="87143-256">Enter a project name and location.</span></span>

    <span data-ttu-id="87143-257">b.</span><span class="sxs-lookup"><span data-stu-id="87143-257">b.</span></span> <span data-ttu-id="87143-258">I hello **projekt SDK** listrutan väljer du en Java-version som är senare än version 1.7.</span><span class="sxs-lookup"><span data-stu-id="87143-258">In hello **Project SDK** drop-down list, select a Java version that's later than version 1.7.</span></span>

    <span data-ttu-id="87143-259">c.</span><span class="sxs-lookup"><span data-stu-id="87143-259">c.</span></span> <span data-ttu-id="87143-260">I hello **Spark Version** listrutan, Välj hello version av Scala som du vill toouse: Scala 2.11.x för Spark 2.0 eller Scala 2.10.x för Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="87143-260">In hello **Spark Version** drop-down list, select hello version of Scala that you want toouse: Scala 2.11.x for Spark 2.0 or Scala 2.10.x for Spark 1.6.</span></span>

    ![dialogrutan Nytt projekt för hello](./media/hdinsight-apache-spark-intellij-tool-plugin/Create-local-project.PNG)

5. <span data-ttu-id="87143-262">Välj **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="87143-262">Select **Finish**.</span></span>

6. <span data-ttu-id="87143-263">hello mallen lägger till en exempelkod (**LogQuery**) under hello **src** mapp som du kan köra lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="87143-263">hello template adds a sample code (**LogQuery**) under hello **src** folder that you can run locally on your computer.</span></span>
   
    ![Platsen för LogQuery](./media/hdinsight-apache-spark-intellij-tool-plugin/local-app.png)

7. <span data-ttu-id="87143-265">Högerklicka på hello **LogQuery** program och välj sedan **kör 'LogQuery'**.</span><span class="sxs-lookup"><span data-stu-id="87143-265">Right-click hello **LogQuery** application, and then select **Run 'LogQuery'**.</span></span> <span data-ttu-id="87143-266">På hello **kör** fliken hello längst ned i visas utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="87143-266">On hello **Run** tab at hello bottom, you see an output like hello following:</span></span>
   
   ![Spark programmet lokala kör resultat](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="convert-existing-intellij-idea-applications-toouse-azure-toolkit-for-intellij"></a><span data-ttu-id="87143-268">Konvertera befintliga IntelliJ IDEA program toouse Azure Toolkit för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="87143-268">Convert existing IntelliJ IDEA applications toouse Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="87143-269">Du kan konvertera hello befintliga Spark Scala program som du skapade i IntelliJ IDEA toobe kompatibla med Azure Toolkit för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="87143-269">You can convert hello existing Spark Scala applications that you created in IntelliJ IDEA toobe compatible with Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="87143-270">Du kan sedan använda hello plugin toosubmit hello program tooan HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="87143-270">You can then use hello plug-in toosubmit hello applications tooan HDInsight Spark cluster.</span></span>

1. <span data-ttu-id="87143-271">Öppna hello associerade .iml fil för ett befintligt Spark Scala program som har skapats via IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="87143-271">For an existing Spark Scala application that was created through IntelliJ IDEA, open hello associated .iml file.</span></span>

2. <span data-ttu-id="87143-272">Hello roten nivån är en **modulen** hello följande element:</span><span class="sxs-lookup"><span data-stu-id="87143-272">At hello root level is a **module** element like hello following:</span></span>
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   <span data-ttu-id="87143-273">Redigera hello elementet tooadd `UniqueKey="HDInsightTool"` så som hello **modulen** ser ut som hello nedan:</span><span class="sxs-lookup"><span data-stu-id="87143-273">Edit hello element tooadd `UniqueKey="HDInsightTool"` so that hello **module** element looks like hello following:</span></span>
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

3. <span data-ttu-id="87143-274">Spara hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="87143-274">Save hello changes.</span></span> <span data-ttu-id="87143-275">Ditt program bör nu vara kompatibla med Azure Toolkit för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="87143-275">Your application should now be compatible with Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="87143-276">Du kan testa den genom att högerklicka på projektnamnet på hello i Projektutforskaren.</span><span class="sxs-lookup"><span data-stu-id="87143-276">You can test it by right-clicking hello project name in Project Explorer.</span></span> <span data-ttu-id="87143-277">hello popup-menyn har nu hello alternativet **skicka Spark-program tooHDInsight**.</span><span class="sxs-lookup"><span data-stu-id="87143-277">hello pop-up menu now has hello option **Submit Spark Application tooHDInsight**.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="87143-278">Felsökning</span><span class="sxs-lookup"><span data-stu-id="87143-278">Troubleshooting</span></span>

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a><span data-ttu-id="87143-279">Fel vid lokal körning: *Använd en större heapstorlek*</span><span class="sxs-lookup"><span data-stu-id="87143-279">Error in local run: *Please use a larger heap size*</span></span>
<span data-ttu-id="87143-280">I Spark 1.6 om du använder en 32-bitars Java SDK under lokal körning kan uppstå hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="87143-280">In Spark 1.6, if you're using a 32-bit Java SDK during local run, you might encounter hello following errors:</span></span>

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

<span data-ttu-id="87143-281">Dessa fel inträffa eftersom hello heap-storlek inte är tillräckligt stor för Spark toorun.</span><span class="sxs-lookup"><span data-stu-id="87143-281">These errors happen because hello heap size is not large enough for Spark toorun.</span></span> <span data-ttu-id="87143-282">Spark kräver minst 471 MB.</span><span class="sxs-lookup"><span data-stu-id="87143-282">Spark requires at least 471 MB.</span></span> <span data-ttu-id="87143-283">(Mer information finns i [SPARK 12081](https://issues.apache.org/jira/browse/SPARK-12081).) En enkel lösning är toouse en 64-bitars Java SDK.</span><span class="sxs-lookup"><span data-stu-id="87143-283">(For more information, see [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081).) One simple solution is toouse a 64-bit Java SDK.</span></span> <span data-ttu-id="87143-284">Du kan också ändra hello JVM i IntelliJ genom att lägga till hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="87143-284">You can also change hello JVM settings in IntelliJ by adding hello following options:</span></span>

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Lägga till alternativ toohello rutan ”alternativ för VM” i IntelliJ](./media/hdinsight-apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a><span data-ttu-id="87143-286">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="87143-286">FAQ</span></span>
<span data-ttu-id="87143-287">toosubmit ett program tooAzure Data Lake Store, Välj **interaktiv** läge under hello Azure-inloggning.</span><span class="sxs-lookup"><span data-stu-id="87143-287">toosubmit an application tooAzure Data Lake Store, choose **Interactive** mode during hello Azure sign-in process.</span></span> <span data-ttu-id="87143-288">Om du väljer **automatisk** läge, du får ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="87143-288">If you select **Automated** mode, you can get an error.</span></span>

![Interaktiv inloggning](./media/hdinsight-apache-spark-intellij-tool-plugin/interative-signin.png)

<span data-ttu-id="87143-290">Nu ska löste vi problemet.</span><span class="sxs-lookup"><span data-stu-id="87143-290">Now, we resolved it.</span></span> <span data-ttu-id="87143-291">Du kan välja ett Azure Data Lake klustret toosubmit ditt program med valfri metod för inloggning.</span><span class="sxs-lookup"><span data-stu-id="87143-291">You can choose an Azure Data Lake Cluster toosubmit your application with any sign-in method.</span></span>

## <a name="feedback-and-known-issues"></a><span data-ttu-id="87143-292">Feedback och kända problem</span><span class="sxs-lookup"><span data-stu-id="87143-292">Feedback and known issues</span></span>
<span data-ttu-id="87143-293">För närvarande kan stöds visa Spark utdata direkt inte.</span><span class="sxs-lookup"><span data-stu-id="87143-293">Currently, viewing Spark outputs directly is not supported.</span></span>

<span data-ttu-id="87143-294">Om du har förslag eller feedback eller om du stöter på problem när du använder plugin-modulen, skicka e-post på hdivstool@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="87143-294">If you have any suggestions or feedback, or if you encounter any problems when you use this plug-in, email us at hdivstool@microsoft.com.</span></span>

## <span data-ttu-id="87143-295"><a name="seealso"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="87143-295"><a name="seealso"></a>Next steps</span></span>
* [<span data-ttu-id="87143-296">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="87143-296">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="87143-297">Demo</span><span class="sxs-lookup"><span data-stu-id="87143-297">Demo</span></span>
* <span data-ttu-id="87143-298">Skapa Scala projekt (video): [skapa Spark Scala-program](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="87143-298">Create Scala project (video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="87143-299">Fjärråtkomst debug (video): [Använd Azure Toolkit för IntelliJ toodebug Spark-program på HDInsight-kluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="87143-299">Remote debug (video): [Use Azure Toolkit for IntelliJ toodebug Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="87143-300">Scenarier</span><span class="sxs-lookup"><span data-stu-id="87143-300">Scenarios</span></span>
* [<span data-ttu-id="87143-301">Spark med BI: utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="87143-301">Spark with BI: Perform interactive data analysis by using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="87143-302">Spark med Machine Learning: använda Spark i HDInsight tooanalyze skapa temperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="87143-302">Spark with Machine Learning: Use Spark in HDInsight tooanalyze building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="87143-303">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="87143-303">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="87143-304">Spark Streaming: Använda Spark i HDInsight toobuild strömmande realtidsprogram</span><span class="sxs-lookup"><span data-stu-id="87143-304">Spark Streaming: Use Spark in HDInsight toobuild real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="87143-305">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="87143-305">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a><span data-ttu-id="87143-306">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="87143-306">Creating and running applications</span></span>
* [<span data-ttu-id="87143-307">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="87143-307">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="87143-308">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="87143-308">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="87143-309">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="87143-309">Tools and extensions</span></span>
* [<span data-ttu-id="87143-310">Använd Azure Toolkit för IntelliJ toodebug Spark-program via fjärranslutning via VPN</span><span class="sxs-lookup"><span data-stu-id="87143-310">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="87143-311">Använd Azure Toolkit för IntelliJ toodebug Spark-program via fjärranslutning via SSH</span><span class="sxs-lookup"><span data-stu-id="87143-311">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="87143-312">Använda HDInsight Tools för IntelliJ med Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="87143-312">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="87143-313">Använda HDInsight Tools i Azure Toolkit för Eclipse toocreate Spark-program</span><span class="sxs-lookup"><span data-stu-id="87143-313">Use HDInsight Tools in Azure Toolkit for Eclipse toocreate Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="87143-314">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="87143-314">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="87143-315">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="87143-315">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="87143-316">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="87143-316">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="87143-317">Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="87143-317">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a><span data-ttu-id="87143-318">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="87143-318">Managing resources</span></span>
* [<span data-ttu-id="87143-319">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="87143-319">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="87143-320">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="87143-320">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

