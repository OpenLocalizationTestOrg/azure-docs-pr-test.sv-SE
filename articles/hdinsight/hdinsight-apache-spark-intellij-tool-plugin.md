---
title: "Azure Toolkit för IntelliJ: skapa Spark-program för ett HDInsight-kluster | Microsoft Docs"
description: "Använd Azure-verktygen för IntelliJ för att utveckla Spark-program som skrivits i Scala och skicka dem till ett HDInsight Spark-kluster."
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
ms.openlocfilehash: 19cb8f436fa4d86f323013a5d4b3b50bf6c80a1a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-toolkit-for-intellij-to-create-spark-applications-for-an-hdinsight-cluster"></a><span data-ttu-id="c4b40-103">Använda Azure Toolkit för IntelliJ för att skapa Spark-program för ett HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="c4b40-103">Use Azure Toolkit for IntelliJ to create Spark applications for an HDInsight cluster</span></span>

<span data-ttu-id="c4b40-104">Använda Azure Toolkit för IntelliJ plugin-programmet för att utveckla Spark-program som skrivits i Scala och skicka dem till ett HDInsight Spark-kluster direkt från IntelliJ integrerad utvecklingsmiljö (IDE).</span><span class="sxs-lookup"><span data-stu-id="c4b40-104">Use the Azure Toolkit for IntelliJ plug-in to develop Spark applications written in Scala, and then submit them to an HDInsight Spark cluster directly from the IntelliJ integrated development environment (IDE).</span></span> <span data-ttu-id="c4b40-105">Du kan använda plugin-programmet på flera sätt:</span><span class="sxs-lookup"><span data-stu-id="c4b40-105">You can use the plug-in in a few ways:</span></span>

* <span data-ttu-id="c4b40-106">Utveckla och skicka Scala Spark-program på ett HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="c4b40-106">Develop and submit a Scala Spark application on an HDInsight Spark cluster.</span></span>
* <span data-ttu-id="c4b40-107">Åtkomst till resurserna i Azure HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="c4b40-107">Access your Azure HDInsight Spark cluster resources.</span></span>
* <span data-ttu-id="c4b40-108">Utveckla och köra ett Scala Spark-program lokalt.</span><span class="sxs-lookup"><span data-stu-id="c4b40-108">Develop and run a Scala Spark application locally.</span></span>

<span data-ttu-id="c4b40-109">Skapa projektet genom att visa den [skapa Spark-program med Azure Toolkit för IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) video.</span><span class="sxs-lookup"><span data-stu-id="c4b40-109">To create your project, view the [Create Spark Applications with the Azure Toolkit for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) video.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c4b40-110">Du kan använda plugin-modulen för att skapa och skicka program bara för ett HDInsight Spark-kluster på Linux.</span><span class="sxs-lookup"><span data-stu-id="c4b40-110">You can use this plug-in to create and submit applications only for an HDInsight Spark cluster on Linux.</span></span>
> 

## <a name="prerequisites"></a><span data-ttu-id="c4b40-111">Krav</span><span class="sxs-lookup"><span data-stu-id="c4b40-111">Prerequisites</span></span>

