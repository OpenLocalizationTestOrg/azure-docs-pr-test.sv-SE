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
# <a name="use-azure-toolkit-for-eclipse-to-create-spark-applications-for-an-hdinsight-cluster"></a><span data-ttu-id="c4ad4-103">Använda Azure Toolkit för Eclipse för att skapa Spark-program för ett HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="c4ad4-103">Use Azure Toolkit for Eclipse to create Spark applications for an HDInsight cluster</span></span>

<span data-ttu-id="c4ad4-104">Använda HDInsight Tools i Azure Toolkit för Eclipse för att utveckla Spark-program som skrivits i Scala och skicka dem till ett Azure HDInsight Spark-kluster direkt från Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-104">Use HDInsight Tools in Azure Toolkit for Eclipse to develop Spark applications written in Scala and submit them to an Azure HDInsight Spark cluster, directly from the Eclipse IDE.</span></span> <span data-ttu-id="c4ad4-105">Du kan använda HDInsight Tools-plugin-programmet på några olika sätt:</span><span class="sxs-lookup"><span data-stu-id="c4ad4-105">You can use the HDInsight Tools plug-in in a few different ways:</span></span>

* <span data-ttu-id="c4ad4-106">Att utveckla och skicka Scala Spark-program på ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="c4ad4-106">To develop and submit a Scala Spark application on an HDInsight Spark cluster</span></span>
* <span data-ttu-id="c4ad4-107">Åtkomst till resurserna i Azure HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="c4ad4-107">To access your Azure HDInsight Spark cluster resources</span></span>
* <span data-ttu-id="c4ad4-108">Att utveckla och köra ett Scala Spark-program lokalt</span><span class="sxs-lookup"><span data-stu-id="c4ad4-108">To develop and run a Scala Spark application locally</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c4ad4-109">Det här verktyget kan användas för att skapa och skicka program bara för ett HDInsight Spark-kluster på Linux.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-109">This tool can be used to create and submit applications only for an HDInsight Spark cluster on Linux.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="c4ad4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c4ad4-110">Prerequisites</span></span>