- <span data-ttu-id="c4b40-112">Ett Apache Spark-kluster i HDInsight Linux.</span><span class="sxs-lookup"><span data-stu-id="c4b40-112">An Apache Spark cluster on HDInsight Linux.</span></span> <span data-ttu-id="c4b40-113">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="c4b40-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
- <span data-ttu-id="c4b40-114">Oracle Java Development Kit.</span><span class="sxs-lookup"><span data-stu-id="c4b40-114">Oracle Java Development Kit.</span></span> <span data-ttu-id="c4b40-115">Du kan installera det från den [Oracle webbplats](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="c4b40-115">You can install it from the [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
- <span data-ttu-id="c4b40-116">IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="c4b40-116">IntelliJ IDEA.</span></span> <span data-ttu-id="c4b40-117">Den här artikeln använder version 2017.1.</span><span class="sxs-lookup"><span data-stu-id="c4b40-117">This article uses version 2017.1.</span></span> <span data-ttu-id="c4b40-118">Du kan installera det från den [JetBrains webbplats](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="c4b40-118">You can install it from the [JetBrains website](https://www.jetbrains.com/idea/download/).</span></span>

## <a name="install-azure-toolkit-for-intellij"></a><span data-ttu-id="c4b40-119">Installera Azure Toolkit för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="c4b40-119">Install Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="c4b40-120">Installationsanvisningar finns i [installera Azure Toolkit för IntelliJ](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="c4b40-120">For installation instructions, see [Install Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>

## <a name="sign-in-to-your-azure-subscription"></a><span data-ttu-id="c4b40-121">Logga in till din Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c4b40-121">Sign in to your Azure subscription</span></span>

1. <span data-ttu-id="c4b40-122">Starta IntelliJ IDE och öppna Utforskaren i Azure.</span><span class="sxs-lookup"><span data-stu-id="c4b40-122">Start the IntelliJ IDE, and open Azure Explorer.</span></span> <span data-ttu-id="c4b40-123">På den **visa** väljer du **verktyget Windows**, och välj sedan **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-123">On the **View** menu, select **Tool Windows**, and then select **Azure Explorer**.</span></span>
       
   ![Länken Azure Explorer](./media/hdinsight-apache-spark-intellij-tool-plugin/show-azure-explorer.png)

2. <span data-ttu-id="c4b40-125">Högerklicka på den **Azure** och sedan väljer **logga In**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-125">Right-click the **Azure** node, and then select **Sign In**.</span></span>

3. <span data-ttu-id="c4b40-126">I den **Azure logga In** dialogrutan **logga in**, och ange dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="c4b40-126">In the **Azure Sign In** dialog box, select **Sign in**, and then enter your Azure credentials.</span></span>

    ![Dialogrutan Azure logga In](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-2.png)

4. <span data-ttu-id="c4b40-128">När du har loggat in, den **Välj prenumerationer** i dialogrutan visas alla de Azure-prenumerationer som är associerade med autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c4b40-128">After you're signed in, the **Select Subscriptions** dialog box lists all the Azure subscriptions that are associated with the credentials.</span></span> <span data-ttu-id="c4b40-129">Välj den **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="c4b40-129">Select the **Select** button.</span></span>

    ![Dialogrutan Välj prenumerationer](./media/hdinsight-apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

5. <span data-ttu-id="c4b40-131">På den **Azure Explorer** fliken, expandera **HDInsight** att visa HDInsight Spark-kluster som finns i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c4b40-131">On the **Azure Explorer** tab, expand **HDInsight** to view the HDInsight Spark clusters that are in your subscription.</span></span>
   
    ![HDInsight Spark-kluster i Azure Explorer](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-3.png)

6. <span data-ttu-id="c4b40-133">Om du vill visa de resurser (till exempel lagringskonton) som är associerade med klustret kan du ytterligare expandera en nod i klustret-namn.</span><span class="sxs-lookup"><span data-stu-id="c4b40-133">To view the resources (for example, storage accounts) that are associated with the cluster, you can further expand a cluster-name node.</span></span>
   
    ![En expanderad klusternamnet nod](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="c4b40-135">Kör en Spark Scala på ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="c4b40-135">Run a Spark Scala application on an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="c4b40-136">Starta IntelliJ IDEA och sedan skapa ett projekt.</span><span class="sxs-lookup"><span data-stu-id="c4b40-136">Start IntelliJ IDEA, and then create a project.</span></span> <span data-ttu-id="c4b40-137">I den **nytt projekt** dialogrutan Gör följande:</span><span class="sxs-lookup"><span data-stu-id="c4b40-137">In the **New Project** dialog box, do the following:</span></span> 

   <span data-ttu-id="c4b40-138">a.</span><span class="sxs-lookup"><span data-stu-id="c4b40-138">a.</span></span> <span data-ttu-id="c4b40-139">Välj **HDInsight** > **Spark i HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-139">Select **HDInsight** > **Spark on HDInsight (Scala)**.</span></span>

   <span data-ttu-id="c4b40-140">b.</span><span class="sxs-lookup"><span data-stu-id="c4b40-140">b.</span></span> <span data-ttu-id="c4b40-141">I den **Build verktyget** väljer du något av följande enligt dina behov:</span><span class="sxs-lookup"><span data-stu-id="c4b40-141">In the **Build tool** list, select either of the following, according to your need:</span></span>

      * <span data-ttu-id="c4b40-142">**Maven**, Scala skapa projekt guiden support</span><span class="sxs-lookup"><span data-stu-id="c4b40-142">**Maven**, for Scala project-creation wizard support</span></span>
      * <span data-ttu-id="c4b40-143">**Segregerade Barlasttankar**, för att hantera beroenden och skapa för Scala-projekt</span><span class="sxs-lookup"><span data-stu-id="c4b40-143">**SBT**, for managing the dependencies and building for the Scala project</span></span>

    ![Dialogrutan Nytt projekt](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

2. <span data-ttu-id="c4b40-145">Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-145">Select **Next**.</span></span>

3. <span data-ttu-id="c4b40-146">Guiden Skapa projekt Scala identifierar automatiskt om du har installerat Scala plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="c4b40-146">The Scala project-creation wizard automatically detects whether you've installed the Scala plug-in.</span></span> <span data-ttu-id="c4b40-147">Välj **installera**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-147">Select **Install**.</span></span>

   ![Kontroll av Scala plugin-program](./media/hdinsight-apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. <span data-ttu-id="c4b40-149">Om du vill hämta Scala plugin-program, Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-149">To download the Scala plug-in, select **OK**.</span></span> <span data-ttu-id="c4b40-150">Följ instruktionerna för att starta om IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="c4b40-150">Follow the instructions to restart IntelliJ.</span></span> 

   ![Dialogrutan Scala plugin-programmet för installation](./media/hdinsight-apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. <span data-ttu-id="c4b40-152">I den **nytt projekt** fönster, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="c4b40-152">In the **New Project** window, do the following:</span></span>  

    ![Att välja Spark SDK](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-new-project.png)

   <span data-ttu-id="c4b40-154">a.</span><span class="sxs-lookup"><span data-stu-id="c4b40-154">a.</span></span> <span data-ttu-id="c4b40-155">Ange ett namn och plats.</span><span class="sxs-lookup"><span data-stu-id="c4b40-155">Enter a project name and location.</span></span>

   <span data-ttu-id="c4b40-156">b.</span><span class="sxs-lookup"><span data-stu-id="c4b40-156">b.</span></span> <span data-ttu-id="c4b40-157">I den **projekt SDK** listrutan, Välj **Java 1.8** för 2.x Spark-kluster eller välj **Java 1.7** för 1.x Spark-klustret.</span><span class="sxs-lookup"><span data-stu-id="c4b40-157">In the **Project SDK** drop-down list, select **Java 1.8** for the Spark 2.x cluster, or select **Java 1.7** for the Spark 1.x cluster.</span></span>

   <span data-ttu-id="c4b40-158">c.</span><span class="sxs-lookup"><span data-stu-id="c4b40-158">c.</span></span> <span data-ttu-id="c4b40-159">I den **Spark version** listrutan Scala guide för att skapa projektet integreras rätt version för Spark SDK och Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="c4b40-159">In the **Spark version** drop-down list, Scala project creation wizard integrates the proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="c4b40-160">Om den Spark-kluster av versionen är äldre än 2.0 väljer **Väck 1.x**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-160">If the Spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="c4b40-161">Annars väljer **Spark2.x**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-161">Otherwise, select **Spark2.x**.</span></span> <span data-ttu-id="c4b40-162">Det här exemplet används **Spark punkt 2.0.2 (Scala 2.11.8)**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-162">This example uses **Spark 2.0.2 (Scala 2.11.8)**.</span></span>

6. <span data-ttu-id="c4b40-163">Välj **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-163">Select **Finish**.</span></span>

7. <span data-ttu-id="c4b40-164">Spark-projekt skapas automatiskt en artefakt.</span><span class="sxs-lookup"><span data-stu-id="c4b40-164">The Spark project automatically creates an artifact for you.</span></span> <span data-ttu-id="c4b40-165">Om du vill visa artefakten gör du följande:</span><span class="sxs-lookup"><span data-stu-id="c4b40-165">To view the artifact, do the following:</span></span>

   <span data-ttu-id="c4b40-166">a.</span><span class="sxs-lookup"><span data-stu-id="c4b40-166">a.</span></span> <span data-ttu-id="c4b40-167">På den **filen** väljer du **projektstruktur**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-167">On the **File** menu, select **Project Structure**.</span></span>

   <span data-ttu-id="c4b40-168">b.</span><span class="sxs-lookup"><span data-stu-id="c4b40-168">b.</span></span> <span data-ttu-id="c4b40-169">I den **projektstruktur** dialogrutan **artefakter** att visa den standard-artefakt som har skapats.</span><span class="sxs-lookup"><span data-stu-id="c4b40-169">In the **Project Structure** dialog box, select **Artifacts** to view the default artifact that is created.</span></span> <span data-ttu-id="c4b40-170">Du kan också skapa egna artefakt genom att välja på plustecknet (**+**).</span><span class="sxs-lookup"><span data-stu-id="c4b40-170">You can also create your own artifact by selecting the plus sign (**+**).</span></span>

      ![Artefakt information i dialogrutan](./media/hdinsight-apache-spark-intellij-tool-plugin/default-artifact.png)
      
8. <span data-ttu-id="c4b40-172">Lägg till din programmets källkod genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="c4b40-172">Add your application source code by doing the following:</span></span>

   <span data-ttu-id="c4b40-173">a.</span><span class="sxs-lookup"><span data-stu-id="c4b40-173">a.</span></span> <span data-ttu-id="c4b40-174">Högerklicka i Projektutforskaren **src**, peka på **ny**, och välj sedan **Scala klass**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-174">In Project Explorer, right-click **src**, point to **New**, and then select **Scala Class**.</span></span>
      
      ![Kommandon för att skapa en Scala-klass från Projektutforskaren](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   <span data-ttu-id="c4b40-176">b.</span><span class="sxs-lookup"><span data-stu-id="c4b40-176">b.</span></span> <span data-ttu-id="c4b40-177">I den **skapa nya Scala klass** dialogrutan, ange ett namn, väljer **objekt** i den **typ** och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-177">In the **Create New Scala Class** dialog box, provide a name, select **Object** in the **Kind** box, and then select **OK**.</span></span>
      
      ![Skapa ny Scala klass dialogruta](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   <span data-ttu-id="c4b40-179">c.</span><span class="sxs-lookup"><span data-stu-id="c4b40-179">c.</span></span> <span data-ttu-id="c4b40-180">I den **MyClusterApp.scala** fil, klistra in följande kod.</span><span class="sxs-lookup"><span data-stu-id="c4b40-180">In the **MyClusterApp.scala** file, paste the following code.</span></span> <span data-ttu-id="c4b40-181">Kod som läser data från HVAC.csv (finns i alla HDInsight Spark-kluster), hämtar de rader som har endast en siffra i sjunde kolumn i CSV-filen och skriver utdata till **/HVACOut** under standardbehållare för lagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="c4b40-181">The code reads the data from HVAC.csv (available on all HDInsight Spark clusters), retrieves the rows that have only one digit in the seventh column in the CSV file, and writes the output to **/HVACOut** under the default storage container for the cluster.</span></span>

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object MyClusterApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find the rows that have only one digit in the seventh column in the CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasb:///HVACOut")
            }
    
        }

9. <span data-ttu-id="c4b40-182">Kör programmet på ett HDInsight Spark-kluster genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="c4b40-182">Run the application on an HDInsight Spark cluster by doing the following:</span></span>

   <span data-ttu-id="c4b40-183">a.</span><span class="sxs-lookup"><span data-stu-id="c4b40-183">a.</span></span> <span data-ttu-id="c4b40-184">Högerklicka på projektnamnet i Projektutforskaren och välj sedan **skicka Spark-program till HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-184">In Project Explorer, right-click the project name, and then select **Submit Spark Application to HDInsight**.</span></span>
      
      ![Programmet skicka Spark på HDInsight-kommando](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

   <span data-ttu-id="c4b40-186">b.</span><span class="sxs-lookup"><span data-stu-id="c4b40-186">b.</span></span> <span data-ttu-id="c4b40-187">Du uppmanas att ange dina autentiseringsuppgifter för Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c4b40-187">You are prompted to enter your Azure subscription credentials.</span></span> <span data-ttu-id="c4b40-188">I den **Spark skicka** dialogrutan, ange följande värden och välj sedan **skicka**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-188">In the **Spark Submission** dialog box, provide the following values, and then select **Submit**.</span></span>
      
      * <span data-ttu-id="c4b40-189">För **Väck kluster (endast Linux)**, Välj HDInsight Spark-kluster som du vill köra programmet.</span><span class="sxs-lookup"><span data-stu-id="c4b40-189">For **Spark clusters (Linux only)**, select the HDInsight Spark cluster on which you want to run your application.</span></span>

      * <span data-ttu-id="c4b40-190">Välj en artefakt IntelliJ projektet eller välj en från hårddisken.</span><span class="sxs-lookup"><span data-stu-id="c4b40-190">Select an artifact from the IntelliJ project, or select one from the hard drive.</span></span>

      * <span data-ttu-id="c4b40-191">I den **Main klassnamn** väljer du de tre punkterna (**...** ), Välj den huvudsakliga klassen i din programkod för källa och väljer sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-191">In the **Main class name** box, select the ellipsis (**...**), select the main class in your application source code, and then select **OK**.</span></span>

        ![Dialogrutan Välj Main-klass](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)

      * <span data-ttu-id="c4b40-193">Eftersom programkoden i det här exemplet inte kräver kommandoradsargument eller referera burkar eller filer, kan du lämna rutorna återstående tomt.</span><span class="sxs-lookup"><span data-stu-id="c4b40-193">Because the application code in this example does not require command-line arguments or reference JARs or files, you can leave the remaining boxes empty.</span></span> <span data-ttu-id="c4b40-194">När du har angett all information bör i dialogrutan likna följande bild.</span><span class="sxs-lookup"><span data-stu-id="c4b40-194">After you provide all the information, the dialog box should resemble the following image.</span></span>
        
        ![Dialogrutan Skicka Spark](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

   <span data-ttu-id="c4b40-196">c.</span><span class="sxs-lookup"><span data-stu-id="c4b40-196">c.</span></span> <span data-ttu-id="c4b40-197">Den **Spark skicka** fliken längst ned i fönstret ska börja visas förloppet.</span><span class="sxs-lookup"><span data-stu-id="c4b40-197">The **Spark Submission** tab at the bottom of the window should start displaying the progress.</span></span> <span data-ttu-id="c4b40-198">Du kan också avbryta programmet genom att välja knappen red i den **Spark skicka** fönster.</span><span class="sxs-lookup"><span data-stu-id="c4b40-198">You can also stop the application by selecting the red button in the **Spark Submission** window.</span></span>
      
      ![Fönstret Spark överföring](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      <span data-ttu-id="c4b40-200">Information om hur du kommer åt jobbutdata finns i ”åtkomst och hantera HDInsight Spark-kluster med hjälp av Azure Toolkit för IntelliJ” senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c4b40-200">To learn how to access the job output, see the "Access and manage HDInsight Spark clusters by using Azure Toolkit for IntelliJ" section later in this article.</span></span>

## <a name="run-or-debug-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="c4b40-201">Köra eller felsöka ett Spark Scala-program på ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="c4b40-201">Run or debug a Spark Scala application on an HDInsight Spark cluster</span></span>
<span data-ttu-id="c4b40-202">Vi rekommenderar också ett annat sätt att skicka Spark-program i klustret.</span><span class="sxs-lookup"><span data-stu-id="c4b40-202">We also recommend another way of submitting the Spark application to the cluster.</span></span> <span data-ttu-id="c4b40-203">Du kan göra det genom att ange parametrarna i den **kör/Debug konfigurationer** IDE.</span><span class="sxs-lookup"><span data-stu-id="c4b40-203">You can do so by setting the parameters in the **Run/Debug configurations** IDE.</span></span> <span data-ttu-id="c4b40-204">Mer information finns i [felsöka Spark-program på ett HDInsight-kluster med Azure Toolkit för IntelliJ via SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span><span class="sxs-lookup"><span data-stu-id="c4b40-204">For more information, see [Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span></span>

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a><span data-ttu-id="c4b40-205">Komma åt och hantera HDInsight Spark-kluster med hjälp av Azure Toolkit för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="c4b40-205">Access and manage HDInsight Spark clusters by using Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="c4b40-206">Du kan utföra olika åtgärder med hjälp av Azure Toolkit för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="c4b40-206">You can perform various operations by using Azure Toolkit for IntelliJ.</span></span>

### <a name="access-the-job-view"></a><span data-ttu-id="c4b40-207">Åtkomst till vyn jobb</span><span class="sxs-lookup"><span data-stu-id="c4b40-207">Access the job view</span></span>
1. <span data-ttu-id="c4b40-208">I Azure Explorer expanderar **HDInsight**, expandera klusternamnet Spark och välj sedan **jobb**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-208">In Azure Explorer, expand **HDInsight**, expand the Spark cluster name, and then select **Jobs**.</span></span>  

    ![Jobbet visa nod](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. <span data-ttu-id="c4b40-210">I den högra rutan i **Spark jobbet vyn** visar alla program som kördes på klustret.</span><span class="sxs-lookup"><span data-stu-id="c4b40-210">In the right pane, the **Spark Job View** tab displays all the applications that were run on the cluster.</span></span> <span data-ttu-id="c4b40-211">Välj namnet på programmet som du vill se mer information.</span><span class="sxs-lookup"><span data-stu-id="c4b40-211">Select the name of the application for which you want to see more details.</span></span>

    ![Programinformation](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)

3. <span data-ttu-id="c4b40-213">Hovra över jobb diagrammet om du vill visa grundläggande jobbinformation om körs.</span><span class="sxs-lookup"><span data-stu-id="c4b40-213">To display basic running job information, hover over the job graph.</span></span> <span data-ttu-id="c4b40-214">Välj en nod på jobbet diagrammet om du vill visa steg diagram och information som genereras av varje jobb.</span><span class="sxs-lookup"><span data-stu-id="c4b40-214">To view the stages graph and information that every job generates, select a node on the job graph.</span></span>

    ![Steget jobbinformation](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. <span data-ttu-id="c4b40-216">Visa ofta används loggar, t.ex *drivrutinen Stderr*, *drivrutinen Stdout*, och *Directory Info*, väljer den **loggen** fliken.</span><span class="sxs-lookup"><span data-stu-id="c4b40-216">To view frequently used logs, such as *Driver Stderr*, *Driver Stdout*, and *Directory Info*, select the **Log** tab.</span></span>

    ![Logginformation](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)

5. <span data-ttu-id="c4b40-218">Du kan också visa Spark historik UI och YARN-Användargränssnittet (på programnivå) genom att välja en länk längst upp i fönstret.</span><span class="sxs-lookup"><span data-stu-id="c4b40-218">You can also view the Spark history UI and the YARN UI (at the application level) by selecting a link at the top of the window.</span></span>

### <a name="access-the-spark-history-server"></a><span data-ttu-id="c4b40-219">Åtkomst till servern för Spark-historik</span><span class="sxs-lookup"><span data-stu-id="c4b40-219">Access the Spark history server</span></span>
1. <span data-ttu-id="c4b40-220">I Azure Explorer expanderar **HDInsight**, högerklickar du på klusternamnet Spark och väljer sedan **öppna Spark historik UI**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-220">In Azure Explorer, expand **HDInsight**, right-click your Spark cluster name, and then select **Open Spark History UI**.</span></span> 

2. <span data-ttu-id="c4b40-221">När du uppmanas ange administratörsautentiseringsuppgifter för det kluster som du angav när du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="c4b40-221">When you're prompted, enter the cluster's admin credentials, which you specified when you set up the cluster.</span></span>

3. <span data-ttu-id="c4b40-222">Du kan använda namnet på programmet för att söka efter programmet just avslutats körs på instrumentpanelen Spark historik server.</span><span class="sxs-lookup"><span data-stu-id="c4b40-222">On the Spark history server dashboard, you can use the application name to look for the application that you just finished running.</span></span> <span data-ttu-id="c4b40-223">I föregående kod, ange namnet på programmet med `val conf = new SparkConf().setAppName("MyClusterApp")`.</span><span class="sxs-lookup"><span data-stu-id="c4b40-223">In the preceding code, you set the application name by using `val conf = new SparkConf().setAppName("MyClusterApp")`.</span></span> <span data-ttu-id="c4b40-224">Programnamnet Spark är därför **MyClusterApp**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-224">Therefore, your Spark application name is **MyClusterApp**.</span></span>

### <a name="start-the-ambari-portal"></a><span data-ttu-id="c4b40-225">Starta Ambari-portal</span><span class="sxs-lookup"><span data-stu-id="c4b40-225">Start the Ambari portal</span></span>
1. <span data-ttu-id="c4b40-226">I Azure Explorer expanderar **HDInsight**, högerklickar du på klusternamnet Spark och väljer sedan **öppna klustret hanteringsportalen (Ambari)**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-226">In Azure Explorer, expand **HDInsight**, right-click your Spark cluster name, and then select **Open Cluster Management Portal (Ambari)**.</span></span> 

2. <span data-ttu-id="c4b40-227">När du uppmanas ange administratörsautentiseringsuppgifterna för klustret.</span><span class="sxs-lookup"><span data-stu-id="c4b40-227">When you're prompted, enter the admin credentials for the cluster.</span></span> <span data-ttu-id="c4b40-228">Du har angett autentiseringsuppgifterna under klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="c4b40-228">You specified these credentials during the cluster setup process.</span></span>

### <a name="manage-azure-subscriptions"></a><span data-ttu-id="c4b40-229">Hantera Azure-prenumerationer</span><span class="sxs-lookup"><span data-stu-id="c4b40-229">Manage Azure subscriptions</span></span>
<span data-ttu-id="c4b40-230">Som standard visar Azure Toolkit för IntelliJ Spark-kluster från alla dina Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="c4b40-230">By default, Azure Toolkit for IntelliJ lists the Spark clusters from all your Azure subscriptions.</span></span> <span data-ttu-id="c4b40-231">Om det behövs kan du ange prenumerationer som du vill komma åt.</span><span class="sxs-lookup"><span data-stu-id="c4b40-231">If necessary, you can specify the subscriptions that you want to access.</span></span> 

1. <span data-ttu-id="c4b40-232">I Azure Explorer högerklickar du på den **Azure** rot nod och välj sedan **hantera prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-232">In Azure Explorer, right-click the **Azure** root node, and then select **Manage Subscriptions**.</span></span> 

2. <span data-ttu-id="c4b40-233">I dialogrutan avmarkerar du kryssrutorna bredvid de prenumerationer som du inte vill att komma åt och välj sedan **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-233">In the dialog box, clear the check boxes next to the subscriptions that you don't want to access, and then select **Close**.</span></span> <span data-ttu-id="c4b40-234">Du kan också välja **logga ut** om du vill logga ut från din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c4b40-234">You can also select **Sign Out** if you want to sign out of your Azure subscription.</span></span>

## <a name="run-a-spark-scala-application-locally"></a><span data-ttu-id="c4b40-235">Köra ett program med Spark Scala lokalt</span><span class="sxs-lookup"><span data-stu-id="c4b40-235">Run a Spark Scala application locally</span></span>
<span data-ttu-id="c4b40-236">Du kan använda Azure Toolkit för IntelliJ för att köra Spark Scala program lokalt på din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="c4b40-236">You can use Azure Toolkit for IntelliJ to run Spark Scala applications locally on your workstation.</span></span> <span data-ttu-id="c4b40-237">Programmen vanligtvis behöver inte åtkomst till resurser i klustret, till exempel behållare för lagring, och du kan köra och testa dem lokalt.</span><span class="sxs-lookup"><span data-stu-id="c4b40-237">The applications usually don't need access to cluster resources, such as storage containers, and you can run and test them locally.</span></span>

### <a name="prerequisite"></a><span data-ttu-id="c4b40-238">Krav</span><span class="sxs-lookup"><span data-stu-id="c4b40-238">Prerequisite</span></span>
<span data-ttu-id="c4b40-239">När du kör programmet lokalt Spark Scala på en Windows-dator, kan du få ett undantag som beskrivs i [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356).</span><span class="sxs-lookup"><span data-stu-id="c4b40-239">While you're running the local Spark Scala application on a Windows computer, you might get an exception, as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span></span> <span data-ttu-id="c4b40-240">Undantaget inträffar eftersom WinUtils.exe saknas i Windows.</span><span class="sxs-lookup"><span data-stu-id="c4b40-240">The exception occurs because WinUtils.exe is missing on Windows.</span></span> 

<span data-ttu-id="c4b40-241">Du löser det här felet [ladda ned den körbara filen](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) till en plats som **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-241">To resolve this error, [download the executable](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) to a location such as **C:\WinUtils\bin**.</span></span> <span data-ttu-id="c4b40-242">Lägg sedan till miljövariabeln **HADOOP_HOME**, och ange värdet för variabeln för att **C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-242">Then, add the environment variable **HADOOP_HOME**, and set the value of the variable to **C\WinUtils**.</span></span>

### <a name="run-a-local-spark-scala-application"></a><span data-ttu-id="c4b40-243">Kör ett lokalt Spark Scala-program</span><span class="sxs-lookup"><span data-stu-id="c4b40-243">Run a local Spark Scala application</span></span>
1. <span data-ttu-id="c4b40-244">Starta IntelliJ IDEA och skapa ett projekt.</span><span class="sxs-lookup"><span data-stu-id="c4b40-244">Start IntelliJ IDEA, and create a project.</span></span> 

2. <span data-ttu-id="c4b40-245">I den **nytt projekt** dialogrutan Gör följande:</span><span class="sxs-lookup"><span data-stu-id="c4b40-245">In the **New Project** dialog box, do the following:</span></span>
   
    <span data-ttu-id="c4b40-246">a.</span><span class="sxs-lookup"><span data-stu-id="c4b40-246">a.</span></span> <span data-ttu-id="c4b40-247">Välj **HDInsight** > **Spark på HDInsight lokala kör sampel (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-247">Select **HDInsight** > **Spark on HDInsight Local Run Sample (Scala)**.</span></span>

    <span data-ttu-id="c4b40-248">b.</span><span class="sxs-lookup"><span data-stu-id="c4b40-248">b.</span></span> <span data-ttu-id="c4b40-249">I den **Build verktyget** väljer du något av följande enligt dina behov:</span><span class="sxs-lookup"><span data-stu-id="c4b40-249">In the **Build tool** list, select either of the following, according to your need:</span></span>

      * <span data-ttu-id="c4b40-250">**Maven**, Scala skapa projekt guiden support</span><span class="sxs-lookup"><span data-stu-id="c4b40-250">**Maven**, for Scala project-creation wizard support</span></span>
      * <span data-ttu-id="c4b40-251">**Segregerade Barlasttankar**, för att hantera beroenden och skapa för Scala-projekt</span><span class="sxs-lookup"><span data-stu-id="c4b40-251">**SBT**, for managing the dependencies and building for the Scala project</span></span>

    ![Dialogrutan Nytt projekt](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run.png)

3. <span data-ttu-id="c4b40-253">Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-253">Select **Next**.</span></span>
 
4. <span data-ttu-id="c4b40-254">I fönstret nästa gör du följande:</span><span class="sxs-lookup"><span data-stu-id="c4b40-254">In the next window, do the following:</span></span>
   
    <span data-ttu-id="c4b40-255">a.</span><span class="sxs-lookup"><span data-stu-id="c4b40-255">a.</span></span> <span data-ttu-id="c4b40-256">Ange ett namn och plats.</span><span class="sxs-lookup"><span data-stu-id="c4b40-256">Enter a project name and location.</span></span>

    <span data-ttu-id="c4b40-257">b.</span><span class="sxs-lookup"><span data-stu-id="c4b40-257">b.</span></span> <span data-ttu-id="c4b40-258">I den **projekt SDK** listrutan väljer du en Java-version som är senare än version 1.7.</span><span class="sxs-lookup"><span data-stu-id="c4b40-258">In the **Project SDK** drop-down list, select a Java version that's later than version 1.7.</span></span>

    <span data-ttu-id="c4b40-259">c.</span><span class="sxs-lookup"><span data-stu-id="c4b40-259">c.</span></span> <span data-ttu-id="c4b40-260">I den **Spark Version** nedrullningsbara listan, Välj versionen av Scala som du vill använda: Scala 2.11.x för Spark 2.0 eller Scala 2.10.x för Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="c4b40-260">In the **Spark Version** drop-down list, select the version of Scala that you want to use: Scala 2.11.x for Spark 2.0 or Scala 2.10.x for Spark 1.6.</span></span>

    ![Dialogrutan Nytt projekt](./media/hdinsight-apache-spark-intellij-tool-plugin/Create-local-project.PNG)

5. <span data-ttu-id="c4b40-262">Välj **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-262">Select **Finish**.</span></span>

6. <span data-ttu-id="c4b40-263">Mallen lägger till en exempelkod (**LogQuery**) under den **src** mapp som du kan köra lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="c4b40-263">The template adds a sample code (**LogQuery**) under the **src** folder that you can run locally on your computer.</span></span>
   
    ![Platsen för LogQuery](./media/hdinsight-apache-spark-intellij-tool-plugin/local-app.png)

7. <span data-ttu-id="c4b40-265">Högerklicka på den **LogQuery** program och välj sedan **kör 'LogQuery'**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-265">Right-click the **LogQuery** application, and then select **Run 'LogQuery'**.</span></span> <span data-ttu-id="c4b40-266">På den **kör** fliken längst ned kan du se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="c4b40-266">On the **Run** tab at the bottom, you see an output like the following:</span></span>
   
   ![Spark programmet lokala kör resultat](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a><span data-ttu-id="c4b40-268">Konvertera befintliga IntelliJ IDEA program att använda Azure Toolkit för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="c4b40-268">Convert existing IntelliJ IDEA applications to use Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="c4b40-269">Du kan konvertera befintliga Spark Scala program som du skapade i IntelliJ IDEA till att vara kompatibel med Azure Toolkit för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="c4b40-269">You can convert the existing Spark Scala applications that you created in IntelliJ IDEA to be compatible with Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="c4b40-270">Du kan sedan använda plugin-programmet för att skicka program till ett HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="c4b40-270">You can then use the plug-in to submit the applications to an HDInsight Spark cluster.</span></span>

1. <span data-ttu-id="c4b40-271">Öppna filen associerade .iml för ett befintligt Spark Scala program som har skapats via IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="c4b40-271">For an existing Spark Scala application that was created through IntelliJ IDEA, open the associated .iml file.</span></span>

2. <span data-ttu-id="c4b40-272">Rot är en **modulen** följande element:</span><span class="sxs-lookup"><span data-stu-id="c4b40-272">At the root level is a **module** element like the following:</span></span>
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   <span data-ttu-id="c4b40-273">Redigera elementet så att lägga till `UniqueKey="HDInsightTool"` så att den **modulen** ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="c4b40-273">Edit the element to add `UniqueKey="HDInsightTool"` so that the **module** element looks like the following:</span></span>
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

3. <span data-ttu-id="c4b40-274">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="c4b40-274">Save the changes.</span></span> <span data-ttu-id="c4b40-275">Ditt program bör nu vara kompatibla med Azure Toolkit för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="c4b40-275">Your application should now be compatible with Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="c4b40-276">Du kan testa den genom att högerklicka på projektnamnet i Projektutforskaren.</span><span class="sxs-lookup"><span data-stu-id="c4b40-276">You can test it by right-clicking the project name in Project Explorer.</span></span> <span data-ttu-id="c4b40-277">Snabbmenyn har nu alternativet **skicka Spark-program till HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="c4b40-277">The pop-up menu now has the option **Submit Spark Application to HDInsight**.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c4b40-278">Felsökning</span><span class="sxs-lookup"><span data-stu-id="c4b40-278">Troubleshooting</span></span>

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a><span data-ttu-id="c4b40-279">Fel vid lokal körning: *Använd en större heapstorlek*</span><span class="sxs-lookup"><span data-stu-id="c4b40-279">Error in local run: *Please use a larger heap size*</span></span>
<span data-ttu-id="c4b40-280">I Spark 1.6 om du använder en 32-bitars Java SDK under lokal körning kan uppstå följande fel:</span><span class="sxs-lookup"><span data-stu-id="c4b40-280">In Spark 1.6, if you're using a 32-bit Java SDK during local run, you might encounter the following errors:</span></span>

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

<span data-ttu-id="c4b40-281">Dessa fel inträffa eftersom heap-storlek inte är tillräckligt stor för Spark ska köras.</span><span class="sxs-lookup"><span data-stu-id="c4b40-281">These errors happen because the heap size is not large enough for Spark to run.</span></span> <span data-ttu-id="c4b40-282">Spark kräver minst 471 MB.</span><span class="sxs-lookup"><span data-stu-id="c4b40-282">Spark requires at least 471 MB.</span></span> <span data-ttu-id="c4b40-283">(Mer information finns i [SPARK 12081](https://issues.apache.org/jira/browse/SPARK-12081).) En enkel lösning är att använda en 64-bitars Java SDK.</span><span class="sxs-lookup"><span data-stu-id="c4b40-283">(For more information, see [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081).) One simple solution is to use a 64-bit Java SDK.</span></span> <span data-ttu-id="c4b40-284">Du kan också ändra inställningarna för JVM i IntelliJ genom att lägga till följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="c4b40-284">You can also change the JVM settings in IntelliJ by adding the following options:</span></span>

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Lägga till alternativ i rutan ”alternativ för virtuell dator” i IntelliJ](./media/hdinsight-apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a><span data-ttu-id="c4b40-286">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="c4b40-286">FAQ</span></span>
<span data-ttu-id="c4b40-287">För att skicka ett program till Azure Data Lake Store, Välj **interaktiv** läge under processen för Azure-inloggning.</span><span class="sxs-lookup"><span data-stu-id="c4b40-287">To submit an application to Azure Data Lake Store, choose **Interactive** mode during the Azure sign-in process.</span></span> <span data-ttu-id="c4b40-288">Om du väljer **automatisk** läge, du får ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="c4b40-288">If you select **Automated** mode, you can get an error.</span></span>

![Interaktiv inloggning](./media/hdinsight-apache-spark-intellij-tool-plugin/interative-signin.png)

<span data-ttu-id="c4b40-290">Nu ska löste vi problemet.</span><span class="sxs-lookup"><span data-stu-id="c4b40-290">Now, we resolved it.</span></span> <span data-ttu-id="c4b40-291">Du kan välja ett Azure Data Lake-kluster att skicka ditt program med valfri metod för inloggning.</span><span class="sxs-lookup"><span data-stu-id="c4b40-291">You can choose an Azure Data Lake Cluster to submit your application with any sign-in method.</span></span>

## <a name="feedback-and-known-issues"></a><span data-ttu-id="c4b40-292">Feedback och kända problem</span><span class="sxs-lookup"><span data-stu-id="c4b40-292">Feedback and known issues</span></span>
<span data-ttu-id="c4b40-293">För närvarande kan stöds visa Spark utdata direkt inte.</span><span class="sxs-lookup"><span data-stu-id="c4b40-293">Currently, viewing Spark outputs directly is not supported.</span></span>

<span data-ttu-id="c4b40-294">Om du har förslag eller feedback eller om du stöter på problem när du använder plugin-modulen, skicka e-post på hdivstool@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="c4b40-294">If you have any suggestions or feedback, or if you encounter any problems when you use this plug-in, email us at hdivstool@microsoft.com.</span></span>

## <span data-ttu-id="c4b40-295"><a name="seealso"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c4b40-295"><a name="seealso"></a>Next steps</span></span>
* [<span data-ttu-id="c4b40-296">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="c4b40-296">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="c4b40-297">Demo</span><span class="sxs-lookup"><span data-stu-id="c4b40-297">Demo</span></span>
* <span data-ttu-id="c4b40-298">Skapa Scala projekt (video): [skapa Spark Scala-program](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="c4b40-298">Create Scala project (video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="c4b40-299">Fjärråtkomst debug (video): [Använd Azure Toolkit för IntelliJ till att felsöka Spark-program på HDInsight-kluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="c4b40-299">Remote debug (video): [Use Azure Toolkit for IntelliJ to debug Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="c4b40-300">Scenarier</span><span class="sxs-lookup"><span data-stu-id="c4b40-300">Scenarios</span></span>
* [<span data-ttu-id="c4b40-301">Spark med BI: utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="c4b40-301">Spark with BI: Perform interactive data analysis by using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="c4b40-302">Spark med Machine Learning: använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="c4b40-302">Spark with Machine Learning: Use Spark in HDInsight to analyze building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="c4b40-303">Spark med Machine Learning: Använda Spark i HDInsight för att förutsäga resultatet av en livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="c4b40-303">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="c4b40-304">Spark Streaming: Använda Spark i HDInsight för att skapa realtid strömmade program</span><span class="sxs-lookup"><span data-stu-id="c4b40-304">Spark Streaming: Use Spark in HDInsight to build real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="c4b40-305">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="c4b40-305">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a><span data-ttu-id="c4b40-306">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="c4b40-306">Creating and running applications</span></span>
* [<span data-ttu-id="c4b40-307">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="c4b40-307">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="c4b40-308">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="c4b40-308">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="c4b40-309">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="c4b40-309">Tools and extensions</span></span>
* [<span data-ttu-id="c4b40-310">Använda Azure Toolkit för IntelliJ för att felsöka Spark-program via fjärranslutning via VPN</span><span class="sxs-lookup"><span data-stu-id="c4b40-310">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="c4b40-311">Använda Azure Toolkit för IntelliJ för att felsöka Spark-program via fjärranslutning via SSH</span><span class="sxs-lookup"><span data-stu-id="c4b40-311">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="c4b40-312">Använda HDInsight Tools för IntelliJ med Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="c4b40-312">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="c4b40-313">Använda HDInsight-verktyg i Azure Toolkit för Eclipse för att skapa Spark-program</span><span class="sxs-lookup"><span data-stu-id="c4b40-313">Use HDInsight Tools in Azure Toolkit for Eclipse to create Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="c4b40-314">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="c4b40-314">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="c4b40-315">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="c4b40-315">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="c4b40-316">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="c4b40-316">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="c4b40-317">Installera Jupyter på datorn och ansluta till ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="c4b40-317">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a><span data-ttu-id="c4b40-318">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="c4b40-318">Managing resources</span></span>
* [<span data-ttu-id="c4b40-319">Hantera resurser för Apache Spark-klustret i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="c4b40-319">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="c4b40-320">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="c4b40-320">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