* <span data-ttu-id="c4ad4-111">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-111">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="c4ad4-112">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="c4ad4-112">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="c4ad4-113">Oracle Java Development Kit version 8, som används för Eclipse IDE-körningsmiljön.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-113">Oracle Java Development Kit version 8, which is used for the Eclipse IDE runtime.</span></span> <span data-ttu-id="c4ad4-114">Du kan ladda ned det från den [Oracle webbplats](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="c4ad4-114">You can download it from the [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="c4ad4-115">Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-115">Eclipse IDE.</span></span> <span data-ttu-id="c4ad4-116">Den här artikeln använder Eclipse Neon.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-116">This article uses Eclipse Neon.</span></span> <span data-ttu-id="c4ad4-117">Du kan installera det från den [Eclipse webbplats](https://www.eclipse.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c4ad4-117">You can install it from the [Eclipse website](https://www.eclipse.org/downloads/).</span></span>   
* <span data-ttu-id="c4ad4-118">Spark SDK.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-118">Spark SDK.</span></span> <span data-ttu-id="c4ad4-119">Du kan ladda ned det från [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="c4ad4-119">You can download it from [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).</span></span>


## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-scala-plugin"></a><span data-ttu-id="c4ad4-120">Installera HDInsight-verktyg i Azure Toolkit för Eclipse och Scala-plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="c4ad4-120">Install HDInsight Tools in Azure Toolkit for Eclipse and Scala Plugin</span></span>
### <a name="install-hdinsight-tools"></a><span data-ttu-id="c4ad4-121">Installera HDInsight-verktyg</span><span class="sxs-lookup"><span data-stu-id="c4ad4-121">Install HDInsight Tools</span></span>
<span data-ttu-id="c4ad4-122">HDInsight-verktyg för Eclipse är tillgänglig som en del av Azure Toolkit för Eclipse.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-122">HDInsight Tools for Eclipse is available as part of Azure Toolkit for Eclipse.</span></span> <span data-ttu-id="c4ad4-123">Installationsanvisningar finns i [installerar Azure Toolkit för Eclipse](../azure-toolkit-for-eclipse-installation.md).</span><span class="sxs-lookup"><span data-stu-id="c4ad4-123">For installation instructions, see [Installing Azure Toolkit for Eclipse](../azure-toolkit-for-eclipse-installation.md).</span></span>
### <a name="install-scala-plugin"></a><span data-ttu-id="c4ad4-124">Installera Scala plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="c4ad4-124">Install Scala Plugin</span></span>
<span data-ttu-id="c4ad4-125">När du öppnar Intellij identifierar HDInsight Tools automatiskt om du har installerat Scala plugin-programmet eller inte.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-125">When you open the Intellij, the HDInsight Tools auto detects whether you installed Scala plugin or not.</span></span> <span data-ttu-id="c4ad4-126">Klicka på **OK** och följ instruktionerna för att installera genom Eclipse Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-126">Click **OK** to continue and follow the instructions to install by the Eclipse Marketplace.</span></span>

 ![Automatisk installation Scala plugin-program](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala.png)

## <a name="sign-in-to-your-azure-subscription"></a><span data-ttu-id="c4ad4-128">Logga in till din Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c4ad4-128">Sign in to your Azure subscription</span></span>
1. <span data-ttu-id="c4ad4-129">Starta Eclipse IDE och öppna Utforskaren i Azure.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-129">Start the Eclipse IDE and open Azure Explorer.</span></span> <span data-ttu-id="c4ad4-130">På den **fönstret** -menyn klickar du på **visa**, och klicka sedan på **andra**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-130">On the **Window** menu, click **Show View**, and then click **Other**.</span></span> <span data-ttu-id="c4ad4-131">I dialogrutan som öppnas, expandera **Azure**, klickar du på **Azure Explorer**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-131">In the dialog box that opens, expand **Azure**, click **Azure Explorer**, and then click **OK**.</span></span>

    ![Visa dialogrutan vy](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-1.png)
2. <span data-ttu-id="c4ad4-133">Högerklicka på den **Azure** noden och klicka sedan på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-133">Right-click the **Azure** node, and then click **Sign in**.</span></span>
3. <span data-ttu-id="c4ad4-134">I den **Azure logga In** dialogrutan Välj autentiseringsmetod, klickar du på **logga in**, och ange dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-134">In the **Azure Sign In** dialog box, choose the authentication method, click **Sign in**, and enter your Azure credentials.</span></span>
   
    ![Azure logga In dialogrutan](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-2.png)
4. <span data-ttu-id="c4ad4-136">När du har loggat in, den **Välj prenumerationer** i dialogrutan visas alla de Azure-prenumerationer som är associerade med autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-136">After you're signed in, the **Select Subscriptions** dialog box lists all the Azure subscriptions associated with the credentials.</span></span> <span data-ttu-id="c4ad4-137">Klicka på **Välj** att stänga dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-137">Click **Select** to close the dialog box.</span></span>

    ![Dialogrutan för prenumerationer](./media/hdinsight-apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
5. <span data-ttu-id="c4ad4-139">På den **Azure Explorer** fliken, expandera **HDInsight** att se HDInsight Spark-kluster i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-139">On the **Azure Explorer** tab, expand **HDInsight** to see the HDInsight Spark clusters under your subscription.</span></span>
   
    ![HDInsight Spark-kluster i Azure Explorer](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-3.png)
6. <span data-ttu-id="c4ad4-141">Ytterligare kan du expandera en nod i namn om du vill se de resurser (till exempel storage-konton) som associeras med klustret.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-141">You can further expand a cluster name node to see the resources (for example, storage accounts) associated with the cluster.</span></span>
   
    ![Expandera en klusternamnet finns resurser](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-4.png)



## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a><span data-ttu-id="c4ad4-143">Ställ in ett Spark Scala-projekt för ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="c4ad4-143">Set up a Spark Scala project for an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="c4ad4-144">I arbetsytan Eclipse IDE klickar du på **filen**, klickar du på **ny**, och klicka sedan på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-144">In the Eclipse IDE workspace, click **File**, click **New**, and then click **Project**.</span></span> 
2. <span data-ttu-id="c4ad4-145">Expandera i guiden Nytt projekt **HDInsight**väljer **Spark i HDInsight (Scala)**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-145">In the New Project wizard, expand **HDInsight**, select **Spark on HDInsight (Scala)**, and then click **Next**.</span></span>

    ![Att välja Spark i HDInsight (Scala)-projekt](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
3. <span data-ttu-id="c4ad4-147">Scala projekt skapa guiden automatiskt identifierar om du har installerat Scala plugin-programmet eller inte.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-147">The Scala project creation wizard auto detects whether you installed Scala plugin or not.</span></span> <span data-ttu-id="c4ad4-148">Klicka på **OK** om du vill fortsätta att ladda ned Scala plugin-programmet, följ instruktionerna för att starta om Eclipse.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-148">Click **OK** to continue downloading the Scala plugin, then follow the instructions to restart Eclipse.</span></span>

    ![Kontrollera scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
4. <span data-ttu-id="c4ad4-150">I den **nytt projekt för HDInsight Scala** dialogrutan, ange följande värden och klicka sedan på **nästa**:</span><span class="sxs-lookup"><span data-stu-id="c4ad4-150">In the **New HDInsight Scala Project** dialog box, provide the following values, and then click **Next**:</span></span>
   * <span data-ttu-id="c4ad4-151">Ange ett namn för projektet.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-151">Enter a name for the project.</span></span>
   * <span data-ttu-id="c4ad4-152">I den **JRE** område, se till att **använder en körningsmiljö JRE** är inställd på **JavaSE 1.7** eller senare.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-152">In the **JRE** area, make sure that **Use an execution environment JRE** is set to **JavaSE-1.7** or later.</span></span>
   * <span data-ttu-id="c4ad4-153">Kontrollera att Spark SDK anges till den plats där du sparade SDK.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-153">Make sure that Spark SDK is set to the location where you downloaded the SDK.</span></span> <span data-ttu-id="c4ad4-154">Länk till nedladdningsplatsen ingår i den [krav](#prerequisites) tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-154">The link to the download location is included in the [prerequisites](#prerequisites) earlier in this article.</span></span> <span data-ttu-id="c4ad4-155">Du kan också hämta SDK från länken som ingår i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-155">You can also download the SDK from the link included in the dialog box.</span></span>

    ![Dialogrutan Nytt projekt för HDInsight Scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
5.  <span data-ttu-id="c4ad4-157">I dialogrutan nästa klickar du på den **bibliotek** fliken Behåll standardvärdena och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-157">In the next dialog box, click the **Libraries** tab and keep the defaults, and then click **Finish**.</span></span> 
   
    ![Biblioteksfliken](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-4.png)
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a><span data-ttu-id="c4ad4-159">Skapa ett Scala program för ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="c4ad4-159">Create a Scala application for an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="c4ad4-160">I Eclipse-IDE från paketet Explorer, expandera projektet som du skapade tidigare, högerklicka på **src**, peka på **ny**, och klicka sedan på **andra**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-160">In the Eclipse IDE, from Package Explorer, expand the project that you created earlier, right-click **src**, point to **New**, and then click **Other**.</span></span>
2. <span data-ttu-id="c4ad4-161">I den **Välj en guide** dialogrutan Expandera **Scala guider**, klickar du på **Scala objekt**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-161">In the **Select a wizard** dialog box, expand **Scala Wizards**, click **Scala Object**, and then click **Next**.</span></span>
   
    ![Välj en dialogruta för guiden](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
3. <span data-ttu-id="c4ad4-163">I den **Skapa ny fil** dialogrutan, ange ett namn för objektet och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-163">In the **Create New File** dialog box, enter a name for the object, and then click **Finish**.</span></span>
   
    ![Skapa en ny fil dialogruta](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
4. <span data-ttu-id="c4ad4-165">Klistra in följande kod i textredigeraren:</span><span class="sxs-lookup"><span data-stu-id="c4ad4-165">Paste the following code in the text editor:</span></span>
   
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
5. <span data-ttu-id="c4ad4-166">Kör programmet på ett HDInsight Spark-kluster:</span><span class="sxs-lookup"><span data-stu-id="c4ad4-166">Run the application on an HDInsight Spark cluster:</span></span>
   
   1. <span data-ttu-id="c4ad4-167">Högerklicka på projektnamnet paketet Explorer och markera sedan **skicka Spark-program till HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-167">From Package Explorer, right-click the project name, and then select **Submit Spark Application to HDInsight**.</span></span>        
   2. <span data-ttu-id="c4ad4-168">I den **Spark skicka** dialogrutan, ange följande värden och klicka sedan på **skicka**:</span><span class="sxs-lookup"><span data-stu-id="c4ad4-168">In the **Spark Submission** dialog box, provide the following values, and then click **Submit**:</span></span>
      
      * <span data-ttu-id="c4ad4-169">För **klusternamnet**, Välj HDInsight Spark-kluster som du vill köra programmet.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-169">For **Cluster Name**, select the HDInsight Spark cluster on which you want to run your application.</span></span>
      * <span data-ttu-id="c4ad4-170">Välj en artefakt Eclipse-projektet, eller välja en från en hårddisk.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-170">Select an artifact from the Eclipse project, or select one from a hard drive.</span></span> <span data-ttu-id="c4ad4-171">Standardvärdet beror på det objekt du högerklickar på paketet Explorer.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-171">The default value depends on the item you right-click from package explorer.</span></span>
      * <span data-ttu-id="c4ad4-172">I den **Main klassnamn** dropdownlist skickas visas alla objektnamn från projektet valda.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-172">In the **Main class name** dropdownlist, submission wizard displays all object names from your selected project.</span></span> <span data-ttu-id="c4ad4-173">Välj eller ange en som du vill köra.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-173">Select or input one that you want to run.</span></span> <span data-ttu-id="c4ad4-174">Om du väljer artefakt från hårddisk måste du ange huvudsakliga klassnamn själv.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-174">If you select artifact from hard disk, you need input main class name by yourself.</span></span> 
      * <span data-ttu-id="c4ad4-175">Eftersom programkoden i det här exemplet inte kräver några kommandoradsargument eller referera burkar eller filer, kan du lämna textrutorna tomt.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-175">Because the application code in this example does not require any command-line arguments or reference JARs or files, you can leave the remaining text boxes empty.</span></span>
        
       ![Dialogrutan Skicka Spark](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
   3. <span data-ttu-id="c4ad4-177">Den **Spark skicka** fliken ska börja visas förloppet.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-177">The **Spark Submission** tab should start displaying the progress.</span></span> <span data-ttu-id="c4ad4-178">Du kan stoppa programmet genom att klicka på red i den **Spark skicka** fönster.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-178">You can stop the application by clicking the red button in the **Spark Submission** window.</span></span> <span data-ttu-id="c4ad4-179">Du kan också visa loggarna för specifika programmet kör genom att klicka på ikonen (betecknas med den blå rutan i bilden).</span><span class="sxs-lookup"><span data-stu-id="c4ad4-179">You can also view the logs for this specific application run by clicking the globe icon (denoted by the blue box in the image).</span></span>
      
       ![Skicka Spark-fönster](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a><span data-ttu-id="c4ad4-181">Komma åt och hantera HDInsight Spark-kluster med hjälp av HDInsight-verktyg i Azure Toolkit för Eclipse</span><span class="sxs-lookup"><span data-stu-id="c4ad4-181">Access and manage HDInsight Spark clusters by using HDInsight Tools in Azure Toolkit for Eclipse</span></span>
<span data-ttu-id="c4ad4-182">Du kan utföra olika åtgärder med hjälp av HDInsight-verktyg, inklusive åtkomst till jobbutdata.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-182">You can perform various operations by using HDInsight Tools, including accessing the job output.</span></span>

### <a name="access-the-job-view"></a><span data-ttu-id="c4ad4-183">Åtkomst till vyn jobb</span><span class="sxs-lookup"><span data-stu-id="c4ad4-183">Access the job view</span></span>
1. <span data-ttu-id="c4ad4-184">I Azure Explorer expanderar **HDInsight**, expandera klusternamnet Spark och klicka sedan på **jobb**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-184">In Azure Explorer, expand **HDInsight**, expand the Spark cluster name, and then click **Jobs**.</span></span> 

    ![Jobbet visa nod](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. <span data-ttu-id="c4ad4-186">Klicka på den **jobb** nod.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-186">Click on the **Jobs** node.</span></span> <span data-ttu-id="c4ad4-187">HDInsight Tools identifieras om du har installerat E (fx) clipse plugin-programmet eller inte.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-187">The HDInsight Tools auto-detects whether you installed the E(fx)clipse plugin or not.</span></span> <span data-ttu-id="c4ad4-188">Klicka på **OK** och följ instruktionerna för att installera Eclipse Marketplace och starta om Eclipse.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-188">Click **OK** to continue and follow the instructions to install the Eclipse Marketplace and restart Eclipse.</span></span>

    ![Installera clipse E (fx)](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

3. <span data-ttu-id="c4ad4-190">Öppna vyn jobb från de **jobb** nod.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-190">Open the Job View from the **Jobs** node.</span></span> <span data-ttu-id="c4ad4-191">I den högra rutan i **Spark jobbet vyn** visar alla program som kördes på klustret.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-191">In the right pane, the **Spark Job View** tab displays all the applications that were run on the cluster.</span></span> <span data-ttu-id="c4ad4-192">Klicka på namnet på programmet som du vill se mer information.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-192">Click the name of the application for which you want to see more details.</span></span>

    ![Programinformation](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)
4. <span data-ttu-id="c4ad4-194">Om du hovrar på jobbet diagrammet visar grundläggande körs Jobbinformationen.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-194">If you hover on the job graph, it displays basic running job info.</span></span> <span data-ttu-id="c4ad4-195">Visas klickar på jobbdiagram steg diagram och information som genereras av varje jobb.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-195">Clicking on the job graph shows the stages graph and info that every job generates.</span></span>

    ![Steget jobbinformation](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

5. <span data-ttu-id="c4ad4-197">Vanliga loggar, inklusive drivrutinen Stderr, drivrutinen Stdout och Directory information visas i den **loggen** fliken.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-197">Frequently used logs, including Driver Stderr, Driver Stdout, and Directory Info, are listed in the **Log** tab.</span></span>

    ![Logginformation](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)
6. <span data-ttu-id="c4ad4-199">Du kan också öppna Spark historik UI och YARN-Användargränssnittet (på programnivå) genom att klicka på respektive hyperlänken längst upp i fönstret.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-199">You can also open the Spark history UI and the YARN UI (at the application level) by clicking the respective hyperlink at the top of the window.</span></span>

### <a name="access-the-storage-container-for-the-cluster"></a><span data-ttu-id="c4ad4-200">Åtkomst till vilken lagringsbehållare som klustret</span><span class="sxs-lookup"><span data-stu-id="c4ad4-200">Access the storage container for the cluster</span></span>
1. <span data-ttu-id="c4ad4-201">I Azure Explorer expanderar den **HDInsight** rotnoden att se en lista över HDInsight Spark-kluster som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-201">In Azure Explorer, expand the **HDInsight** root node to see a list of HDInsight Spark clusters that are available.</span></span>
2. <span data-ttu-id="c4ad4-202">Expandera klusternamnet finns i lagringskontot och standardbehållare för lagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-202">Expand the cluster name to see the storage account and the default storage container for the cluster.</span></span>
   
    ![Storage-konto och standard lagringsbehållare](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-5.png)
3. <span data-ttu-id="c4ad4-204">Klicka på lagring behållarens namn som är associerade med klustret.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-204">Click the storage container name associated with the cluster.</span></span> <span data-ttu-id="c4ad4-205">I den högra rutan, dubbelklickar du på den **HVACOut** mapp.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-205">In the right pane, double-click the **HVACOut** folder.</span></span> <span data-ttu-id="c4ad4-206">Öppna en av de **del -** filer för att se utdata från programmet.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-206">Open one of the **part-** files to see the output of the application.</span></span>

### <a name="access-the-spark-history-server"></a><span data-ttu-id="c4ad4-207">Åtkomst till servern för Spark-historik</span><span class="sxs-lookup"><span data-stu-id="c4ad4-207">Access the Spark history server</span></span>
1. <span data-ttu-id="c4ad4-208">Högerklicka på klusternamnet Spark i Azure Explorer och markera **öppna Spark historik UI**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-208">In Azure Explorer, right-click your Spark cluster name, and then select **Open Spark History UI**.</span></span> <span data-ttu-id="c4ad4-209">När du uppmanas ange administratörsautentiseringsuppgifterna för klustret.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-209">When you're prompted, enter the admin credentials for the cluster.</span></span> <span data-ttu-id="c4ad4-210">Du måste ange dessa vid etablering av klustret.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-210">You must have specified these while provisioning the cluster.</span></span>
2. <span data-ttu-id="c4ad4-211">I instrumentpanelen Spark historik server använder du namnet på programmet för att söka efter programmet just avslutats körs.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-211">In the Spark history server dashboard, you use the application name to look for the application that you just finished running.</span></span> <span data-ttu-id="c4ad4-212">I föregående kod, ange namnet på programmet med `val conf = new SparkConf().setAppName("MyClusterApp")`.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-212">In the preceding code, you set the application name by using `val conf = new SparkConf().setAppName("MyClusterApp")`.</span></span> <span data-ttu-id="c4ad4-213">Därför programnamnet Spark har **MyClusterApp**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-213">Hence, your Spark application name was **MyClusterApp**.</span></span>

### <a name="start-the-ambari-portal"></a><span data-ttu-id="c4ad4-214">Starta Ambari-portal</span><span class="sxs-lookup"><span data-stu-id="c4ad4-214">Start the Ambari portal</span></span>
1. <span data-ttu-id="c4ad4-215">Högerklicka på klusternamnet Spark i Azure Explorer och markera **öppna klustret hanteringsportalen (Ambari)**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-215">In Azure Explorer, right-click your Spark cluster name, and then select **Open Cluster Management Portal (Ambari)**.</span></span> 
2. <span data-ttu-id="c4ad4-216">När du uppmanas ange administratörsautentiseringsuppgifterna för klustret.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-216">When you're prompted, enter the admin credentials for the cluster.</span></span> <span data-ttu-id="c4ad4-217">Du måste ange dessa vid etablering av klustret.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-217">You must have specified these while provisioning the cluster.</span></span>

### <a name="manage-azure-subscriptions"></a><span data-ttu-id="c4ad4-218">Hantera Azure-prenumerationer</span><span class="sxs-lookup"><span data-stu-id="c4ad4-218">Manage Azure subscriptions</span></span>
<span data-ttu-id="c4ad4-219">Som standard visar HDInsight-verktyg i Azure Toolkit för Eclipse Spark-kluster från alla dina Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-219">By default, HDInsight Tools in Azure Toolkit for Eclipse lists the Spark clusters from all your Azure subscriptions.</span></span> <span data-ttu-id="c4ad4-220">Om det behövs kan du ange prenumerationer som du vill ha åtkomst till klustret.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-220">If necessary, you can specify the subscriptions for which you want to access the cluster.</span></span> 

1. <span data-ttu-id="c4ad4-221">I Azure Explorer högerklickar du på den **Azure** rot noden och klicka sedan på **hantera prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-221">In Azure Explorer, right-click the **Azure** root node, and then click **Manage Subscriptions**.</span></span> 
2. <span data-ttu-id="c4ad4-222">Avmarkera kryssrutorna för den prenumeration som du inte vill få åtkomst till och klicka sedan på i dialogrutan **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-222">In the dialog box, clear the check boxes for the subscription that you don't want to access, and then click **Close**.</span></span> <span data-ttu-id="c4ad4-223">Du kan också klicka på **logga ut** om du vill logga ut från din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-223">You can also click **Sign Out** if you want to sign out of your Azure subscription.</span></span>

## <a name="run-a-spark-scala-application-locally"></a><span data-ttu-id="c4ad4-224">Köra ett program med Spark Scala lokalt</span><span class="sxs-lookup"><span data-stu-id="c4ad4-224">Run a Spark Scala application locally</span></span>
<span data-ttu-id="c4ad4-225">Du kan använda HDInsight Tools i Azure Toolkit för Eclipse för att köra Spark Scala program lokalt på din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-225">You can use HDInsight Tools in Azure Toolkit for Eclipse to run Spark Scala applications locally on your workstation.</span></span> <span data-ttu-id="c4ad4-226">Normalt programmen behöver inte åtkomst till resurser i klustret som en lagringsbehållare och du kan köra och testa dem lokalt.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-226">Typically, these applications don't need access to cluster resources such as a storage container, and you can run and test them locally.</span></span>

### <a name="prerequisite"></a><span data-ttu-id="c4ad4-227">Krav</span><span class="sxs-lookup"><span data-stu-id="c4ad4-227">Prerequisite</span></span>
<span data-ttu-id="c4ad4-228">När du kör programmet lokalt Spark Scala på en Windows-dator, kan du få ett undantag som beskrivs i [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356).</span><span class="sxs-lookup"><span data-stu-id="c4ad4-228">While you're running the local Spark Scala application on a Windows computer, you might get an exception as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span></span> <span data-ttu-id="c4ad4-229">Det här undantaget inträffar eftersom **WinUtils.exe** saknas i Windows.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-229">This exception occurs because **WinUtils.exe** is missing in Windows.</span></span> 

<span data-ttu-id="c4ad4-230">För att lösa det här felet måste du [ladda ned den körbara filen](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) till en plats som **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-230">To resolve this error, you must [download the executable](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) to a location like **C:\WinUtils\bin**.</span></span> <span data-ttu-id="c4ad4-231">Sedan måste du lägga till miljövariabeln **HADOOP_HOME** och ange värdet för variabeln för att **C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-231">You must then add the environment variable **HADOOP_HOME** and set the value of the variable to **C\WinUtils**.</span></span>

### <a name="run-a-local-spark-scala-application"></a><span data-ttu-id="c4ad4-232">Kör ett lokalt Spark Scala-program</span><span class="sxs-lookup"><span data-stu-id="c4ad4-232">Run a local Spark Scala application</span></span>
1. <span data-ttu-id="c4ad4-233">Starta Eclipse och skapa ett projekt.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-233">Start Eclipse and create a project.</span></span> <span data-ttu-id="c4ad4-234">I den **nytt projekt** dialogrutan följande alternativ och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-234">In the **New Project** dialog box, make the following choices, and then click **Next**.</span></span>
   
   * <span data-ttu-id="c4ad4-235">I den vänstra rutan, Välj **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-235">In the left pane, select **HDInsight**.</span></span>
   * <span data-ttu-id="c4ad4-236">I den högra rutan, Välj **Spark på HDInsight lokala kör sampel (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-236">In the right pane, select **Spark on HDInsight Local Run Sample (Scala)**.</span></span>

    ![Dialogrutan Nytt projekt](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
2. <span data-ttu-id="c4ad4-238">Följ steg 3 till 6 för att ge projektinformationen, från det tidigare avsnittet [Ställ in ett Spark Scala-projekt för ett HDInsight Spark-kluster](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).</span><span class="sxs-lookup"><span data-stu-id="c4ad4-238">To provide the project details, follow steps 3 through 6 from the earlier section [Set up a Spark Scala project for an HDInsight Spark cluster](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).</span></span>
3. <span data-ttu-id="c4ad4-239">Mallen lägger till en exempelkod (**LogQuery**) under den **src** mapp som du kan köra lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-239">The template adds a sample code (**LogQuery**) under the **src** folder that you can run locally on your computer.</span></span>
   
    ![Platsen för LogQuery](./media/hdinsight-apache-spark-eclipse-tool-plugin/local-app.png)
4. <span data-ttu-id="c4ad4-241">Högerklicka på den **LogQuery** program, peka på **kör som**, och klicka sedan på **1 Scala program**.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-241">Right-click the **LogQuery** application, point to **Run As**, and then click **1 Scala Application**.</span></span> <span data-ttu-id="c4ad4-242">Du kommer att se utdata som liknar detta i den **konsolen** längst ned:</span><span class="sxs-lookup"><span data-stu-id="c4ad4-242">You will see an output like this in the **Console** tab at the bottom:</span></span>
   
   ![Spark programmet lokala kör resultat](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="faq"></a><span data-ttu-id="c4ad4-244">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="c4ad4-244">FAQ</span></span>
<span data-ttu-id="c4ad4-245">För att skicka ett program till Azure Data Lake Store, Välj **interaktiv** läge under processen för Azure-inloggning.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-245">To submit an application to Azure Data Lake Store, choose **Interactive** mode during the Azure sign-in process.</span></span> <span data-ttu-id="c4ad4-246">Om du väljer **automatisk** läge, du får ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-246">If you select **Automated** mode, you can get an error.</span></span>

![Interaktiv inloggning](./media/hdinsight-apache-spark-eclipse-tool-plugin/interactive-authentication.png)

<span data-ttu-id="c4ad4-248">Nu ska löste vi problemet.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-248">Now, we resolved it.</span></span> <span data-ttu-id="c4ad4-249">Du kan välja ett Azure Data Lake-kluster att skicka ditt program med valfri metod för inloggning.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-249">You can choose an Azure Data Lake Cluster to submit your application with any sign-in method.</span></span>

## <a name="feedback-and-known-issues"></a><span data-ttu-id="c4ad4-250">Feedback och kända problem</span><span class="sxs-lookup"><span data-stu-id="c4ad4-250">Feedback and known issues</span></span>
<span data-ttu-id="c4ad4-251">För närvarande kan stöds visa Spark utdata direkt inte.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-251">Currently, viewing Spark outputs directly is not supported.</span></span>

<span data-ttu-id="c4ad4-252">Om du har förslag eller feedback eller om du stöter på problem när du använder det här verktyget gärna skicka ett e-postmeddelande på hdivstool@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="c4ad4-252">If you have any suggestions or feedback, or if you encounter any problems when using this tool, feel free to send us an email at hdivstool@microsoft.com.</span></span>

## <span data-ttu-id="c4ad4-253"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="c4ad4-253"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="c4ad4-254">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="c4ad4-254">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="c4ad4-255">Scenarier</span><span class="sxs-lookup"><span data-stu-id="c4ad4-255">Scenarios</span></span>
* [<span data-ttu-id="c4ad4-256">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="c4ad4-256">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="c4ad4-257">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="c4ad4-257">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="c4ad4-258">Spark med Machine Learning: Använda Spark i HDInsight för att förutsäga resultatet av en livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="c4ad4-258">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="c4ad4-259">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="c4ad4-259">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="c4ad4-260">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="c4ad4-260">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a><span data-ttu-id="c4ad4-261">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="c4ad4-261">Creating and running applications</span></span>
* [<span data-ttu-id="c4ad4-262">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="c4ad4-262">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="c4ad4-263">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="c4ad4-263">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="c4ad4-264">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="c4ad4-264">Tools and extensions</span></span>
* [<span data-ttu-id="c4ad4-265">Använda Azure Toolkit för IntelliJ för att skapa och skicka Spark Scala-program</span><span class="sxs-lookup"><span data-stu-id="c4ad4-265">Use Azure Toolkit for IntelliJ to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="c4ad4-266">Använda Azure Toolkit för IntelliJ för att felsöka Spark-program via fjärranslutning via VPN</span><span class="sxs-lookup"><span data-stu-id="c4ad4-266">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="c4ad4-267">Använda Azure Toolkit för IntelliJ för att felsöka Spark-program via fjärranslutning via SSH</span><span class="sxs-lookup"><span data-stu-id="c4ad4-267">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="c4ad4-268">Använda HDInsight Tools för IntelliJ med Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="c4ad4-268">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="c4ad4-269">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="c4ad4-269">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="c4ad4-270">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="c4ad4-270">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="c4ad4-271">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="c4ad4-271">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="c4ad4-272">Installera Jupyter på datorn och ansluta till ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="c4ad4-272">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a><span data-ttu-id="c4ad4-273">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="c4ad4-273">Managing resources</span></span>
* [<span data-ttu-id="c4ad4-274">Hantera resurser för Apache Spark-klustret i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="c4ad4-274">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="c4ad4-275">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="c4ad4-275">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

